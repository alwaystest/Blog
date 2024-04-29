---
title: ARTS125
tags: ARTS
date: 2024-04-29 17:07:05
---

Notion 好贵，开始折腾新玩具 Obsidian。

<!-- more -->

# Algorithm

none

# Review

[为 Android 换上任意喜欢的字体，你可以试试这个 Magisk 模块](https://sspai.com/post/58049)
霞鹜文楷的 Release Note 提到了一个替换字体的 Magisk 模块，对于我这种懒人来说，Magisk 一般用来获取 Root权限，搞一些 Hack 的东西，并不会花太多时间去搞美化，个人感觉系统美化的终点站还是官方原装，我现在连默认壁纸都不改的。
刚好之前想搞明白系统下各个字体是怎么进行匹配的，在字体缺失的情况下会怎么降级这些逻辑也需要了解一下。

# Tips

连接公司内网的 VPN 有黑科技。
通过 `ifconfig` 发现新建了一个 `tun` 设备
通过 `netstat -rn` 发现修改了路由表，把一些网段增加到了配置中
通过 `scutils --dns` 发现偷摸新增了一个 dns 配置，这条配置在 Network Detail 的 DNS tab 下是看不到的。
通过 `netstat -ni` 发现新增的 `tun` 设备对应了一个 `Link<#xx>` 的 Network。

排查了好久，发现连接到另一个路由器下或者连接到手机开的热点，可以访问公司内网，但是连接到自己的 openwrt 路由器上就不行。
再详细排查，发现是因为 dns 解析的结果为空导致的。
于是开启 dnsmasq 的日志，进行排查，发现日志提示：

```txt
possible DNS-rebind attack detected:
```

原来是 DHCP 的配置中有一个重绑定保护的选项：`stop-dns-rebind` 导致 dnsmasq 拦截了响应是 10.x.x.x 网段的 dns 响应。

本来想着稳妥一点只给某些域名加个白名单的，发现 dnsmasq 的配置不支持正则匹配，索性直接关掉这个开关好了。

# Share

none
