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

### C++的const

    {% endnote %}
