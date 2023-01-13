---
title: ARTS084
tags: ARTS
date: 2023-01-13 10:23:16
---

事已至此，年后再说吧。
<!--more-->

# Algorithm

# Review

[Ghost HTTPS Redirect Loop](https://www.supercodebros.dev/ghost-https-redirect-loop/)

白嫖的域名好像被别人买走了，迫不得已自己购买了一个新的域名。

迁移网站之后发现访问网页出现了不停的 301 重定向的情况。

还以为是我的 Nginx 配置出问题了，和迁移前的网站配置对比了半天之后发现是 Cloudflare 对 SSL 的配置有区别，不能使用 **`Flexible SSL` 。**

找到一篇文章描述这个问题，水落石出了。但其实观察日志应该能观察出来 Cloudflare 的 Request 全部都是 http 协议的吧？

# Tips

[Should I have Travis cache node_modules or $HOME/.npm](https://stackoverflow.com/a/42523517/3819519)

最近想折腾一下 Blog 的 Cache 逻辑，避免每次生成都会改变 ModifiedTime。

顺带看了一下 Node 的 Cache 逻辑。

现在 setup-node 已经处理了 Cache，但是奇怪的是他们不推荐 cache node_module，所以查了一下。涨姿势了。

另外学到了现在 CI 环境需要用 `npm ci` 来代替 `npm install` 了。

这里的 ci 居然是 clean install 的缩写。这种情况下 node_modules 会被干掉，所以 cache 这个目录是没有用的。

# Share
