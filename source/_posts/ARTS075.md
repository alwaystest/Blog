---
title: ARTS075
tags: ARTS
date: 2022-12-03 18:50:22
---

click to see more!
<!--more-->

# Algorithm

# Review

# Tips

sonarqube task 出了点问题，想看看它为啥会调用 compile task，按理说这个东西只进行上传分析就行了。

`gradle tasks --all` 可以查看 gradle 的 task 的依赖情况，但仅限于 Gradle 老版本，新版目前只看到 Stackoverflow 上有人分享了 Plugin 来实现这个功能。最新版本需要 Gradle 6.8 以上才能运行。

用上一个版本运行的时候好像死循环了，一直结束不了。

最终查看[源码](https://github.com/SonarSource/sonar-scanner-gradle/blob/master/src/main/java/org/sonarqube/gradle/SonarQubePlugin.java)看到了 Plugin 声明 Task 依赖的逻辑。

想来是这样，jacoco 的 exec 文件并不包含所有的信息，还需要 class 文件才能生成分析数据。

[What's the difference between mustRunAfter and dependsOn in Gradle?](https://stackoverflow.com/questions/42033490/whats-the-difference-between-mustrunafter-and-dependson-in-gradle)

顺便看到了 mustRunAfter 和 dependsOn 的区别

# Share

