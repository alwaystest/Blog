---
title: ARTS080
tags: ARTS
date: 2022-12-03 18:54:29
---

升级需谨慎，TypeScript 救大命。

<!--more-->

# Algorithm

# Review

[[hexo]如何生成一篇新的post](https://oakland.github.io/2016/05/02/hexo-%E5%A6%82%E4%BD%95%E7%94%9F%E6%88%90%E4%B8%80%E7%AF%87%E6%96%B0%E7%9A%84post/)

收藏一下 Hexo 的说明。之前都是使用的默认 Post，基本不知道 draft 和 page 的用法，这里解释的很清楚啦。

# Tips

VMRuntime 提供了一个方法可以判断 Process 是否运行在 64 位状态下。

摸了一下 X5 SDK 的源码，发现他居然可以判断当前 Apk 是 arm 还是 arm64 包，从而下载对应架构的 core，好奇了一下他是怎么判断的。

Robolectric 也提供了对应的 Shadow，挺有意思的。

Android 6.0 开始提供了对应的 API，不需要再通过反射调用 VMRuntime 了。

[How to know a process of an app is 32-bit or 64-bit programmatically in Android lollipop?](https://stackoverflow.com/a/67883588/3819519)

# Share

年中的时候迷迷糊糊的升级了 Hexo 版本，但是对于具体有什么 Change 没有做具体确认，直到几个月前偶然发现博客中的图片全部 404 了，但是本地生成的文件却没有这个问题，对 Hexo 源码的探索之旅由此开启。

对比 CI 自动部署的产物和本地生成的产物发现，是图片的 src 属性出了问题，CI 自动部署的版本，src 多了一个 `/` 前缀。但是本地生成的却没有。很自然的，我把关注点放到了执行环境上。

把本地环境的 Node 版本，NPM 版本都和 CI 环境统一，运行结果依然是不一致的。

那么问题是出在操作系统上？本地是 MacOs，CI 环境是 Ubuntu。那么在本地用 Docker 启动一个 Ubuntu 镜像，安装好对应的 Node 和 NPM，再执行一遍看看。结果发现确实是 Ubuntu 上的产物的路径会多一个前缀。

其实这个时候应该用二分法回溯提交历史，看看是从哪个版本开始出问题的，根据 ChangeLog 来判断问题出在哪里的。但是我没有，我已经钻进了牛角尖，认为是环境的问题，导致获取路径结果不同了。

于是顺着 Hexo 的 Generate 开始翻代码，可能是因为动态语言的特性，VsCode 提供的代码导航并不能精准的找到所有的调用，给翻阅代码带来了很大的不便。最大的原因还是对 Node 不熟悉，没关系，看多了就熟悉了。

**I am so vegetable.jpg**

总之，在像无头苍蝇一样在 nodule_modules 黑洞中乱撞了半天之后，锁定了一个叫 `hexo-render-marked` 的 Plugin。查过开发者文档后发现，可以有一个配置来设置 render 之后的图片路径，但是在 `_config.yml` 中添加对应配置后发现产物并没有变化。搜索 Github 无果，没有别人遇到类似的问题。

此时，应该想办法 Debug 了，代码实在太过于复杂，这种情况下，跟着代码的执行逻辑，该加日志加日志，该打断点打断点。先搞清楚代码是怎么个执行步骤再定位问题原因会更容易一些。

在 `render.js` 中生成 img 标签的地方打断点，开始 Debug，发现程序并没有执行到，这就奇了怪了，难道我分析错了？

这下只能从 Hexo 源码开始跟了。

大致流程是这样的。`Hexo.init()` 进行初始化：

1. Load Internal Plugins
2. Load Config: _config.yml
3. Load Theme Config: start from hexo 5.0.0
4. Load External Plugins

之后 `Hexo generate` 执行的逻辑在 `plugins/console/generate.js` 中

1. Load Source And Theme
2. Generate File

问题的关键就在 Load Source 的那步。文件是 `lib/box/index.js` 。

`process` 方法中有一个 Cache 的检查逻辑，如果 Source 在 Cache 中已经存在，就会跳过处理。

看 `hexo/index.js` 中的定义，Cache 的实现是一个叫 warehouse 的 database，存储文件为 `db.json` 。

至此，真相大白。

并非 Node 或者 NPM 存在 Bug。也不是 Plugins 有问题。更不是运行环境的问题。

本地机器在旧版本的时候就运行过 generate，本地的 db.json 中已经存储了生成的结果。每次删除 public 文件夹之后生成，也不过是从 db.json 中读取文件内容写出到 File。并没有走 render 的逻辑。所以就会导致本地机器每次 generate 都没有复现问题。巧的是为了节省鸡肋的 MBA 的硬盘空间，我用来试验的 Docker 容器启动到了 Nas 机器上，Repo 是重新 Clone 的，db.json 在 gitignore 中指定了不进入 VCS。所以 Docker 容器 generate 会重新生成目标文件，这些文件就是有问题的。

为什么会有问题呢？

`hexo-render-marked` 升级到 5.0.0 后，有一个 prependRoot 配置项默认改成了 true。而我的 Hexo 配置中 root 就是 `/` 。

我是怎么发现故障原因的呢？其实也不是 Debug 定位到的，代码实在复杂，动态语言又没有地方直观的看某个 Model 都有哪些属性。在看到 warehouse 的代码的时候，为了搞明白 db.json 起什么作用，删除了这个文件，后续生成的文件就正确的应用了新增的 hexo-render-marked 配置了。当时我还在疑惑为啥 Debug 过程中生成的文件是对的，最终的产物为啥就不对了呢。

所以，升级需谨慎，TypeScript 救大命。
