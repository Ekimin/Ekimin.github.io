---
title: Azkaban 调度
date: 
tags: [Azkaban, 调度, 大数据, Python]
categories: 大数据
layout: post
updated: 
comments: 
permalink: 
---

Azkaban 总流程调度

<!--more-->

# Azkaban 总流程调度
## 1.实现功能（需求）

- 周期性扫描MySQL对应的表，将符合要求的Flow调起
- 限制同一个Flow的调起个数，避免资源独占

## 2.具体实现
### 2.1 语言坏境

- Python 2 

### 2.2 MySQL表监控

- 使用Python中MySQLdb模块，操作MySQL

### 2.3 如何调用Azkban任务
使用的是Azkaban自身的API,Azkaban自身提供了一套了RESTful的API！http://azkaban.github.io/azkaban/docs/latest/#ajax-api

用到的主要API：

- **Authenticate（登录获取sessionID）**

```
Method: POST
Request URL: /?action=login
Parameter Location: Request Query String
This API helps authenticate a user and provides a session.id in response.
```

- **Fetch Running Executions of a Flow（获取当前正在执行的Flow）**

```
Given a project name and a flow id, this API call fetches only executions that are currently running.

Method: GET
Request URL: /executor?ajax=getRunning
Parameter Location: Request Query String
```

- **Execute a Flow（执行一个Flow）**

```
This API executes a flow via an ajax call, supporting a rich selection of different options. Running an individual job can also be achieved via this API by disabling all other jobs in the same flow.

Method: GET
Request URL: /executor?ajax=executeFlow
Parameter Location: Request Query String
```

### 2.4 Flow个数控制

- 同一个Flow最大可执行个数为10
- 公共资源池(public)也为10

也就是对需要执行的Flow,需要满足以下要求才会执行：
```
count(flow)<10||(count(flow)+count(public)<20)
```
