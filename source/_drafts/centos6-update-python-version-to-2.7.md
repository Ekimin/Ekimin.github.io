---
title: centos6更新Python
date: 
tags: [Python]
categories: Python
layout: post
updated: 
comments: 
permalink: 
---

CentOs 6 自带的python版本是2.6的，本文记录了升级到2.7以及pip安装和处理yum依赖的方法。

<!--more-->

# 一、下载依赖
```
yum  install -y zlib zlib-devel
yum install -y gcc
yum install -y openssl
yum install -y openssl-devel
```

# 二、安装python2.7

## 1、下载

```
wget http://www.python.org/ftp/python/2.7.10/Python-2.7.10.tgz
```

## 2、解压

```aidl
tar -xvf Python-2.7.10.tar
```

## 3、编译安装

- 创建安装目录

```
mkdir /usr/local/python2.7
```

- 编译

```
cd Python-2.7.10 #进入解压后的Python目录
./configure --prefix=/usr/local/python2.7  #等待编译完成
make && make install #等待安装
```

- 修改原来的python2.6

```
mv /usr/bin/python /usr/bin/python2.6 #将原版本的python更换一个名字
#创建python2.7的软连接
ln -s /usr/local/python2.7/bin/python /usr/bin/python
```

# 三、安装pip

```
wget https://bootstrap.pypa.io/get-pip.py
python get-pip.py
```

# 四、修改yum

由于yum依赖python2.6，所以修改yum头文件，指定其使用2.6版本的python

```
vim /usr/bin/yum
#!/usr/bin/python--> #!/usr/bin/python2.6
```