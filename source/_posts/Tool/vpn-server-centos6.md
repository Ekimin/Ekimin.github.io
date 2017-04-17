---
title: 在 VPS 上搭建 VPN 服务
tags:
  - VPN
categories: 工具
date: 2017-04-17 17:13:30
updated:
comments:
permalink:
---


在 VPS 上搭建 VPN 服务。
环境：CentOs6.4

<!--more-->

# 原理
</br>
1.云服务器上架设VPN.
2.本地连接使用VPN拨号到云服务器.
3.使用云服务器网络访问网络资源.

# 检查

如果检查结果没有这些支持的话，是不能安装pptp的。执行指令：

```bash
[root@WTTServer ~]# modprobe ppp-compress-18 && echo ok
ok
```

如果显示 ok, 表明可以安装pptp服务。下面继续检查：

```bash
[root@WTTServer ~]# cat /dev/net/tun
cat: /dev/net/tun: File descriptor in bad state
```
上面两项检查通过才可以安装pptp服务。

检查系统是否已经集成了ppp服务
```bash
#rpm -q ppp
```

# 架设VPN
## 安装 ppp pptpd iptables

- 1.安装ppp和iptables

PPTPD要求Linux内核支持mppe，一般来说CentOS安装时已经包含了

```bash
#yum install perl ppp iptables //centos默认安装了iptables和ppp
#chkconfig iptables on
#service iptables start
```

- 2.安装pptpd
```bash
[root@WTTServer ~]# rpm -Uvh http://poptop.sourceforge.net/yum/stable/rhel6/pptp-release-current.noarch.rpm
Retrieving http://poptop.sourceforge.net/yum/stable/rhel6/pptp-release-current.noarch.rpm
warning: /var/tmp/rpm-tmp.70X8xV: Header V3 DSA/SHA1 Signature, key ID 862acc42: NOKEY
Preparing...                ########################################### [100%]
   1:pptp-release           ########################################### [100%]
[root@WTTServer ~]# yum install pptpd

[root@WTTServer ~]# chkconfig pptpd on
[root@WTTServer ~]# service pptpd start
Starting pptpd:                                            [  OK  ]
```

以下命令检查PPP是否支持MPPE：

# 配置

- 开启路由转发

```bash
[root@WTTServer ~]# vim /etc/sysctl.conf
```

修改
net.ipv4.ip_forward = 1

执行下面命令让修改的内核参数生效
```bash
[root@WTTServer ~]# sysctl -p
```

- 修改/etc/ppp/options.pptpd  

```
$ sudo vim /etc/ppp/options.pptpd  
```
找到66和67行进行修改
```
#ms-dns 10.0.0.1  
#ms-dns 10.0.0.2  
```

- 配置后结果如下:
```
ms-dns 8.8.8.8  
ms-dns 8.8.4.4  
```


- 配置 ppp VPN账号信息
编辑 chap-secrets 配置文件,这个文件默认只有两行注释,在注释下配置VPN账号信息
```
vim /etc/ppp/chap-secrets  
```
设置规则为 VPN账号 服务类型 VPN密码 IP,若IP为*则代表所有IP都可以使用该账号密码,配置后结果如下:
```
# Secrets for authentication using CHAP  
# client    server  secret          IP addresses  
ymhe   pptpd   123456       *  
```


- iptables 配置

- 清空防火墙配置
```
$ sudo iptables -P INPUT ACCEPT #改成 ACCEPT 表示接收一切请求  
$ sudo iptables -F #清空默认所有规则  
$ sudo iptables -X #清空自定义所有规则  
$ sudo iptables -Z #计数器置0  
```
- 配置规则
```
$ sudo iptables -A INPUT -i lo -j ACCEPT #允许127.0.0.1访问本地服务  
$ sudo iptables -A INPUT -m state --state ESTABLISHED -j ACCEPT #允许访问外部服务  
$ sudo iptables -A INPUT -p icmp -m icmp --icmp-type 8 -j ACCEPT #允许 ping  
$ sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT #开启 ssh 端口,若需要配置其他端口,比如 80 3306 8080 参考 ssh 端口配置方式
```


## 收尾工作
```
$ sudo service iptables restart #重新启动 iptables  
$ sudo service pptpd restart #重新启动 pptpd  
$ sudo chkconfig iptables on #开机启动 iptables  
$ sudo chkconfig pptpd on #开机启动 pptpd  
$ sudo iptables -P INPUT DROP #加载防火墙策略  
```

# 一些命令

## 查看当前在线用户
```
[root@iZj6c2vq0s1w6wkap2geanZ ~]# last | grep still | grep ppp
ymhe     ppp2         117.82.240.219   Thu Nov  3 16:39   still logged in
```

# 参考
[阿里云服务器配置VPN详解](https://bbs.aliyun.com/read/162297.html)
[阿里云centos配置VPN(PPTPD服务)](http://blog.sina.com.cn/s/blog_3eba8f1c0102uxyc.html)
[已解决，结帖Re阿里云Centos6.3搭了VPN内网通，但无法访问外网，折腾3天。](https://bbs.aliyun.com/read/163732.html?ordertype=desc&displayMode=1)
[阿里云Centos配置iptables防火墙](http://blog.kkyun.com/?p=50)