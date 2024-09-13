---
title: ARTS134
tags: ARTS
date: 2024-09-13 10:42:35
---

没想到浓眉大眼的 Android 居然在安全性方面有超过 iOS 的地方。

<!-- more -->

# Algorithm

none

# Review

none

# Tips

被微博国际版的摇一摇广告搞的烦不胜烦，每次打开 App 真的就像捧着个地雷一样毫不夸张。
于是启用了 Surge 的 MITM 功能，过滤了某些广告请求。
MITM 有风险，建议启用域名白名单防止被攻击。

尝到甜头后想在 Android 上依样画葫芦，却发现没有现成的工具可以实现，一时兴起还想着要不要自己写一个 MITM 的功能。偶然间发现 SurgeBoard 已经进行过尝试，Android N 已经增加了系统限制，禁用了自签名证书的口子。
Root 当然可以绕开限制，都 Root 了，MITM 的方式就有点鸡肋了。

# Share

none
