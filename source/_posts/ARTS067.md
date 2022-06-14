---
title: ARTS067
date: 2022-06-01 18:32:46
tags: ARTS
---

AnnotationProssor 与 JVM 的 Debug 方式。

<!--more-->

# Algorithm

None

# Review

[Debug annotation processor in Kotlin](https://cafonsomota.medium.com/debug-annotation-processor-in-kotlin-6eb462e965f8)

升级 Kotlin 版本，kapt 报错

之前没自己写过 AnnotationProcessor，刚好熟悉一下这里

这篇文章提供了一个很好的例子，解释了如何在 AnnotationProssor 中打日志，如何使用 IDE 的 Remote Debug 方式调试 AnnotationProssor。

涉及到 JVM Debug，Gradle，Kapt，Kotlin 分别需要设置的参数，帮助很大

[Annotation processors in Gradle with the annotationProcessor dependency configuration](https://tomgregory.com/annotation-processors-in-gradle-with-the-annotationprocessor-dependency-configuration/)

AnnotationProcessor 在 Gradle 中是如何被传给 javac 的

# Tips

[Kapt annotation processing - how to show full stacktrace](https://stackoverflow.com/a/60358306/3819519)

kapt 执行的时候可以设置一个 verbose 参数，方便看到更多的执行细节

# Share

None
[](https://www.notion.so/5f13e8882fdb4660bc11145e1bf3ecc2)
