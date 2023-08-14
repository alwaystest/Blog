---
title: ARTS102
tags: ARTS
date: 2023-08-10 17:06:13
---

新疆真好，下次还来！

<!--more-->

# Algorithm

none

# Review

[odhcpd](https://openwrt.org/docs/techref/odhcpd)

外网访问群晖还是有些问题，再研究一下 OpenWrt 的东西，尤其需要看看 ipv6 的各种解决方案。

[The UCI system](https://openwrt.org/docs/guide-user/base-system/uci)

很多 ipv6 开启的相关文章都需要手动修改 `/etc/config/dhcp` 文件，里面的格式和内容好像没那么容易理解，终于被翻到了。

各种文章感觉都不如官方文档来的系统和完善，以后还是先查文档吧。

[odhcpd 中继模式原理、局限以及解决方案](https://blog.icpz.dev/articles/notes/odhcpd-relay-mode-discuss/)

odhcpd 进行中继的原理说明

[Introduction to GitHub Packages - GitHub Docs](https://docs.github.com/en/packages/learn-github-packages/introduction-to-github-packages)

最近搞路由器经常发现 ghcr 这个域名，好奇心突发查了一下，发现 Github 居然提供了 Package 托管服务。提供的服务越来越多了，Github 真厉害。

[How to Import Public Certificates into Java’s Truststore from a Browser](https://medium.com/expedia-group-tech/how-to-import-public-certificates-into-javas-truststore-from-a-browser-a35e49a806dc)

运行 Java 程序时，如果需要 Debug 一下网络请求，需要单独配置 TrustStore，只把证书安装到 Mac 系统中还不够。

可以对 Java 使用 `-Djavax.net.ssl.trustStoreType=KeychainStore` 参数，把 Mac 上 KeyChain 中信任的证书加入到 Java 的 KeyStore 中。

目前经过测试，需要把 Charles 或者 ProxyMan 的证书添加到 Login KeyChain 中，System KeyChain 中的证书看起来是不会被识别的。记得把证书标记为信任。

可以用下面的代码试着读取一下 KeyStore 中信任的证书，方便验证。

```kotlin
fun main() {
    val osTrustManager = KeyStore.getInstance("KeychainStore")
    osTrustManager.load(null, null)

    val enumerator = osTrustManager.aliases()
    while (enumerator.hasMoreElements()) {
        val alias = enumerator.nextElement()
        if (osTrustManager.isCertificateEntry(alias)) {
            println(String.format("%s (certificate)\n", alias))
        }
    }
}
```

[[JDK-8303465] KeyStore of type KeychainStore, provider Apple does not show all trusted certificates - Java Bug System](https://bugs.openjdk.org/browse/JDK-8303465)

看起来需要到 JDK 21 的时候 KeyChainStore 会修复一个 Bug，到时候可以看看能不能加载到 System KeyChain 下面的证书。

# Tips

Univercal Control 在 WIFI 下经常出现卡顿，重复按键的情况，直接用数据线把 Mac Mini 和 MBA 连接起来，就稳定多了，看起来内部是会协调通信信道的。

PS：刚写完这段就开始出现卡顿了。不过试着关闭了 MBA 的 WIFI，UniversalControl 的连接也没有中断，看起来信道是数据线，不过依然不够稳定就是了。

最近为了体验 Copilot Chat，尝试使用 VS Code 做一些轻量级的开发和搜索代码，发现之前安装 SpringBoot Plugin 的时候带上了 RedHat 的 Java Plugin，每次修改完 kotlin 代码的时候就会生成一个 bin 文件夹，把 kotlin 源码复制到里面去，贼烦。

找到一个 Issue https://github.com/redhat-developer/vscode-java/issues/634，但是没时间细研究了。先禁用这些 Plugin 了事。主要是也很少开发 Spring Boot 的项目，只是偶尔客串一下。

比较正规的开发还是 Idea 比较爽一些，虽然不能用 Chat，生成代码倒是也够用了。

使用代理也会被 Bing 重定向到 CN 了。痛失 New Bing 帮我总结页面内容。

踩了一个坑，项目使用 Moshi 做 JSON 序列化，使用 Retrofit + OKHTTP 做网络请求，发送请求的时候发现对于 String 类型的 Post Body，发送给服务端的时候会自动加上双引号，导致网络请求参数和约定不符，请求一直失败。

使用 IDE Debug 的时候因为 String 类型展示的时候也会有双引号，很难发现这个微小的区别。直到上 ProxyMan 对网络请求 Debug 之后才发现。

罪魁祸首就在这里了 `com.squareup.moshi.JsonUtf8Writer#string`

Moshi 对 String 做序列化的时候会自动在前后加双引号，倒是符合 JSON 的规范。

又是被下绊子的俩小时。

# Share

none
