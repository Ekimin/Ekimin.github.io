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

Nginx Config

<!--more-->

Nginx 的所有配置文件都放在 /etc/nginx/ 目录，默认配置文件 /etc/nginx/

>注：默认配置适用于通过包管理器（如yum, apt-get）安装的情况。也可能在 /opt/nginx/conf/下。

# 启动，停止，重载

```bash
service nginx start
service nginx stop
service nginx reload
```

# 配置详解

```bash
user www-data;
worker_processes 4;
pid /run/nginx.pid;

events {
        worker_connections 768;
        # multi_accept on;
}
```

user : 拥有和运行nginx服务的用户。大部分基于Debian的发行版使用 www-data。
worker_processes ： 最大线程数。
pid : 记录 nginx 进程信息的文件。

```bash
http {

    ##
    # Basic Settings
    ##

    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;
    # server_tokens off;

    # server_names_hash_bucket_size 64;
    # server_name_in_redirect off;

    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    ##
    # Logging Settings
    ##

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    ##
    # Gzip Settings
    ##

    gzip on;
    gzip_disable "msie6";
```

include : 引用外部配置信息，被引用的文件的内容相当于写在当前位置（http{}内）。


```bash
server {
        listen 80 #default_server;
        listen [::]:80 #default_server ipv6only=on;

        root /usr/share/nginx/html;
        index index.html index.htm;

        # Make site accessible from http://localhost/
        server_name localhost;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ /index.html;
                # Uncomment to enable naxsi on this location
                # include /etc/nginx/naxsi.rules
        }
}
```

# 参考

- [How to config nginx](https://www.linode.com/docs/websites/nginx/how-to-configure-nginx/)
- [Nginx 配置详解](http://blog.csdn.net/tjcyjd/article/details/50695922)