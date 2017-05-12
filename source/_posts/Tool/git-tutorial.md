---
title: Git 教程
date: 2017-03-01 20:13:39
tags: [Git, Github]
categories: 工具
updated:
comments:
permalink:
---


目录：
- Git 教程
- 使用 github 作为 git 托管服务器
- Git 常用命令汇总

<!--more-->

# Git 教程
## 安装Git

- 首先确认服务器是否安装Git

```
[root@iZj6c2vq0s1w6wkap2geanZ ~]# rpm -qa git
git-1.7.1-3.el6_4.1.x86_64
```

- 若没有，则安装
```
$ yum install git
```

## 建一个git用户，用来运行git服务

- 添加用户

```
[root@iZj6c2vq0s1w6wkap2geanZ ~]# adduser git
```

此时在阿里云服务器的根目录：/home文件夹下会自动新增一个文件夹git

- 按这个层级建立文件用于存放客户端用户的公钥：/home/git/.ssh/authorized_keys

在本地客户端的git bash中通过命令：（windows先装Git客户端）

```
$ ssh-keygen -t rsa -C "heyiminwork@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/ymhe/.ssh/id_rsa):
Created directory '/c/Users/ymhe/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/ymhe/.ssh/id_rsa.
Your public key has been saved in /c/Users/ymhe/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:Wfx6+z6eD/Qn73vMmDugoe20otVTQjWEAxlT7+TcrWk heyiminwork@gmail.com
The key's randomart image is:
+---[RSA 2048]----+
|        +=.o+    |
|        .ooo .   |
|          +.o    |
|         + * . . |
|        S . * o .|
|         ..+.. + |
|        .o=o..E=o|
|       .o.o+ o===|
|      .. oo .+*B*|
+----[SHA256]-----+
```

命令生成公钥，默认在c盘：用户/.ssh中id_rsa.pub文件是公钥，用记事本打开复制粘贴到服务器的/home/git/.ssh/authorized_keys文件中即可。

## 服务器中建立git仓库

- 自己在服务器根目录新建一个专门用于存放仓库的git文件夹，如/usr/git
通过如下代码创建并初始化仓库,此时是以个空仓库：
```
[root@iZj6c2vq0s1w6wkap2geanZ git]# git init --bare sample.git
```

裸仓库没有working dir，因为服务器上的git仓库纯粹是为了共享，仓库目录一般以.git结尾。然后把owner改为git：

```
[root@iZj6c2vq0s1w6wkap2geanZ git]# chown -R git:git sample.git
[root@iZj6c2vq0s1w6wkap2geanZ git]# ll
total 4
drwxr-xr-x 7 git git 4096 Nov 18 10:37 sample.git
```

## 禁止git用户登录Shell

出于安全考虑，git用户不应该登录shell。修改方法：编辑/etc/passwd，找到类似一行：
git:x:500:500::/home/git:/bin/bash
改为
git:x:500:500::/home/git:/usr/bin/git-shell
这样，git用户可以正常通过ssh使用git，但无法登录shell。

## 克隆仓库

在Git Bash客户端就可以通过ssh克隆仓库了：

```
ymhe@DESKTOP-B29K8H5 MINGW64 /e/git
$ git clone git@47.90.56.252:/usr/git/sample.git
Cloning into 'sample'...
The authenticity of host '47.90.56.252 (47.90.56.252)' can't be established.
RSA key fingerprint is SHA256:uc0hmvJyknGJsyCyFWrcO/BXAK4CEqo8bVobSSccmNg.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '47.90.56.252' (RSA) to the list of known hosts.
warning: You appear to have cloned an empty repository.

ymhe@DESKTOP-B29K8H5 MINGW64 /e/git
$ ll
total 0
drwxr-xr-x 1 ymhe 197121 0 11月 18 10:46 sample/
```

----

# 使用 Github

## 准备工作
<br/>
## 注册账号
[全球最大同性交友社区](https://github.com)
<br/>
## 安装&配置git
<br/>
### 安装git

- Linux
一般linux系统自带git，没有的话可以到对应仓库安装

```
yum install git
# or
apt-get install git
```

- Windows & Mac
直接下载[github Desktop客户端](https://desktop.github.com/)安装

<br/>
### 配置git

- 生成本地 ssh key
```
ssh-keygen -t rsa -C "emailsample@gmail.com" #Email为github上注册的邮箱
```

之后会要求确认路径和输入密码，使用默认的一路回车就行。成功的话会在~/下生成.ssh文件夹，进去，打开id_rsa.pub，复制里面的key。（id_rsa是私钥，id_rsa.pub是公钥）

- 添加公钥添加到github
登录github上，进入 Account Settings，左边选择SSH and GPG Keys，New SSH Key,title随便填，粘贴在你电脑上生成的公钥。
<br/>
![](leanote://file/getImage?fileId=581980ceab644162e100b4ba)

- 验证ssh key是否生效
```
#本地shell执行
ssh -T git@github.com
```
第一次的会提示是否continue，输入yes就会看到：You've successfully authenticated, but GitHub does not provide shell access 。这就表示已成功连上github。

- 设置username email
```
git config --global user.name "Ekimin" #双引号中为用户名
git config --global user.email "heyiminwork@gmail.com"
```

这个命令，会在“~/.gitconfig”中以如下形式输出设置文件：
```
[ymhe@iZ114e8q2mjZ ~]$ cat ~/.gitconfig
[user]
	name = Ekimin
	email = heyiminwork@gmail.com
```
想更改这些信息时，可以直接编辑这个设置文件。这里设置的姓名和邮箱地址会用在Git的提交日志中。由于在GitHub上公开仓库时，这里的姓名和邮箱地址也会随着提交日志一同被公开，所以请不要使用不便公开的隐私信息。

- 提高命令的可读性

```
[ymhe@iZ114e8q2mjZ ~]$ git config --global color.ui auto
```

-------

## 使用github

### 创建仓库

在主页点击创建仓库 New repository
![](leanote://file/getImage?fileId=581980ceab644162e100b4b8)
<br/>
新建仓库时：
![](leanote://file/getImage?fileId=581980ceab644162e100b4b7)

- Initialize this repository with a README
    支持MarkDown语法，一般新仓库都建议勾选该项
- Add .gitignore
    这个设定会帮我们把不需要在Git仓库中进行版本管理的文件记录在.gitignore文件中，省去了每次根据框架进行设置的麻烦。
- Add a license
    许可协议文件。如果这个仓库中包含的代码已经确定了许可协议，那么请在这里进行选择。随后将自动生成包含许可协议内容的LICENSE 文件，用来表明该仓库内容的许可协议。

仓库创建好后，链接为：https://github.com/用户名/Hello-Word
<br/>
**拓展：公开时的许可协议:**
![](leanote://file/getImage?fileId=581980ceab644162e100b4b9)

### 公开代码

- 克隆新建的仓库
![](leanote://file/getImage?fileId=581980ceab644162e100b4b6)
<br/>github主页或者客户端中都可以获取仓库SSH代码，用于检出。
```
[ymhe@iZ114e8q2mjZ git]$ git clone git@github.com:Ekimin/hello-world.git
Initialized empty Git repository in /home/ymhe/git/hello-world/.git/
remote: Counting objects: 13, done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 13 (delta 1), reused 3 (delta 0), pack-reused 0
Receiving objects: 100% (13/13), done.
Resolving deltas: 100% (1/1), done.
#clone的仓库会保存在当前目录下
[ymhe@iZ114e8q2mjZ git]$ ll
total 4
drwxr-xr-x 3 ymhe ymhe 4096 Sep 30 14:52 hello-world
```


- 进入目录，新建测试文件hello-world.php

```
[ymhe@iZ114e8q2mjZ git]$ cd hello-world/
[ymhe@iZ114e8q2mjZ hello-world]$ ll
total 4
-rw-rw-r-- 1 ymhe ymhe 145 Sep 30 14:52 README.md
[ymhe@iZ114e8q2mjZ hello-world]$ vim hello-world.php #随便写点东西进去
```

- 查看状态

```
[ymhe@iZ114e8q2mjZ hello-world]$ git status
# On branch master
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	hello-world.php
nothing added to commit but untracked files present (use "git add" to track)
```
由于新建的文件还没有提交到git仓库，所以显示为“Untracked files”。

- 提交

将hello-world.php提交至仓库,这样一来，这个文件就进入了版本管理系统的管理之下。今后的更改管理都交由Git进行。

```
[ymhe@iZ114e8q2mjZ hello-world]$ git add hello-world.php
[ymhe@iZ114e8q2mjZ hello-world]$ git commit -m "Add hello-world.php"
[master 9cd88ce] Add hello-world.php
 1 files changed, 3 insertions(+), 0 deletions(-)
 create mode 100644 hello-world.php
```

git add 命令将文件提交至暂存区(在Index数据结构中记录文件提交之前的状态。)，再通过git commit提交，-m参数后添加本次提交的备注

- 查看日志

```
[ymhe@iZ114e8q2mjZ hello-world]$ git log
commit 9cd88ce54ba90d64d5908b20dd4571ae6fdfda60
Author: Ekimin <heyiminwork@gmail.com>
Date:   Fri Sep 30 15:17:42 2016 +0800

    Add hello-world.php
```

- 进行push

push会更新github上的仓库。

```
[ymhe@iZ114e8q2mjZ hello-world]$ git push
Warning: Permanently added the RSA host key for IP address '192.30.253.113' to the list of known hosts.
Counting objects: 4, done.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 316 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To git@github.com:Ekimin/hello-world.git
   0223614..9cd88ce  master -> master
```

至此，修改的代码便在github上公开了。可以查看链接http://github.com/用户名/hello-world

----

# Git 常用命令汇总

## 用户信息配置
```git
git config --global user.name "Ekimin"
git config --global user.email Ekimin@example.com
```

如果用了 --global 选项，那么更改的配置文件就是位于你用户主目录下的那个，以后你所有的项目都会默认使用这里配置的用户信息。如果要在某个特定的项目中使用其他名字或者电邮，只要去掉 --global 选项重新配置即可，新的设定保存在当前项目的 .git/config 文件里。

## 查看配置

```git
git config --list
```

有时候会看到重复的变量名，那就说明它们来自不同的配置文件（比如 /etc/gitconfig 和 ~/.gitconfig），不过最终 Git 实际采用的是最后一个。

```git
$ git config user.name
ymhe
```

## 初始化 git init

```git
git init [-q | --quiet] [--bare] [--template=<template-directory>] [--shared[=<permissions>]] [<directory>]
```

- 在当前目录初始化一个仓库
```git
E:\IDEA\AlgoliaSearch>git init
Initialized empty Git repository in E:/IDEA/AlgoliaSearch/.git/
```
- 在当前目录初始化一个裸库
```git
git init --bare
```

## 添加远程仓库

```git
git remote add [<options>] <name> <url>
```

```git
E:\IDEA\AlgoliaSearch>git remote add origin git@github.com:Ekimin/AlgoliaSearch.git
```

## 第一次 push 到远程仓库

```git
E:\IDEA\AlgoliaSearch>git push --set-upstream origin master
Counting objects: 4, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 1.14 KiB | 0 bytes/s, done.
Total 4 (delta 0), reused 0 (delta 0)
To github.com:Ekimin/AlgoliaSearch.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
```

## 场景：在本地新建分支 dev，push到远程仓库（远程没有dev分支）

```git
E:\IDEA\AlgoliaSearch>git branch
* master

E:\IDEA\AlgoliaSearch>git branch dev

E:\IDEA\AlgoliaSearch>git branch
  dev
* master

E:\IDEA\AlgoliaSearch>git checkout dev
Switched to branch 'dev'

E:\IDEA\AlgoliaSearch>git push --set-upstream origin dev
Total 0 (delta 0), reused 0 (delta 0)
To github.com:Ekimin/AlgoliaSearch.git
 * [new branch]      dev -> dev
Branch dev set up to track remote branch dev from origin.
```

## 删除分支 dev

- 删除本地分支 dev
```git
git branch -d <branch-name>
git branch -D <branch-name> #强制删除（分支没有merge也会被删除）
```


```git
E:\IDEA\AlgoliaSearch>git branch
* dev
  master

E:\IDEA\AlgoliaSearch>git branch -d dev
error: Cannot delete branch 'dev' checked out at 'E:/IDEA/AlgoliaSearch'

E:\IDEA\AlgoliaSearch>git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.

E:\IDEA\AlgoliaSearch>git branch -d dev
Deleted branch dev (was 74a7280).

E:\IDEA\AlgoliaSearch>git branch
* master
```

- 删除远程分支 dev

```git
E:\IDEA\AlgoliaSearch>git push origin :dev
To github.com:Ekimin/AlgoliaSearch.git
 - [deleted]         dev
```

## add添加到缓存区、commit提交、push推送

```git
git add [<options>] [--] <pathspec>...
git commit [<options>] [--] <pathspec>...
git push [<options>] [<repository> [<refspec>...]]
```

```git
E:\IDEA\AlgoliaSearch>git status
On branch master
Your branch is up-to-date with 'origin/master'.
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        src/

nothing added to commit but untracked files present (use "git add" to track)

E:\IDEA\AlgoliaSearch>git add .

E:\IDEA\AlgoliaSearch>git commit -m "new class"
[master 978b221] new class
 1 file changed, 8 insertions(+)
 create mode 100644 src/main/java/com/wentuouto/algolia/app/Algolia.java

E:\IDEA\AlgoliaSearch>git push origin master
Counting objects: 10, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (10/10), 704 bytes | 0 bytes/s, done.
Total 10 (delta 0), reused 0 (delta 0)
To github.com:Ekimin/AlgoliaSearch.git
   74a7280..978b221  master -> master
```

## 查看远程分支
```git
E:\IDEA\AlgoliaSearch>git remote -v
origin  git@github.com:Ekimin/AlgoliaSearch.git (fetch)
origin  git@github.com:Ekimin/AlgoliaSearch.git (push)
```

----

# 参考
- [在GitHub上管理项目](http://www.cnblogs.com/mengdd/p/3447464.html)
- [Git版本控制软件结合GitHub从入门到精通常用命令学习手册](http://www.ihref.com/read-16369.html)
- [gitignore文件](https://github.com/github/gitignore/blob/master/Java.gitignore)
- [廖雪峰Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

# 错误汇总

##  git在push的时候出现insufficient permission for adding an object错误
http://blog.csdn.net/yujunf/article/details/7595231
