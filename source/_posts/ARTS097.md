---
title: ARTS097
tags: ARTS
date: 2023-06-22 17:20:27
---

*写写诗吧*

长大后，乡愁是一个圆圆的碗

胃在家里，心在远方

<!--more-->

# Algorithm

none

# Review

Android UnitTest Meet JUnit5

[Testing on Android using JUnit 5 | Lord Codes](https://www.lordcodes.com/articles/testing-on-android-using-junit-5/)

[Android Unit Testing with JUnit5](https://medium.com/@boonkeat/android-unit-testing-with-junit5-d1b8f9c620b6)

# Tips

遇到一个 PowerMock 和 TestCoroutineScheduler 同时使用的问题。

使用 `runTest` 方法运行单元测试的时候抛出了 NPE，看 coroutines-test 的源码，是在 `runTestCoroutine` 的时候通过 coroutineContext 获取 TestCoroutineScheduler 对象，正常情况下，这里的获取是不会有问题的。

在使用 PowerMockRule 的情况下，同时在声明时创建 TestDispatcher 对象，就会导致 TestDispatcher 内引用的一个单例对象和运行时不一致。

根本原因是 PowerMock 会改变 ClassLoader，创建对象，尤其是看起来像是单例的对象时，一定要注意 ClassLoader 是否会发生改变。

延迟初始化 TestDispatcher，保证他们使用的是同一个 ClassLoader 即可解决这个问题。

# Share

none
