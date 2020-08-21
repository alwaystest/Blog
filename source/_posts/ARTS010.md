---
title: ARTS010
date: 2020-08-21 21:42:25
tags: ARTS
---
# A.R.T.S 010
<!--more-->

# Algorithm

[Corporate Flight Bookings - LeetCode](https://leetcode.com/problems/corporate-flight-bookings/)

Medium，读题就读了半天，不知道是题描述的不行还是我的英文水平不行。

没想到太好的办法，就先来了一个暴力累加。发现居然通过了？杰尼龟眉头一皱，发现事情并没有那么简单。看起来挑战是在实现的复杂度上面。

看了一下别人的解法，发现居然没有看懂。。。

于是又找了一个 [Youtube 的讲解](https://www.youtube.com/watch?v=n5_pD7FRiKM)来看了一下，发现我理解题的方式虽然贴近现实，但离这道题想表达的数学运算离的很远。如果按照这个视频里的方式来理解这道题的话，画个图应该就可以很快的发现规律。

谁会在存储数据的时候把卖出去相同座位数的航班编一个数组存到数据库啊喂！

# Review

[Useful Details About Underscore Keyword in Swift](https://dmitripavlutin.com/useful-details-about-underscore-keyword-in-swift/)

试着写 Swift 代码的时候发现有的方法参数里面定义了 `_` ，有的没有，感觉有点奇怪。于是查了一下这玩意的用途。

似乎我见过的语言里面都是把用不到的变量啊啥的都用 `_` 代替。Swift 里面有 Argument Label  这么个东西，在这里也是用 `_` 来起到省略的作用。一直理解不了 OC 这边加 Argument Label 干啥使，还是基础太虚浮。

# Tips

客串 iOS 开发，被一个老 Bug 缠身。问题是这样的：

有一个自定义 View 继承自 `UITableViewCell`，这个 Cell 有一个点击展开和收起的交互，实现方式是在展开的时候重新测量并设置 View 的高度。这个 View 中有一个 UILabel，展示了一段长文字，文字在比较窄的屏幕上会自动换行展示。但是在 iPad 上，这段文字的展示变成了一行+省略号。

找了半天没找出来什么原因，反而还怀疑是不是 `systemLayoutSizeFittingSize` 这个方法测量的有问题。找 iOS 同事求助后找到了原因。分析如下：

出问题的代码实现是对 `UITableViewCell.contentView` 进行了测量，然后将测量得到的高度设置给这个自定义 View。

但是 `UITableView` 是有一个 SeperatorStyle 属性的，这个属性会导致 `UITableViewCell` 中包含一个 `UITableViewCellSeperatorView` 这个 View 也占用了一部分高度，此时把 ContentView 的高度设置给 Cell，就会导致实际的 Content 高度被挤压了一部分，导致 `UILabel` 的内容无法完全展示。

![Layout](layout.png)

目前的解决方式是在测量 ContentView 的高度后，判断如果有 Seperator，则手动再累加一个 Seperator 的高度。

比较奇怪的一点是，这种情况，我的直觉是可以直接测量 Cell 的高度，而不是测量 Content 的高度，这样有没有 Seperator 系统都会帮我们测量到，就不必这么麻烦了。但是实践了一下发现最后测出来的高度反而是有问题的。

半吊子开发果然还是处处受限。需要稳扎稳打的学习 iOS 开发基础。

# Share

[极简护肤](/2020/08/21/SimpleSkinCare)
