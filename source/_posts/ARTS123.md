---
title: ARTS123
tags: ARTS
date: 2024-03-19 19:08:37
---

💡一个启发 by 也谈钱：

常常会有一个误区，如果保险赚的是保费、理赔的差价，那保险公司会不会在理赔的时候刁难人呢？

这就有点像穷人思维和富人思维的区别。穷人生怕自己的利益受损，处处抠抠搜搜。而富人会想着把蛋糕做大，大家一起来分享利益。

放到保险行业来说，如果理赔的时候抠抠搜搜，相比于大大方方理赔的保险公司，愿意投保的客户就少了，浮存金减少的时候，保险公司的投资收益就会降低，进入负向循环。

而应赔尽赔的保险公司，客户也乐意选择，浮存金就会增加，相对的，投资收益就会提高。只需要做好精算，确保保险的售价不会导致自己亏本 ，那少赔付的几分几毛就算不上什么了。

<!--more-->

# Algorithm

none

# Review

none

# Tips

[zipalign  |  Android Studio  |  Android Developers](https://developer.android.com/tools/zipalign?hl=zh-cn)

zipalign 在 Android SDK build-tools 文件夹下，可以用 -c 参数检测 APK 是否进行过 zipalign 操作

[ZIP文件结构解析 - 枫のBlog](https://goodapple.top/archives/700)

一时兴起问了一下 AI zipalign 是怎么实现的，原来是依靠修改 zip 文件的 Local File Header 中的 extra 字段。

搜了一下实现，发现 GitHub 有一个开源的 Java 版本实现，读一下代码啥都明白啦！

[](https://github.com/iyxan23/zipalign-java/blob/main/src/main/java/com/iyxan23/zipalignjava/ZipAlign.java)

[tools/zipalign/README.txt - platform/build - Git at Google](https://android.googlesource.com/platform/build/+/master/tools/zipalign/README.txt)

再记录一下官方的 readme，zipalign 的目的是允许 mmap 直接把文件映射到内存中。

mmap 真是无处不在，对 zip 文件进行 4 Bytes 对齐，让我想起了很早之前买了 SSD 之后要对磁盘进行 4K 对齐处理，也是为了有更好的性能。

这一切都和底层以 2 作为基数有关。

[](https://github.com/iqiyi/xHook/blob/master/docs/overview/android_plt_hook_overview.zh-CN.md)

顺便搂了一眼 xhook 究竟做了什么事情。啊这不就是非常危险的操作吗？用指针直接修改内存当中的数据，简直就是刀尖上跳舞的艺术。

看另一端的代码处理，发现 iOS 有 Category 和 Swizzle 的概念，通过 [devv.ai](http://devv.ai) 简单了解了一下，又搜索了一下对应的技术实现解析文章，长见识了。

[Category原理解析 - 张远文的博客 | Anyeler Blog](https://anyeler.top/2018/07/29/Category原理解析/)

# Share

none
