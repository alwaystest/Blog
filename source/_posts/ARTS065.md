---
title: ARTS065
date: 2022-05-11 18:29:38
tags: ARTS
---

Mockk 与 JavaAgent

<!--more-->

# Algorithm

None

# Review

None

# Tips

[How can I force update all the snapshot Gradle dependencies in intellij](https://stackoverflow.com/a/62272439/3819519)

之前在 Gradle 文档里面找到了使用命令行的情况下手动刷新 Snapshot 版本的方法，IDE 里面也需要整一下。

得空扫了一眼 Mockk 的源码，看到依赖了一个库，叫 [ByteBuddy](https://bytebuddy.net/#/)。

果然知识都是相通的，这个库用了 JavaAgent 来实现动态修改代码。之前也了解过。

# Share

None
[](https://www.notion.so/43cb462460a34aae80bf6f08727aa12d)
