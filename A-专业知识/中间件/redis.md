
# redis介绍

## Nosql与sql数据库对比

**NoSql**可以翻译做Not Only Sql（不仅仅是SQL），或者是No Sql（非Sql的）数据库。是相对于传统关系型数据库而言，有很大差异的一种特殊的数据库，因此也称之为**非关系型数据库**。

### 结构化与非结构化

传统关系型数据库是结构化数据，每一张表都有严格的**约束信息**：字段名.字段数据类型.字段约束等等信息，插入的数据必须遵守这些约束：

<img src="https://i.imgur.com/4tUgFo6.png" style="zoom:70%;" />


而NoSql则对数据库格式没有严格约束，往往形式松散，自由。
可以是**键值**型：

<img src="https://i.imgur.com/GdqOSsj.png" style="zoom:50%;" />
也可以是**文档**型：

<img src="https://i.imgur.com/zBBQfcc.png" style="zoom:67%;" />

甚至可以是**图**格式：

<img src="https://i.imgur.com/zBnKxWf.png" style="zoom:67%;" />

### 关联和非关联

传统数据库的表与表之间往往存在**关联**，例如外键：

<img src="https://i.imgur.com/tXYSl5x.png" style="zoom:70%;" />
而非关系型数据库不存在**关联关系**，要维护关系要么靠代码中的业务逻辑，要么靠数据之间的**耦合**：

```json
{
  id: 1,
  name: "张三",
  orders: [
    {
       id: 1,
       item: {
	 id: 10, title: "荣耀6", price: 4999
       }
    },
    {
       id: 2,
       item: {
	 id: 20, title: "小米11", price: 3999
       }
    }
  ]
}
```

此处要维护“张三”的订单与商品“荣耀”和“小米11”的关系，不得不冗余的将这两个商品保存在张三的订单文档中，不够优雅。还是建议用业务来维护关联关系。

### 查询方式

- 传统关系型数据库会基于Sql语句做查询，语法有**统一标准**；
- 而不同的非关系数据库查询语法差异极大，五花八门各种各样。

<img src="https://i.imgur.com/AzaHOTF.png" style="zoom:50%;" />

### 事务
传统关系型数据库能满足事务[[Mysql#事务四大特征|ACID]]的原则。

<img src="https://i.imgur.com/J1MqOJM.png" style="zoom:50%;" />
而非关系型数据库往往不支持事务，或者不能严格保证ACID的特性，只能实现**基本**的一致性。

### 存储方式

- 存储方式
    - 关系型数据库**基于磁盘**进行存储，会有大量的磁盘IO，对性能有一定影响
    - 非关系型数据库，他们的操作更多的是依赖于**内存**来操作，内存的读写速度会非常快，性能自然会好一些
- 扩展性
    - 关系型数据库集群模式一般是**主从**，主从数据一致，起到数据备份的作用，称为**垂直**扩展。
	    - SQL的扩展性是垂直的，指的是通过增加更**强大的硬件**来提高系统性能和容量。垂直扩展通常包括升级服务器的CPU、内存、存储等组件，以增加系统处理能力和存储空间。
    - 非关系型数据库可以将数据拆分，存储在不同机器上，可以保存海量数据，解决内存大小有限的问题。称为水平扩展。
	    - NoSQL（非关系型数据库）的扩展性是水平的，指的是通过增加更多的机器节点来提高系统性能和容量。水平扩展通常涉及将负载分散到多个服务器节点上，并将**数据分片存储**在不同节点上。这种方式可以实现更好的负载均衡和数据并行处理。
    - 关系型数据库因为表之间存在关联关系，如果做水平扩展会给数据查询带来很多麻烦
    - 水平扩展具有很好的可伸缩性，可以通过添加新节点来适应不断增长的数据量和用户访问量。而垂直扩展则受限于单个服务器硬件资源的极限，无法无限地提高系统性能和容量。因此，在需要处理大规模数据或高并发访问时，NoSQL数据库通常比传统关系型数据库更具优势。

### 总结

<img src="https://i.imgur.com/kZP40dQ.png" style="zoom:50%;" />

## redis简单介绍

Redis是一种**键值型**的**NoSql**数据库，这里有两个关键字：

- 键值型
- NoSql

<img src="https://article.biliimg.com/bfs/article/7be2da4ab7dab6febe5accef9e636045cec878ea.png" style="zoom:70%;" />

- 其中**键值型**，是指Redis中存储的数据都是以`key.value`对的形式存储，而value的形式多种多样，可以是字符串、数值、甚至json：
- 而NoSql则是相对于传统关系型数据库而言，有很大差异的一种数据库。
- 对于存储的数据，没有类似Mysql那么严格的**约束**，比如唯一性，是否可以为null等等，所以我们把这种**松散结构**的数据库，称之为NoSQL数据库。


### 认识Redis

Redis诞生于2009年全称是**Re**mote **D**ictionary **S**erver 远程词典服务器，是一个基于内存的键值型NoSQL数据库。

**特征**：

- 键值（key-value）型，value支持多种不同数据结构，功能丰富
- **单线程**(Redis是单线程的，它使用一个<font color=#ff0000>事件循环</font>来处理客户端请求。在事件循环中，Redis会不断地从客户端接收命令，并逐个执行这些命令。由于Redis是单线程的，所以每个命令都具备原子性，即一个命令执行完毕后才会执行下一个命令。)，每个命令具备**原子性**
- 低延迟，速度快（基于内存.IO多路复用.良好的编码）。
- **支持数据持久化**(即将数据永久地存储在磁盘上，以便长期使用。而非关系数据库则主要用于查询、热备份和缓存功能，即用于快速查询和备份数据，并且可以缓存数据以提高性能。)
- 支持[[redis#集群结构和作用|主从集群]],[[redis#Redis分片集群|分片集群]]
- 支持多语言客户端


Redis的官方网站地址：[Redis](https://redis.io/)

### 安装Redis

基于Linux系统来安装Redis

依赖库

Redis是基于C语言编写的，因此首先需要安装Redis所需要的gcc依赖：

```sh
yum install -y gcc tcl
```

默认的安装路径是在 `/usr/local/bin`目录下：

<img src="https://i.imgur.com/YSxkGm7.png" style="zoom:80%;" />

该目录已经默认配置到环境变量，因此可以在任意目录下运行这些命令。其中：
- `redis-cli`：是redis提供的命令行客户端
- `redis-server`：是redis的服务端启动脚本
- `redis-sentinel`：是redis的哨兵启动脚本

#### 启动

redis的启动方式有很多种，例如：

- 默认启动
- 指定配置启动
- 开机自启

#### 默认启动

安装完成后，在任意目录输入redis-server命令即可启动Redis：

```sh
redis-server
```

如图：

<img src="https://i.imgur.com/v7xWsqC.png" style="zoom:60%;" />

这种启动属于`前台启动`，会阻塞整个会话窗口，窗口关闭或者按下`CTRL + C`则Redis停止。不推荐使用。

#### 指定配置启动

如果要让Redis以`后台`方式启动，则必须修改Redis配置文件，就在我们之前解压的redis安装包下（`/usr/local/src/redis-6.2.6`），名字叫`redis.conf`：

<img src="https://article.biliimg.com/bfs/article/8f5cb48c0e85cbdcea2ea1cbb3b8ff1d22e217f6.png" alt="image-20211211082225509" style="zoom:67%;" />

我们先将这个配置文件备份一份：
```sh
cp redis.conf redis.conf.bck
```

然后修改redis.conf文件中的一些配置：

```properties
# 允许访问的地址，默认是127.0.0.1，会导致只能在本地访问。修改为0.0.0.0则可以在任意IP访问，生产环境不要设置为0.0.0.0
bind 0.0.0.0
# 守护进程，修改为yes后即可后台运行
daemonize yes 
# 密码，设置后访问Redis必须输入密码
requirepass 123321
```

Redis的其它常见配置：

```properties
# 监听的端口
port 6379
# 工作目录，默认是当前目录，也就是运行redis-server时的命令，日志.持久化等文件会保存在这个目录
dir .
# 数据库数量，设置为1，代表只使用1个库，默认有16个库，编号0~15
databases 1
# 设置redis能够使用的最大内存
maxmemory 512mb
# 日志文件，默认为空，不记录日志，可以指定日志文件名
logfile "redis.log"
```

启动Redis：

```sh
# 进入redis安装目录 
cd /usr/local/src/redis-6.2.6
# 启动
redis-server redis.conf
# 进入redis数据库
redis-cli -h 192.168.153.128 -p 6379
```

停止服务：

```sh
# 利用redis-cli来执行 shutdown 命令，即可停止 Redis 服务，
# 因为之前配置了密码，因此需要通过 -u 来指定密码
redis-cli -u 701121 shutdown
```

# Redis常见命令

## Redis数据结构介绍

Redis是一个`key-value`的数据库，key一般是String类型，不过value的类型多种多样：

|类型|例子 |
|:-:|:-:|
|`String`|`hello world`|
|`Hash`|`{name:"Jack", age: 21}` |
|`List`|`[A -> B -> C -> C]` |
|`Set`|`{A, B, C}`|
|`SortedSet`|`{A: 1, B: 2, C: 3}`|
|`GEO`|`{A:(120.3,30.5)}`|
|`BitMap`|`0110110101110101011`|
|`HyperLog`|`0110110101110101011`|

Redis为了方便我们学习，将操作不同数据类型的命令也做了分组，在官网（ https://redis.io/commands ）可以查看到不同的命令：

<img src="https://article.biliimg.com/bfs/article/68296302d959c9f0050f00fa3af5b57dfda03702.png" alt="1652887648826" style="zoom:50%;" />
当然我们也可以通过Help命令来帮助我们去查看命令

<img src="https://article.biliimg.com/bfs/article/1bccf093eafa243c59d0156abbd1ebd596465d7f.png" alt="1652887748279" style="zoom:50%;" />

## Redis 通用命令

通用指令是部分数据类型的，都可以使用的指令，常见的有：

- KEYS：查看符合模板的所有key
- DEL：删除一个指定的key
- EXISTS：判断key是否存在
- EXPIRE：给一个key设置**有效期**，有效期到期时该key会被自动删除
- TTL：查看一个KEY的剩余有效期

通过`help [command]` 可以查看一个命令的具体用法，例如

<img src="https://article.biliimg.com/bfs/article/ea11398a5329b9a50e510849f64a5fcac70fea1c.png" alt="1652887865189" style="zoom:50%;" />

示例代码如下:

- keys
	```sh
	127.0.0.1:6379> keys *
	1) "name"
	2) "age"
	127.0.0.1:6379>
	
	# 查询以a开头的key
	127.0.0.1:6379> keys a*
	1) "age"
	127.0.0.1:6379>
	```

- DEL
	```sh
	127.0.0.1:6379> help del
	
	  DEL key [key ...]
	  summary: Delete a key
	  since: 1.0.0
	  group: generic
	
	127.0.0.1:6379> del name #删除单个
	(integer) 1  #成功删除1个
	
	127.0.0.1:6379> keys *
	1) "age"
	
	127.0.0.1:6379> MSET k1 v1 k2 v2 k3 v3 #批量添加数据
	OK
	
	127.0.0.1:6379> keys *
	1) "k3"
	2) "k2"
	3) "k1"
	4) "age"
	
	127.0.0.1:6379> del k1 k2 k3 k4
	(integer) 3   #此处返回的是成功删除的key，由于redis中只有k1,k2,k3 所以只成功删除3个，最终返回
	127.0.0.1:6379>
	
	127.0.0.1:6379> keys * #再查询全部的key
	1) "age"	#只剩下一个了
	127.0.0.1:6379>
	```

- EXISTS
	```sh
	127.0.0.1:6379> help EXISTS
	
	  EXISTS key [key ...]
	  summary: Determine if a key exists
	  since: 1.0.0
	  group: generic
	
	127.0.0.1:6379> exists age
	(integer) 1
	
	127.0.0.1:6379> exists name
	(integer) 0
	```

- EXPIRE

	```sh
	127.0.0.1:6379> expire age 10
	(integer) 1
	
	127.0.0.1:6379> ttl age
	(integer) 8
	
	127.0.0.1:6379> ttl age
	(integer) 6
	
	127.0.0.1:6379> ttl age
	(integer) -2
	
	127.0.0.1:6379> ttl age
	(integer) -2  #当这个key过期了，那么此时查询出来就是-2 
	
	127.0.0.1:6379> keys *
	(empty list or set)
	
	127.0.0.1:6379> set age 10 #如果没有设置过期时间
	OK
	
	127.0.0.1:6379> ttl age
	(integer) -1  # ttl的返回值就是-1
	```

### String命令

String类型，也就是字符串类型，是Redis中最简单的存储类型。其value是字符串，不过根据字符串的格式不同，又可以分为3类：
* string：普通字符串
* int：整数类型，可以做自增.自减操作
* float：浮点类型，可以做自增.自减操作

|KEY|VALUE|
|:-:|:-:|
|msg| hello world  |
| num  | 10           |
| score| 92.5         |

String的常见命令有：

|    命令                                 |    描述                                               |
|:-------------------------------------:|:---------------------------------------------------:|
|    `SET`                              |    添加或者**修改**已经存在的一个String类型的键值对                    |
|    `GET`                              |    根据key获取String类型的value                            |
|    `MSET`                             |    **批量**添加多个String类型的键值对                           |
|    `MGET`                             |    根据多个key获取多个String类型的value                        |
|    `INCR`                             |    让一个整型的key自增1                                     |
|    `INCRBY`                           |    让一个整型的key自增并**指定步长**，例如：incrby num 2 让num值自增2    |
|    `INCRBYFLOAT`                      |让一个浮点类型的数字自增并指定步长|
|    `SETNX`                            |    添加一个String类型的键值对，前提是这个key**不存在**，否则不执行           |
|    `SETEX`                            |    添加一个String类型的键值对，并且指定有效期                         |
|  `getrange<key>start end`             |    返回位移为start(从0开始)和end之间(都包括）的子串<br>               |
|`setrange<key>offset substr`|  修改后字符串的长度                                          |
|`append<key>substr`| key存在追加，不存在创建                                       |
> [!attention] 注意
> `setnx`这种特性可以用来设置锁,保证一个key这种临界资源互斥访问


### Key的层级结构

Redis没有类似MySQL中的`Table`的概念，我们该如何区分不同类型的key呢？
例如，需要存储用户商品信息到redis，有一个用户id是1，有一个商品id恰好也是1，此时如果使用id作为key，那就会冲突了，该怎么办？
我们可以通过给key添加**前缀**加以区分，不过这个前缀不是随便加的，有一定的规范：
Redis的key允许有多个单词形成**层级结构**，多个单词之间用'`:`'隔开，格式如下：

<img src="https://article.biliimg.com/bfs/article/ba8533b86925cbf8889d23368963f1877b34af31.png" alt="1652941631682" style="zoom:50%;" />

这个格式并非固定，也可以根据自己的需求来删除或添加词条。

例如我们的项目名称叫 yesho，有user和product两种不同类型的数据，我们可以这样定义key：

- user相关的key：**yesho:user:1**
- product相关的key：**yesho:product:1**

如果Value是一个Java对象，例如一个User对象，则可以将对象序列化为JSON字符串后存储：

|**KEY**|**VALUE**|
|:-:|:-:|
| heima:user:1    | {"id":1, "name": "Jack", "age": 21}       |
| heima:product:1 | {"id":1, "name": "小米11", "price": 4999} |

一旦我们向redis采用这样的方式存储，那么在可视化界面中，redis会以层级结构来进行存储，形成类似于这样的结构，更加方便Redis获取数据

<img src="https://article.biliimg.com/bfs/article/563a685fd40cf3dd4bc337c7cb7b0e6ef54fb2d1.png" alt="1652941883537" style="zoom:50%;" />


### Hash命令

Hash类型，也叫**散列**，其value是一个**无序字典**，类似于Java中的HashMap结构。
String结构是将对象序列化为JSON字符串后存储，当需要修改对象**某个字段**时很不方便：

<img src="https://article.biliimg.com/bfs/article/1f74b3b1c18db8297a0fe0cdff92954294ad209d.png" alt="1652941995945" style="zoom:67%;" />


Hash结构可以将对象中的每个字段**独立存储**，可以针对单个字段做CRUD：

<img src="https://article.biliimg.com/bfs/article/6b12678a0c41983e3f30a777b345484c50cc9893.png" alt="1652942027719" style="zoom:67%;" />


``
**Hash类型的常见命令**

|命令|描述|
|:-:|:-:|
| `HSET key field value` | 添加或者修改hash类型key的field的值 |
| `HGET key field` | 获取一个hash类型key的field的值 |
| `HMSET` | 批量添加多个hash类型key的field的值 |
| `HMGET` | 批量获取多个hash类型key的field的值 |
| `HGETALL` |获取一个hash类型的key中的所有的**field和value**|
| `HKEYS` |获取一个hash类型的key中的所有的field|
| `HINCRBY` | 让一个hash类型key的字段值自增并指定步长 |
| `HSETNX` | 添加一个hash类型的key的field值，前提是这个field不存在，否则不执行 |

### List命令

Redis中的List类型与Java中的LinkedList类似，可以看做是一个双向链表结构。既可以支持正向检索和也可以支持反向检索。
特征也与LinkedList类似：
* 有序
* 元素可以重复
* 插入和删除快
* 查询速度一般
常用来存储一个有序数据，例如：朋友圈点赞列表，评论列表等。

**List的常见命令有：**

|命令|描述|
|:-:|:-:|
| `LPUSH key element ...` |向列表**左侧插入**一个或多个元素|
| `LPOP key` |移除并返回列表**左侧的第一个元素**，没有则返回nil|
| `RPUSH key element ...` |向列表**右侧插入**一个或多个元素|
| `RPOP key` | 移除并返回列表右侧的第一个元素 |
| `LRANGE key star end` |返回一段角标**范围内的所有元素**|
| `BLPOP`和`BRPOP` |与LPOP和RPOP类似，只不过在没有元素时等**待指定时间，**而不是直接返回nil|

<img src="https://article.biliimg.com/bfs/article/9b22125fbe63bce3ea5b6ee4f08c2e51c6484411.png" alt="1652943604992" style="zoom:50%;" />

其中`BLPOP`和`BRPOP`主要用于实现消息队列的阻塞效果。

### Set命令

Redis的Set结构与Java中的HashSet类似，可以看做是一个**value为null**的HashMap。因为也是一个hash表，因此具备与HashSet类似的特征：

* 无序
* 元素不可重复
* 查找快
* 支持交集.并集.差集等功能

**Set类型的常见命令**

|命令|描述|
|:-:|:-:|
| `SADD key member ...`    | 向set中添加一个或多个元素     |
|`SREM key member ...`| 移除set中的指定元素         |
| `SCARD key`              |返回set中元素的**个数**|
|`SISMEMBER key member`|判断一个元素是否存在于set中|
| `SMEMBERS`               | 获取set中的所有元素         |
| `SINTER key1 key2 ...`   | 求key1与key2的交集          |
| `SDIFF key1 key2 ...`    | 求key1与key2的差集          |
| `SUNION key1 key2 ..`    | 求key1和key2的并集           |

### SortedSet类型

Redis的SortedSet是一个**可排序的set集合**，与Java中的`TreeSet`有些类似，但底层数据结构却差别很大。SortedSet中的每一个元素都带有一个**score属性**，可以基于score属性对元素排序，底层的实现是一个[[redis#SkipList|跳表（SkipList]]）加 hash表。

SortedSet具备下列特性：

- 可排序
- 元素不重复
- 查询速度快

因为SortedSet的可排序特性，经常被用来实现排行榜这样的功能。

**SortedSet的常见命令有**： ^7970e5

|命令|描述 |
|:-:|:-:|
| `ZADD key score member`  | 添加一个或多个元素到sorted set ，如果已经存在则更新其score值               |
| `ZREM key member`        | 删除sorted set中的一个指定元素                         |
|`ZSCORE key member`|获取sorted set中的指定元素的score值|
| `ZRANK key member`       |获取sorted set 中的指定元素的**排名**|
| `ZCARD key`              |获取sorted set中的元素个数|
| `ZCOUNT key min max`     |统计**score值在给定范围内的**所有元素的个数|
| `ZINCRBY key increment member`   |让sorted set中的指定元素自增，步长为指定的increment值|
|`ZRANGE key min max`|按照score排序后，获取**指定排名范围内的元素**|
|`ZRANGEBYSCORE key min max withscores`|按照score排序后，获取指定score范围内的元素,withscores是否显示分数|
| `ZDIFF.ZINTER.ZUNION`    | 求差集.交集.并集                                   |
注意：所有的排名默认都是升序，如果要降序则在命令的Z后面添加REV即可，例如：

- **升序**获取sorted set 中的指定元素的排名：`ZRANK key member`
- **降序**获取sorted set 中的指定元素的排名：`ZREVRANK key memeber`


# Redis事务


**Redis事务不满足[[Mysql#事务四大特征|原子性]]**。

大家会有这种疑问，说明并没有用过 Redis 事务，不了解 Redis 事务相关命令的作用，那我们从它的事务命令说起，没用过 Redis 事务的同学看我这一篇就够了~

Redis 事务允许一次性执行一组命令，以命令 `MULTI、EXEC` 为主，还有两个重要的辅助命令：`DISCARD` 和 `WATCH`。

- **MULTI：** **开启事务**，开启后输入的命令都不会直接执行，而是加入**事务命令**队列，如果检测到**参数数量错误**或**命令不存在**，则会中止当前事务并清空队列。而**执行时检测出的错误**并不会阻止其它命令的执行。
- **EXEC：** **执行事务**，按执行命令顺序返回结果。

```bash
127.0.0.1:6379> MULTI
OK

# 不能嵌套事务
127.0.0.1:6379(TX)> MULTI
(error) ERR MULTI calls can not be nested

# 输入事务命令内容
127.0.0.1:6379(TX)> SET a 1
QUEUED
127.0.0.1:6379(TX)> GET a
QUEUED

# 执行事务
127.0.0.1:6379(TX)> EXEC
1) OK
2) "1"

# 开启新事务，测试执行出错
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379(TX)> SET a 666
QUEUED

# 语法错误命令
127.0.0.1:6379(TX)> SET a a b c
QUEUED

127.0.0.1:6379(TX)> GET a
QUEUED

# 执行出错不会阻止其它命令的执行
127.0.0.1:6379(TX)> EXEC
1) OK
2) (error) ERR syntax error
3) "666"
```

- **DISCARD：** **终止事务**，清空**命令队列**并终止事务。
- **WATCH：** **监听 key**，被监听的 key 如果在事务之外被修改，则事务不会执行（EXEC 时结果返回 nil）。
- **UNWATCH：** **取消监听 key**。

```shell
# 监听 "a"
127.0.0.1:6379> WATCH a
OK

# 在开启事务前修改 a
127.0.0.1:6379> SET a 3
OK

# 事务
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379(TX)> SET a 2
QUEUED

# 结果为 nil，未执行任何命令
127.0.0.1:6379(TX)> EXEC
(nil)

# 验证是否未执行 SET a 2
127.0.0.1:6379> GET a
"3"
```


以上 5 个命令就是 Redis 事务提供的全部功能。
**Redis 事务是否满足 ACID 四大特性**：
Redis 事务具有原子性吗？  
答：Redis 事务不具有原子性。原子性指事务中的操作要么全部成功，要么全部失败，如果中途出错需要回滚到事务之前的状态。而 Redis 事务执行出错不会阻止其它命令的执行，所以并不满足这个条件。
**Redis 事务具有隔离性吗**？  
答：Redis 事务具有隔离性。隔离性原本指并发事务不会相互影响，但由于 Redis 是单线程不存在并发事务，所以我们考虑事务和事务之外的命令执行情况，Redis 事务执行时不会执行事务之外的命令，所以可以说它具有隔离性。
**Redis 事务具有一致性吗**？  
答：一致性指事务执行前后要满足相同的约束条件（或者说相同的一致性状态），比如转账前后两个人余额总和要相同。一致性属于应用范畴，我们不讨论 Redis 事务是否具有一致性。
**Redis 事务具有持久性吗**？  
答：Redis 事务不具有持久性。持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来即使数据库发生故障也不应该对其有任何影响。然而 Redis 事务提交后也不会强制进行持久化，所以不满足该条件。  综上所述，Redis只能说具有事务特性中的隔离性


# Redis的Java客户端-Jedis

在Redis官网中提供了各种语言的客户端，地址：[Get started using Redis clients | Redis](https://redis.io/docs/clients/)

<img src="https://i.imgur.com/9f68ivq.png" style="zoom:50%;" />
其中Java客户端也包含很多：

<img src="https://article.biliimg.com/bfs/article/a02138c8995e12de58ce604eba311ce1e211d6b8.png" alt="image-20220609102817435" style="zoom:50%;" />

标记为❤的就是推荐使用的java客户端，包括：

- Jedis和Lettuce：这两个主要是提供了Redis命令对应的API，方便我们操作Redis，而SpringDataRedis又对这两种做了抽象和封装，因此我们后期会直接以SpringDataRedis来学习。
- Redisson：是在Redis基础上实现了分布式的可伸缩的java数据结构，例如Map.Queue等，而且支持跨进程的同步机制：Lock.Semaphore等待，比较适合用来实现特殊的功能需求。

## Jedis快速入门

1）引入依赖：
```xml
<!--jedis-->
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.7.0</version>
</dependency>
<!--单元测试-->
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.7.0</version>
    <scope>test</scope>
</dependency>
```

2）建立连接
新建一个单元测试类，内容如下：

```java
private Jedis jedis;
@BeforeEach
void setup()
{
	 //建立连接
	jedis=new Jedis("192.168.153.128",6379);
	//设置密码
	jedis.auth("701121");
	//选择库
	jedis.select(0);
}
```

3）测试：

```java
@Test
void testString()
{
	//存入数据
	String result=jedis.set("name","胡哥");
	System.out.println("result = " + result);
	//获取数据
	System.out.println(jedis.get("name"));
}
@Test
void testHash()
{
	//存入数据
	jedis.hset("user:1","name","yesho");
	jedis.hset("user:1","age",23);
	//获取
	Map<String, String> map=jedis.hgetAll("user:1");
	System.out.println(map);
}
```

4）释放资源

```java
@AfterEach
void tearDown()
{
	if(jedis!=null)
	{
		jedis.close();
	}
}
```

### Jedis连接池

Jedis本身是**线程不安全的**，并且频繁的创建和销毁连接会有性能损耗，因此我们推荐大家使用Jedis连接池代替Jedis的直连方式
有关池化思想，并不仅仅是这里会使用，很多地方都有，比如说我们的数据库连接池，比如我们tomcat中的线程池，这些都是池化思想的体现。

**创建Jedis的连接池**

```java
public class JedisConnectionFactory{
	//声明池子变量
	private static final JedisPool pool;
	//采用静态代码块对池子进行初始化
	static {  
	  //配置连接池  
	  JedisPoolConfig jedisPoolConfig=new JedisPoolConfig();  
	  //最大连接  
	  jedisPoolConfig.setMaxTotal(8);  
	  //最大空闲连接 ---> 预备用的  
	  jedisPoolConfig.setMaxIdle(8);  
	  //最小空闲连接  
	  jedisPoolConfig.setMinIdle(0);  
	  //设置最长等待时间  
	  jedisPoolConfig.setMaxWaitMillis(1000);  
	  //创建连接池对象  
	  JEDIS_POOL =new JedisPool(jedisPoolConfig,"192.168.153.128",6379,1000,"701121");  
	}
	public static Jedis getJedis()
	{
		return pool.getResource();
	}
}
```


**代码说明：**

- 1）`JedisConnectionFacotry`：[[java基础篇#工厂设计模式|工厂设计模式]]是实际开发中非常常用的一种设计模式，我们可以使用工厂，去降低代的耦合，比如Spring中的Bean的创建，就用到了工厂设计模式
- 2）[[java基础篇#代码块|静态代码块]]：随着类的加载而加载，确保只能执行一次，我们在加载当前工厂类的时候，就可以执行static的操作完成对 连接池的初始化
- 3）最后提供返回连接池中连接的方法.

# Redis的Java客户端-SpringDataRedis

SpringData是Spring中数据操作的模块，包含对各种数据库的集成，其中对Redis的集成模块就叫做`SpringDataRedis`，官网地址：[Spring Data Redis](https://spring.io/projects/spring-data-redis)

* 提供了对不同Redis客户端的**整合**（`Lettuce和Jedis`）
* 提供了`RedisTemplate`统一API来操作Redis
* 支持Redis的发布**订阅模型**
* 支持[[redis#哨兵原理|Redis哨兵]]和[[redis#集群结构和作用|Redis集群]]
* 支持基于Lettuce的**响应式编程**
* 支持基于JDK.JSON.字符串.Spring对象的数据序列化及反序列化
* 支持基于Redis的JDKCollection实现

SpringDataRedis中提供了RedisTemplate工具类，其中封装了各种对Redis的操作。并且将不同数据类型的操作API封装到了不同的类型中：


|API|返回值类型|说明|
|:-:|:-:|:-:|
| `redisTemplate.opsForValue()` | `ValueOperations` |操作**String类**型数据|
| `redisTemplate.opsForHash()`  | `HashOperations`  |操作**Hash**类型数据|
| `redisTemplate.opsForList()`  | `ListOperations`  |操作**List**类型数据|
| `redisTemplate.opsForSet()`   | `SetOperations`   |操作**Set**类型数据|
| `redisTemplate.opsForZSet()`  | `ZSetOperations`  |操作**SortedSet**类型数据|
| `redisTemplate`              | -             | 通用的命令             |

## 快速入门

SpringBoot已经提供了对SpringDataRedis的支持，使用非常简单：

导入pom坐标

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.1.1</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>java_project</groupId>
    <artifactId>SpringDataRedis-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>SpringDataRedis-demo</name>
    <description>SpringDataRedis-demo</description>
    <properties>
        <java.version>17</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
        </dependency>
        <!--Jackson依赖-->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.project-lombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```


配置文件
```yml
spring:
  main:
  banner-mode: off
  data:
    redis:
      host: 192.168.153.128
      port: 6379
      password: 701121
      lettuce:
        pool:
          max-active: 8
          max-idle: 8
          min-idle: 0
          max-wait: 100
```

测试代码

```java

@SpringBootTest
class RedisDemoApplicationTests{
	@Autowired  
	private RedisTemplate<String,Object> redisTemplate;

	@Test
	void testString()
	{
		//写入一条String数据
		redisTemplate.opsForValue().set("name","yesho");
		 Object name =redisTemplate.opsForValue().get("name");
		 System.out.println("name = " + name);
	}
}
```

**贴心小提示：SpringDataJpa使用起来非常简单，记住如下几个步骤即可**

SpringDataRedis的使用步骤：
- 引入spring-boot-starter-data-redis依赖
- 在application.yml配置Redis信息
- 注入RedisTemplate

### 数据序列化器

RedisTemplate可以接收任意Object作为值写入Redis：

<img src="https://i.imgur.com/OEMcbuu.png" style="zoom:50%;" />
只不过写入前会把Object序列化为字节形式，默认是采用JDK序列化，得到的结果是这样的：

<img src="https://i.imgur.com/5FjtWk5.png" style="zoom:50%;" />

缺点：
- 可读性差
- 内存占用较大

我们可以自定义RedisTemplate的序列化方式，代码如下

```java
@Configuration  
public class RedisConfig {  
  @Resource  
  private RedisConnectionFactory connectionFactory;  
  @Bean  
  public RedisTemplate<String, Object> redisTemplate() {  
    //创建RedisTemplate对象  
    RedisTemplate<String, Object> stringObjectRedisTemplate =  
            new RedisTemplate<String,Object>();  
    //设置连接工厂  
    stringObjectRedisTemplate.setConnectionFactory(connectionFactory);  
    //创建Json序列化工具  
    GenericJackson2JsonRedisSerializer jackson2JsonRedisSerializer =  
            new GenericJackson2JsonRedisSerializer();  
    //设置key的序列化工具  
    stringObjectRedisTemplate.setKeySerializer(new StringRedisSerializer());  
    stringObjectRedisTemplate.setHashKeySerializer(new StringRedisSerializer());  
    //设置Value的序列化  
    stringObjectRedisTemplate.setValueSerializer(jackson2JsonRedisSerializer);  
    stringObjectRedisTemplate.setHashValueSerializer(jackson2JsonRedisSerializer);  
    //返回  
    return stringObjectRedisTemplate;  
  }  
}
```

这里采用了JSON序列化来代替默认的JDK序列化方式。最终结果如图：

<img src="https://i.imgur.com/XOAq3cN.png" style="zoom:50%;" />

整体可读性有了很大提升，并且能将Java对象自动的序列化为JSON字符串，并且查询时能自动把JSON反序列化为Java对象。不过，其中记录了序列化时对应的class名称，目的是为了查询时实现自动反序列化。这会带来额外的内存开销。

### StringRedisTemplate

尽管JSON的序列化方式可以满足我们的需求，但依然存在一些问题，如图：

<img src="https://article.biliimg.com/bfs/article/bac026a876d58784365ddcaeefeffeec1af5bd12.png" alt="1653054602930" style="zoom:50%;" />

为了在反序列化时知道对象的类型，JSON序列化器会将类的class类型写入json结果中，存入Redis，会带来额外的内存开销。

为了减少内存的消耗，我们可以采用**手动序列化**的方式，换句话说，就是不借助默认的序列化器，而是我们自己来控制序列化的动作，同时，我们只采用String的序列化器，这样，在存储value时，我们就不需要在内存中就不用多存储数据，从而节约我们的内存空间

<img src="https://article.biliimg.com/bfs/article/24bb7e88f5231ac86ed1b56a498c12eb4dbb6d35.png" alt="image.png" style="zoom:50%;" />

这种用法比较普遍，因此SpringDataRedis就提供了RedisTemplate的子类：StringRedisTemplate，它的key和value的序列化方式默认就是String方式。

省去了我们自定义RedisTemplate的序列化方式的步骤，而是直接使用：

```java
@SpringBootTest
class RedisStringTests {

    @Autowired
    private StringRedisTemplate stringRedisTemplate;

    @Test
    void testString() {
        // 写入一条String数据
        stringRedisTemplate.opsForValue().set("verify:phone:13600527634", "124143");
        // 获取string数据
        Object name = stringRedisTemplate.opsForValue().get("name");
        System.out.println("name = " + name);
    }

    private static final ObjectMapper mapper = new ObjectMapper();

    @Test
    void testSaveUser() throws JsonProcessingException {
        // 创建对象
        User user = new User("虎哥", 21);
        // 手动序列化
        String json = mapper.writeValueAsString(user);
        // 写入数据
        stringRedisTemplate.opsForValue().set("user:200", json);

        // 获取数据
        String jsonUser = stringRedisTemplate.opsForValue().get("user:200");
        // 手动反序列化
        User user1 = mapper.readValue(jsonUser, User.class);
        System.out.println("user1 = " + user1);
    }

}
```

此时我们再来看一看存储的数据，小伙伴们就会发现那个class数据已经不在了，节约了我们的空间~

<img src="https://article.biliimg.com/bfs/article/d4e608a453c4542d35cd43038d358f6467143861.png" alt="1653054945211" style="zoom:50%;" />
最后小总结：

RedisTemplate的两种序列化实践方案：

* 方案一：
  * 自定义RedisTemplate
  * 修改RedisTemplate的序列化器为`GenericJackson2JsonRedisSerializer`

* 方案二：
  * 使用StringRedisTemplate
  * 写入Redis时，手动把对象序列化为JSON
  * 读取Redis时，手动把读取到的JSON反序列化为对象

### Hash结构操作

在基础篇的最后，咱们对Hash结构操作一下，收一个小尾巴，这个代码咱们就不再解释啦

```java
@SpringBootTest
class RedisStringTests {

    @Autowired
    private StringRedisTemplate stringRedisTemplate;


    @Test
    void testHash() {
        stringRedisTemplate.opsForHash().put("user:400", "name", "虎哥");
        stringRedisTemplate.opsForHash().put("user:400", "age", "21");

        Map<Object, Object> entries = stringRedisTemplate.opsForHash().entries("user:400");
        System.out.println("entries = " + entries);
    }
}
```

# 实战篇Redis
## 短信登录
### 基于Session实现登录流程

<img src="https://article.biliimg.com/bfs/article/20692aa84b2955c9ccc28da10c19e076f0c9a604.png" alt="1653066208144" style="zoom:80%;" />

#### 实现发送短信验证码功能

**页面流程**

<img src="https://article.biliimg.com/bfs/article/b49adc3448145a1d0702067382a8c1a9655ac434.png" alt="image.png" style="zoom:60%;" />

发送验证码流程 (在UserController中完成，可以直接在service中写未创建的方法)
1. 发送短信验证码并保存验证码
2. 先校验手机号(在service实现类里面,里面的`RegexUtils`和`RegexPatterns`用来完成这个正则化验证手机号) 
3. 不符合返回错误的信息(通过result)
4. 符合进行生成验证码 (这里的生成的6位用到引入的hutool-all工具类)
5. 保存验证码到session
6. 发送验证码(这里需要用到阿里云一些平台，比较麻烦，用`log.debug`代替)
7. 结束ok   

这个设计的不好，应该phone是key code是value，一会改一下

#### 用户注册与登录
登录流程
检测用户是否存在是通过查询tb_user表是否存在这个用户的手机号

#### 校验登录
校验登录状态流程：

<img src="https://article.biliimg.com/bfs/article/38fdff2d3d7b67af84755890f77b35afc28972e6.png" alt="1653068874258" style="zoom:50%;" />

把用户信息存储在session中，则需要在跨各种处理组件或页面时都要从session中去取得用户信息，这增加了访问session的开销，所以我们这里用[[java基础篇#线程变量(ThreadLocal)|ThreadLocal]]
用拦截器进行校验，但是拦截器的得到的用户要传给控制器，所以要保存到`ThreadLocal`中(在UserHolder工具类 里面)
在整个请求完成后要将用户进行移除，防止内存泄露


#### 隐藏用户敏感信息

1. 这里我们采用的方法是将User类转成简化的UserDTO,用到hutool-all工具类的`BeanUtil.copyProties`的方法

#### session共享问题

**核心思路分析：**

每个tomcat中都有一份属于自己的session,假设用户第一次访问第一台tomcat，并且把自己的信息存放到第一台服务器的session中，但是第二次这个用户访问到了第二台tomcat，那么在第二台服务器上，肯定没有第一台服务器存放的`session`，所以此时 整个登录拦截功能就会出现问题，我们能如何解决这个问题呢？早期的方案是**session拷贝**，就是说虽然每个tomcat上都有不同的session，但是每当任意一台服务器的session修改时，都会同步给其他的Tomcat服务器的session，这样的话，就可以实现session的共享了

但是这种方案具有两个大问题
1. 每台服务器中都有完整的一份session数据，服务器压力过大。
2. session拷贝数据时，可能会出现延迟

<img src="https://article.biliimg.com/bfs/article/100c48e0484a3ba7e89d6c135a25f546202b69b5.png" alt="1653069893050" style="zoom:50%;" />

所以咱们后来采用的方案都是基于`redis`来完成，我们把session换成redis，redis数据本身就是共享的，就可以避免session共享的问题了

#### Redis代替session的业务流程

##### 设计key的结构

首先我们要思考一下利用redis来存储数据，那么到底使用哪种结构呢？由于存入的数据比较简单，我们可以考虑使用String，或者是使用哈希，如下图，如果使用String，同学们注意他的value，用多占用一点空间，如果使用哈希，则他的value中只会存储他数据本身，如果不是特别在意内存，其实使用String就可以啦。

##### 设计key的具体细节

所以我们可以使用String结构，就是一个简单的key，value键值对的方式，但是关于key的处理，session他是每个用户都有自己的session，但是redis的key是**共享**的，咱们就不能使用code了
在设计这个key的时候，我们之前讲过需要满足两点
1. key要具有唯一性
2. key要方便携带
如果我们采用phone：手机号这个的数据来存储当然是可以的，但是如果把这样的敏感数据存储到redis中并且从页面中带过来毕竟不太合适，所以我们在后台生成一个**随机串token**，然后让前端带来这个token就能完成我们的整体逻辑了

##### 整体访问流程

<img src="https://article.biliimg.com/bfs/article/61c2377f83b693d7d06332c36abea6e6a5a76428.png" alt="image.png" style="zoom:50%;" />

1. 完成发送短信验证码的更改
	- 以保存到redis中(这个要用到前面所学到的api)，改成以手机号为key
	- 加验证码有效期的功能
2. 完成短信验证码登录、注册的更改
	- 改成从redis获取手机号和验证码
	- 校验后改成保存用户到redis中
	- 生成一个随机token,作为token令牌(这里采用[[MyBatis-plus#UUID|UUID]])
	- 这里要将用户实体类改成Hash类型(用到`BeanUtil.beanToMap`)
	- 加token有效期的功能
<img src="https://article.biliimg.com/bfs/article/68972853fb0e74643198f66ef24ce51144ef31df.png" alt="1653319474181" style="zoom:70%;" />

3. 完成校验登录的更改
	- 获取请求头中的token (这里前端代码完成了用户token放入请求头中，key为authorization)
	- 这里注意，因为有这步所以用`jmeter`软件测试时要一定要携带这个`token`进入，让系统知道我是哪一个用户，不然会被拦截发生401错误
	- <img src="https://article.biliimg.com/bfs/article/3d767d0f3de01894e2c6f3d2956c4f6914b68c7a.png" alt="image.png" style="zoom:50%;" />
	- 
		- 用strUtil判断token是否为空 空进行拦截
	- 基于token获取redis中的用户
	- 将hash结构转成用户实体类(BeanUtil.fillBeanWithMap)
	- 刷新token有效期

##### 解决状态登录刷新问题

<img src="https://article.biliimg.com/bfs/article/ddf382a8a2abd6dd4d5bc1b571f0180f57360e86.png" alt="1653320822964" style="zoom:60%;" />

在这个方案中，他确实可以使用对应路径的拦截，同时刷新登录token令牌的存活时间，但是现在这个拦截器他**只是拦截需要被拦截的路径**，假设当前用户访问了一些不需要拦截的路径，那么这个拦截器就不会生效，所以此时令牌刷新的动作实际上就不会执行，所以这个方案他是存在问题的


既然之前的拦截器无法对不需要拦截的路径生效，那么我们可以**添加一个拦截器**，在第一个拦截器中**拦截所有的路径**，把第二个拦截器做的事情放入到第一个拦截器中，同时刷新令牌，因为第一个拦截器有了threadLocal的数据，所以此时第二个拦截器只需要判断拦截器中的user对象是否存在即可，完成整体刷新功能。

<img src="https://article.biliimg.com/bfs/article/d69e2c6a78aaefde5dad0f927706a169d5c3eb4e.png" alt="1653320764547" style="zoom:70%;" />

## 商户查询缓存

### 什么是缓存?

**缓存**(Cache),就是数据交换的**缓冲区**,俗称的缓存就是**缓冲区内的数据**,一般从数据库中获取,存储于本地代码(例如:

```java
例1:Static final ConcurrentHashMap<K,V> map = new ConcurrentHashMap<>(); 本地用于高并发
例2:static final Cache<K,V> USER_CACHE = CacheBuilder.newBuilder().build(); 用于redis等缓存
例3:Static final Map<K,V> map =  new HashMap(); 本地缓存
```

由于其被**Static**修饰,所以随着类的加载而被加载到**内存之中**,作为本地缓存,由于其又被**final**修饰,所以其引用(例3:map)和对象(例3:new HashMap())之间的关系是固定的,不能改变,因此不用担心赋值(=)导致缓存失效;

#### 为什么要使用缓存

1. 因为**速度快,好用**
2. 缓存数据存储于**代码**中,而代码**运行在内存**中,内存的读写性能远高于磁盘,缓存可以大大降低**用户访问并发量带来的**服务器读写压力
3. 实际开发过程中,企业的数据量,少则几十万,多则几千万,这么大数据量,如果没有缓存来作为"避震器",系统是几乎撑不住的,所以企业会大量运用到缓存技术;

但是缓存也会增加代码复杂度和运营的成本:

<img src="https://article.biliimg.com/bfs/article/fa64b54403f880c2de9d12966d0df01eff494fd3.png" alt="image.png" style="zoom:50%;" />
#### 如何使用缓存

实际开发中,会构筑**多级缓存**来使系统运行速度进一步提升,例如:本地缓存与redis中的缓存并发使用

- **浏览器缓存**：主要是存在于浏览器端的缓存
- **应用层缓存：** 可以分为tomcat本地缓存，比如之前提到的`map`，或者是使用redis作为缓存
- **数据库缓存：** 在数据库中有一片空间是 buffer pool，增改查数据都会先加载到mysql的缓存中
- **CPU缓存：** 当代计算机最大的问题是 cpu性能提升了，但内存读写速度没有跟上，所以为了适应当下的情况，增加了cpu的L1，L2，L3级的缓存

<img src="https://article.biliimg.com/bfs/article/a30a3473d5d8629bf64850f8bb8c05f80111f646.png" style="zoom:60%;" />
### 添加商户缓存

标准的操作方式就是查询数据库之前先**查询缓存**，如果缓存数据存在，则直接从缓存中返回，如果缓存数据不存在，再查询数据库，然后将数据存入redis。

<img src="https://article.biliimg.com/bfs/article/695f76a4d6a90f42bc64245656fed8d807d2ee81.png" alt="1653322097736" style="zoom:50%;" />

流程：
1. 从redis中查询缓存
2. 判断是否存在
3. 存在直接返回(这里采用string要转成java类用`JSONUtil.tobean`)
4. 不存在，根据id查询数据库
5. id不存在，返回报错
6. id存在，将商铺写入到redis缓存
7. 返回

### 缓存更新策略

缓存更新是redis为了节约内存而设计出来的一个东西，主要是因为内存数据宝贵，当我们向redis插入太多数据，此时就可能会导致缓存中的数据过多，所以redis会对部分数据进行更新，或者把他叫为淘汰更合适。

**内存淘汰：** redis自动进行，当redis内存达到咱们设定的**max-memery**的时候，会自动触发**淘汰机制**，淘汰掉一些不重要的数据(可以自己设置策略方式)
**超时剔除：** 当我们给redis设置了**过期时间ttl**之后，redis会将超时的数据进行删除，方便咱们继续使用缓存
**主动更新：** 我们可以**手动调用方法**把缓存删掉，通常用于解决缓存和数据库不一致问题


|       |内存淘汰| 超时剔除 | 主动更新 |
|:-:|:-:|:-:|:-:|
|**说明**| 不用自己维护，利用Redis的内存淘汰机制，当内存不足时自动淘汰部分数据。下次查询时更新缓存 | 给缓存数据添加TTL时间，到期后自动删除缓存。下次查询时更新缓存 | 编写业务逻辑，在修改数据库的同时，更新缓存。 |
|**一致性**|差| 一般     | 好      |
|**维护成本**|无| 低      | 高      |

业务场景： 
- 低一致性需求：使用内存淘汰机制。例如店铺类型的查询缓存 
- 高一致性需求：主动更新，并以超时剔除作为兜底方案。例如店铺详情查询的缓存 

#### 数据库缓存不一致解决方案：

由于我们的**缓存的数据源来自于数据库**,而数据库的**数据是会发生变化的**,因此,如果当数据库中**数据发生变化,而缓存却没有同步**,此时就会有**一致性问题存在**,其后果是:
用户使用缓存中的过时数据,就会产生类似**多线程数据安全问题**,从而影响业务,产品口碑等;怎么解决呢？有如下几种方案

- `Cache Aside Pattern` 人工编码方式：缓存调用者在更新完数据库后再去更新缓存，也称之为双写方案
- `Read/Write Through Pattern` : 由系统本身完成，数据库与缓存的问题交由系统本身去处理
- `Write Behind Caching Pattern` ：调用者只操作缓存，其他线程去异步处理数据库，实现最终一致

<img src="https://article.biliimg.com/bfs/article/431a3b45453ea8b051dea47e6a7c8bca56bdd749.png" alt="image.png" style="zoom:50%;" />

#### 数据库和缓存不一致采用什么方案

如果采用第一个方案，那么假设我们每次操作数据库后，都操作缓存，但是中间如果没有人查询，那么这个更新动作实际上**只有最后一次生效**，中间的更新动作意义并不大，我们可以把缓存删除，等待再次查询时，将缓存中的数据加载出来
- 删除缓存还是更新缓存？ ^9f80a1
    - 更新缓存：每次更新数据库都更新缓存，无效写操作较多
    - 删除缓存：更新数据库时让缓存失效，查询时再更新缓存
- 如何保证缓存与数据库的操作的同时成功或失败？
    - 单体系统，将缓存与数据库操作放在**一个事务**
    - 分布式系统，利用T**CC等分布式**事务方案

应该具体操作缓存还是操作数据库，我们应当是先操作数据库，再删除缓存，原因在于，如果你选择第一种方案，在两个线程并发来访问时，假设线程1先来，他先把缓存删了，此时线程2过来，他查询缓存数据并不存在，此时他写入缓存，当他写入缓存后，线程1再执行更新动作时，实际上写入的就是旧的数据，新的数据被旧数据覆盖了。

- 先操作缓存还是先操作数据库？
    - 先删除缓存，再操作数据库
    - 先操作数据库，再删除缓存

**先删除缓存，再操作数据库**

<img src="https://article.biliimg.com/bfs/article/0e92a1e4da2cfdf89c5da8be0a5d099ee45a87c2.gif" alt="image.png" style="zoom:70%;" />


**先操作数据库，再删除缓存**

<img src="https://article.biliimg.com/bfs/article/216d49aba1c31f2e14652876578fde2715b74bcd.gif" alt="image.png" style="zoom:50%;" />


### 实现商铺和缓存与数据库双写一致

核心思路如下：
修改ShopController中的业务逻辑，满足下面的需求：
根据id查询店铺时，如果缓存未命中，则查询数据库，将数据库结果写入缓存，并设置超时时间
根据id修改店铺时，先修改数据库，再删除缓存

1. 给id查询店铺的操作设置超时时间
2. 写更新店铺的操作，先更新数据库，后删除缓存(这里要添加spring事务)


### 缓存穿透问题的解决思路

缓存穿透 ：缓存穿透是指客户端请求的数据在**缓存**中和**数据库**中都**不存在**，这样缓存永远不会生效，这些请求都会打到数据库。
常见的解决方案有两种：
- 缓存空对象
    - 优点：实现简单，维护方便
    - 缺点：
        - 额外的**内存消耗**
        - 可能造成短期的不一致

- 布隆过滤
    - 优点：内存占用较少，没有多余key
    - 缺点：
        - 实现复杂
        - 存在误判可能

<img src="https://article.biliimg.com/bfs/article/ebb73e928255bc275d726dc3ec166255b354b45f.png" alt="1653326156516" style="zoom:70%;" />

### 编码解决商品查询的缓存穿透问题：

核心思路如下：
在原来的逻辑中，我们如果发现这个数据在mysql中不存在，直接就返回404了，这样是会存在缓存穿透问题的
现在的逻辑中：如果这个数据不存在，我们不会返回404 ，还是会**把这个数据写入到Redis中**，并且将value设置为**空**，当再次发起查询时，我们如果发现命中之后，判断这个value是否是null，如果是null，则是之前写入的数据，证明是缓存穿透数据，如果不是，则直接返回数据。

<img src="https://article.biliimg.com/bfs/article/be2b3ab2c74ebc15660514d1f4661eb6c8b905d0.png" alt="1653327124561" style="zoom:60%;" />


流程：
1. 返回404那步改成将空值写入redis
2. 加上判断是否是空值


缓存穿透产生的原因是什么？

* 用户请求的数据在缓存中和数据库中都不存在，不断发起这样的请求，给数据库带来巨大压力

缓存穿透的解决方案有哪些？

* 缓存null值
* 布隆过滤
* ❌ 为什么增强id的复杂度，避免被猜测id规律，可以解决缓存穿透  ^73a575
* 做好数据的基础格式校验
* 加强用户权限校验
* 做好热点参数的限流


### 缓存雪崩问题及解决思路

缓存雪崩是指在同一时段**大量的缓存key同时失效**或者**Redis服务宕机**，导致大量请求到达数据库，带来巨大压力。

解决方案：
* 给不同的Key的**TTL添加随机值**
* 利用**Redis集群**提高服务的可用性
* 给缓存业务添加降级**限流策略**
* 给业务添加**多级缓存**

<img src="https://article.biliimg.com/bfs/article/f6d3b49879f6ae48359db6655f4fa760aa802a2d.png" alt="1653327884526" style="zoom:80%;" />
### 缓存击穿问题及解决思路
这里的缓存重建业务比较复杂是什么意思？
缓存击穿问题也叫**热点Key问题**，就是一个被**高并发访问**并且[[零散知识点#缓存重建业务较复杂的key|缓存重建业务较复杂]]的key突然失效了，无数的请求访问会在瞬间给数据库带来巨大的冲击。

常见的解决方案有两种：
* **互斥锁**
* **逻辑过期**

逻辑分析：假设线程1在查询缓存之后，本来应该去查询数据库，然后把这个数据重新加载到缓存的，此时只要线程1走完这个逻辑，其他线程就都能从缓存中加载这些数据了，但是假设在线程1没有走完的时候，后续的线程2，线程3，线程4同时过来访问当前这个方法， 那么这些线程都不能从缓存中查询到数据，那么他们就会同一时刻来访问查询缓存，都没查到，接着同一时间去访问数据库，同时的去执行数据库代码，对数据库访问压力过大

<img src="https://article.biliimg.com/bfs/article/eaecd0e9569f0aefddc05b95aa130eea7931a652.png" alt="1653328022622" style="zoom:80%;" />

解决方案一、使用锁来解决：


因为锁能实现互斥性。假设线程过来，只能一个人一个人的来访问数据库，从而避免对于数据库访问压力过大，但这也会影响查询的性能，因为此时会让查询的性能**从并行变成了串行**，我们可以采用tryLock方法 +[[零散知识点#double check| double check]]来解决这样的问题。



假设现在线程1过来访问，他查询缓存没有命中，但是此时他获得到了锁的资源，那么线程1就会一个人去执行逻辑，假设现在线程2过来，线程2在执行过程中，并没有获得到锁，那么线程2就可以进行到休眠，直到线程1把锁释放后，线程2获得到锁，然后再来执行逻辑，此时就能够从缓存中拿到数据了。

<img src="https://article.biliimg.com/bfs/article/db60ea6395b9f6bfb9693d6dae756a9725964fcf.png" alt="1653328288627" style="zoom:60%;" />

解决方案二、逻辑过期方案

方案分析：我们之所以会出现这个缓存击穿问题，主要原因是在于我们对**key设置了过期时间**，假设我们不设置过期时间，其实就不会有缓存击穿的问题，但是不设置过期时间，这样数据不就一直占用我们内存了吗，我们可以采用**逻辑过期方案**。

我们把过期时间设置在redis的value中，注意：==这个过期时间并不会直接作用于redis==，而是我们**后续通过逻辑去处理**。假设线程1去查询缓存，然后从value中判断出来当前的数据已经过期了，此时线程1去获得互斥锁，那么其他线程会进行阻塞，获得了锁的线程他会**开启一个线程**去进行以前的重构数据的逻辑，直到新开的线程完成这个逻辑后，才释放锁， 而线程1直接进行返回，假设现在线程3过来访问，由于线程线程2持有着锁，所以线程3无法获得锁，线程3也直接返回数据，只有等到新开的线程2把重建数据构建完后，其他线程才能走返回正确的数据。
这种方案巧妙在于，异步的构建缓存，缺点在于在构建完缓存之前，**返回的都是脏数据**。

<img src="https://article.biliimg.com/bfs/article/9a0ed79ea5b77b6148a5c426150825045fe927aa.png" alt="1653328663897" style="zoom:60%;" />


进行对比
**互斥锁方案：** 由于保证了互斥性，所以**数据一致**，且实现简单，因为仅仅只需要加一把锁而已，也没其他的事情需要操心，所以没有额外的内存消耗，缺点在于有锁就有死锁问题的发生，且只能**串行执行性能**肯定受到影响
**逻辑过期方案：** 线程读取过程中不需要等待，性能好，有一个额外的线程持有锁去进行重构数据，但是在重构数据完成前，其他的线程只能返回之前的数据，且实现起来麻烦

<img src="https://article.biliimg.com/bfs/article/e319c91910c0fdb4aa98b709738a467116f577db.png" style="zoom:60%;" />


### 利用互斥锁解决缓存击穿问题

核心思路：相较于原来从缓存中查询不到数据后直接查询数据库而言，现在的方案是进行查询之后，如果从**缓存没有查询到数据**，则**进行互斥锁的获取**，获取互斥锁后，判断是否获得到了锁，如果没有获得到，则**休眠**，过一会再进行尝试，直到获取到锁为止，才能进行查询
如果获取到了锁的线程，再去进行查询，查询后将数据写入redis，再释放锁，返回数据，利用互斥锁就能保证只有一个线程去执行操作数据库的逻辑，防止缓存击穿

<img src="https://article.biliimg.com/bfs/article/6622dedfe62905cd62054c6669eeb8b709cf2ce8.png" alt="1653357860001" style="zoom:60%;" />

核心思路：
1. 这里我们实现锁方式是用过redis的`setnx`这个命令，因为setnx只有key不存在才创建，有返回值，具有互斥性
2. 我们会单独定义`trylock`和`unlock`的函数来实现互斥锁,这里要用`double check`
3. 休眠用`Thread.sleep`
4. 代码健壮性unlock是一定要执行的，用finally结构
5. 测试为了达到高并发的效果我们要在获取商铺后，进行休眠，增加业务的执行时间

###  利用逻辑过期解决缓存击穿问题

**需求：修改根据id查询商铺的业务，基于逻辑过期方式来解决缓存击穿问题**

思路分析：当用户开始查询redis时，判断是否命中，如果**没有命中则直接返回空数据**(没有命中的一定不是**热点key**)，不查询数据库，而一旦命中后，将value取出，判断value中的过期时间是否满足，如果没有过期，则直接返回redis中的数据，如果过期，则在**开启独立线程**后直接**返回之前的数据**，独立线程去重构数据，重构完成后释放互斥锁。

<img src="https://article.biliimg.com/bfs/article/1d11accae91e84116d97381c4b79b9cc577c07ee.png" alt="image.png" style="zoom:70%;" />

核心步骤：
1. 数据写入redis的逻辑过期时间，是自己写一个工具类`RedisDate` 里面设置一个过期时间的变量。
	- 让实体类继承工具类`RedisDate` 
	- 在工具类`RedisDate` 添加一个Object类型的实体类变量
2. 写一个方法将店铺信息和过期时间提前存入到Redis中
3. 写一个测试类测试是否能存入redis中
4. 这里的开启线程使用[[java基础篇#线程池|线程池]]

测试步骤：
1. 测试高并发情况下是不是都来进行缓存的重建
2. 在缓存重建之前我们得到时旧数据还是新数据


### 封装Redis工具类

基于StringRedisTemplate封装一个缓存工具类，满足下列需求：

* 方法1：将任意Java对象序列化为json并存储在string类型的key中，并且可以设置TTL过期时间
* 方法2：将任意Java对象序列化为json并存储在string类型的key中，并且可以设置逻辑过期时间，用于处理缓

存击穿问题

* 方法3：根据指定的key查询缓存，并反序列化为指定类型，利用缓存空值的方式解决缓存穿透问题
	* 这里大量使用泛型
	* 查询方法用到了函数式接口
* 方法4：根据指定的key查询缓存，并反序列化为指定类型，需要利用逻辑过期解决缓存击穿问题

## 优惠卷秒杀

### 全局唯一ID

每个店铺都可以发布优惠券：

<img src="https://article.biliimg.com/bfs/article/2625aea5b8feafae5a26272203f1af9d2f68e822.png" alt="1653362612286" style="zoom:50%;" />
![1653362612286](https://article.biliimg.com/bfs/article/2625aea5b8feafae5a26272203f1af9d2f68e822.png)

当用户抢购时，就会生成订单并保存到`tb_voucher_order`这张表中，而订单表如果使用数据库自增ID就存在一些问题：

- id的**规律性**太明显
- 受**单表**数据量的**限制**

场景分析：如果我们的id具有太明显的规则，用户或者说商业对手很容易猜测出来我们的一些敏感信息，比如商城在一天时间内，卖出了多少单，这明显不合适。
场景分析二：随着我们商城规模越来越大，mysql的单表的容量不宜超过500W，数据量过大之后，我们要进行拆库拆表，但拆分表了之后，他们从逻辑上讲他们是同一张表，所以他们的**id是不能一样的**， 于是乎我们需要保证id的唯一性。

**全局ID生成器**，是一种在<font color=#ff0000>分布式系统</font>下用来生成全局唯一ID的工具，一般要满足下列特性：


<img src="https://article.biliimg.com/bfs/article/c6c3002d54753ba8c0e6a04ac5f8708faf06226f.png" alt="1653363100502" style="zoom:50%;" />

1. 唯一性 
2. 高可用 
3. 高性能 
4. 递增性 
5. 安全性 

采用类似[[MyBatis-plus#雪花算法|雪花算法]]的原理来实现

<img src="https://article.biliimg.com/bfs/article/701adaeb126d020d53948721afd9765b01403578.png" style="zoom:70%;" />

- ID的组成部分：符号位：1bit，永远为0
- 时间戳：31bit，以秒为单位，可以使用69年
- 序列号：32bit，秒内的计数器，支持每秒产生2^32个不同ID

### Redis实现全局唯一Id

核心思路:
1. 实现时间戳的方式是定义一个开始时间戳,用现在的时间减去开始时间戳,两个时间戳都用秒单位.
2. 这里的序列采用是redis的自增操作
3. 序列号自增的key拼接了当前日期,防止单个key的自增数量超过$2^{32}$,还可以统计每天的订单量.
4. 这里的拼接采用移位加或运算
5. 测试一下全局id，利用线程池并发运行


### 添加优惠卷

每个店铺都可以发布优惠券，分为平价券和特价券。平价券可以任意购买，而特价券需要秒杀抢购：

`tb_voucher`：优惠券的基本信息，优惠金额、使用规则等
`tb_seckill_voucher`：优惠券的库存、开始抢购时间，结束抢购时间。**特价**优惠券才需要填写这些信息

平价卷由于优惠力度并不是很大，所以是可以任意领取
而代金券由于优惠力度大，所以像第二种卷，就得限制数量，从表结构上也能看出，特价卷除了具有优惠卷的基本信息以外，还具有库存，抢购时间，结束时间等等字段

<img src="https://article.biliimg.com/bfs/article/86a8b478fe1cf4c496f1fc959a147f8fbe9b39c2.png" alt="1653365145124" style="zoom:50%;" />
### 实现秒杀下单

下单核心思路：当我们点击抢购时，会触发右侧的请求，我们只需要编写对应的controller即可

<img src="https://article.biliimg.com/bfs/article/8a1a4630040f970c941787b5d4883dec308bd3ff.png" alt="1653365839526" style="zoom:50%;" />
秒杀下单应该思考的内容：
下单时需要判断两点：
* 秒杀**是否开始或结束**，如果尚未开始或已经结束则无法下单
* **库存**是否充足，不足则无法下单

下单核心逻辑分析：
当用户开始进行下单，我们应当去查询优惠卷信息，查询到优惠卷信息，判断是否满足秒杀条件
比如时间是否充足，如果时间充足，则进一步判断库存是否足够，如果两者都满足，则扣减库存，创建订单，然后返回订单id，如果有一个条件不满足则直接结束。

<img src="https://article.biliimg.com/bfs/article/618942ef6cca0ee5bce187e5524b73ec5a3a7eb2.png" alt="1653366238564" style="zoom:60%;" />


核心流程：
1.  这里的订单id是用到了之前的id生成器
2. 注意加事务

### 库存超卖问题分析

```java
 if (voucher.getStock() < 1) {
        // 库存不足
        return Result.fail("库存不足！");
    }
    //5，扣减库存
    boolean success = seckillVoucherService.update()
            .setSql("stock= stock -1")
            .eq("voucher_id", voucherId).update();
    if (!success) {
        //扣减库存
        return Result.fail("库存不足！");
    }
```

假设线程1过来查询库存，判断出来库存大于1，正准备去扣减库存，但是还没有来得及去扣减，此时线程2过来，线程2也去查询库存，发现这个数量一定也大于1，那么这两个线程都会去扣减库存，最终多个线程相当于一起去扣减库存，此时就会出现库存的超卖问题。

<img src="https://article.biliimg.com/bfs/article/4bc640899588fcab36904a05ada9dfd2882ce3f5.png"  style="zoom:50%;" />

超卖问题是典型的多线程安全问题，针对这一问题的常见解决方案就是加锁：而对于加锁，我们通常有两种解决方案：见下图：
[[MyBatis-plus#乐观锁|乐观锁]]

<img src="https://article.biliimg.com/bfs/article/253e1f38bac8ee80a8bf3a417f2dd14604af7468.png" style="zoom:60%;" />

### 优惠券秒杀-一人一单

需求：修改秒杀业务，要求同一个优惠券，一个用户只能下一单
**现在的问题在于：**
优惠卷是为了引流，但是目前的情况是，一个人可以无限制的抢这个优惠卷，所以我们应当增加一层逻辑，让一个用户只能下一个单，而不是让一个用户下多个单
具体操作逻辑如下：比如时间是否充足，如果时间充足，则进一步判断库存是否足够，然后再根据优惠卷id和用户id查询是否已经下过这个订单，如果下过这个订单，则不再下单，否则进行下单

<img src="https://article.biliimg.com/bfs/article/3fb75810da71cb15a6f70494e4464e9d54fcbbaa.png" style="zoom:50%;" />

核心思路：
1. 从根据优惠券id和用户id查询订单的操作到返回订单id的这段的操作必须加锁
2. 方式用抽取方法加`synchronized`关键字
3. 注意这里是同一用户实施锁，不同用户它的锁不一致，这里要用[[java基础篇#线程安全|同步代码块]]
4. 这里要使用`intern`方法保证是对相同的值进行加锁而不是对象
5. 这里是如果先释放锁后提交事务，那么方法的所有的行为都是在提交事务后才执行的，我们要做的是提交完再释放，所以把锁加在`@Transactional` 外面
6. [[spring#Spring同一个类中的内部方法调用事务失效|代理失效]]问题



### 集群环境下的并发问题

通过加锁可以解决在单机情况下的一人一单安全问题，但是在集群模式下就不行了。
1、我们将服务启动两份，端口分别为8081和8082：

<img src="https://article.biliimg.com/bfs/article/fea63663979a92e1aaa7f3fa661fc893e3ca3664.png" style="zoom:50%;" />
2、然后修改nginx的conf目录下的nginx.conf文件，配置[[nginx#反向代理|反向代理]]和[[nginx#负载均衡|负载均衡]]：

<img src="https://article.biliimg.com/bfs/article/26d8b5bd5c17a300cb0712420d20d120544cd3c9.png" alt="1653373908620" style="zoom:50%;" />

**具体操作(略)**
**有关锁失效原因分析**
由于现在我们部署了多个tomcat，每个tomcat都有一个属于自己的jvm，那么假设在服务器A的tomcat内部，有两个线程，这两个线程由于使用的是同一份代码，那么他们的锁对象是同一个，是可以实现互斥的，但是如果现在是服务器B的tomcat内部，又有两个线程，但是他们的锁对象写的虽然和服务器A一样，但是锁对象却不是同一个，所以线程3和线程4可以实现互斥，但是却无法和线程1和线程2实现互斥，这就是**集群环境**下，syn锁失效的原因，在这种情况下，我们就需要使用分布式锁来解决这个问题。

<img src="https://article.biliimg.com/bfs/article/f79c49fbd421042f2cc0f1a092a8d06962b1777d.png" style="zoom:50%;" />


## 分布式锁

### 基本原理和实现方式对比

分布式锁：满足**分布式系统**或**集群模式**下多进程可见并且互斥的锁。
分布式锁的核心思想就是让大家都**使用同一把锁**，只要大家使用的是同一把锁，那么我们就能锁住线程，不让线程进行，让程序串行执行，这就是分布式锁的核心思路

<img src="https://article.biliimg.com/bfs/article/1c00db0e3e3eaeb916314e90085213a29a9d9ec7.png" style="zoom:50%;" />

那么分布式锁他应该满足一些什么样的条件呢？
> [!summary]+
> - **可见性**：多个线程都能看到相同的结果，注意：这个地方说的可见性并不是并发编程中指的内存可见性，只是说多个进程之间都能**感知到变化**的意思
> - **互斥**：互斥是分布式锁的最基本的条件，使得程序串行执行
> - **高可用**：程序不易崩溃，时时刻刻都保证较高的可用性
> - **高性能**：由于加锁本身就让性能降低，所有对于分布式锁本身需要他就较高的加锁性能和释放锁性能
> - **安全性**：安全也是程序中必不可少的一环

<img src="https://article.biliimg.com/bfs/article/7926c6d15ee60ecdb0a5bbb8a7396b5a5d6fb67f.png" style="zoom:50%;" />

常见的分布式锁有三种
- Mysql：mysql本身就带有锁机制，但是由于mysql**性能本身一般**，所以采用分布式锁的情况下，其实使用mysql作为分布式锁比较少见
- Redis：redis作为分布式锁是非常常见的一种使用方式，现在企业级开发中基本都使用redis或者zookeeper作为分布式锁，利用`setnx`这个方法，如果插入key成功，则表示获得到了锁，如果有人插入成功，其他人插入失败则表示无法获得到锁，利用这套逻辑来实现分布式锁
- Zookeeper：zookeeper也是企业级开发中较好的一个实现分布式锁的方案，由于本套视频并不讲解zookeeper的原理和分布式锁的实现，所以不过多阐述


|      |**MySQL**|**Redis**|**Zookeeper**|
|:-:|:-:|:-:|:-:|
|互斥|利用MySQL本身的互斥锁机制| 利用setnx这样的互斥命令 | 利用节点的唯一性和有序性实现互斥 |
| 高可用 | 好                           | 好                   | 好                     |
| 高性能 | 一般                         | 好                   | 一般                   |
| 安全性 |断开连接，自动释放锁| 利用锁超时时间，到期释放   | 临时节点，断开连接自动释放   |


为什么redis服务器宕机了，就算释放了。其它线程也获取不到锁啊

当Redis服务器宕机时，所有存储在内存中的数据都会丢失。因此，如果一个线程在Redis服务器宕机之前获取了锁，并且持有锁的时间超过了Redis默认的超时时间（默认为10秒），那么即使Redis服务器重新启动后，其他线程也无法获取到这个锁。

这是因为Redis中实现分布式锁的常用方法是通过设置一个带有超时时间的键值对来实现的。当一个线程获取到锁时，会在Redis中设置一个键值对，并指定一个超时时间。其他线程在获取锁之前会先检查这个键是否存在，如果存在则表示该锁已被其他线程持有。但是由于Redis服务器宕机后所有数据都丢失，所以之前设置的键值对也会被删除，导致其他线程无法正确判断是否已经有线程持有该锁。

为解决这个问题，可以使用一种叫做"Redlock"的算法来实现分布式锁。该算法通过将任务复制到多个独立运行的Redis节点上，并使用大多数原则来判断是否成功获取到锁。即使其中某些节点宕机，只要仍然满足大多数原则，则仍然可以保证获取到正确的锁。这种方式可以提高分布式系统的可靠性和容错性。


### Redis分布式锁的实现核心思路

实现分布式锁时需要实现的两个基本方法：
- 获取锁：
    - 互斥：确保只能有一个线程获取锁
    - 非阻塞：尝试一次，成功返回true，失败返回false
- 释放锁：
    - 手动释放
    - 超时释放：获取锁时添加一个超时时间

核心思路：
我们利用`redis` 的`setNx`方法，当有多个线程进入时，我们就利用该方法，第一个线程进入时，redis 中就有这个key 了，返回了1，如果结果是1，则表示他抢到了锁，那么他去执行业务，然后再删除锁，退出锁逻辑，没有抢到锁的哥们，等待一定时间后重试即可


<img src="https://article.biliimg.com/bfs/article/5cac0ebdd153dafecb8aa46965c6ad81593edbe5.png" alt="image.png" style="zoom:50%;" />


### 实现分布式锁版本一
* 加锁逻辑

**锁的基本接口**

```java
public interface ILock {
  /**
  * 尝试获取锁
  * @param timeoutSec 锁持有的超时时间，过期后自动释放
  * @return true 代表获取锁成功；false代表获取锁失败
  **/
  boolean tryLock(long timeoutSec);

  /*
  * 释放锁
  * */

  void unlock();
}
```

核心步骤：
1. 创建`SimpleRedisLock`实现类实现`ILock`  
2. 尝试获取锁,这里的key要结合前缀和业务名称,而value表明是哪一个线程
3. 替换原来的同步块位自制锁
4. 成功就释放锁

### Redis分布式锁误删情况说明
逻辑说明：
持有锁的线程在锁的内部出现了阻塞，导致他的锁自动释放，这时其他线程，线程2来尝试获得锁，就拿到了这把锁，然后线程2在持有锁执行过程中，线程1反应过来，继续执行，而线程1执行过程中，走到了删除锁逻辑，此时就会把本应该属于线程2的锁进行删除，这就是误删别人锁的情况说明
解决方案：解决方案就是在每个线程释放锁的时候，去判断一下当前这把锁是否属于自己，如果属于自己，则不进行锁的删除，假设还是上边的情况，线程1卡顿，锁自动释放，线程2进入到锁的内部执行逻辑，此时线程1反应过来，然后删除锁，但是线程1，一看当前这把锁不是属于自己，于是不进行删除锁逻辑，当线程2走到删除锁逻辑时，如果没有卡过自动释放锁的时间点，则判断当前这把锁是属于自己的，于是删除这把锁。

<img src="https://article.biliimg.com/bfs/article/d27a68e025618baaf1af48fec497d61ac6cfc140.png" style="zoom:50%;" />



### 解决Redis分布式锁误删问题

需求：修改之前的分布式锁实现，满足：在获取锁时存入**线程标示**（可以用UUID表示,因为集群模式下`Thread.currentThread().getName()` 的名称可能会重复）
在释放锁时先获取锁中的线程标示，判断是否与当前线程标示一致
* 如果一致则释放锁
* 如果不一致则不释放锁
核心逻辑：在存入锁时，放入自己线程的标识，在删除锁时，判断当前这把锁的标识是不是自己存入的，如果是，则进行删除，如果不是，则不进行删除。

<img src="https://article.biliimg.com/bfs/article/1f841ea0db486c7abed643a9f94ccce8a0c1f465.png"  style="zoom:70%;" />

核心步骤：
1. 这里的**线程标示**我们用UUID加线程id拼接



### 分布式锁的原子性问题

更为极端的误删逻辑说明：

线程1现在持有锁之后，在执行业务逻辑过程中，他正准备删除锁，而且已经走到了条件判断的过程中，比如他已经拿到了当前这把锁确实是属于他自己的，正准备删除锁，但是此时他的锁到期了，那么此时线程2进来，但是线程1他会接着往后执行，当他卡顿结束后，他直接就会执行删除锁那行代码，相当于条件判断并没有起到作用，这就是删锁时的原子性问题，之所以有这个问题，是因为**线程1的拿锁，比锁，删锁，实际上并不是原子性的**，我们要防止刚才的情况发生，

<img src="https://article.biliimg.com/bfs/article/26f8cd199100075e60429c5a939c1f383d2c6bc3.png" style="zoom:50%;" />

### Lua脚本解决多条命令原子性问题

Redis提供了Lua脚本功能，在一个脚本中编写多条Redis命令，确保多条命令执行时的原子性。Lua是一种编程语言，它的基本语法大家可以参考网站：[https://www.runoob.com/lua/lua-tutorial.html](https://www.runoob.com/lua/lua-tutorial.html)，
这里重点介绍Redis提供的调用函数，我们可以使用lua去操作redis，又能保证他的原子性，这样就可以实现拿锁比锁删锁是一个原子性动作了，作为Java程序员这一块并不作一个简单要求，并不需要大家过于精通，只需要知道他有什么作用即可。


这里重点介绍Redis提供的调用函数，语法如下：

```lua
redis.call('命令名称', 'key', '其它参数', ...)
```

例如，我们要执行`set name jack`，则脚本是这样：

```lua
# 执行 set name jack
redis.call('set', 'name', 'jack')
```

例如，我们要先执行`set name Rose`，再执行`get name`，则脚本如下：

```lua
# 先执行 set name jack
redis.call('set', 'name', 'Rose')
# 再执行 get name
local name = redis.call('get', 'name')
# 返回
return name
```

写好脚本以后，需要用Redis命令来调用脚本，调用脚本的常见命令如下：

<img src="https://article.biliimg.com/bfs/article/8e8311b2810700639b72089bfcd57e68dfa19be6.png" style="zoom:50%;" />
例如，我们要执行 `redis.call('set', 'name', 'jack')` 这个脚本，语法如下：

<img src="https://article.biliimg.com/bfs/article/68cb679010e65705a4416b72ba387151bb3ffd76.png" alt="1653392218531" style="zoom:50%;" />
如果脚本中的key、value不想写死，可以作为参数传递。key类型参数会放入KEYS数组，其它参数会放入ARGV数组，在脚本中可以从KEYS和ARGV数组获取这些参数：

<img src="https://article.biliimg.com/bfs/article/dfd7cb3fea5f813dc1b3fee9b3a96b5b02d78019.png" alt="1653392438917" style="zoom:50%;" />

接下来我们来回一下我们释放锁的逻辑：
释放锁的业务流程是这样的
1. ​获取锁中的线程标示
2. ​判断是否与指定的标示（当前线程标示）一致
3. ​如果一致则释放锁（删除）
4. ​如果不一致则什么都不做

如果用Lua脚本来表示则是这样的：
最终我们操作redis的拿锁比锁删锁的lua脚本就会变成这样
```lua
-- 这里的 KEYS[1] 就是锁的key，这里的ARGV[1] 就是当前线程标示
-- 获取锁中的标示，判断是否与当前线程标示一致
if (redis.call('GET', KEYS[1]) == ARGV[1]) then
  -- 一致，则删除锁
  return redis.call('DEL', KEYS[1])
end
-- 不一致，则直接返回
return 0
```

### 利用Java代码调用Lua脚本改造分布式锁

lua脚本本身并不需要大家花费太多时间去研究，只需要知道如何调用，大致是什么意思即可，所以在笔记中并不会详细的去解释这些lua表达式的含义。
我们的`RedisTemplate`中，可以利用`execute`方法去执行lua脚本，参数对应关系就如下图股

<img src="https://article.biliimg.com/bfs/article/60681654ec06b658df1129478bebe27ffded29bd.png" style="zoom:70%;" />


核心步骤：
1. 将解锁的方法用换成lua脚本执行
2. 提前写好一个lua脚本，提前加载好这个脚本,路径利用[[maven#classpath类路径|ClassPath]]
3. Rredis去执行这个脚本


## 分布式锁-redission

### 分布式锁-redission功能介绍

基于setnx实现的分布式锁存在下面的问题：


<img src="https://article.biliimg.com/bfs/article/cb22c1d74872ae8654394e56b5fa142de71acee1.png" style="zoom:50%;" />

> [!summary]
> - **重入问题**：可重入就是多个线程**可以获得同一把锁**，比如有一个方法a，调用方法a需要获取锁1，在a里面执行业务调用方法b,而b里面又要获取锁1(同一把锁)，不可重入就会导致无法获取锁，造成死锁问题
> - **不可重试**：是指目前的分布式只能尝试一次，我们认为合理的情况是：当线程在获得锁失败后，他应该能**再次尝试**获得锁。
> - **超时释放：** 我们在加锁时增加了过期时间，这样的我们可以防止死锁，但是如果卡顿的时间超长，虽然我们采用了lua表达式防止删锁的时候，误删别人的锁，但是毕竟没有锁住，有安全隐患，一个线程在执行业务，其他线程也来了
> - **主从一致性：** Redis有一个**主节点**和多个**从节点**，==当执行写操作，访问的是主节点，执行读操作，访问的是从节点==。主节点要把数据同步给从节点，保证主从一致，主宕机了，可以从从节点选出一个当主节点。但是主从节点的同步是有延迟的，如果一个线程在主节点获取了锁，是个set也就是写操作，还没同步给从节点，就宕机了，从节点选出一个当主节点，但是这个从节点没有锁，其他线程就可以拿到锁，会造成安全问题
。



**那么什么是Redission呢**
**Redisson**是一个在Redis的基础上实现的Java**驻内存数据网格**（In-Memory Data Grid）。它不仅提供了一系列的**分布式的Java常用对象**，还提供了许多分布式服务，其中就包含了各种分布式锁的实现。
Redission提供了分布式锁的多种多样的功能

<img src="https://article.biliimg.com/bfs/article/fc2872c0e3eb7c036e51d242f9d360f670aac73a.png" style="zoom:50%;" />

官网地址：[Fetching Title#37ow](https://redisson.org)
GitHub地址：[GitHub - redisson/redisson:](https://github.com/redisson/redisson)

### 分布式锁-Redission快速入门

引入依赖：

```xml
<dependency>
	<groupId>org.redisson</groupId>
	<artifactId>redisson</artifactId>
	<version>3.13.6</version>
</dependency>
```


**配置Redisson客户端**：

```java
@Configuration
public class RedisConfig {

  @Bean
  public RedissonClient redissonClient()
  {
    //配置
    Config config=new Config();
    config.useSingleServer().setAddress("redis://192.168.153.128:6379").setPassword("701121");
    // 创建RedissonClient对象
    return Redisson.create(config);
  }
}
```

如何使用Redission的分布式锁
```java
@Resource
private RedissionClient redissonClient;

@Test
void testRedisson() throws Exception{
    //获取锁(可重入)，指定锁的名称
    RLock lock = redissonClient.getLock("anyLock");
    //尝试获取锁，参数分别是：获取锁的最大等待时间(期间会重试)，锁自动释放时间，时间单位
    boolean isLock = lock.tryLock(1,10,TimeUnit.SECONDS);
    //判断获取锁成功
    if(isLock){
        try{
            System.out.println("执行业务");          
        }finally{
            //释放锁
            lock.unlock();
        }
    }   
}
```

### 分布式锁-redission可重入锁  原理

在Lock锁中，他是借助于底层的一个**voaltile**的一个state变量来记录重入的状态的，比如当前没有人持有这把锁，那么state=0，假如有人持有这把锁，那么state=1，如果持有这把锁的人再次持有这把锁，那么state就会+1 ，如果是对于synchronized而言，他在c语言代码中会有一个count，原理和state类似，也是重入一次就加一，释放一次就-1 ，直到减少成0 时，表示当前这把锁没有被人持有。

在redission中，我们的也支持支持可重入锁
在分布式锁中，他采用`hash`结构用来存储锁，其中大key表示表示这把锁是否存在，用小key表示当前这把锁被哪个线程持有，所以接下来我们一起分析一下当前的这个lua表达式

这个地方一共有3个参数
- **KEYS[1] ： 锁名称**
- **ARGV[1]： 锁失效时间**
- **ARGV[2]： id + ":" + threadId; 锁的小key**


![image.png](https://article.biliimg.com/bfs/article/21406abcb02ad9f0e2a000ebb09b55e59bb71d94.png)


**获取锁的Lua脚本**： 
```lua
local key=KEYS[1];  -- 锁的key
local threadId=ARGV[1]
local releaseTime=ARGV[2]

-- 判断锁是否存在
if redis.call("exists",key)==0 then
  -- 不存在 获取锁
  redis.call('hset',key,threadId,1);
  -- 设置有效期
  redis.call('expire',key,releaseTime);
  return 1;
end;

-- 锁已经存在,判断threadId是否是自己
if redis.call('hexists',key,threadId)==1 then
  -- 锁是自己的，锁计数+1
  redis.call("hincrby",key,threadId,'1');
  -- 重新设置有效期
  redis.call('expire',key,releaseTime);
  return 1;
end;
return 0;
-- 锁不是自己的,获取失败
```

**释放锁的Lua脚本:** 


```lua
local key = KEYS[1]; -- 锁的key
local threadId = ARGV[1]; -- 线程唯一标识
local releaseTime = ARGV[2]; -- 锁的自动释放时间
-- 判断当前锁是否还是被自己持有
if (redis.call('HEXISTS', key, threadId) == 0) then
  return nil; -- 如果已经不是自己，则直接返回
end;
-- 是自己的锁，则重入次数-1
local count = redis.call('HINCRBY', key, threadId, -1);
-- 判断是否重入次数是否已经为0 
if (count > 0) then
  -- 大于0说明不能释放锁，重置有效期然后返回
  redis.call('EXPIRE', key, releaseTime);
  return nil;
else  -- 等于0说明可以释放锁，直接删除
  redis.call('DEL', key);
return nil;
end;
```

### 分布式锁-redission锁重试和WatchDog机制

**说明**：由于课程中已经说明了有关tryLock的源码解析以及其看门狗原理，所以笔者在这里给大家分析lock()方法的源码解析，希望大家在学习过程中，能够掌握更多的知识
抢锁过程中，获得当前线程，通过tryAcquire进行抢锁，该抢锁逻辑和之前逻辑相同
1. 先判断当前这把锁是否存在，如果不存在，插入一把锁，返回null
2. 判断当前这把锁是否是属于当前线程，如果是，则返回null
所以如果返回是null，则代表着当前这哥们已经抢锁完毕，或者可重入完毕，但是如果以上两个条件都不满足，则进入到第三个条件，返回的是**锁的失效时间**，同学们可以自行往下翻一点点，你能发现有个while( true) 再次进行tryAcquire进行抢锁

解决锁的重试的方法：
- 这里获取锁成功返回了nil，也就是null
- 失败了返回==这个锁的剩余有效期==
- 这里的等待重试采用的是**消息订阅**和**信号量机制**方法

解决超时释放的方法：
- 获取锁成功后，进入过期时间续约的方法
- 也就是每10秒续约一次


**Redisson分布式锁原理**

<img src="https://article.biliimg.com/bfs/article/d74f96c29a8feb55c6b5bf38bf99443e1e7a6e0f.png" alt="image.png" style="zoom:50%;" />

**相关博客**：
[分布式锁-Redisson的使用及源码分析](https://blog.csdn.net/qq_41807885/article/details/128054308?spm=1001.2014.3001.5502)
[分布式锁 - Redisson的看门狗机制](https://blog.csdn.net/qq_41807885/article/details/128407627?spm=1001.2014.3001.5502)


### 分布式锁-redission锁的MutiLock原理

为了提高redis的可用性，我们会搭建**集群**或者**主从**，现在以主从为例

此时我们去写命令，写在主机上， 主机会将数据同步给从机，但是假设在主机还没有来得及把数据写入到从机去的时候，此时主机宕机，哨兵会发现主机宕机，并且选举一个slave变成master，而此时新的master中实际上并**没有锁信息**，此时锁信息就已经丢掉了。

<img src="https://article.biliimg.com/bfs/article/461f1bb6f9c50f18550c7a4c09d8c8dfccfd7eaf.png" alt="1653553998403" style="zoom:50%;" />

为了解决这个问题，redission提出来了**MutiLock锁**，就是**不再设置主从概念了**,使用这把锁咱们就不使用主从了，**每个节点的地位都是一样的**， 这把锁加锁的逻辑需要写入到每一个主丛节点上，只有所有的服务器都写入成功，此时才是加锁成功，假设现在某个节点挂了，那么他去获得锁的时候，**只要有一个节点拿不到，都不能算是加锁成功**，就保证了加锁的可靠性。

<img src="https://article.biliimg.com/bfs/article/2e4695e8a8d62021c3e040ae0209a1b3e1dc8a8a.png" alt="1653554055048" style="zoom:50%;" />

那么MutiLock 加锁原理是什么呢？笔者画了一幅图来说明

当我们去设置了多个锁时，redission会将多个锁**添加到一个集合中**，然后用while循环去不停去尝试拿锁，但是会有一个**总共的加锁时间**，这个时间是用需要加锁的个数 * 1500ms ，假设有3个锁，那么时间就是4500ms，假设在这4500ms内，所有的锁都加锁成功， 那么此时才算是加锁成功，如果在4500ms有线程加锁失败，则会再次去进行重试.

<img src="https://article.biliimg.com/bfs/article/78edd6dce0e51ab7bb768ea61b43ae834621266b.png" alt="image.png" style="zoom:100%;" />

1. **不可重入Redis分布式锁**： 
	- **原理**：利用`setnx`的互斥性；利用ex避免死锁；释放锁时判断线程标示 
	- **缺陷**：不可重入、无法重试、锁超时失效 
2. **可重入的Redis分布式锁**： 
	- **原理**：利用hash结构，记录线程标示和重入次数；利用watchDog延续锁时间；利用信号量控制锁重试等待 
	- **缺陷**：redis岩机引起锁失效问题 
3. **Redisson的multiLock**: 
	- **原理**：多个独立的Redis节点，必须在所有节点都获取重入锁，才算获取锁成功 
	- **缺陷**：运维成本高、实现复杂 

## 秒杀优化

### 秒杀优化-异步秒杀思路

我们来回顾一下下单流程
当用户发起请求，此时会请求`nginx`，`nginx`会访问到`tomcat`，而`tomcat`中的程序，会进行**串行**操作，分成如下几个步骤

1、查询优惠卷
2、判断秒杀库存是否足够
3、查询订单
4、校验是否是一人一单
5、扣减库存
6、创建订单

在这六步操作中，又有很多操作是要去**操作数据库的**，而且还是一个线程串行执行， 这样就会导致我们的程序执行的很慢，所以我们需要异步程序执行，那么如何加速呢？

在这里笔者想给大家分享一下课程内没有的思路，看看有没有小伙伴这么想，比如，我们可以不可以使用异步编排来做，或者说我开启N多线程，N多个线程，一个线程执行查询优惠卷，一个执行判断扣减库存，一个去创建订单等等，然后再统一做返回，这种做法和课程中有哪种好呢？答案是课程中的好，因为如果你采用我刚说的方式，如果访问的人很多，那么**线程池中的线程可能一下子就被消耗完了**，而且你使用上述方案，最大的特点在于，你觉得时效性会非常重要，但是你想想是吗？并不是，比如我只要确定他能做这件事，然后我后边慢慢做就可以了，我并不需要他一口气做完这件事，所以我们应当采用的是课程中，类似[[零散知识点#消息队列|消息队列]]的方式来完成我们的需求，而不是使用线程池或者是异步编排的方式来完成这个需求


<img src="https://article.biliimg.com/bfs/article/c64c7c4b394ca182ea02c0adefb0df9a0dcf6109.png" alt="1653560986599" style="zoom:50%;" />

**优化方案**：我们将**耗时比较短的逻辑判断**放入到**redis**中，比如是否库存足够，比如是否一人一单，这样的操作，只要这种逻辑可以完成，就意味着我们是一定可以下单完成的，我们只需要进行快速的逻辑判断，根本就不用等下单逻辑走完，我们直接给用户返回成功， 再**在后台开一个线程**，后台线程慢慢的去**执行queue里边的消息**，这样程序不就超级快了吗？而且也不用担心线程池消耗殆尽的问题，因为这里我们的程序中并没有手动使用任何线程池，当然这里边有两个难点

- 第一个难点是我们怎么在redis中去快速校验一人一单，还有库存判断
- 第二个难点是由于我们校验和tomct下单是两个线程，那么我们如何知道到底哪个单他最后是否成功，或者是下单完成，为了完成这件事我们在redis操作完之后，我们会**将一些信息返回给前端**，同时也会把这些信息丢到异步queue中去，后续操作中，可以通过这个id来查询我们tomcat中的下单逻辑是否完成了。

<img src="https://article.biliimg.com/bfs/article/b7f96c13760814f4390f9d98865de099611145f8.png" style="zoom:50%;" />

我们现在来看看整体思路：
- 当用户下单之后，判断库存是否充足只需要导redis中去根据key找对应的value是否大于0即可，如果不充足，则直接结束，如果充足，继续在redis中判断用户是否可以下单，
- 如果set集合中没有这条数据，说明他可以下单，如果set集合中没有这条记录，则将userId和优惠卷存入到redis中，并且返回0，整个过程需要保证是原子性的，我们可以使用lua来操作
- 当以上判断逻辑走完之后，我们可以判断当前redis中返回的结果是否是0 ，如果是0，则表示可以下单，则将之前说的信息存入到到queue中去，然后返回，然后再来个线程异步的下单，前端可以通过返回的订单id来判断是否下单成功。

<img src="https://article.biliimg.com/bfs/article/94ad13aa9876ebfe5bef6f70652ff842f34b92aa.png" style="zoom:50%;" />

### 秒杀优化-Redis完成秒杀资格判断

需求：
- 新增秒杀优惠券的同时，将优惠券信息保存到**Redis**中，key为(seckill:stock:)
- 基于Lua脚本，判断秒杀库存、一人一单，决定用户是否抢购成功
	- 注意在redis所以`value`的值都是基于字符串，没有int,所以逻辑判断要转换一下
- 如果抢购成功，将优惠券id和用户id封装后存入**阻塞队列**(这里类似于消息队列)
	- 这里的阻塞队列采用的是java中的`BlockingQueues`，在原有的queue的基础上，加了额外的阻塞操作
- 开启线程任务，不断从阻塞队列中获取信息，实现异步下单功能
	- 这里要采用[[java基础篇#线程池|单线程池]]
	- 这里的线程任务采用了[[java基础篇#内部类|匿名内部类]],这些任务在服务启动前就要执行，所以要用[[spring#@PostConstruct|PostConstruct]]注解
	- 任务要不断从队列中取信息
	- 创建订单单独设置一个`handleVoucherOrder`方法,这里线程池的线程无法获取主线程的ThreadLocal的值

<img src="https://article.biliimg.com/bfs/article/06261263806e1d3b58491a3df076d26ba2eb7af4.png" alt="image.png" style="zoom:50%;" />

秒杀业务的优化思路是什么？
- 先利用Redis完成库存余量、一人一单判断，完成抢单业务
- 再将下单业务放入阻塞队列，利用独立线程异步下单
- 基于阻塞队列的异步秒杀存在哪些问题？
    - 内存限制问题
    - 数据安全问题

## Redis消息队列

###  Redis消息队列-认识消息队列

什么是消息队列：字面意思就是存放消息的队列。最简单的消息队列模型包括3个角色：

* **消息队列**：存储和管理消息，也被称为消息代理（Message Broker）
* **生产者**：发送消息到消息队列
* **消费者**：从消息队列获取消息并处理消息

消息队列跟阻塞队列的区别：
1. 消息队列是一种独立的服务，不受jvm的内存限制
2. jvm没有持久化操作，宕机后，会丢失阻塞队列中的所有数据
3. 消息队列不仅仅做数据存储，还要保证数据安全，有[[Mysql#事务四大特征|持久化]]

<img src="https://article.biliimg.com/bfs/article/efd3acf496e945183f5f4eb155f6218347187ebb.png" alt="1653574849336" style="zoom:67%;" />
使用队列的好处在于 **解耦：** 所谓解耦，当两个或多个应用程序需要进行通信时，使用消息队列可以实现解耦效果，从而提高应用程序的灵活性和可扩展性。
例如，假设有一个电子商务平台和一个库存管理系统，需要进行库存变更的同步。传统的实现方式是直接调用库存管理系统的接口，但是这样会将平台和库存管理系统紧密地耦合在一起。
通过使用消息队列，可以改善这种情况。电商平台在发生库存变更的时候，**不直接调用库存管理系统的接口**，而是将库存变更消息发布到消息队列中。库存管理系统通过订阅消息队列，接收并处理库存变更消息。这样，库存管理系统可以独立于电商平台进行升级、扩展或替换，而不会影响到电商平台的正常运行。

### Redis消息队列-基于List实现消息队列

**基于List结构模拟消息队列**

消息队列（Message Queue），字面意思就是存放消息的队列。而Redis的list数据结构是一个双向链表，很容易模拟出队列效果。
队列是入口和出口不在一边，因此我们可以利用：`LPUSH` 结合 `RPOP`、或者 `RPUSH` 结合 `LPOP`来实现。 不过要注意的是，当队列中没有消息时`RPOP`或`LPOP`操作会返回null，并不像JVM的阻塞队列那样会阻塞并等待消息。因此这里应该使用`BRPOP`或者`BLPOP`来实现阻塞效果。

<img src="https://article.biliimg.com/bfs/article/83ba024dd2b4dca30734cea7e3190bc9b8cac0ac.png" alt="1653575176451" style="zoom:50%;" />


基于List的消息队列有哪些优缺点？
优点：
* 利用Redis存储，不受限于JVM内存上限
* 基于Redis的持久化机制，数据安全性有保证
* 可以满足消息有序性
缺点：

* 无法避免消息丢失
	* 假如我从redis队列中取到一个数据,还没处理就挂掉了或者异常了,因为是remove,其他线程就拿不到
* 只支持单消费者

### Redis消息队列-基于PubSub的消息队列

**PubSub**（发布订阅）是Redis2.0版本引入的消息传递模型。顾名思义，消费者可以**订阅一个或多个channel**，生产者向对应channel发送消息后，所有订阅者都能收到相关消息。

- `SUBSCRIBE channel [channel]` ：订阅一个或多个频道
- `PUBLISH channel msg` ：向一个频道发送消息
- `PSUBSCRIBE pattern[pattern]` ：订阅与pattern格式匹配的所有频道
	- Supported glob-style patterns:
	- `h?llo` subscribes to `hello`, `hallo` and `hxllo`
	- `h*llo` subscribes to `hllo` and `heeeello`
	- `h[ae]llo` subscribes to `hello` and `hallo,` but not `hillo`

<img src="https://article.biliimg.com/bfs/article/78f02f5cc44b83e8863343f4d5f2627887d939ba.png" style="zoom:50%;" />

基于PubSub的消息队列有哪些优缺点？
优点：
* 采用发布订阅模型，支持多生产、多消费
缺点：
* 不支持数据持久化
	* 因为如果消息没有被订阅,那就丢失了,不会在redis中保存
* 无法避免消息丢失
* 消息堆积有上限，超出时数据丢失
	* 这里的订阅在**消费者缓存**哪里,而这个缓存时**有上限的**

### Redis消息队列-基于Stream的消息队列

Stream 是 Redis 5.0 引入的一种新数据类型，可以实现一个功能非常完善的消息队列。
发送消息的命令：
<img src="https://article.biliimg.com/bfs/article/5a259a9ade7f40eed55a5b586cc9fcdb8100ae66.png" style="zoom:50%;" />
例如：

```r
## 创建名为users的队列，并向其中发送一个消息，内容是：{name=jack,age=21},并且使用Redis自动生成ID 
127.0.0.1:6379>XADD users * name jack age 21 
"1644805700523-0" 
```

读取消息的方式之一：XREAD

<img src="https://article.biliimg.com/bfs/article/525395d50d62585553f06bc77b868484975d039e.png" style="zoom:50%;" />


例如，使用XREAD读取第一个消息：

<img src="https://article.biliimg.com/bfs/article/dd967e618e1da173c2ba315e674d8907068f8b1d.png" style="zoom:50%;" />

XREAD阻塞方式，读取最新的消息：

<img src="https://article.biliimg.com/bfs/article/63fd54afc0ca478f32571fc6e7355e028331c39a.png" alt="1653577659166" style="zoom:50%;" />

```java
while(true){ 
//尝试读取队列中的消息，最多阻塞2秒 
Object msg = redis.execute ("XREAD COUNT 1 BLOCK 2000 STREAMS users $"); 
if (msg == null) { 
	continue; 
} 
//处理消息 
handleMessage(msg); 
```

注意：当我们指定起始ID为`$`时，代表读取最新的消息，如果我们处理一条消息的过程中，又有超过**1条以上的消息到达队列**，则下次获取时也只能获取到最新的一条，会出现漏读消息的问题

**删除消息队列的消息**：
```r
XDEL key id [id ...]
```

这个命令是 Redis Stream 数据结构的命令之一，用于删除指定消息的 ID。

- `XDEL` 是删除 Redis Stream 中的消息的命令。
- `key` 是指定要操作的 Stream 的键名。
- `id` 是要删除的消息的 ID。可以指定一个或多个 ID 进行删除。

该命令用于从指定 Stream 中删除一个或多个消息。当执行成功时，被删除消息对应的 ID 将不再在 Stream 中存在。



STREAM类型消息队列的XREAD命令特点：
* 消息可回溯 -消息读完不消息，永久保存在队列中
* 一个消息可以被多个消费者读取
* 可以阻塞读取
* 有消息漏读的风险
### Redis消息队列-基于Stream的消息队列-消费者组

Redis消息队列-基于Stream的消息队列-消费者组

<img src="https://article.biliimg.com/bfs/article/70d0f42e9b1a7b5b54f3deec227545f55ad0f4a1.png" style="zoom:70%;" />

创建消费者组：
```r
XGROUP CREATE key groupName ID [MKSTREAM] 
```

**key**：队列名称
**groupName**：消费者组名称
**ID**：起始ID标示，`$`代表队列中最后一个消息，0则代表队列中第一个消息
**MKSTREAM**：队列不存在时自动创建队列
其它常见命令：

**删除指定的消费者组**

```r
XGROUP DESTORY key groupName
```

**给指定的消费者组添加消费者**

```r
XGROUP CREATECONSUMER key groupname consumername
```

**删除消费者组中的指定消费者**
```r
XGROUP DELCONSUMER key groupname consumername
```

**从消费者组读取消息**：
```r
XREADGROUP GROUP group consumer [COUNT count] [BLOCK milliseconds] [NOACK] STREAMS key [key ...] ID [ID ...]
```

* `group`：消费组名称
* `consumer`：消费者名称，如果消费者不存在，会自动创建一个消费者
* `count`：本次查询的最大数量
* `BLOCK milliseconds`：当没有消息时最长等待时间
* `NOACK`：无需手动ACK，获取到消息后自动确认
* `STREAMS key`：指定队列名称
* `ID`：获取消息的起始ID：
	* `>`：读取这个消费者组中**尚未消费**的消息
		* 这个主要用于正常的处理
	* 其它：根据指定id从pending-list中获取已消费但未确认的消息，例如0，是从pending-list中的第一个消息开始
		* 这个用于获取消息成功发生异常时处理时。

**读取pending-list中的数据**：
```r
XPENDING key group [[IDLE min-idle-time] start end count [consumer]]
```

这个命令的作用是返回指定消费者组中处于**待处理状态**的消息的相关信息。

参数解释：
- `key`：指定的流名称。
- `group`：消费者组的名称。
- `IDLE min-idle-time`：可选参数，限制只返回指定时间内处于空闲状态的消费者组信息。
- `start`、`end`：指定待处理状态消息的范围（闭区间）。
- `count`：返回结果的最大数量。
- `consumer`：可选参数，指定只返回与特定消费者关联的消息信息。

**消息确认**：
```r
XACK key group id [id ...]
```

- `id`:这里的不是不是数字，是消息id，就是之前的`*`产生的
- 这条命令执行后的id会从`pending-list`中移除

**消费者监听消息的基本思路**：

```java
while(true){
//尝试监听队列，使用阻塞队列，最长等待时间2000毫秒
Object msg=redis.call("XREADGROUP GROUP g1 c1 COUNT 1 BLOCK 2000 STREAMS s1 >");
if(msg==null) //说明没有消息，继续下一次
{
	continue;
}
try{
	//处理消息，完成后要ACK
	handleMessage(msg);
}
catch(Excepthon e){ //发生异常
		int i=0;
		while(true)
		{
			Object msg=redis.call("XREADGROUP GROUP g1 c1 COUNT 1 STREAMS s1 0");
			if (msg==null) //null 说明没有异常消息，所有的消息
			{
				break;
			}
			try{
				//说明有异常消息
				handleMessage(msg);
				
			}catch(Exception e)
			{
				if(i==100) log.error("已经出现100次异常了，请手工检测");
				i++;
				//再次出现异常，记录日志，继续循环
			}
		}
	}
}
```

STREAM类型消息队列的`XREADGROUP`命令特点：
- 消息可回溯
- 可以多消费者争抢消息，加快消费速度
- 可以阻塞读取
- 没有消息漏读的风险
- 有消息确认机制，保证消息至少被消费一次 

**Redis消息队列：** 

|  |List |PubSub|Stream|
|:-:|:-:|:-:|:-:|
|消息持久化|支持|不支持|支持|
|阻塞读取|支持|支持|支持|
|消息堆积处理|受限于内存空间，可以利用多消费者加快处理 |受限于消费者缓冲区 |受限于队列长度，可以利用消费者组提高消费速度，减少堆积 |
|消息确认机制|不支持 |不支持 |支持|
|消息回溯|不支持|不支持|支持|
### 基于Redis的Stream结构作为消息队列，实现异步秒杀下单

需求：
- 我们需要替换的是阻塞队列，之前阻塞队列装的是整个订单。
* 创建一个Stream类型的消息队列，名为stream.orders
* 修改之前的秒杀下单Lua脚本，在认定有抢购资格后，直接向stream.orders中添加消息，内容包含voucherId、userId、orderId
* 项目启动时，开启一个线程任务，尝试获取stream.orders中的消息，完成下单

添加秒杀订单是在lua脚本中
这里我们对lua脚本的更改
```lua
redis.call('xadd','stream.orders','*','userId',userId,'voucherId',voucherId,'id',orderId)
```


## 达人探店

### 达人探店-发布探店笔记

发布探店笔记
探店笔记类似点评网站的评价，往往是图文结合。对应的表有两个： tb_blog：探店笔记表，包含笔记中的标题、文字、图片等 tb_blog_comments：其他用户对探店笔记的评价
**具体发布流程**

<img src="https://article.biliimg.com/bfs/article/316f527268d64eb8e6f4c4da2798fe6f5dc6d2d8.png" style="zoom:50%;" />

上传接口
```java
@Slf4j
@RestController
@RequestMapping("upload")
public class UploadController {

    @PostMapping("blog")
    public Result uploadImage(@RequestParam("file") MultipartFile image) {
        try {
            // 获取原始文件名称
            String originalFilename = image.getOriginalFilename();
            // 生成新文件名
            String fileName = createNewFileName(originalFilename);
            // 保存文件
            image.transferTo(new File(SystemConstants.IMAGE_UPLOAD_DIR, fileName));
            // 返回结果
            log.debug("文件上传成功，{}", fileName);
            return Result.ok(fileName);
        } catch (IOException e) {
            throw new RuntimeException("文件上传失败", e);
        }
    }

}
```

注意：同学们在操作时，需要修改SystemConstants.IMAGE_UPLOAD_DIR 自己图片所在的地址，在实际开发中图片一般会放在nginx上或者是云存储上。
BlogController

```java
@RestController
@RequestMapping("/blog")
public class BlogController {

    @Resource
    private IBlogService blogService;

    @PostMapping
    public Result saveBlog(@RequestBody Blog blog) {
        //获取登录用户
        UserDTO user = UserHolder.getUser();
        blog.setUpdateTime(user.getId());
        //保存探店博文
        blogService.saveBlog(blog);
        //返回id
        return Result.ok(blog.getId());
    }
}
```

### 达人探店-查看探店笔记

实现查看发布探店笔记的接口

<img src="https://article.biliimg.com/bfs/article/361e6614d8792bc591798dba55c4a830641cfd47.png" style="zoom:70%;" />

核心步骤：
1. 根据id查询blog
2. 查询blog有关的用户，将blog非数据库的属性补全
3. 返回blog

### 达人探店-点赞功能

初始代码:
```java
@GetMapping("/likes/{id}")
public Result queryBlogLikes(@PathVariable("id") Long id) {
    //修改点赞数量
    blogService.update().setSql("liked = liked +1 ").eq("id",id).update();
    return Result.ok();
}
```

问题分析：这种方式会导致一个用户无限点赞，明显是不合理的
造成这个问题的原因是，我们现在的逻辑，发起请求只是给数据库+1，所以才会出现这个问题

<img src="https://article.biliimg.com/bfs/article/0b5b0d278b9c8739e8979c366b6057463383527c.png" style="zoom:50%;" />
完善点赞功能
需求：
* 同一个用户**只能点赞一次**，再次点击则**取消点赞**
* 如果当前用户已经点赞，则点赞按钮高亮显示（前端已实现，判断字段**Blog类的isLike属性**）


实现步骤：
* 给Blog类中添加一个isLike字段，标示是否被当前用户点赞
* 修改点赞功能，利用Redis的set集合判断是否点赞过，未点赞过则点赞数+1，已点赞过则点赞数-1
* 修改根据id查询Blog的业务，判断当前登录用户是否点赞过，赋值给isLike字段
* 修改分页查询Blog业务，判断当前登录用户是否点赞过，赋值给isLike字段

### 达人探店-点赞排行榜

在探店笔记的详情页面，应该把给该笔记点赞的人显示出来，比如最早点赞的TOP5，形成点赞排行榜：
之前的点赞是放到set集合，但是set集合是不能排序的，所以这个时候，咱们可以采用一个可以排序的set集合，就是咱们的`sortedSet`

<img src="https://article.biliimg.com/bfs/article/081091a63b93b596db8b7f7c755f7c28e79f8291.png" alt="1653805077118" style="zoom:50%;" />


我们接下来来对比一下这些集合的区别是什么?

|  |List| Set           | SortedSet       |
|:-:|:-:|:-:|:-:|
| 排序方式   | 按添加顺序排序  | 无法排序    | 根据score值排序 |
| 唯一性     | 不唯一           | 唯一          | 唯一             |
| 查找方式   | 按索引查找      | 根据元素查找  | 根据元素查找 或首尾查找  |


核心步骤：
1. 这里我们用查询set是否存在用的是`ZSCORE`命令
2. 我们把之前的set逻辑全改成sortedset,分数采用时间戳
3. 写我们的方法,名字是`queryBlogLikes`,查询出top5的用户,注意转类型和判空
4. 这里有个问题sql中基于`in`的   排序是基于id大小，所以要`ORDER BY FIELD`一下

## 好友关注
### 好友关注-关注和取消关注

针对用户的操作：可以对用户进行关注和取消关注功能。

<img src="https://article.biliimg.com/bfs/article/3c5c4036c49a8a12b23d1b2d6a1d2f45a8bdafad.png" alt="1653806140822" style="zoom:50%;" />

实现思路：
需求：基于该表数据结构，实现两个接口：
1. 关注和取关接口(`followUserId/true`就是关注，`followUserId/fals`e就是取关)
2. 判断是否关注的接口
关注是User之间的关系，是博主与粉丝的关系，数据库中有一张tb_follow表来标示：

```sql
CREATE TABLE 'tb_follow'( 
id bigint(20) NOT NULL COMMENT '主键' 
user_id bigint(20) unsigned NOT NULL COMMENT '用户id' 
follow_user_id bigint(20) unsigned NOT NULL COMMENT '关联的用户id' 
create time timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间' 
PRIMARY KEY('id') 
)ENGINE=InnoDB DEFAULT CHARSET=utf8mb4; 
```

核心步骤：
1. 创建这两个接口，名称分别是`follow`和`isFollow` 
2. 实现`follow`接口方法，关注就新增follow对象，取消就删除follow对象
3. 实现`isFollow`接口方法,从数据库，是否存在这个用户和关注关系，返回。

### 好友关注-共同关注

想要去看共同关注的好友，需要首先进入到这个页面，这个页面会发起两个请求
1、去查询用户的详情
2、去查询用户的笔记

<img src="https://article.biliimg.com/bfs/article/91bb7c3a6d9ec597c79d07394474639907456afb.png" alt="1653806706296" style="zoom:50%;" />
接下来我们来看看共同关注如何实现：
需求：利用Redis中恰当的数据结构，实现共同关注功能。在博主个人页面展示出当前用户与博主的共同关注呢。
当然是使用我们之前学习过的`set`集合咯，在set集合中，有交集并集补集的api，我们可以把两人的关注的人分别放入到一个set集合中，然后再通过api去查看这两个set集合中的交集数据。

<img src="https://article.biliimg.com/bfs/article/f8e9c50775a15592179055361811f4d737a91a05.png" alt="1653806973212" style="zoom:50%;" />

核心步骤:
1. 改我们之前的关注接口,关注除了改数据库,还要改redis
2. 请关注的交集，并返回

### 好友关注-Feed流实现方案

当我们关注了用户后，这个用户发了动态，那么我们应该把这些数据推送给用户，这个需求，其实我们又把他叫做 [Feed流系统](https://blog.csdn.net/z_ssyy/article/details/128721959?spm=1001.2014.3001.5502)，关注推送也叫做**Feed流**，直译为**投喂**。为用户持续的提供“沉浸式”的体验，通过无限下拉刷新获取新的信息。

- 对于传统的模式的内容解锁：
	- 我们是需要用户去通过搜索引擎或者是其他的方式去解锁想要看的内容
	
	<img src="https://article.biliimg.com/bfs/article/7f7a9315eeaf526caeb791df75fc4c4cae355549.png" alt="1653808641260" style="zoom:50%;" />

- 对于新型的Feed流的的效果：
	- 不需要我们用户再去推送信息，而是系统分析用户到底想要什么，然后直接把内容**推送给用户**，从而使用户能够更加的节约时间，不用主动去寻找。

	<img src="https://article.biliimg.com/bfs/article/88b65d7fad2554c91ce7d8e17737bea78901ecc1.png" alt="1653808993693" style="zoom:50%;" />



Feed流产品有两种常见模式：

1. Timeline：不做内容筛选，简单的按照内容发布时间排序，常用于好友或关注。例如朋友圈
	* 优点：信息全面，不会有缺失。并且实现也相对简单
	* 缺点：信息噪音较多，用户不一定感兴趣，内容获取效率低
2. 智能排序：利用智能算法屏蔽掉违规的、用户不感兴趣的内容。推送用户感兴趣信息来吸引用户
	* 优点：投喂用户感兴趣信息，用户粘度很高，容易沉迷
	* 缺点：如果算法不精准，可能起到反作用

我们基于关注的好友来做Feed流，因此采用`Timeline`的模式。该模式的实现方案有三种：
* 拉模式
* 推模式
* 推拉结合

**拉模式**：也叫做读扩散
	<img src="https://article.biliimg.com/bfs/article/5f591633e5d587fcb75351375d39cb02e7511eb7.png" alt="1653809450816" style="zoom:50%;" />

- 该模式的核心含义就是：当张三和李四和王五发了消息后，都会保存在**自己的邮箱中**，假设赵六要读取信息，那么他会从**读取他自己的收件箱**，此时系统会从他关注的人群中，把他关注人的信息全部都进行拉取，然后在进行排序
- **优点**：比较节约空间，因为赵六在读信息时，并没有重复读取，而且读取完之后可以把他的收件箱进行清楚。
- **缺点**：比较延迟，当用户读取数据时才去关注的人里边去读取数据，假设用户关注了大量的用户，那么此时就会拉取海量的内容，对服务器压力巨大。

**推模式**：也叫做写扩散。
	<img src="https://article.biliimg.com/bfs/article/139b5c97dca6ada02aa7aa49f95de4318c0a45f8.png" alt="1653809875208" style="zoom:50%;" />

- 推模式是没有写邮箱的，当张三写了一个内容，此时会主动的把张三写的内容发送到他的粉丝收件箱中去，假设此时李四再来读取，就不用再去临时拉取了
- **优点**：时效快，不用临时拉取
- **缺点**：内存压力大，假设一个大V写信息，很多人关注他， 就会写很多分数据到粉丝那边去

**推拉结合模式**：也叫做读写混合，兼具推和拉两种模式的优点。
	<img src="https://article.biliimg.com/bfs/article/81fe2aeae50d7a7d3f62478be1df49e05bb678e9.png" alt="1653812346852" style="zoom:50%;" />

推拉模式是一个折中的方案，站在发件人这一段，如果是个普通的人，那么我们采用**写扩散**的方式，直接把数据写入到他的粉丝中去，因为普通的人他的粉丝关注量比较小，所以这样做没有压力，如果是大V，那么他是直接将数据先写入到一份到发件箱里边去，然后再直接写一份到活跃粉丝收件箱里边去，现在站在收件人这端来看，如果是活跃粉丝，那么大V和普通的人发的都会直接写入到自己收件箱里边来，而如果是普通的粉丝，由于他们上线不是很频繁，所以等他们上线时，再从发件箱里边去拉信息。

### 好友关注-推送到粉丝收件箱

需求：
- 修改新增探店笔记的业务，在保存blog到数据库的同时，**推送到粉丝的收件箱**
	- 推送之前我们得知道粉丝，新建一个这样的redis的key
		- 修改之前关注的逻辑，关注是往redis里面存数据
	- 向所有的粉丝的收件箱推送博客消息
- 收件箱满足可以根据时间戳排序，必须用Redis的数据结构实现
- 查询收件箱数据时，可以实现分页查询

Feed流中的数据会不断更新，所以数据的角标也在变化，因此不能采用传统的分页模式。
传统了分页在feed流是不适用的，因为我们的数据会随时发生变化

<img src="https://article.biliimg.com/bfs/article/b6310dd508c2a448b72df64a9179fea3b6210bf0.png" style="zoom:50%;" />

假设在t1 时刻，我们去读取第一页，此时page = 1 ，size = 5 ，那么我们拿到的就是10~6 这几条记录，假设现在t2时候又发布了一条记录，此时t3 时刻，我们来读取第二页，读取第二页传入的参数是page=2 ，size=5 ，那么此时读取到的第二页实际上是从6 开始，然后是6~2 ，那么我们就读取到了重复的数据，所以feed流的分页，不能采用原始方案来做。

Feed流的**滚动分页**
我们需要记录**每次操作的最后一条**，然后从**这个位置开始**去读取数据

<img src="https://article.biliimg.com/bfs/article/83a1d284593e1bdc83b11af710e1ca908dc4edb1.png" alt="1653813462834" style="zoom:50%;" />

举个例子：我们从t1时刻开始，拿第一页数据，拿到了10~6，然后记录下当前最后一次拿取的记录，就是6，t2时刻发布了新的记录，此时这个11放到最顶上，但是不会影响我们之前记录的6，此时t3时刻来拿第二页，第二页这个时候拿数据，还是从6后一点的5去拿，就拿到了5-1的记录。我们这个地方可以采用sortedSet来做，可以进行范围查询，并且还可以记录当前获取数据时间戳最小值，就可以实现滚动分页了

> 因为list结构是记录角标的，如果zset是根据score排序那根list一样，要实现滚动排序，我们记录最后一次记录的不是脚本，而是那个记录值，而set可以结构可以获取指的score范围这点满足滚动排序，所以这个收件箱数据结构用**zset,key是用户名id,score是时间戳，value是blogId**



### 好友关注-实现分页查询收邮箱
需求：在个人主页的“关注”卡片中，查询并展示推送的Blog信息：
具体操作如下：
- 每次查询完成后，我们要分析出查询出数据的最小时间戳，这个值会作为下一次查询的条件
- 我们需要找到与上一次查询相同的查询个数作为偏移量，下次查询时，跳过这些查询过的数据，拿到我们需要的数据
综上：我们的请求参数中就需要携带 lastId：上一次查询的最小时间戳和偏移量这两个参数。
这两个参数第一次会由前端来指定，以后的查询就根据后台结果作为条件，再次传递到后台。

<img src="https://article.biliimg.com/bfs/article/570915b53bd58c4b6c2b4891b0661e3b8588bd65.png" alt="1653819821591" style="zoom:50%;" />
定义出来具体的返回值实体类

```java
@Data
public class ScrollResult {
    private List<?> list;
    private Long minTime;
    private Integer offset;
}
```

核心步骤：
1. 这个请求的实现类是叫做`queryBlogOfFollow`,参数就是lastId和`offset`(这个值是等于上次最小时间戳值相等个数),默认值是0
2. 获取当前用户的收件箱，查询收件箱
3. 解释这个收件箱的数据，也就是根据blogid返回这个博客
4. 收件箱中的每个blog都要有当前用户是否点赞和查询blog信息的功能



## 附近商户

### 附近商户-GEO数据结构的基本用法

 GEO就是Geolocation的简写形式，代表地理坐标。Redis在3.2版本中加入了对GEO的支持，允许存储地理坐标信息，帮助我们根据经纬度来检索数据。常见的命令有：
* `GEOADD`：添加一个地理空间信息，包含：经度（longitude）、纬度（latitude）、值（member）
* `GEODIST`：计算指定的两个点之间的距离并返回
* `GEOHASH`：将指定member的坐标转为hash字符串形式并返回
* `GEOPOS`：返回指定member的坐标
* `GEORADIUS`：指定圆心、半径，找到该圆内包含的所有member，并按照与圆心之间的距离排序后返回。6.以后已废弃
* `GEOSEARCH`：在指定范围内搜索member，并按照与指定点之间的距离排序后返回。范围可以是圆形或矩形。6.2.新功能
* `GEOSEARCHSTORE`：与GEOSEARCH功能一致，不过可以把结果存储到一个指定的key。 6.2.新功能

1. 添加下面几条数据：
	- 北京南站（116.378248 39.865275）
	- 北京站（116.42803 39.903738）
	- 北京西站（116.322287 39.893729）
1. 计算北京西站到北京站的距离
3. 搜索天安门（116.397904 39.909005）附近10km内的所有火车站，并按照距离升序排序

```r
geoadd g1 116.378248 39.865275 bjn 116.42803 39.903738 bjz 116.322287 39.893729 bjx
```

![image.png](https://article.biliimg.com/bfs/article/9d04fd1728e6a6110700804399042452d9fa12d7.png)

从这里我们可以看出来geo底层是`hashset`,经度和纬度可以转成score,value就是member

### 附近商户-导入店铺数据到GEO

具体场景说明：

<img src="https://article.biliimg.com/bfs/article/35bbba549a041b7b9caf083896815536c5032c3d.png" alt="1653822036941" style="zoom:80%;" />
当我们点击美食之后，会出现一系列的商家，商家中可以按照多种排序方式，我们此时关注的是**距离**，这个地方就需要使用到我们的GEO，向后台传入当前用户所在的地址(这里的x,y是当前位置的经度和维度) ，以**当前坐标作为圆心**，同时绑定相同的店家类型type，以及分页信息，把这几个条件传入后台，后台查询出对应的数据再返回。
按照商户类型**做分组**，类型相同的商户作为同一组，以`typeld`为key存入同一个GEO集合中即可 

<img src="https://article.biliimg.com/bfs/article/e1eb3c57f6d21ad1a49644ba34a1714b3e368ce9.png" style="zoom:50%;" />

核心步骤：
1. 当前端发布typeId后，查数据库对typeId进行过滤，找到对应店铺，然后做分页返回给前端 
2. 查询所有的店铺信息，按照typeid分组，形成一个map结构，分批导入redis中，key为`typeid`，value是shopid，score是坐标，这里用到stream流的`groupingBy`来分组，以及`redisGeoCommands.GeoLocation`来一次性导入redis
3. 实现对应的控制类
	- 判断是否需要查询坐标，不需要就按原来的数据库查询
	- 查询redis,按照距离进行排序、分页。
		- 这里先找出分页查询的beg和end的值
		- 这里需要截取beg-end的部分
		- 查询获取店铺id和距离
	- 根据id查询shop

## 用户签到
### 用户签到-BitMap功能演示
我们针对签到功能完全可以通过mysql来完成，比如说以下这张表

<img src="https://article.biliimg.com/bfs/article/e9f62e4f92e7963b0631d14e13c7d87f178904fa.png" alt="1653823145495" style="zoom:80%;" />
用户一次签到，就是一条记录，假如有1000万用户，平均每人每年签到次数为10次，则这张表一年的数据量为 1亿条
每签到一次需要使用（8 + 8 + 1 + 1 + 3 + 1）共**22字节的内存**，一个月则最多需要600多字节
我们如何能够简化一点呢？其实可以考虑小时候一个挺常见的方案，就是小时候，咱们准备一张小小的卡片，你只要签到就打上一个勾，我最后判断你是否签到，其实只需要到小卡片上看一看就知道了

<img src="https://article.biliimg.com/bfs/article/4a65d49915380adaee3b098b7f4f3c57684e68b8.png" alt="image.png" style="zoom:50%;" />


我们可以采用类似这样的方案来实现我们的签到需求。
我们按月来统计用户签到信息，签到记录为1，未签到则记录为0

<img src="https://article.biliimg.com/bfs/article/50fc8f398fab18de42ffaead0282d052fb89130c.png" alt="1653824498278" style="zoom:50%;" />

把每一个bit位对应当月的每一天，形成了**映射关系**。用0和1标示业务状态，这种思路就称为位图（BitMap）。这样我们就用极小的空间，来实现了大量数据的表示
`Redis`中是利用`string`类型数据结构实现`BitMap`，因此最大上限是**512M**，转换为bit则是 2^32个bit位。

BitMap的操作命令有：

|指令|              功能              |
| :-------: | :--------------------------: |
|`SETBIT`| 向指定位置（offset）存入一个0或1 |
| `GETBIT`  |   获取指定位置（offset）的bit值   |
| `BITCOUNT` | 统计BitMap中值为1的bit位的数量  |
|`BITFIELD`|操作（查询、修改、自增）BitMap中bit数组中的指定位置（offset）的值|
| `BITFIELD_RO` |获取BitMap中bit数组，并以十进制形式返回|
| `BITOP`  | 将多个BitMap的结果做位运算（与、或、异或）|
| `BITPOS`  | 查找bit数组中指定范围内第一个0或1出现的位置|

### 用户签到-实现签到功能

需求：实现签到接口，将当前用户当天签到信息保存到Redis中
思路：我们可以把年和月作为bitMap的key，然后保存到一个bitMap中，每次签到就到对应的位上把数字从0变成1，只要对应是1，就表明说明这一天已经签到了，反之则没有签到。
我们通过接口文档发现，此接口并没有传递任何的参数，没有参数怎么确实是哪一天签到呢？这个很容易，可以通过后台代码直接获取即可，然后到对应的地址上去修改bitMap。

<img src="https://article.biliimg.com/bfs/article/acfe2238d5fe68389fe0dfdb146055ec3144562d.png" alt="1653833970361" style="zoom: 50%;" />
核心步骤：
1. 获取当前用户
2. 获取日期
3. 获取今天是本月的第几天
4. 写入redis


### 用户签到-签到统计

**问题1：** 什么叫做连续签到天数？ 从最后一次签到开始向前统计，直到遇到第一次未签到为止，计算总的签到次数，就是连续签到天数。

<img src="https://article.biliimg.com/bfs/article/a6e87f7e0fcd5dc6732b0e07bddbf644db1d11e2.png" alt="image.png" style="zoom:50%;" />

**问题2：** 如何得到本月到今天为止的所有签到数据？
`BITFIELD key GET u[dayOfMonth] 0`
假设今天是10号，那么我们就可以从当前月的第一天开始，获得到当前这一天的位数，是10号，那么就是10位，去拿这段时间的数据，就能拿到所有的数据了，那么这10天里边签到了多少次呢？统计有多少个1即可。
**问题3：如何从后向前遍历每个bit位？**
注意：bitMap返回的数据是10进制，哪假如说返回一个数字8，那么我哪儿知道到底哪些是0，哪些是1呢？我们只需要让得到的10进制数字和1做与运算就可以了，因为1只有遇见1 才是1，其他数字都是0 ，我们把签到结果和1进行与操作，每与一次，就把签到结果向右移动一位，依次内推，我们就能完成逐个遍历的效果了。

<img src="https://article.biliimg.com/bfs/article/c67061c3d3f9466dcaa3498d961f05b08c0e98c7.png"  style="zoom:50%;" />

核心步骤
1. 获取本月截至今天为止的所有的签到记录(十进制数)
2. 循环遍历
3. 进行与运算和移位操作

## UV统计

### UV统计-HyperLogLog

首先我们搞懂两个概念：

- **UV**：全称Unique Visitor，也叫独立访客量，是指通过互联网访问、浏览这个网页的自然人。1天内同一个用户多次访问该网站，只记录1次。
- **PV**：全称Page View，也叫页面访问量或点击量，用户每访问网站的一个页面，记录1次PV，用户多次打开页面，则记录多次PV。往往用来衡量网站的流量。

UV统计在服务端做会比较麻烦，因为要判断该用户是否已经统计过了，需要将统计过的用户信息保存。但是如果每个访问的用户都保存到Redis中，数据量会非常恐怖，那怎么处理呢？

Hyperloglog(HLL)是从Loglog算法派生的概率算法，用于确定非常大的集合的基数，而不需要存储其所有值。相关算法原理大家可以参考：[https://juejin.cn/post/6844903785744056333#heading-0](https://juejin.cn/post/6844903785744056333#heading-0)
Redis中的HLL是基于string结构实现的，单个HLL的内存**永远小于16kb**，**内存占用低**的令人发指！作为代价，其测量结果是概率性的，**有小于0.81％的误差**。不过对于UV统计来说，这完全可以忽略。

```r
PFADD key element [element...] 
summary: Adds the specified elements to the specified HyperLogLog. 
since: 2.8.9 
PFCOUNT key [key ...] 
summary: Return the approximated cardinality of the set (s) observed by 
since: 2.8.9 
PFMERGE destkey sourcekey [sourcekey ...] 
summary: Merge N different HyperLogLogs into a single one. 
since: 2.8.9 
```


### UV统计-测试百万数据的统计

测试思路：我们直接利用单元测试，向`HyperLogLog`中添加100万条数据，看看内存占用和统计效果如何

```java
@Test 
void testHyperLogLog(){ 
	//准备数组，装用户数据 
	String[] users = new String[1000]; 
	//数组角标 
	int index = 0; 
	for (int i = 1; i <= 1000000; i++) { 
		//赋值 
		users[index++]="user_"+i; 
		//每1000条发送一次 
		if (i % 1000 == 0) { 
			index = 0; 
			stringRedisTemplate.opsForHyperLogLog().add("hll1",users); 
		}
	}
	//统计数量 
	Long size = stringRedisTemplate.opsForHyperLogLog ().size("hll1") ; 
	System.out.println ( " size = " + size ); 
}
```


# 高级篇Redis

## 分布式缓存


<img src="https://github.com/YeSho-cpp/pic_bed/blob/main/GIF%202023-8-24%2011-37-55.gif?raw=true" alt="image.png" style="zoom:50%;" />





单机的Redis存在四大问题：

<img src="https://article.biliimg.com/bfs/article/9cb9d495839679f170f5ea13293f50d7295c7c56.png" alt="image.png" style="zoom:50%;" />

### Redis持久化

Redis有两种持久化方案：

- RDB持久化
- AOF持久化

#### RDB持久化

RDB全称`Redis Database Backup file`（Redis数据备份文件），也被叫做**Redis数据快照**。简单来说就是把内存中的所有数据都记录到**磁盘中**。当Redis实例故障重启后，**从磁盘读取快照文件**，恢复数据。快照文件称为**RDB文件**，默认是保存在**当前运行目录**。

##### 执行时机

RDB持久化在四种情况下会执行：

- 执行`save`命令
- 执行`bgsave`命令
- Redis停机时
- 触发RDB条件时

当你的Redis服务器正常运行时，它会周期性地执行RDB持久化操作。默认情况下，Redis每隔60秒检查是否需要执行RDB持久化。如果满足以下两个条件之一，则会触发RDB持久化：

1. 如果在指定时间内有指定数量（默认10000个）以上的写操作（包括新增、修改、删除等），则会触发RDB持久化。
2. 如果在指定时间内有指定数量（默认1000个）以上的写操作，并且至少有一个新键被添加，则会触发RDB持久化。

当你的电脑关机重启后，Redis会重新加载上一次保存的RDB文件来恢复数据。这意味着即使Redis服务器停止工作时没有执行显式的`save`或`bgsave`命令，也能通过加载最近一次生成的RDB文件来恢复数据。

因此，在你电脑关机重启后，Redis数据仍然保存下来是因为发生了自动执行的周期性RDB持久化。



1. **save命令**
	执行下面的命令，可以立即执行一次RDB：
	```r
	[root@localhost ~]# redis-cli 
	127.0.0.1:6379>save     #由Redis主进程来执行RDB,会阻塞所有命令 
	ok 
	127.0.0.1:6379> 
	```

	save命令会导致主进程执行RDB，这个过程中**其它所有命令都会被阻塞**。只有在数据迁移时可能用到。

2. **bgsave命令**
	下面的命令可以异步执行RDB：

	```r
	127.0.0.1:6379>bgsave      #开启子进程执行RDB,避免主进程受到影响 
	Background saving started 
	```

	这个命令执行后会开启独立进程完成RDB，主进程可以持续处理用户请求，不受影响。

3. **停机时**

	Redis停机时会执行一次save命令，实现RDB持久化。

	```log
	User requested shutdown 
	Saving the final RDB snapshot before exiting. 
	DB saved on disk 
	Removing the pid file. 
	Redis is now ready to exit , bye bye 
	```

	在当前目录会生成rdb文件
	```r
	[root@localhost ~]# ll
	total 180
	-rw-------. 1 root root   2749 Jul 16 09:14 anaconda-ks.cfg
	-rw-r--r--. 1 root root 117666 Aug 23 20:22 dump.rdb
	-rw-------. 1 root root   2029 Jul 16 09:14 original-ks.cfg
	-rw-r--r--. 1 root root  55268 Aug 23 20:22 redis.log
	```

4. **触发RDB条件**

Redis内部有触发RDB的机制，可以在redis.conf文件中找到，格式如下：

```properties
# 900秒内，如果至少有1个key被修改，则执行bgsave ， 如果是save "" 则表示禁用RDB
save 900 1  
save 300 10  
save 60 10000 
```

RDB的其它配置也可以在redis.conf文件中设置：

```properties
# 是否压缩 ,建议不开启，压缩也会消耗cpu，磁盘的话不值钱
rdbcompression yes
# RDB文件名称
dbfilename dump.rdb  
# 文件保存的路径目录
dir ./ 
```


##### RDB原理

`bgsave`开始时会fork主进程(**这个过程是阻塞的**)得到子进程，子进程**共享**主进程的内存数据。完成fork后读取内存数据并写入RDB文件。

<img src="https://article.biliimg.com/bfs/article/c49d00d45edc1846c8cb36c945a3800d2143c345.png" style="zoom:80%;" />
> [!summary] fork底层的原理:
> 1. 物理内存相当于物理的内存条,在linux系统中,所有进程都是**无法直接操纵物理内存的**,是操作系统给每个进程**分配虚拟内存**
> 2. 主进程只能操作虚拟内存,操作系统会维护一个虚拟内存与物理内存的**映射表**(页表)
> 3. 主进程操作虚拟内存,虚拟内存基于页表的映射关系到真正的物理内存,主进程这样来实现对物理内存数据的读写
> 4. fork的过程会创建一个子进程,它**copy的不是内存数据,而是页表**,子进程有了跟主进程相同的映射关系,这样就实现了主进程和子进程内存空间的共享
> 5. 所以这个copy的速度是非常快的
> 6. 子进程就可以写入RDB文件了

但是如果主进程在写,子进程在读进行持久化操作,就会造成读写不一致

fork采用的是`copy-on-write`技术： 
- 当主进程执行读操作时，访问共享内存； 
- 当主进程执行写操作时，则会拷贝一份数据，执行写操作。 

RDB方式bgsave的基本流程？
- fork主进程得到一个子进程，共享内存空间
- 子进程读取内存数据并写入新的RDB文件
- 用新RDB文件替换旧的RDB文件
RDB会在什么时候执行？`save 60 1000`代表什么含义？
- 默认是服务停止时
- 代表60秒内至少执行1000次修改则触发RDB
RDB的缺点？
- RDB执行间隔时间长，两次RDB之间写入数据有丢失的风险(在执行下次rdb就宕机了)
- fork子进程、压缩、写出RDB文件都比较耗时

#### AOF持久化

AOF全称为`Append Only File`（追加文件）。Redis处理的每一个写命令都会记录在AOF文件，可以看做是命令日志文件

<img src="https://article.biliimg.com/bfs/article/569fec7435a5a69418bb5dd5f360819ec42dda6d.png" alt="image-20210725151543640" style="zoom:50%;" />
##### AOF配置

AOF默认是关闭的，需要修改`redis.conf`配置文件来开启AOF

```properties
# 是否开启AOF功能，默认是no
appendonly yes
# AOF文件的名称
appendfilename "appendonly.aof"
```

AOF的命令记录的频率也可以通过redis.conf文件来配：

```properties
# 表示每执行一次写命令，立即记录到AOF文件
appendfsync always 
# 写命令执行完先放入AOF缓冲区，然后表示每隔1秒将缓冲区数据写到AOF文件，是默认方案
appendfsync everysec 
# 写命令执行完先放入AOF缓冲区，由操作系统决定何时将缓冲区内容写回磁盘
appendfsync no
```

三种策略对比： 

| 配置项   | 刷盘时机     | 优点                  | 缺点                     |
|:-:|:-:|:-:|:-:|
| Always   | 同步刷盘     | 可靠性高，几乎不丢数据  | 性能影响大               |
| everysec | 每秒刷盘     | 性能适中              | 最多丢失1秒数据           |
| no       | 操作系统控制 | 性能最好              | 可靠性较差，可能丢失大量数据  | 


##### AOF文件重写

因为是记录命令，**AOF文件会比RDB文件大的多**。而且AOF会记录对同一个key的多次写操作，但只有**最后一次写操作才有意义**。通过执行`bgrewriteaof`命令，可以让AOF文件执行重写功能，用最少的命令达到相同效果。

<img src="https://article.biliimg.com/bfs/article/99aa4bb3fc3e0a62e621a4fbfaa410c4a34f3a64.png" alt="image-20210725151729118" style="zoom:67%;" />

如图，AOF原本有三个命令，但是`set num 123 和 set num 666`都是对num的操作，第二次会覆盖第一次的值，因此第一个命令记录下来没有意义。
所以重写命令后，AOF文件内容就是：`mset name jack num 666`

Redis也会在触发**阈值**时自动去重写AOF文件。阈值也可以在redis.conf中配置：
```properties
# AOF文件比上次文件 增长超过多少百分比则触发重写
auto-aof-rewrite-percentage 100
# AOF文件体积最小多大以上才触发重写 
auto-aof-rewrite-min-size 64mb 
```

#### RDB与AOF对比
RDB和AOF各有自己的优缺点，如果对数据安全性要求较高，在实际开发中往往会**结合**两者来使用。 

|  |**RDB**|**AOF**|
|:-:|:-:|:-:|
| 持久化方式         |定时对整个内存做快照| 记录每一次执行的命令         |
| 数据完整性         | 不完整，两次备份之间会丢失 | 相对完整，取决于刷盘策略     |
| 文件大小             | 会有压缩，文件体积小         | 记录命令，文件体积很大             |
| 恢复速度           | 很快                           | 慢                                    |
| 数据恢复优先级   | 低，因为数据完整性不如AOF   | 高，因为数据完整性更高       |
| 系统资源占用       | 高，大量CPU和内存消耗     | 低，主要是磁盘I/O资源          |
| 使用场景           | 可以容忍数分钟的数据丢失   | 对数据安全性要求较高常见   |

### Redis主从

#### 搭建主从架构

单节点Redis的并发能力是有上限的，要进一步提高Redis的并发能力，就需要搭建主从集群，实现读写分离。

<img src="https://article.biliimg.com/bfs/article/625b77d88da11549c50e67c4d5a4be3b4a415883.png" alt="image.png" style="zoom:50%;" />
具体搭建流程参考课前资料《[[redis集群#2.Redis主从集群|Redis集群]]》
#### 主从数据同步原理

##### 全量同步

主从第一次建立连接时，会执行**全量同步**，将master节点的所有数据都拷贝给slave节点，流程：

<img src="https://article.biliimg.com/bfs/article/26bc72068882d3fb583a6e01ecfa737b865da0ba.png"  style="zoom:50%;" />
注意点:
1. 判断是第一次,要把完整数据给slave,在这之前要返回master的数据版本信息,主节点(Master)的数据版本信息指的是主节点的复制偏移量(`replication offset`)和运行ID(`run ID`)。
2. 通过传递复制偏移量，从节点可以知道它需要从主节点的哪个位置开始进行**数据同步**。而运行ID则用于验证主节点的身份，确保从节点连接到了正确的主节点。
3. 执行`bgsave命令`,根据前面所学是有save是异步的,会有数据丢失的风险,只能保证数据基本一致,所以才需要`repl_baklog`这个缓冲区命令,缓存区命令+RDB数据才是一致的完整数据

^a5bdb1

master如何得知salve是第一次来连接呢？？
有几个概念，可以作为判断依据：
- **Replication Id**：简称replid，是数据集的标记，id一致则说明是同一数据集。每一个master都有唯一的replid，slave则会**继承**master节点的replid
- **offset**：偏移量，随着记录在repl_baklog中的数据增多而逐渐增大。slave完成同步时也会**记录当前同步的offset**。如果slave的offset小于master的offset，说明slave数据落后于master，需要**更新**。

因此slave做数据同步，必须向master声明自己的`replication id`和`offset`，master才可以判断到底需要同步哪些数据。  


<img src="https://article.biliimg.com/bfs/article/32aedef4da0e9ea8d33dc95d7a810a63383ba867.png" alt="image.png" style="zoom:50%;" />

- 因为slave原本也是一个master，有自己的replid和offset，当第一次变成slave，与master建立连接时，发送的replid和offset是自己的replid和offset。
- master判断发现slave发送来的replid与自己的不一致，说明这是一个全新的slave，就知道要做全量同步了。
- master会将自己的replid和offset都发送给这个slave，slave保存这些信息。以后slave的replid就与master一致了。
因此，**master判断一个节点是否是第一次同步的依据，就是看replid是否一致**。

##### 增量同步

全量同步需要先做RDB，然后将RDB文件通过网络传输个slave，成本太高了。因此除了第一次做全量同步，其它大多数时候slave与master都是做**增量同步**。

什么是增量同步？就是只更新slave与master存在差异的部分数据。如图：

<img src="https://article.biliimg.com/bfs/article/4e52063d1fa13d9e0fb24bd13c3040da7c50bd99.png" style="zoom:50%;" />

##### .repl_backlog原理

master怎么知道slave与自己的数据差异在哪里呢?
这就要说到全量同步时的`repl_baklog`文件了。
这个文件是一个固定大小的数组，只不过数组是**环形**，也就是说**角标到达数组末尾后，会再次从0开始读写**，这样数组头部的数据就会被覆盖。
`repl_baklog`中会记录Redis处理过的命令日志及offset，包括master当前的offset，和slave已经拷贝到的offset：

<img src="https://article.biliimg.com/bfs/article/2b1afdcfbdb14bb73ca16ca9cc0740a9948fee77.png" alt="image.png" style="zoom:80%;" />
slave与master的offset之间的差异，就是salve需要增量拷贝的数据了。
随着不断有数据写入，master的offset逐渐变大，slave也不断的拷贝，追赶master的offset：

<img src="https://article.biliimg.com/bfs/article/9401deddea078046efb1c81645f86c44b837f31e.png" alt="image.png" style="zoom:80%;" />
直到数组被填满：

<img src="https://article.biliimg.com/bfs/article/6c7c508895ca2b59712c492de053dbb4fd8d6c0e.png" alt="image.png" style="zoom:80%;" />
此时，如果有新的数据写入，就会覆盖数组中的旧数据。不过，旧的数据只要是绿色的，说明是已经被同步到slave的数据，即便被覆盖了也没什么影响。因为未同步的仅仅是红色部分。
但是，如果slave出现网络阻塞，导致master的offset远远超过了slave的offset： 

<img src="https://article.biliimg.com/bfs/article/6ba2e6b5ab4d5b05a993f97743f672cb95886a46.png" alt="image.png" style="zoom:80%;" />
如果master继续写入新数据，其offset就会覆盖旧的数据，直到将slave现在的offset也覆盖：

<img src="https://article.biliimg.com/bfs/article/054abf509da05eb1fb515d8b39964964aea29d1a.png" alt="image.png" style="zoom:80%;" />

棕色框中的红色部分，就是尚未同步，但是却已经被覆盖的数据。此时如果slave恢复，需要同步，却发现自己的offset都没有了(被master抹除了)，无法完成增量同步了。只能做全量同步。

     
> [!warning] 注意: 
> `repl_baklog`大小有上限，写满后会覆盖最早的数据。如果slave断开时间过久，导致尚未备份的数据被覆盖，则无法基于log做增量同步，只能再次全量同步。 


#### 主从同步优化

主从同步可以保证主从数据的一致性，非常重要。
可以从以下几个方面来优化Redis主从就集群：

- 在master中配置`repl-diskless-sync yes`启用无磁盘复制，避免全量同步时的磁盘IO。

```properties
# With slow disks and fast (large bandwidth) networks, diskless replication
# works better.
# 当将该参数设置为yes时，表示启用无磁盘同步。这意味着在主节点将数据发送给从节点进行同步时，主节点不会将数据先写入磁盘，而是直接通过网络传输给从节点。从节点接收到数据后，会直接写入自己的内存中进行恢复。
repl-diskless-sync no
```

- Redis单节点上的内存占用不要太大，减少RDB导致的过多磁盘IO
- 适当提高`repl_baklog`的大小，发现slave宕机时尽快实现故障恢复，尽可能避免全量同步
- 限制一个master上的slave节点数量，如果实在是太多slave，则可以采用**主-从-从**链式结构，减少master压力 
主从从架构图：

<img src="https://article.biliimg.com/bfs/article/e3f433cda9d143aaa92e6a7b63603ce9f0c6ce9a.png" alt="image.png" style="zoom:50%;" />

### Redis哨兵

#### 什么是哨兵模式：

1、哨兵模式的架构：

<img src="https://img-blog.csdnimg.cn/img_convert/11289f8814e5ddd4163a2bd76da9b5e9.png" alt="image.png" style="zoom:70%;" />

2、什么是哨兵模式：
这里的高可用时什么？
在主从模式下（主从模式就是把上图的所有哨兵去掉），master节点负责写请求，然后异步同步给slave节点，从节点负责处理读请求。如果master宕机了，需要手动<font color=#ff0000>将从节点晋升为主节点</font>，并且还要<font color=#ff0000>切换客户端的连接数据源</font>。这就无法达到高可用，而通过哨兵模式就可以解决这一问题。

哨兵模式是Redis的高可用方式，哨兵节点是**特殊的redis服务**，不提供读写服务，主要用来**监控redis实例节点**。 哨兵架构下client端第一次**从哨兵找出redis的主节点**，后续就直接访问redis的主节点，不会每次都**通过sentinel代理访问**redis的主节点，当redis的主节点挂掉时，哨兵会第一时间感知到，并且在slave节点中重新选出来一个新的master，然后将新的master信息通知给client端，从而实现高可用。这里面redis的client端一般都实现了**订阅功能**，订阅sentinel发布的**节点变动消息**。

**哨兵的主要工作任务**：

> **监控**：哨兵会不断地检查你的Master和Slave是**否运作正常**。
> **提醒**：当被监控的某个Redis节点出现问题时，哨兵可以**通过 API 向管理员或者其他应用程序发送通知**。
> **自动故障迁移**：当一个Master不能正常工作时，哨兵会进行**自动故障迁移操作**，将失效Master的其中一个Slave升级为新的Master，并让失效Master的其他Slave改为复制新的Master；当客户端试图连接失效的Master时，集群也会向客户端返回新Master的地址，使得集群可以使用新Master代替失效Master。

#### 哨兵原理

##### 集群结构和作用

哨兵的结构如图：
<img src="https://article.biliimg.com/bfs/article/487916f6e6f2cadd6d90db8ac77dcd6cce7420a9.png" alt="image.png" style="zoom:50%;" />
哨兵的作用如下：

- **监控**：Sentinel 会不断检查您的master和slave是否按预期工作
- **自动故障恢复**：如果master故障，Sentinel会将一个slave提升为master。当故障实例恢复后也以新的master为主
- **通知**：Sentinel充当Redis客户端的服务发现来源，当集群发生故障转移时，会将最新信息推送给Redis的客户端

##### 集群监控原理

Sentinel基于**心跳机制监测服务状态**，每隔1秒向集群的每个实例发送ping命令：

- 主观下线：如果某sentinel节点发现某实例未在规定时间响应，则认为该实例**主观下线**。
- 客观下线：若超过**指定数量**（quorum）的sentinel都认为该实例主观下线，则该实例**客观下线**。`quorum`值最好超过Sentinel实例数量的一半。

<img src="https://article.biliimg.com/bfs/article/849bc3c7ad3012bb33f422d61e3ac1e3fc68dca2.png" alt="image.png" style="zoom:70%;" />
##### 集群故障恢复原理

一旦发现master故障，sentinel需要在salve中选择一个作为新的master，选择依据是这样的：

- 首先会判断slave节点与master节点断开时间长短，如果超过指定值（`down-after-milliseconds * 10`）则会排除该slave节点
- 然后判断slave节点的`slave-priority`值(初始的默认值是1)，越小优先级越高，如果是0则永不参与选举
- 如果`slave-prority`一样，则判断slave节点的offset值，越大说明数据越新，优先级越高
- 最后是判断slave节点的运行id大小，越小优先级越高。

当选出一个新的master后，该如何实现切换呢？

流程如下：


<img src="https://article.biliimg.com/bfs/article/24a5142d383c0a615a4403d242259b6c8142bb79.png" alt="image-20210725154816841" style="zoom:67%;" />

- sentinel给备选的slave1节点发送`slaveof no one`命令，让该节点成为master
- sentinel给**所有其它slave**发送`slaveof 192.168.153.128 7002`命令，让这些slave成为新master的从节点，开始从新的master上同步数据。
- 最后，sentinel将**故障节点标记为slave**，当故障节点恢复后会自动成为新的master的slave节点

#### 搭建哨兵集群

具体搭建流程参考课前资料[[redis集群|《Redis集群.md》]]

#### RedisTemplate

在`Sentinel`集群监管下的Redis主从集群，其节点会因为自动故障转移而发生变化，Redis的客户端必须感知这种变化，及时更新连接信息。Spring的RedisTemplate底层利用lettuce实现了节点的感知和自动切换。

##### 配置Redis地址

然后在配置文件application.yml中指定redis的sentinel相关信息：

```yml
spring:
  redis:
    sentinel:
      master: mymaster
      nodes:
        - 192.168.153.128:27001
        - 192.168.153.128:27002
        - 192.168.153.128:27003
```

##### 配置读写分离

在项目的启动类中，添加一个新的bean：
```java
@Bean
public LettuceClientConfigurationBuilderCustomizer clientConfigurationBuilderCustomizer(){
    return clientConfigurationBuilder -> clientConfigurationBuilder.readFrom(ReadFrom.REPLICA_PREFERRED);
}
```

这个bean中配置的就是读写策略，包括四种：
- `MASTER`：从主节点读取
- `MASTER_PREFERRED`：优先从master节点读取，master不可用才读取replica
- `REPLICA`：从slave（replica）节点读取
- `REPLICA _PREFERRED`：优先从slave（replica）节点读取，所有的slave都不可用才读取master

### Redis分片集群

#### 搭建分片集群

主从和哨兵可以解决高可用、高并发读的问题。但是依然有两个问题没有解决：

- 海量数据存储问题
- 高并发写的问题

使用分片集群可以解决上述问题，如图:

<img src="https://article.biliimg.com/bfs/article/c24ec8849721e38e1a8303ed9c9d020cfc11708e.png" alt="image.png" style="zoom:70%;" />
分片集群特征：

- 集群中有**多个master**，每个`master`保存不同数据(解决高并发写的问题)
- 每个master都可以有多个slave节点(解决高并发读的问题)
- master之间通过ping监测彼此健康状态(起到一个哨兵的作用)
- 客户端请求可以访问集群任意节点，最终都会被转发到正确节点

具体搭建流程参考课前资料[[redis集群#4.搭建分片集群|搭建分片集群]]：


jedis搭建集群代码

```java
private JedisCluster jedisCluster;
@BeforeEach
    void setUp() {
        // 配置连接池
        JedisPoolConfig poolConfig = new JedisPoolConfig();
        poolConfig.setMaxTotal(8);
        poolConfig.setMaxIdle(8);
        poolConfig.setMinIdle(0);
        poolConfig.setMaxWaitMillis(1000);
        HashSet<HostAndPort> nodes = new HashSet<>(); //。HostAndPort 类来自于Jedis库，是用于存储Redis主机名和端口号的组合的类
        nodes.add(new HostAndPort("192.168.150.101", 7001));
        nodes.add(new HostAndPort("192.168.150.101", 7002));
        nodes.add(new HostAndPort("192.168.150.101", 7003));
        nodes.add(new HostAndPort("192.168.150.101", 8001));
        nodes.add(new HostAndPort("192.168.150.101", 8002));
        nodes.add(new HostAndPort("192.168.150.101", 8003));
        jedisCluster = new JedisCluster(nodes, poolConfig);
    }


```


#### 散列插槽

##### 插槽原理

Redis会把每一个master节点映射到0~16383共16384个插槽（`hash slot`）上，查看集群信息时就能看到：
<img src="https://article.biliimg.com/bfs/article/79bf50f481e2b7bdb1423a4762f163e3db908edc.png" alt="image-20210725155820320" style="zoom:80%;" />

数据key不是与节点绑定，而是**与插槽绑定**(以防宕机情况出现)。redis会根据key的有效部分计算插槽值，分两种情况：
- key中包含"`{}`"，且“`{}`”中至少包含1个字符，“`{}`”中的部分是有效部分
- key中不包含“`{}`”，整个key都是有效部分
例如：key是num，那么就根据num计算，如果是{itcast}num，则根据itcast计算。计算方式是利用**CRC16算法**得到一个**hash值**，然后对16384取余，得到的结果就是**slot值**。

有集群情况下启动redis客户端的命令要加`-c`代表集群方式
```sh
redis-cli -c -p 7001
127.0.0.1:7001> set num 123
OK
127.0.0.1:7001> set a 1
-> Redirected to slot [15495] located at 192.168.153.128:7003  #重定向到7003
OK
192.168.153.128:7003>
```

如图，在7001这个节点执行`set a 1`时，对a做hash运算，对16384取余，得到的结果是15495，因此要存储到7003节点。
到了7003后，执行`get num`时，对num做hash运算，对16384取余，得到的结果是2765，因此需要切换到7001节点

```sh
127.0.0.1:7003> get a
"1"
127.0.0.1:7003> get num
-> Redirected to slot [2765] located at 192.168.153.128:7001
"123"
192.168.153.128:7001>
```


> [!summary] 总结
> Redis如何判断某个key应该在哪个实例
> - 将16384个插槽分配到不同的实例
> - 根据key的有效部分计算哈希值，对16384取余
> - 余数作为插槽，寻找插槽所在实例即可
> 如何将同一类数据固定的保存在同一个Redis实例？
> - 这一类数据使用相同的有效部分，例如key都以`{typeId}`为前缀     


#### 集群伸缩

`redis-cli --cluster`提供了很多操作集群的命令，可以通过下面方式查看：

<img src="https://article.biliimg.com/bfs/article/6b1e529169d92baff8b32aa6b0e0ef7124db536e.png" alt="image-20210725160138290" style="zoom:70%;" />

比如，添加节点的命令：

<img src="https://article.biliimg.com/bfs/article/704f133fa4214eb734c972ccce4d124af7cf3c59.png" alt="image-20210725160448139" style="zoom: 80%;" />
##### 需求分析

需求：向集群中添加一个新的master节点，并向其中存储 num = 10
- 启动一个新的redis实例，端口为7004
- 添加7004到之前的集群，并作为一个master节点   
- 给7004节点分配插槽，使得num这个key可以存储到7004实例
这里需要两个新的功能：
- 添加一个节点到集群中
- 将部分插槽分配到新插槽

##### 创建新的redis实例

创建一个文件夹：

```sh
mkdir 7004
```

拷贝配置文件：

```sh
cp redis.conf /7004
```

修改配置文件：

```sh
sed /s/6379/7004/g 7004/redis.conf
```

启动

```sh
redis-server 7004/redis.conf
```

##### 添加新节点到redis

添加节点的语法如下：
<img src="https://article.biliimg.com/bfs/article/704f133fa4214eb734c972ccce4d124af7cf3c59.png" alt="image-20210725160448139" style="zoom:80%;" />
执行命令：
```sh
redis-cli --cluster add-node  192.168.153.128:7004 192.168.153.128:7001
```

通过命令查看集群状态：
```sh
redis-cli -p 7001 cluster nodes
```

如图，7004加入了集群，并且默认是一个master节点：

<img src="https://article.biliimg.com/bfs/article/064dd7416507f3320a1fb76e1759585605a4b93d.png" alt="image-20210725161007099" style="zoom:80%;" />
但是，可以看到7004节点的插槽数量为0，因此没有任何数据可以存储到7004上

##### 转移插槽

我们要将num存储到7004节点，因此需要先看看num的插槽是多少：

<img src="https://article.biliimg.com/bfs/article/7011f162b5634aac943e2cb09d21f86429a0260d.png" alt="image-20210725161241793" style="zoom:80%;" />
如上图所示，num的插槽为2765.
我们可以将0~3000的插槽从7001转移到7004，命令格式如下：

<img src="https://article.biliimg.com/bfs/article/68784ca733dec651138d85bfa36d72b6631b32d5.png" alt="image-20210725161401925" style="zoom:80%;" />
具体命令如下：
建立连接：

<img src="https://article.biliimg.com/bfs/article/288dba01f6f0af7042fd7906d64cab7267407a46.png" alt="image-20210725161506241" style="zoom:80%;" />
得到下面的反馈：

<img src="https://article.biliimg.com/bfs/article/4b2f5e15beae45f9d3882b87fa286df9aae9ffc3.png" alt="image-20210725161540841" style="zoom:80%;" />

询问要移动多少个插槽，我们计划是3000个：
新的问题来了：
<img src="https://article.biliimg.com/bfs/article/003fcc3e9660231c6b5cef33f6bd87fd6a3d96b4.png" alt="image-20210725161637152" style="zoom:80%;" />

那个node来接收这些插槽？？
显然是7004，那么7004节点的id是多少呢？

<img src="https://article.biliimg.com/bfs/article/14b0e3db1d8d890a2d9dc5df8039997b646027e1.png" alt="image-20210725161731738" style="zoom:80%;" />
复制这个id，然后拷贝到刚才的控制台后：

<img src="https://article.biliimg.com/bfs/article/7cd94be46d25f433131d2376bf58bb120fe1a2d1.png" alt="image-20210725161817642" style="zoom:50%;" />

这里询问，你的插槽是从哪里移动过来的？
- all：代表全部，也就是三个节点各转移一部分
- 具体的id：目标节点的id
- done：没有了

这里我们要从7001获取，因此填写7001的id：
<img src="https://article.biliimg.com/bfs/article/5866f094221293d8a1c5982fe093b65650b0d7ee.png" alt="image-20210725162030478" style="zoom:50%;" />
填完后，点击done，这样插槽转移就准备好了：

<img src="https://article.biliimg.com/bfs/article/6ed2e15f6fd323c1272af2feeb5cf031633a8794.png" alt="image-20210725162101228" style="zoom:50%;" />

确认要转移吗？输入yes：
然后，通过命令查看结果：

<img src="https://article.biliimg.com/bfs/article/97c4c99983025f71ebf00926b294f5b8f9162b01.png" alt="image-20210725162145497" style="zoom:80%;" />

可以看到：
<img src="https://article.biliimg.com/bfs/article/f747afc1f08bd03fa8f5e2d96d22a10891da3da7.png" alt="image-20210725162224058" style="zoom:50%;" />
目的达成。

#### 故障转移
集群初识状态是这样的：
<img src="https://article.biliimg.com/bfs/article/d9846e54a77495cf38f6690ee2b7ebb3c81c8a29.png" alt="image-20210727161152065" style="zoom:50%;" />

其中7001、7002、7003都是master，我们计划让7002宕机。

##### 自动故障转移

当集群中有一个master宕机会发生什么呢？
直接停止一个redis实例，例如7002：
```sh
redis-cli -p 7002 shutdown
```

1. 首先是该实例与其它实例失去连接
2. 然后是疑似宕机：

	<img src="https://article.biliimg.com/bfs/article/d917a8bcf63b78664e83da67ea4bdb257ef9fe87.png" alt="image-20210725162319490" style="zoom:80%;" />
3. 最后是确定下线，自动提升一个slave为新的master：

	<img src="https://article.biliimg.com/bfs/article/8c86db9f6da39f9cf35669e189cfafcca3c051f9.png" alt="image-20210725162408979" style="zoom:80%;" />

4. 当7002再次启动，就会变为一个slave节点了：

	<img src="https://article.biliimg.com/bfs/article/03d715df05331a3d2465ebf689fe3e627b8089f7.png" alt="image-20210727160803386" style="zoom:50%;" />

##### 手动故障转移

利用`cluster failover`命令可以手动让集群中的某个master宕机，切换到执行`cluster failover`命令的这个slave节点，实现**无感知的数据迁移**。其流程如下：

<img src="https://article.biliimg.com/bfs/article/f071c9b4f7cb2a817acf782cd0aa81b03c932e0b.png" alt="image-20210725162441407" style="zoom:80%;" />
这种failover命令可以指定三种模式：
- 缺省：默认的流程，如图1~6歩  
- `force`：省略了对offset的一致性校验
- `takeover`：直接执行第5歩，忽略数据一致性、忽略master状态和其它master的意见


**案例需求**：在7002这个slave节点执行手动故障转移，重新夺回master地位

步骤如下：
1. 利用redis-cli连接7002这个节点
2. 执行cluster failover命令

如图：

<img src="https://article.biliimg.com/bfs/article/1cc2c5896a3230a374068f1a2bac80dd01fb66e1.png" alt="image-20210727160037766" style="zoom:80%;" />


效果：
<img src="https://article.biliimg.com/bfs/article/d9846e54a77495cf38f6690ee2b7ebb3c81c8a29.png" alt="image-20210727161152065" style="zoom:80%;" />

#### RedisTemplate访问分片集群

RedisTemplate底层同样基于lettuce实现了分片集群的支持，而使用的步骤与哨兵模式基本一致：

1. 引入redis的starter依赖
2. 配置分片集群地址
3. 配置读写分离
与哨兵模式相比，其中只有分片集群的配置方式略有差异，如下：

```yml
spring:
  redis:
    cluster:
      nodes:
        - 192.168.153.128:7001
        - 192.168.153.128:7002
        - 192.168.153.128:7003
        - 192.168.153.128:8001
        - 192.168.153.128:8002
        - 192.168.153.128:8003
```

## 多级缓存

### 什么是多级缓存

传统的缓存策略一般是请求到达[[javaweb#tomcat服务器|Tomcat]]后，先查询Redis，如果未命中则查询数据库，如图：
<img src="https://article.biliimg.com/bfs/article/541e8432cfa13de8d6a64dff92b425bb81014689.png" alt="image-20210821075259137" style="zoom:70%;" />

存在下面的问题：
- 请求要经过Tomcat处理，**Tomcat的性能**成为整个系统的**瓶颈**
- Redis缓存失效时，会对数据库产生冲击

**多级缓存就是**充分利用**请求处理的每个环节**，分别添加缓存，减轻Tomcat压力，提升服务性能：

<img src="https://article.biliimg.com/bfs/article/a9918395ee294096797d928429cdb90cea21b428.png" alt="image.png" style="zoom:50%;" />

- 浏览器访问静态资源时，优先读取**浏览器本地缓存**(一级缓存)
- 访问非静态资源（ajax查询数据）时，访问服务端
- 请求到达Nginx后，优先读取**Nginx本地缓存**(二级缓存)
- 如果Nginx本地缓存未命中，则去**直接查询Redis**（不经过Tomcat）(三级缓存)
- 如果Redis查询未命中，则查询Tomcat
- 请求进入Tomcat后，优先查询**JVM进程缓存**(四级缓存)
- 如果JVM进程缓存未命中，则查询数据库

在多级缓存架构中，Nginx内部需要编写本地缓存查询、Redis查询、Tomcat查询的业务逻辑，因此这样的nginx服务不再是一个**反向代理服务器**，而是一个编写**业务的Web服务器了**。
因此这样的业务Nginx服务也需要搭建**集群**来提高并发，再有专门的nginx服务来做反向代理，如图：

<img src="https://article.biliimg.com/bfs/article/bbab5347e024bba464f0090e17d58ecc601b79e4.png" alt="image-20210821080511581" style="zoom:50%;" />
### JVM进程缓存

#### 导入商品查询页面

商品查询是购物页面，与商品管理的页面是分离的。

部署方式如图：

<img src="https://article.biliimg.com/bfs/article/0c28aa5900ef95a7ffabc72559db72a74561f210.png" alt="image-20210816111210961" style="zoom:50%;" />

我们需要准备一个反向代理的nginx服务器，如上图红框所示，将静态的商品页面放到nginx目录中。页面需要的数据通过ajax向服务端（**nginx业务集群**）查询。

在nginx这样配置

```nginx.conf
 # nginx-cluster集群，就是将来的nginx业务集群 
    upstream nginx-cluster{
        server 192.168.153.128:8081; 
    }
    server {
        listen       80;
        server_name  localhost;

    # 监听/api路径，反向代理 到nginx-cluster集群 
	location /api {
            proxy_pass http://nginx-cluster;
        }

```


<img src="https://article.biliimg.com/bfs/article/ccadea4f7823358c93ba5d3c1b3d91adc76ef45c.png" alt="image.png" style="zoom:50%;" />

#### 初识Caffeine

缓存在日常开发中启动至关重要的作用，由于是存储在**内存**中，数据的读取速度是非常快的，能大量减少对数据库的访问，减少数据库的压力。我们把缓存分为两类： 
- **分布式缓存**，例如`Redis`: 
	- 优点：存储容量更大、可靠性更好、可以在集群间共享 
	- 缺点：访问缓存有网络开销 
	- 场景：缓存数据量较大、可靠性要求较高、需要在集群间共享 
- **进程本地缓存**，例如`HashMap`、`GuavaCache`: 
	- 优点：读取本地内存，没有网络开销，速度更快 
	- 缺点：存储容量有限、可靠性较低、无法共享 
	- 场景：性能要求较高，缓存数据量较小 

**caffeine**是一个基于Java8开发的，提供了近乎最佳命中率的高性能的本地缓存库。目前Spring内部的缓存使用的就是Caffeine。GitHub地址：[https://github.com/ben-manes/caffeine](https://github.com/ben-manes/caffeine)

Caffeine的性能非常好，下图是官方给出的性能对比：

<img src="https://article.biliimg.com/bfs/article/f2413b3bbbd8b66ecdb272dcadacd6eaf4b54382.png" style="zoom:77%;" />
缓存使用的基本API：

```java
@Test
void testBasicOps() {
    // 构建cache对象
    Cache<String, String> cache = Caffeine.newBuilder().build();

    // 存数据
    cache.put("gf", "迪丽热巴");

    // 取数据
    String gf = cache.getIfPresent("gf"); //如果存在就查询
    System.out.println("gf = " + gf);

    // 取数据，包含两个参数：
    // 参数一：缓存的key
    // 参数二：Lambda表达式，表达式参数就是缓存的key，方法体是查询数据库的逻辑
    // 优先根据key查询JVM缓存，如果未命中，则执行参数二的Lambda表达式，顺便往这个jvm缓存中存入数据库查询数据
    String defaultGF = cache.get("defaultGF", key -> {
        // 根据key去数据库查询数据
        return "柳岩";
    });
    System.out.println("defaultGF = " + defaultGF);
}
```


Caffeine既然是缓存的一种，肯定需要有缓存的清除策略，不然的话内存总会有耗尽的时候。
Caffeine提供了三种缓存驱逐策略：

- **基于容量**：设置缓存的数量上限
```java
// 创建缓存对象
Cache<String, String> cache = Caffeine.newBuilder()
    .maximumSize(1) // 设置缓存大小上限为 1
    .build();
```
- **基于时间**：设置缓存的有效时间
```java
// 创建缓存对象
Cache<String, String> cache = Caffeine.newBuilder()
    // 设置缓存有效期为 10 秒，从最后一次写入开始计时 
    .expireAfterWrite(Duration.ofSeconds(10)) 
    .build();

```
- **基于引用**：设置缓存为软引用或弱引用，利用GC来回收缓存数据。性能较差，不建议使用。

> [!note] 注意
> 在默认情况下，当一个缓存元素过期的时候，Caffeine不会自动立即将其清理和驱逐。而是在**一次读或写操作后**，或者在空闲时间完成对失效数据的驱逐。

```java
@Test
    void testEvictByNum() throws InterruptedException {
        // 创建缓存对象
        Cache<String, String> cache = Caffeine.newBuilder()
                // 设置缓存大小上限为 1
                .maximumSize(1)
                .build();
        // 存数据
        cache.put("gf1", "柳岩");
        cache.put("gf2", "范冰冰");
        cache.put("gf3", "迪丽热巴");
        // 延迟10ms，给清理线程一点时间
        Thread.sleep(10L);
        // 获取数据
        System.out.println("gf1: " + cache.getIfPresent("gf1"));
        System.out.println("gf2: " + cache.getIfPresent("gf2"));
        System.out.println("gf3: " + cache.getIfPresent("gf3"));
    }
gf1: null
gf2: null
gf3: 迪丽热巴
```


#### 实现JVM进程缓存

##### 需求

利用Caffeine实现下列需求：
- 给根据id查询商品的业务添加缓存，缓存未命中时查询数据库
- 给根据id查询商品库存的业务添加缓存，缓存未命中时查询数据库
- 缓存初始大小为100
- 缓存上限为10000

核心步骤：
- 首先，我们需要定义两个Caffeine的缓存对象，分别保存商品、库存的缓存数据。
	- 对象写在config文件夹中，定义成bean,分别是itemCache和stockCache
- 修改之前的查找商品和商品库存的逻辑，添加一层优先查找本地缓存


### Lua语法入门

<img src="https://article.biliimg.com/bfs/article/d24bf41519068dcf0cd171f8314b4bd8c0b3cd09.png" alt="image.png" style="zoom:50%;" />
- 前面我们已经实现了tomcat的进程缓存，编写tomcat的进程缓存我们用的是Tomcat+java
- 现在要完成nginx的业务集群实现nginx的本地缓存,这里要用到[[lua|Lua语言]]

#### 初识Lua

Lua 是一种轻量小巧的脚本语言，用标准C语言编写并以源代码形式开放， 其设计目的是为了嵌入应用程序中，从而为应用程序提供灵活的扩展和定制功能。官网：[https://www.lua.org/](https://www.lua.org/)

<img src="http://www.lua.org/images/lua30.gif" alt="image.png" style="zoom:50%;" />
Lua经常嵌入到**C语言开发的**程序中，例如游戏开发、游戏插件等。Nginx本身也是C语言开发，因此也允许**基于Lua做拓展**。

#### 变量和循环

学习任何语言必然离不开变量，而变量的声明必须先知道数据的类型。

##### Lua的数据类型

Lua中支持的常见数据类型包括：

|数据类型|描述|
|:-:|:-:|
|`nil`|这个最简单，只有值nil属于该类，表示一个无效值（在条件表达式中相当于false)。|
|`boolean`| 包含两个值：false和true |
| `number` | 表示双精度类型的实浮点数 |
| `string` |字符串由一对双引号或单引号来表示,拼接用`..`|
| `function` |由C或Lua编写的**函数** |
| `table` |Lua 中的表（table)其实是一个"**关联数组**"(`associative arrays`),数组的索引可以是数字、字符串或表类型。在Lua里，table的创建是通过”**构造表达式**“来完成，最简单构造表达式是{},用来创建一个空表。|

另外，Lua提供了`type()`函数来判断一个变量的数据类型：

```lua
print(type("Hello world")) 
string 
print(type(10.4*3)) 
number 

```

##### 声明变量

Lua声明变量的时候无需指定数据类型，而是用local来声明变量为局部变量：
```lua
-- 声明字符串，可以用单引号或双引号，
local str = 'hello'
-- 字符串拼接可以使用 ..
local str2 = 'hello' .. 'world'
-- 声明数字
local num = 21
-- 声明布尔类型
local flag = true
```

Lua中的table类型既可以作为数组，又可以作为Java中的map来使用。数组就是特殊的table，key是数组角标而已：

```lua
-- 声明数组 ，key为角标的 table
local arr = {'java', 'python', 'lua'}
-- 声明table，类似java的map
local map =  {name='Jack', age=21}
```

Lua中的数组**角标是从1开始**，访问的时候与Java中类似：

```lua
-- 访问数组，lua数组的角标从1开始
print(arr[1])
```

Lua中的table可以用key来访问：

```lua
-- 访问table
print(map['name'])
print(map.name)
```

##### 循环

对于table，我们可以利用for循环来遍历。不过数组和普通table遍历略有差异。
遍历数组：
```lua
-- 声明数组 key为索引的 table
local arr = {'java', 'python', 'lua'}
-- 遍历数组
for index,value in ipairs(arr) do
    print(index, value) 
end
```

遍历普通table
```lua
-- 声明map，也就是table
local map = {name='Jack', age=21}
-- 遍历table
for key,value in pairs(map) do
   print(key, value) 
end

map = {name='jack',age=21}
print(map['name'])
print(map.name)
```

#### 条件控制、函数

Lua中的条件控制和函数声明与Java类似。

##### 函数

定义函数的语法：

```lua
function 函数名( argument1, argument2..., argumentn)
    -- 函数体
    return 返回值
end
```

例如，定义一个函数，用来打印数组：

```lua
function printArr(arr)
    for index, value in ipairs(arr) do
        print(value)
    end
end
```

##### 条件控制

类似Java的条件控制，例如if、else语法：

```lua
if(布尔表达式)
then
   --[ 布尔表达式为 true 时执行该语句块 --]
else
   --[ 布尔表达式为 false 时执行该语句块 --]
end
```

与java不同，布尔表达式中的逻辑运算是基于英文单词：

|操作符|描述|实例|
|:-:|:-:|:-:|
| and    |逻辑与操作符。若A为false，则返回A，否则返回B| (A and B)为 false   |
| or     |逻辑或操作符。若A为true，则返回A，否则返回B| (A or B)为 true     |
| not    |逻辑非操作符。与逻辑运算结果相反，如果条件为true，逻辑非为false。| not(A and B)为 true |

### 实现多级缓存

多级缓存的实现离不开Nginx编程，而Nginx编程又离不开OpenResty。

#### 安装OpenResty

OpenResty®是一个**基于Nginx**的高性能**Web平台**，用于方便地搭建能够处理超高并发、扩展性极高的动态Web应用、Web服务和动态网关。具备下列特点：

- 具备Nginx的完整功能
- 基于Lua语言进行扩展，集成了大量精良的Lua库、第三方模块
- 允许使用Lua**自定义业务逻辑**、**自定义库**

官方网站： [https://openresty.org/cn/](https://openresty.org/cn/)

#### OpenResty快速入门

我们希望达到的多级缓存架构如图：

<img src="https://article.biliimg.com/bfs/article/40823550037502b33cb7e2fc62ec1a68f0cd322d.png" alt="yeVDlwtfMx" style="zoom:67%;" />
##### OpenResty监听请求

OpenResty的很多功能都依赖于其目录下的Lua库，需要在nginx.conf中指定依赖库的目录，并导入依赖：

1）添加对OpenResty的Lua模块的加载
修改`/usr/local/openresty/nginx/conf/nginx.conf`文件，在其中的http下面，添加下面代码：
```nginx
#lua 模块
lua_package_path "/usr/local/openresty/lualib/?.lua;;";
#c模块     
lua_package_cpath "/usr/local/openresty/lualib/?.so;;";  
```

2）监听/api/item路径
修改`/usr/local/openresty/nginx/conf/nginx.conf`文件，在nginx.conf的server下面，添加对/api/item这个路径的监听：
```nginx
location  /api/item {
    # 默认的响应类型
    default_type application/json;
    # 响应结果由lua/item.lua文件来决定
    content_by_lua_file lua/item.lua;
}
```

这个监听，就类似于SpringMVC中的`@GetMapping("/api/item")`做路径映射。
而`content_by_lua_file lua/item.lua`则相当于调用item.lua这个文件，执行其中的业务，把结果返回给用户。相当于java中调用service。


##### 编写item.lua

1）在`/usr/loca/openresty/nginx`目录创建文件夹：lua
2）在`/usr/loca/openresty/nginx/lua`文件夹下，新建文件：item.lua
3）编写item.lua，返回假数据
item.lua中，利用`ngx.say()`函数返回数据到Response中

```lua
--ngx.say()函数来输出指定的 JSON 字符串。`ngx.say()` 是 ngx_lua 模块提供的函数，用于将内容发送给客户端。在这个例子中，JSON 字符串被直接传递给 `ngx.say()` 函数作为参数。
ngx.say('{"id":10001,"name":"SALSA AIR","title":"RIMOWA 21寸托运箱拉杆箱 SALSA AIR系列果绿色 820.70.36.4","price":17900,"image":"https://m.360buyimg.com/mobilecms/s720x720_jfs/t6934/364/1195375010/84676/e9f2c55f/597ece38N0ddcbc77.jpg!q70.jpg.webp","category":"拉杆箱","brand":"RIMOWA","spec":"","status":1,"createTime":"2019-04-30T16:00:00.000+00:00","updateTime":"2019-04-30T16:00:00.000+00:00","stock":2999,"sold":31290}')
```


#### 请求参数处理

上一节中，我们在OpenResty接收前端请求，但是返回的是假数据。
要返回真实数据，必须根据前端传递来的商品id，查询商品信息才可以。
那么如何获取前端传递的商品参数呢？

##### 获取参数的API
OpenResty中提供了一些API用来获取不同类型的前端请求参数：

<img src="https://article.biliimg.com/bfs/article/1708ac318c7c2c468c379d63ef84bbb064cb4633.png" style="zoom:67%;" />
1）获取商品id

修改`/usr/loca/openresty/nginx/nginx.conf`文件中监听/api/item的代码，利用正则表达式获取ID：
```nginx
location ~ /api/item/(\d+) {
    # 默认的响应类型
    default_type application/json;
    # 响应结果由lua/item.lua文件来决定
    content_by_lua_file lua/item.lua;
}
```

2）拼接ID并返回
修改`/usr/loca/openresty/nginx/lua/item.lua`文件，获取id并拼接到结果中返回：
```lua
-- 获取商品id
local id = ngx.var[1]
-- 拼接并返回
ngx.say('{"id":' .. id .. ',"name":"SALSA AIR","title":"RIMOWA 21寸托运箱拉杆箱 SALSA AIR系列果绿色 820.70.36.4","price":17900,"image":"https://m.360buyimg.com/mobilecms/s720x720_jfs/t6934/364/1195375010/84676/e9f2c55f/597ece38N0ddcbc77.jpg!q70.jpg.webp","category":"拉杆箱","brand":"RIMOWA","spec":"","status":1,"createTime":"2019-04-30T16:00:00.000+00:00","updateTime":"2019-04-30T16:00:00.000+00:00","stock":2999,"sold":31290}')
```

#### 查询Tomcat

拿到商品ID后，本应去缓存中查询商品信息，不过目前我们还未建立nginx、redis缓存。因此，这里我们先根据商品id去`tomcat`查询商品信息。我们实现如图部分：

<img src="https://article.biliimg.com/bfs/article/048760a484aecc831e9442eb22cc747ce4f71de3.png" alt="image-20210821102610167" style="zoom:67%;" />

需要注意的是，我们的OpenResty是在虚拟机，Tomcat是在Windows电脑上。两者IP一定不要搞错了。
<img src="https://article.biliimg.com/bfs/article/b7bf0a7a675c391ea4ff2b808f48056ebdee595a.png" alt="im" style="zoom:67%;" />
##### 发送http请求的API

nginx提供了内部API用以**发送http请求**：
```lua
local resp = ngx.location.capture("/path",{
    method = ngx.HTTP_GET,   -- 请求方式
    args = {a=1,b=2},  -- get方式传参数
})
```

返回的响应内容包括：
- `resp.status`：响应状态码
- `resp.header`：响应头，是一个table
- `resp.body`：响应体，就是响应数据
注意：这里的path是路径，并不包含IP和端口。这个请求会被nginx内部的server监听并处理。
但是我们希望这个请求发送到Tomcat服务器，所以还需要编写一个server来对这个路径做反向代理：

```nginx
 location /path {
     # 这里是windows电脑的ip和Java服务端口，需要确保windows防火墙处于关闭状态
     proxy_pass http://192.168.150.1:8081; 
 }
```

##### 封装http工具

下面，我们封装一个发送Http请求的工具，基于`ngx.location.capture`来实现查询tomcat。
1）添加反向代理，到windows的Java服务
因为`item-service`中的接口都是/item开头，所以我们监听/item路径，代理到windows上的tomcat服务。
修改 `/usr/local/openresty/nginx/conf/nginx.conf`文件，添加一个location：
```nginx
location /item {
    proxy_pass http://192.168.150.1:8081;
}
```

以后，只要我们调用`ngx.location.capture("/item")`，就一定能发送请求到windows的tomcat服务。

2）封装工具类
之前我们说过，OpenResty启动时会加载以下两个目录中的工具文件：

<img src="https://article.biliimg.com/bfs/article/08c3b24913fc8a81bd5265a92fe79b8e2564e691.png" style="zoom: 67%;" />
所以，自定义的http工具也需要放到这个目录下。

在`/usr/local/openresty/lualib`目录下，新建一个`common.lua`文件：
```bash
vi /usr/local/openresty/lualib/common.lua
```

内容如下:
```lua
-- 封装函数，发送http请求，并解析响应
local function read_http(path, params)
    local resp = ngx.location.capture(path,{
        method = ngx.HTTP_GET,
        args = params,
    })
    if not resp then
        -- 记录错误信息，返回404
        ngx.log(ngx.ERR, "http请求查询失败, path: ", path , ", args: ", args)
        ngx.exit(404)
    end
    return resp.body
end
-- 将方法导出
local _M = {  
    read_http = read_http
}  
return _M
```

这个工具将read_http函数封装到_M这个table类型的变量中，并且返回，这类似于导出。
使用的时候，可以利用`require('common')`来导入该函数库，这里的common是函数库的文件名。

这里查询到的结果是json字符串，并且包含商品、库存两个json字符串，页面最终需要的是把两个json拼接为一个json：

<img src="https://article.biliimg.com/bfs/article/f2cc4a129e381c022ef923aaf596a452c1e04020.png" style="zoom:67%;" />
这就需要我们先把JSON变为lua的table，完成数据整合后，再转为JSON。

##### CJSON工具类

OpenResty提供了一个cjson的模块用来处理JSON的序列化和反序列化。

官方地址： [https://github.com/openresty/lua-cjson/](https://github.com/openresty/lua-cjson/)

1）引入cjson模块：
```lua
local cjson = require "cjson"
```

2）序列化：

```lua
local obj = {
    name = 'jack',
    age = 21
}
-- 把 table 序列化为 json
local json = cjson.encode(obj)
```

2）反序列化：

```lua
local json = '{"name": "jack", "age": 21}'
-- 反序列化 json为 table
local obj = cjson.decode(json);
print(obj.name)
```


##### 实现Tomcat查询

下面，我们修改之前的`item.lua`中的业务，添加json处理功能：
```lua
-- 导入common函数库
local common = require('common')
local read_http = common.read_http
-- 导入cjson库
local cjson = require('cjson')

-- 获取路径参数
local id = ngx.var[1]
-- 根据id查询商品
local itemJSON = read_http("/item/".. id, nil)
-- 根据id查询商品库存
local itemStockJSON = read_http("/item/stock/".. id, nil)

-- JSON转化为lua的table
local item = cjson.decode(itemJSON)
local stock = cjson.decode(stockJSON)

-- 组合数据
item.stock = stock.stock
item.sold = stock.sold

-- 把item序列化为json 返回结果
ngx.say(cjson.encode(item))
```


##### 基于ID负载均衡

刚才的代码中，我们的tomcat是单机部署。而实际开发中，tomcat一定是集群模式：
这里我们可以采用[[nginx#负载均衡|nginx的负载均衡]]

<img src="https://article.biliimg.com/bfs/article/278b02bf6f6b55d9427bf9b360e98f47a733f0b4.png" style="zoom: 67%;" />
IDEA启动[多台端口的设置](https://blog.csdn.net/weixin_42585386/article/details/109206410?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169381160216800213045318%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=169381160216800213045318&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-2-109206410-null-null.142^v93^control&utm_term=IDEA%E5%BC%80%E5%90%AF%E5%A4%9A%E4%B8%AA%E5%90%8E%E5%8F%B0%E7%AB%AF%E5%8F%A3&spm=1018.2226.3001.4187)

因此，OpenResty需要对tomcat集群做负载均衡。
而默认的负载均衡规则是轮询模式，当我们查询`/item/10001`时：
- 第一次会访问8081端口的tomcat服务，在该服务内部就形成了JVM进程缓存
- 第二次会访问8082端口的tomcat服务，该服务内部没有JVM缓存（**因为JVM缓存无法共享**），会查询数据库
- **不同的Tomcat服务运行在不同的JVM进程中**
    
这里使用[[nginx#hash|nginx的hash]]

#### Redis缓存预热

Redis缓存会面临冷启动问题：
**冷启动**：服务刚刚启动时，Redis中并没有缓存，如果所有商品数据都在第一次查询时添加缓存，可能会给数据库带来较大压力。
**缓存预热**：在实际开发中，我们可以利用大数据统计用户访问的热点数据，在项目启动时将这些热点数据**提前查询并保存到Redis中**。

我们数据量较少，并且没有数据统计相关功能，目前可以在启动时将所有数据都放入缓存中。
1）利用Docker安装Redis
```bash
docker run --name redis -p 6379:6379 -d redis redis-server --appendonly yes
```

2）在item-service服务中引入Redis依赖
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

3）配置Redis地址
```yaml
spring:
  redis:
    host: 192.168.153.128
```

4）编写初始化类
缓存预热需要在项目启动时完成，并且必须是拿到`RedisTemplate`之后。
这里我们利用[[spring#接口控制|InitializingBean]]接口来实现，因为`InitializingBean`可以在对象被Spring创建并且成员变量全部注入后执行。

```java
@Component
public class RedisHandler impements InitializingBean
{
	@Autowired
	private StringRedisTemplate redisTemplate;
	@Override
	public void afterPropertiesSet() throws Exception { //初始化缓存}
}
```

初始化缓存的步骤
- 查询商品信息
- 将商品信息放入缓存
- 这里的转json用到[[spring#json数据|springmvc使用的jsons序列化工具]]




#### 查询Redis缓存

现在，Redis缓存已经准备就绪，我们可以再OpenResty中实现查询Redis的逻辑了。如下图红框所示：

<img src="https://article.biliimg.com/bfs/article/881b7a08a8dcef061b9ae97888829718701abe98.png" alt="image.png" style="zoom:50%;" />
当请求进入OpenResty之后：
- 优先查询Redis缓存  
- 如果Redis缓存未命中，再查询Tomcat

**接下来要学习的是nginx如何去查询redis?**

##### 封装Redis工具

OpenResty提供了操作Redis的模块，我们只要引入该模块就能直接使用。但是为了方便，我们将Redis操作封装到之前的common.lua工具库中。

修改`/usr/local/openresty/lualib/common.lua`文件：

1）引入Redis模块，并初始化Redis对象

```lua
-- 导入redis
local redis = require('resty.redis')
-- 初始化redis
local red = redis:new() -- 创建了一个新的 Redis 客户端实例
red:set_timeouts(1000, 1000, 1000)
```

2）封装函数，用来释放Redis连接，其实是放入连接池
```lua
-- 关闭redis连接的工具方法，其实是放入连接池
local function close_redis(red)
    local pool_max_idle_time = 10000 -- 连接的空闲时间，单位是毫秒
    local pool_size = 100 --连接池大小
    local ok, err = red:set_keepalive(pool_max_idle_time, pool_size)
    if not ok then
        ngx.log(ngx.ERR, "放入redis连接池失败: ", err)
    end
end
```

3）封装函数，根据key查询Redis数据

```lua
-- 查询redis的方法 ip和port是redis地址，key是查询的key
local function read_redis(ip, port, key)
    -- 获取一个连接
    local ok, err = red:connect(ip, port)
    if not ok then
        ngx.log(ngx.ERR, "连接redis失败 : ", err)
        return nil
    end
    -- 查询redis
    local resp, err = red:get(key)
    -- 查询失败处理
    if not resp then
        ngx.log(ngx.ERR, "查询Redis失败: ", err, ", key = " , key)
    end
    --得到的数据为空处理
    if resp == ngx.null then
        resp = nil
        ngx.log(ngx.ERR, "查询Redis数据为空, key = ", key)
    end
    close_redis(red)
    return resp
end
```

4）导出
```lua
-- 将方法导出
local _M = {  
    read_http = read_http,
    read_redis = read_redis
}  
return _M
```

##### 实现redis查询

接下来，我们就可以去修改item.lua文件，实现对Redis的查询了。

查询逻辑是：
- 根据id查询Redis
- 如果查询失败则继续查询Tomcat
- 将查询结果返回

1）修改`/usr/local/openresty/lua/item.lua`文件，添加一个查询函数：
```lua
-- 导入common函数库
local common = require('common')
local read_http = common.read_http
local read_redis = common.read_redis
-- 封装查询函数
function read_data(key, path, params)
    -- 查询本地缓存
    local val = read_redis("127.0.0.1", 6379, key)
    -- 判断查询结果
    if not val then
        ngx.log(ngx.ERR, "redis查询失败，尝试查询http， key: ", key)
        -- redis查询失败，去查询http
        val = read_http(path, params)
    end
    -- 返回数据
    return val
end
```

而后修改商品查询、库存查询的业务：

```lua
-- 获取路径参数
local id = ngx.var[1]

-- 查询商品信息
local itemJSON = read_data("item:id:" .. id,  "/item/" .. id, nil)
-- 查询库存信息
local stockJSON = read_data("item:stock:id:" .. id, "/item/stock/" .. id, nil)
```

#### nginx本地缓存

现在，整个多级缓存中只差最后一环，也就是nginx的本地缓存了。如图：

<img src="https://article.biliimg.com/bfs/article/906275176263c3b73128f27585b8dbb119bbc6d6.png" style="zoom:67%;" />
##### 本地缓存API

OpenResty为Nginx提供了**shard dict**的功能，可以在nginx的多个worker之间共享数据，实现缓存功能。
1）开启共享字典，在nginx.conf的http下添加配置：
```nginx
 # 共享字典，也就是本地缓存，名称叫做：item_cache，大小150m
 lua_shared_dict item_cache 150m; 
```

2）操作共享字典：
```lua
-- 获取本地缓存对象
local item_cache = ngx.shared.item_cache
-- 存储, 指定key、value、过期时间，单位s，默认为0代表永不过期
item_cache:set('key', 'value', 1000)
-- 读取
local val = item_cache:get('key')
```

##### 实现本地缓存查询

1）修改`/usr/local/openresty/lua/item.lua`文件，修改read_data查询函数，添加本地缓存逻辑

```lua
-- 导入共享词典，本地缓存
local item_cache = ngx.shared.item_cache

-- 封装查询函数
function read_data(key, expire, path, params)
    -- 查询本地缓存
    local val = item_cache:get(key)
    if not val then
        ngx.log(ngx.ERR, "本地缓存查询失败，尝试查询Redis， key: ", key)
        -- 查询redis
        val = read_redis("127.0.0.1", 6379, key)
        -- 判断查询结果
        if not val then
            ngx.log(ngx.ERR, "redis查询失败，尝试查询http， key: ", key)
            -- redis查询失败，去查询http
            val = read_http(path, params)
        end
    end
    -- 查询成功，把数据写入本地缓存
    item_cache:set(key, val, expire)
    -- 返回数据
    return val
end
```

2）修改item.lua中查询商品和库存的业务，实现最新的read_data函数：

```lua
-- 查询商品信息
local itemJSON = read_data("item:id:" .. id,1800, "/item/" .. id, nil)
-- 查询库存信息
local stockJSON = read_data("item:stock:id:" .. id,60, "/item/stock/" .. id, nil)
```

- 其实就是多了缓存时间参数，过期后nginx缓存会自动删除，下次访问即可更新缓存。
- 这里给商品基本信息设置超时时间为30分钟，库存为1分钟。
- 因为库存更新频率较高，如果缓存时间过长，可能与数据库差异较大。


### 缓存同步

虽然使用了缓存增加了访问的速度，减少了访问数据库的次数，效率提高，但同时也带来了**缓存同步**问题，如果缓存数据与数据库数据存在较大差异，可能会产生比较严重的后果。所以我们必须保证数据库数据、缓存数据的一致性，这就是缓存与数据库的同步。

#### 数据同步策略

缓存数据同步的常见方式有三种：

1. **设置有效期**：给缓存设置有效期，到期后自动删除。再次查询时更新

	- 优势：简单、方便
	- 缺点：**时效性差**，缓存过期之前可能不一致
	- 场景：更新频率较低，**时效性要求低的业务**
    

2. **同步双写**：在修改数据库的同时，直接修改缓存

	- 优势：时效性强，缓存与数据库强一致
	- 缺点：有**代码侵入，耦合度**高；
	- 场景：对**一致性、时效性要求较高**的缓存数据
    
3. **异步通知：** 修改数据库时**发送事件通知**，相关服务**监听到通知**后修改缓存数据

	- 优势：**低耦合**，可以同时通知多个缓存服务
	- 缺点：时效性一般，可能存在中间不一致状态
	- 场景：时效性要求一般，有多个服务需要同步

而异步实现又可以基于MQ或者**Canal**来实现：
1）基于**MQ的异步**通知：

<img src="https://article.biliimg.com/bfs/article/58f4eb4ccf7a049fbab71d4328dd2dc43b96821b.png" alt="image-20210821115552327" style="zoom:50%;" />

解读：
- 商品服务完成对数据的修改后，只需要**发送一条消息到MQ中**。
- 缓存服务监听MQ消息，然后完成对缓存的更新
依然有少量的代码侵入。

2）基于**Canal的通知**
<img src="https://article.biliimg.com/bfs/article/5c2f399309a53ac4a9f67718569e1bbf4913be0a.png" alt="image-20210821115719363" style="zoom:50%;" />


解读：
- 商品服务完成商品修改后，业务直接结束，没有任何代码侵入
- **Canal监听MySQL变化**，当发现变化后，立即通知缓存服务
- 缓存服务接收到canal通知，更新缓存
代码零侵入

### 安装Canal

**认识Canal**

**Canal [kə'næl]**，译意为水道/管道/沟渠，canal是阿里巴巴旗下的一款开源项目，基于Java开发。基于**数据库增量日志解析**，提供增量**数据订阅&消费**。GitHub的地址：[https://github.com/alibaba/canal](https://github.com/alibaba/canal)

Canal是基于**mysql的主从同步**来实现的，MySQL主从同步的原理如下：

<img src="https://article.biliimg.com/bfs/article/1c87bf41fe8042728a392aed9238175e9e1917cb.png" style="zoom:67%;" />

**mysql主从原理讲解**：
1. 首先mysql主节点(mater)在做数据增删改时,会记录一个日志到`binary log`文件中,其中记录的数据叫做`binary log events(可以理解为记录执行的命令)`
2. 这个时候slave会**开启一个线程**不断得来**读取这个log文件**,放到一个`replay log`文件中
3. slave会再有一个线程去`replay`这个log并**执行这些操作**,这样可以达到主节点做哪些sql,从节点也做哪些sql,达到数据一致


而Canal就是把**自己伪装成MySQL的一个slave节点**，从而**监听master的binary log变化**。再把得到的变化信息通知给Canal的客户端，进而完成对其它数据库的同步。

<img src="https://camo.githubusercontent.com/63881e271f889d4a424c55cea2f9c2065f63494fecac58432eac415f6e47e959/68747470733a2f2f696d672d626c6f672e6373646e696d672e636e2f32303139313130343130313733353934372e706e67" alt="image.png" style="zoom:50%;" />

#### 监听Canal

Canal提供了各种语言的客户端，当Canal监听到binlog变化时，会通知Canal的客户端。

我们可以利用Canal提供的Java客户端，监听Canal通知消息。当收到变化的消息时，完成对缓存的更新。
不过这里我们会使用GitHub上的第三方开源的`canal-starter`客户端。地址：[https://github.com/NormanGyllenhaal/canal-client](https://github.com/NormanGyllenhaal/canal-client)
与SpringBoot完美整合，自动装配，比官方客户端要简单好用很多。

**引入依赖**：
```xml
<dependency>
    <groupId>top.javatool</groupId>
    <artifactId>canal-spring-boot-starter</artifactId>
    <version>1.2.1-RELEASE</version>
</dependency>
```

**编写配置**:
```yml
canal:
  destination: heima # canal的集群名字，要与安装canal时设置的名称一致
  server: 192.168.150.101:11111 # canal服务地址
```


#### 编写监听器

```java
@CanalTable("tb_item") // 要监听的表的名字
@Component
public class ItemHandler implements EntryHandler<Item> { //EntryHandler指定表关联的实体类，它定义一组处理数据表记录的方法
  @Override
  public void insert(Item item) {
    EntryHandler.super.insert(item);
  }

  @Override
  public void update(Item before, Item after) {
    EntryHandler.super.update(before, after);
  }

  @Override
  public void delete(Item item) {
    EntryHandler.super.delete(item);
  }
}

```


这个有个问题：canal怎么把数据库的数据变成item实体类呢？而canal底层并不依赖于mybatis，所以canal要加一些注解

Canal推送给canal-client的是被修改的这一行数据（row),而我们引入的`canal-client`则会帮我们把行数据封装到Item实体类中。这个过程中需要知道**数据库与实体的映射关系**，要用到PA的几个注解： 

```java
@Data  
@TableName("tb_item")  
public class Item {  
    @TableId(type = IdType.AUTO)  
    @Id  //标记主键字段
    private Long id;//商品id  
    @Column(name = "name")  
    private String name;//商品名称  
    private String title;//商品标题  
    private Long price;//价格（分）  
    private String image;//商品图片  
    private String category;//分类名称  
    private String brand;//品牌名称  
    private String spec;//规格  
    private Integer status;//商品状态 1-正常，2-下架  
    private Date createTime;//创建时间  
    private Date updateTime;//更新时间  
    @TableField(exist = false)  
    @Transient //标记不属于表中的字段  
    private Integer stock;  
    @TableField(exist = false) 
    @Transient 
    private Integer sold;  
}
```

```java
@CanalTable("tb_item") // 要监听的表的名字
@Component
public class ItemHandler implements EntryHandler<Item> { //EntryHandler指定表关联的实体类，它定义一组处理数据表记录的方法

  @Autowired
  private RedisHander redisHandler;
  @Autowired
  private CaffeineConfig caffeineConfig;

  @Override
  public void insert(Item item) {
    // 写数据到JVM进程缓存
    caffeineConfig.itemCache().put(item.getId(), item);
    // 写数据到redis
    redisHandler.saveItem(item);
  }

  @Override
  public void update(Item before, Item after) {
    // 写数据到JVM进程缓存
    caffeineConfig.itemCache().put(after.getId(),after);
    // 写数据到redis
    redisHandler.saveItem(after);
  }

  @Override
  public void delete(Item item) {
    // 删除数据到JVM进程缓存
    caffeineConfig.itemCache().invalidate(item.getId());
    // 删除数据到redis
    redisHandler.deleteItemById(item.getId());
  }
}
```


### 多级缓存总结


![image.png](https://article.biliimg.com/bfs/article/7254c02aabcbd62411506f23978f4913be2af560.png)


步骤：
1. 我们首先这个`item.html`页面放在window这台nginx上，就是一个静态资源服务器和反向代理服务器，静态资源就放在上面，用户通过浏览器请求时，把这个页面返还给用户
2. 而用户的浏览器在渲染的时候发现缺少数据啊，就会发送ajax请求来查询数据，查询地址就是`item/10001`,nginx反向不会去处理业务，只做反向代理，会把请求反向代理给`OpenRestynginx`集群
3. 在`OpenRestynginx`集群上还做了本地缓存，查询商品信息优先从`share dictory`上面去查一查,这里集群是不共享内存的，所以这里针对商品的id去做缓存
4. `OpenRestynginx`没命中，会去做redis查询缓存，reis缓存没命中去查tomcat进程缓存



## 最佳实践
### Redis键值设计

#### 优雅的key结构

Redis的Key虽然可以自定义，但最好遵循下面的几个最佳实践约定：
- 遵循基本格式：`[业务名称]:[数据名]:[id]`
- 长度不超过44字节
- 不包含特殊字符
    
例如：我们的登录业务，保存用户信息，其key可以设计成如下格式：

<img src="https://article.biliimg.com/bfs/article/7111fc81b08a60f9d2dec6f3453031b4c5744096.png" alt="image-20220521120213631" style="zoom:67%;" />
这样设计的好处：
- 可读性强
- 避免key冲突
- 方便管理 
- 更节省内存： key是string类型，底层编码包含`int、embstr和raw`三种。
	- embstr在小于44字节使用，采用连续内存空间，内存占用更小。
	- 当字节数大于44字节时，会转为raw模式存储，在raw模式下，内存空间不是连续的，而是采用一个指针指向了另外一段内存空间，在这段空间里存储SDS内容，这样空间不连续，访问的时候性能也就会收到影响，还有可能产生内存碎片

```bash
127.0.0.1:6379>set num 123 
OK 
127.0.0.1:6379>type num   # 这个获取的是值得类型
string 
127.0.0.1:6379>object encoding num  # 这个获取得值的内部实际编码类型
"int" 
127.0.0.1:6379>set name Jack 
OK 
127.0.0.1:6379>object encoding 
name 
"embstr" 
127.0.0.1:6379>type name 
string 
127.0.0.1:6379>set name aaaaaaaaaaaaaaaaaaaaaaaaaaa 
OK  
127.0.0.1:6379>type name 
string 
127.0.0.1:6379>object encoding name 
"embstr" 
127.0.0.1:6379>set name aaaaaaaaaaaaaaaaaaaaaaaaaaa 
OK 
127.0.0.1:6379>object encoding name 
"raw" 
127.0.0.1:6379>
```

#### 拒绝BigKey

BigKey通常以Key的大小和Key中成员的数量来综合判定，例如：

- Key本身的数据量过大：一个String类型的Key，它的值为5 MB
- Key中的成员数过多：一个ZSET类型的Key，它的成员数量为10,000个
- Key中成员的数据量过大：一个Hash类型的Key，它的成员数量虽然只有1,000个但这些成员的Value（值）总大小为100 MB
    
那么如何判断元素的大小呢？redis也给我们提供了命令

```bash
127.0.0.1:6379>MEMORY USAGE name     # 可以采用memory usage指令查看指定key及其value占用大小 
(integer) 104 
127.0.0.1:6379>set name aaaaaaaaaaaaaaaaaaaaaaaaaaa 
OK 
127.0.0.1:6379>MEMORY USAGE name 
(integer)89 
127.0.0.1:6379>set name jack 
OK 
127.0.0.1:6379>MEMORY USAGE name # 但是一般不推荐使用memory指令，因此这个指令对cpu的使用率是比较高的 
(integer)54 
127.0.0.1:6379>STRLEN name 
(integer)4 
127.0.0.1:6379>LPUSH 12 m1 m2  # 实际开发中 一般来说我们只需要衡量值或者值的个数就行了
(integer)2 
127.0.0.1:6379>LLEN 12 
(integer) 2 
127.0.0.1:6379> 
```

推荐值：
- 单个key的value小于10KB
- 对于集合类型的key，建议元素数量小于1000
 
**BigKey的危害**

- 网络阻塞
    - 对BigKey执行读请求时，少量的QPS就可能导致带宽使用率被占满，导致Redis实例，乃至所在物理机变慢
- 数据倾斜
    - BigKey所在的Redis实例内存使用率远超其他实例，无法使数据分片的内存资源达到均衡
- Redis阻塞
    - 对元素较多的hash、list、zset等做运算会耗时较旧，使主线程被阻塞
- CPU压力
    - 对BigKey的数据序列化和反序列化会导致CPU的使用率飙升，影响Redis实例和本机其它应用


**如何发现BigKey**

- `redis-cli --bigkeys`
	利用redis-cli提供的--bigkeys参数，可以遍历分析所有key，并返回Key的整体统计信息与**每个数据的Top1**的`big key`
	命令：`redis-cli -a 密码 --bigkeys`

	```bash
	redis-cli --bigkeys
	# Scanning the entire keyspace to find biggest keys as well as
	# average sizes per key type.  You can use -i 0.1 to sleep 0.1 sec
	# per 100 SCAN commands (not usually needed).
	
	[00.00%] Biggest string found so far '"num"' with 3 bytes
	[00.00%] Biggest string found so far '"item:stock:id:10004"' with 37 bytes
	[00.00%] Biggest string found so far '"item:stock:id:10005"' with 39 bytes
	[00.00%] Biggest string found so far '"item:id:10004"' with 467 bytes
	
	-------- summary -------
	
	Sampled 12 keys in the keyspace!
	Total key length in bytes is 167 (avg len 13.92)
	
	Biggest string found '"item:id:10004"' has 467 bytes
	
	0 lists with 0 items (00.00% of keys, avg size 0.00)
	0 hashs with 0 fields (00.00% of keys, avg size 0.00)
	12 strings with 2384 bytes (100.00% of keys, avg size 198.67)
	0 streams with 0 entries (00.00% of keys, avg size 0.00)
	0 sets with 0 members (00.00% of keys, avg size 0.00)
	0 zsets with 0 members (00.00% of keys, avg size 0.00)
	```

- scan扫描
	- `KEYS *` 是一个简单的命令，用于返回匹配给定模式的所有键。当你执行 `KEYS *` 命令时，Redis 会阻塞所有其他操作，直到返回满足条件的键列表。这个命令在处理大量键或者键空间较大时可能会对性能产生负面影响.
	- `SCAN` 命令是 Redis 提供的一个更高效和安全的键迭代方式。它通过使用游标（cursor）来完成迭代，每次执行 SCAN 命令只返回一小部分键的数据，并附带下一个游标值，以便你可以继续从上次停止的地方继续迭代。
	 自己编程，利用scan扫描Redis中的所有key，利用strlen、hlen等命令判断key的长度（此处不建议使用MEMORY USAGE）

	```bash
	127.0.0.1:6379>scan count 2 
	1) "3" # 这是游标
	2)  1) "name" 
		2) "num"
	127.0.0.1:6379> scan 3 count 2 
	1) "0"  # 0代表已经扫描完整
	2) 1)"12" 
	127.0.0.1:6379> 
	```

	```java
	public class JedisTest {
	    private Jedis jedis;
	
	    @BeforeEach
	    void setUp() {
	        // 1.建立连接
	        // jedis = new Jedis("192.168.150.101", 6379);
	        jedis = JedisConnectionFactory.getJedis();
	        // 2.设置密码
	        jedis.auth("123321");
	        // 3.选择库
	        jedis.select(0);
	    }
	
	    final static int STR_MAX_LEN = 10 * 1024;
	    final static int HASH_MAX_LEN = 500;
	
	    @Test
	    void testScan() {
	        int maxLen = 0;
	        long len = 0;
	
	        String cursor = "0";
	        do {
	            // 扫描并获取一部分key
	            ScanResult<String> result = jedis.scan(cursor);
	            // 记录cursor
	            cursor = result.getCursor();
	            List<String> list = result.getResult();
	            if (list == null || list.isEmpty()) {
	                break;
	            }
	            // 遍历
	            for (String key : list) {
	                // 判断key的类型
	                String type = jedis.type(key);
	                switch (type) {
	                    case "string":
	                        len = jedis.strlen(key);
	                        maxLen = STR_MAX_LEN;
	                        break;
	                    case "hash":
	                        len = jedis.hlen(key);
	                        maxLen = HASH_MAX_LEN;
	                        break;
	                    case "list":
	                        len = jedis.llen(key);
	                        maxLen = HASH_MAX_LEN;
	                        break;
	                    case "set":
	                        len = jedis.scard(key);
	                        maxLen = HASH_MAX_LEN;
	                        break;
	                    case "zset":
	                        len = jedis.zcard(key);
	                        maxLen = HASH_MAX_LEN;
	                        break;
	                    default:
	                        break;
	                }
	                if (len >= maxLen) {
	                    System.out.printf("Found big key : %s, type: %s, length or size: %d %n", key, type, len);
	                }
	            }
	        } while (!cursor.equals("0"));
	    }
	    
	    @AfterEach
	    void tearDown() {
	        if (jedis != null) {
	            jedis.close();
	        }
	    }
	
	}
	```

- 第三方工具

	- 利用第三方工具，如 Redis-Rdb-Tools 分析RDB快照文件，全面分析内存使用情况
	- [https://github.com/sripathikrishnan/redis-rdb-tools](https://github.com/sripathikrishnan/redis-rdb-tools)
    
- 网络监控
	- 自定义工具，监控进出Redis的网络数据，超出预警值时主动告警
	- 一般阿里云搭建的云服务器就有相关监控页面
	- <img src="https://article.biliimg.com/bfs/article/39a20529a1ea6c62995e60b7d20c12c200fd0530.png" alt="image.png" style="zoom:70%;" />

##### 如何删除BigKey

BigKey内存占用较多，即便时**删除这样的key也需要耗费很长时间**，导致Redis主线程阻塞，引发一系列问题。
- redis 3.0 及以下版本
    - 如果是集合类型，则**遍历**BigKey的元素，先**逐个删除子元素**，最后删除BigKey

<img src="https://article.biliimg.com/bfs/article/e3bfd7bc602bc85bb3553914422dc51184abb966.png" alt="image-20220521140621204" style="zoom:67%;" />

- Redis 4.0以后
    - Redis在4.0后提供了**异步删除**的命令：`unlink`


#### 恰当的数据类型

例1：比如存储一个User对象，我们有三种存储方式：

方式一：json字符串

| user:1 | {"name": "Jack", "age": 21} |
| :----: | :-------------------------: |
优点：实现简单粗暴
缺点：数据耦合，不够灵活(假如我只想修改用户的name属性,要改整个user:1的key)

方式二：字段打散

| user:1:name | Jack |
| :---------: | :--: |
| user:1:age  |  21  |
优点：可以灵活访问对象任意字段
缺点：**占用空间大**、没办法做统一控制

方式三：hash（推荐）


<table>
	<tr>
		<td rowspan="2">user:1</td>
        <td>name</td>
        <td>jack</td>
	</tr>
	<tr>
		<td>age</td>
		<td>21</td>
	</tr>
</table>

优点：底层使用`ziplist`，空间占用小，可以灵活访问对象的任意字段
缺点：代码相对复杂

假如有hash类型的key，其中有100万对field和value，field是自增id，这个key存在什么问题？如何优化？

<table>
	<tr style="color:red">
		<td>key</td>
        <td>field</td>
        <td>value</td>
	</tr>
	<tr>
		<td rowspan="3">someKey</td>
		<td>id:0</td>
        <td>value0</td>
	</tr>
    <tr>
		<td>.....</td>
        <td>.....</td>
	</tr>
    <tr>
        <td>id:999999</td>
        <td>value999999</td>
    </tr>
</table>

存在的问题：
- hash的entry数量超过500时，会使用哈希表而不是`ZipList`，内存占用较多

```bash
127.0.0.1:6379> info memory 
# Memory 
used_memory:65248656 
used_memory_human: 62.23M 
used_memory_rss:78213120 
used_memory_rss_human:74.59M 
```

- 可以通过hash-max-ziplist-entries配置entry上限。但是如果entry过多就会导致BigKey问题

可以通过`config get hash-max-ziplist-entries`命令配置entry上限
```bash
127.0.0.1:6379> config get hash-max-ziplist-entries 
1) "hash-max-ziplist-entries"
2) "512"
127.0.0.1:6379> config set hash-max-ziplist-entries 1000
OK
127.0.0.1:6379> config get hash-max-ziplist-entries
1) "hash-max-ziplist-entries"
2) "1000"
```


**方案一:拆分为string类型**

<table>
	<tr style="color:red">
		<td>key</td>
        <td>value</td>
	</tr>
	<tr>
		<td>id:0</td>
        <td>value0</td>
	</tr>
    <tr>
		<td>.....</td>
        <td>.....</td>
	</tr>
    <tr>
        <td>id:999999</td>
        <td>value999999</td>
    </tr>
</table>

存在的问题：
- string结构底层**没有太多内存优化**，内存占用较多

```bash
127.0.0.1:6379> info memory 
# Memory 
used_memory:81304160 
used_memory_human:77.54M 
used_memory_rss:90193920 
used_memory_rss_human:86.02M 
used_memory_peak:81325168 
```

- 想要批量获取这些数据比较麻烦

**方案二**

拆分为小的hash，将 `id / 100` 作为key， 将`id % 100` 作为field，这样每100个元素为一个Hash

<table>
	<tr style="color:red">
		<td>key</td>
        <td>field</td>
        <td>value</td>
	</tr>
	<tr>
        <td rowspan="3">key:0</td>
		<td>id:00</td>
        <td>value0</td>
	</tr>
    <tr>
		<td>.....</td>
        <td>.....</td>
	</tr>
    <tr>
        <td>id:99</td>
        <td>value99</td>
    </tr>
    <tr>
        <td rowspan="3">key:1</td>
		<td>id:00</td>
        <td>value100</td>
	</tr>
    <tr>
		<td>.....</td>
        <td>.....</td>
	</tr>
    <tr>
        <td>id:99</td>
        <td>value199</td>
    </tr>
    <tr>
    	<td colspan="3">....</td>
    </tr>
    <tr>
        <td rowspan="3">key:9999</td>
		<td>id:00</td>
        <td>value999900</td>
	</tr>
    <tr>
		<td>.....</td>
        <td>.....</td>
	</tr>
    <tr>
        <td>id:99</td>
        <td>value999999</td>
    </tr>
</table>

```bash
127.0.0.1:6379> info memory 
# Memory 
used_memory:25643232 
used_memory_human:24.46M 
used_memory_rss:34988032 
used_memory_rss_human:33.37M 
```


##### 总结

- Key的最佳实践
    - 固定格式：`[业务名]:[数据名]:[id]`
    - 足够简短：不超过44字节
    - 不包含特殊字符
- Value的最佳实践：
    - 合理的拆分数据，拒绝BigKey
    - 选择合适数据结构
    - Hash结构的entry数量不要超过1000
    - 设置合理的超时时间

### 批处理优化
#### Pipeline

我们的客户端与redis服务器是这样交互的

- 单个命令的执行流程
	<img src="https://article.biliimg.com/bfs/article/84b60c534a98a4cd7d15a1073bb47dbe8b837b6d.png" alt="image-20220521151459880" style="zoom:67%;" />
- N条命令的执行流程

	<img src="https://article.biliimg.com/bfs/article/a5f314096fa48b0a62e7d61917ec7d91611f176e.png" alt="image-20220521151524621" style="zoom:67%;" />
- redis处理指令是很快的，主要花费的时候在于网络传输。于是乎很容易想到将多条指令批量的传输给redis

	<img src="https://article.biliimg.com/bfs/article/1f546d9d9dfe694414e559323b9a88ed5b1c197d.png" alt="image-20220521151902080" style="zoom:67%;" />

##### MSet

Redis提供了很多Mxxx这样的命令，可以实现批量插入数据，例如：

- mset
- hmset

利用mset批量插入10万条数据

```java
@Test
void testMxx() {
    String[] arr = new String[2000];
    int j;
    long b = System.currentTimeMillis();
    for (int i = 1; i <= 100000; i++) {
        j = (i % 1000) << 1;
        arr[j] = "test:key_" + i;
        arr[j + 1] = "value_" + i;
        if (j == 0) {
            jedis.mset(arr);
        }
    }
    long e = System.currentTimeMillis();
    System.out.println("time: " + (e - b));
}
```

##### Pipeline

MSET虽然可以批处理，但是却只能**操作部分数据类型**(set和list都限制一个key)，因此如果有对**复杂数据类型**的批处理需要，建议使用Pipeline

```java
@Test
void testPipeline() {
    // 创建管道
    Pipeline pipeline = jedis.pipelined();
    long b = System.currentTimeMillis();
    for (int i = 1; i <= 100000; i++) {
        // 放入命令到管道
        pipeline.set("test:key_" + i, "value_" + i);
        if (i % 1000 == 0) {
            // 每放入1000条命令，批量执行
            pipeline.sync();
        }
    }
    long e = System.currentTimeMillis();
    System.out.println("time: " + (e - b));
}
```



#### 集群下的批处理
如MSET或Pipeline这样的批处理需要在一次请求中携带多条命令，而此时如果Redis是一个集群，那批处理命令的多个key必须落在一个插槽中，否则就会导致执行失败。
大家可以想一想这样的要求其实很难实现，因为我们在批处理时，可能一次要插入很多条数据，这些数据很有可能不会都落在相同的节点上，这就会导致报错了

```bash
127.0.0.1:7001>mset name jack age 21 sex male
(error) CROSSSLOT Keys in request don't hash to the same slot
```



|          | 串行命令              | 串行slot                                                   |  井行slot                                                    |  hash_tag                               |
|:--------:|:-----------------:|:--------------------------------------------------------:|:----------------------------------------------------------:|:---------------------------------------:|
| 实现思路     | for循环遍历，依次执行每个命令  |在客户端计算每个key的 slot,将slot一致分为一组，每组都利用Pipeline 批处理。<font color=#ff0000>串行</font>执行各组命令|在客户端计算每个key的slot,将slot一致分为一组，每组都利用Pipeline 批处理。<font color=#ff0000>并行</font>执行各组命令|  将所有key设置相同的 hash_tag,则所有key 的slot一定相同  |
|  耗时      | N次网络耗时+N次命令耗时     |m次网络耗时+N次命令耗时 m=key的slot个数|1次网络耗时+N次命令耗时|                        1次网络耗时+N次命令耗时    |
|  优点      |  实现简单             |耗时较短|  耗时非常短                                                     |  耗时非常短、实现简单                             |
|  缺点      |  耗时非常久            |  实现稍复杂 slot越多，耗时越久                                       |实现复杂|容易出现**数据倾斜**|

- 第一种方案：串行执行，所以这种方式没有什么意义，当然，执行起来就很简单了，缺点就是耗时过久。
- 第二种方案：串行slot，简单来说，就是执行前，客户端先计算一下对应的key的slot，一样slot的key就放到一个组里边，不同的，就放到不同的组里边，然后对每个组执行pipeline的批处理，他就能<font color=#ff0000>串行执行</font>各个组的命令，这种做法比第一种方法耗时要少，但是缺点呢，相对来说复杂一点，所以这种方案还需要优化一下
- 第三种方案：并行slot，相较于第二种方案，在分组完成后串行执行，第三种方案，就变成了<font color=#ff0000>并行执行各个命令</font>，所以他的耗时就非常短，但是实现呢，也更加复杂。
- 第四种：hash_tag，redis计算key的slot的时候，其实是根据key的有效部分来计算的，通过这种方式就能一次处理所有的key，这种方式耗时最短，实现也简单，但是如果通过操作key的有效部分，那么就会导致所有的key都落在一个节点上，产生数据倾斜的问题，所以我们推荐使用第三种方式。


**串行化执行代码实践**

```java
    @Test
    void testMSet() {
        jedisCluster.mset("name", "Jack", "age", "21", "sex", "male");

    }
    /*
    redis.clients.jedis.exceptions.JedisClusterOperationException: No way to dispatch this command to Redis Cluster because keys have different slots.
    */
```



```java
@Test
    void testMSet2() {
        Map<String, String> map = new HashMap<>(3);
        map.put("name", "Jack");
        map.put("age", "21");
        map.put("sex", "Male");
        //对Map数据进行分组。根据相同的slot放在一个分组
        //key就是slot，value就是一个组
        Map<Integer, List<Map.Entry<String, String>>> result = map.entrySet()
                .stream()
                .collect(Collectors.groupingBy(
                        entry -> ClusterSlotHashUtil.calculateSlot(entry.getKey())) //通过 ClusterSlotHashUtil.calculateSlot(entry.getKey())为每个键执行 calculateSlot(...) 方法，该方法计算了键对应在Redis Cluster中的槽（slot）
                );
        //串行的去执行mset的逻辑
        for (List<Map.Entry<String, String>> list : result.values()) {
            String[] arr = new String[list.size() * 2];
            int j = 0;
            for (int i = 0; i < list.size(); i++) {
                j = i<<2;
                Map.Entry<String, String> e = list.get(0);
                arr[j] = e.getKey();
                arr[j + 1] = e.getValue();
            }
            jedisCluster.mset(arr);
        }
    }

```


Spring集群环境下批处理代码
```java
   @Test
    void testMSetInCluster() {
        Map<String, String> map = new HashMap<>(3);
        map.put("name", "Rose");
        map.put("age", "21");
        map.put("sex", "Female");
        stringRedisTemplate.opsForValue().multiSet(map);


        List<String> strings = stringRedisTemplate.opsForValue().multiGet(Arrays.asList("name", "age", "sex"));
        strings.forEach(System.out::println);

    }
```

**原理分析**

在`RedisAdvancedClusterAsyncCommandsImpl` 类中
首先根据`slotHash`算出来一个partitioned的map，map中的key就是slot，而他的value就是对应的对应相同slot的key对应的数据
通过 `RedisFuture<String> mset = super.mset(op)`;进行异步的消息发送

```java
@Override
public RedisFuture<String> mset(Map<K, V> map) {

    Map<Integer, List<K>> partitioned = SlotHash.partition(codec, map.keySet());

    if (partitioned.size() < 2) {
        return super.mset(map);
    }

    Map<Integer, RedisFuture<String>> executions = new HashMap<>();

    for (Map.Entry<Integer, List<K>> entry : partitioned.entrySet()) {

        Map<K, V> op = new HashMap<>();
        entry.getValue().forEach(k -> op.put(k, map.get(k)));

        RedisFuture<String> mset = super.mset(op);
        executions.put(entry.getKey(), mset);
    }

    return MultiNodeExecution.firstOfAsync(executions);
}
```

### 服务器端优化-持久化配置

Redis的持久化虽然可以保证数据安全，但也会带来很多额外的开销，因此持久化请遵循下列建议：

* 用来做**缓存的Redis实例**尽量不要**开启持久化功能**
* 建议关闭RDB持久化功能，**使用AOF持久化**
* 利用脚本定期在**slave节点做RDB**，实现数据备份
* 设置合理的rewrite阈值，避免**频繁的bgrewrite**
* 配置`no-appendfsync-on-rewrite = yes`，禁止在rewrite期间做aof，避免因AOF引起的阻塞
* 部署有关建议：
  * Redis实例的物理机要预留足够内存，应对fork和rewrite
  * 单个Redis实例内存上限不要太大，例如4G或8G。可以加快fork的速度、减少主从同步、数据迁移压力
  * 不要与CPU密集型应用部署在一起
  * 不要与高硬盘负载应用一起部署。例如：数据库、消息队列


<img src="https://article.biliimg.com/bfs/article/f8411057031e6b71783f9774f7d1ec46ccac5c0a.png" alt="image.png" style="zoom:50%;" />


### 服务器端优化

#### 慢查询优化
##### 什么是慢查询

并不是很慢的查询才是慢查询，而是：在Redis执行时**耗时超过某个阈值的命令**，称为慢查询。
慢查询的危害：由于Redis是**单线程的**，所以当客户端发出指令后，他们都会进入到**redis底层的queue来执行**，如果此时有一些慢查询的数据，就会导致大量**请求阻塞**，从而引起报错，所以我们需要解决慢查询问题。

<img src="https://article.biliimg.com/bfs/article/c54a9d47cd8cf8422f73045b38961e65ce3b1164.png" style="zoom:67%;" />
慢查询的阈值可以通过配置指定：
`slowlog-log-slower-than`：慢查询阈值，单位是微秒。默认是10000，建议1000
慢查询会被放入慢查询日志中，日志的长度有上限，可以通过配置指定：
`slowlog-max-len`：慢查询日志（本质是一个队列）的长度。默认是128，建议1000

```bash
127.0.0.1:6379>config get slowlog-max-len 
1) "slowlog-max-len" 
2) "128" 
127.0.0.1:6379>config get slowlog-log-slower-than 
1) "slowlog-log-slower-than" 
2) "10000" 
```

修改这两个配置可以使用：`config set`命令：
```bash
127.0.0.1:6379>config set slowlog-log-slower-than 1000 
OK 
127.0.0.1:6379>config get slowlog-log-slower-than 
1)"slowlog-log-slower-than" 
2)"1000" 
```

##### 如何查看慢查询

知道了以上内容之后，那么咱们如何去查看慢查询日志列表呢：

* `slowlog len`：查询慢查询日志长度
* `slowlog get [n]`：读取n条慢查询日志
* `slowlog reset`：清空慢查询列表

<img src="https://article.biliimg.com/bfs/article/d443eeab04e1f9b9720d5a421500e9ffc4c25048.png" style="zoom:67%;" />

#### 命令及安全配置

安全可以说是服务器端一个非常重要的话题，如果安全出现了问题，那么一旦这个漏洞被一些坏人知道了之后，并且进行攻击，那么这就会给咱们的系统带来很多的损失，所以我们这节课就来解决这个问题。

Redis会绑定在0.0.0.0:6379，这样将会将Redis服务暴露到公网上，而Redis如果没有做身份认证，会出现严重的安全漏洞.
漏洞重现方式：https://cloud.tencent.com/developer/article/1039000

为什么会出现不需要密码也能够登录呢，主要是Redis考虑到每次登录都比较麻烦，所以Redis就有一种ssh免秘钥登录的方式，生成一对公钥和私钥，私钥放在本地，公钥放在redis端，当我们登录时服务器，再登录时候，他会去解析公钥和私钥，如果没有问题，则不需要利用redis的登录也能访问，这种做法本身也很常见，但是这里有一个前提，前提就是公钥必须保存在服务器上，才行，但是Redis的漏洞在于在不登录的情况下，也能把秘钥送到Linux服务器，从而产生漏洞

漏洞出现的核心的原因有以下几点：

* Redis未设置密码
* 利用了Redis的config set命令动态修改Redis配置
* 使用了Root账号权限启动Redis

所以：如何解决呢？我们可以采用如下几种方案

为了避免这样的漏洞，这里给出一些建议：

* Redis一定要设置密码
* 禁止线上使用下面命令：`keys、flushall、flushdb、config set`等命令。可以利用`rename-command`(原理是修改实际的命令名称)禁用。
* `bind`：限制网卡，禁止外网网卡访问
* 开启防火墙
* 不要使用Root账户启动Redis
* 尽量不是有默认的端口


#### Redis内存划分和内存配置

当Redis内存不足时，可能导致Key频繁被删除、响应时间变长、QPS不稳定等问题。当**内存使用率达到90%以上**时就需要我们警惕，并快速定位到内存占用的原因。

- **有关碎片问题分析**
	Redis底层分配并不是这个key有多大，他就会分配多大，而是有他自己的分配策略，比如8,16,20等等，<font color=#ff0000>假定当前key只需要10个字节，此时分配8肯定不够，那么他就会分配16个字节，多出来的6个字节就不能被使用</font>，这就是我们常说的碎片问题,重启reids就会清理这些redis,所以要定期清理redis
- **进程内存问题分析：**
	这片内存，通常我们都可以忽略不计
- **缓冲区内存问题分析：**
	一般包括客户端缓冲区、AOF缓冲区、复制缓冲区等。客户端缓冲区又包括输入缓冲区和输出缓冲区两种。这部分内存占用波动较大，所以这片内存也是我们需要重点分析的内存问题。

| **内存占用**   | **说明**                                                                             |
|:----------:|:----------------------------------------------------------------------------------:|
|  数据内存      |是Redis最主要的部分，存储Redis的键值信息。主要问题是**BigKey**问题、内存碎片问题|
|  进程内存      |Redis主进程本身运⾏肯定需要占⽤内存，如**代码、常量池**等等；这部分内存⼤约⼏兆，在⼤多数⽣产环境中与Redis数据占⽤的内存相⽐可以忽略。|
|  缓冲区内存     |  一般包括客户端缓冲区、AOF缓冲区、复制缓冲区等。客户端缓冲区又包括输入缓冲区和输出缓冲区两种。这部分内存占用波动较大，不当使用BigKey，可能导致内存溢出。  |  

Redis提供了一些命令，可以查看到Redis目前的内存分配状态： 
- info memory 

	```bash
	#  分析各种内存的占用情况
	127.0.0.1:6379> info memory
	# Memory
	used_memory:917160
	used_memory_human:895.66K
	used_memory_rss:4939776
	used_memory_rss_human:4.71M
	used_memory_peak:18010264
	used_memory_peak_human:17.18M
	used_memory_peak_perc:5.09%
	used_memory_overhead:868496
	used_memory_startup:865848
	used_memory_dataset:48664
	used_memory_dataset_perc:94.84%
	allocator_allocated:1244328
	allocator_active:3993600
	allocator_resident:6758400
	total_system_memory:1019355136
	total_system_memory_human:972.13M
	```

- memory xxx 
	- `MEMORY USAGE key`：返回指定key的内存占用量，以字节为单位。
	- `MEMORY STATS`：返回Redis实例中所有key的内存占用的总和和平均值。
	- `MEMORY DOCTOR`：用于检测和诊断内存问题，显示可能导致内存泄漏或过度分配的key和数据结构。

	```bash
	127.0.0.1:6379> MEMORY stats
	 1) "peak.allocated"
	 2) (integer) 18010264
	 3) "total.allocated"
	 4) (integer) 1015792
	 5) "startup.allocated"
	 6) (integer) 865848
	 7) "replication.backlog"
	 8) (integer) 0
	 9) "clients.slaves"
	10) (integer) 0
	11) "clients.normal"
	12) (integer) 1928
	13) "cluster.links"
	14) (integer) 0
	15) "aof.buffer"
	16) (integer) 8
	17) "lua.caches"
	18) (integer) 0
	```

[[零散知识点#redis内存说明|redis内存说明]]

内存缓冲区常见的有三种： 
- **复制缓冲区**：主从复制的`repl_backlog_buf`,如果太小可能导致频繁的[[redis#全量同步|全量复制]]，影响性能。通过`repl-backlog- size`来设置，默认1mb 
- **AOF缓冲区**：AOF刷盘之前的缓存区域(用于存这刷盘间隔的命令)，AOF执行rewrite的缓冲区。无法设置容量上限 
- **客户端缓冲区**：分为输入缓冲区(**客户端发命令给redis,redis要缓存这些命令**)和输出缓冲区(**redis查完将结果返还给客户端,要放到这个缓存区**)，输入缓冲区最大1G且不能设置(**如果超过了这个空间，redis会直接断开**)。输出缓冲区可以设置 
	- `client-output-buffer-limit <class> <hard limit> <soft limit> <soft seconds>`
		- `<class>` 客户端类型 
			- `normal`:普通客户端 
			- `replica`:主从复制客户端 
			- `pubsub`:PubSub客户端 
		- `<hard limit>`:表示客户端输出缓冲区的[[零散知识点#硬限制|硬限制]]，以字节为单位。当达到这个限制时，Redis将停止向该客户端发送数据。
		- `<soft limit>`:表示客户端输出缓冲区的[[零散知识点#软限制|软限制]]，以字节为单位。当达到这个限制时，Redis将开始慢慢地限制发送给该客户端的数据速率。
		- `<soft seconds>`：表示在达到软限制之后，Redis将会限制发送给该客户端的数据的时间长度，以秒为单位。在这段时间内，Redis会逐渐减少发送给该客户端的数据数量，以避免突然堆积太多未发送的数据。
		- 默认的配置如下： 
		```conf
		client-output-buffer-limit normal 0 0 0
		client-output-buffer-limit replica 256mb 64mb 60
		client-output-buffer-limit pubsub 32mb 8mb 60
		```

		- `INFO CLIENTS`: 这个命令用于获取当前连接到 Redis 服务器的客户端的信息。它返回一个关于每个客户端连接的详细信息列表，包括客户端的ID、IP地址、端口号、所使用的命令数量、创建时间、最后一次交互时间等。这个命令对于监控和诊断Redis服务器的连接状态非常有用。
		```bash
		127.0.0.1:6379> info clients
		# Clients
		connected_clients:4
		cluster_connections:0
		maxclients:10000
		client_recent_max_input_buffer:20480
		client_recent_max_output_buffer:0
		blocked_clients:0
		tracking_clients:0
		clients_in_timeout_table:0
		total_blocking_keys:0
		total_blocking_keys_on_nokey:0
		```
		- `CLIENT LIST`: 这个命令也用于获取与Redis服务器建立连接的客户端的信息。不同于`INFO CLIENTS`命令，它以一种更简单、易读的格式返回客户端连接的信息。返回的信息包括每个客户端的ID、IP地址、端口号、连接状态、数据库ID、所属命令、最后一次交互时间等。这个命令通常在需要快速查看连接状态时使用。
		```bash
		127.0.0.1:6379> client list
		id=5 addr=127.0.0.1:55084 laddr=127.0.0.1:6379 fd=9 name= age=3463 idle=0 flags=N db=0 sub=0 psub=0 ssub=0 multi=-1 qbuf=26 qbuf-free=20448 argv-mem=10 multi-mem=0 rbs=1024 rbp=0 obl=0 oll=0 omem=0 tot-mem=22426 events=r cmd=client|list user=default redir=-1 resp=2 lib-name= lib-ver=
		id=6 addr=192.168.153.128:39130 laddr=172.17.0.2:6379 fd=10 name= age=1837 idle=47 flags=N db=0 sub=0 psub=0 ssub=0 multi=-1 qbuf=0 qbuf-free=0 argv-mem=0 multi-mem=0 rbs=1024 rbp=0 obl=0 oll=0 omem=0 tot-mem=1928 events=r cmd=info user=default redir=-1 resp=2 lib-name= lib-ver=
		id=8 addr=192.168.153.1:61643 laddr=172.17.0.2:6379 fd=12 name= age=1799 idle=1799 flags=P db=0 sub=0 psub=1 ssub=0 multi=-1 qbuf=0 qbuf-free=0 argv-mem=0 multi-mem=0 rbs=1024 rbp=0 obl=0 oll=0 omem=0 tot-mem=1984 events=r cmd=psubscribe user=default redir=-1 resp=2 lib-name= lib-ver=
		id=9 addr=192.168.153.1:61644 laddr=172.17.0.2:6379 fd=13 name= age=1799 idle=3 flags=N db=0 sub=0 psub=0 ssub=0 multi=-1 qbuf=0 qbuf-free=0 argv-mem=0 multi-mem=0 rbs=1024 rbp=0 obl=0 oll=0 omem=0 tot-mem=1928 events=r cmd=info user=default redir=-1 resp=2 lib-name= lib-ver=
		```


#### 集群还是主从

在Redis的默认配置中，如果发现**任意一个插槽**不可用，则**整个集群**都会停止对外服务： 

```conf
# However sometimes you want the subset of the cluster which is working,
# to continue to accept queries for the part of the key space that is still
# covered. In order to do so, just set the cluster-require-full-coverage
# option to no.
#
# cluster-require-full-coverage yes
```

大家可以设想一下，如果有几个slot不能使用，那么此时整个集群都不能用了，我们在开发中，其实最重要的是可用性，所以需要把如下配置修改成no，即有slot不能使用时，我们的redis集群还是可以对外提供服务.为了保证高可用特性，这里建议将 `cluster-require-full-coverage`配置为false 

```bash
127.0.0.1:7001> redis-cli -p 7001 shutdown
[root@localhost tmp]# redis-cli -p 7001 shutdown
[root@localhost tmp]# redis-cli -p 8002 shutdown
[root@localhost tmp]# redis-cli -p 8001 -c
127.0.0.1:8001> set name jack
(error) CLUSTERDOWN The cluster is down
```


**问题2、集群带宽问题**
集群节点之间会不断的**互相Ping**来确定集群中其它节点的状态。每次Ping携带的信息至少包括：
- 插槽信息
- 集群状态信息
集群中节点越多，集群状态**信息数据量也越大**，10个节点的相关信息可能达到1kb，此时每次集群互通需要的带宽会非常高，这样会导致集群中<font color=#ff0000>大量的带宽都会被ping信息所占用</font>，这是一个非常可怕的问题，所以我们需要去解决这样的问题

**解决途径：**
- **避免大集群**，集群节点数不要太多，最好少于1000，如果业务庞大，则建立多个集群。
- 避免在单个物理机中运行太多Redis实例
- 配置合适的`cluster-node-timeout`值

**问题3、命令的集群兼容性问题**
有关这个问题咱们已经探讨过了，当我们使用批处理的命令时，redis要求我们的key必须落在相同的slot上，然后大量的key同时操作时，是无法完成的，所以客户端必须要对这样的数据进行处理，这些方案我们之前已经探讨过了，所以不再这个地方赘述了。

**问题4、lua和事务的问题**
lua和事务都是要保证原子性问题，如果你的key不在一个节点，那么是无法保证lua的执行和事务的特性的，所以在集群模式是没有办法执行lua和事务的


> [!attention] 注意 
> 单体Redis(主从Redis)已经能达到**万级别的QPS**,并且也具备很强的高可用特性。如果主从能满足业务需求的情况下，尽量不搭建Redis集群。

# Redis原理篇 

## redis数据结构

### 动态字符串SDS 

我们都知道Redis中保存的Key是字符串，value往往是<font color=#ff0000>字符串</font>或者<font color=#ff0000>字符串的集</font>合。可见字符串是Redis中最常用的一种数据结构。
不过Redis没有直接使用C语言中的字符串，因为C语言字符串存在很多问题:
- 获取字符串长度的需要通过运算 (因为需要结束标志，需要计算)
- 非二进制安全 (如果里面有'`\0`')
- 不可修改(这个`char *`是保存在常量池的)

	```c
	// c语言，声明字符串 
	char* s="hello"
	// 本质是字符数组： {'h','e','l','l','o','\0'}
	```


Redis构建了一种新的字符串结构，称为简单**动态字符串**（`Simple Dynamic String`），简称SDS。 例如，我们执行命令：

```hash
127.0.0.1:6379> set name 虎哥 
OK 
```

那么Redis将在底层创建两个SDS，其中一个是包含“name”的**SDS**，另一个是包含“虎哥”的SDS。
Redis是C语言实现的，其中SDS是一个**结构体**，源码如下：

```c
struct __attribute__ ((__packed__)) sdshdr8 {
  uint8_t len; /* buf已保存的字符串字节数，不包含结束标示*/
  uint8_t alloc; /* buf申请的总的字节数，不包含结束标示*/ 
  unsigned char flags; /* 不同SDS的头类型，用来控制SDS的头大小 */
  char buf[];
}
```

```c
// 不同的SDS的头类型
#define SDS TYPE 5 0 
#define SDS TYPE 8 1 
#define SDS TYPE 16 2 
#define SDS TYPE 32 3 
#define SDS TYPE 64 4 
```

redis针对不同长度的key和value创建了不同的长度的SDS头类型

```c
struct __attribute__ ((__packed__)) sdshdr8 {
    uint8_t len; /* used */
    uint8_t alloc; /* excluding the header and null terminator */
    unsigned char flags; /* 3 lsb of type, 5 unused bits */
    char buf[];
};
struct __attribute__ ((__packed__)) sdshdr16 {
    uint16_t len; /* used */
    uint16_t alloc; /* excluding the header and null terminator */
    unsigned char flags; /* 3 lsb of type, 5 unused bits */
    char buf[];
};
struct __attribute__ ((__packed__)) sdshdr32 {
    uint32_t len; /* used */
    uint32_t alloc; /* excluding the header and null terminator */
    unsigned char flags; /* 3 lsb of type, 5 unused bits */
    char buf[];
};
struct __attribute__ ((__packed__)) sdshdr64 {
    uint64_t len; /* used */
    uint64_t alloc; /* excluding the header and null terminator */
    unsigned char flags; /* 3 lsb of type, 5 unused bits */
    char buf[];
};
```



> [!summary] 扩展：
> - `__attribute((aligned (n)))`：让所作用的结构成员对齐在n字节自然边界上。如果结构体中有成员的长度大于n，则按照最大成员的长度来对齐。
> - `__attribute__ ((packed))`：取消结构在编译过程中的优化对齐，按照实际占用字节数进行对齐。
> 
> 为什么redis不进行对齐操作呢？
> - SDS的指针并不是指向SDS的起始位置(len位置)，而是直接指向`buf[]`，使得SDS可以直接使用C语言`string.h`库中的某些函数，做到了兼容，十分nice~。
> - 如果不进行对齐填充，那么在获取当前 SDS 的类型时则只需要后退一步即可`flagsPointer = ((unsigned char*)s)-1`；
> - 相反，若进行对齐填充，由于**Padding的存在**，我们在不同的系统中不知道退多少才能获得flags，并且我们也不能将 sds 的指针指向flags，这样就无法兼容C语言的函数了，也不知道前进多少才能得到`buf[]`



例如，一个包含字符串“name”的sds结构如下：

<img src="https://article.biliimg.com/bfs/article/8914c4a3cb4546386e84a7d9fe5dc792e600ada1.png" alt="1653984648404" style="zoom:67%;" />
因为name是长度为4的字符串，所以len和alloc都是4，可以用sdshdr8存储，flags=1，后面就是`buf[]`

SDS之所以叫做**动态字符串**，是因为它具备**动态扩容**的能力，例如一个内容为“hi”的SDS：

<img src="https://article.biliimg.com/bfs/article/043bdb9efb34b32e5b301ddd4c5ea9b5e2109fd2.png" alt="1653984787383" style="zoom:67%;" />


假如我们要给SDS追加一段字符串“,Amy”，这里首先会申请新内存空间：
- 如果新字符串小于1M，则新空间为扩展后字符串长度的两倍+1；
- 如果新字符串大于1M，则新空间为扩展后字符串长度+1M+1。称为**内存预分配**。

<img src="https://article.biliimg.com/bfs/article/f5914d1d09762f85a270c06606a1115c1b83700d.png" alt="1653984822363" style="zoom:67%;" />


> [!tldr]+ 扩展
> - 之所以进行内存预分配是因为，linux是分用户空间和内核空间的，申请内存空间时，程序是无法直接操作硬件的，要跟内核空间进行交互，这个过程需要从**用户态进入到内核态**，然后申请内存，而这个过程是非常**消耗资源的**    
> - 主要是为了减少内存申请的次数
> 



> [!summary] 优点
> 1. 获取字符串长度的**时间复杂度为O(1)** 
> 2. 支持动态扩容 
> 3. 减少内存分配次数 
> 4. 二进制安全 

### IntSet 

IntSet是Redis中set集合的一种实现方式，基于**整数数组**来实现，并且具备**长度可变**、**有序**等特征。 
结构如下：

```c
typedef struct intset {
    uint32_t encoding; //编码方式，支持存放16位、32位、64位整数 
    uint32_t length; // 元素个数
    int8_t contents[]; // 整数数组，保存集ji'heji'he数据 
} intset;
```

为什么这里的contents事int8_t类型，只有1个字节 -127-127
这里contents==只是一个指针==，指向数组的起始地址，inset不依赖c语言的数组，这里的set的数值大小是由encoding属性决定

其中的`encoding`包含三种模式，表示存储的整数大小不同： 
```c
/* INTSET_ENC_INT16 < INTSET_ENC_INT32 < INTSET_ENC_INT64. */
#define INTSET_ENC_INT16 (sizeof(int16_t)) /* 2字节整数，范围类似java的short */ 
#define INTSET_ENC_INT32 (sizeof(int32_t)) /* 4字节整数，范围类似java的int */ 
#define INTSET_ENC_INT64 (sizeof(int64_t)) /* 8字节整数，范围类似java的long */ 
```

为了方便查找，Redis会将intset中所有的整数**按照升序**依次保存在contents数组中，结构如图：

<img src="https://article.biliimg.com/bfs/article/d616f13dacd25fc45397e6a282a6f2f372dbc009.png" alt="1653985149557" style="zoom:67%;" />

现在，数组中每个数字都在`int16_t`的范围内，因此采用的编码方式是`INTSET_ENC_INT16`，每部分占用的字节大小为： 
- `encoding`：4字节 
- `length`：4字节 
- `contents`：2字节(这里存的数是`int16_t`类型一个占2字节) * 3 = 6字节

5可以用1字节存储,这里为什么要统一编码规则?是为了方便角标去**寻址快速定位**到地址,因为统一了每个元素占的大小,就可以套上面的公式寻址

#### IntSet升级 
现在，假设有一个intset,元素为`{5,10,20}`,采用的编码是`INTSET_ENC_INT16`,则每个整数占2字节： 

<img src="https://article.biliimg.com/bfs/article/9d59820689ff0e077e915b88c029cd073bfbfbfd.png" alt="1653985197214" style="zoom:67%;" />
我们向该其中添加一个数字：50000,这个数字超出了`int16_t`的范围，intset会**自动升级编码方式**到合适的大小。 
以当前案例来说流程如下： 
1. 升级编码为`INTSET_ENC_INT32`,每个整数占4字节，并按照新的编码方式及元素个数扩容数组 
2. **倒序**(正序会覆盖,而倒序不会)依次将数组中的元素拷贝到**扩容后的正确位置**
3. 将待添加的元素放入数组末尾
4. 最后，将inset的encoding属性改为`INTSET_ENC_INT32`，将length属性改为4

<img src="https://article.biliimg.com/bfs/article/625bf0d392be08c3b561fc16358f64abdff60b54.png" style="zoom:67%;" />


```c
intset *intsetAdd(intset *is, int64_t value, uint8_t *success){ 
	uint8_t valenc = intsetValueEncoding(value);//获取当前值编码 
	uint32_tpos;//要插入的位置 
	if (success) * success = 1 ; 
	//判断编码是不是超过了当前intset的编码 
	if ( valenc > intrev32ifbe ( is - > encoding ) ) { 
	//超出编码，需要升级 
	return intsetUpgradeAndAdd(is,value); 
	} else { 
	//在当前intset中查找值与value一样的元素的角标pos 
	if (intsetSearch(is,value,&pos)){ // 这里是采用的二分查找
		if(success)*success=0;//如果找到了，则无需插入，直接结束并返回失败 
		return is; 
	} 
		//数组扩容 
		is = intsetResize(is,intrev32ifbe ( is - > length ) + 1 ) ; 
		//移动数组中pos之后的元素到pos+1,给新元素腾出空间 
	if (pos< intrev32ifbe(is - > length)) intsetMoveTail(is,pos,pos + 1); 
	//插入新元素 
	_intsetSet(is,pos,value); 
	//重置元素长度 
	is->length = intrev32ifbe(intrev32ifbe(is->length)+1); 
	return is; 
}
```


```c
static intset *intsetUpgradeAndAdd(intset *is, int64t value){ 
	//获取当前intset编码 
	uint8_t curenc = intrev32ifbe(is - > encoding ) ; 
	//获取新编码 
	uint8_t newenc = intsetValueEncoding(value) ; 
	//获取元素个数 
	int length = intrev32ifbe(is->length); 
	//判断新元素是大于0还是小于0,小于0插入队首、大于0插入队尾 
	int prepend = value < 0?1:0; 
	//重置编码为新编码 
	is->encoding = intrev32ifbe(newenc); 
	//重置数组大小 
	is = intsetResize(is,intrev32ifbe (is - > length ) + 1 ) ; 
	//倒序遍历，逐个搬运元素到新的位置，intsetGetEncoded按照旧编码方式查找旧元素 
	while(length--)//intsetSet按照新编码方式插入新元素 
		intsetSet(is,length+prepend,intsetGetEncoded(is,length,curenc)); 
	/*插入新元素，prepend决定是队首还是队尾*/ 
	if (prepend) 
		_intsetSet(is,O,value); 
	else 
		_intsetSet(is,intrev32ifbe(is->length),value); 
	//修改数组长度 
	is->length = intrev32ifbe(intrev32ifbe(is->length)+1); 
	return is; 
}
```

小总结：

`Intset`可以看做是特殊的整数数组，具备一些特点：
- Redis会确保Intset中的元素唯一、有序
- 具备类型升级机制，可以节省内存空间
- 底层采用二分查找方式来查询
### Dict 

我们知道Redis是一个键值型（Key-Value Pair）的数据库，我们可以根据键实现快速的增删改查。而键与值的映射关系正是通过Dict来实现的。 Dict由三部分组成，分别是：哈希表（DictHashTable）、哈希节点（DictEntry）、字典（Dict）

```c
typedef struct dictht{
  // entry数组 
  // 数组中保存的是指向entry的指针 
  dictEntry **table;
  // 哈希表大小 总是2^n
  unsigned long size; 
  // 哈希表大小的掩码,总等于size-1
  unsigned long sizemask;
  // entry个数
  unsigned long used; //>=size的 因为哈希表允许存储多个键值对映射到同一个桶（即哈希冲突)
}dictht;
```

```c
typedef struct dictEntry{
  void *key; // 键
  union{
    void *val;
    uint64_t u64;
    int64_t s64;
    double d;
  } v; // 值
  // 下一个Entry的指针
  struct dictEntry *next; //当哈希冲突时,用于指向下一个
}dictEntry;
```

当我们向Dict添加**键值对**时，Redis首先根据key计算出hash值(h),然后利用`h&sizemask`来计算元素应该存储到数 组中的哪个索引位置。  
掩码大小为$2^n-1$,二进制所有位都是1,与运算后是**余数部分**

组中的哪个索引位置。我们存储k1=v1,假设k1的哈希值h=1,则1&3=1,因此k1=v1要存储到数组角标1位置。 
下一个k2=v2,h=1,这时采用头插法,dictEntry里1这个桶指向新来的entry,而k2的next指向原先的桶1的entry
<img src="https://article.biliimg.com/bfs/article/84c75d5955283cfbcf9e16dc7c7c4222ac86ad4c.png" style="zoom:67%;" />

```c
typedef struct dict{
  dictType *type; // dict类型,内置不同的哈希函数
  void *privdata;  // 私有数据,在做特殊hash运算时用
  dictht ht[2];  // 一个Dict包含两个哈希表，其中一个是当前数据，另一个一般是空，rehash时使用 
  long rehashidx; // rehash的进度，-1表示未进行  
  int16_t pauserehash; // rehash是否暂停，1则暂停，O则继续
}dict;
```


<img src="https://article.biliimg.com/bfs/article/ca6179a7d32697f02bbceba21ce70e2f9a8875f9.png" style="zoom:67%;" />

#### Dict的扩容

Dict中的`HashTable`就是数组结合单向链表的实现，当集合中元素较多时，必然导致哈希冲突增多，链表过长，则查询效率会大大降低。 

Dict在每次新增键值对时都会检查负载因子（LoadFactor=used/size),满足以下两种情况时会触发哈希表扩容： 
- 哈希表的LoadFactor>=1,并且服务器没有执行`BGSAVE`或者`BGREWRITEAOF`等后台进程； 
- 哈希表的LoadFactor>5; 

```c
static int _dictExpandIfNeeded(dict *d)
{
    /* Incremental rehashing already in progress. Return. */
    if (dictIsRehashing(d)) return DICT_OK;
    /* If the hash table is empty expand it to the initial size. */
    if (d->ht[0].size == 0) return dictExpand(d, DICT_HT_INITIAL_SIZE);
	//当负载因子（used/size)达到1以上，并且当前没有进行bgrewrite等子进程操作 
	//或者负载因子超过5,则进行 dictExpand,也就是扩容 
    if (d->ht[0].used >= d->ht[0].size &&
        (dict_can_resize || d->ht[0].used/d->ht[0].size > dict_force_resize_ratio) &&dictTypeExpandAllowed(d))
    {
	    // 扩容大小为used + 1,底层会对扩容大小做判断，实际上找的是第一个大于等于 used+1 的 2^n 
        return dictExpand(d, d->ht[0].used + 1);
    }
    return DICT_OK;
}
```

Dict除了扩容以外，每次删除元素时，也会对负载因子做检查，当`LoadFactor<0.1`时，会做哈希表收缩： 

```c
// t _ hash.c # hashTypeDeleted() 
	if (dictDelete((dict*)o->ptr, field) == C_OK) {
		deleted = 1;
		// 删除成功后，检查是否需要重置Dict大小，如果需要则调用dictResize重置 
		/* Always check if the dictionary needs a resize after a delete. */
		if (htNeedsResize(o->ptr)) dictResize(o->ptr);
	}

```


```c
int htNeedsResize(dict *dict) {
    long long size, used;
	// 哈希表大小
    size = dictSlots(dict);
    // entry数量
    used = dictSize(dict);
    // size>4 (哈希表初始大小)并且 负载因子低于0.1
    return (size > DICT_HT_INITIAL_SIZE &&
            (used*100/size < HASHTABLE_MIN_FILL));
}
```

```c
int dictResize(dict *d)
{
    unsigned long minimal;
	//如果正在做bgsave或bgrewriteof或rehash,则返回错误 
    if (!dict_can_resize || dictIsRehashing(d)) return DICT_ERR;
    //获取used,也就是entry个数 
    minimal = d->ht[0].used;
    //如果used小于4,则重置为4 
    if (minimal < DICT_HT_INITIAL_SIZE)
        minimal = DICT_HT_INITIAL_SIZE;
    //重置大小为minimal,其实是第一个大于等于minimal的2^n 
    return dictExpand(d, minimal);
}
```

#### Dict的rehash

不管是扩容还是收缩，**必定会创建新的哈希表**，导致哈希表的size和sizemask变化，而key的查询与sizemask有关。因此必须**对哈希表中的每一个key重新计算索引**，插入新的哈希表，这个过程称为`rehash`。过程是这样的： 
- 计算新hash表的realeSize,值取决于当前要做的是扩容还是收缩： 
	- 如果是扩容，则新size为第一个大于等于`dict.ht[0].used+1`的$2^n$
	- 如果是收缩，则新size为第一个大于等于`dict.ht[0].used`的$2^n$(不得小于4)      sx
- 按照新的realeSize申请内存空间，创建`dictht`,并赋值给`dict.ht[1]` 
- 设置`dict.rehashidx=0`,标示开始rehash 
- 将`dict.ht[0]`中的**每一个dictEntry都rehash到**`dict.ht[1]` 
- 将`dict.ht[1]`赋值给`dict.ht[0]`,给`dict.ht[1]`初始化为空哈希表，释放原来的`dict.ht[0]`的内存 


Dict的`rehash`并不是一次性完成的。试想一下，如果Dict中包含**数百万的entry**,要在一次rehash完成，极有可能导致**主线程阻塞**。因此Dict的rehash是分多次、渐进式的完成，因此称为**渐进式rehash**。流程如下： 
- 计算新hash表的size,值取决于当前要做的是扩容还是收缩： 
	- 如果是扩容，则新size为第一个大于等于`dict.ht[0].used+1`的2n 
	- 如果是收缩，则新size为第一个大于等于`dict.ht[0].used`的2n(不得小于4) 
- 按照新的size申请内存空间，创建`dictht`,并赋值给`dict.ht[1]`
- 设置`dict.rehashidx=0`,标示开始rehash 
- **每次执行新增、查询、修改、删除操作时，都检查一下dict.rehashidx是否大于-1,如果是则将`dict.ht[0].table[rehashidx]`的entry链表rehash到`dict.ht[1]`,并且将rehashidx++。直至`dict.ht[0]`的所有数据都rehash到`dict.ht[1]` 
- 将`dict.ht[1]`赋值给`dict.ht[0]`,给`dict.ht[1]`初始化为空哈希表，释放原来的`dict.ht[0]`的内存 
- 在rehash过程中，新增操作，则直接写入`ht[1]`，查询、修改和删除则会在`dict.ht[0]`和`dict.ht[1]`依次查找并执行。这样可以确保`ht[0]`的数据只减不增，随着rehash最终为空

**整个过程可以描述成**：

<img src="https://article.biliimg.com/bfs/article/70346510c43a10f6d9f7479b81664b26b2e7d726.gif" alt="image.png" style="zoom:40%;" />


> [!summary] 小总结：
> Dict的结构：
> * 类似java的HashTable，底层是数组加链表来解决哈希冲突
> * Dict包含两个哈希表，`ht[0]`平常用，`ht[1]`用来rehash
> Dict的伸缩：
> * 当LoadFactor大于5或者LoadFactor大于1并且没有子进程任务时，Dict扩容
> * 当LoadFactor小于0.1时，Dict收缩
> * 扩容大小为第一个大于等于used + 1的$2^n$
> * 收缩大小为第一个大于等于used 的$2^n$
> * Dict采用渐进式rehash，每次访问Dict时执行一次rehash
> * rehash时`ht[0]`只减不增，新增操作只在`ht[1]`执行，其它操作在两个哈希表
> 


### ZipList 

ZipList 是一种特殊的 **“双端链表”** ，由一系列特殊编码的**连续内存块**组成。可以在任意一端进行压入/弹出操作, 并且该操作的时间复杂度为O(1)。

<img src="https://article.biliimg.com/bfs/article/72dd858ae38a93a18a173896e3cc95a49872e1bb.png" alt="1653985987327" style="zoom:67%;" />


|  **属性**    |  **类型**    |  **长度**  |  **用途**                                                                                |
|:----------:|:----------:|:--------:|:--------------------------------------------------------------------------------------:|
|zlbytes|uint32_t|  4 字节    |  记录整个压缩列表占用的内存字节数                                                                      |
|  zltail    |  uint32_t  |  4 字节    |  记录压缩列表表尾节点距离压缩列表的起始地址有多少字节，通过这个偏移量，可以确定表尾节点的地址。                                       |
|  zllen     |  uint16_t  |  2 字节    |记录了压缩列表包含的节点数量。 最大值为`UINT16_MAX`(65534)，如果超过这个值，此处会记录为65535，但节点的真实数量需要遍历整个压缩列表才能计算得出。|
|  entry     |  列表节点      |  不定      |压缩列表包含的各个节点，节点的长度由节点保存的内容决定。|
|  zlend     |  uint8_t   |  1 字节    |特殊值 0xFF （十进制255），用于标记压缩列表的末端。|

#### ZipListEntry

ZipList 中的Entry并不像普通链表那样**记录前后节点的指针**，因为记录两个指针要占用16个字节，浪费内存。而是采用了下面的结构：

<img src="https://article.biliimg.com/bfs/article/8bd4042201b911cd319e8c9d48095190c359e74f.png" style="zoom:67%;" />
* `previous_entry_length`：前一节点的长度，占1个或5个字节。
  * 如果前一节点的长度小于254字节，则采用1个字节来保存这个长度值
  * 如果前一节点的长度大于254字节，则采用5个字节来保存这个长度值，第一个字节为`0xfe`，后四个字节才是真实长度数据
* `encoding`：编码属性，记录`content`的**数据类型**（字符串还是整数）以及**长度**，占用1个、2个或5个字节
* `contents`：负责保存节点的数据，可以是字符串或整数

当前entry的地址加上自身长度就是下一个entry的地址,当前entry的地址减去`previous_entry_length`就是上一个entry的地址,这种方式来达到类似于**记录前后节点的指针**的作用,比起指针更省空间


> [!attention] 注意 
> - ZipList中所有**存储长度**的数值均采用小端字节序，即低位字节在前，高位字节在后。例如： 
> - 数值0×1234,采用[[零散知识点#大小端存储|小端字节]]序后实际存储值为：0×3412 

#### Encoding编码

ZipListEntry中的encoding编码分为字符串和整数两种：
字符串：如果encoding是以“00”、“01”或者“10”开头，则证明content是字符串
前两位用于表示数据类型,后面就是具体的**数据的长度**

|**编码** |**编码长度**|**字符串大小**|
|---|---|---|
|\|00pppppp\||1 bytes|<= 63 bytes|
|\|01pppppp\|qqqqqqqq\||2 bytes|<= 16383 bytes|
|\|10000000\|qqqqqqqq\|rrrrrrrr\|ssssssss\|tttttttt\||5 bytes|<= 4294967295 bytes|

例如，我们要保存字符串：“ab”和 “bc”

<img src="https://article.biliimg.com/bfs/article/3b7bcf3095e234380f7684954ed986d948ae0d66.png" alt="1653986172002" style="zoom:67%;" />


- ab长度为2,在1bytes长度范围内,加上数据类型,encoding=00000010,ab没有前置节点,所以previous_entry_length=0x00,ab实际内容转换成ASCII是01100001 01100010 ,整个Entry部分就是0x00 0x02 0x61 0x62
- bc的Entry就是0x04 0x02 0x62 0x63
- 然后计算zplist除了entry的其他结构的信息,这里计算长度都采用小端字节

ZipListEntry中的encoding编码分为字符串和整数两种：
* 整数：如果encoding是以“11”开始，则证明content是整数，且encoding固定只占用1个字节

|**编码**|**编码长度**|**整数类型**|
|---|---|---|
|11000000|1|int16_t（2 bytes）|
|11010000|1|int32_t（4 bytes）|
|11100000|1|int64_t（8 bytes）|
|11110000|1|24位有符整数(3 bytes)|
|11111110|1|8位有符整数(1 bytes)|
|1111xxxx|1|直接在xxxx位置保存数值，范围从0001~1101，减1后结果为实际值 0~12|
例如，一个ZipList中包含两个整数值：“2”和“5” 

<img src="https://article.biliimg.com/bfs/article/bed9be2455b48dd7e390625cad8b8f072efeea83.png" alt="1653986282879" style="zoom:67%;" />

<img src="https://article.biliimg.com/bfs/article/09d0825393b8112aceb5b30a716a6b3197ff4249.png" alt="1653986217182" style="zoom:67%;" />
#### ZipList的连锁更新问题

ZipList的每个Entry都包含`previous_entry_length`来记录上一个节点的大小，长度是1个或5个字节：
如果前一节点的长度小于254字节，则采用1个字节来保存这个长度值
如果前一节点的长度大于等于254字节，则采用5个字节来保存这个长度值，第一个字节为0xfe，后四个字节才是真实长度数据
现在，假设我们有N个连续的、长度为250~253字节之间的entry，因此entry的`previous_entry_length`属性用1个字节即可表示，如图所示：
但是插入一个254的字节后,所有的后面entry都会增大

<img src="https://article.biliimg.com/bfs/article/b622b42148e1caf0206d6f976018e507c1649790.png" alt="1653986328124" style="zoom:67%;" />



ZipList这种特殊情况下产生的连续多次空间扩展操作称之为**连锁更新**（Cascade Update）。新增、删除都可能导致连锁更新的发生。


**小总结：**

**ZipList特性：**
- 压缩列表的可以看做一种连续内存空间的"双向链表"
- 列表的节点之间不是通过指针连接，而是记录**上一节点和本节点长度来寻址**，内存占用较低
- 如果列表数据过多，导致链表过长，可能影响查询性能
- 增或删较大数据时有可能发生连续更新问题

### QuickList 


问题1：ZipList虽然节省内存，但申请内存**必须是连续空间**，如果**内存占用较多**，申请内存效率很低。怎么办？

​答：为了缓解这个问题，我们必须限制ZipList的长度和entry大小。

问题2：但是我们要存储大量数据，超出了ZipList最佳的上限该怎么办？

​答：我们可以创建多个**ZipList来分片存储数据**。

问题3：数据拆分后比较分散，**不方便管理和查找**，这多个ZipList如何建立联系？

​答：Redis在3.2版本引入了新的数据结构`QuickList`，它是一个**双端链表**，只不过链表中的每个节点都是一个**ZipLis**t。

<img src="https://article.biliimg.com/bfs/article/cc28195763366907b14f1401494df081ae49ce13.png" alt="image.png" style="zoom:60%;" />


为了避免QuickList中的每个ZipList中entry过多，Redis提供了一个配置项：`list-max-ziplist-size`来限制。
- 如果值为正，则代表ZipList的允许的entry个数的最大值
- 如果值为负，则代表ZipList的最大内存大小，分5种情况：
	* -1：每个ZipList的内存占用不能超过4kb
	* -2：每个ZipList的内存占用不能超过8kb
	* -3：每个ZipList的内存占用不能超过16kb
	* -4：每个ZipList的内存占用不能超过32kb
	* -5：每个ZipList的内存占用不能超过64kb

其默认值为 -2：

```bash
127.0.0.1:6379> config get list-max-ziplist-size
1) "list-max-ziplist-size"
2) "-2"
```


除了控制ZipList的大小，QuickList还可以对节点的ZipList做压缩。通过配置项list-compress-depth来控制。因为链表一般都是从首尾访问较多，所以首尾是不压缩的。这个参数是控制首尾不压缩的节点个数： 
- 0:特殊值，代表不压缩 
- 1:标示QuickList的首尾各有1个节点不压缩，中间节点压缩 
- 2:标示QuickList的首尾各有2个节点不压缩，中间节点压缩 
- 以此类推 
默认值： 
```bash
127.0.0.1:6379> config get list-compress-depth
1) "list-compress-depth"
2) "0"
```

以下是QuickList的和QuickListNode的结构源码： 

```c
typedef struct quickList{
	// 头节点指针
	quicklistNode *head;
	// 尾节点
	quicklistNode *tail;
	// 所有ziplist的entry的数量
	unsigned long count;
	// ziplists总数量
	unsigned long len;
	// ziplist的entry上限，默认值-2 
	int fill : QL_FILL_BITS;
	//首尾不压缩的节点数量 
	unsigned int compress : QL_COMP_BITS; /* depth of end nodes not to compress;0=off */
	//内存重分配时的书签数量及数组，一般用不到 
	unsigned int bookmark_count: QL_BM_BITS;
	quicklistBookmark bookmarks[];
}
```

```c
typedef struct quicklistNode {
	// 前一个节点指针 
    struct quicklistNode *prev;
    // 下一个节点指针 
    struct quicklistNode *next;
    // 当前节点的ZipList指针 
    unsigned char *zl;
    // 当前节点的ZipList的字节大小 
    unsigned int sz;             /* ziplist size in bytes */
    // 当前节点的ZipList的entry个数 
    unsigned int count : 16;     /* count of items in ziplist */
    // 编码方式：1,ZipList;2,lzf压缩模式 
    unsigned int encoding : 2;   /* RAW==1 or LZF==2 */
    // 数据容器类型（预留）:1,其它；2,ZipList 
    unsigned int container : 2;  /* NONE==1 or ZIPLIST==2 */
    // 是否被解压缩。1:则说明被解压了，将来要重新压缩 
    unsigned int recompress : 1; /* was this node previous compressed? */
    unsigned int attempted_compress : 1; /* node can't compress; too small */
    unsigned int extra : 10; /* more bits to steal for future usage */
} quicklistNode;
```


一段流程图来描述当前的这个结构

<img src="https://article.biliimg.com/bfs/article/61bb40ce791324bc92e4e9d96d3cb331d4f37338.png" alt="image.png" style="zoom:70%;" />

QuickList的特点： 
- 是一个节点为ZipList的**双端链表**
- 节点采用ZipList,解决了传统链表的内存占用问题 
- 控制了ZipList大小，解决连续内存空间申请效率问题 
- 中间节点可以压缩，进一步节省了内存 


### SkipList

**SkipList（跳表）** 首先是链表，但与传统链表相比有几点差异：
- 元素按照**升序排列**存储
- 节点可能**包含多个指针**，指针跨度不同(分等级类似于红黑树)

<img src="https://article.biliimg.com/bfs/article/7b29861a91ccd0bcd9b76d549f3d802ee35323da.png" alt="image.png" style="zoom:90%;" />

```c
typedef struct zskiplist{
	// 头尾节点指针
	struct zskiplistNode *header, *tail;
	// 节点数目
	unsigned long length;
	// 最大的索引层级,默认是1
	int level;
}zskiplist
```

```c
typedef struct zskiplistNode{
	// 节点存储的值
	sds ele;
	// 节点分数,排序,查应用
	double score;
	struct zskiplistLevel {
		struct zskiplistNoden *forward; // 下一个节点指针
		unsigned long span; // 索引跨度
	} level[]; //多级索引数组
	int level;
}zskiplistNode
```

^26cf67

<img src="https://article.biliimg.com/bfs/article/95689f6f931a23923dda2cdcf5312b1eac034b4f.png" alt="image.png" style="zoom:80%;" />

SkipList的特点：

* 跳跃表是一个**双向链表**，每个节点都包含score和ele值
* 节点**按照score值排序**，score值一样则按照ele字典排序
* 每个节点都可以**包含多层指针**，层数是1到32之间的随机数
* 不同层指针到下一个节点的跨度不同，层级越高，跨度越大
* 增删改查效率与红黑树基本一致，实现却更简单

### RedisObject 

Redis中的任意**数据类型的键和值**都会被**封装**为一个`RedisObject`，也叫做Redis对象，源码如下：

1、什么是redisObject：
从Redis的使用者的角度来看，⼀个Redis节点包含多个`database`（非cluster模式下默认是16个，cluster模式下只能是1个），而一个database维护了从key space到object space的映射关系。这个映射关系的key是string类型，⽽value可以是多种数据类型，比如：
string, list, hash、set、sorted set等。我们可以看到，key的类型固定是string，而value可能的类型是多个。
⽽从Redis内部实现的⾓度来看，database内的这个映射关系是用⼀个dict来维护的。dict的key固定用⼀种数据结构来表达就够了，这就是动态字符串sds。而value则比较复杂，为了在同⼀个dict内能够存储不同类型的value，这就**需要⼀个通⽤的数据结构**，这个通用的数据结构就是robj，全名是redisObject。


<img src="https://article.biliimg.com/bfs/article/f2c8371ca990a9fd3e7c8b64640095865c7653d4.png" alt="image.png" style="zoom:80%;" />

> 一个redisOject所占的为16B,不包含指针所指向的空间,假如说有10个str,每个str都要一个redisOject,也就是160B,而一个list只需要一个redisOject就是16B,所以string这种结构虽然简单,但是会有大量的内存浪费


#### Redis的编码方式 
Redis中会根据存储的数据类型不同，选择不同的编码方式，共包含11种不同类型： 

| **编号** | **编码方式**                | **说明**        |
|:------:|:-----------------------:|:-------------:|
|      0 | OBJ_ENCODING_RAW        | raw编码动态字符串    |
|      1 | OBJ_ENCODING_INT        | long类型的整数的字符串 |
|      2 | OBJ_ENCODING_HT         | hash表（字典dict） |
|      3 | OBJ_ENCODING_ZIPMAP     | 已废弃           |
|      4 | OBJ_ENCODING_LINKEDLIST | 双端链表          |
|      5 | OBJ_ENCODING_ZIPLIST    | 压缩列表          |
|      6 | OBJ_ENCODING_INTSET     | 整数集合          |
|      7 | OBJ_ENCODING_SKIPLIST   | 跳表            |
|      8 | OBJ_ENCODING_EMBSTR     | embstr的动态字符串  |
|      9 | OBJ_ENCODING_QUICKLIST  |快速列表|
|     10 | OBJ_ENCODING_STREAM     | Stream流       |  


### 五种数据结构

Redis中会根据存储的数据类型不同，选择不同的编码方式。每种数据类型的使用的编码方式如下：

| **数据类型**   | **编码方式**                                   |
|:----------:|:------------------------------------------:|
| OBJ_STRING | int、embstr、raw                             |
| OBJ_LIST   | LinkedList和ZipList(3.2以前)、QuickList（3.2以后） |
| OBJ_SET    | intset、HT                                  |
| OBJ_ZSET   | ZipList、HT、SkipList                        |
| OBJ_HASH   | ZipList、HT                                 |  


#### String

String是Redis中最常见的数据存储类型：
- 其基本编码方式是**RAW**，基于简单动态字符串（**SDS**）实现，存储上限为512mb。有两次的申请内存,一个RedisObject,一次SDS

	- <img src="https://article.biliimg.com/bfs/article/19c9aeef4e2d24e7e628de5729982177aaf00d7c.png" alt="image.png" style="zoom:80%;" />

- 如果存储的SDS长度小于44字节(采用整个原因是合起来刚好64字节,是$2^n$,恰好是一个分片大小了,不会产生内存碎片)，则会采用**EMBSTR编码**，此时<font color=#ff0000>object head与SDS是一段连续空间</font>。申请内存时只需要调用一次内存分配函数,效率更高。
- 如果存储的字符串是整数值，并且大小在`LONG_MAX`范围内，则会采用**INT编码**：直接将数据保存在RedisObject的**ptr指针位置**（刚好8字节），**不再需要SDS了**。

	- <img src="https://article.biliimg.com/bfs/article/2f9c5b3a9d5aa500b64dde8e93b651766f64d722.png" alt="1653987172764" style="zoom:67%;" />

	- <img src="https://article.biliimg.com/bfs/article/fe12d5012772d08d03c484d2d214c987023ee65a.png" alt="image.png" style="zoom:50%;" />



<img src="https://article.biliimg.com/bfs/article/e42074b6d1d1f36c9d52c97f1fbe1e5a19e58ee2.png" alt="1653987202522" style="zoom:67%;" />

#### List

Redis的List类型可以从首、尾操作列表中的元素：

```bash
127.0.0.1:6379> LPUSH L1 e3 e2 e1 #从head 写入 
(integer) 3 
127.0.0.1:6379> RPUSH L1 e4 e5 e6 #从tail写入 
(integer) 6 
127.0.0.1:6379> LRANGE 11 0 6 
# 范围获取 
"e1" 
"e2" 
"e3" 
"e4" 
"e5" 
127.0.0.1:6379>LPOP L1 1 
#从head取 
1 ) " e1 " 
127.0.0.1:6379>RPOP 11 1 
#从tail取 
1 ) " e6 " 

```

哪一个数据结构能满足上述特征？
- `LinkedList` ：普通链表，可以从双端访问，内存占用较高，内存碎片较多
- `ZipList` ：压缩列表，可以从双端访问，内存占用低，存储上限低
- `QuickList`：LinkedList + ZipList，可以从双端访问，内存占用较低，包含多个ZipList，存储上限高\

Redis的List结构类似一个双端链表，可以从首、尾操作列表中的元素：
- 在3.2版本之前，Redis采用ZipList和LinkedList来实现List，当元素数量小于512并且元素大小小于64字节时采用ZipList编码，超过则采用LinkedList编码。
- 在3.2版本之后，Redis统一采用QuickList来实现List：

```c
void pushGenericCommand(client *c, int where, int xx) {
    int j;
    // 尝试找到KEY对应的list
    robj *lobj = lookupKeyWrite(c->db, c->argv[1]);
    // 检查类型是否正确
    if (checkType(c,lobj,OBJ_LIST)) return;
    // 检查是否为空
    if (!lobj) {
        if (xx) {
            addReply(c, shared.czero);
            return;
        }
		// 为空,则创建新的QuickList
        lobj = createQuicklistObject();
        quicklistSetOptions(lobj->ptr, server.list_max_ziplist_size,
                            server.list_compress_depth);
        dbAdd(c->db,c->argv[1],lobj);
    }
}
```


```c
robj *createQuicklistObject(void) {
	// 申请内存并初始化QuickList
    quicklist *l = quicklistCreate();
    // 创建RedisOject,type为OBJ_LIST
    // ptr指向 QuickList
    robj *o = createObject(OBJ_LIST,l);
    // 设置编码为QuickList
    o->encoding = OBJ_ENCODING_QUICKLIST;
    return o;
}
```



<img src="https://article.biliimg.com/bfs/article/807c37aac9a6aa4947ebacd34876fa1ca5247e86.png" alt="1653987313461" style="zoom:80%;" />

#### Set结构

Set是Redis中的单列集合，满足下列特点：

* 不保证有序性
* 保证元素唯一
* 求交集、并集、差集

```bash
127.0.0.1:6379> SADD S1 m1 m2 m3 
(integer) 3 
127.0.0.1:6379> SADD s2 m2 m3 m4 
(integer) 3 
127.0.0.1:6379> SISMEMBER S1 m1 
#判断元素是否存在 
(integer) 1 
127.0.0.1:6379> SINTER S1 S2 
#求交集 
1) "m3" 
2) "m2" 
```

可以看出，Set对**查询元素的效率要求非常高**，思考一下，什么样的数据结构可以满足？
HashTable，也就是Redis中的Dict，不过Dict是双列集合（可以存键、值对）

Set是Redis中的集合，不一定确保元素有序，可以满足元素唯一、查询效率jihe
- 为了查询效率和唯一性，set采用HT编码（Dict）。Dict中的key用来存储元素，value统一为null。
- 当存储的所有数据都是**整数**，并且元素数量不超过`set-max-intset-entries`时，Set会采用[[redis#IntSet|IntSet]]编码，以节省内存

```c
robj *setTypeCreate(sds value)
{
	// 判断value是否是数值类型 long long
	if(isSdsRepresentableAsLongLong(value,NULL)==C_OK)
		//如果是数值类型，则采用IntSet编码 
		return createIntsetObject();
	// 否则采用默认编码，也就是HT 
	return createSetObject();
}
```

```c
robj *createIntsetObject(void)
{
	// 初始化INTSET并申请内存空间
	intset *is=intsetNew();
	// 创建RedisObject
	robj *o=createObject(OBJ_SET,is);
	// 指定编码为INTSET
	o->encoding=OBJ_ENCODING_INTSET;
	return o;
}
```

```c
robj *createSetobject(void) { 
	//初始化Dict类型，并申请内存 
	dict *d = dictCreate (&setDictType,NULL); 
	//创建RedisObject 
	robj *o = createobject(OBJ_SET,d); 
	//设置encoding为HT 
	o->encoding = OBJ_ENCODING_HT; 
	return o; 
} 
```


<img src="https://article.biliimg.com/bfs/article/f5a4835d27d8d38bb0316c8870b890401f93b9c9.gif" alt="image.png" style="zoom:50%;" />

#### ZSET

ZSet也就是[[redis#SortedSet类型|SortedSet]]，其中每一个元素都需要指定一个score值和member值：

* 可以根据score值排序后
* member必须唯一
* 可以根据member查询分数

<img src="https://article.biliimg.com/bfs/article/8757c351100c27861d5caabe6ca7945606438f58.png" alt="image.png" style="zoom:60%;" />

因此，zset底层数据结构必须满足<font color=#ff0000>键值存储、键必须唯一、可排序</font>这几个需求。之前学习的哪种编码结构可以满足？ 
- `SkipList`:可以排序，并且可以同时存储score和ele值（member),不满足唯一
- `HT(Dict)`:可以键值存储，并且可以根据key找value,但是无序

```c
typedef struct zset{
	// Dict指针 
	dict *dict;
	// SkipList指针 
	zskiplist *zsl;
}zset;
```

```C
robj *createzsetobject(void) { 
	zset *zs = zmalloc(sizeof(*zs)); 
	robj *o; 
	//创建Dict 
	zs->dict=dictCreate(&zsetDictType,NULL); 
	//创建SkipList 
	zs->zsl=zslcreate(); 
	o = createobject(OBJ_ZSET,zs); 
	o->encoding = OBJ_ENCODING_SKIPLIST; 
	return o; 
}  
```


<img src="https://article.biliimg.com/bfs/article/c316a664e0e98f784ed697770006bd3821b89b3f.png" alt="1653992172526" style="zoom:80%;" />
看到上图,我们就知道Zset得两个缺点:
- 占用空间大
- 一份数据存两份,一份存dict,一份存zset

当元素数量不多时，HT和SkipList的优势不明显，而且更耗内存。因此zset还会采用ZipList(这里就是**采用遍历**了)结构来节省内存，不过需要同时满足两个条件：
* 元素数量小于`zset_max_ziplist_entries`，默认值128
* 每个元素都小于`zset_max_ziplist_value`字节，默认值64

ziplist本身没有排序功能，而且没有键值对的概念，因此需要有zset通过编码实现：

* ZipList是连续内存，因此score和element是紧挨在一起的两个entry， element在前，score在后
* score越小越接近队首，score越大越接近队尾，按照score值升序排列

```c
// zadd添加元素时，先根据key找到zset,不存在则创建新的zset 
zobj=lookupKeywrite(c->db,key); 
if(checkType(c,zobj,OBJ_ZSET)) goto cleanup; 
//判断是否存在 
if(zobj==NULL){//zset不存在 
	if (server.zset_max_ziplist_entries==0|| 
		server.zset_max_ziplist_value<sdslen(c->argv[scoreidx+1]->ptr)) 
	{// zset_max_ziplist_entries设置为o就是禁用了ZipList, 
		//或者value大小超过了zset_max_ziplist_value,采用HT+SkipList 
		zobj = createzsetobject(); 
	} else { // 否则，采用 ZipList 
		zobj = createzsetziplistobject(); 
	}
	dbAdd(c->db,key,zobj); 
}
zsetAdd(zobj,score,ele,flags,&retflags,&newscore); 
```

```c
robj *createzsetobject(void) { 
	//申请内存 
	zset *zs = zmalloc (sizeof(*zs)); 
	robj *o; 
	//创建Dict 
	zs->dict = dictCreate(&zsetDictType,NULL); 
	//创建SkipList 
	zs->zsl=zslCreate(); 
	o = createobject(OBJ_ZSET,zs); 
	o->encoding =OBJ_ENCODING_SKIPLIST; 
	return o; 
}
```

```c
robj *createZsetZiplistobject(void){ 
	//创建ZipList 
	unsigned char*ziplistNew(); 
	robj *o = createobject(OBJ_ZSET, zl); 
	o->encoding = OBJ_ENCODING_ZIPLIST; 
	return o; 
}
```


```c
int zsetAdd(robj *zobj, double score, sds ele, int inflags, int 
*out_flags,double *newscore){ 
	/*判断编码方式*/ 
	if (zobj->encoding== OBJ_ENCODING_ZIPLIST){//是ZipList编码 
		unsigned char *eptr; 
		//判断当前元素是否已经存在，已经存在则更新score即可 
		if ((eptr = zzlFind(zobj->ptr,ele,&curscore))!=NULL){ 
			//...略 
			return 1; 
		} else if(!xx) { 
		//元素不存在，需要新增，则判断ziplist长度有没有超、元素的大小有没有超 
		if (zzlLength(zobj->ptr)+1>server.zsetmaxziplistentries||sdslen(ele)>server.zsetmaxziplistvalue||!ziplistsafeToAdd ( zobj - > ptr , sdslen(ele))) 
		{   //如果超出，则需要转为SkipList编码 
			zsetConvert(zobj,OBJ_ENCODING_SKIPLIST); 
		} else { 
			zobj->ptr=zzlInsert(zobj->ptr,ele,score); 
		if(newscore) *newscore = score; 
			*out_flags|=ZADD_OUT_ADDED; 
			return 1; 
		}
	} else { 
		*out_flags|=ZADD_OUT_NOP; 
		return 1; 
	}
}
	//本身就是SKIPLIST编码，无需转换 
if ( zobj->encoding == OBJ_ENCODING_SKIPLIST ) { 
	//...略 
} else { 
		serverPanic("Unknown sorted set encoding"); 
	}
	return 0 ; /* Never reached. */ 
}
```


ziplist本身没有排序功能，而且没有键值对的概念，因此需要有zset通过编码实现： 
- ZipList是连续内存，因此score和element是紧挨在一起的两个entry,element在前，score在后 
- score越小越接近队首,score越大越接近队尾，按照score值升序排列 

<img src="https://article.biliimg.com/bfs/article/15471cc9437422cb292ccdf03ed29a27f4484ac1.png" style="zoom:80%;" />


#### Hash

Hash结构与Redis中的Zset非常类似：
* 都是键值存储
* 都需求根据键获取值
* 键必须唯一

区别如下：

* zset的键是member，值是score；hash的键和值都是**任意值**
* zset要根据score排序；hash则**无需排序**


**底层实现方式**：压缩列表ziplist或者字典dict

- Hash结构默认采用ZipList编码，用以节省内存。ZipList中相邻的两个entry分别保存field和value 
- 当数据量较大时，Hash结构会转为HT编码，也就是Dict,触发条件有两个： 
	1. ZipList中的元素数量超过了`hash-max-ziplist-entries`(默认512) 
	2. ZipList中的任意entry大小超过了`hash-max-ziplist-value`(默认64字节） 


* 每次插⼊或修改引发的realloc操作会有更⼤的概率造成内存拷贝，从而降低性能。
* ⼀旦发生内存拷贝，内存拷贝的成本也相应增加，因为要拷贝更⼤的⼀块数据。
* 当ziplist数据项过多的时候，在它上⾯查找指定的数据项就会性能变得很低，因为ziplist上的查找需要进行遍历。


<img src="https://article.biliimg.com/bfs/article/70baee77543872fb92ce72750c6cf7f639eadbb0.gif" alt="image.png" style="zoom:50%;" />


```c
void hsetCommand(cient *c) {// hset user1 name Jack age 21 
	int i, created = 0; 
	robj *o;//略 
	//判断hash的key是否存在，不存在则创建一个新的，默认采用ZipList编码 
	if ((o=hashTypeLookupWriteOrCreate(c,c->argv[1]))==NULL) return; 
	//判断是否需要把ZipList转为Dict 
	hashTypeTryConversion(o,c->argv,2,c->argc-1); 
	//循环遍历每一对field和value,并执行hset命令 
	for(i=2;i<c->argc;i+=2) 
		created += !hashTypeSet(o,c->argv[i]->ptr,c->argv[i+1]->ptr,HASH_SET_COPY); 
		//略...
} 
```

```c
robj *hashTypeLookupWriteOrCreate(client *c, robj *key){ 
	//查找key 
	robj *o = LookupKeywrite(c->db,key); 
	if (checkType(c,o,OBJ_HASH))return NULL; 
	//不存在，则创建新的 
	if(o==NULL){ 
		o = createHashObject(); 
		dbAdd(c->db,key,o); 
	}
	return o; 
}
```


```c
robj *createHashobject(void) { 
	// 默认采用ZipList编码，申请ZipList内存空间 
	unsigned char *zl = ziplistNew(); 
	robj *o = createobject(OBJ_HASH,zl); 
	// 设置编码 
	o->encoding = OBJ_ENCODING_ZIPLIST; 
	return o; 
}
```


```c
void hashTypeTryConversion(robj *o, robj **argv, int start, int end) { 
	int i; 
	size_t sum = 0; 
	//本来就不是ZipList编码，什么都不用做了 
	if (o->encoding!=OBJ_ENCODING_ZIPLIST) return; 
	//依次遍历命令中的field、value参数 
	for(i=start;i<=end;i++){ 
		if (!sdsEncodedobject(argv[i])) 
		continue; 
		size_t len = sdslen(argv[i]->ptr); 
		//如果field或value超过hash_max_ziplist_value,则转为HT 
		if (len > server.hash_max_ziplist_value) { 
			hashTypeConvert(o, OBJ_ENCODING_HT); 
			return; 
		}
		sum += Len; 
	}//ziplist大小超过1G,也转为HT 
	if (!ziplistSafeToAdd (o->ptr, sum)) 
		hashTypeConvert(o, OBJ_ENCODING_HT); 
}
```


```c
int hashTypeSet(robj *o, sds field, sds value, int flags) { 
	int update = 0; 
	//判断是否为ZipList编码 
	if (o->encoding == OBJ_ENCODING_ZIPLIST){ 
		unsigned char *zl, *fptr, *vptr; 
		zl=o->ptr; 
		// 查询head指针 
		fptr = ziplistIndex(zl ,ZIPLIST_HEAD); 
		if(fptr != NULL){//head不为空，说明ZipList不为空，开始查找key 
		fptr = ziplistFind(zl, fptr, (unsigned char*) field, sdslen(field) 
		if(fptr !=NULL){//判断是否存在，如果已经存在则更新 
			update = 1; 
			zl=ziplistReplace(zl,vptr,(unsigned char*)value, 
				sdslen(value)); 
			}
		}
		//不存在，则直接push 
		if(!update){//依次push新的field和value到zipList的尾部 
			zl=ziplistPush(zl,(unsigned char*)field,sdslen(field), ZIPLIST_TAIL); 
			zl=ziplistpush(zl,(unsigned char*)value,sdslen(value), ZIPLIST_TAIL); 
	}
	o->ptr=zl; 
	/*插入了新元素，检查list长度是否超出，超出则转为HT*/ 
	if (hashTypeLength(o)>server.hash_max_ziplist_entries) 
		hashTypeConvert(o,OBJENCODINGHT); 
	} else if (o->encoding == OBJ_ENCODING_HT){ 
		//HT编码，直接插入或覆盖 
	} else { 
		serverPanic("Unknown hash encoding"); 
	} 
	return update; 
}
```

#### 布隆过滤器

与前面的hash结构相比，k-v都是存储数据的，如果我们的需求只是查找一个key是否存在，而**不关注它具体的值**，就可以用布隆过滤器解决，**布隆过滤器对比hash是不存储具体的元素**，但是能查找某个元素是否存在。

布隆过滤器的实现细节
1. 布隆过滤器采用了位图(bitmap)结构，达到了解决空间的一种方式
	```c
	unsigned int 32
	uint32_t bitmap[4]; //128位
	```
2. 布隆过滤器用hash的特性，`hash(key)%128=v1` 落在位图上的一个点，将这个点位置标为1，用这一位来标识这个key是否存在
3. 虽然使用hash特性，但还是有冲突，所以采用多个hash函数，`{h1,h2,h3}`, `h1(key)%128=v1` `h2(key)%128=v2`,`h3(key)%128=v3`,这里的hash函数采用了质数，防止到同一个点。
4. 每到一个key，我们可以通过这一套顺序hash，检测这一套位是否都为1，**都为1，说明key存在，有一个不为1，代表不存在**
5. 一套hash能确保不会重复，但是多个key无法保证，所以**都为1，说明key存在**这句话无法保证，但是**有一个不为1，代表不存在**还能保证
	```c
	key: k1,k2
	k1->v11 v12 v13
	k2->v21 v22 v23
	假设 v11的位v22的位相同
	```

	<img src="https://img-blog.csdnimg.cn/7006f54bb22543eaadf437b676eb1737.png" alt="image.png" style="zoom:40%;" />

6. **增加一个节点没问题，删除一个节点有问题**，删除一个key对于的位，可能其他key也对应，所以不能删除
7. 所有布隆过滤器是判断一定不存在，概率存在，这个概率跟hash函数个数，位数长度有关，里面的key的节点个数有关

布隆过滤器的应用场景
- 如果想要判断一个元素是不是在一个集合里，一般想到的是将所有元素保存起来，然后通过比较确定。链表，树、哈希表等数据结构都是这种思路（如下图所示）
<img src="https://pic1.zhimg.com/80/v2-fd393023f85161cf69394abfa297357e_1440w.webp?source=1def8aca" alt="image.png" style="zoom:80%;" />

上面这些数据结构面对数据量特别大的时候显现的缺点：
- 存储容量占比高，考虑到负载因子的存在，通常空间是不能被用满的
- 当数据量特别大时，会占用大量的内存空间。如果存储了类似于URL这样的key，那么内存消费太严重
- 如果使用hashmap，如果已有元素超过了总容量的一半之后，一般就需要考虑扩容了，因为元素多了之后哈希冲突就会增加，退化为链表存储的效率了

## Redis网络模型

### 用户空间和内核空间 

服务器大多都采用Linux系统，这里我们以Linux为例来讲解:
ubuntu和Centos 都是Linux的发行版，发行版可以看成对linux包了一层壳，任何Linux发行版，其系统内核都是Linux。我们的应用都需要**通过Linux内核与硬件交互**

<img src="https://article.biliimg.com/bfs/article/a9fb658ce560ce66d6339586356b68fffc3a42c8.png" alt="image.png" style="zoom:50%;" />

用户的应用，比如redis，mysql等其实是没有办法去执行访问我们操作系统的硬件的，所以我们可以通过发行版的这个壳子去访问内核，再通过内核去访问计算机硬件

<img src="https://article.biliimg.com/bfs/article/77df446d9c4cfa8b9803f0126bba3c841d3c3cd1.png" alt="1653845147190" style="zoom:50%;" />

计算机硬件包括，如cpu，内存，网卡等等，内核（通过寻址空间）可以操作硬件的，但是内核需要不**同设备的驱动**，有了这些驱动之后，内核就可以去对计算机硬件去进行 内存管理，文件系统的管理，进程的管理等等


<img src="https://article.biliimg.com/bfs/article/54b5d9a948895ef1ec331e35709cf401e1dcb57c.png" alt="image.png" style="zoom:60%;" />

我们想要用户的应用来访问，计算机就必须要通过**对外暴露的一些接口**，才能访问到，从而简介的实现对内核的操控，但是内核本身上来说**也是一个应用**，所以他本身**也需要一些内存**，cpu等设备资源，用户应用本身**也在消耗这些资源**，如果不加任何限制，用户去操作随意的去操作我们的资源，就有可能导致一些<font color=#ff0000>冲突</font>，甚至有可能导致我们的系统出现无法运行的问题，因此我们需要把用户和**内核隔离开**

- 进程的**寻址空间**划分成两部分：**内核空间、用户空间**
	- 什么是寻址空间呢？
		- 我们的应用程序也好，还是内核空间也好，都是**没有办法直接去物理内存**的，而是通过分配一些虚**拟内存映射到物理内存中**
		- 我们的内核和应用程序去访问虚拟内存的时候，就需要一个虚拟地址，这个地址是**一个无符号的整数**，比如一个32位的操作系统，他的带宽就是32，他的虚拟地址就是2的32次方，也就是说他寻址的范围就是0~2的32次方， 这片寻址空间对应的就是2的32个字节，就是4GB
		- 这个4GB，会有3个GB分给用户空间，会有1GB给内核系统

		- <img src="https://article.biliimg.com/bfs/article/7ce8b7437365cd4217b111f6db7181b19ae1c0a4.png" alt="1653896377259" style="zoom:67%;" />

- 在linux中，他们权限分成两个等级0和3
- 用户空间只能执行受限的命令（Ring3），而且不能直接调用系统资源，必须通过内核提供的接口来访问
- 内核空间可以执行特权命令（Ring0），调用一切系统资源
- 所以一般情况下，用户的操作是运行在用户空间，而内核运行的数据是在内核空间的，而有的情况下，一个应用程序需要去调用一些特权资源，去调用一些内核空间的操作，所以此时他俩需要在**用户态和内核态之间进行切换**。

<img src="https://article.biliimg.com/bfs/article/bb4f5066dce73673345ab214cc22b9a1700d3372.png" style="zoom:70%;" />
用IO流举例**用户态和内核态之间进行切换**

Linux系统为了提高IO效率，会在用户空间和内核空间都加入缓冲区：
- 写数据时，要把用户缓冲数据拷贝到内核缓冲区，然后写入设备
- 读数据时，要从设备读取数据到内核缓冲区，然后拷贝到用户缓冲区
针对这个操作：我们的用户在写读数据时，会去向内核态申请，想要读取内核的数据，而内核数据要去**等待驱动程序**从硬件上读取数据，当从磁盘上加载到数据之后，内核会将数据**写入到内核的缓冲区**中，然后再将**数据拷贝到用户态的buffer**中，然后再返回给应用程序，整体而言，速度慢，就是这个原因，为了加速，我们希望read也好，还是`wait for data`也最好都不要等待，或者时间尽量的短。

### 阻塞IO 

在《UNIX网络编程》一书中，总结归纳了5种IO模型：
- 阻塞IO（Blocking IO）
- 非阻塞IO（Nonblocking IO）
- IO多路复用（IO Multiplexing）
- 信号驱动IO（Signal Driven IO）
- 异步IO（Asynchronous IO）

应用程序想要去读取数据，他是无法直接去读取磁盘数据的，他需要先到内核里边去**等待内核操作硬件拿到数据**，这个过程就是1，是需要等待的，等到内核从磁盘上把数据加载出来之后，再把这个数据写给用户的缓存区，这个过程是2，如果是阻塞IO，那么整个过程中，用户从发起读请求开始，一直到读取到数据，**都是一个阻塞状态**。

<img src="https://article.biliimg.com/bfs/article/06a4ed7513a0385c50975ba47f9b9893715afe7c.png" alt="image.png" style="zoom:60%;" />

具体流程如下图：
用户去读取数据时，会去先发起`recvform`一个命令，去尝试从内核上加载数据，如果内核没有数据，那么用户就会等待，此时内核会去从硬件上读取数据，内核读取数据之后，会把数据拷贝到用户态，并且返回ok，整个过程，都是阻塞等待的，这就是阻塞IO
总结如下：
顾名思义，阻塞IO就是两个阶段都必须阻塞等待：

<img src="https://article.biliimg.com/bfs/article/762f3608dcf504b64beaa57f463b927d1d8bc12f.png" alt="1653897270074" style="zoom:67%;" />

**阶段一：**
- 用户进程尝试读取数据（比如网卡数据）
- 此时数据尚未到达，内核需要等待数据
- 此时用户进程也处于阻塞状态
    
**阶段二：**
- 数据到达并拷贝到内核缓冲区，代表已就绪
- 将内核数据拷贝到用户缓冲区
- 拷贝过程中，用户进程依然阻塞等待
- 拷贝完成，用户进程解除阻塞，处理数据

### 非阻塞IO 

顾名思义，非阻塞IO的recvfrom操作会立即返回结果而不是阻塞用户进程。
阶段一：
- 用户进程尝试读取数据（比如网卡数据）
- 此时数据尚未到达，内核需要等待数据
- 返回异常给用户进程
- 用户进程拿到error后，再次尝试读取
- 循环往复，直到数据就绪
阶段二：
- 将内核数据拷贝到用户缓冲区 
- 拷贝过程中，用户进程依然阻塞等待
- 拷贝完成，用户进程解除阻塞，处理数据



<img src="https://article.biliimg.com/bfs/article/e24d29fe43fec6c152e78ad50fcaac47dc60d462.png" alt="1653897490116" style="zoom:67%;" />

- 可以看到，非阻塞IO模型中，**用户进程在第一个阶段是非阻塞**，**第二个阶段是阻塞状态**。虽然是非阻塞，但性能并没有得到提高。而且忙等机制会导致CPU空转，CPU使用率暴增。

### IO多路复用 

无论是阻塞IO还是非阻塞IO，用户应用在一阶段都需要调用recvfrom来获取数据，差别在于无数据时的处理方案：
- 如果调用recvfrom时，恰好没有数据，阻塞IO会使CPU阻塞，非阻塞IO使CPU**空转**(cpu可以做一些其他事情)，都不能充分发挥CPU的作用。 
- 如果调用recvfrom时，恰好有数据，则用户进程可以直接进入第二阶段，读取并处理数据

所以怎么看起来以上两种方式性能都不好
而在**单线程情况下**，只能依次处理IO事件，如果正在处理的IO事件恰好未就绪（数据不可读或不可写），线程就会被阻塞，所有IO事件都必须等待，性能自然会很差。

就比如服务员给顾客点餐，**分两步**：
- 顾客思考要吃什么（等待数据就绪）
- 顾客想好了，开始点餐（读取数据）

<img src="https://article.biliimg.com/bfs/article/299993506a8b6a09a2403721c20817f89f4af408.png" alt="image.png" style="zoom:50%;" />


要提高效率有几种办法？
1. 方案一：增加更多服务员（**多线程**）(缺点就是多线程**切换上下文**开销大了)
2. 方案二：不排队，谁想好了吃什么（数据就绪了），服务员就给谁点餐（用户应用就去读取数据）

那么问题来了：<font color=#ff0000>用户进程如何知道内核中数据是否就绪呢</font>？

- **文件描述符**（File Descriptor）：简称FD，是一个**从0开始的无符号整数**，用来关**联Linux中的一个文件**。在Linux中，一切皆文件，例如常规文件、视频、硬件设备等，当然也包括网络套接字（Socket）。
- 通过FD，我们的网络模型可以利用一个**线程监听多个FD**，并在某个FD可读(FD数值变化)、可写时得到通知，从而避免无效的等待，充分利用CPU资源。

阶段一：
* 用户进程调用select，指定要监听的**FD集合**
* 监听FD对应的多个socket
* 任意一个或多个socket数据就绪则返回`readable`(数据可读的socket的集合)
* 此过程中用户进程阻塞
阶段二：
* 用户进程找到就绪的socket
* 依次调用recvfrom读取数据
* 内核将数据拷贝到用户空间
* 用户进程处理数据

<img src="https://article.biliimg.com/bfs/article/cb0f27fd67dc1b3d97b2bf338432fae6f5c6d9d9.png" alt="image.png" style="zoom:70%;" />

**IO多路复用**:利用**单个线程来同时监听多个FD**，并在某个FD可读、可写时得到通知，从而避免无效的等待，充分利用CPU资源。不过监听FD的方式、通知的方式又有多种实现，常见的有：
- select
- poll
- epoll

<img src="https://article.biliimg.com/bfs/article/aa8013d6c134e9d77ca8bf174581177fd99f8799.png" alt="image.png" style="zoom:50%;" />


差异： 
- select和poll只会通知用户进程有FD就绪，但**不确定具体是哪个FD**,需要用户进程逐个遍历FD来确认 
- epoll则会在通知用户进程FD就绪的同时，把**已就绪的FD写入用户空间** 

#### select方式


简单说，就是我们把需要处理的数据封装成FD，然后在用户态时创建一个**fd的集合**（这个集合的大小是要监听的那个**FD的最大值+1**，但是大小整体是有限制的 ），这个集合的长度大小是有限制的，同时在这个集合中，标明出来我们要控制哪些数据，


```c
//定义类型别名__fd_mask,本质是 long int 
typedef long int __fd_mask; 
/* fd_set 记录要监听的fd集合，及其对应状态*/ 
typedef struct { 
	// fds_bits是long类型数组，长度为1024/32=32 
	// 共1024个bit位，每个bit位代表一个fd,0代表未就绪，1代表就绪 
	__fd_mask fds_bits[__FD_SETSIZE / __NFDBITS]; 
	// ... 
} fd_set; 
// select函数，用于监听多个fd的集合 
int select( 
	int nfds,//要监视的fd_set的最大fd+1 
	fd_set *readfds,//要监听读事件的fd集合 
	fd_set *writefds,//要监听写事件的fd集合 
	fd_set *exceptfds,// //要监听异常事件的fd集合 
	//超时时间，null-永不超时；o-不阻塞等待；大于o-固定等待时间 
	struct timeval *timeout 
); 

```



<img src="https://github.com/YeSho-cpp/pic_bed/blob/main/GIF%202023-9-10%2020-14-21.gif?raw=true" alt="image.png" style="zoom:50%;" />

- 比如要监听的数据，是1,2,5三个数据，此时会执行select函数，然后将整个fd发给内核态，
- 内核态会去遍历用户态传递过来的数据，如果发现这里边都数据都没有就绪，就休眠，直到有数据准备好时，就会被唤醒，
- 唤醒之后，**再次遍历**一遍，看看谁准备好了，然后再将处理掉没有准备好的数据，最后再将这个FD集合写回到用户态中去，此时用户态就知道了，
- 奥，有人准备好了，但是对于用户态而言，**并不知道谁处理好了**，所以用户态也需要**去进行遍历**，然后找到对应准备好数据的节点，再去发起读请求，我们会发现，这种模式下他虽然比阻塞IO和非阻塞IO好，但是依然有些麻烦的事情， 比如说频繁的传递fd集合，频繁的去遍历FD等问题

select模式存在的问题：
- 需要将整个fd_set从用户空间拷贝到内核空间，select结束还要再次拷贝回用户空间
- select无法得知**具体是哪个fd就绪**，需要遍历整个fd_set
- fd_set监听的fd数量不能超过1024

#### poll模式

poll模式对select模式做了简单改进，但性能提升不明显，部分关键代码如下：

IO流程：

- 创建pollfd数组，向其中添加关注的fd信息，数组大小自定义
- 调用poll函数，将pollfd数组拷贝到内核空间，转链表存储，无上限
- 内核遍历fd，判断是否就绪
- 数据就绪或超时后，拷贝pollfd数组到用户空间，返回就绪fd数量n
- 用户进程判断n是否大于0,大于0则遍历pollfd数组，找到就绪的fd
    

**与select对比：**

- select模式中的fd_set大小固定为1024，而pollfd在内核中采用链表，理论上无上限
- 监听FD越多，每次遍历消耗时间也越久，性能反而会下降

```c
// pollfd 中的事件类型 
#define POLLIN   //可读事件 
#define POLLOUT  //可写事件 
#define POLLERR  //错误事件 
#define POLLNVAL //fd未打开 
//pollfd结构 
struct pollfd { 
	int fd;           /*要监听的fd */ 
	short int events;/*要监听的事件类型：读、写、异常*/ 
	short int revents;/*实际发生的事件类型*/ 
}; 
// poll函数 
int poll ( 
	struct pollfd *fds, // pollfd数组，可以自定义大小 
	nfds_t nfds, //数组元素个数 
	int timeout //超时时间 
); 

```

revents这个变量在用户态没有初始化,在内核态后,根据类型进行赋值,如果超时了还没有一个fd就绪,那么revents就等于POLLNVAL

1. 创建pollfd数组，向其中添加关注的fd信息，数组大小自定义 
2. 调用poll函数，将pollfd数组拷贝到内核空间，转链表存储，无上限 
3. 内核遍历fd,判断是否就绪 
4. 数据就绪或超时后，拷贝pollfd数组到用户空间，返回就绪fd数量n 
5. 用户进程判断n是否大于0 
6. 大于0则**遍历pollfd数组**，找到就绪的fd 

与select对比： 
- select模式中的fd_set大小固定为1024,而pollfd在内核中采用链表，理论上无上限 
- 监听FD越多，每次遍历消耗时间也越久，性能反而会下降 

#### IO多路复用模型-epoll函数


epoll模式是对select和poll的改进，它提供了三个函数：
1. 第一个是：eventpoll的函数，他内部包含两个东西
	1. 红黑树-> 记录的事要**监听**的FD
	2. 一个是链表->一个链表，记录的是**就绪**的FD
	```c
	struct eventpoll { 
		//... 
		struct rb_root rbr;// 一颗红黑树，记录要监听的FD 
		struct list_head rdlist;// 一个链表，记录就绪的FD 
		//... 
	}; 
	
	// 1.会在内核创建eventpoll结构体，返回对应的句柄epfd ->指向唯一一个的epoll的标识
	int epoll_create(int size); 
	```

2. 紧接着调用epoll_ctl操作，将要监听的数据添加到红黑树上去，并且给每个fd设置一个**监听函数(ep_poll_callback)**，这个函数会在fd数据就绪时触发，就是准备好了，现在就把fd把数据添加到list_head中去(这个函数对于一个epfd来说它只执行一次)
  

	```c
	// 2.将一个FD添加到epoll的红黑树中，并设置ep_poll_callback 
	// callback触发时，就把对应的FD加入到rdlist这个就绪列表中 
	int epoll_ctl( 
		int epfd,//epoll实例的句柄 
		int op, // 要执行的操作，包括：ADD、MOD、DEL 
		int fd, 
		//要监听的FD 
		struct epoll_event *event//要监听的事件类型：读、写、异常等 
	); 
	```

3. 调用epoll_wait函数

	就去等待，在用户态创建一个**空的events数组**，当就绪之后，我们的**回调函数**会把数据添加到list_head中去，当调用这个函数的时候，会去检查list_head，当然这个过程需要参考配置的等待时间，可以等一定时间，也可以一直等， 如果在此过程中，检查到了list_head中有数据会将数据**添加到链表中**，此时将**数据放入到events数组**中，并且返回对应的**操作的数量**，用户态的此时收到响应后，从events中拿到对应准备好的数据的节点，再去调用方法去拿数据。
```c
//3.检查rdlist列表是否为空，不为空则返回就绪的FD的数量 
int epoll wait ( 
	int epfd, // eventpoll实例的句柄 
	struct epoll_event *events,//空event数组，用于接收就绪的FD 
	int maxevents, // events数组的最大长度 
	int timeout //超时时间，-1用不超时；0不阻塞；大于0为阻塞时间 
); 
```


<img src="https://article.biliimg.com/bfs/article/c33f5dfca8b4b5270fceb37e9d1d02569a6b8252.png" alt="image.png" style="zoom:50%;" />



小总结：
select模式存在的三个问题：
* 能监听的FD最大不超过1024
* 每次select都需要把所有要监听的FD都拷贝到内核空间
* 每次都要遍历所有FD来判断就绪状态

poll模式的问题：
* poll利用链表解决了select中监听FD上限的问题，但依然要遍历所有FD，如果监听较多，性能会下降

epoll模式中如何解决这些问题的？
* 基于epoll实例中的红黑树保存要监听的FD，理论上无上限，而且增删改查效率都非常高
* 每个FD只需要**执行一次epoll_ctl添加到红黑树**，以后每次epol_wait无需传递任何参数，无需重复拷贝FD到内核空间
* 利用`ep_poll_callback`机制来监听FD状态，无需遍历所有FD，因此性能不会随监听的FD数量增多而下降

#### 事件通知机制 
当FD有数据可读时，我们调用epoll_wait（或者select、poll）可以得到通知。但是事件通知的模式有两种：

* `LevelTriggered`：简称LT，也叫做水平触发。只要某个FD中有数据可读，每次调用epoll_wait都会得到通知。
* `EdgeTriggered`：简称ET，也叫做边沿触发。只有在某个FD有状态变化时，调用epoll_wait才会被通知。

举个栗子：
1. 假设一个客户端socket对应的FD已经注册到了epoll实例中
2. 客户端socket发送了2kb的数据
3. 服务端调用epoll_wait，得到通知说FD就绪
4. 服务端从FD读取了1kb数据(FD会从就绪队列移除)
5. 回到步骤3（再次调用epoll_wait，形成循环,而LT,会检查是否还有数据没传送完,有了FD会重新连接上就绪队列）

结论
- 如果我们采用LT模式，因为FD中仍有1kb数据，则第⑤步依然会返回结果，并且得到通知
- 如果我们采用ET模式，因为第③步已经消费了FD可读事件，第⑤步FD状态没有变化，因此epoll_wait不会返回，数据无法读取，客户端响应超时。

很多进程都要用同一份fd数据，但是只需要一个进程把数据读到用户空间，其他进程直接用就行了。LT的话就会导致需要这个数据的所有进程都被唤醒来执行这个读取操作，势必造成cpu资源的浪费
#### web服务流程 

<img src="https://article.biliimg.com/bfs/article/f9c121d416bed491faf53dc05c7f72d0daf33405.png" style="zoom:67%;" />

- 服务器启动以后，服务端会去调用`epoll_create`，创建一个epoll实例，epoll实例中包含两个数据
	1. 红黑树（为空）：`rb_root` 用来去记录需要被监听的FD
	2. 链表（为空）：`list_head`，用来存放已经就绪的FD

- 创建好了之后，会去调用`epoll_ctl`函数，此函数会会将需要监听的数据添加到`rb_root`中去，并且对当前这些存在于红黑树的节点设置回调函数，当这些被监听的数据一旦准备完成，就会被调用，而调用的结果就是将红黑树的fd添加到`list_head`中去(但是此时并没有完成)

- 当第二步完成后，就会调用`epoll_wait`函数，这个函数会去校验是否有数据准备完毕（因为数据一旦准备就绪，就会被回调函数添加到`list_head`中），在等待了一段时间后(可以进行配置)，如果等够了超时时间，则返回没有数据，如果有，则进一步判断当前是什么事件，如果是建立连接时间，则调用`accept()`接受客户端socket，拿到建立连接的socket，然后建立起来连接，如果是其他事件，则把数据进行写出

### 信号驱动IO 

信号驱动IO是与内核建立**SIGIO的信号**关联并设置回调，当内核有FD就绪时，会发出SIGIO信号通知用户，期间用户应用可以执行其它业务，无需**阻塞等待**。

<img src="https://article.biliimg.com/bfs/article/8b3c9f0e6b2a6971384805233f5807a18607d41e.png" style="zoom:67%;" />

阶段一：
* 用户进程调用`sigaction`，注册信号处理函数
* 内核返回成功，开始监听FD
* 用户进程不阻塞等待，可以执行其它业务
* 当内核数据就绪后，回调用户进程的SIGIO处理函数

阶段二：
* 收到SIGIO回调信号
* 调用`recvfrom`，读取
* 内核将数据拷贝到用户空间
* 用户进程处理数据

当有大量IO操作时，信号较多，SIGIO处理函数不能及时处理可能**导致信号队列溢出**，而且内核空间与用户空间的频繁信号交互性能也较低。

### 异步IO 

<img src="https://article.biliimg.com/bfs/article/e340cc6f459cd3709ecb438b815d8fa74bb5d823.png" alt="image.png" style="zoom:60%;" />

- 这种方式，不仅仅是用户态在试图读取数据后，不阻塞，而且当内核的数据准备完成后，也不会阻塞
- 他会由内核将所有数据处理完成后，由内核将数据写入到用户态中，然后才算完成，所以性能极高，不会有任何阻塞，全部都由内核完成，可以看到，异步IO模型中，用户进程在两个阶段都是非阻塞状态。


缺点:
1. 在高并发情况下,进程不阻塞,会持续收到用户请求,会进行很多系统调用,内核会积累很多IO读写任务,内存会占用过多,而崩溃
2. 所以在高并发要做调用的限流,代码的实现复杂度就高了

#### 对比
最后用一幅图，来说明他们之间的区别

<img src="https://article.biliimg.com/bfs/article/443d0a35430d6e9213891a52af04cbceacd31752.png" style="zoom:67%;" />



### Redis网络模型 

**Redis到底是单线程还是多线程？**
* 如果仅仅聊Redis的核心业务部分（命令处理），答案是单线程
* 如果是聊整个Redis，那么答案就是多线程

在Redis版本迭代过程中，在两个重要的时间节点上引入了多线程的支持：
* Redis v4.0：引入多线程异步处理一些耗时较旧的任务，例如异步删除命令unlink
* Redis v6.0：在核心网络模型中引入 多线程，进一步提高对于多核CPU的利用率

因此，对于Redis的核心网络模型，在Redis 6.0之前确实都是单线程。是利用epoll（Linux系统）这样的IO多路复用技术在事件循环中不断处理客户端情况。

**为什么Redis要选择单线程？**

* 抛开持久化不谈，Redis是**纯内存操作**，执行速度非常快，它的性能瓶颈是<font color=#ff0000>网络延迟</font>而不是<font color=#ff0000>执行速度</font>，因此多线程并不会带来巨大的性能提升。
* 多线程会导致过多的上下文切换，带来不必要的开销
* 引入多线程会面临<font color=#ff0000>线程安全问题</font>，必然要引入线程锁这样的安全手段，实现复杂度增高，而且性能也会大打折扣

Redis通过IO多路复用来提高网络性能，并且支持**各种不同的多路复用实现**，并且将这些实现进行封装(二次封装)， 提供了统一的高性能事件库API库 AE


<img src="https://article.biliimg.com/bfs/article/2537921c580a1559ab4ec848a8522103d4a37532.png" alt="image.png" style="zoom:80%;" />
这里的ae.文件的封装是对当前同的编译选项进行判断,选择包含不同的文件(类似一个接口,不同的重载实现)

```c
#ifdef HAVE_EVPORT
#include "ae_evport.c"
#else
    #ifdef HAVE_EPOLL
    #include "ae_epoll.c"
    #else
        #ifdef HAVE_KQUEUE
        #include "ae_kqueue.c"
        #else
        #include "ae_select.c"
        #endif
    #endif
#endif
```

#### 整体流程

```c
// server.c 
int main( 
	int argc, 
	char **argv 
) { 
	// ...
	// 初始化服务 
	initServer(); 
	// ...
	// 开始监听事件循环 
	aeMain(server.el); 
	// ..
}
```

```c
void initServer(void){ 
	//... 
	//内部会调用 aeApiCreate(eventLoop),类似epollcreate 
	server.el=aeCreateEventLoop(server.maxclients + CONFIG_FDSET_INCR); 
	// ...
	//监听TCP端口，创建ServerSocket,并得到FD 
	ListenToPort(server.port,&server.ipfd) //都是配置文件可以配置的
	// ...
	// 注册 连接处理器，内部会调用 aeApiAddEvent(&server.ipfd)监听FD 
	createSocketAcceptHandler(&server.ipfd, acceptTcpHandler) // acceptTcpHandler是对应的处理事件(web流程里的acept()),提前写好,回调
	//注册 ae_api_poll 前的处理器 
	aeSetBeforeSleepProc(server.el,beforeSleep); 
}
```

```c
void aeMain(aeEventLoop *eventLoop){ 
	eventLoop->stop = 0; 
	// 循环监听事件 
	while (!eventLoop->stop){ 
		aeProcessEvents( 
		eventLoop, 
		AE_ALL_EVENTS| 
			AE_CALL_BEFORE_SLEEP| 
			AE_CALL_AFTER_SLEEP); 
	}
}
```

```c
int aeProcessEvents( 
	aeEventLoop *eventLoop, 
	int flags) { 
	//... 调用前置处理器 beforeSleep 
	eventLoop->beforesleep(eventLoop); 
	//等待FD就绪，类似epoll_wait 
	numevents = aeApiPoll(eventLoop, tvp); //就绪的数量
	for(j=0;j<numevents;j++){ 
		//遍历处理就绪的FD,调用对应的处理器 (也就是acceptTcpHandler那个回调函数)
	}
}
```

```c
//数据读处理器 
void acceptTcpHandler(...){ 
	// ...
	// 接收socket连接，获取FD 
	fd = accept(s,sa,len); 
	// ...
	// 创建connection,关联fd 
	connection *conn = connCreateSocket(); 
	conn.fd=fd; 
	//...
	// 内部调用aeApiAddEvent(fd,READABLE), 这里就是不是ssfd,而是其他业务
	// 监听socket的FD读事件，并绑定读处理器readQueryFromCLient 
	connSetReadHandler(conn, readQueryFromClient); 
}
```

Redis 6.0版本中引入了多线程，目的是为了提高IO读写效率。因此在**解析客户端命令、写响应结果**时采用了多线程。核心的命令执行、 10多路复用模块依然是由主线程执行。 
<img src="https://article.biliimg.com/bfs/article/dbb4bcb16538e8ba4943209423081d25dc429512.png" alt="image.png" style="zoom:50%;" />


当我们的客户端想要去连接我们服务器，会去先到IO多路复用模型去进行排队，会有一个连接应答处理器，他会去接受读请求，然后又把读请求注册到具体模型中去，此时这些建立起来的连接，如果是客户端请求处理器去进行执行命令时，他会去把数据读取出来，然后把数据放入到client中， clinet去解析当前的命令转化为redis认识的命令，接下来就开始处理这些命令，从redis中的command中找到这些命令，然后就真正的去操作对应的数据了，当数据操作完成后，会去找到命令回复处理器，再由他将数据写出。

```c
void readQueryFromCLient(connection *conn) 
{ 
	//获取当前客户端，客户端中有缓冲区用来读和写 
	client *c = connGetPrivateData(conn); 
	//获取c->querybuf缓冲区大小 
	long int qblen = sdslen(c->querybuf); 
	// 读取请求数据到 c->querybuf 缓冲区 
	connRead(c->conn, c->querybuf+qblen, readlen); 
	// ...
	// 解析缓冲区字符串，转为Redis命令参数存入 c->argv 数组 
	processInputBuffer(c); 
	// ...
	//处理 c->argv 中的命令 
	processCommand(c); 
} 
int processCommand(client *c){ 
	// ...
	//根据命令名称，寻找命令对应的command,例如 setCommand 
	c->cmd = c->lastcmd = LookupCommand(c->argv[0]->ptr); 
	// ...
	//执行command,得到响应结果，例如ping命令，对应pingCommand 
	c->cmd->proc(c); 
	//把执行结果写出，例如ping命令，就返回"pong"给client, 
	// shared. pong是 字符串"pong"的SDS对象 
	addReply(c,shared.pong); 
}
void addReply(client *c, robj *obj) { 
	//尝试把结果写到c-buf客户端写缓存区 
	if (_addReplyToBuffer(c,obj->ptr,sdslen(obj->ptr))!=C_OK) 
		//如果c->buf写不下，则写到c->reply,这是一个链表，容量无上限 
		addReplyProtoToList(c,obj->ptr,sdslen(obj->ptr)); 
	//将客户端添加到server.clients_pending_write这个队列，等待被写出 
	ListAddNodeHead(server.clients_pending_write,c); 
} 
```

```c
void beforeSleep(struct aeEventLoop *eventLoop){ 
	//... 
	//定义迭代器，指向server.clients_pending_write->head; 
	ListIter Li; 
	Li->next = server.clients_pending_write->head; 
	Li->direction = AL_START_HEAD; 
	//循环遍历待写出的client 
	while ((ln=ListNext(&Li))){ 
		//内部调用aeApiAddEvent(fd,WRITEABLE),监听socket的FD读事件 
		//并且绑定写处理器sendReplyToCLient,可以把响应写到客户端socket 
		connSetWriteHandlerWithBarrier(c->conn, sendReplyToClient,ae_barrier) 
	}
}
```


多线程用在从内核缓冲区读数据到redis输入缓冲区和从redis输出缓冲区写数据到内核缓冲区

## Redis通信协议

### RESP协议

Redis是一个CS架构的软件，通信一般分两步（不包括`pipeline`和`PubSub`): 
1. 客户端（client)向服务端（server)发送一条命令 
2. 服务端解析并执行命令，返回响应结果给客户端 
因此客户端发送命令的格式、服务端响应结果的格式必须有一个**规范**，这个规范就是通信协议。 

而在Redis中采用的是**RESP**(Redis Serialization Protocol)协议： 
- Redis 1.2版本引入了RESP协议 
- Redis 2.0版本中成为与Redis服务端通信的标准，称为RESP2 
- Redis 6.0版本中，从RESP2升级到了RESP3协议，增加了更多数据类型并且支持6.0的新特性--**客户端缓存** 


在RESP中，通过首字节的字符来区分不同数据类型，常用的数据类型包括5种： 
- 单行字符串：首字节是'+’，后面跟上单行字符串，以`CRLF("\r\n")`结尾。例如返回`"OK":"+OK\r\n"` 
- 错误（Errors):首字节是'-',与单行字符串格式一样，只是字符串是异常信息，例如：`"-Error message\r\n"` 
- 数值：首字节是':',后面跟上数字格式的字符串，以CRLF结尾。例如：`":10\r\n"` 
- 多行字符串：首字节是‘$’，表示二进制安全的字符串，最大支持512MB: `$5\r\nhello\r\n` 
	- 如果大小为0,则代表空字符串：`"$0\r\n\r\n"` 
	- 如果大小为-1,则代表不存在：`"$-1\r\n"` 

<img src="https://article.biliimg.com/bfs/article/6fcd0a18918b080d5abb5baced0dac796c8940e6.png" alt="image.png" style="zoom:50%;" />


<img src="https://article.biliimg.com/bfs/article/869f8193ab3b1a5e8b8a4b6001f3700673399df0.png" alt="image.png" style="zoom:70%;" />


### 基于Socket自定义Redis的客户端

实现步骤：
1. 建立连接(客户端使用[[java基础篇#TCP通信|socket]],redis那边使用http socket)
2. 获取输出流、输入流(这里我们采用[[java基础篇#字符流|字符流]])
3. 发送请求(这里我们采用可变参数)
4. 解析响应
	- 这里的响应是看首位字符，所以读取首字节就行

### Redis内存回收

Redis之所以性能强，最主要的原因就是基于内存存储。然而单节点的Redis其内存大小不宜过大，会影响持久化或主从同步性能。
我们可以通过修改配置文件来设置Redis的最大内存：

```conf
#格式： 
# maxmemory < bytes> 
# 例如： 
maxmemory 1gb 
```

当内存使用达到上限时，就无法存储更多数据了。为了解决这个问题，Redis提供了一些策略实现内存回收：

#### 过期策略
 
在学习Redis缓存的时候我们说过，可以通过`expire`命令给Redis的key设置TTL（存活时间）：

```bash
127.0.0.1:6379> set name jack 
OK 
127.0.0.1:6379> expire name 5 # 设置ttl为5秒 
(integer) 1 
127.0.0.1:6379> get name  # 立即访问 
"jack" 
127.0.0.1:6379> get name  # 5秒后访问 
(nil) 
```

可以发现，当key的TTL到期以后，再次访问name返回的是nil，说明这个key已经不存在了，对应的内存也得到释放。从而起到内存回收的目的。

Redis本身是一个典型的key-value内存存储数据库，因此所有的key、value都保存在之前学习过的Dict结构中。不过在其database结构体中，有两个Dict：一个用来记录key-value；另一个用来记录key-TTL。

```c
typedef struct redisDb{ 
	dict *dict; /* 存放所有key及value的地方，也被称为keyspace*/ 
	dict *expires; /* 存放每一个key及其对应的TTL存活时间，只包含设置了TTL的key*/ 
	dict *blocking_keys; /* Keys with clients waiting for data (BLPOP)*/ 
	dict *ready_keys; /* Blocked keys that received a PUSH */
	dict *watched keys;  /* WATCHED keys for MULTI/EXEC CAS */ 
	int id; /* Database ID, 0~15*/ 
	long long avg_ttl; /*记录平均TTL时长*/ 
	unsigned long expires_cursor; /* expire检查时在dict中抽样的索引位置.*/ 
	list *defrag_later; /* 等待碎片整理的key列表. */ 
}redisDb; 
```

<img src="https://article.biliimg.com/bfs/article/43d36c18b35b6ec37cc566814c82ec3d5369245b.png" alt="1653983606531" style="zoom:67%;" />

这里有两个问题需要我们思考：
1. Redis是如何知道一个key是否过期呢？
	- 利用两个Dict分别记录key-value对及key-ttl对
2. 是不是TTL到期就立即删除了呢？
- **惰性删除**
	惰性删除：顾明思议并不是在TTL到期后就立刻删除，而是在**访问一个key的时候，检查该key的存活时间**，如果已经过期才执行删除。

	```c
	//查找一个key执行写操作 
	robj *LookupKeyWritewithFlags(redisDb *db, robj *key, int flags) 
	{ 
		//检查key是否过期 
		expireIfNeeded(db, key); 
		return lookupKey(db, key,flags); 
	}
	//查找一个key执行读操作 
	robj * LookupKeyReadwithFlags(redisDb *db , robj *key , int flags) 
	{ 
		robj *val; 
		//检查key是否过期 
		if (expireIfNeeded(db,key)==1){ 
			//...略 
		} 
		return lookupKey (db,key,flags); 
	} 
	```

	```c
	int expireIfNeeded(redisDb *db, robj *key) { 
		//判断是否过期，如果未过期直接结束并返回。 
		if(!keyIsExpired(db,key)) return 0; 
		// ... 略 
		// 删除过期key 
		deleteExpiredKeyAndPropagate(db,key); 
		return 1; 
	}
	```


- **周期删除**：顾明思议是通过一个定时任务，周期性的**抽样部分过期的key**,然后执行删除。执行周期有两种： 
	- Redis会设置一个定时任务`serverCron()`,按照server.hz的频率来执行过期key清理，模式为SLOW 
	- Redis的每个事件循环前会调用`beforeSleep()`函数，执行过期key清理，模式为FAST 

	```c
	//server.c 
	void initServer(void){ 
		//.. 
		//创建定时器，关联回调函数serverCron,处理周期取决于server.hz,默认10 
		aeCreateTimeEvent(server.el, 1, serverCron, NULL, NULL) 
	}
	
	// server.c 
	int serverCron(struct aeEventLoop *eventLoop, long long id, void *clientData) { 
		// 更新 Lruclock到当前时间，为后期的LRU和LFU做准备 
		unsigned int lruclock = getLRUClock(); 
		atomicSet(server.lruclock,lruclock); 
		// 执行database的数据清理，例如过期key处理 
		databasesCron();
		return 1000/server.hz; 
	}
	
	void databasesCron(void) { 
		//尝试清理部分过期key,清理模式默认为SLOW 
		activeExpireCycle(ACTIVE_EXPIRE_CYCLE_SLOW); 
	}
	
	void beforesleep ( struct aeEventLoop *eventLoop){ 
		//尝试清理部分过期key,清理模式默认为FAST 
		activeExpireCycle( 
		ACTIVE_EXPIRE_CYCLE_FAST); 
	}
	
	void aeMain(aeEventLoop *eventLoop) { 
		eventLoop->stop=0; 
		while(!eventLoop->stop ) { 
			// beforeSleep()--> Fast模式清理 
			// n = aeApiPoll() 
			// 如果n>0,FD就绪，处理IO事件 
			// 如果到了执行时间，则调用serverCron()-->SLOW模式清理 
		}
	}
	```


	**SLOW模式规则**： 
	1. 执行频率受`server.hz`影响，默认为10,即每秒执行10次，每个执行周期100ms。 
	2. 执行清理耗时不超过一次执行周期的25%. 
	3. 逐个遍历db,逐个遍历db中的bucket(桶 ),抽取20个key判断是否过期 
	4. 如果没达到时间上限(25ms)并且过期key比例大于10%,再进行一次抽样，否则结束 

	**FAST模式规则**（过期key比例小于10%不执行）: 
	1. 执行频率受`beforeSleep()`调用频率影响，但两次FAST模式间隔不低于2ms 
	2. 执行清理耗时不超过1ms 
	3. 逐个遍历db,逐个遍历db中的`bucket`,抽取20个key判断是否过期 
	4. 如果没达到时间上限（1ms)并且过期key比例大于10%,再进行一次抽样，否则结束 


小总结：

RedisKey的TTL记录方式：
- 在RedisDB中通过一个Dict记录每个Key的TTL时间
过期key的删除策略：
- 惰性清理：每次查找key时判断是否过期，如果过期则删除
- 定期清理：定期抽样部分key，判断是否过期，如果过期则删除。
定期清理的两种模式：
- SLOW模式执行频率默认为10，每次不超过25ms
- FAST模式执行频率不固定，但两次间隔不低于2ms，每次耗时不超过1ms

#### 淘汰策略

内存淘汰：就是当Redis内存使用**达到设置的上限**时，**主动挑选部分key删除**以释放更多内存的流程。Redis会在处理客户端命令的方法`processCommand()`中尝试做内存淘汰：

```c
int processCommand(client *c){ 
	// 如果服务器设置了server.maxmemory属性，并且并未有执行lua脚本 
	if (server.maxmemory &&!server.lua_timedout){ 
		// 尝试进行内存淘汰performEvictions 
		int out_of_memory =(performEvictions()==EVICT_FAIL); 
		// ... 
		if (out_of_memory&&reject_cmd_on_oom) { 
			rejectCommand(c,shared.oomerr); 
			return C_OK ; 
		}
	}
}
```


淘汰策略
Redis支持8种不同策略来选择要删除的key：
- `noeviction`： 不淘汰任何key，但是内存满时不允许写入新数据，默认就是这种策略。
- `volatile-ttl`： 对设置了TTL的key，比较key的剩余TTL值，TTL越小越先被淘汰
- `allkeys-random`：对全体key ，随机进行淘汰。也就是直接从db->dict中随机挑选
- `volatile-random`：对设置了TTL的key ，随机进行淘汰。也就是从db->expires中随机挑选。
- `allkeys-lru`： 对全体key，基于LRU算法进行淘汰
- `volatile-lru`： 对设置了TTL的key，基于LRU算法进行淘汰
- `allkeys-lfu`： 对全体key，基于LFU算法进行淘汰 
- `volatile-lfu`： 对设置了TTL的key，基于LFI算法进行淘汰 比较容易混淆的有两个：
    - `LRU（Least Recently Used）`，最少最近使用。用当前时间减去最后一次访问时间，这个值越大则淘汰优先级越高。
    - `LFU（Least Frequently Used`），最少频率使用。会统计每个key的访问频率，值越小淘汰优先级越高。

Redis的数据都会被封装为RedisObject结构：

```c
typedef struct redisObject { 
	unsigned type:4;    //对象类型 
	unsigned encoding:4;  //编码方式 
	unsigned lru:LRUBITS; //LRU:以秒为单位记录最近一次访问时间，长度24bit 
						 //LFU:高16位以分钟为单位记录最近一次访问时间，低8位记录逻辑访问次数 
	int refcount;     //引用计数，计数为0则可以回收 
	void *ptr;       //数据指针，指向真实数据 
}robj; 
```

LFU的访问次数之所以叫做**逻辑访问次数**，是因为并不是每次key被访问都计数，而是通过运算：
1. 生成0~1之间的随机数R
2. 计算 (旧次数 * lfu_log_factor + 1)，记录为P
3. 如果 R<P ，则计数器 + 1，且最大不超过255(==如果频繁访问某个key,那么R<P的概率就很小了,所以频率不是单单的次数==)
4. 访问次数会随时间衰减，距离上一次访问时间每隔lfu_decay_time分钟(默认1)，计数器 -1



一副图来描述当前的这个流程吧
<img src="https://article.biliimg.com/bfs/article/aaac0cb487b6a544044d20cfa4e559eebf7d031d.png" style="zoom:80%;" />
eviction_pool是一个淘汰池,开始是空的,因为我们不可能遍历所有的key去比较它们的LRU或者LFU,所以这里用到**局部抽样的方法**,在这个局部样本种进行比较,这样性能比较好

因为判断策略,防止后面写的判断逻辑过多,必须统一判断标准,会根据key进行一个**升序排列**,值越大的优先淘汰

判断是否可以存入eviction_pool这步会逐渐放入最应该被放入的,随着时间推移,eviction_pool里面它会越来越像真实的LRU或者LFU 

