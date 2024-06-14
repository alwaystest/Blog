---
title: ARTS127
tags: ARTS
date: 2024-06-14 16:23:53
---

拒绝标签化。

But 为什么好多程序员都这么痴迷二次元呢？

<!-- more -->

# Algorithm

none

# Review

[Kotlin 2.0 — Android project migration guide | by Kacper Wojciechowski | May, 2024 | Medium](https://medium.com/@kacper.wojciechowski/kotlin-2-0-android-project-migration-guide-b1234fbbff65)

Kotlin 2.0 安排!

[From init.vim to init.lua - a crash course](https://www.notonlycode.org/neovim-lua-config/)
Lua 搞起来！


# Tips

几年前没有好好看 Android Lint 的使用说明，还以为生成 baseline 文件需要手动配置 `absolutePaths false` 之后把 lint result 手动复制过去。
今天在新项目加 lint 配置，突然发现莫名其妙的生成了 baseline 文件。
原来官方早已经预判了我的预判。

```
android {
    lintOptions {
        baseline file("lint-baseline.xml")
        abortOnError false
    }
}
```

设置好 baseline 文件之后，只要对应的文件不存在，`gradlew lint`  就会生成 baseline 文件
如果需要更新，删除 baseline 文件重新运行 `gradlew lint`  即可
baselint 文件存在的情况下，再次运行 `gradlew lint`  就会检测增量问题

用 VSCode 临时看一下 iOS 的项目代码，经常会遇到需要到 Pod 文件夹中搜索代码的需求。
如果你只想临时在 `.gitignore` 文件中指定的文件夹中搜索，可以在搜索面板中进行设置：

1. **打开搜索面板**：
    - 按下 `Ctrl + Shift + F`（Windows/Linux）或 `Cmd + Shift + F`（Mac）打开搜索面板。
2. **展开搜索选项**：
    - 点击搜索面板右上角的三个点（...），展开更多搜索选项。
3. **忽略 `.gitignore` 文件**：
    - 取消勾选 `Use Ignore Files` 选项。

被文件系统的问题坑了半个下午。
开发机 Mac 上文件系统是大小写不敏感的，一台 Linux 打包机的文件系统是大小写敏感的。导致 aidl 文件编译一直报错找不到文件。
直接重命名是不行的，请教了一下 AI，分分钟搞定：
先把有问题的文件夹重命名成另外的名字，在重命名到正确的大小写名称上即可。
# Share

none
