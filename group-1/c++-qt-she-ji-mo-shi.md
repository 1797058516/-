---
description: 第一章--第六章   这部分主要讲解C++部分
---

# C++ QT设计模式\_1

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

![](<../.gitbook/assets/image (1) (1).png>)

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

![](<../.gitbook/assets/image (4) (1).png>)

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

![](<../.gitbook/assets/image (11) (1).png>)

const对象不能调用非const方法

const成员函数里可以更改mutable变量的值，不能更改常规数据成员的值。

正确的添加const

### 子对象

对象中包含另外的对象，被包含的对象称为子对象。

## QT简介

### 风格指南与命名约定

代码的风格！好的编程习惯。

## 列表

### 容器简介

只要有可能就应当使用列表而不是数组。

c语言中数最基本的方法就是使用数组来存储这种集合。但在c++中数组是“邪恶”的。

不适用数组理由1.编译器和运行时系统都不会检查数组下标是否位于正确的范围之内。2.使用数组程序员要有责任编写额外的范围检查代码，等等理由。

c++提供了最基本的泛型容器，std::list,QList。

泛型：能够接收模板参数的类或者函数。容器：能够包含其他对象的对象。

### 迭代器

iteration(迭代)：the process of repeating a [mathematical](https://www.oxfordlearnersdictionaries.com/us/definition/english/mathematical) or [computing](https://www.oxfordlearnersdictionaries.com/us/definition/english/computing) process or set of instructions again and again, each time applying it to the result of the previous stage

迭代器是提供对容器中的每一个元素进行间接访问的对象，它专门被设计用在循环之中。

STL风格迭代器的行为更与指针类试。但是与指针的差异是：不存在与指针的空值对应的迭代器值。（英版One important diference between iterators and pointers is that there is no iterator value that corresponds to the null value for pointers.）

eg：使用指针的函数在集合中搜索失败时可以返回一个空指针，但是不存在与空指针对应的迭代器值能够表示失败的基于迭代器的搜索。应该就是基于迭代器的搜索如果搜索失败没有对应的返回值。

c++迭代器，java风格迭代器以及foreach循环都是**迭代器模式**的例子。

![](<../.gitbook/assets/image (9).png>)

## 函数

### 函数重载

同一个函数名根据不同的参数匹配不一样的函数。

同名同参数可以对const进行重载

### 运算符重载

使用关键字operator为运算符赋予新的含义。

### 按值传递参数

大的类如果用值传递的话那么效率会非常的低，因为值传递会复制这个类到栈空间当中，所以函数对参数做出的改变在函数结束时会出栈消失。

### 按引用传递参

[引用&和取值&的区别](c++-qt-she-ji-mo-shi.md#yin-yong-bian-liang-reference-variables)

在c语言中通过指针来传递对象，以避免按值传递的消耗。但是指针的语法与使用对象的语法有所不同，此外偶尔的误用指针也会引起数据的崩溃，导致难以发现运行时的错误。

引用参数也是参数，只不过它是其他对象的别名。

指针和引用主要区别：对于指针，必须解引用它；对于引用，可以用访问被引用实体同样的方式访问按引用传递的对象。

![](<../.gitbook/assets/image (4).png>)

![](<../.gitbook/assets/image (11).png>)

比起指针更倾向于引用，这样可以降低程序偶尔发生内存崩溃的概率。只有在管理那些需要对指针进行操作的对象时（创建、销毁），才会选择使用指针，并且通常可以将这些例程封装为成员函数。

### const引用

![](<../.gitbook/assets/image (12).png>)

### 从函数返回引用

不要让函数返回一个指向局部对象的引用，因为在函数返回时局部变量全部销毁了。

### 对const重载

### inline函数



## 继承与多态

### 简单派生

所有面向对象的语言都支持**继承**这种特性，它使得类能够以多种不同的方式共享代码。它可以使设计良好的类更具有**可复用性。**

从一个共同的基类继承各种成员大大简化了派生类，利用某些**设计模式**，还可以去除冗余代码。

**重构**是一个提高软件设计而不会改变其底层行为的过程，其中一个步骤就是将相式的代码块转换成可复用的函数调用。

派生类的成员函数可以访问类中的protected成员。

#### 基类的成员初始化

创建派生类对象，就必须同时创建并初始化一个基类对象。此外，还必须调用基类构造函数来初始化任何派生对象的基类部分。



### 具有多态性的派生

polymorphous（多态性）：having or passing through many stages of development

&#x20;virtual关键字，它能够使得程序具有多态性。

### 抽象基类的派生

类仅仅是函数和数据的分组，是用来实现某种方式的组织和复用的有效工具。将事物分类，可以使人和计算机更加简单的管理。

当研究设计模式，开发框架和类库时，经常要设计继承树，其中只有叶节点才能被实例化，而所有的内部节点都是抽象的。

抽象基类是不适合实例化的类。特性：1.至少具有一个纯virtual函数。2.没有public构造函数

任何具体派生类都必须重写并定义全部的纯virtual基类函数，以进行实例化。

任何没有重写并定义全部纯virtual基类函数的派生类，都是抽象类。

对具有纯virtual函数的类进行实例化时不允许的，只能通过指针来操作，是纯粹被用来当做多态的基类的。

### 继承设计

### 重载，隐藏与重写

#### 函数隐藏

派生类中的成员函数，会隐藏基类中与之同名的全部函数。则:1.只有派生类函数可以直接被调用。2.类作用域::必须用来显式的调用基类函数。

### 构造函数，析构函数与复制赋值运算符

有三种成员函数从来不会被继承：1.复制构造函数。2.复制赋值运算符。3.析构函数。

#### 构造函数

基类的构造函数必须作为派生类初始化过程的一部分被调用。派生的构造函数在它的初始化表中可能需要指定应该调用基类的哪一个构造函数。

#### 初始化顺序

1.先基类初始化，按照它们在派生类的类首部中出现的顺序依次进行

2.数据成员初始化，按照声明的顺序进行。

#### 复制构造函数

类没有定义构造函数，编译器会自动产生一个public类型的复制构造函数。

要了解复制构造函数和构造函数的区别。

类的实例化可以是复制构造函数方式也可以用构造函数方式进行。



##







