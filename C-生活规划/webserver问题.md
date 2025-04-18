
## 项目中的问题

1. 为什么做这个项目？
	Webserver 能够串联绝大部分的所学的知识，语言+操作系统(各种IO的调用)及其封装+计算机网络+数据库(注册中心的数据库语句、负载均衡)，这个项目包含了面试的基础知识
	Webserver可以做很多扩展 补充...
2. 介绍一下你的项目
	此项目我在学习计算机⽹络和Linux socket编程过程中开发的轻量级Web服务器，应用层实现了一个简单的HTTP服务器，利用多路IO复用，可以同时监听多个请求，使用线程池处理请求，使用的网络模式是主从Reactor模式，主Reactor负责监听新的连接，从Reactor负责这个连接的业务处理和响应，主Reactor将连接请求对象放入线程池中工作队列。睡眠在请求队列上的工作线程被唤醒进行处理。使用状态机解析HTTP请求报文，实现同步/异步日志系统,记录服务器运行状态，还用定时器处理非活动连接，并对系统进行了压力测试在10500个并发的客户端可以保持40000左右的QPS。
3. 项目的难点在哪里，怎么去做优化
	1. 第一个难点是对于服务器⽹络框架、⽇志系统、存储引擎等⼀些基本系统的搭建，这部分的难点主要就是技术的理解和选型，比如采用什么网络模式，线程池工作方式模式等等。
	2. 第二个难点是整个项目构建过程，工作流程，以及网络资源的传输过程的融汇贯通，把各个过程联想起来。
	3. 第三点也是最重要就是找出性能瓶颈所在，然后用自己所学的知识想方法去优化它。
4. 你的webserver，跟别人有什么不同，你自己做了什么改进
	1. 我对原装的socket那一系列原生api做了封装，原来那一套都是传入裸指针，不安全，而且传参数像sockaddr_in需要设置各种参数，对这些参数都做了封装，用了一些RAII编程思想(<font color=#ff0000>可能问什么是RAII</font>)，这些socket函数像bind、listen、accept、connect以及要创建TCP、UDP那些socket Steam不利于理解直接换成枚举类显示TCP还是UDP这样进行了封装，达到了一个高内聚低耦合，做到了见名知意，使用更方便。
	2. 日志写入函数与工作线程**串行执行**，由于涉及**I/O操作**，同步日志会阻塞整个处理流程，服务器所能处理的并发能力将有所下降，尤其是在访问峰值时，写日志可能会成为系统的瓶颈。首先异步日志的锁开销就可以优化一下，这里的异步日志的方式是设置一个阻塞队列，然后在要写入日志时，将这些日志写入到阻塞队列，然后单独建立一个线程，这里执行一个任务去从里面不断的取，没日志就阻塞，然后写日志是由线程池中的线程负责，我这里是8个，所以这是**读多写少的生产者消费者模型**，所以第一个优化可以把互斥锁改成读**写锁**，然后我们读线程和写线程的读写速度也不一样，读线程是写内存，写线程的flush要分两步，写到内核缓冲区再持久化到磁盘,所以**写速度要显著慢于读**,写线程是不是会阻塞读线程，而读线程是**很活跃**的,锁的竞争会影响效率,写线程一直在试图获取阻塞队列的锁,所以我们可以采用工程上**双缓冲区**：一块缓冲区专门用于读线程，只用互斥锁隔离读线程。读线程不会无时不刻得去获取锁，所以锁的竞用开销会降低一点，也不会被写的持久化阻塞，一块缓冲区队列专门用于写，写线程只有一个，所以甚至不用加锁，读线程很快会写满，就暂时阻塞。等到写线程写完再用**右值引用交换**两个缓冲区，这里的同步用信号量完成。后面还能再优化，主要是在两个阻塞队列大小相仿的情况下，读线程很快会写满阻塞，很影响效率，随意读写缓冲区可以各自分为forward和back两个阻塞队列，轻改一下交换逻辑，可以一定程度上解决锁的开销，这是更大的内存开销，经典的时间换空间。
	3. 当请求放到任务队列时，这个线程池的线程都会执行一个循环任务，就是从这个任务队列去取请求，这里请求一定是远远大于线程数目，所以这些线程池中的线程取请求过程中都会竞争加锁，这块可以优化，可以采用**Actor的设计模式**，每个线程池中的线程有自己的队列，主线程跟线程池中的线程通过队列通信，这样就可以**消除共享状态**，因为它每次只能处理一条消息，所以actor内部可以安全的处理状态，而**不用考虑锁机制，没有竞争开销了**，主线程将请求轮询的放到线程的邮箱中，保持负载均衡。
	4. 关于我线程池要怎么开线程开多少线程，在工程上是cpu密集型选择cpu核心数，而io密集型选择2`*`cpu核心数+2，所以我是以(io等待时间+cpu等待时间)`*`核心数/cpu运算 公式来选择线程数量的，然后这里我用了**线程绑定CPU**的技术，绑定之后会更好利用缓存和减少内存冲突，底层是mesi协议实现的缓存一致性，所以线程换核开销可以减小。
	5. 在网络模型上更改，网络模型把epoll换成`io uring`这种异步(但是会被挖坑，因为简历没写，会问为什么`io uring`比epoll优秀)
	6. 在http解析这块，客户端请求资源时，服务端文件系统中寻找对应的文件后，将这个**映射文件内容到内存**，后续虚拟内存系统来访问文件，文件的读取和写入就像访问普通内存一样**高效**。
	7. 采用小根堆的定时器处理空闲连接(这里主要对比升序链表的定时器和时间论)
	8. 因为http协议，我们这个天然处理粘包问题，这个就不能当优化说了。
	9. 采用星球的lru算法和内存池(**同样简历没写，面试官可能看出来在编**)
	10. ftp协议也没做(这里优化可以说零拷⻉函数 sendFile() 来发送，避免拷⻉数据到⽤户态)
5. 你有遇到过什么性能瓶颈吗,是如何解决的？ 同4

6. 介绍你的服务器使用的并发模型 
	我采用的是主从Reactor多线程，主Reactor只负责acceptor的连接建立，已经连接的套接字上的I/O事件交给sub-reactor负责分发。
	主反应堆线程一直感知连接建立的事件，如果有连接建立成功，主反应堆线程通过accept方法获取已经连接套接字，



### TCP聊天室

- **项目描述**：基于斯坦福大学CS144课程。从头开始实现了一个TCP协议，涵盖了多个可靠性传输模块，确保了数据的完整可靠传输。在此过程中，实现了TCP的核心功能。
- **主要工作**：
    1. 字节流管理：实现了字节流管理和流量控制机制，保障数据的读写功能。
    2. 流重组：解决了乱序、丢失、重叠等问题，确保数据恢复到正确的顺序。
    3. TCP连接管理：实现了完整的TCP连接管理，包括三次握手和四次挥手过程。
    4. TCP数据传输：通过超时重传机制和分段发送，保证了数据的可靠传输。
    5. 子网匹配和路由转发：实现了IP路由和ARP协议，以便在复杂网络环境中正确地路由和转发数据。
- **个人收获**：深入了解TCP协议各个方面，掌握了网络通信的原理和机制，以及网络编程和系统级编程的技能。

## 几个比较难的问题或者有争议？

1. 那你为什么选择 reactor 而不是 proactor？(有争议)
	这里的reactor对比，网上是说linux proactor的异步不完整是模拟的，但实际原因是reactor  IO读写是多线程并行的,而模拟preactor模式中，IO读写都是在主线程中串行完成的，是这个原因吧
2. 如果你的服务器换成 HTTPS 处理或者HTTP2.0，怎么做？
	不知道答案，是重新写http解析还是改前端cgi，这一块不是太明白



## 场景题
1. 你的服务器测试环境没问题，生产环境每个月崩溃一次,bug很难复现,你的排查思路?
	1. 时间相关，代码里面访问时间的逻辑是不是有某些，比如段错误(Segmentation Fault)
	2. GDB调式，到了那个时间查看调用栈找出错误
	3. 增大压测，增加问题出现的概率，方便找出问题
	4. 在测试环境和生产环境代码review一下查看有没有隐藏的内存泄漏或者bug
	5. 查看日志
	6. 查看性能检测工具(cpu、线程状态、用linux的lsof、netstat等命令查看连接状态)
	7. 版本控制进行回滚查异 
2. 你的服务器出现大量的time_wait你觉得可能是什么原因？ 有什么解决方案
	- 原因
		1. time_wait是主动关闭的一方的四挥手的最后一个状态，那么可能是短时间内大量连接关闭（大量连接在短时间内创建并关闭）或者没有设置keep-alive
		2. 服务器端主动关闭连接
		3. 发起了大量的空闲连接，后面又多个释放
		4. 代码的问题，read、write返回异常，发送了rst，导致大量释放连接
	- 方案
		1. 设置空闲连接超时
		2. 使用连接池
		3. 换成http/2.0,它支持支持多路复用
		4. 设置`tcp_tw_recycle` 和 `tcp_tw_reuse`参数








## 其他问题

看你简历上写的擅长Wireshark，说一下你使用它的场景 


我面试复盘一下，面试官问我线程池数目，我说我的数目设置成了cpu内核数，是8个，面试官问我线程有没有绑定cpu,我说这是什么东西？他说每个线程在自己的核心中，有隔离性，不会争夺资源，我说没听过这个技术，这是什么东西？

	面试回答示例：
	“当时我没有使用线程绑定CPU的技术，也没有具体设置CPU亲和性。我设定线程池数目为CPU内核数的原因是为了充分利用多核处理器的并行能力。但我了解CPU亲和性是一种可以提高线程性能的方法，通过将线程绑定到特定的CPU核上，可以减少缓存失效和上下文切换的开销，从而提高系统的整体性能。在以后的开发中，我会考虑这方面的优化来进一步提升系统效率。”


然后问我get和post的区别？
大部分都说了，然后我说GET请求在URL中传送的参数是有长度限制的，post没有，然后面试官问我发送请求不从浏览器，自己写程序，发送的get请求可以突破这个长度限制的吗？
我说不知道

“GET请求的长度限制主要是由浏览器和服务器的实现决定的，而不是HTTP协议本身的限制。当在浏览器中发送GET请求时，URL长度通常限制在2048字符左右。然而，如果我们自己编写程序来发送GET请求，可以突破浏览器的限制，但最终还是受限于服务器的处理能力。某些服务器可能对URL长度有特定的限制，因此超长的GET请求可能会被服务器拒绝处理。通常，为了传递大量数据或复杂的数据结构，我们会使用POST请求，这样可以避免长度限制的问题，并且更适合传递敏感信息。”

然后面试官怎么避免死锁，我说换成try_lock,lock加时间，扩大锁的粒度，锁加信号量，面试官问还有吗？	
	面试官提到的避免死锁的问题，除了你提到的方法外，还有几种常见的策略和技术可以用于避免或减少死锁的可能性。以下是更详细的补充：
	
	1. 避免嵌套锁
	避免在持有一个锁的情况下再去获取另一个锁。嵌套锁（也称递归锁）容易导致死锁的发生。
	
	2. 使用锁的顺序
	确保所有线程都以相同的顺序获取锁。如果所有线程都以相同的顺序获取锁，就不会发生循环等待，从而避免死锁。
	
	3. 使用无锁数据结构
	在可能的情况下，使用无锁数据结构（如无锁队列、无锁栈等）。无锁数据结构通过使用原子操作来保证线程安全，避免了锁的使用，从而也避免了死锁。
	
	4. 锁的层次化
	将锁划分为不同的层次，并规定线程只能按层次顺序从低到高获取锁。这种方法类似于锁的顺序，但更适用于复杂系统中的多级锁定。
	
	5. 死锁检测
	实现一个监控系统，定期检查系统中的锁定状态，检测是否存在死锁。一旦检测到死锁，可以采取措施如回滚操作、释放锁等。
	
	6. 资源有序分配
将资源编号，并规定线程必须按资源编号顺序请求资源。通过资源有序分配，可以有效避免循环等待，从而避免死锁。









