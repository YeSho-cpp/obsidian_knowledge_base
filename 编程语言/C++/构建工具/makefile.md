## makefile概述

使用 GCC 的命令行进行程序编译在单个文件下是比较方便的，当工程中的文件逐渐增多，甚至变得十分庞大的时候，使用 GCC 命令编译就会变得力不从心。这种情况下我们需要借助项目构造工具`make`帮助我们完成这个艰巨的任务。 <font color=#ff0000>make是一个命令工具，是一个解释makefile中指令的命令工具</font>，一般来说，大多数的IDE都有这个命令，比如：Visual C++的nmake，QtCreator的qmake等。

make工具在构造项目的时候需要加载一个叫做`makefile`的文件，makefile关系到了整个工程的编译规则。一个工程中的源文件不计数，其按类型、功能、模块分别放在若干个目录中，makefile定义了一系列的规则来指定哪些文件需要先编译，哪些文件需要后编译，哪些文件需要重新编译，甚至于进行更复杂的功能操作，因为makefile就像一个Shell脚本一样，其中也可以执行操作系统的命令。

makefile带来的好处就是——“自动化编译”，一旦写好，只需要一个make命令，整个工程完全自动编译，极大的提高了软件开发的效率。

makefile文件有两种命名方式 `makefile` 和 `Makefile`，构建项目的时候在哪个目录下执行构建命令 make这个目录下的 makefile 文件就会别加载，因此在一个项目中可以有多个`makefile`文件，分别位于不同的项目目录中。

## 规则

> [!info] 规则
> Makefile的框架是由规则构成的。make命令执行时先在Makefile文件中查找各种规则，对各种规则进行解析后运行规则。规则的基本格式为:

```makefile
# 每条规则的语法格式:
target1,target2...: depend1, depend2, ...
	command
	......
	......
```


每条规则由三个部分组成分别是`目标(target)`, `依赖(depend)`和`命令(command)`。

- `命令(command)`: 当前这条规则的动作，一般情况下这个动作就是一个shell命令
	- 例如：通过某个命令编译文件、生成库文件、进入目录等。
	- 动作可以是多个，<font color=#ff0000>每个命令前必须有一个Tab缩进并且独占占一行</font>。
- `依赖(depend)`: 规则所必需的依赖条件，在规则的命令中可以使用这些依赖。
	- 例如：生成可执行文件的目标文件（`*.o`）可以作为依赖使用
	- 如果规则的命令中不需要任何依赖，那么规则的依赖可以为空
	- 当前规则中的依赖可以是其他规则中的某个目标，这样就形成了规则之间的嵌套
	- 依赖可以根据要执行的命令的实际需求, 指定很多个
- `目标(target)`： 规则中的目标，这个目标和规则中的命令是对应的
	- 通过执行规则中的命令，可以生成一个和目标同名的文件
	- 规则中可以有多个命令, 因此可以通过这多条命令来生成多个目标, 所有目标也可以有很多个
	- 通过执行规则中的命令，可以只执行一个动作，不生成任何文件，这样的目标被称为`伪目标`

```bash
# 举例: 有源文件 a.c b.c c.c head.h, 需要生成可执行程序 app
################# 例1 #################
app:a.c b.c c.c
	gcc a.c b.c c.c -o app

################# 例2 #################
# 有多个目标, 多个依赖, 多个命令
app,app1:a.c b.c c.c d.c
	gcc a.c b.c -o app
	gcc c.c d.c -o app1
	
################# 例3 #################	
# 规则之间的嵌套
app:a.o b.o c.o
	gcc a.o b.o c.o -o app
# a.o 是第一条规则中的依赖
a.o:a.c
	gcc -c a.c
# b.o 是第一条规则中的依赖
b.o:b.c
	gcc -c b.c
# c.o 是第一条规则中的依赖
c.o:c.c
	gcc -c c.c
```

## 工作原理

> 在此主要为大家剖析一下通过提供的 `makefile` 文件，构建工具make什么时候编译项目中的所有文件, 什么时候只选择更新项目中的某几个文件。另外再研究一下如果makefile里边有多个规则它们之间是如何配合工作的，我们基于下边的例子，依次进行讲解。

### 规则的执行

- <font color=#ff0000>在调用 make 命令编译程序的时候，make 会首先找到 Makefile 文件中的第1个规则，分析并执行相关的动作</font>。但是需要注意的是，好多时候要执行的动作（命令）中使用的依赖是不存在的，如果使用的依赖不存在，这个动作也就不会被执行。
- 对应的解决方案是先将需要的依赖生成出来，我们就可以在makefile中添加新的规则，将不存在的依赖作为这个新的规则中的目标，当这条新的规则对应的命令执行完毕，对应的目标就被生成了，同时另一条规则中需要的依赖也就存在了。
- 这样，makefile中的某一条规则在需要的时候，就会被其他的规则调用，直到makefile中的第一条规则中的所有的依赖全部被生成，第一条规则中的命令就可以基于这些依赖生成对应的目标，make 的任务也就完成了。

```bash
# makefile
# 规则之间的嵌套
# 规则1
app:a.o b.o c.o
	gcc a.o b.o c.o -o app
# 规则2
a.o:a.c
	gcc -c a.c
# 规则3
b.o:b.c
	gcc -c b.c
# 规则4
c.o:c.c
	gcc -c c.c
```

> 在这个例子中，如果执行 make 命令就会根据这个 makefile 中的4条规则编译这三个源文件。在解析第一条规则的时候发现里边的三个依赖都是不存在的，因此规则对应的命令也就不能被执行。
> 
> 当依赖不存在的时候，make就是查找其他的规则，看哪一条规则是用来生成需要的这个依赖的，找到之后就会执行这条规则中的命令。因此规则2， 规则3， 规则4里的命令会相继被执行，当规则1中依赖全部被生成之后对应的命令也就被执行了，因此规则1的目标被生成，make工作结束。



> [!abstract] 知识点拓展:
> 如果想要执行`makefile`中非第一条规则对应的命令, 那么就不能直接 make, 需要将那条规则的目标也写到 make的后边, 比如只需要执行规则3中的命令, 就需要: make b.o。

### 文件的时间戳
make 命令执行的时候会根据**文件的时间戳**判定是否执行makefile文件中相关规则中的命令。

- 目标是通过依赖生成的，因此`正常情况下：目标时间戳 > 所有依赖的时间戳`, 如果执行make命令的时候检测到规则中的目标和依赖满足这个条件, 那么规则中的命令就不会被执行。
- 当依赖文件被更新了, 文件时间戳也会随之被更新, 这时候 `目标时间戳<某些依赖的时间戳`, 在这种情况下目标文件会通过规则中的命令被重新生成。
- 如果规则中的目标对应的文件根本就不存在，那么规则中的命令肯定会被执行。

```bash
# makefile
# 规则之间的嵌套
# 规则1
app:a.o b.o c.o
	gcc a.o b.o c.o -o app
# 规则2
a.o:a.c
	gcc -c a.c
# 规则3
b.o:b.c
	gcc -c b.c
# 规则4
c.o:c.c
	gcc -c c.c
```

> 根据上文的描述, 先执行 make 命令，基于这个 makefile 编译这几个源文件生成对应的目标文件。然后再修改例子中的 `a.c`, 再次通过`make`编译这几个源文件，那么这个时候先执行规则2更新目标文件`a.o`， 然后再执行规则1更新目标文件`app`，其余的规则是不会被执行的。

### 自动推导

make 是一个功能强大的构建工具，虽然make需要根据 makefile 中指定的规则来完成源文件的编译。作为小白的我们编写makefile的时候难免写的不是那么严谨从而漏写一些构建规则，但是我们会发现程序还是会被编译成功。这是因为 make 有自动推导的能力，不会完全依赖 makefile。

比如: 使用命令 make 编译扩展名为.c 的 C 语言文件的时候，源文件的编译规则不用明确给出。这是因为 make 进行编译的时候会使用一个默认的编译规则，按照默认规则完成对.c文件的编译，生成对应的.o 文件。它使用命令`cc -c`来编译.c 源文件。在 Makefile 中只要给出需要构建的目标文件名（一个.o 文件），make 会自动为这个.o 文件寻找合适的依赖文件（对应的.c 文件），并且使用默认的命令来构建这个目标文件。

假设本地项目目录中有以下几个源文件:
```bash
$ tree
.
├── add.c
├── div.c
├── head.h
├── main.c
├── makefile
├── mult.c
└── sub.c
```

目录中 makefile 文件内容如下
```bash
# 这是一个完整的 makefile 文件
calc:add.o  div.o  main.o  mult.o  sub.o
        gcc  add.o  div.o  main.o  mult.o  sub.o -o calc
```

通过make构建项目:

```bash
$ make
cc    -c -o add.o add.c
cc    -c -o div.o div.c
cc    -c -o main.o main.c
cc    -c -o mult.o mult.c
cc    -c -o sub.o sub.c
gcc  add.o  div.o  main.o  mult.o  sub.o -o calc
```

> 我们可以发现上边的 makefile 文件中只有一条规则, 依赖中所有的 `.o`文件在本地项目目录中是不存在的, 并且也没有其他的规则用来生成这些依赖文件, 这时候 make 会使用**内部默认的构造规则**先将这些依赖文件生成出来, 然后在执行规则中的命令, 最后生成目标文件 calc。

## 变量

> 使用 Makefile 进行规则定义的时候，为了写起来更加灵活，我们可以在里边使用变量。makefile中的变量分为三种：`自定义变量`，`预定义变量`和`自动变量`。

### 自定义变量

> 用`Makefile`进行规则定义的时候，用户可以定义自己的变量，称为用户自定义变量。makefile中的变量是没有类型的，直接创建变量然后给其赋值就可以了。

```bash
# 错误, 只创建了变量名, 没有赋值
变量名 
# 正确, 创建一个变量名并且给其赋值
变量名=变量值
```

在给makefile中的变量赋值之后, 如何在需要的时候将变量值取出来呢?

```bash
# 如果将变量的值取出?
$(变量的名字)

# 举例 add.o  div.o  main.o  mult.o  sub.o
# 定义变量并赋值
obj=add.o  div.o  main.o  mult.o  sub.o
# 取变量的值
$(obj)
```

自定义变量使用举例：

```c
# 这是一个规则，普通写法
calc:add.o  div.o  main.o  mult.o  sub.o
        gcc  add.o  div.o  main.o  mult.o  sub.o -o calc
        
# 这是一个规则，里边使用了自定义变量
obj=add.o  div.o  main.o  mult.o  sub.o
target=calc
$(target):$(obj)
        gcc  $(obj) -o $(target)
```

### 预定义变量

> 在 Makefile 中有一些已经定义的变量，用户可以直接使用这些变量，不用进行定义。在进行编译的时候，某些条件下 Makefile 会使用这些预定义变量的值进行编译。这些预定义变量的名字一般都是大写的，经常采用的预定义变量如下表所示：


|         变量名         |           含义            |   默认值    |
| :-----------------: | :---------------------: | :------: |
|         AR          |      生成静态库库文件的程序名称      |    ar    |
|         AS          |        汇编编译器的名称         |    as    |
|         CC          |        C语言编译器的名称        |    cc    |
|         CPP         |       C语言预编译器的名称        | $(CC) -E |
|         CXX         |       C++语言编译器的名称       |   g++    |
|         FC          |     FORTRAN语言编译器的名称     |   f77    |
|         RM          |        删除文件程序的名称        |  rm -f   |
|       ARFLAGS       |   生成静态库库文件程序的选项   |   无默认值   |
| ASFLAGS |      汇编语言编译器的编译选项       |   无默认值   |
|    CFLAGS     |    C语言编译器的编译选项    |   无默认值   |
|   CPPFLAGS    |   C语言预处理器的编译选项    |   无默认值   |
|   CXXFLAGS    |   C++语言编译器的编译选项   |   无默认值   |
|    FFLAGS     | FORTRAN语言编译器的编译选项 |   无默认值   |
```bash
# 这是一个规则，普通写法
calc:add.o  div.o  main.o  mult.o  sub.o
        gcc  add.o  div.o  main.o  mult.o  sub.o -o calc
        
# 这是一个规则，里边使用了自定义变量和预定义变量
obj=add.o  div.o  main.o  mult.o  sub.o
target=calc
CFLAGS=-O3 # 代码优化
$(target):$(obj)
        $(CC)  $(obj) -o $(target) $(CFLAGS)
```

### 自动变量
Makefile 中的变量除了用户自定义变量和预定义变量外，还有一类自动变量。Makefile 中的规则语句中经常会出现目标文件和依赖文件，`自动变量用来代表这些规则中的目标文件和依赖文件，并且它们只能在规则的命令中使用`。

下表中是一些常见的自动变量。

|  变量    | 含义                                                 |
|:------:|:--------------------------------------------------:|
|  `$*`  |  表示目标文件的名称，不包含目标文件的扩展名                             |
|  `$+`  |  表示所有的依赖文件，这些依赖文件之间以空格分开，按照出现的先后为顺序，其中可能包含重复的依赖文件  |
|  `$<`  |  表示依赖项中第一个依赖文件的名称                                  |
|  `$?`  |  依赖项中，所有比目标文件时间戳晚的依赖文件，依赖文件之间以空格分开                 |
|  `$@`  |  表示目标文件的名称，包含文件扩展名                                 |
|  `$^`  |  依赖项中，所有不重复的依赖文件，这些文件之间以空格分开                       |  

下面几个例子, 演示一下自动变量如何使用。

```bash
# 这是一个规则，普通写法
calc:add.o  div.o  main.o  mult.o  sub.o
        gcc  add.o  div.o  main.o  mult.o  sub.o -o calc
        
# 这是一个规则，里边使用了自定义变量
# 使用自动变量, 替换相关的内容
calc:add.o  div.o  main.o  mult.o  sub.o
	gcc $^ -o $@ 			# 自动变量只能在规则的命令中使用
```

## 模式匹配
在介绍概念之前, 先读一下下面的这个`makefile`文件:
```bash
calc:add.o  div.o  main.o  mult.o  sub.o
        gcc  add.o  div.o  main.o  mult.o  sub.o -o calc
# 语法格式重复的规则, 将 .c -> .o, 使用的命令都是一样的 gcc *.c -c
add.o:add.c
        gcc add.c -c

div.o:div.c
        gcc div.c -c

main.o:main.c
        gcc main.c -c

sub.o:sub.c
        gcc sub.c -c

mult.o:mult.c
        gcc mult.c -c
```

在阅读过程中能够发现从第二个规则开始到第六个规则做的是相同的事情, 但是由于文件名不同不得不在文件中写出多个规则，这就让`makefile`文件看起来非常的冗余，我们可以将这一系列的相同操作整理成一个模板，所有类似的操作都通过模板去匹配`makefile`会因此而精简不少，只是可读性会有所下降。

这个规则模板可以写成下边的样子，这种操作就称之为模式匹配。
```bash
# 模式匹配 -> 通过一个公式, 代表若干个满足条件的规则
# 依赖有一个, 后缀为.c, 生成的目标是一个 .o 的文件, % 是一个通配符, 匹配的是文件名
%.o:%.c
	gcc $< -c
```


<img src="https://subingwen.cn/linux/makefile/image-20200418143747981-1611845766993.png" alt="image.png" style="zoom:60%;" />
## 函数

> makefile中有很多函数并且`所有的函数都是有返回值的`。makefile中函数的格式和C/C++中函数也不同，其写法是这样的： `$(函数名 参数1, 参数2, 参数3, ...)`，主要目的是让我们能够快速方便的得到函数的返回值。
> 
> 这里为大家介绍两个 makefile 中使用频率比较高的函数：`wildcard`和`patsubst`。

### wildcard
这个函数的主要作用是获取**指定目录下指定类型的文件名**，其返回值是以空格分割的、指定目录下的所有符合条件的文件名列表。函数原型如下：

```bash
# 该函数的参数只有一个, 但是这个参数可以分成若干个部分, 通过空格间隔
$(wildcard PATTERN...)
	参数:	指定某个目录, 搜索这个路径下指定类型的文件，比如： *.c
```

- 参数功能:
	- PATTERN 指的是某个或多个目录下的对应的某种类型的文件, 比如当前目录下的.c文件可以写成 *.c
	- 可以指定多个目录，每个路径之间使用空格间隔
- 返回值：

	- 得到的若干个文件的文件列表， 文件名之间使用空格间隔
	- 示例：`$(wildcard *.c ./sub/*.c)`
		- 返回值格式: a.c b.c c.c d.c e.c f.c ./sub/aa.c ./sub/bb.c

- 函数使用举例:
```c
# 使用举例: 分别搜索三个不同目录下的 .c 格式的源文件
src = $(wildcard /home/robin/a/*.c /home/robin/b/*.c *.c)  # *.c == ./*.c
# 返回值: 得到一个大的字符串, 里边有若干个满足条件的文件名, 文件名之间使用空格间隔
/home/robin/a/a.c /home/robin/a/b.c /home/robin/b/c.c /home/robin/b/d.c e.c f.c
```

### patsubst
这个函数的功能是按照**指定的模式替换指定的文件名的后缀**, 函数原型如下:
```c
# 有三个参数, 参数之间使用 逗号间隔
$(patsubst <pattern>,<replacement>,<text>)
```

- 参数功能:
	- pattern: 这是一个模式字符串, 需要指定出要被替换的文件名中的后缀是什么
		- 文件名和路径不需要关心, 因此使用 `%` 表示即可 `[通配符是 %]`
		- 在通配符后边指定出要被替换的后缀, 比如: `%.c`, 意味着 `.c`的后缀要被替换掉
	- replacement: 这是一个模式字符串, 指定参数pattern中的后缀最终要被替换为什么
		- 还是使用 `%` 来表示参数pattern 中文件的路径和名字
		- 在通配符 `%` 后边指定出新的后缀名, 比如: `%.o` 这表示原来的后缀被替换为 `.o`
	- text: 该参数中存储这要被替换的原始数据
	- 返回值:
		- 函数返回被替换过后的字符串。


函数使用举例:
```bash
src = a.cpp b.cpp c.cpp e.cpp
# 把变量 src 中的所有文件名的后缀从 .cpp 替换为 .o
obj = $(patsubst %.cpp, %.o, $(src)) 
# obj 的值为: a.o b.o c.o e.o
```

## makefile的编写
下面基于一个简单的项目, 为大家演示一下编写一个`makefile`从不标准到标准的进化过程。

```bash
# 项目目录结构
.
├── add.c
├── div.c
├── head.h
├── main.c
├── mult.c
└── sub.c
# 需要编写makefile对该项目进行自动化编译
```

### 版本1

```makefile
calc:add.c  div.c  main.c  mult.c  sub.c
        gcc add.c  div.c  main.c  mult.c  sub.c -o calc
```

> 这个版本的优点：书写简单
> 这版本的缺点：只要依赖中的某一个源文件被修改，所有的源文件都需要被重新编译，太耗时、效率低
> 改进方式：提高效率，修改哪一个源文件, 哪个源文件被重新编译, 不修改就不重新编译

### 版本2

```
# 默认所有的依赖都不存在, 需要使用其他规则生成这些依赖
# 因为 add.o 被更新, 需要使用最新的依赖, 生成最新的目标
calc:add.o  div.o  main.o  mult.o  sub.o
        gcc  add.o  div.o  main.o  mult.o  sub.o -o calc

# 如果修改了add.c, add.o 被重新生成
add.o:add.c
        gcc add.c -c

div.o:div.c
        gcc div.c -c

main.o:main.c
        gcc main.c -c

sub.o:sub.c
        gcc sub.c -c

mult.o:mult.c
        gcc mult.c -c
```

> 这个版本的优点：相较于版本1效率提升了
> 这个版本的缺点：规则比较冗余, 需要精简
> 改进方式：在 makefile 中使用变量 和 模式匹配

### 版本3
```bash
# 添加自定义变量 -> makefile中注释前 使用 # 
obj=add.o  div.o  main.o  mult.o  sub.o
target=calc

$(target):$(obj)
        gcc $(obj)  -o $(target)

%.o:%.c
        gcc $< -c
```

> 这个版本的优点：文件精简不少，变得简洁了
> 这个版本的缺点：变量 obj 的值需要手动的写出来, 如果需要编译的项目文件很多，都用手写出来不现实
> 改进方式：在makefile中使用函数

### 版本4
```bash
# 添加自定义变量 -> makefile中注释前 使用 # 
# 使用函数搜索当前目录下的源文件 .c
src=$(wildcard *.c)
# 将源文件的后缀替换为 .o
# % 匹配的内容是不能被替换的, 需要替换的是第一个参数中的后缀, 替换为第二个参数中指定的后缀
# obj=$(patsubst %.cpp, %.o, $(src)) 将src中的关键字 .cpp 替换为 .o
obj=$(patsubst %.c, %.o, $(src))
target=calc

$(target):$(obj)
        gcc $(obj)  -o $(target)

%.o:%.c
        gcc $< -c
```

> 这个版本的优点：解决了自动加载项目文件的问题，解放了双手
> 这个版本的缺点：没有文件删除的功能，不能删除项目编译过程中生成的目标文件（`*.o`）和可执行程序
> 改进方式: 在makefile文件中添加新的规则用于删除生成的目标文件（`*.o`）和可执行程序

### 版本5

```bash
# 添加自定义变量 -> makefile中注释前 使用 # 
# 使用函数搜索当前目录下的源文件 .c
src=$(wildcard *.c)
# 将源文件的后缀替换为 .o
obj=$(patsubst %.c, %.o, $(src))
target=calc
# obj 的值 xxx.o xxx.o xxx.o xx.o
$(target):$(obj)
        gcc $(obj)  -o $(target)

%.o:%.c
        gcc $< -c

# 添加规则, 删除生成文件 *.o 可执行程序
# 这个规则比较特殊, clean根本不会生成, 这是一个伪目标
clean:
        rm $(obj) $(target)
```

> 这个版本的优点: 添加了新的规则（16行）用于文件的删除, 直接`make clean`就可以执行规则中的删除命令了
> 这个版本的缺点: 在下面有具体的问题演示和分析
> 改进方式: 在makefile文件中声明 `clean`是一个伪目标，让 make 放弃对它的时间戳检测

正常情况下这个版本的makefile是可以正常工作的，但是我们如果在这个项目目录中添加一个叫做`clean`的文件（和规则中的目标名称相同），再进行`make clean`发现这个规则就不能正常工作了。

```bash
# 在项目目录中添加一个叫 clean的文件, 然后在 make clean 这个规则中的命令就不工作了
$ ls
add.c  calc   div.c  head.h  main.o    mult.c  sub.c
add.o  div.o  main.c  makefile  mult.o  sub.o  clean  ---> 新添加的

# 使用 makefile 中的规则删除生成的目标文件和可执行程序
$ make clean
make: 'clean' is up to date. 

# 查看目录, 发现相关文件并没有被删除, make clean 失败了
$ ls
add.c  calc   div.c  head.h  main.o    mult.c  sub.c
add.o  clean  div.o  main.c  makefile  mult.o  sub.o
```

> 这个问题的关键点在于`clean`是一个伪目标, 不对应任何实体文件, 在前边讲`关于文件时间戳`更新问题的时候说过，如果目标不存在规则的命令肯定被执行， 如果目标文件存在了就需要比较规则中目标文件和依赖文件的时间戳，满足条件才执行规则的命令，否则不执行。
> 解决这个问题需要在`makefile`中声明`clean`是一个伪目标，这样 make 就不会对文件的时间戳进行检测，规则中的命令也就每次都会被执行了。
> 在 makefile 中声明一个伪目标需要使用 `.PHONY` 关键字, 声明方式为: `.PHONY:伪文件名称`。

### 最终版

```bash
# 添加自定义变量 -> makefile中注释前 使用 # 
# 使用函数搜索当前目录下的源文件 .c
src=$(wildcard *.c)
# 将源文件的后缀替换为 .o
obj=$(patsubst %.c, %.o, $(src))
target=calc

$(target):$(obj)
        gcc $(obj)  -o $(target)

%.o:%.c
        gcc $< -c

# 添加规则, 删除生成文件 *.o 可执行程序
# 声明clean为伪文件
.PHONY:clean
clean:
        # shell命令前的 - 表示强制这个指令执行, 如果执行失败也不会终止
        -rm $(obj) $(target) 
        echo "hello, 我是测试字符串"
```

## 练习题

```shell
# 目录结构
.
├── include
│   └── head.h	==> 头文件, 声明了加减乘除四个函数
├── main.c		==> 测试程序, 调用了head.h中的函数
└── src
    ├── add.c	==> 加法运算
    ├── div.c	==> 除法运算
    ├── mult.c  ==> 乘法运算
    └── sub.c   ==> 减法运算
```

根据上边的项目目录结构编写的makefile文件如下:
```bash
# 最终的目标名 app
target = app
# 搜索当前项目目录下的源文件
src=$(wildcard *.c ./src/*.c)
# 将文件的后缀替换掉 .c -> .o
obj=$(patsubst %.c, %.o, $(src))
# 头文件目录
include=./include

# 第一条规则
# 依赖中都是 xx.o yy.o zz.o
# gcc命令执行的是链接操作
$(target):$(obj)
        gcc $^ -o $@

# 模式匹配规则
# 执行汇编操作, 前两步: 预处理, 编译是自动完成
%.o:%.c
        gcc $< -c -I $(include) -o $@

# 添加一个清除文件的规则
.PHONY:clean

clean:
        -rm $(obj) $(target) -f
```





# Makefile基础知识

## make使用流程

1. 准备好需要编译的源代码
2. 编写Makefile文件
3. 在命令行执行make命令




## 最简单的Makefile

```makefile
hello: hello.cpp
    g++ hello.cpp -o hello # 开头必须为一个Tab，不能为空格
```

**但通常需要将编译与链接分开写，分为如下两步**

```makefile
hello: hello.o
    g++ hello.o -o hello
hello.o: hello.cpp
    g++ -c hello.cpp
```



**规则**(Rules)：一个Makefile文件由一条一条的规则构成，一条规则结构如下

```makefile
target … (目标): prerequisites …(依赖)
        recipe(方法)
        …
        …
```

第二种写法

```makefile
target … (目标): prerequisites …(依赖); recipe(方法) ;…
```



Make主要用于处理C和C++的编译工作，但不只能处理C和C++，所有编译器/解释器能在命令行终端运行的编程语言都可以处理(例如Java、Python、 Golang....)。Make也不只能用来处理编程语言，所有基于一些文件(依赖)的改变去更新另一些文件(目标)的工作都可以做。

**Make编译与打包Java程序示例**

```makefile
snake.jar : C.class Main.class SnakeFrame.class SnakePanel.class
    jar -cvfe snake.jar Main *.class

C.class : C.java
    javac C.java

Main.class : Main.java
    javac Main.java

SnakeFrame.class : SnakeFrame.java
    javac SnakeFrame.java

SnakePanel.class : SnakePanel.java
    javac SnakePanel.java

.PHONY: clean
clean:
    rm *.class *.jar
```







## Makefile文件的命名与指定

Make会自动查找makefile文件，查找顺序为GNUmakefile -> makefile -> Makefile

**GNUmakefile**：不建议使用，因为只有GNU make会识别，其他版本的make（如BSD make, Windows nmake等）不会识别，如果只给GNU make使用的情况

**makefile**：可以使用，GNU make和其他版本make识别

**Makefile**：最常用，强烈建议使用

如果运行make的时候没有找到以上名字的文件，则会报错，这时候可以手动指定文件名

```shell
make -f mkfile  # make -f <filename>
make --file=mkfile # make --file=<filename>
```

> 手动指定之后，make就会使用指定的文件，即使有Makefile或者makefile不会再自动使用





## Makefile文件内容组成

一个Makefile文件通常由五种类型的内容组成：显式规则、隐式规则、变量定义、指令和注释

**显式规则**(*explicit rules*)：显式指明何时以及如何生成或更新目标文件，显式规则包括目标、依赖和更新方法三个部分

**隐式规则**(*implicit rules*)：根据文件自动推导如何从依赖生成或更新目标文件。

**变量定义**(*variable definitions*)：定议变量并指定值，值都是字符串，类似C语言中的宏定义(#define)，在使用时将值展开到引用位置

**指令**(*directives*)：在make读取Makefile的过程中做一些特别的操作，包括：

1. 读取(包含)另一个makefile文件(类似C语言中的#include)

2. 确定是否使用或略过makefile文件中的一部分内容(类似C语言中的#if)

3. 定义多行变量

**注释**(*comments*)：一行当中 # 后面的内容都是注释，不会被make执行。make当中只有单行注释。如果需要用到#而不是注释，用\\\#。


## 一个稍微复杂的Makefile

![](./img/1.png)

```makefile
sudoku: block.o command.o input.o main.o scene.o test.o
    g++ -o sudoku block.o command.o input.o main.o scene.o test.o

block.o: block.cpp common.h block.h color.h
    g++ -c block.cpp

command.o: command.cpp scene.h common.h block.h command.h
    g++ -c command.cpp

input.o: input.cpp common.h utility.inl
    g++ -c input.cpp

main.o: main.cpp scene.h common.h block.h command.h input.h
    g++ -c main.cpp

scene.o: scene.cpp common.h scene.h block.h command.h utility.inl
    g++ -c scene.cpp

test.o: test.cpp test.h scene.h common.h block.h command.h
    g++ -c test.cpp

hello.o: hello.cpp
    g++ -c hello.cpp


clean:
    rm block.o command.o input.o main.o scene.o test.o
    rm sudoku.exe
```

```makefile
target … (目标): prerequisites …(依赖)
        recipe(方法)
        …
        …
```

## 目标

1. Makefile中会有很多目标，但最终目标只有一个，其他所有内容都是为这个最终目标服务的，写Makefile的时候**先写出最终目标，再依次解决总目标的依赖**

2. 一般情况第一条规则中的目标会被确立为最终目标，第一条规则默认会被make执行

3. 通常来说目标是一个文件，一条规则的目的就是生成或更新目标文件。

4. make会根据目标文件和依赖文件最后修改时间判断是否需要执行更新目标文件的方法。如果目标文件不存在或者目标文件最后修改时间早于其中一个依赖文件最后修改时间，则重新执行更新目标文件的方法。否则不会执行。

5. 除了最终目标对应的更新方法默认会执行外，如果Makefile中一个目标不是其他目标的依赖，那么这个目标对应的规则不会自动执行。需要手动指定，方法为
   
   ```makefile
   make <target>  # 如 make clean , make hello.o
   ```

6. 可以使用.DEFAULT_GOAL来修改默认最终目标
   
   ```makefile
   .DEFAULT_GOAL = main
   
   all: 
       @echo all
   
   main:
       @echo main
   ```









## 伪目标

如果一个标并不是一个文件，则这个目标就是伪目标。例如前面的clean目标。如果说在当前目录下有一个文件名称和这个目标名称冲突了，则这个目标就没法执行。这时候需要用到一个特殊的目标 .PHONY，将上面的clean目标改写如下 

```makefile
.PHONY: clean
clean:
    rm block.o command.o input.o main.o scene.o test.o
    rm sudoku.exe
```

这样即使当前目录下存在与目标同名的文件，该目标也能正常执行。

**伪目标的其他应用方式**

如果一条规则的依赖文件没有改动，则不会执行对应的更新方法。如果需要每次不论有没有改动都执行某一目标的更新方法，可以把对应的目标添加到.PHONY的依赖中，例如下面这种方式，则每次执行make都会更新test.o，不管其依赖文件有没有改动

```makefile
test.o: test.cpp test.h
        g++ -c test.cpp

.PHONY: clean test.o
```

## 依赖

### 依赖类型

**普通依赖**

前面说过的这种形式都是普通依赖。直接列在目标后面。普通依赖有两个特点：

1. 如果这一依赖是由其他规则生成的文件，那么执行到这一目标前会先执行生成依赖的那一规则 
2. 如果任何一个依赖文件修改时间比目标晚，那么就重新生成目标文件

**order-only依赖**

依赖文件不存在时，会执行对应的方法生成，但依赖文件更新并不会导致目标文件的更新

如果目标文件已存在，order-only依赖中的文件即使修改时间比目标文件晚，目标文件也不会更新。

定义方法如下：

```makefile
targets : normal-prerequisites | order-only-prerequisites
```

normal-prerequisites部分可以为空
### 指定依赖搜索路径

make默认在Makefile文件所在的目录下查找依赖文件，如果找不到，就会报错。这时候就需要手动指定搜索路径，用VPATH变量或vpath指令。

**VPATH用法如下：**

```makefile
VPATH = <dir1>:<dir2>:<dir3>...
# 例如
VPATH = include:src
```

多个目录之间冒号隔开，这时make会在VPATH指定的这些目录里面查找依赖文件。

**vpath指令用法：**

vpath比VPATH使用更灵活，可以指定某个类型的文件在哪个目录搜索。

用法如下：

```makefile
vpath <pattern> <directories>

vpath %.h include  # .h文件在include目录下查找
vpath %.h include:headers  # .h文件在include或headers文件下查找

vpath % src   # 所有文件都在src下查找

vpath hello.cpp src  # hello.cpp文件在src查找
```


## 更新方法

```makefile
target … (目标): prerequisites …(依赖)
        recipe(方法)
        …
        …
```

#### 关于执行终端

更新方法实际上是一些Shell指令，通常以Tab开头，或直接放在目标-依赖列表后面，用分号隔开。这些指令都需要交给Shell执行，所以需要符合Shell语法。默认使用的Shell是sh，在Windows上如果没有安装sh.exe的话会自动查找使用cmd.exe之类的终端。这时有的指令写法，例如循环语句，与Linux不同，需要注意。

可以通过SHELL变量手动指定Shell

```makefile
SHELL = C:/Windows/System32/WindowsPowerShell/v1.0/powershell.exe
SHELL = cmd.exe
```

默认的执行方式为一条指令重新调用一个Shell进程来执行。有时为了提高性能或其他原因，想让这个目标的所有指令都在同一进程中执行，可以在Makefile中添加 .ONESHELL

```makefile
 .ONESHELL:
```

这样所有指令都会在同一次Shell调用中执行

#### Shell语句回显问题

通常make在执行一条Shell语句前都会先打印这条语句，如果不想打印可以在语句开头在@

```makefile
@echo hello
@g++ -o hello hello.cpp
```

也可以使用.SILENT来指定哪些目标的更新方法指令不用打印

```makefile
.SILENT: main all
```

#### 错误处理

如果一条规则当中包含多条Shell指令，每条指令执行完之后make都会检查返回状态，如果返回状态是0，则执行成功，继续执行下一条指令，直到最后一条指令执行完成之后，一条规则也就结束了。

如果过程中发生了错误，即某一条指令的返回值不是0，那么make就会终止执行当前规则中剩下的Shell指令。

例如

```makefile
clean:
    rm main.o hello.o
    rm main.exe
```

这时如果第一条rm main.o hello.o出错，第二条rm main.exe就不会执行。类似情况下，希望make忽视错误继续下一条指令。在指令开头`-`可以达到这种效果。

```makefile
clean:
    -rm main.o hello.o
    -rm main.exe
```

## 变量应用

Makefile中的变量有点类似C语言中的宏定义，即用一个名称表示一串文本。但与C语言宏定义不同的是，Makefile的变量值是可以改变的。变量定义之后可以在目标、依赖、方法等Makefile文件的任意地方进行引用。

> Makefile中的变量值只有一种类型： 字符串

**变量可以用来表示什么**

- 文件名序列

- 编译选项

- 需要运行的程序

- 需要进行操作的路径

- ......

### 变量定义与引用方式

**定义方式**

```makefile
# <变量名> = <变量值>  <变量名> := <变量值>  <变量名> ::= <变量值>
files = main.cpp hello.cpp
objects := main.o hello.o
var3 ::= main.o
```

> 变量名区分大小写，可以是任意字符串，不能含有":", "#", "="

**使用方式**

```makefile
# $(<变量名>) 或者 ${<变量名>}
main.o : $(files) # 或者 ${files}
    ...
```

> 如果变量名只有一个字符，使用时可以不用括号，如\$a, \$b， 但不建议这样用，不管是否只有一个字符都写成\$(a), \$(b)这种形式

### Makefile读取过程

GNU make分两个阶段来执行Makefile，第一阶段(读取阶段)：

- 读取Makefile文件的所有内容

- 根据Makefile的内容在程序内建立起变量

- 在程序内构建起显式规则、隐式规则

- 建立目标和依赖之间的依赖图

第二阶段(目标更新阶段)：

- 用第一阶段构建起来的数据确定哪个目标需要更新然后执行对应的更新方法

变量和函数的展开如果发生在第一阶段，就称作**立即展开**，否则称为**延迟展开**。立即展开的变量或函数在第一个阶段，也就是Makefile被读取解析的时候就进行展开。延迟展开的变量或函数将会到用到的时候才会进行展开，有以下两种情况：

+ 在一个立即展开的表达式中用到

+ 在第二个阶段中用到

**显式规则中，目标和依赖部分都是立即展开，在更新方法中延迟展开**



### 变量赋值

#### 递归展开赋值（延迟展开）

第一种方式就是直接使用<kbd>=</kbd>，这种方式如果赋值的时候右边是其他变量引用或者函数调用之类的，将不会做处理，直接保留原样，在使用到该变量的时候再来进行处理得到变量值（Makefile执行的第二个阶段再进行变量展开得到变量值）

```makefile
bar2 = ThisIsBar2No.1
foo = $(bar)
foo2 = $(bar2)

all:
    @echo $(foo)  # Huh?
    @echo $(foo2)  # ThisIsBar2No.2
    @echo $(ugh)   # Huh?

bar = $(ugh)
ugh = Huh?
bar2 = ThisIsBar2No.2
```

#### 简单赋值(立即展开)

简单赋值使用<kbd>\:=</kbd>或<kbd>::=</kbd>，这种方式如果等号右边是其他变量或者引用的话，将会在赋值的时候就进行处理得到变量值。（Makefile执行第一阶段进行变量展开）

```makefile
bar2 := ThisIsBar2No.1
foo := $(bar)
foo2 := $(bar2)

all:
    @echo $(foo)    # 空串，没有内容
    @echo $(foo2)    # ThisIsBar2No.1
    @echo $(ugh)    # 

bar := $(ugh)
ugh := Huh?
bar2 := ThisIsBar2No.2
```

#### 条件赋值

条件赋值使用<kbd>?=</kbd>，如果变量已经定义过了（即已经有值了），那么就保持原来的值，如果变量还没赋值过，就把右边的值赋给变量。

```makefile
var1 = 100
var1 ?= 200

all:
    @echo $(var1) # 100 注释var1 = 100之后为200
```

**练习**：试求a的值

```makefile
x = hello
y = world
a := $(x)$(y)

x = y
y = z
a := $($(x))

x = y
y = z
z = u
a := $($($(x)))

x = $(y)
y = z
z = Hello
a := $($(x))
```

#### 追加

使用<kbd>+=</kbd>在变量已有的基础上追加内容

```makefile
files = main.cpp
files += hello.cpp

all:
    @echo $(files)
```

#### Shell运行赋值

使用<kbd>!=</kbd>，运行一个Shell指令后将返回值赋给一个变量

```makefile
gcc_version != gcc --version
files != ls .
```

> 如果使用Windows需要注意，这种赋值方式只适用于与Linux相同的Shell指令，Windows独有的指令不能这样使用。


### 定义多行变量

前面定义的变量都是单行的。

变量值有多行，多用于定义shell指令

**语法**

```makefile
define <varable_name>  # 默认为 = 
# 变量内容
endef

define <varable_name> :=
# 变量内容
endef

define <varable_name> +=
# 变量内容
endef
```


**示例**

```makefile
echosomething = @echo This is the first line

define echosomething +=  

@echo hello
@echo world
@echo 3
endef


all:
    $(echosomething)
```

### 取消变量

如果想清除一个变量，用以下方法

```makefile
undefine <变量名>   如 undefine files,  undefine objs
```

### 环境变量的使用

系统中的环境变量可以直接在Makefile中直接使用，使用方法跟普通变量一样

```makefile
all:
    @echo $(USERNAME)
    @echo $(JAVA_HOME)
    @echo $(SystemRoot)
```

### 变量替换引用

语法：__\$(var:a=b)__，意思是将变量var的值当中每一项结尾的a替换为b，直接上例子

```makefile
files = main.cpp hello.cpp
objs := $(files:.cpp=.o) # main.o hello.o
# 另一种写法
objs := $(files:%.cpp=%.o)
```

### 变量覆盖

所有在Makefile中的变量，都可以在执行make时能过指定参数的方式进行覆盖。

```makefile
OverridDemo := ThisIsInMakefile
all:
    @echo $(OverridDemo)
```

如果直接执行

```shell
make
```

则上面的输出内容为*ThisIsInMakefile*，但可以在执行make时指定参数：

```shell
make OverridDemo=ThisIsFromOutShell # 等号两边不能有空格
# 如果变量值中有空格，需要用引号
make OverridDemo=“This Is From Out Shell”
```

则输出OverridDemo的值是ThisIsFromOutShell或This Is From Out Shell。

用这样的命令参数会覆盖Makefile中对应变量的值，如果不想被覆盖，可以在变量前加上override指令，override具有较高优先级，不会被命令参数覆盖

```makefile
override OverridDemo := ThisIsInMakefile
all:
    @echo $(OverridDemo)
```

这样即使命令行指定参数

```shell
make OverridDemo=ThisIsFromOutShell
```

输出结果依然是ThisIsInMakefile











### 自动变量

**\$@**：①本条规则的目标名；②如果目标是归档文件的成员，则为归档文件名；③在多目标的模式规则中, 为导致本条规则方法执行的那个目标名；

**\$<**：本条规则的第一个依赖名称

**\$?**：依赖中修改时间晚于目标文件修改时间的所有文件名，以空格隔开

**\$^**：所有依赖文件名，文件名不会重复，不包含order-only依赖

**\$+**：类似上一个， 表示所有依赖文件名，包括重复的文件名，不包含order-only依赖

**\$|**：所有order-only依赖文件名

__\：(简单理解)目标文件名的主干部分(即不包括后缀名)

**\$%**：如果目标不是归档文件，则为空；如果目标是归档文件成员，则为对应的成员文件名



以下变量对应上述变量，D为对应变量所在的目录，结尾不带/，F为对应变量除去目录部分的文件名

__\$(@D)__

__\$(@F)__

__\$(*D)__

__\$(*F)__

__\$(%D)__

__\$(%F)__

__\$(<D)__

__\$(<F)__

__\$(^D)__

__\$(^F)__

__\$(+D)__

__\$(+F)__

__\$(?D)__

__\$(?F)__

### 绑定目标的变量

Makefile中的变量一般是全局变量。也就是说定义之后在Makefile的任意位置都可以使用。但也可以将变量指定在某个目标的范围内，这样这个变量就只能在这个目标对应的规则里面保用

语法

```makefile
target … : variable-assignment
target … : prerequisites
    recipes
    …
```

例

```makefile
var1 = Global Var

first: all t2

all: var2 = Target All Var
all:
    @echo $(var1)
    @echo $(var2)

t2:
    @echo $(var1)
    @echo $(var2)
```

这种定义变量的方式，目标也可以使用模式匹配，这样所有能匹配上的目标范围内都可以使用这些变量

```makefile
var1 = Global Var

first: all.v t2.v t3

%.v: var2 = Target %.v Var
all.v:
    @echo $@ -- $(var1)
    @echo $@ -- $(var2)

t2.v:
    @echo $@ -- $(var1)
    @echo $@ -- $(var2)
t3:
    @echo $@ -- $(var1)
    @echo $@ -- $(var2)
```

### 二次展开

前面说过依赖中的变量都是在Makefile读取阶段立即展开的。如果想让依赖的的变量延迟展开，可以使用.SECONDEXPANSION:，添加之后，在依赖中使用变量时用`$$`，可以让变量在第二阶段进行二次展开，从而达到延迟展开的效果。

```makefile
VAR1 = main.cpp
.SECONDEXPANSION:
all: $$(VAR1)
    @echo $^


```
 
# 自动推导与隐式规则

Makefile中有一些生成目标文件的规则使用频率非常高，比如由.c或.cpp文件编译成.o文件，这样的规则在make中可以自动推导，所以可以不用明确写出来，这样的规则称为隐式规则。

## 一些make预定义的规则

### C语言编译

从.c到.o

```makefile
$(CC) $(CPPFLAGS) $(CFLAGS) -c
```

### C++编译

从.cc .cpp .C到.o

```makefile
$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c
```

### 链接

由.o文件链接到可执行文件

```makefile
$(CC) $(LDFLAGS) *.o $(LOADLIBES) $(LDLIBS)
```

## 隐式规则中常用一些变量

这些变量都有默认值，也可以自行修改

### CC

编译C语言的程序，默认为 `cc`

### CXX

编译C++的程序，默认为 `g++`

### AR

归档程序，默认为 `ar`

### CPP

C语言预处理程序，默认为 `$(CC) -E`

### RM

删除文件的程序，默认为`rm -f`

### CFLAGS

传递给C编译器的一些选项，如-O2 -Iinclude

### CXXFLAGS

传递给C++编译器的一些选项，如-std=c++ 11 -fexec-charset=GBK   

### CPPFLAGS

C语言预处理的一些选项

### LDFLAGS

链接选项，如-L.

### LDLIBS

链接需要用到的库，如-lkernel32 -luser32 -lgdi32















# 多目标与多规则

显式规则中一条规则可以有多个目标，多个目标可以是相互独立的目标，也可以是组合目标，用写法来区分

## 独立多目标

相互独立的多个目标与依赖之间直接用`:`，常用这种方式的有以下两种情况

1. 只需要写目标和依赖，不需要写方法的时候
   
   ```makefile
   block.o input.o scene.o : common.h
   ```
   
   这种写法等价于
   
   ```makefile
   block.o : common.h
   input.o : common.h
   scene.o : common.h
   ```

2. 生成(更新)目标的方法写法一样的，只是依赖与目标不一样时。之前写的Makfile中，有如下代码：
   
   ```makefile
   block.o: block.cpp common.h block.h color.h
       g++ -c block.cpp
   command.o: command.cpp command.h scene.h
       g++ -c command.cpp
   input.o: input.cpp common.h utility.inl
       g++ -c input.cpp
   main.o: main.cpp scene.h input.h test.h
       g++ -c main.cpp
   scene.o: scene.cpp common.h scene.h utility.inl
       g++ -c scene.cpp
   test.o: test.cpp test.h
       g++ -c test.cpp
   ```
   
   所有.o文件的生成都用的同一方法
   
   ```makefile
   g++ -c <文件名>
   ```
   
   如果不考虑依赖源文件进行更新时，可以进行简写如下：
   
   ```makefile
   block.o command.o input.o main.o scene.o test.o : common.h block.h command.h ...
       g++ -c $(@:%.o=%.cpp)
   ```
   
   这种写法实际上等价于
   
   ```makefile
   block.o : common.h block.h command.h ...
       g++ -c $(subst .o,.cpp,$@)
   command.o : common.h block.h command.h ...
       g++ -c $(subst .o,.cpp,$@)
   input.o : common.h block.h command.h ...
       g++ -c $(subst .o,.cpp,$@)
   main.o : common.h block.h command.h ...
       g++ -c $(subst .o,.cpp,$@)
   scene.o : common.h block.h command.h ...
       g++ -c $(subst .o,.cpp,$@)
   test.o : common.h block.h command.h ...
       g++ -c $(subst .o,.cpp,$@)
   ```
   
   其中，$@表示的是目标名称。subst是一个字符串替换函数，$(subst .o,.cpp,$@)表示将目标名称中的.o替换为.cpp。
   
   这样的简写可以减少内容的书写量，但是不利于将每个目标与依赖分别对应。

独立多目标虽然写在一起，但是每个目标都是单独调用一次方法来更新的。和分开写效果一样。

## 组合多目标

多目标与依赖之前用`&:`，这样的多个目标称为组合目标。与独立多目标的区别在于，独立多目标每个目标的更新需要单独调用一次更新方法。而组合多目标调用一次方法将更新所有目标

```makefile
block.o input.o scene.o &: block.cpp input.cpp scene.cpp common.h
    g++ -c block.cpp
    g++ -c input.cpp
    g++ -c scene.cpp
```

所有目标的更新方法都写到其中，每次更新只会调用一次。

## 同一目标多条规则

同一目标可以对应多条规则。同一目标的所有规则中的依赖会被合并。但如果同一目标对应的多条规则都写了更新方法，则会使用最新的一条更新方法，并且会输出警告信息。

同一目标多规则通常用来给多个目标添加依赖而不用改动已写好的部分。

```makefile
input.o: input.cpp utility.inl
    g++ -c input.cpp
main.o: main.cpp scene.h input.h test.h
    g++ -c main.cpp
scene.o: scene.cpp scene.h utility.inl
    g++ -c scene.cpp

input.o main.o scene.o : common.h
```

同时给三个目标添加了一个依赖common.h，但是不用修改上面已写好的部分。

# 静态模式

独立多目标可以简化Makefile的书写，但是不利于将各个目标的依赖分开，让目标文件根据各自的依赖进行更新。静态模式可以在一定程度上改进依赖分开问题。

静态模式就是用`%`进行文件匹配来推导出对应的依赖。

**语法**

```makefile
targets …: target-pattern(目标模式): prereq-patterns(依赖模式) …
        recipe
        …
```

先看一个例子

```makefile
block.o : %.o : %.cpp %.h
    g++ -c $<
```

block.o为目标，%.o为目标模式，%.cpp，%.h为依赖模式，对于这一条规则，%.o代表的是目标文件block.o，所以这里的%匹配的是block，因此，%.cpp表示block.cpp，%.h代表block.h，所以block.o : %.o : %.cpp %.h表示的意思同下面这种写法

```makefile
block.o : block.cpp block.h
```

自动推导出block.o依赖block.cpp和block.h。

另外，\$<表示目标的第一个依赖，在这条规则中，\$<表示block.cpp

对应的Makefile可以做如下改进

```makefile
block.o command.o input.o scene.o test.o: %.o : %.cpp %.h
    g++ -c $<
main.o: main.cpp scene.h input.h test.h
    g++ -c main.cpp
```

用这种方式可以在简写的同时一定程度上解决各个目标对应的依赖问题。

(不属于静态模式的内容，隐式规则的内容)利用模式匹配可以直接将所有.cpp到.o文件的编译简写为如下

```makefile
%.o : %.cpp %.h
    g++ -c $<
```

# 条件判断

使用条件指令可以让make执行或略过Makefile文件中的一些部分。

**ifdef**  判断一个变量是已否定义

```makefile
OS = Linux
ifdef Win
    OS = Windows
endif


OS = Linux
ifdef Win
    OS = Windows
else ifdef Mac
    OS= MacOS
endif


ifdef Win
    OS = Windows
else ifdef Mac
    OS= MacOS
else 
    OS = Linux
endif
```

**ifndef** 判断一个变量是否没被定义

```makefile
ifndef FLAGS
    FLAGS = -finput-charset=utf-8
endif
```

**ifeq** 判断两个值是否相等

```makefile
version = 3.0

ifeq ($(version),1.0)            # ifeq后一定要一个空格
    msg := 版本太旧了，请更新版本
else ifeq ($(version), 3.0)
    msg := 版本太新了，也不行
else
    msg := 版本可以用
endif


# 另外的写法
msg = Other
ifeq "$(OS)" "Windows_NT"
    msg = This is a Windows Platform
endif

ifeq '$(OS)' 'Windows_NT'

ifeq '$(OS)' "Windows_NT"
```

**ifneq** 判断两个值是否不等

用法及参数同ifeq，只是判断结果相反











# 文本处理函数

C语言中，函数调用方法是function(arguments)；但在Makefile中调用函数的写法不同

```makefile
$(function arguments) 或 ${function arguments}
$(function arg1,$(arg2),arg3 ...)  # 参数之间不要有空格
```

## 字符替换与分析

#### **subst**

文本替换函数，返回替换后的文本

```makefile
$(subst target,replacement,text)
        --- 用relacement替换text中的target
        --- target 需要替换的内容
        --- replacement 替换为的内容
        --- text 需要处理的内容，可以是任意字符串



objs = main.o hello.o
srcs = $(subst .o,.cpp,$(objs))
headers = $(subst .cpp,.h,$(srcs))

all:
    @echo $(srcs)
    @echo $(headers)
```

**patsubst**

模式替换， 返回替换后的文本

```makefile
$(patsubst pattern,replacement,text)
        --- pattern 需要替换的模式
        --- replacement 需要替换为
        --- text 待处理内容，各项内容需要用空格隔开


objs = main.ohello.o
srcs = $(subst %.o,%.cpp,$(objs))
headers = $(subst %.cpp,%.h,$(srcs))    
```

#### **strip**

去除字符串头部和尾部的空格，中间如果连续有多个空格，则用一个空格替换，返回去除空格后的文本

```makefile
$(strip string)
        --- string 需要去除空格的字符串


files = aa hello.cpp      main.cpp     test.cpp
files := $(subst aa,        ,$(files))
files2 = $(strip $(files))
```

#### findstring

查找字符串，如果找到了，则返回对应的字符串，如果没找到，则反回空串

```makefile
$(findstring find,string)
        --- find 需要查找的字符串
        --- string 用来查找的内容

files = hello.cpp main.cpp test.cpp
find = $(findstring hel,$(files))
find = $(findstring HEL,$(files))
```

#### filter

从文本中筛选出符合模式的内容并返回

```makefile
$(filter pattern…,text)
        --- pattern 模式，可以有多个，用空格隔开
        --- text 用来筛选的文本，多项内容需要用空格隔开，否则只会当一项来处理

files = hello.cpp main.cpp test.cpp main.o hello.o hello.h
files2 = $(filter %.o %.h,$(files))
```

#### filter-out

与filter相反，过滤掉符合模式的，返回剩下的内容

```makefile
$(filter-out pattern…,text)
        --- pattern 模式，可以有多个，用空格隔开
        --- text 用来筛选的文本，多项内容需要用空格隔开，否则只会当一项来处理


files = hello.cpp main.cpp test.cpp main.o hello.o hello.h
files2 = $(filter-out %.o %.cpp,$(files))
```

#### sort

将文本内的各项按字典顺序排列，并且移除重复项

```makefile
$(sort list)
        --- list 需要排序内容


files = hello.cpp main.cpp test.cpp main.o hello.o hello.h main.cpp hello.cpp
files2 = $(sort $(files))
```

#### word

用于返回文本中第n个单词

```makefile
$(word n,text)
        --- n 第n个单词，从1开始，如果n大于总单词数，则返回空串
        --- text 待处理文本

files = hello.cpp main.cpp test.cpp main.o hello.o hello.h main.cpp hello.cpp
files2 = $(word 3,$(files))
```

#### wordlist

用于返回文本指定范围内的单词列表

```makefile
$(wordlist start,end,text)
        --- start 起始位置，如果大于单词总数，则返回空串
        --- end 结束位置，如果大于单词总数，则返回起始位置之后全部，如果start > end，什么都不返回

files = hello.cpp main.cpp test.cpp main.o hello.o hello.h main.cpp hello.cpp
files2 = $(wordlist 3,6,$(files))
```

#### words

返回文本中单词数

```makefile
$(words text)
        --- text 需要处理的文本


files = hello.cpp main.cpp test.cpp main.o hello.o hello.h main.cpp hello.cpp
nums = $(words $(files))
```

#### firstword

返回第一个单词

```makefile
$(firstword text)
```

#### lastword

返回最后一个单词

```makefile
$(lastword text)
```

## 文件名处理函数

#### dir

返回文件目录

```makefile
$(dir files)
        --- files 需要返回目录的文件名，可以有多个，用空格隔开

files = src/hello.cpp main.cpp

files2 = $(dir $(files))
```

#### notdir

返回除目录部分的文件名

```makefile
$(notdir files)
        --- files 需要返回文件列表，可以有多个，用空格隔开

files = src/hello.cpp main.cpp
files2 = $(notdir $(files))
```

#### suffix

返回文件后缀名，如果没有后缀返回空

```makefile
$(suffix files)
        --- files 需要返回后缀的文件名，可以有多个，用空格隔开


files = src/hello.cpp main.cpp hello.o hello.hpp hello
files2 = $(suffix $(files))
```

#### basename

返回文件名除后缀的部分

```makefile
$(basename files)
        --- files 需要返回的文件名，可以有多个，用空格隔开


files = src/hello.cpp main.cpp hello.o hello.hpp hello
files2 = $(basename $(files))
```

#### addsuffix

给文件名添加后缀

```makefile
$(addsuffix suffix,files)
        --- suffix 需要添加的后缀
        --- files 需要添加后缀的文件名，可以有多个，用空格隔开

files = src/hello.cpp main.cpp hello.o hello.hpp hello
files2 = $(addsuffix .exe,$(files))
```

#### addprefix

给文件名添加前缀

```makefile
$(addprefix prefix,files)
        --- prefix 需要添加的前缀
        --- files 需要添加前缀的文件名，可以有多个，用空格隔开

files = src/hello.cpp main.cpp hello.o hello.hpp hello
files2 = $(addprefix make/,$(files))
```

#### join

将两个列表中的内容一对一连接，如果两个列表内容数量不相等，则多出来的部分原样返回

```makefile
$(join list1,list2)
        --- list1 第一个列表
        --- list2 需要连接的第二个列表


f1 = hello main test
f2 = .cpp .hpp
files2 = $(join $(f1),$(f2))
```

#### wildcard

返回符合通配符的文件列表

```makefile
$(wildcard pattern)
        --- pattern 通配符

files2 = $(wildcard *.cpp)
files2 = $(wildcard *)
files2 = $(wildcard src/*.cpp)
```

#### realpath

返回文件的绝对路径

```makefile
$(realpath files)
        --- files 需要返回绝对路径的文件，可以有多个，用空格隔开

f3 = $(wildcard src/*)
files2 = $(realpath $(f3))
```

#### abspath

返回绝对路径，用法同realpath，如果一个文件名不存在，realpath不会返回内容，abspath则会返回一个当前文件夹一下的绝对路径

```makefile
$(abspath files)
```

## 条件函数

#### if

条伯判断，如果条件展开不是空串，则反回真的部分，否则返回假的部分

```makefile
$(if condition,then-part[,else-part])
        --- condition 条件部分
        --- then-part 条件为真时执行的部分
        --- else-part 条件为假时执行的部分，如果省略则为假时返回空串

files = src/hello.cpp main.cpp hello.o hello.hpp hello
files2 = $(if $(files),有文件,没有文件)
```

#### or

返回条件中第一个不为空的部分

```makefile
$(or condition1[,condition2[,condition3…]])

f1 = 
f2 = 
f3 = hello.cpp
f4 = main.cpp
files2 = $(or $(f1),$(f2),$(f3),$(f4))
```

#### and

如果条件中有一个为空串，则返回空，如果全都不为空，则返回最后一个条件

```makefile
$(and condition1[,condition2[,condition3…]])

f1 = 12
f2 = 34
f3 = hello.cpp
f4 = main.cpp
files2 = $(and $(f1),$(f2),$(f3),$(f4))
```

#### intcmp

比较两个整数大小，并返回对应操作结果（GNU make 4.4以上版本）

```makefile
$(intcmp lhs,rhs[,lt-part[,eq-part[,gt-part]]]) 
        --- lhs 第一个数
        --- rhs 第二个数
        --- lt-part  lhs < rhs时执行
        --- eq-part  lhs = rhs时执行
        --- gt-part  lhs > rhs时执行
        --- 如果只提供前两个参数，则lhs == rhs时返回数值，否则返回空串
            参数为lhs,rhs,lt-part时，当lhs < rhs时返回lt-part结果，否则返回空
            参数为lhs,rhs,lt-part,eq-part，lhs < rhs返回lt-part结果，否则都返回eq-part结果
            参数全时，lhs < rhs返回lt-part，lhs == rhs返回eq-part, lhs > rhs返回gt-part



@echo $(intcmp 2,2,-1,0,1)
```

## file

读写文件

```makefile
$(file op filename[,text])
        --- op 操作
                > 覆盖
                >> 追加
                < 读
        --- filename 需要操作的文件名
        --- text 写入的文本内容，读取是不需要这个参数


files = src/hello.cpp main.cpp hello.o hello.hpp hello
write = $(file > makewrite.txt,$(files))
read = $(file < makewrite.txt)
```

## foreach

对一列用空格隔开的字符序列中每一项进行处理，并返回处理后的列表

```makefile
$(foreach each,list,process)
        --- each list中的每一项
        --- list 需要处理的字符串序列，用空格隔开
        --- process 需要对每一项进行的处理

list = 1 2 3 4 5
result = $(foreach each,$(list),$(addprefix cpp,$(addsuffix .cpp,$(each))))
```

作用类似C/C++中的循环

```c++
int list[5] = {1, 2, 3, 4, 5};
int result[5];
int each;
for(int i = 0; i < 5; i++)
{
    each = list[i];
    result[i] = process(each);
}
// 此时result即为返回结果
```

## call

将一些复杂的表达式写成一个变量，用call可以像调用函数一样进行调用。类似于编程语言中的自定义函数。在函数中可以用$(n)来访问第n个参数

```makefile
$(call funcname,param1,param2,…)
        --- funcname 自定义函数（变量名）
        --- 参数至少一个，可以有多个，用逗号隔开

dirof =  $(dir $(realpath $(1))) $(dir $(realpath $(2)))
result = $(call dirof,main.cpp,src/hello.cpp)
```

## value

对于不是立即展开的变量，可以查看变量的原始定义；对于立即展开的变量，直接返回变量值

```makefile
$(value variable)

var = value function test
var2 = $(var)
var3 := $(var)
all:
    @echo $(value var2)
    @echo $(value var3)
```

## 

查看一个变量定义来源

```makefile
$(origin variable)


var2 = origin function 
all:
    @echo $(origin var1)    # undefined 未定义
    @echo $(origin CC)        # default 默认变量
    @echo $(origin JAVA_HOME) # environment 环境变量
    @echo $(origin var2)    # file 在Makefile文件中定义的变量
    @echo $(origin @)        # automatic 自动变量
```

## flavor

查看一个变量的赋值方式

```makefile
$(flavor variable)

var2 = flavor function
var3 := flavor funciton
all:
    @echo $(flavor var1)    # undefined 未定义
    @echo $(flavor var2)    # recursive 递归展开赋值
    @echo $(flavor var3)    # simple 简单赋值
```

## eval

可以将一段文本生成Makefile的内容

```makefile
$(eval text)

define eval_target = 
eval:
    @echo Target Eval Test
endef

$(eval $(eval_target))
```

以上，运行make时将会执行eval目标

## shell

用于执行Shell命令

```makefile
files = $(shell ls *.cpp)
$(shell echo This is from shell function)
```

## let

将一个字符串序列中的项拆开放入多个变量中，并对各个变量进行操作（GNU make 4.4以上版本）

```makefile
$(let var1 [var2 ...],[list],proc)
        --- var 变量，可以有多个，用空格隔开
        --- list 待处理字符串，各项之间空格隔开
        --- proc 对变量进行的操作，结果为let的返回值
            将list中的值依次一项一项放到var中，如果var的个数多于list项数，那多出来的var是空串。如果
            var的个数小于list项数，则先依次把前而的项放入var中，剩下的list所有项都放入最后一个var中


list = a b c d
letfirst = $(let first second rest,$(list),$(first))
letrest = $(let first second rest,$(list),$(rest))


# 结合call可以对所有项进行递归处理
reverse = $(let first rest,$(1),$(if $(rest),$(call reverse,$(rest)) )$(first))
all: ; @echo $(call reverse,d c b a)
```

## 信息提示函数

#### error

提示错误信息并终止make执行

```makefile
$(error text)
        --- text 提示信息

EXIT_STATUS = -1
ifneq (0, $(EXIT_STATUS))
    $(error An error occured! make stopped!)
endif
```

#### warning

提示警告信息，make不会终止

```makefile
$(warning text)

ifneq (0, $(EXIT_STATUS))
    $(warning This is a warning message)
endif
```

#### info

输出一些信息

```makefile
$(info text…)

$(info 编译开始.......)
$(info 编译结束)
```

# 同一项目中有多个Makefile文件

## 包含其他makefile文件

使用`include`指令可以读入其他makefile文件的内容，效果就如同在include的位置用对应的文件内容替换一样。

```makefile
include mkf1 mkf2 # 可以引入多个文件，用空格隔开
include *.mk    # 可以用通配符，表示引入所有以.mk结尾的文件
```

如果找不到对应文件，则会报错，如果要忽略错误，可以在`include`前加`-`

```makefile
-include mkf1 mkf2
```

#### 应用实例：自动生成依赖

```makefile
objs = block.o command.o input.o main.o scene.o test.o

sudoku: $(objs)
    g++ $(objs) -o sudoku

include $(objs:%.o=%.d)

%.d: %.cpp
    @-rm $@
    $(CXX) -MM  $< > $@.temp
    @sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.temp > $@
    @-rm $@.temp


%.o : %.cpp
    g++ -c $<
    @echo $^
```

## 嵌套make

如果将一个大项目分为许多小项目，则可以使用嵌套（递归）使用make。具体做法为，写一个总的Makefile，然后在每个子项目中都写一个Makefile，在总Makefile中进行调用。

例如，可以把sudoku项目中除main.cpp，test.cpp外的其他cpp存为一个子项目，编译为一个库文件，main.cpp test.cpp为另一个子项目，编译为.o然后链接库文件成可执行文件：

库文件Makefile

```makefile
vpath %.h ../include

CXXFLAGS += -I../include -fexec-charset=GBK -finput-charset=UTF-8

cpps := $(wildcard *.cpp)
objs := $(cpps:%.cpp=%.o)

libsudoku.a: $(objs)
    ar rcs $@ $^

$(objs): %.o : %.cpp %.h
```

main.cpp test.cpp的Makefile

```makefile
CXXFLAGS += -I../include -fexec-charset=GBK -finput-charset=UTF-8
vpath %.h ../include
vpath %.a ../lib

../sudoku: main.o test.o -lsudoku
    $(CXX) -o $@ $^
```

总的Makefile

```makefile
.PHONY: all clean

all: subsrc

subsrc: sublib
    $(MAKE) -C src

sublib:
    $(MAKE) -C lib

clean:
    -rm *.exe src/*.o lib/*.o lib/*.a 
```

其中

```makefile
$(MAKE) -C subdir
```

这一指令会自动进入subdir文件夹然后执行make。

可以通过`export`指令向子项目的make传递变量。

```makefile
export var  # 传递var
export         # 传递所有变量
unexport    # 取消传递
```







=？
# 后续学习过程

读一些开源项目的Makefile

**redis**:https://github.com/redis/redis

**ffmpeg**:https://github.com/FFmpeg/FFmpeg

**aubio**:https://github.com/aubio/aubio

**libav**:https://github.com/libav/libav

**OpenH264**:https://github.com/cisco/openh264

**TinyVM**:https://github.com/jakogut/tinyvm

**TinyXML2**:https://github.com/leethomason/tinyxml2
