---
title: ARTS104
tags: ARTS
date: 2023-09-01 15:22:10
---

虽然不怎么摇滚，但是乐夏 3 里面还是发现了一些宝藏歌曲。

也第一次理解了二手玫瑰的热度，还得是靠现场那种没来由的跟着蹦的感觉。

<!--more-->

# Algorithm

ignore

# Review

ignore

# Tips

跟群晖的客服有来有往的 Debug 了一周。最终还是没办法搞定在 IPv4 环境下通过 Proxy + DDNS 连接到 IPv6 的群晖母鸡。

结论是 Synology Client 进行 DNS 解析的时候没有使用代理，本地获取到的地址只有 IPv4 地址，没有 IPv6 地址。

突然想起来之前似乎是直接改了 Hosts 文件，让 Synology Client 使用 DDNS 域名解析后的 IPv6 地址进行连接的，只要解析这一步搞定，后续的代理加个 Port 规则就好了。

疑似 IDEA 的 Rainbow Brackets 有 Bug，常常导致 CPU 占用率奇高，disable 了先观察一段时间

还会有问题

这次禁用了 AceJump 再试试

依然有这样的情况，关闭 layout 编辑页面就好一点了，最后也没能定位到到底是什么原因。。。

# Share

ignore
