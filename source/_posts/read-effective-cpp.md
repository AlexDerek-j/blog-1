---
title: 读Effective C++
date: 2018-02-04 20:04:46
tags: C++
---

# 读Effective C++

2018年一月份读书：《Effective C++：改善程序与设计的55个具体做法》

一月份利用晚上的时间粗读本书，算是对C++基础知识的复习与学习；按照章节顺序来读，前面部分较基础，后边涉及到泛型编程，看不太懂。

读完之后写下本篇，是对一月份学习的一个简要总结，督促后边继续学习，也是对本书内容进行索引总结，方便遇到具体问题快速查阅，以节省时间。

## 内容索引

本书共介绍C++程序设计的55个准则，作者已按照类型划分不同分类：

### 1 基础介绍

通用的也是常用的准则：

1. **了解C++组成**。四部分：基础C, Object-Oriented C++, Template C++(泛型编程), STL(程序库)
2. 用**const, enum, inline**替换#define
3. **尽可能使用const**。
4. **对象被使用前先初始化**。比如声明变量时就赋初值，构造函数使用成员初值列表，而不要在函数内进行赋值

### 2 类的基础方法

主要是这几个编译器会默认给你生成的类方法：默认构造函数，析构函数，拷贝构造函数，拷贝赋值操作符

1. **如果不要编译器生成的，要明确拒绝**。如将方法声明为private，并且不实现
2. **为多态基类声明virtual析构函数**
3. **别让异常逃离析构函数**。析构函数要捕获异常，要么吞下它们，要么结束程序
4. **不在构造和析构过程调用virtual函数**。
5. **令operate＝返回一个reference to *this **。为了支持连等赋值
6. **在operate＝中处理自我赋值**。因为可能出现删除自己，再取自己内容的情况
7. **复制对象时勿忘其每一部分**。

### 3 资源管理

资源包括动态内存分配，以及文件描述符，互斥锁，数据库连接，sockets等。当你不再使用它时，必须还给系统，否则会导致内存泄漏。

使用对象来管理内存，主要是使用类的构造函数，析构函数，拷贝函数。如在构造函数中获得资源，并在析构函数中释放资源。

1. **小心拷贝行为**。禁止拷贝，使用引用计数法
2. **提供对原始资源的访问**。设计时要保留访问原始数据的接口
3. **new和delete要采用相同形式**。如new delete/ new[] delete[]
4. **以独立对象将newed对象置入智能指针**。智能指针会自动释放资源

### 4 设计与声明

作者说，软件设计是令软件作出你希望它做出的事情，以一般性的构想开始，最终演变成十足的细节，以允许特殊接口的开发。

接口的设计，要易用，不易误用。应该向开源库学习，提供的接口清晰无歧义，并尽可能考虑各种输入与异常安全。

1. **设计class犹如设计type**。作者提出一系列问题，是在设计高效classes时需要考虑的
2. **使用传引用替换传值**。传值涉及对象的拷贝，这就需要时间与空间成本；不过对内置类型，传值可能更好
3. **必须返回对象时，不要返回reference**。最怕引用指向local stack对象
4. **将成员变量声明为private**。封装，划分访问控制更安全
5. **宁以non-member non-friend替换member函数**。增加封装性
6. **若所有参数皆需要类型转换，请采用non-member函数**
7. **考虑写不抛出异常的swap函数**。

### 5 实现

设计部分完成之后就该实现了，实现部分要多考虑一些细节问题，避免降低效率，代码膨胀，过度耦合，资源泄漏等问题。

1. **尽可能延后变量定义式的出现时间**。防止程序提前结束，导致不必要的构造和析构
2. **少做转型动作**。也是会影响效率；尽量使用新式转换(四种)
3. **避免返回handles指向对象内部成分**。
4. **为异常安全努力是值得的**。不泄漏资源，不允许数据败坏
5. **了解inline**。会被编译器替换，免除函数调用开销，但是可能会导致代码膨胀
6. **将文件间的编译依存关系降至最低**。

### 6 继承和面向对象设计

我感觉这是C++的精华部分，也挺重要。

1. **public继承表示is－a关系**。
2. **避免遮掩继承而来的名称**。作用域的遮掩行为；可使用using声明式使用基类的名称
3. **区分接口继承和实现继承**。选择派生类是继承基类的接口，还是接口加实现
4. **考虑virtual函数以外的其他选择**。作者提供了几个方案来替代虚函数
5. **绝不重新定义继承而来的non-virtual函数以及缺省参数值**。virtual函数是动态绑定
6. **通过复合塑模出has-a或根据某物实现出**。
7. **明智而审慎地使用private继承和多重继承**。

### 7 模板和泛型编程

关于模板和泛型编程，看的不是很懂，也没仔细看，这里就先直接拷贝作者的条款，以后再看有新的理解再修改补充。

1. **了解隐式接口和编译器多态**
2. **了解typename的双重意义**
3. **学习处理模板化基类内的名称**
4. **将与参数无关的代码抽离templates**
5. **运用成员函数模板接受所有兼容类型**
6. **需要类型转换时请为模板定义非成员函数**
7. **请使用traits classes表现类型信息**
8. **认识template元编程**

### 8 定制new和delete

手动管理内存，才能获得最佳的效率。

1. **了解new－handler的行为**。指定函数处理分配内存失败的情况
2. **了解new delete的合理替换时机**。有许多理由需要定制，包括改善效能，对heap运用错误进行调试，收集heap使用信息
3. **编写new delete时需固守常规**。包括一些固有的程式，以及异常情况的处理
4. **写了placement new也要写placement delete**。placement版本的new是一个特定位置上的new，一般接受一个void*,指向对象被构造之处，防止出现内存分配成功，但构造函数失败导致的内存泄漏问题

### 9 杂项

1. **不要轻忽编译器的警告**。有可能因为错过警告而导致复杂的调试情况
2. **熟悉标准程序库**。熟悉标准程序库，相当于在编写代码时拥有各种工具组件可以挑选，方便快速开发出程序，当然可能对部分对速度有更高要求的程序不太适用，但是通用性还是很高
3. **熟悉Boost**。因为标准程序库多数是从Boost中取来，Boost功能也更强一些


## 下一步

本书非常经典，只读一遍还远无法掌握其精髓，之后需要多看，可能不同的时期看收获也是不一样的。

接下来准备粗读下《C++标准程序库》，了解标准程序库有哪些组件，将常用的记熟，不常用的需要的时候可以快速找到即可。

