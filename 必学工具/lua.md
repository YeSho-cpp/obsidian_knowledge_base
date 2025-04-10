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
