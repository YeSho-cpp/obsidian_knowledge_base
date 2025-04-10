# java依赖搜索
`mvnrepository.com`是一个为Maven用户提供的开源Java库搜索引擎。它提供了各种Java库的详细信息，例如库的版本、依赖项、运行时环境等等。Maven用户可以使用该网站搜索他们需要的库，并将其添加到他们的Maven项目中。 

在`mvnrepository.com`的主页面上，您可以搜索任何开源Java库，并获得库的完整信息，包括其Maven坐标（groupId、artifactId和version）等详细信息。您可以检查特定版本的特性和已知问题，并了解库与其他库的兼容性和依赖关系。除此之外，该网站还提供相关的学习资源和链接，以帮助您更好地理解和使用该库。

因此，`mvnrepository.com`是一个非常有用的工具，可以帮助Java开发人员快速、准确地查找和使用开源Java库，从而提高他们的工作效率和开发经验。

<img src="https://article.biliimg.com/bfs/article/a751a0a8b227c7adc2ec4396309aff2e3c7676e0.png" alt="image.png" style="zoom:75%;" />

# junit
## xml配置
```xml
<!-- https://mvnrepository.com/artifact/junit/junit -->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.1</version>
    <scope>test</scope>
</dependency>
```
## 作用

看[[java基础篇#单元测试|这里]]

# mysql-connector

## xml配置
```xml
<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.32</version>
</dependency>
```

## 作用
`mysql-connector`是一个Java应用程序使用MySQL数据库时必须使用的JDBC驱动程序。JDBC驱动程序是用于将Java应用程序连接到数据库的API，每个数据库都有自己的JDBC驱动程序。

`mysql-connector`驱动程序使得Java程序能够使用MySQL数据库，从而增强了应用程序的数据处理和数据存储功能。该驱动程序是由MySQL官方提供的，通过该驱动程序，Java开发人员可以利用MySQL数据库的各种功能，例如事务处理、查询优化、连接池等。

应该注意的是，开发人员需要将驱动程序添加到他们的Java项目中，以便Java应用程序能够使用该库。在添加驱动程序之后，开发人员可以使用Java代码来连接MySQL数据库中的表，并执行各种操作，例如SELECT、UPDATE、DELETE等。同时，该驱动程序也提供了各种工具和类库，使Java应用程序更加易于开发和维护。

总之，`mysql-connector` JDBC驱动程序是MySQL数据库的一个必不可少的组件，它为Java程序员提供了一种方便且高效的方法来访问MySQL数据库，并为开发人员提供了许多强大的工具和资源，以便于开发和维护Java应用程序。

# druid

## xml配置
```xml
<dependency>  
    <groupId>com.alibaba</groupId>  
    <artifactId>druid</artifactId>  
    <version>1.1.16</version>  
</dependency>
```

本次文件
`druid-1.2.16.jar`

这是[[Mysql#Druid|druid的作用]]

# mybatis

xml配置
```xml
<dependency>  
    <groupId>org.mybatis</groupId>  
    <artifactId>mybatis</artifactId>  
    <version>3.5.0</version>  
</dependency>
```

## 作用

这个 Java 依赖项是用于集成 [[mybatis#MyBatis介绍|mybatis]] 框架的核心库。

此依赖项提供了 MyBatis 的核心功能，包括 SQL 映射文件、会话管理、对象映射和事务支持等。通过引入这个依赖项，您可以轻松地在您的 Java 项目中使用 MyBatis 框架，以便更加高效地进行数据库访问和操作。



# javax.servlet-api


## xml配置
解释下这个依赖的作用
```xml
<!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope>
</dependency>
```

## 作用

这个依赖的作用是将`javax.servlet-api`库添加到项目中。javax.servlet-api是Java Servlet规范的一部分，它提供了开发Web应用程序的**类**和**接口**。通过引入这个依赖，可以在项目中使用Servlet相关的类和接口。


另外，这个依赖还指定了scope为provided。这意味着该库在编译和测试代码时需要存在，但在运行时由容器（如Tomcat）提供。所以，在部署应用程序时，不需要将这个库打包到war或jar文件中。

# jakarta.servlet-api

## xml配置

```xml
<dependency>
	<groupId>jakarta.servlet</groupId>
	<artifactId>jakarta.servlet-api</artifactId>
	<version>5.0.0</version>
</dependency>
```

## 作用
这个Java依赖是用于引入 `Jakarta Servlet API `的库。Servlet API 是用于开发基于Java的Web应用程序的一组接口和类。使用这个依赖，可以在项目中使用 Servlet 相关的功能，例如处理 HTTP 请求和响应、会话管理、过滤器和监听器等。版本号5.0.0表示使用的是 Jakarta Servlet API 的5.0.0版本。

那jakarta.servlet-api跟javax.servlet-api有啥区别？

Jakarta Servlet API 和 javax.servlet-api 都是用于开发 Java Web 应用程序的 API，它们之间主要有以下区别：

1. 命名空间：javax.servlet-api 是 Java Servlet API 的官方标准规范，最初是由 Sun Microsystems（现在是 Oracle）开发和维护的。而 Jakarta Servlet API 是 Apache 软件基金会为了继续推进和发展 Java Servlet 技术而创建的一个项目。

2. 版本：Jakarta Servlet API 是对于最新版本的 Java Servlet 规范的实现，而 javax.servlet-api 是对较早版本规范的实现。目前，Jakarta Servlet API 的最新版本是 5.0，而 javax.servlet-api 的最新版本是 4.0。

3. 转变：在过去，Java EE 平台和相关技术一直由 Oracle 进行开发和管理。然而，在2017年底，Oracle 将 Java EE 交给 Eclipse 基金会进行管理，并将其命名为 Jakarta EE。因此，javax.servlet-api 在 Java EE 中仍然保持不变，而 Jakarta Servlet API 将成为 Jakarta EE 技术栈中的一部分。

总之，两者都提供了相同的功能和 API，但 Jakarta Servlet API 更加现代化，并且将成为未来 Jakarta EE 平台上的标准规范。


## 作用



# spring-context

## xml配置
```xml
<dependency>  
    <groupId>org.springframework</groupId>  
    <artifactId>spring-context</artifactId>  
    <!-- <version>5.2.10.RELEASE</version> -->
    <version>6.0.9</version>  <!--    java17可用        --> 
</dependency>
```

## 作用
`spring-context` 是 Spring 框架中非常重要的一个依赖，它提供了 Spring 应用程序的基本功能，例如 Bean 加载、Bean 生命周期管理、面向切面编程（AOP）、Spring 注解、事件处理等等。 

具体来说，`spring-context` 提供了以下主要功能：

- **IoC 容器**：提供了依赖注入（DI）和控制反转（IoC）的实现机制，可以让开发人员将应用程序的控制权委托给 Spring 框架。
- **Bean 加载和生命周期管理**：负责加载应用程序中定义的 Bean，并管理它们的生命周期、作用域等。
- **Spring AOP 和事务管理**：提供面向切面编程（AOP）和声明式事务管理的实现，可以轻松实现方法级别的切面功能和事务控制。
- **Spring 注解**：提供比 XML 配置更加方便的 Java 注解配置方式。

总之，`spring-context` 是构建 Spring 应用程序的核心依赖，为开发人员提供了一组强大而灵活的工具，使得开发过程变得更加简单、高效和可靠。

# javax.annotation

## xml配置

```xml
<!-- https://mvnrepository.com/artifact/javax.annotation/jsr250-api -->
<dependency>
    <groupId>javax.annotation</groupId>
    <artifactId>jsr250-api</artifactId>
    <version>1.0</version>
</dependency>
```

## 作用
`javax.annotation:jsr250-api` 是一个 Java 标准库中的依赖项，提供了对 JSR-250 规范的支持。JSR-250 定义了一些注解，用于在 Java 类中添加元数据信息，以实现更灵活和可配置的行为。
具体而言，`javax.annotation:jsr250-api` 提供了以下注解：
- `@Resource`：用于标记将资源注入到类中的字段、方法或构造函数上。
- `@PostConstruct`：用于标记一个方法，在实例化之后执行初始化操作。
- `@PreDestroy`：用于标记一个方法，在销毁之前执行清理操作。
- `@RolesAllowed`：用于指定哪些角色可以访问一个受保护的方法或资源。
通过使用这些注解，可以更方便地管理依赖注入、对象初始化和资源清理等行为。

# spring-jdbc

## xml配置
```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.18</version>
</dependency>
```

## 作用

`Spring JDBC`是`Spring`框架的一部分，它提供了一系列类和方法来简化Java应用程序与关系型数据库之间的交互。它建立在`JDBC（Java Database Connectivity）`API之上，并提供了更高级别的抽象，使数据库访问更加方便和易于管理。

使用`Spring JDBC`，你可以通过以下方式实现对数据库的操作：
- 简化的连接管理：`Spring JDBC`提供了一个连接池，用于管理数据库连接，并封装了打开和关闭数据库连接的复杂性。
- 异常处理和转换：`Spring JDBC`将JDBC异常转换为通用的数据访问异常，从而简化了异常处理。
- `SQL`操作的模板化：`Spring JDBC`提供了`JdbcTemplate`类，它封装了执行SQL语句所需的大部分代码，包括参数绑定、结果集提取等，从而简化了SQL操作。
- 对象关系映射（`ORM`）支持：`Spring JDBC`提供了对`ORM`框架（如`Hibernate`、`MyBatis`）的集成支持，使得在`Spring`应用程序中使用`ORM`变得更加容易。

因此，通过添加这个依赖项，你可以在你的Java项目中使用`Spring JDBC`模块，以便更轻松地进行数据库访问和管理。

# mybatis-spring
## xml配置

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>3.0.0</version>
</dependency>
```

## 作用
该`<dependency>`标签是用于在Java项目中使用MyBatis框架与Spring框架集成的依赖项。

MyBatis是一种开源的持久层框架，它通过将SQL语句与Java对象之间的映射来简化数据库访问。它提供了一种灵活的方式来执行SQL查询、更新和删除操作，并且可以轻松地处理复杂的数据库操作。
通过集成Spring框架，你可以更加方便地使用MyBatis，并充分发挥Spring的优势。这个依赖项提供了与Spring框架集成所需的类和方法，包括：

- Spring事务管理：MyBatis-Spring模块提供了Spring事务管理器和MyBatis的事务支持，使你可以在Spring事务范围内执行数据库操作。
- 对象关系映射（ORM）支持：MyBatis-Spring模块提供了对MyBatis的ORM功能的支持，例如自动映射结果集到Java对象、关联查询的支持等。
- 配置简化：MyBatis-Spring模块通过简化配置文件的编写，使得集成和使用MyBatis变得更加容易。

因此，添加这个依赖项可以实现在你的Java项目中集成MyBatis和Spring框架，从而更加方便地进行数据库访问和管理，并享受到两个框架的优势。


# spring-test
## xml配置
```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-test -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.3.27</version> 
    <!-- 这个版本一定要跟spring-webmvc一致-->
    <scope>test</scope>
</dependency>
```
## 作用

spring-test是Spring Framework提供的一个模块，用于测试Spring应用程序的工具。它提供了一些特殊的注解和类，可以帮助开发人员编写单元测试、集成测试和端到端测试。

下面是spring-test的一些主要功能和作用：

1. 提供了用于测试Spring应用程序的注解：spring-test提供了一些特殊的注解，如`@RunWith`、`@ContextConfiguration`等，用于配置和执行测试环境。这些注解可以帮助开发人员轻松地创建和管理Spring上下文，并与测试框架（如JUnit）集成。

2. 支持使用Spring的IoC容器进行测试：spring-test允许开发人员在测试中使用Spring的IoC容器，从而能够方便地注入和管理测试所需的依赖项。这样可以更加真实地模拟应用程序的运行环境，并且可以更好地测试组件之间的交互关系。

3. 提供对Spring MVC的支持：spring-test提供了一些专门用于测试Spring MVC的工具和类。开发人员可以使用这些工具进行模拟HTTP请求和响应，测试控制器的处理逻辑和视图渲染，以及验证URL映射和请求参数绑定等。

4. 支持数据库集成测试：spring-test提供了一些特殊的注解和类，用于支持数据库集成测试。开发人员可以使用这些工具方便地与数据库交互，并进行事务管理、数据准备和清理等操作，从而更好地测试与数据库相关的功能。

总结起来，spring-test为开发人员提供了一套强大的测试工具，可以帮助他们更加方便、高效地编写和执行各种类型的测试。它与Spring Framework紧密集成，提供了对Spring应用程序各个层面（如IoC容器、MVC框架、持久层等）的测试支持。这样，开发人员可以在保证代码质量的同时，更加自信地进行重构和功能扩展。

# aspectjweaver

## xml配置
```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.19</version>
    <scope>runtime</scope>
</dependency>
```
## 作用

`aspectjweaver`是一个Java库，它提供了与AspectJ编译器和运行时框架一起使用的支持。AspectJ是一个面向方面（[[spring#AOP核心概念|AOP]]）编程的扩展，它允许开发人员将横切关注点（比如日志记录、事务管理和安全性）从主要业务逻辑中分离出来。

具体而言，aspectjweaver依赖的作用有以下几个方面：

1. 提供编译时处理：通过在编译过程中增加额外的步骤，`aspectjweaver`能够**捕获源代码中的横切关注点**，并生成合适的字节码来实现这些关注点。这样，开发人员可以在编写代码时专注于核心业务逻辑，而不必手动插入大量的横切代码。

2. 提供运行时织入：`aspectjweaver`支持在程序运行时动态地将`AspectJ`方面织入到目标类中。这意味着你可以在程序执行期间对已编译的类进行增强，而无需修改其源代码。这种动态织入的特性使得应用程序更加灵活和可扩展。

3. 支持各种`AspectJ`语法：`aspectjweaver`能够处理`AspectJ`注解、切面、切入点和通知等特性，使得开发人员可以使用`AspectJ`提供的丰富语法来实现面向方面编程。

# spring-webmvc
## xml配置
```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <!-- <version>5.3.24</version> -->
    <version>5.2.10.RELEASE</version>

</dependency>
```
## 作用

`spring-webmvc`是Spring框架中用于开发Web应用程序的模块，它提供了基于MVC（Model-View-Controller）设计模式的Web应用开发支持。具体作用如下：

1. 提供了控制器层（Controller）：spring-webmvc允许开发者通过使用`@Controller`注解来定义控制器类，控制器负责处理请求并返回响应。

2. 支持模型-视图分离：`spring-webmvc`通过使用`ModelAndView`对象将数据（模型）与视图进行分离，实现业务逻辑和界面显示的解耦。

3. 提供了视图解析器：`spring-webmvc`提供了多种视图解析器，包括JSP、Thymeleaf、FreeMarker等，可以根据需求选择合适的视图技术进行页面渲染。

4. 支持表单处理：`spring-webmvc`提供了表单数据的绑定和验证功能，简化了表单处理操作。

5. 提供了请求参数映射：`spring-webmvc`可以将HTTP请求参数映射到Controller方法的参数上，并支持复杂类型的自动转换。

6. 支持RESTful风格的接口开发：`spring-webmvc`可以通过注解方式实现RESTful风格的接口开发，简化了URL映射和请求处理操作。

总之，`spring-webmvc`为开发Web应用程序提供了一系列方便易用的功能和组件，大大简化了开发工作，提高了开发效率。



# jackson-databind
## xml配置
```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.8</version>
</dependency>
```

## 作用

`Jackson-databind`是一个Java库，它提供了将Java对象序列化为`JSON`（JavaScript Object Notation）格式和将JSON反序列化为Java对象的功能。它是Jackson JSON处理库的核心模块，可以与多种数据格式进行交互，包括JSON、XML、YAML等。

具体来说，jackson-databind的作用有以下几个方面：

1. 对象序列化：可以将Java对象转换为JSON字符串，以便在网络传输或存储时使用。这样可以方便地将数据从一个应用程序传递给另一个应用程序。

2. 对象反序列化：可以将JSON字符串转换为Java对象，以便在代码中操作和处理。通过反序列化，可以轻松地将从外部来源（如API响应）获取的数据转换为可直接使用的Java对象。

3. 数据绑定：可以将JSON中的属性绑定到Java对象的相应属性上。这样，在反序列化时，Jackson会自动匹配JSON中的属性名称与Java对象中的属性名称，并进行赋值。

4. 支持复杂类型：jackson-databind支持对复杂类型（如集合、嵌套对象等）进行序列化和反序列化操作。这使得处理包含复杂数据结构的应用程序更加简单和方便。

总而言之，jackson-databind是一个非常重要的依赖库，它提供了强大的JSON处理功能，使得在Java应用程序中进行对象和JSON之间的转换变得更加简单和高效。


# spring-boot-starter-web

## xml配置

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
```


## 作用
`spring-boot-starter-web` 是一个 Spring Boot 项目的核心依赖之一，它提供了构建基于 Web 应用程序的必要**依赖项**和**自动配置**。它主要包含以下功能和作用：

1. 自动配置：`spring-boot-starter-web` 自动配置了一些常见的 Web 开发所需的配置，例如自动配置了 `DispatcherServlet`、默认的 `ErrorController`、静态资源处理等。

2. 内嵌容器：`spring-boot-starter-web` 默认使用 Tomcat 作为内嵌容器，可以通过其他 Starter 来使用 Jetty 或 Undertow 等其他容器。

3. Spring MVC：`spring-boot-starter-web` 包含了 Spring MVC 的依赖项，可以使用注解驱动的方式进行开发和处理 HTTP 请求。

4. RESTful Web Services：`spring-boot-starter-web` 支持开发 RESTful 风格的 Web 服务，可以使用 `@RestController` 注解来定义 REST 控制器和处理请求。

5. 数据绑定与验证：`spring-boot-starter-web` 提供了数据绑定和验证机制，可以方便地将请求参数绑定到方法参数，并进行数据校验。

6. 异常处理：`spring-boot-starter-web` 提供了异常处理机制，可以通过自定义异常处理器来处理应用程序中的异常情况，并返回合适的响应信息。

总之，通过引入 `spring-boot-starter-web` 依赖，可以快速开始构建基于 Spring Boot 的 Web 应用程序，并利用自动配置和依赖项的功能来简化开发过程。


# spring-boot-starter-test

## xml配置


```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
```


## 作用
`spring-boot-starter-test`是一个Spring Boot提供的用于进行**单元测试**和**集成测试**的依赖。它包含了一些常用的测试框架和工具，以便在开发过程中简化测试的编写和执行。

具体来说，`spring-boot-starter-test`依赖包含了以下功能：

1. `JUnit`：JUnit是一个广泛使用的Java单元测试框架，它提供了一组用于编写、运行和管理单元测试的API。spring-boot-starter-test依赖会自动引入JUnit相关的依赖，使得开发者可以方便地使用JUnit进行单元测试。

2. `Spring Test`：Spring Test是Spring Framework提供的一组用于支持集成测试和端到端（End-to-End）测试的功能。它包含了一些注解和类，如@SpringBootTest、@RunWith(SpringRunner.class)等，用于简化编写Spring Boot应用程序的集成测试。

3. `AssertJ`：AssertJ是一个流行的Java断言库，它提供了丰富而灵活的断言方法，使得编写可读性更高、维护更容易的断言变得更加简单。spring-boot-starter-test依赖中也包含了AssertJ库，方便开发者进行断言操作。

4. `Hamcrest`：Hamcrest是另一个流行的Java断言库，它提供了一组可扩展的匹配器（Matcher）类，可以用于编写灵活且易于阅读的断言。spring-boot-starter-test依赖中也包含了Hamcrest库。

5. `Mockito`：Mockito是一个Java模拟框架，它可以用于创建和管理测试中的模拟对象（Mocks），从而解决测试过程中对外部依赖的问题。spring-boot-starter-test依赖中包含了Mockito库，使得开发者可以方便地使用Mockito进行单元测试。

通过引入`spring-boot-starter-test`依赖，开发者可以更加轻松地编写和执行各种类型的测试，包括单元测试、集成测试和端到端测试，提高代码质量和可靠性。

# spring-boot-configuration-processor

## xml配置

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-configuration-processor</artifactId>
	<optional>true</optional>
</dependency>
```

## 作用

这个java依赖是用于在Spring Boot项目中生成自定义配置类的元数据的。当使用`@ConfigurationProperties`注解在配置类中定义了一些属性时，`spring-boot-configuration-processor`会自动生成用于绑定这些属性的元数据文件。这样，在编译时，IDE（集成开发环境）可以通过这些元数据提供智能提示和完整性检查。此外，它还可以用于生成文档和其他与配置相关的任务。


# mybatis-plus-boot-starter
## xml配置
解释下面这个java依赖的作用？
```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.5.3.1</version>  <!-- springboot 3.0 这个必须3.5.3以上  -->
</dependency>
```


## 作用
这个Java依赖是用于在Spring Boot项目中集成MyBatis Plus框架的。MyBatis Plus是一个开源的持久层框架，它在MyBatis的基础上进行了扩展和增强，简化了数据库操作的开发工作。

这个依赖添加了`mybatis-plus-boot-starter`库的引用，该库提供了MyBatis Plus在Spring Boot项目中所需的所有依赖和配置。通过添加这个依赖，可以方便地使用MyBatis Plus来进行数据库操作，包括增删改查、分页查询、条件查询等。

版本号3.5.3.1指定了使用的MyBatis Plus版本为3.5.3.1。对于使用Spring Boot 3.0及以上版本的项目，必须使用此版本或更高版本才能兼容。

# lombok

## xml配置

解释下面这个java依赖的作用？
```xml
<dependency>
	<groupId>org.projectlombok</groupId>
	<artifactId>lombok</artifactId>
	<version>1.18.26</version>
	<scope>provided</scope>
</dependency>
```

## 作用

这个Java依赖是用来引入Lombok库的。Lombok是一个Java库，它通过提供注解来简化Java代码的编写。它可以自动生成一些常见的代码，如getter和setter方法、构造函数、equals和hashCode方法等，从而减少了开发人员需要手动编写这些重复代码的工作量。

这个依赖的groupId为"org.projectlombok"，artifactId为"lombok"，version为"1.18.12"。通过将此依赖添加到项目中，开发人员可以使用Lombok库中提供的注解来简化代码编写。

在上面的示例中，该依赖的scope被设置为"provided"。这意味着该依赖在编译和测试时可用，但在运行时不会包含在最终构建的项目中。相反，它假定该依赖将由项目所在环境（如应用程序服务器）提供。


# mybatis-plus-generator
## xml配置

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.5.3.1</version>
</dependency>
```

## 作用
这个Java依赖是用于引入`MyBatis Plus Generator`库的。MyBatis Plus Generator是一个<font color=#ff0000>代码生成工具</font>，它可以根据数据库表结构自动生成对应的实体类、Mapper接口和XML文件，简化了开发人员编写重复且繁琐的代码的过程。

通过在项目中添加这个依赖，就可以使用MyBatis Plus Generator提供的代码生成功能。在使用过程中，开发人员需要配置相应的数据源和生成规则，并运行生成器即可自动生成相关代码文件。


# velocity-engine-core
## xml配置
解释下面这个java依赖的作用？
```xml
<dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity-engine-core</artifactId>
    <version>2.3</version>
</dependency>
```

## 作用
这个Java依赖是用来引入`Apache Velocity`引擎的核心功能的。`Velocity`是一个用于生成动态HTML页面、邮件<font color=#ff0000>模板</font>和其他文本文件的<font color=#ff0000>模板引擎</font>。它可以帮助开发人员通过将**模板与数据结合**来生成输出，从而简化了生成动态内容的过程。`velocity-engine-core`是Velocity引擎的核心库，提供了处理模板、解析表达式和生成输出等功能。通过添加这个依赖，开发人员可以在他们的项目中使用Velocity模板引擎来实现动态内容的生成和展示。

# freemarker
## xml配置

```xml
	<dependency>
	<groupId>org.freemarker</groupId>
	<artifactId>freemarker</artifactId>
	<version>2.3.31</version>
	</dependency>
```

## 作用

这个依赖是用于在Java项目中使用FreeMarker模板引擎的。FreeMarker是一个Java模板引擎，它可以用于生成各种文本格式，如HTML、XML、JSON等。这个依赖将FreeMarker库添加到项目的依赖中，使得项目可以使用FreeMarker模板引擎来生成文本文件。在使用FreeMarker模板引擎时，需要在项目中添加这个依赖，并在代码中引入相应的类和接口。

# commons-pool2

## xml配置

```xml
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
        </dependency>
```

## 作用

这个依赖是 Apache Commons Pool 2 的库，它提供了一个对象池框架，用于在应用程序中重复使用可共享的对象，以提高性能和资源利用率。通过对象池，可以避免频繁地创建和销毁对象，从而减少了对象分配和垃圾回收的开销。使用对象池可以使代码更加高效和可扩展。

# hutool-all

## xml配置
说下这个java依赖的作用
```xml
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.7.17</version>
        </dependency>
```

## 作用

这个Java依赖是hutool-all，它是一个Java工具类库，提供了各种常用的工具方法和功能。通过引入hutool-all依赖，可以方便地使用其中的工具类来简化开发过程。

具体来说，hutool-all包含了各种常用的工具类，比如字符串处理、日期时间处理、加密解密、文件操作、HTTP请求等等。使用这些工具类可以大大提高开发效率，避免重复造轮子。

此外，hutool-all还提供了一些特殊的功能模块，比如Excel处理、PDF生成、邮件发送等等。通过引入hutool-all依赖，可以方便地使用这些功能模块来完成一些特定的任务。

总之，引入hutool-all依赖可以让开发者更加方便地使用各种常用的Java工具方法和功能模块，提高开发效率。

## 具体方法

`RandomUtil`这个方法可以用来生成随机的数或者变量什么的

`BeanUtil.copyProperties`方法是Hutool-All工具包中的一个方法，用于**将一个Java对象的属性值拷贝到另一个Java对象**中。

1. 对象属性拷贝：可以将源对象中的属性值复制到目标对象中相同名称的属性上。这样可以避免手动逐个设置属性值，节省了开发者的时间和精力。比如，可以将一个用户表单提交的数据拷贝到一个实体类对象中。

2. 对象属性映射：可以通过设置映射关系，将源对象中的属性值复制到目标对象中不同名称的属性上。这样可以实现两个类之间不完全相同或者完全不同字段之间的值拷贝。比如，可以将数据库查询结果集中每一行数据拷贝到对应实体类对象中。

`BeanUtil.beanToMap`方法的作用是将Java对象转换为Map对象。

具体而言，该方法将给定的Java对象中的属性和对应的值提取出来，并以键值对的形式存储在一个Map中。其中，键表示属性名，值表示属性对应的值。

这个方法在实际开发中非常有用，例如：

- 可以方便地将Java对象转换为Map对象，以便进行序列化、持久化或传输。
- 可以在不改变原有代码逻辑的情况下，方便地将Java对象转换为Map对象进行数据处理和操作。
- 可以方便地进行数据类型转换和数据格式化等操作。

`JSONUtil.tobean`方法是Hutool-All工具库中的一个方法，它的作用是将JSON字符串转换为Java对象。该方法接受两个参数，第一个参数是要转换的JSON字符串，第二个参数是目标Java对象的Class类型。

该方法会根据JSON字符串的键值对，将对应的值赋给目标Java对象中对应的字段。它使用了反射机制，自动匹配字段名和JSON键名，并进行赋值操作。
示例：
```java
String jsonStr = "{\"name\":\"John\", \"age\":30, \"city\":\"New York\"}";
User user = JSONUtil.toBean(jsonStr, User.class);
```

`JSONUtil.toJsonStr`
用于将**Java对象**转换为**JSON格式**的字符串。
通过调用`JSONUtil.toJsonStr`方法，可以将Java对象序列化为JSON字符串。这在处理网络请求、数据传输、前后端交互等场景中非常有用。例如，当需要将一个Java对象转换为JSON字符串后发送给前端页面时，可以使用`JSONUtil.toJsonStr`方法。


# redisson

## xml配置
说明下面这个java依赖的作用？
```xml
<dependency>
	<groupId>org.redisson</groupId>
	<artifactId>redisson</artifactId>
	<version>3.13.6</version>
</dependency>
```


## 作用
这个Java依赖的作用是引入`Redisson`库，用于在Java应用程序中与Redis数据库进行交互。

具体而言，`Redisson`是一个基于`Redis`的分布式Java对象和服务框架，它提供了一组易于使用的API，可以在Java应用程序中使用常见的数据结构和分布式服务。通过使用Redisson库，开发人员可以方便地利用Redis的高性能、高可靠性和强大功能来构建分布式应用程序。

这个依赖项指定了`Redisson`库的版本为3.13.6，并且定义了其groupId为"org.redisson"，artifactId为"redisson"。当项目构建时，Maven或其他构建工具将自动下载并导入所需的库文件。

# canal-spring-boot-starter

## xml配置

说一下下面这个java的依赖的作用
```xml
<dependency>
    <groupId>top.javatool</groupId>
    <artifactId>canal-spring-boot-starter</artifactId>
    <version>1.2.1-RELEASE</version>
</dependency>
```

## 作用
这个Java的依赖是用于使用Canal框架的`Spring Boot Starter`的。Canal是阿里巴巴开源的一款基于数据库增量日志解析，提供增量数据订阅和消费的中间件。它可以解析数据库的binlog日志，并将解析后的增量数据发送给消费者。

这个依赖提供了Canal框架在Spring Boot应用中的集成支持，使开发者能够更方便地使用Canal框架进行**增量数据订阅和消费**。通过引入这个依赖，开发者可以在Spring Boot应用中配置Canal相关的属性，如数据库连接信息、表过滤规则等，并且可以通过编写监听器来处理接收到的增量数据。

总结起来，这个依赖的作用是简化了使用Canal框架进行增量数据订阅和消费的操作，提供了集成支持并提供了一些配置和监听器等功能。

# jjw

## xml配置
```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.1</version>
</dependency>

```

## 作用

[[零散知识点#什么是JWT|jwt]]


# knife4j-spring-boot-starter

## xml配置

```xml
<dependency>
   <groupId>com.github.xiaoymin</groupId>
   <artifactId>knife4j-spring-boot-starter</artifactId>
</dependency>
```

在配置类中加入 knife4j 相关配置
`WebMvcConfiguration.java`

```java
/**
     * 通过knife4j生成接口文档
     * @return
*/
    @Bean
    public Docket docket() {
        ApiInfo apiInfo = new ApiInfoBuilder()
                .title("苍穹外卖项目接口文档")
                .version("2.0")
                .description("苍穹外卖项目接口文档")
                .build();
        Docket docket = new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.sky.controller"))
                .paths(PathSelectors.any())
                .build();
        return docket;
    }
```

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

访问测试

接口文档访问路径为 http://ip:port/doc.html ---> http://localhost:8080/doc.html

<img src="https://article.biliimg.com/bfs/article/2bc5b4bb73d5d47881dc69a1d75ce6d8057e6024.png" alt="image-20221107173033632" style="zoom:50%;" />

接口测试:测试登录功能

<img src="https://article.biliimg.com/bfs/article/834d0ff777fd5313bba2757bdb82741d1c54edfb.png" alt="image-20221107173137506" style="zoom:50%;" />
**思考：**通过 Swagger 就可以生成接口文档，那么我们就不需要 Yapi 了？

1、Yapi 是设计阶段使用的工具，管理和维护接口
2、Swagger 在开发阶段使用的框架，帮助后端开发人员做后端的接口测试

###  常用注解

通过注解可以控制生成的接口文档，使接口文档拥有更好的可读性，常用注解如下：

| **注解**            | **说明**                           |
|:-----------------:|:--------------------------------:|
|`@Api`| 用在类上，例如Controller，表示对类的说明        |
|`@ApiModel`| 用在类上，例如entity、DTO、VO             |
|`@ApiModelProperty`| 用在属性上，描述属性信息                     |
|`@ApiOperation`| 用在方法上，例如Controller的方法，说明方法的用途、作用 |

# pagehelper-spring-boot-starter

## xml配置
介绍下面依赖的作用
```xml
<dependency>
   <groupId>com.github.pagehelper</groupId>
   <artifactId>pagehelper-spring-boot-starter</artifactId>
   <version>${pagehelper}</version>
</dependency>
```


## 作用:
这个依赖是用于在Spring Boot项目中集成`PageHelper`分页插件的。PageHelper是一个开源的Mybatis分页插件，它可以方便地对查询结果进行分页处理。

具体作用如下：

1. 实现了物理分页，通过拦截SQL语句，在查询前自动添加分页相关的SQL代码，比如limit语句。
2. 支持多种数据库，包括MySQL、Oracle、SQL Server等。
3. 提供了丰富的分页参数配置选项，可以自定义每页显示的数量、是否进行count查询等。
4. 提供了丰富的分页结果处理方法，可以获取总记录数、当前页码、总页数等信息。
5. 支持通过注解方式实现分页，简化了代码编写。
6. 可以与Mybatis原生的分页方法结合使用。

通过引入该依赖，并进行相应配置后，在使用Mybatis进行数据库查询时，就可以方便地实现分页功能。


# aliyun-sdk-oss
## xml配置
```xml
<dependency>
    <groupId>com.aliyun.oss</groupId>
    <artifactId>aliyun-sdk-oss</artifactId>
    <version>3.15.1</version>
</dependency>
```

## 作用
[[spring#阿里云OSS|阿里云]]


# httpclient
## xml配置
```xml
<dependency>
	<groupId>org.apache.httpcomponents</groupId>
	<artifactId>httpclient</artifactId>
	<version>4.5.13</version>
</dependency>
```

## 作用

[[javaweb#HttpClient|httpclient]]


# Spring Cache

## xml配置
```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-cache</artifactId>  		            		       	 <version>2.7.3</version> 
</dependency>
```

## 作用

[[sping-boot#Spring Cache|Spring Cache]]

# poi-ooxml

## xml配置


解释下面java依赖的作用？
```xml
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi</artifactId>
    <version>3.16</version>
</dependency>
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
    <version>3.16</version>
</dependency>
```

## 作用

这段代码是用于在Java项目中添加Apache POI库的依赖。Apache POI是一个用于操作Microsoft Office文件（如Excel、Word和PowerPoint）的开源Java库。

第一个依赖`poi`包含了POI的核心功能，可以用于读取、写入和操作Microsoft Office文件。第二个依赖`poi-ooxml`则是基于poi扩展的一个模块，提供了读写Microsoft Office Open XML（OOXML）格式文件（如.xlsx和.pptx）的功能。

通过添加这两个依赖，可以在Java项目中使用POI库中提供的各种类和方法来处理Microsoft Office文件，例如读取Excel数据、创建Word文档或者修改PowerPoint演示文稿等。


