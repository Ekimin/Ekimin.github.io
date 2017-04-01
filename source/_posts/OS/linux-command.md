---
title: Linux常用命令
tags:
  - Linux
  - OS
categories: 操作系统
date: 2015-12-05 11:25:05
updated:
comments:
permalink:
---


一些linux常用命令

<!--more-->

# 系统

## 查看系统版本

***
* 查看Linux版本信息
```shell
[root@test-server etc]# cat /etc/issue
CentOS release 6.7 (Final)
Kernel \r on an \m
```

* 查看LSB本身的版本信息
```shell
ymhe@ymhe-Amarsoft ~ $ lsb_release -a
No LSB modules are available.
Distributor ID:	LinuxMint
Description:	Linux Mint 17.1 Rebecca
Release:	17.1
Codename:	rebecca
```

* 查看Kernel版本
```shell
[root@test-server etc]# uname -r
2.6.32-573.el6.x86_64
```

* 查看系统发行号
```shell
cat /proc/version
Linux version 3.13.0-37-generic (buildd@kapok) (gcc version 4.8.2 (Ubuntu 4.8.2-19ubuntu1) ) #64-Ubuntu SMP Mon Sep 22 21:28:38 UTC 2014
```
# 文件
## 查找文件
***
* **whereis [-bmsu] 文件或目录名**
参数：
-b  :只查找二进制格式的文件
-m  :只在说明文件manual路径下查找
-s  :只查找source源文件
-u  :查找不在上述三个选项中的其他特殊文件
范例：
```shell
[root@test-server etc]# whereis passwd
passwd: /usr/bin/passwd /etc/passwd /usr/share/man/man1/passwd.1.gz
[root@test-server etc]# whereis -m passwd
passwd: /usr/share/man/man1/passwd.1.gz
[root@test-server etc]# whereis -b passwd
passwd: /usr/bin/passwd /etc/passwd
[root@test-server etc]# whereis -m passwd
passwd: /usr/share/man/man1/passwd.1.gz
[root@test-server etc]# whereis -u passwd
passwd: /usr/bin/passwd /etc/passwd /usr/share/man/man1/passwd.1.gz
```
* **locate [ir] 文件名关键字**
参数：
-i  :忽略大小写
-r  :后面可接正则表达式

* **find [PATH] [option] [action]**

## 解压缩文件
***
### tar [-cxtzjvfpPN] 文件与目录 ....
* 参数：
-c ：建立一个压缩文件的参数指令(create 的意思)；
-x ：解开一个压缩文件的参数指令！
-t ：查看 tarfile 里面的文件！  
**特别注意，在参数的下达中，c/x/t仅能存在一个！不可同时存在！因为不可能同时压缩与解压缩。**
-z ：是否同时具有 gzip 的属性？亦即是否需要用 gzip 压缩？
-j ：是否同时具有 bzip2 的属性？亦即是否需要用 bzip2 压缩？
-v ：压缩的过程中显示文件！这个常用，但不建议用在背景执行过程！
-f ：使用档名，请留意，在f之后要立即接档名喔！不要再加参数！例如使用『tar -zcvfP tfile sfile』就是错误的写法，要写成『tar -zcvPf tfile sfile』才对喔！
-p ：使用原文件的原来属性（属性不会依据使用者而变）
-P ：可以使用绝对路径来压缩！
-N ：比后面接的日期(yyyy/mm/dd)还要新的才会被打包进新建的文件中！
--exclude FILE：在压缩的过程中，不要将 FILE 打包！
* 范例：
将整个 /etc目录下的文件全部打包成为/tmp/etc.tar
```shell
[root@linux ~]# tar -cvf /tmp/etc.tar /etc <==仅打包，不压缩！
[root@linux ~]# tar -zcvf /tmp/etc.tar.gz /etc <==打包后，以 gzip 压缩
[root@linux ~]# tar -jcvf /tmp/etc.tar.bz2 /etc <==打包后，以 bzip2 压缩
```


### Zip/Unzip
* zip [参数] [打包后的文件名] [打包的目录路径]
参数：
-a 将文件转成ASCII模式
-F 尝试修复损坏的压缩文件
-h 显示帮助界面
-m 将文件压缩之后，删除源文件
-n 特定字符串 不压缩具有特定字尾字符串的文件
-o 将压缩文件内的所有文件的最新变动时间设为压缩时候的时间
-q 安静模式，在压缩的时候不显示指令的执行过程
-r 将指定的目录下的所有子目录以及文件一起处理
-S 包含系统文件和隐含文件（S是大写）
-t 日期 把压缩文件的最后修改日期设为指定的日期，日期格式为mmddyyyy

* unzip [参数] zip文件
简单例子：
把本目录下的test.zip文件解压：unzip test.zip
参数列表：
-l 列出压缩文件所包含的内容
-v 显示详细的执行过程

## 文件权限管理

三种基本权限
R           读         数值表示为4
W          写         数值表示为2
X           可执行  数值表示为1

### 更改权限
sudo chmod [u所属用户  g所属组  o其他用户  a所有用户]  [+增加权限  -减少权限]  [r  w  x]   目录名
例如：有一个文件filename，权限为“-rw-r----x” ,将权限值改为"-rwxrw-r-x"，用数值表示为765
sudo chmod u+x g+w o+r  filename
上面的例子可以用数值表示
sudo chmod 765 filename


# 用户和组
默认Linux中的组名记录在/etc/group内；用户相关信息记录在/etc/passwd;密码记录在/etc/shadow。

## 权限

- 修改密码

``` Shell
ymhe@he-desktop ~ $ passwd
Changing password for ymhe.
(current) UNIX password:
Enter new UNIX password:
Retype new UNIX password:
```

## 组

- 更改用户组
``` Shell
chgrp user1 install.log
```

----

# 进程管理

## 查看占用内存/CPU最多的进程
```bash
可以使用一下命令查使用内存最多的5个进程
ps -aux | sort -k4nr | head -5
或者
top （然后按下M，注意大写）
可以使用一下命令查使用CPU最多的5个进程
ps -aux | sort -k3nr | head -5
或者
top （然后按下P，注意大写）
```

## 释放内存

- 使用sync命令，将系统缓存区中的脏数据写入磁盘中，包括已修改的i-node、已延迟的块I/O和读写映射文件，命令如下：

```bash
[root@Cloud ~]# sync
```

- 查看释放缓存参数的配置：

```bash
[root@Cloud ~]# cat /proc/sys/vm/drop_caches
0
```

0 代表不自动释放

- 设置自动释放

```bash
[root@Cloud ~]# echo 3 > /proc/sys/vm/drop_caches
```

让配置生效（不需要重启）

```bash
[root@Cloud ~]# sysctl -p
```


----

# 其他

## awk

http://www.cnblogs.com/ggjucheng/archive/2013/01/13/2858470.html
