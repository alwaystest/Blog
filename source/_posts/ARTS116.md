---
title: ARTS116
tags: ARTS
date: 2024-01-17 16:49:44
---

Clash Meta For Android 复活，切换到最新内核 🎉

<!--more-->

# Algorithm

none

# Review

[OpenWrt 文件名格式，安装固件前了解 - 喵二の小博客](https://www.miaoer.xyz/posts/blog/format-openwrt#分区布局)

常用设备上都切换到 Meta 内核了。发现还是有很多好用的功能的。

SubConvert 都可以直接不用了。

但是在路由器上启用 Meta 内核似乎不太顺利。

发现我使用的 H3C NX 30 Pro 最近似乎会比较频繁的自动重启，猜测是 OpenClash 导致的问题，周末花了一点时间想迁移到 Meta 内核，却发现存储空间不足，导致各种执行报错。

什么时候有时间再搞一下传说中的大分区版本。

About OpenWrt on MTK

> NMBM是MediaTek NAND Memory bad Block Management，MediaTek的闪存坏块管理。
> 

[mediatek/filogic预编译镜像中带nmbm字样的应该如何安装？ · immortalwrt/immortalwrt · Discussion #1084 (github.com)](https://github.com/immortalwrt/immortalwrt/discussions/1084)

[能否适配一下H3C NX30 Pro 112M大分区？ · Issue #112 · hanwckf/immortalwrt-mt798x (github.com)](https://github.com/hanwckf/immortalwrt-mt798x/issues/112)

花点时间看了一下背景，这款路由器看起来开启大分区的希望不大了。

存储空间不足，放不下  Meta 内核，还插不了 USB，那没办法了，只能靠剥离无关功能，尽量精简路由器的存储占用了。稳定优先。

[使用JUnit5实现测试并行化](https://javakk.com/2944.html)

观察到 CI 运行单元测试是串行执行的，测试用例比较多的场景下，每次触发 CI 都需要等待非常久的时间，这一点都不敏捷。

看看有没有必要上 JUnit 5 吧

https://github.com/robolectric/robolectric/issues/3023

开发需求写单测的时候想直接的 IDE 里看看覆盖率情况，却发现加了 Robolectric 之后 Run Unit Test With Coverage 会报错，试了一下切换 Runner 到 Jacoco，添加参数，都不奏效。

最终找到这个报告，看起来确实是一个 Bug，可以先临时限制 include packages 来解决，后续等待 Google 官方修复，最新的进展在 Issue Tracker 上，感兴趣的同学可以顶起来。

# Tips

M 系列的芯片太强了，某个软件出 Bug 跑满 CPU 的时候风扇经常都没有动静的，装个 RunCat 随时观察 CPU 是不是快被干冒烟了。

切换到 Meta 之后大部分场景都使用了自动切换的策略，导致 Edge 上登录的微软帐号在多个地区漫游，这周发现 Copilot 一直触发 reconnect，或者提示帐号已注销。重新登录 Edge 上的微软帐号并修改密码之后 Copilot 才恢复正常。看起来自动切换的策略虽然简单，但也会带来一些风险，慎用。

# Share

none
