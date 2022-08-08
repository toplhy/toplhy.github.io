---
layout: post
title: Docker安装ActiveMQ
author: Toplhyi
date: 2022-06-15 17:37 +0800
tags: [docker, activemq, 消息队列]
toc:  true
---

记录使用docker安装activemq队列

### docker command
```
docker run --name activemq -p 61616:61616 -p 8161:8161 rmohr/activemq
```

### describe
```
61616是 activemq 的容器使用端口
8161是 web 页面管理端口
-e ACTIVEMQ_ADMIN_LOGIN=admin 指定登录名
-e ACTIVEMQ_ADMIN_PASSWORD=123456 登录密码
```
