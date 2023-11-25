---
title: 远程工作站安装vsfFOAM
date: 2023-09-28 15:46:20
description: 这篇博客记录了vsfFOAM在远程工作站上安装的过程
tags: 
	- OpenFOAM
	- vsfFOAM
	- Linux
categories: 
	- vsfFOAM
---

{% note warning %}

# vsfFOAM安装

> 将vsfFOAM文件夹直接从PC拖入远程工作站的OpenFOAM文件夹内 后出现问题：
>
> ![image-20230925150025410](https://s2.loli.net/2023/09/25/yZEikHJnRmPYtld.png)
>
> 1. 尝试用chmod +x 赋予权限，但还是有问题
>
>    ![image-20230925175133908](https://s2.loli.net/2023/09/25/DvkcgOIPx4TeKZM.png)
>
>    ![image-20230925175048192](https://s2.loli.net/2023/09/25/kDRBigzvoxHuwTM.png)
>
> * 此问题已解决：OpenFOAM11删除了fvCFD这个头文件，所以推荐vsfFOAM依赖OpenFOAM10使用



## 加载OpenFOAM-10环境

![image-20230925212347606](https://s2.loli.net/2023/09/25/SQVFKahm4q2oz7y.png)

## 编译vsfFOAM

> * 注意vsfFOAM位置
>
>   ![image-20230925212448048](https://s2.loli.net/2023/09/25/LqjcFlIgV514Xfr.png)

* 编译

![image-20230925212525526](https://s2.loli.net/2023/09/25/UbuOm6LDZtjV4aR.png)



## 运行教学算例

> * 出现如下问题，原因是文件路径不符合，解决办法可以直接复制泰勒格林流文件夹里的文件到run文件夹下
>
> ![image-20230925213619260](https://s2.loli.net/2023/09/25/9A8DWP2NLqugSsj.png)
>
> * 加一个/*即可
>   ![image-20230925214436191](https://s2.loli.net/2023/09/28/i6aDyVtUph5qsxj.png)
>   ![image-20230925214542805](https://s2.loli.net/2023/09/28/kFrYa4JtUlgXhMC.png)



### 构造网格

![image-20230925214712144](https://s2.loli.net/2023/09/25/7E5HTYBZpct4agk.png)

### 初始化流场

> * 出现命令找不到的问题，因为vsfFOAM依赖于OpenFOAM，所以实际上是在OpenFOAM中找不到这个命令（如下二图）
>
>   * OpenFOAM中只有setFields能够设置简单的初始场，funkyxxx需要自行安装。
>
>   ![image-20230925220552066](https://s2.loli.net/2023/09/25/jbh2nK7aX3uwits.png)
>
>   ![image-20230925220653957](https://s2.loli.net/2023/09/25/3y9saYoefGlKpLR.png)

#### 安装funkySetFields

> * 可以根据这篇博客以及官方教程来
>   * https://blog.csdn.net/zq93538196/article/details/118310086
>   * https://openfoamwiki.net/index.php/Installation/swak4Foam

![image-20230925235839051](https://s2.loli.net/2023/09/25/1komGpsQ4POtUZ3.png)

### 分解计算域

![image-20230925235906887](https://s2.loli.net/2023/09/25/krHLUtGSWz8nc1D.png)

### 执行运算&重组计算域

* 并行执行运算使用如下指令![image-20230928153233799](https://s2.loli.net/2023/09/28/Yv4L1iqtkpI92F8.png)
  * 并非使用UserGuide中的vsfFoam指令。

* 重组计算域使用如下指令：![image-20230928153400874](https://s2.loli.net/2023/09/28/KOk8RscVuMtHgiW.png)

> 此过程可能会耗费数个小时，建议使用nohup 和& 指令，避免中途退出。
>
> * 最后产生的nohup.out文件大概有9w行数据（如下）：
>
>   ![image-20230928153832170](https://s2.loli.net/2023/09/28/v1fKCHA7Z5RDeTO.png)
>
> 

### paraFoam -builtin后处理

![image-20230927213541886](https://s2.loli.net/2023/09/27/vR2LHDcAF9mib6p.png)

![image-20230927213631892](https://s2.loli.net/2023/09/27/IFOfxmJ2jiysdGY.png)

{% endnote %}
