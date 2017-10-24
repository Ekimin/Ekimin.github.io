---

title: Gitlab 安装使用教程（待更新）
tags:
  - Gitlab
  - Git
categories: 工具
date: 2017-10-23 11:47:19
updated: 
comments: 
permalink: 
---


- Gitlab 版本: 10.0.4
- 服务器： CentOs 6.5

<!--more-->

# 安装

## 安装配置必要依赖

- 安装python依赖包，开启 http 和 ssh
```
sudo yum install -y curl policycoreutils-python openssh-server cronie
sudo lokkit -s http -s ssh
```

- 安装 Postfix（如果不用Postfix作为邮件服务，则不需要安装）

```
sudo yum install postfix
sudo service postfix start
sudo chkconfig postfix on
```

## 安装 gitlab 

- 添加 gitlab 源

```
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
```

- 安装

```
sudo EXTERNAL_URL="http://gitlab.example.com" yum -y install gitlab-ee
```

## 配置

gitlab 配置文件： /etc/gitlab/gitlab.rb 修改过后使用 gitlab-ctl reconfigure 使配置生效

#### 修改IP/域名
external_url 'http://gitlab.example.com' 修改为你自己的域名或者IP

#### 修改时区

gitlab_rails['time_zone'] = 'PRC' 修改时区为中国

#### 自定义邮件通知服务

详见[SMTP Setting](https://docs.gitlab.com/omnibus/settings/smtp.html#smtp-settings)

这里以腾讯企业QQ邮箱为例：

```

```

#### 设置存储仓库数据的备用目录

默认情况下omnibus-gitlab 将仓库数据存储在 /var/opt/gitlab/git-data目录下，仓库存放在子目录 repositories里面。 以可以通过修改/etc/gitlab/gitlab.rb 的这一行来自定义 git-data 的父目录。

```bash
git_data_dirs({ "default" => { "path" => "/mnt/nas/git-data" } })
```

可以通过在/etc/gitlab/gitlab.rb文件中添加下面的几行配置, 来增加多个git数据目录.

```bash
git_data_dirs({
  "default" => { "path" => "/var/opt/gitlab/git-data" },
  "alternative" => { "path" => "/mnt/nas/git-data" }
})
```

运行 sudo gitlab-ctl reconfigure 使修改生效。

</br>

如果 /var/opt/gitlab/git-data 目录已经存在Git仓库数据， 你可以用下面的命令把数据迁移到新的位置:

```bash
# 准备迁移之前要停止GitLab服务，防止用户写入数据。
sudo gitlab-ctl stop

# 注意 'repositories'后面不带斜杠，而
# 'git-data'后面是有斜杠的。
sudo rsync -av /var/opt/gitlab/git-data/repositories /mnt/nas/git-data/

# 如果需要修复权限设置，
# 可运行下面的命令进行修复。
sudo gitlab-ctl reconfigure

# 再次检查下 /mnt/nas/git-data的目录. 正常情况应该有下面这个子目录:
# repositories

sudo ls /mnt/nas/git-data/

# 完工! 启动GitLab，验证下是否能
# 通过web访问Git仓库。
sudo gitlab-ctl start
```

#### 修改默认的运行用户和组

默认情况下omnibus-gitLab使用git用户登陆Git gitlab-shell, Git data是属主也是git，网页上生成的SSH URL也是git用户。 同样的, git用户组也是Git data的属组。

</br>

我们不建议在已经安装好的gitlab上修改默认的user/group，因为这会产生无法预知的副作用。 如果你固执的想要修改默认的user/group，你可以在 /etc/gitlab/gitlab.rb文件中添加下面加行配置:

```bash
user['username'] = "gitlab"
user['group'] = "gitlab"
```

运行 sudo gitlab-ctl reconfigure 使修改生效。

</br>

注意：如果你在已经安装好的GitLab中修改了user/group，reconfigure不会修改子目录的属主和属组，你需要手动修改， 并确认新用户可以读写 repositories 和 uploads 目录。


## 常用操作

### 启动、停止服务

omnibus-gitlab安装配置完成后，服务器会生成一个Runit的服务目录， (runsvdir)通过/etc/inittab 或 /etc/init/gitlab-runsvdir.conf 配置文件引导运行GitLab服务。我们不建议用runsvdir直接控制服务的运行状态， Gitlab提供了gitlab-ctl这个前端命令代替runsvdir

Gitlab可以用下面这些指定启动、停止或者重启 Gitlab所有的组件

```bash
# 启动Gitlab所有组件
sudo gitlab-ctl start

# 停止Gitlab所有组件
sudo gitlab-ctl stop

# 重启Gitlab所有组件
sudo gitlab-ctl restart
```

### 查看各服务状态


```bash
# sudo gitlab-ctl status
run: gitaly: (pid 31226) 94190s; run: log: (pid 30545) 94252s
run: gitlab-monitor: (pid 31264) 94189s; run: log: (pid 30968) 94222s
run: gitlab-workhorse: (pid 31250) 94190s; run: log: (pid 30669) 94246s
run: logrotate: (pid 11791) 633s; run: log: (pid 30835) 94234s
run: nginx: (pid 16189) 86338s; run: log: (pid 30780) 94240s
run: node-exporter: (pid 30913) 94228s; run: log: (pid 30912) 94228s
run: postgres-exporter: (pid 31323) 94188s; run: log: (pid 31146) 94204s
run: postgresql: (pid 30209) 94301s; run: log: (pid 30208) 94301s
run: prometheus: (pid 31308) 94189s; run: log: (pid 31071) 94210s
run: redis: (pid 30083) 94308s; run: log: (pid 30082) 94308s
run: redis-exporter: (pid 31016) 94216s; run: log: (pid 31015) 94216s
run: sidekiq: (pid 29934) 79006s; run: log: (pid 30484) 94258s
run: unicorn: (pid 29983) 78991s; run: log: (pid 30417) 94264s
```

### 首次登陆

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/%E7%AC%AC%E4%B8%80%E6%AC%A1%E7%99%BB%E5%BD%95.png)

首次登陆，需要设置root密码。设置好过后使用root登陆系统，在 admin area 里的setting中修改相关配置：

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/adminsetting.png)

### 终端滚动打印logs

```bash
# 查看所有的logs; 按 Ctrl-C 退出
sudo gitlab-ctl tail

# 拉取/var/log/gitlab下子目录的日志
sudo gitlab-ctl tail gitlab-rails

# 拉取某个指定的日志文件
sudo gitlab-ctl tail nginx/gitlab_error.log
```

### 容器注册服务垃圾回收

Container registry会占用大量的磁盘空间。 垃圾回收命令可清理未使用的layers(与Docker容器的layers同理)。

> Container registry ：GitLab的容器注册服务，与Docker registry同理， 下文中不对registry独立翻译。

- 内置垃圾回收命令在回收垃圾前会停止registry服务
- 垃圾回收需要一定的时间处理， 消耗的时间取决于存储数据的大小。
- 如果你修改了registry 配置文件的路径， 需要指定下修改后的路径。
- 垃圾回收完成后，registry服务会自动启动。

下面的命令将导致Container registry停止服务。

```bash
sudo gitlab-ctl registry-garbage-collect
```

垃圾回收需要一定的时间处理，消耗的时间取决于存储数据的大小。

</br>

如修改过registry配置文件 'config.yml'的路径，用下面的方式执行垃圾回收：

```
sudo gitlab-ctl registry-garbage-collect /path/to/config.yml
```


### 重载配置

```
gitlab-ctl reconfigure
```

#### Visibility and Access Controls 

##### default branch protection 默认分支保护策略

##### default project visibility 默认项目可见权限

#### Sign-up Restrictions 设置是否开启自主注册

#### Sign-in Restrictions 设置是否开启登录

