---
title: 一起写webserver 项目(二)
tags: ['项目']
categories:
  - 一起做项目
---

## 日志和同步原语的封装

> 写一个项目，一开始不知道从哪里开始，我的经验是大致看一下主函数，了解一下有哪些模块？然后从简单的模块开始逐个看，看完了记住流程思路，我们就可以复现，然后这样虽然阅读起来比整体一行一行看简单，但是各个模块之间相互穿插，单个模块看，理解不了它们之间的关系，所以建议所有模块看完后，再重新看一遍，把整个流程串通，这样整个项目就非常清晰了。

其实我们也可以看作者的Readme的整体框架图

<img src="https://camo.githubusercontent.com/326c456073716c6a81d925154df43a7787cf4088b794590c76a0f122274e7ef4/687474703a2f2f7777312e73696e61696d672e636e2f6c617267652f303035544a3263376c79316765306a3161747135686a33306736306c6d3077342e6a7067" alt="image.png" style="zoom:80%;" />

### Locker类

这里的locker的封装是参考《Linux高性能服务器编程》，用了RAII的思想，即将**资源的获取和释放绑定在对象的生命周期**中。比较简单就不用怎么叙述了，其实这里完全可以c++的同步原语语法，感兴趣的读者可以自行尝试

- 信号量
	```c++
	class sem{
	public:
	  sem(){
	    if(sem_init(&m_sem,0,0)!=0){
	      throw std::exception();
	    }
	  }
	  explicit sem(int num){
	    if(sem_init(&m_sem,0,num)!=0){
	      throw std::exception();
	    }
	  }
	  ~sem(){
	    sem_destroy(&m_sem);
	  }
	
	  bool wait(){
	    return sem_wait(&m_sem)==0;
	  }
	  bool post(){
	    return sem_post(&m_sem)==0;
	  }
	
	private:
	  sem_t m_sem{};
	};
	```

- 同步锁
	```c++
	class locker{
	public:
	  locker(){
	    if(pthread_mutex_init(&mutex,nullptr)!=0){
	      throw std::exception();
	    }
	  }
	  ~locker(){
	    pthread_mutex_destroy(&mutex);
	  }
	
	  bool lock(){
	    return pthread_mutex_lock(&mutex)==0;
	  }
	
	  bool unlock(){
	    return pthread_mutex_unlock(&mutex)==0;
	  }
	
	  bool trylock(){
	    return pthread_mutex_trylock(&mutex)==0;
	  }
	
	  pthread_mutex_t *get(){
	    return &mutex;
	  }
	
	private:
	  pthread_mutex_t mutex{};
	};
	```

- 条件变量
	```c++
	class cond{
	public:
	  cond(){
	    if(pthread_cond_init(&m_cond, nullptr)!=0){
	      throw std::exception();
	    }
	  }
	  ~cond(){
	    pthread_cond_destroy(&m_cond);
	  };
	
	  bool wait(pthread_mutex_t *mutex){
	    return pthread_cond_wait(&m_cond,mutex)==0;
	  }
	
	  bool timewait(pthread_mutex_t *mutex,struct timespec t){
	
	    return pthread_cond_timedwait(&m_cond,mutex,&t)==0;
	  }
	
	  bool signal(){
	    return pthread_cond_signal(&m_cond)==0;
	  }
	  bool broadcast(){
	    return pthread_cond_broadcast(&m_cond)==0;
	  }
	
	private:
	  pthread_cond_t m_cond{};
	};
	```

## LOG类

LOG类就是项目中常见的日志系统，由服务器自动创建，并记录运行状态，错误信息，访问数据的文件。
从框架图我们可以看出，这里的日志分为**同步日志**和**异步日志**。

**同步日志**：日志写入函数与工作线程**串行执行**，由于涉及**I/O操作**，同步日志会阻塞整个处理流程，服务器所能处理的并发能力将有所下降，尤其是在访问峰值时，写日志可能会成为系统的瓶颈。

**异步日志**：将工作线程所写的日志内容先存入**阻塞队列**，专门的一个线程与主线程并行执行的关系，这个线程从阻塞队列中取出内容，写入日志，从而不影响主线程。

其实异步日志是一个典型的**生产者-消费者模型**。其中工作线程时生产，写线程是消费者。生产者-消费者模型的临界区(缓冲区)是什么呢？在这个日志系统中，这个临界区就是一个**阻塞队列**

### 循环队列
- 阻塞队列实现

	```c++
	template<class T>
	class block_queue{
	public:
	  // 构造 析构 clear  full empty size() maxsize() back front push pop pop()
	
	  explicit block_queue(int maxsize=1000){
	    if(maxsize<=0){
	      exit(-1);
	    }
	    m_size=0;
	    m_maxsize=maxsize;
	    m_array=new T[maxsize];
	    m_front=-1;
	    m_back=-1;
	  }
	
	  ~block_queue(){
	    m_lock.lock();
	    delete[] m_array;
	    m_lock.unlock();
	  }
	
	  void clear(){
	    m_lock.lock();
	    m_size=0;
	    m_front=-1;
	    m_back=-1;
	    m_lock.unlock();
	  }
	
	  bool full(){
	    m_lock.lock();
	
	    if(m_size==m_maxsize) {
	      m_lock.unlock();
	      return true;
	    }
	    m_lock.unlock();
	    return false;
	  }
	
	  bool empty(){
	    m_lock.lock();
	
	    if(m_size==0){
	      m_lock.unlock();
	      return true;
	    }
	    m_lock.unlock();
	    return false;
	  }
	
	  bool front(T &item){
	    m_lock.lock();
	    if(m_size==0){
	      m_lock.unlock();
	      return false;
	    }
	    m_array[m_front]=item;
	    m_lock.unlock();
	    return true;
	  }
	  bool back(T &item){
	    m_lock.lock();
	    if(m_size==0){
	      m_lock.unlock();
	      return false;
	    }
	    m_array[m_back]=item;
	    m_lock.unlock();
	    return true;
	  }
	
	  int size(){
	    m_lock.lock();
	    int tmp=0;
	    tmp=m_size;
	    m_lock.unlock();
	    return tmp;
	  }
	
	  int maxsize(){
	    m_lock.lock();
	    int tmp=0;
	    tmp=m_maxsize;
	    m_lock.unlock();
	    return tmp;
	  }
	
	  bool push(const T&item){
	    m_lock.lock();
	    if(m_size==m_maxsize){ //队列满了
	      m_cond.broadcast();
	      m_lock.unlock();
	      return false;
	    }
	
	    m_back=(m_back+1)%m_maxsize;
	    m_array[m_back]=item;
	
	    m_size++;
	    m_cond.broadcast();
	    m_lock.unlock();
	    return true;
	  }
	
	  bool pop(T &item){
	    m_lock.lock();
	    while (m_size<=0){ // 防止虚假唤醒
	      if(!m_cond.wait(m_lock.get())){
	        m_lock.unlock();
	        return false;
	      }
	    }
	    m_front=(m_front+1)%m_maxsize;
	    item=m_array[m_front];
	
	    m_size--;
	    m_lock.unlock();
	    return true;
	  }
	
	  bool pop(T &item,int timeout){ // 毫秒
	    m_lock.lock();
	    struct timespec t{0,0};
	    struct timeval now{0,0};
	
	    gettimeofday(&now, nullptr);
	
	    t.tv_sec=now.tv_sec+timeout/1000;
	    t.tv_nsec=(timeout%1000)*1000;
	    if(m_size<=0){
	      if(!m_cond.timewait(m_lock.get(),t)){
	        m_lock.unlock();
	        return false;
	      }
	    }
	
	    if(m_size<=0){ // 锁加条件双重验证
	      m_lock.unlock();
	      return false;
	    }
	
	    m_front=(m_front+1)%m_maxsize;
	    item=m_array[m_front];
	    m_size--;
	    m_lock.unlock();
	    return true;
	  }
	
	private:
	  cond m_cond;
	  locker m_lock;
	
	  int m_maxsize{};
	  int m_size{};
	  T* m_array;
	  int m_front{};
	  int m_back{};
	};
	```

	可以看出这里除了构造，大部分都是要加锁的，这里生产和消费的同步用的条件变量+同步锁

### log

#### 单例模式

这个项目的很多模块都用单例，单例模式保证了一个类只有一个实例，并提供一个访问它的全局访问点，该实例被所有程序模块共享。

单例模式也分为两种，一种是懒汉模式：顾名思义，懒汉模式非常懒，当没有人用它的时候它就不初始化，只有被第一次使用时才去初始化；另一种是饿汉模式：与懒汉模式相反，程序运行时就立刻创建实例进行初始化。

经典的懒汉模式一般要使用**双检测锁**。但C++11之后，可以使用静态局部变量初始化，就不再需要锁，编译器会负责线程安全的问题。

```c++
class Log{
public:
  //  采用懒汉的单例模式
  static Log* get_instance(){
    static Log instance;
    return &instance;
  }
  static void* flush_log_thread(void *args){
    return Log::get_instance()->async_write_log();
  }
  bool init(const char* file_name,int close_log,int log_buf_size,int split_size,int max_queue_size=0);

  void write_log(int level,const char *format,...);

  void flush();

  static tm get_time();

private:
  Log();
  virtual ~Log(){}
private:
  char * m_buf{};
  bool m_is_async; //是否同步
  FILE *m_fp{}; // 文件描述符
  locker m_mutex; // 互斥锁
};
```

一些关键的条件变量
```c++
private:
  char log_name[128]{}; //日志文件名
  char dir_name[128]{}; //目录名称
  int m_close_log{};
  long long m_count; //日志行数
  int m_split_size{}; //日志最大行数
  int m_log_buf_size{}; // 日志缓冲区大小
  int m_today{};
  char * m_buf{};
  block_queue<std::string> *m_block_queue{}; // 阻塞队列(异步使用)
  bool m_is_async; //是否同步
  FILE *m_fp{}; // 文件描述符
  locker m_mutex; // 互斥锁
```

#### 初始化

这里的根据是否设置有阻塞队列的长度判断是否是异步，因为同步用不到阻塞队列，采用异步，我们就要创建一个子进程，这个子进程会worker是这个log类的一个静态方法，
这个静态方法会调用log静态实例的一个私有方法，这个私有方法会不断检测阻塞队列中是否有信息，从阻塞队列的代码我们知道这个会阻塞，如果没有信息，有信息就会进行fputs的系统调用IO操作

```c++
bool Log::init(const char *name, int close_log, int log_buf_size=8192, int split_size=5000000, int max_queue_size) {

  if(max_queue_size>0){ // 异步
    m_is_async= true;
    m_block_queue=new block_queue<std::string>(max_queue_size);
    pthread_t tid;
    pthread_create(&tid, nullptr,Log::flush_log_thread, nullptr);
  }
```

```c++
  static void* flush_log_thread(void *args){
    return Log::get_instance()->async_write_log();
  }
```

```c++
private:
  void *async_write_log(){
    std::string log_str;
    while(m_block_queue->pop(log_str)){
      m_mutex.lock();
      fputs(log_str.c_str(),m_fp);
      m_mutex.unlock();
    }
  }
```

下面的操作就是得到一个日志全名称full_log_name，这个名称就是日志名称+日期，我们文件就会创建或者打开这个full_log_name的文件

```c++
bool Log::init(const char *name, int close_log, int log_buf_size=8192, int split_size=5000000, int max_queue_size) {

  m_close_log=close_log;
  m_split_size=split_size;
  m_buf=new char[log_buf_size];
  memset(m_buf,'\0', m_log_buf_size);

  tm my_tm=Log::get_time();

  m_today=my_tm.tm_mday;

  const char *p=strchr(name,'/');

  char full_log_name[256];
  if(p== nullptr){ // 没有目录
    strcpy(log_name,name);
    snprintf(full_log_name,255,"%d_%02d_%02d_%s",my_tm.tm_year+1900,my_tm.tm_mon+1,my_tm.tm_mday,log_name);
  }
  else{  //有目录
    strcpy(log_name,p+1);
    strncpy(dir_name,name,p- name+1);
    snprintf(full_log_name,255,"%s%d_%02d_%02d_%s",dir_name,my_tm.tm_year+1900,my_tm.tm_mon+1,my_tm.tm_mday,log_name);
  }

  m_fp= fopen(full_log_name,"a");
  if(m_fp== nullptr){
    return false;
  }

  return true;
```

#### write_log

这里的日志分了等级
Log分级：
- Debug，调试代码时的输出，在系统实际运行时，一般不使用。
- Warn，这种警告与调试时终端的warning类似，同样是调试代码时使用。
- Info，报告系统当前的状态，当前执行的流程或接收的信息等。
- Erro，输出系统的错误信息

```c++
void Log::write_log(int level,const char *format,...) {

  tm my_tm=Log::get_time();

  char s[16]={0};
  switch (level) {
    case 0:
      strcpy(s,"[debug]:");
      break;
    case 1:
      strcpy(s,"[info]:");
      break;
    case 2:
      strcpy(s,"[warn]:");
      break;
    case 3:
      strcpy(s,"[error]:");
      break;
    default:
      strcpy(s,"[info]:");
      break;
  }
```

这里的为了日志也是会被分文件的，有下面两种情况
1. 到了新的一天，这时的日志全名称就变了，就是新文件
2. 日志写行数超过了限制的最大行数，这个就要序号分文件了

```c++
  m_mutex.lock();
  m_count++;
  if(my_tm.tm_mday!=m_today||m_count%m_split_size==0){
    char new_log[256]={0};
    fflush(m_fp);
    fclose(m_fp);
    char tail[16]={0};
    snprintf(tail,16,"%d_%02d_%02d_",my_tm.tm_year+1900,my_tm.tm_mon+1,my_tm.tm_mday);
    if(my_tm.tm_mday!=m_today){ // 日期不同，要新建日志文件
      m_count=0;
      snprintf(new_log,255,"%s%s%s",dir_name,tail,log_name);
      m_today=my_tm.tm_mday;

    }
    else if(m_count%m_split_size==0){
      snprintf(new_log, 255, "%s%s%s.%lld", dir_name, tail, log_name, m_count / m_split_size);
    }

    m_fp=fopen(new_log,"a");

  }
  m_mutex.unlock();
```

这里write_log其实是用了c语言的可变参数的，同时搭配了vsnprintf，让传参数更灵活
```c++
  va_list list;
  va_start(list,format);
  std::string log_str;
  m_mutex.lock();
  int n= snprintf(m_buf,48,"%d-%01d-%01d %01d:%01d:%01d %s",my_tm.tm_year+1900,my_tm.tm_mon+1,my_tm.tm_mday,my_tm.tm_hour,my_tm.tm_min,my_tm.tm_sec,s);
  int m= vsnprintf(m_buf+n,m_log_buf_size-n-1,format,list);
  m_buf[m+n]='\n';
  m_buf[m+n+1]='\0';
  log_str=m_buf;
```

平时调用写日志是用定义成不同等级的宏，这样方便书写
```c++
#define LOG_DEBUG(foramt,...) if(m_close_log==0) { Log::get_instance()->write_log(0,foramt, ##__VA_ARGS__); Log::get_instance()->flush();};
#define LOG_INFO(foramt,...) if(m_close_log==0) { Log::get_instance()->write_log(1,foramt, ##__VA_ARGS__); Log::get_instance()->flush();};
#define LOG_WARN(foramt,...) if(m_close_log==0) { Log::get_instance()->write_log(2,foramt, ##__VA_ARGS__); Log::get_instance()->flush();};
#define LOG_ERROR(foramt,...) if(m_close_log==0) { Log::get_instance()->write_log(3,foramt, ##__VA_ARGS__); Log::get_instance()->flush();};
```
