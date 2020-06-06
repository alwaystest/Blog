---
title: ARTS002
date: 2020-06-07 00:44:18
tags:
---

# A.R.T.S 002

# Algorithm

随机了一道，标记是 Hard，把我吓到了，以为要用什么特别取巧的办法，想了半天没啥思路。

看了一眼 Solution，发现居然第一个方法居然以力破巧，是我想多了。于是开始试着自己实现。

试了一下 TDD 的方法来写，有些不适应，还是容易没写单测就写完实现了。然后写完单测发现方法实现确实有问题😅️

发现一个比较爽的点是如果测试用例设计的不错，那可能剩下需要解决的问题就是超时了。

我本来按照暴力方法来实现，提交的时候已经做好了超时的准备，但是提交以后居然发现过了，时间 50% 空间 50%。嗐，我代码里还留了个 TODO 呢，折腾了这么久，既然过了，那就先这样吧😔️

[Making A Large Island - LeetCode](https://leetcode.com/problems/making-a-large-island/)

TDD 的提交过程可以看看[这里](https://github.com/alwaystest/Algorithms/commits/master)

顺便总结了一下 TDD 实践过程中的感受，下周的 Share 好像已经提前完成了呢。

# Review

Android Studio 4.0 出来了，最近没怎么追 Preview 版本的 Android Studio。看看更新文档吧。

[Android Studio 4.0](https://android-developers.googleblog.com/2020/05/android-studio-4.html)

列一下关注的点吧：

MotionLayout 看起来有点意思，可以用 XML 的方式定义复杂的动画，这波加上了可视化的工具 MotionEditor 来生成，但是我感觉最靠谱的还是得自己写。

新增的 LayoutValidation 看起来可以让我们更方便的检查小屏适配的问题，还没有体验用起来怎么样。一眼看到多个屏幕适配的情况，还是比较高效的，之前需要不停的切换，有点烦。

新的 Profiler 试了一下感觉很不习惯，再看看吧。增加了 WASD 按键操作的功能，还没试。

据说是可以用全部的 Java8+ 语法了，需要先升级 AGP 到 4.0.0，可能 AAR 可以先尝尝鲜，不过有了 Kotlin，谁还稀罕 Java8+ 的语法呢？

发现一个 Bug，Android Studio 的 Preference 调不出来了，还想看看 Kotlin 的 Live Template 增加了些什么东西呢。想念写 Java 的时候可以用 `logd` 来打 Log 调试，现在被迫用 `sout` 了。

# Tips

遇到一个小坑

`okhttp3.ResponseBody` 写明了 

> <h3>The response body can be consumed only once.</h3>

打 Log 的时候把 ResponseBody 的内容 Consume 掉了，导致真正需要 ResponseBody 的地方读取不到需要的数据。

看了一下原因，注释里面写的很清楚了`ResponseBody` 这个类可能读取到一个占用内存巨大的资源，所以用持有数据流的方式代替了把响应加载到内存中的方式，这个类被特地设计成了只能读取一次。

试用了 Android Studio 4.0

新 LayoutInspector 不好用，有一个 Legacy Layout Inspector 选项可以挽救一下。

Profiler 也很难用，不容易直接看到耗时方法

整体感觉变的卡顿了一些。

# Share

[RTFSC: ResponseBody](https://blog.alwaystest.ml/2020/06/05/RTFSC-ResponseBody/)
