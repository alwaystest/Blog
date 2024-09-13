---
title: ARTS135
tags: ARTS
date: 2024-09-13 10:43:17
---

我是真的不会 vim

<!-- more -->

# Algorithm

none

# Review

none

# Tips
突发奇想用 vim 写一个 python 脚本。
配置了半天 coc.nvim，发现自动补全的使用体验不尽如人意。
正常输入英文字母是可以触发补全的，但是第一行我想写一个 shebang，在输入 `#` 之后没有出现任何的补全，于是我开启了一场漫长的排查之旅。
[not showing popup for certain trigger\_word · Issue #348 · neoclide/coc-snippets · GitHub](https://github.com/neoclide/coc-snippets/issues/348)
这里提到只输入符号不会触发 coc 的自动补全。
于是在 AI 带领下一头钻进源码尝试找相关的逻辑，AI 胡言乱语，我一无所获。
看不懂代码时最好的办法还是直接 Debug。

```
let $NVIM_COC_LOG_LEVEL = 'trace'
```

开启 Debug 模式之后，进行一番操作，查看 coc 的日志。发现一个关键表现。
触发自动补全时，input 是空字符串，所以导致没有自动补全。

看起来这里的逻辑是 `util.ts` 中 `getInput` 函数来定义的。
知道函数就好办了，在 AI 的辅助下总算是搞明白了这里的逻辑。

> `getInput` 方法的主要作用是从给定的字符串中提取出最后一个单词。这个方法在自动补全功能中很有用，它可以帮助确定当前正在输入的单词。

那么纯输入符号，无法构成单词，也就无法自动触发补全。

用起来虽然简单，但是 Debug 还是需要耗费很长时间的。

> Vimscript isn't going to help you much if you wind up fiddling with your editor all day instead of working, so it's important to strike a balance.

写脚本的过程中又遇到另一个问题
执行 `pip3 install -e` 之后本来以为所有代码都可以直接修改生效，试了几次之后发现我写的 script 无法及时更新，必须重新安装才能生效。
AI 也回答不出来为啥，我提问的机巧真是太弱鸡了。
Google 一番才发现 setupTools 直接给出了[说明](https://setuptools.pypa.io/en/latest/userguide/development_mode.html#limitations)：

> xxx, executable script files xxx may be exposed as a _snapshot_ of the version they were at the moment of the installation.

试了一下，如果不是 script，在外部新写一个脚本，import 项目中的 module，那么每次修改 module 的内容，通过外部脚本 import 来使用，是可以得到及时更新的。

太菜了太菜了，简简单单的问题搞了太久。

# Share

none
