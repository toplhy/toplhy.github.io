---
layout: post
title: Grails4引用quartz插件报错问题
author: Toplhyi
date: 2019-11-12 17:37 +0800
tags: [Grails]
toc:  false
---

Grails版本: 4.0.4

项目中引入quartz插件：
```
compile 'org.grails.plugins:quartz:2.0.13'
```
启动报错：
```
Caused by: java.lang.NoClassDefFoundError: org/quartz/JobExecutionContext
```
解决：
```
compile("org.quartz-scheduler:quartz:2.2.3") {
    exclude group: 'slf4j-api', module: 'c3p0'
}
compile ('org.grails.plugins:quartz:2.0.13')
```
参考[https://github.com/grails-plugins/grails-quartz/issues/107](https://github.com/grails-plugins/grails-quartz/issues/107)
