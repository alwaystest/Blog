---
title: ARTS025
date: 2021-03-08 15:00:30
tags: ARTS
---
我知道生活最终会归于柴米油盐，但是我不想我的生命变成那种鬼样子。即使有一天它终将会来。但我还是想竭尽所能让自己的生活充满焰火。
<!--more-->

# Algorithm

🕊️一下

# Review

[googlesamples/android-custom-lint-rules](https://github.com/googlesamples/android-custom-lint-rules/)

遇到一个 `Obsolete custom lint check` 问题，查看了 POM 文件，没有发现这个依赖是怎么带过来的。后来发现 `lint.jar` 这个文件被直接打包在了 aar 中。结合这篇文章，学习到了 AAR 怎么声明 Lint 规则。

> This tells the Gradle plugin to take the output from the "checks" project and package that as a "lint.jar" payload inside this library's AAR file. When that's done, any other projects that depends on this library will automatically be using the lint checks.

# Tips

之前找了好久的 Gradle 管理依赖的版本逻辑，总算被同事找到了

[Declaring Versions and Ranges](https://docs.gradle.org/current/userguide/single_versions.html)

---

解决了一个小脚本出的问题。从一个文件中读取字符串，然后去掉字符串首尾的双引号。

`cat | awk | tr` 可解决问题

主要是记录一下 `tr` 这个命令，挺好用👍🏻

# Share

憋一个分享中
