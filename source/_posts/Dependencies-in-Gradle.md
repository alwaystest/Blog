---
title: Dependencies in Gradle
date: 2020-06-21 22:51:51
tags: Programming
---
# Dependencies in Gradle

使用 Gradle  构建的时候，项目依赖的 Lib 产生了版本冲突，记录一下 Debug 用到的命令。
<!--more-->

1. 在执行 Task 的时候，增加 `--scan` 参数。就可以从 Gradle 的 BuildScan 网站看到本次构建的依赖情况。

    从这里，你可以知道是哪个 Configuration 的哪个 Lib 产生了版本冲突，为后续查询提供线索。

    当然，版本依赖产生冲突的时候，Console 里面基本已经打出来具体是哪个 Lib，哪些版本冲突了，大可不必再到 BuildScan 去看。BuildScan 只是展示起来更美观一些。

2.  查看对应的 Configuration 为什么会选择那个版本，想办法让选择了低版本的 Configuration 选择一个高版本的 Lib 就可以了。

    举个例子：我这次遇到的报错是某个 SubProject 的 `debugRuntimeClasspath` 这个 Configuration 的 Retrofit 版本出现了问题。

    `./gradlew -q :$subProject:dependencyInsight --dependency com.squareup.retrofit2:retrofit --configuration debugRuntimeClasspath`

    这个命令可以反向追踪 Gradle 为什么会选择当前的 Retrofit 的版本。

在我这里的例子中，命令执行的结果是 Gradle 并没有提示版本冲突，从哪两个版本中选择了一个，而是直接提示选择的版本。但是在下面又把别的库依赖的高版本降了到了低版本，真的是十分奇怪了。如果说遇上了 Gradle 版本冲突判断的 Bug，但我在另一个使用相同 Gradle 版本的项目中执行上述命令，却能提示版本冲突，Gradle 自动选择了更高的版本。

与此同时又带来另一个问题，版本号其实没有一个特别强制的准则，Gradle 是怎么判断各个 Lib 哪个版本是新的呢？找了半天没找到相关文档描述，看起来只能找机会读源码了。

Gradle 官方文档也提到了好多解决版本冲突的办法。包括强制使用版本，忽略低版本的依赖等等。总有一个办法能解决这个问题。

我个人总觉得强制使用的版本会有什么坑，比如代码模块化之后，SubProject 升级了某个库的依赖，RootProject 却没有更新强制使用的版本。如果代码不兼容，编译失败还好说，发现的比较早。如果是各种接口都兼容，测试过程中各种正常，上线后才发现版本没升上去……

我偏好的做法是升级一下依赖。但是如果 Lib 是第三方维护的，emm，起码也有别的办法先让它通过编译。BreakingChange 的话，Fork + PR 吧。

## 贴几个常用的 Gradle 的 Task

### Projects

一个项目可以由一个 RootProject 和若干个 SubProject 组成。

> The powerful support for multi-project builds is one of Gradle’s unique selling points.

展示当前项目的所有 Projects

```bash
./gradlew projects
```

### Configurations

> In Gradle, the scope of a dependency is called a configuration.

A build script developer can declare dependencies for different scopes e.g. just for compilation of source code or for executing tests.

查看 Configurations

```bash
./gradlew projects --info // 会输出 Creating configuration 字样 
```

或者

```bash
./gradlew :$project:dependencies
```

> Every Gradle project provides the task dependencies to render the so-called dependency report from the command line. By default the dependency report renders dependencies for all configurations.

换言之，这个命令可以获取到所有的 Configuration

然后，我们就可以使用以下命令，来查看对应的依赖库的版本是怎么决定的了

```bash
./gradlew -q :src:dependencyInsight --dependency $lib --configuration $configuration
# eg:
./gradlew -q :src:dependencyInsight --dependency com.squareup.okio:okio --configuration debugRuntimeClasspath
```
