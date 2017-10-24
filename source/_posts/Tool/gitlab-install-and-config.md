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

### 修改配置文件

#### 修改IP/域名
external_url 'http://gitlab.example.com' 修改为你自己的域名或者IP

#### 修改时区

gitlab_rails['time_zone'] = 'PRC' 修改时区为中国

#### 自定义邮件通知服务

详见[SMTP Setting](https://docs.gitlab.com/omnibus/settings/smtp.html#smtp-settings)

这里以腾讯企业QQ邮箱为例：

```

```

## 常用操作

### 启动、停止服务

```
gitlab-ctl start
gitlab-ctl stop
gitlab-ctl restart
```

### 重载配置

```
gitlab-ctl reconfigure
```

### 首次登陆

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/%E7%AC%AC%E4%B8%80%E6%AC%A1%E7%99%BB%E5%BD%95.png)

首次登陆，需要设置root密码。设置好过后使用root登陆系统，在 admin area 里的setting中修改相关配置：

![](http://oduq3lfcc.bkt.clouddn.com/image/jinke/gitlab/adminsetting.png)

#### Visibility and Access Controls 

##### default branch protection 默认分支保护策略

##### default project visibility 默认项目可见权限

#### Sign-up Restrictions 设置是否开启自主注册

#### Sign-in Restrictions 设置是否开启登录

