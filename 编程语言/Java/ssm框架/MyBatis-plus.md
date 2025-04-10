# MyBatisPlus简介 

## 入门案例

- MyBatisPlus（简称MP）是基于MyBatis框架基础上开发的<font color=#ff0000>增强型</font>工具，旨在简化开发、提高效率

- 开发方式
	- 基于MyBatis使用MyBatisPlus
	- 基于Spring使用MyBatisPlus
	- <font color=#ff0000>基于SpringBoot使用MyBatisPlus</font>


1. 手动添加mp起步依赖 

	```xml
	<dependency> 
		<groupId>com.baomidou</groupId> 
		<artifactId>mybatis-plus-boot-starter</artifactId> 
		<version>3.5.3.1</version> 
	</dependency> 
	```


> [!attention] 注意
> 
> 由于mp并未被收录到idea的系统内置配置，无法直接选择加入 


2. 设置Jdbc参数（<font color=#ff0000>application.yml</font>) 
	```yml
	server:
	  port: 80
	spring:
	  datasource:
	    username: root
	    url: jdbc:mysql:///mybatisplus_db
	    driver-class-name: com.mysql.cj.jdbc.Driver
	    password: 701121
	    type: com.alibaba.druid.pool.DruidDataSource
	# 开启mp的日志（输出到控制台）
	mybatis-plus:
	  configuration:
	    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
	```


> [!attention] 注意事项 
> 如果使用Druid数据源，需要导入对应坐标 

4. 写BoodaOy

3. 测试类中注入dao接口，测试功能 
	```java
	@SpringBootTest 
	class Mybatisplus01QuickstartApplicationTests { 
		@Autowired 
		private UserDao userDao; 
		@Test 
		public void testGetAll(){ 
			List<User>userList = userDao.selectList(null); 
			userList.forEach(System.out::println); 
			}
	}
	```


## MyBatisPlus概述

MyBatisPlus（简称MP）是基于MyBatis框架基础上开发的增强型工具，旨在<font color=#ff0000>简化开发、提高效率</font>
官网：[https://mybatis.plus/](https://mybatis.plus/)     	[https://mp.baomidou.com/](https://mp.baomidou.com/)


**MyBatisPlus特性**

- 无侵入：只做增强不做改变，不会对现有工程产生影响
- 强大的 CRUD 操作：内置通用 Mapper，少量配置即可实现单表CRUD 操作
- 支持 Lambda：编写查询条件无需担心字段写错
- 支持主键自动生成
- 内置分页插件
- ……

# 标准数据层开发 

## 标准数据层CRUD功能



|功能| 自定义接口 | MP接口 |
|:-:|:-:|:-:|
| 新增 | `boolean save(T t)` | `int insert(T t)` |
| 删除 | `boolean delete(int id)` | `int deleteById(Serializable id)` |
| 修改 | `boolean update(T t)` | `int updateById(T t)` |
| 根据id查询 | `T getById(int id)` | `T selectById(Serializable id)` |
| 查询全部 | `List<T> getAll()` | `List<T> selectList()` |
| 分页查询 | `PageInfo<T> getAll(int page, int size)` | `IPage<T> selectPage(IPage<T> page)` |
| 按条件查询 | `List<T> getAll(Condition condition)` | `IPage<T> selectPage(Wrapper<T> querywrapper)` 

## MP分页查询功能


> [!summary]+ 实现原理概括
> 1. select * from user  ????   <font color=#ff0000> limit 1,2 </font>
> 2. 在原有的查基础上增加功能，这是[[spring#AOP核心概念|AOP]]思想，不过这里我们用[[spring#拦截器概念|拦截]]并增强，
> 3. 所以这里要配置MyBatis-plus的拦截器，在拦截器中开启<font color=#ff0000>分页拦截器</font>



1. 设置分页拦截器作为Spring管理的bean

	```java
	@Configuration  //这个会被引入类扫描到
	public class MpConfig {
	  @Bean
	  public MybatisPlusInterceptor mPlusInterceptor()
	  {
	    //1.定义Mp拦截器
	    MybatisPlusInterceptor mybatisPlusInterceptor=new MybatisPlusInterceptor();
	    //2.添加具体的拦截器
	    mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
	    return mybatisPlusInterceptor;
	  }
	}
	```

2. 执行分页查询
	```java
	  @Test
	  public void testSelectByPage() {
	    IPage<User> page=new Page<User>(2,2);
	    userDao.selectPage(page, null);
	    System.out.println("当前页码值" + page.getCurrent());
	    System.out.println("每页显示数" + page.getSize());
	    System.out.println("一共多少页" + page.getPages());
	    System.out.println("一共多少条数据" + page.getTotal());
	    System.out.println("数据" + page.getRecords());
	  }
	```

# DQL控制 

## 条件查询

- MyBatisPlus将书写复杂的SQL查询条件进行了封装，使用编程的形式完成查询条件的组合

<img src="https://article.biliimg.com/bfs/article/f53e77460313a6bf829eb58e4f98ca8bc9b8529c.png" alt="image.png" style="zoom:50%;" />

### 条件查询——设置查询条件

```java
    //方式一: 按条件查询
    QueryWrapper qw=new QueryWrapper();
    qw.lt("age",17);
    List<User> userList=userDao.selectList(qw);
    System.out.println(userList);
```

```java
    //方式二: lambda格式按条件查找
    QueryWrapper<User> qw=new QueryWrapper<>();
    qw.lambda().lt(User::getAge,18);//简单解释一下这个lambda就是引用实体类的方法，实际传出的数据是实体类里的变量名
    List<User> users = userDao.selectList(qw);
    System.out.println("users = " + users);
```

```java
    //方式三: lambda格式按条件查找
    LambdaQueryWrapper<User> lqw=new LambdaQueryWrapper<>();
    //选择大于18岁并且小于30岁的
     lqw.lt(User::getAge,30).gt(User::getAge,18);  //并且(and)
    //选择大于18岁或者小于30岁的
    lqw.lt(User::getAge,18).or().gt(User::getAge,30);  //或者(or)
    List<User> users = userDao.selectList(lqw);
    System.out.println("users = " + users);
```

## 条件查询——null值处理

<img src="https://article.biliimg.com/bfs/article/546ca65f9cdec3732d954250877d996201671f2c.png" alt="image.png" style="zoom:50%;" />

- 条件参数控制

```java
  @Test
  public void test_Get_null()
  {
    UserCondition userCondition=new UserCondition();
//    userCondition.setCon_age();这里为空
    userCondition.setAge(18);
    LambdaQueryWrapper<User> lqw=new LambdaQueryWrapper<>();
    lqw.lt(null!=userCondition.getCon_age(),User::getAge,userCondition.getCon_age()); //第一个参数为条件
    lqw.gt(null!=userCondition.getAge(),User::getAge,userCondition.getAge());
    List<User> users = userDao.selectList(lqw);
    System.out.println("users = " + users);

  }
```


## 查询投影

- 查询结果包含模型类中部分属性

	```java
	    //查询投影
	LambdaQueryWrapper<User> lqm=new LambdaQueryWrapper<>();
		//第一种方式
		lqm.select(User::getName,User::getTel); 
		List<User> users = userDao.selectList(lqm);
		System.out.println(userList); 
	
		QueryWrapper<User>qw=new QueryWrapper<>();
		qw.select("age","name");
		List<User> users = userDao.selectList(qw);
	```

- 查询结果包含模型类中未定义的属性

	```java
	QueryWrapper<User> qm = new QueryWrapper<User>();
	qm.select("count(*) as nums,gender");
	qm.groupBy("gender");
	List<Map<String, Object>> maps = userDao.selectMaps(qm);
	System.out.println(maps); //maps = [{sex=男, nums=4}, {sex=女, nums=2}] 
	```

实际的sql语句为`SELECT COUNT(*) nums,sex FROM `user` GROUP BY sex`


|nums |sex|
|:-:|:-:|
|4|男|
|2|女|

可以看到这里的maps是以<font color=#ff0000>一行</font>为一个map

## 查询条件

- 范围匹配（> 、 = 、between）
- [[Mysql#模糊查询|模糊匹配]]（like）
- 空判定（null）
- 包含性匹配（in）
- [[Mysql#分组查询|分组]]（group）
- [[Mysql#排序查询|排序]]（order）
- ……

### 等值查询

- 用户登录（eq匹配）
	```java
	  @Test
	  public void test_con_query() throws ParseException {
	    LambdaQueryWrapper<User> lqw=new LambdaQueryWrapper<>();
	    //条件查询  选复数属性对象的一行
	    Date birthday = Date.valueOf("1990-01-01");
	    lqw.eq(User::getUserName,"张三").eq(User::getBirthday,birthday);//满足条件
	    User user = userDao.selectOne(lqw);//这代表只选一个
	    System.out.println("user = " + user);
	  }
	```


### 范围查询

- 购物设定价格区间、户籍设定年龄区间（le ge匹配  或  between匹配）

	```java
	    //设定上下范围
	    lqw.between(User::getBirthday,Date.valueOf("1990-01-01"),Date.valueOf("1995-11-04"));
	    List<User> users = userDao.selectList(lqw);
	    System.out.println("users = " + users);
	```


### 模糊查询

- 查信息，搜索新闻（非全文检索版：like匹配）
	```java
	    //模糊匹配
	    lqw.like(User::getAddress,"南山"); //匹配 %南山%
	    lqw.likeLeft(User::getAddress,"%区"); //匹配 %区
	    lqw.likeRight(User::getAddress,"北京"); //匹配 北京%
	    List<User> users = userDao.selectList(lqw);
	    System.out.println("users = " + users);
	```

### 排序查询

- 排序

```java
@Test
void orderByAsc(){
    //1.创建QueryWrapper对象
    LambdaQueryWrapper<User> lambdaQueryWrapper = new LambdaQueryWrapper<>();
    //2.设置条件，指定升序排序字段
    lambdaQueryWrapper.orderByAsc(User::getAge,User::getId); //按顺序进行排列

	//第一个参数代表空值是否参与排序，第二个参数是否是降序，orderby可以自定义排序规则
	lambdaQueryWrapper.orderBy(true,true,User::getId);    
	lambdaQueryWrapper.orderBy(true,false,User::getAge);
    //3.使用条件完成查询
    List<User> users = userMapper.selectList(lambdaQueryWrapper);
    System.out.println(users);
}

```

- 自定义规则的排序 `func`

```java
@Test
void func(){
    //1.创建QueryWrapper对象
    LambdaQueryWrapper<User> lambdaQueryWrapper = new LambdaQueryWrapper<>();
    //2.构建逻辑判断语句
    lambdaQueryWrapper.func(i -> {
        if(true) {
            i.eq(User::getId, 1);   //这里就用到了lamda简化
        }else {  
            i.ne(User::getId, 1);  
        }  
    });    
    
   //3.完成查询    
   List<User> users = userMapper.selectList(lambdaQueryWrapper);    System._out_.println(users);  
}
```


### 逻辑查询

- 嵌套的and
```java
  @Test
  void test_nestd_and()
  {
    LambdaQueryWrapper<SysUser>lqw=new LambdaQueryWrapper<>();
    lqw.eq(SysUser::getName,"yesho").and(i->i.gt(SysUser::getAge,23).or().lt(SysUser::getAge,45));
    List<SysUser> sysUsers = userMapper.selectList(lqw);
  }
```


- `nested`
```java
@Test
void nested(){
    //1.创建QueryWrapper对象
    LambdaQueryWrapper<User> lambdaQueryWrapper = new LambdaQueryWrapper<>();
    //2.构建条件查询语句
    lambdaQueryWrapper.nested(i -> i.eq(User::getName, "Billie").ne(User::getAge, 22));  //这里的nested相当于一句where
    //3.完成查询
    List<User> users = userMapper.selectList(lambdaQueryWrapper);
    System.out.println(users);
	// Preparing: SELECT id, name, age, email FROM powershop_ user WHERE ((name =? AND age >?)) 
	// Parameters: Billie(String), 22(Integer) 
}
```


### 判空查询

- `isnull`
	```java
	@Test
	void isNull(){
	    //1.创建QueryWrapper对象
	    LambdaQueryWrapper<User> lambdaQueryWrapper = new LambdaQueryWrapper<>();
	    //2.设置条件，指定字段名称
	    lambdaQueryWrapper.isNull(User::getName);//查找User里面name为null的行
	    //3.使用条件完成查询
	    List<User> users = userMapper.selectList(lambdaQueryWrapper);
	    System.out.println(users);
	}
	```

### 包含查询

- `in`
```java
@Test
void in(){
    //1.创建QueryWrapper对象
    LambdaQueryWrapper<User> lambdaQueryWrapper = new LambdaQueryWrapper<>();
    //2.设置条件，指定字段名称和值
    ArrayList<Integer> arrayList = new ArrayList<>();
    Collections.addAll(arrayList,18,20,21);
    // lambdaQueryWrapper.in(User::getAge,18,20,21);
    lambdaQueryWrapper.in(User::getAge,arrayList);
    //3.使用条件完成查询
    List<User> users = userMapper.selectList(lambdaQueryWrapper);
    System.out.println(users);
}

```

- `inSql`  自定义in 第二个参数是字符串
	```java
	@Test
	void inSql(){
	    //1.创建QueryWrapper对象
	    LambdaQueryWrapper<User> lambdaQueryWrapper = new LambdaQueryWrapper<>();
	    //2.设置条件，指定字段名称和值
	    lambdaQueryWrapper.inSql(User::getAge,"18,20,22"); 
	    lambdaQueryWrapper.inSql(User::getAge,"select age from powershop_user where age > 20");
	    //3.使用条件完成查询
	    List<User> users = userMapper.selectList(lambdaQueryWrapper);
	    System._out_.println(users);  
	}
	```


### 聚合查询

- `having`
	```java
	@Test
	void having(){
	    QueryWrapper<User> queryWrapper = new QueryWrapper<>();
	    //分组字段
	    queryWrapper.groupBy("age");
	    //查询字段
	    queryWrapper.select("age,count(*) as field_count");
	    //聚合条件筛选
	    queryWrapper.having("field_count = 1"); //这里是以字符串开始
	    List<Map<String, Object>> maps = userMapper.selectMaps(queryWrapper);
	    System.out.println(maps);
	}
	```


### 自定义条件查询


- apply

	```java
	@Test
	void apply(){
	    //1.创建QueryWrapper对象
	    LambdaQueryWrapper<User> lambdaQueryWrapper = new LambdaQueryWrapper<>();
	    //2.构建条件查询语句
	    lambdaQueryWrapper.apply("id = 1"); 
	    //3.完成查询
	    List<User> users = userMapper.selectList(lambdaQueryWrapper);
	    System.out.println(users);  
	    // Preparing: SELECT id, name, age, email FROM powershop user WHERE (id=1) 
	}
	```

### last查询

- last
```java
@Test
void last(){
    //1.创建QueryWrapper对象
    LambdaQueryWrapper<User> lambdaQueryWrapper = new LambdaQueryWrapper<>();
    //2.构建条件查询语句
    lambdaQueryWrapper.last("limit 0,2");
    //3.完成查询
    List<User> users = userMapper.selectList(lambdaQueryWrapper);
    System.out.println(users);
}
```

### exists查询


```java
@Test
void exists(){
    //1.创建QueryWrapper对象
    LambdaQueryWrapper<User> lambdaQueryWrapper = new LambdaQueryWrapper<>();
    //2.构建查询条件
    lambdaQueryWrapper.exists("select id from powershop_user where age = 18"); //当这个条件满足时下面的才会进行
    //3.查询
    List<User> users = userMapper.selectList(lambdaQueryWrapper);
    System.out.println(users);
}
```



- 练习
	```java
	      //小练习 先降序，先只选前5条
	      lqw.orderByDesc(UserSale::getSale);
	      IPage<UserSale> iPage=new Page<>(0,5);
	      useSaleDao.selectPage(iPage,lqw);
	      System.out.println(iPage.getRecords());
	```



更多查询条件设置参看[https://mybatis.plus/guide/wrapper.html#abstractwrapper](https://mybatis.plus/guide/wrapper.html#abstractwrapper)

## 字段映射与表名映射

问题一：表字段与编码属性设计不同步

<img src="https://article.biliimg.com/bfs/article/05d93b84ee74cb22770db396af90009082de4b61.png" alt="image.png" style="zoom:50%;" />

问题二：编码中添加了数据库中未定义的属性

<img src="https://article.biliimg.com/bfs/article/cf0e60a91fee402a0c24333bb68628d2f26e4ef4.png" alt="image.png" style="zoom:50%;" />


问题三：采用默认查询开放了更多的字段查看权限

<img src="https://article.biliimg.com/bfs/article/5d298f1a9456fc67c3a4afe922e4e00f94e88f37.png" alt="image.png" style="zoom:50%;" />


`@TableField`
作用：设置当前**属性对应的数据库表中的字段关系**

```java
@TableName("tbl_user") //配置数据库表名
public class ckUser {

  private long id;
  private String name;
  @TableField(value = "password",select = false) 
  private String pwd;
  private long age;
  private String tel;
  @TableField(exist = false)
  private Integer online;
  public ckUser() {
  }
```


**相关属性**
- value：设置数据库表字段名称
- exist：设置属性在数据库表字段中是否存在，默认为true。此属性无法与value合并使用
- select：设置属性是否参与查询，此属性与select()映射配置不冲突



# DML控制 

## id生成策略控制

- 不同的表应用不同的id生成策略
	- 日志：自增（1,2,3,4，……）
	- 购物订单：特殊规则（FQ23948AK3843）
	- 外卖单：关联地区日期等信息（10 04 20200314 34 91）
	- 关系表：可省略id
	- ……

`@TableId`

作用：设置当前类中主键属性的生成策略
```java
public class User {
    @TableId(type = IdType.AUTO)
    private Long id;
    }
```

- `AUTO(0)`：使用数据库id自增策略控制id生成
- `NONE(1)`：不设置id生成策略
- `INPUT(2)`：用户手工输入id
- `ASSIGN_ID(3)`：雪花算法生成id（可兼容数值型与字符串型）
- `ASSIGN_UUID(4)`：以UUID生成算法作为id生成策略


### 雪花算法


<img src="https://article.biliimg.com/bfs/article/610efaf36d510f9294481508a7bcc7d8ec192092.png" alt="image.png" style="zoom:50%;" />


**雪花算法**是由一个64位的二进制组成的，最终就是一个`Long`类型的数值。 
主要分为四部分存储 
> [!cite]
> 1. 1位的符号位，固定值为 
> 2. 41位的时间戳 
> 3. 10位的机器码，包含5位机器id和5位服务id 
> 4. 12位的序列号 
> 5. 使用雪花算法可以实现有序、唯一、且不直接暴露排序的数字。


### UUID

UUID(`Universally Unique Identifier`)全局唯一标识符，定义为一个<font color=#ff0000>字符串</font>主键， 采用32位数字组成，编码采用16进制，定义了在时间和空间都完全唯一的系统信息。 

> [!summary]+ UUID的编码规则： 
> 1. 1~8位采用系统时间，在系统时间上精确到毫秒级保证时间上的唯一性；← 
> 2. 9~16位采用底层的IP地址，在服务器集群中的唯一性； 
> 3. 17~24位采用当前对象的HashCode值，在一个内部对象上的唯一性； 
> 4. 25~32位采用调用方法的一个随机数，在一个对象内的毫秒级的唯一性。 
> 

通过以上4种策略可以保证唯一性。在系统中需要用到随机数的地方都可以考虑采用UUID 算法。 

```java
  @Test
  void test_uuid()
  {
    LambdaQueryWrapper<SysUser> lqw=new LambdaQueryWrapper<>();
    SysUser sysUser=new SysUser();
    sysUser.setAge(13);
    sysUser.setEmail("246598@qq.com");
    sysUser.setName("ysehops");
    userMapper.insert(sysUser);
    // 984962f0b7c62740b64229c5f9e5b051,ysehops,13,246598@qq.com
  }
```


## id生成策略全局配置

<img src="https://article.biliimg.com/bfs/article/9d69a5c5409c23417811e21db9c38be6c5035b20.png" alt="image.png" style="zoom:50%;" />
```yml
mybatis-plus:
	global-config:
		db-config:
			id-type: auto
			table-prefix: tbl_
```


## 多记录操作

<img src="https://article.biliimg.com/bfs/article/b90976ae5a26807d6f24e94fc61dd2bd43c68df0.png" alt="image.png" style="zoom:50%;" />

- 按照主键删除多条记录 
	```java
	List<Long> ids= Arrays.asList(new Long[]{2,3}); 
	userDao.deleteBatchIds(ids); 
	```
- 根据主键查询多条记录 
	```java
	List<Long> ids= Arrays.asList(new Long[]{2,3}); 
	List<User> userList = userDao.selectBatchIds(ids); 
	```

## 逻辑删除

- 删除操作业务问题：业务数据从数据库中丢弃 
- 逻辑删除：为数据设置是否可用状态字段，删除时设置状态字段为不可用状态，数据保留在数据库中 

<img src="https://article.biliimg.com/bfs/article/d5c4f6aa1e9337c154b5bce1aeb3117572c03f3c.png" alt="image.png" style="zoom:50%;" />
1. 数据库表中添加逻辑删除标记字段
	<img src="https://article.biliimg.com/bfs/article/0108c8c2940cac60ee109d56ecaeaff47a8606b9.png" alt="image.png" style="zoom:50%;" />

2. 实体类中添加对应字段，并设定当前字段为逻辑删除标记字段
	```java
	@TableName("tbl_user")
	public class User {
	  //逻辑删除字段，标记当前记录是否被删除
	  @TableLogic(value = "0",delval = "1")
	  private Integer deleted;
	  }
	```

3. 配置逻辑删除字面值 
```yml
mybatis-plus: 
	global-config: 
		db-config: 
			logic-delete-field: deleted 
			logic-not-delete-value: 0 
			logic-delete-value: 1 
```

执行SQL语句： `UPDATE tbl user SET deleted=1 WHERE id=? AND deleted=0 `


## 乐观锁 

- 业务并发现象带来的问题：秒杀 

	<img src="https://article.biliimg.com/bfs/article/c55dad3c2c69db2f41e080225e1d45a7290d2e8f.gif" alt="image.png" style="zoom:50%;" />

1.  数据库表中添加锁标记字段
	![image.png](https://article.biliimg.com/bfs/article/5d332fb03981625659d4aab259381946813a4b38.png)

2. 实体类中添加对应字段，并设定当前字段为逻辑删除标记字段
	```java
	public class User {
		private Long id;
		@Version
		private Integer version;
	}
	```




3. 
	- 乐观锁就是通过加version的值，使得每个人的值不一样,来达到锁机制
	- update set abc = 1 , <font color=#ff0000>version = version + 1</font>  where version=1 
	- 所以还是要**配置乐观锁拦截器**实现锁机制对应的动态SQL语句拼装
	```java
	  @Bean
	  public MybatisPlusInterceptor mybatisPlusInterceptor()
	  {
	    MybatisPlusInterceptor mybatisPlusInterceptor=new MybatisPlusInterceptor();
	    mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
	    mybatisPlusInterceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
	    return mybatisPlusInterceptor;
	  }
	```

4. 乐观锁使用案例

	```java
	    //1.先通过要修改的数据id将当前数据查询出来
	    User user = userDao.selectById(3L);
	    //2.将要修改的属性逐一设置进去 
	    user.setName("hexo");
	    userDao.updateById(user);
	```

	```java
	    //测试3个数据同时进行修改，谁会生效
	    User user1=userDao.selectById(3L); //version=1
	    User user2=userDao.selectById(3L); //version=1
	    User user3=userDao.selectById(3L); //version=1
	    user1.setName("hexo");
	    userDao.updateById(user1);//version=2;
	    user2.setName("xcode");
	    userDao.updateById(user2);//version=1 != version=2;
	    user3.setName("vscode");
	    userDao.updateById(user3);//version=1 != version=2;
	```

	```mysql
	==>  Preparing: UPDATE tbl_user SET name=?, password=?, age=?, tel=?, version=? WHERE id=? AND version=? AND deleted=0
	==> Parameters: hexo(String), password3(String), 28(Long), 1112223333(String), 2(Integer), 3(Long), 1(Integer)
	<==    Updates: 1
	==>  Preparing: UPDATE tbl_user SET name=?, password=?, age=?, tel=?, version=? WHERE id=? AND version=? AND deleted=0
	==> Parameters: xcode(String), password3(String), 28(Long), 1112223333(String), 2(Integer), 3(Long), 1(Integer)
	<==    Updates: 0
	```




> [!summary] 总结
> 1. 乐观锁，访问数据的时候，也会拿到 version
> 2. 对数据操作完，想要保存这个操作（比如修改）时，会比对 version
> 3. 将之前拿到的 version 和现在库里的 version 进行比较
> 4. 如果两个 version  不相等，说明已经有别的线程在之前修改过数据了，即与当前线程的操作冲突，则当前线程放弃修改数据


> [!cite]+ 扩展
> 悲观锁：悲观锁是在查询的时候就**锁定数据**，在这次请求未完成之前，不会释放锁(别人也无法查)。等到                      
> 这次请求完毕以后，再释放锁，释放了锁以后，其他请求才可以对于这条数据完成读写 



##  ActiveRecord模式

### ActiveRecord介绍

`ActiveRecord`(活动记录，简称AR)，是一种<font color=#ff0000>领域</font>模型模式，特点是一个模型类对应关系型数据库中的一个表，而模型类的一个实例对应表中的一行记录。ActiveRecord 一直广受解释型动态语言（ PHP 、 Ruby 等）的喜爱，通过围绕一个**数据对象进行CRUD操作**。而 Java 作为准静态（编译型）语言，对于 ActiveRecord 往往只能感叹其优雅，所以 MP 也在 AR 道路上进行了一定的探索，仅仅需要让实体类继承 Model 类且实现主键指定方法，即可开启 AR 之旅。

### ActiveRecord实现

接下来我们来看一下ActiveRecord的实现步骤

- 让实体类继承`Model<User>`类
	```java
	@Data
	@AllArgsConstructor
	@NoArgsConstructor
	public class User extends Model<User> {
	    private Long id;
	    private String name;
	    private Integer age;
	    private String email;
	}
	```


我们可以看到，Model类中提供了一些**增删改查**方法，这样的话我们就可以直接使用实体类对象调用这些增删改查方法了，简化了操作的语法，但是他的底层依然是需要UserMapper的，所以持久层接口并不能省略

- 测试ActiveRecord模式的增删改查

	```java
	@Test
	void activeRecordAdd(){
	    User user = new User();
	    user.setName("wang");
	    user.setAge(35);
	    user.setEmail("wang@powernode.com");
	    user.insert();
	}
	
	@Test
	void activeRecordDelete(){
	    User user = new User();
	    user.setId(8L);
	    user.deleteById();
	}
	
	@Test
	void activeRecordUpdate(){
	    User user = new User();
	    user.setId(6L);
	    user.setAge(50);
	    user.updateById();
	}
	
	@Test
	void activeRecordSelect(){
	    User user = new User();
	    user.setId(6L);
	    User result = user.selectById();
	    System.out.println(result);
	}
	```


## SimpleQuery工具类

### SimpleQuery介绍

`SimpleQuery`可以对`selectList`查询后的结果用Stream流进行了一些封装，使其可以返回一些指定结果，简洁了api的调用

#### list
演示基于字段封装集合
```java
@Test
void testList(){
    List<Long> ids = SimpleQuery.list(new LambdaQueryWrapper<User>().eq(User::getName, "Mary"), User::getId);
    System.out.println(ids);
}
```

演示对于封装后的字段进行lambda操作
```java
@Test
void testList2(){
    List<String> names = SimpleQuery.list(new LambdaQueryWrapper<User>().eq(User::getName, "Mary"),User::getName,e ->  Optional.of(e.getName()).map(String::toLowerCase).ifPresent(e::setName));
    System.out.println(names);
}

```

#### map
演示将所有的对象以id,实体的方式封装为Map集合
```java
@Test
void testMap(){
    //将所有元素封装为Map形式
    Map<Long, User> idEntityMap = SimpleQuery.keyMap(
            new LambdaQueryWrapper<>(), User::getId);
    System.out.println(idEntityMap);
}

```

演示将单个对象以id,实体的方式封装为Map集合

```java
@Test
void testMap2(){
    //将单个元素封装为Map形式
    Map<Long, User> idEntityMap = SimpleQuery.keyMap(
            new LambdaQueryWrapper<User>().eq(User::getId,1L), User::getId);
    System.out.println(idEntityMap);
}

```

演示只想要id和name组成的map
```java
@Test
void testMap3(){
    //只想要只想要id和name组成的map
    Map<Long, String> idNameMap = SimpleQuery.map(new LambdaQueryWrapper<>(), User::getId, User::getName);
    System.out.println(idNameMap);
}
```

#### Group
演示分组效果
```java
@Test
void testGroup(){
    Map<String, List<User>> nameUsersMap = SimpleQuery.group(new LambdaQueryWrapper<>(), User::getName);
    System.out.println(nameUsersMap);
}
```

## 通用枚举

当我们想要表示一组信息，这组信息只能从一些固定的值中进行选择，不能随意写，在这种场景下，枚举就非常的合适。
例如我们想要表示性别，性别只有两个值，要么是男性，要么是女性，那我们就可以使用枚举来描述性别。

1. 编写枚举类
	```java
	public enum GenderEnum {
	
	    MAN(0,"男"),
	    WOMAN(1,"女");
	
	    private Integer gender;
	    private String genderName;
	
	    GenderEnum(Integer gender, String genderName) {
	        this.gender = gender;
	        this.genderName = genderName;
	    }
	}
	```

2. 实体类添加相关字段
	```java
	@Data
	@AllArgsConstructor
	@NoArgsConstructor
	public class User extends Model<User> {
	    private Long id;
	    private String name;
	    private Integer age;
	    private String email;
	    @EnumValue  //一定要加这个注解，不然枚举类型作为int数字插入到数据库中
	    private GenderEnum gender;
	    private Integer status;
	}
	```

3. 添加数据
	```java
	@Test
	void enumTest(){
	    User user = new User();
	    user.setName("liu");
	    user.setAge(29);
	    user.setEmail("liu@powernode.com");
	    user.setGenderEnum(GenderEnum._MAN_);    
	    user.setStatus(1);    
	    userMapper.insert(user);  
	}
	```


# 快速开发 

## 代码生成器 

<img src="https://article.biliimg.com/bfs/article/f43444b38ab4d0d8e019181cf6473ee3cf141217.png" alt="image.png" style="zoom:40%;" />


<img src="https://article.biliimg.com/bfs/article/22ede2c36d173c052074d8f287eac4ed2852fd0b.png" alt="image.png" style="zoom:50%;" />


- 模板：MyBatisPlus提供
- 数据库相关配置：读取数据库获取信息
- 开发者自定义配置：手工配置

所需[[java依赖#velocity-engine-core|velocity-engine-core]]或者[[java依赖#freemarker|freemarker]]和[[java依赖#mybatis-plus-generator|mybatis-plus-generator]]依赖

请看文档[代码生成器（新）](https://baomidou.com/pages/779a6e/#%E5%BF%AB%E9%80%9F%E5%85%A5%E9%97%A8)

执行这个代码就可以生成
```java
 FastAutoGenerator.create("jdbc:mysql:///mybatisplus_db", "root", "701121")
            .globalConfig(builder -> {
              builder.author("Yesho") // 设置作者
                     // .enableSwagger() // 开启 swagger 模式
                      .fileOverride() // 覆盖已生成文件
                      .outputDir("D:\\ComputerSelfStudy\\JAVA"); // 指定输出目录
            })
//            .dataSourceConfig(builder -> builder.typeConvertHandler((globalConfig, typeRegistry, metaInfo) -> {
//              int typeCode = metaInfo.getJdbcType().TYPE_CODE;
//              if (typeCode == Types.SMALLINT) {
//                // 自定义类型转换
//                return DbColumnType.INTEGER;
//              }
//              return typeRegistry.getColumnType(metaInfo);
//
//            }))
            .packageConfig(builder -> {
              builder.parent("java_project") // 设置父包名
                      .moduleName("mybatisplus") // 设置父包模块名
                      .pathInfo(Collections.singletonMap(OutputFile.xml, "D:\\ComputerSelfStudy\\JAVA")); // 设置mapperXml生成路径
            })
            .strategyConfig(builder -> {
              builder.addInclude("user") // 设置需要数据的表名
                      .addTablePrefix("t_", "c_"); // 设置实体类过滤表前缀
            })
            .templateEngine(new FreemarkerTemplateEngine()) // 使用Freemarker引擎模板，默认的是Velocity引擎模板
            .execute();
```


## Easycode插件

请看文档 [IDEA集成EasyCode插件](https://blog.csdn.net/yu1431/article/details/130924049?spm=1001.2014.3001.5502)

## MybatisX快速开发插件

[MybatisX快速开发插件](https://baomidou.com/pages/ba5b24/#%E5%8A%9F%E8%83%BD)

[MyBatisX](https://zhuanlan.zhihu.com/p/606036563)

[Spring Boot + MybatisX = 王炸！！ - 文章详情](https://z.itpub.net/article/detail/14AB6B23632DEEB9EBEC4316933DD7DF)

