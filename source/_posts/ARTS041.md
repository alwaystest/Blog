---
title: ARTS041
date: 2021-09-24 14:47:10
tags: ARTS
---

环球影城转了一圈，好玩！
哪个快就排哪个。

<!--more-->

# Algorithm

[Intersection of Two Arrays II - LeetCode](https://leetcode.com/problems/intersection-of-two-arrays-ii/)

DailyChallenge 找了一道，挺简单的。

TDD 一遍过。

# Review

[Android 开发太难了，这异常竟然捕获不到？](https://mp.weixin.qq.com/s/UcvTk8DAPnEh-DoouoyZMg)

TransactionTooLarge 的问题，有一个隐藏款。

关注稳定性的话，需要对 Bundle 大小做监控。Mark 一下。

[Gradle7.0，依赖统一管理的全新方式，了解一下~](https://mp.weixin.qq.com/s/bU8CEHHbD3TI3rbDMxk_mw)

Gradle7.0，依赖统一管理的一种方式预览。

之前项目里面出过 Gradle 自动协商依赖，导致 AAR 依赖的类找不到，出了线上 Bug。后续可能会用这个方案。

看 Detekt 已经在用这个 Feature 了，上次看源码的时候，版本信息找了老半天才找到。

# Tips

删除 Submodule 之后 SonarQube 任务遇到一个问题

> Execution failed for task ':sonarqube'.
> The base directory of the module 'xxx' does not exist

最终发现是删除 Submodule 的时候，Gradle 的 setting.gradle 中没有删除对 Project 的引用，导致 Sonar 会尝试寻找对应的 Project 目录，此时会报错。

比较难发现的原因是，项目对这个 SubProject 没有依赖，所有的编译，单测任务都是可以正常运行的，只有 Sonar 上报的时候会读到这里。不容易第一时间想到是 setting.gradle 没有清理干净导致的问题。

用 Docker 搞 Android 的 CI。并发运行两个 Job 的时候，发现有概率失败。看表现是两个 Job 同时触发了安装 BuildTools，比较奇怪的尝试安装的 BuildTools 不是我们项目声明使用的版本。

查了一下，AGP 3.0 开始会有一个默认使用的 BuildTools 版本，如果 build.gradle 没有指定，就会使用这个默认版本。把缺失的 module 都指定了一遍之后解决了这个问题。

CI 里面只预装了我以为会使用的 BuildTools，结果还有这种并发跑 Job，自动安装导致的坑。

# Share

[过](https://www.notion.so/45f68971b68e465e84c527f23808c21f)

