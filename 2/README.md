# 类和对象的特性
## 面向过程和面向对象
### 面向过程的软件开发方法
* 按照功能划分软件的结构
* 自顶向下的设计方法
* 以函数作为程序主体 
## 面向对象的软件开发方法
* 将软件系统看作各种对象的集合
* 系统结构比较稳定
* 对象将数据及函数的具体实现方式进行封装
## 类
### 类的UML表示法
|类名|
|:-:|
|属性|
|操作|

### 将类的声明和实现分别放在两个不同的文件中，这样做有以下几点好处：
* 类的实现文件通常较大，分开便于阅读、管理和维护
* 对软件开发商而言，他们可以向用户提供一些程序模块的接口，而不公开程序的源代码。分开管理就可以很好地解决此问题
* 将类定义放在头文件中，以后使用不必再定义，只需一条包含命令即可，实现了代码重用
* 便于团队对大型软件的分工合作开发

### 2.3.3 内置成员函数（inline成员函数）
* 为了减少时间开销，如果在类体中定义的成员函数中不包括循环等控制结构，C++系统会自动地对它们作为内置(inline)函数来处理
* 对需要作为内置函数处理的，在函数的定义或函数的原型声明时作inline声明即可（二者有其一即可）。注意：如果在类体外定义inline函数，则必须将类的声明和成员函数的定义都放在同一个头文件中（或者写在同一个源文件中），否则编译时无法进行置换（将函数代码的拷贝嵌入到函数调用点）。

## 对象
* 对象中的公用函数名就是对象的对外接口。
### 对象的UML表示法
|对象名：类名|
|:-:|
|属性|

### 2.3.4 成员函数的存储方式
* 各对象空间中只有数据成员，而无成员函数的空间。成员函数只存储一份，由对象共享。
* 不同的对象使用的是同一个函数代码段，它怎么能够分别对不同对象中的数据进行操作呢？C++为此专门设立了一个名为this的指针，用来指向不同的对象。

### 2.6.2 类声明和成员函数定义的分离
* 把成员函数的定义不放在头文件中的一个好处是不用重复编译。
* 将若干个常用的功能相近的类声明集中在一起，形成类库。
* 类库包括两个组成部分：
  1. 类声明头文件
  2. 已经过编译的成员函数的定义，它是目标文件
* 在用户程序中包含类声明头文件，类声明头文件就成为用户使用类库的有效方法和公用接口。。
* 用户可以看到头文件中类的声明和成员函数的原型声明，但看不到定义成员函数的源代码，更无法修改成员函数的定义，开发商的权益得到保护。

## 习题
1.	``` C++
	#include <iostream>
	using namespace std;

	class Time {
		public:
			void set_time(void);
			void show_time(void);
			int hour;
			int minute;
			int sec;
	};
	Time t;


	int main() {
		t.set_time();
		t.show_time();
		return 0;
	}

	void Time::set_time(void) {
		cin >> t.hour;
		cin >> t.minute;
		cin >> t.sec;
	}

	void Time::show_time(void) {
		cout << t.hour << ":" << t.minute << ":" << t.sec << endl;
	}
	```
2.	``` C++
	#include <iostream>
	using namespace std;

	class Time {
			int hour;
			int minute;
			int sec;
		public:
			void set_time(void) {
				cin >> hour >> minute >> sec;
			}
			void show_time(void) {
				cout << hour << ":" << minute << ":" << sec << endl;
			}

	};
	Time t;

	int main() {
		t.set_time();
		t.show_time();
		return 0;
	}
	```
3.	``` C++
	#include <iostream>
	using namespace std;

	class Time {
			int hour;
			int minute;
			int sec;
		public:
			void set_time(void);
			void show_time(void);

	};
	Time t;

	int main() {
		t.set_time();
		t.show_time();
		return 0;
	}

	void Time::set_time(void) {
		cin >> hour >> minute >> sec;
	}

	void Time::show_time(void) {
		cout << hour << ":" << minute << ":" << sec << endl;
	}
	```
4.	``` C++
	// main.cpp
	#include <iostream>
	#include "student.h"
	
	int main() {
		Student s;
		s.set_value(1, "Tom", 'm');
		s.display();
		return 0;
	}
	```
	``` C++
	// student.h
	#include <string>
	using namespace std;
	
	class Student {
		public:
			void display();
			void set_value(int, string, char);
		private:
			int num;
			string name;
			char sex;
	};
	```
	``` C++
	// student.cpp
	#include <iostream>
	#include "student.h"
	using namespace std;

	void Student::display()  {
		cout << "num:" << num << endl;
		cout << "name:" << name << endl;
		cout << "sex:" << sex << endl;
	}

	void Student::set_value(int nu, string na, char se) {
		num = nu;
		name = na;
		sex = se;
	}
	```
5.	``` C++
	// arraymax.h
	
	class Array_max {
		public:
			void set_value();
			void max_value();
			void show_value();
		private:
			int array[10];
			int max;
	};
	```
	``` C++
	// file1.cpp
	#include "arraymax.h"

	int main() {
		Array_max arrmax;
		arrmax.set_value();
		arrmax.max_value();
		arrmax.show_value();
		return 0;
	}
	```
	``` C++
	// arraymax.cpp
	#include "arraymax.h"
	#include <iostream>
	using namespace std;

	void Array_max::set_value() {
		int i;
		for (i = 0; i < 10; i++)
			cin >> array[i];
	}

	void Array_max::max_value() {
		int i;
		max = array[0];
		for (i = 1; i < 10; i++)
			if (array[i] > max)
				max = array[i];
	}

	void Array_max::show_value() {
		cout << "max=" << max;
	}

	```
6.	``` C++
	#include <iostream>
	#include <string>
	using namespace std;
	
	class Cuboid {
			double length, width, height, v;
		public:
			void set() {
				cin >> length >> width >> height;
			}
			void work_v() {
				v = length * width * height;
			}
			void show_v() {
				cout << v << endl;
			}
	};
	
	int main() {
		Cuboid c[3];
		for (int i = 0; i < 3; ++i) {
			c[i].set();
		}
		for (int i = 0; i < 3; ++i) {
			c[i].work_v();
			c[i].show_v();
		}
		return 0;
	}
	```
