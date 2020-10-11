---
title: ARTS016
date: 2020-10-11 21:03:22
tags: ARTS
---
# A.R.T.S 016
<!--more-->

# Algorithm

[Longest Palindromic Substring - LeetCode](https://leetcode.com/problems/longest-palindromic-substring/)

练习 dynamic programming。

判断回文的思路没有归纳出来。还得继续练。倒是想到了暴力解的思路，但是做题的时候精力没有集中，一时半会没想到奇数项回文和偶数项回文怎么归纳为一种模式的思路。空想还是不行，画图列算式才是正途。

看到一个 Manacher's Algorithm 算法，找了一下解析了解详情，发现这里讲解的不错。

[老司机开车，教会女朋友什么是「马拉车算法」-五分钟学算法](https://www.cxyxiaowu.com/2665.html)

# Review

[理解协程、LiveData 和 Flow](https://juejin.im/post/6844904158466670599?utm_source=gold_browser_extension#heading-4)

看了一篇中文文章，我们的 LiveData Lib 版本略旧，需要升级一下，把新版提供的好用的 API 玩起来。

[What the Flows: Build an Android app using Flows, Live Data, and MVVM architecture](https://medium.com/@shivamdhuria/what-the-flows-build-an-android-app-using-flows-and-live-data-using-mvvm-architecture-4d3ab807b4dd)

[Implementing Search Filter using Kotlin Channels and Flows in your Android Application](https://proandroiddev.com/implementing-search-filter-using-kotlin-channels-and-flows-in-your-android-application-df7c96e58b19)

简单了解了一下协程的 Flow 和 Channel 的用法。具体在生产环境中如何结合下来刷新和 LoadMore 的操作还需要实践一下。

# Tips

[另类 BadTokenException 问题分析和解决](https://mp.weixin.qq.com/s/DmN9sN_MdugpVm73vnVqww)

从这篇文章中发现了 `android.os.Looper#mLogging` 这个字段，又是一个卡顿监控的思路，有空还是应该多看看源码。

`android.os.Looper#dump(android.util.Printer, java.lang.String)` 可以把消息队列的内容 dump 出来，来观察和分析具体情况。

# Share

🕊️
