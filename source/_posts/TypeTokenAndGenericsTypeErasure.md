---
title: TypeTokenAndGenericsTypeErasure
date: 2020-08-09 18:20:44
tags: Programming
---
# TypeToken 和泛型擦除

Java 中的泛型擦除一直是我的知识盲区。很早之前见过一篇文章，讲 Gson 为什么在反序列化 List 这样的泛型类的时候需要 new  一个 TypeToken 的匿名类出来。

刚好最近 Review 新同学代码的时候又遇到了这个问题，于是再返回来看看，算是读完深入理解 JVM 虚拟机后对相关知识点的一个复习吧。
<!--more-->

之前看过讲 Java 泛型的文章里面提到最多的一句就是

> 元数据中还是保留了泛型信息

啥是元数据？我脑袋一个要比两个大。

Signature 用来存储一个方法在字节码层面的特征签名。

所谓类型擦除只是对 Code 属性中的字节码进行擦除。

先来看看什么是 Signature。

> A signature is a string representing the generic type of a field or method, or generic type information for a class declaration.

> The Signature attribute is an optional fixed-length attribute in the attributes table of a ClassFile, field_info, or method_info structure (§4.1, §4.5, §4.6).

JVM 1.5 支持泛型参数的时候，添加了 Signature 属性，在字节码文件中记录了签名信息。

哪些地方可以生成 Signature 呢？

Class， Field，Method 这三个地方。

对着 16 进制字节码，和 javap 反编译出来的信息，确认了 Fields 的 attribute_info 中有 Signature 字段。

Method 和 Class 的 Signature 都可以在 javap 反编译的文件中找到。

搞了一个简单的泛型类

```java
class Wrapper<R> {

    var fieldr: R? = null

    fun first() {
        println("first")
    }

    fun second(r: R, s: String) {
        println("second")
    }

    fun third(list: List<String>) {
        println("third")
    }
}
```

![SignatureSecond](SignatureSecond.png)

可以看到，second 方法有一个 Signature 属性，后面的 #37 是常量池中的引用 Index。右面的注释应该是 javap 为了方便阅读把常量池中对应的值搞过来了。从这里可以看到，泛型的信息是被记录下来的。

再看一个更常用的例子，上面代码中的 third 方法，被 javap 反编译后如下

![SignatureThird](SignatureThird.png)

参数类型的 `List<String>` 被记录到了 Signature 中。

用 Kotlin 写的话，可以直接用 IDEA 的 Show Kotlin ByteCode 功能，来直接查看对应的字节码信息，不过展示的样式可能和 javap 导出的不同，但是也能看到 Signature 信息。

![SignatureThirdKotlin](SignatureThirdKotlin.png)

那现在看就比较清晰了，因为泛型擦除后，class 文件中依然有数据结构记录泛型对应的具体类型，那在代码运行的时候就可以想办法拿到了。

那 Gson 在使用的时候为什么不是这样来获取泛型信息呢？

```kotlin
fun convert() {
    Gson().fromJson<List<String>>("", listOf<String>().javaClass)
}
```

从字节码来看看答案

![SignatureList](SignatureList.png)

这里我们可以看到，在 Code 区中，泛型擦除，导致这里生成的是一个 List 对象，根本不知道有泛型这回事情。这里生成的对象也不是 Field，也不是 Class，也没有 Method，所以不会有 Signature 来记录当时用的泛型类型。既然没有记录，那运行时当然找不到啦。

而把代码修改成这种方式的时候

```kotlin
fun convert() {
    Gson().fromJson<List<String>>("", object :TypeToken<List<String>>(){}.type)
}
```

诶，出现了一个匿名内部类。既然是泛型的 Class，那就会有 Signature 生成并记录到 Class 文件中。

![SignatureTypeToken](SignatureTypeToken.png)

记录下来，那运行时就可以通过反射来读取啦。

## 题外话

用 `javap -v path/to/class` 来看 class 文件内容

但是这种方式似乎看不到 Field 的定义

学到了使用 `hexdump -C path/to/class` 来查看十六进制文件。

如果想用 Vim 的话，可以先 `vim -b path/to/class` 然后在 Vim 内执行命令 `%!xxd` 来将内容转换为可读格式。

是从这里学到的

[macOS 终端可用的 Hex 查看与编辑器](https://learnku.com/articles/22249)

Mac下有二进制查看/编辑器吗？ - itfanr的回答 - 知乎
https://www.zhihu.com/question/22281280/answer/34778466

## Refs

《深入理解 JVM 虚拟机》

[Chapter 4. The class File Format](https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-4.html#jvms-4.7.9)
