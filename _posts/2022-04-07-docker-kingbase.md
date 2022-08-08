---
layout: post
title: Docker安装Kingbase
author: Toplhyi
date: 2022-04-07 17:37 +0800
tags: [docker, kingbase, 数据库]
toc:  true
---

记录使用docker-compose安装人大金仓kingbase数据库

### docker hub
[kingbase](https://hub.docker.com/r/godmeowicesun/kingbase)

### docker command
```
docker run -d -it --privileged=true -p 54321:54321 -v /opt/docker/kingbase-v8/opt:/opt --name kingbase-rv1 godmeowicesun/kingbase:es-v8-r3-rv1
```
