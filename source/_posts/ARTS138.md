---
title: ARTS138
tags: ARTS
date: 2024-12-15 17:48:33
---

看了 《新蝙蝠侠》，原来阿福就是 Alfred，也有管家的意思。
<!-- more -->

## Algorithm

none

## Review

none

## Tips
新项目的 Native Lib Strip 功能不生效了，简单调研了一下。
目前环境变量声明 NDK 和 ANDROID_NDK_HOME 已经无效了。
在 local.properties 中声明 ndk.dir 的方式也废弃了。
需要在 build.gradle 中直接声明 ndkVersion
```
android {
    ndkVersion = "x.x.x"
}
```
AGP 还能下载缺失的 NDK 版本，有利于 CI 环境配置。

Android 开发者设置可以关闭动画，会导致一些酷炫的功能失效。
Lottie 看起来可以通过使用系统的 Choreographer 来同步帧更新的方式绕过这个限制。

## Share

none

