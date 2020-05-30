---
title: Why functional programming
date: 2020-05-30 23:29:23
tags: Programming
---
# 我为什么喜欢函数式编程风格

# 函数式编程？

当你上网查询函数式编程的时候，大概能查到以下几个关键词：

数据不可变

函数是一等公民（和其他数据类型一样，可以当作参数传递，可以赋值给变量）

没有副作用

……

大家列举了成吨的优点，仿佛不用函数式编程你就赶不上后浪了。

函数式编程希望通过制定一些原则，避免写出难以维护的代码。

# 我为什么喜欢函数式编程风格

函数式的编程风格提高了代码的可读性，可维护性，可测试性。

## 纯函数的好处

> 纯函数是这样一种函数，即相同的输入，永远会得到相同的输出，而且没有任何可观察的副作用。
—— 函数式编程指北

### 可读性和可维护性

在写代码的时候，经常需要维护一个当前状态。有状态的代码是比较难维护的。

```java
class Scratch {
    private Integer sum;

    void plus() {
        sum++;
    }

    void init() {
        sum = 1;
    }

    void deInit() {
        sum = null;
    }

    void manipulate() {
        // impl
    }
}
```

当我试图分析 `manipulate` 函数的时候，我对 `sum` 的状态其实并不是很清楚，为此我还需要看 `init` 函数，以及别的函数，确认 sum 可能的状态，才能实现 `manipulate` 函数，而且此时无法确认调用 `manipulate` 函数时 sum 是否可能为空。哪怕目前的用例可以保证调用这个方法之前一定执行了 `init` 函数，我也担心后面有别的需求偷偷摸摸的调用了 `deinit` 函数，所以我需要一开始就处理异常，判断一下 `sum` 是否可能为空。

如果这个类经过了长年的维护，增加了 N 个方法，长度达到了一千行，那这个类里面任何一个使用 `sum`  的函数都需要小心谨慎的在使用之前判断一下 `sum` 的状态，如果还涉及到了多线程操作，我的天呐，这个代码你来维护吧，我写不下去了。

当我开始强调纯函数的概念时。我的代码会变成这样：

```java
class Scratch {
    
    Integer plusOne(Integer num) {
        return num + 1;
    }

    void manipulate(Integer num) {
        // impl
    }
}
```

去掉了维护的状态，一个函数只干一件事情，对于一个输入，始终返回一个稳定的结果。当分析一个函数时，思路可以缩小到这个函数本身，不用再关注其他函数的实现是否会对这个函数产生影响。

当我想避免在 `manipulate` 函数中处理 `num` 为空的状态时，我只需要增加一个 Annotation 声明，并且检查一下这个函数的调用处就好了，大大地缩小的代码改动范围。代码可维护性得到了提升。

```java
void manipulate(@NotNull Integer num) {
        // impl
}
```

### 可测试性

```kotlin
data class Data(val num: Int)

class Controller {
    private lateinit var data: Data

    fun load() {
        TODO("Load data from Internet")
    }

    fun getTip(): String {
        return "data is $data"
    }
}
```

如果要给 `getTip` 加一个单元测试，就需要想办法给 `data` 赋值。

如果 `load` 的实现非常简单，那 mock 一个网络请求问题也不是很大。但总是觉得很烦，我明明就只想测一下 `getTip` 这个函数，为什么还需要关心 `Controller` 的数据是怎么加载出来的？

当 `load` 的实现需要把 N 个 API 返回的数据组合的时候，有写 mock 代码的功夫，我单元测试都写完三轮了。

怎么办呢？简单啊，把 `data` 设成 public 不就完了么？

```kotlin
class Controller {
    lateinit var data: Data
}
```

有效，但是太暴力了。一个本来应该内部维护的状态，被暴露到外面，现在谁都可以修改 `Controller.data` 了，太危险。

那我们再加一个 `@VisibleForTests` 吧，这样 IDE 就会警告了。除了测试代码，外部的别的代码都不允许直接访问 `data` 字段。但是你知道的，对于程序员来说，Warning 根本不算啥，我们只关注 Error。

还有什么好办法么？去掉 `getTip` 函数依赖的状态

```kotlin
data class Data(val num: Int)

class Controller {
    lateinit var data: Data
        private set

    fun load() {
        TODO("Load data from Internet")
    }

    fun getTip(data: Data): String {
        return "data is $data"
    }
}
```

现在你可以在外部获取 `data` 字段，但是不允许从外部修改。调用 `getTip` 函数的时候，直接这么使 `controller.getTip(controller.data)`

咦，这样我只需要写一遍测试函数，验证 `controller.data` 和 `load` 函数加载的数据是一致的。测试其他函数的时候，我都不需要再 mock 网络请求了，直接 new 对象出来传参即可。是不是写单元测试的效率大大地提升了？

# 函数式编程风格带来的劣势

可能就是鱼与熊掌不可兼得吧。本来一把梭的代码，为了可读性，可维护性，为了写单元测试，可以直接取值的时候偏偏要用参数传值，可以直接修改维护的状态的时候，却偏偏要 copy 一份数据。

非要挑刺的话，感觉写起来会变得复杂，修改的时候 copy 一下数据，徒增 GC 压力。

# 其他

函数式编程不是一个小话题，其它的比如并行安全，递归优化，函数可以当作变量来传递等特性这里都没有提到，想一个合适的例子可太难了。

可读性和可维护性那部分的代码刚开始打算用 Kotlin 来写的，发现 Kotlin 已经吸收了太多函数式编程的思想，想用 Kotlin 写出明显不容易维护的代码还是有点难度的，所以回退到了 Java 的语法。

# Refs

[函数式编程指北 · GitBook (Legacy)](https://legacy.gitbook.com/book/llh911001/mostly-adequate-guide-chinese)

[函数式编程 | | 酷 壳 - CoolShell](https://coolshell.cn/articles/10822.html)