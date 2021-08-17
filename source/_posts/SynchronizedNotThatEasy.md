---
title: SynchronizedNotThatEasy
date: 2021-08-17 22:31:05
tags: Programming
---

排查线上崩溃，遇到了一个被自己蠢哭了的问题。

一年级的面试知识就能搞定

<!--more-->

# ConcurrentModificationException 1

首先来看遇到的第一个问题

```kotlin
interface Logger {
    fun println(msg: String)
}

object Logger {

    private val loggers = HashSet<Logger>()

    @Synchronized
    fun addLogger(l: Logger) {
        loggers.add(l)
    }

    @Synchronized
    fun removeLogger(l: Logger) {
        loggers.remove(l)
    }

    @Synchronized
    fun log(msg: String) {
        loggers.forEach {
            it.println(msg)
        }
    }
}
```

`loggers` 是 private 的。

`addLogger`  `removeLogger` `log` 都加了 `synchronized` 看起来一切都很美好。

然鹅，线上发生了崩溃：ConcurrentModificationException。发生在 log 方法中。我们知道，forEach 只是一个语法糖，底层是调用 Iterator 来实现对容器的遍历，Iterator 为了避免多线程同时操作容器的时候导致遍历出问题，采用了 FailFast 机制，只要 `modCount` 和预期不一致，就会直接抛错，拒绝继续执行。难道一个线程已经获取了锁的情况下，还能有另一个线程进来修改 logger 吗？

写了一段测试代码，用多个线程不停的 add、remove、log，一直没有复现这个崩溃。

甚至怀疑这个 Synchronized 注解在 Class 文件中有没有生效，使用 Show Kotlin Byte Code 功能查看对应的字节码，发现方法声明中确实是有 synchronized 的。这就神奇了。只能先在 add 和 remove 的方法中上报了堆栈，等线上用户复现的时候分析日志。

相信聪明的读者已经猜到了问题发生的原因。其实特别特别简单。给自己一分钟思考一下。

== 一分钟分割线 ==

根据堆栈分析，很快就找到了问题发生的原因。

确实是发生了 forEach 中调用了 add 或 remove。Synchronized 也是有效的，那么唯一的方式就是同一个线程在 log 的同时又调用了 add 或 remove。一个线程又不能同时做两件事，那唯一的发生方式就是在 logger 的实现中，println 调用了 add 或者 remove。

不得不说，这样的代码真是绝了。光看这里的代码，下意识的就认为 println 的实现只会老老实实的打印 msg，谁能想到他还会做别的操作呢？

![usage](usage.gif)

归根结底，还是校招面试就会问的问题，使用 Iterator 的时候，怎么移除其中的元素而不会触发 ConcurrentModificationException。只不过代码逻辑复杂之后，加上一些想当然的猜测，就导致一个很简单的问题在线上持续了至少一个版本才得到解决。

# ConcurrentModificationException 2

有了上面的经验，我们来继续看看第二个问题。

```kotlin
object Context {

    val params: MutableMap<String, String> = Collections.synchronizedMap(mutableMapOf())
}

object Logger {

    fun log(msg: String) {
        val params = mutableMapOf("msg" to msg)
        params.putAll(Context.params)
        println(params)
    }
}
```

这是线上发生的另一个问题，你看，这里的写法看起来真是非常完美呢，我们给 Map 套了一个 SynchronizedMap，那么我们对 `Context.params` 的操作一定就是线程安全的了。

然而现实就是这么打脸，在 Logger 中的 putAll 又出现了 ConcurrentModificationException。

我们看一下 HashMap 的源码，putAll 中是对参数进行了 Iterate，也就是说，抛出异常的是 `Context.params` 这个 map 对象。

OMG，不是 synchronizedMap 吗？

putAll 方法是通过 `map.entrySet()` 获取了一个 Iterator，然后进行遍历。

细细的品一下这个 SynchronizedMap 的源码。没问题，entrySet 也返回了一个 SynchronizedSet 对象，很稳，是线程安全的。那就怪了，为啥线程安全还会抛异常呢？

顺着上一个 Bug  的思路，又排查了一遍，Iterate 的过程中，并没有方法调用来修改 `Context.params` 的内容。

难道是源码在骗我？SynchronizedMap 不是 Synchronized？

再来思考一分钟

== 一分钟分割线 ==

由于崩溃是偶现，看了半天也没什么思路，索性放一放，没准下次再看的时候一下就抓住重点了呢。

几天后

![forgot_all](forgot_all.png)

那么我们重新来看这个问题。

发生了这种情况，必然还是在 Iterate 的过程中，修改了 Map 的内容。那关键就在于，为啥 Iterate 的过程中，SynchronizedMap 的 mutex 没上锁呢？

顺着源码爬呀爬，爬到了 `SynchronizedCollection`

```java
public Iterator<E> iterator() {
    return c.iterator(); // Must be manually synched by user!
}
```

你瞅瞅这个注释！这就是我在 CodeReview 的时候一直反对行尾注解的原因！

你写在这里，哪个瓜娃子会来看呦我的天。

然后翻回去看类的注释，emm，我错了，我就应该先看注释的。这就是写注释的程序员和不写注释的程序员的区别了。不写注释就不会习惯第一时间去看注释。不去看注释，就会在 Bug 里面绕好几天。绕好几天的收获就是 RTFSC，又提升了对这个工具类的设计的理解。

如果是你，你会怎么设计这个迭代器呢？

---

这两个弱鸡问题居然查了这么久，真是有点羞愧。

不过，凡事要看好的一面，今年校招面试又有新问题了呢！
