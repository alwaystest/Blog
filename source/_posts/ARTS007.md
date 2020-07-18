---
title: ARTS007
date: 2020-07-19 00:58:11
tags: ARTS
---
# A.R.T.S 007
<!--more-->

# Algorithm

[Binary Tree Right Side View - LeetCode](https://leetcode.com/problems/binary-tree-right-side-view/submissions/)

Random Medium

从思路上来看是简单的，广度优先遍历即可，UT + Impl。

最近用 TDD 写算法真的是越来越顺了。

用了 Kotlin 的高阶函数，中间的开销不小，第一个方案 Perf 几乎垫底。重构了一波换了数据结构，手动操作集合，Perf 能入眼了，应该还有优化空间。

# Review

[](https://developer.android.com/topic/libraries/app-startup)

玉刚说的公众号里面有一篇文章讲 LeakCanary 的，其中提到了 JetPack 的 app-startup lib

于是来了解了一下用法。

之前有好多库都利用了 ContentProvider 的启动顺序比较靠前来做自己的初始化工作，这样也避免了过多的侵入业务代码。

但是 ContentProvider 是在应用启动的时候通过反射进行对象初始化的，因为是四大组件之一，启动后还会有很多别的操作，如果每个 Lib 都搞一个 ContentProvider 的话，会严重的拖慢启动速度。

这个库将 ContentProvider 初始化的开销压缩为了一个，并且增加了依赖声明，这样就可以按照顺序来初始化。不过反射创建对象还是避免不了的。

当然，如果连剩下的这个 ContentProvider 的开销也想省掉的话，这个 lib 也提供了手动初始化的方式。

# Tips

还是在 LeakCanary 的源码中学到了一招。

之前在 Java 中要实现一个定义了多个方法的 Interface，需要把所有的方法都声明出来，哪怕是空实现。于是就出现了提供空实现的 Adapter 的写法，这样子类只需要 override 需要覆写的方法即可。

使用 Kotlin 的 Delegation + 动态代理，可以很方便的增加接口的空实现，还可以 Override 对应的方法提供具体实现。这样就不用再搞一个 XXXAdapter 来提供接口方法的空实现了。

```kotlin
inline fun <reified T : Any> noOpDelegate(): T {
    val javaClass = T::class.java
    val noOpHandler = InvocationHandler { _, _, _ ->
      // no op
    }
    return Proxy.newProxyInstance(
        javaClass.classLoader, arrayOf(javaClass), noOpHandler
    ) as T
  }
```

当然 Interface 的源码在自己掌控中的时候，也可以提供接口的默认实现。Java 8 或 Kotlin 支持。

之前有看到别人的 Gradle Project 里面还单独定义了 Clean Task，本来不知道有啥用，毕竟 Android 开发的时候，Clean 这个 Task 一直是存在的，不需要我们手动再定义一次了。直到我手动新建了一个空的 Gradle Project。原来 Clean 不是 Gradle 默认就带的 Task。这种情况下，想要快速清理 build 文件夹，就需要手动定义一个 Task，如下：

```groovy
task clean(type: Delete) {
    delete rootProject.buildDir
}
```

# Share

[Android 上的链式唤醒](/2020/07/19/ChainingWakeupOnAndroid)
