---
title: ARTS053
date: 2022-01-12 15:23:02
tags: ARTS
---

今年不用买鞭炮了，我稍微动一动就是一串二百响。

<!-- more -->

# Algorithm

鸽

# Review

[入门DHT协议理论篇](https://l1905.github.io/p2p/dht/2021/04/23/dht01/)

惊了，二分查找还能这么用。

之前看回形针讲过 BT 下载的原理，对其中去中心化的邻居查找算法很好奇，但是一直没有时间来研究。直到最近看到一个去中心化的服务发布 Repo https://github.com/PeerXu/meepo ，想了解一下实现的原理，便于判断是否可以安全使用。顺藤摸瓜找到了这篇文章。

# Tips

遇到了一个线上崩溃

QGLCLoadProgramBinary

只能查到一篇文章说是 WebView 升级到 Arm64 架构之后需要清理 GPU 缓存。

[一个关于Android支持64位CPU架构升级的"锅"](https://www.jianshu.com/p/841c18c6e18d)

绝了，我对浏览器和 Native 这边都不太熟悉，如果让我来看这个问题的话，估计是搞不定了。

在一款华为的平板上还发现了删除文件夹后 WebView 不可用的情况。

[有效解决WebView多进程崩溃 - 掘金](https://juejin.cn/post/6942298361454133261)

[有效解决WebView多进程崩溃（续） - 掘金](https://juejin.cn/post/6950091477192015902/)

华为的部分机型会把 app_webview 改成 app_hws_webview，所以在网上搜到的另一个解决方案是直接删除带 webview 的所有文件夹

[WebView common Crash analysis and Solutions](https://programmer.group/webview-common-crash-analysis-and-solutions.html)

这样一来，Chrome 创建 lock 文件因为没有对应文件夹，会失败。

最终又补了一段记录下所有文件夹路径，删除之后再重新创建的逻辑。

记得删除之后要再次同步 Cookie

# Share

没啥干货要输出的
