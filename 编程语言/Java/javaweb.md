# web相关知识概述
## 什么是web
Web（World Wide Web）即全球广域网，也称为万维网。它是一种基于**超文本**和**HTTP**的、全球性的、动态交互的、跨平台的分布式**图形信息系统**。是建立在Internet上的一种网络服务，为浏览者在Internet上查找和浏览信息提供了图形化的、易于访问的直观界面，其中的文档及超级链接将Internet上的信息节点组织成一个互为关联的网状结构。

**web发展阶段介绍：**
1. web1.0
	1. 1994年在中国第一个web网站是**中国黄页**，由马云。属于静态网页。只能看，不能交互。发布租赁信息
2. web2.0
	1. 动态网站。网站数据是时时更新的。数据来自于数据库的。登录网站显示用户名，天气预报，微博头条。


> [!note]+ 区别
> 网站：前端知识、web服务器知识和数据库知识          
> 网页：前端页面html页面           
> 

## 软件架构模式

1. **BS**:browser server 浏览器服务器。
	- 优点：
		- 1）只需要服务器，用户下载浏览器，维护方便
		- 2）减少用户的磁盘空间
	- 缺点：
		- 1）给服务器造成压力
		- 2）用户观看体验不友好
	- 例如: 天猫、京东、知乎网站
	![image.png](https://article.biliimg.com/bfs/article/20cc947cb8a90323616c3570ff5336a02bade7df.png)
2. **CS**：client server 客户端 服务器
	- 优点：
		- 1）具有客户端和服务器端，减轻服务器的压力
		- 2）用户观看体验友好
	- 缺点：
		- 1）维护成本大
		- 2）版本升级麻烦，占用户磁盘空间
	- 例如: QQ，绝地求生，LOL
	![image.png](https://article.biliimg.com/bfs/article/f55b3e93693ffdd2b5fc0d4c4dc98b2b7d25eb2c.png)

**B/S和C/S通信模式特点:**

<img src="https://article.biliimg.com/bfs/article/044275fb15bd8d398e17eb34a9d10f124691a1d8.png" alt="image.png" style="zoom:50%;" />

> [!example]+ bs或者cs的通信模式:
> 1.先有请求      
> 2.后有咋应      
> 3.请求和响应是成对出现的 

## WEB资源

> [!summary]+ 分类
> 1. 静态资源：html css js。只能书写静态网站。静态网站的数据永远不会发生改变。                      
> 2. 动态资源：使用一些语言可以实现数据的变化，例如java语言。JAVAEE   




## URL请求路径

浏览器通过[[前端html#认识URL|URL]]访问服务器的过程

```
https://www.baidu.com/s?ie=UTF-8&wd=java
```

![image.png](https://article.biliimg.com/bfs/article/4485a5c7c8cd9aaca1f8d37ef2da367e785eb25f.png)

URL请求路径的组成
```
协议://服务器的ip地址:服务器的端口号/项目名/资源路径
```


# AJAX&JSON

## AJAX的概述
### 什么是ajax

<img src="https://article.biliimg.com/bfs/article/36777279c91237fbdcd9a00c76b01e45ac950553.png" alt="image.png" style="zoom:50%;" />

1. AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。
	说明：异步：就是不同步。例如我们向后台发送请求，以前的方式是后台必须返回响应数据才可以在浏览器上进行下一步操作，而异步方式可以在不需要等待后台服务器响应数据，直接可以进行其他操作。
2. AJAX 不是新的编程语言，而是一种使用现有标准的新方法。
3. AJAX 是与服务器交换数据并更新部分网页的技术，在不重新加载整个页面的情况下。

简而言之：ajax就是一项通过js技术**发送异步请求** 的技术。     异步请求，局部更新，不是整个网页更新。

### 同步和异步的区别


![image.png](https://article.biliimg.com/bfs/article/93b44d2125f2028a2a55551cc0979db9069de730.png)


```java
#同步
1. 浏览器与服务器是串行的操作，浏览器发起请求的时候，服务器在处理该请求的时候，浏览器只能等待。响应返回，然后才能够发送下一个请求，如果该请求没有响应，不能发送下一个请求，客户端会处于一直等待过程中。以前学习的开发的方式都是同步的方式。
2. 缺点：执行效率低，用户体验差。
#异步
1. 浏览器与服务器是并行工作的，浏览器发送一个请求，不需要等待响应返回，随时可以再发送下一个请求，即不需要等待。
2. 优点：执行效率高，用户体验更好。
```

<img src="https://article.biliimg.com/bfs/article/39a9e528d0624e78c1a216cd7c1e3dbc3fa46534.png" alt="image.png" style="zoom: 50%;" />

AJAX使用异步的提交方式，浏览器与服务器可以并行操作，即浏览器后台发送数据给服务器。用户在前台还是可以继续工作。用户感觉不到浏览器已经将数据发送给了服务器，并且服务器也已经返回了数据。
<img src="https://article.biliimg.com/bfs/article/ae5f33bc48f804ef40a8331eb7df5953b8b533f5.png" alt="image.png" style="zoom:80%;" />

【1】同步请求存在的问题：
1. 阻塞：请求发出后必须得等到响应结束才能操作页面信息。
2. 全部更新：整个页面更新。
【2】异步请求好处：
​1. 非阻塞：请求发出后不用等到响应结束才能操作页面信息。随时可以操作。
​2. 局部更新：页面的局部位置更新

### AJAX的应用场景

Ajax通常用需要发送异步请求的地方，如表单的异步校验，搜索框的自动补全，异步加载数据；
**注册表单的用户名异步校验**
很多站点的注册页面都具备自动检测用户名是否存在的友好提示，当用户输入的账号已经存在，那么在输入框位置会出现提示信息。该功能整体页面并没有刷新，但仍然可以异步与服务器端进行数据交换，查询用户的输入的用户名是否在数据库中已经存在。

<img src="https://article.biliimg.com/bfs/article/07483f53347bc92aaa9065a1c495ee300cceb45b.png" alt="image.png" style="zoom: 80%;" />

**内容自动补全**
	不管是专注于搜索的百度，还是站点内商品搜索的京东，都有搜索功能，在搜索框输入查询关键字时，整个页面没有刷新，但会根据关键字显示相关查询字条，这个过程是异步的。

### AJAX 的交互模型和传统交互模型的区别
AJAX 的交互模型和传统交互模型的区别如下图所示：
![image.png](https://article.biliimg.com/bfs/article/ab508beaa901a8a145b1c40d787604293a7ea855.png)

**传统交互模型**：浏览器客户端向服务器直接发送请求数据，然后后台服务器接收到请求，处理请求数据，期间浏览器客户端只能等待服务器处理数据，并响应数据，最后服务器将响应数据响应给浏览器客户端，浏览器接收到响应之后才可以继续下一步操作。
**AJAX 的交互模型**：就是浏览器内部多了一个ajax引擎，浏览器客户端向服务器发送请求的数据，都是先由浏览器将请求数据交给ajax引擎，注意，ajax引擎位于浏览器内部，是由js编写的一个软件。然后接下来都是由ajax引擎和服务器进行交互，此时用户可以在浏览器上进行其他操作，如果再次向服务器发送请求，那么依然是交给ajax引擎处理，并且服务器响应的数据也是交给ajax引擎处理，由ajax引擎来分配浏览器的操作。

注意：ajax引擎内部具有一个核心对象：XMLHttpRequest。都是由该对象进行异步请求交互数据的。

## JSON

### 1 JSON概述

JavaScript对象文本表示形式（JavaScript Object Notation : js对象简谱)

> json是js对象,但是js对象不一定是json
> json是目前 前后端数据交互的主要格式之一

```java
* java对象表示形式
		User user = new User();
			user.setUsername("后羿");
			user.setAge(23);
			user.setSex("男");
			...
			
		Product product = new Product();
			product.setName("小米10");
			product.setDesc("1亿像素的手机小王子");
			
* javaScript对象表示形式
		let user ={"username":"后羿","age":23,"sex":"男"}
		let product = {"name":"小米10","desc":"1亿像素的手机小王子"}
```

json可以取代XML笨重的数据结构，和xml相比：更小、更快，更易解析 
> json、xml作用：作为数据的载体，在网络中传输

<img src="https://article.biliimg.com/bfs/article/7035b52b6380a2c409de8e845fe3e6f4bf1a49ea.png" alt="image.png"  />

### 2 JSON基础语法

> json是一种特殊的 js 对象

```java
#json的语法主要有两种:
        1. 对象 { }
        2. 数组 [ ]
        
1. 对象类型
		{name:value,name:value}
		
2. 数组类型
		[
            {name:value,name:value}, 
            {name:value,name:value},
            {name:value,name:value}
		]
		
3. 复杂对象
		{
            name:value,
            wives:[{name:value},{},{}],
            son:{name:value}
		}
#注意: 
	1. 其中name必须是string类型
		json在js中,name的双引号可以省略
	2. value必须是以下数据类型之一：
		字符串
		数字
		对象（JSON 对象）
		数组
		布尔
		Null
	3. JSON 中的字符串必须用双引号包围。(单引号不行!)	
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
<script>
 
    let a = "str";
    let b = function (){

    }
    function c(){

    }
    let jsObj = {
        name : "zs",
        age : 18,
        speak : function (){
            console.log(this.name + "说: 今天天气不错");
        },
        show(){
            console.log(this.name + "在表演");
        }
    }
    console.log(jsObj.name); //zs
    jsObj.speak() // zs说: 今天天气不错

</script>
<script>    
    let jsonObj = {
        name : "zs",
        age : 18
    }
    console.log(jsonObj.name); // zs
    let jsonArray = [
        {
            name : "zs",
            age : 18
        },
        {
            name : "ls",
            age : 19
        }
    ]
    console.log(jsonArray[1].name); // ls
    
    let jsonComplex = {
        name : "xiaobao",
        wives : jsonArray,
        son : {
            name : "dabao",
            age : 3
        }
    }
    console.log(jsonComplex.wives[0].name); // zs
    console.log(jsonComplex.son.name); // dabao
</script>
</html>
```

## Fastjson 
### 1 fastjson引入

**需求**
​在服务器端有如下User对象需要响应给浏览器.
​为了方便浏览器解析, 这就要求服务端在响应之前,需要将转成符合Json格式的字符串.
![image.png](https://article.biliimg.com/bfs/article/562547277a9fc0242c2c24a6da0664afcc422473.png)

```java
public class User {
    private String username;
    private String password;
    public User() {
    }
    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }
    @Override
    public String toString() {
        return "User{" +
                "username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
    //TODO: 自己采用字符串拼接的方式输出。
    public String toJson() {
        return "{\"username\":\""+username+"\",\"password\":"+password+"}";
    }
    public String getUsername() {
        return username;
    }
    public void setUsername(String username) {
        this.username = username;
    }
    public String getPassword() {
        return password;
    }
    public void setPassword(String password) {
        this.password = password;
    }
}
```

通过拼接字符串的形式,将java对象转换成json格式字符串无疑是非常麻烦的,在开发中我们一般使用转换工具来达到实现.
​所谓的转换工具是通过java封装好的一些jar工具包，可以直接将java对象或集合转换成json格式的字符串。

**常见的json转换工具**

|工具名称|介绍|
|:-:|:-:|
| Jsonlib | Java类库，需要导入的jar包较多。|
| Gson    | Google提供的一个简单的json转换工具。|
| Fastjson| Alibaba技术团队提供的一个高性能的json转换工具。|
| Jackson | 开源免费的json转换工具，SpringMVC默认使用Jackson。|
> 其实这些工具使用起来都差不多, 目前我们学习使用的是Fastjson

### fastjson 常用 API

**fastjson 作用:**
1. 将java对象转成json字符串
2. 将json字符串 转成 java对象
**常用API**
fastjson API 入口类是`com.alibaba.fastjson.JSON`,常用的序列化操作都可以在`JSON`类上的静态方法直接完成。

```java
public static final String toJSONString(Object object); // 将JavaBean序列化为JSON文本 
public static final <T> T parseObject(String text, Class<T> clazz); // 把JSON文本解析成指定类型JavaBean 
public static final <T> List<T> parseArray(String text, Class<T> clazz); //把JSON文本解析成JavaBean集合 
```


```java
public String getString(String key); //获取jsonObject对象中的key的值
```
### fastjson 使用实例

导包

```xml
  <!--fastjson-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.47</version>
        </dependency>
```

实体类

```java
public class User {
    private String username;
    private String password;

    public User() {
    }

    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    @Override
    public String toString() {
        return "User{" +
                "username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
    //TODO: 自己采用字符串拼接的方式输出。
    public String toJson() {
        return "{\"username\":\""+username+"\",\"password\":"+password+"}";
    }
    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```

测试

```java
/*
    TODO 需求:
        java对象或集合 转换成 json格式字符串
        json格式字符串 转换成 java对象或集合
 */
public class JsonDemo {
    @Test
    public void method01(){
        User user = new User("zs", "123");
        System.out.println(user.toJson());
    }
    @Test
    public void method02(){
        //java对象转json格式字符串
        User user = new User("ls", "123");
        //{"password":"123","username":"ls"}
        String s = JSON.toJSONString(user);
        System.out.println(s);
    }
    @Test
    public void method03(){
        //java集合转json格式字符串
        User user1 = new User("ls", "123");
        User user2 = new User("ww", "110");

        ArrayList<User> list = new ArrayList<>();
        list.add(user1);
        list.add(user2);
        //[{"password":"123","username":"ls"},{"password":"110","username":"ww"}]
        String s = JSON.toJSONString(list);
        System.out.println(s);
    }
    @Test
    public void method04(){
        //json格式字符串转java对象
        String json = "{\"password\":\"123\",\"username\":\"ls\"}";
        /*
            ORM: object relationship mapping 对象关系映射
            1. 体现: 一条数据映射成一个java对象
            2. 原理: 反射
                1). 首先解析json字符串,变成map
                2). 然后遍历map
                        key 对应 value
                        username  ls   <-
                        password  123
                3). 反射操作对象
                    // User类的Class对象
                    Class clazz = User.class;
                    // 使用User类的空参构造创建实例
                    User user = clazz.newInstance();
                    //获取类中方法
                   // 方法名的推断: set + map中的key
                   Method method = clazz.getMethod("setUsername",String.class);
                   //调用方法 user.setUsername("ls");
                   method.invoke(user,"ls")
         */
        //User{username='ls', password='123'}
        User user = JSON.parseObject(json, User.class);
        System.out.println(user);
    }

}
```

## ajax和json综合

前端代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>ajax和json综合</h1>
    <input type="button" value="请求和响应都是普通字符串" onclick="method01()"> <br>
    <!--
        获取好友列表
    -->
    <input type="button" value="响应数据改成json字符串" onclick="method02()"> <br>
    <!--
        学习了前端框架vue就知道(双向数据绑定)
    -->
    <input type="button" value="请求数据也改成json字符串" onclick="method03()"> <br>

    <hr>
    <h3>好友列表</h3>
    <div id="messageDiv"></div>
    <table width="500px" cellspacing="0px" cellpadding="5px" border="1px" id="myTable">
        <tr>
            <th>id</th>
            <th>name</th>
            <th>age</th>
        </tr>
       <!-- <tr>
            <td></td>
            <td></td>
            <td></td>
        </tr>-->
    </table>
</body>
<script src="js/axios-0.18.0.js"></script>
<script>
    function method01() {
        let url = "/Union01Servlet"
        let param = "username=admin&password=123"

        // axios.get(url+"?"+param)

        axios.post(url,param).then(response=>{
            console.log(response.data);
        }).catch(error=>{
            console.log(error);
        }).finally(()=>{
            console.log("无论如何都执行");
        })
    }
</script>
<script>
    function method02() {
        let url = "/Union02Servlet"
        let param = "username=admin&password=123"

        axios.post(url,param).then(response=>{
            console.log(response.data);
            var messageDiv = document.getElementById("messageDiv");
            var myTable = document.getElementById("myTable");
            if(response.data.flag){
                //请求成功
                messageDiv.innerHTML = response.data.message
                let content = ""
                 for(let i=0;i<response.data.data.length;i++) {
                    let friend = response.data.data[i];
                    content += `<tr>
                                    <td>${friend.id}</td>
                                    <td>${friend.name}</td>
                                    <td>${friend.age}</td>
                                </tr>`;
                }
                myTable.innerHTML += content
            }else{
                //请求失败
                messageDiv.innerHTML = response.data.message
            }
        })
    }
</script>

<script>
    function method03() {
        let url = "/Union03Servlet"
        /*
            TODO: 请求数据改成json格式字符串
         */
        // let param = "username=admin&password=123"
        let param = {username : "admin",password : "123"}

        axios.post(url,param).then(response=>{
            console.log(response.data);
            var messageDiv = document.getElementById("messageDiv");
            var myTable = document.getElementById("myTable");
            if(response.data.flag){
                //请求成功
                messageDiv.innerHTML = response.data.message
                let content = ""
                for(let i=0;i<response.data.data.length;i++) {
                    let friend = response.data.data[i];
                    content += `<tr>
                                    <td>${friend.id}</td>
                                    <td>${friend.name}</td>
                                    <td>${friend.age}</td>
                                </tr>`;
                }
                myTable.innerHTML += content
            }else{
                //请求失败
                messageDiv.innerHTML = response.data.message
            }
        })
    }
</script>
</html>
```

**User代码**

```java
import java.io.Serializable;

public class User implements Serializable {
    private String username;
    private String password;

    @Override
    public String toString() {
        return "User{" +
                "username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```

**Friend 代码**

```java
import java.io.Serializable;

public class Friend implements Serializable {

    private String id;
    private String name;
    private Integer age;

    public Friend() {
    }

    public Friend(String id, String name, Integer age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }
}
```


**Result代码**

```java
/*
    Result : 结果
 */
public class Result implements Serializable {
    private boolean flag;//执行结果，true为执行成功 false为执行失败
    private String message;//返回结果信息
    private Object data;//返回数据(如果是查询操作则设置,如果是增删改则不设置)

    public Result() {
    }
    //失败,或者增删改
    public Result(boolean flag, String message){
        this.flag = flag;
        this.message = message;
    }
    //成功的查询
    public Result(boolean flag, String message, Object data) {
        this.flag = flag;
        this.message = message;
        this.data = data;
    }

    public boolean isFlag() {
        return flag;
    }

    public void setFlag(boolean flag) {
        this.flag = flag;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    public Object getData() {
        return data;
    }

    public void setData(Object data) {
        this.data = data;
    }
}

```

**BaseController代码**
```java
public class BaseController {

    /**
     *  post请求参数为json格式的数据 转换成 javaBean
     */
    public static <T>T getBean(HttpServletRequest request,Class<T> clazz) throws IOException {
        ServletInputStream is = request.getInputStream();
        T t = JSON.parseObject(is, clazz);
        return t;
    }

    public static void printResult(HttpServletResponse response, Result result) throws IOException {
        response.setContentType("application/json;charset=utf-8");
        String json = JSON.toJSONString(result);
        response.getWriter().print(json);
    }
}
```


**Union01Servlet代码**

```java
@WebServlet("/Union01Servlet")
public class Union01Servlet extends HttpServlet {

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 获取请求
            //解决post请求乱码问题
        request.setCharacterEncoding("utf-8");
        String username = request.getParameter("username");
        String password = request.getParameter("password");

        System.out.println(username + "," + password);

        //2. 业务处理

        //3. 响应数据
        response.setContentType("text/html;charset=utf-8");
        response.getWriter().println("响应数据");
    }
}
```

**Union02Servlet代码**
```java
@WebServlet("/Union02Servlet")
public class Union02Servlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        try {
            //1. 获取请求
            String username = request.getParameter("username");
            String password = request.getParameter("password");
            //2. 业务处理

//            int i = 1/0; // 模拟数据库崩溃
            //查询当前用户的好友 List<Friend> list,转成json格式字符串,最后响应
            //用伪数据代替
            Friend f1 = new Friend("1001", "张三", 18);
            Friend f2 = new Friend("1002", "李四", 19);
            Friend f3 = new Friend("1003", "王五", 20);
            ArrayList<Friend> list = new ArrayList<>();
            Collections.addAll(list,f1,f2,f3);

            //3. 响应数据
//        response.setContentType("text/html;charset=utf-8");
            response.setContentType("application/json;charset=utf-8");

            Result result = new Result(true, "获取好友列表成功", list);
            //fastjson等工具实现 (java对象或集合 转成 json格式字符串)
            String json = JSON.toJSONString(result);
            response.getWriter().print(json);
        } catch (Exception e) {
            response.setContentType("application/json;charset=utf-8");
            Result result = new Result(false, "获取好友列表失败");
            //fastjson等工具实现 (java对象或集合 转成 json格式字符串)
            String json = JSON.toJSONString(result);
            response.getWriter().print(json);
        }
    }
}

```

**Union03Servlet代码**
```java
@WebServlet("/Union03Servlet")
public class Union03Servlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        try {
            /*
                TODO 1. 获取请求
                    1). 请求参数是传统格式 name=value&name=value
                        后端获取,用get和post统一获取api即可
                                 String username = request.getParameter("username");
                    2). 请求参数json格式
                        是无法通过 统一格式获取的
                        以post为例, 请求参数在请求体中
             */
//            String username = request.getParameter("username");
//            String password = request.getParameter("password");
//            System.out.println(username + "," + password);


//            ServletInputStream is = request.getInputStream();
//            User user = JSON.parseObject(is, User.class);

            User user = BaseController.getBean(request, User.class);
            System.out.println(user);

            //2. 业务处理

            Friend f1 = new Friend("1001", "张三", 18);
            Friend f2 = new Friend("1002", "李四", 19);
            Friend f3 = new Friend("1003", "王五", 20);
            ArrayList<Friend> list = new ArrayList<>();
            Collections.addAll(list,f1,f2,f3);

            //3. 响应数据
            Result result = new Result(true, "获取好友列表成功", list);
//            response.setContentType("application/json;charset=utf-8");
//            String json = JSON.toJSONString(result);
//            response.getWriter().print(json);

            BaseController.printResult(response,result);
        } catch (Exception e) {
            Result result = new Result(false, "获取好友列表失败");
//            response.setContentType("application/json;charset=utf-8");
//            String json = JSON.toJSONString(result);
//            response.getWriter().print(json);
            BaseController.printResult(response,result);
        }
    }
}

```

# 服务器

## 服务器介绍 

**服务器：**
服务器，是提供**计算服务**的设备。由于服务器需要响应服务请求，并进行处理，因此一般来说服务器应具备承担服务并且保障服务的能力。


> [!summary] 服务器分类 
> 1.硬件服务器：服务器的构成包括处理器、硬盘、内存、系统总线等，和通用的计算机架构类似，但是由于需要提供高可靠的服务，因此在处理能力、稳定性、可靠性、安全性、可扩展性、可管理性等方面要求较高。  
>
> 2.软件服务器:	服务器软件本质上是一个**应用程序**（由代码编写而成），运行在服务器设备上。能够接收请求并根据请求给客户端响应数据，发布资源(静态和动态)。数据库服务器、邮件服务器(易邮)、网页服务器（tomcat nginx发布网页）等

**常见的Web服务器**

<img src="https://article.biliimg.com/bfs/article/4390d47309a470dc96808c795a14695f9f5bb2b0.png" alt="image.png" style="zoom:50%;" />

****

## tomcat服务器

tomcat服务器属于网页服务器，用来发布动态和静态网页的，由Apache公司开发的开源免费的。

下载和安装都省略...

**目录结构介绍:**
![image.png](https://article.biliimg.com/bfs/article/f6e6363b01d7c2af370ba6fd2229ac8335f3bd95.png)

```
bin：脚本目录 *****
	启动脚本(启动服务器)：startup.bat 浏览器地址栏上输入访问地址
	停止脚本(停止服务器)：shutdown.bat

conf：配置文件目录 (config /configuration) *****
	核心配置文件：server.xml
	用户权限配置文件：tomcat-users.xml
	所有web项目默认配置文件：web.xml

lib：依赖库，tomcat和web项目中需要使用的jar包  *****

logs：日志文件.
	localhost_access_log.txt tomcat记录用户访问信息，..表示时间。
	例如：localhost_access_log.2017-04-28.txt
	
temp：临时文件目录，文件夹内内容可以任意删除。

webapps：默认情况下发布WEB项目所存放的目录。  *****

work：tomcat处理JSP的工作目录。 
```

## 使用tomcat服务器发布web项目

**步骤：**

1. 在webapps文件夹下创建heima文件夹

2. 并创建index.html页面
	![image.png](https://article.biliimg.com/bfs/article/04796d174dceefd12df5f97a6cc47ea33636d107.png)

3. 使用记事本打开html页面输入代码：
4. 启动tomcat
5. 访问：必须加项目名heima

使用开发工具idea创建web项目省略....

# Servlet

## Servlet概述

![image.png](https://article.biliimg.com/bfs/article/e9fd126686e32a677abc1b4f4c82b78016193d8c.png)

1. Servlet是一个<font color=#ff0000>接口</font>，即规范
2. 定义的实现类必须实现接口中的所有的抽象方法
3. Servlet全称Server Applet 服务器端的程序。是sun公司提供一套规范，用来**处理客户端请求、响应给浏览器的==动态web资源**==。其实servlet的实质就是java代码，通过java的API动态的向客户端输出内容，并且从客户端接收数据。
4. 一个类要想通过浏览器被访问到,那么这个类就必须**直接或间接的实现Servlet接口**

**Servlet作用**

> [!summary]
> 1. 接收客户端的请求        
> 2. 处理业务逻辑         
> 3. 响应给浏览器客户端         

<img src="https://article.biliimg.com/bfs/article/667e074be420cd98e2e44df6859cc3e90597c0fe.png" alt="image.png" style="zoom:67%;" />


## Servlet快速入门
#servlet的实现方式
**步骤:**
1. 创建web项目
2. 导入servlet依赖
	```
		<!--导入依赖-->
	    <dependencies>
	        <!--导入servlet依赖-->
	        <dependency>
	            <groupId>javax.servlet</groupId>
	            <artifactId>javax.servlet-api</artifactId>
	            <version>3.0.1</version>
	        </dependency>
	    </dependencies>
	```
3. 在创建的web项目中自定义类实现Servlet接口
4. 在自定义类中实现Servlet接口中的所有的抽象方法
5. 在实现Servlet接口的service方法体中书写代码处理业务逻辑
	```java
	/*
	    2.在创建的web项目中自定义类实现Servlet接口
	 */
	public class HelloWorldServlet implements Servlet{
	    //3.在自定义类中实现Servlet接口中的所有的抽象方法
	    //4.在实现Servlet接口的service方法体中书写代码处理业务逻辑
	    @Override
	    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
	        System.out.println("service....");
	    }
	    @Override
	    public void init(ServletConfig servletConfig) throws ServletException {
	    }
	    @Override
	    public ServletConfig getServletConfig() {
	        return null;
	    }
	    @Override
	    public String getServletInfo() {
	        return null;
	    }
	    @Override
	    public void destroy() {
	    }
	}
	```
	5.. 在web项目的核心配置文件web.xml中配置访问servlet的路径。
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
		  http://java.sun.com/xml/ns/javaee/web-app_3_1.xsd"
           version="3.1">
    <!--
       5.在web项目的核心配置文件web.xml中配置访问servlet的路径。
​	    说明：这样配置是告知tomcat有具体的Servlet类需要被访问。
    -->
    <!--
        1.<servlet> 表示将当前Servlet类注册到tomcat中，告知tomcat有一个类要被访问
    -->
    <servlet>
        <!--
            表示当前要被访问类的标识，在当前web.xml中要唯一，helloWorldServlet属于标识符
        -->
        <servlet-name>helloWorldServlet</servlet-name>
        <!--
            配置要访问 的servlet类，必须是类的全路径：包名.类名。
            说明：tomcat底层通过获取这里的类全路径使用反射技术调用当前类的无参构造方法创建对象
        -->
        <servlet-class>sh.a_demo_01.HelloWorldServlet</servlet-class>
    </servlet>
    <!--
        配置要访问的servlet类的映射路径
    -->
    <servlet-mapping>
        <!--这里要和上面的servlet-name文本值一致，这里找到上面的servlet-name-->
        <servlet-name>helloWorldServlet</servlet-name>
        <!--浏览器上地址栏上输入的映射路径及访问路径，这里必须加/-->
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>
```
6. 启动tomcat
	![image.png](https://article.biliimg.com/bfs/article/65dec5fef5944db11b7e2a689d45eff25bfdca76.png)
7. 在浏览器中访问servlet类
	 ![image.png](https://article.biliimg.com/bfs/article/72b12c3a193f7b20f52a28fc6984f49ea3fb2c9b.png)
## Servlet的执行原理

**执行流程：**
![image.png](https://article.biliimg.com/bfs/article/c4cf4b1b7c77b08951fb249bdb7fad52f6334ffe.png)

**原理：**
> [!important] 说明
> 1.当我们点击run运行的时候，tomcat之所以会启动，是因为程序入口(main方法)在tomcat中        
> 2.tomcat开始运行，会加载web项目里面的配置文件web.xml(xml解析，读取数据)            
> 主要是根据url-pattern 找到对应的servlet-class
> 3.然后tomcat进入等待状态(永不停止，除非手动关闭)             
> 4.当用户在浏览器中输入地址：[http://localhost:8080/hello](http://localhost:8080/hello)就会定位到tomcat的访问的项目下面的某个servlet中           
> 5.tomcat会根据 /hello 的servlet的虚拟路径 找到HelloServlet的全限定名            
> 6.tomcat底层通过**反射创建HelloServlet的对象**，并调用HelloServlet的service方法：    
> ```java
> Class clazz = Class.forName("全限定名");
Servlet servlet = clazz.newInstance();//实际上HelloServlet对象，向上转型
servlet.service();

## Servlet生命周期
### 生命周期
**1.生命周期：**
指的是一个对象从生（创建）到死（销毁）的一个过程

**2.生命周期的api:**
```java
// 1. servlet对象创建完毕，使用对象调用此方法，初始化方法，只有在第一次访问的时候执行一次
public void init(ServletConfig servletConfig);
// 2. 用户访问servlet时，调用此方法 (每次访问都会调用一次)
public void service(ServletRequest servletRequest, ServletResponse servletResponse);
// 3. servlet对象销毁时，调用此方法
public void destroy();
```

**3.Servlet生命周期的api执行时机图解**
![image.png](https://article.biliimg.com/bfs/article/33c105bd03b0b3cfbd68ebb3a572c42b332eb073.png)

```
* 创建
	1）默认情况下
		用户第一次访问时，创建servlet，执行init方法
	
* 运行（提供服务）
		用户每次访问时，都执行service方法

* 销毁
		服务器正常关闭时，销毁servlet，执行destroy方法
```

### 服务器启动，立刻加载Servlet对象

**问题:** 
	发现 init 默认**第一次被访问**的时候才调用,适合用来初始化项目数据， 如果项目数据很多, 加载就需要一定的时间，这样就会给用户的体验不好,因为要等比较久的时间

**解决:** 服务器一启动,就执行init方法

**服务器一启动,就执行init方法与第一次访问时执行init()区别：**
当服务器启动时，Servlet容器会扫描Web应用程序中的所有Servlet，并将它们实例化并初始化。在这种情况下，所有的Servlet都会在服务器启动时执行init()方法，而不是在第一次访问时执行。

**实现：在web.xml核心配置文件中对应的servlet标签中按照如下配置：**

> [!note]+ 注意
> 1.使用`<load-on-startup></load-on-startup>`标签进行配置，表示标记容器是否应该在启动的时候加载这个servlet，(实例化并调用其init()方法)    
> 2.它的文本值必须是一个整数，表示servlet应该被载入的顺序    
> 3.如果文本值是负数：默认值是-1 【用户第一次访问时，创建】       
> 4.当值大于等于0时，表示容器在应用启动时就加载并初始化这个  servlet；        
> 5.正数的值越小，该servlet的优先级越高，应用启动时就越先加载。  
> 6.当值相同时，容器就会自己选择顺序来加载             

```xml
 <servlet>
        <servlet-name>life01Servlet</servlet-name>
        <servlet-class>com.itheima.sh.a_servlet_01.Life01Servlet</servlet-class>
        <!--
            load-on-startup 标签可以让tomcat服务器启动就创建对应的servlet。标签文本值必须是整数：
            数字越小，创建servlet的优先级越高
        -->
        <load-on-startup>2</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>life01Servlet</servlet-name>
        <url-pattern>/life01</url-pattern>
    </servlet-mapping>
```

## Servlet实现方式
一共有三种：
![image.png](https://article.biliimg.com/bfs/article/e83e1342a6f7468a2ea256973d2b82ffae944da6.png)

### 实现Servlet方式二_自定义类继承GenericServlet
【1】描述问题：
Servlet中使用频率最高,最重要的方法是`service`方法(大部分场景)
但是我们每次编写`Servlet`实现类,都是直接实现Servlet接口,重写5个抽象方法(太冗余了)
解决问题：
【2】解决问题：
我们可以自定义类继承`GenericServlet`抽象类，只在子类中重写service即可。不用重写所有的抽象方法。
【3】步骤：
1.自定义类继承GenericServlet类
2.在子类中重写service方法，处理业务逻辑
3.在web.xml中进行映射路径的配置
4.在浏览器客户端访问servlet类
【4】代码实现:
```java
/*
1.自定义类继承GenericServlet类
 */
public class Demo01Servlet extends GenericServlet {
    //    2.在子类中重写service方法，处理业务逻辑
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("service....");
    }
}
```

### 实现Servlet方式三_自定义类继承HttpServlet

我们在前端的form表单中,method属性, 学习过有两种常用的[[前端html#^postandget|请求方式]](get/post) 我们现在的service方法是这样的: 用户发送请求,无论是什么请求方式,**都会统一的执行service方法**, 我们无法很好的区别是哪一种请求方式

【2】解决问题：我们可以自定义类继承HttpServlet就可以根据不同的请求做不同的处理：get post
![image.png](https://article.biliimg.com/bfs/article/151fd1191521dc8a3c65e00711e2ec9297632ce9.png)
【3】步骤：
```
1.自定义类继承HttpServlet
2.在子类中根据不同的请求方式重写请求方式的方法：
	get请求---重写doGet方法
	post请求---重写doPost方法
3.在方法体中书写处理业务逻辑的代码
4.在web.xml中进行配置
5.浏览器客户端访问servlet
```
【4】实现
```java
/*
    1.自定义类继承HttpServlet
 */
public class Demo02Servlet extends HttpServlet {
    /*
        	2.在子类中根据不同的请求方式重写请求方式的方法：
​			get请求---重写doGet方法
​			post请求---重写doPost方法
     */
    //	3.在方法体中书写处理业务逻辑的代码
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("get....");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("post....");
    }
}
```

疑问：就是正常浏览器访问tomcat服务器需要访问servlet接口中的service方法，但是方式三在子类中没有重写servlet中的service方法，只重写了doGet和doPost方法，那么底层是如何执行的呢？
![image.png](https://article.biliimg.com/bfs/article/dcbcb8b2d563bcca457948576d8e2498a8eb4b1f.png)

当我们访问自定义类的`servlet`的时候先访问`HttpServlet`类实现Servlet接口中的`service`方法，在service方法体中**调用了重载的service方法**，在该方法体内部获取请求方式，根据不同的请求方式来执行对应的方法。
```
get请求----doGet()方法
post请求---doPost()方法
```

### servlet常见问题
1. 遇到500错误
	表示服务器内部==异常==。
	![image.png](https://article.biliimg.com/bfs/article/4a0c814544ef3428b07bcf71cd6dedf9f32ca362.png)
2. 遇到404错误
	浏览器客户端访问服务器的资源不存在。
	![image.png](https://article.biliimg.com/bfs/article/1ca66fdff07e37b28c964cdfd86de506ffaf552f.png)
3. 遇到405错误
	服务器servlet没有重写doGet或者doPost方法。
	![image.png](https://article.biliimg.com/bfs/article/ee1c7cc9ee00894382aa6e1625a5bbdbb0bfd7f7.png)

## Servlet路径问题
### servlet映射路径
 > [!example]+
> 1.**完全路径匹配**：就是访问什么在web.xml中配置什么路径。`/hello  /user`  掌握        
>   2.**目录匹配**：`/user/*` 只要访问以/user开始的的路径都可以访问  
>   3.**后缀名匹配**：`*.do  *.action   注意这里不能书写/ ` 访问以`.do`或者`.action`结尾的资源路径，后缀名都属于标识符     
>   4.**缺省路径**：`/ `     如果上述三种路径都不满足就访问缺省路径。    

上述访问路径的优先级：  **完全路径匹配 > 目录匹配  >  后缀名匹配  > 缺省路径**

### 绝对路径
【1】**绝对路径有两种写法**：
```
 1.带网络三要素：
    http://ip地址:端口号/资源路径
 2.不带网络三要素：
    /资源路径   这里的/不能省略 ，要求访问的资源必须在同一个服务器上，我们这里指yong'yi'g
```

【2】**代码实现**

**html:**
```html
<a href="http://127.0.0.1:8080/pathAbso">带网络三要素的绝对路径</a><br>
<a href="/pathAbso">不带网络三要素的绝对路径</a><br>
```

### 相对路径
 **相对路径：** 不是相对当前项目，而是针对==当前浏览器地址栏上的url==而言的。
**案例一：**
```
 #假设我们在浏览器地址栏访问的页面路径： http://localhost:8080/demo01.html
 #而在demo01.html页面想使用相对路径访问servlet： http://localhost:8080/pathAbso
 	 说明：
        如果在http://localhost:8080/demo01.html 页面中      访问 http://localhost:8080/pathAbso 该servlet，
        我们通过url观察发现只有最后一级目录不一样，所以在       demo01.html页面中相对的路径的写法是：
        ./pathAbso  这里的./表示当前路径 
        可以省略不写即直接写 pathAbso
```
demo01.html
```html
	<a href="./pathAbso">相对路径</a><br>
    <a href="pathAbso">相对路径</a><br>
```
![image.png](https://article.biliimg.com/bfs/article/d8358945dfafea482fb47974593132a15af3923f.png)
**案例二：**
```
  # 如果在http://localhost:8080/aaa/demo02.html 页面中访问 http://localhost:8080/pathAbso 该servlet
    我们通过url观察发现在demo02.html也面中书写访问的     servlet即pathAbso和当前页面的父目录aaa是同等目录，所以我这里先找该页面的父目录，然后在找该servlet即pathAbso
    ../pathAbso    ../表示上一级目录或者父目录，找到父目录之后再找servlet即pathAbso
```

demo02.html
```html
<a href="../pathAbso">相对路径</a><br>
```
#servlet的实现方式 
## Servlet3.0注解开发
1】问题
说明：之前我们都是使用web.xml进行servlet映射路径的配置。这样配置的弊端：web.xml中具有非常多个配置信息，显得非常臃肿并且容易出错。
【2】解决问题
使用web.xml配置映射路径的方式属于servlet2.5的技术。从servlet3.0开始引入注解配置访问servlet取代了web.xml配置。
【3】配置步骤：
```
1.在包上右键---new---servlet(create new Servlet)
2.输入类名
3.在方法体内输入逻辑代码
4.在浏览器地址栏中输入访问的路径
```
【4】实现
1.在包上右键---new---servlet(create new Servlet)
![image.png](https://article.biliimg.com/bfs/article/ba715a6b6f518e9c0dffa58f14395b753fcc28d5.png)
2.在方法体内输入逻辑代码
```java
@WebServlet("/annoDemo01Servlet")
public class AnnoDemo01Servlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("注解开发");
    }
}
```

# Request&Response

## Request和Response的概述

**重点**
1. service方法的两个参数`request`和`response`是由tomcat创建的
	`void service(ServletRequest var1, ServletResponse var2)`
2. `request` 表示请求数据, `tomcat`将浏览器发送过来的请求数据解析并封装到request对象中
		servlet开发者可以通过request对象获得请求数据
3. `response` 表示响应数据,服务器发送给浏览器的数据
		`servlet`开发者可以通过`response`对象设置响应数据
	![image.png](https://article.biliimg.com/bfs/article/99417a0b9d442d776bba3680de7b2251b5039d8f.png)

==Request是请求对象，Response是响应对象。==这两个对象在我们使用Servlet的时候有看到：

此时，我们就需要思考一个问题request和response这两个参数的作用是什么?
![image.png](https://article.biliimg.com/bfs/article/dc1ba5d6c32e3219fb0a4adb9fe9d6532b2526d4.png)

- request:==获取==请求数据
    - 浏览器会发送HTTP请求到后台服务器`[Tomcat]`
    - HTTP的请求中会包含很多请求数据`[请求行+请求头+请求体]`
    - 后台服务器`[Tomcat]`会对HTTP请求中的数据进行解析并把解析结果存入到一个对象中
    - 所存入的对象即为request对象，所以我们可以从request对象中获取请求的相关参数
    - 获取到数据后就可以继续后续的业务，比如获取用户名和密码就可以实现登录操作的相关业务
- response:==设置==响应数据
* 业务处理完后，后台就需要给前端返回业务处理的结果即响应数据
* 把响应数据封装到response对象中
* 后台服务器`[Tomcat]`会解析response对象,按照`[响应行+响应头+响应体]`格式拼接结果
* 浏览器最终解析结果，把内容展示在浏览器给用户浏览

## Request对象

### Request继承体系

在学习这节内容之前，我们先思考一个问题，前面在介绍Request和Reponse对象的时候，比较细心的同学可能已经发现：

- 当我们的Servlet类实现的是Servlet接口的时候，service方法中的参数是`ServletRequest`和`ServletResponse`
  
- 当我们的Servlet类继承的是`HttpServlet`类的时候，`doGet`和`doPost`方法中的参数就变成`HttpServletRequest`和`HttpServletReponse`
  

那么，

- ServletRequest和HttpServletRequest的关系是什么?
  
- request对象是有谁来创建的?
  
- request提供了哪些API,这些API从哪里查?
  

首先，我们先来看下Request的继承体系:
![image.png](https://article.biliimg.com/bfs/article/928e92403d4ca437a625d048fae4139b61ea8bc7.png)
从上图中可以看出，ServletRequest和HttpServletRequest都是Java提供的，而RequestFacade是Tomcat定义的。

所以ServletRequest和HttpServletRequest是继承关系，并且两个都是接口，接口是无法创建对象，这个时候就引发了下面这个问题:
![image.png](https://article.biliimg.com/bfs/article/51c0bd8519d045d55622758bb92b30e4cde0cf4b.png)

这个时候，我们就需要用到Request继承体系中的`RequestFacade`:

- 该类实现了`HttpServletRequest`接口，也**间接实现了**`ServletRequest`接口。
  
- Servlet类中的service方法、doGet方法或者是doPost方法最终都是由Web服务器`[Tomcat]`来调用的，所以Tomcat提供了方法参数接口的具体实现类，并**完成了对象的创建**
  
- 要想了解RequestFacade中都提供了哪些方法，我们可以直接查看JavaEE的API文档中关于ServletRequest和HttpServletRequest的接口文档，因为RequestFacade实现了其接口就需要重写接口中的方法
对于上述结论，要想验证，可以编写一个Servlet，在方法中把request对象打印下，就能看到最终的对象是不是RequestFacade,代码如下:
```java
@WebServlet("/demo2")
public class ServletDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println(request);
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    }
}
```
启动服务器，运行访问`http://localhost:8080/request-demo/demo2`,得到运行结果:

`org.apache.catalina.connector.RequestFacade@5ed6c830`

###  Request获取请求数据

#### 获取请求行数据

请求行包含三块内容，分别是`请求方式`、`请求资源路径`、`HTTP协议及版本`
![](https://article.biliimg.com/bfs/article/12644ee677944cd6b8f3dc6acc258e322405a280.png)

对于这三部分内容，request对象都提供了对应的API方法来获取，具体如下:

|描述| 方法 |
|:-:|:-:|
| 获取请求方式: `GET` | `String getMethod()` |
| 获取虚拟目录(项目访问路径): `/request-demo` | `String getContextPath()` |
| 获取URL(统一资源定位符): `http://localhost:8080/request-demo/req1` | `StringBuffer getRequestURL()` |
| 获取URI(统一资源标识符): `/request-demo/req1` | `String getRequestURI()` |
| 获取请求参数(GET方式): `username=zhangsan&password=123` | `String getQueryString()` |

介绍完上述方法后，咱们通过代码把上述方法都使用下:
```java
/**
 * request 获取请求数据
 */
@WebServlet("/req1")
public class RequestDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // String getMethod()：获取请求方式： GET
        String method = req.getMethod();
        System.out.println(method);//GET
        // String getContextPath()：获取虚拟目录(项目访问路径)：/request-demo
        String contextPath = req.getContextPath();
        System.out.println(contextPath);
        // StringBuffer getRequestURL(): 获取URL(统一资源定位符)：http://localhost:8080/request-demo/req1
        StringBuffer url = req.getRequestURL();
        System.out.println(url.toString());
        // String getRequestURI()：获取URI(统一资源标识符)： /request-demo/req1
        String uri = req.getRequestURI();
        System.out.println(uri);
        // String getQueryString()：获取请求参数（GET方式）： username=zhangsan
        String queryString = req.getQueryString();
        System.out.println(queryString);
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    }
}
```

#### 获取请求头数据
对于请求头的数据，格式为`key: value`如下:

`User-Agent: Mozilla/5.0 Chrome/91.0.4472.106`

所以根据请求头名称获取对应值的方法为:
```java
String getHeader(String name) 参数name书写的是请求头冒号左边的内容例如：User-Agent
	User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) 	Chrome/96.0.4664.110 Safari/537.36
```

接下来，在代码中如果想要获取客户端浏览器的版本信息，则可以使用
```java
/**
 * request 获取请求数据
 */
@WebServlet("/req1")
public class RequestDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取请求头: user-agent: 浏览器的版本信息
        String agent = req.getHeader("user-agent");
		System.out.println(agent);
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    }
}
```

重新启动服务器后，`http://localhost:8080/request-demo/req1?username=zhangsan&passwrod=123`，获取的结果如下:

`Nozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWlebKit/537.36(KITML,like Gecko)Chrome/92.0.4515.131 Safari/537.36`

#### 获取请求体数据
**浏览器在发送GET请求的时候是没有请求体的，所以需要把请求方式变更为POST**，请求体中的数据格式如下:

```
username=superbaby&password=123
```
对于请求体中的数据，Request对象提供了如下两种方式来获取其中的数据，分别是:

- 获取字节输入流，如果前端发送的是字节数据，比如传递的是文件数据，则使用该方法
```java
ServletInputStream getInputStream()
// 该方法可以获取字节
```
- 获取字符输入流，如果前端发送的是纯文本数据，则使用该方法
```java
BufferedReader getReader()
```

> [!abstract]
> 具体实现的步骤如下: 
> 
> 1.准备一个页面，在页面中添加form表单,用来发送post请求
> 
> 2.在Servlet的doPost方法中获取请求体数据
> 
> 3.在doPost方法中使用request的getReader()或者getInputStream()来获取
> 
> 4.访问测试

1. 在项目的webapp目录下添加一个html页面，名称为：`req.html`
2. 在Servlet的doPost方法中获取数据,调用`getReader()`或者`getInputStream()`方法，因为目前前端传递的是纯文本数据，所以我们采用`getReader()`方法来获取
```java
/**
 * request 获取请求数据
 */
@WebServlet("/req1")
public class RequestDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
         //获取post 请求体：请求参数
        //1. 获取字符输入流
        BufferedReader br = req.getReader();
        //2. 读取数据
        String line = br.readLine();
        System.out.println(line);
    }
}
```

==注意==

BufferedReader流是通过request对象来获取的，当请求完成后request对象就会被销毁，request对象被销毁后，BufferedReader流就会自动关闭，所以此处就不需要手动关闭流了。
1. 启动服务器，通过浏览器访问`http://localhost:8080/request-demo/req.html`

#### 获取请求参数的通用方式

在学习下面内容之前，我们先提出两个问题:

* 什么是请求参数?
* 请求参数和请求数据的关系是什么?

1. **什么是请求参数**?
	为了能更好的回答上述两个问题，我们拿用户登录的例子来说明
	1.1 想要登录网址，需要进入登录页面
	1.2 在登录页面输入用户名和密码
	1.3 将用户名和密码提交到后台
	1.4 后台校验用户名和密码是否正确
	1.5 如果正确，则正常登录，如果不正确，则提示用户名或密码错误
	上述例子中，**用户名和密码其实就是我们所说的请求参数。**
	**get请求：请求参数位于url后面。**
	**post请求：请求参数位于请求体中。**
	2.**什么是请求数据**?
	请求数据则是包含==请求行、请求头和请求体==的所有数据
	3.请求参数和请求数据的关系是什么?
	3.1 请求参数是请求数据中的部分内容
	3.2 如果是GET请求，请求参数在请求行中
	3.3 如果是POST请求，请求参数一般在请求体中

对于请求参数的获取,常用的有以下两种:

> [!summary]
> - GET方式:
> 	`String getQueryString()`
> - POST方式:
> 	`BufferedReader getReader();`

来实现一个案例需求:
1. 发送一个GET请求并携带用户名，后台接收后打印到控制台
2. 发送一个POST请求并携带用户名，后台接收后打印到控制台

```java
@WebServlet("/req1")
public class RequestDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        String result = req.getQueryString();
        System.out.println(result);

    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        BufferedReader br = req.getReader();
        String result = br.readLine();
        System.out.println(result);
    }
}
```

equest对象已经将上述获取请求参数的方法进行了封装，并且request提供的方法实现的功能更强大，以后只需要调用request提供的方法即可，在request的方法中都实现了哪些操作?
(1)根据不同的请求方式获取请求参数，获取的内容如下:
![image.png](https://article.biliimg.com/bfs/article/25b901030697481b4569b1eb0b546be9aa65af1e.png)
(2)把获取到的内容进行分割，内容如下:
![image.png](https://article.biliimg.com/bfs/article/708c9fba49171d220ddaea8233576baa73867fe5.png)
(3)把分割后端数据，存入到一个Map集合中:
<img src="https://article.biliimg.com/bfs/article/852f409fad282b3df2139ba4cf62107328a164ca.png" alt="image.png" style="zoom:50%;" />
**注意**:因为参数的值可能是一个，也可能有多个，所以Map的值的类型为String数组。
基于上述理论，request对象为我们提供了如下方法:

| 描述 | 方法 |
|:-:|:-:|
| 获取请求参数Map集合 | `Map<String, String[]> getParameterMap()` |
| 根据名称获取参数值（数组） | `String[] getParameterValues(String name)` |
| 根据名称获取参数值(单个值) | `String getParameter(String name)` |

2.在Servlet代码中获取页面传递GET请求的参数值
2.1获取GET方式的所有请求参数

```java
/**
 * request 通用方式获取请求参数
 */
@WebServlet("/req2")
public class RequestDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //GET请求逻辑
        System.out.println("get....");
        //1. 获取所有参数的Map集合
        Map<String, String[]> map = req.getParameterMap();
        for (String key : map.keySet()) {
            // username:zhangsan lisi
            System.out.print(key+":");
            //获取值
            String[] values = map.get(key);
            for (String value : values) {
                System.out.print(value + " ");
            }
            System.out.println();
        }
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    }
}
```

获取的结果为:
```
get....
username: zhangsan
password: 123
hobby:1 2
```

2.2获取GET请求参数中的爱好，结果是数组值
```java
/**
 * request 通用方式获取请求参数
 */
@WebServlet("/req2")
public class RequestDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //GET请求逻辑
        //...
        System.out.println("------------");
        String[] hobbies = req.getParameterValues("hobby");
        for (String hobby : hobbies) {
            System.out.println(hobby);
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    }
}
```
获取的结果为:
```
一一
1
2
```

获取GET请求参数中的用户名和密码，结果是单个值
```java
/**
 * request 通用方式获取请求参数
 */
@WebServlet("/req2")
public class RequestDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //GET请求逻辑
        //...
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        System.out.println(username);
        System.out.println(password);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    }
}
```

获取的结果为:

```
zhangsan
123
```

### 解决post请求乱码问题

- 从tomcat8开始以后，对于get请求乱码，tomcat已经解决。对于post请求中文乱码没有解决，需要我们自己处理。
- post请求乱码产生的原因和解决思路
- <img src="https://article.biliimg.com/bfs/article/9267d296e2d5032a36f2d5f680468fe21436f1cc.png" alt="image.png" style="zoom:50%;" />
说明：
> [!note]+ 
> 1)页面使用的编码表是`UTF-8`编码，tomcat使用的是默认编码表`ISO-8859-1`进行解码，编码和解码使用的编码表不一致导致乱码。
> 
> 2)解决思路：先按照ISO-8859-1编码，在按照UTF-8进行重新解码

【3】解决方案

解决方案有三种：

> [!summary]+ 方法总结
> 1.方案一
> - 使用URLEncoder类进行编码:static String encode(String s, String enc)
> 	- 参数：
> 		- s:编码的字符串
> 		- enc:使用编码表
> - 使用URLDecoder进行解码：static String decode(String s, String enc)
> 	- 参数：   
> 		- s:解码的字符串   
> 		- enc:使用编码表   
> 		2.方案二
> - 使用String类中的方法进行编码：   
> 	- `byte[] getBytes(String charsetName)`
> 	- 参数表示指定的编码表，返回值表示编码后的字节数组
> - 使用String类中的构造方法进行解码：
> 	- `String(byte[] bytes, String charsetName)`
> - 参数：
> 	- bytes：字节数组      
> 	- charsetName：表示指定的编码表         
> 	- 返回值：解码后的字符串           
> 
> 3.方案三
> - 如果是get请求，tomcat8底层已经帮助我们解决完了，我们只需要解决post乱码即可，但是上述两种方式对于post请求可以解决乱码，对于get请求本身获取到的已经是正确的数据，处理后又乱码了。
> - 我们的想法是：get请求不用我们自己书写代码处理乱码，只需要我们书写代码处理post乱码。
> - 我们接下来学习第三种解决方案：
> 	- 只解决来自于请求体数据的乱码。而get请求体没有数据，post请求体含有数据，所以我们可以理解为第三种处理方案只是用来解决post乱码的。使用的api是ServletRequest接口中的：
> 		- `void setCharacterEncoding(String env)`
> 	- 参数：指定的编码表
> - 注意：该方式的代码必须书写在获取请求数据之前

**代码实现**
```java
@WebServlet("/httpServletRequestDemo04Servlet")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.获取浏览器的请求数据
//        String username = request.getParameter("username");

        /*
            解决post乱码问题有三种方式：
            【1】方式一
                使用URLEncoder类进行编码:static String encode(String s, String enc)
                                参数：
                                    s:编码的字符串
                                    enc:使用编码表
                使用URLDecoder进行解码：static String decode(String s, String enc)
                                参数：
                                    s:解码的字符串
                                    enc:使用编码表
         */
        //1)编码 ： 使用URLEncoder类进行编码:static String encode(String s, String enc)
//        String encodeUsername = URLEncoder.encode(username, "ISO-8859-1");
//        //2)解码：使用URLDecoder进行解码：static String decode(String s, String enc)
//        username = URLDecoder.decode(encodeUsername, "UTF-8");

        /*
             解决post乱码问题有三种方式：
            【2】方式二：
                使用String类中的方法进行编码：    byte[] getBytes(String charsetName)
                                                  参数表示指定的编码表，返回值表示编码后的字节数组
                使用String类中的构造方法进行解码：String(byte[] bytes, String charsetName)
                                                参数：
                                                    bytes：字节数组
                                                    charsetName：表示指定的编码表
                                                返回值：解码后的字符串

         */
        //1)编码 ： 使用String类中的方法进行编码：    byte[] getBytes(String charsetName)
//        byte[] bytes = username.getBytes("ISO-8859-1");
//        //2)解码：使用String类中的构造方法进行解码：String(byte[] bytes, String charsetName)
//        username = new String(bytes, "UTF-8");

        //username = new String(username.getBytes("ISO-8859-1"), "UTF-8");

        /*
            解决post乱码问题有三种方式：
            【3】方式三：
                如果是get请求，tomcat8底层已经帮助我们解决完了，我们只需要解决post乱码即可，但是上述
                两种方式对于post请求可以解决乱码，对于get请求本身获取到的已经是正确的数据，处理
                后又乱码了。
                我们的想法是：get请求不用我们自己书写代码处理乱码，只需要我们书写代码处理post乱码。
                我们接下来学习第三种解决方案：
                只解决来自于请求体数据的乱码。而get请求体没有数据，post请求体含有数据，所以我们可以理解为第三种处理方案只是用来解决
                post乱码的。使用的api是ServletRequest接口中的：
                    void setCharacterEncoding(String env)
                        参数：指定的编码表
                注意：该方式的代码必须书写在获取请求数据之前
         */
        request.setCharacterEncoding("utf-8");//告知tomcat使用UTF-8解码页面请求数据

        //  1.获取浏览器的请求数据
        String username = request.getParameter("username");
        System.out.println("username = " + username);
    }
}
```

### Request请求转发

==请求转发(forward):一种在**服务器内部**的资源**跳转**方式。==

<img src="https://article.biliimg.com/bfs/article/24d44750a05ec571313097993023ec1accb95b32.png" alt="image.png" style="zoom:67%;" />

> [!info]+
> (1)浏览器发送请求给服务器，服务器中对应的资源A接收到请求
> 
> (2)资源A处理完请求后将请求发给资源B
> 
> (3)资源B处理完后将结果响应给浏览器
> 
> (4)请求从资源A到资源B的过程就叫==请求转发==

**请求转发的实现方式:**

```java
req.getRequestDispatcher("资源Bl路径").forward(req,resp);
说明：
	1）RequestDispatcher dispatcher = req.getRequestDispatcher("资源B路径");
	    RequestDispatcher表示转发器，该接口中有一个方法：forward(request,response)
```

具体如何来使用，我们先来看下需求:

![image.png](https://article.biliimg.com/bfs/article/1a1bfb14d6fd4698a1a733217a7cb29ef50b54f1.png)

针对上述需求，具体的实现步骤为:

> 1.创建一个RequestDemo5类，接收/req5的请求，在doGet方法中打印`demo5`
> 
> 2.创建一个RequestDemo6类，接收/req6的请求，在doGet方法中打印`demo6`
> 
> 3.在RequestDemo5的方法中使用
> 
> req.getRequestDispatcher("/req6").forward(req,resp)进行请求转发
> 
> 4.启动测试

在RequestDemo5的doGet方法中进行请求转发

```java
/**
 * 请求转发
 */
@WebServlet("/req5")
public class RequestDemo5 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("demo5...");
        //请求转发
        request.getRequestDispatcher("/req6").forward(request,response);
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

启动测试

访问`http://localhost:8080/request-demo/req5`,就可以在控制台看到如下内容:
```
demo5...
demo6...
```

说明请求已经转发到了`/req6`

请求转发资源间共享数据:使用Request对象
此处主要解决的问题是把请求从`/req5`转发到`/req6`的时候，如何传递数据给`/req6`。
需要使用request对象提供的三个方法:

|描述|方法|
|:-:|:-:|
| 存储数据到request域中 | `void setAttribute(String name, Object o)` |
| 根据key获取值 | `Object getAttribute(String name)` |
| 根据key删除该键值对 | `void removeAttribute(String name)` |

接着上个需求来:
![image.png](https://article.biliimg.com/bfs/article/49bccfa73a2302d01a4daa8ba1bc51eb4d849d2d.png)

> 1.在RequestDemo5的doGet方法中转发请求之前，将数据存入request域对象中
> 2.在RequestDemo6的doGet方法从request域对象中获取数据，并将数据打印到控制台
> 3.启动访问测试

(1)修改RequestDemo5中的方法
```java
@WebServlet("/req5")
public class RequestDemo5 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("demo5...");
        //存储数据
        request.setAttribute("msg","hello");
        //请求转发
        request.getRequestDispatcher("/req6").forward(request,response);

    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

(2)修改RequestDemo6中的方法

```java
/**
 * 请求转发
 */
@WebServlet("/req6")
public class RequestDemo6 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("demo6...");
        //获取数据
        Object msg = request.getAttribute("msg");
        System.out.println(msg);
    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

(3)启动测试
访问`http://localhost:8080/request-demo/req5`,就可以在控制台看到如下内容:

```
demo5...
demo6...
hello
```

此时就可以实现在转发多个资源之间共享数据。

**请求转发的特点**
- 浏览器**地址栏路径不发生变化**
	虽然后台从`/req5`转发到`/req6`,但是浏览器的地址一直是`/req5`,未发生变化
	![image.png](https://article.biliimg.com/bfs/article/0ef5cc28089ef3b699314a38c6cc85d109239964.png)
- 只能转发到当前服务器的内部资源
	不能从一个服务器通过转发访问另一台服务器
- 一次请求，可以在转发资源间使用request共享数据
	虽然后台从`/req5`转发到`/req6`，但是这个==只有一次请求==
- 问题：request.getParameter()


`request.getParameter(String name)`和`request.getAttribute(String name)`的区别
- `request.getParameter(String name)`:获取来自于浏览器的数据 `<input type="text" name="username" value="锁哥"/>` 
- `request.getParameter("username")`; 获取的是锁哥
- request.getAttribute(String name)获取的是服务器中的代码：`request.setAttibute(String name,Object obj)`;的数据 只能在同一请求中的不同servlet或JSP页面中共享。当请求被处理完成，request对象中存储的数据将被销毁。因此，这些数据可以被认为是临时数据，只适用于当前请求的处理过程，而随后的请求处理过程将需要重新设置这些数据。

```java
request.setAttribute("msg","黑马程序员"); 
String msg = (String) request.getAttribute("msg");
```

### request的生命周期

1.何时创建?
	浏览器第一次访问tomcat服务器的时候
2.谁创建?
	tomcat创建
3.创建对象做什么？
	浏览器第一次访问tomcat服务器的时候，tomcat创建request对象和response对象，传递给servlet中的service方法，然后我们可以在servlet中使用request对象调用方法获取请求数据(请求行 头 体)，然后处理业务逻辑，处理完毕，然后tomcat将响应数据给浏览器，浏览器接收到响应之后，tomcat立刻销毁request和response对象。所以下次再==请求时哪个request已经不是原来的request了==


## HTTP响应详解

<img src="https://article.biliimg.com/bfs/article/44b7cbd7f3beef79c42e19e3d7eb8bcd9524535a.png" alt="image.png" style="zoom:67%;" />

创建servlet

```java
@WebServlet("/getServlet")
public class GetServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //响应给浏览器数据
        response.getWriter().print("get....");
    }
}
```

```java
@WebServlet("/postServlet")
public class PostServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //响应给浏览器数据
        response.getWriter().print("post....");
    }
}

```

**抓包结果:**

![image.png](https://article.biliimg.com/bfs/article/748cb9253dbbc33870adca3414387985a46630e5.png)


> [!summary]
> 1.由于浏览器的原因，浏览器会把请求行和响应行信息放在了一起；
> 
> 2.get和post请求的响应没有区别；

### HTTP响应报文协议介绍

#### 响应行

响应行格式：协议/版本 状态码

> 如：HTTP/1.1 200 ;

|状态码|状态码描述|说明|
|:-:|:-:|:-:|
| **200** | OK                    | 请求已成功，请求所希望的响应头或数据体将随此响应返回。出现此状态码是表示正常状态。 |
| **302** | Move temporarily      |**重定向** ，请求的资源临时从不同的地址响应请求。|
| **304** | Not Modified          |从**浏览器**缓存中读取数据，不从服务器重新获取数据。例如，用户第一次从浏览器访问服务器端图片资源，以后在访问该图片资源的时候就不会再从服务器上加载而直接到浏览器缓存中加载，这样效率更高。 |
| **404** | Not Found             | 请求资源不存在。通常是用户路径编写错误，也可能是服务器资源已删除。 |
| 403     | Forbidden             | 服务器已经理解请求，但是拒绝执行它                           |
| 405     | Method Not Allowed    | 请求行中指定的请求方法不能被用于请求相应的资源，比如请求的方法没有重写doget和dopost |
| **500** | Internal Server Error | 服务器内部错误。通常程序抛异常                               |
#### 响应头

响应头也是用的键值对key:value，服务器基于**响应头**通知浏览器的行为。

![image.png](https://article.biliimg.com/bfs/article/60bd223eee1fa672470e0c2aaa080b71c3209540.png)

**常见的响应头** ：

|      响应头Key      |                         响应头value                          |
|:-:| :----------------------------------------------------------: |
|`location`|     指定响应的路径，需要与状态码302配合使用，完成重定向      |
|`content-Type`|响应正文的类型（MIME类型，属于服务器里面的一种类型，例如文件在window系统有自己的类型，.txt .doc .jpg。文件在服务器中也有自己的类型），同时还可以解决乱码问题。例如：text/html;charset=UTF-8|
|`content-disposition` |通过浏览器以附件形式解析正文，例如：`attachment;filename=xx.zip`|
|`refresh`|页面刷新，例如：3;url=www.itcast.cn //三秒刷新页面到www.itcast.cn|

常见的MIME类型：就是文件在tomcat服务器中的文件类型：

|文件类型| 扩展名 | MIME 类型 |
|:-:|:-:|:-:|
| HTML 文件 | .html | text/html |
| XML 文档 | .xml | text/xml |
| XHTML 文档 | .xhtml | application/xhtml+xml |
| 普通文本 | .txt | text/plain |
| PDF 文档 | .pdf | application/pdf |
| Microsoft Word 文件 | .word | application/msword |
| PNG 图像 | .png | image/png |
| GIF 图形 | .gif | image/gif |
| JPEG 图形 | .jpeg, .jpg | image/jpeg |

**简单的location应用：**
```java
 @Override
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    System.out.println("当前页面处理不了这个请求");
    //设置重定向
    response.setStatus(302);//设置302转台   response.setHeader("location","/responseDemo02Servlet");//Location属于固定的key,表示重定向的地址
  }
```

**简单的content-Type应用：**
```java

public class ResponseDemo01Servlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("image/jpeg"); // 设置响应正文类型为 JPEG 图像
        response.setHeader("Content-Disposition", "attachment;filename=test.jpg"); // 设置下载时的默认文件名为 test.jpg
        byte[] imageData = ... // 获取图片数据，此处省略
        response.getOutputStream().write(imageData); // 将图片数据写入响应正文
    }
}
```

**简单的`content-disposition`应用:**

```java
@Override
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    String filename = request.getParameter("filename");
    FileInputStream fis=new FileInputStream("D:\\download\\"+filename);
    response.setHeader("content-disposition","attachment;filename="+filename);
    ServletOutputStream out = response.getOutputStream();
    int b;
    while ((b=fis.read())!=-1)
    {
      out.write(b);
    }
    fis.close();
  }
```

**简单的`refresh`应用:**
```java
@Override
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    String website = request.getParameter("website");
    response.setHeader("refresh","3;url="+website);
  }
```

#### 响应体

响应体，就是服务器**发送给浏览器的数据**。当前浏览器向服务器请求的资源是hello.html，所以服务器给浏览器响应的数据是**一个html页面**。
请求资源路径：

![image.png](https://article.biliimg.com/bfs/article/dd8445d7b85b1d7f907c4f00651f7efb6d98a54e.png)

响应结果：
![image.png](https://article.biliimg.com/bfs/article/414acdc77a42635b4d23979ec6030a9d452007f8.png)

如果请求是servlet,那么浏览器的响应体接收到的是**servlet响应的数据：**
![image.png](https://article.biliimg.com/bfs/article/ae1eabb0b1983e30f7e387e150e5bc0dea5de5df.png)

![image.png](https://article.biliimg.com/bfs/article/22d5073ca8861925943fa9ab4a6a5d1f85cac767.png)


## Response对象

### Response对象介绍

前面讲解完Request对象，接下来我们回到刚开始的那张图:

![image.png](https://article.biliimg.com/bfs/article/908fd67c75402a899e91de0389243a57fac6bc80.png)

- **Request**:使用request对象来==获取==请求数据
- **Response**:使用response对象来==设置==响应数据

Reponse的继承体系和Request的继承体系也非常相似:
`HttpServletResponse response = new ResponseFacade()`;多态

![image.png](https://article.biliimg.com/bfs/article/b3416b6881fbe898e3ccf9f4c3d66fb0b91ba031.png)

### Response设置响应数据功能介绍

HTTP响应数据总共分为三部分内容，分别是==响应行、响应头、响应体==，对于这三部分内容的数据，respone对象都提供了哪些方法来进行设置?

1. 响应行
	![image.png](https://article.biliimg.com/bfs/article/c9ee6f47f78e7040b2f8b43784c77b35c0756a01.png)
	对于响应行，比较常用的就是设置响应状态码:
	`void setStatus(int sc);`
2. 响应头
	![image.png](https://article.biliimg.com/bfs/article/7f34228b90736d1ed724d7f23bee22b2d8f14463.png)
	设置响应头键值对：
	`void setHeader(String name,String value)`;
	**响应头**：name的值
		`location`  指定响应的路径
		`content-type`:告诉浏览器文件格式，告诉浏览器不要解析html文件(text/plain)，解决中文乱码问题
		`refresh` 定时刷新
		`content-disposition` 以附件形式展示图片等资源
3. 响应体
	![image.png](https://article.biliimg.com/bfs/article/8093d09e0545469403061bde9150fc02f44913e2.png)
	对于响应体，是通过==字符、字节输出流==的方式往浏览器写，

|方法| 描述 |
|:-:|:-:|
| `PrintWriter getWriter()` | 获取字符输出流 |
| `ServletOutputStream getOutputStream()` | 获取字节输出流 | 

### Respones请求重定向

==Response重定向(redirect):一种资源跳转方式(服务器外部的)==。

这里我们对比一下[[javaweb#Request请求转发|请求转发]]

<img src="https://article.biliimg.com/bfs/article/1a909d8bfb9fd03181a863f8227360b486a40f8e.png" alt="image.png" style="zoom:67%;" />

 > [!summary]+
> 1. 浏览器发送请求给服务器，服务器中对应的资源A接收到请求
> 2. 资源A现在无法处理该请求，就会给浏览器响应一个`302`的状态码+`location`的一个访问资源B的路径
> 3. 浏览器接收到响应状态码为302就会重新发送请求到location对应的访问地址去访问资源B
> 4. 资源B接收到请求后进行处理并最终给浏览器响应结果，这整个过程就叫==重定向==


2. 重定向的实现方式:
```java
 resp.setStatus(302);设置响应状态码是302  
 resp.setHeader("location","资源B的访问路径");
```

具体如何来使用，我们先来看下需求:

![image.png](https://article.biliimg.com/bfs/article/341494dce44c37561e8564b5015c75df9d035b41.png)

针对上述需求，具体的实现步骤为:

> 1.创建一个ResponseDemo1类，接收/resp1的请求，在doGet方法中打印`resp1....`
> 
> 2.创建一个ResponseDemo2类，接收/resp2的请求，在doGet方法中打印`resp2....`
> 
> 3.在ResponseDemo1的方法中使用
> 
> response.setStatus(302);
> 
> response.setHeader("Location","/request-demo/resp2") 来给前端响应结果数据
>
> 4.启动测试

创建ResponseDemo1类、创建ResponseDemo2类: 省略

在ResponseDemo1的doGet方法中给前端响应数据

```java
@WebServlet("/resp1")
public class ResponseDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("resp1....");
        //重定向
        //1.设置响应状态码 302
        response.setStatus(302);
        //2. 设置响应头 Location
        response.setHeader("Location","/request-demo/resp2");
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

启动测试

访问`http://localhost:8080/request-demo/resp1`,就可以在控制台看到如下内容:
```
resp1.. ..
resp2.. ..
```

说明`/resp1`和`/resp2`都被访问到了。到这重定向就已经完成了。

虽然功能已经实现，但是从设置重定向的两行代码来看，会发现除了重定向的地址不一样，其他的内容都是一模一样，所以resposne对象给我们提供了简化的编写方式为:

```java
 resposne.sendRedirect("/request-demo/resp2")
```

所以第3步中的代码就可以简化为：

```java
@WebServlet("/resp1")
public class ResponseDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("resp1....");
        //重定向
        resposne.sendRedirect("/request-demo/resp2")；
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

**重定向的特点**
- 浏览器地址栏路径发送变化
  
    当进行重定向访问的时候，由于是由浏览器发送的两次请求，所以地址会发生变化
    ![image.png](https://article.biliimg.com/bfs/article/c6c148b10de77a6ed6d89ab3a378c8010704d8de.png)
- 可以重定向到任何位置的资源(服务内容、外部均可)
  
    因为第一次响应结果中包含了浏览器下次要跳转的路径，所以这个路径是可以任意位置资源。
- 两次请求，不能在多个资源使用request共享数据
  
    因为浏览器发送了两次请求，是两个不同的request对象，就无法通过request对象进行共享数据
```java
@Override //@WebServlet("/responseDemo01Servlet")
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    System.out.println("当前页面处理不了这个请求"); 
    //重定向的简单实现
    request.setAttribute("name","opk");//数据保存在request对象中
    response.sendRedirect("/responseDemo02Servlet");
  }

  @Override //@WebServlet("/responseDemo02Servlet")
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    System.out.println("接受到来自另一个服务器的重定向");
   System.out.println(request.getAttribute("name"));//得不到这个数据
    response.getWriter().write("ni hao ya");

  }
```

介绍完==请求重定向==和==请求转发==以后，接下来需要把这两个放在一块对比下:

|特点| 重定向 | 请求转发 |
|:-:|:-:|:-:|
| 浏览器地址栏路径变化 |✔️| ❌ |
| 可以重定向到任意位置的资源（服务器内部、外部均可） |✔️| ❌ |
| 只能转发到当前服务器的内部资源 | ❌ |✔️|
| 两次请求，不能在多个资源使用 `request` 共享数据 |✔️|❌|
| 一次请求，可以在转发的资源间使用 `request` 共享数据 | ❌ |✔️|

**路径问题：**
1. 问题1：转发的时候路径上没有加`/request-demo`而重定向加了，那么到底什么时候需要加，什么时候不需要加呢?
	![image.png](https://article.biliimg.com/bfs/article/6074d114d359e1bc7f52c138e3f50629e04db2de.png)
	其实判断的依据很简单，只需要记住下面的规则即可:
- 浏览器使用:**需要加虚拟目录(项目访问路径)**
- 服务端使用:**不需要加虚拟目录**
对于转发来说，因为是==在服务端进行==的，所以不需要加虚拟目录
对于重定向来说，路径最终是由浏览器来发送请求，就需要添加虚拟目录。
掌握了这个规则，接下来就通过一些练习来强化下知识的学习:

- `<a href='路劲'>`
- `<form action='路径'>`
- `req.getRequestDispatcher("路径")`
- `resp.sendRedirect("路径")`
答案:
```
1.超链接，从浏览器发送，需要加
2.表单，从浏览器发送，需要加
3.转发，是从服务器内部跳转，不需要加
4.重定向，是由浏览器进行跳转，需要加。
```

### Response响应字符数据

要想将字符数据写回到浏览器，我们需要两个步骤:

- 通过Response对象获取字符输出流： `PrintWriter writer = resp.getWriter()`;
- 通过字符输出流写数据: `writer.write("aaa")`;

接下来，我们实现通过些案例把响应字符数据给实际应用下:

返回一个简单的字符串`aaa`

```java
/**
 * 响应字符数据：设置字符数据的响应体
 */
@WebServlet("/resp3")
public class ResponseDemo3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        //1. 获取字符输出流
        PrintWriter writer = response.getWriter();
		 writer.write("aaa");
    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

![image.png](https://article.biliimg.com/bfs/article/208fd9dc5ebec7bcd965813aee76722aabc67669.png)

返回一串html字符串，并且能被浏览器解析

```java
PrintWriter writer = response.getWriter();
//content-type，告诉浏览器返回的数据类型是HTML类型数据，这样浏览器才会解析HTML标签
response.setHeader("content-type","text/html");
writer.write("<h1>aaa</h1>");
```

![image.png](https://article.biliimg.com/bfs/article/a3dae885d14678dec298a56ae66d9d8df55f717d.png)

==注意:==一次请求响应结束后，response对象就会被销毁掉，所以不要手动关闭流。
返回一个中文的字符串`你好`，需要注意设置响应数据的编码为`utf-8`

```java
//设置响应的数据格式及数据的编码
response.setContentType("text/html;charset=utf-8");
writer.write("你好");
```
![image.png](https://article.biliimg.com/bfs/article/316f82b6629ca8efcf0be6b239a96c05165662ee.png)


### Response响应字节数据

要想将字节数据写回到浏览器，我们需要两个步骤:
- 通过Response对象获取字节输出流：ServletOutputStream outputStream = resp.getOutputStream();
- 通过字节输出流写数据: outputStream.write(字节数据);
接下来，我们实现通过些案例把响应字节数据给实际应用下:

返回一个图片文件到浏览器

```java
/**
 * 响应字节数据：设置字节数据的响应体
 */
@WebServlet("/resp4")
public class ResponseDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 读取文件
        FileInputStream fis = new FileInputStream("d://a.jpg");
        //2. 获取response字节输出流
        ServletOutputStream os = response.getOutputStream();
        //3. 完成流的copy
        byte[] buff = new byte[1024];
        int len = 0;
        while ((len = fis.read(buff))!= -1){
            os.write(buff,0,len);
        }
        fis.close();
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

上述代码中，对于流的copy的代码还是比较复杂的，所以我们可以使用别人提供好的方法来简化代码的开发，具体的步骤是:

上述代码中，对于流的copy的代码还是比较复杂的，所以我们可以使用别人提供好的方法来简化代码的开发，具体的步骤是:

**pom.xml添加依赖**
```xml
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>
```

**调用工具类方法**

```java
//fis:输入流
//os:输出流
IOUtils.copy(fis,os);
```

优化后的代码:

```java
/**
 * 响应字节数据：设置字节数据的响应体
 */
@WebServlet("/resp4")
public class ResponseDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 读取文件
        FileInputStream fis = new FileInputStream("d://a.jpg");
        //2. 获取response字节输出流
        ServletOutputStream os = response.getOutputStream();
        //3. 完成流的copy
      	IOUtils.copy(fis,os);
        fis.close();
    }
}
```


## 用户注册登录案例
用户登录
这里不写代码，只说流程

```python·
#web层：
    1.获取浏览器的请求数据
    2.创建实体类对象user
    3.将用户名和密码封装到对象user
    4.创建业务层对象
    5.使用业务层对象调用业务层登录方法
    6.判断业务层返回的对象是否是null
    7.是null，说明没有查到用户，登录失败，跳转到登录失败页面 #这里的跳转采用重定向
    8.如果不是null,登录成功，跳转到成功页面
    
#service层：   
    1.定义登录方法接收web层传递的用户对象
    2.使用mybatis工具类获取session对象
    3.使用session对象调用方法获取接口Mapper对象
    4.使用Mapper对象调用接口的登录方法
    User u = mapper.login(user);
    5.返回给web层对象u
#dao层：
 	1.定义接口UserMapper
 	2.在接口UserMapper中定义登录方法
```

用户注册

```java
dao层：
     //1.在接口中定义根据用户名查询的方法
     //2.在接口中定义注册方法
service层：
    //1.定义注册的方法接收web层传递的user对象
    //2.获取会话对象
    //3.获取接口代理对象
    //4.使用接口代理对象调用接口中的根据用户名查询的方法
    //5.判断返回的User对象是否等于null
    //6.如果等于null,说明没有查到，可以注册，使用接口代理对象调用注册方法
    //7.提交事务
    //8.返回给web层true
    //9.如果查询的用户对象不等于null,说明用户存在，不能注册，返回给web层false
    //10.释放资源
web层：
    	//1.处理请求乱码
        //2.获取页面的数据
        //3.创建User对象
        //4.将获取的数据封装到User对象中
        //5.创建业务层对象
        //6.使用业务层对象调用注册方法，将User对象作为参数传递到业务层
        //7.判断返回值
        //8.如果返回值是true，注册成功，跳转到登录页面
        //9.如果返回值是false，注册失败，跳转到失败页面
```


# 会话技术

## 会话跟踪技术的概述

对于`会话跟踪`这四个词，我们需要拆开来进行解释，首先要理解什么是`会话`，然后再去理解什么是`会话跟踪`:
- 会话:用户打开浏览器，访问web服务器的资源，会话建立，直到有一方**断开连接**，会话结束。在一次会话中可以包含==多次==请求和响应。
    - 从浏览器发出请求到服务端响应数据给前端之后，一次会话(在浏览器和服务器之间)就被建立了
    - 会话被建立后，如果浏览器或服务端都没有被关闭，则会话就会持续建立着
    - 浏览器和服务器就可以继续使用该会话进行请求发送和响应，上述的整个过程就被称之为==会话==。
    用实际场景来理解下会话，比如在我们访问京东的时候，当打开浏览器进入京东首页后，浏览器和京东的服务器之间就建立了一次会话，后面的搜索商品,查看商品的详情,加入购物车等都是**在这一次会话中**完成。
    思考:下图中总共建立了几个会话?
    ![image.png](https://article.biliimg.com/bfs/article/6f4e9d28e73c347085e379be8a3fe3e3748f35cf.png)
- 每个浏览器都会与服务端建立了一个会话，加起来总共是==3==个会话。
- **会话跟踪**:一种维护浏览器状态的方法，服务器需要识别多次请求是否来自于同一浏览器，以便在同一次会话的多次请求间==共享数据==。
    - 服务器会收到多个请求，多个请求可能来自多个浏览器，如上图中的6个请求来自3个浏览器
    - 服务器需要用来识别请求是否来自同一个浏览器
    - 服务器用来识别浏览器的过程，这个过程就是==会话跟踪==
    - 服务器识别浏览器后就可以在同一个会话中多次请求之间来共享数据
    那么我们又有一个问题需要思考，一个会话中的多次请求为什么要共享数据呢?有了这个数据共享功能后能实现哪些功能呢?
    - 购物车: `加入购物车`和`去购物车结算`是两次请求，但是后面这次请求要想展示前一次请求所添加的商品，就需要用到数据共享。
    - ![image.png](https://article.biliimg.com/bfs/article/97490d3aad97497945d2e574e90333192ecf0781.png)
    - 页面展示用户登录信息:很多网站，登录后访问多个功能发送多次请求后，浏览器上都会有当前登录用户的信息`[用户名]`，比如百度、京东、码云等。
    - ![image.png](https://article.biliimg.com/bfs/article/adfeb5e37e54f6f56ce6c0091e8b0d5b7e86ad2b.png)
    - 网站登录页面的`记住我`功能:当用户登录成功后，勾选`记住我`按钮后下次再登录的时候，网站就会自动填充用户名和密码，简化用户的登录操作，多次登录就会有多次请求，他们之间也涉及到共享数据
    - ![image.png](https://article.biliimg.com/bfs/article/8419c19c684e3d6bd6a159b8c6515bf7daefe607.png)
- 登录页面的验证码功能:生成验证码和输入验证码点击注册这也是两次请求，这两次请求的数据之间要进行对比，相同则允许注册，不同则拒绝注册，该功能的实现也需要在同一次会话中共享数据。
- ![image.png](https://article.biliimg.com/bfs/article/1a2ce67b60dba91a6b8510f7333430540d05fe01.png)
**为什么现在浏览器和服务器不支持数据共享呢?**
- 浏览器和服务器之间使用的是HTTP协议来进行数据传输
- HTTP协议是==无状态(无记忆、无关联)==的，每次浏览器向服务器请求时，服务器都会将该请求视为==新的==请求
- HTTP协议设计成无状态的目的是让每次请求之间相互独立，互不影响
- 请求与请求之间独立后，就无法实现多次请求之间的数据共享

如何实现会话跟踪技术呢? 具体的实现方式有:

> [!example]+ 实现方式
> 客户端会话跟踪技术：==Cookie==     
> 服务端会话跟踪技术：==Session==     

这两个技术都可以实现会话跟踪，它们之间最大的区别:==Cookie是存储在浏览器端而Session是存储在服务器端==

## Cookie

### Cookie的基本使用

**概念**

==Cookie==：客户端会话技术，将数据保存到客户端，以后每次请求都携带Cookie数据进行访问。

**Cookie的工作流程**

![image.png](https://article.biliimg.com/bfs/article/9c1a4557cf758c290bc3b5bb81aca7455d57f816.png)

- 服务端提供了两个Servlet，分别是`ServletA`和`ServletB`
- 浏览器发送HTTP请求1给服务端，服务端ServletA接收请求并进行业务处理
- 服务端ServletA在处理的过程中可以创建(**服务器创建的**)一个Cookie对象并将`name=zs`的数据存入Cookie
- 服务端ServletA在响应数据的时候，会把Cookie对象**响应给**浏览器
- 浏览器接收到响应数据，会把Cookie对象中的数据**存储在浏览器内存中**，此时浏览器和服务端就==建立了一次会话==
- ==在同一次会话==中浏览器再次发送HTTP请求2给服务端ServletB，浏览器会携带Cookie对象中的所有数据
- ServletB接收到请求和数据后，就可以获取到存储在Cookie对象中的数据，这样同一个会话中的多次请求之间就实现了数据共享
- **注意：cookie是创建在服务器端，存在浏览器端**
- 好处：减轻服务器压力，但是不安全。

**Cookie的基本使用**

对于Cookie的使用，我们更关注的应该是后台代码如何操作Cookie，对于Cookie的操作主要分两大类，分别是==发送Cookie==和==获取Cookie==,对于上面这两块内容，分别该如何实现呢?

#### 发送Cookie
- 创建Cookie对象，并设置数据
	`Cookie cookie = new Cookie("key","value");`
- 发送Cookie到客户端：使用==response==对象
	`response.addCookie(cookie);`
	介绍完发送Cookie对应的步骤后，接下面通过一个案例来完成Cookie的发送

在Servlet中创建Cookie对象，存入数据，发送给前端

```java
@WebServlet("/aServlet")
public class AServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //发送Cookie
        //1. 创建Cookie对象
        Cookie cookie = new Cookie("username","zs");
        //2. 发送Cookie，response
        response.addCookie(cookie);
    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

启动测试，在浏览器查看Cookie对象中的值
访问`http://localhost:8080/cookie-demo/aServlet`

#### 获取Cookie

- 获取客户端携带的所有Cookie，使用==request==对象
	`Cookie[] cookies = request.getCookies();`

- 遍历数组，获取每一个Cookie对象：for
- 使用Cookie对象方法获取数据 **[Cookie](../../../javax/servlet/http/Cookie.html#Cookie(java.lang.String,%20java.lang.String))**(String name, String value)
  
```java
cookie.getName(); 获取Cookie类构造方法的第一个参数 name  
cookie.getValue();获取Cookie类构造方法的第二个参数value
```

介绍完获取Cookie对应的步骤后，接下面再通过一个案例来完成Cookie的获取，具体实现步骤为:

> 需求:在Servlet中获取前一个案例存入在Cookie对象中的数据
> 1.编写一个新Servlet类，名称为BServlet
> 2.在BServlet中使用request对象获取Cookie数组，遍历数组，从数据中获取指定名称对应的值
> 3.启动测试，在控制台打印出获取的值

在BServlet中使用request对象获取Cookie数组，遍历数组，从数据中获取指定名称对应的值
```java
@WebServlet("/bServlet")
public class BServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取Cookie
        //1. 获取Cookie数组
        Cookie[] cookies = request.getCookies();
        //2. 遍历数组
        for (Cookie cookie : cookies) {
            //3. 获取数据
            String name = cookie.getName();
            if("username".equals(name)){
                String value = cookie.getValue();
                System.out.println(name+":"+value);
                break;
            }
        }

    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

启动测试，在控制台打印出获取的值
访问`http://localhost:8080/cookie-demo/bServlet`
![image.png](https://article.biliimg.com/bfs/article/8d096895650fe2f4fa8cd391aee394eafc91f612.png)
在IDEA控制台就能看到输出的结果:
![image.png](https://article.biliimg.com/bfs/article/5e063ac664357db7fcfc612e6d8ac151fb757169.png)

### Cookie的原理分析

对于Cookie的实现原理是基于HTTP协议的,其中设计到HTTP协议中的两个请求头信息:
- 响应头:`set-cookie`
- 请求头: `cookie`
![image.png](https://article.biliimg.com/bfs/article/805fd3638dd3a85a114fe1c8c5856ed7399d2fc0.png)

- 前面的案例中已经能够实现，AServlet给前端发送Cookie,BServlet从request中获取Cookie的功能
- 对于AServlet响应数据的时候，Tomcat服务器都是基于HTTP协议来响应数据
- 当**Tomcat发现后端要返回的是一个Cookie对象之后**，Tomcat就会在响应头中添加一行数据==`Set-Cookie:username=zs`==
- 浏览器获取到响应结果后，从响应头中就可以获取到`Set-Cookie`对应值`username=zs`,并将数据存储在浏览器的内存中
- 浏览器再次发送请求给BServlet的时候，浏览器会**自动在请求头中添加**==`Cookie: username=zs`==发送给服务端BServlet
- Request对象会把请求头中cookie对应的值封装成一个个Cookie对象，最终形成一个数组
- BServlet通过Request对象获取到`Cookie[]`后，就可以从中获取自己需要的数据

✍️重点先有**响应头**后有**请求头**

接下来，使用刚才的案例，把上述结论验证下:
(1)访问AServlet对应的地址`http://localhost:8080/cookie-demo/aServlet`
使用Chrom浏览器打开开发者工具(F12或Crtl+Shift+I)进行查看==响应头==中的数据
![image.png](https://article.biliimg.com/bfs/article/96cf78ff6517f026d14d837510dbc5691da91383.png)

（2）访问BServlet对应的地址`[http://localhost:8080/cookie-demo/bServlet](http://localhost:8080/cookie-demo/bServlet)
使用Chrom浏览器打开开发者工具(F12或Crtl+Shift+I)进行查看==请求头==中的数据

![image.png](https://article.biliimg.com/bfs/article/40bab66fa4f9eac65615c3824aecbe41635283dc.png)

### Cookie的使用细节
#### Cookie的存活时间

> cookie有两种：
> 1.**会话级别的cookie**：会话结束，那么cookie消失
> 2.**持久化级别的cookie**：当前会话结束，cookie并没有消失，而是保存，下次访问同一个服务器，cookie数据依然存在

前面让大家思考过一个问题:
<img src="https://article.biliimg.com/bfs/article/62353b4ba1d9ee70ff950617900a9120c543b103.png" alt="image.png" style="zoom:67%;" />

(1)浏览器发送请求给AServlet,AServlet会响应一个存有`usernanme=zs`的Cookie对象给浏览器
(2)浏览器接收到响应数据将cookie存入到浏览器内存中
(3)当浏览器再次发送请求给BServlet,BServlet就可以使用Request对象获取到Cookie数据
(4)在发送请求到BServlet之前，如果把浏览器关闭再打开进行访问，BServlet能否获取到Cookie数据?

针对上面这个问题，通过演示，会发现，BServlet中无法再获取到Cookie数据，这是为什么呢?
- 默认情况下，Cookie存储在浏览器内存中，当浏览器关闭，内存释放，则Cookie被销毁
这个结论就印证了上面的演示效果，但是如果使用这种默认情况下的Cookie,有些需求就无法实现，比如:
![image.png](https://article.biliimg.com/bfs/article/41e07714196a46a4764218b985b3a2307c7a7722.png)

上面这个网站的登录页面上有一个`记住我`的功能，这个功能大家都比较熟悉

- 第一次输入用户名和密码并勾选`记住我`然后进行登录
- 下次再登陆的时候，用户名和密码就会被自动填充，不需要再重新输入登录
- 比如`记住我`这个功能需要记住用户名和密码一个星期，那么使用默认情况下的Cookie就会出现问题
- 因为默认情况，浏览器一关，Cookie就会从浏览器内存中删除，对于`记住我`功能就无法实现
所以我们现在就遇到一个难题是如何将Cookie持久化存储?

Cookie其实已经为我们提供好了对应的API来完成这件事，这个API就是==setMaxAge==,
- 设置Cookie存活时间
	`setMaxAge(int seconds)`

> [!summary] 参数值为:     
> 1.正数：将Cookie写入浏览器**所在电脑的硬盘**，持久化存储。到时间自动删除  
> 2.负数：默认值，Cookie在当前**浏览器内存**中，当浏览器关闭，则Cookie被销毁       
> 3.零：删除对应Cookie 

接下来，咱们就在AServlet中去设置Cookie的存活时间。

```java
@WebServlet("/aServlet")
public class AServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //发送Cookie
        //1. 创建Cookie对象
        Cookie cookie = new Cookie("username","zs");
        //设置存活时间   ，1周 7天
        cookie.setMaxAge(60*60*24*7); //易阅读，需程序计算
		//cookie.setMaxAge(604800); //不易阅读(可以使用注解弥补)，程序少进行一次计算
        //2. 发送Cookie，response
        response.addCookie(cookie);
    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```
修改完代码后，启动测试，访问`http://localhost:8080/cookie-demo/aServlet`
- 访问一个AServlet后，把浏览器关闭重启后，再去访问`http://localhost:8080/cookie-demo/bServet`,能在控制台打印出`username:zs`,说明Cookie没有随着浏览器关闭而被销毁
- 通过浏览器查看Cookie的内容，会发现Cookie的相关信息

![image.png](https://article.biliimg.com/bfs/article/74e86f657a5d36fa062eaa2935d5f96ead248f1b.png)

#### 关于cookie中存储特殊字符问题

如果直接向cookie中存储特殊字符，例如**空格**,分号(;),逗号(,),等号(=)等特殊字符。那么就会出现问题。在向cookie中存储特殊字符之前**必须要先进行编码处理**，然后从cookie中取出之后在进行解码处理。

向cookie中存储特殊字符问题演示


```java
@WebServlet("/specialCookie01Servlet")
public class SpecialCookie01Servlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        /*
            向cookie中存储特殊字符问题演示
         */
        //1.创建Cookie类的对象
//        Cookie cookie = new Cookie("msg", "12 34");报错
        String str = "12 34";
        //编码
        String encode = URLEncoder.encode(str, "utf-8");
        Cookie cookie = new Cookie("msg", encode);
        //2.将cookie存储到浏览器端
        response.addCookie(cookie);
    }
}
```

![image.png](https://article.biliimg.com/bfs/article/b014bf4bc2717b495aca46e2328aca673d18ca5f.png)

【2】解决向cookie中存储特殊字符的问题
方案：在向cookie中存储特殊字符前进行编码，然后取出之后需要解码。

```java
@WebServlet("/specialCookie01Servlet")
public class SpecialCookie01Servlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        /*
            向cookie中存储特殊字符问题演示
         */
        //1.创建Cookie类的对象
//        Cookie cookie = new Cookie("msg", "12 34");报错
        String str = "12 34";
        //编码
        String encode = URLEncoder.encode(str, "utf-8");
        Cookie cookie = new Cookie("msg", encode);
        //2.将cookie存储到浏览器端
        response.addCookie(cookie);
    }
}
```

```java
@WebServlet("/specialCookie02Servlet")
public class SpecialCookie02Servlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.获取浏览器的cookie
        Cookie[] cookies = request.getCookies();
        //2.遍历cookie数组
        for (Cookie cookie : cookies) {
            //3.取出cookie的name
            String cookieName = cookie.getName();
            //4.判断cookieName的值是否是msg
            if("msg".equals(cookieName)){
                //5.取出value
                String value = cookie.getValue();
                //6.解码并输出
                String decode = URLDecoder.decode(value, "utf-8");
                System.out.println(decode);
            }
        }
    }
}
```

## Session

### Session的基本使用
==Session==：服务端会话跟踪技术：将数据**保存到服务端**。
- Session是存储在服务端而Cookie是存储在客户端
- 存储在客户端的数据容易被窃取和截获，存在很多不安全的因素
- 存储在服务端的数据相比于客户端来说就更安全

**Session的工作流程**
![image.png](https://article.biliimg.com/bfs/article/4689b85f31c64148817d34aa4c04f39720c2aa32.png)

- 在服务端的AServlet获取一个Session对象，把数据存入其中
- 在服务端的BServlet获取到相同的Session对象，从中取出数据
- 就可以实现一次会话中多次请求之间的数据共享了
- 现在最大的问题是如何保证AServlet和BServlet使用的是同一个Session对象(在原理分析会讲解)?


**Session的基本使用**
在JavaEE中提供了HttpSession接口，来实现一次会话的多次请求之间数据共享功能。
具体的使用步骤为:
- 获取Session对象,使用的**是request对象**
`HttpSession session = request.getSession();`
- Session对象提供的功能:
    - 存储数据到 session 域中
        ` void setAttribute(String name, Object o)`
    - 根据 key，获取值
         `Object getAttribute(String name)`
    - 根据 key，删除该键值对
         `void removeAttribute(String name)`
             介绍完Session相关的API后，接下来通过一个案例来完成对Session的使用，具体实现步骤为:

> 需求:在一个Servlet中往Session中存入数据，在另一个Servlet中获取Session中存入的数据
> 1.创建名为SessionDemo1的Servlet类
> 2.创建名为SessionDemo2的Servlet类
> 3.在SessionDemo1的方法中:获取Session对象、存储数据
> 4.在SessionDemo2的方法中:获取Session对象、获取数据
> 5.启动测试

SessionDemo1:获取Session对象、存储数据
```java
@WebServlet("/demo1")
public class SessionDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    	//存储到Session中
        //1. 获取Session对象
        HttpSession session = request.getSession();
        //2. 存储数据
        session.setAttribute("username","zs");
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

SessionDemo2:获取Session对象、获取数据
```java
@WebServlet("/demo2")
public class SessionDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取数据，从session中
        //1. 获取Session对象
        HttpSession session = request.getSession();
        //2. 获取数据
        Object username = session.getAttribute("username");
        System.out.println(username);
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

### Session的原理分析

那么Session的底层到底是如何实现一次会话两次请求之间的数据共享呢?

![image.png](https://article.biliimg.com/bfs/article/f2b87c57817b08469a2b354fd840626389bdcc4d.png)

1.通过分析我们发现`tomcat`服务器会为每个浏览器用户开辟一个`session容器`来存储不同用户的数据。那么tomcat中会有很多个session容器,各个session各器是如何区分的呢?
tomcat为了区分不同用户之间的session容器，会络每个session容器分配唯一的标识，该标识称为`JSESSIONID`
2.对于浏览器甲再次访问torcat服务器的时喉，准备付款，是如何找至tocat中自己的session客器的?
其实tomcat在对于用户第一发访问的时候在给每个用户创建session的时候会在底层创建一个**会话级别的cookie**,该 cookie用来存session的唯一标识`JSESSIONID`,
然后**将cookie存储到浏览器端**，当下次再访问的时候携带cookie,tomcat取出cookie中的`JSESSIONID`与tomcat中的session容器的`JSESSIONID`进行比较来查找自己的session容器。

<img src="https://article.biliimg.com/bfs/article/afdbd6d035213f47f6f46529bb3f988fa81f1e3d.png" alt="image.png" style="zoom:70%;" />


> [!summary]+ 总结
> 1. 用户第一次访问的时候，tomcat会创建对应的session容器，每个容器具有唯一的标识`JSESSIONID`,然后tomcat底层创建会话级别的cookie存储唯一标识`JSESSIONID`存储到浏览器端。
> 2. 用户再次访问，tomcat中取出session并从cookie中取出之前保存的唯一标识`JSESSIONID`进行比较查找自己的session容器
> 3. 介绍完Session的原理，我们只需要记住:Session是基于Cookie来实现的
> 4. session和浏览器没有任何关系，存在服务器端的。

### Session的使用细节

#### session持久化方案
**为什么持久化session**
tomcat在创建cookie的时候属于**会话级别的cookie**，**关闭浏览器，cookie消失**，下次打开浏览器不会携带之前的cookie即cookie中的`JSESSIONID`到tomcat服务器中了，那么这样会造成tomcat服务器中会有很多个不能使用的session容器(**session依然还在**，只是找不到了)。严重的话会造成服务器宕机。

持久化session来解决上述问题
主要问题是cookie是会话级别的，我们只需要将会话级别的cookie变为持久化级别的即可。
步骤：
1. 创建session
2. 获取session的JSESSIOID的值
3. 创建Cookie ,Cookie("JSESSIOID",值)
4. 使用cookie对象调用方法setMaxAge()进行cookie的持久化，存活时间建议30min
5. 将cookie响应给浏览器

```java
@WebServlet("/sessionPersis01Servlet")
public class SessionPersis01Servlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.创建session
        HttpSession session = request.getSession();
        //2.获取session的JSESSIOID的值
        String sessionId = session.getId();
        System.out.println(sessionId);
        //3.创建Cookie ,Cookie("JSESSIOID",值)
        Cookie cookie = new Cookie("JSESSIONID", sessionId);
        //4.使用cookie对象调用方法setMaxAge()进行cookie的持久化，存活时间建议30min
        cookie.setMaxAge(60*30);
        //5.将cookie响应给浏览器
        response.addCookie(cookie);

    }
}
```

#### Session钝化与活化

首先需要大家思考的问题是:
服务器重启后，Session中的数据是否还在?
<img src="https://article.biliimg.com/bfs/article/f34ea4b6239de097b52d24ca03a178a6d0e44c21.png" alt="image.png" style="zoom:67%;" />

(1)服务器端AServlet和BServlet共用的`session`对象应该是存储在服务器的内存中
(2)服务器重新启动后，内存中的数据应该是已经被释放，对象也应该都销毁了
所以session数据应该也已经不存在了。但是如果session不存在会引发什么问题呢?
举个例子说明下，
(1)用户把需要购买的商品添加到购物车，因为要实现同一个会话多次请求数据共享，所以假设把数据存入Session对象中
(2)用户正要付钱的时候接到一个电话，付钱的动作就搁浅了
(3)正在用户打电话的时候，购物网站因为某些原因需要重启
(4)重启后session数据被销毁，购物车中的商品信息也就会随之而消失
(5)用户想再次发起支付，就会出为问题
所以说对于session的数据，我们应该做到就算服务器重启了，也应该能把数据保存下来才对。
分析了这么多，那么Tomcat服务器在重启的时候，session数据到底会不会保存？
答案是肯定的。
**钝化**：就是正常关闭tomcat服务器，会将session容器中的数据长久保存到硬盘上。底层原理是[[java基础篇#序列化|序列化]]。
**活化**：就是启动tomcat服务器，将之前钝化的session容器读取到内存中。底层原理是反序列化。

**注意：**
**1.由于钝化和活化的原理是序列化和反序列，所以要求存储在session容器中的对象所属类必须实现序列化接口Serializable。**
**2.演示钝化和活化效果不能在idea中演示，我们需要将当前项目打成war包放到tomcat服务器中的webapps目录下进行演示。**

**钝化和活化的演示**

```java
import java.io.Serializable;
public class Product implements Serializable {
    //成员变量
    private String pname;
    private double price;
    public Product(String pname, double price) {
        this.pname = pname;
        this.price = price;
    }
    public String getPname() {
        return pname;
    }
    public void setPname(String pname) {
        this.pname = pname;
    }
    public double getPrice() {
        return price;
    }
    public void setPrice(double price) {
        this.price = price;
    }
    @Override
    public String toString() {
        return "Product{" +
                "pname='" + pname + '\'' +
                ", price=" + price +
                '}';
    }
}
```


```java
@WebServlet("/setSessionServlet")
public class SetSessionServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        //1.创建session
        HttpSession session = request.getSession();
        //2.获取session的id
        String sessionId = session.getId();
        //3.创建商品对象
        Product p = new Product("笔记本", 9999);
        //4.将商品对象存储到session中
        session.setAttribute("p",p);
        //5.响应数据
        response.getWriter().print("setSessionServlet.....当前JSESSIONID="+sessionId);
    }
}
```

```java
@WebServlet("/getSessionServlet")
public class GetSessionServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        //1.获取session
        HttpSession session = request.getSession();
        //2.获取session的id
        String sessionId = session.getId();
        //3.从session中取出商品
        Product p = (Product) session.getAttribute("p");
        //4.响应数据
        response.getWriter().print("getSessionServlet.....当前JSESSIONID="+sessionId+",p="+p.toString());
    }
}
```

#### Session销毁

session的销毁会有两种方式:
- 默认情况下，无操作，30分钟自动销毁
    - 对于这个失效时间，是可以通过配置进行修改的
        - 在项目的web.xml中配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

    <session-config>
        <session-timeout>10</session-timeout>
    </session-config>
</web-app>
```

如果没有配置，默认是30分钟，默认值是在Tomcat的web.xml配置文件中写死的
![image.png](https://article.biliimg.com/bfs/article/82597550ac32fc5758887da33be34aaaa7de56f1.png)

调用Session对象的`invalidate()`进行销毁
- 在SessionDemo2类中添加session销毁的方法

```java
@WebServlet("/demo2")
public class SessionDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取数据，从session中

        //1. 获取Session对象
        HttpSession session = request.getSession();
        System.out.println(session);

        // 销毁
        session.invalidate();
        //2. 获取数据
        Object username = session.getAttribute("username");
        System.out.println(username);
    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

- 启动访问测试，先访问demo1将数据存入到session，再次访问demo2从session中获取数据

![image.png](https://article.biliimg.com/bfs/article/f7d6dc5eb1199a7fc64b248e1ae3f654301010d7.png)

- 该销毁方法一般会在用户退出的时候，需要将session销毁掉。

### Cookie和Session区别

Session的区别以及应用场景：

|区别| Cookie        | Session            |
|:-:|:-:|:-:|
| 存储位置      | 客户端       | 服务端           |
| 安全性         | 不安全       | 安全             |
| 数据大小      | 最大3KB      | 无大小限制        |
| 存储时间      | 可长期存储   | 默认30分钟        |
| 服务器性能     | 不占用        | 占用资源           |

|应用场景| 存储方式          |
|:-:|:-:|
| 购物车         | Cookie              |
| 以登录用户的名称展示 | Session         |
| 记住我功能     | Cookie              |
| 验证码         | Session             |

总的来说，Cookie主要用于进行客户端状态的识别，比如保证用户在未登录的情况下进行身份认证、记录用户行为等；而Session则一般用于存储用户登录后的数据，例如用户信息、购物车信息等。在具体应用中也要根据实际需求选择使用Cookie或Session。需要注意的是，Cookie存在一定的安全隐患，所以在对安全性要求较高的场景下，建议使用Session来存储敏感数据。

## 用户注册验证码案例

### 需求分析

展示验证码：展示验证码图片，并可以点击切换

![image-20230604142537875](https://article.biliimg.com/bfs/article/34249d36dcaabb1aa7db076425de51e1dfc52daa.png)

![image.png](https://article.biliimg.com/bfs/article/d9e36c6a56cf7b36a4ceea3ae5bfa97501db316f.png)


### .实现步骤

> [!abstract]
> 1.创建登录页面login.html  
> 2.定义一个CheckCodeServlet用来生成验证码图片  
> 3.定义一个LoginServlet用来登录时校验验证码是否正确 

### 代码实现

#### 1.创建登录页面login.html

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录页面</title>
</head>
<body>
<h2>登录页面</h2>
<form action="/loginServlet" method="post">
    账号<input type="text" name="username"><br>
    密码<input type="password" name="password"><br>
    验证码<input type="text" name="code">
    <!--页面一加载就向checkCodeServlet发送请求-->
    <img src="/checkCodeServlet" alt="验证码" style="cursor: pointer" onclick="changeCheckCode(this);"><br>
    <input type="submit" value="登录">
</form>
<script type="text/javascript">
    //定义函数更改验证码图片
    //obj 表示img标签对象
    function changeCheckCode(obj) {//obj=this
        /*
            每次点击验证码图片重新向后台checkCodeServlet发送新的请求
            问题：当我们点击验证码图片，发现浏览器并没有向后台发送请求，因为浏览器认为当前发送的请求的路径是同一个，那么浏览器直接将当前浏览器的图片显示了
            解决问题：
            我们可以在请求的路径后面增加一个时间，告知浏览器这是一个新的请求，和原来的请求不一样
         */
        // obj.src = "/checkCodeServlet";//同一个请求，不会发送新的请求
        obj.src = "/checkCodeServlet?t="+new Date().getTime();//每次请求都不一样
    }
</script>
</body>
</html>
```

#### 定义一个CheckCodeServlet用来生成验证码图片

```java
/*
     说明：验证码是一张图片，而这个图片中的内容是使用代码生成的。
        分析和步骤：
        1）创建一个可以存放图片的缓冲区BufferedImage作为画布；
        2）通过画布获取到针对这个画布的画笔；
        3）修改画布的背景颜色为白色；
        4）设置画布的边框，画边框的时候需要注意下，如果这里写画布的宽width和高height ，就会超出画布就会看不见，所以width和height 分别-1；
        5）创建一个获取随机数的对象；
        6）给画布上写数据；
        7）给画布上画干扰线；
        8）需要把画布中的内容响应给浏览器；ImageIO.write(bi,"JPG",response.getOutputStream());
 */
@WebServlet("/checkCodeServlet")
public class CheckCodeServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //定义画布的宽和高
        int width = 120;
        int height = 30;
        //创建一个可以存放图片的缓冲区，作为画布
        //BufferedImage.TYPE_INT_RGB 表示生成图片的类型
        BufferedImage bi = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
        //通过画布获取到针对这个画布的画笔
        Graphics g = bi.getGraphics();
        //修改画布的背景颜色  每次使用画笔的时候都得给画笔指定颜色
        g.setColor(Color.WHITE);
        //填充画布
        g.fillRect(0, 0, width, height);
        //设置画布的边框
        //给画笔指定颜色
        g.setColor(Color.RED);
        //给画布画边框 如果这里写width height 就会超过画布，因为边框也会占一个像素，所以这里宽和高都需要-1
        g.drawRect(0, 0, width - 1, height - 1);
        //创建一个获取随机数的对象
        Random r = new Random();
        //给画布上画干扰线
        //循环控制画多条线
        for (int i = 1; i <= 3; i++) {
            //设置画笔的颜色
            g.setColor(new Color(r.nextInt(255), r.nextInt(255), r.nextInt(255)));
            //向画布上画干扰线
            //drawLine(x1, y1, x2, y2) 这里四个参数是因为两个点画成一条线
            g.drawLine(r.nextInt(width), r.nextInt(height), r.nextInt(width), r.nextInt(height));
        }
        //定义数据准备向画布中写数据
        String data = "abcdefghigklmnpqrstuvwxyzABCDEFGHIGKLMNOPQRSTUVWXYZ123456789";
        //创建字符串缓冲区
        StringBuilder sb = new StringBuilder();

        //循环控制画四个字符
        for (int i = 1; i <= 4; i++) {
            //设置画笔的颜色 Color.BLUE这里的颜色固定了，只能是蓝色，我们可以让颜色随机变化
//			g.setColor(Color.BLUE);
            g.setColor(new Color(r.nextInt(255), r.nextInt(255), r.nextInt(255)));
            //设置字体  Font.ITALIC表示斜体
            g.setFont(new Font("宋体", Font.ITALIC, 20));
            //给画布上写内容 20表示从x轴的位置开始书写 25表示y轴位置开始书写
//			g.drawString("哈哈哈哈", 20, 25);
            /*
             * data.charAt()表示根据函数的参数进行查找字符
             * data.length()表示字符串的长度
             * r.nextInt()表示生成随机数，但是随机数的范围在0~data字符串的长度
             */
            String str = data.charAt(r.nextInt(data.length())) + "";
            g.drawString(str, 20 * i, 25);
            //将验证码内容拼接到字符串缓冲区中
            sb.append(str);
        }
        //  验证码保存到session中
        request.getSession().setAttribute("checkcode", sb.toString());
        //将生成的验证码图片响应给浏览器
        ImageIO.write(bi, "JPG", response.getOutputStream());
    }
}
```

#### 定义一个LoginServlet用来登录时校验验证码是否正确
```java
@WebServlet("/loginServlet")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.处理post请求乱码
        request.setCharacterEncoding("utf-8");
        //2.获取用户名和密码以及输入框的验证码
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        //页面的输入框的验证码
        String input_code = request.getParameter("code");
        //3.从session中获取验证码图片
        // request.getSession().setAttribute("checkcode", sb.toString());
        String session_checkcode = (String) request.getSession().getAttribute("checkcode");
        //4.比较从session中获取的验证码和从页面获取的验证码是否相等：
        if(input_code!=null && input_code.equalsIgnoreCase(session_checkcode)){
            //相等：将用户名和密码传递到dao层向数据库查询
            System.out.println("验证码校验通过");
        }else{
            //不相等：响应给浏览器客户端提示验证码输入有误
            System.out.println("验证码校验没有通过");
        }
    }
}
```


## MVC模式和三层架构

### MVC模式
MVC 是一种分层开发的模式，其中：
- M：`Model`，业务模型，处理业务
- V：`View`，视图，界面展示
- C：`Controller`，控制器，处理请求，调用模型和视图
![image.png](https://article.biliimg.com/bfs/article/93e7d7e0e87cf7f1d5766ef824419cedb7eb3fe6.png)

控制器（serlvlet）用来**接收浏览器发送过来的请求**，控制器**调用模型（JavaBean）来获取数据**，比如从数据库查询数据；控制器获取到数据后再交由视图（JSP）进行数据展示。

**MVC 好处：**
- 职责单一，互不影响。每个角色做它自己的事，各司其职。
- 有利于分工协作。
- 有利于组件重用

### 三层架构

三层架构是将我们的项目分成了三个层面，分别是 `表现层`、`业务逻辑层`、`数据访问层`。
![image.png](https://article.biliimg.com/bfs/article/ae9d9ff7d3d80b0b1ca44e6674a903451acd37b7.png)

- 数据访问层：对数据库的CRUD基本操作
  
- 业务逻辑层：对业务逻辑进行封装，组合数据访问层层中基本功能，形成复杂的业务逻辑功能。例如 `注册业务功能` ，我们会先调用 `数据访问层` 的 `selectByName()` 方法判断该用户名是否存在，如果不存在再调用 `数据访问层` 的 `insert()` 方法进行数据的添加操作
- 表现层：接收请求，封装数据，调用业务逻辑层，响应数据

而整个流程是，浏览器发送请求，表现层的Servlet接收请求并调用业务逻辑层的方法进行业务逻辑处理，而业务逻辑层方法调用数据访问层方法进行数据的操作，依次返回到serlvet，然后servlet将数据交由 JSP 进行展示。

三层架构的每一层都有特有的包名称：
- 表现层： `com.itheima.controller` 或者 `com.itheima.web`
- 业务逻辑层：`com.itheima.service`
- 数据访问层：`com.itheima.dao` 或者 `com.itheima.mapper`
后期我们还会学习一些框架，不同的框架是对不同层进行封装的
![image.png](https://article.biliimg.com/bfs/article/56608f8c992c7472d5f3e32b4b8038be1cfa5a3b.png)

### MVC 和 三层架构

通过 MVC 和 三层架构 的学习，有些人肯定混淆了。那他们有什么区别和联系？
![image.png](https://article.biliimg.com/bfs/article/72488ea1a444d2af9375c7fda545df1fc5f809b6.png)

如上图上半部分是 MVC 模式，上图下半部分是三层架构。 `MVC 模式` 中的 C（控制器）和 V（视图）就是 `三层架构` 中的表现层，而 `MVC 模式` 中的 M（模型）就是 `三层架构` 中的 业务逻辑层 和 数据访问层。
可以将 `MVC 模式` 理解成是一个大的概念，而 `三层架构` 是对 `MVC 模式` 实现架构的思想。 那么我们以后按照要求将不同层的代码写在不同的包下，每一层里功能职责做到单一，将来如果将表现层的技术换掉，而业务逻辑层和数据访问层的代码不需要发生变化。

# Filter&Listener

## Filter

### Filter概述
**生活中的过滤器**
	净水器、空气净化器、地铁安检
**web中的过滤器**
	当用户访问服务器资源时，过滤器将请求拦截下来，完成一些通用的操作

> [!note]+ Filter的作用
> 1. 拦截客户端对web资源的请求 (重要!)
> 2. 拦截web资源对客户端的响应

**应用场景**
如：`登录验证`(==filter对登录后的网址进行拦截判断后是否重定向==)、`统一编码处理`、`敏感字符过滤`
![image.png](https://article.biliimg.com/bfs/article/480d3243aba7e1e391bf7698c8d33c75c0cf2b04.png)

### FilterAPI介绍
Filter表示过滤器接口，我们想使用该接口必须自定义类实现接口并实现该接口中的所有抽象方法。

|方法|说明|
|:-:|:-:|
|`void init(FilterConfig filterConfig)`| 过滤器对象创建的时候调用的方法                     |
|`void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)`| 执行过滤的方法，每次访问被拦截的资源都会执行该方法 |
|`void destory()`| 过滤器销毁的时候调用的方法                         |

✍️注意：
- `doFilter`第三个参数：`FilterChain` 表示过滤器链接口。
- 放行：使用`FilterChain` 对象调用`FilterChain` 中的方法：`chain.doFilter(request,response)`;即可以让浏览器访问服务器资源
- 不放行:那么不写上述代码，即不让浏览器访问服务器资源

### 代码实现
过滤器类：
```java
//1.自定义过滤器类实现过滤器接口Filter
public class MyFilter implements Filter{
    //2.在自定义类中实现过滤器接口Filter中的所有抽象方法
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }
    //3.在doFilter方法体中书写拦截资源的代码
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("执行过滤器了...");
        //不放行
        //放行  filterChain.doFilter(servletRequest,servletResponse);
    }
    @Override
    public void destroy() {
    }
}

```

说明：

1.如果放行，希望浏览器可以访问拦截的资源则执行该方法：
`filterChain.doFilter(servletRequest,servletResponse);`
2.如果不放行，不希望浏览器访问拦截的资源则不执行该方法

xml配置filter和修改idea的过滤器模板省略

### filter使用细节

#### 生命周期
**生命周期**：指的是一个对象从生（创建）到死（销毁）的一个过程
```java
// 初始化方法
public void init(FilterConfig config);
// 执行拦截方法
public void doFilter(ServletRequest request, ServletResponse response,FilterChain chain);
// 销毁方法
public void destroy();
```

```java
* 创建
		服务器启动项目加载，创建filter对象，执行init方法（只执行一次）
		
* 运行（过滤拦截）
		用户访问被拦截目标资源时，执行doFilter方法

* 销毁
		服务器关闭时，销毁filter对象，执行destroy方法（只执行一次）
		
* 补充：
	过滤器一定是优先于servlet创建的,后于Servlet销毁
```

```java
@WebFilter("/MyServlet2")
public class MyFilter2 implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("MyFilter2 init");
    }
    //当请求经过过滤器,此方法就会运行(每经过一次,就运行一次)
    @Override
    public void doFilter(ServletRequest request,
                         ServletResponse response,
                         FilterChain chain) throws IOException, ServletException {
        System.out.println("MyFilter2 doFilter");
        //请求放行 (相当于请求转发)
        chain.doFilter(request,response);
    }
    @Override
    public void destroy() {
        System.out.println("MyFilter2 destroy");
    }
}
```

#### 拦截路径

在开发时，我们可以指定过滤器的拦截路径来定义拦截目标资源的范围

```java
* 精准匹配
		用户访问指定目标资源（/demo01.html）时，过滤器进行拦截
		
* 目录匹配
		用户访问指定目录下（/user/*）所有资源时，过滤器进行拦截

* 后缀匹配
		用户访问指定后缀名（*.html）的资源时，过滤器进行拦截,不能加/

* 匹配所有
		用户访问该网站所有资源（/*）时，过滤器进行拦截 掌握
```

```java
//@WebFilter("/xx")
//@WebFilter({"/xx","/yy"})
@WebFilter("/xx") // 精准匹配
//@WebFilter("/user/*") // 目录匹配
//@WebFilter("*.do") // 后缀匹配
//@WebFilter("/*") // 匹配所有
public class MyFilter3 implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
    }
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("doFilter");
    }
    @Override
    public void destroy() {
    }
}
```

#### 过滤器链

在一次请求中,若我们请求匹配到了多个filter,通过请求就相当于把这些filter串起来了，形成了过滤器链。
问题：如果多个过滤器都对相同路径进行匹配，执行顺序该是什么？
Filter默认是按照字母顺序执行的，如果过滤器名字第一个字母相同，再看过滤器名字的第二个字母，以此类推。从而形成一个执行链条。
**前提：多个过滤器过滤同一个资源，并且多个过滤器是在同一包下。**

```java
@WebFilter("/apple")
public class AFilter implements Filter {
    public void destroy() {
    }
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) resp;
        System.out.println("AFilter....放行前");
        chain.doFilter(request, response);
        System.out.println("AFilter....放行后");
    }
    public void init(FilterConfig config) throws ServletException {
    }

}
```

```java
@WebFilter("/apple")
public class BFilter implements Filter {
    public void destroy() {
    }

    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) resp;
        System.out.println("BFilter....放行前");
        chain.doFilter(request, response);
        System.out.println("BFilter....放行后");
    }
    public void init(FilterConfig config) throws ServletException {
    }
}
```

```java
@WebServlet("/apple")
public class AppleServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("AppleServlet....");
    }
}
```

```java
AFilter. ...放行前
BFilter. . ..放行前
Appleservlet. . ..
BFilter. ...放行后
AFilter...放行后
```

![image.png](https://article.biliimg.com/bfs/article/0f219833290da715322e18c3aaea5eff7c5ba3d3.png)

### Filter案例
#### Filter统一解决post请求乱码问题
我们需要自己处理post请求的乱码问题，因为我们的一个项目中可能有很多的请求。我们可以在每一个Servlet中都对post的乱码进行处理。但这样太麻烦，我们可以自定义一个Filter类，拦截所有的请求，然后对这些请求进行post乱码处理即可。

**过滤器EncodingFilter中的代码：**

```java
/*
    /*表示拦截当前项目中所有的资源路径
    解决全站乱码问题
 */
@WebFilter("/*")
public class EncodingFilter implements Filter {
    @Override
    public void destroy() {
    }

    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) resp;
        //解决请求乱码
        request.setCharacterEncoding("utf-8");
        //解决响应乱码
        response.setContentType("text/html;charset=utf-8");
        //your  code.... 
        chain.doFilter(request, response);
    }

    @Override
    public void init(FilterConfig config) throws ServletException {
    }

}
```

#### 登录权限校验
**需求**
1. 登录之后：能够访问hack.html，然后下载黑客视频资源，成为NB黑客；
2. 未登录：跳转到登录页面，并提示"请先登录"；
3. 我们对hack.html页面进行过滤，

详细描述：
1. 页面：hack.html 访问这个页面要求必须先登录，不登录不能访问，使用过滤器书写代码让其跳转到登录页面login.html.
2. 登录LoginServlet中获取用户名和密码，存放到User对象中，然后在session中。最后重定向到hack.html 
3. 使用一个**过滤器对hack.html 进行拦截**，先从session中获取用户信息，如果没有登录，获取的是null。跳转到登录页面。如果不为null，说明登录了，可以访问hack.html

**LoginServlet代码**：
```java
@WebServlet("/loginServlet")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取用户名和密码
        String name = request.getParameter("name");
        String password = request.getParameter("password");
        User user = new User(name,password);
        System.out.println("user = " + user);
        //将用户信息保存session中
     request.getSession().setAttribute("user",user);
        response.sendRedirect("/hack.html");
    }
}
```
**LoginFilter代码**：
```java
@WebFilter("/hack.html")
public class LoginFilter implements Filter {
    public void destroy() {
    }

    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) resp;
        //获取用户信息
        User user = (User) request.getSession().getAttribute("user");
        if (user == null) {
            //没有登陆
            response.sendRedirect("/login.html");
            return;
        } else {
            //放行。。
            chain.doFilter(request, response);
        }
    }
    public void init(FilterConfig config) throws ServletException {
    }
}
```

## ServletContext


> [!summary]+ 补充：
> 1. ServletContext 表示上下文对象，属于接口，代表整个web项目，可以使用方法获取当前web项目的所有文件的MIME类型
> 2. 之前学习能够共享数据的对象：
> 	1）`request`: 只能在一次请求一次响应中进行数据的共享
> 	2）`session`：只能在一次会话过程中，可以有多次请求和响应，     
> 	3）`ServletContext`：只要项目存在就可以共享数据，多次会话，多次请求和响应都可以共享数据：操作整个项目的配置文件  
> 	范围大小：` ServletContext > session > request`


## Listener
### Listener概述

**监听**
1. 设置监听器的人
2. 监听器 
2. 监听器目标: 被监听的对象
3. 监听器工作: 被监听的**对象执行某种行为,监听器就开始工作**

```python
# web里面
1. 雇佣人 : web程序开发者
2. 监听器例子A
	1). 监听器A: ServletContextListener ****
	2). 目标 : ServletContext对象
	3). 执行 : 监听ServletContext对象创建和销毁
3. 监听器例子B
	1). 监听器A: HttpSessionListener
	2). 目标 : HttpSession对象
	3). 执行 : 监听HttpSession对象创建和销毁
4. 监听器例子C
	1). 监听器A: HttpRequestListener
	2). 目标 : HttpRequest对象
	3). 执行 : 监听HttpRequest对象创建和销毁	
5.  监听器例子D
	1). 监听器A: ServletContextAttributeListener ****
	2). 目标 : ServletContext对象
	3). 执行 : 监听ServletContext对象增删改数据 (add,remove) 当我们向ServletContext对象中添加数据(setAttribute())和删除数据(removeAttribute())就会被ServletContextAttributeListener监听器监听
	
 HttpSessionAttributeListener HttpRequestAttributeListener
```

**javaweb中的监听器**
在我们的java程序中，有时也需要监视某些事情，一旦被监视的对象发生相应的变化，我们应该采取相应的操作。
监听web三大域对象：HttpServletRequest、HttpSession、ServletContext （创建和销毁）
**场景**
历史访问次数、统计在线人数、系统启动时初始化配置信息

**监听器的接口分类**

|**事件源**|**监听器接口**|**时机**|
|:-:|:-:|:-:|
|**ServletContext**|**ServletContextListener**|上下文域创建和销毁|
|**ServletContext**|**ServletContextAttributeListener**|上下文域属性增删改的操作|
|**HttpSession**|HttpSessionListener|会话域创建和销毁|
|**HttpSession**|HttpSessionAttributeListener|会话域属性增删改的操作|
|**HttpServletRequest**|ServletRequestListener|请求域创建和销毁|
|**HttpServletRequest**|ServletRequestAttributeListener|请求域属性增删改的操作|

### 快速入门

监听器在web开发中使用的比较少,见的机会就更少了,今天我们使用**ServletContextListenner**来带领大家学习下监听器,因为这个监听器是监听器中使用率最高的一个,且监听器的使用方式都差不多。

我们使用这个监听器可以在项目启动和销毁的时候做一些事情,例如,在项目启动的时候加载配置文件。

1. 创建一个普通类，实现ServletContextListenner
2. 重写抽象方法
	监听ServletContext创建
	监听ServletContext销毁
3. 配置
	web.xml
	注解

```java
@WebListener
public class MyListener implements ServletContextListener {
    //tomcat一启动,此方法就会运行
        //运用场景: spring底层封装了一个ServletContextListener加载配置文件
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("contextInitialized");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("contextDestroyed");
    }
}
```

# 综合项目

项目需求

```java
1.我们今天完成的项目整体分为三个模块：
	用户模块  功能：增删改查
	角色模块  功能：增删改查
	权限模块  功能：增删改查
	
2.分析：
	用户和角色的关系：
		一个用户具有多个角色。举例：张三用户可以是QQ黄钻 绿钻 等
		一个角色可以对应多个用户。举例：QQ黄钻可以是张三 李四 等
		
#用户和角色属于多对多的关系，根据表设计原则，多对多关系创建中间表，在中间表中起码要有另外两张主表用户和角色的主键作为外键进行关联

	角色和权限的关系：
		一个角色可以有多种权限。举例：QQ黄钻：可以查看被挡访客，可以装扮空间等
		一种权限可以对应多个角色。举例：可以查看被挡访客的权限可以是QQ黄钻，也可以是绿钻
	
#角色和权限属于多对多的关系，根据表设计原则，多对多关系创建中间表，在中间表中起码要有另外两张主表权限和角色的主键作为外键进行关联
	
#注意：用户 角色 权限具有经典的五张表
```

## 代码技巧1
#代码精华
针对3个模块，分别创建一个servlet 即：
	UserServlet 增删改查
	RoleServlet 增删改查
	PermissionServlet 增删改查
按照以前做法每个需求都会存在一个servlet，那么每个模块会有大概至少四个servlet，三个模块会有至少12个servlet，那么在实际开发中模块会有很多，这样造成会有很多个servlet，如果具有相同的代码，那么每个servlet中代码会产生冗余的代码。
所以我们接下来会对于每个模块创建一个servlet，然后在该servlet完成该模块的增删改查操作。这样就可以减少servlet的创建。
✍️ 这里我们采用在`servlet`中获取请求的url中的方法名 判断当前url的路径请求的是哪个方法,


> [!info]+ 步骤
> 1.创建servlet     
> 2.在servlet中获取请求的url中的方法名       
> 3.判断当前url的路径请求的是哪个方法      
> 4.在servlet中创建增删改查的四个方法     
> 5.在不同的方法体中完成代码      

```java
/user/find
/user/update
/user/add
/user/delete
说明：
	1.
		/user/find： user表示请求的模块名，即访问的哪个模块，用户模块是user,角色模块就是role,权限模块是permission
		find：在servlet中首先根据相应的api获取到find,find表示在servlet中要执行的方法名
```

```java
 //1.获取请求的方法名
        String url = request.getRequestURI();//  "/user/find"
        //获取最后一个/出现的索引
        int lastIndex = url.lastIndexOf('/');
        //从指定索引位置截取到末尾
        String methodName = url.substring(lastIndex + 1);

        //3.判断当前url的路径请求的是哪个方法
        if ("find".equals(methodName)) 
        {....}
```


## 代码技巧2

1不使用过多if判断 2）减少代码重复性，不仅仅使用在用户模块。所有模块都可以使用
这里我们用[[java基础篇#this 和 super|this关键字]]和[[java基础篇#反射|java反射]]

我们这里可以在当前用户模块无论有多少个需求都只需要书写一套模板代码，使用所有的当前用户模块的需求。
我们可以使用**反射思想，根据获取的页面中的方法名来执行具体的方法**，不用再判断了。

```java
  //1.获取请求的方法名
        String url = request.getRequestURI();//  "/user/find"
        //获取最后一个/出现的索引
        int lastIndex = url.lastIndexOf('/');
        //从指定索引位置截取到末尾
        String methodName = url.substring(lastIndex + 1);

        //3.判断当前url的路径请求的是哪个方法

        ////////////////////////////使用反射执行方法，简化if语句////////////////////////////////////////
        //1.获取要执行的方法所属类的Class对象
        //this表示当前类的对象
        Class clazz = this.getClass();//this：谁调用就表示谁。子类对象调用就代表子类对象
        /*
            2.使用Class对象调用Class类中的方法获取要执行的方法：
             Method getMethod(String name, Class<?>... parameterTypes)
                参数：
                    name：方法名----根据url的key获取value即方法名
                    parameterTypes：要执行方法的参数类型 request  response
         */
    try {
            Method m = clazz.getMethod(methodName, HttpServletRequest.class, HttpServletResponse.class);

            /*
                3.使用Method对象调用Method类中的invoke方法：
                 Object invoke(Object obj, Object... args)  对带有指定参数的指定对象调用由此 Method 对象表示的底层方法。
                        参数：
                            obj:要执行方法的对象，例如findAllUsers,这里传递findAllUsers所属类的对象
                            args：要执行方法的实参。request  response
             */
            m.invoke(this,request,response);
        } catch (Exception e) {
            e.printStackTrace();
        }
```

## 代码技巧3

这3个模块i有很多重复操作，我们将这部分代码抽离出来做成一个单独BaseServlet，这里我们不采用每个调用`BaseServlet`类，而是采用[[java基础篇#继承|继承]]，即3个	`UserServlet 、RoleServlet 、PermissionServlet` 都继承`BaseServlet`类,只需要重写自己独特的那部分，

**UserServlet:**

```java
//用户模块
@WebServlet("/user/*")
public class UserServlet extends BaseServlet {
    //4.在servlet中创建增删改查的四个方法
    //5.在不同的方法体中完成代码
    public void deleteUserById(HttpServletRequest request, HttpServletResponse response) {
        System.out.println("根据id删除用户");
    }

    public void addUser(HttpServletRequest request, HttpServletResponse response) {
        System.out.println("添加用户");

    }

    public void updateUserById(HttpServletRequest request, HttpServletResponse response) {
        System.out.println("根据id更新用户");
    }

    public void findAllUsers(HttpServletRequest request, HttpServletResponse response) {
        System.out.println("查询所有用户");
    }
}
```

## 代码技巧4

我们之前在servlet中创建业务层对象：
```java
UserServiceImpl service = new UserServiceImpl();
UserServiceImpl属于一个类
```
弊端：如果我想针对UserServiceImpl类中的内容进行扩展。我们可以在定义一个类似UserServiceImpl这样的类UserServiceImpl2，然后我们在servlet中创建UserServiceImpl2类的对象。如果在继续扩展，定义UserServiceImpl3类，在servlet中又修改创建业务层对象代码。
解决问题思路：就是每次对于UserServiceImpl扩展类创建对象，我们不要去修改servlet源码，我们可以修改配置文件方式，源码达到零修改。

这次我们将使用反射和[[java基础篇#ResourceBundle工具类|读取配置文件]]的代码抽取到工具类中替换new方式创建对象（[[java基础篇#单例设计模式|工厂类]]）

**用来生产对象的工具类**

```java
public class BeansFactory {

    //1.定义静态方法生成单例对象
    /*
        1.创建一个Map集合对象,new HashMap<String,Object>
                key : 配置文件等号左边的内容userService roleService
                value:保存对象
        2.创建静态方法生成单例对象
        3.在静态方法体中根据集合对象调用集合中的get方法根据key获取value  value = map.get(key)
        4.判断获取的value是否等于null
        5.如果value等于null，说明集合中还没有存储对象，创建对象
        6.将创建的对象和key存储到map集合中 map:userService   obj
        7.返回对象
     */
    /*
         1.创建一个Map集合对象,new HashMap<String,Object>
                key : 配置文件等号左边的内容userService roleService
                value:保存对象
     */
    private static HashMap<String, Object> map = new HashMap<String, Object>();

    //2.创建静态方法生成单例对象
    /*
        以下方法具有多线程安全问题：
            t1:第一次执行obj是null，执行if语句，还没执行完，cpu切换到t2
            t2:第一次执行obj依然是null，执行if语句，正常创建一个对象，放到map集合，返回t2创建的对象，此时cpu切换到t1线程
               t1线程又创建一个对象，并返回，那么此时会有多个对象，线程不安全了
     */
    public static synchronized Object getSingleInstance(String beanName) throws Exception {
        //3.在静态方法体中根据集合对象调用集合中的get方法根据key获取value  value = map.get(key)
        Object obj = map.get(beanName);
        //4.判断获取的value是否等于null
        if (obj == null) {
            //  5.如果value等于null，说明集合中还没有存储对象，创建对象
            // 5.1.读取保存实现类的配置文件
            ResourceBundle bundle = ResourceBundle.getBundle("beans");
            //userServiceStr="com.itheima.case2.service.impl.UserServiceImpl"
            String userServiceStr = bundle.getString(beanName);

            // 5.2.使用反射根据上述读取的实现类全路径创建对象
            Class clazz = Class.forName(userServiceStr);
            // 5.3.使用clazz调用实现类UserServiceImpl的无参构造方法创建对象
            obj = clazz.newInstance();
            // 6.将创建的对象和key存储到map集合中 map:userService   obj
            map.put(beanName, obj);
        }
        //7.返回对象
        return obj;
    }
}
```
后面代码执行这行就行
`UserService userserviceImp = BeansFactory.getSingleInstance("UserService");`

```properties
UserService=case2.service.impl.UserserviceImp
RoleService=case2.service.impl.RoleserviceImp
```

## Spring的ioc

Spring框架中的IOC（Inversion of Control，控制反转）是一种编程思想，它将程序的**控制权从应用本身转移到了框架中间来控制**，实现了应用与框架的分离。
简单来说，IOC就是把对象的依赖关系由程序员手动创建和管理，变成交由**框架容器**自动帮助我们处理。Spring的IOC容器主要是通过BeanFactory（Bean工厂）实现的，它可以将Spring配置文件中配置的Bean实例化、配置、组装并管理它们之间的关系，从而大大减轻了手动编写代码的难度。
在Spring中，我们只需要将对象交给容器管理，容器根据配置自动创建对象，并完成它们的注入，大大简化了应用程序的开发，并且将各个对象之间的关系交由容器来维护，使得应用程序更加灵活和易于维护。
以前我们要获取对象,我们自己new.主动获取。现在有了工厂模式,我们需要获取对象,是工厂创建,我们被动接受工厂创建的对象.这就是控制反转.说白了ioc就是采用工厂模式创建对象达到[[java基础篇#解耦合|解耦合]].    


# SPI机制
#servlet的实现方式 
## SPI引入
```java
# 1. 标准/规范
1. 工程 spi_interface
2. 只有一个接口car

# 2. 具体的实现
1. 工程 honda_car 和 tesla_car
2. 工程依赖了spi_interface
		pom.xml
3. 有一个实现类,实现了标准
	  HondaCar implements Car
	  TeslaCar implements Car
4. 还有一个配置文件
	1). 在类路径classpath下
		resources/META-INF/services
	2). 文件名: 接口的全限定名
    	com.itheima.Car
    3). 文件内容: 实现类的全限定名
    	com.itheima.impl.HondaCar
# 3. 调用
1. 工程 spi_test
2. 工程依赖了 honda_car 和 tesla_car
3. 测试类 SpiTest
```


![image.png](https://article.biliimg.com/bfs/article/2d75ea2b691aff091b08a4b9520f5827ee260326.png)

由于其他工程需要使用坐标方式导入该工程，所以需要将该工程进行打包
![image.png](https://article.biliimg.com/bfs/article/aa0e0f6bbdd98a2a2fa3788ebc99847809912af0.png)

```java
 # ServiceLoader<Car> cars = ServiceLoader.load(Car.class);加载接口实现
            1. 加载当前工程依赖的所有工程 classpath:META-INF/services目录下
                跟当前接口参数同名的文件
                   (classpath:META-INF.services/com.itheima.Car文件)
            2. 当前案例依赖了两个工程,那么这两个工程的配置文件都会被读取到
            	honda_car===META-INF.services/com.itheima.Car文件中的com.itheima.impl.HondaCar
            	tesla_car===META-INF.services/com.itheima.Car文件中的com.itheima.impl.TeslaCar
            	注意：配置文件名必须是实现的接口全路径，配置文件中书写实现类的全路径
            3. 通过反射创建接口文件中配置的实例
               Class clazz= Class.forName("com.itheima.impl.TeslaCar");
               Car car =  clazz.newInstance();
```

```java
public class SpiTest {
    /**
     * @Description 测试SPI
     * 工程install后再运行
     */
    @Test
    public void test1(){
        /*
             ServiceLoader<Car> cars = ServiceLoader.load(Car.class);加载接口实现
            1. 加载当前工程依赖的所有工程 classpath:META-INF/services目录下
                跟当前接口参数同名的文件
                   (classpath:META-INF.services/com.itheima.Car文件)
            2. 当前案例依赖了两个工程,那么这两个工程的配置文件都会被读取到
            	honda_car===META-INF.services/com.itheima.Car文件中的com.itheima.impl.HondaCar
            	tesla_car===META-INF.services/com.itheima.Car文件中的com.itheima.impl.TeslaCar
            	注意：配置文件名必须是实现的接口全路径，配置文件中书写实现类的全路径
            3. 通过反射创建接口文件中配置的实例
               Class clazz= Class.forName("com.itheima.impl.TeslaCar");
               Car car =  clazz.newInstance();
         */
         //Car.class 是接口Car的Class对象
        //使用ServiceLoader.load接收接口Class对象。那么该方法底层就会获取Car
        //接口所有实现类的对象放到ServiceLoader容器中
        ServiceLoader<Car> cars = ServiceLoader.load(Car.class);
        for (Car car : cars) {
            System.out.println(car.getColor());
            System.out.println(car.getStartType());
        }
    }
}
```

## ServiceLoader类介绍

![image.png](https://article.biliimg.com/bfs/article/39db4257f92ce86c36f53e7e832d7aa13c04ea79.png)

说明：

1.ServiceLoader功能和ClassLoader功能类似，能装载类文件，但是使用时是有区别的
2.ServiceLoader装载的是一系列有某种共同特征的实现类，即这些类实现接口或者抽象类。而ClassLoader是可以加载任何类的类加载器；
3.ServiceLoader加载时需要特殊的配置：
​	1)在类路径：`classpath:META-INF/services/`接口全路径文件
​	2）在文件中配置实现类全路径com.itheima.impl.HondaCar


## SPI介绍

SPI全称Service Provider Interface，是Java提供的一套用来被第三方实现或者扩展的API，它可以用来启用框架扩展和替换组件。

Java的SPI机制就是将一些**类信息写在约定的文件中**，然后由特定的类加载器ServiceLoader加载解析文件获取资源。

Java SPI 基于 “**接口编程＋策略模式＋配置文件**(约定)”组合实现的动态加载机制。

以下是SPI的一些运用场景:

|场景 |说明 |
|:-:|:-:|
|数据库驱动|数据库驱动加载接口实现类的加载 JDBC加载不同类型数据库的驱动|
|日志门面SLF4J接口实现类加载|SLF4J加载不同提供商的日志实现类|
|Spring|Spring中大量使用了SPI,比如：对servlet3.0规范对ServletContainerInitializer的实现、自动类型转换Type Conversion SPI(Converter SPI、Formatter SPI)等|
|Dubbo|Dubbo中也大量使用SPI的方式实现框架的扩展, 不过它对Java提供的原生SPI做了封装，允许用户扩展实现Filter接口|
|SpringBoot|SpringBoot基于SPI思想实现自动装配|

## Servlet实现方式三_ServletContainerInitializer

之前学习实现servlet有两种：1）注解 2)web.xml

其实Servlet还有一种实现方式就是spi方式。后面学习的框架底层就是采用这种方式。

`ServletContainerInitializer` 是 Servlet 3.0 新增的一个接口，主要用于在容器启动阶段通过编程风格注册`Filter`, `Servlet`以及`Listener`，以取代通过`web.xml`配置注册。这样就利于开发内聚的web应用框架.

将 **素材\ServletContainerInitializer代码**中的工程导入idea


> [!summary]+ 运行原理:
> 1. ServletContainerInitializer接口的实现类通过java SPI声明自己是ServletContainerInitializer 的提供者.
> 2. web容器启动阶段依据java spi获取到所有ServletContainerInitializer的实现类，然后执行其onStartup方法.
> 3. 在onStartup中通过编码方式将组件servlet加载到ServletContext




```java
//@WebServlet("/xx")
public class MyServlet extends HttpServlet {

    @Override
    public void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().write("hello myServlet");
    }

    @Override
    public void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }
}
```


```java
public class MyServletContainerInitializer implements ServletContainerInitializer {
    @Override
    public void onStartup(Set<Class<?>> c, ServletContext ctx) throws ServletException {
        System.out.println("start..........................");
        MyServlet myServlet = new MyServlet();
        ctx.addServlet("myServlet", myServlet).addMapping("/my");
    }
}
```


# HttpClient

## 介绍

`HttpClient`是`Apache Jakarta Common` 下的子项目，可以用来提供高效的、最新的、功能丰富的支持 HTTP 协议的**客户端编程工具包**(<font color=#ff0000>可以在java程序中通过HttpClient来构造http请求并发送http请求</font>)，并且它支持 HTTP 协议最新的版本和建议。

<img src="https://article.biliimg.com/bfs/article/7acee6260075141baae39cadda3edfb738716159.png" style="zoom: 70%;" />

**HttpClient作用：**
- 发送HTTP请求
- 接收响应数据

**HttpClient的maven坐标：**
[[java依赖#httpclient|HttpClient依赖]]

**HttpClient的核心API：**

- `HttpClient`：Http客户端对象类型，使用该类型对象可发起Http请求。
- `HttpClients`：可认为是构建器，可创建HttpClient对象。
- `CloseableHttpClient`：实现类，实现了HttpClient接口。
- `HttpGet`：Get方式请求类型。
- `HttpPost`：Post方式请求类型

**HttpClient发送请求步骤：**

- 创建HttpClient对象
- 创建Http请求对象
- 调用HttpClient的execute方法发送请求

## 入门案例

对HttpClient编程工具包有了一定了解后，那么，我们使用HttpClient在Java程序当中来构造Http的请求，并且把请求发送出去，接下来，就通过入门案例分别发送**GET请求**和**POST请求**，具体来学习一下它的使用方法。


```java
    @Test
    void testGet() throws IOException {
        // 创建httpclient对象
        CloseableHttpClient httpClient = HttpClients.createDefault();
        // 创建请求对象
        HttpGet httpGet = new HttpGet("http://localhost:8080/admin/shop/status");

        // 添加请求头设置token
        httpGet.setHeader("token","eyJhbGciOiJIUzI1NiJ9.eyJlbXBJZCI6MSwiZXhwIjoxNjk1MDM2OTY4fQ.P5D7qUlizpmRMZj2FNxq1bBsoo3aUXVkwgKtD3qtHCA");

        // 发送请求
        CloseableHttpResponse httpResponse = httpClient.execute(httpGet);

        // 获取服务端返回的状态码
        int statusCode = httpResponse.getStatusLine().getStatusCode();

        System.out.println("服务端状态码为" + statusCode);

        // 获取服务端返回的数据
        HttpEntity entity = httpResponse.getEntity();
        String data = EntityUtils.toString(entity);
        System.out.println("服务端返回的数据为" + data);

        // 关闭查询
        httpResponse.close();
        httpClient.close();
    }
```

POST方式请求

在HttpClientTest中添加POST方式请求方法，相比GET请求来说，POST请求若携带参数需要封装请求体对象，并将该对象设置在请求对象中。

**实现步骤：**

1. 创建HttpClient对象
2. 创建请求对象
3. 发送请求，接收响应结果
4. 解析响应结果
5. 关闭资源

```java
    @Test
    void testPost() throws IOException, JSONException {
        // 创建httpclient对象
        CloseableHttpClient httpClient = HttpClients.createDefault();
        // 创建请求对象
        HttpPost httpPost = new HttpPost("http://localhost:8080/admin/employee/login");

        // 添加请求头设置token
        httpPost.setHeader("token","eyJhbGciOiJIUzI1NiJ9.eyJlbXBJZCI6MSwiZXhwIjoxNjk1MDM2OTY4fQ.P5D7qUlizpmRMZj2FNxq1bBsoo3aUXVkwgKtD3qtHCA");

        // TODO: post请求需要传参数
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("username","admin");
        jsonObject.put("password","123456");
        StringEntity stringEntity = new StringEntity(jsonObject.toString());
        // 指定请求的编码方式
        stringEntity.setContentEncoding("utf-8");
        stringEntity.setContentType("application/json");
        httpPost.setEntity(stringEntity);
        // 发送请求
        CloseableHttpResponse response = httpClient.execute(httpPost);
        // 解析返回结束
        int statusCode = response.getStatusLine().getStatusCode();
        HttpEntity entity = response.getEntity();
        System.out.println("服务端状态码为" + statusCode);
        String data = EntityUtils.toString(entity);
        System.out.println("服务端返回的数据为 = " + data);
        // 关闭查询
        response.close();
        httpClient.close();
        System.out.println(jsonObject.toString());
    }
```


# WebSocket

## 介绍

WebSocket 是基于TCP的一种新的**网络协议**。它实现了浏览器与服务器全双工通信——浏览器和服务器只需要完成一次握手，两者之间就可以创建**持久性**的连接， 并进行**双向**数据传输。

**HTTP协议和WebSocket协议对比：**

- HTTP是**短连接**
- WebSocket是**长连接**
- HTTP通信是**单向**的，基于请求响应模式
- WebSocket支持**双向**通信
- HTTP和WebSocket底层都是TCP连接

<img src="https://article.biliimg.com/bfs/article/952c23d20dd92f57b955840fbe9b63e538716159.png" style="zoom:80%;" />   
<img src="https://article.biliimg.com/bfs/article/d4bff18e5e18feb8a7874baa7c923edd38716159.png" style="zoom:、50%;" />

**思考：** 既然WebSocket支持双向通信，功能看似比HTTP强大，那么我们是不是可以基于WebSocket开发所有的业务功能？

**WebSocket缺点：**
- 服务器长期维护长连接需要一定的成本
- 各个浏览器支持程度不一
- WebSocket是长连接，受网络限制比较大，需要处理好重连

**结论：** WebSocket并不能完全取代HTTP，它只适合在特定的场景下使用

**WebSocket应用场景：**
1. 视频弹幕
2. 网页聊天
3. 体育实况更新
4. 股票基金报价实时更新


**实现步骤：**

1. 直接使用websocket.html页面作为WebSocket客户端
2. 导入WebSocket的maven坐标
3. 导入WebSocket服务端组件WebSocketServer，用于和客户端通信
4. 导入配置类WebSocketConfiguration，注册WebSocket的服务端组件
5. 导入定时任务类WebSocketTask，定时向客户端推送数据

定义WebSocket服务端组件

```java
/**
 * WebSocket服务
 */
@Component
@ServerEndpoint("/ws/{sid}") // 指定了WebSocket的URL路径和占位符参数
public class WebSocketServer {

    //存放会话对象
    private static Map<String, Session> sessionMap = new HashMap(); // 这里面的key就是不同客户端的验证

    /**
     * 连接建立成功调用的方法
     */
    @OnOpen // 用于标记一个方法，在WebSocket连接建立时触发该方法。通常在该方法中进行初始化操作，例如获取用户身份、建立数据通道等。
    public void onOpen(Session session, @PathParam("sid") String sid) {
        System.out.println("客户端：" + sid + "建立连接");
        sessionMap.put(sid, session);
    }

    /**
     * 收到客户端消息后调用的方法
     *
     * @param message 客户端发送过来的消息
     */
    @OnMessage // 用于标记一个方法，在接收到WebSocket消息时触发该方法。WebSocket支持双向通信，
    public void onMessage(String message, @PathParam("sid") String sid) {
        System.out.println("收到来自客户端：" + sid + "的信息:" + message);
    }

    /**
     * 连接关闭调用的方法
     *
     * @param sid
     */
    @OnClose
    public void onClose(@PathParam("sid") String sid) {
        System.out.println("连接断开:" + sid);
        sessionMap.remove(sid);
    }

    /**
     * 群发
     *
     * @param message
     */
    public void sendToAllClient(String message) {
        Collection<Session> sessions = sessionMap.values(); // 这里是群发，所以要遍历
        for (Session session : sessions) {
            try {
                //服务器向客户端发送消息
                session.getBasicRemote().sendText(message);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

}
```

定义配置类，注册WebSocket的服务端组件

```java
/**
 * WebSocket配置类，用于注册WebSocket的Bean
 */
@Configuration
public class WebSocketConfiguration {

    @Bean
    public ServerEndpointExporter serverEndpointExporter() {
        return new ServerEndpointExporter();
    }

}
```

定义定时任务类，定时向客户端推送数据(从资料中直接导入即可)
```java
@Component
public class WebSocketTask {
    @Autowired
    private WebSocketServer webSocketServer;

    /**
     * 通过WebSocket每隔5秒向客户端发送消息
     */
    @Scheduled(cron = "0/5 * * * * ?")
    public void sendMessageToClient() {
        webSocketServer.sendToAllClient("这是来自服务端的消息：" + DateTimeFormatter.ofPattern("HH:mm:ss").format(LocalDateTime.now()));
    }
}
```


# Apache POI

## 介绍

Apache POI 是一个处理`Miscrosoft Office`各种文件格式的开源项目。简单来说就是，我们可以使用POI在Java程序中对Miscrosoft Office各种文件进行读写操作。
一般情况下，POI都是用于操作Excel文件。

**Apache POI 的应用场景：**

- 银行网银系统导出交易明细
<img src="https://article.biliimg.com/bfs/article/f02fe6de9f66ffe03c4d048b1f4ceb3738716159.png"  style="zoom:50%;" />
- 各种业务系统导出Excel报表
<img src="https://article.biliimg.com/bfs/article/bc9a2583b1653f5d69ea78502939d9a138716159.png" alt="image-20230131110839959" style="zoom:50%;" />
- 批量导入业务数据

<img src="https://article.biliimg.com/bfs/article/a9148b70f784773623fca17c8e84ec6c38716159.png" alt="image-20230131110856903" style="zoom:50%;" />

Apache POI既可以将数据写入Excel文件，也可以读取Excel文件中的数据，接下来分别进行实现。
**Apache POI的maven坐标：**(项目中已导入)[[java依赖#poi-ooxml|导入坐标]]

## 入门案例

### 将数据写入Excel文件
```java
package com.sky.test;

import org.apache.poi.xssf.usermodel.XSSFCell;
import org.apache.poi.xssf.usermodel.XSSFRow;
import org.apache.poi.xssf.usermodel.XSSFSheet;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;

public class POITest {

    /**
     * 基于POI向Excel文件写入数据
     * @throws Exception
     */
    public static void write() throws Exception{
        //在内存中创建一个Excel文件对象
        XSSFWorkbook excel = new XSSFWorkbook();
        //创建Sheet页
        XSSFSheet sheet = excel.createSheet("itcast");

        //在Sheet页中创建行，0表示第1行
        XSSFRow row1 = sheet.createRow(0);
        //创建单元格并在单元格中设置值，单元格编号也是从0开始，1表示第2个单元格
        row1.createCell(1).setCellValue("姓名");
        row1.createCell(2).setCellValue("城市");

        XSSFRow row2 = sheet.createRow(1);
        row2.createCell(1).setCellValue("张三");
        row2.createCell(2).setCellValue("北京");

        XSSFRow row3 = sheet.createRow(2);
        row3.createCell(1).setCellValue("李四");
        row3.createCell(2).setCellValue("上海");

        FileOutputStream out = new FileOutputStream(new File("D:\\itcast.xlsx"));
        //通过输出流将内存中的Excel文件写入到磁盘上
        excel.write(out);

        //关闭资源
        out.flush();
        out.close();
        excel.close();
    }
    public static void main(String[] args) throws Exception {
        write();
    }
}
```

### 读取Excel文件中的数据

```java
public class POITest {
    /**
     * 基于POI读取Excel文件
     * @throws Exception
     */
    public static void read() throws Exception{
        FileInputStream in = new FileInputStream(new File("D:\\itcast.xlsx"));
        //通过输入流读取指定的Excel文件
        XSSFWorkbook excel = new XSSFWorkbook(in);
        //获取Excel文件的第1个Sheet页
        XSSFSheet sheet = excel.getSheetAt(0);

        //获取Sheet页中的最后一行的行号
        int lastRowNum = sheet.getLastRowNum();

        for (int i = 0; i <= lastRowNum; i++) {
            //获取Sheet页中的行
            XSSFRow titleRow = sheet.getRow(i);
            //获取行的第2个单元格
            XSSFCell cell1 = titleRow.getCell(1);
            //获取单元格中的文本内容
            String cellValue1 = cell1.getStringCellValue();
            //获取行的第3个单元格
            XSSFCell cell2 = titleRow.getCell(2);
            //获取单元格中的文本内容
            String cellValue2 = cell2.getStringCellValue();

            System.out.println(cellValue1 + " " +cellValue2);
        }

        //关闭资源
        in.close();
        excel.close();
    }

    public static void main(String[] args) throws Exception {
        read();
    }
}
```
