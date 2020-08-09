---
title: ARTS009
date: 2020-08-09 18:16:54
tags: ARTS
---
# A.R.T.S 009
<!--more-->

# Algorithm

[Reorder Data in Log Files - LeetCode](https://leetcode.com/problems/reorder-data-in-log-files/)

Easy。但是读题有些费事。一旦烦躁起来，就比较难注意到细节了。

Tie 除了领带的意思，还有打平，相等的意思。读题的时候没有注意到 `in case of ties` 这句话，导致浪费了一次提交机会。

查过之后才想起来似乎美剧里面经常能听到这个单词的这种用法，嗯，还得多听多练。

# Review

[深入理解 Swift 派发机制](https://kemchenj.github.io/2016-12-25-1/)

出于好奇，想了解一下 Swift Extension 和 Kotlin Extension 的区别。

不敢托大，先找到一个英文资料的译文，貌似原文网站打不开了。摘录了一个新的知识点，之前倒是没有注意过函数派发方式这个概念。这篇文章看完还是不太懂，不过感觉我更应该先理解 Java 的 Method Dispatch 机制，需要结合着再看一次《深入理解 JVM 虚拟机》。

> 编译型语言有三种基础的函数派发方式: 直接派发(Direct Dispatch), 函数表派发(Table Dispatch) 和 消息机制派发(Message Dispatch)

Java 默认使用函数表派发, 但你可以通过 final 修饰符修改成直接派发. C++ 默认使用直接派发, 但可以通过加上 virtual 修饰符来改成函数表派发. 而 Objective-C 则总是使用消息机制派发, 但允许开发者使用 C 直接派发来获取性能的提高.

# Tips

Review 学技巧之判断 Kotlin 的 lateinit property 是否被初始化

之前想过是否有办法判断，但是没有深究，只是觉得这个东西没有初始化就使用会报错，但是又不提供一个检测的办法，不太合理。原来还是我太懒了，没有查文档。

```kotlin
lateinit var t: Any

this::t.isInitialized
```

高级语言的语法糖还真的是难搞，换一个写法，底层实现就可能大变样。

`this::t` 和 `this::.isInitialized` 生成的字节码基本上完全不同。

# Share

[TypeToken 和泛型擦除](/2020/08/09/TypeTokenAndGenericsTypeErasure)
