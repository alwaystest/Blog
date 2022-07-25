---
title: ARTS072
date: 2022-07-25 19:09:18
tags: ARTS
---

又认认真真的开始番茄工作法了，效率⬆️️⬆️️
<!--more-->

# Algorithm

Skip

# Review

[Python KeyError Exceptions and How to Handle Them - Real Python](https://realpython.com/python-keyerror/)

Python 访问 Dictionary 的时候，如果 key 不存在，就会抛出异常。

可以使用 `.get` 来避免直接访问，这个时候如果 key 不存在，返回值为 None。

也可以用 `in` 来判断是否存在对应字段。避免了 get 方法无法区分 key 不存在还是 key 对应的本来就是 None 的问题。

[Using multiple working trees in Git](https://blog.oliverjumpertz.dev/using-multiple-working-trees-in-git)

在即刻上看到别人推荐了这篇文档，发现 git 的一个新用法。本来以为没啥机会用到，没想到最近并行开发两个需求，居然用到了这个玩意。还挺好用。

[Sharing build logic between subprojects Sample](https://docs.gradle.org/current/samples/sample_convention_plugins.html)

[Developing Custom Gradle Plugins](https://docs.gradle.org/current/userguide/custom_plugins.html#sec:precompiled_plugins)

研究组件化方案，想看看有没有现成的方案可以抄的，想起来 Detekt 项目下面有很多的 subproject，简单看了一下对应的结构，发现有几个 plugin 不认识，原来是利用了 `precompiled script plugin` 的特性，把共有的构建逻辑抽离成了一个自定义 plugin。然后又用 `composite build` 把应该在 `buildSrc` 中的东西挪到了另一个自定义文件夹中，叫 `build-logic` 。

综合了很多特性，乍一看容易让人摸不着头脑。

# Tips

> This PEP contains the index of all Python Enhancement Proposals, known as PEPs.
> 

原来 Python 的 CheckStyle 结果中的 PEP 对应的是这个意思。

# Share

Skip
[](https://www.notion.so/2d30ad4d996c424b8ba0cb5433b73e43)
