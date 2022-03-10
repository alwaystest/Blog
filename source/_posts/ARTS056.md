---
title: ARTS056
date: 2022-03-10 18:41:34
tags: ARTS
---

历时三个月终于看完了 Learn VimScript the Hard Way。现在我感觉自己开始入门了。

<!--more-->

# Algorithm

过

# Review

[一文带你理解URI 和 URL 有什么区别？](https://mp.weixin.qq.com/s/nuwYGdVLhi3hWq25o9kvuA)

URI 和 URL 的区别

一般很少用 URN，就忽略了吧，主要是命名的时候经常分不太清楚什么时候使用 URI，什么时候用 URL。

> URI 是抽象的定义，URL 和 URN 是具体的实现
> 

言简意赅

[GitHub - mpierucci/Android-Redux: Redux like unidirectional data flow architecture proposal](https://github.com/mpierucci/Android-Redux/)

大致看了一下这个 Redux 的代码，感觉 Redux 把事情搞的太复杂了。这个取舍还是需要好好衡量的。

[初窥门径之JUnit源码分析](https://nicky-chin.cn/2018/07/16/junit-codeanalysis/)

还愿2

很早之前看到有大佬说建议看看 JUnit 的初版源码，便于了解其底层的运行逻辑。今天终于先过了一遍。

这个版本的逻辑看起来还是挺简单的。和目前项目中运行的单元测试流程还是有一些区别，比如测试类需要继承 `TestCase` ，测试方法需要以 `test` 作为前缀。没有看到注解相关的东西。不过要搞

另外还有一点是需要看看 Gradle 是怎么把参数传给 `TestRunner` 的。

# Tips

技术落后了好多。

才发现 Robolectric 提供了 ShadowLooper，可以方便的对 Handler 相关的特性进行测试。

实现原理也比较简单，自己搞一个任务队列，提供一些对外暴露的便于控制时间轴的 API，棒！又学到了。

看滴答清单的记录，这是一个一年前就想了解的问题。

Vim 的 Buffer, Window, Tab 的概念我之前一直分不清楚，现在终于看明白了。直接 `:h window` 就可以看到这条说明。

> In summary, buffer is in memory text of a file, window is the viewport of buffer, a tab page is a collection of windows.
> 

# Share

过
