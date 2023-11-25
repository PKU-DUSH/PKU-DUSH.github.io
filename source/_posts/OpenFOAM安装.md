---
title: 远程工作站安装OpenFOAM
date: 2023-09-27 15:57:24
description: 这篇博客记录了在工作站上远程安装OpenFOAM的过程
tags:
	- OpenFOAM
	- Linux
categories:
	- vsfFOAM
---

{% note default %}

# 远程工作站安装OpenFOAM教程

> 一般来说，Linux下安装OpenFOAM有两种方法：
>
> 一种是直接用apt下载，但是此种方法仅限于个人电脑（因为需要sudo权限）；
>
> 另外一种更为推荐的常用办法是用**源代码编译安装**，由于需要远程连接实验室的电脑，在实验室的Linux环境下安装，所以也只能用这个办法。

> 推荐直接根据官方教程来，注意是Source Pack：https://openfoam.org/download/11-source/

## 下载并解压源文件

### step1 ： 建立OpenFOAM文件夹

![image-20230924144932715](https://s2.loli.net/2023/09/24/xBYPZWgMnkvzK4r.png)

### step2 ：执行下载解压命令 

![image-20230924150351555](https://s2.loli.net/2023/09/24/7oLWQOd95IB1bME.png)

![image-20230924150504744](https://s2.loli.net/2023/09/24/fXPTIvrAEUWm7nG.png)

### step3 ：改文件名，便于以后使用

![image-20230924150737127](https://s2.loli.net/2023/09/24/amTJlYC87QdWSAP.png)

## 设置环境变量

> 注意是在~/.bashrc中设置

![image-20230924152124605](https://s2.loli.net/2023/09/24/LKj5nD4fk3z1eXV.png)

![image-20230924152114681](https://s2.loli.net/2023/09/24/5jQ1pDItLcUZVdS.png)

> 最后运行一下bashrc脚本

![image-20230924152235522](https://s2.loli.net/2023/09/24/1iLYepaK732XVBw.png)

## 下载编译第三方库（包括paraview）

### Installing Scotch/PT-Scotch

![image-20230924154149151](https://s2.loli.net/2023/09/24/6TENoFiHRfj97Y2.png)

### Installing ParaView

![image-20230924154308317](https://s2.loli.net/2023/09/24/ldoEZeqDvt6PAHu.png)

> * 这里出问题了，但是不用管，可以用**paraFoam -builtin**代替
>
> ![image-20230924155942595](https://s2.loli.net/2023/09/24/cEt6iRV1BNHpf9m.png)

## 编译OpenFOAM

![image-20230924160049450](https://s2.loli.net/2023/09/24/vuVrpsQejiHLNtI.png)



## 运行简单算例并可视化

### 创建运行文件夹

![image-20230924160932211](https://s2.loli.net/2023/09/24/WD2LAREJhTSl5aO.png)

### 运行简单算例

![image-20230925212038629](https://s2.loli.net/2023/09/25/ugl7SPyibWa9vI3.png)

* 成功运行

​	![image-20230925212121916](https://s2.loli.net/2023/09/25/MPivyQYuLrEoksH.png)

{% endnote %}
