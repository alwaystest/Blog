---
title: ARTS001
date: 2020-05-30 23:40:52
tags: ARTS
---

# A.R.T.S 001
<!--more-->

# 什么是 A.R.T.S

算了附图好麻烦，关键字 `左耳朵耗子` 。

# Algorithm

随机了一道。似乎在《剑指 Offer》中看到过，先热个身吧。

[Find Minimum in Rotated Sorted Array - LeetCode](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

在一个可能被旋转过的有序数组中找到最小值。

细节是魔鬼，TestCase 中可能出现完全有序的数组，倒是也说得过去，仿佛做了一道阅读理解。

# Review

[Adam7 algorithm](https://en.wikipedia.org/wiki/Adam7_algorithm)

[Interlaced data order](http://www.libpng.org/pub/png/spec/1.2/PNG-DataRep.html#DR.Interlaced-data-order)

终于知道 之前网页上加载图片的时候是怎么实现图片由模糊到清晰的过程了。之前一直以为是提供了一张 thumbnail 和一张清晰的大图，等清晰的大图完全加载完成之后再替换。看了这个算法，恍然大悟。

用一个算法，就可以避免处理和存储两份清晰度不同的图片，不知能节省多少运营开销，果然知识就是财富。

# Tips

本周有个需求和相机拍摄的图片展示方向有关，顺带看了一眼 EXIF 的相关知识。半懂不懂的看了一下 [PNG 的文件格式标准](http://www.libpng.org/pub/png/spec/1.2/PNG-Contents.html)。

惊悉 PNG 格式的图片中可能不包含 EXIF 信息，除非遵循 `PNG-1.2 Extensions V1.5.0 Specification` 。

主要关注了一下为什么需要往 EXIF 中写入 Orientation 信息。

我个人的理解是本身 PNG 中存储图像数据的时候就是遵循从左到右，从上到下的顺序，如果所有的拍摄设备在生成图片的时候，都可以根据设备自身的旋转状态把图片转到正确的角度，然后再写入到文件中，就不需要 EXIF 中存什么旋转信息了。

非要找理由的话，可能某些设备性能不足，处理不了图片旋转再存储的计算量，或者干脆就没有配置重力感应；或者要追求快速的写入速度，比如连拍；或者需要后期变换图片展示角度。增加一个 Orientation 属性可以使旋转图像这件事情的成本很低。

# Share

[Why functional programming](/2020/05/30/why-functional-programming/)
