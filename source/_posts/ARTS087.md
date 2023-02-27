---
title: ARTS087
tags: ARTS
date: 2023-02-27 16:10:41
---

试着模仿一下马伯庸或者急诊室李医生的文风，尝试把日常处理报障，排查问题的过程写成一个个有意思的小故事。

第一篇在此。

<!--more-->

# Algorithm

# Review

[Linux 日志切割神器 logrotate 原理介绍和配置详解](https://wsgzao.github.io/post/logrotate/)

看一些日志相关的东西，突然想起来 logrotate 是怎么实现两个程序并行修改同一个文件的呢？原来是需要操作系统底层支持，并且对应的程序要响应 HUP 信号，对于不支持响应 HUP 信号的，也有一个 copytruncate 方式。

UNIX 环境高级编程 还是得看看，绕不过去。

# Tips

[https://developer.android.com/guide/topics/manifest/application-element](https://developer.android.com/guide/topics/manifest/application-element)

Telegram 的清理数据按钮点击之后跳转到了一个自定义的 Activity，才发现 Android

居然还提供了这样一个定制的口子。可以在 AndroidManifest 中设置 `android:manageSpaceActivity` 来实现，有意思。

[Crontab.guru - The cron schedule expression editor](https://crontab.guru/)

还不会配置 Crontab 吗？发现一个好用的工具。

[GitHub Checkout Action恢复文件修改时间](https://finisky.github.io/githubactiontorestorefilemtime/)

使用 CI 更新 Hexo 网站的时候总是自动更新文章的 UpdateTime，之前一直以为是 DB 没有缓存的问题。今天才发现 DB 不管这个。还是得从文件系统上下手。

# Share

又到了发版的日子，前几次发版进行的都比较顺利，希望今天也不例外，早点完成线上回测，发布灰度版本之后就可以下班回家了。

天不遂人愿，刚开完早会，屁股还没坐热，企业微信里就发出急促的告警：崩啦崩啦！开发同学赶紧来看看是怎么回事！

唉，谁叫咱就是干这个的呢，当初代码写的不严谨，给自己埋的雷，总有一天会蹦出来吓自己一跳，幸好不是上线之后才出的问题，只要在测试环境，一切都还有救。

摇了摇头，把刚拿到的手冲咖啡放到一边，让我来看看是哪个 Bug 在捣鬼。

```
NullPointerException
Attempt to invoke virtual method `int java.lang.String.hashCode()` on a null object reference
```

空指针，看起来问题不大，属于常见一种 Java 崩溃。一般是调用某个对象的方法时，这个对象是 null 导致的崩溃。大部分情况只需要在调用之前判断一下对象是否为 null 就好了。Kotlin 的话更简单，语法糖直接包装起来： `object?.invoke()` 就完事了。

且慢，这个崩溃出现的地方怎么有点奇怪？

```
at com.xxx.xxx.hashCode
at ...
at ...
```

居然是在某个对象的 hashCode 方法里面崩溃了。

我们知道 Java 中 hashCode 是 Object 类中的一个方法，如果不手动重写的话应该是不会有这种问题的。

在 Kotlin 里面有一个特殊的类型，叫 data class，对于这种类，Kotlin  编译器会自动生成 hashCode 方法。

去确认一下这个出问题的类型，果然是 data calss。比较奇怪的是，这个类是很久之前写的，近期并没有什么更新，上一个线上版本使用也是完全正常的，没有出现这个崩溃，那是什么原因导致的这个问题呢？

首先，这个类的使用场景是从服务端取一些数据。对 API 的 Response 进行反序列化之后，会生成这个类的对象，以供业务场景使用。

使用 Charles 查看对应 API 的响应，发现果然有一个字段传了 null 值。会是这个原因导致的吗？

这里插一个背景，之前我们为了开发方便，和服务端同学约定 Response 中的 String 类型字段，不要传 null 值，这样在使用 Java 语言进行开发的时候，就不用对 String 类型做 null 值检测，可以少写一点点代码。但是后来发现，光靠约定是不靠谱的，不知道这条约定的服务端同学开发的新 API 依然可能传 null 值过来，有时候对旧的业务进行改动，也有可能导致某些字段出现 null 值，这样很容易搞出来线上事故。

那有没有自动一些的方式，比如让服务端对序列化框架进行配置，过滤值为 null 的类型呢？这个技术方案是可行的，但是这样的改动影响范围会比较大，有些业务场景下，String 类型为 null 值可能还有特殊的业务含义，不能一刀切。服务端来搞这个改动，似乎有点积重难返。

那就从反序列化上下手吧，我们使用 GSON 进行数据反序列化，查阅了官方文档，发现并没有提供一个配置项可以实现我们的需求。

还是车同学灵机一动，翻阅了 GSON 的源码，发现有一个 Primitives.isPrimitive 方法，是 GSON 反序列化过程中判断是否要特殊处理 null 值的。只要在这里 hack 一下， 让 GSON 认为 String 类型也是 Primitives 类型，如果字段值为 null，就不会对这个字段赋值，覆盖掉这个字段的默认值。

那么这个时候问题就简单了，我们在声明 POJO 的时候，直接给 String 类型字段赋默认值为空字符串。对 GSON 进行 hack，避免 null 值覆盖掉 String 类型的默认值，问题就解决了。绝大部分业务代码中，我们都可以认为 String 类型对象不需要特殊判断 null 值。

再回来这个崩溃。这里为什么又出现了 String 相关的 NPE 呢？理论上服务端传了 null 值也不会影响我们的逻辑呀。

回想起来上次也出现过类似的问题，当时的原因是隐式依赖升级了 GSON 库，导致对 GSON 的 hack 手段失效，难道这次又是这个问题？不太像啊，上次出问题之后，我就对这个情况新加了单元测试，这两天没有报告单元测试失败呀。

这个时候，和服务端沟通的姜同学也来回报，服务端近期没有对这个字段的赋值进行修改，并且查看线上的情况，这个字段也有大量为 null 值的。那看来可以排除服务端的因素了。

有内鬼！

看来问题还是出在这个类上。直接看类的 Kotlin 定义是看不到生成的 hashCode 方法的，既然方法出在 hashCode 上，那就一定要看看这个 hashCode 是怎么实现的。幸好 Android Studio 提供了一个好工具。只需要 Show Kotlin Byte Code，就可以看到 Kolin 编译之后生成的 JVM 字节码了。最终执行的就是这里的逻辑，所以这里有没有可能出现 NPE，一看就知。

奇怪的是，从 Byte Code 层面看不出来有什么问题，对每个 String 类型的字段都有非空判断。这可真是邪了门了。

这个时候姜同学提了一个想法，有没有可能是 IDE 的 Kotlin  版本不一样导致的？

确实有这个可能。把出问题的安装包拖到 IDE 中直接查看字节码，发现果然和 Show Kotlin Byte Code 展示的不一致。data class 生成的 hashCode 方法中，对一个 String 类型的字段没有进行判空就调用了 hashCode 方法，那这里出现 NPE 就情有可原了。

再看看出问题的这个 data class 的声明。出问题的那个 String 字段没有设置默认值为空字符串，所以 GSON 反序列化之后，这个字段的值依然为 null。

那线上版本的字节码应该是做了判空处理的。下载去看字节码，果然如此。

那么问题明显就出在了生成 hashCode 方法的流程中，有什么能影响到这个方法的生成逻辑呢？当然是升级 Kotlin 版本啦。

没错，我们的主项目升级 Kotlin 版本到 1.7.0+ 已经有一段时间了，升级之后，并没有遇到什么兼容性的问题。于是，我们新建的 AAR 项目的 Kotlin 版本也默认升级到了和主项目一致的版本。

然而 Kotlin 有个什么限制呢？如果当前项目依赖的库有使用更高版本的 Kotlin 的，编译器会报错，这也很正常，新版本万一添加了什么黑科技，低版本编译器不一定认识，尽早报错是个好选择。于是，一个新建的 AAR 库就会牵扯一系列上层的库升级 Kotlin 到新版。

主项目升级已经在线上跑了一段时间了，但是主项目编译的时候不会重新生成依赖库的字节码文件，所以主项目升级成功并不代表依赖库升级不会有问题。

所以一通排查下来，最后发现是这个库新增依赖库之后升级了 Kotlin 版本。新版 Kotlin 在生成 hashCode 方法的时候有一个优化，如果某个 String 类型的字段声明的类型是不可空的，那在使用这个字段的时候也就没有必要进行判空了。确实，对一个声明为非空类型的字段判空有点多此一举，优化掉多余的操作是应该的。恰巧我们的代码就埋了这么一个雷，非空字段没有给默认值，服务端又传了个 null 过来，天雷勾动地火，App 就崩了。

BTW，Kotlin 的这个优化是 1.5.0 版本引入的。并不是事故现场 1.7.10。

最后，再给一个例子来看看模拟的事故现场。

Kotlin 代码如下：

```kotlin
data class Model(val name: String)
```

在 Github 的 [Kotlin Repo](https://github.com/JetBrains/kotlin) 中找到对应的 Release 1.4.32 和 1.5.0，下载即可得到对应的 Kotlin 编译器。

使用 `javap -c` 对生成的 Class 文件反编译，得到字节码指令如下：

```kotlin
public int hashCode(); // kotlin 1.4.32
    Code:
       0: aload_0
       1: getfield      #11                 // Field name:Ljava/lang/String;
       4: dup
       5: ifnull        14
       8: invokevirtual #52                 // Method java/lang/Object.hashCode:()I
      11: goto          16
      14: pop
      15: iconst_0
      16: ireturn

public int hashCode(); // kotlin 1.5.0
    Code:
       0: aload_0
       1: getfield      #21                 // Field name:Ljava/lang/String;
       4: invokevirtual #55                 // Method java/lang/String.hashCode:()I
       7: ireturn
```

果然精简了许多。

啥时候试试 Moshi 和 kotlin.serialization?
