---
title: ARTS086
tags: ARTS
date: 2023-02-17 21:44:19
---

弱小和无知不是生存障碍，傲慢才是。

<!--more-->

# Algorithm

# Review

[QECon上海站TOP10精选好文｜蚂蚁测试用例智能生成技术架构与实践](https://mp.weixin.qq.com/s/Htnl9CffNKB39nxfU_psag)

看到同事在尝试自动生成测试用例。了解了一下蚂蚁的开源项目。

整体上用例的生成方式比较偏学术，不太能理解这套东西是怎么判断期望是什么，从而生成各种 Verify  的。

按照之前 TDD 的思路来看，这个有点像先射击，再画靶子。

根据已有代码来生成代码测试这套代码，总觉得有点别扭。

开个脑洞，流浪地球 2 中用量子计算机直接生成操作系统，感觉是可行的。将来的程序员应该也不需要搞懂底层代码是怎么写的了，生成的直接是纯二进制流，不可读，但是足够精简，高效。这样的代码似乎也没有必要再依赖单元测试了。或者说测试用例只管业务层是什么样的表现，而不用去管具体的实现是什么样子的。

# Tips

[Location of files: Main Storage versus SD card](https://android.stackexchange.com/a/196621)

发现一个 Android SD 卡挂载路径的描述。部分场景下文件操作失败，ReadOnlyFileSystem 报错的时候，可以考虑一下是不是设备插了 SD 卡，并且 SD 卡损坏了。

# Share

我们有一个项目，近期把依赖全部迁移到了 AndroidX，于是关闭了 Jetifier，灰度上线。

在灰度期间却发现出现了一些奇怪的崩溃。

崩溃发生在 Native 层，经过排查之后确认 Native 层的代码没有问题，日志显示是 Java 层的某个类找不到了。

ClassNotFound，那基本可以判定问题出现在混淆过程中了。

排查 mapping 文件，发现没有相关的记录，八成是被裁剪掉了。

在 usage.txt 里面找到了痕迹，这个类果然被干掉了。

用 IDE 去看被裁剪掉的这个类，很奇怪，明明有 `@Keep` 注解，为啥还会被裁剪掉呢？

莫非是 Proguard 规则没有配置对？

排查项目的 Proguard 规则。确实没有发现相关的规则。奇怪了，对比另一个项目，也没有单独添加对应的规则，但是另一个项目就没有问题。

Android 的 AAR 库是可以自己带 Proguard 规则的。于是顺藤摸瓜，看看定义 `@Keep` 的这个库是怎么搞的。

瓜在 `androidx.annotation:annotation` 这个库中。

仔细一看，这个库是用 jar 的格式提供的。jar 内部包含了一个 `META-INF/proguard/androidx-annotations.pro` 文件，在这里定义了 Proguard 的规则。

没见过呀，这玩意咋用？

原来早在 2018 年，[R8 就开始支持从 Jar 中获取 Proguard 规则了](https://www.notion.so/A-R-T-S-082-2d3d97d4f37d40c596e530cb1bba615a)。

所以在 2023 年，只要用比较新版本的 AGP，应该都可以自动把规则用起来。

检查一遍项目，并没有禁用 R8。

把构建过程中使用的 Configuration 打出来看看。

```
// You can specify any path and filename.
-printconfiguration ~/tmp/full-r8-config.txt
```

发现 androidx 相关的注解是有的。真神奇，难道发现了一个 R8 的 Bug？

对比另一个项目，声明 Proguard 规则的时候少了一个 `getDefaultProguardFile('proguard-android.txt')` ，这里确实有一个和 `@Keep` 相关的规则，不过是 Support 库的。我们都迁移完 AndroidX了，应该不至于是这里的问题吧。不过 Configuration 里面确实也没有 Support 库相关的 Keep 注解。不妨试试。加上之后发现居然生效了。mapping 里出现了之前被裁剪掉的类。说明这次构建成功的保住了这个类。

那是为什么呢？

我们关闭 Jetifier 的时候，只处理了最终产出 APK 的项目。几个生产 AAR 的项目还没有关闭。恰巧我去看代码的时候，用的是生产 AAR 的那个还没有关闭 Jetifier 的项目。所以在我看来，所有注解全部是 AndroidX 的。但是实际上编译 APK 的时候，注解是 Support 库的。由于依赖库已经以二进制方式提供，编译的时候不会再检查依赖是否存在。所以编译不会失败。

那运行时呢？会不会出现 ClassNotFoundException？可以来看看[这个讨论](https://stackoverflow.com/q/3567413/3819519)。

按照经验来讲，项目里面没有配置 `getDefaultProguardFile` 应该是比较少见的。但是探索这个问题的过程还是挺有趣的，也触摸到了自己知识的盲区。

> 弱小和无知不是生存障碍，傲慢才是。
> 

References:
