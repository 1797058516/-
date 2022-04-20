---
description: 自己动手完成第三章：程序的机器级表示的一些测试加深理解
---

# Computer Systems

### 重点



* [ ] 能够看懂常用汇编代码
* [ ] 理解过程(函数调用)
* [ ] 深入理解指针

### 涉及的Linux指令

```
gcc -Og -S mstore.c  //生成mstore.s汇编文件
gcc -Og -c mstore.c  //生成mstore.o二进制文件 00 01 89人看不懂
objdump -d mstore.o  //将机器文件转换成人能看得懂的汇编代码
```



### 常用的汇编指令以及寄存器

X86寄存器

![](<.gitbook/assets/image (1).png>)

mstore.s

![](.gitbook/assets/image.png)



### 过程

