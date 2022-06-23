---
title: ARTS069
date: 2022-06-23 15:29:18
tags: ARTS
---

Binder 就是 RPC，RPC 就是 Binder！

<!--more-->

# Algorithm

过

# Review

[Java 8 to Java 11 - Part 1](https://javaworklife.wordpress.com/2019/02/01/java-8-to-java-11-part-1/)

[](https://javaworklife.wordpress.com/2019/02/03/java-8-to-java-11-part-2/)

前段时间因为 KotlinLanguageServer 无法启动的问题，把机器的默认 JVM 切换到了 zulu-11

最近升级 kotlin 版本到 1.4 的时候，发现有个 AnnotationProcessor 不兼容 kapt [变更](https://kotlinlang.org/docs/compatibility-guide-14.html#kapt-names-of-synthetic-annotations-methods-for-properties-have-changed)。升级过程中遇到了坑。

AnnotationProcessor 中用到了 tools.jar 中提供的一些黑科技，用来解析 AST，从中获取一些信息。但是从 JDK 9 开始，这个 lib 就被 Jigsaw 拆分了。要兼容的话需要做一些模块化相关的改动。

自己试着改了一下，遇到了各种问题，包括和 Kotlin 代码混编遇到的坑。

纠结了半天，鉴于目前大家的开发环境还没有搞到 JVM 11 这么高，搞出来可能也会有各种限制，就先不搞了，JVM 1.8 还是万年可用，先搞定任务再说。

AGP 7.0+ 也会需要把运行环境切换到 JVM 11 上去，执行 JVM 8 的字节码应该没啥问题，不过拖久了总会遇到的，先记录一下目前看到的方案，到时候再开整。

[Locking dependency versions](https://docs.gradle.org/6.7.1/userguide/dependency_locking.html)

遇到一个第三方依赖声明错误的问题，发现了 Gradle 也加入了 lock version 的机制。

锁版本就成了最佳实践了呗。

# Tips

过

# Share

过
