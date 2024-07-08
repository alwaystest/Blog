---
title: ARTS130
tags: ARTS
date: 2024-07-08 19:00:05
---

我是憨憨，我在 gradle.properties 中给属性赋值字符串，把字符串用双引号扩上了，De 了一下午 bug。

<!-- more -->

# Algorithm

none

# Review
[AnalyticsService prevent project from loading \[173805889\] - Issue Tracker](https://issuetracker.google.com/issues/173805889)
[Gradle 7.4 fails (could not create instance of AnalyticsService) \[226095015\] - Issue Tracker](https://issuetracker.google.com/issues/226095015)

Gradle 开启 includeBuild 之后 Android Studio Sync Project 报错。
看起来是某个版本引入的 Bug，后面看看升级 AGP 到 8.0 能不能解决问题吧。

The Kotlin gradle plugin, version 1.9.0, has also been applied. This plugin is not included with Gradle and, therefore, has to be described using a `plugin id` and a `plugin version` so that Gradle can find and apply it.

[android - How to find gradle plugin id for given gradle library? - Stack Overflow](https://stackoverflow.com/q/74221701)
[5 reasons to switch to the Gradle Kotlin DSL | Tom Gregory](https://tomgregory.com/gradle/5-reasons-to-switch-to-the-gradle-kotlin-dsl/)
[Writing Build Scripts](https://docs.gradle.org/current/userguide/writing_build_scripts.html)

> The Kotlin gradle plugin, version 1.9.0, has also been applied. This plugin is not included with Gradle and, therefore, has to be described using a `plugin id` and a `plugin version` so that Gradle can find and apply it.

想在新项目改用 kts 好久了，终于让我找到一个试验项目，改完之后发现对 Gradle Plugin 的版本声明方式理解出现了知识的缝隙。
按照之前的理解，我们应该是在 `buildScript.dependencies` 中声明依赖的 Plugin 及其版本，接下来就可以 `apply plugin: xxx` 了。
但是改成 `plugins { id: "xxx" }` 这样的写法之后，Gradle 死活找不到对应的 Plugin。
后来发现对于非 Core Gradle Plugin，都需要同时声明 id 和 version 才能被找到。
这个改动倒也不是 kts 才能这样写的，其实是 Gradle 提供的 API，无论是通过 Groovy 还是通过 Kotlin 都是可以调用的，只是语法上有些许区别，那都是语言层面的东西。

直到看到官方给的这个文档的说明，我终于明白了。

[Gradle - Plugin: com.gradle.develocity](https://plugins.gradle.org/plugin/com.gradle.develocity)

`buildScript.dependencies` 是配合 `apply plugin: xxx` 使用的。
`plugins { id: xxx  version: xxx}` 是配合 `pluginManagement` 使用的。
在 buildScript 声明 plugin 和 对应的版本号，plugins 内是不知情的。
此时的我仿佛是个憨憨。

带着这个认知，再去看官方文档中对于 `plugins` 的介绍，就清晰明了了。

[Using Plugins](https://docs.gradle.org/current/userguide/plugins.html)

之前带着错误的观念把两种声明方式混为一谈，看文档都看的头晕脑胀的。

使用 `apply plugin: xxx` 的时候，可以把各个 Plugin 的配置分离到不同的 gradle 文件中，使用 `apply from: xxx.gradle` 应用就可以了，
迁移到 `plugins` 方式声明之后，在独立的配置文件中通过 Project 对象访问对应的属性和方法有些困难。

看到官方文档也有说明，[Gradle Kotlin DSL Primer](https://docs.gradle.org/current/userguide/kotlin_dsl.html#type-safe-accessors)

> Type-safe accessors are unavailable for Script plugins。

> The `plugins{}` block can only be used in a project’s build script `build.gradle(.kts)` and the `settings.gradle(.kts)` file.

要想继续这么用，还是得费点时间，官方文档也给出了一些说明，接下来就是需要花点时间慢慢摸索对应的 API 了。

[Gradle Kotlin DSL Primer](https://docs.gradle.org/current/userguide/kotlin_dsl.html#sec:kotlin_using_standard_api)

虽然我也特别讨厌 Large Classes，但是 Type Safe 明显更吸引我，鱼和熊掌不可兼得的情况下，我宁愿省点时间，直接把所有配置都写到一个 kts 文件中了。
实在受不了的话，再提一个专门用来做配置的 Plugin，复用一下代码。

# Tips

有一个提效工具是用 Java 写的，定时自动执行，观察 Jenkins 的运行记录，发现进程在执行完成之后总要等一会才能退出。简单研究了一下，原来是 OkHttp 进行网络请求之后线程池和连接池都没有释放，对于普通的 Java 程序，JVM 需要等线程结束才会退出。
写 Android 久了，这些简单的概念居然忘了。
只需要关闭 OkHttp 的连接池和线程池就可以正常退出了。
或者直接调用一下 `exit(0)`

# Share

none
