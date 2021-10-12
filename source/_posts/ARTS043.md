---
title: ARTS043
date: 2021-10-12 21:04:16
tags: ARTS
---

Bash 真是一门严谨的语言，操作符左右两边有没有空格完全是两个意思。绝了。

<!--more-->

# Algorithm

[Two Sum - LeetCode](https://leetcode.com/problems/two-sum/)

在 Vim 里面配置了 coc + coc-kotlin。先写一个 easy 的热热身。

写代码倒是没啥问题了。配合 Gradle 跑单测还没跑通。

比起一个 Idea 要爽多了。

# Review

[Parameter expansion](https://wiki.bash-hackers.org/syntax/pe#parameter_expansion)

中文名称叫参数扩展。看一个脚本的时候学到的，可以节省很多工作量。前提是要熟悉这玩意能干啥。

[AndroidX Activity Result APIs -  The new way!](https://medium.com/droid-log/androidx-activity-result-apis-the-new-way-7cfc949a803c)

需要跟进一下新技术了，才看到去年就已经发出来的改动。

感觉这篇文章讲的比较浅，具体底层怎么实现的，为什么要用新的 API 没说的特别清楚。

[掘金](https://juejin.cn/post/6887743061309587463)

找了一篇掘金的文章，看起来是讲清楚了两种调用方式的区别。感觉上是为了 SRP 原则，把一个简单的 API 调用做了一些设计。这就是工程上的取舍吧。

[掘金](https://juejin.cn/post/6959818308644241422)

在这篇文章带领下，简单过了一下源码实现，感觉上大致是结合了 Lifecycle 组件，在 Activity 处理 dispatchResult 的地方做了一层拦截。整体上和 AMS 交互的部分还是没有变的。只是 Client 这边实现的时候换了个写法，不这样的话也没办法保持向前兼容呀对不对。

# Tips

> in lieu of starting an activity.
>

在文档里面学到一个单词，感觉可以用 `instead of` 也能表达同样的意思。

# Share

鸽
