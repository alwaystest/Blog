---
title: ARTS074
tags: ARTS
date: 2022-12-03 18:47:34
---

click to see more!
<!--more-->

# Algorithm

# Review

# Tips

用 type 命令可以区分要使用的 command 是 BuiltIn 关键词还是安装的第三方命令。

之前见过使用 `time` 命令给 command 做耗时统计，但是 zsh 中 time 是一个关键词，使用 `type -a time` 就能看到所有 path 中能找到的。

在 zsh 中调用 GNU 的 time，可以在命令前加 `\`

使用 ADB 隐式调起 App 的时候，URI 中有 `&` 会导致 APK 收到的 data 被截断，因为 ABD Shell 运行在 Shell 环境下， `&` 是关键字，后面的命令被 Shell 截掉了。

可以通过 `\` 转义 data 中的 `&` 符号来解决问题。

# Share
