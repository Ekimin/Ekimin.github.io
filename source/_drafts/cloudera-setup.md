---
title: CDH集群搭建教程
date:
tags: [大数据, CDH, Cloudera, 集群]
categories: 大数据
layout: post
updated:
comments:
permalink:
---

搭建环境:

- 服务器：阿里云服务器4台

<table>
<tr>
<th>服务器</th><th>内存</th><th>硬盘</th><th>IP(内网)</th>
<tr>
<tr>
<td>cloudera01</td><td>2G</td><td>40G</td><td>10.27.162.28</td>
</tr>
<tr>
<td>cloudera02</td><td>2G</td><td>40G</td><td>10.28.98.124</td>
</tr>
<tr>
<td>cloudera03</td><td>2G</td><td>40G</td><td>10.28.98.144</td>
</tr>
<tr>
<td>cloudera04</td><td>2G</td><td>40G</td><td>10.28.98.134</td>
</tr>
</table>

```
[root@iZuf61s6x9zl5wsik38xgaZ ~]# lsb_release -a
LSB Version:	:core-4.1-amd64:core-4.1-noarch
Distributor ID:	CentOS
Description:	CentOS Linux release 7.2.1511 (Core)
Release:	7.2.1511
Codename:	Core
```

<!--more-->

# 集群规划

<table>
<tr>
<th></th><th>clouldera01</th><th>clouldera02</th><th>clouldera03</th><th>clouldera04</th>
<tr>
<tr>
<td>Cloudera Manager</td><td>CM主节点</td><td></td><td></td><td></td>
</tr>
<tr>
<td></td><td></td><td></td><td></td><td></td>
</tr>
<tr>
<td></td><td></td><td></td><td></td><td></td>
</tr>
<tr>
<td></td><td></td><td></td><td></td><td></td>
</tr>
</table>


# CentOs 系统基本设置

**本文环境**
