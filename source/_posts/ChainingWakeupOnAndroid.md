---
title: ChainingWakeupOnAndroid
date: 2020-07-19 01:00:38
tags: Android
---
# Android 上的链式唤醒

> Android 苦卡顿久矣。

<!--more-->

前段时间做了一次 Android 手机上链式唤醒行为的排查，试着记录一下排查过程和思路。

先通俗易懂的讲一下概念。

什么是链式唤醒呢？

你的手机上装了很多个不同的 App，绝大多数情况下，你的需求就是打开某一个 App，使用它的功能，而你没有在使用的 App，是不应该在这个时候启动和运行的。启动和运行就需要占用资源，手机上的资源就那么多，当然要紧着你正在使用的 App 来供给，如果这个时候出现了别的 App 出来消耗资源，那供给给正在使用的 App 的资源就变少了，资源不够了，运行的 App 就可能会出现卡顿的情况。链式唤醒就是这么一个抢占资源的问题。当你打开某个应用 A，应用 A 可能会同时在后台启动应用 B，应用 C，应用 D 等等。在后台启动的意思就是你什么都没看到，什么都不知道，但应用 B，应用 C，应用 D 就已经偷偷开始消耗你手机上宝贵的资源开始运行了。

为什么会这样呢？

应用 A 自己独占所有的资源运行它不香吗？干嘛要启动别的应用来和自己抢资源？

其实应用 A 也不傻，它也不想有别的应用来抢占资源，导致自己出现卡顿，造成不好的用户体验。这一切还得从推送开始说起。

手机短信你知道吧？某天你在超市购物，发现超市在搞活动，只要登记一下姓名和手机号码，注册一下超市的会员，就可以享受 xx 折优惠。这么一填不得了了，超市拿到了你的手机号码，天天给你发短信通知你啥啥啥又特价啦，赶紧来买呀。

手机 App 也是一样的。当你装上某个 App，成为它的用户，你兜里的钱就被它盯上了。它会想方设法的告诉你我们提供了一个什么新服务，尝鲜价 xx 折，赶紧来买呀。当然，我们现在上网都是实名制了，注册帐号基本都需要手机号，既然拿到了手机号，发短信通知就可以了。但是好多软件并不是强制要求注册之后才能使用的。总得先让用户试用一下，看看好不好用再跟用户要手机号注册吧？这种试用的用户可是潜在的付费用户呀，得想办法告诉他们优惠活动。手机号没有，手机总有的吧？都装了 App 了。既然装了 App，就有办法通知到用户。这个功能就是推送。

推送的原理也很简单，手机联网了吧？应用运行的时候，可以通过网络查询是否有新的优惠信息，如果有，就可以通知用户。这种方式有个缺点，应用查询的时候可能还没有活动，但是查询完毕后新活动开始了。怎么解决呢？一遍一遍的查？效率太低了。不如这样，应用把自己的联系方式告诉发布优惠信息的地方，约定当有了新的优惠活动的时候，发布活动的人可以主动找上门来通知应用。这样的话应用只要守株待兔等通知就可以了。但是这么做有一个限制，发布活动的人来通知的时候，应用需要在运行状态。应用的运行状态就好比人活着，只有活着的时候才能处理事情，比如通知用户。应用不在运行状态的时候，不消耗资源，啥也干不了，啥也听不到，自然没有办法接收通知和告诉用户。

现在我们知道了应用需要启动，才能告诉用户有新的活动信息。那为什么应用 A 要启动应用 B 呢？

上面我们知道，应用需要守株待兔，等消息。那应用的功能其实都是程序员一点一点写出来的逻辑。没有应用是自己学会怎么等消息的，建国之后不许成精。你看现在市面上那么多应用，每个应用都有等消息的需求，难道每家公司都要一个一个重新实现一遍吗？当然不是，就像造汽车一样，应用也可以看作由若干个部件组成，其中一个部件就负责处理守株待兔等消息。那商机来了，是不是可以像生产汽车零部件一样，专门搞一个公司，来完成推送这个功能的模块，别的公司如果有需求，可以直接拿来用，就不用自己再造了。当然是可以的。所以就出现了这么一种情况，应用 A，应用 B，应用 C，应用 D 都不需要自己造推送功能的模块了，直接使用了同一家公司生产的推送模块。

上面我们也说了，应用想要接收通知，得处在运行状态。应用 A 启动之后，没问题，可以接收消息并通知用户了，但是此时，应用 B，应用 C，应用 D 并不能接收消息。生产推送模块的公司就开动脑筋了，别的公司提供的模块在应用没有运行的时候就无法接收消息，但是如果我实现了这个功能，是不是我的模块就能更好卖？说干就干，不就是应用没有运行嘛，让他们运行起来，问题不就解决了？于是这个模块就新增了功能，使用这个模块的应用，在启动的时候，会同时启动别的使用这个模块的应用。于是盛大的场景就出现了，当你启动应用 A，应用 A 就会同时启动使用了同一家推送模块的应用 B、C、D…… 大家其乐融融通知用户，我们上新啦，我们有新活动，我们打折啦……

而且有的应用还不止是用户手动打开的时候会启动，为了让自己能及时的通知用户，他们还利用了 Android 系统提供的各种机制，在开机的时候，网络情况变化的时候，让系统启动自己，为用户提供更大的“价值”。

当然，手机厂商也注意到了这些行为，因为应用频繁的启动，消耗资源，会导致手机发热，耗电快等各种问题，如果不处理这些行为，那用户可就去买别家的手机了。所以国内大部分厂商的系统都对应用的这些行为做了限制，并且提供了替代方案，就是厂商自己来做这个推送模块。手机系统是手机上的老大，由系统提供一个推送模块，可以实现不启动应用的情况下，帮应用等消息，当用户点击消息的时候，系统再启动应用。当然，需要应用使用这个模块才能行。不用这个模块的，启动行为会被限制，只能望消息兴叹。随着将来使用系统提供模块的应用越来越多，链式唤醒的行为也会越来越少。

接下来就是技术的部分啦。

作为开发者，我们知道，一个应用，其实底层应用了各种现成的轮子来实现各种各样的功能，在接入前，可能会有代码审查的步骤。对于中小型公司，很可能是缺乏这一步的，或者由于历史原因，很早之前引入的 SDK 可能带了一些有副作用的代码。我们手动去排查的话还是挺难的。那有没有什么办法来检测这些行为呢？我查了一下资料，恰逢 MIUI 12 推出了照明弹功能，我还比较幸运的找到了一篇可以来参考的比较新的文章。

[Android App 链式唤醒分析](https://androidperformance.com/2020/05/07/Android-App-Chain-Wakeup/)

文中提到了自启动的技术分析，使用 

```bash
adb logcat -b events | egrep "am_proc_died|am_proc_start" 
```

这条命令来监控进程的启动与死亡。

于是我尝试观察在应用启动的时候是否会有别的进程被连带启动，以此来判断是否存在链式启动的行为。但是这种方式有个问题，如果链式启动行为是存在的，而且被启动的应用已经在运行中了，那么是不会有新的 Process 被创建出来的，也就观察不到对应的 Log。而为了观察这种行为，ForceStop App 的话，Android 3.1 上系统已经增加了[启动限制](https://developer.android.com/about/versions/android-3.1.html#launchcontrols)，ForceStop 的应用默认不会被广播启动。这样就可能导致某些链式启动行为被漏掉。

当时也想到另一个办法，通过吃内存的办法，让系统去杀 App，此时被杀掉的 App 不会处于 ForceStop 状态，就还有可能被链式唤醒启动。我在酷安找了 **吃掉内存** 这个软件（之前也经常用来给我的 Pixel 清理内存），经过几次测试后，发现可能还是不太靠谱，某些应用拉活的速度非常快，测试的结果可能还不够准确。

用 MIUI 的照明弹功能也是个思路，但是这种方法限制在了小米的设备上，无法判断 MiPush 在别的机型上是否会有链式启动的行为。

还有一个思路是找一台 Root 的手机，安装 LBE 安全大师来监测应用唤醒的情况，目前的情况来看，这个软件似乎也不怎么维护了，我的 Nexus 6P 刷的系统似乎和 LBE 还不怎么兼容，搞不定。

那还有别的什么办法吗？

![Wakeup](wakeup.png)

按照我的理解，一个应用想要唤醒另一个应用，务必会经过系统 API，而 Android 系统提供的可以用来和别的应用程序交互的组件就四种，Activity，Broadcast，Service，ContentProvider。如果我们想监测是否存在启动别的应用的行为，那就从这四个地方下手即可。

刚好之前有看到过 oasisfeng 大佬写的 [Condom](https://github.com/oasisfeng/condom) 库。既然大佬都提供了解决办法，那自然可以顺藤摸瓜找到链式启动涉及的 API。

简单看了一下源码，实现的着实巧妙，通过 Proxy 模式把 Context 包装了一层，达到了偷梁换柱的效果，第三方使用包装过的 Context 干坏事，一抓一个准。

于是顺理成章的，我将 Condom 接入到了待排查的项目中，使用 DryRun 模式，并且在拦截点处打 Log，来观察是否有第三方 SDK 偷偷摸摸搞小动作。

有几个点需要注意一下：

- Push 服务一般会单独起一个进程来搞动作，，所以 Condom 不止要用在主进程，还需要用在别的进程中。
- 一般第三方 SDK 初始化的入口都在 Application.onCreate 中，要注意把所有第三方 SDK 的初始化 Context 都替换掉。
- 用 ContentProvider 来实现初始化的，就有点难搞了。这种我目前想到的只能扒源码了。
- 自己在自己的代码中干坏事，可能会被忽略掉，毕竟排查目标主要是第三方 SDK。
- 像分享这种不在启动的时候初始化，而是使用的时候传了 Context 的功能，也需要特殊留意一下。不过这种不会在应用启动的时候抢占资源，问题应该不大。
- 目前的链式启动在华为，小米这样的国产机上已经基本被治理的服服帖帖了，想要复现，还得是找纯净原生态的 Pixel 系列。

# 另

之前关注了一个公众号叫 真没什么逻辑。看人家分享的技术文章，那排版和配图叫一个舒适。

于是尝试用 Figma 搞了一张配图，看着挺简单的，但是前前后后也耗了我挺长时间的。

写一篇高质量的文章是真的不容易呀。每周一更的话，难度就更高了。

低头向各位大佬学习。