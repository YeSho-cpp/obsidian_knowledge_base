# Xmake编译工具

<img src="https://article.biliimg.com/bfs/article/1efb48265061a411b4851929902937ad38716159.png" alt="image.png" style="zoom:50%;" />

# 什么是Xmake 
- Xmake是一个基于[[lua|Lua]]的轻量级跨平台C/C++构建工具，使用xmake.lua维护项目构建。 
- 相比`makefile/CMakeLists.txt`,配置语法更加简洁直观，对新手非常友好，短时间内就能快速入门，让用户把更多的 
- 精力集中在实际的项目开发上。 

# Xmake的使用
## 常用命令
1. 在工程目录下使用xmake命令根据·进行编译，如果没有lua文件会自动创建。 
```shell
xmake 
```
2. build文件夹下存放着可执行文件（不同xmake.lua生成的可执行文件名称不一样） 
```shell
./build/linux/x86_64/debug/test
```

## xmake.lua的编写 

**一个简单的例子** 
```lua
target("hello") --生成的可执行文件名为hello 
	set_kind("binary") --编译生成二进制文件（可执行文件）
	add_files("src/hello_world.cpp") --添加编译的源代码文件 
	add_includedirs("./include")
```


**如何引用第三方库** 
```lua
add_requires("opencv") --包依赖描述 
	target("CV") 
	set_kind("binary") 
	add_packages("opencv") --导入相关包 5 
	add_files("main.cpp","recognition.cpp") 
```

- add_requires是一个官方的xmake**包管理仓库**，收录了常用的c/c++开发包，提供跨平台支持。 
	- 类似于Python的pip。 
	- 但是，毕竟xmake-repo比较小众，收录的包比较少，也容易出奇怪的bug,所以通常都会自己安装。 
	- 如果系统已经安装对应的库，add_requires会自动找到 
- add_packages 
	- 当编译目标文件时，如果package存在，那么其定义，头文件路径，链接库路径将被自动导入并链接 
	- 等价于手动`add_link`,`add_include_dir`相关路径 

**静态库程序**
```lua
target("library")
    set_kind("static")
    add_files("src/library/*.c")
	set_targetdir("./out")
target("test")
    set_kind("binary")
    add_files("src/*c")
    add_deps("library")
```

通过`add_deps`将一个静态库自动链接到test可执行程序。

完整例子请执行下面的命令来创建：

```bash
xmake create -l c -t static test
```

**动态库程序**

```lua
target("library")
    set_kind("shared")
    add_files("src/library/*.c")

target("test")
    set_kind("binary")
    add_files("src/*c")
    add_deps("library")
```

通过`add_deps`将一个动态库自动链接到test可执行程序。
完整例子请执行下面的命令来创建：
```bash
xmake create -l c -t shared test
```







