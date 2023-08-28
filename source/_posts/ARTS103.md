---
title: ARTS103
tags: ARTS
date: 2023-08-28 16:16:01
---

咦？一周怎么过的这么快。

<!--more-->

# Algorithm

None

# Review

[一文吃透 JavaScript 中 Object 和 Map 的区别 - 掘金](https://juejin.cn/post/6987316014761377799)

之前写 JS 脚本的时候遇到了两个问题，这篇文章解答了我的疑惑。

1. Object 对象和 Map 的区别
2. JSON.stringify 方法序列化 Map 的结果为什么是一个空对象

[JS 项目中究竟应该使用 Object 还是 Map？](https://zhuanlan.zhihu.com/p/358378689)

顺便再了解一下如何进行选择。

[Why can host and nslookup resolve a name but dig cannot?](https://serverfault.com/a/434583/1043681)

之前查过一次 SearchDomain 的概念，最近突然想起来 dig 一直没有办法查到内网对应的 IP 地址，原来是默认不搜索 SearchDomain，还得加个参数。

# Tips

踩了一个 ASM 的坑。

升级到 `sentry-android-gradle-plugin:3.4.3` 版本后，某个依赖库运行时报错 

```
Caused by: java.lang.VerifyError: Verifier rejected class ... register v4 has type Reference: java.lang.Object but expected Precise Reference ...
```

恰巧这个改动是和升级 Gradle， AGP 是在一起提交的，diff 了不同的编译环境之后，以为是 D8 出了问题，把 class 转换到 dex 时遇上了 Bug。先临时回滚了代码保证上线。

后面又深入研究具体原因的时候，对比 dex 内容发现有问题的 dex 文件中出现了 Setnry 相关的代码。

搜索 Sentry Plugin 的源码，发现了相关的踪迹。

Sentry Plugin 内部有字节码插桩处理，做了一些 Hook 操作，正是这里的操作，修改了依赖库的 jar 包内容，从而导致 class → dex 之后的类无法通过校验。

同事反馈升级到最新版本的 Plugin 即可解决问题，但是 diff 了一下改动，没有发现 ASM 或者 Sentry 的 Wrap 策略有什么改动。

Sentry 修改字节码没有使用 Transform API，已经切换到了 `AsmClassVisitorFactory` 的方式，想要查看 Sentry 对 Jar 包做的修改还费了点事。

搜了一圈网上的文档都没有找到新 API 操作后的结果存储怎什么地方，没办法只能扒 AGP 的源码。

大致看到这样的注释

> // The following task registrations MUST follow the order:
//   ASM API -> Legacy transforms -> jacoco transforms
> 

Legacy Transform 的时候注意到在 `/build/intermediates/transforms` 文件夹下，对应的 transform 内存在一个 `__content__.json` 文件，里面会注明一下 transform 之前和之后的文件路径。只要找到第一个 transform 对应的文件夹，就能找到 ASM API 的产物路径了。

我当前的环境产物路径是这样的

```
~/.gradle/caches/transforms-3/xxxxx
```

至于目录后面的数字后缀，之前查过一次，记录一下 ref 吧

[Why are there numbers in Gradle cache directories?](https://stackoverflow.com/a/59358910/3819519)

# Share

None
