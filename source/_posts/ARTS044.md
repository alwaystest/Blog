---
title: ARTS044
date: 2021-10-17 18:46:03
tags: ARTS
---

仰望蓝天，相对于宇宙的宏大，我的那点小烦恼算什么呢

<!--more-->

# Algorithm

[Add Two Numbers - LeetCode](https://leetcode.com/problems/add-two-numbers/)

Medium。好久不写代码确实是手生，随机了好多题，都没有思路。先挑了一个看起来比较有思路的。

Kotlin 的语法糖简化了好多代码。

思路上还是比较简单的。

用 VIM 写了实现和单测。确实没有 IDE 更加智能，但是胜在轻量。问题不大。舒服。

# Review

[Apple Developer Documentation](https://developer.apple.com/documentation/xcode/addressing-crashes-from-swift-runtime-errors)

简单看了一下 swift 的一个 Trap Error 的描述。还得多处理几个崩溃才行。

[Testing in Java & JVM projects](https://docs.gradle.org/current/userguide/java_testing.html)

在试着用 Vim 做为刷 LeetCode 的环境，用 Gradle 执行单测的时候发现好像没有执行。后来发现是执行的太快了。从 Report 就能看到执行的结果。比 IDE 快多了。另外学到了运行指定单测的方式 `--tests` 终于知道 IDE 是怎么玩的啦。

# Tips

vim 更新了一下配置，使用 coc + coc-kotlin + vim-test 实现了一个简单的 Kotlin 编写环境。

把 NetdTree 更换成了 Defx，还有一些快捷键需要熟悉一下。

目前对比 Idea，主要差在了没有配置快捷键一键生成或跳转对应的单元测试文件。新的方法名写完之后，没有快捷键直接补充大括号并跳转到方法实现 block 中。

问题不大。有 LSP 的加持，很多简单问题也能及时被发现，看来还是没办法成为用记事本写代码的大神呢 👻

# Share

略
