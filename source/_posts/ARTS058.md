---
title: ARTS058
date: 2022-05-03 16:27:26
tags: ARTS
---

Gradle 真是够复杂的。

<!--more-->

# A.R.T.S 058

# Algorithm

ignore

# Review

[Using Gradle Plugins](https://docs.gradle.org/current/userguide/plugins.html#sec:plugin_markers)

[Developing Custom Gradle Plugins](https://docs.gradle.org/current/userguide/custom_plugins.html#note_for_plugins_published_without_java_gradle_plugin)

[Using Gradle Plugins](https://docs.gradle.org/current/userguide/plugins.html#sec:custom_plugin_repositories)

升级 AGP，遇到 AppJointPlugin 和 AGP:4.2.0 不兼容的问题，fork 修改了一下， 想发布到 jitpack 上使用。Plugin publish 的过程中遇到一些问题。研究了一下 Gradle Plugin 的开发姿势。

想要使用 Composite Build 隔离 Plugin 的构建过程，踩了很多坑，都是对 Gradle 的机制不熟悉导致的。

[How to use Composite builds as a replacement of buildSrc in Gradle](https://medium.com/bumble-tech/how-to-use-composite-builds-as-a-replacement-of-buildsrc-in-gradle-64ff99344b58)

[Gradle Plugins and Composite Builds](https://ncorti.com/blog/gradle-plugins-and-composite-builds)

# Tips

https://github.com/gradle/gradle/issues/18570

不要太激进的升级你的 Gradle，Gradle 7 编译出来的 Plugin 没法在 Gradle 6 上运行。报错 AbstractMethodError。要不是搜索了一下，又要在这里浪费整个下午了。

Gradle 的 Composing Build 在 6.x 版本上的支持不太行，`./gradlew projects` 在 Gradle 7 上才能展示 `includeBuild` 的 Project。

Gradle 6 上不能直接通过 `./gradlew :includedProj:tasks` 调用对应的任务

Gradle 6 上通过以下方式 Apply Plugin，会导致 AppJoint Plugin 在 Project 中寻找已经注册的 android plugin 的时候报错，看起来这个时候 application plugin 是没有 apply 的，特别奇怪，只能回退到之前的 `apply plugin` 的写法。

```groovy
plugins {
    id "com.android.application"
    id "app-joint"
}
```

总之，感觉新的 Gradle DSL 还有很多需要完善的地方，要玩转新的 Gradle，阅读官方文档是必不可少的。

ComposingBuild 用来开发 GradlePlugin 真好用，在带 Demo 的项目中，直接用 project substitude 对应的 Plugin，避免 Gradle 初始化项目的时候遇到 apply plugin 但是 plugin 又需要 build 完项目之后才能生成的问题。

算是鸡生蛋蛋生鸡的一个解决方案了。

# Share

ignore
