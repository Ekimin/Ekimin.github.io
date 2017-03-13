title: RMI远程调用
date: 2017-01-02 15:58:22
tags: [RMI, Java]
categories: Java
layout: post
updated: 
comments: 
permalink: 
---

# 什么是RMI？

RMI是个典型的为java定制的远程通信协议，在single vm中，可以通过直接调用Java object instance来实现通信，那么在远程通信时，如果也能按照这种方式当然是最好了，这种远程通信的机制成为RPC（Remote Procedure Call），RMI正是朝着这个目标而诞生的。


<!--more-->

# RMI调用的流程

1. 客户端发起请求，请求转交至RMI客户端的stub类；
2. stub类将请求的接口、方法、参数等信息进行序列化；
3. 基于socket将序列化后的流传输至服务器端；
4. 服务器端接收到流后转发至相应的skelton类；
5. skelton类将请求的信息反序列化后调用实际的处理类；
6. 处理类处理完毕后将结果返回给skelton类；
7. Skelton类将结果序列化，通过socket将流传送给客户端的stub；
8. stub在接收到流后反序列化，将反序列化后的Java Object返回给调用者

# RMI 相关问题

1. 传输的标准格式是什么？
是Java ObjectStream。
2. 怎么样将请求转化为传输的流？
基于Java串行化机制将请求的java object信息转化为流。
3. 怎么接收和处理流？
根据采用的协议启动相应的监听端口，当有流进入后基于Java串行化机制将流进行反序列化，并根据RMI协议获取到相应的处理对象信息，进行调用并处理， 处理完毕后的结果同样基于java串行化机制进行返回。
4. 传输协议是？
Socket。

