---
title: ARTS030
date: 2021-05-25 16:55:00
tags: ARTS
---

监管的铁拳呐！是件好事！
<!--more-->
# Algorithm

鸽

# Review

[漫谈唯一设备ID](https://juejin.cn/post/6844903952148856839)

记录一下：拜占庭容错方案

[Identifying App Installations](https://android-developers.googleblog.com/2011/03/identifying-app-installations.html)

瞄了一下 Sentry 的源码，看到一个 Installation 类用来处理 AndroidId 获取异常的情况。查了一下说明。

[One on One Meeting Questions Great Managers Ask Their Teams](https://getlighthouse.com/blog/one-on-one-meeting-questions-great-managers-ask/#S1)

1 对 1 会议中的一些议题和目标

Building Rapport & Trust

Talking about Career Development

Talking about their goals is a good start, but it’s not enough on its own. People need to feel progress on them.

As a manager, it’s important to give your team feedback and coaching, and not just at review time.

Tie feedback to their goals.

refocus on positive things to help pick them up and refocus on what’s good about the situation

make sure you commit to making progress between meetings so you’re both clear on what needs to get done

As your team grows, you’ll need to develop a culture of coaching and development to help your team succeed, and create the leaders you need in your organization.

What’s essential is you then take action.

> 1) What can you do to take action or make progress on what we talked about today?
2) What can I do to take action or make progress on what we talked about today?

# Tips

排查域名的使用情况，项目中抽了很多 AAR，直接在 Android Studio 中无法搜索到所有域名。直接打包，反编译，从反编译的结果中搜索对应的资源

反编译用了 enjarify，记得是上次逆向的时候 dex2jar 的效果不太好来着。不过看着 Google 官方也不怎么维护 enjarify 了，上次更新在 N 久之前。

用 jd-gui 可以搜索和将 Jar 导出为 Java 文件

搜索功能似乎有点问题，搜出来的结果不全

导出 Java 文件在进度的某个地方卡死不动了

上次逆向工程已经很久了，今天再查资料发现基本还是几年前的那几个工具，可能是因为安全原因，资料比较少。

找到一个类似于工具合集的 ByteCodeViewer，看起来是集成了 apktool，dex2jar 等功能。procyon 的反编译 wiki 里面提到 ByteCodeViewer 包含了几个新的工具：Procyon，CFR，FernFlower。

试用了一下 ByteCodeViewer 的搜索功能还挺好用。

[Clippings.io](http://clippings.io) 免费版的限制太多了，每条标记最多 100 个字符，每个月 5 美刀又很贵。

寻找替代工具。发现 [https://github.com/paperboi/kindle2notion](https://github.com/paperboi/kindle2notion) 刚好符合我的需求。

处理 Author 的步骤好像有个 Bug，周末看看能不能提个 PR 过去。

Jenkins 的 Slave 是如何使用 Master 提供的 Credentials 的呢?

[https://github.com/jenkinsci/git-client-plugin/blob/master/src/main/java/org/jenkinsci/plugins/gitclient/CliGitAPIImpl.java](https://github.com/jenkinsci/git-client-plugin/blob/master/src/main/java/org/jenkinsci/plugins/gitclient/CliGitAPIImpl.java)

`launchCommandWithCredentials`

可以看到是读取过来写了个临时文件。

Git 读取了 `GIT_SSH` 环境变量

> If either of these environment variables is set then git fetch and git push will use the specified command instead of ssh when they need to connect to a remote system. The command-line parameters passed to the configured command are determined by the ssh variant. See ssh.variant option in git-config(1) for details.

通过 `ssh -i` 来指定 identity_file 进行连接。

所以，使用 Credentials 的 Jenkins Job，如果自己在执行的命令中写 git 相关的命令，是无法正常运行的。

# Share

如何排查 App 读取 IMEI 的行为

监管的铁拳越来越严格，很多之前被视为合理的操作现在开始变成高危操作。比如获取 IMEI 进行用户追踪。

排查这个问题最简单的办法是在 SDK 读取源码的地方打个断点，Debug 模式运行，这样一来不管是自己的代码还是第三方 SDK，偷偷摸摸读取 IMEI 的行为都会被发现。堆栈信息也是无比清晰。

但是问题在于大部分厂商都会对 Android 系统进行改动，导致使用官方的 SDK 源码进行调试时，断点无法准确的在指定的代码处定位。

Android Studio 在 Debug 的时候会使用 Compile SDK 对应的 SDK 源码进行展示，而不是匹配开发机的系统版本。也造成了断点定位困难。

那有没有什么办法不依赖 SDK 源码，又能准确的使用 Debug 模式让程序停止在指定的地方呢？

偷天换日

获取 IMEI 无非是通过 Context.getService().xxx 嘛，只要我们控制了 Context，再使用一点 Proxy 的小技巧，完全可以控制 getService 方法之后的所有流程。

[Condom](https://github.com/oasisfeng/condom) 就是通过这种方式，提供了一个 Mock 的 TelephonyService。替换了获取 IMEI 的行为实现。

为了通过编译检查，甚至还提供了 [Stub](https://github.com/oasisfeng/condom/tree/master/library/src/main/java/android) 类。学到了。

具体替换实现类为 `NullDeviceIdKit` 。有兴趣的同学可以去膜拜下大佬的思路。

有了 Mock 的实现，接下来只需要在 `NullDeviceIdKit` 中打断点，就可以让 App 乖乖听话啦。
