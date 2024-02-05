---
title: ARTS118
tags: ARTS
date: 2024-02-03 14:00:48
---

上班的路上几乎每天都有交通事故。

上出了一种九死一生的班。

<!--more-->

# Algorithm

none

# Review

none

# Tips

CentOS 停止维护很久了。考虑到安全性和后续各种软件的新特性尝鲜，终于是下定决心把服务器切换到新系统上。

一开始选择了 Fedora，配置一通之后发现新加坡节点的网速甚至比不上美国节点，干脆删了重来。

选择强迫症又对比了一番，发现 Fedora 偏向于试验性质，Debian 偏向于稳定性。

鉴于自己已经不再年轻，没有那么多精力去折腾新东西，选择了更加稳定的版本，实在不行还能 Docker 搞一个 Fedora 的容器来玩。

配置基本环境，在新的系统下试着玩了一下 fish，发现不好玩。

zsh 虽然启动慢一点，胜在顺手。

fish 即便有 oh my fish 的加成，也不像 zsh 一样能很快速的上手。

不过还是勉强用一用吧，万一是我的问题呢。

新版 Docker CE 对 IPv6 的支持已经比较完善了，Compose 中直接就可以指定网段。不需要再去编辑 `/etc/docker/daemon.json`  。

Caddy 搞反向代理不是那么顺利，不像之前的 nginx-proxy + docker-gen 的方案，容器可以独立启动维护，解耦做的更好一些。

更新 template 之后还是踩了一个 upstream 无法访问的坑 https://github.com/nginx-proxy/docker-gen/issues/458

其他方面目前看起来还好。

略微降低了一丢丢机器的配置，大约每月能省个大鸡腿。

# Share

none
