---
title: ARTS039
date: 2021-09-24 14:35:19
tags: ARTS
---

M1 上手。

<!--more-->

# Algorithm

过

# Review

[漫画：Git 中的的数据结构和算法设计](https://mp.weixin.qq.com/s/QnWwd7TC8pBa_I8MFNwO6w)

之前看到的讲 Git 原理特别清晰的一篇，Mark 下

[Kotlin Jetpack 实战: 图解协程原理 | 开发者说·DTalk](https://mp.weixin.qq.com/s/qXjUFWTiUoLs9TON-SJD7w)

之前看到的讲 Coroutine 原理特别清晰的一篇，Mark 下

[How to resolve java.lang.NoClassDefFoundError: javax/xml/bind/JAXBException](https://stackoverflow.com/questions/43574426/how-to-resolve-java-lang-noclassdeffounderror-javax-xml-bind-jaxbexception)

在 M1 机器上配置 Flutter 环境，发现一个 ClassNotFound 的问题。找了一下相关资料，原来是 JDK 规划中的改变，我安装了 zulu 11，导致 Java EE 的 API 无法调用了。这篇回答解释的特别清楚，Mark

# Tips

最近升级 Android Compile SDK 到 29.

发现之前可以编译通过的 Kotlin 代码现在报错无法编译了

出错的代码是这样的：

```kotlin
arguments?.getParcelable(xxx) as Uri // arguments 是个 Bundle 对象
```

错误提示如下：

> Type inference failed. Please try to specify type arguments explicitly.

很奇怪，仅仅是简单的改一下 CompileSDK 的版本不应该影响 Kotlin 的类型推断吧？

点击关联到源码，发现 Androd 28 和 Android 29 的源码并没有什么区别。这也太奇怪了。

于是探究了一下原因。

首先需要有一个背景知识，Source 只是一个配置好关联关系的 Jar 包，实际上，并没有强制关系让这个 Source 一定能对应上包含参与编译的 Class 的 Jar 包。

于是打开 `platforms/android-x/android.jar` 查看对应的 class 是什么样子。这里使用 JD-GUI 来进行反编译。

一看就看到不一样的地方了。

```java
// android 29 的 class 长这样
@Nullable
public Serializable getSerializable(@Nullable String key) {
  throw new RuntimeException("Stub!");
}

// android 28 的 class 长这样
@RecentlyNullable
public Serializable getSerializable(@RecentlyNullable String key) {
  throw new RuntimeException("Stub!");
}
```

而 sources 下的源码 jar 包给的是这样的定义

```java
@Nullable
public <T extends Parcelable> T getParcelable(@Nullable String key) {
    unparcel();
    Object o = mMap.get(key);
    if (o == null) {
        return null;
    }
    try {
        return (T) o;
    } catch (ClassCastException e) {
        typeWarning(key, o, "Parcelable", e);
        return null;
    }
}
```

那么看起来唯一可能导致出问题的就是这里了。 `Nullable` 和 `RecentlyNullable` 的区别。

写一段类似的代码吧

```java
// Java
public class TypeInference {
    public <T extends Integer> T getValue() {
        return (T) Integer.valueOf(1);
    }
}

// Kotlin
fun getValue_giveType_compliant() {
    val value = TypeInference().getValue() as Int
}
```

这种情况下， Kotlin 可以通过 as 来推断 `value` 的类型是 Int，代码可以编译。

```java
// Java
public class TypeInference {
    @Nullable
    public <T extends Integer> T getValue() {
        return (T) Integer.valueOf(1);
    }
}

// Kotlin
fun getValue_giveType_notCompliant() {
    val unableToInference = TypeInference().getValue() // 无法编译
    val value = TypeInference().getValue() as Int // 无法编译
    val compliant1 = TypeInference().getValue() as? Int
    val compliant2 = TypeInference().getValue() as Int?
}
```

增加了 `Nullable` 注解之后，Kotlin 获取到的方法签名是这样的，注意 unableToInference 那行给的错误提示：

> Type inference failed: Not enough information to infer parameter T in
fun <T : Int!> getValue( ): T?
Please specify it explicitly.

当我们继续给出 `as Int` 的时候，我猜测是由于我们给出的类型和 Kotlin 根据方法签名推断出来的不同（注意 Kotlin 的 Int 和 Int? 是两种不同的声明），有冲突的情况下，Kotlin 不知道应该选择哪种，所以依然提示错误，无法通过编译。

所以再进一步，我们给出 `as? Int` 或 `as Int?` 的时候，给出的类型信息和推断的 Nullable 一致，就可以通过编译了。

那么什么是 `RencentlyNonNull` 呢？

查了一下

> Normally, nullability contract violations in Kotlin result in compilation errors. But to ensure the newly annotated APIs are compatible with your existing code, we are using an internal mechanism provided by the Kotlin compiler team to mark the APIs as recently annotated. Recently annotated APIs will result only in warnings instead of errors from the Kotlin compiler. You will need to use Kotlin 1.2.60 or later.
Our plan is to have newly added nullability annotations produce warnings only, and increase the severity level to errors starting in the following year's Android SDK. The goal is to provide you with sufficient time to update your code.

— [https://android-developers.googleblog.com/2018/08/android-pie-sdk-is-now-more-kotlin.html](https://android-developers.googleblog.com/2018/08/android-pie-sdk-is-now-more-kotlin.html)

# Share

## 移动端开发 M1 芯片上车指南

在上车之前，你需要了解 [M1 芯片是啥](https://zh.wikipedia.org/wiki/Apple_M1)

什么是 [Arm 架构](https://zh.wikipedia.org/wiki/ARM%E6%9E%B6%E6%A7%8B)，和传统的 [x86](https://zh.wikipedia.org/wiki/X86) 架构有什么区别

为了保持向前兼容，苹果提供了一个叫 [Rosetta](https://support.apple.com/zh-cn/HT211861) 的软件。

然后我们再来谈谈 M1 上车踩过的坑

### JDK 需要使用支持 arm 架构的

截止当前，openjdk 只提供了一个 preview 版本的 openjdk 17 可以在 arm 架构上运行。

所幸 Azul 提供了 Zulu JDK，支持 Arm 架构。

最新版本的 JDK 已经把版本刷到了 16，但是我还是建议先安装 Java 8，避免更高版本的特性导致一些[奇奇怪怪的问题](https://stackoverflow.com/a/43574427/3819519)。

### protoc 编译报错

> Could not find protoc-osx-aarch_64.exe (com.google.protobuf:protoc:2.5.0).

将声明修改一下

```groovy
protoc {
 artifact = 'com.google.protobuf:protoc:2.5.0'
}
// 修改为
protoc {
    artifact = 'com.google.protobuf:protoc:2.5.0:osx-x86_64'
}
```

当然，这个改动不适合提交到代码仓库，可以设置一个环境变量，替代写死的配置，使用起来兼容性更好一些。
