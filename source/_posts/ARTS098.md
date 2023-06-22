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

none

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
