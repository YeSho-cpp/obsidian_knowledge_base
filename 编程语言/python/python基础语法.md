# Python语言介绍
## ⼀. 编译型和解释型
有各种各样的编程语⾔都能编写计算机程序, 但是计算机并不能直接识别
和执⾏编程语⾔编写的程序代码.
<img src="https://article.biliimg.com/bfs/article/6e06db7e49f8d5fe1dfb7b7e98e7a190eec488eb.png" alt="image.png" style="zoom:50%;" />计算机只能认识⼀种语⾔, 就是⼆进制的机器码
![image.png](https://article.biliimg.com/bfs/article/8c539dbb775d148bde8dcf8b47512dfda07e9c66.png)

程序需要最终转换为⼆进制的机器码才能被执⾏, ⽽转换的过程就分为两
种:<font color=#ff0000>编译型</font>和<font color=#ff0000>解释型</font>
![image.png](https://article.biliimg.com/bfs/article/d5228ca9598d6e910ff638b2e2ee56cbb14196b9.png)

我们写⼀个程序, 把⼤象装冰箱, 总共分三步, 打开冰箱⻔、把⼤象装冰箱、
关上冰箱⻔
程序写好之后, 需要转换成⼆进制的机器码才能执⾏
![image.png](https://article.biliimg.com/bfs/article/059a008449badf5c110074e834883bdae642c191.png)

对于编译型语⾔通过<font color=#ff0000>编译器编译</font>之后可以直接将代码⽣成⼆进制程序执⾏
![image.png](https://article.biliimg.com/bfs/article/53a49c7c22dc80dab8415da599528b59a4a54888.png)

⽽对于解释型语⾔需要在执⾏的时候通过解释器<font color=#ff0000>解释⼀段执⾏⼀段</font>
**总结**
编译器和解释器都会将源代码转换成机器码或字节码。但是二者的实现方式不同：
- 编译器会在编译阶段将<font color=#ff0000>整个源代码编译成</font>目标文件、可执行文件或库文件等形式的二进制代码,这些二进制代码可以直接由计算机硬件执行。
- 解释器则是<font color=#ff0000>逐行解释</font>源代码,并动态地生成中间表达形式或字节码,最终再根据解释器的实现,将这些中间码翻译成机器码并运行。
![[解释型语言和编译型语言优缺点.km]]


## 二. 认识数据类型
在 Python ⾥为了应对不同的业务需求,也把数据分为不同的类型。
![[python数据类型.km]]

> 检测数据类型的方法：`type()`

### 变量类型的特征
在Python中定义变量是 不需要指定类型（在其他很多⾼级语⾔中都需要）
```python
#不需要执行数据类型a是int类型
a = 10
#b是float类型
b = 1.12345
```

### 不可变类型与可变类型
#### 不可变类型
在 Python 中,不可变类型是指<font color=#ff0000>创建后不可以被修改的对象</font>。例如,**整数、浮点数、字符串、元组**等都是不可变类型。
对于不可变类型,如果进行修改操作,会产生一个<font color=#ff0000>新的对象</font>,而原来的对象不受影响。例如,对字符串进行操作后,原字符串并没有改变,而是返回一个新的字符串。例如：
```python
s = "hello"
s2 = s.upper()   # s 不发生变化,s2 是更新为大写后的新字符串
print(s)    # 输出 hello
print(s2)   # 输出 HELLO
```
重点
不可变类型在赋值时是<font color=#ff0000>创建新的对象</font>,而不是指向同一对象
```python
a = 'hello'
b = a    # b 和 a 不是指向同一对象,而是分别指向两个内容相同的新字符串对象
print(a, b)  # 输出 hello hello
a = 123
b = a    # b 和 a 不是指向同一对象,而是分别指向两个内容相同的新整数对象
print(a, b)  # 输出 123 123
```
#### 可变类型
在 Python 中,可变类型是指可以被<font color=#ff0000>修改的对象</font>。例如,列表、字典、集合等都是可变类型。
相比之下,对于可变类型,如果进行修改操作,会直接影响原对象本身,而不会创建新的对象。例如,向列表中添加或删除元素,会直接修改原列表。例如：
```python
lst = [1, 2, 3]
lst.append(4)   # 在原列表上添加一个元素
print(lst)      # 输出 [1, 2, 3, 4]
```
需要注意的是可变类型<font color=#ff0000>赋值时两个变量</font>指向同一对象的：
```python
a=[1,2,3]
b=a
b.append(4)
print(b) # [1,2,3,4]
```
### 格式化输出
所谓的格式化输出即按照⼀定的格式输出内容。
#### 认识格式化符号
下面是 Python 中常见的格式化符号：

| 格式化操作符 | 含义 |
|:-:|:-:|
| `%d`         | 整数 |
|`%f`| 浮点数 |
|`%u`|无符号十进制整数|
| `%s`         | 字符串 |
| `%r`         | 带有引号的字符串 |
| `%c`         | 字符 |
| `%o`         | 八进制数 |
| `%x`         | 十六进制数（小写字母） |
| `%X`         | 十六进制数（大写字母） |
|`%%`| 字符% |
>技巧
- `%06d`:表示输出的整数显示位数,不足以0补全,超出当前位数则原
样输出
- `%.2f`:表示小数点后显示的小数位数。

#### 格式化符号基础使用方法
##### %格式化
```python
age = 18
name = 'TOM'
weight = 75.5
stu_id = 1
stu_id2 = 1000
# 1 . 今年我的年龄是x岁 -- 整数 % d
print('今年我的年龄是%d岁' %age)
# 2 . 我的名字是x -- 字符串 % s
print('我的名字是%s' %name)
# 3. 我的体重是x公⽄ -- 浮点数 % f
print('我的体重是%.3f公⽄' %weight)
# 4 . 我的学号是x -- % d
print('我的学号是%d' %stu_id)
# 4 .1 我的学号是001
print('我的学号是%03d' % stu_id)
print('我的学号是%03d' % stu_id2 )
# 5 . 我的名字是x,今年x岁了
print('我的名字是%s,今年%d岁了' % (name, age))
# 5 .1 我的名字是x,明年x岁了
print('我的名字是%s,明年%d岁了' % (name, age + 1 ))
```

格式化符号高级使用方法
- `%Ond`,表示输出的整数显示位数,不足时以0补全,超出当前位数则
原样输出
- `%.nf`,表示小数点后面显示的小数位数为n位
- 在print中用输出符号占位,在百分号后面用小括号把对应的变量给括
起来


##### f-格式化字符串
格式化字符串除了%s,还可以写为f '{表达式}'
```python
name = 'TOM '
age = 18
# 我的名字是x,今年x岁了
print('我的名字是% s,今年% s岁了' % (name,age))
# 语法 f'{ 表达式} '
print(f'我的名字是{name},今年{age}岁了')
```

> f-格式化字符串是Python3.6中新增的格式化方法,该方法更简单易读。

##### format()方法
在 Python 中， `format()` 方法是一种用于格式化字符串的方式。它可以通过在字符串中使用花括号 `{}` 来指示需要填充的值，并通过该方法中传入的参数对其进行替换。下面是一个简单的例子： 

```python
name = "Alice"
age = 25
print("My name is {}, and I am {} years old".format(name, age))
```

在这个例子中，`format()` 方法将 `name` 和 `age` 替换到字符串 `"My name is {}, and I am {} years old"` 中的占位符 `{}` 中。当打印输出时，`{}` 中的内容将被相应的变量替换，输出结果为：

```
My name is Alice, and I am 25 years old
```

此外，`format()` 方法还支持更多的格式化选项，例如指定小数点位数、格式化时间和日期等。以下是另一个例子：

``` python
pi = 3.1415926
print("{:.2f}".format(pi)) # 保留两位小数
import datetime
today = datetime.datetime.now()
print("{:%Y-%m-%d %H:%M:%S}".format(today)) # 格式化当前时间
```

运行结果为：

```
3.14
2021-07-29 22:03:42
```

除了上述示例中使用的位置型参数外，`format()` 方法还支持关键字参数和下标引用等高级用法。这些方法非常灵活，可以满足各种不同的字符串格式化需求。


##### 转义字符
- `\n`︰换行。
- `\t`:制表符,一个tab键(4个空格)的距离。


```python
print ( 'hello ' )
print ( 'world ' )

print( 'hello\nPython ' )
print( '\tabcd ' )
```

##### 结束符
>想—想,为什么两个print会换行输出?

```python
print( '输出的内容',end="\n")
```

>在Python中,print(),默认自带end="\n"这个换行结束符,所以导致每两个print直接会换行展示,用户可以按需求更改结束符。

```python
#以换行符结尾
print ( 'hello ' , end="\n")
#以制表符
print ( 'world ' , end="\t" )
 #以...结尾
print ( 'hello ', end= "..." )
```



**总结**
- 格式化符号
	- `%s`:格式化输出字符串 
	- `%d`:格式化输出整数。 
	- `%f`:格式化输出浮点数
- f-字符串
	- `f'{表达式}'`
- 转义字符
	- `\n`:换行
	- `\t`:制表符
- print结束符
```python
print('内容',end="")
```

## python输入
在Python中,程序接收用户输入的数据的功能即是输入。
<img src="https://article.biliimg.com/bfs/article/2b20b04c4a77d50c3d3f4dc162ed89055cac35a1.png" alt="image.png" style="zoom:35%;" />

**输入的语法**
```python
input("提示信息")
```

**输入的特点**
- 当程序执行到`input` ,等待用户输入,输入完成之后才继续向下执行。
- 在Python中,input接收用户输入后,一般存储到变量,方便使用。
- 在Python中,input会把接收到的任意用户输入的数据都当做字符串处理。

```python
password = input('请输⼊您的密码：')
print(f'您输⼊的密码是{password}')
# <class 'str'>
print(type(password))
```

**总结**
- 输⼊功能
	- input('提示⽂字')
- 输⼊的特点
	- ⼀般将input接收的数据存储到变量
	- input接收的任何数据默认都是字符串数据类型

# python转换数据类型

## 转换数据类型的函数

下面是Python中常用的数据类型转换函数以及使用方法的表格：

| 函数名 | 描述 | 
|:-:|:-:|
|`int(x)`| 将x转换为一个整数。 |
| `float(x)` | 将x转换为一个浮点数。 |
|`str(x)`|将对象 x 转换为字符串。|
| `repr(x)` | 将对象 x 转换为表达式字符串。 |
| `eval(str)` | 用来计算在字符串中的有效Python表达式,并返回一个对象。 |
| `tuple(s)` | 将序列 s 转换为一个元组。 |
| `list(s)` | 将序列 s 转换为一个列表。 |
| `set(s)` |转换为可变集合。|
| `dict(d)` |创建一个字典。d 必须是一个序列 (key,value)元组。|
| `frozenset(s)` | 转换为不可变集合。 |
| `chr(x)` | 将一个整数转换为一个字符。 |
| `ord(x)` |将一个字符转换为它的整数值。|
| `hex(x)` | 将一个整数转换为一个十六进制字符串。 |
|`oct(x)`| 将一个整数转换为一个八进制字符串。 |
|`bin(x)`|将一个整数转换为一个二进制字符串。|

表达式字符串指的是Python中可以使用`eval()`函数进行求值的字符串表达式,它包含了Python语言中合法的表达式和变量。使用`repr()`函数可将一个对象转换成Python表达式字符串。

例如,对于表达式字符串`"1+2"`,我们可以使用`eval("1+2")`进行求值并返回结果3。类似地,对于表达式字符串`"len('hello')"`,我们同样可以使用`eval("len('hello')")`进行求值并返回结果5。
```python
# 1 . floa t() -- 转换成浮点型
num_1 = 1
print(float(num_1 ))
print(type(float(num_1 )))
# 2 . str() -- 转换成字符串类型
num_2 = 10
print(type(str(num_2)))
# 3. tuple() -- 将⼀个序列转换成元组
list1 = [10 , 20 , 30 ]
print(tuple(list1 ))
print(type(tuple(list1 )))
# 4 . list() -- 将⼀个序列转换成列表
t1 = (100 , 200 , 300 )
print(list(t1))
print(type(list(t1)))
# 5 . eval() -- 将字符串中的数据转换成Python表达式原本类型
str1 = '10 '
str2 = '[ 1, 2,3] '
str3 = '(1000 , 2000 , 3000 )'
print(type(eval(str1 )))
print(type(eval(str2 )))
print(type(eval(str3)))
```

# 运算符的分类
- 算数运算符
- 赋值运算符
- 复合赋值运算符
- 比较运算符
- 逻辑运算符
## 算数运算符
下面是Python常用的算数运算符及其示例：

|运算符| 描述 | 示例 |
|:-:|:-:|:-:|
|`+`| 加法 | 2 + 3 = 5 |
|`-`| 减法 | 5 - 2 = 3 |
|`*`| 乘法 | 2 * 3 = 6 |
|`/`|除法（得到一个浮点数）| 7 / 2 = 3.5 |
|`//`| 整除（得到一个整数） | 7 // 2 = 3 |
|`%`| 取模（得到余数） | 7 % 2 = 1 |
|`**`| 幂运算 | 2 ** 3 = 8 |

## 赋值运算符
单个变量赋值
```python
num = 1
print( num)
```
python允许多个变量赋值
```python
num1, float1, str1 = 10,0.5, 'hello world '
print(num1)
print(float1)
print(str1)
```

多变量赋相同值
```python
a = b = 10
print(a)
print(b)
```

## 复合赋值运算符

Python中的复合赋值运算符是一种方便的简写方式,可以将一个运算符和等号结合起来用于赋值操作。下面是Python常用的复合赋值运算符及其示例：

|运算符| 描述 |示例|
|:-:|:-:|:-:|
|`+=`| 加法赋值 |`x += 2 等同于 x = x + 2`|
|`-=`| 减法赋值 |`x -= 2 等同于 x = x - 2`|
| `*=`     | 乘法赋值 |`x *= 2` 等同于 `x = x * 2`|
| `/=`     | 除法赋值 |` x /= 2 `等同于 `x = x / 2 ` |
| `//=`    | 整除赋值 | `x //= 2` 等同于 `x = x // 2` |
| `%=`     | 取模赋值 |` x %= 2` 等同于` x = x % 2` |
| `**= `  | 幂运算赋值 |`x **= 2` 等同于 `x = x ** 2`|

## 逻辑运算符

Python的主要用于连接多个条件表达式,判断其组合是否成立,返回布尔值`True`或`False`。下面是Python常用的逻辑运算符及其示例：

|运算符| 描述 | 示例 |
|:-:|:-:|:-:|
| `and`     | 与运算,当所有条件都为`True`时返回`True`,否则返回`False` | `x > 0 and y < 10` |
| `or`     | 或运算,当任一条件为`True`时返回`True`,否则返回`False` | `x == 0 or y == 0` |
| `not`      | 非运算,当条件为`False`时返回`True`,否则返回`False` | `not (x == y)` |

# 判断语句和循环语句

## if语法
### if判断语句基本格式介绍
- if语句是用来进行判断的,满足条件执行特定的代码,其使用格式如
```python
if 要判断的条件:
	条件成立时,要做的事情
```

代码示例:
```python
if False:
	print( '条件成立执行的代码1')
	print( '条件成立执行的代码2')
#注意:在这个下方的没有加缩进的代码,不属于if语句块,即和条件成立与否
print( '这个代码执行吗?')
```

### if语句使用逻辑运算

>在程序开发中,执行结果可能和多个条件有关比如多个条件都成立才能执行,或者有一个条件成立就可以执行,这时就需要使用逻辑运算符逻辑运算符可以把多个条件按照逻辑进行连接,变成更复杂的条件

```
输入年龄age,编写代码判断年龄是否正确,要求人的年龄在0-120之间
```
代码实现:
```python
# 输入年龄
age = input('请输入年龄')
#判断是否在0-120之间
if age>=0 and age<=120:
	print('年龄正确')
```

### if-else
>想—想:在使用if的时候,它只能做到满足条件时要做的事情。那万一需要在不满足条件的时候,做某些事,该怎么办呢?
答:使用if-else

**if-else的使用格式**
```python
if 条件:
满足条件时要做的事情1
满足条件时要做的事情2
满足条件时要做的事情3...(省略)...
else:
不满足条件时要做的事情1
不满足条件时要做的事情2
不满足条件时要做的事情3...(省略)...
```

> 输入年龄,如果年龄大于等于18岁,可以上网,否则提示小朋友回家写作业去

代码实现:
```python
age = int( input('请输入您的年龄:'))
if age >= 18:
	print(f'您输入的年龄是{age} ,已经成年,可以上网')
else:
	print(f'您输入的年龄是{age} ,小朋友,回家写作业去')
```

### if...elif...else...语句格式

**多重判断elif**
elif的使用格式如下:
```python
if xxx1
事情1
elif xxx2:
事情2
elif xxx3:
事情3
else xxx4
剩余的情况
```
- 当xxx1满足时,执行事情1,然后整个if结束
- 当xxx1不满足时,那么判断xxx2,如果xxx2满足,则执行事情2,然后整个if结束
- 当xxx1不满足时,xxx2也不满足,如果xxx3满足,则执行事情3,然后整个if结束

### if 实现三目运算操作
语法如下:
```python
条件成立执行的表达式 if 条件 else 条件不成立执行的表达式
```
`a if a>b else b`
如果a > b的条件成立,三目运算的结果是a,否则就是b
案例:
```python
#求a和b两个数字中的较大值
a = 10
b = 20
#使用三目运算符求较大值
max = a if a > b else b
print("较大值为:%d" % max)
```

## while循环
在程序中需要重复的执行一个功能的时候,用循环来实现。
**1. while循环的格式**
```python
while 条件:
	条件满足时, 做的事情1
	条件满足时, 做的事情2
	条件满足时, 做的事情3
	...(省略)...
```
**2体验while循环**
> 打印5遍媳妇儿,我错了


```python
i = 1
while i <= 5:
	print( '媳妇儿,我错了')
	i += 1 # i = i +1
```

### for循环
像while循环一样,for可以完成循环的功能。
在Python中for循环可以遍历任何序列的项目,如一个列表或者一个字符串等。
**for循环的格式**
```python
for 临时变量 in 列表或者字符串等可迭代对象:
	循环满足条件时执行的代码
```
**遍历字符串中每⼀个元素**
```python
str1 = 'itheima '
for i in str1 :
	print(i)
```

# python容器

## 字符串
python字符串支持`''`字符串
```python
name3 = ''' Tom '''
name4 = """ Rose """
a = ''' i a m T om ,
nice to meet y ou! '''
b = """ i am Rose,
nice to meet y ou! """
print(a)
print(b,end="")
```
✍️注意
	三引号形式的字符串⽀持换⾏。
### 下标和切片
#### 1.下标索引
所谓“下标”,就是编号,就好比超市中的存储柜的编号,通过这个编号就能找到相应的存储空间
- 生活中的"下标"超市储物柜
- <img src="https://article.biliimg.com/bfs/article/62778fe1638694031b05c3f1b61ea57821a253cc.png" alt="image.png" style="zoom:50%;" />
- 字符串中"下标"的使用
如果有字符串:` name = 'abcdef'`,在内存中的实际存储如下:
<img src="https://article.biliimg.com/bfs/article/ffbd332e599c0de1d7fd5b3293b43fd25720e0a9.png" alt="image.png" style="zoom:50%;" />

如果想取出部分字符,那么可以通过下标的方法,（注意python中下标从О开始)

#### 2切片
切片是指对操作的对象截取其中一部分的操作。**字符串、列表、元组**都支持切片操作。
**切片的语法︰`[起始:结束:步长]`**
注意:选取的区间从"起始"位开始,到结束"位的前一位结束（不包含结束位本身),步长表示选取间隔。
我们以字符串为例讲解。
如果取出一部分,则可以在中括号]中,使用:
```python
name = 'abcdef'
print(name[0:3])#取下标0~2的字符
print(name[2:] )#取下标为2开始到最后的字符
print(name[1:-1])#取下标为1开始到最后第2个之间的字符
```
### 字符串常见操作

下面是按照操作顺序整理的字符串操作的五列markdown表格：

| 操作名 | 操作语法 | 操作示例代码 | 示例结果 | 操作含义 |
|:-:|:-:|:-:|:-:|:-:|
| `find()` | `字符串序列.find(子串, 开始位置下标, 结束位置下标)` | `'hello world YeSho and cpp'.find('world', 3, 15)` | `6` | 返回子串中第一个字符所在的索引值,如果没有找到则返回-1。此处子串为"world",开始位置下标为3,结束位置下标为15(不包括该位置)。 |
| `index()` | `字符串序列.index(子串,start,end)` | `'hello world YeSho and cpp'.index('cpp', 0, 25)` | `23` | 返回子串中第一个字符所在的索引值,如果没有找到则报错。此处子串为"cpp",开始位置下标为0,结束位置下标为25(不包括该位置)。 |
| `count()` |`字符串序列.count(sub[, start[, end]])`| `'hello world YeSho and cpp'.count('o', 5, 20)` | `1` | 统计子串在字符串序列中出现的次数。此处子串为"o",开始位置下标为5,结束位置下标为20(不包括该位置)。 |
| `replace()` |`字符串序列.replace(旧子串, 新子串[, 替换次数])`| `'hello world YeSho and cpp'.replace('YeSho', 'User')` | `'hello world User and cpp'` | 将字符串序列中的所有旧子串替换成新子串,可以指定替换次数。此处将"YeSho"替换成"User"。 |

|操作名|操作语法|操作示例代码|示例结果|操作含义|
|:-:|:-:|:-:|:-:|:-:|
| `split()` | `字符串序列.split([分隔符][,maxsplit])` | `'hello world YeSho and cpp'.split(' ', 3)` | `['hello', 'world', 'YeSho', 'and cpp']` |使用分隔符将字符串分割成字符串**列表**,可以指定分割次数。此处分隔符为" ",分割次数为3。|
| `join()` | `连接符.join(字符串序列)` | `' '.join(['hello', 'world', 'YeSho', 'and', 'cpp'])` | `'hello world YeSho and cpp'` | 将字符串序列中的元素使用连接符拼接成一个字符串。此处连接符为" "。 |
| `capitalize()` | `字符串序列.capitalize()` | `'hello world YeSho and cpp'.capitalize()` | `'Hello world yesho and cpp'` | 将字符串序列第一个字符转化成大写字母,其余字符转化成小写字母。 |
| `title()` | `字符串序列.title()` | `'hello world YeSho and cpp'.title()` | `'Hello World Yesho And Cpp'` | 将字符串序列中每个单词的首字母转化成大写字母,其余字母转化成小写字母。 |

|操作名|操作语法|操作示例代码|示例结果|操作含义|
|:-:|:-:|:-:|:-:|:-:|
|`lower()`| `字符串序列.lower()` | `'hello world YeSho and cpp'.lower()` | `'hello world yesho and cpp'` | 将字符串序列中所有字符转换成小写字母。 |
| `upper()` | `字符串序列.upper()` | `'hello world YeSho and cpp'.upper()` | `'HELLO WORLD YESHO AND CPP'` | 将字符串序列中所有字符转换成大写字母。 |
| `lstrip()` | `字符串序列.lstrip([去除字符])` | `' hello world YeSho and cpp '.lstrip()` | `'hello world YeSho and cpp '` | 去掉字符串序列左侧的空格和指定字符（默认为空格）。 |
| `rstrip()` | `字符串序列.rstrip([去除字符])` | `' hello world YeSho and cpp '.rstrip()` | `' hello world YeSho and cpp'` | 去掉字符串序列右侧的空格和指定字符（默认为空格）。 |

|操作名|操作语法|操作示例代码|示例结果|操作含义|
|:-:|:-:|:-:|:-:|:-:|
| `strip()` | `字符串序列.strip([去除字符])` | `' hello world YeSho and cpp '.strip()` | `'hello world YeSho and cpp'` | 去掉字符串序列左右两侧的空格和指定字符（默认为空格）。 |
| `ljust()` | `字符串序列.ljust(长度[,填充字符])` | `'hello'.ljust(10, '-')` | `'hello-----'` | 使用指定字符在字符串序列右侧填充至指定长度。此处字符长度为10,用"-"进行右侧填充。 |
| `rjust()` | `字符串序列.rjust(长度[,填充字符])` | `'hello'.rjust(10, '-')` | `'-----hello'` | 使用指定字符在字符串序列左侧填充至指定长度。此处字符长度为10,用"-"进行左侧填充。 |
| `center()` | `字符串序列.center(长度[,填充字符])` | `'hello'.center(9, '-')` | `'--hello--'` | 使用指定字符在字符串序列左右两侧填充至指定长度。此处字符长度为9,用"-"进行左右两侧填充。 |

|操作名|操作语法|操作示例代码|示例结果|操作含义|
|:-:|:-:|:-:|:-:|:-:|
| `startswith()` | `字符串序列.startswith(子串,[起始位置,[终止位置]])` | `'hello world YeSho and cpp'.startswith('hello',0,4)` | `True` | 判断字符串序列是否以指定子串开头,可以指定起始位置和终止位置。此处判断从开头到第4个字符位置是否以"hello"开头。|
| `endswith()` | `字符串序列.endswith(子串,[起始位置,[终止位置]])` | `'hello world YeSho and cpp'.endswith('cpp',20,25)` | `True` |判断字符串序列是否以指定子串结尾,可以指定起始位置和终止位置。此处判断从第20个字符位置到结尾是否以"cpp"结尾。|
| `isalpha()` | `字符串序列.isalpha()` | `'hello world YeSho and cpp'.isalpha()` | `False` | 判断字符串序列中所有字符是否都是字母,如果是返回True,否则返回False。|
| `isdigit()` | `字符串序列.isdigit()` | `'2021'.isdigit()` | `True` | 判断字符串序列中所有字符是否都是数字,如果是返回True,否则返回False。 |

| 操作名 | 操作语法 |操作示例代码| 示例结果 | 操作含义 |
|:-:|:-:|:-:|:-:|:-:|
| `isalnum()` | `字符串序列.isalnum()` | `'hello2021'.isalnum()` | `True` | 判断字符串序列是否只包含字母或数字,如果是返回True,否则返回False。 |
| `isspace()` | `字符串序列.isspace()` | `' '.isspace()` | `True` | 判断字符串序列是否只包含空格字符,如果是返回True,否则返回False。 |
| `rfind()` | `字符串序列.rfind(子串,[起始位置,[终止位置]])` | `'hello world YeSho and cpp'.rfind('world',0,15)` | `6` | 返回子串中最后一个字符所在的索引值,如果没有找到则返回-1。此处子串为"world",从左往右第一次出现在位置6。|
|`rindex()`| `字符串序列.rindex(子串,[起始位置,[终止位置]])` | `'hello world YeSho and cpp'.rindex('cpp',0,25)` |`23`|
|`partition` |`字符串序列.partition('分隔符')`|`hello world YeSho and cpp'.partition('and')`|`('hello world YeSho ', 'and', ' cpp')`|将字符串按照分隔符分成三部分,返回一个元组|
|`rpartition`|`字符串序列.rpartition('分隔符')`|`'hello world YeSho and cpp'.rpartition('and')`|`('hello world YeSho ', 'and', ' cpp')`|与 partition() 类似,但从右边开始查找分隔符|
|`splitlines`|`字符串序列.splitlines(keepends=False)` |  |`['hello', 'world', 'YeSho', 'and', 'cpp']`|将字符串按照行拆分成列表,每个元素为一行。如果 keepends=True,则在行末尾保留行结束符(\n),否则删除。如果字符串不以行结束符结尾,则只返回一个元素,即整个字符串。 |
## 列表
### 列表介绍
`<1>`列表的格式
变量A的类型为列表
```python
namesList = [ 'xiaowang ' , 'xiaoZhang' , 'xia oHua ']
```
比C语言的数组强大的地方在于列表中的<font color=#ff0000>元素可以是不同类型</font>的
```python
testList = [ 1, 'a ']
```
### 列表的相关操作
#### 查找函数
- `index()`:返回指定数据所在位置的下标。
1. 语法
```python
列表序列.index(数据,开始位置下标,结束位置下标)
```
2. 快速体验
```python
name_list = ['Tom ' , 'Lily ' , 'Rose']
print(name_list.index ( 'Lily ', 0,2))#1
```
> 注意:如果查找的数据不存在则报错。

- `count()`:统计指定数据在当前列表中出现的次数。
```python
name_list = [ 'Tom ' , 'Lily ', 'Rose ']print(name_list.count( 'Lily ' ))#1
```
- len():访问列表长度、即列表由数据的个数
```python
name_list=['TOM','Lily','roSE']
print(len(name_list)) #3
```

#### 判断是否存在
- `in`∶判断指定数据在某个列表序列,如果在返回True,否则返回False
```python
name_list = [ 'Tom ' , 'Lily ', 'Rose ']
#结果:True
print( 'Lily' in name_list)
#结果:False
print( 'Lilys' in name list)
```
- not in:判断指定数据不在某个列表序列,如果不在返回True,否则
返回False
```python
name_list = [ 'Tom ' , 'Lily ', 'Rose']
#结果:False
print( 'Lily' not in name_list)
#结果:True
print( 'Lilys' not in name_list)
```
#### 添加元素( " 增" append, extend, insert)
当我们需要在 Python 中处理一系列有序数据时,列表是一个非常实用的数据类型。下面我来介绍一下 Python 列表添加元素的三个方法：`append`、`extend` 和 `insert`。 

- `append` 方法可以将指定对象添加到列表尾部,它只接受一个参数。

  ```python
  # append 举例
  a = [1, 2, 3]
  a.append(4)
  print(a)  # 输出 [1, 2, 3, 4]
  ```

- `extend` 方法可以将<font color=#ff0000>指定列表</font>中所有元素添加到当前列表末尾,也只接受一个参数,该参数必须为可迭代对象（如列表、元组、字符串等）。

  ```python
  # extend 举例
  a = [1, 2, 3]
  a.extend([4, 5])
  print(a)  # 输出 [1, 2, 3, 4, 5]
  ```

- `insert` 方法可以将指定对象插入到列表的指定位置,这个方法需要两个参数,第一个参数为要插入的位置,第二个参数为要插入的对象。

  ```python
  # insert 举例
  a = [1, 2, 3]
  a.insert(1, 'hello')
  print(a)  # 输出 [1, 'hello', 2, 3]
  ```
	✍️
	需要注意的是,虽然这三个方法的作用都是向列表中添加元素,但它们的效率和使用场景是不同的。一般来说,`append` 方法比 `extend` 方法效率更高；而对于大规模数据集合,最好使用列表推导式或 `extend` 方法来添加元素。
#### 删除元素( " 删" del, pop, remove)
在 Python 中,我们还可以使用三种方法来删除 List 中的元素。这三种方法是 `del`、`pop` 和 `remove`。

- 使用 `del` 关键字可以删除列表中<font color=#ff0000>指定位置</font>的元素或清空整个列表（<font color=#ff0000>不带参数</font>）。例如：

  ```python
  # del 示例
  a = [1, 2, 3]
  del a[1]     # 删除索引为 1 的元素
  print(a)     # 输出 [1, 3]
  
  b = [1, 2, 3]
  del b       # 清空整个列表
  print(b)     # NameError: name 'b' is not defined
  ```

- `pop` 可以弹出列表中<font color=#ff0000>指定位置</font>的元素,并返回该元素的值,如果未指定索引,则默认弹出最后一个元素。例如：

  ```python
  # pop 示例
  a = [1, 2, 3]
  x = a.pop(1)
  print(x)    # 输出 2
  print(a)    # 输出 [1, 3]
  
  b = [1, 2, 3]
  y = b.pop()
  print(y)    # 输出 3
  print(b)    # 输出 [1, 2]
  ```

- `remove` 方法可以删除列表中<font color=#ff0000>指定的元素</font>,但该方法只会删除第一个匹配项,如果有多个相同的元素,需要多次调用该方法。例如：

  ```python
  # remove 示例
  a = [1, 2, 3, 2]
  a.remove(2)
  print(a)    # 输出 [1, 3, 2]
  a.remove(2)
  print(a)    # 输出 [1, 3]
  ```

✍️注意
	需要注意的是,使用 `del` 和 `pop` 方法时,要确保列表不为空并且指定的索引在列表范围内；而使用 `remove` 方法时,要确保列表中有要删除的元素。

#### 列表修改数据( sort, rev erse)
在 Python 中,我们可以使用 `sort` 方法对列表进行原地排序（即修改原列表）,也可以使用 `reverse` 方法对列表进行倒序排列。

- `sort` 方法可以按升序或降序对列表中的元素进行排序,默认情况下是按升序排序,可以通过指定 `reverse=True` 参数来实现降序排序。例如：

  ```python
  # sort 示例
  a = [3, 1, 4, 2]
  a.sort()
  print(a)    # 输出 [1, 2, 3, 4]
  a.sort(reverse=True)
  print(a)    # 输出 [4, 3, 2, 1]
  ```

  需要注意的是,`sort` 方法会直接原地修改列表,不会返回新的列表对象。

- `reverse` 方法可以将列表中的元素按照相反的顺序排列。例如：

  ```python
  # reverse 示例
  a = [1, 2, 3]
  a.reverse()
  print(a)    # 输出 [3, 2, 1]
  ```

  同样需要注意的是,`reverse` 方法也是原地修改列表,不会返回新的列表对象。

同时,需要注意的是,这两个方法都是作用在列表本身上,不会返回任何值。如果需要得到一个新的排好序或倒序的列表,可以使用 `sorted` 和 `reversed` 函数,这两个函数分别返回一个新的列表对象。
#### 复制( copy )
在 Python 中,我们可以使用 `copy` 方法来复制一个列表。其中,有两种方式可以完成列表的复制：

- 使用切片： `a_copy = a[:]`
- 使用 `copy` 方法： `a_copy = a.copy()`

这两种方式都能够得到原列表 `a` 的一个副本（即新的列表对象）,并且对 `a_copy` 进行修改不会影响原列表 `a`。例如：

```python
# 复制示例
a = [1, 2, 3]
a_copy = a[:]      # 切片复制
print(a_copy)      # 输出 [1, 2, 3]

a_copy[0] = 0      # 修改副本
print(a_copy)      # 输出 [0, 2, 3]
print(a)           # 输出 [1, 2, 3],原列表未受影响

# 使用 copy 方法复制
a = ['foo', 'bar', 'baz']
a_copy = a.copy()
print(a_copy)      # 输出 ['foo', 'bar', 'baz']

a_copy[0] = 'FOO'  # 修改副本
print(a_copy)      # 输出 ['FOO', 'bar', 'baz']
print(a)           # 输出 ['foo', 'bar', 'baz'],原列表未受影响
```

需要注意的是,如果直接将列表赋值给另一个变量,如 `b = a`,则 `b` 和 `a` 实际上是同一个列表对象的两个引用,修改其中任何一个都会影响另外一个。
### 列表推导式
列表推导式是 Python 中很常用、简洁的语法,可以通过一行语句生成新的列表。其基本语法形式为：

```python
[expression for item in iterable if condition]
```

其中,`expression` 表示需要执行的表达式,`item` 表示在 `iterable` 中迭代的变量名,`if condition` 是可选的条件,根据条件筛选数据。

下面是一个生成 1 到 10 的平方数列表的例子：

```python
squares = [x**2 for x in range(1, 11)]
print(squares)
```

输出结果为：

```
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

在这个例子中,我们使用了 `range()` 函数生成 1 到 10 的序列,然后使用列表推导式将每个元素的平方添加到 `squares` 列表中。

另外,我们还可以使用<font color=#ff0000>条件语句来筛选元素</font>,例如：

```python
even_squares = [x**2 for x in range(1, 11) if x % 2 == 0]
print(even_squares)
```

这个例子生成了一个由 1 到 10 中偶数的平方数组成的列表：

```
[4, 16, 36, 64, 100]
```

嵌套的 `for` 循环可以用来实现更复杂的列表推导式,例如可以在循环语句中嵌套一个条件语句来根据特定的条件过滤元素。

如果我们要求得两个列表的笛卡尔积,也可以使用嵌套的 `for` 循环来实现,例如：

```python
list_a = [1, 2, 3]
list_b = ['a', 'b', 'c']
cartesian_product = [(a, b) for a in list_a for b in list_b]
```

在这个例子中,我们定义了两个列表 `list_a` 和 `list_b`,然后使用两个嵌套的 `for` 循环遍历了这两个列表中所有可能的元素组合,将它们作为二元组 `(a, b)` 添加到 `cartesian_product` 列表中,最终得到了两个列表的笛卡尔积。\
列表推导式还可以这样
```python
seg_array[seg_array > 0] = 1 #这种在列表里判断是否并赋值的简短写法
```

## 元组
### 元组介绍
一个元组可以存储多个数据,元组内的数据是<font color=#ff0000>不能修改</font>的。
元组特点:定义元组使用**小括号**,且逗号隔开各个数据,数据可以是不同的数据类型。
注意︰如果定义的元组只有一个数据,那么这个数据后面也好添加逗号,否则数据类型为唯一的这个数据的数据类型
```python
t2 = (10,)
print (type(t1)) #tuple
t3 = (20)
print (type(t3)) #int
```

### 元组的常见操作
元组数据不支持修改,只支持查找,具体如下:
- 按下标查找数据
```python
tuple1 = ( 'aa ', 'bb', 'cc ', 'bb' )
print(tuple1[0]) #aa
```
- `index()`:查找某个数据,如果数据存在返回对应的下标,否则报错,语法和列表、字符串的index
方法相同。
```python
tuple1 = ( 'aa ', 'bb', 'cc ', 'bb')
print(tuple1 .index ( 'aa ' ))# 0
```
- `count()`︰统计某个数据在当前元组出现的次数。
```python
tuple1 = ( 'aa ', 'bb', 'cc ' , ' bb')
print(tuple1.count('bb')) #2
```
- `len()`:统计元组中数据的个数。
```python
tuple1 = ( 'aa ' , 'bb', 'cc' , 'bb ' )
print(len(tuple1 )) #4
```
> 注意:元组内的直接数据如果修改则立即报错


```python
tuple1 = ( 'aa ' , 'bb', 'cc' , 'bb')
tuple1[0] = 'aaa'
```
> 但是如果元组里面有列表,修改列表里面的数据则是支持的,
故自觉很重要。

```python
tuple2 = (10,20, [ 'aa ' , ' bb', 'cc 'l, 50,30 )
print(tuple2[2] )#访问到列表
tuple2[2][0] = 'aaaaa '
print(tuple2) #结果:(10,20,[ faaaaa ' , ' bb ' , 'cc '], 50,30 )
```

## 字典
### 字典介绍
字典里面的数据是以<font color=#ff0000>键值对</font>形式出现,字典数据和数据顺序没有关系,即字典<font color=#ff0000>不支持下标,</font>
后期无论数据如何变化,只需要按照对应的键的名字查找数据即可。
字典特点∶
- 符号为大括号
- 数据为键值对形式出现
- 各个键值对之间用逗号隔开
```python
#有数据字典
dict1 = { 'name': 'Tom' , 'age': 20, 'gender':'男'}
#空字典
dict2={}
dict3=dict()
```

> 注意:一般称冒号前面的为键(key),简称k;冒号后面的为值(value),简称v。

### 字典常见操作
#### 增
Python 字典的增加元素操作可以通过以下两种方式实现：

##### 1. 直接赋值

你可以直接创建一个新的键值对,并将其添加到字典中。例如,下面这行代码就创建了一个新的字典,并向其中添加了一个键为 `'name'`、值为 `'Alice'` 的键值对：

```python
my_dict = {}
my_dict['name'] = 'Alice'
```

你还可以连续添加多个键值对,例如：

```python
my_dict['age'] = 25
my_dict['city'] = 'New York'
my_dict['country'] = 'USA'
```

上述代码将分别向 `my_dict` 字典中添加三个键值对,其中键 `'age'` 对应的值为 `25`,键 `'city'` 对应的值为 `'New York'`,键 `'country'` 对应的值为 `'USA'`。

##### 2. 使用 `update()` 方法

除了直接赋值外,你还可以使用 `update()` 方法向字典中添加多个键值对。`update()` 方法的参数可以是另一个字典对象,也可以是一个包含键值对的序列（如列表或元组）。

演示了如何使用 `update()` 方法往字典中添加键值对：

```python
my_dict = {'name': 'Alice', 'age': 25}
new_data = {'city': 'New York', 'country': 'USA'}
my_dict.update(new_data)
```

在上述代码中,我们首先创建了一个字典 `my_dict`,它包含两个键值对。然后,我们又创建了一个字典 `new_data`,它包含两个键值对：键 `'city'` 对应的值是 `'New York'`,键 `'country'` 对应的值是 `'USA'`。

最后,我们使用 `update()` 方法将 `new_data` 中的键值对添加到 `my_dict` 中。执行完这段代码后,字典 `my_dict` 的内容就变成了：

```python
{'name': 'Alice', 'age': 25, 'city': 'New York', 'country': 'USA'}
```

📢注意
		尝试访问一个字典中不存在的键,Python 会引发 `KeyError` 异常,两种方法可以避免这种情况发生。第一种是使用 `in` 关键字,第二种使用 `get()` 方法,不存在会返回`none`
### 删
- `del() / del`:删除字典或删除字典中指定键值对。c·
```python
dict1 = { ' name ': 'Tom ' , 'age':20, 'gender':'男'}
del dict1[ 'gender'] #结果:{ ' name ':'Tom ,age ': 20}print(dict1 )
```
. `clear()`︰清空字典
```python
dict1 = {' name ': 'Tom ' , 'age ': 20,'gender':'男'}
dict1.clear() #{}
```
### 查
- `keys()`
获取字典所有的键
```python
dict1 = { ' name ':  'Tom' , 'age':20, 'gender': '男}
print(dict1.keys()) #dict_keys([ 'name' , 'age' , 'gender'])
```
- `values()`
获取字典所有的值
```python
dict1 = { 'name': 'Tom' , 'age ': 20,'gender':'男'}
print(dict1.values() ) # dict_values([ 'Tom ' ,20,'男'])
```
- `items()`
获取字典所有的条目
```python
dict1 = { 'name ': 'Tom' , 'age': 20,'gender':'男'}
print(dict1.items()) # dict_items([( 'name' , 'Tom'), ('age','男')])
```

### 字典推导式
字典推导式是一种使用紧凑语法快速创建新字典的方法。它的语法类似于列表推导式,但用 `{}` 符号来表示字典对象。下面是一个简单的例子：

```python
numbers = [1, 2, 3, 4]
squares = {x: x**2 for x in numbers}
```

在这个例子中,我们定义了一个包含数字的列表 `numbers`,然后使用字典推导式创建了一个新字典 `squares`,其中键为 `numbers` 列表中的元素,值为对应元素的平方。具体来说,推导式的语法是 `{expression for element in iterable}`,其中 `expression` 表示生成字典值的表达式,`element` 表示循环变量,`iterable` 表示迭代对象。因此,上述字典推导式中,变量 `x` 从 `numbers` 中循环取出每个元素,并计算出其平方作为字典的值,最终得到一个以 `1`、`2`、`3` 和 `4` 为键,以它们的平方为值的字典。

除了基本用法,字典推导式也可以结合条件语句和嵌套循环来创建更复杂的字典对象。例如：

```python
items = {'apple': 0.5, 'banana': 0.3, 'cherry': 0.1, 'date': 0.2}
cheap_fruits = {k: v for k, v in items.items() if v < 0.4}
```

在这个例子中,我们定义了一个字典 `items`,其中键为水果名称,值为它们的单价（单位为美元）。然后,使用字典推导式创建了一个新字典 `cheap_fruits`,其中只包含原字典中单价低于 $0.4$ 美元的水果。具体来说,我们使用 `items.items()` 迭代原字典中的所有键值对,然后提取出满足条件 `if v < 0.4` 的键值对,并将其添加到新字典中,最终得到一个以水果名称为键,以低于 $0.4$ 美元的单价为值的字典。
## 集合
### 集合介绍
集合可以<font color=#ff0000>去掉重复数据并且是无序的,不支持下标</font>
`{}`代表的是空字典,不是空集合,创建空集合,需要set()
创建集合使用`{}`或`set()` ,但是如果要创建空集合只能使用set() ,因为`{}`用来创建空字典。
```python
s1 = {10,20,30,40,50}
print(s1)
s2 = {10,30,20,10,30,40,30,50}
print(s2)
s3 = set('abcdefg')
print(s3)
s4 = set()
print(type(s4))# set
s5 = {}
print(type(s5))# dict
```
### 集合常见操作方法
#### 增加数据
- `add()`
```python
s1 = {10,20}
s1.add(100)
s1.add(10)
print(s1) # {100,10,20}
```
>因为集合有去重功能;所以,当向集合内追加的数据是当前集合已有数据的话,则不进行任何操作。

- `update()`,追加的数据是序列。
```python
s1 = {10,20}
# s1.update(100)#报错
s1.update([100,200] )
s1.update( 'abc' )
print(s1)
```
### 删除数据
- `remove()`,删除集合中的<font color=#ff0000>指定数据</font>,如果数据<font color=#ff0000>不存在则报错</font>。
```python
s1 = {10,20}
s1.remove(10)print(s1)
s1.remove(10)#报错
print(s1)
```
- `discard()`,删除集合中的<font color=#ff0000>指定数据</font>,如果数据<font color=#ff0000>不存在也不会报错</font>。
```python
s1 = {10,20}
s1.discard(10)
print(s1)
s1.discard(10)
print(s1)
```
- `pop()`,<font color=#ff0000>随机删除</font>集合中的某个数据,并返回这个数据。
```python
s1 = {10,20,30,40,50}
del_num = s1.pop()
print(del_num)
print(s1)
```

### 集合的交、并、差

在集合中,有几个基本的操作：交、并、差。这些操作可以通过运算符或方法来实现。

- 交（intersection）操作：获取两个集合中共同存在的元素。可以使用“&”运算符或intersection()方法来实现。例如：`{1, 2, 3} & {2, 3, 4}` 或 `{1, 2, 3}.intersection({2, 3, 4})` 的结果都为`{2, 3}`。
- 并（union）操作：获取两个集合中所有不重复的元素。可以使用“|”运算符或union()方法来实现。例如：`{1, 2, 3} | {2, 3, 4}` 或 `{1, 2, 3}.union({2, 3, 4})` 的结果都为`{1, 2, 3, 4}`。
- 差（difference）操作：获取第一个集合中存在而第二个集合中不存在的元素。可以使用“-”运算符或difference()方法来实现。例如：`{1, 2, 3} - {2, 3, 4}` 或 `{1, 2, 3}.difference({2, 3, 4})` 的结果都为`{1}`。

下面是一些代码示例：

```python
set1 = {1, 2, 3}
set2 = {2, 3, 4}

# 交
print(set1 & set2)  # {2, 3}
print(set1.intersection(set2))  # {2, 3}

# 并
print(set1 | set2)  # {1, 2, 3, 4}
print(set1.union(set2))  # {1, 2, 3, 4}

# 差
print(set1 - set2)  # {1}
print(set1.difference(set2))  # {1}
```

在上述示例中,首先定义了两个集合set1和set2。然后,分别使用交、并、差方法或运算符对这两个集合进行操作,并将结果打印出来。
### 集合推导式
集合推导式是一种使用紧凑语法快速创建新集合的方法。它的语法类似于列表推导式和字典推导式,但用 `{}` 符号来表示集合对象。下面是一个简单的例子：

```python
numbers = [1, 2, 3, 4]
squares = {x**2 for x in numbers}
```

在这个例子中,我们定义了一个包含数字的列表 `numbers`,然后使用集合推导式创建了一个新集合 `squares`,其中包含了 `numbers` 列表中每个元素的平方。具体来说,推导式的语法是 `{expression for element in iterable}`,其中 `expression` 表示生成集合元素的表达式,`element` 表示循环变量,`iterable` 表示迭代对象。因此,上述集合推导式中,变量 `x` 从 `numbers` 中循环取出每个元素,并计算出其平方加入到集合中,最终得到一个由 `1`、`4`、`9` 和 `16` 组成的集合。

除了基本用法,集合推导式也可以结合条件语句和嵌套循环来创建更复杂的集合对象。例如：

```python
words = ['apple', 'banana', 'cherry', 'date', 'elderberry']
vowels = {'a', 'e', 'i', 'o', 'u'}
vowel_words = {w for w in words if any(v in w for v in vowels)}
```

在这个例子中,我们定义了一个包含单词的列表 `words`,以及一个元音字母的集合 `vowels`。然后,使用集合推导式创建了一个新集合 `vowel_words`,其中只包含原列表中每个包含元音字母的单词。具体来说,我们使用 `any(v in w for v in vowels)` 判断当前循环变量 `w` 是否包含任何一个元音字母,如果是则将其添加到新集合中。最终得到一个由 `'apple'`、`'banana'`、`'cherry'` 和 `'date'` 组成的集合,因为它们都包含至少一个元音字母。

## 容器公共点
### 运算符

|运算符|描述 |支持容器类型|
|:-:|:-:|:-:|
|`+`|合并|字符串、列表、元组|
|`*`|复制|字符串、列表、元组 |
|`in`|元素是否存在|字符串、列表、元组、字典、集合 |
|`not in`|元素是否不存在|字符串、列表、元组、字典、集合|
`+`
```python
# 1. 字符串
str1 = 'aa '
str2 = 'bb'
str3 = str1 + str2
print(str3) # aabb
#2. 列表
list1 = [1,2]
list2 = [10,20]
list3 = list1 + list2
print(list3) #[1,2,10,20]
# 3. 元组
t1 = (1,2)
t2 = (10,20)
t3 = t1 + t2
print(t3) # (10,20,100,200 )
```

`*`
```python
# 1. 字符串
print('-'*10) # --------
# 2. 列表
list1 = [ 'hello']
print(list1 * 4 )# ['hello' , 'hello','hello' , 'hello']
#3. 元组
t1 = ('world',)
print(t1*4) #('world','world','world', 'world')
```

### 公共方法
|方法名|描述|
|:-:|:-:|
| `len()`   | 返回容器中元素的数量 |
| `del` 或 `del()` | 从容器中删除元素 |
| `max()` | 返回容器中的最大值 |
| `min()` | 返回容器中的最小值 |
| `range(start, end, step)` | 返回指定范围内的整数序列 |
|`enumerate()`|同时返回元素索引和值|
|`zip(list1,list2...)`|两个或多个对象打包成一个元组构成的新对象|
当我们需要遍历一个列表时,有时候需要获取元素的索引和对应的值,这个时候 `enumerate()` 函数就很实用了。下面是一个使用 `enumerate()` 函数遍历列表并输出索引和对应值的例子：

```python
fruits = ['apple', 'banana', 'cherry']
for index, value in enumerate(fruits):
    print(index, value)
```

输出结果为：

```
0 apple
1 banana
2 cherry
```

可以看到,在遍历 `fruits` 列表的过程中,通过 `enumerate()` 函数获取当前元素的索引 `index` 和值 `value`,然后输出即可。

## collections模块
Python的collections模块包含了一些专门处理数据类型的工具类和容器类。它扩展了内置容器(list, dict, set等)的用法,提供了一些新的数据结构类型,方便我们在编程中使用不同的容器类型来操作不同的数据结构。

常用的集合类数据类型如下：

1. `namedtuple`：一个带有命名字段的元组,可以像访问对象属性一样使用其中的元素。
2. `deque`：双端队列,支持从任意一端添加和删除元素。
3. `Counter`：计数器,统计可哈希对象的出现次数。
4. `OrderedDict`：有序字典,维护了字典元素的插入顺序。
5. `defaultdict`：默认字典,字典的子类,支持添加默认值的字典。当字典不存在某个键时,将返回用户指定的默认值。
6. `ChainMap`：多个字典的集合,将多个字典组合成一个字典。

### namedtuple
元组(tuple)不能为元组内部的数据进行命名,所以往往我们并不知道一个元组所要表达的意义,`namedtuple`(具名元组)则为每个成员分配名称索引以及数字索引。namedtuple比普通tuple具有更好的可读性,可以使代码更易于维护。同时与字典相比,又更加的轻量和高效。
```python
import collections
Person=collections.namedtuple('Person',['name',"age",'gender'])
bob=Person(name="bob",age=12,gender='男')

print(bob) # Person(name='bob', age=12, gender='男')
print(type(bob)) # <class '__main__.Person'>
```

### deque()
deque返回一个新的双向队列对象,从左到右初始化(用方法append()) ,从`iterable`(迭代对象)数据创建。如果iterable没有指定,新队列为空。deque队列是由栈或者queue队列生成的,支持线程安全。内存高效添加(append)和弹出(pop),从两端都可以操作数据,与列表1ist相似。
deque与list的区别在于:头部插入与删除的时间复杂度为0(1)。
```
append(x): 添加x到右端
appendleft(x): 添加x到左端
insert( i,x): 在位置i插入×
pop(): 移去并且返回deque最右侧的那一个元素
popleft(): 移去并且返回deque最左侧的那一个元素
count(x): 计算deque中元素等于x的个数
..... 
```
### OrderDict
`OrderDict`有序字典类似于普通字典,但是它会记住首次插入键的顺序。`OrderDict`在迭代操作的时候会保持元素被插入时的顺序,其内部维护着一个根据键插入顺序排序的双向链表,莓及含一个新的完素插入进来的时候,它会被放到链表的能鄱"雷子内部需要组护另外一个链表,Orde rDict对象占用的内存是普通字典的2倍。
```python
from collections import orderedDict
a = orderedDict()
a['A'] = 10
a['B'] = 126 
a['C'] = 117 
a['D']= 13 
print(a)
# 输出
orderedDict([('A',10),('B',12),('C',11),('D',13)]
```

### defaultdict()
这个factory_function可以是`list、set、str`等等,作用是当key不存在时,返回的是工厂函数的默认值,而相同情况下dict()会返回KeyError。
如果我们将==int==传递给defaultdict()函数,则可以形成一个value为整数的字典。
如果我们将==list==(不带引号）传递给defaultdict()函数,则可以形成一个==value==为列表的字典
```python
from collections import defaultdict
def default_value( ):
	return 0
a = defaultdict(default_value)
b = dict()
print(a[1])
print(b[1])
#输出
0
KeyError: 1
```

```python
a = [('a',1),('b',2),('c',3)]
mylist = defaultdict(list)#创建
for i, j in a:
	mylist[i].append(j)
print(mylist)

# 输出
defaultdict(<class 'list'>, { 'a': [1],'b':[2],'c ': [3]})
```

```python
name = 'Bubbles'
mydict = defaultdict(int) # 创建—个vaLue为整数类型的字典
for i in name :
	mydict[i]+= 1
print(mydict)
# 输出
defaultdict(<class 'int'>, {'B:1,'u': 1,'b': 2,'1': 1,'e':1,'s':1})
```
# 函数
在python中我们怎样返回多个值？
在Python中,一个函数可以返回多个值。实际上,Python中的多个返回值被打包为一个元组（tuple）对象并一起返回。以下是几种方法：

1. 使用元组(tuple)来返回

``` python
def my_func():
    # 进行一些处理
    return value1, value2, value3

# 调用函数并将返回值解包
result1, result2, result3 = my_func()
```

在这个例子中,函数 `my_func()` 返回三个变量 `value1`、`value2` 和 `value3` 的值,利用了 Python 的元组打包和解包特性。调用该函数并将其结果赋值给变量 `result1`、`result2` 和 `result3`,这样就可以分别访问每个值。

2. 使用列表(list) 或 字典(dict)来返回

另一种方法是将多个值放入列表或字典中。这种方式比较灵活,因为它允许您任意组合不同类型的数据,并且可以按照自己的需要访问它们。

``` python
def my_func():
    # 进行一些处理
    return [value1, value2], {'key1': value3}

# 调用函数并访问返回值
result_list, result_dict = my_func()
print(result_list[0])  # 输出第一个值
print(result_dict['key1'])  # 输出第二个值
```

在这个例子中,函数 `my_func()` 返回一个由列表和字典组成的元组。调用该函数并将其结果赋值给变量 `result_list` 和 `result_dict`,使用列表和字典的索引或键可以访问它们的值。

## 函数的不定长参数
Python 中的函数不定长参数,指的是可以接受任意数量或任意类型参数的函数。Python 提供了2种方法来实现这种不定长参数。

1. 使用 `*args` 来表示不定长参数

当函数参数列表中使用星号 `*` 时,它允许您传递任意数量的非关键字参数给函数。所有这些参数将被组织在一个<font color=#ff0000>元组</font>(tuple)中,并将其分配给名称为 `args` 的变量。下面是一个简单的示例：

``` python
def my_func(*args):
    print(args)

my_func('a', 123, [1, 2, 3])
```

输出结果为:

```
('a', 123, [1, 2, 3])
```

在这个例子中,我们定义了一个 `my_func()` 函数,并在参数列表中使用了 `*args`。这意味着该函数可以接受任意数量的非关键字参数,并将它们作为一个元组(`tuple`)存储在 `args` 变量中。然后我们调用该函数并传递3个参数 `'a'`、`123` 和 `[1, 2, 3]`。最后,函数将输出一个包含所有这些参数的元组。

2. 使用 `**kwargs `来表示不定长关键字参数

另一种常见的情况是需要通过名称为关键字参数(keyword arguments)传递一些参数。在这种情况下,可以使用两个星号 `**` 来声明一个关键字参数<font color=#ff0000>字典</font>,其中键是参数名称,而值是参数值。 下面是一个示例：

``` python
def my_func(**kwargs):
    print(kwargs)

my_func(name='Alice', age=25, city='New York')
```

输出结果为:

```
{'name': 'Alice', 'age': 25, 'city': 'New York'}
```

在这个例子中,我们定义了一个 `my_func()` 函数,并在参数列表中使用了 `**kwargs`。这意味着该函数可以接受任意数量的关键字参数,并将它们作为一个字典(dictionary)存储在 `kwargs` 变量中。然后我们调用该函数并传递3个关键字参数,每个参数都有一个名称和一个与之相对应的值。最后,函数将输出一个包含所有这些参数的字典。

除此之外,在 Python 中还可以同时使用 `*args` 和 `**kwargs` 来实现接受任意数量或类型的非关键字和关键字参数。

在Python中,星号`*`除了可以用于指定可变参数外,还可以在序列类型（如列表、元组等）前面加上星号,将其<font color=#ff0000>拆解成独立的位置参数。</font>因此,在代码`print(*l)`中,`*l`表示将列表`l`中的元素依次传递给`print`函数,相当于拆解成多个位置参数,等价于`print(1, 2, 3)`。

传递元组和字典给可变参数
在实参的前面加上`*`的作用是将元组中的元素<font color=#ff0000>解包</font>成一个一个的元素传递给函数.对于元组列表以及集合都可以使用*
```python
def func(*args):
	for ele in args:
		print(ele)
l = [1,2,3]
# 传递元组给可变参数
func(*l)
```
传递字典给可变参数kwargs
在实参的前面加上`**`的作用是将字典中的元素<font color=#ff0000>解包</font>成一个一个的不存在的关键字参数传递给函数.
注意:如果字典前面加上`*`表示的是将字典的key值解包出来
```python
def func(**kwargs):
	for key , value in kwargs.items():
		print(key ,value)
#调用
d = {'name':'张三, 'age': 30}
func(**d)
```


## 高阶函数
### 高阶函数介绍
在 Python 中,函数可以像任何其他对象一样进行操作。高阶函数<font color=#ff0000>是指接受一个或多个函数</font>作为参数,并且/或者返回一个新函数的函数。在 Python 中,高阶函数是一种非常方便和灵活的工具,可以让您编写更加优雅和简洁的代码。
### map高阶函数
map() 是一种常见的 Python 高阶函数,它接受一个函数和一个可迭代对象作为输入,并将该函数应用于可迭代对象的每个元素。map() 函数返回一个新的可迭代对象,其中包含将该函数应用于输入对象的每个元素后得到的结果。

语法：

```
map(function, iterable, ...)
```

参数：

- function：要应用于每个元素的函数。
- iterable：需要处理的序列或集合。

返回值：
- map的返回值类型是<class 'map'>,直接输出是<map object at 0xxxx>,要转成list
例子：

```python
# 将列表中的每个元素平方
lst = [1, 2, 3, 4, 5]
square_lst = list(map(lambda x: x ** 2, lst))
print(square_lst) # [1, 4, 9, 16, 25]

# 将字符串列表中的每个元素转换为整数
str_lst = ['1', '2', '3', '4']
int_lst = list(map(int, str_lst))
print(int_lst) # [1, 2, 3, 4]
```

在这两个示例中,我们都使用了 `map()` 函数来将一个函数应用于一个可迭代对象的所有元素。在第一个示例中,我们使用 lambda 函数来计算每个元素的平方。在第二个示例中,我们使用 `int()` 内置函数来将字符串列表中的每个元素转换为整数。

通过使用 map() 函数,我们可以大大简化对序列或集合的操作,并使代码更加清晰和易读。

### filer高阶函数

`filter()` 函数是 Python 内置函数之一,用于过滤序列。该函数的语法为：

```python
filter(function, iterable)
```

其中,`function` 是一个返回值为布尔类型的函数,用于判断 `iterable` 中的元素<font color=#ff0000>是否符合条件</font>。`iterable` 则是需要进行过滤的序列,如 `list、tuple、dict` 等。

`filter()` 函数会返回一个由符合条件的元素所组成的新序列,其数据类型与 `iterable` 一致。

下面是一个使用 `filter()` 函数的示例代码,它从列表中过滤出所有偶数：

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8]
even_numbers = list(filter(lambda x: x % 2 == 0, numbers))
print(even_numbers)   # [2, 4, 6, 8]
```

在上述示例代码中,我们使用 `lambda` 表达式定义了一个匿名函数,该函数用于判断传入的参数是否为偶数。通过将这个函数作为参数传给 `filter()` 函数,就可以过滤出所有的偶数,并用 `list()` 函数将结果转换成列表形式。

### reduce高阶函数
`reduce()` 函数也是 Python 内置函数之一,用于对可迭代对象（如 list、tuple 等）中的元素进行累积操作。它的语法为：

```python
reduce(function, iterable[, initial])
```

其中,`function` 是一个接受==两个参数==的函数,用于将前两个元素进行运算得到一个结果,再将该结果与后面的元素继续运算,直到遍历完整个序列。

`iterable` 是需要进行操作的序列,而 `initial` 则是可选参数,表示累积计算的初始值。如果没有提供该参数,则默认使用序列中的第一个元素作为初始值。

下面是一个使用 `reduce()` 函数的示例代码,它对列表中的所有元素进行求和：

```python
from functools import reduce

numbers = [1, 2, 3, 4, 5]
sum = reduce(lambda x, y: x + y, numbers)
print(sum)   # 15
```

在上述示例代码中,我们首先通过 `from functools import reduce` 导入了 `reduce()` 函数,然后使用 `lambda` 表达式定义了一个匿名函数,该函数用于将两个参数相加。最后,将这个函数作为参数传给 `reduce()` 函数,就可以对列表中的所有元素进行累加操作,得到它们的和。

需要注意的是,如果要使用 `reduce()` 函数,需要从 functools 模块中导入该函数。


## global
在Python中,`global`是一个关键字,用于在函数内部声明对全局变量的引用。当在一个函数内部对一个变量进行修改,并且该变量与全局作用域中的同名变量存在时,如果没有使用`global`声明,Python会将其视为一个新的局部变量。

下面是一个示例代码：

```python
x = 10   # 全局变量

def func():
    x = 20   # 局部变量
    print("x inside function:", x)

func()
print("x outside function:", x)
```

上述代码中,函数`func`内部使用了与全局作用域中的变量`x`同名的局部变量。因此,函数内部的打印结果为`x inside function: 20`,而外部打印的结果为`x outside function: 10`,这是因为函数内部的变量`x`和全局变量`x`并不是同一个变量。

为了在函数内部对全局变量进行操作,需要使用`global`关键字来声明变量。下面是修改后的示例代码：

```python
x = 10   # 全局变量

def func():
    global x
    x = 20   # 修改全局变量
    print("x inside function:", x)

func()
print("x outside function:", x)
```

在上述代码中,我们使用了`global`关键字来声明变量`x`是全局变量,这样在函数内部修改`x`时就会修改全局变量的值。因此,在函数内部和外部打印的结果都是`x: 20`。

需要注意的是,使用`global`关键字可能会导致代码变得难以维护,尤其是在涉及多个模块或线程时。如果可以避免使用全局变量,则最好不要使用`global`关键字。

## 迭代器生成器
### 迭代的概念
迭代属性是Python一大特性，也才允许我们通过`for in`循环体遍历比如列表、字典等集合类型数据类型内的数据，或者用in成员函数判断某元素是否在某数据内存在、使用列表解析式等，让代码变得简洁明晰，如果想深入理解Python这一大特性，其实还需要深入了解迭代器和生成器的概念。

以下先整体介绍可迭代、迭代器、生成器的概念和相互之间的关系

1. **可迭代**：指实现了Python迭代协议，可以通过`for in `循环体遍历的对象，比如list、dict等内置数据类型、迭代器、生成器
2. **迭代器**：指可以记住自己遍历位置的对象，直观体现便是可以使用`next()函数`返回值，迭代器只能往前，不能往后，当遍历完毕后，next(iteror)会抛出一个`StopIteration`异常
3. **生成器**：指使用`yield`的函数，生成器也是只能往前，不能往后，当遍历完毕后，`next(iteror)`会抛出一个`StopIteration`异常
4. 三个概念的包含关系：==可迭代＞迭代器＞生成器==
5. 迭代器和生成器，均可以通过`next(obj)`的方式不断返回下一个值
6. 可迭代的对象（包括生成器），均可以通过`iter(obj)`，转化为迭代器

**判断对象是否可迭代方法**
python也提供了判断是否可迭代的方法，即`isinstance`，代码如下

```python
from collections import Iterable
from collections import Iterator
list1=[1,2,3]
print(isinstance(list1,Iterable)) #返回True
print(isinstance(list1,Iterator)) #返回False
```
**迭代器和生成器的比较**
1. 迭代器是个类，且需要实现`__iter__`和`__next__`魔法函数，语法相对来说较为冗余
2. 生成器是个使用`yield`的函数，相较而言，代码会更加少
3. 在同一代码内，生成器只能遍历一次

for in循环的原理
```python
#1、只实现__getitem__
class A:
    def __init__(self):
        self.data=[1,2,3]
 
    def __getitem__(self,index):
        return self.data[index]
a=A()
for i in a:
    print(i)
#输出为 1、2、3
 
#2、实现__getitem__和__iter__
class A:
    def __init__(self):
        self.data=[1,2,3]
        self.data1=[4,5,6]
    
    def __iter__(self):
        return iter(self.data1)    
 
    def __getitem__(self,index):
        return self.data[index]
 
a=A()
for i in a:
    print(i)
#输出为 4、5、6
```

1. 如以上代码所示，如果只是实现`__getitem__`，`for in` 循环体会自动调用`__getitem__`函数，并自动对Index从0开始自增，并将对应索引位置的值赋值给 i，直到引发IndexError错误
2. 如果还实现了`__iter__`，则会忽略`__getitem__`，只调用`__iter__`，并对`__iter__`返回的迭代器进行成员遍历，并自动将遍历的成员逐次赋值给 i，直到引发`StopIteration`
### 可迭代对象
如何创建一个可迭代对象，核心点：

1.  关键是在定义类的时候，需要实现`__iter__`魔法函数，该函数返回一个迭代器即可
2.  实现了`__iter__`，也即实现了Python的迭代协议

```python
class Myiter:
    def __init__(self):
        self.a=1
    def __iter__(self):
        a=[1,2,3,4]
        return iter(a)
#此种实现的方式，不是一个迭代器，但是可迭代，即可通过for in 循环体进行遍历
it=Myiter()
for i in it:
    print(it)
```

**可迭代对象原理讲解**
1. 上面的代码，只是实现了`__iter__`魔法函数，但是已经可以在`for in` 循环体内进行遍历
2. 此时，因为没有实现`__next__`模范函数，所以只是可迭代对象，但并不是迭代器
3. 比如list数据类型，是可迭代对象，但并不是迭代器，可以观察list数据类型魔法函数，使用`dir(list)`，其输出中有`__iter__`魔法函数，但并没有`__next__`魔法函数
### 迭代器
其本质是一个实现了支持`iter()`和`next()`方法的对象，所以，如果想创建一个迭代器，则需要定义一个类，并在该类中实现`__iter__`和`__next__`魔法函数

1. __iter__必须返回一个迭代器
2. __next__实现数值推演算法
```python
class Myiter:
    #一般在初始时，传入或者初始化一些实例变量值，便于在__next__中使用
    def __init__(self):
        self.a=1
    #该函数必须返回一个迭代器
    def __iter__(self):
        return self
    #该值返回每次next调用，需要返回的下一个值，其实也就是实现数值推演算法
    def __next__(self):
        self.a+=1
        return self.a
#下面实例化一个迭代器
it=Myiter()
```
 
### 生成器
#### yield

`yield` 是 Python 中用于定义生成器函数的关键字。

生成器函数和普通函数的不同是，它们在执行时不会一次性返回所有结果。相反，每次调用生成器函数时，它会<font color=#ff0000>返回一个值并暂停执行函数</font>。当下一次再次调用生成器函数时，函数会从它<font color=#ff0000>上次暂停的地方继续执行</font>，并且返回下一个值。这种方式可以在需要逐个获取大数据量的时候，避免将所有数据都一次性加载到内存中造成性能问题。

使用 `yield` 关键字可以将一个函数转换为生成器函数。在生成器函数内部，`yield` 语句用来返回一个值并暂停函数执行。在下一次调用生成器函数时，函数会从上一次暂停的位置继续执行，直到再次遇到 `yield` 语句，然后再次返回一个值并暂停函数执行。这样交替进行，直到函数执行完毕或者遇到了 `return` 语句结束函数执行。

下面是一个简单的例子，演示如何使用 `yield` 定义一个生成器函数：

```python
def my_generator():
    yield 1
    yield 2
    yield 3

gen = my_generator()

print(next(gen)) # 1
print(next(gen)) # 2
print(next(gen)) # 3
```

在这个例子中，`my_generator()` 函数使用了三个 `yield` 语句，每次调用生成器函数会返回一个值并暂停执行直到下一次调用。最后，通过 `next()` 函数来获取每个值。

注意，当生成器函数执行完毕后再次调用 `next()` 函数会抛出 `StopIteration` 异常，表示所有结果都已经生成完成。在实际应用中，可以使用 `for` 循环迭代获取所有的结果，也可以使用 `yield from` 语句将另一个生成器内部的值透明地传递给外层生成器。
在 Python 中，生成器是一种特殊的迭代器，它可以用来生成一个值序列。与常规函数不同，生成器只会在需要生成值时才计算它们，这样可以节省大量的系统资源。

#### 生成器
通常情况下，我们使用 `yield` 语句来定义生成器。这个 `yield` 语句就像一个 `return` 语句，不同之处在于它记住了函数的状态。每次执行 `yield` 语句时，程序都会暂停并将结果返回给调用方。下一次从程序离开的地方继续执行时，生成器的状态被恢复，以进行下一次计算。

以下是一个简单的例子，演示如何使用生成器来生成斐波那契数列：

```python
def fibonacci(n):
    a, b = 0, 1

    for i in range(n):
        yield a
        a, b = b, a + b

# 输出前十个斐波那契数列
for num in fibonacci(10):
    print(num)
```

在这个例子中，我们定义了一个名为 `fibonacci` 的生成器对象，并使用 `yield` 语句返回斐波那契数列的每个值。然后，我们通过循环遍历前十个斐波那契数并输出它们。

需要注意的是，当使用生成器时，我们并不需要一开始就生成所有的值，而是可以按需生成。这种方式可以节省系统资源，并且使程序更加高效。

# 引用
## 引用介绍

引用就是变量<font color=#ff0000>指向数据存储空间的现象</font>,使用id(数据/变量)操作获取数据存储的内存空间<font color=#ff0000>引用地址</font>
引用
在python中,值是靠引用来传递来的。
我们可以用**id()来判断两个变量是否为同一个值的引用**。我们可以将id值理解为那块内存的地址标示。

```python
>>> a = 1
>>> b = a
>>> id(a)
13033816
>>>id(b) #注意两个变量的id值相同13033816
>>> a = 2
>>> id(a)
13033792
>>> id(b) #b的id值依旧
13033816
>>> a = [1,2]
>>> b = a
>>> id(a)
139935018544808
>>> id(b)
139935018544808
>>> a.append(3)
>>> a
[1,2,3]
>>> id(a)
139935018544808
>>> id(b) #注意a与b始终指向同一个地址
139935018544808
```

![image.png](https://article.biliimg.com/bfs/article/42a6411db112e0919e590b28d528d71261bfc97e.png)

**总结:**
- 之前为了更好的理解变量,咱们可以把a=100理解为变量a中存放了100,事实上变量a存储是100的引用(可理解为在内存中的一个编号)

## 引用当做实参
在Python中,函数参数传递是一种复合的机制,它既有引用传递也有值传递,具体取决于这个参数的类型和使用方式。

对于<font color=#ff0000>不可变类型的参数</font>（如数字、字符串、元组）,实际上是采用==值传递==的方式。这意味着,在函数内对这些参数进行修改并不会影响到函数外部相应变量的值,因为它们在函数内部被看作是新的局部变量,而函数外部的原始变量并没有改变。

例如：

```python
def func1(a):
    a = 20

b = 10
func1(b)
print(b) # 输出结果为 10
```

在这个例子中,我们定义了一个名为 `func1` 的函数,并将一个整型参数 `a` 传给它。在函数内部,我们将参数 `a` 的值修改为了 20。但是在函数调用结束后,我们打印出了原始变量 `b` 的值,它仍然是 10,没有被修改。

但是对于<font color=#ff0000>可变类型的参数</font>（如列表、字典等）,实际上是采用==引用传递==的方式。也就是说,在函数内对这些参数进行的修改会直接反映到函数外部相应对象的值上。

例如：

```python
def func2(lst):
    lst.append(4)

my_list = [1, 2, 3]
func2(my_list)
print(my_list) # 输出结果为 [1, 2, 3, 4]
```

在这个例子中,我们定义了一个名为 `func2` 的函数,并将一个包含三个元素的列表传给它。在函数内部,我们将参数 `lst`（即原始变量 `my_list` 的引用）的值修改为了包含四个元素的列表 [1, 2, 3, 4]。由于此处的参数 `lst` 是一个引用,因此对它的修改实际上也修改了函数外部的原始变量 `my_list`。

因此,可以说 Python 中的函数参数既不是纯粹的值传递,也不是纯粹的引用传递,而是一种<font color=#ff0000>复合的机制</font>,具体取决于参数的类型和使用方式。

# lambda
## lambda介绍
**lambda的应用场景**
如果一个函数<font color=#ff0000>有一个返回值</font>,并且只有<font color=#ff0000>一句代码</font>,可以使用lambda简化。
**lambda语法**
```python
lambda 参数列表︰表达式
```
>注意
lambda表达式的参数可有可无,函数的参数在lambda表达式中完全适用。
lambda表达式能接收任何数量的参数但只能返回一个表达式的值。

快速入门
```python
#函数
def fn1():
	return 200
print(fn1)
print(fn1())
#lambda表达式
fn2 = lambda: 100
print(fn2)
print(fn2())
```

> 注意:直接打印lambda表达式,输出的是此lambda的内存地址

## lambda应用
**lambda实现**
```python
fn1 = lambda a,b:a + b
print(fn1(1,2))
```
### lambda的参数形式
1.无参数
```python
fn1 = lambda : 100
print(fn1())
```
2.一个参数
```python
fn1 = lambdaa : a
print(fn1('hello world'))
```
3.默认参数
```python
fn1 = lambda a , b, c=100 : a + b + c
print(fn1(10,20))
```
4.可变参数: `*args`
```python
fn1 = lambda *args: args
print(fn1(10,20,30))
```
5.可变参数:``**kwargs``
```python
fn1 = lambda **kwargs: kwargs
print(fn1(name='python', age=20))
```


### 带判断的lambda
```python
fn1 = lambda a, b: a if a > b else b
print(fn1(1000,500))
```

### lambda控制自定义排序
**列表数据按字典key的值排序**
```python
students = [
{ 'name':'TOM', 'age ':20} ,{ 'name': 'ROSE' , 'age': 19} ,{ 'name ':'Jack', 'age': 22}]
#按name值升序排列
students.sort(key=lambda x: x['name'])
print(students)
#按name值降序排列
students.sort(key=lambda x: x['name'],reverse=True)
print(students)
#按age值升序排列
students.sort(key=lambda x:x['age'])
print(students)
```

**自定义排序**
```python
# 自定义排序,要求现在长度降序,再按字母降序,怎么用lambda
a=["aab","dsfsdf",'dfs']
a.sort(key=lambda x:(len(x),x),reverse=True) #先按长度怕排,再按字母
print(a) # a=["fab","dsfsdf",'dfs']
```

# 文件操作

## 文件的基本操作

### 打开文件
在python,使用open函数,可以打开一个已经存在的文件,或者创建一个新文件
语法如下:
```python
open(name,mode)
```
- name:是要打开的目标文件名的<font color=#ff0000>字符串</font>(可以包含文件所在的具体路径)。
- mode:设置打开文件的模式(访问模式)︰只读、写入、追加等。
示例如下：
```python
f = open( 'test.txt ', 'w')
```
说明:

当使用Python打开一个文件时,你需要指定所需的文件模式。以下是Python中可用的不同文件模式：

|模式|描述|
|:-:|:-:|
| `r` | 以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。 |
| `w` | 打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在,创建新文件。 |
| `a` |打开一个文件用于追加。如果该文件已存在,文件指针将会放在文件的结尾。也就是说,新的内容将会被写入到已有内容之后。如果该文件不存在,创建新文件进行写入。|
| `x` | 写模式,新建一个文件,如果该文件存在则会报错。 |
| `b` | 以二进制模式打开文件。一般用于非文本文件如图片、视频等。 |
| `t` | 以文本模式打开文件。这是默认模式,在文本模式下,会自动将`\r\n`转换为`\n`,并忽略平台差异。 |
| `+` | 可读写模式（可添加到其他模式中使用）,可以同时读取和写入文件。 |

`\r`和`\r\n`是回车符和换行符的表示方式,它们都属于不同平台中的文本换行符规范。

`\r`通常用于Mac OS 9及之前的版本。在早期的苹果操作系统中,每次按下回车键时,会输出一个回车符(`\r`)；

`\n`通常用于Linux/Unix操作系统。而在Windows操作系统中,则使用两个字符`\r\n`,即回车符和换行符。

Python中的文本模式,在读取文件时会自动将`\r\n`转换为`\n`。也就是说,如果在Windows下编写的文件,可以在Python中以文本模式打开并正确处理。

### 关闭文件
`close()`
示例如下:
```python
#新建一个文件,文件名为: test.txt
f = open('test.txt', 'w')
#关闭这个文件
f.close()
```
### 文件的读写
#### 写数据(write)
使用`write()`可以完成向文件写入数据
demo:新建一个文件 `file_write_test.py` ,向其中写入如下代码:
```python
f = open('test.txt', 'w ')
f.write( 'hello world, i am here! ' )
f.close()
```
运行之后会在 file_write_test.py文件所在的路径中创建一个文件test.txt ,其中数据如下:
![image.png](https://article.biliimg.com/bfs/article/74154f93ac8cfd9b111b8e53a303673023338451.png)

> 注意︰
> 1. `w`和`a`模式:如果文件不存在则创建该文件;如果文件存在,w模式先清空再写入,a模式直接末尾追加。
> 2. `r`模式:如果文件不存在则报错。

#### 读数据(read)
使用read(num)可以从文件中读取数据,num表示要从文件中读取的**数据的长度**（单位是字节),如果没有传入num,那么就表示读取文件中所有的数据
demo:新建一个文件`file_read_test.py` ,向其中写入如下代码:
```python
f = open('test.txt', 'r')
content = f.read(5) #最多读取5个数据
print(content)
print("—*30) #分割线,用来测试
content = f.read() #从上次读取的位置继续读取剩下的所有的数据
print(content)
f.close() #关闭文件,这个可以是个好习惯哦
```
#### 读数据(readlines)
就像read没有参数时一样,readlines可以<font color=#ff0000>按照行的方式</font>把整个文件中的内容进行一次性读取,并且返回的是一个列表,其中每一行的数据为一个元素
```python
#coding=utf-8
f = open( 'test.txt', 'r')
content = f.readlines()
print(type(content))

i=1
for temp in content:
	print("%d:%s" %(i, temp) )
	i += 1
f.close()
```
同理`readline()`⼀次读取⼀⾏内容。

### 文件的定位读写
#### 获取当前读写的位置
在读写文件的过程中,如果想知道<font color=#ff0000>当前的位置</font>,可以使用`tell()`来获取
```python
#打开一个已经存在的文件
f = open("./python/test.txt", "r" )
str = f.read(3)
print("读取的数据是%s:" %str)
#查找当前位置
position = f.tell()
print("当前文件位置%s: " % position)
str = f.read (3)
print("读取的数据是%s∶" % str)
#查找当前位置
position = f.tell()
print("当前文件位置%s:" % position)
f.close()
```

#### 定位到某个位置
如果在读写文件的过程中,需要从另外一个位置进行操作的话,可以使用`seek()`
seek(offset,from)有2个参数
- offset:偏移量
- from:方向
	- 0:表示文件开头。 
	- 1:表示当前位置。
	- 2:表示文件末尾
	demo:把位置设置为:从文件开头,偏移5个字节
```python
#打开一个已经存在的文件
f = open("test.txt", "r" )
str = f.read (30)
print("读取的数据是:" % str)
# 查找当前位置
position = f.tell()
print("当前文件位置: " % position)

#重新设置位置
f.seek(5,0)

#查找当前位位置
position = f.tell()
print("当前文件位置:" %position)

f.close()
```



## 文件和文件夹的操作
**文件的相关操作**
有些时候,需要对文件进行重命名、删除等一些操作,python的os模块中都有这么功能
1. 文件重命名
os模块中的rename()可以完成对文件的重命名操作
rename(需要修改的文件名,新的文件名)
```python
import os
os.rename("毕业论文.txt","毕业论文-最终版.t×t" )
```
2. 删除文件
os模块中的remove()可以完成对文件的删除操作
remove(待删除的文件名)
```python
import os
os.remove("毕业论文.txt")
```
3. 创建文件夹
```python
import os
os.mkdir("张三")
```
4. 获取当前目录
```python
import os
os.getcwd()
```
5. 改变默认目录
```python
import os
os.chdir("..")
```
6. 获取目录列表
```python
import os
os.listdir("./")
```
7.删除文件夹
```python
import os
os.rmdir("张三")
```

8. os.path.join()函数

`os.path.join()`是Python中用于连接路径的函数。它可以将多个路径组合在一起,并返回一个新的路径字符串。

具体来说,`os.path.join()`会根据操作系统的不同,在多个路径字符串之间添加路径分隔符,并返回一个新的路径字符串。例如,在Unix/Linux系统下使用`os.path.join('usr', 'bin', 'python')`,函数将返回`'usr/bin/python'`；而在Windows系统下使用`os.path.join('C:\\', 'Program Files', 'Python')`,函数将返回`'C:\\Program Files\\Python'`。

使用`os.path.join()`的好处是,它会自动处理路径分隔符的差异问题,因此无需手动编写跨平台的代码。此外,由于使用了`os.path.join()`,代码也更加可读和易于维护,因为它可以明确地指出要连接的路径。

下面是一个示例代码：

```python
import os

path1 = 'usr'
path2 = 'bin'
path3 = 'python'

full_path = os.path.join(path1, path2, path3)
print(full_path)  # Unix/Linux: usr/bin/python  Windows: usr\bin\python
```

在上述示例中,首先导入了Python的`os`模块,然后定义了三个要连接的路径（在Unix/Linux下使用的路径分隔符为`/`,在Windows下使用的路径分隔符为`\`）。接着,使用`os.path.join()`函数将这些路径组合成一个完整的路径,并将其打印出来。在不同的操作系统上运行时,输出结果会自动适应对应的路径分隔符。

# 面向对象
当我们使用Python编程时,面向对象编程（OOP）是一种非常重要的编程范式。 在Python中,面向对象编程可以通过类和对象来实现。

## 类(Class)



### 构造函数`__init__()`

类是一种用户自定义数据类型,它描述了数据的属性和操作的数据类型。类可以看作是一种模板或蓝图,用于创建对象的具体实例。在Python中,类可以有构造函数(`__init__()`方法)、属性和方法。

以下是一个示例类定义：

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def introduce(self):
        print("My name is " + self.name + " and my age is " + str(self.age) + ".")
```

在上面的代码中,`Person`类有一个构造函数`__init__()`和一个名为`introduce()`的方法。 `self`是一个特殊的参数,它用于表示类的实例本身。`name`和`age`是初始化类实例时传递的两个参数,它们成为类的属性。

**对象(Object)**

在Python中,一个类的实例称为该类的对象。我们可以通过在类的名称后使用括号来创建一个对象,并将其分配给一个变量。例如,上面的示例中我们可以创建多个属于`Person`类的对象,如下所示：

```python
person1 = Person("Alice", 25)
person2 = Person("Bob", 30)
```

上述代码首先创建了两个Person类的对象person1和person2。 每个对象都有自己的属性值,可以通过对象变量名调用其属性值,例如：

```python
print(person1.name)
print(person2.age)
```

以上代码将输出`Alice`和`30`。

我们还可以使用对象来调用该类中定义的方法,例如：

```python
person1.introduce()   # Output: My name is Alice and my age is 25.
person2.introduce()   # Output: My name is Bob and my age is 30.
```

在上面的代码中,我们使用对象person1和person2来调用`introduce()`方法,输出该实例的属性值。

###  `__str__()`

`__str__()` 是 Python 中一种特殊方法,用于自定义对象的打印字符串形式。当一个对象被传递给 `print()` 函数时,Python 解释器会自动调用该对象的 `__str__()` 方法并将其返回值作为打印输出。

例如,假设我们有如下的 `Person` 类：

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return f"{self.name} is {self.age} years old."
```

在上面的代码中,我们定义了一个 `Person` 类,并重写了它的 `__str__()` 方法。该方法返回一个格式化的字符串,其中包含该人的姓名和年龄。

现在,我们可以创建几个 `Person` 对象,并将它们作为参数传递给 `print()` 函数,如下所示：

```python
person1 = Person("Alice", 25)
person2 = Person("Bob", 30)

print(person1)   # Output: Alice is 25 years old.
print(person2)   # Output: Bob is 30 years old.
```

在上面的代码中,我们首先创建了两个不同的 `Person` 对象,并分别赋值给变量 `person1` 和 `person2`。然后,我们使用 `print()` 函数分别打印这两个对象。由于我们已经重写了 `Person` 类的 `__str__()` 方法,因此 `print()` 函数将分别输出 `Alice is 25 years old.` 和 `Bob is 30 years old.`。

如果一个类没有定义 `__str__()` 方法,那么当我们尝试打印该类的对象时,Python 解释器会默认调用该类的父类 `object` 的 `__str__()` 方法,并返回一个格式为 `<classname object at memory_address>` 的字符串,其中 `<classname>` 是该类的名称,`memory_address` 是该对象在内存中的地址。

例如,假设我们有如下的 `Person` 类：

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

现在,我们可以创建一个 `Person` 对象并将其打印输出：

```python
person = Person("Alice", 25)
print(person)   # Output: <__main__.Person object at memory_address>
```

由于 `Person` 类没有定义 `__str__()` 方法,因此 Python 解释器会调用其父类 `object` 的 `__str__()` 方法,并返回一个以 `<classname object at memory_address>` 格式表示的字符串。其中,`<classname>` 是该类的名称,`memory_address` 是指向该对象在内存中的地址的数字。这个字符串并不是特别有用,也不包含有关对象本身的实际信息。

###  `__del__`
`__del__()` 方法是 Python 中的一个特殊方法,用于在对象被销毁（即释放其占用的内存）之前执行一些必要的清理工作。该方法与 `__init__()` 方法类似,但是是在对象生命周期的结束阶段调用,而不是在创建对象时调用。

当我们使用 `del` 关键字删除对象时,Python 解释器会自动调用对象的 `__del__()` 方法来完成对象的清理操作。请注意,我们不能手动调用 `__del__()` 方法来销毁对象,因为它是由解释器自动调用的。

以下是一个简单的示例,说明如何使用 `__del__()` 方法：

```python
class MyClass:
    def __init__(self, name):
        self.name = name
        print(f"{self.name} has been created.")
        
    def __del__(self):
        print(f"{self.name} is being destroyed.")

# 创建一个对象
obj = MyClass("My Object")

# 手动删除对象
del obj
```

在上面的示例中,我们定义了一个名为 `MyClass` 的类,并在其中定义了 `__init__()` 和 `__del__()` 两个方法。在 `__init__()` 方法中,我们创建了一个新的对象并打印一条消息来验证对象已经成功创建。在 `__del__()` 方法中,我们打印一条消息来说明该对象正在被销毁。

然后,我们创建了一个 `MyClass` 对象并将其引用赋给变量 `obj`。接着,我们使用 `del` 关键字手动删除对象。这时,Python 解释器会自动调用该对象的 `__del__()` 方法,并打印一条消息来说明该对象正在被销毁。

需要注意的是,在实际编程中,我们并不总是需要使用 `__del__()` 方法。在大多数情况下,Python 的垃圾回收机制会自动管理对象的内存分配和释放,而无需手动干预。如果确实需要进行清理操作,则建议使用 `with` 语句或上下文管理器来完成。

### `__call__`

`__call__`是Python中一个特殊的方法,它可以让对象像函数一样被调用。如果一个类中定义了`__call__`方法,那么该类的实例对象就可以像函数一样被调用。

下面是一个使用`__call__`方法的例子：

```python
class MyClass:
    def __init__(self):
        pass

    def __call__(self, *args, **kwargs):
        print("__call__ method is called.")

obj = MyClass()

# 调用 __call__ 方法
obj()
```

在这个例子中,我们定义了一个名为MyClass的类,并且在其中定义了一个`__call__`方法。当我们创建该类的实例对象并像调用函数一样调用它时,其内部实际上会调用`__call__`方法,并输出一条信息。

使用`__call__`方法可以使得一个类产生像函数一样的行为,以便更加方便地执行某些操作。比如,我们可以在一个类中定义需要重复执行的操作,然后通过调用实例对象来实现这些操作,在某些情况下可以极大地简化代码。

`callable()`是Python内置函数之一,用于判断一个对象是否可调用。通俗地讲,就是用来检查某个对象是否可以像函数一样被调用。

使用方法如下：

```python
callable(object)
```

其中,`object`表示要检查的对象。

如果参数`object`是可调用的对象（如函数、类、实现了`__call__`方法的对象）,返回`True`,否则返回`False`。

下面是几个示例：

```python
def foo():
    pass

class Bar:
    def __call__(self):
        pass

print(callable(foo)) # True
print(callable(Bar())) # True
x = 5
print(callable(x)) # False
```

###  `__getitem__()`
`__getitem__()`是Python中的魔法方法之一。它允许对象像序列（列表、元组等）或映射（字典）一样被索引,即使用对象名后跟方括号并提供索引的方式来获取对象的元素。

当我们在一个类中实现了`__getitem__()`方法时,就可以使用该类的实例对象进行索引操作。

下面是一个简单的示例代码：

```python
class MyList:
    def __init__(self, list_):
        self.list = list_

    def __getitem__(self, index):
        return self.list[index]
```

在这个示例中,我们定义了一个名为`MyList`的类,并在其中实现了`__getitem__()`方法。在`__init__()`方法中初始化实例变量`self.list`,其值为传入该类构造函数的参数`list_`。在`__getitem__()`方法中,通过调用列表的索引操作符`[]`来获取指定位置的元素。

下面展示了我们如何创建一个`MyList`对象并对其进行索引操作：

```python
my_list = MyList([1, 2, 3, 4, 5])
print(my_list[0]) # 1
print(my_list[3]) # 4
```

在上面的代码示例中,我们首先创建了一个`MyList`对象并将其初始化为包含整数`1-5`的列表。然后,通过`my_list[0]`和`my_list[3]`语句来获取该对象的第一个和第四个元素。由于我们在`MyList`类中实现了`__getitem__()`方法,因此这些操作可以成功执行。

总体来说,通过实现`__getitem__()`方法,我们可以使自己定义的类的实例对象能够像序列或字典对象一样进行索引,从而使代码更加灵活和易读。

### 其他魔法方法

好的,以下是常用的 Python 类的魔法方法及其解释：

|魔法方法|解释|
|:-:|:-:|
| `__init__(self[, ...])` | 初始化对象,在创建一个实例时自动调用。 |
| `__str__(self)` | 返回一个对象的描述信息,当使用 `print()` 函数打印对象时会被调用。 |
| `__repr__(self)` | 返回一个对象的字符串表示,通常可以通过 `eval(repr(obj))` 来重新创建该对象。 |
|`__len__(self)` | 返回一个对象的长度,通常与内置函数 `len()` 结合使用。 |
|`__getitem__(self, key)`| 定义获取序列中某个元素的行为,通常与索引操作符 `[]` 结合使用。 |
| `__setitem__(self, key, value)` | 定义设置序列中某个元素的行为,通常与赋值操作符 `=` 结合使用。 |
| `__getattr__(self, name)` | 定义当用户访问不存在的属性时的行为。 |
| `__setattr__(self, name, value)` | 定义当一个属性被设置时的行为。 |
| `__delattr__(self, name)` | 定义当一个属性被删除时的行为。 |
| `__call__(self[, args...])` | 定义当一个对象被调用时的行为。 |

## 继承

### 继承介绍以及单继承

Python中,继承是一种面向对象编程的重要概念。它允许我们定义一个新类（称为子类）,该类**继承了另一个现有类（称为父类）的所有属性和方法**,并可以根据需要<font color=#ff0000>添加或修改</font>其行为。

在Python中,实现继承只需要在定义子类时将父类放在圆括号中,并且新类将自动继承其所有属性和方法。这是一个简单的示例：

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def eat(self):
        print(f"{self.name} is eating.")

class Cat(Animal):
    def meow(self):
        print(f"{self.name} says meow.")
```

在上面的代码示例中,我们定义了两个类：`Animal`和`Cat`。`Cat`类是从`Animal`类派生而来的子类。因此,在`Cat`类中,我们不需要重新编写`__init__()`和`eat()`方法,因为它们已经在父类中定义并被继承了。同时,我们还添加了一个新的`meow()`方法,该方法只存在于子类中。

> 在Python中,所有类默认继承`object`类,object类是顶级类或基类;其他子类叫做<font color=#ff0000>派生类</font>

### 多继承

在Python中,多继承是指一个类可以从多个父类直接继承。这使得Python更加灵活,因为它允许程序员使用已经存在的代码来构建新的类。

在多继承中,如果一个方法同时出现在多个父类中,则按照<font color=#ff0000>MRO规则</font>来确定搜索该方法时的顺序。MRO算法通过一个复杂的算法来计算类的方法解析顺序,这确保了每个方法只被执行一次,并且不会被重复执行。

下面是一个多继承的示例：

```python
class Base1:
    def greet(self):
        print("Hello from Base1")

class Base2:
    def greet(self):
        print("Hello from Base2")

class MyClass(Base1, Base2):
    pass

obj = MyClass()
obj.greet()
```

在上面的代码示例中,我们定义了三个类：`Base1`、`Base2`和`MyClass`。`MyClass`继承自`Base1`和`Base2`类。当`greet()`方法在`MyClass`实例对象上调用时,它将按照MRO规则寻找适当的方法。由于`Base1`在`Base2`之前被列出,因此将优先使用`Base1`的`greet()`方法。因此,在上述示例中,输出将是“Hello from Base1”。

`MRO（Method Resolution Order）`规则是指在Python中确定继承链中的方法解析顺序的算法。MRO算法确保了每个方法只被执行一次,并且不会被重复执行。
在上述示例中,`pass`代表"什么也不做"。它通常用作<font color=#ff0000>占位符</font>,因为Python中<font color=#ff0000>不允许使用空代码块</font>。在这种情况下,我们将它放在`MyClass`类的定义中,表示`MyClass`类没有其他功能或属性。

### 子类调用和重写父类属性和方法
子类可以重写（覆盖）父类中的同名属性和方法。当子类重新定义一个具有与父类相同名称的属性或方法时,子类将继承该属性或方法,但它会覆盖父类中拥有同样名称的属性或方法。

如果子类希望使用父类的同名属性或方法,可以调用父类的该属性或方法。在Python中,我们可以通过 `super()` 函数来调用父类的方法,例如：

```python
class Parent:
    def some_method(self):
        print("This is a parent method")
        
class Child(Parent):
    def some_method(self):
        super().some_method()  # 调用父类同名方法
        print("This is a child method")
```

在上述示例中,`Child` 类重写了 `Parent` 中的 `some_method()` 方法,并在其中调用了 `super()` 函数来执行父类的同名方法。

如果子类重写了父类中的同名属性,则父类的属性值会被子类中的属性值所取代,而且子类不能像调用方法一样通过 `super()` 来调用父类的同名属性。

### 多层继承

多层继承是指创建一个类,它从另一个派生类派生而来,而第二个派生类又从第一个派生类的某个基类派生而来的过程。也就是说,通过多层继承,我们可以构建起一个复杂的继承关系,使得派生类可以访问到在多个父类中定义的属性和方法。

多层继承可以帮助我们更好地组织代码,并允许我们实现更为复杂和灵活的功能。例如,一些框架或库可能使用多层继承来实现数据结构或特定功能的抽象。

在Python中,多层继承可以按照以下方式实现：

```python
class Grandparent:
    def some_method(self):
        print("This is a method from Grandparent class")

class Parent(Grandparent):
    def some_method(self):
        super().some_method()
        print("This is a method from Parent class")

class Child(Parent):
    def some_method(self):
        super(Parent,self).some_method()
        print("This is a method from Child class")
```

在上述示例中,我们使用了 `super(Parent, self)` 来表示在 `Parent` 类中查找下一个方法,从而实现调用 `Grandparent` 类中的 `some_method` 方法。通过指定第一个参数为 `Parent` 类,可以让 `super()` 函数按照 `Parent-Child` 的顺序查找方法。

### 定义私有属性和方法
在Python中,可以通过在属性名或方法名前面添加两个下划线（`__`）来定义私有属性和方法。这意味着它们只能在类内部访问,而不能从类的外部进行访问。

以下是一个示例：

```python
class MyClass:
    def __init__(self):
        self.public_attribute = "Public Attribute" # 公共属性
        self.__private_attribute = "Private Attribute" # 私有属性

    def public_method(self):
        print("This is a public method.")

    def __private_method(self):
        print("This is a private method.")


instance = MyClass()

# 访问公共属性
print(instance.public_attribute)  # 输出: "Public Attribute"

# 试图访问私有属性
# This will result in an AttributeError
#print(instance.__private_attribute)

# 调用公共方法
instance.public_method()  # 输出: "This is a public method."

# 试图调用私有方法
# This will result in an AttributeError
#instance.__private_method()

```

在上述示例中,我们定义了一个名为 `MyClass` 的类,并在其中定义了一个公共属性和一个私有属性,以及一个公共方法和一个私有方法。使用双下划线定义的属性和方法都是类的私有成员,只能在类的内部访问。

## 多态
### 了解多态
多态指的是一类事物有多种形态,(一个抽象类有多个子类,因而多态的概念依赖于继承)。
- 定义:多态是一种使用对象的方式,子类重写父类方法,调用不同子类对象的相同父类方法,可以产生不同的执行结果
- 好处:调用灵活,有了多态,更容易编写出通用的代码,做出通用的编程,以适应需求的不断变化!
- 实现步骤︰
	- 定义父类,并提供公共方法。
	- 定义子类,并重写父类方法
	- 传递子类对象给调用者,可以看到不同子类执行效果不同

体验多态
```python
class Dog(object):
	def work(self):#父类提供统一的方法,哪怕是空方法
		print( '指哪打哪...')
class ArmyDog(Dog): #继承Dog类
	def work(self):#子类重写父类同名方法
		print( '追击敌人...' )
class DrugDog(Dog):
	def work(self):
		print( '追查毒品...' )
class Person(object):
	def work_with_dog (self,dog):#传入不同的对象,执行不同的
		dog.work()
ad = ArmyDog()
dd = DrugDog()
daqiu = Person()
daqiu.work_with_dog(ad)
daqiu.work_with_dog(dd)
```

## 类属性和实例属性
在Python中,类属性是属于类的变量,而实例属性是属于实例的变量。

### 类属性

在<font color=#ff0000>类中定义的变量</font>称为类属性。类属性与类的所有实例<font color=#ff0000>共享其值</font>。我们可以通过访问类名来访问类属性,也可以通过类的任何实例来访问它。以下是一个示例：

```python
class MyClass:
    class_attribute = "Class Attribute"

instance1 = MyClass()
instance2 = MyClass()

# 访问类属性
print(MyClass.class_attribute)  # 输出："Class Attribute"
print(instance1.class_attribute) # 输出："Class Attribute"
print(instance2.class_attribute) # 输出："Class Attribute"

# 修改类属性
MyClass.class_attribute = "New Class Attribute"
print(MyClass.class_attribute)  # 输出："New Class Attribute"
print(instance1.class_attribute) # 输出："New Class Attribute"
print(instance2.class_attribute) # 输出："New Class Attribute"
```

### 实例属性

实例属性是类的每个实例独有的变量。这意味着在不同的实例之间,它们的值可以不同。以下是一个示例：

```python
class MyClass:
    def __init__(self):
        self.instance_attribute = "Instance Attribute"

instance1 = MyClass()
instance2 = MyClass()

# 访问实例属性
print(instance1.instance_attribute) # 输出："Instance Attribute"
print(instance2.instance_attribute) # 输出："Instance Attribute"

# 修改实例属性
instance1.instance_attribute = "New Instance Attribute"
print(instance1.instance_attribute) # 输出："New Instance Attribute"
print(instance2.instance_attribute) # 输出："Instance Attribute"
```

**通过实例(对象)去修改类属性**

可以通过实例的名称（或变量）访问类属性,但若尝试在实例上直接更改类属性,则会在实例上<font color=#ff0000>创建新的同名实例属性</font>,而不是更改类属性。 

我们可以使用 `MyClass.class_attribute = "new value"` 的方式直接更改类属性。例如：

```python
class MyClass:
    class_attribute = "Class Attribute"

instance1 = MyClass()

# 直接访问类属性并进行修改
MyClass.class_attribute = "New Class Attribute"
print(MyClass.class_attribute)  # 输出："New Class Attribute"
print(instance1.class_attribute) # 输出："New Class Attribute"
```

总之,类属性是类共享的变量,实例属性是每个实例私有的变量,可以通过实例访问类属性,但不能直接通过实例更改它。为了改变类属性的值,应该直接使用类名称进行赋值操作,而不是使用实例变量。

## 静态⽅法和类⽅法

在Python中,静态方法和类方法是两种特殊的方法类型。

### 静态方法

静态方法是不需要访问实例或类的方法,它们是独立于类和实例的。静态方法使用 `@staticmethod` 装饰器进行装饰。在静态方法内部,不能直接访问实例属性或者类属性。以下是一个示例：

``` python
class MyClass:
    class_attribute = "Class Attribute"
  
    @staticmethod
    def my_static_method():
        print("This is a static method")

MyClass.my_static_method()  # 直接通过类名调用静态方法
```

在上述示例中,我们定义了一个静态方法 `my_static_method()` 。它没有任何参数,并且我们可以通过类名称来直接调用该方法。

### 类方法

类方法是需要访问类变量或操作类的方法,但不需要访问实例变量或操作实例的方法。在定义时需要使用 `@classmethod` 装饰器进行装饰。类方法第一个参数通常被命名为 `cls`,表示该方法的类本身。我们可以通过 `cls` 来访问类变量。以下是一个示例：

```python
class MyClass:
    class_attribute = "Class Attribute"

    @classmethod
    def my_class_method(cls):
        print("Class attribute:", cls.class_attribute)

MyClass.my_class_method()  # 直接通过类名调用类方法
```

在上述示例中,我们定义了一个类方法 `my_class_method()` ,该方法没有任何参数。我们可以通过类名称来直接调用该方法,并且在类方法内部可以使用 `cls` 访问类变量。

总之,静态方法是独立于实例和类的方法,而类方法则通常与类变量一起使用。使用静态方法和类方法可以使代码更清晰易懂,也可以让我们更好地组织和管理代码。

## 类型注解
### 变量的类型注解
**类型注解**
Python在3.5版本的时候引入了类型注解,以方便静态类型检查工具,IDE等第三方工具。
类型注解:**在代码中涉及数据交互的地方,提供数据类型的注解**（显式的说明)。
主要功能:
- 帮助第三方IDE工具(如PyCharm)对代码进行类型推断,协助做代码提示·帮助开发者自身对变量进行类型注释
支持:
- 变量的类型注解
- 函数(方法)形参列表和返回值的类型注解

类型注解的语法
为变量设置类型注解
基础语法: <font color=#ff0000>变量:类型</font>
**基础数据类型注解**

```python
var_1: int = 10
var_2: float = 3.1415926
var_3: boo1 = True
var_4: str = "itheima"
```

类对象类型注解
```python
class student:
	pass
stu : student = student()
```

**基础容器类型注解**
容器类型详细注解
```python
my_1ist: list = [1,2,3]
my_tuple: tuple[str,int,boo1] = ("itheima",666,True)
my_set: set[int] = {1,2,3}
my_dict: dict[str,int] = {"itheima" :666}
my_str: str = "itheima"
```

```python
my_list: list[int] = [1,2,3]
my_tuple: tuple = (1,2,3)
my_set: set = {1,2,3}
my_dict: dict[str, int] = {"itheima":666}
```

注意:
- 元组类型设置类型详细注解,需要将每一个元素都标记出来。
- 字典类型设置类型详细注解,需要2个类型,第一个是key第二个是value

除了使用变量:类型,这种语法做注解外,也可以在注释中进行类型注解。语法:
`# type:类型`
**在注释中进行类型注解**
```python
class student:
	pass
var_1 = random .randint(1,10) # type: int
var_2 = json.1oads(data) # type: dict[str, int]
var_3 = func() # type: student
```

### 函数(方法)的类型注解
函数和方法的形参类型注解语法:
```python
def 函数方法名(形参名:类型,形参名:类型,......):
	pass
```

![image.png](https://article.biliimg.com/bfs/article/159bd314d0b0cedb1d1c11feb32c77355821793a.png)

**函数(方法)的类型注解–返回值注解**
同时,函数（方法)的返回值也是可以添加类型注解的。
语法如下:
```python
def函数方法名(形参:类型.....,形参:类型)->返回值类型:
	pass
```

```python
def add(x:int,y:int) -> int:
	return x+y
```

```python
def func(data: list[int]) -> list[int]:
	pass
```

### Union类型
Union联合类型注解,在变量注解、函数(方法）形参和返回值注解中,均可使用。
```python
my_list: list[union(int,str)] = [1,2,"itcast", "itheima"]
my_dict: dict[str,union[str,int]] ={ "name": "周杰轮","age": 31}
def func(data:union[int,str]) -> union[int,str] :
	pass
```

# 异常和模块
## 了解异常
1. 什么是异常:
异常就是程序运行的过程中出现了错误
2.bug是什么意思:
bug就是指异常的意思,因为历史因为小虫子导致计算机失灵的案例,所以延续至今,bug就代表软件出现错误。
## 异常分类
Python中有很多异常类型,它们可以帮助我们更好地处理程序运行时可能出现的问题。在Python中,所有异常都是 `Exception` 类或其子类的实例。下面是Python中一些常见的异常分类和示例：

| 异常类型              | 描述                                                         |示例|
|:-:|:-:|:-:|
|`SyntaxError`| 语法错误                                                     | `print("Hello, world!)`                                    |
|`NameError`| 找不到变量                                                   | `print(x)`                                                 |
|`TypeError`| 不同类型之间的无效操作                                       | `1 + "2"`                                                  |
|`ZeroDivisionError`| 除数为零                                                     | `10 / 0`                                                   |
|`ValueError`| 参数不合法                                                   | `int("a")`                                                 |
|`IndexError`| 索引超出范围                                                 | `lst = [1, 2, 3]; lst[4]`                                  |
|`KeyError`| 字典中找不到键                                               | `d = {"a": 1}; d["b"]`                                      |
|`FileNotFoundError`| 找不到指定文件                                               | `with open("abc.txt", "r") as file: print(file.read())`    |
|`Exception`| 所有异常的基类                                               |                                                          |

## 异常的捕获方法
### 捕获常规异常
基本语法:
```python
try:
	可能发生错误的代码
except:
	如果出现异常执行的代码
```
快速入门
需求:尝试以`r`模式打开文件,如果文件不存在,则以`w`方式打开。
```python
try:
	f = open('linux.txt', 't')
except:
	f = open('linux.txt', 'w')
```

### 捕获指定异常
基本语法:
```python
try:
	print(name)
except NameError as e:
	print('name变量名称未定义错误')
```

**注意事项**
1. 如果尝试执行的代码的异常类型和要捕获的异常类型不一致,则无法捕获异常。
2. 一般try下方只放一行尝试执行的代码。

### 捕获多个异常
当捕获多个异常时,可以把要捕获的异常类型的名字,放到`except`后,并使用元组的方式进行书写。
```python
try:
	print(1/0)
except (NameError,ZeroDivisionError):
	print('ZeroDivision错误...")
```

### 捕获异常并输出描述信息
基本语法:
```python
try:
	print(num)
except (NameError, ZeroDivisionError) as e:
	print(e)
```

### 异常else
else表示的是如果没有异常要执行的代码。
```python
try:
	print(1)
except Exception as e:
	print(e)
else:
	print('我是else,是没有异常的时候执行的代码')
```

### 异常的finally
finally表示的是无论是否异常都要执行的代码,例如关闭文件。
```python
try:
	f = open('test.txt','r')
except Exception as e:
	f = open('test.txt', 'w')
else:
	print('没有异常,真开心')
finally:
	f.close()
```

### 手动抛出异常raise
在Python中，`raise` 是一种错误处理机制。它用于手动抛出异常并指定要抛出异常的类型和描述信息。

在代码执行时，当遇到某些情况需要停止执行并通知调用者时，使用 `raise` 可以抛出一个异常。可以使用 raise 抛出 Python 中的任何异常类型（如 ValueError、TypeError、NameError 等），也可以自定义异常类型，并将异常实例作为参数传递给 raise 语句。

结合 try-except 语句一起使用，可以捕获并处理异常，使程序能够更加健壮和灵活地处理错误情况。

例如：

```python
# 自定义异常类
class MyError(Exception):
    pass

# 抛出异常
x = -1
if x < 0:
    raise MyError("x不能是负数")
```

此时会抛出一个 MyError 类型的异常，并且终止程序的运行。
## 异常的传递

**异常的传递**
异常是具有传递性的
当<font color=#ff0000>函数func01</font>中发生异常,并且没有捕获处理这个异常的时候,异常会传递到<font color=#ff0000>函数func02</font>,当func02也没有捕获处理这个异常的时候
<font color=#ff0000>main函数</font>会捕获这个异常,这就是<font color=#ff0000>异常的传递性</font>.
<img src="https://article.biliimg.com/bfs/article/bffd2f6b489226f53417ea8c30ba5ee1cf48dd88.png" alt="image.png" style="zoom:33%;" />

提示:
	<font color=#ff0000>当所有函数都没有捕获异常的时候,程序就会报错</font>
![image.png](https://article.biliimg.com/bfs/article/c13bdb1c035e865b89a2cd2e58e64dfec7d801c2.png)


## 模块的概念

在Python中，模块是一种组织代码的方式。它允许将相关的代码放在一个独立的文件中，并通过导入这个文件来使用其中定义的函数、类和变量。

模块可以包含可执行的代码，也可以只包含函数和类的定义。Python中有很多内置模块，如math、random等，同时也可以创建自己的模块。

要使用一个模块，需要先导入它。有三种常用的导入方式：

1. `import`语句：使用import关键字后，加上要导入的模块名。例如：import math
   这样就可以使用math模块中定义的函数和变量。

2. `from...import`语句：使用from关键字后，加上要导入的模块名和要导入的内容（函数、类或变量）。例如：from math import sqrt
   这样就可以直接使用sqrt函数，而不需要通过math.sqrt来调用。

3. `import...as`语句：使用as关键字后，加上要给导入的模块起一个别名。例如：import math as m
   这样就可以用m来代替math来调用其中定义的内容。

在一个Python脚本中，可以通过多个import语句来导入多个模块，并且可以在脚本中随时使用这些被导入的模块。

除了内置模块外，还有很多第三方库也提供了丰富的功能和工具，可以通过pip等包管理工具进行安装和使用。

模块的使用可以提高代码的可维护性和复用性。通过将相关的代码组织在一个模块中，可以更好地组织代码结构，并且可以在多个项目中重复使用。


##  `if __name__ == '__main__'`

1. 摘要

	通俗的理解`__name__ == '__main__'`：假如你叫小明.py，在朋友眼中，你是小明`(__name__ == '小明')`；在你自己眼中，你是你自己`(__name__ == '__main__')`。
	
	`if __name__ == '__main__'`的意思是：当.py文件被直接运行时，`if __name__ == '__main__'`之下的代码块将被运行；当.py文件以模块形式被导入时，`if __name__ == '__main__'`之下的代码块不被运行。

2. 程序入口

	对于很多编程语言来说，程序都必须要有一个入口，比如C，C++，以及完全面向对象的编程语言Java，C#等。如果你接触过这些语言，对于程序入口这个概念应该很好理解，C，C++都需要有一个main函数作为程序的入口，也就是程序的运行会从main函数开始。同样，Java，C#必须要有一个包含Main方法的主类，作为程序入口。
	
	而Python则不同，它属于脚本语言，不像编译型语言那样先将程序编译成二进制再运行，而是动态的逐行解释运行。也就是从脚本第一行开始运行，没有统一的入口。

	一个Python源码文件（.py）除了可以被直接运行外，还可以作为模块（也就是库），被其他.py文件导入。不管是直接运行还是被导入，.py文件的最顶层代码都会被运行（Python用缩进来区分代码层次），而当一个.py文件作为模块被导入时，我们可能不希望一部分代码被运行。
	```python
	PI = 3.14
	
	def main():
	    print("PI:", PI)
	
	main()
	
	# 运行结果：PI: 3.14
	```
	现在，我们写一个用于计算圆面积的area.py文件，area.py文件需要用到const.py文件中的PI变量。从const.py中，我们把PI变量导入area.py：
	```python
	from const import PI
	
	def calc_round_area(radius):
	    return PI * (radius ** 2)
	
	def main():
	    print("round area: ", calc_round_area(2))
	
	main()
	
	'''
	运行结果：
	PI: 3.14
	round area:  12.56
	'''
	```

	2.2 修改const.py，添加`if __name__ == "__main__"`

	我们看到const.py中的main函数也被运行了，实际上我们不希望它被运行，因为const.py提供的main函数只是为了测试常量定义。这时`if __name__ == '__main__'`派上了用场，我们把const.py改一下，添加`if __name__ == "__main__"`：

	```python
	PI = 3.14
	
	def main():
	    print("PI:", PI)
	
	if __name__ == "__main__":
	    main()
	```
	
	运行const.py，输出如下：

	```python
	PI: 3.14
	```

	运行area.py，输出如下：
	
	```python
	round area:  12.56
	```

	如上，我们可以看到`if __name__ == '__main__'`相当于Python模拟的程序入口，Python本身并没有这么规定，这只是一种编码习惯。由于模块之间相互引用，不同模块可能有这样的定义，而程序入口只有一个。到底哪个程序入口被选中，这取决于`__name__`的值。

3. `__name__`

	3.1 `__name__`反映一个包的结构

	`__name__`是内置变量，可用于反映一个包的结构。假设我们有一个包a，包的结构如下：

	```
	a
	├── b
	│   ├── c.py
	│   └── __init__.py
	└── __init__.py
	```

	在包a中，文件`c.py，__init__.py，__init__.py`的内容都为：

	```python
	print(__name__)
	```

	当一个.py文件（模块）被其他.py文件（模块）导入时，我们在命令行执行

	```shell
	python -c "import a.b.c"
	```

	输出结果：

	```
	a
	a.b
	a.b.c
	```

	由此可见，`__name__`可以清晰地反映一个模块在包中的层次。

	3.2 `__name__`表示当前模块的名字

	`__name__`是内置变量，可用于表示当前模块的名字。我们直接运行一个.py文件（模块）

	```
	python a/b/c.py
	```

	输出结果：

	```
	__main__
	```

	由此我们可知：如果一个.py文件（模块）被直接运行时，则其没有包结构，其`__name__`值为`__main__`，即模块名为`__main__`。
	
	所以，`if __name__ == '__main__'`的意思是：当.py文件被直接运行时，`if __name__ == '__main__'`之下的代码块将被运行；当.py文件以模块形式被导入时，`if __name__ == '__main__'`之下的代码块不被运行。

##  `__init__.py`和`__main__.py`文件

在构建大型Python项目时经常会使用到__init__.py和__main__.py文件，通过它们可以灵活的管理模块和包。

###  `__init__.py`文件的应用

在Python项目中，如果某个文件夹被设置成包的形式，那么该文件夹中必须要包含一个__init__.py文件，即使它是空文件。当导入这个包时，__init__.py文件中的代码会被优先执行。

- **构建层级包**

封装一个包，确保每个目录下都定义了一个__init__.py文件， 例如：

```shell
ml_api/
    __init__.py
    regression/
        __init__.py
        linear_regression.py
        ridge_regression.py
        ...
    classification/
        __init__.py
        svm.py
        xgboost.py
        ...
run.py
```

这样就能够在其他项目模块执行各种import语句，如下所示：
```python
import ml_api.classification.svm
from ml_api.classification import xgboost
import ml_api.regression import ridge_regression as ridge
```

__init__.py文件的目的是使得不同运行级别的包能够可选的初始化代码。如果执行了语句`import ml_api`那么文件ml_api/__init__.py将被导入，建立以ml_api为命名空间的内容。像`import ml_api.classification.svm`这样导入，那么文件ml_api/__init__.py和文件ml_api/classification/__init__.py将在文件ml_api/classification/svm.py导入之前导入。

- **控制子模块导入**

绝大部分时候让__init__.py文件为空就好，但是有些情况下可以包含代码，从而行使特殊的功能，如控制子模块导入。

```python3
# ml_api/classification/__init__.py
from . import svm
from . import xgboost
```

现在仅通过`import ml_api.classification`一行代码就可以替代`import ml_api.classification.svm`和`import ml_api.classification.xgboost` 两行代码的功能。

- **对子模块进行重构**

__init__.py还可以将多个模块合并到一个逻辑命名空间。程序模块可以通过变成包的形式分割成多个独立的文件，不妨考虑

```python3
# test_module.py
class C1:
    def f1(self):
        print("C1.f1")

class C2(C1):
    def f2(self):
        print("C2.f2")
```

如果想把test_module.py拆分成两个文件，然后分别定义成类，从而实现逻辑上独立。要做到这一点，首先需要使用test_module目录来替换文件test_module.py。 这这个目录下，创建如下文件：

```python
test_module/
    __init__.py
    c1.py
    c2.py
```

在c1.py文件中插入以下代码：

```python
# c1.py
class C1:
    def f1(self):
        print("C1.f1")
```

在c2.py文件中插入以下代码：

```text
# c2.py
from .c1 import C1
class C2(C1):
    def f2(self):
        print("C2.f2")
```

最后，在 __init__.py 中，将上述两个文件聚合在一起：

```python
# __init__.py
from .c1 import C1
from .c2 import C2
```

至此，test_module就整合为统一的逻辑模块

```powershell
>>>import test_module
>>>c1 = test_module.C1()
>>>c1.f1()
C1.f1
>>>c2 = test_module.C2()
>>>c2.f2()
C2.f2
```

上面讨论的其实是包的设计问题，在一个大型的代码库中，每个功能模块都是独立的文件，用户如果想使用相应的功能按需导入即可

```python
from package.module_1 import C1
from package.module_2 import C2
...
```

但是这样往往会导致用户的负担增加，因为他需要知道不同的功能代码位于包的哪个模块，通常情况下应该是将这些逻辑统一起来，使用一条import语句简化导入流程

```python
from package import C1, C2
```

因此需要使用__init__.py文件来将每个独立的模块聚合在一起。再举一个亲身使用的例子，假设使用fastAPI作为后端搭建机器学习算法API，为了叙述方便，简化的目录结构如下

```text
ml_api/
    ml/
        __init__.py
            regression.py
            classification.py
    run.py
```

构建回归算法的请求API

```python
# regression.py
from fastapi import APIRouter
...
regression_app = APIRouter()
...
```

构建分类算法的请求API

```python
# classification.py
from fastapi import APIRouter
...
classification_app = APIRouter()
...
```

使用__init__.py文件整合所有算法模块

```python
# __init__.py
from .regression import regression_app
from .classification import classification_app
```

最后在run.py文件中统一所有的路由，组建完整的API

```python
import uvicorn
from fastapi import FastAPI
...
from ml import regression_app, classification_app

app = FastAPI(...)
...
app.include_router(regression_app, prefix='/regression')
app.include_router(classification_app, prefix='/classification')

if __name__ == '__main__':
    uvicorn.run('run:app', host='0.0.0.0', port=55555, reload=True, debug=True)
```

- **延迟加载**

对于构建一个很大的包，如果在__init__.py文件中一次性导入了所有必需的模块，可能会引发一些问题。所以在某些特殊情况下，需要设计成当组件使用时才会被加载。以上一小节第一个项目为例，如果要实现这种逻辑，__init__.py文件需要改动一些代码：

```python
# __init__.py
def C1:
    from .c1 import C1
    return C1()

def C2():
    from .c2 import C2
    return C2()
```

在这个版本中，只有当C1和C2函数被调用时才会加载C1和C2类，但是对于用户而言，使用方法上并不会有什么不同

```powershell
>>>import test_module
>>>c1 = test_module.C1()
>>>c1.f1()
C1.f1
```

### `__main__.py`文件的应用

在Python项目中，__main__.py相较于__init__.py文件的用法来说比较单一，一句话总结就是，如果想要使得某个文件夹能够执行，那么该文件夹中必须要包含一个__main__.py文件，否则就会抛出异常。假设项目目录结构如下

```text
your_package/
    __init__.py
    __main__.py
    a.py
    b.py
    ...
```

然后在__main__.py文件中插入行使相关功能的代码，运行your_package即可

```powershell
>>>python your_package
```

## 模块中的__all__
如果一个模块文件中有all变量,当使用`from xxx import*`导入时,只能导入这个列表中的元素。
- my_module1模块代码
```python
__all__= ['testA']
def testA():
	print('testA')
def testB():
	print('testB')
```
总结
·如果一个文件中有`__all__`变量,那么也就意味着不在这个变量中的元素,不会被`from xxx import *`时导入

# 闭包和装饰器
## 闭包
## 闭包的使用
## 修改闭包内使用的外部变量
## 装饰器
## 装饰器的使用
## 通用装饰器的使用
## 多个装饰器的使用
## 带有参数的装饰器
## 类装饰器的使用

# 正则表达式
## property属性
## with语句和上下文管理器
## 生成器的创建方式
## 深拷贝和浅拷贝
## 正则表达式的概述
## re的模块介绍
## 匹配单个字符
## 匹配多个字符
## 匹配开头和结尾
## 匹配分组





