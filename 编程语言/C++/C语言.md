# C语言概述

## C语言

**计算机结构组成**



<img src="https://article.biliimg.com/bfs/article/beb942dd4f59cf8f9ca102691aff1df938716159.png" alt="image.png" style="zoom:60%;" />

## 为什么要学习C语言


优点
- 代码量小
- 执行速度快
- 功能强大
- 编程自由

缺点
- 写代码实现周期长
- 可移植性较差
- 过于自由，经验不足易出错
- 对平台库依赖较多 

**C语言应用领域**
C语言的应用极其广泛，从网站后台，到底层操作系统，从多媒体应用到大型网络游戏，均可使用C语言来开发：
- C语言可以写网站后台程序
- C语言可以专门针对某个主题写出功能强大的程序库
- C语言可以写出大型游戏的引擎
- C语言可以写出另一个语言来
- C语言可以写操作系统和驱动程序，并且只能用C语言编写
- 任何设备只要配置了微处理器，就都支持C语言。从微波炉到手机，都是由C语言技术来推动的


### C语言关键字

32个关键字：(由系统定义，不能重作其它定义) 


| 更新缓冲区  |    2     |           3            |             4              |
| :----: | :------: | :--------------------: | :------------------------: |
|  auto  |  break   |          case          |            char            |
| const  | continue |        default         |             do             |
| double |   else   |          enum          |           extern           |
| float  |   for    |          goto          |            int             |
|  long  | register |         return         |           short            |
| signed |  sizeof  |         static         |           struct           |
| switch | typedef  | unsigned union    void | volatile&nbsp; &nbsp;while |


### C语言运算符

34种运算符： 

根据下面的内容写一个n列的markdown图表,默认文字居中,符号都加 ``
- 算术运算符：`+ - * / % ++ -- `
- 关系运算符：`< <= == > >= !=` 
- 逻辑运算符：`! && ||` 
- 位运算符 : `<<  >> ~ | ^ &` 
- 赋值运算符: `= 及其扩展`
- 条件运算符:`?:` 
- 逗号运算符:`,`
- 指针运算符:`* &`
- 求字节数 : `sizeof` 
- 强制类型转换:(类型) 
- 分量运算符:`. ->` 
- 下标运算符:`[]`
- 其它: `()` 

### 第一个C语言程序：HelloWorld

```c
#include <stdio.h>

int main()
{
  printf("hello world\n");
  return 0;
}
```

C语言的源代码文件是一个普通的文本文件，<font color=#ff0000>但扩展名必须是.c</font>。

#### 通过gcc编译C代码

##### gcc编译器介绍
编辑器(如vi、记事本)是指我用它来写程序的(编辑代码)，而我们写的代码语句，电脑是不懂的，我们需要把它**转成电脑能懂的语句**，编译器就是这样的转化工具。就是说，我们用编辑器编写程序，由编译器编译后才可以运行！
编译器是将易于编写、阅读和维护的高级计算机语言**翻译**为计算机能解读、运行的低级机器语言的程序。
gcc(`GNU Compiler Collection`，GNU 编译器套件)，是由 GNU 开发的编程语言编译器。gcc原本作为GNU操作系统的官方编译器，现已被大多数类Unix操作系统(如Linux、BSD、Mac OS X等)采纳为标准的编译器，gcc同样适用于微软的Windows。

gcc最初用于编译C语言，随着项目的发展gcc已经成为了能够编译C、C++、Java、Ada、fortran、Object C、Object C++、Go语言的编译器大家族。

编译命令格式:

```shell
gcc [-option1] ... <filename>
g++ [-option1] ... <filename>
```

- 命令、选项和源文件之间使用<font color=#ff0000>空格</font>分隔
- 一行命令中可以有<font color=#ff0000>零个、一个或多个</font>选项
- 文件名可以包含文件的<font color=#ff0000>绝对路径</font>，也可以使用<font color=#ff0000>相对路径</font>
- 如果命令中不包含输出可执行文件的文件名，可执行文件的文件名会自动生成一个<font color=#ff0000>默认名</font>，Linux平台为a.out，Windows平台为a.exe

**gcc、g++编译常用选项说明**：

|  **选项**   |          **含义**           |
| :-------: | :-----------------------: |
| `-o file` |    指定生成的**输出文件名**为file    |
|   `-E`    |          只进行预处理           |
| `-S`(大写)  |         只进行预处理和编译         |
| `-c`(小写)  |       只进行预处理、编译和汇编        |
|   `-g`    | 用来生成调试信息(会比原来的`a.out`文件大) |

```shell
gcc main.c
$ ls
a.out  main.c  main.cpp  out
$ ./a.out
hello world
gcc main.c -o main
$ ls
main  main.c  main.cpp  out
```


##### gcc编译过程

GCC的编译流程分为四个步骤：
1. 预处理(Pre-Processing)
2. 编译(Compiling)
3. 汇编 (Assembliang)
4. 链接(Linking)


<img src="https://article.biliimg.com/bfs/article/029b7511941307c3f601a81f0bdf875638716159.png" alt="image.png" style="zoom:70%;" />


**预处理**(Pre-Processing)  
预处理用于将所有的`#开头`预编译指令以及宏定义**替换成其真正的内容**，预处理之后得到的仍然是文本文件，但文件体积会大很多。  
1. 头文件展开。 --- 不检查语法错误。 可以展开任意文件。
	可以加任何头文件，例如xxx.txt
2. 宏定义替换。 --- 将宏名替换为宏值。
	`#define PI 3.14` // 定义常量 PI 宏定义
3. 替换注释。	--- 将代码中的注释变成空行
4. 展开条件编译 --- 根据条件来展开(是否包含在源代码中)指令。
	`#ifdef PI ` //如果定义了PI变量
	xxxxx
	`#endif` //结束语

预处理程序也有它的特殊性，暂时没有办法调试，可以让 GCC 使用“-E”选项，略过后面的编译链接，只输出预处理后的源码，比如：

```bash
g++ test03.cpp -E -o a.cxx #输出预处理后的源码
```


**编译**(Compiling)  
将预处理之后的程序转换为特定的**汇编代码**(assembly code) 的过程。  
1. 逐行检查语法错误。【重点】	--- 整个编译4步骤中<font color=#ff0000>最耗时</font>的过程。
2. 语法和语义优化
3. 将C程序翻译成汇编指令，得到.s 汇编文件。


**汇编** (Assembliang)  
汇编过程将上一步的汇编代码转成机器码(machine code)，这一步产生的文件叫目标文件(`*.o`),是**二进制格式的可重定位目标文件**。此步骤会为每一个源文件产生一个目标文件。
- `*.o`文件的格式组成
	- elf文件头
	- .text
	- .data
	- .bss
	- .svmbal
	- .section table



**链接**(Linking)  
链接过程将多个目标文件(main.o,sum.o)以及所需的库文件(.so等)**链接**成最终可执行文件(executable file)。  
1. 所有的`.o文件`段合并(你的.text和我的.text...合并)，符号表合并后，进行**符号解析**(所有对符号的引用，都要找到符号定义的地方，就是找那些`*UND*`)
2. 数据地址回填
3. 库引入
4. 符号的重定位(重定向)
	- 在编译阶段，每个源文件被单独编译成一个对象文件。如果源文件中有对全局变量、函数、或其他模块中定义的符号的引用，则这些引用在编译时不会被分配一个最终地址，而是先标记为未解析的(地址都是00)。
	- 链接器开始将所有对象文件合并成一个可执行文件时，首先会为每个段(比如代码段 .text、数据段 .data 等)分配地址空间。它会合并同类的段，并给每个段中的符号分配一个**最终地址**
	- 链接器根据重定位表中的信息，更新那些之前只有偏移量或者暂时地址的符号引用。链接器会把每个符号的引用更新为它最终的地址。


```shell
objdump -t main.o

main.o:     file format elf64-x86-64
SYMBOL TABLE:
0000000000000000 l    df *ABS*  0000000000000000 main.cpp
0000000000000000 l    d  .text  0000000000000000 .text
0000000000000000 l    d  .bss   0000000000000000 .bss
0000000000000000 l     O .bss   0000000000000001 _ZStL8__ioinit
0000000000000033 l     F .text  0000000000000052 _Z41__static_initialization_and_destruction_0ii
0000000000000085 l     F .text  0000000000000015 _GLOBAL__sub_I_data
0000000000000000 g     O .data  0000000000000004 data
0000000000000000 g     F .text  0000000000000033 main
0000000000000000         *UND*  0000000000000000 gdata
0000000000000000         *UND*  0000000000000000 _Z3sumii
0000000000000000         *UND*  0000000000000000 _ZNSt8ios_base4InitC1Ev
0000000000000000         *UND*  0000000000000000 .hidden __dso_handle
0000000000000000         *UND*  0000000000000000 _GLOBAL_OFFSET_TABLE_
0000000000000000         *UND*  0000000000000000 _ZNSt8ios_base4InitD1Ev
0000000000000000         *UND*  0000000000000000 __cxa_atexit
```

我们用objdump命令查看查看目标文件(Object File)的符号表。
根据[[零散知识点#虚拟地址空间|虚拟地址空间]]的知识，main和data的位置我们都很容易理解，`gdata` 和 `_Z3sumii`(`sum` 函数的经过名称修饰(mangling)后的名字)都是未定义的(`_UND_`)，这意味着它们在这个对象文件中被引用，但定义在其他地方。

```shell
g++ -E test.cpp -o test.i  # 预处理 
$ ls 
test.cpp test.i  
g++ -S test. i-o test.s  # 编译 
ls 
test.cpp test.i test.s 
g++ -c test.s -o test.o  # 汇编 
ls 
test.cpp test.i test.o test.s 
g++ -o test test.o # 链接 
ls 
test test.cpp test.i test.o test.s 
./test # 可执行文件 
hello world 
```


```shell
objdump -t sum.o

sum.o:     file format elf64-x86-64

SYMBOL TABLE:
0000000000000000 l    df *ABS*  0000000000000000 sum.cpp
0000000000000000 l    d  .text  0000000000000000 .text
0000000000000000 g     O .data  0000000000000004 gdata
0000000000000000 g     F .text  0000000000000014 _Z3sumii
```

##### g++重要的编译参数

1.  `-O[n]` 优化源代码

```sh
## 所谓优化，例如省略掉代码中从未使用过的变量、直接将常量表达式用结果值代替等等，这些操作
会缩减目标文件所包含的代码量，提高最终生成的可执行文件的运行效率。
# -O 选项告诉 g++ 对源代码进行基本优化。这些优化在大多数情况下都会使程序执行的更快。 -O2
选项告诉 g++ 产生尽可能小和尽可能快的代码。 如-O2，-O3，-On(n 常为0–3)
# -O 同时减小代码的长度和执行时间，其效果等价于-O1
# -O0 表示不做优化
# -O1 为默认优化
# -O2 除了完成-O1的优化之外，还进行一些额外的调整工作，如指令调整等。
# -O3 则包括循环展开和其他一些与处理特性相关的优化工作。
# 选项将使编译的速度比使用 -O 时慢， 但通常产生的代码执行速度会更快。
# 使用 -O2优化源代码，并输出可执行文件
g++ -O2 test.cpp
```


```c++
int main() {
  unsigned long int counter;
  unsigned long int result;
  unsigned long int temp;
  unsigned int five;
  int i;
  for (counter = 0; counter < 2009 * 2009 * 100 / 4 + 2010;
       counter += (10 - 6) / 4) {
    temp = counter / 1979;
    for (i = 0; i < 20; i++) {
      // 每一次循环 都会进行复杂的计算
      five = 200 * 200 / 8000;
      result = counter;
    } 
  }
   cout << "result= " << result << endl;
}
```

上面的代码每一步都在多余的常数的加减乘除，整个是低效的

```sh
yesho@:~/Code/C/GDB$ time ./a_without_o 
result= 100904034
real    0m2.048s
user    0m2.048s
sys     0m0.000s
yesho@:~/Code/C/GDB$ time ./a_with_o2 
result= 100904034
real    0m0.002s
user    0m0.001s
sys     0m0.001s
```
上面可以看到优化与不优化的时间差距

2. l 和 -L 指定库文件 | 指定库文件路径

```sh
# -l参数(小写)就是用来指定程序要链接的库，-l参数紧接着就是库名
# 在/lib和/usr/lib和/usr/local/lib里的库直接用-l参数就能链接
# 链接glog库
g++ -lglog test.cpp
# 如果库文件没放在上面三个目录里，需要使用-L参数(大写)指定库文件所在目录
# -L参数跟着的是库文件所在的目录名
# 链接mytest库，libmytest.so在/home/bing/mytestlibfolder目录下
g++ -L/home/bing/mytestlibfolder -lmytest test.cpp
```

3. -I 指定头文件搜索目录

```sh
# -I
# /usr/include目录一般是不用指定的，gcc知道去那里找，但 是如果头文件不在/usr/icnclude
里我们就要用-I参数指定了，比如头文件放在/myinclude目录里，那编译命令行就要加上-
I/myinclude 参数了，如果不加你会得到一个”xxxx.h: No such file or directory”的错
误。-I参数可以用相对路径，比如头文件在当前 目录，可以用-I.来指定。上面我们提到的–cflags参
数就是用来生成-I参数的。
g++ -I/myinclude test.cpp
```

4. -Wall 打印警告信息
```sh
# 打印出gcc提供的警告信息
g++ -Wall test.cpp
```

5. -w 关闭警告信息
```sh
# 关闭所有警告信息
g++ -w test.cpp
```

6. -std=c++11 设置编译标准
```sh
# 使用 c++11 标准编译 test.cpp
g++ -std=c++11 test.cpp
```

7. -o 指定输出文件名
```sh
# 指定即将产生的文件名
# 指定输出可执行文件名为test
g++ test.cpp -o test
```

8. -D 定义宏
```sh
# 在使用gcc/g++编译的时候定义宏
# 常用场景：
# -DDEBUG 定义DEBUG宏，可能文件中有DEBUG宏部分的相关信息，用个DDEBUG来选择开启或关闭
DEBUG
```

示例代码：

```c
// -Dname 定义宏name,默认定义内容为字符串“1”
#include <stdio.h>
int main()
{
  #ifdef DEBUG
 printf("DEBUG LOG\n");
  #endif
 printf("in\n");
}
// 1. 在编译的时候，使用gcc -DDEBUG main.cpp
// 2. 第七行代码可以被执行
```

#### gdb调试器
**前言**：
- GDB(GNU Debugger)是一个用来调试C/C++程序的功能强大的调试器，是Linux系统开发
- C/C++最常用的调试器
- 程序员可以使用GDB来跟踪程序中的错误，从而减少程序员的工作量。
- Linux 开发C/C++ 一定要熟悉 GDB
- VSCode是通过调用GDB调试器来实现C/C++的调试工作的；

> Windows 系统中，常见的集成开发环境(IDE)，如 VS、VC等，它们内部已经嵌套了相应的调试器

**GDB主要功能**：
- 设置断点(断点可以是条件表达式)
- 使程序在指定的代码行上暂停执行，便于观察
- 单步执行程序，便于调试
- 查看程序中变量值的变化
- 动态改变程序的执行环境
- 分析崩溃程序产生的core文件

##### 常用调试命令参数
调试开始：执行`gdb [exefilename]` ，进入gdb调试程序，其中exefilename为要调试的可执行文件名

```sh
## 以下命令后括号内为命令的简化使用，比如run(r)，直接输入命令 r 就代表命令run
$(gdb)help(h) # 查看命令帮助，具体命令查询在gdb中输入help + 命令
$(gdb)run(r) # 重新开始运行文件(run-text：加载文本文件，run-bin：加载二进制文
件)
$(gdb)start # 单步执行，运行程序，停在第一行执行语句
$(gdb)list(l) # 查看原代码(list-n,从第n行开始查看代码。list+ 函数名：查看具体函数)
$(gdb)set # 设置变量的值
$(gdb)next(n)  # 单步调试(逐过程，函数直接执行)
$(gdb)step(s) # 单步调试(逐语句：跳入自定义函数内部执行)
$(gdb)backtrace(bt) # 查看函数的调用的栈帧和层级关系
$(gdb)frame(f) # 切换函数的栈帧
$(gdb)info(i) # 查看函数内部局部变量的数值
$(gdb)finish # 结束当前函数，返回到函数调用点
$(gdb)continue(c) # 继续运行
$(gdb)print(p) # 打印值及地址
$(gdb)quit(q) # 退出gdb
$(gdb)break+num(b) # 在第num行设置断点
$(gdb)info breakpoints # 查看当前设置的所有断点
$(gdb)dump 行号 # 跳到指定断点位置
$(gdb)delete breakpoints num(d) # 删除第num个断点
$(gdb)display # 追踪查看具体变量值
$(gdb)undisplay # 取消追踪观察变量
$(gdb)watch # 被设置观察点的变量发生修改时，打印显示
$(gdb)i watch # 显示观察点
$(gdb)enable breakpoints # 启用断点
$(gdb)disable breakpoints # 禁用断点
$(gdb)x # 查看内存x/20xw 显示20个单元，16进制，4字节每单元
$(gdb)run argv[1] argv[2] # 调试时命令行传参
$(gdb)set follow-fork-mode child#Makefile项目管理：选择跟踪父子进程(fork())
$(gdb)thread <线程号>：切换到指定线程。例如，thread 2 将切换到线程号为 2 的线程。
$(gdb)info threads：显示当前程序中所有线程的信息和编号。
$(gdb)thread apply all <命令>：对所有线程执行相同的命令。
$(gdb)thread next 或 thread step: 切换到下一个可执行的线程。
$(gdb)thread select <条件>: 根据条件选择并切换到相应的线程
```

> Tips:
> 1. 编译程序时需要加上-g，之后才能用gdb进行调试：gcc -g main.c -o main
> 2. 回车键：重复上一命令



```sh
To start the inferior without using a shell, use "set startup-with-shell off".
(gdb) run
Starting program: /home/yesho/Code/C/GDB/a_yes_g 
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
sum = 5050
The program is over.
[Inferior 1 (process 22898) exited normally]
(gdb) break
No default breakpoint address now.
(gdb) break 11
Breakpoint 1 at 0x55555555519d: file debug_demo.cpp, line 11.
(gdb) info breakpoints 
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x000055555555519d in main(int, char**) at debug_demo.cpp:11
(gdb) b 12 16
malformed linespec error: unexpected number, "16"
(gdb) b 12
Breakpoint 2 at 0x55555555519f: file debug_demo.cpp, line 12.
(gdb) info breakpoints 
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x000055555555519d in main(int, char**) at debug_demo.cpp:11
2       breakpoint     keep y   0x000055555555519f in main(int, char**) at debug_demo.cpp:12
```

以下是`disp`属性的不同取值及其含义：

- `keep`：断点将保留，并在每次断点处停止时都会被重新启用。
- `del`：断点将被删除，并且不再起作用。这意味着当程序执行到该断点时，GDB将不会暂停执行。
- `once`：断点会在第一次触发后被自动删除。这意味着当程序执行到该断点时，它只会停止一次，然后断点将被自动删除。


```sh
(gdb) b 12    # 打断点
Breakpoint 1 at 0x119f: file debug_demo.cpp, line 12.
(gdb) r       
Starting program: /home/yesho/Code/C/GDB/a_yes_g 
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

Breakpoint 1, main (argc=1, argv=0x7fffffffd9e8) at debug_demo.cpp:12
12          sum=sum+i;
(gdb) print i
$1 = 1           
```

C语言的源代码文件是一个普通的文本文件，<font color=#ff0000>但扩展名必须是.c</font>。

#### C语言结构

##### include头文件包含
- `#include`的意思是头文件包含，`#include <stdio.h>`代表包含stdio.h这个头文件
- 使用C语言库函数需要提前包含库函数对应的头文件，如这里使用了`printf()`函数，需要包含stdio.h头文件
- 可以通过man 3 printf查看printf所需的头文件
- `#include`”，它的作用是“包含文件”。注意，不是“包含头文件”，而是可以包含任意的文件。使用“#include”可以把源码、普通文本，甚至是图片、音频、视频都引进来。



`#include<>`  与 `#include ""` 的区别：
- `< >` 表示系统直接按系统指定的目录检索
- `""` 表示系统先在 "" 指定的路径(没写路径代表当前路径)查找头文件，如果找不到，再按系统指定的目录检索
- 编写一些代码片段，存进“`.inc`”文件里，然后有选择地加载，用得好的话，可以实现“源码级别的抽象”。



```shell
cd /usr/include/
$ pwd
/usr/include
$ ls -l | grep stdio.h
-rw-r--r--  1 root root  31526 Jul 14 02:07 stdio.h
```

##### main函数
- 一个完整的C语言程序，是由一个、且只能有一个`main()`函数(又称主函数，必须有)和若干个其他函数结合而成(可选)。
- main函数是C语言程序的入口，程序是从main函数开始执行。

##### {} 括号，程序体和代码块
- {}叫<font color=#ff0000>代码块</font>，一个代码块内部可以有一条或者多条语句
- C语言每句可执行代码都是";"分号结尾
- 所有的#开头的行，都代表<font color=#ff0000>预编译</font>指令，预编译指令行结尾是没有分号的
- 所有的<font color=#ff0000>可执行语句</font>必须是在代码块里面

##### 注释
- `//`叫行注释，注释的内容编译器是忽略的，注释主要的作用是在代码中加一些说明和解释，这样有利于代码的阅读
- `//`叫块注释
- 块注释是C语言标准的注释方法
- 行注释是从C++语言借鉴过来的

##### printf函数
- printf是C语言库函数，功能是向<font color=#ff0000>标准输出设备</font>输出一个<font color=#ff0000>字符串</font>
- `printf(“hello world\n”)`;//`\n`的意思是回车换行

##### return语句
- return代表函数执行完毕，返回return代表<font color=#ff0000>函数的终止</font>
- 如果main定义的时候前面是int，那么return后面就需要写一个整数；如果main定义的时候前面是void，那么return后面什么也不需要写
- 在main函数中return 0代表程序执行成功，return -1代表程序执行失败
- `int main()`和`void main()`在C语言中是一样的，但C++只接受int main这种定义方式

#### system函数

```c
#include <stdlib.h>
int system(const char command);
```

功能：在已经运行的程序中执行<font color=#ff0000>另外一个外部程序</font>
参数：外部可执行程序名字
返回值：
成功：0
失败：任意数字

执行系统命令。如：pause、cmd、calc(计算器)、mapoint(画图工具)、notepad(记事本) cls(清屏操作)… 

示例代码:
```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
	//system("calc"); //windows平台
	system("ls"); //Linux平台, 需要头文件#include <stdlib.h>
	return 0;
}
```

# 数据类型

## 常量与变量

### 关键字
C的关键字共有32个 
- 数据类型关键字(12个) 
	char, short, int, long, float, double,unsigned, signed, struct, union, enum, void 
- 控制语句关键字(12个) 
	if,else,switch,case,default,for,do,while,break,continue,goto,return 
- 存储类关键字(5个) 
	auto,extern,register,static,const 
- 其他关键字(3个) 
	sizeof,typedef,volatile 

### 数据类型
数据类型的作用：编译器预算对象(变量)分配的内存空间大小。

![[c语言数据类型.km]]

### 常量

常量：
- 在程序运行过程中，其值<font color=#ff0000>不能被改变</font>的量
- 常量一般出现在表达式或赋值语句中

| 整型常量  |   100，200，-100，0    |
| :---: | :-----------------: |
| 实型常量  | 3.14 ， 0.125，-3.123 |
| 字符型常量 |  ‘a’,‘b’,‘1’,‘\n’   |
| 字符串常量 |  “a”,“ab”，“12356”   |

3种方式
1. “hello”、'A'、-10、3.1415926(浮点常量)
2. `#define PI 3.1415`	【强调】：没有分号结束标记。 【推荐】 定义宏： 定义语法： `#define` 宏名 宏值(后期不可以该改变)
3. `const int a = 10`;	定义语法：const 类型名 变量名 = 变量值。const关键字： 被该关键字修饰的变量，表示为只读变量(后期可以通过指针修改)。

#c语言面试点
const和define的区别
const 定义的是变量不是常量，只是这个变量的值不允许改变是常变量,带有类型。编译运行的时候起作用存在**类型检查**。
define 定义的是不带类型的常数，只进行简单的字符替换。在预编译的时候起作用，不存在<font color=#ff0000>类型检查</font>。

区别
1. 编译器处理方式不同
	- `#define `宏是在<font color=#ff0000>预处理阶段</font>展开。
	- const 常量是<font color=#ff0000>编译运行阶段</font>使用。
2. 类型和安全检查不同
	- `#define `宏没有类型，不做任何类型检查，仅仅是展开。
	- `const` 常量有具体的类型，在编译阶段会执行类型检查。
3. 存储方式不同
	- `#define` 宏仅仅是展开，有多少地方使用，就展开多少次，不会<font color=#ff0000>分配内存</font>。(宏定义不分配内存，变量定义分配内存。)
	- const常量会在<font color=#ff0000>内存中分配</font>(可以是堆中也可以是栈中)。

4. const 可以节省空间，避免不必要的内存分配。 例如：
5. c语言的const是一个**只读变量**，并不是一个常量，可通过指针间接修改
6. 非const变量默认为extern。要使const变量能够在其他文件中访问，必须在文件中显式地指定它为extern。
 
顶层const和底层const的区别？
- `顶层const`：顶层表示指针本身是个常量，顶层const在拷贝的时候，因为它本身就是常量，只需要考虑它本身拷出和拷入对象之间类型是否相同；
- `底层const`：底层表示指针所指向的对象是个常量。拷贝时严格要求相同的底层const资格。而底层const只是代表它所指对象时常量，所以在拷贝它的时候还要考虑它指向的对象是否是相同类型。
### 变量

变量：
- 在程序运行过程中，其值可以改变
- 变量在使用前<font color=#ff0000>必须先定义</font>，定义变量前<font color=#ff0000>必须有相应的数据类型</font>

标识符命名规则：
- 标识符不能是关键字
- 标识符只能由字母、数字、下划线组成
- 第一个字符必须为字母或下划线
- 标识符中字母区分大小写

变量特点：
- 变量在编译时为其分配相应的内存空间
- 可以通过其<font color=#ff0000>名字</font>和<font color=#ff0000>地址</font>访问相应内存

<img src="https://article.biliimg.com/bfs/article/e39635c5a88ad9e9c45439ee62262e4538716159.png" alt="image.png" style="zoom:50%;" />

#c语言面试点 
#### 声明和定义区别
- 声明变量不需要建立存储空间，如：extern int a;
- 定义变量需要建立存储空间，如：int b;

[[C语言#^extern|extern后面的用途]]
```c
#include <stdio.h>
int _main_()
{
   //extern 关键字只做声明，不能做任何定义，后面还会学习，这里先了解
   //声明一个变量a，a在这里没有建立存储空间
   extern int a;
   a = 10;   //err, 没有空间，就不可以赋值
   int b = 10;  //定义一个变量b，b的类型为int，b赋值为10
   return 0;

}
```

从广义的角度来讲声明中包含着定义，即<font color=#ff0000>定义是声明的一个特例</font>，所以并非所有的声明都是定义：
- int b 它既是声明，同时又是定义
- 对于 extern b来讲它只是声明不是定义

> 变量的定义：	int a = 40;
> 变量的声明：	1) int a;	 没有变量值的变量定义 叫做声明。
> 2)extern int a; 添加了关键字 extern。(声明的两种方式)
> 1. 变量定义会开辟内存空间。变量声明不会开辟内存空间。
> 2. 变量要想使用必须有定义。
> 当编译器编译程序时，在变量使用之前，必须要看到变量定义。如果没有看到变量定义，编译器会自动找寻一个变量声明提升成为定义。
> 如果该变量的声明前有extern关键字，无法提升。
> 【建议】：定义变量时。尽量不要重名。

使用示例

```c
#include <stdio.h>
#define MAX 10 //声明了一个常量，名字叫MAX，值是10，常量的值一旦初始化不可改

int main()
{
	int a;	//定义了一个变量，其类型为int，名字叫a
	const int b = 10; //定义一个const常量，名为叫b，值为10
	//b = 11; //err,常量的值不能改变
	//MAX = 100;	//err,常量的值不能改变
	a = MAX;//将abc的值设置为MAX的值
	a = 123;
	printf("%d\n", a); //打印变量a的值
	return 0;
}
```

## 整型：int

### 整型变量的定义和输出

|  打印格式   |          含义          |
| :-----: | :------------------: |
|   %d    |  输出一个有符号的10进制int类型   |
| %o(字母o) |     输出8进制的int类型      |
|   %x    | 输出16进制的int类型，字母以小写输出 |
|   %X    | 输出16进制的int类型，字母以大写输出 |
|   %u    |    输出一个10进制的无符号数     |

```c
#include <stdio.h>

int main()
{
	int a = 123;	//定义变量a，以10进制方式赋值为123
	int b = 0567;	//定义变量b，以8进制方式赋值为0567
	int c = 0xabc;	//定义变量c，以16进制方式赋值为0xabc
	printf("a = %d\n", a);
	printf("8进制：b = %o\n", b);
	printf("10进制：b = %d\n", b);
	printf("16进制：c = %x\n", c);
	printf("16进制：c = %X\n", c);
	printf("10进制：c = %d\n", c);
	unsigned int d = 0xffffffff; //定义无符号int变量d，以16进制方式赋值
	printf("有符号方式打印：d = %d\n", d);
	printf("无符号方式打印：d = %u\n", d);
	return 0;
}
```

### 整型变量的输入

```c
#include <stdio.h>

int main()
{
	int a;
	printf("请输入a的值：");
	//不要加“\n”
	scanf("%d", &a);// 使用变量的地址作为参数，以便将输入的值存储到该变量所占据的内存位置上
	printf("a = %d\n", a); //打印a的值
	return 0;
}
```

### short、int、long、long long

|数据类型|占用空间|
|---|---|
|short(短整型)|2字节|
|int(整型)|4字节|
|long(长整形)|Windows为4字节，Linux为4字节(32位)，8字节(64位)|
|long long(长长整形)|8字节|
注意：
- 需要注意的是，整型数据在内存中占的字节数与所选择的操作系统有关。虽然 C 语言标准中没有明确规定整型数据的长度，但 long 类型整数的长度不能短于int类型，short类型整数的长度不能长于int类型。
- 当一个小的数据类型赋值给一个大的数据类型，不会出错，因为编译器会自动转化。但当一个大的类型赋值给一个小的数据类型，那么就可能丢失高位。

|整型常量|所需类型|
|---|---|
|10|代表int类型|
|10l, 10L|代表long类型|
|10ll, 10LL|代表long long类型|
|10u, 10U|代表unsigned int类型|
|10ul, 10UL|代表unsigned long类型|
|10ull, 10ULL|代表unsigned long long类型|


| 打印格式 |           含义           |
| :--: | :--------------------: |
|  f   |       输出short类型        |
|  %d  |        输出int类型         |
| %ld  |        输出long类型        |
| %lld |     输出long long类型      |
| %hu  |   输出unsigned short类型   |
|  %u  |    输出unsigned int类型    |
| %lu  |   输出unsigned long类型    |
| %llu | 输出unsigned long long类型 |


```c
#include <stdio.h>

int main()
{
	short a = 10;
	int b = 10;
	long c = 10l; //或者10L
	long long d = 10ll; //或者10LL

	printf("sizeof(a) = %u\n", sizeof(a));
	printf("sizeof(b) = %u\n", sizeof(b));
	printf("sizeof(c) = %u\n", sizeof(c));
	printf("sizeof(c) = %u\n", sizeof(d));

	printf("short a = %hd\n", a);
	printf("int b = %d\n", b);
	printf("long c = %ld\n", c);
	printf("long long d = %lld\n", d);

	unsigned short a2 = 20u;
	unsigned int b2 = 20u;
	unsigned long c2= 20ul; 
	unsigned long long d2 = 20ull; 

	printf("unsigned short a = %hu\n", a2);
	printf("unsigned int b = %u\n", b2);
	printf("unsigned long c = %lu\n", c2);
	printf("unsigned long long d = %llu\n", d2);

	return 0;
}
```

### 有符号数和无符号数区别

#### 有符号数

有符号数是最高位为符号位，0代表正数，1代表负数。

```C
#include <stdio.h>

int main()
{
	signed int a = -1089474374; //定义有符号整型变量a
	printf("%X\n", a); //结果为 BF0FF0BA
	//B       F      0        F       F     0        B	      A
	//1011 1111 0000 1111 1111 0000 1011 1010
	return 0;
}
```


#### 无符号数

<font color=#ff0000>无符号数最高位不是符号位</font>，而就是数的一部分，无符号数不可能是负数。

```C
#include <stdio.h>

int main()
{
   unsigned int a = 3236958022; //定义无符号整型变量a
   _printf_("%X\n", a); //结果为 C0F00F46
   return 0;
}
```

#### 有符号和无符号整型取值范围

| 数据类型       | 占用空间 | 取值范围                                |
|:--------------:|:--------:|:---------------------------------------:|
| short          |      2字节 |-32768 到 32767 (-$2^{15}$ ~ $2^{15}$-1)|
| int            |      4字节 |-2147483648 到 2147483647 ($-2^{31}$ ~ $2^{31}-1$)|
| long           |      4字节 |-2147483648 到 2147483647 ($-2^{31}$ ~ $2^{31}$-1)|
| unsigned short |      2字节 |0 到 65535 (0 ~ $2^{16}$-1) |
| unsigned int   |      4字节 |0 到 4294967295 (0 ~ $2^{32}$-1)|
| unsigned long  |      4字节 |0 到 4294967295 (0 ~ $2^{32}$-1)|

## sizeof关键字

- sizeof不是函数，所以不需要包含任何头文件，它的功能是<font color=#ff0000>计算一个数据类型</font>的大小，单位为字节
- sizeof的返回值为size_t
- <font color=#ff0000>size_t类型在32位操作系统下是unsigned int，是一个无符号的整数</font>

```c
#include <stdio.h>
int _main_()
{
   int a;
   int b = sizeof(a);//sizeof得到指定值占用内存的大小，单位：字节
   _printf_("b = %d\n", b);
   _size_t_ c = sizeof(a);
   _printf_("c = %u\n", c);//用无符号数的方式输出c的值
   return 0;
}
```

## 字符型：char

### 字符变量的定义和输出
字符型变量用于存储一个单一字符，在 C 语言中用 char 表示，其中每个字符变量都会占用 1 个字节。在给字符型变量赋值时，需要用一对英文半角格式的单引号(`' '`)把字符括起来。

字符变量实际上并不是把该字符本身放到变量的内存单元中去，而是<font color=#ff0000>将该字符对应的 ASCII 编码放到变量的存储单元</font>中。<font color=#ff0000>char的本质就是一个1字节大小的整型。</font>

```c
#include <stdio.h>
int _main_()
{
   char ch = 'a';
   _printf_("sizeof(ch) = %u\n", sizeof(ch));
   _printf_("ch[%%c] = %c\n", ch); //打印字符
   _printf_("ch[%%d] = %d\n", ch); //打印‘a’ ASCII的值
   char A = 'A';
   char a = 'a';
   _printf_("a = %d\n", a);   //97
   _printf_("A = %d\n", A); //65
   _printf_("A = %c\n", 'a' - 32); //小写a转大写A
   _printf_("a = %c\n", 'A' + 32); //大写A转小写a
   ch = ' ';
   _printf_("空字符：%d\n", ch); //空字符ASCII的值为32
   _printf_("A = %c\n", 'a' - ' '); //小写a转大写A
   _printf_("a = %c\n", 'A' + ' '); //大写A转小写a
   return 0;
}
```

### 字符变量的输入

```c
#include <stdio.h>
int _main_()
{
   char ch;
   _printf_("请输入ch的值：");
   //不要加“\n”
   _scanf_("%c", &ch);
   _printf_("ch = %c\n", ch); //打印ch的字符
   return 0;
}
```


[[零散知识点#ASCII对照表|ASCII对照表]]

### 转义字符

<img src="https://article.biliimg.com/bfs/article/d886519ac64ceb6aa193436ce5f6bb4238716159.png" alt="image.png" style="zoom:70%;" />

```c
#include <stdio.h>

int main()
{
	printf("abc");
	printf("\refg\n"); //\r切换到句首， \n为换行键
	printf("abc");
	printf("\befg\n");//\b为退格键， \n为换行键
	printf("%d\n", '\123');// '\123'为8进制转义字符，0123对应10进制数为83
	printf("%d\n", '\x23');// '\x23'为16进制转义字符，0x23对应10进制数为35
	return 0;
}

```

## 实型(浮点型)：float、double

实型变量也可以称为浮点型变量，浮点型变量是用来存储小数数值的。在C语言中， 浮点型变量分为两种： 单精度浮点数(float)、 双精度浮点数(double)， 但是double型变量所表示的浮点数比 float 型变量更精确。
由于浮点型变量是由有限的存储单元组成的，因此只能提供有限的有效数字。在有效位以外的数字将被舍去，这样可能会产生一些误差。
不以f结尾的常量是double类型，以f结尾的常量(如3.14f)是float类型。

```c
#include <stdio.h>

int main()
{
	//传统方式赋值
	float a = 3.14f; //或3.14F
	double b = 3.14;
	printf("a = %f\n", a);
	printf("b = %lf\n", b);
	//科学法赋值
	a = 3.2e3f; //3.21000 = 3200，e可以写E
	printf("a1 = %f\n", a);
	a = 100e-3f; //1000.001 = 0.1
	printf("a2 = %f\n", a);
	a = 3.1415926f;
	printf("a3 = %f\n", a); //结果为3.141593
	return 0;
}
```

## 进制

进制也就是进位制，是人们规定的一种进位方法。 对于任何一种进制—X进制，就表示某一位置上的数运算时是逢X进一位。 十进制是逢十进一，十六进制是逢十六进一，二进制就是逢二进一，以此类推，x进制就是逢x进位。

| 十进制 |  二进制  | 八进制 | 十六进制 |
| :-: | :---: | :-: | :--: |
|  0  |   0   |  0  |  0   |
|  1  |   1   |  1  |  1   |
|  2  |  10   |  2  |  2   |
|  3  |  11   |  3  |  3   |
|  4  |  100  |  4  |  4   |
|  5  |  101  |  5  |  5   |
|  6  |  110  |  6  |  6   |
|  7  |  111  |  7  |  7   |
|  8  | 1000  | 10  |  8   |
|  9  | 1001  | 11  |  9   |
| 10  | 1010  | 12  |  A   |
| 11  | 1011  | 13  |  B   |
| 12  | 1100  | 14  |  C   |
| 13  | 1101  | 15  |  D   |
| 14  | 1110  | 16  |  E   |
| 15  | 1111  | 17  |  F   |
| 16  | 10000 | 20  |  10  |

### 二进制

二进制是计算技术中广泛采用的一种数制。二进制数据是用0和1两个数码来表示的数。它的基数为2，进位规则是“逢二进一”，借位规则是“借一当二”。

当前的计算机系统使用的基本上是二进制系统，数据在计算机中主要是以<font color=#ff0000>补码</font>的形式存储的。

|    术语     |                                        含义                                        |
| :-------: | :------------------------------------------------------------------------------: |
|  bit(比特)  |                  一个二进制代表一位，一个位只能表示0或1两种状态。数据传输是习惯以“位”(bit)为单位。                   |
| Byte(字节)  | 一个字节为8个二进制，称为8位，计算机中存储的<font color=#ff0000>最小单位是字节</font>。数据存储是习惯以“字节”(Byte)为单位。 |
| WORD(双字节) |                                     2个字节，16位                                     |
|   DWORD   |                                 两个WORD，4个字节，32位                                  |
|    1b     |                                     1bit，1位                                      |
|    1B     |                                   1Byte,1字节，8位                                   |
|   1k，1K   |                                       1024                                       |
|  1M(1兆)   |                                1024k, 1024``1024                                 |
|    1G     |                                      1024M                                       |
|    1T     |                                      1024G                                       |
|  1Kb(千位)  |                                  1024bit,1024位                                   |
| 1KB(千字节)  |                                 1024Byte，1024字节                                  |
|  1Mb(兆位)  |                             1024Kb = `1024  1024bit`                             |
| 1MB(兆字节)  |                            1024KB = `1024  1024`Byte                             |

十进制转化二进制的方法：用十进制数除以2，分别取余数和商数，商数为0的时候，将余数倒着数就是转化后的结果。
<img src="https://article.biliimg.com/bfs/article/bb3fe56dd1fa0952fe17c5685f1b287638716159.png" alt="image.png" style="zoom:50%;" />

十进制的小数转换成二进制：小数部分和2相乘，取整数，不足1取0，<font color=#ff0000>每次相乘都是小数部分</font>，顺序看取整后的数就是转化后的结果。
<img src="https://article.biliimg.com/bfs/article/a085f7acd88a0e879e820fddcd63bd2638716159.png" alt="image.png" style="zoom:50%;" />



### 八进制

八进制，Octal，缩写OCT或O，一种以8为基数的计数法，采用0，1，2，3，4，5，6，7八个数字，逢八进1。一些编程语言中常常以数字0开始表明该数字是八进制。
八进制的数和二进制数可以按位对应(<font color=#ff0000>八进制一位对应二进制三位</font>)，因此常应用在计算机语言中。
![image.png](https://article.biliimg.com/bfs/article/1747f08ad5c1b624889fc849e597290938716159.png)

十进制转化八进制的方法：
用十进制数除以8，分别取余数和商数，商数为0的时候，将余数倒着数就是转化后的结果。

<img src="https://article.biliimg.com/bfs/article/5aa8ce6a19ec73de2a8a5078be181b1838716159.png" alt="image.png" style="zoom:50%;" />

### 十六进制

十六进制(英文名称：Hexadecimal)，同我们日常生活中的表示法不一样，它由0-9，A-F组成，<font color=#ff0000>字母不区分大小写</font>。与10进制的对应关系是：0-9对应0-9，A-F对应10-15。

十六进制的数和二进制数可以按位对应(十六进制一位对应二进制四位)，因此常应用在计算机语言中。

<img src="https://article.biliimg.com/bfs/article/1fd1b4b1a3732ece26b66837246d429838716159.png" alt="image.png" style="zoom:50%;" />


十进制转化十六进制的方法：

用十进制数除以16，分别取余数和商数，商数为0的时候，将余数倒着数就是转化后的结果。


<img src="https://article.biliimg.com/bfs/article/ac7a5bc1ce491edc92b18882f0cc689c38716159.png" alt="image.png" style="zoom:50%;" />

### C语言如何表示相应进制数


| 十进制  | 以正常数字1-9开头，如123 |
| ---- | --------------- |
| 八进制  | 以数字0开头，如0123    |
| 十六进制 | 以0x开头，如0x123    |
| 二进制  | C语言不能直接书写二进制数   |
```c
#include <stdio.h>
int main()
{
	int a = 123;		//十进制方式赋值
	int b = 0123;		//八进制方式赋值， 以数字0开头
	int c = 0xABC;	//十六进制方式赋值
	//如果在printf中输出一个十进制数那么用%d，八进制用%o，十六进制是%x
	printf("十进制：%d\n",a );
	printf("八进制：%o\n", b);	//%o,为字母o,不是数字
	printf("十六进制：%x\n", c);
	return 0;
}
```

## 计算机内存数值存储方式

### 原码
一个数的原码(原始的二进制码)有如下特点：
- 最高位做为符号位，0表示正,为1表示负
- 其它数值部分就是数值本身绝对值的二进制数
- 负数的原码是在其绝对值的基础上，最高位变为1

下面数值以1字节的大小描述：

|十进制数|原码|
|---|---|
|+15|0000 1111|
|-15|1000 1111|
|+0|0000 0000|
|-0|1000 0000|
原码表示法简单易懂，与带符号数本身转换方便，只要符号还原即可，但当两个<font color=#ff0000>正数相减</font>或<font color=#ff0000>不同符号数相加</font>时，必须比较两个数哪个绝对值大，才能决定谁减谁，才能确定结果是正还是负，所以原码不便于加减运算。


### 反码

对于正数，反码与原码相同
对于负数，符号位不变，其它部分取反(1变0,0变1)

|十进制数|反码|
|---|---|
|+15|0000 1111|
|-15|1111 0000|
|+0|0000 0000|
|-0|1111 1111|
反码运算也不方便，通常用来作为求补码的中间过渡。

### 补码

在计算机系统中，数值一律用补码来存储。

补码特点：
- 对于正数，原码、反码、补码相同
- 对于负数，其补码为它的反码加1
- 补码符号位不动，其他位求反，最后整个数加1，得到原码

|十进制数|补码|
|---|---|
|+15|0000 1111|
|-15|1111 0001|
|+0|0000 0000|
|-0|0000 0000|

#### 补码的意义
示例1：用8位二进制数分别表示+0和-0

|十进制数|原码|
|---|---|
|+0|0000 0000|
|-0|1000 0000|

|十进制数|反码|
|---|---|
|+0|0000 0000|
|-0|1111 1111|
不管以原码方式存储，还是以反码方式存储，0也有两种表示形式。为什么同样一个0有两种不同的表示方法呢？
但是如果以补码方式存储，补码统一了零的编码：

|十进制数|补码|
|---|---|
|+0|0000 0000|
|-0|10000 0000由于只用8位描述，最高位1丢弃，变为0000 0000|
示例2：计算9-6的结果
以原码方式相加：

|十进制数|原码|
|---|---|
|9|0000 1001|
|-6|1000 0110|
<img src="https://article.biliimg.com/bfs/article/09e82684b6c9921176c0450b0a844c8838716159.png" alt="image.png" style="zoom:50%;" />


结果为-15，不正确。
以补码方式相加：

|十进制数|补码|
|---|---|
|9|0000 1001|
|-6|1111 1010|
![image.png](https://article.biliimg.com/bfs/article/9804670585dcfc59302a15dbf00a25c538716159.png)


最高位的1溢出,剩余8位二进制表示的是3，正确。

<font color=#ff0000>在计算机系统中，数值一律用补码来存储</font>，主要原因是：
- 统一了零的编码
- 将符号位和其它位统一处理
- 将减法运算转变为加法运算
- 两个用补码表示的数相加时，如果最高位(符号位)有进位，则进位被舍弃

### 数值溢出

当超过一个数据类型能够存放最大的范围时，数值会溢出。
有符号位最高位溢出的区别：符号位溢出会导致数的正负发生改变，但最高位的溢出会导致最高位丢失。

| 数据类型          | 占用空间 | 取值范围                        |
| ------------- | ---- | --------------------------- |
| char          | 1字节  | -128到 127(-$2^7$ ~ $2^7$-1) |
| unsigned char | 1字节  | 0 到 255(0 ~ 28-1)           |

```c
#include <stdio.h>

int main()
{
	char ch;

	//符号位溢出会导致数的正负发生改变
	ch = 0x7f + 2; //127+2
	printf("%d\n", ch);
	//	0111 1111
	//+2后 1000 0001，这是负数补码，其原码为 1111 1111，结果为-127

	//最高位的溢出会导致最高位丢失
	unsigned char ch2;
	ch2 = 0xff+1; //255+1
	printf("%u\n", ch2);
	//	  1111 1111
	//+1后 10000 0000， char只有8位最高位的溢出，结果为0000 0000，十进制为0

	ch2 = 0xff + 2; //255+1
	printf("%u\n", ch2);
	//	  1111 1111
	//+1后 10000 0001， char只有8位最高位的溢出，结果为0000 0001，十进制为1

	return 0;
}
```

## 类型限定符

| 限定符  | 含义                                                                                                                      |
|:--------:|:---------------------------------------------------------------------------------------------------------------------------:|
| extern   | 声明一个变量，extern声明的变量没有建立存储空间。<br><br>extern int a;//变量在定义的时候创建存储空间                                                            |
| const    | 定义一个常量，常量的值不能修改。<br><br>const int a = 10;                                                                                   |
|Volatile|防止编译器优化代码<br><br>volatile int flg = 0;<br><br>flg=1;flg=2;flg=21;flg=12;flg=0;(最后还是0编译器会将中间的部分省略掉，加了volatile就不会优化了)|
|register |定义寄存器变量，提高效率。register是建议型的指令，而不是命令型的指令，如果CPU有空闲寄存器，那么register就生效，如果没有空闲寄存器，那么register无效。此外register定义的变量在寄存器不在内存所以没有内存地址，速度快|

## 字符串格式化输出和输入

### 字符串常量

字符串是内存中一段连续的char空间，以`'\0'`(数字0)结尾。
字符串常量是由双引号括起来的字符序列，如“china”、“C program”，“$12.5”等都是合法的字符串常量。

字符串常量与字符常量的不同：
<img src="https://article.biliimg.com/bfs/article/2049f99f4f130a7aab52fcd1e97f859b38716159.png" alt="image.png" style="zoom:50%;" />

每个字符串的结尾，编译器会自动的添加一个结束标志位`'\0'`，即 "a" 包含两个字符`'a'`和`’\0’`。

### printf函数和putchar函数
printf是输出一个字符串，putchar输出一个char。
printf格式字符：

| 打印格式 | 对应数据类型     | 含义                                             |
|:--------:|:--------------:|:--------------------------------------------------:|
| %d       | int            | 接受整数值并将它表示为有符号的十进制整数                               |
| %hd      | short int      | 短整数                                                |
| %hu      | unsigned short | 无符号短整数                                             |
| %o       | unsigned int   | 无符号8进制整数                                           |
| %u       | unsigned int   | 无符号10进制整数                                          |
| %x,%X    | unsigned int   | 无符号16进制整数，x对应的是abcdef，X对应的是ABCDEF                  |
| %f       | float          | 单精度浮点数                                             |
| %lf      | double         | 双精度浮点数                                             |
| %e,%E    | double         | 科学计数法表示的数，此处"e"的大小写代表在输出时用的"e"的大小写                 |
| %c       | char           | 字符型。可以把输入的数字按照ASCII码相应转换为对应的字符                     |
| %s       | char          | 字符串。输出字符串中的字符直至字符串中的空字符(字符串以`'\0‘`结尾，这个`'\0'`即空字符) |
| %p       | void          | 以16进制形式输出指针                                        |
|`%%`|`%`|输出一个百分号|

printf附加格式

| 字符      | 含义                                                                  |
|:-----------:|:-----------------------------------------------------------------------:|
| l(字母l)      | 附加在d,u,x,o前面，表示长整数                                                      |
| -           | 左对齐                                                                     |
| m(代表一个整数)   | 数据最小宽度                                                                  |
|      0(数字0) | 将输出的前面补上0直到占满指定列宽为止不可以搭配使用-                                             |
| m.n(代表一个整数) | m指域宽，即对应的输出项在输出设备上所占的字符数。n指精度，用于说明输出的实型数的小数位数。对数值型的来说，未指定n时，隐含的精度为n=6位。 |  

```c
#include <stdio.h>
int main()
{
	int a = 100;
	printf("a = %d\n", a);//格式化输出一个字符串
	printf("%p\n", &a);//输出变量a在内存中的地址编号
	printf("%%d\n");

	char c = 'a';
	putchar(c);//putchar只有一个参数，就是要输出的char
	long a2 = 100;
	printf("%ld, %lx, %lo\n", a2, a2, a2);

	long long a3 = 1000;
	printf("%lld, %llx, %llo\n", a3, a3, a3);

	int abc = 10;
	printf("abc = '%6d'\n", abc);
	printf("abc = '%-6d'\n", abc);
	printf("abc = '%06d'\n", abc);
	printf("abc = '%-06d'\n", abc);

	double d = 12.3;
	printf("d = \' %-10.3lf \'\n", d);

	return 0;
}
```

### scanf函数与getchar函数
- getchar是从标准输入设备读取一个char。
- scanf通过%转义的方式可以得到用户通过标准输入设备输入的数据。

```c
#include <stdio.h>

int main()
{
	char ch1;
	char ch2;
	char ch3;
	int a;
	int b;

	printf("请输入ch1的字符：");
	ch1 = getchar();
	printf("ch1 = %c\n", ch1);

	getchar(); //测试此处getchar()的作用

	printf("请输入ch2的字符：");
	ch2 = getchar();
	printf("\'ch2 = %ctest\'\n", ch2);

	getchar(); //测试此处getchar()的作用
	printf("请输入ch3的字符：");
	scanf("%c", &ch3);//这里第二个参数一定是变量的地址，而不是变量名
	printf("ch3 = %c\n", ch3);

	printf("请输入a的值：");
	scanf("%d", &a);
	printf("a = %d\n", a);

	printf("请输入b的值：");
	scanf("%d", &b);
	printf("b = %d\n", b);

	return 0;
}
```

#c语言疑问问题
接收字符串：
scanf 具有安全隐患。如果存储空间不足，数据能存储到内存中，但不被保护。【空间不足不要使用】
推荐使用scanf_s这样的函数
```c
int main () 
{
	char a[5]; 
	scanf("%s",a);//用了多余的空间不是自己的，存在安全隐患 
	printf("a=%s\n",a);//因为printf是第一个字符直到取到以‘\0}为止
	scanf_s("%s", a,5);
	printf("a=%s\n",a);
}
```

# 运算符与表达式

## 常用运算符分类

|   运算符类型   |         作用          |
| :-------: | :-----------------: |
|   算术运算符   |      用于处理四则运算       |
|   赋值运算符   |    用于将表达式的值赋给变量     |
|   比较运算符   | 用于表达式的比较，并返回一个真值或假值 |
|   逻辑运算符   |  用于根据表达式的值返回真值或假值   |
|   位运算符    |     用于处理数据的位运算      |
| sizeof运算符 |      用于求字节数长度       |

## 算术运算符

| 运算符 |   术语   |     示例      |    结果     |
| :-: | :----: | :---------: | :-------: |
|  +  |   正号   |     +3      |     3     |
|  -  |   负号   |     -3      |    -3     |
|  +  |   加    |   10 + 5    |    15     |
|  -  |   减    |   10 - 5    |     5     |
| ``  |   乘    |   10 `` 5   |    50     |
| `/` |   除    |  10 `/` 5   |     2     |
|  %  | 取模(取余) |   10 % 3    |     1     |
| ++  |  前自增   | a=2; b=++a; | a=3; b=3; |
| ++  |  后自增   | a=2; b=a++; | a=3; b=2; |
| --  |  前自减   | a=2; b=--a; | a=1; b=1; |
| --  |  后自减   | a=2; b=a--; | a=1; b=2; |

#c语言面试点 
前置自增加++与后置自增加++区别:
- 计算优先级是后置++的优先级高于前置++
- 前置++的效率会高于后置++的效率
	- 因为对于后置++来说，它会创建一个临时的变量

## 赋值运算符

| 运算符 | 术语  |     示例     |    结果     |
| :-: | :-: | :--------: | :-------: |
|  =  | 赋值  | a=2; b=3;  | a=2; b=3; |
| +=  | 加等于 | a=0; a+=2; |   a=2;    |
| -=  | 减等于 | a=5; a-=3; |   a=2;    |
|  =  | 乘等于 | a=2; a=2;  |   a=4;    |
| /=  | 除等于 | a=4; a/=2; |   a=2;    |
| %=  | 模等于 | a=3; a%2;  |   a=1;    |

## 比较运算符

C 语言的比较运算中， “真”用数字“1”来表示， “假”用数字“0”来表示。

| 运算符 |  术语  |   示例   | 结果  |
| :-: | :--: | :----: | :-: |
| ==  | 相等于  | 4 == 3 |  0  |
| !=  | 不等于  | 4 != 3 |  1  |
|  <  |  小于  | 4 < 3  |  0  |
|  >  |  大于  | 4 > 3  |  1  |
| <=  | 小于等于 | 4 <= 3 |  0  |
| >=  | 大于等于 | 4 >= 1 |  1  |


<img src="https://article.biliimg.com/bfs/article/e17b0abb2295f9b2de0985a37033bbe638716159.png" alt="image.png" style="zoom:50%;" />

## 运算符优先级

<img src="https://article.biliimg.com/bfs/article/b8db35805c530479340b972d8c5dd27c38716159.png" alt="image.png" style="zoom:70%;" />


<img src="https://article.biliimg.com/bfs/article/d0eba59a26955330d1b029bcd22a565338716159.png" alt="image.png" style="zoom:70%;" />

<img src="https://article.biliimg.com/bfs/article/a79584ee2f804db1dcc93b57465d8f9a38716159.png" alt="image.png" style="zoom:70%;" />

## 类型转换

数据有不同的类型，不同类型数据之间进行混合运算时必然涉及到类型的转换问题。

转换的方法有两种：
- 自动转换(隐式转换)：遵循一定的规则,由编译系统自动完成。
- 强制类型转换：把表达式的运算结果强制转换成所需的数据类型。

类型转换的原则：占用内存字节数少(值域小)的类型，向占用内存字节数多(值域大)的类型转换，以保证精度不降低。

<img src="https://article.biliimg.com/bfs/article/3e142ce19422f624325b3c274a63259638716159.png" alt="image.png" style="zoom:50%;" />



### 隐式转换

```c
#include <stdio.h>

int main()
{
	int num = 5;
	printf("s1=%d\n", num / 2);
	printf("s2=%lf\n", num / 2.0);
	return 0;
}
```

### 强制转换

强制类型转换指的是使用强制类型转换运算符，将一个变量或表达式转化成所需的类型，其基本语法格式如下所示：

`(类型说明符) (表达式)`
强制类型转换：
语法：(目标类型)带转换变量
(目标类型)带转换表达式
大多数用于函数调用期间，实参给形参传值。

```c
#include <stdio.h>

int main()
{
	float x = 0;
	int i = 0;
	x = 3.6f;
    int* p=(int*)malloc(100);//将void* 强制转换成int 
	i = x;			//x为实型, i为整型，直接赋值会有警告
	i = (int)x;		//使用强制类型转换

	printf("x=%f, i=%d\n", x, i);
	return 0;
}
```


# 程序流程结构

## 概述
C语言支持最基本的三种程序运行结构：顺序结构、选择结构、循环结构。
- 顺序结构：程序按顺序执行，不发生跳转。
- 选择结构：依据是否满足条件，有选择的执行相应功能。
- 循环结构：依据条件是否满足，循环多次执行某段代码。


## 选择结构
### if语句

#### 三目运算符

`布尔表达式?表达式1:表达式2`
运算过程：如果布尔表达式的值为 true ，则返回 表达式1 的值，否则返回 表达式2 的值

```c
#include <stdio.h>

int main()
{
	int a = 10;
	int b = 20;
	int c;

	if (a > b)
	{
		c = a;
	}
	else
	{
		c = b;
	}
	printf("c1 = %d\n", c);

	a = 1;
	b = 2;
	c = ( a > b ? a : b );
	printf("c2 = %d\n", c);

	return 0;
}
```

### switch语句

```c
#include <stdio.h>

int main()
{
	char c;
	c = getchar();

	switch (c) //参数只能是整型变量
	{
	case '1':
		printf("OK\n");
		break;//switch遇到break就中断了
	case '2':
		printf("not OK\n");
		break;
	default://如果上面的条件都不满足，那么执行default
		printf("are u ok?\n");
	}
	return 0;
}
```

> [!warning]
> 
如果没有`break`,后面几个case会不进行判断直接执行了


# 数组和字符串

## 概述
在程序设计中，为了方便处理数据把具有相同类型的若干变量按有序形式组织起来——称为数组。
数组就是在<font color=#ff0000>内存中连续的相同类型的变量空间</font>。同一个数组所有的成员都是相同的数据类型，同时所有的成员在内存中的地址是连续的。

<img src="https://article.biliimg.com/bfs/article/fb1456b983d8bf0472cb080a1dd2bd9838716159.png" alt="image.png" style="zoom:70%;" />


数组属于构造数据类型：
- 一个数组可以分解为多个数组元素：这些数组元素可以是基本数据类型或构造类型。

```c
	int a[10];  
	struct Stu boy[10];
```

- 按数组元素类型的不同，数组可分为：数值数组、字符数组、指针数组、结构数组等类别。
```c
	int a[10];
	char s[10];
    char p[10];
	struct Student { string name; int age; }s[3];
```

通常情况下，数组元素下标的个数也称为维数，根据维数的不同，可将数组分为一维数组、二维数组、三维数组、四维数组等。通常情况下，我们将<font color=#ff0000>二维及以上的数组称为多维数组</font>。

## 一维数组

### 一维数组的定义和使用
- 数组名字符合标识符的书写规定(数字、英文字母、下划线)
- 数组名不能与其它变量名相同，同一作用域内是唯一的
- 方括号`[]`中常量表达式表示数组元素的个数

```shell
	int a[3]表示数组a有3个元素
	其下标从0开始计算，因此3个元素分别为a[0],a[1],a[2]
```

- 定义数组时`[]`内最好是常量，使用数组时`[]`内即可是常量，也可以是变量


```c
#include <stdio.h>

int main()
{
	int a[10];//定义了一个数组，名字叫a，有10个成员，每个成员都是int类型
	//a[0]…… a[9]，没有a[10]
	//没有a这个变量，a是数组的名字，但不是变量名，它是常量
	a[0] = 0;
	//……
	a[9] = 9;

	int i = 0;
	for (i = 0; i < 10; i++)
	{
		a[i] = i; //给数组赋值
	}

	//遍历数组，并输出每个成员的值
	for (i = 0; i < 10; i++)
	{
		printf("%d ", a[i]);
	}
	printf("\n");
	return 0;
}
```

### 一维数组的初始化

在定义数组的同时进行赋值，称为初始化。全局数组若不初始化，编译器将其初始化为零。<font color=#ff0000>局部数组若不初始化，内容为随机值</font>。
```c
	int a[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };//定义一个数组，同时初始化所有成员变量
	int a[10] = { 1, 2, 3 };//初始化前三个成员，后面所有元素都设置为0
	int a[10] = { 0 };//所有的成员都设置为0
	 //[]中不定义元素个数，定义时必须初始化
	int a[] = { 1, 2, 3, 4, 5 };//定义了一个数组，有5个成员
```

### 数组名

<font color=#ff0000>数组名是一个地址的常量，代表数组中首元素的地址。</font>

```c
#include <stdio.h>
int main()
{
	int a[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };//定义一个数组，同时初始化所有成员变量
	printf("a = %p\n", a);
	printf("&a[0] = %p\n", &a[0]);
	int n = sizeof(a); //数组占用内存的大小，10个int类型，10  4  = 40
	int n0 = sizeof(a[0]);//数组第0个元素占用内存大小，第0个元素为int，4
	int i = 0;
	for (i = 0; i < sizeof(a) / sizeof(a[0]); i++)
	{
		printf("%d ", a[i]);
	}
	printf("\n");
	return 0;
}
```

#c语言疑问问题 

数组输出的值
```c
int main()
{
  int a[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
  printf("a=%p\n",a); // a=0x7ffd9610d440
  printf("&a=%p\n",&a); // &a=0x7ffd9610d440  
  printf("a=%d\n",*a); // a=1
  printf("(a+1)=%d\n",*(a+1)); //(a+1)=2=a[1]  
  printf("a+1=%p\n",(a+1)); // a+1=0x7ffd9610d444 
  printf("&a+1=%p\n",&a+1); // &a+1=0x7ffd9610d468 &a+1 并不等同于 &a[1]，而是偏移了整个数组的大小。
  printf("&(a+1)=%p\n",&(a+1)); // 报错不允许这样,a+1已经是指针了,不能再&了
}
```

> [!note]
> 1. 当单独使用时，数组名 `a` 会退化（decay）为指向数组第一个元素的指针。它的值等同于 `&a[0]`。
> 2. `&a`：这是对整个数组 `a` 取地址。它的类型是指向包含5个整数的数组的指针，即 `int (*)[5]`。
> 3. 虽然 `a`、`&a[0]` 和 `&a` 在数值上是相同的（它们都指向数组的开始位置），但它们的**类型**是不同的：
> 4. `a + 1` 会将指针移动一个 `int` 的大小（通常是4字节）
> 5. `&a + 1` 会将指针移动整个数组的大小（在这个例子中是40字节，10个int）



## 二维数组

### 二维数组的定义和使用

二维数组定义的一般形式是：
`类型说明符 数组名[常量表达式1][常量表达式2]`
其中常量表达式1表示第一维下标的长度，常量表达式2 表示第二维下标的长度。

`int a[3][4]`;
- 命名规则同一维数组
- 定义了一个三行四列的数组，数组名为a其元素类型为整型，该数组的元素个数为3×4个，即：

![image.png](https://article.biliimg.com/bfs/article/d985560f275907bc7a4bd9846306de4838716159.png)


维数组a是按行进行存放的，先存放`a[0]`行，再存放`a[1]`行、`a[2]`行，并且每行有四个元素，也是依次存放的。
- 二维数组在概念上是二维的：其下标在两个方向上变化，对其访问一般需要两个下标。
- 在内存中并不存在二维数组，二维数组实际的硬件存储器是连续编址的，<font color=#ff0000>也就是说内存中只有一维数组</font>，即放完一行之后顺次放入第二行，和一维数组存放方式是一样的。

### 二维数组的初始化

```c
//分段赋值 	int a[3][4] = {{ 1, 2, 3, 4 },{ 5, 6, 7, 8, },{ 9, 10, 11, 12 }};
	int a[3][4] = 
	{ 
		{ 1, 2, 3, 4 },
		{ 5, 6, 7, 8, },
		{ 9, 10, 11, 12 }
	};
	//连续赋值
	int a[3][4] = { 1, 2, 3, 4 , 5, 6, 7, 8, 9, 10, 11, 12  };
	//可以只给部分元素赋初值，未初始化则为0
	int a[3][4] = { 1, 2, 3, 4  };
	//所有的成员都设置为0
	int a[3][4] = {0};
	//[]中不定义元素个数，定义时必须初始化
	int a[][4] = { 1, 2, 3, 4, 5, 6, 7, 8};
```


### 数组名
<font color=#ff0000>数组名是一个地址的常量，代表数组中首元素的地址</font>。
```c
#include <stdio.h>

int main()
{
	//定义了一个二维数组，名字叫a
	//二维数组是本质上还是一维数组，此一维数组有3个元素
    //每个元素又是一个一维数组int[4]
	int a[3][4] = { 1, 2, 3, 4 , 5, 6, 7, 8, 9, 10, 11, 12  };

	//数组名为数组首元素地址，二维数组的第0个元素为一维数组
	//第0个一维数组的数组名为a[0]
	printf("a = %p\n", a);
	printf("a[0] = %p\n", a[0]);
	//测二维数组所占内存空间，有3个一维数组，每个一维数组的空间为44
	//sizeof(a) = 3  4  4 = 48
	printf("sizeof(a) = %d\n", sizeof(a));

	//测第0个元素所占内存空间，a[0]为第0个一维数组int[4]的数组名，44=16
	printf("sizeof(a[0]) = %d\n", sizeof(a[0]) );

	//测第0行0列元素所占内存空间，第0行0列元素为一个int类型，4字节
	printf("sizeof(a[0][0]) = %d\n", sizeof(a[0][0]));

	//求二维数组行数
	printf("i = %d\n", sizeof(a) / sizeof(a[0]));

	// 求二维数组列数
	printf("j = %d\n", sizeof(a[0]) / sizeof(a[0][0]));

	//求二维数组行列总数
	printf("n = %d\n", sizeof(a) / sizeof(a[0][0]));

	return 0;
}
```

## 字符数组与字符串

字符数组与字符串区别
- 字符串：`"xx"` 隐藏了后面的`'\0'`
- <font color=#ff0000>C语言中没有字符串这种数据类型</font>，可以通过char的数组来替代；
- 字符串一定是一个char的数组，但char的数组未必是字符串；
- <font color=#ff0000>数字0(和字符`‘\0’`等价)结尾的char数组就是一个字符串</font>，但如果char数组没有以数字0结尾，那么就不是一个字符串，只是普通字符数组，所以字符串是一种特殊的char的数组。

```c
#include <stdio.h>

int main()
{
	char c1[] = { 'c', ' ', 'p', 'r', 'o', 'g' }; //普通字符数组
	printf("c1 = %s\n", c1); //乱码，因为没有’\0’结束符
	//以‘\0’(‘\0’就是数字0)结尾的字符数组是字符串
	char c2[] = { 'c', ' ', 'p', 'r', 'o', 'g', '\0'}; 
	printf("c2 = %s\n", c2);
	//字符串处理以‘\0’(数字0)作为结束符，后面的'h', 'l', 'l', 'e', 'o'不会输出
	char c3[] = { 'c', ' ', 'p', 'r', 'o', 'g', '\0', 'h', 'l', 'l', 'e', 'o', '\0'};
	printf("c3 = %s\n", c3);
	return 0;
}
```


### 字符串的初始化


```c
#include <stdio.h>

// C语言没有字符串类型，通过字符数组模拟
// C语言字符串，以字符‘\0’, 数字0
int main()
{
	//不指定长度, 没有0结束符，有多少个元素就有多长
	char buf[] = { 'a', 'b', 'c' };
	printf("buf = %s\n", buf);	//乱码

	//指定长度，后面没有赋值的元素，自动补0
	char buf2[100] = { 'a', 'b', 'c' };
	char buf[1000]={“hello”};
	printf("buf2 = %s\n", buf2);

	//所有元素赋值为0
	char buf3[100] = { 0 };

	//char buf4[2] = { '1', '2', '3' };//数组越界

	char buf5[50] = { '1', 'a', 'b', '0', '7' };
	printf("buf5 = %s\n", buf5);

	char buf6[50] = { '1', 'a', 'b', 0, '7' };
	printf("buf6 = %s\n", buf6);

	char buf7[50] = { '1', 'a', 'b', '\0', '7' };
	printf("buf7 = %s\n", buf7);

	//使用字符串初始化，编译器自动在后面补0，常用
	char buf8[] = "agjdslgjlsdjg";

	//'\0'后面最好不要连着数字，有可能几个数字连起来刚好是一个转义字符
	//'\ddd'八进制字义字符，'\xdd'十六进制转移字符
	// \012相当于\n
	char str[] = "\012abc";
	printf("str == %s\n", str);

	return 0;
}
```

### 字符串的输入输出

由于字符串采用了`'\0'`标志，字符串的输入输出将变得简单方便。

```c
#include <stdio.h>
int main()
{
	char str[100];
	printf("input string1 : \n");
	scanf("%s", str);//scanf(“%s”,str)默认以空格分隔
	printf("output:%s\n", str);
	return 0;
}
```

字符串获取 scanf：
注意：
- 1)用于存储字符串的空间必须足够大，防止溢出。 `char str[5]`;
- 2) 获取字符串，%s， 遇到空格 和 \n 终止。
借助“正则表达式”, 获取带有空格的字符串：`scanf("%[^\n]", str)`;

`gets()`

```c
#include <stdio.h>
char gets(char s);
功能：从标准输入读入字符，并保存到s指定的内存空间，直到出现换行符或读到文件结尾为止。
参数：
	s：字符串首地址
返回值：
	成功：读入的字符串
	失败：NULL
```

gets(str)与scanf(“%s”,str)的区别：
- gets(str)允许输入的字符串含有空格
- scanf(“%s”,str)不允许含有空格

<font color=#ff0000>注意：由于scanf()和gets()无法知道字符串s大小，必须遇到换行符或读到文件结尾为止才接收输入，因此容易导致字符数组越界(缓冲区溢出)的情况。</font>

`fgets()`

```c
#include <stdio.h>
char fgets(char s, int size, FILE stream);
功能：从stream指定的文件内读入字符，保存到s所指定的内存空间，直到出现换行字符、读到文件结尾或是已读了size - 1个字符为止，最后会自动加上字符 '\0' 作为字符串结束。
参数：
	s：字符串
	size：指定最大读取字符串的长度(size - 1)
	stream：文件指针，如果读键盘输入的字符串，固定写为stdin
返回值：
	成功：成功读取的字符串
	读到文件尾或出错： NULL
```

```c
int main()
{
  char str[100];
  FILE file=fopen("in.txt", "r");
  printf("请输入一个字符串：");
  fgets(str, sizeof(str), file);
  fgets(str, sizeof(str), stdin);
  printf("您输入的字符串是：%s",str);
}
```

fgets()在读取一个用户通过键盘输入的字符串的时候，同时把用户输入的回车也做为字符串的一部分。通过scanf和gets输入一个字符串的时候，不包含结尾的“\n”，<font color=#ff0000>但通过fgets结尾多了“\n”</font>。fgets()函数是安全的，不存在缓冲区溢出的问题。

`puts()`
```c
#include <stdio.h>
int puts(const char s);
功能：标准设备输出s字符串，在输出完成后自动输出一个'\n'。
参数：
	s：字符串首地址
返回值：
	成功：非负数
	失败：-1
```

```c
#include <stdio.h>

int main()
{
	printf("hello world");
	puts("hello world");
	return 0;
}
```

`fputs()`

```c
#include <stdio.h>
int fputs(const char  str, FILE  stream);
功能：将str所指定的字符串写入到stream指定的文件中， 字符串结束符 '\0'  不写入文件。 
参数：
	str：字符串
	stream：文件指针，如果把字符串输出到屏幕，固定写为stdout
返回值：
	成功：0
	失败：-1
```

fputs()是puts()的文件操作版本，但fputs()不会自动输出一个'\n'。

```c
	printf("hello world");
	puts("hello world");
	fputs("hello world", stdout);
```

`strlen()`
```c
#include <string.h>
size_t strlen(const char s);
功能：计算指定指定字符串s的长度，不包含字符串结束符‘\0’
参数：
s：字符串首地址
返回值：字符串s的长度，size_t为unsigned int类型
```


```c
	char str[] = "abc\0defg";
	int n = strlen(str);
	printf("n = %d\n", n);

```

# 函数

## 随机数函数

```c
功能：获取当前系统时间
参数：常设置为NULL
返回值：当前系统时间, time_t 相当于long类型，单位为毫秒

#include <stdlib.h>
void srand(unsigned int seed);
功能：用来设置rand()产生随机数时的随机种子
参数：如果每次seed相等，rand()产生随机数相等
返回值：无

#include <stdlib.h>
int rand(void);
功能：返回一个随机数值
参数：无
返回值：随机数

```

```c
#include <stdio.h>
#include <time.h>
#include <stdlib.h>

int main()
{
	time_t tm = time(NULL);//得到系统时间
	srand((unsigned int)tm);//随机种子只需要设置一次即可

	int r = rand();
	printf("r = %d\n", r);

	return 0;
}
```


- 在rand函数的内部，是通过一个公式计算出一个值作为随机值，下次再调用rand的时候，再把这个随机值作为参数传给这个公式计算出一个新的随机值，周而复始。
- 在C库中，是通过一个静态全局变量来作为“种子”，而这个“种子”的值是通过`srand`函数改变的，如果不写srand函数，这个“种子”值默认赋值为1。这就解释了“为何不写`srand`函数，rand函数就会生成伪随机数”，因为程序只要重新开始运行，“种子”值就会被默认赋值为1，那么通过公式算出来的序列肯定就一直相同了。
## 函数的定义

### 函数定义格式

函数定义的一般形式：
```c
	返回类型 函数名(形式参数列表)
	{
		数据定义部分;
		执行语句部分;
	}
```

### 函数名字、形参、函数体、返回值


<img src="https://article.biliimg.com/bfs/article/6f53773245f7bd476c03add95df5ce3938716159.png" alt="image.png" style="zoom:50%;" />


1. 函数名
	理论上是可以随意起名字，最好起的名字见名知意，应该让用户看到这个函数名字就知道这个函数的功能。注意，函数名的后面有个圆换号()，代表这个为函数，不是普通的变量名。
2. 形参列表
	在定义函数时指定的形参，在<font color=#ff0000>未出现函数调用时，它们并不占内存中的存储单元</font>，因此称它们是形式参数或虚拟参数，简称形参，表示它们并不是实际存在的数据，所以，形参里的变量不能赋值。

### 函数的形参和实参

- 形参出现在函数定义中，在整个函数体内都可以使用，离开该函数则不能使用。
- 实参出现在主调函数中，进入被调函数后，实参也不能使用。
- 实参变量对形参变量的数据传递是“值传递”，即单向传递，<font color=#ff0000>只由实参传给形参，而不能由形参传回来给实参。</font>
- 在调用函数时，编译系统临时给形参分配存储单元。调用结束后，形参单元被释放。
- 实参单元与形参单元是不同的单元。调用结束后，形参单元被释放，函数调用结束返回主调函数后则不能再使用该形参变量。实参单元仍保留并维持原值。<font color=#ff0000>因此，在执行一个被调用函数时，形参的值如果发生改变，并不会改变主调函数中实参的值。</font>

### 无参函数调用
如果是调用无参函数，则不能加上“实参”，但括号不能省略。

```c
// 函数的定义
void test()
{
}
int main()
{
	// 函数的调用
	test();	// right, 圆括号()不能省略
	test(250); // error, 函数定义时没有参数
	return 0;
}
```

### 有参函数调用
a)如果实参表列包含多个实参，则各参数间用逗号隔开。

```c
// 函数的定义
void test(int a, int b)
{
}

int main()
{
	int p = 10, q = 20;
	test(p, q);	// 函数的调用

	return 0;
}
```

b)实参与形参的个数应相等，类型应匹配(相同或赋值兼容)。实参与形参按顺序对应，一对一地传递数据。
<font color=#ff0000>无论实参是何种类型的量，在进行函数调用时，它们都必须具有确定的值，以便把这些值传送给形参</font>
c)实参可以是常量、变量或表达式，。所以，这里的变量是在圆括号( )外面定义好、赋好值的变量。

```c
// 函数的定义
void test(int a, int b)
{
}

int main()
{
	// 函数的调用
	int p = 10, q = 20;
	test(p, q);	// right
	test(11, 30 - 10); // right

	test(int a, int b); // error, 不应该在圆括号里定义变量

	return 0;
}
```

### 函数返回值
a)如果函数定义没有返回值，函数调用时不能写void关键字，调用函数时也不能接收函数的返回值。
```c
// 函数的定义
void test()
{
}

int main()
{
	// 函数的调用
	test(); // right
	void test(); // error, void关键字只能出现在定义，不可能出现在调用的地方
	int a = test();	// error, 函数定义根本就没有返回值

	return 0;
}
```

b)如果函数定义有返回值，这个返回值我们根据用户需要可用可不用，但是，假如我们需要使用这个函数返回值，<font color=#ff0000>我们需要定义一个匹配类型的变量来接收。</font>
```c
// 函数的定义, 返回值为int类型
int test()
{
}

int main()
{
	// 函数的调用
	int a = test(); // right, a为int类型
	int b;
	b = test();	// right, 和上面等级

	char p = test(); // 虽然调用成功没有意义, p为char , 函数返回值为int, 类型不匹配

	// error, 必须定义一个匹配类型的变量来接收返回值
	// int只是类型，没有定义变量
	int = test();	
	
	// error, 必须定义一个匹配类型的变量来接收返回值
	// int只是类型，没有定义变量
	int test();
	
	return 0;
}
```


## 函数的声明

#语言之前的区别
如果使用用户自己定义的函数，而该函数与调用它的函数(即主调函数)不在同一文件中，或者<font color=#ff0000>函数定义的位置在主调函数之后</font>，则必须在调用此函数之前对被调用的函数作声明,<font color=#ff0000>其他语言没有这个要求</font>
隐式声明：【不要依赖】
> 默认编译器做隐式声明函数时，返回都为 int ，根据调用语句不全函数名和形参列表。

所谓函数声明，就是在函数尚在未定义的情况下，事先将<font color=#ff0000>该函数的有关信息通知编译系统</font>，相当于告诉编译器，函数在后面定义，以便使编译能正常进行。

<font color=#ff0000>注意</font>：一个函数只能被定义一次，但可以声明多次。

```c
#include <stdio.h>

int max(int x, int y); // 函数的声明，分号不能省略
// int max(int, int); // 另一种方式

int main()
{
	int a = 10, b = 25, num_max = 0;
	num_max = max(a, b); // 函数的调用

	printf("num_max = %d\n", num_max);

	return 0;
}
// 函数的定义
int max(int x, int y)
{
	return x > y ? x : y;
}
```

函数定义和声明的区别：
1)定义是指对函数功能的确立，包括指定函数名、函数类型、形参及其类型、函数体等，它是一个完整的、独立的函数单位。
2)声明的作用则是把函数的名字、函数类型以及形参的个数、类型和顺序(注意，不包括函数体)通知编译系统，以便在对包含函数调用的语句进行编译时，据此对其进行对照检查(例如函数名是否正确，实参与形参的类型和个数是否一致)。

## main函数与exit函数
在main函数中调用exit和return结果是一样的，但在子函数中调用return只是代表子函数终止了，在子函数中调用`exit`，那么程序终止。
```c
#include <stdio.h>
#include <stdlib.h>
void fun()
{
	printf("fun\n");
	//return;
	exit(0);
}
int main()
{
	fun();
	while (1);

	return 0;
}
```

## 多文件(分文件)编程
### 分文件编程
- 把函数声明放在头文件`xxx.h`中，在主函数中包含相应头文件
- 在头文件对应的`xxx.c`中实现`xxx.h`声明的函数

<img src="https://article.biliimg.com/bfs/article/4c6f84f2d166939cd9c047e0a34517e638716159.png" alt="image.png" style="zoom:50%;" />
### 防止头文件重复包含
当一个项目比较大时，往往都是分文件，这时候有可能不小心把同一个头文件 include 多次，或者头文件嵌套包含。

a.h 中包含 b.h ：
`#include "b.h"`

b.h 中包含 a.h：
`#include "a.h"`

main.c 中使用其中头文件：
```c
#include "a.h"
int main()
{
	return 0;
}
```

编译上面的例子，会出现如下错误：

> In included file: main file cannot be included recursively when building a preamblec

为了避免同一个文件被include多次，C/C++中有两种方式，一种是`#ifndef` 方式，一种是` #pragma once` 方式。
方法一：
```c
#ifndef __SOMEFILE_H__
#define __SOMEFILE_H__

// 声明语句
//include 头文件
//函数声明
//类型定义
//宏定义
#endif
```

方法二：
```c
#pragma once

// 声明语句
```


## 可变参数

C语言中一般使用宏定义实现可变参数，先看一个示例：
```c++
#include <stdarg.h>
void func(const char *fmt, ...)
{
    va_list ap;
    va_start(ap, fmt);

    auto a = va_arg(ap, int);
    auto b = va_arg(ap, double);
    auto c = va_arg(ap, char);
    
    cout << a << ", " << b << ", " << c << endl;
    va_end(ap);
}
int main()
{
    func("%d %f %s\n", 1, 2.0f, "hello world");
    return 0;
}
```

这是一个很常见的C语言的可变参数的使用，`va_start` 用于初始化 `va_list` 变量，`va_arg` 用于提取可变参数，`va_end` 用于释放 `va_list`

这个示例可以用在一般函数上，无法使用在宏定义中，如果一定要在宏定义中使用，需要配合 `__VA_ARGS__`，示例如下：
```c++
//#define CALC(fmt, ...) func(fmt, ...) //错误的使用
#define CALC(fmt, ...) func(fmt, __VA_ARGS__)
int main()
{
    CALC("%d %f %s\n", 1, 2.0f, "hello world");
    return 0;
}
```

# 指针
## 概述
### 内存
内存含义：
- 存储器：计算机的组成中，用来存储程序和数据，辅助CPU进行运算处理的重要部分。
- 内存：内部存贮器，暂存程序/数据——掉电丢失 SRAM、DRAM、DDR、DDR2、DDR3。
- 外存：外部存储器，长时间保存程序/数据—掉电不丢ROM、ERRROM、FLASH(NAND、NOR)、硬盘、光盘。

内存是沟通CPU与硬盘的桥梁：
- 暂存放CPU中的运算数据
- 暂存与硬盘等外部存储器交换的数据

### 物理存储器和存储地址空间
有关内存的两个概念：物理存储器和存储地址空间。

物理存储器：实际存在的具体存储器芯片。
- 主板上装插的内存条
- 显示卡上的显示RAM芯片
- 各种适配卡上的RAM芯片和ROM芯片

存储地址空间：对存储器编码的范围。我们在软件上常说的内存是指这一层含义。
- 编码：对每个物理存储单元(一个字节)分配一个号码
- 寻址：可以根据分配的号码找到相应的存储单元，完成数据的读写

### 内存地址
- 将内存抽象成一个很大的一维字符数组。
- 编码就是对内存的每一个字节分配一个32位或64位的编号(与32位或者64位处理器相关)。
- 这个内存编号我们称之为内存地址。
	- 内存中的每一个数据都会分配相应的地址：
- char:占一个字节分配一个地址
- int: 占四个字节分配四个地址
- float、struct、函数、数组等

<img src="https://article.biliimg.com/bfs/article/54ed7e089a3688e51b07cd5b5e3f98f238716159.png" alt="image.png" style="zoom:80%;" />

### 指针和指针变量
- <font color=#ff0000>内存区的每一个字节都有一个编号，这就是“地址”</font>。
- 如果在程序中定义了一个变量，在对程序进行编译或运行时，系统就会给这个变量分配内存单元，并确定它的内存地址(编号)
- <font color=#ff0000>指针的实质就是内存“地址”。指针就是地址，地址就是指针。</font>
- <font color=#ff0000>指针是内存单元的编号，指针变量是存放地址的变量。</font>
- 通常我们叙述时会把指针变量简称为指针，实际他们含义并不一样。

<img src="https://article.biliimg.com/bfs/article/d88d4f97b1a5a3ad61ede7533a568a4438716159.png" alt="image.png" style="zoom:20%;" />


## 指针基础知识
### 指针变量的定义和使用
- 指针也是一种数据类型，指针变量也是一种变量
- 指针变量指向谁，就把谁的地址赋值给指针变量
- `“”`操作符操作的是指针变量指向的内存空间
```c
#include <stdio.h>

int main()
{
	int a = 0;
	char b = 100;
	printf("%p, %p\n", &a, &b); //打印a, b的地址

	//int 代表是一种数据类型，int指针类型，p才是变量名
	//定义了一个指针类型的变量，可以指向一个int类型变量的地址
	int p;
	p = &a;//将a的地址赋值给变量p，p也是一个变量，值是一个内存地址编号
	printf("%d\n", p);//p指向了a的地址，p就是a的值

	char p1 = &b;
	printf("%c\n", p1);//p1指向了b的地址，p1就是b的值

	return 0;
}
```

注意：&可以取得一个变量在内存中的地址。但是，<font color=#ff0000>不能取寄存器变量</font>，因为寄存器变量不在内存里，而在CPU里面，所以是没有地址的。

### 通过指针间接修改变量的值
```c
	int a = 0;
	int b = 11;
	int p = &a;

	p = 100;
	printf("a = %d, p = %d\n", a, p);

	p = &b;
	p = 22;
	printf("b = %d, p = %d\n", b, p);
```
`p = 250`;指针的解引用。 间接引用。
`p` ： <font color=#ff0000>将p变量的内容取出，当成地址看待，找到该地址对应的内存空间。</font>
如果做左值： 存数据到空间中。
如果做右值： 取出空间中的内容。

#c语言面试点 
什么是左值和右值
左值：是可以取地址的值，从本质上来说左值在内存空间中有存储，所以能取地址，能对他赋值,可以出现在赋值语句的左边或者右边，比如变量；
右值：是没有存放在实际的地址空间里面的，一般是运算过程的中间值，他可能是寄存器里面的立即数等,只能出现在赋值语句的右边，比如常量。

任意“指针”类型大小：
指针的大小与类型无关。 <font color=#ff0000>只与当前使用的平台架构有关。   32位：4字节。	 64位： 8字节</font>。

### 指针大小

- 使用sizeof()测量指针的大小，得到的总是：4或8
- sizeof()测的是指针变量指向存储地址的大小
- 在32位平台，所有的指针(地址)都是32位(4字节)
- 在64位平台，所有的指针(地址)都是64位(8字节)

```c
int p1;
	int* p2;
	char* p3;
	char* p4;
	printf("sizeof(p1) = %d\n", sizeof(p1));
	printf("sizeof(p2) = %d\n", sizeof(p2));
	printf("sizeof(p3) = %d\n", sizeof(p3));
	printf("sizeof(p4) = %d\n", sizeof(p4));
	printf("sizeof(double) = %d\n", sizeof(double));
```


### 野指针和空指针
指针变量也是变量，是变量就可以任意赋值，不要越界即可(32位为4字节，64位为8字节)，但是，<font color=#ff0000>任意数值赋值给指针变量没有意义，因为这样的指针就成了野指针</font>，此指针指向的区域是未知(操作系统不允许操作此指针指向的内存区域)。所以，<font color=#ff0000>野指针不会直接引发错误，操作野指针指向的内存区域才会出问题</font>。
1. 没有一个有效的地址空间的指针。
`Int *p; *p=1000;`
2. p变量有一个值，但该值不是可访问的内存区域。
```c
	Int *p=100; *p=1000;
	
	int a = 100;
	int *p;
	p = a; //把a的值赋值给指针变量p，p为野指针， ok，不会有问题，但没有意义
	p = 0x12345678; //给指针变量p赋值，p为野指针， ok，不会有问题，但没有意义
	p = 1000;  //操作野指针指向未知区域，内存出问题，err
```

但是，野指针和有效指针变量保存的都是数值，为了标志此指针变量没有指向任何变量(空闲可用)，C语言中，可以把NULL赋值给此指针，这样就标志此指针为空指针，没有任何指针。
`int p = NULL;`
NULL是一个值为0的宏常量：
`#define NULL    ((void )0)`

#c语言面试点 
什么情况下会导致野指针？

- 指针变量未初始化
	任何指针变量刚被创建时不会自动成为NULL指针，它的缺省值是随机的，它会乱指一气。所以，指针变量在创建的同时应当被初始化，要么将指针设置为NULL，要么让它指向合法的内存。
- 指针释放后未置空
	有时指针在free或delete后未赋值 NULL，便会使人以为是合法的。别看free和delete的名字(尤其是delete)，它们只是把指针所指的内存给释放掉，但并没有把指针本身干掉。此时指针指向的就是“垃圾”内存。释放后的指针应立即将指针置为NULL，防止产生“野指针”。
- 指针操作超越变量作用域
	不要返回指向栈内存的指针或引用，因为栈内存在函数结束时会被释放。


### 万能指针`void *`
`void*`指针可以指向任意变量的内存空间：
```c
	void *p = NULL;

	int a = 10;
	p = (void *)&a; //指向变量时，最好转换为void 

	//使用指针变量指向的内存时，转换为int 
	( (int *)p ) = 11;
	printf("a = %d\n", *a);
```

### const修饰的指针变量
#c语言面试点 
```c
	int a = 100;
	int b = 200;

	//常量指针
	//修饰，指针指向内存区域的值不能修改，指针指向可以变
	const int* p1 = &a; //等价于int const p1 = &a;
	//p1 = 111; //err
	p1 = &b; //ok

	//指针常量
	//修饰p2，指针指向不能变，指针指向的内存可以修改
	int*  const p2 = &a;
	//p2 = &b; //err
	p2 = 222; //ok
```
在编辑程序时，指针作为函数参数，如果不想修改指针对应内存空间的值，需要使用const修饰指针数据类型。
<font color=#ff0000>总结：const 向右修饰(忽略掉类型名)，被修饰的部分即为只读。</font>
<font color=#ff0000>常用：在函数形参内，用来限制指针所对应的内存空间为只读。</font>
## 指针和数组
### 数组名
数组名字是数组的首元素地址，但它是一个地址常量：
```c
	int a[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 }; 
	printf("a = %p\n", a);
	printf("&a[0] = %p\n", &a[0]);

	//a = 10; //err, 数组名只是常量，不能修改
```

### 指针操作数组元素
```c
#include <stdio.h>

int  main()
{
	int a[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
	int i = 0;
	int n = sizeof(a) / sizeof(a[0]);
	
	for (i = 0; i < n; i++)
	{
		//printf("%d, ", a[i]);
		printf("%d, ", *(a+i));
	}
	printf("\n");

	int *p = a; 	//定义一个指针变量保存a的地址
	for (i = 0; i < n; i++)
	{
		p[i] = 2*i;
	}

	for (i = 0; i < n; i++)
	{
		printf("%d, ", *(p + i));
	}
	printf("\n");
	return 0;
}
```


### 指针加减运算
1)加法运算
- 指针计算不是简单的整数相加
- 如果是一个`int `，+1的结果是增加一个int的大小
- 如果是一个`char `，+1的结果是增加一个char大小
- &数组名 + 1加过<font color=#ff0000>一个数组的大小</font>(数组元素个数 x sizeof(数组元素类型))
```c
#include <stdio.h>

int main()
{
	int a;
	int p = &a;
	printf("%d\n", p);
	p += 2;//移动了2个int
	printf("%d\n", p);

	char b = 0;
	char p1 = &b;
	printf("%d\n", p1);
	p1 += 2;//移动了2个char
	printf("%d\n", p1);

	return 0;
}
```

通过改变指针指向操作数组元素：
```c
#include <stdio.h>

int main()
{
	int a[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
	int i = 0;
	int n = sizeof(a) / sizeof(a[0]);

	int *p = a;
	for (i = 0; i < n; i++)
	{
		printf("%d, ", *p);
		p++;
	}
	printf("\n");
	return 0;
}
```

2) 减法运算
示例1：
```c
#include <stdio.h>

int main()
{
	int a[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
	int i = 0;
	int n = sizeof(a) / sizeof(a[0]);

	int *p = a+n-1;
	for (i = 0; i < n; i++)
	{
		printf("%d, ", *p);
		p--;
	}
	printf("\n");
	return 0;
}
```

示例2：
```c
#include <stdio.h>

int main()
{
	int a[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
	int p2 = &a[2]; //第2个元素地址
	int p1 = &a[1]; //第1个元素地址
	printf("p1 = %p, p2 = %p\n", p1, p2);

	int n1 = p2 - p1; //n1 = 1
	int n2 = (int)p2 - (int)p1; //n2 = 4
	printf("n1 = %d, n2 = %d\n", n1, n2);
	
	return 0;
}
```


### 指针数组 
指针数组，它是数组，数组的每个元素都是指针类型。
```c
#include <stdio.h>
int main()
{
	//指针数组
	int *p[3];
	int a = 1;
	int b = 2;
	int c = 3;
	int i = 0;

	p[0] = &a;
	p[1] = &b;
	p[2] = &c;

	for (i = 0; i < sizeof(p) / sizeof(p[0]); i++ )
	{
		printf("%d, ", (p[i]));
	}
	printf("\n");
	return 0;
}
```

### 数组指针

数组指针，它是指针，指向一个数组。

```c++
int arr[4] = {1, 2, 3, 4}; 
int(*b)[4]; // 声明了一个指针b，它指向一个包含4个int类型元素的数组
int i;
for(i = 0; i < 4; i++) {
    printf("%d ", (*b)[i]);
}

```

## 多级指针 
- C语言允许有多级指针存在，在实际的程序中一级指针最常用，其次是二级指针。
- 二级指针就是指向一个一级指针变量地址的指针。
- 三级指针基本用不着，但考试会考。
```c
	int a = 10;
	int *p = &a; //一级指针
	*p = 100; //p就是a

	int **q = &p;
	//q就是p
	//q就是a

	int ***t = &q;
	//t就是q
	//t就是p
	//t就是a
```

## 指针和函数
### 函数形参改变实参的值
```c
#include <stdio.h>

void swap1(int x, int y)
{
	int tmp;
	tmp = x;
	x = y;
	y = tmp;
	printf("x = %d, y = %d\n", x, y);
}

void swap2(int x, int y)
{
	int tmp;
	tmp = x;
	x = y;
	y = tmp;
}

int main()
{
	int a = 3;
	int b = 5;
	swap1(a, b); //值传递
	printf("a = %d, b = %d\n", a, b);

	a = 3;
	b = 5;
	swap2(&a, &b); //地址传递
	printf("a2 = %d, b2 = %d\n", a, b);

	return 0;
}
```

栈 帧：
当函数调用时，系统会在 stack 空间上申请一块内存区域，用来供函数调用，主要存放形参和局部变量(定义在函数内部)。
当函数调用结束，这块内存区域自动被释放(消失)。
传值和传址：
<font color=#ff0000>传值：函数调用期间，实参将自己的值，拷贝一份给形参。</font>
<font color=#ff0000>传址：函数调用期间，实参将地址值，拷贝一份给形参。 【重点】</font>
(地址值 --》 在swap函数栈帧内部，修改了main函数栈帧内部的局部变量值)
指针做函数参数：
```c
int swap2(int a, int b);
int swap2(char a, char b);
```
调用时，传有效的地址值。

<img src="https://article.biliimg.com/bfs/article/ea8cd5a2f8cd00086100ee8f411613ad38716159.png" alt="image.png" style="zoom:50%;" />

<img src="https://article.biliimg.com/bfs/article/f576244cdbb96a5f9505352ef60f3c0c38716159.png" alt="image.png" style="zoom:40%;" />

### 数组名做函数参数

数组名做函数参数，函数的形参会退化为指针：
```c
#include <stdio.h>

void printArrary(int a, int n)	
{
	int i = 0;
	for (i = 0; i < n; i++)
	{
		printf("%d, ", a[i]);
	}
	printf("\n");
}

int main()
{
	int a[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
	int n = sizeof(a) / sizeof(a[0]);

	//数组名做函数参数
	printArrary(a, n); 
	return 0;
}
```

数组做函数参数：
`void BubbleSort(int arr[10]) == void BubbleSort(int arr[])  == void BubbleSort(int arr)`
传递不再是整个数组，而是数组的首地址(一个指针)。
所以，当整型数组做函数参数时，我们通常在函数定义中，封装2个参数。一个表数组首地址，一个表元素个数。
指针做函数返回值：
`int test_func(int a, int b);`
指针做函数返回值，不能返回【局部变量的地址值】。
数组做函数返回值：C语言，不允许！！！！  只能写成指针形式。

### 指针做为函数的返回值
```c
#include <stdio.h>

int a = 10;

int* getA()
{
	return &a;
}


int main()
{
	( getA() ) = 111;
	printf("a = %d\n", a);

	return 0;
}
```

## 指针和字符串
### 字符指针
```c
#include <stdio.h>

int main()
{
	char str[] = "hello world";//变量可读可写
	char *p = str;
	p = 'm';
	p++;
	p = 'i';
	printf("%s\n", str);
	p = "mike jiang";
	printf("%s\n", p);
	char q = "test";//字符串常量，只读
	printf("%s\n", q);
    printf("%p\n", q);//这个就是“test”字符串的地址
	return 0;
}
```

### 字符指针做函数参数
```c
#include <stdio.h>
void mystrcat(char *dest, const char* src)
{
	int len1 = 0;
	int len2 = 0;
	while (dest[len1])
	{
		len1++;
	}
	while (src[len2])
	{
		len2++;
	}
	int i;
	for (i = 0; i < len2; i++)
	{
		dest[len1 + i] = src[i];
	}
}


int main()
{
	char dst[100] = "hello mike";
	char src[] = "123456";
	
	mystrcat(dst, src);
	printf("dst = %s\n", dst);

	return 0;
}```

### const修饰的指针变量
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(void)
{
	//const修饰一个变量为只读
	const int a = 10;
	//a = 100; //err
	//指针变量， 指针指向的内存， 2个不同概念
	char buf[] = "aklgjdlsgjlkds";
	//从左往右看，跳过类型，看修饰哪个字符
	//如果是， 说明指针指向的内存不能改变
	//如果是指针变量，说明指针的指向不能改变，指针的值不能修改
	const char* p = buf;
	// 等价于上面 char const p1 = buf;
	//p[1] = '2'; //err
	p = "agdlsjaglkdsajgl"; //ok
	char  const p2 = buf;
	p2[1] = '3';
	//p2 = "salkjgldsjaglk"; //err
	//p3为只读，指向不能变，指向的内存也不能变
	const char  const p3 = buf;
	return 0;
}
```

### 指针数组做为main函数的形参
`int main(int argc, char* argv[]);`
- main函数是操作系统调用的，第一个参数标明argc数组的成员数量，argv数组的每个成员都是`char `类型(字符串常量)
- argv是命令行参数(gcc hello.c -o hello.exe `xx xx` 中.exe文件后面的xxx)的字符串数组
- argc代表命令行参数的数量，程序名字本身算一个参数

```c
#include <stdio.h>

//argc: 传参数的个数(包含可执行程序)
//argv：指针数组，指向输入的参数
int main(int argc, char* argv[])
{

	//指针数组，它是数组，每个元素都是指针
	char a[] = { "aaaaaaa", "bbbbbbbbbb", "ccccccc" };
	int i = 0;

	printf("argc = %d\n", argc);
	for (i = 0; i < argc; i++)
	{
		printf("%s\n", argv[i]);
	}
	return 0;
}
```

### 字符串处理函数
1) strcpy()
```c
#include <string.h>
char strcpy(char* dest, const char* src);
功能：把src所指向的字符串复制到dest所指向的空间中，'\0'也会拷贝过去
参数：
	dest：目的字符串首地址
	src：源字符首地址
返回值：
	成功：返回dest字符串的首地址
	失败：NULL
```

<font color=#ff0000>注意</font>：如果参数dest所指的内存空间不够大，可能会造成缓冲溢出的错误情况。

```c
	char dest[20] = "123456789";
	char src[] = "hello world";
	strcpy(dest, src);
	printf("%s\n", dest);
```

2) strncpy()
```c
#include <string.h>
char strncpy(char* dest, const char* src, size_t n);
功能：把src指向字符串的前n个字符复制到dest所指向的空间中，是否拷贝结束符看指定的长度是否包含'\0'。
参数：
	dest：目的字符串首地址
	src：源字符首地址
	n：指定需要拷贝字符串个数
返回值：
	成功：返回dest字符串的首地址
	失败：NULL
```

```c
	char dest[20] ;
	char src[] = "hello world";

	strncpy(dest, src, 5);
	printf("%s\n", dest);

	dest[5] = '\0';
	printf("%s\n", dest);
```

3) strcat()
```c
#include <string.h>
char strcat(char dest, const char src);
功能：将src字符串连接到dest的尾部，‘\0’也会追加过去
参数：
	dest：目的字符串首地址
	src：源字符首地址
返回值：
	成功：返回dest字符串的首地址
	失败：NULL
```

```c
	char str[20] = "123";
	char src = "hello world";
	printf("%s\n", strcat(str, src));
```

4) strncat()
```c
#include <string.h>
char strncat(char dest, const char src, size_t n);
功能：将src字符串前n个字符连接到dest的尾部，‘\0’也会追加过去
参数：
	dest：目的字符串首地址
	src：源字符首地址
	n：指定需要追加字符串个数
返回值：
	成功：返回dest字符串的首地址
	失败：NULL
```

```c
	char str[20] = "123";
	char src = "hello world";
	printf("%s\n", strncat(str, src, 5));
```

5) strcmp()
```c
#include <string.h>
int strcmp(const char s1, const char s2);
功能：比较 s1 和 s2 的大小，比较的是字符ASCII码大小。
参数：
	s1：字符串1首地址
	s2：字符串2首地址
返回值：
	相等：0
	大于：>0 在不同操作系统strcmp结果会不同   返回ASCII差值
	小于：<0
```
	

```c
	char str1 = "hello world";
	char str2 = "hello mike";

	if (strcmp(str1, str2) == 0)
	{
		printf("str1==str2\n");
	}
	else if (strcmp(str1, str2) > 0)
	{
		printf("str1>str2\n");
	}	
	else
	{
		printf("str1<str2\n");
	}
```

6) strncmp()
```c
#include <string.h>
int strncmp(const char s1, const char s2, size_t n);
功能：比较 s1 和 s2 前n个字符的大小，比较的是字符ASCII码大小。
参数：
	s1：字符串1首地址
	s2：字符串2首地址
	n：指定比较字符串的数量
返回值：
	相等：0
	大于： > 0
	小于： < 0
```
	
```c
	char str1 = "hello world";
	char str2 = "hello mike";

	if (strncmp(str1, str2, 5) == 0)
	{
		printf("str1==str2\n");
	}
	else if (strcmp(str1, "hello world") > 0)
	{
		printf("str1>str2\n");
	}
	else
	{
		printf("str1<str2\n");
	}
```

7) sprintf()
```c
#include <stdio.h>
int sprintf(char str, const char format, ...);
功能：根据参数format字符串来转换并格式化数据，然后将结果输出到str指定的空间中，直到出现字符串结束符 '\0'  为止。
参数：
	str：字符串首地址
	format：字符串格式，用法和printf()一样
返回值：
	成功：实际格式化的字符个数
	失败： - 1
```


```c
	char dst[100] = { 0 };
	int a = 10;
	char src[] = "hello world";
	printf("a = %d, src = %s", a, src);
	printf("\n");

	int len = sprintf(dst, "a = %d, src = %s", a, src);
	printf("dst = \" %s\"\n", dst);
	printf("len = %d\n", len);
```

8) sscanf()
```c
#include <stdio.h>
int sscanf(const char str, const char format, ...);
功能：从str指定的字符串读取数据，并根据参数format字符串来转换并格式化数据。
参数：
	str：指定的字符串首地址
	format：字符串格式，用法和scanf()一样
返回值：
	成功：参数数目，成功转换的值的个数
	失败： - 1
```


```c
	char src[] = "a=10, b=20";
	int a;
	int b;
	sscanf(src, "a=%d,  b=%d", &a, &b);
	printf("a:%d, b:%d\n", a, b);
```

9) strchr()
```c
#include <string.h>
char* strchr(const char* s, int c);
功能：在字符串s中查找字母c出现的位置
参数：
	s：字符串首地址
	c：匹配字母(字符)
返回值：
	成功：返回第一次出现的c地址(将前面的不匹配的截断)
	失败：NULL
```

```c
	char src[] = "ddda123abcd";
	char *p = strchr(src, 'a');
	printf("p = %s\n", p);
```

10) strstr()
```c
#include <string.h>
char strstr(const char* haystack, const char* needle);
功能：在字符串haystack中查找字符串needle出现的位置
参数：
	haystack：源字符串首地址
	needle：匹配字符串首地址
返回值：
	成功：返回第一次出现的needle地址
	失败：NULL
```

```c
	char src[] = "ddddabcd123abcd333abcd";
	char *p = strstr(src, "abcd");
	printf("p = %s\n", p);
```

11) strtok()
```c
#include <string.h>
char strtok(char str, const char delim);
功能：来将字符串分割成一个个片段。当strtok()在参数s的字符串中发现参数delim中包含的分割字符时, 则会将该字符改为\0 字符，当连续出现多个时只替换第一个为\0。
参数：
	str：指向欲分割的字符串
	delim：为分割字符串中包含的所有字符
返回值：
	成功：分割后字符串首地址
	失败：NULL
```

- 在第一次调用时：strtok()必需给予参数s字符串
- 往后的调用则将参数s设置成**NULL**，每次调用成功则返回指向被分割出片段的指针

```c
	char a[100] = "adcfvcv.ebcyhghbdfg$casdert";
	char *s = strtok(a, ".$");//将""分割的子串取出
	while (s != NULL)
	{
		printf("%s\n", s);
		s = strtok(NULL, "");
	}
```

12) atoi()
```c
#include <stdlib.h>
int atoi(const char *nptr);
功能：atoi()会扫描nptr字符串，跳过前面的空格字符，直到遇到数字或正负号才开始做转换，而遇到非数字或字符串结束符('\0')才结束转换，并将结果返回返回值。
参数：
	nptr：待转换的字符串
返回值：成功转换后整数
```

类似的函数有：
- atof()：把一个小数形式的字符串转化为一个浮点数。
- atol()：将一个字符串转化为long类型

```c
	char str1[] = "          -10";
	int num1 = atoi(str1);
	printf("num1 = %d\n", num1);

	char str2[] = "0.123";
	double num2 = atof(str2);
	printf("num2 = %lf\n", num2);

	char str3[] = "123L";
	long num3 = atol(str3);
	printf("num3 = %ld\n", num3);
```


## 指针小结



|      定义       |            说明            |
| :------------: | :------------------------: |
|    `int i`     |          定义整型变量          |
|   `int *p`     |      定义一个指向int的指针变量      |
|  `int a[10]`   | 定义一个有10个元素的数组，每个元素类型为int |
|  `int *p[10]`  | 定义一个有10个元素的数组，每个元素类型为int* |
| `int *func()`  |     定义一个函数，返回值为int*型     |
|  `int func()`  |     定义一个函数，返回值为int型      |
|   `int **p`    |   定义一个指向int的指针的指针，二级指针   |


# 内存管理
## 作用域
C语言变量的作用域分为：
- 代码块作用域(代码块是{}之间的一段代码)
- 函数作用域
- 文件作用域
### 局部变量
局部变量也叫auto自动变量(auto可写可不写)，一般情况下代码块{}内部定义的变量都是自动变量，它有如下特点：
- 在一个函数内定义，只在函数范围内有效
- 在复合语句中定义，只在复合语句中有效
- <font color=#ff0000>随着函数调用的结束或复合语句的结束局部变量的声明声明周期也结束</font>
- 如果没有赋初值，内容为随机
```c
#include <stdio.h>

void test()
{
	//auto写不写是一样的
	//auto只能出现在{}内部
	auto int b = 10; 
}

int main(void)
{
	//b = 100; //err， 在main作用域中没有b

	if (1)
	{
		//在复合语句中定义，只在复合语句中有效
		int a = 10;
		printf("a = %d\n", a);
	}

	//a = 10; //err离开if()的复合语句，a已经不存在
	
	return 0;
}
```

### 静态(static)局部变量
- static局部变量的作用域也是在定义的函数内有效
- static局部变量的生命周期和程序运行周期一样，同时staitc局部变量的值<font color=#ff0000>只初始化一次</font>，但可以赋值多次
- static局部变量若未赋以初值，则由系统自动赋值：数值型变量自动赋初值0，字符型变量赋空字符

```c
#include <stdio.h>
void fun1()
{
	int i = 0;
	i++;
	printf("i = %d\n", i);
}
void fun2()
{
	//静态局部变量，没有赋值，系统赋值为0，而且只会初始化一次
	static int a;
	a++;
	printf("a = %d\n", a);
}
int main(void)
{
	fun1();
	fun1();
	fun2();
	fun2();
	return 0;
}
```

### 全局变量

- 在函数外定义，可被本文件<font color=#ff0000>及其它文件中的函数所共用</font>，若其它文件中的函数调用此变量,==须用extern声明==(这是在本文件中，在其他文件定义的) ^extern
- 全局变量的生命周期和程序运行周期一样
- 不同文件的全局变量不可重名
 
### 静态(static)全局变量

- 在函数外定义,作用范围被<font color=#ff0000>限制在所定义的文件中</font>
- 不同文件静态全局变量可以重名,但作用域不冲突
- static全局变量的生命周期和程序运行周期一样，同时staitc全局变量的值只初始化一次

### extern全局变量声明
- extern int a;声明一个变量，这个全局变量在别的文件中已经定义了，这里只是声明，而不是定义。
- 没用extern 函数这一说，函数本身就是全局的，默认就是extern

```c
//文件1.cpp代码
 
#include<stdio.h>
extern int g_val;    //声明外部符号g_val
int main()
{
	printf("%d", g_val);
}
 
 
//文件2.cpp代码
int g_val = 10;
```
### 全局函数和静态函数

在C语言中函数默认都是全局的，使用关键字static可以将函数声明为静态，函数定义为static就意味着这个函数只能在定义这个函数的文件中使用，在其他文件中不能调用，即使在其他文件中声明这个函数都没用。

对于不同文件中的staitc函数名字可以相同。

<img src="https://article.biliimg.com/bfs/article/7aa898bed5825d51a11dd111416fb24e38716159.png" alt="image.png" style="zoom:50%;" />


> [!attention] 注意：
> - 允许在不同的函数中使用相同的变量名，它们代表不同的对象，分配不同的单元，互不干扰。
> - 同一源文件中,允许全局变量和局部变量同名，在局部变量的作用域内，全局变量不起作用。
> - 所有的函数默认都是全局的，意味着所有的函数都不能重名，但如果是staitc函数，那么作用域是文件级的，所以不同的文件static函数名是可以相同的。

### 总结

|     类型     |  作用域  | 生命周期    |
| :--------: | :---: | :------ |
|   auto变量   | 一对{}内 | 当前函数    |
| static局部变量 | 一对{}内 | 整个程序运行期 |
|  extern变量  | 整个程序  | 整个程序运行期 |
| static全局变量 | 当前文件  | 整个程序运行期 |
|  extern函数  | 整个程序  | 整个程序运行期 |
|  static函数  | 当前文件  | 整个程序运行期 |
| register变量 | 一对{}内 | 当前函数    |
|    全局变量    | 整个程序  | 整个程序运行期 |

## 内存布局
### 内存分区
C代码经过<font color=#ff0000>预处理、编译、汇编、链接</font>4步后生成一个可执行程序。
在 Windows 下，程序是一个普通的可执行文件，以下列出一个二进制可执行文件的基本情况：

<img src="https://article.biliimg.com/bfs/article/24098a3e60b8f5c4e9140e321799f34a38716159.png" alt="image.png" style="zoom:50%;" />


通过上图可以得知，在没有运行程序前，也就是说<font color=#ff0000>程序没有加载到内存前</font>，可执行程序内部已经分好3段信息，分别为<font color=#ff0000>代码区(text)、数据区(data)和未初始化数据区(bss)</font>3个部分(有些人直接把data和bss合起来叫做静态区或全局区)。

- 代码区
存放CPU执行的机器指令。通常代码区是可共享的(即另外的执行程序可以调用它)，使其可共享的目的是对于频繁被执行的程序，只需要在内存中有一份代码即可。<font color=#ff0000>代码区通常是只读的</font>，使其只读的原因是防止程序意外地修改了它的指令。另外，代码区还规划了局部变量的相关信息。

- 全局初始化数据区/静态数据区(data段)
该区包含了在程序中明确被初始化的全局变量、已经初始化的静态变量(包括全局静态变量和局部静态变量)和常量数据(如字符串常量)。

- 未初始化数据区(又叫bss区)
存入的是全局未初始化变量和未初始化静态变量。未初始化数据区的数据在程序开始执行之前被内核初始化为 0 或者空(NULL)。

程序在加载到内存前，<font color=#ff0000>代码区和全局区(data和bss)的大小就是固定的</font>，程序运行期间不能改变。然后，运行可执行程序，系统把程序加载到内存，除了根据可执行程序的信息分出代码区(text)、数据区(data)和未初始化数据区(bss)之外，还额外增加了栈区、堆区。

<img src="https://article.biliimg.com/bfs/article/01aa9c18c55a314f139dde1c7951e53938716159.png" alt="image.png" style="zoom:70%;" />

- 代码区(text segment)
加载的是可执行文件代码段，所有的可执行代码都加载到代码区，这块内存是不可以在运行期间修改的。

- 未初始化数据区(BSS)
加载的是可执行文件BSS段，位置可以分开亦可以紧靠数据段，存储于数据段的数据(全局未初始化，静态未初始化数据)的生存周期为整个程序运行过程。

- 全局初始化数据区/静态数据区(data segment)
加载的是可执行文件数据段，存储于数据段(全局初始化，静态初始化数据，文字常量(只读))的数据的生存周期为整个程序运行过程。

- 栈区(stack)
栈是一种先进后出的内存结构，由编译器自动分配释放，存放函数的参数值、返回值、局部变量等。在程序运行过程中实时加载和释放，因此，局部变量的生存周期为申请到释放该段栈空间。
<font color=#ff0000>stack：栈。 在其之上开辟 栈帧。windows 1M --- 10M	Linux： 8M --- 16M</font>

- 堆区(heap)
堆是一个大容器，它的容量<font color=#ff0000>要远远大于栈</font>，但没有栈那样先进后出的顺序。用于动态内存分配。堆在内存中位于BSS区和栈区之间。一般由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收。
<font color=#ff0000>heap：堆。 给用户自定义数据提供空间。 约 1.3G</font>

<img src="https://article.biliimg.com/bfs/article/1a12b0ed8e1f06942a4bc89b09167dda38716159.png" alt="image.png" style="zoom:50%;" />


### 存储类型总结

|     类型     |  作用域  |  生命周期   |        存储位置         |
| :--------: | :---: | :-----: | :-----------------: |
|   auto变量   | 一对{}内 |  当前函数   |         栈区          |
| static局部变量 | 一对{}内 | 整个程序运行期 | 初始化在data段，未初始化在BSS段 |
|  extern变量  | 整个程序  | 整个程序运行期 | 初始化在data段，未初始化在BSS段 |
| static全局变量 | 当前文件  | 整个程序运行期 | 初始化在data段，未初始化在BSS段 |
|  extern函数  | 整个程序  | 整个程序运行期 |         代码区         |
|  static函数  | 当前文件  | 整个程序运行期 |         代码区         |
| register变量 | 一对{}内 |  当前函数   |    运行时存储在CPU寄存器     |
|   字符串常量    | 当前文件  | 整个程序运行期 |        data段        |

```c
#include <stdio.h>
#include <stdlib.h>

int e;
static int f;
int g = 10;
static int h = 10;
int main()
{
	int a;
	int b = 10;
	static int c;
	static int d = 10;
	char i = "test";
	char k = NULL;
	printf("&a\t %p\t //局部未初始化变量\n", &a);
	printf("&b\t %p\t //局部初始化变量\n", &b);
	printf("&c\t %p\t //静态局部未初始化变量\n", &c);
	printf("&d\t %p\t //静态局部初始化变量\n", &d);
	printf("&e\t %p\t //全局未初始化变量\n", &e);
	printf("&f\t %p\t //全局静态未初始化变量\n", &f);
	printf("&g\t %p\t //全局初始化变量\n", &g);
	printf("&h\t %p\t //全局静态初始化变量\n", &h);
	printf("i\t %p\t //只读数据(文字常量区)\n", i);
	k = (char )malloc(10);
	printf("k\t %p\t //动态分配的内存\n", k);
	return 0;
}
```


### 内存操作函数
1) memset()
```c
#include <string.h>
void memset(void s, int c, size_t n);
功能：将s的内存区域的前n个字节以参数c填入
参数：
	s：需要操作内存s的首地址
	c：填充的字符，c虽然参数为int，但必须是unsigned char , 范围为0~255
	n：指定需要设置的大小
返回值：s的首地址
```

类似还有一个`bzero`

```c
/ Set N bytes of S to 0.  /
extern void bzero (void __s, size_t __n) __THROW __nonnull ((1));
功能：是将指定内存区域的内容清零
```

```c
	int a[10];
	memset(a, 0, sizeof(a));
	memset(a, 97, sizeof(a));
	int i = 0;
	for (i = 0; i < 10; i++)
	{
		printf("%c\n", a[i]);
	}
```

2) memcpy()
```c
#include <string.h>
void memcpy(void dest, const void src, size_t n);
功能：拷贝src所指的内存内容的前n个字节到dest所值的内存地址上。
参数：
	dest：目的内存首地址
	src：源内存首地址，注意：dest和src所指的内存空间不可重叠，可能会导致程序报错
	n：需要拷贝的字节数
返回值：dest的首地址
```

```c
	int a[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
	int b[10];
	
	memcpy(b, a, sizeof(a));
	int i = 0;
	for (i = 0; i < 10; i++)
	{
		printf("%d, ", b[i]);
	}
	printf("\n");

	//memcpy(&a[3], a, 5  sizeof(int)); //err, 内存重叠
```

3) memmove()
	memmove()功能用法和memcpy()一样，区别在于：dest和src所指的内存空间重叠时，memmove()仍然能处理，不过执行效率比memcpy()低些。

4) memcmp()
```c
#include <string.h>
int memcmp(const void* s1, const void* s2, size_t n);
功能：比较s1和s2所指向内存区域的前n个字节
参数：
	s1：内存首地址1
	s2：内存首地址2
	n：需比较的前n个字节
返回值：
	相等：=0
	大于：>0
	小于：<0
```

```c
	int a[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
	int b[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

	int flag = memcmp(a, b, sizeof(a));
	printf("flag = %d\n", flag);
```

### 堆区内存分配和释放
1)malloc() 
```c
#include <stdlib.h>
void malloc(size_t size);
功能：在内存的动态存储区(堆区)中分配一块长度为size字节的连续区域，用来存放类型说明符指定的类型。分配的内存空间内容不确定，一般使用memset初始化。
参数：
	size：需要分配内存大小(单位：字节)
返回值：
成功：分配空间的起始地址
失败：NULL
```

```c
#include <stdlib.h> 
#include <stdio.h>
#include <string.h>

int main()
{
	int count, array, n;
	printf("请输入要申请数组的个数:\n");
	scanf("%d", &n);

	array = (int *)malloc(n *sizeof(int));
	if (array == NULL)
	{
		printf("申请空间失败!\n");
		return -1;
	}
	//将申请到空间清0
	memset(array, 0, sizeof(int)*n);

	for (count = 0; count < n; count++) /给数组赋值/
		array[count] = count;

	for (count = 0; count < n; count++) /打印数组元素/
		printf("%2d", array[count]);

	free(array);// free后的空间，不会立即失效。 通常将free后的 地址置为NULL。
	return 0;
}
```

2)free()
```c
#include <stdlib.h>
void free(void ptr);
功能：释放ptr所指向的一块内存空间，ptr是一个任意类型的指针变量，指向被释放区域的首地址。对同一内存空间多次释放会出错。
参数：
ptr：需要释放空间的首地址，被释放区应是由malloc函数所分配的区域。
返回值：无
```

## 内存分区代码分析

1) 返回栈区地址
```c
#include <stdio.h>
int fun()
{
	int a = 10;
	return &a;//函数调用完毕，a释放
}

int main(int argc, char argv[])
{
	int p = NULL;
	p = fun();
	p = 100; //操作野指针指向的内存,err

	return 0;
}
```

2) 返回data区地址
```c
#include <stdio.h>

int fun()
{
	static int a = 10;
	return &a; //函数调用完毕，a不释放
}

int main(int argc, char argv[])
{
	int p = NULL;
	p = fun();
	p = 100; //ok
	printf("p = %d\n", p);

	return 0;
}
```

3) 值传递1
```c
#include <stdio.h>
#include <stdlib.h>

void fun(int tmp)
{
	tmp = (int )malloc(sizeof(int));
	tmp = 100;
}

int main(int argc, char argv[])
{
	int p = NULL;
	fun(p); //值传递，形参修改不会影响实参
	printf("p = %d\n", p);//err，操作空指针指向的内存

	return 0;
}
```

4) 值传递2
```c
#include <stdio.h>
#include <stdlib.h>

void fun(int tmp)
{
	tmp = 100;
}

int main(int argc, char argv[])
{
	int p = NULL;
	p = (int *)malloc(sizeof(int));

	fun(p); //值传递
	printf("p = %d\n", p); //ok，p为100

	return 0;
}
```

5) 返回堆区地址
```c
#include <stdio.h>
#include <stdlib.h>
int fun()
{
	int tmp = NULL;
	tmp = (int *)malloc(sizeof(int));
	tmp = 100;
	return tmp;//返回堆区地址，函数调用完毕，不释放
}
int main(int argc, char argv[])
{
	int p = NULL;
	p = fun();
	printf("p = %d\n", *p);//ok
	//堆区空间，使用完毕，手动释放
	if (p != NULL)
	{
		free(p);
		p = NULL;
	}
	return 0;
}
```


# 复合类型(自定义类型)
## 结构体
### 概述
数组：描述一组具有相同类型数据的有序集合，用于处理大量相同类型的数据运算。

有时我们需要将不同类型的数据组合成一个有机的整体，如：一个学生有学号/姓名/性别/年龄/地址等属性。显然单独定义以上变量比较繁琐，数据不便于管理。

Ｃ语言中给出了另一种构造数据类型——结构体。

<img src="https://article.biliimg.com/bfs/article/252c7243817ff0ac074ca59ec565b1f038716159.png" alt="image.png" style="zoom:50%;" />

### 结构体变量的定义和初始化

定义结构体变量的方式：
- 先声明结构体类型再定义变量名
- 在声明类型的同时定义变量
- 直接定义结构体类型变量(无类型名)

<img src="https://article.biliimg.com/bfs/article/c8fc94738c6fbeff920536b33512f2f638716159.png" alt="image.png" style="zoom:50%;" />



结构体类型和结构体变量关系：
- 结构体类型：指定了一个结构体类型，它相当于一个模型，但其中并无具体数据，系统对之也不分配实际内存单元。
- 结构体变量：系统根据结构体类型(内部成员状况)为之分配空间。


```c
//结构体类型的定义
struct stu
{
	char name[50];
	int age;
};

//先定义类型，再定义变量(常用)
struct stu s1 = { "mike", 18 };


//定义类型同时定义变量
struct stu2
{
	char name[50];
	int age;
}s2 = { "lily", 22 };

struct
{
	char name[50];
	int age;
}s3 = { "yuri", 25 };
```

### 结构体成员的使用
```c
#include<stdio.h>
#include<string.h>

//结构体类型的定义
struct stu
{
	char name[50];
	int age;
};

int main()
{
	struct stu s1;

	//如果是普通变量，通过点运算符操作结构体成员
	strcpy(s1.name, "abc");
	s1.age = 18;
	printf("s1.name = %s, s1.age = %d\n", s1.name, s1.age);

	//如果是指针变量，通过->操作结构体成员
	strcpy((&s1)->name, "test");
	(&s1)->age = 22;
	printf("(&s1)->name = %s, (&s1)->age = %d\n", (&s1)->name, (&s1)->age);

	return 0;
}
```

### 结构体数组
```c
#include <stdio.h>

//统计学生成绩
struct stu
{
	int num;
	char name[20];
	char sex;
	float score;
};

int main()
{
	//定义一个含有5个元素的结构体数组并将其初始化
	struct stu boy[5] = {
		{ 101, "Li ping", 'M', 45 },			
		{ 102, "Zhang ping", 'M', 62.5 },
		{ 103, "He fang", 'F', 92.5 },
		{ 104, "Cheng ling", 'F', 87 },
		{ 105, "Wang ming", 'M', 58 }};

	int i = 0;
	int c = 0;
	float ave, s = 0;
	for (i = 0; i < 5; i++)
	{
		s += boy[i].score;	//计算总分
		if (boy[i].score < 60)
		{
			c += 1;		//统计不及格人的分数
		}
	}

	printf("s=%f\n", s);//打印总分数
	ave = s / 5;					//计算平均分数
	printf("average=%f\ncount=%d\n\n", ave, c); //打印平均分与不及格人数
	for (i = 0; i < 5; i++)
	{
		printf(" name=%s,  score=%f\n", boy[i].name, boy[i].score);
           // printf(" name=%s,  score=%f\n", (boy+i)->name, (boy+i)->score);

	}
	return 0;
}
```

### 结构体套结构体
```c
#include <stdio.h>

struct person
{
	char name[20];
	char sex;
};

struct stu
{
	int id;
	struct person info;
};

int main()
{
	struct stu s[2] = { 1, "lily", 'F', 2, "yuri", 'M' };

	int i = 0;
	for (i = 0; i < 2; i++)
	{
		printf("id = %d\tinfo.name=%s\tinfo.sex=%c\n", s[i].id, s[i].info.name, s[i].info.sex);
	}

	return 0;
}
```

### 结构体赋值
```c
#include<stdio.h>
#include<string.h>

//结构体类型的定义
struct stu
{
	char name[50];
	int age;
};

int main()
{
	struct stu s1;

	//如果是普通变量，通过点运算符操作结构体成员
	strcpy(s1.name, "abc");
	s1.age = 18;
	printf("s1.name = %s, s1.age = %d\n", s1.name, s1.age);

	//相同类型的两个结构体变量，可以相互赋值
	//把s1成员变量的值拷贝给s2成员变量的内存
	//s1和s2只是成员变量的值一样而已，它们还是没有关系的两个变量
	struct stu s2 = s1;
	//memcpy(&s2, &s1, sizeof(s1));
	printf("s2.name = %s, s2.age = %d\n", s2.name, s2.age);
	return 0;
}
```

### 结构体和指针
1)指向普通结构体变量的指针
```c
#include<stdio.h>

//结构体类型的定义
struct stu
{
	char name[50];
	int age;
};

int main()
{
	struct stu s1 = { "lily", 18 };

	//如果是指针变量，通过->操作结构体成员
	struct stu p = &s1;
	printf("p->name = %s, p->age=%d\n", p->name, p->age);
	printf("(p).name = %s, (p).age=%d\n",  (p).name,  (p).age);

	return 0;
}
```

2)堆区结构体变量
```c
#include<stdio.h>
#include <string.h>
#include <stdlib.h>

//结构体类型的定义
struct stu
{
	char name[50];
	int age;
};

int main()
{
	struct stu p = NULL;

	p = (struct stu )malloc(sizeof(struct  stu));

	//如果是指针变量，通过->操作结构体成员
	strcpy(p->name, "test");
	p->age = 22;

	printf("p->name = %s, p->age=%d\n", p->name, p->age);
	printf("(p).name = %s, (p).age=%d\n", (p).name,  (p).age);

	free(p);
	p = NULL;

	return 0;
}
```

3)结构体套一级指针
```c
#include<stdio.h>
#include <string.h>
#include <stdlib.h>
//结构体类型的定义
struct stu
{
	char name; //一级指针
	int age;
};
int main()
{
	struct stu p = NULL;
	p = (struct stu )malloc(sizeof(struct  stu));
	p->name = malloc(strlen("test") + 1);
	strcpy(p->name, "test");
	p->age = 22;
	printf("p->name = %s, p->age=%d\n", p->name, p->age);
	printf("(p).name = %s, (p).age=%d\n", (p).name, (p).age);
	if (p->name != NULL)
	{
		free(p->name);
		p->name = NULL;
	}
	if (p != NULL)
	{
		free(p);
		p = NULL;
	}
	return 0;
}
```


### 结构体做函数参数
1)结构体普通变量做函数参数
```c
#include<stdio.h>
#include <string.h>

//结构体类型的定义
struct stu
{
	char name[50];
	int age;
};

//函数参数为结构体普通变量
void set_stu(struct stu tmp)
{
	strcpy(tmp.name, "mike");
	tmp.age = 18;
	printf("tmp.name = %s, tmp.age = %d\n", tmp.name, tmp.age);
}

int main()
{
	struct stu s = { 0 };
	set_stu(s); //值传递
	printf("s.name = %s, s.age = %d\n", s.name, s.age);

	return 0;
}
```

2)结构体指针变量做函数参数
```c
#include<stdio.h>
#include <string.h>

//结构体类型的定义
struct stu
{
	char name[50];
	int age;
};

//函数参数为结构体指针变量
void set_stu_pro(struct stu tmp)
{
	strcpy(tmp->name, "mike");
	tmp->age = 18;
}

int main()
{
	struct stu s = { 0 };
	set_stu_pro(&s); //地址传递
	printf("s.name = %s, s.age = %d\n", s.name, s.age);

	return 0;
}
```

3)结构体数组名做函数参数
```c
#include<stdio.h>

//结构体类型的定义
struct stu
{
	char name[50];
	int age;
};

//void set_stu_pro(struct stu tmp[100], int n)
//void set_stu_pro(struct stu tmp[], int n)
void set_stu_pro(struct stu tmp, int n)
{
	int i = 0;
	for (i = 0; i < n; i++)
	{
		sprintf(tmp->name, "name%d%d%d", i, i, i);
		tmp->age = 20 + i;
		tmp++;
	}
}

int main()
{
	struct stu s[3] = { 0 };
	int i = 0;
	int n = sizeof(s) / sizeof(s[0]);
	set_stu_pro(s, n); //数组名传递

	for (i = 0; i < n; i++)
	{
		printf("%s, %d\n", s[i].name, s[i].age);
	}
	return 0;
}
```

4) const修饰结构体指针形参变量
```c
//结构体类型的定义
struct stu
{
	char name[50];
	int age;
};

void fun1(struct stu  const p)
{
	//p = NULL; //err
	p->age = 10; //ok
}

//void fun2(struct stu const  p)
void fun2(const struct stu   p)
{
	p = NULL; //ok
	//p->age = 10; //err
}

void fun3(const struct stu  const p)
{
	//p = NULL; //err
	//p->age = 10; //err
}
```

## 共用体(联合体)

- 联合union是一个能在<font color=#ff0000>同一个存储空间</font>存储不同类型数据的类型；
- 联合体所占的内存长度等于其最长成员的长度倍数，也有叫做共用体；
- 同一内存段可以用来存放几种不同类型的成员，但每一瞬时只有一种起作用；
- 共用体变量中起作用的成员是<font color=#ff0000>最后一次存放的成员</font>，在存入一个新的成员后原有的成员的值会被覆盖；
- 共用体变量的地址和它的各成员的地址都是同一地址。

```c
#include <stdio.h>

//共用体也叫联合体 
union Test
{
	unsigned char a;
	unsigned int b;
	unsigned short c;
};
int main()
{
	//定义共用体变量
	union Test tmp;
	//1、所有成员的首地址是一样的
	printf("%p, %p, %p\n", &(tmp.a), &(tmp.b), &(tmp.c));
	//2、共用体大小为最大成员类型的大小
	printf("%lu\n", sizeof(union Test));
	//3、一个成员赋值，会影响另外的成员
	//左边是高位，右边是低位
	//低位放低地址，高位放高地址
	tmp.b = 0x44332211;
	printf("%x\n", tmp.a); //11
	printf("%x\n", tmp.c); //2211
	tmp.a = 0x00;
	printf("short: %x\n", tmp.c); //2200
	printf("int: %x\n", tmp.b); //44332200
	return 0;
}
```

## 枚举
枚举：将变量的值一一列举出来，变量的值只限于列举出来的值的范围内。

枚举类型定义：
```c
enum  枚举名
{
	枚举值表
};
```

- 在枚举值表中应列出所有可用值，也称为枚举元素。
- <font color=#ff0000>枚举值是常量</font>，不能在程序中用赋值语句再对它赋值。
- 举元素本身由系统定义了一个表示序号的数值从0开始顺序定义为0，1，2 …


```c
#include <stdio.h>
enum weekday
{
	sun = 2, mon, tue, wed, thu, fri, sat;
} ;
enum bool
{
	flase, true
};
int main()
{
	enum weekday a, b, c;
	a = sun;
	b = mon;
	c = tue;
	printf("%d,%d,%d\n", a, b, c);
	enum bool flag;
	flag = true;

	if (flag == 1)
	{
		printf("flag为真\n");
	}
	return 0;
}
```

## typedef
typedef为C语言的关键字，作用是为一种数据类型(基本类型或自定义数据类型)定义一个新名字，<font color=#ff0000>不能创建新类型</font>。

- 与#define不同，typedef仅限于数据类型，而不是能是表达式或具体的值
- `#define`发生在预处理，typedef发生在编译阶段
```c
#include <stdio.h>

typedef int INT;
typedef char BYTE;
typedef BYTE T_BYTE;
typedef unsigned char UBYTE;

typedef struct type
{
	UBYTE a;
	INT b;
	T_BYTE c;
}TYPE, PTYPE;

int main()
{
	TYPE t;
	t.a = 254;
	t.b = 10;
	t.c = 'c';

	PTYPE p = &t;
	printf("%u, %d, %c\n", p->a, p->b, p->c);

	return 0;
}`
```


# 文件操作

## 概述

### 磁盘文件和设备文件
- 磁盘文件
指一组相关数据的有序集合,通常存储在外部介质(如磁盘)上，使用时才调入内存。

- 设备文件
在操作系统中把每一个与主机相连的输入、输出设备看作是一个文件，把它们的输入、输出等同于对磁盘文件的读和写。屏幕、键盘、鼠标、显卡、声卡。

### 磁盘文件的分类
计算机的存储在物理上是二进制的，所以物理上所有的磁盘文件本质上都是一样的：以字节为单位进行顺序存储。

<img src="https://article.biliimg.com/bfs/article/596156689d5da8e2a08d37c7a27d87af38716159.png" alt="image.png" style="zoom:60%;" />

从用户或者操作系统使用的角度(逻辑上)把文件分为：
- 文本文件：基于字符编码的文件  
- 二进制文件：基于值编码的文件

### 文本文件和二进制文件
1)文本文件
- 基于字符编码，常见编码有ASCII、UNICODE等
- 一般可以使用文本编辑器直接打开
- 数5678的以ASCII存储形式(ASCII码)为：
	00110101 00110110 00110111 00111000

2)二进制文件
- 基于值编码,自己根据具体应用,指定某个值是什么意思
- 把内存中的数据按其在内存中的存储形式原样输出到磁盘上
- 数5678的存储形式(二进制码)为：
- 00010110 00101110
	
## 文件的打开和关闭
### 文件指针
在C语言中用一个指针变量指向一个文件，这个指针称为文件指针。 
```c
typedef struct
{
	short           level;	//缓冲区"满"或者"空"的程度 
	unsigned        flags;	//文件状态标志 
	char            fd;		//文件描述符
	unsigned char   hold;	//如无缓冲区不读取字符
	short           bsize;	//缓冲区的大小
	unsigned char   buffer;//数据缓冲区的位置 
	unsigned        ar;	 //指针，当前的指向 
	unsigned        istemp;	//临时文件，指示器
	short           token;	//用于有效性的检查 
}FILE;
```

磁盘的数据不能直接访问，必须加载到内存中才能正常访问。这个`typedef struct` 是磁盘数据中的的映射来自内存
FILE是系统使用typedef定义出来的有关文件信息的一种结构体类型，<font color=#ff0000>结构中含有文件名、文件状态和文件当前位置等信息。</font>

声明FILE结构体类型的信息包含在头文件`“stdio.h”`中，一般设置一个指向FILE类型变量的指针变量，然后通过它来引用这些FILE类型变量。通过文件指针就可对它所指的文件进行各种操作。

<img src="https://article.biliimg.com/bfs/article/d7d70bf853cc1b82189a1f351e6a1c9c38716159.png" alt="image.png" style="zoom:50%;" />


C语言中有三个特殊的文件指针由系统默认打开，<font color=#ff0000>用户无需定义即可直接使用</font>:
- stdin： 标准输入，默认为当前终端(键盘)，我们使用的scanf、getchar函数默认从此终端获得数据。
- stdout：标准输出，默认为当前终端(屏幕)，我们使用的printf、puts函数默认输出信息到此终端。
- stderr：标准出错，默认为当前终端(屏幕)，我们使用的perror函数默认输出信息到此终端。

### 文件的打开
任何文件使用之前必须打开：
```c
#include <stdio.h>
FILE  fopen(const char  filename, const char  mode);
功能：打开文件
参数：
	filename：需要打开的文件名，根据需要加上路径
	mode：打开文件的模式设置
返回值：
	成功：文件指针
	失败：NULL
```

第一个参数的几种形式:
```c
	FILE fp_passwd = NULL;

	//相对路径：
	//打开当前目录passdw文件：源文件(源程序)所在目录
	FILE fp_passwd = fopen("passwd.txt", "r");
	
	//打开当前目录(test)下passwd.txt文件
	fp_passwd = fopen(". / test / passwd.txt", "r");
	
	//打开当前目录上一级目录(相对当前目录)passwd.txt文件
	fp_passwd = fopen(".. / passwd.txt", "r");
		
	//绝对路径：
	//打开C盘test目录下一个叫passwd.txt文件
	fp_passwd = fopen("c:/test/passwd.txt","r");
```

第二个参数的几种形式(打开文件的方式)：


|  打开模式  |                                 含义                                  |
| :----: | :-----------------------------------------------------------------: |
|  r或rb  |                   以只读方式打开一个文本文件(不创建文件，若文件不存在则报错)                    |
|  w或wb  |                 以写方式打开文件(如果文件存在则清空文件，文件不存在则创建一个文件)                  |
|  a或ab  |                    以追加方式打开文件，在末尾添加内容，若文件不存在则创建文件                    |
| r+或rb+ |                        以可读、可写的方式打开文件(不创建新文件)                        |
| w+或wb+ |              以可读、可写的方式打开文件(如果文件存在则清空文件，文件不存在则创建一个 文件)               |
| a+或ab+ | 以添加方式打开可读、可写的文件。若文件不存在则创建 文件；如果 文件存在，则写入的数据会被加到 文件尾后，即 文件原先的内容会被保留。 |

注意：
- b是二进制模式的意思，b只是在Windows有效，在Linux用r和rb的结果是一样的
- Unix和Linux下所有的文本文件行都是`\n`结尾，而Windows所有的文本文件行都是`\r\n`结尾
- 在Windows平台下，以“文本”方式打开文件，不加b：
	- 当读取文件的时候，系统会将所有的 "`\r\n`" 转换成 `"\n"`
	- 当写入文件的时候，系统会将 `"\n"` 转换成 `"\r\n"` 写入 
	- <font color=#ff0000>以"二进制"方式打开文件，则读写都不会进行这样的转换</font>
-  在Unix/Linux平台下，“文本”与“二进制”模式没有区别，`"\r\n"` 作为两个字符原样输入输出

```c
int main(void)
{
	FILE fp = NULL;

	// "\\"这样的路径形式，只能在windows使用
	// "/"这样的路径形式，windows和linux平台下都可用，建议使用这种
	// 路径可以是相对路径，也可是绝对路径
	fp = fopen("../test", "w");
	//fp = fopen("..\\test", "w");

	if (fp == NULL) //返回空，说明打开失败
	{
		//perror()是标准出错打印函数，能打印调用库函数出错原因
		perror("open");
		return -1;
	}

	return 0;
}
```

### 文件的关闭
任何文件在使用后应该关闭：
- 打开的文件会占用内存资源，如果总是打开不关闭，会消耗很多内存
- 一个进程同时打开的文件数是有限制的，超过最大同时打开文件数，再次调用fopen打开文件会失败
- 如果没有明确的调用fclose关闭打开的文件，那么程序在退出的时候，操作系统会统一关闭。

```c
#include <stdio.h>
int fclose(FILE  stream);
功能：关闭先前fopen()打开的文件。此动作让缓冲区的数据写入文件中，并释放系统所提供的文件资源。
参数：
	stream：文件指针
返回值：
	成功：0
	失败：-1
```

```c
	FILE  fp = NULL;
	fp = fopen("abc.txt", "r");
	fclose(fp);
```

## 文件的顺序读写
### 按照字符读写文件fgetc、fputc
1)写文件
```c
#include <stdio.h>
int fputc(int ch, FILE  stream);
功能：将ch转换为unsigned char后写入stream指定的文件中
参数：
	ch：需要写入文件的字符
	stream：文件指针
返回值：
	成功：成功写入文件的字符
	失败：返回-1
```

```c
char buf[] = "this is a test for fputc";
int i = 0;
int n = strlen(buf);
for (i = 0; i < n; i++)
{
	//往文件fp写入字符buf[i]
	int ch = fputc(buf[i], fp);
	printf("ch = %c\n", ch);
}
```

2)文件结尾

在C语言中，`EOF`表示文件结束符(end of file)。在while循环中以EOF作为文件结束标志，<font color=#ff0000>这种以EOF作为文件结束标志的文件，必须是文本文件</font>。在文本文件中，数据都是以字符的ASCII代码值的形式存放。我们知道，ASCII代码值的范围是0~127，不可能出现-1，因此可以用EOF作为文件结束标志。

`#define EOF    (-1)`

当把数据以二进制形式存放到文件中时，就会有-1值的出现，因此不能采用EOF作为二进制文件的结束标志。为解决这一个问题，ANSI C提供一个feof函数，用来判断文件是否结束。<font color=#ff0000>feof函数既可用以判断二进制文件又可用以判断文本文件。</font>
```c
#include <stdio.h>
int feof(FILE  stream);
功能：检测是否读取到了文件结尾。判断的是最后一次“读操作的内容”，不是当前位置内容(上一个内容)。
参数：
	stream：文件指针
返回值：
	非0值：已经到文件结尾
	0：没有到文件结尾
```

3)读文件
```c
#include <stdio.h>
int fgetc(FILE  stream);
功能：从stream指定的文件中读取一个字符
参数：
	stream：文件指针
返回值：
	成功：返回读取到的字符
	失败：-1
```

```c
char ch;
#if 0
while ((ch = fgetc(fp)) != EOF)
{
	printf("%c", ch);
}
printf("\n");
#endif
while (!feof(fp)) //文件没有结束，则执行循环
{
	ch = fgetc(fp);
	printf("%c", ch);
}
printf("\n");
```

### 按照行读写文件fgets、fputs
1)写文件
```c
#include <stdio.h>
int fputs(const char  str, FILE  stream);
功能：将str所指定的字符串写入到stream指定的文件中，字符串结束符 '\0'  不写入文件。 
参数：
	str：字符串
	stream：文件指针
返回值：
	成功：0
	失败：-1
```


```c
char buf[] = { "123456\n", "bbbbbbbbbb\n", "ccccccccccc\n" };
int i = 0;
int n = 3;
for (i = 0; i < n; i++)
{
	int len = fputs(buf[i], fp);
	printf("len = %d\n", len);
}
```
2)读文件
```c
#include <stdio.h>
char  fgets(char  str, int size, FILE  stream);
功能：从stream指定的文件内读入字符，保存到str所指定的内存空间，直到出现换行字符、读到文件结尾或是已读了size - 1个字符为止，最后会自动加上字符 '\0' 作为字符串结束。
参数：
	str：字符串
	size：指定最大读取字符串的长度(size - 1)
	stream：文件指针
返回值：
	成功：成功读取的字符串
	读到文件尾或出错： NULL
```

```c
char buf[100] = 0;

while (!feof(fp)) //文件没有结束
{
	memset(buf, 0, sizeof(buf));
	char p = fgets(buf, sizeof(buf), fp);
	if (p != NULL)
	{
		printf("buf = %s", buf);
	}
}
```


### 按照格式化文件fprintf、fscanf
1)写文件
```c
#include <stdio.h>
int fprintf(FILE  stream, const char  format, ...);
功能：根据参数format字符串来转换并格式化数据，然后将结果输出到stream指定的文件中，指定出现字符串结束符 '\0'  为止。
参数：
	stream：已经打开的文件
	format：字符串格式，用法和printf()一样
返回值：
	成功：实际写入文件的字符个数
	失败：-1
```

`fprintf(fp, "%d %d %d\n", 1, 2, 3);`

2)读文件
```c
#include <stdio.h>
int fscanf(FILE  stream, const char  format, ...);
功能：从stream指定的文件读取字符串，并根据参数format字符串来转换并格式化数据。
参数：
	stream：已经打开的文件
	format：字符串格式，用法和scanf()一样
返回值：
	成功：参数数目，成功转换的值的个数
	失败： - 1
```

```c
int a = 0;
int b = 0;
int c = 0;
fscanf(fp, "%d %d %d\n", &a, &b, &c);
printf("a = %d, b = %d, c = %d\n", a, b, c);
```


### 按照块读写文件fread、fwrite
1)写文件
```c
#include <stdio.h>
size_t fwrite(const void ptr, size_t size, size_t nmemb, FILE stream);
功能：以数据块的方式给文件写入内容
参数：
	ptr：准备写入文件数据的地址
	size： size_t 为 unsigned int类型，此参数指定写入文件内容的块数据大小
	nmemb：写入文件的块数，写入文件数据总大小为：size  nmemb
	stream：已经打开的文件指针
返回值：
	成功：实际成功写入文件数据的块数目，此值和 nmemb 相等
	失败：0
```


```c
typedef struct Stu
{
	char name[50];
	int id;
}Stu;

Stu s[3];
int i = 0;
for (i = 0; i < 3; i++)
{
	sprintf(s[i].name, "stu%d%d%d", i, i, i);
	s[i].id = i + 1;
}

int ret = fwrite(s, sizeof(Stu), 3, fp);
printf("ret = %d\n", ret);
```

2)读文件
```c
#include <stdio.h>
size_t fread(void ptr, size_t size, size_t nmemb, FILE stream);
功能：以数据块的方式从文件中读取内容
参数：
	ptr：存放读取出来数据的内存空间
	size： size_t 为 unsigned int类型，此参数指定读取文件内容的块数据大小
	nmemb：读取文件的块数，读取文件数据总大小为：size  nmemb
	stream：已经打开的文件指针
返回值：
	成功：实际成功读取到内容的块数，如果此值比nmemb小，但大于0，说明读到文件的结尾。
	失败：0
	0: 表示读到文件结尾。(feof())
```

```c
typedef struct Stu
{
	char name[50];
	int id;
}Stu;

Stu s[3];
int ret = fread(s, sizeof(Stu), 3, fp);
printf("ret = %d\n", ret);

int i = 0;
for (i = 0; i < 3; i++)
{
	printf("s = %s, %d\n", s[i].name, s[i].id);
}
```


## 文件的随机读写
```C
#include <stdio.h>
int fseek(FILE stream, long offset, int whence);
功能：移动文件流(文件光标)的读写位置。
参数：
	stream：已经打开的文件指针
	offset：根据whence来移动的位移数(偏移量)，可以是正数，也可以负数，如果正数，则相对于whence往右移动，如果是负数，则相对于whence往左移动。如果向前移动的字节数超过了文件开头则出错返回，如果向后移动的字节数超过了文件末尾，再次写入时将增大文件尺寸。
	whence：其取值如下：
		SEEK_SET：从文件开头移动offset个字节
		SEEK_CUR：从当前位置移动offset个字节
		SEEK_END：从文件末尾移动offset个字节
返回值：
	成功：0
	失败：-1

#include <stdio.h>
long ftell(FILE stream);
功能：获取文件流(文件光标)的读写位置。
参数：
	stream：已经打开的文件指针
返回值：
	成功：当前文件流(文件光标)的读写位置
	失败：-1

#include <stdio.h>
void rewind(FILE stream);
功能：把文件流(文件光标)的读写位置移动到文件开头。
参数：
	stream：已经打开的文件指针
返回值：
	无返回值
```

- 计算文件大小

```c
int size=lseek(fd,0,SEEK_END);
```

```c
typedef struct Stu
{
	char name[50];
	int id;
}Stu;

//假如已经往文件写入3个结构体
//fwrite(s, sizeof(Stu), 3, fp);

Stu s[3];
Stu tmp; 
int ret = 0;

//文件光标读写位置从开头往右移动2个结构体的位置
fseek(fp, 2  sizeof(Stu), SEEK_SET);

//读第3个结构体
ret = fread(&tmp, sizeof(Stu), 1, fp);
if (ret == 1)
{
	printf("[tmp]%s, %d\n", tmp.name, tmp.id);
}

//把文件光标移动到文件开头
//fseek(fp, 0, SEEK_SET);
rewind(fp);

ret = fread(s, sizeof(Stu), 3, fp);
printf("ret = %d\n", ret);

int i = 0;
for (i = 0; i < 3; i++)
{
	printf("s === %s, %d\n", s[i].name, s[i].id);
}
```


## Windows和Linux文本文件区别

- b是二进制模式的意思，b只是在Windows有效，在Linux用r和rb的结果是一样的
- Unix和Linux下所有的文本文件行都是`\n`结尾，而Windows所有的文本文件行都是`\r\n`结尾
- 在Windows平台下，以“文本”方式打开文件，不加b：
	- 当读取文件的时候，系统会将所有的 "`\r\n`" 转换成 "`\n`"
	- 当写入文件的时候，系统会将 `"\n"` 转换成 `"\r\n"` 写入 
	- <font color=#ff0000>以"二进制"方式打开文件，则读\写都不会进行这样的转换</font>
- 在Unix/Linux平台下，“文本”与“二进制”模式没有区别，"`\r\n`" 作为两个字符原样输入输出

判断文本文件是Linux格式还是Windows格式:
```c
#include<stdio.h>

int main(int argc, char args)
{
	if (argc < 2)
		return 0;
	FILE p = fopen(args[1], "rb");
	if (!p)
		return 0;

	char a[1024] = { 0 };
	fgets(a, sizeof(a), p);

	int len = 0;
	while (a[len])
	{
		if (a[len] == '\n')
		{
			if (a[len - 1] == '\r')
			{
				printf("windows file\n");
			}
			else
			{
				printf("linux file\n");
			}
		}
		len++;
	}

	fclose(p);

	return 0;
}
```

10.6 获取文件状态
```c
#include <sys/types.h>
#include <sys/stat.h>
int stat(const char path, struct stat buf);
功能：获取文件状态信息
参数：
path：文件名
buf：保存文件信息的结构体
返回值：
成功：0
失败-1
```

```c
struct stat {
	dev_t         st_dev;         //文件的设备编号
	ino_t         st_ino;          //节点
	mode_t        st_mode;   //文件的类型和存取的权限
	nlink_t       st_nlink;     //连到该文件的硬连接数目，刚建立的文件值为1
	uid_t         st_uid;         //用户ID
	gid_t         st_gid;         //组ID
	dev_t         st_rdev;      //(设备类型)若此文件为设备文件，则为其设备编号
	off_t         st_size;        //文件字节数(文件大小)
	unsigned long st_blksize;   //块大小(文件系统的I/O 缓冲区大小)
	unsigned long st_blocks;    //块数
	time_t        st_atime;     //最后一次访问时间
	time_t        st_mtime;    //最后一次修改时间
	time_t        st_ctime;     //最后一次改变时间(指属性)
};
```

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <stdio.h>

int main(int argc, char args)
{
	if (argc < 2)
		return 0;

	struct stat st = { 0 };

	stat(args[1], &st);
	int size = st.st_size;//得到结构体中的成员变量
	printf("%d\n", size);
	return 0;
}
```

## 删除文件、重命名文件名
```c
#include <stdio.h>
int remove(const char pathname);
功能：删除文件
参数：
	pathname：文件名
返回值：
	成功：0
	失败：-1

#include <stdio.h>
int rename(const char oldpath, const char newpath);
功能：把oldpath的文件名改为newpath
参数：
oldpath：旧文件名
newpath：新文件名
返回值：
成功：0
失败： - 1
```

## 文件缓冲区
### 文件缓冲区
ANSI C标准采用“缓冲文件系统”处理数据文件。

所谓缓冲文件系统是指系统自动地在内存区为程序中每一个正在使用的文件<font color=#ff0000>开辟一个文件缓冲区</font>从内存向磁盘输出数据必须先送到内存中的<font color=#ff0000>缓冲区</font>，装满缓冲区后才一起送到磁盘去。

如果从磁盘向计算机读入数据，则一次从磁盘文件将一批数据输入到内存缓冲区(充满缓冲区)，然后再从缓冲区逐个地将数据送到程序数据区(给程序变量) 。

### 磁盘文件的存取

<img src="https://article.biliimg.com/bfs/article/c97224011e5139904f768e98b96f190e38716159.png" alt="image.png" style="zoom:50%;" />


- 磁盘文件，一般保存在硬盘、U盘等掉电不丢失的磁盘设备中，在需要时调入内存
- 在内存中对文件进行编辑处理后，保存到磁盘中
- 程序与磁盘之间交互，不是立即完成，系统或程序可根据需要设置缓冲区，以提高存取效率

缓冲区刷新：
标准输出-- stdout -- 标准输出缓冲区。写给屏幕的数据，都是先存缓冲区中，由缓冲区一次性刷新到物理设备(屏幕)
标准输入-- stdin -- 标准输入缓冲区。从键盘读取的数据，直接读到 缓冲区中，由缓冲区给程序提供数据。
预读入、缓输出。
<font color=#ff0000>行缓冲</font>：printf(); 遇到`\n`就会将缓冲区中的数据刷新到物理设备上。
<font color=#ff0000>全缓冲</font>：文件。 缓冲区存满，数据刷新到物理设备上。
<font color=#ff0000>无缓冲</font>：perror。 缓冲区中只要有数据，就立即刷新到物理设备。

默认缓冲类型规则
1. **标准输入和输出**：
    - 标准输出 (`stdout`) 通常是行缓冲的，这意味着当数据包含换行符时，缓冲区会被刷新（如果标准输出关联到终端设备）。
    - 标准输入 (`stdin`) 也通常是行缓冲的，但具体行为可能依赖于平台和运行环境。
    - 标准错误 (`stderr`) 通常是无缓冲的，以确保错误信息能立即输出。
2. **文件I/O**：
    - 打开的文件通常是全缓冲的，这意味着数据只有在缓冲区满或显式调用 `fflush()` 或关闭文件时才会被写入磁盘。
### 更新缓冲区
```c
#include <stdio.h>
int fflush(FILE stream);
功能：更新缓冲区，让缓冲区的数据立马写到文件中。
参数：
stream：文件指针
返回值：
成功：0
失败：-1
```


输入`/`输出函数家族


| 家族名     | 目的    | 可用于所有流     | 只用于stdin和stdout |
| ------- | ----- | ---------- | --------------- |
| getchar | 字符输入  | fgetc、getc | getchar         |
| putchar | 字符输出  | fputc、putc | putchar         |
| gets    | 文本行输入 | fgets      | gets            |
| puts    | 文本行输出 | fputs      | puts            |
| scanf   | 格式化输入 | fscanf     | scanf           |
| printf  | 格式化输出 | fprintf    | printf          |

## Linux系统文件IO

每个系统都有自己的专属函数，我们习惯称其为系统函数。系统函数并不是内核函数，因为内核函数是不允许用户使用的，系统函数就充当了二者之间的桥梁，这样用户就可以间接的完成某些内核操作了。

在前面介绍了文件描述符，在Linux系统中必须要使用系统提供的IO函数才能基于这些文件描述符完成对相关文件的读写操作。这些Linux系统IO函数和标准C库的IO函数使用方法类似，函数名称也类似，下边开始一一介绍。

### open/close

#### 函数原型

> 通过open函数我们即可打开一个磁盘文件，如果磁盘文件不存在还可以创建一个新的的文件，函数原型如下：

```c++
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>

/
open是一个系统函数, 只能在linux系统中使用, windows不支持
fopen 是标准c库函数, 一般都可以跨平台使用, 可以这样理解:
		- 在linux中 fopen底层封装了Linux的系统API open
		- 在window中, fopen底层封装的是 window 的 api
/
// 打开一个已经存在的磁盘文件
int open(const char pathname, int flags);
// 打开磁盘文件, 如果文件不存在, 就会自动创建
int open(const char pathname, int flags, mode_t mode);
```

参数介绍:
- pathname: 被打开的文件的文件名
- flags: 使用什么方式打开指定的文件，这个参数对应一些宏值，需要根据实际需求指定
	- 必须要指定的属性, 以下三个属性不能同时使用, 只能任选其一
	- `O_RDONLY`: 以只读方式打开文件
	- `O_WRONLY`: 以只写方式打开文件
	- `O_RDWR`: 以读写方式打开文件
	- 可选属性, 和上边的属性一起使用
		- `O_APPEND`: 新数据追加到文件尾部, 不会覆盖文件的原来内容
		- `O_CREAT`: 如果文件不存在, 创建该文件, 如果文件存在什么也不做
		- `O_EXCL`: 检测文件是否存在, 必须要和 O_CREAT 一起使用, 不能单独使用: `O_CREAT | O_EXCL`
	- 检测到文件不存在, 创建新文件
	- 检测到文件已经存在, 创建失败, 函数直接返回-1(如果不添加这个属性，不会返回-1)
- mode: 在创建新文件的时候才需要指定这个参数的值，用于指定新文件的权限，这是一个八进制的整数
	- 这个参数的最大值为：0777
	- 创建的新文件对应的最终实际权限, 计算公式: (`mode & ~umask`)
		- umask 掩码可以通过 umask 命令查看
		```c++
		$ umask
		0002
		```
	- 假设 mode 参数的值为 0777, 通过计算得到的文件权限为 0775
		```c=+
		# umask(文件掩码):  002(八进制)  = 000000010 (二进制)  
		# ~umask(掩码取反): ~000000010 (二进制) = 111111101 (二进制)  
		# 参数mode指定的权限为: 0777(八进制) = 111111111(二进制)
		# 计算公式: mode & ~umask
		             111111111
		       &     111111101
		            ------------------
		             111111101    二进制
		            ------------------
		             mod=0775     八进制  
		```

- 返回值:
	- 成功: 返回内核分配的文件描述符, 这个值被记录在内核的文件描述符表中，这是一个大于0的整数
	- 失败: -1

#### close函数原型
> 通过open函数可以让内核给文件分配一个文件描述符, 如果需要释放这个文件描述符就需要关闭文件。对应的这个系统函数叫做 close，函数原型如下：

```c++
#include <unistd.h>
int close(int fd);
```

- 函数参数: fd 是文件描述符, 是open() 函数的返回值
- 函数返回值: 函数调用成功返回值 0, 调用失败返回 -1

#### 打开已存在文件

> 我们可以使用open()函数打开一个本地已经存在的文件, 假设我们想要读写这个文件, 操作代码如下:

```c++
// open.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>

int main()
{
    // 打开文件
    int fd = open("abc.txt", O_RDWR);
    if(fd == -1)
    {
        printf("打开文件失败\n");
    }
    else
    {
        printf("fd: %d\n", fd);
    }

    close(fd);
    return 0;
}
```

编译并执行程序
```c++
$ gcc open.c 
$ ./a.out 
fd: 3		# 打开的文件对应的文件描述符值为 3
```

#### 创建新文件

> 如果要创建一个新的文件，还是使用 open 函数，只不过需要添加 `O_CREAT` 属性, 并且给新文件指定操作权限。

```c++
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>

int main()
{
    // 创建新文件
    int fd = open("./new.txt", O_CREAT|O_RDWR, 0664);
    if(fd == -1)
    {
        printf("打开文件失败\n");
    }
    else
    {
        printf("创建新文件成功, fd: %d\n", fd);
    }

    close(fd);
    return 0;
}
```

```c++
$ gcc open1.c 
$ ./a.out 
创建新文件成功, fd: 3
```

假设在创建新文件的时候, 给 open 指定第三个参数指定新文件的操作权限, 文件也是会被创建出来的, 只不过新的文件的权限可能会有点奇怪, 这个权限会随机分配而且还会出现一些特殊的权限位, 如下:

```c++
$ $ ll new.txt 
-r-x--s--T 1 robin robin 0 Jan 30 16:17 new.txt   # T 就是一个特殊权限
```

#### 文件状态判断
在创建新文件的时候我们还可以通过 `O_EXCL`进行文件的检测, 具体处理方式如下:

```c++
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>

int main()
{
    // 创建新文件之前, 先检测是否存在
    // 文件存在创建失败, 返回-1, 文件不存在创建成功, 返回分配的文件描述符
    int fd = open("./new.txt", O_CREAT|O_EXCL|O_RDWR);
    if(fd == -1)
    {
        printf("创建文件失败, 已经存在了, fd: %d\n", fd);
    }
    else
    {
        printf("创建新文件成功, fd: %d\n", fd);
    }

    close(fd);
    return 0;
}
```

编译并执行程序:
```c++
$ gcc open1.c 
$ ./a.out 
创建文件失败, 已经存在了, fd: -1
```

### read/write
#### read
> read 函数用于读取文件内部数据，在通过 open 打开文件的时候需要指定读权限，函数原型如下：

```c++
#include <unistd.h>
ssize_t read(int fd, void buf, size_t count);
```

- 参数:
	- fd: 文件描述符, `open()` 函数的返回值, 通过这个参数定位打开的磁盘文件
	- buf: 是一个传出参数, 指向一块有效的内存, 用于存储从文件中读出的数据
		- 传出参数: 类似于返回值, 将变量地址传递给函数, 函数调用完毕, 地址中就有数据了
	- count: buf指针指向的内存的大小, 指定可以存储的最大字节数
- 返回值:
	- 大于0: 从文件中读出的字节数，读文件成功
	- 等于0: 代表文件读完了，读文件成功
	- -1: 读文件失败了

#### write
> write 函数用于将数据写入到文件内部，在通过 open 打开文件的时候需要指定写权限，函数原型如下：

```c++
#include <unistd.h>
ssize_t write(int fd, const void buf, size_t count);
```

- 参数:
	- fd: 文件描述符, `open()` 函数的返回值, 通过这个参数定位打开的磁盘文件
	- buf: 指向一块有效的内存地址, 里边有要写入到磁盘文件中的数据
		- count: 要往磁盘文件中写入的字节数, 一般情况下就是buf字符串的长度, `strlen(buf)`
- 返回值:
	- 大于0: 成功写入到磁盘文件中的字节数
	- -1: 写文件失败了

### 文件拷贝

> 假设有一个比较大的磁盘文件, 打开这个文件得到文件描述符fd1，然后在创建一个新的磁盘文件得到文件描述符fd2, 在程序中通过 fd1 将文件内容读出，并通过fd2将读出的数据写入到新文件中。

```c++
// 文件的拷贝
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>

int main()
{
    // 1. 打开存在的文件english.txt, 读这个文件
    int fd1 = open("./english.txt", O_RDONLY);
    if(fd1 == -1)
    {
        perror("open-readfile");
        return -1;
    }

    // 2. 打开不存在的文件, 将其创建出来, 将从english.txt读出的内容写入这个文件中
    int fd2 = open("copy.txt", O_WRONLY|O_CREAT, 0664);
    if(fd2 == -1)
    {
        perror("open-writefile");
        return -1;
    }

    // 3. 循环读文件, 循环写文件
    char buf[4096];
    int len = -1;
    while( (len = read(fd1, buf, sizeof(buf))) > 0 )
    {
        // 将读到的数据写入到另一个文件中
        write(fd2, buf, len); 
    }
    // 4. 关闭文件
    close(fd1);
    close(fd2);

    return 0;
}
```

### lseek
系统函数 `lseek` 的功能是比较强大的, 我们既可以通过这个函数移动文件指针, 也可以通过这个函数进行文件的拓展。这个函数的原型如下:

```c
#include <sys/types.h>
#include <unistd.h>

off_t lseek(int fd, off_t offset, int whence);
```

- 参数:
	- fd: 文件描述符, open() 函数的返回值, 通过这个参数定位打开的磁盘文件
	- offset: 偏移量，需要和第三个参数配合使用
	- whence: 通过这个参数指定函数实现什么样的功能
		- `SEEK_SET`: 从文件头部开始偏移 offset 个字节
		- `SEEK_CUR`: 从当前文件指针的位置向后偏移offset个字节
		- `SEEK_END`: 从文件尾部向后偏移offset个字节
- 返回值:
	- 成功: 文件指针从头部开始计算总的偏移量
	- 失败: -1

#### 移动文件指针

> 通过对 lseek 函数第三个参数的设置, 经常使用该函数实现如下几个功能， 如下所示：

- 文件指针移动到文件头部
```c++
lseek(fd, 0, SEEK_SET);
```
- 得到当前文件指针的位置
```c++
lseek(fd, 0, SEEK_CUR); 
```
- 得到文件总大小
```c++
lseek(fd, 0, SEEK_END);
```

#### 文件拓展

假设使用一个下载软件进行一个大文件下载，但是磁盘很紧张，如果不能马上将文件下载到本地，磁盘空间就可能被其他文件占用了，导致下载软件下载的文件无处存放。那么这个文件怎么解决呢？

我们可以在开始下载的时候先进行文件拓展，将一些字符写入到目标文件中，让拓展的文件和即将被下载的文件一样大，这样磁盘空间就被成功抢到手，软件就可以慢悠悠的下载对应的文件了。

> 使用 `lseek` 函数进行文件拓展必须要满足一下条件：
> 1. 文件指针必须要偏移到文件尾部之后， 多出来的就需要被填充的部分。
> 2. 文件拓展之后，必须要使用 `write()`函数进行一次写操作(写什么都可以，没有字节数要求)。

文件拓展举例：

```c++
// lseek.c
// 拓展文件大小
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>

int main()
{
    int fd = open("hello.txt", O_RDWR);
    if(fd == -1)
    {
        perror("open");
        return -1;
    }

    // 文件拓展, 一共增加了 1001 个字节
    lseek(fd, 1000, SEEK_END);
    write(fd, " ", 1);
        
    close(fd);
    return 0;
}

```

### truncate/ftruncate
`truncate/ftruncate `这两个函数的功能是一样的，可以对文件进行拓展也可以截断文件。使用这两个函数拓展文件比使用lseek要简单。这两个函数的函数原型如下：
```c++
// 拓展文件或截断文件
#include <unistd.h>
#include <sys/types.h>

int truncate(const char path, off_t length);
	- 
int ftruncate(int fd, off_t length);
```

- 参数：
	- path: 要拓展/截断的文件的文件名
	- fd: 文件描述符, open() 得到的
	- length: 文件的最终大小
		- 文件原来size > length，文件被截断, 尾部多余的部分被删除, 文件最终长度为length
		- 文件原来size < length，文件被拓展, 文件最终长度为length
- 返回值: 成功返回0; 失败返回值-1

> `truncate()` 和 `ftruncate()` 两个函数的区别在于一个使用文件名一个使用文件描述符操作文件, 功能相同。
> 不管是使用这两个函数还是使用 `lseek()` 函数拓展文件，文件尾部填充的字符都是 0。

### perror

在查看Linux系统函数的时候, 我们可以发现一个规律: 大部分系统函数的返回值都是整形，并且通过这个返回值来描述系统函数的状态(调用是否成功了)。在man 文档中关于系统函数的返回值大部分时候都是这样描述的：

```c
RETURN VALUE
       On  success,  zero is returned.  On error, -1 is returned, and errno is set
       appropriately.
       
       如果成功，则返回0。出现错误时，返回-1，并给errno设置一个适当的值。
```

> `errno`是一个全局变量，只要调用的Linux系统函数有异常(返回-1), 错误对应的错误号就会被设置给这个全局变量。这个错误号存储在系统的两个头文件中：
> 
> 1. /usr/include/asm-generic/errno-base.h
> 2. /usr/include/asm-generic/errno.h

得到错误号，去查询对应的头文件是非常不方便的，我们可以通过 perror 函数将错误号对应的描述信息打印出来

```c++
#include <stdio.h>
// 参数, 自己指定这个字符串的值就可以, 指定什么就会原样输出, 除此之外还会输出错误号对应的描述信息
void perror(const char s);	
```

举例: 使用 perrno 打印错误信息

```c
// open.c
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>

int main()
{
    int fd = open("hello.txt", O_RDWR|O_EXCL|O_CREAT, 0777);
    if(fd == -1)
    {
        perror("open");
        return -1;
    }
        
    close(fd);
    return 0;
}
```

编译并执行程序
```c
$ gcc open.c
$ $ ./a.out 
open: File exists	# 通过 perror 输出的错误信息
```

### 错误号
为了方便查询, 特将全局变量 errno 和错误信息描述的对照关系贴出:

#### Part1
信息来自头文件: `/usr/include/asm-generic/errno-base.h`
```c
#define EPERM            1      / Operation not permitted /
#define ENOENT           2      / No such file or directory /
#define ESRCH            3      / No such process /
#define EINTR            4      / Interrupted system call /
#define EIO              5      / I/O error /
#define ENXIO            6      / No such device or address /
#define E2BIG            7      / Argument list too long /
#define ENOEXEC          8      / Exec format error /
#define EBADF            9      / Bad file number /
#define ECHILD          10      / No child processes /
#define EAGAIN          11      / Try again /
#define ENOMEM          12      / Out of memory /
#define EACCES          13      / Permission denied /
#define EFAULT          14      / Bad address /
#define ENOTBLK         15      / Block device required /
#define EBUSY           16      / Device or resource busy /
#define EEXIST          17      / File exists /
#define EXDEV           18      / Cross-device link /
#define ENODEV          19      / No such device /
#define ENOTDIR         20      / Not a directory /
#define EISDIR          21      / Is a directory /
#define EINVAL          22      / Invalid argument /
#define ENFILE          23      / File table overflow /
#define EMFILE          24      / Too many open files /
#define ENOTTY          25      / Not a typewriter /
#define ETXTBSY         26      / Text file busy /
#define EFBIG           27      / File too large /
#define ENOSPC          28      / No space left on device /
#define ESPIPE          29      / Illegal seek /
#define EROFS           30      / Read-only file system /
#define EMLINK          31      / Too many links /
#define EPIPE           32      / Broken pipe /
#define EDOM            33      / Math argument out of domain of func /
#define ERANGE          34      / Math result not representable /
```

#### Part2

信息来自头文件: `/usr/include/asm-generic/errno.h`

```c
#define EDEADLK         35      / Resource deadlock would occur /
#define ENAMETOOLONG    36      / File name too long /
#define ENOLCK          37      / No record locks available /

/
  This error code is special: arch syscall entry code will return
  -ENOSYS if users try to call a syscall that doesn't exist.  To keep
  failures of syscalls that really do exist distinguishable from
  failures due to attempts to use a nonexistent syscall, syscall
  implementations should refrain from returning -ENOSYS.
 /
#define ENOSYS          38      / Invalid system call number /

#define ENOTEMPTY       39      / Directory not empty /
#define ELOOP           40      / Too many symbolic links encountered /
#define EWOULDBLOCK     EAGAIN  / Operation would block /
#define ENOMSG          42      / No message of desired type /
#define EIDRM           43      / Identifier removed /
#define ECHRNG          44      / Channel number out of range /
#define EL2NSYNC        45      / Level 2 not synchronized /
#define EL3HLT          46      / Level 3 halted /
#define EL3RST          47      / Level 3 reset /
#define ELNRNG          48      / Link number out of range /
#define EUNATCH         49      / Protocol driver not attached /
#define ENOCSI          50      / No CSI structure available /
#define EL2HLT          51      / Level 2 halted /
#define EBADE           52      / Invalid exchange /
#define EBADR           53      / Invalid request descriptor /
#define EXFULL          54      / Exchange full /
#define ENOANO          55      / No anode /
#define EBADRQC         56      / Invalid request code /
#define EBADSLT         57      / Invalid slot /

#define EDEADLOCK       EDEADLK

#define EBFONT          59      / Bad font file format /
#define ENOSTR          60      / Device not a stream /
#define ENODATA         61      / No data available /
#define ETIME           62      / Timer expired /
#define ENOSR           63      / Out of streams resources /
#define ENONET          64      / Machine is not on the network /
#define ENOPKG          65      / Package not installed /
#define EREMOTE         66      / Object is remote /
#define ENOLINK         67      / Link has been severed /
#define EADV            68      / Advertise error /
#define ESRMNT          69      / Srmount error /
#define ECOMM           70      / Communication error on send /
#define EPROTO          71      / Protocol error /
#define EMULTIHOP       72      / Multihop attempted /
#define EDOTDOT         73      / RFS specific error /
#define EBADMSG         74      / Not a data message /
#define EOVERFLOW       75      / Value too large for defined data type /
#define ENOTUNIQ        76      / Name not unique on network /
#define EBADFD          77      / File descriptor in bad state /
#define EREMCHG         78      / Remote address changed /
#define ELIBACC         79      / Can not access a needed shared library /
#define ELIBBAD         80      / Accessing a corrupted shared library /
#define ELIBSCN         81      / .lib section in a.out corrupted /
#define ELIBMAX         82      / Attempting to link in too many shared libraries /
#define ELIBEXEC        83      / Cannot exec a shared library directly /
#define EILSEQ          84      / Illegal byte sequence /
#define ERESTART        85      / Interrupted system call should be restarted /
#define ESTRPIPE        86      / Streams pipe error /
#define EUSERS          87      / Too many users /
#define ENOTSOCK        88      / Socket operation on non-socket /
#define EDESTADDRREQ    89      / Destination address required /
#define EMSGSIZE        90      / Message too long /
#define EPROTOTYPE      91      / Protocol wrong type for socket /
#define ENOPROTOOPT     92      / Protocol not available /
#define EPROTONOSUPPORT 93      / Protocol not supported /
#define ESOCKTNOSUPPORT 94      / Socket type not supported /
#define EOPNOTSUPP      95      / Operation not supported on transport endpoint /
#define EPFNOSUPPORT    96      / Protocol family not supported /
#define EAFNOSUPPORT    97      / Address family not supported by protocol /
#define EADDRINUSE      98      / Address already in use /
#define EADDRNOTAVAIL   99      / Cannot assign requested address /
#define ENETDOWN        100     / Network is down /
#define ENETUNREACH     101     / Network is unreachable /
#define ENETRESET       102     / Network dropped connection because of reset /
#define ECONNABORTED    103     / Software caused connection abort /
#define ECONNRESET      104     / Connection reset by peer /
#define ENOBUFS         105     / No buffer space available /
#define EISCONN         106     / Transport endpoint is already connected /
#define ENOTCONN        107     / Transport endpoint is not connected /
#define ESHUTDOWN       108     / Cannot send after transport endpoint shutdown /
#define ETOOMANYREFS    109     / Too many references: cannot splice /
#define ETIMEDOUT       110     / Connection timed out /
#define ECONNREFUSED    111     / Connection refused /
#define EHOSTDOWN       112     / Host is down /
#define EHOSTUNREACH    113     / No route to host /
#define EALREADY        114     / Operation already in progress /
#define EINPROGRESS     115     / Operation now in progress /
#define ESTALE          116     / Stale file handle /
#define EUCLEAN         117     / Structure needs cleaning /
#define ENOTNAM         118     / Not a XENIX named type file /
#define ENAVAIL         119     / No XENIX semaphores available /
#define EISNAM          120     / Is a named type file /
#define EREMOTEIO       121     / Remote I/O error /
#define EDQUOT          122     / Quota exceeded /

#define ENOMEDIUM       123     / No medium found /
#define EMEDIUMTYPE     124     / Wrong medium type /
#define ECANCELED       125     / Operation Canceled /
#define ENOKEY          126     / Required key not available /
#define EKEYEXPIRED     127     / Key has expired /
#define EKEYREVOKED     128     / Key has been revoked /
#define EKEYREJECTED    129     / Key was rejected by service /

/ for robust mutexes /
#define EOWNERDEAD      130     / Owner died /
#define ENOTRECOVERABLE 131     / State not recoverable /

#define ERFKILL         132     / Operation not possible due to RF-kill /

#define EHWPOISON       133     / Memory page has hardware error /
```


## 文件的属性信息

> 众所周知，Linux是一个基于文件的操作系统，因此作为文件本身也就有很多属性，如果想要查看某一个文件的属性有两种方式：命令和函数。虽然有两种方式但是它们对应的名字是相同的，叫做stat。另外使用file命令也可以查看文件的一些属性信息。

### file 命令

> 该命令用来识别文件类型，也可用来辨别一些文件的编码格式。它是通过查看文件的头部信息来获取文件类型，而不是像Windows通过扩展名来确定文件类型的。

命令语法如下：

```shell
# 参数在命令中的位置没有限制
$ file 文件名 [参数] 
```

file 命令的参数是可选项, 可以不加, 常用的参数如下表:

| 参数  |          描述          |
| :-: | :------------------: |
| -b  | 只显示文件类型和文件编码, 不显示文件名 |
| -i  |    显示文件的 MIME 类型     |
| -F  |     设置输出字符串的分隔符      |
| -L  |    查看软连接文件自身文件属性     |

#### 查看文件类型和编码格式
使用不带任何选项的 `file` 命令，即可查看指定文件的类型和文件编码信息。

```sh
# 空文件
$ file 11.txt 
11.txt: empty

# 源文件, 编码格式为: ASCII
$ file b.cpp
b.cpp: C source, ASCII text

# 源文件, 编码格式为: UTF-8 
robin@OS:~$ file test.cpp 
test.cpp: C source, UTF-8 Unicode (with BOM) text, with CRLF line terminators

# 可执行程序, Linux中的可执行程序为 ELF 格式
robin@OS:~$ file a.out 
a.out: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/l, for GNU/Linux 2.6.32, BuildID[sha1]=5317ae9fba592bf583c4f680d8cc48a8b58c96a5, not stripped
```

#### 只显示文件格式以及编码

使用`-b`参数，可以使 file 命令的输出不出现文件名，只显示文件格式以及编码。

```shell
# 空文件
$ file 11.txt -b
empty

# 源文件, 编码格式为: ASCII
$ file b.cpp -b
C source, ASCII text

# 源文件, 编码格式为: UTF-8 
robin@OS:~$ file test.cpp  -b
C source, UTF-8 Unicode (with BOM) text, with CRLF line terminators

# 可执行程序, Linux中的可执行程序为 ELF 格式
robin@OS:~$ file a.out  -b
ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/l, for GNU/Linux 2.6.32, BuildID[sha1]=5317ae9fba592bf583c4f680d8cc48a8b58c96a5, not stripped
```

#### 显示文件的 MIME 类型
给file命令添加`-i`参数，可以输出文件对应的`MIME`类型的字符串。

> MIME(Multipurpose Internet Mail Extensions)多用途互联网邮件扩展类型。是设定某种扩展名的文件用一种应用程序来打开的方式类型，当该扩展名文件被访问的时候，浏览器会自动使用指定应用程序来打开。多用于指定一些客户端自定义的文件名，以及一些媒体文件打开方式。

```shell
# charset 为该文件的字符编码

# 源文件, MIME类型: text/x-c, 字符编码: utf-8
$ file occi.cpp -i
occi.cpp: text/x-c; charset=utf-8

# 压缩文件, MIME类型: application/gzip, 字符编码: binary
$ file fcgi.tar.gz -i
fcgi.tar.gz: application/gzip; charset=binary

# 文本文件, MIME类型: text/plain, 字符编码: utf-8
$ file english.txt -i
english.txt: text/plain; charset=utf-8

# html文件, MIME类型: text/html, 字符编码: us-ascii
$ file demo.html -i
demo.html: text/html; charset=us-ascii
```

#### 设置输出分隔符

> 在 file 命令中，文件名和后边的属性信息默认使用冒号(:)分隔，我们可以通过 -F参数修改分隔符，分隔符可以是单字符也可以是一个字符串，如果分隔符是字符串需要将这个参数值写到引号中(单/双引号都可以)。

```shell
# 默认格式输出
$ file english.txt 
english.txt: UTF-8 Unicode text, with very long lines, with CRLF line terminators

# 修改分隔符为字符串 “==>"
$ file english.txt -F "==>"
english.txt==> UTF-8 Unicode text, with very long lines, with CRLF line terminators

$ file english.txt -F '==>'
english.txt==> UTF-8 Unicode text, with very long lines, with CRLF line terminators

# 修改分隔符为单字符 '='
$ file english.txt -F = 
english.txt= UTF-8 Unicode text, with very long lines, with CRLF line terminators
```

#### 查看软连接文件

> 软连接文件是一个特殊格式的文件, 查看这种格式的文件可以得到两种结果: 第一种是软连接文件本身的属性信息, 另一种是链接文件指向的那个文件的属性信息。
> 直接通过 file 查看文件属性得到的是链接文件指向的文件的信息，如果添加参数 `-L`得到的链接文件自身的属性信息

```shell
# 使用 ls 查看链接文件属性信息
$ ll link.lnk 
lrwxrwxrwx 1 root root 24 Jan 25 17:27 link.lnk -> /root/luffy/onepiece.txt

# 使用file直接查看链接文件信息: 得到的是链接文件指向的那个文件的名字
$ file link.lnk 
link.lnk: symbolic link to /root/luffy/onepiece.txt'

# 使用 file 查看链接文件自身属性信息, 添加参数 -L
$ file link.lnk -L
link.lnk: UTF-8 Unicode text
```

### stat 命令

stat命令显示文件或目录的详细属性信息包括文件系统状态，比ls命令输出的信息更详细。语法格式如下:
```shell
# 参数在命令中的位置没有限制
$ stat [参数] 文件或者目录名
```

关于这个命令的可选参数如下表:

| 参数  |            功能            |
| :-: | :----------------------: |
| -f  | 不显示文件本身的信息，显示文件所在文件系统的信息 |
| -L  |    查看软链接文件关联的文件的属性信息     |
| -c  |      查看文件某个单个的属性信息       |
| -t  |  简洁模式，只显示摘要信息, 不显示属性描述   |

#### 显示所有属性

```shell
$ stat english.txt 
  File: 'english.txt'
  Size: 129567          Blocks: 256        IO Block: 4096   regular file
Device: 801h/2049d      Inode: 526273      Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1001/   robin)   Gid: ( 1001/   robin)
Access: 2021-01-31 00:00:36.791490304 +0800
Modify: 2021-01-31 00:00:36.791490304 +0800
Change: 2021-01-31 00:00:36.791490304 +0800
 Birth: -
```

在输出的信息中我们可以看到很多属性,

- `File`: 文件名
- `Size`: 文件大小, 单位是字节
- `Blocks`: 文件使用的数据块总数
- `IO Block`: IO块大小
- `regular file`: 文件的实际类型，文件类型不同，该关键字也会变化
- `Device`: 设备编号
- `Inode`: Inode号，操作系统用inode编号来识别不同的文件，找到文件数据所在的block，读出数据。
- `Links`: 硬链接计数
- `Access`: 文件所有者+所属组用户+其他人对文件的访问权限
- `Uid`: 文件所有者名字和所有者ID
- `Gid`: 文件所有数组名字已经组ID
- `Access Time`: 表示文件的访问时间。当文件内容被访问时，这个时间被更新
- `Modify Time`: 表示文件内容的修改时间，当文件的数据内容被修改时，这个时间被更新
- `Change Time`: 表示文件的状态时间，当文件的状态被修改时，这个时间被更新，例如：文件的硬链接链接数，大小，权限，Blocks数等。
- `Birth`: 文件生成的日期

#### 只显示系统信息
给 stat 命令添加 `-f`参数将只显示文件在文件系统中的相关属性信息, 文件自身属性不显示

```shell
$ stat luffy/ -f
  File: "luffy/"
    ID: 47d795d8889d00d3 Namelen: 255     Type: ext2/ext3
Block size: 4096       Fundamental block size: 4096
Blocks: Total: 10288179   Free: 8991208    Available: 8546752
Inodes: Total: 2621440    Free: 2515927
```

#### 软连接文件
使用 stat 查看软链接类型的文件, 默认显示的是这个软链接文件的属性信息, 添加参数 `-L`就可以查看这个软连接文件关联的文件的属性信息了。

```shell
# 查看软件文件属性 -> 使用 ls -l
ls -l link.lnk 
lrwxrwxrwx 1 root root 24 Jan 25 17:27 link.lnk -> /root/luffy/onepiece.txt

# 使用 stat 查看软连接文件属性信息
$ stat link.lnk 
  File: ‘link.lnk’ -> ‘/root/luffy/onepiece.txt’
  Size: 24              Blocks: 0          IO Block: 4096   symbolic link
Device: fd01h/64769d    Inode: 393832      Links: 1
Access: (0777/lrwxrwxrwx)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2021-01-30 23:46:29.922760178 +0800
Modify: 2021-01-25 17:27:12.057386837 +0800
Change: 2021-01-25 17:27:12.057386837 +0800
 Birth: -

# 使用 stat 查看软连接文件关联的文件的属性信息
$ stat link.lnk -L
  File: ‘link.lnk’
  Size: 3700              Blocks: 8          IO Block: 4096   regular file
Device: fd01h/64769d    Inode: 660353      Links: 2
Access: (0444/-r--r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2021-01-30 23:46:53.696723182 +0800
Modify: 2021-01-25 17:54:47.000000000 +0800
Change: 2021-01-26 11:57:00.587658977 +0800
 Birth: -
```

#### 简洁输出
使用 stat 进行简洁信息输出的可读性不是太好, 所有的属性描述都别忽略了, 如果只想得到属性值, 可以给该命令添加`-t`参数:

```shell
$ stat luffy/ -t
luffy/ 4096 8 41fd 1001 1001 801 662325 8 0 0 1611659086 1580893020 1580893020 0 4096
```

#### 单个属性输出
如果每次只想通过 `stat` 命令得到某一个文件属性, 可以给名添加 -c参数。 不同的文件属性分别对应一些定义好的特殊符号，想得到哪个属性值，将其指定到参数 -c后边即可。属性对应的字符如下表：

| 格式化字符 |                      功能                       |
| :---: | :-------------------------------------------: |
| `%a`  |             文件的八进制访问权限(#和0是输出标准)              |
| `%A`  |              人类可读形式的文件访问权限(rwx)               |
| `%b`  |                    已分配的块数量                    |
| `%B`  |               报告的每个块的大小(以字节为单位)               |
| `%C`  |               SELinux 安全上下文字符串                |
| `%d`  |                  设备编号 (十进制)                   |
| `%D`  |                  设备编号 (十六进制)                  |
| `%F`  |                     文件类型                      |
| `%g`  |                   文件所属组组ID                    |
| `%G`  |                    文件所属组名字                    |
| `%h`  |                     用连接计数                     |
| `%i`  |                    inode编号                    |
| `%m`  |                      挂载点                      |
| `%n`  |                      文件名                      |
| `%N`  |         用引号括起来的文件名，并且会显示软连接文件引用的文件路径          |
| `%o`  |                  最佳I/O传输大小提示                  |
| `%s`  |                 文件总大小, 单位为字节                  |
| `%t`  |           十六进制的主要设备类型，用于字符/块设备特殊文件            |
| `%T`  |           十六进制的次要设备类型，用于字符/块设备特殊文件            |
| `%u`  |                    文件所有者ID                    |
| `%U`  |                    文件所有者名字                    |
| `%w`  |       文件生成的日期 ，人类可识别的时间字符串 – 获取不到信息不显示        |
| `%W`  |     文件生成的日期 ，自纪元以来的秒数 (参考 %X )– 获取不到信息不显示     |
| `%x`  |            最后访问文件的时间, 人类可识别的时间字符串             |
| `%X`  | 最后访问文件的时间, 自纪元以来的秒数(从1970.1.1开始到最后一次文件访问的总秒数) |
| `%y`  |           最后修改文件内容的时间, 人类可识别的时间字符串            |
| `%Y`  |         最后修改文件内容的时间, 自纪元以来的秒数(参考 %X )         |
| `%z`  |           最后修改文件状态的时间, 人类可识别的时间字符串            |
| `%Z`  |         最后修改文件状态的时间, 自纪元以来的秒数(参考 %X )         |

仔细阅读上表可以知道：文件的每一个属性都有一个或者多个与之对应的格式化字符，这样就可以精确定位所需要的属性信息了，下面举了几个例子，可以作为参考：

```shell
$ stat occi.cpp -c %a
644

$ stat occi.cpp -c %A           
-rw-r--r--

# 使用 ls -l 验证权限
$ ll occi.cpp 
-rw-r--r-- 1 robin robin 1406 Jan 31 00:00 occi.cpp		# 0664

$ stat link.lnk -c %N
'link.lnk' -> '/home/robin/english.txt'

$ stat link.lnk -c %y
2021-01-31 10:48:52.317846411 +0800
```

### stat/lstat 函数
stat/lstat 函数的功能和 stat 命令的功能是一样的, 只不过是应用场景不同。这两个函数的区别在于处理软链接文件的方式上：

- `lstat()`: 得到的是软连接文件本身的属性信息
- `stat()`: 得到的是软链接文件关联的文件的属性信息
函数原型如下：
```c++
#include <sys/types.h>
#include <sys/stat.h>
#include <unistd.h>

int stat(const char pathname, struct stat buf);
int lstat(const char pathname, struct stat buf);
```

- 参数:
	- pathname: 文件名, 要获取这个文件的属性信息
	- buf: 传出参数, 文件的信息被写入到了这块内存中
- 返回值: 函数调用成功返回 0，调用失败返回 -1

> 这个函数的第二个参数是一个结构体类型, 这个结构体相对复杂, 通过这个结构体可以存储得到的文件的所有属性信息, 结构体原型如下:

```c
struct stat {
    dev_t          st_dev;        	// 文件的设备编号
    ino_t           st_ino;        	// inode节点
    mode_t      st_mode;      		// 文件的类型和存取的权限, 16位整形数  -> 常用
    nlink_t        st_nlink;     	// 连到该文件的硬连接数目，刚建立的文件值为1
    uid_t           st_uid;       	// 用户ID
    gid_t           st_gid;       	// 组ID
    dev_t          st_rdev;      	// (设备类型)若此文件为设备文件，则为其设备编号
    off_t            st_size;      	// 文件字节数(文件大小)   --> 常用
    blksize_t     st_blksize;   	// 块大小(文件系统的I/O 缓冲区大小)
    blkcnt_t      st_blocks;    	// block的块数
    time_t         st_atime;     	// 最后一次访问时间
    time_t         st_mtime;     	// 最后一次修改时间(文件内容)
    time_t         st_ctime;     	// 最后一次改变时间(指属性)
};
```

#### 获取文件大小
下面调用 stat() 函数, 以代码的方式演示一下如何得到某个文件的大小:
```c
#include <sys/stat.h>

int main()
{
    // 1. 定义结构体, 存储文件信息
    struct stat myst;
    // 2. 获取文件属性 english.txt
    int ret = stat("./english.txt", &myst);
    if(ret == -1)
    {
        perror("stat");
        return -1;
    }
    printf("文件大小: %d\n", (int)myst.st_size);
    return 0;
}
```


#### 获取文件类型
文件的类型信息存储在 `struct stat` 结构体的`st_mode`成员中, 它是一个 `mode_t`类型, 本质上是一个16位的整数。Linux API中为我们提供了相关的宏函数，通过对应的宏函数可以直接判断出文件是不是某种类型，这些信息都可以通过 man 文档(man 2 stat)查询到。

相关的宏函数原型如下：
```c
// 类型是存储在结构体的这个成员中: mode_t  st_mode;  
// 这些宏函数中的m 对应的就是结构体成员  st_mode
// 宏函数返回值: 是对应的类型返回-> 1, 不是对应类型返回0

S_ISREG(m)  is it a regular file?  
	- 普通文件
S_ISDIR(m)  directory?
	- 目录
S_ISCHR(m)  character device?
	- 字符设备
S_ISBLK(m)  block device?
	- 块设备
S_ISFIFO(m) FIFO (named pipe)?
	- 管道
S_ISLNK(m)  symbolic link?  (Not in POSIX.1-1996.)
	- 软连接
S_ISSOCK(m) socket?  (Not in POSIX.1-1996.)
    - 本地套接字文件
```

在程序中通过宏函数判断文件类型, 实例代码如下:

```c++
int main()
{
    // 1. 定义结构体, 存储文件信息
    struct stat myst;
    // 2. 获取文件属性 english.txt
    int ret = stat("./hello", &myst);
    if(ret == -1)
    {
        perror("stat");
        return -1;
    }

    printf("文件大小: %d\n", (int)myst.st_size);

    // 判断文件类型
    if(S_ISREG(myst.st_mode))
    {
        printf("这个文件是一个普通文件...\n");
    }

    if(S_ISDIR(myst.st_mode))
    {
        printf("这个文件是一个目录...\n");
    }
    if(S_ISLNK(myst.st_mode))
    {
        printf("这个文件是一个软连接文件...\n");
    }

    return 0;
}
```

#### 获取文件权限
用户对文件的操作权限也存储在 `struct stat` 结构体的`st_mode`成员中, 在这个16位的整数中不同用户的权限存储位置如下图，如果想知道有没有相关权限可以通过按位与(&)操作将这个标志位值取出判断即可。

<img src="https://www.subingwen.cn/linux/stat/image-20210131012745017.png" alt="image.png" style="zoom:50%;" />
Linux 中为我们提供了用于不同用户不同权限判定使用的宏，具体信息如下：

```shell
关于变量 st_mode: 
- st_mode -- 16位整数
	○ 0-2 bit -- 其他人权限
		- S_IROTH    00004  读权限   100
		- S_IWOTH    00002  写权限   010
		- S_IXOTH    00001  执行权限  001
		- S_IRWXO    00007  掩码, 过滤 st_mode中除其他人权限以外的信息
	○ 3-5 bit -- 所属组权限
		- S_IRGRP    00040  读权限
		- S_IWGRP    00020  写权限
		- S_IXGRP    00010  执行权限
		- S_IRWXG    00070  掩码, 过滤 st_mode中除所属组权限以外的信息
	○ 6-8 bit -- 文件所有者权限
		- S_IRUSR    00400    读权限
		- S_IWUSR    00200    写权限
		- S_IXUSR    00100    执行权限
		- S_IRWXU    00700    掩码, 过滤 st_mode中除文件所有者权限以外的信息
	○ 12-15 bit -- 文件类型
		- S_IFSOCK   0140000 套接字
		- S_IFLNK    0120000 符号链接(软链接)
		- S_IFREG    0100000 普通文件
		- S_IFBLK    0060000 块设备
		- S_IFDIR    0040000 目录
		- S_IFCHR    0020000 字符设备
		- S_IFIFO    0010000 管道
		- S_IFMT     0170000 掩码,过滤 st_mode中除文件类型以外的信息
			
############### 按位与操作举例 ###############			
    1111 1111 1111 1011   # st_mode
    0000 0000 0000 0100   # S_IROTH
&
----------------------------------------
    0000 0000 0000 0000   # 没有任何权限
    
```

通过仔细阅读上边提供的宏信息, 我们可以知道处理使用它们得到用户对文件的操作权限, 还可以用于判断文件的类型(判断文件类型的第二种方式)，具体操作方式可以参考如下代码：

```c++
#include <sys/stat.h>

int main()
{
    // 1. 定义结构体, 存储文件信息
    struct stat myst;
    // 2. 获取文件属性 english.txt
    int ret = stat("./hello", &myst);
    if(ret == -1)
    {
        perror("stat");
        return -1;
    }

    printf("文件大小: %d\n", (int)myst.st_size);

    // 判断文件类型
    if(S_ISREG(myst.st_mode))
    {
        printf("这个文件是一个普通文件...\n");
    }

    if(S_ISDIR(myst.st_mode))
    {
        printf("这个文件是一个目录...\n");
    }
    if(S_ISLNK(myst.st_mode))
    {
        printf("这个文件是一个软连接文件...\n");
    }

    // 文件所有者对文件的操作权限
    printf("文件所有者对文件的操作权限: ");
    if(myst.st_mode & S_IRUSR)
    {
        printf("r");
    }
    if(myst.st_mode & S_IWUSR)
    {
        printf("w");
    }
    if(myst.st_mode & S_IXUSR)
    {
        printf("x");
    }
    printf("\n");
    return 0;
}
```

### 练习
掌握了如何通过 stat / lstat 函数获取文件相关属性之后, 我们就可以使用这两个函数来模拟执行命令 ls -l 的效果，具体代码实现如下：

```c
#include <stdio.h>
#include <string.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <stdlib.h>
#include <time.h>
#include <pwd.h>
#include <grp.h>


int main(int argc, char argv[])
{
    if(argc < 2)
    {
        printf("./a.out filename\n");
        exit(1);
    }

    struct stat st;
    int ret = stat(argv[1], &st);
    if(ret == -1)
    {
        perror("stat");
        exit(1);
    }

    // 存储文件类型和访问权限
    char perms[11] = {0};
    // 判断文件类型
    switch(st.st_mode & S_IFMT)
    {
        case S_IFLNK:
            perms[0] = 'l';
            break;
        case S_IFDIR:
            perms[0] = 'd';
            break;
        case S_IFREG:
            perms[0] = '-';
            break;
        case S_IFBLK:
            perms[0] = 'b';
            break;
        case S_IFCHR:
            perms[0] = 'c';
            break;
        case S_IFSOCK:
            perms[0] = 's';
            break;
        case S_IFIFO:
            perms[0] = 'p';
            break;
        default:
            perms[0] = '?';
            break;
    }
    // 判断文件的访问权限
    // 文件所有者
    perms[1] = (st.st_mode & S_IRUSR) ? 'r' : '-';
    perms[2] = (st.st_mode & S_IWUSR) ? 'w' : '-';
    perms[3] = (st.st_mode & S_IXUSR) ? 'x' : '-';
    // 文件所属组
    perms[4] = (st.st_mode & S_IRGRP) ? 'r' : '-';
    perms[5] = (st.st_mode & S_IWGRP) ? 'w' : '-';
    perms[6] = (st.st_mode & S_IXGRP) ? 'x' : '-';
    // 其他人
    perms[7] = (st.st_mode & S_IROTH) ? 'r' : '-';
    perms[8] = (st.st_mode & S_IWOTH) ? 'w' : '-';
    perms[9] = (st.st_mode & S_IXOTH) ? 'x' : '-';

    // 硬链接计数
    int linkNum = st.st_nlink;
    // 文件所有者
    char fileUser = getpwuid(st.st_uid)->pw_name;
    // 文件所属组
    char fileGrp = getgrgid(st.st_gid)->gr_name;
    // 文件大小
    int fileSize = (int)st.st_size;
    // 修改时间
    char time = ctime(&st.st_mtime);
    char mtime[512] = {0};
    strncpy(mtime, time, strlen(time)-1);

    char buf[1024];
    sprintf(buf, "%s  %d  %s  %s  %d  %s  %s", 
            perms, linkNum, fileUser, fileGrp, fileSize, mtime, argv[1]);

    printf("%s\n", buf);

    return 0;
}
```


## 文件描述符复制和重定向

在Linux中只要调用open()函数就可以给被操作的文件分配一个文件描述符，除了使用这种方式Linux系统还提供了一些其他的 API 用于文件描述符的分配，相关函数有三个：`dup, dup2, fcntl`。

### dup
#### 函数详解
dup函数的作用是复制文件描述符，这样就有多个文件描述符可以指向同一个文件了。函数原型如下：
```c
#include <unistd.h>
int dup(int oldfd);
```

- 参数： oldfd 是要被复制的文件描述符
- 返回值：函数调用成功返回被复制出的文件描述符，调用失败返回 -1
下图展示了 `dup()`函数具体行为, 这样不过使用 `fd1`还是使用`fd2`都可以对磁盘文件`A`进行操作了。

<img src="https://www.subingwen.cn/linux/fcntl-dup2/image-20210131123632413.png" alt="image.png" style="zoom:50%;" />

> 被复制出的新文件描述符是独立于旧的文件描述符的，二者没有连带关系。也就是说当旧的文件描述符被关闭了，复制出的新文件描述符还是可以继续使用的。

#### 示例代码
下面的代码中演示了通过`dup()`函数进行文件描述符复制, 并验证了复制之后两个新、旧文件描述符是独立的，二者没有连带关系。
```C++
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>

int main()
{
    // 1. 创建一个新的磁盘文件
    int fd = open("./mytest.txt", O_RDWR|O_CREAT, 0664);
    if(fd == -1)
    {
        perror("open");
        exit(0);
    }
    printf("fd: %d\n", fd);

    // 写数据
    const char pt = "你好, 世界......";
    // 写成功之后, 文件指针在文件尾部
    write(fd, pt, strlen(pt));


    // 复制这个文件描述符 fd
    int newfd = dup(fd);
    printf("newfd: %d\n", newfd);

    // 关闭旧的文件描述符
    close(fd);

    // 使用新的文件描述符继续写文件
    const char ppt = "((((((((((((((((((((((骚年，你要相信光！！！))))))))))))))))))))))";
    write(newfd, ppt, strlen(ppt));
    close(newfd);

    return 0;
}
```

### dup2
#### 函数详解
dup2() 函数是 dup() 函数的加强版，基于dup2() 既可以进行文件描述符的复制, 也可以进行文件描述符的重定向。文件描述符重定向就是改变已经分配的文件描述符关联的磁盘文件。

dup2() 函数原型如下：
```c++
#include <unistd.h>
// 1. 文件描述符的复制, 和dup是一样的
// 2. 能够重定向文件描述符
// 	- 重定向: 改变文件描述符和文件的关联关系, 和新的文件建立关联关系, 和原来的文件断开关联关系
//		1. 首先通过open()打开文件 a.txt , 得到文件描述符 fd
//		2. 然后通过open()打开文件 b.txt , 得到文件描述符 fd1
//		3. 将fd1从定向 到fd上:
//			fd1和b.txt这磁盘文件断开关联, 关联到a.txt上, 以后fd和fd1都对用同一个磁盘文件 a.txt
int dup2(int oldfd, int newfd);
```

- 参数: `oldfd`和`newfd` 都是文件描述符
- 返回值: 函数调用成功返回新的文件描述符, 调用失败返回 -1

关于这个函数的两个参数虽然都是文件描述符，但是在使用过程中又对应了不同的场景，具体如下：

- 场景1:
假设参数 `oldfd` 对应磁盘文件 `a.txt`, `newfd`对应磁盘文件`b.txt`。在这种情况下调用`dup2`函数, 是给`newfd`做了重定向，`newfd` 和文件 `b.txt` 断开关联, 相当于关闭了这个文件, 同时 newfd 指向了磁盘上的a.txt文件，最终 oldfd 和 newfd 都指向了磁盘文件 a.txt。

<img src="https://www.subingwen.cn/linux/fcntl-dup2/image-20210131140755616.png" alt="image.png" style="zoom:40%;" />

- 场景2:
假设参数 `oldfd` 对应磁盘文件 `a.txt`, `newfd`不对应任何的磁盘文件(newfd 必须是一个大于等于0的整数)。在这种情况下调用dup2函数, 在这种情况下会进行文件描述符的复制，newfd 指向了磁盘上的a.txt文件，最终 oldfd 和 newfd 都指向了磁盘文件 a.txt。
<img src="https://www.subingwen.cn/linux/fcntl-dup2/image-20210131144401271.png" alt="image.png" style="zoom:40%;" />
- 场景3:

假设参数 `oldfd` 和`newfd`两个文件描述符对应的是同一个磁盘文件 a.txt, 在这种情况下调用dup2函数, 相当于啥也没发生, 不会有任何改变。

<img src="https://www.subingwen.cn/linux/fcntl-dup2/image-20210131145111359.png" alt="image.png" style="zoom:40%;" />

#### 示例代码

> 给dup2() 的第二个参数指定一个空闲的没被占用的文件描述符就可以进行文件描述符的复制了, 示例代码如下:

```c++
// 使用dup2 复制文件描述符
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>

int main()
{
    // 1. 创建一个新的磁盘文件
    int fd = open("./111.txt", O_RDWR|O_CREAT, 0664);
    if(fd == -1)
    {
        perror("open");
        exit(0);
    }
    printf("fd: %d\n", fd);

    // 写数据
    const char pt = "你好, 世界......";
    // 写成功之后, 文件指针在文件尾部
    write(fd, pt, strlen(pt));


    // 2. fd1没有对应任何的磁盘文件, fd1 必须要 >=0
    int fd1 = 1023;

    // fd -> 111.txt
    // 文件描述符复制, fd1指向fd对应的文件 111.txt
    dup2(fd, fd1);

    // 关闭旧的文件描述符
    close(fd);

    // 使用fd1写文件
    const char ppt = "((((((((((((((((((((((骚年，你要相信光！！！))))))))))))))))))))))";
    write(fd1, ppt, strlen(ppt));
    close(fd1);

    return 0;
}
```

> 将两个有效的文件描述符分别传递给 `dup2()` 函数，就可以实现文件描述符的重定向了。`将第二个参数的文件描述符重定向到参数1文件描述符指向的文件上。` 示例代码如下:


```c++
// 使用dup2 文件描述符重定向
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>

int main()
{
    // 1. 创建一个新的磁盘文件
    int fd = open("./111.txt", O_RDWR|O_CREAT, 0664);
    if(fd == -1)
    {
        perror("open");
        exit(0);
    }
    printf("fd: %d\n", fd);

    // 写数据
    const char pt = "你好, 世界......";
    // 写成功之后, 文件指针在文件尾部
    write(fd, pt, strlen(pt));


    // 2. 创建第二个磁盘文件 222.txt
    int fd1 = open("./222.txt", O_RDWR|O_CREAT, 0664);
    if(fd1 == -1)
    {
        perror("open1");
        exit(0);
    }

    // fd -> 111.txt, fd1->222.txt
    // 从定向, 将fd1指向fd对应的文件 111.txt
    dup2(fd, fd1);

    // 关闭旧的文件描述符
    close(fd);

    // 使用fd1写文件
    const char ppt = "((((((((((((((((((((((骚年，你要相信光！！！))))))))))))))))))))))";
    write(fd1, ppt, strlen(ppt));
    close(fd1);

    return 0;
}
```

### fcntl
#### 函数详解
fcntl() 是一个变参函数, 并且是多功能函数，在这里只介绍如何通过这个函数实现文件描述符的复制和获取/设置已打开的文件属性。该函数的函数原型如下：
```c++
#include <unistd.h>
#include <fcntl.h>	// 主要的头文件

int fcntl(int fd, int cmd, ... / arg / );
```

- 参数:
	- fd: 要操作的文件描述符
	- cmd: 通过该参数控制函数要实现什么功能
- 返回值：函数调用失败返回 -1，调用成功，返回正确的值：
	- 参数 cmd = F_DUPFD：返回新的被分配的文件描述符
	- 参数 cmd = F_GETFL：返回文件的flag属性信息

fcntl() 函数的 cmd 可使用的参数列表:

| 参数 cmd 的取值 |      功能描述      |
| :--------: | :------------: |
|  F_DUPFD   | 复制一个已经存在的文件描述符 |
|  F_GETFL   |   获取文件的状态标志    |
|  F_SETFL   |   设置文件的状态标志    |

文件的状态标志指的是在使用 open() 函数打开文件的时候指定的 flags 属性, 也就是第二个参数

```c
int open(const char pathname, int flags);
```

下表中列出了一些常用的文件状态标志：

|   文件状态标志   |      说明      |
| :--------: | :----------: |
|  O_RDONLY  |     只读打开     |
|  O_WRONLY  |     只写打开     |
|   O_RDWR   |    读、写打开     |
|  O_APPEND  |     追加写      |
| O_NONBLOCK |    非阻塞模式     |
|   O_SYNC   | 等待写完成(数据和属性) |
|  O_ASYNC   |    异步I/O     |
|  O_RSYNC   |    同步读和写     |

#### 复制文件描述符
使用 `fcntl()` 函数进行文件描述符复制, 第二个参数cmd需要指定为 `F_DUPFD`(这是个变参函数其他参数不需要指定)。

```c
int newfd = fcntl(fd, F_DUPFD);
```

> 使用 fcntl() 复制文件描述符, 函数返回值为新分配的文件描述符，示例代码如下:

```c++
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>

int main()
{
    // 1. 创建一个新的磁盘文件
    int fd = open("./mytest.txt", O_RDWR|O_CREAT, 0664);
    if(fd == -1)
    {
        perror("open");
        exit(0);
    }
    printf("fd: %d\n", fd);

    // 写数据
    const char pt = "你好, 世界......";
    // 写成功之后, 文件指针在文件尾部
    write(fd, pt, strlen(pt));


    // 复制这个文件描述符 fd
    int newfd = fcntl(fd, F_DUPFD);
    printf("newfd: %d\n", newfd);

    // 关闭旧的文件描述符
    close(fd);

    // 使用新的文件描述符继续写文件
    const char ppt = "((((((((((((((((((((((骚年，你要相信光！！！))))))))))))))))))))))";
    write(newfd, ppt, strlen(ppt));
    close(newfd);

    return 0;
}
```

#### 设置文件状态标志
通过 `open()`函数打开文件之后, 文件的`flag`属性就已经被确定下来了，如果想要在打开状态下修改这些属性，可以使用 `fcntl()`函数实现, 但是有一点需要注意, 不是所有的flag 属性都能被动态修改, 只能修改如下状态标志: `O_APPEND, O_NONBLOCK, O_SYNC, O_ASYNC, O_RSYNC`等。

得到已打开的文件的状态标志，需要将 cmd 设置为 `F_GETFL`，得到的信息在函数的返回值中

```c
int flag = fcntl(fd, F_GETFL);
```

设置已打开的文件的状态标志，需要将 cmd 设置为 `F_SETFL`，新的flag需要通过第三个参数传递给 `fcntl()` 函数

```c++
// 得到文件的flag属性
int flag = fcntl(fd, F_GETFL);
// 添加新的flag 标志
flag = flag | O_APPEND;
// 将更新后的falg设置给文件
fcntl(fd, F_SETFL, flag);
```

> 举例： 通过fcntl()函数`获取/设置已打开的文件属性`，先来描述一下场景：
> 
> 如果要往当前文件中写数据, 打开一个新文件, 文件的写指针在文件头部，数据默认也是写到文件开头，如果不想将数据写到文件头部, 可以给文件追加一个`O_APPEND`属性。实例代码如下：

```c++
// 写实例程序, 给文件描述符追加 O_APPEND
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>

int main()
{
    // 1. 打开一个已经存在的磁盘文件
    int fd = open("./111.txt", O_RDWR);
    if(fd == -1)
    {
        perror("open");
        exit(0);
    }
    printf("fd: %d\n", fd);

    // 如果不想将数据写到文件头部, 可以给文件描述符追加一个O_APPEND属性
    // 通过fcntl获取文件描述符的 flag属性
    int flag = fcntl(fd, F_GETFL);
    // 给得到的flag追加 O_APPEND属性
    flag = flag | O_APPEND; // flag |= O_APPEND;
    // 重新将flag属性设置给文件描述符
    fcntl(fd, F_SETFL, flag);

    // 使用fd写文件, 添加的数据应该写到文件尾部
    const char ppp = "((((((((((((((((((((((骚年，你要相信光！！！))))))))))))))))))))))";
    write(fd, ppp, strlen(ppp));
    close(fd);

    return 0;
}
```

## 目录遍历

众所周知，Linux的目录是一个树状结构，了解数据结构的小伙伴都明白，遍历一棵树最简单的方式是递归。在我们已经掌握了递归的使用方法之后，遍历树状目录也不是一件难事儿。

Linux给我们提供了相关的目录遍历的函数，分别为：`opendir(), readdir(), closedir()`。目录的操作方式和标准C库提供的文件操作步骤是类似的。下面来依次介绍一下这几个函数。

### 目录三剑客
#### opendir

> 在目录操作之前必须要先通过 opendir() 函数打开这个目录，函数原型如下：

```c
#include <sys/types.h>
#include <dirent.h>
// 打开目录
DIR opendir(const char name);
```

- 参数: name -> 要打开的目录的名字
- 返回值: DIR, 结构体类型指针。打开成功返回目录的实例，打开失败返回 NULL
#### readdir

> 目录打开之后，就可以通过 `readdir()` 函数遍历目录中的文件信息了。每调用一次这个函数就可以得到目录中的一个文件信息，当目录中的文件信息被全部遍历完毕会得到一个空对象。先来看一下这个函数原型：

```c
// 读目录
#include <dirent.h>
struct dirent readdir(DIR dirp);
```

- 参数：`dirp -> opendir()` 函数的返回值
- 返回值：函数调用成功，返回读到的文件的信息, 目录文件被读完了或者函数调用失败返回 NULL
函数返回值 `struct dirent` 结构体原型如下:

```c
struct dirent {
    ino_t          d_ino;       / 文件对应的inode编号, 定位文件存储在磁盘的那个数据块上 /
    off_t          d_off;       / 文件在当前目录中的偏移量 /
    unsigned short d_reclen;    / 文件名字的实际长度 /
    unsigned char  d_type;      / 文件的类型, linux中有7中文件类型 /
    char           d_name[256]; / 文件的名字 /
};
```

关于结构体中的文件类型`d_type`，可使用的宏值如下：
- `DT_BLK`：块设备文件
- `DT_CHR`：字符设备文件
- `DT_DIR`：目录文件
- `DT_FIFO`：管道文件
- `DT_LNK`：软连接文件
- `DT_REG`：普通文件
- `DT_SOCK`：本地套接字文件
- `DT_UNKNOWN`：无法识别的文件类型

那么，如何通过 readdir() 函数遍历某一个目录中的文件呢？

```c++
// 打开目录
DIR dir = opendir("/home/test");
struct dirent ptr = NULL;
// 遍历目录
while( (ptr=readdir(dir)) != NULL)
{
    .......
}
```

#### closedir
目录操作完毕之后, 需要通过 `closedir()`关闭通过`opendir()`得到的实例，释放资源。函数原型如下：

```c
// 关闭目录, 参数是 opendir() 的返回值
int closedir(DIR dirp);
```

- 参数：`dirp-> opendir()` 函数的返回值
- 返回值: 目录关闭成功返回0, 失败返回 -1

### 遍历目录
#### 遍历单层目录
如果只遍历单层目录是不需要递归的，按照上边介绍的函数的使用方法，依次继续调用即可。假设我们需要得到某个指定目录下 `mp3` 格式文件的个数，示例代码如下：

```c
// filenum.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <dirent.h>

int main(int argc, char argv[])
{
    // 1. 打开目录
    DIR dir = opendir(argv[1]);
    if(dir == NULL)
    {
        perror("opendir");
        return -1;
    }

    // 2. 遍历当前目录中的文件
    int count = 0;
    while(1)
    {
        struct dirent ptr = readdir(dir);
        if(ptr == NULL)
        {
            printf("目录读完了...\n");
            break;
        }
        // 读到了一个文件
        // 判断文件类型
        if(ptr->d_type == DT_REG)
        {
            char p = strstr(ptr->d_name, ".mp3");
            if(p != NULL && (p+4) == '\0')
            {
                count++;
                printf("file %d: %s\n", count, ptr->d_name);
            }
        }
    }

    printf("%s目录中mp3文件的个数: %d\n", argv[1], count);

    // 关闭目录
    closedir(dir);

    return 0;
}
```

#### 遍历多层目录
Linux 的目录是树状结构，遍历每层目录的方式都是一样的，也就是说最简单的遍历方式是递归。程序的重点就是确定递归结束的条件：`遍历的文件如果不是目录类型就结束递归`。

示例代码如下：

```c
// filenum.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <dirent.h>

int getMp3Num(const char path)
{
    // 1. 打开目录
    DIR dir = opendir(path);
    if(dir == NULL)
    {
        perror("opendir");
        return 0;
    }
    // 2. 遍历当前目录
    struct dirent ptr = NULL;
    int count = 0;
    while((ptr = readdir(dir)) != NULL)
    {
        // 如果是目录 . .. 跳过不处理
        if(strcmp(ptr->d_name, ".")==0 ||
           strcmp(ptr->d_name, "..") == 0)
        {
            continue;
        }
        // 假设读到的当前文件是目录
        if(ptr->d_type == DT_DIR)
        {
            // 目录
            char newPath[1024];
            sprintf(newPath, "%s/%s", path, ptr->d_name);
            // 读当前目录的子目录
            count += getMp3Num(newPath);
        }
        else if(ptr->d_type == DT_REG)
        {
            // 普通文件
            char p = strstr(ptr->d_name, ".mp3");
            // 判断文件后缀是不是 .mp3
            if(p != NULL && (p+4) == '\0')
            {
                count++;
                printf("%s/%s\n", path, ptr->d_name);
            }
        }
    }

    closedir(dir);
    return count;
}

int main(int argc, char argv[])
{
    // ./a.out path
    if(argc < 2)
    {
        printf("./a.out path\n");
        return 0;
    }

    int num = getMp3Num(argv[1]);
    printf("%s 目录中mp3文件个数: %d\n", argv[1], num);

    return 0;
}
```

编译并运行程序：
```shell
$ gcc filenum.c
# 查看 abc 目录中mp3 文件个数
$ ./a.out abc
abc/sub/3.mp3
abc/sub/1.mp3
abc/sub/5.mp3
abc/sub/4.mp3
abc/sub/2.mp3
abc/sub/music/test2.mp3
abc/sub/music/test3.mp3
abc/sub/music/test1.mp3
abc/hello.mp3
abc 目录中mp3文件个数: 9
```

### scandir函数
除了使用上边介绍的目录三剑客遍历目录，也可以使用`scandir()`函数进行目录的遍历(只遍历指定目录，不进入到子目录中进行递归遍历)，它的参数并不简单，涉及到三级指针和回调函数的使用。

其函数原型如下：

```c
// 头文件
#include <dirent.h> 
int scandir(const char dirp, struct dirent namelist,
              int (filter)(const struct dirent ),
              int (compar)(const struct dirent , const struct dirent ));
int alphasort(const struct dirent a, const struct dirent b);
int versionsort(const struct dirent a, const struct dirent b);
```

- 参数:
- dirp: 需要遍历的目录的名字
- namelist: 三级指针, 传出参数, 需要在指向的地址中存储遍历目录得到的所有文件的信息
	- `在函数内部会给这个指针指向的地址分配内存，要注意在程序中释放内存`
- filter: 函数指针, 指针指向的函数就是回调函数, 需要在自定义函数中指定如果过滤目录中的文件
	- 如果不对目录中的文件进行过滤, 该函数指针指定为NULL即可
	- 如果自己指定过滤函数, 满足条件要返回1, 否则返回 0
- compar: 函数指针, 对过滤得到的文件进行排序, 可以使用提供的两种排序方式:
	- alphasort: 根据文件名进行排序
	- versionsort: 根据版本进行排序
- 返回值: 函数执行成功返回找到的匹配成功的文件的个数，如果失败返回-1。

### 文件过滤
`scandir()` 可以让使用者自定义文件的过滤方式, 然后将过滤函数的地址传递给 scandir() 的第三个参数，我们可以得知过滤函数的原型是这样的：

```c
// 函数的参数就是遍历的目录中的子文件对应的结构体
int (filter)(const struct dirent );
```

> 基于这个函数指针定义的函数就可以称之为回调函数, 这个函数不是由程序猿调用, 而是通过 scandir() 调用，因此这个函数的实参也是由 scandir() 函数提供的，作为回调函数的编写人员，只需要搞明白这个参数的含义是什么，然后在函数体中直接使用即可。

假设还是判断目录中某一个文件是否为Mp3格式, 函数实现如下:

```c
int isMp3(const struct dirent ptr)
{
    if(ptr->d_type == DT_REG)
    {
        char p = strstr(ptr->d_name, ".mp3");
        if(p != NULL && (p+4) == '\0')
        {
            return 1;
        }
    }
    return 0;
}
```

#### 遍历目录
了解了 `scandir()` 函数的使用之后, 下边写一个程序, 来搜索指定目录下的 `mp3`格式文件个数和文件名, 代码如下:

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <dirent.h>

// 文件过滤函数
int isMp3(const struct dirent ptr)
{
    if(ptr->d_type == DT_REG)
    {
        char p = strstr(ptr->d_name, ".mp3");
        if(p != NULL && (p+4) == '\0')
        {
            return 1;
        }
    }
    return 0;
}

int main(int argc, char argv[])
{
    if(argc < 2)
    {
        printf("./a.out path\n");
        return 0;
    }
    struct dirent namelist = NULL;
    int num = scandir(argv[1], &namelist, isMp3, alphasort);
    for(int i=0; i<num; ++i)
    {
        printf("file %d: %s\n", i, namelist[i]->d_name);
        free(namelist[i]);
    }
    free(namelist);
    return 0;
}
```

最后再解析一下 `scandir()` 的第二个参数，传递的是一个二级指针的地址:

```c
struct dirent namelist = NULL;
int num = scandir(argv[1], &namelist, isMp3, alphasort);
```

那么在这个 namelist 中存储的什么类型的数据呢？也就是 `struct dirent namelist` 指向的什么类型的数据?
答案: 指向的是一个指针数组 `struct dirent namelist[]`

- 数组元素的个数就是遍历的目录中的文件个数
- 数组的每个元素都是指针类型: `struct dirent `, 指针指向的地址是有 `scandir()` 函数分配的, 因此在使用完毕之后需要释放内存

# 位运算

## 位逻辑运算符

### 按位取反~
一元运算符~将每个1变为0，将每个0变为1，如下面的例子：
```c
~(10011010)
01100101
```

假设a是一个unsigned char，已赋值为2.在二进制中，2是00000010.于是-a的值为11111101或者253。请注意该运算符不会改变a的值，a仍为2。
```c
unsigned char a = 2;   //00000010
unsigned char b = ~a;  //11111101
printf("ret = %d\n", a); //ret = 2
printf("ret = %d\n", b); //ret = 253
```

### 位与(AND): &
二进制运算符&通过对两个操作数逐位进行比较产生一个新值。对于每个位，只有两个操作数的对应位都是1时结果才为1。
```c
   (10010011) 
 & (00111101) 
 = (00010001)
```
C也有一个组合的位与-赋值运算符：&=。下面两个将产生相同的结果：
```c
val &= 0377
val = val & 0377
```

### 位或(OR): |
二进制运算符|通过对两个操作数逐位进行比较产生一个新值。对于每个位，如果其中任意操作数中对应的位为1，那么结果位就为1.
```c
	(10010011)
  | (00111101)
  = (10111111)
```
C也有组合位或-赋值运算符： |=
```c
val |= 0377
val = val | 0377
```

### 位异或: 
二进制运算符`^`对两个操作数逐位进行比较。对于每个位，如果操作数中的对应位有一个是1(但不是都是1)，那么结果是1.如果都是0或者都是1，则结果位0.
```c
	(10010011)
  ^ (00111101)
  = (10101110)
```
C也有一个组合的位异或-赋值运算符： `^=`
```c
val ^= 0377
val = val ^ 0377
```


### 用法

#c语言面试点 
```c
交换两个数不需要临时变量
//a ^ b = temp;
//a ^ temp = b;
//b ^ temp = a
 (10010011)
^(00100110)
=(10110101)

 (10110101)
^(00100110)
  10010011
用加减法但是有溢出风险
int a = 10;
int b = 30;
a=a+b
b=a-b
a=a-b
```


```c
//判断奇偶
def is_even(num):
    if num & 1 == 0:
        return True
    else:
        return False
# 示例用法
print(is_even(4))  # 输出: True，因为4是偶数
print(is_even(7))  # 输出: False，因为7是奇数
```

```c++

// 与2的幂取模
// 当用一个数与 2 的幂减 1(如 1, 3, 7, 15, 31 等)进行位与操作时，相当于对这个数执行模 2 的幂(如 2, 4, 8, 16, 32 等)操作。
  size_t n=d(rd);
  size_t uint_max=(1<<n)-1;
  size_t n32=7&uint_max;
  size_t n31=7%(uint_max+1);
  cout<<(n31==n32)<<endl; //1
```



## 移位运算符


现在让我们了解一下C的移位运算符。移位运算符将位向左或向右移动。同样，我们仍将明确地使用二进制形式来说明该机制的工作原理。
### 左移 <<

左移运算符`<<`将其左侧操作数的值的每位向左移动，移动的位数由其右侧操作数指定。空出来的位用0填充，并且丢弃移出左侧操作数末端的位。在下面例子中，每位向左移动两个位置。
```c
(10001010) << 2
(00101000)
```

该操作将产生一个新位置，但是不改变其操作数。
	
```c
1 << 1 = 2;
2 << 1 = 4;
4 << 1 = 8;
8 << 2 = 32
```
左移一位相当于原值`2`.
### 右移 >>

右移运算符>>将其左侧的操作数的值每位向右移动，移动的位数由其右侧的操作数指定。丢弃移出左侧操作数有段的位。对于unsigned类型，使用0填充左端空出的位。对于有符号类型，结果依赖于机器。空出的位可能用0填充，或者使用符号(最左端)位的副本填充。
	
```c
//有符号值
(10001010) >> 2
(00100010)     //在某些系统上的结果值

(10001010) >> 2
(11100010)     //在另一些系统上的结果

//无符号值
(10001010) >> 2
(00100010)    //所有系统上的结果值
```

用法：移位运算符
	移位运算符能够提供快捷、高效(依赖于硬件)对2的幂的乘法和除法。
	
```c
number << n	number乘以2的n次幂
number >> n	如果number非负，则用number除以2的n次幂
```

# 链表
## 链表基本概念
### 什么是链表

<img src="https://article.biliimg.com/bfs/article/542abc1a01c007270c3467494491126e38716159.png" alt="image.png" style="zoom:50%;" />


- 链表是一种常用的数据结构，它通过指针将一些列数据结点，连接成一个数据链。相对于数组，链表具有更好的动态性(<font color=#ff0000>非顺序存储</font>)。
- 数据域用来存储数据，指针域用于建立与下一个结点的联系。
- 建立链表时无需预先知道数据总量的，可以==随机的分配空间==，可以高效的在链表中的任意位置实时插入或删除数据。
- 链表的开销，主要是==访问顺序性==和组织链的空间损失。

数组和链表的区别：


> 数组：一次性分配一块连续的存储区域。
> 优点：随机访问元素效率高
> 缺点：1) 需要分配一块连续的存储区域(很大区域，有可能分配失败)
>       2) 删除和插入某个元素效率低
> 链表：无需一次性分配一块连续的存储区域，只需分配n块节点存储区域，通过指针建立关系。
> 优点：1) 不需要一块连续的存储区域
> 	  2) 删除和插入某个元素效率高
> 缺点：随机访问元素效率低


### 链表节点
大家思考一下，我们说链表是由一系列的节点组成，那么如何表示一个包含了数据域和指针域的节点呢？
链表的节点类型实际上是结构体变量，此结构体包含数据域和指针域：
- 数据域用来存储数据；
- 指针域用于建立与下一个结点的联系，当<font color=#ff0000>此节点为尾节点时，指针域的值为NULL</font>；

```c
typedef struct Node 
{
	//数据域
	int id;
	char name[50];

	//指针域
	struct Node next;       
}Node;
```


![image.png](https://article.biliimg.com/bfs/article/a4652c7772b64e517b4a10d7e94d2bc938716159.png)


### 链表的分类
链表分为：静态链表和动态链表
静态链表和动态链表是线性表链式存储结构的两种不同的表示方式：
- 所有结点都是在程序中定义的，不是临时开辟的，也不能用完后释放，这种链表称为“静态链表”。
- 所谓动态链表，是指在程序执行过程中从无到有地建立起一个链表，即一个一个地开辟结点和输入各结点数据，并建立起前后相链的关系。

#### 静态链表
```c
typedef struct Stu
{
	int id;	//数据域
	char name[100];

	struct Stu next; //指针域
}Stu;
```

```c
void test()
{
	//初始化三个结构体变量
	Stu s1 = { 1, "yuri", NULL };
	Stu s2 = { 2, "lily", NULL };
	Stu s3 = { 3, "lilei", NULL };

	s1.next = &s2; //s1的next指针指向s2
	s2.next = &s3;
	s3.next = NULL; //尾结点

	Stu p = &s1;
	while (p != NULL)
	{
		printf("id = %d, name = %s\n", p->id, p->name);

		//结点往后移动一位
		p = p->next; 
	}
}
```


动态链表
```c
typedef struct Stu{
	int id;	//数据域
	char name[100];

	struct Stu next; //指针域
}Stu;
```

```c
void test(){
	//动态分配3个节点
	Stu s1 = (Stu )malloc(sizeof(Stu));
	s1->id = 1;
	strcpy(s1->name, "yuri");

	Stu s2 = (Stu )malloc(sizeof(Stu));
	s2->id = 2;
	strcpy(s2->name, "lily");

	Stu s3 = (Stu )malloc(sizeof(Stu));
	s3->id = 3;
	strcpy(s3->name, "lilei");

	//建立节点的关系
	s1->next = s2; //s1的next指针指向s2
	s2->next = s3;
	s3->next = NULL; //尾结点

	//遍历节点
	Stu p = s1;
	while (p != NULL)
	{
		printf("id = %d, name = %s\n", p->id, p->name);

		//结点往后移动一位
		p = p->next; 
	}

	//释放节点空间
	p = s1;
	Stu tmp = NULL;
	while (p != NULL)
	{
		tmp = p;
		p = p->next;

		free(tmp);
		tmp = NULL;
	}
}
```

#### 带头和不带头链表
- 带头链表：固定一个节点作为头结点(数据域不保存有效数据)，起一个标志位的作用，以后不管链表节点如果改变，此头结点固定不变。

	<img src="https://article.biliimg.com/bfs/article/23c1b5060a864feae6e03a783afb17cb38716159.png" alt="image.png" style="zoom:50%;" />

- 不带头链表：头结点不固定，根据实际需要变换头结点(如在原来头结点前插入新节点，然后，新节点重新作为链表的头结点)。

	<img src="https://article.biliimg.com/bfs/article/fdad138e35b2c303bfc5c6abb2fe0dfc38716159.png" alt="image.png" style="zoom:50%;" />

#### 单向链表、双向链表、循环链表
单向链表：

<img src="https://article.biliimg.com/bfs/article/ff416b9923aeedf8d6d8a124faf3316038716159.png" alt="image.png" style="zoom:50%;" />

双向链表：

<img src="https://article.biliimg.com/bfs/article/b457d21a8e50fb0891ddbbd0d4254cba38716159.png" alt="image.png" style="zoom:50%;" />

循环链表：
<img src="https://article.biliimg.com/bfs/article/0c030564606b453db836d34432ea8e4a38716159.png" alt="image.png" style="zoom:50%;" />

## 链表基本操作
### 创建链表
使用结构体定义节点类型：
```c
typedef struct _LINKNODE
{
	int id; //数据域
	struct _LINKNODE next; //指针域
}link_node;
```

编写函数：`link_node init_linklist()`
建立带有头结点的单向链表，循环创建结点，结点数据域中的数值从键盘输入，以 -1 作为输入结束标志，链表的头结点地址由函数值返回.

```c
typedef struct _LINKNODE{
	int data;
	struct _LINKNODE next;
}link_node;

link_node init_linklist(){
	
	//创建头结点指针
	link_node head = NULL;
	//给头结点分配内存
	head = (link_node)malloc(sizeof(link_node));
	if (head == NULL){
		return NULL;
	}
	head->data = -1;
	head->next = NULL;

	//保存当前节点
	link_node p_current = head;
	int data = -1;
	//循环向链表中插入节点
	while (1){
	
		printf("please input data:\n");
		scanf("%d",&data);

		//如果输入-1，则退出循环
		if (data == -1){
			break;
		}

		//给新节点分配内存
		link_node newnode = (link_node)malloc(sizeof(link_node));
		if (newnode == NULL){
			break;
		}

		//给节点赋值
		newnode->data = data;
		newnode->next = NULL;

		//新节点入链表，也就是将节点插入到最后一个节点的下一个位置
		p_current->next = newnode;
		//更新辅助指针p_current
		p_current = newnode;
	}

	return head;
}
```

### 遍历链表
编写函数：`void foreach_linklist(link_node head)`
顺序输出单向链表各项结点数据域中的内容：
```c
//遍历链表
void foreach_linklist(link_node head){
	if (head == NULL){
		return;
	}

	//赋值指针变量
	link_node p_current = head->next;
	while (p_current != NULL){
		printf("%d ",p_current->data);
		p_current = p_current->next;
	}
	printf("\n");
}
```


### 插入节点
编写函数: `void insert_linklist(link_node head,int val,int data)`.
在指定值后面插入数据data,如果值val不存在，则在尾部插入。
```c
//在值val前插入节点
void insert_linklist(link_node head, int val, int data){
	
	if (head == NULL){
		return;
	}

	//两个辅助指针
	link_node p_prev = head;
	link_node p_current = p_prev->next;
	while (p_current != NULL){
		if (p_current->data == val){
			break;
		}
		p_prev = p_current;
		p_current = p_prev->next;
	}

	//如果p_current为NULL，说明不存在值为val的节点
	if (p_current == NULL){
		printf("不存在值为%d的节点!\n",val);
		return;
	}

	//创建新的节点
	link_node newnode = (link_node)malloc(sizeof(link_node));
	newnode->data = data;
	newnode->next = NULL;

	//新节点入链表
	newnode->next = p_current;
	p_prev->next = newnode;
}
```
### 删除节点
编写函数: `void remove_linklist(link_node head,int val)`
删除第一个值为val的结点.
```c
//删除值为val的节点
void remove_linklist(link_node head,int val){
	if (head == NULL){
		return;
	}

	//辅助指针
	link_node p_prev = head;
	link_node p_current = p_prev->next;

	//查找值为val的节点
	while (p_current != NULL){
		if (p_current->data == val){
			break;
		}
		p_prev = p_current;
		p_current = p_prev->next;
	}
	//如果p_current为NULL，表示没有找到
	if (p_current == NULL){
		return;
	}
	
	//删除当前节点： 重新建立待删除节点(p_current)的前驱后继节点关系
	p_prev->next = p_current->next;
	//释放待删除节点的内存
	free(p_current);
}
```

### 销毁链表
编写函数: `void destroy_linklist(link_node head)`
销毁链表，释放所有节点的空间.
```c
//销毁链表
void destroy_linklist(link_node head){
	if (head == NULL){
		return;
	}
	//赋值指针
	link_node p_current = head;
	while (p_current != NULL){
		//缓存当前节点下一个节点
		link_node p_next = p_current->next;
		free(p_current);
		p_current = p_next;
	}
}
```


# 函数指针和回调函数
## 函数指针
### 函数类型
通过什么来区分两个不同的函数？
- 一个函数在编译时被分配一个入口地址，这个地址就称为函数的指针，<font color=#ff0000>函数名</font>代表函数的入口地址。
- 函数三要素： 名称、参数、返回值。C语言中的函数有自己特定的类型。
c语言中通过typedef为函数类型重命名：
```c
typedef int (*f)(int, int);		// f 为函数类型
typedef void p(int);		// p 为函数类型
```

这一点和数组一样，因此我们可以用一个指针变量来存放这个入口地址，然后通过该指针变量调用函数。

> <font color=#ff0000>注意</font>：通过函数类型定义的变量是不能够直接执行，因为没有函数体。只能通过类型定义一个函数指针指向某一个具体函数，才能调用。

```c
typedef int(p)(int, int);

void my_func(int a,int b){
	printf("%d %d\n",a,b);
}

void test(){

	p p1;
	//p1(10,20); //错误，不能直接调用，只描述了函数类型，但是并没有定义函数体，没有函数体无法调用
	p p2 = my_func;
	p2(10,20); //正确，指向有函数体的函数入口地址
}
```


### 函数指针(指向函数的指针)
- 函数指针定义方式(先定义函数类型，根据类型定义指针变量);
- 先定义函数指针类型，根据类型定义指针变量;
- 直接定义函数指针变量;

```c
int my_func(int a,int b){
	printf("ret:%d\n", a + b);
	return 0;
}

//1. 先定义函数类型，通过类型定义指针
void test01(){
	typedef int(FUNC_TYPE)(int, int);
	FUNC_TYPE f = my_func;
	//如何调用？
	(f)(10, 20);
	f(10, 20);
}

//2. 定义函数指针类型
void test02(){
	typedef int(FUNC_POINTER)(int, int);
	FUNC_POINTER f = my_func;
	//如何调用？
	(f)(10, 20);
	f(10, 20);
}

//3. 直接定义函数指针变量
void test03(){
	
	int(f)(int, int) = my_func;
	//如何调用？
	(f)(10, 20);
	f(10, 20);
}
```

### 函数指针数组
函数指针数组，每个元素都是函数指针。

```c
void func01(int a){
	printf("func01:%d\n",a);
}
void func02(int a){
	printf("func02:%d\n", a);
}
void func03(int a){
	printf("func03:%d\n", a);
}

void test(){

#if 0
	//定义函数指针
	void(func_array[])(int) = { func01, func02, func03 };
#else
	void(func_array[3])(int);
	func_array[0] = func01;
	func_array[1] = func02;
	func_array[2] = func03;
#endif

	for (int i = 0; i < 3; i ++){
		func_array[i](10 + i);
		(func_array[i])(10 + i);
	}
}
```


### 函数指针做函数参数(回调函数)
函数参数除了是普通变量，还可以是函数指针变量。
```c
//形参为普通变量
void fun( int x ){}
//形参为函数指针变量
void fun( int(p)(int a) ){}
```
函数指针变量常见的用途之一是把指针作为参数传递到其他函数，指向函数的指针也可以作为参数，以实现函数地址的传递。

```c
int plus(int a, int b)
{
	return a + b;
}
int sub(int a, int b)
{
	return a - b;
}
int mul(int a, int b)
{
	return a  b;
}
int division(int a, int b)
{
	return a / b;
}

typedef int (myCalculate)(int,int); 

//函数指针 做函数的参数 --- 回调函数
void Calculator(int(myCalculate)(int, int), int a, int b)
{
	int ret = myCalculate(a, b); //dowork中不确定用户选择的内容，由后期来指定运算规则
	printf("ret = %d\n", ret);
}
void Calculator(myCalculate func, int a, int b)
{
	int ret = myCalculate(a, b); //dowork中不确定用户选择的内容，由后期来指定运算规则
	printf("ret = %d\n", ret);
}
void test01()
{
	printf("请输入操作符\n");
	printf("1、+ \n");
	printf("2、- \n");
	printf("3、 \n");
	printf("4、/ \n");

	int select = -1;
	scanf("%d", &select);

	int num1 = 0;
	printf("请输入第一个操作数：\n");
	scanf("%d", &num1);

	int num2 = 0;
	printf("请输入第二个操作数：\n");
	scanf("%d", &num2);

	switch (select)
	{
	case  1:
		Calculator(plus, num1, num2);
		break;
	case  2:
		Calculator(sub, num1, num2);
		break;
	case 3:
		Calculator(mul, num1, num2);
		break;
	case 4:
		Calculator(division, num1, num2);
		break;
	default:
		break;
	}

}
```

> [!attention]
> <font color=#ff0000>注意</font>：函数指针和指针函数的区别：        
> 函数指针是指向函数的指针；       
> 指针函数是返回类型为指针的函数；      
> 

# 预处理
## 预处理的基本概念
C语言对源程序处理的四个步骤：预处理、编译、汇编、链接。
预处理是在程序源代码被编译之前，由预处理器(Preprocessor)对程序源代码进行的处理。这个过程并不对程序的源代码语法进行解析，但它会把源代码分割或处理成为特定的符号为下一步的编译做准备工作。

## 文件包含指令(#include)
### 文件包含处理
“文件包含处理”是指一个源文件可以将另外一个文件的全部内容包含进来。Ｃ语言提供了#include命令用来实现“文件包含”的操作。

<img src="https://article.biliimg.com/bfs/article/4753eac5012d43fc5cef69eea5f0525c38716159.png" alt="image.png" style="zoom:20%;" />



### `#incude<>和#include""区别`
- `""` 表示系统先在file1.c所在的当前目录找file1.h，如果找不到，再按系统指定的目录检索。
- `< >` 表示系统<font color=#ff0000>直接</font>按系统指定的目录检索。

注意：
	1. `#include <>`常用于包含库函数的头文件；
	2. `#include ""`常用于包含自定义的头文件；
	3. 理论上`#include`可以包含任意格式的文件(.c .h等) ，但一般用于<font color=#ff0000>头文件</font>的包含；


## 宏定义
### 无参数的宏定义(宏常量)

如果在程序中大量使用到了100这个值，那么为了方便管理，我们可以将其定义为：
const int num = 100; 但是如果我们使用num定义一个数组，在不支持c99标准的编译器上是不支持的，因为num不是一个编译器常量，如果想得到了一个编译器常量，那么可以使用：
`#define num 100`
在编译预处理时，将程序中在该语句以后出现的所有的num都用100代替。这种方法使用户能以一个简单的名字代替一个长的字符串，在预编译时将宏名替换成字符串的过程称为“<font color=#ff0000>宏展开</font>”。宏定义，只在宏定义的文件中起作用。

```c
#define PI 3.1415
void test(){
	double r = 10.0;
	double s = PI  r  r;
	printf("s = %lf\n", s);
}
```

说明：
1)	宏名一般用大写，以便于与变量区别；
2)	宏定义可以是常数、表达式等；
3)	宏定义不作<font color=#ff0000>语法检查</font>，只有在编译被宏展开后的源程序才会报错；
4)	宏定义不是C语言，不在行末加分号；
5)	宏名有效范围为从定义到本源文件结束；
6)	可以用#undef命令终止宏定义的作用域；
7)	在宏定义中，可以引用已定义的宏名；

### 带参数的宏定义(宏函数)
在项目中，经常把一些短小而又频繁使用的函数写成宏函数，这是由于宏函数没有普通函数参数<font color=#ff0000>压栈、跳转、返回</font>等的开销，可以调高程序的效率。
宏通过使用参数，可以创建外形和作用都与函数类似地类函数宏(function-like macro). 宏的参数也用圆括号括起来。
```c
#define SUM(x,y) (( x )+( y ))
void test(){
	//仅仅只是做文本替换 下例替换为 int ret = ((10)+(20));
	//不进行计算
	int ret = SUM(10, 20);
	printf("ret:%d\n",ret);
}
```

注意:
- 宏的名字中不能有空格，但是在替换的字符串中可以有空格。ANSI C允许在参数列表中使用空格；
- 用括号括住每一个参数，并括住宏的整体定义。
- 用大写字母表示宏的函数名。
- 如果打算宏代替函数来加快程序运行速度。假如在程序中只使用一次宏对程序的运行时间没有太大提高。
- 宏是没有作用域概念的，永远是全局生效。所以，对于一些用来简化代码、起临时作用的宏，最好是用完后尽快用“#undef”取消定义，避免冲突的风险。像下面这样：

```c
#define CUBE(a) (a)  (a)  (a) // 定义一个简单的求立方的宏
cout << CUBE(10) << endl; // 使用宏简化代码
cout << CUBE(15) << endl; // 使用宏简化代码
#undef CUBE // 使用完毕后立即取消定义
```

## 条件编译
### 基本概念
一般情况下，源程序中所有的行都参加编译。但有时希望对部分源程序行<font color=#ff0000>只在满足一定条件时才编译</font>，即对这部分源程序行指定编译条件。

<img src="https://article.biliimg.com/bfs/article/25ee341df4503c48657626c27247b87538716159.png" alt="image.png" style="zoom:50%;" />

### 条件编译
防止头文件被重复包含引用；
```c
#ifndef _SOMEFILE_H
#define _SOMEFILE_H

//需要声明的变量、函数
//宏定义
//结构体

#endif
```

## 一些特殊的预定宏

C编译器，提供了几个特殊形式的预定义宏，在实际编程中可以直接使用，很方便。
```c
//	__FILE__			宏所在文件的源文件名 
//	__LINE__			宏所在行的行号
//	__DATE__			代码编译的日期
//	__TIME__			代码编译的时间

void test()
{
	printf("%s\n", __FILE__);
	printf("%d\n", __LINE__);
	printf("%s\n", __DATE__);
	printf("%s\n", __TIME__);
}
```


# 多进程

## 进程控制
### 进程概述

从严格意义上来讲，程序和进程是两个不同的概念，他们的状态，占用的系统资源都是不同的。
- 程序：就是磁盘上的可执行文件文件, 并且只占用磁盘上的空间，是一个静态的概念。
- 进程：被执行之后的程序叫做进程，不占用磁盘空间，需要消耗系统的`内存，CPU资源`，每个运行的进程的都对应一个属于自己的虚拟地址空间，这是一个动态的概念 [[零散知识点#虚拟地址空间|虚拟地址空间详解]]



#### 并行和并发
- CPU时间片
	- CPU在某个时间点只能处理一个任务，但是操作系统都支持多任务的，那么在计算机CPU只有一个核的情况下是怎么完成多任务处理的呢？原理和古时候救济灾民的思路是一样的，每个人分一点，但是又不叫吃饱。
	- <font color=#ff0000>CPU会给每个进程被分配一个时间段</font>，进程得到这个时间片之后才可以运行，使各个程序从表面上看是同时进行的。<font color=#ff0000>如果在时间片结束时进程还在运行，CPU的使用权将被收回，该进程将会被中断挂起等待下一个时间片</font>。如果进程在时间片结束前阻塞或结束，则CPU当即进行切换，这样就可以避免CPU资源的浪费。
	- 因此可以得知，在我们使用的计算机中启动的多个程序，从宏观上看是同时运行的，从微观上看由于CPU一次只能处理一个进程，所有它们是轮流执行的，只不过切换速度太快，我们感觉不到罢了，因此CPU的核数越多计算机的处理效率越高。
- 并发和并行
这两个概念呢都可以笼统的解释为：多个进程同时运行，但是他们两个的同时并不是同一个概念。Erlang 之父 Joe Armstrong 用一张5岁小孩都能看懂的图解释了并发与并行的区别：

<img src="https://www.subingwen.cn/linux/process/Concurrency-vs-Parallelism.png" alt="image.png" style="zoom:80%;" />

> 并发：第一幅图是并发。

- 并发的同时运行是一个假象，咖啡机也好CPU也好在某一个时间点只能为某一个个体来服务，因此不可能同时处理多任务，这是通过上图的咖啡机/计算机的CPU快速的时间片切换实现的。
- 并发是针对某一个硬件资源而言的，在某个时间段之内处理的任务的总量，量越大效率越高。
- 并发也可以理解为是一个屌丝通过不断努力自我升华的结果。

> 并行：第二幅图是并行。

- 并行的多进程同时运行是真实存在的，可以在同一时刻同时运行多个进程
- 并行需要依赖多个硬件资源，单个是无法实现的(图中有两台咖啡机)。
- 并行可以理解为是一个高富帅，出生就有天然的硬件优势，资源多自然办事效率就高。

#### PCB

> PCB - 进程控制块(Processing Control Block)，Linux内核的进程控制块本质上是一个叫做 `task_struct`的结构体。在这个结构体中记录了进程运行相关的一些信息，下面介绍一些常用的信息：

- 进程id：每一个进程都一个唯一的进程ID，类型为 `pid_t`, 本质是一个整形数
- 进程的状态：进程有不同的状态, 状态是一直在变化的，有就绪、运行、挂起、停止等状态。
- 进程对应的虚拟地址空间的信息。
- 描述控制终端的信息，进程在哪个终端启动默认就和哪个终端绑定。
- 当前工作目录：默认情况下, 启动进程的目录就是当前的工作目录
- `umask`掩码：在创建新文件的时候，通过这个掩码屏蔽某些用于对文件的操作权限。
- 文件描述符表：每个被分配的文件描述符都对应一个已经打开的磁盘文件
- 和信号相关的信息：在Linux中 `调用函数, 键盘快捷键, 执行shell命令`等操作都会产生信号。
	- 阻塞信号集：记录当前进程中阻塞哪些已产生的信号，使其不能被处理
- 未决信号集：记录在当前进程中产生的哪些信号还没有被处理掉。
- 用户id和组id：当前进程属于哪个用户, 属于哪个用户组
- 会话(Session)和进程组：多个进程的集合叫进程组，多个进程组的集合叫会话。
- 进程可以使用的资源上限：可以使用shell命令`ulimit -a`查看详细信息。

#### 进程状态
进程一共有五种状态分别为：`创建态，就绪态，运行态，阻塞态(挂起态)，退出态(终止态)`其中创建态和退出态维持的时间是非常短的，稍纵即逝。我们主要是需要将`就绪态, 运行态, 挂起态`，三者之间的状态切换搞明白。

<img src="https://www.subingwen.cn/linux/process/1557237111672.png" alt="image.png" style="zoom:60%;" />

- 就绪态: 万事俱备，只欠东风(`CPU资源`)
	- 进程被创建出来了，有运行的资格但是还没有运行，需要抢CPU时间片
	- 得到CPU时间片，进程开始运行，从就绪态转换为运行态。
	- 进程的CPU时间片用完了, 再次失去CPU, 从运行态转换为就绪态。
- 运行态：获取到CPU资源的进程，进程只有在这种状态下才能运行
	- 运行态不会一直持续，进程的CPU时间片用完之后, 再次失去CPU，从运行态转换为就绪态
	- 只要进程还没有退出，就会在就绪态和运行态之间不停的切换。
- 阻塞态：进程被强制放弃CPU，并且没有抢夺CPU时间片的资格
	- 比如: 在程序中调用了某些函数(比如: sleep())，进程又运行态转换为阻塞态(挂起态)
	- 当某些条件被满足了(比如：slee() 睡醒了)，进程的阻塞状态也就被解除了，进程从阻塞态转换为就绪态。
- 退出态: 进程被销毁, 占用的系统资源被释放了
	- 任何状态的进程都可以直接转换为退出态。

#### 进程命令
在研究如何创建进程之前，先来看一下如何在终端中通过命令完成进程相关的操作。

- 查看进程
	```c
	$ ps aux
		- a: 查看所有终端的信息
		- u: 查看用户相关的信息
		- x: 显示和终端无关的进程信息
	```

	<img src="https://www.subingwen.cn/linux/process/image-20210203171636845.png" alt="image.png" style="zoom:40%;" />
	
如果特别想知道每个参数控制着哪些信息, 可以通过 `ps a, ps u, ps x`分别查看。
- 杀死进程
`kill`命令可以发送某个信号到对应的进程，进程收到某些信号之后默认的处理动作就是退出进程，如果要给进程发送信号，可以先查看一下Linux给我们提供了哪些标准信号。

> 查看Linux中的标准信号:

```c
$ kill -l
 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX
```


> 9号信号(SIGKILL)的行为是无条件杀死进程，想要杀死哪个进程就可以把这个信号发送给这个进程，操作如下：

```c
# 无条件杀死进程, 进程ID通过 ps aux 可以查看
$ kill -9 进程ID
$ kill -SIGKILL 进程ID
```

<img src="https://www.subingwen.cn/linux/process/image-20210203172310788.png" alt="image.png" style="zoom:40%;" />

### 进程创建

#### 函数

> Linux中进程ID为 `pid_t` 类型，其本质是一个正整数，通过上边的`ps aux`命令已经得到了验证。PID为1的进程是Linux系统中创建的第一个进程。

- 获取当前进程的进程ID(PID)
	```c
	#include <sys/types.h>
	#include <unistd.h>
	pid_t getpid(void);
	```
- 获取当前进程的父进程 ID(PPID)
```c
#include <sys/types.h>
#include <unistd.h>
pid_t getppid(void);
```
- 创建一个新的进程
```c
#include <unistd.h>
pid_t fork(void);
```

> 小贴士:
> Linux中看似创建一个新的进程非常简单，函数连参数都没有，实际上如果想要真正理解这个函数还是得死几个脑细胞。
 
可以看[[零散知识点#fork函数详解|fork]]

> fork函数复制当前进程，在内核进程表中创建一个新的进程表项。子进程一些属性被赋予了新的值**信号位图**被清除(原进程设置的信号处理函数不再对新进程起作用)。数
> 据的复制采用的是所谓的**写时复制**(copy on writte)，即只有在任一进程(父进程或子进程)对数据执行了写操作时，复制才会发生(先是缺页中断，然后操作系统给子进程分配内存并复制父进程的数据)。


### execl和execlp函数
在项目开发过程中，有时候有这种需求，需要通过现在运行的进程启动磁盘上的另一个可执行程序，也就是通过一个进程启动另一个进程，就是替换替换进程映像，这种情况下我们可以使用 `exec族函数`，函数原型如下：

```c
#include<unistd.h>
extern char** environ;//设置新程序的环境变量
int execl(const char* path, const char* arg, ...);
int execlp(const char* file, const char* arg, ...);
int execle(const char* path, const char* arg, ..., char* const envp[]);
int execv(const char* path, const char* argv[]);
int execvp(const char* file, const char* arg[]);
int execve(const char* path, const char* arg[], char* const envp[]);
```


> 作用：将当前进程替换成另一个进程并执行
> 参数：
> `path`: 指定可执行文件的完整路径
> `file`:接收文件名,该文件的具体位置在环境遍历path中搜寻
> `arg`：接收可变参数(被传递给新程序的main）
> `argv`:接收参数数组（被传递给新程序的main）
> `envp[]`:设置新程序的环境变量，若未设置则环境变量由environ指定
> 返回值：一般不返回（这是因为当exec成功执行后，原程序的代码不会执行，返回值也就没用了）
> 失败：-1


<font color=#ff0000>这些函数执行成功后不会返回</font>，因为调用进程的实体，包括`代码段，数据段和堆栈`等都已经被新的内容取代(也就是说用户区数据基本全部被替换掉了)，只留下进程ID等一些表面上的信息仍保持原样，颇有些神似”三十六计”中的”金蝉脱壳”。看上去还是旧的躯壳，却已经注入了新的灵魂。只有`调用失败了，它们才会返回一个 -1，从原程序的调用点接着往下执行。`

> 也就是说 `exec族`函数并没有创建新进程的能力，只是有大无畏的牺牲精神，让起启动的新进程寄生到自己虚拟地址空间之内，并挖空了自己的地址空间用户区，把新启动的进程数据填充进去。

`exec族`函数中最常用的有两个`execl()和execlp()`，这两个函数是对其他4个函数做了进一步的封装，下面介绍一下。

#### execl
该函数可用于执行任意一个可执行程序，`函数需要通过指定的文件路径才能找到这个可执行程序`。
```c
#include <unistd.h>
// 变参函数
int execl(const char path, const char arg, ...);
```

- 参数:
	- `path`: 要启动的可执行程序的路径, 推荐使用绝对路径
	- `arg`: ps aux 查看进程的时候, 启动的进程的名字, 可以随意指定, 一般和要启动的可执行程序名相同
	- `...` : 要执行的命令需要的参数，可以写多个，最后以 NULL 结尾，表示参数指定完了。
- 返回值：如果这个函数执行成功, 没有返回值，如果执行失败, 返回 -1

#### execlp()

该函数常用于执行已经设置了环境变量的可执行程序，函数中的 `p`就是`path`，也是说这个函数会自动搜索系统的环境变量`PATH`，因此使用这个函数执行可执行程序不需要指定路径，只需要指定出名字即可。

```c
// p == path
int execlp(const char file, const char arg, ...);
```

- 参数:
	- `file`: 可执行程序的名字
		- 在环境变量PATH中，可执行程序可以不加路径
		- 没有在环境变量PATH中, 可执行程序需要指定绝对路径
	- `arg`: 参数代表第一个命令行参数。<font color=#ff0000>可选地</font> ps aux 查看进程的时候, 启动的进程的名字, 可以随意指定, 一般和要启动的可执行程序名相同
	- `...` : 要执行的命令需要的参数，可以写多个，最后以 NULL 结尾，表示参数指定完了。
- 返回值：如果这个函数执行成功, 没有返回值，如果执行失败, 返回 -1
#### 函数的使用
关于exec族函数，我们一般不会在进程中直接调用，如果直接调用这个进程的代码区代码被替换也就不能按照原来的流程工作了。<font color=#ff0000>我们一般在调用这些函数的时候都会先创建一个子进程，在子进程中调用 exec 族函数，子进程的用户区数据被替换掉开始执行新的程序中的代码逻辑，但是父进程不受任何影响仍然可以继续正常工作</font>。

execl() 或者 execlp() 函数的使用方法如下:

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
int main()
{
    // 创建子进程
    pid_t pid = fork();
    // 在子进程中执行磁盘上的可执行程序
    if(pid == 0)
    {
        // 磁盘上的可执行程序 /bin/ps
#if 1
        execl("/bin/ps", "title", "aux", NULL);
        // 也可以这么写
        // execl("/bin/ps", "title", "a", "u", "x", NULL);  
#else
        execlp("ps", "title", "aux", NULL);
        // 也可以这么写
        // execl("ps", "title", "a", "u", "x", NULL);
#endif
        // 如果成功当前子进程的代码区别 ps中的代码区代码替换
        // 下面的所有代码都不会执行
        // 如果函数调用失败了,才会继续执行下面的代码
        perror("execl");
        printf("++++++++++++++++++++++++\n");
        printf("++++++++++++++++++++++++\n");
        printf("++++++++++++++++++++++++\n");
        printf("++++++++++++++++++++++++\n");
        printf("++++++++++++++++++++++++\n");
        printf("++++++++++++++++++++++++\n");
    }
    else if(pid > 0)
    {
        printf("我是父进程.....\n");
    }

    return 0;
}
```

<img src="https://www.subingwen.cn/linux/process/image-20210204123455742.png" alt="image.png" style="zoom:40%;" />

### 进程控制
进程控制主要是指`进程的退出, 进程的回收`和进程的特殊状态`孤儿进程和僵尸进程`。

#### 结束进程
如果想要直接退出某个进程可以在程序的任何位置调用`exit()或者_exit()`函数。函数的参数相当于退出码, 如果参数值为 0 程序退出之后的状态码就是0, 如果是100退出的状态码就是100。

```c
// 专门退出进程的函数, 在任何位置调用都可以
// 标准C库函数
#include <stdlib.h>
void exit(int status);

// Linux的系统函数
// 可以这么理解, 在linux中 exit() 函数 封装了 _exit()
#include <unistd.h>
void _exit(int status);
```

在 main 函数中直接使用 `return`也可以退出进程, 假如是在一个普通函数中调用 return 只能返回到调用者的位置，而不能退出进程。

```c
//  return 必须要在main()函数中调用, 才能退出进程 
// 举例:
// 没有问题的例子
int main()
{
    return 0;	// 进程退出了
}

////////////////////////// 不能退出的例子 //////////////////////////

int func()
{
    return 666;	// 返回到调用者调用该函数的位置, 返回到 main() 函数的第19行
}

int main()
{
    // 调用这个函数, 当前进程能不能退出? ===> 不能
    int ret = func();
}
```

#### 孤儿进程
在一个启动的进程中创建子进程，这时候父子进程同时运行，但是父进程由于某种原因先退出了，子进程还在运行，这时候这个子进程就可以被称之为孤儿进程(跟现实是一样的)。

操作系统是非常关爱运行的每一个进程的，当检测到某一个进程变成了孤儿进程，这时候系统中就会有一个固定的进程领养这个孤儿进程(有干爹了)。如果使用Linux没有桌面终端，这个领养孤儿进程的进程就是 init 进程(PID=1)，如果有桌面终端，这个领养孤儿进程就是桌面进程。

那么问题来了，系统为什么要领养这个孤儿进程呢？<font color=#ff0000>在子进程退出的时候, 进程中的用户区可以自己释放, 但是进程内核区的pcb资源自己无法释放，必须要由父进程来释放子进程的pcb资源，孤儿进程被领养之后，这件事儿干爹就可以代劳了，这样可以避免系统资源的浪费</font>。

下面这段代码就可以得到一个孤儿进程：

```c
int main()
{
    // 创建子进程
    pid_t pid = fork();

    // 父进程
    if(pid > 0)
    {
        printf("我是父进程, pid=%d\n", getpid());
    }
    else if(pid == 0)
    {
        sleep(1);	// 强迫子进程睡眠1s, 这个期间, 父进程退出, 当前进程变成了孤儿进程
        // 子进程
        printf("我是子进程, pid=%d, 父进程ID: %d\n", getpid(), getppid());
    }
    return 0;
}
```


```shell
# 程序输出的结果
$ ./a.out 
我是父进程, pid=22459
我是子进程, pid=22460, 父进程ID: 1		# 父进程向退出, 子进程变成孤儿进程, 子进程被1号进程回收
```

#### 僵尸进程

在一个启动的进程中创建子进程，这时候就有了父子两个进程，父进程正常运行, 子进程先与父进程结束, 子进程无法释放自己的PCB资源, 需要父进程来做这个件事儿, 但是如果父进程也不管, 这时候子进程就变成了僵尸进程。

> `僵尸进程不能将它看成是一个正常的进程，这个进程已经死亡了，用户区资源已经被释放了，只是还占用着一些内核资源(PCB)`。 僵尸进程就相当于是一副已经腐烂只剩下骨头的尸体。
> 僵尸进程的出现是由于这个已死亡的进程的父进程不作为造成的。

运行下面的代码就可以得到一个僵尸进程了：

```c
int main()
{
    pid_t pid;
    // 创建子进程
    for(int i=0; i<5; ++i)
    {
        pid = fork();
        if(pid == 0)
        {
            break;
        }
    }
    // 父进程
    if(pid > 0)
    {
        // 需要保证父进程一直在运行
        // 一直运行不退出, 并且也做回收, 就会出现僵尸进程
        while(1)
        {
            printf("我是父进程, pid=%d\n", getpid());
            sleep(1);
        }
    }
    else if(pid == 0)
    {
        // 子进程, 执行这句代码之后, 子进程退出了
        printf("我是子进程, pid=%d, 父进程ID: %d\n", getpid(), getppid());
    }
    return 0;
}
```

```shell
# ps aux 查看进程信息
# Z+ --> 这个进程是僵尸进程, defunct, 表示进程已经死亡
robin     22598  0.0  0.0   4352   624 pts/2    S+   10:11   0:00 ./app
robin     22599  0.0  0.0      0     0 pts/2    Z+   10:11   0:00 [app] <defunct> # 子进程
robin     22600  0.0  0.0      0     0 pts/2    Z+   10:11   0:00 [app] <defunct> # 子进程
robin     22601  0.0  0.0      0     0 pts/2    Z+   10:11   0:00 [app] <defunct> # 子进程
robin     22602  0.0  0.0      0     0 pts/2    Z+   10:11   0:00 [app] <defunct> # 子进程
robin     22603  0.0  0.0      0     0 pts/2    Z+   10:11   0:00 [app] <defunct> # 子进程
```

> 消灭僵尸进程的方法是，杀死这个僵尸进程的父进程，这样僵尸进程的资源就被系统回收了。通过`kill -9 僵尸进程PID`的方式是不能消灭僵尸进程的，这个命令只对活着的进程有效，僵尸进程已经死了，鞭尸是不能解决问题的。

#### 进程回收
为了避免僵尸进程的产生，一般我们会在父进程中进行子进程的资源回收，回收方式有两种，一种是阻塞方式`wait()`，一种是非阻塞方式`waitpid()`。
##### wait
这是个阻塞函数，如果没有子进程退出, 函数会一直阻塞等待, 当检测到子进程退出了, 该函数阻塞解除回收子进程资源。这个函数被调用一次, 只能回收一个子进程的资源，如果有多个子进程需要资源回收, 函数需要被调用多次。

函数原型如下：

```c
// man 2 wait
#include <sys/wait.h>
pid_t wait(int status);
```

- 参数：传出参数，通过传递出的信息判断回收的进程是怎么退出的，如果不需要该信息可以指定为 NULL。取出整形变量中的数据需要使用一些宏函数，具体操作方式如下：
	- `WIFEXITED(status)`: 返回1, 进程是正常退出的
		- `WEXITSTATUS(status)`：得到进程退出时候的状态码，相当于return后边的数值, 或者exit()函数的参数
	- `WIFSIGNALED(status)`: 返回1, 进程是被信号杀死了
		- `WTERMSIG(status)`: 获得进程是被哪个信号杀死的，会得到信号的编号
- 返回值:
	- 成功：返回被回收的子进程的进程ID
	- 失败: -1
		- 没有子进程资源可以回收了, 函数的阻塞会自动解除, 返回-1
		- 回收子进程资源的时候出现了异常

下面代码演示了如何通过 wait()回收多个子进程资源：

```c
int main() {
  pid_t pid;
  // 创建子进程
  for (int i = 0; i < 5; ++i) {
    pid = fork();
    if (pid == 0) {
      break;
    }
  }

  // 父进程
  if (pid > 0) {
    // 需要保证父进程一直在运行
    // 一直运行不退出, 并且也做回收, 就会出现僵尸进程
    while (1) {

      int status;
      // 进行进程回收
      pid_t ret = wait(&status);
      if (WIFEXITED(status)) {
        printf("子进程是正常退出的");
      }
      else if (WIFSIGNALED(status)) {
       printf("子进程是被信号杀死的");
      }
      if (ret > 0) {
        printf("成功回收了子进程资源, 子进程PID: %d\n", ret);
      } else {
        printf("回收失败, 或者是已经没有子进程了...\n");
        break;
      }

      printf("我是父进程, pid=%d\n", getpid());
      sleep(1);
    }
  } else if (pid == 0) {
    // 子进程, 执行这句代码之后, 子进程退出了
    printf("我是子进程, pid=%d, 父进程ID: %d\n", getpid(), getppid());
  }
  return 0;
}
```

##### waitpid
`waitpid()` 函数可以看做是 `wait()` 函数的升级版，通过该函数可以控制回收子进程资源的方式是阻塞还是非阻塞，另外还可以通过该函数进行精准打击，可以精确指定回收某个或者某一类或者是全部子进程资源。

该函数函数原型如下：
```c
// man 2 waitpid
#include <sys/wait.h>
// 这个函数可以设置阻塞, 也可以设置为非阻塞
// 这个函数可以指定回收哪些子进程的资源
pid_t waitpid(pid_t pid, int status, int options);
```

- 参数:
	- pid:
		- `-1`：回收所有的子进程资源, 和wait()是一样的, 无差别回收，并不是一次性就可以回收多个, 也是需要循环回收的
		- `大于0`：指定回收某一个进程的资源 ，pid是要回收的子进程的进程ID
		- `0`：回收当前进程组的所有子进程ID
		- `小于 -1`：pid 的绝对值代表进程组ID，表示要回收这个进程组的所有子进程资源
	- status: NULL, 和wait的参数是一样的
	- options: 控制函数是阻塞还是非阻塞
		- `0`: 函数是行为是阻塞的 ==> 和wait一样
		- `WNOHANG`: 函数是行为是非阻塞的
- 返回值:
	- 如果函数是非阻塞的, 并且子进程还在运行, 返回0
	- 成功: 得到子进程的进程ID
	- 失败: -1
		- 没有子进程资源可以回收了, 函数如果是阻塞的, 阻塞会解除, 直接返回-1
		- 回收子进程资源的时候出现了异常

> 下面代码演示了如何通过 waitpid()阻塞回收多个子进程资源：

```c
// 和wait() 行为一样, 阻塞
#include <sys/wait.h>

int main()
{
    pid_t pid;
    // 创建子进程
    for(int i=0; i<5; ++i)
    {
        pid = fork();
        if(pid == 0)
        {
            break;
        }
    }

    // 父进程
    if(pid > 0)
    {
        // 需要保证父进程一直在运行
        while(1)
        {
            // 回收子进程的资源
            // 子进程由多个, 需要循环回收子进程资源
            int status;
            pid_t ret = waitpid(-1, &status, 0);  // == wait(NULL);
            if(ret > 0)
            {
                printf("成功回收了子进程资源, 子进程PID: %d\n", ret);
                                // 判断进程是不是正常退出
                if(WIFEXITED(status))
                {
                    printf("子进程退出时候的状态码: %d\n", WEXITSTATUS(status));
                }
                if(WIFSIGNALED(status))
                {
                    printf("子进程是被这个信号杀死的: %d\n", WTERMSIG(status));
                }
            }
            else
            {
                printf("回收失败, 或者是已经没有子进程了...\n");
                break;
            }
            printf("我是父进程, pid=%d\n", getpid());
        }
    }
    else if(pid == 0)
    {
        // 子进程, 执行这句代码之后, 子进程退出了
        printf("===我是子进程, pid=%d, 父进程ID: %d\n", getpid(), getppid());
    }
    return 0;
}
```

下面代码演示了如何通过 waitpid()非阻塞回收多个子进程资源：

```c
// 非阻塞处理
#include <sys/wait.h>

int main()
{
    pid_t pid;
    // 创建子进程
    for(int i=0; i<5; ++i)
    {
        pid = fork();
        if(pid == 0)
        {
            break;
        }
    }

    // 父进程
    if(pid > 0)
    {
        // 需要保证父进程一直在运行
        while(1)
        {
            // 回收子进程的资源
            // 子进程由多个, 需要循环回收子进程资源
            // 子进程退出了就回收, 
            // 没退出就不回收, 返回0
            int status;
            pid_t ret = waitpid(-1, &status, WNOHANG);  // 非阻塞
            if(ret > 0)
            {
                printf("成功回收了子进程资源, 子进程PID: %d\n", ret);
                // 判断进程是不是正常退出
                if(WIFEXITED(status))
                {
                    printf("子进程退出时候的状态码: %d\n", WEXITSTATUS(status));
                }
                if(WIFSIGNALED(status))
                {
                    printf("子进程是被这个信号杀死的: %d\n", WTERMSIG(status));
                }
            }
            else if(ret == 0)
            {
                printf("子进程还没有退出, 不做任何处理...\n");
            }
            else
            {
                printf("回收失败, 或者是已经没有子进程了...\n");
                break;
            }
            printf("我是父进程, pid=%d\n", getpid());
        }
    }
    else if(pid == 0)
    {
        // 子进程, 执行这句代码之后, 子进程退出了
        printf("===我是子进程, pid=%d, 父进程ID: %d\n", getpid(), getppid());
    }
    return 0;
}
```

##### 补充解决办法
waitpid调用将是非阻塞的,但是我们都知道**要在事件已经发生的情况下执行非阻塞调用才能提高程序的效率**，那么父进程从何得知某个子进程已经退出了呢？正是SIGCHLD信号的用途。当一个进程结束时，它将给其父进程发送一个SIGCHLD信号。因此，我们可以在父进程中捕获SIGCHLD信号，并在信号处理函数中调用waitpid函数以“彻底结束”一个子进程

可以使用`signal(SIGCHLD,SIG_IGN);`方法解决僵尸进程的问题，`signal(SIGCHLD, SIG_IGN)`的作用是告诉操作系统忽略 SIGCHLD 信号。如果父进程忽略了 SIGCHLD 信号，那么当子进程终止时，操作系统会自动回收子进程的资源，包括释放进程表中的相关条目和释放进程控制块等资源。这样，子进程的进程描述符和其他资源都会被操作系统正常地清理，不会导致僵尸进程的产生。

```c
static void handle_child(int sig) {
  pid_t pid;
  int stat;
  while ((pid = waitpid(-1,＆stat, WNOHANG))＞0) {
    /*对结束的子进程进行善后处理*/
  }
}
```


```c
signal(SIGCHLD, handle_child);
```


## 管道

管道的是进程间通信(IPC - InterProcess Communication)的一种方式，管道的本质其实就是<font color=#ff0000>内核中的一块内存</font>(或者叫内核缓冲区)，这块缓冲区中的数据存储在一个<font color=#ff0000>环形队列中</font>，因为管道在内核里边，因此我们不能直接对其进行任何操作。

<img src="https://www.subingwen.cn/linux/pipe/%E5%BE%AA%E7%8E%AF%E9%98%9F%E5%88%97.png" alt="image.png" style="zoom:50%;" />

因为管道数据是通过队列来维护的，我们先来分析一个管道中数据的特点：
- 管道对应的内核缓冲区大小是固定的，<font color=#ff0000>默认为4k</font>(也就是队列最大能存储4k数据)
- 管道分为两部分：读端和写端(队列的两端)，数据从写端进入管道，从读端流出管道。
- 管道中的数据只能读一次，做一次读操作之后数据也就没有了(读数据相当于出队列)。
- 管道是<font color=#ff0000>单工的</font>：数据只能单向流动, 数据从写端流向读端。
- 对管道的操作(读、写)默认是阻塞的
	- 读管道：管道中没有数据，读操作被阻塞，当管道中有数据之后阻塞才能解除
	- 写管道：管道被写满了，写数据的操作被阻塞，当管道变为不满的状态，写阻塞解除
管道在内核中, 不能直接对其进行操作，我们通过什么方式去读写管道呢？其实管道操作就是文件IO操作，内核中管道的两端分别对应两个文件描述符，通过写端的文件描述符把数据写入到管道中，通过读端的文件描述符将数据从管道中读出来。读写管道的函数就是Linux中的[[C语言#文件操作|文件IO函数]]

```c
// 读管道
ssize_t read(int fd, void buf, size_t count);
// 写管道的函数
ssize_t write(int fd, const void buf, size_t count);
```

<img src="https://www.subingwen.cn/linux/pipe/image-20210204211818071.png" alt="image.png" style="zoom:40%;" />

在上图中假设父进程通过一系列操作可以通过文件描述符表中的文件描述符fd3写管道，通过fd4读管道，然后再`通过 fork() 创建出子进程，那么在父进程中被分配的文件描述符 fd3， fd4也就被拷贝到子进程中，子进程通过 fd3可以将数据写入到内核的管道中，通过fd4将数据从管道中读出来`。

也就是说<font color=#ff0000>管道是独立于任何进程的</font>，并且充当了两个进程用于数据通信的载体，只要两个进程能够得到同一个管道的入口和出口(读端和写端的文件描述符)，那么他们之间就可以通过管道进行数据的交互。

### 匿名管道
#### 创建匿名管道
匿名管道是管道的一种，既然是匿名也就是说这个管道没有名字，但其本质是不变的，就是位于内核中的一块内存，匿名管道拥有上面介绍的管道的所有特性，额外的我们需要知道，`匿名管道只能实现有血缘关系的进程间通信`，什么叫有血缘的进程关系呢，比如：父子进程，兄弟进程，爷孙进程，叔侄进程。最后说一下创建匿名管道的函数，函数原型如下：

```c
#include <unistd.h>
// 创建一个匿名的管道, 得到两个可用的文件描述符
int pipe(int pipefd[2]);
```

- 参数：传出参数，需要传递一个整形数组的地址，数组大小为 2，也就是说最终会传出两个元素
	- `pipefd[0]`: 对应管道读端的文件描述符，通过它可以将数据从管道中读出
	- `pipefd[1]`: 对应管道写端的文件描述符，通过它可以将数据写入到管道中
- 返回值：成功返回 0，失败返回 -1

#### 进程间通信

使用匿名管道只能够实现有血缘关系的进程间通信，要求写一段程序完成下边的功能：

```c
需求描述:
   在父进程中创建一个子进程, 父子进程分别执行不同的操作:
     - 子进程: 执行一个shell命令 "ps aux", 将命令的结果传递给父进程
     - 父进程: 将子进程命令的结果输出到终端
```

```c
dup2(int fd, int fd2); 
// fd是要被复制的文件描述符，而fd2是要将其复制到的目标文件描述符。进行文件描述符重定向，而不是简单的文件复制,
// 它会关闭目标文件描述符fd2(如果已经打开)，然后将文件描述符fd复制到fd2处。这意味着fd和fd2现在都指向相同的文件或资源。
```


需求分析:
- 子进程中执行shell命令相当于启动一个磁盘程序，因此需要使用 `execl()/execlp()`函数
	- `execlp(“ps”, “ps”, “aux”, NULL)`
- 子进程中执行完shell命令直接就可以在终端输出结果，如果将这些信息传递给父进程呢？
	- 数据传递需要使用管道，子进程需要将数据写入到管道中
	- 将默认输出到终端的数据写入到管道就需要进行输出的重定向，需要使用 `dup2()`做这件事情
		- `dup2(fd[1], STDOUT_FILENO)`;
- 父进程需要读管道，将从管道中读出的数据打印到终端
- 父进程最后需要释放子进程资源，防止出现僵尸进程
<font color=#ff0000>在使用管道进行进程间通信的注意事项：必须要保证数据在管道中的单向流动</font>。这句话怎么理解呢，通过下面的图来分析一下：

> 第一步: 在父进程中创建了匿名管道，得到了两个分配的文件描述符，fd3操作管道的读端，fd4操作管道的写端。

<img src="https://www.subingwen.cn/linux/pipe/image-20210204223438269.png" alt="image.png" style="zoom:40%;" />

> 第二步：父进程创建子进程，父进程的文件描述符被拷贝，在子进程的文件描述符表中也得到了两个被分配的可以使用的文件描述符，通过fd3读管道，通过fd4写管道。通过下图可以看到管道中数据的流动不是单向的，有以下这么几种情况：
> 
> 1. 父进程通过fd4将数据写入管道，然后父进程再通过fd3将数据从管道中读出
> 2. 父进程通过fd4将数据写入管道，然后子进程再通过fd3将数据从管道中读出
> 3. 子进程通过fd4将数据写入管道，然后子进程再通过fd3将数据从管道中读出
> 4. 子进程通过fd4将数据写入管道，然后父进程再通过fd3将数据从管道中读出
> 前边说到过，管道行为默认是阻塞的，`假设子进程通过写端将数据写入管道，父进程的读端将数据读出，这样子进程的读端就读不到数据，导致子进程阻塞在读管道的操作上`，这样就会给程序的执行造成一些不必要的影响。如果我们本来也没有打算让进程读或者写管道，那么就可以将进程操作的读端或者写端关闭。

<img src="https://www.subingwen.cn/linux/pipe/image-20210204224031041.png" alt="image.png" style="zoom:30%;" />
> 第三步：为了避免两个进程都读管道，但是可能其中某个进程由于读不到数据而阻塞的情况，我们可以关闭进程中用不到的那一端的文件描述符，这样数据就只能单向的从一端流向另外一端了，如下图，我们关闭了父进程的写端，关闭了子进程的读端：

<img src="https://www.subingwen.cn/linux/pipe/image-20210204225930296.png" alt="image.png" style="zoom:40%;" />
根据上面的分析，最终可以写出下面的代码：

```c
// 管道的数据是单向流动的:
// 操作管道的是两个进程, 进程A读管道, 需要关闭管道的写端, 进程B写管道, 需要关闭管道的读端
// 如果不做上述的操作, 会对程序的结果造成一些影响, 对管道的操作无法结束
#include <fcntl.h>
#include <sys/wait.h>

int main()
{
    // 1. 创建匿名管道, 得到两个文件描述符
    int fd[2];
    int ret = pipe(fd);
    if(ret == -1)
    {
        perror("pipe");
        exit(0);
    }
    // 2. 创建子进程 -> 能够操作管道的文件描述符被复制到子进程中
    pid_t pid = fork();
    if(pid == 0)
    {
        // 关闭读端
        close(fd[0]);
        // 3. 在子进程中执行 execlp("ps", "ps", "aux", NULL);
        // 在子进程中完成输出的重定向, 原来输出到终端现在要写管道
        // 进程打印数据默认输出到终端, 终端对应的文件描述符: stdout_fileno
        // 标准输出 重定向到 管道的写端
        dup2(fd[1], STDOUT_FILENO);
        execlp("ps", "ps", "aux", NULL);
        perror("execlp");
    }

    // 4. 父进程读管道
    else if(pid > 0)
    {
        // 关闭管道的写端
        close(fd[1]);
        // 5. 父进程打印读到的数据信息
        char buf[4096];
        // 读管道
        // 如果管道中没有数据, read会阻塞
        // 有数据之后, read解除阻塞, 直接读数据
        // 需要循环读数据, 管道是有容量的, 写满之后就不写了
        // 数据被读走之后, 继续写管道, 那么就需要再继续读数据
        while(1)
        {
            memset(buf, 0, sizeof(buf));
            int len = read(fd[0], buf, sizeof(buf));
            if(len == 0)
            {
                // 管道的写端关闭了, 如果管道中没有数据, 管道读端不会阻塞
                // 没数据直接返回0, 如果有数据, 将数据读出, 数据读完之后返回0
                break;
            }
            printf("%s, len = %d\n", buf, len);
        }
        close(fd[0]);

        // 回收子进程资源
        wait(NULL);
    }
    return 0;
}
```

### 有名管道
#### 创建有名管道

有名管道拥有管道的所有特性，之所以称之为有名是因为管道在磁盘上有实体文件, 文件类型为`p` ，<font color=#ff0000>有名管道文件大小永远为0，因为有名管道也是将数据存储到内存的缓冲区中，打开这个磁盘上的管道文件就可以得到操作有名管道的文件描述符，通过文件描述符读写管道存储在内核中的数据。</font>

有名管道也可以称为 fifo (first in first out)，使用有名管道既可以进行有血缘关系的进程间通信，也可以进行没有血缘关系的进程间通信。创建有名管道的方式有两种，一种是通过命令，一种是通过函数。

- 通过命令
	```shell
	$ mkfifo 有名管道的名字
	```

- 通过函数
	```c
	#include <sys/types.h>
	#include <sys/stat.h>
	// int open(const char pathname, int flags, mode_t mode);
	int mkfifo(const char pathname, mode_t mode);
	```

- 参数:
	- `pathname`: 要创建的有名管道的名字
	- `mode`: 文件的操作权限, 和`open()`的第三个参数一个作用，最终权限: (mode & ~umask)
- 返回值：创建成功返回 0，失败返回 -1

#### 进程间通信

不管是有血缘关系还是没有血缘关系，使用有名管道实现进程间通信的方式是相同的，就是在两个进程中分别以读、写的方式打开磁盘上的管道文件，得到用于读管道、写管道的文件描述符，就可以调用对应的`read()、write()`函数进行读写操作了。

> [!tip] 小贴士：
`有名管道操作需要通过 open() 操作得到读写管道的文件描述符，如果只是读端打开了或者只是写端打开了，进程会阻塞在这里不会向下执行，直到在另一个进程中将管道的对端打开，当前进程的阻塞也就解除了。所以当发现进程阻塞在了open()函数上不要感到惊讶。` 

- 写管道的进程
	```c
	/
		1. 创建有名管道文件 
			mkfifo()
		2. 打开有名管道文件, 打开方式是 o_wronly
			int wfd = open("xx", O_WRONLY);
		3. 调用write函数写文件 ==> 数据被写入管道中
			write(wfd, data, strlen(data));
		4. 写完之后关闭文件描述符
			close(wfd);
	/
	```

	```c
	#include <fcntl.h>
	#include <sys/stat.h>
	
	int main()
	{
	    // 1. 创建有名管道文件
	    int ret = mkfifo("./testfifo", 0664);
	    if(ret == -1)
	    {
	        perror("mkfifo");
	        exit(0);
	    }
	    printf("管道文件创建成功...\n");
	
	    // 2. 打开管道文件
	    // 因为要写管道, 所有打开方式, 应该指定为 O_WRONLY
	    // 如果先打开写端, 读端还没有打开, open函数会阻塞, 当读端也打开之后, open解除阻塞
	    int wfd = open("./testfifo", O_WRONLY);
	    if(wfd == -1)
	    {
	        perror("open");
	        exit(0);
	    }
	    printf("以只写的方式打开文件成功...\n");
	
	    // 3. 循环写管道
	    int i = 0;
	    while(i<100)
	    {
	        char buf[1024];
	        sprintf(buf, "hello, fifo, 我在写管道...%d\n", i);
	        write(wfd, buf, strlen(buf));
	        i++;
	        sleep(1);
	    }
	    close(wfd);
	
	    return 0;
	}
	```


- 读管道的进程
	```c
	/
		1. 这两个进程需要操作相同的管道文件
		2. 打开有名管道文件, 打开方式是 o_rdonly
			int rfd = open("xx", O_RDONLY);
		3. 调用read函数读文件 ==> 读管道中的数据
			char buf[4096];
			read(rfd, buf, sizeof(buf));
		4. 读完之后关闭文件描述符
			close(rfd);
	/
```

	```c
	#include <fcntl.h>
	#include <sys/stat.h>
	
	int main()
	{
	    // 1. 打开管道文件
	    // 因为要read管道, so打开方式, 应该指定为 O_RDONLY
	    // 如果只打开了读端, 写端还没有打开, open阻塞, 当写端被打开, 阻塞就解除了
	    int rfd = open("./testfifo", O_RDONLY);
	    if(rfd == -1)
	    {
	        perror("open");
	        exit(0);
	    }
	    printf("以只读的方式打开文件成功...\n");
	
	    // 2. 循环读管道
	    while(1)
	    {
	        char buf[1024];
	        memset(buf, 0, sizeof(buf));
	        // 读是阻塞的, 如果管道中没有数据, read自动阻塞
	        // 有数据解除阻塞, 继续读数据
	        int len = read(rfd, buf, sizeof(buf));
	        printf("读出的数据: %s\n", buf);
	        if(len == 0)
	        {
	            // 写端关闭了, read解除阻塞返回0
	            printf("管道的写端已经关闭, 拜拜...\n");
	            break;
	        }
	    }
	    close(rfd);
	
	    return 0;
	}
	```

### 管道的读写行为

关于管道不管是有名的还是匿名的，在进行读写的时候，它们表现出的行为是一致的，下面是对其读写行为的总结:

- 读管道，需要根据写端的状态进行分析：
	- 写端没有关闭 (操作管道写端的文件描述符没有被关闭)
		- 如果管道中没有数据 ==> `读阻塞`, 如果管道中被写入了数据, `阻塞`解除
		- 如果管道中有数据 ==> `不阻塞`，管道中的数据被读完了, 再继续读管道还会`阻塞`
	- 写端已经关闭了 (没有可用的文件描述符可以写管道了)
		- 管道中没有数据 ==> 读端解除`阻塞`, read函数返回0
		- 管道中有数据 ==> read先将数据读出, 数据读完之后返回0, 不会`阻塞`了
- 写管道，需要根据读端的状态进行分析：
	- 读端没有关闭
		- 如果管道有存储的空间, 一直写数据
		- 如果管道写满了, 写操作就`阻塞`, 当读端将管道数据读走了, 解除`阻塞`继续写
	- 读端关闭了，管道破裂(异常), 进程直接退出

管道的两端默认是`阻塞`的，如何将管道设置为非`阻塞`呢？管道的读写两端的非`阻塞`操作是相同的，下面的代码中将匿名的读端设置为了非`阻塞`：

```c
// 通过fcntl 修改就可以, 一般情况下不建议修改
// 管道操作对应两个文件描述符, 分别是管道的读端 和 写端

// 1. 获取读端的文件描述符的flag属性
int flag = fcntl(fd[0], F_GETFL);
// 2. 添加非阻塞属性到 flag中
flag |= O_NONBLOCK;
// 3. 将新的flag属性设置给读端的文件描述符
fcntl(fd[0], F_SETFL, flag);
// 4. 非阻塞读管道
char buf[4096];
read(fd[0], buf, sizeof(buf));
```

## 内存映射(mmap)


创建内存映射区
如果想要实现进程间通信，可以通过函数创建一块内存映射区，和管道不同的是管道对应的内存空间在内核中，而内存映射区对应的内存空间在<font color=#ff0000>进程的用户区</font>(用于加载动态库的那个区域)，也就是说`进程间通信使用的内存映射区不是一块，而是在每个进程内部都有一块`。
  
由于每个进程的地址空间是独立的，各个进程之间也不能直接访问对方的内存映射区，需要通信的进程需要将各自的内存映射区和同一个磁盘文件进行映射，这样进程之间就可以通过磁盘文件这个唯一的桥梁完成数据的交互了。

<img src="https://www.subingwen.cn/linux/mmap/image-20210205145238501.png" alt="image.png" style="zoom:60%;" />

如上图所示：`磁盘文件数据可以完全加载到进程的内存映射区也可以部分加载到进程的内存映射区，当进程A中的内存映射区数据被修改了，数据会被自动同步到磁盘文件，同时和磁盘文件建立映射关系的其他进程内存映射区中的数据也会和磁盘文件进行数据的实时同步，这个同步机制保障了各个进程之间的数据共享`。

使用内存映射区既可以进程有血缘关系的进程间通信也可以进程没有血缘关系的进程间通信。创建内存映射区的函数原型如下：

```c
#include <sys/mman.h>
// 创建内存映射区
void mmap(void addr, size_t length, int prot, int flags, int fd, off_t offset);
```

- 参数:
	- `addr`: 从动态库加载区的什么位置开始创建内存映射区，一般指定为NULL, 委托内核分配
	- `length`: 创建的内存映射区的大小(单位：字节)，实际上这个大小是按照4k的整数倍去分配的
	- `prot`: 对内存映射区的操作权限
			- `PROT_READ`: 读内存映射区
			- `PROT_WRITE`: 写内存映射区
			- 如果要对映射区有读写权限: `PROT_READ | PROT_WRITE`
	- `flags`:
		- `MAP_SHARED`: 多个进程可以共享数据，进行映射区数据同步
		- `MAP_PRIVATE`: 映射区数据是私有的，不能同步给其他进程
	- `fd`: 文件描述符, 对应一个打开的磁盘文件，内存映射区通过这个文件描述符和磁盘文件建立关联
	- `offset`: 磁盘文件的偏移量，文件从偏移到的位置开始进行数据映射，使用这个参数需要注意两个问题：
		- 偏移量必须是4k的整数倍, 写0代表不偏移
		- 这个参数必须是大于 0 的
- 返回值:
	- 成功: 返回一个内存映射区的起始地址
	- 失败: `MAP_FAILED (that is, (void ) -1)`

> mmap() 函数的参数相对较多，在使用该函数创建用于进程间通信的内存映射区的时候，各参数的指定都有一些注意事项，具体如下：

```c
1. 第一个参数 addr 指定为 NULL 即可
2. 第二个参数 length 必须要 > 0
3. 第三个参数 prot，进程间通信需要对内存映射区有读写权限，因此需要指定为：PROT_READ | PROT_WRITE
4. 第四个参数 flags，如果要进行进程间通信, 需要指定 MAP_SHARED
5. 第五个参数 fd，打开的文件必须大于0，进程间通信需要文件操作权限和映射区操作权限相同
     - 内存映射区创建成功之后, 关闭这个文件描述符不会影响进程间通信
6. 第六个参数 offset，不偏移指定为0，如果偏移必须是4k的整数倍
```

内存映射区使用完之后也需要释放，释放函数原型如下：

```c
int munmap(void addr, size_t length);
```

- 参数:
	- `addr`: mmap()的返回值, 创建的内存映射区的起始地址
	- `length`: 和mmap()第二个参数相同即可
- 返回值：函数调用成功返回 0，失败返回 -1

对内存映射区进行写入操作，可能不能立刻同步到磁盘文件，要立刻同步，可以用下面的函数
```c
// 将内存中的数据同步到文件系统中。它通常用于确保对文件所做的修改被持久化到磁盘上，以确保数据的一致性和持久性。
int msync(void *addr, size_t length, int flags);
```


- addr是要同步到文件系统的内存区域的起始地址。
- length是要同步的内存区域的长度(以字节为单位)。
- flags是一个控制行为的标志位，通常可以是以下几种之一：
	- `MS_ASYNC`：表示异步地将内存数据刷新到磁盘上。
	- `MS_SYNC`：表示同步地将内存数据刷新到磁盘上，并阻塞调用进程直到刷新操作完成。
	- `MS_INVALIDATE`：表示刷新内存映射区域的内容，但不写回到文件中，而是标记这些内容已经失效。



> [!attention] 内存映射注意点
> - 映射不能改变文件的大小
> - 可用于进程间通信的有效地址空间不完全受限于被映射文件的大小
> - 文件一旦被映射后，所有对映射区域的访问实际上是对内存区域的访问。映射区域内容写回文件时，所写内容不能超过文件的大小。


### 进程间通信

操作内存映射区和操作管道是不一样的，得到内存映射区之后是直接对内存地址进行操作，管道是通过文件描述符读写队列中的数据，管道的读写是阻塞的，内存映射区的读写是非阻塞的。内存映射区创建成功之后，得到了映射区内存的起始地址，使用相关的内存操作函数读写数据就可以了。

#### 有血缘关系
由于创建子进程会发生虚拟地址空间的复制，那么在父进程中创建的内存映射区也会被复制到子进程中，这样在子进程里边就可以直接使用这块内存映射区了，所以对于有血缘关系的进程，进行进程间通信是非常简单的，处理代码如下：

```c
/
    1. 先创建内存映射区, 得到一个起始地址, 假设使用ptr指针保存这个地址
    2. 通过fork() 创建子进程 -> 子进程中也就有一个内存映射区, 子进程中也有一个ptr指针指向这个地址
    3. 父进程往自己的内存映射区写数据, 数据同步到了磁盘文件中, 磁盘文件数据又同步到子进程的映射区中
       子进程从自己的映射区往外读数据, 这个数据就是父进程写的
/
```

```c
#include <sys/mman.h>
#include <fcntl.h>

int main()
{
    // 1. 打开一个磁盘文件
    int fd = open("./english.txt", O_RDWR);
    // 设置文件大小为4000字节 用于更改文件的大小。它接受一个文件描述符 `fd` 和一个新的文件大小值作为参数。
	ftruncate(fd, 4000); // 必须设置不然会报段错误
    // 2. 创建内存映射区
    void ptr = mmap(NULL, 4000, PROT_READ|PROT_WRITE,
                     MAP_SHARED, fd, 0);
    if(ptr == MAP_FAILED)
    {
        perror("mmap");
        exit(0);
    }

    // 3. 创建子进程
    pid_t pid = fork();
    if(pid > 0)
    {
        // 父进程, 写数据
        const char pt = "我是你爹, 你是我儿子吗???";
        memcpy(ptr, pt, strlen(pt)+1);
    }
    else if(pid == 0)
    {
        // 子进程, 读数据
        usleep(1);	// 内存映射区不阻塞, 为了让子进程读出数据
        printf("从映射区读出的数据: %s\n", (char)ptr);
    }

    // 释放内存映射区
    munmap(ptr, 4000);

    return 0;
}
```

#### 没有血缘关系
对于没有血缘关系的进程间通信，需要在每个进程中分别创建内存映射区，但是这些进程的内存映射区必须要关联相同的磁盘文件，这样才能实现进程间的数据同步。

> 进程A的测试代码:

```c
int main()
{
  int fd=open("./text.txt", O_CREAT|O_RDWR);
  ftruncate(fd, 4000);
  void ptr=mmap(NULL, 4000, PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0);
  memcpy(ptr, "hello one", 5);
  munmap(ptr, 4000);
}
```

> 进程B的测试代码:

```c
int main() {
  int fd = open("./text.txt", O_CREAT | O_RDWR);
  void ptr = mmap(NULL, 4000, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
  printf("读到的数据为%s",(char )ptr);
  munmap(ptr, 4000);
}
```

### 拷贝文件
使用内存映射区除了可以实现进程间通信，也可以进行文件的拷贝，使用这种方式拷贝文件可以减少程序猿的工作量，我们只需要负责创建内存映射区和打开磁盘文件，关于文件中的数据读写就无需关心了。

> [!tip] 使用内存映射区拷贝文件思路：
> 1. 打开被拷贝文件，得到文件描述符 `fd1`，并计算出这个文件的大小 `size`
> 2. 创建内存映射区A并且和被拷贝文件关联，也就是和fd1关联起来，得到映射区地址 `ptrA`
> 3. 创建新文件，得到文件描述符 `fd2`，用于存储被拷贝的数据，并且将这个文件大小拓展为 `size`
> 4. 创建内存映射区B并且和新创建的文件关联，也就是和fd2关联起来，得到映射区地址 `ptrB`
> 5. 进程地址空间之间的数据拷贝，`memcpy(ptrB， ptrA，size)`，数据自动同步到新建文件中
> 6. 关闭内存映射区

文件拷贝示例代码如下：

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>
#include <sys/mman.h>

int main()
{
    // 1. 打开一个操盘文件english.txt得到文件描述符
    int fd = open("./english.txt", O_RDWR);
    // 计算文件大小
    int size = lseek(fd, 0, SEEK_END);

    // 2. 创建内存映射区和english.txt进行关联, 得到映射区起始地址
    void ptrA = mmap(NULL, size, PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0);
    if(ptrA == MAP_FAILED)
    {
        perror("mmap");
        exit(0);
    }

    // 3. 创建一个新文件, 存储拷贝的数据
    int fd1 = open("./copy.txt", O_RDWR|O_CREAT, 0664);
    // 拓展这个新文件
    ftruncate(fd1, size);

    // 4. 创建一个映射区和新文件进行关联, 得到映射区的起始地址second
    void ptrB = mmap(NULL, size, PROT_READ|PROT_WRITE, MAP_SHARED, fd1, 0);
    if(ptrB == MAP_FAILED)
    {
        perror("mmap----");
        exit(0);
    }
    // 5. 使用memcpy拷贝映射区数据
    // 这两个指针指向两块内存, 都是内存映射区
    // 指针指向有效的内存, 拷贝的是内存中的数据
    memcpy(ptrB, ptrA, size);

    // 6. 释放内存映射区
    munmap(ptrA, size);
    munmap(ptrB, size);
    close(fd);
    close(fd1);

    return 0;
}
```

## 共享内存

**共享内存是最高效的IPC机制**，因为它**不涉及进程之间的任何数据传输**。这种高效率带来的问题是，我们必须**用其他辅助手段来同步进程对共享内存的访问**，否则会产生竞态条件。因此，共享内存通常和其他
进程间通信方式一起使用。

共享内存不同于内存映射区，它不属于任何进程，并且不受进程生命周期的影响。通过调用Linux提供的系统函数就可得到这块共享内存。使用之前需要让进程和共享内存进行关联，得到共享内存的起始地址之后就可以直接进行读写操作了，进程也可以和这块共享内存解除关联, 解除关联之后就不能操作这块共享内存了。在所有进程间通信的方式中共享内存的效率是最高的。

> 共享内存操作默认不阻塞，如果多个进程同时读写共享内存，可能出现数据混乱，共享内存需要借助其他机制来保证进程间的数据同步，比如：信号量，共享内存内部没有提供这种机制。

### 创建/打开共享内存
#### shmget
在使用共享内存之前必须要先做一些准备工作，如果共享内存不存在就需要先创建出来，如果已经存在了就需要先打开这块共享内存。不管是创建还是打开共享内存使用的函数是同一个，函数原型如下:
```c
#include <sys/ipc.h>
#include <sys/shm.h>
int shmget(key_t key, size_t size, int shmflg);
```

- 参数:
	- key: 类型 key_t 是个整形数, `通过这个key可以创建或者打开一块共享内存，该参数的值一定要大于0`
	- size: 创建共享内存的时候, 指定共享内存的大小，如果是打开一块存在的共享内存, size是没有意义的
	- shmflg：创建共享内存的时候指定的属性
		- `IPC_CREAT`: 创建新的共享内存，如果创建共享内存, 需要指定对共享内存的操作权限，比如：IPC_CREAT | 0664
		- `IPC_EXCL`: 检测共享内存是否已经存在了，必须和 IPC_CREAT一起使用
- 返回值：共享内存创建或者打开成功返回标识共享内存的唯一的ID，失败返回-1
函数使用举例:
> 场景1：创建一块大小为4k的共享内存

```c
shmget(100, 4096, IPC_CREAT|0664);
```

> 场景2：创建一块大小为4k的共享内存, 并且检测是否存在

```c
// 	如果共享内存已经存在, 共享内存创建失败, 返回-1, 可以perror() 打印错误信息
shmget(100, 4096, IPC_CREAT|0664|IPC_EXCL);
```

> 场景3：打开一块已经存在的共享内存

```c
// 函数参数虽然指定了大小和IPC_CREAT, 但是都不起作用, 因为共享内存已经存在, 只能打开, 参数4096也没有意义
shmget(100, 4096, IPC_CREAT|0664);
shmget(100, 0, 0);
```

> 场景4：打开一块共享内存, 如果不存在就创建

```c
shmget(100, 4096, IPC_CREAT|0664);
```

#### ftok
`shmget() `函数的第一个参数是一个大于0的正整数，如果不想自己指定可以通过 ftok()函数直接生成这个key值。该函数的函数原型如下：

```c
// ftok函数原型
#include <sys/types.h>
#include <sys/ipc.h>

// 将两个参数作为种子, 生成一个 key_t 类型的数值
key_t ftok(const char pathname, int proj_id);
```

- 参数:
	- `pathname`: 当前操作系统中一个存在的路径
	- `proj_id`: 这个参数只用到了int中的一个字节, 传参的时候要将其作为 char 进行操作，取值范围: 1-255
- 返回值：函数调用成功返回一个可用于创建、打开共享内存的key值，调用失败返回-1

使用举例：

```c
// 根据路径生成一个key_t
key_t key = ftok("/home/robin", 'a');
// 创建或打开共享内存
shmget(key, 4096, IPC_CREATE|0664);
```

### 关联和解除关联
#### shmat
创建/打开共享内存之后还必须和共享内存进行关联，这样才能得到共享内存的起始地址，通过得到的内存地址进行数据的读写操作，关联函数的原型如下：

```c
void shmat(int shmid, const void shmaddr, int shmflg);
```

- 参数:
	- `shmid`: 要操作的共享内存的ID, 是`shmget()`函数的返回值
	- `shmaddr`: 共享内存的起始地址, 用户不知道, 需要让内核指定, 写NULL
	- `shmflg`: 和共享内存关联的对共享内存的操作权限
		- `SHM_RDONLY`: 读权限, 只能读共享内存中的数据
		- `0`: 读写权限，可以读写共享内存数据
- 返回值：关联成功，返回值共享内存的起始地址，关联失败返回 `(void ) -1`

#### shmdt
当进程不需要再操作共享内存，可以让进程和共享内存解除关联，另外如果没有执行该操作，进程退出之后，结束的进程和共享内存的关联也就自动解除了。
```c
int shmdt(const void shmaddr);
```

- 参数：shmat() 函数的返回值, 共享内存的起始地址
- 返回值：关联解除成功返回0，失败返回-1

### 删除共享内存
#### shmctl
shmctl()函数是一个多功能函数，可以设置、获取共享内存的状态也可以将共享内存标记为删除状态。`当共享内存被标记为删除状态之后，并不会马上被删除，直到所有的进程全部和共享内存解除关联，共享内存才会被删除`。因为通过shmctl()函数只是能够标记删除共享内存，所以在程序中多次调用该操作是没有关系的。

```c
// 共享内存控制函数
int shmctl(int shmid, int cmd, struct shmid_ds buf);

// 参数 struct shmid_ds 结构体原型          
struct shmid_ds {
	struct ipc_perm shm_perm;    / Ownership and permissions /
	size_t          shm_segsz;   / Size of segment (bytes) /
	time_t          shm_atime;   / Last attach time /
	time_t          shm_dtime;   / Last detach time /
	time_t          shm_ctime;   / Last change time /
	pid_t           shm_cpid;    / PID of creator /
	pid_t           shm_lpid;    / PID of last shmat(2)/shmdt(2) /
    // 引用计数, 多少个进程和共享内存进行了关联
	shmatt_t        shm_nattch;  / 记录了有多少个进程和当前共享内存进行了管联 /
	...
};
```


- 参数:
	- shmid: 要操作的共享内存的ID, 是 shmget() 函数的返回值
	- cmd: 要做的操作
		- IPC_STAT: 得到当前共享内存的状态
		- IPC_SET: 设置共享内存的状态
		- IPC_RMID: 标记共享内存要被删除了
	- buf:
		- cmd$==$IPC_STAT, 作为传出参数, 会得到共享内存的相关属性信息
		- cmd$==$IPC_SET, 作为传入参, 将用户的自定义属性设置到共享内存中
		- cmd$==$IPC_RMID, buf就没意义了, 这时候buf指定为NULL即可
- 返回值：函数调用成功返回值大于等于0，调用失败返回-1

#### 相关shell命令
使用`ipcs`添加参数`-m`可以查看系统中共享内存的详细信息

```shell
$ ipcs -m

------------ 共享内存段 --------------
键        shmid      拥有者  权限     字节     nattch     状态      
0x00000000 425984     oracle     600        524288     2          目标       
0x00000000 327681     oracle     600        524288     2          目标       
0x00000000 458754     oracle     600        524288     2          目标 	
```

使用`ipcrm`命令可以标记删除某块共享内存

```shell
# key == shmget的第一个参数
$ ipcrm -M shmkey  

# id == shmget的返回值
$ ipcrm -m shmid	
```

#### 共享内存状态

```c
// 参数 struct shmid_ds 结构体原型          
struct shmid_ds {
	struct ipc_perm shm_perm;    / Ownership and permissions /
	size_t          shm_segsz;   / Size of segment (bytes) /
	time_t          shm_atime;   / Last attach time /
	time_t          shm_dtime;   / Last detach time /
	time_t          shm_ctime;   / Last change time /
	pid_t           shm_cpid;    / PID of creator /
	pid_t           shm_lpid;    / PID of last shmat(2)/shmdt(2) /
    // 引用计数, 多少个进程和共享内存进行了关联
	shmatt_t        shm_nattch;  / 记录了有多少个进程和当前共享内存进行了管联 /
	...
};
```

通过`shmctl()`我们可以得知，共享内存的信息是存储到一个叫做`struct shmid_ds`的结构体中，其中有一个非常重要的成员叫做`shm_nattch`，在这个成员变量里边记录着当前共享内存关联的进程的个数，一般将其称之为引用计数。当共享内存被标记为删除状态，并且这个引用计数变为0之后共享内存才会被真正的被删除掉。

当共享内存被标记为删除状态之后，共享内存的状态也会发生变化，共享内存内部维护的key从一个正整数变为0，其属性从公共的变为私有的。这里的私有是指只有已经关联成功的进程才允许继续访问共享内存，不再允许新的进程和这块共享内存进行关联了。下图演示了共享内存的状态变化：

<img src="https://www.subingwen.cn/linux/shm/image-20200223112227101.png" alt="image.png" style="zoom:50%;" />
### 进程间通信
使用共享内存实现进程间通信的操作流程如下:

```c
1. 调用linux的系统API创建一块共享内存
    - 这块内存不属于任何进程, 默认进程不能对其进行操作
    
2. 准备好进程A, 和进程B, 这两个进程需要和创建的共享内存进行关联
    - 关联操作: 调用linux的 api
    - 关联成功之后, 得到了这块共享内存的起始地址
        
3. 在进程A或者进程B中对共享内存进行读写操作
    - 读内存: printf() 等;
	- 写内存: memcpy() 等;

4. 通信完成, 可以让进程A和B和共享内存解除关联
    - 解除成功, 进程A和B不能再操作共享内存了
    - 共享内存不受进程生命周期的影响的
    
5. 共享内存不在使用之后, 将其删除
    - 调用linux的api函数, 删除之后这块内存被内核回收了
```

> 写共享内存的进程代码:

```c
#include <stdio.h>
#include <sys/shm.h>
#include <string.h>

int main()
{
    // 1. 创建共享内存, 大小为4k
    int shmid = shmget(1000, 4096, IPC_CREAT|0664);
    if(shmid == -1)
    {
        perror("shmget error");
        return -1;
    }

    // 2. 当前进程和共享内存关联
    void ptr = shmat(shmid, NULL, 0);
    if(ptr == (void ) -1)
    {
        perror("shmat error");
        return -1;
    }

    // 3. 写共享内存
    const char p = "hello, world, 共享内存真香...";
    memcpy(ptr, p, strlen(p)+1);

    // 阻塞程序
    printf("按任意键继续, 删除共享内存\n");
    getchar();

    shmdt(ptr);

    // 删除共享内存
    shmctl(shmid, IPC_RMID, NULL);
    printf("共享内存已经被删除...\n");

    return 0;
}
```

> 读共享内存的进程代码:

```c
#include <stdio.h>
#include <sys/shm.h>
#include <string.h>

int main()
{
    // 1. 创建共享内存, 大小为4k
    int shmid = shmget(1000, 0, 0);
    if(shmid == -1)
    {
        perror("shmget error");
        return -1;
    }

    // 2. 当前进程和共享内存关联
    void ptr = shmat(shmid, NULL, 0);
    if(ptr == (void ) -1)
    {
        perror("shmat error");
        return -1;
    }

    // 3. 读共享内存
    printf("共享内存数据: %s\n", (char)ptr);

    // 阻塞程序
    printf("按任意键继续, 删除共享内存\n");
    getchar();

    shmdt(ptr);

    // 删除共享内存
    shmctl(shmid, IPC_RMID, NULL);
    printf("共享内存已经被删除...\n");

    return 0;
}
```

### shm和mmap的区别
`共享内存`和`内存映射区`都可以实现进程间通信，下面来分析一下二者的区别：
- 实现进程间通信的方式
	- shm: 多个进程只需要一块共享内存就够了，共享内存不属于进程，需要和进程关联才能使用
	- 内存映射区: 位于每个进程的虚拟地址空间中, 并且需要关联同一个磁盘文件才能实现进程间数据通信
- 效率:
	- shm: 直接对内存操作，效率高
	- 内存映射区: 需要内存和文件之间的数据同步，效率低
- 生命周期
	- 内存映射区：进程退出, 内存映射区也就没有了
	- shm：进程退出对共享内存没有影响，调用相关函数/命令/ 关机才能删除共享内存
- 数据的完整性 -> 突发状态下数据能不能被保存下来(比如: 突然断电)
	- 内存映射区：可以完整的保存数据, 内存映射区数据会同步到磁盘文件
	- shm：数据存储在物理内存中, 断电之后系统关闭, 内存数据也就丢失了

## system V消息队列

消息队列是在两个进程之间传递二进制块数据的一种简单有效的方式。每个数据块都有一个特定的类型,接收方可以根据类型来有选择地接收数据,而不一定像管道和命名管道那样必须以先进先出的方式接收数据。

概念可以看[[零散知识点#消息队列|这里]]

内核为每个IPC对象维护一个数据结构 
```c
struct ipc_perm {
    key_t          __key;    /* Key supplied to shmget(), semget(), or msgget() */
    uid_t          uid;      /* Owner's user ID */
    gid_t          gid;      /* Owner's group ID */
    uid_t          cuid;     /* Creator's user ID */
    gid_t          cgid;     /* Creator's group ID */
    mode_t         mode;     /* Read/write permission */
    unsigned short __seq;    /* Sequence number 用于区分具有相同键值的IPC对象。 */
};
```



```c
struct msqid_ds {
    struct ipc_perm msg_perm;     // 消息队列权限 就是ipc对象
    msgqnum_t       msg_qnum;     // 队列中的消息数
    struct msg      *msg_first;     /* ptr to first message on queue */
    struct msg      *msg_last;      /* ptr to last message on queue */
    msglen_t        msg_cbytes;   // 当前的队列的字节数 
    msglen_t        msg_qbytes;   // 队列的最大字节数 一个消息的消息的总和限制
    pid_t           msg_lspid;    // 最后发送消息的进程ID
    pid_t           msg_lrpid;    // 最后接收消息的进程ID
    time_t          msg_stime;    // 最后发送消息的时间
    time_t          msg_rtime;    // 最后接收消息的时间
    time_t          msg_ctime;    // 最后一次修改队列的时间或创建队列的时间
};
```

<img src="https://img-blog.csdnimg.cn/2020041911561241.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25pZTIzMTQ1NTA0NDE=,size_16,color_FFFFFF,t_70" alt="image.png" style="zoom:70%;" />

```c
消息队列也有管道一样的不足,就是每个消息的 最大长度是有上限的(MSGMAX),
每个消息队 列的总的字节数是有上限的(MSGMNB),系统上消息队列的总数也有一个上限(MSGMNI)
```


### msgget 函数

msgget 函数用于**创建**一个新的消息队列或**访问**一个已存在的消息队列。

```c
#include <sys/msg.h>
int msgget(key_t key, int oflag);
        // 返回：若成功则为非负标识符,若出错则为-1
```

返回值是一个整数标识符,其他三个msg函数就用它来指代该队列。它是基于指定的key产生的,而key既可以是ftok的返回值,也可以是常值`IPC_PRIVATE`(这种进程的创建方式key可以重复,就代表私有消息队列,不存在共享了)。
oflag读写权限值的组合。它还可以与`IPC_CREAT或IPC_CREAT | IPC_EXCL`按位或。
当创建一个新消息队列时,msqid_ds 结构的如下成员被初始化。
- msg_perm 结构的uid和cuid成员被设置成当前进程的有效用户ID, gid和cgid成员被设置成当前进程的有效组ID。
- oflag中的读写权限位存放在msg_perm.mode中。
- msg_qnum、msg_lspid、msg_lrpid、msg_stime和msg_rtime被置为0。
- msg_ctime被设置成当前时间。
- msg_qbytes被设置成系统限制值。


<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240317191626.png" alt="image.png" style="zoom:50%;" />
实例代码


```c
int main() {

  int msgid;
//  msgid = msgget(1234, 0666|IPC_CREAT | IPC_EXCL);
//  msgid = msgget(IPC_PRIVATE, 0666|IPC_CREAT | IPC_EXCL);
  msgid = msgget(1234, 0);

  if (msgid == -1) {
    ERR_EXIT("msgget");
  }

  printf("msgget success\n");
  printf("msgid = %d\n", msgid);


  return EXIT_SUCCESS;
}
```

### msgctl 函数
msgctl 函数提供在一个消息队列上的各种**控制操作**。
```c
#include <sys/msg.h>
int msgctl(int msqid, int cmd, struct msqid_ds *buff);
            // 返回：若成功则为0,若出错则为-1
```

msgctl 函数提供3个命令。

|    命令    |                                                      说明                                                      |
| :------: | :----------------------------------------------------------------------------------------------------------: |
| IPC_RMID |                                    从系统中删除由msqid指定的消息队列。当前在该队列上的任何消息都被丢弃。                                     |
| IPC-SET  | 给所指定的消息队列设置其msqid_ds结构的以下4个成员： msg_perm.uid、msg_perm.gid、msg_perm.mode和msg_qbytes。 它们的值来自由buff参数指向的结构中的相应成员。 |
| IPC-STAT |                                   (通过buff参数)给调用者返回与所指定消息队列对应的当前msgid_ds结构。                                   |
  
示例代码
```c
  int msgid;
  msgid = msgget(1234, 0);
  if (msgid == -1) {
    ERR_EXIT("msgget");
  }
  printf("msgget success\n");
  printf("msgid = %d\n", msgid);
  msgctl(msgid,IPC_RMID,nullptr);
```

```c
  int msgid;
  msgid = msgget(1234, 0);

  if (msgid == -1) {
    ERR_EXIT("msgget");
  }

  printf("msgget success\n");
  printf("msgid = %d\n", msgid);
  msqid_ds buf;

  msgctl(msgid,IPC_STAT,&buf);
  printf("mode = %o\n",buf.msg_perm.mode);
  printf("bytes = %ld\n",buf.__msg_cbytes);
  printf("number = %ld\n",buf.msg_qnum);
  printf("msgmnb = %ld\n",buf.msg_qbytes);
```

```c
  int msgid;
  msgid = msgget(1234, 0);

  if (msgid == -1) {
    ERR_EXIT("msgget");
  }

  msqid_ds buf{};
  msgctl(msgid,IPC_STAT,&buf);

  sscanf("600","%o",&buf.msg_perm.mode);

  msgctl(msgid,IPC_SET,&buf);
```


### msgsnd 函数
使用msgget打开一个消息队列后,我们使用msgsnd往其上**放置一个消息**。

```c
#include <sys/msg.h>
int msgsnd(int msqid, const void *ptr, size_t length, int flag);
    	            // 返回：若成功则为0,若出错则为-1
```

**参数**：
其中msqid是由msgget返回的标识符。ptr是一个结构指针,该结构具有如下的模板,它定义在`<sys/msg.h>`中
- **length**:是msgp指向的消息长度,这个长度不含保存消息类型的那个long int长整型
- **flag**:控制着当前消息队列满或到达系统上限时将要发生的事情

```c
struct msgbuf{
    long mtype;/*消息类型*/
    char mtext[512];/*消息数据*/
}
```

- flag=IPC_NOWAIT表示队列满不等待,返回EAGAIN错误。
- 消息结构在两方面受到制约。首先,它必须小于系统规定的上限值;其次,它必须以一个long int长整数开始,接收者函数将利用这个长整数确定消息的类型

### msgrcv 函数
使用msgrcv函数从某个消息队列中读出一个消息。

```c
#include <sys/msg.h>
ssize_t msgrcv(int msqid, void *ptr, size_t length, long type, int flag);
	// 返回：若成功则为读入缓冲区中数据的字节数,若出错则为-1
```

其中ptr参数指定所接收消息的存放位置。跟msgsnd一样,该指针指向紧挨在真正的消息数据之前返回的长整数类型字段,length指定了由ptr指向的缓冲区中数据部分的大小。这是该函数能返回的最大数据量。该长度不包括长整数类型字段。
type指定希望从所给定的队列中读出什么样的消息。
- 如果type为0,那就返回该队列中的第一个消息。既然每个消息队列都是作为一个FIFO(先进先出)链表维护的,因此ype为0指定返回该队列中最早的消息。
- 如果type大于0,那就返回其类型值为type的第一个消息。
- 如果tpe小于0,那就返回其类型值小于或等于type参数的绝对值的消息中类型值最小的第一个消息。

- 返回值:成功返回实际放到接收缓冲区里去的字符个数,失败返回-1

## POSIX 消息队列
功能:用来创建和访问一个消息队列
原型:
```c
mqd_t mq_open(const char *name, int oflag);
mqd_t mq_open(const char *name, int oflag, mode_t mode,struct mq_attr *attr);
```

参数
- name:某个消息队列的名字
- oflag: 与open函数类似,可以是O_RDONLY、O WRONLY.
	- O_RDWR,还可以按位或上O_CREAT、O_EXCL、
	- O_NONBLOCK等。
- mode:如果oflag指定了O_CREAT,需要设置mode。
- attr：(可选)一个指向 mq_attr结构的指针,该结构指定了消息队列的属性。如果此参数为NULL,则使用系统默认属性。
- 返回值:成功返回消息队列文件描述符;失败返回(mqd_t)-1


- 功能:关闭消息队列
- 原型
	```c
	mqd_tmq_close(mqd_t mqdes);
	```
- 参数
	- mqdes:消息队列描述符
- 返回值:成功返回0;失败返回-1


- 功能:删除消息队列
- 原型
	```c
	mqd_tmq_unlink(const thar *name);
	```
- 参数
	- name:消息队列的名字
- 返回值:成功返回0;失败返回-1


- 功能:获取/设置消息队列属性
- 原型
	```c
	mqd_tmq_getattr(mqd_t mqdes, struct mq_attr *attr);
	mqd_tmq_setattr(mqd_t mqdes, struct mq_attr *newattr, struct mq_attr *oldattr);
	```

- 参数
	- mqdes：是消息队列描述符,通常是之前通过调用`mq_open`函数获得的。
	- attr：是指向`mq_attr`结构的指针,该结构用于返回消息队列的属性。`mq_attr`结构体定义如下：

```c
struct mq_attr {
    long mq_flags;   /* 消息队列标志：例如,是否为非阻塞 */
    long mq_maxmsg;  /* 队列中允许的最大消息数量 */
    long mq_msgsize; /* 每个消息的最大字节数 */
    long mq_curmsgs; /* 队列中当前的消息数量 */
};
```
- 返回值:成功返回0;失败返回-1


- 功能:用于向 POSIX 消息队列发送消息。
- 原型
```c
#include <mqueue.h>
int mq_send(mqd_t mqdes, const char *msg_ptr, size_t msg_len, unsigned msg_prio);
```

- 参数说明：
	- `mqdes`：消息队列描述符,是通过之前调用 `mq_open` 函数成功打开一个消息队列后获得的。
	- `msg_ptr`：指向要发送的消息的指针。
	- `msg_len`：消息的长度,以字节为单位。这个长度不能超过在消息队列创建时指定的最大消息长度。
	- `msg_prio`：消息的优先级。在 POSIX 消息队列中,每个消息都有一个优先级,数值越低表示优先级越低。队列按优先级顺序(从最高到最低)排序消息；同优先级的消息则按照先进先出(FIFO)的顺序处理。

- 返回值：
	- 成功时,`mq_send` 返回 `0`。
	- 发生错误时,返回 `-1` 并设置 `errno` 来指示错误的原因。

- 常见的 `errno` 值包括：
	- `EAGAIN`：消息队列已满,且队列被设置为非阻塞模式。
	- `EBADF`：提供的消息队列描述符无效。
	- `EMSGSIZE`：`msg_len` 超过了在消息队列创建时指定的最大消息长度。
	- `EINTR`：调用在完成前被信号中断。



- 功能:用于从 POSIX 消息队列中接收消息。
- 原型
```c
#include <mqueue.h>
ssize_t mq_receive(mqd_t mqdes, char *msg_ptr, size_t msg_len, unsigned *msg_prio);
```

- 参数说明：
	- `mqdes`：消息队列描述符,是通过之前调用 `mq_open` 函数成功打开一个消息队列后获得的。
	- `msg_ptr`：指向缓冲区的指针,该缓冲区用于接收从消息队列中读取的消息。
	- `msg_len`：`msg_ptr` 指向的缓冲区的大小。这个大小应该足够大,能够容纳队列中任意消息的最大长度。
	- `msg_prio`：一个指向 `unsigned int` 的指针,用于接收读取的消息的优先级。如果这个参数不是 `NULL`,则读取的消息的优先级将被存储在 `*msg_prio` 指向的位置。

- 返回值：
	- 成功时,`mq_send` 返回 `0`。
	- 发生错误时,返回 `-1` 并设置 `errno` 来指示错误的原因。

- 常见的 `errno` 值包括：
	- `EAGAIN`：消息队列已满,且队列被设置为非阻塞模式。
	- `EBADF`：提供的消息队列描述符无效。
	- `EMSGSIZE`：`msg_len` 超过了在消息队列创建时指定的最大消息长度。
	- `EINTR`：调用在完成前被信号中断。


- 功能:允许进程注册或取消注册**接收关于消息队列状态变化**的通知。具体来说,它可以用于设置一个通知机制,当消息队列从空变为非空时(也就是说,当消息队列中出现了至少一条消息时),它会通知一个进程或线程。
- 原型
```c
#include <mqueue.h>
int mq_notify(mqd_t mqdes, const struct sigevent *notification);
```

- 参数说明：
	- `mqdes`：是消息队列描述符,是通过之前调用 `mq_open` 函数成功打开一个消息队列后获得的。
	- `notification`：是指向 `sigevent` 结构的指针,该结构指定当消息队列变为非空时如何通知进程。如果此参数为 `NULL`,则取消之前注册的任何通知。

	- `sigevent` 结构可用于指定多种通知类型,最常见的包括：
		- 发送信号(`SIGEV_SIGNAL`)：当消息到达时,向进程发送一个信号。
		- 启动线程(`SIGEV_THREAD`)：当消息到达时,启动一个新线程来处理消息。



```c
struct sigevent {
    int          sigev_notify;             // 通知类型
    int          sigev_signo;              // 信号编号
    union sigval sigev_value;              // 信号值
    void       (*sigev_notify_function)(union sigval); // 通知函数
    pthread_attr_t *sigev_notify_attributes; // 线程属性 
};
```

```c
union sigval {         /* Data passed with notification */
	int  sival_int;    /* Integer value */
	void *sival_ptr;   /* Pointer value */
};
```



- 返回值：
	- 成功时,`mq_notify` 返回 `0`。
	- 发生错误时,返回 `-1` 并设置 `errno` 来指示错误的原因。


- 任何时刻只能有一个进程可以被注册为接收某个给定队列的通知
- 当有一个消息到达某个先前为空的队列,而且已有一个进程被注册为接收该队列的通知时,只有没有任何线程阻塞在该队列的`mq_receive`调用的前提下,通知才会发出。
- 当通知被发送给它的注册进程时,其注册被撤消。进程必须再次调用`mq_notify`以重新注册飞如果需要的话),重新注册要放在从消息队列读出消息之前而不是之后。


## 守护进程

守护进程(Daemon Process)，也就是通常说的 `Daemon` 进程(精灵进程)，是 Linux 中的后台服务进程。它是一个<font color=#ff0000>生存期较长</font>的进程，通常<font color=#ff0000>独立于控制终端并且周期性地执行某种任务或等待处理某些发生的事件</font>。一般采用以d结尾的名字。

### 进程组
多个进程的集合就是进程组, 这个组中必须有一个组长, 组长就是进程组中的第一个进程，组长以外的都是普通的成员，每个进程组都有一个唯一的组ID，进程组的ID和组长的PID是一样的。

进程组中的成员是可以转移的，如果当前进程组中的成员被转移到了其他的组，或者进制中的所有进程都退出了，那么这个进程组也就不存在了。如果进程组中组长死了, 但是当前进程组中有其他进程，这个进程组还是继续存在的。下面介绍几个常用的进程组函数：

> 得到当前进程所在的进程组的组ID

```c
pid_t getpgrp(void);
```

> 获取指定的进程所在的进程组的组ID，参数 pid 就是指定的进程

```c
pid_t getpgid(pid_t pid);
```

> 将某个进程移动到其他进程组中或者创建新的进程组

```c
int setpgid(pid_t pid, pid_t pgid);
```

- 参数:
- pid: 某个进程的进程ID
- pgid: 某个进程组的组ID
	- 如果pgid对应的进程组存在，pid对应的进程会移动到这个组中, pid != pgid
	- 如果pgid对应的进程组不存在，会创建一个新的进程组, 因此要求 pid == pgid, 当前进程就是组长了
- 返回值：函数调用成功返回0，失败返回-1

使用这个函数的注意事项:

1. 调用这个函数的进程不能是组长进程, 如果是该函数调用失败，如果保证这个函数能调用成功呢?
	- 先`fork()`创建子进程, 终止父进程, 让子进程调用这个函数
2. 如果调用这个函数的进程不是进程组长, 会话创建成功
	- 这个进程会变成当前会话中的第一个进程，同时也会变成新的进程组的组长
	- 该函数调用成功之后, 当前进程就脱离了控制终端，因此不会阻塞终端
### 会话
会话(session)是由一个或多个进程组组成的，一个会话可以对应一个控制终端, 也可以没有。一个普通的进程可以调用 `setsid()` 函数使自己成为新 `session` 的领头进程(会长)，并且这个 `session` 领头进程还会被放入到一个新的进程组中。先来看一下`setsid()`函数的原型:

```c
#include <unistd.h>

// 获取某个进程所属的会话ID
pid_t getsid(pid_t pid);

// 将某个进程变成会话 =>> 得到一个守护进程
// 使用哪个进程调用这个函数, 这个进程就会变成一个会话
pid_t setsid(void);
```


> [!tip] 使用这个函数的注意事项:
> 1. 调用这个函数的进程不能是组长进程, 如果是该函数调用失败，如果保证这个函数能调用成功呢?
> 	- 先fork()创建子进程, 终止父进程, 让子进程调用这个函数
> 2. 如果调用这个函数的进程不是进程组长, 会话创建成功
> 	- 这个进程会变成当前会话中的第一个进程，同时也会变成新的进程组的组长
> 	- 该函数调用成功之后, 当前进程就脱离了控制终端，因此不会阻塞终端

### 创建守护进程
如果要创建一个守护进程，标准步骤如下，部分操作可以根据实际需求进行取舍：

1. 创建子进程, 让父进程退出
	- 因为父进程有可能是组长进程，不符合条件，也没有什么利用价值，退出即可
	- 子进程没有任何职务, 目的是让子进程最终变成一个会话, 最终就会得到守护进程

2. 通过子进程创建新的会话，调用函数 setsid()，脱离控制终端, 变成守护进程

3. 改变当前进程的工作目录 (可选项, 不是必须要做的)
	- 某些文件系统可以被卸载, 比如: U盘, 移动硬盘，进程如果在这些目录中运行，运行期间这些设备被卸载了，运行的进程也就不能正常工作了。
	- 修改当前进程的工作目录需要调用函数 `chdir()`
	```c
	int chdir(const char path); // 是一个C语言的系统调用，用于改变当前进程的工作目录。
	```

4. 重新设置文件的掩码 (可选项, 不是必须要做的)
	- 掩码：umask,在创建新文件的时候需要和掩码进行运算，去掉文件的某些权限
	- 设置掩码需要使用函数`umask()`
	```c
	mode_t umask(mode_t mask);
	```

5. 关闭/重定向文件描述符 (不做也可以, 但是建议做一下)
	- 启动一个进程, 文件描述符表中默认有三个被打开了, 对应的都是当前的终端文件
	- 因为进程通过调用 setsid() 已经脱离了当前终端, 因此关联的文件描述符也就没用了, 可以关闭
	```c
	close(STDIN_FILENO);
	close(STDOUT_FILENO);
	close(STDERR_FILENO);
	```

	- 重定向文件描述符(和关闭二选一): 改变文件描述符关联的默认文件, 让他们指向一个特殊的文件/dev/null，只要把数据扔到这个特殊的设备文件中, 数据被被销毁了

	```c
	int fd = open("/dev/null", O_RDWR);
	// 重定向之后, 这三个文件描述符就和当前终端没有任何关系了
	dup2(fd, STDIN_FILENO);
	dup2(fd, STDOUT_FILENO);
	dup2(fd, STDERR_FILENO);
	```

6. 根据实际需求在守护进程中执行某些特定的操作

### 守护进程的应用

写一个守护进程, 每隔2s获取一次系统时间, 并将得到的时间写入到磁盘文件中。


```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <signal.h>
#include <sys/time.h>
#include <time.h>

// 信号的处理动作
void writeFile(int num)
{
    // 得到系统时间
    time_t seconds = time(NULL);
    // 时间转换, 总秒数 -> 可以识别的时间字符串
    struct tm loc = localtime(&seconds);
    // sprintf();
    char curtime = asctime(loc); // 自带换行
    // 打开一个文件, 如果文件不存在, 就创建, 文件需要有追加属性
    // ./对应的是哪个目录? /home/robin
    // 0664 & ~022
    int fd = open("./time+++++++.log", O_WRONLY|O_CREAT|O_APPEND, 0664);
    write(fd, curtime, strlen(curtime));
    close(fd);
}

int main()
{
    // 1. 创建子进程, 杀死父进程
    pid_t pid = fork();
    if(pid > 0)
    {
        // 父进程
        exit(0); // kill(getpid(), 9); raise(9); abort();
    }

    // 2. 子进程, 将其变成会话, 脱离当前终端
    setsid();

    // 3. 修改进程的工作目录, 修改到一个不能被修改和删除的目录中 /home/robin
    chdir("/home/robin");

    // 4. 设置掩码, 在进程中创建文件的时候这个掩码就起作用了
    umask(022);

    // 5. 重定向和终端关联的文件描述符 -> /dev/null
    int fd = open("/dev/null", O_RDWR);
    dup2(fd, STDIN_FILENO);
    dup2(fd, STDOUT_FILENO);
    dup2(fd, STDERR_FILENO);

    // 5. 委托内核捕捉并处理将来发生的信号-SIGALRM(14)
    struct sigaction act;
    act.sa_flags = 0;
    act.sa_handler = writeFile;
    sigemptyset(&act.sa_mask);
    sigaction(SIGALRM, &act, NULL);

    // 6. 设置定时器
    struct itimerval val;
    val.it_value.tv_sec = 2;
    val.it_value.tv_usec = 0;
    val.it_interval.tv_sec = 2;
    val.it_interval.tv_usec = 0;
    setitimer(ITIMER_REAL, &val, NULL);

    while(1)
    {
        sleep(100);
    }

    return 0;
}
```


## 信号

### 信号概述

信号是由用户、系统或者进程**发送给目标进程的信息**，以通知目标进程**某个状态的改变或系统异常**,Linux中的信号是一种消息处理机制, 它本质上是一个整数，不同的信号对应不同的值，由于信号的结构简单所以天生不能携带很大的信息量，但是信号在系统中的优先级是非常高的。

在Linux中的很多常规操作中都会有相关的信号产生，先从我们最熟悉的场景说起：

- `通过键盘操作产生了信号`：用户按下Ctrl-C，这个键盘输入产生一个硬件中断，使用这个快捷键会产生信号, 这个信号会杀死对应的某个进程
- `通过shell命令产生了信号`：通过kill命令终止某一个进程，`kill -9 进程PID`
- `通过函数调用产生了信号`：如果CPU当前正在执行这个进程的代码调用，比如函数 `sleep()`，进程收到相关的信号，被迫挂起
- `通过对硬件进行非法访问产生了信号`：正在运行的程序访问了非法内存，发生段错误，进程退出。

信号也可以实现进程间通信，但是信号能传递的数据量很少，不能满足大部分需求，另外信号的优先级很高，并且它对应的处理动作是回调完成的，它会打乱程序原有的处理流程，影响到最终的处理结果。因此非常不建议使用信号进行进程间通信。

#### 信号编号

> 通过 `kill -l` 命令可以察看系统定义的信号列表:

```shell
# 执行shell命令查看信号
$ kill -l
 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX
```


|  编号   |         信号          |                                 对应事件                                 |     默认动作      |
| :---: | :-----------------: | :------------------------------------------------------------------: | :-----------: |
|   1   |       SIGHUP        |                   用户退出shell时，由该shell启动的所有进程将收到这个信号                   |     终止进程      |
|   2   |       SIGINT        |             当用户按下了<Ctrl+C>组合键时，用户终端向正在运行中的由该终端启动的程序发出此信号             |     终止进程      |
|   3   |       SIGQUIT       |           用户按下<ctrl+\>组合键时产生该信号，用户终端向正在运行中的由该终端启动的程序发出些信号            |     终止进程      |
|   4   |       SIGILL        |                           CPU检测到某进程执行了非法指令                           | 终止进程并产生core文件 |
|   5   |       SIGTRAP       |                         该信号由断点指令或其他 trap指令产生                         | 终止进程并产生core文件 |
|   6   |       SIGABRT       |                           调用abort函数时产生该信号                            | 终止进程并产生core文件 |
|   7   |       SIGBUS        |                          非法访问内存地址，包括内存对齐出错                           | 终止进程并产生core文件 |
|   8   |       SIGFPE        |             在发生致命的运算错误时发出。不仅包括浮点运算错误，还包括溢出及除数为0等所有的算法错误              | 终止进程并产生core文件 |
|   9   |       SIGKILL       |                        无条件终止进程。本信号不能被忽略，处理和阻塞                        | 终止进程，可以杀死任何进程 |
|  10   |       SIGUSE1       |                      用户定义的信号。即程序员可以在程序中定义并使用该信号                      |     终止进程      |
|  11   |       SIGSEGV       |                          指示进程进行了无效内存访问(段错误)                          | 终止进程并产生core文件 |
|  12   |       SIGUSR2       |                    另外一个用户自定义信号，程序员可以在程序中定义并使用该信号                     |     终止进程      |
|  13   |       SIGPIPE       |                       Broken pipe向一个没有读端的管道写数据                       |     终止进程      |
|  14   |       SIGALRM       |                       定时器超时，超时的时间 由系统调用alarm设置                       |     终止进程      |
|  15   |       SIGTERM       | 程序结束信号，与SIGKILL不同的是，该信号可以被阻塞和终止。通常用来要示程序正常退出。执行shell命令Kill时，缺省产生这个信号 |     终止进程      |
|  16   |      SIGSTKFLT      |                       Linux早期版本出现的信号，现仍保留向后兼容                        |     终止进程      |
|  17   |       SIGCHLD       |                          子进程结束时，父进程会收到这个信号                           |    忽略这个信号     |
|  18   |       SIGCONT       |                           如果进程已停止，则使其继续运行                            |     继续/忽略     |
|  19   |       SIGSTOP       |                        停止进程的执行。信号不能被忽略，处理和阻塞                         |     为终止进程     |
|  20   |       SIGTSTP       |                   停止终端交互进程的运行。按下<ctrl+z>组合键时发出这个信号                   |     暂停进程      |
|  21   |       SIGTTIN       |                              后台进程读终端控制台                              |     暂停进程      |
|  22   |       SIGTTOU       |                    该信号类似于SIGTTIN，在后台进程要向终端输出数据时发生                    |     暂停进程      |
|  23   |       SIGURG        |            套接字上有紧急数据时，向当前正在运行的进程发出些信号，报告有紧急数据到达。如网络带外数据到达            |     忽略该信号     |
|  24   |       SIGXCPU       |                进程执行时间超过了分配给该进程的CPU时间 ，系统产生该信号并发送给该进程                 |     终止进程      |
|  25   |       SIGXFSZ       |                             超过文件的最大长度设置                              |     终止进程      |
|  26   |      SIGVTALRM      |            虚拟时钟超时时产生该信号。类似于SIGALRM，但是该信号只计算该进程占用CPU的使用时间             |     终止进程      |
|  27   |       SGIPROF       |               类似于SIGVTALRM，它不公包括该进程占用CPU时间还包括执行系统调用时间                |     终止进程      |
|  28   |      SIGWINCH       |                              窗口变化大小时发出                               |     忽略该信号     |
|  29   |        SIGIO        |                         此信号向进程指示发出了一个异步IO事件                          |     忽略该信号     |
|  30   |       SIGPWR        |                                  关机                                  |     终止进程      |
|  31   |       SIGSYS        |                               无效的系统调用                                | 终止进程并产生core文件 |
| 34~64 | SIGRTMIN ～ SIGRTMAX |                    LINUX的实时信号，它们没有固定的含义(可以由用户自定义)                    |     终止进程      |

#### 查看信号信息
通过Linux提供的 man 文档可以查询所有信号的详细信息:
```shell
# 查看man文档的信号描述
$ man 7 signal
```

在信号描述中介绍了对产生的信号的五种默认处理动作，分别是：

1. `Term`：信号将进程终止
2. `Ign`：信号产生之后默认被忽略了
3. `Core`：信号将进程终止, 并且生成一个core文件(一般用于gdb调试)
4. `Stop`：信号会暂停进程的运行
5. `Cont`：信号会让暂停的进程继续运行

> 关于对信号的介绍有一句非常重要的描述:
> `The signals SIGKILL and SIGSTOP cannot be caught, blocked, or ignored.`
> 
> `9号信号和19号信号不能被 捕捉, 阻塞, 和 忽略`
> 
> - `9号信号: 无条件杀死进程`
> - `19号信号: 无条件暂停进程`

#### 信号的状态
Linux中的信号有三种状态，分别为：产生，未决，递达。

1. `产生`：键盘输入, 函数调用, 执行shell命令, 对硬件进行非法访问都会产生信号
2. `未决`：信号产生了, 但是这个信号还没有被处理掉, 这个期间信号的状态称之为未决状态
3. `递达`：信号被处理了(被某个进程处理掉)

<img src="https://www.subingwen.cn/linux/signal/image-20210206135036666.png" alt="image.png" style="zoom:30%;" />
### 信号相关函数

Linux中能够产生信号的函数有很多，下面介绍几个常用函数：

#### kill/raise/abort

这三个函数的功能比较类似，可以发送相关的信号给到对应的进程。

- kill 发送指定的信号到指定的进程，函数原型如下：
	```c
	#include <signal.h>
	// 给某一个进程发送一个信号
	int kill(pid_t pid, int sig);
	```

- 参数:
	- pid: 进程ID(man 文档里边写的比较详细)
	- sig: 要发送的信号

函数使用举例:
```c
// 自己杀死自己
kill(getpid(), 9);
// 子进程杀死自己的父进程
kill(getppid(), 10);
```

- raise：给当前进程发送指定的信号，函数原型如下：
```c
// 给自己发送某一个信号
#include <signal.h>
int raise(int sig);	// 参数就是要给当前进程发送的信号
```

- abort：给当前进程发送一个固定信号 (SIGABRT)，函数原型如下：
```c 
// 这是一个中断函数, 调用这个函数, 发送一个固定信号 (SIGABRT), 杀死当前进程
#include <stdlib.h>
void abort(void);
```

#### 定时器


网络程序需要处理的第三类事件是**定时事件**。服务器管理着众多定时事件，使之能在预期的时间点被触发且不影响服务器的主要逻辑，将每个定时事件分别封装成**定时器**。定时是指在一段时间之后触发某段代
码的机制。
Linux提供了三种定时方法，它们是：
- socket选项[[网络编程#端口复用|SO_RCVTIMEO]]和SO_SNDTIMEO。仅对与数据接收和发送相关的socket专用系统调用(send、sendmsg、recv、recvmsg、accept和connect)
- SIGALRM信号。
- I/O复用系统调用的超时参数。

根据下面的内容写一个3列的markdown图表,列名是系统调用 有效选项 系统调用超时后的行为



|  系统调用   |    有效选项     |              系统调用超时后的行为              |
| :-----: | :---------: | :----------------------------------: |
|  send   | SO SNDTIMEO | 返回 -1，  设置errno: EAGAIN, EWOULDBLOCK |
| sendmsg | SO SNDTIMEO | 返回 -1，  设置errno: EAGAIN, EWOULDBLOCK |
|  recv   | SO RCVTIMEO | 返回 -1，  设置errno: EAGAIN, EWOULDBLOCK |
| recvmsg | SO RCVTIMEO | 返回 -1，  设置errno: EAGAIN, EWOULDBLOCK |
| accept  | SO RCVTIMEO | 返回 -1，  设置errno: EAGAIN, EWOULDBLOCK |
| connect | SO SNDTIMEO |     返回 -1，  设置errno: EINPROGRESS     |


##### alarm
`alarm()` 函数只能进行<font color=#ff0000>单次定时</font>，定时完成发射出一个信号。

```c
#include <unistd.h>
unsigned int alarm(unsigned int seconds);
```

- 参数: 倒计时seconds秒, 倒计时完成发送一个信号 SIGALRM , 当前进程会收到这个信号，这个信号默认的处理动作是中断当前进程
- 返回值: 大于0表示倒计时还剩多少秒，返回值为0表示倒计时完成, 信号被发出

> 使用这个定时器函数, 检测一下当前计算机1s钟之内能数多少个数

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

int main()
{
    // 1. 设置一个定时器, 定时1s
    alarm(1);	// 1s之后会发出一个信号, 这个信号将中断当前进程
    int i = 0;
    while(1)
    {
        printf("%d\n", i++);
    }
    return 0;
}
```

执行上述程序的时候, 计算一下时间
```shell
# 直接通过终端输出
$ time ./a.out
real    0m1.013s		# 实际数数用的总时间
user    0m0.060s		# 用户区代码使用的时间
sys     0m0.324s		# 内核区使用的时间

real = user + sys + 消耗的时间(频率的从用户区到内核区进程切换)


# 不直接写终端, 将数据重定向到磁盘文件中
$ time ./a.out > a.txt
Alarm clock

real    0m1.002s    # 用户实际数数的时间变长了
user    0m0.740s
sys     0m0.236s
```

> 文件IO操作需要进行用户区到内核区的切换，处理方式不同，二者之间切换的频率也不同。也就是说对文件IO操作进行优化是可以提供程序的执行效率的。

##### setitimer
`setitimer ()` 函数可以进行周期性定时，每触发一次定时器就会发射出一个信号。

```c
// 这个函数可以实现周期性定时, 每个一段固定的时间, 发出一个特定的定时器信号
#include <sys/time.h>

struct itimerval {
	struct timeval it_interval; / 时间间隔 /
	struct timeval it_value;    / 第一次触发定时器的时长 /
};
// 举例: luffy有一个闹钟, 并且使用这个闹钟定时:
// 早晨7点中起床, 第一次闹钟响起时可能起不来, 之后每隔5分钟再响一次
//  - it_value: 当前设置闹钟的时间点 到 明天早晨7点 对应的总秒数
//  - it_interval: 闹钟第一次响过之后, 每隔5分钟响一次

// 这个结构体表示的是一个时间段: tv_sec + tv_usec
struct timeval {
	time_t      tv_sec;         / 秒 /
	suseconds_t tv_usec;        / 微妙 /
};

int setitimer(int which, const struct itimerval new_value, 
              struct itimerval old_value);
```


- 参数:
	- which: 定时器使用什么样的计时法则, 不同的计时法则发出的信号不同
		- `ITIMER_REAL`: 自然计时法, 最常用, 发出的信号为SIGALRM, 一般使用这个宏值，自然计时法时间 = 用户区 + 内核 + 消耗的时间(从进程的用户区到内核区切换使用的总时间)
		- `ITIMER_VIRTUAL`: 只计算程序在用户区运行使用的时间，发射的信号为 SIGVTALRM
		- `ITIMER_PROF`: 只计算内核运行使用的时间, 发出的信号为SIGPROF
	- new_value: 给定时器设置的定时信息, 传入参数
	- old_value: 上一次给定时器设置的定时信息, 传出参数，如果不需要这个信息, 指定为NULL

### 信号集
#### 阻塞/未决信号集

在PCB中有两个非常重要的信号集。一个称之为“阻塞信号集”，另一个称之为“未决信号集”。这两个信号集体现在内核中就是两张表。但是<font color=#ff0000>操作系统不允许我们直接对这两个信号集进行任何操作，而是需要自定义另外一个集合，借助信号集操作函数来对PCB中的这两个信号集进行修改</font>。

- `信号的 “未决” 是一种状态，指的是从信号的产生到信号被处理前的这一段时间`。
- `信号的 “阻塞” 是一个开关动作，指的是阻止信号被处理，但不是阻止信号产生`。

<font color=#ff0000>信号的阻塞就是让系统暂时保留信号留待以后发送</font>。由于另外有办法让系统忽略信号，所以一般情况下信号的阻塞只是暂时的，只是为了防止信号打断某些敏感的操作。

<img src="https://www.subingwen.cn/linux/signal/image-20210206135824272.png" alt="image.png" style="zoom:50%;" />

阻塞信号集和未决信号集在内核中的结构是相同的，它们都是一个整形数组(被封装过的), 一共 128 字节 (`int [32] == 1024 bit`)，1024个标志位，其中前31个标志位，每一个都对应一个Linux中的标准信号，通过标志位的值来标记当前信号在信号集中的状态。

```shell
# 上图对信号集在内核中存储的状态的描述
# 前31个信号: 1-31 , 对应 1024个标志位的前31个标志位
			信号		标志位(从低地址位 到 高地址位)
		 	  1      ->  	0
			  2             1
			  3             2
			  4             3
			 31            30	

```

- 在阻塞信号集中，描述这个信号有没有被阻塞
	- 默认情况下没有信号是被阻塞的, 因此信号对应的标志位的值为 0
	- 如果某个信号被设置为了阻塞状态, 这个信号对应的标志位 被设置为 1
- 在未决信号集中, 描述信号是否处于未决状态
	- 如果这个信号被阻塞了, 不能处理, 这个信号对应的标志位被设置为1
	- 如果这个信号的阻塞被解除了, 未决信号集中的这个信号马上就被处理了, 这个信号对应的标志位值变为0
	- 如果这个信号没有阻塞, 信号产生之后直接被处理, 因此不会在未决信号集中做任何记录

> 信号掩码：首先，程序可以指定一个“信号掩码”，它是一组信号的集合。这个集合**定义了当前线程希望阻塞（即暂时忽略）的信号**。通过这种方式，程序可以防止这些信号的默认处理行为（如终止进程）并在适当的时候主动查询或处理这些信号


#### 信号集函数

因为用户是<font color=#ff0000>不能直接操作内核</font>中的阻塞信号集和未决信号集的，必须要调用系统函数，关于阻塞信号集可以通过系统函数进行读写操作，未决信号集只能对其进行读操作。

先来看一下读/写阻塞信号集的函数

```c
#include <signal.h>
// 使用这个函数修改内核中的阻塞信号集
// sigset_t 被封装之后得到的数据类型, 原型:int[32], 里边一共有1024给标志位, 每一个信号对应一个标志位
int sigprocmask(int how, const sigset_t set, sigset_t oldset);
```

- 参数:
	- how:
		- `SIG_BLOCK`: 将参数set集合中的数据追加到阻塞信号集中
		- `SIG_UNBLOCK`: 将参数set集合中的信号在阻塞信号集中解除阻塞
		- `SIG_SETMASK`: 使用参set结合中的数据覆盖内核的阻塞信号集数据
	- `oldset`: 通过这个参数将设置之前的阻塞信号集数据传出，如果不需要可以指定为NULL
	- 返回值：函数调用成功返回0，调用失败返回-1

`sigprocmask()` 函数有一个 sigset_t 类型的参数，对这种类型的数据进行初始化需要调用一些相关的操作函数：

```c
#include <signal.h>
// 如果在程序中读写 sigset_t 类型的变量
// 阻塞信号集和未决信号集都存储在 sigset_t 类型的变量中, 这个变量对应一块内存
// 阻塞信号集和未决信号集, 对应的内存中有1024bit = 128字节

// 将set集合中所有的标志位设置为0
int sigemptyset(sigset_t set);
// 将set集合中所有的标志位设置为1
int sigfillset(sigset_t set);
// 将set集合中某一个信号(signum)对应的标志位设置为1
int sigaddset(sigset_t set, int signum);
// 将set集合中某一个信号(signum)对应的标志位设置为0
int sigdelset(sigset_t set, int signum);
// 判断某个信号在集合中对应的标志位到底是0还是1, 如果是0返回0, 如果是1返回1
int sigismember(const sigset_t set, int signum);
```

<img src="https://www.subingwen.cn/linux/signal/image-20210206142122901.png" alt="image.png" style="zoom:40%;" />
未决信号集不需要程序猿修改, 如果设置了某个信号阻塞, 当这个信号产生之后, 内核会将这个信号的未决状态记录到未决信号集中，当阻塞的信号被解除阻塞, 未决信号集中的信号随之被处理, 内核再次修改未决信号集将该信号的状态修改为递达状态(标志位置0)。因此，写未决信号集的动作都是内核做的，这是一个读未决信号集的操作函数：

```c
#include <signal.h>
// 这个函数的参数是传出参数, 传出的内核未决信号集的拷贝
// 读一下这个集合就指定哪个信号是未决状态
int sigpending(sigset_t set);
```

下面举一个简单的例子，演示一下信号集操作函数的使用：
```text
需求: 
在阻塞信号集中设置某些信号阻塞, 通过一些操作产生这些信号, 然后读未决信号集, 最后再解除这些信号的阻塞
假设阻塞这些信号: 
  - 2号信号: SIGINT: ctrl+c
  - 3号信号: SIGQUIT: ctrl+\
  - 9号信号: SIGKILL: 通过shell命令给进程发送这个信号 kill -9 PID
```

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <signal.h>

int main()
{
    // 1. 初始化信号集
    sigset_t myset;
    sigemptyset(&myset);
    // 设置阻塞的信号
    sigaddset(&myset, SIGINT);  // 2
    sigaddset(&myset, SIGQUIT); // 3
    sigaddset(&myset, SIGKILL); // 9 测试不能被阻塞

    // 2. 将初始化的信号集中的数据设置给内核
    sigset_t old;
    sigprocmask(SIG_BLOCK, &myset, &old);

    // 3. 让进程一直运行, 在当前进程中产生对应的信号
    int i = 0;
    while(1)
    {
        // 4. 读内核的未决信号集
        sigset_t curset;
        sigpending(&curset);
        // 遍历这个信号集
        for(int i=1; i<32; ++i)
        {
            int ret = sigismember(&curset, i);
            printf("%d", ret);
        }
        printf("\n");
        sleep(1);
        i++;
        if(i==10)
        {
            // 解除阻塞, 重新设置阻塞信号集
            //sigprocmask(SIG_UNBLOCK, &myset, NULL);
            sigprocmask(SIG_SETMASK, &old, NULL);
        }
    }
    return 0;
}
```

通过测试最终得到结论：<font color=#ff0000>程序中对 9 号信号的阻塞是无效的，因为它无法被阻塞</font>。

最后通过一张图总结一下这些信号集操作函数之间的关系:

<img src="https://www.subingwen.cn/linux/signal/image-20210206181522522.png" alt="image.png" style="zoom:30%;" />

### 信号捕捉

Linux中的每个信号产生之后都会有对应的<font color=#ff0000>默认处理行为</font>，如果想要忽略这个信号或者修改某些信号的默认行为就需要在程序中捕捉该信号。程序中进行信号捕捉可以看做是一个<font color=#ff0000>注册的动作</font>，提前告诉应用程序信号产生之后做什么样的处理，当进程中对应的信号产生了，这个处理动作也就被调用了。

#### signal

使用 `signal()` 函数可以捕捉进程中产生的信号，并且修改捕捉到的函数的行为，这个信号的自定义处理动作是一个回调函数，内核通过 signal() 得到这个回调函数的地址，在信号产生之后该函数会被内核调用。

```c
#include <signal.h>
// 在程序中什么时候产生信号, 程序猿是不知道的, 因此不能在信号产生之后再去处理
// 在信号产生之前, 提供一个注册函数, 用来捕捉信号
//	  - 假设在将来这个信号产生了, 就委托内核进行捕捉, 这个信号的默认动作就不能被执行
//	  - 执行什么样的处理动作 ==> 在signal函数中指定的处理动作
//	  - 如果这个信号不产生, 回调函数永远不会被调用
sighandler_t signal(int signum, sighandler_t handler);   
```


- 参数:
	- `signum`: 需要捕捉的信号
	- `handler`: 信号捕捉到之后的处理动作, 这是一个函数指针, 函数原型

	```c
	typedef void (sighandler_t)(int);
	```

<font color=#ff0000>这个回调函数是需要程序猿写的, 但是程序猿不调用, 由内核调用，内核调用回调函数的时候, 会给它传递一个实参，这个实参的值就是捕捉的那个信号值。</font>

下面的测试程序中使用`signal()`函数来捕捉定时器产生的信号 SIGALRM：

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/time.h>
#include <signal.h>

// 定时器信号的处理动作
void doing(int arg)
{
    printf("当前捕捉到的信号是: %d\n", arg);
    // 打印当前的时间
}

int main()
{
    // 注册要捕捉哪一个信号, 执行什么样的处理动作
    signal(SIGALRM, doing);
    // 1. 调用定时器函数设置定时器函数
    struct itimerval newact;
    // 3s之后发出第一个定时器信号, 之后每隔1s发出一个定时器信号
    newact.it_value.tv_sec = 3;
    newact.it_value.tv_usec = 0;
    newact.it_interval.tv_sec = 1;
    newact.it_interval.tv_usec = 0;
    // 这个函数也不是阻塞函数, 函数调用成功, 倒计时开始
    // 倒计时过程中程序是继续运行的
    setitimer(ITIMER_REAL, &newact, NULL);

    // 编写一个业务处理, 阻止当前进程自己结束, 让当前进程被发出的信号杀死
    while(1)
    {
        sleep(1000000);
    }

    return 0;
}
```

#### sigaction
`sigaction()`函数和`signal()`函数的功能是一样的，用于捕捉进程中产生的信号，并将用户自定义的信号行为函数(回调函数)注册给内核，内核在信号产生之后调用这个处理动作。`sigaction()`可以看做是`signal()`函数是加强版，函数参数更多更复杂，函数功能也更强一些。函数原型如下：

```c
#include <signal.h>
int sigaction(int signum, const struct sigaction act, struct sigaction oldact);
```

- 参数:
	- `signum:` 要捕捉的信号
	- `act`: 捕捉到信号之后的处理动作
	- `oldact`: 上一次调用该函数进行信号捕捉设置的信号处理动作, 该参数一般指定为NULL
- 返回值：函数调用成功返回0，失败返回-1
该函数的参数是一个结构体类型，结构体原型如下：

```c
struct sigaction {
	void     (sa_handler)(int);    // 指向一个函数(回调函数)
	void     (sa_sigaction)(int, siginfo_t , void );
	sigset_t   sa_mask;             // 初始化为空即可, 处理函数执行期间不屏蔽任何信号
	int        sa_flags;	        // 0
	void     (sa_restorer)(void);  //不用
};
```

结构体成员介绍：
- sa_handler: 函数指针，指向的函数就是捕捉到的信号的处理动作
- sa_sigaction: 函数指针，指向的函数就是捕捉到的信号的处理动作
- sa_mask: `在信号处理函数执行期间, 临时屏蔽某些信号`, 将要屏蔽的信号设置到集合中即可
	- 当前处理函数执行完毕, 临时屏蔽自动解除
	- 假设在这个集合中不屏蔽任何信号, 默认也会屏蔽一个(捕捉的信号是谁, 就临时屏蔽谁)
- sa_flags：使用哪个函数指针指向的函数处理捕捉到的信号
	- 0：使用 sa_handler (一般情况下使用这个)
	- `SA_SIGINFO`：使用 sa_sigaction (使用信号传递数据$==$进程间通信)
- `sa_restorer`: 被废弃的成员

示例代码，通过`sigaction()`捕捉阻塞信号集中解除阻塞的信号，如果捕捉多个信号，可以给不同的信号添加不同的处理动作，代码中的处理动作只有一个：

```c 
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <signal.h>

// 信号的处理动作
void callback(int num)
{
    printf("当前捕捉的信号: %d\n", num);
}

int main()
{
    // 1. 初始化信号集
    sigset_t myset;
    sigemptyset(&myset);
    // 设置阻塞的信号
    sigaddset(&myset, SIGINT);  // 2
    sigaddset(&myset, SIGQUIT); // 3
    sigaddset(&myset, SIGKILL); // 9 测试不能被阻塞

    // 当阻塞的信号被解除阻塞, 该信号就可以被捕捉到了
    // 如果信号被捕捉到之后, 马上就被处理掉了 --> 递达状态
    struct sigaction act;
    act.sa_handler = callback;
    act.sa_flags = 0;
    sigemptyset(&act.sa_mask);
    sigaction(SIGINT, &act, NULL);
    // 和sigint的处理动作相同
    sigaction(SIGQUIT, &act, NULL);
    sigaction(SIGKILL, &act, NULL);

    // 2. 将初始化的信号集中的数据设置给内核
    sigset_t old;
    sigprocmask(SIG_BLOCK, &myset, &old);

    // 3. 让进程一直运行, 在当前进程中产生对应的信号
    int i = 0;
    while(1)
    {
        // 4. 读内核的未决信号集
        sigset_t curset;
        sigpending(&curset);
        // 遍历这个信号集
        for(int i=1; i<32; ++i)
        {
            int ret = sigismember(&curset, i);
            printf("%d", ret);
        }
        printf("\n");
        sleep(1);
        i++;
        if(i==10)
        {
            // 解除阻塞, 重新设置阻塞信号集
            //sigprocmask(SIG_UNBLOCK, &myset, NULL);
            sigprocmask(SIG_SETMASK, &old, NULL);
        }
    }
    return 0;
}
```

> 通过测试最终得到结论：<font color=#ff0000>程序中对 9 号信号的捕捉是无效的，因为它无法被捕捉。</font>


### SIGCHLD 信号
当子进程退出、暂停、从暂停回复运行的时候，在子进程中会产生一个`SIGCHLD`信号，并将其发送给父进程，但是父进程收到这个信号之后默认就忽略了。我们可以在父进程中对这个信号加以利用，基于这个信号来回收子进程的资源，因此需要在父进程中捕捉子进程发送过来的这个信号。

下面是基于信号回收子进程资源的示例代码

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>
#include <signal.h>

// 回收子进程处理函数
void recycle(int num)
{
    printf("捕捉到的信号是: %d\n", num);
    // 子进程的资源回收, 非阻塞
    // SIGCHLD信号17号信号, 1-31号信号不支持排队
    // 如果这些信号同时产生多个, 最终处理的时候只处理一次
    // 假设多个子进程同时退出, 父进程同时收到了多个sigchld信号
    // 父进程只会处理一次这个信号, 因此当前函数被调用了一次, waitpid被调用一次
    // 相当于只回收了一个子进程, 但是是同时死了多个子进程, 因此就出现了僵尸进程
    // 解决方案: 循环回收即可
    while(1)
    {
        // 如果是阻塞回收, 就回不到另外一个处理逻辑上去了
        pid_t pid = waitpid(-1, NULL, WNOHANG);
        if(pid > 0)
        {
            printf("child died, pid = %d\n", pid);
        }
        else if(pid == 0)
        {
            // 没有死亡的子进程, 直接退出当前循环
            break;
        }
        else if(pid == -1)
        {
            printf("所有子进程都回收完毕了, 拜拜...\n");
            break;
        }
    }
}


int main()
{
    // 设置sigchld信号阻塞
    sigset_t myset;
    sigemptyset(&myset);
    sigaddset(&myset, SIGCHLD);
    sigprocmask(SIG_BLOCK, &myset, NULL);

    // 循环创建多个子进程 - 20
    pid_t pid;
    for(int i=0; i<20; ++i)
    {
        pid = fork();
        if(pid == 0)
        {
            break;
        }
    }

    if(pid == 0)
    {
        printf("我是子进程, pid = %d\n", getpid());
    }
    else if(pid > 0)
    {
        printf("我是父进程, pid = %d\n", getpid());
        // 注册信号捕捉, 捕捉sigchld
        struct sigaction act;
        act.sa_flags  =0;
        act.sa_handler = recycle;
        sigemptyset(&act.sa_mask);
        // 注册信号捕捉, 委托内核处理将来产生的信号
        // 当信号产生之后, 当前进程优先处理信号, 之前的处理动作会暂停
        // 信号处理完毕之后, 回到原来的暂停的位置继续运行
        sigaction(SIGCHLD, &act, NULL);

        // 解除sigcld信号的阻塞
        // 信号被阻塞之后,就捕捉不到了, 解除阻塞之后才能捕捉到这个信号
        sigprocmask(SIG_UNBLOCK, &myset, NULL);

        // 父进程执行其他业务逻辑就可以了
        // 默认父进程执行这个while循环, 但是信号产生了, 这个执行逻辑或强迫暂停
        // 	父进程去处理信号的处理函数
        while(1)
        {
            sleep(100);
        }
    }
    return 0;
}
```


## 服务器并发

### 单线程/进程
在TCP通信过程中，服务器端启动之后可以同时和多个客户端建立连接，并进行网络通信，但是在介绍[[网络编程|TCP通信流程]]的时候，提供的服务器代码却不能完成这样的需求，先简单的看一下之前的服务器代码的处理思路，再来分析代码中的弊端：

```c
// server.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <arpa/inet.h>

int main()
{
    // 1. 创建监听的套接字
    int lfd = socket(AF_INET, SOCK_STREAM, 0);
    // 2. 将socket()返回值和本地的IP端口绑定到一起
    struct sockaddr_in addr;
    addr.sin_family = AF_INET;
    addr.sin_port = htons(10000);   // 大端端口
    // INADDR_ANY代表本机的所有IP, 假设有三个网卡就有三个IP地址
    // 这个宏可以代表任意一个IP地址
    addr.sin_addr.s_addr = INADDR_ANY;  // 这个宏的值为0 == 0.0.0.0
    int ret = bind(lfd, (struct sockaddr)&addr, sizeof(addr));
    // 3. 设置监听
    ret = listen(lfd, 128);
    // 4. 阻塞等待并接受客户端连接
    struct sockaddr_in cliaddr;
    int clilen = sizeof(cliaddr);
    int cfd = accept(lfd, (struct sockaddr)&cliaddr, &clilen);
    // 5. 和客户端通信
    while(1)
    {
        // 接收数据
        char buf[1024];
        memset(buf, 0, sizeof(buf));
        int len = read(cfd, buf, sizeof(buf));
        if(len > 0)
        {
            printf("客户端say: %s\n", buf);
            write(cfd, buf, len);
        }
        else if(len  == 0)
        {
            printf("客户端断开了连接...\n");
            break;
        }
        else
        {
            perror("read");
            break;
        }
    }
    close(cfd);
    close(lfd);
    return 0;
}
```

在上面的代码中用到了三个会引起程序阻塞的函数，分别是：

- `accept()`：如果服务器端没有新客户端连接，阻塞当前进程/线程，如果检测到新连接解除阻塞，建立连接
- `read()`：如果通信的套接字对应的读缓冲区没有数据，阻塞当前进程/线程，检测到数据解除阻塞，接收数据
- `write()`：如果通信的套接字写缓冲区被写满了，阻塞当前进程/线程(这种情况比较少见)

如果需要和发起新的连接请求的客户端建立连接，那么就必须在服务器端通过一个循环调用`accept()`函数，另外已经和服务器建立连接的客户端需要和服务器通信，发送数据时的阻塞可以忽略，当接收不到数据时程序也会被阻塞，这时候就会非常矛盾，被`accept()`阻塞就无法通信，被`read()`阻塞就无法和客户端建立新连接。因此得出一个结论，基于上述处理方式，在单线程/单进程场景下，服务器是无法处理多连接的，解决方案也有很多，常用的有三种：

1. 使用多线程实现
2. 使用多进程实现
3. 使用IO多路转接(复用)实现
4. 使用IO多路转接 + 多线程实现

### 多进程并发

如果要编写多进程版的并发服务器程序，首先要考虑，创建出的多个进程都是什么角色，这样就可以在程序中对号入座了。在Tcp服务器端一共有两个角色，分别是：监听和通信，<font color=#ff0000>监听是一个持续的动作</font>，如果有新连接就建立连接，如果没有新连接就阻塞。关于通信是需要和多个客户端同时进行的，因此需要多个进程，这样才能达到互不影响的效果。进程也有两大类：父进程和子进程，通过分析我们可以这样分配进程：

- 父进程：
	- 负责监听，处理客户端的连接请求，也就是在父进程中循环调用`accept()`函数
	- 创建子进程：建立一个新的连接，就创建一个新的子进程，让这个子进程和对应的客户端通信
	- 回收子进程资源：子进程退出回收其内核PCB资源，防止出现僵尸进程
- 子进程：负责通信，基于父进程建立新连接之后得到的文件描述符，和对应的客户端完成数据的接收和发送。
	- 发送数据：`send() / write()`
	- 接收数据：`recv() / read()`
在多进程版的服务器端程序中，多个进程是有血缘关系，对应有血缘关系的进程来说，还需要想明白他们有哪些资源是可以被继承的，哪些资源是独占的，以及一些其他细节：

- 子进程是父进程的拷贝，在子进程的内核区PCB中，文件描述符也是可以被拷贝的，因此在父进程可以使用的文件描述符在子进程中也有一份，并且可以使用它们做和父进程一样的事情。
- 父子进程有用各自的独立的虚拟地址空间，因此所有的资源都是独占的
- 为了节省系统资源，对于只有在父进程才能用到的资源，可以在子进程中将其释放掉，父进程亦如此。
- 由于需要在父进程中做`accept()`操作，并且要释放子进程资源，如果想要更高效一下可以使用信号的方式处理

<img src="https://www.subingwen.cn/linux/concurrence/1558162823677.png" alt="image.png" style="zoom:80%;" />

多进程版并发TCP服务器示例代码如下：
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <arpa/inet.h>
#include <signal.h>
#include <sys/wait.h>
#include <errno.h>

// 信号处理函数
void callback(int num)
{
    while(1)
    {
        pid_t pid = waitpid(-1, NULL, WNOHANG);
        if(pid <= 0)
        {
            printf("子进程正在运行, 或者子进程被回收完毕了\n");
            break;
        }
        printf("child die, pid = %d\n", pid);
    }
}

int childWork(int cfd);
int main()
{
    // 1. 创建监听的套接字
    int lfd = socket(AF_INET, SOCK_STREAM, 0);
    if(lfd == -1)
    {
        perror("socket");
        exit(0);
    }

    // 2. 将socket()返回值和本地的IP端口绑定到一起
    struct sockaddr_in addr;
    addr.sin_family = AF_INET;
    addr.sin_port = htons(10000);   // 大端端口
    // INADDR_ANY代表本机的所有IP, 假设有三个网卡就有三个IP地址
    // 这个宏可以代表任意一个IP地址
    // 这个宏一般用于本地的绑定操作
    addr.sin_addr.s_addr = INADDR_ANY;  // 这个宏的值为0 == 0.0.0.0
    // inet_pton(AF_INET, "192.168.237.131", &addr.sin_addr.s_addr);
    int ret = bind(lfd, (struct sockaddr)&addr, sizeof(addr));
    if(ret == -1)
    {
        perror("bind");
        exit(0);
    }

    // 3. 设置监听
    ret = listen(lfd, 128);
    if(ret == -1)
    {
        perror("listen");
        exit(0);
    }

    // 注册信号的捕捉
    struct sigaction act;
    act.sa_flags = 0;
    act.sa_handler = callback;
    sigemptyset(&act.sa_mask);
    sigaction(SIGCHLD, &act, NULL);

    // 接受多个客户端连接, 对需要循环调用 accept
    while(1)
    {
        // 4. 阻塞等待并接受客户端连接
        struct sockaddr_in cliaddr;
        int clilen = sizeof(cliaddr);
        int cfd = accept(lfd, (struct sockaddr)&cliaddr, &clilen);
        if(cfd == -1)
        {
            if(errno == EINTR)
            {
                // accept调用被信号中断了, 解除阻塞, 返回了-1
                // 重新调用一次accept
                continue;
            }
            perror("accept");
            exit(0);
 
        }
        // 打印客户端的地址信息
        char ip[24] = {0};
        printf("客户端的IP地址: %s, 端口: %d\n",
               inet_ntop(AF_INET, &cliaddr.sin_addr.s_addr, ip, sizeof(ip)),
               ntohs(cliaddr.sin_port));
        // 新的连接已经建立了, 创建子进程, 让子进程和这个客户端通信
        pid_t pid = fork();
        if(pid == 0)
        {
            // 子进程 -> 和客户端通信
            // 通信的文件描述符cfd被拷贝到子进程中
            // 子进程不负责监听
            close(lfd);
            while(1)
            {
                int ret = childWork(cfd);
                if(ret <=0)
                {
                    break;
                }
            }
            // 退出子进程
            close(cfd);
            exit(0);
        }
        else if(pid > 0)
        {
            // 父进程不和客户端通信
            close(cfd);
        }
    }
    return 0;
}


// 5. 和客户端通信
int childWork(int cfd)
{

    // 接收数据
    char buf[1024];
    memset(buf, 0, sizeof(buf));
    int len = read(cfd, buf, sizeof(buf));
    if(len > 0)
    {
        printf("客户端say: %s\n", buf);
        write(cfd, buf, len);
    }
    else if(len  == 0)
    {
        printf("客户端断开了连接...\n");
    }
    else
    {
        perror("read");
    }

    return len;
}
```


在上面的示例代码中，父子进程中分别关掉了用不到的文件描述符(父进程不需要通信，子进程也不需要监听)。如果客户端主动断开连接，那么服务器端负责和客户端通信的子进程也就退出了，子进程退出之后会给父进程发送一个叫做`SIGCHLD`的信号，在父进程中通过`sigaction()`函数捕捉了该信号，通过回调函数`callback()`中的`waitpid()`对退出的子进程进行了资源回收。

另外还有一个细节要说明一下，这是父进程的处理代码：

```c
int cfd = accept(lfd, (struct sockaddr)&cliaddr, &clilen);
while(1)
{
        int cfd = accept(lfd, (struct sockaddr)&cliaddr, &clilen);
        if(cfd == -1)
        {
            if(errno == EINTR)
            {
                // accept调用被信号中断了, 解除阻塞, 返回了-1
                // 重新调用一次accept
                continue;
            }
            perror("accept");
            exit(0);
 
        }
 }
```

如果父进程调用`accept()`函数没有检测到新的客户端连接，父进程就阻塞在这儿了，这时候有子进程退出了，发送信号给父进程，父进程就捕捉到了这个信号`SIGCHLD`， 由于信号的优先级很高，会打断代码正常的执行流程，因此父进程的阻塞被中断，转而去处理这个信号对应的函数`callback()`，处理完毕，再次回到`accept()`位置，但是这是已经无法阻塞了，函数直接返回-1，此时函数调用失败，错误描述为`accept: Interrupted system call`，对应的错误号为`EINTR`，由于代码是被信号中断导致的错误，所以可以在程序中对这个错误号进行判断，让父进程重新调用`accept()`，继续阻塞或者接受客户端的新连接。


- 直接开销
	- 切换页表全局目录（PGD）
	- 切换内核态堆栈
	- 切换硬件上下文（进程恢复前，必须装入寄存器的数据统称为硬件上下文）
	- 刷新 TLB
	- 系统调度器的代码执行
- 间接开销
	- CPU 缓存失效导致的进程需要到内存直接访问的 IO 操作变多


# 线程

## 线程概述
先从概念上了解一下线程和进程之间的区别：
 
- 进程有自己独立的地址空间, 多个线程共用同一个地址空间
	- 线程更加节省系统资源, 效率不仅可以保持的, 而且能够更高
	- 在一个地址空间中多个线程独享: 每个线程都有属于自己的栈区, 寄存器(内核中管理的)
	- 在一个地址空间中多个线程共享: 代码段, 堆区, 全局数据区, 打开的文件(文件描述符表)都是线程共享的
- 线程是程序的最小执行单位, 进程是操作系统中最小的资源分配单位
	- 每个进程对应一个虚拟地址空间，一个进程只能抢一个CPU时间片
	- 一个地址空间中可以划分出多个线程, 在有效的资源基础上, 能够抢更多的CPU时间片

	<img src="https://www.subingwen.cn/linux/thread/1048430-20170710134655212-558296442.png" alt="image.png" style="zoom:60%;" />

- CPU的调度和切换: 线程的上下文切换比进程要快的多
	- 上下文切换：进程/线程分时复用CPU时间片，在切换之前会将上一个任务的状态进行保存, 下次切换回这个任务的时候, 加载这个状态继续运行，任务从保存到再次加载这个过程就是一次上下文切换。
	- 线程更加廉价, 启动速度更快, 退出也快, 对系统资源的冲击小。

<font color=#ff0000>在处理多任务程序的时候使用多线程比使用多进程要更有优势，但是线程并不是越多越好</font>，如何控制线程的个数呢？
1. 文件IO操作：文件IO对CPU是使用率不高, 因此可以分时复用CPU时间片, 线程的个数 = 2×CPU核心数 (效率最高)
2. 处理复杂的算法(主要是CPU进行运算, 压力大)，线程的个数 = CPU的核心数 (效率最高)

- 进程：资源分配的基本单位
- 线程：调度的基本单位
- 无论是线程还是进程，在 linux 中都以 task_struct 描述，从内核角度看，与进程无本质区别
- Glibc 中的 pthread 库提供 NPTL（Native POSIX Threading Library）支持

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241017170306.png" alt="image.png" style="zoom:60%;" />



## 创建线程
### 线程函数
每一个线程都有一个唯一的线程ID，ID类型为`pthread_t`，这个ID是一个<font color=#ff0000>无符号长整形数</font>，如果想要得到当前线程的线程ID，可以调用如下函数

```c
pthread_t pthread_self(void); // 返回当前线程的id
```

在一个进程中调用线程创建函数，就可得到一个子线程，和进程不同，需要给每一个创建出的线程指定一个处理函数，否则这个线程无法工作。

```c
#include <pthread.h>
int pthread_create(pthread_t thread, const pthread_attr_t attr,
                   void (start_routine) (void ), void arg);
// Compile and link with -pthread, 线程库的名字叫pthread, 全名: libpthread.so libptread.a
```

- 参数:
	- `thread`: 传出参数，是无符号长整形数，线程创建成功, 会将线程ID写入到这个指针指向的内存中
	- `attr`: 线程的属性, 一般情况下使用默认属性即可, 写NULL
	- `start_routine`: 函数指针，创建出的子线程的处理动作，也就是该函数在子线程中执行。
	- `arg`: 作为实参传递到 `start_routine` 指针指向的函数内部,一般将主线程栈内存传递给子线程
- 返回值：线程创建成功返回0，创建失败返回对应的错误号


初始化与销毁属性
- **初始化属性对象：**
    `int pthread_attr_init(pthread_attr_t *attr);`
    此函数用于初始化线程属性对象，`attr` 指向将被初始化的线程属性对象。成功时返回0，失败时返回错误码。
- **销毁属性对象：**    
    `int pthread_attr_destroy(pthread_attr_t *attr);`
    此函数用于销毁线程属性对象，`attr` 指向将被销毁的线程属性对象。成功时返回0，失败时返回错误码。

获取与设置分离属性
- **获取分离状态：**
    `int pthread_attr_getdetachstate(const pthread_attr_t *attr, int *detachstate);`
    此函数用于获取线程的分离状态属性，`attr` 指向线程属性对象，`detachstate` 指向的变量将被设置为线程的当前分离状态(`PTHREAD_CREATE_DETACHED` 或 `PTHREAD_CREATE_JOINABLE`)。成功时返回0，失败时返回错误码。
- **设置分离状态：**
    `int pthread_attr_setdetachstate(pthread_attr_t *attr, int detachstate);`
    此函数用于设置线程的分离状态属性，`attr` 指向线程属性对象，`detachstate` 是要设置的分离状态。成功时返回0，失败时返回错误码。

获取与设置栈大小
- **设置栈大小：**    
    `int pthread_attr_setstacksize(pthread_attr_t *attr, size_t stacksize);`
    此函数用于设置线程的栈大小属性，`attr` 指向线程属性对象，`stacksize` 是要设置的栈大小。成功时返回0，失败时返回错误码。
- **获取栈大小：**
    `int pthread_attr_getstacksize(const pthread_attr_t *attr, size_t *stacksize);`
    此函数用于获取线程的栈大小属性，`attr` 指向线程属性对象，`stacksize` 指向的变量将被设置为线程的当前栈大小。成功时返回0，失败时返回错误码。


获取与设置线程竞争范围
- **获取线程竞争范围：**    
    `int pthread_attr_getscope(const pthread_attr_t *attr, int *contentionscope);`
    该函数用于获取线程的竞争范围属性。`attr` 是指向线程属性对象的指针，而 `contentionscope` 是指向整数的指针，在函数调用成功后，这个整数将被设置为当前的竞争范围(通常是 `PTHREAD_SCOPE_SYSTEM` 或 `PTHREAD_SCOPE_PROCESS`)。成功时返回0，失败时返回错误码。
- **设置线程竞争范围：**
    `int pthread_attr_setscope(pthread_attr_t *attr, int contentionscope);`
    此函数用于设置线程的竞争范围属性。`attr` 是指向线程属性对象的指针，`contentionscope` 是要设置的竞争范围值。成功时返回0，失败时返回错误码。

获取与设置调度策略
- **获取线程调度策略：**
    `int pthread_attr_getschedpolicy(const pthread_attr_t *attr, int *policy);`
    此函数用于获取线程的调度策略属性。`attr` 是指向线程属性对象的指针，`policy` 是指向整数的指针，在函数调用成功后，这个整数将被设置为当前的调度策略(例如 `SCHED_FIFO`, `SCHED_RR`, `SCHED_OTHER` 等)。成功时返回0，失败时返回错误码。
- **设置线程调度策略：**
    `int pthread_attr_setschedpolicy(pthread_attr_t *attr, int policy);`
    此函数用于设置线程的调度策略属性。`attr` 是指向线程属性对象的指针，`policy` 是要设置的调度策略值。成功时返回0，失败时返回错误码。

获取与设置继承的调度策略
- **获取继承的调度策略：**
    `int pthread_attr_getinheritsched(const pthread_attr_t *attr, int *inheritsched);`
    该函数用于获取线程的继承调度策略属性。`attr` 是指向线程属性对象的指针，而 `inheritsched` 是指向整数的指针，在函数调用成功后，这个整数将被设置为当前的继承调度策略(通常是 `PTHREAD_INHERIT_SCHED` 或 `PTHREAD_EXPLICIT_SCHED`)。成功时返回0，失败时返回错误码。
- **设置继承的调度策略：**
    `int pthread_attr_setinheritsched(pthread_attr_t *attr, int inheritsched);`
    此函数用于设置线程的继承调度策略属性。`attr` 是指向线程属性对象的指针，`inheritsched` 是要设置的继承调度策略。成功时返回0，失败时返回错误码。
    
获取与设置调度参数
- **获取调度参数：**
    `int pthread_attr_getschedparam(const pthread_attr_t *attr, struct sched_param *param);`
    此函数用于获取线程的调度参数。`attr` 是指向线程属性对象的指针，`param` 是指向 `sched_param` 结构体的指针，该结构体包含了线程优先级等调度参数。在函数调用成功后，`param` 结构体将被填充为当前的调度参数。成功时返回0，失败时返回错误码。
- **设置调度参数：**
    `int pthread_attr_setschedparam(pthread_attr_t *attr, const struct sched_param *param);`
    此函数用于设置线程的调度参数。`attr` 是指向线程属性对象的指针，`param` 是指向包含新调度参数的 `sched_param` 结构体的指针。成功时返回0，失败时返回错误码。

设置并发级别
- **设置并发级别：**
    `int pthread_setconcurrency(int new_level);`
    此函数用于设置线程库的并发级别，`new_level` 参数表示新的并发级别。这个级别表示了内核应该为用户线程提供的并行度。例如，如果设置为 `N`，则内核**可能**会尝试创建至少 `N` 个内核线程来映射用户线程。成功时返回0，失败时返回错误码。

获取并发级别
- **获取并发级别：**
    `int pthread_getconcurrency(void);`
    此函数用于获取线程库的当前并发级别，它返回当前的并发级别。这个级别表示了内核为用户线程提供的并行度。成功时返回当前的并发级别，失败时返回-1。

```c
  // 1. 定义线程属性变量并初始化

  pthread_attr_t attr;
  pthread_attr_init(&attr);

  // 2.获取线程分离属性的默认值

  int state;
  pthread_attr_getdetachstate(&attr, &state);
  if (state == PTHREAD_CREATE_JOINABLE) {
    cout << "PTHREAD_CREATE_JOINABLE" << endl;
  } else if (state == PTHREAD_CREATE_DETACHED) {
    cout << "PTHREAD_CREATE_DETACHED" << endl;
  }

  // 3.获取线程栈大小属性的默认值

  size_t size;
  pthread_attr_getstacksize(&attr, &size);
  printf("stack size=%zu\n", size);

  // 4.获取线程属性中的作用范围(scope)
  int scope;
  pthread_attr_getscope(&attr, &scope);
  if (scope == PTHREAD_SCOPE_SYSTEM) {// 系统级线程
    cout << "PTHREAD_SCOPE_SYSTEM" << endl;
  } else if (scope == PTHREAD_SCOPE_PROCESS) {// 进程级线程
    cout << "PTHREAD_SCOPE_PROCESS" << endl;
  }

  // 4. 获取线程属性中的调度策略

  int policy;
  pthread_attr_getschedpolicy(&attr, &policy);
  if (policy == SCHED_FIFO) {// 先进先出
    cout << "SCHED_FIFO" << endl;
  } else if (policy == SCHED_RR) {// 轮转
    cout << "SCHED_RR" << endl;
  } else if (policy == SCHED_OTHER) {// 其他
    cout << "SCHED_OTHER" << endl;
  }


  // 5. 获取线程的继承调度策略属性

  int inherit;
  pthread_attr_getinheritsched(&attr, &inherit);

  if (inherit == PTHREAD_INHERIT_SCHED) {
    cout << "PTHREAD_INHERIT_SCHED" << endl;// 继承调度策略
  } else if (inherit == PTHREAD_EXPLICIT_SCHED) {
    cout << "PTHREAD_EXPLICIT_SCHED" << endl;// 显式调度策略
  }

  // 6. 获取线程调度参数

  sched_param sched{};
  pthread_attr_getschedparam(&attr, &sched);
  printf("sched.sched_priority=%d\n", sched.sched_priority);
  int level = pthread_getconcurrency(); // 获取并发级别 
  printf("level=%d\n", level);
```


### 创建线程
下面是创建线程的示例代码，在创建过程中一定要保证编写的线程函数与规定的函数指针类型一致：`void (start_routine) (void )`

```c
// pthread_create.c 
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>

// 子线程的处理代码
void working(void arg)
{
    printf("我是子线程, 线程ID: %ld\n", pthread_self());
    for(int i=0; i<9; ++i)
    {
        printf("child == i: = %d\n", i);
    }
    return NULL;
}

int main()
{
    // 1. 创建一个子线程
    pthread_t tid;
    pthread_create(&tid, NULL, working, NULL);

    printf("子线程创建成功, 线程ID: %ld\n", tid);
    // 2. 子线程不会执行下边的代码, 主线程执行
    printf("我是主线程, 线程ID: %ld\n", pthread_self());
    for(int i=0; i<3; ++i)
    {
        printf("i = %d\n", i);
    }
    
    // 休息, 休息一会儿...
    // sleep(1);
    
    return 0;
}
```

```shell
# pthread_create 函数的定义在某一个库中, 编译的时候需要加库名 pthread
$ gcc pthread_create.c -lpthread
$ ./a.out 
子线程创建成功, 线程ID: 139712560109312
我是主线程, 线程ID: 139712568477440
i = 0
i = 1
i = 2
```

在打印的日志输出中为什么子线程处理函数没有执行完毕呢(只看到了子线程的部分日志输出)？
主线程一直在运行, 执行期间创建出了子线程，说明主线程有CPU时间片, 在这个时间片内将代码执行完毕了, 主线程就退出了。`子线程被创建出来之后需要抢cpu时间片, 抢不到就不能运行，如果主线程退出了, 虚拟地址空间就被释放了, 子线程就一并被销毁了。但是如果某一个子线程退出了, 主线程仍在运行, 虚拟地址空间依旧存在`。
得到的结论：在没有人为干预的情况下，虚拟地址空间的生命周期和主线程是一样的，与子线程无关。
目前的解决方案: 让子线程执行完毕, 主线程再退出, 可以在主线程中添加挂起函数 `sleep()`;


## 线程退出
在编写多线程程序的时候，如果想要让线程退出，但是不会导致虚拟地址空间的释放(针对于主线程)，我们就可以调用线程库中的线程退出函数，`只要调用该函数当前线程就马上退出了，并且不会影响到其他线程的正常运行，不管是在子线程或者主线程中都可以使用`。

```c
#include <pthread.h>
void pthread_exit(void retval);
```

- 参数: 线程退出的时候携带的数据，当前子线程的主线程会得到该数据。如果不需要使用，指定为NULL

> 下面是线程退出的示例代码，可以在任意线程的需要的位置调用该函数：

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>

// 子线程的处理代码
void working(void arg)
{
    sleep(1);
    printf("我是子线程, 线程ID: %ld\n", pthread_self());
    for(int i=0; i<9; ++i)
    {
        if(i==6)
        {
            pthread_exit(NULL);	// 直接退出子线程
        } 
        printf("child == i: = %d\n", i);
    }
    return NULL;
}

int main()
{
    // 1. 创建一个子线程
    pthread_t tid;
    pthread_create(&tid, NULL, working, NULL);

    printf("子线程创建成功, 线程ID: %ld\n", tid);
    // 2. 子线程不会执行下边的代码, 主线程执行
    printf("我是主线程, 线程ID: %ld\n", pthread_self());
    for(int i=0; i<3; ++i)
    {
        printf("i = %d\n", i);
    }

    // 主线程调用退出函数退出, 地址空间不会被释放
    pthread_exit(NULL);
    
    return 0;
}
```


## 线程回收
### 线程函数
线程和进程一样，子线程退出的时候其内核资源主要由主线程回收，线程库中提供的线程回收函叫做`pthread_join()`，这个函数是一个阻塞函数，<font color=#ff0000>如果还有子线程在运行，调用该函数就会阻塞，子线程退出函数解除阻塞进行资源的回收，函数被调用一次，只能回收一个子线程，如果有多个子线程则需要循环进行回收</font>。

另外通过线程回收函数还可以获取到子线程退出时传递出来的数据，函数原型如下：

```c
#include <pthread.h>
// 这是一个阻塞函数, 主线程在运行这个函数就阻塞
// 子线程退出, 函数解除阻塞, 回收对应的子线程资源, 类似于回收进程使用的函数 wait()
int pthread_join(pthread_t thread, void retval);
```

- 参数:
	- `thread`: 要被回收的子线程的线程ID
	- `retval`: 二级指针, 指向一级指针的地址, 是一个传出参数, 这个地址中存储了`pthread_exit()`传递出的数据，如果不需要这个参数，可以指定为NULL
- 返回值：线程回收成功返回0，回收失败返回错误号。

### 回收子线程数据
在子线程退出的时候可以使用`pthread_exit()`的参数将数据传出，在回收这个子线程的时候可以通过`phread_join()`的第二个参数来接收子线程传递出的数据。接收数据有很多种处理方式，下面来列举几种：

#### 使用子线程栈
通过函数`pthread_exit(void retval)`;可以得知，子线程退出的时候，需要将数据记录到一块内存中，通过<font color=#ff0000>参数传出的是存储数据的内存的地址，而不是具体数据</font>，由因为参数是`void`类型，所有这个万能指针可以指向任意类型的内存地址。先来看第一种方式，将子线程退出数据保存在子线程自己的栈区：

```c
// pthread_join.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>

// 定义结构
struct Persion
{
    int id;
    char name[36];
    int age;
};

// 子线程的处理代码
void working(void arg)
{
    printf("我是子线程, 线程ID: %ld\n", pthread_self());
    for(int i=0; i<9; ++i)
    {
        printf("child == i: = %d\n", i);
        if(i == 6)
        {
            struct Persion p;
            p.age  =12;
            strcpy(p.name, "tom");
            p.id = 100;
            // 该函数的参数将这个地址传递给了主线程的pthread_join()
            pthread_exit(&p);
        }
    }
    return NULL;	// 代码执行不到这个位置就退出了
}

int main()
{
    // 1. 创建一个子线程
    pthread_t tid;
    pthread_create(&tid, NULL, working, NULL);

    printf("子线程创建成功, 线程ID: %ld\n", tid);
    // 2. 子线程不会执行下边的代码, 主线程执行
    printf("我是主线程, 线程ID: %ld\n", pthread_self());
    for(int i=0; i<3; ++i)
    {
        printf("i = %d\n", i);
    }

    // 阻塞等待子线程退出
    void ptr = NULL;
    // ptr是一个传出参数, 在函数内部让这个指针指向一块有效内存
    // 这个内存地址就是pthread_exit() 参数指向的内存
    pthread_join(tid, &ptr);
    // 打印信息
    struct Persion pp = (struct Persion)ptr;
    printf("子线程返回数据: name: %s, age: %d, id: %d\n", pp->name, pp->age, pp->id);
    printf("子线程资源被成功回收...\n");
    return 0;
}
```

> 通过打印的日志可以发现，在主线程中没有没有得到子线程返回的数据信息，具体原因是这样的：
> 
> `如果多个线程共用同一个虚拟地址空间，每个线程在栈区都有一块属于自己的内存，相当于栈区被这几个线程平分了，当线程退出，线程在栈区的内存也就被回收了，因此随着子线程的退出，写入到栈区的数据也就被释放了`。

#### 使用全局变量
位于同一虚拟地址空间中的线程，虽然不能`共享栈区数据，但是可以共享全局数据区和堆区数据`，因此在子线程退出的时候可以将传出数据存储到全局变量、静态变量或者堆内存中。在下面的例子中将数据存储到了全局变量中：
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>

// 定义结构
struct Persion
{
    int id;
    char name[36];
    int age;
};

struct Persion p;	// 定义全局变量

// 子线程的处理代码
void working(void arg)
{
    printf("我是子线程, 线程ID: %ld\n", pthread_self());
    for(int i=0; i<9; ++i)
    {
        printf("child == i: = %d\n", i);
        if(i == 6)
        {
            // 使用全局变量
            p.age  =12;
            strcpy(p.name, "tom");
            p.id = 100;
            // 该函数的参数将这个地址传递给了主线程的pthread_join()
            pthread_exit(&p);
        }
    }
    return NULL;
}

int main()
{
    // 1. 创建一个子线程
    pthread_t tid;
    pthread_create(&tid, NULL, working, NULL);

    printf("子线程创建成功, 线程ID: %ld\n", tid);
    // 2. 子线程不会执行下边的代码, 主线程执行
    printf("我是主线程, 线程ID: %ld\n", pthread_self());
    for(int i=0; i<3; ++i)
    {
        printf("i = %d\n", i);
    }

    // 阻塞等待子线程退出
    void ptr = NULL;
    // ptr是一个传出参数, 在函数内部让这个指针指向一块有效内存
    // 这个内存地址就是pthread_exit() 参数指向的内存
    pthread_join(tid, &ptr);
    // 打印信息
    struct Persion pp = (struct Persion)ptr;
    printf("name: %s, age: %d, id: %d\n", pp->name, pp->age, pp->id);
    printf("子线程资源被成功回收...\n");
    
    return 0;
}
```

#### 使用主线程栈
虽然每个线程都有属于自己的栈区空间，`但是位于同一个地址空间的多个线程是可以相互访问对方的栈空间上的数据的`。由于很多情况下还需要在主线程中回收子线程资源，所以主线程一般都是最后退出，基于这个原因在下面的程序中将子线程返回的数据保存到了主线程的栈区内存中：

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>

// 定义结构
struct Persion
{
    int id;
    char name[36];
    int age;
};


// 子线程的处理代码
void working(void arg)
{
    struct Persion p = (struct Persion)arg;
    printf("我是子线程, 线程ID: %ld\n", pthread_self());
    for(int i=0; i<9; ++i)
    {
        printf("child == i: = %d\n", i);
        if(i == 6)
        {
            // 使用主线程的栈内存
            p->age  =12;
            strcpy(p->name, "tom");
            p->id = 100;
            // 该函数的参数将这个地址传递给了主线程的pthread_join()
            pthread_exit(p);
        }
    }
    return NULL;
}

int main()
{
    // 1. 创建一个子线程
    pthread_t tid;

    struct Persion p;
    // 主线程的栈内存传递给子线程
    pthread_create(&tid, NULL, working, &p);

    printf("子线程创建成功, 线程ID: %ld\n", tid);
    // 2. 子线程不会执行下边的代码, 主线程执行
    printf("我是主线程, 线程ID: %ld\n", pthread_self());
    for(int i=0; i<3; ++i)
    {
        printf("i = %d\n", i);
    }

    // 阻塞等待子线程退出
    void ptr = NULL;
    // ptr是一个传出参数, 在函数内部让这个指针指向一块有效内存
    // 这个内存地址就是pthread_exit() 参数指向的内存
    pthread_join(tid, &ptr);
    // 打印信息
    printf("name: %s, age: %d, id: %d\n", p.name, p.age, p.id);
    printf("子线程资源被成功回收...\n");
    
    return 0;
}
```

在上面的程序中，调用`pthread_create()`创建子线程，并将主线程中栈空间变量`p`的地址传递到了子线程中，在子线程中将要传递出的数据写入到了这块内存中。也就是说在程序的`main()`函数中，通过指针变量`ptr`或者通过结构体变量p都可以读出子线程传出的数据。

## 线程分离
在某些情况下，程序中的主线程有属于自己的业务处理流程，如果让主线程负责子线程的资源回收，调用`pthread_join()`只要子线程不退出主线程就会一直被阻塞，主要线程的任务也就不能被执行了。

在线程库函数中为我们提供了线程分离函数`pthread_detach()`，调用这个函数之后指定的子线程就可以和主线程分离，当`子线程退出的时候，其占用的内核资源就被系统的其他进程接管并回收了`。线程分离之后在主线程中使用`pthread_join()`就回收不到子线程资源了。

```c
#include <pthread.h>
// 参数就子线程的线程ID, 主线程就可以和这个子线程分离了
int pthread_detach(pthread_t thread);
```

下面的代码中，在主线程中创建子线程，并调用线程分离函数，实现了主线程和子线程的分离：

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>

// 子线程的处理代码
void working(void arg)
{
    printf("我是子线程, 线程ID: %ld\n", pthread_self());
    for(int i=0; i<9; ++i)
    {
        printf("child == i: = %d\n", i);
    }
    return NULL;
}

int main()
{
    // 1. 创建一个子线程
    pthread_t tid;
    pthread_create(&tid, NULL, working, NULL);

    printf("子线程创建成功, 线程ID: %ld\n", tid);
    // 2. 子线程不会执行下边的代码, 主线程执行
    printf("我是主线程, 线程ID: %ld\n", pthread_self());
    for(int i=0; i<3; ++i)
    {
        printf("i = %d\n", i);
    }

    // 设置子线程和主线程分离
    pthread_detach(tid);

    // 让主线程自己退出即可
    pthread_exit(NULL);
    return 0;
}
```


## 其他线程函数


### 线程特定数据

线程特定数据(Thread-Specific Data，TSD)是一种线程**局部存储机制**，允许每个线程拥有自己的**独立变量副本**。这意味着在多线程环境中，每个线程都可以访问和修改自己的变量副本，而不会影响其他线程的副本。线程特定数据提供了一种在多线程程序中共享数据的方式，同时保持数据在每个线程中的独立性。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240320112337.png?" alt="image.png" style="zoom:60%;" />

**pthread_key_create**

`int pthread_key_create(pthread_key_t *key, void (*destructor)(void *));`
- **功能**：创建一个新的线程特定数据键(key)，并为每个线程关联一个与该键关联的值。
- **参数**：
    - `key`：指向 pthread_key_t 类型的指针，用于存储创建的线程特定数据键的标识符。
    - `destructor`：一个函数指针，用于指定在线程结束时释放与键相关联的数据的函数，如果不需要释放资源，可设为 NULL。
- **返回值**：成功返回0，失败返回相应的错误码。

**pthread_key_delete**

`int pthread_key_delete(pthread_key_t key);`
- **功能**：删除一个先前创建的线程特定数据键，释放其相关联的资源。
- **参数**：
    - `key`：要删除的线程特定数据键的标识符。
- **返回值**：成功返回0，失败返回相应的错误码。

**pthread_getspecific**

`void *pthread_getspecific(pthread_key_t key);`
- **功能**：获取当前线程关联于指定键的线程特定数据的值。
- **参数**：
    - `key`：要获取其关联值的线程特定数据键的标识符。
- **返回值**：返回当前线程关联于指定键的线程特定数据的值。

**pthread_setspecific**

`int pthread_setspecific(pthread_key_t key, const void *value);`=
- **功能**：设置当前线程关联于指定键的线程特定数据的值。
- **参数**：
    - `key`：要设置其关联值的线程特定数据键的标识符。
    - `value`：要设置的线程特定数据的值。
- **返回值**：成功返回0，失败返回相应的错误码。

**pthread_once**

`int pthread_once(pthread_once_t *once_control, void (*init_routine)(void));`
- **功能**：确保某个函数在多线程环境中只会被执行一次，通常用于初始化。
- **参数**：
    - `once_control`：用于控制初始化函数是否被调用的控制变量。
    - `init_routine`：初始化函数的函数指针。
- **返回值**：成功返回0，失败返回相应的错误码。

`pthread_once_t once_control = PTHREAD_ONCE_INIT;` 这行代码用于初始化 `once_control` 变量为 `PTHREAD_ONCE_INIT`，这是一个宏定义，用于初始化 `pthread_once_t` 类型的控制变量，表示初始化只执行一次。


```c
pthread_key_t key_tsd;
pthread_once_t once_control = PTHREAD_ONCE_INIT;
struct tsd {
  pthread_t tid;
  char *str;
};


void destroy_tsd(void *value) {
  cout << "destroy_tsd" << endl;
  free(value);
}

void once_routine() {
  pthread_key_create(&key_tsd, destroy_tsd);
  cout << "once_routine" << endl;
}

void *thread_routine(void *arg) {
  pthread_once(&once_control,once_routine);
  tsd *buf = (tsd *) malloc(20);
  buf->tid = pthread_self();
  buf->str = (char *) arg;
  pthread_setspecific(key_tsd, buf);
  printf("%s setspecific %p\n", (char *) arg, buf);
  buf = (tsd *) pthread_getspecific(key_tsd);
  printf("%s getspecific %p\n", (char *) arg, buf);
  sleep(2);
  buf = (tsd *) pthread_getspecific(key_tsd);
  printf("%s getspecific %p\n", (char *) arg, buf);
  return nullptr;
}




int main() {


  // 1.创建一个key
  //  pthread_key_create(&key_tsd, destroy_tsd);

  pthread_t tid1;
  pthread_t tid2;
  pthread_create(&tid1, nullptr, thread_routine, (void *) "thread1");
  pthread_create(&tid2, nullptr, thread_routine, (void *) "thread2");

  pthread_join(tid1, nullptr);
  pthread_join(tid2, nullptr);
  pthread_key_delete(key_tsd);

  return EXIT_SUCCESS;
}
```



### 线程取消
线程取消的意思就是在某些特定情况下在一个线程中杀死另一个线程。使用这个函数杀死一个线程需要分两步：
1. 在线程A中调用线程取消函数`pthread_cancel`，指定杀死线程B，这时候线程B是死不了的
2. 在线程B中进行一次系统调用(从用户区切换到内核区)，否则线程B可以一直运行。

这其实和`七步断肠散、含笑半步癫`的功效是一样的，吃了毒药不动或者不笑也没啥事儿

```c
#include <pthread.h>
// 参数是子线程的线程ID
int pthread_cancel(pthread_t thread);
```

- 参数：要杀死的线程的线程ID
- 返回值：函数调用成功返回0，调用失败返回非0错误号。

在下面的示例代码中，主线程调用线程取消函数，只要在子线程中进行了<font color=#ff0000>系统调用</font>，当子线程执行到这个位置就挂掉了。

```c++
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>

// 子线程的处理代码
void working(void arg)
{
    int j=0;
    for(int i=0; i<9; ++i)
    {
        j++;
    }
    // 这个函数会调用系统函数, 因此这是个间接的系统调用
    printf("我是子线程, 线程ID: %ld\n", pthread_self());
    for(int i=0; i<9; ++i)
    {
        printf(" child i: %d\n", i);
    }

    return NULL;
}

int main()
{
    // 1. 创建一个子线程
    pthread_t tid;
    pthread_create(&tid, NULL, working, NULL);

    printf("子线程创建成功, 线程ID: %ld\n", tid);
    // 2. 子线程不会执行下边的代码, 主线程执行
    printf("我是主线程, 线程ID: %ld\n", pthread_self());
    for(int i=0; i<3; ++i)
    {
        printf("i = %d\n", i);
    }

    // 杀死子线程, 如果子线程中做系统调用, 子线程就结束了
    pthread_cancel(tid);

    // 让主线程自己退出即可
    pthread_exit(NULL);
    
    return 0;
}
```



> [!note]
> 关于系统调用有两种方式：
> 
> 1. 直接调用Linux系统函数
> 2. 调用标准C库函数，为了实现某些功能，在Linux平台下标准C库函数会调用相关的系统函数


### 线程ID比较
在Linux中线程ID本质就是一个无符号长整形，因此可以直接使用比较操作符比较两个线程的ID，但是线程库是可以跨平台使用的，在某些平台上 pthread_t可能不是一个单纯的整形，这中情况下比较两个线程的ID必须要使用比较函数，函数原型如下：

```c
#include <pthread.h>
int pthread_equal(pthread_t t1, pthread_t t2);
```

- 参数：t1 和 t2 是要比较的线程的线程ID
- 返回值：如果两个线程ID相等返回非0值，如果不相等返回0

## 多线程并发

编写多线程版的并发服务器程序和[[C语言#多进程并发|多进程思路]]差不多，考虑明白了对号入座即可。多线程中的线程有两大类：主线程(父线程)和子线程，他们分别要在服务器端处理监听和通信流程。根据多进程的处理思路，就可以这样设计了：

- 主线程：
	- 负责监听，处理客户端的连接请求，也就是在父进程中循环调用accept()函数
	- 创建子线程：建立一个新的连接，就创建一个新的子进程，让这个子进程和对应的客户端通信
	- 回收子线程资源：由于回收需要调用阻塞函数，这样就会影响accept()，直接做线程分离即可。
- 子线程：负责通信，基于主线程建立新连接之后得到的文件描述符，和对应的客户端完成数据的接收和发送。
	- 发送数据：`send() / write()`
	- 接收数据：`recv() / read()`

在多线程版的服务器端程序中，<font color=#ff0000>多个线程共用同一个地址空间</font>，有些数据是共享的，有些数据的独占的，下面来分析一些其中的一些细节：

- 同一地址空间中的多个线程的栈空间是独占的
- 多个线程共享全局数据区，堆区，以及内核区的文件描述符等资源，因此需要注意数据覆盖问题，并且在多个线程访问共享资源的时候，还需要进行线程同步。


<img src="https://www.subingwen.cn/linux/concurrence/image-20210306181008515.png" alt="image.png" style="zoom:40%;" />

多线程版Tcp服务器示例代码如下：

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <arpa/inet.h>
#include <pthread.h>

struct SockInfo
{
    int fd;                      // 通信
    pthread_t tid;               // 线程ID
    struct sockaddr_in addr;     // 地址信息
};

struct SockInfo infos[128];

void working(void arg)
{
    while(1)
    {
        struct SockInfo info = (struct SockInfo)arg;
        // 接收数据
        char buf[1024];
        int ret = read(info->fd, buf, sizeof(buf));
        if(ret == 0)
        {
            printf("客户端已经关闭连接...\n");
            info->fd = -1;
            break;
        }
        else if(ret == -1)
        {
            printf("接收数据失败...\n");
            info->fd = -1;
            break;
        }
        else
        {
            write(info->fd, buf, strlen(buf)+1);
        }
    }
    return NULL;
}

int main()
{
    // 1. 创建用于监听的套接字
    int fd = socket(AF_INET, SOCK_STREAM, 0);
    if(fd == -1)
    {
        perror("socket");
        exit(0);
    }

    // 2. 绑定
    struct sockaddr_in addr;
    addr.sin_family = AF_INET;          // ipv4
    addr.sin_port = htons(8989);        // 字节序应该是网络字节序
    addr.sin_addr.s_addr =  INADDR_ANY; // == 0, 获取IP的操作交给了内核
    int ret = bind(fd, (struct sockaddr)&addr, sizeof(addr));
    if(ret == -1)
    {
        perror("bind");
        exit(0);
    }

    // 3.设置监听
    ret = listen(fd, 100);
    if(ret == -1)
    {
        perror("listen");
        exit(0);
    }

    // 4. 等待, 接受连接请求
    int len = sizeof(struct sockaddr);

    // 数据初始化
    int max = sizeof(infos) / sizeof(infos[0]);
    for(int i=0; i<max; ++i)
    {
        bzero(&infos[i], sizeof(infos[i]));
        infos[i].fd = -1;
        infos[i].tid = -1;
    }

    // 父进程监听, 子进程通信
    while(1)
    {
        // 创建子线程
        struct SockInfo pinfo;
        for(int i=0; i<max; ++i)
        {
            if(infos[i].fd == -1)
            {
                pinfo = &infos[i];
                break;
            }
            if(i == max-1)
            {
                sleep(1);
                i--;
            }
        }

        int connfd = accept(fd, (struct sockaddr)&pinfo->addr, &len);
        printf("parent thread, connfd: %d\n", connfd);
        if(connfd == -1)
        {
            perror("accept");
            exit(0);
        }
        pinfo->fd = connfd;
        pthread_create(&pinfo->tid, NULL, working, pinfo);
        pthread_detach(pinfo->tid);
    }

    // 释放资源
    close(fd);  // 监听

    return 0;
}
```

在编写多线程版并发服务器代码的时候，需要注意父子线程共用同一个地址空间中的文件描述符，因此每当在主线程中建立一个新的连接，都需要将得到文件描述符值保存起来，不能在同一变量上进行覆盖，这样做丢失了之前的文件描述符值也就不知道怎么和客户端通信了。

在上面示例代码中是将成功建立连接之后得到的用于通信的文件描述符值保存到了一个全局数组中，每个子线程需要和不同的客户端通信，需要的文件描述符值也就不一样，只要保证存储每个有效文件描述符值的变量对应不同的内存地址，在使用的时候就不会发生数据覆盖的现象，造成通信数据的混乱了。

## 线程同步

### 线程同步概念
假设有4个线程A、B、C、D，当前一个线程A对内存中的`共享资源`进行访问的时候，其他线程B, C, D都不可以对这块内存进行操作，直到线程A对这块内存访问完毕为止，B，C，D中的一个才能访问这块内存，剩余的两个需要继续阻塞等待，以此类推，直至所有的线程都对这块内存操作完毕。 线程对内存的这种访问方式就称之为线程同步，通过对概念的介绍，我们可以了解到`所谓的同步并不是多个线程同时对内存进行访问，而是按照先后顺序依次进行的`。

#### 为什么要同步

> 在研究线程同步之前，先来看一个两个线程交替数数(每个线程数50个数，交替数到100)的例子：

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
#include <pthread.h>

#define MAX 50
// 全局变量
int number;

// 线程处理函数
void funcA_num(void arg)
{
    for(int i=0; i<MAX; ++i)
    {
        int cur = number;
        cur++;
        usleep(10);
        number = cur;
        printf("Thread A, id = %lu, number = %d\n", pthread_self(), number);
    }

    return NULL;
}

void funcB_num(void arg)
{
    for(int i=0; i<MAX; ++i)
    {
        int cur = number;
        cur++;
        number = cur;
        printf("Thread B, id = %lu, number = %d\n", pthread_self(), number);
        usleep(5);
    }

    return NULL;
}

int main(int argc, const char argv[])
{
    pthread_t p1, p2;

    // 创建两个子线程
    pthread_create(&p1, NULL, funcA_num, NULL);
    pthread_create(&p2, NULL, funcB_num, NULL);

    // 阻塞，资源回收
    pthread_join(p1, NULL);
    pthread_join(p2, NULL);

    return 0;
}
```

编译并执行上面的测试程序，得到如下结果：
```c++
$ ./a.out 
Thread B, id = 140504473724672, number = 1
Thread B, id = 140504473724672, number = 2
Thread A, id = 140504482117376, number = 2
Thread B, id = 140504473724672, number = 3
Thread A, id = 140504482117376, number = 4
Thread B, id = 140504473724672, number = 5
Thread A, id = 140504482117376, number = 6
Thread B, id = 140504473724672, number = 7
Thread B, id = 140504473724672, number = 8
Thread A, id = 140504482117376, number = 7
Thread B, id = 140504473724672, number = 8
Thread B, id = 140504473724672, number = 9
Thread A, id = 140504482117376, number = 8
Thread B, id = 140504473724672, number = 9
Thread A, id = 140504482117376, number = 9
Thread B, id = 140504473724672, number = 10
Thread B, id = 140504473724672, number = 11
Thread A, id = 140504482117376, number = 10
Thread B, id = 140504473724672, number = 11
Thread A, id = 140504482117376, number = 11
Thread B, id = 140504473724672, number = 12
Thread A, id = 140504482117376, number = 13
Thread B, id = 140504473724672, number = 14
Thread A, id = 140504482117376, number = 15
Thread B, id = 140504473724672, number = 16
Thread B, id = 140504473724672, number = 17
Thread B, id = 140504473724672, number = 18
Thread B, id = 140504473724672, number = 19
Thread A, id = 140504482117376, number = 17
Thread B, id = 140504473724672, number = 18
Thread B, id = 140504473724672, number = 19
Thread A, id = 140504482117376, number = 19
Thread B, id = 140504473724672, number = 20
Thread A, id = 140504482117376, number = 20
Thread B, id = 140504473724672, number = 21
Thread A, id = 140504482117376, number = 21
Thread B, id = 140504473724672, number = 22
Thread A, id = 140504482117376, number = 22
Thread B, id = 140504473724672, number = 23
Thread A, id = 140504482117376, number = 23
Thread B, id = 140504473724672, number = 24
Thread A, id = 140504482117376, number = 24
Thread B, id = 140504473724672, number = 25
Thread A, id = 140504482117376, number = 25
Thread B, id = 140504473724672, number = 26
Thread A, id = 140504482117376, number = 26
Thread B, id = 140504473724672, number = 27
Thread A, id = 140504482117376, number = 27
Thread B, id = 140504473724672, number = 28
Thread A, id = 140504482117376, number = 28
Thread B, id = 140504473724672, number = 29
Thread A, id = 140504482117376, number = 29
Thread B, id = 140504473724672, number = 30
Thread A, id = 140504482117376, number = 30
Thread B, id = 140504473724672, number = 31
Thread A, id = 140504482117376, number = 31
Thread B, id = 140504473724672, number = 32
Thread A, id = 140504482117376, number = 32
Thread B, id = 140504473724672, number = 33
Thread A, id = 140504482117376, number = 33
Thread B, id = 140504473724672, number = 34
Thread A, id = 140504482117376, number = 34
Thread B, id = 140504473724672, number = 35
Thread A, id = 140504482117376, number = 35
Thread B, id = 140504473724672, number = 36
Thread A, id = 140504482117376, number = 36
Thread B, id = 140504473724672, number = 37
Thread A, id = 140504482117376, number = 37
Thread B, id = 140504473724672, number = 38
Thread A, id = 140504482117376, number = 38
Thread B, id = 140504473724672, number = 39
Thread A, id = 140504482117376, number = 39
Thread A, id = 140504482117376, number = 40
Thread B, id = 140504473724672, number = 41
Thread B, id = 140504473724672, number = 42
Thread A, id = 140504482117376, number = 42
Thread A, id = 140504482117376, number = 43
Thread B, id = 140504473724672, number = 44
Thread B, id = 140504473724672, number = 45
Thread A, id = 140504482117376, number = 45
Thread B, id = 140504473724672, number = 46
Thread A, id = 140504482117376, number = 46
Thread B, id = 140504473724672, number = 47
Thread A, id = 140504482117376, number = 47
Thread B, id = 140504473724672, number = 48
Thread A, id = 140504482117376, number = 48
Thread B, id = 140504473724672, number = 49
Thread A, id = 140504482117376, number = 50
Thread B, id = 140504473724672, number = 51
Thread A, id = 140504482117376, number = 51
Thread B, id = 140504473724672, number = 52
Thread A, id = 140504482117376, number = 53
Thread A, id = 140504482117376, number = 54
Thread A, id = 140504482117376, number = 55
Thread A, id = 140504482117376, number = 56
Thread A, id = 140504482117376, number = 57
Thread A, id = 140504482117376, number = 58
Thread A, id = 140504482117376, number = 59
Thread A, id = 140504482117376, number = 60
Thread A, id = 140504482117376, number = 61
robin@OS:~/abc/b$ 
```

通过对上面例子的测试，可以看出虽然每个线程内部循环了50次每次数一个数，但是最终没有数到100，通过输出的结果可以看到，有些数字被重复数了多次，其原因就是没有对线程进行同步处理，造成了数据的混乱。

两个线程在数数的时候需要分时复用CPU时间片，并且测试程序中调用了`sleep()`导致线程的CPU时间片没用完就被迫挂起了，这样就能让CPU的上下文切换(保存当前状态, 下一次继续运行的时候需要加载保存的状态)更加频繁，更容易再现数据混乱的这个现象。

<img src="https://www.subingwen.cn/linux/thread-sync/1568967847637.png" alt="image.png" style="zoom:70%;" />

CPU对应寄存器、一级缓存、二级缓存、三级缓存是独占的，用于存储处理的数据和线程的状态信息，数据被CPU处理完成需要再次被写入到物理内存中，物理内存数据也可以通过文件IO操作写入到磁盘中。
在测试程序中两个线程共用全局变量number当线程变成运行态之后开始数数，从物理内存加载数据，让后将数据放到CPU进行运算，最后将结果更新到物理内存中。如果数数的两个线程都可以顺利完成这个流程，那么得到的结果肯定是正确的。
如果线程A执行这个过程期间就失去了CPU时间片，线程A被挂起了最新的数据没能更新到物理内存。线程B变成运行态之后从物理内存读数据，很显然它没有拿到最新数据，只能基于旧的数据往后数，然后失去CPU时间片挂起。线程A得到CPU时间片变成运行态，第一件事儿就是将上次没更新到内存的数据更新到内存，但是这样会导致线程B已经更新到内存的数据被覆盖，活儿白干了，最终导致有些数据会被重复数很多次。

#### 同步方式
对于多个线程访问共享资源出现数据混乱的问题，需要进行线程同步。常用的线程同步方式有四种：互斥锁、读写锁、条件变量、信号量。所谓的共享资源就是多个线程共同访问的变量，这些变量通常为全局数据区变量或者堆区变量，这些变量对应的共享资源也被称之为临界资源。

<img src="https://www.subingwen.cn/linux/thread-sync/image-20200106092600543.png" alt="image.png" style="zoom:60%;" />

找到临界资源之后，再找和临界资源相关的上下文代码，这样就得到了一个代码块，这个代码块可以称之为临界区。确定好临界区(临界区越小越好)之后，就可以进行线程同步了，线程同步的大致处理思路是这样的：

- 在临界区代码的上边，添加加锁函数，对临界区加锁。
	- 哪个线程调用这句代码，就会把这把锁锁上，其他线程就只能阻塞在锁上了。
- 在临界区代码的下边，添加解锁函数，对临界区解锁。
	- 出临界区的线程会将锁定的那把锁打开，其他抢到锁的线程就可以进入到临界区了。
- 通过锁机制能保证临界区代码最多只能同时有一个线程访问，这样并行访问就变为串行访问了。

### 互斥锁
#### 互斥锁函数

互斥锁是线程同步最常用的一种方式，通过互斥锁可以锁定一个代码块, 被锁定的这个代码块, 所有的线程只能顺序执行(不能并行处理)，这样多线程访问共享资源数据混乱的问题就可以被解决了，需要付出的代价就是执行效率的降低，因为默认临界区多个线程是可以并行处理的，现在只能串行处理。

在Linux中互斥锁的类型为`pthread_mutex_t`，创建一个这种类型的变量就得到了一把互斥锁：

```c
pthread_mutex_t  mutex;
```

在创建的锁对象中保存了当前这把锁的状态信息：锁定还是打开，如果是锁定状态还记录了给这把锁加锁的线程信息(线程ID)。一个互斥锁变量只能被一个线程锁定，被锁定之后其他线程再对互斥锁变量加锁就会被阻塞，直到这把互斥锁被解锁，被阻塞的线程才能被解除阻塞。`一般情况下，每一个共享资源对应一个把互斥锁，锁的个数和线程的个数无关`。

> Linux 提供的互斥锁操作函数如下，如果函数调用成功会返回0，调用失败会返回相应的错误号：

```c++
// 初始化互斥锁
// restrict: 是一个关键字, 用来修饰指针, 只有这个关键字修饰的指针可以访问指向的内存地址, 其他指针是不行的
// 1. 分配并设置内存：函数会分配内存以保存互斥锁的状态和其他相关信息 2. 初始化互斥锁属性：如果提供了属性对象，函数将使用该对象中的属性对互斥锁进行初始化。否则，将使用默认属性。 初始化互斥锁状态
int pthread_mutex_init(pthread_mutex_t restrict mutex,
           const pthread_mutexattr_t restrict attr);
// 释放互斥锁资源            
int pthread_mutex_destroy(pthread_mutex_t mutex);
```

- 参数:
	- mutex: 互斥锁变量的地址
	- attr: 互斥锁的属性, 一般使用默认属性即可, 这个参数指定为NULL

```c
// 修改互斥锁的状态, 将其设定为锁定状态, 这个状态被写入到参数 mutex 中
int pthread_mutex_lock(pthread_mutex_t mutex);
```

这个函数被调用, 首先会判断参数 mutex 互斥锁中的状态是不是锁定状态:
- 没有被锁定, 是打开的, 这个线程可以加锁成功, 这个这个锁中会记录是哪个线程加锁成功了
- 如果被锁定了, 其他线程加锁就失败了, 这些线程都会阻塞在这把锁上
- 当这把锁被解开之后, 这些阻塞在锁上的线程就解除阻塞了，并且这些线程是通过竞争的方式对这把锁加锁，没抢到锁的线程继续阻塞

```c
// 尝试加锁
int pthread_mutex_trylock(pthread_mutex_t mutex);
```

调用这个函数对互斥锁变量加锁还是有两种情况:
- 如果这把锁没有被锁定是打开的，线程加锁成功
- 如果锁变量被锁住了，调用这个函数加锁的线程，不会被阻塞，加锁失败直接返回错误号

```c
// 对互斥锁解锁
int pthread_mutex_unlock(pthread_mutex_t mutex);
```

不是所有的线程都可以对互斥锁解锁，哪个线程加的锁, 哪个线程才能解锁成功。

#### 互斥锁使用
我们可以将上面多线程交替数数的例子修改一下，使用互斥锁进行线程同步。两个线程一共操作了同一个全局变量，因此需要添加一互斥锁，来控制这两个线程。

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
#include <pthread.h>

#define MAX 100
// 全局变量
int number;

// 创建一把互斥锁
// 全局变量, 多个线程共享
pthread_mutex_t mutex;

// 线程处理函数
void funcA_num(void arg)
{
    for(int i=0; i<MAX; ++i)
    {
        // 如果线程A加锁成功, 不阻塞
        // 如果B加锁成功, 线程A阻塞
        pthread_mutex_lock(&mutex);
        int cur = number;
        cur++;
        usleep(10);
        number = cur;
        pthread_mutex_unlock(&mutex);
        printf("Thread A, id = %lu, number = %d\n", pthread_self(), number);
    }

    return NULL;
}

void funcB_num(void arg)
{
    for(int i=0; i<MAX; ++i)
    {
        // a加锁成功, b线程访问这把锁的时候是锁定的
        // 线程B先阻塞, a线程解锁之后阻塞解除
        // 线程B加锁成功了
        pthread_mutex_lock(&mutex);
        int cur = number;
        cur++;
        number = cur;
        pthread_mutex_unlock(&mutex);
        printf("Thread B, id = %lu, number = %d\n", pthread_self(), number);
        usleep(5);
    }

    return NULL;
}

int main(int argc, const char argv[])
{
    pthread_t p1, p2;

    // 初始化互斥锁
    pthread_mutex_init(&mutex, NULL);

    // 创建两个子线程
    pthread_create(&p1, NULL, funcA_num, NULL);
    pthread_create(&p2, NULL, funcB_num, NULL);

    // 阻塞，资源回收
    pthread_join(p1, NULL);
    pthread_join(p2, NULL);

    // 销毁互斥锁
    // 线程销毁之后, 再去释放互斥锁
    pthread_mutex_destroy(&mutex);
    return 0;
}
```

### 死锁
当多个线程访问共享资源, 需要加锁, 如果锁使用不当, 就会造成死锁这种现象。如果线程死锁造成的后果是：所有的线程都被阻塞，并且线程的阻塞是无法解开的(因为可以解锁的线程也被阻塞了)。
造成死锁的场景有如下几种：
- 加锁之后忘记解锁

```c
// 场景1
void func()
{
    for(int i=0; i<6; ++i)
    {
        // 当前线程A加锁成功, 当前循环完毕没有解锁, 在下一轮循环的时候自己被阻塞了
        // 其余的线程也被阻塞
    	pthread_mutex_lock(&mutex);
    	....
    	.....
        // 忘记解锁
    }
}

// 场景2
void func()
{
    for(int i=0; i<6; ++i)
    {
        // 当前线程A加锁成功
        // 其余的线程被阻塞
    	pthread_mutex_lock(&mutex);
    	....
    	.....
        if(xxx)
        {
            // 函数退出, 没有解锁(解锁函数无法被执行了)
            return ;
        }
        
        pthread_mutex_lock(&mutex);
    }
}
```

- 重复加锁, 造成死锁
```c
void func()
{
    for(int i=0; i<6; ++i)
    {
        // 当前线程A加锁成功
        // 其余的线程阻塞
    	pthread_mutex_lock(&mutex);
        // 锁被锁住了, A线程阻塞
        pthread_mutex_lock(&mutex);
    	....
    	.....
        pthread_mutex_unlock(&mutex);
    }
}

// 隐藏的比较深的情况
void funcA()
{
    for(int i=0; i<6; ++i)
    {
        // 当前线程A加锁成功
        // 其余的线程阻塞
    	pthread_mutex_lock(&mutex);
    	....
    	.....
        pthread_mutex_unlock(&mutex);
    }
}

void funcB()
{
    for(int i=0; i<6; ++i)
    {
        // 当前线程A加锁成功
        // 其余的线程阻塞
    	pthread_mutex_lock(&mutex);
        funcA();		// 重复加锁
    	....
    	.....
        pthread_mutex_unlock(&mutex);
    }
}
```

- 在程序中有多个共享资源, 因此有很多把锁，随意加锁，导致相互被阻塞
```text
场景描述:
  1. 有两个共享资源:X, Y，X对应锁A, Y对应锁B
     - 线程A访问资源X, 加锁A
     - 线程B访问资源Y, 加锁B
  2. 线程A要访问资源Y, 线程B要访问资源X，因为资源X和Y已经被对应的锁锁住了，因此这个两个线程被阻塞
     - 线程A被锁B阻塞了, 无法打开A锁
     - 线程B被锁A阻塞了, 无法打开B锁
```

<img src="https://www.subingwen.cn/linux/thread-sync/1557806644326.png" alt="image.png" style="zoom:60%;" />

在使用多线程编程的时候，如何避免死锁呢？
- 避免多次锁定, 多检查
- 对共享资源访问完毕之后, 一定要解锁，或者在加锁的使用 trylock
- 如果程序中有多把锁, 可以控制对锁的访问顺序(顺序访问共享资源，但在有些情况下是做不到的)，另外也可以在对其他互斥锁做加锁操作之前，先释放当前线程拥有的互斥锁。
- 项目程序中可以引入一些专门用于死锁检测的模块

### 读写锁
#### 读写锁函数
读写锁是互斥锁的升级版, `在做读操作的时候可以提高程序的执行效率，如果所有的线程都是做读操作, 那么读是并行的`，但是使用互斥锁，读操作是串行的。
读写锁是一把锁，锁的类型为`pthread_rwlock_t`，有了类型之后就可以创建一把互斥锁了：

```c
pthread_rwlock_t rwlock;
```

之所以称其为读写锁，是因为这把锁既可以锁定读操作，也可以锁定写操作。为了方便理解，可以大致认为在这把锁中记录了这些信息：

- 锁的状态: 锁定/打开
- 锁定的是什么操作: 读操作/写操作，`使用读写锁锁定了读操作，需要先解锁才能去锁定写操作，反之亦然`。
- 哪个线程将这把锁锁上了

读写锁的使用方式也互斥锁的使用方式是完全相同的：找共享资源, 确定临界区，在临界区的开始位置加锁(读锁/写锁)，临界区的结束位置解锁。
因为通过一把读写锁可以锁定读或者写操作，下面介绍一下关于读写锁的特点：
1. 使用读写锁的读锁锁定了临界区，线程对临界区的访问是并行的，`读锁是共享的`。
2. 使用读写锁的写锁锁定了临界区，线程对临界区的访问是串行的，`写锁是独占的`。
3. 使用读写锁分别对两个临界区加了读锁和写锁，两个线程要同时访问者两个临界区，访问写锁临界区的线程继续运行，访问读锁临界区的线程阻塞，因为`写锁比读锁的优先级高`。
<font color=#ff0000>如果说程序中所有的线程都对共享资源做写操作，使用读写锁没有优势，和互斥锁是一样的，如果说程序中所有的线程都对共享资源有写也有读操作，并且对共享资源读的操作越多，读写锁更有优势</font>。

> Linux提供的读写锁操作函数原型如下，如果函数调用成功返回0，失败返回对应的错误号：

```C
#include <pthread.h>
pthread_rwlock_t rwlock;
// 初始化读写锁
int pthread_rwlock_init(pthread_rwlock_t restrict rwlock,
           const pthread_rwlockattr_t restrict attr);
// 释放读写锁占用的系统资源
int pthread_rwlock_destroy(pthread_rwlock_t rwlock);
```

- 参数:
	- `rwlock`: 读写锁的地址，传出参数
	- `attr`: 读写锁属性，一般使用默认属性，指定为NULL

```C
// 在程序中对读写锁加读锁, 锁定的是读操作
int pthread_rwlock_rdlock(pthread_rwlock_t rwlock);
```

调用这个函数，如果读写锁是打开的，那么加锁成功；如果读写锁已经锁定了读操作，调用这个函数依然可以加锁成功，因为读锁是共享的；如果读写锁已经锁定了写操作，调用这个函数的线程会被阻塞。

```C
// 这个函数可以有效的避免死锁
// 如果加读锁失败, 不会阻塞当前线程, 直接返回错误号
int pthread_rwlock_tryrdlock(pthread_rwlock_t rwlock);
```

调用这个函数，如果读写锁是打开的，那么加锁成功；如果读写锁已经锁定了读操作，调用这个函数依然可以加锁成功，因为读锁是共享的；如果读写锁已经锁定了写操作，调用这个函数加锁失败，对应的线程不会被阻塞，可以在程序中对函数返回值进行判断，添加加锁失败之后的处理动作。

```C
// 在程序中对读写锁加写锁, 锁定的是写操作
int pthread_rwlock_wrlock(pthread_rwlock_t rwlock);
```

调用这个函数，如果读写锁是打开的，那么加锁成功；如果读写锁已经锁定了读操作或者锁定了写操作，调用这个函数的线程会被阻塞。

```C
// 这个函数可以有效的避免死锁
// 如果加写锁失败, 不会阻塞当前线程, 直接返回错误号
int pthread_rwlock_trywrlock(pthread_rwlock_t rwlock);
```

调用这个函数，如果读写锁是打开的，那么加锁成功；如果读写锁已经锁定了读操作或者锁定了写操作，调用这个函数加锁失败，但是线程不会阻塞，可以在程序中对函数返回值进行判断，添加加锁失败之后的处理动作。

```C
// 解锁, 不管锁定了读还是写都可用解锁
int pthread_rwlock_unlock(pthread_rwlock_t rwlock);
```


#### 读写锁使用
题目要求：8个线程操作同一个全局变量，3个线程不定时写同一全局资源，5个线程不定时读同一全局资源。

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>

// 全局变量
int number = 0;

// 定义读写锁
pthread_rwlock_t rwlock;

// 写的线程的处理函数
void writeNum(void arg)
{
    while(1)
    {
        pthread_rwlock_wrlock(&rwlock);
        int cur = number;
        cur ++;
        number = cur;
        printf("++写操作完毕, number : %d, tid = %ld\n", number, pthread_self());
        pthread_rwlock_unlock(&rwlock);
        // 添加sleep目的是要看到多个线程交替工作
        usleep(rand() % 100);
    }

    return NULL;
}

// 读线程的处理函数
// 多个线程可以如果处理动作相同, 可以使用相同的处理函数
// 每个线程中的栈资源是独享
void readNum(void arg)
{
    while(1)
    {
        pthread_rwlock_rdlock(&rwlock);
        printf("--全局变量number = %d, tid = %ld\n", number, pthread_self());
        pthread_rwlock_unlock(&rwlock);
        usleep(rand() % 100);
    }
    return NULL;
}

int main()
{
    // 初始化读写锁
    pthread_rwlock_init(&rwlock, NULL);

    // 3个写线程, 5个读的线程
    pthread_t wtid[3];
    pthread_t rtid[5];
    for(int i=0; i<3; ++i)
    {
        pthread_create(&wtid[i], NULL, writeNum, NULL);
    }

    for(int i=0; i<5; ++i)
    {
        pthread_create(&rtid[i], NULL, readNum, NULL);
    }

    // 释放资源
    for(int i=0; i<3; ++i)
    {
        pthread_join(wtid[i], NULL);
    }

    for(int i=0; i<5; ++i)
    {
        pthread_join(rtid[i], NULL);
    }

    // 销毁读写锁
    pthread_rwlock_destroy(&rwlock);

    return 0;
}
```

### 自旋锁
自旋锁类似于五斥锁，它的性能比互斥锁更高，但消耗的cpu资源多。
自旋锁与互斥锁很重要的一个区别在于，线程在申请自旋锁的时候，线程不会被挂起，它处于忙等待的状态。
这些函数是用于操作自旋锁的，自旋锁是一种轻量级的同步原语，它允许一个线程在获取锁之前忙等待，而不是被挂起，以减少线程切换的开销。下面是这些函数的详细说明：

pthread_spin_init
`int pthread_spin_init(pthread_spinlock_t *lock, int pshared);`

- **功能**：初始化自旋锁。
- **参数**：
    - `lock`：指向自旋锁变量的指针。
    - `pshared`：用于指定自旋锁的共享属性，通常设为 `PTHREAD_PROCESS_PRIVATE` 表示自旋锁是进程私有的。
- **返回值**：成功返回0，失败返回相应的错误码。

pthread_spin_destroy
`int pthread_spin_destroy(pthread_spinlock_t *lock);`

- **功能**：销毁自旋锁。
- **参数**：
    - `lock`：指向自旋锁变量的指针。
- **返回值**：成功返回0，失败返回相应的错误码。

pthread_spin_lock
`int pthread_spin_lock(pthread_spinlock_t *lock);`

- **功能**：获取自旋锁。
- **参数**：
    - `lock`：指向自旋锁变量的指针。
- **返回值**：成功返回0，失败返回相应的错误码。

pthread_spin_unlock

`int pthread_spin_unlock(pthread_spinlock_t *lock);`

- **功能**：释放自旋锁。
- **参数**：
    - `lock`：指向自旋锁变量的指针。
- **返回值**：成功返回0，失败返回相应的错误码。

注意事项

- 自旋锁适用于临界区代码执行时间非常短的情况，如果临界区执行时间较长，可能会造成其他线程长时间忙等待，浪费CPU资源。
- 自旋锁不支持递归锁。
- 使用自旋锁时，需要确保临界区内的代码尽量简短和高效，以减少其他线程的忙等待时间。

### 条件变量
#### 条件变量函数

**条件变量则是用于在线程之间同步共享数据的值**。严格意义上来说，条件变量的主要作用不是处理线程同步, 而是进行线程的阻塞。如果在多线程程序中只使用条件变量无法实现线程的同步, 必须要配合互斥锁来使用。虽然条件变量和互斥锁都能阻塞线程，但是二者的效果是不一样的，二者的区别如下：

- 假设有A-Z 26个线程，这26个线程共同访问同一把互斥锁，如果线程A加锁成功，那么其余B-Z线程访问互斥锁都阻塞，所有的线程只能顺序访问临界区
- 条件变量只有在满足指定条件下才会阻塞线程，如果条件不满足，多个线程可以同时进入临界区，同时读写临界资源，这种情况下还是会出现共享资源中数据的混乱。

一般情况下条件变量用于处理生产者和消费者模型，并且和互斥锁配合使用。条件变量类型对应的类型为`pthread_cond_t`，这样就可以定义一个条件变量类型的变量了：

```c
pthread_cond_t cond;
```

被条件变量阻塞的线程的线程信息会被记录到这个变量中，以便在解除阻塞的时候使用。
> 条件变量操作函数函数原型如下：

```c
#include <pthread.h>
pthread_cond_t cond;
// 初始化
int pthread_cond_init(pthread_cond_t restrict cond,
      const pthread_condattr_t restrict attr);
// 销毁释放资源        
int pthread_cond_destroy(pthread_cond_t cond);
```

- 参数:
	- cond: 条件变量的地址
	- attr: 条件变量属性, 一般使用默认属性, 指定为NULL

```c
// 线程阻塞函数, 哪个线程调用这个函数, 哪个线程就会被阻塞
int pthread_cond_wait(pthread_cond_t restrict cond, pthread_mutex_t restrict mutex);
```

通过函数原型可以看出，该函数在阻塞线程的时候，需要一个互斥锁参数，这个互斥锁主要功能是进行线程同步，让线程顺序进入临界区，避免出现数共享资源的数据混乱。该函数会对这个互斥锁做以下几件事情：

- 在阻塞线程时候，如果线程已经对互斥锁mutex上锁，那么会将这把锁打开，这样做是为了避免死锁
- 当线程解除阻塞(被cond唤醒)的时候，函数内部会帮助这个线程再次将这个mutex互斥锁锁上，继续向下访问临界区，<font color=#ff0000>注意被cond唤醒，需要竞争加锁mutex，只有加锁mutex成功的才能解除这个阻塞</font>

```c
// 表示的时间是从1971.1.1到某个时间点的时间, 总长度使用秒/纳秒表示
struct timespec {
	time_t tv_sec;      / Seconds /
	long   tv_nsec;     / Nanoseconds [0 .. 999999999] /
};
// 将线程阻塞一定的时间长度, 时间到达之后, 线程就解除阻塞了
int pthread_cond_timedwait(pthread_cond_t restrict cond,
           pthread_mutex_t restrict mutex, const struct timespec restrict abstime);
```

这个函数的前两个参数和`pthread_cond_wait`函数是一样的，第三个参数表示线程阻塞的时长，但是需要额外注意一点：`struct timespec`这个结构体中记录的时间是`从1971.1.1到某个时间点的时间，总长度使用秒/纳秒表示`。因此赋值方式相对要麻烦一点：

```c
time_t mytim = time(NULL);	// 1970.1.1 0:0:0 到当前的总秒数
struct timespec tmsp;
tmsp.tv_nsec = 0;
tmsp.tv_sec = time(NULL) + 100;	// 线程阻塞100s
```

```c
// 唤醒阻塞在条件变量上的线程, 至少有一个被解除阻塞
int pthread_cond_signal(pthread_cond_t cond);
// 唤醒阻塞在条件变量上的线程, 被阻塞的线程全部解除阻塞
int pthread_cond_broadcast(pthread_cond_t cond);
```

调用上面两个函数中的任意一个，都可以换线被`pthread_cond_wait`或者`pthread_cond_timedwait`阻塞的线程，区别就在于`pthread_cond_signal`是唤醒至少一个被阻塞的线程(总个数不定)，`pthread_cond_broadcast`是唤醒所有被阻塞的线程。

#### 生产者和消费者
生产者和消费者模型的组成：

1. 生产者线程 -> 若干个
	- 生产商品或者任务放入到任务队列中
	- 任务队列满了就阻塞, 不满的时候就工作
	- 通过一个生产者的条件变量控制生产者线程阻塞和非阻塞
2. 消费者线程 -> 若干个
	- 读任务队列, 将任务或者数据取出
	- 任务队列中有数据就消费，没有数据就阻塞
	- 通过一个消费者的条件变量控制消费者线程阻塞和非阻塞
3. 队列 -> 存储任务/数据，对应一块内存，为了读写访问可以通过一个数据结构维护这块内存
	- 可以是数组、链表，也可以使用stl容器：`queue / stack / list / vector`

<img src="https://www.subingwen.cn/linux/thread-sync/1564644834918.png" alt="image.png" style="zoom:80%;" />
> 场景描述：使用条件变量实现生产者和消费者模型，生产者有5个，往链表头部添加节点，消费者也有5个，删除链表头部的节点。

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>

// 链表的节点
struct Node
{
    int number;
    struct Node* next;
};

// 定义条件变量, 控制消费者线程
pthread_cond_t cond;
// 互斥锁变量
pthread_mutex_t mutex;
// 指向头结点的指针
struct Node *head = NULL;

// 生产者的回调函数
void producer(void arg)
{
    // 一直生产
    while(1)
    {
        pthread_mutex_lock(&mutex);
        // 创建一个链表的新节点
        struct Node pnew = (struct Node)malloc(sizeof(struct Node));
        // 节点初始化
        pnew->number = rand() % 1000;
        // 节点的连接, 添加到链表的头部, 新节点就新的头结点
        pnew->next = head;
        // head指针前移
        head = pnew;
        printf("+++producer, number = %d, tid = %ld\n", pnew->number, pthread_self());
        pthread_mutex_unlock(&mutex);

        // 生产了任务, 通知消费者消费
        pthread_cond_broadcast(&cond);

        // 生产慢一点
        sleep(rand() % 3);
    }
    return NULL;
}

// 消费者的回调函数
void consumer(void arg)
{
    while(1)
    {
        pthread_mutex_lock(&mutex);
        // 一直消费, 删除链表中的一个节点
//        if(head == NULL)   // 这样写有bug
        while(head == NULL)
        {
            // 任务队列, 也就是链表中已经没有节点可以消费了
            // 消费者线程需要阻塞
            // 线程加互斥锁成功, 但是线程阻塞在这行代码上, 锁还没解开
            // 其他线程在访问这把锁的时候也会阻塞, 生产者也会阻塞 ==> 死锁
            // 这函数会自动将线程拥有的锁解开
            pthread_cond_wait(&cond, &mutex);
            // 当消费者线程解除阻塞之后, 会自动将这把锁锁上
            // 这时候当前这个线程又重新拥有了这把互斥锁
        }
        // 取出链表的头结点, 将其删除
        struct Node pnode = head;
        printf("--consumer: number: %d, tid = %ld\n", pnode->number, pthread_self());
        head  = pnode->next;
        free(pnode);
        pthread_mutex_unlock(&mutex);        

        sleep(rand() % 3);
    }
    return NULL;
}

int main()
{
    // 初始化条件变量
    pthread_cond_init(&cond, NULL);
    pthread_mutex_init(&mutex, NULL);

    // 创建5个生产者, 5个消费者
    pthread_t ptid[5];
    pthread_t ctid[5];
    for(int i=0; i<5; ++i)
    {
        pthread_create(&ptid[i], NULL, producer, NULL);
    }

    for(int i=0; i<5; ++i)
    {
        pthread_create(&ctid[i], NULL, consumer, NULL);
    }

    // 释放资源
    for(int i=0; i<5; ++i)
    {
        // 阻塞等待子线程退出
        pthread_join(ptid[i], NULL);
    }

    for(int i=0; i<5; ++i)
    {
        pthread_join(ctid[i], NULL);
    }

    // 销毁条件变量
    pthread_cond_destroy(&cond);
    pthread_mutex_destroy(&mutex);

    return 0;
}
```

代码分析

```c
void consumer(void arg)
{
    while(1)
    {
        pthread_mutex_lock(&mutex);
        // 一直消费, 删除链表中的一个节点
        if(head == NULL)   // 这样写有bug
        {
            pthread_cond_wait(&cond, &mutex);
        }
        // 取出链表的头结点, 将其删除
        struct Node pnode = head;
        printf("--consumer: number: %d, tid = %ld\n", pnode->number, pthread_self());
        head  = pnode->next;
        free(pnode);
        pthread_mutex_unlock(&mutex);        

        sleep(rand() % 3);
    }
    return NULL;
}
/
为什么在第7行使用if 有bug:
    当任务队列为空, 所有的消费者线程都会被这个函数阻塞 pthread_cond_wait(&cond, &mutex);
    也就是阻塞在代码的第9行
	
    当生产者生产了1个节点, 调用 pthread_cond_broadcast(&cond); 唤醒了所有阻塞的线程
      - 有一个消费者线程通过 pthread_cond_wait()加锁成功, 其余没有加锁成功的线程继续阻塞
      - 加锁成功的线程向下运行, 并成功删除一个节点, 然后解锁
      - 没有加锁成功的线程解除阻塞继续抢这把锁, 另外一个子线程加锁成功
      - 但是这个线程删除链表节点的时候链表已经为空了, 后边访问这个空节点的时候就会出现段错误
    解决方案:
      - 需要循环的对链表是否为空进行判断, 需要将if 该成 while
/
```

### 信号量(Semaphore)
#### 信号量函数

信号量用在多线程多任务同步的，一个线程完成了某一个动作就通过信号量告诉别的线程，别的线程再进行某些动作。信号量是一种特殊的变量，它只能取自然数值并且只支持两种操作：等待（wait）和信号（signal）,信号量不一定是锁定某一个资源，而是流程上的概念，比如：有A，B两个线程，B线程要等A线程完成某一任务以后再进行自己下面的步骤，这个任务并不一定是锁定某一资源，还可以是进行一些计算或者数据处理之类。

`信号量(信号灯)`与互斥锁和条件变量的主要不同在于”灯”的概念，灯亮则意味着资源可用，灯灭则意味着不可用。信号量主要阻塞线程, 不能完全保证线程安全，如果要保证线程安全, 需要信号量和互斥锁一起使用。

信号量和条件变量一样用于处理生产者和消费者模型，用于阻塞生产者线程或者消费者线程的运行。信号的类型为`sem_t`对应的头文件为`<semaphore.h>`：

```c
#include <semaphore.h>
sem_t sem;
```

> Linux提供的信号量操作函数原型如下：

```c
#include <semaphore.h>
// 初始化信号量/信号灯
int sem_init(sem_t sem, int pshared, unsigned int value);
// 资源释放, 线程销毁之后调用这个函数即可
// 参数 sem 就是 sem_init() 的第一个参数            
int sem_destroy(sem_t sem);
```

- 参数:
	- sem：信号量变量地址
	- pshared：
		- 0：线程同步
		- 非0：进程同步
	- value：初始化当前信号量拥有的资源数(>=0)，如果资源数为0，线程就会被阻塞了。

```c
// 参数 sem 就是 sem_init() 的第一个参数  
// 函数被调用sem中的资源就会被消耗1个, 资源数-1
int sem_wait(sem_t sem);
```

当线程调用这个函数，并且`sem`中的资源数`>0`，线程不会阻塞，线程会占用`sem`中的一个资源，因此资源数-1，直到`sem`中的资源数减为`0`时，资源被耗尽，因此线程也就被阻塞了。

```c
// 参数 sem 就是 sem_init() 的第一个参数  
// 函数被调用sem中的资源就会被消耗1个, 资源数-1
int sem_trywait(sem_t sem);
```

当线程调用这个函数，并且`sem`中的资源数>0，线程不会阻塞，线程会占用`sem`中的一个资源，因此资源数-1，直到sem中的资源数减为`0`时，资源被耗尽，但是线程不会被阻塞，直接返回错误号，因此可以在程序中添加判断分支，用于处理获取资源失败之后的情况。

```c
// 表示的时间是从1971.1.1到某个时间点的时间, 总长度使用秒/纳秒表示
struct timespec {
	time_t tv_sec;      / Seconds /
	long   tv_nsec;     / Nanoseconds [0 .. 999999999] /
};
// 调用该函数线程获取sem中的一个资源，当资源数为0时，线程阻塞，在阻塞abs_timeout对应的时长之后，解除阻塞。
// abs_timeout: 阻塞的时间长度, 单位是s, 是从1970.1.1开始计算的
int sem_timedwait(sem_t sem, const struct timespec abs_timeout);
```

该函数的参数`abs_timeout`和`pthread_cond_timedwait`的最后一个参数是一样的，使用方法不再过多赘述。当线程调用这个函数，并且`sem`中的资源数`>0`，线程不会阻塞，线程会占用`sem`中的一个资源，因此资源数-1，直到`sem`中的资源数减为`0`时，资源被耗尽，线程被阻塞，当阻塞指定的时长之后，线程解除阻塞。

```c
// 调用该函数给sem中的资源数+1
int sem_post(sem_t sem);
```

调用该函数会将`sem`中的资源数`+1`，如果有线程在调用`sem_wait、sem_trywait、sem_timedwait`时因为sem中的资源数为`0`被阻塞了，这时这些线程会解除阻塞，获取到资源之后继续向下运行。

```c
// 查看信号量 sem 中的整形alarm数的当前值, 这个值会被写入到sval指针对应的内存中
// sval是一个传出参数
int sem_getvalue(sem_t sem, int sval);
```

通过这个函数可以查看`sem`中现在拥有的资源个数，通过第二个参数`sval`将数据传出，也就是说第二个参数的作用和返回值是一样的。

#### 生产者和消费者
由于生产者和消费者是两类线程，并且在还没有生成之前是不能进行消费的，在使用信号量处理这类问题的时候可以定义两个信号量，分别用于记录生产者和消费者线程拥有的总资源数。

```c
// 生产者线程 
sem_t psem;
// 消费者线程
sem_t csem;

// 信号量初始化
sem_init(&psem, 0, 5);    // 5个生产者可以同时生产
sem_init(&csem, 0, 0);    // 消费者线程没有资源, 因此不能消费

// 生产者线程
// 在生产之前, 从信号量中取出一个资源
sem_wait(&psem);	
// 生产者商品代码, 有商品了, 放到任务队列
......	 
......
......
// 通知消费者消费，给消费者信号量添加资源，让消费者解除阻塞
sem_post(&csem);
	
////////////////////////////////////////////////////////
////////////////////////////////////////////////////////

// 消费者线程
// 消费者需要等待生产, 默认启动之后应该阻塞
sem_wait(&csem);
// 开始消费
......
......
......
// 消费完成, 通过生产者生产，给生产者信号量添加资源
sem_post(&psem);
```

通过上面的代码可以知道，初始化信号量的时候没有消费者分配资源，消费者线程启动之后由于没有资源自然就被阻塞了，等生产者生产出产品之后，再给消费者分配资源，这样二者就可以配合着完成生产和消费流程了。

#### 信号量使用
> 场景描述：使用信号量实现生产者和消费者模型，生产者有5个，往链表头部添加节点，消费者也有5个，删除链表头部的节点。

##### 总资源数为1
如果生产者和消费者线程使用的信号量对应的总资源数为1，那么不管线程有多少个，可以工作的线程只有一个，其余线程由于拿不到资源，都被迫阻塞了。
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <semaphore.h>
#include <pthread.h>

// 链表的节点
struct Node
{
    int number;
    struct Node next;
};

// 生产者线程信号量
sem_t psem;
// 消费者线程信号量
sem_t csem;

// 互斥锁变量
pthread_mutex_t mutex;
// 指向头结点的指针
struct Node  head = NULL;

// 生产者的回调函数
void producer(void arg)
{
    // 一直生产
    while(1)
    {
        // 生产者拿一个信号灯
        sem_wait(&psem);
        // 创建一个链表的新节点
        struct Node pnew = (struct Node)malloc(sizeof(struct Node));
        // 节点初始化
        pnew->number = rand() % 1000;
        // 节点的连接, 添加到链表的头部, 新节点就新的头结点
        pnew->next = head;
        // head指针前移
        head = pnew;
        printf("+++producer, number = %d, tid = %ld\n", pnew->number, pthread_self());

        // 通知消费者消费, 给消费者加信号灯
        sem_post(&csem);
        

        // 生产慢一点
        sleep(rand() % 3);
    }
    return NULL;
}

// 消费者的回调函数
void consumer(void arg)
{
    while(1)
    {
        sem_wait(&csem);
        // 取出链表的头结点, 将其删除
        struct Node pnode = head;
        printf("--consumer: number: %d, tid = %ld\n", pnode->number, pthread_self());
        head  = pnode->next;
        free(pnode);
        // 通知生产者生成, 给生产者加信号灯
        sem_post(&psem);

        sleep(rand() % 3);
    }
    return NULL;
}

int main()
{
    // 初始化信号量
    // 生产者和消费者拥有的信号灯的总和为1
    sem_init(&psem, 0, 1);  // 生成者线程一共有1个信号灯
    sem_init(&csem, 0, 0);  // 消费者线程一共有0个信号灯

    // 创建5个生产者, 5个消费者
    pthread_t ptid[5];
    pthread_t ctid[5];
    for(int i=0; i<5; ++i)
    {
        pthread_create(&ptid[i], NULL, producer, NULL);
    }

    for(int i=0; i<5; ++i)
    {
        pthread_create(&ctid[i], NULL, consumer, NULL);
    }

    // 释放资源
    for(int i=0; i<5; ++i)
    {
        pthread_join(ptid[i], NULL);
    }

    for(int i=0; i<5; ++i)
    {
        pthread_join(ctid[i], NULL);
    }

    sem_destroy(&psem);
    sem_destroy(&csem);

    return 0;
}
```

<font color=#ff0000>通过测试代码可以得到如下结论：如果生产者和消费者使用的信号量总资源数为1，那么不会出现生产者线程和消费者线程同时访问共享资源的情况，不管生产者和消费者线程有多少个，它们都是顺序执行的。</font>

##### 总资源数大于1
如果生产者和消费者线程使用的信号量对应的总资源数为大于1，这种场景下出现的情况就比较多了：
- 多个生产者线程同时生产
- 多个消费者同时消费
- 生产者线程和消费者线程同时生产和消费

以上不管哪一种情况都可能会出现多个线程访问共享资源的情况，如果想防止共享资源出现数据混乱，那么就需要使用互斥锁进行线程同步，处理代码如下：

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <semaphore.h>
#include <pthread.h>

// 链表的节点
struct Node
{
    int number;
    struct Node next;
};

// 生产者线程信号量
sem_t psem;
// 消费者线程信号量
sem_t csem;

// 互斥锁变量
pthread_mutex_t mutex;
// 指向头结点的指针
struct Node  head = NULL;

// 生产者的回调函数
void producer(void arg)
{
    // 一直生产
    while(1)
    {
        // 生产者拿一个信号灯
        sem_wait(&psem);
        // 加锁, 这句代码放到 sem_wait()上边, 有可能会造成死锁
        pthread_mutex_lock(&mutex);
        // 创建一个链表的新节点
        struct Node pnew = (struct Node)malloc(sizeof(struct Node));
        // 节点初始化
        pnew->number = rand() % 1000;
        // 节点的连接, 添加到链表的头部, 新节点就新的头结点
        pnew->next = head;
        // head指针前移
        head = pnew;
        printf("+++producer, number = %d, tid = %ld\n", pnew->number, pthread_self());
        pthread_mutex_unlock(&mutex);

        // 通知消费者消费
        sem_post(&csem);
        
        // 生产慢一点
        sleep(rand() % 3);
    }
    return NULL;
}

// 消费者的回调函数
void consumer(void arg)
{
    while(1)
    {
        sem_wait(&csem);
        pthread_mutex_lock(&mutex);
        struct Node pnode = head;
        printf("--consumer: number: %d, tid = %ld\n", pnode->number, pthread_self());
        head  = pnode->next;
        // 取出链表的头结点, 将其删除
        free(pnode);
        pthread_mutex_unlock(&mutex);
        // 通知生产者生成, 给生产者加信号灯
        sem_post(&psem);

        sleep(rand() % 3);
    }
    return NULL;
}

int main()
{
    // 初始化信号量
    sem_init(&psem, 0, 5);  // 生成者线程一共有5个信号灯
    sem_init(&csem, 0, 0);  // 消费者线程一共有0个信号灯
    // 初始化互斥锁
    pthread_mutex_init(&mutex, NULL);

    // 创建5个生产者, 5个消费者
    pthread_t ptid[5];
    pthread_t ctid[5];
    for(int i=0; i<5; ++i)
    {
        pthread_create(&ptid[i], NULL, producer, NULL);
    }

    for(int i=0; i<5; ++i)
    {
        pthread_create(&ctid[i], NULL, consumer, NULL);
    }

    // 释放资源
    for(int i=0; i<5; ++i)
    {
        pthread_join(ptid[i], NULL);
    }

    for(int i=0; i<5; ++i)
    {
        pthread_join(ctid[i], NULL);
    }

    sem_destroy(&psem);
    sem_destroy(&csem);
    pthread_mutex_destroy(&mutex);

    return 0;
}
```

在编写上述代码的时候还有一个需要注意是事项，不管是消费者线程的处理函数还是生产者线程的处理函数内部有这么两行代码：
```c
// 消费者
sem_wait(&csem);
pthread_mutex_lock(&mutex);

// 生产者
sem_wait(&csem);
pthread_mutex_lock(&mutex);
```

这两行代码的调用顺序是不能颠倒的，如果颠倒过来就有可能会造成死锁，下面来分析一种死锁的场景：

```c
void producer(void arg)
{
    // 一直生产
    while(1)
    {
        pthread_mutex_lock(&mutex);
        // 生产者拿一个信号灯
        sem_wait(&psem);
		......
        ......
        // 通知消费者消费
        sem_post(&csem);
        pthread_mutex_unlock(&mutex);
        
        // 生产慢一点
        sleep(rand() % 3);
    }
    return NULL;
}

// 消费者的回调函数
void consumer(void arg)
{
    while(1)
    {
        pthread_mutex_lock(&mutex);
        sem_wait(&csem);
		......
        ......
        // 通知生产者生成, 给生产者加信号灯
        sem_post(&psem);
        pthread_mutex_unlock(&mutex);

        sleep(rand() % 3);
    }
    return NULL;
}

int main()
{
    // 初始化信号量
    sem_init(&psem, 0, 5);  // 生成者线程一共有5个信号灯
    sem_init(&csem, 0, 0);  // 消费者线程一共有0个信号灯
	......
	......
    return 0;
}
```

在上面的代码中，初始化状态下消费者线程没有任务信号量资源，假设某一个消费者线程先运行，调用`pthread_mutex_lock(&mutex)`;对互斥锁加锁成功，然后调用`sem_wait(&csem)`;由于没有资源，因此被阻塞了。其余的消费者线程由于没有抢到互斥锁，因此被阻塞在互斥锁上。对应生产者线程第一步操作也是调用`pthread_mutex_lock(&mutex)`;，但是这时候互斥锁已经被消费者线程锁上了，所有生产者都被阻塞，到此为止，多余的线程都被阻塞了，程序产生了死锁。


#### 线程和信号

每个线程都可以独立地设置信号掩码，设置进程信号掩码的函数sigprocmask，但在多线程环境下我们应该使用如下所示的pthread版本的sigprocmask函数来设置线程信号掩码：

```c
#include＜pthread.h＞
#include＜signal.h＞
int pthread_sigmask(int how,const sigset_t*newmask,sigset_t*oldmask);
```

所有线程共享信号处理函数。当我们在一个线程中设置了某个信号的信号处理函数后，它将覆盖其他线程为同一个信号设置的信号处理函数。

应该定义一个专门的线程来处理所有的信号。这可以通过如下两个步骤来实现：

1. 在主线程创建出其他子线程之前就调用pthread_sigmask来设置好信号掩码，所有新创建的子线程都将自动继承这个信号掩码。这样做之后，实际上所有线程都不会响应被屏蔽的信号了。
2. 在某个线程中调用如下函数来等待信号并处理之：
	```c
	#include＜signal.h＞
	int sigwait(const sigset_t*set,int*sig);
	```

sigwait函数：sigwait允许线程等待信号掩码中指定的任一信号到来。当其中一个信号到达时，sigwait返回，同时通过它的参数返回**接收到的信号的值**。这意味着线程可以在特定时间点同步地处理信号，而不是依赖于异步的信号处理函数。
处理信号：当sigwait返回后，表示一个信号已被接收，线程可以根据返回的信号值对该信号进行处理。这种方式允许线程以同步方式处理信号，即线程可以控制何时处理信号以及如何响应信号。

```c
static void *sig_thread(void *arg) {
  sigset_t *set = (sigset_t *) arg;
  int s, sig;
  for (;;) {
    /*第二个步骤，调用sigwait等待信号*/
    s = sigwait(set, &sig);
    if (s != 0)
      handle_error_en(s, "sigwait");
    printf("Signal handling thread got signal%d\n", sig);
  }
}
int main(int argc, char *argv[]) {
  pthread_t thread;
  sigset_t set;
  int s;
  /*第一个步骤，在主线程中设置信号掩码*/
  sigemptyset(&set);
  sigaddset(&set, SIGQUIT);
  sigaddset(&set, SIGUSR1);
  s = pthread_sigmask(SIG_BLOCK, &set, NULL);
  if (s != 0)
    handle_error_en(s, "pthread_sigmask");
  s = pthread_create(&thread, NULL, &sig_thread, (void *) &set);
  if (s != 0)
    handle_error_en(s, "pthread_create");
  pause();
}
```



# 一些补充点

bool类型不应该参与计算

```c++
bool b=true;
bool b2=-b;   //仍然为true
//b为true，提升为对应int=1，-b=-1
//b2=-1≠0，所以b2仍未true
 ```
       
(gdb) p i
$2 = 1
(gdb) p N
$3 = 100
(gdb) continue # 继续执行
Continuing.

Breakpoint 1, main (argc=1, argv=0x7fffffffd9e8) at debug_demo.cpp:12
12          sum=sum+i;
(gdb) print i
$4 = 2
(gdb) display i # 显示值
1: i = 2
(gdb) c
Continuing.

Breakpoint 1, main (argc=1, argv=0x7fffffffd9e8) at debug_demo.cpp:12
12          sum=sum+i;
1: i = 3
(gdb) 
Continuing.

Breakpoint 1, main (argc=1, argv=0x7fffffffd9e8) at debug_demo.cpp:12
12          sum=sum+i;
1: i = 4
(gdb) list 列出断点周围代码
7         int N=100;
8         int sum=0;
9         int i=1;
10
11        while (i<=N) {
12          sum=sum+i;
13          i=i+1;
14        }
15        
16        cout<<"sum = "<<sum<<endl;
(gdb) display sum # 追踪sum
2: sum = 6
(gdb) c
Continuing.

Breakpoint 1, main (argc=1, argv=0x7fffffffd9e8) at debug_demo.cpp:12
12          sum=sum+i;
1: i = 5
2: sum = 10
(gdb) watch i # 显示观察点
Hardware watchpoint 2: i
(gdb) c
Continuing.

Hardware watchpoint 2: i

Old value = 5
New value = 6
main (argc=1, argv=0x7fffffffd9e8) at debug_demo.cpp:11
11        while (i<=N) {
1: i = 6
2: sum = 15
(gdb) i watch
Num     Type           Disp Enb Address            What
2       hw watchpoint  keep y                      i
        breakpoint already hit 1 time
(gdb) b 16
Breakpoint 3 at 0x5555555551b1: file debug_demo.cpp, line 16.
(gdb) jump 16 
Continuing at 0x5555555551b1.

Breakpoint 3, main (argc=1, argv=0x7fffffffd9e8) at debug_demo.cpp:16
16        cout<<"sum = "<<sum<<endl;
1: i = 1
2: sum = 0
```


#### C语言结构

##### include头文件包含
- `#include`的意思是头文件包含，`#include <stdio.h>`代表包含stdio.h这个头文件
- 使用C语言库函数需要提前包含库函数对应的头文件，如这里使用了`printf()`函数，需要包含stdio.h头文件
- 可以通过man 3 printf查看printf所需的头文件
- `#include`”，它的作用是“包含文件”。注意，不是“包含头文件”，而是可以包含任意的文件。使用“#include”可以把源码、普通文本，甚至是图片、音频、视频都引进来。



`#include<>`  与 `#include ""` 的区别：
- `< >` 表示系统直接按**系统指定**的目录检索
- `""` 表示系统先在 "" 指定的路径(没写路径代表当前路径)查找头文件，如果找不到，再按系统指定的目录检索
- 编写一些代码片段，存进“`*.inc`”文件里，然后有选择地加载，用得好的话，可以实现“源码级别的抽象”。



```shell
cd /usr/include/
$ pwd
/usr/include
$ ls -l | grep stdio.h
-rw-r--r--  1 root root  31526 Jul 14 02:07 stdio.h
```

##### main函数
- 一个完整的C语言程序，是由一个、且只能有一个`main()`函数(又称主函数，必须有)和若干个其他函数结合而成(可选)。
- main函数是C语言程序的入口，程序是从main函数开始执行。

##### {} 括号，程序体和代码块
- {}叫<font color=#ff0000>代码块</font>，一个代码块内部可以有一条或者多条语句
- C语言每句可执行代码都是";"分号结尾
- 所有的#开头的行，都代表<font color=#ff0000>预编译</font>指令，预编译指令行结尾是没有分号的
- 所有的<font color=#ff0000>可执行语句</font>必须是在代码块里面

##### 注释
- `//`叫行注释，注释的内容编译器是**忽略**的，注释主要的作用是在代码中加一些说明和解释，这样有利于代码的阅读
- `/**/`叫块注释
- 块注释是C语言标准的注释方法
- 行注释是从C++语言借鉴过来的

##### printf函数
- printf是C语言库函数，功能是向<font color=#ff0000>标准输出设备</font>输出一个<font color=#ff0000>字符串</font>
- `printf(“hello world\n”)`;//`\n`的意思是回车换行

##### return语句
- return代表函数执行完毕，返回return代表<font color=#ff0000>函数的终止</font>
- 如果main定义的时候前面是int，那么return后面就需要写一个整数；如果main定义的时候前面是void，那么return后面什么也不需要写
- 在main函数中return 0代表程序执行成功，return -1代表程序执行失败
- `int main()`和`void main()`在C语言中是一样的，但C++只接受int main这种定义方式

#### system函数

```c
#include <stdlib.h>
int system(const char *command);
```

功能：在已经运行的程序中执行<font color=#ff0000>另外一个外部程序</font>
参数：外部可执行程序名字
返回值：
成功：0
失败：任意数字

执行系统命令。如：pause、cmd、calc(计算器)、mapoint(画图工具)、notepad(记事本) cls(清屏操作)… 

示例代码:
```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
	//system("calc"); //windows平台
	system("ls"); //Linux平台, 需要头文件#include <stdlib.h>
	return 0;
}
```


##  __attribute__的使用

`__attribute__` 可以设置<font color=#ff0000>函数属性</font>(Function Attribute )、<font color=#ff0000>变量属性</font>(Variable Attribute )和<font color=#ff0000>类型属性</font>(Type Attribute )。

### aligned
指定对象的对齐格式(以字节为单位)，如

```c++
struct S {
short b[3];
} __attribute__ ((aligned (8)));

typedef int int32_t __attribute__ ((aligned (8)));
```

该声明将强制编译器确保(尽它所能)变量类型为struct S 或者int32_t 的变量在分配空间时采用**8字节对齐方式**。
如果aligned 后面不紧跟一个指定的数字值，那么编译器将依据你的目标机器情况使用**最大最有益的对齐方式**

```C++
struct S {
short b[3];
} __attribute__ ((aligned));
```

### packed

使用该属性对`struct` 或者`union`类型进行定义，设定其类型的每一个变量的**内存约束**。就是告诉编译器**取消结构在编译过程中的优化对齐**(使用1字节对齐),按照实际占用字节数进行对齐，是GCC特有的语法。

```C++
struct unpacked_struct
{
      char c;
      int i;
};
         
struct packed_struct
{
     char c;
     int  i;
     struct unpacked_struct s;
}__attribute__ ((__packed__));
```

如

```c++
在TC下：struct my{ char ch; int a;}      sizeof(int)=2;sizeof(my)=3;(紧凑模式)

在GCC下：struct my{ char ch; int a;}     sizeof(int)=4;sizeof(my)=8;(非紧凑模式)

在GCC下：struct my{ char ch; int a;}__attrubte__ ((packed))        sizeof(int)=4;sizeof(my)=5
```

下面的例子中使用`__attribute__`属性定义了一些结构体及其变量，并给出了输出结果和对结果的分析。代码为：
```c++
struct p
{
    int a;
    char b;
    short c;

}__attribute__((aligned(4))) pp;

struct m
{
    char a;
    int b;
    short c;
}__attribute__((aligned(4))) mm;

struct o
{
    int a;
    char b;
    short c;
}oo;

struct x
{
    int a;
    char b;
    struct p px;
    short c;
}__attribute__((aligned(8))) xx;

int main()
{           
    printf("sizeof(int)=%d,sizeof(short)=%d.sizeof(char)=%d\n",sizeof(int)
                                                ,sizeof(short),sizeof(char));
    printf("pp=%d,mm=%d \n", sizeof(pp),sizeof(mm));
    printf("oo=%d,xx=%d \n", sizeof(oo),sizeof(xx));
    return 0;
}
```

输出结果：
```c++
sizeof(int)=4,sizeof(short)=2.sizeof(char)=1
pp=8,mm=12
oo=8,xx=24
```

## optimize

`_attribute__((optimize("ON")))`是GCC编译器用于指定优化级别的一种方式;
在debug我们可以使用`_attribute__((optimize("O0")))`告诉编译器在编译代码时禁用优化，即将优化级别设置为最低。