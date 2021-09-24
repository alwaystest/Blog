---
title: ARTS040
date: 2021-09-24 14:39:20
tags: ARTS
---

内存问题真是个大难题

<!--more-->

# Algorithm

过

# Review

[AOP技术在APP开发中的多场景实践](https://mp.weixin.qq.com/s/yWZew3XefM6Rl0glD-UVXg)

AOP 的几个使用场景，Mark 下。看起来大家都在使用 AOP 来做隐私方法，字段合规检测。

线程使用数量优化的场景之前确实没有想过，不过应该不是什么新的使用场景。最近刚好要治理一下 App 场景的线程使用，预计会用到这个思路。

[快速缓解 32 位 Android 环境下虚拟内存地址空间不足的"黑科技"](https://mp.weixin.qq.com/s/ZbFQQ5AVsHy0YL_kKMAiYA)

微信大佬写的一个优化内存占用的思路，启发很大。有很多方案看起来是我们这边可以落地试用的。

两个可以快速用起来的点

- 上线 64 位支持
- 手动设置线程栈空间

别的操作看起来风险还是比较大的，先看看上这两个方案能否对线上 OOM 问题的解决有帮助。

# Tips

[Equals method for data class in Kotlin](https://stackoverflow.com/a/37524561/3819519)

Review 代码发现同事对 kotlin data class 重写了 equals 方法和 hashCode 方法，挺奇怪的，问了一下原因，原来是数组的对比存在一些问题，看来 data class 也不是万能的呀。

[What's the point in adding a new line to the end of a file?](https://unix.stackexchange.com/a/18789/305653)

CheckStyle 有一条规则是文件最后要留一行，之前找到过说明还分享给了同事，新同学又问这个问题，突然找不到记录了，Mark 一下。

[JCommander](https://jcommander.org/#_syntax)

升级 Detekt，翻源码发现这个库，可以方便的搞 CLI 工具。Mark 下。

其中有一个 `@ syntax` ，记得 `curl` 之前也用过类似的方法来上传文件，看起来是个通用的约定。

Detekt 有一个 Bug，导致 ktlint 扫描到的 Issue 无法定位到具体的文件位置，生成 baseline 会直接禁用掉对应规则的检测。试着升级了一波。

升级之后，出现了成吨的 Indentation 检测告警，发现是对应的 ktlint 版本有 Bug，看到最新的几个版本修复了多个相关问题，就尝试把 detekt-formatting 依赖的 ktlint 版本强制升一下，结果发现 Gradle 的 denpendencies 能看到依赖升级，但是代码检查中却不起作用。查了好长时间，发现 detekt-formatting 直接把 ktlint 的代码也打包到自己的 Jar 包中了，这样 cli 就可以直接依赖 detekt-formatting 的 jar 包，实现独立运行。刚好近期 detekt 升级了一波依赖，使用了最新的 ktlint 版本 v0.42.1，但是没有发布 release，使用 gradle substitude + jitpack 终于用上了最新版的检测，却发现 Indentation 检测依然有不少问题，只能先禁用这一条规则，先把别的规则用起来了。

发现 github 把主分支命名成 main 了。政治正确真是喵喵喵。

[Kotlin: why do I need to initialize a var with custom getter?](https://stackoverflow.com/a/41441530/3819519)

Review 的时候发现的一个问题，明明设置了 setter 和 getter，应该是不需要再给 property 赋值了，赋值也没用啊。getter 又不会读。搞的代码可读性挺差的，这个类初始化的时候会调用 setter 吗？如果不好好看看 kotlin 的底层实现，都不一定能猜出来下次获取到的是个什么值。

```kotlin
var bool: Boolean = false
        get() = sharedPreference.get(key, defaultValue)
        set(value) {
            field = value
            sharedPreference.put(key, value)
        }
```

# Share

过
