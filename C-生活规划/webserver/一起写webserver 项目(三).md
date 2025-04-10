---
title: 一起写webserver 项目(三)
tags: ['项目']
categories:
  - 一起做项目
---

## 数据库连接池的封装


### 什么是池

- **池的概念**
    - “浪费”服务器的硬件**资源**，以换取其运行效率。
    - 池是一组资源的集合，这组资源在服务器启动之初就被完全创建好并初始化，这称为静态资源分配。
    - 直接从池中取得所需资源比动态分配资源的速度要快得多，因为分配系统资源的系统调用都是很耗时的


- **为什么需要数据库连接池**？
    - 每个逻辑单元可能都需要频繁地访问本地的某个数据库。
    - 连接池是服务器预先和数据库程序建立的一组连接的集合。当某个逻辑单元需要访问数据库时，它可以直接从连接池中取得一个连接的实体并使用之。待完成数据库的访问之后，逻辑单元再将该连接返还给连接池


### 单例模式

单例模式的好处就不再赘述
```c++
class connection_pool{
public:
    static connection_pool* Getinstance() {
        static connection_pool connPool;
        return &connPool;
    }
 
private:
    connection_pool();
    ~connection_pool();
};
```

再把构造函数和析构函数补充完整，里面初始化的成员不要着急，后面会写；
```c++
connection_pool::connection_pool() {
    m_FreeConn = 0;
    m_CurConn = 0;
}
connection_pool::~connection_pool() {
    DestroyPool();
}
```


### 初始化

- 整体思路
	- 这里初始化的操作就是建立起`max_con`个连接，这些连接的类型是`MYSQL *`,将这个 `max_con`个连接存到一个数据结构中，方便存取，这里采用的是链表`List`。
	- 这里数据库的资源使用信号量进行同步，所以信号量的初始化为数据库的连接总数。
	```c++
	public:
	  string m_url;
	  string m_user;
	  string m_port;
	  string m_password;
	  string m_databaseName;
	  int m_close_log{};
	private:
	  int m_free_con; // 空闲连接数
	  int m_cur_con; // 正在使用的连接数
	  int m_max_con; // 最大连接数
	  list<MYSQL *>connList;
	  locker lock;
	  sem reserve{};
	  connection_pool();
	  ~connection_pool();
	```

	```c++
	void connection_pool::init(string url, int port, string user, string password, string dbName, int close_log,int max_con) {
	
		// ....
	  for(int i=0;i<max_con;i++){
	    MYSQL *con= nullptr;
	
	    con=mysql_init(con);
	
	    if(con== nullptr){
	      LOG_ERROR("MySQL Error");
	      exit(1);
	    }
	
	    con=mysql_real_connect(con,m_url.c_str(),m_user.c_str(),m_password.c_str(),m_databaseName.c_str(),port, nullptr,0);
	    if(con== nullptr){
	      LOG_ERROR("MySQL Error");
	      exit(1);
	    }
	    connList.push_back(con);
	    ++m_free_con;
	  }
	  m_max_con=m_free_con;
	  reserve = sem(m_free_con); // 信号量的初始化
	}
	```

### 数据库的访问

这里的获取和释放数据库连接也是类**生产者-消费者模型**，这里用的是信号量+同步锁，这里的无论是获取，释放还是销毁，我们都要用mutex来保证线程同步，同时，获取连接前需要wait()阻塞等到临界区有资源，释放连接后需要post()来提醒其他线程临界区有新资源

```c++
MYSQL *connection_pool::getConnection() { // 消费者

  MYSQL *con= nullptr;
  if(connList.empty()) return nullptr;
  reserve.wait();
  lock.lock();

  con=connList.front();
  connList.pop_front();
  --m_free_con;
  ++m_cur_con;
  lock.unlock();
  return con;
}
bool connection_pool::releaseConnection(MYSQL *con) { // 生产者

  if(con== nullptr) return false;

  lock.lock();
  connList.push_back(con);
  ++m_free_con;
  --m_cur_con;
  lock.unlock();
  reserve.post();
  return true;
}
```


### RAII 类

这里使用了RAII实现**资源池的自动回收机制**

- `ResourcePool`为资源池类,可以创建指定数量的资源,并提供获取和释放资源的接口。
- `ResourceWrapper`为资源包装类,用于获取资源,并在对象销毁时自动释放资源。
- `Resource`为资源类,用于模拟资源,通过id来标识,其构造函数和析构函数分别用于获取和释放资源。

这里的`connection_pool`就是`ResourcePool`，而`MYSQL *`是`Resource`,这里可以创建了一个资源包装类`ResourceWrapper`，可以通过**构造函数**调用数据库连接函数,通过**析构函数**调用销毁这个数据的连接，这样就实现了资源的获取与释放与类的实例的生命周期绑定

```c++
class connection_poolRAII{
public:
  connection_poolRAII(MYSQL **SQL,connection_pool *connPool) {   
  *SQL=connPool->getConnection();  // 构造就获取一个连接
  connRAII=*SQL;  
  poolRAII=connPool;  
}
  ~connection_poolRAII(){  // 析构就销毁这个连接
  poolRAII->releaseConnection(connRAII);  
}
private:
  MYSQL *connRAII;
  connection_pool *poolRAII;
};
```



## 定时器的封装


### 为什么需要定时器

- 为什么需要定时器？
    - 定时器是网路程序要处理的第三类事件，这里主要用来处理非活动连接，一个连接长时间没有响应，为了节省有限的系统资源，就要关闭这个这个连接，资源分给其他客户端，保证服务器的运行效率。
    - 定时器能在预期的时间点发生，且不影响服务器的主要逻辑。
      

下面是我画的一个框架图

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240430111306.png " alt="image.png" style="zoom:60%;" />


### 定时器类

我们先定义一个定时器类，定时器类里我们封装连接资源，定时事件指针，以及超时时间。

```c++
/*util_timer前置声明，因为client_data使用了util_timer类*/
class util_timer;
 
/*用户数据结构体(连接资源)*/
struct client_data{
    sockaddr_in address;//客户端的socket地址
    int sockfd;         //socket文件描述符
    util_timer *timer;  //定时器
};
 
/*定时器类*/
class util_timer{
public:
    util_timer():prev(nullptr), next(nullptr){}
 
public:
    time_t expire;                  //超时时间
    /*回调函数声明：声明一个返回值为空的函数指针cb_func,传入clent_data指针作为函数参数*/
    void (*cb_func)(client_data *); //回调函数指针
    client_data *user_data;         //连接资源
    
    util_timer *prev;               //前向定时器
    util_timer *next;               //后继定时器
};
```


### 上升链表类

```c++
class sort_timer_lst{
public:
  sort_timer_lst();
  ~sort_timer_lst();
  void add_timer(util_timer *timer);
  void del_timer(util_timer *timer);
  void adjust_timer(util_timer *timer);
  void tick();
private:
  void add_timer(util_timer *timer,util_timer* lst_head);
  util_timer *head;
  util_timer *tail;
};
```

下面的代码都是参考《Linux高性能服务器编程》
```c++
sort_timer_lst::sort_timer_lst() {
  head= nullptr;
  tail= nullptr;
}
sort_timer_lst::~sort_timer_lst() {
  util_timer *tmp=head;
  while(tmp){
    head=tmp->next;
    delete tmp;
    tmp=head;
  }
}
void sort_timer_lst::add_timer(util_timer *timer) {
  if (timer== nullptr) return;
  if(head== nullptr&&tail== nullptr){
    head=tail=timer;
    return;
  }
  // 插在头部
  if(head!= nullptr&&timer->expire<head->expire){
    timer->next=head;
    head->prev=timer;
    timer->prev= nullptr;
    head=timer;
    return;
  }

  // 插在尾部
  if(tail!= nullptr&&timer->expire>tail->expire){
    tail->next=timer;
    timer->prev=tail;
    timer->next= nullptr;
    tail=timer;
    return;
  }

  // 插在中间，只能搜索了
  add_timer(timer,head);

}
void sort_timer_lst::add_timer(util_timer *timer, util_timer *lst_head) {
  // 从头部开始查找第一个大于超时大于timer位置进行插入
  util_timer *pre=lst_head;
  util_timer *cur=lst_head->next;
  while(cur){
    if(cur->expire>timer->expire){
      timer->prev=pre;
      timer->next=cur;
      pre->next=timer;
      cur->prev=timer;
      break;
    }
    cur=cur->next;
    pre=pre->next;
  }
  if(cur== nullptr){
    pre->next=timer;
    timer->prev=pre;
    timer->next= nullptr;
    tail=timer;
  }
}
void sort_timer_lst::del_timer(util_timer *timer) {
  if (timer== nullptr) return;
  if((timer==head)&&(timer==tail)){
    delete timer;
    head= nullptr;
    tail= nullptr;
    return;
  }
  if(timer==head){
    auto tmp=head;
    head=head->next;
    head->prev= nullptr;
    delete timer;
    return;
  }

  if(timer==tail){
    auto tmp=tail;
    tail=tail->prev;
    tail->next= nullptr;
    delete timer;
    return;
  }
  timer->prev->next=timer->next;
  timer->next->prev=timer->prev;
  delete timer;
}
void sort_timer_lst::adjust_timer(util_timer *timer) {
  // 这个只负责延长时间
  if (timer==nullptr) return;
  auto tmp=timer->next;
  //  延长后比最小的还小
  if(tmp== nullptr||timer->expire<tmp->expire){
    return;
  }
  if(timer==head){
    head=head->next;
    head->prev= nullptr;
    timer->next= nullptr;
    add_timer(timer,head);
  }

  else{ // 先删除后插入
    timer->prev->next = timer->next;
    timer->next->prev = timer->prev;
    add_timer(timer, timer->next);
  }

}
```

接下来是一个tick函数，就是从头节点开始查找直到一个未到期的定时器，之前的都要触发该定时器的回调函数并从链表定时器中删除，而该回调函数所作的事情就是将这个sockfd从epoll等待队列中删除并关闭这个文件描述符。

```c++
void sort_timer_lst::tick() { // 就是从头节点开始查找直到一个未到期的定时器，之前的都要触发该定时器的回调函数
  if(head== nullptr) return;

  time_t cur= time(nullptr);
  util_timer *tmp=head;
  while(tmp){

    if(tmp->expire>cur){
      break;
    }

    tmp->cb_func(tmp->user_data);

    head=tmp->next;
    if(head){
      head->prev= nullptr;
    }
    delete tmp;
    tmp=head;
  }
}
```


```c++
void cb_func(client_data *uer_data){

  epoll_ctl(Utils::u_epollfd,EPOLL_CTL_DEL,uer_data->sockfd, nullptr);
  assert(uer_data);
  close(uer_data->sockfd);
  // 后面补充
  http_conn::m_user_count--;
}
```



### 抽象工具类

定时器设计基本完成了，这个工具类就是用来合理使用它。

这个工具类的作用主要是通知主线程一些事件，这里的通知方式是采用**信号**，这里采用**统一事件源**，信号出现的时候不是立即去执行信号处理函数(或者真正的信号处理逻辑函数)，而是通过管道发送给主进程信号的编号，主循环收到信号就记录下来，等其他IO事件完成之后，就调用tick()处理非活动连接。

下面使用这个工具类，具体可以上面的框架图
1. 主线程初始化这个Utils类
2. 主线程调用addfd将pipe管道与epollfd相关联
3. 主线程调用addsig将目标信号(SIGALRM SIGTERM）加入监听的信号集
4. 主线程循环eventLoop()开始，服务器开始运行
5. 多个客户连接长久未响应
6. 经过TIMESLOT,触发信号，主循环收到信号
7. 主循环调用tick()处理非活动连接

```c++
class Utils{
public:
  Utils() = default;
  ~Utils() = default;
  void init(int time_slot);
  int setnonblocking(int fd);
  void addfd(int epollfd,int fd,bool one_shot,int TRIGMode);
  void addsig(int sig,void(handler)(int),bool restart= true);
  static void sig_handler(int sig);
  void show_errno(int connfd,const char *info);
  void time_handler();

public:
  static int *u_pipefd;
  static int u_epollfd;
  int m_TIMESLOT{};
  sort_timer_lst m_time_lst;
};
```


```c++
int* Utils::u_pipefd=nullptr;
int Utils::u_epollfd=0;

void Utils::init(int time_slot) {
  m_TIMESLOT=time_slot;
}
int Utils::setnonblocking(int fd) {
  int old_op=fcntl(fd,F_GETFL);
  int new_op=old_op|O_NONBLOCK;
  fcntl(fd,F_SETFL,new_op);
  return old_op;
}
void Utils::addfd(int epollfd, int fd, bool one_shot, int TRIGMode) {
  epoll_event event{};
  event.data.fd=fd;
  if(one_shot)
    event.events|=EPOLLONESHOT;
  if(TRIGMode==1)
    event.events=EPOLLIN|EPOLLET|EPOLLHUP;
  else
    event.events=EPOLLIN|EPOLLHUP;
  epoll_ctl(epollfd,EPOLL_CTL_ADD,fd,&event);
  setnonblocking(fd);
}
void Utils::addsig(int sig, void (handler)(int), bool restart) {
  struct sigaction sa{};
  memset(&sa,'\0', sizeof(sa));
  sa.sa_handler=handler;
  if(restart)
    sa.sa_flags|=SA_RESTART;
  sigfillset(&sa.sa_mask);
  assert(sigaction(sig, &sa, nullptr) != -1);
}
void Utils::sig_handler(int sig) {
  int olderrno=errno;
  int msg=sig;
  send(u_pipefd[1],(char *)&msg,1,0);
  errno=olderrno;
}
void Utils::show_errno(int connfd, const char *info) {
  send(connfd,info, strlen(info), 0);
  close(connfd);
}
void Utils::time_handler() {
  m_time_lst.tick();
  alarm(m_TIMESLOT);
}
```



## 半同步/半反应堆线程池

### 概述

先来看下半同步半反应堆的框架吧

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240430112445.png" alt="image.png" style="zoom:60%;" />


几个问题？

1. 请求队列和线程池的关系？ --> 请求队列就是线程池中常见的工作队列，放在I/O处理单元和逻辑单元之间是**各单元之间的通信方式的抽象**，请求请求存放的是TCP连接
2. 哪些地方需要锁和信号量？ --> 这里用锁和信号量完成生产者消费者模式，生产者就是往工作队列放任务，消费者就是线程池的工作线程处理任务
3. 在线程池中数据库连接池完成什么作用 --> 主要用于reactor，因为在reactor模式下数据的读写操作由工作线程完成，这里用数据库的RAII类获得一个mysql连接，用于业务逻辑的增删改查。
4. 请求队列放的是什么？ --> 按线程池角度就是任务，但是按服务器编程框架来说，这里放的是客户端的连接，这个就是后面的`http`类，考虑代码的复用性，这里用了模板。


```c++
template<class T>
class thread_pool{
public:
  thread_pool(int actor_mode,connection_pool* connPool,int thread_num=8,int max_quest=10000);
  ~thread_pool();
  bool append(T *request,int state);
  bool append_p(T *request);
private:
  static void* worker(void *arg);
  void run();
private:
  int m_thread_num;
  int m_max_quest;
  pthread_t* m_threads;
  connection_pool* m_connPool{};
  int m_actor_mode;
  list<T *>m_work_queue; // 请求队列 
  locker m_queue_locker;
  sem m_queue_stat;
};
```

### 初始化

初始化操作就是创建thread_num个线程，这里用了线程分离，不会担心线程的资源回收问题，也避免了僵尸线程。

```c++
template<class T>
thread_pool<T>::thread_pool(int actor_mode, connection_pool *connPool, int thread_num, int max_quest):m_thread_num(thread_num),m_max_quest(max_quest),m_threads(nullptr),m_connPool(connPool),m_actor_mode(actor_mode)
{
  if(thread_num<=0||max_quest<=0) {
    throw std::exception();
  }
  m_threads=new pthread_t[m_thread_num];

  for(int i=0;i<thread_num;++i){
    if(pthread_create(m_threads+i, nullptr,worker,this)!=0)
    {
      delete []m_threads;
      throw std::exception();
    }

    if(pthread_detach(m_threads[i]))
    {
      delete []m_threads;
      throw std::exception();
    }

  }
}
```

### 添加事件

就是简单的链表添加操作。但注意，添加完后就可以唤醒线程来取事件处理进行处理。
```c++
template<class T>
bool thread_pool<T>::append(T *request, int state) {
  m_queue_locker.lock();
  if(!request||m_work_queue.size()>=m_max_quest) {
    m_queue_locker.unlock();
    return false;
  }
  request->m_state=state;
  m_work_queue.push_back(request);
  m_queue_locker.unlock();
  m_queue_stat.post();
  return true;
}

template<class T>
bool thread_pool<T>::append_p(T *request) {

  m_queue_locker.lock();
  if(!request||m_work_queue.size()>=m_max_quest) {
    m_queue_locker.unlock();
    return false;
  }
  m_work_queue.push_back(request);
  m_queue_locker.unlock();
  m_queue_stat.post();
  return true;
}
```

### worker和run

woker函数就是创建线程的工作函数，这里用了类的静态成员函数作为工作函数解决了work函数不能有参数的问题，因为每个类的**非静态的类成员函数都有一个隐藏参数this指针。** run函数就是在循环中处理逻辑。

```c++
template<class T>
void *thread_pool<T>::worker(void *arg) {
  auto *pool=(thread_pool*)arg;
  pool->run();
  return pool;
}
```


```c++
template<class T>
void thread_pool<T>::run() {
  while (true){
    m_queue_stat.wait();
    m_queue_locker.lock();
    if(m_work_queue.empty()){
      m_queue_locker.unlock();
      continue;
    }

    T* request=m_work_queue.front();
    m_work_queue.pop_front();
    m_queue_locker.unlock();
    if(!request) continue;
    // 需要补充
    if(m_actor_mode==1){ // reator模式
      if(request->m_state==0){ // 读模式
        if(request->read_once()){
          request->improv=1;
          connection_poolRAII mysqlconn(&request->mysql,m_connPool);
          request->process();
        }else{
          request->improv=1;
          request->timer_flag=1;
        }
      }
      else{ // 写模式
        if(request->write()){
          request->improv=1;
        }else{
          request->improv=1;
          request->timer_flag=1;
        }
      }
    }
    else { // Proactor模式
      connection_poolRAII mysqlconn(&request->mysql,m_connPool);
      request->process();
    }
  }
}
```


