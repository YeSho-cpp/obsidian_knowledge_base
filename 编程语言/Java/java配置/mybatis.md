## MyBatis介绍
**什么是MyBatis**
- MyBatis 是一款优秀的持久层框架，用于简化 JDBC 开发 ^10c29d
- MyBatis 本是 Apache 的一个开源项目iBatis, 2010年这个项目由apache software -foundation 迁移到了google code，并且改名为MyBatis 。2013年11月迁移到Github ^2077cb
- 官网：[mybatis – MyBatis 3 | 简介](https://mybatis.org/mybatis-3/zh/index.html)
**持久层**
- 负责将数据到保存到数据库的那一层代码
- JavaEE三层架构：表现层、业务层、<font color=#ff0000>持久层</font>
**框架**
- 框架就是一个半成品软件，是一套可重用的、通用的、软件基础代码模型
- 在框架的基础之上构建软件编写更加高效、规范、通用、可扩展
**mybatis框架整体架构**
![image.png](https://article.biliimg.com/bfs/article/f8e0f610f850420e688fc08f027261490d7466aa.png)


**MyBatis的ORM(Object Relational Mapping 对象关系映射)方式**

![image.png](https://article.biliimg.com/bfs/article/eb61d62d3d4bfc823fd17f2796dfe3fa87272057.png)

Mybatis有两种映射方式:
1. 通过XML映射;
2. 通过注解;

## MyBatis 快速入门 Mapper 代理开发
**入门案例：环境的搭建和代码实现**
### 创建maven项目
### 在pom.xml文件中导入依赖
```xml
<dependencies>  
    <!--mybatis核心包-->  
    <dependency>  
        <groupId>org.mybatis</groupId>  
        <artifactId>mybatis</artifactId>  
        <version>3.5.0</version>  
    </dependency>  
    <!--logback日志包-->  
    <dependency>  
        <groupId>org.slf4j</groupId>  
        <artifactId>slf4j-api</artifactId>  
        <version>1.7.26</version>  
    </dependency>  
    <dependency>        <groupId>ch.qos.logback</groupId>  
        <artifactId>logback-core</artifactId>  
        <version>1.2.3</version>  
    </dependency>  
    <dependency>        <groupId>ch.qos.logback</groupId>  
        <artifactId>logback-classic</artifactId>  
        <version>1.2.3</version>  
    </dependency>  
    <!--mysql驱动-->  
    <dependency>  
        <groupId>mysql</groupId>  
        <artifactId>mysql-connector-java</artifactId>  
        <version>8.0.32</version>  
    </dependency>  
  
    <dependency>        
    <groupId>junit</groupId>  
        <artifactId>junit</artifactId>  
        <version>4.10</version>  
        <scope>test</scope>  
    </dependency>  
</dependencies>
```
在mybatis中一个会话相当于一次访问数据库的过程，**一个会话对象类似于一个Connection连接对象**。

1.  **SqlSessionFactoryBuilder**：这是一个临时对象，用完就不需要了。通过这个工厂建造类来创建一个会话工厂。
    
2.  **SqlSessionFactory**：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对象即可。通过会话工厂对象来创建会话对象。
    
3.  **SqlSession**： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后会话需要关闭。
编写流程：Resources工具类直接可以读取src目录下配置文件，转成输入流。
![](https://article.biliimg.com/bfs/article/b995864c36f164a76211d2dcc12c97fc96ae3416.png)

```java
public class MyBatisTest {  
  @Test  
  public void queryAllusers() throws Exception {  
    //1.创建mybatis会话工厂创造类对象  
    SqlSessionFactoryBuilder builder=new SqlSessionFactoryBuilder();  
    //2.使用会话工厂创造类对象builder调用方法加载核心配置文件  
//    InputStream fis = MyBatisTest.class.getClassLoader().getResourceAsStream("mybatis-config.xml");  
    InputStream is = Resources.getResourceAsStream("mybatis-config.xml");  
    SqlSessionFactory factory = builder.build(is);  
    //3.根据会话工厂对象调用方法获取会话对象  
    SqlSession sqlSession = factory.openSession(); //sqlSession底层封装的是Connection  
    //4.使用会话对象调用SqLSession类中的方法获取接口UserMapper的代理对象  
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
    //5.使用接口代理对象调用接口的查询方法  
    List<User> list = mapper.queryAllusers();  
    //7.打印集合  
    System.out.println("list="+list);  
  }  
}
```
### MyBatis 配置
**注意**

mybatis配置文件分两种

1. 核心配置文件：`mybatis-config.xml` 配置连接数据库参数
2. 映射文件：`UserMapper.xml`编写SQL语句
![image-20211103190702567](https://article.biliimg.com/bfs/article/6fd7bf8570b260145b81d5603100c0c7761717ed.png)
### mybatis执行流程分析
![image.png](https://article.biliimg.com/bfs/article/4e67fe29e858865e069a58592b2f0e1100b8143f.png)

说明：

1. 第一步：是从核心配置文件`mybatis-config.xml`中构建`SqlSessionFactory`对象，由于核心配置文件`mybatis-config.xml`中==关联了映射文件UserMapper.xml==,所以在SqlSessionFactory中也存在映射文件的内容

2. 第二步：是从SqlSessionFactory中获取SqlSession会话对象，其实SqlSession会话对象底层封装的就是conn连接对象

3. 第三步：是通过SqlSession会话对象调用查询方法selectList然后根据参数找到映射文件中中的sql语句并将数据封装到==pojo的User对象中==

## MyBatis 核心配置文件
### Mybatis全局配置介绍
mybatis-config.xml，是MyBatis的全局配置文件，包含全局配置信息，如数据库连接参数、插件等。整个框架中只需要一个即可。 
参考:https://mybatis.org/mybatis-3/zh/configuration.html
![image.png](https://article.biliimg.com/bfs/article/68f62ef50b94061e78f80195c742f905f8b651ee.png)
说明:上述标签在实际使用过程中,要严格遵循使用顺序,否则报错;
### properties(属性)
1. 作用：
	加载外部的java资源文件（properties文件）
2. 使用`<properties>`标签的方式
	通过`properties`标签` resource`属性引入加载外部properties文件;
3. 使用`${key}`获取设置的属性值；
	核心配置文件通过`<properties>`表中的resource属性引入外部jdbc.properties文件;
	第一步:创建jdbc.properties配置文件:
	
```properties
driver=com.mysql.cj.jdbc.Driver  
url=jdbc:mysql:///${table}$  
username=root  
password=701121
```

第二步:引入外部的配置文件

`<properties resource="db.properties"/>`

第三步:加载value值 

```xml
<dataSource type="POOLED">
<property name="driver" value="${jdbc.driverClass}"/>
<property name="url" value="${jdbc.url}"/>
<property name="username" value="${jdbc.userName}"/>
<property name="password" value="${jdbc.password}"/>
</dataSource>
```

### settings(设置)
settinngs是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为。
![image.png](https://article.biliimg.com/bfs/article/9219141b02fb2f85d9e64063250bc5b484987bc6.png)
settings参数有很多，我们先学习驼峰匹配`mapUnderscoreToCamelCase`
#列名属性不一致 
1.开启驼峰自动映射的配置和作用?
	1)配置
	```xml
	<setttings>
	        <setting name="mapUnderscoreToCamelCase" value="true"/>
	</setttings>
	```
	2)作用
	自动将表中字段比如:`user_name` 映射到pojo属性:`userName`,
	无需给sql中字段取别名;
### typeAliases(类型别名)
- 作用

	类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置，意在降低冗余的全限定类名书写。例如：

	```xml
	<typeAliases>
	  <typeAlias alias="Author" type="domain.blog.Author"/>
	  <typeAlias alias="Blog" type="domain.blog.Blog"/>
	  <typeAlias alias="Comment" type="domain.blog.Comment"/>
	  <typeAlias alias="Post" type="domain.blog.Post"/>
	  <typeAlias alias="Section" type="domain.blog.Section"/>
	  <typeAlias alias="Tag" type="domain.blog.Tag"/>
	</typeAliases>
	```

	类型别名是给类的全限定名称(包名.类名) 取一个短名称。存在的意义仅在于用来减少类完全限定名的冗余。例如：

	![image.png](https://article.biliimg.com/bfs/article/72d8ef9842ea596487695472cd23694348aa4bbd.png)

- 应用

	- package

		扫描指定包下的所有类，扫描之后的别名就是类名，大小写不敏感（不区分大小写），建议使用的时候和类名一致。

		设置别名：
		```xml
		<!--
		        三、typeAliases（类型别名）
		            【1】作用：给类的全限定名称 取一个短名称   com.heima.mybatis.pojo.User==>User
		            【2】用法：
		                1、单独取别名：<typeAlias type="com.heima.mybatis.pojo.User" alias="User"/>
		                2、批量取别名：<package name="com.heima.mybatis.pojo"/> 扫描到当前包下的所有类
		                              类的类名==》别名
		    -->
		    <typeAliases>
		      
		        <!--扫描com.itheima.sh.pojo包下所有的类，类名直接作为别名(别名不区分大小写)-->
		        <package name="com.itheima.sh.pojo"/>
		    </typeAliases>
		```

		使用别名：
		![image.png](https://article.biliimg.com/bfs/article/fd3bacc3dd3094349ef276fed34eb89791603e97.png)

	【内置别名】

	​	这是一些为常见的 Java 类型内建的相应的类型别名。它们都是不区分大小写的，注意对基本类型名称重复采取的特殊命名风格。
	

|别名  |映射的类型 |
| :--------: | :--------: |
|   `_byte`    |    byte    |
|   `_long`    |    long    |
|   `_short`   |   short    |
|  |  |
|    `_int`    |    int     |
|  `_integer`  |    int     |
|  `_double`   |   double   |
|   `_float`   |   float    |
|  `_boolean`  |  boolean   |
|`string`|   String   |
|    `byte`    |    Byte    |
|   ` long`    |    Long    |
|   `short`    |   Short    |
|    `int`     |  Integer   |
|  `integer `  |  Integer   |
|   `double `  |   Double   |
|   `float`    |   Float    |
|  `boolean`   |  Boolean   |
|    `date`    |    Date    |
|  `decimal`   | BigDecimal |
|` bigdecimal` | BigDecimal |
|   `object`   |   Object   |
|    `map`     |    Map     |
|  `hashmap`   |  HashMap   |
|   ` list`    |    List    |
| `arraylist`  | ArrayList  |
| `collection` | Collection |
|  `iterator`  |  Iterator  |

【代码演示】

```xml
<!--parameterType="int" 表示sql语句参数id的类型，int是Integer的别名-->
	<select id="queryById" resultType="user" parameterType="int">
		select * from user where id = #{id}
	</select>
```

### typeHandlers(类型处理器)

​	无论是 MyBatis 在预处理语句（PreparedStatement）中设置一个参数时，还是从结果集中取出一个值时， 都会用类型处理器将获取的值以合适的方式转换成 Java 类型。下表描述了一些默认的类型处理器。

![image.png](https://article.biliimg.com/bfs/article/fae2f6ce636d95fa9484a16f957a2fbfbfedaea3.png)


~~~xml
<!--
        四、typeHandlers（类型处理器）
            数据库数据类型：varchar    ===StringTypeHandler===> 实体类：String
            数据库数据类型：double   DoubleTypeHandler  实体类中的数据： java.lang.Double 
-->
~~~

### environments（环境配置） 

```html
	MyBatis 可以配置成适应多种环境，例如，开发、测试和生产环境需要有不同的配置；
尽管可以配置多个环境，每个 SqlSessionFactory 实例只能选择其一。
虽然，这种方式也可以做到很方便的分离多个环境，但是实际使用场景下，我们更多的是选择使用spring来管理数据源，来做到环境的分离。
```


```xml
      父标签： environments（环境配置）
			子标签：
                environment（环境变量）
                transactionManager（事务管理器）
                dataSource（数据源） 
```



1. 默认环境设置

	第一步：在environments标签中配置多个environment，通过属性default指定一个默认环境配置；
	![](https://article.biliimg.com/bfs/article/979ac473b337c370536e33b0c54e78d203b23fa5.png)

	第二步：在构建SqlSessionFactory时，可指定具体环境，如果不指定就使用默认的环境配置；

	![](https://article.biliimg.com/bfs/article/65758881e50c1fec11441926000601c5e5a5189d.png)

2. 指定环境设置

	第一步：在environments中配置多个环境

	![](https://article.biliimg.com/bfs/article/979ac473b337c370536e33b0c54e78d203b23fa5.png)



	第二步：在构建SqlSessionFactory时，通过environment的id指定环境
	
	![](https://article.biliimg.com/bfs/article/82e2bbd686d302ee3fd5f728baa055a525068f12.png)

### mappers(映射器)
Mappers标签作用：提供了关联加载xml映射文件的配置功能
**使用方式**：

1、加载XML映射文件，关联UserMapper.java接口
	​【1】`<mapper resource="UserMapper.xml"/>` 从==resources相对目录==下加载映射文件；
​     说明:如果项目采用基于xml的开发模式,建议使用方式1开发;
2、加载接口，关联映射文件
	条件：1、<font color=#ff0000>接口名和映射文件名保持一致</font>；2、<font color=#ff0000>路径保持一致</font>；
	【2】批量加载class：`<package name="com.heima.mybatis.dao"/>`
	说明:如果基于注解开发的开发的话,推荐使用方式2开发
```xml
 <mappers>  
        <!--加载映射文件,放到src下即可-->  
        <mapper resource="sh/dao/userMapper.xml"/>  
        </mappers>
```

```xml
 <mappers>  
 
<!-- 加载映射文件 sh.dao表示UserMapper接口所在的包-->  
	<package name="sh.dao"/>  
</mappers>
```

## MyBatis映射文件配置
Mapper映射文件中定义了操作数据库的sql,每一个sql都被包含在一个statement中。映射文件是mybatis操作数据库的核心。
SQL 映射文件只有很少的几个顶级元素（按照应被定义的顺序列出）： 
![image.png](https://article.biliimg.com/bfs/article/111f218d848f65667bfb0178b5bd1683330d3665.png)
说明：
	映射文件中需要直接书写SQL语句对数据库进行操作，对数据库操作SQL语句主要有CRUD这四类。
	这四类对应到映射文件中的配置为四类标签：`select`，`insert`,`update`，`delete` 。
#### select标签
	select标签属性:

|         属性名         |                   说明                    | 是否必须 |
| :--------------------: | :---------------------------------------: | :------: |
|         ` id`          | 这条SQL语句的唯一标识，和接口的方法名一致 |    是    |
|    ` parameterType`    |                 入参类型                  |    否    |
| `resultType/resultMap` |                返回值类型                 |    是    |


练习1:查询id是1的用户信息;
	1)定义接口方法
	//根据id查询
	`User queryById(Integer id);`
	2)xml文件sql绑定接口方法
```xml
	<select id="queryById" resultType= "user" parameterType= "int“ >
    select * from user where id = #{id}
	</select>
```
#### insert标签
属性:

| 属性  |                   说明                    | 是否必须 |
| :---: | :---------------------------------------: | :------: |
| `id ` | 这条SQL语句的唯一标识，和接口的方法名一致 |    是    |

练习：向数据库添加用户
	1)定义接口方法：
	`void addUser(User user);`
```xml
<insert id="addUser" parameterType="user">
        insert into user values(null,#{userName},#{birthday},#{sex},#{address});
</insert>
```

说明:`#{username},#{birthday},#{sex},#{address}` 大括号里面的值必须和pojo的实体类User类中的属性名一致，否则会报错,等号左边user_name是字段名，大括号中的userName是User实体类成员变量名
注意事项:Mybatis默认事务==手动提交==,可设置事务自动提交:

#### update标签

| 属性  |                   说明                    | 是否必须 |
| :---: | :---------------------------------------: | :------: |
| `id ` | 这条SQL语句的唯一标识，和接口的方法名一致 |    是    |

1)定义接口方法
	`void updateUser(User user);`
2)绑定映射文件
```xml
<update id="updateUser">
        update user set user_name=#{userName},sex=#{sex} where id=#{id}
</update>
```

#### delete标签

| 属性  |说明| 是否必须 |
| :---: | :---------------------------------------: | :------: |
| `id ` |这条SQL语句的唯一标识，和接口的方法名一致|    是    |

1)定义接口方法
	`void deleteById(Integer id);`
2)映射文件绑定接口方法
```xml
<delete id="deleteUser">
        delete from user where id=#{id}
</delete>
```

### Mybatis入参&参数获取&出参

#### Mybatis入参
**parameterType**
CRUD标签都有一个属性`parameterType`，底层的`statement`通过它指定接收的参数类型。入参数据有以下几种类型：`HashMap`，基本数据类型（包装类），实体类；
说明：
	设置传入这条语句的参数类的**完全限定名**或**别名**。
	这个属性是可选的，因为 MyBatis 可以通过类型处理器（`TypeHandler`） 推断出具体传入语句的参数类型。
##### 1.入参是单个参数
	单个参数：接口方法传入一个参数
举例：
	【接口传参】
	`User queryById(Integer id);`
	【接收参数】
```xml
	<!--根据id查询-->
	<select id="queryById" resultType="User" parameterType="int">
	select *,user_name AS  userName from user where id = #{id}
	</select>
```
在xml中可通过`#{任意变量名}`都可以接收到参数；如果接口传入的是单个参数,可以在xml中使用**任意变量**去接收,但是不建议乱写,最好见名知意;

##### 2.入参是多个参数

练习:根据用户名和性别查询用户
【接口传参】
	`List<User> findUsersByUserNameAndSex(String name,String sex);`
【xml与接口绑定】

```xml
<select id="findUsersByUserNameAndSex" resultType="user">
select * from user where user_name=#{name} and sex=#{sex}
</select>
```
【测试结果】
![image.png](https://article.biliimg.com/bfs/article/1f2f762314c436c2035503ba75040970076db33a.png)

【解决方案】
方式1:使用参数索引获取：`arg0,arg1`(了解即可) 
```xml
<select id="findUsersByUserNameAndSex" resultType="user">
select * from user where user_name=#{arg0} and sex=#{arg1}
</select>
```
方式2:使用参数位置获取：`param1,param2`(了解即可) 
```xml
<select id="findUsersByUserNameAndSex" resultType="user">
select * from user where user_name=#{param1} and sex=#{param2}
</select>
```
方式3:使用命名注解@Param修饰接口方法的形参(掌握) 

`List<User> findUsersByUserNameAndSex(@Param("name") String name,@Param("sex") String sex);`

```xml
<select id="findUsersByUserNameAndSex" resultType="user">
select * from user where user_name=#{name} and sex=#{sex}
</select>
```

> 说明：
            1.mybatis中对于接口方法参数只有一个简单类型的时候，底层不会做任何处理，在获取的时候`#{任意标识符}`
            2.mybatis中对于接口方法参数有多个简单类型的时候，底层做了处理的，将传递的实参放到一个**map集合**中了，

##### 3.pojo参数【掌握】

说明：接口方法传入pojo类型的数据时，那么在映射文件中使用`#{}`获取实参的时候，大括号中书写**实体类的成员变量名**或者实体类中的`getXxx()`去掉的字符串名字
测试代码:
【接口】
	`void saveUser(User user);`
【映射文件】

```xml
<insert id="saveUser">
insert into user values(null,#{username},#{age},#{birthday},#{sex},#{address})
</insert>
```

##### 4.入参是Map类型

说明：接口方法传入Map类型的数据时，xml中使用#{map中key}可直接获取map中的value值；
练习：根据用户名和性别查询用户信息，入参为Map集合，泛型都是String类型分别表示用户名和性别。
【接口:】
	`List<User> QueryByNameAndSex(Map map);`
【映射文件:】

```xml
<select id=“queryByNameAndSex" resultType="user" parameterType="map">
select * from user where user_name=#{name} and sex=#{sex}
</select>
```

**自增主键回填（了解）**
	需求:新增一条数据成功后，将这条数据的主键封装到实体类中，并**查看主键的**值。
	实现方式:使用insert标签的属性useGeneratedKeys，keyProperty，keyColumn实现; 
	参数说明:
	

|        属性        |                           说明                           |
| :----------------: | :------------------------------------------------------: |
|`useGeneratedKeys`| true  获取自动生成的主键，相当于select  last_insert_id() |
|    ` keyColumn`    |                      表中主键的列名                      |
|`keyProperty`|                   实体类中主键的属性名                   |


测试代码:
【接口:】
	`Integer addUserAndGetFkId2(User user);`
实现方式:使用insert标签的属性useGeneratedKeys，keyProperty，keyColumn实现
【映射文件:】

```xml
<insert id="addUserAndGetFkId2" useGeneratedKeys="true" keyColumn="id" keyProperty="id">
insert into user values(null,#{username},#{birthday},#{sex},#{address})
</insert>
```

#### 参数值的获取

- `#{}和${}两种获取参数方式`
	- 参数值的获取指的是statement获取接口方法中传入的参数。
	- 获取参数，有两种方式：`#{}`和`${}`；
1. `#{}`取值
	- 使用`#{}`的sql是进行==预编译==的,可以防止sql注入;
	- 预编译意味着它只对对参数进**预检查**，而非参数(**表名，字段名**)就会报错，无法检查就会报错
2. `${}`取值
	- ${id} 获取id值时，**必须**使用命名参数取值==@param==；
	- 如果是取单个值,也可使用`${value}`获取；
	- 参数值直接**拼接**到sql中,会有sql注入的风险；
- `${}`取值测试代码：
	【定义接口】
		`User findById2(@Param("id") Integer id);`
	【映射文件】

	```xml
	<select id="findById2" resultType="user" parameterType="int">
	select * from user where id=${id}
	</select>
	```
	【测试】
	```java
	@Test
	public void test12(){
		UserMapper mapper = MybatisUtils.getMapper(UserMapper.class);
		User user = mapper.findById2(1);
		System.out.println(user);
		MybatisUtils.close();
	}
	```

- `${}`取值的应用场景
	在一些特殊的应用场景中，需要对SQL语句部分（不是参数）进行拼接，这个时候就必须使用`${}`来进行拼接，不能使用`#{}`.例如：
	1、企业开发中随着数据量的增大，往往会将数据表按照年份进行分表，如：2017_user，2018_user....，对这些表进行查询就需要动态把年份传入进来，而年份是表名的一部分，并不是==参数==，JDBC无法对其预编译，所以只能使用`${}`进行拼接：  
	`	SELECT * FROM ${year}_user；`
	
	2、根据表名查询数据总记录数：
		`SELECT COUNT(*) FROM user`
		`SELECT COUNT(*) FROM order`
		`SELECT COUNT(*) FROM  ${tableName}`
	简言之：如果需要设置到SQL中的不是查询的条件，只能使用`${}`拼接；
	练习：根据输入的表名统计指定表下的总记录数;
	【定义接口方法】
		`Integer countByTableName(@Param("tableName") String tableName);`
	【绑定映射文件】

	```xml
	<select id="countByTableName" resultType="integer">
	select count(*) from ${tableName}
	</select>
	```

- `${}`取值注意事项 (了解)
	1.使用${变量}方式获取变量值时,不要与**全局的properties**下定义的参数名称冲突,否则数据注入错误;
	2.使用${变量}传入字符串类型时,需要自己维护字符串的上==引号==;

	举例：`${}`获取配置文件中的值
	```xml
	<!--根据用户名和性别查询-->
	<select id="queryByUserNameAndSex" resultType="User">
	SELECT * FROM  user WHERE  user_name = '${jdbc.user}' AND  sex = #{sex}
	</select>
	```

	`${}`获取单个值时，最好是通过命名参数的形式获取。

 **扩展**：
	 需求：模糊查询姓名中含有 喆 人信息
	 `List<User> queryUserByLikeUserName(@Param("userName") String userName);`
	说明：
		1.`select * from user where user_name like '%#{userName}%'` 这里不能是`#{}`获取实参，因为这里是拼接
		2.`select * from user where user_name like '%${userName}%'` 这里考虑是拼接必须是`${}`获取实参，但是`${}`还具有sql注入问题？
			`select * from user where user_name like '%喆%'`
		    此时可以使用mysql自带函数，拼接函数：concat(数值1,数值2,数值3,....) 可以将concat函数中的数据拼接为一个值
		    举例：`concat('%','锁哥','%')===》 结果是'%锁哥%'`
				`concat('%',#{userName},'%') ===>假设 #{userName}获取的值是 喆===》'%喆%'`
			在使用 mybatis 构造模糊查询语句时，应该使用 `%${user_name}%` 表达式来对 user_name 进行模糊匹配，而不是 `#{%user_name%}`。因为 `#{}` 表示的是参数占位符，而模糊查询中的通配符 `%` 应该与 user_name 一起作为查询条件的一部分传递给 SQL 执行语句，因此应该使用 `${}`。


#### Mybatis出参

##### 简单结果集映射
在使用原生的JDBC操作时，对于结果集ResultSet，需要手动处理;mybatis框架提供了resultType和resultMap来对结果集进行封装;
只要一个方法有返回值需要处理，那么 resultType和resultMap必须有一个;
1. `resultType`
	- 从sql语句中返回的期望类型的类的完全限定名或别名。 
	- 注意如果返回的是集合，那应该设置为集合包含的类型，而不是集合本身。
	- 可以使用 resultType 或 resultMap，但不能同时使用。
	- 返回值是简单类型
		- 例如 `int ,string ===>resultType=“`书写对应的简单类型别名或者全限定名即可
		- 基本类型 `int short double` ...    别名： `_`基本类型名称
		- 包装类 类 `String ArrayList` .... 别名：类名首字母小写
2. 返回值为一个pojo(User)对象时
	- resultType=“pojo类的全限定名即可
3. 返回值为一个`List<User>`时
	- 当返回值为List集合时，resultType需要设置成集合中存储的**具体的**pojo数据类型;

##### Map类型结果映射
1. 返回一条数据，封装到map中
	- 需求：查询id是1的数据，将查询的结果封装到`Map<String,Object>`中;
		- 定义接口
			`Map<String,Object> findMapById(@Param("id") Integer id);`
		- 定义映射文件配置
		```xml
		<select id="findMapById" resultType="map">
		select id,user_name as userName,address from user where id=#{id}
		</select>
		```


2. 返回多条数据，封装到map中
	- 需求:查询数据表所有的数据封装到Map<Integer,User>集合中
	- 要求:Key值为一条记录的主键，Value值为pojo的对象.
	- 说明：需要在接口的方法上使用**注解@MapKey**指定数据表中哪一列作为Map集合的key，否则mybatis不知道具体哪个列作为Map集合的key.
	- 定义接口
		`@MapKey("id")
		`Map<Integer,User> findAllToMap();
	- 定义映射文件配置
	```xml
	<select id="findAllToMap" resultType="map">
	select id,user_name as name,birthday,sex,address from user
	</select>
	```

##### resultMap映射(掌握)
正常开发中,数据库字段名称与Pojo类属性名称不一致时,一般通过驼峰映射或者As关键字取别名可以搞定,但是很多场景下,对于复杂的orm映射,上述的2种方式就不能适用了;
#列名属性不一致
resultMap是mybatis中最重要最强大的元素，使用ResultMap可以解决复杂映射问题：
	1. POJO属性名和表结构字段名不一致的问题（有些情况下也不是标准的驼峰格式，比如id和userId）
	2. 完成高级查询，比如说，一对一、一对多、多对多。

使用resultMap完成根据用户id查询用户信息的结果集的封装(`resultSet->JavaBean`）
- 步骤一：将驼峰匹配注释掉 
	一旦注释掉驼峰匹配，那么再通过findById查询的结果中，用户名就无法封装了，此时我们可以尝试使用ResultMap来解决这个问题。
	```xml
	<settings>
	<!--作用:表:user_name 类:userName/username 自动映射-->
	<setting name="mapUnderscoreToCamelCase" value="false"/>
	</settings>
	```
- 步骤二：配置resultMap
	resultMap标签的作用:自定义结果集，自行设置结果集的封装方式;

|属性名|描述|
|:-:|:-:|
|`id`|resultMap标签的唯一标识，不能重复，一般用来被引用|
|`type`| 结果集的封装类型 |
|`autoMapping`|操作单表时，不配置默认为true,如果pojo对象中的属性名称和表中字段名称相同，则自动映射。|

在映射文件中自定义结果集类型：
```xml
<!--type="user" 表示结果集的封装类型是user-->
<resultMap id="userMap" type="user" autoMapping="true">
<!--配置主键映射关系 一定书写-->
<id column="id" property="id"></id>
<!--配置用户名的映射关系  column 表示数据表列  property表示pojo的属性 可选并且多个-->
<result column="user_name" property="name"></result>
</resultMap>
```
- 步骤三：修改查询语句的statement
```xml
<select id="findById" resultMap="userMap">
select * from user where id=#{id}
</select>
```

## 动态SQL(掌握)

### 动态标签介绍
MyBatis 的强大特性之一便是它的**动态 SQL**。如果你有使用 [[Mysql#JDBC 简介|JDBC]] 或其它类似框架的经验，你就能体会到根据不同条件拼接 SQL 语句的痛苦。
场景
	查询男性用户，如果输入用户名，模糊查询，否则只查询男性用户  
	`List<User> queryUsersBySexOrUserName(@Param ( "userName") String userName); ` 
	![image.png](https://article.biliimg.com/bfs/article/3e3075c5cdd810a72feb5dac4d389259d8bdf94b.png)
**MyBatis 3** 开始精简了元素种类，现在只需学习原来一半的元素便可。MyBatis 采用功能强大的 `OGNL` 的表达式来淘汰其它大部分元素。 

> 了解:
**OGNL（ Object Graph Navigation Language ）对象图导航语言，这是一种强大的表达式语言，通过它可以非常方便的来操作对象属性**

动态SQL中的业务逻辑判断需要使用到以下运算符： `ognl`表达式

```
1.   e1 or e2 满足一个即可
2.   e1 and e2 都得满足
3.    e1 == e2,e1 eq e2 判断是否相等
4.    e1 != e2,e1 neq e2 不相等
5.    e1 lt e2：小于   lt表示less than 
6.    e1 lte e2：小于等于，其他gt（大于）,gte（大于等于） gt 表示greater than
7.    e1 in e2 
8.    e1 not in e2
9.    e1 + e2,e1 * e2,e1/e2,e1 - e2,e1%e2
10.   !e,not e：非，求反
11.   e.method(args)调用对象方法
12.   e.property对象属性值  user.userName
13.   e1[ e2 ]按索引取值，List,数组和Map
14.   @class@method(args)调用类的静态方法
15.   @class@field调用类的静态字段值
```

**Mybatis常见标签如下**
- `if`：判断
- `choose (when, otherwise)`：分支判断    
- `switch`:多选一
- `where`标签
- `set`标签
- `foreach`：循环遍历标签

### if标签
if标签格式:  

```xml
<if test= "ognl表达式>  
    sqL语句  
</if>  
```
**说明：**
	1）if标签：判断语句，用于进行逻辑判断的。如果判断条件为true，则执行if标签的文本内容
	2）test属性：用来编写表达式，支持ognl；
**需求:**
	查询男性用户，如果输入了用户名，按用户名模糊查询,如果没有输入用户名，就查询所有男性用户；
**代码：**
```xml
<select id="queryUsersBySexOrUserName" resultType="User" >  
    select *from day06_1.user where sex='男'  
    <if test="userName!=null and userName.trim()!=''">  
        and user_name like concat('%',#{userName},'%')  
    </if>  
</select>
<!-- ognl.表达式中的userName必须和注解@Param("userName")中的属性值userName一致，userName.trim(）去掉userName字符串的前后空格-->
```

### choose,when,otherwise
说明:
```xml
1.<choose>
	<when test="条件表达式1">
	sqL1
	</when>
	<when test="条件表达式2">
	sqL2
	</when>
	......
	<otherwise>
	sqL3
	</otherwise>
</choose>
```

- choose标签：分支选择（**多选一**，遇到成立的条件即停止）
- when子标签：编写条件，不管有多少个when条件，一旦其中一个条件成立，后面的when条件都不执行。
- test属性：编写ognl表达式
- otherwise子标签：当所有条件都不满足时，才会执行该条件。
**练习：**
	编写一个查询方法，设置两个参数，一个是用户名，一个是住址。
	根据用户名或者住址查询所有男性用户:
	如果输入了用户名则按照用户名模糊查找，否则就按照住址查找，两个条件只能成立一个，如果都不输入就查找用户名为“孙悟空”的用户。
**代码：**
```xml
<select id="queryUersBySexAndadress" resultType="User">
        select *from day06_1.user where sex='男'
        <choose>
            <when test="name!=null and name.trim()!=''">
                and user_name like concat('%',#{name},'%')
            </when>
            <when test="address!=null">
                and address like concat('%',#{address},'%')
            </when>
            <otherwise>
                and user_name='孙悟空'
            </otherwise>
        </choose>
</select>
```

### where标签

**需求**：
      如果输入了用户名按照用户名进行查询如果输入住址,按住址进行查询,
      如果两者都输入,两个条件都要成立。
      两者都不输入就查询所有数据

`List<User> queryUsersByUserNameAndAddress(@Param("userName") String userWame,Param("adress")String address);`
如果不使用where标签会有两个问题:
1.如果都不满足条件，sql语句是这样的:select * from user where有问题
2.如果用户名不满足，但是地址满足，那么sqL语句是这样的: 
`seLect * from user where and address=#{address}:`where和and不能直接相邻
解决问题:使用where动态标签
where标签：拼接多条件查询时 
     1、能够**添加where**关键字； 
     2、能够**去除多余的and或者or**关键字；

**代码：**
```xml
<select id="queryUsersByUserNameAndAddress" resultType="User">
        select *from day06_1.user
        <where>
            <if test="name!=null and name.trim()!=''">
                user_name=#{name}
            </if>
            <if test="address!=null and address.trim()!=''">
                and address=#{address}
            </if>
        </where>
</select>
```

### set标签

**案例:**
修改用户信息，如果参数user中的某个属性为null，则不修改。
	`void updateUserById(User user);`
说明:
1.set标签作用:
	1）添加set关键字
	2)去掉多余的逗号
2.set标签只能使用在update位置
3.由于接口方法void updateUserById (User user);形参是User复杂类型所以在ognl表达式中使用的变量都是
  User实体类的成员变量名:
`<if test="userName !=nuLl"></if>` ==== userName就是User实体类中的成员变量private String userName;

代码：
```xml
<update id="queryUpdate" useGeneratedKeys="true">  
    update day06_1.user  
    <set>  
        <if test="userName!=null">  
            user_name=#{userName},  
        </if>  
        <if test="birthday!=null">  
            birthday=#{birthday},  
        </if>  
        <if test="sex!=null">  
            sex=#{sex},  
        </if>  
        <if test="address!=null">  
            address=#{address}  
        </if>  
    </set>  
    where id=#{id}  
</update>
```

### foreach标签
需求:
	按照id值是1，2,3来查询用户数据;

```java
List<User> queryUsersByIds (@Param("listIds") List<Integer>listIds );
        select* from user where id in(1,2,3)
```

说明：
1.foreach标签用来遍历数组或者集合，格式如下:

```xml
<foreach collection="数组或者集合名” separator="数据分隔符” item="保存集合或者数组的每个元素的变量名” open="以什么开始“,close=”以什么结束”>
</foreach>
```

举例:

```xml
	<!--根据多个id值查询-->
    <select id="queryByIds" resultType="user">
        SELECT * FROM  user WHERE id IN
        <foreach collection="arrIds" item="ID" separator="," open="(" close=")">
            #{ID}
        </foreach>
    </select>
```


```java  
    @Test
    public void queryByIds() {
        Integer[] arrIds = {1,2,3};
        List<User> userList = userMapper.queryByIds(arrIds);
        System.out.println("userList = " + userList);
    }
```





## mybatis高级查询(掌握)
### mybatis高级查询 

> 表与表的关系:
> 	一对一: ab两表的关系,由任意一张表维护(外键) 比如:b表维护ab的管理,那么在b表中创建一个字段aid;
> 	一对多:ab两张表 比如:从a看是一个a对应b的多条数据,但是从b看是一个b只能对应一个a的数据;
> 	多对多:ab两张表 比如:从a看是一个a对应b的多条数据,同时从b看是一个b对应a表的多条数据;

例子：
	![image.png](https://article.biliimg.com/bfs/article/384b456403ff713f3b4237f827880904a5013b8e.png)


### 一对一查询
**一对一映射语法格式:** 

```xml
<resultMap id=“映射ID” type=“主表实体名称” autoMapping=“true” >
<!-- 添加主表与主表实体映射 -->
<!--association：配置关联对象（User）的映射关系,一般与resultMap标签联合使用 -->
<association property="主表实体中对应从表的属性名称" javaType="从表实体类型" autoMapping="true">
<!-- 添加从表中字段与实体属性映射关系 -->
</association>
</resultMap>
```
需求：通过订单编号20140921003查询出订单信息，并查询出下单人信息。
说明:一个订单只能对应一个用户信息;
- 需求分析:
	```sql
	-- 方式1:分步查询
	-- 1.1根据订单编号查询订单信息
	select * from tb_order where order_number='20140921003';-- user_id:1
	-- 1.2根据user_id=1查询下单人信息
	 select * from tb_user where id=1;
	-- 方式2:关联查询
	select tb_order.id as order_id,tb_order.order_number,tb_user.* 
	from tb_order,tb_user 
	where 
	tb_user.id=tb_order.user_id 
	and tb_order.order_number='20140921003';`
	```

- 订单实体添加属性映射
	```java
	public class Order {
	private Integer id;
	private String orderNumber;
	private User orderUser;//定义成员变量保存用户信息
	//getter  setter toString
		}
	```

- 添加order接口及方法
	```java
	public interface OrderMapper {
		/**
		* 根据订单编号查询订单信息,包含下单人信息
		* @param orderNumber
		* @return
		*/
		Order findOrderByOrderNumber(@Param("orderNumber") String orderNumber);
			}	
	```

- 创建order映射文件,编写SQL
	```xml
	 <!--
        实现一对一查询
        1.resultMap标签实现多表查询
        2.通过id="queryOneToOneResultMap"的属性值关联下面的sql语句，和下面select标签的resultMap="queryOneToOneResultMap"属性值一致
        3.type="Order" ： 表示接口方法 Order queryOneToOne(@Param("orderNumber") String orderNumber);返回值类型
        4.多表查询一定书写autoMapping="true"
    -->
    <resultMap id="queryOneToOneResultMap" type="Order" autoMapping="true">
        <!--        tb_order表和Order实体类-->
        <!--
            5.配置tb_order的主键字段和Order实体类主键的成员变量关系
            <id column="tb_order表主键对应的字段名" property="Order实体类中主键对应的成员变量名"/>
        -->
        <id column="oid" property="id"/>
        <!--
            6.通过分析，下面的sql语句查询的结果中的tb_order和tb_user表属于一对一关系，在mybatis中一对一关系使用标签：
             <association property="实体类关联的对象名即实体类中的成员变量名" autoMapping="true" javaType="关联对象所属类型"></association>
              private User user;
              举例：
                1） property="user"：属性值user表示在Order实体类中保存用户信息的对象名
                2） javaType="User"：表示Order实体类的成员变量即上述的user对象所属类型是User类型
                3） autoMapping="true" ：表示如果tb_user表的字段名和User实体类成员变量名一致那么自动封装数据，多表必须书写
        -->
        <association property="user" autoMapping="true" javaType="User">
            <!--tb_user表和User实体类-->
            <!--
               5.配置tb_user的主键字段和User实体类主键的成员变量关系
               <id column="tb_order表主键对应的字段名" property="Order实体类中主键对应的成员变量名"/>
                <id column="id" property="id"/>
                    column="id":tb_user表的主键字段名id
                    property="id" :User实体类中封装tb_user表主键的成员变量名
           -->
            <id column="id" property="id"/>
            <!--配置tb_user表其他字段-->
            <result column="password" property="password"/>
        </association>
    </resultMap>
    <!--
        查询语句
        id: 接口中方法的名字
        resultType：返回的实体类的类型，类全名
         //需求1：通过订单编号20140921003查询出订单信息，并查询出下单人信息。
        Order queryOneToOne(@Param("orderNumber") String orderNumber);
    -->
    <select id="queryOneToOne" resultMap="queryOneToOneResultMap">
        select o.id oid,o.order_number,u.*
                from tb_order o
                inner join tb_user u
                on o.user_id = u.id
        where o.order_number=#{orderNumber};
    </select>
	```

一对一查询总结:

![image.png](https://article.biliimg.com/bfs/article/77db5447724fffa949c360ee113f5d6de3f4dedb.png)


### 一对多查询
```xml
	<!--一对多映射-->
	<resultMap id="唯一标识" type="映射的类" autoMapping="true">
		<id column="主键字段" property="映射类中属性名称"/>
		<result column="非主键字段" property="映射类中属性名称"/>
		<!--一对一映射-->
		<collection property="映射的类中属性名称" javaType="list" ofType="集合泛型" autoMapping="true">
			<id column="主键字段" property="java类型中属性名称"/>
			<result column="非主键字段" property="java类型中属性名称"/>
		</collection>
	</resultMap>

```
需求:查询id为1的用户及其订单信息
	分析：
	一个用户可以有多个订单。
	一个订单只能属于一个用户。
	用户(1)-----订单(n)
1.User实体添加映射关系
```java
	public class User implements Serializable{
	private List<Order> orders;
	private Long id;
	// 用户名
	private String userName;
	// 密码
	private String password;
	// 姓名
	private String name;
	// 年龄    private Integer age;
	//0-女 1-男
	private Integer sex;
	// getter  and setter and toString
}
```
2.编写接口
```java
	User findUserAndOrdersByUserId(@Param("id") Long id);
```
3.编写sql映射文件关联订单集合
```xml
	<!--定义一对多的映射规则-->
	<resultMap id="userMap" type="user" autoMapping="true">
		<!--映射主表-->
		<id column="id" property="id"/>
		<result column="user_name" property="userName"/>
		<!--配置一对多映射-->
		<!--
            6.在mybatis中配置一对多使用的标签是collection
            7.collection标签的属性：
	                1）property="orderList"：表示在User实体类中的成员变量，用来保存多个订单信息的：private    List<Order> orderList;
                2）javaType="List" ：表示User实体类中保存多个订单信息的容器类型,可以不写，单列集合底层使用collection集合父接口
                3) ofType="Order" 表示User实体类中保存多个订单信息的容器的泛型类型，必须书写
        -->
			<collection property="orders" javaType="list" ofType="order" autoMapping="true">
			<id column="order_id" property="id"/>
			<result column="order_number" property="orderNumber"/>
		</collection>
	</resultMap>

	<select id="findUserAndOrdersByUserId" resultMap="userMap">
			select
			tb_user.*,tb_order.id as order_id,
			tb_order.order_number
			from tb_user,tb_order where tb_user.id = tb_order.user_id
			and tb_user.id = #{id}
	</select>
```

一对多总结:
![image.png](https://article.biliimg.com/bfs/article/70f3d01bfffcf0fc87c9bdbdab4c0910cd996a09.png)

`
### 多对多查询
- 用户和角色的关系：
	- 一个用户具有多个角色。举例：张三用户可以是QQ黄钻 绿钻 等
	- 一个角色可以对应多个用户。举例：QQ黄钻可以是张三 李四 等
- 角色和权限的关系：
	- 一个角色可以有多种权限。举例：QQ黄钻：可以查看被挡访客，可以装扮空间等
	- 一种权限可以对应多个角色。举例：可以查看被挡访客的权限可以是QQ黄钻，也可以是绿钻
- 根据表设计原则，多对多关系==创建中间表==，在中间表中起码要有另外**两张主表用户和角色的主键作为外键进行关联**
	![image-20210127085517907.png](https://article.biliimg.com/bfs/article/b5ce1c19c10c33756cddba01348ef2e318fc64a5.png)
	![image-20201106091731134.png](https://article.biliimg.com/bfs/article/db2e43cbac65ff4036bde7c9a586b3234705efcb.png)
- 需求:分页查询用户和对应的角色信息
	```mysql
	use `day08-02`;
	# 需求:分页查询用户和对应的角色信息
	select *from t_user limit 0,3;
	# 2.将上述分页查询的用户信息和中间表t_user_role以及角色表t_role连接查询
	select u.*,r.id rid,r.name,r.keyword,r.description from (select *from t_user limit 0,3) as u
	inner join t_user_role ur inner join
		t_role r on ur.user_id=u.id and ur.role_id=r.id; #多个inner join 最后on加上条件
	```
- 接口
	```java
	List<User> queryAllUsersAndRoles(@Param("beg") int beg, @Param("pages") int pages);
	```
- 编写sql映射文件
	```xml
	<resultMap id="usermap" type="User" autoMapping="true">  
		<id column="id" property="id" />  
		<collection property="roles" ofType="Role" autoMapping="true">  
			<id column="rid" property="id"/>  
		</collection>  
	</resultMap>  
	<select id="queryAllUsersAndRoles" resultMap="usermap">  
		select u.*,r.id rid,r.name,r.keyword,r.description  
		from (select *from t_user limit #{beg},#{pages}) as u  
		inner join t_user_role ur inner join t_role r  
			on ur.user_id=u.id and ur.role_id=r.id  
	</select>
	```
## mybatis注解开发(掌握)

### mybatis注解概述

>上述我们已经学习mybatis的SQL映射文件可以使用xml的方式配置，但是我们发现**不同的用户模块接口**都对应一个映射文件，并且在映射文件中书写sql语句也比较麻烦。所以Mybatis为用户提供了快速的开发方式，因为有时候大量的XML配置文件的编写时非常繁琐的，因此Mybatis也提供了更加简便的基于注解(Annnotation)的配置方式。
>注解配置的方式在很多情况下能够取代mybatis的映射文件，提高开发效率。

- CRUD相关注解
	1. `@Insert`：保存  
		 - Value：sql语句（和xml的配置方式一模一样）
	2. `@Update`：更新 
		 - Value：sql语句      
	3.` @Delete`: 删除
		 - Value：sql语句     
	4. `@Select`: 查询
		 - Value：sql语句  
	5. `@Options`：可选配置（获取主键）
		 - `userGeneratedKeys`：开关,值为true表示可以获取主键  相当于select last_insert_id()
		 - `keyProperty`     ：对象属性
		 - `keyColumn`       : 列名
注意点：
	注解开发一定使用[[mybatis#mappers(映射器)|包扫描]]方式:加载其它的映射文件xmL形式
### 新增@Insert

需求：
	定义方法实现注解插入数据到tb_user表中
定义接口
```java
public interface UserMapper {
	@Insert("insert into tb_user values(null,#{userName},#{password},#{name},#{age},#{sex})")
	Integer addUser(User user);
}//#{userName}:因为addUser方法的形参类型是复杂类型pojo即User实体类，所以这里大括号中书写的内容userName看User实体类中的成员 或者User实体类中的getUserName()去掉get将U变为u即userName
```
### 删除@Delete
需求：
	使用注解@Delete删除id值为6的数据;
定义接口
```java
	@Delete("delete from tb_user where id=#{id}")
	Integer deleteByUserId(@Param("id") Integer id);
```

### 修改@Update
需求：
	修改id为1的用户的数据
定义接口方法
```java
	@Update("update tb_user set user_name=#{userName},password=#{password}, name=#{name},age=#{age},sex=#{sex} where id=#{id}")
	Integer updateUser(User user);
```
### 查询@Select
目标：
	使用注解查询所有的用户数据
步骤：
	第一步：在接口中查询所有的用户数据的方法上面添加注解：@Select，然后设置其value属性值为具体的SQL查询语句；
	第二步：测试
接口
```java
	@Select("select * from tb_user")
	List<User> findAll();
```
### 返回新增数据的id(自增主键回填)

问题：
	上面注解实现CRUD的测试中，数据新增成功，但是id值没有正常返回.
目标：
	使用注解完成数据新增，新增成功后返回数据的主键id值
接口
```java
	@Insert("insert into tb_user values(null,#{userName},#{password},#{name},#{age},#{sex})")
	@Options(useGeneratedKeys = true,keyColumn = "id" ,keyProperty = "id")
	Integer addUserAndGetFk(User user);
```
### 注解实现别名映射
#列名属性不一致
根据之前的学习，如果数据表的列名和pojo实体类的属性名不一致，会导致数据表的数据无法封装到实体类属性值中，对此我们有如下解决方案
使用注解：
	`@Results实现映射(等价于<resultMap>标签)，注解源码如下：`
```java
	public @interface Results {
	    Result[] value() default {};
	}
```
我们发现value属于Result数组类型，而Result属于一个注解，注解的属性如下：
```java
	public @interface Result {
		boolean id() default false;如果是true表示当前字段是主键字段
	    //对应数据表的列
	    String column() default "";
	    //对应pojo类的属性
	    String property() default "";
	    //javaType：返回的对象类型
	    Class<?> javaType() default void.class;
	    //one： 一对一配置 相当于xml中association标签
	    One one() default @One;
	    //many： 一对多配置 相当于xml中collection标签
	    Many many() default @Many;
	}
```
接口
```java
	@Select("select * from tb_user where id=#{id}")
	@Results(id="userMap",
	value = {
		@Result(column = "id",property = "id",id=true),//是@Results注解数组value中第一个数据索引是0
		@Result(column = "user_name",property = "userName")//是@ResuLts注解数组value中第二个数据索引是1
	})
	User findById(Long id);
```

## mybatis基于注解实现动态SQL（理解）
【需求】：
	查询男性用户，如果输入了用户名，按用户名模糊查询,如果没有输入用户名，就查询所有男性用户； 
### @SelectProvider
- 创建提供sql的工具类
```java
	public class SqlProvider {
		public String findUserByName(@Param("uName") String name){
			String sql="select * from tb_user where sex=1";
		if(name!=null){
			sql+=" and user_name like concat('%',#{uName},'%')";
		}
			return sql;
		}
	}
```
- 接口
	- 使用 `@SelectProvider` 注解，注解中的type参数是提供构建 **SQL 的类**，method 是**构建SQL的方法**。构建 SQL 的方法的参数要和==接口的参数一致==，并且多个参数要使用@Param命名参数。
```java
	@SelectProvider(type = SqlProvider.class,method = "findUserByName")
	List<User> findUserByName2(@Param("uName") String name);
```

### @SelectProvider+SQL类

上述实现过程中，sql拼接容易出错，我们可借助mybatis提供的一个对象：SQL完成sql语句拼接。
创建SQL对象
	`SQL sql = new SQL();`
[[java基础篇#StringBuilder 类|链式编程]]，每个方法返回值都是SQL类对象；(类似StringBuider)
- 定义接口
```java
	@SelectProvider(type = SqlProvider.class,method = "findUserByName3")
	List<User> findUserByName3(@Param("uName") String name);
```
- 定义提供sql的类
```java
public String findUserByName3(@Param("uName") String name){
	SQL sql = new SQL();
	sql = sql.SELECT("*").FROM("tb_user").WHERE("sex=1");
	if(name!=null){
		sql = sql.AND().WHERE("user_name like concat('%',#{uName},'%')");
	}
	return sql.toString();
}
```

## 使用PageHelper实现分页查询（详细）

[[java依赖#pagehelper-spring-boot-starter|导入依赖]]

**pagehelper插件配置**
如果是 springboot，则直接配置几个配置项即可：
```yaml
# mybatis 相关配置
mybatis:
  #... 其他配置信息
  configuration-properties:
       helperDialect: mysql
       offsetAsPageNum: true
       rowBoundsWithCount: true
       reasonable: true
  mapper-locations: classpath:mapper/*.xml
```



```java
//第一种，RowBounds方式的调用
List<User> list = sqlSession.selectList("x.y.selectIf", null, new RowBounds(0, 10));

//第二种，Mapper接口方式的调用，推荐这种使用方式。
PageHelper.startPage(1, 10);
List<User> list = userMapper.selectIf(1);

//第三种，Mapper接口方式的调用，推荐这种使用方式。
PageHelper.offsetPage(1, 10);
List<User> list = userMapper.selectIf(1);

//第四种，参数方法调用
//存在以下 Mapper 接口方法，你不需要在 xml 处理后两个参数
public interface CountryMapper {
    List<User> selectByPageNumSize(
            @Param("user") User user,
            @Param("pageNum") int pageNum, 
            @Param("pageSize") int pageSize);
}
//配置supportMethodsArguments=true
//在代码中直接调用：
List<User> list = userMapper.selectByPageNumSize(user, 1, 10);

//第五种，参数对象
//如果 pageNum 和 pageSize 存在于 User 对象中，只要参数有值，也会被分页
//有如下 User 对象
public class User {
    //其他fields
    //下面两个参数名和 params 配置的名字一致
    private Integer pageNum;
    private Integer pageSize;
}
//存在以下 Mapper 接口方法，你不需要在 xml 处理后两个参数
public interface CountryMapper {
    List<User> selectByPageNumSize(User user);
}
//当 user 中的 pageNum!= null && pageSize!= null 时，会自动分页
List<User> list = userMapper.selectByPageNumSize(user);

//第六种，ISelect 接口方式
//jdk6,7用法，创建接口
Page<User> page = PageHelper.startPage(1, 10).doSelectPage(new ISelect() {
    @Override
    public void doSelect() {
        userMapper.selectGroupBy();
    }
});
//jdk8 lambda用法
Page<User> page = PageHelper.startPage(1, 10).doSelectPage(()-> userMapper.selectGroupBy());

//也可以直接返回PageInfo，注意doSelectPageInfo方法和doSelectPage
pageInfo = PageHelper.startPage(1, 10).doSelectPageInfo(new ISelect() {
    @Override
    public void doSelect() {
        userMapper.selectGroupBy();
    }
});
//对应的lambda用法
pageInfo = PageHelper.startPage(1, 10).doSelectPageInfo(() -> userMapper.selectGroupBy());

//count查询，返回一个查询语句的count数
long total = PageHelper.count(new ISelect() {
    @Override
    public void doSelect() {
        userMapper.selectLike(user);
    }
});
//lambda
total = PageHelper.count(()->userMapper.selectLike(user));

```


前面的User类，`UserMapper`以及`userService`都不需要更改，我们直接在`userServiceImpl`中添加分页查询的方法即可。
```java
    //分页查询功能
    public List<User> queryUserByPage(Integer pageNum,Integer pageSize){
        PageHelper.startPage(pageNum,pageSize);//
        return userMapper.queryUser();
    }

```

PageHelper的底层是利用[[java基础篇#ThreadLocal简介|ThreadLocal]]将page的信息存入线程，写xml文件会**动态**的把limit等拼进入
即使用时，只需提前声明要分页的信息，得到的结果就是有分页信息的了。 如果不想进行 count，只要查分页数据，则调用: PageHelper.startPage(pageNum,pageSize,false);即可，避免了不必要的count消耗。
## 综合案例


![](https://article.biliimg.com/bfs/article/00113a877a21b4f4f1ef3e63a2d8dcddcbd57ed6.png)

这里的综合案例有三层分别是
![image.png](https://article.biliimg.com/bfs/article/4e0394c3d124fbe96a467ce09368ac3abc8a62d8.png)
层与层之间怎么产生关联？就是==方法之间的调用==



这是一个创建mysql图表的语句，你给我写几行插入数的语句吧,

```sql
CREATE TABLE `setmeal` (

  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键',

  `category_id` bigint NOT NULL COMMENT '菜品分类id',

  `name` varchar(32) COLLATE utf8_bin NOT NULL COMMENT '套餐名称',

  `price` decimal(10,2) NOT NULL COMMENT '套餐价格',

  `status` int DEFAULT '1' COMMENT '售卖状态 0:停售 1:起售',

  `description` varchar(255) COLLATE utf8_bin DEFAULT NULL COMMENT '描述信息',

  `image` varchar(255) COLLATE utf8_bin DEFAULT NULL COMMENT '图片',

  `create_time` datetime DEFAULT NULL COMMENT '创建时间',

  `update_time` datetime DEFAULT NULL COMMENT '更新时间',

  `create_user` bigint DEFAULT NULL COMMENT '创建人',

  `update_user` bigint DEFAULT NULL COMMENT '修改人',

  PRIMARY KEY (`id`),

  UNIQUE KEY `idx_setmeal_name` (`name`)

) ENGINE=InnoDB AUTO_INCREMENT=32 DEFAULT CHARSET=utf8mb3 COLLATE=utf8_bin COMMENT='套餐';
```


以下是几行插入数据的语句：

```sql
INSERT INTO `setmeal` (`category_id`, `name`, `price`, `status`, `description`, `image`, `create_time`, `update_time`, `create_user`, `update_user`)
VALUES (1, '套餐1', 50.00, 1, '这是套餐1的描述', 'image1.jpg', NOW(), NOW(), 1, 1);

INSERT INTO `setmeal` (`category_id`, `name`, `price`, `status`, `description`, `image`, `create_time`, `update_time`, `create_user`, `update_user`)
VALUES (2, '套餐2', 80.00, 1, '这是套餐2的描述', 'image2.jpg', NOW(), NOW(), 2, 2);

INSERT INTO `setmeal` (`category_id`, `name`, `price`, `status`, `description`, `image`, 
                       create_time, update_time, create_user, update_user)
VALUES (3, '套餐3', 120.00, 0, '这是套餐3的描述', 'image3.jpg', NOW(), NOW(), 3, NULL);
```

请注意，以上示例中的插入值仅供参考，具体数据根据实际情况进行调整。






