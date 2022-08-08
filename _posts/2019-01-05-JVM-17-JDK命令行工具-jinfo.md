---
layout: post
title: 17.JDK命令行工具-jinfo
author: Toplhyi
date: 2019-01-05 17:53 +0800
tags: [JVM]
toc:  false
---

## JDK命令行工具-jinfo

jinfo的作用是实时的查看和调整虚拟机各项参数。

![jvm_17](/images/jvm/jvm_17.png)

用法：jinfo \[option] \<pid\>  

option为以下之一：

| 选项                     | 作用                       |
| ------------------------ | -------------------------- |
| -flag \<name\>           | 输出对应名称的参数         |
| -flag [+\|-] \<name\>    | 开启或者关闭对应名称的参数 |
| -flag \<name\>=\<value\> | 设定对应名称的参数         |
| -flags                   | 输出全部的参数             |
| -sysprops                | 输出系统属性               |
