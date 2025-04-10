# SpringBoot简介

## 入门案例 

- SpringBoot是由Pivota1团队提供的全新框架，其设计目的是用来==简化==Spring应用的**初始搭建**以及**开发过程** 

- 原生开发SpringMVC程序过程

	<img src="https://article.biliimg.com/bfs/article/02ebbc5a6ee39585205b24a6d155781c9354b776.gif" alt="image.png" style="zoom:50%;" />


步骤 :SpringBoot入门程序 
1. 创建新模块，选择Spring初始化，并配置模块相关基础信息;选择当前模块需要使用的技术集

	<img src="https://article.biliimg.com/bfs/article/8113cc040c37bcaa51019cdbc84c4cd3b0121357.png" alt="image.png" style="zoom:75%;" />

2. 开发控制器类

```java
@RestController
@RequestMapping("/books")
public class BookController {
  @GetMapping("/{id}")
  public String getById(@PathVariable Integer id)
  {
    System.out.println("id==>"+id);
    return "hello, string boot!";
  }
}
```

3. 运行自动生成的Application类

	<img src="https://article.biliimg.com/bfs/article/07276095b86076ecd7793c408c275e5b7d7a544e.png" alt="image.png" style="zoom:75%;" />


- 最简SpringBoot程序所包含的基础文件 
	- pom.xml文件 
		```xml
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
		    <artifactId>springboot_01_quickstart</artifactId>
		    <version>0.0.1-SNAPSHOT</version>
		    <dependencies>
		        <dependency>
		            <groupId>org.springframework.boot</groupId>
		            <artifactId>spring-boot-starter-web</artifactId>
		        </dependency>
		    </dependencies>
		    <build>
		        <plugins>
		            <plugin>
		                <groupId>org.springframework.boot</groupId>
		                <artifactId>spring-boot-maven-plugin</artifactId>
		                <version>3.1.1</version>
		            </plugin>
		        </plugins>
		    </build>
		</project>
		```

	- Application类 

		```java
		@SpringBootApplication
		public class Application {
		  public static void main(String[] args) {
		    SpringApplication.run(Application.class, args);
		  }
		}
		```

**Spring程序与SpringBoot程序对比**

|       **类/配置文件**       |  **Spring**  | **SpringBoot** |
| :-------------------------: | :----------: | :------------: |
|     **pom文件中的坐标**     | **手工添加** |  **勾选添加**  |
|      **web3.0配置类**       | **手工制作** |     **无**     |
| **Spring/SpringMVC配置类** | **手工制作** |     **无**     |
|         **控制器**          | **手工制作** |  **手工制作**  |

> [!attention]+ 注意事项 
> 基于idea开发SpringBoot程序需要确保**联网**且能够加载到程序框架结构                                      
> 	这一套工程结构包括这两个文件都是从网上下载来的,IDEA走的还是SpringBoot官网那套，所以要联网

- 基于SpringBoot官网创建项目

	<img src="https://article.biliimg.com/bfs/article/ec61fb1139d3abb705776b1f3e685997995c6225.png" alt="image.png" style="zoom:50%;" />

## SpringBoot项目快速启动 

- 前后端分离合作开发 

<img src="https://article.biliimg.com/bfs/article/b79d15ba922aa6a5c4ae7317da62fc333322e0a5.gif" alt="image.png" style="zoom:50%;" />

1. 对SpringBoot项目打包（执行Maven构建指令package) 
2. 执行启动指令 
	`java -jar springboot.jar `


> [!attention]+ 注意事项 
> - jar支持命令行启动需要依赖maven插件支持，请确认打包时是否具有SpringBoot对应的[[java插件#spring-boot-maven-plugin|maven插件 ]]       
> - 只有boot可以打的jar包可以用命令行 -jar这个选项                
> - 还设置了入口程序



> [!important]+ 区别
> - `java springboot.jar`命令是直接执行了`springboot.jar`文件，但是这种方式需要确保当前目录已经设置了正确的classpath，否则可能会出现类找不到的错误。        
> - 而`java -jar springboot.jar`命令则通过使用`-jar`选项来指定要执行的JAR文件，Java会自动将JAR文件添加到classpath中，并执行其中的主类。这种方式更加方便，可以避免一些类路径配置问题。
> - 因此，推荐使用`java -jar springboot.jar `命令来执行Spring Boot应用程序。

## SpringBoot概述 

- SpringBoot是由Pivotal团队提供的全新框架，其设计目的是用来**简化**Spring应用的**初始搭建**以及**开发过程** 

- Spring程序缺点 
	- 配置繁琐 
	- 依赖设置繁琐 
- SpringBoot程序优点 
	- 自动配置 
	- 起步依赖（简化依赖配置） 
	- 辅助功能（内置服务器，……） 

### 起步依赖

![image.png](https://article.biliimg.com/bfs/article/e00a33601139a756608891287cd5a490315fe799.png)



> [!important]+ 核心点
> 1. 在properties中已经写好了各种各样的<font color=#ff0000>最适宜</font>的依赖的版本属性，所以只需写依赖，因为继承就不需要写版本号了，也减少了冲突的可能性
> 2. 像这种`spring-boot-starter-web`叫起步依赖，里面已经写好了所需的很多依赖，减少依赖的配置




- starter 
	- SpringBoot中常见项目名称，定义了当前项目使用的所有项目坐标，以达到<font color=#ff0000>减少依赖配置</font>的目的 
	```xml
	<?xml version="1.0" encoding="UTF-8"?> 
	<project xmlns="http://maven.apache.org/POM/4.0.0"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0https://maven.apache.org/xsd/maven-4.0.0.xsd"> 
		<parent> 
			<groupId>org.springframework.boot</groupId> 
			<artifactId>spring-boot-starter-parent</artifactId> 
			<version>3.1.1</version> 
		</parent> 
		<dependencies> 
			<dependency> 
				<groupId>org.springframework.boot</groupId> 
				<artifactId>spring-boot-starter-web</artifactId> 
			</dependency> 
			<dependency> 
				<groupId>org.springframework.boot</groupId> 
				<artifactId>spring-boot-starter-test</artifactId> 
				<scope>test</scope> 
			</dependency> 
		</dependencies> 
	</project> 
	```

- parent 
	- 所有SpringBoot项目要继承的项目，定义了若干个坐标版本号（依赖管理，而非依赖）,以达到<font color=#ff0000>减少依赖冲突</font>的目的 
	- spring-boot-starter-parent(2.5.0)与spring-boot-starter-parent(2.4.6)共计57处坐标版本不同 

- 实际开发 
	- 使用任意坐标时，仅书写GAV中的G和A,V由SpringBoot提供 
	- 如发生坐标错误，再指定version(要小心版本冲突） 

- 辅助功能 
	```xml
	<?xml version="1.0"encoding="UTF-8"?> 
	<project xmlns="http://maven.apache.org/POM/4.0.0"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xsi:schemaLocation="http://maven.apache.org/pom/4.0.0https://maven.apache.org/xsd/maven-4.0.0.xsd"> 
		<parent> 
			<groupId>org.springframework.boot</groupId> 
			<artifactId>spring-boot-starter-parent</artifactId> 
			<version>3.1.1</version> 
		</parent> 
		<dependencies> 
			<dependency> 
				<groupId>org.springframework.boot</groupId> 
				<artifactId>spring-boot-starter-web</artifactId> 
			</dependency> 
		</dependencies> 
	</project> 
	```

### SpringBoot程序启动

- 启动方式 
```java
@SpringBootApplication 
public class Springboot01QuickstartApplication { 
	public static void main(String[] args){ 
		SpringApplication.run(Springbooto1QuickstartApplication.class,args); 
	}
}
```

- SpringBoot在创建项目时，采用jar的打包方式 (<font color=#ff0000>这里的web也是jar包</font>)
- SpringBoot的<font color=#ff0000>引导类</font>是项目的入口，运行main方法就可以启动项目 

- 使用maven依赖管理变更起步依赖项 
 
```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <!--web起步依赖环境中，排除Tomcat起步依赖--> 
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <!--添加Jetty起步依赖，版本由SpringBoot的starter控制--> 
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jetty</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>jakarta.servlet</groupId>
                    <artifactId>jakarta.servlet-api</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>jakarta.servlet</groupId>
            <artifactId>jakarta.servlet-api</artifactId>
            <version>5.0.0</version> <!--不用5.0.0会报错--> 
        </dependency>
    </dependencies>
```

- Jetty比Tomcat更轻量级，可扩展性更强（相较于Tomcat),谷歌应用引擎（GAE)已经全面切换为Jetty 

# 基础配置

## 配置文件格式 
- 修改服务器端口

	<img src="https://article.biliimg.com/bfs/article/ba3364751090c71a7a589100c08f82356abd8142.png" alt="image.png" style="zoom:75%;" />
- SpringBoot提供了多种属性配置方式 

- application.<font color=#ff0000>properties</font> 
```properties
server.port=80
```

- application.<font color=#ff0000>yml </font>

```yml
server: 
	port: 81 
```

- application.<font color=#ff0000>yaml</font> 

```yaml
server: 
	port: 82 
```

**自动提示功能消失解决方案**
<img src="https://article.biliimg.com/bfs/article/faea702f7f5489e26519acfe6f67d2799b6eff2e.png" alt="image.png" style="zoom:50%;" />


- SpringBoot配置文件加载顺序（了解） 
	- application.<font color=#ff0000>properties</font> > application.<font color=#ff0000>yml</font> > application.<font color=#ff0000>yaml</font> 

> [!attention] 注意事项 
> SpringBoot核心配置文件名为<font color=#ff0000>application</font>                 
> SpringBoot内置属性过多，且所有属性集中在一起修改，在使用时，通过提示键+关键字修改属性 

## yaml 

### yaml语法规则

- YAML(YAML Ain't Markup Language),一种数据序列化格式 
- 优点: 
	- 容易阅读 
	- 容易与脚本语言交互 
	- 以<font color=#ff0000>数据</font>为核心，重数据**轻格式** 
- YAML文件扩展名 
	- <font color=#ff0000>.yml</font>(主流） 
	- .yaml 

<img src="https://article.biliimg.com/bfs/article/991ae369ba2cab312c5dcaa5578b15c7486623d9.png" alt="image.png" style="zoom:75%;" />
- 大小写敏感 
- 属性层级关系使用**多行**描述，每行结尾使用**冒号**结束 
- 使用**缩进**表示层级关系，同层级左侧对齐，只允许使用空格（不允许使用Tab键） 
- 属性值前面<font color=#ff0000>添加空格</font>（属性名与属性值之间使用冒号+空格作为分隔） 
- `# 表示注释 `

- 核心规则:**数据前面要加空格与冒号隔开** 

数组数据在数据书写位置的下方使用减号作为数据开始符号，每行书写一个数据，减号与数据间空格分隔 
```yml
enterprise: 
  name: itcast 
  age: 16 
  tel: 4006184000 
  subject: 
	-Java 
	-前端 
	-大数据 
```

### yaml数据读取

1.  使用@Value读取单个数据，属性名引用方式:<font color=#ff0000>${一级属性名.二级属性名……}</font>
	<img src="https://article.biliimg.com/bfs/article/0f40326e7e45723104ca935154437282df55e87d.png" alt="image.png" style="zoom:50%;" />

2. 封装全部数据到Environment对象
	<img src="https://article.biliimg.com/bfs/article/4f8c5e85f39c01b0250676fa5b1cb1e193ae9565.png" alt="image.png" style="zoom:50%;" />
	`Environment` 是一个Spring框架中的组件，它提供了访问应用程序环境变量和配置属性的方法。
#java读取配置文件 
3. 自定义对象封装指定数据
	<img src="https://article.biliimg.com/bfs/article/a7bbd43f88ad124babbc84eb99a43744709076d5.png" alt="image.png" style="zoom:50%;" />
	这个注解是用来指定配置文件中的属性前缀的。通过设置 `prefix` 属性为 `"enterprise"`，应用程序可以自动将<font color=#ff0000>配置文件</font>中以 `"enterprise"` 为前缀的属性与带有 `@ConfigurationProperties` 注解的<font color=#ff0000>类</font>中的属性进行**绑定**。这样可以方便地将配置文件中的属性值注入到应用程序中使用。

**自定义对象封装数据警告解决方案** 

[[java依赖#spring-boot-configuration-processor|spring-boot-configuration-processor]]

## 多环境启动 
 
如何在一个配置文件中制作多环境?

<img src="https://article.biliimg.com/bfs/article/6f5c68c6f229bc1860d5bbb6703cc4741c306932.png" alt="image.png" style="zoom:50%;" />

### yml多环境启动
```yml
# 设置启动环境
spring:
  profiles:
    active: test
---   #代表一个环境
# 开发
spring:
  config:
    activate:
      on-profile: dev
server:
  port: 80
---
# 生产
spring:
  config:
    activate:
      on-profile: pro
server:
  port: 81
---
# 测试
spring:
  config:
    activate:
      on-profile: test
server:
  port: 82
```


### properties文件多环境启动 
- 主启动配置文件application.properties 
	`spring.profiles.active = pro `
- 环境分类配置文件application-<font color=#ff0000>pro</font>.properties 
	`server.port=80 `
- 环境分类配置文件application-<font color=#ff0000>dev</font>.properties 
	`server.port=81 `
- 环境分类配置文件application-<font color=#ff0000>test</font>.properties 
	`server.port=82 `

### 多环境启动命令格式

- 带参数启动SpringBoot 
```sh
java -jar springboot.jar --spring.profiles.active=test 

java -jar springboot.jar --server.port=88 

java -jar springboot.jar--server.port=88 --spring.profiles.active=test 

```

<img src="https://article.biliimg.com/bfs/article/39330b6a865f72c3810c8a352bb518116dac3ed4.gif" alt="image.png" style="zoom:50%;" />
**参数加载优先顺序**


参看[https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config)

<img src="https://article.biliimg.com/bfs/article/cac063d1972fa0c49a4d3d24e1c26912385cb4a9.png" alt="image.png" style="zoom:75%;" />




### 多环境开发控制

![image.png](https://article.biliimg.com/bfs/article/6925d77af77c9e4e532fec9ff04e9685130fe396.png)



**Maven与SpringBoot多环境兼容**

1. Maven中设置多环境属性

	```xml
	<profiles>    
		<profile>        
			<id>dev_env</id>        
			<properties>            
				<profile.active>dev</profile.active>        
			</properties>        
			<activation>            
				<activeByDefault>true</activeByDefault>        
			</activation>    
		</profile>    
		<profile>        
			<id>pro_env</id>        
			<properties>            
				<profile.active>pro</profile.active>        
			</properties>    
		</profile>    
		<profile>        
			<id>test_env</id>        
			<properties>            
				<profile.active>test</profile.active>        
			</properties> 
		</profile>
	</profiles>
	```

2. SpringBoot中引用Maven属性

```yml
spring:  
  profiles:  
    active: ${profile.active} 
---  
spring:  
  config:  
    activate:  
      on-profile: dev  
server:  
  port: 80  
---  
spring:  
  config:  
    activate:  
      on-profile: pro  
server:  
  port: 81  
---    
spring:  
  config:  
    activate:  
      on-profile: test  
server:  
  port: 82
```

3. 执行Maven打包指令

	- Maven指令执行完毕后，生成了对应的包，其中类参与编译，但是配置文件并没有编译，而是复制到包中 

	<img src="https://article.biliimg.com/bfs/article/c31df0deca2d9faa4306fbb44e6a903eb4dc59c4.png" alt="image.png" style="zoom:50%;" />

	- 解决思路:对于源码中非java类的操作要求加载Maven对应的属性，解析${}占位符 

4. 对资源文件开启对默认占位符的解析 
	```xml
	<build> 
		<plugins> 
			<plugin> 
			<artifactId>maven-resources-plugin</artifactId> 
			<configuration> 
				<encoding>utf-8</encoding> 
				<useDefaultDelimiters>true</useDefaultDelimiters> 
			</configuration> 
			</plugin> 
		</plugins> 
	</build> 
	```

	- Maven打包加载到属性，打包顺利通过

	<img src="https://article.biliimg.com/bfs/article/28f29b4bc9e0ec6d211e91f463eba2c6b93a54f4.png" alt="image.png" style="zoom:50%;" />

## 配置文件分类 

<img src="https://article.biliimg.com/bfs/article/beaf12b1c7ec22b2d4c46f654b591833646f7f02.png" alt="image.png" style="zoom:50%;" />

- SpringBoot中4级配置文件 
	1级: file : `config/application.yml`                             <font color=#ff0000>【最高】 </font>
	2级: file : `application.yml` 
	3级: `classpath : config/application.yml` 
	4级: `classpath : application.yml`                             <font color=#ff0000>【最低】</font> 

- 作用: 
	- 1级与2级留做系统打包后设置通用属性 
	- 3级与4级用于系统开发阶段设置通用属性 


# 整合第三方技术

## 整合JUnit 


**Spring整合JUnit(复习）** 

<img src="https://article.biliimg.com/bfs/article/ec98fa554da57a79ffe0c429f79ebbd3d0baef12.png" alt="image.png" style="zoom:50%;" />

- SpringBoot整合JUnit 
	```java
	@SpringBootTest  //设置JUnit加载的SpringBoot启动类
	class Springboot07JunitApplicationTests { 
			@Autowired 
			private BookService bookService; 
			@Test 
			public void testsave(){ 
				bookService.save(); 
		}
	}
	```



以前Spring的测试配置[[spring#整合JUnit|哪个配置类]]的注解不用写了


![image.png](https://article.biliimg.com/bfs/article/025191c98305bd3e1e928f424397278f34ea806f.png)

1. 这个是<font color=#ff0000>引导类</font>，这个类就起到了配置类的作用，这个类在什么位置，它就会把它**所在的包及其子包**扫描一遍，所以这里`BookServiceImpl`的`@service`才能被加载成bean
2. SpringBoot的测试类会**加载同个包**下面的<font color=#ff0000>引导类</font>
3. 如果测试类在SpringBoot启动类的包或子包中，可以省略启动类的设置，也就是省略classes的设定
	```java
	@SpringBootTest(classes = Springboot07JunitApplication.class)
	class Springboot07JunitApplicationTests {}
	```

## 整合MyBatis

- Spring整合MyBatis（复习）
	- SpringConfig
		- 导入JdbcConfig
		- 导入MyBatisConfig
		```java
		@Configuration 
		@ComponentScan("com.itheima") 
		@PropertySource("classpath:jdbc.properties") 
		@Import({JdbcConfig.class,MyBatisConfig.class}) 
		public class SpringConfig { 
		{ 
		}
		```

	- JDBCConfig
		- 定义数据源（加载properties配置项：driver、url、username、password）

		```java
		public class JdbcConfig { 
			@Value("${jdbc.driver}") 
			private String driver; 
			@Value("${jdbc.ur1}") 
			private String url; 
			@Value("${jdbc.username}") 
			private String userName; 
			@Value("${jdbc.password}") 
			private String password; 
			@Bean 
			public DataSource getDataSource(){ 
				DruidDataSource ds = new DruidDataSource(); 
				ds.setDriverClassName(driver); 
				ds.setUrl(ur1); 
				ds.setUsername(userName); 
				ds.setPassword(password); 
				return ds; 
				}
			}
		```
	- MyBatisConfig
		- 定义SqlSessionFactoryBean
		- 定义映射配置
		```java
		@Bean 
		public SqlSessionFactoryBean getSqlSessionFactoryBean(DataSource dataSource){ 
			SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean(); 
			ssfb.setTypeAliasesPackage("com.itheima.domain"); 
			ssfb.setDataSource(dataSource); 
			return ssfb; 
		}
		@Bean 
		public MapperScannerConfigurer getMapperScannerConfigurer(){ 
			MapperScannerConfigurer msc = new MapperScannerConfigurer(); 
			msc.setBasePackage("com.itheima.dao"); 
			return msc; 
		}
		```


<img src="https://article.biliimg.com/bfs/article/1d2ca86b1945383fa90aa2a6e06885378bbf9b6f.png" alt="image.png" style="zoom:50%;" />

- 设置数据源参数
	```yml
	spring:
	  datasource:
	    username: root
	    url: jdbc:mysql:///ssm_db
	    driver-class-name: com.mysql.cj.jdbc.Driver
	    password: 701121
	    type: com.alibaba.druid.pool.DruidDataSource
	```


BookDao.java文件
```java
@Mapper  //动态生成接口的<font color=#ff0000>实现类</font>
public interface BookDao {

  @Select("select *from tbl_book where id=#{id}")
  public Book getById(Integer id);
}

```

以前在SpringConfig.java文件有这句注释
`@ComponentScan({"project.service","project.dao"})`
而SpringBoot没有配置文件就用@Mapper作为代替映射


```java
@SpringBootTest
class Springboot08MybatisApplicationTests {
  @Autowired
  private BookDao bookDao;
  @Test
  public void getById() {
    Book book = bookDao.getById(2);
    System.out.println("book = " + book);
  }
}
```


## 基于SpringBoot的SSM整合案例 

1. pom.xml 
	 配置起步依赖，必要的资源坐标（druid) 
2. application.yml 
	设置数据源、端口等 
3. 配置类 
	 全部删除 
4. dao 
	设置@Mapper 
5. 测试类 
6. 页面 
	放置在resources目录下的static目录中 



# Spring Cache


## 介绍
`Spring Cache` 是一个框架，实现了**基于注解**的缓存功能，只需要简单地加一个注解，就能实现缓存功能。
`Spring Cache` 提供了一层**抽象**，底层可以切换**不同的缓存实现**，例如：

- EHCache
- Caffeine
- Redis(常用)

[[java依赖#canal-spring-boot-starter|导入依赖]]



常用注解

| **注解**         |**说明**|
|:--------------:|:-----------------------------------------------------------:|
|`@EnableCaching`|开启缓存注解功能，通常加在**启动类**上|
|`@Cacheable`| 在方法执行前先查询缓存中是否有数据，如果有数据，则直接返回缓存数据；如果没有缓存数据，调用方法并将方法返回值放到缓存中 |
|`@CachePut`|将方法的返回值放到缓存中|
|`@CacheEvict`|将一条或多条数据从缓存中删除|

在spring boot项目中，使用缓存技术只需在项目中导入相关缓存技术的依赖包，并在启动类上使用@EnableCaching开启缓存支持即可。
例如，使用Redis作为缓存技术，只需要导入Spring data Redis的maven坐标即可。


**引导类上加@EnableCaching:**

```java
@Slf4j
@SpringBootApplication
@EnableCaching//开启缓存注解功能
public class CacheDemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(CacheDemoApplication.class,args);
        log.info("项目启动成功...");
    }
}
```


**2). @CachePut注解**
**@CachePut 说明：**

- 作用: 将方法返回值，放入缓存
- value: 缓存的名称, 每个缓存名称下面可以有很多key
- key: 缓存的key ----------> 支持Spring的表达式语言SPEL语法

**在save方法上加注解@CachePut**
当前`UserController`的save方法是用来保存用户信息的，我们希望在该用户信息保存到数据库的同时，也往缓存中缓存一份数据，我们可以在save方法上加上注解 @CachePut，用法如下：

```java
    @PostMapping
    @CachePut(cacheNames = "userCache",key = "#user.id") //如果使用Spring Cache缓存数据，key的生成：userCache::1
	//    @CachePut(cacheNames = "userCache",key = "#result.id")
    public User save(@RequestBody User user){
        userMapper.insert(user);
        return user;
    }
```


**3). @Cacheable注解**

**@Cacheable 说明:**
- 作用: 在方法执行前，spring先查看缓存中是否有数据，如果有数据，则直接返回缓存数据；若没有数据，调用方法并将方法返回值放到缓存中
- value: 缓存的名称，每个缓存名称下面可以有多个key
- key: 缓存的key ----------> 支持Spring的表达式语言SPEL语法

**在getById上加注解@Cacheable**

```java
	@GetMapping
    @Cacheable(cacheNames = "userCache",key="#id")
    public User getById(Long id){
        User user = userMapper.getById(id);
        return user;
    }
```

**4). @CacheEvict注解**
- **@CacheEvict 说明：**
- 作用: 清理指定缓存
- value: 缓存的名称，每个缓存名称下面可以有多个key
- key: 缓存的key ----------> 支持Spring的表达式语言SPEL语法

**在 delete 方法上加注解@CacheEvict**

```java
	@DeleteMapping
    @CacheEvict(cacheNames = "userCache",key = "#id")//删除某个key对应的缓存数据
    public void deleteById(Long id){
        userMapper.deleteById(id);
    }

	@DeleteMapping("/delAll")
    @CacheEvict(cacheNames = "userCache",allEntries = true)//删除userCache下所有的缓存数据
    public void deleteAll(){
        userMapper.deleteAll();
    }
```

**重启服务,通过swagger接口文档测试，访问UserController的deleteAll()方法**

# Spring Task

## 介绍

**Spring Task** 是Spring框架提供的**任务调度工具**，可以按照约定的时间**自动执行**某个代码逻辑。
**定位：** 定时任务框架
**作用：** 定时自动执行某段Java代码

为什么要在Java程序中使用Spring Task？

**应用场景：**
1. 信用卡每月还款提醒
2. 银行贷款每月还款提醒
3. 火车票售票系统处理未支付订单
4. 入职纪念日为用户发送通知

**强调：** 只要是需要定时处理的场景都可以使用Spring Task

## cron表达式

**cron表达式**其实就是一个字符串，通过cron表达式可以**定义任务触发的时间**
**构成规则：** 分为6或7个<font color=#ff0000>域</font>，由空格分隔开，每个域代表一个含义
每个域的含义分别为：秒、分钟、小时、日、月、周、年(可选)
**举例：**

2022年10月12日上午9点整 对应的cron表达式为：**0 0 9 12 10 ? 2022**

<img src="https://article.biliimg.com/bfs/article/d9a567354a2015af5d6ad9c97966d16b38716159.png" alt="image-20221218184412491" style="zoom:50%;" />

**说明：** 一般**日**和**周**的值不同时设置，其中一个设置，另一个用？表示。

为了描述这些信息，提供一些特殊的字符。这些具体的细节，我们就不用自己去手写，因为这个cron表达式，它其实有在线生成器

cron表达式在线生成器：[在线Cron表达式生成器](https://cron.qqe2.com/)

**通配符：**

`*` 表示所有值；
`?` 表示未说明的值，即不关心它为何值；
`-` 表示一个指定的范围；
`,` 表示附加一个可能值；
`/` 符号前表示开始时间，符号后表示每次递增的值；

**cron表达式案例：**

| 表达式                    |  描述                                    |
|:----------------------:|:--------------------------------------:|
|`0 */1 * * * ?` |  每隔1分钟执行一次                             |
|  `0 0 5-15 * * ?`      |  每天5-15点整点触发                           |
|`0 0/3 * * * ?`|  每三分钟触发一次                              |
|  `0 0-5 14 * * ?`      |  在每天下午2点到下午2:05期间的每1分钟触发               |
|  `0 0/5 14 * * ?`      |  在每天下午2点到下午2:55期间的每5分钟触发               |
|  `0 0/5 14,18 * * ?`   |  在每天下午2点到2:55期间和下午6点到6:55期间的每5分钟触发     |
|  `0 0/30 9-17 * * ?`   |  朝九晚五工作时间内每半小时                         |
|  `0 0 10,14,16 * * ?`  |  每天上午10点，下午2点，4点                       |  

## 入门案例

1. 导入maven坐标 spring-context（已存在）
2. 启动类添加注解 @EnableScheduling 开启任务调度
3. 自定义定时任务类

**编写定时任务类：**
进入sky-server模块中
```java
/**
 * 自定义定时任务类
 */
@Component
@Slf4j
public class MyTask {

    /**
     * 定时任务 每隔5秒触发一次
     */
    @Scheduled(cron = "0/5 * * * * ?")
    public void executeTask(){
        log.info("定时任务开始执行：{}",new Date());
    }
}
```

**开启任务调度：**
启动类添加注解 @EnableScheduling

```java
@SpringBootApplication
@EnableTransactionManagement //开启注解方式的事务管理
@Slf4j
@EnableCaching
@EnableScheduling
public class SkyApplication {
    public static void main(String[] args) {
        SpringApplication.run(SkyApplication.class, args);
        log.info("server started");
    }
}
```


# Swagger

## 介绍

Swagger 是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务(<https://swagger.io/>)。 它的主要作用是：

1. 使得前后端分离开发更加方便，有利于团队协作
2. 接口的文档在线自动生成，降低后端开发人员编写接口文档的负担
3. 功能测试 


`Spring`已经将Swagger纳入自身的标准，建立了Spring-swagger项目，现在叫Springfox。通过在项目中引入Springfox ，即可非常简单快捷的使用Swagger。

`knife4j`是为Java MVC框架集成Swagger生成Api文档的增强解决方案,前身是`swagger-bootstrap-ui`,取名kni4j是希望它能像一把匕首一样小巧,轻量,并且功能强悍!
目前，一般都使用knife4j框架。

[[java依赖#knife4j-spring-boot-starter|引入依赖]]

```java
    @Bean
    public Docket docket1() {
        log.info("准备生成接口文档...");
        ApiInfo apiInfo = new ApiInfoBuilder()
                .title("苍穹外卖项目接口文档")
                .version("2.0")
                .description("苍穹外卖项目接口文档")
                .build();
        Docket docket = new Docket(DocumentationType.SWAGGER_2)
                .groupName("管理端接口") // groupName分组
                .apiInfo(apiInfo)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.sky.controller.admin"))
                .paths(PathSelectors.any())
                .build();
        return docket;
    }

    @Bean
    public Docket dockete() {
        log.info("准备生成接口文档...");
        ApiInfo apiInfo = new ApiInfoBuilder()
                .title("苍穹外卖项目接口文档")
                .version("2.0")
                .description("苍穹外卖项目接口文档")
                .build();
        Docket docket = new Docket(DocumentationType.SWAGGER_2)
                .groupName("用户端接口")
                .apiInfo(apiInfo)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.sky.controller.user"))
                .paths(PathSelectors.any())
                .build();
        return docket;
    }
```

访问测试

接口文档访问路径为 http://ip:port/doc.html ---> http://localhost:8080/doc.html


设置静态资源映射，否则接口文档页面无法访问
WebMvcConfiguration.java

```java
/**
     * 设置静态资源映射
     * @param registry
*/
protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/doc.html").addResourceLocations("classpath:/META-INF/resources/");
        registry.addResourceHandler("/webjars/**").addResourceLocations("classpath:/META-INF/resources/webjars/");
}
```


<img src="https://article.biliimg.com/bfs/article/2bc5b4bb73d5d47881dc69a1d75ce6d8057e6024.png" alt="image-20221107173033632" style="zoom:50%;" />

接口测试:测试登录功能
<img src="https://article.biliimg.com/bfs/article/834d0ff777fd5313bba2757bdb82741d1c54edfb.png" alt="image-20221107173137506" style="zoom:70%;" />

**思考：** 通过 Swagger 就可以生成接口文档，那么我们就不需要 Yapi 了？

1、Yapi 是设计阶段使用的工具，管理和维护接口
2、Swagger 在开发阶段使用的框架，帮助后端开发人员做后端的接口测试
