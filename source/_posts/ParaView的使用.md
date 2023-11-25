---
title: ParaView使用
date: 2023-10-07 19:13:31
description: 这篇博客记录了ParaView的一些相关使用，包括本地ParaView远程查看工作站文件、软件的基本使用等。
tags:
	- OpenFOAM
	- vsfFOAM
	- Linux
categories:
	- vsfFOAM
---
{% note warning %}

# ParaView使用

## 本地ParaView查看远程工作站文件

1. 首先在远程终端输入`pvserver`
 ![image-20231007191801789](https://s2.loli.net/2023/10/08/qIGsmbXt6exFWQ5.png)

2. 在本地ParaView中 File->connect  进行配置
   * Server Type 选择Client/Server
   * Host输入工作站IP
   * port默认即可
3. 双击刚创建好的Server即可连接到远程机器，直接打开远程文件

> * 注意如果没有foam文件，手动创建一个run.foam的空文件，然后用ParaView打开此空文件即可
> * 此后连接过程中，有可能出现提示框等待60s，且一直不消失导致卡住，个人解决办法是删掉并重新配置step 2
>   * update：梯子关了就好了



## ParaView的基本使用

> 官方教程：https://docs.paraview.org/en/latest/UsersGuide/index.html

1. 切片、切块
   * Slice/Clip，Properties中的Nomal调整方向（法向量）

   ![image-20231008154400595](https://s2.loli.net/2023/10/08/6FD8X1lZduhaMcQ.png)

2. 显示外部、框架、内部

   ![image-20231008155015532](https://s2.loli.net/2023/10/08/8WhQRTYdKNI4qt3.png)

   * 外部：Surface/Surface with edges/Wireframe/Feature Edges

   * 内部：Volume（需要渲染，有点慢）

3. 画等值面

   * contour ->  properties -> Value Range中设置参数（代表的就是值为该参数的等值面）

     ![image-20231008160013396](https://s2.loli.net/2023/10/08/3NyAhnGTLHjrzfx.png)
     
     ![image-20231008165533308](https://s2.loli.net/2023/10/08/7iuy14xfdB8Q65C.png)

4. 设置阈值（高于某值的一律设为最大值，低于某值则设为最小值）

   * Lower/Higher Threshold

     ![image-20231008160619201](https://s2.loli.net/2023/10/08/V7lciHktSv6nJj3.png)

{% endnote %}