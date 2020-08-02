---
title: ARTS008
date: 2020-08-03 01:38:54
tags: ARTS
---
# A.R.T.S 008
<!--more-->

# Algorithm

TODO

# Review

[Composing builds](https://docs.gradle.org/current/userguide/composite_builds.html)

Gradle 有一个 Compose Build 功能，看起来可以放到项目模块化中实践一下。看了一波官方的介绍，依然不知道怎么在 Android 项目中实践。和普通的 Substitude 有什么区别吗？

# Tips

从同事的提交学到了一个判断应用在前后台状态的方法。之前见过的都是用 `ActivityLifecycleCallbacks` 对 Activity 做计数，根据可见的 Activity 数量来判断应用是否处在前台。

原来从 AMS 也是可以获取到应用是否在前台的。

```java
public static boolean isAppForeground() {
    ActivityManager am = (ActivityManager) GlobalContext.INSTANCE.getApplication().getSystemService(Context.ACTIVITY_SERVICE);
    if (am == null) {
        return false;
    }
    List<ActivityManager.RunningAppProcessInfo> info = am.getRunningAppProcesses();
    if (info == null || info.size() == 0) {
        return false;
    }
    for (ActivityManager.RunningAppProcessInfo aInfo : info) {
        if (aInfo.importance == ActivityManager.RunningAppProcessInfo.IMPORTANCE_FOREGROUND) {
            if (aInfo.processName.equals(GlobalContext.INSTANCE.getPackageName())) {
                return true;
            }
        }
    }
    return false;
}
```

# Share

[从左耳听风学到的交流的技巧](/2020/08/03/CommunicationTips)
