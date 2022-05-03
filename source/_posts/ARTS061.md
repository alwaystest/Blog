---
title: ARTS061
date: 2022-05-03 16:34:13
tags: ARTS
---

Copilot 居然能猜到我写 Mock 的 JSON 数据时要编什么内容，确实是有点牛皮的。

写 Groovy 脚本打印 Debug 信息的时候也能给出提示，厉害了。

<!--more-->

# A.R.T.S 061

# Algorithm

ignore

# Review

[Anti-corruption Layer pattern - Cloud Design Patterns](https://docs.microsoft.com/en-us/azure/architecture/patterns/anti-corruption-layer)

ACL: Anti-corruption Layer

一种隔离两个子系统的设计模式

[【内网穿透笔记】NAPT类型测试与XTCP点对点内网穿透适用例外_邓大帅的博客-CSDN博客_frp xtcp模式](https://blog.csdn.net/deng_xj/article/details/89187944)

[2020-1-11-内网穿透神器frp之进阶配置](https://xinyuehtx.github.io/post/%E5%86%85%E7%BD%91%E7%A9%BF%E9%80%8F%E7%A5%9E%E5%99%A8frp%E4%B9%8B%E8%BF%9B%E9%98%B6%E9%85%8D%E7%BD%AE.html)

学习了一下 NAT 内网穿透的限制条件

# Tips

https://github.com/neovim/neovim/issues/2386

新设备上又遇到了 VIM 粘贴中文乱码的问题

新设备上重新配置了一下开发环境，由于硬盘大小限制，打算把 Cocoapods 的 Spec 切换到 CDN 去，却发现 pod install 报错

> Unable to add a source with url `https://cdn.cocoapods.org/` named `trunk`
> 

看 Github 的 Issue，Cocoapods 1.9 开始出现这个问题，一个解决办法是回退到 1.8

哪里有往后退的道理，brew 上看到有 1.13 版本，于是升级，看起来问题解决了。

# Share

AppJoint 或许不应该支持从 AAR 中读取 ServiceProvider 注解的类

AppJoint 应该是一个类似于胶水一样的工具，把多个模块提供的 Impl 在某个地方集中声明，注入到 core 中。

对于不太稳定的模块，对外暴露的接口可能经常会变，如果把 Mediator 的声明和实现都下放到底层 Lib，代码管理起来会更加混乱。

在壳 Module 中统一提供 ServiceProvider 实现，Lib 只负责提供具体实现，由壳 Module 决定哪个 Service 的方法应该调用哪个实现。

如果为了集成方便，在 AppJoint 的 Transform 中加入更多在 AAR 中寻找 ServiceProvider 注解的实现类的逻辑，维护起来可能会更加麻烦，Transform 的性能也会变慢，毕竟要解开那么多 Lib 去遍历。

简单一点就好。
