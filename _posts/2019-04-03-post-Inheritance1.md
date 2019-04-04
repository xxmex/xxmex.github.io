---
layout:     post                    # 使用的布局（不需要改）
title:      继承(一) 三种继承方式               # 标题 
subtitle:   #副标题
date:       2019-04-03              # 时间
author:     lyk                      # 作者
header-img: img/post-Inheritance1.jpg    #这篇文章标题背景图片
catalog: true                # 是否归档
tags:                     #标签
    - c++
    - OOP
---
## 继承定义
继承是使代码可以复用的重要手段，也是面向对象程序设计的核心思想之一。

继承就是不修改原有的类，直接利用原来的类的属性和方法并进行扩展。原来的类称为基类，继承的类称为派生类，他们的关系就像父子一样，所以又叫父类和子类。

一般格式如下：
```cpp
class 派生类名 : 继承类型 基类名
```
派生类成员可以访问基类的**public成员**和**protected成员**。
## 三种继承方式
继承类型有三种，**共有继承(public)**，**私有继承(private)**和**保护继承(protected)**。

**共有继承：**

共有继承的特点是基类成员在派生类中都保持原来的状态
- 基类中的public仍为public，
- 基类中的protected仍为protected，
- 基类中的private仍为private；


**私有继承：**

私有继承的特点是基类中所有成员在派生类中都变为私有成员
- 基类中的public，protected变为private，
- 基类中的private仍为private；

**保护继承：**
- 基类中的public变为protected，
- 基类中的protected仍为protected，
- 基类中的private仍为private；

**private在派生类中依然存在，但不论以哪种方法继承基类，派生类都不能直接访问基类的私有成员。**

继承方式 | 基类的public成员|基类的protected成员|基类的private成员
:-:|:-:|:-:|:-:|
public继承|public成员|protected成员|private成员
protected继承|protected成员|protected成员|private成员
private继承|private成员|private成员|private成员

for example：
```cpp
class Base {	//基类 
	public :
		int pub;
	private:
		int pri;
	protected :
		int pro;
};

class A : public Base{	//public继承 
	public :
		int a;
		void init() {
			a = pub;	//可以，依然为public成员 
			a = pro;	//可以，依然为protected成员 
			a = pri;	//错误，基类的私有成员在派生类中是不可见的 
		}
}; 

class B : protected Base{	//protected继承 
	public :
		int b;
		void init() {
			b = pub;	//可以，变为protected成员 
			b = pro;	//可以，依然为protected成员 
			b = pri;	//错误，基类的私有成员在派生类中是不可见的 
		}
}; 

class C : private Base{	//private继承 
	public :
		int c;
		void init() {
			c = pub;	//可以，变为private成员 
			c = pro;	//可以，变为private成员 
			c = pri;	//错误，基类的私有成员在派生类中是不可见的 
		}
}; 
int x;
	A a;
	x = a.pub;	//可以，public继承的public成员是public的，对对象可见 
	x = a.pro;	//错误，public继承的protected成员是protected的，对对象不可见 
	x = a.pri;	//错误，public继承的private成员是private的，对对象不可见
	
	B b;
	x = b.pub;	//错误，protected继承的public成员是protected的，对对象不可见
	x = b.pro;	//错误，protected继承的protected成员是protected的，对对象不可见
	x = b.pri;	//错误，protected继承的private成员是private的，对对象不可见
	
	C c;
	x = c.pub;	//错误，protected继承的public成员是private的，对对象不可见
	x = c.pro;	//错误，protected继承的protected成员是private的，对对象不可见
	x = c.pri;	//错误，protected继承的private成员是private的，对对象不可见
	return 0;
```
- public继承是一个接口继承，保持is-a原则，每个父类可用的成员对子类也可用,因为每个子类对象也都是一个父类对象。
- protetced/private继承是一个实现继承，基类的部分成员并非完全成为子类接口的一部分，是has-a的关系原则，所以非特殊情况下不会使用这两种继承关系，在绝大多数的场景下使用的都是公有继承。
- class的默认继承是private的，struct的默认继承是public的。
