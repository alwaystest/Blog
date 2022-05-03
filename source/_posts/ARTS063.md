---
title: ARTS063
date: 2022-05-03 16:37:23
tags: ARTS
---

新冠三年，形势愈发紧张了。

<!--more-->

# A.R.T.S 063

# Algorithm

ignore

# Review

[Hadoop、Hive、Spark 之间是什么关系？](https://cloud.tencent.com/developer/article/1042387)

在公司的数据仓库里面查一些东西，发现有个选项，可以选择查询引擎，里面有 Hive，MySQL，Spark 等等好几种选择，该怎么选呢？

这篇文章对于门外汉来说够用了。Glance。

[Exceptions in coroutines](https://medium.com/androiddevelopers/exceptions-in-coroutines-ce8da1ec060c)

[破解 Kotlin 协程（4）：异常处理篇](https://www.bennyhuo.com/2019/04/23/coroutine-exceptions/)

[Kotlin Coroutines(协程) 完全解析（四），协程的异常处理](https://johnnyshieh.me/posts/kotlin-coroutine-exception-handling/)

Exception 处理真是常看常新，这次是 Kotlin 协程的 Exception 传递问题。

> Coroutine builders come in two flavors: propagating exceptions automatically ([launch](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/launch.html) and [actor](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.channels/actor.html)) or exposing them to users ([async](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/async.html) and [produce](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.channels/produce.html)). When these builders are used to create a *root* coroutine, that is not a *child* of another coroutine, the former builders treat exceptions as **uncaught** exceptions, similar to Java's `Thread.uncaughtExceptionHandler`, while the latter are relying on the user to consume the final exception,
>

为什么 async 作为 root coroutine 的时候就可以把 exception 延迟到 await 的时候处理，不是 root coroutine 的时候则会马上崩溃呢？

起初还以为是 async 类似于异步调用，就像 try catch 无法捕获 post runnable 中的异常一样。结果看了半天源码 + 字节码，发现不是这么回事，而且越看越乱，花了一整天时间，依然没啥头绪。

在放弃的边缘，试着搜索了一下 kotlin coroutine exception 传递 相关的话题，终于找到了原因所在。

因为 **Exception propagation** 的原因，当使用 async 创建的 coroutine 不是 root coroutine 时，Continuation 中发生异常，会传播到父协程中去，不出意外的话（不要搞 Supervisor），root coroutine 的实现类是 StandaloneCoroutine。这个类继承了 AbstractCoroutine，并且重写了 handleJobException 方法，会处理 Exception，比如调用到 uncaughtExceptionHandler，这种情况下，Android 系统会默认做崩溃处理。

但是 async 创建的 coroutine 作为 root coroutine 就不同了，实现类是 DeferredCoroutine，这个类同样继承自 AbstractCoroutine，但是没有重写 handleJobException 方法，是不会把 exception 告诉 uncaughtExceptionHandler 的。这个时候，调用 await 方法，会把 CompletedExceptionally 结果中包含的 Exception 重新抛出，这个时候就可以 catch 到对应的异常了。

# Tips

> 在华为平板上，可以通过调整系统设置中“字体和显示大小”，放大界面，这个时候这个方法的获取结果就是 false，将平板误识别为手机
> 

[Android 判断当前设备是手机还是平板的最有效的方法_Fantasy丶Lin的博客-CSDN博客_android 判断是平板还是手机](https://blog.csdn.net/fantasy_lin_/article/details/111828002)

最近发现 Vim 的 Kotlin Language Server 无法启动了，查看错误日志发现是 JVM 版本过低，导致无法加载 Class 文件导致的。之前一直在用 Java 1.8，配置 M1 环境的时候尝试过直接在 Java 11 的环境下跑项目，发现由于 Java 9 开始的 Jigsaw 会导致某个类找不到，具体忘记是哪个类了。是时候再试试 Java 11 啦。整起！

# Share

ignore

