# C++的初步知识
## 1.1 从C到C++
* C++支持封装和信息隐藏
* C++用泛化和继承的方法实现重用和多态
* C++支持重载
* C++支持泛型程序设计
* C++有功能强大的标准库
## 1.3 C++对C的扩充
### 1.3.1 C++的输入输出
* cout是输出流类的对象
* `std::cout`，`::`是作用域运算符，这样就可以不写`using namespace std;`
* `cout << setw(6) << 1;` ，`setw(6)`的作用是为其后面的一个输出预留6列的空间，如果输出项的长度不足6列，则数据向右对齐，否则按实际长度输出。注意包含头文件iomanip（或iomanip.h）

### 补充 名字空间
* 一个名字空间由关键字namespace开始，通常后接一个标识符来标识名字空间，在名字空间开始和结束的地方分别用左右大括号标记
``` C++
namespace ns1 {
	int inflag;
}
namespace ns2 {
	int inflag;
}
int main() {
	ns1::inflag = 1;
	ns2::inflag = 2;
	return 0;
}
```
* 使用`using namespace 命名空间名`进行名字空间的声明
``` C++
namespace ns1 {
	int inflag;
}
namespace ns2 {
	int inflag;
}
int main() {
	using namespace ns1;
	inflag = 1;
	ns2::inflag = 2;
	return 0;
}
```
* 使用`using 命名空间成员名`使用命名空间成员
``` C++
namespace ns1 {
	int inflag;
}
namespace ns2 {
	int inflag;
}
int main() {
	using ns1::inflag;
	inflag = 1;
	ns2::inflag = 2;
	return 0;
}
```
* 无名的名字空间
``` C++
namespace {
	void fun() {
		std::cout << "OK." << std::endl;
	}
}
```
* 因为没有名字，因此无法在其它文件中引用
* 作用域为本文件从声明的位置开始到文件结束
* 有些集成化的调试工具可以对const常量进行调试，但不能对宏常量进行调试
### 1.3.3 函数原型声明
* 在C语言中，如果所调用的函数是整形的，也可以不进行函数声明。在C++中，如果函数调用的位置在函数定义之前，则要求在函数调用之前必须对所调用的函数作函数原型声明（强制性的）  
* 在C++中，必须有如第一行的函数原型声明
``` C++
int mymax(int, int);
int main() {
	mymax(1, 2);
	return 0;
}
int mymax(int a, int b) {
	return a > b ? a : b;
}
```
* 在C语言中，如果所调用的函数是整形的，可以不进行如第一行的函数原型声明  
``` C
//int mymax(int, int);
int main() {
	mymax(1, 2);
	return 0;
}
int mymax(int a, int b) {
	return a > b ? a : b;
}
```
### 1.3.4 函数的重载
* 一个函数名多用，通过参数个数和参数类型来区分，不能只是返回类型不同
### 1.3.5 函数模板
* 实际上是一个通用函数，其函数类型和形参类型不具体指定，用一个虚拟的类型来代表
* 定义函数模板的一般形式：
``` C++
template<typename 或 class T>
通用函数定义
```
* 只适用于函数的参数个数相同而类型不同，且函数体相同的情况
* 函数模板不是一个实实在在的函数，编译系统并不产生任何执行代码
* 当编译系统在程序中发现有与函数模板中相匹配的函数调用时，便生成一个重载函数，该重载函数的函数体与函数模板的函数体相同。该重载函数称为`模板函数`，它是函数模板的一个具体实例，只处理一种唯一的数据类型。
* typename和class在一种特殊情况下是有区别的
### 1.3.6 有默认参数的函数
* 指定默认值的参数必须放在形参列表的最右端
* ~~一个函数不能既作为重载函数，又作为有默认参数的函数~~
* 注意二义性的问题即可同时使用
* 必须在函数调用之前将默认值的信息通知编译系统
``` C++
#include<iostream>
using namespace std;
int mymax(int , int = 3);
int main() {
	cout << mymax(4);
	return 0;
}
int mymax(int a, int b) {
	return a > b ? a : b;
}
```
### 1.3.7 变量的引用
* 变量的“引用”就是变量的别名，因此引用又称为别名（alias）。
* 可以用const对引用加以限定，不允许改变改引用的值。但它并不阻止改变引用所代表的变量的值。这一特征在使用引用作为函数形参时是有用的，因为有时希望保护形参的值不被改变
* 可以用常量或者表达式对引用进行初始化，但此时必须用const作声明。
* 不能建立void类型的引用`void &a`
* 不能建立引用的数组`int &a[9]`
* 不能建立指向引用的指针`int & *p`
* 可以建立指针变量的引用
``` C++
int i = 5;
int *p = &i;
int* &pt = p;
```
* 引用的用途主要是用来作函数的参数或函数的返回值
``` C++
#include<iostream>
using namespace std;
int &f() {
	static int a;
	return a;
}
int main() {
	for(f() = 0; f() < 10; ++f()) {
		cout << f() << endl;
	}
	return 0;
}
```
### 1.3.8 内置函数
* inline function，又称内嵌函数、内联函数
* 使用内置函数可以节省运行时间，但却增加了目标程序的长度

### 1.3.9 作用域运算符
* `::a`表示全局作用域中的变量a
``` C++
#include<iostream>
using namespace std;
double a = 3.14;
int main() {
	int a = 3;
	cout << a << endl;
	cout << ::a << endl;
	return 0;
}
```
### 1.3.10 字符串变量
* 每一个字符串元素中只包含字符串本身的字符而不包括“\0”
* `sizeof(string)`的值是不确定的，书中说Visual C++中为4字节，本机测试为32字节

### 1.3.11 动态分配/撤销内存的运算符new和delete
* `delete[]pt;`在指针变量前面加一对方括号，表示对数组空间的操作

### 1.3.12 C++对C功能扩展的小结
1. 允许使用以//开头的注释
4. 可以用const定义常变量
9. 增加了内置函数，以提高程序的执行效率
10. 增加了单目的作用域运算符，这样在局部变量作用域内也能引用全局变量
12. 用new和delete代替malloc和free函数，使分配动态空间更加方便

## 习题
1. ```
   ThisisaC++program.

   ```
2. ```
   a+b=33

   ```
3. 分析：输出m,a,b,c之间最小的变量，m的值是随机的。
   验证：m值为0,应该是不确定的
4. 变量 c 未声明，输出应该用 >> 
5. 函数 add，变量 c 和 z 未声明；函数add的定义，第一行行末不应该有分号
6. 升序排序变量 x,y,z
7. ``` C++
   int max(int a, int b, int c = 0) {
	int m = a > b ? a : b;
	return m > c ? m : c;
   }
   ```
8. ``` C++
   void sort(int &a, int &b) {
	if (a > b) {
		cout << b << ' ' << a;
	} else {
		cout << a << ' ' << b;
	}
   }
   ```
9. ``` C++
   void sort(int &a, int &b, int &c) {
	int t;
	if (b < a) {
		t = a;
		a = b;
		b = t;
	}
	if (c < a) {
		t = a;
		a = c;
		c = t;
	}
	if (c < b) {
		t = b;
		b = c;
		c = t;
	}
   }
   ```
10. ``` C++
    void strcat(string &a, string &b) {
	a = a + b;
    }
    ```
