---
title: ARTS014
date: 2020-09-22 21:36:58
tags: ARTS
---
# A.R.T.S 014
<!--more-->

# Algorithm

[Can Place Flowers - LeetCode](https://leetcode.com/problems/can-place-flowers/)

Easy

第一思路就是暴力过一遍。看了一下参数，感觉可能超时，那就提前判断一下呗。不难。

# Review

[Gesture Navigation: Going edge-to-edge (I)](https://medium.com/androiddevelopers/gesture-navigation-going-edge-to-edge-812f62e4e83e)

被测试同学推动调研了一个 Android Feature 的适配，感觉有些惭愧，这些东西应该让开发来发现和推动的，我们目前似乎有些缺乏主动性和对新技术的好奇心。

---

旁听了公司内移动端大佬们的前瞻性会议，发现自己的信息源有些闭塞，错过了很多有趣的新技术。这就是变油腻的感觉吗？不是说好了 35 岁之后才会认为所有新技术都是反人类的吗。该反思一下了。

[Android真响应式架构--MvRx](https://www.jianshu.com/p/53240a44ec49)

虽然是中文文章，但也值得看一看了。上次组内分享了一次根据 React 思想搞的 ReKotlin。感觉还是有些模糊，参考一下可以开源出来的方案是怎么搞的。

---

[How do hashCode() and identityHashCode() work at the back end?](https://stackoverflow.com/questions/4930781/how-do-hashcode-and-identityhashcode-work-at-the-back-end)

Object 的 `toString` 方法默认返回的值代表啥？

# Tips

Kotlin 中经常用到 `by lazy` 。一般场景是这样的。

```kotlin
val obj by lazy {
	objImpl()
}

obj.xxx()
```

有的时候可能这个 lazy 对象并没有被初始化，但是像在 onDestroy 的时候我们需要保证一下 `obj.clean()` 被调用了。如果直接使用 `obj.clean()` ，那在 obj 之前没有被使用过的情况下，此时会再触发一次 objImpl 的初始化，对性能造成浪费。

Lazy 接口提供了 `isInitialized` 方法，可以避免这种性能浪费。

```kotlin
val objLazy = lazy {
	objImpl()
}
val obj by objLazy

obj.xxx()
//...
fun onDestroy() {
	if (objLazy.isInitialized()) {
		obj.clean()
	}
}
```

可以这样来实现。

# Share

🕊️
