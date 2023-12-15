---
title: Linux命令
date: 2023-09-27 17:48:13
tags:
	- Linux
	- BASH
categories:
	- Linux
description: 记录一下最近使用但容易遗忘的Linux命令
---

{% note warning  %}

# Linux命令

## 编译结果输出到文件

* 针对中断产生的编译结果，可以输出到指定文件中以供分析

  > tee : 输出结果的同时也保留终端输出
  >
  > 1：正确运行输出
  >
  > 2：错误运行输出

  * **输出正确运行和错误运行结果（即输出全部编译结果）**

    ~~~bash
    make 2>&1 | tee out.txt
    ~~~

  * 输出正确运行结果
    ~~~bash
    make > out.txt    OR   make 1> out.txt   
    ~~~
  
  * 输出错误运行结果
    ~~~bash
    make 2> out.txt
    ~~~



## 后台不挂断地执行命令

> nohup : 不挂断地运行命令（没有后台运行的功能）
> & : 在后台运行，当用户退出（挂起）的时候，命令自动跟着结束
>
> 输出结果自动存放在nohup.out中
>
> * 注意使用该指令后要用exit退出终端，直接关闭可能会导致进程终止

~~~bash
nohup ./Allwmake &
~~~



> 查看当前后台运行的进程

~~~BASH
jobs -l
~~~



> 参考链接：https://blog.csdn.net/qq_37555071/article/details/113781938



## 一些系统命令

* 查找进程并kill

 ~~~bash
    ps -aux  //显示所有程序
    ps -ef | grep 进程关键字  //查找指定进程
    kill -9 进程号PID    //kill进程
 ~~~



* 查看CPU占用等信息

~~~BASH
	top    //查看CPU占用，shift+m 按内存占用排序
~~~

* 查看进程运行时间
  * ps -eo lstart 启动时间
  * ps -eo etime 运行多长时间.

~~~BASH
ps -eo pid,lstart,etime | grep 2459398
//2459398 Fri Dec  8 01:26:12 2023  5-15:30:10
~~~

* 解压命令

> 1. *.tar 用 tar –xvf 解压
> 2. *.tar.gz和*.tgz 用 tar –xzf 解压
> 3. *.zip 用 unzip 解压

> 压缩命令：`tar -zcvf /home/dush/OpenFOAM/dush-10/swak4Foam.tar.gz swak4Foam `

* 查看cpu核数

~~~BASH
cat /proc/cpuinfo
lscpu
~~~

* 查看最大可用线程数(CPU逻辑核心)

~~~BASH
nproc
//make -j $(nproc)
~~~





## .sh文件Permission denied的解决办法

> 赋予全部权限777（rwx）

~~~bash
chmod 777 xxx.sh
~~~



## 文件操作
* 移动文件

~~~BASH
//如果需要复制，把mv改为cp
//改文件名aaa->bbb
mv aaa bbb

//移动文件或文件夹
mv 文件（夹）名 目的路径

//移动 info 目录到 logs 目录中。注意，如果 logs 目录不存在，则该命令将 info 改名为 logs
mv info/ logs 

//移动 info 目录下所有目录和文件到 logs 目录中
mv info/* logs 
~~~

* 查看文件

~~~BASH
//在终端显示文件所有内容
cat file.txt

//按页查看文件内容，适合查看大文件
//按空格翻页，按q退出
less file.txt
less -N file.txt  //添加行号	
~~~

* 删除文件

~~~BASH
rm[选项] 文件或目录
选项：
-f：强制删除（force），和 -i 选项相反，使用 -f，系统将不再询问，而是直接删除目标文件或目录。
-i：和 -f 正好相反，在删除文件或目录之前，系统会给出提示信息，使用 -i 可以有效防止不小心删除有用的文件或目录。
-r：递归删除，主要用于删除目录，可删除指定目录及包含的所有内容，包括所有的子目录和文件。
//删除目录是 rm -r， 但是会一直提示，所以rm -rf
//虽然 "-rf" 选项是用来删除目录的，但是删除文件也不会报错。所以，为了使用方便，一般不论是删除文件还是删除目录，都会直接使用 "rm -rf" 选项
~~~

## 在没有root权限的情况下编译软件显示Permission denied问题

> 没有root权限是无法写入/usr/local/bin/文件中的

解决办法：

1. 自定义编译的输出目录，比如`./configure --prefix=/home/dush/re2c `
2. 把命令添加到环境变量中
   * 打开～/.bashrc or ~/.zshrc
   * 在末尾加入`export PATH=$PATH:/home/dush/re2c/bin` 即可

* 或者直接把可执行文件加入到环境变量，例如：`export PATH=$PATH:/home/dush/tree-2.1.1`

> 可以看考这个链接：https://blog.csdn.net/qq_41705840/article/details/124900211

## 终端SCP上传/下载文件

> 若下载目录则 scp -r

* 从工作站上下载文件到本地

~~~BASH
scp username@servername:/path/filename /Users/mac/path
~~~

* 从本地上传文件到工作站

~~~BASH
scp /path/filename username@servername:/path
~~~





{% endnote %}

