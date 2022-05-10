# Page 1

条款7

为什么要为多态基类声明virtual析构函数？

**Why is it exactly, that when the base-class destructor is non-virtual, the object will not be destroyed correctly?**

**答**If at the point where you delete the object the static type of the variable is the bas type, than the destructor of the base type will be called, but the destructor of the sub class won't be called (as it is not virtual).

As a result the resources allocated by the base class will be freed, but the resources allocated by the sub class won't.

**Thus the object won't be destructed correctly.**

You are correct about that table: it is called a virtual method table or "vtable". But the result of the destructor being non-virtual is not that the destructors are not called in the correct order, but that the destructor(s) of the sub class(es) are not called at all!



虚函数

[https://www.geeksforgeeks.org/inheritance-in-c/?ref=lbp](https://www.geeksforgeeks.org/inheritance-in-c/?ref=lbp)

**虚拟功能规则**

1. 虚函数不能是静态的。
2. 虚函数可以是另一个类的友元函数。
3. 应该使用基类类型的指针或引用来访问虚函数，以实现运行时多态性。
4. 虚函数的原型在基类和派生类中应该是相同的。
5. 它们总是在基类中定义并在派生类中被覆盖。派生类不需要重写（或重新定义虚函数），在这种情况下，使用函数的基类版本。
6. 一个类可以有[虚拟析构函数](https://www.geeksforgeeks.org/virtual-destructor/)，但不能有虚拟构造函数。

