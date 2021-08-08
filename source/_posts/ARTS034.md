---
title: ARTS034
date: 2021-08-08 15:10:54
tags: ARTS
---

越来越忙，需要一个长假 Reset 一下这种疲于奔命的状态。
<!--more-->

# Algorithm

略

# Review

[在线预览|GB/T 35273-2020](http://c.gb688.cn/bzgk/gb/showGb?type=online&hcno=4568F276E0F8346EB0FBA097AA0CE05E)

国标对于个人信息的定义，最近接了很多监管和整改的需求，主要关注点在于不能在用户未授权的情况下获取用户的隐私数据。但是一开始对于用户的隐私数据是什么，大家都没有明确的定义。经过了艰苦的沟通，终于找到了信息的源头，个人隐私数据的定义都在 GB-35273 中给出了划分标准，在附录中给出了一些常见的数据是否属于个人隐私数据的说明。后续监管会越来越严格，保护消费者的利益是没有问题的，就怕监管一刀切。有文档就好办多啦。

[OKRs are not "BAU"](https://www.whatmatters.com/series_entries/s2-3-okrs-bau-business-as-usual/)

应该避免 Business-as-usual OKRs。

OKR 的目的是让团队现有的状况变的更好，而不是维持当前的状态。

# Tips

使用 Keynote 演示的时候，会有一些情况需要展示鼠标指针，默认在演示页面是不会展示出来鼠标指针的。如果用双屏演示的话，可以看到演讲者屏幕上有一个 ？ 样式的帮助按钮，在其中，我们能看到很多快捷键的介绍，其中，可以发现，按下 `c` 键，就可以在演示屏幕展示鼠标指针啦。是一个很方便的功能，但是经常被忘记呢。

# Share

遇到了一个 CursorWindowAllocationException 的问题，零零散散记录一下当时查到的点。后面有空再整理吧。问题还没解决，头大。

先从报错的 ErrorNo 入手看看是什么意思

```kotlin
/**
 * [errno(3)](http://man7.org/linux/man-pages/man3/errno.3.html) is the last error on the calling
 * thread.
 */
#define errno (*__errno())
```

> RETURN VALUE top
Usually, on success zero is returned. A few ioctl() requests use
the return value as an output parameter and return a nonnegative
value on success. On error, -1 is returned, and errno is set to
indicate the error.

[ioctl(2) - Linux manual page](https://man7.org/linux/man-pages/man2/ioctl.2.html)

[](https://cs.android.com/android/platform/superproject/+/master:frameworks/base/libs/androidfw/CursorWindow.cpp;l=44;drc=master;bpv=1;bpt=1)

```kotlin
status_t CursorWindow::create(const String8& name, size_t size, CursorWindow** outCursorWindow) {
```

[](https://cs.android.com/android/platform/superproject/+/master:system/core/libutils/include/utils/Errors.h;l=30;drc=master;bpv=1;bpt=1)

```kotlin
/**
 * The type used to return success/failure from frameworks APIs.
 * See the anonymous enum below for valid values.
 */
typedef int32_t status_t;
```

> 9 EBADF Bad file descriptor. A file descriptor argument was out of range, referred to no open
file, or a read (write) request was made to a file that was only open for writing (read-
ing).

可以用这个命令看看 errno 分别都有哪些值，对应哪些意思。

`man errno`

页面上新增了一个使用了 GifView 的自定义  View。

是 Layout 的问题？一点一点注释掉看看。

复制几个新增的自定义 View，感觉是更容易复现问题了。

完全注释掉新增自定义 View，问题比较难复现了

会和 FD 有关系吗？

看机器复现的时刻，APM 的监控数据，看起来和 FD 的关系不大

nativeUsedMemory 看起来也比较正常

开线程疯狂写也没有触发同样的问题

看起来是没写到数据库，参数没配置对

配置对之后，疯狂写还是没有复现。写巨长的字符串也没有复现。

[https://issuetracker.google.com/issues/170228126?pli=1](https://issuetracker.google.com/issues/170228126?pli=1)

会是 Room 的 Bug 吗
