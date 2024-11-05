---
title: ARTS137
tags: ARTS
date: 2024-11-05 15:09:26
---

Kotlin 真是一门好语言！

<!-- more -->

## Algorithm

none

## Review

[Android Studio  |  Android Developers](https://developer.android.com/build/releases/past-releases/agp-7-0-0-release-notes#r8-missing-class-warning)
升级 AGP 到 8.0，混淆之后遇到了一些问题，果然大版本的 BreakChange 还是需要花点时间好好研究研究。
R8 优化代码的时候会检测 Class 定义是否存在，AGP 7 的时候还只是 print warning，到了 AGP 8，ClassMissing 开始成为 Error，阻断打包。

[R8 FAQ](https://r8.googlesource.com/r8/+/refs/heads/master/compatibility-faq.md#r8-full-mode)
AGP 8 启用了更激进的优化策略，需要更新 GSON 对应的 Proguard Rule

被一个崩溃困扰了好久，最后发现是 R8 FullMode 默认移除字段级别的 Annotation 信息，导致 Gson 反序列化泛型对象识别错了类型导致的。
混淆还会替换 SourceFile 信息，增加一些 inline 优化，导致 Debug 巨困难。
不过要是思路清晰，定位和发现这个问题不是什么难事，我一直以为是 inline 优化把泛型信息抹掉了，一直尝试判断是哪一行字节码搞错了类型，绕了远路。

## Tips

review 一个内存泄漏的改动，是匿名内部类隐式持有外部类对象导致的问题。
发现 Kotlin 默认内部类就不会持有外部类的引用。如果非要持有，需要显式声明 `inner`
真好！

## Share

none
