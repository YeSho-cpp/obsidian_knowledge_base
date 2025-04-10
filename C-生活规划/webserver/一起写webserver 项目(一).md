---
title: 一起写webserver 项目(一)
tags: ['项目']
categories:
  - 一起做项目
---


在做整个项目之前建议看完《Linux高性能服务器编程》-中国-游双，这个项目会用到里面的很多知识。

## webserver 环境配置

### 安装Linux环境
webserver作为c++的一个经典项目，虽然烂大街，但是对于网络编程和系统编程非常重要，几乎等同于spring于java,可以不用，但基本要会。

选择一个github的项目 [GitHub - qinguoyi/TinyWebServer: :fire: Linux下C++轻量级WebServer服务器](https://github.com/qinguoyi/TinyWebServer)

我用的是wsl2子系统，发行版是Debian系统，gcc、g++默认都已经安装了，推荐大家用云服务器，这样项目运行访问时就不必是回环地址，这样更贴合生产环境。

### 安装MYSQL
使用**apt**包管理器：
```sh
sudo apt-get update
sudo apt-get install mysql-server
```
其他发行版也同理

### 运行项目
然后就是克隆项目运行
```sh
git init ## 将本地仓库初始化
git clone <url> ## 将需要的项⽬从 github 上克隆下来，url为项⽬地址
```

测试前确认已安装MySQL数据库（mysql的配置）
```sql
// 建⽴yourdb库
create database yourdb;
// 创建user表
USE yourdb;
CREATE TABLE user(
username char(50) NULL,
passwd char(50) NULL
)ENGINE=InnoDB;
// 添加数据
INSERT INTO user(username, passwd) VALUES('name', 'passwd');
```

修改main.cpp中的数据库初始化信息
```sh
//数据库登录名,密码,库名
string user = "root";
string passwd = "root";
string databasename = "yourdb";
```

随后我们执行
```sh
sh ./build.sh
```

出现了BUG

![image.png](http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240429210138.png)

这里是缺少mysql库文件，我们去查一下GitHub上的issue，发现作者给了解决方法


<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240429210238.png" alt="image.png" style="zoom:60%;" />


执行代码
```sh
sudo apt-get install libmysqlclient-dev
```

再make一遍，果然不再报库文件缺失，至于warning不用管。
这时候ls一下，可以看到server可运行文件了

```sh
./server
```

<img src="http://img-blog.csdnimg.cn/img_convert/e009c7109caf9c7308f3f6b3f5e2a390.png" alt="image.png" style="zoom:60%;" />


光标不动了，说明运行成功。

### 浏览器访问

接下来就是浏览器访问了，在保持服务器运行的情况下，打开浏览器

如果是虚拟机的同学，可以使用回环地址（不知道的翻一下计网的书）

> 127.0.0.1:9006

云服务器的同学，可以去管理台查一下自己的云服务器的公网IP，然后输入

> IP:9006

如果发现打不开，就去服务器实例的防火墙（腾讯云）/安全组（阿里云）里面把9006端口设置为允许

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240429210447.png" alt="image.png" style="zoom:60%;" />


下面开始正式写代码！