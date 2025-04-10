# Nginx概述 

## Nginx介绍 

Nginx是一款轻量级的**web服务器**/**反向代理服务器**及电子邮件（IMAP/POP3)代理服务器。其特点是占有内存少，并发能力强，事实上nginx的<font color=#ff0000>并发</font>能力在同类型的网页服务器中表现较好，中国大陆使用nginx的网站有：百度、京东、 新浪、网易、腾讯、淘宝等。

Nginx是由**伊戈尔·赛索耶夫**为俄罗斯访问量第二的Rambler.ru站点（俄文：PaM6nep)开发的，第一个公开版本 0.1.0发布于2004年10月4日。 

官网：[nginx](https://nginx.org/) 

<img src="https://article.biliimg.com/bfs/article/dd9f1b888a77c2cf7836683f4029ef5acb3c3912.png" alt="image.png" style="zoom:50%;" />

## Nginx下载和安装 

可以到Nginx官方网站下载Nginx的安装包，地址为：[nginx: download](https://nginx.org/en/download.html) 


> [!example]+ 安装过程： 
> 1. 安装依赖包 `yum -y install gcc pcre-devel zlib-devel openssl openssl-devel` 
> 2. 下载Nginx安装包`wget https://nginx.org/download/nginx-1.16.1.tar.gz `
> 3. 解压 `tar -zxvf nginx-1.16.1.tar.gz` 
> 4. `cd nginx-1.16.1` 
> 5. `./configure --prefix=/usr/local/nginx` 
> 6. `make && make install` 



## Nginx目录结构 

安装完Nginx后，我们先来熟悉一下Nginx的目录结构，如右图：

重点目录/文件：
- `conf/nginx.conf`   ---->   nginx配置文件
- `html`   ---->   存放静态文件(html、css、js等)
- `logs`   ---->   日志目录，存放日志文件
- `sbin/nginx`   ---->   二进制文件，用于启动、停止Nginx服务


<img src="https://article.biliimg.com/bfs/article/7206bd13761b9481f46c133689fcf9051eb683fd.png" alt="image.png" style="zoom:50%;" />


# Nginx命令 

**查看版本** 

- 查看Nginx版本可以使用命令： 
- `nginx -v `

	<img src="https://article.biliimg.com/bfs/article/2b83a1451344c51fbbb1a16f04a8646ac0e1fba1.png" alt="image.png" style="zoom:50%;" />

**检查配置文件正确性** 
- 在启动Nginx服务之前，可以先检查一下conf/nginx.conf文件配置的是否有错误，命令如下： 
- `nginx -t `

	<img src="https://article.biliimg.com/bfs/article/47c903992c3a8d71f29c4645654ab7a8a03c60e5.png" alt="image.png" style="zoom:50%;" />
	
**启动和停止** 

- 启动Nginx服务使用如下命令： 
- `nginx `

- 停止Nginx服务使用如下命令： 
- `nginx -s stop `

- 处理完所有的请求并停止
- `nginx -s quit`

- 启动完成后可以查看Nginx进程： 
- `ps -ef | grep nginx `

- 将日志写入一个新的文件
- `nginx -s reopen`

开启不了网址需要关闭防火墙:  `systemctl stop firewalld`

**重新加载配置文件** 

- 当修改Nginx配置文件后，需要重新加载才能生效，可以使用下面命令重新加载配置文件： 
- `nginx -s reload `

**使用systemctl启动、停止、重新加载**

- `systemctl start nginx`
- `systemctl status nginx`

# Nginx配置文件结构 

**整体结构介绍** 

Nginx配置文件（`conf/nginx.conf`)整体分为三部分： 

- 全局块   ---->   和Nginx运行相关的全局配置 
- events块   ---->   和网络连接相关的配置 
- http块   ---->   代理、缓存、日志记录、虚拟主机配置 
	- http全局块 
	- <font color=#ff0000>Server块 </font>
		- Server全局块 
		- location块 

**注意**：http块中可以配置<font color=#ff0000>多个</font>Server块，每个Server块中可以   配置多个location块。 

<img src="https://article.biliimg.com/bfs/article/7037f14a8c59284e19697cd1676afca360827d4c.png" alt="image.png" style="zoom:50%;" />


# Nginx具体应用 

## 部署静态资源 

**Nginx**可以作为静态web服务器来部署[[javaweb#WEB资源|静态资源]]。**静态资源**指在服务端真实存在并且能够直接展示的一些文件，比如 常见的html页面、css文件、js文件、图片、视频等资源。 
相对于[[javaweb#tomcat服务器|Tomcat]],Nginx处理静态资源的能力更加高效，所以在生产环境下，一般都会将静态资源部署到Nginx中。 
将静态资源部署到Nginx非常简单，只需要将文件复制到Nginx安装目录下的html(`/usr/local/nginx/html`)目录中即可。 

```Nginx
server {
	listen 80;   #监听端口 
	server_name localhost;   服务器名称 
	location / { #匹配客户端请求url 
		root html; #指定静态资源根目录 
		index index.html; #指定默认首页 
	}
}
```

### listen

监听可以配置成`IP`或`端口`或`IP+端口`

- `listen 127.0.0.1:8000;`
- `listen 127.0.0.1;（ 端口不写,默认80 ）`
- `listen 8000;`
- `listen *:8000;`
- `listen localhost:8000;`

### server_name

- server_name主要用于区分，可以随便起。
- 也可以使用变量 `$hostname` 配置成主机名。
- 或者配置成域名： `example.org` `www.example.org` `*.example.org`
- 如果多个server的端口重复，那么根据`域名`或者`主机名`去匹配 server_name 进行选择。

下面的例子中：

- `curl http://localhost:80`会访问`/usr/share/nginx/html`
- `curl http://nginx-dev:80`会访问`/home/AdminLTE-3.2.0`

```nginx
# curl http://localhost:80 会访问这个
server {
    listen       80;
    server_name  localhost;
    #access_log  /var/log/nginx/host.access.log  main;
    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
 # curl http://nginx-dev:80 会访问这个
server{
    listen 80;
    server_name nginx-dev;#主机名
    location / {
        root /home/AdminLTE-3.2.0;
        index index.html index2.html index3.html;
    }
}
```

### location

- `/`请求指向 root 目录
- location 总是从`/`目录开始匹配，如果有子目录，例如`/css`，他会指向`/static/css`

```nginx
location /css {
  root /static;
}
```



## 反向代理 

### 正向代理

- 正向代理 
	是一个位于**客户端**和**原始服务器**（origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标（原始服务器）,然后代理向原始服务器转交请求并将获得的内容返回给客户端。 
	正向代理的典型用途是为在防火墙内的局域网客户端提供**访问Internet的途径**。 
	正向代理一般是在<font color=#ff0000>客户端</font>设置代理服务器，通过代理服务器转发请求，最终访问到目标服务器。 

	<img src="https://article.biliimg.com/bfs/article/35089b6658cf8764bd8066042ece6e0d0a841589.png" alt="image.png" style="zoom:50%;" />

<img src="https://article.biliimg.com/bfs/article/427b77e2364465468c829ac6158a0a8c06b4c5bb.png" alt="image.png" style="zoom:50%;" />
### 反向代理

- 反向代理 
	反向代理服务器位于**用户**与**目标服务器**之间，但是对于用户而言，反向代理服务器就相当于目标服务器，即用户直接访问反向代理服务器就可以获得目标服务器的资源，反向代理服务器负责将请求转发给目标服务器(在==目标服务器上显示的请求地址是nginx==，这是关键)。 
	用户**不需要知道目标服务器的地址**，也无须在用户端作任何设定。 
	<img src="https://article.biliimg.com/bfs/article/c34427de66b231fad617c604fe1f9f17e76dc11b.png" alt="image.png" style="zoom:50%;" />

	<img src="https://article.biliimg.com/bfs/article/4448af1c905ba5c8b7b3964c1ba018d5993242c0.png" alt="image.png" style="zoom:50%;" />


> [!note] 区别：
> - 正向是指给客户端做代理，反向是指给服务器做代理           
> - 举个栗子：你买东西的时候找代理人帮你去买就是正向代理，找代理商卖给你东西就是反向代理，厂商就是web服务器，代理商就是nginx反向代理服务器              
> - 一个是基于客户端的，一个是基于服务器的       
> - 和正向代理不同，反向代理相当于是为目标服务器工作的，当你去访问某个网站时，你以为你访问问的是目标服务器，其实不然，当你访问时，其实是由一个代理服务器去接收你的请求           

### 反向代理配置

- 配置反向代理 

	<img src="https://article.biliimg.com/bfs/article/639eb6ff26f48c433977d2fb21c78d99d8d97b78.png" alt="image.png" style="zoom:50%;" />

	```Nginx
location /some/path/ {
    #nginx的主机地址
    proxy_set_header Host $http_host;
    #用户端真实的IP，即客户端IP
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

    proxy_pass http://localhost:8088;
}
	```

- 如果`proxy-pass`的地址只配置到端口，不包含`/`或其他路径，那么location将被追加到转发地址中

	```nginx
	location /some/path/ {
	    proxy_pass http://localhost:8080;
	}
	```

	如上所示，访问 `http://localhost/some/path/page.html` 将被代理到 `http://localhost:8080/some/path/page.html`

	```nginx
	location /some/path/ {
	    proxy_pass http://localhost:8080/zh-cn/;
	}
	```

	如果`proxy-pass`的地址包括`/`或其他路径，那么/some/path将会被替换，如上所示，访问 `http://localhost/some/path/page.html` 将被代理到 `http://localhost:8080/zh-cn/page.html`。

### 设置代理请求headers

‎用户可以重新定义或追加header信息传递给后端[‎](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass_request_headers)‎服务器。可以包含文本、变量及其组合。默认情况下，仅重定义两个字段：‎
```nginx
proxy_set_header Host       $proxy_host;
proxy_set_header Connection close;
```

由于使用反向代理之后，后端服务无法获取用户的真实IP，所以，一般反向代理都会设置以下header信息。
```nginx
location /some/path/ {
    #nginx的主机地址
    proxy_set_header Host $http_host;
    #用户端真实的IP，即客户端IP
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_pass http://localhost:8088;
}
```

常用变量的值：

- `$host`：nginx主机IP，例如192.168.56.105
- `$http_host`：nginx主机IP和端口，192.168.56.105:8001
- `$proxy_host`：localhost:8088，proxy_pass里配置的主机名和端口
- `$remote_addr`:用户的真实IP，即客户端IP。



> [!note]+ nginx反向代理的好处： 
> - 提高访问速度(nginx反向代理可以做[[nginx#缓存（cache）|缓存]]) 
> - 进行[[nginx#负载均衡|负载均衡]](集群模式下好用)
> - 保证后端服务安全(nginx可以走内网转发给后端) 




## 动静分离

`Apache Tocmat` 严格来说是一款java EE服务器，主要是用来处理[[javaweb#Servlet概述|servlet]]请求。处理css、js、图片这些**静态文件**的**IO性能**不够好，因此，将静态文件交给nginx处理，可以提高系统的访问速度，减少tomcat的请求次数，有效的 给后端服务器降压。 

**动静分离** 

<img src="https://article.biliimg.com/bfs/article/e15b670ab51ab951302ae409ae5cfd27b07d8c14.png" alt="image.png" style="zoom:50%;" />

静态文件目录说明：

除了js、css、图片文件之外，还有字体文件和一个ie提示页面

<img src="https://article.biliimg.com/bfs/article/d9c1d457caa83737748f3156270c4d5f91b65f31.png" alt="image.png" style="zoom:40%;" />

```nginx
server
{
  listen 8002;
  server_name ruoyi.tomcat;
  # 动态匹配
  location / {
    proxy_pass http://localhost:8080;
  }

  # 静态匹配
  location ~ \.(js|css|png|jpg|gif|ico)
  {
    root /home/www/static;
  }
  location = /html/ie.html{
    root /home/www/static;
  }
  location ^~ /fonts/ {
    root /home/www/static;
  }
}
```

#### location 修饰符

- location可以使用修饰符或正则表达式

**修饰符**：
- = 等于，严格匹配 ，匹配优先级最高。

- `^~` 表示普通字符匹配。使用前缀匹配。如果匹配成功，则不再匹配其它 location。优先级第二高。

- `~` 区分大小写

- `~*` 不区分大小写

- 优先级

优先级从高到低依次为：。

1. 精确匹配（=）
2. 前缀匹配（^~）
3. 正则匹配（~和～*）
4. 不写

```nginx
location ^~ /images/ {
    proxy_pass http://localhost:8080;
}

location ~ \.jpg {
    proxy_pass http://localhost:8080;
}
```

如上所示：

- `/images/1.jpg`代理到 `http://localhost:8080/images/1.jpg`
- `/some/path/1.jpg` 代理到`http://localhost:8080/some/path/1.jpg`

## 缓冲(buffer)和缓存(cache)

### 缓冲（buffer）

- 缓冲一般放在**内存**中，如果不适合放入内存（比如超过了指定大小），则会将响应写入**磁盘临时文件**中。
- 启用缓冲后，nginx先将后端的请求响应（`response`）放入缓冲区中，等到整个响应完成后，再发给客户端。

	<img src="https://article.biliimg.com/bfs/article/321da9694f089b418ad6135153870e7e99e22d2d.png" alt="image.png" style="zoom:50%;" />

- 客户端往往是用户网络，情况复杂，可能出现网络不稳定，速度较慢的情况。
- 而nginx到后端server一般处于同一个机房或者区域，网速稳定且速度极快。

	<img src="https://article.biliimg.com/bfs/article/264d5e86d0957551687e9d0aa6c38f8c698393c3.png" alt="image.png" style="zoom:50%;" />

- 如果禁用了缓冲，则在客户端从代理服务器接收响应时，响应将同步发送到客户端。对于需要尽快开始接收响应的快速交互式客户端，此行为可能是可取的。
- 这就会带来一个问题：因为客户端到nginx的网速过慢，导致nginx只能以一个较慢的速度将响应传给客户端；进而导致后端server也只能以同样较慢的速度传递响应给nginx，造成一次请求连接耗时过长。

<img src="https://article.biliimg.com/bfs/article/8b4808337b6e8f98439a91fa492d011acfab5fdf.png" alt="image.png" style="zoom:50%;" />

- 开启代理缓冲后，nginx可以用较快的速度尽可能将响应体读取并缓冲到本地内存或磁盘中，然后同时根据客户端的网络质量以合适的网速将响应传递给客户端。
- 这样既解决了server端连接过多的问题，也保证了能持续稳定的像客户端传递响应。
- 使用[proxy_buffering](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffering)启用和禁用缓冲，nginx默认为 on 启用缓冲，若要关闭，设置为 off 。
- 在高并发的情况下，后端server可能会出现大量的连接积压，最终拖垮server端。

```nginx
proxy_buffering off;
```

[proxy_buffers](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffers) 指令设置每个连接读取响应的缓冲区的`大小`和`数量` 。默认情况下，缓冲区大小等于一个内存页，4K 或 8K，具体取决于操作系统。
来自后端服务器响应的第一部分存储在单独的缓冲区中，其大小通过 [proxy_buffer_size](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffer_size) 指令进行设置，此部分通常是相对较小的响应headers，通常将其设置成小于默认值。

```nginx
location / {
    proxy_buffers 16 4k;
    proxy_buffer_size 2k;
    proxy_pass http://localhost:8088;
}
```

- 如果整个响应不适合存到内存里，则将其中的一部分保存到磁盘上的‎‎临时文件中‎‎。
- ‎[‎proxy_max_temp_file_size‎](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_max_temp_file_size)‎设置临时文件的最大值。
- ‎[‎proxy_temp_file_write_size‎](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_temp_file_write_size)‎设置一次写入临时文件的大小。

### 缓存（cache）

启用缓存后，nginx将响应保存在**磁盘**中，返回给客户端的数据**首先从缓存中获取**，这样子相同的请求不用每次都发送给后端服务器，<font color=#ff0000>减少</font>到后端**请求的数量**。

<img src="https://article.biliimg.com/bfs/article/0350b11125c768fb8cdf6a0350fa517cc5eee526.png" alt="image.png" style="zoom:50%;" />

- 启用缓存，需要在http上下文中使用 [proxy_cache_path](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path) 指令，定义缓存的本地文件目录，名称和大小。
- 缓存区可以被多个server共享，使用[proxy_cache](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache) 指定使用哪个缓存区。

```nginx
http {
    proxy_cache_path /data/nginx/cache keys_zone=mycache:10m;
    server {
        proxy_cache mycache;
        location / {
            proxy_pass http://localhost:8000;
        }
    }
}
```

缓存目录的文件名是 [proxy_cache_key](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_key) 的MD5值。

例如：`/data/nginx/cache/**c**/**29**/b7f54b2df7773722d382f4809d650**29c**`

- [proxy_cache_key](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_key) 默认设置如下：
	```nginx
	proxy_cache_key $scheme$proxy_host$uri$is_args$args;
	```
- 也可以自定义缓存的键，例如
	```nginx
	proxy_cache_key "$host$request_uri$cookie_user";
	```
- 缓存不应该设置的太敏感，可以使用[proxy_cache_min_uses](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_min_uses)设置相同的key的请求，访问次数超过指定数量才会被缓存。
	```nginx
	proxy_cache_min_uses 5;
	```
- 默认情况下，响应无限期地保留在缓存中。仅当缓存超过最大配置大小时，按照时间删除最旧的数据。

**示例**
```nginx
proxy_cache_path /var/cache/nginx/data keys_zone=mycache:10m;

server {

    listen 8001;
    server_name ruoyi.localhost;
    
    location / {
        #设置buffer
        proxy_buffers 16 4k;
        proxy_buffer_size 2k;
        proxy_pass http://localhost:8088;        

    }


    location ~ \.(js|css|png|jpg|gif|ico) {
        #设置cache
        proxy_cache mycache;
        proxy_cache_valid 200 302 10m;
        proxy_cache_valid 404      1m;
        proxy_cache_valid any 5m;

        proxy_pass http://localhost:8088;  
    }

    location = /html/ie.html {

        proxy_cache mycache;
        proxy_cache_valid 200 302 10m;
        proxy_cache_valid 404      1m;
        proxy_cache_valid any 5m;

        proxy_pass http://localhost:8088;  
    }

    location ^~ /fonts/ {

        proxy_cache mycache;
        proxy_cache_valid 200 302 10m;
        proxy_cache_valid 404      1m;
        proxy_cache_valid any 5m;

        proxy_pass http://localhost:8088;  
    }

}
```



## 负载均衡 

早期的网站流量和业务功能都比较简单，单台服务器就可以满足基本需求，但是随着互联网的发展，业务流量越来越大并且业务逻辑也越来越复杂，单台服务器的性能及**单点故障**问题就凸显出来了，因此需要多台服务器组成应用集群， 进行性能的水平扩展以及避免单点故障出现。 

- 应用集群：将同一应用部署到多台机器上，组成应用集群，接收负载均衡器分发的请求，进行业务处理并返回响应数据 
- 负载均衡器：将用户请求根据对应的负载均衡算法分发到应用集群中的一台服务器进行处理 
- 跨多个应用程序实例的负载平衡是一种常用技术，用于优化资源利用率、最大化吞吐量、减少延迟和确保容错配置。‎使用nginx作为非常有效的HTTP负载平衡器，将流量分配到多个应用程序服务器，可以提升Web应用程序的性能，提高扩展性和可靠性。

	<img src="https://article.biliimg.com/bfs/article/c59598ec03ca6dc641291f39151747eeb84567f7.png" alt="image.png" style="zoom:50%;" />

> 使用 `upstream`定义一组服务 。
> 注意：upstream 位于 http上下文中，与server 并列，不要放在server中。

```nginx
upstream ruoyi-apps {
    #不写，采用轮循机制
    server localhost:8080;
    server localhost:8088;
}

server {
  listen 8003;
  server_name ruoyi.loadbalance;
  location / {
    proxy_pass http://ruoyi-apps;
  }
}
```



**负载均衡策略**： 

|名称 | 说明                 |
|:-:|:-:|
| `轮询`     | 默认方式             |
|`weight`| 权重方式             |
|`ip_hash`| 依据ip分配方式       |
|`least_conn`| 依据最少连接方式     |
|`url hash`| 依据url分配方式      |
|`fair`| 依据响应时间方式     |

将下一个请求分配给活动连接数最少的服务器（较为空闲的服务器）。‎
```nginx
upstream backend {
    least_conn;
    server backend1.example.com;
    server backend2.example.com;
}
```

> 请注意，使用轮循机制或最少连接的负载平衡，每个客户端的请求都可能分发到不同的服务器。不能保证同一客户端将始终定向到同一服务器。‎

### ip-hash
**客户端的IP地址**将用作哈希键，来自同一个ip的请求会被转发到相同的服务器。
```nginx
upstream backend {
    ip_hash;
    server backend1.example.com;
    server backend2.example.com;
}
```

> 此方法可确保来自同一客户端的请求将始终定向到同一服务器，除非此服务器不可用。

### hash

通用hash，允许用户自定义hash的key，key可以是字符串、变量或组合。
- `$remote_addr`：客户端的IP地址
- `$http_cookie`：客户端的Cookie
- `$http_user_agent`：客户端的User Agent（浏览器或应用程序信息）
- `$http_referer`：客户端请求的来源URL
- `$request_uri`:请求的URI
例如，key可以是配对的源 IP 地址和端口，也可以是 URI，如以下示例所示：
```nginx
upstream backend {
    hash $request_uri consistent; # 使用请求的URI进行哈希计算，并将该哈希结果与后端服务器进行匹配。
    server backend1.example.com;
    server backend2.example.com;
}
```

> 请注意：基于 IP 的哈希算法存在一个问题，那就是当有一个上游服务器宕机或者扩容的时候，会引发大量的路由变更，进而引发连锁反应，导致大量缓存失效等问题。

`consistent`参数启用 ‎[‎ketama‎](http://www.last.fm/user/RJ/journal/2007/04/10/rz_libketama_-_a_consistent_hashing_algo_for_memcache_clients)‎ 一致哈希算法，如果在上游组中添加或删除服务器，只会重新映射部分键，从而最大限度地减少缓存失效。‎
假设我们基于 key 来做 hash，现在有 4 台上游服务器，如果 hash 算法对 key 取模，请求根据用户定义的哈希键值均匀分布在所有上游服务器之间。

<img src="https://article.biliimg.com/bfs/article/fb7853ca86dce90cca2b7c25021e0f908e46b0fd.png" alt="image.png" style="zoom:50%;" />

当有一台服务器宕机的时候，就需要重新对 key 进行 hash，最后会发现所有的对应关系全都失效了，从而会引发缓存大范围失效。

<img src="https://article.biliimg.com/bfs/article/27eadab6581e928a5ec940f548953713ca72dfeb.png" alt="image.png" style="zoom:50%;" />

<img src="https://article.biliimg.com/bfs/article/e59a2ee9f47898f79a481bece0e9359d019cada3.png" alt="image.png" style="zoom:50%;" />
### ‎随机‎‎ (random）

每个请求都将传递到随机选择的服务器。
two是可选参数，NGINX 在考虑服务器权重的情况下随机选择两台服务器，然后使用指定的方法选择其中一台，默认为选择连接数最少（least_conn‎）的服务器。
```nginx
upstream backend {
    random two least_conn;
    server backend1.example.com;
    server backend2.example.com;
    server backend3.example.com;
    server backend4.example.com;
}
```

### 权重

<img src="https://article.biliimg.com/bfs/article/05266acf6be16fea815a9d003d79f76604942834.png" alt="image.png" style="zoom:50%;" />
```nginx
upstream my-server {
    server performance.server weight=3;
    server app1.server;
    server app2.server;
}
```

如上所示，每 5 个新请求将按如下方式分布在应用程序实例中：3 个请求将定向到performance.server，一个请求将转到app1.server，另一个请求将转到app2.server。‎


### 健康检查

在反向代理中，如果后端服务器在某个周期内响应失败次数超过规定值，nginx会将此服务器标记为失败，并在之后的一个周期不再将请求发送给这台服务器。‎
通过[fail_timeout‎](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#fail_timeout)‎ 来设置检查周期，默认为10秒。
通过[max_fails‎](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#max_fails)来设置检查失败次数，默认为1次。‎
‎在以下示例中，如果NGINX无法向服务器发送请求或在30秒内请求失败次数超过3次，则会将服务器标记为不可用30秒。
```nginx
upstream backend {
  server backend1.example.com;
  server backend2.example.com max_fails=3 fail_timeout=30s; 
} 
```


# HTTPS配置

HTTPS 协议是**由HTTP 加上TLS/SSL 协议构建的可进行加密传输、身份认证的网络协议**，主要通过数字证书、加密算法、非对称密钥等技术完成互联网数据传输加密，实现互联网传输安全保护。

## 生成证书

```bash
openssl genrsa -des3 -out server.key 2048
openssl req -new -key server.key -out server.csr
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```

1. 首先，使用 `openssl genrsa` 命令生成一个 RSA 密钥对，并使用 Triple DES 对密钥进行加密，以保护密钥的安全性。接着，使用 `openssl req` 命令生成一个证书签名请求 (CSR)，用于向证书颁发机构 (CA) 申请颁发证书。该命令使用之前生成的 RSA 密钥对，并将证书请求保存到名为 `server.csr` 的文件中。
2. 然后，使用 `openssl x509` 命令使用之前生成的 RSA 密钥对和证书签名请求，生成一个自签名的 X.509 证书。该命令指定证书的有效期为 365 天，并将证书保存到名为 `server.crt` 的文件中。
3. 最后，将生成的 `server.key` 和 `server.crt` 文件分别用作 SSL/TLS 服务器的私钥和公钥证书，以保护服务器的通信安全。


> [!summary] nginx配置https的好处
> 1. nginx配置https后，后台多的情况下就不需要再挨个配置https了，因为nginx充当反向代理了。
> 2. 由于ssl用到了加密，它对cpu会有额外的消耗，nginx完成了**https的握手**，就不用把这个开销传递给后端了，可以降低后端的压力。
> 


**SSL/TLS 握手过程一般包括以下步骤**：
1. 客户端向服务器发送 SSL/TLS 握手请求。
2. 服务器向客户端发送 SSL/TLS 握手响应，其中包括服务器的 SSL/TLS 证书和公钥等信息。
3. 客户端验证服务器的证书，如果证书有效，则生成一个随机数并使用服务器的公钥加密，然后将加密后的随机数发送给服务器。
4. 服务器使用自己的私钥解密客户端发送的随机数，并使用客户端发送的随机数和服务器的随机数生成一个会话密钥。
5. 客户端和服务器使用会话密钥进行加密和解密，以保证通信的机密性。


## 配置ssl


```nginx
server{
  listen 8004 ssl;
 
  # 证书
  ssl_certificate /home/ssl/server.crt;
  # 密钥
  ssl_certificate_key /home/ssl/server.key;
  # 指定启用的 SSL/TLS 协议版本为 TLSv1、TLSv1.1 和 TLSv1.2
  ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
  # 指定要使用的加密算法为 HIGH
  ssl_ciphers         HIGH:!aNULL:!MD5;
  # 设置密码需要加上这句，cert.pass; 这个文件时自己创建的
  ssl_password_file   /home/ssl/cert.pass;  
  server_name ruoyi.https.loadbalance;
  location / {
    proxy_pass http://ruoyi-apps;
  }
}
```

## https优化

SSL 操作会消耗额外的 CPU 资源。CPU 占用最多的操作是 SSL 握手。有两种方法可以最大程度地减少每个客户端的这些操作数：

- 使保持活动连接能够通过一个连接发送多个请求
- 重用 SSL 会话参数以避免并行连接和后续连接的 SSL 握手

会话存储在工作进程之间共享并由 [ssl_session_cache](https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache) 指令配置的 SSL 会话缓存中。一兆字节的缓存包含大约 4000 个会话。默认缓存超时为 5 分钟。可以使用 [ssl_session_timeout](https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_timeout) 指令增加此超时。以下是针对具有 10 MB 共享会话缓存的多核系统优化的示例配置：

```nginx
ssl_session_cache   shared:SSL:10m;
ssl_session_timeout 10m;
```


# TCP反向代理

## stream

```nginx.conf
#HTTP代理
http {
  server {
    listen 8002;
    proxy_pass http://localhost:8080/;
  }
}

#TCP代理
stream {
  server {
    listen 13306;
    proxy_pass localhost:3306;
  }
}
```

## tcp负载均衡

```nginx.conf
stream {
  
  upstream backend-mysql {
  
    server localhost:3306;
    server localhost:3307;
    
    keepalive 8;
  }
  
  server {
    listen 13306;
    proxy_pass backend-mysql;
  }
}
```

使用`keepalive`定义连接池里空闲连接的数量。`keepalive_timeout` 默认60s。如果连接池里的连接空闲时间超过这个值，则连接关闭。

在最简单的 HTTP 实现中，客户端打开新连接，写入请求，读取响应，然后关闭连接以释放关联的资源。

<img src="https://article.biliimg.com/bfs/article/106b83d6f5f5c63ed7594e5a8601cd107095682c.png" alt="image.png" style="zoom:50%;" />


在客户端读取响应后，保持连接处于打开状态，因此可以将其重新用于后续请求。

<img src="https://article.biliimg.com/bfs/article/3ffd859f9eb99e420df452d9a6d9561fb7809435.png" alt="image.png" style="zoom:50%;" />
使用 [keepalive](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#keepalive) 指令启用从 NGINX Plus 到上游服务器的保持活动连接，定义在每个工作进程的缓存中保留的与上游服务器的空闲保持活动连接的最大数量。当超过此数字时，将关闭最近最少使用的连接。如果没有 keepalives，您将增加更多的开销，并且连接和临时端口都效率低下。

现代 Web 浏览器通常会打开 6 到 8 个保持连接。

# 重写(return和rewrite)

nginx有两个重写指令：`return`和`rewrite`

## return

服务端停止处理并将状态码status code返回给客户端

`return _code_ _URL_`

`return _code_ _text_`

`return _code_`

`return _URL_`

强制所有请求使用Https

错误写法

```nginx
server {
    listen 8003;
    server_name ruoyi.loadbalance;
    # nginx 重定向到客户端这个地址，客户端localhost是自己
    return 301 https://localhost:8004; 
    
}
```

正确写法

```nginx
server {

    listen 8003;
    server_name ruoyi.loadbalance;

    return 301 https://192.168.56.105:8004;
}
```

## 转发和重定向

转发是服务端行为，重定向是客户端行为。

#### 转发
发向代理proxy_pass属于转发，浏览器的访问栏输入的地址不会发生变化。
<img src="https://article.biliimg.com/bfs/article/6321bc4fbee6da58ee50f5e2adaf613337776f81.png" alt="image.png" style="zoom:50%;" />


#### 重定向

return，rewrite属于重定向，在客户端进行。浏览器的访问栏输入的地址会发生变化。

<img src="https://article.biliimg.com/bfs/article/1a69ea7eca7d3a327af49f4239fa3b9f3dc010ae.png" alt="image.png" style="zoom:50%;" />

域名迁移，不让用户收藏的链接或者搜索引擎的链接失效

将请求从 www.old-name.com **old-name.com** 永久重定向到 **www.new-name.com，包含http和https请求 **

```nginx
server {
    listen 80;
    listen 443 ssl;
    server_name www.old-name.com old-name.com;
    return 301 $scheme://www.new-name.com$request_uri;
}
```

由于捕获了域名后面的 URL 部分，因此，如果新旧网站之间存在一对一的页面对应关系（例如，www.new-name.com/about  具有与www.old-name.com/abou 相同的基本内容），则此重写是合适的。如果除了更改域名之外还重新组织了网站，则通过省略以下内容，将所有请求重定向到主页可能会更安全

```nginx
server {
    listen 80;
    listen 443 ssl;
    server_name www.old-name.com old-name.com;
    return 301 $scheme://www.new-name.com;
}
```


添加www
```nginx
# add 'www'
server {
    listen 80;
    listen 443 ssl;
    server_name domain.com;
    return 301 $scheme://www.domain.com$request_uri;
}
```

#### 状态码

- 2xx 成功
- 3xx 表示重定向
	- 301 永久重定向
	- 302 临时重定向
- 4xx 请求地址出错
	- 403 拒绝请求
	- 404 请求找不到
- 5xx 服务器内部错误

## rewrite

‎如果指定的正则表达式与请求 URI 匹配，则 URI 将按照字符串中的指定进行更改。指令按其在配置文件中出现的先后顺序执行。

```nginx
server {
    # ...
    rewrite ^(/download/.*)/media/(\w+)\.?.*$ $1/mp3/$2.mp3 last;
    rewrite ^(/download/.*)/audio/(\w+)\.?.*$ $1/mp3/$2.ra  last;
    return  403;
    # ...
}
```


上面是使用该指令的示例 NGINX 重写规则。它匹配以字符串 `/download` 开头的 URL，然后在路径后面的某个位置包含 `/media/` 或 `/audio/` 目录。它将这些元素替换为 `/mp3/`，并添加相应的文件扩展名，`.mp3` 或 `.ra`。和 变量捕获未更改的路径元素。例如，`/download/cdn-west/media/file1` 变成了 `/download/cdn-west/mp3/file1.mp3`。如果文件名上有扩展名（如 `.flv`），则表达式会将其剥离，并将其替换为 `.mp3`。

如果字符串包含新的请求参数，则以前的请求参数将追加到这些参数之后。如果不需要这样做，则在替换字符串的末尾放置一个问号可以避免附加它们，例如：*replacement*

```nginx
rewrite ^/users/(.*)$ /show?user=$1? last;
```



### last与break
`last`：如果当前规则不匹配，停止处理后续rewrite规则，使用重写后的路径，重新搜索location及其块内指令
`break`:如果当前规则不匹配，停止处理后续rewrite规则，执行{}块内其他指令

### 不使用last和break
在`root /home/AdminLTE-3.2.0/pages`下创建一个1.txt，里面内容是`this is a file`

```nginx
server {

    listen 8000;
    server_name nginx-dev;

    rewrite_log on;

    location / {
        rewrite ^/old/(.*) /new/$1;
        rewrite ^/new/(.*) /pages/$1;
        #根目录
        root /home/AdminLTE-3.2.0;
        #首页
        index index.html index2.html index3.html;
    }

    location  /pages/1.txt {
        return 200 "this is rewrite test!";
    }

}
```

默认按顺序执行。
访问 http://192.168.56.105:8000/old/1.txt
结果：this is rewrite test!
日志：

```latex
[notice] 26837#26837: *1181 "^/old/(.*)" matches "/old/1.txt", client: 192.168.56.1, server: nginx-dev, request: "GET /old/1.txt HTTP/1.1", host: "192.168.56.105:8000"
[notice] 26837#26837: *1181 rewritten data: "/new/1.txt", args: "", client: 192.168.56.1, server: nginx-dev, request: "GET /old/1.txt HTTP/1.1", host: "192.168.56.105:8000"
[notice] 26837#26837: *1181 "^/new/(.*)" matches "/new/1.txt", client: 192.168.56.1, server: nginx-dev, request: "GET /old/1.txt HTTP/1.1", host: "192.168.56.105:8000"
[notice] 26837#26837: *1181 rewritten data: "/pages/1.txt", args: "", client: 192.168.56.1, server: nginx-dev, request: "GET /old/1.txt HTTP/1.1", host: "192.168.56.105:8000"
```

### 使用break
访问 http://192.168.56.105:8000/old/1.txt
1. 匹配到了`rewrite ^/old/(.*) /new/$1`
2. break指令不执行后续的rewrite规则,以新的/new/1.txt路径去执行块内的其他指令
3.  去`root`目录下寻找文件, 由于不再村`/home/AdminLTE-3.2.0/new/1.txt`这个文件，返回404

```nginx
server {

    listen 8000;
    server_name nginx-dev;

    rewrite_log on;

    location / {
        rewrite ^/old/(.*) /new/$1 break;
        rewrite ^/new/(.*) /pages/$1;
        #根目录
        root /home/AdminLTE-3.2.0;
        #首页
        index index.html index2.html index3.html;
    }

    location  /pages/1.txt {
        return 200 "this is rewrite test!";
    }

}
```

访问 http://192.168.56.105:8000/old/1.txt
结果：
<img src="https://article.biliimg.com/bfs/article/4e7b8c9c5d9a795904b832f5f08a5060662de675.png" alt="image.png" style="zoom:50%;" />
访问日志：
```latex
[notice] 26772#26772: *1179 "^/old/(.*)" matches "/old/1.txt", client: 192.168.56.1, server: nginx-dev, request: "GET /old/1.txt HTTP/1.1", host: "192.168.56.105:8000"
[notice] 26772#26772: *1179 rewritten data: "/new/1.txt", args: "", client: 192.168.56.1, server: nginx-dev, request: "GET /old/1.txt HTTP/1.1", host: "192.168.56.105:8000"
[error] 26772#26772: *1179 open() "/home/AdminLTE-3.2.0/new/1.txt" failed (2: No such file or directory), client: 192.168.56.1, server: nginx-dev, request: "GET /old/1.txt HTTP/1.1", host: "192.168.56.105:8000"
```

### 使用last

访问 http://192.168.56.105:8000/old/1.txt
1. 匹配到了`rewrite ^/old/(.*) /new/$1`
2. `**last**`指令不执行后续的rewrite规则,以新的`/new/1.txt`路径去匹配location
3. 先匹配到`location /`, 有匹配到location里的`rewrite ^/new/(.*) /pages/$1`规则，重定向到`/pages/1.txt`
4. 匹配到了`location /pages/1.txt` ，于是返回了`this is rewrite test!`

```nginx
server {

    listen 8000;
    server_name nginx-dev;

    rewrite_log on;

    location / {
        rewrite ^/old/(.*) /new/$1 last;
        rewrite ^/new/(.*) /pages/$1;
        #根目录
        root /home/AdminLTE-3.2.0;
        #首页
        index index.html index2.html index3.html;
    }

    location  /pages/1.txt {
        return 200 "this is rewrite test!";
    }

}
```

访问 http://192.168.56.105:8000/old/1.txt
结果：`this is rewrite test!`
日志：

```nginx
[notice] 26969#26969: *1185 "^/old/(.*)" matches "/old/1.txt", client: 192.168.56.1, server: nginx-dev, request: "GET /old/1.txt HTTP/1.1", host: "192.168.56.105:8000"
[notice] 26969#26969: *1185 rewritten data: "/new/1.txt", args: "", client: 192.168.56.1, server: nginx-dev, request: "GET /old/1.txt HTTP/1.1", host: "192.168.56.105:8000"
[notice] 26969#26969: *1185 "^/old/(.*)" does not match "/new/1.txt", client: 192.168.56.1, server: nginx-dev, request: "GET /old/1.txt HTTP/1.1", host: "192.168.56.105:8000"
[notice] 26969#26969: *1185 "^/new/(.*)" matches "/new/1.txt", client: 192.168.56.1, server: nginx-dev, request: "GET /old/1.txt HTTP/1.1", host: "192.168.56.105:8000"
[notice] 26969#26969: *1185 rewritten data: "/pages/1.txt", args: "", client: 192.168.56.1, server: nginx-dev, request: "GET /old/1.txt HTTP/1.1", host: "192.168.56.105:8000"
```


# 其他常见指令

## gzip压缩

压缩响应通常会显著减小传输数据的大小。但是，由于压缩发生在运行时，因此它还会增加相当大的处理开销，从而对性能产生负面影响。NGINX在将响应发送到客户端之前执行压缩，但如果后端服务器已经对内容进行了压缩，则nginx不会再压缩。
若要启用压缩，请在参数中包含 `gzip` 指令。

```nginx
gzip on; 
gzip_types text/plain application/xml; 
gzip_min_length 1000; 
```

默认情况下，NGINX仅使用压缩MIME类型是text/html的响应。若要使用其他 MIME 类型压缩响应，可以使用 `gzip_types` 指令并列出其他类型。
若要指定压缩响应的最小长度，请使用 `gzip_min_length `指令。默认值为 20 个字节（此处调整为 1000)。

## sendfile

默认情况下，NGINX处理文件传输本身，并在发送之前将文件复制到缓冲区中。启用 `sendfile` 指令可消除将数据复制到缓冲区的步骤，直接将一个文件复制到另一个文件。
启用sendfile，类似Java中的零拷贝（zero copy）
```nginx
location /download {
  sendfile           on;
  tcp_nopush         on; 
  #... 
} 
```

将 [tcp_nopush](https://nginx.org/en/docs/http/ngx_http_core_module.html#tcp_nopush) 指令与 [sendfile](https://nginx.org/en/docs/http/ngx_http_core_module.html#sendfile) 指令一起使用。这使NGINX能够在获得数据块后立即在一个数据包中发送HTTP响应标头。

## try_files

`try_files`指令可用于检查指定的文件或目录是否存在;如果不存在，则重定向到指定位置。
如下，如果原始URI对应的文件不存在，NGINX将内部重定向到`/www/data/images/default.gif`

```nginx
server {
    root /www/data;

    location /images/ {
        try_files $uri /images/default.gif;
    }
}
```

最后一个参数也可以是状态代码（状态码之前需要加上等号）。
在下面的示例中，如果指令的所有参数都无法解析为现有文件或目录，则会返回404错误。

```nginx
location / {
    try_files $uri $uri/ $uri.html =404;
}
```

在下一个示例中，如果原始 URI 和附加尾随斜杠的 URI 都没有解析到现有文件或目录中，则请求将重定向到命名位置，该位置会将其传递到代理服务器。
```nginx
location / {
    try_files $uri $uri/ @backend;
}

location @backend {
    proxy_pass http://backend.example.com;
}
```

## error_page

为错误指定显示的页面。值可以包含变量。
```nginx
error_page 404             /404.html; 
error_page 500 502 503 504 /50x.html;
```

# 推荐写法及注意事项

## 推荐写法

### 重复的配置可继承自父级

例如：

```nginx
server {
  server_name www.example.com;
  
  location / {
    root /var/www/nginx-default/;
    # [...]
  }
  location /foo {
    root /var/www/nginx-default/;
    # [...]
  }
  location /bar {
    root /some/other/place;
  # [...]
  }
}
```

推荐写法

```nginx
server {
    server_name www.example.com;
    root /var/www/nginx-default/;
    
    location / {
        # root继承父级配置
        # [...]
    }
    location /foo {
        # root继承父级配置
        # [...]
    }
    location /bar {
        # 覆盖
        root /some/other/place;
        # [...]
    }
}
```

这样在添加新的location时，可以避免重复配置。

### 不要将所有请求都代理到后端服务器

不推荐：
```nginx
location / {
    proxy_pass http://localhost:8088;        
}
```

考虑到很多请求是访问静态内容（如图片，css，javascript等文件）,可以使用缓存或者配置静态目录来减少发送到后端的请求数量，这样可以减小后端服务器的开销
```nginx
server {

    listen 8002;
    server_name ruoyi.tomcat;
    root /home/www/static;

    location / {
        try_files $uri $uri/ @proxy;
    }

    location @proxy {
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        proxy_pass http://localhost:8080;
    }
  
}
```

### 若非必要，不要缓存动态请求，只缓存静态文件

nginx关于缓存的指令非常多

[proxy_cache](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache)  
[proxy_cache_background_update](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_background_update)  
[proxy_cache_bypass](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_bypass)  
[proxy_cache_convert_head](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_convert_head)  
[proxy_cache_key](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_key)  
[proxy_cache_lock](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_lock)  
[proxy_cache_lock_age](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_lock_age)  
[proxy_cache_lock_timeout](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_lock_timeout)  
[proxy_cache_max_range_offset](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_max_range_offset)  
[proxy_cache_methods](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_methods)  
[proxy_cache_min_uses](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_min_uses)  
[proxy_cache_path](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path)  
[proxy_cache_purge](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_purge)  
[proxy_cache_revalidate](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_revalidate)  
[proxy_cache_use_stale](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_use_stale)  
[proxy_cache_valid](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_valid)  
[proxy_no_cache](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_no_cache)

由于nginx服务端缓存非常复杂，在使用缓存的时候，我们要清楚的知道在什么条件下，哪些文件会被缓存。

在配置文件中，最好能够清晰的指定哪些文件使用缓存。

### 检查文件是否存在使用try_files代替if -f

不推荐用法：

```nginx
server {
    root /var/www/example.com;
    location / {
        if (!-f $request_filename) {
            break;
        }
    }
}
```

推荐用法：
```nginx
server {
    root /var/www/example.com;
    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

### 在重写路径中包含http://或https://
```nginx
#推荐写法
rewrite ^ http://example.com permanent; 

#不推荐的写法
rewrite ^ example.com permanent; 
```

### 保持重写规则简单干净

例如下面的这个例子可以变得更简单易懂。

```nginx
#复杂的写法
rewrite ^/(.*)$ http://example.com/$1 permanent; 

#简单有效的写法
rewrite ^ http://example.com$request_uri? permanent; 
return 301 http://example.com$request_uri; 
```

## 注意事项

### 1.正确的配置未生效，请清除浏览器缓存

当你确定修改的配置的正确的，但是未生效，请清除浏览器缓存或者禁用浏览器缓存。

### 2.在HTTPS中不启用 SSLv3

由于 SSLv3 中存在 POODLE 漏洞，建议不要在启用了 SSL 的站点中使用 SSLv3。您可以使用以下行非常轻松地禁用 SSLv3，并仅提供 TLS 协议：

```nginx
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
```

### 3.不要将 root 目录配置成 `/`或 `/root`。

错误用法

```nginx
server {
  #错误用法
  root /;
  
  location /project/path {
    #错误用法
    root /root;
  }
}
```

### 4.谨慎使用chmod 777

这可能是解决问题最简单的方式，同时也说明，你没有真的弄清楚哪里出了问题。

可以使用`namei -om /path/to/check`显示路径上的所有权限，并找到问题的根本原因。

### 5.不要将部署的项目拷贝到默认目录下

升级或更新nginx的时候，默认目录可能被覆盖。

下面是我连接公司服务器的过程，是在终端进行的

```
chenjiahui@chenjiahuideMacBook-Pro ~ % ssh chenjiahui10@relay.corp.kuaishou.com

********************************************************************************
                      欢迎使用快手自研堡垒机Relay(Relay04升级版)
                    登录密码为：邮箱密码+token(快手OTP)
********************************************************************************
                        用户手册: http://ksurl.cn/XHdmGaOQ
                        讲解视频: http://ksurl.cn/9DHY1Dmg

                         反馈问题:KIM扫码加入Relay用户群
                   ███████████████████████████████████████
                   █ ▄▄▄▄▄ █▄▄▄ ▀   █▄ ▄ ▄ █▀  █▀█ ▄▄▄▄▄ █
                   █ █   █ ██▄▀ █▄ ▀█ █▄▀ ███  ███ █   █ █
                   █ █▄▄▄█ ██▀▄ ▄ ▀▄█ ▄  █ ▀▀▄████ █▄▄▄█ █
                   █▄▄▄▄▄▄▄█ ▀▄█ █ █▄▀▄█▄█▄▀▄▀ ▀▄█▄▄▄▄▄▄▄█
                   █▄ ▀ ▄█▄█▀ ▀█▄▄ ▄▄▀  ███▀ ▀█ ▄█▄▀▀▀██▄█
                   █▄  ▀█ ▄ ▄▀ █▄█ ▀██  █▄  ▀█▄▄ ▄▄▄▄▀ ▄▄█
                   █ ▄▀▀▄█▄█ ▀██▀▄▄███ █ ▀  ▄▀▄ █ ▄▄▀▀████
                   ███ ▄ ▄▄▀▀▀▀█▀▀ ▄▀▀ █  ██ ██▄▄█▄▄   █▄█
                   █▀▀ ▀█▄▄  ██ ▄██▄█▀▀▄▄█ █▀▄   ▄▄▄█▄▄▄██
                   █▀█▀▄▀█▄▄█▀ ▀▄ ▄▀  ██▄ █  ▀█ ▀▀▄█▄ ▄  █
                   █▄ ▄▀▄▀▄█▄▀█▄▀ ██ █  █▀▄▀▄█  ▀▀▄█ █▀▄██
                   █ ▀██ ▄▄██▄▄ ▀ █ ▀▄█▄▀ ▄ ▀██ █▄ ▀▄  █ █
                   █ █▀  ▀▄ ▄██▄▄ ▀█▄█ █▄▀  ▀▀▄  █▄▄▀ █▄██
                   █ ▄ ▀ ▀▄▀  ▀ ▄▄██▀▀█▄ ▀█▀▄██▄█▄▄██▀  ▄█
                   █▄█▄██▄▄█ ██▀▀▀ ▄▀▀▀  █ ▄▀▄   ▄▄▄ ▄  ██
                   █ ▄▄▄▄▄ ██▄▀▄▀█▀█▄▀█▄▀▀▀▄█▀▄█ █▄█ ▀▄▀ █
                   █ █   █ █ ▀▄█▄▄ ▀▄ ▄██▄▄▀▀█   ▄ ▄▄ ▀▀ █
                   █ █▄▄▄█ █▀▄▀█▄█ ███▀█▄   ▄▄  █▀▀  ▄▀▀ █
                   █▄▄▄▄▄▄▄█▄▄███▄▄▄▄█▄█▄█▄▄▄██▄▄▄▄█████▄█
********************************************************************************
(chenjiahui10@relay.corp.kuaishou.com) 已发送KIM登录确认信息[kim://thread?id=252263812734&type=0],KIM确认后,回车直接登录,也可以输入邮箱密码+OTP登录:

[chenjiahui10@relay](输入h查看帮助)# ssh public-bjzey-rs-kis111.idczw.hb1.kwaidc.com -l chenjiahui10 -p 22
ssh chenjiahui10@public-bjzey-rs-kis111.idczw.hb1.kwaidc.com -p22
Last login: Tue Jul  2 15:05:23 2024 from 10.30.2.29
chenjiahui10@public-bjzey-rs-kis111.idczw.hb1.kwaidc.com:~
$
```

第一个是ssh命令但是进入的一个堡垒机选择页面，我已经免密码了，ssh-keygen -t ed25519了，/Users/chenjiahui/.ssh/id_ed25519.pub 和/Users/chenjiahui/.ssh/id_ed25519 公钥已经在服务器上了，但是你可以看到KIM确认后,回车直接登录，需要我在手机上的kim点一下，然后回车才行,我想问下这个vscode怎么配置？
\

我还发现一个问题
```sh
(chenjiahui10@relay.corp.kuaishou.com) 已发送KIM登录确认信息[kim://thread?id=252263812734&type=0],KIM确认后,回车直接登录,也可以输入邮箱密码+OTP登录:
(chenjiahui10@relay.corp.kuaishou.com) 已发送KIM登录确认信息[kim://thread?id=252263812734&type=0],KIM确认后,回车直接登录,也可以输入邮箱密码+OTP登录:
(chenjiahui10@relay.corp.kuaishou.com) 已发送KIM登录确认信息[kim://thread?id=252263812734&type=0],KIM确认后,回车直接登录,也可以输入邮箱密码+OTP登录:
chenjiahui10@relay.corp.kuaishou.com: Permission denied (keyboard-interactive).
```

三次在没有手机点kim后就会Permission denied，我想问下这个vscode怎么配置？