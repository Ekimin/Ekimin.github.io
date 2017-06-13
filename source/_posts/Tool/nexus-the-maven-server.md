---
title: 用 nexus 3.X 搭建 Maven 私服
tags:
  - Maven
  - Nexus
  - 工具
categories: 工具
date: 2017-04-06 16:22:48
updated:
comments:
permalink:
---


环境：
CentOs 6.4 
JDK 1.8.121

<!--more-->

# 安装jdk

如果没有jdk，需要安装jdk。 参考：[安装JDK](http://wentuotuo.com/2016/01/14/Java/JDK-install/)
nexus 2.x 版本需要jdk1.7
nexus 3.x 版本需要jdk1.8

# 安装nexus

- 下载nexus： https://www.sonatype.com/download-oss-sonatype

- 解压后得到两个目录：(一个 nexus 服务，一个私有库目录)

```bash
drwxr-xr-x 9 root root 4096 Apr  6 15:32 nexus-3.2.1-01
drwxr-xr-x 3 root root 4096 Apr  6 15:32 sonatype-work
```

- 配置文件 nexus-default.properties 路径在./etc/，这一点和2.x版本区别很大。可以根据情况修改配置文件

- 启动
```bash
[root@WTTServer bin]# pwd
/opt/nexus/nexus-3.2.1-01/bin
[root@WTTServer bin]# ./nexus start &
[1] 2209
[root@WTTServer bin]# WARNING: ************************************************************
WARNING: Detected execution as "root" user.  This is NOT recommended!
WARNING: ************************************************************
Starting nexus

[1]+  Done                    ./nexus start
```

![](http://oduq3lfcc.bkt.clouddn.com/image/nexuui.png)

# 进阶设定

## 修改管理员用户密码

默认管理员用户名/密码为 admin/admin123, 修改密码：登录UI界面，点击用户设置，修改密码。

## 重置管理员密码

http://blog.csdn.net/tianya6607/article/details/53305620

## nexus 虚拟机设定

./bin/nexus.vmoptions 文件设定了nexus虚拟机参数，包括运行最大最小内存等等。默认内存是1200M，一般个人用不到这么多，果断调到500

## 上传第三方 jar

http://blog.csdn.net/wang465745776/article/details/52527905

```bash
C:\Users\ymhe>mvn deploy:deploy-file -DgroupId=com.software -DartifactId=artid -Dversion=1.1.1 -Dpackaging=jar -Dfile=
F:\artid-1.1.1.jar -Durl=http://XXXXX.com:8081/xxxx/maven-thirdparty/ -DrepositoryId=nexus
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] ------------------------------------------------------------------------
[INFO]
...
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
```

# 常用命令

- /usr/local/nexus/nexus/bin/nexus {start|stop|run|run-redirect|status|restart|force-reload}

# 参考资料

http://www.cnblogs.com/kevingrace/p/6201984.html