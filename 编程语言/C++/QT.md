# Qt基础

## Qt入门

### qt概述

#### 什么是Qt

> 不论我们学习什么样的知识点首先第一步都需要搞明白它是什么，这样才能明确当前学习的方向是否正确，下面给大家介绍一下什么是Qt。

- 是一个跨平台的C++应用程序开发框架
	- 具有短平快的优秀特质: 投资少、周期短、见效快、效益高
	- 几乎支持所有的平台, 可用于桌面程序开发以及嵌入式开发
	- 有属于自己的事件处理机制
	- 可以搞效率的开发基于窗口的应用程序。
- Qt是标准 C++ 的扩展, C++的语法在Qt中都是支持的
	- 良好封装机制使得Qt的模块化程度非常高，可重用性较好，可以快速上手。
	- Qt 提供了一种称为signals/slots的安全类型来替代callback（回调函数），这使得各个元件之间的协同工作变得十分简单。


##### Qt的特点

> 知道了Qt是什么之后，给大家介绍一下Qt这个框架的一些优点，就是因为具有了这些优秀的特质才使得现在很多企业都首选Qt进行基于窗口的应用程序开发，并且近年来市场对Qt程序猿的需求也在不断攀升。


- 广泛用于开发GUI程序，也可用于开发非GUI程序。
	- <font color=#ff0000>GUI = Graphical User Interface</font>
	- 也就是基于窗口的应用程序开发。
- 有丰富的 API
	- Qt 包括多达 250 个以上的 C++ 类
	- 可以处理正则表达式。
- 支持 2D/3D 图形渲染，支持 OpenGL
- Qt给程序猿提供了非常详细的官方文档
- 支持XML，Json
- 框架底层模块化， 使用者可以根据需求选择相应的模块来使用
- 可以轻松跨平台
	- 和Java的跨平台方式不同
	- 在不同的平台使用的是相同的上层接口，但是在底层封装了不同平台对应的API（暗度陈仓）。


##### Qt中的模块

> Qt类库里大量的类根据功能分为各种模块，这些模块又分为以下几大类：

- Qt 基本模块（Qt Essentials)：提供了Qt在所有平台上的基本功能。
- Qt 附加模块（Qt Add-Ons)：实现一些特定功能的提供附加价值的模块。
- 增值模块（Value-AddModules)：单独发布的提供额外价值的模块或工具。
- 技术预览模块（Technology Preview Modules）：一些处于开发阶段，但是可以作为技术预览使用的模块。
- Qt 工具（Qt Tools)：帮助应用程序开发的一些工具。

> Qt官网或者帮助文档的“All Modules”页面可以查看所有这些模块的信息。以下是官方对Qt基本模块的描述。关于其他模块感兴趣的话可以自行查阅。
> 于其他模块感兴趣的话可以自行查阅。

|          模块           |               描述               |
| :-------------------: | :----------------------------: |
|        Qt Core        |     Qt 类库的核心，所有其他模块都依赖于此模块     |
|        Qt GUI         |    设计 GUI 界面的基础类，包括 OpenGL     |
|     Qt Multimedia     |        音频、视频、摄像头和广播功能的类        |
| Qt Multimedia Widgets |         实现多媒体功能的界面组件类          |
|      Qt Network       |         使网络编程更简单和轻便的类          |
|        Qt QML         |    用于 QML 和 JavaScript语言的类     |
|       Qt Quick        |    用于构建具有定制用户界面的动态应用程序的声明框架    |
|   Qt Quick Controls   | 创建桌面样式用户界面，基于 Qt Quick 的用户界面控件 |
|   Qt Quick Dialogs    |      用于 Qt Quick 的系统对话框类型      |
|   Qt Quick Layouts    |     用于 Qt Quick 2 界面元素的布局项     |
|        Qt SQL         |        使用 SQL 用于数据库操作的类        |
|        Qt Test        |        用于应用程序和库进行单元测试的类        |
|      Qt Widgets       |     用于构建 GUI 界面的 C++ 图形组件类     |
### 第一个Qt项目

main.cpp

在这个源文件中有程序的入口函数 main()，下面给大家介绍下这个文件中自动生成的几行代码

```c++
#include "mainwindow.h"		// 生成的窗口类头文件
#include <QApplication>		// 应用程序类头文件

int main(int argc, char *argv[])
{
    // 创建应用程序对象, 在一个Qt项目中实例对象有且仅有一个
    // 类的作用: 检测触发的事件, 进行事件循环并处理
    QApplication a(argc, argv);
    // 创建窗口类对象
    MainWindow w;
    // 显示窗口
    w.show();
    // 应用程序对象开始事件循环, 保证应用程序不退出
    return a.exec();
}
```

 mainwindow.h

这个文件是窗口界面对应的类的头文件。

```cpp
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>		// Qt标准窗口类头文件

QT_BEGIN_NAMESPACE
// mainwindow.ui 文件中也有一个类叫 MainWindow, 将这个类放到命名空间 Ui 中
namespace Ui { class MainWindow; }	
QT_END_NAMESPACE

class MainWindow : public QMainWindow
{
    Q_OBJECT	// 这个宏是为了能够使用Qt中的信号槽机制

public:
    MainWindow(QWidget *parent = nullptr);
    ~MainWindow();

private:
    Ui::MainWindow *ui;		// 定义指针指向窗口的 UI 对象
};
#endif // MAINWINDOW_H
```

mainwindow.cpp

```cpp
#include "mainwindow.h"
#include "ui_mainwindow.h"

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)	// 基于mainwindow.ui创建一个实例对象
{
    // 将 mainwindow.ui 的实例对象和 当前类的对象进行关联
    // 这样同名的连个类对象就产生了关联, 合二为一了
    ui->setupUi(this);
}

MainWindow::~MainWindow()
{
    delete ui;
}
```


### Qt中的窗口类

> 我们在通过Qt向导窗口基于窗口的应用程序的项目过程中倒数第二步让我们选择跟随项目创建的第一个窗口的基类, 下拉菜单中有三个选项, 分别为: QMainWindow、QDialog、QWidget如下图：

#### 基础窗口类

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241223211427.png?" alt="image.png" style="zoom:60%;" />




- 常用的窗口类有3个
	- 在创建Qt窗口的时候, 需要让自己的窗口类继承上述三个窗口类的其中一个
- `QWidget`
	- 所有窗口类的基类
	- Qt中的控件(按钮, 输入框, 单选框…)也属于窗口, 基类都是`QWidget`
	- 可以内嵌到其他窗口中: 没有边框
	- 可以不内嵌单独显示: 独立的窗口, 有边框
- `QDialog`
	- 对话框类, 后边的章节会具体介绍这个窗口
	- 不能内嵌到其他窗口中
- `QMainWindow`
	- 有工具栏, 状态栏, 菜单栏, 后边的章节会具体介绍这个窗口
	- 不能内嵌到其他窗口中


#### 窗口的显示
- 内嵌窗口
	- 依附于某一个大的窗口, 作为了大窗口的一部分
	- 大窗口就是这个内嵌窗口的父窗口
	- 父窗口显示的时候, 内嵌的窗口也就被显示出来了
	
- 不内嵌窗口
	- 这类窗口有边框, 有标题栏
	- 需要调用函数才可以显示

```cpp
// QWidget是所有窗口类的基类, 调用这个提供的 show() 方法就可以显示将任何窗口显示出来
// 非模态显示
void QWidget::show();	// 显示当前窗口和它的子窗口

// 对话框窗口的非模态显示: 还是调用show() 方法
// 对话框窗口的模态显示
[virtual slot] int QDialog::exec();
```


### 坐标体系

> 在Qt关于窗口的显示是需要指定位置的，这个位置是通过坐标来确定的，所有坐标的选取又都是基于坐标原点来确定的，关于这些细节的确定，下面依次给大家进行讲解。

#### 窗口的坐标原点

所有坐标的确定都需要先找到坐标原点, <font color=#ff0000>Qt的坐标原点在窗口的左上角</font>

- x轴向右递增
- y轴向下递增

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241223212520.png" alt="image.png" style="zoom:60%;" />

#### 窗口的相对坐标

> 在一个Qt窗口中一般都有很多子窗口内嵌到这个父窗口中，其中每个窗口都有自己的坐标原点，子窗口的位置也就是其使用的坐标点就是它的父窗口坐标体系中的坐标点。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241223212610.png" alt="image.png" style="zoom:60%;" />

- 在Qt的某一个窗口中有可能有若干个控件, 这个控件都是嵌套的关系
	- A窗口包含B窗口, B窗口包含C窗口
- 每个窗口都有坐标原点, 在左上角
	- 子窗口的位置是基于父窗口的坐标体系来确定的, 也就是说通过父窗口左上角的坐标点来确定自己的位置
- Qt中窗口显示的时候使用的相对坐标, 相对于自己的父窗口
- 将子窗口移动到父窗口的某个位置

```c++
// 所有窗口类的基类: QWidget
// QWidget中提供了移动窗口的 API函数
// 参数 x, y是要移动的窗口的左上角的点, 窗口的左上角移动到这个坐标点
void QWidget::move(int x, int y);
void QWidget::move(const QPoint &);
```

### 内存回收

在Qt中创建对象的时候会提供一个 <font color=#ff0000>Parent对象指针</font>（可以查看类的构造函数），下面来解释这个parent到底是干什么的。

QObject是以对象树的形式组织起来的。<font color=#ff0000>当你创建一个QObject对象时，会看到QObject的构造函数接收一个QObject指针作为参数，这个参数就是 parent，也就是父对象指针</font>。这相当于，在创建QObject对象时，可以提供一个其父对象，我们创建的这个QObject对象会自动添加到其父对象的children()列表。当父对象析构的时候，这个列表中的所有对象也会被析构。（注意，<font color=#ff0000>这里的父对象并不是继承意义上的父类！</font>）

QWidget是能够在屏幕上显示的一切组件的父类。QWidget继承自QObject，因此也继承了这种对象树关系。一个孩子自动地成为父组件的一个子组件。因此，它会显示在父组件的坐标系统中，被父组件的边界剪裁。例如，当用户关闭一个对话框的时候，应用程序将其删除，那么，我们希望属于这个对话框的按钮、图标等应该一起被删除。事实就是如此，因为这些都是对话框的子组件。

Qt 引入对象树的概念，在一定程度上解决了内存问题。
- 当一个QObject对象在堆上创建的时候，Qt 会同时为其创建一个对象树。不过，对象树中对象的顺序是没有定义的。这意味着，销毁这些对象的顺序也是未定义的。
- 任何对象树中的 QObject对象 delete 的时候，如果这个对象有 parent，则自动将其从 parent 的children()列表中删除；如果有孩子，则自动 delete 每一个孩子。Qt 保证没有QObject会被 delete 两次，这是由析构顺序决定的。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241223213809.png" alt="image.png" style="zoom:60%;" />

```cpp
#include <QObject>
#include <QDebug>

class MyObject : public QObject {
public:
    // 构造函数接收一个父对象指针
    MyObject(const QString& name, QObject* parent = nullptr) 
        : QObject(parent), m_name(name) {
        qDebug() << m_name << "created";
    }
    
    // 析构函数
    ~MyObject() {
        qDebug() << m_name << "destroyed";
    }

private:
    QString m_name;
};

int main() {
    // 创建父对象
    MyObject* parent = new MyObject("Parent");
    
    // 创建子对象，将parent作为父对象
    MyObject* child1 = new MyObject("Child1", parent);
    MyObject* child2 = new MyObject("Child2", parent);
    
    // 创建孙对象
    MyObject* grandChild = new MyObject("GrandChild", child1);
    
    // 查看parent的子对象列表
    qDebug() << "Parent's children:" << parent->children().size();
    
    // 删除parent对象
    delete parent;
    
    // 不需要手动删除child1、child2和grandChild
    // 它们会随着parent的删除自动析构
    
    return 0;
}
```


综上所述, 我们可以得到一个结论: <font color=#ff0000>Qt中有内存回收机制, 但是不是所有被new出的对象被自动回收, 满足条件才可以回收, 如果想要在Qt中实现内存的自动回收</font>, 需要满足以下两个条件:
- 创建的对象必须是QObject类的子类(间接子类也可以)
	- QObject类是没有父类的, Qt中有很大一部分类都是从这个类派生出去的
		- Qt中使用频率很高的窗口类和控件都是 QObject 的直接或间接的子类
		- 其他的类可以自己查阅Qt帮助文档
- 创建出的类对象, 必须要指定其父对象是谁, 一般情况下有两种操作方式:

```cpp
// 方式1: 通过构造函数
// parent: 当前窗口的父对象, 找构造函数中的 parent 参数即可
QWidget::QWidget(QWidget *parent = Q_NULLPTR, Qt::WindowFlags f = Qt::WindowFlags());
QTimer::QTimer(QObject *parent = nullptr);

// 方式2: 通过setParent()方法
// 假设这个控件没有在构造的时候指定符对象, 可以调用QWidget的api指定父窗口对象
void QWidget::setParent(QWidget *parent);
void QObject::setParent(QObject *parent);
```

## Qt中的基础数据类型

### 1. 基础类型

因为Qt是一个C++ 框架, 因此C++中所有的语法和数据类型在Qt中都是被支持的, 但是Qt中也定义了一些属于自己的数据类型, 下边给大家介绍一下这些基础的数类型。

QT基本数据类型定义在`#include <QtGlobal>` 中，QT基本数据类型有：

| 类型名称 | 注释 | 备注 |
|:---:|:---:|:---:|
| `qint8` | `signed char` | 有符号8位数据 |
| `qint16` | `signed short` | 16位数据类型 |
| `qint32` | `signed short` | 32位有符号数据类型 |
| `qint64` | `long long int` 或 `__int64` | 64位有符号数据类型，Windows中定义为`__int64` |
| `qintptr` | `qint32` 或 `qint64` | 指针类型 根据系统类型不同而不同，32位系统为`qint32`、64位系统为`qint64` |
| `qlonglong` | `long long int` 或 `__int64` | Windows中定义为`__int64` |
| `qptrdiff` | `qint32` 或 `qint64` | 根据系统类型不同而不同，32位系统为`qint32`、64位系统为`qint64` |
| `qreal` | `double` 或 `float` | 除非配置了-qreal float选项，否则默认为`double` |
| `quint8` | `unsigned char` | 无符号8位数据类型 |
| `quint16` | `unsigned short` | 无符号16位数据类型 |
| `quint32` | `unsigned int` | 无符号32位数据类型 |
| `quint64` | `unsigned long long int` 或 `unsigned __int64` | 无符号64比特数据类型，Windows中定义为`unsigned __int64` |
| `quintptr` | `quint32` 或 `quint64` | 根据系统类型不同而不同，32位系统为`quint32`、64位系统为`quint64` |
| `qulonglong` | `unsigned long long int` 或 `unsigned __int64` | Windows中定义为`__int64` |
| `uchar` | `unsigned char` | 无符号字符类型 |
| `uint` | `unsigned int` | 无符号整型 |
| `ulong` | `unsigned long` | 无符号长整型 |
| `ushort` | `unsigned short` | 无符号短整型 |

> 虽然在Qt中有属于自己的整形或者浮点型, 但是在变成过程中这些一般不用, 常用的类型关键字还是 C/C++中的 int, float, double 等。

### log输出

#### 在调试窗口中输入日志

在Qt中进行log输出, 一般不使用c中的`printf`, 也不是使用C++中的`cout`, Qt框架提供了专门用于日志输出的类, 头文件名为 `QDebug`, 使用方法如下:

```c++
// 包含了QDebug头文件, 直接通过全局函数 qDebug() 就可以进行日志输出了
qDebug() << "Date:" << QDate::currentDate();
qDebug() << "Types:" << QString("String") << QChar('x') << QRect(0, 10, 50, 40);
qDebug() << "Custom coordinate type:" << coordinate;

// 和全局函数 qDebug() 类似的日志函数还有: qWarning(), qInfo(), qCritical()
int number = 666;
float i = 11.11;
qWarning() << "Number:" << number << "Other value:" << i;
qInfo() << "Number:" << number << "Other value:" << i;
qCritical() << "Number:" << number << "Other value:" << i;

qDebug() << "我是要成为海贼王的男人!!!";
qDebug() << "我是隔壁的二柱子...";
qDebug() << "我是鸣人, 我擅长嘴遁!!!";
```


### 字符串类型

在Qt中不仅支持C, C++中的字符串类型, 而且还在框架中定义了专属的字符串类型, 我们必须要掌握在Qt中关于这些类型的使用和相互之间的转换。

语言类型	字符串类型
C	char*
C++	std::string, char*
Qt	QByteArray, QString 等


| 语言类型 |         字符串类型         |
| :--: | :-------------------: |
|  C   |         char*         |
| C++  |  std::string, char*   |
|  Qt  | QByteArray, QString 等 |

#### QByteArray

在Qt中<font color=#ff0000>QByteArray</font>可以看做是c语言中 `char*`的升级版本。我们在使用这种类型的时候可通过这个类的构造函数申请一块动态内存，用于存储我们需要处理的字符串数据。

下面给大家介绍一下这个类中常用的一些API函数，<font color=#ff0000>大家要养成遇到问题主动查询帮助文档的好习惯</font>。

- 构造函数
```cpp
// 构造空对象, 里边没有数据
QByteArray::QByteArray();
// 将data中的size个字符进行构造, 得到一个字节数组对象
// 如果 size==-1 函数内部自动计算字符串长度, 计算方式为: strlen(data)
QByteArray::QByteArray(const char *data, int size = -1);
// 构造一个长度为size个字节, 并且每个字节值都为ch的字节数组
QByteArray::QByteArray(int size, char ch);
```

- 数据操作
```cpp
// 在尾部追加数据
// 其他重载的同名函数可参考Qt帮助文档, 此处略
QByteArray &QByteArray::append(const QByteArray &ba);
void QByteArray::push_back(const QByteArray &other);

// 头部添加数据
// 其他重载的同名函数可参考Qt帮助文档, 此处略
QByteArray &QByteArray::prepend(const QByteArray &ba);
void QByteArray::push_front(const QByteArray &other);

// 插入数据, 将ba插入到数组第 i 个字节的位置(从0开始)
// 其他重载的同名函数可参考Qt帮助文档, 此处略
QByteArray &QByteArray::insert(int i, const QByteArray &ba);

// 删除数据
// 从大字符串中删除len个字符, 从第pos个字符的位置开始删除
QByteArray &QByteArray::remove(int pos, int len);
// 从字符数组的尾部删除 n 个字节
void QByteArray::chop(int n);
// 从字节数组的 pos 位置将数组截断 (前边部分留下, 后边部分被删除)
void QByteArray::truncate(int pos);
// 将对象中的数据清空, 使其为null
void QByteArray::clear();

// 字符串替换
// 将字节数组中的 子字符串 before 替换为 after
// 其他重载的同名函数可参考Qt帮助文档, 此处略
QByteArray &QByteArray::replace(const QByteArray &before, const QByteArray &after);
```

- 子字符串查找和判断
```cpp
// 判断字节数组中是否包含子字符串 ba, 包含返回true, 否则返回false
bool QByteArray::contains(const QByteArray &ba) const;
bool QByteArray::contains(const char *ba) const;
// 判断字节数组中是否包含子字符 ch, 包含返回true, 否则返回false
bool QByteArray::contains(char ch) const;

// 判断字节数组是否以字符串 ba 开始, 是返回true, 不是返回false
bool QByteArray::startsWith(const QByteArray &ba) const;
bool QByteArray::startsWith(const char *ba) const;
// 判断字节数组是否以字符 ch 开始, 是返回true, 不是返回false
bool QByteArray::startsWith(char ch) const;

// 判断字节数组是否以字符串 ba 结尾, 是返回true, 不是返回false
bool QByteArray::endsWith(const QByteArray &ba) const;
bool QByteArray::endsWith(const char *ba) const;
// 判断字节数组是否以字符 ch 结尾, 是返回true, 不是返回false
bool QByteArray::endsWith(char ch) const;
```

- 遍历
```cpp
// 使用迭代器
iterator QByteArray::begin();
iterator QByteArray::end();

// 使用数组的方式进行遍历
// i的取值范围 0 <= i < size()
char QByteArray::at(int i) const;
char QByteArray::operator[](int i) const;
```

- 查看字节数
```cpp
// 返回字节数组对象中字符的个数
int QByteArray::length() const;
int QByteArray::size() const;
int QByteArray::count() const;

// 返回字节数组对象中 子字符串ba 出现的次数
int QByteArray::count(const QByteArray &ba) const;
int QByteArray::count(const char *ba) const;
// 返回字节数组对象中 字符串ch 出现的次数
int QByteArray::count(char ch) const;
```

- 类型转换
```cpp
// 将QByteArray类型的字符串 转换为 char* 类型
char *QByteArray::data();
const char *QByteArray::data() const;

// int, short, long, float, double -> QByteArray
// 其他重载的同名函数可参考Qt帮助文档, 此处略
QByteArray &QByteArray::setNum(int n, int base = 10);
QByteArray &QByteArray::setNum(short n, int base = 10);
QByteArray &QByteArray::setNum(qlonglong n, int base = 10);
QByteArray &QByteArray::setNum(float n, char f = 'g', int prec = 6);
QByteArray &QByteArray::setNum(double n, char f = 'g', int prec = 6);
[static] QByteArray QByteArray::number(int n, int base = 10);
[static] QByteArray QByteArray::number(qlonglong n, int base = 10);
[static] QByteArray QByteArray::number(double n, char f = 'g', int prec = 6);

// QByteArray -> int, short, long, float, double
int QByteArray::toInt(bool *ok = Q_NULLPTR, int base = 10) const;
short QByteArray::toShort(bool *ok = Q_NULLPTR, int base = 10) const;
long QByteArray::toLong(bool *ok = Q_NULLPTR, int base = 10) const;
float QByteArray::toFloat(bool *ok = Q_NULLPTR) const;
double QByteArray::toDouble(bool *ok = Q_NULLPTR) const;

// std::string -> QByteArray
[static] QByteArray QByteArray::fromStdString(const std::string &str);
// QByteArray -> std::string
std::string QByteArray::toStdString() const;

// 所有字符转换为大写
QByteArray QByteArray::toUpper() const;
// 所有字符转换为小写
QByteArray QByteArray::toLower() const;
```

#### QString
> QString也是封装了字符串, 但是内部的编码为<font color=#ff0000>utf8</font>, UTF-8属于Unicode字符集, <font color=#ff0000>它固定使用多个字节（window为2字节, linux为3字节）来表示一个字符</font>，这样可以将世界上几乎所有语言的常用字符收录其中。

下面给大家介绍一下这个类中常用的一些API函数。

- 构造函数
```cpp
// 构造一个空字符串对象
QString::QString();
// 将 char* 字符串 转换为 QString 类型
QString::QString(const char *str);
// 将 QByteArray 转换为 QString 类型
QString::QString(const QByteArray &ba);
// 其他重载的同名构造函数可参考Qt帮助文档, 此处略
```

- 数据操作
```cpp
// 尾部追加数据
// 其他重载的同名函数可参考Qt帮助文档, 此处略
QString &QString::append(const QString &str);
QString &QString::append(const char *str);
QString &QString::append(const QByteArray &ba);
void QString::push_back(const QString &other);

// 头部添加数据
// 其他重载的同名函数可参考Qt帮助文档, 此处略
QString &QString::prepend(const QString &str);
QString &QString::prepend(const char *str);
QString &QString::prepend(const QByteArray &ba);
void QString::push_front(const QString &other);

// 插入数据, 将 str 插入到字符串第 position 个字符的位置(从0开始)
// 其他重载的同名函数可参考Qt帮助文档, 此处略
QString &QString::insert(int position, const QString &str);
QString &QString::insert(int position, const char *str);
QString &QString::insert(int position, const QByteArray &str);

// 删除数据
// 从大字符串中删除len个字符, 从第pos个字符的位置开始删除
QString &QString::remove(int position, int n);

// 从字符串的尾部删除 n 个字符
void QString::chop(int n);
// 从字节串的 position 位置将字符串截断 (前边部分留下, 后边部分被删除)
void QString::truncate(int position);
// 将对象中的数据清空, 使其为null
void QString::clear();

// 字符串替换
// 将字节数组中的 子字符串 before 替换为 after
// 参数 cs 为是否区分大小写, 默认区分大小写
// 其他重载的同名函数可参考Qt帮助文档, 此处略
QString &QString::replace(const QString &before, const QString &after, Qt::CaseSensitivity cs = Qt::CaseSensitive);
```

- 子字符串查找和判断
```cpp
// 参数 cs 为是否区分大小写, 默认区分大小写
// 其他重载的同名函数可参考Qt帮助文档, 此处略

// 判断字符串中是否包含子字符串 str, 包含返回true, 否则返回false
bool QString::contains(const QString &str, Qt::CaseSensitivity cs = Qt::CaseSensitive) const;

// 判断字符串是否以字符串 ba 开始, 是返回true, 不是返回false
bool QString::startsWith(const QString &s, Qt::CaseSensitivity cs = Qt::CaseSensitive) const;

// 判断字符串是否以字符串 ba 结尾, 是返回true, 不是返回false
bool QString::endsWith(const QString &s, Qt::CaseSensitivity cs = Qt::CaseSensitive) const;
```

- 遍历
```cpp
// 使用迭代器
iterator QString::begin();
iterator QString::end();

// 使用数组的方式进行遍历
// i的取值范围 0 <= position < size()
const QChar QString::at(int position) const
const QChar QString::operator[](int position) const;
```

- 查看字节数
```cpp
// 返回字节数组对象中字符的个数 (字符个数和字节个数是不同的概念)
int QString::length() const;
int QString::size() const;
int QString::count() const;

// 返回字节串对象中 子字符串 str 出现的次数
// 参数 cs 为是否区分大小写, 默认区分大小写
int QString::count(const QStringRef &str, Qt::CaseSensitivity cs = Qt::CaseSensitive) const;
```

- 类型转换
```cpp
// 将int, short, long, float, double 转换为 QString 类型
// 其他重载的同名函数可参考Qt帮助文档, 此处略
QString &QString::setNum(int n, int base = 10);
QString &QString::setNum(short n, int base = 10);
QString &QString::setNum(long n, int base = 10);
QString &QString::setNum(float n, char format = 'g', int precision = 6);
QString &QString::setNum(double n, char format = 'g', int precision = 6);
[static] QString QString::number(long n, int base = 10);
[static] QString QString::number(int n, int base = 10);
[static] QString QString::number(double n, char format = 'g', int precision = 6);

// 将 QString 转换为 int, short, long, float, double 类型
int QString::toInt(bool *ok = Q_NULLPTR, int base = 10) const;
short QString::toShort(bool *ok = Q_NULLPTR, int base = 10) const;
long QString::toLong(bool *ok = Q_NULLPTR, int base = 10) const
float QString::toFloat(bool *ok = Q_NULLPTR) const;
double QString::toDouble(bool *ok = Q_NULLPTR) const;

// 将标准C++中的 std::string 类型 转换为 QString 类型
[static] QString QString::fromStdString(const std::string &str);
// 将 QString 转换为 标准C++中的 std::string 类型
std::string QString::toStdString() const;

// QString -> QByteArray
// 转换为本地编码, 跟随操作系统
QByteArray QString::toLocal8Bit() const;
// 转换为 Latin-1 编码的字符串 不支持中文
QByteArray QString::toLatin1() const;
// 转换为 utf8 编码格式的字符串 (常用)
QByteArray QString::toUtf8() const;

// 所有字符转换为大写
QString QString::toUpper() const;
// 所有字符转换为小写
QString QString::toLower() const;
```

- 字符串格式
```cpp
// 其他重载的同名函数可参考Qt帮助文档, 此处略
QString QString::arg(const QString &a, 
          int fieldWidth = 0, 
          QChar fillChar = QLatin1Char( ' ' )) const;
QString QString::arg(int a, int fieldWidth = 0, 
          int base = 10, 
          QChar fillChar = QLatin1Char( ' ' )) const;

// 示例程序
int i;                // 假设该变量表示当前文件的编号
int total;            // 假设该变量表示文件的总个数
QString fileName;     // 假设该变量表示当前文件的名字
// 使用以上三个变量拼接一个动态字符串
QString status = QString("Processing file %1 of %2: %3")
                  .arg(i).arg(total).arg(fileName);
```


