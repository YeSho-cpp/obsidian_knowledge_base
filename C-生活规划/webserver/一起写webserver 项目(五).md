---
title: 一起写webserver 项目(五)
tags: ['项目']
categories:
  - 一起做项目
---

## webserver类的封装

### 参数和方法

```c++
  //基础
  int m_close_log; // 是否关闭日志
  int m_is_async; // 日志是否异步
  char *m_root; // 我们之前服务器上用于存放网页文件的根目录的路径
  http_conn *users; // 对应所有的客户端
  int m_listen_fd;
  int m_TRIGMode;
  int m_LISTENTrigmode;
  int m_CONNTrigmode;
  int m_opt_linger;
  int m_epoll_fd;
  int m_pipefd[2];
  epoll_event events[MAX_EVENT_NUMBER];

  // 数据库相关
  connection_pool *m_sql_pool;
  int m_port;
  string m_user;
  string m_password;
  string m_dbName;
  int m_sql_num;

  // 线程池相关
  thread_pool<http_conn> *m_pool;
  int m_actormodel;
  int m_thread_num;

  // 定时相关
  client_data *user_timer;
  Utils utils;
```

### 初始化各模块

WebServer()

- 这里有初始化uers对象
- 得到服务项目路径用getcwd方法，初始化m_root,结合服务路径得到m_root
- 初始化定时器

```c++
Webserver::Webserver() {
  //http_conn类对象
  users=new http_conn[MAX_FD];

  //root文件夹路径
  char server_path[200];
  getcwd(server_path,200);
  char root[6]="/root";
  m_root=(char *) malloc(strlen(root)+ strlen(server_path)+1);
  strcpy(m_root,server_path);
  strcat(m_root,root);

  // 定时器
  user_timer=new client_data[MAX_FD];
}
```



~WebServer

- 关闭用到的各种文件描述符
- 删除创建在堆上的内存

```c++
Webserver::~Webserver() {
  close(m_epoll_fd);
  close(m_pipefd[0]);
  close(m_pipefd[1]);
  close(m_listen_fd);
  delete[] users;
  delete[] user_timer;
  delete m_pool;
  free(m_root);
}
```


init
- 初始化各参数

```c++
void Webserver::init(int port, string user, string passwd, string databaseName, int log_write, int opt_linger, int trigmode, int sql_num, int thread_num, int close_log, int actor_model) {
  m_port=port;
  m_user=user;
  m_password=passwd;
  m_dbName=databaseName;
  m_is_async=log_write;
  m_opt_linger=opt_linger;
  m_TRIGMode=trigmode;
  m_sql_num=sql_num;
  m_thread_num=thread_num;
  m_close_log=close_log;
  m_actormodel=actor_model;
}
```


### 初始化日志

- 根据是否和是否异步日志用单例创建日志
- file_name="./ServerLog"log_buf_size=2000,split_line=800000,异步的max_queue_size=800

```c++
void Webserver::log_write() {
  if(m_close_log==0){
    if(m_is_async==0){
      Log::get_instance()->init("./ServerLog",m_close_log,2000,800000);
    }
    else if(m_is_async==1){
      Log::get_instance()->init("./ServerLog",m_close_log,2000,800000,800);
    }
  }
}
```

### 初始化数据库连接池

- 单例创建连接池对象
- 初始化连接池对象，url为localhost
- 调用http的initmysql_result初始化数据库读取表

```c++
void Webserver::sql_pool() {
  m_sql_pool = connection_pool::GetInstance();
  m_sql_pool->init("localhost",3306,m_user,m_password,m_dbName,m_close_log,m_sql_num);
  users->initmysql_result(m_sql_pool);
}
```



### 初始化线程池

- 初始化线程池

```c++
void Webserver::threadPool() {
  m_pool=new thread_pool<http_conn>(m_actormodel,m_sql_pool,m_thread_num);
}
```


### 模式选择 trig_mode()

有4中模式分别是 第一个是监听的模式，第二个连接

1. LT+LT   m_TRIGMode=0
2. LT+ET  m_TRIGMode=1
3. ET+LT  m_TRIGMode=2
4. ET+ET  m_TRIGMode=3

```c++
void Webserver::trig_mode() {

  if(m_TRIGMode==0){ // LT+LT
    m_LISTENTrigmode=0;
    m_CONNTrigmode=0;
  }

  if(m_TRIGMode==1){ // LT+ET
    m_LISTENTrigmode=0;
    m_CONNTrigmode=1;
  }

  if(m_TRIGMode==2){ // ET+LT
    m_LISTENTrigmode=1;
    m_CONNTrigmode=0;
  }

  if(m_TRIGMode==3){ // ET+ET
    m_LISTENTrigmode=1;
    m_CONNTrigmode=1;
  }
}
```



### eventListen

- 这是一个正常的正常的创建listenfd、绑定、监听流程
- 我们得到监听的套字节之后要设置是否是优雅关闭，用setsockopt方法，optname=SO_LINGER,optval创一个结构linger 分别是{0，1}和{1，1}
- 绑定地址后还要设置reuseraddr
- 创建m_epollfd，这个是http类和抽象工具类Utils的变量，初始化一下
- 调用抽象工具类Utils的添加信号方法，分别是SIGPIPE、SIGALRM、SIGTEAM,调用alarm

```c++
void Webserver::eventListen() {

  //网络编程基础步骤
  m_listen_fd = socket(AF_INET, SOCK_STREAM, 0);
  assert(m_listen_fd>=0);

  //优雅关闭连接
  if(m_opt_linger==0){ // 优雅关闭连接
    struct linger tmp{0,1};
    setsockopt(m_listen_fd,SOL_SOCKET,SO_LINGER,&tmp, sizeof(tmp));
  }
  else if(m_opt_linger==1){ // 不优雅
    struct linger tmp{1,1};
    setsockopt(m_listen_fd,SOL_SOCKET,SO_LINGER,&tmp, sizeof(tmp));
  }

  int ret=0;
  sockaddr_in address{};
  bzero(&address, sizeof(address));
  address.sin_port= htons(m_port);
  address.sin_family=AF_INET;
  address.sin_addr.s_addr= htonl(INADDR_ANY);

  int flag=1;
  setsockopt(m_listen_fd,SOL_SOCKET,SO_REUSEADDR,&flag, sizeof(flag));
  ret= bind(m_listen_fd, reinterpret_cast<const sockaddr *>(&address), sizeof(address));
  assert(ret>=0);
  ret= listen(m_listen_fd,5);
  assert(ret>=0);

  utils.init(TIMESLOT);

  //epoll创建内核事件表
  // epoll_event events[MAX_EVENT_NUMBER];
  m_epoll_fd=epoll_create(5);
  assert(m_epoll_fd!=-1);

  utils.addfd(m_epoll_fd,m_listen_fd, false,m_LISTENTrigmode);

  ret= socketpair(PF_UNIX,SOCK_STREAM,0,m_pipefd);
  assert(ret!=-1);
  utils.setnonblocking(m_pipefd[1]);
  utils.addfd(m_epoll_fd,m_pipefd[0], false,0);

  utils.addsig(SIGPIPE,SIG_IGN);
  utils.addsig(SIGALRM,Utils::sig_handler, false);
  utils.addsig(SIGTERM,Utils::sig_handler, false);

  alarm(TIMESLOT);

  //工具类,信号和描述符基础操作
  http_conn::m_epoll_fd=m_epoll_fd;
  Utils::u_epollfd=m_epoll_fd;
  Utils::u_pipefd=m_pipefd;

}
```



### eventLoop()

- 这是一个正常的事件循环方式在while调用epoll_wait，关注顺序分别是监听、异常、信号、读写事件
- 循环我们也要检测是否超时，超时了触发抽象工具类的定时处理任务，超时通过timeout这个函数
- 这里有dealclientdata()、deal_timer、dealwithsignal、dealwithread、dealwithwrite分别对应监听、异常、信号、读写事件所要处理的逻辑

```c++
void Webserver::eventLoop() {
  bool stop_server=false;
  bool timeout=false;
  while(!stop_server){
    int num= epoll_wait(m_epoll_fd,events,MAX_EVENT_NUMBER,-1);
    if(num<0&&errno!=EINTR){
      LOG_ERROR("%s","epoll failure");
      break;
    }
    for(int i=0;i<num;i++){
         int sock_fd= events[i].data.fd;

         //处理新到的客户连接
         if(sock_fd==m_listen_fd){
            bool flag=dealclientdata();
            if(!flag) continue;
         }
         else if(events[i].events&(EPOLLRDHUP|EPOLLHUP|EPOLLERR)){
           //服务器端关闭连接，移除对应的定时器
           util_timer *timer= user_timer[sock_fd].timer;
           deal_timer(timer,sock_fd);
         }
         else if((sock_fd==m_pipefd[0])&&(events[i].events&EPOLLIN)){ //处理信号
           bool flag= dealwithsignal(timeout,stop_server);
           if(!flag)
             LOG_ERROR("%s", "dealclientdata failure");
         }
         else if(events[i].events&EPOLLIN){ //处理客户连接上接收到的数据
           dealwithread(sock_fd);
         }
         else if(events[i].events&EPOLLOUT){
           dealwithwrite(sock_fd);
         }
    }
    if(timeout){
      utils.time_handler();
      LOG_INFO("%s", "timer tick");
      timeout= false;
    }
  }
}
```


### dealclientdata()

处理新连接

- 这就是一个处理的连接的代码，核心就是调用accept方法
- LT模式我们调用一次accept，异常有两种，一种就是accpet返回了错误负值，因为我们的监听的描述符是用的非阻塞方式，另外一种就是连接数量超过了设置的最大限制
- ET模式我们就是循环调用accept
- 成功我们都会调用timer方法，因为这个时候我们已经得到新连接的客户端的基本信息，我们用来初始化http_conn类，定时器类client_data，特别初始化这个util_timer类，连接的超时是当前时间+3倍的timeslot,这个时候也要添加到升序链表的定时器。

```c++
bool Webserver::dealclientdata() {

  sockaddr_in client_address{};
  socklen_t client_len= sizeof(client_address);
  if(m_LISTENTrigmode==0){ // LT
    int conn_fd=accept(m_listen_fd, reinterpret_cast<sockaddr *>(&client_address),&client_len);
    if(conn_fd<0){
      LOG_ERROR("%s:errno is:%d", "accept error", errno);
      return false;
    }
    else if(http_conn::m_user_count>=MAX_FD){
      utils.show_errno(conn_fd, "Internal server busy");
      LOG_ERROR("%s", "Internal server busy");
      return true;
    }
    timer(conn_fd,client_address);
  }
  else{ //ET
    while(true){
      int conn_fd=accept(m_listen_fd, reinterpret_cast<sockaddr *>(&client_address),&client_len);
      if(conn_fd<0){
        LOG_ERROR("%s:errno is:%d", "accept error", errno);
        break;
      }
      else if(http_conn::m_user_count>=MAX_FD){
        utils.show_errno(conn_fd, "Internal server busy");
        LOG_ERROR("%s", "Internal server busy");
        break;
      }
      timer(conn_fd,client_address);
    }
    return false;
  }
  return true;
}
```

```c++
void Webserver::timer(int conn_fd, struct sockaddr_in client_address) {
  users[conn_fd].init(conn_fd,client_address,m_root,m_CONNTrigmode,m_close_log,m_user,m_password,m_dbName);

  //初始化client_data数据
  //创建定时器,设置回调函数和超时时间,绑定用户数据,将定时器添加到链表中
  user_timer[conn_fd].address=client_address;
  user_timer[conn_fd].sockfd=conn_fd;
  auto *timer=new util_timer;
  timer->user_data=&user_timer[conn_fd];
  timer->cb_func=cb_func;
  time_t cur= time(nullptr);
  timer->expire=cur+3*TIMESLOT;
  user_timer[conn_fd].timer=timer;
  utils.m_time_lst.add_timer(timer);
}
```


### deal_timer

就是删除这个链接不再监视它，并且从升序链表的定时器中删除，这里的删除我们用之前定义的回调函数cb_func

```c++
void Webserver::deal_timer(util_timer *timer, int sockfd) {
  timer->cb_func(&user_timer[sockfd]);
  if(timer){
    utils.m_time_lst.del_timer(timer);
  }
  LOG_INFO("close fd %d",user_timer[sockfd].sockfd);
}
```


### dealwithsignal

- 主要就是调用recv函数，然后判断是那种信号，SIGALRM超时标志改为true，SIGTERM停止服务标志改为false

```c++
bool Webserver::dealwithsignal(bool &timeout, bool &stop_server) {
  char signals[1024];
  int ret=recv(m_pipefd[0],signals, sizeof(signals),0);
  if(ret<=0){
    return false;
  }

  for(int i=0;i<ret;++i){
    switch (signals[i]) {
      case SIGALRM:{
        timeout= true;
        break;
      }
      case SIGTERM:
      {
        stop_server= true;
        break;
      }
    }
  }
  return true;
}
```

### dealwithread

读事件设计了reactor和proctor两种模式，不过这里的reactor等待子线程IO完成的设计好像有点问题，主线程似乎会跟着罚站，不知道有没有什么优化方法
```c++
void Webserver::dealwithread(int sockfd) {
  util_timer *timer=user_timer[sockfd].timer;
  if(m_actormodel==1){ //reactor
    if(timer){
      adjust_timer(timer);
    }
    //若监测到读事件，将该事件放入请求队列
    m_pool->append(users + sockfd, 0);

    while (true)
    {
      if (1 == users[sockfd].improv)
      {
        if (1 == users[sockfd].timer_flag)
        {
          deal_timer(timer, sockfd);
          users[sockfd].timer_flag = 0;
        }
        users[sockfd].improv = 0;
        break;
      }
    }

  }
  else{ // proactor

      if(users[sockfd].read_once()){
        LOG_INFO("deal with the client(%s)", inet_ntoa(users[sockfd].get_address()->sin_addr));

        //若监测到读事件，将该事件放入请求队列
        m_pool->append_p(users + sockfd);

        if (timer)
        {
          adjust_timer(timer);
        }
      }
      else
      {
        deal_timer(timer, sockfd);
      }
  }
}
```

### dealwithwrite

```c++
void Webserver::dealwithwrite(int sockfd) {
  util_timer *timer = user_timer[sockfd].timer;
  //reactor
  if (1 == m_actormodel)
  {
    if (timer)
    {
      adjust_timer(timer);
    }

    m_pool->append(users + sockfd, 1);

    while (true)
    {
      if (1 == users[sockfd].improv)
      {
        if (1 == users[sockfd].timer_flag)
        {
          deal_timer(timer, sockfd);
          users[sockfd].timer_flag = 0;
        }
        users[sockfd].improv = 0;
        break;
      }
    }
  }
  else
  {
    //proactor
    if (users[sockfd].write())
    {
      LOG_INFO("send data to the client(%s)", inet_ntoa(users[sockfd].get_address()->sin_addr));

      if (timer)
      {
        adjust_timer(timer);
      }
    }
    else
    {
      deal_timer(timer, sockfd);
    }
  }
}
```


## 配置文件，服务器，启动！

### 配置文件和main文件
配置文件和main文件就不讲了，直接贴代码，感兴趣的同学自己看看
```c++
#ifndef CONFIG_H
#define CONFIG_H
 
#include "webserver.h"
 
using namespace std;
 
class Config
{
public:
    Config();
    ~Config(){};
 
    void parse_arg(int argc, char*argv[]);
 
    //端口号
    int PORT;
 
    //日志写入方式
    int LOGWrite;
 
    //触发组合模式
    int TRIGMode;
 
    //listenfd触发模式
    int LISTENTrigmode;
 
    //connfd触发模式
    int CONNTrigmode;
 
    //优雅关闭链接
    int OPT_LINGER;
 
    //数据库连接池数量
    int sql_num;
 
    //线程池内的线程数量
    int thread_num;
 
    //是否关闭日志
    int close_log;
 
    //并发模型选择
    int actor_model;
};
 
#endif
```


```c++
#include "config.h"
 
Config::Config(){
    //端口号,默认9006
    PORT = 9006;
    //PORT = 9000;
 
    //日志写入方式，默认同步
    LOGWrite = 0;
 
    //触发组合模式,默认listenfd LT + connfd LT
    TRIGMode = 0;
 
    //listenfd触发模式，默认LT
    LISTENTrigmode = 0;
 
    //connfd触发模式，默认LT
    CONNTrigmode = 0;
 
    //优雅关闭链接，默认不使用
    OPT_LINGER = 0;
 
    //数据库连接池数量,默认8
    sql_num = 8;
 
    //线程池内的线程数量,默认8
    thread_num = 8;
 
    //关闭日志,默认不关闭
    close_log = 0;
 
    //并发模型,默认是proactor
    actor_model = 0;
}
 
void Config::parse_arg(int argc, char*argv[]){
    int opt;
    const char *str = "p:l:m:o:s:t:c:a:";
    while ((opt = getopt(argc, argv, str)) != -1)
    {
        switch (opt)
        {
        case 'p':
        {
            PORT = atoi(optarg);
            break;
        }
        case 'l':
        {
            LOGWrite = atoi(optarg);
            break;
        }
        case 'm':
        {
            TRIGMode = atoi(optarg);
            break;
        }
        case 'o':
        {
            OPT_LINGER = atoi(optarg);
            break;
        }
        case 's':
        {
            sql_num = atoi(optarg);
            break;
        }
        case 't':
        {
            thread_num = atoi(optarg);
            break;
        }
        case 'c':
        {
            close_log = atoi(optarg);
            break;
        }
        case 'a':
        {
            actor_model = atoi(optarg);
            break;
        }
        default:
            break;
        }
    }
}
```


```c++
#include "config.h"
 
int main(int argc, char *argv[])
{
    //需要修改的数据库信息,登录名,密码,库名
    string user = "root";
    string passwd = "123";
    string databasename = "testDB";
 
    //命令行解析
    Config config;
    config.parse_arg(argc, argv);
 
    WebServer server;
 
    //初始化
    server.init(config.PORT, user, passwd, databasename, config.LOGWrite, 
                config.OPT_LINGER, config.TRIGMode,  config.sql_num,  config.thread_num, 
                config.close_log, config.actor_model);
    
 
    //日志
    server.log_write();
 
    //数据库
    server.sql_pool();
 
    //线程池
    server.thread_pool();
 
    //触发模式
    server.trig_mode();
 
    //监听
    server.eventListen();
 
    //运行
    server.eventLoop();
 
    return 0;
}
```


个性化运⾏（使⽤⾃⼰的参数运⾏）

```sh
./server [-p port] [-l LOGWrite] [-m TRIGMode] [-o OPT_LINGER] [-s sql_num] [-t
thread_num] [-c close_log] [-a actor_model]
```

- -p，⾃定义端⼝号
	- 默认9006
- -l，选择⽇志写⼊⽅式，默认同步写⼊
	- 0，同步写⼊
	- 1，异步写⼊
- -m，listenfd和connfd的模式组合，默认使⽤LT + LT
	- 0，表示使⽤LT + LT
	- 1，表示使⽤LT + ET
	- 2，表示使⽤ET + LT
	- 3，表示使⽤ET + ET
- -o，优雅关闭连接，默认不使⽤
	- 0，不使⽤
	- 1，使⽤
- -s，数据库连接数量
	- 默认为8
	- -t，线程数量
	- 默认为8
- -c，关闭⽇志，默认打开
	- 0，打开⽇志
	- 1，关闭⽇志
- -a，选择反应堆模型，默认Proactor
	- 0，Proactor模型
	- 1，Reactor模型

### 压力测试

#### webbench原理

> webbench是一个开源的用来测试服务器性能的小软件，其基本原理是创建多个子进程进行服务器访问，子进程将自己得到的数据通过管道发给主进程，主进程将结果统计打印输出在屏幕上

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240430212039.png" alt="image.png" style="zoom:60%;" />

#### 测试结果


- Proactor LT+LT  40274.4QPS
	```sh
	yesho@:~/Code/C++/CLiondemo/Webserver/test_pressure/webbench-1.5$ ./webbench -c 10500 -t 5 http://127.0.0.1:9006/
	Webbench - Simple Web Benchmark 1.5
	Copyright (c) Radim Kolar 1997-2004, GPL Open Source Software.
	
	Benchmarking: GET http://127.0.0.1:9006/
	10500 clients, running 5 sec.
	
	Speed=2416464 pages/min, 4510733 bytes/sec.
	Requests: 201372 susceed, 0 failed.
	
	```

- Proactor LT+ET
	```sh
	yesho@:~/Code/C++/CLiondemo/Webserver/test_pressure/webbench-1.5$ ./webbench -c 10500 -t 5 http://127.0.0.1:9006/
	Webbench - Simple Web Benchmark 1.5
	Copyright (c) Radim Kolar 1997-2004, GPL Open Source Software.
	
	Benchmarking: GET http://127.0.0.1:9006/
	10500 clients, running 5 sec.
	
	Speed=1728564 pages/min, 3226652 bytes/sec.
	Requests: 144047 susceed, 0 failed.
	```

- Proactor ET+LT
	```sh
	yesho@:~/Code/C++/CLiondemo/Webserver/test_pressure/webbench-1.5$ ./webbench -c 10500 -t 5 http://127.0.0.1:9006/
	Webbench - Simple Web Benchmark 1.5
	Copyright (c) Radim Kolar 1997-2004, GPL Open Source Software.
	
	Benchmarking: GET http://127.0.0.1:9006/
	10500 clients, running 5 sec.
	
	Speed=1585944 pages/min, 2960406 bytes/sec.
	Requests: 132162 susceed, 0 failed.
	
	```


- Proactor ET+ET
	```sh
	yesho@:~/Code/C++/CLiondemo/Webserver/test_pressure/webbench-1.5$ ./webbench -c 10500 -t 5 http://127.0.0.1:9006/
	Webbench - Simple Web Benchmark 1.5
	Copyright (c) Radim Kolar 1997-2004, GPL Open Source Software.
	
	Benchmarking: GET http://127.0.0.1:9006/
	10500 clients, running 5 sec.
	
	Speed=1749768 pages/min, 3266233 bytes/sec.
	Requests: 145814 susceed, 0 failed.
	
	```


- Reactor LT+LT
	```bash
	yesho@:~/Code/C++/CLiondemo/Webserver/test_pressure/webbench-1.5$ ./webbench -c 10500 -t 5 http://127.0.0.1:9006/
	Webbench - Simple Web Benchmark 1.5
	Copyright (c) Radim Kolar 1997-2004, GPL Open Source Software.
	
	Benchmarking: GET http://127.0.0.1:9006/
	10500 clients, running 5 sec.
	
	Speed=1150344 pages/min, 2147913 bytes/sec.
	Requests: 95862 susceed, 0 failed.
	```


项目地址: https://github.com/YeSho-cpp/My-webserver