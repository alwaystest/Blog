---
title: ARTS082
tags: ARTS
date: 2023-01-13 10:20:08
---

AppCode 被放弃了。
<!--more-->

# Algorithm

# Review

# Tips

# Share

Padavan 二级路由启用 IPv6 似乎简单了。

主路由获取的 IPv6 子网前缀是 /60 的情况下，理论上只有下一级设备能 SLAAC 分配到 IPv6 地址。二级路由的子设备一般无法获取。

之前需要搞 natp66，或者增加开机规则，搞桥接规则。

最近内网的 IPv6 访问出了一些问题，IPv6 不通了。也没找到是什么原因。

干脆升级后重置一波系统，看看能不能恢复，死马当作活马医了。不行就重新买一个支持 Openwrt 的路由从头搞起。

没想到重设之后，简单的在 Wan 设置中开启 IPv6.

连接类型 Native DHCPv6

开启硬件加速

获取 IPv6 外网地址 Stateless

自动获取 IPv6 DNS

通过 DHCPv6 获取内网 IPv6 地址

启用 Lan 路由器通告

启用 Lan DHCPv6 服务器 Stateless

很奇怪，之前这么一套配置是搞不定的。

最后，如果有从外网访问的需求，还需要配置对应的防火墙规则。

比如要从外网访问群晖，需要配置如下规则

```bash
ip6tables -A FORWARD -p tcp --dport 5000 -j ACCEPT
ip6tables -A FORWARD -p tcp --dport 5001 -j ACCEPT
```

