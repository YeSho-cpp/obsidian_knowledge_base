# Go语言特点

- 其他编程语言的弊端。
	- 硬件发展速度远远超过软件。
	- C语言等原生语言缺乏好的**依赖管理**(依赖头文件)。
	- Java和C++等语言过于笨重。
	- 系统语言对**垃圾回收**和**并行计算**等基础功能缺乏支持。
	- 对**多核计算机**缺乏支持。
 
## Golang的优势

- Go语言是一个可以**编译高效**,支持高并发的,面向垃圾回收的全新语言。
	- 秒级完成大型程序的单节点编译。
	- 依赖管理清晰。
	- 不支持继承,程序员无需花费精力定义不同类型之间的关系。
	- 支持垃圾回收,支持并发执行,支持多线程通讯。
	- 对多核计算机支持友好。

- Go 语言不支持的特性
	- 不支持函数重载和操作符重载
	- 为了避免在C/C++开发中的一些Bug和混乱,不支持隐式转换
	- 支持接口抽象,不支持继承(Go使用**组合和接口**来实现多态，而不是继承。)
	- 不支持动态加载代码(Go1.8引入了**插件系统**，允许有限形式的动态代码加载，但仅在某些平台上支持)
	- 不支持动态链接库(从 Go 1.5 开始，可以使用 `-buildmode=shared` 创建共享库)
	- 通过**recover**和**panic**来替代异常机制
	- 不支持传统的断言(可以使用`value.(Type)`语法进行接口类型断言),还有`switch v.(type)` 用于类型选择
	- 没有传统意义上的静态变量(使用包级变量来实现类似静态变量的功能)


- 极简单的部署方式
	- 可直接编译成**机器码**
	- 不依赖其他的库(是一个**静态**的(可执行)文本文件)
	- 直接运行即可部署(不依赖任何的库,可以直接生成)
- 静态类型语言
	- 编译的时候检查出来隐藏的大多数问题
- 语言层面的并发
	- 天生的基因支持
		```c++
		package main
		
		import (
			"fmt"
			"time"
		)
		func goFunc(i int){
			fmt.Println("goroutine ",i, " ....")
		}
		
		func main(){
			for i :=0;i<10000;i++{
				//这里开启了10000个协程就是10000个并行操作，不需要去关注时间片，资源分配等等
				go goFunc(i) //开启一个并发协程
			}
			time.Sleep(time.Second)
		}
		```
	- 充分利用多核
- 强大的标准库
	- runtime系统调度机制(做一些垃圾回收,go调度的平均分配以及其他的优化)
	- 高效的GC垃圾回收(新版本GC就加了**三色标记**和**混合写屏障**)
	- 丰富的标准库
		- 文本
		- 输入输出
		- 数据结构算法
		- 数学
		- 文件系统
		- 日期和时间
		- 加解密
		- 应用构建
		- email
		- 高效的GC垃圾回收
		- 进程、线程、Goroutine
		- debug
		- 测试
		- 压缩
		- 同步机制
		- 数据持久存储与交换
		- 网络通信
- 简单易学
	- 25个关键字
	- C语言简洁基因,内嵌C语法支持
	- 面向对象特征(继承、多态、封装)
	- 跨平台

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240804151812.png" alt="image.png" style="zoom:50%;" />


## Golang适合用来做什么(强项)
 
1. 云计算基础设施领域
	代表项目:docker、kubernetes、etcd、consul、cloudflareCDN、七牛云
2. 基础后端软件
	代表项目:tidb、influxdb、cockroachdb等。
3. 微服务
	代表项目:go-kit、micro、monzo bank的typhon、bilibilili等。
4. 互联网基础设施
	代表项目:以太坊、hyperledger等。

## Golang的不足(缺点)

1. 包管理,大部分包都在GitHub上。
2. 无泛化类型, GO2.0计划有加上(现在有了)。
3. 所有Excepiton都用Error来处理(比较有争议)。
4. 对C的降级处理,并非无缝,没有C降级到asm那么完美(序列化问题)

# go环境

## 环境命令

在终端输入go我们就能看到这里用法
```sh
go
```

1. 最常用的核心命令：
```sh
go run      - 编译并运行程序
go build    - 编译程序
go get      - 下载并安装包和依赖
go test     - 运行测试
go mod      - 管理模块
go fmt      - 格式化代码
```

2. 开发相关命令：
```sh
doc      - 查看文档
   例: go doc fmt.Println

env      - 查看Go环境变量
   例: go env GOPATH

vet      - 代码静态检查，发现可能的错误
   例: go vet main.go

clean    - 清理编译文件
   例: go clean
```

3. 模块管理相关：

```sh
mod 子命令：
- go mod init     - 初始化新模块
- go mod tidy     - 添加缺少的模块，移除无用模块
- go mod vendor   - 将依赖复制到vendor目录
- go mod download - 下载依赖到本地缓存
```

4. 工具链命令：
```sh
generate  - 通过处理源码生成Go文件
install   - 编译和安装包
list      - 列出包或模块信息
fix       - 更新包以使用新API
```

5. 帮助主题：
```sh
buildmode       - 构建模式说明
environment     - 环境变量说明
go.mod          - go.mod文件格式说明
modules         - 模块系统说明
packages        - 包管理说明
```

6. 常见使用场景：

```sh
1. 创建新项目：
   go mod init project-name
   
2. 运行程序：
   go run main.go

3. 编译程序：
   go build

4. 格式化代码：
   go fmt ./...

5. 运行测试：
   go test ./...

6. 添加依赖：
   go get github.com/xxx/yyy

7. 整理依赖：
   go mod tidy
```

7. 环境相关：
```sh
GOPATH  - Go工作空间路径
GOROOT  - Go安装路径
GOPROXY - Go模块代理
GOBIN   - 可执行文件安装路径
```

### go build

- Go语言不支持动态链接,因此编译时会将所有依赖编译进同一个二进制文件。
- 指定输出目录。
	- `go build -o bin/mybinary.`
- 常用环境变量设置编译操作系统和CPU架构。
	- `GOOS=linux GOARCH=amd64 go build`
- 全支持列表。
	- `$GOROOT/src/go/build/syslist.go`


### Go test
Go语言原生自带测试

```go
import (
	"testing"

	"github.com/stretchr/testify/assert"
)

func add(a, b int) int {
	return a + b
}
func TestIncrease(t *testing.T) {
	t.Log("Start testing")
	result := add(1, 2)
	assert.Equal(t, result, 3)
}

```

- `go test ./...-v` 运行测试
- go test命令扫描所有`*_test.go`为结尾的文件,惯例是将测试代码与正式代码放在同目录,
- 如foo.go的测试代码一般写在`foo_test.go`
```sh
yesho@:~/go/src/github.com/cncamp/golang/examples/module1/callbacks$ go test -v
=== RUN   TestIncrease
    main_test.go:13: Start testing
--- PASS: TestIncrease (0.00s)
PASS
```

### Go vet
代码静态检查，发现可能的 bug 或者可疑的构造
- Print-format 错误，检查类型不匹配的print
```go
str := “hello world!”
fmt.Printf("%d\n", str)
```

- Boolean 错误，检查一直为 true、false 或者冗余的表达式

```go
fmt.Println(i != 0 || i != 1)
```


- Range 循环，比如如下代码主协程会先退出，go routine无法被执行

```go
words := []string{"foo", "bar", "baz"}
for _, word := range words {
	go func() {
		fmt.Println(word).
	}()
}
```

- Unreachable的代码，如 return 之后的代码
- 其他错误，比如变量自赋值，error 检查滞后等

```go
res, err := http.Get("https://www.spreadsheetdb.io/")
defer res.Body.Close()
if err != nil {
	log.Fatal(err)
}
```

### Golang playground

官方 playground
https://play.golang.org/

可直接编写和运行 Go 语言程序

国内可直接访问的 playground
https://goplay.tools/

# go基本语法

## 格式化IO


```go
package main // 表示当前程序所在的包

// ()可以支持导入多个包
import (
	"fmt"
	"time"
)

// main函数 
func main() { // go语言要求 { 一定跟函数在一行
	// golong 中的表达式,加”;“没区别
	fmt.Println(" hello Go!")

	time.Sleep(1*time.Second)
}
```

- `import "fmt"`:导入一个格式化的包 要用于格式化 I/O（输入和输出）。fmt 包提供了格式化字符串输出、输入读取和一些格式化工具函数，常用于打印输出、读取输入和格式化字符串。
	- **基本打印函数**：
		- `fmt.Print`：直接打印参数。
		- `fmt.Println`：打印参数，并在结尾添加换行符。
		- `fmt.Printf`：格式化打印参数，根据提供的格式字符串进行格式化。
		- `fmt.Fprintf`：将格式化的文本写入指定的 io.Writer（如文件、网络连接、HTTP响应等）
	- **格式化字符串**：
		- `fmt.Sprintf`：格式化字符串并返回结果，而不是直接打印。
		- `fmt.Sprintf`：与 `fmt.Printf` 类似，但返回格式化后的字符串而不是直接输出。
	- **基本读取函数**：
		- `fmt.Scan`：从标准输入读取参数。
		- `fmt.Scanln`：从标准输入读取参数，遇到换行符结束。
		- `fmt.Scanf`：根据格式字符串从标准输入读取参数。
	- 格式化工具函数
		- **字符串格式化**：
		    - `fmt.Sprintf`：格式化字符串并返回。
		    - `fmt.Errorf`：创建一个格式化的错误。
	- **格式化标志**：
		- `%v`：默认格式打印。
		- `%+v`：在结构体中，打印字段名和值。
		- `%#v`：打印Go语言语法表示的值。
		- `%T`：打印值的类型。
		- `%%`：打印百分号。
		- `%w`：用于错误包装，保留原始错误信息，支持错误链传递和解包（Go 1.13+）。
			- 常与 `fmt.Errorf` 配合使用
			- 可通过 `errors.Unwrap` 获取原始错误
			- 支持 `errors.Is` 和 `errors.As` 进行错误检查
- go语言要求 `{` 一定跟函数在一行 否则编译错误
- `package main`//程序的包名

```go
func main() {
    // 准备示例数据
    p := Person{
        Name: "张三",
        Age:  25,
    }
    
    // 1. %v - 默认格式
    fmt.Printf("%v\n", p)
    // 输出: {张三 25}
    
    // 2. %+v - 包含字段名
    fmt.Printf("%+v\n", p)
    // 输出: {Name:张三 Age:25}
    
    // 3. %#v - Go语法格式
    fmt.Printf("%#v\n", p)
    // 输出: main.Person{Name:"张三", Age:25}
    
    // 4. %T - 类型信息
    fmt.Printf("%T\n", p)
    // 输出: main.Person
    
    // 5. %% - 打印百分号
    fmt.Printf("比例: 85%%\n")
    // 输出: 比例: 85%

    // 综合示例
    slice := []int{1, 2, 3}
    fmt.Printf("默认格式: %v\n", slice)      // 输出: 默认格式: [1 2 3]
    fmt.Printf("Go语法格式: %#v\n", slice)   // 输出: Go语法格式: []int{1, 2, 3}
    fmt.Printf("类型信息: %T\n", slice)      // 输出: 类型信息: []int

    // fmt.Fprintf 示例
    // 1. 写入到文件
    file, _ := os.Create("test.txt")
    defer file.Close()
    fmt.Fprintf(file, "用户信息：%+v\n", p)  // 写入到文件

    // 2. 写入到 bytes.Buffer
    var buf bytes.Buffer
    fmt.Fprintf(&buf, "姓名：%s，年龄：%d\n", p.Name, p.Age)
    fmt.Println("Buffer内容:", buf.String())  // 输出：Buffer内容: 姓名：张三，年龄：25

    // 3. HTTP响应示例
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, "欢迎访问，路径是：%s", r.URL.Path)
    })

}
```

```go
// %w 的使用示例
func main() {
    // 1. 基本错误包装
    originalErr := errors.New("数据库连接失败")
    wrappedErr := fmt.Errorf("查询用户信息时出错: %w", originalErr)
    fmt.Printf("完整错误: %v\n", wrappedErr)
    // 输出: 完整错误: 查询用户信息时出错: 数据库连接失败

    // 2. 错误链示例
    err1 := errors.New("端口被占用")
    err2 := fmt.Errorf("服务启动失败: %w", err1)
    err3 := fmt.Errorf("系统初始化失败: %w", err2)
    
    fmt.Printf("错误链: %v\n", err3)
    // 输出: 错误链: 系统初始化失败: 服务启动失败: 端口被占用

    // 3. 解包错误
    original := errors.Unwrap(wrappedErr)
    fmt.Printf("解包后的原始错误: %v\n", original)
    // 输出: 解包后的原始错误: 数据库连接失败

    // 4. 与 %v 对比
    err := errors.New("原始错误")
    fmt.Printf("使用%%v: %v\n", fmt.Errorf("出错: %v", err))  // 不可解包
    fmt.Printf("使用%%w: %v\n", fmt.Errorf("出错: %w", err))  // 可以解包
}
```

## 变量的声明

```go
package main

import(
	"fmt"
)
/*
	四种变量的声明方式
*/

// 声明全局变量 方法一、方法二、方法三、方法是可以的
var gA int = 100
var gB=200

// 用方法四声明全局变量
// := 只能用在函数体内来声明
// gC:=200

func main()  {
	// 方法一：声明一个变量 默认值是0
	var a int // var用于声明一个或多个变量
	fmt.Println("a= ",a)	

	// 方法二： 声明一个变量，初始化一个值
	var b int =100
	fmt.Println("b= ",b)	
	fmt.Printf("type of b = %T\n",b)

	var bb string = "abcd"
	fmt.Printf("bb = %s, type of bb = %T\n",bb,bb)

	// 方法三：在初始化的时候，可以省去数据类型，通过值自动匹配当前变量的数据类型
	var c=100
	fmt.Println("c =",c);
	fmt.Printf("type of c = %T\n",c)

	var cc= "abcd"

	fmt.Printf("cc = %s, type of cc =%T\n",cc,cc)

	// 方法四： 省去var关键字，直接自动匹配  := 是用于声明并初始化变量的简写操作符
	e :=100
	fmt.Println("e= ",e)
	fmt.Printf("type of e =%T\n",e)

	g :=3.14
	fmt.Println("g= ",g)
	fmt.Printf("type of g =%T\n",g)


	// ====
	fmt.Println("gA = ",gA,",gB = ",gB)
	// fmt.Println("gC =",gC)

	// 声明多个变量
	var xx,yy int=100,200
	fmt.Println("xx =",xx,", yy=",yy)
	var kk,ll=100,"Aceld"
	fmt.Println("kk = ",kk," , ll = ",ll)


	var(
		vv int=100
		jj bool=true
	)

	fmt.Println("vv= ",vv,", jj=",jj)

	// 声明但无使用
	var _ Handler = &HandlerBasedOnMap{} // `var _` 使用了空标识符（下划线），它表示我们声明了一个变量但不会使用它。
}

```

- 局部变量的声明
	1. 声明一个变量 默认值是0 `var int a`
	2. 声明一个变量，初始化一个值 `var int b=100`
	3. 在初始化的时候，可以省去数据类型，通过值自动匹配当前变量的数据类型 `var c="sdf"`
	4. 省去var关键字，直接自动匹配  `:=` 是用于声明并初始化变量的简写操作符 `d:=12`
- 全局变量的声明
	1. 方法四不支持全局
- 多行变量的声明
	- 单行写法 
		- `var xx,yy int =100,200`
		- `var kk,ll =100,"Aceld"`
	- 多行写法 
		- var(
				vv int=100
				jj bool=true
			)

## const与iota

### 常量（const）

在Go语言中，`const`用于定义常量。常量的值在**编译时就已经确定**，并且在程序运行时无法修改。常量是只读的，无法通过任何方式修改其值。

```go
const length int = 10
fmt.Println("length is", length)
```

常量可以用于初始化数组的大小：

```go
const size = 10
var arr [size]int
```

### iota

`iota`是Go语言中的一个常量生成器，可以在每一个`const`声明块中使用。`iota`从0开始，逐行递增1。

```go
const (
    BEIJING  = iota // 0
    SHANGHAI       // 1
    SHENZHEN       // 2
)
fmt.Println(BEIJING, SHANGHAI, SHENZHEN) // 输出：0 1 2

```

- iota与表达式
	`iota`可以与表达式结合使用，以生成更复杂的常量值：

	```go
	const (
	    a, b = iota + 1, iota + 2 // 0+1=1, 0+2=2
	    c, d                      // 1+1=2, 1+2=3
	    e, f                      // 2+1=3, 2+2=4
	    g, h = iota * 2, iota * 3 // 3*2=6, 3*3=9
	)
	fmt.Println(a, b, c, d, e, f, g, h) // 输出：1 2 2 3 3 4 6 9
	```

- 使用注意事项
	`iota`只能在`const`声明块中使用，在其他地方（如变量声明中）使用`iota`会导致编译错误：


```go
// 错误示例
var a int = iota // 编译错误：iota只能在常量声明中使用
```


要写返回值的3种写法的文案，帮我写下

```go
package main
import "fmt"

func fool1(a string, b int) int {
	fmt.Println("------fool1------")
	fmt.Println("a is", a)
	fmt.Println("b is", b)
	c:=100
	return c
}

//返回多个返回值 匿名
func fool2(a string, b int) (int, int) {
	fmt.Println("------fool2------")
	fmt.Println("a is", a)
	fmt.Println("b is", b)
	c:=100
	d:=200
	return c,d
}

//返回多个返回值 命名
func fool3(a string, b int)(r1 int, r2 int) {
	fmt.Println("------fool3------")
	fmt.Println("a is", a)
	fmt.Println("b is", b)

	// r1,r2属于fool3的形参 默认初始值为0
	fmt.Println("r1 is", r1) // 
	fmt.Println("r2 is", r2)

	r1=100
	r2=200
	return
}


func fool4(a string, b int)(r1,r2 int){
	fmt.Println("------fool4------")
	fmt.Println("a is", a)
	fmt.Println("b is", b)
	r1=100
	r2=200
	retun
```


## 控制结构

### If

- 基本形式
```go
if condition1 {
	// do something
} else if condition2 {
	// do something else
} else {
	// catch-all or default
}
```

- if 的简短语句
	- 同 for 一样， if 语句可以在条件表达式前执行一个简单的语句。

```go
if v := x - 100; v < 0{
	return v
}
```

### switch

```go
switch var1 {
	case val1: //空分支
	case val2:
	fallthrough //执行case3中的f()
	case val3:
	f()
	default: //默认分支
	//...
}
```

### For

Go 只有一种循环结构：for 循环。
- 计入计数器的循环
```go
for 初始化语句; 条件语句; 修饰语句 {}
	for i := 0; i < 10; i++ {
		sum += i
	}
```
- 初始化语句和后置语句是可选的，此场景与 while 等价（Go 语言不支持 while）
```go
for ; sum < 1000; {
	sum += sum
}
```

- 无限循环
```go
for {
	if condition1 {
		break
	}
}
```

### for-range

遍历数组，切片，字符串，Map 等
```go
for index, char := range myString {
	...
}
for key, value := range MyMap {
	...
}
for index, value := range MyArray {
	...
}
// 如果只需要 index 也可以去掉 写成 
for index := range arr {  
    fmt.Printf("only index: %d \n", index)  
}
```

需要注意：如果 for range 遍历指针数组，则 value 取出的指针地址为原指针地址的**拷贝**。

## 数值与类型转换

### 数值类型

1. 整数类型
```go
// 有符号整数
int8    // -128 到 127
int16   // -32768 到 32767
int32   // -2^31 到 2^31-1
int64   // -2^63 到 2^63-1
int     // 32位或64位，取决于系统

// 无符号整数
uint8   // 0 到 255
uint16  // 0 到 65535
uint32  // 0 到 2^32-1
uint64  // 0 到 2^64-1
uint    // 32位或64位，取决于系统

// 特殊整数类型
byte    // uint8的别名
rune    // int32的别名，表示Unicode码点
uintptr // 存储指针的无符号整数
```

2. 浮点数类型：
```go
float32     // IEEE-754 32位浮点数
float64     // IEEE-754 64位浮点数

// 示例
var f1 float32 = 3.14
var f2 float64 = 3.141592653589793
```

3. 复数类型：
```go
complex64   // 32位实数和虚数
complex128  // 64位实数和虚数

// 示例
var c1 complex64 = 3.2 + 12i
var c2 complex128 = 3.2 + 12i
```

4. 布尔类型：
```go
bool    // true 或 false

// 示例
var b1 bool = true
var b2 bool = false
```

5. 字符串类型：
```go
string  // UTF-8字符串

// 示例
var s1 string = "hello"
var s2 string = `多行
字符串`
```

6. 指针类型
```go
// 普通指针
*T          // 指向类型T的指针

// 特殊指针类型
unsafe.Pointer  // 通用指针类型，类似C的void*
                // 可以存储任何变量的地址
                // 可以在不同指针类型间转换

// 指针相关的无符号整数
uintptr         // 用于指针运算的整数类型
```


### 指针类型详解及示例

- 普通指针操作

	```go
	var i int = 42
	// 普通指针
	ptr := &i
	fmt.Printf("值: %d, 地址: %p\n", *ptr, ptr)
	```

- unsafe.Pointer使用场景
	```go
	// 1. 不同类型指针间的转换
	var f float64 = 3.14
	// float64指针 -> unsafe.Pointer -> uint64指针
	bits := (*uint64)(unsafe.Pointer(&f))
	
	// 2. 与uintptr配合进行指针运算
	type MyStruct struct {
	    a int64
	    b int64
	}
	s := MyStruct{1, 2}
	// 获取字段b的地址
	bPtr := unsafe.Pointer(uintptr(unsafe.Pointer(&s)) + unsafe.Offsetof(s.b))
	b := *(*int64)(bPtr)
	
	// 3. 切片和字符串的零拷贝转换
	func stringToBytes(s string) []byte {
	    return *(*[]byte)(unsafe.Pointer(
	        &reflect.SliceHeader{
	            Data: (*reflect.StringHeader)(unsafe.Pointer(&s)).Data,
	            Len:  len(s),
	            Cap:  len(s),
	        }))
	}
	```

- unsafe.Pointer的合法转换规则

	```go
	// 1. *T1 -> unsafe.Pointer -> *T2
	// 2. unsafe.Pointer <-> uintptr
	// 3. pointer -> unsafe.Pointer -> pointer
	// 4. uintptr -> unsafe.Pointer (特定场景)
	
	var x int64 = 42
	// *int64 -> unsafe.Pointer
	p := unsafe.Pointer(&x)
	// unsafe.Pointer -> *int32
	y := (*int32)(p)
	```

- 指针类型的零值
	```go
	var (
	    normalPtr *int           // nil
	    unsafePtr unsafe.Pointer // nil
	    addr     uintptr        // 0
	)
	```


### 类型变换

- 类型转换
	- 表达式 T(v) 将值 v 转换为类型 T。
		- 一些关于数值的转换：
		```go
		var i int = 42
		var f float64 = float64(i)
		var u uint = uint(f)
		```
	- 或者，更加简单的形式：
		```go
		i := 42
		f := float64(i)
		u := uint(f)
		```

- 类型推导
	- 在声明一个变量而不指定其类型时（即使用不带类型的 := 语法或 var = 表达式语法），变量的类型由右值推导得出。
		```go
		var i int
		j := i // j 也是一个 int
		```


# 数据结构

## 数组

- 相同类型且长度固定连续内存片段
- 以编号访问每个元素
- 定义方法
	- `var identifier [len]type`
- 示例
- `myArray := [3]int{1,2,3}`


## 切片(slice)

- 切片是对数组一个连续片段的引用
- 数组定义中不指定长度即为切片
	```go
	var identifier []type
	```

- 切片在未初始化之前默认为nil， 长度为0

### 常用方法


#### Make和New

- new创建一个该类型的**实例**,并且返回指向该实例的**指针**。new函数是内建函数,函数定义:`func new(Type) *Type`
	- 使用new函数来分配空间
	- 传递给new函数的是一个**类型**,而不是一个值
	- 返回值是指向这个新分配的地址的指针

- Make 返回初始化后的实例本身，可预设内存空间，避免未来的内存拷贝

make的作用是为slice,map or chan的初始化然后返回引用make函数是内建函数,函数定义: 
`func make(t Type, size ...IntegerType) Type` 
`make(T,args)`函数的目的和new(T)不同仅仅用于创建slice,map,channel而且返回类型是**实例**



示例
```go
mySlice1 := new([]int)
mySlice2 := make([]int, 0)
mySlice3 := make([]int, 10)
mySlice4 := make([]int, 10, 20)
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241108223413.png" alt="image.png" style="zoom:60%;" />



```go
func main() {
    // 使用 new 创建一个指向 nil 切片的指针
    slice := new([]int)

    // 由于指针初始值为 nil,我们需要先使用 make 为其分配内存
    *slice = make([]int, 3, 5)

    // 通过索引访问并修改切片元素
    (*slice)[0] = 10
    (*slice)[1] = 20
    (*slice)[2] = 30

    fmt.Println("Slice:", *slice)  // 输出: Slice: [10 20 30]
    fmt.Println("Length:", len(*slice))  // 输出: Length: 3
    fmt.Println("Capacity:", cap(*slice)) // 输出: Capacity: 5
}
```


1. 直接初始化
	通过直接给定值来声明和初始化一个`slice`：
	```go
	slice1 := []int{1, 2, 3}
	fmt.Printf("slice1=%v, len=%d\n", slice1, len(slice1))
	```
	- `slice1` 被声明为一个包含三个元素的`slice`，元素值为 `1, 2, 3`。
	- 使用 `len(slice1)` 获取 `slice` 的长度。

2. 零值切片

	声明一个未初始化的`slice`，其值为零值：

	```go
	var slice2 []int
	fmt.Printf("len=%d, slice=%v\n", len(slice2), slice2)
	```

	- `slice2` 被声明为一个整数类型的`slice`，但未进行初始化。
	- 未初始化的`slice` 的长度为 0。
	
	尝试对未初始化的`slice`赋值会导致运行时错误，例如：

	```go
		// slice2[0] = 2 // 这行代码会引发错误
	```

3. 使用`make`函数

	使用 `make` 函数来创建一个指定长度和容量的`slice`：
	```go
	var slice3 []int
	slice3 = make([]int, 3, 5) // 3是长度，5是容量
	fmt.Printf("len=%d, slice=%v, cap=%d\n", len(slice3), slice3, cap(slice3))
	```

	- `slice3` 被声明并使用 `make` 函数初始化，长度为 3，容量为 5。

	另一种常见的 `make` 函数用法，不指定容量：
	
	```go
	slice4 := make([]int, 3)
	fmt.Printf("len=%d, slice=%v\n", len(slice4), slice4)
	```

	- `slice4` 被声明并初始化，长度为 3，初始值为 0。

4. 判断`slice`是否为空

	通过比较 `slice` 与 `nil` 来判断一个`slice`是否为空：
	```go
	if slice2 == nil {
	    fmt.Println("slice2 是一个空切片")
	} else {
	    fmt.Println("slice2 不是一个空切片")
	}
	```

	- 如果一个`slice`未被初始化或已被赋值为`nil`，则它被认为是空切片。
	
	这些方法展示了如何在Go语言中声明和初始化不同类型的`slice`。`slice`提供了灵活的方式来处理动态数组，使得Go语言在处理数据集合时更加高效和简洁。


#### 切片截取

可以通过设置下限及上限来设置截取切片_`[lower-bound:upper-bound]`，实例如下：

```go
package main

import "fmt"


func main() {
   /* 创建切片 */
   numbers := []int{0,1,2,3,4,5,6,7,8}   
   printSlice(numbers)


   /* 打印原始切片 */
   fmt.Println("numbers ==", numbers)


   /* 打印子切片从索引1(包含) 到索引4(不包含)*/
   fmt.Println("numbers[1:4] ==", numbers[1:4])


   /* 默认下限为 0*/
   fmt.Println("numbers[:3] ==", numbers[:3])


   /* 默认上限为 len(s)*/
   fmt.Println("numbers[4:] ==", numbers[4:])


   numbers1 := make([]int,0,5)
   printSlice(numbers1)


   /* 打印子切片从索引  0(包含) 到索引 2(不包含) */
   number2 := numbers[:2]
   printSlice(number2)


   /* 打印子切片从索引 2(包含) 到索引 5(不包含) */
   number3 := numbers[2:5]
   printSlice(number3)


}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```

执行以上代码输出结果为：

```sh
len=9 cap=9 slice=[0 1 2 3 4 5 6 7 8]
numbers == [0 1 2 3 4 5 6 7 8]
numbers[1:4] == [1 2 3]
numbers[:3] == [0 1 2]
numbers[4:] == [4 5 6 7 8]
len=0 cap=5 slice=[]
len=2 cap=9 slice=[0 1]
len=3 cap=7 slice=[2 3 4]
```


#### append() 和 copy() 函数

如果想增加切片的容量，我们必须创建一个新的更大的切片并把原分片的内容都拷贝过来。
下面的代码描述了从拷贝切片的 copy 方法和向切片追加新元素的 append 方法。

```go
package main


import "fmt"


func main() {
   var numbers []int
   printSlice(numbers)


   /* 允许追加空切片 */
   numbers = append(numbers, 0)
   printSlice(numbers)


   /* 向切片添加一个元素 */
   numbers = append(numbers, 1)
   printSlice(numbers)


   /* 同时添加多个元素 */
   numbers = append(numbers, 2,3,4)
   printSlice(numbers)


   /* 创建切片 numbers1 是之前切片的两倍容量*/
   numbers1 := make([]int, len(numbers), (cap(numbers))*2)


   /* 拷贝 numbers 的内容到 numbers1 */
   copy(numbers1,numbers)
   printSlice(numbers1)   
}


func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```

- copy的使用超过这个容量是不会复制的

#### 切片排序

1. 基本用法：
   - Go 标准库提供了 `sort` 包来处理排序。
   - 从 Go 1.21 开始，还引入了更方便的 `slices` 包。

2. 使用 `sort` 包：
   - 对于基本类型（如 int, float64, string），可以直接使用：
     ```go
     sort.Ints(slice)        // 排序 int 切片
     sort.Float64s(slice)    // 排序 float64 切片
     sort.Strings(slice)     // 排序 string 切片
     ```

   - 对于自定义类型或更复杂的排序逻辑，可以使用 `sort.Slice()`:
     ```go
     sort.Slice(slice, func(i, j int) bool {
         return slice[i] < slice[j]  // 自定义比较逻辑
     })
     ```

3. 使用 `slices` 包（Go 1.21+）：
   - 提供了更简洁的 API：
     ```go
     slices.Sort(slice)  // 可用于多种类型的切片
     ```

   - 也支持自定义比较函数：
     ```go
     slices.SortFunc(slice, func(a, b YourType) bool {
         return a.Value < b.Value  // 自定义比较逻辑
     })
     ```

4. 稳定排序：
   - 如果需要保持相等元素的原始顺序，可以使用稳定排序：
     ```go
     sort.Stable(sort.IntSlice(slice))  // 使用 sort 包
     slices.SortStable(slice)           // 使用 slices 包（Go 1.21+）
     ```

5. 降序排序：
   - 标准库默认是升序排序。
   - 要降序排序，可以反转比较逻辑：
     ```go
     sort.Slice(slice, func(i, j int) bool {
         return slice[i] > slice[j]  // 降序
     })
     ```

6. 部分排序：
   - 如果只需要排序切片的一部分，可以使用切片操作：
     ```go
     sort.Ints(slice[start:end])
     ```

7. 检查是否已排序：
   - 可以使用 `sort.IsSorted()` 或 `slices.IsSorted()` 来检查切片是否已排序。

8. 性能考虑：
   - Go 的排序实现通常使用快速排序的变种，对于大多数情况性能很好。
   - 对于已经部分排序的数据，它会自动切换到其他算法以提高效率。

9. 并发排序：
   - 标准库的排序函数不是并发安全的。如果需要并发排序，需要自己实现或使用第三方库。

### 切片底层

```go
// slice的底层结构
type slice struct {
    array unsafe.Pointer // 指向底层数组的指针
    len   int           // 切片长度
    cap   int           // 切片容量
}
```

切片扩容机制：

```go
func growthExample() {
    s := make([]int, 0)
    
    // Go 1.18之前的扩容规则：
    // - 当新容量小于1024时，新容量将扩大为原来的2倍
    // - 当新容量大于等于1024时，新容量将扩大为原来的1.25倍
    
    // Go 1.18之后的扩容规则：
    // - 当前容量小于256时，新容量为原容量的2倍
    // - 当前容量大于等于256时，新容量将增加 (当前容量+3*256)/4
    
    for i := 0; i < 10; i++ {
        s = append(s, i)
        fmt.Printf("len=%d, cap=%d\n", len(s), cap(s))
    }
}
```

共享底层数组：

```go
func shareArrayExample() {
    // 多个切片共享底层数组
    arr := []int{1, 2, 3, 4, 5}
    s1 := arr[1:3]    // [2,3]
    s2 := arr[2:4]    // [3,4]
    
    // 修改s1会影响s2和arr
    s1[1] = 30        // arr = [1,2,30,4,5]
                      // s2 = [30,4]

    // 解决方法：使用copy创建新的切片
    s3 := make([]int, len(s1))
    copy(s3, s1)      // s3是独立的切片
}
```

常见陷阱：

```go
func commonPitfalls() {
    // 1. append可能导致新建数组
    s1 := make([]int, 3, 4)
    s2 := s1
    s1 = append(s1, 1, 2) // s1指向新数组，s2仍指向旧数组
    
    // 2. 切片作为参数传递
    func modify(s []int) {
        s[0] = 100        // 会修改原切片
        s = append(s, 1)  // 不会影响原切片
    }
    
    // 3. range遍历时的值复制
    s := []int{1, 2, 3}
    for i, v := range s {
        s[i] = v + 1      // 正确
        v = v + 1         // 无效，v是副本
    }
}
```

## map

### 声明方式
map和slice类似，只不过是数据结构不同，下面是map的一些声明方式。

在Go语言中，`map`是一种键值对的数据结构，类似于其他编程语言中的哈希表或字典。与`slice`不同，`map`可以通过键快速地查找和更新数据。以下是几种常见的`map`声明和使用方式。

1. 声明并初始化`map`

**第一种声明方式**

```go
var test1 map[string]string
// 在使用map前，需要先make，make的作用是给map分配数据空间
test1 = make(map[string]string, 10) 
test1["one"] = "php"
test1["two"] = "golang"
test1["three"] = "java"
fmt.Println(test1) // map[two:golang three:java one:php]
```

在这里，我们首先声明了一个名为`test1`的`map`，键和值的类型都是字符串。然后通过`make`函数分配空间，并添加了三个键值对。

**第二种声明方式**

```go
test2 := make(map[string]string)
test2["one"] = "php"
test2["two"] = "golang"
test2["three"] = "java"
fmt.Println(test2) // map[one:php two:golang three:java]

```

第二种声明方式更加简洁，直接使用`make`函数创建并初始化了`map`。

**第三种声明方式**

```go
test3 := map[string]string{
    "one": "php",
    "two": "golang",
    "three": "java",
}
**fmt.Println(test3)** // map[one:php two:golang three:java]

```

### 访问 Map 元素

```go
// 按 Key 取值
value, exists := myMap["a"]
if exists {
	println(value)
}

// 遍历 Map
for k, v := range myMap {
	println(k, v)
}
```

### map方法

在Go语言中，`map`是一种强大且灵活的数据结构，用于存储键值对。与`slice`不同，`map`允许通过键快速访问值。下面是一些关于`map`的常见使用方式的示例和解释。

1. 添加元素

使用语法 `map[key] = value` 向`map`中添加元素。例如：

```go
cityMap["no1"] = "北京"
cityMap["no2"] = "天津"
cityMap["no3"] = "上海"
```

2. 遍历`map`

使用 `for range` 语法遍历`map`中的所有键值对
```go
for k, v := range cityMap {
    fmt.Printf("k=%v v=%v\n", k, v)
}
```

3. 删除元素

使用 `delete` 函数从`map`中删除指定的键值对：
```go
delete(cityMap, "no1")
```

4. 更新元素

直接通过键访问并修改对应的值:

```go
cityMap["no2"] = "武汉"
```

5. 引用传递

在Go语言中，`map`是通过引用传递的，这意味着在函数中对`map`进行的修改会影响到原始`map`。例如：
```go
func printMap(cityMap map[string]string) {
    cityMap["no4"] = "广州"
    for k, v := range cityMap {
        fmt.Printf("func inner k=%v v=%v\n", k, v)
    }
}
```

7. 判断是元素是否存在

当从 map（映射）中获取一个值时，可以同时得到两个返回值：第一个是实际的值（`value`），第二个是一个布尔值（在这里被命名为 `exists`），用来表示所获取的键是否存在。
```go
myMap := make(map[string]string,10)
myMap["a"]="b"
value,exists :=myMap["a"]
```

### map底层

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241108231211.png?" alt="image.png" style="zoom:60%;" />
1. 主结构(hmap)：

```go
type hmap struct {
    count    int    // 当前map中的元素个数，图中显示20个
    B        uint8  // 桶(bucket)数量的幂数，图中为2，表示有2^2=4个桶
    buckets  unsafe.Pointer // 指向桶数组的指针，图中指向地址0xc0000ca018
    // ... 其他字段
}
```

2. 桶的组成(bmap)：
每个桶包含以下部分：
- tophash：存储hash值的高8位，用于快速比较，图中蓝色部分
- keys：存储key值，图中米色部分
- elems：存储实际的value值，图中蓝色部分
- overflow：指向溢出桶的指针，图中底部的细条

3. 桶的布局
```go
type bmap struct {
    tophash [8]uint8     // 每个桶可以存储8个键值对
    keys    [8]keytype   // 8个key连续存储
    values  [8]elemtype  // 8个value连续存储
    overflow *bmap       // 溢出桶指针 实际上就是一个新的bmap结构
}
```

4. 整体结构：

- 图中显示有4个桶(2^2)，编号从0到3
- 每个桶都是一个bmap结构
- 每个桶可以存储8个键值对
- 当一个桶装满时，会使用overflow桶链接新的桶

5. 内存布局：

- key和value是分开存储的，而不是key/value pair紧挨着存储
- 这种设计有利于内存对齐，提高访问效率
- tophash数组用于快速判断key是否存在，避免不必要的key比较

6. 特点总结：

- 使用桶（bucket）设计来管理hash冲突
- 每个桶固定存储8个键值对
- key/value分离存储，有利于内存对齐
- 使用tophash加速查找过程
- 采用溢出桶处理桶已满的情况

查找溢出桶中的key实际上是一个遍历链表的过程

```go
// 简化的查找过程
func mapAccess(key interface{}) interface{} {
    // 1. 计算hash值
    hash := hashFunc(key)
    
    // 2. 定位到初始桶的位置
    bucketIndex := hash & (2^B - 1)    // O(1)
    bucket := buckets[bucketIndex]      // O(1)
    
    // 3. 获取tophash值(hash值的高8位)
    topHash := hash >> (64-8)           // O(1)
    
    // 4. 在桶链中查找
    for b := bucket; b != nil; b = b.overflow {  // 关键在这个循环
        // 在当前桶中查找
        for i := 0; i < 8; i++ {
            if b.tophash[i] == topHash {
                // key的完整比较
                if b.keys[i] == key {
                    return b.values[i]
                }
            }
        }
        // 如果当前桶没找到，继续查找overflow桶
    }
    return nil
}
```

> [!note] 注意
> 
如果并发读写map会导致panic，因为map里面有标志位


## STL

### 字符串

#### rune，byte的区别

> Golang中不能直接对字符串string进行下标操作，都是利用rune跟byte也进行字符串的操作。

```go
s := "hello"
// s[0] = 'H'  // 这行代码会导致编译错误
```

**byte**，占用1个节字，就 8 个比特位，所以它和 `uint8` 类型本质上没有区别，它表示的是 ACSII 表中的一个字符。
如下这段代码，分别定义了 byte 类型和 uint8 类型的变量 a 和 b

```go
import "fmt"

func main() {
    var a byte = 65 
    // 8进制写法: var c byte = '\101'     其中 \ 是固定前缀
    // 16进制写法: var c byte = '\x41'    其中 \x 是固定前缀
    
	var b uint8 = 66
    fmt.Printf("a 的值: %c \nb 的值: %c", a, b)
    // 或者使用 string 函数
    // fmt.Println("a 的值: ", string(a)," \nb 的值: ", string(b))
}
```

在 ASCII 表中，由于字母 A 的ASCII 的编号为 65 ，字母 B 的ASCII 编号为 66，所以上面的代码也可以写成这样

```go
import "fmt"

func main() {
	var a byte = 'A'
	var b uint8 = 'B'
    fmt.Printf("a 的值: %c \nb 的值: %c", a, b)
}
```

他们的输出结果都是一样的。

```
a 的值: A 
b 的值: B
复制代码
```

**rune**，占用4个字节，共32位比特位，所以它和 `uint32` 本质上也没有区别。它表示的是一个 Unicode字符（Unicode是一个可以表示世界范围内的绝大部分字符的编码规范）。

```go
import (
	"fmt"
	"unsafe"
)

func main() {
	var a byte = 'A'
	var b rune = 'B'
	fmt.Printf("a 占用 %d 个字节数\nb 占用 %d 个字节数", unsafe.Sizeof(a), unsafe.Sizeof(b))
}
```

输出如下
```
a 占用 1 个字节数
b 占用 4 个字节数
```

由于 byte 类型能表示的值是有限，只有 2^8=256 个。所以如果你想表示中文的话，你只能使用 rune 类型。

```go
var name rune = '中'
```

或许你已经发现，上面我们在定义字符时，不管是 byte 还是 rune ，我都是使用单引号，而没使用双引号。
对于从 Python 转过来的人，这里一定要注意了，在 Go 中单引号与 双引号并不是等价的。
单引号用来表示字符，在上面的例子里，如果你使用双引号，就意味着你要定义一个字符串，赋值时与前面声明的前面会不一致，这样在编译的时候就会出错。

```go
cannot use "A" (type string) as type byte in assignment
```

上面我说了，byte 和 uint8 没有区别，rune 和 uint32 没有区别，那为什么还要多出一个 byte 和 rune 类型呢？
理由很简单，因为uint8 和 uint32 ，直观上让人以为这是一个数值，但是实际上，它也可以表示一个字符，所以为了消除这种直观错觉，就诞生了 byte 和 rune 这两个别名类型。

#### 字符串常用函数

以下是 `strings` 包中一些常用函数的详细说明：

1. strings.Contains(s, substr)
   - 用途：检查字符串 s 是否包含子串 substr
   - 返回：布尔值
   - 示例：
     ```go
     fmt.Println(strings.Contains("seafood", "foo")) // 输出: true
     ```

2. strings.HasPrefix(s, prefix)
   - 用途：检查字符串 s 是否以 prefix 开头
   - 返回：布尔值
   - 示例：
     ```go
     fmt.Println(strings.HasPrefix("seafood", "sea")) // 输出: true
     ```

3. strings.HasSuffix(s, suffix)
   - 用途：检查字符串 s 是否以 suffix 结尾
   - 返回：布尔值
   - 示例：
     ```go
     fmt.Println(strings.HasSuffix("seafood", "food")) // 输出: true
     ```

4. strings.Index(s, substr)
   - 用途：查找子串 substr 在字符串 s 中第一次出现的位置
   - 返回：索引值（如果不存在则返回 -1）
   - 示例：
     ```go
     fmt.Println(strings.Index("chicken", "ken")) // 输出: 4
     ```

5. strings.Join(slice, sep)
   - 用途：将字符串切片 slice 中的元素连接成一个字符串，元素之间用 sep 分隔
   - 返回：连接后的字符串
   - 示例：
     ```go
     s := []string{"foo", "bar", "baz"}
     fmt.Println(strings.Join(s, ", ")) // 输出: foo, bar, baz
     ```

6. strings.Split(s, sep)
   - 用途：将字符串 s 按照分隔符 sep 进行分割
   - 返回：分割后的字符串切片
   - 示例：
     ```go
     fmt.Printf("%q\n", strings.Split("a,b,c", ",")) // 输出: ["a" "b" "c"]
     ```

7. strings.ToLower(s)
   - 用途：将字符串 s 中的所有字符转换为小写
   - 返回：转换后的字符串
   - 示例：
     ```go
     fmt.Println(strings.ToLower("HELLO")) // 输出: hello
     ```

8. strings.ToUpper(s)
   - 用途：将字符串 s 中的所有字符转换为大写
   - 返回：转换后的字符串
   - 示例：
     ```go
     fmt.Println(strings.ToUpper("hello")) // 输出: HELLO
     ```

9. strings.TrimSpace(s)
   - 用途：去除字符串 s 首尾的空白字符（空格、制表符、换行符等）
   - 返回：处理后的字符串
   - 示例：
     ```go
     fmt.Println(strings.TrimSpace("  hello  ")) // 输出: hello
     ```

10. strings.Replace(s, old, new, n)
    - 用途：将字符串 s 中的 old 子串替换为 new 子串，最多替换 n 次（如果 n < 0，则替换所有）
    - 返回：替换后的字符串
    - 示例：
      ```go
      fmt.Println(strings.Replace("oink oink oink", "k", "ky", 2)) // 输出: oinky oinky oink
      ```

这些函数都是 `strings` 包中的常用函数，它们提供了丰富的字符串处理能力，可以帮助您更方便地操作和分析字符串。在实际编程中，这些函数可以大大简化字符串相关的操作。

11. Fields的基本定义 `func Fields(s string) []string`
	- 将字符串按照空白字符（空格、制表符\t、换行符\n等）分割成子串
	- 自动去除字符串首尾的空白字符
	- 自动去除单词之间的多余空白字符
	- 返回一个字符串切片，包含所有非空白字符组成的子串

	```go
	func main() {
	    // 例子1：普通空格分隔
	    s1 := "the sky is blue"
	    fmt.Printf("%q\n", strings.Fields(s1))
	    // 输出: ["the" "sky" "is" "blue"]
	
	    // 例子2：多个空格和制表符
	    s2 := "  hello   world\t\tgo  "
	    fmt.Printf("%q\n", strings.Fields(s2))
	    // 输出: ["hello" "world" "go"]
	
	    // 例子3：混合空白字符
	    s3 := "a\n\tgood   example\r\n"
	    fmt.Printf("%q\n", strings.Fields(s3))
	    // 输出: ["a" "good" "example"]
	
	    // 例子4：空字符串或只有空白字符
	    s4 := "   "
	    fmt.Printf("%q\n", strings.Fields(s4))
	    // 输出: [] (空切片)
	}
	```


#### strconv 包

strconv 包是 Go 标准库中的一个重要组件，主要用于字符串和基本数据类型之间的转换。

**主要功能**

1. 字符串转换为整数
    - `Atoi(s string) (int, error)`
    - `ParseInt(s string, base int, bitSize int) (int64, error)`
2. 整数转换为字符串
    - `Itoa(i int) string`
    - `FormatInt(i int64, base int) string`
3. 字符串转换为浮点数
    - `ParseFloat(s string, bitSize int) (float64, error)`
4. 浮点数转换为字符串
    - `FormatFloat(f float64, fmt byte, prec, bitSize int) string`
5. 字符串转换为布尔值
    - `ParseBool(str string) (bool, error)`
6. 布尔值转换为字符串
    - `FormatBool(b bool) string`

常用函数示例

```go
// 字符串转整数
num, err := strconv.Atoi("123")

// 整数转字符串
str := strconv.Itoa(456)

// 字符串转浮点数
f, err := strconv.ParseFloat("3.14", 64)

// 浮点数转字符串
fStr := strconv.FormatFloat(3.14, 'f', 2, 64)

// 字符串转布尔值
b, err := strconv.ParseBool("true")

// 布尔值转字符串
bStr := strconv.FormatBool(false)
```

- 大多数转换函数都会返回一个错误值，用于指示转换是否成功。
- 在进行数值转换时，要注意可能的溢出问题。
- `ParseInt` 和 `ParseUint` 允许指定进制（如二进制、十六进制等）。
- 使用 `FormatFloat` 时，可以控制输出的格式和精度。


# 面向对象

Go 语言中**没有传统意义上的 class 概念**,但是它通过**结构体(struct)** 和**接口(interface)** 来实现面向对象编程。
结构体里面只能包含属性，接口只能给行为

## type 定义
- type 定义
	- type 名字 `interface {}`
	- type 名字 `struct {}`
	- type 名字 别的类型
	- type 别名 = 别的类型
- 结构体初始化
- 指针与方法接收器
- 结构体如何实现接口

```go
package main

import "fmt"

func main() {
	fake := FakeFish{}
	// fake 无法调用原来 Fish 的方法
	// 这一句会编译错误
	//fake.Swim()
	fake.FakeSwim()

	// 转换为Fish
	td := Fish(fake)
	// 真的变成了鱼
	td.Swim()

	sFake := StrongFakeFish{}
	// 这里就是调用了自己的方法
	sFake.Swim()

	td = Fish(sFake)
	// 真的变成了鱼
	td.Swim()
}

// 定义了一个新类型，注意是新类型
type FakeFish Fish

func (f FakeFish) FakeSwim() {
	fmt.Printf("我是山寨鱼，嘎嘎嘎\n")
}

// 定义了一个新类型
type StrongFakeFish Fish

func (f StrongFakeFish) Swim() {
	fmt.Printf("我是华强北山寨鱼，嘎嘎嘎\n")
}

type Fish struct {
}

func (f Fish) Swim() {
	fmt.Printf("我是鱼，假装自己是一直鸭子\n")
}
```

```go
package main

import "fmt"

func main() {
	var n News = fakeNews{
		Name: "hello",
	}
	n.Report()
}

type News struct {
	Name string
}

func (d News) Report() {
	fmt.Println("I am news: " + d.Name)
}

type fakeNews = News
```



## 结构体

### 结构体的定义和使用

在Go语言中，`struct`是一种聚合数据类型，可以将多个不同类型的字段组合在一起。`struct`在定义和使用上有很多灵活的特性，下面是一个关于`struct`基本定义和使用的示例和解释。

1. 定义结构体

首先，我们定义一个名为`Book`的结构体，它包含两个字段：`title`和`author`。

```go
package main

import (
    "fmt"
)

// 声明一种新的数据类型，myint ，是 int 的一个别名
type myint int

// 定义一个结构体
type Book struct {
    title  string
    author string
}
```

这里，我们还声明了一种新的数据类型`myint`，它是`int`的一个别名。

2. 使用结构体

在`main`函数中，我们可以声明和使用`Book`结构体。

```go
func main() {
    var book1 Book
    book1.title = "Go语言"
    book1.author = "张三"

    fmt.Println("book1=", book1)
    fmt.Printf("book1.title=%s\n", book1.title)
}
```

我们声明了一个名为`book1`的`Book`类型变量，并给它的`title`和`author`字段赋值。然后，我们打印出`book1`的值。

3. 结构体的传递方式


函数可以通过值或指针传递结构体。

**通过值传递**

```go
func changeBook2(book *Book) {
    // 指针传递
    book.author = "王五"
}
```

在这种方式下，函数接收到的是结构体的指针，对指针指向的结构体的修改会影响原始结构体。

> [!attention]
> 
go 语言中不支持指针的运算，可以使用指针来引用变量的地址，并通过指针来修改原始变量的值。然而，你不能像 C 或 C++ 那样对指针进行算术运算。

4. 测试结构体的传递方式

在`main`函数中，我们调用了上述两个函数来测试结构体的传递方式。

```go
func main() {
    var book1 Book
    book1.title = "Go语言"
    book1.author = "张三"

    fmt.Println("book1=", book1)
    fmt.Printf("book1.title=%s\n", book1.title)

    changeBook1(book1)
    fmt.Println("After changeBook1, book1=", book1)

    changeBook2(&book1)
    fmt.Println("After changeBook2, book1=", book1)
}

```

输出结果：

```sh
book1= {Go语言 张三}
book1.title=Go语言
After changeBook1, book1= {Go语言 张三}
After changeBook2, book1= {Go语言 王五}

```

可以看到，通过`changeBook1`函数对`book1`的修改没有生效，而通过`changeBook2`函数对`book1`的修改生效了。

## 封装

方法是在**特定类型上定义的函数**。可以为任何类型（包括内置类型）定义方法。

在Go语言中，尽管没有传统面向对象语言中的类和继承，但我们可以通过结构体和方法来实现面向对象的编程思想。以下是一个关于如何在Go语言中表示和封装类的示例和解释。

1. 定义结构体

我们首先定义一个名为`Hero`的结构体，它具有三个字段：`Name`、`Ad`和`level`。

```go
package main

import "fmt"

// 如果类名首字母大写，表示其他包也能够访问
type Hero struct {
    // 如果类的属性首字母大写，表示该属性是对外部能够访问，否则的话只能够类的内部访问
    Name  string
    Ad    int
    level int
}

```

- `Name`和`Ad`字段首字母大写，表示它们是**公共**的，其他包也可以访问。
- `level`字段首字母小写，表示它是**私有**的，仅在当前包内可以访问。

2. 定义方法

为结构体定义方法，实现对结构体数据的封装和操作。

```go
func (this Hero) SetName(newName string) {
    // this 是调用该方法的对象的一个副本
    this.Name = newName
}
```

在这种方式下，方法接收者是结构体的副本，对副本的修改不会影响原始结构体。因此，即使`SetName`方法修改了`this.Name`，也不会影响原始对象。

```go
func (this *Hero) Show() {
    fmt.Println("Name = ", this.Name)
    fmt.Println("Ad = ", this.Ad)
    fmt.Println("Level = ", this.level)
}

func (this *Hero) GetName() {
    fmt.Println("Name = ", this.Name)
}

func (this *Hero) SetName(newName string) {
    // this是调用该方法的对象的一个指针
    this.Name = newName
}

```

- `Show`方法打印`Hero`对象的所有字段。
- `GetName`方法获取`Hero`对象的`Name`字段。
- `SetName`方法设置`Hero`对象的`Name`字段。

注意，这些方法的接收者是`*Hero`，表示方法接收者是指向`Hero`的指针。

> 在Go语言中，没有`. 和 ->` 的区别。无论接收者是值类型还是指针类型，都使用`.`来调用方法。编译器会自动处理对值类型或指针类型的调用。

3. 使用结构体和方法

在`main`函数中，我们创建一个`Hero`对象并调用其方法。

```go
func main() {
    // 创建一个对象
    hero := Hero{Name: "zhang2", Ad: 100, level: 1}
    
    hero.Show()

    hero.SetName("Li4")

    hero.Show()
}

```

输出结果： 

```
Name =  zhang2
Ad =  100
Level =  1
Name =  Li4
Ad =  100
Level =  1

```

通过这个示例，我们展示了如何在Go语言中定义一个结构体并为其添加方法，实现面向对象编程的封装特性。

## 继承

在Go语言中，没有传统面向对象语言中的类继承概念，但通过结构体的嵌套，我们可以实现类似继承的行为。这种嵌套结构体的方式使得我们可以重用代码并扩展功能。以下是一个示例，展示如何在Go语言中实现继承。

1. 定义基类（父类）

首先，我们定义一个基类`Human`，包含两个字段`name`和`sex`，并定义两个方法`Eat`和`Walk`。

```go
package main

import "fmt"

type Human struct {
    name string
    sex  string
}

func (h *Human) Eat() {
    fmt.Println("Human.Eat()...")
}

func (h *Human) Walk() {
    fmt.Println("Human.Walk()...")
}
```

2. 定义子类

**组合 (Composition)**: 可以通过将一个类型的值嵌入到另一个类型来实现组合，这种方式类似于继承的概念，但更加灵活且不涉及类型层次结构。

接下来，我们定义一个子类`SuperMan`，通过嵌套`Human`结构体来实现继承，并添加一个新的字段`level`。

```go
type SuperMan struct {
    Human // 通过嵌套结构体实现继承
    level int
}

// 重定义父类的方法Eat()
func (s *SuperMan) Eat() {
    fmt.Println("SuperMan.Eat()...")
}

// 子类的新方法
func (s *SuperMan) Fly() {
    fmt.Println("SuperMan.Fly()...")
}

```

> go的继承没有访问权限(公有、私有)之分

3. 使用继承

在`main`函数中，我们创建基类和子类的实例，并调用它们的方法。

```go
func main() {
    // 创建基类实例
    h := Human{"张三", "男"}
    h.Walk()
    h.Eat()

    // 创建子类实例
    s := SuperMan{
        Human: Human{"李四", "女"},
        level: 23,
    }

    // 访问子类和父类的方法
    s.Walk() // 调用父类方法
    s.Eat()  // 调用子类重写的方法
    s.Fly()  // 调用子类新方法
}
```


> [!note] 注意
> 
> - Go允许多重组合
> - 同名方法会产生冲突
> - 解决冲突的方法：
>     - **显式调用**
>     - 命名组合
>     - 重新实现方法

## 多态

在Go语言中，面向对象编程的多态性通过**接口**（interface)是一组方法的抽象来实现。接口定义了方法集，**任何实现了这些方法的类型都自动实现了该接口**。这使得你可以编写可以接受多种类型的通用，而具体类型可以实现这些接口。在使用接口的地方，可以传递任何实现了该接口的类型，体现出多态的特性。以下是一个示例，展示如何在Go语言中实现多态。

1. 定义接口

首先，我们定义一个`AnimalIF`接口，包含三个方法：`Sleep`、`GetColor`和`GetType`。

```go
package main

import "fmt"

// 本质是一个指针
type AnimalIF interface {
    Sleep()
    GetColor() string // 获取动物的颜色
    GetType() string  // 获取动物的种类
}

```

2. 定义具体的类

接下来，我们定义两个具体的类`Cat`和`Dog`，它们实现了`AnimalIF`接口。

```go
// 具体的类
type Cat struct {
    color string
}

func (this *Cat) Sleep() {
    fmt.Println("Cat is Sleeping")
}

func (this *Cat) GetColor() string {
    return this.color
}

func (this *Cat) GetType() string {
    return "Cat"
}

type Dog struct {
    color string
}

func (this *Dog) Sleep() {
    fmt.Println("Dog is Sleeping")
}

func (this *Dog) GetColor() string {
    return this.color
}

func (this *Dog) GetType() string {
    return "Dog"
}

```


3. 多态性示例

我们可以编写一个函数`showAnimal`，接受一个`AnimalIF`接口类型的参数，来展示多态性。

```go
func showAnimal(animal AnimalIF) {
    animal.Sleep() // 多态
    fmt.Println("Color =", animal.GetColor())
    fmt.Println("Kind =", animal.GetType())
}
```

4. 使用多态

在`main`函数中，我们创建`Cat`和`Dog`的实例，并通过接口类型的参数传递给`showAnimal`函数。

```go
func main() {
    cat := Cat{"Green"}
    dog := Dog{"Yellow"}

    showAnimal(&cat)
    showAnimal(&dog)
}
```

输出结果：

```sh
Cat is Sleeping
Color = Green
Kind = Cat
Dog is Sleeping
Color = Yellow
Kind = Dog
```

1. **解释**
2. **定义接口**：`AnimalIF`接口定义了三个方法，任何实现了这些方法的类型都被认为实现了该接口。
3. **实现接口**：`Cat`和`Dog`结构体实现了`AnimalIF`接口中的所有方法，因此它们都实现了该接口。
4. **多态性**：通过定义一个接受接口类型参数的函数`showAnimal`，我们可以传递任何实现了该接口的类型（如`Cat`和`Dog`）。在函数内部调用接口方法时，具体调用的是传入类型的实现，体现了多态性。


> [!attention]
> - **接口实现**：结构体必须实现接口中定义的**所有**方法，才能算作实现了该接口，从而体现多态性。  
>- **方法名称冲突**：如果结构体的方法名称与接口方法名称相同且签名匹配，这个方法会被视为接口方法的一部分。如果方法名和签名不匹配，它们不会冲突，结构体仍然可以定义额外的方法作为普通方法
>- **接口组合**：，当一个接口组合另外一个接口，实现这个接口的结构体必须同时实现所有方法，一般用来保持原有接口功能的同时扩展新的功能


### 万能类型与类型断言

在Go语言中，`interface{}`是一种特殊的接口类型，被称为空接口，它可以表示任何类型的数据。这使得我们可以编写函数来处理多种不同类型的数据。在使用空接口的同时，我们也需要一种机制来区分它到底是哪种具体类型，这就是类型断言（type assertion）。

1. 定义空接口和类型断言

首先，我们定义一个函数 `myFunc`，它接受一个 `interface{}` 类型的参数，并在内部进行类型断言，来判断传入的参数实际是什么类型。

```go
package main

import (
	"fmt"
)

// interface{} 万能数据类型
func myFunc(arg interface{}) {
	fmt.Println("myFunc is called ...")
	fmt.Println(arg)

	// interface{} 如何区分，此时引用的底层数据类型到底是什么？

	// 给 interface{} 提供“类型断言”的机制
	value, ok := arg.(string)
	if !ok {
		fmt.Println("arg is not string type")
	} else {
		fmt.Println("arg is string type, value=", value)
		fmt.Printf("value type is %T\n", value)
	}
}

type Book struct {
	auth string
}

func main() {
	book := Book{"Golang"}

	myFunc(book)  // 传入 Book 类型
	myFunc(100)   // 传入 int 类型
	myFunc("abc") // 传入 string 类型
}
```


2. 解释代码
- **空接口 `interface{}`**：
    - `interface{}` 可以接受任何类型的值。
    - 在调用 `myFunc` 时，我们可以传入任意类型的参数。
- **类型断言 `arg.(string)`**：
    
    - `arg.(string)` 表示将 `arg` 断言为 `string` 类型。
    - `value, ok := arg.(string)` 是一种安全的类型断言方式，如果断言成功，`ok` 为 `true`，`value` 为断言后的值；否则，`ok` 为 `false`。
- **类型断言的应用**：
    - 在 `myFunc` 函数中，我们首先尝试将 `arg` 断言为 `string` 类型，并根据 `ok` 的值判断断言是否成功。
    - 如果断言失败，我们输出相应的信息。如果断言成功，我们输出断言后的值及其类型。
3. 执行结果

运行上述代码，输出结果如下：
```sh
myFunc is called ...
{Golang}
arg is not string type
myFunc is called ...
100
arg is not string type
myFunc is called ...
abc
arg is string type, value= abc
value type is string
```

从输出结果可以看到：

- 当传入 `Book` 类型和 `int` 类型时，类型断言失败，输出 "arg is not string type"。
- 当传入 `string` 类型时，类型断言成功，输出 "arg is string type, value= abc" 以及 "value type is string"。

4. 使用switch简化类型断言

为了处理更多类型，可以使用 `switch` 语句来简化类型断言的代码。

```go
func myFunc(arg interface{}) {
	fmt.Println("myFunc is called ...")
	fmt.Println(arg)

	// 使用 switch 进行类型断言
	switch value := arg.(type) {
	case string:
		fmt.Println("arg is string type, value=", value)
		fmt.Printf("value type is %T\n", value)
	case int:
		fmt.Println("arg is int type, value=", value)
		fmt.Printf("value type is %T\n", value)
	case Book:
		fmt.Println("arg is Book type, value=", value)
		fmt.Printf("value type is %T\n", value)
	default:
		fmt.Println("arg is of unknown type")
	}
}
```



## 反射

### 反射原理

在Go语言中，变量的定义和使用实际上涉及到两个方面：变量的类型和变量的值。理解变量的类型时，我们可以将其分为两类：**静态类型（static type）** 和**具体类型（concrete type）**。

<img src="https://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240713223636.png" alt="image.png" style="zoom:60%;" />


- 静态类型（Static Type）

	**静态类型**是在编译时确定的变量类型，是我们在代码中显式声明的类型或通过类型推断确定的类型。静态类型包括基本类型（如`int`、`string`、`float`）和自定义类型（如结构体、接口等）。

	```go
	package main
	
	import "fmt"
	
	func main() {
	    var a int = 10     // 静态类型是 int
	    var b string = "hello" // 静态类型是 string
	    
	    fmt.Printf("a is of type %T\n", a)
	    fmt.Printf("b is of type %T\n", b)
	}
	
	```

	在这个示例中，变量`a`的静态类型是`int`，变量`b`的静态类型是`string`。静态类型是在编译时确定的，编译器会根据这些类型进行类型检查和约束。

- 具体类型（Concrete Type）

	**具体类型**是指变量在运行时的实际类型，尤其是当我们使用接口类型时。接口类型本身并不存储数据，而是指向具体实现了该接口的类型。在运行时，接口变量会持有一个具体类型和值。

	```go
	package main
	
	import "fmt"
	
	type Animal interface {
	    Speak() string
	}
	
	type Dog struct{}
	
	func (d Dog) Speak() string {
	    return "Woof!"
	}
	
	type Cat struct{}
	
	func (c Cat) Speak() string {
	    return "Meow!"
	}
	
	func main() {
	    var animal Animal
	
	    animal = Dog{}
	    fmt.Printf("Animal's concrete type is %T\n", animal) // 输出: Animal's concrete type is main.Dog
	
	    animal = Cat{}
	    fmt.Printf("Animal's concrete type is %T\n", animal) // 输出: Animal's concrete type is main.Cat
	}
	
	```

	在这个示例中，`animal`的静态类型是`Animal`接口，但是在运行时它的具体类型可以是`Dog`或`Cat`。通过`%T`格式说明符，我们可以看到`animal`变量在运行时的具体类型。


这个type和value是一个pair，就是每个变量内部都有一个pair,那这个反射就是通过这个变量得到这个type或者这个value

```go
func main(){

	// tty: pair<type:*os.File,value="/dev/tty"文件描述符>
	tty, err := os.OpenFile("/dev/tty",os.O_RDWR,0)

	if err!=nil {
		fmt.Println("open file error",err)
		return
	}

	// r: pair<type: ,value>
	var r io.Reader
	// r: pair<type:*os.File, value:"/dev/tty"文件描述符">
	r=tty

	// w:pair<type: , value>

	var w io.Writer
	// w: pair<type:*os.File,value:"/dev/tty"文件描述符">
	w=r.(io.Writer)


	w.Write([]byte("HELLO THIS IS A TEST...\n"))
}
```

- 在Go语言中，变量在赋值的时候，类型和值会一起传递。确实，`tty` 被赋值给 `r` 的时候，`r` 的 pair 是 `<type: *os.File, value: "/dev/tty" 文件描述符>`。
- `tty` 变量的类型是 `*os.File`，值是指向文件描述符的指针。将 `tty` 赋值给 `r` 时，`r` 的类型信息和值都被更新为 `*os.File` 和指向 `"/dev/tty"` 文件描述符的指针。
- `w, ok := r.(io.Writer)` ,这里触发了类型断言不仅仅是强制转换，它还会检查**实际类型是否实现了目标接**口，并返回两个值：断言后的值和一个布尔值，表示断言是否成功。

> **类型断言的作用**
> 类型断言用于将一个接口类型转换为另一个具体类型或接口类型。它的语法是 `x.(T)`，其中 `x` 是一个接口类型的变量，`T` 是目标类型。类型断言会检查 `x` 的具体类型是否实现了目标类型 `T`。

- 这里，`r.(io.Writer)` 是对 `r` 的类型断言，检查 `r` 的具体类型（即 `*os.File`）是否实现了 `io.Writer` 接口。
- `*os.File` 既实现了 `io.Reader` 接口，也实现了 `io.Writer` 接口，因此这个类型断言成功。
- `w` 被赋值为 `*os.File` 类型的值，`ok` 为 `true`。

> Go语言的类型断言不仅仅是在编译时检查，还涉及运行时检查，以确保类型安全。


下面是一个简单的示例说明这点(实际类型是否实现了目标接口）


```go
package main

import "fmt"

type Reader interface {
	ReadBook()
}

type Writer interface {
	WriteBook()
}

// 具体类型

type Book struct{

}

func (this *Book) ReadBook(){
	fmt.Println("Read a book")
}

func (this *Book) WriteBook() {
	fmt.Println("Write a book")
}

func main(){
	// b:pair<type: *Book, value: 指向新创建的 Book 结构体实例的内存地址>
	b :=&Book{}

	// r:pair<type: ,value>
	var r Reader
	// r:pair<type: *Book, value: 指向新创建的 Book 结构体实例的内存地址>
	r = b

	r.ReadBook()

	var w Writer

	w=r.(Writer)

	w.WriteBook()
}```


### 反射操作

在Go语言中，反射机制提供了一种动态获取类型信息和操作值的能力。与其他编程语言（如[[java基础篇#反射|Java]]）类似，Go语言的反射也用于在运行时获取变量的类型和值，以及调用结构体的方法。以下是反射机制的基本用法和示例。

示例一：基本反射操作

```go
package main

import (
	"fmt"
	"reflect"
)

func reflectNum(arg interface{}) {
	fmt.Println("type: ", reflect.TypeOf(arg))
	fmt.Println("value: ", reflect.ValueOf(arg))
}

func main() {
	var a float64 = 1.235
	reflectNum(a)
}
```


- **reflectNum 函数**：
    - 接受一个 `interface{}` 类型的参数 `arg`。
    - 使用 `reflect.TypeOf(arg)` 获取参数的类型。
    - 使用 `reflect.ValueOf(arg)` 获取参数的值。
- **main 函数**：
    
    - 定义一个 `float64` 类型的变量 `a`，并调用 `reflectNum` 函数。

```sh
type:  float64
value:  1.235
```

示例二：操作结构体的字段和方法

```go
package main

import (
	"fmt"
	"reflect"
)

type User struct {
	Id   int
	Name string
	Age  int
}

func (this User) Call() {
	fmt.Println("user is called ..")
	fmt.Printf("%v\n", this)
}

func main() {
	user := User{1, "Aceld", 18}
	DoFieldAndMethod(user)
}

func DoFieldAndMethod(input interface{}) {
	// 获取input的Type
	inputType := reflect.TypeOf(input)
	fmt.Println("inputType is:", inputType)

	// 获取input的Value
	inputValue := reflect.ValueOf(input)
	fmt.Println("inputValue is:", inputValue)

	// 通过Type获取里面的字段
	for i := 0; i < inputType.NumField(); i++ {
		field := inputType.Field(i)
		value := inputValue.Field(i)

		fmt.Printf("%s: %v = %v\n", field.Name, field.Type, value)
	}

	// 通过Type获取里面的方法并调用
	for i := 0; i < inputType.NumMethod(); i++ {
		method := inputType.Method(i)
		fmt.Printf("%s: %v\n", method.Name, method.Type)
	}
}
```

- **User 结构体**：
    - 定义了一个 `User` 结构体，包含 `Id`、`Name` 和 `Age` 字段。
    - 定义了一个 `Call` 方法。
- **main 函数**：
    - 创建一个 `User` 结构体的实例 `user`，并调用 `DoFieldAndMethod` 函数。
- **DoFieldAndMethod 函数**：
    - 接受一个 `interface{}` 类型的参数 `input`。
    - 使用 `reflect.TypeOf(input)` 获取参数的类型。
    - 使用 `reflect.ValueOf(input)` 获取参数的值。
    - 遍历结构体的字段，并获取每个字段的名称、类型和值。
	    - `NumField()`-返回结构体类型中字段的数量
    - 遍历结构体的方法，并获取每个方法的名称和类型。
	    - `NumMethod()` - 返回该类型的方法数量

```sh
inputType is: main.User
inputValue is: {1 Aceld 18}
Id: int = 1
Name: string = Aceld
Age: int = 18
Call: func(main.User)
```

> 反射提供了强大的动态操作能力，但也带来了性能开销和代码复杂度，因此应在必要时使用，避免滥用。

下面是一个测试代码

```go
package main

import (
    "fmt"
    "reflect"
    "testing"
    "time"
)

type MyStruct struct{}

func (m MyStruct) DirectCall() {
    // fmt.Println("Direct call")
}

func BenchmarkDirectCall(b *testing.B) {
    m := MyStruct{}
    for i := 0; i < b.N; i++ {
        m.DirectCall()
    }
}

func (m MyStruct) ReflectCall() {
    // fmt.s dsPrintln("Reflect call")
}

func BenchmarkReflectCall(b *testing.B) {
    m := MyStruct{}
    method := reflect.ValueOf(m).MethodByName("ReflectCall")
    for i := 0; i < b.N; i++ {
        method.Call(nil)
    }
}

func main() {
    fmt.Println("Running benchmarks...")

    start := time.Now()
    m := MyStruct{}
    for i := 0; i < 1000000; i++ {
        m.DirectCall()
    }
    duration := time.Since(start)
    fmt.Println("Direct call duration:", duration)

    start = time.Now()
    method := reflect.ValueOf(m).MethodByName("ReflectCall")
    for i := 0; i < 1000000; i++ {
        method.Call(nil)
    }
    duration = time.Since(start)
    fmt.Println("Reflect call duration:", duration)
}
```

```go
Running benchmarks...
Direct call duration: 334.251µs
Reflect call duration: 202.89981ms
```

### 类型断言switch

除了用反射获取数据类型，还有一个方式就是使用，类型断言 switch。在其他地方直接使用 arg.(type) 会导致编译错误。

```go
whatAmI := func(i interface{}) {
        switch t := i.(type) { // 类型断言
        case bool:
            fmt.Println("I'm a bool")
        case int:
            fmt.Println("I'm an int")
        default:
            fmt.Printf("Don't know type %T\n", t)
        }
    }
    whatAmI(true)
    whatAmI(1)
    whatAmI("hey")
```


## 结构体标签

### 反射解析标签

在Go语言中，结构体中的字段除了有名字和类型外，还可以有一个可选的标签(tag),这是一种用于向结构体字段添加**元数据**的方式，通常用于在结构体字段上附加一些额外的信息。这些标签可以在运行时通过反射获取和解析，从而提供灵活的元数据操作功能。以下是一个示例，展示如何使用反射解析结构体标签

- 使用场景:Kubernetes APIServer对所有资源的定义都用Json tag和protoBuff tag
	- `NodeName string "json:"nodeName,omitempty" protobuf:"bytes,10,opt,name=nodeName"`

**定义结构体**：

```go
type resume struct {
    Name string `info:"name" doc:"我的名字"`
    Sex  string `info:"sex"`
}
```

- 结构体 `resume` 定义了两个字段 `Name` 和 `Sex`。
- 每个字段上都有标签 `info` 和 `doc`，用来描述字段的额外信息。

**定义函数 `findTag`**：

```go
func findTag(set interface{}) {
    t := reflect.TypeOf(set)
    for i := 0; i < t.NumField(); i++ {
 	   field := t.Field(i)
 	   tagInfo := field.Tag.Get("info")
 	   tagDoc := field.Tag.Get("doc")
 	   fmt.Printf("Field: %s, info: %s, doc: %s\n", field.Name, tagInfo, tagDoc)
    }
}
```

- `findTag` 函数使用反射获取传入参数 `set` 的类型信息。
- `reflect.TypeOf(set)` 获取 `set` 的类型信息 `t`。
- 遍历 `t` 的字段，通过 `t.Field(i)` 获取每个字段的标签信息。
- 使用 `field.Tag.Get("info")` 和 `field.Tag.Get("doc")` 获取标签的值。

**在 `main` 函数中调用 `findTag`**：

```go
func main() {
    var a resume
    findTag(a)
}
```

- 创建 `resume` 类型的变量 `a`。
- 调用 `findTag` 函数，传入 `a`，解析并打印结构体字段的标签信息。

```sh
Field: Name, info: name, doc: 我的名字
Field: Sex, info: sex, doc: 
```

**总结**
- **结构体标签**：通过在结构体字段后使用反引号（`` ` ``）添加标签，可以为字段附加元数据。这些标签通常用于序列化、数据库映射等场景。
- **反射解析标签**：使用 `reflect.TypeOf` 获取类型信息，通过 `NumField` 获取字段数量，通过 `Field(i)` 获取字段信息，再使用 `Tag.Get` 方法获取标签的值。
- **灵活性**：结构体标签提供了一种灵活的方式，可以在运行时通过反射获取和使用这些元数据，从而实现更多动态功能。


### 标签 JSON 中的应用

在 Go 语言中，结构体标签（Tags）是用来为结构体的字段添加元数据的常用方式，特别是在进行**序列化**和**反序列化**操作时，标签起到了重要的作用。本文将介绍如何使用结构体标签在 JSON 编码和解码中进行应用。

1. 定义结构体和标签

在结构体字段后面添加标签，通过标签可以控制字段在 JSON 中的名称。例如：

```go
type Movie struct {
	Title  string   `json:"title"`
	Year   int      `json:"year"`
	Price  int      `json:"rmb"`
	Actors []string `json:"actors"`
}
```

- `Title` 字段在 JSON 中表示为 `title`。
- `Year` 字段在 JSON 中表示为 `year`。
- `Price` 字段在 JSON 中表示为 `rmb`。
- `Actors` 字段在 JSON 中表示为 `actors`。

2. 使用 JSON 标签进行序列化

通过 `json.Marshal` 函数将结构体编码为 JSON 格式的字节切片：

```go
func main() {
	movie := Movie{"喜剧之王", 2000, 10, []string{"xingye", "zhangbozhi"}}

	// 编码过程 结构体 ---> JSON
	jsonStr, err := json.Marshal(movie)
	if err != nil {
		fmt.Println("json marshal error", err)
		return
	}

	fmt.Printf("jsonStr = %s\n", jsonStr)
}
```

输出结果： 

```makefile
jsonStr = {"title":"喜剧之王","year":2000,"rmb":10,"actors":["xingye","zhangbozhi"]}
```

3. 使用 JSON 标签进行反序列化

通过 `json.Unmarshal` 函数将 JSON 字节切片解码为结构体：

```go
func main() {
	jsonStr := `{"title":"喜剧之王","year":2000,"rmb":10,"actors":["xingye","zhangbozhi"]}`
	var movie Movie

	// 解码过程 JSON ---> 结构体
	err := json.Unmarshal([]byte(jsonStr), &movie)
	if err != nil {
		fmt.Println("json unmarshal error", err)
		return
	}

	fmt.Printf("movie = %+v\n", movie)
}
```

4. 其他标签选项

- **omitempty**：如果字段为空值，则在序列化时忽略该字段。
- **-**：完全忽略字段，不进行序列化。


```go
type Movie struct {
	Title  string   `json:"title"`
	Year   int      `json:"year,omitempty"`
	Price  int      `json:"-"`
	Actors []string `json:"actors"`
}

func main() {
	movie := Movie{"喜剧之王", 0, 10, []string{"xingye", "zhangbozhi"}}

	jsonStr, err := json.Marshal(movie)
	if err != nil {
		fmt.Println("json marshal error", err)
		return
	}

	fmt.Printf("jsonStr = %s\n", jsonStr)
}
```


输出结果： 
```go
jsonStr = {"title":"喜剧之王","actors":["xingye","zhangbozhi"]}
```

总结
- **结构体标签**：通过在结构体字段后面添加 JSON 标签，可以控制字段在 JSON 中的名称和序列化行为。
- **序列化和反序列化**：使用 `json.Marshal` 和 `json.Unmarshal` 函数可以将结构体和 JSON 进行相互转换。
- **灵活性**：通过 `omitempty` 和 `-` 等标签选项，可以灵活地控制序列化和反序列化的细节。


# 函数

## Main 函数

- 每个 Go 语言程序都应该有个 main package
- Main package 里的 main 函数是 Go 语言程序入口

```go
package main
func main() {
	args := os.Args
	if len(args) != 0 {
		println("Do not accept any argument")
		os.Exit(1)
	}
	println("Hello world")
}
```


## 命令行参数


在Go语言中，编写能够接受命令行参数的程序是一项基本但极其重要的技能。无论是简单的脚本、实用工具还是复杂的服务，命令行参数都是与外部世界交互的主要方式之一。本章将详细介绍如何在Go程序中使用`flag`包来解析和处理命令行参数。

命令行参数的基础

Go语言的`flag`包提供了简单易用的API来处理命令行参数。在程序启动时，`flag`包会自动解析传入的命令行参数，并将其转换成易于程序访问的格式。

### 使用`flag`包

定义命令行参数

在程序中，我们首先需要定义哪些参数是可以接受的。`flag`包提供了多种类型的变量定义方法，比如`StringVar`、`IntVar`、`BoolVar`等，分别对应于字符串、整数、布尔值类型的参数。

```go
import (
	"flag"
)

var myString string
var myInt int
var myBool bool

func init() {
	flag.StringVar(&myString, "name", "default_name", "A string flag")
	flag.IntVar(&myInt, "number", 10, "An integer flag")
	flag.BoolVar(&myBool, "verbose", false, "A boolean flag")
}
```

解析命令行参数

定义完参数后，我们需要调用`flag.Parse()`函数来解析命令行参数。这一步骤通常在`main`函数中进行。

```go
func main() {
	flag.Parse()

	fmt.Println("String Flag:", myString)
	fmt.Println("Integer Flag:", myInt)
	fmt.Println("Boolean Flag:", myBool)
}
```

处理未知参数和自定义帮助信息

有时，程序可能需要处理一些未知的命令行参数或提供更详细的帮助信息。`flag`包也提供了相应的功能。

```go
func main() {
	flag.Parse()

	for _, arg := range flag.Args() {
		fmt.Println("Unknown argument:", arg)
	}

	flag.Usage()
}
```

实践案例：网络服务配置

假设我们要编写一个网络服务，它需要监听特定的IP地址和端口号。我们可以使用`flag`包来实现这一需求。

```go
var (
	serverIp   string
	serverPort int
)

func init() {
	flag.StringVar(&serverIp, "ip", "127.0.0.1", "设置服务器IP地址(默认是127.0.0.1)")
	flag.IntVar(&serverPort, "port", 8888, "设置服务器端口(默认是8888)")
}

func main() {
	flag.Parse()

	listenAddr := fmt.Sprintf("%s:%d", serverIp, serverPort)
	fmt.Printf("Listening on %s...\n", listenAddr)

	// 网络服务代码...
}
```

## Init 函数

Init 函数：会在包初始化时运行
谨慎使用 init 函数
当多个依赖项目引用统一项目，且被引用项目的初始化在 init 中完成，并且不可重复运行时，会导致启动错误

a.go
```go
package a

import (
	"fmt"

	_ "github.com/cncamp/golang/examples/module1/init/b"
)

func init() {
	fmt.Println("init from a")
}

```

b.go
```go
package b

import (
	"fmt"
)

func init() {
	fmt.Println("init from b")
}

```

```go
package main

import (
	"fmt"

	_ "github.com/cncamp/golang/examples/module1/init/a"
	_ "github.com/cncamp/golang/examples/module1/init/b"
)

func init() {
	fmt.Println("main init")
}
func main() {

}
```

## 返回值

多值返回
- 函数可以返回任意数量的返回值

```go
// 返回多个值
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}

// 使用多返回值
func main() {
    result, err := divide(10, 2)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    fmt.Println("Result:", result)
}
```

- 命名返回值
	- Go 的返回值可被命名，它们会被视作定义在函数顶部的变量。
	- 返回值的名称应当具有一定的意义，它可以作为文档使用。
	- 没有参数的 return 语句返回已命名的返回值。也就是直接返回。

```go
// 命名返回值示例
func getPersonInfo() (name string, age int, err error) {
    name = "Alice"
    age = 30
    err = nil
    return  // 空return会返回name, age, err
}

// 等价的非命名返回值写法
func getPersonInfoRegular() (string, int, error) {
    name := "Alice"
    age := 30
    return name, age, nil  // 必须显式指定返回值
}
```

- 调用者忽略部分返回值
```go
result, _ = strconv.Atoi(origStr)
```

## 传递变长参数

Go 语言中的可变长参数允许调用方传递任意多个相同类型的参数

- 函数定义
`func append(slice []Type, elems ...Type) []Type`

- 调用方法
```go
myArray := []string{}
myArray = append(myArray, "a","b","c")
```

## 内置函数

|         函数名         |         作用         |
| :-----------------: | :----------------: |
|        close        |        管道关闭        |
|      len, cap       | 返回数组、切片，Map 的长度或容量 |
|      new, make      |        内存分配        |
|    copy, append     |        操作切片        |
|   panic, recover    |        错误处理        |
|   print, println    |         打印         |
| complex, real, imag |        操作复数        |
## 回调函数

- 函数作为参数传入其它函数，并在其他函数内部调用执行 
	- `strings.IndexFunc(line, unicode.IsSpace)`
	- `Kubernetes controller的leaderelection`

```go
// 1. 无返回值的回调函数
func processItems(items []string, callback func(item string)) {
    for _, item := range items {
        callback(item)
    }
}

// 使用示例
func main() {
    items := []string{"a", "b", "c"}
    processItems(items, func(item string) {
        fmt.Println(item)
    })
}

// 2. 有返回值的回调函数
func processWithResult(nums []int, callback func(num int) int) []int {
    result := make([]int, len(nums))
    for i, num := range nums {
        result[i] = callback(num)
    }
    return result
}

// 使用示例
func main() {
    nums := []int{1, 2, 3}
    results := processWithResult(nums, func(num int) int {
        return num * 2
    })
}

// 3. 多参数多返回值的回调函数
func processData(data []string, validator func(string, int) (bool, error)) []string {
    var result []string
    for i, item := range data {
        ok, err := validator(item, i)
        if err != nil {
            continue
        }
        if ok {
            result = append(result, item)
        }
    }
    return result
}

// 使用示例
func main() {
    data := []string{"a", "b", "c"}
    results := processData(data, func(s string, index int) (bool, error) {
        if s == "" {
            return false, errors.New("empty string")
        }
        return true, nil
    })
}

// 4. 使用类型别名简化回调函数声明
type FilterFunc func(string) bool

func filter(items []string, f FilterFunc) []string {
    var result []string
    for _, item := range items {
        if f(item) {
            result = append(result, item)
        }
    }
    return result
}

// 使用示例
func main() {
    items := []string{"a", "", "b"}
    results := filter(items, func(s string) bool {
        return s != ""
    })
}

// 5. 闭包作为回调函数
func createProcessor(threshold int) func(int) bool {
    return func(value int) bool {
        return value > threshold
    }
}

func processValues(values []int, checker func(int) bool) []int {
    var result []int
    for _, v := range values {
        if checker(v) {
            result = append(result, v)
        }
    }
    return result
}

// 使用示例
func main() {
    values := []int{1, 2, 3, 4, 5}
    checker := createProcessor(3)
    results := processValues(values, checker)
}

// 6. 方法作为回调函数
type Handler struct {
    prefix string
}

func (h *Handler) handle(s string) string {
    return h.prefix + s
}

func processStrings(items []string, processor func(string) string) []string {
    result := make([]string, len(items))
    for i, item := range items {
        result[i] = processor(item)
    }
    return result
}

// 使用示例
func main() {
    h := &Handler{prefix: "pre-"}
    items := []string{"a", "b", "c"}
    results := processStrings(items, h.handle)
}
```

## 闭包

- 匿名函数
	- 不能独立存在
	- 可以赋值给其他变量
	- `x:= func(){}`
	- 可以直接调用
	- `func(x,y int){println(x+y)}(1,2)`
	- 可作为函数返回值
	- `func Add() (func(b int) int)`

- 使用场景

```go
defer func() {
	if r := recover(); r != nil {
		println(“recovered in FuncX”)
	}
}()
```


## 传值还是传指针

- Go 语言只有一种规则-传值
- 函数内修改参数的值不会影响函数外原始变量的值
- 可以传递指针参数将变量地址传递给调用函数，Go 语言会复制该指针作为函数内的地址，但指向同一地址

# 异常

## 错误处理

Go 语言无内置exceptio机制，只提供error接口供定义错误

```go
type error interface {
	Error() string
}
```

- 可通过 errors.New 或 fmt.Errorf 创建新的 error
	- `var errNotFound error = errors.New("NotFound")`
- 通常应用程序对 error 的处理大部分是判断 error 是否为 nil
如需将 error 归类，通常交给应用程序自定义，比如 kubernetes 自定义了与 apiserver 交互的不同类型错误

```go
type StatusError struct {
	ErrStatus metav1.Status
}
var _ error = &StatusError{}

// Error implements the Error interface.
func (e *StatusError) Error() string {
	return e.ErrStatus.Message
}
```

## defer

- 函数返回之前执行某个语句或函数
	- 等同于 Java 和 C#的finally
- 常见的 defer 使用场景：记得关闭你打开的资源
	- `defer file.Close()`
	- `defer mu.Unlock()`
	- `defer println("")`

## Panic和recover

- panic: 可在系统出现不可恢复错误时主动调用 panic, panic 会使当前线程直接 crash
- defer: 保证执行并把控制权交还给接收到 panic 的函数调用者
- recover: 函数从panic或错误场景中恢复

1. 基本的 panic 示例：
```go
func main() {
    fmt.Println("开始")
    panic("出现严重错误！")  // 程序会在这里崩溃
    fmt.Println("结束")    // 这行不会执行
}
```

2. defer 的使用：
```go
func main() {
    // defer 的执行顺序是后进先出(LIFO)
    defer fmt.Println("清理1")
    defer fmt.Println("清理2")
    
    fmt.Println("正常操作")
    panic("出错了！")
    
    // 输出顺序:
    // 正常操作
    // 清理2
    // 清理1
    // panic: 出错了！
}
```

3. recover 恢复 panic：

```go
func main() {
    defer func() {
        if err := recover(); err != nil {
            fmt.Printf("恢复错误: %v\n", err)
        }
    }()
    
    fmt.Println("开始")
    panic("出现错误")
    fmt.Println("结束")  // 不会执行
}
```


recover 的主要用途：
1. 防止程序完全崩溃
2. 记录错误信息
3. 优雅地处理错误
4. 确保资源被正确清理
5. 让程序从错误中恢复并继续运行

## errors包

- New 创建一个新的 error
- Is 判断是不是特定的某个error
- As 类型转换为特定的error
- Unwrap 解除包装，返回被包装的 error

```go
package main

import (
	"errors"
	"fmt"
)

func main() {
	var err error = &MyError{}
	println(err.Error())

	ErrorsPkg()

}

type MyError struct {
}

func (m *MyError) Error() string {
	return "Hello, it's my error"
}

func ErrorsPkg() {
	err := &MyError{}
	// 使用 %w 占位符，返回的是一个新错误
	// wrappedErr 是一个新类型，fmt.wrapError
	wrappedErr := fmt.Errorf("this is an wrapped error %w", err)

	// 再解出来
	if err == errors.Unwrap(wrappedErr) {
		fmt.Println("unwrapped")
	}

	if errors.Is(wrappedErr, err) {
		// 虽然被包了一下，但是 Is 会逐层解除包装，判断是不是该错误
		fmt.Println("wrapped is err")
	}

	copyErr := &MyError{}
	// 这里尝试将 wrappedErr转换为 MyError
	// 注意我们使用了两次的取地址符号
	if errors.As(wrappedErr, &copyErr) {
		fmt.Println("convert error")
	}
}
```

1. error 其实就是一个内置的普通的接口。error 相关的操作在 errors 包里面
2. panic 强调的是无可挽回了。但是也可以用 recover 恢复过来
3. 闭包是很强大的特性，但是要小心延时绑定

# 并发编程

## goroutine 

### goroutine 基本模型和调度设计策略

#### 操作系统调度的发展

早期单进程操作系统，在一个时间轴上，每个进程顺序的执行，同一时刻只能有一个线程执行的效果

<img src="https://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240714174508.png" alt="image.png" style="zoom:80%;" />

单进程时代的两个问题？
1. 单一执行流程、计算机智能一个任务一个任务处理
2. 进程阻塞所带来的cpu浪费时间


<img src="https://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240714174722.png" alt="image.png" style="zoom:100%;" />

多进程/多线程解决了阻塞问题，但又面临新的问题

<img src="https://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240714175139.png" alt="image.png" style="zoom:100%;" />


可以看到切换过程中有切换**上下文的成本**，导致进程/线程数量越多，切换**成本就越大**，也就越**浪费**

- 虽然CPU 100%
	- 60%执行程序
	- 40%在切换中

多线程开发设计变得**越来越复杂**，多线程随着**同步竞争**(如 锁、竞争资源冲突等)

<img src="https://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240714175859.png" alt="image.png" style="zoom:30%;" />



所以工程师就在思考一个线程分为内核态和用户态

- 内核线程与用户线程的区别
	- **内核线程**：由操作系统内核直接管理和调度的线程。它们在内核态下运行，可以直接访问系统的物理内存和其他底层资源。内核线程的创建、调度和管理完全由操作系统控制，这意味着每次切换内核线程都会涉及内核态的上下文切换，这会带来一定的开销。
	- **用户线程**：也称为轻量级进程（Lightweight Process, LWP），它们在用户空间中运行，由应用程序或者用户级的线程库管理。用户线程之间的切换不会引起内核态的上下文切换，因此开销相对较小。但用户线程需要依赖于内核线程来获得CPU时间，即一个内核线程可以承载多个用户线程。

- 绑定机制
	当一个线程被创建时，它实际上被分解为两部分：一个内核线程和一个或多个用户线程。内核线程负责与操作系统交互，管理底层资源；用户线程则专注于应用程序逻辑的执行。这两者通过一种**绑定机制**连接起来，使得从CPU的角度来看，它只与内核线程交互，而用户线程的存在对CPU来说是透明的。

- 操作系统的视角
	操作系统仍然认为自己在调度的是单一的线程（实际上是内核线程）。当内核线程被调度时，它会携带其绑定的用户线程一起运行。从操作系统的角度看，它无需关心用户线程的存在，也不需要修改任何代码来适应这种模型，因为内核线程的管理和调度机制保持不变。

- 用户空间的灵活性
	通过将线程划分为内核线程和用户线程，开发者可以在用户空间内自由地创建、管理和调度用户线程，而不需要操作系统内核的介入。这为应用程序提供了更大的灵活性和效率，因为用户线程之间的切换成本较低，而且可以更精细地控制线程间的同步和通信。

<img src="https://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240714204803.png" alt="image.png" style="zoom:40%;" />
我们改一个名称，内核线程就做thread，用户线程叫做协程，

<img src="https://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240714205015.png" alt="image.png" style="zoom:40%;" />
我们的绑定，内核线程通过一个协程调度器绑定多个协程，cpu还是无感，还认为只要一个线程，但是上层开了3个协程，每个协程挂载一个任务，这样用户态仍然能保持一个并发效果，而cpu本身是不需要切换的，这样就解决cpu高消耗调度的瓶颈，这样是一个N比1的协程关系

<img src="https://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240714205613.png" alt="image.png" style="zoom:40%;" />

这种N:1的弊端就是有一个协程阻塞了，那么与之绑定的内核线程将无法立即执行其他协程，这是因为内核线程与协程的绑定关系，导致内核线程在等待当前协程解除阻塞时无法**自动切换**到另一个非阻塞的协程上(**当一个协程执行阻塞操作（如I/O操作）时，操作系统只能识别到线程级别的阻塞。它无法分辨是整个线程阻塞还是只有一个协程阻塞**)。相反，用户态的协程调度器负责在适当的时候选择并执行另一个非阻塞的协程，而这一切都在用户空间内部完成，对操作系统和内核线程调度来说是透明的。

如果换成1:1的话，协程的创建、删除和切换的代价都由CPU完成，代价有点昂贵了、 

所以我们经常用M:N的关系

<img src="https://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240714212529.png" alt="image.png" style="zoom:40%;" />
上层语言需要做一个协程的调度器。

#### Golang对协程的处理

<img src="https://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240720175656.png" alt="image.png" style="zoom:80%;" />



首先对协程改了一个名字叫做Goroutine,不仅改了名字，还改了内存，将Goroutine改成几KB，把大部分多余不必要的空间砍掉了
几KB的内存导致可以大量的Goroutine生产。

**Goroutine的灵活调度器**

go语言早期调度器的处理用一个全局的队列去保存生产出来的协程，调度器采用了M:N模型，比如m0想要调用一个协程时，要去获取全局的队列，全局队列首先会有个锁来进行保护，协程任务做完任务之后再还回去。


<img src="https://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240721125450.png" alt="image.png" style="zoom:60%;" />


1. 创建、销毁、调度G都需要每个M获取锁,这就形成了**激烈的锁竞争**。
2. M转移G会造成**延迟(主要是局部性)和额外的系统负载**。
3. 系统调用(CPU在M之间的切换)导致频繁的线程阻塞和取消阻塞操作**增加了系统开销**。

##### GMP

> [!multi-column]
>
>> [!note]+  什么是GMP？
>>
>>- 实际上就是3个元素的元素的缩写
>>- processor是一个处理器，可以理解为来处理goroutine协程的
>>- processor包含了**每一个goroutine的资源**，如果要运行一个goroutine的话，要先获得一个**获取processor**
>
>> [!info] + GMP
>> <img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240721130826.png" alt="image.png" style="zoom:60%;" />

##### GMP模型


> [!multi-column]
>
>> [!note]+ 什么是GMP模型呢？
>>
>>- GMP模型是Go语言的调度系统，由多层次组成。 
>>- 底层是硬件层，即物理CPU核心，负责实际的计算执行。 
>>- 其上是操作系统层，包含操作系统调度器，管理内核线程在物理CPU上的运行。 
>>- Go运行时的用户态调度层是GMP模型的核心，包括三个主要组件： 
>>	- G (Goroutine): 代表协程
>>	- M (Machine): 代表操作系统线程 
>>	- P (Processor): 代表处理器，是重要的调度单元 
>>- P (Processor) 的详细特点： 
>>	- 保存了每一个真正的进程或每一个程序的全部协程资源，包括栈、堆、数据等 
>>	- 维护一个本地队列 (Local Queue)，存放待运行的Goroutine 
>>	- 数量由GOMAXPROCS参数设置，决定程序最大并行度 
>>	- 同一时刻只能执行一个Goroutine 
>>	- P的个数不是固定的，可以通过GOMAXPROCS参数动态调整 
>>- M (Machine) 的工作方式：
>>	- 需要先获取一个P 
>>	- 然后从P的本地队列获取Goroutine来执行 
>>- 除了P的本地队列，还有一个全局队列 (Global Queue)：
>>	- 存放等待运行的Goroutine 
>>	- 当P的本地队列为空时，会从全局队列获取Goroutine 
>>	- 也用于负载均衡，当某个P的本地队列满时，会将一部分Goroutine移到全局队列 
>>- 调度过程： 
>>	- M获取P后，P会将其本地队列中的协程交给M去执行 
>>	- 一个P同一时刻只能执行一个协程 
>>	- 程序能够进行的最高并行度就是GOMAXPROCS的值 
>>- GMP模型的优势： 
>>	- 在用户态实现高效的协程调度 
>>	- 充分利用多核心CPU 
>>	- 平衡并发和并行
>>	- 使Go程序能高效运行在现代多核处理器上
>
>> [!info]+ GMP模型
><img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240721130658.png" alt="image.png" style="zoom:70%;" />
>> 

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241109081726.png" alt="image.png" style="zoom:60%;" />

##### 调度器的设计策略

- 复用线程：
	- 这是为了减少创建和销毁线程的开销。
	- Go通过维护一个线程池来实现线程复用，避免频繁创建新线程。
    1. **Work Stealing机制**：当一个P的本地队列为空时，它会尝试从其他P的队列中"偷取"Goroutine来执行，提高CPU利用率。
	<img src="https://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240721140341.png" alt="image.png" style="zoom:60%;" />

    2. Hand Off机制：当一个M因为系统调用而阻塞时，会尝试将P与M进行**分离**，P会被交给其他M执行，保证资源不被闲置。

	```go
	初始状态：
	M0 --- P --- G0（准备系统调用）
	      |
	      G1、G2、G3（本地队列）
	
	系统调用后：
	M0 --- G0（系统调用中）    M1 --- P --- G1
	                               |
	                               G2、G3
	```

	<img src="https://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240721140416.png" alt="image.png" style="zoom:80%;" />


- 利用并行：调度器会充分利用多核处理器的并行能力。通过GOMAXPROCS参数，可以控制同时运行的OS线程数量。
- 抢占：调度器支持协作式和强制式抢占。这确保了长时间运行的Goroutine不会独占CPU，提高了调度的公平性。

	<img src="https://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240721140552.png" alt="image.png" style="zoom:60%;" />


- 全局G队列：
	- 当P的本地队列满时，部分Goroutine会被放入全局队列。
	- 全局队列也用于负载均衡，当P的本地队列为空时，先去其他队列偷，如果都没有，会从全局队列获取任务。

- G 会被放入 sudog 主要是在进行同步操作（synchronization）时
	- 当 G 被放入 sudog 时，P 和 M 的状态变化是这样的：

	```go
	1. 初始状态：
	M1 --- P --- G1（即将进行同步操作）
	          |
	          G2、G3（本地队列）
	
	2. G1 被放入 sudog 后：
	M1 --- P --- G2（G1放入sudog等待队列）
	          |
	          G3（本地队列）
	
	说明：
	- G1 进入阻塞态，被放入相应同步原语的等待队列(sudog)
	- P 会立即调度下一个可运行的 G（这里是G2）
	- M1 继续执行新的 G
	```
	
	1. Channel 阻塞示例：
	
		```go
		func channelExample() {
		    ch := make(chan int) // 无缓冲channel
		    
		    // 初始状态
		    // M1 --- P --- G1(准备读channel)
		    //           |
		    //           G2、G3
		    
		    go func() {
		        val := <-ch  // G1 阻塞在channel读取操作
		        // G1 被放入 channel 的 recvq(sudog)
		        // M1 --- P --- G2
		        //           |
		        //           G3
		    }()
		}
		```
	
	2. Mutex 锁示例：
		```go
		func mutexExample() {
		    var mu sync.Mutex
		    
		    // M1 --- P --- G1(持有锁)
		    //           |
		    //           G2(准备获取锁)、G3
		    
		    mu.Lock()  // G1获得锁
		    
		    go func() {
		        mu.Lock()  // G2尝试获取锁，但被阻塞
		        // G2 被放入 mutex 的等待队列(sudog)
		        // M1 --- P --- G3
		        defer mu.Unlock()
		    }()
		}
		```
	
	3. WaitGroup 示例：
		```go
		func waitGroupExample() {
		    var wg sync.WaitGroup
		    wg.Add(2)
		    
		    // M1 --- P --- G1(主goroutine)
		    //           |
		    //           G2、G3
		    
		    go func() { // G2
		        // 做一些工作...
		        wg.Done()
		    }()
		    
		    go func() { // G3
		        // 做一些工作...
		        wg.Done()
		    }()
		    
		    wg.Wait()  // G1 等待，被放入sudog
		    // M1 --- P --- G2
		    //           |
		    //           G3
		}
		```
	
	4. Select 多路复用示例：
		```go
		func selectExample() {
		    ch1 := make(chan int)
		    ch2 := make(chan int)
		    
		    // M1 --- P --- G1(准备select)
		    //           |
		    //           G2、G3
		    
		    select {
		    case <-ch1:
		    case <-ch2:
		        // G1 被同时放入ch1和ch2的recvq(sudog)
		        // M1 --- P --- G2
		        //           |
		        //           G3
		    }
		}
		```

- gFree 的工作原理：

	```sh
	1. 获取 g：
	初始状态：
	gFree列表: [g1, g2, g3]
	当需要新goroutine时：
	- 优先从gFree取出 g1
	- gFree列表变为: [g2, g3]
	
	2. 归还 g：
	某个goroutine完成时：
	- g4 完成工作
	- g4 放入gFree列表
	- gFree列表变为: [g4, g2, g3]
	```

	```sh
	// 区别示意图：
	                 
	P的本地运行队列(LRQ)         gFree列表
	[G1, G2, G3]              [g1, g2, g3]
	│                         │
	└─存放待运行的goroutine     └─存放可重用的g结构体
	   - 有具体的任务函数          - 只是空的g结构体
	   - 等待被调度执行            - 等待被重新初始化使用
	```


- Goroutine创建过程
	- 获取或者创建新的 Goroutine 结构体
		- 从处理器的 gFree 列表中查找空闲的 Goroutine
		- 如果不存在空闲的 Goroutine，会通过 runtime.malg 创建一个栈大小足够的新结构体
	- 将函数传入的参数移到 Goroutine 的栈上
	- 更新 Goroutine 调度相关的属性，更新状态为_Grunnable
	- 返回的 Goroutine 会存储到全局变量 allgs 中



pIdle 的工作流程：
```sh
1. P 进入空闲：
初始状态：
M1 --- P1 --- G1
M2 --- P2 --- G2

当 G1 完成，P1 没有其他 G 可运行：
- P1 可能进入 pIdle
- M1 也进入休眠

2. P 被重新使用：
当新建 goroutine 或 M 需要 P 时：
- 从 pIdle 获取空闲的 P
- P 重新投入工作
```

### 调度器行为

- 为了保证公平，当全局运行队列中有待执行的 Goroutine 时，通过 schedtick 保证有一定几率`(1/61)`会从**全局的运行队列**中查找对应的 Goroutine
- 从处理器**本地的运行队列**中查找待执行的 Goroutine
- 如果前两种方法都没有找到 Goroutine，会通过 runtime.findrunnable 进行**阻塞地查找** Goroutine
	- 从本地运行队列、全局运行队列中查找
	- 从网络轮询器中查找是否有 Goroutine 等待运行
	- 通过 runtime.runqsteal 尝试从其他随机的处理器中窃取待运行的 Goroutine


### 创建goroutine

1. 基本创建方式

使用`go`关键字可以很方便地创建一个goroutine。

```go

func newTask() {
    i := 0
    for {
        i++
        fmt.Printf("new Goroutine: i = %d\n", i)
        time.Sleep(1 * time.Second)
    }
}

func main() {
    // 创建一个goroutine去执行newTask()函数
    go newTask()
    
    fmt.Println("main goroutine exit")
}
```

`go newTask()`创建了一个新的goroutine来执行`newTask`函数。主goroutine继续执行后面的代码。

需要注意的是，如果main函数结束，所有的goroutine都会被强制结束。如果我们想让程序持续运行，可以在main函数中添加一个循环：

```go
func main() {
    go newTask()
    
    // 保持main函数不退出
    i := 0
    for {
        i++
        fmt.Printf("main Goroutine: i = %d\n", i)
        time.Sleep(1 * time.Second)
    }
}
```


2. 使用匿名函数创建goroutine

我们也可以使用匿名函数来创建goroutine：

```go
package main

import (
    "fmt"
    "runtime"
    "time"
)

func main() {
    // 创建一个无参数无返回值的goroutine
    go func() {
        defer fmt.Println("A.defer")
        func() {
            defer fmt.Println("B.defer")
            // 退出当前的goroutine
            runtime.Goexit()
            fmt.Println("B") // 这行代码不会被执行
        }()
        fmt.Println("A") // 这行代码不会被执行
    }() // 这里是立即执行函数（Immediately Invoked Function Expression，简称 IIFE） // 这里其实是定义了一个匿名函数并立即执行它

    // 休眠以观察goroutine的执行
    time.Sleep(1 * time.Second)
}
```

这个展示了如何使用匿名函数创建goroutine，以及如何使用`runtime.Goexit()`提前退出goroutine。注意，`Goexit()`会执行所有已注册的defer函数。


4. 获取goroutine的返回值

需要注意的是，Go语言不支持直接获取goroutine的返回值：

```go
// 这种写法是不支持的
/*
flag := go func(a int, b int) bool {
    fmt.Println("a = ", a, ", b = ", b)
    return true
}(10, 20)
*/
```

因为go语言创建的goroutine是并发的，不是阻塞的。如果需要获取goroutine的返回值，应该使用channel进行通信。

**重要概念**

1. Goroutine是并发执行的，不会阻塞主程序的执行。
2. 如果main函数结束，所有的goroutine都会被强制结束，不管它们是否执行完毕。
3. `runtime.Goexit()`可以立即终止当前的goroutine，但会在终止前执行所有已注册的defer函数。
4. Go语言的并发模型基于CSP（Communicating Sequential Processes），推荐使用channel进行goroutine间的通信。



## channel

### channel的基本定义和使用
 channel是一种用于在不同goroutine之间进行通信和同步的**内置数据结构**。它是Go并发编程模型的核心概念之一，实现了CSP（Communicating Sequential Processes）模型

```go
package main

import "fmt"

func main() {
	// 定义一个channel
	c :=make(chan int)

	go func() {
		defer fmt.Println("goroutine结束")

		fmt.Println("goroutine 正在运行...")

		// 这里采用的是无缓冲，发送和接收操作都会阻塞是同步的，直到另一方准备好
		c <- 666 // 1处 将666 发送给c 这里的1处将666放到c中,等到2处执行了才会接触阻塞
	}()

	num := <-c // 2处 从c中接受数据，并赋值给num 2处必须等到1处放了666才会继续执行

	fmt.Println("num=",num)
	fmt.Println("main goroutine 结束...")

}
```


### chanel 有缓冲与无缓冲同步问题

#### 无缓冲的channel


> [!multi-column]
>
>> [!note]+ 无缓冲的channel
>>
>>- 在第1步,两个`goroutine`都到达通道,但哪个都没有开始执行发送或者接收。 
>>- 在第2步,左侧的`goroutine`将它的手伸进了通道,这模拟了向通道发送数据的行为。这时,这个`goroutine`会在通道中被锁住,直到交换完成。 
>>- 在第3步,右侧的`goroutine`将它的手放入通道,这模拟了从通道里接收数据。这个goroutine一样也会在通道中被锁住,直到交换完成。 
>>- 在第4步和第5步,进行交换,并最终,在第6步,两个`goroutine`都将它们的手从通道里拿出来,这模拟了被锁住的`goroutine`得到释放。两个`goroutine`现在都可以去做其他事情了。
>
>> [!info] 无缓冲图片
>><img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240721170739.png" alt="image.png" style="zoom:60%;" />



#### 有缓冲的channel


> [!multi-column]
>
>> [!note]+ 有缓冲的channel
>>    
>>- 在第1步,右侧的goroutine正在从通道接收一个值。 
>>- 在第2步,右侧的这个goroutine独立完成了接收值的动作,而左测的goroutine正在发送一个新值到通道里。 
>>- 在第3步,左侧的goroutine还在向通道发送新值,而右侧的gooutine正在从通道接收另外一个值。这个步骤里的两个操作既不是同步的,,也不会互相阻塞。 
>>- 最后,在第4步,所有的发送和接收都完成,而通道里还有几个值,也有一些空间可以存更多的值。
>>- 简单来说有缓冲 channel：当**缓冲区满时发送阻塞**，**当缓冲区空时接收阻塞**
>
>> [!info] 有缓冲图片
>><img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240721172614.png" alt="image.png" style="zoom:60%;" />


```go
func main() {
	c := make(chan int,3) // 带有缓冲区的channel

	fmt.Println("len(c) = ",len(c),", cap(c)",cap(c))

	go func(){
		defer fmt.Println("子go程结束")
		for i:=0; i<4;i++{
			c <- i
			fmt.Println("子go程正在运行，发送的元素= ",i ," len(c) = ",len(c),", cap(c) = ",cap(c))
		}
	}() 

	time.Sleep(2*time.Second)
	for i:=0; i<4;i++{
		num := <- c // 从c中接受数据，并赋值给num
		fmt.Println("num = ",num)
	}

	fmt.Println("main over")

}
```


```sh
len(c) =  0 , cap(c) 3
子go程正在运行，发送的元素=  0  len(c) =  1 , cap(c) =  3
子go程正在运行，发送的元素=  1  len(c) =  2 , cap(c) =  3
子go程正在运行，发送的元素=  2  len(c) =  3 , cap(c) =  3
num =  0
num =  1
num =  2
num =  3
main over
子go程正在运行，发送的元素=  3  len(c) =  0 , cap(c) =  3
子go程结束
```


当子goroutine尝试发送第4个元素时（i=3），它会被阻塞，因为缓冲区已满。

#### channel的关闭特点


```go
func main() {
	c:=make(chan int)

	go func(){
		for i:=0;i<5;i++{
			c <- i
		}

		//close 可以关闭一个channel
		close(c)
	}()

	for {
		//ok 如果为true表示channel 没有关闭，如果为false表示channel已经关闭
		if data,ok:= <-c;ok{
			fmt.Println(data)
		}else{
			break
		}
	}

	fmt.Println("main over")
}
```



```go
go func(){
	for i:=0;i<5;i++{
		c <- i // panic: send on closed channel
		close(c)

	}
```


- channel不像文件一样需要经常去关闭,只有当你确实没有任何发送数据了,或者你想显式的结束range循环之类的,才去关闭channel;
- 关闭channel后,无法向channel再发送数据(引发panic错误后导致接收立即返回零值);
- 关闭channel后,**可以继续从channel接收数据**;
- 对于nil channel,无论收发都会被阻塞。


#### channel与range

```go
/* 	for {
		//ok 如果为true表示channel 没有关闭，如果为false表示channel已经关闭
		if data,ok:= <-c;ok{
			fmt.Println(data)
		}else{
			break
		}
	} */

	// 可以使用range来迭代不断操作channel
	for data := range c {
		fmt.Println(data)
	}
```
#### select

`select` 是 Go 语言中的一个重要控制结构，主要用于处理多个通道操作。它的作用和特点如下：

1. 多路复用：
   - 允许一个 goroutine 等待多个通信操作。
   - 可以**同时监听**多个 channel 的读写操作。
2. 非阻塞操作：
   - 可以实现非阻塞的 channel 操作。
   - 当没有 case 就绪时，可以执行默认操作（如果有 default 子句）。
3. 随机选择：
   - 当多个 case 同时就绪时，会**随机选择**一个执行。
   - 这保证了公平性，防止饥饿问题。
4. 阻塞和非阻塞：
   - 如果没有 default 子句，select 会阻塞直到某个 case 可以执行。
   - 有 default 子句时，如果没有 case 就绪，会立即执行 default。
5. 空 select：
   - `select {}` 会永远阻塞。
6. 用途：
   - 超时处理
   - 优雅退出
   - 负载均衡
   - 合并多个 channel
7. 示例：
```go
func fibonacci(c, quit chan int) {
	x, y := 0, 1
	for {
		select {
			case c <- x: // 如果c中管道可写
				x, y = y, x+y
			case <-quit: // 如果quit可读
				fmt.Println("quit")
				return
		}
	}

}

func main() {
	c :=make(chan int)
	quit :=make(chan int)

	//sub go
	go func(){
		for i:=0;i<10;i++{
			fmt.Println(<-c)
		}
		quit <-0
	}()

	//main go
	fibonacci(c,quit)
}
```

8. 注意事项：
   - select 语句只能用于 channel 操作。
   - 一个 case 语句中最多只能有一个 channel 操作。

`select` 是 Go 并发编程中的一个强大工具，它使得同时处理多个 channel 变得简单和高效，是实现复杂并发逻辑的关键结构。

#### 定时器Timer

- time.Ticker 以指定的时间间隔重复的向通道 C 发送时间值
- 使用场景
	- 为协程设定超时时间

```go
timer := time.NewTimer(time.Second)
select {
	// check normal channel
	case <-ch:
	fmt.Println("received from ch")
	case <-timer.C:
	fmt.Println("timeout waiting from channel ch")
}
```

#### 上下文Context

Context主要用于在多个Goroutine之间传递**取消信号、截止时间、键值对**等信息。让我通过实际例子来说明：

1. 取消信号传递：
```go
func cancelExample() {
    // 创建可取消的context
    ctx, cancel := context.WithCancel(context.Background())
    
    // 启动工作goroutine
    go worker(ctx)
    
    // 一段时间后取消
    time.Sleep(time.Second)
    cancel() // 发送取消信号
}

func worker(ctx context.Context) {
    for {
        select {
        case <-ctx.Done():
            // 收到取消信号，退出
            fmt.Println("worker cancelled")
            return
        default:
            // 继续工作
            time.Sleep(100 * time.Millisecond)
        }
    }
}
```

2. 设置超时：
```go
func timeoutExample() {
    // 创建带1秒超时的context
    ctx, cancel := context.WithTimeout(context.Background(), time.Second)
    defer cancel()

    go func() {
        if err := slowOperation(ctx); err != nil {
            // 处理超时错误
            fmt.Println("operation failed:", err)
        }
    }()
}

func slowOperation(ctx context.Context) error {
    select {
    case <-time.After(2 * time.Second):
        return nil
    case <-ctx.Done():
        return ctx.Err() // 返回超时错误
    }
}
```

3. 传递键值对：
```go
func valueExample() {
    // 创建带值的context
    ctx := context.WithValue(context.Background(), "userId", "123")
    
    go handleRequest(ctx)
}

func handleRequest(ctx context.Context) {
    // 获取context中的值
    userId := ctx.Value("userId").(string)
    fmt.Printf("Processing request for user: %s\n", userId)
}
```

4. 多层取消：
```go
func multiCancelExample() {
    // 父context
    parentCtx, parentCancel := context.WithCancel(context.Background())
    defer parentCancel()
    
    // 子context
    childCtx, childCancel := context.WithCancel(parentCtx)
    defer childCancel()
    
    go func() {
        select {
        case <-parentCtx.Done():
            fmt.Println("parent cancelled")
        case <-childCtx.Done():
            fmt.Println("child cancelled")
        }
    }()
    
    // 取消父context会导致子context也被取消
    parentCancel()
}
```

5. 在HTTP请求中使用：
```go
func httpExample() {
    http.HandleFunc("/api", func(w http.ResponseWriter, r *http.Request) {
        ctx := r.Context()
        
        // 创建5秒超时的子context
        timeoutCtx, cancel := context.WithTimeout(ctx, 5*time.Second)
        defer cancel()
        
        select {
        case <-timeoutCtx.Done():
            http.Error(w, "Request timed out", http.StatusGatewayTimeout)
            return
        case result := <-doSlowOperation(timeoutCtx):
            fmt.Fprintf(w, "Result: %v", result)
        }
    })
}

func doSlowOperation(ctx context.Context) <-chan string {
    ch := make(chan string)
    go func() {
        // 模拟耗时操作
        select {
        case <-ctx.Done():
            return
        case <-time.After(time.Second):
            ch <- "operation complete"
        }
    }()
    return ch
}
```

6. 在数据库操作中使用：
```go
func dbExample(ctx context.Context) error {
    // 设置3秒超时
    timeoutCtx, cancel := context.WithTimeout(ctx, 3*time.Second)
    defer cancel()
    
    // 执行数据库查询
    var result string
    err := db.QueryRowContext(timeoutCtx, "SELECT * FROM users WHERE id = ?", 1).Scan(&result)
    if err != nil {
        if err == context.DeadlineExceeded {
            return fmt.Errorf("database query timed out")
        }
        return err
    }
    return nil
}
```

Context的主要作用：

1. 控制Goroutine生命周期：

- 优雅退出
- 资源清理
- 避免goroutine泄漏

2. 超时控制：

- API超时设置
- 数据库查询超时
- RPC调用超时

3. 数据传递：

- 请求ID传递
- 用户信息传递
- 追踪信息传递

4. 多级取消：

- 父子context关系
- 取消传播
- 错误处理


### channel 底层实现

#### 无缓冲区的Channel

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241109102911.png" alt="image.png" style="zoom:60%;" />

```go
type hchan struct {
    recvq    waitq         // 等待接收的goroutine队列
    sendq    waitq         // 等待发送的goroutine队列
    lock     mutex         // 互斥锁
}
```

1. 结构组成（灰色部分确实用不到）：
	- recvq：接收者等待队列
	- sendq：发送者等待队列
	- lock：互斥锁，保证并发安全

2. 等待队列结构：
	- first：队列头指针
	- last：队列尾指针
	- 队列中的每个节点是一个sudog结构体，代表被阻塞的goroutine

3. 工作流程：

发送数据时（`ch <- data`）：
```sh
1. 获取锁
2. 检查recvq是否有等待的接收者
   - 有接收者：直接把数据交给接收者，唤醒接收者goroutine
   - 无接收者：将当前goroutine包装成sudog，放入sendq队列并阻塞
3. 释放锁
```

接收数据时（`<-ch`）：
```sh
1. 获取锁
2. 检查sendq是否有等待的发送者
   - 有发送者：直接从发送者获取数据，唤醒发送者goroutine
   - 无发送者：将当前goroutine包装成sudog，放入recvq队列并阻塞
3. 释放锁
```

重点说明：

1. 无缓冲channel本质是一个同步点
2. 发送和接收必须配对进行
3. sudog双向链表实现等待队列
4. 每次操作都需要获取锁，确保并发安全

#### 带缓冲区的Channel

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241109103741.png" alt="image.png" style="zoom:60%;" />

1. 主要字段（全部启用）：

```go
type hchan struct {
    qcount   uint           // 当前缓冲队列中的元素数量
    dataqsiz uint           // 缓冲队列的总大小
    buf      unsafe.Pointer // 指向环形缓冲区的指针
    sendx    uint          // 发送数据的位置索引
    recvx    uint          // 接收数据的位置索引
    recvq    waitq         // 等待接收的goroutine队列
    sendq    waitq         // 等待发送的goroutine队列
    lock     mutex         // 互斥锁
}
```

2. 环形缓冲区的工作原理：
```go
ch := make(chan int, 4) // 创建容量为4的带缓冲channel

// buf数组: [0][1][2][3]
// sendx: 写入位置
// recvx: 读取位置
// qcount: 当前元素个数
```

3. 主要操作流程：

发送数据(ch <- data)：
```go
// 1. 缓冲区未满时
if qcount < dataqsiz {
    // 将数据写入buf[sendx]位置
    // sendx向前移动，到末尾时归零
    // qcount增加
}

// 2. 缓冲区已满时
else {
    // 将当前goroutine包装为sudog
    // 放入sendq等待队列
    // 阻塞直到被唤醒
}
```

接收数据(<-ch)：
```go
// 1. 缓冲区有数据时
if qcount > 0 {
    // 从buf[recvx]读取数据
    // recvx向前移动，到末尾时归零
    // qcount减少
    // 如果sendq有等待的发送者，将其数据写入缓冲区
}

// 2. 缓冲区空时
else {
    // 将当前goroutine包装为sudog
    // 放入recvq等待队列
    // 阻塞直到被唤醒
}
```

4. 实际例子：
```go
func bufferChannelExample() {
    ch := make(chan int, 4)
    
    // 写入数据
    ch <- 1  // buf: [1][ ][ ][ ] sendx=1 recvx=0 qcount=1
    ch <- 2  // buf: [1][2][ ][ ] sendx=2 recvx=0 qcount=2
    ch <- 3  // buf: [1][2][3][ ] sendx=3 recvx=0 qcount=3
    
    // 读取数据
    <-ch     // buf: [ ][2][3][ ] sendx=3 recvx=1 qcount=2
    <-ch     // buf: [ ][ ][3][ ] sendx=3 recvx=2 qcount=1
    
    // 继续写入
    ch <- 4  // buf: [4][ ][3][ ] sendx=0 recvx=2 qcount=2
}
```

## 锁

### Mutex

- sync.Mutex 互斥锁
	- Lock()加锁，Unlock 解锁
```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func main() {
	go lock()
	time.Sleep(5 * time.Second)
}

func lock() {
	lock := sync.Mutex{}
	for i := 0; i < 3; i++ {
		lock.Lock()
		defer lock.Unlock()
		fmt.Println("lock:", i)
	}
}
```

#### 底层原理

```go
type Mutex struct {
    state int32  // 互斥锁的状态
    sema  uint32 // 信号量
}

// state字段的比特位含义：
// 第1位(最低位): 表示锁是否被持有（0：未被持有，1：被持有）
// 第2位: 表示是否有唤醒的goroutine（0：没有，1：有）
// 第3位: 表示是否处于饥饿状态（0：正常模式，1：饥饿模式）
// 剩余位: 等待队列中的goroutine数量
```

Mutex有两种工作模式：

1. 正常模式:
```go
func Lock() {
    // 1. 先尝试直接获取锁
    if atomic.CompareAndSwapInt32(&m.state, 0, mutexLocked) {
        return // 获取成功直接返回
    }
    
    // 2. 获取失败，进入自旋
    for i := 0; i < 4; i++ {
        // 自旋等待一会，看看能不能获取到锁
        runtime.Gosched()
        if atomic.CompareAndSwapInt32(&m.state, 0, mutexLocked) {
            return
        }
    }
    
    // 3. 自旋失败，排队等待
    runtime_SemacquireMutex(&m.sema)
}
```

2. 饥饿模式:
```go
// 当一个goroutine等待超过1ms时，会进入饥饿模式
func Lock() {
    // 在饥饿模式下，锁直接交给等待队列中的第一个goroutine
    if m.state&mutexStarving != 0 {
        queueLock()  // 直接排队
        return
    }
    normalLock()    // 否则使用正常模式
}
```

完整的加锁流程示例：
```go
func 完整的加锁流程() {
    // 场景1: 无人使用锁
    goroutine1.Lock() {
        CAS成功
        直接获得锁
    }

    // 场景2: 锁被占用，但等待时间短
    goroutine2.Lock() {
        CAS失败
        开始自旋
        等到锁释放
        获得锁
    }

    // 场景3: 长时间等待
    goroutine3.Lock() {
        CAS失败
        自旋失败
        进入等待队列
        可能进入饥饿模式
    }
}
```

解锁流程：
```go
func Unlock() {
    // 1. 正常模式：释放锁，唤醒一个等待的goroutine
    if atomic.AddInt32(&m.state, -mutexLocked) == 0 {
        return // 没有等待者，直接返回
    }
    
    // 2. 有等待的goroutine，唤醒一个
    runtime_Semrelease(&m.sema)
}
```

锁分快慢两种获取路径

互斥锁可以处于两种操作模式:**正常模式**和**饥饿模式**
在正常模式下,等待者按照先进先出(FIFO)的顺序排队,但是被换醒的等待者并不拥有互斥锁,并且要与新到达的协程(goroutine)竞争互斥锁的所有权。

新到达的协程具有优势 --它们已经在CPU上运行,而且可能能数量众多,所以被唤醒的等待者很有可能竞争失败。在这种情况下,它会被排到等待队列的前端。
如果一个等待者超过1毫秒未能获取到互斥锁,它就会将互斥锁切换到饥饿模式。

在饥饿模式下,互斥锁的所有权直接从解锁的协程传递给队列前端的等待者。新到达的协程即使看到互斥锁似乎未被锁定也不会会尝试获取,也不会尝试空转(自旋)。
相反,它们会将自己排到等待队列的末尾。

如果一个等待者获得了互斥锁并且发现:(1)它是队列中的最后一个等待者,或者(2)它的等待时间少于1毫秒,它就会将互斥锁切换回正常操作模式。

为什么与正常模式协程竞争

**性能优化**：
```go
// 新到达协程可能正在CPU上运行，而等待队列中的协程需要上下文切换
func 性能比较() {
    // 场景1: 新到达协程直接获取锁
    新协程.Lock()   // 已经在CPU上运行
    执行任务()      // 无需上下文切换
    新协程.Unlock()

    // 场景2: 唤醒等待协程
    等待协程.恢复()  // 需要上下文切换
    执行任务()
    等待协程.Unlock()
}
```

**局部性原理**：
```go
// 新到达的协程更可能持有CPU缓存
func 缓存示例() {
    新协程 := func() {
        mutex.Lock()
        // 数据可能还在CPU缓存中
        操作共享数据()
        mutex.Unlock()
    }

    等待协程 := func() {
        // 已经等待一段时间
        // CPU缓存可能已失效
    }
}
```


### RWMutex

```go
// 1. 多个goroutine可以同时获取读锁
func multipleReaders() {
    var rwLock sync.RWMutex
    
    // 这些可以并发执行
    go func() {
        rwLock.RLock()
        defer rwLock.RUnlock()
        // 读取操作
    }()
    
    go func() {
        rwLock.RLock()
        defer rwLock.RUnlock()
        // 读取操作
    }()
}

// 2. 写锁是独占的
func exclusiveWriter() {
    var rwLock sync.RWMutex
    
    rwLock.Lock()
    // 其他读写操作都会被阻塞
    rwLock.Unlock()
}
```

#### 底层原理

```go
type RWMutex struct {
    w           Mutex   // 图书馆的大门锁
    writerSem   uint32  // 管理员等待室（管理员在这里等待所有读者离开）
    readerSem   uint32  // 读者等待室（当管理员在整理时，新读者在这里等待）
    readerCount int32   // 当前馆内有多少读者
    readerWait  int32   // 管理员等待离开的读者数量
}
```

正常的读者进入流程：
```go
func RLock() {
    // 检查并记录新读者
    if atomic.AddInt32(&readerCount, 1) < 0 {
        // readerCount < 0 说明有管理员在整理
        // 新读者需要在等待室(readerSem)等待
        runtime_SemacquireMutex(&readerSem)
    }
}
// 相当于：
if 管理员正在整理 {
    请在等待室等候
} else {
    直接进入阅读
}
```

管理员开始整理流程：

```go
func Lock() {
    // 先锁住大门，防止其他管理员进入
    w.Lock()
    
    // 将readerCount设为负数，表示有管理员要整理
    r := atomic.AddInt32(&readerCount, -rwmutexMaxReaders)
    
    // 如果还有读者在馆内，管理员要等待
    if r != -rwmutexMaxReaders {
        runtime_SemacquireMutex(&writerSem)
    }
}
// 相当于：
锁住图书馆大门
通知所有人管理员要开始整理了
如果馆内还有读者，管理员在等待室等候
```

读者离开流程：
```go
func RUnlock() {
    if r := atomic.AddInt32(&readerCount, -1); r < 0 {
        // 如果是最后一个离开的读者，需要通知等待的管理员
        if atomic.AddInt32(&readerWait, -1) == 0 {
            runtime_Semrelease(&writerSem)
        }
    }
}
// 相当于：
读者离开时登记
如果管理员正在等待，且是最后一个离开的读者
    通知管理员可以开始整理了
```

管理员完成整理流程：
```go
func Unlock() {
    // 恢复readerCount
    r := atomic.AddInt32(&readerCount, rwmutexMaxReaders)
    
    // 通知所有等待的读者可以进入了
    for i := 0; i < int(r); i++ {
        runtime_Semrelease(&readerSem)
    }
    
    // 解锁大门，允许其他管理员进入
    w.Unlock()
}
// 相当于：
整理完成
通知等待室的所有读者可以进来了
解锁图书馆大门
```


### WaitGroup
- sync.WaitGroup
	- 等待一组 goroutine 返回

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func main() {
	waitBySleep()
}

//
func waitBySleep() {
	for i := 0; i < 100; i++ {
		go fmt.Println(i)
	}
	time.Sleep(time.Second)
}

func waitByChannel() {
	c := make(chan bool, 100)
	for i := 0; i < 100; i++ {
		go func(i int) {
			fmt.Println(i)
			c <- true
		}(i)
	}

	for i := 0; i < 100; i++ {
		<-c
	}
}

func waitByWG() {
	wg := sync.WaitGroup{}
	wg.Add(100)
	for i := 0; i < 100; i++ {
		go func(i int) {
			fmt.Println(i)
			wg.Done()
		}(i)
	}
	wg.Wait()
}

```

### Once
- sync.Once
	- 保证某段代码只执行一次

```go
package main

import (
	"fmt"
	"sync"
)

type SliceNum []int

func NewSlice() SliceNum {
	return make(SliceNum, 0)

}

func (s *SliceNum) Add(elem int) *SliceNum {
	*s = append(*s, elem)
	fmt.Println("add", elem)
	fmt.Println("add SliceNum end", s)
	return s
}

func main() {
	var once sync.Once
	s := NewSlice()
	// 看源代码理解once的行为
	once.Do(func() {
		s.Add(16)
	})
	once.Do(func() {
		s.Add(16)
	})
	once.Do(func() {
		s.Add(16)
	})
}
```


### Cond
- sync.Cond
	- 让一组 goroutine 在满足特定条件时被唤醒

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

type Queue struct {
	queue []string
	cond  *sync.Cond
}

func main() {
	q := Queue{
		queue: []string{},
		cond:  sync.NewCond(&sync.Mutex{}),
	}
	go func() {
		for {
			q.Enqueue("a")
			time.Sleep(time.Second * 2)
		}
	}()
	for {
		q.Dequeue()
		time.Sleep(time.Second)
	}
}

func (q *Queue) Enqueue(item string) {
	q.cond.L.Lock()
	defer q.cond.L.Unlock()
	q.queue = append(q.queue, item)
	fmt.Printf("putting %s to queue, notify all\n", item)
	q.cond.Broadcast()
}

func (q *Queue) Dequeue() string {
	q.cond.L.Lock()
	defer q.cond.L.Unlock()
	for len(q.queue) == 0 {
		fmt.Println("no data available, wait")
		q.cond.Wait()
	}
	result := q.queue[0]
	q.queue = q.queue[1:]
	return result
}

```

### sync.map

Go 语言原生 map 并不是线程安全的，对它进行并发读写操作的时候，需要加锁。而 `sync.map` 则是一种并发安全的 map，在 Go 1.9 引入。

#### sync.map的使用

```go
package main

import (
	"fmt"
	"sync"
)

func main()  {
	var m sync.Map
	// 1. 写入
	m.Store("qcrao", 18)
	m.Store("stefno", 20)

	// 2. 读取
	age, _ := m.Load("qcrao")
	fmt.Println(age.(int))

	// 3. 遍历
	m.Range(func(key, value interface{}) bool {
		name := key.(string)
		age := value.(int)
		fmt.Println(name, age)
		return true
	})

	// 4. 删除
	m.Delete("qcrao")
	age, ok := m.Load("qcrao")
	fmt.Println(age, ok)

	// 5. 读取或写入
	m.LoadOrStore("stefno", 100)
	age, _ = m.Load("stefno")
	fmt.Println(age)
}
```

第 1 步，写入两个 k-v 对；
第 2 步，使用 Load 方法读取其中的一个 key；
第 3 步，遍历所有的 k-v 对，并打印出来；
第 4 步，删除其中的一个 key，再读这个 key，得到的就是 nil；
第 5 步，使用 LoadOrStore，尝试读取或写入 "Stefno"，因为这个 key 已经存在，因此写入不成功，并且读出原值。
程序输出：

```
18
stefno 20
qcrao 18
<nil> false
20
```


#### sync.map原理

sync.Map底层使用了两个原生map，一个叫read，仅用于读；一个叫dirty，用于在特定情况下存储最新写入的key-value数据：

<img src="https://ucc.alicdn.com/images/user-upload-01/1cc2d5ec1bc8422a9fdc912ba5a69d66.png?x-oss-process=image/resize,w_1400/format,webp" alt="image.png" style="zoom:60%;" />
read好比整个sync.Map的一个“**高速缓存**”，当goroutine从sync.Map中读数据时，sync.Map会首先查看read这个缓存层是否有用户需要的数据（key是否命中），如果有（key命中），则通过原子操作将数据读取并返回，这是sync.Map推荐的快路径(fast path)，也是**sync.Map的读性能极高**的原因。

- **写**操作：直接写入dirty（负责写的map）
- **读**操作：先读read（负责读操作的map），没有再读dirty（负责写操作的map）

<img src="https://ucc.alicdn.com/images/user-upload-01/eb659045c21142b992cc8bda51342707.png?x-oss-process=image/resize,w_1400/format,webp" alt="image.png" style="zoom:60%;" />

sync.Map 的实现原理可概括为：

1. 通过 read 和 dirty 两个字段实现数据的**读写分离**，读的数据存在只读字段 read 上，将最新写入的数据则存在 dirty 字段上
2. 读取时会先查询 read，不存在再查询 dirty，写入时则只写入 dirty
3. 读取 read 并不需要加锁，而_读或写 dirty 则需要加锁_
4. 另外有 `_misses_` 字段来统计 read 被穿透的次数（被穿透指需要读 dirty 的情况），超过一定次数则将 dirty 数据更新到 read 中（触发条件：misses=len(dirty)）

<img src="https://ucc.alicdn.com/images/user-upload-01/a98832789cd34c9db0cc1facf134deb1.png?x-oss-process=image/resize,w_1400/format,webp" alt="image.png" style="zoom:80%;" />

优缺点

- 优点：Go官方所出；通过**读写分离**，降低锁时间来提高效率；
- 缺点：不适用于大量写的场景，这样会导致 **`_read map_`** 读不到数据而进一步加锁读取，同时dirty map也会一直晋升为read map，整体性能较差，甚至没有单纯的 **`_map+metux_`** 高。
- 适用场景：**读多写少**的场景。

可见，通过这种读写分离的设计，解决了并发场景下的写入安全，又使读取速度在大部分情况可以接近内建 map，非常适合读多写少的情况。


<img src="https://ucc.alicdn.com/images/user-upload-01/20f35453c3ed4212bc154ac440624464.png?x-oss-process=image/resize,w_1400/format,webp" alt="image.png" style="zoom:70%;" />


<img src="https://ucc.alicdn.com/images/user-upload-01/0327370848ef4d648b19d1209bf4d160.png?x-oss-process=image/resize,w_1400/format,webp" alt="image.png" style="zoom:80%;" />

<img src="https://ucc.alicdn.com/images/user-upload-01/9582a20c2f0146c8ad42df8d989f085b.png?x-oss-process=image/resize,w_1400/format,webp" alt="image.png" style="zoom:80%;" />

### sync.pool

> 一句话总结：保存和复用临时对象，减少内存分配，降低 GC 压力。

举个简单的例子：
```go
type Student struct {
	Name   string
	Age    int32
	Remark [1024]byte
}

var buf, _ = json.Marshal(Student{Name: "Geektutu", Age: 25})

func unmarsh() {
	stu := &Student{}
	json.Unmarshal(buf, stu)
}
```

json 的反序列化在文本解析和网络通信过程中非常常见，当程序并发度非常高的情况下，短时间内需要创建大量的临时对象。而这些对象是都是分配在堆上的，会给 GC 造成很大压力，严重影响程序的性能。

Go 语言从 1.3 版本开始提供了对象重用的机制，即 sync.Pool。sync.Pool 是可伸缩的，同时也是并发安全的，其大小仅受限于内存的大小。sync.Pool 用于存储那些被分配了但是没有被使用，而未来可能会使用的值。这样就可以不用再次经过内存分配，可直接复用已有对象，减轻 GC 的压力，从而提升系统的性能。

sync.Pool 的大小是可伸缩的，高负载时会动态扩容，存放在池中的对象如果不活跃了会被自动清理。

#### sync.pool的使用

声明对象池
只需要实现 New 函数即可。对象池中没有对象时，将会调用 New 函数创建。

```go
var studentPool = sync.Pool{
    New: func() interface{} { 
        return new(Student) 
    },
}
```

Get & Put

```go
stu := studentPool.Get().(*Student)
json.Unmarshal(buf, stu)
studentPool.Put(stu)
```

- `Get()` 用于从对象池中获取对象，因为返回值是 `interface{}`，因此需要类型转换。
- `Put()` 则是在对象使用完毕后，返回对象池。


#### 性能测试

struct 反序列化

```go
func BenchmarkUnmarshal(b *testing.B) {
	for n := 0; n < b.N; n++ {
		stu := &Student{}
		json.Unmarshal(buf, stu)
	}
}

func BenchmarkUnmarshalWithPool(b *testing.B) {
	for n := 0; n < b.N; n++ {
		stu := studentPool.Get().(*Student)
		json.Unmarshal(buf, stu)
		studentPool.Put(stu)
	}
}
```

测试结果如下：

```go
$ go test -bench . -benchmem
goos: darwin
goarch: amd64
pkg: example/hpg-sync-pool
BenchmarkUnmarshal-8           1993   559768 ns/op   5096 B/op 7 allocs/op
BenchmarkUnmarshalWithPool-8   1976   550223 ns/op    234 B/op 6 allocs/op
PASS
ok      example/hpg-sync-pool   2.334s
```

在这个例子中，因为 Student 结构体内存占用较小，内存分配几乎不耗时间。而标准库 json 反序列化时利用了反射，效率是比较低的，占据了大部分时间，因此两种方式最终的执行时间几乎没什么变化。但是内存占用差了一个数量级，使用了 `sync.Pool` 后，内存占用仅为未使用的 234/5096 = 1/22，对 GC 的影响就很大了。

## atomic

### atomic 包介绍
atomic包提供了底层的原子级内存操作，用于在多线程程序中同步访问整型变量和指针。

主要使用场景
- 计数器
- 标志位
- 原子性的加载/存储操作
- 指针的原子性操作

支持的数据类型
- int32, int64
- uint32, uint64
- uintptr
- unsafe.Pointer
- pointer (通过 atomic.Value)

基本操作类型
1. **Load**: 原子性读取
2. **Store**: 原子性存储
3. **Add**: 原子性增加
4. **Swap**: 原子性交换
5. **CompareAndSwap**: 比较并交换

### 使用示例

1. 原子计数器
	```go
	var counter int64
	
	// 增加
	atomic.AddInt64(&counter, 1)
	
	// 读取
	count := atomic.LoadInt64(&counter)
	
	// 存储
	atomic.StoreInt64(&counter, 100)
	```

2. CompareAndSwap使用
	```go
	var flag int32
	
	// CAS操作
	success := atomic.CompareAndSwapInt32(&flag, 0, 1)
	if success {
	    // 成功将0替换为1
	}
	```

3. atomic.Value的使用
	```go
	var config atomic.Value
	
	// 存储
	config.Store(map[string]string{"key": "value"})
	
	// 加载
	conf := config.Load().(map[string]string)
	```

性能考虑
- atomic操作比互斥锁更轻量
- 适用于简单的并发场景
- 复杂场景仍建议使用mutex
- 避免过度使用，可能影响代码可读性

### 常见应用场景

1. 并发安全的计数器
	```go
	type Counter struct {
	    count int64
	}
	
	func (c *Counter) Inc() {
	    atomic.AddInt64(&c.count, 1)
	}
	
	func (c *Counter) Value() int64 {
	    return atomic.LoadInt64(&c.count)
	}
	```

2. 并发安全的开关控制
	```go
	type Switch struct {
	    on int32
	}
	
	func (s *Switch) TurnOn() bool {
	    return atomic.CompareAndSwapInt32(&s.on, 0, 1)
	}
	
	func (s *Switch) TurnOff() bool {
	    return atomic.CompareAndSwapInt32(&s.on, 1, 0)
	}
	```

注意事项
1. 原子操作必须使用指针
2. 避免将原子变量复制
3. int类型的原子操作需要显式指定32位或64位
4. atomic.Value只能存储可比较的类型

与其他同步机制的比较
- **Mutex**: 适用于复杂的临界区
- **Channel**: 适用于通信
- **Atomic**: 适用于简单的数值或状态同步

# 文件IO

让我介绍 Go 中常用的文件 IO 操作：

1. **读取文件**
```go
// 1. 整个文件读取
func readWholeFile() {
    data, err := os.ReadFile("test.txt")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(string(data))
}

// 2. 分块读取
func readByBlock() {
    f, err := os.Open("test.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()

    buffer := make([]byte, 1024)
    for {
        n, err := f.Read(buffer)
        if err == io.EOF {
            break
        }
        fmt.Println(string(buffer[:n]))
    }
}

// 3. 按行读取
func readByLine() {
    f, err := os.Open("test.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()

    scanner := bufio.NewScanner(f)
    for scanner.Scan() {
        fmt.Println(scanner.Text())
    }
}
```

2. **写入文件**
```go
// 1. 写入整个文件
func writeWholeFile() {
    data := []byte("Hello, World!")
    err := os.WriteFile("test.txt", data, 0644)
    if err != nil {
        log.Fatal(err)
    }
}

// 2. 追加写入
func appendToFile() {
    f, err := os.OpenFile("test.txt", os.O_APPEND|os.O_CREATE|os.O_WRONLY, 0644)
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()

    if _, err := f.WriteString("新内容\n"); err != nil {
        log.Fatal(err)
    }
}

// 3. 使用缓冲写入
func bufferedWrite() {
    f, err := os.Create("test.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()

    w := bufio.NewWriter(f)
    w.WriteString("Hello\n")
    w.WriteString("World\n")
    w.Flush() // 别忘了刷新缓冲
}
```

3. **文件操作**
```go
// 1. 创建目录
func createDir() {
    err := os.MkdirAll("path/to/dir", 0755)
    if err != nil {
        log.Fatal(err)
    }
}

// 2. 删除文件
func removeFile() {
    err := os.Remove("test.txt")  // 删除单个文件
    if err != nil {
        log.Fatal(err)
    }
    
    err = os.RemoveAll("testdir") // 删除目录及其内容
    if err != nil {
        log.Fatal(err)
    }
}

// 3. 重命名/移动文件
func moveFile() {
    err := os.Rename("old.txt", "new.txt")
    if err != nil {
        log.Fatal(err)
    }
}
```

4. **文件信息**
```go
// 1. 获取文件信息
func getFileInfo() {
    info, err := os.Stat("test.txt")
    if err != nil {
        log.Fatal(err)
    }
    
    fmt.Println("Name:", info.Name())
    fmt.Println("Size:", info.Size())
    fmt.Println("Mode:", info.Mode())
    fmt.Println("ModTime:", info.ModTime())
    fmt.Println("IsDir:", info.IsDir())
}

// 2. 检查文件是否存在
func checkFileExist() bool {
    _, err := os.Stat("test.txt")
    return !os.IsNotExist(err)
}
```

5. 遍历目录
```go
filepath.Walk(root string, fn WalkFunc) error

type WalkFunc func(path string, info os.FileInfo, err error) error
err = filepath.Walk(inputDir, func(path string, info os.FileInfo, err error) error {
    if err != nil {
        return err    // 如果遍历过程中发生错误，直接返回
    }
    if !info.IsDir() && filepath.Ext(path) == ".txt" {
        files = append(files, path)    // 如果是txt文件，添加到文件列表
    }
    return nil    // 返回nil继续遍历
})

```



7. **实用例子**

文件复制：
```go
func copyFile(src, dst string) error {
    // 打开源文件
    sourceFile, err := os.Open(src)
    if err != nil {
        return err
    }
    defer sourceFile.Close()

    // 创建目标文件
    destFile, err := os.Create(dst)
    if err != nil {
        return err
    }
    defer destFile.Close()

    // 复制内容
    _, err = io.Copy(destFile, sourceFile)
    return err
}
```

大文件处理：
```go
func processLargeFile(filename string) error {
    file, err := os.Open(filename)
    if err != nil {
        return err
    }
    defer file.Close()

    reader := bufio.NewReader(file)
    buffer := make([]byte, 1024)

    for {
        n, err := reader.Read(buffer)
        if err == io.EOF {
            break
        }
        if err != nil {
            return err
        }

        // 处理数据块
        processChunk(buffer[:n])
    }
    return nil
}
```

文件监控：
```go
func watchFile(filename string) {
    initialStat, err := os.Stat(filename)
    if err != nil {
        log.Fatal(err)
    }

    for {
        stat, err := os.Stat(filename)
        if err != nil {
            log.Fatal(err)
        }

        if stat.ModTime() != initialStat.ModTime() {
            fmt.Println("File has been modified")
            initialStat = stat
        }

        time.Sleep(time.Second)
    }
}
```

错误处理最佳实践：
```go
func handleFileOperations() error {
    // 1. 使用defer确保资源释放
    f, err := os.Open("test.txt")
    if err != nil {
        return fmt.Errorf("opening file: %w", err)
    }
    defer func() {
        if err := f.Close(); err != nil {
            log.Printf("error closing file: %v", err)
        }
    }()

    // 2. 检查文件大小
    info, err := f.Stat()
    if err != nil {
        return fmt.Errorf("getting file info: %w", err)
    }
    if info.Size() > maxFileSize {
        return fmt.Errorf("file too large: %d bytes", info.Size())
    }

    // 3. 处理文件内容
    // ...

    return nil
}
```

这些是Go语言中最常用的文件IO操作。记住几个关键点：
1. 总是记得关闭文件（使用defer）
2. 适当的错误处理很重要
3. 对于大文件，使用缓冲读写
4. 注意文件权限设置

# 内存管理

## 内存回收

### TCMalloc概览

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241109121001.png" alt="image.png" style="zoom:60%;" />

- page:内存页,一块8K大小的内存空间。Go与操作系统之间的内存申请和释放,都是以page为单位的
- span:内存块,一个或多个连续的page组成一个span
- sizeclass:空间规格,每个span都带有一个sizeclass,标记着该span中的page应该如何使用
- object:对象,用来存储一个变量数据内存空间,一个span在初始化时,会被切割成一堆等大的object;假设object的大小是16B,span大小是8K,那么就会把span中的page就会被初始化8K/16B=512个object。所谓内存分配,就是分配,就是分配一个object出去

- 对象大小定义
	- 小对象大小:0~256KB
	- 中对象大小:256KB~1MB
	- 大对象大小:>1MB
- 小对象的分配流程
	- ThreadCache -> CentralCache -> HeapPage,大部分时候, ThreadCache缓存都是足够的,不需要去访问
- CentralCache和HeapPage,无系统调用配合无锁分配,分配效率是非常高的
- 中对象分配流程
	- 直接在PageHeap中选择适当的大小即可,128Page的Span,所保存的最大内存就是1MB
- 大对象分配流程
	- 从largespanset选择合适数量的页面组成span,用来存储数居

### Go语言内存分配

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241109121246.png" alt="image.png" style="zoom:60%;" />

- mcache:小对象的内存分配直接走
	- size class从1到66,每个class两个span
	- Span大小是8KB,按span class大小切分
- mcentral
	- Span内的所有内存块都被占用时,没有剩余空间继续分配对象,mcache会向mcentral申请1个span,mcache拿到span后继续分配对象
	- 当mcentral向mcache提供span时,如果没有符合条件的sp.an,mcentral会向mheap申请span
- mheap
	- 当mheap没有足够的内存时,mheap会向OS申请内存
	- Mheap把Span组织成了树结构,而不是链表
	- 然后把Span分配到heapArena进行管理,它包含地址映射和span是否包含指针等位图
		- 为了更高效的分配、回收和再利用内存


## 内存回收

- 引用计数(Python,PHP,Swift)
	- 对每一个对象维护一个引用计数,当引用该对象的对象被销毁的时候,引用计数减1,当引用计数为0的时候,回收该对象
	- 优点:对象可以很快的被回收,不会出现内存耗尽或达到某个阀值时才回收
	- 缺点:不能很好的处理循环引用,而且实时维护引用计数,有有也一定的代价
- 标记-清除(Golang)
	- 从根变量开始遍历所有引用的对象,引用的对象标记为"被引用",没有被标记的进行回收
	- 优点:解决引用计数的缺点
	- 缺点:需要STW(stop the word),即要暂停程序运行
- 分代收集(Java)
	- 按照生命周期进行划分不同的代空间,生命周期长的放入老年年代,短的放入新生代,新生代的回收频率高于老年
	- 代的频率

### GC

#### 三色标记法

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241109121820.png" alt="image.png" style="zoom:60%;" />

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241109121901.png" alt="image.png" style="zoom:60%;" />


<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241109122016.png" alt="image.png" style="zoom:60%;" />

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241109123733.png" alt="image.png" style="zoom:60%;" />
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241109123757.png" alt="image.png" style="zoom:60%;" />


- GC 开始时，认为所有 object 都是 白色，即垃圾。
- 从 root 区开始遍历，被触达的 object 置成 灰色。
- 遍历所有灰色 object，将他们内部的引用变量置成 **灰色**，自身置成 **黑色**
- 循环第 3 步，直到没有灰色object了，只剩下了黑白两种，白色的都是垃圾。
- 对于黑色 object，如果在标记期间发生了写操作，写屏障会在真正赋值前将新对象标记为灰色。
- 标记过程中，mallocgc新分配的 object，会先被标记成黑色再返回。

- 强三色不变式
	- 不存在黑色对象引用白色对象
- 弱三色不变式
	- 所有被黑色对象引用的白色对象必须有灰色上游对象

#### 写屏障

在特定程序运行时刻,当对对象的指针进行**写操作**(即修改对象的引用关系)时触发的**特殊处理机制**。 其主要目的是确保垃圾回收过程中能够正确地追踪对象的引用关系,防止出现错误的回收或遗漏未回收的情况 仅作用域堆上内存回收

##### Dijkstra插入写屏障 

在并发标记阶段,当一个**黑色对象**(已被标记为可达的对象)新指向一个白色对象(未被标记为可达的对象)时,将白色对象标记为灰色。

##### Yuasa删除写屏障
在并发标记阶段,当一个**灰色对象**(正在被标记但还未完成标记的对象)删除指向一个白色对象(未被标记为可达的对象)的指针时,将白色对象标记为灰色

##### 混合写屏障
插入写屏障+删除写屏障



## 内存逃逸

1. 什么是逃逸?
逃逸是指在函数内部创建的对象或变量,在函数结束后仍然被**其他部分引用或持有**




- 写屏障
	- 在特定程序运行时刻,当对对象的指针进行写操作(即修改对象的引用关系)时触发的特殊处理机制。其主要目的是确保垃圾回收过程中能够正确地追踪对象的引用关系,防止出现错误的回收或遗漏未回收的情况
- 仅作用域堆上内存回收

### 内存逃逸原因


1. 指针逃逸

```go
// 典型的指针逃逸
func newValue() *int {
    x := 42
    return &x  // x逃逸到堆上，因为返回了局部变量的地址
}

// 间接赋值导致逃逸
func indirect() {
    x := 42
    p := &x
    global = p  // x逃逸到堆上，因为指针被赋值给全局变量
}
```

2. 栈空间大小不确定
```go
// 编译期无法确定大小
func makeSlice(size int) []int {
    return make([]int, size)  // 可能逃逸到堆上
}

// 大对象
func makeLargeArray() [1024000]int {
    var arr [1024000]int  // 可能逃逸到堆上，因为太大
    return arr
}
```

3. 接口类型导致的逃逸


```go
// 接口类型转换导致逃逸
func printInterface(v interface{}) {
    fmt.Println(v)  // v逃逸到堆上
}

// 具体类型到接口的转换
type Stringer interface {
    String() string
}

func toString(s Stringer) string {
    return s.String()  // s可能逃逸到堆上
}
```

4. 闭包捕获变量

```go
// 闭包导致变量逃逸
func newClosure() func() int {
    x := 0
    return func() int {
        x++  // x逃逸到堆上，因为被闭包引用
        return x
    }
}

// 多层闭包
func multiClosure() func() func()  int {
    x := 0
    return func() func() int {
        y := x
        return func() int {
            return y  // x和y都会逃逸
        }
    }
}
```

5. channel/slice/map 存储指针
```go
// channel存储指针导致逃逸
func chanPointer() {
    ch := make(chan *int)
    x := 42
    ch <- &x  // x逃逸到堆上
}

// slice存储指针
func slicePointer() {
    s := make([]*int, 0)
    x := 42
    s = append(s, &x)  // x逃逸到堆上
}

// map存储指针
func mapPointer() {
    m := make(map[string]*int)
    x := 42
    m["key"] = &x  // x逃逸到堆上
}
```

6. 函数参数逃逸
```go
// 参数为interface{}
func takeInterface(v interface{}) {
    // v逃逸到堆上
}

// 参数为切片并且发生扩容
func appendSlice(s []int) []int {
    return append(s, 1)  // s可能逃逸到堆上
}
```


### 内存逃逸分析

使用编译器命令分析：

```
# 基本分析命令
go build -gcflags="-m" main.go

# 详细分析
go build -gcflags="-m -m" main.go

# 输出汇编代码
go build -gcflags="-S" main.go
```


# Go modules

### 什么是go modules?

Go Modules 是 Go 语言的依赖管理系统，从 Go 1.11 版本引入，并在 Go 1.13 版本中成为默认的依赖管理方式。它解决了早期 Go 项目在依赖管理方面的诸多问题，提供了一种更加强大、灵活和可靠的方式来管理项目依赖。以下是 Go Modules 的主要特点和概念：

1. 版本化依赖管理：
   - 允许指定依赖的具体版本。
   - 支持语义化版本控制（Semantic Versioning）。
2. 去中心化：
   - 不需要中央仓库，可以直接从**源代码仓库**（如 GitHub）获取依赖。
3. 可重现的构建：
   - 保证在不同环境下构建结果的一致性。
4. go.mod 文件：
   - 项目的**核心配置文件**，记录所有依赖及其版本。
   - 由 Go 命令自动维护。
5. go.sum 文件：
   - 包含依赖包的**加密校验和**，用于验证下载的模块。
6. 模块路径：
   - 每个模块都有一个唯一的模块路径，通常对应于代码仓库的位置。
7. 版本选择：
   - 自动选择最兼容的依赖版本。
   - 支持手动指定版本或版本范围。
8. 工作区外的项目支持：
   - 不再强制要求项目位于 GOPATH 中。
9. 缓存机制：
   - 下载的模块会被缓存，提高构建速度。
10. 代理支持：
    - 可以使用模块代理来加速依赖下载，提高可用性。
11. 版本控制：
    - 支持通过 git tags 来管理版本。
12. 向后兼容：
    - 旧项目可以逐步迁移到 Go Modules。

### GOPATH的工作模式

Go Modoules的目的之一就是淘汰GOPATH,  那么GOPATH是个什么?
为什么在 Go1.11 前就使用 GOPATH，而 Go1.11 后就开始逐步建议使用 Go modules，不再推荐 GOPATH 的模式了呢？

1. Wait is GOPATH?
	```sh
	$ go env
	
	GOPATH="/home/itheima/go"
	...
	```

	我们输入go env命令行后可以查看到 GOPATH 变量的结果，我们进入到该目录下进行查看，如下：

	```sh
	go
	├── bin
	├── pkg
	└── src
	    ├── github.com
	    ├── golang.org
	    ├── google.golang.org
	    ├── gopkg.in
	    ....
	```


	GOPATH目录下一共包含了三个子目录，分别是：
	- bin：存储所编译生成的二进制文件。
	- pkg：存储预编译的目标文件，以加快程序的后续编译速度。
	- src：存储所有`.go`文件或源代码。在编写 Go 应用程序，程序包和库时，一般会以`$GOPATH/src/github.com/foo/bar`的路径进行存放。
	
	因此在使用 GOPATH 模式下，我们需要将应用代码存放在固定的`$GOPATH/src`目录下，并且如果执行`go get`来拉取外部依赖会自动下载并安装到`$GOPATH`目录下。


2. GOPATH模式的弊端

	在 GOPATH 的 `$GOPATH/src` 下进行 `.go` 文件或源代码的存储，我们可以称其为 GOPATH 的模式，这个模式拥有一些弊端.
	
	- **A. 无版本控制概念.** 在执行`go get`的时候，你无法传达任何的版本信息的期望，也就是说你也无法知道自己当前更新的是哪一个版本，也无法通过指定来拉取自己所期望的具体版本。
	- **B.无法同步一致第三方版本号.** 在运行 Go 应用程序的时候，你无法保证其它人与你所期望依赖的第三方库是相同的版本，也就是说在项目依赖库的管理上，你无法保证所有人的依赖版本都一致。
	- **C.无法指定当前项目引用的第三方版本号.** 你没办法处理 v1、v2、v3 等等不同版本的引用问题，因为 GOPATH 模式下的导入路径都是一样的，都是`github.com/foo/bar`。



### Go Modules的模式基础环境说明


使用 Go Modules 的基本步骤：

1. go mod 命令

|         操作         |                命令                 |
| :----------------: | :-------------------------------: |
|       初始化模块        |    `go mod init <module-path>`    |
|        下载依赖        |         `go mod download`         |
|        添加依赖        | `go get <package-path>@<version>` |
|        更新依赖        |            `go get -u`            |
|      整理和清理依赖       |           `go mod tidy`           |
|        验证依赖        |          `go mod verify`          |
|      查看现有的依赖       |          `go mode graph`          |
| 导出项目所有的依赖到vendor目录 |          `go mod vendor`          |
|    查看为什么需要依赖某模块    |           `go mode why`           |
2. go mod 环境变量

	可以通过 go env 命令来进行查看

	```sh
	$ go env
	GO111MODULE="auto"
	GOPROXY="https://proxy.golang.org,direct"
	GONOPROXY=""
	GOSUMDB="sum.golang.org"
	GONOSUMDB=""
	GOPRIVATE=""
	...
	```

 - GO111MODULE
	Go语言提供了 `GO111MODULE`这个环境变量来作为Go modules的开关，其允许设置以下参数：
	- auto：只要项目包含了 go.mod 文件的话启用Go modules，目前在 Go1.11 至 Go1.14 中仍然是默认值。
	- on：启用 Go modules，推荐设置，将会是未来版本中的默认值。
	- off：禁用 Go modules，不推荐设置。
	
	可以通过来设置
	
	```go
	$ go env -w GO111MODULE=on
	```

- GOPROXY
	这个环境变量主要是用于设置 Go 模块代理（Go module proxy）,其作用是用于使 Go 在后续拉取模块版本时直接通过镜像站点来快速拉取。
	GOPROXY 的默认值是：https://proxy.golang.org,direct
	如：
	```go
	$ go env -w GOPROXY=https://goproxy.cn,https://mirrors.aliyun.com/goproxy/,direct
	```
- GOSUMDB
	它的值是一个 `Go checksum database`，用于在拉取模块版本时（无论是从源站拉取还是通过 Go module proxy 拉取）保证拉取到的模块版本数据未经过篡改，若发现不一致，也就是可能存在篡改，将会立即中止。
	
	GOSUMDB 的默认值为：sum.golang.org，在国内也是无法访问的，但是 GOSUMDB 可以被 Go 模块代理所代理（详见：Proxying a Checksum Database）。
	
	因此我们可以通过设置GOPROXY来解决，而先前我们所设置的模块代理 goproxy.cn 就能支持代理 sum.golang.org，所以这一个问题在设置GOPROXY后，你可以不需要过度关心。
	
	另外若对 GOSUMDB 的值有自定义需求，其支持如下格式：
	- 格式 1：`<SUMDB_NAME>+<PUBLIC_KEY>`。
	- 格式 2：`<SUMDB_NAME>+<PUBLIC_KEY> <SUMDB_URL>`。
	也可以将其设置为“off”，也就是禁止 Go 在后续操作中校验模块版本。

- `GONOPROXY/GONOSUMDB/GOPRIVATE`
	这三个环境变量都是用在当前项目依赖了私有模块，例如像是你公司的私有git仓库，又或是github中的私有库，都是属于私有模块，都是要进行设置的，否则会拉取失败。
	
	更细致来讲，就是依赖了由GOPROXY指定的Go模块代理或由GOSUMDB指定Go checksum database都无法访问到的模块时的场景。
	
	而一般建议直接设置 GOPRIVATE，它的值将作为GONOPROXY和GONOSUMDB的默认值，所以建议的最佳姿势是直接使用GOPRIVATE。
	
	并且它们的值都是一个以英文逗号 “,” 分割的模块路径前缀，也就是可以设置多个，例如：
	```sh
	$ go env -w GOPRIVATE="git.example.com,github.com/eddycjy/mquote"
	```
	设置后，前缀为 git.xxx.com 和 github.com/eddycjy/mquote 的模块都会被认为是私有模块。
	如果不想每次都重新设置，我们也可以利用通配符，例如：
	```sh
	$ go env -w GOPRIVATE="*.example.com"
	```
	这样子设置的话，所有模块路径为 example.com 的子域名（例如：git.example.com）都将不经过 Go module proxy 和 Go checksum database，**需要注意的是不包括 example.com 本身**。
 
### 使用Go Modules初始化项目


1. 开启Go Modules
	
	```sh
	 $ go env -w GO111MODULE=on
	```
	
	又或是可以通过直接设置系统环境变量（写入对应的`~/.bash_profile` 文件亦可）来实现这个目的：
	
	```sh
	$ export GO111MODULE=on
	```

2. 初始化项目  
  
	创建项目目录
	
	```sh
	$ mkdir -p $HOME/aceld/modules_test
	$ cd $HOME/aceld/modules_test
	```
	
	执行Go modules 初始化
	
	```sh
	$ go mod init github.com/aceld/modules_test
	go: creating new go.mod: module github.com/aceld/modules_test
	```


在执行 `go mod init` 命令时，我们指定了模块导入路径为 `github.com/aceld/modules_test`。接下来我们在该项目根目录下创建 `main.go` 文件，如下：

```go
package main

import (
    "fmt"
    "github.com/aceld/zinx/znet"
    "github.com/aceld/zinx/ziface"
)

//ping test 自定义路由
type PingRouter struct {
    znet.BaseRouter
}

//Ping Handle
func (this *PingRouter) Handle(request ziface.IRequest) {
    //先读取客户端的数据
    fmt.Println("recv from client : msgId=", request.GetMsgID(), 
              ", data=", string(request.GetData()))

    //再回写ping...ping...ping
    err := request.GetConnection().SendBuffMsg(0, []byte("ping...ping...ping"))
    if err != nil {
      fmt.Println(err)
    }
}

func main() {
    //1 创建一个server句柄
    s := znet.NewServer()

    //2 配置路由
    s.AddRouter(0, &PingRouter{})

    //3 开启服务
    s.Serve()
}
```

OK, 我们先不要关注代码本身,我们看当前的main.go也就是我们的`aceld/modules_test`项目,是依赖一个叫`github.com/aceld/zinx`库的. znet和ziface只是zinx的两个模块.  
  
接下来我们在`$HOME/aceld/modules_test`,本项目的根目录执行

```bash
$ go get github.com/aceld/zinx/znet

go: downloading github.com/aceld/zinx v0.0.0-20200221135252-8a8954e75100
go: found github.com/aceld/zinx/znet in github.com/aceld/zinx v0.0.0-20200221135252-8a8954e75100
```

我们会看到 我们的`go.mod`被修改,同时多了一个`go.sum`文件.

查看go.mod文件

> 	aceld/modules_test/go.mod\

```go
module github.com/aceld/modules_test

go 1.14

require github.com/aceld/zinx v0.0.0-20200221135252-8a8954e75100 // indirect
```

我们来简单看一下这里面的关键字
`module`: 用于定义当前项目的模块路径

`go`:标识当前Go版本.即初始化版本

`require`: 当前项目依赖的一个特定的必须版本

`// indirect`: 示该模块为间接依赖，也就是在当前应用程序中的 import 语句中，并没有发现这个模块的明确引用，有可能是你先手动 `go get` 拉取下来的，也有可能是你所依赖的模块所依赖的.我们的代码很明显是依赖的`"github.com/aceld/zinx/znet"`和`"github.com/aceld/zinx/ziface"`,所以就间接的依赖了`github.com/aceld/zinx`

查看go.sum文件

在第一次拉取模块依赖后，会发现多出了一个 go.sum 文件，其详细罗列了当前项目直接或间接依赖的所有模块版本，并写明了那些模块版本的 SHA-256 哈希值以备 Go 在今后的操作中保证项目所依赖的那些模块版本不会被篡改。

```bash
github.com/aceld/zinx v0.0.0-20200221135252-8a8954e75100 h1:Ez5iM6cKGMtqvIJ8nvR9h74Ln8FvFDgfb7bJIbrKv54=
github.com/aceld/zinx v0.0.0-20200221135252-8a8954e75100/go.mod h1:bMiERrPdR8FzpBOo86nhWWmeHJ1cCaqVvWKCGcDVJ5M=
github.com/golang/protobuf v1.3.3/go.mod h1:vzj43D7+SQXF/4pzW/hwtAqwc6iTitCiVSaWz5lYuqw=
```


我们可以看到一个模块路径可能有如下两种：

h1:hash情况

```bash
github.com/aceld/zinx v0.0.0-20200221135252-8a8954e75100 h1:Ez5iM6cKGMtqvIJ8nvR9h74Ln8FvFDgfb7bJIbrKv54=
```

go.mod hash情况 

```bash
github.com/aceld/zinx v0.0.0-20200221135252-8a8954e75100/go.mod h1:bMiERrPdR8FzpBOo86nhWWmeHJ1cCaqVvWKCGcDVJ5M=
github.com/golang/protobuf v1.3.3/go.mod h1:vzj43D7+SQXF/4pzW/hwtAqwc6iTitCiVSaWz5lYuqw=
```

h1 hash 是 Go modules 将目标模块版本的 zip 文件开包后，针对所有包内文件依次进行 hash，然后再把它们的 hash 结果按照固定格式和算法组成总的 hash 值。

而 h1 hash 和 go.mod hash 两者，要不就是同时存在，要不就是只存在 go.mod hash。那什么情况下会不存在 h1 hash 呢，就是当 Go 认为肯定用不到某个模块版本的时候就会省略它的 h1 hash，就会出现不存在 h1 hash，只存在 go.mod hash 的情况。

### 修改模块的版本依赖关系

为了作尝试,假定我们现在都zinx版本作了升级, 由`zinx v0.0.0-20200221135252-8a8954e75100` 升级到 `zinx v0.0.0-20200306023939-bc416543ae24` (注意zinx是一个没有打版本tag打第三方库,如果有的版本号是有tag的,那么可以直接对应v后面的版本号即可)

那么,我们是怎么知道zinx做了升级呢, 我们又是如何知道的最新的`zinx`版本号是多少呢?

先回到`$HOME/aceld/modules_test`,本项目的根目录执行

```bash
$ go get github.com/aceld/zinx/znet
go: downloading github.com/aceld/zinx v0.0.0-20200306023939-bc416543ae24
go: found github.com/aceld/zinx/znet in github.com/aceld/zinx v0.0.0-20200306023939-bc416543ae24
go: github.com/aceld/zinx upgrade => v0.0.0-20200306023939-bc416543ae24
```

这样我们,下载了最新的zinx, 版本是`v0.0.0-20200306023939-bc416543ae24`
然后,我么看一下go.mod

```go
module github.com/aceld/modules_test
go 1.14
require github.com/aceld/zinx v0.0.0-20200306023939-bc416543ae24 // indirect
```

我们会看到,当我们执行`go get` 的时候, 会自动的将本地将当前项目的`require`更新了.变成了最新的依赖.
好了, 现在我们就要做另外一件事,就是,我们想用一个旧版本的zinx. 来修改当前`zinx`模块的依赖版本号.
目前我们在`$GOPATH/pkg/mod/github.com/aceld`下,已经有了两个版本的zinx库

```bash
/go/pkg/mod/github.com/aceld$ ls
zinx@v0.0.0-20200221135252-8a8954e75100
zinx@v0.0.0-20200306023939-bc416543ae24
```

目前,我们`/aceld/modules_test`依赖的是`zinx@v0.0.0-20200306023939-bc416543ae24` 这个是最新版, 我们要改成之前的版本`zinx@v0.0.0-20200306023939-bc416543ae24`.

回到`/aceld/modules_test`项目目录下,执行

```bash
$ go mod edit -replace=zinx@v0.0.0-20200306023939-bc416543ae24=zinx@v0.0.0-20200221135252-8a8954e75100
```

然后我们打开go.mod查看一下

```go
module github.com/aceld/modules_test
go 1.14
require github.com/aceld/zinx v0.0.0-20200306023939-bc416543ae24 // indirect
replace zinx v0.0.0-20200306023939-bc416543ae24 => zinx v0.0.0-20200221135252-8a8954e75100
```

这里出现了`replace`关键字.用于将一个模块版本替换为另外一个模块版本。


# go web

## http库

```go
package main

import (
	"fmt"
	"log"
	"net/http"
)

func main() {
	http.HandleFunc("/", home)
	http.HandleFunc("/user/", user)
	http.HandleFunc("/user/creater", createrUser)
	http.HandleFunc("/order", order)
	log.Fatal(http.ListenAndServe(":8080", nil))
}

func home(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hi there, I love %s", r.URL.Path[1:])
}

func user(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "这是用户")
}

func createrUser(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "这是创建用户")
}

func order(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "这是订单")
}
```

这是go写一个webserver的简单代码，当你访问 `http://localhost:8080/golang `时，整个过程如下：

1. 客户端发起请求：
- 浏览器向 localhost:8080 发送 HTTP 请求
- 请求包含路径 "/golang"
- HTTP 方法默认为 GET

2. 服务器接收请求：
```go
log.Fatal(http.ListenAndServe(":8080", nil)) // 这行代码内部实际上完成了: 创建 socket bind 绑定端口 listen 监听连接 accept 接受连接
```
- ListenAndServe 在 8080 端口持续监听 HTTP 请求
- 当请求到来时，创建新的 goroutine 处理请求
- nil 参数表示使用默认的 ServeMux（HTTP 请求路由器）

3. 路由匹配：
```go
http.HandleFunc("/", handler)
```
- DefaultServeMux 查找与请求路径匹配的处理函数
- "/" 模式会匹配所有的路径
- 找到之前注册的 handler 函数

4. 请求处理：
```go
func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hi there, I love %s!", r.URL.Path[1:])
}
```
- 创建 http.ResponseWriter 和 http.Request 对象
- r.URL.Path 获取请求路径 "/golang"
- `[1:] `切片操作去掉路径开头的 "/"，得到 "golang"
- fmt.Fprintf 将格式化的字符串写入响应

5. 响应返回：
- 服务器将响应内容 "Hi there, I love golang!" 发送回客户端
- 浏览器接收响应并显示内容

完整的请求-响应过程示意：
```
客户端                                          服务器
   |                                             |
   |  GET /golang HTTP/1.1                       |
   | -----------------------------------------> |
   |                                             | 
   |                                     查找处理函数
   |                                     执行 handler
   |                                             |
   |  HTTP/1.1 200 OK                           |
   |  Content-Type: text/plain                   |
   |  Content-Length: 23                         |
   |                                             |
   |  Hi there, I love golang!                   |
   | <----------------------------------------- |
   |                                             |
```

这就是一个完整的 HTTP 请求处理流程。需要我详细解释某个具体环节吗？

### Request Body

- Body 和 GetBody
- URL
- Method
- Header
- Form
- ...

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241116111351.png" alt="image.png" style="zoom:60%;" />


#### GetBody

- Body：只能读取一次，意味着你读了别人就不能读了；别人读了你就不能读了；
- GetBody：原则上是可以多次读取，但是在原生的http.Request里面，这个是 nil
- 在读取到 body 之后，我们就可以用于反序列化，比如说将json格式的字符串转化为一个对象等

```go
func readBodyOnce(w http.ResponseWriter, r *http.Request) {
	body, err := io.ReadAll(r.Body)
	if err != nil {
		fmt.Fprintf(w, "read body failed: %v", err)
		// 记住要返回，不然就还会执行后面的代码
		return
	}
	// 类型转换，将 []byte 转换为 string
	fmt.Fprintf(w, "read the data: %s \n", string(body))

	// 尝试再次读取，啥也读不到，但是也不会报错
	body, err = io.ReadAll(r.Body)
	if err != nil {
		// 不会进来这里
		fmt.Fprintf(w, "read the data one more time got error: %v", err)
		return
	}
	fmt.Fprintf(w, "read the data one more time: [%s] and read data length %d \n", string(body), len(body))
}
```


```go
func getBodyIsNil(w http.ResponseWriter, r *http.Request) {
	if r.GetBody == nil {
		fmt.Fprintf(w, "GetBody is nil \n")
	} else {
		fmt.Fprintf(w, "GetBody not nil \n")
	}
}

```


如果想读取多次，可以自己保存第一次读取的内容，并创建新的 Reader（推荐方式）：

```go
func readBodyMultiTimes(w http.ResponseWriter, r *http.Request) {
    // 读取原始数据
    body, _ := io.ReadAll(r.Body)
    
    // 第一次使用
    fmt.Fprintf(w, "First read: %s\n", string(body))
    
    // 创建新的 Reader
    r.Body = io.NopCloser(bytes.NewBuffer(body))
    
    // 第二次读取
    secondBody, _ := io.ReadAll(r.Body)
    fmt.Fprintf(w, "Second read: %s\n", string(secondBody))
    
    // 如果还需要读取，可以继续创建新的 Reader
    r.Body = io.NopCloser(bytes.NewBuffer(body))
}
```

### Request Query

- 除了 Body，我们还可能传递参数的地方是 Query
- 也就是
	- `http://xxx.com/your/path?id=123&b=456`
- 所有的值都被解释为字符串，所以需要自己解析为数字等

```go
func queryParms(w http.ResponseWriter, r *http.Request) {
	values := r.URL.Query()
	fmt.Fprintf(w, "query is %#v\n", values)
}
```

在postman上面测试结果为

```
query is map[a:[123 145] b:[345]]
```

### Request URL

包含路径方面的所有信息和一些很有用的操作

**1. 基本组成部分**：
```go
// 示例 URL: https://user:pass@example.com:8080/path?query=value#fragment
type URL struct {
    Scheme     string    // 协议：http, https, ftp 等
    Opaque     string    // 编码后的不透明数据，用于替代 path（很少使用）
    User       *Userinfo // 用户信息（用户名和可选的密码）
    Host       string    // host 或 host:port
    Path       string    // 路径部分
    RawPath    string    // 编码后的路径
    ForceQuery bool      // 即使没有查询参数也添加 ?
    RawQuery   string    // 编码后的查询字符串
    Fragment   string    // 引用的片段（# 后面的部分）
    RawFragment string   // 编码后的片段
}
```

**2. 实际例子**：
```go
// 示例 1: 完整 URL
fullURL := "https://user:pass@example.com:8080/path?key=value#section"
u, _ := url.Parse(fullURL)
fmt.Printf("Scheme: %s\n", u.Scheme)     // https
fmt.Printf("User: %v\n", u.User)         // user:pass
fmt.Printf("Host: %s\n", u.Host)         // example.com:8080
fmt.Printf("Path: %s\n", u.Path)         // /path
fmt.Printf("RawQuery: %s\n", u.RawQuery) // key=value
fmt.Printf("Fragment: %s\n", u.Fragment) // section

// 示例 2: 简单路径
simpleURL := "/wholeUrl"
u2, _ := url.Parse(simpleURL)
// 这就是你看到的情况，只有 Path 有值
```


```go
func wholeUrl(w http.ResponseWriter, r *http.Request) {
	data, _ := json.Marshal(r.URL)
	fmt.Fprintf(w, string(data))
}
```

- URL 里面 Host 不一定有值
- r.Host 一般都有值，是Host这个header的值
- RawPath 也是不一定有
- Path肯定有

```get
localhost:8080//wholeUrl
```


```json
{
    "Scheme": "",
    "Opaque": "",
    "User": null,
    "Host": "",
    "Path": "/wholeUrl",
    "RawPath": "",
    "OmitHost": false,
    "ForceQuery": false,
    "RawQuery": "",
    "Fragment": "",
    "RawFragment": ""
}
```

### Request Header

- header大体上是两类，一类是http 预定义的；一类是自己定义的
- Go 会自动将 header 名字转为标准名字——其实就是大小写调整
- 一般用 X 开头来表明是自己定义的，比如说 X-mycompany-your=header

```go
func header(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "head is %v\n", r.Header)
}
```

```
head is map[Accept:[*/*] Accept-Encoding:[gzip, deflate, br] Connection:[keep-alive] Referer:[http://localhost:8080//header] User-Agent:[Apifox/1.0.0 (https://apifox.com)]]
```

### Form

- Form 和 ParseForm
- 要先调用 ParseForm
- 建议加上 Content-Type: application/x-www-form-urlencoded

```go
func form(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "before parse form %v\n", r.Form)
	err := r.ParseForm()
	if err != nil {
		fmt.Fprintf(w, "parse form error %v\n", r.Form)
	}
	fmt.Fprintf(w, "before parse form %v\n", r.Form)
}
```

Form 和 Query 确实有相似之处，但也有重要区别。让我解释：

1. **来源不同**：
```go
func handler(w http.ResponseWriter, r *http.Request) {
    // Query 参数来自 URL
    // 例如：/api?name=tom&age=20
    queryParams := r.URL.Query()
    
    // Form 数据可以来自：
    // 1. URL 查询参数
    // 2. POST 请求体（application/x-www-form-urlencoded）
    // 3. POST 请求体（multipart/form-data）
    r.ParseForm()
    formParams := r.Form
}
```

2. **具体区别**：
```go
func example(w http.ResponseWriter, r *http.Request) {
    // 1. Query 只包含 URL 中的参数
    urlQuery := r.URL.Query()
    
    // 2. Form 包含 URL 参数和 POST 表单数据的合并
    r.ParseForm()
    formAll := r.Form
    
    // 3. PostForm 只包含 POST 表单数据
    postOnly := r.PostForm
}
```

3. **实际应用示例**：
```go
func handleRequest(w http.ResponseWriter, r *http.Request) {
    // URL: /api?id=123&name=tom
    // POST body: name=jerry&age=20
    
    // 解析表单
    r.ParseForm()
    
    // Query 只能获取 URL 参数
    queryID := r.URL.Query().Get("id")     // "123"
    queryName := r.URL.Query().Get("name") // "tom"
    
    // Form 获取所有参数（URL + POST）
    formID := r.Form.Get("id")      // "123"
    formName := r.Form.Get("name")  // ["tom", "jerry"]
    formAge := r.Form.Get("age")    // "20"
    
    // PostForm 只获取 POST 数据
    postName := r.PostForm.Get("name") // "jerry"
    postAge := r.PostForm.Get("age")   // "20"
}
```

4. **不同请求方法的表现**：
```go
func demonstrateDifference(w http.ResponseWriter, r *http.Request) {
    // GET 请求：/api?name=tom
    if r.Method == http.MethodGet {
        name := r.URL.Query().Get("name") // "tom"
        r.ParseForm()
        formName := r.Form.Get("name")    // "tom"
        postName := r.PostForm.Get("name") // ""
    }
    
    // POST 请求：/api?name=tom
    // Body: name=jerry
    if r.Method == http.MethodPost {
        name := r.URL.Query().Get("name") // "tom"
        r.ParseForm()
        formName := r.Form.Get("name")    // ["tom", "jerry"]
        postName := r.PostForm.Get("name") // "jerry"
    }
}
```

5. **常见使用场景**：
```go
func commonUsage(w http.ResponseWriter, r *http.Request) {
    // 1. 简单的 GET 参数获取
    if r.Method == http.MethodGet {
        // 推荐使用 Query
        params := r.URL.Query()
        name := params.Get("name")
    }
    
    // 2. 处理表单提交
    if r.Method == http.MethodPost {
        // 必须先解析
        r.ParseForm()
        
        // 只需要 POST 数据时
        postData := r.PostForm
        
        // 需要所有数据时（包括 URL 参数）
        allData := r.Form
    }
    
    // 3. 文件上传
    if r.Method == http.MethodPost {
        // 对于 multipart/form-data
        r.ParseMultipartForm(10 << 20) // 10 MB
        file, header, err := r.FormFile("file")
    }
}
```

6. **主要差异总结**：

```go
// 差异点：
// 1. 数据来源
//    Query: 仅 URL 参数
//    Form:  URL 参数 + POST 数据
//
// 2. 解析要求
//    Query: 无需显式解析
//    Form:  需要调用 ParseForm()
//
// 3. 使用场景
//    Query: GET 请求参数
//    Form:  表单提交、文件上传
//
// 4. 数据格式
//    Query: application/x-www-form-urlencoded
//    Form:  支持 urlencoded 和 multipart/form-data
```

选择建议：
1. GET 请求用 Query
2. POST 表单提交用 Form/PostForm
3. 需要同时处理 URL 参数和表单数据时用 Form
4. 只需要 POST 数据时用 PostForm

这样可以让代码更清晰且符合 HTTP 语义。


# 项目案例

## 即时通信系统

### 构建基础Server

- main.go

	```go
	package main
	
	func main (){
		server := NewServer("127.0.0.1",8888)
		server.Start()
	}
	```

- server.go

	server类型
	
	```go
	type Server struct {
		Ip   string
		Port int
	}
	
	```


	创建一个server对象
	
	```go
	//创建一个server的接口
	func NewServer(ip string, port int) *Server {
		server := &Server{
			Ip:   ip,
			Port: port,
		}
	
		return server
	}
```

	启动server对象

	```go
	//启动服务器的接口
	func (this *Server) Start() {
		//socket listen
		listener, err := net.Listen("tcp", fmt.Sprintf("%s:%d", this.Ip, this.Port))
		if err != nil {
			fmt.Println("net.Listen err:", err)
			return
		}
		//close listen socket
		defer listener.Close()
	
		for {
			//accept
			conn, err := listener.Accept()
			if err != nil {
				fmt.Println("listener accept err:", err)
				continue
			}
	
			//do handler
			go this.Handler(conn)
		}
	}
	```

	处理链接的对象
	
	```go
	func (this *Server) Handler(conn net.Conn) {
		//...当前链接的业务
		fmt.Println("链接建立成功")
	}
	```




