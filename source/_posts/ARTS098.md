---
title: ARTS098
tags: ARTS
date: 2023-06-22 17:27:55
---

最近有些忙

最近没什么输入，所以很难有一些输出

自省

<!--more-->

# Algorithm

none

# Review

none

# Tips

[Ash: /usr/libexec/sftp-server: not found when using scp](https://forum.openwrt.org/t/ash-usr-libexec-sftp-server-not-found-when-using-scp/125772/2)

更新路由器，openwrt 使用的还是旧版本的，Mac 上已经安装了 OpenSSH 9.0+ 的版本，会导致 scp 命令尝试使用 SFTP 服务器，使用 -O 参数可以回落到老版本的策略。

[OpenWRT 中 vlan 的使用 - Marquis](https://marquistj13.github.io/MyBlog/2019/04/vlan-intro/)

[请教大家一个网络技术上的问题，openwrt-OPENWRT专版-恩山无线论坛 -  Powered by Discuz!](https://www.right.com.cn/forum/forum.php?mod=redirect&goto=findpost&ptid=7752772&pid=15891589)

路由器刷机 OpenWRT 之后 IPv6 搞不定，初步怀疑是桥接的问题，顺便看了一下 VLAN 划分的意义，这里搞得有点难懂的。

> 这是网络上关于VLAN的基础术语的中文翻译，客观点来说这翻译的很糊涂，会把人搞昏的。对应的英文是：
> 
> 
> 关：off
> 
> 已标记：tagged
> 
> 未标记：untagged
> 
> 具体含义其实对应的是否打VLAN标签，“关（off）”很好理解，“已标记（tagged）”是带着VLAN标签通过这个端口，“未标记（untagged）”是数据包从端口出去的时候去掉标签，数据包进入端口的时候打上标签。
> 
> 如果不想了解这些技术细节，可以简单的这么理解：如果某个物理端口要属于某一个VLAN就选择“未标记（untagged）”，如果不属于某个VLAN就“关（off）”，如果要允许多个VLAN的数据通过就“标记（tagged）”
> 

最后发现 OpenWRT 似乎已经通过 ODhcpd 搞定了之前需要复杂配置的IPV6 /64 PD 的二级路由地址分配，最后一步就是配置一下防火墙就Ok了

还有一点小问题，通过 curl 访问获取自己的IPv6地址，返回的依然是路由器的地址，而不是当前设备被分配的v6地址，群晖获取到的公网IP地址也是路由器的地址，只能通过局域网地址来上报 DDns，有待研究。

# Share

换了一台设备，在新设备上配置输入法。

使用 Squirrel + 小鹤双拼的方案，很久之前从网上复制了一份小鹤双拼的 schema。

最近发现一个新的配置 [雾凇拼音](https://dvel.me/posts/rime-ice/) 维护挺勤快，上手也简单了不少，图方便，直接上车。

用了一段时间发现之前的用户词库没有同步过来。

仔细看了一下，之前的小鹤双拼使用的是 `luna_pinyin` 的词库，而这次双拼方案使用的是 `rime_ice` 的词库，根据 user_dict_manager.cc 中的逻辑，synchronize 逻辑只会 merge dict_name 相同的 userdb，所以[说明文档](https://dvel.me/posts/rime-ice/#%e7%94%a8%e6%88%b7%e8%af%8d%e5%85%b8%e8%bf%81%e7%a7%bb)添加了一段用户词典迁移的介绍。

另外发现 Squirrel 自己带了一个 `rime_dict_manager` 的工具，尝试使用的时候发现 userdb 文件夹下有一个同步锁，搞起来还是比较复杂的，还是老老实实按照上面的说明文档进行迁移吧。

另外总是记不住输入法的日志文件在什么地方，标记一下

```kotlin
cd $TMPDIR
ls|grep rime
```

就可以找到日志文件啦
