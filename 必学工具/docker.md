# Docker简介


Docker是一个用于 **构建(build) 运行(run) 传送(share)** 应用程序的平台,一个开源的**应用==容器==引擎**,可以将我们的应用程序打包成一个个的**集装箱**,这个小鲸鱼会把它运送到任何需要的地方

<img src="https://article.biliimg.com/bfs/article/c55a233598eb1412a63a573a793dcfc691273256.gif" alt="image.png" style="zoom:50%;" />

<img src="https://article.biliimg.com/bfs/article/cdb095e69304aa090bde29d0c2edba103d86c82c.png" alt="image.png" style="zoom:50%;" />

## 为什么要使用Docker 

<img src="https://article.biliimg.com/bfs/article/92a005115b3227544cee874d5686db96b51e7500.png"  style="zoom:40%;" />

上面一个网站的开发环境,前端使用vue框架来构建网站的界面,后端使用Java的SpringBoot微服务框架,使用MySQL数据库来存储数据,没用docker就需要安装上面那么多的环境,别说是开发和测试又得重新配

<img src="https://article.biliimg.com/bfs/article/2e9e308aab25967c1621891e86feeb4db629c7f0.gif" alt="image.png" style="zoom:40%;" />


## Docker和虚拟机的区别 

<img src="https://article.biliimg.com/bfs/article/da8f7acc5ead635df2dd51bd22dfe0f509ba13cd.png" alt="image.png" style="zoom:40%;" />

- 虚拟化技术是一种将**物理资源**虚拟为**多个逻辑资源**的技术,它可以将一台物理服务器虚拟成多个逻辑服务器
- 每个逻辑服务器都有自己的操作系统和cpu、内存、硬盘和网络接口等等,它们之间是**完全隔离**的,可以独立运行
- 虚拟机在一定程度上实现了**资源的整合**,可以将一台服务器的**计算能力、存储能力、网络资源**分配给多个逻辑服务器,实现多台服务器的功能。


<img src="https://article.biliimg.com/bfs/article/3f8545f8f67e1ff5bfbf0cecee8261d7cbc96f96.png" alt="image.png" style="zoom:40%;" />


虚拟机的缺点:
- 每台虚拟机都需要占用大量的资源,比如CPU 内存 硬盘网络等等，而且启动速度非常慢
- 大部分情况下,其实我们的一台服务器上,只要运行一个主要**对外提供服务的应用程序**就可以了,并不需要一个完整操作系统所提供的功能 
- 上述的网站只需要一个web服务器,而不需要其他资源,比如操作系统内核、各种系统服务等等



<img src="https://article.biliimg.com/bfs/article/74a59b76b76c44ebcfb05674252744c87328a222.png" alt="image.png" style="zoom:40%;" />


容器:
- Docker只是容器的一种实现,是一个**容器化**的**解决方案**和**平台**,容器是**应用层的抽象**,容器一种**虚拟化技术**和虚拟机类似,也是一个独立的环境,可以在这个环境中运行应用程序 
- 和虚拟机不同的是,容器不需要运行一个完整的操作系统,而是使用**宿主机的操作系统**,启动速度非常快,容器在linux内核层面(使用 [Cgroups](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Resource_Management_Guide/ch01.html)(实现容器资源的使用) 和 [namespaces](https://lwn.net/Articles/528078/)(将容器中的进程划定在容器空间中相互隔离))来实现**进程间的隔离**,容器在主机操作系统上的用户空间中作为**独立进程**运行。
- 因为需要的资源更少,所以可以在一个物理服务器上运行更多的容器,可以更加充分的利用服务器的资源,减少资源的闲置和浪费,
- 一个物理服务器上可以运行几个虚拟机,但是却可以运行上百个容器

镜像:是一个**只读的模板**,它可以用来**创建容器**,容器是docker的**运行实例**,它提供了一个**独立的可移植的环境**,镜像和容器的关系类似于java中的类与实例

仓库:Docker仓库是用来**存储Docker镜像的地方**,最流行和最常用的仓库就是`Dockerhub`,是一个公共的dokcer仓库,用来集中存储和管理Docker镜像,可以下载各种镜像,实现**镜像的共享和复用**了

# Docker基本原理和概念

<img src="https://article.biliimg.com/bfs/article/d5235bc05f0ca231845e666e44fa427f8de6e9cf.png" alt="image.png" style="zoom:40%;" />
- Docker是使用`Client-Server`架构模式 
- `Docker Client`和`Docker Daemon`之间通过`Socket`或者 [[spring#REST风格|RESTful API]] 进行通信 
- `Docker Daemon`是服务端的守护进程,负责管理Docker的各种资源
- `Docker Client`负责向`Docker Daemon`发送请求,`Docker Daemon`接收到请求之后进行处理,然后将结果**返回**给`Docker Client`

## docker的优势
1. 一键部署，开箱即用
	- 容器使用基于**image镜像**的部署模式，image中包含了运行应用程序所需的一切：代码、运行时、系统工具、系统库和配置文件。
	- 无论是单个程序还是多个程序组成的复杂服务，或者分布式系统，都可以使用`docker run`或`docker compose up`命令一键部署，省去了大量搭建、配置环境、调试和排查错误的时间。

2. 一次打包，到处运行
	- Docker为容器创建了行业标准，使容器成为了软件交付过程中的一种**标准化格式**，将软件打包成容器镜(image)，能够使软件在不同环境下运行一致，应用程序可以快速可靠地从一个环境移植到另外一个环境，并确保在所有的**部署目标**(例如开发、测试、生产环境)上都按预期运行，从而避免了“在我电脑上是好的，怎么到你那却不能用了？”的问题。

## 容器化和Dockerfile 


<img src="https://article.biliimg.com/bfs/article/3733e6603f3f12d597d942a292686e4a14b1358a.png" alt="image.png" style="zoom:40%;" />

容器化就是应用程序打包成容器,然后在容器中运行应用程序的过程 
1. 创建一个`Dockerfile`,来告诉Docker构建应用程序镜像所需要的**步骤**和**配置** 
	- `Dockerfile`是一个文本文件,里面包含了**一条条的指令**,用来告诉Docker如何来构建镜像,这个镜像包括应用程序执行的所有命令(各种依赖、配置环境\运行程序所需要的所有内容)
	- 包含下面这些内容
		- 精简版的操作系统,比如`Alpine`
		- 应用程序的运行时环境(比如NodeJS Java Python)
		- 应用程序(比如SpringBoot打包好的jar包)
		- 应用程序的第三方依赖库或者包
		- 应用程序的配置文件 环境变量 
2. 使用`Dockerfile`构建镜像 
3. 使用镜像创建和运行容器 
# 安装docker

## linux下安装Docker

安装环境：CentOS 7.3+
如果之前安装了旧版docker，请先删除。
```bash
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

安装仓库

```bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

安装docker engine
```bash
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin #分别是dock服务端、客户端、容器、容器编排工具
```

客户端发出的指令交给服务端，服务端并不直接创建容器，而是交给`containerd.io`创建容器，销毁运行容器 

启动docker，运行hello world查看是否成功
```bash
sudo systemctl start docker
sudo docker run hello-world
```

配置国内镜像仓库地址：
新建`/etc/docker/daemon.json`文件，输入如下内容：

```bash
{
  "registry-mirrors": [
    "https://registry.docker-cn.com",
    "http://hub-mirror.c.163.com",
    "https://fsp2sfpr.mirror.aliyuncs.com/"
  ]
}
```

然后重启，配置开机启动

```bash
sudo systemctl restart docker
sudo systemctl enable docker
sudo systemctl enable containerd
```

关闭docker

```bash
sudo systemctl stop docker。
```

# docker 命令

## docker run 开箱即用

- 我们可以使用`docker pull`命令将镜像拉取到本地
	```bash
	docker pull nginx  #这里的nginx是镜像名称
	```

	镜像包含许多版本，称之为**tag**,我们可以在镜像名称后面加:xx.xx 指定版本
	```bash
	docker pull nginx:1.22
	# 不写tag默认使用的latest 最新版本的镜像
	docker pull nginx:mainline-alpine3.18-slim  
	# 建议镜像加tag
	```

- 查看镜像
	```bash
	docker images
	```

	我们来运行一下这个nginx镜像

	```bash
	docker run --name some-nginx -d -p 8080:80 nginx:1.22 
	# docker run:这是运行docker容器的命令
	# --name some-nginx:这是给容器指定一个名称
	# -d:这是一个选项,表示在后台运行容器
	# -p 8080:80:这也是一个选项，用于将宿主机的8080端口映射到容器的80端口。这样，当你访问主机的8080端口时，就会被转发到容器的80端口。 
	# nginx:1.2: 这是要运行的镜像的名称和标签。 
	
	-p 8080-8090:8080-8090 # 公开端口范围，前后必须对应 
	-p 192.168.56.106:8080:80 # 如果宿主机有多个ip,可以指定绑定到哪个ip 
	
	```

- 列出**正在运行**的容器 

	```bash
	docker ps 
	docker container ls
	# CONTAINER ID   IMAGE        COMMAND      CREATED     STATUS      PORTS       NAMES
	
	docker ps -a # 查看所用的容器,包括已经退出的
	```

- 查看容器日志
	```bash
	docker logs [CONTAINER ID|NAMES] 
	```

- 停止容器

	```bash
	docker stop [CONTAINER ID|NAMES] 
	# 这时候docker ps命令相应的容器就查不到
	```

- 重启容器 
	
	```bash
	docker restart [CONTAINER ID|NAMES] 
	```

- 删除容器

	```bash
	docker rm [CONTAINER ID|NAMES] 
	docker container rm [CONTAINER ID|NAMES] 
	```


- 创建一个mysql镜像

	```bash
	docker run \
	 -p 3306:3306 \
	 --name mysql \
	 -v $PWD/conf:/etc/mysql/conf.d \
	 -v $PWD/logs:/logs \
	 -v $PWD/data:/var/lib/mysql \
	 -e MYSQL_ROOT_PASSWORD=701121 \
	 --privileged \
	 -d \
	 mysql:5.7.25
	# -v 是一个选项 $PWD代表当前目录,将容器的/etc/mysql/conf.d映射 宿主机当前目录/conf
	# -e 环境变量选项  MYSQL_ROOT_PASSWORD=701121 设置mysql密码
	# --privileged 这是一个选项，表示该容器被授予了特权模式，具有对宿主系统的完全访问权限。
	# dock run 会先去本地查找镜像,没有就会去镜像开源网站找
	```

	但是这样的mysql镜像是无法创建命令行客户端的

	```bash
	docker run -it --rm mysql:5.7.25 mysql -hsome.mysql.host -uroot -p
	# -it: 使用交互式终端,与容器进行交互
	# --rm:当容器退出时,自动删除容器
	# -h[mysql的ip]
	# -u[用户名]
	```

上面命令启动另一个 `mysql` 容器实例，并针对原始 `mysql` 容器运行 `mysql` 命令行客户端，允许您对数据库实例执行SQL语句：
容器的mysql的ip地址可以通过`docker inspect`查询到
`docker inspect` 命令用于获取有关 Docker 容器、镜像、网络或卷的详细信息。它提供了容器或其他 Docker 对象的元数据和配置信息。
当运行 `docker inspect [CONTAINER ID|NAMES]` 命令时，命令会返回一个 JSON 格式的输出，其中包含了选定对象的所有信息。

这些信息可能包括：
- 容器的名称、ID、状态和资源利用情况。
- 容器的网络设置，如 IP 地址、端口映射和网络连接。
- 容器的挂载点和卷的配置。
- 镜像的名称、ID、标签和构建信息。
- 镜像的大小和层次结构。
- 网络的名称、驱动程序和相关设置。
- 卷的名称、路径和权限设置等。
        
另外`docker inspect`出现的信息还可以进行格式化

```bash
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' my-container
# 172.17.0.3
```


- 以交互模式进入容器(开启容器中的终端)

	```bash
	docker exec -it [container] bash  
	docker attach [container] 
	```

# docker网络

## 默认网络

```bash
docker network ls 
# NETWORK ID     NAME      DRIVER    SCOPE
# fabce6787e9a   bridge    bridge    local
# f8e8ef3cf23c   host      host      local
# 4b4b97c4b4c3   none      null      local
```

docker 会自动创建三个网络,`bridge`,`host`,`none`

- bridge**桥接网络**
	- 如果不指定，新创建的容器默认将连接到bridge网络。
	- 默认情况下，使用bridge网络，宿主机可以ping通容器ip，容器中也能ping通宿主机。
	- 容器之间只能通过**IP地址相互访问**，由于容器的ip会随着启动顺序发生变化，因此不推荐使用ip访问。
	```bash
	curl 172.17.0.3:80
	<!DOCTYPE html>
	<html>
	<head>
	...
	# 宿主机可以通过ping通容器ip 0000000000000 
	```

	通过docker exec -it 命令进入的容器终端没有常见的Linux命令，所以用[[零散知识点#BusyBox|busybox]],来验证容器之间可以通过IP地址访问

	```bash
	docker run -it --rm busybox
	/ # ping 172.17.0.2
	PING 172.17.0.2 (172.17.0.2): 56 data bytes
	64 bytes from 172.17.0.2: seq=0 ttl=64 time=0.281 ms
	64 bytes from 172.17.0.2: seq=1 ttl=64 time=0.113 ms
	```


- host (慎用，可能会有**安全问题**)
	- 容器与宿主机**共享网络**，不需要映射端口即可通过宿主机IP访问。(-p选项会被忽略)
	- 主机模式网络可用于**优化性能**，在容器需要处理大量端口的情况下，它不需要**网络地址转换**(NAT)，并且不会为每个端口创建“**用户空间代理**”


- none
禁用容器中所用**网络**，在启动容器时使用。

## 用户自定义网络

|步骤 |命令|
|:-:|:-:|
| 创建用户自定义网络 | `docker network create my-net` |
| 将已有容器连接到此网络 | `docker network connect my-net db-mysql` |
| 创建容器时指定网络 | `docker run -it --rm --network my-net mysql:5.7 mysql -h**db-mysql** -uroot -p` |

在用户自定义网络上，容器之间可以通过容器名进行访问。
用户自定义网络使用Docker的**嵌入式DNS服务器**将容器名解析成IP。


```bash
# 创建自定义网络
docker network create my-net
ec160819211e4e81b9a5bb22f77485e821ae5070533a8195b9be4adbce40b7cd

# 将这个网络连接到mysql容器
docker network connect my-net mysql
```

再次查看mysql的具体信息会发现多了一个网络
```json
"Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "fabce6787e9a05a2e1194db8764ea830db08866d21fdf5f1648cca034a27256b",
                    "EndpointID": "b45d3f597811a0b5947a2a453c338f18ccb8c380556ce19056762e27305c4069",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                },
                "my-net": {   //这里多了一个自定义的网络
                    "IPAMConfig": {},
                    "Links": null,
                    "Aliases": [
                        "dd28af4fe606"
                    ],
                    "NetworkID": "ec160819211e4e81b9a5bb22f77485e821ae5070533a8195b9be4adbce40b7cd",
                    "EndpointID": "4b384adf489cc43777b40364c2df8df99b4963a2b4b2132383ab87883fd2feba",
                    "Gateway": "172.18.0.1",
                    "IPAddress": "172.18.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:12:00:02",
                    "DriverOpts": {}
                }
            }
```

```bash
# 容器断开这个网络
docker network disconnect my-net mysql
```

mysql的具体信息另一个网络的信息就没了

```json
"Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "fabce6787e9a05a2e1194db8764ea830db08866d21fdf5f1648cca034a27256b",
                    "EndpointID": "b45d3f597811a0b5947a2a453c338f18ccb8c380556ce19056762e27305c4069",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
```


下面实现之前启动mysql终端的命令，替换为自定义网络连接
```bash
docker run -it --rm --network my-net mysql:5.7.25 mysql -hmysql -uroot -p  # 这里的--network my-net是指定这个网络
```

> [!warning] 注意
> 我们在启动镜像时就指定了**自定义的网络**，那么容器就不会再连接默认的网络(**桥接网络**)了
> 


```json
"Networks": {
                "my-net": {   //这里多了一个自定义的网络
                    "IPAMConfig": {},
                    "Links": null,
                    "Aliases": [
                        "dd28af4fe606"
                    ],
                    "NetworkID": "ec160819211e4e81b9a5bb22f77485e821ae5070533a8195b9be4adbce40b7cd",
                    "EndpointID": "4b384adf489cc43777b40364c2df8df99b4963a2b4b2132383ab87883fd2feba",
                    "Gateway": "172.18.0.1",
                    "IPAddress": "172.18.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:12:00:02",
                    "DriverOpts": {}
                }
            }
```

# docker 存储

这里挂载是什么操作？
- 将数据存储在容器中，一旦容器被删除，数据也会被删除。同时也会使容器变得越来越大，不方便恢复和迁移。
- 将数据存储到容器之外，这样删除容器也不会丢失数据。一旦容器故障，我们可以重新创建一个容器，将数据[[零散知识点#什么是挂载？Linux挂载|挂载]](**指向宿主机上特定目录的连接点**)到容器里，就可以快速的恢复。

## 存储方式

docker 提供了以下存储选项

<img src="https://article.biliimg.com/bfs/article/17ece4fab3a086fae1a2befb5110f858e6e48dd1.png" alt="image.png" style="zoom:70%;" />


- `volume` 卷
	**卷**存储在主机文件系统分配一块**专有存储区域**，由Docker(在Linux上)管理，并且**与主机的核心功能隔离**。非Docker进程不能修改文件系统的这一部分。卷是在Docker中**持久保存数据**的最佳方式。
- `bind mount` 绑定挂载
	**绑定挂载**可以将主机文件系统上目录或文件**装载**或**映射**到容器中，但是主机上的非Docker进程可以修改它们，同时在容器中也可以更改主机文件系统，包括创建、修改或删除文件或目录，使用不当，可能会带来安全隐患。
- `tmpfs` 临时挂载
	tmpfs挂载仅存储在主机系统的**内存**中，从不写入主机系统的文件系统。当容器停止时，数据将被删除。

## 绑定挂载(bind mount)

绑定挂载适用以下场景：
- 将**配置文件**从主机共享到容器。
- 在Docker主机上的开发环境和容器之间共享源代码或编译目录。
- 例如,可以将 Maven的`target/`目录挂载到容器中，每次在主机上用Maven打包项目时，容器内都可以使用新编译的程序包。

### -v

- 绑定挂载将主机上的目录或者文件装载到容器中。绑定挂载会覆盖容器中的目录或文件。
- 如果宿主机目录不存在，docker会自动创建这个目录。但是docker只自动创建文件夹，不会创建文件。
- 例如，mysql的配置文件和数据存储目录使用主机的目录。可以将配置文件设置为只读(read-only)防止容器更改主机中的文件。

```bash
docker run -e MYSQL_ROOT_PASSWORD=701121 \
           -v /home/mysql/mysql.cnf:/etc/mysql/conf.d/mysql.cnf:ro  \ # -v 就代表使用绑定挂载  ro代表只读
           -v /home/mysql/data:/var/lib/mysql  \  # 这一步完成容器种的数据存储到宿主机种,/var/lib/mysql是容器保存数据的地方,/home/mysql/data 这个目录就算不存在,容器也会帮宿主机创建
           -d mysql:5.7 
```

### --tmpfs 临时挂载

临时挂载将数据保留在主机**内存**中，当容器停止时，文件将被删除
```bash
docker run -d -it --tmpfs /tmp nginx:1.22  # 指定了一个tmpfs文件系统挂载到了容器的/tmp目录上
```


## volume 卷

卷是docker容器存储数据的首选方式，卷有以下优势：

- 卷可以在多个正在运行的容器之间共享数据。仅当**显式删除卷**时，才会删除卷。
- 当你想要将容器数据存储在外部网络存储上或云提供商上，而不是本地时。
- 卷更容易备份或迁移，当您需要备份、还原数据或将数据从一个  Docker主机迁移到另一个 Docker 主机时，卷是更好的选择


|命令|功能|
|:-:|:-:|
|`docker volume create [volume]`| 创建一个数据卷 |
|`docker volume ls`| 查看数据卷 |
|`docker volume inspect [volume]`| 查看数据卷详细信息 |
|`docker volume rm [volume]`| 删除数据卷 |
|`docker volume prune` | 删除所有未使用的数据卷 |

```bash
# 创建卷
docker volume create vol-my-data

# 修改之前的命令 改用卷保存容器的数据
docker run -e MYSQL_ROOT_PASSWORD=701121 \
           -v /home/mysql/mysql.cnf:/etc/mysql/conf.d/mysql.cnf:ro  \ 
           -v vol-my-data:/var/lib/mysql  \ 
           -d mysql:5.7 
# 使用这个命令查看卷的信息
docker inspect vol-my-data

{

  "CreatedAt": "2023-08-29T23:54:28-07:00",
  "Driver": "local",
  "Labels": null,
  "Mountpoint": "/var/lib/docker/volumes/vol-my-data/_data",
  "Name": "vol-my-data",
  "Options": null,
  "Scope": "local"
}
```


# 部署自己的应用

本例子我们使用docker来部署一个应用系统，RuoYi是一款用java编写的，基于SpringBoot+Bootstrap的后台管理系统。
ruoyi官方文档：[http://doc.ruoyi.vip/ruoyi/](http://doc.ruoyi.vip/ruoyi/)
源码下载：[https://gitee.com/y_project/RuoYi/tree/v4.7.4/](https://gitee.com/y_project/RuoYi/tree/v4.7.4/)
将源码编译打包成ruoyi-admin.jar文件，放到宿主机/home/app目录下，/home/app/sql目录下是数据库初始化脚本。
配置文件中修改了端口、数据库连接信息。

```yml
#application.yml
server:
  # 服务器的HTTP端口，默认为80
  port: 8080

---
#application-druid.yml
	url: jdbc:mysql://ruoyi-db:3306/ry?useUnicode=true&characterEncoding=utf8
  username: root
  password: 123456
```
- 准备工作：
创建网络和存储卷
```bash
docker volume create ruoyi-data
docker network create ruoyi-net
```

## 部署mysql并初始化数据库

我们在创建数据库容器的时候，需要做三件事：

- 创建数据库`ry`
- 设置字符集为`utf-8`
- 执行数据库初始化脚本

使用`MYSQL_DATABASE`环境变量创建数据库
设置字符集`--character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci`
容器使用`/docker-entrypoint-initdb.d`目录下的脚本初始化数据库，脚本可以是`.sh` `.sql`和`.sql.gz`这三种格式。

Initializing a fresh instance
> 当容器第一次启动时，将创建具有指定名称的新数据库，并使用提供的配置变量初始化。此外，它将执行在 `/docker-entrypoint-initdb.d` 中找到的扩展名为 `.sh` 、 `.sql` 和 `.sql.gz` 的文件。文件将按字母顺序执行。您可以通过将SQL转储装载到该目录中来轻松地填充 `mysql` 服务，并提供包含贡献数据的自定义映像。默认情况下，SQL文件将导入到`MYSQL_DATABASE`变量指定的数据库中。


```bash
docker run -e MYSQL_ROOT_PASSWORD=123456 \
           -e MYSQL_DATABASE=ry \
					 -v /home/app/sql:/docker-entrypoint-initdb.d \
           -v ruoyi-data:/var/lib/mysql  \
           --network ruoyi-net \
           --name ruoyi-db \
           -d mysql:5.7 \
           --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

启动这个项目需要jdk,也拉一个jdk的容器
```bash
docker pull openjdk:8u342-jre
```


创建这个jdk的容器,并配置好端口、网络文件映射
```bash
docker run --name ruoyi-app \
-p 8080:8080 \
--network ruoyi-net \
-v /home/yesho/HelloDocker/app/ruoyi-admin.jar:/usr/local/src/ruoyi-admin.jar \
-d openjdk:8u342-jre \
java -jar  /usr/local/src/ruoyi-admin.jar
```


## 解决乱码问题：

乱码问题是容器中mysql默认字符集引起的，我们需要将默认字符集改为`utf8mb4`。

参考：[https://github.com/docker-library/mysql/issues/131](https://github.com/docker-library/mysql/issues/131)

可以进入容器，使用以下命令查看数据库字符集

```bash
docker exec -it ruoyi-db mysql -uroot -p
>show variables like '%character%';
```

> 注意：由于删除容器不会删除存储卷，修改字符集需要删除存储卷，不然已经导入的数据字符集不会发生改变删除容器和卷

### 1.修改运行参数

使用环境变量`LANG=C.UTF-8`设置客户端字符集
```bash
docker run  -e MYSQL_ROOT_PASSWORD=123456 \
            -e MYSQL_DATABASE=ry \
            -e LANG=C.UTF-8 \
            -v /home/app/sql:/docker-entrypoint-initdb.d \
            -v ruoyi-data:/var/lib/mysql  \
            --network ruoyi-net \
            --name ruoyi-db \
            -d mysql:5.7 \
            --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

**或者**
使用--skip-character-set-client-handshake忽略客户端字符集，使用客户端和服务端字符集一致
```bash
docker run  -e MYSQL_ROOT_PASSWORD=123456 \
            -e MYSQL_DATABASE=ry \
            -v /home/app/sql:/docker-entrypoint-initdb.d \
            -v ruoyi-data:/var/lib/mysql  \
            --network ruoyi-net \
            --name ruoyi-db \
            -d mysql:5.7 \
            --skip-character-set-client-handshake --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci 
```

### 2.修改配置文件  
修改`/home/mysql/mysql.cnf`
```cnf
[mysqld]
character-set-server=utf8mb4
collation-server=utf8mb4_general_ci
init-connect='SET NAMES utf8mb4'

[client]
default-character-set=utf8mb4

[mysql]
default-character-set=utf8mb4
```

将配置文件挂载到容器中

```bash
docker run -e MYSQL_ROOT_PASSWORD=123456 \
           -e MYSQL_DATABASE=ry \
           -v /home/mysql/mysql.cnf:/etc/mysql/conf.d/mysql.cnf:ro  \
					 -v /home/app/sql:/docker-entrypoint-initdb.d \
           -v ruoyi-data:/var/lib/mysql  \
           --network ruoyi-net \
           --name ruoyi-db \
           -d mysql:5.7 
```

# docker compose容器编排

在实际工作中，部署一个应用可能需要部署多个容器，一个一个部署非常不方便。`docker compose`可以一键部署和启动多个容器，它使用`yaml`文件来编排服务。
`github`和`docker hub`很多项目都提供了`docker-compose.yaml`文件，我们可以一键部署项目，非常方便。

### 一键部署wordpress
[wordpress](https://hub.docker.com/_/wordpress)是一个著名的开源博客系统。
将以下内容保存到本地的docker-compose.yml文件中。
`docker compose`命令启动时，默认在当前目录下寻找`compose.yaml`或`compose.yml`，为了兼容之前的版本，也会查找`docker-compose.yaml`或`docker-compose.yml`。
也可以使用`-f`参数手动指定文件`docker compose -f docker-compose-dev.yml up -d`

```yaml
version: '3.1'

services:

  wordpress:
    image: wordpress
    restart: always
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: exampledb
    volumes:
      - wordpress:/var/www/html

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: exampledb
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - db:/var/lib/mysql

volumes:
  wordpress:
  db:
```


|命令|功能|
|:-:|:-:|
| `docker compose up -d`      | 一键部署启动                      |
| `docker compose start/stop` | 启动/停止服务                    |
| `docker compose down`       | 停止并删除容器，不会删除存储卷volume |

### compose文件结构
`docker-compose.yml`通常需要包含以下几个顶级元素：

- `version` 已弃用，早期版本需要此元素。
- `services`必要元素，定义一个或多个容器的运行参数
	- 在`services`中可以通过以下元素定义容器的运行参数
	- `image` 容器 镜像
	- `ports`端口映射 通常带`s`的都要用**列表**或者**数组**
	- `environment`环境变量
	- `networks`容器使用的网络
	- `volumes`容器挂载的存储卷
	- `command`容器启动时执行的命令
	- `depends_on`定义启动顺序
	- 复数形式(例如`ports`,`networks`,`volumes`,`depends_on`)参数需要传入列表
- `networks`创建自定义网络
- `volumes` 创建存储卷

### yaml文件语法
- 缩进代表上下级关系
- 缩进时不允许使用Tab键，只允许使用空格
- `:` 键值对，后面必须有空格
- `-`列表，后面必须有空格 
- `[ ]`数组
- `#`注释
- `{key:value,k1:v1}`map
- `|` 多行文本块

如果一个文件中包含多个文档

- `---`表示一个文档的开始

还有一种常见的用法:
把公共的配置提取出来，用`&`来建立锚点，`<<`合并到当前数据，用`*`引用锚点，例如
```yaml
version: '3.7'

# Settings and configurations that are common for all containers
x-minio-common: &minio-common
  image: quay.io/minio/minio:RELEASE.2022-08-13T21-54-44Z
  command: server --console-address ":9001" http://minio{1...2}/data{1...2}
  expose:
    - "9000"
    - "9001"
  
services:
  minio1:
    <<: *minio-common
    volumes:
      - data1-1:/data1
      - data1-2:/data2

  minio2:
    <<: *minio-common
    volumes:
      - data2-1:/data1
      - data2-2:/data2

volumes:
  data1-1:
  data1-2:
  data2-1:
  data2-2:
```

### 编排自己的项目

以ruoyi项目为例子，先采用挂载目录的方式部署应用，等我们学完dockfile打包，就可以完整的部署应用了。

```yaml
version: '3.1'

services:
	ruoyi-app:
		image: openjdk:8u342-jre
		ports:
			- 8080:8080
		volumes: 
			- /home/app/ruoyi-admin.jar:/usr/local/src/ruoyi-admin.jar
		command: java -jar /usr/local/src/ruoyi-admin.jar
		networks: 
			- ruoyi-net
		depends_on:
			- ruoyi-db
	ruoyi-db:
		image: mysql:5.7
		environment:
			- MYSQL_ROOT_PASSWORD=123456
			- MYSQL_DATABASE=ry
		command: [
			"--character-set-server=utf8mb4",
		  "--collation-server=utf8mb4_general_ci",
		  "--skip-character-set-client-handshake"
		]
		volumes: 
			- /home/app/sql:/docker-entrypoint-initdb.d
			- ruoyi-data:/var/lib/mysql
		networks:
			- ruoyi-net
	
volumes:
  ruoyi-data:

networks:
  ruoyi-net:
```

`command`支持以下写法：

```yaml
#推荐使用数组或列表的方式
#数组
command:
	["java",
  "-jar",
  "/usr/local/src/ruoyi-admin.jar"
	]
#列表
command: 
  - java
  - -jar
  - /usr/local/src/ruoyi-admin.jar

# shell命令模式
command: java -jar /usr/local/src/ruoyi-admin.jar
```

执行复杂的脚本
```yaml
command:
  - bash
  - "-c"
  - |
    set -ex
    # Generate mysql server-id from pod ordinal index.
    [[ `hostname` =~ -([0-9]+)$ ]] || exit 1
    ordinal=${BASH_REMATCH[1]}
    echo [mysqld] > /mnt/conf.d/server-id.cnf
    # Add an offset to avoid reserved server-id=0 value.
    echo server-id=$((100 + $ordinal)) >> /mnt/conf.d/server-id.cnf
    # Copy appropriate conf.d files from config-map to emptyDir.
    if [[ $ordinal -eq 0 ]]; then
      cp /mnt/config-map/primary.cnf /mnt/conf.d/
    else
      cp /mnt/config-map/replica.cnf /mnt/conf.d/
    fi       
```

`environment`支持如下两种写法

```yaml
# 使用map
environment:
    MYSQL_DATABASE: exampledb
    MYSQL_USER: exampleuser
    MYSQL_PASSWORD: examplepass
    MYSQL_RANDOM_ROOT_PASSWORD: '1'

#使用列表
environment:
    - MYSQL_ROOT_PASSWORD=123456
    - MYSQL_DATABASE=ry
    - LANG=C.UTF-8
```


### 容器启动顺序depends_on

数据库初始化完成之前，不会建立connections。

<img src="https://article.biliimg.com/bfs/article/ab78dfd69347a08ddc3e929a9ecb847c37bbecae.png" alt="image.png" style="zoom:60%;" />

`depends_on`只能保证容器的启动和销毁顺序，不能确保依赖的容器是否ready。
```yaml
version: "3.9"
services:
  web:
    build: .
    depends_on:
      - db
      - redis
  redis:
    image: redis
  db:
    image: postgres
```

在这个例子中，`depends_on`只能保证`web`容器在`db`，`redis`之后启动，不会关注他们的状态是否启动完成或准备就绪。
要确保应用服务在数据库初始化完成后再启动，需要配合`condition`和`healthcheck`使用。
```yaml
services:
  web:
    build: .
    depends_on:
      db:
        condition: service_healthy # 示只有在 db服务处于健康状态时，web 服务才会启动；
      redis:
        condition: service_started # 表示只有在 redis服务启动后，web 服务才会启动
  redis:
    image: redis
  db:
    image: postgres
```

`condition`有三种状态：
- `service_started`容器已启动
- `service_healthy`容器处于健康状态
- `service_completed_successfully`容器执行完成且成功退出(退出状态码为0)
我们来改造一下我们自己的`docker-compose.yaml`文件，完整例子如下：
```yaml
services: 

  ruoyi-app:
    #  docker run --name ruoyi-app      \
    #             -p 8080:8080        \
    #             --network ruoyi-net      \
    #             -v /home/app/ruoyi-admin.jar:/usr/local/src/ruoyi-admin.jar   \
    #             -d openjdk:8u342-jre    \
    #             java -jar /usr/local/src/ruoyi-admin.jar
    image: openjdk:8u342-jre
    restart: always
    ports:
      - 8080:8080
    networks:
      - ruoyi-net
    volumes:
      - /home/app/ruoyi-admin.jar:/usr/local/src/ruoyi-admin.jar
    command: [ "java", "-jar", "/usr/local/src/ruoyi-admin.jar" ]
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 10s
    depends_on:
      ruoyi-db:
        condition: service_healthy

  ruoyi-db:
    #  docker run --name ruoyi-db -p 3303:3306 \
    #             --network ruoyi-net        \
    #             -v ruoyi-data:/var/lib/mysql  \
    #             -v /home/app/sql:/docker-entrypoint-initdb.d   \
    #             -e MYSQL_DATABASE=ry         \
    #             -e MYSQL_ROOT_PASSWORD=123456    \
    #             -d mysql:5.7      \
    #             --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci --skip-character-set-client-handshake
    image: mysql:5.7
    environment:
      - MYSQL_DATABASE=ry
      - MYSQL_ROOT_PASSWORD=123456
    volumes:
      - ruoyi-data:/var/lib/mysql
      - /home/app/sql:/docker-entrypoint-initdb.d
    networks:
      - ruoyi-net
    command:
      [
        "--character-set-server=utf8mb4",
        "--collation-server=utf8mb4_unicode_ci",
        "--skip-character-set-client-handshake"
      ]
    # 这里是service_healthy怎么去检查健康状态的？
    healthcheck:
      test: ["CMD", 'mysqladmin', 'ping', '-h', 'localhost', '-u', 'root', '-p$$MYSQL_ROOT_PASSWORD']
      interval: 10s  # 连续健康检查之间的时间间隔
      timeout: 5s # 单个健康检查的最长允许时间
      retries: 5 # 健康检查被视为失败之前尝试的次数
      start_period: 10s # 容器启动后开始进行健康检查之前等待的时间

volumes:
  ruoyi-data:

networks:
  ruoyi-net:

```


# dockerfile制作镜像

## 什么是dockerfile？

`Dockerfile`是一个**文本文件**，其内包含了一条条的**指令**(Instruction)，用于构建镜像。每一条指令构建一层镜像，因此每一条指令的内容，就是**描述该层镜像应当如何构建**。
`dockerfile`用于指示`docker image build`命令自动构建Image的源代码是纯文本文件

 
> [!example] 构建步骤： 
> 1. 编写一个`dockerfile`文件  
> 2. `docker build`构建成为一个镜像  
> 3. `docker run` 运行镜像  
> 4. `docker pusH` 发布镜像（Docker Hub 、阿里云镜像仓库）
> 


## Dockerfile 构建过程

#### 基础知识

1 每个保留关键字（指令）都是必须是大写字母  
2 执行从上到下顺序执行  
3 # 表示注释  
4 每一个指令都会创建一个新的镜像层，并提交

<img src="https://img-blog.csdnimg.cn/20210316230028667.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Nsb3ZlcjY2MQ==,size_16,color_FFFFFF,t_70" alt="image.png" style="zoom:60%;" />

#### 步骤：开发，部署，运维，缺一不可

- `Dockerfile`：构建文件，定义了一切的步骤，源代码  
- `Dockerimages`：通过`Dockerfile`构建生成的镜像，最终发布和运行的产品  
- `Docker容器`：容器就是镜像运行起来提供服务器


## dockerfile指令

`dockerfile`通常包含以下几个常用命令：
```dockerfile
FROM ubuntu:18.04
WORKDIR /app
# 这里最好用相对路径,宿主机是相对于dockerfile这个文件,而容器是相对于WORKDIR
COPY . .   
RUN make .
CMD python app.py
EXPOSE 80
```

- `FROM` 打包使用的**基础镜像**
- `WORKDIR`相当于`cd`命令，进入docker容器工作目录
- `COPY` 将宿主机的文件**复制**到容器内,这里
- `RUN`打包时执行的命令，相当于**打包过程**中在容器中执行`shell`脚本，通常用来安装应用程序所需要的依赖、设置权限、初始化配置文件等(编译时执行的命令)
- `CMD`运行镜像时执行的命令(也就是容器启动时要运行的默认启动命令)
- `EXPOSE`指定容器在运行时监听的网络端口，它并不会公开端口，仅起到声明的作用，公开端口需要容器运行时使用-p参数指定。

## 制作自己的镜像

参考我们之前的配置，制作`dockerfile`文件
```dockerfile
# image: openjdk:8u342-jre
#   ports:
#     - 8080:8080
#   networks:
#     - ruoyi-net
#   volumes:
#     - /home/app/ruoyi-admin.jar:/usr/local/src/ruoyi-admin.jar
#   command: [ "java", "-jar", "/usr/local/src/ruoyi-admin.jar" ]

FROM openjdk:8u342-jre
WORKDIR /app
COPY ./ruoyi-admin.jar .
CMD [ "java","-jar","ruoyi-admin.jar" ]
EXPOSE 8080
```

```bash
docker build .  # 打包
docker images
REPOSITORY    TAG         IMAGE ID       CREATED         SIZE
<none>        <none>      a2f9279fc0d2   2 minutes ago   349MB  # none这是我们在打包的时候没有给他指定名称和tag
```

```bash
docker tag a2f9279fc0d2 ruoyi-app:4.7.4-jar  # 设置镜像的标签
docker images
REPOSITORY    TAG         IMAGE ID       CREATED         SIZE
ruoyi-app     4.7.4-jar   a2f9279fc0d2   5 minutes ago   349MB
```

打包完后之后就可以运行自己的镜像了,修改一下之前的compose文件,改成自己的镜像,因为我们将应用程序打包到容器之中了(copy那步),不需要使用挂载券这种方式了,执行的命令也定义到了打包文件,所以不需要command命令了

```yaml
services: 

  ruoyi-app:
    image: ruoyi-app:4.7.4-jar
    ports:
      - 8080:8080
    networks:
      - ruoyi-net
    # volumes:
    #   - /home/app/ruoyi-admin.jar:/usr/local/src/ruoyi-admin.jar
    # command: [ "java", "-jar", "/usr/local/src/ruoyi-admin.jar" ]
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 10s
    depends_on:
      ruoyi-db:
        condition: service_healthy
```

## image镜像与layer层

image文件由一系列层构建而成，`dockerfile`每一个命令都会生成一个层会指定一个唯一的id。每一层都是**只读**的。
例如前面我们制作镜像，就产生了4个层。
<img src="https://article.biliimg.com/bfs/article/362a9d2841e697653a521766f517575d765831f3.png" alt="image.png" style="zoom:50%;" />
也可以使用`docker image history ruoyi-java:4.7.4`命令查看

<img src="https://article.biliimg.com/bfs/article/4ed29d655bbf16d1cd79e576ff0d5811856473d7.png" alt="image.png" style="zoom:70%;" />

创建容器时，会创建一个新的可写层，通常称为“容器层”。对正在运行的容器所做的所有更改（如写入新文件、修改现有文件和删除文件）都将写入容器层，而不会修改镜像。

<img src="https://article.biliimg.com/bfs/article/f63d11f546669927a3e7580178a180ca786627b0.png" alt="image.png" style="zoom:70%;" />
## 多阶段构建

在构建基于Java的应用程序时，需要一个JDK将**源代码**编译为Java字节码。但是，在**生产中**不需要该JDK。
多阶段构建可以将**生成时**依赖与**运行时**依赖分开，减小整个image文件大小。

### Maven/Tomcat 示例

使用`Maven`来构建应用，在最终的image中不需要包含maven。我们可以使用多阶段构建，**每一个阶段**从`FROM`开始，最终的image只会从最后一个阶段构建，**不会包含前面阶段产生的层**，因此可以减少镜像体积。

```dockerfile
FROM maven AS build
WORKDIR /source
COPY . .
RUN mvn package


FROM openjdk:8u342-jre
WORKDIR /app
# 使用 COPY --from=build 指令从第一个阶段的 build镜像中复制 /soure/ruoyi-admin/targe/ruoyi-admin.jar 文件到 /app 目录中。
COPY --from=build /soure/ruoyi-admin/targe/ruoyi-admin.jar . 
ENTRYPOINT [ "java","-jar","ruoyi-admin.jar" ]
EXPOSE 8080
```


`docker build -t ruoyi-jar:4.7.4 .` -t参数指定名称和版本

### ENTRYPOINT和CMD的区别

- `dockerfile`应该至少包含一个`ENTRYPOINT`或`CMD` 
- `ENTRYPOINT`指定容器启动时执行的默认程序,一般运行容器时**不会被替换**或**覆盖**。
- 除非使用`--entrypoint`进行指定。

```bash
docker run -it --entrypoint /bin/bash redis  # 这里将redis默认执行的命令修改为bash
```

`CMD`可以在启动容器时**被替换**和**覆盖**。
例如`docker run -it --rm mysql:5.7 /bin/bash`
如果镜像中`ENTRYPOINT`和`CMD`都存在，则`CMD`将作为`ENTRYPOINT`的参数使用。

# 私有仓库

## docker registry 

我们可以使用`docker push`将自己的image推送到`docker hub`中进行共享，但是在实际工作中，很多公司的代码不能上传到公开的仓库中，因此我们可以创建自己的镜像仓库。  
docker 官网提供了一个docker registry的私有仓库项目，可以方便的通过docker部署。

```bash
docker run -d -p 5000:5000 --restart always --name registry registry:2  # 运行一个名为 "registry" 的容器并启动一个 Docker 镜像仓库（Registry）--restart always：这个选项指定了当容器发生异常停止时，Docker 会自动重启该容器，并且始终如此 
# 在另一机器推送之前 要将原有的镜像名称加上推送的仓库ip和端口
(另一台虚拟机) docker image tag ruoyi-java:4.7.4 192.168.153.128:5000/ruoyi-java:4.7.4  
(另一台虚拟机) docker push localhost:5000/ruoyi-java:4.7.4   # 这时候推送到自己的私有仓库中了
``` 

如果遇到以下错误：
<img src="https://cdn.nlark.com/yuque/0/2022/png/28915315/1663307599499-8035dfa9-01e1-44dd-8d96-e27c52ea0a57.png" alt="image.png" style="zoom:50%;" />
这是因为`docker push`默认使用`HTTPS`协议，而服务端的`registry`仓库使用的是`HTTP`。
解决这个问题，需要修改`/etc/docker/daemon.json`，加入
```json
"insecure-registries": ["192.168.56.108:5000"]
```

```bash
docker pull localhost:5000/ruoyi-admin:latest # 拉取要加仓库ip和端口
```

## harbor

`habor`是一个功能更强大镜像仓库，它具有完整的权限控制和Web界面，更符合我们的实际工作场景。
下载bitname发布的harbor镜像配置包：[https://github.com/bitnami/containers/archive/main.tar.gz](https://github.com/bitnami/containers/archive/main.tar.gz)

```bash
mkdir harbor
tar xzvf containers-main.tar.gz
cd containers-main/bitnami/harbor-portal
docker compose up -d
```

浏览器访问：[192.168.153.128](http://192.168.153.128/harbor/projects)，默认用户名/密码：`admin/bitnami`

<img src="https://cdn.nlark.com/yuque/0/2022/png/28915315/1663308776828-c8e2eb02-b689-4f47-9243-83a1a1cb5f41.png?x-oss-process=image%2Fresize%2Cw_937%2Climit_0" alt="image.png" style="zoom:70%;" />
## 保存与加载image

当我们处于离线状态，比如在很多内网上不能访问互联网，这时候不能通过镜像仓库的方式共享image，我们可以使用**导出**和**导入**功能，手动拷贝镜像。

- `docker save`会包含所有层，以及所有标签 + 版本信息。
- `docker save alpine:3.15 > alpine-3.15.tar` 保存image 这个本地没有才会从dockhub镜像拉取
- `docker rmi alpine:3.15` 删除本地image
- `scp alpline:3.15 > alpine-3.15.tar`  SCP（Secure Copy）工具将名为 "mysql-5.7.tar" 的文件从本地主机复制到远程主机上的用户 "root" 的家目录下
- `docker load < alpine-3.15.tar` 加载image



> [!attention] 注意：
> - 不要跟export和import命令混淆
> - `docker save/load IMAGE` save和load操作的是镜像
> - `docker export/import CONTAINER`export和import操作对象是容器
> - image包含多个层，每一层都不可变，save保存的信息包含每个层和所有标签 + 版本信息。
> - 容器运行的时候会创建一个可写入的容器层，所有的更改都写入容器层，export导出的只有容器层，不包含父层和标签信息。
> <img src="https://cdn.nlark.com/yuque/0/2022/png/28915315/1661007394206-b7d81707-a557-41e8-a840-f708acf20292.png" alt="image.png" style="zoom:60%;" />






