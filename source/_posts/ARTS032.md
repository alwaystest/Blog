---
title: ARTS032
date: 2021-06-08 01:05:08
tags: ARTS
---
煎牛排还是加了黄油更好吃
<!--more-->

# Algorithm

[Merge Two Sorted Lists - LeetCode](https://leetcode.com/problems/merge-two-sorted-lists/)

一个工具方法引发的血案

代码写的没啥问题，测试用例的一个帮助方法写错了，浪费了很多时间。

长时间不写果然手生，哪怕一直在面试别人，Review 别人写的代码。

# Review

[硬核系列 | 深入剖析字节码增强](https://xie.infoq.cn/article/d367c19896e4cef6fbb661cf7)

看了一篇中文的字节码增强说明

# Tips

[How can you cache gradle inside docker?](https://stackoverflow.com/questions/47823818/how-can-you-cache-gradle-inside-docker/66773109#66773109)

Gradle 6.2.1 才增加了 read only dependency cache

在 Jenkins 上如果用 Docker 并发构建 Gradle 任务，同时希望能使用 Volume 挂载 Gradle 的 cache 目录来达到加速构建的目的，会出现 Cache 文件加锁导致构建失败的情况。

目前想到的不用升级 Gradle 的解决办法：

1. 使用 SonaType 构建本地缓存，通过内网下载 Cache
2. 对于每一个并发 Job，都指定一个独立的 Volume 目录进行挂载，这样就避免了多个容器竞争。但是会导致依赖在 Host 机器上冗余存储的情况。

# Share

None
