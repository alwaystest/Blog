---
title: 'RTFSC: ResponseBody'
date: 2020-06-05 23:50:14
tags: Programming
---

# RTFSC: ResponseBody

上回说到 `ResponseBody` 持有的是数据流，而不是数据。
<!--more-->

由此想到了另一个问题，当网络请求出错时，我们会在 UI 线程中调用 `Response.errorBody.string()`，这样的话，如果从 TCP 连接中读取信息，岂不是会阻塞 UI 线程？

看了半天源码没看出个所以然，想查查有没有现成的分析，发现了这么一篇简单的分析，看来我不是第一个踩到坑里的，先记录一下。

[OkHttp踩坑记：为何 response.body().string() 只能调用一次？](https://juejin.im/post/5a524eef518825732c536025)

这篇文章才算真正帮我理清了思路，看到了 `string()` 实际上对应了从底层 Socket 读取数据的操作。所以在 UI 线程调用 `Response.errorBody.string()` 应该是有风险的。

[OKHttp源码解析(6)----拦截器CallServerInterceptor](https://juejin.im/post/5c0791695188251ba9057d23)

为什么说是有风险呢？

不严谨的来讲，这里的 Socket 底层可以对应一个 TCP 连接，而系统底层会对 TCP 连接维护一个接收缓冲区，当数据量足够小的时候，在 UI 线程读取 Socket 中的内容，应该还是比较快的，但是这里可能还会存在切换内核态以及挂起等待数据拷贝到应用内存的消耗，所以放到 UI 线程还是不够经济的。但是当 `errorBody.string()` 对应的数据量比较大，缓冲区无法全部存储时，接收方 TCP 滑动窗口大小为 0，此时发送方不再发送数据，等待新的窗口通知。在这种情况下，应用层调用 `errorBody.string()` 会从 Socket 中读取数据，这么一来，相当于继续从 TCP 连接中读取信息，一旦网络情况不好，就会发生阻塞的情况。如果是在 UI 线程中调用这个方法，emm……

既然前人已经栽了树，分析了整个代码的调用流程，那我就乘个凉，不再继续分析了。倒是可以考虑给个图示，方便将来快速回顾，毕竟跟着代码走半天还是有点费事的。

![http://www.plantuml.com/plantuml/png/0/XLFBJiCm4BpxA_RG0m-v5TSUgj98Y0C7rF83njcKYCIExBK05Svy1KSkV1ZYCyYpSHw4N6BjUcPs9fkr9M78fGcJYR0MjYYIG2k5acAjuC0WVnOBk0jkeQsNLRxJyX49Z7YgJcLrFeS68mqAGeYqINkN6gZrSMCmxmBVk2X2W-5EEoCnRnKlziRgoQ-9iej0XoZRgRy_Fd_Ultgc_c7pU3XVzGDPWhF7srvcMxIPLXOtEdnfzLkksLNyZE4DgZqzL99JOvGjth0O96UKtpklpYUdhVsH0Me6SM_924bm17NppkCE8J8wxIgGYTM-KXWOvFnrrL4SVeJPPIrREpiCJ8sf5bNVrLI00qefaWIioz3qoIPfIZD_3zv1zth5WdZxqbsEaTJE9WqjGMf58bQMiJMgCFyhVm40](http://www.plantuml.com/plantuml/png/0/XLFBJiCm4BpxA_RG0m-v5TSUgj98Y0C7rF83njcKYCIExBK05Svy1KSkV1ZYCyYpSHw4N6BjUcPs9fkr9M78fGcJYR0MjYYIG2k5acAjuC0WVnOBk0jkeQsNLRxJyX49Z7YgJcLrFeS68mqAGeYqINkN6gZrSMCmxmBVk2X2W-5EEoCnRnKlziRgoQ-9iej0XoZRgRy_Fd_Ultgc_c7pU3XVzGDPWhF7srvcMxIPLXOtEdnfzLkksLNyZE4DgZqzL99JOvGjth0O96UKtpklpYUdhVsH0Me6SM_924bm17NppkCE8J8wxIgGYTM-KXWOvFnrrL4SVeJPPIrREpiCJ8sf5bNVrLI00qefaWIioz3qoIPfIZD_3zv1zth5WdZxqbsEaTJE9WqjGMf58bQMiJMgCFyhVm40)

另外有一个错误的认知被纠正了一下，OkHttp 的 enqueue 的回调不是在 UI 线程中的，而是还在线程池线程中。

看了一眼 `SuspendForBody` 这个类，对 `Continuation` 和 `Call` 使用了适配器模式。Kotlin 底层毕竟还是依赖 JVM，协程最终也是对 `Continuation` 这个类的使用。

可以看到 `retrofit2.KotlinExtensions` 中的操作，`enqueue` 之后，`onResponse` 的回调还是不在 UI 线程中的，此时可以进行耗时操作，比如这里可以读取 `response.body()` ，然后让协程 `resume()` 来继续往下走。这样才是正确的使用方式。

这个 CallAdapter 像套娃一样套了一层又一层，为了防止 `SuspendForBody` 的 callback 被调到的时候，`DefaultCallAdapter` 已经把线程切换回 UI 线程，甚至还在 `parseAnnotations` 阶段为 SuspendFunction 的情况做了特殊处理，强行添加了 `SkipCallbackExecutor` 注解，跳过之前的线程调度。

果然细节是魔鬼，想了解高性能的代码是怎么写出来的，还是需要 RTFSC。

# Refs

[一个TCP发送缓冲区的问题引发的解析](https://elsef.com/2020/02/29/%E4%B8%80%E4%B8%AATCP%E5%8F%91%E9%80%81%E7%BC%93%E5%86%B2%E5%8C%BA%E7%9A%84%E9%97%AE%E9%A2%98%E5%BC%95%E5%8F%91%E7%9A%84%E8%A7%A3%E6%9E%90/)
