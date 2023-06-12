---
title: ARTS094
tags: ARTS
date: 2023-05-11 23:00:51
---

Lzay is the ladder of progress!

<!--more-->

# Algorithm

none

# Review

[在 Mac 上输入 DNS 和搜索域设置](https://support.apple.com/zh-cn/guide/mac-help/mh141272/mac)

之前没搞懂公司内网的域名解析逻辑，今天偶然看到同事遇到的另一个问题，通过配置 DNS 搜索域解决了，豁然开朗。

也可以参考维基百科的说明，这个配置项看起来也不是 Mac 独有的。

[Search domain](https://en.wikipedia.org/wiki/Search_domain)

[Remote Access API](https://www.jenkins.io/doc/book/using/remote-access-api/)

突发奇懒，想用 Alfred 简化日常触发打包的流程，简单看了一下 Jenkins 提供的 RemoteAccess 方式。

1. 如果 Jenkins 配置了 ACL，需要先到自己的帐号生成 APIToken
2. 需要用 Post 的方式来访问 API
3. 使用 curl 访问时，要指定 `user` 参数和 `data` 参数，指定 `data` 参数后才能触发 Post 请求
4. 具体参数就取决于 Job 的配置了，如果 parameter 指定了默认值，curl 的时候可以不用再次指定

# Tips

修改 git commit message 使用的 template，添加了一行 prompt，尝试让 github copilot 帮忙生成 commit message。

SouceTree 自动配置了一个 template 文件，默认内容是空的。可以参考这个问题的答案

[What is .stCommitMsg in git global configuration file?](https://stackoverflow.com/a/40817750/3819519)

配置 prompt 内容如下，可以自行调整看看效果。

> # Q: hwo to describe this commit? please write it as git commit summary
>

prerequest：

 [A.R.T.S 092](/2023/04/18/ARTS092)

git tag 是有类型区分的，一种是 lightweight tag，另一种是 annotated tag.

详情可查 `man git-tag`

再具体一点也可以看看下面这篇文章

[Git Object: Git 的数据存储模型](https://zhuanlan.zhihu.com/p/351557158)

# Share

none
