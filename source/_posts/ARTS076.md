---
title: ARTS076
tags: ARTS
date: 2022-12-03 18:51:53
---

click to see more!
<!--more-->

# Algorithm

# Review

# Tips

apng 文件头中有一个 acTL 控制块，标识了图片包含的帧数量以及循环次数。

尝试了 pngcheck，没有找到直接打印对应参数的方法。

既然我们知道了文件结构，那尝试直接用 16 进制来肉眼解读吧。

vim -b 以二进制模式打开 apng 文件，使用 xxd 把当前打开的内容转换成 16 进制的格式。可以直接用 `:%!xxd` 

vim 好用！

# Share
