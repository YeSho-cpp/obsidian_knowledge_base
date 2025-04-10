---
title: 一起写webserver 项目(四)
tags: ['项目']
categories:
  - 一起做项目
---

## http连接的封装

先前我们写的线程，数据库池或者底层的小工具如locker锁等东西是地基，是骨架，那么http解析部分就是主体，是血肉。这是这个项目最关键的部分。

> 下面是一张简易的框架图，首先说明一下这个WebServer的本质，WebServer的本质上是⼀个**⾼性能⽹络框架**，它提供了⼀个单服务端（当然也可以扩展为多服务端）与多客户端的⾼效连接框架，但是客户端与服务端连接上以后具体应该做些什么（也就是有哪些业务），这就可以由我们⾃由发挥了，这就是 WebServer 的功能扩展。⽬前⼤多数的WebServer都将从服务端获取**MIME**作为主要功能。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240430204114.png " alt="image.png" style="zoom:80%;" />


由于变量和参数过多，先不介绍初始化了，先从设计讲起。
### http_conn的设计

其中大部分设计都是参考了《Linux高性能服务器编程》
#### 固定的一些方法和变量

定义http响应的一些状态信息
```c++
const char *ok_200_title = "OK";
const char *error_400_title = "Bad Request";
const char *error_400_form = "Your request has bad syntax or is inherently impossible to staisfy.\n";
const char *error_403_title = "Forbidden";
const char *error_403_form = "You do not have permission to get file form this server.\n";
const char *error_404_title = "Not Found";
const char *error_404_form = "The requested file was not found on this server.\n";
const char *error_500_title = "Internal Error";
const char *error_500_form = "There was an unusual problem serving the request file.\n";
```


类内的方法枚举,
```c++
enum METHOD
{
	GET = 0,
	POST,
	HEAD,
	PUT,
	DELETE,
	TRACE,
	OPTIONS,
	CONNECT,
	PATCH
};
```

分析状态，这些状态是解析请求的不同阶段，所有的完成后我们才能回应响应
```c++
    enum CHECK_STATE
    {
        CHECK_STATE_REQUESTLINE = 0, // 当前正在分析请求行
        CHECK_STATE_HEADER, // 当前正在分析请求头
        CHECK_STATE_CONTENT // 当前正在分析请求体
    };
```

请求结果枚举
```c++
    enum HTTP_CODE
    {
        NO_REQUEST, // 表示请求不完整，需要继续读取客户数据  这是解析请求的不同阶段的默认返回值
        GET_REQUEST, // 表示获得了一个完整的客户请求  只有这个完成后我们才能进行
        BAD_REQUEST, // 表示客户请求有语法错误 
        NO_RESOURCE, // 请求的资源不存在
        FORBIDDEN_REQUEST, // 表示客户对资源没有足够的访问权限
        FILE_REQUEST, // 这是一个文件请求
        INTERNAL_ERROR, // 表示服务器内部错误；
        CLOSED_CONNECTION // 表示客户端已经关闭连接了
    };
```

状态行
```c++
    enum LINE_STATUS
    {
        LINE_OK = 0,
        LINE_BAD,
        LINE_OPEN
    };
```


一些固定常用方法，具体的实现就不展示了，也可以在《Linux高性能服务器编程》找到

```c++
int setnonblocking(int fd) // 对文件描述符设置非阻塞
void addfd(int epollfd, int fd, bool one_shot, int TRIGMode) //将内核事件表注册读事件，ET模式，选择开启EPOLLONESHOT
void removefd(int epollfd, int fd) //从内核时间表删除描述符
void modfd(int epollfd, int fd, int ev, int TRIGMode) //将事件重置为EPOLLONESHOT
void http_conn::unmap(); // 解除内存映射，并将资源文件到内存的m_file_address重置
```


### read_once()

设置都有读写缓存区，分别是`m_read_buf`和`m_write_buf`

```c++
  char m_read_buf[READ_BUFFER_SIZE]{};
  int m_read_idx{};
  int m_check_idx{};
  char m_write_buf[WRITE_BUFFER_SIZE]{};
  int m_write_idx{};
  int m_start_line{};
```


<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240430171402.png" alt="image.png" style="zoom:60%;" />

- `m_read_idx`:代表从客户端接受的数据的末尾位置。
- `m_check_idx`:用来标记当前正在检查的字符的位置。
- `m_start_line`: 用来标记缓冲区中当前处理的行的起始位置。

`read_once`这个函数主要完成将数据读到缓冲区，定义了一块缓冲区`m_read_buf`专门用来存放从浏览器发送来的请求报文，并用一个指针`m_read_idx`记录,这里分LT、ET模式，在LT模式下，`epoll_wait`会无数次地通知应用程序读事件的发生，直到应用程序去取。这里的应用程序是什么呢？很显然就是下面这个`read_once()`代码，在`if(m_TRIGMode == 0)`程序块里，应用程序用`recv`去将sockfd内的内容取到`m_readbuf`里面，如果没有取完，程序是无所谓的，它会继续往下执行，直到下一次epoll_wait再次通知，它便再进行recv操作。

而在ET模式下，epoll_wait只会通知应用程序一次，应用程序被要求在这一次就把sockfd中全部的数据取出，即read_once，可以看到在ET模式下，代码执行一个永不结束的循环while(true），唯有当数据全部取完（即recv返回-1并设置errno为EAGAIN或EWOULDBLOCK)时，程序才会退出，不然程序就一直循环下去。（其实当对方关闭了sock连接也会退出，但这就属于异常处理的流程，而非常规流程了）

```c++
bool http_conn::read_once() {
  if(m_read_idx>=READ_BUFFER_SIZE){
    return false;
  }
  int read_ret=0;
  if(m_TRIGMode==0){ // LT模式
    read_ret= recv(m_sock_fd,m_read_buf+m_read_idx,READ_BUFFER_SIZE-m_read_idx,0);
    if(read_ret<=0){
      return false;
    }
    m_read_idx+=read_ret;
    return true;
  }
  else{ // ET 模式
    while(true){
      read_ret= recv(m_sock_fd,m_read_buf+m_read_idx,READ_BUFFER_SIZE-m_read_idx,0);
      if(read_ret==-1){ // // 这两个错误表明在非阻塞模式下没有数据可读。这是正常的
        if(errno==EAGAIN||errno==EWOULDBLOCK){
          break;
        }
        return false;
      }
      else if(read_ret==0) return false;
      m_read_idx+=read_ret;
    }
    return true;
  }
}
```

### process 

这是http最为核心的函数了，这是线程池工作线程不断循环执行的逻辑了

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240430165359.png" alt="image.png" style="zoom:60%;" />

主要执行process_read和process_write。

```c++
void http_conn::process() {
  HTTP_CODE read_ret = process_read();
  if(read_ret==NO_REQUEST){
    modfd(m_epoll_fd,m_sock_fd,EPOLLIN,m_TRIGMode);
    return;
  }
  bool write_ret= process_write(read_ret);

  if(!write_ret){
    close_conn();
  }
  modfd(m_epoll_fd,m_sock_fd,EPOLLOUT,m_TRIGMode);
}
```

#### process_read

process_read用来处理连接，我们设计了两个状态机**主状态机/从状态机**来进行报文解析。下图是处理连接的拓扑图： 

- 从状态机负责读取一行的数据
- 当从状态机成功读完一行后，就将这行数据交给主状态机
- 主状态机会根据报文格式以及自身状态对该请求行进行解析
- 解析完后主状态机的状态改变，切换到下一个状态再等待从状态机的数据循环、

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240430184115.png" alt="image.png" style="zoom:60%;" />



从上面的`read_once()`函数中我们获得了一个字符数组`m_read_buf`，和一个指针`m_read_idx`。我们开始读取m_read_buf至今为止保存的内容。这时我们需要创建一个新的指针`m_checked_idx`来记录每一行报文的结束地址。

下面是process_read流程图，

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240430183621.png" alt="image.png" style="zoom:100%;" />


##### parse_line

因为http报文是**按行来分开**不同的信息的，而发送过来的数据要按行处理分出一行的内容，因为报文的行都是以`＜CR＞＜LF＞`结束,我们可以通过这个条件让指针`m_checked_idx`来记录每一行报文的结束地址。当`m_checked_idx`在一行报文的末尾时，那`m_start_line`到`m_checked_idx`就是一行的首尾了。

```c++
//从状态机，用于分析出一行内容
//返回值为行的读取状态，有LINE_OK,LINE_BAD,LINE_OPEN
http_conn::LINE_STATUS http_conn::parse_line() {
  char temp;
  for(;m_check_idx<m_read_idx;++m_check_idx){
    temp=m_read_buf[m_check_idx];
    if(temp=='\r'){
      if(m_check_idx+1==m_read_idx)
        return LINE_OPEN;
      else if(m_read_buf[m_check_idx+1]=='\n'){
        m_read_buf[m_check_idx++]='\0';
        m_read_buf[m_check_idx++]='\0';
        return LINE_OK;
      }
      return LINE_BAD;
    }
    else if(temp=='\n'){
      if(m_check_idx>1&&m_read_buf[m_check_idx-1]=='\r'){
        m_read_buf[m_check_idx-1]='\0';
        m_read_buf[m_check_idx++]='\0';
        return LINE_OK;
      }
      return LINE_BAD;
    }
  }
  return LINE_OPEN;
}
```

而`get_line`就是一行内容了，因为我们在末尾都赋值了`\0`
```c++
text = get_line();
char *get_line() {return m_read_buf+m_start_line;};
ret = parse_request_line(text);
```

##### parse_request_line

```c++
  METHOD m_method; // 请求方法
  char *m_url{};  // url 比如 http://www.baidu.com/index.html
  char *m_version{}; // http 版本
  int cgi{}; //是否启用的POST
```

这个函数主要用来解析请求行 比如`GET http://www.baidu.com/index.html HTTP/1.0`

```c++
http_conn::HTTP_CODE http_conn::parse_request_line(char *text) {

  m_url = strpbrk(text, " \t");
  if (!m_url)
  {
    return BAD_REQUEST;
  }
  *m_url++ = '\0';
  char *method = text;
  if (strcasecmp(method, "GET") == 0)
    m_method = GET;
  else if (strcasecmp(method, "POST") == 0)
  {
    m_method = POST;
    cgi = 1;
  }
  else
    return BAD_REQUEST;
  m_url += strspn(m_url, " \t"); // 这里主要用于去除m_url多余的空格
  m_version = strpbrk(m_url, " \t");
  if (!m_version)
    return BAD_REQUEST;
  *m_version++ = '\0';
  m_version += strspn(m_version, " \t");
  if (strcasecmp(m_version, "HTTP/1.1") != 0)
    return BAD_REQUEST;
  if (strncasecmp(m_url, "http://", 7) == 0)
  {
    m_url += 7;
    m_url = strchr(m_url, '/');
  }

  if (strncasecmp(m_url, "https://", 8) == 0)
  {
    m_url += 8;
    m_url = strchr(m_url, '/');
  }

  if (!m_url || m_url[0] != '/')
    return BAD_REQUEST;
  //当url为/时，显示判断界面
  if (strlen(m_url) == 1)
    strcat(m_url, "judge.html");
  m_check_state = CHECK_STATE_HEADER;
  return NO_REQUEST;
}
```

##### parse_headers

解析完请求行，我们再来解析请求头部，根据报文格式，以及头部字段名的种类，我们有下面的代码

```c++
	bool m_linger{}; // 是否保持连接
	long m_content_length{};
	char *m_host{};
```

```c++
//解析http请求的一个头部信息
http_conn::HTTP_CODE http_conn::parse_headers(char *text) {
  if (text[0] == '\0')
  {
    if (m_content_length != 0)
    {
      m_check_state = CHECK_STATE_CONTENT;
      return NO_REQUEST;
    }
    return GET_REQUEST;
  }
  else if (strncasecmp(text, "Connection:", 11) == 0)
  {
    text += 11;
    text += strspn(text, " \t");
    if (strcasecmp(text, "keep-alive") == 0)
    {
      m_linger = true;
    }
  }
  else if (strncasecmp(text, "Content-length:", 15) == 0)
  {
    text += 15;
    text += strspn(text, " \t");
    m_content_length = atol(text);
  }
  else if (strncasecmp(text, "Host:", 5) == 0)
  {
    text += 5;
    text += strspn(text, " \t");
    m_host = text;
  }
  else
  {
    LOG_INFO("oop!unknown header: %s", text)
  }
  return NO_REQUEST;
```

##### parse_content

如果时post请求还要解析请求体

```c++
char* m_string{}; //存储请求体数据
```

```c++
http_conn::HTTP_CODE http_conn::parse_content(char *text) {
  if(m_read_idx>=(m_content_length+m_check_idx)){
    text[m_content_length]='\0';
    //POST请求中最后为输入的用户名和密码
    m_string=text;
    return GET_REQUEST;
  }
  return NO_REQUEST;
}
```

##### process_read

在POST请求报文中，因为消息体结尾没有 `\r\n` ，所以不会触发`parse_line()`解析，所以我们只能根据主状态机进行条件判断进入循环。但这会有个问题，等POST请求报文全部解析完后，m_check_state依然是`CHECK_STATE_CONTENT`,还是不会退出循环。这不是我们所希望的，所以我们让它加上`line_status == LINE_OK`,这样当POST消息体全部解析完后，`line_status`会被赋值为`LINE_OPEN`,就不再进入主循环

```c++
http_conn::HTTP_CODE http_conn::process_read() {
  LINE_STATUS line_status=LINE_OK;
  HTTP_CODE ret=NO_REQUEST;
  char *text=nullptr;
  while ((m_check_state == CHECK_STATE_CONTENT && line_status == LINE_OK) || ((line_status = parse_line()) == LINE_OK))
  {
    text = get_line();
    m_start_line = m_check_idx;
    LOG_INFO("%s", text)
    switch (m_check_state)
    {
      case CHECK_STATE_REQUESTLINE:
      {
        ret = parse_request_line(text);
        if (ret == BAD_REQUEST)
          return BAD_REQUEST;
        break;
      }
      case CHECK_STATE_HEADER:
      {
        ret = parse_headers(text);
        if (ret == BAD_REQUEST)
          return BAD_REQUEST;
        else if (ret == GET_REQUEST)
        {
          return do_request();
        }
        break;
      }
      case CHECK_STATE_CONTENT:
      {
        ret = parse_content(text);
        if (ret == GET_REQUEST)
          return do_request(); // 才具备了执行do_request的充分条件
        line_status = LINE_OPEN;
        break;
      }
      default:
        return INTERNAL_ERROR;
    }
  }
  return NO_REQUEST;
}
```

#### do_request

因为客户端主要从服务端获取**MIME**作为主要功能，客户发起了一个图片的请求`http://xxx/images/pic.jpg`,在服务端来说这是一个链接，在服务端那边就是一个请求资源文件的路径，服务端根据请求的`URL`路径，去其文件系统中寻找对应的文件。如果文件存在，服务端将继续处理；如果文件不存在，则通常返回404错误（资源未找到）。

`do_request()`主要目的是解析HTTP请求URL，根据不同的路径（URL中的部分内容）来执行不同的操作，例如处理CGI请求（如登录、注册等），处理静态文件请求等。

```c++
  char m_real_file[FILENAME_LEN]{};
  char* doc_root{}; // 服务器上用于存放网页文件的根目录的路径
  char *m_file_address{}; // 请求文件被mmap到内存中的位置
```

其实本质上这个函数是为了得到`m_real_file`,资源文件的路径，得到`m_real_file`后**映射文件内容到内存**，这样操作系统可以利用虚拟内存系统来访问文件，文件的读取和写入就像访问普通内存一样高效。

```c++
http_conn::HTTP_CODE http_conn::do_request() {
  strcpy(m_real_file,doc_root);
  int len= strlen(doc_root);
  const char *p= strrchr(m_url,'/');
  //处理cgi
  if (cgi == 1 && (*(p + 1) == '2' || *(p + 1) == '3')) // 服务器优化设计定的动作对应特定的数字标识 2代表登录 3代表注册
  {
    //根据标志判断是登录检测还是注册检测
    char flag = m_url[1]; //m_url:/2CGISQL.cgi
    char *m_url_real = (char *)malloc(sizeof(char) * 200);
    strcpy(m_url_real, "/");
    strcat(m_url_real, m_url + 2);
    strncpy(m_real_file + len, m_url_real, FILENAME_LEN - len - 1);
    free(m_url_real);

    //将用户名和密码提取出来
    //user=123&passwd=123
    char name[100], password[100];
    int i;
    for (i = 5; m_string[i] != '&'; ++i)
      name[i - 5] = m_string[i];
    name[i - 5] = '\0';

    int j = 0;
    for (i = i + 10; m_string[i] != '\0'; ++i, ++j)
      password[j] = m_string[i];
    password[j] = '\0';

    if (*(p + 1) == '3')
    {
      //如果是注册，先检测数据库中是否有重名的
      //没有重名的，进行增加数据
      char *sql_insert = (char *)malloc(sizeof(char) * 200);

      snprintf(sql_insert, strlen(sql_insert), "INSERT INTO user(username, passwd) VALUES('%s', '%s')",name,password);
      m_lock.lock();
      if (users.find(name) == users.end())
      {

        int res = mysql_query(mysql, sql_insert);
        users.insert(pair<string, string>(name, password));


        if (!res)
          strcpy(m_url, "/log.html");
        else
          strcpy(m_url, "/registerError.html");
      }
      else
        strcpy(m_url, "/registerError.html");
      m_lock.unlock();
    }
    //如果是登录，直接判断
    //若浏览器端输入的用户名和密码在表中可以查找到，返回1，否则返回0
    else if (*(p + 1) == '2')
    {
      if (users.find(name) != users.end() && users[name] == password)
        strcpy(m_url, "/welcome.html");
      else
        strcpy(m_url, "/logError.html");
    }
  }
  if (*(p + 1) == '0')
  {
    char *m_url_real = (char *)malloc(sizeof(char) * 200);
    strcpy(m_url_real, "/register.html");
    strncpy(m_real_file + len, m_url_real, strlen(m_url_real));
    free(m_url_real);
  }
  else if (*(p + 1) == '1')
  {
    char *m_url_real = (char *)malloc(sizeof(char) * 200);
    strcpy(m_url_real, "/log.html");
    strncpy(m_real_file + len, m_url_real, strlen(m_url_real));
    free(m_url_real);
  }
  else if (*(p + 1) == '5')
  {
    char *m_url_real = (char *)malloc(sizeof(char) * 200);
    strcpy(m_url_real, "/picture.html");
    strncpy(m_real_file + len, m_url_real, strlen(m_url_real));

    free(m_url_real);
  }
  else if (*(p + 1) == '6')
  {
    char *m_url_real = (char *)malloc(sizeof(char) * 200);
    strcpy(m_url_real, "/video.html");
    strncpy(m_real_file + len, m_url_real, strlen(m_url_real));

    free(m_url_real);
  }
  else if (*(p + 1) == '7')
  {
    char *m_url_real = (char *)malloc(sizeof(char) * 200);
    strcpy(m_url_real, "/fans.html");
    strncpy(m_real_file + len, m_url_real, strlen(m_url_real));

    free(m_url_real);
  }
  else
  {
    strncpy(m_real_file + len, m_url, FILENAME_LEN - len - 1);
  }


  if (stat(m_real_file, &m_file_stat) < 0) // 这里<0就代表文件不存在了
  {

    return NO_RESOURCE;
  }

  if (!(m_file_stat.st_mode & S_IROTH))  // S_IROTH，即是否设置了其他（other）用户的读权限
    return FORBIDDEN_REQUEST;

  if (S_ISDIR(m_file_stat.st_mode)) // 宏 S_ISDIR 检查 st_mode 是否表示这是一个目录
    return BAD_REQUEST;

  int fd = open(m_real_file, O_RDONLY);
  m_file_address = (char *)mmap(nullptr, m_file_stat.st_size, PROT_READ, MAP_PRIVATE, fd, 0);
  close(fd);
  return FILE_REQUEST;
}
```

#### process_write


这个process_write主要来根据处理HTTP请求的结果构建相应的HTTP响应并准备发送数据的。

HTTP应答的部分内容如下：

```c
HTTP/1.0 200 OK
Server:BWS/1.0
Content-Length:8024
Content-Type:text/html;charset=gbk
Set-Cookie:BAIDUID=A5B6C72D68CF639CE8896FD79A03FBD8:FG=1;expires=Wed,04-Jul-42 00:10:47 GMT;path=/;domain=.baidu.com
Via:1.0 localhost(squid/3.0 STABLE18)
```


##### add_response
这里定义一个基础的往HTTP**响应的缓冲区**添加格式化的数据的函数，并且使用可变参数`va_list`增加它的可复用性。这样我们后面的各种写数据就可以直接调用`add_response`了。

```c++
bool http_conn::add_response(const char *format, ...) {
  if (m_write_idx >= WRITE_BUFFER_SIZE)
    return false;
  va_list arg_list;
  va_start(arg_list, format);
  int len = vsnprintf(m_write_buf + m_write_idx, WRITE_BUFFER_SIZE - 1 - m_write_idx, format, arg_list);
  if (len >= (WRITE_BUFFER_SIZE - 1 - m_write_idx))
  {
    va_end(arg_list);
    return false;
  }
  m_write_idx += len;
  va_end(arg_list);

  LOG_INFO("request:%s", m_write_buf)

  return true;
}
```

调用add_response的函数系列

```c++
/*添加状态行*/
bool http_conn::add_status_line(int status, const char *title) {
    return add_response("%s %d %s\r\n", "HTTP/1.1", status, title);
}
 
/*添加消息报头，具体的添加长度文本 连接状态 和空行*/
bool http_conn::add_headers(int content_len) {
    return add_content_length(content_len) && add_linger() && add_blank_line();
}
bool http_conn::add_content_length(int content_len) {
    return add_response("Content-Length:%d\r\n", content_len);
}
bool http_conn::add_content_type() {
    return add_response("Content-Type:%s\r\n", "text/html");
}
bool http_conn::add_linger() {
    return add_response("Connection:%s\r\n",(m_linger == true) ? "keep-alive" : "close");
}
bool http_conn::add_blank_line() {
    return add_response("%s", "\r\n");
}
bool http_conn::add_content(const char *content) {
    return add_response("%s", content);
}
```

##### process_write


```c++
  struct stat m_file_stat{}; // 目标文件的状态信息
  struct iovec m_iv[2]{}; // 用于writev操作的结构体数组。
  int m_iv_count{}; // 被用于输出的iovec结构体数量
```

如果是`FILE_REQUEST`,我们要发两段信息，用了iovec，因为后面write用`writev`，一段是用于写缓冲区的给客户端的响应，一段是文件内容，是客户端请求的资源文件

```c++
bool http_conn::process_write(http_conn::HTTP_CODE ret) {
  switch (ret)
  {
    case INTERNAL_ERROR:
    {
      add_status_line(500, error_500_title);
      add_headers(strlen(error_500_form));

      if (!add_content(error_500_form))
        return false;
      break;
    }
    case BAD_REQUEST:
    {
      add_status_line(404, error_404_title);
      add_headers(strlen(error_404_form));
      if (!add_content(error_404_form))
        return false;
      break;
    }
    case FORBIDDEN_REQUEST:
    {
      add_status_line(403, error_403_title);
      add_headers(strlen(error_403_form));
      if (!add_content(error_403_form))
        return false;
      break;
    }
    case FILE_REQUEST:
    {
      add_status_line(200, ok_200_title);
      if (m_file_stat.st_size != 0)
      {
        add_headers(m_file_stat.st_size);
        m_iv[0].iov_base = m_write_buf;
        m_iv[0].iov_len = m_write_idx;
        m_iv[1].iov_base = m_file_address;
        m_iv[1].iov_len = m_file_stat.st_size;
        m_iv_count = 2;
        bytes_to_send = m_write_idx + m_file_stat.st_size;
        return true;
      }
      else
      {
        const char *ok_string = "<html><body></body></html>";
        add_headers(strlen(ok_string));
        if (!add_content(ok_string))
          return false;
      }
    }
    default:{
      return false;
    }

  }
  m_iv[0].iov_base = m_write_buf;
  m_iv[0].iov_len = m_write_idx;
  m_iv_count = 1;
  bytes_to_send = m_write_idx;
  return true;
}
```

### write

在写缓冲区写满要发送的数据后，我们最后调用write将其发送给浏览器客户端,如果保持连接发送完了数据后要重置各参数不返回false

```c++
  int bytes_to_send{};
  int bytes_has_send{};
```

```c++
bool http_conn::write() {

  int temp = 0;

  if (bytes_to_send == 0) // 所有响应数据已经成功发送
  {
    modfd(m_epoll_fd, m_sock_fd, EPOLLIN, m_TRIGMode);
    reset();
    return true;
  }

  while (true)
  {
    temp = writev(m_sock_fd, m_iv, m_iv_count);

    if (temp < 0)
    {
      if (errno == EAGAIN) // 缓冲写满了
      {
        modfd(m_epoll_fd, m_sock_fd, EPOLLOUT, m_TRIGMode);
        return true;
      }
      unmap();
      return false;
    }

    bytes_has_send += temp;
    bytes_to_send -= temp;
    if (bytes_has_send >= m_iv[0].iov_len)
    {
      m_iv[0].iov_len = 0;
      m_iv[1].iov_base = m_file_address + (bytes_has_send - m_write_idx);
      m_iv[1].iov_len = bytes_to_send;
    }
    else
    {
      m_iv[0].iov_base = m_write_buf + bytes_has_send;
      m_iv[0].iov_len = m_iv[0].iov_len - bytes_has_send;
    }

    if (bytes_to_send <= 0)
    {
      unmap();
      modfd(m_epoll_fd, m_sock_fd, EPOLLIN, m_TRIGMode);

      if (m_linger)
      {
        reset();
        return true;
      }
      else
      {
        return false;
      }
    }
  }
}
```

### 初始化工作

现在列出类中的各种参数，这样就很清晰每个参数的使用途径了

```c++
public:
  static int m_epoll_fd;
  static int m_user_count;
  MYSQL *mysql{};
  int m_state{}; // 0 读 1 写
private:
  int m_sock_fd{};
  sockaddr_in m_address{};
  int m_TRIGMode{}; // 触发模式
  char *m_file_address{}; // 请求文件被mmap到内存中的位置
  char m_read_buf[READ_BUFFER_SIZE]{};
  int m_read_idx{};
  int m_check_idx{};
  char m_write_buf[WRITE_BUFFER_SIZE]{};
  int m_write_idx{};
  int m_start_line{};
  char m_real_file[FILENAME_LEN]{};
  char* doc_root{}; // 服务器上用于存放网页文件的根目录的路径
  CHECK_STATE m_check_state;
  METHOD m_method;
  char *m_url{};
  char *m_version{};
  char *m_host{};
  struct stat m_file_stat{}; // 目标文件的状态信息
  struct iovec m_iv[2]{}; // 用于writev操作的结构体数组。
  int m_iv_count{}; // 被用于输出的iovec结构体数量
  int cgi{}; //是否启用的POST
  char* m_string{}; //存储请求体数据
  int m_close_log{};
  char sql_user[100]{};
  char sql_password[100]{};
  char sql_name[100]{};
  int bytes_to_send{};
  int bytes_has_send{};
  // 用户信息和数据库配置
  map<string,string>m_users;
  bool m_linger{}; // 是否保持连接
  long m_content_length{};
```


```c++
void http_conn::init(int sockfd, const sockaddr_in &addr, char *root, int TRIGMode,int close_log, string user, string passwd, string sqlname) {
  m_sock_fd=sockfd;
  m_TRIGMode=TRIGMode;
  m_address=addr;
  m_close_log=close_log;
  addfd(m_epoll_fd,m_sock_fd,true,TRIGMode);
  m_user_count++;
  //当浏览器出现连接重置时，可能是网站根目录出错或http响应格式出错或者访问的文件中内容完全为空
  doc_root=root;
  strcpy(sql_user,user.c_str());
  strcpy(sql_password,passwd.c_str());
  strcpy(sql_name,sqlname.c_str());
  reset(); // 主要进行私有无参的重置  在我们发送完所有的数据调用这个方法
}
```

```c++
void http_conn::reset() { // 主要进行私有无参的重置  在我们发送完所有的数据调用这个方法
  mysql= nullptr;
  m_state=0;
  cgi=0;

  m_file_address= nullptr;
  m_read_idx=0;
  m_check_idx=0;
  m_start_line=0;
  m_write_idx=0;

  m_check_state=CHECK_STATE_REQUESTLINE;
  m_method=GET;


  m_url= nullptr;
  m_version= nullptr;
  m_host= nullptr;
  m_string= nullptr;

  bytes_to_send=0;
  bytes_has_send=0;
  m_content_length=0;
  m_linger=false;

  timer_flag=0;
  improv=0;

  memset(m_read_buf,'\0',READ_BUFFER_SIZE);
  memset(m_write_buf,'\0',WRITE_BUFFER_SIZE);
  memset(m_real_file,'\0',FILENAME_LEN);

}
```
