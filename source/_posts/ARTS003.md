---
title: ARTS003
date: 2020-06-13 15:32:38
tags: ARTS
---
# A.R.T.S 003

# Algorithm

[Sliding Window Maximum - LeetCode](https://leetcode.com/problems/sliding-window-maximum/)

又是一道 Hard，取滑动窗口中的最大值。乍一看不难，暴力过一遍就好了。看到有个提示，能不能达成线性时间复杂度？于是想了半天。

没有思路，决定先用暴力方式走一遍，按照惯例，还是先写了单元测试，再完成方法实现。

有测试用例之后验证实现确实非常的方便呀，不过太依赖测试用例的话，细节的地方脑子没想清楚就去跑用例了，不是一个太好的习惯。理想的情况应该是先在脑海中过一遍，没有问题了再跑测试用例。养成二话不说就跑测试用例的坏习惯后，怕是手写算法容易吃亏。

Anyway，暴力方法果然超时了。再考虑优化方案。其实挺像业务开发的，先搞出一套能用的系统，然后再进行细节优化。

有一句话是这么说的，方向错了，再努力也是浪费时间。这点挺难权衡的，现在做 Hard 难度的算法题，总觉得有什么取巧的办法可以搞，迟迟无法下手，但最终的答案大部分又是走的暴力解的思路加上一点点优化，难搞。

一开始想到了用最大堆来搞，但是没想到怎么移除即将出队的元素，另外自己实现一个最大堆似乎有点繁琐，想到了 HashMap 内部有红黑树相关的实现，于是找到了 `TreeMap` 这个轮子，加上引用计数，简单搞定。

时间 50%，空间只比 38.23% 的人少，看起来还有优化空间。

看了一个印度🇮🇳小哥的[思路](https://leetcode.com/problems/sliding-window-maximum/discuss/677549/Java-O(n)-solution-with-clear-explanation-using-WhiteBoard)，挺有意思的。用数据结构来存储 Index，同时还能利用传进来的参数是数组的特性。Index 还不会重复，避免了处理数据重复带来的复杂度。

# Review

[Kotlin From Scratch: Exception Handling](https://code.tutsplus.com/tutorials/kotlin-from-scratch-exception-handling--cms-29820)

[Exceptions](https://kotlinlang.org/docs/reference/exceptions.html)

理了一下 CheckedException 和 UnCheckedException，一直依赖 IDE 提示，都快忘光了。

> Checked exceptions are exceptions that are checked at compile time.

编译器会检查 CheckedException。

调用一个声明了 throws exception 的方法但没有 try ... catch 或者重新 throw。

或者方法实现中抛出了 Exception 但是没有声明方法会抛出 Exception。

结果都是无法编译成功。

Kotlin 中，所有的 Exception 都是 UnCheckedException。编译器不会检查一个方法抛出的 Exception 是否被 Caller 处理。所以 `@Throws` Annotation 在 Kotlin 中几乎相当于一个摆设。只有在 Java 调用的时候，才会作用为 CheckedException。如果在 Kotlin 中调用，连个高亮警告都没有，感觉是个坑。

找了一下，还真的发现了一个轮子可以直接用。

[Csense - kotlin checked exceptions - Plugins | JetBrains](https://plugins.jetbrains.com/plugin/12673-csense--kotlin-checked-exceptions)

# Tips

Kotlin 的 protected 方法的可见性相当与 private + visible in subclasses

所以需要单元测试的方法不能用 protected 来修饰

![Tips001.png](tips.png)

---

当一个方法被标注了 `Throws(IOException::class)` ，而又没有在使用 `[Dispatchers.IO](http://dispatchers.IO)` 的 CoroutineContext 中调用，IDEA 会提示 `Inappropriate blocking method call`

如[这里](https://discuss.kotlinlang.org/t/warning-inappropriate-blocking-method-call-with-coroutines-how-to-fix/16903/5)所说，IDEA 认为这个方法是一个 IO 方法，需要使用 IO Thread Pool 来执行，否则因为 Blocking IO 导致当前工作的线程被阻塞，导致线程复用率下降。

但是我不赞成他说的删除掉 Throws 注解，这里如果有可能抛出 Exception，还是需要通知 Caller 的，如果连通知都没有，那发生问题的概率就比较大了。

# Share

[TDD 的一些实践](/2020/06/10/PracticeOfTDD)
