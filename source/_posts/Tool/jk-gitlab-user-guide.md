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
| 1  | V1.0 | 2017-10-23 | 何益民 |  |    |

<!--more-->


## 简介

金科代码管理中心采用 Git 作为版本控制工具，旨在提供高效、稳定、安全的代码管理，方便开发者协作，统一有效地推进各项开发任务。

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

<span id="installgit"></sapn>

### 下载安装 Git

Linux 和 Mac 系统自带 Git 工具，打开终端就可以使用相关命令。这里主要讲一下Windows环境的安装。

- 下载地址： [官方下载地址](https://git-scm.com/downloads)  [备用下载地址](http://file.wentuotuo.com/Git-2.14.2.3-64-bit.exe)

下载过后直接运行安装，默认配置就行，安装完成后，你的鼠标右键提示菜单中会多出两项：

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/%E5%8F%B3%E9%94%AEgit%E8%8F%9C%E5%8D%95.png)

<span id="configgit"></span>
### 配置 Git

安装好 Git 后，需要一些配置工作，不用担心，你只需要配置一次。右键任意位置打开 Gitbash here

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/git%E5%8F%B3%E9%94%AE.png)

在打开的终端中，配置用户名和邮箱信息：

```
git config --global user.name "你的代码中心的用户名"
git config --global user.email "youremail@XXX.com"
```

这里是告诉 Git 你的用户名和邮箱，之后提交代码时，这些信息会同时上传到服务器作为隐藏提交信息。

**验证配置：**

输入 $ git config --global --list 验证配置的信息是否正确

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/gitconfig.png)

----

## 正式开始

了解了 Git 的基本概念和操作后，下面正式进入金科代码管理中心的使用。

<span id="uploadsshkey"></span>

### 上传 sshkey

**Tips: 没有上传sshkey的电脑无法通过 ssh 协议下载和上传代码、提交合并代码等操作，但不影响登录、查看以及使用http协议的相关操作。**

git仓库之间的代码传输协议主要使用ssh协议。SSH是一个专为远程登录会话和其他网络服务提供安全性的协议。默认状态下SSH链接是需要密码认证的，可以通过添加系统认证（即公钥-私钥）的修改，修改后系统间可以实现免密认证。生成ssh公钥需要使用ssh-keygen工具，在金科代码管理中心，代码的上传下载、提交合并等操作都依赖ssh公钥。下面讲述如何上传你的ssh公钥。

linux/mac 系统自带ssh-keygen工具，直接在终端中操作即可。
Windows 用户需要先下载安装 Git工具，参见 [安装Git](#installgit)

**第一步：生成公钥：**
Windows用户安装好Git工具后，右键任意位置选择 gitbash here 打开终端。Mac、Linux直接打开系统自带的终端：

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
|         O + .o =|
|        o.= .o .o|
+-----------------+
```

生成的公钥存储在： {个人主目录}/.ssh/id_rsa.pub 文件中。
- Windows用户：C:\Users\{你的用户名}\.ssh\id_rsa.pub
- Linux & Mac：/home/{你的用户名}/.ssh/id_rsa.pub
</br>
可以直接打开文件或用下面命令查看公钥：（由于.ssh文件夹是隐藏文件夹，所以需要在控制面板-文件夹选项中打开隐藏文件显示才能看见。所以这里推荐直接干命令）

```
cat ~/.ssh/id_rsa.pub
```
或
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

**第三步：验证**

添加好ssh key过后在终端中输入 ssh -T git@<server> 进行测试链接，这里以测试环境192.168.9.75为例。
输入 ssh -T git@192.168.9.75 后，因为是首次连接，会提示你是否继续连接，输入 yes 回车。

当你输入yes过后，如果提示你输入 "git@192.168.9.75's passwords:", 则说明上传的ssh keys无效，请检查key并重新生成上传。

如果出现下面欢迎提示，则说明添加成功：

```
$ ssh -T git@192.168.9.75
The authenticity of host '192.168.9.75 (192.168.9.75)' can't be established.
RSA key fingerprint is SHA256:46UPdFB+77wyUqaLfu/kUu13/f/ed7oN4/V95bLkZEk.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.9.75' (RSA) to the list of known hosts.
Welcome to GitLab, 何益民!
```

## 项目管理

<span id="newproject"></span>

### 创建项目


![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/project%E9%A1%B5%E9%9D%A2.png)

点击上方 “+” 或 Projects 页面的 “New project” 创建项目。

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/%E5%88%9B%E5%BB%BA%E9%A1%B9%E7%9B%AE.png)

### 删除项目

非管理员只能删除自己创建的项目，操作步骤：

1. 进入要删除的项目。
2. 点击左下角Setting
3. 点击 Advanced settings 右边的 Expand 展开更多选项
4. 点击下方 Remove project
5. 为了防止误操作，你必须输入项目名字确认删除。

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/expand%20advanced%20settings.png)

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/%E5%88%A0%E9%99%A4%E9%A1%B9%E7%9B%AE.png)


### 项目设置

进入项目，点击左下角 Settings

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/%E9%A1%B9%E7%9B%AE%E8%AE%BE%E7%BD%AE.png)

#### 添加项目成员

选择Setting 的子菜单 Members，这里以给项目 testproject 添加两个成员 张三和李四 为例：

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/%E6%B7%BB%E5%8A%A0%E6%88%90%E5%91%98.png)

添加过后，张三和李四就拥有该项目的相应权限了，若要修改，可以在页面下面进行操作：

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/%E6%B7%BB%E5%8A%A0%E8%BF%87%E5%90%8E2.png)

### 克隆项目

假定你已经上传好了 sshkey，如果没有，请参考[上传sshkey](#uploadsshkey)

这里以项目 testproject 为例：

- 复制项目 ssh 地址：git@192.168.9.75:curry/testproject.git

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/%E5%85%8B%E9%9A%86%E9%A1%B9%E7%9B%AE1.png)

- 打开终端，输入命令 git clone git@192.168.9.75:curry/testproject.git

该命令会将 testproject 项目从服务器克隆到当前文件夹

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/%E5%85%8B%E9%9A%86%E9%A1%B9%E7%9B%AE2.png)

### 上传项目

上面克隆clone 下来的项目 testproject 是一个新建的空项目（Blank project），里面只有一个隐藏的 .git 文件夹，里面包含git版本控制的所有信息。

![](http://oduq3lfcc.bkt.clouddn.com/tp%E7%9B%AE%E5%BD%95.png)

这里新建一个名叫 HelloJinke.txt 的文件。

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/hellowjinke.png)

在当前项目目录右键 gitbash here 打开终端，或者cd到该目录，输入git status 查看仓库状态。

![](http://oduq3lfcc.bkt.clouddn.com/gitstatus.png)

可以看到刚刚新建的 HelloJinke.txt 还没有被加入版本控制。输入 git add HelloJinke.txt 将其加入版本控制库，然后再 git status 查看状态：

![](http://oduq3lfcc.bkt.clouddn.com/status1.png)

这里 Git 已经将新文件加入到版本控制中了，下面输入 git commit -m "新建文件HelloJinke.txt" 提交改变到本地版本库。

![](http://oduq3lfcc.bkt.clouddn.com/commit.png)

这样我们刚刚新建的文件就提交到了本地版本库中了，为什么说是本地版本库呢？因为 Git 是分布式的版本管理工具，你本地和金科代码中心的Git仓库本质上没有什么区别，只是为了大家写作，我们“推选”金科代码管理中心的Git仓库作为逻辑上的服务器，大家都把代码 push 到那里，方便管理。

那么如何把刚刚新建的文件推送（push）到远程仓库（金科代码管理中心的Git库）呢？这里就要用到 git push 命令了。

试试 git push :

```
$ git push
Counting objects: 3, done.
Writing objects: 100% (3/3), 234 bytes | 234.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To 192.168.9.75:heym/testproject.git
 * [new branch]      master -> master
```

这样代码就上传到了远程仓库，在testproject项目页面，可以看到刚刚新建的文件

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/1push.png)

## 组管理

- 组是若干项目的集合
- 组内的项目都有组路径作为前缀
- 可以将已经存在的项目移动到某个组内

### 创建组

点击最上方 Group 进入组管理界面，点击 New group 创建一个新的组。（这里以创建一个名叫库里的组为例）

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/%E7%BB%84%E7%95%8C%E9%9D%A2.png)

### 组内创建项目

在组界面点击 New project 创建项目，创建项目过程同上，参见[创建项目](#newproject)

![](http://oduq3lfcc.bkt.clouddn.com/%E6%96%B0%E5%BB%BA%E7%BB%84%E5%86%85%E9%A1%B9%E7%9B%AE.png)

### 将项目转移到组

这里以将 testproject 转移到组 库里 为例。

1. 进入要转移的项目 testproject；
2. 点击左下角 Settings；
3. 点击 Expand 展开 Advance settings 选项卡；
4. 定位到 Transfer project，选择组 库里，点击 Transfer project进行转移
5. 为了防止误操作，需要输入要转移的项目名字确认。

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/expand%20advanced%20settings.png)

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/%E8%BD%AC%E7%A7%BB%E9%A1%B9%E7%9B%AE.png)

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/%E8%BD%AC%E7%A7%BB%E7%A1%AE%E8%AE%A4.png)

### 组内成员管理

#### 添加成员到组

![](http://oduq3lfcc.bkt.clouddn.com/%E6%B7%BB%E5%8A%A0%E6%88%90%E5%91%98%E5%88%B0%E7%BB%84.png)

#### 成员管理

![](http://oduq3lfcc.bkt.clouddn.com/%E7%AE%A1%E7%90%86%E7%BB%84%E6%88%90%E5%91%98.png)


## 错误汇总

常见错误汇总，使用 CTRL+F 搜索

### 项目界面出现 “You won't be able to pull or push project code via SSH until you add an SSH key to your profile”

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/error%E6%8F%90%E7%A4%BA%E4%B8%8A%E4%BC%A0key.png)

**解决方法：** 上传你电脑的SSH KEYS到代码管理中心，参见[上传sshkey](#uploadsshkey)

### 克隆git clone或上传git push 等操作出现类似 “git@192.168.9.75's password:” 输入密码的提示

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/error%E6%8F%90%E7%A4%BA%E8%BE%93%E5%85%A5%E5%AF%86%E7%A0%81.png)

**解决方法：** 上传你电脑的SSH KEYS到代码管理中心，参见[上传sshkey](#uploadsshkey)
