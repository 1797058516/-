# C++ QT设计模式

## C++简介

int factArg = 0 ;//C语言风格初始化

int fact(1) ; //C++语言风格初始化

函数重载。

QT提供了qmake工具，它会产生makefile文件。不用自己写makefile。

### 字符串

C++中处理字符串的三种途径：

1\. const char\* ,or C-style strings, which are used mainly when you are interfac- ing with C libraries, and rarely otherwise

2.string , from the C++ standard library, which is available **everywhere**. &#x20;

3.Qstring,which is preferred over STL string,because it has richer API and is easier to use.它支持写时复制和隐式共享。

### c++类型

in the output,you can see that sizeof(qstring) is only 4 bytes, but it is a complex class that **uses dynamic memory**, so you must call length() to get the number of QChar in the string.

![](<../.gitbook/assets/image (1).png>)

在运行时，qstring能够与具有同一个值的另一个字符串共享内存。



![](<../.gitbook/assets/image (8).png>)

atio()函数

argv是argument vector的缩写，它包含全部命令行字符串的一个数组。

main()函数需要向父进程返回一个int值。进程的返回值被称为退出状态（exit status），父进程可以使用退出状态决定下一步做什么。

#### 运算符

与（&&），或（||）

条件表达：(boolExpr) ? expr1 : expr2&#x20;

returns expr1 if boolExpr is true , and otherwise returns expr2 .

### const关键字

把某个实体声明为const之后，编译器将其视为只读。

因为不能进行赋值操作，所以const对象**必须进行初始化。**

****![](<../.gitbook/assets/image (6).png>)****

无需针对const变量分配存储空间，除非要取它地址。如果const变量的初始化器是一个能够在编译时进行求值的常量表达式，且编译器知道它的每一个使用之处，则没必要为它分配存储空间。

### 指针与内存范围

#### 一元运算符&和\*

#### 运算符new和delete

对空指针、被删除的指针或者未初始化的指针进行解引用操作会导致段错误（segmentation fault）；

尽管空指针解引用运行时会报错，但是在程序中空指针相当有用。eg搜索指针容器如果有的话返回该指针没有搜到返回空指针，通过空指针判断是否搜索成功。

### 引用变量(Reference variables)

左值是一个指向对象的表达式。eg变量、解引用的指针等。本质上，左值就是具有内存地址并且可以拥有名称的任何元素。注意，临时表达式或者常量表达式都不是左值，eg：i+l或者3。

![](<../.gitbook/assets/image (7).png>)

![](<../.gitbook/assets/image (4).png>)

引用&和取址&之间避免混淆！！

1.取址：返回该对象的地址，所以只能在**赋值语句的右侧**出现，或者在指针变量初始化表达式中出现。

2.引用：只出现在类型名称与所声明的引用名称中间。

### const\*与\*const

使用指针时，会涉及两个对象：指针本身以及它所指向的对象。

![](<../.gitbook/assets/image (10).png>)

## 类与对象

讲解类和对象，讨论对象上的成员函数操作。将介绍UML，以及解释static成员和const成员。还有构造函数、析构函数、复制操作以及友元。

#### struct

什么是数据成员？结构体中定义的变量。

void printFraction(**Fraction f**)如果该结构中的组成元素非常庞大，那么通过值传递这个结构的代价非常大！利用引用！

#### 类定义

class类似于struct，默认的属性不一样class是private，反之。

类的成员函数指定了 该类型的所有对象的行为。每一个成员函数都能够返回类中的其他 成员。非成员函数只能通过调用成员函数间接得操作对象。

private、public、protected属性之间的区别？

可访问性与可见性怎么理解并且这两之间的区别？

#### 封装

封装是面向对象的第一个概念性步骤。

封装的好处（特性）是什么？

#### UML介绍

UML框图能够简明而直观的方式展示类之间如何协作等。

![](<../.gitbook/assets/image (5).png>)

#### 类的友员

能够打破类的访问性规则--private等属性。友员机制能够允许非成员函数访问一个类的私有成员。

友员破坏封装性可能危害程序的可维护性，尽量少使用友员。

_友员函数一般在哪两种情况使用？_

#### 构造函数

可选但重要，不能忽略它。不带实参进行调用的构造函数称为默认构造函数。

在私有区域的构造函数？

#### 析构函数

当程序终止时，所有静态存储的对象都会被销毁。

### static关键字

#### static局部变量

函数中局部变量在函数结束时局部变量出栈然后销毁，声明静态（static）局部变量函数中该变量存储空间不在栈中，所以函数结束时该变量还是储存在静态存储区中。这样函数即使退出也不会对函数中的静态变量有影响。

#### static类成员

static数据成员只有一个实例，并且被该类的**所有**对象共享！声明static数据成员不影响sizeof(对象)的类对象大小。

static成员不占用全局命名空间的名称--存储空间跟全局不在一个区域。

static类成员必须在类定义中声明。在UML框图中带有下划线。

static数据成员都必须在类定义之外中被初始化。

### 类的声明和定义

![](../.gitbook/assets/image.png)

预处理不允许这样的循环依赖！

### 复制构造函数和赋值运算符

对象的”生命周期“管理意味着完全控制对象的诞生、繁殖和消亡的过程。构造函数管理对象的诞生，析构函数管理对象的消亡。

使用复制构造函数和赋值运算符管理对象的**繁殖**过程。

### 转换

### const成员函数

![](<../.gitbook/assets/image (11).png>)

正确的添加const

### 子对象

对象中包含另外的对象，被包含的对象称为子对象。

## QT简介

### 风格指南与命名约定

发
