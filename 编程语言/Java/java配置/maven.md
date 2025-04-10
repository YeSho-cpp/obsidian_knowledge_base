## Maven 
### Maven 简介
- Maven是专门用于管理和构建Java项目的工具，它的主要功能有：
	- 提供了一套标准化的项目结构
	- 提供了一套标准化的构建流程（编译，测试，打包，发布……）
	- 提供了一套依赖管理机制

![image.png](https://article.biliimg.com/bfs/article/2491af7a14f627f827233966b3c6f6edf692759d.png)

<font color=#ff0000>Maven提供了一套标准化的项目结构，所有IDE使用Maven构建的项目结构完全一样，所有IDE创建的Maven项目可以通用</font>
- Maven是专门用于管理和构建Java项目的工具，它的主要功能有： 
	- 提供了一套标准化的项目结构
	- 提供了一套标准化的构建流程（编译，测试，打包，发布……）
	- 提供了一套依赖管理机制
- 标准化的构建流程

![image.png](https://article.biliimg.com/bfs/article/3e5131377d00d6f2c29a7ac0f1862489c20ee321.png)

**Maven提供了一套简单的命令来完成项目构建**
- 依赖管理
	- 依赖管理其实就是管理你项目所依赖的第三方资源 (jar包、插件…)
	![image.png](https://article.biliimg.com/bfs/article/1764241a995d7609058d0b05d89339df11515bda.png)
	

**Maven 模型：**
![image.png](https://article.biliimg.com/bfs/article/36a3edec536738464c5f8c8b5181d1c3825d67aa.png)


**maven坐标**是指 Maven 项目的坐标 ^zuobiao

Maven 项目坐标通常包括以下信息：

1. groupId：该项目所属的组 ID，通常是公司或组织的名称。
2. artifactId：该项目在组 ID 中的唯一标识符，通常是项目名称。
3. version：该项目的版本号。


使用 Maven，我们可以在项目的 pom.xml 文件中配置依赖项，指定它们的坐标信息。这样，当构建项目时，Maven 会自动下载并安装这些依赖项。

**Maven 作用：**
- 标准化的项目结构
- 标准化的构建流程
- 方便的依赖管理

![image.png](https://article.biliimg.com/bfs/article/f0e6cec0c4cf4d6251ea3c4eee8a9fbe7d3c37c9.png)
- 仓库分类：
	- 本地仓库：自己计算机上的一个目录
	- 中央仓库：由Maven团队维护的全球唯一的仓库
		- 地址：https://repo1.maven.org/maven2/
	- 远程仓库(私服)：一般由公司团队搭建的私有仓库
- 当项目中使用坐标引入对应依赖jar包后，首先会查找本地仓库中是否有对应的jar包：
	- 如果有，则在项目直接引用;
	- b如果没有，则去中央仓库中下载对应的jar包到本地仓库。
- 还可以搭建远程仓库，将来jar包的查找顺序则变为：
	- 本地仓库 -> 远程仓库 -> 中央仓库

### Maven 安装配置
1. 解压 apache-maven-3.6.1.rar 既安装完成 
2. 配置环境变量 MAVEN_HOME 为安装路径的bin目录 
3. 配置本地仓库：修改 conf/settings.xml 中的 `<localRepository>` 为一个指定目录 
4. 配置阿里云私服：修改 conf/settings.xml 中的 `<mirrors>`标签，为其添加如下子标签：

```xml
<mirror>
     <id>nexus-aliyun</id>
     <mirrorOf>*</mirrorOf>
     <name>Nexus aliyun</name>
     <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```

### IDEA 配置 Maven
- 什么是坐标？
	- Maven 中的坐标是<font color=#ff0000>资源的唯一标识</font>
	- 使用坐标来定义项目或引入项目中需要的依赖
- Maven 坐标主要组成
	- `groupId`：定义当前Maven项目隶属组织名称（通常是域名反写，例如：com.itheima）
	- `artifactId`：定义当前Maven项目名称（通常是模块名称，例如 order-service、goods-service）
	- `version`：定义当前项目版本号
	![image.png](https://article.biliimg.com/bfs/article/c9eb3c60e8c3b135c2c254b090388c245e4c6500.png)

### Maven 基本使用

#### Maven 常用命令
- `compil`e ：编译项目的源码，一般来说编译的是src/main/java目录下的java文件至项目输出的主classpath目录中
- `clean`：清理
- `test`：使用单元测试框架运行测试，测试代码不会被打包或部署；
- `package`：接收编译好的代码，打包成可以发布的格式，如jar和war；打包后的文件通常会存放在项目的target目录中。这个命令只是将代码打包成可发布形式，并不会将包安装到本地或远程仓库。
- `deploy`：将包安装到**远程仓库**，供其他开发人员或项目使用。与install命令不同的是，deploy命令会将包上传到远程仓库，以便其他开发人员可以在不同的环境中获取和使用该包。该命令通常在构建和发布过程中使用，以便其他团队成员可以及时获取最新的版本并进行开发、测试或部署。

#### classpath类路径

classpath类路径在 `Spring Boot` 中既指程序在打包前的`/java/`目录加上`/resource`目录，也指程序在打包后生成的`/classes/`目录。两者实际上指的是同一个目录，里面包含的文件内容一模一样

<img src="https://article.biliimg.com/bfs/article/8dd4eb5c281ea5b9785a7eeb96e9d4a888cd49ae.png" alt="image.png" style="zoom:50%;" />

二、获取classpath路径
以下两种方式均可，但是并不能用于生产环境，因为当我们把程序打成jar包时，由于jar包本质是压缩文件，无法被直接打包，所以生成的路径中会含有感叹号!导致路径定位错误，例如：`jar!/BOOT-INF/classes!/application.yml (No such file or directory)`

```java
// 方式一：
String path1 = ClassUtils.getDefaultClassLoader().getResource("").getPath();

// 方式二：
String path2 = ResourceUtils.getURL("classpath:").getPath();

```

此时，如果我们想要读取`jar`包内的文件，可以采取第 3 种方式不读取路径、直接读取文件流：

```java
// 方式 三
InputStream input = ClassUtils
        .getDefaultClassLoader()
        .getResourceAsStream("application.yml");
Reader reader = new InputStreamReader(input, "UTF-8");

```

三、获取项目路径

上面介绍了如何获取`classpath`路径之后，其实有时候我们会发现自己只想获取当前程序所在路径或`jar`包所在路径，那么此时又应该如何获取呢？

```java
// 方式一：
File file = new File(".");
File path1 = file.getAbsoluteFile();

// 方式二：
String path2 = System.getProperty("user.dir");
```


#### Maven生命周期
- Maven 构建项目生命周期描述的是一次构建过程经历了多少个事件
- Maven 对项目构建的生命周期划分为3套
	- clean：清理工作
	- default：核心工作，例如编译，测试，打包，安装等
	- site：产生报告，发布站点等
	<img src="https://article.biliimg.com/bfs/article/2320ecd2bc9684f0ca2963c38bdd36132b5a4860.png" alt="image.png" style="zoom:75%;" />

|Maven阶段|          描述        |
|:-:|----------------------|
|validate| 校验项目是否正确并且所有必要的信息可以完成项目的构建过程。|
|   initialize| 初始化构建状态，比如设置属性值。|
|generate-sources|生成包含在编译阶段中的任何源代码。|
|process-sources|处理源代码，比如说，过滤任意值。|
|generate-resources|生成将会包含在项目包中的资源文件。|
|process-resources|复制和处理资源到目标目录，为打包阶段最好准备。|
|compile|编译项目的源代码。 |
|process-classes|处理编译生成的文件，比如说对Java class文件做字节码改善优化。|
|generate-test-sources|生成包含在编译阶段中的任何测试源代码。|
|process-test-sources|处理测试源代码，比如说，过滤任意值。|
|generate-test-resources|为测试创建资源文件。 |
|process-test-resources|复制和处理测试资源到目标目录。|
|test-compile|编译测试源代码到测试目标目录。|
|process-test-classes|处理测试源码编译生成的文件。|
|test|使用合适的单元测试框架运行测试（Juint是其中之一）。|
|prepare-package|在实际打包之前，执行任何的必要的操作为打包做准备。|
|package |将编译后的代码打包成可分发格式的文件，比如JAR、WAR或者EAR文件。|
|pre-integration-test|在执行集成测试前进行必要的动作。比如说，搭建需要的环境。|
|integration-test|处理和部署项目到可以运行集成测试环境中。|
|post-integration-test|在执行集成测试完成后进行必要的动作。比如说，清理集成测试环境。|
|verify|运行任意的检查来验证项目包有效且达到质量标准。|
|install|安装项目包到本地仓库，这样项目包可以用作其他本地项目的依赖。|
|deploy|将最终的项目包复制到远程仓库中与其他开发者和项目共享。 |
### 依赖管理

**使用坐标导入 jar 包**
1. 在 pom.xml 中编写 `<dependencies>` 标签

2. 在 `<dependencies>` 标签中 使用 `<dependency>` 引入坐标

3. 定义坐标的 groupId，artifactId，version 

4. 点击刷新按钮，使坐标生效 

  ![image.png](https://article.biliimg.com/bfs/article/8a7102855a23fc2872b1247d36d9288f64774de0.png)

**使用坐标导入 jar 包 – 快捷方式**
1. 在 pom.xml 中 按 alt + insert，选择 Dependency 
2. 在弹出的面板中搜索对应坐标，然后双击选中对应坐标 
3. 点击刷新按钮，使坐标生效

![image.png](https://article.biliimg.com/bfs/article/94fa2b7198172f103ccb888b02596cd2ff37e6f3.png)
