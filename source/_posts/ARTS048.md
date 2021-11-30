---
title: ARTS048
date: 2021-11-30 14:34:38
tags: ARTS
---

玩一玩 Flutter。

MBA M1 8G RAM 有点吃不消呀。

<!--more-->

# Algorithm

ignore

# Review

[研读Flutter--打包编译流程详解](https://www.jianshu.com/p/4e8ccb02e92d)

了解了一下  `flutter run` 这个命令是怎么调用到 Gradle 的。写的很详细，先粗过一遍，了解整个流程，后续有需要的话再单独拆开细细的品。

# Tips

[Allow to configure the behavior of deleting the .gradle folder if there is a connection problem · Issue #89959 · flutter/flutter](https://github.com/flutter/flutter/issues/89959)

Flutter 绝了，开发者怎么想的，直接删 `.gradle` 目录？

# Share

## Flutter 上路的一些吐槽

Flutter 在细节体验上还是差一些的。

通过 TextController 给 TextField 设置内容后，Cursor 的位置还需要自己手动管理。否则光标会默认出现在 TextField 中间，很奇怪。

SharedPreference 在 Android 端的实现只对应了一个文件，虽然存取的时候用了异步，但是从 SharedPreference 全量存取的特性来看，如果存储的内容比较多，这个文件的读写性能上会比较有压力。目前还没有进行过测试。

Build 工程的产物分别是什么东西？

看到 build/app/output 文件夹下有 apk 和 flutter-apk 两个文件夹，下面都有 Android 的安装包。这两个文件夹有什么区别？

[Flutter: What is the difference between the apk/release directory and flutter-apk directory under build/app/outputs?](https://stackoverflow.com/q/62910148/3819519)

从这个答案来看，两者是一样的东西，不过是为了兼容老版本的一些工具链，才保留了 apk 文件夹下的产物。

Option + Return 是一个非常好用的快捷键。Flutter 也知道自己的样板代码有点多呀。
