---
title: 
date: 
tags: 
categories: 
layout: 
updated: 
comments: 
permalink: 
---


# 环境

服务器：centos 6.5
内存： 16G 
IP地址：192.168.9.75

<!--more-->

# 安装部署

参考官方安装教程： https://about.gitlab.com/installation/

- 开启 HTTP 和 SSH
```bash
sudo yum install -y curl openssh-server cronie
sudo lokkit -s http -s ssh
```

- 安装 Postfix 用于发送邮件（非必选）

```
sudo yum install postfix
sudo service postfix start
sudo chkconfig postfix on
```

- 添加 gitlab 源

```bash
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
```

- 安装 gitlab 

```bash
sudo yum -y install gitlab-ee
```

- 启动 gitla

```bash
sudo gitlab-ctl reconfigure
```

第一次登录需要设置root密码。

# 配置

```bash
# vim /etc/gitlab/gitlab.rb 
```
## 访问IP、域名设置

修改 external_url 'http://192.168.9.75' 如果有域名，则配置域名

## 邮件配置

这里以QQ邮箱为例

```bash
### Email Settings
gitlab_rails['stmp_enabled'] = true
gitlab_rails['smtp_address'] = "smtp.qq.com"
gitlab_rails['smtp_port'] = 25
gitlab_rails['smtp_user_name'] = "XXX@qq.com"
#### 邮箱独立密码
gitlab_rails['smtp_password'] = "XXX"
gitlab_rails['smtp_domain'] = "smtp.qq.com"
gitlab_rails['smtp_authentication'] = :plain
gitlab_rails['smtp_enable_starttls_auto'] = true
gitlab_rails['gitlab_email_from'] = "XXX@qq.com"
user["git_user_email"] = "XXX@qq.com "
```



