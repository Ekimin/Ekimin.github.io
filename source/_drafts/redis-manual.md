---
title: Redis 手册
date:
tags: [Redis]
categories: 数据库
layout: post
updated:
comments:
permalink:
---

Redis 常用操作

<!--more-->

# 源码安装

```bash
$ wget http://download.redis.io/releases/redis-3.2.8.tar.gz
$ tar xzf redis-3.2.8.tar.gz
$ cd redis-3.2.8
$ make
```

编译过后生成二进制文件保存在 redis-3.2.8/src 下。

# 启动 redis server

```bash
[root@iZj6c2vq0s1w6wkap2geanZ redis-3.2.8]# cd src/
[root@iZj6c2vq0s1w6wkap2geanZ src]# ./redis-server
25346:C 23 Mar 16:55:44.597 # Warning: no config file specified, using the default config. In order to specify a config file use ./redis-server /path/to/redis.conf
25346:M 23 Mar 16:55:44.670 # Not listening to IPv6: unsupproted
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 3.2.8 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 25346
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

25346:M 23 Mar 16:55:44.671 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
25346:M 23 Mar 16:55:44.671 # Server started, Redis version 3.2.8
25346:M 23 Mar 16:55:44.671 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
25346:M 23 Mar 16:55:44.671 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
25346:M 23 Mar 16:55:44.671 * The server is now ready to accept connections on port 6379
```

启动的时候可以选着后台启动server，方便下面开启 redis-cli

```bash
[root@iZj6c2vq0s1w6wkap2geanZ src]# ./redis-server &
[root@iZj6c2vq0s1w6wkap2geanZ src]# ./redis-cli 
127.0.0.1:6379> 
```

# Jedis

Jedis 提供了一组 Java API 方法操作 Redis。