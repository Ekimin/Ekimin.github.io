---
title: 金科代码管理中心使用手册
tags:
  - Gitlab
  - Git
  - 工具
categories: 工具
date: 2017-10-24 14:18:35
updated:
comments:
permalink:
---


|序号|版本号|修订日期|修订人|修订内容|备注|
|:--:|:----:|:------:|:----:|:------:|:--:|
| 1  | V1.0 | 2017-10-23 | 何益民 |  ||

<!--more-->


## 简介

金科代码管理中心采用 Git 作为版本控制工具，旨在提供高效、稳定、安全的代码管理，沉淀知识，方便开发者协作等功能。

**测试地址： http://192.168.9.75**
**正式地址： http://192.168.9.83**

PS：测试环境可自由申请账号测试使用，正式环境账号请联系管理员开通。

## 关于 Git

提到版本控制，可能最先联想到的是 SVN，但是两者有本质区别。
</br>
svn是集中化的版本控制系统, 只有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。
</br>
git是分布式的版本控制系统, 每一个终端都是一个仓库，客户端并不只提取最新版本的文件快照，而是把原始的代码仓库完整地镜像下来。每一次的提取操作，实际上都是一次对代码仓库的完整备份。
</br>
关于 git 的更多知识，这里不再赘述，请参考相关教程或自行搜索了解。
</br>
**对于没有使用过git的用户，这里推荐几篇教程：**

[Git教程 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
[猴子都能懂的Git教程](https://backlog.com/git-tutorial/cn/)
[Git基本操作](http://www.runoob.com/git/git-basic-operations.html)
[Git和SVN区别](http://blog.jobbole.com/31444/)

----

## 正式开始

了解了 Git 的基本概念和操作后，下面正式进入金科代码管理中心的使用。

### 上传 sshkey

**Tips: 没有上传sshkey的电脑无法下载和上传代码，但不影响登录，设置等常规操作**

git仓库之间的代码传输协议主要使用ssh协议。需要使用ssh-keygen上传公钥，使用非对称加密传输。下面讲述如何上传你的ssh公钥。

linux/mac 系统自带ssh-keygen工具，直接在终端中操作即可。
Windows 用户需要先下载安装 Git工具，下载地址： [官方下载地址](https://git-scm.com/downloads)  [备用下载地址](http://file.wentuotuo.com/Git-2.14.2.3-64-bit.exe)

**第一步：生成公钥：**
Windows用户安装好Git工具后，右键任意位置选择 gitbash here 打开终端。Mac、Linux直接打开终端：
![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/git%E5%8F%B3%E9%94%AE.png)

```
ssh-keygen -t rsa -C "your comment here"
```

- ssh-keygen: 生成ssh公钥的工具
- -t 参数： 加密方式，这里选择 rsa 公钥密码
- -C 参数： 备注，可以填写自定义信息便于记忆，最好使用英文备注信息以免出现乱码问题

输入命令后，一路回车，直到出现以下信息，则表明创建成功：

```
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
Created directory '/root/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
3e:5b:e3:f8:cc:f1:67:dd:9f:85:11:5b:23:23:d3:61 your comment here
The key's randomart image is:
+--[ RSA 2048]----+
|             E   |
|            o .  |
|           o +...|
|            o o+.|
|        S     o  |
|       .       o |
|        o +   ..o|
|         O +  o =|
|        o.= .o .o|
+-----------------+
```

生成的公钥存储在： {个人主目录}/.ssh/id_rsa.pub 文件中。
- Windows用户：C:\Users\{你的用户名}\.ssh\id_rsa.pub
- Linux & Mac：/home/{你的用户名}/.ssh/id_rsa.pub

可以直接打开文件或用下面命令查看公钥：（由于.ssh文件夹是隐藏文件夹，所以需要在控制面板-文件夹选项中打开隐藏文件显示才能看见。所以这里推荐直接干命令）
```
cd
cat .ssh/id_rsa.pub
```
![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/rsakey.png)


**第二步：将公钥添加到代码中心**

点击右上角头像，选择Settings，然后在左边菜单列选择 SSH KEYS，将刚刚生成的公钥复制到key下面文本框，Title自定义标题，方便记忆，点击 add key完成添加。

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/%E4%B8%AA%E4%BA%BA%E8%AE%BE%E7%BD%AE1.png)

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/SSH%20KEYS.png)

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/sshkey2.png)
