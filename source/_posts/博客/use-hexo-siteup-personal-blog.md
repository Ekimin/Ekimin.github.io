---
title: 用 Hexo 搭建个人博客
date: 2017-02-05 11:55:13
tags: [Hexo, Github, 博客]
categories: 博客
layout: post
updated:
comments:
permalink:
---

目录：
- Hexo 博客原理
- Hexo + Github 搭建个人博客
- Hexo + VPS 搭建个人博客
- 博客备份和还原
- 主题优化


<!--more-->

# Hexo 博客原理

## Hexo简介

Hexo 是一个简单地、轻量地、基于Node的一个静态博客框架，可以将支持的类型的文件转换成静态Web页面（html+css+JavaScript），hexo是不支持动态页面的（如jsp等）。
下面是一张架构图：
![](http://oduq3lfcc.bkt.clouddn.com/hexo%E5%8D%9A%E5%AE%A2%E6%9E%B6%E6%9E%84.png)

大致流程：将 *.md 渲染成静态文件，然后Git推送到服务器的repository,服务器再通过 git-hooks 同步网站根目录。

## 搭建流程

了解了原理，不难理解 Hexo 博客的搭建流程，大致以下几步。
- 服务器环境搭建，包括安装 Git 、Nginx配置 、创建 git 用户 。
- 本地 Hexo 初始化， 包括安装 NodeJS 、Hexo-cli, 生成本地静态网站
- 使用Git自动化部署发布博客

**注：如果使用 Github 作为远程博客仓库，那么第一步就省了，因为 Github 的程序员已经帮我们做好了。**

# Hexo + github 搭建个人博客

使用 Github 作为远程博客仓库优势多多，首先繁琐的服务器环境搭建不用我们操心了，而且也不用担心服务器挂掉，当然免费也是一项大优势。
具体搭建过程参考本人另一篇博文：[传送门](http://wentuotuo.com/2016/10/13/%E5%8D%9A%E5%AE%A2/hexo-blog-based-on-github/)

# Hexo + VPS 搭建个人博客

如果你有个人VPS，想把博客搭建到自己的服务器上，那么在上面步骤之前还需要在自己服务器上部署一些环境

本文测试环境：
VPS：阿里云香港服务器一台
系统：CentOs 7

## 安装 Git

终端直接输入 git 查看是否安装有（默认CentOs 会自带git），如果没有，那么先安装 git。

```bash
yum install git
```
## 安装 NodeJS

根据你的VPS操作系统选择安装不同的版本，官方安装文档：[传送门](https://nodejs.org/en/download/package-manager)

我的OS是CentOs的，所以我选择 ArchLinux，然后里面定位到CentOs的安装命令：

![](http://oduq3lfcc.bkt.clouddn.com/nodejs%E4%B8%8B%E8%BD%BD.png)

```bash
curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
yum install nodejs
```

## 安装 hexo

```bash
[root@iZj6c2vq0s1w6wkap2geanZ blog.git]# npm install -g hexo
```


## 新建 git 用户

**虽然用 root 也能完成，但是为了安全起见，我们还是应该新建一个 git 用户来专门进行博客的管理，而不是全部事情都丢给 root。**

- 查看系统内是否已经有 git 用户

```bash
# cat /etc/passwd
```
### 新建 git 用户

新建一个名叫 git 的用户，如果之前已经建立了，则跳过此命令
```bash
adduser git
```

### 给 git 用户添加 sudo 权限

```bash
chmod 740 /etc/sudoers
vim /etc/sudoers
```
定位到以下内容：
```bash
## Allow root to run any commands anywhere
root    ALL=(ALL)     ALL
```


然后在下面新增一行:

```bash
git ALL=(ALL) ALL
```

添加后效果如图：

![](http://oduq3lfcc.bkt.clouddn.com/git%E9%85%8D%E7%BD%AEsudoer%E6%9D%83%E9%99%90.png)


保存编辑后退出。

**PS：如果不熟悉VI或者VIM的使用，参考[Linux vi/vim编辑器常用命令与用法总结](http://www.cnblogs.com/jiayongji/p/5771444.html)**

别忘了把文件权限改回去（还是为了安全）：

```bash
chmod 400 /etc/sudoers
```

### 给 git 用户设置密码

```bash
sudo passwd git #该命令执行后输入两次要设置的密码
```

### 创建证书登录

把自己电脑的公钥，也就是 ~/.ssh/id_rsa.pub 文件里的内容添加到服务器的 /home/git/.ssh/authorized_keys 文件中，添加公钥之后可以防止每次 push 都输入密码

例如，我的Windows电脑里面的id_rsa.pub文件存放在：
![](http://oduq3lfcc.bkt.clouddn.com/rsapub.png)
如果你本地机器是Linux系统，那么该文件路径应该在：

```bash
~/.ssh/ #当前用户主目录下的.ssh文件夹中
```

如果你发现并没有id_rsa.pub文件，那么说明之前没有生成过，请参考 [生成SSH Key](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)

服务器中/home/git/.ssh/authorized_keys文件在这里：
```bash
[root@iZj6c2vq0s1w6wkap2geanZ git]# cd /home/git
[root@iZj6c2vq0s1w6wkap2geanZ git]# ls -al
total 24
drwx------  3 git  git  4096 Nov 18 10:26 .
drwxr-xr-x. 5 root root 4096 Nov 21 16:15 ..
-rw-r--r--  1 git  git    18 Oct 16  2014 .bash_logout
-rw-r--r--  1 git  git   176 Oct 16  2014 .bash_profile
-rw-r--r--  1 git  git   124 Oct 16  2014 .bashrc
drwxr-xr-x  2 root root 4096 Nov 18 10:28 .ssh
[root@iZj6c2vq0s1w6wkap2geanZ git]# cd .ssh
[root@iZj6c2vq0s1w6wkap2geanZ .ssh]# ll
total 4
-rw-r--r-- 1 root root 404 Nov 18 10:27 authorized_keys
```

将你本机的公钥添加到上面authorized_keys就可以了，多个key换行。

### 设置 VPS 的 ssh 端口

```bash
[root@iZj6c2vq0s1w6wkap2geanZ hexo]# vim /etc/ssh/sshd_config
```

```bash
Port 22
```

## 安装配置 Nginx

### 安装 Nginx

```bash
[root@iZj6c2vq0s1w6wkap2geanZ nginx]# yum install nginx
```

### 配置 Nginx

- 删除nginx虚拟主机配置文件

```bash
[root@iZj6c2vq0s1w6wkap2geanZ conf.d]# rm -rf /etc/nginx/conf.d/*
```

- 新建 /etc/nginx/conf.d/git.conf 文件，将以下内容添加到其中

```bash
server {
  root /var/www/hexo;  #网站根目录
  index index.html index.htm;
  server_name www.wentuotuo.com;   #你的域名
  location / {
    try_files $uri $uri/ /index.html;
  }
}
```

- 修改nginx配置文件

将/etc/nginx/nginx.conf中的user值修改为 git
并新增：
```bash
include /etc/nginx/sites-available/*.conf;
include /var/www/*.conf;
```
添加后配置文件如图：（红色方框内是新增内容）
![](http://oduq3lfcc.bkt.clouddn.com/nginx-conf.png)


**测试配置是否正确**

```bash
[root@iZj6c2vq0s1w6wkap2geanZ blog.git]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

- 配置防火墙

```bash
[root@iZj6c2vq0s1w6wkap2geanZ blog.git]# iptables -I INPUT -p tcp --dport 80 -j ACCEPT
```


### 启动 nginx

```bash
[root@iZj6c2vq0s1w6wkap2geanZ blog.git]# service nginx reload
[root@iZj6c2vq0s1w6wkap2geanZ blog.git]# /etc/init.d/nginx start
Starting nginx:                                            [  OK  ]
```



## 创建博客仓库

此处切换到 git 用户操作
```bash
[root@iZj6c2vq0s1w6wkap2geanZ .ssh]# su git
```

### 创建放仓库的目录
```bash
[git@iZj6c2vq0s1w6wkap2geanZ var]$ sudo mkdir /var/www/blog

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for git:
```

这里有提示信息，可以忽视，输入前面设置的git用户的密码完成目录创建。目录路径可以自行改变，但是要保证git用户有相应权限。

### 创建一个裸仓库 blog

```bash
[git@iZj6c2vq0s1w6wkap2geanZ blog]$ sudo git init --bare blog.git
[sudo] password for git:
Initialized empty Git repository in /var/www/blog/blog.git/
```

这样就创建了一个叫 blog 的裸仓库。
--bare 参数是告诉 Git 创建一个裸仓库，裸仓库没有工作区，我们不会在裸仓库上进行操作，它只为共享而存在。

## 配置 git hooks

参考资料：[自定义 Git - Git 钩子](https://git-scm.com/book/zh/v2/%E8%87%AA%E5%AE%9A%E4%B9%89-Git-Git-%E9%92%A9%E5%AD%90)

我们这里要使用的是 post-receive 的 hook，这个 hook 会在整个 git 操作过程完结以后被运行。

### 新建 post-receive 文件

- 在 /var/www/blog/blog.git/hook/ 目录下新建 post-receive 文件：

```bash
[git@iZj6c2vq0s1w6wkap2geanZ hooks]$ cd /var/www/blog/blog.git/hooks/
[git@iZj6c2vq0s1w6wkap2geanZ hooks]$ sudo vim post-receive
```

- 在新建的文件中输入以下信息并保存：

```bash
#!/bin/bash
GIT_REPO=/var/www/blog/blog.git #换成你部署静态网页的git仓库
TMP_GIT_CLONE=/tmp/HexoBlog #临时文件目录
PUBLIC_WWW=/var/www/hexo #网站发布目录，即实际的网站目录，里面包含index.html等网站内容
rm -rf ${TMP_GIT_CLONE}
git clone $GIT_REPO $TMP_GIT_CLONE
rm -rf ${PUBLIC_WWW}/*
cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}
```

- 给post-receive 文件添加执行权限

```bash
[git@iZj6c2vq0s1w6wkap2geanZ hooks]$ sudo chmod +x post-receive
```

## 其他配置

### 改变 blog.git 拥有者为 git 用户

```bash
[git@iZj6c2vq0s1w6wkap2geanZ hooks]$ cd /var/www/blog/
[git@iZj6c2vq0s1w6wkap2geanZ blog]$ sudo chown -R git:git blog.git
```

### 改变 hexo 目录拥有者为 git 用户

```bash
[git@iZj6c2vq0s1w6wkap2geanZ ~]$ cd /var/www/
[git@iZj6c2vq0s1w6wkap2geanZ www]$ sudo chown -R git:git hexo
```

### 禁止 git 用户登录 shell

为了安全考虑，在配置好上面信息以后，禁止git用户登录shell。

cat /etc/passwd 查看系统用户信息，找到git用户哪一行。
```bash
[git@iZj6c2vq0s1w6wkap2geanZ blog]$ cat /etc/passwd
```
比如我的机器上是：
git:x:500:500::/home/git:/bin/bash

将其改为：
git:x:500:500::/home/git:/usr/bin/git-shell

这样 git 用户可以正常使用 git-shell 但是不能使用bin/bash, 也就不能登录shell了。

## 发布博客

在站点_config.yml 中配置deploy信息。

```yaml
deploy:
  type: git
  repository: git@wentuotuo.com:/var/www/blog/blog.git
  branch: master
```

repository换成你的VPS上博客仓库路径

----

# 用git进行博客备份和还原

## 博客备份

## 博客还原（在新环境中继续撰写博客）

- 安装 git、nodejs、hexo环境（同上）
- 克隆博客仓库
> git clone [你的仓库URL]

- 切换到源码分支（如果源码分支是默认分支，那么克隆过后会默认处于该分支下，不需要再手动切换）
> git checkout [你的分支名字]

- 安装npm hexo 本地环境
```bash
npm install hexo --save #安装hexo环境到本目录
npm install #安装npm 环境到本目录,该步骤不做的话可能会出现hexo generate过后缺少文件（如index页面）
npm install hexo-deployer-git #hexo通过git发布网页的插件

npm install hexo-algolia --save #使用algolia搜索的还需要安装此插件
#...其他插件安装
```

- 环境恢复成功，可以使用hexo g -d 发布博客了
