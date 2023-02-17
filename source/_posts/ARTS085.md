---
title: ARTS085
tags: ARTS
date: 2023-02-17 21:42:27
---

换了个新手机。

<!--more-->

# Algorithm

# Review

[What is the difference between pip and conda?](https://stackoverflow.com/a/68897551/3819519)

又来踩 Python 的坑了。打算发布 Python 包到私有源的时候发现 egg，wheel, setup 一堆概念。这里讲明白了这段历史。

简单理解。egg 和 easy_install 废弃了，不用看。

[`setup.py`](http://setyp.py) 提供了以下功能

- sdist 以源码分发
- bdist_wheel 编译好的二进制分发
    - 方便搞一些 Platform 相关的依赖（C，C++ 啥的）

`[setup.py](http://setup.py)` 大概就像 Java 项目的 pom.xml

[Python Packages](https://py-pkgs.org/08-ci-cd.html)

介绍了 Python 包如何在 Github 做 CI CD。

# Tips

# Share

新的一年，更换了一台新的测试手机。

同是荣耀系列，本来以为手机克隆会容易一些，确发现不是这么简单。

新手机没有 Google 框架，PlayStore 和 GooglePhotos 都没办法使用。

简单看了一下有一个 GSpace 的 App 可以提供 Google 框架的环境，有点像平行世界，通过 GSpace 安装的 App，都是在虚拟的环境中。

对 Google 服务有强依赖的 App，比如 GooglePhotos 可以安装在这里。

像 Spotify 这样依赖没那么强的，通过 GSpace 安装之后反而播放音乐时的通知栏展示会有问题，播放控制按钮全部不可见了。

用 ApkPure 单独下载安装 Spotify 可解。

Youtube 非重度用户，安装 Vanced 也可以，还能支持后台播放，去广告。放个工作背景音挺好用。之前的 Pixel 上安装过，本来以为依赖 Xposed 环境，结果偶然发现 Vanced 还有一个独立版本，可以不依赖 MicroG 之类的东西，挺有意思。

荣耀 3000 价位的机器，送的手机套也太掉价了。
