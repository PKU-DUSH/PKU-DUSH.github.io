---
title: git&github
date: 2023-11-25 14:00:04
tags:
	- Git
	- Github
categories:
	- Linux
description: 关于git或者github相关的命令和操作
---

{% note warning  %}

## git merge/pull request

<img src="https://s2.loli.net/2023/11/22/Qfo34dlcw5W1NBH.png" alt="image-20231122205333317" style="zoom:50%;" />

<img src="https://s2.loli.net/2023/11/22/Rfg36EaTvnxbLIs.png" alt="image-20231122201538592" style="zoom:50%;" />

{% note info  %}


 * `git status` # 查看当前状态
 * `git log` # 查看操作记录
 * `git branch`  # 显示所有本地分支
 * `git branch -d dev` # 删除分支

{% endnote %}

> 1. `git clone`
> 2. `git checkout -b dev` # 创建并切换至“dev”分支
> 3. `git add . ` **# 将目录中所有文件添加到暂存区中**
> 4. `git commit -m "xxx"`  **# 把暂存区中的文件提交到本地仓库中并添加描述信息**
> 5. `git checkout master` # 切换到master分支
> 6. `git merge dev` # 把dev分支合并到当前master分支
> 7. `git push -u origin master`  # **将本地仓库的分支提交至相应的远程仓库分支**
>    * 第一次使用 `git push -u origin master` 之后，下次可以直接使用 `git pull` 拉取代码，就不需要输入完整的命令 `git pull origin master `来拉取代码了。第二次也可以用` git push`推送代码而不用`git push origin master`。
>    * 一般情况下(多人合作)，本地修改代码后，每次从本地仓库merge到远程仓库之前都要先进行git pull（会自动和本地提交合并，与本地有冲突时需要手动合并，所以要commit之后再pull否则会覆盖修改的代码）操作，保证push到远程仓库时没有版本冲突。
>
> * ~~~bash
>   git clone
>   cd
>   git checkout -b dev
>   git add . && git commit -m "test-dev"
>       
>   #如果推送到远程的dev就直接git push origin dev
>   git checkout master
>   git merge dev
>   git push origin master
>   ~~~

> tips:
>
> * `git clone`包括了`git init`（本地创建文件git remote origin关联远程仓库才之前需要git init）
> * 以上是合作者的操作方式，如果不是，那么需要先fork成为自己的github仓库 -> git clone到本地修改 -> 推送到自己的github上之后再PR
> * git merge 冲突问题解决方案：https://blog.csdn.net/qq_42780289/article/details/97945300

## 终端git超时问题

> fatal: unable to connect to github.com:
> github.com[0: 20.205.243.166]: errno=Connection timed out

* 把`git clone git://github.com/ninja-build/ninja.git`中的`git`改成`https`就好了

{% endnote %}

