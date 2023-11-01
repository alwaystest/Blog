---
title: ARTS105
tags: ARTS
date: 2023-09-26 15:24:17
---

肝需求肝懵了。

<!--more-->

# Algorithm

ignore

# Review

[RecyclerView 性能优化 | 是什么在破坏缓存机制?_51CTO博客_recyclerview的缓存机制](https://blog.51cto.com/u_15375308/4009498)

改完某个 Bug 发现 RecyclerView 的回收不能正常工作了。Debug 了有一小时吧。。。

看了半天源码和回收没什么太大的关系，主要是使用了 ViewPager2 的场景下，在 `onPageSelected` 回调中又调用了 `notifyDataSetChanged` 方法，
看 RecyclerView 的日志提示，这么搞确实不太合理。

研究了好几天以后，发现了一个解决方式：`post { // notify }`

没错就是这么简单，肝业务肝的脑子都木了，居然耗费了那么长时间 Debug。

# Tips

ignore

# Share

ignore
