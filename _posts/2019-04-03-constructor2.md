---
layout:     post                    # 使用的布局（不需要改）
title:      构造函数(二) 初始化列表               # 标题 
subtitle:   #副标题
date:       2019-04-03              # 时间
author:     lyk                      # 作者
header-img: img/constructor2.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - c++
    - OOP
---
## 初始化列表
为了给类成员变量赋值，可以再构造函数的函数体内对成员变量赋值，也可以采用**初始化列表**。
如：
```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	private :
		int a, b;
	public :
		A(int x, int y) : a(x), b(y) {}
		/*
		相当于
		A(int x, int y) {
		    a = x, b = y;
		}
		*/
		void print() {
			printf("%d %d\n", a, b);
		}
};
int main() {
	A tmp(10, 20);
	tmp.print();
	A *p = new A(30, 40);
	p->print();
	return 0;
}
```
输出
```cpp
10 20
30 40
```

可以看到，利用初始化列表赋值时，在参数之后与函数主体之间直接跟一个 `:`, 后面是变量名，变量名后的括号内是给其所赋的值。**注意后面的括号，函数主体内只是没有内容，并不是没有函数主体**。

<font color = "red">使用初始化列表与赋值相比并没有什么效率上的提高，只是方便书写。</font>

初始化列表可以对全部的成员变量进行初始化，也可以对局部的变量进行初始化。
```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	private :
		int a, b;
	public :
		A(int x, int y) :  b(x) {
			a = y;
		}
		void print() {
			printf("%d %d\n", a, b);
		}
};
int main() {
	A tmp(10, 20);
	tmp.print();
	A *p = new A(30, 40);
	p->print();
	return 0;
}
```
输出
```cpp
20 10
40 30
```

**<font color = "red">特别注意，成员变量的初始化顺序与初始化列表中列出的类成员变量顺序无关，与类成员变量的声明顺序有关</font>**
```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	private :
		int a, b;
	public :
		A(int x) : b(x), a(b) {}
		void print() {
			printf("%d %d\n", a, b);
		}
};
int main() {
	A tmp(10);
	tmp.print();
	A *p = new A(30);
	p->print();
	return 0;
}
```
输出
```cpp
4194304 10
4064376 30
```
稍微改一改
```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	private :
		int b, a;	//注意这里 
	public :
		A(int x) : b(x), a(b) {}
		void print() {
			printf("%d %d\n", a, b);
		}
};
int main() {
	A tmp(10);
	tmp.print();
	A *p = new A(30);
	p->print();
	return 0;
}
```

输出
```cpp
10 10
30 30
```
## 何时使用初始化列表
**1. 类的成员变量被const修饰时**
```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	private :
		const int a;
	public :
		A(int x) : a(x) {}
		void print() {
			printf("%d\n", a);
		}
};
int main() {
	A tmp(10);
	tmp.print();
	A *p = new A(30);
	p->print();
	return 0;
}
```

**2. 初始化类的引用成员变量时**
```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	private :
		int &a;
	public :
		A(int x) : a(x) {}
		void print() {
			printf("%d\n", a);
		}
};
int main() {
	A tmp(10);
	tmp.print();
	A *p = new A(30);
	p->print();
	return 0;
}
```
输出
```cpp
10
30
```
**3. 类的成员变量是对象，且这个对象只有含有参数的构造函数，没有默认构造函数。**
```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	private :
		int a, b;
	public :
		A(int x, int y) : a(x), b(y) {}
		void print() {
			printf("%d\n", a * b);
		}
};

class B {
	private :
		A c;
	public :
		B(int x, int y) : c(x, y) {}
		void query() {
			c.print();
		}
};

int main() {
	B tmp(10, 20);
	tmp.query();
	B *p = new B(30, 20);
	p->query();
	return 0;
}
```
输出
```cpp
200
600
```
**4. 在派生类中初始化基类成员，调用基类的构造函数**
```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	private :
		int a;
	public :
		A(int x) : a(x) {
			printf("value of a : %d\n", a);
		}
};

class B : public A {
	public :
		B(int x) : A(x) {
			printf("B was created.\n\n");
		}
};

int main() {
	B tmp(10);
	B *p = new B(30);
	return 0;
}

```
输出
```cpp
value of a : 10
B was created.

value of a : 30
B was created.
```
