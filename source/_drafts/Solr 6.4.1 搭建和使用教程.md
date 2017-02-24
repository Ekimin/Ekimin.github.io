title: Solr 6.4.1 搭建和使用教程
date: 
tags: [Solr]
categories: 大数据
layout: 
updated: 
comments: 
permalink: 
---


<!--more-->

# Solr 搭建

## 环境

Solr：solr-6.4.1.zip
JDK：1.8.0_111
tomcat：8.5.1 （8版本都支持）
OS：Windows 10


## 通过tomcat容器部署solr服务

**Solr目录结构**

![](http://oduq3lfcc.bkt.clouddn.com/solr%E7%9B%AE%E5%BD%95.png)


1. 将solr-6.4.1/server/solr-webapp 下的webapp文件夹复制到 tomcat/webapps/ 目录下，并重命名为 solr

2. 将solr-6.4.1/server/lib/ext 下所有jar包复制到 tomcat/webapps/solr/WEB-INF/lib 下。

3. 将solr-6.4.1/server/dist/ 下 solr-dataimporthandler-6.4.1.jar 和 solr-dataimporthandler-extras-6.4.1.jar 也复制到tomcat/webapps/solr/WEB-INF/lib 下。（若实际使用中需要用到其他包，在自行导入）

4. 将solr-6.4.1/server/下的solr 文件夹复制到另外一个目录，比如我这里放到了E盘，并重命名为solrhome.

5. 


