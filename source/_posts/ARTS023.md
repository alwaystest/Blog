---
title: ARTS023
date: 2021-02-22 11:53:26
tags: ARTS
---
放假啦！
<!--more-->
# Algorithm

🕊️

# Review

🕊️

# Tips

追查一个 Bug 的过程中，发现一个 Coroutine 中优化性能的小操作

`MainCoroutineDispatcher.immiediate` 可以在当前线程已经是 UI 线程的情况下，避免一次额外线程切换的开销。

使用 Wireshark 抓包检查 TCP 并发连接数，可以通过以下方法过滤指定域名下的链接

```kotlin
tls.tls.handshake.extensions_server_name contains "domain" or http.host contains "domain"
```

但是这样的方式过滤后，分析 Conversations，无法看到 TCP 连接的 Duration。怀疑是 Filter 应用到了上层，把底层协议的一部分包过滤掉了，导致无法分析。

在 Conversations 中把结果 copy 出来，导入 Excel 中，Split 一下，复制 IP 地址，去重，然后使用下面的 Filter 过滤即可看到 TCP 连接的 Duration 和并发情况。

```kotlin
ip.addr in {ip1 ip2}
```

---

看 UI 自动化测试的脚本，发现有一个调用在 Macaca API 中找不到，发现在 Readme 里面提到了

> Macaca WD Client is inspired by admc/wd, according to W3C WebDriver Spec.

这个 API 还有一套规范，学到了。

# Share

🕊️
