---
title: ARTS055
date: 2022-03-10 18:38:10
tags: ARTS
---

年后再说。

<!--more-->

# Algorithm

过

# Review

过

# Tips

`adb shell am start -a android.settings.SECURITY_SETTINGS` 

步步高的学习机在设置页面隐藏了许多选项，比如安装证书，用来调试网络请求的入口就被隐藏掉了。可以尝试通过隐式 Intent 的方式来强行调起安装的入口页面。

当然，前提是需要开启开发者模式，开发者模式需要联系官方去开通。普通用户还是不要想了。

这个 Intent 可以在 AOSP 源码中找 Manifest。可以先打开自己手机的对应页面，观察 Activity 的名字，然后顺藤摸瓜去找。

Gradle 直接提供了命令去查看 keystore 的签名信息。接入第三方 SDK 的时候会用到。就没必要用 keytool 了，整了半天还不一定能展示出 MD5

`./gradlew signingReport` 

# Share

AnimationSet 在 Android O 以上会重置动画进度的问题分析

优化了一个动画的性能。

这个动画是使用 AnimationSet 来组合几个子 View 动画，并且通过给子动画设置不同的 currentPlayTime 来实现这几个动画错开时间播放的。

优化之后突然发现子动画之间没有相位差了，原来是之前的动画性能过于低，代码中手动设置了隔几帧才更新一下页面，机缘巧合之下，动画的运行效果刚好有了一些相位差。优化完成后，取消了隔几帧才更新一下界面的效果，就导致所有的动画都展示出了相同的效果，也就是说之前设置的 `currentPlayTime` 失效了。

看 SDK 的注释发现，在 Android O 及以后，AnimationSet 在开始动画的时候会尝试重置 `currentPlayTime` ：

> // In pre-O releases, calling start() doesn't reset all the animators values to start values.
// As a result, the start of the animation is inconsistent with what setCurrentPlayTime(0) would
// look like on O. Also it is inconsistent with what reverse() does on O, as reverse would
// advance all the animations to the right beginning values for before starting to reverse.
// From O and forward, we will add an additional step of resetting the animation values (unless
// the animation was previously seeked and therefore doesn't start from the beginning).
private final boolean mShouldResetValuesAtStart;
> 

既然不能修改 currentPlayTime，那就换一个，分别设置子 Animator 的 `startDelay`，于是优化后的动画就可以完美的带相位差运行啦。
