---
title: ARTS020
date: 2020-12-09 21:24:45
tags: ARTS
---

擦脸巾真好用
<!--more-->

# Algorithm

🕊️

# Review

🕊️

# Tips

Android 11 禁止软件访问别的软件在 External Storage 上的 app-specific directory。导致之前将文件放到 `getExternalFilesDir` 然后将 Path 传递给微信分享的方式行不通了。

从 Android N 就开始推行使用 FileProvider 分享文件，没想到还有这么一个尾巴没除干净。

之前大家的习惯都是直接给微信授权存储访问权限的，如果没有存储访问权限，其实这个分享功能也会出问题。权限一步步收紧，挺好的。

顺便还发现了 Android P 之前实际上是可以将应用私有文件夹的内容共享给别的 App 读取的。

> Apps that target Android 9 or higher cannot share data with other apps using world-accessible Unix permissions.

---

看到 JacocoPlugin 里面有用 Conventions 这么个东西，同时又看到别的 Plugin 在用 Extensions 来做参数传递，搞得有点迷惑了。查了一下两者的不同。

[Gradle plugin: Convention vs. Extension](https://stackoverflow.com/questions/17589206/gradle-plugin-convention-vs-extension)

---

[Usage with Jenkinsfile for pipeline? · Issue #507 · jenkinsci/ghprb-plugin](https://github.com/jenkinsci/ghprb-plugin/issues/507)

Jenkins 使用 Pipeline 构建的时候，勾选了 lightweight checkout，配合 GerritTrigger 会有问题，无法 checkout 到对应的分支。取消勾选即可。

# Share

简单看了一下 FileProvider 的相关源码，备忘一下。

发现从 Manifest 中读取信息和 ViewInflator 从 XML 解析生成 View 还挺像的。

从 `PackageParser` 中能看到从 Manifest读取信息的各个步骤。

FileProvider 初始化的时候使用的 ProviderInfo 就是从这里读取信息的。

FileProvider 继承自 ContentProvider 的，用于对外分享文件，所以在 `attachInfo` 的时候检查了 `grantUriPermissions` ，要求这个属性必须为 True。

`grantUriPermissions` 调用的结果是更新了 AMS 中存储的 URI 和对应 Permission 的状态。

ContentProvider 在执行对应的 Query 或者 Insert 的时候又会去 AMS 检查对应的权限。

FileProvider 上面设置的 `grantUriPermissions` 并不是说自动给分享出去的 URI 设置权限，而是告诉 AMS 是否允许为其他 App 授予 URI 的访问权限。

搞明白套路之后系统源码也没那么神奇嘛，感觉比两年前我翻源码的时候轻松多了，还是有进步的。
