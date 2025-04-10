

# 分模块开发与设计 
## 分模块开发概念和意义
<img src="https://article.biliimg.com/bfs/article/76b86abd110ce3b242411f2cf3f2713940043bde.png" alt="image.png" style="zoom:50%;" />

银行的功能越来越多
我们做商品开发的团队一定要将它所制作的domain的功能给其他团队使用，这里要进行模块间的共享
<img src="https://article.biliimg.com/bfs/article/c71d9aed0a372522e1d411fed7c294311bf09d19.png" alt="image.png" style="zoom:50%;" />

`domain`的功能抽取出来，所有团队都可以用，通过一坐标就可以导入

<img src="https://article.biliimg.com/bfs/article/a45132ea7c85b6a00add174c0da585f6aaeea5c5.png" alt="image.png" style="zoom:75%;" />

- 将原始模块按照功能拆分成若干个子模块，方便模块间的相互调用，接口共享

<img src="https://article.biliimg.com/bfs/article/b4781ba0906aee013501790931fc7dfc75a25637.png" alt="image.png" style="zoom:75%;" />

## 分模块开发

1. 把domain从当前的模块中拆分出来
domain的坐标(pojo模块的pom.xml代码)
```xml
  <groupId>java_project</groupId>
  <artifactId>maven_03_pojo</artifactId>
  <version>1.0-SNAPSHOT</version>
```
ssm的pom.xml引入模块代码
```xml
    <!--pojo模块引入-->
    <dependency>
      <groupId>java_project</groupId>
      <artifactId>maven_03_pojo</artifactId>
      <version>1.0-SNAPSHOT</version>
    </dependency>
```

📢pojo模块一定通过maven指令安装模块到本地仓库（**install**指令）

![image.png](https://article.biliimg.com/bfs/article/8acc562031a60fd71d34991962976e12562ec68a.png)


> [!attention]+ 注意事项 
> 团队内部开发需要发布模块功能到团队内部可共享的仓库中（<font color=#ff0000>私服</font>） 

# 依赖管理 

## 依赖传递 
- 依赖具有传递性 
	- 直接依赖：在当前项目中通过依赖配置建立的依赖关系 
	- 间接依赖：被资源的资源如果依赖其他资源，当前项目间接依赖其他资源 

<img src="https://article.biliimg.com/bfs/article/db4243c7259157da66cff962e0864a4451aacefe.png" alt="image.png" style="zoom:50%;" />

- 依赖传递冲突问题 
	- 路径优先：当依赖中出现相同的资源时，层级越**深**，优先级越**低**，层级越浅，优先级越高 
	- 声明优先：当资源在**相同层级**被依赖时，配置顺序靠前的覆盖配置顺序靠后的 
	- 特殊优先：当同级配置了相同资源的不同版本，**后配置**的覆盖先配置的 

	<img src="https://article.biliimg.com/bfs/article/63dc1b52c90b82df754233e5702d786fd7026541.png" alt="image.png" style="zoom:50%;" />


方式
#IDEA按钮 
点击这个按钮可以查看模块的依赖情况
	<img src="https://article.biliimg.com/bfs/article/0b29702b1411a853f0f1cc8f3f9e028917548a84.png" alt="image.png" style="zoom:50%;" />

## 可选依赖 

- 可选依赖指**对外隐藏**当前所依赖的资源——不透明 
	````xml
	<dependency> 
		<groupId>java_project</groupId> 
		<artifactId>maven_03_pojo</artifactId> 
		<version>1.0-SNAPSHOT</version> 
		<!--可选依赖是隐藏当前工程所依赖的资源，隐藏后对应资源将不具有依赖传递性--> 
		<optional>true</optional> 
	</dependency> 
	````

是模块作者不想让**调用方**看到所依赖的资源
## 排除依赖 

- 排除依赖指主动断开依赖的资源，被排除的资源无需指定版本一 不需要
```xml
<dependency> 
	<groupId>java_project</groupId> 
	<artifactId>maven_04_dao</artifactId> 
	<version>1.0-SNAPSHOT</version> 
<!--排除依赖是隐藏当前资源对应的依赖关系--> 
	<exclusions> 
		<exclusion> 
			<groupId>log4j</groupId> 
			<artifactId>log4j</artifactId> 
		</exclusion> 
		<exclusion> 
			<groupId>org.mybatis</groupId> 
			<artifactId>mybatis</artifactId> 
		</exclusion> 
	</exclusions> 
</dependency>
```
- 排除依赖资源仅指定GA即可，无需指定V 

是调用方不想要某个**依赖的间接依赖**

# 聚合与继承 

## 聚合
- 聚合：将多个模块组织成一个整体，**同时进行项目构建的过程**称为聚合 
- 聚合工程：通常是一个不具有业务功能的 **“空”** 工程（有且仅有一个pom文件） 
- 作用：使用聚合工程可以将多个工程编组，通过对聚合工程进行构建，实现对所包含的模块进行**同步构建** 
	- 当工程中某个模块发生更新（变更）时，必须保障工程中与已更新模块关联的模块**同步更新**，此时可以使用聚合工程来解决批量模块同步构建的问题 

<img src="https://article.biliimg.com/bfs/article/d1f2fc78dfd3f507d7f58bd20475f50a02074b9f.png" alt="image.png" style="zoom:50%;" />

1.创建Maven模块，设置打包类型为pom 
```xml
<packaging>pom</packaging> 
```

> [!attention]+ 注意事项
> 每个maven工程都有对应的打包方式，默认为jar,web工程打包方式为war 

2.设置当前聚合工程所包含的子模块名称 
```xml
<modules> 
	<module>../maven_ssm</module> 
	<module>../maven_pojo</module> 
	<module>../maven_dao</module> 
</modules> 
```

> [!attention]+ 注意事项 
> 聚合工程中所包含的模块在进行构建时会根据模块间的依赖关系设置**构建顺序**，与聚合工程中模块的配置书写位置无关 
> 参与聚合的工程无法向上感知是否参与聚合，只能向下配置哪些模块参与本工程的聚合 

<img src="https://article.biliimg.com/bfs/article/a464b79c2177639273f45e71663a2711a3acb151.png" alt="image.png" style="zoom:75%;" />

## 继承

<img src="https://article.biliimg.com/bfs/article/d194d0e0099906dd4ad776b7a3408a8d6a8c8dfa.png" alt="image.png" style="zoom:50%;" />

- 概念：继承描述的是两个工程间的关系，与java中的继承相似，子工程可以继承父工程中的配置信息，常见于依赖 关系的继承 
- 作用： 
	- 简化配置 
	- 减少版本冲突 

1. 配置子工程中**可选**的依赖关系 (子模块要继承要自己写，但是不用写版本)
	```xml
	<dependencyManagement> 
		<dependencies> 
			<dependency> 
				<groupId>com.alibaba</groupId> 
				<artifactId>druid</artifactId> 
				<version>1.1.16</version> 
			</dependency> 
		...... 
		</dependencies> 
	</dependencyManagement> 
	```

2. 在子工程中配置当前工程所继承的父工程 
	```xml
	<!--定义该工程的父工程--> 
	<parent> 
		<groupId>java_project</groupId> 
		<artifactId>maven_parent</artifactId> 
		<version>1.0-SNAPSHOT</version> 
		<!--填写父工程的pom文件--> 
		<relativePath>../maven_parent/pom.xml</relativePath> 
	</parent> 
	```

3. 在子工程中配置使用父工程中可选依赖的坐标 
```xml
<dependencies> 
	<dependency> 
	<groupId>com.alibaba</groupId> 
	<artifactId>druid</artifactId> 
	</dependency> 
</dependencies> 
```

> [!attention]+ 注意事项 
> 子工程中使用父工程中的可选依赖时，仅需要提供群组id和项目id,无需提供版本，版本由父工程统一提供，避免版本冲突 
> 子工程中还可以定义父工程中没有定义的依赖关系 

**聚合与继承的区别** 
- 作用 
	- 聚合用于快速构建项目 
	- 继承用于快速配置 
- 相同点： 
	- 聚合与继承的`pom.xm1`文件打包方式均为pom,可以将两种关系制作到同一个pom文件中 
	- 聚合与继承均属于设计型模块，并无实际的模块内容 
- 不同点： 
	- 聚合是在当前模块中配置关系，聚合可以感知到参与聚合的模块有哪些 
	- 继承是在子模块中配置关系，父模块无法感知哪些子模块继承了自己 





# 属性管理 

## 属性 

<img src="https://article.biliimg.com/bfs/article/51d2fd1df06a497a30959f12f28bb5e457bd3c0c.png" alt="image.png" style="zoom:75%;" />

1. 定义属性 

	```xml
	<!--定义自定义属性--> 
	<properties> 
		<spring.version>5.2.10.RELEASE</spring.version> 
		<junit.version>4.12</junit.version> 
	</properties> 
	```

2. 引用属性 
	```xml
	<dependency> 
		<groupId>org.springframework</groupId> 
		<artifactId>spring-context</artifactId> 
		<version>${spring·version}</version> 
	</dependency> 
	```


### 步骤 : 资源文件引用属性 
1. 定义属性 
	```xml
	<!--定义自定义属性--> 
	<properties> 
		<spring.version>5.2.10.RELEASE</spring.version> 
		<junit.version>4.12</junit.version> 
		<jdbc.url>jdbc:mysql://127.0.0.1:3306/ssm_db</jdbc.url> 
	</properties> 
	```

2. 配置文件中引用属性 
```properties
jdbc.driver=com.mysql.jdbc.Driver 
jdbc.url=${jdbc.url} 
jdbc.username=root 
jdbc.password=root 
```

3. 开启资源文件目录加载属性的过滤器 
	```xml
	<build> 
		<resources> 
			<resource> 
				<directory>${project.basedir}/src/main/resources</directory> 
				<filtering>true</filtering> 
			</resource> 
		</resources> 
	</build> 
	```
	`${project.basedir}`代表项目根目录的路径如果`maven_02_ssm`、`maven_03_pojo`和`maven_04_dao`这三个模块都继承了`maven_01_parent`，那么它们将继承`maven_01_parent`模块中的配置信息，包括构建过程中的资源配置。这意味着这三个子模块都可以通过`${project.basedir}`来引用各自根目录下的资源目录。
	这段配置代码是用来配置构建过程中资源的使用。具体来说，`<directory>`元素指定了要使用的资源目录，这里是`maven_02_ssm/src/main/resources`目录。`<filtering>`元素设置为`true`表示对资源文件进行过滤，可以在构建过程中替换文件中的占位符为实际的值。这个注解的目的是确保在构建过程中正确使用和处理项目的资源文件。

4. 配置maven打war包时，忽略web.xm1检查 
	```xml
	<plugin> 
		<groupId>org.apache.maven.plugins</groupId> 
		<artifactId>maven-war-plugin</artifactId> 
		<version>3.2.3</version> 
		<configuration> 
			<failonMissingwebXml>false</failonMissingwebxml> 
		</configuration> 
	</plugin> 
	```


**其他属性**

| **属性分类** |        **引用格式**        |          **示例**           |
| :----------: | :------------------------: | :-------------------------: |
|  自定义属性  |      ${自定义属性名}       |      ${spring.version}      |
|   内置属性   |       ${内置属性名}        |   ${basedir}  ${version}    |
| Setting属性  |     ${setting.属性名}      | ${settings.localRepository} |
| Java系统属性 | ${系统属性分类.系统属性名} |        ${user.home}         |
| 环境变量属性 |   ${env.环境变量属性名}    |      ${env.JAVA_HOME}       |

## 版本管理 

![image.png](https://article.biliimg.com/bfs/article/0c72d0c8365b95024bf1503764dfe8024e7be50b.png)

- 工程版本： 
	- `SNAPSHOT`(快照版本） 
		- 项目开发过程中临时输出的版本，称为快照版本 
		- 快照版本会随着开发的进展不断更新 
	- `RELEASE`(发布版本） 
		- 项目开发到进入阶段里程碑后，向团队外部发布较为稳定的版本，这种版本所对应的构件文件是稳定的，即便进行功能的 
		- 后续开发，也不会改变当前发布版本内容，这种版本称为发布版本 
- 发布版本 
	- alpha版 
	- beta版 
	- 纯数字版 

# 多环境配置与应用 
## 多环境开发

<img src="https://article.biliimg.com/bfs/article/f16d11ea929a5d0ee321bbf2eeb1be5518022dba.png" alt="image.png" style="zoom:50%;" />

- maven提供配置多种环境的设定，帮助开发者使用过程中**快速切换环境**

1. 定义多环境
	```xml
	 <!-- 配置多环境-->
	    <profiles>
	        <!--开发环境-->
	        <profile>
	            <id>env_dep</id> <!--标识环境名-->
	            <properties>
	                <jdbc.url>jdbc:mysql://127.0.0.1:3306/ssm_db</jdbc.url>
	            </properties>
	            <!-- 设置默认启动-->
	            <activation>
	                <activeByDefault>true</activeByDefault>
	            </activation>
	        </profile>
	        <!--  生产环境 -->
	        <profile>
	            <id>env_pro</id>
	            <properties>
	                <jdbc.url>jdbc:mysql://65.13.40.251:3306/ssm_db</jdbc.url>
	            </properties>
	        </profile>
	        <!--  测试环境-->
	        <profile>
	            <id>env_test</id>
	            <properties>
	                <jdbc.url>jdbc:mysql://61.41.134.129/ssm_db</jdbc.url>
	            </properties>
	        </profile>
	    </profiles>
	```

2. 使用多环境（构建过程)

	`mvn 指令 -P 环境定义id `
	
	范例： 
	
	`mvn install -P pro_env `

	<img src="https://article.biliimg.com/bfs/article/26b34053925011b7fabdcc5a4311c20a0e863a26.png" alt="image.png" style="zoom:50%;" />




## 跳过测试 

- 应用场景 
	- 功能更新中并且没有开发完毕 
	- 快速打包 
	- ...

#IDEA按钮
1. 用IDEA的图标跳过测试(不能筛选，全部排除)

	<img src="https://article.biliimg.com/bfs/article/a5b635e1b296b90eadafa279212c653376973ec5.png" alt="image.png" style="zoom:75%;" />


2. 用java插件
	```xml
	<plugins>
	            <plugin>
	                <groupId>org.apache.maven.plugins</groupId>
	                <artifactId>maven-surefire-plugin</artifactId>
	                <version>2.12.4</version>
	                <configuration>
	                    <!--排除测试-->
	                    <skipTests>false</skipTests>
	                    <!--排除掉不参与测试的内容-->
	                    <excludes>
	                        <exclude>**/BookServiceTest.java</exclude>
	                    </excludes>
	                </configuration>
	            </plugin>
	        </plugins>
	```

3. 用maven命令行
	- 跳过测试 

		`mvn 指令 -D skipTests` 
		
	- 范例： 

		`mvn install -D skipTests `

# 私服 

## 私服简介 

<img src="https://article.biliimg.com/bfs/article/7232ff226ac2c24cb3cc30471f85096686ccfb42.png" alt="image.png" style="zoom:50%;" />


私服是一台独立的服务器，用于解决团队内部的资源共享与资源同步问题

- Nexus
	- Sonatype公司的一款maven私服产品
	- 下载地址：[Nexus](https://help.sonatype.com/repomanager3/download)

### Nexus安装与启动

- 启动服务器（命令行启动）
    - `nexus.exe /run nexus`
- 访问服务器（默认端口：8081）
    - [http://localhost:8081](http://localhost:8081)
- 修改基础配置信息
    - 安装路径下etc目录中`nexus-default.properties`文件保存有nexus基础配置信息，例如默认访问端口。
- 修改服务器运行配置信息
    - 安装路径下bin目录中`nexus.vmoptions`文件保存有nexus服务器启动对应的配置信息，例如默认占用内存空间。

## 私服仓库分类 

### 私服资源操作流程分析

<img src="https://article.biliimg.com/bfs/article/674ad94bfedb918f7b26e5bd062030a79c1e8415.png" alt="image.png" style="zoom:50%;" />

### 私服仓库分类

| **仓库类别** | **英文名称** |             **功能**              | **关联操作** |
| :----------: | :----------: | :-------------------------------: | :----------: |
| **宿主仓库** |  **hosted**  |**保存自主研发 **+**第三方资源**|   **上传**   |
| **代理仓库** |  **proxy**   |       **代理连接中央仓库**        |   **下载**   |
|  **仓库组**  |  **group**   |    **为仓库编组简化下载操作**     |   **下载**   |

## 资源上传与下载 

<img src="https://article.biliimg.com/bfs/article/f7d5b76725ca5b994da3a56aea2010e3463e2ad7.png" alt="image.png" style="zoom:50%;" />

这里的`访问私服的用户名/密码`、`上传的位置（宿主地址）` 、`访问私服的用户名/密码` 、`下载的地址（仓库组地址）`的配置都是在**本地仓库**中(因为本地仓库与私服直接打交道) 


### 本地仓库访问私服权限设置
- 配置位置（setting.xml文件中）
	```xml
	    <!-- 配置访问私服的权限 -->
	    <server>
	      <id>java_project-snapshot</id>
	      <username>admin</username>
	      <password>1998.2.21</password>
	    </server>
	    <server>
	      <id>java_project-release</id>
	      <username>admin</username>
	      <password>1998.2.21</password>
	    </server>
	```

### 本地仓库访问私服地址设置

- 配置位置（setting.xml文件中）
```xml
    <!-- 私服的访问路径 -->
    <mirror>
      <id>maven-public</id> <!-- 这个是私服仓库组的id -->
      <mirrorOf>*</mirrorOf> <!-- 匹配路径-->
      <url>http://localhost:8081/repository/maven-public/</url>
    </mirror>
    <mirror>
```


### 工程上传到私服服务器设置
当前项目要发布到哪一个私服仓库
- 配置位置（工程pom文件中） 
	```xml
	 <!--配置当前工程保存在私服中的具体位置-->
	    <distributionManagement>
	         <repository>
	             <id>java_project-release</id>
	             <url>http://localhost:8081/repository/java_project-release/</url>
	         </repository>
	         <snapshotRepository>
	             <id>java_project-snapshot</id>
	             <url>http://localhost:8081/repository/java_project-snapshot/</url>
	         </snapshotRepository>
	    </distributionManagement>
	```

- 发布命令
	`mvn deploy `

这个`<version>1.0-RELEASE</version>`决定deploy到私服是RELEASE版本还是SNAPSHOT
```xml
    <groupId>java_project</groupId>
    <artifactId>maven_01_parent</artifactId>
    <version>1.0-RELEASE</version>
    <packaging>pom</packaging>
```


### 私服访问中央服务器设置

- 配置位置（nexus服务器页面设置）

	将原始的**代理仓库地址**从`https://repo1.maven.org/maven2/`换成`http://maven.aliyun.com/nexus/content/groups/public`

<img src="https://article.biliimg.com/bfs/article/f50c94aa556fdcf5f6b1746ea659766fab6eb081.png" alt="image.png" style="zoom:50%;" />
