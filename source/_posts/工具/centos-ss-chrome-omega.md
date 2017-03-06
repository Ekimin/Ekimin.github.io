---
title: CentOs安装配置SS+Chrome SwitchyOmega配置
tags:
  - Linux
  - ShadowSocks
  - SwithyOmega
  - 翻墙
categories: 工具
date: 2016-12-08 10:39:29
updated:
comments:
permalink:
---


目录


<!--more-->

# 第一步 安装Shadowsocks服务端

## 安装ss

```
[root@iZj6c2vq0s1w6wkap2geanZ ~]# yum install python-setuptools && easy_install pip 
Loaded plugins: security
base                                                                                                                                                                                                                                                   | 3.7 kB     00:00     
epel                                                                                                                                                                                                                                                   | 4.3 kB     00:00     
epel/primary_db                                                                                                                                                                                                                                        | 5.9 MB     00:05     
extras                                                                                                                                                                                                                                                 | 3.4 kB     00:00     
updates                                                                                                                                                                                                                                                | 3.4 kB     00:00     
updates/primary_db                                                                                                                                                                                                                                     | 3.7 MB     00:03     
Setting up Install Process
Package python-setuptools-0.6.10-3.el6.noarch already installed and latest version
Nothing to do
Searching for pip
Best match: pip 7.1.0
Adding pip 7.1.0 to easy-install.pth file
Installing pip script to /usr/bin
Installing pip2.6 script to /usr/bin
Installing pip2 script to /usr/bin

Using /usr/lib/python2.6/site-packages
Processing dependencies for pip
Finished processing dependencies for pip
```
```
[root@iZj6c2vq0s1w6wkap2geanZ ~]# pip install shadowsocks
/usr/lib/python2.6/site-packages/pip/_vendor/requests/packages/urllib3/util/ssl_.py:90: InsecurePlatformWarning: A true SSLContext object is not available. This prevents urllib3 from configuring SSL appropriately and may cause certain SSL connections to fail. For more information, see https://urllib3.readthedocs.org/en/latest/security.html#insecureplatformwarning.
  InsecurePlatformWarning
You are using pip version 7.1.0, however version 9.0.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
Collecting shadowsocks
  Downloading http://mirrors.aliyun.com/pypi/packages/02/1e/e3a5135255d06813aca6631da31768d44f63692480af3a1621818008eb4a/shadowsocks-2.8.2.tar.gz
Installing collected packages: shadowsocks
  Running setup.py install for shadowsocks
Successfully installed shadowsocks-2.8.2
```

## 创建配置文件/etc/shadowsocks.json
```
[root@iZj6c2vq0s1w6wkap2geanZ ~]# touch /etc/shadowsocks.json 
[root@iZj6c2vq0s1w6wkap2geanZ ~]# vim /etc/shadowsocks.json 
```
配置文件内容
```
{
"server":"47.90.56.123",    #服务器地址
"server_port":443,  #服务器端口
"local_address": "127.0.0.1", #本地地址
"local_port":1080,  #本地端口
"password":"myPass", #用来加密的密码
"timeout":600,  #超时时间
"method":"rc4-md5" #加密方法，可选择 “bf-cfb”, “aes-256-cfb”, “des-cfb”, “rc4″等。推荐rc4-md5，速度快
}
```


## 启动服务
### 使用配置文件在后台运行shadowsocks服务

```
[root@iZj6c2vq0s1w6wkap2geanZ ~]# ssserver -c /etc/shadowsocks.json  -d start
INFO: loading config from /etc/shadowsocks.json
2016-12-08 10:49:54 INFO     loading libcrypto from libcrypto.so.10
started
```

### 无配置文件启动

```
ssserver -p 443 -k MyPass -m rc4-md5 -d start
```

## 停止服务

```
[root@iZj6c2vq0s1w6wkap2geanZ ~]# ssserver -c /etc/shadowsocks.json  -d stop
INFO: loading config from /etc/shadowsocks.json
stopped
```

至此，服务器就安装好了

----

# Chrome Omega配置
![](http://oduq3lfcc.bkt.clouddn.com/swithyomega1.png)

![](http://oduq3lfcc.bkt.clouddn.com/swithyomega2.png)

</br>

AutoProxy规则列表：https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt