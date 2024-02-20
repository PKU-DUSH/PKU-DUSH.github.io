---
title: C++基础
date: 2023-10-24 00:01:26
tags:
	- C++
categories:
	- C++笔记
description: 此笔记关于C++的一些基础知识
---

{% note warning %}

# C++知识

## C++内存基础

### 进程虚拟空间地址划分

![image-20231007213657680](https://s2.loli.net/2023/10/07/oV6nFMAJB5rpih2.png)

![image-20231007212931110](https://s2.loli.net/2023/10/07/GtWVpPKcloYHOT6.png)

* 指令在运行的时候存放在在.text段；数据放在.data和.bbs段
  * .data：存放已经初始化的数据且数据不为0
  * .bbs：存放未初始化的数据（系统会赋值为0，所以打印未初始化的数据会输出为0）
* 程序内局部变量都是指令；全局变量和静态变量都是数据
* 指令存放在.text段，而指令执行的时候会在栈上开辟空间

![image-20231007213056216](https://s2.loli.net/2023/10/07/feUsTCEPRav6V8j.png)



* 对于不同的线程，用户空间是隔离的，内核空间是共享的
* 管道通信实际上是在内核空间划分了内存，所以才能通信，因为用户空间是隔离的

### 从编译器角度理解C++代码的编译和链接原理

* 编译过程如下：

![image-20231023223509832](https://s2.loli.net/2023/10/23/7QoHJPys9A1L3mG.png)

* 编译生成的.o文件组成

  *  这里的每一行称为段
     ![image-20231023221321923](https://s2.loli.net/2023/10/23/rcIURvgo6GnLS9X.png)

  

* 编译中的符号表
  ![image-20231023220927089](https://s2.loli.net/2023/10/23/6AUndlEhVjrNX5f.png)

  ![image-20231023221039508](https://s2.loli.net/2023/10/23/BZCtUo89REv7gTw.png)

  ![image-20231023220457249](https://s2.loli.net/2023/10/24/74OUCkfGo18MTEt.png)

  * 其中会标出 数据和指令存放的位置（.text和data），而引用的暂时标记为UND(可以出现很多UND，但是在代码中只能定义一次，要不就是重定义了)
  * g：global  l：local，链接的时候只能链接global的符号
  * 编译过程中不分配地址，前面的地址都是0；但是指令在编译阶段就已经产生了，

* 链接 

  ![image-20231023223528196](https://s2.loli.net/2023/10/23/1nHxDJ9pz2LmRMS.png)

  * 第一步：合并编译后的.o文件的相同段（所以可执行文件也是一段一段的），并分配虚拟地址
  * 符号重定向：符号解析成功后分配虚拟地址，把虚拟地址写回指令中叫符号重定向（因为编译后不分配地址，地址都为0）

* 可执行文件

  * 和.o文件一样都是由段组成的，但是多了progrma heads，用于指明把哪些内容加载到内存 
    ![image-20231023235152854](https://s2.loli.net/2023/10/23/QDbs4n5KO9tzLvM.png)

  

  * 此时.o文件在磁盘上，并有了虚拟地址，之后就是操作系统虚拟地址到物理地址的转换了

    ![image-20231023235539410](https://s2.loli.net/2023/10/23/EmSwrzRiOl4tgkJ.png)
    
## C++基础知识

### C++的new和delete

![image-20231025230340531](https://s2.loli.net/2023/10/25/TfyCK9qI6RAm8Xh.png)

* new和malloc的两个区别

  ![image-20231025230147364](https://s2.loli.net/2023/10/25/enyimbTpE4hkaqt.png)

  ![image-20231025225738550](https://s2.loli.net/2023/10/25/GNBWLMVSnjqYtCU.png)

* delete和free的区别
  * 无论是开辟数组还是单个元素 都是free(p)
  * 而单个元素 delete p；数组 delete[] p；

* new有多少种？
  * 定位new 是把data赋值为50，也就是在指定的内存上填充值

![image-20231025230745636](https://s2.loli.net/2023/10/25/eUh5FfR89mpi4rs.png)

### C++的const、指针和引用

![image-20231127140228238](https://s2.loli.net/2023/11/27/M6KNyRHigUoYZJz.png)

#### C++和C中的const区别

> C中的const并不是完全意义上的常量，只是在语法上规定了a不能作为左值再次赋值，实际上还是变量，因为可以用指针修改,所以也不能初始化中定义数组的长度(打印出来的值全是30)
>
> ![image-20240220224830692](https://s2.loli.net/2024/02/20/3T2AHCZYBFWSmr7.png)

> C++中必须初始化，但是是所有出现const的地方直接用20代替。所以*p=30确实改变了a内存的值，但是\*（&a）是直接变成了\*（&20）,所以输出还是20
>
> ![image-20231127141913977](https://s2.loli.net/2023/11/27/RGJ1EtpNlXWFI8Z.png)
>
> * 假如用一个变量初始化（变量的值只有运行的时候才知道），可以运行，但是就退化成常变量了，和C中一模一样了
>
> ![image-20231127142209072](https://s2.loli.net/2023/11/27/on8CBeTxfqAiKjD.png)

#### const和一二级指针的结合应用

> const修饰的量常出现的错误是：
>
> 1. 常量不能再作为左値＜=这样会直接修改常量的値
> 2. 不能把常量的地址泄露给一个普通的指针或者普通的引用变量< = 这样会间接修改常量的值，所以必须`const int *p = &a`

> const 和一级指针结合：
>
> * const 修饰离它最近的类型，可以把const和离它最近的类型去掉，剩下的部分不能改变了；
>   * 或者可以认为const修饰最大范围内能修改的变量：
>   * 比如`const int*p` ，*p被完全包含，所以不能变；`int *const p`,只有p被包含，所以p不能变。
>
> 1. `const int *p  = &a`：可以改变p的值，不能改变*p的值，也就是可以改变地址，不能改变地址对应的值
> 2. `int const* p `:和上面的一样！因为*和int不一样，不能单独定义变量，所以const修饰的还是int
> 3. `int *const p`: 修饰的是·`int*`，所以不可以改变p的值，可以改变*p的值，也就是不可以改变地址，可以改变地址对应的值
> 4. `const int *const p = &a`：最严格的指针，地址和值都不能改变，第一个const修饰int不能改变*p，第二个const修饰`int*`不能改变p。![image-20240221015833692](https://s2.loli.net/2024/02/21/wo9nrC7pAzOEQmc.png)
>
> * 类型转换总结：
>
> ![image-20240221020924985](https://s2.loli.net/2024/02/21/RzS2gkBxcJQMXK6.png)



> * 注意这两个输出都是 `int *`类型的
>
> ![image-20240221020656659](https://s2.loli.net/2024/02/21/et2cmL7kogDra9y.png)
>
> 



#### C++的左值引用和右值引用

#### const、指针和引用的结合使用

### 函数重载

### inline内联函数和形参带默认值的函数

#### inline内联函数

#### 形参带默认值的函数







### 





{% endnote %}
