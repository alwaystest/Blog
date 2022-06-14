---
title: ARTS068
date: 2022-06-08 18:34:27
tags: ARTS
---

Glide 源码常看常新。

<!--more-->

# Algorithm

None

# Review

[Glide4.8源码拆解（四）Bitmap解析之下采样浅析](https://hitendev.github.io/2019/01/13/Glide4-8%E6%BA%90%E7%A0%81%E6%8B%86%E8%A7%A3%EF%BC%88%E5%9B%9B%EF%BC%89Bitmap%E8%A7%A3%E6%9E%90%E4%B9%8B%E4%B8%8B%E9%87%87%E6%A0%B7%E6%B5%85%E6%9E%90/)

Glide 加载图片放大的逻辑分析，既然别人写了，就不用自己再分析一遍了。

[基于CNAME解析实践的域名优雅方案](https://wiki.eryajf.net/pages/027a17/#_1-%E5%88%86%E6%9E%90)

发现一个运维同学的 Blog，大致翻了一遍，有一些收获。挺棒的。

# Tips

`android.graphics.BitmapFactory.Options#inScaled`

Glide 除了设置 transform 之外，还会利用这个属性配合 `inTargetDensity`  `inDensity` 对图片进行缩放，具体逻辑在 `com.bumptech.glide.load.resource.bitmap.Downsampler` 中。

又学习到了新东西。

Fun fact:

sans-serif 是法语来着

sans 无

serif 衬线

# Share

None
[](https://www.notion.so/aafc43a4ffb14806a7f98d0284a265d7)
