---
title: ARTS064
date: 2022-05-07 18:26:20
tags: ARTS
---

隔离隔离。

<!--more-->

# Algorithm

None

# Review

None

# Tips

> **默认网关（Default Gateway），也叫缺省网关，是子网与外网连接的设备，通常是一个路由器。当一台计算机发送信息时，根据发送信息的目标地址，通过子网掩码来判定目标主机是否在本地子网中，如果目标主机在本地子网中，则直接发送即可。如果目标不在本地子网中则将该信息送到默认网关/路由器，由路由器将其转发到其他网络中，进一步寻找目标主机。**
> 

搞了一个旁路网关，担心会影响内网多媒体通信，比如 DHCP 下发了网关地址为旁路网关，导致局域网下访问 Jellyfin 也需要把数据发送到旁路网关绕一圈。查了一下相关概念。当年学的网络知识有一些遗忘了，算是复习一下。结论是旁路网关不会影响局域网内部数据传输。

佐证

[Openwrt 路由总结：自动编译固件、正确设置旁路网关，破解迷思......](https://blog.lishun.me/openwrt-mega-post)

docker 容器运行 openwrt 为何要开启网卡的混杂模式

https://github.com/lisaac/openwrt-in-docker/issues/6

> Your networking equipment needs to be able to handle “promiscuous mode”, where one physical interface can be assigned multiple MAC addresses.
[https://docs.docker.com/network/macvlan/](https://docs.docker.com/network/macvlan/)
> 

树莓派中宿主机不开启混杂模式好像对 Docker 中的 OpenWRT 能否正常运行没有什么影响。

机器重启，混杂模式就自动失效了。

待确认：不开启混杂模式，路由器会提示某个 IP 的 Mac 地址发生变化。可能是混杂模式关闭导致的。

主路由配置 DHCP 网关为 OpenWRT 的 IP 地址时，树莓派宿主机会因为无法访问网关而无法上网。因为 macvlan 模式有限制，宿主机和容器无法直接相互访问。

网上有相关的介绍，再增加一个 macvlan 模式的网桥，就可以访问容器了。

问题是路由表好像是自动把 DHCP 的网关添加到了最高优先级的位置，所有网络请求都会直接访问 OpenWRT 的地址，而不是走网桥绕一下。

```groovy
sudo ip route add $OPENWRT_IP dev macvlan # 执行访问 OpenWRT 的 IP 地址由网桥路由
```

此时单独 ping 网桥是可以通的，但是访问外网，又会匹配到之前的规则，直接访问旁路网关。

```groovy
sudo ip route del default # 手动删除这条规则，就可以绕路网桥访问旁路网关
```

另一种方案

修改 `/etc/dhcpd.conf` 文件，指定 eth0 使用静态地址。手动指定网关为主路由。

修改 `/etc/network/interfaces` 好像无法阻止 router 自动添加路由规则

这种方案只能让宿主机器访问外网，流量不经过旁路网关，部分访问会受限。

找到了更加合适的方案

手动调整 eth0 的路由优先级，降低即可

```groovy
# vim /etc/dhcpcd.conf

intreface eth0
metric 1000 # 降低路由规则优先级
```

[Can I prevent a default route being added when bringing up an interface?](https://unix.stackexchange.com/a/657438/305653)

发现了一个新的软件包安装方法 snap

可以在树莓派上成功安装 nvim

感觉理念上有点子 brew 的意思

# Share

None
[](https://www.notion.so/25e5ea05703e429081064be35575af1f)
