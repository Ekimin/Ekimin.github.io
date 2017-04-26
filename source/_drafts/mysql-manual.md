---
title: mysql 手册
date: 
tags: [Mysql, 数据库]
categories: 数据库
layout: post
updated: 
comments: 
permalink: 
---

mysql 错误汇总

<!--more-->

# 错误汇总

## mysql 启动报错：Can't open the mysql.plugin table

使用 service mysqld start 启动时失败：

> [root@WTTServer log]# service mysqld start
  MySQL Daemon failed to start.
  Starting mysqld:                                           [FAILED]

查看/var/log/mysqld.log日志发现：

> [ERROR] /usr/libexec/mysqld: Error writing file '/var/run/mysqld/mysqld.p170426 11:27:12 mysqld_safe Starting mysqld daemon with databases from /var/lib/mysql
  /usr/libexec/mysqld: Table 'mysql.plugin' doesn't exist
  [ERROR] Can't open the mysql.plugin table. Please run mysql_upgrade to create it.
  
**解决方法**

运行 mysql_install_db --user=mysql --ldata=/var/lib/mysql/  然后重新启动