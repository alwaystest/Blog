---
title: ARTS021
date: 2020-12-23 18:28:52
tags: ARTS
---

滑雪冲冲冲！
<!--more-->

# Algorithm

🕊️

# Review

🕊️

# Tips

之前提过用 Brew 安装旧版本的 Formula，搞的很复杂。

这周看到别的同学提供了另一个方法，特别简单。把对应版本的 formula.rb 文件下载到本地，cd 到对应目录， `brew install YOUR_FORMULA.rb` 即可。真是非常简单了。

---

发现 Detekt 的 TODO 检测有些问题。

```kotlin
/**
 * TODO:
 */
```

这样 KDoc 的注释是不检测的

```kotlin
/*
 * TODO:
 */

// TODO:
```

这样是可以检测到的

```kotlin
// TODO
```

没有冒号也是不会被检测出来的，不过这个可以配置。

给 Repo 提了 Issue，已经修复。

---

玩 Jenkins 的时候看到 Pipeline 中 fetch 操作指定的参数格式是 `+/refs/heads/master:refs/remotes/origin/master` 感觉挺有意思，有点像 `scp` 命令里面指定位置的格式。另外前面那个 `+` 看起来挺奇怪的，查了一下 manual ：

> The format of a <refspec> parameter is an optional plus +, followed by the source <src>,
followed by a colon :, followed by the destination ref <dst>. The colon can be omitted when
<dst> is empty. <src> is typically a ref, but it can also be a fully spelled hex object
name.

之前经常用 oh-my-zsh 提供的 `gfa` 来 fetch，看起来效率不高，之后还是直接 `gf` 吧。

# Share

### 单元测试用例的取舍

之前写单元测试的时候见到了几种写法

第一种的命名方式

testMethod_variousCondition_verifyResult

在实现单测的时候，只需要一个方法，验证所有条件下调用 testMethod 之后的结果是否符合预期。

这样写看出来的用例看起来像是重复了一遍代码逻辑，而且感觉用例的可读性没有那么高。

另一种命名方式

testMethod_condition1_expectResult

testMethod_condition2_expectResult

testMethod_condition3_expectResult

将不同的 condition 和 result 拆到多个测试用例里面。

这样写会有很多重复的 setup 代码，优势是逻辑清楚，单个用例的可读性更高一些。

我个人更倾向于第二种写法。那这种情况如何避免抄 setup 的代码呢？

我自己设想可以通过拆分 TestCaseClass，方法封装起来，复用。

两种写法各有优势，感觉是一个取舍的问题。

那对于同一个对象的不同字段，有必要拆分到多个用例里面验证吗？

看起来同样是个取舍的问题。当然也有可能是代码设计的不够合理，具体怎么写更好，还需要多多实践摸索。
