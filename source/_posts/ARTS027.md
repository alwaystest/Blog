---
title: ARTS027
date: 2021-04-17 16:58:02
tags: ARTS
---

念念不忘，必有回响。
<!--more-->

# Algorithm

鸽一下

# Review

[Bash 脚本 set 命令教程](http://www.ruanyifeng.com/blog/2017/11/bash-set.html)

不经常写 Bash 脚本，set 命令的作用经常会忘记。

`set -e` 之类的命令忘记干啥使的可以到这里翻一翻。

不知道为啥 `man set` 看不到具体的命令说明，直接给了一个 Builtin 页面。

# Tips

[Where to view man pages for Bash builtin commands?](https://stackoverflow.com/questions/22991942/where-to-view-man-pages-for-bash-builtin-commands)

续上周遇到的问题，man 命令查不到 set 的用法

原来 Bash 里面还有 help 这个命令

ZSH 里面还有 zshbuiltins，可以直接用 man 去查

---

[Difference between Gradle's terms evaluation and execution](https://stackoverflow.com/questions/16070567/difference-between-gradles-terms-evaluation-and-execution)

找到了一个为所有 subproject 配置 testCoverageEnabled 字段的时机。官方说明在此

[Build Lifecycle](https://docs.gradle.org/current/userguide/build_lifecycle.html#sec:project_evaluation)

> Project evaluation
> You can receive a notification immediately before and after a project is evaluated. This can be used to do things like performing additional configuration once all the definitions in a build script have been applied, or for some custom logging or profiling.

```groovy
allprojects {
    afterEvaluate { project ->
        if (project.hasTests) {
            println "Adding test task to $project"
            project.task('test') {
                doLast {
                    println "Running tests for $project"
                }
            }
        }
    }
}
```

---

遇到一个感觉像是 Python 的 Bug。

有一个脚本会遍历文件夹下的文件，通过 `os.listdir` 得到的值和手动 `ls` 看到的不一样，很神奇。一点点打 Log 才发现这个问题。

解决方式也很简单，删除那个有问题的目录，重新拷贝一份回来即可。

这个问题发生在打包机的 Slave 上，可能是 branch 切换导致的文件残留。

---

[How to debug Gradle Plugins with IntelliJ](https://medium.com/grandcentrix/how-to-debug-gradle-plugins-with-intellij-eef2ef681a7b)

如何 Debug Gradle Plugin。

# Share

Gradle Maven 上传遇到一个代理相关的问题。表现是开启 Clash 的代理后，Gradle 上传产物到 Maven，会遇到 `failed to respond` 的问题。

起先通过 Charles 和 WireShark 查看对应的 HTTP 请求，都没有发现问题，所有的请求都是正常返回的。

看起来只能通过看源码的方式来找问题了。Maven-Publish Plugin 似乎没有版本，是随 Gradle 打包的。

开启 Gradle 的 debug 日志，发现一条异常日志

`Connection can be kept alive for 4000 MILLISECONDS`

看 Gradle 的源码，正常的话，在 Response 中需要有 `Keep-Alive: timeout=4` 才会出现这样的连接复用日志，但是在 Charles 和 WireShark 中都没有找到服务端有这样的 Header 返回。

怀疑是 Gradle 依赖的 HttpClient 在设置了代理的时候对 Response 动了什么手脚。

下了 Gradle 的源码，发现 `OutputEventListenerBackedLoggerContext` 这里关闭了 HttpClient 相关的一部分日志，自己编译出一份打开 Log 的 Gradle 查找原因。果然在 Response 中发现了对应的字段。但是依然没有找到类似于 Interceptor 一样的在 Header 中添加 `Keep-Alive: timeout=4` 这样的字段的实现。

唯一的发现是在 Request Header 中添加了 Proxy-Connection 字段。

```
/**
* This protocol interceptor is responsible for adding {@code Connection}
* or {@code Proxy-Connection} headers to the outgoing requests, which
* is essential for managing persistence of {@code HTTP/1.0} connections.
*
* @since 4.0
*/
```

`RequestClientConnControl` 

添加字段为 `Proxy-Connection: Keep-Alive`

无奈只能想办法通过 Debug 来找问题。开启 Gradle 的 Debug 后，找问题一下子变得迅捷起来，发现 Header 是直接从 Stream 中读出来的，那问题方向就十分明显了。Charles 只记录到了 [localhost](http://localhost) 到 Server 的 API，并没有记录到 Gradle 到 Clash 之间的请求内容。那用 curl 模拟一下请求看看。

`curl -x socks5://127.0.0.1:7891 -v [http://xxx](http://xxx)` 发现 Response 里面并没有 Keep-Alive 时间的设置。

几经周折，才想起来，会不会是代理方式设置有问题，于是换了 HTTP 代理试试。

`curl -x [http://127.0.0.1:7890](http://127.0.0.1:7890/) -v http://xxx`

```
> Host: xxxx
> User-Agent: curl/7.64.1
> Accept: */*
> Proxy-Connection: Keep-Alive
>
< HTTP/1.1 200 OK
< Content-Length: 958
< Accept-Ranges: bytes
< Connection: keep-alive
< Content-Type: application/xml
< Date: Tue, 06 Apr 2021 07:58:04 GMT
< Etag: "{SHA1{xxxx}}"
< Keep-Alive: timeout=4
< Last-Modified: Tue, 06 Apr 2021 07:34:22 GMT
< Proxy-Connection: keep-alive
< Server: Tengine/2.x.x
< X-Content-Type-Options: nosniff
< X-Frame-Options: SAMEORIGIN
```

将代理替换成 V2ray 后，发现没有这个问题了，可以将问题缩小到 Clash 的实现上。

此时，用手机开启一个 Clash 服务，设置允许局域网连接，使用 WireShark 抓包查看 Gradle 和 Clash 之间的通信情况。可以发现，Clash 的 Http 代理模式，在返回给 Gradle 响应之后，TCP 层直接发送了一个 FIN 准备断开连接了，导致 Gradle 的 HttpClient 复用和 Clash 之间的连接的时候 TCP 层发生错误 RST。应用层表现就是 `failed to respond` 。


解决办法：

1. 可以在 `~/.gradle/gradle.properties` 文件中配置代理白名单

    ```
    systemProp.http.nonProxyHosts=maven.yourdomain.com|*.aliyun.com
    ```

2. 使用 V2ray
3. 自己编译 Clash 看看发生了什么问题，提个 PR 😄️

后记：

其实可以通过监听 Loopback 来看 Gradle 和 Clash 之间的交互来着，没必要到手机上启动一个 Clash 了。

---

## 另一个看到的东西

看到 Gradle 的日志实现方式，这个类有点意思 `StaticLoggerBinder` 。

包名是`package org.slf4j.impl;` ，slf4j 是个挺有意思的实现方式。
