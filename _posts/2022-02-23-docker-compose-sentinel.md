---
layout: post
title: Docker-compose安装Sentinel
author: Toplhyi
date: 2022-02-23 17:37 +0800
tags: [docker, docker compose, sentinel]
toc:  true
---

记录使用docker-compose安装sentinel

### docker-compose.yml
```
version: '3'
services:
  sentinel:
    image: bladex/sentinel-dashboard
    container_name: sentinel
    restart: on-failure
    environment:
      TZ: Asia/Shanghai
      LANG: en_US.UTF-8
    ports:
      - 8858:8858
```
