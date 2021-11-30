---
title: ARTS049
date: 2021-11-30 14:35:42
tags: ARTS
---

事已至此，先发出来吧。

<!--more-->

# Algorithm

ignore

# Review

[Learning Flutter's new Navigation and Routing system](https://medium.com/flutter/learning-flutters-new-navigation-and-routing-system-7c9068155ade)

了解了一下 Flutter Router 的实现

[Android | 带你理解 NativeAllocationRegistry 的原理与设计思想](https://zhuanlan.zhihu.com/p/268822452)

看内存相关的问题，KOOM 上报的内存情况中，有一个 NativeAllocationRegistry 没有见过，于是了解了一下相关的实现。果然还是绕不过强软弱虚四种引用类型。

# Tips

https://github.com/permissions-dispatcher/PermissionsDispatcher/issues/533

MIUI 上遇到一个奇怪的问题，在我们还没有申请 ReadPhoneState 权限的时候，手机就已经提醒在尝试读取信息了。

看起来是 MIUI 魔改出来的一个问题，只要用 AppOps 读取对应的权限就会被系统认为是读取敏感信息了。

不要尝试适配国内魔改的 OS。

---

发现我的 Blog 没有被搜索引擎索引到。于是简单配置了一下。

1. 生成 sitemap
2. 到 Google Search Console 提交自己的网站地址。
    1. 搜索了一些教程，看之前需要往自己的网站下放一个文件。自己试了一下，发现可以通过在 DNS 响应中添加 TXT 来验证自己持有域名。我配置之后自己已经可以 Dig 到相关的信息，但是 Google 那边一直无法通过。于是换了 URL Prefix 的方式来验证。由于 Hexo Blog 接入了 Google Analytics，所以这一步直接就通过验证了。
3. 通过验证后看起来 Google 就会直接去分析了，具体数据需要等等才能看到。看之前的教程，还需要配置 sitemap 啥的，我先不配，看看是不是约定大于配置，可以一键搞定。
    1. 似乎是不行

# Share

### 一种单测代码分段的写法

之前看过腾讯的 《单元测试xxx》，推荐的单测写法是这样的

```kotlin
@Test
fun targetMethod_condition_expection() {
    // some code to prepare

    // targetMethod()

    // verify results
}
```

经过这样划分代码段落，可读性确实提高了。

单测代码更多的需要考虑到 CornerCase。需要多个测试方法来验证，很容易出现模板代码冗余的情况。

那规则是死的，人是活的，可以灵活的调整一下。对于同时验证 condition 成立和不成立两种情况来说，可以放到一个方法中，感觉可读性也不是太差。

```kotlin
@Test
fun targetMethod_condition_expection() {
    // set condition false

    // targetMethod()

    // verify results

    // set condition true
 
    // targetMethod

    // verify results
}
```

感觉这篇文章好像在描述茴香豆的茴字的八种写法。
