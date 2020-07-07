---
title: ARTS004
date: 2020-06-21 22:53:57
tags: ARTS
---
# A.R.T.S 004
<!--more-->

# Algorithm
[Minimum Flips to Make a OR b Equal to c - LeetCode](https://leetcode.com/problems/minimum-flips-to-make-a-or-b-equal-to-c/)

1318 说是中等难度，实际上感觉偏简单了点。

继续先单测后实现。一遍过。爽歪歪。

# Review

[dependency_resolution](https://docs.gradle.org/4.10.1/userguide/introduction_dependency_management.html#sec:dependency_resolution)

[customizing_dependency_resolution_behavior](https://docs.gradle.org/4.10.1/userguide/customizing_dependency_resolution_behavior.html#customizing_dependency_resolution_behavior)

遇到了 Gradle 解析版本冲突的问题，看了一下相关的文档，感觉还是没抓到点上。

# Tips

Android 编译时间比较长，在等编译结果的时候，如果要做其他事情，希望编译成功的时候发出一个提示，在 Mac 上可以这么写

```bash
./gradlew assemble; say complete
```

`;` 不管前面命令是否执行成功，都会执行后面的命令

`&&` 则需要前面的任务执行成功，才会执行后面的任务

`||` 则是前面的任务执行失败后才会执行后面的任务

配合 `$?` 获取上一个命令的执行结果，可以实现

```bash
./gradlew assemble; say task result $?
```

---

# Share

[Dependencies in Gradle](/2020/06/21/Dependencies-in-Gradle)
