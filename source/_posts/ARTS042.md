---
title: ARTS042
date: 2021-09-24 14:48:51
tags: ARTS
---

有点难过的，战友们没了。

<!--more-->

# Algorithm

过

# Review

过

# Tips

[error opening HPROF file: IOException: Unknown HPROF Version](https://stackoverflow.com/questions/6219049/error-opening-hprof-file-ioexception-unknown-hprof-version)

好久没有排查内存泄漏问题了。LeakCanary 导出的快照文件不能被 MAT 直接识别，需要用 `hprof-conv` 工具转换一下才行。

试用了一下 Android Studio 直接打开 hprof 文件，感觉也还挺管用的，直接就定位出来 Leak 了。看到的引用结果和 MAT 也基本没啥区别。

# Share

过

