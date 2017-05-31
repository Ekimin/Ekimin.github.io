---
title: 
date: 
tags: 
categories: 
layout: 
updated: 
comments: 
permalink: 
---

安装Django之前，需要安装好python环境，参考python环境安装。

<!--more-->

## 安装Django

```bash
(fswtt) [root@WTTServer bin]# pip3 install django
Collecting django
  Downloading Django-1.11-py2.py3-none-any.whl (6.9MB)
    100% |████████████████████████████████| 6.9MB 108kB/s 
Collecting pytz (from django)
  Downloading pytz-2017.2-py2.py3-none-any.whl (484kB)
    100% |████████████████████████████████| 491kB 1.4MB/s 
Installing collected packages: pytz, django
Successfully installed django-1.11 pytz-2017.2
```

安装完后，我们就可以使用Django自带的django-admin命令。django-admin是Django自带的一个管理任务的命令行工具。

django-admin用法如下：

django-admin <command> [options]

命令包括：

```text
[django]
    check
    compilemessages
    createcachetable
    dbshell
    diffsettings
    dumpdata
    flush
    inspectdb
    loaddata
    makemessages
    makemigrations
    migrate
    runfcgi
    runserver
    shell
    sql
    sqlall
    sqlclear
    sqlcustom
    sqldropindexes
    sqlflush
    sqlindexes
    sqlinitialdata
    sqlmigrate
    sqlsequencereset
    squashmigrations
    startapp
    startproject
    syncdb
    test
    testserver
    validate
```

## 创建项目

在 /home/workspace 下创建一个名为blog的项目
```bash
(fswtt) [root@WTTServer workspace]# django-admin startproject blog
(fswtt) [root@WTTServer workspace]# cd blog/
(fswtt) [root@WTTServer workspace]# ll
total 4
drwxr-xr-x 3 root root 4096 May  4 14:37 blog
```

blog 目录下：
```text
.
├── blog
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── manage.py
```

blog目录对应的就是blog这个项目，将会放置这个项目的一些相关配置：
1. settings.py包含了这个项目的相关配置。如数据库环境、启用的插件等等。
2. urls.py即URL Dispatcher的配置，指明了某个URL应该指向某个函数来处理。
3. wsgi.py用于部署。WSGI（Python Web Server Gateway Interface，Web服务器网关接口）是为Python语言定义的Web服务器和Web应用程序或框架之间的一种简单而通用的接口。
4. __init__.py指明了这是一个Python模块。

manage.py 会在每个Django项目中自动生成，它可以和django-admin做类似的事。如我们可以用manage.py来启动测试环境的服务器：



# 问题

## python manage.py runserver 出现 No module named '_sqlite3'

1，首先安装 sqlite-devel
```bash
yum install sqlite-devel
```

2，重新编译安装Python

```bash
./configure
make
make install 
```