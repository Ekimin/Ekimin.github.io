---
title: IntelliJ IDEA 笔记
date: 2017-01-16 14:13:57
tags: [IDEA, 乱码]
categories: 工具
---

本文记录了IDEA的使用配置和遇到的问题，方便后面查询

<!--more-->

# IDEA 集成SVN
- 环境：

OS：Windows 10 (64位)

IDEA版本：IntelliJ IDEA 2016.3.4 Build #IU-163.12024.16, built on January 31, 2017

- 下载安装

[SVN官网](https://tortoisesvn.net/)下载SVN并安装。

**注意：安装时需要选择安装command line client tools, 默认这一项是不安装的。如果不安装，那么后面找不到svn.exe这个文件**

![](http://oduq3lfcc.bkt.clouddn.com/IDEA-SVN%E5%AE%89%E8%A3%85.png)

- 在IDEA中配置SVN

File-settings-subversion, 勾选Use command line client，选择刚刚安装的SVN中的svn.exe。(如果在bin下找不到这个文件，返回上一步检查安装的时候是否选择安装了command line client tool)

![](http://oduq3lfcc.bkt.clouddn.com/IDEA-SVN%E5%AE%89%E8%A3%85-setting.png)

# 乱码问题

**IDEA乱码一般以下几类，按照如下设置后基本都可以解决**

## 全局设置

打开File-Setting, 找到File Encodings这个选项，把encoding设置成你工程的编码即可，一般是UTF-8。也可以在右下角快捷改变当前文件的编码。改变之后会提示你reload文件。

## 控制台乱码

- 执行main函数时乱码

同样是打开setting，找到 Build,Execution,Deployment > Compiler > Java Compiler， 设置 Additional command line parameters选项为 -encoding utf-8，然后rebuild下，重新运行

- 运行tomcat等容器时乱码

打开Run/Debug Configuration,选择你的tomcat

然后在  Server > VM options 设置为 -Dfile.encoding=UTF-8 ，重启tomcat

## 如果上面不管用，可以尝试下面方法修改配置文件
打开IDEA安装目录找到 idea.exe.vmoptions （64位为 idea64.exe.vmoptions ）文件， 在文件末尾加上一行 -Dfile.encoding=UTF-8
然后重启IDEA，一般问题都可以解决

# IDEA 激活
[激活服务器](http://idea.lanyus.com/)