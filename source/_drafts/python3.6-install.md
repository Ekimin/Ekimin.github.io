---
title: python 环境安装
date: 
tags: [Python]
categories: Python
layout: post
updated: 
comments: 
permalink: 
---

环境：
centos：6.5


<!--more-->

- 安装依赖：
```bash
[root@WTTServer bin]# yum install openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel
```

- 下载安装包：

```bash
[root@WTTServer software]# wget https://www.python.org/ftp/python/3.6.0/Python-3.6.0.tgz
```

- 解压：
```bash
[root@WTTServer software]# tar -zxvf Python-3.6.0.tgz 

[root@WTTServer software]# ll
total 21768
drwxr--r-- 17 git  games     4096 Dec 23 10:21 Python-3.6.0
-rw-r--r--  1 root root  22256403 Dec 23 10:24 Python-3.6.0.tgz
```

- 把Python3.6安装到 /usr/local 目录:
```bash
[root@WTTServer software]# cd Python-3.6.0
[root@WTTServer Python-3.6.0]# ./configure --prefix=/usr/local

[root@WTTServer Python-3.6.0]# make

[root@WTTServer Python-3.6.0]# make altinstall
```

- 安装完成后目录：

python3.6程序的执行文件：/usr/local/bin/python3.6
python3.6应用程序目录：/usr/local/lib/python3.6
pip3的执行文件：/usr/local/bin/pip3.6
pyenv3的执行文件：/usr/local/bin/pyenv-3.6


- 更改/usr/bin/python链接：

```bash
[root@WTTServer bin]# cd /usr/bin
[root@WTTServer bin]# mv  python python.backup
[root@WTTServer bin]#  ln -s /usr/local/bin/python3.6 /usr/bin/python
[root@WTTServer bin]#  ln -s /usr/local/bin/python3.6 /usr/bin/python3
[root@WTTServer bin]# ln -s /usr/local/bin/pip3.6 /usr/bin/pip3
```

## 安装virtualenv

```bash
[root@WTTServer bin]# pip3 install virtualenv
```

默认安装到：/usr/bin/virtualenv

- 在创建我们的这个虚拟环境之前，我们可以创建一个存储所有virtualenv的目录：
```bash
[root@WTTServer home]# cd /home
[root@WTTServer home]# mkdir env
[root@WTTServer home]# cd env/
[root@WTTServer env]# mkdir virtualenv
```

- 创建一个新的虚拟环境：

```bash
[root@WTTServer env]# cd virtualenv/
[root@WTTServer virtualenv]# virtualenv ./fswtt --no-site-packages
Using base prefix '/usr/local'
New python executable in /home/env/virtualenv/fswtt/bin/python
Installing setuptools, pip, wheel...done.
```

PS:如果你想使用不同的Python版本的话，那么需要指定Python版本的路径:
$ virtualenv --distribute -p /usr/local/bin/python3.3 somewhere/virtualenvs/<project-name>


- 通过到相应的目录下执行激活就可以使用这个虚拟环境了：

```bash
[root@WTTServer virtualenv]# cd fswtt
[root@WTTServer fswtt]# cd bin
[root@WTTServer bin]# source activate
(fswtt) [root@WTTServer bin]# 
```

可以看到shell环境开始变成（fswtt）了，表示当前环境已经切换到fswtt环境。

- 停止使用只使用执行下面的命令即可：

```bash
(fswtt) [root@WTTServer bin]# deactivate
[root@WTTServer bin]# 
```

