---
title: ARTS052
date: 2022-01-04 21:06:28
tags: ARTS
---

视频剪辑真是太难了。

想要做出一个好的视频，节奏需要足够紧凑。视频当中要呈现什么内容，应该是在拍摄之前就想好的。视频的字幕，贴纸，音乐的风格整体上要基本保持一致。

Insta 360 的剪辑功能只能说满足了基本的要求，导出结果也不够稳定，有的视频明明预览的时候镜头切换和音乐节奏是配合的很好的，导出之后就会出现偏差。视频倍速，深度跟踪的时间轴调节只支持缩小，不支持扩大。更改镜头朝向的操作上手难度还比较高。剪辑没有备份功能，对视频做修改的时候会提心吊胆，回退编辑操作还崩过一次。目前感觉比较适合用来确定好镜头的方向然后导出视频。

片头和片尾的字幕编辑用了剪映，在剪辑视频的体验上还是比较友好的，不愧是做短视频起家的。贴纸和字幕的风格得仔细挑选，感觉大部分都土土的。

<!--more-->

# Algorithm

Skip

# Review

[Arrays in query params](https://medium.com/raml-api/arrays-in-query-params-33189628fa68)

API 请求的时候参数传了一个数组，之前没有关注过数组在 URL 中是怎么表现的，查了一下，原来还有这么多种方式。

Retrofit 的 RequestFactory 和 ParameterHandler 可以看到相关的处理方式。

`@Query` 的注释也有说明 Retrofit 是怎么处理数组类型参数的。

挺有意思。

# Tips

使用 ViewModel 的场景下，经常会有很多订阅 LiveData 的模板代码。

可以这么写，提高一些代码的复用率。

```swift
fun toastObserver() = Observer<Event<String>> { event ->
    event.getContentIfNotHandled()?.let {
        ToastUtils.toast(it)
    }
}
```

关于 Event 的定义及用法，见[此处](https://medium.com/androiddevelopers/livedata-with-snackbar-navigation-and-other-events-the-singleliveevent-case-ac2622673150)

---

`android:duplicateParentState="true"`

可以使用这个属性，设置 TextView 的 textColor 为 Drawable，就可以通过 Active 等各种状态改变 View 的展示效果了。

# Share

Skip
