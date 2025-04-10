
C++文档工具Doxgen--- [Doxygen 10 分钟入门教程 | Simple](https://cedar-renjun.github.io/2014/03/21/learn-doxygen-in-10-minutes/)

# Protobuf

## Protobuf概述
下面的话是识别的，加回车或者其他语法，当markdown

protobuf也叫`protocol buffer`是google的一**种数据交换的格式**,是一种轻便高效的**序列化数据结构的协议**，它独立于语言,独立于平台。google提供了多种语言的实现:`java、c#、c++、go`和`python`等,每一种实现都包含了相应语言的编译器以及库文件。

由于它是一种**二进制的格式**,比使用xml、json进行数据交换许多。可以把它用于**分布式应用之间**的数据通信或者异构环境下的数据交换。
作为一种效率和兼容性都很优秀的二进制数据传输格式,可以用于诸如网络传输、配置文件、数据存储等诸多领域。

### 源码安装

安装看[这里](https://blog.csdn.net/j8267643/article/details/134133091)

### 如何使用
在学习protbuf之前，我们需要搞清楚使用protbuf进行数据的序列化主要有哪几个步骤：
确定数据格式，数据可简单可复杂，比如：
```c==
// 要序列化的数据
// 第一种: 单一数据类型
int number;

// 第二种: 复合数据类型
struct Person
{
    int id;
    string name;
    string sex;	
    int age;
};
```

2. 创建一个新的文件, 文件名随意指定, 文件后缀为`.proto`
3. 根据`protobuf`的语法, 编辑`.proto`文件
4. 使用`protoc`命令将 `.proto` 文件转化为相应的 C++ 文件
	- 源文件:`xxx.pb.cc –> xxx`对应的名字和`.proto`文件名相同
	- 头文件:`xxx.pb.h –> xxx`对应的名字和`.proto`文件名相同
5. 需要将生成的c++文件添加到项目中, 通过文件中提供的类 API 实现数据的序列化/反序列化


protobuf中的数据类型 和 C++ 数据类型对照表:


| Protobuf 类型 |       C++ 类型        |                    备注                    |
| :---------: | :-----------------: | :--------------------------------------: |
|   double    |       double        |                  64位浮点数                  |
|    float    |        float        |                  32位浮点数                  |
|    int32    |         int         |                  32位整数                   |
|    int64    |        long         |                  64位整数                   |
|   uint32    |    unsigned int     |                 32位无符号整数                 |
|   uint64    |    unsigned long    |                 64位无符号整数                 |
|   sint32    |     signed int      |           32位整数，处理负数效率比int32更高           |
|   sint64    |     signed long     |           64位整数，处理负数效率比int64更高           |
|   fixed32   |    unsigned int     |   总是4个字节。如果数值总是比228大的话，这个类型会比uint32高效。   |
|   fixed64   | unsigned long (64位) | 总是8个字节。如果数值总是比总是比256大的话，这个类型会比uint64高效。  |
|  sfixed32   |      int (32位)      |                  总是4个字节                  |
|  sfixed64   |     long (64位)      |                  总是8个字节                  |
|    bool     |        bool         |                   布尔类型                   |
|   string    |       string        |    一个字符串必须是UTF-8编码或者7-bit ASCII编码的文本     |
|    bytes    |       string        | 处理多字节的语言字符、如中文, 建议protobuf中字符型类型使用 bytes |
|    enum     |        enum         |                    枚举                    |
|   message   |   object of class   |                 自定义的消息类型                 |

## Protobuf 语法
### 基本使用
假设我定义了这样一个结构体，现在要基于这个结构体完成数据的序列化，结构体原型如下：
```c++
struct Person
{
    int id;
    string name;
    string sex;	// man woman
    int age;
};
```

接下来，我们需要新建一个文件，给它起个名字，后缀指定为 .proto，在文件的第一行需要指定Protobuf的版本号，有两个版本Protobuf 2 和 Protobuf 3，此处我们使用的是版本3。

```PROTOBUF
syntax="proto3";
```

接着需要定义一个消息体，其格式如下：

```c
message 名字	
{
    // 类中的成员, 格式
    数据类型 成员名字 = 1;
    数据类型 成员名字 = 2;
    数据类型 成员名字 = 3;
	   ......     
	   ......
}
```

- message后面的名字就是生成的类的名字，自己指定一个合适的名字即可
- 等号后面的编号要从1开始，每个成员都有一个唯一的编号，不能重复，一般连续编号即可。

基于上面的语法，上面结构体对应的`.proto`文件的内容可以写成这样：

```c++
// Person.proto
syntax = "proto3";

// 在该文件中对要序列化的结构体进行描述
message Person
{
    int32 id = 1;
    bytes name = 2;
    bytes sex = 3;	
    int32 age = 4;
}
```

`.proto`文件编辑好之后就可以使用protoc工具将其转换为C++文件了。

```shell
$ protoc -I path .proto文件 --cpp_out=输出路径(存储生成的c++文件) 
```

在`protoc`命令中，`-I` 参数后面可以跟随一个或多个路径，用于告诉编译器在哪些路径下查找导入的文件或依赖的文件，使用绝对路径或相对路径都是没问题的。

例如，`protoc -I path1 -I path2` 或 `protoc -I path1:path2` 都表示告诉编译器在 `path1` 和 `path2` 路径下查找导入的文件或依赖的文件。

我们可以在`.proto`文件所在目录执行`protoc`命令，并生成到当前目录：

```shell
$ protoc ./Person.proto --cpp_out=. 
# 或者使用 -I 参数
$ protoc -I ./ Person.proto --cpp_out=.
```

### repeated 限定修饰符
在使用Protocol Buffers（Protobuf）中，可以使用repeated关键字作为限定修饰符来表示一个字段可以有多个值，即重复出现的字段。repeated关键字可以用于以下数据类型：基本数据类型、枚举类型和自定义消息类型。

比如要序列化的数据中有一个数组，结构如下：

```c++
// 要序列化的数据
struct Person
{
    int id;
    string name[10];
    string sex;	
    int age;
};
```

Protobuf 的消息体就可以写成下面的样子:

```c++
message Person
{
    int32 id = 1;
    repeated bytes name = 2;
    bytes sex = 3;	
    int32 age = 4;
}
```

使用`repeated`关键字定义的字段在`Protobuf`序列化和反序列化时会被当作一个集合或数组来处理。这个`name`可以作为一个动态数组来使用。

### 枚举
在 `Protocol Buffers` 中，枚举类型用于定义一组特定的取值。下面定义一个枚举，并将其添加到Person中：

```c
// 要序列化的数据
// 枚举
enum Color
{
    Red = 5,	// 可以不给初始值, 默认为0
    Green,
    Yellow,
    Blue
};

// 要序列化的数据
struct Person
{
    int id;
    string name[10];
    string sex;	
    int age;
    // 枚举类型
    Color color;
};
```

以下是如何在 `Protobuf` 中定义和使用枚举类型，其语法如下：

```PLAINTEXT
enum 名字
{
    元素名 = 0;	// 枚举中第一个原素的值必须为0
    元素名 = 数值;
}
```

枚举元素之间使用分号间隔;，并且需要注意一点`proto3`中的第一个枚举值必须为 0，第一个元素以外的元素值可以随意指定。
上面例子中的数据在`.proto`文件中可以写成如下格式:

```c++
// 定义枚举类型
enum Color
{
    Red = 0;
    Green = 3;		// 第一个元素以外的元素值可以随意指定
    Yellow = 6;
    Blue = 9;
}
// 在该文件中对要序列化的结构体进行描述
message Person
{
    int32 id = 1;
    repeated bytes name = 2;
    bytes sex = 3;	
    int32 age = 4;
    // 枚举类型
    Color color = 5;
}
```

### proto文件的导入
在 Protocol Buffers 中，可以使用import语句在当前.ptoto中导入其它的.proto文件。这样就可以在一个.proto文件中引用并使用其它文件中定义的消息类型和枚举类型。

语法格式如下:
```PROTOBUF
import "要使用的proto文件的名字";
```

假设现在我有一个`proto`文件`Address.proto`，里边记录了地址信息：
```PROTOBUF
syntax = "proto3";
// 地址信息
message Address
{
    bytes addr = 1;
    bytes number = 2;
}
```

我现需要再上面定义的`Person.proto`中添加这个人的地址信息，因此就需要用到`Address.proto`中定义的消息体：
```c++
syntax = "proto3";
// 使用另外一个proto文件中的数类型, 需要导入这个文件
import "Address.proto";

// 在该文件中对要序列化的结构体进行描述
// 定义枚举类型
enum Color
{
    Red = 0;
    Green = 3;		// 第一个元素以外的元素值可以随意指定
    Yellow = 6;
    Blue = 9;
}
// 在该文件中对要序列化的结构体进行描述
message Person
{
    int32 id = 1;
    repeated bytes name = 2;
    bytes sex = 3;	
    int32 age = 4;
    // 枚举类型
    Color color = 5;
    // 添加地址信息, 使用的是外部proto文件中定义的数据类型
    Address addr = 6;
}
```

- import语句中指定的文件路径可以是相对路径或绝对路径。如果文件在相同的目录中，只需指定文件名即可。
- 导入的文件将会在编译时与当前文件一起被编译。
- 导入的文件也可以继续导入其他文件，形成一个文件依赖的层次结构。

### 包(package)
在`Protobuf`中，可以使用package关键字来定义一个消息所属的包（package）。包是用于组织和命名消息类型的一种机制，类似于命名空间的概念。
在一个`.proto`文件中，可以通过在顶层使用package关键字来定义包：
```c++
syntax = "proto3";
package mypackage;
message MyMessage 
{
  // ...
}
```

在这个示例中，我们使用`package`关键字将`MyMessage`消息类型定义在名为`mypackage`的包中。包名作为一个标识符来命名，可以使用任何有效的标识符，按惯例使用小写字母和下划线。

使用包可以避免不同`.proto`文件中的消息类型名称冲突，同时也可以更好地组织和管理大型项目中的消息定义。可以将消息类型的名称定义在特定的包中，并使用限定名来引用这些类型。

下面有两个`proto`文件，分别给他们添加一个`package`：
proto文件 - Address.proto 
```PROTOBUF
syntax = "proto3";
// 添加命名空间 Dabing
package Dabing;

// 地址信息, 这个Address类属于命名空间: Dabing
message Address
{
    bytes addr = 1;
    bytes number = 2;
}
```

proto文件 - `Person.proto`

```c++
syntax = "proto3";
// 使用另外一个proto文件中的数类型, 需要导入这个文件
import "Address.proto";
// 指定命名空间 ErBing
package ErBing;

// 以下的类 Person 和枚举 Color 都属于命名空间 ErBing
// 在该文件中对要序列化的结构体进行描述
// 定义枚举类型
enum Color
{
    Red = 0;
    Green = 3;		// 第一个元素以外的元素值可以随意指定
    Yellow = 6;
    Blue = 9;
}
// 在该文件中对要序列化的结构体进行描述
message Person
{
    int32 id = 1;
    repeated bytes name = 2;
    bytes sex = 3;	
    int32 age = 4;
    // 枚举类型
    Color color = 5;
    // 添加地址信息, 使用的是外部proto文件中定义的数据类型
    // 如果这个外边类型属于某个命名空间, 语法格式:
    // 命名空间的名字.类名 变量名=编号;
    Dabing.Address addr = 6;
}
```

## 序列化和反序列化

###  `**.pb.h` 头文件
通过`protoc`命令对`.proto`文件的转换，得到的头文件中有一个类，这个类的名字和`.proto`文件中message关键字后边指定的名字相同，`.proto`文件中message消息体的成员就是生成的类的私有成员。

那么如何访问生成的类的私有成员呢？ 可以调用生成的类提供的公共成员函数，这些函数有如下规律：

- 清空(初始化) 私有成员的值: `clear_变量名()`
- 获取类私有成员的值: `变量名()`
- 给私有成员进行值的设置: `set_变量名(参数)`
- 得到类私有成员的地址, 通过这块地址读/写当前私有成员变量的值: `mutable_变量名()`
- 如果这个变量是数组类型:
	- 数组中元素的个数: `变量名_size()`
	- 添加一块内存, 存储新的元素数据: `add_变量名()` 、`add_变量名(参数)`

### 序列化
序列化是指将数据结构或对象转换为可以在储存或传输中使用的二进制格式的过程。在计算机科学中，序列化通常用于将内存中的对象持久化存储到磁盘上，或者在分布式系统中进行数据传输和通信。

`Protobuf`中为我们提供了相关的用于数据序列化的 API，如下所示：
```c++
// 头文件目录: google\protobuf\message_lite.h
// --- 将序列化的数据 数据保存到内存中
// 将类对象中的数据序列化为字符串, c++ 风格的字符串, 参数是一个传出参数
bool SerializeToString(std::string* output) const;
// 将类对象中的数据序列化为字符串, c 风格的字符串, 参数 data 是一个传出参数
bool SerializeToArray(void* data, int size) const;

// ------ 写磁盘文件, 只需要调用这个函数, 数据自动被写入到磁盘文件中
// -- 需要提供流对象/文件描述符关联一个磁盘文件
// 将数据序列化写入到磁盘文件中, c++ 风格
// ostream 子类 ofstream -> 写文件
bool SerializeToOstream(std::ostream* output) const;
// 将数据序列化写入到磁盘文件中, c 风格
bool SerializeToFileDescriptor(int file_descriptor) const;
```

### 反序列化
反序列化是指将序列化后的二进制数据重新转换为原始的数据结构或对象的过程。通过反序列化，我们可以将之前序列化的数据重新还原为其原始的形式，以便进行数据的读取、操作和处理。

`Protobuf`中为我们提供了相关的用于数据序列化的API，如下所示：

```c++
// 头文件目录: google\protobuf\message_lite.h
bool ParseFromString(const std::string& data) ;
bool ParseFromArray(const void* data, int size);
// istream -> 子类 ifstream -> 读操作
// wo ri
// w->写 o: ofstream , r->读 i: ifstream
bool ParseFromIstream(std::istream* input);
bool ParseFromFileDescriptor(int file_descriptor);
```


# 内存泄漏检测 mtrace 内存追踪、valgrind 工具

[内存检查：mtrace 内存追踪、valgrind 工具\_valgrind, mtrace-CSDN博客](https://blog.csdn.net/weixin_41565755/article/details/102785710)



还可以用sanitizers [C++工程实践必备技能 - 掘金](https://juejin.cn/post/7194435171633299513)



其他工具参考 [C++工程实践必备技能\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1qA411r7ia/?spm_id_from=333.999.0.0&vd_source=3c8bacbe673849ddb69e7ada3ae5944f)

