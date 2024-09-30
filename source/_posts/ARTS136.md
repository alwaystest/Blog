---
title: ARTS136
tags: ARTS
date: 2024-09-30 18:08:54
---

大树底下好乘凉。

<!-- more -->

# Algorithm

none

# Review

[Android 高版本 androidx 引发 D8 "String.length()" null 问题解决](https://mp.weixin.qq.com/s/1a-t94dYcRGBOt8F5ZuFsg)
上周刚遇到过的一个问题，通过降级版本临时解决了。

# Tips

翻代码的时候偶然发现一个 ProcessLifecycleOwner 的工具类，可以比较方便的监测 App 前后台状态。
同理，还有一个 ViewTreeLifecycleOwner 可以通过 View 对象来获取 LifecycleScope 从而进行协程生命周期绑定。
Kotlin 这颗大树底下真的好乘凉，学无止境。

ipv6spi 
> SPI 是一种状态包检测（Stateful Packet Inspection）技术，主要用于防火墙和网络安全设备中。IPv6 SPI 是指在 IPv6 网络中使用 SPI 技术来检测和过滤数据包。
> 
> 在 IPv6 网络中，SPI 可以帮助防止各种类型的攻击，例如拒绝服务（DoS）攻击、分布式拒绝服务（DDoS）攻击、端口扫描等。SPI 可以检测和阻止这些攻击，保护网络和设备的安全。


新装了宽带，从外网访问不到 NAS 了，可以定位到是防火墙的问题，但光猫不像 OpenWRT 一样可以自由操作 ip6tables，找不到去哪里配置端口规则。
最后发现关闭 ipv6spi 就可以搞定，虽然多了一丝丝不安全因素，但是 ipv6 诶，目前我的认知是茫茫多的地址池，扫描是很难的了，除非对方直接搞到域名。噢，BT 下载也可能泄漏地址信息，有空再深入研究一下。

# Share

none
