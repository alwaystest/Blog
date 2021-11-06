---
title: ARTS047
date: 2021-11-06 16:57:50
tags: ARTS
---

有的树已经秃了，有的还没有。
人也一样。

<!--more-->

# Algorithm

[Remove Nth Node From End of List - LeetCode](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

Medium，移除链表中的节点。难度倒是不大。需要考虑一下细节，应该移除的是哪个。

# Review

上班路上一直在看 LearnVimScriptTheHardWay，学到不少。总算对自己的 vimrc 有了更多的掌控。

# Tips

外接硬盘使用 CaseSensitive 导致 Gradle 编译报错 Class Duplication。

尝试干掉 `~/.gradle` 文件夹，在 CaseInsensitive 的地方新建这个文件夹重新编译。

外接硬盘可以不用格式化，直接使用 Disk Utility 在磁盘上新建 Volume，格式选择 APFS 即可。新旧两个 Volume 会共用一个硬盘，但是数据是隔离的，简直太省事了。我之前的概念还停留在磁盘分区，不同的区使用不同的磁盘格式。找了一下相关说明：

[在 Mac 上的"磁盘工具"中添加、删除或抹掉 APFS 宗卷](https://support.apple.com/zh-cn/guide/disk-utility/dskua9e6a110/21.0/mac/12.0)

华为的 Maven Repo 有坑。

`[https://developer.huawei.com/repo](https://developers.huawei.com/repo)` 

在这个 Repo 请求不存在的依赖，比如 `com.androidx.fragment` 会返回一个 HTML，导致 Gradle 解析结果的时候报错。

自己搭建一个 Sonatype，代理华为的这个 Repo 即可。如果发现 Repo 状态为 Unavailable 或者 Automatically blocked，可以关闭 Sonatype 的 AutoBlocking。否则这个问题会导致 Sonatype 认为远端 Repo 不可用，Proxy 无法生效。

# Share

依然没有
