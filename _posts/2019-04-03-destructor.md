---
layout:     post                    # 使用的布局（不需要改）
title:      析构函数               # 标题 
subtitle:   #副标题
date:       2019-04-03              # 时间
author:     lyk                      # 作者
header-img: img/post-destructor.jpg    #这篇文章标题背景图片
catalog: true                # 是否归档
tags:                     #标签
    - c++
    - OOP
---
## 什么是析构函数
创建对对象时，系统会自动调用构造函数为我们进行初始化，同样，在销毁对象时也会自动调用一个函数为我们收尾，如释放内存等，这个函数是析构函数。
 
析构函数也是一种特殊的成员函数。

**特点**

- 析构函数的名称和类的名称相同，在前面加`~`
- 析构函数没有返回值，无参数
- 析构函数只能在类中使用，且只有一个参数
- 不能继承和重载析构函数
- 析构函数不能显性调用

### 什么时候调用析构函数

**1.生命周期结束被销毁时**

```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	public :
		A(){ printf("A()\n"); }
		~A() { printf("~A()\n"); }
};
 
int main() {
	A a;
	printf("do sth\n");
	return 0;
}
```
输出
```cpp
A()
do sth
~A()
```
**2.主动delete指向对象的指针时**

我们用new创建了一个指向对象的指针，如果不主动delete，像这样
```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	public :
		A(){ printf("A()\n"); }
		~A() { printf("~A()\n"); }
};

int main() {
	A *p = new A;
	printf("do sth\n");
	return 0;
}
```
会输出
```cpp
A()
do sth
```
调用delete释放空间时会调用析构函数
```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	public :
		A(){ printf("A()\n"); }
		~A() { printf("~A()\n"); }
};

int main() {
	A *p = new A;
	printf("do sth\n");
	delete p;
	return 0;
}
```
输出
```cpp
A()
do sth
~A()
```
**3.对象A是对象B的成员，B的析构函数被调用时，A的也会被调用**

```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	public :
		A(){ printf("A()\n"); }
		~A() { printf("~A()\n"); }
};
class B {
	A a;
	public :
		B(){ printf("B()\n"); }
		~B() { printf("~B()\n"); }
};
class C {
	B a;
	public :
		C(){ printf("C()\n"); }
		~C() { printf("~C()\n"); }
};
int main() {
	C c;
	return 0;
}
```

输出
```cpp
A()
B()
C()
~C()
~B()
~A()
```

构造函数是先调用类成员的，再调用自己的，析构函数是先调用自己的，再调用类成员的。
