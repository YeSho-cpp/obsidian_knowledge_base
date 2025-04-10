
## 数据库相关概念

**没有数据库的日子**
应用程序直接操作底层文件
- 利用文件系统将数据持久化
- 自己设计并发控制
- 自己设计故障恢复
- 自己定义文件格式和访问方式
	- 应用来设计数据的逻辑结构
	- 应用来设计数据物理结构，存储结构，存取方式，输入方法。

**<font color=#ff0000>数据库</font>** ^db
- 存储数据的**仓库**，数据是有组织的进行存储 一个以某种**组织的方式**存储的数据集合,保存数据的仓库。它体现我们电脑中，就是一个==软件或者文件系统==。然后把数据都保存这些特殊的文件中，并且需要使用固定的语言（**SQL语言/语句**）去**操作**文件中的数据。
- 英文：DataBase，简称`DB`
<font color=#ff0000>模式(schema)</font>
- 关于数据库和表的布局及特性的信息。
- 用来描述数据库中特定的表以及整个数据库（和其中表的关系)。跟数据库同义。
<font color=#ff0000>数据库管理系统</font>
- 管理数据库的大型软件 
- 英文：DataBase Management System，简称 `DBMS`
<font color=#ff0000>SQL</font>
- 英文：Structured Query Language，简称 `SQL`，结构化查询语言 
- 操作关系型数据库的编程语言 
- 定义操作所有关系型数据库的统一标准

<img src="https://article.biliimg.com/bfs/article/70176566936096b417b5ebd8163f440cc74b0704.png" alt="image.png" style="zoom:70%;" />

<font color=#ff0000>主键(primary key)</font>：一列（或一组列)，其值能够唯一区分表中每个行，其值只能是非NULL值。

### 数据的存储方式
需求：开发一个学生选课系统，学生系统中含有学生信息、老师信息、课程信息。需要将关系和数据进行保存，保存到哪里合适呢？
1. **数据保存在内存**

     ```java
     Student s = new Student("张三",18,"上海");  
     Teacher t = new Teacher("锁哥",19,"上海");  
     Course c = new Course("语文",90);
    ```
   new出来的对象存储在堆中.堆是内存中的一小块空间
   优点：存储速度快 
   缺点：断电/程序退出,数据就清除了
1. **数据使用IO流技术保存在硬盘的普通文件中** 优点：永久保存 缺点：IO流的查找，增加，修改，删除数据比较麻烦。同时使用IO流技术需要频繁调用系统资源和将系统资源还给系统，这样操作效率比较低

2. **数据保存在数据库** 优点：永久保存,通过SQL语句比较方便的操作数据库。是方式一和方式二的结合。可以解决上述两种方式的缺点。
### 数据库的优点

数据库是按照特定的格式将数据存储**在文件中**，通过SQL语句可以方便的对大量数据进行增、删、改、查操作，数据库是对大量的信息进行管理的高效的解决方案。

### 常见数据库

我们开发应用程序的时候，程序中的所有数据，最后都需要保存到专业软件中。这些专业的保存数据的软件我们称为数据库。我们学习数据库，并不是学习如何去开发一个数据库软件，我们学习的是如何使用数据库以及数据库中的数据记录的操作。而数据库软件是由第三方公司研发。

**Oracle**：它是Oracle公司的大型关系型数据库。系统可移植性好、使用方便、功能强，适用于各类大、中、小、微机环境。它是一种高效率、安全可靠的。但是它是收费的。
**MYSQL**：早期由瑞典一个叫MySQL AB公司开发的，后期被sun公司收购，再后期被Oracle收购。体积小、速度快、总体拥有成本低，尤其是[开放源码](https://baike.baidu.com/item/%E5%BC%80%E6%94%BE%E6%BA%90%E7%A0%81)这一特点，一般中小型网站的开发都选择 MySQL 作为网站数据库。MySQL6.x版本也开始收费。
**DB2** ：IBM公司的数据库产品,收费的。常应用在银行系统中.
**SQLServer**：MicroSoft 公司收费的中型的数据库。C#、.net等语言常使用。
**SyBase**：Sybase公司的。 已经淡出历史舞台。提供了一个非常专业数据建模的工具PowerDesigner。

**常用数据库**：Java开发应用程序主要使用的数据库：MySQL（5.6）、Oracle、DB2。
在web应用中，使用的最多的就是MySQL数据库，原因如下：

1. 开源、免费
2. 功能足够强大，足以应付web应用开发（最高支持千万级别的并发访问）
### 客户机-服务器

MySQL、Oracle以及Microsoft sQL Server等数据库是基于**客户机-服务器的数据库**。客户机-服务器应用分为两个不同的部分。服务器部分是负责所有数据访问和处理的一个==软件==。这个软件运行在称为数据库服务器的计算机上。
与**数据文件打交道**的只有服务器软件。关于数据、数据添加、删除和数据更新的所有请求都由服务器软件完成。这些==请求或更改==来自运行客户机软件的计算机。客户机是与用户打交道的软件。
例如，如果你请求一个按字母顺序列出的产品表，则客户机软件通过网络提交该请求给服务器软件。服务器软件处理这个请求，根据需要过滤、丢弃和排序数据;然后把结果送回到你的客户机软件。
### 关系型数据库

在开发软件的时候，软件中的数据之间必然会有一定的**关系**存在。比如商品和客户之间的关系，一个客户是可以买多种商品，而一种商品是可以被多个客户来购买的。

需要把这些数据保存在数据库中，同时也要维护数据之间的关系，这时就可以直接使用上述的那些数据库。而上述的所有数据库都属于关系型数据库。

**关系型数据**：设计数据库的时候，需要使用`E-R`实体关系图来描述。

E-R 是两个单词的首字母，E表示`Entity`实体 R表示`Relationship`关系。

**实体**：可以理解成我们Java程序中的一个对象。比如商品，客户等都是一个实体对象。在E-R图中使用`矩形`(长方形) 表示。
**属性**：实体对象中是含有属性的，比如商品名、价格等。针对一个实体中的属性，我们称为这个实体的数据，在E-R图中使用`椭圆`表示。
**关系**：实体和实体之间的关系：在E-R图中使用`菱形`表示。
**需求：** 使用E-R图描述 客户、商品、订单之间的关系。

<img src="https://article.biliimg.com/bfs/article/5b8f8ccdf7466c749e08f20db2fb816618ddd897.png" alt="image.png" style="zoom:60%;" />
### 使用mysql
#### 连接mysql
<img src="https://article.biliimg.com/bfs/article/1ed5c2c9d5cafcdd4a17078f35622be6b3074bde.png" alt="image.png" style="zoom:60%;" />

MySQL是一个需要账户名密码登录的数据库，登陆后使用，它提供了一个默认的root账号，使用安装时设置的密码即可登录

1.  登录格式1：`mysql -u用户名 -p密码` 例如：
     mysql -uroot -p1234
    在打开的dos窗口中输入mysql -uroot -p命令：
    后输入密码方式：
	```shell
	mysql -u root -p
	下一行输入密码
	```
2. 登录格式2：
	**mysql** **[-h 连接的主机ip -P端口3306]** **-u 用户名 -p 密码**
	例如：
	```shell
	mysql -h 192.168.1.251 -P 3306 -u root -p 1234 
	```

#### mysql表的关系
**实体类与表的对应关系**
<img src="https://article.biliimg.com/bfs/article/db952ed20b19c5e077850258e61b7e65f073bbd2.png" alt="image.png" style="zoom:50%;" />
<img src="https://article.biliimg.com/bfs/article/82371bea2228e5d89c205e3ea9caf8789ea25e3c.png" alt="image.png" style="zoom:50%;" />
说明：

1、一个数据库软件可以安装多个数据仓库，数据仓库可以简称为数据库，在数据库中创建数据表来保存数据。
2、数据库的一行称为记录，可以理解成java实例化后的一个对象。
3、数据库的一列称为字段，理解成java类中的属性。
4、一个数据仓库中是可以有多张表的。

未来关于数据库我们学习的目标：

1）对数据库进行增删改查(CRUD);(主要对整个数据库进行操作，比如删除一个数据库和增加一个数据库)。create(增) read(查) update(改) delete(删)
2）对数据表结构进行增删改查(CRUD);(主要对整张表，比如删除一张表和增加一张表)。
3）**对表中的数据进行增删改查(CRUD);(主要对表中的具体数据，比如删除一个表中的一行记录和增加一行数据)。**

注意：关于对数据库表的操作，查询是开发中最难的，也是最重要的；

## SQL语句的分类和语法
### 什么是SQL

`Structured Query Language`结构化查询语言。SQL语句不依赖于任何平台，对所有的数据库是通用的。学会了SQL语句的使用，可以在任何的数据库使用,但都有特有内容。SQL语句功能强大、[简单](http://baike.baidu.com/subview/66543/5094958.htm)易学、使用方便。

### SQL特点

SQL语句是一个非过程性的语言，每一条SQL执行完都会有一个具体的结果出现。多条语句之间没有影响。

过程性语言：例如java。

int a = 10;
int b = 20;
int sum = a +b;

### SQL作用

SQL语句主要是操作数据库，数据表，**数据表中的数据记录**。

### SQL语句分类

SQL是用来存取关系数据库的语言，具有定义、操纵、控制和查询关系型数据库的四方面功能。所以针对四方面功能，我们将SQL进行了分类。

1.  <font color=#ff0000>DDL</font>(Data Definition Language)数据定义语言 用来定义数据库对象：数据库，表，列等。关键字：create drop alter truncate(清空数据记录) show等定义是对表的结构，例如字段名，类型。
2.  <font color=#ff0000>DML</font>(Data Manipulation Language)数据操作语言★★★
    在数据库表中更新，增加和删除记录。如 update(更新),insert(插入),delete(删除) 不包含查询。操作时对表的参数的进行修改，就是表的每一行数据。
3.  <font color=#ff0000>DQL</font>(Data Query Language) 数据查询语言★★★★★ 数据表记录的查询。关键字select。
4.  <font color=#ff0000>DCL</font>(**Data Control Language**)数据控制语言(了解)
    
    是用来设置或更改数据库用户或角色权限的语句，如grant(设置权限)，revoke(撤销权限)，begin transaction等。这个比较少用到。
    <img src="https://article.biliimg.com/bfs/article/759d2ff622f61f45f3ff7e9474cc1f0724c08db9.png" alt="image.png" style="zoom:50%;" />
### MySQL数据类型
MySQL中的我们常使用的数据类型如下：

| 类型 | 描述 |
| :-: | :-: |
|`int`| 整型 |
|`bigint`|整型(-9223372036854775808到9223372036854775807)|
| `double` | 浮点型 |
| `varchar` | 字符串型 |
| `date` | 日期类型，格式为yyy-MM-dd，只有年月日，没有时分秒 |
详细的数据类型如下(不建议详细阅读！)

<img src="https://article.biliimg.com/bfs/article/ec3e2b71f3cd31bd242d4cae1cc3342ffdc77eeb.png" alt="image.png" style="zoom:60%;" />

具体操作: 

创建student表包含id,name,birthday字段

```sql
-- 需求1：创建学生表
create table stu(
	-- 编号
	id int,
	name varchar(10), -- 10表示姓名的值最多只能是10个字符
	sex char(1), -- 1表示性别是1个字符
	birthday date,
	score double(5,2), -- 5 表示长度最多是5   2 表示小数点保留2位，即位数 3.14
	email varchar(64),
	tel varchar(20),
	-- 学生状态,tinyint 表示微整型 
	-- status 0 表示正常 1 休学 2 毕业
	status tinyint
);
```

### DDL

| 操作类型  |           大概命令           |
| :---: | :----------------------: |
| 创建数据库 |  `create database XXX`   |
| 删除数据库 |   `drop database XXX`    |
| 切换数据库 |        `use XXX`         |
| 查看数据库 |     `SHOW DATABASES`     |
| 修改数据库 | `ALTER DATABASE 数据库 XXX` |
| 使用数据库 |         `使用数据库`          |
#### 创建数据库

1.  直接创建数据库
    
    ```sql
    create database [ if not exists ] 数据库名 [ default charset 字符集 ] [ collate 排序规则 ];
    ```
    
2. 判断是否存在并创建数据库

   ```sql
   CREATE DATABASE IF NOT EXISTS 数据库名;
   ```

3. 创建数据库并指定字符集(编码表)

   ```sql
   CREATE DATABASE 数据库名 character set 字符集;
   说明：字符集就是编码表名。在mysql中utf8 latin1 
   ```

4. 具体操作：
* 直接创建数据库db1
  ```sql
  CREATE DATABASE db1;
  ```

  ![直接创建数据库](https://article.biliimg.com/bfs/article/0568eb31368b04764bf234913572b24d153083c3.png)

* 判断是否存在并创建数据库db2

  ```sql
  CREATE DATABASE IF NOT EXISTS db2;
  ```

  ![判断是否存在并创建数据库](https://article.biliimg.com/bfs/article/741048e353a0e653a4ec90d255af22032bc7bb69.png)

* 创建数据库db3并指定字符集为gbk

  ```sql
  CREATE DATABASE db2 CHARACTER SET gbk;
  ```

  ![创建数据库并指定字符集](https://article.biliimg.com/bfs/article/e9b6bffc77296d6289192b6f7d00cdd332b5c756.png)

#### 查看数据库

1. 查看所有的数据库

```sql
SHOW DATABASES;
```

![查看所有数据库](https://article.biliimg.com/bfs/article/faf5e803dd52d07e5ecceab4c65ebd7058e33039.png)

2. 查看某个数据库的定义信息

```sql
SHOW CREATE DATABASE 数据库名;
```

   ![查看某个数据库的定义信息](https://article.biliimg.com/bfs/article/4fe8d0df0dae2cb01e1c057d284f2e396a9b924d.png)

#### DDL修改和删除数据库
##### 修改数据库字符集

```mysql
ALTER 表示修改：
ALTER DATABASE 数据库 default character set 新的字符集;
```
具体操作：

* 将db3数据库的字符集改成utf8

  ```sql
  ALTER DATABASE db3 DEFAULT CHARACTER SET utf8;
  ```

  注意：如果修改数据库指定的编码表是utf8,记住不能写utf-8。utf-8是错误的(mysql不认识utf-8)。
  java中的常用编码        :      UTF-8; GBK;GB2312;ISO-8859-1;
  对应mysql数据库中的编码: utf8;  gbk;gb2312; latin1;

  ![修改数据库字符集](https://article.biliimg.com/bfs/article/1c856b478d711a012a017c55ccde82261b9c7032.png)
##### 删除数据库
```mysql
drop --表示删除数据库或表
DROP DATABASE 数据库名;
```
具体操作：
-  删除db2数据库
  ```mysql
    DROP DATABASE db2;
  ```


<img src="https://article.biliimg.com/bfs/article/7bb8fe10c0d8d86b702be0fb8aa62c99fdd0b084.png" alt="image.png" style="zoom:80%;" />

#### DDL使用数据库
##### 1.查看正在使用的数据库

```mysql 
 select -- 查询  
 SELECT DATABASE();
```

##### 2.使用/切换数据库

```sql
USE 数据库名;
```

具体操作：

-  查看正在使用的数据库
    
    ```mysql
     SELECT DATABASE();
    ```
    ![查看正在使用的数据库](https://article.biliimg.com/bfs/article/77efcfcd9e40223fba9872d5839df5248c1ac9d5.png)
    
-   使用db1数据库
    ```mysql 
     USE db1;
    ```
    ![使用db1数据库](https://article.biliimg.com/bfs/article/b4bdb45507de9da3c960f4eb04c3d8e7b4ce5af5.png)
#### DDL表操作
##### DDL创建表

**创建表**

```sql
CREATE TABLE 表名 (字段名1 字段类型1, 字段名2 字段类型2...);
```

建议写成如下格式:

```sql
public class Student{
	int age;
}
CREATE TABLE 表名 (
    字段名1 字段类型1, 
    字段名2 字段类型2
);
最后一个字段不加逗号
```

关键字说明：

```sql
CREATE -- 表示创建
TABLE -- 表示表
```


##### DDL查看表

1. 查看某个数据库中的所有表
   ```sql
   SHOW TABLES;
   ```
2. 查看表结构
   ```sql
   DESC 表名;
   ```
3. 查看创建表的SQL语句
   ```sql
   SHOW CREATE TABLE 表名;
   ```
  
  具体操作：
  
* 查看mysql数据库中的所有表

  ```sql
  SHOW TABLES;
  ```

  ![查看某个数据库中的所有表](https://article.biliimg.com/bfs/article/010dd83b15286dd441dcadf68706634665b5c230.png)

* 查看student表的结构

  ```sql
  DESC student;
  ```

  ![查看student表的结构](https://article.biliimg.com/bfs/article/63a774e18d141e992e1260a714be97d2b4242907.png)

* 查看student的创建表SQL语句

  ```sql
  SHOW CREATE TABLE student;
  ```

  <img src="https://article.biliimg.com/bfs/article/07336a4522923f03073a9b75e9552f078193e09b.png" alt="image.png" style="zoom:70%;" />

#### DDL删除表

**快速创建一个表结构相同的表**

```sql
CREATE TABLE 表名 LIKE 其他表;
```

具体操作：

- 创建s1表，s1表结构和student表结构相同

  ```sql
  CREATE TABLE s1 LIKE student;
  ```

**删除表**

1. 直接删除表

   ```sql
   DROP TABLE 表名;
   ```

2. 判断表是否存在并删除表

   ```sql
   DROP TABLE IF EXISTS 表名;
   ```

具体操作：

* 直接删除表s1表

  ```sql
  DROP TABLE s1;
  ```

  ![直接删除表](https://article.biliimg.com/bfs/article/dd6dfe3a95c55c2aa686f3352b7ac1386e44c63c.png)

* 判断表是否存在并删除s1表

  ```sql
  DROP TABLE IF EXISTS s1;
  ```

  ![判断表存在并删除](https://article.biliimg.com/bfs/article/52c3984729f087937c24c40fb20b016d9250db47.png)

#### DDL修改表结构(了解)

> 修改表结构使用不是很频繁，只需要了解，等需要使用的时候再回来查即可

1. 添加表列

   ```sql
   ALTER TABLE 表名 ADD 字段名/列名 类型;
   ```

   具体操作：

   * 为学生表添加一个新的字段remark,类型为varchar(20)

     ```sql
     ALTER TABLE student ADD remark VARCHAR(20);
     ```

     ![添加字段](https://article.biliimg.com/bfs/article/c0bfd668764fbd43e95b648a8bf19acc2cea6e4e.png)

2. 修改列类型

   ```sql
   ALTER TABLE 表名 MODIFY 字段名 新的类型;
   ```
   具体操作：
   * 将student表中的remark字段的改成varchar(100)

     ```sql
     ALTER TABLE student MODIFY remark VARCHAR(100);
     ```

     ![修改字段类型](https://article.biliimg.com/bfs/article/1450218ce64f49f5bdbd682cc2d41c1d52c43ac9.png)

3. 修改列名

   ```sql
   ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型;
   ```

   具体操作：
   * 将student表中的remark字段名改成intro，类型varchar(30)
     ```sql
     ALTER TABLE student CHANGE remark intro varchar(30);
     ```

     ![修改表字段名称](https://article.biliimg.com/bfs/article/5260a1f2e11ad5d9939ca1932016c17fa7b3d423.png)
4. 删除列

   ```sql
   ALTER TABLE 表名 DROP 字段名;
   ```
   具体操作：
   * 删除student表中的字段intro
     ```sql
     ALTER TABLE student DROP intro;
     ```

     ![删除字段](https://article.biliimg.com/bfs/article/465f080b94f81ff22ea57708dd17c8a00d0b7ab7.png)

5. 修改表名
   ```sql
   RENAME TABLE 旧表名 TO 新表名;
   ```
   具体操作：
   * 将学生表student改名成student2.

     ```sql
      RENAME TABLE student TO student2;
     ```

     ![修改表名](https://article.biliimg.com/bfs/article/43236d63582a0721416a7aaf1a2035a30274ee3b.png)

6. 修改字符集
   ```sql
   ALTER TABLE 表名 character set 字符集;
   ```
   具体操作：
   * 将sutden2表的编码修改成gbk
     ```sql
     ALTER TABLE student2 character set gbk;
     ```
      ![修改字符集](https://article.biliimg.com/bfs/article/966f6392e6994d61e0c69699eaf2fbcb61605afd.png)

### DML

#### DML插入记录(重点)

创建student表包含id,name,age,birthday,sex,address字段。

```sql
CREATE TABLE student (
      id INT,
      name VARCHAR(20),
      age int,
      birthday DATE,
      sex char(2),
      address varchar(50)
);
```

**插入全部字段**

* 所有的字段名都写出来

  ```sql
  INSERT -- 表示往表里插入记录
  INSERT INTO 表名 (字段名1, 字段名2...) VALUES (字段值1, 字段值2...);
  ```

* 不写字段名

  ```sql
  INSERT INTO 表名 VALUES (字段值1, 字段值2...);
  ```

**插入部分数据**

```sql
INSERT INTO 表名 (字段名1, 字段名2...) VALUES (字段值1, 字段值2...);
```

说明：插入部分数据的时候，要求列名一定书写出来。

**批量插入数据**

~~~sql
INSERT INTO 表名 values(字段值1, 字段值2...),(字段值1, 字段值2...),(字段值1, 字段值2...);
~~~

没有添加数据的字段会使用NULL

1. 关键字说明

   ```sql
   INSERT INTO 表名 – 表示往哪张表中添加数据
   (字段名1, 字段名2, …)  --  要给哪些字段设置值
   VALUES (值1, 值2, …); -- 设置具体的值
   注意：使用select * from 表名 --查看该表的所有信息。
   ```

2. 具体操作:

   * 插入部分数据，往学生表中添加 id, name, age, sex数据

   ```sql
   INSERT INTO student (id, name, age, sex) VALUES (1, '张三', 20, '男');
   ```

   ![添加部分数据](https://article.biliimg.com/bfs/article/c973d39afe371ae9330fffee9249c170b644c86d.png)

   * 向表中插入所有字段
     * 所有的字段名都写出来
     ```sql
      INSERT INTO student (id, name, age, sex, address) VALUES (2, '李四',  23, '女', '广州');
     ```

     ![所有字段都添加数据](https://article.biliimg.com/bfs/article/b49d1f387aec3361bd3505ce9bc49168c88143a7.png)

     * 不写字段名
     ```sql
     INSERT INTO student VALUES (3, '王五', 18, '男', '北京');
     ```

     ![添加所有字段数据](https://article.biliimg.com/bfs/article/0c98b78e80292ea457d1deaaf49d17c733d75503.png)

     批量插入数据

     ```sql
     INSERT INTO student VALUES (4, '赵六', 18, '男', '上海'),(5, '田七', 20, '男', '杭州');
     ```

**注意**

> - 值与列一一对应。有多少个列，就需要写多少个值。如果某一个列没有值。可以使用null。表示插入空。
> - 值的数据类型，与列被定义的数据类型要相匹配。并且值的长度，不能够超过定义的列的长度。
> - 字符串：插入字符类型的数据，建议写英文单引号括起来。在mysql中，使用单引号表示字符串
> - date 时间类型的数据也得使用英文单引号括起来： 如’yyyy-MM-dd’


**小结**

1. 向表中添加一条完整记录：
所有的字段都写出来: INSERT INTO 表名 (字段1, 字段2, ....) VALUES (值1, 值2, ...);

```
insert into student(id,name,birthday,sex,address) values(1,"幂幂",'2000-10-10','女','上海');
```

**2.不写字段名: INSERT INTO 表名 VALUES  (值1, 值2, ...);** 开发使用

```
insert into student values(3,'冰冰',null,'女','北京');
```

3.向表中添加一条记录部分列：必须写字段名,否则不知道添哪个字段.
INSERT INTO 表名 (字段1, 字段2, ....) VALUES (值1, 值2, ...);

```
insert into student(id,name,address) values(2,'岩岩','湖南');
```

4.批量插入数据

~~~
INSERT INTO 表名 values(字段值1, 字段值2...),(字段值1, 字段值2...),(字段值1, 字段值2...)
~~~

#### DML更新表记录

**讲解**

1. 不带条件修改数据

   ```sql
   UPDATE 表名 SET 字段名=新的值,字段名=新的值,..;
   ```

2. 带条件修改数据

   ```sql
   UPDATE 表名 SET 字段名=新的值,字段名=新的值,.. WHERE 条件
   ```

3. 关键字说明

   ```sql
   UPDATE: 表示修改记录 
   SET: 要改哪个字段
   WHERE: 设置条件
   ```

4. 具体操作：

   * 不带条件修改数据，将所有的性别改成女

     ```sql
     UPDATE student SET sex='女';
     ```

     ![修改所有数据](https://article.biliimg.com/bfs/article/4f50618d5ddec7f66d8da44447b9e9cd98cf6ad2.png)

   * 带条件修改数据，将id号为2的学生性别改成男
     ```sql
     UPDATE student SET sex='男' WHERE id=2;
     ```

     ![带条件修改](https://article.biliimg.com/bfs/article/a62244ed8dd8bec4bbdc3dd7f3d8d9953e2c8e30.png)

   * 一次修改多个列，把id为3的学生，年龄改成26岁，address改成北京

     ```sql
     UPDATE student SET age=26, address='北京' WHERE id=3;
     ```

     ![一次性修改2个字段](https://article.biliimg.com/bfs/article/9ffa0c4ab2167e847b3c627aa8b041cb9be580ce.png)

**小结**

1. 不带条件的更新数据库记录：UPDATE 表名 SET 字段名=新的值;
2. 带条件：UPDATE 表名  SET 字段名=新的值 WHERE 条件;

#### DML删除表记录

**讲解**

1. 不带条件删除数据

   ```sql
   DELETE -- 删除记录
   DELETE FROM 表名;
   表还在，可以操作，只是删除数据。
   ```

2. 带条件删除数据

   ```sql
   DELETE FROM 表名 WHERE 条件;
   ```

3. truncate删除表记录 属于DDL

   ```sql
   TRUNCATE TABLE 表名;
   ```

   >truncate和delete的区别：
   >
   >* delete是将表中的数据一条一条删除
   >* truncate是将整个表摧毁，重新创建一个新的表,新的表结构和原来表结构一模一样
   >  ![truncate](https://article.biliimg.com/bfs/article/f896c83cb1076a2f39f87c25d85e76ed937d9253.png)

4. 具体操作：

   * 带条件删除数据，删除id为3的记录

     ```sql
     DELETE FROM student WHERE id=3;
     ```

     ![删除满足条件的记录](https://article.biliimg.com/bfs/article/8be7d860458254f8225bfb1a0645e1901207d7b2.png)

   * 带条件删除数据，删除id为1和2的记录

   ~~~sql
   DELETE FROM student WHERE id in(1,2);
   ~~~

   

   * 不带条件删除数据,删除表中的所有数据

     ```sql
     DELETE FROM student;
      ```

     <img src="https://article.biliimg.com/bfs/article/c0b34201a27b127768b6b1a9c62ddfde5132b86c.png" alt="image.png" style="zoom:60%;" />

**小结**

1. 指定条件删除：DELETE FROM 表名 WHERE 条件;
2. 没有条件删除所有的记录：DELETE FROM 表名;
3. 删除表结构再创建表：TRUNCATE TABLE 表名;

### DQL

#### 基础查询

1. 查询多个字段   

   ```sql
   SELECT 字段列表 FROM 表名;
   SELECT * FROM 表名;  -- 查询所有数据
   -- 查询student表中的多个字段
   SELECT * FROM student
   -- 写出查询每列的名称
   SELECT id, name ,age, sex, address FROM student;
   ```

2. 去除重复记录

   ```sql
   SELECT DISTINCT 字段名,字段名,.. FROM 表名;
   -- 查询address列并且结果不出现重复的address
   SELECT DISTINCT address 城市 FROM student;
   ```

3. 查询时给列、表指定别名需要使用AS关键字

   ```sql
   SELECT 字段名1 AS 别名, 字段名2 AS 别名... FROM 表名;
   SELECT 字段名1 AS 别名, 字段名2 AS 别名... FROM 表名 AS 表别名;
   AS: AS 也可以省略
   -- 查询sudent表中name 和 age 列，name列的别名为”姓名”，age列的别名为”年龄”
   SELECT NAME AS 姓名, age AS 年龄 FROM student;
   ```

#### 	条件查询

1. 条件查询语法

   ```sql
   SELECT 字段列表 FROM 表名 WHERE 条件列表;
   ```

2. 条件

| 符号 | 功能 |
|:-:|:-:|
|`<>` 或 `=` |不等于或等于|
| `BETWEEN ...AND ...` |在某个范围之内(都包含)|
| `IN(...)` | 多选一 |
|`LIKE`占位符 |模糊查询 `_`单个任意字符 `%%`多个任意字符 |
|`IS NULL`|是NULL|
|`IS NOT NULL`| 不是NULL |
|`AND 或 &&` |  |
|`OR`|或者|
|`NOT 或 !`|非，不是|

1.查询math分数大于80分的学生

```sql
SELECT * FROM student3 WHERE math>80;
```

2.查询english分数小于或等于80分的学生

```sql
SELECT * FROM student3 WHERE english<=80;
```

3.查询age等于20岁的学生

```sql
SELECT * FROM student3 WHERE age=20;
```

4.查询age不等于20岁的学生

```sql
SELECT * FROM student3 WHERE age!=20;
SELECT * FROM student3 WHERE age<>20;
```

5.查询age大于35且性别为男的学生(两个条件同时满足)

```sql
SELECT * FROM student3 WHERE  age>35 AND sex='男';
```

6.查询age大于35或性别为男的学生(两个条件其中一个满足)

```sql
SELECT * FROM student333 WHERE age>35 OR sex='男';
```

7.查询id是1或3或5的学生

```sql
SELECT * FROM student3 WHERE id=1 OR id=3 OR id=5
```

8.使用in完成查询id是1或3或5的学生

```sql
SELECT * FROM student3 WHERE id IN (1,3,5);
```

9.查询id不是1或3或5的学生

```sql
SELECT * FROM student3 WHERE id NOT IN (1,3,5);
```

10.范围

```sql
BETWEEN 值1 AND 值2 -- 表示从值1到值2范围，包头又包尾
比如：age BETWEEN 80 AND 100
相当于： age>=80 && age<=100 
```

查询english成绩大于等于75，且小于等于90的学生

```sql
SELECT * FROM student3 WHERE english>=75 AND english<=90;
SELECT * FROM student3 WHERE english BETWEEN 75 AND 90;
```

where在计算次序上，可以使用圆括号明确次序
```mysql
	SELECT prod_name,prod_priceFROM products
	WHERE (vend_id = 1002 OR vend_id = 1003) AND prod_price >= 10;
```
#### 模糊查询

<font color=#ff0000>通配符(wildcard):</font> 用来匹配值的一部分的特殊字符。
- 百分号`(%)`通配符: %表示任何字符出现任意次数(可以是0次）。
- 下划线`(_)`通配符：下划线的用途与%一样，但下划线只匹配单个字符而不是多个字符。
<font color=#ff0000>搜索模式(search pattern):</font> 由字面值、通配符或两者组合构成的搜索条件,它可以==区分大小写==
```sql
SELECT * FROM 表名 WHERE 字段名 LIKE '通配符字符串’;
满足通配符字符串规则的数据就会显示出来所谓的通配符字符串就是含有通配符的字符串MySQL通配符有两个：%: 表示零个一个多个字符(任意多个字符)_: 表示一个字符
```

​	1）查询姓马的学生

```sql
SELECT * FROM student3 WHERE NAME LIKE '马%';
```

​	2）查询姓名中包含'德'字的学生

```sql
SELECT * FROM student3 WHERE NAME LIKE '%德%';
```

​	3）查询姓马，且姓名有三个字的学生

```sql
SELECT * FROM student3 WHERE NAME LIKE '马__';
```

#### 正则查询
[[正则表达式]]与MySQL有何关系? 
	MySQL用wHERE子句对正则表达式提供了初步的支持，允许你指定正则表达式，过滤SELECT检索出的数据。
##### 基本字符匹配
检索列 prod_name 包含文本`1000`的所有行：
关键词是regexp
```mysql
SELECT prod_name FROM products
WHERE prod_name REGEXP '1000' ORDER BY prod_name;
```

>匹配不区分大小写 MySQL中的正则表达式匹配 不区分大小写（即，大写和小写都匹配）。为区分大小写，可使用 `BINARY` 关键字，如 `WHERE prod_name REGEXP BINARY 'JetPack .000'` 。

##### OR匹配
为搜索两个串之一（或者为这个串，或者为另一个串），使用 `|` ，如下所示：
```mysql
SELECT prod_nameFROM products
WHERE prod_name REGEXP '1000 | 2000'ORDER BY prod_name;
```
#####   []匹配
想匹配特定的字符，怎么办？可通过指定一组用`[ ]` 括起来的字符来完成，如下所示：
```mysql
SELECT prod_name FROM products
WHERE prod_name REGEXP '[123] Ton'ORDER BY prod_name;
```
下面的集合将匹配数字0到9：
```mysql
SELECT prod_name FROM products
WHERE prod_name REGEXP '[1-5] Ton'ORDER BY prod_name ;
```
##### 匹配字符类
字符类
<img src="https://article.biliimg.com/bfs/article/32b215acd4eef5bb60fe0f9607047bcc81f14dcc.png" alt="image.png" style="zoom:50%;" />

正则表达式重复元字符
<img src="https://article.biliimg.com/bfs/article/4c9b40b12a56b66de3e4537e8bb859673ae42864.png" alt="image.png" style="zoom:60%;" />
匹配连在一起的4位数字：
```mysql
SELECT prod_name FROM products
WHERE prod_name REGEXP '[[:digit:]]{4}'ORDER BY prod_name ;
```
<img src="https://article.biliimg.com/bfs/article/d8fd2627c738df7604c2d383cd3b19ad1730ff26.png" alt="image.png" style="zoom:60%;" />

##### 定位符
<img src="https://article.biliimg.com/bfs/article/c8da809cdf0d69d93b3e13684ad177f19d3d5f88.png" alt="image.png" style="zoom:50%;" />

例如，如果你想找出以一个数（包括以小数点开始的数）开始的所有产品，怎么办？简单搜索 `[0-9\\.] （或 [[:digit:]\\.] ）`不行，因为
它将在文本内任意位置查找匹配。解决办法是使用 `^` 定位符，如下所示：
```mysql
SELECT prod_name FROM products
WHERE prod_name REGEXP '^[O-9\\.]'ORDER BY prod_name;
```
📝`^` 的双重用途:
	`^` 有两种用法。在集合中（用 `[]` 定义），用它来否定该集合，否则，用来指串的开始处。

#### 排序查询

1. 排序查询语法

   ```sql
   SELECT 字段列表 FROM 表名 ORDER BY 排序字段名1 [排序方式1],排序字段名2 [排序方式2] …;
   ```


排序方式：

- `ASC`：升序排列（默认值）
- `DESC`：降序排列

查询所有数据,使用年龄降序排序

```sql
SELECT * FROM student3 ORDER BY age DESC;
```

查询所有数据,在年龄降序排序的基础上，如果年龄相同再以数学成绩降序排序

```sql
SELECT * FROM student3 ORDER BY age DESC, math DESC;
```


**指定字段值的特定顺序对结果进行排序：**

`ORDER BY FIELD` 是一个用于排序的特殊命令。它允许你按照指定字段值的特定顺序对结果进行排序。
`ORDER BY FIELD` 命令接受两个参数：

1. 字段名或表达式：这是你想要根据其值排序的字段。
2. 值列表：这是你希望结果按照它们在列表中出现的顺序进行排序的值。

以下是使用 `ORDER BY FIELD` 命令的示例：

```sql
SELECT id, name
FROM users
ORDER BY FIELD(id, 3, 1, 2)
```

在上面的示例中，我们通过 `FIELD(id, 3, 1, 2)` 指定了排序顺序。这意味着我们希望结果集按照 id 列的值进行排序，并且想要先显示 id = 3 的记录，然后是 id = 1 的记录，最后是 id = 2 的记录。

#### 分组查询

##### 聚合函数

1. 概念：

   <font color=#ff0000>  将一列数据作为一个整体，进行纵向计算。</font>

2. 聚合函数分类
   
|                  函数名                   |                               描述                               |
| :------------------------------------: | :------------------------------------------------------------: |
|             `SUM(column)`              |                          计算列中所有值的总和。                           |
|            `COUNT(column)`             | 统计列中的行数。如果指定了列名，则仅统计该列中非空值的行数；如果使用了 `COUNT(*)`，则将统计所有行数而不考虑空值。 |
|             `MAX(column)`              |                           返回列中最大的值。                            |
|             `MIN(column)`              |                           返回列中最小的值。                            |
|             `AVG(column)`              |                            计算列的平均值。                            |
|    `IFNULL(column, default_value)`     |      检查列值是否为空。如果为空，则返回指定的 `default_value` 值；如果不为空，则返回列值。       |
| `GROUP_CONCAT(column ORDER BY column)` |        将同一个分组(group by)内的多个列值连接为一个字符串。默认情况下，连接的值之间由逗号分隔        |

3. 聚合函数语法：

```sql
SELECT 聚合函数名(列名) FROM 表;
```

​	<font color=#ff0000>   注意：null 值不参与所有聚合函数运算</font>

- 统计数学与英语的总和值

  方法一：统计数学与英语的总和值

  ```sql
  -- 统计数学与英语的总和值
  select sum(math) + sum(english) from student3;
  ```
	方法二：先统计每个人的数学与英语的总和值

    ```SQL
    -- 先统计每个人的数学与英语的总和值
    select sum(math) + sum(english) from student3;
    ```

- **ifnull函数**

  ```
  1.ifnull(列名, 默认值)函数表示判断该列是否为null,如果为null，返回默认值，如果不为null，返回实际的值。
  ```

##### 分组查询

分组查询本质是==根据某些条件将数据表的行(记录)进行合并==

1.分组查询语法

```sql
SELECT 字段列表 FROM 表名 [WHERE 分组前条件限定] GROUP BY 分组字段名 [HAVING 分组后条件过滤];
```

注意：

  1）==分组之后，查询的字段为聚合函数和分组字段，查询其他字段无任何意义==
  2）==group by后边的字段值相同才能划分为一组；==

==where 和 having 区别==：

- 执行时机不一样：where 是分组之前进行限定，不满足where条件，则不参与分组，而having是分组之后对结果进行过滤。
- 可判断的条件不一样：where 不能对聚合函数进行判断，having 可以。

<font color=#ff0000>执行顺序</font>： FROM 表名 where > GROUP BY > 聚合函数 > having

1.分组的语法格式？

```sql
SELECT 字段5 FROM 表名1 WHERE 条件2 GROUP BY 字段3 HAVING 条件4 order by6... ;
```

2.分组的原理？

```sql
先将相同数据作为一组,返回每组的第一条数据,单独分组没有意义,分组后跟聚合函数操作
```

3.where和having的区别？

```sql
having是在分组后对数据进行过滤.
where是在分组前对数据进行过滤
having后面可以使用聚合函数
where后面不可以使用聚合函数
```

```sql
-- 2.查询每种颜色车辆总价大于30车辆的颜色和总价
select color,sum(price) from car group by color having sum(price)>30;
```

> [!caution] 注意
> 用分组查询时SELECT 列表中的所有列必须要么在GROUP BY子句中声明，要么是被聚合函数（如 COUNT(), MAX(), MIN(), SUM(), AVG() 等）包含的。

#### 分页查询

1.分页查询语法

```sql
SELECT 字段列表 FROM 表名 LIMIT  起始索引 , 查询条目数;
```

- 起始索引：从0开始,索引是0表示数据表第一行数据
​	计算公式：==起始索引 = (当前页码-1) * 每页显示的条数==
​	补充：
- 分页查询 `limit `是MySQL数据库的方言
- Oracle 分页查询使用 `rownumber`
- SQL Server分页查询使用 `top`

### DCL
DCL英文全称是Data Control Language(数据控制语言)，用来管理数据库用户、控制数据库的**访问权限**。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240113153000.png?" alt="image.png" style="zoom:50%;" />

#### 管理用户

**查询用户**
```sql
select * from mysql.user; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240113153152.png?" alt="image.png" style="zoom:50%;" />
其中Host代表当前用户访问的主机, 如果为localhost, 仅代表只能够在当前本机访问，是不可以远程访问的。 User代表的是访问该数据库的用户名。在MySQL中需要通过Host和User来唯一标识一个用户。

**创建用户**
```sql
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
```

**修改用户密码**

```sql
ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';
```

**删除用户**

```sql
DROP USER '用户名'@'主机名';
```


> [!attention] 注意事项:
> - 在MySQL中需要通过`用户名@主机名`的方式，来唯一标识一个用户。 
> - 主机名可以使用 `%` 通配。
> - 这类SQL开发人员操作的比较少，主要是DBA（ Database Administrator 数据库管理员）使用。
> 

案例：

A. 创建用户itcast, 只能够在当前主机localhost访问, 密码123456;
```sql
create user 'itcast'@'localhost' identified by '123456'; 
```

B. 创建用户heima, 可以在任意主机访问该数据库, 密码123456;

```sql
create user 'heima'@'%' identified by '123456'; 
```

C. 修改用户heima的访问密码为1234;

```sql
alter user 'heima'@'%' identified with mysql_native_password by '1234'; 
```

D. 删除 itcast@localhost 用户

```sql
drop user 'itcast'@'localhost';
```

#### 权限控制
MySQL中定义了很多种权限，但是常用的就以下几种：

| 权限 | 说明 |
| :--: | :--: |
| ALL, ALL PRIVILEGES | 所有权限 |
| SELECT | 查询数据 |
| INSERT | 插入数据 |
| UPDATE | 修改数据 |
| DELETE | 删除数据 |
| ALTER | 修改表 |
| DROP | 删除数据库/表/视图 |
| CREATE | 创建数据库/表 |
1). 查询权限

```sql
SHOW GRANTS FOR '用户名'@'主机名' ; 
```

2). 授予权限

```sql
GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';
```

3). 撤销权限

```sql
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名'; 
```

注意事项：
• 多个权限之间，使用逗号分隔
• 授权时， 数据库名和表名可以使用 * 进行通配，代表所有。

案例:
A. 查询 'heima'@'%' 用户的权限

```sql
show grants for 'heima'@'%'; 
```

B. 授予 'heima'@'%' 用户itcast数据库所有表的所有操作权限

```sql
grant all on itcast.* to 'heima'@'%'; 
```

C. 撤销 'heima'@'%' 用户的itcast数据库的所有权限

```sql
revoke all on itcast.* from 'heima'@'%'; 
```


## MySQL函数

### 字符串函数

MySQL中内置了很多字符串函数，常用的几个如下：

| 函数 | 功能 |
| :--: | :--: |
| CONCAT(S1,S2,...Sn) | 字符串拼接，将S1, S2,... Sn拼接成一个字符串 |
| LOWER(str) | 将字符串str全部转为小写 |
| UPPER(str) | 将字符串str全部转为大写 |
| LPAD(str,n,pad) | 左填充，用字符串pad对str的左边进行填充，达到n个字符 |
| RPAD(str,n,pad) | 右填充，用字符串pad对str的右边进行填充，达到n个字符 |
| TRIM(str) | 去掉字符串头部和尾部的空格 |
| SUBSTRING(str,start,len) | 返回从字符串str从start位置起的len个长度的字符串 |

演示如下：

```sql
select concat('Hello' , ' MySQL');  # concat : 字符串拼接
select lower('Hello');  # lower : 全部转小写
select upper('Hello'); # upper : 全部转大写
select lpad('01', 5, '-'); # lpad : 左填充
select rpad('01', 5, '-'); # rpad : 右填充
select trim(' Hello MySQL ');  # trim : 去除空格
select substring('Hello MySQL',1,5); # substring : 截取子字符串
```

案例:

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240114140853.png" alt="image.png" style="zoom:60%;" />

由于业务需求变更，企业员工的工号，统一为5位数，目前不足5位数的全部在前面补0。比如： 1号员工的工号应该为00001。

```sql
UPDATA emp set workno =lpad(workno,5,'0');
```

### 数值函数

常见的数值函数如下：

| 函数 | 功能 |
| :--: | :--: |
| CEIL(x) | 向上取整 |
| FLOOR(x) | 向下取整 |
| MOD(X,y) | 返回x/y的模 |
| RAND() | 返回0~1内的随机数 |
| ROUND(X,y) | 求参数x的四舍五入的值，保留y位小数 |

演示如下： 
```sql
select ceil(1.1); # ceil：向上取整
select floor(1.9); # floor：向下取整
select mod(7,4); # mod：取模
select rand(); # rand：获取随机数
select round(2.344,2); # round：四舍五入
```

案例：
通过数据库的函数，生成一个六位数的随机验证码。
思路： 获取随机数可以通过rand()函数，但是获取出来的随机数是在0-1之间的，所以可以在其基础上乘以1000000，然后舍弃小数部分，如果长度不足6位，补0

```sql
select lpad(round(rand()*1000000 , 0), 6, '0');
```

### 日期函数
常见的日期函数如下：

| 函数 | 功能 |
| :--: | :--: |
| CURDATE() | 返回当前日期 |
| CURTIME() | 返回当前时间 |
| NOW() | 返回当前日期和时间 |
| YEAR(date) | 获取指定date的年份 |
| MONTH(date) | 获取指定date的月份 |
| DAY(date) | 获取指定date的日期 |
| DATE_ADD(date, INTERVAL exprtype) | 返回一个日期/时间值加上一个时间间隔expr后的时间值 |
| DATEDIFF(date1,date2) | 返回起始时间date1 和 结束时间date2之间的天数 |
 
演示如下：
```sql 
select curdate(); # curdate：当前日期
select curtime(); # curtime：当前时间
select now(); # now：当前日期和时间
select YEAR(now()); # YEAR , MONTH , DAY：当前年、月、日
select MONTH(now());
select DAY(now()); 
select date_add(now(), INTERVAL 70 YEAR ); # date_add：增加指定的时间间隔
select datediff('2021-10-01', '2021-12-01'); # atediff：获取两个日期相差的天数
```

案例：
查询所有员工的入职天数，并根据入职天数倒序排序。
思路： 入职天数，就是通过当前日期 - 入职日期，所以需要使用datediff函数来完成。

```sql
select name, datediff(curdate(), entrydate) as 'entrydays' from emp order by entrydays desc;
```

### 流程函数
流程函数也是很常用的一类函数，可以在SQL语句中实现条件筛选，从而提高语句的效率。

| 函数 | 功能 |
| :--: | :--: |
| IF(value,t,f) | 如果value为true，则返回t，否则返回<br>f |
| IFNULL(value1,value2) | 如果value1不为空，返回value1，否则<br>返回value2 |
| CASE WHEN [val1] THEN [res1] ...<br>ELSE [default] END | 如果val1为true，返回res1，... 否<br>则返回default默认值 |
| CASE [expr] WHEN [val1] THEN<br>[res1] ... ELSE [default] END | 如果expr的值等于val1，返回<br>res1，... 否则返回default默认值 |
|  |  |
演示如下： 

```sql
select if(false, 'Ok', 'Error'); # if
select ifnull('Ok','Default');
select ifnull('','Default');    # ifnull
select ifnull(null,'Default');


select name, (case workaddress when '北京' then '一线城市' when '上海' then '一线城市' else
'二线城市' end ) as '工作地址' from emp; # case when then else end 需求: 查询emp表的员工姓名和工作地址 (北京/上海 ----> 一线城市 , 其他 ----> 二线城市)
```

案例:
```sql
create table score(
	id int comment 'ID',
	name varchar(20) comment '姓名',
	math int comment '数学',
	english int comment '英语',
	chinese int comment '语文'
	) comment '学员成绩表';
insert into score(id, name, math, english, chinese) VALUES (1, 'Tom', 67, 88, 95), (2, 'Rose' , 23, 66, 90),(3, 'Jack', 56, 98, 76);
```

具体的SQL语句如下:

```sql
select id,name,
	(case when math >= 85 then '优秀' when math >=60 then '及格' else '不及格' end ) '数学',
	(case when english >= 85 then '优秀' when english >=60 then '及格' else '不及格' end ) '英语',
	(case when chinese >= 85 then '优秀' when chinese >=60 then '及格' else '不及格' end ) '语文'
from score;
```

## 约束

**约束的概念和分类**

1.约束的概念
- 约束是作用于表中**列上的规则**，用于限制加入表的数据
- 约束的存在保证了数据库中数据的==正确性、有效性和完整性==

2.约束的分类 

| 约束名称 | 描述                                                         | 关键字  |
| :------: | :----------------------------------------------------------: | :-----: |
| 非空约束 | 保证列中所有数据不能有null值                                  | NOT NULL |
| 唯一约束 | 保证列中所有数据各不相同                                      | UNIQUE   |
| 主键约束 | 主键是一行数据的唯一标识，要求非空且唯一                    | PRIMARY KEY |
| 检查约束 | 保证列中的值满足某一条件                                      | CHECK     |
| 默认约束 | 保存数据时，未指定值则采用默认值                              | DEFAULT   |
| 外键约束 | 外键用来让两个表的数据之间建立链接，保证数据的一致性和完整性 | FOREIGN KEY |

### 非空约束

1.概念
- ​非空约束用于保证列中所有数据不能有NULL值
2.语法
	1）添加约束
```sql
-- 创建表时添加非空约束
CREATE TABLE 表名(
  列名 数据类型 NOT NULL,
  …
); 
```

```sql
-- 建完表后添加非空约束
ALTER TABLE 表名 MODIFY 字段名 数据类型 NOT NULL;
```

2）删除约束

```sql
ALTER TABLE 表名 MODIFY 字段名 数据类型;
```

### 唯一约束

1.概念
- 唯一约束用于保证列中所有数据各不相同
2.语法
	​1）添加约束
```sql
-- 创建表时添加唯一约束
CREATE TABLE 表名(
  列名 数据类型 UNIQUE [AUTO_INCREMENT],
  -- AUTO_INCREMENT: 当不指定值时自动增长
  …
); 
CREATE TABLE 表名(
  列名 数据类型,
  …
  [CONSTRAINT] [约束名称] UNIQUE(列名)
); 
-- 建完表后添加唯一约束
ALTER TABLE 表名 MODIFY 字段名 数据类型 UNIQUE;
-- 可以获取插入新记录时自动生成的 `AUTO_INCREMENT` 列的值。
select last_insert_id();
```

2）删除约束

```sql
ALTER TABLE 表名 DROP INDEX 字段名;
```

### 主键约束
1.概念
- 主键是一行数据的唯一标识，要求非空且唯一
- 一张表只能有一个主键
2.语法
	​1）添加约束
```sql
-- 创建表时添加主键约束
CREATE TABLE 表名(
  列名 数据类型 PRIMARY KEY [AUTO_INCREMENT],
  …
); 
CREATE TABLE 表名(
  列名 数据类型,
  [CONSTRAINT] [约束名称] PRIMARY KEY(列名)
); 
```

```sql
-- 建完表后添加主键约束
ALTER TABLE 表名 ADD PRIMARY KEY(字段名);
```

2）删除约束
```sql
ALTER TABLE 表名 DROP PRIMARY KEY;
```

### 默认约束

1.概念
- 保存数据时，未指定值则采用默认值
2.语法

​	1）添加约束

```sql
-- 创建表时添加默认约束
CREATE TABLE 表名(
  列名 数据类型 DEFAULT 默认值,
  …
); 
```

```sql
-- 建完表后添加默认约束
ALTER TABLE 表名 ALTER 列名 SET DEFAULT 默认值;
```

（2）删除约束

```sql
ALTER TABLE 表名 ALTER 列名 DROP DEFAULT;
```

练习代码

```sql
-- 员工表
CREATE TABLE emp (
  id INT PRIMARY KEY auto_increment, -- 员工id，主键且自增长
  ename VARCHAR(50) NOT NULL UNIQUE, -- 员工姓名，非空并且唯一
  joindate DATE NOT NULL , -- 入职日期，非空
  salary DOUBLE(7,2) NOT NULL , -- 工资，非空
  bonus DOUBLE(7,2) DEFAULT 0 -- 奖金，如果没有奖金默认为0
);
```

### 总结

>1.非空约束的常用格式?
>
>```字段名 数据类型 NOT NULL```
>
>2.说出唯一约束的作用和常用格式？
>
>让这个字段的值不能重复，==多个null不算重复。==
>
>字段名 字段类型 UNIQUE
>
>3.主键约束特点和格式？
>
>非空且唯一，一张表只能一个主键
>
>字段名 数据类型 PRIMARY KEY
>
>4.默认约束特点和格式?
>
>保存数据时，未指定值则采用默认值
>
>列名 数据类型 DEFAULT 默认值

## 数据库设计

### 数据库设计简介

1. 软件的研发步骤

   <img src="https://article.biliimg.com/bfs/article/b7e44504bf88ca4a526d323e7ebce000780bed5e.png" alt="image.png" style="zoom:60%;" />

2. 数据库设计概念
   - 数据库设计就是根据业务系统的具体需求，结合我们所选用的DBMS，为这个业务系统构造出最优的数据存储模型。
   - 建立数据库中的<font color=#ff0000>表结构</font>以及<font color=#ff0000>表与表之间的关联关系</font>的过程。
   - 有哪些表？表里有哪些字段？表和表之间有什么关系？
3. 数据库设计的步骤
   ①需求分析（数据是什么? 数据具有哪些属性? 数据与属性的特点是什么）
   ②逻辑分析（通过ER图对数据库进行逻辑建模，不需要考虑我们所选用的数据库管理系统）
   ③物理设计（根据数据库自身的特点把逻辑设计转换为物理设计）
   ④维护设计（1.对新的需求进行建表；2.表优化）

<img src="https://article.biliimg.com/bfs/article/51d0fc95ce0ccdc8238c186b1f55a25722ec54f4.png" alt="image-20230325110818192" style="zoom:75%;" />

**表关系**

- 一对一：
  - 如：用户 和 用户详情
  - 一对一关系多用于表拆分，将一个实体中经常使用的字段放
    一张表，不经常使用的字段放另一张表，用于提升查询性能
- 一对多(多对一)：
  如：部门 和 员工
  一个部门对应多个员工，一个员工对应一个部门
- 多对多：
  - 如：商品 和 订单
  - 一个商品对应多个订单，一个订单包含多个商品

<img src="https://article.biliimg.com/bfs/article/9f9ebccd4b2947d014a074c6b5aa67d42fbcfda0.png" alt="image-20230325111155262" style="zoom:50%;" />

### 表关系之多对多
- 多对多：
  - 如：订单 和 商品
  - 一个商品对应多个订单，一个订单包含多个商品
- 实现方式：建立第三张<font color=#ff0000>中间表</font>，中间表至少包含<font color=#ff0000>两个外键</font>，分别<font color=#ff0000>关联两方主键</font>

<img src="https://article.biliimg.com/bfs/article/69fae38aec7fc71b133aeba8ce0421f0ff56fce4.png" alt="image-20230325111317254" style="zoom:50%;" />

### 外键约束

1. 概念
   - 外键用来让两个表的数据之间建立链接，保证数据的一致性和完整性
   例如：上述多对多中的订单商品表来维护订单表和商品表之间的关系。
2. 使用中间表的目的是维护两表多对多的关系：
    1) 中间表插入的数据 必须在多对多的主表中存在。
    2) 如果主表的记录在中间表维护了关系，就不能随意删除。如果可以删除，中间表就找不到对应的数据了，这样就没有意义了。
3. 上述是中间表存在的意义，可是我们这里所创建的中间表并没有起到上述的作用，而是存在缺点的：
   缺点: 我们是可以向中间表插入不存在的订单编号和商品编号的。
1.语法
	​（1）添加约束

```sql
-- 创建表时添加外键约束
CREATE TABLE 表名(
  列名 数据类型,
  …
  [CONSTRAINT] [外键名称] FOREIGN KEY(外键列名) REFERENCES 主表(主表列名) 
); 
```

```sql
-- 关键字解释：
constraint: 添加约束，可以不写
foreign key(当前表中的列名): 将某个字段作为外键
references 被引用表名(被引用表的列名) : 外键引用主表的主键
```

```sql
-- 建完表后添加外键约束
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称);
```

（2）删除约束

```sql
ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;
```

1.给上述创建好的第三张表添加外键约束。

  1）清空上述三张表：

```sql
truncate tb_goods;
truncate tb_order;
truncate tb_order_goods;
```

2）增加外键约束：

```sql
-- 来自于订单表
alter table tb_order_goods add constraint o_id_fk foreign key(order_id) references tb_order(id);
-- 来自于商品表
alter table tb_order_goods add constraint g_id_fk foreign key(goods_id) references tb_goods(id);
```

<img src="https://article.biliimg.com/bfs/article/88b4b49d5954b77dbf946b4ce2da6cc374455276.png" alt="image.png" style="zoom:60%;" />

**注意**

有了外键约束操作数据注意事项?
   1)要求添加数据需要先==添加主表，然后添加从表==。
   2)要求删除数据需要先==删除从表，然后再删除主表==。

### 表关系之一对多

- 一对多(多对一)：
  - 如：部门表 和 员工表
  - 一个部门对应多个员工，一个员工对应一个部门
- 实现方式：在<font color=#ff0000>多</font>的一方建立<font color=#ff0000>外键</font>，指向一的一方的主键

<img src="https://article.biliimg.com/bfs/article/394f849ccc6995baa92eb094efd226f0417324b0.png" alt="image.png" style="zoom:60%;" />

### 表关系之一对一

- 一对一：
  - 如：用户 和 用户详情
  - 一对一关系多用于表拆分，将一个实体中经常使用的字段放一张表，不经常使用的字段放另一张表，用于提升查询性能
- 实现方式：在任意一方加入外键，关联另一方主键，并且设置外键为==唯一(UNIQUE)==

<img src="https://article.biliimg.com/bfs/article/a2d5273d946cd383f256d249686258e2ef28887e.png" alt="image.png" style="zoom:60%;" />

## 多表查询

### 多表查询简介

1)什么是多表查询?

​	同时查询多张表,获取需要的数据;
​	比如:我们向查询水果对应的价格,需要将水果表和价格表同时进行查询;

​	<img src="https://article.biliimg.com/bfs/article/f1cd5faa63d2b47d9386ac7d3ee146013918efcd.png" alt="image-20230325150328498" style="zoom:50%;" />

2)多表查询的分类

<img src="https://article.biliimg.com/bfs/article/1dd37715dc44c6c3f3eafc76c77638e6ac67c08a.png" alt="image-20230325150348995" style="zoom:50%;" />

3)笛卡尔积现象查询?
​	同时查询多张表,获取需要的数据;
​	比如:我们向查询水果对应的价格,需要将水果表和价格表同时进行查询;
```sql
--价格:1
create table price( 
  id int primary key auto_increment,
  price double
 );
-- 水果n
create table fruit(
  id int primary key auto_increment,
  name varchar(20) not null,
  price_id int,
  foreign key(price_id) references price(id)
 );
 insert into price values(1,2.30);
 insert into price values(2,3.50);
 insert into price values(4,null);
 insert into fruit values(1,'苹果',1);
 insert into fruit values(2,'橘子',2);
 insert into fruit values(3,'香蕉',null);
```

<img src="https://article.biliimg.com/bfs/article/6d3b052610cc07f24170bf1bb686ad9cd73f58f2.png" alt="image.png" style="zoom:50%;" />

### 内连接

**1** **什么是内连接**

内连接查询又称为交集查询,也就是查询只显示满足条件的数据；

**2** **显式内连接**

显示内连接：使用INNER JOIN ... ON语句, 可以省略INNER关键字

```sql
-- 语法格式：
select *from 表名1 inner join 表名2 on 条件;
--或者
select * from 表名1 join 表名2 on 条件 
```

<img src="https://article.biliimg.com/bfs/article/0f964a23ef6f3ab997555f24c0488dfe1768cc8a.png" alt="image.png" style="zoom:60%;" />

**3** **隐式内连接**

​看不到join关键字,条件使用where指定

```sql
select 列名 , 列名, .... from 表名1,表名2 where 表名1.列名 = 表名2.列名;
```

**内连接示例：**

```sql
--  隐式内连接
  select  *from fruit,price where fruit.price_id=price.id;
-- 显式内连接
  select *from fruit inner join price on fruit.price_id=price.id;
```

<img src="https://article.biliimg.com/bfs/article/2c3ed60bfcb89f1696aaafc3d8627c5322c1df8f.png" alt="image.png" style="zoom:50%;" />

**内连接作用？**

1. 过滤笛卡尔积(没有联结关系的表返回的就是笛卡尔积);
2. 获取两表的交集部分(都满足条件的部分);
### 外连接

**1** **左外连接**
- 左表的记录全部表示出来;
- 右表只会显示符合搜索条件的记录;
这里的圆的面积都是指的==数据表的记录(一行数据)==，这里合并的全都是A行数为主

<img src="https://article.biliimg.com/bfs/article/a9c6838ff594af77c003d6c21eb66afa56020730.png" alt="image-20230325210011185" style="zoom:50%;" />

语法格式：

```sql
select * from 表1 left [outer] join 表2 on 条件;
-- 说明：
-- 1.left 关键字左边的表定义为左表，left 关键字右边的表定义为右表,查询的内容以左表为主，
如果左表有数据，而右表没有对应的数据，仍然会把左表数据进行显示。
-- 2.outer关键字可省略
```

练习:不管能否查到水果对应的价格，都要把水果显示出来；

```sql
-- 左外连接查询
select *from fruit left outer join price on fruit.price_id=price.id;
```

<img src="https://article.biliimg.com/bfs/article/5ccb3ec804de2c37cc1e6b5f407ff4ebec29e452.png" alt="image.png" style="zoom:60%;" />

**2** **右外连接**
- 右表的记录全部表示出来;
- 左表只会显示符合搜索条件的记录;
<img src="https://article.biliimg.com/bfs/article/d4321809b1d46f74dc222c43167e07d12433d860.png" alt="image-20230325210850211" style="zoom:50%;" />

语法格式：

```sql
select *from 表1 right [outer] join 表2 on 条件;
--说明：
-- 1.right关键字左边的表定义为左表，right关键字右边的表定义为右表,查询的内容以右表为主，
如果右表有数据，而左表没有对应的数据，仍然会把右表数据进行显示。
-- 2.outer关键字可省略
```

练习:不管能否查到价格对应的水果，都要把价格显示出来

```sql
select * from fruit right outer join price on fruit.price_id=price.id
```

<img src="https://article.biliimg.com/bfs/article/ffea0158ddcf257c5703fab1b54a4cf5aa83d36e.png" alt="image.png" style="zoom:50%;" />

### 子查询

**1** **介绍**

子查询就是一个SQL查询的结果**作为**另一个SQL查询语句语法的一部分;

<img src="https://article.biliimg.com/bfs/article/ad83c1fa2d15a3ae4df89b6ea42de31010a2f285.png" alt="image-20230325211002798" style="zoom:60%;" />

**2** **子查询结果分类**

子查询的结果分为3种情况:

- 单行单列
  - ![image-20230325211147153](https://article.biliimg.com/bfs/article/fcdea827bf8091424b7517eada792477ae61a0ca.png)
  - 使用比较运算符:  > 、>= 、<、<=、=等

- 多行单列
  - ![image-20230325211208168](https://article.biliimg.com/bfs/article/58d464cd0a21e6028809f93de8b2316df39604f6.png)
  - 使用**in**关键字

- 多行多列
  - <img src="https://article.biliimg.com/bfs/article/fcfdfb69ebff01c1ae1b86233007b53fc69f478d.png" alt="image.png" style="zoom:60%;" />
  - 将查询结果使用**as**取别名作为一张表，与其他表进行关联查询
### 组合查询
多个查询（多条 SELECT 语句），并将结果作为单个查询结果集返回叫做组合查询，
假如需要价格小于等于5的所有物品的一个列表，而且还想包括供应商1001和1002生产的所有物品（不考虑价格）。当然，可以利用 WHERE 子句来完成此工作，不过这次我们将使用 UNION 。
```mysql
SELECT vend_id，prod_id，prod_price FROM products
WHERE prod_price <= 5
UNION
SELECT vend_id，prod_id，prod_price FROM products
WHERE vend_id IN (1001,1002);
```
UNION规则
- UNION 必须由两条或两条以上的 SELECT 语句组成，语句之间用关键字 UNION 分隔（因此，如果组合4条 SELECT 语句，将要使用3个
UNION 关键字）。
- UNION 中的每个查询必须包含相同的列、表达式或聚集函数（不过列数据类型必须兼容：类型不必完全相同，但必须是DBMS可以
隐含地转换的类型（例如，不同的数值类型或不同的日期类型）。
### 多表查询案例

```sql
-- 4.查询每个同学的学号、姓名、选课数、总成绩
/*注意:如果子查询中查询的结果字段是聚合函数，并且最后结果需要使用聚合函数，
    那么必须使用as给聚合函数的字段起别名*/
select student_id,count(student_id),sum(score) from studentcourse group by student_id;
select s.id,s.name,temp.选课数,temp.总成绩 from student s,(select student_id,count(student_id) as 选课数,sum(score) as 总成绩 from studentcourse group by student_id) temp where student_id=id;
```

## 事务

### 事务简介

- 数据库的==事务（Transaction）==是一种机制、一个操作序列，包含了==一组数据库操作命令==
- 事务把所有的命令作为一个整体一起向系统提交或撤销操作请求，即这一组数据库命令==要么同时成功，要么同时失败==
- 事务是一个不可分割的工作逻辑单元
- 回滚事务是指在数据库管理系统中，当一个事务中有一条或多条操作**失败**或发生**异常**时，系统会将该事务之前的所有操作全部撤销或回滚，使数据库**恢复**到事务开始前的状态。

<img src="https://article.biliimg.com/bfs/article/bd976857025596a48eac3724926f1fff1244ac20.png" style="zoom: 50%;" />

```sql
-- 开启事务
START TRANSACTION;
-- 提交事务
COMMIT;
-- 回滚事务
ROLLBACK;
```

### 事务操作

能够使用手动的方式提交事务
MYSQL中可以有两种方式进行事务的操作：

1. 手动提交事务：先开启，再提交
2. 自动提交事务(默认的):即执行一条sql语句提交一次事务。

事务有关的SQL语句：

手动提交事务使用步骤
​    第1种情况：开启事务 -> 执行SQL语句 -> 成功 -> 提交事务
​    第2种情况：开启事务 -> 执行SQL语句 -> 失败 -> 回滚事务

<img src="https://article.biliimg.com/bfs/article/b68e9c8cd26e72fc5d00821e7a9b2c5516b4163d.png" alt="image.png" style="zoom:60%;" />

准备数据:

```sql
# 创建一个表：账户表.
create database day04_db; 
# 使用数据库
use day04_db;
# 创建账号表
create table account(
	id int primary key auto_increment,
	name varchar(20),
	money double
);
# 初始化数据
insert into account values (null,'a',1000);
insert into account values (null,'b',1000);
```

练习：

​	实现:a给b转账100元,演示提交事务和回滚事务
```sql
-- 需求1:a给b转账100元,演示提交事务
-- 1.1.手动开启事务
start transaction;
-- 1.2. a扣款100
update account set money=money-100 where name='a';
-- 1.3 b收款100
update account set money=money+100 where name='b';
-- 1.4 事务提交
commit;
-- 需求2:a给b转账100元,演示回滚事务
-- 1.1.手动开启事务
start transaction;
-- 1.2. a扣款100
update account set money=money-100 where name='a';
-- 1.3 b收款100
update account set money=money+100 where name='b';
-- 1.4 事务回滚
rollback;
```

**自动提交事务**	

MySQL的每一条DML(增删改)语句都是一个单独的事务，每条语句都会自动开启一个事务，执行完毕自动提交事务，MySQL默认开始自动提交事务。自动提交，通过修改mysql全局变量“autocommit”进行控制。

1.通过以下命令可以查看当前autocommit模式：

```sql
show variables like '%commit%';
```

<img src="https://article.biliimg.com/bfs/article/6fca8b8c8f9a541e3e61331219e50793885405cb.png" alt="image-20230325231028533" style="zoom:50%;" />

2.设置自动提交的参数为OFF:

```sql
set autocommit = 0;  -- 0:OFF  1:ON
```

**原理**

<img src="https://article.biliimg.com/bfs/article/3afd6fa6fc4acb3fa834b9b0b9e6e8873d8a605f.png" alt="image.png" style="zoom:70%;" />

1. 一个用户登录成功以后，服务器会创建一个临时日志文件。日志文件用来保存用户事务状态。
2. 如果没有使用事务，则所有的操作直接写到数据库中，不会使用日志文件。
3. 如果开启事务，将所有的写操作写到日志文件中。
4. 如果这时用户提交了事务，则将日志文件中所有的操作写到数据库中。
5. 如果用户回滚事务，则日志文件会被清空，不会影响到数据库的操作。

### 事务四大特征

事务的四大特性(ACID)(面试)

数据库的事务必须具备**ACID**特性，ACID是指 Atomicity（原子性）、Consistensy（一致性）、Isolation（隔离性）和Durability（持久性）的英文缩写。

1、`隔离性（Isolation）`

**多个用户并发的访问数据库时，一个用户的事务不能被其他用户的事务干扰，多个并发的事务之间要相互隔离。**

<img src="https://article.biliimg.com/bfs/article/be2a279b8010b3d310ae9b1db0c830038b1ea23f.png" alt="image-20230325231245926" style="zoom:50%;" />

2、`持久性(Durability)`

**指一个事务一旦被提交，它对数据库的改变将是永久性的，哪怕数据库发生异常，重启之后数据依然存在。**

<img src="https://article.biliimg.com/bfs/article/3e0e841ad2aee958465a7ad492f98c77da71ff79.png" alt="image-20230325231255595" style="zoom:50%;" />

3、`原子性(Atomicity)` 

**原子性是指事务包装的一组sql(一组业务逻辑)是一个不可分割的工作单位**，事务中的操作要么都发生，要么都不发生。

4、`一致性(Consistency)`

**一致性是指数据处于一种语义上有意义且正确的状态；** 

事务一致性是指事务执行的结果必须是使数据从一个一致性状态变到另一个一致性状态。
事务的成功与失败，最终数据库的数据都是符合实际生活的**业务逻辑**。一致性绝大多数**依赖业务逻辑**和原子性。
<img src="https://article.biliimg.com/bfs/article/25654b8ed8d8330bca45cb5f241175930fb13f32.png" alt="image-20230325231316897" style="zoom:50%;" />



### 事务并发访问问题

事务的并发访问引发的三个问题(面试)
事务在操作时的理想状态：多个事务之间互不影响，如果隔离级别设置不当就可能引发并发访问问题。

| 并发访问的问题 |                             含义                              |
| :-----: | :---------------------------------------------------------: |
|   脏读    |                   一个事务读取到了另一个事务中尚未提交的数据。                    |
|  不可重复读  | 一个事务中多次读取的数据内容不一致； 要求的是一个事务中多次读取时数据是不一致的，这是事务update时引发的问题；  |
| 幻读（虚读）  | 一个事务内读取到了别的事务插入或者删除的数据，导致前后读取记录行数不同。  这是insert或delete时引发的问题 |

1.脏读：

**指一个事务读取了另外一个事务未提交的数据**；
脏读强调的是一个事务读取了未提交的事务的数据;

<img src="https://article.biliimg.com/bfs/article/59dbde6eb5d0f47990832556420a689b2deb6076.png" alt="image-20230325231557054" style="zoom:50%;" />2.不可重复读：

**在一个事务内多次读取表中的数据，多次读取的内容不同**，**多发生在其他事务**update操作时;
<img src="https://article.biliimg.com/bfs/article/831e516e4a8e9683766858ee9d6dcb752dd61c1c.png" alt="image-20230325231649307" style="zoom:50%;" />

3.幻读（虚读）

**一个事务内读取到了别的事务插入或者删除的数据，导致前后读取记录行数不同**,多发生在**delete或insert**时；

<img src="https://article.biliimg.com/bfs/article/82ffca6419f298a7e3764a8e5887f32b61d76ea8.png" alt="image-20230325231736715" style="zoom:50%;" />

### 事务的隔离级别

MySQL数据库规范规定了4种隔离级别，用于解决上述出现的事务并发问题;

| 级别 |   名字   |     隔离级别      | 脏读 | 不可重复读 | 幻读 | 数据库默认隔离级别  |
| :--: | :------: | :---------------: | :--: | :--------: | :--: | :-----------------: |
|  1   | 读未提交 | read uncommitted |  是  |     是     |  是  |                     |
|  2   | 读已提交 |  read committed  |  否  |     是     |  是  | Oracle和SQL  Server |
|  3   | 可重复读 | repeatable read  |  否  |     否     |  是  |        MySQL        |
|  4   |  串行化  |   serializable    |  否  |     否     |  否  |                     |

说明：

- 其实上述三个问题中，**最严重的就是脏读**（读取了错误数据），这个问题一定要避免；
- 关于不可重复读和虚读其实并不是**逻辑上的错误**，而是数据的**时效性**问题，所以这种问题并不属于很严重的错误；
- 如果对于数据的时效性要求不是很高的情况下，我们是可以接受不可重复读和虚读的情况发生的；

2、安全和性能对比

安全: 串行化>可重复读>读已提交>读未提交 
性能: 串行化<可重复读<读已提交<读未提交 

## 存储引擎

### MySQL体系结构

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240115163927.png?" alt="image.png" style="zoom:70%;" />

1). 连接层
最上层是一些客户端和链接服务，包含本地sock通信和大多数基于客户端/服务端工具实现的类似于TCP/IP的通信。主要完成一些类似于**连接处理、授权认证**、及相关的安全方案。在该层上引入了线程池的概念，为通过认证安全接入的客户端提供线程。同样在该层上可以实现基于SSL的安全链接。服务器也会为安全接入的每个客户端验证它所具有的操作权限。
2). 服务层
第二层架构主要完成大多数的核心服务功能，如SQL接口，并完成**缓存的查询**，SQL的分析和优化，部分内置函数的执行。所有跨存储引擎的功能也在这一层实现，如**过程、函数**等。在该层，服务器会解析查询并创建相应的内部解析树，并对其完成相应的优化如确定表的查询的顺序，是否利用索引等，最后生成相应的执行操作。如果是select语句，服务器还会查询内部的缓存，如果缓存空间足够大，这样在解决大量读操作的环境中能够很好的提升系统的性能。
3). 引擎层
存储引擎层， 存储引擎真正的负责了MySQL中数据的存储和提取，服务器通过API和存储引擎进行通信。不同的存储引擎具有不同的功能，这样我们可以根据自己的需要，来选取合适的存储引擎。数据库中的索引是在存储引擎层实现的。
4). 存储层
数据存储层， 主要是将数据(如: redolog、undolog、数据、索引、二进制日志、错误日志、查询日志、慢查询日志等)存储在文件系统之上，并完成与存储引擎的交互。
### 存储引擎介绍

存储引擎就是存储数据、建立索引、更新/查询数据等技术的**实现方式**。存储引擎**是基于表的**，而不是基于库的，所以存储引擎也可被称为表类型。我们可以在创建表的时候，来指定选择的存储引擎，如果没有指定将自动选择默认的存储引擎。

建表时指定存储引擎

```sql
CREATE TABLE 表名(
字段1 字段1类型 [ COMMENT 字段1注释 ] ,
......
字段n 字段n类型 [COMMENT 字段n注释 ]
) ENGINE = INNODB [ COMMENT 表注释 ] ;
```

查询当前数据库支持的存储引擎
```sql
show engines; 
```

### 存储引擎特点

MySQL 提供了多种存储引擎，每种存储引擎都有其独特的特点和适用场景。以下是 MySQL 中常见的一些存储引擎及其特点：

1. InnoDB
	- 支持事务处理和行级锁；
	- 通过使用**聚簇索引**提高查询性能；
	- 可以在高并发的情况下保证数据的安全性和完整性；
	- 支持外键约束等高级功能。

	文件
		xxx.ibd：xxx代表的是表名，innoDB引擎的每张表都会对应这样一个表空间文件，存储该表的表结构（frm-早期的 、sdi-新版的）、数据和索引。

	参数：innodb_file_per_table
	
	```sql
	show variables like 'innodb_file_per_table'; 
	```

	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240115164615.png?" alt="image.png" style="zoom:70%;" />

	如果该参数开启，代表对于InnoDB引擎的表，每一张表都对应一个ibd文件。 我们直接打开MySQL的数据存放目录： `C:\ProgramData\MySQL\MySQL Server 8.0\Data` ， 这个目录下有很多文件夹，不同的文件夹代表不同的数据库，我们直接打开itcast文件夹。
	
	可以看到里面有很多的ibd文件，每一个ibd文件就对应一张表，比如：我们有一张表 account，就有这样的一个account.ibd文件，而在这个ibd文件中不仅存放表结构、数据，还会存放该表对应的**索引信息**。 而该文件是基于二进制存储的，不能直接基于记事本打开，我们可以使用mysql提供的一个指令 ibd2sdi ，通过该指令就可以从ibd文件中提取sdi信息，而sdi数据字典信息中就包含该表的表结构。

	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240115164759.png?" alt="image.png" style="zoom:60%;" />

	逻辑存储结构
	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240115164846.png?" alt="image.png" style="zoom:60%;" />
	- 表空间: InnoDB存储引擎逻辑结构的最高层，ibd文件其实就是表空间文件，在表空间中可以包含多个Segment段。
	- 段: 表空间是由各个段组成的， 常见的段有数据段、索引段、回滚段等。InnoDB中对于段的管理，都是引擎自身完成，不需要人为对其控制，一个段中包含多个区。
	- 区: 区是表空间的单元结构，每个区的大小为1M。 默认情况下， InnoDB存储引擎页大小为16K， 即一个区中一共有64个连续的页。
	- 页: 页是组成区的最小单元，**页也是InnoDB 存储引擎磁盘管理的最小单元**，每个页的大小默认为 16KB。为了保证页的连续性，InnoDB 存储引擎每次从磁盘申请 4-5 个区。
	- 行: InnoDB 存储引擎是面向行的，也就是说数据是按行进行存放的，在每一行中除了定义表时所指定的字段以外，还包含两个隐藏字段(后面会详细介绍)。

2. MyISAM
	- 不支持事务处理和行级锁；
	- 支持全文搜索和压缩表格等功能；
	- 对于读密集型应用具有较好的性能表现；
	- 适合于静态或者基于日志文件的应用，如日志分析等。

3. Memory
	- 将表格存储在内存中，读写速度非常快；
	- 不支持大量数据的存储，且断电后数据将全部丢失；
	- 可以被用于缓存一些频繁访问的数据，如会话管理、缓存等。

4. NDB（MySQL Cluster）
	- 支持数据分区和集群部署，提高可扩展性和容错性；
	- 支持实时数据备份和恢复；
	- 对于高并发的 OLTP 应用和在线游戏具有较好的性能表现。

5. CSV
	- 将数据以 CSV（Comma Separated Value）格式存储；
	- 可以被用于数据的导入和导出、数据交换等场景。

### 区别及特点

| 特点 | InnoDB | MyISAM | Memory |
| :--: | :--: | :--: | :--: |
| 存储限制 | 64TB | 有 | 有 |
| 事务安全 | 支持 | - | - |
| 锁机制 | 行锁 | 表锁 | 表锁 |
| B+tree索引 | 支持 | 支持 | 支持 |
| Hash索引 | - | - | 支持 |
| 全文索引 | 支持(5.6版本之后) | 支持 | - |
| 空间使用 | 高 | 低 | N/A |
| 内存使用 | 高 | 低 | 中等 |
| 批量插入速度 | 低 | 高 | 高 |
| 支持外键 | 支持 | - | - |
面试题:
InnoDB引擎与MyISAM引擎的区别 ?
1. InnoDB引擎, 支持事务, 而MyISAM不支持。
2. InnoDB引擎, 支持行锁和表锁, 而MyISAM仅支持表锁, 不支持行锁。
3. InnoDB引擎, 支持外键, 而MyISAM是不支持的。


## 索引

### 索引介绍及分类

​	1)什么是索引

Mysql官方对索引的定义为：**索引（index）是帮助Mysql高效获取数据的数据结构。**

**可以得到索引的本质：索引就是数据结构;**

<img src="https://article.biliimg.com/bfs/article/ebef7b589a371c0358e2a1117f29c6eb779e6519.png" alt="image-20230327112328040" style="zoom:50%;" />

**MySQL索引分类**

根据下面的内容写一个4列的markdown图表 

| 分类 | 含义 | 特点 | 关键字 |
| :--: | :--: | :--: | :--: |
| 主键索引 | 针对于表中主键创建的索引 | 默认自动创建, 只能有一个 PRIMARY | PRIMARY |
| 唯一索引 | 避免同一个表中某数据列中的值重复 | 可以有多个 UNIQUE | UNIQUE |
| 常规索引 | 快速定位特定数据 | 可以有多个 |  |
| 全文索引 | 全文索引查找的是文本中的关键词，而不是比较值 | 可以有多个 FULLTEXT | FULLTEXT |
说明：

1. 我们创建表时就会指定主键和唯一约束，那么就==相当于给表的字段添加了主键和唯一索引==。
2. MySQL 中，`KEY`和`INDEX`关键字是等价的

### MySQL索引语法

**在已有表的字段上直接创建**

```sql
-- 创建普通索引
create index 索引名 on 表名(字段);
-- 创建唯一索引
create unique index 索引名 on 表名(字段);
-- 创建普通组合索引
create index 索引名 on 表名(字段1,字段2,..);
-- 创建唯一组合索引
create unique index 索引名 on 表名(字段1,字段2,..);
```

说明：如果在同一张表中创建多个索引，要保证索引名是不能重复,采用上述方式不能添加主键索引

**在已有表的字段上修改表时指定**

```sql
-- 添加一个主键，这意味着索引值必须是唯一的，且不能为NULL
alter table 表名 add primary key(字段);  --默认索引名：primary
-- 添加唯一索引（除了NULL外，NULL可能会出现多次）
alter table 表名 add unique(字段); -- 默认索引名：字段名
-- 添加普通索引，索引值可以出现多次。
alter table 表名 add index(字段); -- 默认索引名：字段名
```

**查看索引**

```
show index from 表名; 
```

**删除索引**

- 语法

【语法1】直接删除

~~~sql
-- 直接删除
drop index 索引名 on 表名;
~~~

【语法2】修改表时删除

```sql
-- 修改表时删除
alter table 表名 drop index 索引名;
```

测试

```sql
create table student(
    id int,
    name varchar(32),
    telephone varchar(11)
);
-- 给name字段设置普通索引
create index name_idx on student(name);
-- 给telephone字段设置唯一索引
create unique index telephone_uni_idx on student(telephone);
alter table student add primary key(id);
alter table student add index(name);
drop index name_idx on student;
alter table student drop index telephone_uni_idx;
alter table user add primary key(id);
create unique index id_index on user(id);
show index from student; 
```

**索引的优缺点**

**优势**

-  类似于书籍的目录索引，提高数据检索的效率，**降低数据库的IO成本**。IO次数越多，效率越低。
-  索引底层就是排序，通过索引列对数据进行排序，**降低数据排序的成本**，降低CPU的消耗。
劣势
- 在数据库建立过程中，需花费较多的时间去建立并**维护索引**，特别是随着数据总量的增加，所花费的时间将不断递增。
- 在数据库中创建的索引需要占用一定的**物理存储空间**，这其中就包括数据表所占的数据空间以及所创建的每一个索引所占用的物理空间。
- 在对表中的数据进行**修改时**，例如对其进行增加、删除或者是修改操作时，索引还需要进行动态的维护，这给数据库的维护速度带来了一定的麻烦。

### 索引的数据结构

概述
我们知道**索引**是帮助MySQL高效获取**排好序**的**数据结构**。
为什么使用索引后查询效率提高很多呢？
肯定和mysql底层的数据结构有关的，接下来我们就分析下mysql中的索引底层的数据结构。
表结构及其数据如下： 

| id | name      | age |
|----|-----------|-----|
| 1  | 金庸      | 36  |
| 2  | 张无忌    | 22  |
| 3  | 杨逍      | 33  |
| 4  | 韦一笑    | 48  |
| 5  | 常遇春    | 53  |
| 6  | 小昭      | 19  |
| 7  | 灭绝      | 45   |
|8   | 周芷若    |17   |
|9   |丁敏君     |23   |
|10   |赵敏       |20   |
假如我们要执行的SQL语句为 ： `select * from user where age = 45`;

1. 无索引情况

	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240120212021.png?" alt="image.png" style="zoom:50%;" />

	在无索引情况下，就需要从第一行开始扫描，一直扫描到最后一行，我们称之为 全表扫描，性能很低。

2. 有索引情况
	如果我们针对于这张表建立了索引，假设索引结构就是二叉树，那么也就意味着，会对age这个字段建立一个二叉树的索引结构。

	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240120212109.png?" alt="image.png" style="zoom:50%;" />
此时我们在进行查询时，只需要扫描三次就可以找到数据了，极大的提高的查询的效率。

#### 索引结构


| 索引结构 | 描述 |
| :--: | :--: |
| B+Tree索引 | 最常见的索引类型，大部分引擎都支持 B+树索引 |
| Hash索引 | 底层数据结构是用哈希表实现的, 只有精确匹配索引列的查询才有效,不支持范围查询 |
| R-tree | 空间索引是MyISAM引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少 |
| Full-text | 是一种通过建立倒排索引,快速匹配文档的方式。类似于Lucene,Solr,ES |
上述是MySQL中所支持的所有的索引结构，接下来，我们再来看看不同的存储引擎对于索引结构的支持情况。

| 索引类型 | InnoDB | MyISAM | Memory |
| :--: | :--: | :--: | :--: |
| B+tree索引 | 支持 | 支持 | 支持 |
| Hash 索引 | 不支持 | 不支持 | 支持 |
| R-tree 索引 | 不支持 | 支持 | 不支持 |
| Full-text 5.6版本之后支持 | 支持 | 支持 | 不支持 |
#### 二叉树

假如说MySQL的索引结构采用二叉树的数据结构，比较理想的结构如下：

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240120212745.png?" alt="image.png" style="zoom:60%;" />

如果主键是顺序插入的，则会形成一个单向链表，结构如下：

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240120212840.png?" alt="image.png" style="zoom:60%;" />

所以，如果选择二叉树作为索引结构，会存在以下缺点：
- 顺序插入时，会形成一个链表，查询性能大大降低。
- 大数据量情况下，层级较深，检索速度慢。

可以选择红黑树，红黑树是一颗自平衡二叉树，那这样即使是顺序插入数据，最终形成的数据结构也是一颗平衡的二叉树,结构如下:

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240120212931.png?" alt="image.png" style="zoom:50%;" />
但是，即使如此，由于红黑树也是一颗二叉树，所以也会存在一个缺点：
- 大数据量情况下，层级较深，检索速度慢。
所以，在MySQL的索引结构中，并没有选择二叉树或者红黑树，而选择的是B+Tree，那么什么是
B+Tree呢？在详解B+Tree之前，先来介绍一个B-Tree。

#### B-Tree

B-Tree，B树是一种多叉路衡查找树，相对于二叉树，B树每个节点可以有多个分支，即多叉。
以一颗最大度数（max-degree）为5(5阶)的b-tree为例，那这个B树每个节点最多存储4个key，5个指针：

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240120213113.png" alt="image.png" style="zoom:60%;" />

> 知识小贴士: 树的度数指的是一个节点的子节点个数。


我们可以通过一个数据结构可视化的网站来简单演示一下:[Data Structure Visualization](https://www.cs.usfca.edu/~galles/visualization/Algorithms.html)
插入一组数据： 100 65 169 368 900 556 780 35 215 1200 234 888 158 90 1000 88 120 268 250 。然后观察一些数据插入过程中，节点的变化情况。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240120213703.png?" alt="image.png" style="zoom:50%;" />

特点：
- 5阶的B树，每一个节点最多存储4个key，对应5个指针。
- 一旦节点存储的key数量到达5，就会裂变，中间元素向上分裂。
- 在B树中，非叶子节点和叶子节点都会存放数据。

#### B+Tree
B+Tree是B-Tree的变种，我们以一颗最大度数（max-degree）为4（4阶）的b+tree为例，来看一下其结构示意图：
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240120214147.png?" alt="image.png" style="zoom:50%;" />
我们可以看到，两部分：
- 蓝色框框起来的部分，是索引部分，仅仅起到索引数据的作用，不存储数据。
- 绿色框框起来的部分，是数据存储部分，在其叶子节点中要存储具体的数据。

我们可以通过一个数据结构可视化的网站来简单演示一下: [B+ Tree Visualization](https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html)

插入一组数据： 100 65 169 368 900 556 780 35 215 1200 234 888 158 90 1000 88 120 268 250 。然后观察一些数据插入过程中，节点的变化情况。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240120214437.png?" alt="image.png" style="zoom:50%;" />

最终我们看到，B+Tree 与 B-Tree相比，主要有以下三点区别：
- 所有的数据都会出现在叶子节点。
- 叶子节点形成一个**单向链表**。
- 非叶子节点仅仅起到索引数据作用，具体的数据都是在叶子节点存放的。

上述我们所看到的结构是标准的B+Tree的数据结构，接下来，我们再来看看MySQL中优化之后的B+Tree。
MySQL索引数据结构对经典的B+Tree进行了优化。在原B+Tree的基础上，增加一个指向相邻叶子节点的链表指针，就形成了带有顺序指针的B+Tree，提高区间访问的性能，利于排序。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240120214619.png?" alt="image.png" style="zoom:50%;" />

#### Hash

MySQL中除了支持B+Tree索引，还支持一种索引类型---Hash索引。

1). 结构
哈希索引就是**采用一定的hash算法，将键值换算成新的hash值，映射到对应的槽位上**，然后存储在hash表中。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240120215025.png?" alt="image.png" style="zoom:50%;" />

如果两个(或多个)键值，映射到一个相同的槽位上，他们就产生了hash冲突（也称为hash碰撞），可以通过链表来解决。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240120215057.png?" alt="image.png" style="zoom:50%;" />

2). 特点
A. Hash索引只能用于对等比较(=，in)，不支持范围查询（between，>，< ，...）
B. 无法利用索引完成排序操作
C. 查询效率高，通常(不存在hash冲突的情况)只需要一次检索就可以了，效率通常要高于B+tree索引

#面试点 
> [!question] 为什么InnoDB存储引擎选择使用B+tree索引结构?
> 1. 相对于二叉树，层级更少，搜索效率高； 
> 2. 对于B-tree，无论是叶子节点还是非叶子节点，都会保存数据，这样导致一页中存储的键值减少，指针跟着减少，要同样保存大量数据，只能增加树的高度，导致性能降低； 
> 3. 相对Hash索引，B+tree支持范围匹配及排序操作；


聚集索引&二级索引
而在 InnoDB存储引擎中，根据索引的存储形式，又可以分为以下两种：

| 分类 | 含义 | 特点 |
| :--: | :--: | :--: |
| 聚集索引(ClusteredIndex) | 将数据存储与索引放到了一块，索引结构的叶子节点保存了行数据 | 必须有,而且只有一个 |
| 二级索引(SecondaryIndex) | 将数据与索引分开存储，索引结构的叶子节点关联的是对应的主键 | 可以存在多个 |
聚集索引选取规则:
- 如果存在主键，主键索引就是聚集索引。
- 如果不存在主键，将使用第一个唯一（`UNIQUE`）索引作为聚集索引。
- 如果表没有主键，或没有合适的唯一索引，则InnoDB会自动生成一个rowid作为隐藏的聚集索引。

聚集索引和二级索引的具体结构如下：
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240120220731.png?" alt="image.png" style="zoom:50%;" />
- 聚集索引的叶子节点下挂的是这一行的数据 。
- 二级索引的叶子节点下挂的是该字段值对应的主键值。

接下来，我们来分析一下，当我们执行如下的SQL语句时，具体的查找过程是什么样子的。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240120220958.png?" alt="image.png" style="zoom:50%;" />

具体过程如下:
1. 由于是根据name字段进行查询，所以先根据name='Arm'到name字段的二级索引中进行匹配查找。但是在二级索引中只能查找到 Arm 对应的主键值 10。
2. 由于查询返回的数据是*，所以此时，还需要根据主键值10，到聚集索引中查找10对应的记录，最终找到10对应的行row。
3. 最终拿到这一行的数据，直接返回即可。

> 回表查询： 这种先到二级索引中查找数据，找到主键值，然后再到聚集索引中根据主键值，获取数据的方式，就称之为回表查询。 


> [!question] 思考题：
> InnoDB主键索引的B+tree高度为多高呢?         
> - 假设:
> 	- 一行数据大小为1k，一页中可以存储16行这样的数据。InnoDB的指针占用6个字节的空间，主键即使为bigint，占用字节数为8。
> - 高度为2：
> 	- `n * 8 + (n + 1) * 6 = 16*1024` , 算出n约为 1170
> 	- `1171* 16 = 18736`
> 	- 也就是说，如果树的高度为2，则可以存储 18000 多条记录。
> - 高度为3：
> 	- `1171 * 1171 * 16 = 21939856`
> 	- 也就是说，如果树的高度为3，则可以存储 2200w 左右的记录。


### SQL性能分析
#### SQL执行频率

MySQL 客户端连接成功后，通过 `show [session|global] status` 命令可以提供服务器状态信息。通过如下指令，可以查看当前数据库的INSERT、UPDATE、DELETE、SELECT的访问频次：

```sh
-- session 是查看当前会话 ;
-- global 是查询全局数据 ;
SHOW GLOBAL STATUS LIKE 'Com_______';
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121105729.png" alt="image.png" style="zoom:60%;" />
Com_delete: 删除次数
Com_insert: 插入次数
Com_select: 查询次数
Com_update: 更新次数
我们可以在当前数据库再执行几次查询操作，然后再次查看执行频次，看看 `Com_select` 参数会不会变化。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121105847.png?" alt="image.png" style="zoom:60%;" />

#### 慢查询日志
慢查询日志记录了所有执行时间超过指定参数（long_query_time，单位：秒，默认10秒）的所有SQL语句的日志。
MySQL的慢查询日志默认没有开启，我们可以查看一下系统变量 `slow_query_log`。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121104431.png?" alt="image.png" style="zoom:60%;" />

如果要开启慢查询日志，需要在MySQL的配置文件（`/etc/my.cnf`）中配置如下信息：

```sh
# 开启MySQL慢日志查询开关
slow_query_log=1
# 设置慢日志的时间为2秒，SQL语句执行时间超过2秒，就会视为慢查询，记录慢查询日志
long_query_time=2
```

配置完毕之后，通过以下指令重新启动MySQL服务器进行测试，查看慢日志文件中记录的信息`/var/lib/mysql/localhost-slow.log`。

```sh
systemctl restart mysqld 
```

然后，再次查看开关情况，慢查询日志就已经打开了。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121104559.png?" alt="image.png" style="zoom:60%;" />

测试：
A. 执行如下SQL语句 ：
```sql
select * from tb_user; -- 这条SQL执行效率比较高, 执行耗时 0.00sec
select count(*) from tb_sku; -- 由于tb_sku表中, 预先存入了1000w的记录, count一次,耗时13.35sec
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121104644.png?" alt="image.png" style="zoom:50%;" />
B. 检查慢查询日志 ：
最终我们发现，在慢查询日志中，只会记录执行时间超多我们预设时间（2s）的SQL，执行较快的SQL是不会记录的。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121104709.png?" alt="image.png" style="zoom:60%;" />
那这样，通过慢查询日志，就可以定位出执行效率比较低的SQL，从而有针对性的进行优化。

#### profile详情

`show profiles` 能够在做SQL优化时帮助我们了解时间都耗费到哪里去了。通过have_profiling参数，能够看到当前MySQL是否支持profile操作：

```sql
SELECT @@have_profiling ; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121110003.png?" alt="image.png" style="zoom:50%;" />

可以看到，当前MySQL是支持 profile操作的，但是开关是关闭的。可以通过set语句在`session/global`级别开启profiling：

```sql
SET profiling = 1; 
```

开关已经打开了，接下来，我们所执行的SQL语句，都会被MySQL记录，并记录执行时间消耗到哪儿去了。 我们直接执行如下的SQL语句：
```sql
select * from tb_user;
select * from tb_user where id = 1;
select * from tb_user where name = '白起';
select count(*) from tb_sku;
```
执行一系列的业务SQL的操作，然后通过如下指令查看指令的执行耗时：
```sql
-- 查看每一条SQL的耗时基本情况
show profiles;
-- 查看指定query_id的SQL语句各个阶段的耗时情况
show profile for query query_id;
-- 查看指定query_id的SQL语句CPU的使用情况
show profile cpu for query query_id;
```

查看每一条SQL的耗时情况:

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121110125.png?" alt="image.png" style="zoom:60%;" />

查看指定SQL各个阶段的耗时情况 :
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121110159.png?" alt="image.png" style="zoom:60%;" />
#### explain
EXPLAIN 或者 DESC命令获取 MySQL 如何执行 SELECT 语句的信息，包括在 SELECT 语句执行过程中表**如何连接和连接**的顺序。
语法:
```sql
-- 直接在select语句之前加上关键字 explain / desc
EXPLAIN SELECT 字段列表 FROM 表名 WHERE 条件 ;
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121113634.png?" alt="image.png" style="zoom:70%;" />

Explain 执行计划中各个字段的含义:

| 字段 | 含义 |
| :--: | :--: |
| id | select查询的序列号，表示查询中执行select子句或者是操作表的顺序(id相同，执行顺序从上到下；id不同，值越大，越先执行)。 |
| select_type | 表示 SELECT 的类型，常见的取值有 SIMPLE（简单表，即不使用表连接或者子查询）、PRIMARY（主查询，即外层的查询）、UNION（UNION 中的第二个或者后面的查询语句）、SUBQUERY（SELECT/WHERE之后包含了子查询）等 |
| type | 表示连接类型，性能由好到差的连接类型为NULL、system、const、eq_ref、ref、range、 index、all 。 |
| possible_key | 显示可能应用在这张表上的索引，一个或多个。 |
| key | 实际使用的索引，如果为NULL，则没有使用索引。 |
| key_len | 表示索引中使用的字节数， 该值为索引字段最大可能长度，并非实际使用长度，在不损失精确性的前提下， 长度越短越好 。 |
| rows | MySQL认为必须要执行查询的行数，在innodb引擎的表中，是一个估计值，可能并不总是准确的。 |
| filtered | 表示返回结果的行数占需读取行数的百分比， filtered 的值越大越好。 |
### 索引使用
#### 验证索引效率

在讲解索引的使用原则之前，先通过一个简单的案例，来验证一下索引，看看是否能够通过索引来提升数据查询性能。在演示的时候，我们还是使用之前准备的一张表 `tb_sku` , 在这张表中准备了1000w的记录。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121114718.png?=" alt="image.png" style="zoom:60%;" />
这张表中id为主键，有主键索引，而其他字段是没有建立索引的。 我们先来查询其中的一条记录，看看里面的字段情况，执行如下SQL：

```sql
select * from tb_sku where id = 1\G; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121114844.png?" alt="image.png" style="zoom:60%;" />
可以看到即使有1000w的数据,根据id进行数据查询,性能依然很快，因为主键id是有索引的。 那么接下来，我们再来根据 sn 字段进行查询，执行如下SQL：

```sql
SELECT * FROM tb_sku WHERE sn = '100000003145001'; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121114922.png?" alt="image.png" style="zoom:50%;" />

我们可以看到根据sn字段进行查询，查询返回了一条数据，结果耗时 20.78sec，就是因为sn没有索引，而造成查询效率很低。
那么我们可以针对于sn字段，建立一个索引，建立了索引之后，我们再次根据sn进行查询，再来看一下查询耗时情况。
创建索引：
```sql
create index idx_sku_sn on tb_sku(sn) ; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121115015.png?" alt="image.png" style="zoom:50%;" />

然后再次执行相同的SQL语句，再次查看SQL的耗时。
```sql
SELECT * FROM tb_sku WHERE sn = '100000003145001'; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121115513.png?" alt="image.png" style="zoom:50%;" />

我们明显会看到，sn字段建立了索引之后，查询性能大大提升。建立索引前后，查询耗时都不是一个数量级的。

#### 最左前缀法则
如果索引了多列（联合索引），要遵守最左前缀法则。最左前缀法则指的是查询从索引的最左列开始，并且不跳过索引中的列。如果跳跃某一列，索引将会部分失效(后面的字段索引失效)。
以 tb_user 表为例，我们先来查看一下之前 tb_user 表所创建的索引。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121115552.png?" alt="image.png" style="zoom:60%;" />

在 tb_user 表中，有一个联合索引，这个联合索引涉及到三个字段，顺序分别为：profession，age，status。
对于最左前缀法则指的是，查询时，最左变的列，也就是profession必须存在，否则索引全部失效。而且中间不能跳过某一列，否则该列后面的字段索引将失效。 接下来，我们来演示几组案例，看一下
具体的执行计划：

```sql
explain select * from tb_user where profession = '软件工程' and age = 31 and status= '0';
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121115646.png?" alt="image.png" style="zoom:60%;" />

```sql
explain select * from tb_user where profession = '软件工程' and age = 31;
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121115717.png?" alt="image.png" style="zoom:60%;" />
```sql
explain select * from tb_user where profession = '软件工程'; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121115750.png?" alt="image.png" style="zoom:60%;" />

以上的这三组测试中，我们发现只要联合索引最左边的字段 profession存在，索引就会生效，只不过索引的长度不同。 而且由以上三组测试，我们也可以推测出profession字段索引长度为47、age字段索引长度为2、status字段索引长度为5。

```sql
explain select * from tb_user where age = 31 and status = '0'; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121115829.png?" alt="image.png" style="zoom:60%;" />
而通过上面的这两组测试，我们也可以看到索引并未生效，原因是因为不满足最左前缀法则，联合索引最左边的列profession不存在。
```sql
explain select * from tb_user where profession = '软件工程' and status = '0';
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121115922.png?" alt="image.png" style="zoom:60%;" />
上述的SQL查询时，存在profession字段，最左边的列是存在的，索引满足最左前缀法则的基本条件。但是查询时，跳过了age这个列，所以后面的列索引是不会使用的，也就是索引部分生效，所以索引的长度就是47。



> [!question] 思考题：
> - 当执行SQL语句: explain select * from tb_user where age = 31 and status = '0' and profession = '软件工程'； 时，是否满足最左前缀法则，走不走上述的联合索引，索引长度？
> 	
> 	- <img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121120020.png" alt="image.png" style="zoom:60%;" />
> 	- 可以看到，是完全满足最左前缀法则的，索引长度54，联合索引是生效的。
> - 注意 ： 最左前缀法则中指的最左边的列，是指在查询时，联合索引的最左边的字段(即是第一个字段)必须存在，与我们编写SQL时，条件编写的先后顺序无关。

#### 范围查询
联合索引中，出现范围查询(>,<)，范围查询右侧的列索引失效。

```sql
explain select * from tb_user where profession = '软件工程' and age > 30 and status = '0';
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121120234.png?" alt="image.png" style="zoom:60%;" />
当范围查询使用>或<时，走联合索引了，但是索引的长度为49，就说明范围查询右边的status字段是没有走索引的。

```sql
explain select * from tb_user where profession = '软件工程' and age >= 30 and status = '0';
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121120314.png?" alt="image.png" style="zoom:60%;" />

当范围查询使用>= 或 <= 时，走联合索引了，但是索引的长度为54，就说明所有的字段都是走索引的。
所以，在业务允许的情况下，尽可能的使用类似于 >= 或 <= 这类的范围查询，而避免使用 > 或 <。

#### 索引失效情况

##### 索引列运算

不要在索引列上进行运算操作， 索引将失效。
在tb_user表中，除了前面介绍的联合索引之外，还有一个索引，是phone字段的单列索引。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121120738.png?" alt="image.png" style="zoom:60%;" />
当根据phone字段进行等值匹配查询时, 索引生效。

```sql
explain select * from tb_user where phone = '17799990015'; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121120814.png?" alt="image.png" style="zoom:60%;" />

当根据phone字段进行函数运算操作之后，索引失效。

```sql
explain select * from tb_user where substring(phone,10,2) = '15'; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121120853.png?" alt="image.png" style="zoom:60%;" />

##### 字符串不加引号
字符串类型字段使用时，不加引号，索引将失效。
接下来，我们通过两组示例，来看看对于字符串类型的字段，加单引号与不加单引号的区别：

```sql
explain select * from tb_user where profession = '软件工程' and age = 31 and status= '0';
explain select * from tb_user where profession = '软件工程' and age = 31 and status= 0;
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121120955.png" alt="image.png" style="zoom:60%;" />
```sql
explain select * from tb_user where phone = '17799990015';
explain select * from tb_user where phone = 17799990015;
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121121025.png" alt="image.png" style="zoom:60%;" />

经过上面两组示例，我们会明显的发现，如果字符串不加单引号，对于查询结果，没什么影响，但是数据库存在隐式类型转换，索引将失效。

##### 模糊查询
如果仅仅是尾部模糊匹配，索引不会失效。如果是头部模糊匹配，索引失效。
接下来，我们来看一下这三条SQL语句的执行效果，查看一下其执行计划：
由于下面查询语句中，都是根据profession字段查询，符合最左前缀法则，联合索引是可以生效的，我们主要看一下，模糊查询时，%加在关键字之前，和加在关键字之后的影响。

```sql
explain select * from tb_user where profession like '软件%';
explain select * from tb_user where profession like '%工程';
explain select * from tb_user where profession like '%工%';
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121121129.png?" alt="image.png" style="zoom:60%;" />
经过上述的测试，我们发现，在like模糊查询中，在关键字后面加%，索引可以生效。而如果在关键字前面加了%，索引将会失效。

##### or连接条件
用or分割开的条件， 如果or前的条件中的列有索引，而后面的列中没有索引，那么涉及的索引都不会被用到。
```sql
explain select * from tb_user where id = 10 or age = 23;
explain select * from tb_user where phone = '17799990017' or age = 23;
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121121220.png?" alt="image.png" style="zoom:60%;" />
由于age没有索引，所以即使id、phone有索引，索引也会失效。所以需要针对于age也要建立索引。然后，我们可以对age字段建立索引。

```sql
create index idx_user_age on tb_user(age); 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121121305.png" alt="image.png" style="zoom:60%;" />
建立了索引之后，我们再次执行上述的SQL语句，看看前后执行计划的变化。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121121330.png" alt="image.png" style="zoom:60%;" />
最终，我们发现，当or连接的条件，左右两侧字段都有索引时，索引才会生效。

##### 数据分布影响
如果MySQL评估使用索引比全表更慢，则不使用索引。
```sql
select * from tb_user where phone >= '17799990005';
select * from tb_user where phone >= '17799990015';
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121121418.png" alt="image.png" style="zoom:60%;" />
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121121436.png" alt="image.png" style="zoom:60%;" />

经过测试我们发现，相同的SQL语句，只是传入的字段值不同，最终的执行计划也完全不一样，这是为什么呢？
就是因为MySQL在查询时，会评估使用索引的效率与走全表扫描的效率，如果走全表扫描更快，则放弃索引，走全表扫描。 因为索引是用来索引少量数据的，如果通过索引查询返回大批量的数据，则还不如走全表扫描来的快，此时索引就会失效。
接下来，我们再来看看 is null 与 is not null 操作是否走索引。
执行如下两条语句 ：
```sql
explain select * from tb_user where profession is null;
explain select * from tb_user where profession is not null;
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121121525.png" alt="image.png" style="zoom:60%;" />


接下来，我们做一个操作将profession字段值全部更新为null。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121121600.png" alt="image.png" style="zoom:50%;" />
然后，再次执行上述的两条SQL，查看SQL语句的执行计划。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121121629.png" alt="image.png" style="zoom:60%;" />
最终我们看到，一模一样的SQL语句，先后执行了两次，结果查询计划是不一样的，为什么会出现这种现象，这是和数据库的数据分布有关系。查询时MySQL会评估，走索引快，还是全表扫描快，如果全表扫描更快，则放弃索引走全表扫描。 因此，is null 、is not null是否走索引，得具体情况具体分析，并不是固定的。

#### SQL提示
目前tb_user表的数据情况如下:
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121180153.png?" alt="image.png" style="zoom:50%;" />
索引情况如下:
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121180215.png" alt="image.png" style="zoom:50%;" />

把上述的 `idx_user_age, idx_email` 这两个之前测试使用过的索引直接删除。
```sql
drop index idx_user_age on tb_user;
drop index idx_email on tb_user;
```

执行SQL : `explain select * from tb_user where profession = '软件工程'`;
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121180304.png?" alt="image.png" style="zoom:60%;" />
查询走了联合索引。

执行SQL，创建profession的单列索引：`create index idx_user_pro on tb_user(profession)`;

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121180345.png" alt="image.png" style="zoom:60%;" />

创建单列索引后，再次执行A中的SQL语句，查看执行计划，看看到底走哪个索引。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121180414.png" alt="image.png" style="zoom:60%;" />
测试结果，我们可以看到，possible_keys中 idx_user_pro_age_sta,idx_user_pro 这两个索引都可能用到，最终MySQL选择了idx_user_pro_age_sta索引。这是MySQL自动选择的结果。
那么，我们能不能在查询的时候，自己来指定使用哪个索引呢？ 答案是肯定的，此时就可以借助于MySQL的SQL提示来完成。 接下来，介绍一下SQL提示。

SQL提示，是优化数据库的一个重要手段，简单来说，就是在SQL语句中加入一些人为的提示来达到优化操作的目的。
1). use index ： 建议MySQL使用哪一个索引完成此次查询（仅仅是建议，mysql内部还会再次进行评估）。
```sql
explain select * from tb_user use index(idx_user_pro) where profession = '软件工程';
```
2). ignore index ： 忽略指定的索引。
```sql
explain select * from tb_user ignore index(idx_user_pro) where profession = '软件工程';
```
3). force index ： 强制使用索引。
```sql
explain select * from tb_user force index(idx_user_pro) where profession = '软件工程';
```

示例演示：
A. use index 
```sql
explain select * from tb_user use index(idx_user_pro) where profession = '软件工程';
```
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121180634.png?" alt="image.png" style="zoom:60%;" />
B. ignore index
```sql
explain select * from tb_user ignore index(idx_user_pro) where profession = '软件工程';
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121180719.png?" alt="image.png" style="zoom:60%;" />
C. force index 
```sql
explain select * from tb_user force index(idx_user_pro_age_sta) where profession ='软件工程';
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121180757.png" alt="image.png" style="zoom:60%;" />

#### 覆盖索引

尽量使用覆盖索引，减少`select *`。 那么什么是覆盖索引呢？ 覆盖索引是指查询使用了索引，并且需要返回的列，在该索引中已经全部能够找到 。

接下来，我们来看一组SQL的执行计划，看看执行计划的差别，然后再来具体做一个解析。
```sql
explain select id, profession from tb_user where profession = '软件工程' and age = 31 and status = '0' ;
explain select id,profession,age, status from tb_user where profession = '软件工程' and age = 31 and status = '0' ;
explain select id,profession,age, status, name from tb_user where profession = '软件工程' and age = 31 and status = '0' ;
explain select * from tb_user where profession = '软件工程' and age = 31 and status = '0';
```
上述这几条SQL的执行结果为:
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121182651.png" alt="image.png" style="zoom:60%;" />
从上述的执行计划我们可以看到，这四条SQL语句的执行计划前面所有的指标都是一样的，看不出来差异。但是此时，我们主要关注的是后面的Extra，前面两天SQL的结果为 `Using where; Using Index` ; 而后面两条SQL的结果为: `Using index condition`。

根据下面的内容写一个2列的markdown图表
Extra 含义
Using where; Using Index 查找使用了索引，但是需要的数据都在索引列中能找到，所以不需要回表查询数据
Using index condition 查找使用了索引，但是需要回表查询数据

| Extra | 含义 |
| :--: | :--: |
| Using where; Using Index | 查找使用了索引，但是需要的数据都在索引列中能找到，所以不需要回表查询数据 |
| Using index condition | 查找使用了索引，但是需要回表查询数据 |

因为，在tb_user表中有一个联合索引 idx_user_pro_age_sta，该索引关联了三个字段profession、age、status，而这个索引也是一个二级索引，所以叶子节点下面挂的是这一行的主键id。 所以当我们查询返回的数据在id、profession、age、status之中，则直接走二级索引直接返回数据了。 如果超出这个范围，就需要拿到主键id，再去扫描聚集索引，再获取额外的数据了，这个过程就是回表。 而我们如果一直使用select * 查询返回所有字段值，很容易就会造成回表
查询（除非是根据主键查询，此时只会扫描聚集索引）。

为了大家更清楚的理解，什么是覆盖索引，什么是回表查询，我们一起再来看下面的这组SQL的执行过程。
A. 表结构及索引示意图:

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121182935.png" alt="image.png" style="zoom:50%;" />
id是主键，是一个聚集索引。 name字段建立了普通索引，是一个二级索引（辅助索引）。

B. 执行SQL : select * from tb_user where id = 2;

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121183004.png" alt="image.png" style="zoom:50%;" />
根据id查询，直接走聚集索引查询，一次索引扫描，直接返回数据，性能高。
C. 执行SQL：selet id,name from tb_user where name = 'Arm';

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121183029.png" alt="image.png" style="zoom:50%;" />
虽然是根据name字段查询，查询二级索引，但是由于查询返回在字段为 id，name，在name的二级索引中，这两个值都是可以直接获取到的，因为覆盖索引，所以不需要回表查询，性能高。

D. 执行SQL：selet id,name,gender from tb_user where name = 'Arm';

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121183106.png" alt="image.png" style="zoom:50%;" />
由于在name的二级索引中，不包含gender，所以，需要两次索引扫描，也就是需要回表查询，性能相对较差一点。 


> [!question] 思考题：
> - 一张表, 有四个字段(id, username, password, status), 由于数据量大, 需要对以下SQL语句进行优化, 该如何进行才是最优方案:
> - `select id,username,password from tb_user where username ='itcast'`;
> - 答案: 针对于 username, password建立联合索引, sql为: create index idx_user_name_pass on tb_user(username,password);
> - 这样可以避免上述的SQL语句，在查询的过程中，出现回表查询。


#### 前缀索引
当字段类型为字符串（varchar，text，longtext等）时，有时候需要索引很长的字符串，这会让索引变得很大，查询时，浪费大量的磁盘IO， 影响查询效率。此时可以只将字符串的一部分前缀，建立索引，这样可以大大节约索引空间，从而提高索引效率。

1. 语法
```sql
create index idx_xxxx on table_name(column(n)) ; 
```

示例:
为tb_user表的email字段，建立长度为5的前缀索引。
```sql
create index idx_email_5 on tb_user(email(5)); 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121195417.png?" alt="image.png" style="zoom:60%;" />
2. 前缀长度
可以根据索引的选择性来决定，而选择性是指不重复的索引值（基数）和数据表的记录总数的比值，
索引选择性越高则查询效率越高， 唯一索引的选择性是1，这是最好的索引选择性，性能也是最好的。

```sql
select count(distinct email) / count(*) from tb_user ;
select count(distinct substring(email,1,5)) / count(*) from tb_user;
```

3. 前缀索引的查询流程

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121195524.png?" alt="image.png" style="zoom:60%;" />

#### 单列索引与联合索引
单列索引：即一个索引只包含单个列。
联合索引：即一个索引包含了多个列。
我们先来看看tb_user表中目前的索引情况:
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121200046.png" alt="image.png" style="zoom:60%;" />
在查询出来的索引中，既有单列索引，又有联合索引。
接下来，我们来执行一条SQL语句，看看其执行计划：

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121200115.png" alt="image.png" style="zoom:60%;" />
通过上述执行计划我们可以看出来，在and连接的两个字段phone、name上都是有单列索引的，但是最终mysql只会选择一个索引，也就是说，只能走一个字段的索引，此时是会回表查询的。
紧接着，我们再来创建一个phone和name字段的联合索引来查询一下执行计划。

```sql
create unique index idx_user_phone_name on tb_user(phone,name); 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121200158.png" alt="image.png" style="zoom:60%;" />
此时，查询时，就走了联合索引，而在联合索引中包含phone、name的信息，在叶子节点下挂的是对应的主键id，所以查询是无需回表查询的。

> 在业务场景中，如果存在多个查询条件，考虑针对于查询字段建立索引时，建议建立联合索引，而非单列索引。 

如果查询使用的是联合索引，具体的结构示意图如下：
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121200256.png" alt="image.png" style="zoom:50%;" />


### 索引设计原则

1. 针对于数据量较大，且查询比较频繁的表建立索引。
2. 针对于常作为查询条件（where）、排序（order by）、分组（group by）操作的字段建立索引。
3. 尽量选择区分度高的列作为索引，尽量建立唯一索引，区分度越高，使用索引的效率越高。
4. 如果是字符串类型的字段，字段的长度较长，可以针对于字段的特点，建立前缀索引。
5. 尽量使用联合索引，减少单列索引，查询时，联合索引很多时候可以覆盖索引，节省存储空间，避免回表，提高查询效率。
6. 要控制索引的数量，索引并不是多多益善，索引越多，维护索引结构的代价也就越大，会影响增删改的效率。
7. 如果索引列不能存储NULL值，请在创建表时使用NOT NULL约束它。当优化器知道每列是否包含NULL值时，它可以更好地确定哪个索引最有效地用于查询。

## SQL优化
### 插入数据
### insert

如果我们需要一次性往数据库表中插入多条记录，可以从以下三个方面进行优化。
```sql
insert into tb_test values(1,'tom');
insert into tb_test values(2,'cat');
insert into tb_test values(3,'jerry');
.....
```

1). 优化方案一
批量插入数据 
```sql
Insert into tb_test values(1,'Tom'),(2,'Cat'),(3,'Jerry'); 
```

2). 优化方案二
手动控制事务
```sql
start transaction;
insert into tb_test values(1,'Tom'),(2,'Cat'),(3,'Jerry');
insert into tb_test values(4,'Tom'),(5,'Cat'),(6,'Jerry');
insert into tb_test values(7,'Tom'),(8,'Cat'),(9,'Jerry');
commit;
```

3). 优化方案三
主键顺序插入，性能要高于乱序插入。
```sql
主键乱序插入 : 8 1 9 21 88 2 4 15 89 5 7 3
主键顺序插入 : 1 2 3 4 5 7 8 9 15 21 88 89
```

#### 大批量插入数据
如果一次性需要插入大批量数据(比如: 几百万的记录)，使用insert语句插入性能较低，此时可以使
用MySQL数据库提供的**load指令**进行插入。操作如下：

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121204444.png?" alt="image.png" style="zoom:50%;" />
可以执行如下指令，将数据脚本文件中的数据加载到表结构中：
```sql
-- 客户端连接服务端时，加上参数 -–local-infile
mysql –-local-infile -u root -p
-- 设置全局参数local_infile为1，开启从本地加载文件导入数据的开关
set global local_infile = 1;
-- 执行load指令将准备好的数据，加载到表结构中
load data local infile '/root/sql1.log' into table tb_user fields
terminated by ',' lines terminated by '\n' ;
```

> 主键顺序插入性能高于乱序插入

示例演示:
A. 创建表结构

```sql
CREATE TABLE `tb_user` (
`id` INT(11) NOT NULL AUTO_INCREMENT,
`username` VARCHAR(50) NOT NULL,
`password` VARCHAR(50) NOT NULL,
`name` VARCHAR(20) NOT NULL,
`birthday` DATE DEFAULT NULL,
`sex` CHAR(1) DEFAULT NULL,
PRIMARY KEY (`id`),
UNIQUE KEY `unique_user_username` (`username`)
) ENGINE=INNODB DEFAULT CHARSET=utf8 ;
```

B. 设置参数
```sql
-- 客户端连接服务端时，加上参数 -–local-infile
mysql –-local-infile -u root -p
-- 设置全局参数local_infile为1，开启从本地加载文件导入数据的开关
set global local_infile = 1;
```

C. load加载数据

```sql
load data local infile '/root/load_user_100w_sort.sql' into table tb_user fields terminated by ',' lines terminated by '\n' ;
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121204659.png?" alt="image.png" style="zoom:60%;" />

我们看到，插入100w的记录，17s就完成了，性能很好。

> 在load时，主键顺序插入性能高于乱序插入

### 主键优化

在上一小节，我们提到，主键顺序插入的性能是要高于乱序插入的。 这一小节，就来介绍一下具体的原因，然后再分析一下主键又该如何设计。
1). 数据组织方式
在InnoDB存储引擎中，表数据都是根据主键顺序组织存放的，这种存储方式的表称为索引组织表(index organized table IOT)。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121205453.png" alt="image.png" style="zoom:50%;" />

行数据，都是存储在聚集索引的叶子节点上的。而我们之前也讲解过InnoDB的逻辑结构图：
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121205517.png" alt="image.png" style="zoom:50%;" />
在InnoDB引擎中，数据行是记录在逻辑结构 page 页中的，而每一个页的大小是固定的，默认16K。那也就意味着， 一个页中所存储的行也是有限的，如果插入的数据行row在该页存储不小，将会存储到下一个页中，**页与页之间会通过指针连接**。

2). 页分裂
页可以为空，也可以填充一半，也可以填充100%。每个页包含了2-N行数据(如果一行数据过大，会行溢出)，根据主键排列。
A. 主键顺序插入效果
①. 从磁盘中申请页， 主键顺序插入
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121205631.png" alt="image.png" style="zoom:50%;" />
②. 第一个页没有满，继续往第一页插入
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121205653.png" alt="image.png" style="zoom:50%;" />

③. 当第一个也写满之后，再写入第二个页，页与页之间会通过指针连接
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121205719.png" alt="image.png" style="zoom:50%;" />

④. 当第二页写满了，再往第三页写入
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121205742.png" alt="image.png" style="zoom:50%;" />
B. 主键乱序插入效果
①. 加入1#,2#页都已经写满了，存放了如图所示的数据
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121205808.png" alt="image.png" style="zoom:50%;" />
②. 此时再插入id为50的记录，我们来看看会发生什么现象
会再次开启一个页，写入新的页中吗？

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121205832.png" alt="image.png" style="zoom:50%;" />

不会。因为，索引结构的叶子节点是有顺序的。按照顺序，应该存储在47之后。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121205856.png" alt="image.png" style="zoom:50%;" />
但是47所在的1#页，已经写满了，存储不了50对应的数据了。 那么此时会开辟一个新的页 3#。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121205918.png" alt="image.png" style="zoom:50%;" />
但是并不会直接将50存入3#页，而是会将1#页后一半的数据，移动到3#页，然后在3#页，插入50。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121205948.png" alt="image.png" style="zoom:50%;" />
移动数据，并插入id为50的数据之后，那么此时，这三个页之间的数据顺序是有问题的。 1#的下一个页，应该是3#， 3#的下一个页是2#。 所以，此时，需要重新设置链表指针。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121210013.png" alt="image.png" style="zoom:50%;" />
上述的这种现象，称之为 "**页分裂**"，是比较耗费性能的操作。

3). 页合并
目前表中已有数据的索引结构(叶子节点)如下：
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121210043.png" alt="image.png" style="zoom:50%;" />
当我们对已有数据进行删除时，具体的效果如下:
当删除一行记录时，实际上记录并没有被物理删除，只是记录被标记（flaged）为删除并且它的空间变得允许被其他记录声明使用。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121210111.png" alt="image.png" style="zoom:50%;" />
当我们继续删除2#的数据记录
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121210131.png" alt="image.png" style="zoom:50%;" />
当页中删除的记录达到 `MERGE_THRESHOLD`（默认为页的50%），InnoDB会开始寻找最靠近的页（前或后）看看是否可以将两个页合并以优化空间使用。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121210215.png" alt="image.png" style="zoom:50%;" />
删除数据，并将页合并之后，再次插入新的数据21，则直接插入3#页
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121210240.png" alt="image.png" style="zoom:50%;" />
这个里面所发生的合并页的这个现象，就称之为 "页合并"。
> 知识小贴士：
> 	MERGE_THRESHOLD：合并页的阈值，可以自己设置，在创建表或者创建索引时指定。


4). 索引设计原则
- 满足业务需求的情况下，尽量降低主键的长度。
- 插入数据时，尽量选择顺序插入，选择使用AUTO_INCREMENT自增主键。
- 尽量不要使用UUID做主键或者是其他自然主键，如身份证号。
- 业务操作时，避免对主键的修改。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121210343.png?" alt="image.png" style="zoom:90%;" />
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240121210411.png?" alt="image.png" style="zoom:90%;" />

### order by优化
MySQL的排序，有两种方式：
<font color=#ff0000>Using filesort</font> : 通过表的索引或全表扫描，读取满足条件的数据行，然后在**排序缓冲区sort buffer**中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫 FileSort 排序。
<font color=#ff0000>Using index</font> : 通过有序索引顺序扫描**直接返回有序数据**，这种情况即为 using index，不需要额外排序，操作效率高。
对于以上的两种排序方式，Using index的性能高，而Using filesort的性能低，我们在优化排序操作时，尽量要优化为 Using index。

接下来，我们来做一个测试：
**执行排序SQL**
```sql
explain select id,age,phone from tb_user order by age ; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240126204550.png" alt="image.png" style="zoom:60%;" />

```sql
explain select id,age,phone from tb_user order by age, phone ; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240126204659.png" alt="image.png" style="zoom:60%;" />
由于 age, phone 都没有索引，所以此时再排序时，出现Using filesort， 排序性能较低。

**创建索引**
```sql
-- 创建索引
create index idx_user_age_phone_aa on tb_user(age,phone);
```

创建索引后，根据age, phone进行升序排序
```sql
explain select id,age,phone from tb_user order by age; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240126204805.png" alt="image.png" style="zoom:60%;" />

```sql
explain select id,age,phone from tb_user order by age , phone; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240126205202.png" alt="image.png" style="zoom:60%;" />

建立索引之后，再次进行排序查询，就由原来的Using filesort， 变为了 Using index，性能
就是比较高的了。

**创建索引后，根据age, phone进行降序排序**

```sql
explain select id,age,phone from tb_user order by age desc , phone desc ; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240126221608.png" alt="image.png" style="zoom:60%;" />
也出现 Using index， 但是此时Extra中出现了 Backward index scan，这个代表反向扫描索引，因为在MySQL中我们创建的索引，默认索引的叶子节点是从小到大排序的，而此时我们查询排序时，是从大到小，所以，在扫描时，就是反向扫描，就会出现 Backward index scan。 在MySQL8版本中，支持降序索引，我们也可以创建降序索引。

**根据phone，age进行升序排序，phone在前，age在后。**

```sql
explain select id,age,phone from tb_user order by phone , age; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240126221720.png" alt="image.png" style="zoom:60%;" />
排序时,也需要满足最左前缀法则,否则也会出现 filesort。因为在创建索引的时候， age是第一个字段，phone是第二个字段，所以排序时，也就该按照这个顺序来，否则就会出现 `Using filesort`。


**根据age, phone进行降序一个升序，一个降序**
```sql
explain select id,age,phone from tb_user order by age asc , phone desc ; 
```
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240126221854.png" alt="image.png" style="zoom:60%;" />
因为创建索引时，如果未指定顺序，默认都是按照升序排序的，而查询时，一个升序，一个降序，此时就会出现Using filesort。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240126221943.png" alt="image.png" style="zoom:60%;" />
为了解决上述的问题，我们可以创建一个索引，这个联合索引中 age 升序排序，phone 倒序排序。

**创建联合索引(age 升序排序，phone 倒序排序)**

```sql
create index idx_user_age_phone_ad on tb_user(age asc ,phone desc); 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240126222030.png" alt="image.png" style="zoom:60%;" />

**然后再次执行如下SQL**
```sql
explain select id,age,phone from tb_user order by age asc , phone desc; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240126222156.png" alt="image.png" style="zoom:60%;" />

**升序/降序联合索引结构图示**:

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240126222330.png" alt="image.png" style="zoom:60%;" />
由上述的测试,我们得出order by优化原则:
A. 根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则。
B. 尽量使用覆盖索引。
C. 多字段排序, 一个升序一个降序，此时需要注意联合索引在创建时的规则（ASC/DESC）。
D. 如果不可避免的出现filesort，大数据量排序时，可以适当增大排序缓冲区大小`sort_buffer_size`(默认256k)。

### group by优化

在没有索引的情况下，执行如下SQL，查询执行计划：
```sql
explain select profession , count(*) from tb_user group by profession ;
```
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240126223518.png" alt="image.png" style="zoom:60%;" />
然后，我们在针对于 profession ， age， status 创建一个联合索引。
```sql
create index idx_user_pro_age_sta on tb_user(profession , age , status); 
```

紧接着，再执行前面相同的SQL查看执行计划。

```sql
explain select profession , count(*) from tb_user group by profession ; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240126223737.png?" alt="image.png" style="zoom:60%;" />
再执行如下的分组查询SQL，查看执行计划：

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240126223816.png?" alt="image.png" style="zoom:60%;" />
我们发现，如果仅仅根据age分组，就会出现 Using temporary ；而如果是 根据profession,age两个字段同时分组，则不会出现 Using temporary。原因是因为对于分组操作，在联合索引中，也是符合**最左前缀法则的**。
所以，在分组操作中，我们需要通过以下两点进行优化，以提升性能：
A. 在分组操作时，可以通过索引来提高效率。
B. 分组操作时，索引的使用也是满足最左前缀法则的。

### limit优化
在数据量比较大时，如果进行limit分页查询，在查询时，越往后，分页查询效率越低。
我们一起来看看执行limit分页查询耗时对比：
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240127000508.png" alt="image.png" style="zoom:60%;" />
通过测试我们会看到，越往后，分页查询效率越低，这就是分页查询的问题所在。
因为，当在进行分页查询时，如果执行 limit 2000000,10 ，此时**需要MySQL排序前2000010记录**，仅仅返回 2000000 - 2000010 的记录，其他记录丢弃，查询排序的代价非常大。

优化思路: 一般分页查询时，通过创建覆盖索引 能够比较好地提高性能，可以通过**覆盖索引加子查询形式**进行优化。
```sql
explain select * from tb_sku t , (select id from tb_sku order by id
limit 2000000,10) a where t.id = a.id;
```

### count优化
#### 概述

```sql
select count(*) from tb_user ; 
```

在之前的测试中，我们发现，如果数据量很大，在执行count操作时，是非常耗时的。
MyISAM 引擎把一个表的总行数存在了磁盘上，因此执行 `count(*)` 的时候会直接返回这个数，效率很高； 但是如果是带条件的count，MyISAM也慢。
InnoDB 引擎就麻烦了，它执行 `count(*)` 的时候，需要把数据一行一行地从引擎里面读出来，然后累积计数。
如果说要大幅度提升InnoDB表的count效率，主要的优化思路：自己计数(可以借助于redis这样的数据库进行,但是如果是带条件的count又比较麻烦了)。

#### count用法
count() 是一个聚合函数，对于返回的结果集，一行行地判断，如果 count 函数的参数不是NULL，累计值就加 1，否则不加，最后返回累计值。
用法：`count（*）`、count（主键）、count（字段）、count（数字）

| count用法 | 含义 |
| :--: | :--: |
| count(主键) | InnoDB引擎会遍历整张表，把每一行的主键id值都取出来，返回给服务层。服务层拿到主键后，直接按行进行累加(主键不可能为null) |
| count(字段) 没有not null约束 | InnoDB 引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，服务层判断是否为null，不为null，计数累加。 |
| count(字段) 有not null约束 | InnoDB 引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，直接按行进行累加。 |
| count(数字) | InnoDB 引擎遍历整张表，但不取值。服务层对于返回的每一行，放一个数字“1”，直接按行进行累加。 |
| `count(*)` | InnoDB引擎并不会把全部字段取出来，而是专门做了优化，不取值，服务层直接按行进行累加。 |
> 按照效率排序的话，count(字段) < count(主键 id) < count(1) ≈ `count(*)`，所以尽量使用 `count(*)`。

详细可以看[这章](obsidian://bookmaster?type=open-book&bid=KzLlZbYyXOsmpVOe)


### update优化

我们主要需要注意一下update语句执行时的注意事项。
```sql
update course set name = 'javaEE' where id = 1 ; 
```
当我们在执行删除的SQL语句时，会锁定id为1这一行的数据，然后事务提交之后，行锁释放。
但是当我们在执行如下SQL时。
```sql
update course set name = 'SpringBoot' where name = 'PHP' ; 
```
当我们开启多个事务，在执行上述的SQL时，我们发现行锁升级为了表锁。 导致该update语句的性能大大降低。

> InnoDB的行锁是针对索引加的锁，不是针对记录加的锁 ,并且该索引不能失效，否则会从行锁升级为表锁。

## 视图
**定义**：
	视图（View）是一种**虚拟存在**的表。视图中的数据并不在数据库中**实际存在**，行和列数据来自定义视图的查询中使用的表，并且是在使用视图时**动态生成的**(在视图被访问时根据其定义的SQL查询语句实时计算的，而不是在视图创建时预先计算和存储)。通俗的讲，视图只保存了查询的SQL逻辑，不保存查询结果。所以我们在创建视图的时候，主要的工作就落在创建这条SQL查询语句上。
### 语法
1. 创建
	```sql
	create [or replace] view 视图名称[(列名列表)] as select [[with CASCADED | Local ] check option]
	```
2. 查询
	```sql
	查看创建视图语句: show create view 视图名称;
	查看视图数据: select *from 视图名称 ......;
	```
3. 修改
	```sql
	方式一：create [or replace] view 视图名称[(列名列表)] as select [[with CASCADED | Local ] check option]
	方式二：alter [or replace] view 视图名称[(列名列表)] as select [[with CASCADED | Local ] check option]
	```
4. 删除
	```sql
	drop view [if exists ] 视图名称 [,视图名称] ... 
	```

**演示示例**：
```sql
-- 创建视图
create or replace view stu_v_1 as select id,name from student where id <= 10;
-- 查询视图
show create view stu_v_1;
select * from stu_v_1;
select * from stu_v_1 where id < 3;
-- 修改视图
create or replace view stu_v_1 as select id,name,no from student where id <= 10;
alter view stu_v_1 as select id,name from student where id <= 10;
-- 删除视图
drop view if exists stu_v_1;
```

上述我们演示了，视图应该如何创建、查询、修改、删除，那么我们能不能通过视图来插入、更新数据呢？ 接下来，做一个测试。

```sql
create or replace view stu_v_1 as select id,name from student where id <= 10 ;
select * from stu_v_1;
insert into stu_v_1 values(6,'Tom');
insert into stu_v_1 values(17,'Tom22');
```

执行上述的SQL，我们会发现，id为6和17的数据都是可以成功插入的。 但是我们执行查询，查询出来的数据，却没有id为17的记录。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240207235016.png" alt="image.png" style="zoom:90%;" />

- 因为我们在创建视图的时候，指定的条件为 id<=10, id为17的数据，是不符合条件的，所以没有查询出来，但是这条数据**确实是已经成功的插入到了基表中**。
- 如果我们定义视图时，如果指定了条件，然后我们在插入、修改、删除数据时，是否可以做到必须满足条件才能操作，否则不能够操作呢？ 答案是可以的，这就需要借助于视图的检查选项了。

### 检查选项

当使用`WITH CHECK OPTION`子句创建视图时，MySQL会**通过视图检查正在更改的每个行**，例如插入，更新，删除，以使其符合视图的定义。 MySQL允许基于另一个视图创建视图，它还会检查依赖视图中的规则以保持一致性。为了确定检查的范围，mysql提供了两个选项： CASCADED和LOCAL，默认值为CASCADED。

1. CASCADED
**级联**。
比如，v2视图是基于v1视图的，如果在v2视图创建的时候指定了检查选项为 cascaded，但是v1视图创建时未指定检查选项。 则在执行检查时，不仅会检查v2，还会级联检查v2的关联视图v1(如图上v1好像页有后面的`with cascaded check option`)。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240207235803.png?" alt="image.png" style="zoom:60%;" />
2. LOCAL
本地。
比如，v2视图是基于v1视图的，如果在v2视图创建的时候指定了检查选项为 local ，但是v1视图创建时未指定检查选项。 则在执行检查时，只会检查v2，不会检查v2的关联视图v1。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240207235949.png?" alt="image.png" style="zoom:60%;" />
### 视图的更新

要使视图可更新，视图中的行与基础表中的行之间必须存在一对一的关系。如果视图包含以下任何一
项，则该视图不可更新：
A. 聚合函数或窗口函数（SUM()、 MIN()、 MAX()、 COUNT()等）
B. DISTINCT
C. GROUP BY
D. HAVING
E. UNION 或者 UNION ALL

示例演示:
```sql
create view stu_v_count as select count(*) from student;
```

上述的视图中，就只有一个单行单列的数据，如果我们对这个视图进行更新或插入的，将会报错。
```sql
insert into stu_v_count values(10); 
```

`[HYO00](1471] The target table stu_y_count of the INSERT is not insertable-into`

### 视图的作用
1. **简单**
	视图不仅可以简化用户对数据的理解，也可以简化他们的操作。那些被经常使用的查询可以被定义为视图，从而使得用户不必为以后的操作每次指定全部的条件。
2. **安全**
	数据库可以授权，但不能授权到数据库特定行和特定的列上。通过视图用户只能查询和修改他们所能见到的数据
3. **数据独立**
	视图可帮助用户屏蔽真实表结构变化带来的影响。

### 案例
1. 为了保证数据库表的安全性，开发人员在操作tb_user表时，只能看到的用户的基本字段，屏蔽手机号和邮箱两个字段。
```sql
create view tb_user_view as select id,name,profession,age,gender,status,createtime from tb_user;
select * from tb_user_view;
```
2. 查询每个学生所选修的课程（三张表联查），这个功能在很多的业务中都有使用到，为了简化操作，定义一个视图。
```sql
create view tb_stu_course_view as select s.name student_name, s.no student_no,
c.name course_name from student s, student_course sc , course c where s.id =
sc.studentid and sc.courseid = c.id;

select * from tb_stu_course_view;
```

## 存储过程

存储过程是**事先经过编译并存储在数据库中的一段SQL语句的集合**，调用存储过程可以简化应用开发人员的很多工作，减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的。

存储过程思想上很简单，就是数据库 SQL 语言层面的代码封装与重用。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240208000936.png" alt="image.png" style="zoom:60%;" />


特点:
- 封装，复用 -----------------------> 可以把某一业务SQL封装在存储过程中，需要用到的时候直接调用即可。
- 可以接收参数，也可以返回数据 --------> 再存储过程中，可以传递参数，也可以接收返回值。
- 减少网络交互，效率提升 -------------> 如果涉及到多条SQL，每执行一次都是一次网络传输。 而如果封装在存储过程中，我们只需要网络交互一次可能就可以了。

### 基本语法

1. 创建
```sql
create procedure 存储过程名称 ([ 参数列表 ])
begin 
	-- SQL语句
END;
```

2. 调用

```sql
call 名称 ([参数])；
```

3. 查看
```sql
SELECT * FROM INFORMATION_SCHEMA.ROUTINES WHERE ROUTINE_SCHEMA = 'xxx'; -- 查询指定数据库的存储过程及状态信息
SHOW CREATE PROCEDURE 存储过程名称 ; -- 查询某个存储过程的定义
```

4. 删除
```sql
DROP PROCEDURE [ IF EXISTS ] 存储过程名称 ； 
```

**注意**:
	在命令行中，执行创建存储过程的SQL时，需要通过关键字 delimiter 指定SQL语句的结束符。

演示示例:

```sql
-- 存储过程基本语法
-- 创建
create procedure p1()
begin
select count(*) from student;
end;
-- 调用
call p1();
-- 查看
select * from information_schema.ROUTINES where ROUTINE_SCHEMA = 'itcast';
show create procedure p1;
-- 删除
drop procedure if exists p1;
```

### 变量

在MySQL中变量分为三种类型: **系统变量、用户定义变量、局部变量**。
#### 系统变量
系统变量 是MySQL服务器提供，不是用户定义的，属于服务器层面。分为**全局变量**（GLOBAL）、**会话变量**（SESSION）。
1. 查看系统变量

```sql
SHOW [ SESSION | GLOBAL ] VARIABLES ; -- 查看所有系统变量
SHOW [ SESSION | GLOBAL ] VARIABLES LIKE '......'; -- 可以通过LIKE模糊匹配方
式查找变量
SELECT @@[SESSION | GLOBAL] 系统变量名; 
```

2. 设置系统变量
```sql
SET [ SESSION | GLOBAL ] 系统变量名 = 值 ;
SET @@[SESSION | GLOBAL]系统变量名 = 值 ;
```


> [!caution] 注意:
> 如果没有指定SESSION/GLOBAL，默认是SESSION，会话变量。    
> - mysql服务重新启动之后，所设置的全局参数会失效，要想不失效，可以在 /etc/my.cnf 中配置。        
> A. 全局变量(GLOBAL): 全局变量针对于所有的会话。                 
> B. 会话变量(SESSION): 会话变量针对于单个会话，在另外一个会话窗口就不生效了。             

演示示例:
```sql
-- 查看系统变量
show session variables ;
show session variables like 'auto%';
show global variables like 'auto%';
select @@global.autocommit;
select @@session.autocommit;
-- 设置系统变量
set session autocommit = 1;
insert into course(id, name) VALUES (6, 'ES');
set global autocommit = 0;
select @@global.autocommit;
```

#### 用户定义变量
用户定义变量 是用户根据需要自己定义的变量，用户变量不用提前声明，在用的时候直接用 "**@变量名**" 使用就可以。其作用域为当前连接。
1. 赋值
方式一:
```sql
SET @var_name = expr [, @var_name = expr] ... ;
SET @var_name := expr [, @var_name := expr] ... ;
```

赋值时，可以使用 = ，也可以使用 := 。

方式二:
```sql
SELECT @var_name := expr [, @var_name := expr] ... ;
SELECT 字段名 INTO @var_name FROM 表名;
```

2. 使用
```sql
SELECT @var_name ; 
```

> 注意: 用户定义的变量无需对其进行声明或初始化，只不过获取到的值为NULL。

演示示例:

```sql
-- 赋值
set @myname = 'itcast';
set @myage := 10;
set @mygender := '男',@myhobby := 'java';
select @mycolor := 'red';
select count(*) into @mycount from tb_user;
-- 使用
select @myname,@myage,@mygender,@myhobby;
select @mycolor , @mycount;
select @abc;
```

#### 局部变量
局部变量 是根据需要定义的在局部生效的变量，访问之前，需要`DECLARE`声明。可用作存储过程内的局部变量和**输入参数**，局部变量的范围是在其内声明的BEGIN ... END块。
1. 声明
	```sql
	DECLARE 变量名 变量类型 [DEFAULT ... ] ;
	```

变量类型就是数据库字段类型：INT、BIGINT、CHAR、VARCHAR、DATE、TIME等。

2. 赋值
```sql
SET 变量名 = 值 ;
SET 变量名 := 值 ;
SELECT 字段名 INTO 变量名 FROM 表名 ... ;
```

演示示例:
```sql
-- 声明局部变量 - declare
-- 赋值
create procedure p2()
begin
declare stu_count int default 0;
select count(*) into stu_count from student;
select stu_count;
end;
call p2();
```

### if
1. 介绍

```sql                                
IF 条件1 THEN
.....
ELSEIF 条件2 THEN -- 可选
.....
ELSE -- 可选
.....
END IF;
```
在if条件判断的结构中，ELSE IF 结构可以有多个，也可以没有。 ELSE结构可以有，也可以没有。

2. 案例
根据定义的分数score变量，判定当前分数对应的分数等级。
- score >= 85分，等级为优秀。
- score >= 60分 且 score < 85分，等级为及格。
- score < 60分，等级为不及格。

```sql
create procedure p3()
begin
	declare score int default 58;
	declare result varchar(10);
	if score >= 85 then
		set result := '优秀';
	elseif score >= 60 then
		set result := '及格';
	else
		set result := '不及格';
	end if;
	select result;
end;
call p3();
```

上述的需求我们虽然已经实现了，但是也存在一些问题，比如：score 分数我们是在存储过程中定义死的，而且最终计算出来的分数等级，我们也仅仅是最终查询展示出来而已。那么我们能不能，把score分数动态的传递进来，计算出来的分数等级是否可以作为返回值返回呢？答案是肯定的，我们可以通过接下来所讲解的参数来解决上述的问题。

### 参数
1. 介绍
参数的类型，主要分为以下三种：IN、OUT、INOUT。 具体的含义如下：

|  类型   |           含义           | 备注  |
| :---: | :--------------------: | :-: |
|  IN   |  该类参数作为输入，也就是需要调用时传入值  | 默认  |
|  OUT  | 该类参数作为输出，也就是该参数可以作为返回值 |  无  |
| INOUT |  既可以作为输入参数，也可以作为输出参数   |  无  |
用法：
```sql
CREATE PROCEDURE 存储过程名称 ([ IN/OUT/INOUT 参数名 参数类型 ])
BEGIN
-- SQL语句
END ;
```

2. 案例一
根据传入参数score，判定当前分数对应的分数等级，并返回。
- score >= 85分，等级为优秀。
- score >= 60分 且 score < 85分，等级为及格。
- score < 60分，等级为不及格。

```sql
create procedure p4(in score int, out result varchar(10))
begin
	if score >= 85 then
		set result := '优秀';
	elseif score >= 60 then
		set result := '及格';
	else
		set result := '不及格';
	end if;
end;

-- 定义用户变量 @result来接收返回的数据, 用户变量可以不用声明
call p4(18, @result);

select @result;
```

3. 案例二
将传入的200分制的分数，进行换算，换算成百分制，然后返回。
```sql
create procedure p5(inout score double)
begin
	set score := score * 0.5;
end;

set @score = 198;
call p5(@score);

select @score;
```

case
1. 介绍
case结构及作用，和我们在基础篇中所讲解的流程控制函数很类似。有两种语法格式：
语法1：
```sql
-- 含义： 当case_value的值为 when_value1时，执行statement_list1，当值为 when_value2时，
执行statement_list2， 否则就执行 statement_list
CASE case_value
	WHEN when_value1 THEN statement_list1
	[ WHEN when_value2 THEN statement_list2] ...
	[ ELSE statement_list ]
END CASE;
```

语法2：
```sql
-- 含义： 当条件search_condition1成立时，执行statement_list1，当条件search_condition2成
立时，执行statement_list2， 否则就执行 statement_list
CASE
	WHEN search_condition1 THEN statement_list1
	[WHEN search_condition2 THEN statement_list2] ...
	[ELSE statement_list]
END CASE;
```

2. 案例
根据传入的月份，判定月份所属的季节（要求采用case结构）。
- 1-3月份，为第一季度
- 4-6月份，为第二季度
- 7-9月份，为第三季度
- 10-12月份，为第四季度

```sql
create procedure p6(in month int)
begin
	declare result varchar(10);
	case
		when month >= 1 and month <= 3 then
			set result := '第一季度';
		when month >= 4 and month <= 6 then
			set result := '第二季度';
		when month >= 7 and month <= 9 then
			set result := '第三季度';
		when month >= 10 and month <= 12 then
			set result := '第四季度';
		else
			set result := '非法参数';
		end case;
		
		select concat('您输入的月份为: ',month, ', 所属的季度为: ',result);
end;
call p6(16);
```

> 注意：如果判定条件有多个，多个条件之间，可以使用 and 或 or 进行连接。

### while
1. 介绍
while 循环是有条件的循环控制语句。满足条件后，再执行循环体中的SQL语句。具体语法为：
```sql
-- 先判定条件，如果条件为true，则执行逻辑，否则，不执行逻辑
WHILE 条件 DO
SQL逻辑...
END WHILE;
```

2. 案例
计算从1累加到n的值，n为传入的参数值。
```sql
-- A. 定义局部变量, 记录累加之后的值;
-- B. 每循环一次, 就会对n进行减1 , 如果n减到0, 则退出循环
create procedure p7(in n int)
begin
	declare total int default 0;
	while n>0 do
		set total := total + n;
		set n := n - 1;
	end while;
	select total;
end;

call p7(100);
```

### repeat
1. 介绍
repeat是有条件的循环控制语句(<font color=#ff0000>先执行一次逻辑</font>), 当满足until声明的条件的时候，则退出循环 。具体语法为：
```sql
-- 先执行一次逻辑，然后判定UNTIL条件是否满足，如果满足，则退出。如果不满足，则继续下一次循环
REPEAT
	SQL逻辑...
	UNTIL 条件
END REPEAT;
```

2. 案例
计算从1累加到n的值，n为传入的参数值。(使用repeat实现)
```sql
-- A. 定义局部变量, 记录累加之后的值;
-- B. 每循环一次, 就会对n进行-1 , 如果n减到0, 则退出循环
create procedure p8(in n int)
begin
	declare total int default 0;
	repeat
		set total := total + n;
		set n := n - 1;
	until n <= 0
	end repeat;
	
	select total;
end;
call p8(10);
call p8(100);
```

### loop
1. 介绍
LOOP 实现**简单的循环**，如果不在SQL逻辑中增加退出循环的条件，可以用其来实现简单的死循环。
LOOP可以配合一下两个语句使用：
LEAVE ：配合循环使用，退出循环。
ITERATE：必须用在循环中，作用是跳过当前循环剩下的语句，直接进入下一次循环。

```sql
[begin_label:] LOOP
	SQL逻辑...
END LOOP [end_label];
```

```sql
LEAVE label; -- 退出指定标记的循环体
ITERATE label; -- 直接进入下一次循环
```

上述语法中出现的 `begin_label，end_label，label` 指的都是我们所自定义的标记。

2. 案例一
计算从1累加到n的值，n为传入的参数值。
```sql
-- A. 定义局部变量, 记录累加之后的值;
-- B. 每循环一次, 就会对n进行-1 , 如果n减到0, 则退出循环 ----> leave xx
create procedure p9(in n int)
begin
	declare total int default 0;
	
	sum:loop
		if n<=0 then
			leave sum;
		end if;
		set total := total + n;
		set n := n - 1;
	end loop sum;
	
	select total;
end;

call p9(100);		
```

3. 案例二
计算从1到n之间的偶数累加的值，n为传入的参数值。
```sql
-- A. 定义局部变量, 记录累加之后的值;
-- B. 每循环一次, 就会对n进行-1 , 如果n减到0, 则退出循环 ----> leave xx
-- C. 如果当次累加的数据是奇数, 则直接进入下一次循环. --------> iterate xx
create procedure p10(in n int)
begin
	declare total int default 0;
	sum:loop
		if n<=0 then
			leave sum;
		end if;
		if n%2 = 1 then
			set n := n - 1;
			iterate sum;
		end if;
		
		set total := total + n;
		set n := n - 1;
	end loop sum;
	select total;
end;

call p10(100);
```

### 游标
1. 介绍
游标（CURSOR）是用来**存储查询结果集**的数据类型 , 在存储过程和函数中可以使用游标对结果集进行**循环的处理**。游标的使用包括游标的声明、OPEN、FETCH 和 CLOSE，其语法分别如下。
A. 声明游标
```sql
DECLARE 游标名称 CURSOR FOR 查询语句 ;
```
B. 打开游标
```sql
OPEN 游标名称 ; 
```
C. 获取游标记录
```sql
FETCH 游标名称 INTO 变量 [, 变量 ] ;
```
D. 关闭游标
```sql
CLOSE 游标名称 ; 
```
2. 案例
根据传入的参数uage，来查询用户表tb_user中，所有的用户年龄小于等于uage的用户姓名（name）和专业（profession），并将用户的姓名和专业插入到所创建的一张新表(id,name,profession)中。
```sql
-- 逻辑:
-- A. 声明游标, 存储查询结果集
-- B. 准备: 创建表结构
-- C. 开启游标
-- D. 获取游标中的记录
-- E. 插入数据到新表中
-- F. 关闭游标
create procedure p11(in uage int)
begin
	declare uname varchar(100);
	declare upro varchar(100);
	declare u_cursor cursor for select name,profession from tb_user where age <=uage;
	drop table if exists tb_user_pro;
	create table if not exists tb_user_pro(
		id int primary key auto_increment,
		name varchar(100),
		profession varchar(100)
	);
	open u_cursor;
	while true do
		fetch u_cursor into uname,upro;
		insert into tb_user_pro values (null, uname, upro);
	end while;
	close u_cursor;
end;
call p11(30);
```

上述的存储过程，最终我们在调用的过程中，会报错，之所以报错是因为上面的while循环中，并没有退出条件。当游标的数据集获取完毕之后，再次获取数据，就会报错，从而终止了程序的执行。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240215223254.png?" alt="image.png" style="zoom:60%;" />
但是此时，tb_user_pro表结构及其数据都已经插入成功了，我们可以直接刷新表结构，检查表结构中的数据。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240215223330.png" alt="image.png" style="zoom:60%;" />
上述的功能，虽然我们实现了，但是逻辑并不完善，而且程序执行完毕，获取不到数据，数据库还报错。 接下来，我们就需要来完成这个存储过程，并且解决这个问题。
要想解决这个问题，就需要通过MySQL中提供的 条件处理程序 Handler 来解决。

### 条件处理程序
1). 介绍
条件处理程序（Handler）可以用来定义在流程控制结构执行过程中遇到问题时相应的处理步骤。具体语法为：
```sql
DECLARE handler_action HANDLER FOR condition_value [, condition_value] ... statement ;
handler_action 的取值：
	CONTINUE: 继续执行当前程序
	EXIT: 终止执行当前程序
condition_value 的取值：
	SQLSTATE sqlstate_value: 状态码，如 02000
	SQLWARNING: 所有以01开头的SQLSTATE代码的简写
	NOT FOUND: 所有以02开头的SQLSTATE代码的简写
	SQLEXCEPTION: 所有没有被SQLWARNING 或 NOT FOUND捕获的SQLSTATE代码的简写
```

> SQLSTATE是由五个字符组成的字符串，其中前两个字符表示错误类，后三个字符表示错误子类。例如，'02000'表示没有数据（即查询未返回结果），而'23000'表示完整性约束违反。


1. 案例
我们继续来完成在上一小节提出的这个需求，并解决其中的问题。
根据传入的参数uage，来查询用户表tb_user中，所有的用户年龄小于等于uage的用户姓名（name）和专业（profession），并将用户的姓名和专业插入到所创建的一张新表(id,name,profession)中。
A. 通过SQLSTATE指定具体的状态码
```sql
-- 逻辑:
-- A. 声明游标, 存储查询结果集
-- B. 准备: 创建表结构
-- C. 开启游标
-- D. 获取游标中的记录
-- E. 插入数据到新表中
-- F. 关闭游标
create procedure p11(in uage int)
begin
	declare uname varchar(100);
	declare upro varchar(100);
	declare u_cursor cursor for select name,profession from tb_user where age <=uage;
	-- 声明条件处理程序 ： 当SQL语句执行抛出的状态码为02000时，将关闭游标u_cursor，并退出
	declare exit handler for SQLSTATE '02000' close u_cursor;
	drop table if exists tb_user_pro;
	create table if not exists tb_user_pro(
		id int primary key auto_increment,
		name varchar(100),
		profession varchar(100)
	);
	open u_cursor;
	while true do
		fetch u_cursor into uname,upro;
		insert into tb_user_pro values (null, uname, upro);
	end while;
	close u_cursor;
end;
call p11(30);
```

B. 通过SQLSTATE的代码简写方式 NOT FOUND
02 开头的状态码，代码简写为 NOT FOUND
```sql
create procedure p12(in uage int)
begin
	declare uname varchar(100);
	declare upro varchar(100);
	declare u_cursor cursor for select name,profession from tb_user where age <=uage;
	-- 声明条件处理程序 ： 当SQL语句执行抛出的状态码为02开头时，将关闭游标u_cursor，并退出
	declare exit handler for not found close u_cursor;
	drop table if exists tb_user_pro;
	create table if not exists tb_user_pro(
		id int primary key auto_increment,
		name varchar(100),
		profession varchar(100)
	);
	open u_cursor;
	while true do
		fetch u_cursor into uname,upro;
		insert into tb_user_pro values (null, uname, upro);
	end while;
	close u_cursor;
end;
call p12(30);
```

存储函数
1). 介绍
存储函数是有返回值的存储过程，存储函数的参数只能是IN类型的。具体语法如下：
```sql
CREATE FUNCTION 存储函数名称 ([ 参数列表 ])
RETURNS type [characteristic ...]
BEGIN
	-- SQL语句
	RETURN ...;
END ;
```

characteristic说明：
- `DETERMINISTIC`：相同的输入参数总是产生相同的结果
- `NO SQL`：不包含 SQL 语句。
- `READS SQL DATA`：包含读取数据的语句，但不包含写入数据的语句。

2. 案例
计算从1累加到n的值，n为传入的参数值。 
```sql
create function fun1(n int)
returns int deterministic
begin
	declare total int default 0;
	while n>0 do
		set total := total + n;
		set n := n - 1;
	end while;
	return total;
end;
select fun1(50);
```

在mysql8.0版本中binlog默认是开启的，一旦开启了，mysql就要求在定义存储过程时，需要指定characteristic特性，否则就会报如下错误：
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240215225230.png?" alt="image.png" style="zoom:60%;" />
> 可以指定一些特性（characteristics）来影响存储过程的行为。这些特性提供了额外的信息，比如存储过程如何执行、它是否应该被写入二进制日志中，以及调用存储过程时的安全要求等

## 触发器
### 介绍
触发器是与表有关的**数据库对象**，指在`insert/update/delete`之前(BEFORE)或之后(AFTER)，触发并执行触发器中定义的SQL语句集合。触发器的这种特性可以协助应用在数据库端确保数据的完整性, 日志记录 , 数据校验等操作 。
使用别名OLD和NEW来**引用触发器**中发生变化的记录内容，这与其他的数据库是相似的。现在触发器还只支持行级触发，不支持语句级触发。 

| 触发器类型     | NEW 和 OLD     |
|:----:|:----:|
| INSERT 型触发器     | NEW 表示将要或者已经新增的数据     |
| UPDATE 型触发器     | OLD 表示修改之前的数据 , NEW 表示将要或已经修改后的数据     |
| DELETE 型触发器      | OLD 表示将要或者已经删除的数据     |

### 语法
1. 创建

```sql
CREATE TRIGGER trigger_name
BEFORE/AFTER INSERT/UPDATE/DELETE 
ON tbl_name FOR EACH ROW -- 行级触发器
BEGIN
	trigger_stmt;
END;
```

2. 查看
```sql
SHOW TRIGGERS ; 
```
3. 删除
```sql
DROP TRIGGER [schema_name.]trigger_name ; -- 如果没有指定 schema_name，默认为当前数据库 。
```

### 案例
通过触发器记录 tb_user 表的数据变更日志，将变更日志插入到日志表user_logs中, 包含增加,修改 , 删除;

测试:
```sql
-- 查看
show triggers ;
-- 插入数据到tb_user
insert into tb_user(id, name, phone, email, profession, age, gender, status,createtime) VALUES (26,'三皇子','18809091212','erhuangzi@163.com','软件工程',23,'1','1',now());
```

测试完毕之后，检查日志表中的数据是否可以正常插入，以及插入数据的正确性。
B. 修改数据触发器

```sql
create trigger tb_user_update_trigger
	after update on tb_user for each row
begin
	insert into user_logs(id, operation, operate_time, operate_id, operate_params)
VALUES
	(null, 'update', now(), new.id,
		concat('更新之前的数据: id=',old.id,',name=',old.name, ', phone=',
	old.phone, ', email=', old.email, ', profession=', old.profession,
		' | 更新之后的数据: id=',new.id,',name=',new.name, ', phone=',
	NEW.phone, ', email=', NEW.email, ', profession=', NEW.profession));
end;
```

测试:
```sql
-- 查看
show triggers ;
-- 更新
update tb_user set profession = '会计' where id = 23;
update tb_user set profession = '会计' where id <= 5;
```

测试完毕之后，检查日志表中的数据是否可以正常插入，以及插入数据的正确性。 

C. 删除数据触发器
```sql
create trigger tb_user_delete_trigger
after delete on tb_user for each row
begin
	insert into user_logs(id, operation, operate_time, operate_id, operate_params)
	VALUES (null, 'delete', now(), old.id,
	concat('删除之前的数据: id=',old.id,',name=',old.name, ', phone=',
	old.phone, ', email=', old.email, ', profession=', old.profession));
end;
```

测试:
```sql
-- 查看
show triggers ;
-- 删除数据
delete from tb_user where id = 26;
```

测试完毕之后，检查日志表中的数据是否可以正常插入，以及插入数据的正确性。 

## 锁
### 概述

锁是计算机协调多个进程或线程并发访问某一资源的机制。在数据库中，除传统的计算资源（CPU、RAM、I/O）的争用以外，数据也是一种供许多用户共享的资源。如何保证数据并发访问的一致性、有效性是所有数据库必须解决的一个问题，锁冲突也是影响数据库并发访问性能的一个重要因素。从这个角度来说，锁对数据库而言显得尤其重要，也更加复杂。
MySQL中的锁，按照锁的粒度分，分为以下三类：
- 全局锁：锁定数据库中的所有表。
- 表级锁：每次操作锁住整张表。
- 行级锁：每次操作锁住对应的行数据。

### 全局锁
#### 介绍
全局锁就是对**整个数据库实例**加锁，加锁后整个实例就处于只读状态，后续的DML的写语句，DDL语句，已经更新操作的事务提交语句都将被阻塞。
其典型的使用场景是做**全库的逻辑备份**，对所有的表进行锁定，从而获取**一致性**视图，保证数据的完整性。
为什么全库逻辑备份，就需要加全就锁呢？

A. 我们一起先来分析一下不加全局锁，可能存在的问题。
假设在数据库中存在这样三张表: tb_stock 库存表，tb_order 订单表，tb_orderlog 订单日志表。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240216203101.png?" alt="image.png" style="zoom:60%;" />
- 在进行数据备份时，先备份了tb_stock库存表。
- 然后接下来，在业务系统中，执行了下单操作，扣减库存，生成订单（更新`tb_stock`表，插入`tb_order`表）。
- 然后再执行备份 tb_order表的逻辑。
- 业务中执行插入订单日志操作。
- 最后，又备份了tb_orderlog表。

此时备份出来的数据，是存在问题的。因为备份出来的数据，tb_stock表与tb_order表的数据不一致(有最新操作的订单信息,但是库存数没减)。
那如何来规避这种问题呢? 此时就可以借助于MySQL的全局锁来解决。

B. 再来分析一下加了全局锁后的情况
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240216203351.png?" alt="image.png" style="zoom:60%;" />
对数据库进行进行逻辑备份之前，先对整个数据库加上全局锁，一旦加了全局锁之后，其他的DDL、DML全部都处于阻塞状态，但是可以执行DQL语句，也就是处于只读状态，而数据备份就是查询操作。那么数据在进行逻辑备份的过程中，数据库中的数据就是不会发生变化的，这样就保证了数据的一致性
和完整性。

#### 语法
1. 加全局锁
	```sql
	flush tables with read lock;
	```
2. 数据备份

	```sql
	mysqldump -uroot -p701121 练习>/path/mysql/练习.sql
	```

3. 释放锁

	```sql
	unlock tables;
	```

#### 特点
数据库中加全局锁，是一个比较重的操作，存在以下问题：
- 如果在主库上备份，那么在备份期间都不能执行更新，业务基本上就得停摆。
- 如果在从库上备份，那么在备份期间从库不能执行**主库同步**过来的二进制日志（binlog），会导致主从延迟。

在InnoDB引擎中，我们可以在备份时加上参数 `--single-transaction` 参数来完成不加锁的一致性数据备份(基于[[零散知识点#什么是快照？ 快照与备份有什么区别？|快照]]原理)。 
```sql
mysqldump --single-transaction -uroot –p123456 itcast > itcast.sql 
```

### 表级锁
#### 介绍
表级锁，每次操作锁住整张表。锁定粒度大，发生锁冲突的概率最高，并发度最低。应用在MyISAM、InnoDB、BDB等存储引擎中。
对于表级锁，主要分为以下三类：
- 表锁
- 元数据锁（meta data lock，MDL）
- 意向锁

#### 表锁
对于表锁，分为两类：
- 表共享读锁（read lock）
- 表独占写锁（write lock）

语法：
- 加锁：lock tables 表名... read/write。
- 释放锁：unlock tables / 客户端断开连接。

特点:
A. 读锁 

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240216204158.png" alt="image.png" style="zoom:60%;" />
左侧为客户端一，对指定表加了读锁，不会影响右侧客户端二的读，但是会阻塞右侧客户端的写。
测试:

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240216204239.png?" alt="image.png" style="zoom:60%;" />
B. 写锁
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240216204305.png?" alt="image.png" style="zoom:60%;" />
左侧为客户端一，对指定表加了写锁，会阻塞右侧客户端的读和写。
测试:

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240216204333.png?" alt="image.png" style="zoom:60%;" />

> **结论**: 读锁不会阻塞其他客户端的读，但是会阻塞所有客户端写。写锁既会阻塞<font color=#ff0000>其他客户端</font>的读，又会阻塞其他客户端的写。

#### 元数据锁

meta data lock , 元数据锁，简写MDL。
MDL加锁过程是**系统自动控制**，无需显式使用，在访问一张表的时候会自动加上。MDL锁主要作用是维护**表元数据的数据一致性**，在表上有活动事务的时候，不可以对元数据进行写入操作。为 了避免DML与DDL冲突，保证读写的正确性。
这里的元数据，大家可以简单理解为就是一张表的**表结构**。 也就是说，某一张表涉及到**未提交的事务**时，是**不能够修改这张表的表结构**的。
在MySQL5.5中引入了MDL，当对一张表进行增删改查的时候，加MDL读锁(共享)；当对表结构进行变更操作的时候，加MDL写锁(排他)。
常见的SQL操作时，所添加的元数据锁：

| 对应SQL  | 锁类型  | 说明 |
| :--: | :--: | :--: |
| lock tables xxx read / write | SHARED_READ_ONLY / SHARED_NO_READ_WRITE |  |
| select 、select ... `lock in share mode` | SHARED_READ | 与SHARED_READ、SHARED_WRITE**兼容**，与EXCLUSIVE互斥 |
| insert 、update、delete、select ... for update | SHARED_WRITE | 与SHARED_READ、SHARED_WRITE兼容，与<br>EXCLUSIVE互斥 |
| alter table ...  | EXCLUSIVE | 与其他的MDL都互斥 |

> `LOCK IN SHARE MODE`是MySQL中的一个锁定读取选项，用于在执行查询时对选中的行施加共享锁。

演示：
当执行SELECT、INSERT、UPDATE、DELETE等语句时，添加的是元数据共享锁（SHARED_READ / SHARED_WRITE），之间是兼容的。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240216210149.png?" alt="image.png" style="zoom:60%;" />
当执行SELECT语句时，添加的是元数据共享锁（SHARED_READ），会阻塞元数据排他锁（EXCLUSIVE），之间是互斥的。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240216210217.png?" alt="image.png" style="zoom:60%;" />
我们可以通过下面的SQL，来查看数据库中的元数据锁的情况：
```sql
select object_type,object_schema,object_name,lock_type,lock_duration from performance_schema.metadata_locks ;
```

我们在操作过程中，可以通过上述的SQL语句，来查看元数据锁的加锁情况。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240216210318.png?" alt="image.png" style="zoom:60%;" />

> 一旦有会话被阻塞等待 MDL 写锁，任何之后尝试对同一表申请新的 MDL 读锁的操作也会被阻塞。这是因为 MySQL 的 MDL 系统采用的是一种防止死锁的策略，即不允许在已有等待写锁的情况下再授予新的读锁。
#### 意向锁
1. 介绍
为了避免DML在执行时，加的行锁与表锁的冲突，在InnoDB中引入了意向锁，使得表锁不用检查每行数据是否加锁，使用意向锁来减少表锁的检查。假如没有意向锁，客户端一对表加了行锁后，客户端二如何给表加表锁呢，来通过示意图简单分析一下：
首先客户端一，开启一个事务，然后执行DML操作，在执行DML语句时，会对涉及到的行加行锁。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240216210437.png?" alt="image.png" style="zoom:60%;" />

当客户端二，想对这张表加表锁时，会检查当前表是否有对应的行锁，如果没有，则添加表锁，此时就会从第一行数据，检查到最后一行数据，效率较低。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240216210526.png?" alt="image.png" style="zoom:60%;" />

有了意向锁之后 :
客户端一，在执行DML操作时，会对涉及的行加行锁，同时也会对该表加上意向锁。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240216210556.png?" alt="image.png" style="zoom:60%;" />
而其他客户端，在对这张表加表锁的时候，会根据该表上所加的意向锁来判定是否可以成功加表锁，而不用逐行判断行锁情况了。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240216210627.png?" alt="image.png" style="zoom:60%;" />
2. 分类
- **意向共享锁(IS)**: 由语句select ... lock in share mode添加。 与表锁共享锁(read)兼容，与表锁排他锁(write)互斥。
- **意向排他锁(IX)**: 由insert、update、delete、select...for update添加。与表锁共享锁(read)及排他锁(write)都互斥，意向锁之间不会互斥。

> 一旦事务提交了，意向共享锁、意向排他锁，都会自动释放。

可以通过以下SQL，查看意向锁及行锁的加锁情况：

```sql
select object_schema,object_name,index_name,lock_type,lock_mode,lock_data from performance_schema.data_locks;
```

演示：
A. 意向共享锁与表读锁是兼容的
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240216231437.png" alt="image.png" style="zoom:60%;" />
B. 意向排他锁与表读锁、写锁都是互斥的

![image.png](http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217153808.png)


### 行级锁
#### 介绍

行级锁，每次操作锁住对应的行数据。锁定**粒度最小**，发生锁冲突的概率最低，**并发度最高**。应用在InnoDB存储引擎中。

InnoDB的数据是基于索引组织的，行锁是通过对索引上的**索引项**加锁来实现的，而不是对记录加的锁。对于行级锁，主要分为以下三类：

- 行锁（Record Lock）：锁定单个行记录的锁，防止其他事务对此行进行update和delete。在RC、RR隔离级别下都支持。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217155231.png?" alt="image.png" style="zoom:60%;" />

- 间隙锁（Gap Lock）：锁定索引记录间隙（不含该记录），确保索引记录间隙不变，防止其他事务在这个间隙进行insert，产生幻读。在RR隔离级别下都支持。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217155252.png?" alt="image.png" style="zoom:60%;" />

- 临键锁（Next-Key Lock）：行锁和间隙锁组合，同时锁住数据，并锁住数据前面的间隙Gap。在RR隔离级别下支持。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217155327.png?" alt="image.png" style="zoom:60%;" />
#### 行锁
1. 介绍
InnoDB实现了以下两种类型的行锁：
- 共享锁（S）：允许一个事务去读一行，阻止**其他事务**获得相同数据集的排它锁。
- 排他锁（X）：允许获取排他锁的事务更新数据，阻止**其他事务**获得相同数据集的共享锁和排他锁。

两种行锁的兼容情况如下:

| 请求锁类型 | S(共享锁) | X(排他锁) |
| ---- | ---- | ---- |
| S(共享锁) | 兼容 | 冲突 |
| X (排他锁) | 冲突 | 冲突 |

常见的SQL语句，在执行时，所加的行锁如下：

| SQL | 行锁类型 | 说明 |
| :--: | :--: | :--: |
| INSERT | 排他锁 | 自动加锁 |
| UPDATE | 排他锁 | 自动加锁 |
| DELETE | 排他锁 | 自动加锁 |
| SELECT（正常） | 不加任何锁 |  |
| SELECT ... LOCK IN SHARE MODE | 共享锁 | 需要手动在SELECT之后加LOCK IN SHARE MODE |
| SELECT ... FOR UPDATE | 排他锁 | 需要手动在SELECT之后加FOR UPDATE |

2. 演示

默认情况下，InnoDB在 REPEATABLE READ事务隔离级别运行，InnoDB使用 next-key 锁进行搜索和索引扫描，以防止幻读。
- 针对唯一索引进行检索时，对**已存在的记录进行等值匹配时**，将会自动优化为行锁。
- InnoDB的行锁是针对于索引加的锁，不通过**索引条件检索数据**，那么InnoDB将对表中的所有记录加锁，此时 就会升级为表锁。

A. 普通的select语句，执行时，不会加锁。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217171738.png?" alt="image.png" style="zoom:60%;" />


B. select...lock in share mode，加共享锁，共享锁与共享锁之间兼容

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217171751.png?" alt="image.png" style="zoom:60%;" />

共享锁与排他锁之间互斥。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217171828.png?" alt="image.png" style="zoom:60%;" />
客户端一获取的是id为1这行的共享锁，客户端二是可以获取id为3这行的排它锁的，因为不是同一行数据。 而如果客户端二想获取id为1这行的排他锁，会处于阻塞状态，以为共享锁与排他锁之间互斥。

C. 排它锁与排他锁之间互斥

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217171904.png?" alt="image.png" style="zoom:60%;" />
当客户端一，执行update语句，会为id为1的记录加排他锁； 客户端二，如果也执行update语句更新id为1的数据，也要为id为1的数据加排他锁，但是客户端二会处于阻塞状态，因为排他锁之间是互斥的。 直到客户端一，把事务提交了，才会把这一行的行锁释放，此时客户端二，解除阻塞。

D.无索引行锁升级为表锁
我们在两个客户端中执行如下操作:

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217211512.png?" alt="image.png" style="zoom:60%;" />

在客户端一中，开启事务，并执行update语句，更新name为Lily的数据，也就是id为19的记录 。
然后在客户端二中更新id为3的记录，却不能直接执行，会处于阻塞状态，为什么呢？

原因就是因为此时，客户端一，根据name字段进行更新时，name字段是没有索引的，如果没有索引，
此时行锁会升级为表锁(因为行锁是对索引项加的锁，而name没有索引)。

接下来，我们再针对name字段建立索引，索引建立之后，再次做一个测试：

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217211722.png?" alt="image.png" style="zoom:60%;" />
此时我们可以看到，客户端一，开启事务，然后依然是根据name进行更新。而客户端二，在更新id为3的数据时，更新成功，并未进入阻塞状态。 这样就说明，我们根据索引字段进行更新操作，就可以避免行锁升级为表锁的情况。

#### 间隙锁&临键锁
默认情况下，InnoDB在 REPEATABLE READ事务隔离级别运行，InnoDB使用 next-key 锁进行搜索和索引扫描，以防止幻读。
- 索引上的等值查询(唯一索引)，给**不存在的记录加锁**时, 优化为**间隙锁** 。
- 索引上的等值查询(非唯一普通索引)，**向右遍历时最后一个值不满足查询需求**时，next-keylock 退化为**间隙锁**。
- 索引上的范围查询(唯一索引)--会访问到不满足条件的第一个值为止。

> 注意：间隙锁唯一目的是防止其他事务插入间隙。间隙锁可以共存，一个事务采用的间隙锁不会阻止另一个事务在同一间隙上采用间隙锁。


示例演示
A. 索引上的等值查询(唯一索引)，给不存在的记录加锁时, 优化为间隙锁。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217213317.png?" alt="image.png" style="zoom:60%;" />

B. 索引上的等值查询(非唯一普通索引)，向右遍历时最后一个值不满足查询需求时，next-key lock 退化为间隙锁。
介绍分析一下：
我们知道InnoDB的B+树索引，叶子节点是有序的双向链表。 假如，我们要根据这个二级索引查询值为18的数据，并加上共享锁，我们是只锁定18这一行就可以了吗？ 并不是，因为是非唯一索引，这个结构中可能有多个18的存在，所以，在加锁时会继续往后找，找到一个不满足条件的值（当前案例中也就是29）。此时会对18加临键锁，并对29之前的间隙加锁。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217214423.png?" alt="image.png" style="zoom:60%;" />

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217214019.png?" alt="image.png" style="zoom:60%;" />
C. 索引上的范围查询(唯一索引)--会访问到不满足条件的第一个值为止。
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217214214.png?" alt="image.png" style="zoom:60%;" />

查询的条件为id>=19，并添加共享锁。 此时我们可以根据数据库表中现有的数据，将数据分为三个部分：
`[19] (19,25] (25,+∞]`

所以数据库数据在加锁是，就是将19加了行锁，25的临键锁（包含25及25之前的间隙），正无穷的临键锁(正无穷及之前的间隙)。

## InnoDB引擎
### 逻辑存储结构
InnoDB的逻辑存储结构如下图所示:
	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240115164846.png?" alt="image.png" style="zoom:70%;" />
1. 表空间
	表空间是InnoDB存储引擎逻辑结构的最高层， 如果用户启用了参数 `innodb_file_per_table`(在8.0版本中默认开启) ，则每张表都会有一个表空间（xxx.ibd），一个**mysql实例**可以对应多个表空间，用于存储记录、索引等数据。
2. 段
	段，分为**数据段**（Leaf node segment）、**索引段**（Non-leaf node segment）、**回滚段**（Rollback segment），InnoDB是索引组织表，数据段就是B+树的叶子节点， 索引段即为B+树的非叶子节点。段用来管理多个Extent（区）。
3. 区
	区，表空间的**单元结构**，每个区的大小为1M。 默认情况下， InnoDB存储引擎页大小为16K， 即一个区中一共有64个连续的页。
4. 页
	页，是InnoDB 存储引擎**磁盘管理的最小单元**，每个页的大小默认为16KB。为了保证页的连续性，InnoDB存储引擎每次从磁盘申请4-5个区。
5. 行
	行，InnoDB 存储引擎数据是按行进行存放的。
	在行中，默认有两个隐藏字段：
	- `Trx_id`：每次对某条记录进行改动时，都会把对应的事务id赋值给trx_id隐藏列。
	- `Roll_pointer`：每次对某条引记录进行改动时，都会把旧的版本写入到undo日志中，然后这个隐藏列就相当于一个指针，可以通过它来找到该记录修改前的信息。

### 架构
#### 概述
MySQL5.5 版本开始，默认使用**InnoDB存储引擎**，它擅长事务处理，具有崩溃恢复特性，在日常开发中使用非常广泛。下面是InnoDB架构图，左侧为内存结构，右侧为磁盘结构。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217233355.png?" alt="image.png" style="zoom:100%;" />

#### 内存结构

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217233447.png?" alt="image.png" style="zoom:100%;" />
在左侧的内存结构中，主要分为这么四大块儿： Buffer Pool、Change Buffer、Adaptive Hash Index、Log Buffer。 接下来介绍一下这四个部分。


1. Buffer Pool
	- InnoDB存储引擎基于磁盘文件存储，访问物理硬盘和在内存中进行访问，速度相差很大，为了尽可能弥补这两者之间的I/O效率的差值，就需要把经常使用的数据加载到缓冲池中，避免每次访问都进行磁盘I/O。
	- 在InnoDB的缓冲池中不仅缓存了索引页和数据页，还包含了undo页、插入缓存、自适应哈希索引以及InnoDB的锁信息等等。
	- 缓冲池 Buffer Pool，是主内存中的一个**区域**，里面可以缓存磁盘上经常操作的真实数据，在执行增删改查操作时，先操作缓冲池中的数据（若缓冲池没有数据，则从磁盘加载并缓存），然后再**以一定频率刷新**到磁盘，从而减少磁盘IO，加快处理速度。


	缓冲池以Page页为单位，底层采用链表数据结构管理Page。根据状态，将Page分为三种类型：
	• **free page**：空闲page，未被使用。
	• **clean page**：被使用page，数据没有被修改过。
	• **dirty page**：脏页，被使用page，数据被修改过，表中数据与磁盘的数据产生了不一致。
	在专用服务器上，通常将多达80％的物理内存分配给缓冲池 。参数设置： show variables like 'innodb_buffer_pool_size';

	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217233817.png?" alt="image.png" style="zoom:60%;" />
2. Change Buffer
	Change Buffer，更改缓冲区（针对于**非唯一二级索引页**），在执行DML语句时，如果这些数据Page没有在Buffer Pool中，不会直接操作磁盘，而会将数据变更存在更改缓冲区 Change Buffer中，在**未来数据被读取时**，再将数据合并恢复到Buffer Pool中，再将合并后的数据刷新到磁盘中。
	Change Buffer的意义是什么呢?
	先来看一幅图，这个是二级索引的结构图：
	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217233912.png?" alt="image.png" style="zoom:80%;" />
	与聚集索引不同，二级索引通常是非唯一的，并且以相对随机的顺序插入二级索引。同样，删除和更新可能会影响索引树中不相邻的二级索引页，如果每一次都操作磁盘，会造成大量的磁盘IO。有了ChangeBuffer之后，我们可以在缓冲池中进行合并处理，减少磁盘IO。

3. Adaptive Hash Index
	自适应hash索引，用于优化对Buffer Pool数据的查询。MySQL的innoDB引擎中虽然没有直接支持hash索引，但是给我们提供了一个功能就是这个自适应hash索引。因为前面我们讲到过，hash索引在进行等值匹配时，一般性能是要高于B+树的，因为hash索引一般只需要一次IO即可，而B+树，可能需要几次匹配，所以hash索引的效率要高，但是hash索引又不适合做范围查询、模糊匹配等。
	
	InnoDB存储引擎会监控对表上各索引页的查询，如果观察到在特定的条件下hash索引可以提升速度，则建立hash索引，称之为自适应hash索引。
	**自适应哈希索引，无需人工干预，是系统根据情况自动完成**。
	参数： adaptive_hash_index

4. Log Buffer
	Log Buffer：日志缓冲区，用来保存要写入到磁盘中的log日志数据（redo log、undo log）,默认大小为16MB，日志缓冲区的日志会定期刷新到磁盘中。如果需要更新、插入或删除许多行的事务，增加日志缓冲区的大小可以节省磁盘I/O。
	参数:
	- innodb_log_buffer_size：缓冲区大小
	- innodb_flush_log_at_trx_commit：日志刷新到磁盘时机，取值主要包含以下三个：
		- 1: 日志在每次事务提交时写入并刷新到磁盘，默认值。
		- 0: 每秒将日志写入并刷新到磁盘一次。
		- 2: 日志在每次事务提交后写入，并每秒刷新到磁盘一次。
	
		<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240217234235.png?" alt="image.png" style="zoom:80%;" />

#### 磁盘结构
接下来，再来看看InnoDB体系结构的右边部分，也就是磁盘结构：

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305173111.png?" alt="image.png" style="zoom:80%;" />
1. System Tablespace
	系统表空间是更改缓冲区的存储区域。如果表是在系统表空间而不是每个表文件或通用表空间中创建的，它也可能包含表和索引数据。(在MySQL5.x版本中还包含InnoDB数据字典、undolog等)
	参数：`innodb_data_file_path`

	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305173248.png?" alt="image.png" style="zoom:60%;" />
	系统表空间，默认的文件名叫 `ibdata1`。

2. File-Per-Table Tablespaces
	如果开启了innodb_file_per_table开关 ，则每个表的文件表空间包含单个InnoDB表的**数据**和**索引** ，并存储在文件系统上的单个数据文件中。
	开关参数：innodb_file_per_table ，该参数默认开启。

	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305173635.png?" alt="image.png" style="zoom:60%;" />
	那也就是说，我们没创建一个表，都会产生一个表空间文件，如图：

	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305174545.png?" alt="image.png" style="zoom:60%;" />

3. General Tablespaces
	通用表空间，需要通过 `CREATE TABLESPACE` 语法创建通用表空间，在创建表时，可以指定该表空间。
	A. 创建表空间
	```sql
	CREATE TABLESPACE ts_name ADD DATAFILE 'file_name' ENGINE = engine_name;
		```
	
	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305175036.png?" alt="image.png" style="zoom:60%;" />
	B. 创建表时指定表空间

	```sql
	CREATE TABLE xxx ... TABLESPACE ts_name;
	```
	
	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305175107.png?" alt="image.png" style="zoom:60%;" />

4. Undo Tablespaces
	撤销表空间，MySQL实例在初始化时会自动创建两个默认的undo表空间（初始大小16M），用于存储undo log日志。

5. Temporary Tablespaces
	InnoDB 使用会话临时表空间和全局临时表空间。存储用户创建的临时表等数据。

6. Doublewrite Buffer Files
	双写缓冲区，innoDB引擎将数据页从Buffer Pool刷新到磁盘前，先将数据页写入双写缓冲区文件中，便于系统异常时恢复数据。

7. Redo Log
	重做日志，是用来实现事务的持久性。该日志文件由两部分组成：重做日志缓冲（redo log buffer）以及重做日志文件（redo log）,前者是在内存中，后者在磁盘中。当事务提交之后会把所有修改信息都会存到该日志中, 用于在刷新脏页到磁盘时,发生错误时, 进行数据恢复使用。 
	以**循环**(写入的位置不是一直向文件末尾添加，而是在文件中的一个循环区域中写入)方式写入重做日志文件，涉及两个文件：

	前面我们介绍了InnoDB的内存结构，以及磁盘结构，那么内存中我们所更新的数据，又是如何到磁盘中的呢？ 此时，就涉及到一组后台线程，接下来，就来介绍一些InnoDB中涉及到的后台线程。

	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305180141.png?" alt="image.png" style="zoom:60%;" />

#### 后台线程

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305194128.png?" alt="image.png" style="zoom:80%;" />
在InnoDB的后台线程中，分为4类，分别是：`Master Thread` 、`IO Thread`、`Purge Thread`、`Page Cleaner Thread`。

1. Master Thread
	核心后台线程，**负责调度其他线程**，还负责将缓冲池中的数据异步刷新到磁盘中, 保持数据的一致性，还包括脏页的刷新、合并插入缓存、undo页的回收 。

2. IO Thread
	在InnoDB存储引擎中大量使用了AIO(**Asynchronous I/O**)来处理IO请求, 这样可以极大地提高数据库的性能，而IO Thread主要负责这些IO请求的回调。

|         线程类型         | 默认个数 |       职责       |
| :------------------: | :--: | :------------: |
|     Read thread      |  4   |     负责读操作      |
|     Write thread     |  4   |     负责写操作      |
|      Log thread      |  1   | 负责将日志缓冲区刷新到磁盘  |
| Insert buffer thread |  1   | 负责将写缓冲区内容刷新到磁盘 |

我们可以通过以下的这条指令，查看到InnoDB的状态信息，其中就包含IO Thread信息。

```sql
show engine innodb status \G; 
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305195445.png?" alt="image.png" style="zoom:90%;" />

3. Purge Thread
	主要用于回收事务已经提交了的undo log，在事务提交之后，undo log可能不用了，就用它来回收。

4. Page Cleaner Thread
	协助 Master Thread 刷新脏页到磁盘的线程，它可以减轻 Master Thread 的工作压力，减少阻塞。

### 事务原理
#### 事务基础

1. 事务
事务是一组操作的集合，它是一个**不可分割**的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。

2. 特性
- 原子性（Atomicity）：事务是不可分割的最小操作单元，要么全部成功，要么全部失败。
- 一致性（Consistency）：事务完成时，必须使所有的数据都保持一致状态。
- 隔离性（Isolation）：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。
- 持久性（Durability）：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。
 
> 而对于这四大特性，实际上分为两个部分。 其中的原子性、一致性、持久化，实际上是由InnoDB中的两份日志来保证的，一份是**redo log**日志，一份是**undo log**日志。 而隔离性是通过数据库的锁，加上MVCC来保证的。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305200244.png?" alt="image.png" style="zoom:60%;" />

#### redo log
重做日志，记录的是事务提交时**数据页的物理修改**，是用来实现事务的**持久性**。
该日志文件由两部分组成：重做日志缓冲（redo log buffer）以及重做日志文件（redo log file）,前者是在内存中，后者在磁盘中。当事务提交之后会把**所有修改信息**都存到该日志文件中, 用于在**刷新脏页**到磁盘,发生错误时, 进行数据恢复使用。

我们知道，在InnoDB引擎中的内存结构中，主要的内存区域就是缓冲池，在缓冲池中缓存了很多的数据页。 当我们在一个事务中，执行多个增删改的操作时，InnoDB引擎会先操作缓冲池中的数据，如果缓冲区没有对应的数据，会通过后台线程将磁盘中的数据加载出来，存放在缓冲区中，然后将缓冲池中的数据修改，修改后的数据页我们称为脏页。 而脏页则会在一定的时机，通过后台线程刷新到磁盘中，从而保证缓冲区与磁盘的数据一致。 而缓冲区的脏页数据并不是实时刷新的，而是一段时间之后将缓冲区的数据刷新到磁盘中，假如刷新到磁盘的过程出错了，而提示给用户事务提交成功，而数据却没有持久化下来，这就出现问题了，没有保证事务的持久性。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305201905.png?" alt="image.png" style="zoom:60%;" />

那么，如何解决上述的问题呢？ 在InnoDB中提供了一份日志 redo log，接下来我们再来分析一下，通过redolog如何解决这个问题。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305201936.png?" alt="image.png" style="zoom:60%;" />
有了redolog之后，当对缓冲区的数据进行增删改之后，会首先将**操作的数据页的变化**，记录在redo log buffer中。在事务提交时，会将redo log buffer中的数据刷新到redo log磁盘文件中。过一段时间之后，如果刷新缓冲区的脏页到磁盘时，发生错误，此时就可以借助于redo log进行数据恢复，这样就保证了事务的持久性。 而如果脏页成功刷新到磁盘或者涉及到的数据已经落盘，此时redolog就没有作用了，就可以删除了，所以存在的两个redolog文件是循环写的。

那为什么每一次提交事务，要刷新redo log 到磁盘中呢，而不是直接将buffer pool中的脏页刷新到磁盘呢?
因为在业务操作中，我们操作数据一般都是随机读写磁盘的，而不是顺序读写磁盘。 而redo log在往磁盘文件中写入数据，由于是日志文件，所以都是顺序写的。顺序写的效率，要远大于随机写。 这种先写日志的方式，称之为**WAL（Write-Ahead Logging）**。

#### undo log
回滚日志，用于记录数据被修改前的信息, 作用包含两个: 提供回滚(保证事务的原子性)和MVCC(多版本并发控制)。

undo log和redo log记录物理日志不一样,它是逻辑日志。可以认为当delete一条记录时，undo log中会记录一条对应的insert记录，反之亦然，当update一条记录时，它记录一条对应相反的update记录。当执行rollback时，就可以从undo log中的逻辑记录读取到相应的内容并进行回滚。

**Undo log销毁**：undo log在事务执行时产生，事务提交时，并不会立即删除undo log，因为这些日志可能还用于MVCC。

**Undo log存储**：undo log采用段的方式进行管理和记录，存放在前面介绍的 rollback segment回滚段中，内部包含1024个undo log segment。

### MVCC
#### 基本概念

1. 当前读
	读取的是记录的最新版本，读取时还要保证其他并发事务不能修改当前记录，会对读取的记录进行加锁。对于我们日常的操作，如：`select ... lock in share mode(共享锁)`，`select ... for update、update、insert、delete(排他锁)`都是一种当前读。

	测试：
	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305204526.png?" alt="image.png" style="zoom:60%;" />
	在测试中我们可以看到，即使是在默认的RR隔离级别下，事务A中依然可以读取到事务B最新提交的内容，因为在查询语句后面加上了 lock in share mode 共享锁，此时是当前读操作。当然，当我们加排他锁的时候，也是当前读操作。

2. 快照读
	简单的select（不加锁）就是快照读，快照读，读取的是记录数据的可见版本，有可能是历史数据，不加锁，是非阻塞读。
	- Read Committed：每次select，都生成一个快照读。
	- Repeatable Read：开启事务后第一个select语句才是快照读的地方。
	- Serializable：快照读会退化为当前读。

测试:
	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305204848.png?" alt="image.png" style="zoom:60%;" />

在测试中,我们看到即使事务B提交了数据,事务A中也查询不到。 原因就是因为普通的select是快照读，而在当前默认的RR隔离级别下，开启事务后第一个select语句才是快照读的地方，后面执行相同的select语句都是从快照中获取数据，可能不是当前的最新数据，这样也就保证了可重复读。

3. MVCC
全称 Multi-Version Concurrency Control，多版本并发控制。指维护一个数据的多个版本，使得读写操作没有冲突，快照读为MySQL实现MVCC提供了一个非阻塞读功能。MVCC的具体实现，还需要依赖于数据库记录中的三个隐式字段、undo log日志、readView。

接下来，我们再来介绍一下InnoDB引擎的表中涉及到的隐藏字段 、undolog 以及 readview，从而来介绍一下MVCC的原理。

#### 隐藏字段
##### 介绍
| id  | age | name |
| :-: | :-: | :--: |
|  1  |  1  | tom  |
|  3  |  3  | cat  |

当我们创建了上面的这张表，我们在查看表结构的时候，就可以显式的看到这三个字段。 实际上除了这三个字段以外，InnoDB还会自动的给我们添加三个隐藏字段及其含义分别是：

|    隐藏字段     |                     含义                      |
| :---------: | :-----------------------------------------: |
|  DB_TRX_ID  |      最近修改事务ID，记录插入这条记录或最后一次修改该记录的事务ID。      |
| DB_ROLL_PTR | 回滚指针，指向这条记录的上一个版本，用于配合undo log，指向上一个版<br>本。 |
|  DB_ROW_ID  |         隐藏主键，如果表结构没有指定主键，将会生成该隐藏字段。         |
而上述的前两个字段是肯定会添加的， 是否添加最后一个字段`DB_ROW_ID`，得看当前表有没有主键，如果有主键，则不会添加该隐藏字段。

#### undolog
##### 介绍

- 回滚日志，在insert、update、delete的时候产生的便于数据回滚的日志。
- 当insert的时候，产生的undo log日志只在回滚时需要，在事务提交后，可被立即删除。
- 而update、delete的时候，产生的undo log日志不仅在回滚时需要，在快照读时也需要，不会立即被删除。

##### 版本链
有一张表原始数据为：

| id  | age | name | DB_TRX_ID | DB_ROLL_PTR |
| :-: | :-: | :--: | :-------: | :---------: |
| 30  | 30  | A30  |     1     |    null     |
> DB_TRX_ID : 代表最近修改事务ID，记录插入这条记录或最后一次修改该记录的事务ID，是自增的。
> DB_ROLL_PTR ： 由于这条数据是才插入的，没有被更新过，所以该字段值为null。

然后，有四个并发事务同时在访问这张表。
A. 第一步

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305211214.png?" alt="image.png" style="zoom:80%;" />

当事务2执行第一条修改语句时，会记录undo log日志，记录数据变更之前的样子; 然后更新记录，并且记录本次操作的事务ID，回滚指针，回滚指针用来指定如果发生回滚，回滚到哪一个版本。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305211244.png?OSSAccessKeyId=LTAI5tBK1gnqyQLHK2d7sx6F&Expires=9000000001&Signature=VVykSe9qfuniqzcDjM/Oe/IFX94=" alt="image.png" style="zoom:60%;" />
B.第二步
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305211323.png?" alt="image.png" style="zoom:60%;" />
当事务3执行第一条修改语句时，也会记录undo log日志，记录数据变更之前的样子; 然后更新记录，并且记录本次操作的事务ID，回滚指针，回滚指针用来指定如果发生回滚，回滚到哪一个版本。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305211349.png?" alt="image.png" style="zoom:60%;" />
C. 第三步

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305211420.png?" alt="image.png" style="zoom:60%;" />
当事务4执行第一条修改语句时，也会记录undo log日志，记录数据变更之前的样子; 然后更新记录，并且记录本次操作的事务ID，回滚指针，回滚指针用来指定如果发生回滚，回滚到哪一个版本。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240305211449.png?" alt="image.png" style="zoom:60%;" />
> 最终我们发现，不同事务或相同事务对同一条记录进行修改，会导致该记录的undolog生成一条记录版本链表，链表的头部是最新的旧记录，链表尾部是最早的旧记录。 

#### readview

READ VIEW 是[[零散知识点#什么是快照？ 快照与备份有什么区别？|快照]],就是像拍照一样 把这一瞬间的景象(数据)保存下来，
快照一旦形成就不会改变 只能当前快照声明周期了再生成新的快照，
我们的事务隔离等级中的**可重复读**和**读提交**都是通过READ VIEW实现的。

<img src="https://img-blog.csdnimg.cn/direct/897c90805688419c82da557b8c997dca.png" alt="image.png" style="zoom:60%;" />

ReadView中包含了四个核心字段：

|       字段       |         含义         |
| :------------: | :----------------: |
|     m_ids      |    当前活跃的事务ID集合     |
|   min_trx_id   |      最小活跃事务ID      |
|   max_trx_id   | 预分配事务ID，当前最大事务ID+1 |
| creator_trx_id |  ReadView创建者的事务ID  |
而在readview中就规定了版本链数据的访问规则：
在创建 Read View 后，我们可以将记录中的 trx_id 划分这三种情况：

<img src="https://img-blog.csdnimg.cn/direct/9d910766cd234d9f9cdd5df797d0eb53.png" alt="image.png" style="zoom:60%;" />


一个事务去访问记录的时候，除了自己的更新记录总是可见之外，还有这几种情况：

- 如果记录的 trx_id 值小于 Read View 中的 min_trx_id 值，表示这个版本的记录是在创建 Read View 前已经提交的事务生成的，所以该版本的记录对当前事务可见。
- 如果记录的 trx_id 值大于等于 Read View 中的 max_trx_id 值，表示这个版本的记录是在创建 Read View 后才启动的事务生成的，所以该版本的记录对当前事务不可见
- 如果记录的 trx_id 值在 Read View 的 min_trx_id 和 max_trx_id 之间，需要判断 trx_id 是否在 m_ids 列表中：
	- 如果记录的 trx_id 在 m_ids 列表中，表示生成该版本记录的活跃事务依然活跃着（还没提交事务），所以该版本的记录对当前事务不可见。
	- 如果记录的 trx_id 不在 m_ids列表中，表示生成该版本记录的活跃事务已经被提交，所以该版本的记录对当前事务可见。


> [!attention] 注意
> 执行一个事务的时候创建一个READ VIEW。            
> (「读提交」隔离级别是在「每个语句执行前」都会重新生成一个 Read View，                 
> 而「可重复读」隔离级别是「启动事务时」生成一个 Read View，然后整个事务期间都在用这个 Read View。)              
> 更新数据都是先读后写的，而这个读，只能读当前的值，称为“当前读”（current read）
> 


> 

#### 原理分析
##### RC隔离级别
RC隔离级别下，在事务中每一次执行快照读时生成ReadView。

我们就来分析事务5中，两次快照读读取数据，是如何获取数据的?
在事务5中，查询了两次id为30的记录，由于隔离级别为Read Committed，所以每一次进行快照读
都会生成一个ReadView，那么两次生成的ReadView如下。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306094502.png?" alt="image.png" style="zoom:60%;" />
那么这两次快照读在获取数据时，就需要根据所生成的ReadView以及ReadView的版本链访问规则，到undolog版本链中匹配数据，最终决定此次快照读返回的数据。

A. 先来看第一次快照读具体的读取过程：

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306094625.png?" alt="image.png" style="zoom:70%;" />

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306094658.png?" alt="image.png" style="zoom:60%;" />
在进行匹配时，会从undo log的版本链，从上到下进行挨个匹配：

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306095042.png?" alt="image.png" style="zoom:60%;" />
B. 再来看第二次快照读具体的读取过程:

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306095112.png?" alt="image.png" style="zoom:60%;" />
在进行匹配时，会从undo log的版本链，从上到下进行挨个匹配： 

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306095237.png?" alt="image.png" style="zoom:60%;" />

##### RR隔离级别
RR隔离级别下，仅在事务中第一次执行快照读时生成ReadView，后续复用该ReadView。 而RR 是可重复读，在一个事务中，执行两次相同的select语句，查询到的结果是一样的。
那MySQL是如何做到可重复读的呢? 我们简单分析一下就知道了

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306095317.png?" alt="image.png" style="zoom:60%;" />
我们看到，在RR隔离级别下，只是在事务中第一次快照读时生成ReadView，后续都是复用该ReadView，那么既然ReadView都一样， ReadView的版本链匹配规则也一样， 那么最终快照读返回的结果也是一样的。

所以呢，MVCC的实现原理就是通过 InnoDB表的隐藏字段、UndoLog 版本链、ReadView来实现的。而MVCC + 锁，则实现了事务的隔离性。 而一致性则是由redolog与undolog保证。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306095408.png?" alt="image.png" style="zoom:60%;" />

## MySQL管理

### 系统数据库
Mysql数据库安装完成后，自带了一下四个数据库，具体作用如下：

|        数据库         |                            含义                            |
| :----------------: | :------------------------------------------------------: |
|       mysql        |         存储MySQL服务器正常运行所需要的各种信息（**时区、主从、用户、权限等**）         |
| information_schema |          提供了访问数据库元数据的各种表和视图，包含数据库、表、字段类型及访问权限等           |
| performance_schema |        为MySQL服务器运行时状态提供了一个底层监控功能，主要用于收集数据库服务器性能参数        |
|        sys         | 包含了一系列方便 DBA 和开发人员利用 performance_schema性能数据库进行性能调优和诊断的视图 |
### 常用工具
#### mysql
该mysql不是指mysql服务，而是指mysql的客户端工具。

```
语法 ：
	mysql [options] [database]
选项 ：
	-u, --user=name #指定用户名
	-p, --password[=name] #指定密码
	-h, --host=name #指定服务器IP或域名
	-P, --port=port #指定连接端口
	-e, --execute=name #执行SQL语句并退出
```

示例： 

```sql
mysql -uroot -p701121 练习 -e "select *from stu";
```

```sh
mysql -uroot -p701121 练习 -e "select *from stu";
mysql: [Warning] Using a password on the command line interface can be insecure.
+----+-------+-----+
| id | name  | age |
+----+-------+-----+
|  1 | Jsp   |   1 |
|  3 | cat   |   3 |
|  7 | Ruby  |   7 |
|  8 | rose  |   8 |
| 11 | jetty |  11 |
| 19 | stu   |  19 |
| 25 | luci  |  25 |
+----+-------+-----+
```

#### mysqladmin
mysqladmin 是一个执行**管理操作**的客户端程序。可以用它来检查服务器的配置和当前状态、创建并删除数据库等。

```sh
通过帮助文档查看选项：
	mysqladmin --help
```

```sh
Where command is a one or more of: (Commands may be shortened)
  create databasename   Create a new database
  debug                 Instruct server to write debug information to log
  drop databasename     Delete a database and all its tables
  extended-status       Gives an extended status message from the server
  flush-hosts           Flush all cached hosts
  flush-logs            Flush all logs
  flush-status          Clear status variables
  flush-tables          Flush all tables
  flush-threads         Flush the thread cache
  flush-privileges      Reload grant tables (same as reload)
  kill id,id,...        Kill mysql threads
  password [new-password] Change old password to new-password in current format
  ping                  Check if mysqld is alive
  processlist           Show list of active threads in server
  reload                Reload grant tables
  refresh               Flush all tables and close and open logfiles
  shutdown              Take server down
  status                Gives a short status message from the server
  start-replica         Start replication
  start-slave           Deprecated: use start-replica instead
  stop-replica          Stop replication
  stop-slave            Deprecated: use stop-replica instead
  variables             Prints variables available
  version               Get version info from server
```

```sh
语法:
	mysqladmin [options] command ...
选项:
	-u, --user=name #指定用户名
	-p, --password[=name] #指定密码
	-h, --host=name #指定服务器IP或域名
	-P, --port=port #指定连接端口
```

示例:

```sh
mysqladmin -uroot –p1234 drop 'test01';
mysqladmin -uroot –p1234 version;
```

```sql
mysqladmin -uroot -p701121 version
mysqladmin: [Warning] Using a password on the command line interface can be insecure.
C:\Program Files\MySQL\MySQL Server 8.0\bin\mysqladmin.exe  Ver 8.0.30 for Win64 on x86_64 (MySQL Community Server - GPL)
Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Server version          8.0.30
Protocol version        10
Connection              localhost via TCP/IP
TCP port                3306
Uptime:                 45 min 49 sec
```

#### mysqlbinlog
由于服务器生成的二进制日志文件以二进制格式保存，所以如果想要检查这些文本的文本格式，就会使用到mysqlbinlog 日志管理工具。

```sh
语法 ：
	mysqlbinlog [options] log-files1 log-files2 ...
选项 ：
	-d, --database=name 指定数据库名称，只列出指定的数据库相关操作。
	-o, --offset=# 忽略掉日志中的前n行命令。
	-r,--result-file=name 将输出的文本格式日志输出到指定文件。
	-s, --short-form 显示简单格式， 省略掉一些信息。
	--start-datatime=date1 --stop-datetime=date2 指定日期间隔内的所有日志。
	--start-position=pos1 --stop-position=pos2 指定位置间隔内的所有日志。
```

示例:
A. 查看 binlog.000008这个二进制文件中的数据信息
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306101607.png?" alt="image.png" style="zoom:60%;" />

上述查看到的二进制日志文件数据信息量太多了，不方便查询。 我们可以加上一个参数 -s 来显示简单格式。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306101633.png?" alt="image.png" style="zoom:60%;" />

#### mysqlshow
mysqlshow 客户端对象查找工具，用来很快地查找存在哪些数据库、数据库中的表、表中的列或者索引。

```sh
语法 ：
	mysqlshow [options] [db_name [table_name [col_name]]]
选项 ：
	--count 显示数据库及表的统计信息（数据库，表 均可以不指定）
	-i 显示指定数据库或者指定表的状态信息
示例：
	#查询test库中每个表中的字段书，及行数
	mysqlshow -uroot -p2143 test --count
	#查询test库中book表的详细情况
	mysqlshow -uroot -p2143 test book --count
```

A. 查询每个数据库的表的数量及表中记录的数量

```sh
 mysqlshow -uroot -p701121 练习 --count;
```

```sh
Database: 练习
+-------------+----------+------------+
|   Tables    | Columns  | Total Rows |
+-------------+----------+------------+
| a           |        2 |          0 |
| account     |        3 |          2 |
| score       |        5 |          3 |
| stu         |        3 |          7 |
| student     |        3 |          5 |
| t_course    |        3 |          3 |
| t_score     |        3 |         18 |
| t_student   |        4 |         13 |
| t_teacher   |        2 |          3 |
| tb_user     |        9 |         24 |
| tb_user_pro |        3 |         11 |
| user_logs   |        5 |          8 |
| v1          |        4 |          7 |
| v2          |        4 |          4 |
| v3          |        4 |          4 |
+-------------+----------+------------+
15 rows in set.
```

B. 查看数据库练习score表的统计信息

```sh
mysqlshow -uroot -p701121 练习 score --count;
```

```sh
Database: 练习  Table: score  Rows: 3
+---------+-------------+--------------------+------+-----+---------+-------+---------------------------------+---------+
| Field   | Type        | Collation          | Null | Key | Default | Extra | Privileges                      | Comment |
+---------+-------------+--------------------+------+-----+---------+-------+---------------------------------+---------+
| id      | int         |                    | YES  |     |         |       | select,insert,update,references | ID      |
| name    | varchar(20) | utf8mb4_0900_ai_ci | YES  |     |         |       | select,insert,update,references | 姓名    |
| math    | int         |                    | YES  |     |         |       | select,insert,update,references | 数学    |
| english | int         |                    | YES  |     |         |       | select,insert,update,references | 英语    |
| chinese | int         |                    | YES  |     |         |       | select,insert,update,references | 语文    |
+---------+-------------+--------------------+------+-----+---------+-------+---------------------------------+---------+
```

C.查看数据库练习中的score表的math字段的信息

```sh
mysqlshow -uroot -p701121 练习 score math --count;
```

```sh
Database: 练习  Table: score  Rows: 3  Wildcard: math
+-------+------+-----------+------+-----+---------+-------+---------------------------------+---------+
| Field | Type | Collation | Null | Key | Default | Extra | Privileges                      | Comment |
+-------+------+-----------+------+-----+---------+-------+---------------------------------+---------+
| math  | int  |           | YES  |     |         |       | select,insert,update,references | 数学    |
+-------+------+-----------+------+-----+---------+-------+---------------------------------+---------+
```

#### mysqldump
mysqldump 客户端工具用来备份数据库或在不同数据库之间进行数据迁移。备份内容包含创建表，及插入表的SQL语句。

```sh
语法 ：
	mysqldump [options] db_name [tables]
	mysqldump [options] --database/-B db1 [db2 db3...]
	mysqldump [options] --all-databases/-A
连接选项 ：
	-u, --user=name 指定用户名
	-p, --password[=name] 指定密码
	-h, --host=name 指定服务器ip或域名
	-P, --port=# 指定连接端口
输出选项：
	--add-drop-database 在每个数据库创建语句前加上 drop database 语句
	--add-drop-table 在每个表创建语句前加上 drop table 语句 , 默认开启 ; 不开启 (--skip-add-drop-table)
	-n, --no-create-db 不包含数据库的创建语句
	-t, --no-create-info 不包含数据表的创建语句
	-d --no-data 不包含数据
	-T, --tab=name 自动生成两个文件：一个.sql文件，创建表结构的语句；一个.txt文件，数据文件
```

示例:
A. 备份db01数据库
> mysqldump -uroot -p1234 db01 > db01.sql

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306103214.png?" alt="image.png" style="zoom:60%;" />
可以直接打开db01.sql，来查看备份出来的数据到底什么样。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306103242.png?=" alt="image.png" style="zoom:60%;" />

备份出来的数据包含：
- 删除表的语句
- 创建表的语句
- 数据插入语句
如果我们在数据备份时，不需要创建表，或者不需要备份数据，只需要备份表结构，都可以通过对应的参数来实现。

B. 备份db01数据库中的表数据，不备份表结构(-t)
> mysqldump -uroot -p1234 -t db01 score > score.sql

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306103327.png?" alt="image.png" style="zoom:60%;" />


打开 `db02.sql` ，来查看备份的数据，只有insert语句，没有备份表结构。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306103346.png?" alt="image.png" style="zoom:60%;" />
C. 将db01数据库的表的表结构与数据分开备份(-T)
> mysqldump -uroot -p1234 -T /root db01 score

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306103611.png?" alt="image.png" style="zoom:60%;" />
执行上述指令，会出错，数据不能完成备份，原因是因为我们所指定的数据存放目录/root，MySQL认为是不安全的，需要存储在MySQL信任的目录下。那么，哪个目录才是MySQL信任的目录呢，可以查看一下系统变量 secure_file_priv 。执行结果如下：
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306103638.png?" alt="image.png" style="zoom:60%;" />

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306103654.png?" alt="image.png" style="zoom:60%;" />
上述的两个文件 score.sql 中记录的就是表结构文件，而 score.txt 就是表数据文件，但是需要注意表数据文件，并不是记录一条条的insert语句，而是按照一定的格式记录表结构中的数据。如下：

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306103724.png?" alt="image.png" style="zoom:60%;" />

#### mysqlimport/source
1. mysqlimport
	mysqlimport 是客户端数据**导入**工具，用来导入mysqldump 加 -T 参数后导出的文本文件。

	```sql
	语法 ：
		mysqlimport [options] db_name textfile1 [textfile2...]
	示例 ：
		mysqlimport -uroot -p2143 test /tmp/city.txt
	```
	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306104507.png?" alt="image.png" style="zoom:60%;" />
1. source
	如果需要导入sql文件,可以使用mysql中的source 指令 :

	```sql
	语法 ：
	source /root/xxxxx.sql
	```


## 日志

### 错误日志
错误日志是 MySQL 中最重要的日志之一，它记录了当 mysqld 启动和停止时，以及服务器在运行过程中发生任何**严重错误时的相关信息**。当数据库出现任何故障导致无法正常使用时，建议首先查看此日
志。
该日志是默认开启的，默认存放目录 `/var/log/`，默认的日志文件名为 mysqld.log 。查看日志
位置：

```sh
show variables like '%log_error%';
```

```sh
+---------------+--------------------------+
| Variable_name | Value                    |
+---------------+--------------------------+
| log_error     | /var/log/mysql/error.log |
+---------------+--------------------------+
```

### 二进制日志
#### 介绍
二进制日志（BINLOG）记录了所有的 DDL（数据定义语言）语句和 DML（数据操纵语言）语句，但**不包括数据查询（SELECT、SHOW）语句**。
作用：①. 灾难时的数据恢复；②.MVySQL的主从复制。在MySQL8版本中，默认二进制日志是开启着的，涉及到的参数如下：

```sh
show variables like '%log_bin%';
```


```sh
+---------------------------------+-----------------------------+
| Variable_name                   | Value                       |
+---------------------------------+-----------------------------+
| log_bin                         | ON                          |
| log_bin_basename                | /var/lib/mysql/binlog       |
| log_bin_index                   | /var/lib/mysql/binlog.index |
| log_bin_trust_function_creators | OFF                         |
| log_bin_use_v1_row_events       | OFF                         |
| sql_log_bin                     | ON                          |
+---------------------------------+-----------------------------+
```

参数说明：
- log_bin_basename：当前数据库服务器的binlog日志的基础名称(前缀)，具体的binlog文件名需要再该basename的基础上加上编号(编号从000001开始)。
- log_bin_index：binlog的索引文件，里面记录了当前服务器关联的binlog文件有哪些。

#### 格式
MySQL服务器中提供了多种格式来记录二进制日志，具体格式及特点如下：

|   日志格式    |                            含义                             |
| :-------: | :-------------------------------------------------------: |
| STATEMENT |       基于SQL语句的日志记录，记录的是SQL语句，对数据进行修改的SQL都会记录在日志文件中。       |
|    ROW    |                基于行的日志记录，记录的是每一行的数据变更。（默认）                 |
|   MIXED   | 混合了STATEMENT和ROW两种格式，默认采用STATEMENT，在某些特殊情况下会自动切换为ROW进行记录。 |

```sh
show variables like '%binlog_format%'; 
```

```sh
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| binlog_format | ROW   |
+---------------+-------+
```

如果我们需要配置二进制日志的格式，只需要在 `/etc/mysql/my.cnf` 中配置 binlog_format 参数即可。


#### 查看
由于日志是以**二进制方式**存储的，不能直接读取，需要通过二进制日志查询工具 mysqlbinlog 来查看，具体语法：

```sh
mysqlbinlog [ 参数选项 ] logfilename

参数选项：
	-d 指定数据库名称，只列出指定的数据库相关操作。
	-o 忽略掉日志中的前n行命令。
	-v 将行事件(数据变更)重构为SQL语句
	-vv 将行事件(数据变更)重构为SQL语句，并输出注释信息
```

#### 删除
对于比较繁忙的业务系统，每天生成的binlog数据巨大，如果长时间不清除，将会占用大量磁盘空间。可以通过以下几种方式清理日志：

|                         指令                         |                   含义                    |
| :------------------------------------------------: | :-------------------------------------: |
|                   `reset master`                   | 删除全部 binlog 日志，日志编号从 binlog.000001 重新开始 |
|         `purge master logs to 'binlog.*'`          |             删除 * 编号之前的所有日志              |
| `purge master logs before 'yyyy-mm-dd hh24:mi:ss'` |           删除指定日期和时间之前产生的所有日志            |
也可以在mysql的配置文件中配置二进制日志的过期时间，设置了之后，二进制日志过期会自动删除。

```sh
show variables like '%binlog_expire_logs_seconds%'; 
```


### 查询日志
查询日志中记录了客户端的所有操作语句，而二进制日志不包含查询数据的SQL语句。默认情况下，
查询日志是未开启的。

```sh
mysql> show variables like '%general%';
+------------------+------------------------------------+
| Variable_name    | Value                              |
+------------------+------------------------------------+
| general_log      | OFF                                |
| general_log_file | /var/lib/mysql/LAPTOP-UMBENK9B.log |
+------------------+------------------------------------+
```

如果需要开启查询日志，可以修改MySQL的配置文件 /etc/my.cnf 文件，添加如下内容：

```sh
#该选项用来开启查询日志 ， 可选值 ： 0 或者 1 ； 0 代表关闭， 1 代表开启
general_log=1
#设置日志的文件名 ， 如果没有指定， 默认的文件名为 host_name.log
general_log_file=mysql_query.log
```

开启了查询日志之后，在MySQL的数据存放目录，也就是 `/var/lib/mysql/` 目录下就会出现`mysql_query.log` 文件。之后所有的客户端的增删改查操作都会记录在该日志文件之中，长时间运行后，该日志文件将会非常大。

### 慢查询日志
慢查询日志记录了所有执行时间超过参数 long_query_time 设置值并且扫描记录数不小于`min_examined_row_limit` 的所有的SQL语句的日志，默认未开启。long_query_time 默认为10 秒，最小为 0， 精度可以到微秒。
如果需要开启慢查询日志，需要在MySQL的配置文件 `/etc/my.cnf` 中配置如下参数：

```sh
#慢查询日志
slow_query_log=1
#执行时间参数
long_query_time=2
```

默认情况下，不会记录管理语句，也不会记录不使用索引进行查找的查询。可以使用`log_slow_admin_statements`和更改此行为`log_queries_not_using_indexes`，如下所述。

```sh
#记录执行较慢的管理语句
log_slow_admin_statements =1
#记录执行较慢的未使用索引的语句
log_queries_not_using_indexes = 1
```

> 上述所有的参数配置完成之后，都需要重新启动MySQL服务器才可以生效。


```sql
1. 索引优化
   - 合适的索引
   - 避免过多索引
   - 注意索引失效情况

2. 查询优化
   - 只查需要的列
   - 合理使用JOIN
   - 避免子查询
   - 合理使用索引

3. 配置优化
   - 内存配置
   - 并发配置
   - 缓存配置

4. 架构优化
   - 读写分离
   - 分库分表
   - 使用缓存
```


## 主从复制
### 概述
主从复制是指将主数据库的 DDL 和 DML 操作**通过二进制日志传到从库服务器中**，然后在从库上对这些日志**重新执行**（也叫重做），从而使得从库和主库的数据保持同步。
MySQL支持一台主库同时向多台从库进行复制， 从库同时也可以作为其他从服务器的主库，实现**链状复制**。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306145808.png?" alt="image.png" style="zoom:60%;" />

MySQL 复制的优点主要包含以下三个方面：
- 主库出现问题，可以快速切换到从库提供服务。
- 实现读写分离，降低主库的访问压力。
- 可以在从库中执行备份，以避免备份期间影响主库服务。

### 原理
MySQL主从复制的核心就是二进制日志，具体的过程如下：

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306150031.png?" alt="image.png" style="zoom:60%;" />

从上图来看，复制分成三步：
1. Master 主库在事务提交时，会把数据变更记录在二进制日志文件 Binlog 中。
2. 从库读取主库的二进制日志文件 `Binlog` ，写入到从库的中继日志 `Relay Log` 。
3. slave重做中继日志中的事件，将改变反映它自己的数据。

### 搭建

可以用docker进行搭建，具体看这个[博客](https://liangxr.blog.csdn.net/article/details/128099023) 

#### 主库配置
在主服务器上授予主从复制权限

```sh
#创建slave用户，并设置密码，该用户可在任意主机连接该MySQL服务
CREATE USER 'slave'@'%' IDENTIFIED WITH mysql_native_password BY '123456'
;
#为 'itcast'@'%' 用户分配主从复制权限
GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'slave'@'%';
```

通过指令，查看二进制日志坐标

```sh
show master status;
+------------------+----------+--------------+----------------
| File             | Position | Binlog_Do_DB | Binlog_Ignore_D
+------------------+----------+--------------+----------------
| mysql-bin.000003 |      683 |              |
+------------------+----------+--------------+----------------
```

字段含义说明：
- file : 从哪个日志文件开始推送日志文件
- position ： 从哪个位置开始推送日志
- binlog_ignore_db : 指定不需要同步的数据库

#### 从库配置

登录mysql，设置主库配置

```sh
change master to master_host='172.17.0.2', master_user='slave', master_password='123456', master_port=3306, master_log_file='mysql-bin.000003', master_log_pos= 683, master_connect_retry=30;
```


|      含义      |       参数名       |
| :----------: | :-------------: |
|    主库IP地址    |   MASTER_HOST   |
|   连接主库的用户名   |   MASTER_USER   |
|   连接主库的密码    | MASTER_PASSWORD |
| binlog日志文件名  | MASTER_LOG_FILE |
| binlog日志文件位置 | MASTER_LOG_POS  |
开启同步操作

```sh
start slave ; 
```

查看主从同步状态

```sh
show slave status ;
```

#### 测试
1. 在主库上创建数据库、表，并插入数据

```sh
create database db01;
use db01;
create table tb_user(
	id int(11) primary key not null auto_increment,
	name varchar(50) not null,
	sex varchar(1)
)engine=innodb default charset=utf8mb4;
insert into tb_user(id,name,sex) values(null,'Tom', '1'),(null,'Trigger','0'),
(null,'Dawn','1');
```

在从库中查询数据，验证主从是否同步

## 分库分表

### 介绍

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306210226.png?" alt="image.png" style="zoom:60%;" />


随着互联网及移动互联网的发展，应用系统的数据量也是成指数式增长，若采用单数据库进行数据存储，存在以下性能瓶颈：
1. **IO瓶颈**：热点数据太多，数据库缓存不足，产生大量磁盘IO，效率较低。 请求数据太多，带宽不够，网络IO瓶颈。
2. **CPU瓶颈**：排序、分组、连接查询、聚合统计等SQL会耗费大量的CPU资源，请求数太多，CPU出现瓶颈。

为了解决上述问题，我们需要对数据库进行分库分表处理。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306210301.png?" alt="image.png" style="zoom:60%;" />
分库分表的中心思想都是将数据分散存储，使得单一数据库/表的数据量变小来缓解单一数据库的性能问题，从而达到提升数据库性能的目的。

#### 拆分策略
分库分表的形式，主要是两种：垂直拆分和水平拆分。而拆分的粒度，一般又分为分库和分表，所以组成的拆分策略最终如下：

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306210420.png?" alt="image.png" style="zoom:60%;" />


#### 垂直拆分

##### 垂直分库

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306210500.png?" alt="image.png" style="zoom:70%;" />
垂直分库：以**表为依据**，根据业务将不同表拆分到不同库中。
特点：
- 每个库的**表结构都不一样**。
- 每个库的**数据也不一样**。
- 所有库的并集是全量数据。


##### 垂直分表

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306210712.png?" alt="image.png" style="zoom:70%;" />


垂直分表：以字段为依据，根据字段属性将不同字段拆分到不同表中。
特点：
- 每个表的结构都不一样。
- 每个表的数据也不一样，一般通过一列（主键/外键）关联。
- 所有表的并集是全量数据。

#### 水平拆分
##### 水平分库

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306210750.png?" alt="image.png" style="zoom:70%;" />
水平分库：以字段为依据，按照一定策略，将**一个库的数据**拆分到多个库中。
特点：
- 每个库的表结构都一样。
- 每个库的数据都不一样。
- 所有库的并集是全量数据。

##### 水平分表

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306210941.png?" alt="image.png" style="zoom:70%;" />

水平分表：以字段为依据，按照一定策略，将一个表的数据拆分到多个表中。
特点：
- 每个表的表结构都一样。
- 每个表的数据都不一样。
- 所有表的并集是全量数据。

> 在业务系统中，为了缓解磁盘IO及CPU的性能瓶颈，到底是垂直拆分，还是水平拆分；具体是分库，还是分表，都需要根据具体的业务需求具体分析。 

#### 实现技术

**shardingJDBC**：基于AOP原理，在应用程序中对本地执行的SQL进行拦截，解析、改写、路由处理。需要自行编码配置实现，只支持java语言，性能较高。
**MyCat**：数据库分库分表中间件，不用调整代码即可实现分库分表，支持多种语言，性能不及前者。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306211124.png?" alt="image.png" style="zoom:60%;" />

本次课程，我们选择了是MyCat数据库中间件，通过MyCat中间件来完成分库分表操作。

### MyCat概述
#### 介绍
Mycat是开源的、活跃的、基于Java语言编写的MySQL数据库中间件。可以像使用mysql一样来使用mycat，对于开发人员来说根本感觉不到mycat的存在。
开发人员只需要连接MyCat即可，而具体底层用到几台数据库，每一台数据库服务器里面存储了什么数据，都无需关心。 具体的分库分表的策略，只需要在MyCat中配置即可。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306211235.png?" alt="image.png" style="zoom:60%;" />

优势：
- 性能可靠稳定
- 强大的技术团队
- 体系完善
- 社区活跃

## 读写分离
### 介绍

读写分离,简单地说是把对数据库的读和写操作分开,以对应不同的数据库服务器。主数据库提供写操作，从数据库提供读操作，这样能有效地减轻单台数据库的压力。
通过MyCat即可轻易实现上述功能，不仅可以支持MySQL，也可以支持Oracle和SQL Server。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306215306.png?" alt="image.png" style="zoom:60%;" />

### 一主一从
#### 原理
MySQL的主从复制，是基于二进制日志（binlog）实现的。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240306215359.png?" alt="image.png" style="zoom:60%;" />

后面的省略



## MySQL性能 

1.1 分析-数据库查询效率低下

要提高操作数据库的性能，有如下两种方式：

①硬优化：就是软优化之后性能还很低，只能采取硬优化，最后的步骤了，就是公司花钱购买服务器，在硬件上进行优化。

②**软优化**: 在操作和设计数据库方面上进行优化（重点）(**表结构和sql语句**)

1.2 分析-执行次数比较多的语句

​	1.执行次数比较多的语句分类

​		1）查询密集型
 	  2）修改密集型 es solor

​	2.查询累计插入和返回数据条数，即查看当前数据库属于查询密集型还是修改密集型

```sql
-- 查询累计插入和返回数据条数show global status like 'Innodb_rows%'
```

<img src="https://article.biliimg.com/bfs/article/1f870337ec8c76f1f33107da461ea94a83af9d7e.png" alt="image-20230327111939912" style="zoom:50%;" />

1.3 查看SQL语句的执行效率

准备千万级别数据:

```sql
-- 1. 准备表
CREATE TABLE user(
    id INT,
    username VARCHAR(32),
    password VARCHAR(32),
    sex VARCHAR(6),
    email VARCHAR(50)
);
-- 2. 创建存储过程，实现批量插入记录
DELIMITER $$ -- 声明存储过程的结束符号为$$
-- 可以将下面的存储过程理解为java中的一个方法，插入千万条数据之后，在调用存储过程CREATE PROCEDURE auto_insert()
BEGIN
DECLARE i INT DEFAULT 1;
START TRANSACTION; -- 开启事务
WHILE(i<=10000000)DO
INSERT INTO user VALUES(i,CONCAT('jack',i),MD5(i),'male',CONCAT('jack',i,'@itcast.cn'));
SET i=i+1;
END WHILE;
COMMIT; -- 提交
END$$ -- 声明结束
DELIMITER ; -- 重新声明分号为结束符号
-- 3. 查看存储过程
SHOW CREATE PROCEDURE auto_insert;
-- 4. 调用存储过程
CALL auto_insert();
```

说明:上述sql大概运行10分钟左右; 

```sql
-- 查询id是22的用户,测试查询耗时
select * from user where id=22; -- 5.5s
```

## JDBC 简介



客户端操作MySQL数据库的方式:

1. 使用第三方客户端来访问MySQL：SQLyog、Navicat

2. 使用MySQL自带的命令行方式

3. **通过Java来访问MySQL数据库，今天要学习的内容**

   **如何通过Java代码去操作数据库呢？**
   

**什么是JDBC?：**
	Sun公司为了简化、统一对数据库的操作，定义了==一套java操作数据库的接口的规范==，称之为JDBC。JDBC的全称为：java database connection (java和 数据库的连接 ) 就是使用java代码来操作数据库。   ^123dd1

JDBC的原理 

1. **java程序依赖于jdk,jdk和数据库是2个不同的应用程序，那他们是怎么进行访问的呢？**

   要想搞清楚这个问题，我们必须了解下电脑是如何和其他硬件设备**交互**的。假设我们电脑安装完系统之后是一个无驱动的操作系统。那么当我们电脑想播放声音，即安装音响，必须得安装声卡驱动。同时电脑想和u盘硬件进行交互，也必须在电脑上安装对应的驱动。如果驱动安装失败，很显然他们是不能正常的交互的。这个==安装驱动的其实就是为了定义他们两个相互交互的规则==。只有统一了规则，才能交互。

   具体的解释如下图所示：

   ![image-20230327173700123](https://article.biliimg.com/bfs/article/2c199dea0f02b6b49d986b07ea87f3392ec63a80.png)

   同理：java程序想和数据库进行交互，也必须得安装数据库驱动，这样才能交互。但是，我们数据库有多种，这样就会导致不同的数据库具备不同的数据库驱动。sun公司就会制定一套规则，这套规则就是用来java程序连接数据库的，然后各**大数据库厂商只需要实现**这个规则即可。这个规则就是jdbc技术，即接口。换句话就是说，就是数据库厂商使用sun公司提供的接口，然后作为java程序员实现接口中的方法即可。**接口中的方法体由具体的数据库厂商来实现。**

<img src="https://article.biliimg.com/bfs/article/7b298aaf211914b5cfd5628b71e0f995ff503cb5.png" alt="image-20230327173719171" style="zoom:50%;" />



**JDBC四个核心对象**

这几个类都是在java.sql包中

1. DriverManager(类):  数据库驱动管理类。这个类的作用：1）注册驱动; 2)创建java代码和数据库之间的连接，即获取Connection接口;
2. Connection(接口): 是一个接口, 建立数据库连接的一个接口。作用：建立数据库和java代码之间的连接。表示与数据库创建的连接
3. Statement(接口)、PreparedStatement(接口) (解决安全隐患问题，比如sql注入的问题)： 数据库操作，向数据库发送sql语句。执行SQL语句的对象
4. ResultSet: 结果集或一张虚拟表。 Statement 发送sql语句，得到的结果 封装在 ResultSet 中。
5. ![](https://article.biliimg.com/bfs/article/bfaae742fdfa31c3ed845690522071f675a885bb.png)

DBC四个核心对象？

1. DriverManager(类): 用于注册驱动和获取连接
2. Connection(接口): 表示与数据库创建的连接
3. Statement(接口): 执行SQL语句的对象
4. ResultSet(接口): 结果集或一张虚拟表

## JDBC API 详解

**JDBC注册驱动**

`Connection`表示Java程序与数据库之间的连接，只有拿到Connection才能操作数据库。

我们Java程序需要通过数据库驱动才能连接到数据库，因此需要注册驱动。
MySQL的驱动的入口类是：`com.mysql.jdbc.Driver`
![](https://article.biliimg.com/bfs/article/669f3c722f6323f5460998011ae3a65b0341493c.png)

**API介绍**

`java.sql.DriverManager`类用于注册驱动。提供如下方法注册驱动

```java
static void registerDriver(Driver driver) 
向 DriverManager 注册给定驱动程序。 
```

说明：当前的DriverManager.registerDriver(Driver driver);方法的参数是Driver，这是jdbc的一个接口，所以我们需要给定==实现该接口的实现类==。如果我们连接的是mysql数据库，那么需要导入mysql数据库提供的包，也就是com.mysql.jdbc.Driver; 下的Driver类。如果我们连接的是oracle数据库，那么需要导入oracle数据库提供的包。

案例代码

```java
public class Demo01 {
	public static void main(String[] args) throws Exception {
		// 注册驱动
		DriverManager.registerDriver(new com.mysql.jdbc.Driver());
	}
}
```

说明：这里的new Driver()的类Driver就是来自mysql数据库。

通过查询com.mysql.jdbc.Driver源码，我们发现Driver类“主动”将自己进行注册

```java
public class Driver extends NonRegisteringDriver implements java.sql.Driver {
    static {
        try {
            // 自己自动注册
            java.sql.DriverManager.registerDriver(new Driver());
        } catch (SQLException E) {
            throw new RuntimeException("Can't register driver!");
        }
    }
    public Driver() throws SQLException {
    }
}
```

![](https://article.biliimg.com/bfs/article/596d84843c35be117d51329d5ef7cef2a2f73552.png)

>注意：使用`DriverManager.registerDriver(new com.mysql.jdbc.Driver());`，存在以下方面不足
>
>1. 驱动被注册两次

使用`Class.forName("com.mysql.cj.jdbc.Driver");`加载驱动，这样驱动只会注册一次

通常开发我们使用Class.forName() 加载驱动。`Class.forName("com.mysql.cj.jdbc.Driver");`会走Driver类的静态代码块。在静态代码块中注册一次驱动。
![](https://article.biliimg.com/bfs/article/aeb77f778d46340a7758dfa99e7ef779b96456cd.png)

>总结：注册MySQL驱动使用`Class.forName("com.mysql.cj.jdbc.Driver");`

**获取连接**

能够通过JDBC获取数据库连接

**API介绍**

`java.sql.DriverManager`类中有如下方法获取数据库连接

```java
static Connection getConnection(String url, String user, String password) 
连接到给定数据库 URL ，并返回连接。 
```

**参数说明**

1. `String url`：连接数据库的URL，用于说明连接数据库的位置
2. `String user`：数据库的账号
3. `String password`：数据库的密码

连接数据库的URL地址格式：`协议名:子协议://服务器名或IP地址:端口号/数据库名?参数=参数值`
![](https://article.biliimg.com/bfs/article/a25a5a9ed9910cda1b90f8a16d62f22fe9362d26.png)
MySQL写法：`jdbc:mysql://localhost:3306/day04_db`
如果是本地服务器，端口号是默认的3306，则可以简写：`jdbc:mysql:///day04_db`

![image-20230327175357747](https://article.biliimg.com/bfs/article/84e62996aa47ee628f49d9131be53aecec7f433f.png)

**JDBC实现对单表数据增、删、改**

我们要对数据库进行增、删、改、查，需要使用`Statement`对象来执行SQL语句。

**API介绍**

获取Statement对象

在`java.sql.Connection`接口中有如下方法获取到`Statement`对象

```java
Statement createStatement() 
创建一个 Statement 对象来将 SQL 语句发送到数据库
PreparedStatement prepareStatement(String sql) 创建一个 PreparedStatement 对象来将参数化的 SQL 语句发送到数据库。
//PreparedStatement 是安全的，Statement不安全的。并且Statement效率低，PreparedStatement效率高
```

Statement的API介绍
1.
```java
   int executeUpdate(String sql)
   根据执行的DML（INSERT、UPDATE、DELETE）语句，返回受影响的行数
```
2.
```java
   ResultSet executeQuery(String sql)
   根据查询语句返回结果集,只能执行SELECT语句
```

   >注意：在MySQL中，只要==不是查询就是修改==。
   >executeUpdate：用于执行增删改
   >executeQuery：用于执行查询

Connection还可以手动控制mysql事务：

```java
//开启事务  
void setAutoCommit(boolean autoCommit) 
//将此连接的自动提交模式设置为给定状态。autoCommit - 为 true 表示启用自动提交模式；为 false 表示禁用自动提交模式
1. conn.setAutoCommit(false);//一切正常提交事务  void commit()
2. conn.commit()//出现异常，回滚事务  
void rollback()
3. conn.rollback()
```

Statement向数据库发送sql语句，使用Statement中的不同的方法可以向数据库发送不同的sql语句：

```java
// 1）DQL查询语句：  执行给定的 SQL 语句，该语句返回单个 ResultSet 对象。
ResultSet executeQuery(String sql) 
/* 参数：sql - 要发送给数据库的 SQL 语句，通常为静态 SQL SELECT 语句
返回值：ResultSet用来存放查询的结果，表示结果集 */
// 2）DML增删改和DDL语句(创建表和数据库)使用的方法：
int executeUpdate(String sql)
/* 执行给定 SQL 语句，该语句可能为 INSERT、UPDATE 或 DELETE 语句(DML)，或者不返回任何内容的 SQL 语句（如 SQL DDL 语句）。*/
//返回值：
1) 对于 SQL 数据操作语言 (DML) 语句，返回行记录数，影响的行数
2) 对于什么都不返回的 SQL 语句，返回 0 ，执行DDL返回的是0 了解
```

测试

```java
// 1.插入记录
		String sql = "insert into user values(null, 'zhaoliu', 'abc')";
		int i = stmt.executeUpdate(sql);
		System.out.println("影响的行数:" + i);
		// 2.修改记录
		sql = "update user set username='tianqi' where username='zhaoliu'";
		i = stmt.executeUpdate(sql);
		System.out.println("影响的行数:" + i);
		// 3.删除记录
		sql = "delete from user where id=4";
		i = stmt.executeUpdate(sql);
		System.out.println("影响的行数:" + i);	
		// 释放资源
		stmt.close();
		conn.close();
```

**JDBC实现对单表数据查询**

`ResultSet`用于保存执行查询SQL语句的结果。
我们不能一次性取出所有的数据，需要一行一行的取出。

**resultSet的原理**

1. ResultSet内部有一个指针,刚开始记录开始位置
2. 调用next方法, ResultSet内部指针会移动到下一行数据
3. 我们可以通过ResultSet得到一行数据 getXxx得到某列数据
   ![](https://article.biliimg.com/bfs/article/38b648db6d6d0211f92cf54fe209cc8e4d090128.png)

**ResultSet获取数据的API**

其实ResultSet获取数据的API是有规律的get后面加数据类型。我们统称`getXXX()`

<img src="https://article.biliimg.com/bfs/article/55754bf29d5b83dc310f947ff9bc80749a7a05b0.png" alt="image.png" style="zoom:60%;" />

​     例如：

<img src="https://article.biliimg.com/bfs/article/9bf2c2a7ae1824f66b6ec4ca8f67703fc08365ca.png" alt="image.png" style="zoom:60%;" />

对于上图中的一行数据，我要获取username为zhangsan这列的值，有如下==2种写法==：

    1) rs.getString(“username”);  通过列名获取该列的值。
    2) rs.getString(2);        通过username==列所在的第二个位置==获取该列的值。

案例代码

```java
public class Demo04 {
	public static void main(String[] args) throws Exception {
		Class.forName("com.mysql.cj.jdbc.Driver");	
		Connection conn = DriverManager.getConnection("jdbc:mysql:///day04_db", "root", "1234");
		Statement stmt = conn.createStatement();
		String sql = "select * from user";
		ResultSet rs = stmt.executeQuery(sql);	
		// 内部有一个指针,只能取指针指向的那条记录
        while(rs.next()){// 指针移动一行,有数据才返回true
				int id = rs.getInt("id");
				String name = rs.getString(2);
				String pwd= rs.getString(3);
				System.out.println(id+"+++"+name+"++++"+pwd);
			}	
		// 关闭资源
		rs.close();
		stmt.close();
		conn.close();
	}
}
```

**JDBC实现登录案例**

```java
public class JDBCTest04 {
    public static void main(String[] args) throws Exception {
        /*
            练习：模拟登录，如果登录成功，提示登录成功，登录失败，提示失败
         */
        //1.创建键盘录入对象
        Scanner sc = new Scanner(System.in);
        //2.提示输入用户名和密码
        System.out.println("-------请输入用户名:-------");
        //获取用户名
        String inputUsername = sc.nextLine();
        System.out.println("-------请输入密码:-------");
        //获取密码
        String inputPassword = sc.nextLine();
        //3.获取数据
        //4.注册驱动
        //5.获取和数据库连接对象
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/day03_heima139", "root", "1234");
        //6.获取发送sql语句的对象
        Statement st = conn.createStatement();
        //7.发送sql语句
        /*
            错误的完整的sql语句：select * from user3 where username=zhangsanand password=123
            正确的完整的sql语句：select * from user3 where username='zhangsan' and password='123'
         */
        ResultSet rs = st.executeQuery("select * from user3 where username='" + inputUsername + "' and password='" + inputPassword+"'");
        //8.处理结果集
        //用户名唯一，查询的是一条数据，所以这里使用if即可
        if(rs.next()){
            //rs.next() :如果当前指针指向的行有数据则返回true
            //获取用户名
            String username = rs.getString("username");
            //输出
            System.out.println("恭喜您，亲，登录成功，欢迎光临我的小店，你的用户名是"+username);
        }else{
            System.out.println("用户名或者密码错误");
        }
        //9.释放资源
        rs.close();
        st.close();
        conn.close();
    }
}
```



**PreparedStatement**

1. 什么是SQL注入?
   - 由于没有对用户输入进行充分检查，而SQL又是拼接而成，在用户输入参数时，在参数中添加一些SQL 关键字，达到改变SQL运行结果的目的，也可以完成恶意攻击。
   - 简单来说就是：用户在页面提交数据的时候人为的添加一些特殊字符，使得sql语句的结构发生了变化，最终可以在没有用户名或者密码的情况下进行登录。
2. sql注入原因演示_模拟登陆
   sql注入代码演示：
   需求: 根据用户名和密码 查询用户信息(只知道用户名,不知道密码)。
   恶意注入方式：在sql语句中添加 -- 是mysql的注释。
   用户名username输入 zhangsan' 空格--空格 ，密码password 随意。
   `select * from user where username ='zhangsan' -- ' and password ='kajajha'''` ;
   对上述sql语句进行说明：
	`-- ' and password ='kajajha''' ; --` 表示注释的意思，这样就会将密码都给注释掉了，就相当于只根据用户名zhangsan来查询了。

   <img src="https://article.biliimg.com/bfs/article/864c5aba50d74988789317dcf72e818641997b03.png" alt="image.png" style="zoom:60%;" />

3. PreparedStatement解决SQL注入方案
   1.获取PreparedStatement对象
   PreparedStatement是Statement的子接口，可以防止sql注入问题。可以通过Connection接口中的prepareStatement(sql)方法获得PreparedStatement的对象。

   方法如下所示：

   ```java
    PreparedStatement	prepareStatement(String sql)  创建一个 PreparedStatement 对象来将参数化的 SQL 语句发送到数据库。
   ```

   注意：sql提前创建好的。sql语句中需要参数。使用？进行占位

   `select *from user where username=? and password = ?;`

   **操作步骤**

   - 步骤一：`PreparedStatement pstmt = conn.prepareStatement(sql);` -----需要你事先传递sql模板。如果sql需要参数，使用？进行占位。
   - 步骤二：设置参数（执行sql之前）：`pstmt.setXXX(int index, 要放入的值)` -----根据不同类型的数据进行方法的选择。第一个参数index表示的是？出现的位置。从1开始计数，有几个问号，就需要传递几个参数。

   **方法的参数说明：**

   第一个参数：int index ;表示的是问号出现的位置。 问号是从1开始计数
   第二个参数：给问号的位置传入的值。
   步骤三、执行，不需要在传递sql了。

     `pstmt.executeQuery();---执行select`
     `pstmt.executeUpdate();`---执行insert，delete，update

4. **PreparedStatement** **原理**

   - PreparedStatement 好处：
     1.预编译SQL，性能更高
     2.防止SQL注入：将==敏感字符进行转义==
   - PreparedStatement 原理：
     1.在获取PreparedStatement对象时，将sql语句发送给mysql服务器进行检查，编译（这些步骤很耗时）
     2.执行时就不用再进行这些步骤了，速度更快
     如果sql模板一样，则只需要进行一次检查、编译

     ![image-20230329114154688](https://article.biliimg.com/bfs/article/91c94e464d63a2f5e59f1da2e194cd72917c4037.png)

   

   **修改案例**

   ```java
   public class JDBCTest04 {
     public static void main(String[] args) throws SQLException {
      /*
      需求:模拟登录，键盘输入用户名和密码完成登录功能
      */
       //0.键盘录入数据
       Scanner sc = new Scanner(System.in);
       System.out.println("请输入用户名");
       String inputname = sc.nextLine();
       System.out.println("请输入密码");
       String inputpassword = sc.nextLine();
       //1.注册驱动
       //2.获取和数据库的连接
       String url="jdbc:mysql:///day03-1";
       String user="root";
       String password="701121";
       Connection conn = DriverManager.getConnection(url, user, password);
       //设置sql语句的模板
       String sql="select *from user2 where username=? and password=?";//?表示占位符
       //3.获取发送sqL语句对象
       PreparedStatement statement = conn.prepareStatement(sql);
       //4.发送sqL语句
       statement.setString(1,inputname);
       statement.setString(2,inputpassword);
       ResultSet res = statement.executeQuery();
       //5.处理结果集
       if(res.next())
       {
         System.out.println("登录成功");
       }
       else
       {
         System.out.println("登录失败，请重新输入用户名或者密码！");
       }
       //6.释放资源
       res.close();
       statement.close();
       conn.close();
     }
   }
   ```


## 数据库连接池

**没有连接池的现状**

首先我们通过画图的形式来分析一下我们目前所学的jdbc程序的结构。

<img src="https://article.biliimg.com/bfs/article/c1dea343f9492fe4eac8b9aba0f59159af0082a3.png" alt="image.png" style="zoom:70%;" />

**数据库连接池简介**

- 数据库连接池是个容器，负责分配、管理数据库连接(Connection)
- 它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个；
- 释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏

**好处：**

- 资源重用
- 提升系统响应速度
- 避免数据库连接遗漏

<img src="https://article.biliimg.com/bfs/article/0e3e4bdb4e6ab4887584c4f3ffee5dd300dea86b.png" alt="image-20230329153846702" style="zoom:50%;" />

**常用连接池的介绍**

`javax.sql.DataSource`表示数据库**连接池**，DataSource本身只是Sun公司提供的一个接口,没有具体的实现，它的实现由连接池的数据库厂商去实现。我们只需要学习这个工具如何使用即可。

该接口如下：

```java
public interface DataSource {
	Connection getConnection();
}
```

常用的连接池实现组件有以下这些：

1. **阿里巴巴-德鲁伊Druid连接池**：Druid是阿里巴巴开源平台上的一个项目，整个项目由数据库连接池、插件框架和SQL解析器组成。该项目主要是为了扩展JDBC的一些限制，可以让程序员实现一些特殊的需求。
2. **`C3P0`是一个开源的JDBC连接池，支持JDBC3规范和JDBC2的标准扩展。目前使用它的开源项目有Hibernate，Spring等。C3P0有自动回收空闲连接功能。**
3. `DBCP(DataBase Connection Pool)`数据库连接池，是Apache上的一个Java连接池项目。dbcp没有自动回收空闲连接的功能。

### Druid
**DRUID简介**

​	Druid是阿里巴巴开发的号称为监控而生的数据库连接池(可以监控访问数据库的性能)，Druid是目前最好的数据库连接池。在功能、性能、扩展性方面，都超过其他数据库连接池。Druid已经在阿里巴巴部署了超过600个应用，经过一年多生产环境大规模部署的严苛考验。如：一年一度的双十一活动，每年春运的抢火车票。

**Druid常用的配置参数**

|       **url**       | **数据库**连接字符串**jdbc:mysql://localhost:3306/数据库名** |
| :-----------------: | :----------------------------------------------------------: |
|    **username**     |                      **数据库的用户名**                      |
|    **password**     |                       **数据库的密码**                       |
| **driverClassName** | **驱动类名。根据url自动识别，这一项可配可不配，如果不配置druid会根据url自动识别数据库的类型，然后选择相应的数据库驱动名** |
|   **initialSize**   | **初始化时建立的物理连接的个数。初始化发生在显式调用init方法，或者第一次获取连接对象时** |
|    **maxActive**    |                    **连接池中最大连接数**                    |
|     **maxWait**     |           **获取连接时最长等待时间，单位是毫秒。**           |

**Druid连接池基本使用**

**API介绍**

核心类：**DruidDataSourceFactory**

获取数据源的方法：使用**com.alibaba.druid.pool.DruidDataSourceFactory**类中的静态方法：

```java
public static javax.sql.DataSource createDataSource(Properties properties)
创建一个连接池，连接池的参数使用properties中的数据
```

配置信息在properties属性对象中。

我们可以看到Druid连接池在创建的时候需要一个Properties对象来设置参数，所以我们使用properties文件来保存对应的参数。

Druid连接池的配置文件名称随便，放到src目录或者项目根目录下面加载
`druid.properties`文件内容：

```properties
# 数据库连接参数
url=jdbc:mysql://localhost:3306/day05_db
username=root
password=123
driverClassName=com.mysql.jdbc.Driver
```

**案例代码**

```java
public class Demo03 {
    public static void main(String[] args) throws Exception {
      //加载properties文件的内容到Properties对象中
        Properties info = new Properties();
        //加载项目下的属性文件 相对项目根目录
//        FileInputStream fis = new FileInputStream("druid.properties");
        //相对src目录
        InputStream fis = Test01.class.getClassLoader().getResourceAsStream("druid.properties");
        //从输入流中加载属性
        info.load(fis);
        System.out.println(info);
        //创建DRUID连接池，使用配置文件中的参数
        DataSource dataSource = DruidDataSourceFactory.createDataSource(info);
        //从DRUID连接池中取出连接
        //Connection conn = dataSource.getConnection();
        //System.out.println("conn = " + conn);
// 需求: 根据用户名和密码 查询用户信息
        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            // 获得连接
            conn = dataSource.getConnection();
            // 获得发送sql的对象
            String sql = "select * from emp where name=? and city=?";
            pstmt = conn.prepareStatement(sql);
            // 如果有问号,需要 设置参数,注意:下标从1开始
            pstmt.setString(1, "刘备");
            pstmt.setString(2, "北京");
            // 执行sql 获得结果
            rs = pstmt.executeQuery();
            // 处理结果
            if (rs.next()) {
                int id = rs.getInt("id");
                String name = rs.getString("name");
                String city = rs.getString("city");
                System.out.println(id + ":::" + name + "===" + city);
            } else {
                System.out.println("没有查到对应的用户信息!");
            }
        } catch (Exception e) {
        } finally {
//            JDBCUtils.release(conn, pstmt, rs);
        }
    }
}
```



```
mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '7011' WITH GRANT OPTION;
```


