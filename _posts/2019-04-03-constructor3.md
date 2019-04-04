---
layout:     post                    # 使用的布局（不需要改）
title:      构造函数(三) 拷贝构造函数               # 标题 
subtitle:   #副标题
date:       2019-04-03              # 时间
author:     lyk                      # 作者
header-img: img/post-constructor3.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - c++
    - OOP
---
## 什么是拷贝构造函数
~~拷贝听起来真高级~~

拷贝构造函数形如
```cpp
class_name(const class_name &object_name)
```
拷贝构造函数是一种特殊的构造函数，只有一个参数，这个参数是**本类**中的一个对象，以**引用**的形式传参，一般用const修饰，使参数值不变。

如果没有定义拷贝构造函数，编译器会自动隐式生成一个拷贝构造函数，用来简单的复制类中每个成员变量。

一个简单的类对象拷贝
```cpp
#include <bits/stdc++.h>
using namespace std;
class A {
	private :
		int a, b;
	public :
		A(int x, int y) : a(x), b(y) {}
		void print() {
			printf("%d %d\n", a, b);
		}
};

int main() {
	A a(10, 20);
	A b = a;
	b.print();
	return 0;
}
```
输出
>10 20

具体看一下拷贝构造函数怎么用。
```cpp
#include <bits/stdc++.h>
using namespace std;
class A {
	private :
		int a, b;
	public :
		A(int x, int y) : a(x), b(y) {}
		A(const A &c) {
			a = c.a;
			b = c.b;
		}
		void print() {
			printf("%d %d\n", a, b);
		}
};

int main() {
	A a(10, 20);
	A b = a;
	b.print();
	return 0;
}
```
## 为什么要引用
为什么用引用的方式传参呢。

举个反例
```cpp
#include <bits/stdc++.h>
using namespace std;
class A {
	private :
		int a;
	public :
		A(int x) : a(x) {}
		A(const A c) {
			a = c.a;
		}
		void print() {
			printf("%d\n", a);
		}
};

int main() {
	A a(10);
	A b = a;
	b.print();
	return 0;
}
```
`A b = a`时，调用了传值型的拷贝构造函数，
相当于`b.A(a)`
因为是传的值，所以要用a的值创建一个副本对象`c`，让`c`的值为`a`，则又需要调用构造函数`c.A(a)`，这样会无限创造副本对象`c`，无限调用`c.A(a)`;

所以
**<font color = "red">引用是为了防止无限递归。</font>**

其实没有加引用的话编译器也是不过编译的
## 拷贝构造函数的调用时机
1. 对象需要通过另一个对象对其进行赋值

```cpp
A a(10, 20);
A b = a;
```
2. 当函数的参数是类的对象
```cpp
#include <bits/stdc++.h>
using namespace std;
class A {
	private :
		int a, b;
	public :
		A(int x, int y) : a(x), b(y) {}
		A(const A &c) {a = c.a, b = c.b;}
		void print() {printf("%d\n", a * b);}
};

void fun(A c) { 	//函数的形参是类的对象 
	c.print();
}

int main() {
	A a(10, 20);
	fun(a);			//实参是类的对象，调用函数时将复制一个新对象c
	return 0;
}
```
3. 当函数的返回值是类的对象
```cpp
#include <bits/stdc++.h>
using namespace std;
class A {
	private :
		int a, b;
	public :
		A(int x, int y) : a(x), b(y) {}
		A(const A &c) {a = c.a, b = c.b;}
		void print() {printf("%d\n", a * b);}
};

A fun() { 	
	A b(20, 10);
	return b;		//函数的返回值是类的对象 
}

int main() {
	A a = fun();	//fun()返回 A 类的临时对象，并赋值给a 
	a.print();
	return 0;
}
```
## 深拷贝和浅拷贝
默认拷贝构造函数可以完成对象的数据值得**简单复制**，这就是浅拷贝。

对象的成员是指向堆时，浅拷贝只是将指针简单复制一遍，指向了原有的内存，而深拷贝是将新建的指针指向了一块新的内存。
### 浅拷贝
浅拷贝就是将数据简单复制一下，就像上面讲的。

看这段代码
```cpp
#include <bits/stdc++.h>
using namespace std;
class A {
	private :
		int *p;
	public :
		A(int x) {
			p = new int(x);
			printf("Constructor\n");
		}
		A(const A &oth) {
			this->p = new int(*oth.p);
			printf("Constructor\n");
		}
		~A() {
			if (p != NULL) delete p;
			printf("Destructor\n"); 
		}
		void print() { printf("%d\n", *p); }
		
};
int main() {
	A a(10);
	A b = a;
	return 0;
}
```
这是段代码出错了，为什么？

如图所示，
![未命名.PNG](https://i.loli.net/2019/03/31/5ca033ca37448.png)

b把a的指针简单复制了一遍，两者都指向了同一块内存，释放内存的时候被释放了两次，导致出错。
### 深拷贝
对于上面的代码，我们手写一个拷贝构造函数，像这样
```
#include <bits/stdc++.h>
using namespace std;
class A {
	private :
		int *p;
	public :
		A(int x) {
			p = new int(x);
			printf("Constructor\n");
		}
		A(const A &oth) {
			this->p = new int(*oth.p);
			printf("Constructor\n");
		}
		~A() {
			if (p != NULL) delete p;
			printf("Destructor\n"); 
		}
		void print() { printf("%d\n", *p); }
		
};
int main() {
	A a(10);
	A b = a;
	return 0;
}
```
我们给b初始化时，给它新开辟了一段内存空间
![未命名.PNG](https://i.loli.net/2019/03/31/5ca054bb1afb7.png)

这样，深拷贝就没有上述浅拷贝时内存释放两次的情况了。
