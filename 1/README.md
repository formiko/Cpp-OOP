# C++的初步知识
## 1.1 从C到C++
* C++支持封装和信息隐藏
* C++用泛化和继承的方法实现重用和多态
* C++支持重载
* C++支持泛型程序设计
* C++有功能强大的标准库
## 1.2 最简单的C++程序
* cout是输出流类的对象
* `std::cout`，`::`是作用域运算符，这样就可以不写`using namespace std;`
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
* 在C语言中，如果所调用的函数是整形的，也可以不进行函数声明。在C++中，如果函数调用的位置在函数定义之前，则要求在函数调用之前必须对所调用的函数作函数原型声明（强制性的）
在C++中，必须有如第一行的函数原型声明
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
在C语言中，如果所调用的函数是整形的，可以不进行如第一行的函数原型声明

