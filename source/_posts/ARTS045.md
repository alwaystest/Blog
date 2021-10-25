---
title: ARTS045
date: 2021-10-26 01:37:50
tags: ARTS
---

> Vimscript isn't going to help you much if you wind up fiddling with your editor all day instead of working, so it's important to strike a balance.

<!--more-->

# Algorithm

[Merge k Sorted Lists - LeetCode](https://leetcode.com/problems/merge-k-sorted-lists/)

有段时间没有看数据结构了，居然忘了最小堆的使用。

Hard 就 Hard 在细节处理上。犯了几个错误

- Comparator 用 kotlin + Vim 不会写了，没有 IDE 果然还是不行
- 只处理了部分异常数据，考虑的不够全面
- 一下子把所有元素都放进堆里面，取出的时候，链表中搞出了环。虽然可以用手动 new node 的方式避免，但是内存效率就低了。
- 一下子把所有元素都放进堆里面，增加了元素之间比较的次数，效率会低一些。

Vim 总算是好用一些了。我之前都是怎么用的。。。

# Review

[Learn Vimscript the Hard Way](https://learnvimscriptthehardway.stevelosh.com/chapters/14.html)

最近在折腾 VIM 的配置，踩坑中。也学到了不少新东西。这个电子书挺好，打算过一遍全文，加深对 VIM 的理解。

[Learn Vimscript the Hard Way](https://learnvimscriptthehardway.stevelosh.com/preface.html)

开始从头阅读，这个前言好精彩。

# Tips

`set xxx?` 可以展示当前设置的值

`set xxx!` 可以快速改变 bool 的值

`ZZ` 可以快速保存关闭，比 `:wq`  的键位少一点

增加了 `,ev` 和 `,sv` 的配置，用来快速编辑和应用 vimrc

设置了 `jk` 作为退出 InsertMode 的 map。经过几天的试用，感觉还是 Esc 更加方便。

- 在 Vim 模式中，这样设置挺好的。
- 在 Idea 中，由于有很多弹出的 Dialog，比如 Search 代码的时候，有弹窗的时候，返回主界面还是 Esc 简单。甚至 `<c-[>` 都比 jk 好用。所以在 Idea 中的时候， Esc 还是一个特别常用的键，频繁的切换 Esc 和 jk 两种用法，不利于形成肌肉记忆。

所以估计我之后还是 Esc 用的更多一些。反正我的键盘也有独立的 Esc 键，按起来还挺方便的。

# Share

略过
