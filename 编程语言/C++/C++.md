### 
# C++初识
## 简单的c++程序

```c++
#include<iostream>
using namespace std;

int main(){	
	cout << "hello world" << endl;
	return EXIT_SUCCESS;
}
```

分析：
- `#include<iostream>`; 预编译指令,引入头文件iostream.
- `using namespace std`; 使用标准命名空间
- `cout << “hello world”<< endl`; 和printf功能一样,输出字符串”hello wrold”

**c++头文件为什么没有.h？**
- 在c语言中头文件使用扩展名.h,将其作为一种通过**名称标识文件类型**的简单方式。但是c++得用法改变了,c++头文件没有扩展名。但是有些c语言的头文件被转换为c++的头文件,这些文件被重新命名,丢掉了扩展名.h(使之成为c++风格头文件),并在文件名称前面加上前缀c(表明来自c语言)。例如c++版本的math.h为cmath.
- 由于C使用不同的扩展名来表示不同文件类型,因此用一些特殊的扩展名(如hpp或hxx)表示c++的头文件也是可以的,ANSI/IOS标准委员会也认为是可以的,但是关键问题是用哪个比较好,最后一致同意不适用任何扩展名。


|  头文件类型  |     约定     |     示例     |               说明               |
| :-----: | :--------: | :--------: | :----------------------------: |
| c++旧式风格 |   以.h结尾    | iostream.h |            c++程序可用             |
|  c旧式风格  |   以.h结尾    |   math.h   |           c/c++程序可用            |
| c++新式风格 |    无扩展名    |  iostream  |    c++程序可用,使用namespace std     |
|  转换后的c  | 加上前缀c,无扩展名 |   cmath    | c++程序可用,可使用非c特性,如namespace std |

**问题2：using namespace std 是什么?**
namespace是指<font color=#ff0000>标识符</font>的各种可见范围。命名空间用关键字namespace来定义。命名空间是C++的一种机制,用来把单个标识符下的大量有逻辑联系的程序实体组合到一起。此标识符作为此组群的名字。

问题3：cout、endl 是什么？
cout是c++中的标准输出流,endl是输出换行并刷新缓冲区。c语言中是[[C语言#更新缓冲区|fflush]]

**面向对象三大特性**
- 封装
	把客观事物封装成**抽象**的类,并且类可以把自己的数据和方法只让可信的类或者对象操作,对不可信的进行信息隐藏。
	类将成员变量和成员函数封装在类的内部,根据需要设置访问权限,通过成员函数管理内部状态。
- 继承
	继承所表达的是类之间相关的关系,这种关系使得对象可以继承另外一类对象的特征和能力。
	继承的作用：避免公用代码的重复开发,减少代码和数据冗余。
- 多态
	多态性可以简单地概括为“一个接口,多种方法”,字面意思为多种形态。程序在运行时才决定调用的函数,它是面向对象编程领域的核心概念。

# C的缺陷
## C++的语法

优点： 
1. `强大的抽象封装能力`：这让C++语言具备了强大的开发工程能力, 在封装的同时C++最大程度的保留了高性能； 
2. `高性能`：运行快,快并且占用资源少一直是C++语言的追求； 
3. `低功耗`：特别适合在各种微型的嵌入式设备中运行高效的程序； 

缺点：
1. 语法相对复杂,细节比较多,学习曲线比较陡；
2. 需要一些好的范式和规范,否则代码很难维护

## C语言字符语法的常见陷阱 
### 字符数组和字符串问题
1. C语言中常见的词法、语法问题 
```cpp
    // char c1='yes'; //这里会报错
    char c2="yes"; // 会警告,这里会出现截断,实际c2=s
    const char *slash="/";
    //const char *slash2='/'; // 报错 ,不能将char *转成char
    const char *slash3=&c2;
```
2. C语言的考虑 
- C语言是高级语言中的低级语言, 
- 优点是：小巧,高效,接近底层 
- 缺点是：细节和陷阱比较多 
3. C++的思考 
兼容C语言,同时推出既高效又易于大规模开发的机制,string类的使用 

### 退化指针问题

```c
void test(int buf[6]) {
  int l = sizeof(buf);
  cout << l << endl; // 8
}
int main(int argc, char **argv) {

  int buf[] = {1, 2, 3, 4, 5, 6};

  int l = sizeof(buf);
  cout << l deque<< endl; // 24
  test(buf); 
}
```

1. **数组名是数组的首元素地址**: 在大多数上下文中,这是正确的。但在`sizeof`操作符的上下文中不是这样的。当你使用`sizeof`操作符时,它会返回数组的总大小,而不是指向数组第一个元素的指针的大小。
2. 当数组作为函数参数传递时,它会退化为指向其第一个元素的指针
3. sizeof操作符的行为取决于它的操作数。对数组使用sizeof将返回整个数组的大小,而对指针使用sizeof将返回指针的大小,即使这个指针指向一个数组。

### 移位运算问题

这里是逻辑右移还是算术右移
```c
int main(int argc, char **argv) {

	char a4=0x95;
	a4=(a4>>4);
	printf("0x%x\n",a4); //0xfffffff9 算术右移
	
}
```

c语言不同的操作系统 可能执行不同的移位逻辑,这里一缺点

1. 问题表现 
	逻辑右移还是算术右移： 好的方案：1)右移只对无符号数 
	移位操作位数的限制            2)移位数大于0,小于位数； 
2. 问题原因 
	C在设计移位操作时需要考虑整数表示的上下文环境 
3. C++中的解决方案 
`bitset`的使用 


# C++对C的扩展

## ::作用域运算符
通常情况下,如果有两个同名变量,一个是全局变量,另一个是局部变量,那么局部变量在其作用域内具有较高的优先权,它将屏蔽全局变量。
```c
//全局变量
int a = 10;
void test(){
	//局部变量
	int a = 20;
	//全局a被隐藏
	cout << "a:" << a << endl;
}
```

程序的输出结果是a:20。在test函数的输出语句中,使用的变量a是test函数内定义的局部变量,因此输出的结果为局部变量a的值。
作用域运算符可以用来解决局部变量与全局变量的重名问题 
```c
//全局变量
int a = 10;
//1. 局部变量和全局变量同名
void test(){
	int a = 20;
	//打印局部变量a
	cout << "局部变量a:" << a << endl;
	//打印全局变量a
	cout << "全局变量a:" << ::a << endl;
}
```
这个例子可以看出,作用域运算符可以用来解决局部变量与全局变量的重名问题,即在局部变量的作用域内,可用`::`对被屏蔽的同名的全局变量进行访问。

## 名字控制

创建名字是程序设计过程中一项最基本的活动,c++允许我们对名字的产生和名字的可见性进行控制。

### C++命名空间(namespace)

在c++中,名称（name）可以是符号常量、变量、函数、结构、枚举、类和对象等等。工程越大,名称互相冲突性的可能性越大。另外使用多个厂商的类库时,也可能导致名称冲突。为了避免,在大规模程序的设计中,以及在程序员使用各种各样的C++库时,这些标识符的命名发生冲突,标准C++引入关键字`namespace`（命名空间/名字空间/名称空间）,可以更好地控制标识符的**作用域**。
- 命名空间的用法:
```c++
#include <iostream>

using namespace std;

namespace A {
	int a=10;
	namespace B {
	int b=20;
	}
	void func(); // 声明和实现可分离
}
//  命名空间可嵌套命名空间
void A::func(){
	cout<<"你好"<<endl;
}
// 命名空间是开放的,即可以随时把新的成员加入已有的命名空间中
namespace A{
	void func(){
		cout << "hello namespace!" << endl;
	}
}
int a=10;
// 命名空间的用法
void test2()
{
	cout<<"A中a="<<A::a<<" "<<A::B::b<<" "<<endl;
	A::func();
}
int b=4;
// 无名命名空间,意味着命名空间中的标识符只能在本文件内访问,相当于给这个标识符加上了static,使得其可以作为内部连接
namespace  {
	int b=0;
	cout<<"b="<<::b<<endl; // 这里的b是全局作用域的b
}
int main(int argc, char** argv)
{
	namespace A = myA; // 命名空间别名
    test2();
}
```

### using声明
- using声明可使得<font color=#ff0000>指定的标识符可用</font>。
	```c++
	namespace A{
		int paramA = 20;
		int paramB = 30;
		void funcA(){ cout << "hello funcA" << endl; }
		void funcB(){ cout << "hello funcA" << endl; }
	}
	
	void test(){
		//1. 通过命名空间域运算符
		cout << A::paramA << endl;
		A::funcA();
		//2. using声明
		using A::paramA;
		using A::funcA;
		cout << paramA << endl;
		//cout << paramB << endl; //不可直接访问
		funcA();
		//3. 同名冲突
		//int paramA = 20; //相同作用域注意同名冲突
	}
	```

- using声明碰到函数重载
	```c++
	namespace A{
		void func(){}
		void func(int x){}
		int  func(int x,int y){}
	}
	void test(){
		using A::func;
		func();
		func(10);
		func(10, 20);
	}
	```
如果命名空间包含一组用相同名字重载的函数,using声明就声明了这个重载函数的所有集合。
### using编译指令
using编译指令使整个命名空间标识符可用.
```c++
namespace A{
	int paramA = 20;
	int paramB = 30;
	void funcA(){ cout << "hello funcA" << endl; }
	void funcB(){ cout << "hello funcB" << endl; }
}

void test01(){
	using namespace A;
	cout << paramA << endl;
	cout << paramB << endl;
	funcA();
	funcB();

	//不会产生二义性
	int paramA = 30;
	cout << paramA << endl;
}


namespace B{
	int paramA = 20;
	int paramB = 30;
	void funcA(){ cout << "hello funcA" << endl; }
	void funcB(){ cout << "hello funcB" << endl; }
}

void test02(){
	using namespace A;
	using namespace B;
	//二义性产生,不知道调用A还是B的paramA
	//cout << paramA << endl;
}
```

<font color=#ff0000>注意：使用using声明或using编译指令会增加命名冲突的可能性。也就是说,如果有名称空间,并在代码中使用作用域解析运算符,则不会出现二义性。</font>

### struct类型加强
- c中定义结构体变量需要加上struct关键字,c++不需要。
- c中的结构体只能定义成员变量,不能定义成员函数。c++即可以定义成员变量,也可以定义成员函数。

```c++
//1. 结构体中即可以定义成员变量,也可以定义成员函数
struct Student{
	string mName;
	int mAge;
	void setName(string name){ mName = name; }
	void setAge(int age){ mAge = age; }
	void showStudent(){
		cout << "Name:" << mName << " Age:" << mAge << endl;
	}
};

//2. c++中定义结构体变量不需要加struct关键字
void test01(){
	Student student;
	student.setName("John");
	student.setAge(20);
	student.showStudent();
}
```

#cpp一些概念


> [!tip] [左值和右值概念]
> - 在c++中可以放在赋值操作符左边的是左值,可以放到赋值操作符右面的是右值。           
> - 有些变量即可以当左值,也可以当右值。            
> - 左值为Lvalue,L代表Location,表示<font color=#ff0000>内存可以寻址</font>,可以赋值。             
> - 右值为Rvalue,R代表Read,就是可以知道它的值。             
> - 比如:int temp = 10; temp在内存中有地址,10没有,但是可以Read到它的值。            

但实际的标准更复杂

<img src="https://article.biliimg.com/bfs/article/edc936dfb6819b60046f13571624e06938716159.png" alt="image.png" style="zoom:60%;" />

- 一个 lvalue 是通常可以放在等号左边的表达式,左值
- 一个 rvalue 是通常只能放在等号右边的表达式,右值
- 一个 glvalue 是 generalized lvalue,广义左值
- 一个 xvalue 是 expiring lvalue,将亡值
- 一个 prvalue 是 pure rvalue,纯右值

## 引用(reference)
### 引用基本用法
c++增加了另外一种给<font color=#ff0000>函数传递地址</font>的途径,这就是按引用传递(pass-by-reference),它也存在于其他一些编程语言中,并不是c++的发明。

- 变量名实质上是**一段连续内存空间**的别名,是一个标号(门牌号)
- 程序中通过变量来申请并命名内存空间
- 通过变量的名字可以使用存储空间

> 对一段连续的内存空间只能取一个别名吗？
> c++中新增了引用的概念,引用可以作为一个已定义变量的别名。

基本语法: 
`Type& ref = val;`

> [!note] 注意事项：
> 1. `&`在此不是求地址运算,而是起**标识**作用。
> 2. 类型标识符是指目标变量的类型
> 3. <font color=#ff0000>必须在声明引用变量时进行初始化</font>。
> 4. 引用初始化之后不能改变。
> 5. 不能有NULL引用。必须确保引用是和一块合法的存储单元关联。
> 6. 可以建立对数组的引用。


```c++
//1. 认识引用
void test01(){

	int a = 10;
	//给变量a取一个别名b
	int& b = a;
	cout << "a:" << a << endl;
	cout << "b:" << b << endl;
	cout << "------------" << endl;
	//操作b就相当于操作a本身
	b = 100;
	cout << "a:" << a << endl;
	cout << "b:" << b << endl;
	cout << "------------" << endl;
	//一个变量可以有n个别名
	int& c = a;
	c = 200;
	cout << "a:" << a << endl;
	cout << "b:" << b << endl;
	cout << "c:" << c << endl;
	cout << "------------" << endl;
	//a,b,c的地址都是相同的
	cout << "a:" << &a << endl;
	cout << "b:" << &b << endl;
	cout << "c:" << &c << endl;
	
}
//2. 使用引用注意事项
void test02(){
	//1) 引用必须初始化
	//int& ref; //报错:必须初始化引用
	//2) 引用一旦初始化,不能改变引用
	int a = 10;
	int b = 20;
	int& ref = a;
	//3) 不能对数组建立引用
	int arr[10];
	//int& ref3[10] = arr;
}
```


```c++
	//1. 建立数组引用方法一
	typedef int ArrRef[10];
	int arr[10];
	ArrRef& aRef = arr;
	for (int i = 0; i < 10;i ++){
		aRef[i] = i+1;
	}
	for (int i = 0; i < 10;i++){
		cout << arr[i] << " ";
	}
	cout << endl;
	//2. 建立数组引用方法二
	int(&f)[10] = arr;
	for (int i = 0; i < 10; i++){
		f[i] = i+10;
	}
	for (int i = 0; i < 10; i++){
		cout << arr[i] << " ";
	}
	cout << endl;
```

### 函数中的引用
最常见看见引用的地方是在函数参数和返回值中。当引用被用作函数参数的时,在函数内对任何引用的修改,将对还函数外的参数产生改变。当然,可以通过传递一个指针来做相同的事情,但引用具有更清晰的语法。
如果从函数中返回一个引用,必须像从函数中返回一个指针一样对待。当函数返回值时,引用关联的内存一定要存在。
```c++
//值传递
void ValueSwap(int m,int n){
	int temp = m;
	m = n;
	n = temp;
}
//地址传递
void PointerSwap(int* m,int* n){
	int temp = *m;
	*m = *n;
	*n = temp;
}
//引用传递
void ReferenceSwap(int& m,int& n){
	int temp = m;
	m = n;
	n = temp;
}
void test(){
	int a = 10;
	int b = 20;
	//值传递
	ValueSwap(a, b);
	cout << "a:" << a << " b:" << b << endl;
	//地址传递
	PointerSwap(&a, &b);
	cout << "a:" << a << " b:" << b << endl;
	//引用传递
	ReferenceSwap(a, b);
	cout << "a:" << a << " b:" << b << endl;
}
```

通过引用参数产生的效果同按地址传递是一样的。引用的语法更清楚简单：	
1. 函数调用时传递的实参不必加`“&”`符 
2. 在被调函数中不必在参数前加`“*”`符
引用作为其它变量的别名而存在,因此在一些场合可以代替指针。C++主张用引用传递取代地址传递的方式,因为引用语法容易且不易出错。

```c++
//返回局部变量引用
int& TestFun01(){
	int a = 10; //局部变量
	return a;
}
//返回静态变量引用
int& TestFunc02(){	
	static int a = 20;
	cout << "static int a : " << a << endl;
	return a;
}
int main(){
	//不能返回局部变量的引用
	int& ret01 = TestFun01();
	//如果函数做左值,那么必须返回引用
	TestFunc02();
	TestFunc02() = 100;
	TestFunc02();

	return EXIT_SUCCESS;
}
```

- 不能返回局部变量的引用。
- 函数当左值,必须返回引用。


### 引用的本质
引用的本质在c++内部实现是一个指针常量.
`Type& ref = val; // Type* const ref = &val;`

c++编译器在编译过程中使用常指针作为引用的内部实现,因此引用所占用的空间大小与指针相同,只是这个过程是编译器内部实现,用户不可见。

```c++
//发现是引用,转换为 int* const ref = &a;
void testFunc(int& ref){
	ref = 100; // ref是引用,转换为*ref = 100
}
int main(){
	int a = 10;
	int& aRef = a; //自动转换为 int* const aRef = &a;这也能说明引用为什么必须初始化
	aRef = 20; //内部发现aRef是引用,自动帮我们转换为: *aRef = 20;
	cout << "a:" << a << endl;
	cout << "aRef:" << aRef << endl;
	testFunc(a);
	return EXIT_SUCCESS;
}
```

### 指针引用
在c语言中如果想改变一个指针的指向而不是它所指向的内容,函数声明可能这样:
`void fun(int**);`
给指针变量取一个别名。
```c++
Type* pointer = NULL;  
Type*& = pointer;
```

Type* pointer = NULL;  Type*& = pointer;
```c++
struct Teacher{
	int mAge;
};
//指针间接修改teacher的年龄
void AllocateAndInitByPointer(Teacher** teacher){
	*teacher = (Teacher*)malloc(sizeof(Teacher));
	(*teacher)->mAge = 200;  
}
//引用修改teacher年龄
void AllocateAndInitByReference(Teacher*& teacher){
	teacher->mAge = 300;
}
void test(){
	//创建Teacher
	Teacher* teacher = NULL;
	//指针间接赋值
	AllocateAndInitByPointer(&teacher);
	cout << "AllocateAndInitByPointer:" << teacher->mAge << endl;
	//引用赋值,将teacher本身传到ChangeAgeByReference函数中
	AllocateAndInitByReference(teacher);
	cout << "AllocateAndInitByReference:" << teacher->mAge << endl;
	free(teacher);
}
```

对于c++中的定义那个,语法清晰多了。函数参数变成指针的引用,用不着取得指针的地址。

### 常量引用
常量引用的定义格式:
`const Type& ref = val;`

常量引用注意：
- 字面量不能赋给引用,但是可以赋给const引用
- const修饰的引用,不能修改。
```c++
void test01(){
	int a = 100;
	const int& aRef = a; //此时aRef就是a
	//aRef = 200; 不能通过aRef的值
	a = 100; //OK
	cout << "a:" << a << endl;
	cout << "aRef:" << aRef << endl;
}
void test02(){
	//不能把一个字面量赋给引用
	//int& ref = 100;
	//但是可以把一个字面量赋给常引用
	const int& ref = 100; //int temp = 200; const int& ret = temp;
}
```


> [!example] const引用使用场景
> 常量引用主要用在**函数的形参**,尤其是类的拷贝/复制构造函数。
> 将函数的形参定义为常量引用的好处:
> - 引用不产生新的变量,减少形参与实参传递时的开销。
> - 由于引用可能导致实参随形参改变而改变,将其定义为常量引用可以消除这种副作用。
>     如果希望实参随着形参的改变而改变,那么使用一般的引用,如果不希望实参随着形参改变,那么使用常引用。



```c++
//const int& param防止函数中意外修改数据
void ShowVal(const int& param){
	cout << "param:" << param << endl;
}
```

## 内联函数(inline function)
### 内联函数的引出

在c中我们经常把一些短并且执行频繁的计算写成宏,而不是函数,这样做的理由是为了执行效率,宏可以避免函数调用的开销,这些都由预处理来完成。
但是在c++出现之后,使用预处理宏会出现两个问题：
- 第一个在c中也会出现,宏看起来像一个函数调用,但是会有隐藏一些难以发现的错误。
- 第二个问题是c++特有的,预处理器不允许访问类的成员,<font color=#ff0000>也就是说预处理器宏不能用作类类的成员函数</font>。

为了保持**预处理宏的效率又增加安全性**,而且还能像一般成员函数那样可以在类里访问自如,c++引入了**内联函数**(inline function).

内联函数为了继承宏函数的效率,没有函数调用时开销,然后又可以像普通函数那样,可以进行参数,返回值类型的安全检查,又可以作为成员函数。
### 预处理宏的缺陷
预处理器宏存在问题的关键是我们可能认为预处理器的行为和编译器的行为是一样的。当然也是由于宏函数调用和函数调用在外表看起来是一样的,因为也容易被混淆。但是其中也会有一些微妙的问题出现:
问题一：
```c++
#define ADD(x,y) x+y
inline int Add(int x,int y){
	return x + y;
}
void test(){
	int ret1 = ADD(10, 20) * 10; //希望的结果是300
	int ret2 = Add(10, 20) * 10; //希望结果也是300
	cout << "ret1:" << ret1 << endl; //210
	cout << "ret2:" << ret2 << endl; //300
}
```
问题二：
```c++
#define COMPARE(x,y) ((x) < (y) ? (x) : (y))
int Compare(int x,int y){
	return x < y ? x : y;
}
void test02(){
	int a = 1;
	int b = 3;
	//cout << "COMPARE(++a, b):" << COMPARE(++a, b) << endl; // 3
	cout << "Compare(int x,int y):" << Compare(++a, b) << endl; //2
}
```
问题三:
预定义宏函数**没有作用域概念**,无法作为一个类的成员函数,也就是说预定义宏没有办法表示类的范围。
### 内联函数
#### 内联函数基本概念
在c++中,预定义宏的概念是用内联函数来实现的,而内联函数本身也是一个真正的函数。内联函数具有普通函数的所有行为。唯一不同之处在于内联函数会<font color=#ff0000>在适当的地方像预定义宏一样展开</font>,所以不需要函数调用的开销。因此应该不使用宏,使用内联函数。
- 在普通函数(非成员函数)函数前面加上inline关键字使之成为内联函数。但是必须注意必须函数体和声明结合在一起,否则编译器将它作为普通函数来对待。
`inline void func(int a);`
以上写法没有任何效果,仅仅是声明函数,应该如下方式来做:
`inline int func(int a){return ++;}`
注意: 编译器将会检查函数参数列表使用是否正确,并返回值(进行必要的转换)。这些是预处理器无法完成的。
内联函数的确占用空间,但是内联函数相对于普通函数的优势只是省去了函数调用时候的**压栈,跳转,返回的开销**。我们可以理解为内联函数是<font color=#ff0000>以空间换时间</font>。
#### 类内部的内联函数
为了定义内联函数,通常必须在函数定义前面放一个inline关键字。但是在类内部定义内联函数时并不是必须的。任何在类内部定义的函数自动成为内联函数。
```c++
class Person{
public:
	Person(){ cout << "构造函数!" << endl; }
	void PrintPerson(){ cout << "输出Person!" << endl; }
}
```

构造函数Person,成员函数PrintPerson在类的内部定义,自动成为内联函数。
#### 内联函数和编译器
内联函数并不是何时何地都有效,为了理解内联函数何时有效,应该要知道编译器碰到内联函数会怎么处理？
对于任何类型的函数,编译器会将函数类型(包括函数名字,参数类型,返回值类型)放入到符号表中。同样,当编译器看到内联函数,并且对内联函数体进行分析没有发现错误时,也会将内联函数放入符号表。
当调用一个内联函数的时候,编译器首先确保传入参数类型是正确匹配的,或者如果类型不正完全匹配,但是可以将其转换为正确类型,并且返回值在目标表达式里匹配正确类型,或者可以转换为目标类型,内联函数就会直接替换函数调用,这就消除了函数调用的开销。假如内联函数是成员函数,对象this指针也会被放入合适位置。
类型检查和类型转换、包括在合适位置放入对象this指针这些都是预处理器不能完成的。

但是c++内联编译会有一些限制,以下情况编译器可能考虑不会将函数进行内联编译：
- 不能存在任何形式的循环语句
- 不能存在过多的条件判断语句
- 函数体不能过于庞大
- 不能对函数进行取址操作
- inline在debug版本上不起作用

<font color=#ff0000>内联仅仅只是给编译器一个建议,编译器不一定会接受这种建议,如果你没有将函数声明为内联函数,那么编译器也可能将此函数做内联编译。一个好的编译器将会内联小的、简单的函数。</font>

## 函数的默认参数

c++在声明函数原型的时可为一个或者多个参数指定默认(缺省)的参数值,当函数调用的时候如果没有指定这个值,编译器会自动用默认值代替。
```c++
void TestFunc01(int a = 10, int b = 20){
	cout << "a + b  = " << a + b << endl;
}
//注意点:
//1. 形参b设置默认参数值,那么后面位置的形参c也需要设置默认参数
void TestFunc02(int a,int b = 10,int c = 10){}
//2. 如果函数声明和函数定义分开,函数声明设置了默认参数,函数定义不能再设置默认参数
void TestFunc03(int a = 0,int b = 0);
void TestFunc03(int a, int b){}

int main(){
	//1.如果没有传参数,那么使用默认参数
	TestFunc01();
	//2. 如果传一个参数,那么第二个参数使用默认参数
	TestFunc01(100);
	//3. 如果传入两个参数,那么两个参数都使用我们传入的参数
	TestFunc01(100, 200);

	return EXIT_SUCCESS;
}
```


> [!note] 注意点：
> - 函数的默认参数从左向右,如果一个参数设置了默认参数,那么这个参数之后的参数都必须设置默认参数。
> - 如果函数声明和函数定义分开写,函数声明和函数定义不能同时设置默认参数。

## 函数的占位参数
c++在声明函数时,可以设置占位参数。占位参数只有参数类型声明,而没有参数名声明。一般情况下,在函数体内部无法使用占位参数。
```c++
void TestFunc01(int a,int b,int){
	//函数内部无法使用占位参数
	cout << "a + b = " << a + b << endl;
}
//占位参数也可以设置默认值
void TestFunc02(int a, int b, int = 20){
	//函数内部依旧无法使用占位参数
	cout << "a + b = " << a + b << endl;
}
int main(){

	//错误调用,占位参数也是参数,必须传参数
	//TestFunc01(10,20); 
	//正确调用
	TestFunc01(10,20,30);
	//正确调用
	TestFunc02(10,20);
	//正确调用
	TestFunc02(10, 20, 30);

	return EXIT_SUCCESS;
}
```

什么时候用,在后面我们要讲的操作符重载的后置++要用到这个.

## 函数重载(overload)
### 函数重载概述


在传统c语言中,函数名必须是唯一的,程序中不允许出现同名的函数。在c++中是允许出现同名的函数,这种现象称为**函数重载**。C++代码产生函数符号的时候，**函数名+参数列表**类型组成的！ 
函数重载的目的就是为了方便的使用函数名。
### 函数重载
#### 函数重载基本语法
**实现函数重载的条件**：
- 同一个作用域
- 参数个数不同
- 参数类型不同
- 参数顺序不同

```c++
//1. 函数重载条件
namespace A{
	void MyFunc(){ cout << "无参数!" << endl; }
	void MyFunc(int a){ cout << "a: " << a << endl; }
	void MyFunc(string b){ cout << "b: " << b << endl; }
	void MyFunc(int a, string b){ cout << "a: " << a << " b:" << b << endl;}
    void MyFunc(string b, int a){cout << "a: " << a << " b:" << b << endl;}
    void MyFunc(...) {cout<<"谁都没有匹配到"} //可变参数模板 这是一种特殊的重载，其目的是在没有找到更具体匹配的情况下，提供一个默认的函数选项。
}
//2.返回值不作为函数重载依据
namespace B{
	void MyFunc(string b, int a){}
	//int MyFunc(string b, int a){} //无法重载仅按返回值区分的函数
}
```

<font color=#ff0000>注意</font>: 函数重载和默认参数一起使用,需要额外注意二义性问题的产生。
```c++
void MyFunc(string b){
	cout << "b: " << b << endl;
}
//函数重载碰上默认参数
void MyFunc(string b, int a = 10){
	cout << "a: " << a << " b:" << b << endl;
}
int main(){
	MyFunc("hello"); //这时,两个函数都能匹配调用,产生二义性
	return 0;
}
```



> [!note] 思考：
> 为什么函数返回值不作为重载条件呢？     
> - 当编译器能从上下文中确定唯一的函数的时,如`int ret = func()`,这个当然是没有问题的。然而,我们在编写程序过程中可以忽略他的返回值。那么这个时候,一个函数为
> - `void func(int x)`;另一个为`int func(int x)`; 当我们直接调用`func(10)`,这个时候编译器就不确定调用那个函数。所以在c++中禁止使用返回值作为重载的条件。


#### 函数重载实现原理
编译器为了实现函数重载,也是默认为我们做了一些幕后的工作,编译器用不同的参数类型来修饰不同的函数名,比如`void func()`; 编译器可能会将函数名修饰成_func,当编译器碰到void func(int x),编译器可能将函数名修饰为_func_int,当编译器碰到void func(int x,char c),编译器可能会将函数名修饰为_func_int_char我这里使用”可能”这个字眼是因为编译器如何修饰重载的函数名称并没有一个统一的标准,所以不同的编译器可能会产生不同的内部名。
```c++
void func(){}
void func(int x){}
void func(int x,char y){}
```
以上三个函数在linux下生成的编译之后的函数名为:
```c++
_Z4funcv //v 代表void,无参数
_Z4funci //i 代表参数为int类型
_Z4funcic //i 代表第一个参数为int类型,第二个参数为char类型
```

### extern “C”浅析
以下在Linux下测试:
c函数: void MyFunc(){} ,被编译成函数: `MyFunc`
c++函数: void MyFunc(){},被编译成函数: `_Z6Myfuncv`

通过这个测试,由于c++中需要支持函数重载,所以c和c++中对同一个函数经过编译后生成的函数名是不相同的,这就导致了一个问题,如果在c++中调用一个使用c语言编写模块中的某个函数,那么c++是根据c++的名称修饰方式来查找并**链接**这个函数,那么就会发生链接错误,以上例,c++中调用MyFunc函数,在链接阶段会去找Z6Myfuncv,结果是没有找到的,因为这个MyFunc函数是c语言编写的,生成的符号是MyFunc。
那么如果我想在c++调用c的函数怎么办？
extern "C"的主要作用就是为了<font color=#ff0000>实现c++代码能够调用其他c语言代码</font>。加上extern "C"后,这部分代码编译器**按c语言的方式进行编译和链接(也就是按照c的方式去生成符号)**,而不是按c++的方式。
`MyModule.h`
```c
#ifndef MYMODULE_H
#define MYMODULE_H

#include<stdio.h>

#if __cplusplus  // 判断当前编译环境是否为c++
extern "C"{
#endif

	void func1();
	int func2(int a,int b);

#if __cplusplus
}
#endif

#endif
```

`MyModule.c`
```c
#include"MyModule.h"

void func1(){
	printf("hello world!");
}
int func2(int a, int b){
	return a + b;
}
```

`TestExternC.cpp`
```c++
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
using namespace std;
#if 0
	#ifdef __cplusplus
	extern "C" {
		#if 0
			void func1();
			int func2(int a, int b);
		#else
			#include"MyModule.h"
		#endif
	}
	#endif
#else
	extern "C" void func1();
	extern "C" int func2(int a, int b);
#endif
int main(){
	func1();
	cout << func2(10, 20) << endl;
	return EXIT_SUCCESS;
}
```

## 资源管理方案--RAII 

> RAII是C++中一种**管理资源**、**避免资源泄漏**的惯用法,利用**内存(大多是栈内存)管理资源**的特点来实现。

### 什么是RAII

RAII是Resource Acquisition Is Initialization的缩写,即“资源获取即初始化”。它是C++语言的一种**管理资源、避免资源泄漏**的惯用法,在函数中由栈管理的临时对象,在函数结束时会自动析构,从而自动释放资源,因此,我们可以通过**构造函数获取**资源,通过**析构函数释放**资源。即：

```c++
Object() {
    // acquire resource in constructor
}

~Object() {
    // release resource in destructor
}
```

RAII总结如下：
- 将每一种资源封装在一个RAII类中：
	- 所有**资源**在构造函数中获取,例如：分配内存、打开文件、建立数据库连接等；如果无法完成则在构造函数中抛出异常；
	- 所有**资源**在析构函数中释放,例如：释放内存、关闭文件、销毁数据库连接等；不应该抛出任何异常。

- 通过RAII类实例获取资源：
	- 具有自动生命管理周期或临时对象生命周期
	- 其生命周期与第一种绑定。

### 为什么要使用RAII

在C++中,通过new运算符动态申请内存,例如：

```c++
Foo* ptr = new Foo(1);

// ...
delete ptr;
```

在这段代码中,new运算符在计算机内存的堆上申请了一块Foo类型的内存,然后将其地址赋值给存储在栈上的指针ptr。为了能够释放内存资源,我们需要使用完new运算符申请的内存后,手动调用delete运算符释放内存。

但是,情况并不总是如此简单。

```c++
Foo* ptr = new Foo(1);

f(ptr);  // -->① may throw exception
if(ptr->g()) {
    // ... --> ② may forget to delete ptr
    return;
}
// ...
delete ptr;
```

如上面这个例子,我们可能会遇到以下几种情况：

1. 忘记delete释放内存。比如释放原指针指向的内存前就改变了指针的指向。
2. **程序抛出异常后**导致无法delete。比如上面的①处,如果f函数抛出异常,没有机会运行delete,从而导致内存泄漏。
3. 需求变更后,修改了函数,新增了分支,提前返回,却没有delete；现实情况代码复杂的话可能没有这么显而易见。
4. 而通过RAII这样一种机制,我们可以使其自动释放内存。

### C++ STL中RAII的应用

#### 智能指针
智能指针是RAII的一种实现,它是一种模板类,用于管理动态分配的对象。智能指针的主要作用是自动释放内存,从而避免内存泄漏。C++11中提供了三种智能指针：unique_ptr、shared_ptr和weak_ptr。它们的详细原理将在之后的文章中介绍。这里我们以unique_ptr为例,它的构造函数如下：

```c++
template< class T, class Deleter = std::default_delete<T> > class unique_ptr;
```

unique_ptr的析构函数会自动释放内存,因此,我们可以通过unique_ptr来管理动态分配的内存,从而避免内存泄漏。例如：
```c++
std::unique_ptr<int> ptr = std::make_unique<int>(1); // release memory when ptr is out of scope
```

#### 互斥锁

在多线程编程中,`std::lock_guard, std::unique_lock, std::shared_lock`等也利用了RAII的原理,用于管理互斥锁。当这些类的等对象创建时,会自动获取互斥锁；当对象销毁时,会自动释放互斥锁。

`std::lock_guard`的构造函数如下：
```c++
template< class Mutex > class lock_guard;
```

`std::lock_guard`的析构函数会自动释放互斥锁,因此,我们可以通过`std::lock_guard`来管理互斥锁,从而避免忘记释放互斥锁。例如：

```c++
std::mutex mtx;
std::lock_guard<std::mutex> lock(mtx); // unlock when lock is out of scope
```

不使用RAII的情况下,我们需要手动释放互斥锁,如下所示：
```c++
std::mutex mtx;
mtx.lock();
// ...
mtx.unlock();
```

#### 文件操作

`std::ifstream, std::ofstream`等C++标准库的IO操作都是RAII的实现。
#### 事务处理
数据库事务处理中,如果在事务结束时没有提交或回滚,就会导致数据库连接一直被占用,从而导致数据库连接池耗尽。因此,我们需要在事务结束时自动提交或回滚,从而释放数据库连接。这一过程也可以通过RAII来实现。

### RAII的编程实践
基于RAII实现资源池的自动回收机制：


- `ResourcePool`为资源池类,可以创建指定数量的资源,并提供获取和释放资源的接口。
- `ResourceWrapper`为资源包装类,用于获取资源,并在对象销毁时自动释放资源。
- `Resource`为资源类,用于模拟资源,通过id来标识,其构造函数和析构函数分别用于获取和释放资源。

实现资源管理类需要注意的一些事项：

需要仔细考虑拷贝构造函数和拷贝赋值运算符的实现,若需拷贝,应考虑实现引用计数或对资源进行深拷贝；若无必要,最好将其删除。这里我们使用了=delete进行了删除；
需提供移动构造函数和移动赋值运算符,以便于使用`std::move()`,转移资源的控制权；
提供获取原始资源的接口。
代码实现如下：

```c++
constexpr int kErrorId = -1;
template<typename T>
class ResourcePool {
public:
    ResourcePool(int size) {
        for (int i = 0; i < size; ++i) {
            pool_.emplace_back(i);
        }
    }

    T getResource() {
        if (pool_.empty()) {
            std::cout << "Resource is not available now." << std::endl;
            return T();
        }
        T resource = std::move(pool_.front());
        pool_.pop_front();
        std::cout<< "Resource " << resource.ID() << " is acquired." << std::endl;
        return resource;
    }

    void releaseResource(T&& resource) {
        if (resource.ID() == kErrorId) {
          return;
        }
        std::cout << "Resource " << resource.ID() << " is released." << std::endl;
        pool_.emplace_back(std::forward<T>(resource));
    }

private:
    std::deque<T> pool_;
};

template<typename T>
class ResourceWrapper {
public:
    ResourceWrapper(ResourcePool<T>& pool) : pool_(pool), resource_(pool_.getResource()) {
      if(resource_.ID() == kErrorId) {
        throw std::runtime_error("Resource is not available now.");
      }
    }
    ~ResourceWrapper() {
        pool_.releaseResource(std::move(resource_));
    }
    ResourceWrapper(const ResourceWrapper& other) = delete;
    ResourceWrapper& operator=(const ResourceWrapper& other) = delete;

    ResourceWrapper(ResourceWrapper&& other) noexcept : pool_(other.pool_), resource_(std::move(other.resource_)) {
    }

    ResourceWrapper& operator=(ResourceWrapper&& other) noexcept {
      pool_ = other.pool_;
      resource_ = std::move(other.resource_);
      return *this;
    }
    const T& GetRawResource() const noexcept{
        return resource_;
    }
private:
    ResourcePool<T>& pool_;
    T resource_;
};

class Resource {
public:
    constexpr explicit Resource(int id) : id_(id) {
      std::cout << "Resource " << id_ << " is created." << std::endl;
    }
    Resource(): id_(kErrorId) {}
    ~Resource() = default;
    int ID() const {
        return id_;
    }
    // delete copy constructor and copy assignment operator
    Resource(const Resource& other) = delete;
    Resource& operator=(const Resource& other) = delete;

    Resource(Resource&& other) noexcept : id_(other.id_) {
      other.id_ = kErrorId;
    }

    Resource& operator=(Resource&& other) noexcept {
      id_ = other.id_;
      other.id_ = kErrorId;
      return *this;
    }
private:
    int id_;
};

constexpr int kPoolSize = 3;
ResourcePool<Resource> pool(kPoolSize); // Resource pool with 3 resources in global scope.

void RequestRourceTest() {
    constexpr int kResourcesNum = 3;
    for (int i = 0; i < kResourcesNum; ++i) {
        ResourceWrapper<Resource> resource_wrapper(pool);
        resource_wrapper.GetRawResource();
    }
}

int main() {
    RequestRourceTest();
    return 0;
}
```


RAII技术的核心思想是将**资源的获取和释放绑定在对象的生命周期**中,这样可以确保资源在不再需要时被正确释放。我们还介绍了如何使用RAII技术来管理动态内存、文件句柄和互斥锁等资源,并提供了一些示例代码来说明如何实现RAII类。

## 断言

### 动态断言

assert用来断言一个表达式必定为真。比如说,数字必须是正数,指针必须非空、函数必须返回 true：

```c++
assert(i > 0 && "i must be greater than zero");
assert(p != nullptr);
assert(!str.empty());
```

当程序（也就是 CPU）运行到 `assert` 语句时,就会计算表达式的值,如果是 false,就会输出错误消息,然后调用 `abort()` 终止程序的执行。
注意,assert **虽然是一个宏**,但在**预处理阶段不生效**,而是在运行阶段才起作用,所以又叫“动态断言”。

### 静态断言

静态断言（static assertion）内的断言表达式不是整数常量表达式导致的。在C++中,静态断言用于在**编译时**对代码进行静态检查,并在断言条件为假时产生编译错误。静态断言通常用于验证在编译时已知的条件,而不是在运行时进行的条件检查。

```c++
static_assert(0 == 3, "a must equal to three"); // 使用字面值常量
```

```c++
template<int N>  // 注意N是一个模板参数,实际上当N实例化时,N是一个已知的编译时常量
struct fib
{
	static_assert(N >= 0, "N >= 0");
	static const int value =
	fib<N - 1>::value + fib<N - 2>::value;
};
int result=fib<5>::value; // 编译时就知道是5了
```

要想保证我们的程序只在64位系统上运行,可以用静态断言在编译阶段检查long 的大小,必须是8个字节（当然,你也可以换个思路用预处理编程来实现）。

```c++
static_assert(sizeof(long) >= 8, "must run on x64");
```

注意：
`static_assert` 运行在编译阶段,只能看到编译时的常数和类型,看不到运行时的**变量、指针、内存数据**等,是“静态”的,所以不要简单地把 assert 的习惯搬过来用。

```c++
char* p = nullptr;
static_assert(p == nullptr, "some error."); // 错误用法
```


```c++
template<typename T>
void check_type(T v){
  static_assert(is_integral<T>::value,"int");
  cout << "static_assert : " << typeid(v).name() << endl;
  cout << is_void<void>::value << endl;
}
int main() {
  int c=4;
  check_type(c);
}
```

注意：
- 在C++中,模板参数是在<font color=#ff0000>编译时</font>求值的。当使用函数模板check_type时,虽然您传递的是一个变量c,但类型T是编译时确定的,即int。这允许static_assert在编译时进行断言检查。

总结：静态断言可以作为**编译期的一种约定**,配合错误提示能够更快发现编译期的错误。

### if constexpr

**特点：**
1. 编译时条件判断：`if constexpr` 语句的条件在编译时进行求值，如果条件为 true，则编译 if 语句块中的代码；如果条件为 false，则编译 else 语句块中的代码（如果有的话）。
2. 消除无用代码：在条件为 false 的情况下，相关的代码块不仅不会执行，它们甚至不会被编译进最终的程序。这可以减少最终可执行文件的大小，并避免无关代码引起的潜在编译错误。
3. 简化模板编程：`if constexpr` 在模板编程中特别有用，允许根据模板参数的不同特性编写不同的代码路径。这对于编写能够处理多种类型的泛型代码尤为重要。

**示例**
下面是一个使用 if constexpr 的例子，它展示了如何根据模板参数是否为整数类型来选择不同的代码路径：

```c++
#include <iostream>
#include <type_traits>

template <typename T>
void process(T value) {
    if constexpr (std::is_integral<T>::value) {
        std::cout << "Processing integer: " << value << std::endl;
    } else {
        std::cout << "Processing non-integer: " << value << std::endl;
    }
}
int main() {
    process(10);    // 输出: Processing integer: 10
    process(3.14);  // 输出: Processing non-integer: 3.14
    return 0;
}
```

在这个例子中，if constexpr 用于在编译时判断 T 是否为整数类型。根据 T 的类型，编译器将只编译相应的代码块。

**使用注意**
- `if constexpr` 仅适用于条件可以在编译时确定的情况。这通常意味着条件表达式依赖于模板参数。
- 在 `if constexpr` 的条件为 false 的代码块中，即使包含无效或不合法的代码，只要这些代码不是正在编译的路径，编译器也不会报错。

总的来说，`if constexpr` 是 C++17 中一个重要的特性，它为编写更灵活、更高效的模板代码提供了强大的支持。

### std::conditional_t

`std::conditional_t` 是 C++ 标准库中的一个模板类型，它是 `std::conditional` 的一个简化版本，用于在编译时根据一个条件选择其中一个类型。它是类型特征（type traits）的一部分，这是 C++ 模板元编程中的一个重要概念。

基本上，`std::conditional_t` 是这样工作的：

- 它接受三个参数：一个布尔表达式和两种类型。
- 如果布尔表达式为 `true`，`std::conditional_t` 将产生第二个参数所指定的类型。
- 如果布尔表达式为 `false`，它将产生第三个参数所指定的类型。

换句话说，`std::conditional_t` 是一个编译时的三元运算符（`?:`）的类型版本。

在你给出的代码中，`std::conditional_t` 被用于基于模板参数 `T` 是否为 `void` 来选择返回类型：
```c++
std::conditional_t<std::is_void_v<T>, void, std::vector<T>>
```

这里的用法如下：

- `std::is_void_v<T>` 检查 `T` 是否是 `void` 类型。如果是 `void`，表达式的值为 `true`；否则为 `false`。
- 如果 `T` 是 `void`，`std::conditional_t` 产生 `void` 类型。
- 如果 `T` 不是 `void`，`std::conditional_t` 产生 `std::vector<T>` 类型。

这个特性在模板编程中非常有用，特别是当你需要根据模板参数的不同类型来决定不同的行为或返回类型时。在你的示例中，它允许同一个函数 `get` 根据 `T` 的类型返回不同的类型，从而实现了一种编译时的多态。

# 类和对象

## 类和对象的基本概念

### 类的封装
- 封装
	1.  把变量（属性）和函数（操作）合成一个整体,封装在一个类中
	2.  对变量和函数进行访问控制
- 访问权限
	1.	在类的内部(作用域范围内),没有访问权限之分,所有成员可以相互访问
	2.	在类的外部(作用域范围外),访问权限才有意义：public,private,protected
	3.	在类的外部,只有public修饰的成员才能被访问,在没有涉及继承与派生时,private和protected是同等级的,外部不允许访问

|    访问属性     | 属性  | 对象内部 | 对象外部 |
| :---------: | :-: | :--: | :--: |
|  `public`   | 公有  | 可访问  | 可访问  |
| `protected` | 保护  | 可访问  | 不可访问 |
|  `private`  | 私有  | 可访问  | 不可访问 |


```c++
//封装两层含义
//1. 属性和行为合成一个整体
//2. 访问控制,现实事物本身有些属性和行为是不对外开放
class Person{
//人具有的行为(函数)
public:
	void Dese(){ cout << "我有钱,年轻,个子又高,就爱嘚瑟!" << endl;}
//人的属性(变量)
public:
	int mTall; //多高,可以让外人知道
protected:
	int mMoney; // 有多少钱,只能儿子孙子知道
private:
	int mAge; //年龄,不想让外人知道
};

int main(){
	Person p;
	p.mTall = 220;
	//p.mMoney 保护成员外部无法访问
	//p.mAge 私有成员外部无法访问
	p.Dese();

	return EXIT_SUCCESS;
}
```


#c语言面试点 
> [!help] struct和class的区别?
> 
> class默认访问权限为private,struct默认访问权限为public.

```c++
class A{
	int mAge;
};
struct B{
	int mAge;
};

void test(){
	A a;
	B b;
	//a.mAge; //无法访问私有成员
	b.mAge; //可正常外部访问
}
```


### 将成员变量设置为private
1.	可赋予客户端访问数据的一致性。
	如果成员变量不是public,客户端唯一能够访问对象的方法就是通过成员函数。如果类中所有public权限的成员都是函数,客户在访问类成员时只会默认访问函数,不需要考虑访问的成员需不需要添加(),这就省下了许多搔首弄耳的时间。
2.	可细微划分访问控制。
	使用成员函数可使得我们对变量的控制处理更加精细。如果我们让所有的成员变量为public,每个人都可以读写它。如果我们设置为private,我们可以实现“不准访问”、“只读访问”、“读写访问”,甚至你可以写出“只写访问”。
```c++
class AccessLevels{
public:
	//对只读属性进行只读访问
	int getReadOnly(){ return readOnly; }
	//对读写属性进行读写访问
	void setReadWrite(int val){ readWrite = val; }
	int getReadWrite(){ return readWrite; }
	//对只写属性进行只写访问
	void setWriteOnly(int val){ writeOnly = val; }
private:
	int readOnly; //对外只读访问
	int noAccess; //外部不可访问
	int readWrite; //读写访问
	int writeOnly; //只写访问
};`
```

## 对象的构造和析构

### 构造函数和析构函数
构造函数主要作用在于创建对象时为对象的成员属性赋值,构造函数由编译器自动调用,无须手动调用。
析构函数主要用于对象销毁前系统自动调用,执行一些清理工作。
**构造函数语法**：
- 构造函数函数名和类名相同,没有返回值,不能有void,但可以有参数。
- ClassName(){}

**析构函数语法**：
- 析构函数函数名是在类名前面加”~”组成,没有返回值,不能有void,不能有参数,不能重载。
- ~ClassName(){}

```c++
class Person{
public:
	Person(){
		cout << "构造函数调用!" << endl;
		pName = (char*)malloc(sizeof("John"));
		strcpy(pName, "John");
		mTall = 150;
		mMoney = 100;
	}
	~Person(){
		cout << "析构函数调用!" << endl;
		if (pName != NULL){
			free(pName);
			pName = NULL;
		}
	}
public:
	char* pName;
	int mTall;
	int mMoney;
};

void test(){
	Person person;
	cout << person.pName << person.mTall << person.mMoney << endl;
}
```


### 构造函数的分类及调用
- 按参数类型：分为无参构造函数和有参构造函数
- 按类型分类：普通构造函数和拷贝构造函数(复制构造函数)
```c++
class Person{
public:
	Person(){
		cout << "no param constructor!" << endl;
		mAge = 0;
	}
	//有参构造函数
	Person(int age){
		cout << "1 param constructor!" << endl;
		mAge = age;
	}
	//拷贝构造函数(复制构造函数) 使用另一个对象初始化本对象
	Person(const Person& person){
		cout << "copy constructor!" << endl;
		mAge = person.mAge;
	}
	//打印年龄
	void PrintPerson(){
		cout << "Age:" << mAge << endl;
	}
private:
	int mAge;
};
//1. 无参构造调用方式
void test01(){
	
	//调用无参构造函数
	Person person1; 
	person1.PrintPerson();

	//无参构造函数错误调用方式
	//Person person2();
	//person2.PrintPerson();
}
//2. 调用有参构造函数
void test02(){
	
	//第一种 括号法,最常用
	Person person01(100);
	person01.PrintPerson();

	//调用拷贝构造函数
	Person person02(person01);
	person02.PrintPerson();

	//第二种 匿名对象(显示调用构造函数)
	Person(200); //匿名对象,没有名字的对象

	Person person03 = Person(300);
	person03.PrintPerson();

	//注意: 使用匿名对象初始化判断调用哪一个构造函数,要看匿名对象的参数类型
	Person person06(Person(400)); //等价于 Person person06 = Person(400);
	person06.PrintPerson();

	//第三种 =号法 隐式转换
	Person person04 = 100; //Person person04 =  Person(100)
	person04.PrintPerson();

	//调用拷贝构造
	Person person05 = person04; //Person person05 =  Person(person04)
	person05.PrintPerson();
}
```

> <font color=#ff0000>b为A的实例化对象,A a = A(b) 和 A(b)的区别</font>？
> 当A(b) 有变量来接的时候,那么编译器认为他是一个匿名对象,当没有变量来接的时候,编译器认为你A(b) 等价于 A b.

<font color=#ff0000>注意</font>:不能调用拷贝构造函数去初始化匿名对象,也就是说以下代码不正确:
```c++
class Teacher{
public:
	Teacher(){
		cout << "默认构造函数!" << endl;
	}
	Teacher(const Teacher& teacher){
		cout << "拷贝构造函数!" << endl;
	}
public:
	int mAge;
};
void test(){
	
	Teacher t1;
	//error C2086:“Teacher t1”: 重定义
	Teacher(t1);  //此时等价于 Teacher t1;
}
```

### 拷贝构造函数的调用时机
- 对象以值传递的方式传给函数参数
- 函数局部对象以值传递的方式从函数返回(vs debug模式下调用一次拷贝构造,qt不调用任何构造)
- 用一个对象初始化另一个对象

```c++
class Person{
public:
	Person(){
		cout << "no param contructor!" << endl;
		mAge = 10;
	}
	Person(int age){
		cout << "param constructor!" << endl;
		mAge = age;
	}
	Person(const Person& person){
		cout << "copy constructor!" << endl;
		mAge = person.mAge;
	}
	~Person(){
		cout << "destructor!" << endl;
	}
public:
	int mAge;
};
//1. 旧对象初始化新对象
void test01(){

	Person p(10);
	Person p1(p);
	Person p2 = Person(p);
	Person p3 = p; // 相当于Person p2 = Person(p);
}

//2. 传递的参数是普通对象,函数参数也是普通对象,传递将会调用拷贝构造
void doBussiness(Person p){}

void test02(){
	Person p(10);
	doBussiness(p);
}

//3. 函数返回局部对象
Person MyBusiness(){
	Person p(10);
	cout << "局部p:" << (int*)&p << endl;
	return p;
}

```

### 构造函数调用规则
- 默认情况下,c++编译器至少为我们写的类增加3个函数
	1. 默认构造函数(无参,函数体为空)
	2. 默认析构函数(无参,函数体为空)
	3. 默认拷贝构造函数,对类中非静态成员属性简单值拷贝
- 如果用户定义拷贝构造函数,c++不会再提供任何默认构造函数
- 如果用户定义了普通构造(非拷贝),c++不在提供默认无参构造,但是会提供默认拷贝构造


#cpp面试点 
### 拷贝构造函数的几个细节


> [!question] 1.为什么拷贝构造函数必须是引用传递，不能是值传递？
> - 为了防止无限的递归调用。 具体一些可以这么讲： 当一个对象需要以值方式传递时，编译器会生成代码调用它的拷贝构造函数以生成一个复本。如果类A的拷贝构造函数是以值方式传递一个类A对象作为参数的话，当需要调用类A的拷贝构造函数时，需要以值方式传进一个A的对象作为实参； 而以值方式传递需要调用类A的拷贝构造函数；结果就是调用类A的拷贝构造函数导致又一次调用类A的拷贝构造函数，这就是一个无限递归。
> - 可以这样理解A a2(a1); 执行这句时，因为值传递我们需要一个other，这个other是a1的副本，但是产生这个other的过程相当于A other(a1) ，又会创建一个新的other,而导致无限循环


### 深拷贝和浅拷贝
#### 浅拷贝
同一类型的对象之间可以赋值,使得两个对象的成员变量的值相同,两个对象仍然是独立的两个对象,这种情况被称为浅拷贝.
一般情况下,浅拷贝没有任何副作用,但是当类中有指针,并且指针指向**动态分配**的内存空间,析构函数做了动态内存释放的处理,会导致内存问题。

<img src="https://article.biliimg.com/bfs/article/cf1701b39cce5f38d776baaf830e1a9538716159.png" alt="image.png" style="zoom:70%;" />



#### 深拷贝
当类中有指针,并且此指针有**动态分配**空间,析构函数做了释放处理,往往需要<font color=#ff0000>自定义拷贝构造函数</font>,自行给指针动态分配空间,深拷贝。
 
<img src="https://article.biliimg.com/bfs/article/246897f4975f01c666c66a981c2e3d4c38716159.png" alt="image.png" style="zoom:70%;" />


```c++
class Person{
public:
	Person(char* name,int age){
		pName = (char*)malloc(strlen(name) + 1);
		strcpy(pName,name);
		mAge = age;
	}
	//增加拷贝构造函数
	Person(const Person& person){
		pName = (char*)malloc(strlen(person.pName) + 1);
		strcpy(pName, person.pName);
		mAge = person.mAge;
	}
	~Person(){
		if (pName != NULL){
			free(pName);
		}
	}
private:
	char* pName;
	int mAge;
};

void test(){
	Person p1("Edward",30);
	//用对象p1初始化对象p2,调用c++提供的默认拷贝构造函数
	Person p2 = p1; // Person p2(p1);
}`
```

### 多个对象构造和析构
#### 初始化列表
构造函数和其他函数不同,除了有名字,参数列表,函数体之外还有初始化列表。
初始化列表简单使用:
```c++
class Person{
public:
#if 0
	//传统方式初始化
	Person(int a,int b,int c){
		mA = a;
		mB = b;
		mC = c;
	}
#endif
	//初始化列表方式初始化
	Person(int a, int b, int c):mA(a),mB(b),mC(c){}
	void PrintPerson(){
		cout << "mA:" << mA << endl;
		cout << "mB:" << mB << endl;
		cout << "mC:" << mC << endl;
	}
private:
	int mA;
	int mB;
	int mC;
};
```

<font color=#ff0000>注意</font>：初始化成员列表(参数列表)只能在构造函数使用。

#### 类对象作为成员
在类中定义的数据成员一般都是基本的数据类型。但是类中的成员也可以是对象,叫做对象成员。
C++中对对象的初始化是非常重要的操作,当创建一个对象的时候,c++编译器必须确保调用了所有子对象的构造函数。如果所有的子对象有默认构造函数,编译器可以自动调用他们。但是如果子对象没有默认的构造函数,或者想指定调用某个构造函数怎么办？
那么是否可以在类的构造函数直接调用子类的属性完成初始化呢？但是如果子类的成员属性是私有的,我们是没有办法访问并完成初始化的。
解决办法非常简单：对于**子类调用构造函数**,c++为此提供了专门的语法,即<font color=#ff0000>构造函数初始化列表</font>。
当调用构造函数时,首先按各对象成员在<font color=#ff0000>类定义中的顺序</font>（和参数列表的顺序无关）依次调用它们的构造函数,对这些对象初始化,最后再调用本身的函数体。也就是说,<font color=#ff0000>先调用对象成员的构造函数,再调用本身的构造函数</font>。
析构函数和构造函数调用顺序相反,先构造,后析构。
```c++
//汽车类
class Car{
public:
	Car(){
		cout << "Car 默认构造函数!" << endl;
		mName = "大众汽车";
	}
	Car(string name){
		cout << "Car 带参数构造函数!" << endl;
		mName = name;
	}
	~Car(){
		cout << "Car 析构函数!" << endl;
	}
public:
	string mName;
};

//拖拉机
class Tractor{
public:
	Tractor(){
		cout << "Tractor 默认构造函数!" << endl;
		mName = "爬土坡专用拖拉机";
	}
	Tractor(string name){
		cout << "Tractor 带参数构造函数!" << endl;
		mName = name;
	}
	~Tractor(){
		cout << "Tractor 析构函数!" << endl;
	}
public:
	string mName;
};

//人类
class Person{
public:
#if 1
	//类mCar不存在合适的构造函数
	Person(string name){
		mName = name;
	}
#else
	//初始化列表可以指定调用构造函数
	Person(string carName, string tracName, string name) : mTractor(tracName), mCar(carName), mName(name){
		cout << "Person 构造函数!" << endl;
	}
#endif
	
	void GoWorkByCar(){
		cout << mName << "开着" << mCar.mName << "去上班!" << endl;
	}
	void GoWorkByTractor(){
		cout << mName << "开着" << mTractor.mName << "去上班!" << endl;
	}
	~Person(){
		cout << "Person 析构函数!" << endl;
	}
private:
	string mName;
	Car mCar;
	Tractor mTractor;
};

void test(){
	//Person person("宝马", "东风拖拉机", "赵四");
	Person person("刘能");
	person.GoWorkByCar();
	person.GoWorkByTractor();
}
```

### explicit关键字
c++提供了关键字explicit,禁止通过构造函数进行的隐式转换。声明为explicit的构造函数不能在隐式转换中使用。

`[explicit注意]`
- explicit用于修饰构造函数,防止隐式转化。
- 是针对单参数的构造函数(或者除了第一个参数外其余参数都有默认值的多参构造)而言。


```c++
class MyString{
public:
	explicit MyString(int n){
		cout << "MyString(int n)!" << endl;
	}
	MyString(const char* str){
		cout << "MyString(const char* str)" << endl;
	}
};

int main(){

	//给字符串赋值？还是初始化？
	//MyString str1 = 1; 
	MyString str2(10);

	//寓意非常明确,给字符串赋值
	MyString str3 = "abcd";
	MyString str4("abcd");

	return EXIT_SUCCESS;
}
```

### 动态对象创建
当我们创建数组的时候,总是需要提前预定数组的长度,然后编译器分配预定长度的数组空间,在使用数组的时,会有这样的问题,数组也许空间太大了,浪费空间,也许空间不足,所以对于数组来讲,如果能根据需要来分配空间大小再好不过。
所以动态的意思意味着不确定性。
为了解决这个普遍的编程问题,在运行中可以创建和销毁对象是最基本的要求。当然c早就提供了动态内存分配（`dynamic memory allocation`）,函数malloc和free可以在运行时从堆中分配存储单元。
然而这些函数在c++中不能很好的运行,因为它不能帮我们完成对象的初始化工作。
#### 对象创建
当创建一个c++对象时会发生两件事:
1.	为对象分配内存
2.	调用构造函数来初始化那块内存
第一步我们能保证实现,需要我们确保第二步一定能发生。c++强迫我们这么做是因为使用未初始化的对象是程序出错的一个重要原因。
#### C动态分配内存方法
为了在运行时动态分配内存,c在他的标准库中提供了一些函数,malloc以及它的变种calloc和realloc,释放内存的free,这些函数是有效的、但是原始的,需要程序员理解和小心使用。为了使用c的动态内存分配函数在堆上创建一个类的实例,我们必须这样做:
```c++
class Person{
public:
	Person(){
		mAge = 20;
		pName = (char*)malloc(strlen("john")+1);
		strcpy(pName, "john");
	}
	void Init(){
		mAge = 20;
		pName = (char*)malloc(strlen("john")+1);
		strcpy(pName, "john");
	}
	void Clean(){
		if (pName != NULL){
			free(pName);
		}
	}
public:
	int mAge;
	char* pName;
};
int main(){

	//分配内存
	Person* person = (Person*)malloc(sizeof(Person));
	if(person == NULL){
		return 0;
	}
	//调用初始化函数
	person->Init();
	//清理对象
	person->Clean();
	//释放person对象
	free(person);

	return EXIT_SUCCESS;
}
```

问题：
1. 程序员必须确定对象的长度。
2. malloc返回一个`void*`指针,c++不允许将`void*`赋值给其他任何指针,必须强转。
3. malloc可能申请内存失败,所以必须判断返回值来确保内存分配成功。
4. 用户在使用对象之前必须记住对他初始化,构造函数不能显示调用初始化(构造函数是由编译器调用),用户有可能忘记调用初始化函数。

c的动态内存分配函数太复杂,容易令人混淆,是不可接受的,c++中我们推荐使用运算符new 和 delete.

#### new operator 
C++中解决动态内存分配的方案是把创建一个对象所需要的操作都结合在一个称为new的运算符里。当用new创建一个对象时,它就在堆里为对象分配内存**并调用构造函数完成初始化**。
```c++
Person* person = new Person;
相当于:
Person* person = (Person*)malloc(sizeof(Person));
	if(person == NULL){
		return 0;
	}
person->Init(); 构造函数

// new 运算还支持在某个指定的内存中构造完成初始化
new (pt) T(val); // 用于在特定的内存位置（也就是pt所指向的位置）上构造一个对象T
```
New操作符能确定在调用构造函数初始化之前内存分配是成功的,所有不用显式确定调用是否成功。
现在我们发现在堆里创建对象的过程变得简单了,只需要一个简单的表达式,它带有内置的长度计算、类型转换和安全检查。这样在堆创建一个对象和在栈里创建对象一样简单。

#### delete operator
new表达式的反面是delete表达式。delete表达式先调用析构函数,然后释放内存。正如new表达式返回一个指向对象的指针一样,delete需要一个对象的地址。
delete只适用于由new创建的对象。
如果使用一个由malloc或者calloc或者realloc创建的对象使用delete,这个行为是未定义的。因为大多数new和delete的实现机制都使用了malloc和free,所以很可能没有调用析构函数就释放了内存。
如果正在删除的对象的指针是NULL,将不发生任何事,因此建议在删除指针后,立即把指针赋值为NULL,以免对它删除两次,对一些对象删除两次可能会产生某些问题。

```c++
class Person{
public:
	Person(){
		cout << "无参构造函数!" << endl;
		pName = (char*)malloc(strlen("undefined") + 1);
		strcpy(pName, "undefined");
		mAge = 0;
	}
	Person(char* name, int age){
		cout << "有参构造函数!" << endl;
		pName = (char*)malloc(strlen(name) + 1);
		strcpy(pName, name);
		mAge = age;
	}
	void ShowPerson(){
		cout << "Name:" << pName << " Age:" << mAge << endl;
	}
	~Person(){
		cout << "析构函数!" << endl;
		if (pName != NULL){
			delete pName;
			pName = NULL;
		}
	}
public:
	char* pName;
	int mAge;
};

void test(){
	Person* person1 = new Person;
	Person* person2 = new Person("John",33);

	person1->ShowPerson();
	person2->ShowPerson();

	delete person1;
	delete person2;
}
```

#cpp面试点 
new和delete与malloc和free的区别?
- new不仅可以做**内存开辟**，还可以做内存初始化操作 
- malloc开辟内存失败，是通过返回值和nullptr做比较；而new开辟内存失败，是通过抛出badalloc类型的异常来判断的。 
- malloc按字节开辟内存的；new开辟内存时需要指定类型 
- opeartor new /operator delete可以被重载，⽽malloc/free并不允许重载。
- new/delete会调⽤对象的构造函数/析构函数以完成对象的构造/析构。⽽malloc则不会

#### 用于数组的new和delete
使用new和delete在堆上创建数组非常容易。
```c++
//创建字符数组
char* pStr = new char[100];
//创建整型数组
int* pArr1 = new int[100]; 
//创建整型数组并初始化
int* pArr2 = new int[10]{ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

//释放数组内存
delete[] pStr;
delete[] pArr1;
delete[] pArr2;
```

<font color=#ff0000>当创建一个对象数组的时候,必须对数组中的每一个对象调用构造函数</font>,除了在栈上可以聚合初始化,必须提供一个默认的构造函数。
```c++
class Person{
public:
	Person(){
		pName = (char*)malloc(strlen("undefined") + 1);
		strcpy(pName, "undefined");
		mAge = 0;
	}
	Person(char* name, int age){
		pName = (char*)malloc(sizeof(name));
		strcpy(pName, name);
		mAge = age;
	}
	~Person(){
		if (pName != NULL){
			delete pName;
		}
	}
public:
	char* pName;
	int mAge;
};

void test(){
	//栈聚合初始化
	Person person[] = { Person("john", 20), Person("Smith", 22) };
	cout << person[1].pName << endl;
    //创建堆上对象数组必须提供构造函数
	Person* workers = new Person[20];
}
```

#### delete `void*`可能会出错
如果对一个`void*`指针执行delete操作,这将可能成为一个程序错误,除非指针指向的内容是非常简单的,因为它将不执行析构函数.以下代码未调用析构函数,导致可用内存减少。

```c++
class Person{
public:
	Person(char* name, int age){
		pName = (char*)malloc(sizeof(name));
		strcpy(pName,name);
		mAge = age;
	}
	~Person(){
		if (pName != NULL){
			delete pName;
		}
	}
public:
	char* pName;
	int mAge;
};

void test(){
	void* person = new Person("john",20);
	delete person;
}
```


问题：malloc、free和new、delete可以混搭使用吗？也就是说malloc分配的内存,可以调用delete吗？通过new创建的对象,可以调用free来释放吗？

#### 使用new和delete采用相同形式
```c++
Person* person = new Person[10];
delete person;
```
以上代码有什么问题吗？(vs下直接中断、qt下析构函数调用一次)
使用了new也搭配使用了delete,问题在于Person有10个对象,那么其他9个对象可能没有调用析构函数,也就是说其他9个对象可能删除不完全,因为它们的析构函数没有被调用。
我们现在清楚使用new的时候发生了两件事: 一、分配内存；二、调用构造函数,那么调用delete的时候也有两件事：一、析构函数；二、释放内存。
那么刚才我们那段代码最大的问题在于：person指针指向的内存中到底有多少个对象,因为这个决定应该有多少个析构函数应该被调用。换句话说,person指针指向的是一个单一的对象还是一个数组对象,由于单一对象和数组对象的内存布局是不同的。更明确的说,数组所用的内存通常还包括“**数组大小记录**”,使得delete的时候知道应该调用几次析构函数。单一对象的话就没有这个记录。单一对象和数组对象的内存布局可理解为下图:


<img src="https://article.biliimg.com/bfs/article/6c679ac023f8b0edf94e8a02fcbdafc838716159.png" alt="image.png" style="zoom:60%;" />

 
本图只是为了说明,编译器不一定如此实现,但是很多编译器是这样做的。
当我们使用一个delete的时候,我们必须让delete知道指针指向的内存空间中是否存在一个“数组大小记录”的办法就是我们告诉它。当我们使用`delete[]`,那么delete就知道是一个对象数组,从而清楚应该调用几次析构函数。


```c++
void *operator new(size_t size) {
  void *p = malloc(size);
  if (p == nullptr)
    throw std::bad_alloc();
  std::cout << "operator new addr "<<p<< std::endl;
  return p;
}

// delete p;调用p指向对象的析构函数、再调用operator delete释放内存空间
void operator delete(void *p) noexcept {
  std::cout << "operator delete addr "<<p<< std::endl;
  free(p);
}

void *operator new[](size_t size) {
  void *p = malloc(size);
  if (p == nullptr)
    throw std::bad_alloc();
  std::cout << "operator new addr "<< p<< std::endl;
  return p;
}

// delete p;调用p指向对象的析构函数、再调用operator delete释放内存空间
void operator delete[](void *p) noexcept {

  std::cout << "operator delete addr "<<p<<std::endl;
  free(p);
}

class Test {
public:
    Test(int ma = 0) : ma(ma) { std::cout << "Test()" << std::endl; }

    ~Test() {
      std::cout << "~Test()" << std::endl;
    };
private:
    int ma;
};
```

```c++
  Test *p2 = new Test[10];
  std::cout<<"p2 addr "<<p2<<std::endl;
  delete []p2;
/*
operator new addr 0x557c8197beb0 
Test()
Test()
Test()
Test()
Test()
Test()
Test()
Test()
Test()
Test()
p2 addr 0x557c8197beb8
~Test()
operator delete addr 0x557c8197beb8  地址不一样，这个指向的是一个元素的地址
munmap_chunk(): invalid pointer
*/
```

```c++
Test *p2 = new Test[10];
std::cout<<"p2 addr "<<p2<<std::endl;
delete []p2;
/*
operator new addr 0x56543f289eb0
Test()
Test()
Test()
Test()
Test()
Test()
Test()
Test()
Test()
Test()
p2 addr 0x56543f289eb8
~Test()
~Test()
~Test()
~Test()
~Test()
~Test()
~Test()
~Test()
~Test()
~Test()
operator delete addr 0x56543f289eb0 要从这个地址开始释放才对
*/
```



**结论**:
    如果在new表达式中使用`[]`,必须在相应的delete表达式中也使用`[]`.如果在new表达式中不使用`[]`, 一定不要在相应的delete表达式中使用`[]`，对于非内置类型使不能够混用的

#### std::memmove

作用是**拷贝一定长度的内存的内容，** 函数原型如下：

```c++
void* memmove( void* dest, const void* src, std::size_t count );
// void * memmove ( void * destination, const void * source, size_t num ); // 另一种形式的声明
```

函数参数定义如下：

| **参数名字** |     **意义**     |
| :------: | :------------: |
|  `dest`  |  目的地的内存地址的指针   |
|  `src`   | 源内容内存地址的**指针** |
| `count`  |  要复制的**字节数**   |
示例代码如下：

```c++
// 代码基本都是使用 C 风格函数
#include <iostream>
#include <cstring>
#include <stdio.h>

// 打印数组函数
void print_array(int* arr, int n) {
    if (arr == NULL) { // C语言用 NULL，C++11用nullptr
        return;
    }
    for (int i = 0; i < n; i++){
        printf("%d ", arr[i]);
    }
    printf("\n");
}

// int 类型对象占 4 个字节
void int_arr_copy_test() {
    int arr[] = {1,2,3,4,5,6,7,8,9}; // int 数组
    print_array(arr, 9);
    // std::memmove(arr + 4, arr + 3, sizeof(int)); // 从 [4, 5, 6] 复制到 [5, 6, 7]
    std::memmove(arr + 4, arr + 3, 4); // 从 [4, 5, 6] 复制到 [5, 6, 7]
    print_array(arr, 9);
}

// char 类型对象占 1 个字节
void char_arr_copy_test() {
    char str1[] = "1234567890"; // char 数组
    puts (str1);
    // std::cout << str << '\n';
    std::memmove(str1 + 4, str1 + 3, 4); // 从 [4, 5, 6] 复制到 [5, 6, 7]
    puts (str1);

}
int main()
{
    int_arr_copy_test();
    char_arr_copy_test();
}
```

程序编译运行后输出如下：

<img src="https://img-blog.csdnimg.cn/7ba3c55ae10a4fcda766e136800db273.png" alt="image.png" style="zoom:50%;" />

内存块拷贝函数的实现
`memmove` 的实现如下

```c++
void *mymemmove(void *dest, const void *src, size_t n)
{
    char temp[n];
    int i;
    char *d = dest;
    const char *s = src;

    for (i = 0; i < n; i++)
        temp[i] = s[i];
    for (i = 0; i < n; i++)
        d[i] = temp[i];

    return dest;
}
```

### allocator类

- 标准库allocator类定义在头文件memory中,帮助我们将内存分配和对象构造分离开。
- 分配的是原始的、未构造的内存。
- allocator是一个模板。 
- `allocator<string> alloc`;


**标准库allocator类及其算法**：

|           操作           |                                                                    解释                                                                     |
| :--------------------: | :---------------------------------------------------------------------------------------------------------------------------------------: |
|    `allocator<T> a`    |                                                定义了一个名为`a`的`allocator`对象,它可以为类型为`T`的对象分配内存                                                 |
|    `a.allocate(n)`     |                                                      分配一段原始的、未构造的内存,保存`n`个类型为`T`的对象。                                                      |
|  `a.deallocate(p, n)`  | 释放从`T*`指针`p`中地址开始的内存,这块内存保存了`n`个类型为`T`的对象；`p`必须是一个先前由`allocate`返回的指针。且`n`必须是`p`创建时所要求的大小。在调用`deallocate`之前,用户必须对每个在这块内存中创建的对象调用`destroy`。 |
| `a.construct(p, args)` |                                   `p`必须是一个类型是`T*`的指针,指向一块原始内存；`args`被传递给类型为`T`的构造函数,用来在`p`指向的内存中构造一个对象。                                   |
|     `a.destroy(p)`     |                                                     `p`为`T*`类型的指针,此算法对`p`指向的对象执行析构函数。                                                     |

先说明为什么需要把内存开辟和对象的构造分开以及对象的析构和内存释放分离开

```c++
template<typename T>
class vector{
private:
    T *_first;  // 指向数组的第一个元素
    T *_last;  // 指向数组有效元素的后继位置
    T *_end;   // 指向数组空间的后继位置
    void expand(){
        int size = _end - _first;
        int newsize = size * 2;
        T *ptmp = new T[newsize];
        for (int i = 0; i < size; ++i) {
            ptmp[i]=_first[i];
        }
        delete [] _first;
        _first=ptmp;
        _last=_first+size;
        _end=_first+newsize;
    }
public:
    vector(int size=10){
        // 需要把内存开辟和对象的构造分开来做
        _first = new T[size];
        _last = _first;
        _end = _first + size;
    }
    ~vector(){
      // 析构容器有效的元素，然后释放整个容器底层的内存空间
      delete [] _first;
      _first=_last=_end=nullptr;
    }
    vector(const vector<T> &rhs){
        int size = rhs._end - rhs._first;
        _first = new T[size];
        _last = _first + (rhs._last - rhs._first);
        _end = _first + size;
        for(int i=0;i<size;i++){
            _first[i] = rhs._first[i];
        }
    }
    vector<T>& operator=(const vector<T> &rhs){
      if (this==&rhs) return *this;
      delete [] _first;
      int size = rhs._end - rhs._first;
      _first = new T[size];
      int len=rhs._last-rhs._first;
      for (int i = 0; i < len; ++i) {
        _first[i]=rhs._first[i];
      }
      _last=_first+len;
      _end=_first+size;
      return *this;
    }
    void push_back(const T &val){ // 向末尾添加元素
      if (full()) expand();
      *_last++ = val;
    }

    void pop_cack(){
      if (empty()) return;
      --_last;
    }
    T back() const{
      return *(_last-1);
    }
    bool full() const {return _last==_end;}
    bool empty() const {return _first==_last;}
    int size() const {return _last-_first;}
};
```

```python
class Test{
public:
    Test(){
    std::cout<<"Test()"<<std::endl;
  }
  ~Test(){
    std::cout<<"~Test()"<<std::endl;
  }
};

int main(){
	vector<Test> v1;
}
```

我们会打印很多Test的构造和析构函数，因为new操作内存分配和对象构造没有分离开

这个时候我们就需要allocator类帮助我们将内存分配和对象构造分离开。

```python
// 容器的空间配置器allocator 做四件事情 内存开辟/内存释放 对象构造/对象析构
// 定义容器的空间配置器，和c++标准库的allocator一样
template<typename T>
class Allocator{
public:
    // 内存开辟
    T* allocate(int size){
      return (T *)malloc(sizeof(T)*size);
    }
    // 内存释放
    void deallocate(T *pt){
      free(pt);
    }
    // 对象构造
    void construct(T *pt,const T &val){
      new (pt) T(val);
    }
    // 对象析构
    void destroy(T *pt){
      pt->~T();
    }
};
```

```python
// 容器底层内存开辟，内存释放，对象构造和析构，都通过allocator空间配置器来实现
template<typename T,typename Alloc=Allocator<T>>
class vector{
private:
    T *_first;  // 指向数组的第一个元素
    T *_last;  // 指向数组有效元素的后继位置
    T *_end;   // 指向数组空间的后继位置
    Alloc _alloc; // 定义容器的空间配置器对象
    void expand(){
        int size = _end - _first;
        int newsize = size * 2;
        T *ptmp =_alloc.allocate(newsize);

        for (int i = 0; i < size; ++i) {
            _alloc.construct(ptmp+i,_first[i]);
        }
        for (int i = 0; i < size; ++i) {
            _alloc.destroy(_first+i);
        }
        _alloc.deallocate(_first);
        _first=ptmp;
        _last=_first+size;
        _end=_first+newsize;
    }
public:
    vector(int size=10){
        // 需要把内存开辟和对象的构造分开来做
        _first = _alloc.allocate(size);
        _last = _first;
        _end = _first + size;
    }
    ~vector(){
      // 析构容器有效的元素，然后释放整个容器底层的内存空间
      for (T *pt = _first; pt != _last; ++pt) {
        _alloc.destroy(pt);
      }
      _alloc.deallocate(_first);
      _first=_last=_end=nullptr;
    }
    vector(const vector<T> &rhs){
        int size = rhs._end - rhs._first;
        _first = _alloc.allocate(size);
        _last = _first + (rhs._last - rhs._first);
        _end = _first + size;
        for(int i=0;i<size;i++){
            _alloc.construct(_first+i,rhs._first[i]);
        }
    }
    vector<T>& operator=(const vector<T> &rhs){
      if (this==&rhs) return *this;

      for(T *pt = _first; pt != _last; ++pt) {
        _alloc.destroy(pt);
      }
      _alloc.deallocate(_first);
      int size = rhs._end - rhs._first;
      _first = _alloc.allocate(size);
      int len=rhs._last-rhs._first;
      for (int i = 0; i < len; ++i) {
        _alloc.construct(_first+i,rhs._first[i]);
      }
      _last=_first+len;
      _end=_first+size;
      return *this;
    }
    void push_back(const T &val){ // 向末尾添加元素
      if (full()) expand();
      _alloc.construct(_last++,val);
    }
    void pop_cack(){
      if (empty()) return;
      _alloc.destroy(--_last);
    }
    T back() const{
      return *(_last-1);
    }
    bool full() const {return _last==_end;}
    bool empty() const {return _first==_last;}
    int size() const {return _last-_first;}
};
```

### 静态成员
在类定义中,它的成员（包括成员变量和成员函数）,这些成员可以用关键字static声明为静态的,称为静态成员。
不管这个类创建了多少个对象,静态成员只有一个拷贝,这个拷贝被所有属于这个类的对象共享。

#### 静态成员变量
在一个类中,若将一个成员变量声明为`static`,这种成员称为静态成员变量。与一般的数据成员不同,无论建立了多少个对象,都只有一个静态数据的拷贝。静态成员变量,属于某个类,所有对象共享。 
静态变量,是在编译阶段就分配空间,对象还没有创建时,就已经分配空间。

- 静态成员变量必须在类中声明,在类外定义。
- 静态数据成员不属于某个对象,在为对象分配空间中不包括静态成员所占空间。
- 静态数据成员可以通过类名或者对象名来引用。

```c++
class Person{
public:
	//类的静态成员属性
	static int sNum;
private:
	static int sOther;
};
//类外初始化,初始化时不加static
int Person::sNum = 0;
int Person::sOther = 0;
int main(){
	//1. 通过类名直接访问
	Person::sNum = 100;
	cout << "Person::sNum:" << Person::sNum << endl;
	//2. 通过对象访问
	Person p1, p2;
	p1.sNum = 200;
	cout << "p1.sNum:" << p1.sNum << endl;
	cout << "p2.sNum:" << p2.sNum << endl;
	//3. 静态成员也有访问权限,类外不能访问私有成员
	//cout << "Person::sOther:" << Person::sOther << endl;
	Person p3;
	//cout << "p3.sOther:" << p3.sOther << endl;
	system("pause");
	return EXIT_SUCCESS;
}
```

#### 静态成员函数
在类定义中,前面有static说明的成员函数称为静态成员函数。静态成员函数使用方式和静态变量一样,同样在对象没有创建前,即可通过类名调用。静态成员函数主要为了访问静态变量,但是,不能访问普通成员变量。
静态成员函数的意义,不在于信息共享,数据沟通,而在于**管理静态数据成员**,完成对静态数据成员的封装。

- 静态成员函数只能访问静态变量,不能访问普通成员变量
- 静态成员函数的使用和静态成员变量一样
- 静态成员函数也有访问权限
- 普通成员函数可访问静态成员变量、也可以访问非经常成员变量

```c++
class Person{
public:
	//普通成员函数可以访问static和non-static成员属性
	void changeParam1(int param){
		mParam = param;
		sNum = param;
	}
	//静态成员函数只能访问static成员属性
	static void changeParam2(int param){
		//mParam = param; //无法访问
		sNum = param;
	}
private:
	static void changeParam3(int param){
		//mParam = param; //无法访问
		sNum = param;
	}
public:
	int mParam;
	static int sNum;
};

//静态成员属性类外初始化
int Person::sNum = 0;
int main(){
	//1. 类名直接调用
	Person::changeParam2(100);

	//2. 通过对象调用
	Person p;
	p.changeParam2(200);

	//3. 静态成员函数也有访问权限
	//Person::changeParam3(100); //类外无法访问私有静态成员函数
	//Person p1;
	//p1.changeParam3(200);
	return EXIT_SUCCESS;
}

```

#### const静态成员属性
如果一个类的成员,既要实现共享,又要实现不可改变,那就用`static const`修饰。定义静态const数据成员时,最好在类内部初始化。
```c++
class Person{
public:
	//static const int mShare = 10;
	const static int mShare = 10; //只读区,不可修改
};
int main(){

	cout << Person::mShare << endl;
	//Person::mShare = 20;

	return EXIT_SUCCESS;
}
```
#### 静态成员实现单例模式
单例模式是一种常用的软件设计模式。在它的核心结构中只包含一个被称为单例的特殊类。通过单例模式可以保证系统中一个类只有一个实例而且该实例易于外界访问,从而方便对实例个数的控制并节约系统资源。如果希望在系统中某个类的对象只能存在一个,单例模式是最好的解决方案。
 
![image.png](https://article.biliimg.com/bfs/article/db5d382fa8fe449a40b6c35e4470308538716159.png)


<font color=#ff0000>Singleton（单例</font>）：在单例类的内部实现只生成一个实例,同时它提供一个静态的`getInstance()`工厂方法,让客户可以访问它的唯一实例；为了防止在外部对其实例化,将其**默认构造函数和拷贝构造函数设计为私有**；在单例类内部定义了一个Singleton类型的静态对象,作为外部共享的唯一实例。

用单例模式,模拟公司员工使用打印机场景,打印机可以打印员工要输出的内容,并且可以累积打印机使用次数。


```c++
class Printer{
public:
	static Printer* getInstance(){ return pPrinter;}
	void PrintText(string text){
		cout << "打印内容:" << text << endl;
		cout << "已打印次数:" << mTimes << endl;
		cout << "--------------" << endl;
		mTimes++;
	}
private:
	Printer(){ mTimes = 0; }
	Printer(const Printer&){}
private:
	static Printer* pPrinter;
	int mTimes;
};

Printer* Printer::pPrinter = new Printer;

void test(){
	Printer* printer = Printer::getInstance();
	printer->PrintText("离职报告!");
	printer->PrintText("入职合同!");
	printer->PrintText("提交代码!");
}
```


## C++面向对象模型初探
### 成员变量和函数的存储 

c++实现了“封装”,那么数据(成员属性)和操作(成员函数)是什么样的呢？
“数据”和“处理数据的操作(函数)”是分开存储的。
- c++中的<font color=#ff0000>非静态数据成员</font>直接内含在类对象中,就像c struct一样。
- 成员函数(member function)虽然内含在class声明之内,却不出现在对象中。
- 每一个非内联成员函数(non-inline member function)只会诞生一份函数实例.

```c++
class MyClass01{
public:
	int mA;
};

class MyClass02{
public:
	int mA;
	static int sB;
};

class MyClass03{
public:
	void printMyClass(){
		cout << "hello world!" << endl;
	}
public:
	int mA;
	static int sB;
};

class MyClass04{
public:
	void printMyClass(){
		cout << "hello world!" << endl;
	}
	static void ShowMyClass(){
		cout << "hello world！" << endl;
	}
public:
	int mA;
	static int sB;
};

int main(){

	MyClass01 mclass01;
	MyClass02 mclass02;
	MyClass03 mclass03;
	MyClass04 mclass04;

	cout << "MyClass01:" << sizeof(mclass01) << endl; //4
	//静态数据成员并不保存在类对象中
	cout << "MyClass02:" << sizeof(mclass02) << endl; //4
	//非静态成员函数不保存在类对象中
	cout << "MyClass03:" << sizeof(mclass03) << endl; //4
	//静态成员函数也不保存在类对象中
	cout << "MyClass04:" << sizeof(mclass04) << endl; //4

	return EXIT_SUCCESS;
}
```


通过上面的案例,我们可以的得出：<font color=#ff0000>C++类对象中的变量和函数是分开存储</font>。

### this指针
#### this指针工作原理
通过上例我们知道,c++的数据和操作也是分开存储,并且每一个非内联成员函数(non-inline member function)只会诞生一份函数实例,也就是说多个同类型的对象会共用一块代码
那么问题是：这一块代码是如何区分那个对象调用自己的呢？

<img src="https://article.biliimg.com/bfs/article/65f25bd467d80dc5c4b269bdc7baa84f38716159.png" alt="image.png" style="zoom:50%;" />

 
c++通过提供特殊的对象指针,this指针,解决上述问题。this指针指向被调用的成员函数所属的对象。
c++规定,this指针是**隐含在对象成员函数内**的一种指针。当一个对象被创建后,它的每一个成员函数都含有一个系统自动生成的隐含指针this,用以保存这个对象的地址,也就是说虽然我们没有写上this指针,编译器在编译的时候也是会加上的。因此this也称为“指向本对象的指针”,this指针并不是对象的一部分,不会影响sizeof(对象)的结果。
this指针是C++实现封装的一种机制,它将对象和该对象调用的成员函数连接在一起,在外部看来,每一个对象都拥有自己的函数成员。一般情况下,并不写this,而是让系统进行默认设置。

> this指针永远指向当前对象。


成员函数通过this指针即可知道操作的是那个对象的数据。This指针是一种隐含指针,它隐含于每个类的非静态成员函数中。This指针无需定义,直接使用即可。
注意：静态成员函数内部没有this指针,静态成员函数不能操作非静态成员变量。

c++编译器对普通成员函数的内部处理
#### this指针的使用
- 当形参和成员变量同名时,可用this指针来区分
- 在类的非静态成员函数中返回对象本身,可使用`return *this`.
```c++
class Person{
public:
	//1. 当形参名和成员变量名一样时,this指针可用来区分
	Person(string name,int age){
		//name = name;
		//age = age; //输出错误
		this->name = name;
		this->age = age;
	}
	//2. 返回对象本身的引用
	//重载赋值操作符
	//其实也是两个参数,其中隐藏了一个this指针
	Person PersonPlusPerson(Person& person){
		string newname = this->name + person.name;
		int newage = this->age + person.age;
		Person newperson(newname, newage);
		return newperson;
	}
	void ShowPerson(){
		cout << "Name:" << name << " Age:" << age << endl;
	}
public:
	string name;
	int age;
};

//3. 成员函数和全局函数(Perosn对象相加)
Person PersonPlusPerson(Person& p1,Person& p2){
	string newname = p1.name + p2.name;
	int newage = p1.age + p2.age;
	Person newperson(newname,newage);
	return newperson;
}

int main(){

	Person person("John",100);
	person.ShowPerson();

	cout << "---------" << endl;
		Person person1("John",2······················
	Person person3 = PersonPlusPerson(person1, person2);
	person1.ShowPerson();
	person2.ShowPerson();
	person3.ShowPerson();
	//2. 成员函数实现两个对象相加
	Person person4 = person1.PersonPlusPerson(person2);
	person4.ShowPerson();

	system("pause");
	return EXIT_SUCCESS;
}
```

#### const修饰成员函数

- 用const修饰的成员函数时,<font color=#ff0000>const修饰this指针指向的内存区域</font>,成员函数体内不可以修改本类中的任何普通成员变量,
- 当成员变量类型符前用mutable修饰时例外。
- 通过使用mutable类型符修饰**互斥锁**，我们可以在const成员函数中对互斥锁进行操作而不违反const限定。这样可以确保即使在const成员函数中也能够保持线程安全性。

```cpp
class MyClass {
public:
    void doSomething() const {  // const成员函数
        std::lock_guard<std::mutex> lock(mutex);  // 获取互斥锁
        // 对类的数据成员进行操作
    }
private:
    mutable std::mutex mutex;  // 使用mutable修饰互斥锁
};
```

在上述代码中，`doSomething()`是一个const成员函数，但它需要获取互斥锁来保证线程安全性。通过使用`mutable`修饰`std::mutex`，我们可以在const成员函数中获取和释放该互斥锁。

总而言之，使用`mutable`类型符修饰互斥锁允许在const成员函数中对互斥锁进行修改，以保持线程安全性。

```c++
//const修饰成员函数
class Person{
public:
	Person(){
		this->mAge = 0;
		this->mID = 0;
	}
	//在函数括号后面加上const,修饰成员变量不可修改,除了mutable变量
	void sonmeOperate() const{
		//this->mAge = 200; //mAge不可修改
		this->mID = 10;
	}
	void ShowPerson(){
		cout << "ID:" << mID << " mAge:" << mAge << endl;
	}
private:
	int mAge;
	mutable int mID;
};

int main(){

	Person person;
	person.sonmeOperate();
	person.ShowPerson();

	system("pause");
	return EXIT_SUCCESS;
}
```

#### const修饰对象(常对象)

- 常对象<font color=#ff0000>只能调用const的成员函数</font>
- 常对象可访问const或非const数据成员,不能修改,除非成员用mutable修饰

```c++
class Person{
public:
	Person(){
		this->mAge = 0;
		this->mID = 0;
	}
	void ChangePerson() const{
		//mAge = 100;
		mID = 100;
	}
	void ShowPerson(){
        this->mAge = 1000;
		cout << "ID:" << this->mID << " Age:" << this->mAge << endl;
	}

public:
	int mAge;
	mutable int mID;
};

void test(){	
	const Person person;
	//1. 可访问数据成员
	cout << "Age:" << person.mAge << endl;
	//person.mAge = 300; //不可修改
	person.mID = 1001; //但是可以修改mutable修饰的成员变量
	//2. 只能访问const修饰的函数
	//person.ShowPerson();
	person.ChangePerson();
}
```

<img src="https://article.biliimg.com/bfs/article/6a0b49d23a7a526ddf65086d49a78fa538716159.png" alt="image.png" style="zoom:50%;" />

## 友元

类的主要特点之一是数据隐藏,即类的私有成员无法在类的外部(作用域之外)访问。但是,有时候需要在类的外部访问类的私有成员,怎么办？
解决方法是<font color=#ff0000>使用友元函数</font>,友元函数是一种特权函数,c++允许这个特权函数访问私有成员。这一点从现实生活中也可以很好的理解：
比如你的家,有客厅,有你的卧室,那么你的客厅是Public的,所有来的客人都可以进去,但是你的卧室是私有的,也就是说只有你能进去,但是呢,你也可以允许你的闺蜜好基友进去。
程序员可以把一个全局函数、某个类中的成员函数、甚至整个类声明为友元。
### 友元语法

- friend关键字只出现<font color=#ff0000>在声明</font>处
- 其他类、类成员函数、全局函数都可声明为友元
- 友元函数不是类的成员,不带this指针
- 友元函数可访问对象任意成员属性,包括私有属性

```c++
class Building;
//友元类
class MyFriend{
public:
	//友元成员函数
	void LookAtBedRoom(Building& building);
	void PlayInBedRoom(Building& building);
};
class Building{
	//全局函数做友元函数
	friend void CleanBedRoom(Building& building);
#if 0
	//成员函数做友元函数
	friend void MyFriend::LookAtBedRoom(Building& building);
	friend void MyFriend::PlayInBedRoom(Building& building);
#else	
	//友元类
	friend class MyFriend;
#endif
public:
	Building();
public:
	string mSittingRoom;
private:
	string mBedroom;
};

void MyFriend::LookAtBedRoom(Building& building){
	cout << "我的朋友参观" << building.mBedroom << endl;
}
void MyFriend::PlayInBedRoom(Building& building){
	cout << "我的朋友玩耍在" << building.mBedroom << endl;
}

//友元全局函数
void CleanBedRoom(Building& building){
	cout << "友元全局函数访问" << building.mBedroom << endl;
}

Building::Building(){
	this->mSittingRoom = "客厅";
	this->mBedroom = "卧室";
}

int main(){

	Building building;
	MyFriend myfriend;

	CleanBedRoom(building);
	myfriend.LookAtBedRoom(building);
	myfriend.PlayInBedRoom(building);

	system("pause");
	return EXIT_SUCCESS;
}
```


> [!note] 友元类注意
> 1. 友元关系不能被继承。
> 2. 友元关系是单向的,类A是类B的朋友,但类B不一定是类A的朋友。
> 3. 友元关系不具有传递性。类B是类A的朋友,类C是类B的朋友,但类C不一定是类A的朋友。

### 扩展的 friend 语法

#### 语法改进
在 C++11 标准中对 friend关键字进行了一些改进,以保证其更加好用：
声明一个类为另外一个类的友元时,不再需要使用class关键字,并且还可以使用类的别名（使用 typedef 或者 using 定义）。
我们可以看看下面的例子：
```c++
#include <iostream>
using namespace std;

// 类声明
class Tom;
// 定义别名
using Honey = Tom;

// 定义两个测试类
class Jack
{
    // 声明友元
    // friend class Tom;    // C++98 标准语法
    friend Tom;             // C++11 标准语法 
    string name = "jack";   // 默认私有
    void print()            // 默认私有
    {
        cout << "my name is " << name << endl;
    }
};

class Lucy
{
protected:
    // 声明友元
    // friend class Tom;    // C++98 标准语法
    friend Honey;           // C++11 标准语法 
    string name = "lucy";
    void print()
    {
        cout << "my name is " << name << endl;
    }
};

class Tom
{
public:
    void print()
    {
        // 通过类成员对象访问其私有成员
        cout << "invoke Jack private member: " << jObj.name << endl;
        cout << "invoke Jack private function: " << endl;
        jObj.print();

        cout << "invoke Lucy private member: " << lObj.name << endl;
        cout << "invoke Lucy private function: " << endl;
        lObj.print();
    }
private:
    string name = "tom";
    Jack jObj;
    Lucy lObj;
};

int main()
{
    Tom t;
    t.print();
    return 0;
}
```

在上面的例子中 `Tom 类`分别作为了`Jack类`和`Lucy类`的友元类,然后在`Tom类`中定义了`Jack类`和`Lucy类`的对象`jObj`和`lObj`,这样我们就可以在Tom类中通过这两个类对象直接访问它们各自的私有或者受保护的成员变量或者成员函数了。

#### 为类模板声明友元
虽然在C++11标准中对友元的改进不大,却会带来应用的变化——程序员可以为类模板声明友元了,这在C++98中是无法做到的。使用方法如下：
```c++
class Tom;

template<typename T>  
class Person
{
    friend T;
};

int main()
{
    Person<Tom> p;
    Person<int> pp;
    return 0;
}
```

- 第11行：Tom类是Person类的友元
- 第12行：对于int类型的模板参数,友元声明被忽略（第6行）
这样一来,我们就可以在模板实例化时才确定一个模板类是否有友元,以及谁是这个模板类的友元。
下面基于一个实际场景来讲解一下如何给模板类指定友元：
假设有一个矩形类,一个圆形类,我们在对其进行了一系列的操作之后,需要验证一下矩形的宽度和高度、圆形的半径是否满足要求,并且要求这个校验操作要在另一个类中完成。

```c++
template<typename T>  
class Rectangle
{
public:
    friend T;
    Rectangle(int w, int h) : width(w), height(h) {}
private:
    int width;
    int height;
};

template<typename T> 
class Circle
{
public:
    friend T;
    Circle(int r) : radius(r) {}
private:
    int radius;
};

// 校验类
class Verify
{
public:
    void verifyRectangle(int w, int h, Rectangle<Verify> &r)
    {
        if (r.width >= w && r.height >= h)
        {
            cout << "矩形的宽度和高度满足条件!" << endl;
        }
        else
        {
            cout << "矩形的宽度和高度不满足条件!" << endl;
        }
    }

    void verifyCircle(int r, Circle<Verify> &c)
    {
        if (r >= c.radius)
        {
            cout << "圆形的半径满足条件!" << endl;
        }
        else
        {
            cout << "圆形的半径不满足条件!" << endl;
        }
    }
};

int main()
{
    Verify v;
    Circle<Verify> circle(30);
    Rectangle<Verify> rect(90, 100);
    v.verifyCircle(60, circle);
    v.verifyRectangle(100, 100, rect);
    return 0;
}
```

- 第28行：在Verify类中 访问了 Rectangle类 的私有成员变量
- 第40行：在Verify类中 访问了 Circle类 的私有成员变量
程序输出的结果：
```c++
圆形的半径满足条件!
矩形的宽度和高度不满足条件!
```

在上面的例子中我们定义了两个类模板`Rectangle`和`Circle`并且将其模板类型定义为了它们的友元（如果是模板类型是基础类型友元的定义就被忽略了）。在main()函数中测试的时候将`Verify`类指定为了两个模板类的实际友元类型。这样我们在`Verify`类中就可以通过`Rectangle`类和`Circle`类的实例对象访问它们内部的私有成员变量了。

补充说明：
1. 在上面的测试程序中`Rectangle`类和`Circle类`我们没有提供对应的set方法来设置私有成员的值,为了简化程序直接通过构造函数的初始化列表完成了它们的初始化。
2. 在上面的程序中也没有给`Rectangle类`和`Circle类`提供get方法,这样如果想要在类外部访问私有（或受保护）成员就只能使用友元了（此处这样处理完全了为了测试的需要）。
## 运算符重载
### 运算符重载基本概念
运算符重载,就是对已有的运算符重新进行定义,赋予其另一种功能,以适应不同的数据类型。
<font color=#ff0000>运算符重载(operator overloading)只是一种”语法上的方便”,也就是它只是另一种函数调用的方式。</font>
在c++中,可以定义一个处理类的新运算符。这种定义很像一个普通的函数定义,只是函数的名字由关键字operator及其紧跟的运算符组成。差别仅此而已。它像任何其他函数一样也是一个函数,当编译器遇到适当的模式时,就会调用这个函数。
**语法**：
	定义重载的运算符就像定义函数,只是该函数的名字是`operator@`,这里的@代表了被重载的运算符。函数的参数中参数个数取决于两个因素。
- 运算符是一元(一个参数)的还是二元(两个参数)；
- 运算符被定义为全局函数(对于一元是一个参数,对于二元是两个参数)还是成员函数(对于一元没有参数,对于二元是一个参数-此时该类的对象用作左耳参数)

### 运算符重载碰上友元函数
友元函数是一个全局函数,和我们上例写的全局函数类似,只是友元函数可以访问某个类私有数据。
案例: **重载左移操作符(<<),使得cout可以输出对象。**
```c++
class Person{
	friend ostream& operator<<(ostream& os, Person& person);
public:
	Person(int id,int age){
		mID = id;
		mAge = age;
	}
private:
	int mID;
	int mAge;
};

ostream& operator<<(ostream& os, Person& person){
	os << "ID:" << person.mID << " Age:" << person.mAge;
	return os;
}

int main(){

	Person person(1001, 30);
	//cout << person; //cout.operator+(person)
	cout << person << " | " << endl;

	return EXIT_SUCCESS;
}
```

### 可重载的运算符
几乎C中所有的运算符都可以重载,但运算符重载的使用时相当受限制的。特别是<font color=#ff0000>不能使用C中当前没有意义的运算符</font>(例如用`**`求幂)<font color=#ff0000>不能改变运算符优先级</font>,不能改变运算符的参数个数。这样的限制有意义,否则,所有这些行为产生的运算符只会混淆而不是澄清寓语意。

<img src="https://article.biliimg.com/bfs/article/143177e09f4ebbb25a1598876bc93a7b38716159.png" alt="image.png" style="zoom:50%;" />

### 自增自减(++/--)运算符重载
重载的++和--运算符有点让人不知所措,因为我们总是希望能根据它们出现在所作用对象的前面还是后面来调用不同的函数。解决办法很简单,例如当编译器看到++a(前置++),它就调用operator++(a),当编译器看到a++（后置++）,它就会去调用`operator++(a,int)`.

```c++
class Complex{
	friend ostream& operator<<(ostream& os,Complex& complex){
		os << "A:" << complex.mA << " B:" << complex.mB << endl;
		return os;
	}
public:
	Complex(){
		mA = 0;
		mB = 0;
	}
	//重载前置++
	Complex& operator++(){
		mA++;
		mB++;
		return *this;
	}
	//重载后置++
	Complex operator++(int){	
		Complex temp;
		temp.mA = this->mA;
		temp.mB = this->mB;
		mA++;
		mB++;
		return temp;
	}
	//前置--
	Complex& operator--(){
		mA--;
		mB--;
		return *this;
	}
	//后置--
	Complex operator--(int){
		Complex temp;
		temp.mA = mA;
		temp.mB = mB;
		mA--;
		mB--;
		return temp;
	}
	void ShowComplex(){
		cout << "A:" << mA << " B:" << mB << endl;
	}
private:
	int mA;
	int mB;
};

void test(){
	Complex complex;
	complex++;
	cout << complex;
	++complex;
	cout << complex;

	Complex ret = complex++;
	cout << ret;
	cout << complex;

	cout << "------" << endl;
	ret--;
	--ret;
	cout << "ret:" << ret;
	complex--;
	--complex;
	cout << "complex:" << complex;
}
```


### 指针运算符(`*、->`)重载
```c++
class Person{
public:
	Person(int param){
		this->mParam = param;
	}
	void PrintPerson(){
		cout << "Param:" << mParam << endl;
	}
private:
	int mParam;
};

class SmartPointer{
public:
	SmartPointer(Person* person){
		this->pPerson = person;
	}
	//重载指针的->、*操作符
	Person* operator->(){
		return pPerson;
	}
	Person& operator*(){
		return *pPerson;
	}
	~SmartPointer(){
		if (pPerson != NULL){
			delete pPerson;
		}
	}
public:
	Person* pPerson;
};

void test01(){
	
	//Person* person = new Person(100);
	//如果忘记释放,那么就会造成内存泄漏

	SmartPointer pointer(new Person(100));
	pointer->PrintPerson();
}
```

### 赋值(=)运算符重载
赋值符常常初学者的混淆。这是毫无疑问的,因为’=’在编程中是最基本的运算符,可以进行赋值操作,也能引起拷贝构造函数的调用。

```c++
class Person{
	friend ostream& operator<<(ostream& os,const Person& person){
		os << "ID:" << person.mID << " Age:" << person.mAge << endl;
		return os;
	}
public:
	Person(int id,int age){
		this->mID = id;
		this->mAge = age;
	}
	//重载赋值运算符
	Person& operator=(const Person& person){
		this->mID = person.mID;
		this->mAge = person.mAge;
		return *this;
	}
private:
	int mID;
	int mAge;
};

//1. =号混淆的地方
void test01(){
	Person person1(10, 20);
	Person person2 = person1; //调用拷贝构造
	//如果一个对象还没有被创建,则必须初始化,也就是调用构造函数
	//上述例子由于person2还没有初始化,所以会调用构造函数
	//由于person2是从已有的person1来创建的,所以只有一个选择
	//就是调用拷贝构造函数
	person2 = person1; //调用operator=函数
	//由于person2已经创建,不需要再调用构造函数,这时候调用的是重载的赋值运算符
}
//2. 赋值重载案例
void test02(){
	Person person1(20, 20);
	Person person2(30, 30);
	cout << "person1:" << person1;
	cout << "person2:" << person2;
	person2 = person1;
	cout << "person2:" << person2;
}
//常见错误,当准备给两个相同对象赋值时,应该首先检查一下这个对象是否对自身赋值了
//对于本例来讲,无论如何执行这些赋值运算都是无害的,但如果对类的实现进行修改,那么将会出现差异；
//3. 类中指针
class Person2{
	friend ostream& operator<<(ostream& os, const Person2& person){
		os << "Name:" << person.pName << " ID:" << person.mID << " Age:" << person.mAge << endl;
		return os;
	}
public:
	Person2(char* name,int id, int age){
		this->pName = new char[strlen(name) + 1];
		strcpy(this->pName, name);
		this->mID = id;
		this->mAge = age;
	}
#if 1
	//重载赋值运算符
	Person2& operator=(const Person2& person){

		//注意:由于当前对象已经创建完毕,那么就有可能pName指向堆内存
		//这个时候如果直接赋值,会导致内存没有及时释放
		if (this->pName != NULL){
			delete[] this->pName;
		}

		this->pName = new char[strlen(person.pName) + 1];
		strcpy(this->pName,person.pName);
		this->mID = person.mID;
		this->mAge = person.mAge;
		return *this;
	}
#endif
	//析构函数
	~Person2(){
		if (this->pName != NULL){
			delete[] this->pName;
		}
	}
private:
	char* pName;
	int mID;
	int mAge;
};

void test03(){
	Person2 person1("John",20, 20);
	Person2 person2("Edward",30, 30);
	cout << "person1:" << person1;
	cout << "person2:" << person2;
	person2 = person1;
	cout << "person2:" << person2;
}
```

如果没有重载赋值运算符,编译器会自动创建默认的赋值运算符重载函数。行为类似默认拷贝构造,进行简单值拷贝。

### 等于和不等于($==、!=$)运算符重载

```c++
class Complex{
public:
	Complex(char* name,int id,int age){
		this->pName = new char[strlen(name) + 1];
		strcpy(this->pName, name);
		this->mID = id;
		this->mAge = age;
	}
	//重载==号操作符
	bool operator==(const Complex& complex){
		if (strcmp(this->pName,complex.pName) == 0 && 
		    this->mID == complex.mID && 
			this->mAge == complex.mAge){
			return true;
		}
		return false;
	}
	//重载!=操作符
	bool operator!=(const Complex& complex){
		if (strcmp(this->pName, complex.pName) != 0 || 
		    this->mID != complex.mID || 
			this->mAge != complex.mAge){
			return true;
		}
		return false;
	}
	~Complex(){
		if (this->pName != NULL){
			delete[] this->pName;
		}
	}
private:
	char* pName;
	int mID;
	int mAge;
};
void test(){
	Complex complex1("aaa", 10, 20);
	Complex complex2("bbb", 10, 20);
	if (complex1 == complex2){ cout << "相等!" << endl; }
	if (complex1 != complex2){ cout << "不相等!" << endl; }
}
```

### 函数调用符号()重载
```c++
class Complex{
public:
	int Add(int x,int y){
		return x + y;
	}
	int operator()(int x,int y){
		return x + y;
	}
};
void test01(){
	Complex complex;
	cout << complex.Add(10,20) << endl;
	//对象当做函数来调用
	cout << complex(10, 20) << endl;
}
```

这里常常用于自定义排序的规则

### 类型转换操作符函数

类型转换操作符函数是类成员函数,它允许将类对象转换为其他类型。这样可以在需要特定类型的地方使用类对象,从而提供更高的灵活性和可用性。

类型转换操作符函数有以下特点：

1. **命名和语法：** 类型转换操作符函数的命名必须是`operator type()`的形式,其中`type`表示要转换的目标类型。它没有返回类型的声明,因为返回类型是由`type`指定的。
2. **没有参数：** 类型转换操作符函数不带任何参数。它们只是作为类的成员函数存在。
3. **explicit关键字（可选）：** 类型转换操作符函数可以用`explicit`关键字进行显式声明,以防止隐式类型转换。如果没有使用`explicit`关键字,编译器将自动应用隐式类型转换。
4. **返回类型：** 类型转换操作符函数的返回类型是由`type`指定的目标类型。它可以是任何有效的C++类型,包括基本类型、自定义类型和标准库类型。
5. **函数体：** 类型转换操作符函数的函数体中包含实现将类对象转换为目标类型的逻辑。根据需要,可以访问类的成员变量和成员函数。

下面是一个示例,展示了类型转换操作符函数的使用：

```cpp
class MyString {
private:
    std::string m_string;
public:
    explicit operator int() const {
        return std::stoi(m_string);
    }
    
    explicit operator double() const {
        return std::stod(m_string);
    }
    
    MyString(const std::string& str) : m_string(str) {}
};

int main() {
    MyString str("42");
    
    int num = static_cast<int>(str);
    double dbl = static_cast<double>(str);
    
    std::cout << num << std::endl;  // 输出 42
    std::cout << dbl << std::endl;  // 输出 42.0
    
    return 0;
}
```

在这个示例中,`MyString`类定义了两个类型转换操作符函数,分别转换为`int`和`double`类型。在`main`函数中,我们创建了一个`MyString`对象 `str`,然后使用`static_cast`显式转换为`int`和`double`类型。
通过类型转换操作符函数,我们可以在需要`int`或`double`类型的地方使用`MyString`对象,并根据需要进行类型转换。这提供了更大的灵活性,使得用户可以自由地将类对象转换为所需的类型,以满足特定的需求。

### 符号重载总结

- $=, [], ()$ 和 -> 操作符只能通过<font color=#ff0000>成员函数</font>进行重载 
- $<<$ 和 $>>$只能通过<font color=#ff0000>全局函数</font>配合友元函数进行重载 
- 不要重载 $&&$ 和 $||$ 操作符,因为无法实现短路规则


常规建议

<img src="https://article.biliimg.com/bfs/article/f911a7268011814586f083811a7d41e438716159.png" alt="image.png" style="zoom:50%;" />


## 继承和派生
### 继承概述
#### 为什么需要继承

<img src="https://article.biliimg.com/bfs/article/8ccdf2e7658bc9c16ed22739367d0de838716159.png" alt="image.png" style="zoom:50%;" />


```c++
网页类
class IndexPage{
public:
	//网页头部
	void Header(){
		cout << "网页头部!" << endl;
	}
	//网页左侧菜单
	void LeftNavigation(){
		cout << "左侧导航菜单!" << endl;
	}
	//网页主体部分
	void MainBody(){
		cout << "首页网页主题内容!" << endl;
	}
	//网页底部
	void Footer(){
		cout << "网页底部!" << endl;
	}
private:
	string mTitle; //网页标题
};

#if 0
//如果不使用继承,那么定义新闻页类,需要重新写一遍已经有的代码
class NewsPage{
public:
	//网页头部
	void Header(){
		cout << "网页头部!" << endl;
	}
	//网页左侧菜单
	void LeftNavigation(){
		cout << "左侧导航菜单!" << endl;
	}
	//网页主体部分
	void MainBody(){
		cout << "新闻网页主体内容!" << endl;
	}
	//网页底部
	void Footer(){
		cout << "网页底部!" << endl;
	}
private:
	string mTitle; //网页标题
};

void test(){
	NewsPage* newspage = new NewsPage;
	newspage->Header();
	newspage->MainBody();
	newspage->LeftNavigation();
	newspage->Footer();
}
#else
//使用继承,可以复用已有的代码,新闻业除了主体部分不一样,其他都是一样的
class NewsPage : public IndexPage{
public:
	//网页主体部分
	void MainBody(){
		cout << "新闻网页主主体内容!" << endl;
	}
};
void test(){
	NewsPage* newspage = new NewsPage;
	newspage->Header();
	newspage->MainBody();
	newspage->LeftNavigation();
	newspage->Footer();
}
#endif
int main(){

	test();

	return EXIT_SUCCESS;
}
```

#### 继承基本概念

c++最重要的特征是代码重用,通过继承机制可以利用已有的数据类型来定义新的数据类型,新的类不仅拥有旧类的成员,还拥有新定义的成员。
一个B类继承于A类,或称从类A派生类B。这样的话,类A成为基类（父类）, 类B成为派生类（子类）。
派生类中的成员,包含两大部分：
- 一类是从基类继承过来的,一类是自己增加的成员。
- 从基类继承过过来的表现其共性,而新增的成员体现了其个性。

<img src="https://article.biliimg.com/bfs/article/39429ae1fab56f14cc848d3eed8701cc38716159.png" alt="image.png" style="zoom:80%;" />


#### 派生类定义
**派生类定义格式**：
```c++
Class 派生类名 :  继承方式 基类名{
	 //派生类新增的数据成员和成员函数
}
```

三种继承方式： 
- `public` ：    公有继承
- `private` ：   私有继承
- `protected` ： 保护继承

从继承源上分： 
- 单继承：指每个派生类只直接继承了一个基类的特征
- 多继承：指多个基类派生出一个派生类的继承关系,多继承的派生类直接继承了不止一个基类的特征

### 派生类访问控制

派生类继承基类,派生类拥有基类中全部成员变量和成员方法（除了构造和析构之外的成员方法）,但是在派生类中,继承的成员并不一定能直接访问,不同的继承方式会导致不同的访问权限。
派生类的访问权限规则如下：
 
<img src="https://article.biliimg.com/bfs/article/b422d0a8676fc81d2b8688a170241c0a38716159.png" alt="image.png" style="zoom:50%;" />

<img src="https://article.biliimg.com/bfs/article/d9c71cdc5a01a7d78e3a179fc423cd0438716159.png" alt="image.png" style="zoom:50%;" />

```c++
//基类
class A{
public:
	int mA;
protected:
	int mB;
private:
	int mC;
};
//1. 公有(public)继承
class B : public A{
public:
	void PrintB(){
		cout << mA << endl; //可访问基类public属性
		cout << mB << endl; //可访问基类protected属性
		//cout << mC << endl; //不可访问基类private属性
	}  
};
class SubB : public B{
	void PrintSubB(){
		cout << mA << endl; //可访问基类public属性
		cout << mB << endl; //可访问基类protected属性
		//cout << mC << endl; //不可访问基类private属性
	}
};
void test01(){

	B b;
	cout << b.mA << endl; //可访问基类public属性
	//cout << b.mB << endl; //不可访问基类protected属性
	//cout << b.mC << endl; //不可访问基类private属性
}

//2. 私有(private)继承
class C : private A{
public:
	void PrintC(){
		cout << mA << endl; //可访问基类public属性
		cout << mB << endl; //可访问基类protected属性
		//cout << mC << endl; //不可访问基类private属性
	}
};
class SubC : public C{
	void PrintSubC(){
		//cout << mA << endl; //不可访问基类public属性
		//cout << mB << endl; //不可访问基类protected属性
		//cout << mC << endl; //不可访问基类private属性
	}
};
void test02(){
	C c;
	//cout << c.mA << endl; //不可访问基类public属性
	//cout << c.mB << endl; //不可访问基类protected属性
	//cout << c.mC << endl; //不可访问基类private属性
}
//3. 保护(protected)继承
class D : protected A{
public:
	void PrintD(){
		cout << mA << endl; //可访问基类public属性
		cout << mB << endl; //可访问基类protected属性
		//cout << mC << endl; //不可访问基类private属性
	}
};
class SubD : public D{
	void PrintD(){
		cout << mA << endl; //可访问基类public属性
		cout << mB << endl; //可访问基类protected属性
		//cout << mC << endl; //不可访问基类private属性
	}
};
void test03(){
	D d;
	//cout << d.mA << endl; //不可访问基类public属性
	//cout << d.mB << endl; //不可访问基类protected属性
	//cout << d.mC << endl; //不可访问基类private属性
}
```

### 继承中的构造和析构
#### 继承中的对象模型

在C++编译器的内部可以理解为结构体,子类是由父类成员叠加子类新成员而成：
```c++
class Aclass{
public:
	int mA;
	int mB;
};
class Bclass : public Aclass{
public:
	int mC;
};
class Cclass : public Bclass{
public:
	int mD;
};
void test(){
	cout << "A size:" << sizeof(Aclass) << endl;
	cout << "B size:" << sizeof(Bclass) << endl;
	cout << "C size:" << sizeof(Cclass) << endl;
}
```

#### 对象构造和析构的调用原则

- **继承中的构造和析构**
	- 子类对象在创建时会首先调用父类的构造函数
	- 父类构造函数执行完毕后,才会调用子类的构造函数
	- 当父类构造函数有参数时,需要在子类初始化列表(参数列表)中<font color=#ff0000>显示调用父类构造函数</font>
	- 析构函数调用顺序和构造函数相反

 
```c++
class A{
public:
	A(){
		cout << "A类构造函数!" << endl;
	}
	~A(){
		cout << "A类析构函数!" << endl;
	}
};

class B : public A{
public:
	B(){
		cout << "B类构造函数!" << endl;
	}
	~B(){
		cout << "B类析构函数!" << endl;
	}
};

class C : public B{
public:
	C(){
		cout << "C类构造函数!" << endl;
	}
	~C(){
		cout << "C类析构函数!" << endl;
	}
};

void test(){
	C c;
}
class Base{  
protected:  
    int ma;  
public:  
    Base(int a):ma(a){}  
    virtual void print() const{  
        std::cout << "Base: " << ma << std::endl;  
    }  
};  
  
class Derived: public Base{  
private:  
  int mb;  
public:  
    Derived(int data):Base(data), mb(data){  
      std::cout<<"Derived constructor"<<std::endl;  
    }  
    ~Derived(){  
      std::cout<<"Derived destructor"<<std::endl;  
    }  
  
};
```

- 继承与组合混搭的构造和析构

<img src="https://article.biliimg.com/bfs/article/ef6ab69ab65911ad815c78e429a04c8c38716159.png" alt="image.png" style="zoom:50%;" />


```c++
class D{
public:
	D(){
		cout << "D类构造函数!" << endl;
	}
	~D(){
		cout << "D类析构函数!" << endl;
	}
};
class A{
public:
	A(){
		cout << "A类构造函数!" << endl;
	}
	~A(){
		cout << "A类析构函数!" << endl;
	}
};
class B : public A{
public:
	B(){
		cout << "B类构造函数!" << endl;
	}
	~B(){
		cout << "B类析构函数!" << endl;
	}
};
class C : public B{
public:
	C(){
		cout << "C类构造函数!" << endl;
	}
	~C(){
		cout << "C类析构函数!" << endl;
	}
public:
	D c;
};
void test(){
	C c;
}
```

### 继承中同名成员的处理方法

- 当子类成员和父类成员同名时,子类依然从父类继承同名成员
- 如果子类有成员和父类同名,子类访问其成员默认访问子类的成员(本作用域,就近原则)
- 在子类通过作用域::进行同名成员区分(在派生类中使用基类的同名成员,显示使用类名限定符)
  
```c++
class Base{
public:
	Base():mParam(0){}
	void Print(){ cout << mParam << endl; }
public:
	int mParam;
};

class Derived : public Base{
public:
	Derived():mParam(10){}
	void Print(){
		//在派生类中使用和基类的同名成员,显示使用类名限定符
		cout << Base::mParam << endl;
		cout << mParam << endl;
	}
	//返回基类重名成员
	int& getBaseParam(){ return  Base::mParam; }
public:
	int mParam;
};

int main(){

	Derived derived;
	//派生类和基类成员属性重名,子类访问成员默认是子类成员
	cout << derived.mParam << endl; //10
	derived.Print();
	//类外如何获得基类重名成员属性
	derived.getBaseParam() = 100;
	cout << "Base:mParam:" << derived.getBaseParam() << endl;

	return EXIT_SUCCESS;
}
```

**注意: 如果重新定义了基类中的重载函数,将会发生什么？**

```c++
class Base{
public:
	void func1(){
		cout << "Base::void func1()" << endl;
	};
	void func1(int param){
		cout << "Base::void func1(int param)" << endl;
	}
	void myfunc(){
		cout << "Base::void myfunc()" << endl;
	}
};
class Derived1 : public Base{
public:
	void myfunc(){
		cout << "Derived1::void myfunc()" << endl;
	}
};
class Derived2 : public Base{
public:
	//改变成员函数的参数列表
	void func1(int param1, int param2){
		cout << "Derived2::void func1(int param1,int param2)" << endl;
	};
};
class Derived3 : public Base{
public:
	//改变成员函数的返回值
	int func1(int param){
		cout << "Derived3::int func1(int param)" << endl;
		return 0;
	}
};
int main(){

	Derived1 derived1;
	derived1.func1();
	derived1.func1(20);
	derived1.myfunc();
	cout << "-------------" << endl;
	Derived2 derived2;
	//derived2.func1();  //func1被隐藏
	//derived2.func1(20); //func2被隐藏
	derived2.func1(10,20); //重载func1之后,基类的函数被隐藏
	derived2.myfunc();
	cout << "-------------" << endl;
	Derived3 derived3;
	//derived3.func1();  没有重新定义的重载版本被隐藏
	derived3.func1(20);
	derived3.myfunc();

	return EXIT_SUCCESS;
}
```


> [!summary] 总结
> - Derive1 重定义了Base类的myfunc函数,derive1可访问func1及其重载版本的函数。
> - Derive2通过改变函数参数列表的方式重新定义了基类的func1函数,则从基类中继承来的其他重载版本被隐藏,不可访问
> - Derive3通过改变函数返回类型的方式重新定义了基类的func1函数,则从基类继承来的没有重新定义的重载版本的函数将被隐藏。
> - <font color=#ff0000>隐藏是作用域的隐藏，跟重载不同，只要函数名相同就会隐藏，不看参数列表什么的是否相同</font>


任何时候重新定义基类中的一个重载函数,在新类中所有的其他版本**将被自动隐藏**.

### 非自动继承的函数

不是所有的函数都能自动从基类继承到派生类中。构造函数和析构函数用来处理对象的创建和析构操作,构造和析构函数只知道对它们的特定层次的对象做什么,也就是说构造函数和析构函数不能被继承,必须为每一个特定的派生类分别创建。
另外operator=也不能被继承,因为它完成类似构造函数的行为。也就是说尽管我们知道如何由=右边的对象如何初始化=左边的对象的所有成员,但是这个并不意味着对其派生类依然有效。
在继承的过程中,如果没有创建这些函数,编译器会自动生成它们。

### 继承中的静态成员特性
静态成员函数和非静态成员函数的共同点:
1.	他们都可以被继承到派生类中。
2.	如果重新定义一个静态成员函数,所有在基类中的其他重载函数会被隐藏。
3.	如果我们改变基类中一个函数的特征,所有使用该函数名的基类版本都会被隐藏。

```c++
class Base{
public:
	static int getNum(){ return sNum; }
	static int getNum(int param){
		return sNum + param;
	}
public:
	static int sNum;
};
int Base::sNum = 10;

class Derived : public Base{
public:
	static int sNum; //基类静态成员属性将被隐藏
#if 0
	//重定义一个函数,基类中重载的函数被隐藏
	static int getNum(int param1, int param2){
		return sNum + param1 + param2;
	}
#else
	//改变基类函数的某个特征,返回值或者参数个数,将会隐藏基类重载的函数
	static void getNum(int param1, int param2){
		cout <<  sNum + param1 + param2 << endl;
	}
#endif
};
int Derived::sNum = 20;
```

### 多继承
#### 多继承概念
我们可以从一个类继承,我们也可以能同时从多个类继承,这就是多继承。但是由于多继承是非常受争议的,从多个类继承可能会导致函数、变量等同名导致较多的歧义。 

<img src="https://article.biliimg.com/bfs/article/692493409203e235edc359cdc044c6be38716159.png" alt="image.png" style="zoom:90%;" />


```c++
class Base1{
public:
	void func1(){ cout << "Base1::func1" << endl; }
};
class Base2{
public:
	void func1(){ cout << "Base2::func1" << endl; }
	void func2(){ cout << "Base2::func2" << endl; }
};
//派生类继承Base1、Base2
class Derived : public Base1, public Base2{};
int main(){

	Derived derived;
	//func1是从Base1继承来的还是从Base2继承来的？
	//derived.func1(); 
	derived.func2();

	//解决歧义:显示指定调用那个基类的func1
	derived.Base1::func1(); 
	derived.Base2::func1();

	return EXIT_SUCCESS;
}
```

- 多继承会带来一些二义性的问题, 如果两个基类中有同名的函数或者变量,那么通过派生类对象去访问这个函数或变量时就不能明确到底调用从基类1继承的版本还是从基类2继承的版本？
- 解决方法就是显示指定调用那个基类的版本。

#### 菱形继承和虚继承

两个派生类继承同一个基类而又有某个类同时继承者两个派生类,这种继承被称为菱形继承,或者钻石型继承。

<img src="https://article.biliimg.com/bfs/article/f7e9d16f04f7663b0b88091ac77dadf138716159.png" alt="image.png" style="zoom:60%;" />


这种继承所带来的问题：
1.	羊继承了动物的数据和函数,鸵同样继承了动物的数据和函数,当草泥马调用函数或者数据时,就会产生二义性。
2.	草泥马继承自动物的函数和数据继承了两份,其实我们应该清楚,这份数据我们只需要一份就可以。

```c++
class BigBase{
public:
	BigBase(){ mParam = 0; }
	void func(){ cout << "BigBase::func" << endl; }
public:
	int mParam;
};

class Base1 : public BigBase{};
class Base2 : public BigBase{};
class Derived : public Base1, public Base2{};

int main(){

	Derived derived;
	//1. 对“func”的访问不明确
	//derived.func();
	//cout << derived.mParam << endl;
	cout << "derived.Base1::mParam:" << derived.Base1::mParam << endl;
	cout << "derived.Base2::mParam:" << derived.Base2::mParam << endl;

	//2. 重复继承
	cout << "Derived size:" << sizeof(Derived) << endl; //8

	return EXIT_SUCCESS;
}
```

上述问题如何解决？对于调用二义性,那么可通过指定调用那个基类的方式来解决,那么重复继承怎么解决？
对于这种菱形继承所带来的两个问题,c++为我们提供了一种方式,采用**虚基类**。那么我们采用虚基类方式将代码修改如下：

```c++
class BigBase{
public:
	BigBase(){ mParam = 0; }
	void func(){ cout << "BigBase::func" << endl; }
public:
	int mParam;
};

class Base1 : virtual public BigBase{};
class Base2 : virtual public BigBase{};
class Derived : public Base1, public Base2{};

int main(){

	Derived derived;
	//二义性问题解决
	derived.func();
	cout << derived.mParam << endl;
	//输出结果:12
	cout << "Derived size:" << sizeof(Derived) << endl;

	return EXIT_SUCCESS;
}
```

以上程序Base1 ,Base2采用虚继承方式继承BigBase,那么BigBase被称为**虚基类**。
通过虚继承解决了菱形继承所带来的二义性问题。
但是虚基类是如何解决二义性的呢？并且derived大小为12字节,这是怎么回事？

#### 虚继承实现原理
```c++
class BigBase{
public:
	BigBase(){ mParam = 0; }
	void func(){ cout << "BigBase::func" << endl; }
public: int mParam;
};
#if 0 //虚继承
class Base1 : virtual public BigBase{};
class Base2 : virtual public BigBase{};
#else //普通继承
class Base1 :  public BigBase{};
class Base2 :  public BigBase{};
#endif
class Derived : public Base1, public Base2{};
```



<img src="https://article.biliimg.com/bfs/article/8fbb671c08683c9c57f72d89f99979f338716159.png" alt="image.png" style="zoom:60%;" />



通过内存图,我们发现普通继承和虚继承的对象内存图是不一样的。我们也可以猜测到编译器肯定对我们编写的程序做了一些手脚。（<font color=#ff0000>虚基类继承了一个虚基类指针</font>,指针有一个<font color=#ff0000>虚基类表</font>,表中记录<font color=#ff0000>偏移量</font>,通过这个偏移量可以找到唯一的一块内存数据）

- BigBase 菱形最顶层的类,内存布局图没有发生改变。
- Base1和Base2通过虚继承的方式派生自BigBase,这两个对象的布局图中可以看出编译器为我们的对象中增加了一个vbptr (virtual base pointer),vbptr指向了一张表,这张表保存了当前的虚指针相对于虚基类的<font color=#ff0000>首地址的偏移量</font>。
- Derived派生于Base1和Base2,继承了两个基类的vbptr指针,并调整了vbptr与虚基类的首地址的偏移量。

由此可知编译器帮我们做了一些幕后工作,使得这种菱形问题在继承时候能只继承一份数据,并且也解决了二义性的问题。现在模型就变成了Base1和 Base2 Derived三个类对象共享了一份BigBase数据。

当使用虚继承时,虚基类是被共享的,也就是在继承体系中无论被继承多少次,对象内存模型中均只会出现一个虚基类的子对象（这和多继承是完全不同的）。即使共享虚基类,但是必须要有一个类来完成基类的初始化（因为所有的对象都必须被初始化,哪怕是默认的）,同时还不能够重复进行初始化,那到底谁应该负责完成初始化呢？C++标准中选择在每一次继承子类中<font color=#ff0000>都必须书写初始化语句</font>（因为每一次继承子类可能都会用来定义对象）,但是<font color=#ff0000>虚基类的初始化是由最后的子类完成,其他的初始化语句都不会调用</font>。

```C++
class BigBase{
public:
	BigBase(int x){mParam = x;}
	void func(){cout << "BigBase::func" << endl;}
public:
	int mParam;
};
class Base1 : virtual public BigBase{
public:
	Base1() :BigBase(10){} //不调用BigBase构造
};
class Base2 : virtual public BigBase{
public:
	Base2() :BigBase(10){} //不调用BigBase构造
};

class Derived : public Base1, public Base2{
public:
	Derived() :BigBase(10){} //调用BigBase构造
};
//每一次继承子类中都必须书写初始化语句
int main(){
	Derived derived;
	return EXIT_SUCCESS;
}
```

注意：
    虚继承只能解决具备公共祖先的多继承所带来的二义性问题,不能解决没有公共祖先的多继承的.

工程开发中真正意义上的多继承是几乎不被使用,因为多重继承带来的代码复杂性远多于其带来的便利,多重继承对代码维护性上的影响是灾难性的,在设计方法上,任何多继承都可以用单继承代替。

## 多态

### 虚函数
#### 向上类型转换及问题

对象可以作为自己的类或者作为它的基类的对象来使用。还能**通过基类的地址**来操作它。取一个对象的地址(指针或引用),并将其作为基类的地址来处理,这种称为**向上类型转换**。
也就是说：父类引用或指针可以指向子类对象,通过父类指针或引用来操作子类对象。
```c++
class Animal{
public:
	void speak(){
		cout << "动物在唱歌..." << endl;
	}
};

class Dog : public Animal{
public:
	void speak(){
		cout << "小狗在唱歌..." << endl;
	}
};

void DoBussiness(Animal& animal){
	animal.speak();
}

void test(){
	Dog dog;
	DoBussiness(dog);
}
```

- 运行结果: 动物在唱歌
- 问题抛出: 我们给DoBussiness传入的对象是dog,而不是animal对象,输出的结果应该是Dog::speak。

#### 捆绑

解决这个问题,我们需要了解下绑定(捆绑,binding)概念。

> 把函数体与函数调用相联系称为绑定(捆绑,binding)

##### 静态绑定
当绑定在程序运行之前(由编译器和连接器)完成时,称为早绑定(**静态绑定**)(`early binding`).C语言中只有一种函数调用方式,就是静态绑定。
上面的问题就是由于静态绑定引起的,因为编译器在只有Animal地址时并不知道要调用的正确函数。编译是根据指向对象的指针或引用的类型来选择函数调用。这个时候由于DoBussiness的参数类型是`Animal&`,编译器确定了应该调用的speak是Animal::speak的,而不是真正传入的对象Dog::speak。

##### 动态绑定
解决方法就是**迟绑定**(迟捆绑,动态绑定,运行时绑定,late binding),意味着绑定要根据对象的实际类型,发生在运行。
C++语言要实现这种动态绑定,必须有某种机制来**确定运行时对象的类型**并调用合适的成员函数。对于一种编译语言,编译器并不知道实际的对象类型（编译器并不知道Animal类型的指针或引用指向的实际的对象类型）。

#### 虚函数的概念
C++动态多态性是通过**虚函数**来实现的,虚函数允许子类（派生类）**重新定义**父类（基类）成员函数,而子类（派生类）重新定义父类（基类）虚函数的做法称为覆盖(override),或者称为重写。

- 对于特定的函数进行动态绑定,c++要求在基类中声明这个函数的时候使用`virtual`关键字,动态绑定也就对`virtua`l函数起作用.
- 为创建一个需要动态绑定的虚成员函数,可以简单在这个函数声明前面加上`virtual`关键字,定义时候不需要.
- 如果一个函数在基类中被声明为virtual,那么在所有派生类中它都是`virtual`的.
- 在派生类中virtual函数的重定义称为重写(override).
- `virtual`关键字只能修饰成员函数.
- 构造函数不能为虚函数

<font color=#ff0000>注意</font>: 仅需要在基类中声明一个函数为virtual.调用所有匹配基类声明行为的派生类函数都将使用虚机制。虽然可以在派生类声明前使用关键字virtual(这也是无害的),但这个样会使得程序显得冗余和杂乱。(我建议写上)

```c++
class Animal{
public:
	virtual void speak(){
		cout << "动物在唱歌..." << endl;
	}
};
class Dog : public Animal{
public:
	virtual void speak(){
		cout << "小狗在唱歌..." << endl;
	}
};
void DoBussiness(Animal& animal){
	animal.speak();
}
void test(){
	Dog dog;
	DoBussiness(dog);
}
```
#### 如何实现动态绑定
 
```c++
class Base{
protected:
    int ma;
public:
    Base(int a):ma(a){}
    virtual void show() const{
        std::cout << "show: " << ma << std::endl;
    }
    virtual void show(int) const{
      std::cout << "show(int): " << ma << std::endl;
    }
};
class Derived: public Base{
private:
    int mb;
public:
    Derived(int a=20):Base(a), mb(a){}
    void show() const {
        std::cout << "show: " << ma << ", " << mb << std::endl;
    }
    void show(int) const override{
      std::cout << "show(int): " << ma << ", " << mb << std::endl;
    }
};
```


- 如果类里面定义了**虚函数**,那么编译阶段,编译器给这个类类型产生一个唯一的**vftable**虚函数表,虚函数表中主要存储的内容就是`RTTI`(run-time type information)指针和虚函数的地址。当程序<font color=#ff0000>运行</font>时,每一张虚函数表 都会加载到内存的`.rodata`(只读区域)区，下方的0是**偏移量**。
- RTTI指针指向一个类型信息结构（type_info），该结构存储了有关对象类型的信息，如类的名称和类的继承关系。RTTI主要用于支持C++的**动态类型识别和运行时类型检查**。
- 一个类里面定义了虚函数,那么这个类定义的对象,其运行时,内存中开始部分,多存储一个vfptr虚函数指针,指向相应类型的虚函数表vftable。一个类型定义的n个对象,它们的多个vfpt指向的都是同一张虚函数表 
- 一个类里面虚函数的个数,不影响对象内存大小`（vfptr,) `,影响的是虚函数表的大小 
- 如果派生类中的方法,和基类继承来的某个方法,**返回值、函数名、参数列表**都相同, 而且基类的方法是virtual虚函数,那么派生类的这个方法,自动处理成虚函数 ,这里的覆盖是虚函数表中**虚函数地址的覆盖**

<img src="https://article.biliimg.com/bfs/article/a57a120c01e0fc9d27f0b7baf3bacd0638716159.png" alt="image.png" style="zoom:70%;" />

<img src="https://article.biliimg.com/bfs/article/87118f7206329cd9776f46604423775838716159.png" alt="image.png" style="zoom:70%;" />




```c++
	Derive d(50); 
	Base *pb = &d; 
	/*
	pb->Base Base::show如果发现show是普通函数，就进行静态绑定 
										  call Base::show 
	pb->Base Base::show如果发现show是虚函数，就进行动态绑定了 
	mov eax, dword ptr [pb] 
	mov ecx, dword ptr [eax] 
	call ecx(虚函数的地址）动态（运行时期）的绑定（函数的调用）这里call的时寄存器，是运行时才知道里面放的什么？
	pb的类型：Base->有没有虚函数 
	如果Base没有虚函数，*pb识别的就是编译时期的类型 *pb <=> Base类型 
	如果Base有虚函数，*pb识别的就是运行时期的类型 RTTI类型 
	*/
```

- 当子类无重写基类虚函数时:

<img src="https://article.biliimg.com/bfs/article/6b77c3f8f1ab616a11584ee35c99f42f38716159.png" alt="image.png" style="zoom:50%;" />

 

> **过程分析**:
>      Animal* animal = new Dog;
>      animal->fun1();
>      当程序执行到这里,会去animal指向的空间中寻找vptr指针,通过vptr指针找到func1函数,此时由于子类并没有重写也就是覆盖基类的func1函数,所以调用func1时,仍然调用的是基类的func1.
> **执行结果**: 我是基类的func1
> **测试结论**: 无重写基类的虚函数,无意义

- 当子类重写基类虚函数时:

<img src="https://article.biliimg.com/bfs/article/e0ed2960b54a8a9d69c97bc246a073cd38716159.png" alt="image.png" style="zoom:50%;" />


 
> **过程分析**:
>      Animal* animal = new Dog;
>      animal->fun1();
>      当程序执行到这里,会去animal指向的空间中寻找vptr指针,通过vptr指针找到func1函数,由于子类重写基类的func1函数,所以调用func1时,调用的是子类的func1.
> **执行结果**: 我是子类的func1
> **测试结论**: 无重写基类的虚函数,无意义


**多态的成立条件**：
- 有继承
- 子类重写父类虚函数函数
	- a) 返回值,函数名字,函数参数,必须和父类完全一致(<font color=#ff0000>析构函数除外</font>)
	- b) 子类中virtual关键字可写可不写,建议写
- 类型兼容,父类指针,父类引用 指向 子类对象

**虚函数依赖**： 
1. 虚函数能产生地址，存储在vftable当中 
2. 对象必须存在（vfptr->vftable->虚函数地址） 
3. 如果**不是通过指针或者引用变量**来调用虚函数，那就是静态绑定 


> [!question] 问题：哪些函数不能实现成虚函数？ 
> 1. 构造函数 
> 	- virtual+构造函数 NO! 
> 	- 构造函数中（调用的任何函数，都是静态绑定的）调用虚函数，也不会发生静态绑定 
> 	派生类对象构造过程 
> 		1. 先调用的是基类的构造函数 
> 		2. 才调用派生类的构造函数 
> 2. static静态成员方法 No! virtual + static 

### 虚析构函数
#### 虚析构函数作用

```c++
class Base{
protected:
    int ma;
public:
    Base(int a=10):ma(a){}
    virtual void show() const{
        std::cout << "show: " << ma << std::endl;
    }
    virtual void show(int) const{
      std::cout << "show(int): " << ma << std::endl;
    }
    ~Base(){std::cout<< "Base destructor" << std::endl;}
};

class Derived: public Base{
private:
    int mb;
    int *ptr;
public:
    Derived(int data):Base(data), mb(data),ptr(new int(data)){}
    void show() const override{
        std::cout << "show: " << ma << ", " << mb << std::endl;
    }
    void show(int) const override{
      std::cout << "show(int): " << ma << ", " << mb << std::endl;
    }
    ~Derived(){
      delete ptr;
      std::cout<< "Derived destructor" << std::endl;
    }
};

int main() {
  Base *pb=new Derived(10);
  pb->show();
  delete pb; // 派生类的析构函数没有被调用
  return 0;
}
```

- 这里派生类的析构函数没有调用，发生内存泄露
- `pb->Base Base::~Base` 对于析构函数的调用，就是静态绑定了 `call Base::~Base` 

虚析构函数是为了解决基类的指针指向派生类对象,并用<font color=#ff0000>基类的指针删除派生类对象</font>。
由于有虚析构函数会首先调用派生类worker的析构函数,然后再调用基类People的析构函数
```c++
class Base{
protected:
    int ma;
public:
    Base(int a=10):ma(a){}
    virtual void show() const{
        std::cout << "show: " << ma << std::endl;
    }
    virtual void show(int) const{
      std::cout << "show(int): " << ma << std::endl;
    }
    virtual ~Base(){std::cout<< "Base destructor" << std::endl;}
};
```

- `pb->Base Base::~Base` 对于析构函数的调用，就是动态绑定了 
- `pb->Derive Derive vftable &Derive::~Derive` 


> 什么时候把基类的析构函数必须实现成虚函数？ 
> 基类的指针（引用）**指向堆**上new出来的派生类对象的时候，delete pb(基类的指针） 
> 它调用析构函数的时候，必须发生动态绑定，否则会导致派生类的析构函数无法调用 

#### 纯虚析构函数
纯虚析构函数在c++中是合法的,但是在使用的时候有一个额外的限制：必须为纯虚析构函数提供一个函数体。
那么问题是：如果给虚析构函数提供函数体了,那怎么还能称作纯虚析构函数呢？
纯虚析构函数和非纯析构函数之间唯一的不同之处在于纯虚析构函数使得基类是抽象类,不能创建基类的对象。
```c++
//非纯虚析构函数
class A{
public:
	virtual ~A();
};

A::~A(){}

//纯析构函数
class B{
public:
	virtual ~B() = 0;
};

B::~B(){}

void test(){
	A a; //A类不是抽象类,可以实例化对象
	B b; //B类是抽象类,不可以实例化对象
}
```

如果类的目的不是为了实现多态,作为基类来使用,就不要声明虚析构函数,反之,则应该为类声明虚析构函数。

### 多态基本概念

多态性(polymorphism)提供接口与具体实现之间的另一层隔离,从而将”what”和”how”分离开来。
#### 多态的类别
- 静态(编译时期)的多态：函数重载、模板（函数模板和类模板）
	```c++
	bool compare(int,int){} 
	bool compare (double, double){} 
	compare(10,20); // call compare_int_int在编译阶段就确定好调用的函数版本 
	compare(10.5,20.5); // call compare_douoble_double 在编译阶段就确定好调用的函数 
	
	template<typename T>
	bool compare(T a, T b){} 
	
	compare(10,20); // =>int 实例化一个compare<int> 
	compare(10.5,20.5); // =>double 实例化一个compare<double> 
	```

- 动态(运行时期)的多态：
	- 在继承结构中，基类**指针（引用)** 指向派生类对象，通过该指针（引用）调用同名覆盖方法（虚函数），基类指针指向哪个派生类对象，就会调用哪个派生类对象的同名覆盖方法，称为多态 `pbase->show()`; 
	- 多态底层是通过[[C++#动态绑定|动态绑定]]来实现的，`pbase-` 访问谁的`vfptr=》` 继续访问谁的`vftable =》`当然调用的是对应的派生类对象的方法了 

```c
//计算器
class Caculator{
public:
	void setA(int a){
		this->mA = a;
	}
	void setB(int b){
		this->mB = b;
	}
	void setOperator(string oper){
		this->mOperator = oper;
	}
	int getResult(){
		
		if (this->mOperator == "+"){
			return mA + mB;
		}
		else if (this->mOperator == "-"){
			return mA - mB;
		}
		else if (this->mOperator == "*"){
			return mA * mB;
		}
		else if (this->mOperator == "/"){
			return mA / mB;
		}
	}
private:
	int mA;
	int mB;
	string mOperator;
};

//这种程序不利于扩展,维护困难,如果修改功能或者扩展功能需要在源代码基础上修改
//面向对象程序设计一个基本原则:开闭原则(对修改关闭,对扩展开放)

//抽象基类
class AbstractCaculator{
public:
	void setA(int a){
		this->mA = a;
	}
	virtual void setB(int b){
		this->mB = b;
	}
	virtual int getResult() = 0;
protected:
	int mA;
	int mB;
	string mOperator;
};

//加法计算器
class PlusCaculator : public AbstractCaculator{
public:
	virtual int getResult(){
		return mA + mB;
	}
};

//减法计算器
class MinusCaculator : public AbstractCaculator{
public:
	virtual int getResult(){
		return mA - mB;
	}
};

//乘法计算器
class MultipliesCaculator : public AbstractCaculator{
public:
	virtual int getResult(){
		return mA * mB;
	}
};

void DoBussiness(AbstractCaculator* caculator){
	int a = 10;
	int b = 20;
	caculator->setA(a);
	caculator->setB(b);
	cout << "计算结果：" << caculator->getResult() << endl;
	delete caculator;
}
```

> 多态性改善了代码的<font color=#ff0000>可读性和组织性</font>,同时也使创建的程序具有可扩展性,项目不仅在最初创建时期可以扩展,而且当项目在需要有新的功能时也能扩展。

 
### 抽象类和纯虚函数(pure virtual function)

在设计时,常常希望基类仅仅作为其派生类的一个接口。这就是说,仅想对基类进行向上类型转换,使用它的接口,而不希望用户实际的创建一个基类的对象。同时创建一个纯虚函数允许接口中放置成员原函数,而不一定要提供一段可能对这个函数毫无意义的代码。
 
做到这点,可以在基类中加入**至少一个纯虚函数**(pure virtual function),使得基类称为抽象类(abstract class).
- 纯虚函数使用关键字virtual,并在其后面加上=0。如果试图去实例化一个抽象类,编译器则会阻止这种操作。
- 当继承一个抽象类的时候,必须实现所有的纯虚函数,否则由抽象类派生的类也是一个抽象类。
- Virtual void fun() = 0;告诉编译器在vtable中为函数保留一个位置,但在这个特定位置不放地址。

> 建立公共接口目的是为了将子类公共的操作抽象出来,可以通过一个公共接口来操纵一组类,且这个公共接口不需要事先(或者不需要完全实现)。可以创建一个公共类.

案例: 模板方法模式

<img src="https://article.biliimg.com/bfs/article/d3825d254f72887b01f9b213f6d7547b38716159.png" alt="image.png" style="zoom:50%;" />


```c++
//抽象制作饮品
class AbstractDrinking{
public:
	//烧水
	virtual void Boil() = 0;
	//冲泡
	virtual void Brew() = 0;
	//倒入杯中
	virtual void PourInCup() = 0;
	//加入辅料
	virtual void PutSomething() = 0;
	//规定流程
	void MakeDrink(){
		Boil();
		Brew();
		PourInCup();
		PutSomething();
	}
};

//制作咖啡
class Coffee : public AbstractDrinking{
public:
	//烧水
	virtual void Boil(){
		cout << "煮农夫山泉!" << endl;
	}
	//冲泡
	virtual void Brew(){
		cout << "冲泡咖啡!" << endl;
	}
	//倒入杯中
	virtual void PourInCup(){
		cout << "将咖啡倒入杯中!" << endl;
	}
	//加入辅料
	virtual void PutSomething(){
		cout << "加入牛奶!" << endl;
	}
};

//制作茶水
class Tea : public AbstractDrinking{
public:
	//烧水
	virtual void Boil(){
		cout << "煮自来水!" << endl;
	}
	//冲泡
	virtual void Brew(){
		cout << "冲泡茶叶!" << endl;
	}
	//倒入杯中
	virtual void PourInCup(){
		cout << "将茶水倒入杯中!" << endl;
	}
	//加入辅料
	virtual void PutSomething(){
		cout << "加入食盐!" << endl;
	}
};

//业务函数
void DoBussiness(AbstractDrinking* drink){
	drink->MakeDrink();
	delete drink;
}

void test(){
	DoBussiness(new Coffee);
	cout << "--------------" << endl;
	DoBussiness(new Tea);
}
```

### 纯虚函数和多继承
多继承带来了一些争议,但是接口继承可以说一种毫无争议的运用了。
绝大数面向对象语言都不支持多继承,但是绝大数面向对象对象语言都支持接口的概念,c++中没有接口的概念,但是可以通过纯虚函数实现接口。

<font color=#ff0000>接口类中只有函数原型定义,没有任何数据定义。</font>

多重继承接口不会带来二义性和复杂性问题。接口类只是一个功能声明,并不是功能实现,子类需要根据功能说明定义功能实现。

<font color=#ff0000>注意:除了析构函数外,其他声明都是纯虚函数。</font>
	
### 重写 重载 重定义
- 重载,<font color=#ff0000>同一作用域的同名函数</font>
	1.	同一个作用域
	2.	参数个数,参数顺序,参数类型不同
	3.	和函数返回值,没有关系
	4.	const也可以作为重载条件  //`do(const Teacher& t){}  do(Teacher& t)`
- 重定义（隐藏）
	1.	有继承
	2.	子类（派生类）重新定义父类（基类）的同名成员（非virtual函数）
- 重写（覆盖）
	1.	有继承
	2.	子类（派生类）重写父类（基类）的virtual函数
	3.	函数返回值,函数名字,函数参数,必须和基类中的虚函数一致

```c++
class A{
public:
	//同一作用域下,func1函数重载
	void func1(){}
	void func1(int a){}
	void func1(int a,int b){}
	void func2(){}
	virtual void func3(){}
};

class B : public A{
public:
	//重定义基类的func2,隐藏了基类的func2方法
	void func2(){}
	//重写基类的func3函数,也可以覆盖基类func3
	virtual void func3(){}
};
```


# 模板元编程
## 模板概论

c++提供了函数模板(function template.)所谓函数模板,<font color=#ff0000>实际上是建立一个通用函数,其函数类型和形参类型不具体制定,用一个虚拟的类型来代表。这个通用函数就成为函数模板。凡是函数体相同的函数都可以用这个模板代替</font>,不必定义多个函数,只需在模板中定义一次即可。在调用函数时系统会根据实参的类型来取代模板中的虚拟类型,从而实现不同函数的功能。
- c++提供两种模板机制:函数模板和类模板
- 类属 - 类型参数化,又称参数模板
总结：
- 模板把函数或类要处理的数据类型参数化,表现为参数的多态性,成为类属。
- 模板用于表达逻辑结构相同,但具体数据元素类型不同的数据对象的通用行为。
## 函数模板
### 什么是函数模板？
```c++
//交换int数据
void SwapInt(int& a,int& b){
	int temp = a;
	a = b;
	b = temp;
}

//交换char数据
void SwapChar(char& a,char& b){
	char temp = a;
	a = b;
	b = temp;
}
//问题：如果我要交换double类型数据,那么还需要些一个double类型数据交换的函数
//繁琐,写的函数越多,当交换逻辑发生变化的时候,所有的函数都需要修改,无形当中增加了代码的维护难度

//如果能把类型作为参数传递进来就好了,传递int就是Int类型交换,传递char就是char类型交换
//我们有一种技术,可以实现类型的参数化---函数模板


//class 和 typename都是一样的,用哪个都可以
template<class T>
void MySwap(T& a,T& b){
	T temp = a;
	a = b;
	b = temp;
}

void test01(){
	
	int a = 10;
	int b = 20;
	cout << "a:" << a << " b:" << b << endl;
	//1. 这里有个需要注意点,函数模板可以自动推导参数的类型
	MySwap(a,b);
	cout << "a:" << a << " b:" << b << endl;

	char c1 = 'a';
	char c2 = 'b';
	cout << "c1:" << c1 << " c2:" << c2 << endl;
	//2. 函数模板可以自动类型推导,那么也可以显式指定类型
	MySwap<char>(c1, c2);
	cout << "c1:" << c1 << " c2:" << c2 << endl;
}
```


用模板是为了实现泛型,可以减轻编程的工作量,增强函数的重用性。
**课堂练习**
使用函数模板实现对char和int类型数组进行排序？

```c++
//模板打印函数
template<class T>
void PrintArray(T arr[],int len){
	for (int i = 0; i < len;i++){
		cout << arr[i] << " ";
	}
	cout << endl;
}

//模板排序函数
template<class T>
void MySort(T arr[],int len){
	
	for (int i = 0; i < len;i++){
		for (int j = len - 1; j > i;j--){
			if (arr[j] > arr[j - 1]){
				T temp = arr[j - 1];
				arr[j - 1] = arr[j];
				arr[j] = temp;
			}
		}
	}

}

void test(){
	
	//char数组
	char tempChar[] = "aojtifysn";
	int charLen = strlen(tempChar);

	//int数组
	int tempInt[] = {7,4,2,9,8,1};
	int intLen = sizeof(tempInt) / sizeof(int);

	//排序前 打印函数
	PrintArray(tempChar, charLen);
	PrintArray(tempInt, intLen);
	//排序
	MySort(tempChar, charLen);
	MySort(tempInt, intLen);
	//排序后打印
	PrintArray(tempChar, charLen);
	PrintArray(tempInt, intLen);
}
```


> 模板的非类型参数都是常量，且必须是整数类型(整数或者地址/引用都可以) 只能使用，而不能修改

## 函数模板和普通函数区别
- 函数模板不允许自动类型转化
- 普通函数能够自动进行类型转化


```c++
//函数模板
template<class T>
T MyPlus(T a, T b){
	T ret = a + b;
	return ret;
}

//普通函数
int MyPlus(int a,char b){
	int ret = a + b;
	return ret;
}

void test02(){

	int a = 10;
	char b = 'a';

	//调用函数模板,严格匹配类型
	MyPlus(a, a);
	MyPlus(b, b);
	//调用普通函数
	MyPlus(a, b);
	//调用普通函数  普通函数可以隐式类型转换
	MyPlus(b, a);

	//结论：
	//函数模板不允许自动类型转换,必须严格匹配类型
	//普通函数可以进行自动类型转换
}
```

## 函数模板和普通函数在一起调用规则

- c++编译器优先考虑普通函数
- 可以通过空模板实参列表的语法限定编译器只能通过模板匹配
- 函数模板可以像普通函数那样可以被重载
- 如果函数模板可以产生一个更好的匹配,那么选择模板


```c++
//函数模板
template<class T>
T MyPlus(T a, T b){
	T ret = a + b;
	return ret;
}

//普通函数
int MyPlus(int a, int b){
	int ret = a + b;
	return ret;
}

void test03(){
	int a = 10;
	int b = 20;
	char c = 'a';
	char d = 'b';
	//如果函数模板和普通函数都能匹配,c++编译器优先考虑普通函数
	cout << MyPlus(a, b) << endl;
	//如果我必须要调用函数模板,那么怎么办? //在函数名后加<>
	cout << MyPlus<>(a, b) << endl;
	//此时普通函数也可以匹配,因为普通函数可以自动类型转换
	//但是此时函数模板能够有更好的匹配
	//如果函数模板可以产生一个更好的匹配,那么选择模板
	cout << MyPlus(c,d);
}

//函数模板重载
template<class T>
T MyPlus(T a, T b, T c){
	T ret = a + b + c;
	return ret;
}

void test04(){

	int a = 10;
	int b = 20;
	int c = 30;
	cout << MyPlus(a, b, c) << endl;
	//如果函数模板和普通函数都能匹配,c++编译器优先考虑普通函数
}
```

## 模板机制剖析
思考:为什么函数模板可以和普通函数放在一起?c++编译器是如何实现函数模板机制的？
### 编译过程
hello.cpp程序是高级c语言程序,这种程序易于被人读懂。为了在系统上运行hello.c程序,每一条c语句都必须转化为低级的机器指令。然后将这些机器指令打包成可执行目标文件格式,并以二进制形式存储于磁盘中。
预处理(Pre-processing) -> 编译(Compiling) ->汇编(Assembling) -> 链接(Linking)
 
<img src="https://article.biliimg.com/bfs/article/4835f02adee3245129d6362b77dcf34f38716159.png" alt="image.png" style="zoom:60%;" />


### 模板实现机制
函数模板机制结论：
- 编译器并不是把函数模板处理成能够处理任何类型的函数
- 函数模板通过具体类型产生不同的函数
- 编译器会对函数模板进行<font color=#ff0000>两次编译</font>,在声明的地方对模板代码本身进行编译,在调用的地方对参数替换后的代码进行编译。

## 模板的局限性
假设有如下模板函数：
```c++
	template<class T>
	void f(T a, T b)
	{ … }
```

如果代码实现时定义了赋值操作 a = b,但是T为数组,这种假设就不成立了
同样,如果里面的语句为判断语句 if(a>b),但T如果是结构体,该假设也不成立,另外如果是传入的数组,数组名为地址,因此它比较的是地址,而这也不是我们所希望的操作。
总之,编写的模板函数**很可能无法处理某些类型**,另一方面,有时候通用化是有意义的,但C++语法不允许这样做。为了解决这种问题,可以提供模板的重载,为这些特定的类型<font color=#ff0000>提供具体化的模板</font>。

```c++
class Person
{
public:
	Person(string name, int age)
	{
		this->mName = name;
		this->mAge = age;
	}
	string mName;
	int mAge;
};

//普通交换函数
template <class T>
void mySwap(T &a,T &b)
{
	T temp = a;
	a = b; 
	b = temp;
}
//第三代具体化,显示具体化的原型和定意思以template<>开头,并通过名称来指出类型
//具体化优先于常规模板
template<>
void  mySwap<Person>(Person &p1, Person &p2)
{
	string nameTemp;
	int ageTemp;

	nameTemp = p1.mName;
	p1.mName = p2.mName;
	p2.mName = nameTemp;

	ageTemp = p1.mAge;
	p1.mAge = p2.mAge;
	p2.mAge = ageTemp;

}

void test()
{
	Person P1("Tom", 10);
	Person P2("Jerry", 20);

	cout << "P1 Name = " << P1.mName << " P1 Age = " << P1.mAge << endl;
	cout << "P2 Name = " << P2.mName << " P2 Age = " << P2.mAge << endl;
	mySwap(P1, P2);
	cout << "P1 Name = " << P1.mName << " P1 Age = " << P1.mAge << endl;
	cout << "P2 Name = " << P2.mName << " P2 Age = " << P2.mAge << endl;
}

```


## 类模板
### 类模板基本概念
类模板和函数模板的定义和使用类似,我们已经进行了介绍。有时,有两个或多个类,其功能是相同的,仅仅是数据类型不同。
- 类模板用于实现类所需数据的类型参数化

```c++
template<class NameType, class AgeType>
class Person
{
public:
	Person(NameType name, AgeType age)
	{
		this->mName = name;
		this->mAge = age;
	}
	void showPerson()
	{
		cout << "name: " << this->mName << " age: " << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};

void test01()
{
	//Person P1("德玛西亚",18); // 类模板不能进行类型自动推导 
	Person<string, int>P1("德玛西亚", 18);
	P1.showPerson();
}
```

### 类模板做函数参数
```c++
//类模板
template<class NameType, class AgeType>
class Person{
public:
	Person(NameType name, AgeType age){
		this->mName = name;
		this->mAge = age;
	}
	void PrintPerson(){
		cout << "Name:" << this->mName << " Age:" << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};

//类模板做函数参数
void DoBussiness(Person<string,int>& p){
	p.mAge += 20;
	p.mName += "_vip";
	p.PrintPerson();
}

int main(){

	Person<string, int> p("John", 30);
	DoBussiness(p);

	system("pause");
	return EXIT_SUCCESS;
}

```

### 类模板派生普通类
```c++
//类模板
template<class T>
class MyClass{
public:
	MyClass(T property){
		this->mProperty = property;
	}
public:
	T mProperty;
};

//子类实例化的时候需要具体化的父类,子类需要知道父类的具体类型是什么样的
//这样c++编译器才能知道给子类分配多少内存

//普通派生类
class SubClass : public MyClass<int>{
public:
	SubClass(int b) : MyClass<int>(20){
		this->mB = b;
	}
public:
	int mB;
};
```

### 类模板派生类模板
```c++
//父类类模板
template<class T>
class Base
{
	T m;
};
template<class T>
class Child2 : public Base<double>  //继承类模板的时候,必须要确定基类的大小
{
public:
	T mParam;
};

void test02()
{
	Child2<int> d2;
}
```


### 类模板类内实现
```c++
template<class NameType, class AgeType>
class Person
{
public:
	Person(NameType name, AgeType age)
	{
		this->mName = name;
		this->mAge = age;
	}
	void showPerson()
	{
		cout << "name: " << this->mName << " age: " << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};

void test01()
{
	//Person P1("德玛西亚",18); // 类模板不能进行类型自动推导 新版的编译器可以了
	Person<string, int>P1("德玛西亚", 18);
	P1.showPerson();
}
```

### 类模板类外实现
```c++
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<string>
using namespace std;

template<class T1, class T2>
class Person{
public:
	Person(T1 name, T2 age);
	void showPerson();

public:
	T1 mName;
	T2 mAge;
};


//类外实现
template<class T1, class T2>
Person<T1, T2>::Person(T1 name, T2 age){
	this->mName = name;
	this->mAge = age;
}


template<class T1, class T2>
void Person<T1, T2>::showPerson(){
	cout << "Name:" << this->mName << " Age:" << this->mAge << endl;
}

void test()
{
	Person<string, int> p("Obama", 20);
	p.showPerson();
}

int main(){

	test();

	system("pause");
	return EXIT_SUCCESS;
}
```


### 模板类碰到友元函数

```c++
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
using namespace std;
#include <string>

template<class T1, class T2> class Person;
//告诉编译器这个函数模板是存在
template<class T1, class T2> void PrintPerson2(Person<T1, T2>& p);

//友元函数在类内实现
template<class T1, class T2>
class Person{
	//1. 友元函数在类内实现
	friend void PrintPerson(Person<T1, T2>& p){
		cout << "Name:" << p.mName << " Age:" << p.mAge << endl;
	}

	//2.友元函数类外实现
	//告诉编译器这个函数模板是存在
	friend void PrintPerson2<>(Person<T1, T2>& p);

	//3. 类模板碰到友元函数模板
	template<class U1, class U2>
	friend void PrintPerson(Person<U1, U2>& p);

public:
	Person(T1 name, T2 age){
		this->mName = name;
		this->mAge = age;
	}
	void showPerson(){
		cout << "Name:" << this->mName << " Age:" << this->mAge << endl;
	}
private:
	T1 mName;
	T2 mAge;
};

void test01()
{
	Person <string, int>p("Jerry", 20);
	PrintPerson(p);
}


// 类模板碰到友元函数
//友元函数类外实现  加上<>空参数列表,告诉编译去匹配函数模板
template<class T1 , class T2>
void PrintPerson2(Person<T1, T2>& p)
{
	cout << "Name2:" << p.mName << " Age2:" << p.mAge << endl;
}

void  test02()
{
	Person <string, int>p("Jerry", 20);
	PrintPerson2(p);   //不写可以编译通过,写了之后,会找PrintPerson2的普通函数调用,因为写了普通函数PrintPerson2的声明	
}

int main(){

	//test01();
	test02();
	system("pause");
	return EXIT_SUCCESS;
}
```


### 类模板的应用
设计一个数组模板类(MyArray),完成对不同类型元素的管理
```c++
void test();
using namespace std;
template<class T>
class MyArray {
    template<class T1>
    friend ostream &operator<<(ostream &os, const MyArray<T1> &array);
public:
    explicit MyArray(int capacity) : capacity(capacity) {
        this->size = 0;
        this->address = new T[this->capacity];
    }
    // 拷贝构造
    MyArray(const MyArray &myArray) {
        this->capacity = myArray.capacity;
        this->size = myArray.size;
        this->address = new T[this->capacity];
        for (int i = 0; i < this->size; ++i) {
            this->address[i] = myArray.address[i];
        }
    }
    T &operator[](int index) {
        return this->address[index];
    }
    void push_back(const T &val) {
        if (this->size >= this->capacity) {
            return;
        }
        this->address[this->size] = val;
        this->size++;
    }
    void pop_back() {
        if (this->size == 0) {
            return;
        }
        this->size--;
    }
    int getSize() {
        return this->size;
    };
    virtual ~MyArray() {
        if (this->address != NULL) {
            delete[] this->address;
            this->address = NULL;
            this->size = 0;
            this->capacity = 0;
        }
    }
private:
    T *address; //指向堆的空间
    int capacity{};
    int size{};
};
template<class T1>
ostream &operator<<(ostream &os, const MyArray<T1> &array) {
    for (int i = 0; i < array.size; ++i) {
        os << array.address[i];
    }
    os << endl;
    return os;
}
class Person {
    friend ostream &operator<<(ostream &os, const Person &person) {
        os << "Name: " << person.mName << ", Age: " << person.mAge;
        return os;
    }
public:
    Person() = default;
    Person(string name, int age) {
        this->mName = std::move(name);
        this->mAge = age;
    }
public:
    string mName;
    int mAge{};
};
int main() {

    test();
    return EXIT_SUCCESS;
}
void test() {
    MyArray<Person> myArrayPerson(5);
    Person p1("德玛西亚", 30);
    Person p2("提莫", 20);
    Person p3("孙悟空", 18);
    Person p4("赵信", 15);
    Person p5("赵云", 24);
    myArrayPerson.push_back(p1);
    myArrayPerson.push_back(p2);
    myArrayPerson.push_back(p3);
    myArrayPerson.push_back(p4);
    myArrayPerson.push_back(p5);
    cout << myArrayPerson<<endl;
    cout<<myArrayPerson[2]<<endl;
    cout<<myArrayPerson.getSize();
}
```

## 模板的优化

### 默认模板参数

在C++98/03标准中,类模板可以有默认的模板参数：

```c++
#include <iostream>
using namespace std;
template <typename T=int, T t=520>
class Test
{
public:
    void print()
    {
        cout << "current value: " << t << endl;
    }
};
int main()
{
    Test<> t;
    t.print();

    Test<int, 1024> t1;
    t1.print();

    return 0;
}
```

但是不支持函数的默认模板参数,在C++11中添加了对函数模板默认参数的支持:
```c++
#include <iostream>
using namespace std;

template <typename T=int>	// C++98/03不支持这种写法, C++11中支持这种写法
void func(T t)
{
    cout << "current value: " << t << endl;
}

int main()
{
    func(100);
    return 0;
}
```

当所有模板参数都有默认参数时,函数模板的调用如同一个普通函数。但对于类模板而言,哪怕所有参数都有默认参数,在使用时也必须在模板名后跟随`<>`来实例化。

另外：函数模板的默认模板参数在使用规则上和其他的默认参数也有一些不同,它没有必须写在参数表最后的限制。这样当默认模板参数和模板参数自动推导结合起来时,书写就显得非常灵活了。我们可以指定函数模板中的一部分模板参数使用默认参数,另一部分使用自动推导,比如下面的例子：
```c++
#include <iostream>
#include <string>
using namespace std;

template <typename R = int, typename N>
R func(N arg)
{
    return arg;
}

int main()
{
    auto ret1 = func(520);
    cout << "return value-1: " << ret1 << endl;

    auto ret2 = func<double>(52.134);
    cout << "return value-2: " << ret2 << endl;

    auto ret3 = func<int>(52.134);
    cout << "return value-3: " << ret3 << endl;

    auto ret4 = func<char, int>(100);
    cout << "return value-4: " << ret4 << endl;

    return 0;
}
	/* 
	return value-1: 520
	return value-2: 52.134
	return value-3: 52
	return value-4: d
	*/
```


当默认模板参数和模板参数自动推导同时使用时（优先级从高到低）：
- 如果可以推导出参数类型则使用推导出的类型
- 如果函数模板无法推导出参数类型,那么编译器会使用默认模板参数
- 如果无法推导出模板参数类型并且没有设置默认模板参数,编译器就会报错。

看一下下面的例子：
```c++
#include <iostream>
#include <string>
using namespace std;
// 函数模板定义
template <typename T, typename U = char>
void func(T arg1 = 100, U arg2 = 100)
{
    cout << "arg1: " << arg1 << ", arg2: " << arg2 << endl;
}
int main()
{
    // 模板函数调用
    func('a');
    func(97, 'a');
    // func();    //编译报错
    return 0;
}
```

程序输出的结果为:
```c++
arg1: a, arg2: d
arg1: 97, arg2: a
```

分析一下调用的模板函数`func()`：

- `func('a')`：参数T被自动推导为char类型,U使用的默认模板参数为char类型
- `func(97, 'a')`;：参数T被自动推导为int类型,U使用推导出的类型为char
- `func()`;：参数T没有指定默认模板类型,并且无法自动推导,编译器会直接报错
	- 模板参数类型的自动推导是根据模板函数调用时指定的实参进行推断的,没有实参则无法推导
	- 模板参数类型的自动推导不会参考函数模板中指定的默认参数。

### 模板的别名

1. **为容器定义别名**：

假设你经常使用`std::vector`容器存储整数,你可以为它定义一个别名,使其使用更为方便：

```c++
template<typename T>
using Vec = std::vector<T>;
Vec<int> numbers;
```

2. **为复杂的模板类型定义别名**：

有些模板类型可能非常复杂,定义别名可以提高代码的清晰度。例如,假设你有一个映射字符串到函数的映射：
```c++
template<typename ReturnType, typename... Args>
using FuncMap = std::map<std::string, std::function<ReturnType(Args...)>>;
FuncMap<void, int, int> funcRegistry;
```

3. **为模板类的嵌套类型定义别名**：
某些类可能有嵌套类型,这些嵌套类型可能依赖于外部类的模板参数。你可以为这些嵌套类型定义别名：
```c++
template<typename T>
using MapIterator = typename std::map<int, T>::iterator;
std::map<int, double> data;
MapIterator<double> it = data.begin();
```

### 可变参数的模板

在C++11之前,类模板和函数模板只能含有固定数量的模板参数。C++11增强了模板功能,允许模板定义中包含0到任意个模板参数,这就是可变参数模板。
可变参数模板和普通模板的语义是一样的,只是写法上稍有区别,声明可变参数模板时需要在typename或class后面带上省略号“`...`”：

```c++
template<class ... T> void func(T ... args)//T叫模板参数包,args叫函数参数包
{
//可变参数模板函数
}
func();    // OK：args不含有任何实参
func(1);    // OK：args含有一个实参：int
func(2, 1.0);   // OK：args含有两个实参int和double
```

省略号“...”的作用有两个：
1. 声明一个参数包,这个参数包中可以包含0到任意个模板参数
2. 在模板定义的右边,可以将参数包展开成一个一个独立的参数
3. `sizeof...`运算符,返回参数的数目。

#### 可变参数模板函数
##### 可变参数模板函数的定义
一个可变参数模板函数的定义如下：

```c++
template<class ... T> void func(T ... args)
{//可变参数模板函数
    //sizeof...（sizeof后面有3个小点）计算变参个数
    cout << "num = " << sizeof...(args) << endl;
}

int main()
{
    func();     // num = 0
    func(1);    // num = 1
    func(2, 1.0);   // num = 2

    return 0;
}

```

##### 参数包的展开
**递归方式展开**

通过递归函数展开参数包,需要提供一个参数包展开的函数和一个递归终止函数。
```c++
//递归终止函数
void debug()
{
    cout << "empty\n";
}

//展开函数
template <class T, class ... Args>
void debug(T first, Args ... last)
{
    cout << "parameter " << first << endl;
    debug(last...);
}

int main()
{
    debug(1, 2, 3, 4);
    /*
    运行结果：
        parameter 1
        parameter 2
        parameter 3
        parameter 4
        empty
    */
    return 0;
}
```

递归调用过程如下：
```c++
debug(1, 2, 3, 4);
debug(2, 3, 4);
debug(3, 4);
debug(4);
debug();
```

**非递归方式展开**

```c++
template <class T>
void print(T arg)
{
    cout << arg << endl;
}
template <class ... Args>
void expand(Args ... args)
{
    int a[] = { (print(args), 0)... };
}
int main()
{
    expand(1, 2, 3, 4);
    return 0;
}
```

expand函数的<font color=#ff0000>逗号表达式</font>：`(print(args), 0)`, 也是按照这个执行顺序,先执行print(args),再得到逗号表达式的结果0。

逗号运算符的核心特性是：
1. 它按从左到右的顺序评估其操作数。
2. 它的结果是最右边的操作数的值。
3. 它通常用于需要在单个表达式中按顺序执行多个操作的场合。

同时,通过初始化列表来初始化一个变长数组,`{ (print(args), 0)... }`将会展开成`( (print(args1), 0), (print(args2), 0), (print(args3), 0), etc...)`, 最终会创建一个元素只都为0的数组`int a[sizeof...(args)]`。

#### 可变参数模板类

##### 继承方式展开参数包

可变参数模板类的展开一般需要定义2 ~ 3个类,包含类声明和特化的模板类：

```c++
template<typename... A> class BMW{};  // 变长模板的声明

template<typename Head, typename... Tail>  // 递归的偏特化定义
class BMW<Head, Tail...> : public BMW<Tail...>
{//当实例化对象时,则会引起基类的递归构造
public:
    BMW()
    {
        printf("type: %s\n", typeid(Head).name());
    }

    Head head;
};

template<> class BMW<>{
public:
	BMW() {std::cout << "End of inheritance chain" << std::endl;}

};  // 边界条件 这是一个模板特化,用于参数包为空的情况。这其实是递归的终止条件。当所有的类型参数都被处理后,我们最终会得到一个空的BMW模板实例。

int main()
{
    BMW<int, char, float> car;
    /*
    运行结果：
        type: f
        type: c
        type: i
    */
    return 0;
}
```

- `BMW<int, char, float>` 继承自 `BMW<char, float>`
- `BMW<char, float>` 继承自 `BMW<float>`
- `BMW<float>` 继承自 `BMW<>`

##### 模板递归和特化方式展开参数包

```c++
template <long... nums> struct Multiply;// 变长模板的声明
template <long first, long... last>
struct Multiply<first, last...> // 变长模板类
{
    static const long val = first * Multiply<last...>::val;
}; 
template<>
struct Multiply<> // 边界条件
{
    static const long val = 1;
};
int main()
{
    cout << Multiply<2, 3, 4, 5>::val << endl; // 120
    return 0;
}
```

#### index_sequence

`integer_sequence` 是 c++ 14中新增加的一个**元编程**工具,其实没有什么特殊的，就是一个类

```c++
template<typename _Tp, _Tp... _Idx>
  struct integer_sequence
  {
    typedef _Tp value_type;
    static constexpr size_t size() noexcept { return sizeof...(_Idx); }
  };

```

`index_sequence`会在编译期生成一个从0开始的序列,然后你就可以对其进行一些奇奇怪怪的操作

```c++
template <size_t... N> void print(std::index_sequence<N...>) {
  std::vector<int> res;
  (void)std::initializer_list<int>{
      ((res.push_back(N), std::cout << N << " "), 0)...};
  std::for_each(res.begin(), res.end(), [](int x) {std::cout << x << " ";});
}
int main() {
  auto t = std::make_index_sequence<10>();
  print(t);
  return 0;
}
```

比如 `((res.push_back(N), std::cout << N << " "), 0)...`这句话，就会在编译期被展开

make_index_sequence
那么，index_sequence 是如何生成的呢？

有两种形式

- 编译器内建
- 递归式的生成

第二种方式嘛还是用到了元编程的惯用伎俩，特化，递归式的编程

```c++
template <int... N> struct index_seq {};
template <int N, int... M>
struct make_index_seq : public make_index_seq<N - 1, N - 1, M...> {};
template <int... M> struct make_index_seq<0, M...> : public index_seq<M...> {};`
```

对齐使用也是一样的形式

```c++
template <int... N> void print(index_seq<N...>) {
  (void)std::initializer_list<int>{((std::cout << N << " "), 0)...};
}

int main() {
  auto r = make_index_seq<100>();
  print(r);
  return 0;
}
```

刚才，看见了print去打印的时候打印了 0-(N-1)的元素
那么，这个行为是在编译期展开的，我们就可以用到其他需要常量的地方
比如一个简单的需求：
打印tuple！

```c++
template <typename T, typename F, int...N>
void exec_for_tuple(const T& tup, F&& func, index_seq<N...>) {
  (void)std::initializer_list<int> {
      (func(std::get<N>(tup)), 0)...
  };
}
template <typename Func, typename ...Arg>
void for_tuple(Func&& func, std::tuple<Arg...> tuple) {
  exec_for_tuple(tuple, std::forward<Func>(func), make_index_seq<sizeof...(Arg)>());
}
```

exec_for_tuple部分应该非常好懂，但是为什么中间还要再转发一层呢？
因为tuple元素的个数我们不能直接获取到，我们写的又要是一个通用的函数
所以要通过 sizeof...(arg) 这种伎俩来将其元素个数计算出来
如何调用呢？
如下所示：
```c++
std::tuple<int, int, double> tuple{1, 2, 3.0};
for_tuple([](auto t) {
  std::cout << t << " ";
}, tuple);

```

##### index_sequence_for
那么，看到现在，你知道 `index_sequence_for`又是何物吗？
其实吧，刚才就已经见过 `index_sequence_for`这个东西了
其实就是计算可变长模板参数的个数，然后将其长度做成一个sequence出来

```c++
template<typename... _Types>
using index_sequence_for = make_index_seq<sizeof...(_Types)>;
```

#### fold expression（折叠表达式）

折叠表达式是C++17新引进的语法特性。使用折叠表达式可以简化对C++11中引入的参数包的处理，从而在某些情况下避免使用递归。

**语法形式**
折叠表达式共有四种语法形式。分别为一元的左折叠和右折叠，以及二元的左折叠和右折叠。

1. 一元右折叠(unary right fold)
	`( pack op ... )`
	一元右折叠`(E op ...)`展开之后变为 `E1 op (... op (EN-1 op EN))`
2. 一元左折叠(unary left fold)
	`( ... op pack )`
	一元左折叠`(... op E)`展开之后变为 `((E1 op E2) op ...) op EN`
3. 二元右折叠(binary right fold)
	`( pack op ... op init )`
	二元右折叠`(E op ... op I)`展开之后变为 `E1 op (... op (EN−1 op (EN op I)))`
4. 二元左折叠(binary left fold)
	`( init op ... op pack )`
	二元左折叠`(I op ... op E)`展开之后变为 `(((I op E1) op E2) op ...) op EN`

- 语法形式中的op代表运算符，pack代表参数包，init代表初始值。
- 初始值在右边的为右折叠，展开之后从右边开始折叠。而初始值在左边的为左折叠，展开之后从左边开始折叠。
- 不指定初始值的为一元折叠表达式，而指定初始值的为二元折叠表达式。
- 当一元折叠表达式中的参数包为空时，只有三个运算符`（&& || 以及逗号）`有缺省值，其中`&&`的缺省值为true,||的缺省值为false，逗号的缺省值为`void()`。

求和
假设我们想写一个能接受一个及以上参数的求和函数，
```c++
#include <iostream>
using namespace std;
 
template<typename First>  
First sum1(First&& value)  
{  
    return value;  
}
 
template<typename First, typename Second, typename... Rest>  
First sum1(First&& first, Second&& second, Rest&&... rest)  
{  
    return sum1(first + second, forward<Rest>(rest)...);  
}
 
template<typename First, typename... Rest>
First sum2(First&& first, Rest&&... rest)
{
    return (first + ... + rest);
}
 
int main()
{
    cout << sum1(1) << sum1(1, 2) << sum1(1, 2, 3) << endl; // 136
    cout << sum2(1) << sum2(1, 2) << sum2(1, 2, 3) << endl; // 136
}
```

- 在C++17之前，求和函数sum1的实现必须分成两个部分。
	- 其中4到8行的sum1函数用于处理一个参数的情况。而10到14行的sum1函数用于处理两个及以上参数的情况。
	- 当参数个数大于一个时，10到14行的sum1函数将前两个参数相加，然后递归调用自身。
	- 当参数个数只有一个时，4到8行的sum1函数将此参数返回，完成求和。
	- `sum1(1, 2, 3) = sum1(1+2, 3) = sum1(3, 3) = sum1(3+3) = sum1(6) = 6`
- 而在C++17之后，由于有了折叠表达式这个新特性，求和函数sum2不再需要处理特殊情况，实现大为简化。
	- `sum2(1, 2, 3) = (1 + ... + pack(2, 3)) = (1+2) + 3 = 6`
	- 这里sum2的实现所采用的是二元左折叠。

**“与”和“或”**
```c++
#include <iostream>
using namespace std;
 
template<typename... Args>
bool all(Args... args) {return (... && args);}
template<typename... Args>
bool any(Args... args) {return (... || args);}
 
int main() 
{
    cout << boolalpha << all(true, false, true) << endl; // false
    cout << boolalpha << any(true, false, true) << endl; // true
    cout << boolalpha << all() << endl; // true
    cout << boolalpha << any() << endl; // false
}
```

- 在这里all和any函数分别实现了不特定多数布尔值的与和或的运算。这两个函数的实现均采用了一元左折叠。
	- `all(true, false, true) = (... && pack(true, false, true)) = (true && false) && true = false`
	- `any(true, false, true) = (... || pack(true, false, true)) = (true || false) || true = true`
- 当一元折叠表达式中的参数包为空时，&&的缺省值为true，而||的缺省值为false。
	- `all() = (... && pack()) = true`
	- `any() = (... || pack()) = false`

**“打印”和“调用”**
```c++
#include <iostream>
using namespace std;
 
template<typename... Ts>
void printAll(Ts&&... mXs)
{
    (cout << ... << mXs) << endl;
}
 
template<typename TF, typename... Ts>
void forArgs(TF&& mFn, Ts&&... mXs)
{
    (mFn(mXs), ...);
}
 
int main() 
{
    printAll(3, 4.0, "5"); // 345
    printAll(); // 空行
    forArgs([](auto a){cout << a;}, 3, 4.0, "5"); // 345
    forArgs([](auto a){cout << a;}); // 空操作
}
```


- printAll函数实现了对不特定多数值的打印输出。该函数的实现采用了二元左折叠。
	```c++
	printAll(3, 4.0, "5")
	= (cout << ... << pack(3, 4.0, "5")) << endl
	= ((cout << 3) << 4.0) << "5" << endl
	= 打印345并换行
	```
- 当二元折叠表达式的参数包为空时，其计算结果为该二元折叠表达式中所预设的初始值。
	```c++
	printAll()
	= (cout << ... << pack()) << endl
	= cout << endl
	= 空行
	```
- forArgs函数实现了依次使用多个参数调用某个单参数函数的功能。该函数的实现采用了一元右折叠。
	```c++
	forArgs([](auto a){cout << a;}, 3, 4.0, "5")
	= ([](auto a){cout << a;}(pack(3, 4.0, "5")), ...)
	= [](auto a){cout << a;}(3), ([](auto a){cout << a;}(4.0), ([](auto a){cout << a;}("5")))
	= 打印345
	```
- 当使用逗号的一元折叠表达式中的参数包为空时，其计算结果为标准规定的缺省值void()。
	```c++
	forArgs([](auto a){cout << a;})
	= ([](auto a){cout << a;}(pack()), ...)
	= void()
	```

## 模板进阶

### SFINAE

SFINAE可以说是C++模板进阶的门槛之一，

我们不用纠结这个词的发音，它来自于 Substitution failure is not an error 的首字母缩写。

我们从最简单的词“Error”开始理解。Error就是一般意义上的编译错误。一旦出现**编译错误**，大家都知道，编译器就会中止编译，并且停止接下来的代码生成和链接等后续活动。

其次，我们再说“Failure”。很多时候光看字面意思，很多人会把Failure和Error等同起来。但是实际上Failure很多场合下只是一个**中性词**。比如我们看下面这个虚构的例子就知道这两者的区别了。

假设我们有一个语法分析器，其中某一个规则需要匹配一个token，它可以是标识符，字面量或者是字符串，那么我们会有下面的代码：

```cpp
switch(token)
{
case IDENTIFIER:
    // do something
    break;
case LITERAL_NUMBER:
    // do something
    break;
case LITERAL_STRING:
    // do something
    break;
default:
    throw WrongToken(token);
}
```

假如我们当前的token是LITERAL_STRING的时候，那么第一步，它在匹配IDENTIFIER时，我们可以认为它**failure**了。

但是如果这个Token既不是标识符，也不是数字字面量，也不是字符串字面量的时候，并且，我们的语法规则认为这一条件是无论如何都不可接受的，这时我们就认为它是一个**error**。

比如大家所熟知的函数重载，也是如此。比如说下面这个例子：
```cpp
struct A {};
struct B: public A {};
struct C {};

void foo(A const&) {}
void foo(B const&) {}

void callFoo() {
  foo( A() );
  foo( B() );
  foo( C() );
}
```

那么 foo( A() ) 虽然匹配 foo(B const&) 会失败，但是它起码能匹配 foo(A const&)，所以它是正确的； foo( B() ) 能同时匹配两个函数原型，但是B&要更好一些，因此它选择了B。而foo( C() ); 因为两个函数都匹配失败（**Failure**）了，所以它找不到相应的原型，这时才会爆出一个编译器错误（**Error**）。

所以到这里我们就明白了，**在很多情况下，** Failure is not an error，因为编译器在遇到Failure的时候，往往还需要尝试其他的可能性。

好，现在我们把最后一个词，Substitution，加入到我们的字典中。现在这句话的意思就是说，我们要把 Failure is not an error 的概念，推广到Substitution阶段。

所谓substitution，就是将函数模板中的形参，替换成实参的过程。C++标准中对这一概念的解释比较拗口，它分别指出了以下几点：

- 什么时候函数模板会发生实参 替代（Substitute） 形参的行为
- 什么样的行为被称作 Substitution  
- 什么样的行为**不**可以被称作 Substitution Failure —— 他们叫SFINAE error。

```cpp
template <
  typename T0, 
  // 一大坨其他模板参数
  typename U = /* 和前面T有关的一大坨 */
>
RType /* 和模板参数有关的一大坨 */
functionName (
   PType0 /* PType0 是和模板参数有关的一大坨 */,
   PType1 /* PType1 是和模板参数有关的一大坨 */,
   // ... 其他参数
) {
  // 实现，和模板参数有关的一大坨
}
```

那么，所有函数签名上的“和模板参数有关的一大坨”，基本都是Substitution时要处理的东西（当然也有一些例外）。一个更具体的例子来解释上面的“一大坨”：

```cpp
template <
  typename T, 
  typenname U = typename vector<T>::iterator // 1
>
typename vector<T>::value_type  // 1
  foo( 
      T*, // 1
      T&, // 1
      typename T::internal_type, // 1
      typename add_reference<T>::type, // 1
      int // 这里都不需要 substitution
  )
{
   // 整个实现部分，都没有 substitution。这个很关键。
}
```

嗯，粗糙的介绍完SFINAE之后，我们先来看一个最常见的例子看看它是什么个行为：

```cpp
struct X {
  typedef int type;
};

struct Y {
  typedef int type2;
};

template <typename T> void foo(typename T::type);    // Foo0
template <typename T> void foo(typename T::type2);   // Foo1
template <typename T> void foo(T);                   // Foo2

void callFoo() {
   foo<X>(5);    // Foo0: Succeed, Foo1: Failed,  Foo2: Failed
   foo<Y>(10);   // Foo0: Failed,  Foo1: Succeed, Foo2: Failed
   foo<int>(15); // Foo0: Failed,  Foo1: Failed,  Foo2: Succeed
}
```

在这个例子中，当我们指定 `foo<Y>` 的时候，`substitution`就开始工作了，而且会同时工作在三个不同的foo签名上。如果我们仅仅因为Y没有type，就在匹配Foo0时宣布出错，那显然是武断的，因为我们起码能保证，也希望将这个函数匹配到Foo1上。

### std::enable_if

这个模板类的实现相当的简单，看一下一个版本的实现。

```cpp
template<bool B, class T = void>
struct enable_if {};
 
template<class T>
struct enable_if<true, T> { typedef T type; };
```

一个普通版本的模板类定义，一个偏特化版本的模板类定义。它在第一个模板参数为false的时候并不会定义type，只有在第一模板参数为true的时候才会定义type。看一下下面的模板实例化代码

```c++
typename std::enable_if<true, int>::type t; //正确
typename std::enable_if<true>::type; //可以通过编译，没有实际用处，推导的模板是偏特化版本，第一模板参数是true，第二模板参数是通常版本中定义的默认类型即void
typename std::enable_if<false>::type; //无法通过编译，type类型没有定义
typename std::enable_if<false, int>::type t2; //同上
```

我们可以看到，通过`typename std::enable_if<bool>::type`这样传入一个bool值，就能推导出这个type是不是未定义的。那么这种用法有什么用途呢？结合上面的SFINAE来看代码

```c++
template <typename T>
typename std::enable_if<std::is_trivial<T>::value>::type SFINAE_test(T value)
{
    std::cout<<"T is trival"<<std::endl;
}

template <typename T>
typename std::enable_if<!std::is_trivial<T>::value>::type SFINAE_test(T value)
{
    std::cout<<"T is none trival"<<std::endl;
}
```

这两个函数如果是普通函数的话，根据重载的规则是不会通过编译的。即便是模板函数，如果这两个函数都能推导出正确的结果，也会产生重载二义性问题，但是正因为`std::enable_if`的运用使这两个函数的返回值在同一个函数调用的推导过程中只有一个合法，遵循SFINAE原则，则可以顺利通过编译。

```cpp
SFINAE_test(std::string("123"));
SFINAE_test(123);
```
  

当第一个函数调用进行模板函数推导的时候，第一个版本的模板函数`std::is_trivial<T>::value`为false，继而`std::enable_if<std::is_trivial<T>::value>::type`这个类型未定义，不能正确推导，编译器区寻找下一个可能的实现，所以接下来找到第二个模板函数，`!std::is_trivial<T>::value`的值是true，`继而std::enable_if<std::is_trivial<T>::value>::type`是void类型，推导成功。这时候`SFINAE_test(std::string("123"))`;调用有了唯一确定的推导，即第二个模板函数，所以程序打印T is none trival。与此相似的过程，第二个函数调用打印出T is trival。  
这样写的好处是什么？这个例子中可以认为我们利用SFINAE特性实现了通过不同返回值，相同函数参数进行了函数重载，这样代码看起来更统一一些。还有一些其他应用`std::enable_if`的方式，比如在模板参数列表里，在函数参数列表里，都是利用SFINAE特性来实现某一些函数的选择推导。来看一下cpprefrence上的例子代码

```cpp
#include <type_traits>
#include <iostream>
#include <string>
 
namespace detail { struct inplace_t{}; }
void* operator new(std::size_t, void* p, detail::inplace_t) {
    return p;
}
 
// enabled via the return type
template<class T,class... Args>
typename std::enable_if<std::is_trivially_constructible<T,Args&&...>::value>::type 
    construct(T* t,Args&&... args) 
{
    std::cout << "constructing trivially constructible T\n";
}
 
// enabled via a parameter
template<class T>
void destroy(T* t, 
             typename std::enable_if<std::is_trivially_destructible<T>::value>::type* = 0) 
{
    std::cout << "destroying trivially destructible T\n";
}
 
// enabled via a template parameter
template<class T,
         typename std::enable_if<
             !std::is_trivially_destructible<T>{} &&
             (std::is_class<T>{} || std::is_union<T>{}),
            int>::type = 0>
void destroy(T* t)
{
    std::cout << "destroying non-trivially destructible T\n";
    t->~T();
}
int main()
{
    std::aligned_union_t<0,int,std::string> u;
 
    construct(reinterpret_cast<int*>(&u));
    destroy(reinterpret_cast<int*>(&u));
 
    construct(reinterpret_cast<std::string*>(&u),"Hello");
    destroy(reinterpret_cast<std::string*>(&u));
}
```

可以从代码中看到，上面例子覆盖了基本所有`std::enable_if`可以应用的场景，核心就是应用SFINAE原则，来实现调用函数去推导正确的模板函数版本。

### std::true_type和std::false_type

我们先通过一个例程来认识下它吧。

```c++
#include <iostream>
using namespace std;
int main()
{
    std::true_type a;
    std::false_type b;
    std::cout << a << std::endl;
    std::cout << b << std::endl;
	return 0;
}
```

分别输出了1和0.  
接下来，再看看它的源码，其实挺简单的。

```c++
template <bool _Val>
using bool_constant = integral_constant<bool, _Val>;

using true_type  = bool_constant<true>;
using false_type = bool_constant<false>;

// STRUCT TEMPLATE integral_constant
template <class _Ty, _Ty _Val>
struct integral_constant {
    static constexpr _Ty value = _Val;

    using value_type = _Ty;
    using type       = integral_constant;
    constexpr operator value_type() const noexcept {
        return value;
    }
    _NODISCARD constexpr value_type operator()() const noexcept {
        return value;
    }
};
```

通过源码我们可以知道，其实它就是一个结构体模板。这里用到了using定义的模板，称为别名模板，这个知识点可以参考我这个系列的其它博客。

**与true和false的区别**
`true_type，false_type`：代表类型（类/结构体类型）
`true,false`：代表值
而bool既可以代表true也可以代表false。而true_type类型代表的就是true，false_type类型代表的就是false.

作用
我们知道要代表true和false用bool不就可以了吗？为什么还要整出这个玩意来，其实用处还是挺多的，一般是当作**基类被继承**，当作为基类被继承时，派生类也就具备了真或假的这种意味。比如标准库当中的`is_pointer`、`is_union`、`is_function`等类模板，都是继承自类似`BoolConstant`或者`BoolConstant`这样的基类来实现的。
下面我们先通过一个例程来认识下这个东西的用途吧。

```c++
#include <iostream>
using namespace std;
template <typename T, bool val>
struct AClass
{
    AClass() //构造函数
    {
        cout << "AClass::AClass()执行了" << endl;
        if (val)
        {
            T tmpa = 15;
        }
        else
        {
            T tmpa = "abc"; //这里有问题
        }
    }
};
int main()
{
    AClass<int, true> obj;
	return 0;
}

```

像上面这段代码大家可以知道，编译时就会报错的，
我们可以通过改成如下的方式去解决。
```c++
#include <iostream>
using namespace std;
template <typename T, bool val>
struct AClass
{
    AClass() //构造函数
    {
        cout << "AClass::AClass()执行了" << endl;
        if constexpr (val)
        {
            T tmpa = 15;
        }
        else
        {
            T tmpa = "abc";
        }
    }
};
int main()
{
    AClass<int, true> obj;
	return 0;
}

```

当然，最后我们还是要看看`std::true_type` 和`std::false_type`是怎么解决这个问题的。

```c++
#include <iostream>
using namespace std;
template <typename T, bool val>
struct AClass
{
    AClass() //构造函数
    {
        cout << "AClass::AClass()执行了" << endl;
        fun(bool_constant<val>());
    }
    void fun(std::true_type)
    {
        T tmpa = 15;
    }
    void fun(std::false_type)
    {
        T tmpa = "abc";
    }
};
int main()
{
    AClass<int, true> obj;
	return 0;
}
```

# C++类型转换

类型转换(cast)是将一种数据类型转换成另一种数据类型。例如,如果将一个整型值赋给一个浮点类型的变量,编译器会暗地里将其转换成浮点类型。
转换是非常有用的,但是它也会带来一些问题,比如在转换指针时,我们很可能将其转换成一个比它更大的类型,但这可能会破坏其他的数据。


> 无论什么原因,任何一个程序如果使用很多类型转换都值得怀疑.

## 静态转换(static_cast)

- 用于类层次结构中<font color=#ff0000>基类（父类）和派生类（子类）之间指针或引用的转换</font>。
- 进行上行转换（把派生类的指针或引用转换成基类表示）是安全的；
- 进行下行转换（把基类指针或引用转换成派生类表示）时,由于没有动态类型检查(**因为编译它不知道你实际际上指向的对象是不是派生类的实例**),所以是不安全的。可以理解为一个“动物”不一定是“狗”。
- 用于基本数据类型之间的转换,如把int转换成char,把char转换成int。这种转换的安全性也要开发人员来保证。

```c++
class Animal{};
class Dog : public Animal{};
class Other{};

//基础数据类型转换
void test01(){
	char a = 'a';
	double b = static_cast<double>(a);
}

//继承关系指针互相转换
void test02(){
	//继承关系指针转换
	Animal* animal01 = NULL;
	Dog* dog01 = NULL;
	//子类指针转成父类指针,安全
	Animal* animal02 = static_cast<Animal*>(dog01);
	//父类指针转成子类指针,不安全
	Dog* dog02 = static_cast<Dog*>(animal01);
}

//继承关系引用相互转换
void test03(){

	Animal ani_ref;
	Dog dog_ref;
	//继承关系指针转换
	Animal& animal01 = ani_ref;
	Dog& dog01 = dog_ref;
	//子类指针转成父类指针,安全
	Animal& animal02 = static_cast<Animal&>(dog01);
	//父类指针转成子类指针,不安全
	Dog& dog02 = static_cast<Dog&>(animal01);
}

//无继承关系指针转换
void test04(){
	
	Animal* animal01 = NULL;
	Other* other01 = NULL;

	//转换失败
	//Animal* animal02 = static_cast<Animal*>(other01);
}
```

## 动态转换(dynamic_cast)

- dynamic_cast主要用于类层次间的上行转换和下行转换；
- 在类层次间进行上行转换时,dynamic_cast和static_cast的效果是一样的；
- 在进行下行转换时,dynamic_cast具有类型检查的功能,比static_cast更安全；

```c++
class Animal {
public:
	virtual void ShowName() = 0;
};
class Dog : public Animal{
	virtual void ShowName(){
		cout << "I am a dog!" << endl;
	}
};
class Other {
public:
	void PrintSomething(){
		cout << "我是其他类!" << endl;
	}
};

//普通类型转换
void test01(){

	//不支持基础数据类型
	int a = 10;
	//double a = dynamic_cast<double>(a);
}

//继承关系指针
void test02(){

	Animal* animal01 = NULL;
	Dog* dog01 = new Dog;

	//子类指针转换成父类指针 可以
	Animal* animal02 = dynamic_cast<Animal*>(dog01);
	animal02->ShowName();
	//父类指针转换成子类指针 不可以
	//Dog* dog02 = dynamic_cast<Dog*>(animal01);
}

//继承关系引用
void test03(){

	Dog dog_ref;
	Dog& dog01 = dog_ref;

	//子类引用转换成父类引用 可以
	Animal& animal02 = dynamic_cast<Animal&>(dog01);
	animal02.ShowName();
}

//无继承关系指针转换
void test04(){
	
	Animal* animal01 = NULL;
	Other* other = NULL;

	//不可以
	//Animal* animal02 = dynamic_cast<Animal*>(other);
}
```

## 常量转换(const_cast)
该运算符用来修改类型的const属性,只能改变运算对象的底层const。
- 常量指针被转化成非常量指针,并且仍然指向原来的对象；
- 常量引用被转换成非常量引用,并且仍然指向原来的对象；

> <font color=#ff0000>注意</font>:不能直接对非指针和非引用的变量使用const_cast操作符去直接移除它的const.

```c++
//常量指针转换成非常量指针
void test01(){
	
	const int* p = NULL;
	int* np = const_cast<int*>(p);

	int* pp = NULL;
	const int* npp = const_cast<const int*>(pp);

	const int a = 10;  //不能对非指针或非引用进行转换
	//int b = const_cast<int>(a); }

//常量引用转换成非常量引用
void test02(){

int num = 10;
	int & refNum = num;

	const int& refNum2 = const_cast<const int&>(refNum);
	
}
```


## 重新解释转换(reinterpret_cast)。。。。。

这是最不安全的一种转换机制,是类似于C风格的强制类型转换 最有可能出问题。
主要用于将一种数据类型从一种类型转换为另一种类型。它可以将一个指针转换成一个整数,也可以将一个整数转换成一个指针.

## std::common_type_t

在 C++ 中，`std::common_type_t` 是一个类型特征，它是 `std::common_type` 模板的别名模板，用于推导两个或多个类型的共同类型。这个共同类型是所有给定类型可以隐式转换到的类型，这在模板编程中尤其有用，因为它允许你处理不同类型的值而不失去类型安全性。

**用法**
`std::common_type_t<T1, T2, ...>` 推导出一个类型，该类型是所有 `T1`, `T2`, ... 类型的公共上层类型。如果这样的类型存在，它通常是能够包含所有提供的类型值而不丢失信息的最宽松类型。

```c++
template <typename F, typename T1, typename T2, typename T = std::common_type_t<T1, T2>>
```

这里，`std::common_type_t<T1, T2>` 用于推导出 `T1` 和 `T2` 这两种类型的共同类型，并将其作为默认模板参数 `T` 的类型。这在模板函数中特别有用，特别是在执行需要统一类型的操作时（如比较、算术运算等）。

**示例**
假设你有两个不同类型的变量，比如 `int` 和 `double`，`std::common_type_t` 将会推导出它们的共同类型
```c++
int a = 5;
double b = 3.14;
std::common_type_t<decltype(a), decltype(b)> c = a + b; // c 的类型是 double
```

在这个例子中，`std::common_type_t<decltype(a), decltype(b)>` 被推导为 `double`，因为这是 `int` 和 `double` 可以隐式转换到的共同类型。


> [!attention] 注意事项
> - 如果没有共同类型，std::common_type 的实例化将导致编译错误。
> - std::common_type 通常使用标准转换规则来确定最佳类型，这可能包括标准的算术转换和用户定义的隐式转换。
> - 总之，std::common_type_t 在需要处理多种类型的泛型编程中非常有用，尤其是在需要将不同类型的值统一到一个共同类型进行操作的情况下。


## std::is_convertible

C++ STL的`std::is_convertible`模板用于检查是否可以将任何数据类型A隐式转换为任何数据类型B。它返回布尔值true或false。
头文件：
```c++
#include<type_traits>
```
模板类别：
```c++
template< class From, class To >
struct is_convertible;

template< class From, class To >
struct is_nothrow_convertible;
```
用法:
`is_convertible <A*, B*>::value;`
参数：它采用A和B两种数据类型：
- A:它代表要转换的参数。
- B:它代表参数A隐式转换的参数。
返回值：
- **True**:如果将给定的数据类型A转换为数据类型B。
- **False**:如果给定的数据类型A没有转换为数据类型B。
下面是演示C++中`std::is_convertible`的程序：

**程序:**

```c++
// C++ program to illustrate 
// std::is_convertible example 
#include <bits/stdc++.h> 
#include <type_traits> 
using namespace std; 
  
// Given classes 
class A { 
}; 
class B:public A { 
}; 
class C { 
}; 
  
// Driver Code 
int main() 
{ 
    cout << boolalpha; 
  
    // Check if class B is 
    // convertible to A or not 
    bool BtoA = is_convertible<B*, A*>::value; 
    cout << BtoA << endl; 
  
    // Check if class A is 
    // convertible to B or not 
    bool AtoB = is_convertible<A*, B*>::value; 
    cout << AtoB << endl; 
  
    // Check if class B is 
    // convertible to C or not 
    bool BtoC = is_convertible<B*, C*>::value; 
    cout << BtoC << endl; 
  
    // Check if int is convertible 
    // to float or not 
    cout << "int to float:"
         << is_convertible<int, float>::value 
         << endl; 
  
    // Check if int is convertible 
    // to const int or not 
    cout << "int to const int:"
         << is_convertible<int, const int>::value 
         << endl; 
  
    return 0; 
}
```


输出：
```txt
true
false
false
int to float:true
int to const int:true
```

# C++异常
## 异常基本概念

Bjarne Stroustrup说：提供异常的基本目的就是为了处理上面的问题。基本思想是：让一个函数在发现了自己无法处理的错误时抛出（throw）一个异常,然后它的（直接或者间接）调用者能够处理这个问题。也就是《C++ primer》中说的：将问题检测和问题处理相分离。 
一种思想：在所有支持异常处理的编程语言中（例如java）,要认识到的一个思想：在异常处理过程中,由问题检测代码可以抛出一个对象给问题处理代码,通过这个对象的类型和内容,实际上完成了两个部分的通信,通信的内容是“出现了什么错误”。当然,各种语言对异常的具体实现有着或多或少的区别,但是这个通信的思想是不变的。

<font color=#ff0000>一句话：异常处理就是处理程序中的错误。所谓错误是指在程序运行的过程中发生的一些异常事件（如：除0溢出,数组下标越界,所要读取的文件不存在,空指针,内存不足等等）。</font>

**回顾一下：我们以前编写程序是如何处理异常**？
在C语言的世界中,对错误的处理总是围绕着两种方法：一是使用整型的返回值标识错误；二是使用errno宏（可以简单的理解为一个全局整型变量）去记录错误。当然C++中仍然是可以用这两种方法的。
	这两种方法最大的缺陷就是会出现不一致问题。例如有些函数返回1表示成功,返回0表示出错；而有些函数返回0表示成功,返回非0表示出错。  
	还有一个缺点就是函数的返回值只有一个,你通过函数的返回值表示错误代码,那么函数就不能返回其他的值。当然,你也可以通过指针或者C++的引用来返回另外的值,但是这样可能会令你的程序略微晦涩难懂。

**c++异常机制相比C语言异常处理的优势**?
- 函数的返回值可以忽略,但异常不可忽略。如果程序出现异常,但是没有被捕获,程序就会终止,这多少会促使程序员开发出来的程序更健壮一点。而如果使用C语言的error宏或者函数返回值,调用者都有可能忘记检查,从而没有对错误进行处理,结果造成程序莫名其面的终止或出现错误的结果。
- 整型返回值没有任何语义信息。而异常却包含语义信息,有时你从类名就能够体现出来。
- 整型返回值缺乏相关的上下文信息。异常作为一个类,可以拥有自己的成员,这些成员就可以传递足够的信息。
- 异常处理可以在调用跳级。这是一个代码编写时的问题：假设在有多个函数的调用栈中出现了某个错误,使用整型返回码要求你在每一级函数中都要进行处理。而使用异常处理的栈展开机制,只需要在一处进行处理就可以了,不需要每级函数都处理。

```c++
//如果判断返回值,那么返回值是错误码还是结果？
//如果不判断返回值,那么b==0时候,程序结果已经不正确
//A写的代码
int A_MyDivide(int a,int b){
	if (b == 0){
		return -1;
	}

	return a / b;
}

//B写的代码
int B_MyDivide(int a,int b){

	int ba = a + 100;
	int bb = b;

	int ret = A_MyDivide(ba, bb);  //由于B没有处理异常,导致B结果运算错误

	return ret;
}

//C写的代码
int C_MyDivide(){

	int a = 10;
	int b = 0;

	int ret = B_MyDivide(a, b); //更严重的是,由于B没有继续抛出异常,导致C的代码没有办法捕获异常
	if (ret == -1){
		return -1;
	}
	else{
		return ret;
	}
}

//所以,我们希望：
//1.异常应该捕获,如果你捕获,可以,那么异常必须继续抛给上层函数,你不处理,不代表你的上层不处理
//2.这个例子,异常没有捕获的结果就是运行结果错的一塌糊涂,结果未知,未知的结果程序没有必要执行下去
```

## 异常语法
### 异常基本语法
```c++
int A_MyDivide(int a, int b){
	if (b == 0){
		throw 0;
	}

	return a / b;
}

//B写的代码 B写代码比较粗心,忘记处理异常
int B_MyDivide(int a, int b){

	int ba = a;
	int bb = b;

	int ret = A_MyDivide(ba, bb) + 100;  //由于B没有处理异常,导致B结果运算错误

	return ret;
}

//C写的代码
int C_MyDivide(){

	int a = 10;
	int b = 0;

	int ret = 0;

//没有处理异常,程序直接中断执行
#if 1 
	ret = B_MyDivide(a, b);

//处理异常
#else 
	try{
		ret = B_MyDivide(a, b); //更严重的是,由于B没有继续抛出异常,导致C的代码没有办法捕获异常
	}
	catch (int e){
		cout << "C_MyDivide Call B_MyDivide 除数为:" << e << endl;
	}
#endif
	
	return ret;
}

int main(){

	C_MyDivide();
	system("pause");
	return EXIT_SUCCESS;
}
```

**总结**:

- 若有异常则通过throw操作创建一个异常对象并抛出。
- 将可能抛出异常的程序段放到try块之中。
- 如果在try段执行期间没有引起异常,那么跟在try后面的catch字句就不会执行。
- catch子句会根据出现的先后顺序被检查,匹配的catch语句捕获并处理异常(或继续抛出异常)
- 如果匹配的处理未找到,则运行函数terminate将自动被调用,其缺省功能调用abort终止程序。
- 处理不了的异常,可以在catch的最后一个分支,使用throw,向上抛。

c++异常处理使得异常的引发和异常的处理不必在一个函数中,这样底层的函数可以着重解决具体问题,而不必过多的考虑异常的处理。上层调用者可以在适当的位置设计对不同类型异常的处理。
### 异常严格类型匹配
异常机制和函数机制互不干涉,但是<font color=#ff0000>捕捉方式是通过严格类型匹配</font>。

```c++
void TestFunction(){
	cout << "开始抛出异常..." << endl;
	//throw 10; //抛出int类型异常
	//throw 'a'; //抛出char类型异常
	//throw "abcd"; //抛出char*类型异常
	string ex = "string exception!";
	throw ex;
}
int main(){
	try{
		TestFunction();
	}
	catch (int){
		cout << "抛出Int类型异常!" << endl;
	}
	catch (char){
		cout << "抛出Char类型异常!" << endl;
	}
	catch (char*){
		cout << "抛出Char*类型异常!" << endl;
	}
	catch (string){
		cout << "抛出string类型异常!" << endl;
	}
	//捕获所有异常
	catch (...){
		cout << "抛出其他类型异常!" << endl;
	}
	system("pause");
	return EXIT_SUCCESS;
}
```

### 栈解旋(unwinding)
异常被抛出后,从进入try块起,到异常被抛掷前,这期间在栈上构造的所有对象,都会被自动析构。析构的顺序与构造的顺序相反,这一过程称为栈的解旋(unwinding).

```c++
class Person{
public:
	Person(string name){
		mName = name;
		cout << mName << "对象被创建!" << endl;
	}
	~Person(){
		cout << mName << "对象被析构!" << endl;
	}
public:
	string mName;
};

void TestFunction(){
	
	Person p1("aaa");
	Person p2("bbb");
	Person p3("ccc");

	//抛出异常
	throw 10;
}

int main(){

	try{
		TestFunction();
	}
	catch (...){
		cout << "异常被捕获!" << endl;
	}

	system("pause");
	return EXIT_SUCCESS;
}
```


### 异常接口声明

- 为了加强程序的可读性,可以在函数声明中<font color=#ff0000>列出可能抛出异常的所有类型</font>,例如：`void func() throw(A,B,C)`;这个函数func能够且只能抛出类型A,B,C<font color=#ff0000>及其子类型</font>的异常。
- 如果在函数声明中没有包含异常接口声明,则此函数可以抛任何类型的异常,例如:void func()
- 一个不抛任何类型异常的函数可声明为:void func() throw()
- 如果一个函数抛出了它的异常接口声明所不允许抛出的异常,unexcepted函数会被调用,该函数默认行为调用terminate函数中断程序。

```c++
//可抛出所有类型异常
void TestFunction01(){
	throw 10;
}

//只能抛出int char char*类型异常
void TestFunction02() throw(int,char,char*){ //c++17已经禁止这个了
	string exception = "error!";
	throw exception;
}

//不能抛出任何类型异常
void TestFunction03() throw(){ // 新版改为了noexcept
	throw 10;
}

int main(){

	try{
		//TestFunction01();
		//TestFunction02();
		//TestFunction03();
	}
	catch (...){
		cout << "捕获异常!" << endl;
	}
	
	system("pause");
	return EXIT_SUCCESS;
}
```


### 异常变量生命周期

- throw的异常是有类型的,可以是数字、字符串、类对象。
- throw的异常是有类型的,catch需严格匹配异常类型。

```c++
class MyException
{
public:
	MyException(){
		cout << "异常变量构造" << endl;
	};
	MyException(const MyException & e)
	{
		cout << "拷贝构造" << endl;
	}
	~MyException()
	{
		cout << "异常变量析构" << endl;
	}
};
void DoWork()
{
	throw new MyException(); //test1 2都用 throw MyExecption();
}

void test01()
{
	try
	{
		DoWork();
	}
	catch (MyException e)
	{
		cout << "捕获 异常" << endl;
	}
}
void test02()
{
	try
	{
		DoWork();
	}
	catch (MyException &e)
	{
		cout << "捕获 异常" << endl;
	}
}

void test03()
{
	try
	{
		DoWork();
	}
	catch (MyException *e)
	{
		cout << "捕获 异常" << endl;
		delete e;
	}
}
```


### 异常的多态使用

```c++
//异常基类
class BaseException{
public:
	virtual void printError(){};
};

//空指针异常
class NullPointerException : public BaseException{
public:
	virtual void printError(){
		cout << "空指针异常!" << endl;
	}
};
//越界异常
class OutOfRangeException : public BaseException{
public:
	virtual void printError(){
		cout << "越界异常!" << endl;
	}
};

void doWork(){

	throw NullPointerException();
}

void test()
{
	try{
		doWork();
	}
	catch (BaseException& ex){
		ex.printError();
	}
}
```


## C++标准异常库
### 标准库介绍
标准库中也提供了很多的异常类,它们是通过类继承组织起来的。异常类继承层级结构图如下：
 
<img src="https://article.biliimg.com/bfs/article/f1a6902323f09979405e2afc0aedf98138716159.png" alt="image.png" style="zoom:70%;" />


每个类所在的头文件在图下方标识出来。
**标准异常类的成员**：
1. 在上述继承体系中,每个类都有提供了构造函数、复制构造函数、和赋值操作符重载。
2. logic_error类及其子类、runtime_error类及其子类,它们的构造函数是接受一个string类型的形式参数,用于异常信息的描述
3. 所有的异常类都有一个`what()`方法,返回`const char*` 类型（C风格字符串）的值,描述异常信息。

**标准异常类的具体描述**：

|                   |                                                                                                                   |
| :---------------: | :---------------------------------------------------------------------------------------------------------------: |
|       异常名称        |                                                        描述                                                         |
|     exception     |                                                    所有标准异常类的父类                                                     |
|     bad_alloc     |                                    当operator new and operator new[],请求分配内存失败时                                     |
|   bad_exception   | 这是个特殊的异常,如果函数的异常抛出列表里声明了bad_exception异常,当函数内部抛出了异常抛出列表中没有的异常,这是调用的unexpected函数中若抛出异常,不论什么类型,都会被替换为bad_exception类型 |
|    bad_typeid     |                               使用typeid操作符,操作一个NULL指针,而该指针是带有虚函数的类,这时抛出bad_typeid异常                                |
|     bad_cast      |                                              使用dynamic_cast转换引用失败的时候                                              |
| ios_base::failure |                                                    io操作过程出现错误                                                     |
|    logic_error    |                                                 逻辑错误,可以在运行前检测的错误                                                  |
|   runtime_error   |                                                运行时错误,仅在运行时才可以检测的错误                                                |

**logic_error的子类：**


|       异常名称       |                               描述                               |
| :--------------: | :------------------------------------------------------------: |
|   length_error   |             试图生成一个超出该类型最大长度的对象时,例如vector的resize操作              |
|   domain_error   |             参数的值域错误,主要用在数学函数中。例如使用一个负值调用只能操作非负数的函数             |
|   out_of_range   |                             超出有效范围                             |
| invalid_argument | 参数不合适。在标准库中,当利用string对象构造bitset时,而string中的字符不是’0’或’1’的时候,抛出该异常 |

**runtime_error的子类：**


|       异常名称       |                               描述                               |
| :--------------: | :------------------------------------------------------------: |
|   range_error    |                        计算结果超出了有意义的值域范围                         |
|  overflow_error  |                             算术计算上溢                             |
| underflow_error  |                             算术计算下溢                             |
| invalid_argument | 参数不合适。在标准库中,当利用string对象构造bitset时,而string中的字符不是’0’或’1’的时候,抛出该异常 |

```c++
#include<stdexcept>
class Person{
public:
	Person(int age){
		if (age < 0 || age > 150){
			throw out_of_range("年龄应该在0-150岁之间!");
		}
	}
public:
	int mAge;
};

int main(){

	try{
		Person p(151);
	}
	catch (out_of_range& ex){
		cout << ex.what() << endl;
	}
	
	system("pause");
	return EXIT_SUCCESS;
}
```

### 编写自己的异常类
- 标准库中的异常是有限的；
- 在自己的异常类中,可以添加自己的信息。（标准库中的异常类值允许设置一个用来描述异常的字符串）。

2. 如何编写自己的异常类？
	1. 建议自己的异常类要继承标准异常类。因为C++中可以抛出任何类型的异常,所以我们的异常类可以不继承自标准异常,但是这样可能会导致程序混乱,尤其是当我们多人协同开发时。
	2. 当继承标准异常类时,应该重载父类的what函数和虚析构函数。
	3. 因为栈展开的过程中,要复制异常类型,那么要根据你在类中添加的成员考虑是否提供自己的复制构造函数。

```c++
//自定义异常类
class MyOutOfRange:public exception
{
public:
	MyOutOfRange(const string  errorInfo)
	{
		this->m_Error = errorInfo;
	}

	MyOutOfRange(const char * errorInfo)
	{
		this->m_Error = string(errorInfo);
	}

	virtual  ~MyOutOfRange()
	{
	
	}
	virtual const char *  what() const
	{
		return this->m_Error.c_str() ; // c_str()就是将C++的string转化为C的字符串数组,c_str()生成一个const char *指针,指向字符串的首地址
	}

	string m_Error;

};

class Person
{
public:
	Person(int age)
	{
		if (age <= 0 || age > 150)
		{
			//抛出异常 越界
			//cout << "越界" << endl;
			//throw  out_of_range("年龄必须在0~150之间");

			//throw length_error("长度异常");
			throw MyOutOfRange(("我的异常 年龄必须在0~150之间"));
		}
		else
		{
			this->m_Age = age;
		}
		
	}

	int m_Age;
};
void test01()
{
	try
	{
		Person p(151);
	}
	catch ( out_of_range & e )
	{
		cout << e.what() << endl;
	}
	catch (length_error & e)
	{
		cout << e.what() << endl;
	}
	catch (MyOutOfRange e)
	{
		cout << e.what() << endl;
	}
}
```

## std::current_exception

`std::current_exception` 是 C++11 引入的函数，用于捕获并创建**当前抛出的异常的副本**。这个副本被封装在 `std::exception_ptr` 类型中，这是一个可以跨线程安全传递的异常指针。
作用：在异常处理块（catch 块）中使用 `std::current_exception` 来捕获当前抛出的异常，并将其以 `std::exception_ptr` 的形式保存下来。
与 `std::promise` 配合：`std::current_exception` 可以与 `std::promise::set_exception` 配合使用，以将异常状态从一个线程传递到另一个线程。


示例：
```c++
try {
    // 一些可能抛出异常的操作
} catch (...) {
    auto exceptionPtr = std::current_exception();
    // 现在可以将 exceptionPtr 传递到其他线程
}
```


# c++输入和输出流
## 流的概念和流类库的结构

程序的输入指的是从输入文件将数据传送给程序,程序的输出指的是从程序将数据传送给输出文件。
C++输入输出包含以下三个方面的内容：
- 对系统指定的标准设备的输入和输出。即从键盘输入数据,输出到显示器屏幕。这种输入输出称为标准的输入输出,简称标准I/O。
- 以外存磁盘文件为对象进行输入和输出,即从磁盘文件输入数据,数据输出到磁盘文件。以外存文件为对象的输入输出称为文件的输入输出,简称文件I/O。
- 对内存中指定的空间进行输入和输出。通常指定一个字符数组作为存储空间(实际上可以利用该空间存储任何信息)。这种输入和输出称为字符串输入输出,简称串I/O。
- C++编译系统提供了用于输入输出的iostream类库。iostream这个单词是由3个部 分组成的,即i-o-stream,意为输入输出流。在iostream类库中包含许多用于输入输出的类。常用的见表

<img src="https://article.biliimg.com/bfs/article/81f3c4f1c6234752f64185910b07e3e438716159.png" alt="image.png" style="zoom:80%;" />

<img src="https://article.biliimg.com/bfs/article/6c6e831dea6fbd6b78d69b28e6da215438716159.png" alt="image.png" style="zoom:70%;" />



ios是抽象基类,由它派生出istream类和ostream类,两个类名中第1个字母i和o分别代表输入(input)和输出(output)。 istream类支持输入操作,ostream类支持输出操作, iostream类支持输入输出操作。iostream类是从istream类和ostream类通过多重继承而派生的类。其继承层次见上图表示。



C++对文件的输入输出需要用ifstrcam和ofstream类,两个类名中第1个字母i和o分别代表输入和输出,第2个字母f代表文件 (file)。ifstream支持对文件的输入操作, ofstream支持对文件的输出操作。类ifstream继承了类istream,类ofstream继承了类ostream,类fstream继承了 类iostream。见图 

<img src="https://article.biliimg.com/bfs/article/b8a0c244ca07fdbad21b90f059784e0a38716159.png" alt="image.png" style="zoom:70%;" />

I/O类库中还有其他一些类,但是对于一般用户来说,以上这些已能满足需要了。
**与iostream类库有关的头文件** 
iostream类库中不同的类的声明被放在不同的头文件中,用户在自己的程序中用`#include`命令包含了有关的头文件就相当于在本程序中声明了所需 要用到的类。可以换 —种说法：头文件是程序与类库的接口,iostream类库的接口分别由不同的头文件来实现。常用的有 
- iostream  包含了对输入输出流进行操作所需的基本信息。
- fstream  用于用户管理的文件的I/O操作。
- strstream  用于字符串流I/O。
- stdiostream  用于混合使用C和C + +的I/O机制时,例如想将C程序转变为C++程序。
- iomanip  在使用格式化I/O时应包含此头文件。

**在iostream头文件中定义的流对象**

在 iostream 头文件中定义的类有 ios,istream,ostream,iostream,istream 等。
在iostream头文件中不仅定义了有关的类,还定义了4种流对象,
**对象	含义	对应设备	对应的类	c语言中相应的标准文件**



|  名称  |  描述   | 设备  |         类型         | 文件描述符  |
| :--: | :---: | :-: | :----------------: | :----: |
| cin  | 标准输入流 | 键盘  | istream_withassign | stdin  |
| cout | 标准输出流 | 屏幕  | ostream_withassign | stdout |
| cerr | 标准错误流 | 屏幕  | ostream_withassign | stderr |
| clog | 标准错误流 | 屏幕  | ostream_withassign | stderr |

在iostream头文件中定义以上4个流对象用以下的形式（以cout为例）：
    `ostream cout (stdout)`;
	在定义cout为ostream流类对象时,把标准输出设备stdout作为参数,这样它就与标准输出设备(显示器)联系起来,如果有
    cout <<3;
就会在显示器的屏幕上输出3。
**在iostream头文件中重载运算符**
“<<”和“>>”本来在C++中是被定义为左位移运算符和右位移运算符的,由于在iostream头文件中对它们进行了重载, 使它们能用作标准类型数据的输入和输出运算符。所以,在用它们的程序中必须用#include命令把iostream包含到程序中。
	`#include <iostream>`
1)	`>>a`表示将数据放入a对象中。
2)	`<<a`表示将a对象中存储的数据拿出。



## 标准I/O流
标准I/O对象:cin,cout,cerr,clog
**cout流对象**
cout是console output的缩写,意为在控制台（终端显示器）的输出。强调几点。
1. cout不是C++预定义的关键字,它是ostream流类的对象,在iostream中定义。 顾名思义,流是流动的数据,cout流是流向显示器的数据。cout流中的数据是用流插入	运算符“<<”顺序加入的。如果有:`cout<<"I "<<"study C++ "<<"very hard. << “hello world !"`;
	按顺序将字符串"I ", "study C++ ", "very hard."插人到cout流中,cout就将它们送到显示器,在显示器上输出字符串"I study C++ very hard."。cout流是容纳数据的载体,它并不是一个运算符。人们关心的是cout流中的内容,也就是向显示器输出什么。
2. 用“cout<<”输出基本类型的数据时,可以不必考虑数据是什么类型,系统会判断数据的类型,并根据其类型选择调用与之匹配的运算符重载函数。这个过程都是自动的,	用户不必干预。如果在C语言中用prinf函数输出不同类型的数据,必须分别指定相应的输出格式符,十分麻烦,而且容易出错。C++的I/O机制对用户来说,显然是方便而安全的。
3. cout流在内存中对应开辟了一个缓冲区,用来存放流中的数据,当向cout流插人一个endl时,不论缓冲区是否已满,都立即输出流中所有数据,然后插入一个换行符, 并刷新流（清空缓冲区）。注意如果插人一个换行符`”\n“`（如`cout<<a<<"\n"`）,则只输出和换行,而不刷新cout流(但并不是所有编译系统都体现出这一区别）。
4. 在iostream中只对"<<"和">>"运算符用于标准类型数据的输入输出进行了重载,但未对用户声明的类型数据的输入输出进行重载。如果用户声明了新的类型,并希望用"<<"和">>"运算符对其进行输入输出,按照重运算符重载来做。

**cerr流对象**
<font color=#ff0000>cerr流对象是标准错误流,cerr流已被指定为与显示器关联</font>。cerr的 作用是向标准错误设备(standard error device)输出有关出错信息。cerr与标准输出流cout的作用和用法差不多。但有一点不同：cout流通常是传送到显示器输出,但也可以被重定向输出到磁盘文件,而cerr流中的信息只能在显示器输出。当调试程序时,往往不希望程序运行时的出错信息被送到其他文件,而要求在显示器上及时输出,这时 应该用cerr。cerr流中的信息是用户根据需要指定的。
**clog流对象**
clog流对象也是标准错误流,它是console log的缩写。它的作用和cerr相同,都是在终端显示器上显示出错信息。区别：cerr是不经过缓冲区,直接向显示器上输出有关信息,而clog中的信息存放在缓冲区中,缓冲区满后或遇endl时向显示器输出。

缓冲区的概念:

<img src="https://article.biliimg.com/bfs/article/d9e214a54349b2e555a33a33585017e238716159.png" alt="image.png" style="zoom:60%;" />



## 标准输入流
标准输入流对象cin,重点掌握的函数
```c++
cin.get() //一次只能读取一个字符
cin.get(一个参数) //读一个字符
cin.get(两个参数) //可以读字符串
cin.getline()
cin.ignore()
cin.peek() // 用于查看输入流中下一个字符,而不从输入流中移除它
cin.putback() // 将一个字符插入到输入流中,以便下一次读取时可以读取到插入的字符
```

```c++
//cin.get
void test01(){
#if 0
	char ch = cin.get();
	cout << ch << endl;
	cin.get(ch);
	cout << ch << endl;
	//链式编程
	char char1, char2, char3, char4;
	cin.get(char1).get(char2).get(char3).get(char4);

	cout << char1 << " " << char2 << "" << char3 <<  " " << char4 << " ";
#endif

	char buf[1024] = { 0 };
	//cin.get(buf.1024);
	cin.getline(buf,1024);
	cout << buf;
}

//cin.ignore
void test02(){

	char buf[1024] = { 0 };
	cin.ignore(2); //忽略2个缓冲区当前字符,不写是一个
	cin.get(buf,1024);
	cout << buf << endl;
}

//cin.putback 将数据放回缓冲区
void test03(){

	//从缓冲区取走一个字符
	char ch = cin.get();
	cout << "从缓冲区取走的字符:" << ch << endl;
	//将数据再放回缓冲区
	cin.putback(ch);
	char buf[1024] = { 0 };
	cin.get(buf,1024);
	cout << buf << endl;

}

//cin.peek 偷窥
void test04(){
	
	//偷窥下缓冲区的数据
	char ch = cin.peek();
	cout << "偷窥缓冲区数据:" << ch << endl;
	char buf[1024] = { 0 };
	cin.get(buf, 1024);
	cout << buf << endl;
}

//练习  作业 使用cin.get和putback完成类似功能
void test05(){
	
	cout << "请输入一个数字或者字符串:" << endl;
	char ch = cin.peek();
	if(ch >= '0' && ch <= '9'){
		int number;
		cin >> number;
		cout << "数字:" << number << endl;
	}
	else{
		char buf[64] = { 0 };
		cin.getline(buf, 64);
		cout << "字符串:" <<  buf << endl;
	}
}
```

## 标准输出流
### 字符输出

```c++
cout.flush() //刷新缓冲区 Linux下有效
cout.put() //向缓冲区写字符
cout.write() //从buffer中写num个字节到当前输出流中。
```

```c++
//cout.flush 刷新缓冲区,linux下有效
void test01(){	
	cout << "hello world";
	//刷新缓冲区
	cout.flush(); 
}

//cout.put 输出一个字符
void test02(){
	
	cout.put('a');
	//链式编程
	cout.put('h').put('e').put('l');
}

//cout.write 输出字符串 buf,输出多少个
void test03(){
	
	//char* str = "hello world!";
	//cout.write(str, strlen(str));
	char* str = "*************";
	for (int i = 1; i <= strlen(str); i ++){
		cout.write(str, i);
		cout << endl;
	}

	for (int i = strlen(str); i > 0; i --){
		cout.write(str, i);
		cout << endl;
	}

}
```

### 格式化输出
在输出数据时,为简便起见,往往不指定输出的格式,由系统根据数据的类型采取默认的格式,但有时希望数据按指定的格式输出,如要求以十六进制或八进制形式输出一个整数,对输出的小数只保留两位小数等。有两种方法可以达到此目的。
1. 使用控制符的方法；
2. 使用流对象的有关成员函数。

#### 使用流对象的有关成员函数


通过调用流对象cout中用于控制输出格式的成员函数来控制输出格式。用于控制输出格式的常用的成员函数如下：

|     流成员函数      |    与之作用相同的控制符     |                      作用                       |
| :------------: | :---------------: | :-------------------------------------------: |
| `precision(n)` | `setprecision(n)` |                  设置实数的精度为n位                   |
|   `width(n)`   |     `setw(n)`     |                   设置字段宽度为n位                   |
|   `fill(c)`    |   `setfill(c)`    |                    设置填充宇符c                    |
|    `setf()`    |  `setiosflags()`  | 设置输出格式状态,括号中应给出格式状态,内容与控制符setiosflags括号中的内容相同 |
|   `unsetf()`   |  `resetioflags`   |            终止已设置的输出格式状态,在括号中应指定内容             |

流成员函数setf和控制符setiosflags括号中的参数表示格式状态,它是通过格式标志来指定的。格式标志在类ios中被定义为枚举值。因此在引用这些格式标志时要在前面加上类名ios和域运算符“::”。格式标志见表13.5。


设置格式状态的格式标志 

|       作用        |               描述               |
| :-------------: | :----------------------------: |
|    ios::left    |        输出数据在本域宽范围内向左对齐         |
|   ios::right    |        输出数据在本域宽范围内向右对齐         |
|  ios::internal  | 数值的符号位在域宽内左对齐,数值右对齐,中间由填充字符填充  |
|    ios::dec     |           设置整数的基数为10           |
|    ios::oct     |           设置整数的基数为8            |
|    ios::hex     |           设置整数的基数为16           |
|  ios::showbase  | 强制输出整数的基数（八进制数以0打头,十六进制数以0x打头） |
|  ios:showpoint  |         强制输出浮点数的小点和尾数0         |
| ios::uppercase  |   在以科学记数法格式E和以十六进制输出字母时以大写表示   |
|  ios::showpos   |            对正数显示“+”            |
| ios::scientific |         浮点数以科学记数法格式输出          |
|   ios::fixed    |        浮点数以定点格式（小数形式）输出        |
|  ios::unitbuf   |          每次输出之后刷新所有的流          |
|   ios::stdio    |     每次输出之后清除stdout,stderr      |

#### 控制符格式化输出

C++提供了在输入输出流中使用的控制符(有的书中称为操纵符)。

|  控制符                            |  作用                                                                               |
|:-------------------------------:|:---------------------------------------------------------------------------------:|
|  `dec`                          |  设置数值的基数为10                                                                       |
|  `hex`                          |  设置数值的基数为16                                                                       |
|  `oct`                          |  设置数值的基数为8                                                                        |
|  `setfill(c)`                   |  设置填充字符c,c可以是字符常量或字符变量                                                            |
|`setprecision(n)`|  设置浮点数的精度为n位。在以一般十进制小数形式输出时,n代表有效数字。在以fixed(固定小数位数）形式和scientific(指数）形式输出时,n为小数位数  |
|`setw(n)`|  设置字段宽度为n位                                                                        |
|`setiosflags(ios::fixed)` |  设置浮点数以固定的小数位数显示                                                                  |
|`setiosftags(ios:scientific)`|  设置浮点数以科学记数法（即指数形式）显示                                                             |
|  `setiosflags(ios:left)`        |  输出数据左对齐                                                                          |
|  `setiosflags(ios:right)`       |  输出数据右对齐                                                                          |
|  `setiosflags(ios:skipws)`      |  忽略前导的空格                                                                          |
|  `setiosflags(ios:uppercase)`   |  数据以十六进制形式输出时字母以大写表示                                                              |
|  `setiosflags(ios:lowercase)`   |  数据以十六进制形式输出时宇母以小写表示                                                              |
|  `setiosflags(ios:showpos)`     |  输出正数时给出"+"号                                                                      |  

需要注意的是：如果使用了控制符,在程序单位的开头除了要加`iostream`头文件外,还要加`iomanip`头文件。 


```c++
//通过流成员函数
void test01(){
	
	int number = 99;
	cout.width(20);
	cout.fill('*');
	cout.setf(ios::left);
	cout.unsetf(ios::dec); //卸载十进制,必须卸载默认的十进制状态
	cout.setf(ios::hex);
	cout.setf(ios::showbase);
	cout.unsetf(ios::hex);
	cout.setf(ios::oct);
	cout << number << endl;

}

//使用控制符
void test02(){

	int number = 99;
	cout << setw(20)
		<< setfill('~')
		<< setiosflags(ios::showbase)
		<< setiosflags(ios::left)
		<< hex
		<< number
		<< endl;

}
```


## 文件读写
### 文件流类和文件流对象

输入输出是以系统指定的标准设备（输入设备为键盘,输出设备为显示器）为对象的。在实际应用中,常以磁盘文件作为对象。即从磁盘文件读取数据,将数据输出到磁盘文件。
和文件有关系的输入输出类主要在`fstream.h`这个头文件中被定义,在这个头文件中主要被定义了三个类,由这三个类控制对文件的各种输入输出操作,他们分别是ifstream、ofstream、fstream,其中fstream类是由iostream类派生而来,他们之间的继承关系见下图所示：

<img src="https://article.biliimg.com/bfs/article/0c7aad8ff01d236e96d0c7aac5e0628438716159.png" alt="image.png" style="zoom:100%;" />


由于文件设备并不像显示器屏幕与键盘那样是标准默认设备,所以它在fstream头文件中是没有像cout那样预先定义的全局对象,所以我们必须自己定义一个该类的对象。ifstream类,它是从istream类派生的,用来支持从磁盘文件的输入。ofstream类,它是从ostream类派生的,用来支持向磁盘文件的输出。

fstream类,它是从iostream类派生的,用来支持对磁盘文件的输入输出。

### C++打开文件

所谓打开(open)文件是一种形象的说法,如同打开房门就可以进入房间活动一样。 打开文件是指在文件读写之前做必要的准备工作,包括：
- 为文件流对象和指定的磁盘文件建立关联,以便使文件流流向指定的磁盘文件。
- 指定文件的工作方式,如：该文件是作为输入文件还是输出文件,是ASCII文件还是二进制文件等。

以上工作可以通过两种不同的方法实现:
1. 调用文件流的成员函数open。如
	```c++
	ofstream outfile;  //定义ofstream类(输出文件流类)对象outfile
	outfile.open("f1.dat",ios::out);  //使文件流与f1.dat文件建立关联
	```

	第2行是调用输出文件流的成员函数open打开磁盘文件f1.dat,并指定它为输出文件,文件流对象outfile将向磁盘文件f1.dat输出数据。ios::out是I/O模式的一种,表示以输出方式打开一个文件。或者简单地说,此时f1.dat是一个输出文件,接收从内存输出的数据。
	磁盘文件名可以包括路径,如"c:\\new\\f1.dat",如缺省路径,则默认为当前目录下的文件。

2. 在定义文件流对象时指定参数
	在声明文件流类时定义了带参数的构造函数,其中包含了打开磁盘文件的功能。因此,	可以在定义文件流对象时指定参数,调用文件流类的构造函数来实现打开文件的功能。

根据下面的内容写一个2列的markdown图表
方式 
作用 
`ios::in` 
以输入方式打开文件 
`ios::out` 
以输出方式打开文件（这是默认方式）,如果已有此名字的文件,则将其原有内容全部清除 
`ios::app` 
以输出方式打开文件,写入的数据添加在文件未尾 
`ios::ate` 
打开一个已有的文件,文件指针指向文件末尾 
`ios::trunc` 
打开一个文件,如果文件已存在,则删除其中全部数据,如文件不存在,则建立新文件。如已指定了ios:out方式,而未指定ios::app,ios:ate,ios:in,则同时默认此方式
`ios::binary` 
以二进制方式打开一个文件,如不指定此方式则默认为ASCIII方式 
`ios::nocreate` 
打开一个已有的文件,如文件不存在,则打开失败。nocrcate的意思是不建立新文件 
`ios::noreplace` 
如果文件不存在则建立新文件,如果文件已存在则操作失败,replace的意思是不更新原有文件 
`ios:in|ios:out` 
以输入和输出方式打开文件,文件可读可写 
`ios::out|ios:binary` 
以二进制方式打开一个输出文件 
`ios:in|ios:binary`
以二进制方式打开一个输入文件 

|方式             |作用                                    |
|-----------------|---------------------------------------|
|`ios::in`        |以输入方式打开文件                      |
|`ios::out`       |以输出方式打开文件（默认方式）,如果已有此名字的文件,则将其原有内容全部清除|
|`ios::app`       |以输出方式打开文件,写入的数据添加在文件末尾|
|`ios::ate`       |打开一个已有的文件,文件指针指向文件末尾|
|`ios::trunc`     |打开一个文件,如果文件已存在,则删除其中全部数据,如文件不存在,则建立新文件。如已指定了ios:out方式,而未指定ios::app,ios:ate,ios:in,则同时默认此方式|
|`ios::binary`    |以二进制方式打开一个文件,如不指定此方式则默认为ASCIII方式|
|`ios::nocreate`  |打开一个已有的文件,如文件不存在,则打开失败。nocrcate的意思是不建立新文件 |
|`ios::noreplace` |如果文件不存在则建立新文件,如果文件已存在则操作失败,replace的意思是不更新原有文件|
|`ios:in\|ios:out`|以输入和输出方式打开文件,文件可读可写   |
|`ios::out\|ios:binary`  |以二进制方式打开一个输出文件             |
| `ios:in\ ios:binary `   |以二进制方式打开一个输入文               |

格式化以后,还可以回复原来的格式状态,这里我们需要保存原来你的格式,然后再回复

```c++
int main() {
    default_random_engine e;  
    uniform_real_distribution<double> u(0,1);  
    
    // 保存原始的格式状态和精度
    ios::fmtflags original_flags = cout.flags();
    streamsize original_precision = cout.precision();
    for (int i = 0; i < 10; ++i) {  
        cout << std::setiosflags(ios::fixed) << std::setprecision(4) << u(e) << " ";  
    }  
    cout << endl;  
    // 恢复原始的格式状态和精度
    cout.flags(original_flags);
    cout.precision(original_precision);
    cout << u(e) << endl;
    return 0;
}
```



### C++关闭文件
在对已打开的磁盘文件的读写操作完成后,应关闭该文件。关闭文件用成员函数close。如：outfile.close( );  //将输出文件流所关联的磁盘文件关闭
<font color=#ff0000>所谓关闭,实际上是解除该磁盘文件与文件流的关联,原来设置的工作方式也失效,这样,就不能再通过文件流对该文件进行输入或输出</font>。此时可以将文件流与其他磁盘文件建立关联,通过文件流对新的文件进行输入或输出。如:
`outfile.open("f2.dat",ios::app|ios::nocreate)`；
此时文件流outfile与f2.dat建立关联,并指定了f2.dat的工作方式。


### C++对ASCII文件的读写操作


如果文件的每一个字节中均以ASCII代码形式存放数据,即一个字节存放一个字符,这个文件就是ASCII文件(或称字符文件)。程序可以从ASCII文件中读入若干个字符,也可以向它输出一些字符。
1. 用流插入运算符“<<”和流提取运算符“>>”输入输出标准类型的数据。“<<”和“ >>”都巳在iostream中被重载为能用于ostream和istream类对象的标准类型的输入输出。由于ifstream和 ofstream分别是ostream和istream类的派生类；因此它们从ostream和istream类继承了公用的重载函数,所以在对磁盘文件的操作中,可以通过文件流对象和流插入运算符“<<”及 流提取运算符“>>”实现对磁盘 文件的读写,如同用cin、cout和<<、>>对标准设备进行读写一样。
2. 用文件流的put、get、geiline等成员函数进行字符的输入输出,：用C++流成员函数put输出单个字符、C++ get()函数读入一个字符和C++ getline()函数读入一行字符。
```c++
int main(){

	char* sourceFileName = "./source.txt";
	char* targetFileName = "./target.txt";
	//创建文件输入流对象
	ifstream ism(sourceFileName, ios::in);
	//创建文件输出流对象
	ofstream osm(targetFileName,ios::out);

	if (!ism){
		cout << "文件打开失败!" << endl;
	}

	while (!ism.eof()){
		char buf[1024] = { 0 };
		ism.getline(buf,1024);
		cout << buf << endl;
		osm << buf << endl;
	}

	//关闭文件流对象
	ism.close();
	osm.close();

	system("pause");
	return EXIT_SUCCESS;
}
```

### C++对二进制文件的读写操作
二进制文件不是以ASCII代码存放数据的,它将内存中数据存储形式不加转换地传送到磁盘文件,因此它又称为内存数据的映像文件。因为文件中的信息不是字符数据,而是字节中的二进制形式的信息,因此它又称为字节文件。
对二进制文件的操作也需要先打开文件,用完后要关闭文件。在打开时要用ios::binary指定为以二进制形式传送和存储。二进制文件除了可以作为输入文件或输出文件外,还可以是既能输入又能输出的文件。这是和ASCII文件不同的地方。

用成员函数read和write读写二进制文件
对二进制文件的读写主要用istream类的成员函数read和write来实现。这两个成员函数的原型为
```c++
    istream& read(char *buffer,int len);
    ostream& write(const char * buffer,int len);
```
字符指针buffer指向内存中一段存储空间。len是读写的字节数。调用的方式为：
```c++
    a. write(p1,50);
    b. read(p2,30);
```

上面第一行中的a是输出文件流对象,write函数将字符指针p1所给出的地址开始的50个字节的内容不加转换地写到磁盘文件中。在第二行中,b是输入文件流对象,read 函数从b所关联的磁盘文件中,读入30个字节(或遇EOF结束）,存放在字符指针p2所指的一段空间内。


```c++
class Person{
public:
	Person(char* name,int age){
		strcpy(this->mName, name);
		this->mAge = age;
	}
public:
	char mName[64];
	int mAge;
};

int main(){

	char* fileName = "person.txt";
	//二进制模式读写文件
	//创建文件对象输出流
	ofstream osm(fileName, ios::out | ios::binary);

	Person p1("John",33);
	Person p2("Edward", 34);

	//Person对象写入文件
	osm.write((const char*)&p1,sizeof(Person));
	osm.write((const char*)&p2, sizeof(Person));

	//关闭文件输出流
	osm.close();

	//从文件中读取对象数组
	ifstream ism(fileName, ios::in | ios::binary);
	if (!ism){
		cout << "打开失败!" << endl;
	}
	
	Person p3;
	Person p4;

	ism.read((char*)&p3, sizeof(Person));
	ism.read((char*)&p4, sizeof(Person));

	cout << "Name:" << p3.mName << " Age:" << p3.mAge << endl;
	cout << "Age:" << p4.mName << " Age:" << p4.mAge << endl;

	//关闭文件输入流
	ism.close();
	system("pause");
	return EXIT_SUCCESS;
}
```



# STL

## STL概论

STL(<font color=#ff0000>Standard Template Library</font>,标准模板库),是惠普实验室开发的一系列软件的统称。现在主要出现在 c++中,但是在引入 c++之前该技术已经存在很长时间了。
STL 从广义上分为: <font color=#ff0000>容器(container) 算法(algorithm) 迭代器(iterator)</font>,容器和算法之间通过迭代器进行无缝连接。STL 几乎所有的代码都采用了模板类或者模板函数,这相比传统的由函数和类组成的库来说提供了更好的代码重用机会。STL(Standard Template Library)标准模板库,在我们 c++标准程序库中隶属于 STL 的占到了 80%以上。

### STL六大组件简介

STL提供了六大组件,彼此之间可以组合套用,这六大组件分别是:<font color=#ff0000>容器、算法、迭代器、仿函数、适配器（配接器）、空间配置器</font>。

**容器**：各种数据结构,如`vector、list、deque、set、map`等,用来存放数据,从实现角度来看,STL容器是一种class template。
**算法**：各种常用的算法,如`sort、find、copy、for_each`。从实现的角度来看,STL算法是一种function tempalte.
**迭代器**：扮演了容器与算法之间的**胶合剂**,共有五种类型,从实现角度来看,迭代器是一种将`operator* , operator-> , operator++,operator--`等指针相关操作予以重载的class template. 所有STL容器都附带有自己专属的迭代器,只有容器的设计者才知道如何遍历自己的元素。<font color=#ff0000>原生指针(native pointer)也是一种迭代器</font>。
**仿函数**：行为类似函数,可作为算法的某种策略。从实现角度来看,仿函数是一种重载了operator()的class或者class template
**适配器**：一种用来**修饰**容器或者仿函数或迭代器接口的东西。
**空间配置器**：负责空间的配置与管理。从实现角度看,配置器是一个实现了动态空间配置、空间管理、空间释放的class tempalte.

STL六大组件的交互关系,容器通过空间配置器取得数据存储空间,算法通过迭代器存储容器中的内容,仿函数可以协助算法完成不同的策略的变化,适配器可以修饰仿函数。

### STL优点

- STL 是 C++的一部分,因此不用额外安装什么,它被内建在你的编译器之内。
- STL 的一个重要特性是将数据和操作分离。数据由容器类别加以管理,操作则由可定制的算法定义。迭代器在两者之间充当“粘合剂”,以使算法可以和容器交互运作
- 程序员可以不用思考 STL 具体的实现过程,只要能够熟练使用STL就OK了。这样他们就可以把精力放在程序开发的别的方面。
- STL 具有高可重用性,高性能,高移植性,跨平台的优点。
- **高可重用性**：STL 中几乎所有的代码都采用了模板类和模版函数的方式实现,这相比于传统的由函数和类组成的库来说提供了更好的代码重用机会。关于模板的知
- 识,已经给大家介绍了。
- **高性能**：如 map 可以高效地从十万条记录里面查找出指定的记录,因为 map 是采用红黑树的变体实现的。
- **高移植性**：如在项目 A 上用 STL 编写的模块,可以直接移植到项目 B 上。

## STL三大组件
#### 容器

STL容器就是将运用最广泛的一些数据结构实现出来。
常用的数据结构：数组(array),链表(list),tree(树),栈(stack),队列(queue),集合(set),映射表(map),根据数据在容器中的排列特性,这些数据分为<font color=#ff0000>序列式容器</font>和<font color=#ff0000>关联式容器</font>两种。
- 序列式容器强调值的排序,序列式容器中的每个元素均有固定的位置,除非用删除或插入的操作改变这个位置。Vector容器、Deque容器、List容器等。
- 关联式容器是<font color=#ff0000>非线性的树结构</font>,更准确的说是二叉树结构。各元素之间没有严格的物理上的顺序关系,也就是说<font color=#ff0000>元素在容器中并没有保存元素置入容器时的逻辑顺序</font>。关联式容器另一个显著特点是：在值中选择一个值作为关键字key,这个关键字对值起到索引的作用,方便查找。Set/multiset容器 Map/multimap容器

<img src="https://article.biliimg.com/bfs/article/bc12e099814041d6d88cc604e089f73338716159.png" alt="image.png" style="zoom:50%;" />


### 算法

STL收录的算法经过了数学上的效能分析与证明,是极具复用价值的,包括常用的排序,查找等等。特定的算法往往搭配特定的数据结构,算法与数据结构相辅相成。
算法分为:<font color=#ff0000>质变算法和非质变算法</font>。
质变算法：是指运算过程中会更改区间内的元素的内容。例如拷贝,替换,删除等等
非质变算法：是指运算过程中不会更改区间内的元素内容,例如查找、计数、遍历、寻找极值等等

### 迭代器

迭代器(iterator)是一种抽象的设计概念,现实程序语言中并没有直接对应于这个概念的实物。在`<<Design Patterns>>`一书中提供了23中设计模式的完整描述,其中iterator模式定义如下：提供一种方法,<font color=#ff0000>使之能够依序寻访某个容器所含的各个元素,而又无需暴露该容器的内部表示方式</font>。
迭代器的设计思维-STL的关键所在,STL的中心思想在于将容器(container)和算法(algorithms)分开,彼此独立设计,最后再一贴胶着剂将他们撮合在一起。从技术角度来看,容器和算法的泛型化并不困难,c++的class template和function template可分别达到目标,如果设计出两这个之间的良好的胶着剂,才是大难题。

迭代器的种类:
- **输入迭代器**:
	- 提供对数据的只读访问	只读,支持`++、==、!=、*`
- **输出迭代器**	
	- 提供对数据的只写访问	只写,支持`++ *`
- **前向迭代器**	
	- 提供读写操作,并能向前推进迭代器读写,支持`++、==、！=`
- **双向迭代器**	
	- 提供读写操作,并能向前和向后操作读写,支持`++、--`,
- **随机访问迭代器**	
	- 提供读写操作,并能以跳跃的方式访问容器的任意数据,是功能最强的迭代器 读写,支持`++、--、[n]、-n、<、<=、>、>=`

迭代器还可以分为：

|         操作         |                   解释                   |
| :----------------: | :------------------------------------: |
|     `iterator`     |              此容器类型的迭代器类型               |
|  `const_iterator`  |          可以读取元素但不能修改元素的迭代器类型           |
| `reverse_iterator` | 反向迭代器(在遍历容器元素时是从容器的最后一个元素向第一个元素反向进行迭代) |

一些STL容器成员函数提供了常规和常量版本。例如,`cbegin()`和`cend()`总是返回`const_iterator`,而`begin()`和`end()`根据容器的常量性返回`iterator`或`const_iterator`。


#cpp面试点
#### 容器操作可能使迭代器失效

- 在向容器添加元素后：
    - 如果容器是`vector`或`string`,且存储空间被重新分配,则指向容器的迭代器、指针、引用都会失效。
    - 对于`deque`,插入到除首尾位置之外的任何位置都会导致指向容器的迭代器、指针、引用失效。如果在首尾位置添加元素,迭代器会失效,但指向存在元素的引用和指针不会失效。
    - 对于`list`和`forward_list`,指向容器的迭代器、指针和引用依然有效。
- 在从一个容器中删除元素后：
    - 对于`list`和`forward_list`,指向容器其他位置的迭代器、引用和指针仍然有效。
    - 对于`deque`,如果在首尾之外的任何位置删除元素,那么指向被删除元素外其他元素的迭代器、指针、引用都会失效；如果是删除`deque`的尾元素,则尾后迭代器会失效,但其他不受影响；如果删除的是`deque`的头元素,这些也不会受影响。
    - 对于`vector`和`string`,指向被删元素之前的迭代器、引用、指针仍然有效。
    - 注意：当我们删除元素时,尾后迭代器总是会失效。
    - 注意：使用失效的迭代器、指针、引用是严重的运行时错误！
    - 建议：将要求迭代器必须保持有效的程序片段最小化。
    - 建议：不要保存`end`返回的迭代器。

### 案例
```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

//STL 中的容器 算法 迭代器
void test01(){
	vector<int> v; //STL 中的标准容器之一 ：动态数组
	v.push_back(1); //vector 容器提供的插入数据的方法
	v.push_back(5);
	v.push_back(3);
	v.push_back(7);
	//迭代器
	vector<int>::iterator pStart = v.begin(); //vector 容器提供了 begin()方法 返回指向第一个元素的迭代器
	vector<int>::iterator pEnd = v.end(); //vector 容器提供了 end()方法 返回指向最后一个元素下一个位置的迭代器
	//通过迭代器遍历
	while (pStart != pEnd){
		cout << *pStart << " ";
		pStart++;
	}
	cout << endl;
}
//STL 容器不单单可以存储基础数据类型,也可以存储类对象
class Teacher
{
public:
	Teacher(int age) :age(age){};
	~Teacher(){};
public:
	int age;
};
void test02(){
	vector<Teacher> v; //存储 Teacher 类型数据的容器
	Teacher t1(10), t2(20), t3(30);
	v.push_back(t1);
	v.push_back(t2);
	v.push_back(t3);
	vector<Teacher>::iterator pStart = v.begin();
	vector<Teacher>::iterator pEnd = v.end();
	//通过迭代器遍历
	while (pStart != pEnd){
		cout << pStart->age << " ";
		pStart++;
	}
	cout << endl;
}
//存储 Teacher 类型指针
void test03(){
	vector<Teacher*> v; //存储 Teacher 类型指针
	Teacher* t1 = new Teacher(10);
	Teacher* t2 = new Teacher(20);
	Teacher* t3 = new Teacher(30);
	v.push_back(t1);
	v.push_back(t2);
	v.push_back(t3);
	//拿到容器迭代器
	vector<Teacher*>::iterator pStart = v.begin();
	vector<Teacher*>::iterator pEnd = v.end();
	//通过迭代器遍历
	while (pStart != pEnd){
		cout << (*pStart)->age << " ";
		pStart++;
	}
	cout << endl;
}
//容器嵌套容器 难点(不理解,可以跳过)
void test04()
{
	vector< vector<int> > v;
	vector<int>v1;
	vector<int>v2;
	vector<int>v3;

	for (int i = 0; i < 5;i++)
	{
		v1.push_back(i);
		v2.push_back(i * 10);
		v3.push_back(i * 100);
	}
	v.push_back(v1);
	v.push_back(v2);
	v.push_back(v3);

	for (vector< vector<int> >::iterator it = v.begin(); it != v.end();it++)
	{
		for (vector<int>::iterator subIt = (*it).begin(); subIt != (*it).end(); subIt ++)
		{
			cout << *subIt << " ";
		}
		cout << endl;
	}
} 
int main(){
	//test01();
	//test02();
	//test03();
	test04();
	system("pause");
	return EXIT_SUCCESS;
}
```


### 迭代器的实现原理

```c++
class String{

private:
  char *_str;
  friend ostream& operator<<(ostream &out,const String &s);
  friend String operator +(const String &s1,const String &s2);
public:
    //构造函数
    String(const char *ptr=nullptr){
      if (ptr== nullptr){
        _str=new char[1];
        *_str='\0';
      }
      else{
        _str=(char *)malloc(strlen(ptr)+1);
        strcpy(_str,ptr);
      }
    }
    ~String(){
      delete []_str;
      _str= nullptr;
    }
    String(const String &other){
      if (other._str== nullptr){
        _str=new char[1];
        *_str='\0';
      }
      else{
        _str=(char *) malloc(strlen(other._str)+1);
        strcpy(_str,other._str);
      }
    }
    String& operator =(const String& src){
      if (&src==this) return *this;
      delete []_str;
      _str=new char[strlen(src._str)+1];
      strcpy(_str,src._str);
      return *this;
    }
    String& operator =(const char *ptr){
      delete []_str;
      _str=new char[strlen(ptr)+1];
      strcpy(_str,ptr);
      return *this;
    }
    bool operator >(const String& src) const{
      return strcmp(this->_str,src._str)>0;
    }
    bool operator <(const String& src) const{
      return strcmp(this->_str,src._str)<0;
    }
    bool operator ==(const String& src) const
    {
      return strcmp(this->_str,src._str)==0;
    }
    int length() const{
      return strlen(this->_str);
    }
    char& operator [](int index){
      return _str[index];
    }
    const char& operator [](int index) const{
      return _str[index];
    }
    const char * c_str() const{
      return _str;
    }
    class iterator{
    public:
        iterator(char *p= nullptr)  :_p(p){}
        bool operator!=(const iterator &it){
          return _p !=it._p;
        }
        void operator++(){
          ++_p;
        }
        char& operator*() {return *_p;}
    private:
        char *_p;
    };
    iterator begin(){return iterator(_str);};
    iterator end(){return iterator(_str+length());}
};

String operator +(const String &s1,const String &s2){
  char *tmp=new char[s1.length()+s2.length()+1];
  strcpy(tmp,s1._str);
  strcat(tmp,s2._str);

  return {tmp};

}
ostream& operator<<(ostream &out,const String &s){

  out<<s._str;
  return out;
}
```

可以看到迭代器是容器里面的一个类，相当于类中类，它提供一种统一的方式，来**透明**的遍历容器 

```c++
  String str1="hello world";
  String::iterator it=str1.begin();
  for(;it!=str1.end();++it){
    cout<<*it<<" ";
  }
  cout<<endl;
```

## 常用容器
### string容器
#### string容器基本概念
C风格字符串(以空字符结尾的字符数组)太过复杂难于掌握,不适合大程序的开发,所以C++标准库定义了一种string类,定义在头文件`<string>`。
String和c风格字符串对比：
- `Char*`是一个指针,String是一个类
	string封装了char*,管理这个字符串,是一个`char*`型的容器。
- String封装了很多实用的成员方法
	查找find,拷贝copy,删除delete 替换replace,插入insert
- 不用考虑内存释放和越界
	 string管理`char*`所分配的内存。每一次string的复制,取值都由string类负责维护,不用担心复制越界和取值越界等。

#### string容器常用操作
##### string 构造函数
```c++
string();//创建一个空的字符串 例如: string str; count     
string(const string& str);//使用一个string对象初始化另一个string对象
string(const char* s);//使用字符串s初始化
string(int n, char c);//使用n个字符c初始化 
```

##### string基本赋值操作
```c++
string& operator=(const char* s);//char*类型字符串 赋值给当前的字符串
string& operator=(const string &s);//把字符串s赋给当前的字符串
string& operator=(char c);//字符赋值给当前的字符串
string& assign(const char *s);//把字符串s赋给当前的字符串
string& assign(const char *s, int n);//把字符串s的前n个字符赋给当前的字符串
string& assign(const string &s);//把字符串s赋给当前字符串
string& assign(int n, char c);//用n个字符c赋给当前字符串
string& assign(const string &s, int start, int n);//将s从start开始n个字符赋值给字符串
```

##### string存取字符操作
```c++
char& operator[](int n);//通过[]方式取字符
char& at(int n);//通过at方法获取字符
```

##### string拼接操作
```c++
string& operator+=(const string& str);//重载+=操作符
string& operator+=(const char* str);//重载+共享锁=操作符
string& operator+=(const char c);//重载+=操作符
string& append(const char *s);//把字符串s连接到当前字符串结尾
string& append(const char *s, int n);//把字符串s的前n个字符连接到当前字符串结尾
string& append(const string &s);//同operator+=()
string& append(const string &s, int pos, int n);//把字符串s中从pos开始的n个字符连接到当前字符串结尾
string& append(int n, char c);//在当前字符串结尾添加n个字符c
```

##### string查找和替换
```c++
int find(const string& str, int pos = 0) const; //查找str第一次出现位置,从pos开始查找
int find(const char* s, int pos = 0) const;  //查找s第一次出现位置,从pos开始查找
int find(const char* s, int pos, int n) const;  //从pos位置查找s的前n个字符第一次位置
int find(const char c, int pos = 0) const;  //查找字符c第一次出现位置
int rfind(const string& str, int pos = npos) const;//查找str最后一次位置,从pos开始查找
int rfind(const char* s, int pos = npos) const;//查找s最后一次出现位置,从pos开始查找
int rfind(const char* s, int pos, int n) const;//从pos查找s的前n个字符最后一次位置
int rfind(const char c, int pos = 0) const; //查找字符c最后一次出现位置
string& replace(int pos, int n, const string& str); //替换从pos开始n个字符为字符串str
string& replace(int pos, int n, const char* s); //替换从pos开始的n个字符为字符串s`
```

##### string比较操作
```c++
/*
compare函数在>时返回 1,<时返回 -1,==时返回 0。
比较区分大小写,比较时参考字典顺序,排越前面的越小。
大写的A比小写的a小。
*/
int compare(const string &s) const;//与字符串s比较
int compare(const char *s) const;//与字符串s比较
```

##### string子串
```c++
string substr(int pos = 0, int n = npos) const;//返回由pos开始的n个字符组成的字符串
```

##### string插入和删除操作
```c++
string& insert(int pos, const char* s); //插入字符串
string& insert(int pos, const string& str); //插入字符串
string& insert(int pos, int n, char c);//在指定位置插入n个字符c
string& erase(int pos, int n = npos);//删除从Pos开始的n个字符 
```


##### 3.1.2.9 string和c-style字符串转换
```c++
//string 转 char*
string str = "itcast";
const char* cstr = str.c_str();
//char* 转 string 
char* s = "itcast";
string str(s);
```

<font color=#ff0000>提示</font>:
  在c++中存在一个从`const char*`到string的隐式类型转换,却不存在从一个string对象到C_string的自动类型转换。对于string类型的字符串,可以通过c_str()函数返回string对象对应的C_string.
  通常,程序员在整个程序中应坚持使用string类对象,直到必须将内容转化为`char*`时才将其转换为C_string.

<font color=#ff0000>提示</font>:
   为了修改string字符串的内容,下标操作符`[]`和at都会返回字符的引用。但当字符串的内存被重新分配之后,可能发生错误.

```c++
	string s = "abcdefg";
	char& a = s[2];
	char& b = s[3];

	a = '1';
	b = '2';

	cout << s << endl;
	cout << (int*)s.c_str() << endl;

	s = "pppppppppppppppppppppppp";

	//a = '1';
	//b = '2';

	cout << s << endl;
	cout << (int*)s.c_str() << endl;
```

`cctype`头文件中定义了一组标准函数：

|      函数       |                   解释                   |
| :-----------: | :------------------------------------: |
| `isalnum(c)`  |             当`c`是字母或数字时为真              |
| `isalpha(c)`  |               当`c`是字母时为真               |
| `iscntrl(c)`  |              当`c`是控制字符时为真              |
| `isdigit(c)`  |               当`c`是数字时为真               |
| `isgraph(c)`  |            当`c`不是空格但可以打印时为真            |
| `islower(c)`  |              当`c`是小写字母时为真              |
| `isprint(c)`  |             当`c`是可打印字符时为真              |
| `ispunct(c)`  |              当`c`是标点符号时为真              |
| `isspace(c)`  | 当`c`是空白时为真（空格、横向制表符、纵向制表符、回车符、换行符、进纸符） |
| `isupper(c)`  |              当`c`是大写字母时为真              |
| `isxdigit(c)` |             当`c`是十六进制数字时为真             |
| `tolower(c)`  |     当`c`是大写字母,输出对应的小写字母；否则原样输出`c`      |
| `toupper(c)`  |     当`c`是小写字母,输出对应的大写字母；否则原样输出`c`      |

### vector容器
#### vector容器基本概念
vector的数据安排以及操作方式,与array非常相似,两者的唯一差别在于空间的运用的灵活性。Array是静态空间,一旦配置了就不能改变,要换大一点或者小一点的空间,可以,一切琐碎得由自己来,首先配置一块新的空间,然后将旧空间的数据搬往新空间,再释放原来的空间。Vector是<font color=#ff0000>动态空间</font>,随着元素的加入,它的内部机制会自动扩充空间以容纳新元素。因此vector的运用对于内存的合理利用与运用的灵活性有很大的帮助,我们再也不必害怕空间不足而一开始就要求一个大块头的array了。
Vector的实现技术,关键在于其对大小的控制以及重新配置时的数据移动效率,一旦vector旧空间满了,如果客户每新增一个元素,vector内部只是扩充一个元素的空间,实为不智,因为所谓的扩充空间(不论多大),一如刚所说,是”配置新空间-数据移动-释放旧空间”的大工程,时间成本很高,应该加入某种未雨绸缪的考虑,稍后我们便可以看到vector的空间配置策略。

<img src="https://article.biliimg.com/bfs/article/840b6a2a990fa3daeeced318ae38fdf738716159.png" alt="image.png" style="zoom:50%;" />


#### vector迭代器
Vector维护一个线性空间,所以不论元素的型别如何,普通指针都可以作为vector的迭代器,因为vector迭代器所需要的操作行为,如operaroe*, operator->, operator++, operator--, operator+, operator-, operator+=, operator-=, 普通指针天生具备。Vector支持随机存取,而普通指针正有着这样的能力。所以vector提供的是**随机访问迭代器**(Random Access Iterators).
根据上述描述,如果我们写如下的代码：

```c++
Vector<int>::iterator it1;
Vector<Teacher>::iterator it2;
it1的型别其实就是Int*,it2的型别其实就是Teacher*.
```


```c++
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<vector>
using namespace std;

int main(){

	vector<int> v;
	for (int i = 0; i < 10;i ++){
		v.push_back(i);
		cout << v.capacity() << endl;  // v.capacity()容器的容量
	}
	system("pause");
	return EXIT_SUCCESS;
}
```

#### vector的数据结构

Vector所采用的数据结构非常简单,线性连续空间,它以两个迭代器_Myfirst和_Mylast分别指向配置得来的连续空间中目前已被使用的范围,并以迭代器_Myend指向整块连续内存空间的尾端。
为了降低空间配置时的速度成本,vector实际配置的大小可能比客户端需求大一些,以备将来可能的扩充,这边是容量的概念。换句话说,<font color=#ff0000>一个vector的容量永远大于或等于其大小,一旦容量等于大小,便是满载,下次再有新增元素,整个vector容器就得另觅居所</font>。

**注意**：
   所谓动态增加大小,并不是在原空间之后续接新空间(因为无法保证原空间之后尚有可配置的空间),而是一块更大的内存空间,然后将原数据拷贝新空间,并释放原空间。因此,对vector的任何操作,一旦引起空间的重新配置,<font color=#ff0000>指向原vector的所有迭代器就都失效了</font>。这是程序员容易犯的一个错误,务必小心。

#### vector常用API操作
##### vector构造函数
```c++
vector<T> v; //采用模板实现类实现,默认构造函数
vector(v.begin(), v.end());//将v[begin(), end())区间中的元素拷贝给本身。
vector(n, elem);//构造函数将n个elem拷贝给本身。
vector(const vector &vec);//拷贝构造函数。 不会拷贝

//例子 使用第二个构造函数 我们可以...
int arr[] = {2,3,4,1,9};
vector<int> v1(arr, arr + sizeof(arr) / sizeof(int)); 
```

##### vector常用赋值操作
```c++
assign(beg, end);//将[beg, end)区间中的数据拷贝赋值给本身。
assign(n, elem);//将n个elem拷贝赋值给本身。
vector& operator=(const vector  &vec);//重载等号操作符capacity()
swap(vec);// 将vec与本身的元素互换。 利用指针进行
```

##### vector大小操作
```c++
size();//返回容器中元素的个数
empty();//判断容器是否为空
resize(int num);//重新指定容器的长度为num,若容器变长,则以默认值填充新位置。如果容器变短,则末尾超出容器长度的元素被删除。
resize(int num, elem);//重新指定容器的长度为num,若容器变长,则以elem值填充新位置。如果容器变短,则末尾超出容器长>度的元素被删除。
capacity();//容器的容量
reserve(int len);//容器预留len个元素长度,预留位置不初始化,元素不可访问。
shrink_to_fit(); //将capacity()减少到和size()相同大小
```

##### vector数据存取操作
```c++
at(int idx); //返回索引idx所指的数据,如果idx越界,抛出out_of_range异常。
operator[];//返回索引idx所指的数据,越界时,运行直接报错
front();//返回容器中第一个数据元素
back();//返回容器中最后一个数据元素


```

##### vector插入和删除操作
```c++
insert(const_iterator pos, int count,ele);//迭代器指向位置pos插入count个元素ele.
push_back(ele); //尾部插入元素ele
pop_back();//删除最后一个元素
erase(const_iterator start, const_iterator end);//删除迭代器从start到end之间的元素
erase(const_iterator pos);//删除迭代器指向的元素
clear();//删除容器中所有元素
```


#### vector小案例
##### 巧用swap,收缩内存空间

```c++
#include<iostream>
#include<vector>
using namespace std;
int main(){
	vector<int> v;
	for (int i = 0; i < 100000;i ++){
		v.push_back(i);
	}

	cout << "capacity:" << v.capacity() << endl;
	cout << "size:" << v.size() << endl;

	//此时 通过resize改变容器大小
	v.resize(10);

	cout << "capacity:" << v.capacity() << endl;
	cout << "size:" << v.size() << endl;

	//容量没有改变
	vector<int>(v).swap(v);//v.shrink_to_fit();

	cout << "capacity:" << v.capacity() << endl;
	cout << "size:" << v.size() << endl;


	system("pause");
	return EXIT_SUCCESS;
}`
```


##### reserve预留空间
```c++
#include<iostream>
#include<vector>
using namespace std;

int main(){
	vector<int> v;
	//预先开辟空间
	v.reserve(100000);
	int* pStart = NULL;
	int count = 0;
	for (int i = 0; i < 100000;i ++){
		v.push_back(i);
		if (pStart != &v[0]){
			pStart = &v[0];
			count++;
		}
	}
	cout << "count:" << count << endl;
	system("pause");
	return EXIT_SUCCESS;
}
```


### deque容器
#### deque容器基本概念
Vector容器是单向开口的连续内存空间,deque则是一种双向开口的连续线性空间。所谓的双向开口,意思是可以在头尾两端分别做元素的插入和删除操作,当然,vector容器也可以在头尾两端插入元素,但是在其头部操作效率奇差,无法被接受。
 
<img src="https://article.biliimg.com/bfs/article/e666b3c9722fd8aed16081e09f2a07be38716159.png" alt="image.png" style="zoom:50%;" />


Deque容器和vector容器最大的差异,一在于deque允许使用常数项时间对头端进行元素的插入和删除操作。二在于deque没有容量的概念,因为它是动态的**以分段连续空间组合**而成,随时可以增加一段新的空间并链接起来,换句话说,像vector那样,”旧空间不足而重新配置一块更大空间,然后复制元素,再释放旧空间”这样的事情在deque身上是不会发生的。也因此,deque没有必须要提供所谓的空间保留(reserve)功能.
虽然deque容器也提供了Random Access Iterator,但是它的迭代器并不是普通的指针,**其复杂度和vector不是一个量级**,这当然影响各个运算的层面。因此,除非有必要,我们应该尽可能的使用vector,而不是deque。对deque进行的排序操作,为了最高效率,可将deque先完整的复制到一个vector中,对vector容器进行排序,再复制回deque.
#### deque容器实现原理
Deque容器是连续的空间,至少逻辑上看来如此,连续现行空间总是令我们联想到array和vector,array无法成长,vector虽可成长,却只能向尾端成长,而且其成长其实是一个假象,事实上(1) 申请更大空间 (2)原数据复制新空间 (3)释放原空间 三步骤,如果不是vector每次配置新的空间时都留有余裕,其成长假象所带来的代价是非常昂贵的。
Deque是由一段一段的定量的连续空间构成。一旦有必要在deque前端或者尾端增加新的空间,便配置一段连续定量的空间,串接在deque的头端或者尾端。Deque最大的工作就是维护这些分段连续的内存空间的整体性的假象,并提供随机存取的接口,避开了重新配置空间,复制,释放的轮回,代价就是复杂的迭代器架构。
既然deque是分段连续内存空间,那么就必须有中央控制,维持整体连续的假象,数据结构的设计及迭代器的前进后退操作颇为繁琐。Deque代码的实现远比vector或list都多得多。
Deque采取一块所谓的map(注意,不是STL的map容器)**作为主控**,这里所谓的map是一小块连续的内存空间,其中每一个元素(此处成为一个结点)都是一个指针,指向另一段连续性内存空间,称作缓冲区。缓冲区才是deque的存储空间的主体。
 
<img src="https://article.biliimg.com/bfs/article/7333c8b82f8e44dc14528e83915b2ecf38716159.png" alt="image.png" style="zoom:50%;" />


#### deque常用API
##### deque构造函数
```c++
deque<T> deqT;//默认构造形式
deque(beg, end);//构造函数将[beg, end)区间中的元素拷贝给本身。
deque(n, elem);//构造函数将n个elem拷贝给本身。
deque(const deque &deq);//拷贝构造函数。
```

##### deque赋值操作
```c++
assign(beg, end);//将[beg, end)区间中的数据拷贝赋值给本身。
assign(n, elem);//将n个elem拷贝赋值给本身。
deque& operator=(const deque &deq); //重载等号操作符 
swap(deq);// 将deq与本身的元素互换
```

##### deque大小操作
```c++
deque.size();//返回容器中元素的个数
deque.empty();//判断容器是否为空
deque.resize(num);//重新指定容器的长度为num,若容器变长,则以默认值填充新位置。如果容器变短,则末尾超出容器长度的元素被删除。
deque.resize(num, elem); //重新指定容器的长度为num,若容器变长,则以elem值填充新位置,如果容器变短,则末尾超出容器长度的元素被删除。
```

##### deque双端插入和删除操作
```c++
push_back(elem);//在容器尾部添加一个数据
push_front(elem);//在容器头部插入一个数据
pop_back();//删除容器最后一个数据
pop_front();//删除容器第一个数据
```

##### deque数据存取
```c++
at(idx);//返回索引idx所指的数据,如果idx越界,抛出out_of_range。
operator[];//返回索引idx所指的数据,如果idx越界,不抛出异常,直接出错。
front();//返回第一个数据。
back();//返回最后一个数据
```
##### deque插入操作
```c++
insert(pos,elem);//在pos位置插入一个elem元素的拷贝,返回新数据的位置。
insert(pos,n,elem);//在pos位置插入n个elem数据,无返回值。
insert(pos,beg,end);//在pos位置插入[beg,end)区间的数据,无返回值。
```

##### deque删除操作
```c++
clear();//移除容器的所有数据
erase(beg,end);//删除[beg,end)区间的数据,返回下一个数据的位置。
erase(pos);//删除pos位置的数据,返回下一个数据的位置。
```



### list容器
#### list容器基本概念

链表是一种物理存储单元上非连续、非顺序的存储结构,数据元素的逻辑顺序是通过链表中的指针链接次序实现的。链表由一系列结点（链表中每一个元素称为结点）组成,结点可以在运行时动态生成。每个结点包括两个部分：一个是存储数据元素的数据域,另一个是存储下一个结点地址的指针域。
相较于vector的连续线性空间,list就显得负责许多,它的好处是每次插入或者删除一个元素,就是配置或者释放一个元素的空间。因此,list对于空间的运用有绝对的精准,一点也不浪费。而且,对于任何位置的元素插入或元素的移除,list永远是常数时间。
List和vector是两个最常被使用的容器。
List容器是一个**双向链表**。

<img src="https://article.biliimg.com/bfs/article/92130144b7afd87debaa950aaf28d98f38716159.png" alt="image.png" style="zoom:50%;" />


- 采用动态存储分配,不会造成内存浪费和溢出
- 链表执行插入和删除操作十分方便,修改指针即可,不需要移动大量元素
- 链表灵活,但是空间和时间额外耗费较大

#### list容器的迭代器

List容器不能像vector一样以普通指针作为迭代器,因为其节点不能保证在同一块连续的内存空间上。List迭代器必须有能力指向list的节点,并有能力进行正确的递增、递减、取值、成员存取操作。所谓”list正确的递增,递减、取值、成员取用”是指,递增时指向下一个节点,递减时指向上一个节点,取值时取的是节点的数据值,成员取用时取的是节点的成员。
由于list是一个双向链表,迭代器必须能够具备前移、后移的能力,所以list容器提供的是Bidirectional Iterators.
List有一个重要的性质,插入操作和删除操作都不会造成原有list迭代器的失效。这在vector是不成立的,因为vector的插入操作可能造成记忆体重新配置,导致原有的迭代器全部失效,甚至List元素的删除,也只有被删除的那个元素的迭代器失效,其他迭代器不受任何影响。
#### list常用API
##### list构造函数
```c++
list<T> lstT;//list采用采用模板类实现,对象的默认构造形式：
list(beg,end);//构造函数将[beg, end)区间中的元素拷贝给本身。
list(n,elem);//构造函数将n个elem拷贝给本身。
list(const list &lst);//拷贝构造函数。
```
##### list数据元素插入和删除操作
```c++
push_back(elem);//在容器尾部加入一个元素
pop_back();//删除容器中最后一个元素
push_front(elem);//在容器开头插入一个元素
pop_front();//从容器开头移除第一个元素
insert(pos,elem);//在pos位置插elem元素的拷贝,返回新数据的位置。
insert(pos,n,elem);//在pos位置插入n个elem数据,无返回值。
insert(pos,beg,end);//在pos位置插入[beg,end)区间的数据,无返回值。
clear();//移除容器的所有数据
erase(beg,end);//删除[beg,end)区间的数据,返回下一个数据的位置。
erase(pos);//删除pos位置的数据,返回下一个数据的位置。
remove(elem);//删除容器中所有与elem值匹配的元素。
```

##### list大小操作
```c++
size();//返回容器中元素的个数
empty();//判断容器是否为空
resize(num);//重新指定容器的长度为num,
若容器变长,则以默认值填充新位置。
如果容器变短,则末尾超出容器长度的元素被删除。
resize(num, elem);//重新指定容器的长度为num,
若容器变长,则以elem值填充新位置。
如果容器变短,则末尾超出容器长度的元素被删除。
```

##### list赋值操作
```c++
assign(beg, end);//将[beg, end)区间中的数据拷贝赋值给本身。
assign(n, elem);//将n个elem拷贝赋值给本身。
list& operator=(const list &lst);//重载等号操作符
swap(lst);//将lst与本身的元素互换。
```
##### list数据的存取
```c++
front();//返回第一个元素。
back();//返回最后一个元素。
```
##### list反转排序
```c++
reverse();//反转链表,比如lst包含1,3,5元素,运行此方法后,lst就包含5,3,1元素。
sort(); //list排序
```


`splice` 函数是 C++ 标准库中 `std::list` 容器的成员函数，用于将一个列表中的元素移动到另一个列表中。


```c++
void splice(iterator position, list& x); // 将整个列表的元素移动到另一个列表中
void splice(iterator position, list& x, iterator i); // 将另一个列表中的一个元素移动到当前位置
void splice(iterator position, list& x, iterator first, iterator last); // 将另一个列表中的某个范围的元素移动到当前位置
```

### forward_list

单向链表。只支持单向顺序访问。在链表任何位置进行插入/删除操作速度都很快。
1. 存储结构：`std::forward_list` 是单向链表,而 `std::list` 是双向链表。
2. 内存占用：由于 `std::list` 是双向链表,它需要额外的空间来存储向后的指针,而 `std::forward_list` 只需要存储向前的指针。
3. 功能：因为 `std::forward_list` 是单向链表,所以它没有提供像 `std::list` 那样的双向迭代和其他相关的功能。

```c++
#include <iostream>
#include <forward_list>

int main() {
    std::forward_list<int> flist = {1, 2, 3, 4, 5};
    // 插入元素
    flist.push_front(0);
    // 遍历并打印
    for (int x : flist) {
        std::cout << x << " ";
    }
    std::cout << std::endl;
    // 注意：forward_list 没有 push_back 或者 pop_back 函数
    // 因为它是单向链表,只能从前端添加或删除
    return 0;
}
```

| 操作                          | 解释                                                                     |
|:---------------------------:|:----------------------------------------------------------------------:|
| `lst.before_begin()`        | 返回指向链表首元素之前不存在的元素的迭代器,此迭代器不能解引用。                                       |
| `lst.cbefore_begin()`       | 同上,但是返回的是常量迭代器。                                                        |
| `lst.insert_after(p, t)`    | 在迭代器`p`之后插入元素。`t`是一个对象                                                 |
| `lst.insert_after(p, n, t)` | 在迭代器`p`之后插入元素。`t`是一个对象,`n`是数量。若`n`是0则函数行为未定义                           |
| `lst.insert_after(p, b, e)` | 在迭代器`p`之后插入元素。由迭代器`b`和`e`指定范围。                                         |
| `lst.insert_after(p, il)`   | 在迭代器`p`之后插入元素。由`il`指定初始化列表。                                            |
| `emplace_after(p, args)`    | 使用`args`在`p`之后的位置,创建一个元素,返回一个指向这个新元素的迭代器。若`p`为尾后迭代器,则函数行为未定义。          |
| `lst.erase_after(p)`        | 删除`p`指向位置之后的元素,返回一个指向被删元素之后的元素的迭代器,若`p`指向`lst`的尾元素或者是一个尾后迭代器,则函数行为未定义。 |
| `lst.erase_after(b, e)`     | 类似上面,删除对象换成从`b`到`e`指定的范围。                                              |  

### set/multiset容器
#### set/multiset容器基本概念
##### set容器基本概念
Set的特性是。所有元素都会根据元素的键值自动被排序。Set的元素不像map那样可以同时拥有实值和键值,set的元素即是键值又是实值。Set不允许两个元素有相同的键值。
我们可以通过set的迭代器改变set元素的值吗？不行,因为set元素值就是其键值,关系到set元素的排序规则。如果任意改变set元素值,会严重破坏set组织。换句话说,set的iterator是一种`const_iterator`.
set拥有和list某些相同的性质,当对容器中的元素进行插入操作或者删除操作的时候,操作之前所有的迭代器,在操作完成之后依然有效,被删除的那个元素的迭代器必然是一个例外。
##### multiset容器基本概念
multiset特性及用法和set完全相同,唯一的差别在于它**允许键值重复**。set和multiset的底层实现是**红黑树**,红黑树为平衡二叉树的一种。
树的简单知识：
二叉树就是任何节点最多只允许有两个字节点。分别是左子结点和右子节点。

<img src="https://article.biliimg.com/bfs/article/beb44ebd028bad827480969736e0d93838716159.png" alt="image.png" style="zoom:60%;" />


[[红黑树]]
 
#### set常用API
##### set构造函数
```c=+
set<T> st;//set默认构造函数：
mulitset<T> mst; //multiset默认构造函数: 
set(const set &st);//拷贝构造函数
```
##### set赋值操作
```c++
set& operator=(const set &st);//重载等号操作符
swap(st);//交换两个集合容器
```
##### set大小操作
```c++
size();//返回容器中元素的数目
empty();//判断容器是否为空
```

##### set插入和删除操作
```c++
insert(elem);//在容器中插入元素。
clear();//清除所有元素
erase(pos);//删除pos迭代器所指的元素,返回下一个元素的迭代器。
erase(beg, end);//删除区间[beg,end)的所有元素 ,返回下一个元素的迭代器。
erase(elem);//删除容器中值为elem的元素。
```
##### set查找操作
```c++
find(key);//查找键key是否存在,若存在,返回该键的元素的迭代器；若不存在,返回set.end();
count(key);//查找键key的元素个数
lower_bound(keyElem);//返回第一个key>=keyElem元素的迭代器。
upper_bound(keyElem);//返回第一个key>keyElem元素的迭代器。
equal_range(keyElem);//返回容器中key与keyElem相等的上下限的两个迭代器。
```

**set的返回值    指定set排序规则**:

```c++
//插入操作返回值
void test01(){

	set<int> s;
	pair<set<int>::iterator,bool> ret = s.insert(10);
	if (ret.second){
		cout << "插入成功:" << *ret.first << endl;
	}
	else{
		cout << "插入失败:" << *ret.first << endl;
	}
	
	ret = s.insert(10);
	if(ret.second){
		cout << "插入成功:" << *ret.first << endl;
	}
	else{
		cout << "插入失败:" << *ret.first << endl;
	}

}

struct MyCompare02{
	bool operator()(int v1,int v2){
		return v1 > v2;
	}
};

//set从大到小
void test02(){

	srand((unsigned int)time(NULL));
	//我们发现set容器的第二个模板参数可以设置排序规则,默认规则是less<_Kty>
	set<int, MyCompare02> s;
	for (int i = 0; i < 10;i++){
		s.insert(rand() % 100);
	}
	
	for (set<int, MyCompare02>::iterator it = s.begin(); it != s.end(); it ++){
		cout << *it << " ";
	}
	cout << endl;
}

//set容器中存放对象
class Person{
public:
	Person(string name,int age){
		this->mName = name;
		this->mAge = age;
	}
public:
	string mName;
	int mAge;
};


struct MyCompare03{
	bool operator()(const Person& p1,const Person& p2){
		return p1.mAge > p2.mAge;
	}
};

void test03(){

	set<Person, MyCompare03> s;

	Person p1("aaa", 20);
	Person p2("bbb", 30);
	Person p3("ccc", 40);
	Person p4("ddd", 50);

	s.insert(p1);
	s.insert(p2);
	s.insert(p3);
	s.insert(p4);

	for (set<Person, MyCompare03>::iterator it = s.begin(); it != s.end(); it++){
		cout << "Name:" << it->mName << " Age:" << it->mAge << endl;
	}

}
```

#### 对组(pair) 
对组(pair)将一对值组合成一个值,这一对值可以具有不同的数据类型,两个值可以分别用pair的两个公有属性first和second访问。
类模板：`template <class T1, class T2> struct pair`.
如何创建对组?
```c++
//第一种方法创建一个对组
pair<string, int> pair1(string("name"), 20);
cout << pair1.first << endl; //访问pair第一个值
cout << pair1.second << endl;//访问pair第二个值
//第二种
pair<string, int> pair2 = make_pair("name", 30);
cout << pair2.first << endl;
cout << pair2.second << endl;
//pair=赋值
pair<string, int> pair3 = pair2;
cout << pair3.first << endl;
cout << pair3.second << endl; 
```

### map/multimap容器
#### map/multimap基本概念

1. map元素都是pair,所有的元素都会根据元素的键值自动排序
2. map不允许有两个相同的键值
3. `multimap`和map的操作类似,唯一区别multimap键值可重复。
4. map和multimap都是以红黑树为底层实现机制。

#### map/multimap常用API
##### map构造函数
```c++
map<T1, T2> mapTT;//map默认构造函数: 
map(const map &mp);//拷贝构造函数
```

##### map赋值操作
```c++
map& operator=(const map &mp);//重载等号操作符
swap(mp);//交换两个集合容器
```

##### map大小操作
```c++
size();//返回容器中元素的数目
empty();//判断容器是否为空
```
##### map插入数据元素操作
```c++
map.insert(...); //往容器插入元素,返回pair<iterator,bool>
map<int, string> mapStu;
// 第一种 通过pair的方式插入对象
mapStu.insert(pair<int, string>(3, "小张"));
// 第二种 通过pair的方式插入对象
mapStu.inset(make_pair(-1, "校长"));
// 第三种 通过value_type的方式插入对象
mapStu.insert(map<int, string>::value_type(1, "小李"));
// 第四种 通过数组的方式插入值
mapStu[3] = "小刘";
mapStu[5] = "小王";
```
##### map删除操作
```c++
clear();//删除所有元素
erase(pos);//删除pos迭代器所指的元素,返回下一个元素的迭代器。
erase(beg,end);//删除区间[beg,end)的所有元素 ,返回下一个元素的迭代器。
erase(keyElem);//删除容器中key为keyElem的对组。
```
##### map查找操作
```c++
find(key);//查找键key是否存在,若存在,返回该键的元素的迭代器；/若不存在,返回map.end();
count(keyElem);//返回容器中key为keyElem的对组个数。对map来说,要么是0,要么是1。对multimap来说,值可能大于1。
lower_bound(keyElem);//返回第一个key>=keyElem元素的迭代器。
upper_bound(keyElem);//返回第一个key>keyElem元素的迭代器。
equal_range(keyElem);//返回容器中key与keyElem相等的上下限的两个迭代器。
```


unordered_map定义的key只能是基本数据类型，如果要自定义，那就得自己写hash函数

```c++
    auto pair_hash = [](const std::pair<int,int>& p) {
        return std::hash<int>{}(p.first) ^ (std::hash<int>{}(p.second) << 1);
    };
```

```c++
  std::unordered_map<std::pair<int,int>, int, decltype(pair_hash)> myMap(0, pair_hash);
  // 插入元素
  myMap[{1, 2}] = 100;
  myMap[{3, 4}] = 200;
```



### STL容器使用时机

<img src="https://article.biliimg.com/bfs/article/c40b24b663671ab261a7b5c3de061f9f38716159.png" alt="image.png" style="zoom:60%;" />


- vector的使用场景：比如软件历史操作记录的存储,我们经常要查看历史记录,比如上一次的记录,上上次的记录,但却不会去删除记录,因为记录是事实的描述。
- deque的使用场景：比如排队购票系统,对排队者的存储可以采用deque,支持头端的快速移除,尾端的快速添加。如果采用vector,则头端移除时,会移动大量的数据,速度慢。

## 适配器

```c++
template<typename T,typename Container=std::deque<T>>
class Stack{
private:
    Container elems;
public:
    void push(const T &val) {elems.push_back(val);}
    void pop() {elems.pop_back();}
    T top() const{return elems.back();}

};
```


1. 适配器底层没有自己的数据结构，它是另外一个**容器**的封装，它的方法，全部由底层依赖的容器进行实现的 
2. 没有实现自己的迭代器 
### stack容器
#### stack容器基本概念
stack是一种先进后出(First In Last Out,FILO)的数据结构,它只有一个出口,形式如图所示。stack容器允许新增元素,移除元素,取得栈顶元素,但是除了最顶端外,没有任何其他方法可以存取stack的其他元素。换言之,stack不允许有遍历行为。
有元素推入栈的操作称为:push,将元素推出stack的操作称为pop.


<img src="https://article.biliimg.com/bfs/article/dec26061b10e08f978ad0575f288fbde38716159.png" alt="image.png" style="zoom:50%;" />


 
#### stack没有迭代器
Stack所有元素的进出都必须符合”**先进后出**”的条件,只有stack顶端的元素,才有机会被外界取用。Stack不提供遍历功能,也不提供迭代器。
#### stack常用API
##### stack构造函数
```C++
stack<T> stkT;//stack采用模板类实现, stack对象的默认构造形式： 
stack(const stack &stk);//拷贝构造函数
```

##### stack赋值操作
```C++
stack& operator=(const stack &stk);//重载等号操作符
```
##### stack数据存取操作
```C++
push(elem);//向栈顶添加元素
pop();//从栈顶移除第一个元素
top();//返回栈顶元素
```
##### stack大小操作

```c++
empty();//判断堆栈是否为空
size();//返回堆栈的大小
```


### queue容器
#### 容器基本概念

Queue是一种先进先出(First In First Out,FIFO)的数据结构,它有两个出口,queue容器允许从一端新增元素,从另一端移除元素。

#### queue没有迭代器
Queue所有元素的进出都必须符合”先进先出”的条件,只有queue的顶端元素,才有机会被外界取用。Queue不提供遍历功能,也不提供迭代器。

#### queue常用API
##### queue构造函数
```c++
queue<T> queT;//queue采用模板类实现,queue对象的默认构造形式：
queue(const queue &que);//拷贝构造函数
```
##### queue存取、插入和删除操作
```c++
push(elem);//往队尾添加元素
pop();//从队头移除第一个元素
back();//返回最后一个元素
front();//返回第一个元素
```

##### queue赋值操作
```c++
queue& operator=(const queue &que);//重载等号操作符
```
##### queue大小操作
```c++
empty();//判断队列是否为空
size();//返回队列的大小
```

### priority_queue

`priority_queue` 在 C++ 标准模板库 (STL) 中是基于堆（heap）数据结构实现的。特别是,它是基于一个二叉堆实现的,是一种特殊的队列,它能够在队列中进行排序（堆排序,底层实现结构是vector 或者deque; 
使得每次可以访问队列中的最大（或最小,取决于所提供的比较操作）元素。元素的添加可以在任何位置,但每次移除操作都会移除当前最大（或最小）的元素。

`priority_queue`: 提供了 `push`, `pop`, `top`, `empty`, `size` 等成员函数。


> [!help] 一些问题
> - queue => deque 为什么不依赖vector呢？??  stack=>deque 为什么不依赖vector呢？??
> 	1. vector的初始内存使用效率太低了！没有deque好 `queue<int>` `stack<int>` vector 0-1-2-4-8 deque 40 
> 	2. 对于queue来说，需要支持尾部插入，头部删除，O(1)  如果queue依赖vector,其出队效率很低 
> 	3. vector需要**大片的连续内存**，而deque只需要分段的内存，当存储大量数据时，显然deque对于内存的利用率更好一些 
> 
> - priority_queue=>vector为什么依赖vector??? 
> 	1. priority queue: push入队 pop出队 top查看队顶元素 empty判断队空 
> 	2. size返回元素个数 默认：大根堆 
> 	3. 底层默认把数据组成一个大根堆结构在一个**内存连续的数组**上构建一个大根堆或者小根堆的 

```c++
//小顶堆
priority_queue <int,vector<int>,greater<int> > q;
//大顶堆
priority_queue <int,vector<int>,less<int> >q;
//默认大顶堆
priority_queue<int> a;
```

## 函数对象

### 函数对象的使用场景

```c++
template<typename T>
bool myless(T a, T b) {
  return a < b;
}
template<typename T>
bool mygreter(T a,T b){
  return a > b;
}


template<typename T,typename funciton>
bool compare(T a,T b,funciton f){
  return f(a,b);
}

int main() {
  std::cout<<compare(4,2,myless<int>)<<std::endl;
  return 0;
}
```

- 上面的通过函数指针调用函数，是没有办法内联的，效率很低，因为有函数调用开销。
- <font color=#ff0000>之所以无法内联是因为函数指针的一个关键特征是它的间接性。当您通过函数指针调用一个函数时，编译器通常无法在编译时确定将要调用哪个具体的函数。这是因为函数指针的值可能在运行时改变，因此编译器通常无法在编译阶段进行有效的内联优化</font>。

我们可以使用**函数对象**

重载函数调用操作符的类,其对象常称为**函数对象**（function object）,即它们是行为类似函数的对象,也叫仿函数(functor),其实就是重载“()”操作符,使得类对象可以像函数那样调用。
<font color=#ff0000>注意</font>:
1. 函数对象(仿函数)是一个类,不是一个函数。
2. 函数对象(仿函数)重载了”() ”操作符使得它可以像函数一样调用。


```c++
template<typename T>
class myless {
public:
    bool operator()(T a, T b) {
    return a < b;
  }
};
template<typename T>
class mygreter {
public:
    bool operator()(T a, T b) {
      return a>b;
    }
};

template<typename T,typename funciton>
bool compare(T a,T b,funciton f){
  return f(a,b);
}

int main() {
//  std::cout<<compare(4,2,myless<int>)<<std::endl;
  std::cout<<compare(4,2,myless<int>())<<std::endl;

  return 0;
}
```

- 使用函数对象时，您实际上是传递了一个具体的类的实例。这个类的 `operator()` 是一个普通的成员函数。当模板函数 `compare` 被实例化时，编译器能够看到**完整的函数定义**，这使得它能够在编译时进行内联替换。
- 函数对象的行为在编译时是已知的，因为您传递的是一个具体类型的对象。编译器可以查看这个类型的 `operator()` 实现，并直接在调用点进行内联展开。


分类:假定某个类有一个重载的`operator()`,而且重载的`operator()`要求获取一个参数,我们就将这个类称为“一元仿函数”（unary functor）；相反,如果重载的operator()要求获取两个参数,就将这个类称为“二元仿函数”（binary functor）。
函数对象的作用主要是什么？STL提供的算法往往都有两个版本,其中一个版本表现出最常用的某种运算,另一版本则允许用户通过template参数的形式来指定所要采取的策略。

```c++
//函数对象是重载了函数调用符号的类
class MyPrint
{
public:
	MyPrint()
	{
		m_Num = 0;
	}
	int m_Num;
public:
	void operator() (int num)
	{
		cout << num << endl;
		m_Num++;
	}
};

//函数对象
//重载了()操作符的类实例化的对象,可以像普通函数那样调用,可以有参数 ,可以有返回值
void test01()
{
	MyPrint myPrint;
	myPrint(20);

}
// 函数对象超出了普通函数的概念,可以保存函数的调用状态
void test02()
{
	MyPrint myPrint;
	myPrint(20);
	myPrint(20);
	myPrint(20);
	cout << myPrint.m_Num << endl;
}

void doBusiness(MyPrint print,int num)
{
	print(num);
}

//函数对象作为参数
void test03()
{
	//参数1：匿名函数对象
	doBusiness(MyPrint(),30);
}
```


**总结**：
1、函数对象通常不定义构造函数和析构函数,所以在构造和析构时不会发生任何问题,避免了函数调用的运行时问题。
2、函数对象超出普通函数的概念,函数对象可以有自己的状态
3、函数对象可内联编译,性能好。用函数指针几乎不可能
4、模版函数对象使函数对象具有通用性,这也是它的优势之一  

### 谓词
谓词是指<font color=#ff0000>普通函数</font>或<font color=#ff0000>重载的operator()</font>返回值是bool类型的函数对象(仿函数)。如果operator接受一个参数,那么叫做一元谓词,如果接受两个参数,那么叫做二元谓词,谓词可作为一个判断式。
```c++
class GreaterThenFive
{
public:
	bool operator()(int num)
	{
		return num > 5;
	}

};
//一元谓词
void test01()
{
	vector<int> v;
	for (int i = 0; i < 10;i ++)
	{
		v.push_back(i);
	}
	
	 vector<int>::iterator it =  find_if(v.begin(), v.end(), GreaterThenFive());
	 if (it == v.end())
	 {
		 cout << "没有找到" << endl;
	 }
	 else
	 {
		 cout << "找到了: " << *it << endl;
	 }
}

//二元谓词
class MyCompare
{
public:
	bool operator()(int num1, int num2)
	{
		return num1 > num2;
	}
};

void test02()
{
	vector<int> v;
	v.push_back(10);
	v.push_back(40);
	v.push_back(20);
	v.push_back(90);
	v.push_back(60);

	//默认从小到大
	sort(v.begin(), v.end());
	for (vector<int>::iterator it = v.begin(); it != v.end();it++)
	{
		cout << *it << " ";
	}
	cout << endl;
	cout << "----------------------------" << endl;
	//使用函数对象改变算法策略,排序从大到小
	sort(v.begin(), v.end(),MyCompare());
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}
```



## 常用算法

### 内建函数对象

STL内建了一些**函数对象**。分为:算数类函数对象,关系运算类函数对象,逻辑运算类仿函数。这些仿函数所产生的对象,用法和一般函数完全相同,当然我们还可以产生无名的临时对象来履行函数功能。使用内建函数对象,需要引入头文件 `#include<functional>`。

- 6个算数类函数对象,除了negate是一元运算,其他都是二元运算。
```c++
template<class T> T plus<T>//加法仿函数
template<class T> T minus<T>//减法仿函数
template<class T> T multiplies<T>//乘法仿函数
template<class T> T divides<T>//除法仿函数
template<class T> T modulus<T>//取模仿函数
template<class T> T negate<T>//取反仿函数
```

- 6个关系运算类函数对象,每一种都是二元运算。
```c++
template<class T> bool equal_to<T>//等于
template<class T> bool not_equal_to<T>//不等于
template<class T> bool greater<T>//大于
template<class T> bool greater_equal<T>//大于等于
template<class T> bool less<T>//小于
template<class T> bool less_equal<T>//小于等于
```

- 逻辑运算类运算函数,not为一元运算,其余为二元运算。
```c++
template<class T> bool logical_and<T>//逻辑与
template<class T> bool logical_or<T>//逻辑或
template<class T> bool logical_not<T>//逻辑非
```

内建函数对象举例:
```c++
//取反仿函数
void test01()
{
	negate<int> n;
	cout << n(50) << endl;
}

//加法仿函数
void test02()
{
	plus<int> p;
	cout << p(10, 20) << endl;
}

//大于仿函数
void test03()
{
	vector<int> v;
	srand((unsigned int)time(NULL));
	for (int i = 0; i < 10; i++){
		v.push_back(rand() % 100);
	}

	for (vector<int>::iterator it = v.begin(); it != v.end(); it++){
		cout << *it << " ";
	}
	cout << endl;
	sort(v.begin(), v.end(), greater<int>());

	for (vector<int>::iterator it = v.begin(); it != v.end(); it++){
		cout << *it << " ";
	}
	cout << endl;

}
```


#### 算法概述
算法主要是由头文件`<algorithm> <functional> <numeric>`组成。
`<algorithm>`是所有STL头文件中最大的一个,其中常用的功能涉及到比较,交换,查找,遍历,复制,修改,反转,排序,合并等...
`<numeric>`体积很小,只包括在几个序列容器上进行的简单运算的模板函数.
`<functional>` 定义了一些模板类,用以声明函数对象。
### 常用遍历算法
```c=+
/*
    遍历算法 遍历容器元素
	@param beg 开始迭代器
	@param end 结束迭代器
	@param _callback  函数回调或者函数对象
	@return 函数对象
*/
for_each(iterator beg, iterator end, _callback);
/*
	transform算法 将指定容器区间元素搬运到另一容器中
	注意 : transform 不会给目标容器分配内存,所以需要我们提前分配好内存
	@param beg1 源容器开始迭代器
	@param end1 源容器结束迭代器
	@param beg2 目标容器开始迭代器
	@param _cakkback 回调函数或者函数对象
	@return 返回目标容器迭代器
*/
transform(iterator beg1, iterator end1, iterator beg2, _callbakc)
```


for_each:
```c++
/*
template<class _InIt,class _Fn1> inline
void for_each(_InIt _First, _InIt _Last, _Fn1 _Func)
{
	for (; _First != _Last; ++_First)
		_Func(*_First);
}
*/
//普通函数
void print01(int val){
	cout << val << " ";
}
//函数对象
struct print001{
	void operator()(int val){
		cout << val << " ";
	}
};

//for_each算法基本用法
void test01(){
	
	vector<int> v;
	for (int i = 0; i < 10;i++){
		v.push_back(i);
	}

	//遍历算法
	for_each(v.begin(), v.end(), print01);
	cout << endl;

	for_each(v.begin(), v.end(), print001());
	cout << endl;

}
struct print02{
	print02(){
		mCount = 0;
	}
	void operator()(int val){
		cout << val << " ";
		mCount++;
	}
	int mCount;
};

//for_each返回值
void test02(){

	vector<int> v;
	for (int i = 0; i < 10; i++){
		v.push_back(i);
	}

	print02 p = for_each(v.begin(), v.end(), print02());
	cout << endl;
	cout << p.mCount << endl;
}

struct print03 : public binary_function<int, int, void>{
	void operator()(int val,int bindParam) const{
		cout << val + bindParam << " ";
	}
};

//for_each绑定参数输出
void test03(){
	
	vector<int> v;
	for (int i = 0; i < 10; i++){
		v.push_back(i);
	}

	for_each(v.begin(), v.end(), bind2nd(print03(),100));
}
```


transform:
```c++
//transform 将一个容器中的值搬运到另一个容器中
/*
	template<class _InIt, class _OutIt, class _Fn1> inline 
	_OutIt _Transform(_InIt _First, _InIt _Last,_OutIt _Dest, _Fn1 _Func)
	{	

		for (; _First != _Last; ++_First, ++_Dest)
			*_Dest = _Func(*_First);
		return (_Dest);
	}

	template<class _InIt1,class _InIt2,class _OutIt,class _Fn2> inline
	_OutIt _Transform(_InIt1 _First1, _InIt1 _Last1,_InIt2 _First2, _OutIt _Dest, _Fn2 _Func)
	{	
		for (; _First1 != _Last1; ++_First1, ++_First2, ++_Dest)
			*_Dest = _Func(*_First1, *_First2);
		return (_Dest);
	}
*/

struct transformTest01{
	int operator()(int val){
		return val + 100;
	}
};
struct print01{
	void operator()(int val){
		cout << val << " ";
	}
};
void test01(){

	vector<int> vSource;
	for (int i = 0; i < 10;i ++){
		vSource.push_back(i + 1);
	}

	//目标容器
	vector<int> vTarget;
	//给vTarget开辟空间
	vTarget.resize(vSource.size());
	//将vSource中的元素搬运到vTarget
	vector<int>::iterator it = transform(vSource.begin(), vSource.end(), vTarget.begin(), transformTest01());
	//打印
	for_each(vTarget.begin(), vTarget.end(), print01()); cout << endl;
	
}

//将容器1和容器2中的元素相加放入到第三个容器中
struct transformTest02{
	int operator()(int v1,int v2){
		return v1 + v2;
	}
};
void test02(){

	vector<int> vSource1;
	vector<int> vSource2;
	for (int i = 0; i < 10; i++){
		vSource1.push_back(i + 1);	
	}

	//目标容器
	vector<int> vTarget;
	//给vTarget开辟空间
	vTarget.resize(vSource1.size());
	transform(vSource1.begin(), vSource1.end(), vSource2.begin(),vTarget.begin(), transformTest02());
	//打印
	for_each(vTarget.begin(), vTarget.end(), print01()); cout << endl;
}
```

### 常用查找算法
```c++
/*
	find算法 查找元素
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param value 查找的元素
	@return 返回查找元素的位置
*/
find(iterator beg, iterator end, value)

/*
	find_if算法 条件查找
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param  callback 回调函数或者谓词(返回bool类型的函数对象)
	@return bool 查找到返回迭代器 否则end
*/
find_if(iterator beg, iterator end, _callback);
/* 与std::find_if()的使用方式类似，不同之处在于它查找的是第一个使得谓词p返回false的元素 */
find_if_not(iterator beg, iterator end, _callback)

/*
	adjacent_find算法 查找相邻重复元素
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param  _callback 回调函数或者谓词(返回bool类型的函数对象)
	@return 返回相邻元素的第一个位置的迭代器
*/
adjacent_find(iterator beg, iterator end, _callback);
/*
	binary_search算法 二分查找法
	注意: 在无序序列中不可用
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param value 查找的元素
	@return bool 查找返回true 否则false
*/
bool binary_search(iterator beg, iterator end, value);

/*
	count算法 统计元素出现次数
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param  value回调函数或者谓词(返回bool类型的函数对象)
	@return int返回元素个数
*/
count(iterator beg, iterator end, value);

/*
count_if算法 统计元素出现次数
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param  callback 回调函数或者谓词(返回bool类型的函数对象)
	@return int返回元素个数
*/
count_if(iterator beg, iterator end, _callback);

/*
可以在任何序列中搜索第一个匹配给定子序列的迭代器。
返回匹配的迭代器
*/
search( ForwardIt1 first, ForwardIt1 last, ForwardIt2 s_first, ForwardIt2 s_last);

std::vector<int> v = {1, 2, 3, 4, 5, 1, 2, 3, 4, 5};
std::vector<int> t = {1, 2, 3};

auto it = std::search(v.begin(), v.end(), t.begin(), t.end());

if (it != v.end()) {
    std::cout << "Found at position: " << std::distance(v.begin(), it) << std::endl;
} else {
    std::cout << "Not found" << std::endl;
}


find_first_of(Iterator first1, InputIterator last1, Iterator first2, Iterator __last2)
/*
输入：两对迭代器标记两段范围,在第一段中找第二段中任意元素,返回第一个匹配的元素,找不到返回第一段的`end`迭代器。
*/

```

### 常见的删除算法

```c++
//用于在序列中移除满足特定条件的元素,并返回移除后的新序列的尾迭代器。
remove_if(_ForwardIterator __first, _ForwardIterator __last, _Predicate __pred)
// 高效删除vector中下标的元素
vector v={1,2,3,4,5,6,7,8,9,10};  
auto newEnd=remove_if( v.begin(),v.end(),[&v](auto& num){return (&num-&v[0]+1)%2==0;});  
v.erase(newEnd,v.end());  
std::cout << "移除奇数后的序列：";  
for(auto& num:v){  
std::cout << num << " ";  
}
// 提供了一种方便的方法来删除容器中满足特定谓词条件的元素。例如,对于 `std::vector`、`std::deque`、`std::list` 和 `std::forward_list`
std::erase_if
```

### 常用排序算法
```c++
/*
	merge算法 容器元素合并,并存储到另一容器中
	注意:两个容器必须是有序的
	@param beg1 容器1开始迭代器
	@param end1 容器1结束迭代器
	@param beg2 容器2开始迭代器
	@param end2 容器2结束迭代器
	@param dest  目标容器开始迭代器
*/
merge(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest)
/*
	sort算法 容器元素排序
	@param beg 容器1开始迭代器
	@param end 容器1结束迭代器
	@param _callback 回调函数或者谓词(返回bool类型的函数对象)
*/
sort(iterator beg, iterator end, _callback)
/*
	random_shuffle算法 对指定范围内的元素随机调整次序
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
*/
random_shuffle(iterator beg, iterator end)
/*
	reverse算法 反转指定范围的元素
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
*/
reverse(iterator beg, iterator end)

unique(iterator beg, iterator end)
/*
用于移除相邻重复的元素。该函数接受一对迭代器作为参数,并返回一个指向新的逻辑末尾的迭代器。unique()函数通过比较相邻的元素,并将“重复”的元素移到容器的末尾,然后返回指向非重复部分的迭代器。
*/

/*
序列重排为字典序的下一个排列。如果可以形成更大的排列则返回 true,如果这已经是最大的排列（即不存在下一个排列）,则序列会被重置为最小的排列（即字典序最小的排列）,并返回 false。
*/

// 实现原理：
// 1.从给定的数字从右向左找到第一个下降点，即找到第一个数字，它比右边的数字小。
// 2.如果不存在这样的下降点，则表示数字已经是最大的排列，返回 -1。
// 3.否则，从右向左找到第一个比该下降点数字大的数字，交换它们。
// 4.最后，将交换位置后，下降点右边的数字翻转，使其变为最小的排列。
template< class BidirIt >
bool next_permutation( BidirIt first, BidirIt last );

int main() {
    std::vector<int> vec = {1, 2, 3}; // 完整的全排列必须有序

    do {
        // 打印当前排列
        for (int num : vec) {
            std::cout << num << ' ';
        }
        std::cout << '\n';
    } while (std::next_permutation(vec.begin(), vec.end())); // 生成下一个排列
    return 0;
}
```


### 常用拷贝和替换算法
```c++
/*
	copy算法 将容器内指定范围的元素拷贝到另一容器中
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param dest 目标起始迭代器
*/
copy(iterator beg, iterator end, iterator dest)
/*
	replace算法 将容器内指定范围的旧元素修改为新元素
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param oldvalue 旧元素
	@param oldvalue 新元素
*/
replace(iterator beg, iterator end, oldvalue, newvalue)
/*
	replace_if算法 将容器内指定范围满足条件的元素替换为新元素
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param callback函数回调或者谓词(返回Bool类型的函数对象)
	@param oldvalue 新元素
*/
replace_if(iterator beg, iterator end, _callback, newvalue)
/*
	swap算法 互换两个容器的元素
	@param c1容器1
	@param c2容器2
*/
swap(container c1, container c2)
```

### 常用算数生成算法
```c++
/*
	accumulate算法 计算容器元素累计总和
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param value累加值
*/
accumulate(iterator beg, iterator end, value)
/*
	fill算法 向容器中添加元素
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param value t填充元素
*/
fill(iterator beg, iterator end, value)
```


### 常用集合算法
```c++
/*
	set_intersection算法 求两个set集合的交集
	注意:两个集合必须是有序序列
	@param beg1 容器1开始迭代器
	@param end1 容器1结束迭代器
	@param beg2 容器2开始迭代器
	@param end2 容器2结束迭代器
	@param dest  目标容器开始迭代器
	@return 目标容器的最后一个元素的迭代器地址
*/
set_intersection(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest)
/*
	set_union算法 求两个set集合的并集
	注意:两个集合必须是有序序列
	@param beg1 容器1开始迭代器
	@param end1 容器1结束迭代器
	@param beg2 容器2开始迭代器
	@param end2 容器2结束迭代器
	@param dest  目标容器开始迭代器
	@return 目标容器的最后一个元素的迭代器地址
*/
set_union(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest)
/*
	set_difference算法 求两个set集合的差集
	注意:两个集合必须是有序序列
	@param beg1 容器1开始迭代器
	@param end1 容器1结束迭代器
	@param beg2 容器2开始迭代器
	@param end2 容器2结束迭代器
	@param dest  目标容器开始迭代器
	@return 目标容器的最后一个元素的迭代器地址
*/
set_difference(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest)
```

# C++11语法

## 指针空值类型 - nullptr

C++ 中将 NULL 定义为字面常量 0,并不能保证在所有场景下都能很好的工作,比如,函数重载时,`NULL` 和 `0` 无法区分：

```c++
#include <iostream>
using namespace std;
void func(char *p)
{
    cout << "void func(char *p)" << endl;
}
void func(int p)
{
    cout << "void func(int p)" << endl;
}
int main()
{
    func(NULL);   // 想要调用重载函数 void func(char *p)
    func(250);    // 想要调用重载函数 void func(int p)
    return 0;
}
```

通过打印的结果可以看到,虽然调用`func(NULL)`;最终链接到的还是`void func(int p)`和预期是不一样的,其实这个原因前边已经说的很明白了,在C++中`NULL`和`0`是等价的。

在底层源码中NULL这个宏是这样定义的:
```c++
#ifndef NULL
    #ifdef __cplusplus
        #define NULL 0
    #else
        #define NULL ((void *)0)
    #endif
#endif
```

在由于 C++ 中,`void *` 类型无法隐式转换为其他类型的指针

出于兼容性的考虑,C++11 标准并没有对 NULL 的宏定义做任何修改,而是另其炉灶,引入了一个新的关键字`nullptr`。nullptr 专用于初始化空类型指针,不同类型的指针变量都可以使用 nullptr 来初始化：
```c++
int*    ptr1 = nullptr;
char*   ptr2 = nullptr;
double* ptr3 = nullptr;
```

```c++
#include <iostream>
using namespace std;

void func(char *p)
{
    cout << "void func(char *p)" << endl;
}

void func(int p)
{
    cout << "void func(int p)" << endl;
}

int main()
{
    func(nullptr);
    func(250);
    return 0;
}
```

## 原始字面量

在C++11中添加了定义原始字符串的字面量,定义方式为：R “xxx(原始字符串)xxx”其中（）两边的字符串可以省略。原始字面量R可以直接表示字符串的实际含义,而不需要额外对字符串做转义或连接等操作。

比如：编程过程中,使用的字符串中常带有一些特殊字符,对于这些字符往往要做专门的处理,使用了原始字面量就可以轻松的解决这个问题了,比如打印路径：

```c++
#include<iostream>
#include<string>
using namespace std;
int main()
{
    string str = "D:\hello\world\test.text";
    cout << str << endl;
    string str1 = "D:\\hello\\world\\test.text";
    cout << str1 << endl;
    string str2 = R"(D:\hello\world\test.text)";
    cout << str2 << endl;

    return 0;
}
```

输出的结果为:
```c++
D:helloworld    est.text
D:\hello\world\test.text
D:\hello\world\test.text
```

- 在`D:\hello\world\test.text`中`\h`和`\w`转义失败,对应的字符会原样输出
- 在`D:\\hello\\world\\test.text`中路径的间隔符为`\`但是这个字符又是转义字符,因此需要使用转义字符将其转义,最终才能得到一个没有特殊含义的普通字符\
- 在`R"(D:\hello\world\test.text)"`使用了原始字面量R（）中的内容就是描述路径的原始字符串,无需做任何处理

## constexpr

#cpp面试点 
介绍consterxpr之前我们先介绍下c++与c中const的区别？
- c语言const修饰的量,可以不用初始化,不叫常量,叫做**常变量** ` int array[a]`;会报错,而且可以强转通过指针修改变量值
- c++的const必须初始化
- C中,const就是当作一个**变量来编译**生成指令的。 C++中,所有出现const常量名字的地方,都被**常量的初始化**替换了！ 

```c++
  const int a=20;
  int array[20]= {};

  int *p=(int* )&a;
  *p=30;
  cout<<a<<" "<<*p<<" "<<*(&a)<<endl; //20 30 30
```


- `const`定义的变量,不要求在编译时就能被算出,也就是说可以由变量赋值
- `constexpr`定义的变量,在编译时就能被算出,只能由常量表达式赋值

```c++
int x = 10;
const int i = x * x;           正确
constexpr int j = x * x;       编译器报错,x*x不是常量表达式
```

也就是说`constexpr` 比 `const`更常量一点。
- `const`可以说是在 【有了初值之后】,永远只读。
- 而`constexpr`则是从 【没初始化的时候】 就已经是一个常量了

`constexpr`是用来修饰常量表达式的。所谓常量表达式,指的就是由多个（≥1）常量（值不会改变）组成并且在编译过程中就得到计算结果的表达式。
常量表达式和非常量表达式的计算时机不同,非常量表达式只能在程序运行阶段计算出结果,但是常量表达式的计算往往发生在程序的编译阶段,这可以极大提高程序的执行效率,因为表达式只需要在编译阶段计算一次,节省了每次程序运行时都需要计算一次的时间。

对于 C++ 内置类型的数据,可以直接用 `constexpr` 修饰,但如果是自定义的数据类型（用 `struct` 或者 `class` 实现）,直接用 constexpr 修饰是不行的。

```c++
// 此处的constexpr修饰是无效的
constexpr struct Test
{
    int id;
    int num;
};
```

如果要定义一个结构体/类常量对象,可以这样写：

```c++
struct Test
{
    int id;
    int num;
};

int main()
{
    constexpr Test t{ 1, 2 };
    constexpr int id = t.id;
    constexpr int num = t.num;
    // error,不能修改常量
    t.num += 100;
    cout << "id: " << id << ", num: " << num << endl;

    return 0;
}
```

## noexcept

noexcept 形如其名,表示其修饰的函数不会抛出异常 。不过与 `throw()`动态异常声明不同的是,`在 C++11 中如果 noexcept 修饰的函数抛出了异常,编译器可以选择直接调用 std::terminate() 函数来终止程序的运行,这比基于异常机制的 throw() 在效率上会高一些`。这是因为异常机制会带来一些额外开销,比如函数抛出异常,会导致函数栈被依次地展开（栈解旋）,并自动调用析构函数释放栈上的所有对象。
因此对于不会抛出异常的函数我们可以这样写:
```c++
double divisionMethod(int a, int b) noexcept
{
    if (b == 0)
    {
        cout << "division by zero!!!" << endl;
        return -1;
    }
    return a / b;
}
```

从语法上讲,noexcept 修饰符有两种形式：

1. 简单地在函数声明后加上 noexcept 关键字
2. 可以接受一个常量表达式作为参数,如下所示∶

```c++
double divisionMethod(int a, int b) noexcept(常量表达式);
```

常量表达式的结果会被转换成一个bool类型的值：

- 值为 true,表示函数不会抛出异常
- 值为 false,表示有可能抛出异常这里
- <font color=#ff0000>不带常量表达式的noexcept相当于声明了noexcept（true）,即不会抛出异常</font>。


## 自动类型推导

### auto
#### 推导规则
C++11中auto并不代表一种实际的数据类型,只是一个类型声明的 “<font color=#ff0000>占位符</font>”,auto并不是万能的在任意场景下都能够推导出变量的实际类型,<font color=#ff0000>使用auto声明的变量必须要进行初始化</font>,以让编译器推导出它的实际类型,在编译时将auto占位符替换为真正的类型。使用语法如下：

```c++
auto 变量名 = 变量值;
```

```c++
auto x = 3.14;      // x 是浮点型 double
auto y = 520;       // y 是整形 int
auto z = 'a';       // z 是字符型 char
auto nb;            // error,变量必须要初始化
auto double nbl;    // 语法错误, 不能修改数据类型   
```

不仅如此,auto还可以和指针、引用结合起来使用也可以带上const、volatile限定符,在不同的场景下有对应的推导规则,规则内容如下：

- <font color=#ff0000>当变量不是指针或者引用类型时,推导的结果中不会保留const、volatile关键字</font>
- <font color=#ff0000>当变量是指针或者引用类型时,推导的结果中会保留const、volatile关键字</font>

其实总结下来就是auto会忽略`顶层const`。

```c++
int tmp = 250;
const auto a1 = tmp;
auto a2 = a1;
const auto &a3 = tmp;
auto &a4 = a3;
```

变量`a1`的数据类型为 `const int`,因此`auto`关键字被推导为 int类型
变量`a2`的数据类型为 `int`,但是a2没有声明为指针或引用因此 `const`属性被去掉(忽略顶层const), auto被推导为 `int`
变量`a3`的数据类型为 `const int&`,a3被声明为引用因此 `const`属性被保留,auto关键字被推导为 `int`类型
变量`a4`的数据类型为 `const int&`,a4被声明为引用因此 `const`属性被保留,auto关键字被推导为 `const int`类型


#### auto的限制

1. 不能作为函数参数使用。因为只有在函数调用的时候才会给函数参数传递实参,auto要求必须要给修饰的变量赋值,因此二者矛盾。

	```c++
	int func(auto a, auto b)	// error
	{	
	    cout << "a: " << a <<", b: " << b << endl;
	}
	```

2. 不能用于类的非静态成员变量的初始化

	```c++
	class Test
	{
	    auto v1 = 0;                    // error
	    static auto v2 = 0;             // error,类的静态非常量成员不允许在类内部直接初始化
	    static const auto v3 = 10;      // ok
	}
	```

3. 不能使用auto关键字定义数组

	```c++
	int func()
	{
	    int array[] = {1,2,3,4,5};  // 定义数组
	    auto t1 = array;            // ok, t1被推导为 int* 类型
	    auto t2[] = array;          // error, auto无法定义数组
	    auto t3[] = {1,2,3,4,5};;   // error, auto无法定义数组
	}
	```

4. 无法使用auto推导出模板参数

	```c++
	template <typename T>
	struct Test{}
	
	int func()
	{
	    Test<double> t;
	    Test<auto> t1 = t;           // error, 无法推导出模板类型
	    return 0;
	}
	```

#### auto的应用

1. 用于STL的容器遍历。
	在C++11之前,定义了一个stl容器之后,遍历的时候常常会写出这样的代码：

	```c++
	#include <map>
	int main()
	{
	    map<int, string> person;
	    map<int, string>::iterator it = person.begin();
	    for (; it != person.end(); ++it)
	    {
	        // do something
	    }
	    return 0;
	}
	```

	可以看到在定义迭代器变量 it 的时候代码是很长的,写起来就很麻烦,使用了auto之后,就变得清爽了不少：
	```c++
	#include <map>
	int main()
	{
	    map<int, string> person;
	    // 代码简化
	    for (auto it = person.begin(); it != person.end(); ++it)
	    {
	        // do something
	    }
	    return 0;
	}
	```

2. 用于泛型编程,在使用模板的时候,很多情况下我们不知道变量应该定义为什么类型,比如下面的代码：

	```c++
	#include <iostream>
	#include <string>
	using namespace std;
	
	class T1
	{
	public:
	    static int get()
	    {
	        return 10;
	    }
	};
	
	class T2
	{
	public:
	    static string get()
	    {
	        return "hello, world";
	    }
	};
	
	template <class A>
	void func(void)
	{
	    auto val = A::get();
	    cout << "val: " << val << endl;
	}
	
	int main()
	{
	    func<T1>();
	    func<T2>();
	    return 0;
	}
	```

3. auto可以作用函数的返回值的类型,避免显式指定返回类型,减少代码冗余和提高代码可读性

	```c++
	auto add(int a, int b) {
    return a + b;  // 返回类型被推导为 int
	}
	auto add(double a, double b) {
	    return a + b;  // 返回类型被推导为 double
	}
	```



补充点：
1. C++14新增了字面量后缀“s”,表示标准字符串,所以就可以用“auto str=”xxx“s;”的形式直 接推导出std::string类型。 
### decltype

```c++
decltype (表达式)
```

decltype 是“declare type”的缩写,意思是“声明类型”。decltype的推导<font color=#ff0000>是在编译期完成的</font>,它只是用于表达式类型的推导,并**不会计算**表达式的值。来看一组简单的例子：
```c++
int a = 10;
decltype(a) b = 99;                 // b -> int
decltype(a+3.14) c = 52.13;         // c -> double
decltype(a+b*c) d = 520.1314;       // d -> double
```

可以看到`decltype`推导的表达式可简单可复杂,在这一点上auto是做不到的,auto只能推导已初始化的变量类型。

#### 推导规则

1. 表达式为**普通变量**或者**普通表达式**或者**类表达式**,在这种情况下,使用decltype推导出的类型和表达式的类型是一致的。

	```c++
	#include <iostream>
	#include <string>
	using namespace std;
	
	class Test
	{
	public:
	    string text;
	    static const int value = 110;
	};
	
	int main()
	{
	    int x = 99;
	    const int &y = x;
	    decltype(x) a = x;
	    decltype(y) b = x;
	    decltype(Test::value) c = 0;
	
	    Test t;
	    decltype(t.text) d = "hello, world";
	
	    return 0;
	}
	```

	- 变量a被推导为 `int`类型
	- 变量b被推导为 `const int &`类型
	- 变量c被推导为 `const int`类型
	- 变量d被推导为 `string`类型

2. 表达式是函数调用,使用decltype推导出的类型和函数返回值一致。

	```c++
	class Test{...};
	//函数声明
	int func_int();                 // 返回值为 int
	int& func_int_r();              // 返回值为 int&
	int&& func_int_rr();            // 返回值为 int&&
	
	const int func_cint();          // 返回值为 const int
	const int& func_cint_r();       // 返回值为 const int&
	const int&& func_cint_rr();     // 返回值为 const int&&
	
	const Test func_ctest();        // 返回值为 const Test
	
	//decltype类型推导
	int n = 100;
	decltype(func_int()) a = 0;		
	decltype(func_int_r()) b = n;	
	decltype(func_int_rr()) c = 0;	
	decltype(func_cint())  d = 0;	
	decltype(func_cint_r())  e = n;	
	decltype(func_cint_rr()) f = 0;	
	decltype(func_ctest()) g = Test();	
	```

	- 变量a被推导为 int类型
	- 变量b被推导为 int&类型
	- 变量c被推导为 int&&类型
	- 变量d被推导为 int类型
	- 变量e被推导为 const int &类型
	- 变量f被推导为 const int &&类型
	- 变量g被推导为 const Test类型

	函数 `func_cint()` 返回的是一个纯右值（在表达式执行结束后不再存在的数据,也就是临时性的数据,没有地址）,<font color=#ff0000>对于纯右值而言,只有类类型可以携带const、volatile限定符</font>,除此之外需要忽略掉这两个限定符,因此推导出的变量d的类型为 int 而不是const int。

3. 表达式是一个**左值**,或者被**括号()** 包围,使用 decltype推导出的是表达式类型的引用（如果有const、volatile限定符不能忽略）。

	```c++
	#include <iostream>
	#include <vector>
	using namespace std;
	
	class Test
	{
	public:
	    int num;
	};
	
	int main() {
	    const Test obj;
	    //带有括号的表达式
	    decltype(obj.num) a = 0;
	    decltype((obj.num)) b = a;
	    //加法表达式
	    int n = 0, m = 0;
	    decltype(n + m) c = 0;
	    decltype(n = n + m) d = n;
	    return 0;
	}
	```

	- `obj.num` 为类的成员访问表达式,符合场景1,因此 a 的类型为`int`
	- `obj.num` 带有括号,符合场景3,因此b 的类型为 `const int&`。
	- `n+m` 得到一个右值,符合场景1,因此c的类型为 `int`
	- `n=n+m` 得到一个左值 n,符合场景3,因此d的类型为 `int&`

#### decltype的应用
关于decltype的应用多出现在泛型编程中。比如我们编写一个类模板,在里边添加遍历容器的函数,操作如下
```c++
#include <list>
using namespace std;

template <class T>
class Container
{
public:
    void func(T& c)
    {
        for (m_it = c.begin(); m_it != c.end(); ++m_it)
        {
            cout << *m_it << " ";
        }
        cout << endl;
    }
private:
    ??? m_it;  // 这里不能确定迭代器类型
};

int main()
{
    const list<int> lst;
    Container<const list<int>> obj;
    obj.func(lst);
    return 0;
}
```

在程序的第17行出了问题,关于迭代器变量一共有两种类型：`只读（T::const_iterator`）和`读写（T::iterator）`,有了decltype就可以完美的解决这个问题了,当 T 是一个 非 const 容器得到一个 T::iterator,当 T 是一个 const 容器时就会得到一个 `T::const_iterator`。

```c++
#include <list>
#include <iostream>
using namespace std;

template <class T>
class Container
{
public:
    void func(T& c)
    {
        for (m_it = c.begin(); m_it != c.end(); ++m_it)
        {
            cout << *m_it << " ";
        }
        cout << endl;
    }
private:
    decltype(T().begin()) m_it;  // 这里不能确定迭代器类型
};

int main()
{
    const list<int> lst{ 1,2,3,4,5,6,7,8,9 };
    Container<const list<int>> obj;
    obj.func(lst);
    return 0;
}
```

`decltype(T().begin())`这种写法在vs2017/vs2019下测试可用完美运行。

定义函数指针在C++里一直是个比较头疼的问题,因为传统的写法实在是太怪异了。但现在就简单了,你只要手里有一个函数,就可以用`decltype`很容易得到指针类型。
```c++
// UNIX信号函数的原型,看着就让人晕,你能手写出函数指针吗？
void (*signal(int signo, void (*func)(int)))(int)
// 使用decltype可以轻松得到函数指针类型
using sig_func_ptr_t = decltype(&signal) ;
```

所以这也是 decltype 可以“显身手”的地方。它可以搭配别名任意定义类型,再应用到成员变量、成员函数上,变通地实现 auto 的功能。

```c++
class DemoClass final {
public:
    using set_type=set<int>;
private:
    set_type m_set;
    using iter_type=  decltype(m_set.begin());
    iter_type mpos;
};
```

### 返回值后置

在泛型编程中,可能需要通过参数的运算来得到返回值的类型,比如下面这个场景：
```c++
#include <iostream>
using namespace std;
// R->返回值类型, T->参数1类型, U->参数2类型
template <typename R, typename T, typename U>
R add(T t, U u)
{
    return t + u;
}

int main()
{
    int x = 520;
    double y = 13.14;
    // auto z = add<decltype(x + y), int, double>(x, y);
    auto z = add<decltype(x + y)>(x, y);	// 简化之后的写法
    cout << "z: " << z << endl;
    return 0;
}
```

关于返回值,从上面的代码可以推断出和表达式 `t+u`的结果类型是一样的,因此可以通过通过`decltype`进行推导,关于模板函数的参数t和u可以通过实参自动推导出来,因此在程序中就也可以不写。虽然通过上述方式问题被解决了,但是解决方案有点过于理想化,因为对于调用者来说,是不知道函数内部执行了什么样的处理动作的。

因此如果要想解决这个问题就得直接在 add 函数身上做文章,先来看第一种写法：
```c++
template <typename T, typename U>
decltype(t+u) add(T t, U u)
{
    return t + u;
}
```

当我们在编译器中将这几行代码改出来后就直接报错了,因此decltype中的 t 和 u 都是函数参数,直接这样写相当于变量还没有定义就直接用上了,这时候变量还不存在,有点心急了。

在C++11中增加了返回类型后置语法,说明白一点就是将decltype和auto结合起来完成返回类型的推导。其语法格式如下:

```c++
// 符号 -> 后边跟随的是函数返回值的类型
auto func(参数1, 参数2, ...) -> decltype(参数表达式)
```

通过对上述返回类型后置语法代码的分析,得到结论：`auto` 会追踪 `decltype()` 推导出的类型,因此上边的`add()`函数可以做如下的修改：

```c++
#include <iostream>
using namespace std;

template <typename T, typename U>
// 返回类型后置语法
auto add(T t, U u) -> decltype(t+u) 
{
    return t + u;
}

int main()
{
    int x = 520;
    double y = 13.14;
    // auto z = add<int, double>(x, y);
    auto z = add(x, y);		// 简化之后的写法
    cout << "z: " << z << endl;
    return 0;
}
```


## 基于范围的for循环

C++11基于范围的for循环,语法格式：
```c++
for (declaration : expression)
{
    // 循环体
}
```

在上面的语法格式中declaration表示遍历声明,在遍历过程中,当前被遍历到的元素会被存储到声明的变量中。expression是要遍历的对象,它可以是表达式、容器、数组、初始化列表等。

使用基于范围的for循环遍历容器,示例代码如下：

```c++
#include <iostream>
#include <vector>
using namespace std;

int main(void)
{
    vector<int> t{ 1,2,3,4,5,6 };
    for (auto value : t)
    {
        cout << value << " ";
    }
    cout << endl;

    return 0;
}
```

在上面的例子中,<font color=#ff0000>是将容器中遍历的当前元素拷贝到了声明的变量value中,因此无法对容器中的元素进行写操作,如果需要在遍历过程中修改元素的值,需要使用引用</font>。
```c++
#include <iostream>
#include <vector>
using namespace std;

int main(void)
{
    vector<int> t{ 1,2,3,4,5,6 };
    cout << "遍历修改之前的容器: ";
    for (auto &value : t)
    {
        cout << value++ << " ";
    }
    cout << endl << "遍历修改之后的容器: ";

    for (auto &value : t)
    {
        cout << value << " ";
    }
    cout << endl;

    return 0;
}
```

代码输出的结果：
```c++
遍历修改之前的容器: 1 2 3 4 5 6
遍历修改之后的容器: 2 3 4 5 6 7
```

对容器的遍历过程中,如果只是读数据,不允许修改元素的值,可以使用const定义保存元素数据的变量,在定义的时候建议使用`const auto &`,这样相对于`const auto`效率要更高一些。
```c++
#include <iostream>
#include <vector>
using namespace std;

int main(void)
{
    vector<int> t{ 1,2,3,4,5,6 };
    for (const auto& value : t)
    {
        cout << value << " ";
    }

    return 0;
}
```

**使用细节**

使用基于范围的for循环有一些需要注意的细节,先来看一下对关系型容器map的遍历：
```c++
using namespace std;

int main(void)
{
    map<int, string> m{
        {1, "lucy"},{2, "lily"},{3, "tom"}
    };

    // 基于范围的for循环方式
    for (auto& it : m)
    {
        cout << "id: " << it.first << ", name: " << it.second << endl;
    }

    // 普通的for循环方式
    for (auto it = m.begin(); it != m.end(); ++it)
    {
        cout << "id: " << it->first << ", name: " << it->second << endl;
    }

    return 0;
}
```

在上面的例子中使用两种方式对map进行了遍历,通过对比有两点需要注意的事项：

1. 使用普通的for循环方式（基于迭代器）遍历关联性容器, auto自动推导出的是一个迭代器类型,需要使用迭代器的方式取出元素中的键值对（和指针的操作方法相同）：
	- it->first
	- it->second
2. 使用基于范围的for循环遍历关联性容器,auto自动推导出的类型是容器中的value_type,相当于一个对组（std::pair）对象,提取键值对的方式如下：
	- it.first
	- it.second

**元素只读**
通过对基于范围的for循环语法的介绍可以得知,在for循环内部声明一个变量的引用就可以修改遍历的表达式中的元素的值,但是这并不适用于所有的情况,<font color=#ff0000>对应set容器来说,内部元素都是只读的,这是由容器的特性决定的,因此在for循环中auto&会被视为const auto &</font> 。

```c++
#include <iostream>
#include <set>
using namespace std;

int main(void)
{
    set<int> st{ 1,2,3,4,5,6 };
    for (auto &item : st) 
    {
        cout << item++ << endl;		// error, 不能给常量赋值
    }
    return 0;
}
```

除此之外,<font color=#ff0000>在遍历关联型容器时也会出现同样的问题,基于范围的for循环中,虽然可以得到一个std::pair引用,但是我们是不能修改里边的first值的,也就是key值</font>。
```c++
#include <iostream>
#include <string>
#include <map>
using namespace std;

int main(void)
{
    map<int, string> m{
        {1, "lucy"},{2, "lily"},{3, "tom"}
    };

    for (auto& item : m)
    {
        // item.first 是一个常量
        cout << "id: " << item.first++ << ", name: " << item.second << endl;  // error
    }

    return 0;
}
```

**访问次数**
基于范围的for循环遍历的对象可以是一个表达式或者容器/数组等。假设我们对一个容器进行遍历,在遍历过程中for循环对这个容器的访问频率是一次还是多次呢？我们通过下面的例子验证一下：

```c++
#include <iostream>
#include <vector>
using namespace std;

vector<int> v{ 1,2,3,4,5,6 };
vector<int>& getRange()
{
    cout << "get vector range..." << endl;
    return v;
}

int main(void)
{
    for (auto val : getRange())
    {
        cout << val << " ";
    }
    cout << endl;

    return 0;
}
```

输出的结果如下：
```c++
get vector range...
1 2 3 4 5 6
```

从上面的结果中可以看到,不论基于范围的for循环迭代了多少次,函数getRange()只在第一次迭代之前被调用,得到这个容器对象之后就不会再去重新获取这个对象了。

**结论**：
> 对应基于范围的for循环来说,冒号后边的表达式只会被执行一次。在得到遍历对象之后会先确定好迭代的范围,基于这个范围直接进行遍历。如果是普通的for循环,在每次迭代的时候都需要判断是否已经到了结束边界



## 列表初始化

#### 聚合体
在C++11中,列表初始化的使用范围被大大增强了,但是一些模糊的概念也随之而来,列表初始化可以用于自定义类型的初始化,但是对于一个自定义类型,列表初始化可能有两种执行结果：

```c++
#include <iostream>
#include <string>
using namespace std;

struct T1
{
    int x;
    int y;
}a = { 123, 321 };

struct T2
{
    int x;
    int y;
    T2(int, int) : x(10), y(20) {}
}b = { 123, 321 };

int main(void)
{
    cout << "a.x: " << a.x << ", a.y: " << a.y << endl;
    cout << "b.x: " << b.x << ", b.y: " << b.y << endl;
    return 0;
}
a.x: 123, a.y: 321
b.x: 10, b.y: 20
```

- <font color=#ff0000>对象a是对一个自定义的聚合类型进行初始化,它将以拷贝的形式使用初始化列表中的数据来初始化T1结构体中的成员。</font>
- <font color=#ff0000>在结构体T2中自定义了一个构造函数,因此实际的初始化是通过这个构造函数完成的。</font>

那么,使用列表初始化时,对于什么样的类型C++会认为它是一个聚合体呢？

- 普通数组本身可以看做是一个聚合类型

```c++
int x[] = {1,2,3,4,5,6};
double y[3][3] = {
    {1.23, 2.34, 3.45},
    {4.56, 5.67, 6.78},
    {7.89, 8.91, 9.99},
};
char carry[] = {'a', 'b', 'c', 'd', 'e', 'f'};
std::string sarry[] = {"hello", "world", "nihao", "shijie"};
```

满足以下条件的类（class、struct、union）可以被看做是一个聚合类型：

- 无用户自定义的构造函数。
- 无私有或保护的非静态数据成员。
	- 场景1: 类中有私有成员, 无法使用列表初始化进行初始化
	```c++
	struct T1
	{
	    int x;
	    long y;
	protected:
	    int z;
	}t{ 1, 100, 2};		// error, 类中有私有成员, 无法使用初始化列表初始化
	```

	- 场景2：类中有非静态成员可以通过列表初始化进行初始化,但它不能初始化静态成员变量。

	```c++
	struct T2
	{
	    int x;
	    long y;
	protected:
	    static int z;
	}t{ 1, 100, 2};		// error
	```

	<font color=#ff0000>结构体中的静态变量 z 不能使用列表初始化进行初始化,它的初始化遵循静态成员的初始化方式。</font>

	```c++
	struct T2
	{
	    int x;
	    long y;
	protected:
	    static int z;
	}t{ 1, 100};		// ok
	// 静态成员的初始化
	int T2::z = 2;
	```

- 无基类。
- 无虚函数。

> 从C++14开始,使用列表初始化也可以初始化在类中使用{}和=初始化过的非静态数据成员。

#### 非聚合体
对于聚合类型的类可以直接使用列表初始化进行对象的初始化,如果不满足聚合条件还想使用列表初始化其实也是可以的,<font color=#ff0000>需要在类的内部自定义一个构造函数, 在构造函数中使用初始化列表对类成员变量进行初始化</font>:

```c++
#include <iostream>
#include <string>
using namespace std;

struct T1
{
    int x;
    double y;
    // 在构造函数中使用初始化列表初始化类成员
    T1(int a, double b, int c) : x(a), y(b), z(c){}
    virtual void print()
    {
        cout << "x: " << x << ", y: " << y << ", z: " << z << endl;
    }
private:
    int z;
};

int main(void)
{
    T1 t{ 520, 13.14, 1314 };	// ok, 基于构造函数使用初始化列表初始化类成员
    t.print();
    return 0;
}
```

另外,需要额外注意的是<font color=#ff0000>聚合类型的定义并非递归的</font>,也就是说<font color=#ff0000>当一个类的非静态成员是非聚合类型时,这个类也可能是聚合类型</font>,比如下面的这个例子：

```c++
#include <iostream>
#include <string>
using namespace std;
struct T1
{
    int x;
    double y;
private:
    int z;
};
struct T2
{
    T1 t1;
    long x1;
    double y1;
};
int main(void)
{
    T2 t2{ {}, 520, 13.14 };
    return 0;
}
```

可以看到,T1并非一个聚合类型,因为它有一个Private的非静态成员。但是尽管T2有一个非聚合类型的非静态成员t1,T2依然是一个聚合类型,可以直接使用列表初始化的方式进行初始化。
最后强调一下t2对象的初始化过程,对于非聚合类型的成员t1做初始化的时候,可以直接写一对空的大括号`{}`,这相当于调用是T1的无参构造函数。

> <font color=#ff0000>对于一个聚合类型,使用列表初始化相当于对其中的每个元素分别赋值,而对于非聚合类型,则需要先自定义一个合适的构造函数,此时使用列表初始化将会调用它对应的构造函数。</font>

### std::initializer_list

和vector一样,`initializer_list`也是一种模板类型。和vector不一样的是,initializer_list对象中的**元素永远是常量值**,我们无法改变initializerlist对象中元素的值。 
#### 作为构造函数参数
自定义的类如果在构造对象的时候想要接收任意个数的实参,可以给构造函数指定为`std::initializer_list`类型,在自定义类的内部还是使用容器来存储接收的多个实参。
```c++
using namespace std;
class Test
{
public:
    Test(std::initializer_list<string> list)
    {
        for (auto it = list.begin(); it != list.end(); ++it)
        {
            cout << *it << " ";
            m_names.push_back(*it);
        }
        cout << endl;
    }
private:
    vector<string> m_names;
};
int main(void)
{
    Test t({ "jack", "lucy", "tom" });
    Test t1({ "hello", "world", "nihao", "shijie" });
    return 0;
}
```

#### 开源使用例子

```c++
struct __declspec(dllexport) Part {
    Part(const std::string& p_name, const std::string& p_value, const std::string& p_content_type = {})
            : name{p_name}, value{p_value},
              content_type{p_content_type}, is_file{false}, is_buffer{false} {}
    Part(const std::string& p_name, const std::int32_t& p_value, const std::string& p_content_type = {})
            : name{p_name}, value{std::to_string(p_value)},
              content_type{p_content_type}, is_file{false}, is_buffer{false} {}
    Part(const std::string& p_name, const File& file, const std::string& p_content_type = {})
            : name{p_name}, value{file.filepath},
              content_type{p_content_type}, is_file{true}, is_buffer{false} {}
    Part(const std::string& p_name, const Buffer& buffer, const std::string& p_content_type = {})
            : name{p_name}, value{buffer.filename}, content_type{p_content_type}, data{buffer.data},
              datalen{buffer.datalen}, is_file{false}, is_buffer{true} {}
    std::string name;
    std::string value;
    std::string content_type;
    Buffer::data_t data{nullptr};
    // Ignored here since libcurl reqires a long:
    // NOLINTNEXTLINE(google-runtime-int)
    long datalen{0};
    bool is_file;
    bool is_buffer;
};

class __declspec(dllexport) Multipart {
  public:
    Multipart(const std::initializer_list<Part>& p_parts):parts(p_parts){};
    std::vector<Part> parts;
};


// 在我们实际去使用cpr发送带multipart的请求时,我们的请求消息如下：
// The uploaded file is named "path-to-file"
cpr::Response r = cpr::Post(cpr::Url{"http://www.httpbin.org/post"},
                  cpr::Multipart{{"part-name", cpr::File{"path-to-file"}}});

// The uploaded file is named "new-file-name"
cpr::Response r = cpr::Post(cpr::Url{"http://www.httpbin.org/post"},
                  cpr::Multipart{{"part-name", cpr::File{"path-to-file", "new-file-name"}}});

// The uploaded files are named "path-to-file1" and "path-to-file2"
cpr::Response r = cpr::Post(cpr::Url{"http://www.httpbin.org/post"},
                  cpr::Multipart{{"part-name", cpr::Files{"path-to-file1", "path-to-file2"}}});

// The uploaded files are named "new-file-name1" and "new-file-name2"
cpr::Response r = cpr::Post(cpr::Url{"http://www.httpbin.org/post"},
                  cpr::Multipart{{"part-name", cpr::Files{
                                       File{"path-to-file1", "new-file-name1"},
                                       File{"path-to-file2", "new-file-name2"},
                               }}});
```

在实际使用中,我们的Multipart的参数往往很复杂,而cpr的代码实现中,通过构造了一个拥有多个构造函数的Part结构,然后让Multipart接口参数为Part的`initializer_lis`t,就可以实现对各类复杂输入的兼容了

`nlohmann/json`
这个应该是C++项目中最常用的json库之一,从中我们也能窥见initializer_list的使用,比如说,我们有的时候会这样去初始化一个json：
```cpp
json j = {{"name", "John"}, {"age", 30}, {"city", "New York"}};
```

实际上,在json的内部实现中,我们会发现,它也是通过initilizer_list嵌套复杂结构的方式去处理复杂的输入的：

```c++
using initializer_list_t = std::initializer_list<detail::json_ref<basic_json>>;
JSON_HEDLEY_WARN_UNUSED_RESULT
    static basic_json binary(typename binary_t::container_type&& init, typename binary_t::subtype_type subtype)
    {
        auto res = basic_json();
        res.m_type = value_t::binary;
        res.m_value = binary_t(std::move(init), subtype);
        return res;
    }
    /// @brief explicitly create an array from an initializer list
    /// @sa https://json.nlohmann.me/api/basic_json/array/
    JSON_HEDLEY_WARN_UNUSED_RESULT
    static basic_json array(initializer_list_t init = {})
    {
        return basic_json(init, false, value_t::array);
    }
```

## 委托构造和继承构造函数

### 委托构造函数
委托构造函数允许使用同一个类中的一个构造函数调用其它的构造函数,从而简化相关变量的初始化。下面举例说明：
```c++
#include <iostream>
using namespace std;
class Test
{
public:
    Test() {};
    Test(int max)
    {
        this->m_max = max > 0 ? max : 100;
    }
    Test(int max, int min)
    {
        this->m_max = max > 0 ? max : 100;              // 冗余代码
        this->m_min = min > 0 && min < max ? min : 1;   
    }
    Test(int max, int min, int mid)
    {
        this->m_max = max > 0 ? max : 100;             // 冗余代码
        this->m_min = min > 0 && min < max ? min : 1;  // 冗余代码
        this->m_middle = mid < max && mid > min ? mid : 50;
    }
    int m_min;
    int m_max;
    int m_middle;
};
int main()
{
    Test t(90, 30, 60);
    cout << "min: " << t.m_min << ", middle: " 
         << t.m_middle << ", max: " << t.m_max << endl;
    return 0;
}
```

在上面的程序中有三个构造函数,但是这三个函数中都有重复的代码,在C++11之前构造函数是不能调用构造函数的,加入了委托构造之后,我们就可以轻松地完成代码的优化了：

```c++
#include <iostream>
using namespace std;

class Test
{
public:
    Test() {};
    Test(int max)
    {
        this->m_max = max > 0 ? max : 100;
    }

    Test(int max, int min):Test(max)
    {
        this->m_min = min > 0 && min < max ? min : 1;
    }

    Test(int max, int min, int mid):Test(max, min)
    {
        this->m_middle = mid < max && mid > min ? mid : 50;
    }

    int m_min;
    int m_max;
    int m_middle;
};

int main()
{
    Test t(90, 30, 60);
    cout << "min: " << t.m_min << ", middle: " 
         << t.m_middle << ", max: " << t.m_max << endl;
    return 0;
}
```

### 继承构造函数
C++11中提供的继承构造函数可以让派生类直接使用基类的构造函数,而无需自己再写构造函数,尤其是在基类有很多构造函数的情况下,可以极大地简化派生类构造函数的编写。先来看没有继承构造函数之前的处理方式：

```c++
#include <iostream>
#include <string>
using namespace std;

class Base
{
public:
    Base(int i) :m_i(i) {}
    Base(int i, double j) :m_i(i), m_j(j) {}
    Base(int i, double j, string k) :m_i(i), m_j(j), m_k(k) {}

    int m_i;
    double m_j;
    string m_k;
};

class Child : public Base
{
public:
    Child(int i) :Base(i) {}
    Child(int i, double j) :Base(i, j) {}
    Child(int i, double j, string k) :Base(i, j, k) {}
};

int main()
{
    Child c(520, 13.14, "i love you");
    cout << "int: " << c.m_i << ", double: " 
         << c.m_j << ", string: " << c.m_k << endl;
    return 0;
}
```

通过测试代码可以看出,在子类中初始化从基类继承的类成员,需要在子类中重新定义和基类一致的构造函数,这是非常繁琐的,C++11中通过添加继承构造函数这个新特性完美的解决了这个问题,使得代码更加精简。

继承构造函数的使用方法是这样的：通过使用`using 类名::构造函数名`（其实类名和构造函数名是一样的）来声明使用基类的构造函数,这样子类中就可以不定义相同的构造函数了,直接使用基类的构造函数来构造派生类对象。

```c++
#include <iostream>
#include <string>
using namespace std;

class Base
{
public:
    Base(int i) :m_i(i) {}
    Base(int i, double j) :m_i(i), m_j(j) {}
    Base(int i, double j, string k) :m_i(i), m_j(j), m_k(k) {}

    int m_i;
    double m_j;
    string m_k;
};

class Child : public Base
{
public:
    using Base::Base;
};

int main()
{
    Child c1(520, 13.14);
    cout << "int: " << c1.m_i << ", double: " << c1.m_j << endl;
    Child c2(520, 13.14, "i love you");
    cout << "int: " << c2.m_i << ", double: " 
         << c2.m_j << ", string: " << c2.m_k << endl;
    return 0;
}
```

在修改之后的子类中,没有添加任何构造函数,而是添加了`using Base::Base`;这样就可以在子类中直接继承父类的所有的构造函数,通过他们去构造子类对象了。

另外如果在子类中隐藏了父类中的同名函数,也可以通过`using`的方式在子类中使用基类中的这些父类函数：

```c++
#include <iostream>
#include <string>
using namespace std;

class Base
{
public:
    Base(int i) :m_i(i) {}
    Base(int i, double j) :m_i(i), m_j(j) {}
    Base(int i, double j, string k) :m_i(i), m_j(j), m_k(k) {}

    void func(int i)
    {
        cout << "base class: i = " << i << endl;
    }
    
    void func(int i, string str)
    {
        cout << "base class: i = " << i << ", str = " << str << endl;
    }

    int m_i;
    double m_j;
    string m_k;
};

class Child : public Base
{
public:
    using Base::Base;
    using Base::func;
    void func()
    {
        cout << "child class: i'am luffy!!!" << endl;
    }
};

int main()
{
    Child c(250);
    c.func();
    c.func(19);
    c.func(19, "luffy");
    return 0;
}
```

上述示例代码输出的结果为：
```c++
child class: i'am luffy!!!
base class: i = 19
base class: i = 19, str = luffy
```

子类中的`func()`函数隐藏了基类中的两个`func()`因此默认情况下通过子类对象只能调用无参的`func()`,在上面的子类代码中添加了`using Base::func`;之后,就可以通过子类对象直接调用父类中被隐藏的带参`func()`函数了。

## 右值引用

### 左值与右值定义

C++11 增加了一个新的类型,称为右值引用（ R-value reference）,标记为`&&`。在介绍右值引用类型之前先要了解什么是左值和右值：
C++11 中右值可以分为两种：一个是**将亡值**（ xvalue, expiring value）,另一个则是**纯右值**（ prvalue, PureRvalue）：

- **纯右值**：非**引用**返回的临时变量、运算表达式产生的临时变量、原始字面量和 lambda 表达式等
- **将亡值**：与右值引用相关的表达式,比如,T&&类型函数的返回值、 std::move 的返回值等。

```c++
int value = 520;
```

### 右值引用

- 右值引用就是对一个右值进行引用的类型。因为右值是匿名的,所以我们只能通过引用的方式找到它。
- 因为引用类型本身并不拥有所绑定对象的内存,只是该对象的**一个别名**。通过右值引用的声明,该右值又“重获新生”,也就是<font color=#ff0000>延长生命周期</font>

```c++
int&& value = 520;
class Test
{
public:
    Test()
    {
        cout << "construct: my name is jerry" << endl;
    }
    Test(const Test& a)
    {
        cout << "copy construct: my name is tom" << endl;
    }
};
Test getObj()
{
    return Test();
}
int main()
{
    int a1;
    int &&a2 = a1;        // error
    Test& t = getObj();   // error
    Test && t = getObj();
    const Test& t = getObj();
    return 0;
}
```

- 在上面的例子中`int&& value = 520`;里面`520`是纯右值,`value`是对字面量`520`这个右值的引用。
- 在`int &&a2 = a1`;中a1虽然写在了=右边,但是它仍然是一个左值,使用左值初始化一个右值引用类型是不合法的。
- 在`Test& t = getObj()`这句代码中语法是错误的,右值不能给普通的左值引用赋值。
- 在`Test && t = getObj()`;中`getObj()`返回的临时对象被称之为将亡值,t是这个将亡值的右值引用。
- `const Test& t = getObj()`这句代码的语法是正确的,常量左值引用是一个万能引用类型,它可以接受左值、右值、常量左值和常量右值。

下面说说这个<font color=#ff0000>延长生命周期</font>的作用
- 右值引用的应用在于初始化一个对象a,需要创建一个临时对象对对象a初始化,这个临时对象构造需要时间,还要将数据拷贝给a,拷贝完后临时对象就析构了,从创建到销毁生命周期比较短,但是这个临时对象花费了大量的时间资源,这个时候右值引用延长临时对象生命周期,直接把这个临时对象作为a使用,等到临时对象数据操作完了,再销毁。

我们使用右值引用的目的是实现移动,而实现移动的意义是**减少运行的开销**——在引用计数指针的场景下,这个开销并不大。移动构造和拷贝构造的差异仅在于：

```c++
string result =
string("Hello, ") + name + ".";
```

在 C++11 之前的年代里,这种写法是绝对不推荐的。因为它会引入很多额外开销,执行流程大致如下：
1. 调用构造函数 `string(const char*)`,生成临时对象 1；`"Hello, "` 复制 1 次。
2. 调用 `operator+(const string&, const string&)`,生成临时对象2；`"Hello,"` 复制 2 次,`name` 复制 1 次。
3. 调用 `operator+(const string&, const char*)`,生成对象3；`"Hello, "` 复制 3 次,`name` 复制 2 次,`"."` 复制 1 次。
4. 假设返回值优化能够生效（最佳情况）,对象 3 可以直接在 `result` 里构造完成。 
5. 临时对象 2 析构,释放指向 `string("Hello, ") + name` 的内存。 
6. 临时对象 1 析构,释放指向 `string("Hello, ")` 的内存。

既然 C++ 是一门追求性能的语言,一个合格的 C++ 程序员会写:

```c++
string result = "Hello, ";
result += name;
result += ".";
```

这样的话,只会调用构造函数一次和 `string::operator+=` 两次,没有任何临时对象需要生成和析构,所有的字符串都只复制了一次。但显然代码就啰嗦多了——尤其如果拼接的步骤比较多的话。从 C++11 开始,这不再是必须的。同样上面那个单行的语句,执行流程大致如下：
1. 调用构造函数 `string(const char*)`,生成临时对象 1；`"Hello, "` 复制 1 次。 
2. 调用 `operator+(string&&, const string&)`,直接在临时对象 1 上面执行追加操作,并把结果移动到临时对象 2；name 复制 1 次。
3. 调用 `operator+(string&&, const char*)`,直接在临时对象 2 上面执行追加操作,并把结果移动到 `result`；"." 复制 1 次。
4. 临时对象 2 析构,内容已经为空,不需要释放任何内存。
5. 临时对象 1 析构,内容已经为空,不需要释放任何内存。


### &&的特性

在C++中,并不是所有情况下 `&&` 都代表是一个右值引用,具体的场景体现在模板和自动类型推导中,如果是模板参数需要指定为`T&&`,如果是自动类型推导需要指定为`auto &&`,在这两种场景下 &&被称作<font color=#ff0000>未定的引用类型</font>。另外还有一点需要额外注意`const T&&`表示一个右值引用,不是未定引用类型。

先来看第一个例子,在函数模板中使用&&:

```c++
template<typename T>
void f(T&& param);
void f1(const T&& param);
f(10); 	
int x = 10;
f(x); 
f1(x);	// error, x是左值
f1(10); // ok, 10是右值
```

在上面的例子中函数模板进行了自动类型推导,需要通过传入的实参来确定参数param的实际类型。

- 第4行中,对于`f(10)`来说传入的实参10是右值,因此`T&&`表示右值引用
- 第6行中,对于`f(x)`来说传入的实参是x是左值,因此`T&&`表示左值引用
- 第7行中,`f1(x)`的参数是`const T&&`不是未定引用类型,不需要推导,本身就表示一个右值引用

再来看第二个例子:
```c++
int main()
{
    int x = 520, y = 1314;
    auto&& v1 = x;
    auto&& v2 = 250;
    decltype(x)&& v3 = y;   // error
    cout << "v1: " << v1 << ", v2: " << v2 << endl;
    return 0;
};
```


- 第4行中 `auto&&`表示一个整形的左值引用
- 第5行中 `auto&&`表示一个整形的右值引用
- 第6行中`decltype(x)&&`等价于`int&&`是一个右值引用不是未定引用类型,y是一个左值,不能使用左值初始化一个右值引用类型。

在C++11中<font color=#ff0000>引用折叠</font>的规则如下：
- 通过右值推导 T&& 或者 auto&& 得到的是一个右值引用类型
- 通过非右值（右值引用、左值、左值引用、常量右值引用、常量左值引用）推导 T&& 或者 auto&& 得到的是一个左值引用类型

```c++
int&& a1 = 5;
auto&& bb = a1;
auto&& bb1 = 5;

int a2 = 5;
int &a3 = a2;
auto&& cc = a3;
auto&& cc1 = a2;

const int& s1 = 100;
const int&& s2 = 100;
auto&& dd = s1;
auto&& ee = s2;

const auto&& x = 5;
```

- 第2行：`a1`为右值引用,推导出的`bb`为左值引用类型
- 第3行：5为右值,推导出的`bb1`为右值引用类型
- 第7行：`a3`为左值引用,推导出的`cc`为左值引用类型
- 第8行：`a2`为左值,推导出的`cc1`为左值引用类型
- 第12行：`s1`为常量左值引用,推导出的`dd`为常量左值引用类型
- 第13行：`s2`为常量右值引用,推导出的`ee`为常量左值引用类型
- 第15行：`x`为右值引用,不需要推导,只能通过右值初始化


### 右值引用的传递

```c++
int &&a = 5;  // 正确。5是一个右值。
int &&b = a;  // 错误。a是一个已命名的右值引用,因此它是一个左值。
```

虽然`a`是一个右值引用,但`a`本身是一个已命名的对象,因此**它是一个左值**。因此,你不能将一个左值赋值给一个右值引用。这就是为什么第二行代码会报错。

```c++
#include <iostream>
using namespace std;
void printValue(int &i)
{
    cout << "l-value: " << i << endl;
}
void printValue(int &&i)
{
    cout << "r-value: " << i << endl;
}
void forward(int &&k)
{
    printValue(k);
}
int main()
{
    int i = 520;
    printValue(i);
    printValue(1314);
    forward(250);

    return 0;
};
```

```c++
l-value: 520
r-value: 1314
l-value: 250
```

根据测试代码可以得知,编译器会根据传入的参数的类型（左值还是右值）调用对应的重置函数（printValue）,函数forward()接收的是一个右值,但是在这个函数中调用函数`printValue()`时,参数k变成了一个命名对象,编译器会将其当做左值来处理。

最后总结一下关于&&的使用：
- 左值和右值是独立于他们的类型的,右值引用类型可能是左值也可能是右值。
- 编译器会将已命名的右值引用视为左值,将未命名的右值引用视为右值。
- `auto&&`或者函数参数类型自动推导的`T&&`是一个未定的引用类型,它可能是左值引用也可能是右值引用类型,这取决于初始化的值类型（上面有例子）。
- 通过右值推导 `T&&` 或者 `auto&&` 得到的是一个右值引用类型,其余都是左值引用类型。

### 转移(move)

- 在C++11添加了右值引用,并且不能使用左值初始化右值引用,如果想要使用左值初始化一个右值引用需要借助`std::move()`函数,使用std::move方法可以**将左值转换为右值引用**。
- 使用这个函数并不能移动任何东西,而是和移动构造函数一样都具有**移动语义**,将对象的状态或者所有权从一个对象转移到另一个对象,只是转移,没有内存拷贝。
- **移动语义**使编译器可以使用移动操作来代替昂贵的拷贝操作， 但注意 `std::move` 本身并没有执行什么“移动”操作
- 从实现上讲,std::move基本等同于一个类型转换：`static_cast<T&&>(lvalue)`;,函数原型如下:
```c++
template<class _Ty>
_NODISCARD constexpr remove_reference_t<_Ty>&& move(_Ty&& _Arg) _NOEXCEPT
{	// forward _Arg as movable
    return (static_cast<remove_reference_t<_Ty>&&>(_Arg));
}
```

使用方法如下：
```c++
class Test
{
public：
    Test(){}
    ......
}
int main()
{
    Test t;
    Test && v1 = t;          // error
    Test && v2 = move(t);    // ok
    return 0;
}
```



> [!check] 使用move的场景
> - 当我们打算用一个左值去调用某个需要右值引用的函数，并且返回后不会再去使用左值原本包含的值的时候，就可以使用std::move。
> - 对于 [[C++#POD类型|POD]] 类型，事实上 move 只能执行 copy 操作。此外现在的很多类型已经优化了构造函数可能也不能从 std::move 中获得更多收益，所以有没有性能提升还是看 benchmark。

> **没有移动操作**：要移动的对象没有提供移动操作，所以移动的写法也会变成复制操作。 **移动不会更快**：要移动的对象提供的移动操作并不比复制速度更快。 **移动不可用**：进行移动的上下文要求移动操作不会抛出异常，但是该操作没有被声明为noexcept (item14)


### forward

右值引用类型是独立于值的,一个右值引用作为函数参数的形参时,在函数内部转发该参数给内部其他函数时,它就变成一个左值,并不是原来的类型了。如果需要按照参数原来的类型转发到另一个函数,可以使用C++11提供的`std::forward()`函数,该函数实现的功能称之为完美转发。

```c++
// 函数原型
template <class T> T&& forward (typename remove_reference<T>::type& t) noexcept;
template <class T> T&& forward (typename remove_reference<T>::type&& t) noexcept;
// 译器可能会因为不确定性而无法解析它是一个类型还是一个值。为了明确地告诉编译器“这是一个类型,不是一个值”,你需要使用typename关键字。
// 精简之后的样子
std::forward<T>(t);
```

- `当T为左值引用类型时,t将被转换为T类型的左值`
- `当T不是左值引用类型时,t将被转换为T类型的右值`

下面通过一个例子演示一下关于forward的使用:

```c++
#include <iostream>
using namespace std;

template<typename T>
void printValue(T& t)
{
    cout << "l-value: " << t << endl;
}

template<typename T>
void printValue(T&& t)
{
    cout << "r-value: " << t << endl;
}

template<typename T>
void testForward(T && v)
{
    printValue(v);
    printValue(move(v));
    printValue(forward<T>(v));
    cout << endl;
}

int main()
{
    testForward(520);
    int num = 1314;
    testForward(num);
    testForward(forward<int>(num));
    testForward(forward<int&>(num));
    testForward(forward<int&&>(num));

    return 0;
}
```


> [!check] forward比move的不同
> 1. forward传递出去的既可以是左值又可以是右值
> 2. 而move的把所有的值无论是左值还是右值都转换为右值，调用的永远是右值引用参数的函数

### emplace_back

**emplace_back()和push_back()的区别**
场景一：考虑是否原地构造
`push_back()`：向容器中加入一个右值元素(临时对象)时,首先会调用构造函数**构造这个临时对象**,然后需要调用**拷贝构造函数**将这个临时对象放入容器中。原来的临时变量释放。这样造成的问题就是临时变量申请资源的浪费。
`emplace_back()`：引入了右值引用,转移构造函数,在插入的时候直接构造,只需要构造一次即可。
也就是说,两者的底层实现的机制不同。`push_back()` 向容器尾部添加元素时,首先会创建这个元素,然后再将这个元素拷贝或者移动到容器中（如果是拷贝的话,事后会自行销毁先前创建的这个元素）；而 `emplace_back()` 在实现时,则是直接在容器尾部创建这个元素,省去了**拷贝或移动元素**的过程。
代码如下（示例）：
```c++
class testDemo
{
public:
    testDemo(int num):num(num){
        std::cout << "调用构造函数" << endl;
    }
    testDemo(const testDemo& other) :num(other.num) {
        std::cout << "调用拷贝构造函数" << endl;
    }
    testDemo(testDemo&& other) :num(other.num) {
        std::cout << "调用移动构造函数" << endl;
    }
private:
    int num;
};

int main()
{
    cout << "emplace_back:" << endl;
    std::vector<testDemo> demo1;
    demo1.emplace_back(2);  

    cout << "push_back:" << endl;
    std::vector<testDemo> demo2;
    demo2.push_back(2);
}
```

输出结果为：
```c++
emplace_back:
调用构造函数
push_back:
调用构造函数
调用移动构造函数
```

1. **当传入的参数是左值**：
    对于 `push_back`,当传入一个左值时,它会调用元素的拷贝构造函数,因为你正尝试复制一个值到容器中。
    对于 `emplace_back`,当传入一个左值时,它同样会调用元素的拷贝构造函数。这是因为`emplace_back`的设计初衷是直接在容器内部构造元素,而不是先构造一个元素然后再将它移入容器。所以,在此情境下,`emplace_back`与`push_back`在传入左值时表现是一致的。
2. **当传入的参数是右值或将亡值**：
    `push_back` 会检测到这是一个右值,并调用元素的移动构造函数（如果提供了的话）。
    `emplace_back` 则直接在容器内部位置上构造这个元素,避免了额外的移动或拷贝。但实际上,当提供一个单独的右值作为参数时,`emplace_back`通常也会利用移动构造。
3. **构造参数与元素构造参数不同的情况**：
    这是`emplace_back`真正的威力所在。`emplace_back`可以使用传递给它的参数直接在容器内部位置上构造元素。这意味着,你可以传递原始构造参数,而不是预先构造的对象。例如,对于一个`std::pair<int, std::string>`的`std::vector`：
	```c++
	std::vector<std::pair<int, std::string>> vec;
	vec.emplace_back(42, "meaning");
	```
## 编译指令

`#include、#define` 都是预处理指令,是用来控制预处理器的。那么问题就来了,有没有用来控制编译器的“编译指令”呢？

到了 C++11,标准委员会终于认识到了“编译指令”的好处,于是就把“民间”用法升级为“官方版本”,起了个正式的名字叫“属性”。你可以把它理解为给变量、函数、类等“贴”上一个编译阶段的“标签”,**方便编译器识别处理**,类似于java中的[[java基础篇#注解|注解]]

- 属性”没有新增关键字,而是用两对方括号的形式“`[[…]]`”,方括号的中间就是属性标签
- “属性”也支持非标准扩展,允许以类似名字空间的方式使用编译器自己的一些“非官方”属性


C++中常用的属性标签如下：
- **deprecated**：这个指明其修饰的函数是一个已经被抛弃的接口,最好换成新接口。
- **unused**：用于变量、类型、函数等,表示其虽然暂时不用,但是最好保留下来。
- **constructor**：指明此函数会在main函数之前执行
- **destructor**：知名此函数会在main函数之后执行
- **always_inline**：要求编译器强制内联函数,inline关键字的加强版
- **hot**：标记为热点函数,要求编译器更积极优化
- **noreturn**:告诉编译器某个函数不会正常返回到调用点,即函数在执行过程中会终止程序或以其他方式退出,而不是返回到调用方。
### 属性Demo
**deprecated**
我们定义一个函数为deprecated修饰,这会表示这个函数是个旧的接口。
```c++
[[gnu::deprecated]]
int old_func()
{
    return 0;
}
int main()
{
    cout << old_func() << endl;
}
[ik@localhost test]$ g++ -o test test.cpp
test.cpp: 在函数‘int main()’中:
test.cpp:13:22: 警告：‘int old_func()’ is deprecated [-Wdeprecated-declarations]
     cout << old_func() << endl;
                      ^
test.cpp:6:5: 附注：在此声明
 int old_func()
     ^~~~~~~~
```

**unused**
目的是为了提醒开发人员和其他阅读代码的人,该变量是有意未被使用的,可能是因为暂时不需要或者为了保留将来的扩展性。
```c++
[[gnu::unused]] int var;
int main()
{
    return 0;
}
```

**constructor && deconsructor**
我们定义两个函数,`at_start`会在main函数之前执行,`at_exit`会在main函数之后执行

```c++
[[gnu::constructor]]
int at_start()
{
    printf("excute before main::func\n");
}

[[gnu::destructor]]
int at_exit()
{
    printf("excute after main::func\n");
}
int main()
{
    cout << "main::func excute!" << endl;
    return 0;
}
```

> 注：
> constructor修饰的函数不能用cout,因为cout需要全局变量初始化！
> constructor(x),这里面的x代表构造的优先级

**always_inline**
```c++
[[gnu::always_inline]] inline void func()
{
    printf("i am test_func()");
}
```


**noreturn**
```
[[noreturn]]
int case1(bool flag)
{
    throw std::runtime_error("XXX");
}
```

**hot**: 目的是告诉编译器优化器更积极地对这些函数进行优化,以提高程序的性能。具体的操作方式和优化策略可能因编译器而异
```c++
[[gnu::hot]]
void case3()
{
    using namespace std;

    cout << "case3" << endl;
}
```

## final和override

### final

final**限制某个类不能被继承,或者某个虚函数不能被重写**,如果使用final修饰函数,只能修饰<font color=#ff0000>虚函数</font>,并且要把final关键字<font color=#ff0000>放到类或者函数的后面</font>。

```c++
class Base
{
public:
    virtual void test()
    {
        cout << "Base class...";
    }
};

class Child : public Base
{
public:
    void test() final
    {
        cout << "Child class...";
    }
};

class GrandChild : public Child
{
public:
    // 语法错误, 不允许重写
    void test()
    {
        cout << "GrandChild class...";
    }
};
```

```c++
class Base
{
public:
    virtual void test()
    {
        cout << "Base class...";
    }
};

class Child final: public Base
{
public:
    void test()
    {
        cout << "Child class...";
    }
};

// error, 语法错误
class GrandChild : public Child
{
public:
};
```

### override

`override`关键字确保在派生类中声明的重写函数与基类的虚函数有相同的签名,同时也明确<font color=#ff0000>表明将会重写基类的虚函数</font>,这样就可以保证重写的虚函数的正确性,也提高了代码的可读性,和final一样这个关键字要写到方法的后面。

```c++
class Base
{
public:
    virtual void test()
    {
        cout << "Base class...";
    }
};
class Child:public Base:
{
public:
	void test() override{
		cout<<"Child class..."<<endl;
	}
}
class GrandChild : public Child
{
public:
    void test() override
    {
        cout << "Child class...";
    }
};
```


## POD类型

POD是英文中 **Plain Old Data** 的缩写,翻译过来就是普通的旧数据 。POD在C++中是非常重要的一个概念,**通常用于说明一个类型的属性,尤其是用户自定义类型的属性**。

1. `Plain` ：表示是个普通的类型
2. `Old` ：体现了其与C的兼容性,支持标准C函数

在C++11中将 POD划分为两个基本概念的合集,即∶平凡的（trivial） 和标准布局的（standard layout ） 。
### 平凡类型

一个平凡的类或者结构体应该符合以下几点要求：
1. 拥有平凡的<font color=#ff0000>默认</font>构 造函数（trivial constructor）和析构函数（trivial destructor）。平凡的默认构造函数就是说构造函数`什么都不干`。
	- 通常情况下,`不定义类的构造函数`,编译器就会为我们`生成一个平凡的默认构造函数`。
	```c++
	// 使用默认的构造函数
	class Test {};
	```
	- `一旦定义了构造函数`,即使构造函数不包含参数,函数体里也没有任何的代码,`那么该构造函数也不再是"平凡"的`。
	```c++
	class Test1 
	{
	    Test1();	// 程序猿定义的构造函数, 非默认构造
	};
	```
	关于析构函数也和上面列举的构造函数类似,一旦被定义就不平凡了。但是这也并非无药可救,使用=default关键字可以显式地声明默认的构造函数,从而使得类型恢复 “平凡化”。

2. 拥有平凡的拷贝构造函数（trivial copy constructor）和移动构造函数（trivial move constructor）
	- 平凡的拷贝构造函数基本上等同于使用memcpy 进行类型的构造。
	- 同平凡的默认构造函数一样,不声明拷贝构造函数的话,编译器会帮程序员自动地生成。
	- 可以显式地使用=default 声明默认拷贝构造函数。 
	- 而平凡移动构造函数跟平凡的拷贝构造函数类似,只不过是用于移动语义。

3. 拥有平凡的拷贝赋值运算符（trivial assignment operator）和移动赋值运算符（trivial move operator）。
	这基本上与平凡的拷贝构造函数和平凡的移动构造运算符类似。
4. 不包含虚函数以及虚基类。
	- 类中使用`virtual 关键字修饰的函数` 叫做虚函数
	```c++
	class Base 
	{
	public:
	    Base() {}
	    virtual void print() {}
	};
	```
	- 虚基类是在`创建子类的时候在继承的基类前加virtual` 关键字 修饰
	```c++
	语法: class 派生类名：virtual  继承方式  基类名
	```

示例代码：
```c++
class Base 
{
public:
    Base() {}
};
// 子类Child,虚基类：Base
class Child : virtual public Base 
{
    Child() {}
};
```

### "标准布局”类型

标准布局类型主要主要指的是**类**或者**结构体**的结构或者组合方式。
标准布局类型的类应该符合以下五点定义,最重要的为前两条：
1. 所有非静态成员有相同 的访问权限（public,private,protected）
	- 类成员拥有相同的访问权限（标准布局类型）

	```c++
	class Base
	{
	public:
	    Base() {}
	    int a;
	    int b;
	    int c;
	};
	```

2. 在类或者结构体继承时,满足以下两种情况之一∶ 
	- 派生类中有非静态成员,基类中包含静态成员（或基类没有变量）。
	- 基类有非静态成员,而派生类没有非静态成员。
	```c++
	struct Base { static int a;};
	struct Child: public Base{ int b;};          // ok
	struct Base1 { int a;};
	struct Child1: public Base1{ static int c;}; // ok
	struct Child2:public Base, public Base1 { static int d;); // ok
	struct Child3:public Base1{ int d;};         // error
	struct Child4:public Base1, public Child     // error
	{
	    static int num;
	};
	```


> 通过上述例子得到的结论：
> 1. 非静态成员只要同时出现在派生类和基类间,即不属于标准布局。
> 2. 对于多重继承,一旦非静态成员出现在多个基类中,即使派生类中没有非静态成员变量,派生类也不属于标准布局。

3. 子类中第一个非静态成员的类型与其基类不同。
	```c++
	struct Parent{};
	struct Child : public Parent
	{
	    Parent p;	// 子类的第一个非静态成员
	    int foo;
	};
	```

	上面的例子中`Child`不是一个标准布局类型,因为它的第一个非静态成员变量`p`和父类的类型相同,改成下面这样子类就变成了一个标准布局类型：
	```c++
	struct Parent{};
	struct Child1 : public Parent
	{
	    int foo;   // 子类的第一个非静态成员
	    Parent p;	
	};
	```

	这条规则对于我们来说是比较特别的,这样规定的目的主要是是节约内存,提高数据的读取效率。对于上面的两个子类`Child`和`Child1`来说它们的内存结构是不一样的,<font color=#ff0000>在基类没有成员的情况下</font>：

	- C++标准允许`标准布局类型（Child1）`派生类的第一个`成员foo与基类共享地址`,此时基类并没有占据任何的实际空间（可以节省一点数据）
	- 对于子类`Child`而言,如果子类的第一个成员仍然是基类类型,C++标准要求类型相同的对象它们的地址必须不同（`基类地址不能和子类中的变量p类型相同`）,此时需要分配额外的地址空间将二者的地址错开。

<img src="https://subingwen.cn/cpp/POD/image-20211216174452356.png" alt="image.png" style="zoom:60%;" />

### 对 POD 类型的判断

#### 对“平凡”类型判断
C++11提供的类模板叫做 is_trivial,其定义如下：
```c++
template<class T>struct std::is_trivial;
```

`std::is_trivial` 的成员`value` 可以用于判断T的类型是否是一个平凡的类型（`value 函数返回值为布尔类型`）。除了类和结构体外,`is_trivial`还可以对内置的标准类型数据（比如int、float都属于平凡类型）及数组类型（元素是平凡类型的数组总是平凡的）进行判断。

```c++
#include <iostream>
#include <type_traits>
using namespace std;

class A {};
class B { B() {} };
class C : B {};
class D { virtual void fn() {} };
class E : virtual public A { };

int main() 
{
    cout << std::boolalpha;
    cout << "is_trivial:" << std::endl;
    cout << "int: " << is_trivial<int>::value << endl;
    cout << "A: " << is_trivial<A>::value << endl;
    cout << "B: " << is_trivial<B>::value << endl;
    cout << "C: " << is_trivial<C>::value << endl;
    cout << "D: " << is_trivial<D>::value << endl;
    cout << "E: " << is_trivial<E>::value << endl;
    return 0;
}
```

输出的结果：
```c++
is_trivial:
int: true
A: true
B: false
C: false
D: false
E: false
```

- **int** ：内置标准数据类型,属于 trivial 类型
- **A** ：拥有默认的构造和析构函数,属于 trivial 类型
- **B** ：自定义了构造函数,因此不属于 trivial 类型
- **C** ：基类中自定义了构造函数,因此不属于 trivial 类型
- **D** ：类成员函数中有虚函数,因此不属于 trivial 类型
- **E** ：继承关系中有虚基类,因此不属于 trivial 类型

#### 对“标准布局”类型的判断
在C++11中,我们可以使用模板类来帮助判断类型是否是一个标准布局的类型,其定义如下：
```c++
template <typename T> struct std::is_standard_layout;
```
通过 `is_standard_layout`模板类的成员 `value（is_standard_layout<T>∶∶value）`,我们可以在代码中打印出类型的标准布局属性,函数返回值为布尔类型。

**示例程序**
```c++
// pod.cpp
struct A { };
struct B : A { int j; };
struct C
{
public:
    int a;
private:
    int c;
};
struct D1 {  static int i; };
struct D2 {  int i; };
struct E1 { static int i; };
struct E2 { int i; };
struct D : public D1, public E1 { int a; };
struct E : public D1, public E2 { int a; };
struct F : public D2, public E2 { static int a; };
struct G : public A
{
    int foo;
    A a;
};
struct H : public A
{
    A a;
    int foo;
};
int main() 
{
    cout << std::boolalpha;
    cout << "is_standard_layout:" << std::endl;
    cout << "A: " << is_standard_layout<A>::value << endl;
    cout << "B: " << is_standard_layout<B>::value << endl;
    cout << "C: " << is_standard_layout<C>::value << endl;
    cout << "D: " << is_standard_layout<D>::value << endl;
    cout << "D1: " << is_standard_layout<D1>::value << endl;
    cout << "E: " << is_standard_layout<E>::value << endl;
    cout << "F: " << is_standard_layout<F>::value << endl;
    cout << "G: " << is_standard_layout<G>::value << endl;
    cout << "H: " << is_standard_layout<H>::value << endl;
    return 0;
}
```


- 输出的结果
```c++
is_standard_layout:
A: true
B: true
C: false
D: true
D1: true
E: false
F: false
G: false
H: false
```


**A** ：没有虚基类和虚函数,属于 standard_layout 类型
**B** ：没有虚基类和虚函数,属于 standard_layout 类型
**C** ：所有非静态成员访问权限不一致,不属于 standard_layout 类型
**D** ：基类和子类没有同时出现非静态成员变量,属于 standard_layout 类型
**D1** ：没有虚基类和虚函数,属于 standard_layout 类型
**E** ：基类和子类中同时出现了非静态成员变量,不属于 standard_layout 类型
**F** ：多重继承中在基类里同时出现了非静态成员变量,不属于 standard_layout 类型
**G** ：使用的编译器不同,得到的结果也不同。
**H** ：子类中第一个非静态成员的类型与其基类类型不能相同,不属于 standard_layout 类型
## 默认函数控制 =default 与 =delete

在C++中声明自定义的类,编译器会默认帮助程序员生成一些他们未自定义的成员函数。这样的函数版本被称为”默认函数”。这样的函数一共有六个

1. **无参构造函数**：创建类对象
2. **拷贝构造函数**：拷贝类对象
3. **移动构造函数**：拷贝类对象
4. **拷贝赋值函数**：类对象赋值
5. **移动赋值函数**：类对象赋值
6. **析构函数** ：销毁类对象

<font color=#ff0000>在C++语法规则中,一旦程序员实现了这些函数的自定义版本,则编译器不会再为该类自动生成默认版本。</font>

一旦声明了自定义版本的构造函数,则有可能导致我们定义的类型不再是[[C++#POD类型|POD类型]],我们便不再能够享受POD类型为我们带来的便利。

### default 和 =delete
在C++11标准中称= default修饰的函数为`显式默认【缺省】（explicit defaulted）函数`,而称=delete修饰的函数为删除（deleted）函数或者显示删除函数。
#### =default

它的作用是应该实现这个函数,但我不想自己写

```c++
class Base
{
public:
    Base() = default;
    Base(const Base& obj) = default;
    Base(Base&& obj) = default;
    Base& operator= (const Base& obj) = default;
    Base& operator= (Base&& obj) = default;
    ~Base() = default;
};
```

- 第4行：指定无参构造为默认函数
- 第5行：指定拷贝构造函数为默认函数
- 第6行：指定移动构造函数为默认函数
- 第7行：指定复制赋值操作符重载函数为默认函数
- 第8行：指定移动赋值操作符重载函数为默认函数
- 第9行：指定析构函数为默认函数

**使用 =defaut 指定的默认函数和类提供的默认函数是等价的**

**在类外部指定函数为默认函数**
默认函数除了在类定义的内部指定,也可以在类的外部指定,如下所示：
```c++
// 类定义
class Base
{
public:
    Base();
    Base(const Base& obj);
    Base(Base&& obj);
    Base& operator= (const Base& obj);
    Base& operator= (Base&& obj);
    ~Base();
};
// 在类定义之外指定成员函数为默认函数
Base::Base() = default;
Base::Base(const Base& obj) = default;
Base::Base(Base&& obj) = default;
Base& Base::operator= (const Base& obj) = default;
Base& Base::operator= (Base&& obj) = default;
Base::~Base() = default;
```


> 如果程序猿对C++类提供的默认函数（上面提到的六个函数）进行了实现,那么可以通过 =default 将他们再次指定为默认函数,不能使用 =default 修饰这六个函数以外的函数。

```c++
class Base
{
public:
    Base() = default;
    Base(const Base& obj) = default;
    Base(Base&& obj) = default;
    Base& operator= (const Base& obj) = default;
    Base& operator= (Base&& obj) = default;
    ~Base() = default;

    // 以下写法全部都是错误的
    Base(int a = 0) = default;
    Base(int a, int b) = default;
    void print() = default;
    bool operator== (const Base& obj) = default;
    bool operator>=(const Base& obj) = default;
};
```

- 第12行：自定义带参构造,不允许使用 **=default** 修饰（即使有默认参数也不行）
- 第13行：自定义带参构造,不允许使用 **=default** 修饰
- 第14行：自定义函数,不允许使用 **=default** 修饰
- 第15、16行：不是移动、复制赋值运算符重载,不允许使用 **=default** 修饰

#### =delete
=delete 表示显示删除,`显式删除可以避免用户使用一些不应该使用的类的成员函数`,使用这种方式可以有效的防止某些类型之间自动进行隐式类型转换产生的错误。下面举例说明：

禁止使用默认生成的函数
```c++
class Base
{
public:
    Base() = default;
    Base(const Base& obj) = delete;
    Base& operator= (const Base& obj) = delete;
};

int main()
{
    Base b;
    Base tmp1(b);    // error
	tmp1 = b;    // error
    return 0;
}
```

- 第5行：禁用拷贝构造函数
- 第6行：禁用 = 进行对象复制
- 第12行：拷贝构造函数已被显示删除,无法拷贝对象
- 第13行：复制对象的赋值操作符重载函数已被显示删除,无法复制对象

**禁止使用自定义函数**

```c++
class Base
{
public:
    Base(int num) : m_num(num) {}
    Base(char c) = delete;
    void print(char c) = delete;
    void print()
    {
        cout << "num: " << m_num << endl;
    }
    void print(int num)
    {
        cout << "num: " << num << endl;
    }
private:
    int m_num;
};

int main()
{
    Base b(97);       // 'a' 对应的 acscii 值为97
    Base b1('a');     // error
    b.print();
    b.print(97);
    b.print('a');     // error
    return 0;
}
```

- 第5行：禁用带 `char`类型参数的构造函数,防止隐式类型转换（char转int)
- 第6行：禁止使用带`char`类型的自定义函数,防止隐式类型转换（char转int)
- 第22行：对应的构造函数被禁用,因此无法使用该构造函数构造对象
- 第25行：对应的打印函数被禁用,因此无法给函数传递char类型参数

## using的使用

### 定义别名
在 C++中可以通过 typedef 重定义一个类型,语法格式如下：
```c++
typedef 旧的类型名 新的类型名;
// 使用举例
typedef unsigned int uint_t;
```

C++11中规定了一种新的方法,使用别名声明(alias declaration)来定义类型的别名,即使用using。

类型别名和类型的名字等价,只要是类型的名字能出现的地方,就能使用类型别名。使用typedef定义的别名和使用using定义的别名在语义上是等效的。

使用using定义别名的语法格式是这样的：
```c++
using 新的类型 = 旧的类型;
// 使用举例
using uint_t = int;
```

通过using和typedef的语法格式可以看到二者的使用没有太大的区别,假设我们定义一个函数指针,using的优势就能凸显出来了,看一下下面的例子：
```c++
// 使用typedef定义函数指针
typedef int(*func_ptr)(int, double);

// 使用using定义函数指针
using func_ptr1 = int(*)(int, double);
```

使用using定义函数指针别名的写法看起来就非常直观了,`把别名的名字强制分离到了左边,而把别名对应的实际类型放在了右边`,比较清晰,可读性比较好。

### 模板的别名
使用typedef重定义类似很方便,但是它有一点限制,比如无法重定义一个模板,比如我们需要一个固定以int类型为key的map,它可以和很多类型的value值进行映射,如果使用typedef这样直接定义就非常麻烦:

```c++
typedef map<int, string> m1;
typedef map<int, int> m2;
typedef map<int, double> m3;
```

```c++
template <typename T>
typedef map<int, T> type;	// error, 语法错误
```

使用typename不支持给模板定义别名,这个简单的需求仅通过typedef很难办到,需要添加一个外敷类：
```c++
#include <iostream>
#include <functional>
#include <map>
using namespace std;

template <typename T>
// 定义外敷类
struct MyMap
{
    typedef map<int, T> type;
};

int main(void)
{
    MyMap<string>::type m;
    m.insert(make_pair(1, "luffy"));
    m.insert(make_pair(2, "ace"));

    MyMap<int>::type m1;
    m1.insert(1, 100);
    m1.insert(2, 200);

    return 0;
}
```

通过上边的例子可以直观的感觉到,需求简单但是实现起来并不容易。在C++11中,新增了一个特性就是可以通过使用using来为一个模板定义别名,对于上面的需求可以写成这样:
```c++
template <typename T>
using mymap = map<int, T>;
```

完整的示例代码如下:
```c++
#include <iostream>
#include <functional>
#include <map>
using namespace std;

template <typename T>
using mymap = map<int, T>;

int main(void)
{
    // map的value指定为string类型
    mymap<string> m;
    m.insert(make_pair(1, "luffy"));
    m.insert(make_pair(2, "ace"));

    // map的value指定为int类型
    mymap<int> m1;
    m1.insert(1, 100);
    m1.insert(2, 200);

    return 0;
}
```

上面的例子中通过使用using给模板指定别名,就可以基于别名非常方便的给value指定相应的类型,这样使编写的程序变得更加灵活,看起来也更加简洁一些。

<font color=#ff0000>最后在强调一点：using语法和typedef一样,并不会创建出新的类型,它们只是给某些类型定义了新的别名。using相较于typedef的优势在于定义函数指针别名时看起来更加直观,并且可以给模板定义别名。</font>

## 智能指针

在C++中没有**垃圾回收机制**,必须自己释放分配的内存,否则就会造成内存泄露。解决这个问题最有效的方法是使用**智能指针**（smart pointer）。智能指针是存储指向动态分配（堆）对象指针的**类**,用于生存期的控制,能够确保在离开指针所在**作用域**时,自动地销毁动态分配的对象,防止内存泄露。智能指针的核心实现技术是**引用计数**,每使用它一次,内部引用计数加1,每析构一次内部的引用计数减1,减为0时,删除所指向的堆内存。

C++11中提供了三种智能指针,使用这些智能指针时需要引用头文件`<memory>`：

- `std::shared_ptr`：共享的智能指针
- `std::unique_ptr`：独占的智能指针
- `std::weak_ptr`：弱引用的智能指针,它不共享指针,不能操作资源,是用来监视shared_ptr的。

### 共享智能指针

#### shared_ptr的初始化
共享智能指针是指多个智能指针可以**同时管理**同一块有效的内存,共享智能指针`shared_ptr` 是一个模板类,如果要进行初始化有三种方式：通过构造函数、`std::make_shared`辅助函数以及`reset`方法。共享智能指针对象初始化完毕之后就指向了要管理的那块堆内存,如果想要查看当前有多少个智能指针同时管理着这块内存可以使用共享智能指针提供的一个成员函数use_count,函数原型如下：


```c++
// shared_ptr<T> 类模板中,提供了多种实用的构造函数, 语法格式如下:
std::shared_ptr<T> 智能指针名字(创建堆内存);
```

##### 通过构造函数初始化

```c++
// shared_ptr<T> 类模板中,提供了多种实用的构造函数, 语法格式如下:
std::shared_ptr<T> 智能指针名字(创建堆内存);
```

```c++
#include <iostream>
#include <memory>
using namespace std;

int main()
{
    // 使用智能指针管理一块 int 型的堆内存
    shared_ptr<int> ptr1(new int(520));
    cout << "ptr1管理的内存引用计数: " << ptr1.use_count() << endl;
    // 使用智能指针管理一块字符数组对应的堆内存
    shared_ptr<char> ptr2(new char[12]);
    cout << "ptr2管理的内存引用计数: " << ptr2.use_count() << endl;
    // 创建智能指针对象, 不管理任何内存
    shared_ptr<int> ptr3;
    cout << "ptr3管理的内存引用计数: " << ptr3.use_count() << endl;
    // 创建智能指针对象, 初始化为空
    shared_ptr<int> ptr4(nullptr);
    cout << "ptr4管理的内存引用计数: " << ptr4.use_count() << endl;
    return 0;
}
```

测试代码输出的结果如下:
```c++
ptr1管理的内存引用计数: 1
ptr2管理的内存引用计数: 1
ptr3管理的内存引用计数: 0
ptr4管理的内存引用计数: 0
```

注意:
> 如果智能指针被初始化了一块有效内存,那么这块内存的引用计数+1,如果智能指针没有被初始化或者被初始化为nullptr空指针,引用计数不会+1。另外,不要使用一个原始指针初始化多个shared_ptr。

```c++
int *p = new int;
shared_ptr<int> p1(p);
shared_ptr<int> p2(p);		// error, 编译不会报错, 运行会出错
```

##### 通过拷贝和移动构造函数初始化
当一个智能指针被初始化之后,就可以通过这个**智能指针初始化其他新对象**。在创建新对象的时候,对应的拷贝构造函数或者移动构造函数就被自动调用了。

```c++
#include <iostream>
#include <memory>
using namespace std;

int main()
{
    // 使用智能指针管理一块 int 型的堆内存, 内部引用计数为 1
    shared_ptr<int> ptr1(new int(520));
    cout << "ptr1管理的内存引用计数: " << ptr1.use_count() << endl;
    //调用拷贝构造函数
    shared_ptr<int> ptr2(ptr1);
    cout << "ptr2管理的内存引用计数: " << ptr2.use_count() << endl;
    shared_ptr<int> ptr3 = ptr1;
    cout << "ptr3管理的内存引用计数: " << ptr3.use_count() << endl;
    //调用移动构造函数
    shared_ptr<int> ptr4(std::move(ptr1));
    cout << "ptr4管理的内存引用计数: " << ptr4.use_count() << endl;
    std::shared_ptr<int> ptr5 = std::move(ptr2);
    cout << "ptr5管理的内存引用计数: " << ptr5.use_count() << endl;
    return 0;
}
```


测试程序输入的结果：
```c++
ptr1管理的内存引用计数: 1
ptr2管理的内存引用计数: 2
ptr3管理的内存引用计数: 3
ptr4管理的内存引用计数: 3
ptr5管理的内存引用计数: 3
```

如果使用拷贝的方式初始化共享智能指针对象,这两个对象会同时管理同一块堆内存,堆内存对应的引用计数也会增加；如果使用移动的方式初始智能指针对象,只是**转让了内存的所有权**(原来的指针就没有内存所有权了,指向`nullptr`),管理内存的对象并不会增加,因此内存的引用计数不会变化。

##### 通过std::make_shared初始化

通过C++提供的`std::make_shared()` 就可以完成内存对象的创建并将其初始化给智能指针,函数原型如下：

```c++
template< class T, class... Args >
shared_ptr<T> make_shared(Args&&... args );
```

- `T`：模板参数的数据类型
- `Args&&... args` ：要初始化的数据,如果是通过make_shared创建对象,需按照构造函数的参数列表指定

```c++
#include <iostream>
#include <string>
#include <memory>
using namespace std;

class Test
{
public:
    Test() 
    {
        cout << "construct Test..." << endl;
    }
    Test(int x) 
    {
        cout << "construct Test, x = " << x << endl;
    }
    Test(string str) 
    {
        cout << "construct Test, str = " << str << endl;
    }
    ~Test()
    {
        cout << "destruct Test ..." << endl;
    }
};
int main()
{
    // 使用智能指针管理一块 int 型的堆内存, 内部引用计数为 1
    shared_ptr<int> ptr1 = make_shared<int>(520);
    cout << "ptr1管理的内存引用计数: " << ptr1.use_count() << endl;

    shared_ptr<Test> ptr2 = make_shared<Test>();
    cout << "ptr2管理的内存引用计数: " << ptr2.use_count() << endl;

    shared_ptr<Test> ptr3 = make_shared<Test>(520);
    cout << "ptr3管理的内存引用计数: " << ptr3.use_count() << endl;

    shared_ptr<Test> ptr4 = make_shared<Test>("我是要成为海贼王的男人!!!");
    cout << "ptr4管理的内存引用计数: " << ptr4.use_count() << endl;
    return 0;
}
```

<font color=#ff0000>注意</font>:
使用`std::make_shared()`模板函数可以完成内存地址的创建,并将最终得到的内存地址传递给共享智能指针对象管理。如果申请的内存是普通类型,通过函数的（）可完成地址的初始化,如果要创建一个类对象,函数的（）内部需要指定构造对象需要的参数,也就是<font color=#ff0000>类构造函数的参数</font>
`std::make_shared` 不直接支持动态数组的初始化
##### 通过 reset方法初始化
共享智能指针类提供的`std::shared_ptr::reset`方法函数原型如下：
```c++
void reset() noexcept;
template< class Y >
void reset( Y* ptr );
template< class Y, class Deleter >
void reset( Y* ptr, Deleter d );
template< class Y, class Deleter, class Alloc >
void reset( Y* ptr, Deleter d, Alloc alloc );
```

- `ptr`：指向要取得所有权的对象的指针
- `d`：指向要取得所有权的对象的指针
- `aloc`：内部存储所用的分配器

测试代码如下：
```c++
#include <iostream>
#include <string>
#include <memory>
using namespace std;

int main()
{
    // 使用智能指针管理一块 int 型的堆内存, 内部引用计数为 1
    shared_ptr<int> ptr1 = make_shared<int>(520);
    shared_ptr<int> ptr2 = ptr1;
    shared_ptr<int> ptr3 = ptr1;
    shared_ptr<int> ptr4 = ptr1;
    cout << "ptr1管理的内存引用计数: " << ptr1.use_count() << endl;
    cout << "ptr2管理的内存引用计数: " << ptr2.use_count() << endl;
    cout << "ptr3管理的内存引用计数: " << ptr3.use_count() << endl;
    cout << "ptr4管理的内存引用计数: " << ptr4.use_count() << endl;

    ptr4.reset();
    cout << "ptr1管理的内存引用计数: " << ptr1.use_count() << endl;
    cout << "ptr2管理的内存引用计数: " << ptr2.use_count() << endl;
    cout << "ptr3管理的内存引用计数: " << ptr3.use_count() << endl;
    cout << "ptr4管理的内存引用计数: " << ptr4.use_count() << endl;
    shared_ptr<int> ptr5;
    ptr5.reset(new int(250));
    cout << "ptr5管理的内存引用计数: " << ptr5.use_count() << endl;
    return 0;
}
```

测试代码输入的结果:
```c++
ptr1管理的内存引用计数: 4
ptr2管理的内存引用计数: 4
ptr3管理的内存引用计数: 4
ptr4管理的内存引用计数: 4   
ptr1管理的内存引用计数: 3
ptr2管理的内存引用计数: 3
ptr3管理的内存引用计数: 3
ptr4管理的内存引用计数: 0
ptr5管理的内存引用计数: 1
```


对于一个未初始化的共享智能指针,可以通过reset方法来初始化,当智能指针中有值的时候,调用reset会使引用计数减1。

```c++
struct MyClass {
    MyClass() { std::cout << "MyClass constructed\n"; }
    ~MyClass() { std::cout << "MyClass destructed\n"; }
};

// 自定义删除器
void myDeleter(MyClass* p) {
  std::cout << "Using custom deleter to delete MyClass\n";
  delete p;
}

int main() {
  std::shared_ptr<MyClass> ptr(new MyClass);

  ptr.reset(new MyClass, myDeleter);  // 使用自定义删除器
}
```

`reset`成员函数用于将`shared_ptr`重新指向一个新的指针,同时递减**原先所指向的对象的引用计数**。如果原先的引用计数变为0,则释放原先的对象。除了简单的重新指定新的对象之外,`reset`也可以让你提供自定义的删除器和分配器。

##### 获取原始指针

通过智能指针可以管理一个普通变量或者对象的地址,此时原始地址就不可见了。当我们想要修改变量或者对象中的值的时候,就需要从智能指针对象中先取出数据的原始内存的地址再操作,解决方案是调用共享智能指针类提供的`get()`方法,其函数原型如下：
```c++
T* get() const noexcept;
```

测试代码如下:
```c++
#include <iostream>
#include <string>
#include <memory>
using namespace std;

int main()
{
    int len = 128;
    shared_ptr<char> ptr(new char[len]);
    // 得到指针的原始地址
    char* add = ptr.get();
    memset(add, 0, len);
    strcpy(add, "我是要成为海贼王的男人!!!");
    cout << "string: " << add << endl;
    
    shared_ptr<int> p(new int);
    *p = 100;
    cout << *p.get() << "  " << *p << endl;
    
    return 0;
}
```

#### 指定删除器
当智能指针管理的内存对应的引用计数变为0的时候,这块内存就会被智能指针析构掉了。另外,我们在初始化智能指针的时候也可以自己指定删除动作,这个删除操作对应的函数被称之为删除器,这个删除器函数本质是一个**回调函数**,我们只需要进行实现,其调用是由智能指针完成的。

```c++
#include <iostream>
#include <memory>
using namespace std;

// 自定义删除器函数,释放int型内存
void deleteIntPtr(int* p)
{
    delete p;
    cout << "int 型内存被释放了...";
}

int main()
{
    shared_ptr<int> ptr(new int(250), deleteIntPtr);
    return 0;
}
```

删除器函数也可以是lambda表达式,因此代码也可以写成下面这样：

```c++
int main()
{
    shared_ptr<int> ptr(new int(250), [](int* p) {delete p; });
    return 0;
}
```

在上面的代码中,lambda表达式的参数就是智能指针管理的内存的地址,有了这个地址之后函数体内部就可以完成删除操作了。
在C++11中使用`shared_ptr`管理动态数组时,需要指定删除器,因为`std::shared_ptr`的默认删除器不支持数组对象,具体的处理代码如下：

```c++
int main()
{
    shared_ptr<int> ptr(new int[10], [](int* p) {delete[]p; });
    return 0;
}
```

另外,我们还可以自己封装一个make_shared_array方法来让shared_ptr支持数组,代码如下:

```c++
#include <iostream>
#include <memory>
using namespace std;

template <typename T>
shared_ptr<T> make_share_array(size_t size)
{
    // 返回匿名对象
    return shared_ptr<T>(new T[size], default_delete<T[]>());
}

int main()
{
    shared_ptr<int> ptr1 = make_share_array<int>(10);
    cout << ptr1.use_count() << endl;
    shared_ptr<char> ptr2 = make_share_array<char>(128);
    cout << ptr2.use_count() << endl;
    return 0;
}
```

### 独占的智能指针

#### 初始化
`std::unique_ptr`是一个独占型的智能指针,它不允许其他的智能指针共享其内部的指针,可以通过它的构造函数初始化一个独占智能指针对象,但是不允许通过赋值将一个unique_ptr赋值给另一个unique_ptr。

```c++
// 通过构造函数初始化对象
unique_ptr<int> ptr1(new int(10));
// error, 不允许将一个unique_ptr赋值给另一个unique_ptr
unique_ptr<int> ptr2 = ptr1;
```

`std::unique_ptr`不允许复制,但是可以通过函数返回给其他的`std::unique_ptr`,还可以通过`std::move`来转译给其他的`std::unique_ptr`,这样原始指针的所有权就被转移了,这个原始指针还是被独占的。

```c++
#include <iostream>
#include <memory>
using namespace std;

unique_ptr<int> func()
{
    return unique_ptr<int>(new int(520));
}

int main()
{
    // 通过构造函数初始化
    unique_ptr<int> ptr1(new int(10));
    // 通过转移所有权的方式初始化
    unique_ptr<int> ptr2 = move(ptr1);
    unique_ptr<int> ptr3 = func();

    return 0;
}
```

`unique_ptr`独占智能指针类也有一个reset方法,函数原型如下：

```c++
void reset( pointer ptr = pointer() ) noexcept;
```

使用reset方法可以让unique_ptr解除对原始内存的管理,也可以用来初始化一个独占的智能指针。

```c++
int main()
{
    unique_ptr<int> ptr1(new int(10));
    unique_ptr<int> ptr2 = move(ptr1);

    ptr1.reset();
    ptr2.reset(new int(250));

    return 0;
}
```

- `ptr1.reset()`;解除对原始内存的管理
- `ptr2.reset(new int(250))`;重新指定智能指针管理的原始内存

如果想要获取独占智能指针管理的原始地址,可以调用get()方法,函数原型如下：

```c++
pointer get() const noexcept;
```

```c++
int main()
{
    unique_ptr<int> ptr1(new int(10));
    unique_ptr<int> ptr2 = move(ptr1);

    ptr2.reset(new int(250));
    cout << *ptr2.get() << endl;	// 得到内存地址中存储的实际数值 250

    return 0;
}
```

#### 删除器
unique_ptr指定删除器和shared_ptr指定删除器是有区别的,unique_ptr指定删除器的时候需要确定删除器的类型,所以不能像shared_ptr那样直接指定删除器,举例说明：random_device

```c++
shared_ptr<int> ptr1(new int(10), [](int*p) {delete p; });	// ok
unique_ptr<int> ptr1(new int(10), [](int*p) {delete p; });	// error

int main()
{
    using func_ptr = void(*)(int*);
    unique_ptr<int, func_ptr> ptr1(new int(10), [](int*p) {delete p; });
	unique_ptr<int, function<void(int*)>> ptr1(new int(10), [](int*p) {delete p; });
    return 0;
} 
```

在上面的代码中第7行,`func_ptr`的类型和`lambda`表达式的类型是一致的。在lambda表达式没有捕获任何变量的情况下是正确的,如果捕获了变量,编译时则会报错：

```c++
int main()
{
    using func_ptr = void(*)(int*);
    unique_ptr<int, func_ptr> ptr1(new int(10), [&](int*p) {delete p; });	// error
    return 0;
}
```

上面的代码中错误原因是这样的,在lambda表达式没有捕获任何外部变量时,可以直接转换为函数指针,一旦捕获了就无法转换了,如果想要让编译器成功通过编译,那么需要使用可调用对象包装器来处理声明的函数指针：

```c++
int main()
{
    using func_ptr = void(*)(int*);
    unique_ptr<int, function<void(int*)>> ptr1(new int(10), [&](int*p) {delete p; });
    return 0;
}
```

### 弱引用智能指针

#### 基本使用方法
弱引用智能指针`std::weak_ptr`可以看做是`shared_ptr`的助手,它不管理shared_ptr内部的指针。`std::weak_ptr`没有重载操作符`*`和`->`,因为它不共享指针,不能操作资源,所以它的构造不会增加引用计数,析构也不会减少引用计数,它的主要作用就是作为一个旁观者监视`shared_ptr`中管理的资源是否存在。

##### 初始化

```c++
// 默认构造函数
constexpr weak_ptr() noexcept;
// 拷贝构造
weak_ptr (const weak_ptr& x) noexcept;
template <class U> weak_ptr (const weak_ptr<U>& x) noexcept;
// 通过shared_ptr对象构造
template <class U> weak_ptr (const shared_ptr<U>& x) noexcept;
```

在C++11中,`weak_ptr`的初始化可以通过以上提供的构造函数来完成初始化,具体使用方法如下：
```c++
#include <iostream>
#include <memory>
using namespace std;
int main() 
{
    shared_ptr<int> sp(new int);
    weak_ptr<int> wp1;
    weak_ptr<int> wp2(wp1);
    weak_ptr<int> wp3(sp);
    weak_ptr<int> wp4;
    wp4 = sp;
    weak_ptr<int> wp5;
    wp5 = wp3;
    
    return 0;
}
```

- `weak_ptr<int> wp1`;构造了一个空weak_ptr对象
- `weak_ptr<int> wp2(wp1)`;通过一个空weak_ptr对象构造了另一个空weak_ptr对象
- `weak_ptr<int> wp3(sp)`;通过一个shared_ptr对象构造了一个可用的weak_ptr实例对象
- `wp4 = sp`;通过一个shared_ptr对象构造了一个可用的weak_ptr实例对象（这是一个隐式类型转换）
- `wp5 = wp3`;通过一个weak_ptr对象构造了一个可用的weak_ptr实例对象

##### 其他常用方法

###### use_count()
通过调用std::weak_ptr类提供的use_count()方法可以获得当前所观测资源的引用计数,函数原型如下：
```c++
// 函数返回所监测的资源的引用计数
long int use_count() const noexcept;
```

修改一下上面的测试程序,添加打印资源引用计数的代码:
```c++
#include <iostream>
#include <memory>
using namespace std;

int main() 
{
    shared_ptr<int> sp(new int);

    weak_ptr<int> wp1;
    weak_ptr<int> wp2(wp1);
    weak_ptr<int> wp3(sp);
    weak_ptr<int> wp4;
    wp4 = sp;
    weak_ptr<int> wp5;
    wp5 = wp3;

    cout << "use_count: " << endl;
    cout << "wp1: " << wp1.use_count() << endl;
    cout << "wp2: " << wp2.use_count() << endl;
    cout << "wp3: " << wp3.use_count() << endl;
    cout << "wp4: " << wp4.use_count() << endl;
    cout << "wp5: " << wp5.use_count() << endl;
    return 0;
}
```

测试程序输出的结果为:
```c++
use_count:
wp1: 0
wp2: 0
wp3: 1
wp4: 1
wp5: 1
```

通过打印的结果可以知道,虽然弱引用智能指针`wp3、wp4、wp5`监测的资源是同一个,但是它的引用计数并没有发生任何的变化,也进一步证明了`weak_ptr`只是监测资源,并不管理资源。

###### expired()
通过调用std::weak_ptr类提供的expired()方法来判断观测的资源是否已经被释放,函数原型如下：
```c++
// 返回true表示资源已经被释放, 返回false表示资源没有被释放
bool expired() const noexcept;
```

函数的使用方法如下:
```c++
#include <iostream>
#include <memory>
using namespace std;

int main() 
{
    shared_ptr<int> shared(new int(10));
    weak_ptr<int> weak(shared);
    cout << "1. weak " << (weak.expired() ? "is" : "is not") << " expired" << endl;

    shared.reset();
    cout << "2. weak " << (weak.expired() ? "is" : "is not") << " expired" << endl;

    return 0;
}
```

测试代码输出的结果:
```c++
1. weak is not expired
2. weak is expired
```

`weak_ptr`监测的就是`shared_ptr`管理的资源,当共享智能指针调用`shared.reset()`;之后管理的资源被释放,因此`weak.expired()`函数的结果返回true,表示监测的资源已经不存在了。

###### lock()
通过调用`std::weak_ptr`类提供的`lock()`方法来获取管理所监测资源的`shared_ptr`对象,函数原型如下：
```c++
shared_ptr<element_type> lock() const noexcept;
```
函数的使用方法如下:
```c++
#include <iostream>
#include <memory>
using namespace std;

int main()
{
    shared_ptr<int> sp1, sp2;
    weak_ptr<int> wp;

    sp1 = std::make_shared<int>(520);
    wp = sp1;
    sp2 = wp.lock();
    cout << "use_count: " << wp.use_count() << endl;

    sp1.reset();
    cout << "use_count: " << wp.use_count() << endl;

    sp1 = wp.lock();
    cout << "use_count: " << wp.use_count() << endl;

    cout << "*sp1: " << *sp1 << endl;
    cout << "*sp2: " << *sp2 << endl;

    return 0;
}
```

测试代码输出的结果为:
```c++
use_count: 2
use_count: 1
use_count: 2
*sp1: 520
*sp2: 520
```

- `sp2 = wp.lock()`;通过调用lock()方法得到一个用于管理weak_ptr对象所监测的资源的共享智能指针对象,使用这个对象初始化sp2,此时所监测资源的引用计数为2
- `sp1.reset()`;共享智能指针sp1被重置,weak_ptr对象所监测的资源的引用计数减1
- `sp1 = wp.lock()`;sp1重新被初始化,并且管理的还是weak_ptr对象所监测的资源,因此引用计数加1
- 共享智能指针对象sp1和sp2管理的是同一块内存,因此最终打印的内存中的结果是相同的,都是520

###### reset()
通过调用`std::weak_ptr`类提供的reset()方法来清空对象,使其不监测任何资源,函数原型如下：
```c++
void reset() noexcept;
```

函数的使用是非常简单的,示例代码如下：
```c++
#include <iostream>
#include <memory>
using namespace std;

int main() 
{
    shared_ptr<int> sp(new int(10));
    weak_ptr<int> wp(sp);
    cout << "1. wp " << (wp.expired() ? "is" : "is not") << " expired" << endl;

    wp.reset();
    cout << "2. wp " << (wp.expired() ? "is" : "is not") << " expired" << endl;

    return 0;
}
```

测试代码输出的结果为:
```c++
1. wp is not expired
2. wp is expired
```

`weak_ptr`对象sp被重置之后`wp.reset()`;变成了空对象,不再监测任何资源,因此`wp.expired()`返回true

#### 返回管理this的shared_ptr
如果在一个类中编写了一个函数,通过这个得到管理当前对象的共享智能指针,我们可能会写出如下代码：
```c++
#include <iostream>
#include <memory>
using namespace std;

struct Test
{
    shared_ptr<Test> getSharedPtr()
    {
        return shared_ptr<Test>(this);
    }
    
    ~Test()
    {
        cout << "class Test is disstruct ..." << endl;
    }

};

int main() 
{
    shared_ptr<Test> sp1(new Test);
    cout << "use_count: " << sp1.use_count() << endl;
    shared_ptr<Test> sp2 = sp1->getSharedPtr();
    cout << "use_count: " << sp1.use_count() << endl;
    return 0;
}
```

执行上面的测试代码,运行中会出现异常,在终端还是能看到对应的日志输出：
```c++
use_count: 1
use_count: 1
class Test is disstruct ...
class Test is disstruct ...
```


这个问题可以通过`weak_ptr`来解决,通过wake_ptr返回管理this资源的共享智能指针对象`shared_ptr`。C++11中为我们提供了一个模板类叫做`std::enable_shared_from_this<T>`,这个类中有一个方法叫做`shared_from_this()`,通过这个方法可以返回一个共享智能指针,在函数的内部就是使用`weak_ptr`来监测this对象,并通过调用`weak_ptr`的`lock()`方法返回一个shared_ptr对象。

修改之后的代码为：

```c++
#include <iostream>
#include <memory>
using namespace std;

struct Test:public enable_shared_from_this<Test>
{
    shared_ptr<Test> getSharedPtr()
    {
        return shared_from_this();
    }
    ~Test()
    {
        cout << "class Test is disstruct ..." << endl;
    }
};

int main() 
{
    shared_ptr<Test> sp1(new Test);
    cout << "use_count: " << sp1.use_count() << endl;
    shared_ptr<Test> sp2 = sp1->getSharedPtr();
    cout << "use_count: " << sp1.use_count() << endl;
    return 0;
}
```

测试代码输出的结果为:
```c++
use_count: 1
use_count: 2
class Test is disstruct ...
```

最后需要强调一个细节：<font color=#ff0000>在调用enable_shared_from_this类的shared_from_this()方法之前,必须要先初始化函数内部weak_ptr对象,否则该函数无法返回一个有效的shared_ptr对象（具体处理方法可以参考上面的示例代码）</font>。

#### 解决循环引用问题
智能指针如果循环引用会导致内存泄露,比如下面的例子：
```c++
#include <iostream>
#include <memory>
using namespace std;

struct TA;
struct TB;

struct TA
{
    shared_ptr<TB> bptr;
    ~TA()
    {
        cout << "class TA is disstruct ..." << endl;
    }
};

struct TB
{
    shared_ptr<TA> aptr;
    ~TB()
    {
        cout << "class TB is disstruct ..." << endl;
    }
};

void testPtr()
{
    shared_ptr<TA> ap(new TA);
    shared_ptr<TB> bp(new TB);
    cout << "TA object use_count: " << ap.use_count() << endl;
    cout << "TB object use_count: " << bp.use_count() << endl;

    ap->bptr = bp;
    bp->aptr = ap;
    cout << "TA object use_count: " << ap.use_count() << endl;
    cout << "TB object use_count: " << bp.use_count() << endl;
}

int main()
{
    testPtr();
    return 0;
}
```

测试程序输出的结果如下:
```c++
TA object use_count: 1
TB object use_count: 1
TA object use_count: 2
TB object use_count: 2
```

在测试程序中,共享智能指针`ap、bp`对TA、TB实例对象的引用计数变为2,在共享智能指针离开作用域之后引用计数只能减为1,这种情况下不会去删除智能指针管理的内存,导致类`TA、TB`的实例对象不能被析构,最终造成内存泄露。通过使用`weak_ptr`可以解决这个问题,只要将类TA或者TB的任意一个成员改为weak_ptr,修改之后的代码如下：

```c++
#include <iostream>
#include <memory>
using namespace std;

struct TA;
struct TB;

struct TA
{
    weak_ptr<TB> bptr;
    ~TA()
    {
        cout << "class TA is disstruct ..." << endl;
    }
};

struct TB
{
    shared_ptr<TA> aptr;
    ~TB()
    {
        cout << "class TB is disstruct ..." << endl;
    }
};

void testPtr()
{
    shared_ptr<TA> ap(new TA);
    shared_ptr<TB> bp(new TB);
    cout << "TA object use_count: " << ap.use_count() << endl;
    cout << "TB object use_count: " << bp.use_count() << endl;

    ap->bptr = bp;
    bp->aptr = ap;
    cout << "TA object use_count: " << ap.use_count() << endl;
    cout << "TB object use_count: " << bp.use_count() << endl;
}

int main()
{
    testPtr();
    return 0;
}
```

程序输出的结果:
```c++
TA object use_count: 1
TB object use_count: 1
TA object use_count: 2
TB object use_count: 1
class TB is disstruct ...
class TA is disstruct ...
```

通过输出的结果可以看到类`TA`或者`TB`的对象被成功析构了。

上面程序中,在对类`TA`成员赋值时`ap->bptr = bp`;由于`bptr`是`weak_ptr`类型,这个赋值操作并不会增加引用计数,所以`bp`的引用计数仍然为1,在离开作用域之后bp的引用计数减为0,类TB的实例对象被析构。

在类`TB`的实例对象被析构的时候,内部的`aptr`也被析构,其对TA对象的管理解除,内存的引用计数减为1,当共享智能指针ap离开作用域之后,对TA对象的管理也解除了,内存的引用计数减为0,类`TA`的实例对象被析构。


#### 智能指针解决多线程安全

```c++
class A{
public:
    A(){
        std::cout<<"A()"<<std::endl;
    }
    ~A(){
        std::cout<<"~A()"<<std::endl;
    }
    void testA(){
        std::cout<<"非常好用的方法!"<<std::endl;
    }
};

// 子进程
void handler01(std::weak_ptr<A>p){
    std::this_thread::sleep_for(std::chrono::seconds(1));
    // 我们需要检测指针的有效性 可以用智能指针的引用计数
    if(std::shared_ptr<A>q = p.lock()){
        q->testA();
    } else{
        std::cout<<"对象已经销毁"<<std::endl;
    }
}
// main线程
int main() {
  std::thread t1;
  {
    std::shared_ptr<A> p(new A());
     t1=std::thread(handler01, std::weak_ptr<A>(p));
  }
  t1.join();
}
```

- 利用弱引用指针获取指针的有效性，防止多线程情况下造成野指针的或者未定义的情况

什么时候会线程安全问题，这个博客有参考 [当我们谈论shared\_ptr的线程安全性时，我们在谈论什么 - 知乎](https://zhuanlan.zhihu.com/p/416289479)

## Lambda表达式

### 基本用法

lambda表达式是C++11最重要也是最常用的特性之一,这是现代编程语言的一个特点,lambda表达式有如下的一些优点：

- 声明式的编程风格：就地匿名定义目标函数或函数对象,不需要额外写一个命名函数或函数对象。
- 简洁：避免了代码膨胀和功能分散,让开发更加高效。
- 在需要的时间和地点实现功能闭包,使程序更加灵活。
lambda表达式定义了一个**匿名函数**,并且可以捕获一定范围内的变量。lambda表达式的语法形式简单归纳如下：

```c++
[capture](params) opt -> ret {body;};
```

其中capture是捕获列表,params是参数列表,opt是函数选项,ret是返回值类型,body是函数体。
1. 捕获列表`[]`: 捕获一定范围内的变量
2. 参数列表(): 和普通函数的参数列表一样,如果没有参数参数列表可以省略不写。
	```c++
	auto f = [](){return 1;}	// 没有参数, 参数列表为空
	auto f = []{return 1;}		// 没有参数, 参数列表省略不写
	```

3. opt 选项,不需要可以省略
	- `mutable`: 可以修改按值传递进来的拷贝（注意是能修改拷贝,而不是值本身）
	- `exception`: 指定函数抛出的异常,如抛出整数类型的异常,可以使用`throw()`;
4. 返回值类型：在C++11中,lambda表达式的返回值是通过返回值后置语法来定义的。
5. 函数体：函数的实现,这部分不能省略,但函数体可以为空

### 捕获列表
lambda表达式的捕获列表可以捕获一定范围内的变量,具体使用方式如下：

- `[]` - 不捕捉任何变量
- `[&]` - 捕获外部作用域中所有变量, 并作为引用在函数体内使用 (按引用捕获)
- `[x]` - 捕获外部作用域中所有变量, 并作为副本在函数体内使用 (按值捕获)
	- 拷贝的副本在匿名函数体内部是只读的
- `[=, &foo]` - 按值捕获外部作用域中所有变量, 并按照引用捕获外部变量 foo
- `[bar]` - 按值捕获 bar 变量, 同时不捕获其他变量
- `[&bar]` - 按引用捕获 bar 变量, 同时不捕获其他变量
- `[this]` - 捕获当前类中的this指针
	- 让lambda表达式拥有和当前类成员函数同样的访问权限
	- 如果已经使用了 & 或者 =, 默认添加此选项
下面通过一个例子,看一下初始化列表的具体用法：
```c++
#include <iostream>
#include <functional>
using namespace std;

class Test
{
public:
    void output(int x, int y)
    {
        auto x1 = [] {return m_number; };                      // error
        auto x2 = [=] {return m_number + x + y; };             // ok
        auto x3 = [&] {return m_number + x + y; };             // ok
        auto x4 = [this] {return m_number; };                  // ok
        auto x5 = [this] {return m_number + x + y; };          // error
        auto x6 = [this, x, y] {return m_number + x + y; };    // ok
        auto x7 = [this] {return m_number++; };                // ok
    }
    int m_number = 100;
};
```

- x1：错误,没有捕获外部变量,不能使用类成员 m_number
- x2：正确,以值拷贝的方式捕获所有外部变量
- x3：正确,以引用的方式捕获所有外部变量
- x4：正确,捕获this指针,可访问对象内部成员
- x5：错误,捕获this指针,可访问类内部成员,没有捕获到变量x,y,因此不能访问。
- x6：正确,捕获this指针,x,y
- x7：正确,捕获this指针,并且可以修改对象内部变量的值


```c++
int main(void)
{
    int a = 10, b = 20;
    auto f1 = [] {return a; };                        // error
    auto f2 = [&] {return a++; };                     // ok
    auto f3 = [=] {return a; };                       // ok
    auto f4 = [=] {return a++; };                     // error
    auto f5 = [a] {return a + b; };                   // error
    auto f6 = [a, &b] {return a + (b++); };           // ok
    auto f7 = [=, &b] {return a + (b++); };           // ok

    return 0;
}
```

- f1：错误,没有捕获外部变量,因此无法访问变量 a
- f2：正确,使用引用的方式捕获外部变量,可读写
- f3：正确,使用值拷贝的方式捕获外部变量,可读
- f4：错误,使用值拷贝的方式捕获外部变量,可读不能写
- f5：错误,使用拷贝的方式捕获了外部变量a,没有捕获外部变量b,因此无法访问变量b
- f6：正确,使用拷贝的方式捕获了外部变量a,只读,使用引用的方式捕获外部变量b,可读写
- f7：正确,使用值拷贝的方式捕获所有外部变量以及b的引用,b可读写,其他只读

注意:
> 在匿名函数内部,需要通过lambda表达式的捕获列表控制如何捕获外部变量,以及访问哪些变量。默认状态下lambda表达式无法修改通过复制方式捕获外部变量,如果希望修改这些外部变量,需要通过引用的方式进行捕获。

### 返回值
很多时候,lambda表达式的返回值是非常明显的,因此在C++11中允许省略lambda表达式的返回值。

```c++
// 完整的lambda表达式定义
auto f = [](int a) -> int
{
    return a+10;  
};

// 忽略返回值的lambda表达式定义
auto f = [](int a)
{
    return a+10;  
};
```

一般情况下,不指定lambda表达式的返回值,编译器会根据return语句自动推导返回值的类型,但需要注意的是<font color=#ff0000>labmda表达式不能通过列表初始化自动推导出返回值类型</font>。
```c++
// ok,可以自动推导出返回值类型
auto f = [](int i)
{
    return i;
}

// error,不能推导出返回值类型
auto f1 = []()
{
    return {1, 2};	// 基于列表初始化推导返回值,错误
}
```

### 函数本质
使用lambda表达式捕获列表捕获外部变量,如果希望去修改按值捕获的外部变量,那么应该如何处理呢？这就需要使用mutable选项,被mutable修改是lambda表达式就算没有参数也要写明参数列表,并且可以去掉按值捕获的外部变量的只读（const）属性。

```c++
int a = 0;
auto f1 = [=] {return a++; };              // error, 按值捕获外部变量, a是只读的
auto f2 = [=]()mutable {return a++; };     // ok
```

最后再剖析一下为什么通过值拷贝的方式捕获的外部变量是只读的:

- lambda表达式的类型在C++11中会被看做是一个带`operator()`的类,即仿函数。
- 按照C++标准,lambda表达式的operator()默认是const的,一个const成员函数是无法修改成员变量值的。
`mutable`选项的作用就在于取消operator()的const属性。

因为lambda表达式在C++中会被看做是一个仿函数,因此可以使用`std::function`和`std::bind来`存储和操作lambda表达式：

```c++
#include <iostream>
#include <functional>
using namespace std;

int main(void)
{
    // 包装可调用函数
    std::function<int(int)> f1 = [](int a) {return a; };
    // 绑定可调用函数
    std::function<int(int)> f2 = bind([](int a) {return a; }, placeholders::_1);

    // 函数调用
    cout << f1(100) << endl;
    cout << f2(200) << endl;
    return 0;
}
```

对于没有捕获任何变量的lambda表达式,还可以转换成一个普通的函数指针：

```c++
using func_ptr = int(*)(int);
// 没有捕获任何外部变量的匿名函数
func_ptr f = [](int a)
{
    return a;  
};
// 函数调用
f(1314);
```

### 总结

1. 在 C++ 里,每个 lambda 表达式都会**有一个独特的类型**,而这个类型只有编译器才知道,我们是无法直接写出来的,所以必须用 auto。
2. 因为 lambda 表达式毕竟不是普通的变量,所以 C++ 也鼓励程序员尽量“匿名”使用 lambda 表达式。也就是说,它不必显式赋值给一个有名字的变量,直接声明就能用,免去你费力起名的烦恼。

## 可调用对象包装器、绑定器

### 可调用对象

在C++中存在“可调用对象”这么一个概念。准确来说,可调用对象有如下几种定义：
- 是一个函数指针
	```c++
	int print(int a, double b)
	{
	    cout << a << b << endl;
	    return 0;
	}
	// 定义函数指针
	int (*func)(int, double) = &print;
	```
- 是一个具有`operator()`成员函数的类对象（仿函数）
	```c++
	using namespace std;
	
	struct Test
	{
	    // ()操作符重载
	    void operator()(string msg)
	    {
	        cout << "msg: " << msg << endl;
	    }
	};
	
	int main(void)
	{
	    Test t;
	    t("我是要成为海贼王的男人!!!");	// 仿函数
	    return 0;
	}
	```
- 是一个可被转换为函数指针的类对象
	```c++
	using namespace std;
	using func_ptr = void(*)(int, string);
	struct Test
	{
	    static void print(int a, string b)
	    {
	        cout << "name: " << b << ", age: " << a << endl;
	    }
	
	    // 将类对象转换为函数指针
	    operator func_ptr()
	    {
	        return print;
	    }
	};
	
	int main(void)
	{
	    Test t;
	    // 对象转换为函数指针, 并调用
	    t(19, "Monkey D. Luffy");
	
	    return 0;
	}
	```

- 是一个类成员函数指针或者类成员指针
```c++
using namespace std;
struct Test
{
    void print(int a, string b)
    {
        cout << "name: " << b << ", age: " << a << endl;
    }
    int m_num;
};
int main(void)
{
    // 定义类成员函数指针指向类成员函数
    void (Test::*func_ptr)(int, string) = &Test::print;
    // 类成员指针指向类成员变量
    int Test::*obj_ptr = &Test::m_num;

    Test t;
    // 通过类成员函数指针调用类成员函数
    (t.*func_ptr)(19, "Monkey D. Luffy");
    // 通过类成员指针初始化类成员变量
    t.*obj_ptr = 1;
    cout << "number is: " << t.m_num << endl;

    return 0;
}
```

在上面的例子中满足条件的这些可调用对象对应的类型被统称为**可调用类型**。C++中的可调用类型虽然具有比较统一的操作形式,但定义方式五花八门,这样在我们试图使用统一的方式保存,或者传递一个可调用对象时会十分繁琐。现在,C++11通过提供`std::function` 和 `std::bind`统一了可调用对象的各种操作。

### bind1st 和 bind2nd

bind1st和bind2nd。两者在C++11中均已不推荐使用，C++17中已经移除.
但是可以看一下这两个的底层实现原理

```c++
template<typename Iterator, typename Compare>
Iterator my_find_if(Iterator first, Iterator last, Compare comp) {
  while (first != last) {
    if (comp(*first))
      return first;
    ++first;
  }
  return last;
}

template<typename Compare, typename T>
class my_binder1st {
private:
  Compare comp;
  T value;
public:
  my_binder1st(Compare comp, T value): comp(comp), value(value) {}
  bool operator()(const T &x) {
    return comp(value, x);
  }
};
int main() {
  std::vector<int> v(10);
  for (int i = 0; i < 10; ++i) {
    v[i] = dist(e);
  }
  std::sort(v.begin(), v.end());
  auto it1 = my_find_if(v.begin(), v.end(), my_binder1st(std::less<int>(), 50));
  if (it1 != v.end()) {
    v.insert(it1, 50);
  }
  for (auto i: v) {
    std::cout << i << " ";
  }
}
```


### 可调用对象包装器function

`std::function`是可调用对象的**包装器**。它是一个**类模板**,可以容纳除了类成员（函数）指针之外的所有可调用对象。通过指定它的模板参数,它可以用统一的方式处理函数、函数对象、函数指针,并允许保存和延迟执行它们。

#### 基本用法

`std::function`必须要包含一个叫做functional的头文件,可调用对象包装器使用语法如下:
```c++
#include <functional>
std::function<返回值类型(参数类型列表)> diy_name = 可调用对象;
```

下面的实例代码中演示了可调用对象包装器的基本使用方法：
```c++
#include <iostream>
#include <functional>
using namespace std;

int add(int a, int b)
{
    cout << a << " + " << b << " = " << a + b << endl;
    return a + b;
}

class T1
{
public:
    static int sub(int a, int b)
    {
        cout << a << " - " << b << " = " << a - b << endl;
        return a - b;
    }
};

class T2
{
public:
    int operator()(int a, int b)
    {
        cout << a << " * " << b << " = " << a * b << endl;
        return a * b;
    }
};

int main(void)
{
    // 绑定一个普通函数
    function<int(int, int)> f1 = add;
    // 绑定以静态类成员函数
    function<int(int, int)> f2 = T1::sub;
    // 绑定一个仿函数
    T2 t;
    function<int(int, int)> f3 = t;

    // 函数调用
    f1(9, 3);
    f2(9, 3);
    f3(9, 3);

    return 0;
}
```

输入结果如下:
```c++
9 + 3 = 12
9 - 3 = 6
9 * 3 = 27
```

通过测试代码可以得到结论：std::function可以将可调用对象**进行包装**,得到一个统一的格式,包装完成得到的对象相当于一个**函数指针**,和函数指针的使用方式相同,通过包装器对象就可以完成对包装的函数的调用了。

#### 作为回调函数使用
因为回调函数本身就是通过函数指针实现的,**使用对象包装器可以取代函数指针的作用**,来看一下下面的例子：
```c++
#include <iostream>
#include <functional>
using namespace std;

class A
{
public:
    // 构造函数参数是一个包装器对象
    A(const function<void()>& f) : callback(f)
    {
    }

    void notify()
    {
        callback(); // 调用通过构造函数得到的函数指针
    }
private:
    function<void()> callback;
};

class B
{
public:
    void operator()()
    {
        cout << "我是要成为海贼王的男人!!!" << endl;
    }
};
int main(void)
{
    B b;
    A a(b); // 仿函数通过包装器对象进行包装
    a.notify();

    return 0;
}
```

通过上面的例子可以看出,使用对象包装器`std::function`可以非常方便的将仿函数转换为一个函数指针,通过进行函数指针的传递,在其他函数的合适的位置就可以调用这个包装好的仿函数了。
另外,使用`std::function`作为函数的传入参数,可以将定义方式不相同的可调用对象进行统一的传递,这样大大增加了程序的灵活性。

#### function函数对象类型的实现原理

介绍原理之前，我们先介绍下模板的**完全特例化**和**非完全(部分)特例化**

```c++
template<typename T>
bool compare(T a,T b){
  return a < b;
}
template<>  // 完全特例化
bool compare<const char*>(const char* a,const char* b){
  return strcmp(a,b) < 0;
}
int main() {
  bool b =compare(10, 20);
  compare("aaa","bbb"); // 优先匹配完全特例化
}
```

```c++
template<typename T>
class Vector {
public:
    Vector() { std::cout << "call vector template init" << std::endl; }
};

// 下面是对char* 类型进行的完全的模板特例化
template<>
class Vector<char *> {
public:
    Vector() {
      std::cout << "call vector char* template init" << std::endl;
    }
};

template<typename Ty>
class Vector<Ty*> {
public:
    Vector() {
      std::cout << "call vector Ty* template init" << std::endl;
    }
};

// 指针函数指针(有返回值，有两个形参变量)提供的部分特例化
template<typename R,typename A1,typename A2>
class Vector<R(*)(A1,A2)>{
public:
    Vector(){
      std::cout << "call vector R(*)(A1,A2) template init" << std::endl;
    }
};

// 针对函数(有一个返回值，有两个形参变量)类型进行特例化
template<typename R,typename A1,typename A2>
class Vector<R(A1,A2)>{
public:
    Vector(){
      std::cout<<"call vector R(A1,A2) template init"<<std::endl;
    }
};

int sum(int a,int b) {return a+b;}

int main() {

  Vector<int> v1;
  Vector<char *> v2;
  Vector<int*>v3;
  Vector<int(*)(int,int)>v4;

  // 注意区分一下函数类型和函数指针类型
  typedef int(*Func1)(int,int);
  Func1 f=sum;
  std::cout<<f(10,20)<<std::endl;

  typedef int Func2(int,int);
  Func2* f2=sum;
  std::cout<<f2(10,20)<<std::endl;

  return EXIT_SUCCESS;
}
```


看完上面的例子,我们实现function函数对象类型的实现原理
```c++
void hello(std::string str) {
  std::cout << str << std::endl;
}

int sum(int a, int b, int c) {

  return a + b + c;
}

template<typename Ty>
class myfunction {
};


template<typename R, typename ...args>
class myfunction<R(args...)> {
private:
    using mfunc = R(*)(args...);
    mfunc _func;
public:
    myfunction(mfunc func1) : _func(func1) {}

    R operator()(args...a) {
      return _func(a...);
    }
};


int main() {
  myfunction<void(std::string)> f1(hello);
  f1("hello world");
  myfunction<int(int, int, int)> f2 = sum;
  std::cout << f2(1, 2, 3);
  return EXIT_SUCCESS;
}
```

### 绑定器
`std::bind`用来将**可调用对象与其参数**一起进行绑定。绑定后的结果可以使用`std::function`进行保存,并延迟调用到任何我们需要的时候。通俗来讲,它主要有两大作用：

1. 将可调用对象与其参数一起绑定成一个仿函数。
2. 将多元（参数个数为n,n>1）可调用对象转换为一元或者（n-1）元可调用对象,即只绑定部分参数。
绑定器函数使用语法格式如下：
```c++
// 绑定非类成员函数/变量
auto f = std::bind(可调用对象地址, 绑定的参数/占位符);
// 绑定类成员函/变量
auto f = std::bind(类函数/成员地址, 类实例对象地址, 绑定的参数/占位符);
```

下面来看一个关于绑定器的实际使用的例子：
```c++
#include <iostream>
#include <functional>
using namespace std;

void callFunc(int x, const function<void(int)>& f)
{
    if (x % 2 == 0)
    {
        f(x);
    }
}

void output(int x)
{
    cout << x << " ";
}

void output_add(int x)
{
    cout << x + 10 << " ";
}

int main(void)
{
    // 使用绑定器绑定可调用对象和参数
    auto f1 = bind(output, placeholders::_1);
    for (int i = 0; i < 10; ++i)
    {
        callFunc(i, f1);
    }
    cout << endl;

    auto f2 = bind(output_add, placeholders::_1);
    for (int i = 0; i < 10; ++i)
    {
        callFunc(i, f2);
    }
    cout << endl;

    return 0;
}
```

测试代码输出的结果:
```c++
0 2 4 6 8
10 12 14 16 18
```

在上面的程序中,使用了`std::bind`绑定器,在函数外部通过绑定不同的函数,控制了最后执行的结果。`std::bind`绑定器返回的是一个仿函数类型,得到的返回值可以直接赋值给一个`std::function`,在使用的时候我们并不需要关心绑定器的返回值类型,使用auto进行自动类型推导就可以了。

`placeholders::_1`是一个占位符,代表这个位置将在函数调用时被传入的第一个参数所替代。同样还有其他的占位符`placeholders::_2、placeholders::_3、placeholders::_4、placeholders::_5等……`

有了占位符的概念之后,使得std::bind的使用变得非常灵活:
```c++
#include <iostream>
#include <functional>
using namespace std;

void output(int x, int y)
{
    cout << x << " " << y << endl;
}

int main(void)
{
    // 使用绑定器绑定可调用对象和参数, 并调用得到的仿函数
    bind(output, 1, 2)();
    bind(output, placeholders::_1, 2)(10);
    bind(output, 2, placeholders::_1)(10);

    // error, 调用时没有第二个参数
    // bind(output, 2, placeholders::_2)(10);
    // 调用时第一个参数10被吞掉了,没有被使用
    bind(output, 2, placeholders::_2)(10, 20);

    bind(output, placeholders::_1, placeholders::_2)(10, 20);
    bind(output, placeholders::_2, placeholders::_1)(10, 20);


    return 0;
}
```

示例代码执行的结果:
```c++
1  2		// bind(output, 1, 2)();
10 2		// bind(output, placeholders::_1, 2)(10);
2 10		// bind(output, 2, placeholders::_1)(10);
2 20		// bind(output, 2, placeholders::_2)(10, 20);
10 20		// bind(output, placeholders::_1, placeholders::_2)(10, 20);
20 10		// bind(output, placeholders::_2, placeholders::_1)(10, 20);
```

通过测试可以看到,`std::bind`可以直接绑定函数的所有参数,也可以仅绑定部分参数。在绑定部分参数的时候,通过使用std::placeholders来决定空位参数将会属于调用发生时的第几个参数。

可调用对象包装器`std::function`是不能实现对类成员函数指针或者类成员指针的包装的,但是通过绑定器std::bind的配合之后,就可以完美的解决这个问题了,再来看一个例子,然后再解释里边的细节：

```c++
#include <iostream>
#include <functional>
using namespace std;

class Test
{
public:
    void output(int x, int y)
    {
        cout << "x: " << x << ", y: " << y << endl;
    }
    int m_number = 100;
};

int main(void)
{
    Test t;
    // 绑定类成员函数
    function<void(int, int)> f1 = 
        bind(&Test::output, &t, placeholders::_1, placeholders::_2);
    // 绑定类成员变量(公共)
    function<int&(void)> f2 = bind(&Test::m_number, &t);

    // 调用
    f1(520, 1314);
    f2() = 2333;
    cout << "t.m_number: " << t.m_number << endl;

    return 0;
}
```

示例代码输出的结果:
```c++
x: 520, y: 1314
t.m_number: 2333
```

在用绑定器绑定类成员函数或者成员变量的时候需要将它们所属的实例对象一并传递到绑定器函数内部。f1的类型是`function<void(int, int)>`,通过使用`std::bind`将Test的成员函数output的地址和对象t绑定,并转化为一个仿函数并存储到对象f1中。

使用绑定器绑定的类成员变量m_number得到的仿函数被存储到了类型为`function<int&(void)>`的包装器对象f2中,并且可以在需要的时候修改这个成员。其中int是绑定的类成员的类型,并且允许修改绑定的变量,因此需要指定为变量的引用,由于没有参数因此参数列表指定为void。

示例程序中是使用function包装器保存了bind返回的仿函数,如果不知道包装器的模板类型如何指定,可以直接使用auto进行类型的自动推导,这样使用起来会更容易一些。


**我们可以使用绑定器指定线程创建的调用类中的方法**

```c++
// 线程类
// 1. 我们在这个线程池的内部进行调用线程的方法
class Thread{
private:
    using Func= std::function<void(int)>;
    Func _func;
public:
    Thread(Func func):_func(func){}
    std::thread start(int id){
      std::thread t(_func,id);
      return t;
    }
};

// 线程池类
class ThreadPool{
private:
    std::vector<Thread*>_pool;
    std::vector<std::thread>_handler;
    void runInThread(int id){
      std::lock_guard<std::mutex>lock(g_mutex);
      std::cout<<"Thread call id is "<<id<<std::endl;
    }
public:
    ThreadPool(){}
    ~ThreadPool(){ for (auto &item: _pool){
        delete item;
    } }
    // 1. 创建线程 调用线程的start方法
    void createPool(int size){
      for (int i = 0; i < size; ++i) {
        _pool.push_back(new Thread(std::bind(&ThreadPool::runInThread,this,std::placeholders::_1))); // 这里调用了bind来加载类内函数对象作为线程的初始方法，类的对象就是this
      }
      for (int i = 0; i < size; ++i) {
        _handler.push_back(_pool[i]->start(i+1));
      }
      for (auto &t: _handler){
        t.join();
      }
    }
};
```

#### bind的实现原理

```c++
// 简化版本的 bind，仅用于演示
template<typename Func,typename ...Args>
class SimpleBind{
private:
    Func func;
    std::tuple<Args...>args;
public:
    SimpleBind(Func func,Args...args):func(func),args(std::make_tuple(args...)){}
    template<size_t ...indexs>
    auto callFunc(std::index_sequence<indexs...>) -> decltype(func(std::get<indexs>(args)...)) {
      return func(std::get<indexs>(args)...);
    }

    auto operator()() -> decltype(func(std::get<Args>(args)...)){
      return callFunc(std::index_sequence_for<Args...>{});
    }
};
void hello(std::string str) {
  std::cout << str << std::endl;
}

// 一个工厂函数来创建 SimpleBind 对象
template<typename Func, typename... Args>
auto make_simple_bind(Func func, Args... args) -> std::function<decltype(func(args...))()> {
  auto bound_func = SimpleBind<Func, Args...>(func, args...);
  return [bound_func]() mutable -> decltype(func(args...)) {
      return bound_func();
  };
}

int main(){

  std::function<void()> func=make_simple_bind(hello,"hello world");
  func();
  return EXIT_SUCCESS;

}
```


> [!summary] 代码实现的精华
> 1. 我们用[[C++#tuple类型|tuple]]来包装可变参数
> 2. 利用[[C++#index_sequence|index_sequence]]去生成整数序列，利用这个整数序列去调用tuple里面的每个变量值
> 3. 利用工厂函数简化了创建对象的过程，省略了手动指定所有模板参数只需传递所需的函数和参数即可
> 4. 利用[[C++#decltype|decltype]]推断的[[C++#返回值后置|返回值后置]]和返回[[C++#Lambda表达式|Lambda]]保证返回值可以被[[C++#可调用对象包装器function|function]]接受
> 


### std::invoke

`std::invoke `是 C++17 引入的一个实用函数模板，用于**统一的方式调用不同类型的可调用对象**。它可以处理普通函数、函数指针、lambda 表达式、成员函数以及具有 `operator()` 的对象（如函数对象）。std::invoke 的灵活性使它成为模板编程和泛型编程中一个非常有用的工具。

基本语法如下：
```c++
std::invoke(callable, args...)
```

- `callable`：一个可调用对象，比如函数指针、成员函数指针、lambda 表达式或任何实现了 `operator()` 的对象。
- `args...`：传递给 `callable` 的参数列表。

**功能和优点**
- 泛型调用：能够处理几乎所有类型的可调用对象，这使得它在模板编程中尤为有用。
- 成员函数和对象方法：能够调用类的成员函数。如果 callable 是成员函数指针，args... 的第一个参数应该是一个指向类实例的指针或引用。
- 自动类型推导：结合 auto 关键字和 decltype，可以用来推导函数的返回类型。

下面是 `std::invoke` 的一些使用示例：

```c++
#include <iostream>
#include <functional>

struct MyClass {
    int myFunction(int x) { return x * x; }
};

int main() {
    // 使用 lambda 表达式
    auto lambda = [](int a, int b) { return a + b; };
    std::cout << std::invoke(lambda, 5, 3) << std::endl;  // 输出 8

    // 使用成员函数
    MyClass obj;
    auto memberFunc = &MyClass::myFunction;
    std::cout << std::invoke(memberFunc, obj, 4) << std::endl;  // 输出 16

    // 使用函数对象
    struct FunctionObject {
        int operator()(int x) { return x * 10; }
    };
    FunctionObject funObj;
    std::cout << std::invoke(funObj, 6) << std::endl;  // 输出 60

    return 0;
}
```

### std::decay_t

`std::decay_t` 是 C++14 中引入的一个类型转换工具，位于 `<type_traits>` 头文件中。它用于在某些上下文中获取一个类型的衰减（decay）类型，即移除引用、数组和函数类型，得到**原始类型**。

```c++
#include <iostream>
#include <type_traits>

template <typename T>
void printType() {
    // 使用 std::decay_t 获取衰减后的类型
    using DecayType = std::decay_t<T>;
    std::cout << "Original type: " << typeid(T).name() << std::endl;
    std::cout << "Decayed type: " << typeid(DecayType).name() << std::endl;
}

int main() {
    printType<int>();            // 原始类型是 int，衰减后的类型也是 int
    printType<int&>();           // 原始类型是 int&，衰减后的类型是 int
    printType<const int[]>();    // 原始类型是 const int[]，衰减后的类型是 const int*
    printType<void(int)>();      // 原始类型是 void(int)，衰减后的类型是 void(*)(int)
    
    return 0;
}
```

**主要特点**：
1. 移除引用： 如果输入类型是引用，`std::decay_t` 会移除引用，返回基础类型。
2. 数组转指针： 如果输入类型是数组，`std::decay_t` 会将数组转换为指向数组元素类型的指针。
3. 函数转函数指针： 如果输入类型是函数类型，`std::decay_t` 会将其转换为相应的函数指针类型。

**使用场景**：
- 在模板编程中，用于获取模板参数的衰减类型，以简化代码。
- 在元函数（metafunction）中，用于执行类型转换，以便进行更一般性的类型处理。
- 使用 `std::decay_t` 可以将这些引用折叠掉，从而获取底层的类型。这样可以确保存储的函数对象类型与传入的函数对象类型一致，无论传入的是左值还是右值。

```c++
template <typename T>
struct Example {
    using DecayType = std::decay_t<T>;

    DecayType process(T value) {
        // 在这里可以使用 DecayType 进行处理
        return value;
    }
};
```

总的来说，`std::decay_t` 是一个非常方便的工具，可以在编写泛型代码时帮助处理模板参数的类型，使代码更具通用性和可维护性。

### std::invoke_result

在C++中，有时我们需要获取函数或可调用对象的**返回值类型**，以便进行后续的操作，在泛型编程中很常用，特别是当不同的参数集的结果类型不同时。在早期的C++版本中，我们需要手动推导函数返回值类型，这个过程非常复杂，也容易出错。为了解决这个问题，C++11引入了`std::result_of`和`std::result_of_t`（C++14），这两个模板可以方便地获取函数或可调用对象的返回值类型。而在C++17中，废弃了std::result_of而引入了更好用的`std::invoke_result`和`std::invoke_result_t`。

#### std::result_of/std::result_of_t

std::result_of是一个函数类型萃取器（function type traits），它可以推导函数类型的返回值类型，它定义在头文件<type_traits>中。std::result_of模板类需要两个模板参数：第一个是函数类型，第二个是函数的参数类型。它的定义如下：

```c++
template <typename F, typename... Args>
class result_of<F(Args...)>;
```

在模板参数中，`F`**必须是可调用类型、对函数的引用或对可调用类型的引用**，`Args`代表**函数参数类型**。例如，如果我们有一个函数`add`，它的类型为`int(int, double)`，我们可以按照下列示例代码来使用`std::result_of`以获取其返回值类型：

```c++
#include <type_traits>
#include <functional>

int add(int x, double y)
{
    return x + static_cast<int>(y);
}

int main()
{
    std::result_of<std::function<int(int, double)>(int, double)>::type result = 0;
    static_assert(std::is_same<decltype(result), int>::value, "result type should be int");
    return 0;
}
```

在这个示例中，我们使用`std::result_of<std::function<int(int, double)>(int, double)>::type`来获取返回值类型，并使用`std::is_same`来检查返回值类型是否为int。这里，因为我们知道函数的入口（或理解为函数指针），所以`std::result_of<std::function<int(int, double)>(int, double)>::type`也可以改成`std::result_of<decltype(&add)(int, double)>::type`（注意这里用的是&add，而不是add，F不能识别函数类型，但可以是函数指针的类型）。而代码中的result是一个变量，而不是类型，所以不能直接在`std::is_same`中引用，即不能写成`std::is_same<result, int>`，必须使用decltype获取result的类型。

C++14引入了一个方便的类型别名`std::result_of_t`，它可以替代`std::result_of<F(Args...)>::type`，简化代码。使用`std::result_of_t`，上面的示例可以写成：
```c++
#include <type_traits>
int add(int x, double y)
{
    return x + static_cast<int>(y);
}
int main()
{
    std::result_of_t<decltype(&add)(int, double))> result = 0;
    static_assert(std::is_same<decltype(result), int>::value, "result type should be int");
    return 0;
}
```

#### std::invoke_result/std::invoke_result_t

在C++17中，`std::result_of`已被弃用，建议使用`std::invoke_result`来代替。`std::invoke_result`可以获取函数、成员函数和可调用对象的返回值类型。与std::result_of不同的是，`std::invoke_result`支持成员函数指针和指向成员函数的指针，以及可调用对象的包装器std::function。

`std::invoke_result/std::invoke_result_t`的定义如下：
```c++
template <typename F, typename... Args>
struct invoke_result;
template <typename F, typename... Args>
using invoke_result_t = typename invoke_result<F, Args...>::type;
```

在模板参数中，F代表函数类型、成员函数指针类型或可调用对象类型，Args代表函数或成员函数的参数类型。例如，如果我们有一个函数add，它的类型为`int(int, double)`，我们可以参照如下示例代码使用`std::invoke_result_t`来获取它的返回值类型：

```c++
#include <type_traits>

int add(int x, double y)
{
    return x + static_cast<int>(y);
}

int main()
{
    std::invoke_result_t<decltype(add), int, double> result = 0;
    static_assert(std::is_same<decltype(result), int>::value, "result type should be int");
    return 0;
}
```

在这个示例中，我们使用`std::invoke_result_t<decltype(add), int, double>`来获取函数add的返回值类型，并使用`std::is_same`来检查返回值类型是否为int。

如果我们有一个类A和一个成员函数`A::add()`，则可以使用下列代码获取成员函数的返回值：

```c++
#include <type_traits>

class A
{
public:
    int add(int x, double y)
    {
        return x + static_cast<int>(y);
    }
};

int main()
{
    std::invoke_result_t<decltype(&A::add), A*, int, double> result = 0;
    static_assert(std::is_same<decltype(result), int>::value, "result type should be int");
    return 0;
}
```

在这个示例中，我们使用`std::invoke_result_t<decltype(&A::add), A*, int, double>`来获取成员函数`A::add`的返回值类型。需要注意的是，由于成员函数A::add()是属于类的，而成员函数必须通过类的对象或指针进行调用，因此需要将函数的第一个参数类型指定为该函数所在类的类型的指针类型，即"A*"。（编译器为我们隐藏了调用成员函数时所需要的this指针参数，所以这里会显得有点绕）。剩下类型就是该成员函数的其他参数类型。

如果我们有一个可调用对象，我们可以直接将它传递给`std::invoke_result_t`。例如：

```c++
#include <type_traits>
#include <functional>
int main()
{
    std::function<int(int, double)> add = [](int x, double y) {
        return x + static_cast<int>(y);
    };
    std::invoke_result_t<decltype(add), int, double> result = 0;
    static_assert(std::is_same<decltype(result), int>::value, "result type should be int");
    return 0;
}
```

### 类型擦除

类型擦除是一种编程技术，在 C++ 中，它通常用于将具有不同类型但共享相同接口的对象统一为一个共同的类型。这样做的主要目的是为了提供一个类型无关的接口，同时在内部保持类型的多态性。

类型擦除（type erasure）是一种编程技术，它允许在编译时隐藏或“擦除”类型信息，从而在运行时以一种统一的方式处理不同类型的对象。这在实现多态和通用性时非常有用。类型擦除的一个经典例子就是标准库中的`std::function`和`std::any`。

#### 类型擦除的概念

类型擦除的主要目的是将不同类型的对象包装成一个公共的接口，以便可以通过统一的接口进行操作，而无需知道对象的实际类型。类型擦除通常通过以下几个步骤实现：

1. **定义一个抽象基类**：提供一个公共的接口，包含需要的操作。
2. **定义一个模板派生类**：该类封装实际对象，实现基类的接口。
3. **使用基类指针操作派生类对象**：通过多态性，实现对不同类型对象的统一操作。

#### 示例代码

以下是一个简化的示例，展示了如何通过类型擦除实现类似`std::function`的功能：

```c++
#include <iostream>
#include <memory>
#include <functional>

// 抽象基类，定义了一个统一的接口
class CallableBase {
public:
    virtual void call(int) = 0;
    virtual ~CallableBase() = default;
};

// 模板派生类，封装实际的可调用对象
template<typename T>
class CallableWrapper : public CallableBase {
    T callable;
public:
    CallableWrapper(T callable) : callable(callable) {}
    void call(int x) override {
        callable(x);
    }
};

// 自定义的 Function 类，使用类型擦除实现多态
class Function {
    std::unique_ptr<CallableBase> callable;
public:
    template<typename T>
    Function(T callable) : callable(new CallableWrapper<T>(callable)) {}
    void operator()(int x) {
        callable->call(x);
    }
};

int main() {

    Function func1 = [](int x) { std::cout << "Lambda called with " << x << std::endl; };
    func1(10);

    Function func2 = [](int x) { std::cout << "Another lambda called with " << x << std::endl; };
    func2(20);

    return 0;
}
```

std::any的类型擦除
std::any是C++17引入的一个类型安全的容器，它可以存储任意类型的值。std::any通过类型擦除实现了对不同类型对象的统一存储和操作。

示例代码
以下是一个简化的示例，展示了如何通过类型擦除实现类似std::any的功能：

```c++
#include <iostream>
#include <memory>
#include <typeinfo>
// 抽象基类，定义了一个统一的接口
class AnyBase {
public:
    virtual ~AnyBase() = default;
    virtual const std::type_info& type() const = 0;
};
// 模板派生类，封装实际的对象
template<typename T>
class AnyWrapper : public AnyBase {
    T value;
public:
    AnyWrapper(T value) : value(value) {}
    const std::type_info& type() const override {
        return typeid(T);
    }
    T& get() {
        return value;
    }
};
// 自定义的 Any 类，使用类型擦除实现多态
class Any {
    std::unique_ptr<AnyBase> content;
public:
    template<typename T>
    Any(T value) : content(new AnyWrapper<T>(value)) {}

    template<typename T>
    T& get() {
        auto derived = dynamic_cast<AnyWrapper<T>*>(content.get());
        if (derived) {
            return derived->get();
        } else {
            throw std::bad_cast();
        }
    }

    const std::type_info& type() const {
        return content->type();
    }
};
int main() {
    Any a = 10;
    std::cout << "Stored type: " << a.type().name() << ", value: " << a.get<int>() << std::endl;

    a = std::string("Hello");
    std::cout << "Stored type: " << a.type().name() << ", value: " << a.get<std::string>() << std::endl;
    return 0;
}
```


## 强类型枚举

### 枚举
#### 枚举的使用
枚举类型是C及C++中一个基本的内置类型,不过也是一个有点”奇怪”的类型。从枚举的本意上来讲,就是要定义一个类别,并穷举同一类别下的个体以供代码中使用。由于枚举来源于C,所以出于设计上的简单的目的,枚举值常常是对应到整型数值的一些名字,比如：
```c++
// 匿名枚举
enum {Red, Green, Blue};
// 有名枚举
enum Colors{Red, Green, Blue};
```

在枚举类型中的枚举值编译器会默认从0开始赋值,而后依次向下递增,也就是说Red=0,Green=1,Blue=2。
#### 枚举的缺陷
C/C++的enum有个很”奇怪” 的设定,就是具名（有名字）的enum类型的名字,以及 enum 的成员的名字都是<font color=#ff0000>全局可见</font>的。这与 C++中具名的 `namespace、class/struct 及 union` 必须通过名字::成员名的方式访问相比是格格不入的,编码过程中一不小心程序员就容易遇到问题。比如∶
```c++
enum China {Shanghai, Dongjing, Beijing, Nanjing};
enum Japan {Dongjing, Daban, Hengbin, Fudao};
```

上面定义的两个枚举在编译的时候,编译器会报错,具体信息如下：
```c++
error C2365: “Dongjing”: 重定义；以前的定义是“枚举数”
```

错误的原因上面也提到了,在这两个具名的枚举中Dongjing是全局可见的,所有编译器就会提示其重定义了。

另外,由于C中枚举被设计为常量数值的”别名”的本性,所以枚举的成员总是可以被隐式地转换为整型,但是很多时候我们并不想这样。

### 强类型枚举
#### 优势
针对枚举的缺陷,C++11标准引入了一种新的枚举类型,即枚举类,又称强类型枚举（strong-typed enum）。 声明强类型枚举非常简单,只需要在 enum 后加上关键字 class。比如∶

```c++
// 定义强类型枚举
enum class Colors{Red, Green, Blue};
```

强类型枚举具有以下几点优势∶ 
- 强作用域,强类型枚举成员的名称不会被输出到其父作用域空间。
	- 强类型枚举只能是有名枚举,如果是匿名枚举会导致枚举值无法使用（因为没有作用域名称）。
- 转换限制,强类型枚举成员的值不可以与整型隐式地相互转换。 
- 可以指定底层类型。强类型枚举默认的底层类型为 int,但也可以显式地指定底层类型, 具体方法为在枚举名称后面加上`∶type`,其中 **type 可以是除 wchar_t 以外的任何整型**。比如:

```c++
enum class Colors :char { Red, Green, Blue };
```

> wchar_t 是什么?
> 
> 双字节类型,或宽字符类型,是C/C++的一种扩展的存储方式,一般为16位或32位,所能表示的字符数远超char型。
> 主要用在国际化程序的实现中,但它不等同于 unicode 编码。unicode 编码的字符一般以wchar_t类型存储。

了解了强类型枚举的优势之后,我们再看一段程序：
```c++
enum class China { Shanghai, Dongjing, Beijing, Nanjing, };
enum class Japan:char { Dongjing, Daban, Hengbin, Fudao };
int main()
{
    int m = Shanghai;           // error
    int n = China::Shanghai;    // error
    if ((int)China::Beijing >= 2)
    {
    	cout << "ok!" << endl;
    }
    cout << "size1: " << sizeof(China::Dongjing) << endl;
    cout << "size2: " << sizeof(Japan::Dongjing) << endl;
    return 0;
}
```

- 第5行：该行的代码有两处错误
	1. 强类型枚举属于强作用于类型,不能直接使用,枚举值前必须加枚举类型
	2. 强类型枚举不会进行隐式类型转换,因此枚举值不能直接给int行变量赋值（虽然强类型枚举的枚举值默认就是整形,但其不能作为整形使用）。
- 第6行：语法错误,将强类型枚举值作为整形使用,此处不会进行隐式类型转换
- 第7行：语法正确,强类型枚举值在和整数比较之前做了强制类型转换。
- 第11行：打印的结果为4,强类型枚举底层类型值默认为int,因此占用的内存是4个字节
- 第12行：打印的结果为1,显示指定了强类型枚举值的类型为char,因此占用的内存大小为1个字节,这样我们就可以节省更多的内存空间了。

对原有枚举的扩展
相比于原来的枚举,强类型枚举更像是一个属于C++的枚举。但为了配合新的枚举类型,C++11还对原有枚举类型进行了扩展：

1. 原有枚举类型的底层类型在默认情况下,仍然由编译器来具体指定实现。但也可以跟强类型枚举类一样,显式地由程序员来指定。其指定的方式跟强类型枚举一样,都是枚举名称后面加上∶type,其中type 可以是除 wchar_t 以外的任何整型。比如∶
	```c++
	enum Colors : char { Red, Green, Blue };
	```

2. 关于作用域,在C++11中,枚举成员的名字除了会自动输出到父作用域,也可以在枚举类型定义的作用域内有效。比如：

	```c++
	enum Colors : char { Red, Green, Blue };
	int main()
	{
	    Colors c1 = Green;          // C++11以前的用法
	    Colors c2 = Colors::Green;  // C++11的扩展语法
	    return 0;
	}
	```

上面程序中第4、5行的写法都是合法的。

C++11中对原有枚举类型的这两个扩展都保留了向后兼容性,也方便了程序员在代码中同时操作两种枚举类型。此外,我们在声明强类型枚举的时候,也可以使用关键字`enum struct`。实际上 `enum struct` 和`enum class`在语法上没有任何区别（`enum class` 的成员没有公有私有之分,也不会使用模板来支持泛化的声明 ）。


## 非受限联合体

### 什么是非受限联合体
联合体又叫共用体,我将其称之为`union`,它的使用方式和结构体类似,程序猿可以在联合体内部定义多种不同类型的数据成员,但是这些数据会共享同一块内存空间（也就是如果对多个数据成员同时赋值会发生数据的覆盖）。在某些特定的场景下,通过这种特殊的数据结构我们就可以实现内存的复用,从而达到节省内存空间的目的。

在C++11之前我们使用的联合体是有局限性的,主要有以下三点：

1. 不允许联合体拥有非POD类型的成员
2. 不允许联合体拥有静态成员
3. 不允许联合体拥有引用类型的成员
在新的C++11标准中,取消了关于联合体对于数据成员类型的限定,规定**任何非引用类型都可以成为联合体的数据成员,这样的联合体称之为非受限联合体**（Unrestricted Union）

### 非受限联合体的使用
#### 静态类型的成员
对于非受限联合体来说,静态成员有两种分别是`静态成员变量`和`静态成员函数`,我们来看一下下面的代码：
```c++
union Test
{
    int age;
    long id;
    // int& tmp = age; // error
    static char c;
    static int print()
    {
        cout << "c value: " << c << endl;
        return 0;
    }
};
char Test::c;
// char Test::c = 'a';

int main()
{
    Test t;
    Test t1;
    t.c = 'b';
    t1.c = 'c';
    t1.age = 666;
    cout << "t.c: " << t.c << endl;
    cout << "t1.c: " << t1.c << endl;
    cout << "t1.age: " << t1.age << endl;
    cout << "t1.id: " << t1.id << endl;
    t.print();
    Test::print();
    return 0;
}
```

执行程序输出的结果如下:
```c++
t.c: c
t1.c: c
t1.age: 666
t1.id: 666
c value: c
c value: c
```

接下来我们逐一分析一下上面的代码:

- 第5行：语法错误,非受限联合体中不允许出现引用类型
- 第6行：非受限联合体中的静态成员变量
	1. 需要在非受限联合体外部声明（第13行）或者初始化（第14行）之后才能使用
	2. 通过打印的结果可以发现18、19行的t和t1对象共享这个静态成员变量（和类 class/struct 中的静态成员变量的使用是一样的）。
- 第7行：非受限联合体中的静态成员函数
	1. 在静态函数print()只能访问非受限联合体Test中的静态变量,对于非静态成员变量（age、id）是无法访问的。
	2. 调用这个静态方法可以通过对象（第27行）也可以通过类名（第28行）实现。
- 第24、25、26行：通过打印的结果可以得出结论在非受限联合体中静态成员变量和非静态成员变量使用的不是同一块内存。

#### 非POD类型成员

在 C++11标准中会默认删除一些非受限联合体的默认函数。比如,非受限联合体有一个非POD的成员,而该非POD成员类型拥有 [[C++#非POD类型成员|非平凡的构造函数]],那么非受限联合体的默认构造函数将被编译器删除。其他的特殊成员函数,例如默认拷贝构造函数、拷贝赋值操作符以及析构函数等,也将遵从此规则。下面来举例说明：

```c++
union Student
{
    int id;
    string name;
};

int main()
{
    Student s;
    return 0;
}
```

编译程序会看到如下的错误提示:
```c++
warning C4624: “Student”: 已将析构函数隐式定义为“已删除”
error C2280: “Student::Student(void)”: 尝试引用已删除的函数
```

上面代码中的非受限联合体`Student`中拥有一个非PDO类型的成员`string name`,`string` 类中有非平凡构造函数,因此`Student`的构造函数被删除（通过警告信息可以得知它的析构函数也被删除了）导致对象无法被成功创建出来。解决这个问题的办法就是由程序猿自己为非受限联合体定义构造函数,在定义构造函数的时候我们需要用到定位放置 new操作。

##### placement new
一般情况下,使用new申请空间时,是从系统的堆（heap）中分配空间,申请所得的空间的位置是根据当时的内存的实际使用情况决定的。但是,在某些特殊情况下,可能需要在已分配的特定内存创建对象,这种操作就叫做placement new即定位放置 new。
定位放置new操作的语法形式不同于普通的new操作：
- 使用new申请内存空间：`Base* ptr = new Base`;
- 使用定位放置new申请内存空间：

	```c++
	ClassName* ptr = new (定位的内存地址)ClassName;
	```

我们来看下面的示例程序:
```c++
#include <iostream>
using namespace std;

class Base
{
public:
    Base() {}
    ~Base() {}
    void print()
    {
        cout << "number value: " << number << endl;
    }
private:
    int number;
};

int main()
{
    int n = 100;
    Base* b = new (&n)Base;
    b->print();
    return 0;
}
```

程序运行输出的结果为:
```c++
number value: 100
```

在程序的第20行,使用定位放置的方式为指针b申请了一块内存,也就是说此时指针 b指向的内存地址和变量 n对应的内存地址是同一块（栈内存）,而在Base类中成员变量 number的起始地址和Base对象的起始地址是相同的,所以打印出 number 的值为100也就是整形变量 n 的值。

最后,给大家总结一下关于placement new的一些细节：

1. 使用定位放置new操作,既可以在栈(stack)上生成对象,也可以在堆（heap）上生成对象,这取决于定位时指定的内存地址是在堆还是在栈上。
2. 从表面上看,定位放置new操作是申请空间,其本质是利用已经申请好的空间,真正的申请空间的工作是在此之前完成的。
3. 使用定位放置new 创建对象时会自动调用对应类的构造函数,但是由于对象的空间不会自动释放,如果需要释放堆内存必须显示调用类的析构函数。
4. 使用定位放置new操作,我们可以反复动态申请到同一块堆内存,这样可以避免内存的重复创建销毁,从而提高程序的执行效率（比如网络通信中数据的接收和发送）。

##### 自定义非受限联合体构造函数
掌握了placement new的使用,我们通过一段程序来演示一下如果在非受限联合体中自定义构造函数：
```c++
class Base
{
public:
    void setText(string str)
    {
        notes = str;
    }
    void print()
    {
        cout << "Base notes: " << notes << endl;
    }
private:
    string notes;
};

union Student
{
    Student()
    {
        new (&name)string;
    }
    ~Student() {}

    int id;
    Base tmp;
    string name;
};

int main()
{
    Student s;
    s.name = "蒙奇·D·路飞";
    s.tmp.setText("我是要成为海贼王的男人!");
    s.tmp.print();
    cout << "Student name: " << s.name << endl;
    return 0;
}
```

程序打印的结果如下：
```c++
Base notes: 我是要成为海贼王的男人!
Student name: 我是要成为海贼王的男人!
```

我们在上面的程序里边给非受限制联合体显示的指定了构造函数和析构函数,在程序的第31行需要创建一个非受限联合体对象,这时便调用了联合体内部的构造函数,在构造函数的第20行通过定位放置 new的方式将构造出的对象地址定位到了联合体的成员string name的地址上了,这样联合体内部其他非静态成员也就可以访问这块地址了（通过输出的结果可以看到对联合体内的tmp对象赋值,会覆盖name对象中的数据）。

##### 匿名的非受限联合体
一般情况下我们使用的非受限联合体都是具名的（有名字）,但是我们也可以定义匿名的非受限联合体,一个比较实用的场景就是配合着类的定义使用。我们来设定一个场景：
```c++
木叶村要进行第99次人口普查,人员的登记方式如下：
    - 学生只需要登记所在学校的编号
    - 本村学生以外的人员需要登记其身份证号码
    - 本村外来人员需要登记户口所在地+联系方式
```

```c++
// 外来人口信息
struct Foreigner
{
    Foreigner(string s, string ph) : addr(s), phone(ph) {}
    string addr;
    string phone;
};

// 登记人口信息
class Person
{
public:
    enum class Category : char {Student, Local, Foreign};
    Person(int num) : number(num), type(Category::Student) {}
    Person(string id) : idNum(id), type(Category::Local) {}
    Person(string addr, string phone) : foreign(addr, phone), type(Category::Foreign) {}
    ~Person() {}

    void print()
    {
        cout << "Person category: " << (int)type << endl;
        switch (type)
        {
        case Category::Student:
            cout << "Student school number: " << number << endl;
            break;
        case Category::Local:
            cout << "Local people ID number: " << idNum << endl;
            break;
        case Category::Foreign:
            cout << "Foreigner address: " << foreign.addr
                << ", phone: " << foreign.phone << endl;
            break;
        default:
            break;
        }
    }

private:
    Category type;
    union
    {
        int number;
        string idNum;
        Foreigner foreign;
    };
};

int main()
{
    Person p1(9527);
    Person p2("1101122022X");
    Person p3("砂隐村村北", "1301810001");
    p1.print();
    p2.print();
    p3.print();
    return 0;
}
```

程序输出的结果：
```c++
Person category: 0
Student school number: 9527
Person category: 1
Local people ID number: 1101122022X
Person category: 2
Foreigner address: 砂隐村村北, phone: 1301810001
```

根据需求我们将木叶村的人口分为了三类并通过枚举记录了下来,在`Person类`中添加了一个匿名的非受限联合体用来存储人口信息,仔细分析之后就会发现这种处理方式的优势非常明显：`尽可能地节省了内存空间`。
- Person类可以直接访问匿名非受限联合体内部的数据成员。
- 不使用匿名非受限联合体申请的内存空间等于 number、 idNum 、 foreign 三者内存之和。
- 使用匿名非受限联合体之后number、 idNum 、 foreign 三者共用同一块内存。


# C++17

## std::string_view 

C++17中我们可以使用std::string_view来获取一个字符串的**视图**，字符串视图并不真正的创建或者拷贝字符串，而只是拥有一个字符串的**查看功能**。

> 你可以把原始的字符串当作一条马路，而我们是在马路边的一个房子里，我们只能通过房间的窗户来观察外面的马路。这个房子就是std::string_view，你只能看到马路上的车和行人，但是你无法去修改他们，可以理解你对这个马路是只读的。正是这样std::string_view比std::string会快上很多。


下面是一些 `std::string_view` 的常见用法：

1. 创建 `std::string_view` 对象：
   ```cpp
   std::string_view strView("Hello, World!");
   std::string_view strView1("Hello, World!"，5);
   std::string_view subStrView = strView.substr(0, 5);
   ```

2. 获取字符串的长度：
   ```cpp
   std::string_view strView("Hello");
   std::size_t length = strView.length(); // 或者使用 size() 方法
   ```

3. 比较字符串：
   ```cpp
   std::string_view str1("Hello");
   std::string_view str2("World");

   if (str1 == str2) {
       // 字符串相等
   } else if (str1 < str2) {
       // str1 小于 str2
   } else {
       // str1 大于 str2
   }
   ```

4. 提取子字符串：
   ```cpp
   std::string_view strView("Hello, World!");

   std::string_view subStrView1 = strView.substr(0, 5); // "Hello"
   std::string_view subStrView2 = strView.substr(7);    // "World!"
   ```

5. 查找子字符串：
   ```cpp
   std::string_view strView("Hello, World!");

   std::size_t index = strView.find("World"); // 返回子字符串的起始位置，如果未找到则返回 std::string_view::npos
   ```

6. 转换为 `std::string`：
   ```cpp
   std::string_view strView("Hello");
   std::string str(strView); // 将字符串视图转换为字符串
   ```

7. data(): 返回指向字符串数据的指针。
```c++
	const char* dataPtr = strView.data();
```

请注意，`std::string_view` 并不会自动管理内存分配，它只是一个字符序列的非拥有引用。因此，在使用时要确保底层字符串的生命周期长于 `std::string_view` 对象。

因为`std::string_view`是原始字符串的视图，如果在查看`std::string_view`的同时修改了字符串，或者字符串被消毁，那么将是**未定义的行为**。
```c++
std::string_view GetStringView()
{
    std::string name = "xunwu";
 
    return std::string_view(name);  //离开作用域时，name已经被回收销毁 
}
```



[[C++#fold expression（折叠表达式）|折叠表达式]]

## std::void_t

在`<type_traits>`头文件里有void_t的定义

```c++
template <class... _Types>
using void_t = void;
```

很简单，其实就是void，只不过可以传入模板参数，比如`std::void_t<int, float, double>`，但归根到底他还是void

那他有啥用，看上去像是一段无用的程序
这里就用到SFINAE(Substitution failure is not an error)的知识了，我们分三步走

先看这个问题: 下面有三种foo函数，称其为Foo0，Foo1，Foo2，请问 foo foo foo 分别匹配到了哪个函数？

```c++
struct X {
  typedef int type;
};

struct Y {
  typedef int type2;
};

template <typename T> void foo(typename T::type);    // Foo0
template <typename T> void foo(typename T::type2);   // Foo1
template <typename T> void foo(T);                   // Foo2

foo<X>(5);    // Foo0: Succeed, Foo1: Failed,  Foo2: Failed
foo<Y>(10);   // Foo0: Failed,  Foo1: Succeed, Foo2: Failed
foo<int>(15); // Foo0: Failed,  Foo1: Failed,  Foo2: Succeed
```


对于foo，因为X有type，所以直接匹配到了Foo0
对于foo，因为Y没有typ，所以匹配Foo0失败，尝试匹配下一个，Y有type2，成功匹配到了Foo1
对于foo，因为int既没有type有没有type2，所以Foo0和Foo1都匹配失败，最终匹配到了Foo2
看到了吧，虽然有的地方匹配失败了，但是编译器并不会直接报错，他会尝试去匹配其他的

- 回到`std::void_t`，通过上一点可知，我们对他的定义应该更严谨 : 如果传进来的模板参数是正常的，那`std::void_t`就是void；但是如果传进来的模板产生了错误，`std::void_t`会产生匹配错误，编译器您去找别人吧...
- 将 `SFINAE` 和 `std::void_t` 结合，我们可以得到任意类型的不同匹配分支

```c++
// primary template handles types that have no nested ::type member:
template< class, class = void >
struct has_type_member : std::false_type { };
 
// specialization recognizes types that do have a nested ::type member:
template< class T >
struct has_type_member<T, std::void_t<typename T::type>> : std::true_type { };

auto f = has_type_member<float>::value; // false
auto x = has_type_member<X>::value; // true
auto y = has_type_member<Y>::value; // false
```

对于`has_type_member<float>`，因为float没有type，所以第二个模板会匹配失败，编译器会认为现在的`has_type_member<float>`实际是`has_type_member<float, void>`(因为void是个默认参数)，进而成功匹配第一个模板
对于`has_type_member<X>`，因为X有type，第二个模板直接匹配成功，编译器会认为这是第一个模板的一个特化版本
这里`std::void_t`的作用，就是把传进来的各个类型进行一个检测，哦没错过了吧！诶不对传进来的有问题编译器你快过来看看！

实际使用
判断stream是否有operator<<

```c++
template <class T0, class T1, class = void>
struct _has_stream_bit_shl : std::false_type {};
template <class T0, class T1>
struct _has_stream_bit_shl<T0, T1, std::void_t<
	decltype(std::declval<T0>() << std::decfalse_typelval<T1>())
	>> : std::true_type {};
auto a = _has_stream_bit_shl<std::ostream&, const int&>::value; // true
auto b = _has_stream_bit_shl<std::ostream&, const X&>::value; // false
```

## std::optional<>

在编程时，我们经常会遇到可能会返回传递使用一**个确定类型对象**的场景。 也就是说，这个对象可能有一个确定类型的值也可能没有任何值。 因此，我们需要一种方法来模拟类似指针的语义：指针可以通过`nullptr`来表示没有值。 解决方法是定义该对象的同时再定义一个**附加的bool类型**的值作为标志来表示该对象是否有值。 `std::optional<>`提供了一种类型安全的方式来实现这种对象。

可选对象所需的内存等于 内含 对象的大小加上一个bool类型的大小。 因此，可选对象一般比内含对象大一个字节（可能还要加上内存对齐的空间开销）。 可选对象不需要分配堆内存，并且对齐方式和内含对象相同。

和`std::variant<>、std::any`一样，可选对象有值语义。 也就是说，拷贝操作会被实现为 深拷贝 ：将创建一个新的独立对象， 新对象在自己的内存空间内拥有原对象的标记和内含值（如果有的话）的拷贝。 拷贝一个无内含值的`std::optional<>`的开销很小， 但拷贝有内含值的`std::optional<>`的开销约等于拷贝内含值的开销。 另外，`std::optional<>`对象也支持move语义。

### 使用std::optional<>

`std::optional<>`模拟了一个可以为空的任意类型的实例。 它可以被用作成员、参数、返回值等。

```c++
#include <optional>
#include <string>
#include <iostream>

// 如果可能的话把string转换为int：
std::optional<int> asInt(const std::string& s)
{
    try {
        return std::stoi(s);
    }
    catch (...) {
        return std::nullopt;
    }
}

int main()
{
    for (auto s : {"42", "  077", "hello", "0x33"}) {
        // 尝试把s转换为int，并打印结果：
        std::optional<int> oi = asInt(s);
        if (oi) {
            std::cout << "convert '" << s << "' to int: " << *oi << "\n";
        }
        else {
            std::cout << "can't convert '" << s << "' to int\n";
        }
    }
}
```

这段程序包含了一个asInt()函数来把传入的字符串转换为整数。 然而这个操作有可能会失败，因此把返回值定义为`std::optional<>`， 这样我们可以返回 “无整数值” 而不是约定一个特殊的int值， 或者向调用者抛出异常来表示失败。
因此，当转换成功时我们用stoi()返回的int初始化返回值并返回， 否则会返回std::nullopt来表明没有int值。

对于返回的`std::optional<int>`类型的oi， 我们可以判断它是否含有值（将该对象用作布尔表达式）并通过“**解引用**”的方式访问了该可选对象的值

```c++
if (oi) {
    std::cout << "convert '" << s << "' to int: " << *oi << "\n";
}
```

注意用字符串"`0x33`"调用`asInt()`将会返回0， 因为`stoi()`不会以十六进制的方式来解析字符串。
还有一些别的方式来处理返回值，例如：
```c++
std::optional<int> oi = asInt(s);
if (oi.has_value()) {
    std::cout << "convert '" << s << "' to int: " << oi.value() << "\n";
}
```

这里使用了`has_value()`来检查是否返回了一个值， 还使用了value()来访问值。value()比`运算符*`更安全： 当没有值时它会抛出一个异常。`运算符*`应该只用于已经确定含有值的场景， 否则程序将可能有未定义的行为。

另一个使用`std::optional<>`的例子是传递可选的参数和 设置可选的数据成员：

```c++
#include <string>
#include <optional>
#include <iostream>

class Name
{
private:
    std::string first;
    std::optional<std::string> middle;
    std::string last;
public:
    Name (std::string f, std::optional<std::string> m, std::string l)
          : first{std::move(f)}, middle{std::move(m)}, last{std::move(l)} {
    }
    friend std::ostream& operator << (std::ostream& strm, const Name& n) {
        strm << n.first << ' ';
        if (n.middle) {
            strm << *n.middle << ' ';
        }
        return strm << n.last;
    }
};

int main()
{
    Name n{"Jim", std::nullopt, "Knopf"};
    std::cout << n << '\n';

    Name m{"Donald", "Ervin", "Knuth"};
    std::cout << m << '\n';
}
```

类Name代表了一个由名、可选的中间名、姓组成的姓名。 成员middle被定义为可选的，当没有中间名时可以传递一个`std::nullopt`， 这和中间名是空字符串是不同的状态。
注意和通常值语义的类型一样，最佳的定义构造函数的方式是以值传参，然后把参数的值移动到成员里。
注意`std::optional<>`改变了成员middle的值的使用方式。 直接使用`n.middle`将是一个布尔表达式，表示是否有中间名。 使用`*n.middle`可以访问当前的值（如果有值的话）。
另一个访问值的方法是使用成员函数`value_or()`，当没有值的时候可以指定一个备选值。 例如，在类Name里我们可以实现为：

```c++
std::cout << middle.value_or("");   // 打印中间名或空
```

然而，这种方式下，当没有值时名和姓之间将有两个空格而不是一个。

###  `std::optional<>`类型和操作

标准库在头文件`<optional>`中以如下方式定义了`std::optional<>`类

```c++
namespace std {
    template<typename T> class optional;
}
```

另外还定义了下面这些类型和对象：

`std::nullopt_t`类型的`std::nullopt`，作为可选对象无值时候的“值”。
从`std::exception`派生的`std::bad_optional_access`异常类， 当无值时候访问值将会抛出该异常。
可选对象还使用了`<utility>`头文件中定义的`std::in_place`对象（类型是`std::in_place_t`）来支持用多个参数初始化可选对象（见下文）。

####  `std::optional<>`的操作

表`std::optional`的操作列出了`std::optional<>`的所有操作：

|       **操作符**       |            **效果**             |
| :-----------------: | :---------------------------: |
|        构造函数         | 创建一个可选对象（可能会调用内含类型的构造函数也可能不会） |
| `make_optional<>()` |        创建一个用参数初始化的可选对象        |
|        析构函数         |           销毁一个可选对象            |
|         `=`         |            赋予一个新值             |
|     `emplace()`     |          给内含类型赋予一个新值          |
|      `reset()`      |        销毁值（使对象变为无值状态）         |
|    `has_value()`    |          返回可选对象是否含有值          |
|      转换为`bool`      |          返回可选对象是否含有值          |
|         `*`         |     访问内部的值（如果无值将会产生未定义行为）     |
|        `->`         |    访问内部值的成员（如果无值将会产生未定义行为）    |
|      `value()`      |       访问内部值（如果无值将会抛出异常）       |
|    `value_or()`     |      访问内部值（如果无值将返回参数的值）       |
|      `swap()`       |           交换两个对象的值            |
|      `hash<>`       |         计算哈希值的函数对象的类型         |

## std::variant<>


# 特殊类型

## tuple类型

- `tuple`是类似`pair`的模板,每个成员类型都可以不同,但`tuple`可以有任意数量的成员。
- 但每个确定的`tuple`类型的成员数目是固定的。
- 我们可以将`tuple`看做一个“快速而随意”的数据结构。

**tuple支持的操作**：

| 操作                                           | 解释                                                                                                     |
|:--------------------------------------------:|:------------------------------------------------------------------------------------------------------:|
| `tuple<T1, T2, ..., Tn> t;`                  | `t`是一个`tuple`,成员数为`n`,第`i`个成员的类型是`Ti`所有成员都进行值初始化。                                                      |
| `tuple<T1, T2, ..., Tn> t(v1, v2, ..., vn);` | 每个成员用对应的初始值`vi`进行初始化。此构造函数是`explicit`的。                                                                |
| `make_tuple(v1, v2, ..., vn)`                | 返回一个用给定初始值初始化的`tuple`。`tuple`的类型从初始值的类型**推断**。                                                         |
| `t1 == t2`                                   | 当两个`tuple`具有相同数量的成员且成员对应相等时,两个`tuple`相等。                                                               |
| `t1 relop t2`                                |`tuple`的关系运算使用**字典序**。两个`tuple`必须具有相同数量的成员。|
| `get<i>(t)`                                  | 返回`t`的第`i`个数据成员的引用：如果`t`是一个左值,结果是一个左值引用；否则,结果是一个右值引用。`tuple`的所有成员都是`public`的。                          |
|`tuple_size<tupleType>::value`| 一个类模板,可以通过一个`tuple`类型来初始化。它有一个名为`value`的`public constexpr static`数据成员,类型为`size_t`,表示给定`tuple`类型中成员的数量。 |
|`tuple_element<i, tupleType>::type` | 一个类模板,可以通过一个整型常量和一个`tuple`类型来初始化。它有一个名为`type`的`public`成员,表示给定`tuple`类型中指定成员的类型。                        |
定义和初始化示例：

- `tuple<size_t, size_t, size_t> threeD;`
- `tuple<size_t, size_t, size_t> threeD{1,2,3};`
- `auto item = make_tuple("0-999-78345-X", 3, 2.00)；`

访问tuple成员：
- `auto book = get<0>(item);`
- `get<2>(item) *= 0.8;`

```c++
// 定义一个 tuple 类型
using MyTuple = std::tuple<int, double, std::string>;

// 获取第一个元素的类型
using FirstElementType = std::tuple_element<0, MyTuple>::type; 
// FirstElementType 是 int

// 获取第二个元素的类型
using SecondElementType = std::tuple_element<1, MyTuple>::type; 
// SecondElementType 是 double

// 获取第三个元素的值
auto ThirdElement = std::tuple_size<MyTuple>::value; 
```

### 使用tuple返回多个值
- tuple最常见的用途是从一个函数返回多个值。


```c++
#include <iostream>
#include <tuple>

std::tuple<int, double, std::string> getValues() {
  int intValue = 10;
  double doubleValue = 3.14;
  std::string stringValue = "Hello";
  return std::make_tuple(intValue, doubleValue, stringValue);
}

int main() {
  // 调用函数获取多个值
  auto result = getValues();
  // 从 std::tuple 中提取值
  int intValue = std::get<0>(result);
  double doubleValue = std::get<1>(result);
  std::string stringValue = std::get<2>(result);
  std::cout << intValue << ", " << doubleValue << ", " << stringValue << std::endl;
  return 0;
}

```

### tuple存储可变数量的参数

tuple的特性特别适合存储可变数量的参数，当我们需要[[C++#可变参数的模板|可变参数]]类型实例对象，不能使用`Args... args` ,这是因为在类定义的过程中，我们是在编译期**确定类的内存布局和大小**，而可变参数包的数量在编译期是不可知的。我们可以用tuple的类模板进行包装,编译就知道这是个tuple类了，无需关心tuple里面具体的类型什么的

```c++
template<typename... Args>
struct MyClass;
template<typename T, typename... Args>
struct MyClass<T, Args...> {
    T value;
    std::tuple<Args...>args;
};
```

调用可变参数的每个参数可以用到tuple的`get<i>(t)`
```c++
func(std::get<indexs>(args)...)
args(std::make_tuple(args...))
```



## bitset类型

- 处理二进制位的有序集；
- `bitset`也是类模板,但尖括号中输入的是`bitset`的**长度**而不是元素类型,因为元素类型是固定的,都是一个二进制位。

初始化`bitset`的方法：

| 操作                                    | 解释                                                                                                                                                                                  |
|:-------------------------------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| `bitset<n> b;`                        | `b`有`n`位；每一位均是0.此构造函数是一个`constexpr`。                                                                                                                                                |
| `bitset<n> b(u);`                     | `b`是`unsigned long long`值`u`的低`n`位的拷贝。如果`n`大于`unsigned long long`的大小,则`b`中超出`unsigned long long`的高位被置为0。此构造函数是一个`constexpr`。                                                        |
| `bitset<n> b(s, pos, m, zero, one);`  | `b`是`string s`从位置`pos`开始`m`个字符的拷贝。`s`只能包含字符`zero`或`one`：如果`s`包含任何其他字符,构造函数会抛出`invalid_argument`异常。字符在`b`中分别保存为`zero`和`one`。`pos`默认为0,`m`默认为`string::npos`,`zero`默认为'0',`one`默认为'1'。 |
| `bitset<n> b(cp, pos, m, zero, one);` | 和上一个构造函数相同,但从`cp`指向的字符数组中拷贝字符。如果未提供`m`,则`cp`必须指向一个`C`风格字符串。如果提供了`m`,则从`cp`开始必须至少有`m`个`zero`或`one`字符。                                                                                |  

初始化案例；

- `bitset<13> bitvec1(0xbeef);`
- `bitset<32> bitvec4("1100");`

```c++
int main()
{
  bitset<3>b(11012); //二进制是0010101100000100 这里的数字有2已经可以判断不是二进制了,就按十进制了,纯01按二进制
  cout<<b<<endl; // 100

  std::bitset<3> b("1101");
  std::cout << b << std::endl;//110

  string s = "AABABA";  
  bitset<6> b(s, 0, 6, 'B', 'A'); // b: 110101

  return EXIT_SUCCESS;
}
```


`bitset`操作：

| 操作                       | 解释                                                                            |
|:------------------------:|:-----------------------------------------------------------------------------:|
| `b.any()`                | `b`中是否存在1。                                                                    |
| `b.all()`                | `b`中都是1。                                                                      |
| `b.none()`               | `b`中是否没有1。                                                                    |
| `b.count()`              | `b`中1的个数。                                                                     |
| `b.size()`               |                                                                               |
| `b.test(pos)`            | `pos`下标是否是1                                                                   |
| `b.set(pos)`             | `pos`置1                                                                       |
| `b.set()`                | 所有都置1                                                                         |
| `b.reset(pos)`           | 将位置`pos`处的位复位                                                                 |
| `b.reset()`              | 将`b`中所有位复位                                                                    |
| `b.flip(pos)`            | 将位置`pos`处的位取反                                                                 |
| `b.flip()`               | 将`b`中所有位取反                                                                    |
| `b[pos]`                 | 访问`b`中位置`pos`处的位；如果`b`是`const`的,则当该位置位时,返回`true`；否则返回`false`。                 |
| `b.to_ulong()`           | 返回一个`unsigned long`值,其位模式和`b`相同。如果`b`中位模式不能放入指定的结果类型,则抛出一个`overflow_error`异常。 |
| `b.to_ullong()`          | 类似上面,返回一个`unsigned long long`值。                                               |
| `b.to_string(zero, one)` | 返回一个`string`,表示`b`中位模式。`zero`和`one`默认为0和1。                                    |
| `os << b`                | 将`b`中二进制位打印为字符`1`或`0`,打印到流`os`。                                               |
| `is >> b`                | 从`is`读取字符存入`b`。当下一个字符不是1或0时,或是已经读入`b.size()`个位时,读取过程停止。                       |  

```c++
#include <iostream>
#include <bitset>
#include <stdexcept>

int main()
{
    std::bitset<8> b("10101010");
    std::cout << "位集: " << b << std::endl;

    try {
        unsigned long result = b.to_ulong();
        std::cout << "转换为 unsigned long: " << result << std::endl;
    }
    catch (std::overflow_error& e) {
        std::cout << "发生溢出错误: " << e.what() << std::endl;
    }

    return 0;
}

```

### bitset的移位操作

```c++
  bitset<10>priv=0x7F;
  bitset<10>P_BACKUP=(1<<6);
  bitset<10>P_ADMIN=(1<<7);

  cout<<priv<<endl;
  cout<<P_BACKUP<<endl;
  cout<<P_ADMIN<<endl;

  if ((priv&P_BACKUP)==P_BACKUP){
    cout<<"BAKUP"<<endl;
  }
  if ((priv&P_ADMIN)==P_ADMIN){
    cout<<"ADMIN"<<endl;
  }
```


## 正则表达式

[[正则表达式|正则表达式]]（reqular expression）是一种描述字符序列的方法,是一种很强大的工具。

正则表达式库组件：

| 组件                | 解释                                           |
|:-----------------:|:--------------------------------------------:|
| `regex`           | 表示一个正则表达式的类                                  |
| `regex_match`     |将一个字符序列与一个正则表达式匹配(这里是整个)|
| `regex_search`    |寻找第一个与正则表达式匹配的**子序列**|
| `regex_replace`   | 使用给定格式替换一个正则表达式                              |
| `sregex_iterator` | 迭代器适配器,调用`regex_searcg`来遍历一个`string`中所有匹配的子串 |
| `smatch`          | 容器类,保存在`string`中搜索的结果                        |
|`ssub_match`| `string`中匹配的子表达式的结果                          |

`regex_match`和`regex_search`的参数：

| 操作                 | 解释                                                                                               |
|:------------------:|:------------------------------------------------------------------------------------------------:|
| `(seq, m, r, mft)` | 在字符序列`seq`中查找`regex`对象`r`中的正则表达式。`seq`可以是一个`string`、标识范围的一对迭代器、一个指向空字符结尾的字符数组的指针。                |
| `(seq, r, mft)`    | `m`是一个`match`对象,用来保存匹配结果的相关细节。`m`和`seq`必须具有兼容的类型。`mft`是一个可选的`regex_constants::match_flag_type`值。 |  
- 这些操作会返回`bool`值,指出是否找到匹配。

### 使用正则表达式库

- regex使用的正则表达式语言是**ECMAScript**,模式`[[::alpha::]]`匹配任意字母。这里的ECMAScript是什么？
- 由于反斜线是C++中的特殊字符,在模式中每次出现`\`的地方,必须用一个额外的反斜线`\\`告知C++我们需要一个反斜线字符。现在可以用[[C++#原始字面量|原始字面量]]消除这个隐患
- 简单案例：

	```c++
	  // 拼写规则i除非在c之后, 否则必须在e之前”的单词：
	  string  pattern("[^c]ei");
	  pattern="[[:alpha:]]*"+pattern+"[[:alpha:]]*";
	  regex r(pattern);
	  smatch results; // 定义一个对象保存结果
	  string test_str="receipt freind theif receive";
	  if(regex_search(test_str,results,r))
	    cout<<results.str()<<endl;
	  return EXIT_SUCCESS;
	```

	```c++
	  sregex_iterator it(test_str.begin(),test_str.end(),r);
	  sregex_iterator end; //创建sregex_iterator对象时,将其初始化为默认构造函数即可表示遍历结束。
	  while (it!=end)
	  {
	    cout<<it->str()<<endl;
	    ++it;
	  }
	```

`regex`（和`wregex`）选项：

| 操作                             | 解释                                                                                                                                                |
|:------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------:|
| `regex r(re)` `regex r(re, f)` | `re`表示一个正则表达式,它可以是一个`string`、一对表示字符范围的迭代器、一个指向空字符结尾的字符数组的指针、一个字符指针和一个计数器、一个花括号包围的字符列表。`f`是指出对象如何处理的标志。`f`通过下面列出来的值来设置。如果未指定`f`,其默认值为`ECMAScript`。 |
| `r1 = re`                      | 将`r1`中的正则表达式替换Wie`re`。`re`表示一个正则表达式,它可以是另一个`regex`对象、一个`string`、一个指向空字符结尾的字符数组的指针或是一个花括号包围的字符列表。                                                  |
| `r1.assign(re, f)`             | 和使用赋值运算符（=）的效果相同：可选的标志`f`也和`regex`的构造函数中对应的参数含义相同。                                                                                                |
|`r.mark_count()`|`r`中子表达式的数目(即括号分组的数量)|
| `r.flags()`                    |返回`r`的标志集|
```c++
  std::string pattern(R"((\w+)\s+(\w+))");
  std::regex r(pattern);
  std::string test_str = "Hello World";
  smatch result;
  if (std::regex_search(test_str,result ,r)) {
    int num_groups = r.mark_count();
    std::cout << "Number of groups: " << num_groups << std::endl; //2
    cout<<result.str()<<endl;
  }
```

定义`regex`时指定的标志：

| 操作           | 解释                     |
|:------------:|:----------------------:|
| `icase`      | 在匹配过程中忽略大小写            |
| `nosubs`     | 不保存匹配的子表达式             |
| `optimize`   | 执行速度优先于构造速度            |
| `ECMAScript` | 使用`ECMA-262`指定的语法      |
| `basic`      | 使用`POSIX`基本的正则表达式语法    |
| `extended`   | 使用`POSIX`扩展的正则表达式语法    |
| `awk`        | 使用`POSIX`版本的`awk`语言的语法 |
| `grep`       | 使用`POSIX`版本的`grep`的语法  |
| `egrep`      | 使用`POSIX`版本的`egrep`的语法 |  
- 可以将正则表达式本身看做是一种简单程序语言设计的程序。在运行时,当一个`regex`对象被初始化或被赋予新模式时,才被“编译”。
- 如果编写的正则表达式存在错误,会在运行时抛出一个`regex_error`的异常。
- 避免创建不必要的正则表达式。构建一个`regex`对象可能比较耗时。

```c++
  regex r("[[:alnum:]]+\\.(cpp|cxx|cc)$",regex::icase); // 忽略大小写
  smatch results;
  string filename;
  while (cin>>filename){
    if (regex_search(filename,results,r)){
      cout<<results.str()<<endl;
    }
  }
```

### 匹配与regex迭代器类型
`sregex_iterator`操作（用来获得所有匹配）：

| 操作                             | 解释                                                                                          |
|:------------------------------:|:-------------------------------------------------------------------------------------------:|
| `sregex_iterator it(b, e, r);` | 一个`sregex_iterator`,遍历迭代器`b`和`e`表示的`string`。它调用`sregex_search(b, e, r)`将`it`定位到输入中第一个匹配的位置。 |
| `sregex_iterator end;`         | `sregex_iterator`的尾后迭代器                                                                     |
| `*it`, `it->`                  | 根据最后一个调用`regex_search`的结果,返回一个`smatch`对象的引用或一个指向`smatch`对象的指针。                              |
| `++it` , `it++`                | 从输入序列当前匹配位置开始调用`regex_search`。前置版本返回递增后迭代器；后置版本返回旧值。                                        |
| `it1 == it2`                   | 如果两个`sregex_iterator`都是尾后迭代器,则它们相等。两个非尾后迭代器是从相同的输入序列和`regex`对象构造,则它们相等。                     |  

示例：

```
// 将字符串file中所有匹配模式r的子串输出
for (sregex_iterator it(file.begin(), file.end(), r), end_it; it != end_it; ++it){
    cout << it ->str() << endl;
}
```

`smatch`操作：

| 操作                     | 解释                                                                                                  |
|:----------------------:|:---------------------------------------------------------------------------------------------------:|
| `m.ready()`            | 如果已经通过调用`regex_search`或`regex_match`设置了`m`,则返回`true`；否则返回`false`。如果`ready`返回`false`,则对`m`进行操作是未定义的。 |
| `m.size()`             | 如果匹配失败,则返回0,；否则返回最近一次匹配的正则表达式中子表达式的数目。                                                              |
| `m.empty()`            | 等价于`m.size() == 0`                                                                                  |
| `m.prefix()`           | 一个`ssub_match`对象,标识当前匹配之前的序列                                                                        |
| `m.suffix()`           | 一个`ssub_match`对象,标识当前匹配之后的部分                                                                        |
| `m.format(...)`        |                                                                                                     |
| `m.length(n)`          | 第`n`个匹配的子表达式的大小                                                                                     |
| `m.position(n)`        | 第`n`个子表达式距离序列开始的长度                                                                                  |
| `m.str(n)`             | 第`n`个子表达式匹配的`string`                                                                                |
| `m[n]`                 | 对应第`n`个子表达式的`ssub_match`对象                                                                          |
| `m.begin(), m.end()`   | 表示`m`中`ssub_match`元素范围的迭代器。                                                                         |
| `m.cbegin(), m.cend()` | 常量迭代器                                                                                               |  

```c++
  std::regex regex_pattern("([0-9]+)"); // 匹配数字的正则表达式模式
  std::string str = "Hello 123 World 456";
  std::smatch match;

  std::regex_search(str, match, regex_pattern);

  cout<<"m.ready:"<<match.ready()<<endl;
  cout<<"m.size:"<<match.size()<<endl;
  cout<<"m.prefix:"<<match.prefix()<<endl;
  cout<<"m.position:"<<match.position(1)<<endl;
  for (auto sub_match : match) {
    std::cout << "Sub-match: " << sub_match.str() << std::endl;
    std::cout << "Length: " << sub_match.length() << std::endl;
    std::cout << "m.prefix: " << sub_match << std::endl;
  }

```

### 使用子表达式

正则表达式语法通常用括号表示子表达式。
子表达式的索引从1开始。
在`fmt`中用`$`后跟子表达式的索引号来标识一个特定的子表达式。

示例：
```c++
if (regex_search(filename, results, r))
    cout << results.str(1) << endl;  // .str(1)获取第一个子表达式匹配结果
```

`ssub_match`子匹配操作：

| 操作                | 解释                                                                          |
|:-----------------:|:---------------------------------------------------------------------------:|
| `matched`         | 一个`public bool`数据成员,指出`ssub_match`是否匹配了                                     |
| `first`, `second` | `public`数据成员,指向匹配序列首元素和尾后位置的迭代器。如果未匹配,则`first`和`second`是相等的。                |
| `length()`        | 匹配的大小,如果`matched`为`false`,则返回0。                                             |
| `str()`           | 返回一个包含输入中匹配部分的`string`。如果`matched`为`false`,则返回空`string`。                    |
| `s = ssub`        | 将`ssub_match`对象`ssub`转化为`string`对象`s`。等价于`s=ssub.str()`,转换运算符不是`explicit`的。 |  

### 使用regex_replace

正则表达式替换操作：

| 操作                                                                        | 解释                                                                                                                                                           |
|:-------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| `m.format(dest, fmt, mft)`, `m.format(fmt, mft)`                          | 使用格式字符串`fmt`生成格式化输出,匹配在`m`中,可选的`match_flag_type`标志在`mft`中。第一个版本写入迭代器`dest`指向的目的为止,并接受`fmt`参数,可以是一个`string`,也可以是一个指向空字符结尾的字符数组的指针。`mft`的默认值是`format_default`。 |
| `rege_replace(dest, seq, r, fmt, mft)`, `regex_replace(seq, r, fmt, mft)` | 遍历`seq`,用`regex_search`查找与`regex`对象`r`相匹配的子串,使用格式字符串`fmt`和可选的`match_flag_type`标志来生成输出。`mft`的默认值是`match_default`                                              |  


```c++
string phone = "(\\()?(\\d{3})(\\))?([-. ])?(\\d{3})([-. ]?)(\\d{4})"
string fmt = "$2.$5.$7";  // $2代表第二个捕获组的值,$5代表第五个捕获组的值,$7代表第七个捕获组的值。
regex r(phone);  // 用来寻找模式的regex对象
string number = "(908) 555-1800";
cout << regex_replace(number, r, fmt) << endl;
```

匹配标志：

| 操作                  | 解释                     |
|:-------------------:|:----------------------:|
| `match_default`     | 等价于`format_default`    |
| `match_not_bol`     | 不将首字符作为行首处理            |
| `match_not_eol`     | 不将尾字符作为行尾处理            |
| `match_not_bow`     | 不将首字符作为单词首处理           |
| `match_not_eow`     | 不将尾字符作为单词尾处理           |
| `match_any`         | 如果存在多于一个匹配,则可以返回任意一个匹配 |
| `match_not_null`    | 不匹配任何空序列               |
| `match_continuous`  | 匹配必须从输入的首字符开始          |
| `match_prev_avail`  | 输入序列包含第一个匹配之前的内容       |
| `format_default`    | 用`ECMAScript`规则替换字符串   |
| `format_sed`        | 用`POSIX sed`规则替换字符串    |
| `format_no_copy`    | 不输出输入序列中未匹配的部分         |
| `format_first_only` | 只替换子表达式的第一次出现          |  

## 随机数

- 新标准之前,C和C++都依赖一个简单的C库函数`rand`来生成随机数,且只符合均匀分布。
- 新标准：**随机数引擎** + **随机数分布类**, 定义在 `random`头文件中。
- C++程序应该使用`default_random_engine`类和恰当的分布类对象。


### 随机数工具概览
C++为我们提供了一套强大的随机数生成工具,它们都包含在`<random>`头文件中。这些工具允许我们生成各种分布的随机数,如均匀分布、正态分布等。

|  工具名称 (Tool Name)               |  描述 (Description)                            |
|:-------------------------------:|:--------------------------------------------:|
|std::random_device|  一个真正的随机数生成器,通常用于为其他随机数引擎提供种子。               |
|std::mt19937|  一个基于Mersenne Twister算法的伪随机数生成器。             |
|  std::uniform_int_distribution  |  用于生成均匀分布的整数随机数。                             |
|  std::normal_distribution       |  用于生成正态分布的浮点随机数。                             |  

#### std::random_device的深入探索
`std::random_device`是一个真正的随机数生成器,它不依赖于任何算法,而是直接从系统的随机数源获取数据。这使得它非常适合为其他随机数引擎提供种子,确保每次程序运行时都能产生不同的随机序列。

```c++
std::random_device rd;  // 创建一个真正的随机数生成器
std::mt19937 gen(rd()); // 使用random_device为mt19937引擎提供种子
```


**随机数与伪随机数的区别**
在深入研究之前,我们首先需要理解两个关键术语：真随机数 (True Random Numbers) 和 伪随机数 (Pseudo-Random Numbers)。

- 真随机数：这些数字的生成不依赖于任何算法,而是基于某种物理现象,如放射性衰变或电子噪声。因此,它们是真正的随机数,不可预测。
- 伪随机数：这些数字是通过算法生成的,给定相同的初始种子,它们将产生相同的数字序列。尽管它们看起来是随机的,但实际上是可以预测的。

C++标准库提供了多种伪随机数生成器,例如：

- `std::default_random_engine`
- `std::mt19937` (Mersenne Twister 19937)
- `std::ranlux24_base`
每种生成器都有其特定的应用和特性。例如,std::mt19937是一个广泛使用的生成器,因为它提供了一个非常长的周期和高质量的随机数。

| 生成器                          | 特性      | 适用场景        |
|:----------------------------:|:-------:|:-----------:|
| `std::default_random_engine` | 默认的生成器  | 快速生成随机数     |
| `std::mt19937`               | 高质量,长周期 | 需要高质量随机数的应用 |
| `std::ranlux24_base`         | 少量的状态信息 | 嵌入式系统       |  


示例：
```c++
#include <iostream>
#include <random>

int main() {
    std::mt19937 generator; // 使用默认种子
    std::uniform_int_distribution<int> distribution(1, 100); // 生成1到100之间的随机数

    int random_number = distribution(generator);
    std::cout << "随机数 (Random Number): " << random_number << std::endl;

    return 0;
}
```

`std::random_device`（随机设备）是C++标准库中的一个类,它提供了一个**非确定性**的随机数生成器。与伪随机数生成器不同,它**不依赖于种子**,因此每次生成的随机数都是真正的随机。

| 随机数引擎                           | 是否需要种子 | 是否真随机 |
|:-------------------------------:|:------:|:-----:|
| std::random_device              | 否      | 是     |
| std::mt19937                    | 是      | 否     |
| std::linear_congruential_engine | 是      | 否     |  

**生成随机密码**

```c++
#include <random>
#include <string>
#include <iostream>

std::string generate_random_password(size_t length) {
    const char charset[] = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz";
    const size_t max_index = (sizeof(charset) - 1);
    std::random_device rd;
    std::string password;

    for (size_t i = 0; i < length; ++i) {
        password += charset[rd() % max_index];
    }

    return password;
}

int main() {
    std::cout << "Random password: " << generate_random_password(10) << std::endl;
    return 0;
}
```

随机打乱容器中的元素

```c++
#include <algorithm> // for std::shuffle
#include <vector>
#include <random>

std::vector<int> numbers = {1, 2, 3, 4, 5};
std::random_device rd; // 随机数设备 (Random device)
std::mt19937 g(rd()); // 使用Mersenne Twister算法生成随机数

std::shuffle(numbers.begin(), numbers.end(), g);
```


### 随机数引擎和分布

**随机数引擎操作**

| 操作                    | 解释                                    |
|:---------------------:|:-------------------------------------:|
| `Engine e;`           | 默认构造函数；使用该引擎类型默认的种子                   |
| `Engine e(s);`        | 使用整型值`s`作为种子                          |
| `e.seed(s)`           | 使用种子`s`重置引擎的状态                        |
| `e.min()`,`e.max()`   | 此引擎可生成的最小值和最大值                        |
| `Engine::result_type` | 此引擎生成的`unsigned`整型类型                  |
| `e.discard(u)`        | 将引擎推进`u`步；`u`的类型为`unsigned long long` |

示例：
```c++
// 初始化分布类型
uniform_int_distribution<unsigned> u(0, 9);
// 初始化引擎
default_random_engine e;
// 随机生成0-9的无符号整数
cout << u(e) << endl;
```


**设置随机数发生器种子**：
这里对于一个给定的发生器,每次运行程序它都会返回相同的数值序列。 
- 种子就是一个数值,引擎可以利用它从序列中一个新位置重新开始生成随机数。

	```c++
	  default_random_engine e1;
	  default_random_engine e2(12);
	  // 不同的种子,生成的数字不一样
	  for (int i = 0; i < 10; ++i) {
	    cout<<e1()<<endl;
	    cout<<e2()<<endl;
	  }
	```

- 种子可以使用系统函数`time(0)`。

### 其他随机数分布

分布类型的操作：

| 操作                  | 解释                                                          |
|:-------------------:|:-----------------------------------------------------------:|
| `Dist d;`           | 默认够赞函数；使`d`准备好被使用。其他构造函数依赖于`Dist`的类型；分布类型的构造函数是`explicit`的。 |
| `d(e)`              | 用相同的`e`连续调用`d`的话,会根据`d`的分布式类型生成一个随机数序列；`e`是一个随机数引擎对象。       |
| `d.min()`,`d.max()` | 返回`d(e)`能生成的最小值和最大值。                                        |
| `d.reset()`         | 重建`d`的状态,是的随后对`d`的使用不依赖于`d`已经生成的值。                          |  
#### 生成随机实数

生成均匀分布的随机数 
```c++
  default_random_engine e;
  uniform_real_distribution<double> u(0,1);
  for (int i = 0; i < 10; ++i) {
    cout << std::setiosflags(ios::fixed) << std::setprecision(4) << u(e) << " ";
  }
  cout << endl;
  cout << u(e) << endl;
  return 0;
```

分布类型的操作：

| 操作                  | 解释                                                          |
|:-------------------:|:-----------------------------------------------------------:|
| `Dist d;`           | 默认够赞函数；使`d`准备好被使用。其他构造函数依赖于`Dist`的类型；分布类型的构造函数是`explicit`的。 |
| `d(e)`              | 用相同的`e`连续调用`d`的话,会根据`d`的分布式类型生成一个随机数序列；`e`是一个随机数引擎对象。       |
| `d.min()`,`d.max()` | 返回`d(e)`能生成的最小值和最大值。                                        |
| `d.reset()`         | 重建`d`的状态,是的随后对`d`的使用不依赖于`d`已经生成的值。                          |  

生成非均匀分布的随机数 

```c++
  default_random_engine e;  // 生成随机整数
  normal_distribution<> n(4,1.5); // 均值4,标准差1.5
  vector<unsigned> vals(9);
  for (int i = 0; i < 200; ++i) {
    long v = lround(n(e));
    if (v<vals.size())
      ++vals[v];
  }
  for (int i = 0; i < vals.size(); ++i) {
    cout<<i<<": "<<string(vals[i],'*')<<endl;
  }
```

输出结果

```txt
0: ***
1: ********
2: ********************
3: **************************************
4: **********************************************************
5: ******************************************
6: ***********************
7: *******
8: *
```

# 特殊工具与技术

## typeid运算符

- `typeid运算符(typeid operator)`,它允许程序向表达式提问：**你的对象是什么类型？**
- `typeid`表达式的形式是`typeid(e)`,其中`e`可以是任意表达式或类型的名字,它操作的结果是一个常量对象的引用。它可以作用于任意类型的表达式。
- 通常情况下,使用typeid比较两条表达式的类型是否相同,或者比较一条表达式的类型是否与指定类型相同：

```c++
Derived *dp = new Derived;
Base *bp = dp;

if (typeid(*bp) == typeid(*dp)) {
    // bp和dp指向同一类型的对象
}

if (typeid(*bp) == typeid(Derived)) {
    // bp实际指向Derived对象
}
```

- 当`typeid`作用于指针时(而非指针所指向的对象),返回的结果是该指针的静态编译时类型。

```c++
// 下面的检查永远是失败的：bp的类型是指向Base的指针
if (typeid(bp) == typeid(Derived)) {
    // 永远不会执行
}
```


当我们想输出某个类型的名称时，其他平台可以用`typeid(T).name()` ,但是在Linux的g++环境下，g++对内置或者自定义的类型有内部的名称，如果要得到更直观的类型名称，可以使用一些库，例如 Boost.TypeIndex 库提供的 `boost::typeindex::type_id_with_cvr<T>().pretty_name()` 方法

```c++
#include <boost/type_index.hpp>
#include <iostream>

template<typename T>
void func(T a){
  std::cout << boost::typeindex::type_id_with_cvr<T>().pretty_name() << std::endl;
}
int main() {
  func(10);
  func("aaa");
  return EXIT_SUCCESS;
}
```


## 位域

- 类可以将其(非静态)数据成员定义成**位域(bit-field)**,在一个位域中含有一定数量的二进制位。当一个程序需要向其他程序或硬件设备传递二进制数据时,通常会用到位域。
- 位域在内存中的布局是与机器相关的。
- 位域的类型必须是整型或枚举类型。因为带符号位域的行为是由具体实现确定的,通常情况下我们使用无符号类型保存一个位域。

```c++
typedef unsigned int Bit;
class File {
    Bit mode: 2;
    Bit modified: 1;
    Bit prot_owner: 3;
    Bit prot_group: 3;
    Bit prot_world: 3;
public:
    enum modes {READ = 01, WRITE = 02, EXECUTE = 03};
    File &open(modes);
    void close();
    void write();
    bool isRead() const;
    void setWrite();
}
// 使用位域
void File::write() {
    modified = 1;
    // ...
}
void File::close() {
    if( modified)
        // ...保存内容
}
File &File::open(File::modes m) {
    mode |= READ;   // 按默认方式设置READ
    // 其他处理
    if(m & WRITE)   // 如果打开了READ和WRITE
        // 按照读/写方式打开文件
    return *this;
}
```

# 处理日期和时间的chrono库


C++11中提供了日期和时间相关的库chrono,通过chrono库可以很方便地处理日期和时间,为程序的开发提供了便利。chrono库主要包含三种类型的类：`时间间隔duration`、`时钟clocks`、`时间点time point`。

## 时间间隔duration
### 常用类成员

duration表示一段时间间隔,用来记录时间长度,可以表示几秒、几分钟、几个小时的时间间隔。duration的原型如下：
```c++
// 定义于头文件 <chrono>
template<
    class Rep,
    class Period = std::ratio<1>
> class duration;
```

- `Rep`：这是一个数值类型,表示时钟数（周期）的类型（默认为整形）。若 Rep 是浮点数,则 duration 能使用小数描述时钟周期的数目。
- `Period`：表示时钟的周期,它的原型如下：

```c++
// 定义于头文件 <ratio>
template<
    std::intmax_t Num,
    std::intmax_t Denom = 1
> class ratio;
```

ratio类表示每个时钟周期的秒数,其中第一个模板参数Num代表分子,Denom代表分母,该分母值默认为1,因此,ratio代表的是一个分子除以分母的数值,比如：`ratio<2>`代表一个时钟周期是2秒,`ratio<60>`代表一分钟,`ratio<60*60>`代表一个小时,`ratio<60*60*24>`代表一天。而`ratio<1,1000>`代表的是1/1000秒,也就是1毫秒,ratio`<1,1000000>`代表一微秒,`ratio<1,1000000000>`代表一纳秒。

为了方便使用,在标准库中定义了一些常用的时间间隔,比如：时、分、秒、毫秒、微秒、纳秒,它们都位于chrono命名空间下,定义如下：

<img src="https://article.biliimg.com/bfs/article/769e07c81d4708a014c7b6aff180cdd138716159.png" alt="image.png" style="zoom:50%;" />



<font color=#ff0000>注意：到 hours 为止的每个预定义时长类型至少涵盖 ±292 年的范围。</font>

duration类的构造函数原型如下：
```c++
// 1. 拷贝构造函数
duration( const duration& ) = default;
// 2. 通过指定时钟周期的类型来构造对象
template< class Rep2 >
constexpr explicit duration( const Rep2& r );
// 3. 通过指定时钟周期类型,和时钟周期长度来构造对象
template< class Rep2, class Period2 >
constexpr duration( const duration<Rep2,Period2>& d );
```

> 为了更加方便的进行duration对象之间的操作,类内部进行了操作符重载：

<img src="https://article.biliimg.com/bfs/article/2b4ce09895c222e6a6a2738c6528394438716159.png" alt="image.png" style="zoom:50%;" />
duration类还提供了获取时间间隔的时钟周期数的方法`count()`,函数原型如下：

```c++
constexpr rep count() const;
```

### 类的使用

> 通过构造函数构造事件间隔对象示例代码如下：

```c++
#include <chrono>
#include <iostream>
using namespace std;
int main()
{
    chrono::hours h(1);                          // 一小时
    chrono::milliseconds ms{ 3 };                // 3 毫秒
    chrono::duration<int, ratio<1000>> ks(3);    // 3000 秒

    // chrono::duration<int, ratio<1000>> d3(3.5);  // error
    chrono::duration<double> dd(6.6);               // 6.6 秒

    // 使用小数表示时钟周期的次数
    chrono::duration<double, std::ratio<1, 30>> hz(3.5);
}
```

- h(1)时钟周期为1小时,共有1个时钟周期,所以h表示的时间间隔为1小时
- ms(3)时钟周期为1毫秒,共有3个时钟周期,所以ms表示的时间间隔为3毫秒
- ks(3)时钟周期为1000秒,一共有三个时钟周期,所以ks表示的时间间隔为3000秒
- d3(3.5)时钟周期为1000秒,时钟周期数量只能用整形来表示,但是此处指定的是浮点数,因此语法错误
- dd(6.6)时钟周期为默认的1秒,共有6.6个时钟周期,所以dd表示的时间间隔为6.6秒
- hz(3.5)时钟周期为1/30秒,共有3.5个时钟周期,所以hz表示的时间间隔为`1/30*3.5`秒

chrono库中根据duration类封装了不同长度的时钟周期（也可以自定义）,基于这个时钟周期再进行周期次数的设置就可以得到总的时间间隔了（`时钟周期 * 周期次数 = 总的时间间隔`）。


```c++
#include <chrono>
#include <iostream>
int main()
{
    std::chrono::milliseconds ms{3};         // 3 毫秒
    std::chrono::microseconds us = 2*ms;     // 6000 微秒
    // 时间间隔周期为 1/30 秒
    std::chrono::duration<double, std::ratio<1, 30>> hz(3.5);
 
    std::cout <<  "3 ms duration has " << ms.count() << " ticks\n"
              <<  "6000 us duration has " << us.count() << " ticks\n"
              <<  "3.5 hz duration has " << hz.count() << " ticks\n";       
}

```

输出的结果为：
```c++
3 ms duration has 3 ticks
6000 us duration has 6000 ticks
3.5 hz duration has 3.5 ticks
```

- `ms`时间单位为毫秒,初始化操作`ms{3}`表示时间间隔为3毫秒,一共有3个时间周期,每个周期为1毫秒
- `us`时间单位为微秒,初始化操作`2*ms`表示时间间隔为6000微秒,一共有6000个时间周期,每个周期为1微秒
- `hz`时间单位为秒,初始化操作hz(3.5)表示时间间隔为`1/30*3.5`秒,一共有3.5个时间周期,每个周期为1/30秒

由于在duration类内部做了操作符重载,因此时间间隔之间可以直接进行算术运算,比如我们要计算两个时间间隔的差值,就可以在代码中做如下处理

```c++
#include <iostream>
#include <chrono>
using namespace std;

int main()
{
    chrono::minutes t1(10);
    chrono::seconds t2(60);
    chrono::seconds t3 = t1 - t2;
    cout << t3.count() << " second" << endl;
}
```

程序输出的结果：

```c++
540 second
```

在上面的测试程序中,t1代表10分钟,t2代表60秒,t3是t1减去t2,也就是`60*10-60=540`,这个540表示的时钟周期,每个时钟周期是1秒,因此两个时间间隔之间的差值为540秒。

注意事项：duration的加减运算有一定的规则,当两个duration时钟周期不相同的时候,会先统一成一种时钟,然后再进行算术运算,统一的规则如下：假设有`ratio<x1,y1>` 和 `ratio<x2,y2>`两个时钟周期,首先需要求出x1,x2的最大公约数X,然后求出y1,y2的最小公倍数Y,统一之后的时钟周期ratio为`ratio<X,Y>`。

```c++
#include <iostream>
#include <chrono>
using namespace std;

int main()
{
    chrono::duration<double, ratio<9, 7>> d1(3);
    chrono::duration<double, ratio<6, 5>> d2(1);
    // d1 和 d2 统一之后的时钟周期
    chrono::duration<double, ratio<3, 35>> d3 = d1 - d2;
}
```

对于分子6,、9最大公约数为3,对于分母7、5最小公倍数为35,因此推导出的时钟周期为`ratio<3,35>`

## 时间点 time point
chrono库中提供了一个表示时间点的类time_point,该类的定义如下：

```c++
// 定义于头文件 <chrono>
template<
    class Clock,
    class Duration = typename Clock::duration
> class time_point;
```

它被实现成如同存储一个 `Duration` 类型的自 `Clock` 的纪元起始开始的时间间隔的值,通过这个类最终可以得到时间中的某一个时间点。

- `Clock`：此时间点在此时钟上计量
- `Duration`：用于计量从纪元起时间的 `std::chrono::duration` 类型


比如当前系统时间点

```c++
static time_point
      now() noexcept;
auto now=std::chrono::system_clock::now();
```



> time_point类的构造函数原型如下：

```c++
// 1. 构造一个以新纪元(epoch,即：1970.1.1)作为值的对象,需要和时钟类一起使用,不能单独使用该无参构造函数
time_point();
// 2. 构造一个对象,表示一个时间点,其中d的持续时间从epoch开始,需要和时钟类一起使用,不能单独使用该构造函数
explicit time_point( const duration& d );
// 3. 拷贝构造函数,构造与t相同时间点的对象,使用的时候需要指定模板参数
template< class Duration2 >
time_point( const time_point<Clock,Duration2>& t );
```

> 在这个类中除了构造函数还提供了另外一个`time_since_epoch()`函数,用来获得1970年1月1日到time_point对象中记录的时间经过的时间间隔（duration）,函数原型如下：

```c++
duration time_since_epoch() const;
```

除此之外,时间点time_point对象和时间段对象duration之间还支持直接进行算术运算（即加减运算）,时间点对象之间可以进行逻辑运算,具体细节可以参考下面的表格：

其中 tp 和 tp2 是`time_point` 类型的对象, dtn 是`duration`类型的对象。

<img src="https://article.biliimg.com/bfs/article/e870443adf16e8f59af2cda08ee4b0b138716159.png" alt="image.png" style="zoom:50%;" />

duration可以进行类型转换


```c++
std::chrono::duration_cast<std::chrono::>()
```


还可以转换成时间字符串

```c++
static std::time_t
      to_time_t(const time_point& __t)
```


```c++
  auto now=std::chrono::system_clock::now(); // 当前时间
  auto duration=  now.time_since_epoch(); // 从1970 到现在的间隔
  std::cout << "时间点的纳秒数：" << duration.count() << std::endl;
  auto future=now+std::chrono::hours(24);
  auto time_diff=future-now;
  std::cout << "时间点之间的时间间隔（小时）：" << std::chrono::duration_cast<std::chrono::hours>(time_diff).count()
            << std::endl;

  std::time_t now_t = std::chrono::system_clock::to_time_t(now);
  std::cout << "当前时间：" << std::ctime(&now_t);

```

## 时钟clocks

chrono库中提供了获取当前的系统时间的时钟类,包含的时钟一共有三种：

- `system_clock`：系统的时钟,系统的时钟可以修改,甚至可以网络对时,因此使用系统时间计算时间差可能不准。
- `steady_clock`：是固定的时钟,相当于秒表。开始计时后,时间只会增长并且不能修改,适合用于记录程序耗时
- `high_resolution_clock`：和时钟类 steady_clock 是等价的（是它的别名）。
在这些时钟类的内部有time_point、duration、Rep、Period等信息,基于这些信息来获取当前时间,以及实现time_t和time_point之间的相互转换。

在这些时钟类的内部有`time_point`、`duration`、`Rep`、`Period`等信息,基于这些信息来获取当前时间,以及实现`time_t`和`time_point`之间的相互转换。

|  时钟类成员类型       |  描述                                                |
|:--------------:|:--------------------------------------------------:|
|  rep           |  表示时钟周期次数的有符号算术类型                                  |
|  period        |  表示时钟计次周期的 std::ratio 类型                           |
|  duration      |  时间间隔,可以表示负时长                                      |
|  time_point    |  表示在当前时钟里边记录的时间点                                   |  

在使用chrono提供的时钟类的时候,不需要创建类对象,直接调用类的静态方法就可以得到想要的时间了。


### system_clock
具体来说,时钟类`system_clock`是一个系统范围的实时时钟。`system_clock`提供了对当前时间点`time_point`的访问,将得到时间点转换为`time_t`类型的时间对象,就可以基于这个时间对象获取到当前的时间信息了。

> system_clock时钟类在底层源码中的定义如下：

```c++
struct system_clock { // wraps GetSystemTimePreciseAsFileTime/GetSystemTimeAsFileTime
    using rep                       = long long;
    using period                    = ratio<1, 10'000'000>; // 100 nanoseconds
    using duration                  = chrono::duration<rep, period>;
    using time_point                = chrono::time_point<system_clock>;
    static constexpr bool is_steady = false;

    _NODISCARD static time_point now() noexcept 
    { // get current time
        return time_point(duration(_Xtime_get_ticks()));
    }

    _NODISCARD static __time64_t to_time_t(const time_point& _Time) noexcept 
    { // convert to __time64_t
        return duration_cast<seconds>(_Time.time_since_epoch()).count();
    }

    _NODISCARD static time_point from_time_t(__time64_t _Tm) noexcept 
    { // convert from __time64_t
        return time_point{seconds{_Tm}};
    }
};
```

> 通过以上源码可以了解到在`system_clock`类中的一些细节信息：

- rep：时钟周期次数是通过整形来记录的long long
- period：一个时钟周期是100纳秒`ratio<1, 10'000'000>`
- duration：时间间隔为`rep*period`纳秒`chrono::duration<rep, period>`
- time_point：时间点通过系统时钟做了初始化`chrono::time_point<system_clock>`,里面记录了新纪元时间点

> 另外还可以看到`system_clock`类一共提供了三个静态成员函数：

```c++
// 返回表示当前时间的时间点。
static std::chrono::time_point<std::chrono::system_clock> now() noexcept;
// 将 time_point 时间点类型转换为 std::time_t 类型
static std::time_t to_time_t( const time_point& t ) noexcept;
// 将 std::time_t 类型转换为 time_point 时间点类型
static std::chrono::system_clock::time_point from_time_t( std::time_t t ) noexcept;
```

> 比如,我们要获取当前的系统时间,并且需要将其以能够识别的方式打印出来,示例代码如下：

```c++
#include <chrono>
#include <iostream>
using namespace std;
using namespace std::chrono;
int main()
{
    // 新纪元1970.1.1时间
    system_clock::time_point epoch;

    duration<int, ratio<60*60*24>> day(1);
    // 新纪元1970.1.1时间 + 1天
    system_clock::time_point ppt(day);

    using dday = duration<int, ratio<60 * 60 * 24>>;
    // 新纪元1970.1.1时间 + 10天
    time_point<system_clock, dday> t(dday(10));

    // 系统当前时间
    system_clock::time_point today = system_clock::now();
    
    // 转换为time_t时间类型
    time_t tm = system_clock::to_time_t(today);
    cout << "今天的日期是:    " << ctime(&tm);

    time_t tm1 = system_clock::to_time_t(today+day);
    cout << "明天的日期是:    " << ctime(&tm1);

    time_t tm2 = system_clock::to_time_t(epoch);
    cout << "新纪元时间:      " << ctime(&tm2);

    time_t tm3 = system_clock::to_time_t(ppt);
    cout << "新纪元时间+1天:  " << ctime(&tm3);

    time_t tm4 = system_clock::to_time_t(t);
    cout << "新纪元时间+10天: " << ctime(&tm4);
}
```

示例代码打印的结果为：
```c++
今天的日期是:    Thu Apr  8 11:09:49 2021
明天的日期是:    Fri Apr  9 11:09:49 2021
新纪元时间:      Thu Jan  1 08:00:00 1970
新纪元时间+1天:  Fri Jan  2 08:00:00 1970
新纪元时间+10天: Sun Jan 11 08:00:00 1970
```

### steady_clock
如果我们通过时钟不是为了获取当前的系统时间,而是进行程序耗时的时长,此时使用syetem_clock就不合适了,因为这个时间可以跟随系统的设置发生变化。在C++11中提供的时钟类steady_clock相当于秒表,只要启动就会进行时间的累加,并且不能被修改,非常适合于进行耗时的统计。

> `steady_clock`时钟类在底层源码中的定义如下：

```c++
struct steady_clock { // wraps QueryPerformanceCounter
    using rep                       = long long;
    using period                    = nano;
    using duration                  = nanoseconds;
    using time_point                = chrono::time_point<steady_clock>;
    static constexpr bool is_steady = true;

    // get current time
    _NODISCARD static time_point now() noexcept 
    { 
        // doesn't change after system boot
        const long long _Freq = _Query_perf_frequency(); 
        const long long _Ctr  = _Query_perf_counter();
        static_assert(period::num == 1, "This assumes period::num == 1.");
        const long long _Whole = (_Ctr / _Freq) * period::den;
        const long long _Part  = (_Ctr % _Freq) * period::den / _Freq;
        return time_point(duration(_Whole + _Part));
    }
};
```

通过以上源码可以了解到在steady_clock类中的一些细节信息：

- rep：时钟周期次数是通过整形来记录的long long
- period：一个时钟周期是1纳秒nano
- duration：时间间隔为1纳秒nanoseconds
- time_point：时间点通过系统时钟做了初始化`chrono::time_point<steady_clock>`

另外,在这个类中也提供了一个静态的`now()`方法,用于得到当前的时间点,函数原型如下：

```c++
static std::chrono::time_point<std::chrono::steady_clock> now() noexcept;
```

假设要测试某一段程序的执行效率,可以计算它执行期间消耗的总时长,示例代码如下：
```c++
#include <chrono>
#include <iostream>
using namespace std;
using namespace std::chrono;
int main()
{
    // 获取开始时间点
    steady_clock::time_point start = steady_clock::now();
    // 执行业务流程
    cout << "print 1000 stars ...." << endl;
    for (int i = 0; i < 1000; ++i)
    {
        cout << "*";
    }
    cout << endl;
    // 获取结束时间点
    steady_clock::time_point last = steady_clock::now();
    // 计算差值
    auto dt = last - start;
    cout << "总共耗时: " << dt.count() << "纳秒" << endl;
}
```

#### high_resolution_clock
`high_resolution_clock`提供的时钟精度比`system_clock`要高,它也是不可以修改的。在底层源码中,这个类其实是`steady_clock`类的别名。

```c++
using high_resolution_clock = steady_clock;
```
因此`high_resolution_clock`的使用方式和`steady_clock`是一样的,在此就不再过多进行赘述了。

## 转换函数
### duration_cast

`duration_cast`是chrono库提供的一个模板函数,这个函数不属于duration类。通过这个函数可以对duration类对象内部的时钟周期Period,和周期次数的类型Rep进行修改,该函数原型如下：
```c++
template <class ToDuration, class Rep, class Period>
  constexpr ToDuration duration_cast (const duration<Rep,Period>& dtn);
```

1. 如果是对时钟周期进行转换：源时钟周期必须能够整除目的时钟周期（比如：小时到分钟）。
2. 如果是对时钟周期次数的类型进行转换：低等类型默认可以向高等类型进行转换（比如：int 转 double）。
3. 如果时钟周期和时钟周期次数类型都变了,根据第二点进行推导（也就是看时间周期次数类型）。
4. 以上条件都不满足,那么就需要使用 duration_cast 进行显示转换。

我们可以修改一下上面测试程序执行时间的代码,在代码中修改duration对象的属性：

```c++
#include <iostream>
#include <chrono>
using namespace std;
using namespace std::chrono;

void f()
{
    cout << "print 1000 stars ...." << endl;
    for (int i = 0; i < 1000; ++i)
    {
        cout << "*";
    }
    cout << endl;
}

int main()
{
    auto t1 = steady_clock::now();
    f();
    auto t2 = steady_clock::now();

    // 整数时长：时钟周期纳秒转毫秒,要求 duration_cast
    auto int_ms = duration_cast<chrono::milliseconds>(t2 - t1);

    // 小数时长：不要求 duration_cast
    duration<double, ratio<1, 1000>> fp_ms = t2 - t1;

    cout << "f() took " << fp_ms.count() << " ms, "
        << "or " << int_ms.count() << " whole milliseconds\n";
}

```

示例代码输出的结果：
```
print 1000 stars ....

f() took 40.2547 ms, or 40 whole milliseconds。
```

### time_point_cast
`time_point_cast`也是chrono库提供的一个模板函数,这个函数不属于time_point类。函数的作用是对时间点进行转换,因为不同的时间点对象内部的时钟周期Period,和周期次数的类型Rep可能也是不同的,一般情况下它们之间可以进行隐式类型转换,也可以通过该函数显示的进行转换,函数原型如下：

```c++
template <class ToDuration, class Clock, class Duration>
time_point<Clock, ToDuration> time_point_cast(const time_point<Clock, Duration> &t);
```

关于函数的使用,示例代码如下：
```c++
#include <chrono>
#include <iostream>
using namespace std;

using Clock = chrono::high_resolution_clock;
using Ms = chrono::milliseconds;
using Sec = chrono::seconds;
template<class Duration>
using TimePoint = chrono::time_point<Clock, Duration>;

void print_ms(const TimePoint<Ms>& time_point)
{
    std::cout << time_point.time_since_epoch().count() << " ms\n";
}

int main()
{
    TimePoint<Sec> time_point_sec(Sec(6));
    // 无精度损失, 可以进行隐式类型转换
    TimePoint<Ms> time_point_ms(time_point_sec);
    print_ms(time_point_ms);    // 6000 ms

    time_point_ms = TimePoint<Ms>(Ms(6789));
    // error,会损失精度,不允许进行隐式的类型转换
    TimePoint<Sec> sec(time_point_ms);

    // 显示类型转换,会损失精度。6789 truncated to 6000
    time_point_sec = std::chrono::time_point_cast<Sec>(time_point_ms);
    print_ms(time_point_sec); // 6000 ms
}
```

<font color=#ff0000>注意事项：关于时间点的转换如果没有没有精度的损失可以直接进行隐式类型转换,如果会损失精度只能通过显示类型转换,也就是调用time_point_cast函数来完成该操作</font>。

# 线程

## C++线程的使用

C++11中提供的线程类叫做`std::thread`,基于这个类创建一个新的线程非常的简单,只需要提供线程函数或者函数对象即可,并且可以同时指定线程函数的参数。我们首先来了解一下这个类提供的一些常用API：
### 构造函数
```c++
// ①
thread() noexcept;
// ②
thread( thread&& other ) noexcept;
// ③
template< class Function, class... Args >
explicit thread( Function&& f, Args&&... args );
// ④
thread( const thread& ) = delete;
```

- 构造函数①：默认构造函,构造一个线程对象,在这个线程中不执行任何处理动作
- 构造函数②：移动构造函数,将 `other` 的线程所有权转移给新的`thread` 对象。之后 `other` 不再表示执行线程。
- 构造函数③：创建线程对象,并在该线程中执行函数f中的业务逻辑,args是要传递给函数f的参数
	- 任务函数`f`的可选类型有很多,具体如下：
		- `普通函数,类成员函数,匿名函数,仿函数`（这些都是可调用对象类型）
		- 可以是可调用对象包装器类型,也可以是使用绑定器绑定之后得到的类型（仿函数）
- 构造函数④：使用`=delete`显示删除拷贝构造, 不允许线程对象之间的拷贝

- `thread::hardware_concurrency()` :获取当前系统支持的最大线程数量
### 公共成员函数
#### get_id()
应用程序启动之后默认只有一个线程,这个线程一般称之为**主线程或父线程**,通过线程类创建出的线程一般称之为子线程,每个被创建出的线程实例都对应一个线程ID,这个ID是唯一的,可以通过这个ID来区分和识别各个已经存在的线程实例,这个获取线程ID的函数叫做`get_id()`,函数原型如下：

```c++
std::thread::id get_id() const noexcept;
```

示例程序如下：
```c++
#include <iostream>
#include <thread>
#include <chrono>
using namespace std;

void func(int num, string str)
{
    for (int i = 0; i < 10; ++i)
    {
        cout << "子线程: i = " << i << "num: " 
             << num << ", str: " << str << endl;
    }
}

void func1()
{
    for (int i = 0; i < 10; ++i)
    {
        cout << "子线程: i = " << i << endl;
    }
}

int main()
{
    cout << "主线程的线程ID: " << this_thread::get_id() << endl;
    thread t(func, 520, "i love you");
    thread t1(func1);
    cout << "线程t 的线程ID: " << t.get_id() << endl;
    cout << "线程t1的线程ID: " << t1.get_id() << endl;
}
```

- `thread t(func, 520, "i love you")`;：创建了子线程对象`t`,`func()`函数会在这个子线程中运行
	- `func()`是一个回调函数,线程启动之后就会执行这个任务函数,程序猿只需要实现即可
	- `func()`的参数是通过`thread`的参数进行传递的,`520,i love you`都是调用`func()`需要的实参
	- 线程类的构造函数③是一个变参函数,因此无需担心线程任务函数的参数个数问题
	- 任务函数`func()`一般返回值指定为void,因为子线程在调用这个函数的时候不会处理其返回值
- `thread t1(func1)`;：子线程对象t1中的任务函数`func1()`,没有参数,因此在线程构造函数中就无需指定了
- 通过线程对象调用`get_id()`就可以知道这个子线程的线程ID了,`t.get_id()`,`t1.get_id()`

[[C++#命名空间 - this_thread|基于命名空间 this_thread 得到当前线程的线程ID]]

在上面的示例程序中有一个bug,在主线程中依次创建出两个子线程,打印两个子线程的线程ID,最后主线程执行完毕就退出了（主线程就是执行main()函数的那个线程）。默认情况下,主线程销毁时会将与其关联的两个子线程也一并销毁,但是这时有可能子线程中的任务还没有执行完毕,最后也就得不到我们想要的结果了。

当启动了一个线程（创建了一个thread对象）之后,在这个线程结束的时候（`std::terminate()`）,我们如何去回收线程所使用的资源呢？thread库给我们两种选择：

- 加入式（join()）
- 分离式（detach()）
另外,我们必须要在线程对象销毁之前在二者之间作出选择,否则程序运行期间就会有bug产生。

#### join()
join()字面意思是连接一个线程,意味着主动地等待线程的终止（**线程阻塞**）。在某个线程中通过子线程对象调用join()函数,调用这个函数的线程被阻塞,但是子线程对象中的任务函数会继续执行,当任务执行完毕之后join()会**清理当前子线程中的相关资源**然后返回,同时,调用该函数的线程解除阻塞继续向下执行。

再次强调,我们一定要搞清楚这个函数阻塞的是哪一个线程,函数在哪个线程中被执行,那么函数就阻塞哪个线程。该函数的函数原型如下

```c++
void join();
```

有了这样一个线程阻塞函数之后,就可以解决在上面测试程序中的bug了,<font color=#ff0000>如果要阻塞主线程的执行,只需要在主线程中通过子线程对象调用这个方法即可,当调用这个方法的子线程对象中的任务函数执行完毕之后,主线程的阻塞也就随之解除了</font>。修改之后的示例代码如下

```c++
int main()
{
    cout << "主线程的线程ID: " << this_thread::get_id() << endl;
    thread t(func, 520, "i love you");
    thread t1(func1);
    cout << "线程t 的线程ID: " << t.get_id() << endl;
    cout << "线程t1的线程ID: " << t1.get_id() << endl;
    t.join();
    t1.join();
}
```

当主线程运行到第八行`t.join()`;,根据子线程对象t的任务函数`func()`的执行情况,主线程会做如下处理：
- 如果任务函数`func()`还没执行完毕,主线程阻塞,直到任务执行完毕,主线程解除阻塞,继续向下运行
- 如果任务函数`func()`已经执行完毕,主线程不会阻塞,继续向下运行
同样,第9行的代码亦如此。

> 为了更好的理解`join()`的使用,再来给大家举一个例子,场景如下：
> 程序中一共有三个线程,其中两个子线程负责分段下载同一个文件,下载完毕之后,由主线程对这个文件进行下一步处理,那么示例程序就应该这么写：

```c++
#include <iostream>
#include <thread>
#include <chrono>
using namespace std;

void download1()
{
    // 模拟下载, 总共耗时500ms,阻塞线程500ms
    this_thread::sleep_for(chrono::milliseconds(500));
    cout << "子线程1: " << this_thread::get_id() << ", 找到历史正文...." << endl;
}

void download2()
{
    // 模拟下载, 总共耗时300ms,阻塞线程300ms
    this_thread::sleep_for(chrono::milliseconds(300));
    cout << "子线程2: " << this_thread::get_id() << ", 找到历史正文...." << endl;
}

void doSomething()
{
    cout << "集齐历史正文, 呼叫罗宾...." << endl;
    cout << "历史正文解析中...." << endl;
    cout << "起航,前往拉夫德尔...." << endl;
    cout << "找到OnePiece, 成为海贼王, 哈哈哈!!!" << endl;
    cout << "若干年后,草帽全员卒...." << endl;
    cout << "大海贼时代再次被开启...." << endl;
}

int main()
{
    thread t1(download1);
    thread t2(download2);
    // 阻塞主线程,等待所有子线程任务执行完毕再继续向下执行
    t1.join();
    t2.join();
    doSomething();
}
```

示例程序输出的结果：
```c++
子线程2: 72540, 找到历史正文....
子线程1: 79776, 找到历史正文....
集齐历史正文, 呼叫罗宾....
历史正文解析中....
起航,前往拉夫德尔....
找到OnePiece, 成为海贼王, 哈哈哈!!!
若干年后,草帽全员卒....
大海贼时代再次被开启....
```

在上面示例程序中最核心的处理是在主线程调用`doSomething()`;之前在第35、36行通过子线程对象调用了join()方法,这样就能够保证两个子线程的任务都执行完毕了,也就是文件内容已经全部下载完成,主线程再对文件进行后续处理,如果子线程的文件没有下载完毕,主线程就去处理文件,很显然从逻辑上讲是有问题的。

[[C++#sleep_for()|基于命名空间 this_thread 让当前线程休眠]]

#### detach()
`detach()`函数的作用是进行线程分离,分离主线程和创建出的子线程。<font color=#ff0000>在线程分离之后,主线程退出也会一并销毁创建出的所有子线程,在主线程退出之前,它可以脱离主线程继续独立的运行,任务执行完毕之后,这个子线程会自动释放自己占用的系统资源</font>。（其实就是孩子翅膀硬了,和家里断绝关系,自己外出闯荡了,如果家里被诛九族还是会受牵连）。该函数函数原型如下：

```c++
void detach();
```

线程分离函数没有参数也没有返回值,只需要在线程成功之后,通过线程对象调用该函数即可,继续将上面的测试程序修改一下：

```c++
int main()
{
    cout << "主线程的线程ID: " << this_thread::get_id() << endl;
    thread t(func, 520, "i love you");
    thread t1(func1);
    cout << "线程t 的线程ID: " << t.get_id() << endl;
    cout << "线程t1的线程ID: " << t1.get_id() << endl;
    t.detach();
    t1.detach();
    // 让主线程休眠, 等待子线程执行完毕
    this_thread::sleep_for(chrono::seconds(5));
}
```

<font color=#ff0000>注意事项：线程分离函数detach()不会阻塞线程,子线程和主线程分离之后,在主线程中就不能再对这个子线程做任何控制了,比如：通过join()阻塞主线程等待子线程中的任务执行完毕,或者调用get_id()获取子线程的线程ID。有利就有弊,鱼和熊掌不可兼得,建议使用join()。</font>

#### joinable()
joinable()函数用于判断主线程和子线程是否处理关联（连接）状态,一般情况下,二者之间的关系处于关联状态,该函数返回一个布尔类型：

- 返回值为true：主线程和子线程之间有关联（连接）关系
- 返回值为false：主线程和子线程之间没有关联（连接）关系

```c++
bool joinable() const noexcept;
```

示例代码如下：

```c++
#include <iostream>
#include <thread>
#include <chrono>
using namespace std;

void foo()
{
    this_thread::sleep_for(std::chrono::seconds(1));
}

int main()
{
    thread t;
    cout << "before starting, joinable: " << t.joinable() << endl;

    t = thread(foo);
    cout << "after starting, joinable: " << t.joinable() << endl;

    t.join();
    cout << "after joining, joinable: " << t.joinable() << endl;

    thread t1(foo);
    cout << "after starting, joinable: " << t1.joinable() << endl;
    t1.detach();
    cout << "after detaching, joinable: " << t1.joinable() << endl;
}
```

示例代码打印的结果如下：

```c++
before starting, joinable: 0
after starting, joinable: 1
after joining, joinable: 0
after starting, joinable: 1
after detaching, joinable: 0
```

基于示例代码打印的结果可以得到以下结论：

- 在创建的子线程对象的时候,如果没有指定任务函数,那么子线程不会启动,主线程和这个子线程也不会进行连接
- 在创建的子线程对象的时候,如果指定了任务函数,子线程启动并执行任务,主线程和这个子线程自动连接成功
- 子线程调用了`detach()`函数之后,父子线程分离,同时二者的连接断开,调用`joinable()`返回false
- 在子线程调用了`join()`函数,子线程中的任务函数继续执行,直到任务处理完毕,这时`join()`会清理（回收）当前子线程的相关资源,所以这个子线程和主线程的连接也就断开了,因此,调用`join()`之后再调用`joinable()`会返回false。

#### operator=

线程中的资源是不能被复制的,因此通过=操作符进行赋值操作最终并不会得到两个完全相同的对象。
```c++
// move (1)	
thread& operator= (thread&& other) noexcept;
// copy [deleted] (2)	
thread& operator= (const other&) = delete;
```

通过以上=操作符的重载声明可以得知：

- 如果`other`是一个右值,会进行资源所有权的转移
- 如果`other`不是右值,禁止拷贝,该函数被显示删除（=delete）,不可用

[[C语言#线程概述|C线程库]]
## 命名空间 - this_thread

在C++11中不仅添加了线程类,还添加了一个关于线程的命名空间`std::this_thread`,在这个命名空间中提供了四个公共的成员函数,通过这些成员函数就可以对当前线程进行相关的操作了。

### get_id()
调用命名空间`std::this_thread`中的`get_id()`方法可以得到当前线程的线程ID,函数原型如下：

```c++
thread::id get_id() noexcept;
```

关于函数使用对应的示例代码如下：

```c++
#include <iostream>
#include <thread>
using namespace std;

void func()
{
    cout << "子线程: " << this_thread::get_id() << endl;
}

int main()
{
    cout << "主线程: " << this_thread::get_id() << endl;
    thread t(func);
    t.join();
}
```

程序启动,开始执行`main()`函数,此时只有一个线程也就是主线程。当创建了子线程对象t之后,指定的函数func()会在子线程中执行,这时通过调用`this_thread::get_id()`就可以得到当前线程的线程ID了。

### sleep_for()
[[C语言#进程状态|进程被创建后一共有五种状态]]
同样地线程被创建后也有这五种状态：`创建态,就绪态,运行态,阻塞态(挂起态),退出态(终止态)` ,关于状态之间的转换是一样的,请参考进程,在此不再过多的赘述。

线程和进程的执行有很多相似之处,在计算机中启动的多个线程都需要占用CPU资源,但是CPU的个数是有限的并且每个CPU在同一时间点不能同时处理多个任务。<font color=#ff0000>为了能够实现并发处理,多个线程都是分时复用CPU时间片,快速的交替处理各个线程中的任务。因此多个线程之间需要争抢CPU时间片,抢到了就执行,抢不到则无法执行</font>（因为默认所有的线程优先级都相同,内核也会从中调度,不会出现某个线程永远抢不到CPU时间片的情况）。

命名空间`this_thread`中提供了一个休眠函数`sleep_for()`,调用这个函数的线程会马上从运行态变成阻塞态并在这种状态下休眠一定的时长,因为阻塞态的线程已经让出了CPU资源,代码也不会被执行,所以线程休眠过程中对CPU来说没有任何负担。这个函数是函数原型如下,参数需要指定一个休眠时长,是一个时间段：

[[C++#时间间隔duration|chrono库中的时间段类 duration 的使用]]

```c++
template <class Rep, class Period>
  void sleep_for (const chrono::duration<Rep,Period>& rel_time);
```

示例程序如下：

```c++
#include <iostream>
#include <thread>
#include <chrono>
using namespace std;

void func()
{
    for (int i = 0; i < 10; ++i)
    {
        this_thread::sleep_for(chrono::seconds(1));
        cout << "子线程: " << this_thread::get_id() << ", i = " << i << endl;
    }
}

int main()
{
    thread t(func);
    t.join();
}
```

在`func()`函数的for循环中使用了`this_thread::sleep_for(chrono::seconds(1))`;之后,每循环一次程序都会阻塞1秒钟,也就是说每隔1秒才会进行一次输出。需要注意的是：<font color=#ff0000>程序休眠完成之后,会从阻塞态重新变成就绪态,就绪态的线程需要再次争抢CPU时间片,抢到之后才会变成运行态,这时候程序才会继续向下运行。</font>


### sleep_until()
命名空间this_thread中提供了另一个休眠函数`sleep_until()`,和`sleep_for()`不同的是它的参数类型不一样

`sleep_until()`：指定线程阻塞到某一个指定的时间点`time_point`类型,之后解除阻塞
`sleep_for()`：指定线程阻塞一定的时间长度`duration` 类型,之后解除阻塞

[[C++#时间点 time point|chrono库中的时间点类 time_point 的使用]]
该函数的函数原型如下：
```c++
template <class Clock, class Duration>
  void sleep_until (const chrono::time_point<Clock,Duration>& abs_time);
```

示例程序如下：
```c++
#include <iostream>
#include <thread>
#include <chrono>
using namespace std;

void func()
{
    for (int i = 0; i < 10; ++i)
    {
        // 获取当前系统时间点
        auto now = chrono::system_clock::now();
        // 时间间隔为2s
        chrono::seconds sec(2);
        // 当前时间点之后休眠两秒
        this_thread::sleep_until(now + sec);
        cout << "子线程: " << this_thread::get_id() << ", i = " << i << endl;
    }
}

int main()
{
    thread t(func);
    t.join();
}
```

`sleep_until()`和`sleep_for()`函数的功能是一样的,只不过前者是基于时间点去阻塞线程,后者是基于时间段去阻塞线程,项目开发过程中根据实际情况选择最优的解决方案即可。

### yield()
命名空间`this_thread`中提供了一个非常绅士的函数`yield()`,在线程中调用这个函数之后,处于运行态的线程会主动让出自己已经抢到的CPU时间片,最终变为就绪态,这样其它的线程就有更大的概率能够抢到CPU时间片了。使用这个函数的时候需要注意一点,线程调用了`yield()`之后会主动放弃CPU资源,但是这个变为就绪态的线程会马上参与到下一轮CPU的抢夺战中,不排除它能继续抢到CPU时间片的情况,这是概率问题。

```c++
void yield() noexcept;
```

函数对应的示例程序如下：
```c++
#include <iostream>
#include <thread>
using namespace std;

void func()
{
    for (int i = 0; i < 100000000000; ++i)
    {
        cout << "子线程: " << this_thread::get_id() << ", i = " << i << endl;
        this_thread::yield();
    }
}

int main()
{
    thread t(func);
    thread t1(func);
    t.join();
    t1.join();
}
```

在上面的程序中,执行`func()`中的`for`循环会占用大量的时间,在极端情况下,如果当前线程占用CPU资源不释放就会导致其他线程中的任务无法被处理,或者该线程每次都能抢到CPU时间片,导致其他线程中的任务没有机会被执行。解决方案就是每执行一次循环,让该线程主动放弃CPU资源,重新和其他线程再次抢夺CPU时间片,如果其他线程抢到了CPU时间片就可以执行相应的任务了。

结论：
- `std::this_thread::yield()` 的目的是避免一个线程长时间占用CPU资源,从而导致多线程处理性能下降
- `std::this_thread::yield()` 是让当前线程主动放弃了当前自己抢到的CPU资源,但是在下一轮还会继续抢

## 线程同步

### 互斥锁

#### std::mutex
不论是在C还是C++中,进行线程同步的处理流程基本上是一致的,C++的`mutex`类提供了相关的API函数：
##### 成员函数
`lock()`函数用于<font color=#ff0000>给临界区加锁,并且只能有一个线程获得锁的所有权</font>,它有阻塞线程的作用,函数原型如下：

```c++
void lock();
```

独占互斥锁对象有两种状态：`锁定`和`未锁定`。如果互斥锁是打开的,调用`lock()`函数的线程会得到互斥锁的所有权,并将其上锁,其它线程再调用该函数的时候由于得不到互斥锁的所有权,就会被`lock()`函数阻塞。当拥有互斥锁所有权的线程将互斥锁解锁,此时被`lock()`阻塞的线程解除阻塞,抢到互斥锁所有权的线程加锁并继续运行,没抢到互斥锁所有权的线程继续阻塞。

除了使用lock()还可以使用try_lock()获取互斥锁的所有权并对互斥锁加锁,函数原型如下：

```c++
bool try_lock();
```

二者的区别在于`try_lock()`不会阻塞线程,`lock()`会阻塞线程：

- 如果互斥锁是未锁定状态,得到了互斥锁所有权并加锁成功,函数返回true
- 如果互斥锁是锁定状态,无法得到互斥锁所有权加锁失败,函数返回false
当互斥锁被锁定之后可以通过`unlock()`进行解锁,但是需要注意的是<font color=#ff0000>只有拥有互斥锁所有权的线程也就是对互斥锁上锁的线程才能将其解锁,其它线程是没有权限做这件事情的</font>。该函数的函数原型如下：
```c++
void unlock();
```

通过介绍以上三个函数,使用互斥锁进行线程同步的大致思路差不多就能搞清楚了,主要分为以下几步：

1. 找到多个线程操作的共享资源（全局变量、堆内存、类成员变量等）,也可以称之为临界资源
2. 找到和共享资源有关的上下文代码,也就是临界区（下图中的黄色代码部分）
3. 在临界区的上边调用互斥锁类的`lock()`方法
4. 在临界区的下边调用互斥锁的`unlock()`方法

<font color=#ff0000>线程同步的目的是让多线程按照顺序依次执行临界区代码,这样做线程对共享资源的访问就从并行访问变为了线性访问,访问效率降低了,但是保证了数据的正确性。</font>

<img src="https://www.subingwen.cn/cpp/mutex/image-20210410100224910.png" alt="image.png" style="zoom:30%;" />

> 当线程对互斥锁对象加锁,并且执行完临界区代码之后,一定要使用这个线程对互斥锁解锁,否则最终会造成线程的死锁。死锁之后当前应用程序中的所有线程都会被阻塞,并且阻塞无法解除,应用程序也无法继续运行。


##### 线程同步
举个栗子,我们让两个线程共同操作同一个全局变量,二者交替数数,将数值存储到这个全局变量里边并打印出来。

```c++
using namespace std;
int g_num = 0;  // 为 g_num_mutex 所保护
mutex g_num_mutex;
void slow_increment(int id)
{
    for (int i = 0; i < 3; ++i) 
    {
        g_num_mutex.lock();
        ++g_num;
        cout << id << " => " << g_num << endl;
        g_num_mutex.unlock();

        this_thread::sleep_for(chrono::seconds(1));
    }
}
int main()
{
    thread t1(slow_increment, 0);
    thread t2(slow_increment, 1);
    t1.join();
    t2.join();
}
```

在上面的示例程序中,两个子线程执行的任务的一样的（其实也可以不一样,不同的任务中也可以对共享资源进行读写操作）,在任务函数中把与全局变量相关的代码加了锁,两个线程只能顺序访问这部分代码（如果不进行线程同步打印出的数据是混乱且无序的）。另外需要强调一点：

1. <font color=#ff0000>在所有线程的任务函数执行完毕之前,互斥锁对象是不能被析构的,一定要在程序中保证这个对象的可用性</font>。
2. <font color=#ff0000>互斥锁的个数和共享资源的个数相等,也就是说每一个共享资源都应该对应一个互斥锁对象。互斥锁对象的个数和线程的个数没有关系</font>。

#### std::lock_guard

`lock_guard`是C++11新增的一个模板类,使用这个类,可以简化互斥锁`lock()`和`unlock()`的写法,同时也更安全。这个模板类的定义和常用的构造函数原型如下：

```c++
// 类的定义,定义于头文件 <mutex>
template< class Mutex >
class lock_guard;

// 常用构造函数
explicit lock_guard( mutex_type& m );
```

`lock_guard`在使用上面提供的这个构造函数构造对象时,会自动锁定互斥量,而在<font color=#ff0000>退出作用域</font>后进行析构时就会自动解锁,从而保证了互斥量的正确操作,避免忘记`unlock()`操作而导致线程死锁。lock_guard使用了**RAII技术**,就是在类构造函数中分配资源,在析构函数中释放资源,保证资源出了作用域就释放。

使用lock_guard对上面的例子进行修改,代码如下：

```c++
void slow_increment(int id){
  for (int i = 0; i < 3; ++i) {
    { // 添加作用域
      lock_guard<mutex> lock(g_num_mutex);
      ++g_num;
      cout << id << " ==>" << g_num << endl;
    }
    this_thread::sleep_for(chrono::seconds(1));
  }
}
```


通过修改发现代码被精简了,而且不用担心因为忘记解锁而造成程序的死锁,但是这种方式也有弊端,在上面的示例程序中整个for循环的体都被当做了临界区,多个线程是线性的执行临界区代码的,因此临界区越大程序效率越低,还是需要根据实际情况选择最优的解决方案。


> [!note] 注意：
> - `lock_guard`是根据智能指针`scoped_ptr`实现的，它不支持**拷贝构造、赋值构造、赋值重载**
> - `lock_guard`不可能用在函数参数传递或者返回过程中，只能用在简单的临界区代码段的互斥操作中 
> - 在函数调用的场景中应该使用`unique_lock`


#### std::scoped_lock类

C++17提供了新的RAII类模板`std::scoped_lock<>`。`std:: scoped_lock<>`和`std::lock_guard<>`完全等价，只不过前者是可变参数模板（variadic template），接收各种互斥型别作为模板参数列表，还以多个互斥对象作为构造函数的参数列表，它在作用域块的存在期间占有一或多个互斥。


#### std::unique_lock

- `std::unique_lock`是一种**更灵活**的互斥锁包装器，提供了更多的控制能力
- 它允许**延迟锁定**（即在构造时不获取锁）、**提前解锁**、**重新锁定**以及**转移锁**的所有权等操作
- `std::unique_lock` 的开销略高于 `std::lock_guard`，但提供了更大的灵活性
- 它可以与条件变量一起使用

```c++
std::mutex mtx;
{
    std::unique_lock<std::mutex> lock(mtx, std::defer_lock); // 延迟锁定
    // 在需要时进行锁定
    lock.lock();
    // 受保护的代码区域
    lock.unlock();
    // 在同一作用域中可能再次锁定
    lock.lock();
}
```



> [!summary] std::unique_lock 的使用场景
> - 当需要在作用域内对锁进行**更复杂的控制**时（例如，在作用域内部临时解锁），`std::unique_lock` 是更合适的选择。
> - 在使用条件变量时，`std::unique_lock` 能够在 wait 调用期间自动解锁并在 wait 返回后重新锁定，这是处理条件变量时必需的。



#### std::recursive_mutex
递归互斥锁`std::recursive_mutex`允许同一线程多次获得互斥锁,可以用来解决同一线程需要多次获取互斥量时死锁的问题,在下面的例子中使用独占非递归互斥量会发生死锁：

```c++
#include <iostream>
#include <thread>
#include <mutex>
using namespace std;
struct Calculate
{
    Calculate() : m_i(6) {}

    void mul(int x)
    {
        lock_guard<mutex> locker(m_mutex);
        m_i *= x;
    }

    void div(int x)
    {
        lock_guard<mutex> locker(m_mutex);
        m_i /= x;
    }

    void both(int x, int y)
    {
        lock_guard<mutex> locker(m_mutex);
        mul(x);
        div(y);
    }

    int m_i;
    mutex m_mutex;
};

int main()
{
    Calculate cal;
    cal.both(6, 3);
    return 0;
}
```

上面的程序中执行了`cal.both(6, 3)`;调用之后,程序就会发生死锁,在`both()`中已经对互斥锁加锁了,继续调用`mult()`函数,已经得到互斥锁所有权的线程再次获取这个互斥锁的所有权就会造成死锁（在C++中程序会异常退出,使用C库函数会导致这个互斥锁永远无法被解锁,最终阻塞所有的线程）。要解决这个死锁的问题,一个简单的办法就是使用递归互斥锁`std::recursive_mutex`,它允许一个线程多次获得互斥锁的所有权。修改之后的代码如下：

```c++
#include <iostream>
#include <thread>
#include <mutex>
using namespace std;
struct Calculate
{
    Calculate() : m_i(6) {}
    void mul(int x)
    {
        lock_guard<recursive_mutex> locker(m_mutex);
        m_i *= x;
    }
    void div(int x)
    {
        lock_guard<recursive_mutex> locker(m_mutex);
        m_i /= x;
    }
    void both(int x, int y)
    {
        lock_guard<recursive_mutex> locker(m_mutex);
        mul(x);
        div(y);
    }
    int m_i;
    recursive_mutex m_mutex;
};
int main()
{
    Calculate cal;
    cal.both(6, 3);
    cout << "cal.m_i = " << cal.m_i << endl;
    return 0;
}
```


虽然递归互斥锁可以解决同一个互斥锁频繁获取互斥锁资源的问题,但是还是建议少用,主要原因如下：

1. 使用递归互斥锁的场景往往都是可以简化的,使用递归互斥锁很容易放纵复杂逻辑的产生,从而导致bug的产生
2. 递归互斥锁比非递归互斥锁效率要低一些。
3. 递归互斥锁虽然允许同一个线程多次获得同一个互斥锁的所有权,但最大次数并未具体说明,一旦超过一定的次数,就会抛出std::system错误。

`std::recursive_mutex` 的底层实现原理
1. **底层互斥锁**： 使用一个普通的互斥锁（通常是 `std::mutex` 或等效的操作系统原语）来实现基本的锁定和解锁功能。
2. **计数器**： 一个计数器，用于记录当前线程锁定互斥锁的次数。
3. **线程ID**： 记录当前锁定互斥锁的线程ID，以确保只有持有锁的线程能够递归地锁定和解锁。

#### std::timed_mutex
`std::timed_mutex`是超时独占互斥锁,主要是在获取互斥锁资源时增加了超时等待功能,因为不知道获取锁资源需要等待多长时间,为了保证不一直等待下去,设置了一个超时时长,超时后线程就可以解除阻塞去做其他事情了。

`std::timed_mutex比std::_mutex`多了两个成员函数：`try_lock_for()`和`try_lock_until()`：

```c++
void lock();
bool try_lock();
void unlock();

// std::timed_mutex比std::_mutex多出的两个成员函数
template <class Rep, class Period>
  bool try_lock_for (const chrono::duration<Rep,Period>& rel_time);

template <class Clock, class Duration>
  bool try_lock_until (const chrono::time_point<Clock,Duration>& abs_time);
```

- `try_lock_for`函数是当线程获取不到互斥锁资源的时候,让线程阻塞一定的时间长度
- `try_lock_until`函数是当线程获取不到互斥锁资源的时候,让线程阻塞到某一个指定的时间点
- 关于两个函数的返回值：当得到互斥锁的所有权之后,函数会马上解除阻塞,返回true,如果阻塞的时长用完或者到达指定的时间点之后,函数也会解除阻塞,返回false

下面的示例程序中为大家演示了`std::timed_mutex`的使用：

```c++
#include <iostream>
#include <thread>
#include <mutex>
using namespace std;

timed_mutex g_mutex;

void work()
{
    chrono::seconds timeout(1);
    while (true)
    {
        // 通过阻塞一定的时长来争取得到互斥锁所有权
        if (g_mutex.try_lock_for(timeout))
        {
            cout << "当前线程ID: " << this_thread::get_id() 
                << ", 得到互斥锁所有权..." << endl;
            // 模拟处理任务用了一定的时长
            this_thread::sleep_for(chrono::seconds(10));
            // 互斥锁解锁
            g_mutex.unlock();
            break;
        }
        else
        {
            cout << "当前线程ID: " << this_thread::get_id() 
                << ", 没有得到互斥锁所有权..." << endl;
            // 模拟处理其他任务用了一定的时长
            this_thread::sleep_for(chrono::milliseconds(50));
        }
    }
}

int main()
{
    thread t1(work);
    thread t2(work);

    t1.join();
    t2.join();

    return 0;
}
```

示例代码输出的结果：

```c++
当前线程ID: 125776, 得到互斥锁所有权...
当前线程ID: 112324, 没有得到互斥锁所有权...
当前线程ID: 112324, 没有得到互斥锁所有权...
当前线程ID: 112324, 没有得到互斥锁所有权...
当前线程ID: 112324, 没有得到互斥锁所有权...
当前线程ID: 112324, 没有得到互斥锁所有权...
当前线程ID: 112324, 没有得到互斥锁所有权...
当前线程ID: 112324, 没有得到互斥锁所有权...
当前线程ID: 112324, 没有得到互斥锁所有权...
当前线程ID: 112324, 没有得到互斥锁所有权...
当前线程ID: 112324, 得到互斥锁所有权...
```

在上面的例子中,通过一个`while`循环不停的去获取超时互斥锁的所有权,如果得不到就阻塞1秒钟,1秒之后如果还是得不到阻塞50毫秒,然后再次继续尝试,直到获得互斥锁的所有权,跳出循环体。

关于递归超时互斥锁`std::recursive_timed_mutex`的使用方式和`std::timed_mutex`是一样的,只不过它可以允许一个线程多次获得互斥锁所有权,而`std::timed_mutex`只允许线程获取一次互斥锁所有权。另外,递归超时互斥锁`std::recursive_timed_mutex`也拥有和`std::recursive_mutex`一样的弊端,不建议频繁使用。

### 共享锁

`shared_mutex` 是 C++17 标准库中引入的一种同步原语，用于多线程编程环境下的读写锁。它允许多个线程同时读取共享数据，而写线程则会独占访问权，从而避免写操作和读操作之间的竞争条件。`shared_mutex` 提供了两种锁模式：共享锁（读锁）和独占锁（写锁）。

**主要功能**

- **共享锁（Shared Lock）**：允许多个线程同时持有锁，用于读取操作。
- **独占锁（Exclusive Lock）**：只允许一个线程持有锁，用于写入操作。

关键类和函数

- **`std::shared_mutex`**：定义了多读单写的互斥锁。
- **`std::shared_lock`**：用于获取共享锁，多个线程可以同时持有。
- **`std::unique_lock`**：用于获取独占锁，在同一时刻只允许一个线程持有。

**使用示例**

以下是一个简单的示例，展示如何使用 `shared_mutex` 来实现线程安全的读写操作：

```c++
#include <iostream>
#include <thread>
#include <shared_mutex>
#include <vector>

class SharedResource {
public:
    void read() const {
        std::shared_lock<std::shared_mutex> lock(mutex_);
        std::cout << "Reading: " << data_ << std::endl;
    }

    void write(int value) {
        std::unique_lock<std::shared_mutex> lock(mutex_);
        data_ = value;
        std::cout << "Writing: " << data_ << std::endl;
    }

private:
    mutable std::shared_mutex mutex_;
    int data_ = 0;
};

void reader(const SharedResource& resource) {
    for (int i = 0; i < 5; ++i) {
        resource.read();
        std::this_thread::sleep_for(std::chrono::milliseconds(100));
    }
}

void writer(SharedResource& resource, int value) {
    for (int i = 0; i < 5; ++i) {
        resource.write(value + i);
        std::this_thread::sleep_for(std::chrono::milliseconds(150));
    }
}

int main() {
    SharedResource resource;
    std::vector<std::thread> threads;

    // 创建多个读线程
    for (int i = 0; i < 3; ++i) {
        threads.emplace_back(reader, std::cref(resource));
    }

    // 创建一个写线程
    threads.emplace_back(writer, std::ref(resource), 10);

    // 等待所有线程完成
    for (auto& thread : threads) {
        thread.join();
    }

    return 0;
}
```

**适用场景**

`shared_mutex` 适用于读多写少的场景，例如：

- 缓存系统
- 配置文件读取
- 统计信息收集

**注意事项**

- **线程安全**：不要在持有共享锁的情况下进行写操作，这会导致未定义行为和数据竞争。
- **不可递归**：`shared_mutex` 不能递归锁定，即一个线程不能多次持有同一个 `shared_mutex` 的锁。
- **性能问题**：在某些情况下，读写锁可能会导致写操作饥饿，具体取决于实现和使用模式。

`mutable` 关键字

在使用 `shared_mutex` 时，经常会看到 `mutable` 关键字。它用于允许在 `const` 成员函数中修改锁或其他成员变量。

```c++
class Example {
public:
    void read() const {
        std::shared_lock<std::shared_mutex> lock(mutex_);
        std::cout << "Reading: " << data_ << std::endl;
    }

    void write(int value) {
        std::unique_lock<std::shared_mutex> lock(mutex_);
        data_ = value;
        std::cout << "Writing: " << data_ << std::endl;
    }

private:
    mutable std::shared_mutex mutex_;
    int data_ = 0;
};

```

**总结**
`shared_mutex` 是一种强大的工具，用于提高多线程程序的并发性能，特别是在读多写少的场景中。使用 `shared_mutex` 时要确保在读操作中不进行写操作，并注意不可递归锁定的问题。通过合理使用 `shared_mutex` 和 `mutable` 关键字，可以编写高效且线程安全的多线程程序。
### 条件变量

条件变量是C++11提供的另外一种用于等待的同步机制,它能阻塞一个或多个线程,直到收到另外一个线程发出的通知或者超时时,才会唤醒当前阻塞的线程。条件变量需要和互斥量配合起来使用,C++11提供了两种条件变量：

- `condition_variable`：需要配合`std::unique_lock<std::mutex>`进行wait操作,也就是阻塞线程的操作。
- `condition_variable_any`：可以和任意带有`lock()`、`unlock()`语义的mutex搭配使用,也就是说有四种：
	- `std::mutex`：独占的非递归互斥锁
	- `std::timed_mutex`：带超时的独占非递归互斥锁
	- `std::recursive_mutex`：不带超时功能的递归互斥锁
	- `std::recursive_timed_mutex`：带超时的递归互斥锁

`std::unique_lock` 在其作用域结束时会自动解锁互斥体（mutex）,与 `std::lock_guard` 的行为类似,主要和条件变量一起使用

条件变量通常用于生产者和消费者模型,大致使用过程如下：

1. 拥有条件变量的线程获取互斥量
2. 循环检查某个条件,如果条件不满足阻塞当前线程,否则线程继续向下执行
	- 产品的数量达到上限,生产者阻塞,否则生产者一直生产。。。
	- 产品的数量为零,消费者阻塞,否则消费者一直消费。。。
3. 条件满足之后,可以调用`notify_one()`或者`notify_all()`唤醒一个或者所有被阻塞的线程
	- 由消费者唤醒被阻塞的生产者,生产者解除阻塞继续生产。。。
	- 由生产者唤醒被阻塞的消费者,消费者解除阻塞继续消费。

#### condition_variable
##### 成员函数
`condition_variable`的成员函数主要分为两部分：线程等待（阻塞）函数 和线程通知（唤醒）函数,这些函数被定义于头文件 `<condition_variable>`。

- 等待函数
调用wait()函数的线程会被阻塞
```c++
// ①
void wait (unique_lock<mutex>& lck);
// ②
template <class Predicate>
void wait (unique_lock<mutex>& lck, Predicate pred);
```

- 函数①：调用该函数的线程直接被阻塞
- 函数②：该函数的第二个参数是一个判断条件,是一个返回值为布尔类型的**函数** (可以替代一般条件变量的通知)
	- 该参数可以传递一个有名函数的地址,也可以直接指定一个匿名函数
	- 表达式返回false当前线程被阻塞,表达式返回true当前线程不会被阻塞,继续向下执行
- 独占的互斥锁对象不能直接传递给wait()函数,需要通过模板类unique_lock进行二次处理,通过得到的对象仍然可以对独占的互斥锁对象做如下操作,使用起来更灵活。


|     公共成员函数     |                   说明                    |
| :------------: | :-------------------------------------: |
|      lock      |                锁定关联的互斥锁                 |
|    try_lock    |         尝试锁定关联的互斥锁,若无法锁定,函数直接返回         |
|  try_lock_for  |  试图锁定关联的可定时锁定互斥锁,若互斥锁在给定时长中仍不能被锁定,函数返回  |
| try_lock_until | 试图锁定关联的可定时锁定互斥锁,若互斥锁在给定的时间点后仍不能被锁定,函数返回 |
|     unlock     |                 将互斥锁解锁                  |

<font color=#ff0000>如果线程被该函数阻塞,这个线程会释放占有的互斥锁的所有权,当阻塞解除之后这个线程会重新得到互斥锁的所有权,继续向下执行</font>（这个过程是在函数内部完成的,了解这个过程即可,其目的是为了避免线程的死锁）。

`wait_for()`函数和`wait()`的功能是一样的,只不过多了一个阻塞时长,假设阻塞的线程没有被其他线程唤醒,当阻塞时长用完之后,线程就会自动解除阻塞,继续向下执行。

```c++
template <class Rep, class Period>
cv_status wait_for (unique_lock<mutex>& lck,
                    const chrono::duration<Rep,Period>& rel_time);
	
template <class Rep, class Period, class Predicate>
bool wait_for(unique_lock<mutex>& lck,
               const chrono::duration<Rep,Period>& rel_time, Predicate pred);
```

`wait_until()`函数和`wait_for()`的功能是一样的,它是指定让线程阻塞到某一个时间点,假设阻塞的线程没有被其他线程唤醒,当到达指定的时间点之后,线程就会自动解除阻塞,继续向下执行。
```c++
template <class Clock, class Duration>
cv_status wait_until (unique_lock<mutex>& lck,
                      const chrono::time_point<Clock,Duration>& abs_time);

template <class Clock, class Duration, class Predicate>
bool wait_until (unique_lock<mutex>& lck,
                 const chrono::time_point<Clock,Duration>& abs_time, Predicate pred);
```

- 通知函数
```c++
void notify_one() noexcept;
void notify_all() noexcept;
```

- `notify_one()`：唤醒一个被当前条件变量阻塞的线程
- `notify_all()`：唤醒全部被当前条件变量阻塞的线程

##### 生产者和消费者模型
我们可以使用条件变量来实现一个同步队列,这个队列作为生产者线程和消费者线程的共享资源,示例代码如下

```c++
#include <iostream>
#include <thread>
#include <mutex>
#include <list>
#include <functional>
#include <condition_variable>
using namespace std;

class SyncQueue
{
public:
    SyncQueue(int maxSize) : m_maxSize(maxSize) {}
    
    void put(const int& x)
    {
        unique_lock<mutex> locker(m_mutex);
        // 判断任务队列是不是已经满了
        while (m_queue.size() == m_maxSize)
        {
            cout << "任务队列已满, 请耐心等待..." << endl;
            // 阻塞线程
            m_notFull.wait(locker);
        }
        // 将任务放入到任务队列中
        m_queue.push_back(x);
        cout << x << " 被生产" << endl; 
        // 通知消费者去消费
        m_notEmpty.notify_one();
    }

    int take()
    {
        unique_lock<mutex> locker(m_mutex);
        while (m_queue.empty())
        {
            cout << "任务队列已空,请耐心等待。。。" << endl;
            m_notEmpty.wait(locker);
        }
        // 从任务队列中取出任务(消费)
        int x = m_queue.front();
        m_queue.pop_front();
        // 通知生产者去生产
        m_notFull.notify_one();
        cout << x << " 被消费" << endl;
        return x;
    }

    bool empty()
    {
        lock_guard<mutex> locker(m_mutex);
        return m_queue.empty();
    }

    bool full()
    {
        lock_guard<mutex> locker(m_mutex);
        return m_queue.size() == m_maxSize;
    }

    int size()
    {
        lock_guard<mutex> locker(m_mutex);
        return m_queue.size();
    }

private:
    list<int> m_queue;     // 存储队列数据
    mutex m_mutex;         // 互斥锁
    condition_variable m_notEmpty;   // 不为空的条件变量
    condition_variable m_notFull;    // 没有满的条件变量
    int m_maxSize;         // 任务队列的最大任务个数
};

int main()
{
    SyncQueue taskQ(50);
    auto produce = bind(&SyncQueue::put, &taskQ, placeholders::_1);
    auto consume = bind(&SyncQueue::take, &taskQ);
    thread t1[3];
    thread t2[3];
    for (int i = 0; i < 3; ++i)
    {
        t1[i] = thread(produce, i+100);
        t2[i] = thread(consume);
    }

    for (int i = 0; i < 3; ++i)
    {
        t1[i].join();
        t2[i].join();
    }
    return 0;
}
```

条件变量`condition_variable`类的`wait()`还有一个重载的方法,可以接受一个条件,这个条件也可以是一个返回值为布尔类型的函数,条件变量会先检查判断这个条件是否满足,如果满足条件（布尔值为true）,则当前线程重新获得互斥锁的所有权,结束阻塞,继续向下执行；如果不满足条件（布尔值为false）,当前线程会释放互斥锁（解锁）同时被阻塞,等待被唤醒。

上面示例程序中的`put()`、`take()`函数可以做如下修改：

- put()函数
```c++
	void put(const int& x)
	{
	    unique_lock<mutex> locker(m_mutex);
	    // 根据条件阻塞线程
	    m_notFull.wait(locker, [this]() {
	        return m_queue.size() != m_maxSize;
	    });
	    // 将任务放入到任务队列中
	    m_queue.push_back(x);
	    cout << x << " 被生产" << endl;
	    // 通知消费者去消费
	    m_notEmpty.notify_one();
	}
	```

- take()函数
	```c++
	int take()
	{
	    unique_lock<mutex> locker(m_mutex);
	    m_notEmpty.wait(locker, [this]() {
	        return !m_queue.empty();
	    });
	    // 从任务队列中取出任务(消费)
	    int x = m_queue.front();
	    m_queue.pop_front();
	    // 通知生产者去生产
	    m_notFull.notify_one();
	    cout << x << " 被消费" << endl;
	    return x;
	}
	```

<font color=#ff0000>修改之后可以发现,程序变得更加精简了,而且执行效率更高了</font>,因为在这两个函数中的`while`循环被删掉了,但是最终的效果是一样的,推荐使用这种方式的`wait()`进行线程的阻塞。

#### condition_variable_any
##### 成员函数
`condition_variable_any`的成员函数也是分为两部分：线程等待（阻塞）函数 和线程通知（唤醒）函数,这些函数被定义于头文件 `<condition_variable>`。

- 等待函数
	```c++
	// ①
	template <class Lock> void wait (Lock& lck);
	// ②
	template <class Lock, class Predicate>
	void wait (Lock& lck, Predicate pred);
	```

- 函数①：调用该函数的线程直接被阻塞
- 函数②：该函数的第二个参数是一个判断条件,是一个返回值为布尔类型的函数
	- `该参数可以传递一个有名函数的地址,也可以直接指定一个匿名函数`
	- `表达式返回false当前线程被阻塞,表达式返回true当前线程不会被阻塞,继续向下执行`
- 可以直接传递给`wait()`函数的互斥锁类型有四种,分别是：
	- `std::mutex、std::timed_mutex、std::recursive_mutex、std::recursive_timed_mutex`
- `如果线程被该函数阻塞,这个线程会释放占有的互斥锁的所有权,当阻塞解除之后这个线程会重新得到互斥锁的所有权,继续向下执行`（这个过程是在函数内部完成的,了解这个过程即可,其目的是为了避免线程的死锁）。
`wait_for()`函数和`wait()`的功能是一样的,只不过多了一个阻塞时长,假设阻塞的线程没有被其他线程唤醒,当阻塞时长用完之后,线程就会自动解除阻塞,继续向下执行。
```c++
template <class Lock, class Rep, class Period>
cv_status wait_for (Lock& lck, const chrono::duration<Rep,Period>& rel_time);
	
template <class Lock, class Rep, class Period, class Predicate>
bool wait_for (Lock& lck, const chrono::duration<Rep,Period>& rel_time, Predicate pred);
```

`wait_until()`函数和`wait_for()`的功能是一样的,它是指定让线程阻塞到某一个时间点,假设阻塞的线程没有被其他线程唤醒,当到达指定的时间点之后,线程就会自动解除阻塞,继续向下执行。
```c++
template <class Lock, class Clock, class Duration>
cv_status wait_until (Lock& lck, const chrono::time_point<Clock,Duration>& abs_time);

template <class Lock, class Clock, class Duration, class Predicate>
bool wait_until (Lock& lck, 
                 const chrono::time_point<Clock,Duration>& abs_time, 
                 Predicate pred);
```

通知函数
```c++
void notify_one() noexcept;
void notify_all() noexcept;
```

- `notify_one()`：唤醒一个被当前条件变量阻塞的线程
- `notify_all()`：唤醒全部被当前条件变量阻塞的线程

##### 生产者和消费者模型
使用条件变量`condition_variable_any`同样可以实现上面的生产者和消费者的例子,代码只有个别细节上有所不同：

```c++
#include <iostream>
#include <thread>
#include <mutex>
#include <list>
#include <functional>
#include <condition_variable>
using namespace std;

class SyncQueue
{
public:
    SyncQueue(int maxSize) : m_maxSize(maxSize) {}

    void put(const int& x)
    {
        lock_guard<mutex> locker(m_mutex);
        // 根据条件阻塞线程
        m_notFull.wait(m_mutex, [this]() {
            return m_queue.size() != m_maxSize;
        });
        // 将任务放入到任务队列中
        m_queue.push_back(x);
        cout << x << " 被生产" << endl;
        // 通知消费者去消费
        m_notEmpty.notify_one();
    }

    int take()
    {
        lock_guard<mutex> locker(m_mutex);
        m_notEmpty.wait(m_mutex, [this]() {
            return !m_queue.empty();
        });
        // 从任务队列中取出任务(消费)
        int x = m_queue.front();
        m_queue.pop_front();
        // 通知生产者去生产
        m_notFull.notify_one();
        cout << x << " 被消费" << endl;
        return x;
    }

    bool empty()
    {
        lock_guard<mutex> locker(m_mutex);
        return m_queue.empty();
    }

    bool full()
    {
        lock_guard<mutex> locker(m_mutex);
        return m_queue.size() == m_maxSize;
    }

    int size()
    {
        lock_guard<mutex> locker(m_mutex);
        return m_queue.size();
    }

private:
    list<int> m_queue;     // 存储队列数据
    mutex m_mutex;         // 互斥锁
    condition_variable_any m_notEmpty;   // 不为空的条件变量
    condition_variable_any m_notFull;    // 没有满的条件变量
    int m_maxSize;         // 任务队列的最大任务个数
};

int main()
{
    SyncQueue taskQ(50);
    auto produce = bind(&SyncQueue::put, &taskQ, placeholders::_1);
    auto consume = bind(&SyncQueue::take, &taskQ);
    thread t1[3];
    thread t2[3];
    for (int i = 0; i < 3; ++i)
    {
        t1[i] = thread(produce, i + 100);
        t2[i] = thread(consume);
    }

    for (int i = 0; i < 3; ++i)
    {
        t1[i].join();
        t2[i].join();
    }

    return 0;
}
```

总结：以上介绍的两种条件变量各自有各自的特点,`condition_variable` 配合 `unique_lock` 使用更灵活一些,可以在在任何时候自由地释放互斥锁,而`condition_variable_any` 如果和`lock_guard` 一起使用必须要等到其生命周期结束才能将互斥锁释放。但是,`condition_variable_any` 可以和多种互斥锁配合使用,应用场景也更广,而 `condition_variable` 只能和独占的非递归互斥锁（mutex）配合使用,有一定的局限性。·

### 原子变量

C++11提供了一个原子类型`std::atomic<T>`,通过这个原子类型管理的内部变量就可以称之为原子变量,我们可以给原子类型指定bool、char、int、long、指针等类型作为模板参数（不支持浮点类型和复合类型）。

原子指的是**一系列不可被CPU上下文交换的机器指令**,这些指令组合在一起就形成了原子操作。在多核CPU下,当某个CPU核心开始运行原子操作时,会**先暂停其它CPU内核对内存的操作**,以保证原子操作不会被其它CPU内核所干扰。

由于原子操作是通过指令提供的支持,因此它的性能相比锁和消息传递会好很多。相比较于锁而言,原子类型不需要开发者处理加锁和释放锁的问题,同时支持修改,读取等操作,还具备较高的并发性能,几乎所有的语言都支持原子类型。

可以看出原子类型是无锁类型,但是无锁不代表无需等待,因为原子类型内部使用了**CAS循环**,当大量的冲突发生时,该等待还是得等待！但是总归比锁要好。

**CAS操作的工作原理**

CAS操作涉及三个参数：
1. **内存地址V**：需要修改的变量。
2. **旧的预期值A**：预期变量的旧值。
3. **新的值B**：希望将变量更新为的新值。

CAS操作执行以下步骤：
1. **比较**：检查内存地址V处的当前值是否等于预期值A。
2. **交换**：如果内存地址V处的当前值等于预期值A，则将V更新为新值B；否则不执行更新。

CAS操作的伪代码如下：
```c++
bool CAS(int* addr, int expected, int newValue) {
    if (*addr == expected) {
        *addr = newValue;
        return true;
    }
    return false;
}
```

实际示例：C++中的原子操作
在C++中，标准库提供了一些原子类型和操作，如`std::atomic`。这些原子类型和操作通常使用CAS指令来实现无锁同步。

示例代码：使用`std::atomic`实现原子加法
```c++
#include <iostream>
#include <atomic>
#include <thread>
#include <vector>

std::atomic<int> counter(0);

void increment() {
    for (int i = 0; i < 1000; ++i) {
        counter.fetch_add(1, std::memory_order_relaxed); // 原子加法操作
    }
}

int main() {
    std::vector<std::thread> threads;
    for (int i = 0; i < 10; ++i) {
        threads.push_back(std::thread(increment));
    }
    for (auto& th : threads) {
        th.join();
    }
    std::cout << "Final counter value: " << counter << std::endl;
    return 0;
}

```


> CAS全称是Compare and swap, 它通过一条指令读取指定的内存地址,然后判断其中的值是否等于给定的前置值,如果相等,则将其修改为新的值

C++11内置了整形的原子变量,这样就可以更方便的使用原子变量了。在多线程操作中,使用原子变量之后就不需要再使用互斥量来保护该变量了,用起来更简洁。因为对原子变量进行的操作只能是一个**原子操作**（atomic operation）,原子操作指的是不会被线程调度机制打断的操作,这种操作一旦开始,就一直运行到结束,**中间不会有任何的上下文切换**。多线程同时访问共享资源造成数据混乱的原因就是因为CPU的上下文切换导致的,使用原子变量解决了这个问题,因此互斥锁的使用也就不再需要了。


#### atomic 类成员
**类定义**
```c++
// 定义于头文件 <atomic>
template< class T >
struct atomic;
```

通过定义可得知：`在使用这个模板类的时候,一定要指定模板类型`。
##### 构造函数
```c++
// ①
atomic() noexcept = default;
// ②
constexpr atomic( T desired ) noexcept;
// ③
atomic( const atomic& ) = delete;
```

- 构造函数①：默认无参构造函数。
- 构造函数②：使用 desired 初始化原子变量的值。
- 构造函数③：使用=delete显示删除拷贝构造函数, 不允许进行对象之间的拷贝

##### 公共成员函数
原子类型在类内部重载了=操作符,并且不允许在类的外部使用 =进行对象的拷贝

```c++
T operator=( T desired ) noexcept;
T operator=( T desired ) volatile noexcept;

atomic& operator=( const atomic& ) = delete;
atomic& operator=( const atomic& ) volatile = delete;
```

原子地以 desired 替换当前值。按照 order 的值影响内存。
```c++
void store( T desired, std::memory_order order = std::memory_order_seq_cst ) noexcept;
void store( T desired, std::memory_order order = std::memory_order_seq_cst ) volatile noexcept;
```

- `desired`：存储到原子变量中的值
- `order`：强制的内存顺序

原子地加载并返回原子变量的当前值。按照order的值影响内存。直接访问原子对象也可以得到原子变量的当前值。
```c++
T load( std::memory_order order = std::memory_order_seq_cst ) const noexcept;
T load( std::memory_order order = std::memory_order_seq_cst ) const volatile noexcept;
```

##### 特化成员函数
复合赋值运算符重载,主要包含以下形式：

<img src="https://article.biliimg.com/bfs/article/d26e2eef539e36c37eed4c8960a153e938716159.png" alt="image.png" style="zoom:50%;" />

- 以上各个 operator 都会有对应的 `fetch_*` 操作,详细见下表：

<img src="https://article.biliimg.com/bfs/article/813869ac495a9fd9f54ce9aabf9b56c738716159.png" alt="image.png" style="zoom:50%;" />

##### 内存顺序约束
通过上面的 API 函数我们可以看出,在调用 atomic类提供的 API 函数的时候,需要指定**原子顺序**,在C++11给我们提供的 API 中使用枚举用作执行原子操作的函数的实参,以指定如何同步不同线程上的其他操作。

这里说的顺序是指令级别的并行性和现代处理器的顺序
现代的多核CPU为了提高性能,可能会对指令进行乱序执行。这意味着即使某些指令在代码中先于其他指令,它们可能会在实际执行时被重新排序。此外,CPU的各个核心之间的内存访问也可能不是按照程序员预期的顺序发生。

```c++
x = 1;
y = 2;
```

乍一看,您可能会认为 x=1 总是在 y=2 之前执行。然而,由于多种原因（例如乱序执行、CPU缓存等）,在多线程环境中,其他线程可能会首先看到 y 的改变,然后才看到 x 的改变。

定义如下:

```c++
typedef enum memory_order {
    memory_order_relaxed,   // relaxed
    memory_order_consume,   // consume
    memory_order_acquire,   // acquire
    memory_order_release,   // release
    memory_order_acq_rel,   // acquire/release
    memory_order_seq_cst    // sequentially consistent
} memory_order;
```

- `memory_order_relaxed`, 这是最宽松的规则,它对编译器和CPU不做任何限制,可以乱序
- `memory_order_release` 释放,设定内存屏障(Memory barrier),保证它之前的操作永远在它之前,但是它后面的操作可能被重排到它前面
- `memory_order_acquire` 获取, 设定内存屏障,保证在它之后的访问永远在它之后,但是它之前的操作却有可能被重排到它后面,往往和Release在不同线程中联合使用
- `memory_order_consume`：改进版的`memory_order_acquire` ,开销更小
- `memory_order_acq_rel`,它是Acquire 和 Release 的结合,同时拥有它们俩提供的保证。比如你要对一个 atomic 自增 1,同时希望该操作之前和之后的读取或写入操作不会被重新排序
- `memory_order_seq_cst` 顺序一致性, `memory_order_seq_cst` 就像是`memory_order_acq_rel`的加强版,它不管原子操作是属于读取还是写入的操作,只要某个线程有用到`memory_order_seq_cst` 的原子操作,线程中该`memory_order_seq_cst` 操作前的数据操作绝对不会被重新排在该`memory_order_seq_cst` 操作之后,且该`memory_order_seq_cst` 操作后的数据操作也绝对不会被重新排在`memory_order_seq_cst` 操作前。

##### C++20新增成员
在C++20版本中添加了新的功能函数,可以通过原子类型来阻塞线程,和条件变量中的等待/通知函数是一样的。


|      公共成员函数       |          说明           |
| :---------------: | :-------------------: |
|    wait(C++20)    |    阻塞线程直至被提醒且原子值更改    |
| notify_one(C++20) | 通知（唤醒）至少一个在原子对象上阻塞的线程 |
| notify_all(C++20) |  通知（唤醒）所有在原子对象上阻塞的线程  |


#### 原子变量的使用
假设我们要制作一个多线程交替数数的计数器,我们使用互斥锁和原子变量的方式分别进行实现,对比一下二者的差异：
##### 互斥锁版本

```c++
#include <iostream>
#include <thread>
#include <mutex>
#include <atomic>
#include <functional>
using namespace std;

struct Counter
{
    void increment()
    {
        for (int i = 0; i < 10; ++i)
        {
            lock_guard<mutex> locker(m_mutex);
            m_value++;
            cout << "increment number: " << m_value 
                << ", theadID: " << this_thread::get_id() << endl;
            this_thread::sleep_for(chrono::milliseconds(100));
        }
    }

    void decrement()
    {
        for (int i = 0; i < 10; ++i)
        {
            lock_guard<mutex> locker(m_mutex);
            m_value--;
            cout << "decrement number: " << m_value 
                << ", theadID: " << this_thread::get_id() << endl;
            this_thread::sleep_for(chrono::milliseconds(100));
        }
    }

    int m_value = 0;
    mutex m_mutex;
};

int main()
{
    Counter c;
    auto increment = bind(&Counter::increment, &c);
    auto decrement = bind(&Counter::decrement, &c);
    thread t1(increment);
    thread t2(decrement);

    t1.join();
    t2.join();

    return 0;
}
```

##### 原子变量版本

```c++
struct Counter
{
    void increment()
    {
      for (int i = 0; i < 10; ++i)
      {
        m_value.fetch_add(1, std::memory_order_relaxed); // increment atomically
        cout << "increment number: " << m_value.load(std::memory_order_relaxed)
             << ", theadID: " << this_thread::get_id() << endl;

        this_thread::sleep_for(chrono::milliseconds(100));
      }
    }

    void decrement()
    {
      for (int i = 0; i < 10; ++i)
      {
        m_value.fetch_sub(1, std::memory_order_relaxed); // decrement atomically
        cout << "decrement number: " << m_value.load(std::memory_order_relaxed)
             << ", theadID: " << this_thread::get_id() << endl;

        this_thread::sleep_for(chrono::milliseconds(100));
      }
    }

    atomic<int> m_value = 0;
};
int main()
{
  Counter c;
  auto increment = bind(&Counter::increment, &c);
  auto decrement = bind(&Counter::decrement, &c);
  thread t1(increment);
  thread t2(decrement);
  t1.join();
  t2.join();
  return 0;
}
```

通过代码的对比可以看出,使用了原子变量之后,就不需要再定义互斥量了,在使用上更加简便,并且这两种方式都能保证在多线程操作过程中数据的正确性,不会出现数据的混乱。

原子类型`atomic<T> `可以封装原始数据最终得到一个原子变量对象,操作原子对象能够得到和操作原始数据一样的效果,当然也可以通过`store()`和`load()`来读写原子对象内部的原始数据。

### call_once

在某些特定情况下,某些函数只能在多线程环境下调用一次,比如：要初始化某个对象,而这个对象只能被初始化一次,就可以使用`std::call_once()`来保证函数在多线程环境下只能被调用一次。使用`call_once()`的时候,需要一个`once_flag`作为`call_once()`的传入参数,该函数的原型如下：

```c++
// 定义于头文件 <mutex>
template< class Callable, class... Args >
void call_once( std::once_flag& flag, Callable&& f, Args&&... args );
```

- flag：`once_flag`类型的对象,要保证这个对象能够被多个线程同时访问到
- f：回调函数,可以传递一个有名函数地址,也可以指定一个匿名函数
- args：作为实参传递给回调函数

多线程操作过程中,`std::call_once()`内部的回调函数只会被执行一次,示例代码如下：
```c++
#include <iostream>
#include <thread>
#include <mutex>
using namespace std;

once_flag g_flag;
void do_once(int a, string b)
{
    cout << "name: " << b << ", age: " << a << endl;
}
void do_something(int age, string name)
{
    static int num = 1;
    call_once(g_flag, do_once, 19, "luffy");
    cout << "do_something() function num = " << num++ << endl;
}
int main()
{
    thread t1(do_something, 20, "ace");
    thread t2(do_something, 20, "sabo");
    thread t3(do_something, 19, "luffy");
    t1.join();
    t2.join();
    t3.join();
    return 0;
}
```


示例程序输出的结果：
```c++
name: luffy, age: 19
do_something() function num = 1
do_something() function num = 2
do_something() function num = 3
```

通过输出的结果可以看到,虽然运行的三个线程中都执行了任务函数`do_something()`但是`call_once()`中指定的回调函数只被执行了一次,我们的目的也达到了。
## 多线程异步操作


C++11中增加的线程类,使得我们能够非常方便的创建和使用线程,但有时会有些不方便,比如需要获取线程返回的结果,就不能通过join()得到结果,只能通过一些额外手段获得,比如：定义一个全局变量,在子线程中赋值,在主线程中读这个变量的值,整个过程比较繁琐。C++提供的线程库中提供了一些类用于访问异步操作的结果。

那么,什么叫做异步呢？

<img src="https://www.subingwen.cn/cpp/async/image-20210412104358831.png" alt="image.png" style="zoom:30%;" />
我们去星巴克买咖啡,因为都是现磨的,所以需要等待,但是我们付完账后不会站在柜台前死等,而是去找个座位坐下来玩玩手机打发一下时间,当店员把咖啡磨好之后,就会通知我们过去取,这就叫做异步。

- 顾客（主线程）发起一个任务（子线程磨咖啡）,磨咖啡的过程中顾客去做别的事情了,有两条时间线（异步）
- 顾客（主线程）发起一个任务（子线程磨咖啡）,磨咖啡的过程中顾客没去做别的事情而是死等,这时就只有一条时间线（同步）,此时效率相对较低。

> 在I/O模型中：
> - 同步：内核向应用程序通知I/O就绪事件，由应用程序（工作线程）完成读写
> - 异步：内核向应用程序通知I/O完成事件，内核自己完成读写


> 在并发模式中：
> - 同步：代码按顺序执行
> - 异步：代码可能受中断，信号等影响，交错地执行

### std::future
因此多线程程序中的任务大都是异步的,主线程和子线程分别执行不同的任务,如果想要在主线中得到某个子线程任务函数**返回的结果**可以使用C++11提供的`std:future`类,这个类需要和其他类或函数搭配使用,先来介绍一下这个类的API函数：

类的定义
通过类的定义可以得知,`future`是一个模板类,也就是这个类可以**存储任意指定类型**的数据。
```c++
// 定义于头文件 <future>
template< class T > class future;
template< class T > class future<T&>;
template<> class future<void>;
```

构造函数
```c++
// ①
future() noexcept;
// ②
future( future&& other ) noexcept;
// ③
future( const future& other ) = delete;
```

- 构造函数①：默认无参构造函数
- 构造函数②：移动构造函数,转移资源的所有权
- 构造函数③：使用=delete显示删除拷贝构造函数, 不允许进行对象之间的拷贝

**常用成员函数（public)**

一般情况下使用=进行赋值操作就进行对象的拷贝,但是`future`对象不可用复制,因此会根据实际情况进行处理：

- 如果other是右值,那么转移资源的所有权
- 如果other是非右值,不允许进行对象之间的拷贝（该函数被显示删除禁止使用）

```c++
future& operator=( future&& other ) noexcept;
future& operator=( const future& other ) = delete;
```

取出`future`对象内部保存的数据,其中`void get()`是为`future<void>`准备的,此时对象内部类型就是void,该函数是一个<font color=#ff0000>阻塞函数</font>,当子线程的<font color=#ff0000>数据就绪</font>后解除阻塞就能得到传出的数值了。
```c++
T get();
T& get();
void get();
```

因为`future`对象内部存储的是异步线程任务执行完毕后的结果,是在调用之后**的将来**得到的,因此可以通过调用`wait()`方法,阻塞当前线程,等待这个子线程的任务执行完毕,任务执行完毕当前线程的阻塞也就解除了。
```c++
void wait() const;
```

如果当前线程`wait()`方法就会死等,直到子线程任务执行完毕将**返回值写入到future对象**中,调用`wait_for()`只会让线程阻塞一定的时长,但是这样并不能保证对应的那个子线程中的任务已经执行完毕了。

`wait_until()`和`wait_for()`函数功能是差不多,前者是阻塞到某一指定的时间点,后者是阻塞一定的时长。

```c++
template< class Rep, class Period >
std::future_status wait_for( const std::chrono::duration<Rep,Period>& timeout_duration ) const;

template< class Clock, class Duration >
std::future_status wait_until( const std::chrono::time_point<Clock,Duration>& timeout_time ) const;
```

当`wait_until()`和`wait_for()`函数返回之后,并不能确定子线程当前的状态,因此我们需要判断函数的返回值,这样就能知道子线程当前的状态了：

|           常量            |           解释           |
| :---------------------: | :--------------------: |
| future_status::deferred |      子线程中的任务函仍未启动      |
|  future_status::ready   |  子线程中的任务已经执行完毕,结果已就绪   |
| future_status::timeout  | 子线程中的任务正在执行中,指定等待时长已用完 |

### std::promise
std::promise是一个**协助线程赋值的类**,它能够**将数据和future对象绑定**起来,为获取线程函数中的某个值提供便利。

类成员函数
类定义
通过std::promise类的定义可以得知,这也是一个模板类,我们要在线程中传递什么类型的数据,模板参数就指定为什么类型。

```c++
// 定义于头文件 <future>
template< class R > class promise;
template< class R > class promise<R&>;
template<> class promise<void>;
```

构造函数
```c++
// ①
promise();
// ②
promise(promise&& other) noexcept;
// ③
promise(const promise& other) = delete;
```

- 构造函数①：默认构造函数,得到一个空对象
- 构造函数②：移动构造函数
- 构造函数③：使用=delete显示删除拷贝构造函数, 不允许进行对象之间的拷贝

**公共成员函数**
在`std::promise`类内部管理着一个future类对象,调用`get_future()`就可以得到这个future对象了

```c++
std::future<T> get_future();
```

存储要传出的 `value` 值,并立即让状态就绪,这样数据被传出其它线程就可以得到这个数据了。重载的第四个函数是为`promise<void>`类型的对象准备的。
```c++
void set_value( const R& value );
void set_value( R&& value );
void set_value( R& value );
void set_value();
```


除了设置value还可以将异常状态存储

```c++
void set_exception(exception_ptr __p)
```


存储要传出的 `value` 值,但是不立即令状态就绪。在当前线程退出时,子线程资源被销毁,再令状态就绪。
```c++
void set_value_at_thread_exit( const R& value );
void set_value_at_thread_exit( R&& value );
void set_value_at_thread_exit( R& value );
void set_value_at_thread_exit();
```

#### promise的使用
通过promise传递数据的过程一共分为5步：

1. 在主线程中创建std::promise对象
2. 将这个std::promise对象通过引用的方式传递给子线程的任务函数
3. 在子线程任务函数中给std::promise对象赋值
4. 在主线程中通过std::promise对象取出绑定的future实例对象
5. 通过得到的future对象取出子线程任务函数中返回的值。

子线程任务函数执行期间,让状态就绪
```c++
#include <iostream>
#include <thread>
#include <future>
using namespace std;

int main()
{
    promise<int> pr;
    thread t1([](promise<int> &p) {
        p.set_value(100);
        this_thread::sleep_for(chrono::seconds(3));
        cout << "睡醒了...." << endl;
    }, ref(pr));

    future<int> f = pr.get_future();
    int value = f.get();
    cout << "value: " << value << endl;

    t1.join();
    return 0;
}
```

示例程序输出的结果：
```c++
value: 100
睡醒了....
```

示例程序的中子线程的任务函数指定的是一个匿名函数,在这个匿名的任务函数执行期间通过`p.set_value(100)`;传出了数据并且激活了状态,数据就绪后,外部主线程中的`int value = f.get()`;解除阻塞,并将得到的数据打印出来,5秒钟之后子线程休眠结束,匿名的任务函数执行完毕。

**子线程任务函数执行结束,让状态就绪**
```c++
#include <iostream>
#include <thread>
#include <future>
using namespace std;

int main()
{
    promise<int> pr;
    thread t1([](promise<int> &p) {
        p.set_value_at_thread_exit(100);
        this_thread::sleep_for(chrono::seconds(3));
        cout << "睡醒了...." << endl;
    }, ref(pr));

    future<int> f = pr.get_future();
    int value = f.get();
    cout << "value: " << value << endl;

    t1.join();
    return 0;
}
```

示例程序输出的结果：
```c++
睡醒了....
value: 100
```

在示例程序中,子线程的这个匿名的任务函数中通过`p.set_value_at_thread_exit(100)`;在执行完毕并退出之后才会传出数据并激活状态,数据就绪后,外部主线程中的`int value = f.get()`;解除阻塞,并将得到的数据打印出来,因此子线程在休眠5秒钟之后主线程中才能得到传出的数据。

另外,在这两个实例程序中有一个知识点需要强调,在外部主线程中创建的promise对象必须要通过引用的方式传递到子线程的任务函数中,<font color=#ff0000>在实例化子线程对象的时候,如果任务函数的参数是引用类型,那么实参一定要放到std::ref()函数中,表示要传递这个实参的引用到任务函数中</font>

C++11 中引入 `std::ref` 用于取某个变量的引用，这个引入是为了解决一些传参问题。

```c++
void f(int& n1, int& n2, const int& n3)
{
    std::cout << "In function: " << n1 << ' ' << n2 << ' ' << n3 << '\n';
    ++n1; // increments the copy of n1 stored in the function object
    ++n2; // increments the main()'s n2
    // ++n3; // compile error
}

int main()
{
    int n1 = 1, n2 = 2, n3 = 3;
    std::function<void()> bound_f = std::bind(f, n1, std::ref(n2), std::cref(n3));
    n1 = 10;
    n2 = 11;
    n3 = 12;
    std::cout << "Before function: " << n1 << ' ' << n2 << ' ' << n3 << '\n';
    bound_f();
    std::cout << "After function: " << n1 << ' ' << n2 << ' ' << n3 << '\n';
}
```

上述代码在执行 `std::bind` 后，在函数 `f()` 中 `n1` 的值仍然是 1，`n2` 和 `n3` 改成了修改的值，**说明 `std::bind` 使用的是参数的拷贝而不是引用，因此必须显示利用 `std::ref` 来进行引用绑定**。具体为什么 `std::bind` 不使用引用，可能确实有一些需求，使得 C++11 的设计者认为默认应该采用拷贝，如果使用者有需求，加上 `std::ref` 即可。


### std::packaged_task
std::packaged_task类包装了一个[[C++#可调用对象包装器、绑定器|可调用对象包装器类对象]]（可调用对象包装器包装的是可调用对象,可调用对象都可以作为函数来使用）

这个类可以将内部包装的函数和future类绑定到一起,以便进行后续的异步调用,它和std::promise有点类似,std::promise内部保存一个共享状态的值,而std::packaged_task保存的是一个函数。

#### 类成员函数
类的定义
通过类的定义可以看到这也是一个模板类,模板类型和要在线程中传出的数据类型是一致的。

```c++
// 定义于头文件 <future>
template< class > class packaged_task;
template< class R, class ...Args >
class packaged_task<R(Args...)>;
```

**构造函数**

```c++
// ①
packaged_task() noexcept;
// ②
template <class F>
explicit packaged_task( F&& f );
// ③
packaged_task( const packaged_task& ) = delete;
// ④
packaged_task( packaged_task&& rhs ) noexcept;
```

- 构造函数①：无参构造,构造一个无任务的空对象
- 构造函数②：通过一个可调用对象,构造一个任务对象
- 构造函数③：显示删除,不允许通过拷贝构造函数进行对象的拷贝
- 构造函数④：移动构造函数

**常用公共成员函数**
通过调用任务对象内部的`get_future()`方法就可以得到一个future对象,基于这个对象就可以得到传出的数据了。
```c++
std::future<R> get_future();
```

#### packaged_task的使用
`packaged_task`其实就是对子线程要执行的任务函数进行了包装,和可调用对象包装器的使用方法相同,包装完毕之后直接将包装得到的任务对象传递给线程对象就可以了。

```c++
#include <iostream>
#include <thread>
#include <future>
using namespace std;

int main()
{
    packaged_task<int(int)> task([](int x) {
        return x += 100;
    });

    thread t1(ref(task), 100);

    future<int> f = task.get_future();
    int value = f.get();
    cout << "value: " << value << endl;

    t1.join();
    return 0;
}
```

在上面的示例代码中,通过`packaged_task`类包装了一个匿名函数作为子线程的任务函数,最终的得到的这个任务对象需要通过引用的方式传递到子线程内部,这样才能在主线程的最后通过任务对象得到future对象,再通过这个future对象取出子线程通过返回值传递出的数据。
出处。

### std::async
std::async函数比前面提到的`std::promise和packaged_task`更高级一些,因为通过这函数可以直接启动一个子线程并在这个子线程中执行对应的任务函数,异步任务执行完成返回的结果也是存储到一个future对象中,当需要获取异步任务的结果时,只需要调用future 类的`get()`方法即可,如果不关注异步任务的结果,只是简单地等待任务完成的话,可以调用future 类的`wait()`或者`wait_for()`方法。该函数的函数原型如下：

```c++
// 定义于头文件 <future>
// ①
template< class Function, class... Args>
std::future<std::result_of_t<std::decay_t<Function>(std::decay_t<Args>...)>>
    async( Function&& f, Args&&... args );

// ②
template< class Function, class... Args >
std::future<std::result_of_t<std::decay_t<Function>(std::decay_t<Args>...)>>
    async( std::launch policy, Function&& f, Args&&... args );
```

可以看到这是一个模板函数,在C++11中这个函数有两种调用方式：

- 函数①：直接调用传递到函数体内部的可调用对象,返回一个future对象
- 函数②：通过指定的策略调用传递到函数内部的可调用对象,返回一个future对象

函数参数:

- `f`：可调用对象,这个对象在子线程中被作为任务函数使用
- `Args`：传递给 f 的参数（实参）
- `policy`：可调用对象·f的执行策略


|  策略                       |  说明                                                               |
|:-------------------------:|:-----------------------------------------------------------------:|
|  `std::launch::async`     |  调用async函数时创建新的线程执行任务函数                                           |
|  `std::launch::deferred`  |  调用async函数时不执行任务函数,直到调用了future的get()或者wait()时才执行任务（这种方式不会创建新的线程）  |  

关于`std::async()`函数的使用,对应的示例代码如下：

#### 方式1
调用`async()`函数直接创建线程执行任务

```c++
#include <iostream>
#include <thread>
#include <future>
using namespace std;

int main()
{
    cout << "主线程ID: " << this_thread::get_id() << endl;
    // 调用函数直接创建线程执行任务
    future<int> f = async([](int x) {
        cout << "子线程ID: " << this_thread::get_id() << endl;
        this_thread::sleep_for(chrono::seconds(5));
        return x += 100;
    }, 100);

    future_status status;
    do {
        status = f.wait_for(chrono::seconds(1));
        if (status == future_status::deferred)
        {
            cout << "线程还没有执行..." << endl;
            f.wait();
        }
        else if (status == future_status::ready)
        {
            cout << "子线程返回值: " << f.get() << endl;
        }
        else if (status == future_status::timeout)
        {
            cout << "任务还未执行完毕, 继续等待..." << endl;
        }
    } while (status != future_status::ready);

    return 0;
}
```

示例程序输出的结果为：
```c++
主线程ID: 8904
子线程ID: 25036
任务还未执行完毕, 继续等待...
任务还未执行完毕, 继续等待...
任务还未执行完毕, 继续等待...
任务还未执行完毕, 继续等待...
任务还未执行完毕, 继续等待...
子线程返回值: 200
```

<img src="https://article.biliimg.com/bfs/article/76d891e52f018bb09c75981b2349b25f38716159.png" alt="image.png" style="zoom:50%;" />

#### 方式2
调用`async()`函数不创建线程执行任务

```c++
#include <iostream>
#include <thread>
#include <future>
using namespace std;

int main()
{
    cout << "主线程ID: " << this_thread::get_id() << endl;
    // 调用函数直接创建线程执行任务
    future<int> f = async(launch::deferred, [](int x) {
        cout << "子线程ID: " << this_thread::get_id() << endl;
        return x += 100;
    }, 100);
    this_thread::sleep_for(chrono::seconds(5));
    cout << f.get();
    return 0;
}
```

示例程序输出的结果：

```c++
主线程ID: 24760
主线程开始休眠5秒...
子线程ID: 24760
200
```

由于指定了`launch::deferred` 策略,因此调用`async()`函数并不会创建新的线程执行任务,当使用future类对象调用了get()或者wait()方法后才开始执行任务（此处一定要注意调用wait_for()函数是不行的）。

通过测试程序输出的结果可以看到,两次输出的线程ID是相同的,任务函数是在主线程中被延迟（主线程休眠了5秒）调用了。


> [!summary] 最终总结：
> 1. 使用async()函数,是多线程操作中最简单的一种方式,不需要自己创建线程对象,并且可以得到子线程函数的返回值。
> 2. 使用std::promise类,在子线程中可以传出返回值也可以传出其他数据,并且可选择在什么时机将数据从子线程中传递出来,使用起来更灵活。
> 3. 使用std::packaged_task类,可以将子线程的任务函数进行包装,并且可以得到子线程的返回值。


# 协程

## 协程的概念

> 协程是在用户空间被调度的，而线程是在内核空间被调度，在用户空间到内核空间有切换的开销，而且线程之前会有资源竞争，协程是多任务的子例程，是用户来操作的，在IO密集而计算不密集用协程会更效，因为IO主要开销是上下文切换，而协程的切换开销非常小。**线程能利用cpu多核的资源，而协程不行**。


普通的函数的返回点: 返回
协程返回点: **暂停**+返回


> [!multi-column]
>
>> [!note]+ 普通函数的调用
>>
>><img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240510143452.png" alt="image.png" style="zoom:40%;" />
>
>> [!warning]+ 协程的调用
>>
>><img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240510143521.png" alt="image.png" style="zoom:40%;" />


- 和普通函数不同的是协程是**有状态**的知道自己上一次执行到哪里？而普通函数是**无状态**的。
- 协程会在被**暂停时**保存运行状态，并可以从保存的运行状态中恢复，这个概念跟线程很相似，线程也可以被恢复和暂停，但是线程的回复和暂停对程序员不可见，由操作系统内核控制的，而协程是在用户态的对程序员是可见的。
- 协程可以被称作用户态的线程

#cpp面试点 进程、线程和协程的区别是什么?

> [!multi-column]
>
>> [!note]+ 进程
>> 1. 进程是**资源分配**的最小单位，每个进程都有自己的独立内存空间，进程由**进程控制块**、**程序段**和**数据段**组成。
>> 2. 进程是应用程序运行的载体，可看做是正在执行的程序。
>> 3. 由于每个进程都有**独立的代码和数据空间**，所以进程间的切换会有较大的开销。 
>> 4. <img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240510144546.png" alt="image.png" style="zoom:40%;" />
>
>> [!warning]+ 线程
>>
>>1. 线程是**CPU任务调度和执行**的最小单位，一个进程中可以包含多个线程。 
>>2. 线程可以并发运行，且**共享进程提供的相同的代码和数据空间**
>>3. 每个线程都有自己独立的**运行栈**和**程序计数器**，线程间切换的开销小。 
>>4. <img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240510144848.png" alt="image.png" style="zoom:40%;" />
>
>> [!warning]+ 协程
>>
>>1. 又称微线程，是一种**用户态的轻量级线程**，一个线程可以有多个协程。 
>>2. 协程的调度完全由用户控制（也就是在用户态执行）。 
>>3. 协程拥有自己的寄存器上下文和栈。 
>>4. 协程调度切换时，将寄存器上下文和栈保存到线程的**堆区**，在切回来的时候，恢复先前保存的寄存器上下文和栈，直接操作栈则基本**没有内核切换的开销**，可以不加锁的访问全局变量，所以协程的上下文切换非常快。 
>>5. <img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240510145539.png" alt="image.png" style="zoom:40%;" />


# 设计模式


## 设计模式基础概念

- 什么是设计模式
	1. **定义**：
	   - 在软件开发中经过验证的
	   - 用于解决特定环境下**重复出现问题的解决方案**
	   - 可以理解为"解决问题的固定套路"
	
	2. **使用原则**：
	   - 不是越多越好
	   - 要根据实际需求合理使用
	   - 过度使用会导致代码复杂化

- 设计模式解决的核心问题

	1. **需求的双重性**：
	   - 稳定点：不变的部分
	   - 变化点：可能改变的部分
	   
	2. **常见场景**：
	   ```
	   ┌────────────────┐
	   │ 需求类型       │ 适用场景
	   ├────────────────┼─────────────────
	   │ 全是稳定点     │ 注重代码可读性
	   │ 全是变化点     │ 游戏开发、脚本语言
	   │ 稳定+变化混合  │ 最常见，需要设计模式
	   └────────────────┘
	   ```
	
	3. **设计目标**：
	   - 最小化修改成本
	   - 通过少量代码修改适应需求变化
	   - 保持代码的可维护性和扩展性

- 形象比喻
	```
	问题：如何保持房间整洁（稳定点）而又养猫（变化点）？
	解决方案：把猫关在固定区域（笼子）里
	设计原则：隔离变化，保护稳定
	```

- 实践要点
	1. **识别模式**：
	   - 辨识问题中的稳定点和变化点
	   - 选择合适的设计模式
	
	2. **权衡使用**：
	   - 评估收益和成本
	   - 避免过度设计
	
	3. **持续优化**：
	   - 根据实际效果调整
	   - 保持代码的简洁性




## 单例模式

单例模式：一个类不管创建多少次对象，永远只能得到该类型一个对象的实例 

```c++
A *p1=new A();
A *p2=new A();
A *p3=new A();

// 常用到是，比如日志模块、数据库模块、

```


定义单例模式的步骤方法
- 构造函数私有化 
- 定义一个唯一的实例对象
- 定义获取类的实例对象的接口方法


> [!abstract] 单例模式种类：
> **饿汉式单例模式**：还没有获取实例对象，实例对象就已经产生了         
> **懒汉式单例模式**：唯一的实例对象，直到第一次获取它的时候，才产生       


饿汉式单例模式
```c++
class Singleton {
private:
    // 私有的静态实例
    static Singleton instance;

    // 私有的构造函数
    Singleton()=default;

public:
    // 删除拷贝构造函数和赋值操作符
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

    // 公共的静态方法，用于获取实例
    static Singleton* getInstance() {
      return &instance;
    }
};

Singleton Singleton::instance;
```

懒汉式单例模式
```c++
class Singleton {
private:
    // 私有的静态实例
    static Singleton* instance;
    // 私有的构造函数
    Singleton()=default;
public:
    // 删除拷贝构造函数和赋值操作符
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

    // 公共的静态方法，用于获取实例
    static Singleton& getInstance() {
      // 在第一次调用时创建实例
      if (instance == nullptr) {
        instance = new Singleton();
      }
      return *instance;
    }
};
// 初始化静态成员变量
Singleton* Singleton::instance = nullptr;
int main(){
  Singleton& s1 = Singleton::getInstance();
  cout<< &s1 << endl;
  return EXIT_SUCCESS;
}
```

上面的懒汉式单例代码是线程不安全的
可以做下面的修改
```c++
class Singleton {
private:
    // 私有的静态实例
    static Singleton*  instance;

    // 私有的构造函数
    Singleton()=default;

public:
    // 删除拷贝构造函数和赋值操作符
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

    // 公共的静态方法，用于获取实例
    static Singleton& getInstance() {
      // 在第一次调用时创建实例
      if (instance == nullptr) {
        std::lock_guard<std::mutex> lock(mtx); // 锁+双重的判断
        if (instance == nullptr)
        instance = new Singleton();
      }，
      return *instance;
    }
};

// 初始化静态成员变量
Singleton* Singleton::instance = nullptr;
```


扩展点：
- 上面的公共的静态方法里面也可以创建静态局部变量进行初始化(`static Singleton*  instance`)
- 在 C++11 和更高版本的标准中，静态局部变量的初始化确实是**线程安全的**。这意味着编译器会生成必要的代码以确保在多线程环境中，静态局部变量的初始化只会发生一次。

```c++
class Singleton {
public:
    static Singleton& getInstance() {
        static Singleton instance;
        return instance;
    }

    // ...
};
```


在 C++ 中，`std::once_flag` 是一个与 `std::call_once` 函数一起使用的类型，它用于确保某个函数或代码块只被执行一次，即使在多线程环境中也是如此。这是一个非常有用的机制，用于实现线程安全的懒惰初始化（lazy initialization）和其他只需执行一次的初始化操作。

```c++
 static std::shared_ptr<T>GetInstance(){
    static std::once_flag s_flag;
    std::call_once(s_flag,[&](){
      _instance=std::make_shared<T>();
    });
    return _instance;
  }
```



## 工厂模式

### 工厂模式概念和优点

工厂模式:是一种常用的设计模式，其核心目的是实现对象的创建和实例化的**解耦**。在工厂模式中，对象的创建过程不是通过直接调用构造函数，而是通过调用一个专门的工厂类或方法来完成。

- 封装性
	- **隐藏创建逻辑**：工厂模式可以隐藏对象创建的复杂逻辑。这对于创建过程涉及多个步骤或配置的场景特别有用。
- 可扩展性和灵活性
	- **简化扩展**：当系统需要引入新类型的对象时，不需要修改客户端代码，只需扩展工厂类即可。这符合开闭原则（对扩展开放，对修改关闭）。
	- **灵活性**：工厂模式允许在运行时决定创建哪个具体类的实例，提供了更大的灵活性。
- 解耦
	- **客户端和具体类解耦**：客户端代码不需要知道具体的类名，只需要知道工厂接口。这降低了系统各部分之间的耦合度。
- 代码维护
	- **易于维护和修改**：如果需要更改对象的创建逻辑或引入新的对象类型，只需修改工厂类。这使得代码更易于维护和修改。


**实现方式**
- 简单工厂模式：一个工厂类根据传入的参数决定创建哪种类型的对象。
- 工厂方法模式：定义一个用于创建对象的接口，让子类决定实例化哪一个类。工厂方法让类的实例化推迟到子类。
- 抽象工厂模式：创建一组相关或依赖对象的接口，而无需指定它们具体的类。


#### 简单工厂模式

Factory基类，提供了一个[[C++#纯虚函数和多继承|纯虚函数]]（创建产品）,定义派生类（具体产品的工厂）负责创建对应的产品，可以做到不同的产品，在不同的工厂里面创建，能够对现有工厂，以及产品的修改关闭 

```c++
class Car {
protected:
    string _name;
public:
    Car(string name) : _name(std::move(name)) {}

    virtual void show() = 0;
};

class BMW : public Car {
public:
    BMW(string name) : Car(std::move(name)) {}

    void show() override {
      cout << "获取了一辆宝马汽车" << endl;
    }
};

class Audi : public Car {
public:
    Audi(string name) : Car(std::move(name)) {}

    void show() override {
      cout << "获取了一辆奥迪汽车" << endl;
    }
};

class CarFactory {
public:
    virtual shared_ptr<Car> getCar(string name) = 0;
};

class BMWFactory : public CarFactory {
public:
    shared_ptr<Car> getCar(string name) override {
      return make_shared<BMW>(name);
    }
};
class AudiFactory : public CarFactory {
public:
    shared_ptr<Car> getCar(string name) override {
      return make_shared<Audi>(name);
    }
};

int main() {
  shared_ptr<CarFactory> bmwFactory = make_shared<BMWFactory>();
  shared_ptr<CarFactory> audiFactory = make_shared<AudiFactory>();
  bmwFactory->getCar("宝马")->show();
  return EXIT_SUCCESS;
}
```

#### 抽象工厂

- 实际上，很多产品是有关联关系的，属于一个产品簇，不应该放在不同的工厂里面去创建，这样 一是不符合实际的产品对象创建逻辑，二是工厂类太多了，不好维护 
- 把有关联关系的，属于一个产品簇的所有产品创建的接口函数，放在一个抽象工厂里面AbstractFacto ,派生类（具体产品的工厂）应该负责创建该产品簇里面所有的产品 

```c++
// 系列产品1
class Car {
protected:
    string _name;
public:
    Car(string name) : _name(std::move(name)) {}

    virtual void show() = 0;
};

class BMW : public Car {
public:
    BMW(string name) : Car(std::move(name)) {}

    void show() override {
      cout << "获取了一辆宝马汽车" << endl;
    }
};

class Audi : public Car {
public:
    Audi(string name) : Car(std::move(name)) {}

    void show() override {
      cout << "获取了一辆奥迪汽车" << endl;
    }
};

// 系列产品2
class Light{
public:
    virtual void show() = 0;
};

class BMWLight : public Light {
public:
    void show() override {
      cout << "获取了一盏宝马车灯" << endl;
    }
};


class AudiLight : public Light {
public:
    void show() override {
      cout << "获取了一盏奥迪车灯" << endl;
    }
};


// 工厂方法=>抽象工厂（对有一组关联关系的产品簇提供产品对象的统一创建）

class abstractFactory {
public:
    virtual Car *getCar(string name)=0;
    virtual Light *getLight()=0;
};

class BMWFactory : public abstractFactory {
public:
    Car *getCar(string name) override {
      return new BMW(name);
    }

    Light *getLight() override {
      return new BMWLight();
    }
};

class AudiFactory:public abstractFactory{
public:
    Car *getCar(string name ) override {
      return new Audi(name);
    }

    Light *getLight() override {
      return new AudiLight();
    }
};

int main() {
  unique_ptr<abstractFactory>bmwf(new BMWFactory());
  bmwf->getCar("奔驰")->show();
  shared_ptr<abstractFactory>audif= make_shared<AudiFactory>();
  audif->getLight()->show();
  return EXIT_SUCCESS;
}
```

## 代理模式

代理模式是一种结构型设计模式，它允许我们提供一个**代理**或**占位符**来**控制对另一个对象的访问**。代理对象充当了实际对象的替身，可以在不改变原始对象代码的情况下，添加新的功能或控制对原始对象的访问。

```c++
class VideoSite {
public:
    virtual void freeMovie() = 0; // 免费电影
    virtual void vipMovie() = 0; // vip电影
    virtual void ticketMovie() = 0; // 用券的电影
    virtual ~VideoSite() = default;
};

// 代理类跟原来的类功能是一样的
class VideoSiteProxy : public VideoSite {
public:
    void freeMovie() override {
        cout << "观看免费电影" << endl;
    }

    void vipMovie() override  {
        cout << "观看vip电影" << endl;
    }

    void ticketMovie() override {
        cout << "观看用券的电影" << endl;
    }
};

class freeVideoSiteProxy : public VideoSiteProxy {
public:
    explicit freeVideoSiteProxy(VideoSite *videoSite) : videoSite(videoSite) {}

     ~freeVideoSiteProxy() {
        delete videoSite;
    }

    void freeMovie() override {
      videoSite->freeMovie();
    }

    void vipMovie() override {
      cout<<"免费用户不能观看付费电影"<<endl;
    }

    void ticketMovie() override {
      cout<<"免费用户不能观看用券电影"<<endl;
    }
private:
    VideoSite *videoSite;
};

class vipVideoSiteProxy : public VideoSiteProxy {
public:
    explicit vipVideoSiteProxy(VideoSite *videoSite) : videoSite(videoSite) {}

    ~vipVideoSiteProxy() {
        delete videoSite;
    }

    void freeMovie() override {
      videoSite->freeMovie();
    }

    void vipMovie() override {
      videoSite->vipMovie();
    }

    void ticketMovie() override {
      cout<<"您目前没有券，需要购买电影券，才能观看电影"<<endl;
    }
private:
    VideoSite *videoSite;
};

class ticketVideoSiteProxy : public VideoSiteProxy {
public:
    explicit ticketVideoSiteProxy(VideoSite *videoSite) : videoSite(videoSite) {}

    ~ticketVideoSiteProxy() override {
        delete videoSite;
    }

    void freeMovie() override {
      videoSite->freeMovie();
    }

    void vipMovie() override {
      videoSite->vipMovie();
    }

    void ticketMovie() override {
      videoSite->ticketMovie();
    }
private:
    VideoSite *videoSite;
};

void watchMovie(unique_ptr<VideoSite>&ptr){
  ptr->freeMovie();
  ptr->vipMovie();
  ptr->ticketMovie();
}

int main() {

  unique_ptr<VideoSite> ptr1=make_unique<freeVideoSiteProxy>(new VideoSiteProxy());
  unique_ptr<VideoSite> ptr2=make_unique<vipVideoSiteProxy>(new VideoSiteProxy());
  unique_ptr<VideoSite> ptr3=make_unique<ticketVideoSiteProxy>(new VideoSiteProxy());
  watchMovie(ptr1);
  watchMovie(ptr2);
  watchMovie(ptr3);
  return EXIT_SUCCESS;
}
```

## 装饰器模式

装饰器：主要是增加现有类的功能  但是: 增加现有类的功能，
* 装饰器模式 Decorator * 通过子类实现功能增强的问题：为了增强现有类的功能，通过实现子类的方式，  
* 重写接口，是可以完成功能扩展的，但是代码中有太多的子类添加进来了  
 
<img src="https://article.biliimg.com/bfs/article/ffc5429bea10e7da7d0c8ffea5058e4b38716159.png" alt="image.png" style="zoom:60%;" />


```c++
class Car {
public:
    virtual void show() = 0;
    virtual ~Car() = default;
};

class BMW : public Car {
public:
    void show() override {
      cout << "这是一辆宝马汽车的配置!" << endl;
    }
};

class Benz : public Car {
public:
    void show() override {
      cout << "这是一辆奔驰汽车的配置!" << endl;
    }
};

class Audi : public Car {
public:
    void show() override {
      cout << "这是一辆奥迪汽车的配置!" << endl;
    }
};

// 装饰器类

class CarDecorator01 : public Car {
public:
    explicit CarDecorator01(Car *p) : ptr(p) {}

    void show() override {
      ptr->show();
      cout << "定速巡航" << endl;
    }

private:
    Car *ptr;
};

class CarDecorator02 : public Car {
private:
    Car *ptr;
public:
    explicit CarDecorator02(Car *p) : ptr(p) {}

    void show() override {
      ptr->show();
      cout << "自动刹车" << endl;
    }
};

class CarDecorator03 : public Car {
private:
    Car *ptr;
public:
    explicit CarDecorator03(Car *p) : ptr(p) {}

    void show() override {
      ptr->show();
      cout << "车道偏离" << endl;
    }
};

void doingShow(const initializer_list<unique_ptr<Car>>& cars) {
  for (const auto &item : cars) {
    item->show();
  }
}

int main() {

  unique_ptr<Car>p1=make_unique<CarDecorator01>(new BMW);
  unique_ptr<Car>p2=make_unique<CarDecorator02>(new Benz);
  unique_ptr<Car>p3=make_unique<CarDecorator03>(new Audi);
  doingShow({std::move(p1),std::move(p2),std::move(p3)});
  return EXIT_SUCCESS;
}
```

## 适配器模式

适配器模式是一种结构型设计模式，它**允许不兼容的接口能够一起工作**。这种模式的核心思想是创建一个**适配器类**，作为两个不兼容接口之间的**桥梁**，使它们能够协同工作。

```c++
/*  
 * 适配器模式：让不兼容的接口可以在一起工作  
 * 电脑=> 投影到 => 投影仪上 VGA HDMI TypeC * VGA接口的电脑，(TV)投影仪也是VGA接口  
 */  
// VGA接口类  
class VGA {  
public:  
    virtual void show() = 0;  
    virtual ~VGA() = default;  
};  
  
// TV01表示支持VGA接口的投影仪  
class TV01 : public VGA {  
public:  
    void show() override {  
      cout << "使用VGA播放的TV01投影仪" << endl;  
    }  
};  
  
// 实现一个电脑类(只支持VGA接口)  
  
class Computer {  
private:  
    VGA *vga;  
public:  
    explicit Computer(VGA *vga) : vga(vga) {}  
    ~Computer() {  
      delete vga;  
    }  
    void show() {  
      vga->show();  
    }  
};  
  
//进了一批新的投影仪，但是新的投影仪都是只支持HDMI接口  
  
class HDMI {  
public:  
    virtual void show() = 0;  
    virtual ~HDMI() = default;  
};  
  
class TV02 : public HDMI {  
public:  
    void show() override {  
      cout << "使用HDMI播放的TV02投影仪" << endl;  
    }  
};  
  
/*  
 方法1:换一个支持HDMI接口的电脑，这个就叫代码重构  
 方法2:买一个转换头，能够把VGA信号转成HDMI信号， 能把HDMI信号转成VGA信号,这个就叫适配器类  
 */  
class HDMI2VGA : public VGA {  
private:  
    HDMI *hdmi;  
public:  
    explicit HDMI2VGA(HDMI *hdmi) : hdmi(hdmi) {}  
    void show() override { // 该方法相当于就是转换头，做不同接口的信号转换的  
      hdmi->show();  
    }  
    ~HDMI2VGA() override {  
      delete hdmi;  
    }  
}; 
  
// 由于电脑(VGA接口)和投影仪(HDMI接口)不兼容，所以需要一个适配器类  
// 适配器类需要继承电脑类，实现投影仪类的接口  
int main() {  
  // 1.创建一个电脑对象  
  auto *cp= new Computer(new HDMI2VGA(new TV02));  
  cp->show();  
  return EXIT_SUCCESS;  
}

```

## 观察者模式

- 行为型模式：主要关注的是对象之间的**通信**  
- 观察者-监听者模式（发布-订阅模式）设计模式：主要关注的是对象的**一对多**的关系，也就是多个对象**都依赖一个对象**，当该对象的状态发生改变时，其它对象都能够接**收到相应的通知**。  
- 一组数据(数据对象)=> 通过这一组数据曲线图(对象1)/柱状图(对象2)/圆饼图(对象3)当数据对象改变时，对象1、对象2、对象3应该及时的收到相应的通知！

```c++
/*
 * Observer1 Observer2 Observer3
 * Subject(主题）主题有更改，应该及时通知相应的观察者，去处理相应的事件
 */

// 观察者抽象类
class Observer{
public:
    virtual void handler(int msgid)=0;
};

// 第一个观察者实例 对1，3感兴趣

class Observer1:public Observer{
public:
    void handler(int msgid) override {
      switch (msgid) {
        case 1:
          cout<<"Observer1 receive msg 1"<<endl;
          break;
        case 3:
          cout<<"Observer1 receive msg 3"<<endl;
          break;
        default:
          cout<<"Observer1 receive unknow msg"<<endl;
          break;
      }
    }
};

// 第二个观察者实例 对1，2感兴趣
class Observer2:public Observer{
public:
    void handler(int msgid) override {
      switch (msgid) {
        case 1:
          cout<<"Observer2 receive msg 1"<<endl;
          break;
        case 2:
          cout<<"Observer2 receive msg 2"<<endl;
          break;
        default:
          cout<<"Observer2 receive unknow msg"<<endl;
          break;
      }
    }
};
// 第三个观察者实例 对2，3感兴趣
class Observer3:public Observer{
public:
    void handler(int msgid) override {
      switch (msgid) {
        case 2:
          cout<<"Observer3 receive msg 1"<<endl;
          break;
        case 3:
          cout<<"Observer3 receive msg 3"<<endl;
          break;
        default:
          cout<<"Observer3 receive unknow msg"<<endl;
          break;
      }
    }
};

// 主题抽象类
class Subject {
private:
    unordered_map<int,vector<Observer*>>_subMap;
public:
    // 给主题增加观察者对象
    void addsub(int msgid,Observer* obs){
      _subMap[msgid].push_back(obs);
    }
    // 主题检测发生改变，通知相应的观察者对象
    void notify(int msgid){
      for(auto& obs:_subMap[msgid]){
        obs->handler(msgid);
      }
    }

};
// 给主题增加观察者对象
// 主题检测发生改变，通知相应的观察者对象
int main() {
  Subject subject;
  Observer1 obs1;
  Observer2 obs2;
  Observer3 obs3;
  subject.addsub(1,&obs1);
  subject.addsub(3,&obs1);
  subject.addsub(1,&obs2);
  subject.addsub(2,&obs2);
  subject.addsub(2,&obs3);
  subject.addsub(3,&obs3);

  int msgid=0;
  for(int i=0;i<10;i++){
    cout<<"input msg-id:";
    cin>>msgid;
    subject.notify(msgid);
  }
  return EXIT_SUCCESS;
}
```

## 责任链模式


定义:使多个对象都有机会处理请求,从而避免请求的**发送者(请求方)** 和**接收者(处理方)** 之间的**耦合关系**。将这些对象连成一条链,并沿着这条链传递请求,直到有一个对象处理它为止。

解决问题：解耦请求方和处理方,处理方由多个处理步骤构成,请求方不知道是被哪些处理过程处理(对于请求方屏蔽处理细节)


<img src="https://refactoringguru.cn/images/patterns/content/chain-of-responsibility/chain-of-responsibility.png?id=56c10d0dc712546cc283cfb3fb463458" alt="image.png" style="zoom:80%;" />

<img src="https://refactoringguru.cn/images/patterns/diagrams/chain-of-responsibility/solution1-zh.png?id=b852e6cd659ebe75b3bf2a49fd48c86b" alt="image.png" style="zoom:80%;" />

主要角色：

1. **请求方（Sender）**：发起请求的对象
2. **处理者接口（Handler）**：定义处理请求的接口
3. **具体处理者（Concrete Handler）**：实现处理者接口的具体类
4. **上下文（Context）**：请求的上下文信息

1. 上下文对象

	```c++
	class Context { // 这是一个请求方
	public:
	  std::string name;
	  int day;
	};
	
	// 这些是处理对象 可以理解为这是一条部门批准的申请链
	class HandleByMainProgram;
	class HandleByProjMgr;
	class HandleByBoss;
	class HandleByBeauty;
	```


2. 处理者接口
	```c++
	class IHandler { // 这是一个接口，所有的处理对象都要继承
	public:
	  IHandler() : next(nullptr) {}
	  virtual ~IHandler();
	  void setNextHandler(IHandler *handler) { // 设置下一个处理对象 传递责任链
	    next = handler;
	  }
	  bool Handle(const Context &ctx) { // 处理方法
	                               // 先看自己能不能处理，不能处理传递给下一个对象
	                               // 否则就打断
	    if (CanHandle(ctx)) {
	      return HandleRequest(ctx);
	    } else if (GetNextHandler()) {
	      return GetNextHandler()->Handle(ctx);
	    } else {
	      // err
	    }
	    return false;
	  }
	
	protected:
	  virtual bool HandleRequest(const Context &ctx) = 0;
	  virtual bool CanHandle(const Context &ctx) = 0;
	  IHandler *GetNextHandler() { return next; }
	
	private:
	  IHandler *next; // 下一个处理对象
	};
	```


3. 具体处理者
	```c++
	class HandleByMainProgram : public IHandler {
	protected:  
	  virtual bool HandleRequest(const Context &ctx) {
	    std::cout << "HandleByMainProgram" << std::endl;
	    return true;
	  }
	  virtual bool CanHandle(const Context &ctx) {
	    return ctx.day <= 1;
	  }
	};
	
	class HandleByProjMgr:public IHandler{
	protected:
	  virtual bool HandleRequest(const Context &ctx){
	    std::cout << "HandleByProjMgr" << std::endl;
	    return true;
	  }
	  virtual bool CanHandle(const Context &ctx){
	    return ctx.day <=20;
	  }
	};
	
	
	class HandleByBoss:public IHandler{
	protected:
	  virtual bool HandleRequest(const Context &ctx){
	    std::cout << "HandleByBoss" << std::endl;
	    return true;
	  }
	  virtual bool CanHandle(const Context &ctx){
	    return ctx.day <30;
	  }
	};
	
	class HandleByBeauty:public IHandler{
	protected:
	  virtual bool HandleRequest(const Context &ctx){
	    std::cout << "HandleByBeauty" << std::endl;
	    return true;
	  }
	  virtual bool CanHandle(const Context &ctx){
	    return ctx.day <=3;
	  }
	};
	```

4. 责任链的组装和使用
	```c++
	class LeaveProcess {
	public:
	  LeaveProcess() { // 可以看到这里我们形成了一条链
	    IHandler *h0 = new HandleByBeauty(); 
	    IHandler *h1 = new HandleByMainProgram();
	    IHandler *h2 = new HandleByProjMgr();
	    IHandler *h3 = new HandleByBoss();
	    h0->setNextHandler(h1);
	    h1->setNextHandler(h2);
	    h2->setNextHandler(h3);
	    handler = h0;
	  }
	  bool Handle(const Context &ctx) { return handler->Handle(ctx); }
	private:
	  IHandler *handler;
	};
	
	int main(){
	  // 抽象工厂
	
	  // nginx http处理
	
	  // 设置下一指针
	
	  Context ctx;
	  auto leave = std::make_shared<LeaveProcess>();
	  leave->Handle(ctx);
	  return 0;
	}
	```

- 那如何扩展呢？
	- 责任链模式所应对的变化点是我们可能需要灵活的去**增加处理节点**
	- 方法就是去继承IHandler,同时去修改我们处理方构建这个责任链的过程，往里面添加一个顺序


- 应用案例
	- **请假审批流程**
	- **日志处理系统**
	- **Web应用的过滤器链**
	- **Nginx的处理流程**
	- **数据库连接池的SQL处理**




