---
title: ARTS006
date: 2020-07-12 20:41:01
tags: ARTS
---
# Algorithm
<!--more-->

[Image Smoother - LeetCode](https://leetcode.com/problems/image-smoother/)

Random Easy

UT + Impl 搞定

# Review

[](https://developer.android.com/training/dependency-injection)

前段时间看到 Google 出了新的 DI 框架 Hilt，虽然还是 Beta 版，但是还是有兴趣了解一下是不是能比 Dagger 学习曲线更简单一点的。

看了 Hilt 相关的部分，Dagger 的没再看，应该有好多新增的 API 了。

目前看起来 Hilt 想利用 Dagger2 的优势，所以是基于 Dagger2 来搞的，目测学习曲线依旧陡峭。

还看到 Hilt 使用了 Java 8  的 Feature，需要将 `compileOptions.targetCompatibility` 设为 Java 8。

不过好消息是 AGP 4.0 增加了更多的 Java 8 API Desugring，之后可以试试会不会有坑。

看起来目前的时间节点上还有许多限制。

> Hilt only supports activities that extend ComponentActivity, such as AppCompatActivity.
Hilt only supports fragments that extend androidx.Fragment.
Hilt does not support retained fragments.

先过了一遍文档，看看大概是个什么路数，之后踩坑可能还是需要在个人开发的 App 里面真正的用起来才行。

大体感知是对 Dagger 的一个增强，提供了更多和 Android Lifecycle，Components 绑定更强的用法。

# Tips

上周赶着填充了一部分新牛的 Sample 项目，发现我一开始写 ViewModel 初始化的方法写的有点繁琐。

```kotlin
ViewModelProviders.of(
            this,
            viewModelFactory {
                PaymentDetailViewModel(
                    params1,
                    params2
                )
            }
        ).get(PaymentDetailViewModel::class.java)
```

当时主要是考虑到为了方便 ViewModel 的单测，采取了依赖注入的方式，把 ViewModel 的依赖对象暴露到了构造函数中，如果使用 `jvmOverloads` 的方式加上参数默认值，可以简化代码为：

```kotlin
val viewModel: PaymentDetailViewModel by viewModels()
```

但是这样的实现会有性能问题（底层实现是反射调用无参构造函数），而且有些参数不适合给默认值。

所以目前感觉最好的写法可能还是

```kotlin
val viewModel: PaymentDetailViewModel by viewModels {
    viewModelFactory {
        PaymentDetailViewModel(
            params1,
            params2
        )
    }
}
```

感觉还是有些繁琐，起码比之前的好一些了。

Architecture Components 的好多玩法还是需要多熟悉一下的，能学到不少简化代码的思路。

# Share

[Notification Channels 和广告推送](/2020/07/12/NotificationChannelsAndAds)
