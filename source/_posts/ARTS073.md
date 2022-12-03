---
title: ARTS073
date: 2022-12-03 12:01:01
tags: ARTS
---

点开看看呀。

<!--more-->

# Algorithm

# Review

[Project (Gradle API 7.5)](https://docs.gradle.org/current/javadoc/org/gradle/api/Project.html)

Gradle 的 Project Interface 的描述，Project Interface 是 programmatic 访问 Gradle 特性的入口，这里的文档说明也很清晰。一些入门的基本概念基本都介绍到了。长时间不搞 Gradle 相关的开发很容易会忘记，可以翻翻这里，快速回忆 Gradle In Action 的相关内容。

[Improving Java Interop: Top-Level Functions and Properties | The Kotlin Blog](https://blog.jetbrains.com/kotlin/2015/06/improving-java-interop-top-level-functions-and-properties/)

用 FatAAR Plugin 打包遇到一个 Kotlin 找不到方法定义的问题，最终发现是一个 Bug 导致 kotlin_module 文件没有被打入 aar 中，虽然 kotlin 的产物也是 jvm 的 class，但是 kotlin 调用自己定义的方法居然还需要使用 kotlin_module 文件来进行引导，反而是 Java 可以不需要 kotlin_module 直接调用。神奇。

这篇文章介绍了 kotlin 在实现 TopLevel Functions 时的各种考虑，遇到的各种问题和解决方式。使用的时候完全没有想过这个问题，一旦深入考虑，就会发现基础部分的坑大得很。这篇文章写的特别赞。

[Customizing publishing](https://docs.gradle.org/current/userguide/publishing_customization.html)

Gradle 的 Components 介绍。

给 Android 的 AAR 配置 MavenPublish 任务时，发现即使给 `publish` 设置了依赖 `assemble` task，执行的时候也会报错找不到构建产物。

看了一下任务的执行过程，发现 publish 或者 publishToMavenLocal 只是对外暴露的一个门面，内部实际上依赖了更详细的任务，需要给这个任务配置依赖。但是内部的任务是根据 publication 和 repository 的名字拼接出来的，如果不想在代码中硬编码这个名字，就需要另想办法。

搜索到了 [stackoverflow](https://stackoverflow.com/a/42160584/3819519) 的方案，顺着找了一下原理。原来是和 Components 有关。

maven-publish plugin 的源码可以在 Gradle 源码中找 `org.gradle.api.publish.maven.plugins.MavenPublishPlugin`

Gradle 真的好复杂。

# Tips

Gradle 的 Project evaluate 的顺序默认是 BFS，也提供了对应的 API 声明依赖，修改 evaluate 的执行顺序。

如果要等所有的 projects 都 evaluate 之后再配置某个 Task，也可以直接使用 `gradle.projectsEvaluated` api 来配置

[Execute Gradle task after subprojects are configured](https://stackoverflow.com/a/53771516/3819519)

`derive` 和 `deverge` 乍一看长得很像，但意思却恰恰相反。派生和分歧。看文档的时候差点给我忽悠瘸了。

[GitLab Pages administration | GitLab](https://docs.gitlab.com/ee/administration/pages/index.html#overview)

gitlab 提供了 pages 的功能，发布静态网站相当方便了。

# Share

