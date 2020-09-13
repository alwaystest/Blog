---
title: ARTS013
date: 2020-09-13 13:40:56
tags: ARTS
---
# A.R.T.S 013
<!--more-->

# Algorithm

[Explore - LeetCode](https://leetcode.com/explore/challenge/card/september-leetcoding-challenge/554/week-1-september-1st-september-7th/3449/)

上周玩了一道 LeetCode September Challenge。

输出两个二叉搜索树合并之后的内容，要求结果是有序的。

感觉有点像合并两个有序链表的变形题。

两个思路

- 将二叉树中序遍历得到两个有序数组，按照合并有序数组来写
- 随便遍历获取二叉树的内容，对结果进行排序

---

[Linked List Random Node - LeetCode](https://leetcode.com/problems/linked-list-random-node/)

Medium。感觉这道题应该是简单级别的。

直接考虑 Follow up 的问题

> What if the linked list is extremely large and its length is unknown to you? Could you solve this efficiently without using extra space?

第一想法是按照 TopK 问题一样，对于每个 Node，Random Pick Boolean，如果 True，就 update 要返回的 Result。想法很美好，但是提交了之后没有通过验证。应该是从数学的角度上考虑，这个方法没办法保证选择每个 Node 的概率是一样的。

想来想去还是没有想到更好的遍历一次就搞定的办法。这道题至少是需要遍历一次列表的，遍历完之后长度就已知了。目前能想到的办法还是先遍历一次，获取到链表的长度，然后随机一个数字，取对应的 Node 的 Value 就可以了。

# Review

[JaCoCo - Control Flow Analysis](https://www.jacoco.org/jacoco/trunk/doc/flow.html)

[Class Ids](https://www.jacoco.org/jacoco/trunk/doc/classids.html)

之前看别的文章一直说 Jacoco 使用 Probe 来进行覆盖率结果统计，已知没有找到相关的官方描述的文档，上周看覆盖率的事情，终于被我翻到原理篇了。

# Tips

[esafirm/pokedroid](https://github.com/esafirm/pokedroid/blob/master/gradle/jacoco.gradle)

学到了一个技巧，在一个包含多个 SubProject 的 Gradle 工程中，要获取所有 SubProject 的 src 和编译好的类文件的路径。可以采取类似 MapReduce 的方式，让每个 SubProject 自己收集自己的 Path，然后在 RootProject 里面读取即可。

# Share

🕊️
