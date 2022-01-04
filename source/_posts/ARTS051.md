---
title: ARTS051
date: 2022-01-04 21:05:11
tags: ARTS
---

快过年喽

<!--more-->

# Algorithm

Skip

# Review

Skip

# Tips

看到 swift 有这样的语法

```swift
guard let someThingNotNull = someThingMayNull else {
    failure()
    return
}
```

刚好 Kotlin 这边写的代码也有类似的需求，于是试着找了一个类似的语法

```kotlin
val someThingNotNull = someThingMayNull ?: return

val someThingNotNull = someThingMayNull ?: run {
    failure()
    return
}
```

# Share

做新需求，升级了 KTX 版本，终于用上了 `by viewModels` 的语法。

创建 ViewModel 比较麻烦的一点是，如果 ViewModel 的构造方法不是无参的，或者不是 AndroidViewModel 的子类，就需要自己提供一个 `androidx.lifecycle.ViewModelProvider.Factory` 对象，来指定生成对应的 ViewModel 的方式。整个代码会非常繁琐。

需要对 ViewModel 写单元测试的时候，根据依赖反转的原则，往往会需要传入 Repo 对象。代码会这么写。

```kotlin
interface IRepo {
    fun getName(): String
}

class Repo : IRepo {
    override fun getName(): String {
        return "name"
    }
}

class ViewModel(val repo: IRepo) : androidx.lifecycle.ViewModel() {}

private val viewModel by viewModels<ViewModel>() {
    object : ViewModelProvider.Factory {
        override fun <T : androidx.lifecycle.ViewModel?> create(modelClass: Class<T>): T {
            return ViewModel(Repo()) as T
        }
    }
}
```

Factory 子类的实现基本上是一堆模板代码。

有没有什么办法能减少这样的模板代码呢？

可以使用 Kotlin 的默认构造参数，

```kotlin
interface IRepo {
    fun getName(): String
}

class Repo : IRepo {
    override fun getName(): String {
        return "name"
    }
}

class ViewModel(val repo: IRepo=Repo()) : androidx.lifecycle.ViewModel() {}

private val viewModel by viewModels<ViewModel>()
```

这样一来，代码就顺眼多了。

当 ViewModel 做网络请求时，往往会需要一些参数。在一些历史代码中，我们选择了在 ViewModel 的构造函数中对这些参数赋值。其实并没有必要，反而增加了代码复杂度。如果网络请求需要参数，那么把这些参数放到开始请求的函数调用里面就行了，避免污染 ViewModel 的构造函数。

```kotlin
// 不方便的写法
class ViewModel(val repo: IRepo = Repo(), val id: String) : androidx.lifecycle.ViewModel() {
    
    fun startLoad() {
        repo.getName(id)
    }
}

// 方便的写法
class ViewModel(val repo: IRepo = Repo()) : androidx.lifecycle.ViewModel() {

    fun startLoad(id: String) {
        repo.getName(id)
    }
}
```

好像又是一篇茴香豆。
