---
title: ARTS117
tags: ARTS
date: 2024-02-03 13:20:45
---

年前怎么这么忙啊！

<!--more-->

# Algorithm

none

# Review

[posix是什么都不知道，还好意思说你懂Linux？](https://zhuanlan.zhihu.com/p/392588996)

说来惭愧，之前只知道有这么一套标准，但是不知道标准具体是管什么的。这次总算能明确一下了。

目前遇到的两个相关问题：

1. fish 不完全兼容 posix 标准
2. alpine 没有使用 glibc 和 GNU Core Utilities

[Is Sqlite Database instance thread safe](https://stackoverflow.com/a/6675272/3819519)

Sqlite 线程安全的问题，怎么依稀记得之前也研究过类似的问题呢。

[Keeping File Descriptors (FD’s) in check on Android](https://medium.com/helpshift-engineering/keeping-file-descriptors-fds-in-check-on-android-24778973a173)

CursorWindowAllocationException 又有一种可能的原因。我是没有想到的。

[limitedParallelism](https://kotlinlang.org/api/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-coroutine-dispatcher/limited-parallelism.html)

抠抠搜搜的又找到一个复用线程池的办法。

# Tips

神奇的事情来了，用 Lean 的源码自己编译了一版 NX30 Pro 的系统出来，产物只有 20 MB，对比之下恩山的版本有 40 MB。应该是我没有引入太多功能导致资源占用变低了。这下能用 Meta 内核搞事情啦。

初步验证了一下稳定性，比我之前瞎折腾之后强多了，还没有发现自动重启的情况。

稳定为先吧，其他乱七八糟的功能能分离就分离。拒绝 All in boom。

# Share

none
