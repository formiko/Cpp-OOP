# 对运算符进行重载
## 4.1 为什么要对运算符重载
## 4.2 对运算符重载的方法
* 运算符重载的方法是定义一个重载运算符的函数。在使用被重载的运算符时，系统就自动调用该函数，以实现相应的功能。运算符重载实质上是函数的重载。
* 重载运算符的函数一般格式如下：
	``` C++
	函数类型 operator  运算符名称(形参表)
	{ 对运算符的重载处理 }
	```
* 如，想将“+”用于Complex类（复数）的加法运算，函数的原型可以是这样的：
	``` C++
	Complex operator + (Complex& c1, Complex& c2);
	```
* 注意：函数名是由operator和运算符组成的。上面的“operator+”就是函数名，意思是“对运算符+重载的函数”。这类函数和其他函数在形式上没有什么区别。
## 4.3 重载运算符的规则
1. C++ 不允许用户自己定义新的运算符，只能对已有的C++运算符进行重载。
2. C++允许重载的运算符。不能重载的运算符有5个:
	```
	.	成员访问运算符
	*	成员指针访问运算符
	::	域运算符
	sizeof	长度运算符
	?:	条件运算符
	```
3. 重载不能改变运算符运算对象（即操作数）的个数
4. 重载不能改变运算符的优先级别
5. 重载不能改变运算符的结合性
6. 重载运算符的函数不能有默认的参数（否则就改变了运算符参数的个数）
7. 重载的运算符必须和用户定义的自定义类型的对象一起使用，其参数至少应有一个是类对象（或类对象的引用）。也就是说，参数不能全部是C++的标准类型，以防止用户修改用于标准类型数据的运算符的性质。
8. 用于类对象的运算符一般必须重载，但有两个例外，运算符“=”和“&”不必用户重载。
	1. “=”，用户可以认为它是系统提倡的默认的对象赋值运算符，可以直接用于对象间的赋值，不必自己进行重载。但是有时候系统提供的默认的对象赋值运算符不能满足程序的要求，例如，数据成员中包含指向动态分配内存的指针成员时，这种情况需要自己重载赋值运算符。
	2. “&”，不必重载，它能返回类对象在内存中的起始地址。
9. 从理论上说，可以将一个运算符重载为执行任意的操作，如可以将加法运算符重载为输出对象中的信息，将“>”运算符重载为“<”运算。但这样使人莫名其妙。应当使重载运算符的功能类似于该运算符作用域标准类型数据时所实现的功能。
## 4.4 运算符重载函数作为类成员函数和友元函数
* 个人理解：类成员函数用来重载运算符，功能要弱一点，因为类成员函数有一个隐藏的参数this，相当于固定了双目运算的第一个运算对象必须为当前对象，而不能是标准数据类型或者其他对象。
* C++规定，赋值运算符=、下标运算符[]、函数调用运算符()、成员运算符->必须作为成员函数。
* 流插入<<和流提取运算符>>、类型转换运算符（个人困惑：类型转换运算符为什么不能。回顾：4.9.3类型转换函数会讲到，因为转换的本体是本类的对象）不能定义为类的成员函数、只能作为友元函数。
## 4.5 重载双目运算符
## 4.6 重载单目运算符
* 前置自增运算符和后置自增运算符的区分方法。C++约定：在自增（自减）运算符重载函数中，增加一个int型形参，就是后置自增（自减）运算符函数。
## 4.7 重载流插入运算符和流提取运算符
* 对“<<”和“>>”重载的函数形式如下：
	``` C++
	istream &operator >> (istream &, 自定义类 &);
	ostream &operator << (ostream &, 自定义类 &);
	```
* 只能将重载“>>”和“<<”的函数作为友元函数，而不能将它们定义为成员函数。
## 4.8 有关运算符重载的归纳
* 在运算符重载中使用引用（reference）的重要性。作为参数，传地址，因此不生成临时变量（实参的副本），减少了时间和空间的开销。返回值是对象的引用（如cout流），可以被赋值或参与其他操作，要特别小心。
## 4.9 不同类型数据间的转换
### 4.9.1 标准类型数据间的转换
* C++提供的显示类型转换，形式为：
	``` C++
	类型名(数据)
	```
* C语言中采用的形式为：
	``` C++
	(类型名)数据
	```
### 4.9.2 用转换构造函数进行不同类型数据的转换
* 用处：将一个指定类型的数据转换为类的对象。
* 转换构造函数只有一个形参，如
	``` C++
	Complex(double r){real = r; imag = 0;}
	```
* 在该类的作用域内可以用一下形式进行类型转换：
	``` C++
	类名(指定类型的数据)
	```
### 4.9.3 类型转换函数
* 用处：将一个类的对象转换成另一类型的数据。
* 类型转换函数的一般形式为
	``` C++
	operator 类型名()
	{实现转换的语句}
	```
  在函数名前面不能指定函数类型，函数没有参数。其返回值的类型是由函数名中指定的类型名来确定的。类型转换函数只能作为成员函数，因为转换的主体是本类的对象。不能作为友元函数或普通函数。
## 习题
1.
	``` C++
	#include <iostream>
	using namespace std;

	class Complex {
			double real, imag;
			friend Complex operator + (Complex &c1, Complex &c2);
		public:
			Complex(int r, int i): real(r), imag(i) {}
			void display();
	};

	void Complex::display() {
		cout << "(" << real << "," << imag << "i)" << endl;
	}

	Complex operator + (Complex &c1, Complex &c2) {
		return Complex(c1.real + c2.real, c1.imag + c2.imag);
	}

	int main() {
		Complex c1(1, 2), c2(3, 4);
		Complex c3 = c1 + c2;
		c3.display();
		return 0;
	}
	```
2.
	``` C++
	#include <iostream>
	using namespace std;
	
	class Complex {
			double real, imag;
			friend Complex operator + (Complex &c1, Complex &c2);
			friend Complex operator - (Complex &c1, Complex &c2);
			friend Complex operator * (Complex &c1, Complex &c2);
			friend Complex operator / (Complex &c1, Complex &c2);
		public:
			Complex(int r, int i): real(r), imag(i) {}
			void display();
	};
	
	void Complex::display() {
		cout << "(" << real << "," << imag << "i)" << endl;
	}
	
	Complex operator + (Complex &c1, Complex &c2) {
		return Complex(c1.real + c2.real, c1.imag + c2.imag);
	}
	
	Complex operator - (Complex &c1, Complex &c2) {
		return Complex(c1.real - c2.real, c1.imag - c2.imag);
	}
	
	Complex operator * (Complex &c1, Complex &c2) {
		return Complex(c1.real * c2.real - c1.imag * c2.imag, c1.real * c2.imag + c1.imag * c2.real);
	}
	
	Complex operator / (Complex &c1, Complex &c2) {
		Complex c(0, 0);
		c.real = (c1.real * c2.real + c1.imag * c2.imag) / (c2.real * c2.real + c2.imag * c2.imag);
		c.imag = (c1.imag * c2.real - c1.real * c2.imag) / (c2.real * c2.real + c2.imag * c2.imag);
		return c;
	}
	
	int main() {
		Complex c1(1, 2), c2(3, 4);
		(c1 + c2).display();
		(c1 - c2).display();
		(c1 * c2).display();
		(c1 / c2).display();
		return 0;
	}
	```
3. 
	``` C++
	#include <iostream>
	using namespace std;

	class Complex {
			double real, imag;
			friend Complex operator + (const Complex &c1,
			                           const Complex &c2);	//个人：因为不能对常量和临时生成的对象取地址，所以用const
			friend Complex operator + (Complex &c1, double r);
		public:
			Complex(int r, int i): real(r), imag(i) {}
			Complex(double r) {
				real = r;
				imag = 0;
			}
			void display();
	};

	void Complex::display() {
		cout << "(" << real << "," << imag << "i)" << endl;
	}

	Complex operator + (const Complex &c1, const Complex &c2) {
		return Complex(c1.real + c2.real, c1.imag + c2.imag);
	}

	Complex operator + (Complex &c1, double r) {
		return Complex(r + c1.real, c1.imag);
	}

	int main() {
		Complex c1(1, 2), c2(3, 4);
		(c1 + c2).display();
		(c1 + 99).display();
		(99 + c1).display();
		return 0;
	}
	```
4.
	``` C++
	#include <iostream>
	using namespace std;

	class Matrix {
			int arr[2][3];
			friend Matrix operator + (Matrix &m1, Matrix &m2);
		public:
			Matrix();
			Matrix(int a[2][3]);
			void display();
	};

	Matrix::Matrix() {
		for (int i = 0; i < 2; ++i) {
			for (int j = 0; j < 3; ++j) {
				arr[i][j] = 0;
			}
		}
	}

	Matrix::Matrix(int a[2][3]) {
		for (int i = 0; i < 2; ++i) {
			for (int j = 0; j < 3; ++j) {
				arr[i][j] = a[i][j];
			}
		}
	}

	void Matrix::display() {
		for (int i = 0; i < 2; ++i) {
			for (int j = 0; j < 3; ++j) {
				cout << arr[i][j] << " ";
			}
			cout << endl;
		}
	}

	Matrix operator + (Matrix &m1, Matrix &m2) {
		Matrix re;
		for (int i = 0; i < 2; ++i) {
			for (int j = 0; j < 3; ++j) {
				re.arr[i][j] = m1.arr[i][j] + m2.arr[i][j];
			}
		}
		return re;
	}

	int main() {
		int t[2][3] = {{1, 2, 3}, {4, 5, 6}};
		Matrix m1(t);
		Matrix m2(t);
		(m1 + m2).display();
		return 0;
	}
	```
5.
	``` C++
	#include <iostream>
	using namespace std;

	class Matrix {
			int arr[2][3];
			friend Matrix operator + (Matrix &m1, Matrix &m2);
			friend ostream &operator << (ostream &output, const Matrix &m);
			friend istream &operator >> (istream &input, Matrix &m);
		public:
			Matrix();
			Matrix(int a[2][3]);
	};

	Matrix::Matrix() {
		for (int i = 0; i < 2; ++i) {
			for (int j = 0; j < 3; ++j) {
				arr[i][j] = 0;
			}
		}
	}

	Matrix::Matrix(int a[2][3]) {
		for (int i = 0; i < 2; ++i) {
			for (int j = 0; j < 3; ++j) {
				arr[i][j] = a[i][j];
			}
		}
	}

	ostream &operator << (ostream &output, const Matrix &m) {
		for (int i = 0; i < 2; ++i) {
			for (int j = 0; j < 3; ++j) {
				output << m.arr[i][j] << " ";
			}
			output << endl;
		}
		return output;
	}

	istream &operator >> (istream &input, Matrix &m) {
		for (int i = 0; i < 2; ++i) {
			for (int j = 0; j < 3; ++j) {
				input >> m.arr[i][j];
			}
		}
		return input;
	}

	Matrix operator + (Matrix &m1, Matrix &m2) {
		Matrix re;
		for (int i = 0; i < 2; ++i) {
			for (int j = 0; j < 3; ++j) {
				re.arr[i][j] = m1.arr[i][j] + m2.arr[i][j];
			}
		}
		return re;
	}

	int main() {
		Matrix m1, m2;
		cin >> m1 >> m2;
		cout << m1 + m2;
		return 0;
	}
	```
6.
	``` C++
	#include <iostream>
	using namespace std;

	class Complex {
			double real, imag;
		public:
			Complex() {
				real = 0;
				imag = 0;
			}
			Complex(int r): real(r) {
				imag = 0;
			}
			Complex(double r, double i): real(r), imag(i) {}
			operator double() {
				return real;
			}
			void display() {
				cout << "(" << real << "," << imag << "i)" << endl;
			}
	};

	int main() {
		Complex c1(1, 2);
		double d1 = c1 + 3.0;
		cout << d1 << endl;
		Complex(d1).display();
		return 0;
	}
	```
7. 在Teacher类中声明定义转换构造函数，参数为Student类的引用。
