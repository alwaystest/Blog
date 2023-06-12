---
title: ARTS093
tags: ARTS
date: 2023-05-06 22:44:19
---

玩图形的都长了好几倍于我的脑细胞。

<!--more-->

# Algorithm

none

# Review

[深入浅出理解Tombstone|极客笔记 (deepinout.com)](https://deepinout.com/android-system-analysis/android-debug-related/easy-to-understand-tombstone.html#:~:text=Tombstone%E7%AE%80%E4%BB%8B%20%E5%BD%93%E4%B8%80%E4%B8%AAnative%E7%A8%8B%E5%BA%8F%E5%BC%80%E5%A7%8B%E6%89%A7%E8%A1%8C%E6%97%B6%EF%BC%8C%E7%B3%BB%E7%BB%9F%E4%BC%9A%E6%B3%A8%E5%86%8C%E4%B8%80%E4%BA%9B%E8%BF%9E%E6%8E%A5%E5%88%B0debuggerd%E7%9A%84signal%20handlers%E3%80%82,%E9%92%88%E5%AF%B9%E8%BF%9B%E7%A8%8B%E5%87%BA%E7%8E%B0%E7%9A%84%E4%B8%8D%E5%90%8C%E7%9A%84%E5%BC%82%E5%B8%B8%E7%8A%B6%E6%80%81%EF%BC%8CLinux%20kernel%E4%BC%9A%E5%8F%91%E9%80%81%E7%9B%B8%E5%BA%94%E7%9A%84signal%E7%BB%99%E5%BC%82%E5%B8%B8%E8%BF%9B%E7%A8%8B%EF%BC%8Cdebuggerd%E6%8D%95%E8%8E%B7%E8%BF%99%E4%BA%9Bsignal%EF%BC%8C%E5%81%9A%E5%87%BA%E7%9B%B8%E5%BA%94%E5%A4%84%E7%90%86%E7%9A%84%E5%90%8C%E6%97%B6%EF%BC%88%E4%B8%80%E8%88%AC%E6%9D%A5%E8%AF%B4%E6%98%AF%E9%80%80%E5%87%BA%E5%BC%82%E5%B8%B8%E8%BF%9B%E7%A8%8B%EF%BC%89%EF%BC%8C%E5%9C%A8%2Fdata%2Ftombstones%E7%9B%AE%E5%BD%95%E4%B8%8B%E7%94%9F%E6%88%90%E4%B8%80%E4%B8%AAtombstone%E3%80%82%20Tombstone%E6%96%87%E4%BB%B6%E4%BB%A5tombstone_XX%E5%BD%A2%E5%BC%8F%E5%91%BD%E5%90%8D%EF%BC%8C%E8%AF%A5%E6%96%87%E4%BB%B6%E4%B8%AA%E6%95%B0%E4%B8%8A%E9%99%90%E5%8F%AF%E4%BB%A5%E8%BF%9B%E8%A1%8C%E8%AE%BE%E7%BD%AE%EF%BC%8C%E5%BD%93%E8%B6%85%E8%BF%87%E4%B8%8A%E9%99%90%E6%97%B6%E5%88%99%E6%AF%8F%E6%AC%A1%E8%A6%86%E7%9B%96%E6%97%B6%E9%97%B4%E6%9C%80%E8%80%81%E7%9A%84%E6%96%87%E4%BB%B6%E3%80%82%20Tombstone%E8%AE%B0%E5%BD%95%E4%BA%86%E5%B4%A9%E6%BA%83%E7%9A%84%E8%BF%9B%E7%A8%8B%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BF%A1%E6%81%AF%EF%BC%8C%E5%A0%86%E6%A0%88%E8%B0%83%E7%94%A8%E4%BF%A1%E6%81%AF%EF%BC%8C%E5%86%85%E5%AD%98%E4%BF%A1%E6%81%AF%E7%AD%89%E7%AD%89%E3%80%82)

接触到了 iqiyi 开源的 xCrash，了解到了 Native 的异常处理机制，Mark 一下，估计很快就会忘记。

# Tips

[Point in polygon](https://en.wikipedia.org/wiki/Point_in_polygon)

Android 的 `ShapeableImageView` 是个挺好用的东西，粗略看了一下其中的代码实现。有一个 ShapeMask 的概念，利用 Path 和 Xfermode 把 View 要绘制的部分用 Mask 裁切出来。这个过程发生在 Draw 的步骤，还挺涨见识。

似乎很久之前看过 Path 绘制相关的知识，现在几乎都忘记了。用 new bing 搜索了一下 `signed edge crossing method` 发现了上面这篇文章，确实是一个很通用的计算方式，不限于 Android 平台。

# Share

none
