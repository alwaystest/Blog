---
title: ARTS121
tags: ARTS
date: 2024-03-06 18:52:41
---

Flag 要立起来。今年跑步里程要到 300KM。

<!--more-->

# Algorithm

none

# Review

[Clash Verge系列使用最佳实践 | Lainbo's Blog](https://lainbo.com/article/clash-config)

了解了一下 dat 文件和 mmdb 文件格式的区别，以及为什么要自定义 geodata。

# Tips

SynologyDrive 在 Mac IPv4 环境无法通过代理连接 IPv6 only 的 NAS，之前是通过修改 hosts 文件，加上配置端口规则来进行连接的。但是最近发现只要 NAS 的网络前缀一变，就需要修改 hosts 文件进行匹配。

刚好 Infuse 也没有办法在 IPv4 环境连接 IPv6 only 的 webdav 服务器。又研究了一下 TUN 模式。

这下大概是搞明白了，通过虚拟一个网卡 + DNS 劫持的方式，让流量通向虚拟网卡，就可以不用修改 hosts 文件，直接通过域名规则进行匹配了。

# Share

none
