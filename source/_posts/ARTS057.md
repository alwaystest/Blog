---
title: ARTS057
date: 2022-03-10 18:44:22
tags: ARTS
---

太敏感可能真的不是一件好事

<!--more-->

# Algorithm

过

# Review

[Mocking is not rocket science: MockK features](https://blog.kotlin-academy.com/mocking-is-not-rocket-science-mockk-features-e5d55d735a98)

在看 Mockk 的一些文档

# Tips

为了编译一个项目，在磁盘上新建了一个大小写敏感的卷，后来编译别的 App 的时候发现出现了一些奇怪的问题，别人的机器可以正常编译，我的就不行。

突然想起来可能和代码存放的位置有关，于是把代码挪了个位置，放到了不区分大小写的卷。

恢复正常了居然。

我的 Case 是使用 Gradle 编译另一个 Android App，报错：

> Extension of type 'JavaPluginExtension' does not exist
> 

[Multi-project test dependencies with gradle](https://stackoverflow.com/q/5644011/3819519)

遇到了一个跨 Module 分享 Test Src 中帮助类的问题，发现一个 Plugin 可以搞定

`java-test-fixtures`

# Share

前段时间试着用 `watchtower` + `chinesesubfinder` 两个 Docker 镜像来尝试自动给 Jellyfin 中的电影匹配字幕，最近发现 chinesesubfinder 的镜像无法被 watchtower 自动更新到最新了。检查了一下日志，发现是我给 chinesesubfinder 设置了端口映射，但是 chinesesubfinder  的 docker 镜像中并没有 expose 这个端口，如果是手动维护的话，不会有什么问题。但是 watchtower 在自动更新容器的时候会检查 exposed 端口数量是否和当前容器配置的端口映射数量相同，如果不同，会拒绝更新。

这就麻烦了，搜到一个 Issue https://github.com/containrrr/watchtower/issues/945，但是最终 watchtower 也没有进行修改。

找 chinesesubfinder 的作者更新一下 dockerfile 也是一个思路，不过今天太晚了，我先自己搞定吧。

之前更新容器的时候，用 docker-compose 可以方便的保留容器的配置信息，不过在群晖上还没试过。又翻了一下 chinesesubfinder 的说明文档，发现了 portainer 这个神器。直接点开容器信息，GUI 中提供了一个 recreate 的选项，真是太棒了！还提供了 volume 和 image 的检测，清理不用的 volumes 和 images 又方便了一大步，点赞。
