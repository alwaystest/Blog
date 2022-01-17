---
title: ARTS054
date: 2022-01-17 23:04:46
tags: ARTS
---

过了腊八就是年？你倒是放假呀！

<!-- more -->

# Algorithm

[Linked List Cycle - LeetCode](https://leetcode.com/problems/linked-list-cycle/)

Easy，很常规的思路，甚至可以先写实现，然后再补单测。

又好久没写算法了，算是热身吧。

# Review

[Handling versions which change over time](https://docs.gradle.org/current/userguide/dynamic_versions.html)

应该使用 `-refresh-dependencies` 参数来刷新 snapshot 包，而不是设置 `cacheChangingModulesFor` 为 0 秒，有缓存还是要用起来的。

## Tips

ConstraintLayout 的 measure 逻辑还是比较复杂的，线上遇到了一个卡顿的问题，排查发现是在一个 View 嵌套层级很深的地方通过 Animator 改变 View 的高度，从而实现类似音量条变高变低的动画导致的。当页面嵌套的层级比较深的时候，尽量不要频繁的改变 View 的大小，可以考虑使用自定义 View，重写 onDraw 的方式来实现动画，降低 Layout 的频率，提升性能。当然最好用的方式还是想办法降低 View 的嵌套层级。不过业务复杂起来之后，降低嵌套层级还是不太容易搞的。

Android Studio 提供的 Profiler 查看 CPU Trace 在 Sample 模式下对于堆栈的判断会有问题，看到一个不停的 post 到主线程执行的逻辑被识别成了递归调用。这种情况还是看 SystemTrace 或者直接 TraceJavaMethod 更准确，但是 Trace 会极大的影响性能，并且结果文件也会很大，解析起来也比较复杂，会有卡死 AndroidStudio 的可能。

不断放大缩小，或者移动 Trace 的时间范围，Android Studio 会不断的计算各个方法的耗时比例，如果 Trace 文件过大，或者选择的范围经常变来变去，可能会导致 AndroidStudio 不断的尝试计算，占用 100% 的 CPU。可以试着先选定一个比较小的方法，然后再左右移动，这样计算会快很多。

总感觉 Profiler 的体验没怎么被重视的样子，怎么会卡成这个样子。好像充满了程序员的风格，能用就行，卡不卡，好不好用的，之后再说。

# Share

鸽
