---
title: ARTS036
date: 2021-08-08 15:15:01
tags: ARTS
---

先买房还是先攒钱，这似乎不是一个问题。
<!--more-->

# Algorithm

过

# Review

排查一个 iOS 的问题，这么久了一直没搞明白 iOS 的开发者证书是什么鬼限制，为啥要卡死 100 个设备/年。于是找了一下，顺便又温习了一下 Android 签名机制，这么久了，居然都没注意到 V3 可以解决 keystore 有效期的问题。不愁了。

一直好奇，这个 keystore 是我们自己生成的，密钥在自己手上，那还不是随便改？那个有效期完全没什么用啊。看了 AOSP 的源码，对比 Signature 的代码在这里 `frameworks/base/core/java/android/content/pm/Signature.java` equals 方法直接就是对 byte 数组做对比，好嘛，看起来只要开发者重新生成了证书，哪怕签名一致，只要里面的任何一个数据变了都会导致校验失败。所以使用相同的公钥私钥对重新生成 keystore 应该也是不可行的了。

[Android V3 签名方案，使用密钥转轮为签名更新做准备！](https://juejin.cn/post/6844903843361210381)

这篇文章对 Android 的签名机制的历史由来做了个说明

[Android签名与渠道包制作-V2/V3签名原理](https://blog.islinjw.cn/2021/04/07/Android%E7%AD%BE%E5%90%8D%E4%B8%8E%E6%B8%A0%E9%81%93%E5%8C%85%E5%88%B6%E4%BD%9C-V2-V3%E7%AD%BE%E5%90%8D%E5%8E%9F%E7%90%86/)

这篇文章从代码的角度分析了一下 V2 签名和 V3 签名的逻辑，并且提到了国内厂商并没有理会有效期这个东西。

看了一下 Android UI 适配的文章，比之前的理解更为深入了一些。

http://jessyan.me/autosize-introduce/

https://mp.weixin.qq.com/s/X-aL2vb4uEhqnLzU5wjc4Q

# Tips

在 m1 mba 上安装配置 Android 开发环境。遇到了一些问题。记录一下

`./gradlew --version` 可以查看运行 Gradle 时使用的 JVM 版本。

# Share

[我的买房思路](/2021/08/08/money-or-house/)
