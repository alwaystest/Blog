---
title: about-cookies
date: 2023-01-13 10:43:01
tags:
---

比不知道更可怕的是以为自己知道。

<!--more-->

---

![AboutCookies](AboutCookies.001.jpeg)

今天跟大家分享一个 Cookie 相关的知识吧。
我把最近 Debug 过程查的一些资料整理了一下，给大家做个分享，就是今天的主题：Cookie 那些事。

![AboutCookies](AboutCookies.002.jpeg)

首先来看一下这次分享的几个话题。

![AboutCookies](AboutCookies.003.jpeg)

什么是 Cookie 呢？
在 MDN 文档中是这么描述的：
Cookie 是一小段服务端发送给用户的 Web 浏览器的数据。Web 浏览器会把这段数据保存下来，后面再次请求相同的服务端的时候，会把这段数据再次发回去。
这里的 Web 浏览器的说法有点狭隘，我们可以理解为所有使用 HTTP 协议进行通信的客户端。

很简单，就是服务端和客户端之间发来发去的一段数据。
那么 Cookie 的出现解决了什么问题呢？
我们知道，HTTP 协议是无状态的，Cookie 就提供了一个维护 “状态” 的这么一个可能性。举个例子：用户的登录态。

![AboutCookies](AboutCookies.004.jpeg)

这里列举了 Cookie 常见的三种用途。
第一个是 Session 管理：
比如登录相关的状态，购物车的内容，游戏当前的分数，等等服务端需要的一些数据。
第二个是个性化：
比如用户的一些配置，当前使用的主题，等等。
第三个是用来做用户追踪：
最常见的就是各家广告商了，还有各种网站分析，访问量统计的服务提供商，都可以利用 Cookie + Referer 的方式把用户访问轨迹关联起来。

我们可以看几个例子。

![AboutCookies](AboutCookies.005.jpeg)

以我自己的 Blog 为例，我的 Blog 基本上全部是展示静态网页，在这个基础上增加了访客统计，接入了 Google 的网站流量统计服务。
打开 Chrome 的开发者工具之后，可以看到网站正在使用的 Cookie。
其中 busuanzi 就是访客统计功能。_ga 是 GoogleAnalytics 相关的。仔细看的话，可以看到 _ga 对应的 Domain 并不是 Google，是因为现在大部分浏览器都默认禁止第三方 Cookie 来保护用户隐私了，这种情况下，使用第一方 Cookie 就可以绕过浏览器的限制，继续摸用户的屁股。

![AboutCookies](AboutCookies.006.jpeg)

那第三方是怎么获取用户访问了哪个网站的呢？
可以观察一下 busuanzi 的网络请求，可以发现 Header 中有两个关键元素。一个是 Cookie，发送了一个 ID，用来区分用户。
另一个是 Referer，代表了发起请求的网站，只需要解析出来这两个参数，就可以完成网页访问量统计的功能。
同理，也可以根据 ID 把用户访问页面的轨迹串联起来，这样，就可以根据用户浏览记录推断用户喜好，进而卖广告了。
我们也知道，Google 有相当大的收入来源是广告，那么禁用第三方 Cookie 明显会对 Google 的收入造成威胁，我们看看这个老六是怎么干的。

![AboutCookies](AboutCookies.007.jpeg)

我到 Firefox 的官网截了一张嘲讽图，可以看到，Chrome 的默认设置是不会阻止第三方 Cookie 的。
所谓免费的就是最贵的，所以 Google 有充足的动力把 Chrome 做的好用。
在移动互联网时代，用户使用 App 的情况下，Cookie 追踪就没有用武之地了，加上浏览器也在收紧对第三方 Cookie 的控制，
那么怎么继续追踪用户呢？不如做个操作系统吧？用 IMEI，AdsId 来替换 Cookie 中存储的 ID，让天下没有难做的广告。

![AboutCookies](AboutCookies.008.jpeg)

了解完 Cookie 的常见使用场景，我们再来看看 Cookie 不适合做的事情吧。

![AboutCookies](AboutCookies.009.jpeg)

浏览器中 Cookie 的存储空间是有限制的，根据 RFC 的约定，Cookie 的存储空间大小为 4K。
所以我们就清楚了，Cookie 不适合用来存储大量的数据。
相对应的，浏览器会提供适合存储大量数据的一些机制。

![AboutCookies](AboutCookies.010.jpeg)

打开开发者工具，可以看到这里的 Storage 下有这么多分类。
LocalStorage，SessionStorage，IndexedDB，WebSql。
其中  WebSql 已经废弃了。LocalStorage 和 SessionStorage 也是有存储大小限制的，SessionStorage 限制最大存储空间是 5MB，LocalStorage 更大，但是没有找到明确的说明。他们存储的都是键值对类型的数据。
IndexedDB 提供了更大的存储空间，更适合存储一些结构化的数据。
对比一下这几种方案，Cookie 和本地存储都是浏览器支持的，本地存储可能在一些古老的浏览器上不可用。
Cookie 可以通过 HTTP 请求自动发送给请求的服务器，而本地存储需要通过本地操作主动处理之后才能发送给服务器。
以我的 Blog 为例，可以看到，LocalStorage 里面存储了一个 ThemeMode，是网页 DarkMode 是否启用的标记。
我们之前提到过，像这种 Theme，个性化相关的配置放到 Cookie 中也可以，这样的话，每个网络请求都会把对应 Domain 下的 Cookie 发送给 Server，如果 Server 不需要读取这些配置的话，其实有点浪费，如果 Cookie 有很多的话，可能会劣化用户体验。比如极端情况下某个 API 的 Response Body 只有一个 Bool 值，但是 Cookie 却占了几 KB 的大小。而且 Cookie 是每个 Request 都会带上的。
像这种静态网页的 Blog，更合适的存储空间就是 LocalStorage 了。
当然 Http 2 协议中，有对这种情况的优化。在 Http 1 协议中，Header 不会被压缩。Http 2 协议中，增加了对 Header 的压缩技术，可以通过维护动态字典，把 Cookie 重复传输的负载降低。

![AboutCookies](AboutCookies.011.jpeg)

接下来我们看看 Cookie 是怎么使用的。

![AboutCookies](AboutCookies.012.jpeg)

最基本的使用就是在服务端的 Response Header 中加入 Set-Cookie，后面是对应的键值对。每个 Set-Cookie 后面只能跟一个键值对。
客户端在发送请求的时候，在 Request Header 中加入 Cookie，后面是对应的键值对，这里就可以添加一个或多个 Cookie，用分号和一个空格分割多个 Cookie。
根据 Cookie 的不同使用场景，服务端在 Set-Cookie 的时候还能添加更多的参数。

![AboutCookies](AboutCookies.013.jpeg)

可以看到 Set-Cookie 的用法中，后面可以追加更多属性。
大致就是这么几方面：
Expires 和 Max-Age 限制 Cookie 的有效时间
Domain，Path，Secure，HttpOnly，SameSite 都是用来限制 Cookie 的使用范围的
更具体的说明可以查找对应的文档去了解，客户端的使用中，相对用的比较多的就是 Domain，Expires 这两个。

![AboutCookies](AboutCookies.014.jpeg)

从刚才的使用说明中，我们能看到 Cookie 是有有效期限制的。
从 Cookie 的有效期来区分，可以分为两类。

![AboutCookies](AboutCookies.015.jpeg)

Session Cookie 和 Permanent Cookie
Session Cookie 顾名思义，有效期只存在于当前会话。一般情况下这种 Cookie 只会存放到内存中，不会持久化保存。但是这也不是绝对的，有一些浏览器也会提供 Session Restore 的功能，这样 Session Cookie 也会被持久化。
Permanent Cookie 就需要持久化保存，下次会话的时候如果没有过期，就可以继续使用。
如果 Set-Cookie 没有指定 Expires，那这个 Cookie 就会被认为是 Session Cookie。
如果同时指定了 Expires 和 Max-Age，Max-Age 会被优先使用。

![AboutCookies](AboutCookies.016.jpeg)

刚才也提到了，Cookie 也可以指定作用范围。

![AboutCookies](AboutCookies.017.jpeg)

Domain 指定 Cookie 生效的域名及子域名
Path 属性指定了 Cookie 生效的 URL 中必须包含指定的 Path
Secure 指定了生效的 Schema 必须是 Https，除非 Request 要发往 Localhost
我们可以在开发者工具里面看到 Cookie 的这些属性。

![AboutCookies](AboutCookies.018.jpeg)

HttpOnly 用来禁止 JS 访问 Cookie，保证数据的安全性。
SameSite 属性用来指定 Cookie 在跨域访问的场景下是否生效。

![AboutCookies](AboutCookies.019.jpeg)

介绍完这些基本信息后，我们再来关注一下身份验证这个场景。

![AboutCookies](AboutCookies.020.jpeg)

首先，我们了解一下 HTTP 通用鉴权流程。
客户端访问需要鉴权的 API 的时候，如果没有带上鉴权需要的信息，服务端会返回错误码 401，并且在 Response Header 中告诉客户端服务端支持的鉴权方式。
客户端收到 401 响应后，一般会让用户输入帐号密码来获取对应的授权。
接下来，客户端重新发起请求，这次请求中会带上鉴权需要的信息，服务端收到请求后，验证信息没有问题后，返回用户请求的资源。
这套通用鉴权流程中，用户的 credential 会通过 Request Header 中的 Authorization 字段传递。
不同的 Auth Scheme，鉴权需要的数据也是不一样的，我们看看几种常见的 Scheme。

![AboutCookies](AboutCookies.021.jpeg)

Scheme 有很多，我们关注一下最常见的前两种。
首先看看 Basic authentication scheme

![AboutCookies](AboutCookies.022.jpeg)

Basic 的实现非常简单。
只需要用用户名和密码以冒号分割构造一个字符串，把这个字符串进行 base64 编码，加上固定的前缀，放到 Request Header 中就可以了。
因为 base64 只是编码，并没有加密，所以这种方式是很不安全的，在信道被监听的场景下，用户的帐号密码很容易被泄漏。
JiraPythonLibrary 中有一种方式鉴权方式就是 Basic Scheme，源码稍微绕了一下，但是也很好懂。

![AboutCookies](AboutCookies.023.jpeg)

这里的前缀 Basic 就是 RFC 规定的。

![AboutCookies](AboutCookies.024.jpeg)

接下来看 Bearer Scheme
这个协议是 OAuth 2.0 规定的用来发送 Token 的。
OAuth 2.0 是用来授权第三方应用获取用户数据的，这个标准有点复杂。想要了解具体工作方式的同学可以看看阮一峰的 Blog，有几篇文章对 OAuth 2.0 的流程做了讲解。
简单描述一下：
在用户本人授权同意后，系统产生一个有效期很短的 Token，叫做 AccessToken，代替密码，供第三方使用。
这样首先保证了用户自己的密码不会被泄漏给第三方。
那为什么 AccessToken 要设置一个很短的有效期呢？这点主要是从安全方面来考虑，一旦 AccessToken 不小心泄漏，短的有效期可以缩短被攻击的窗口时间，及时止损。
那有效期缩短，第三方服务同样也受到限制，AccessToken 过期之后怎么办呢？再给用户展示一个弹窗，获取用户授权，体验太差了。
这个时候，就出现了另一个 Token，叫做 RefreshToken。这个 Token 是和 AccessToken 一起告知第三方服务的，并且 RefreshToken 的有效期会设置长一些。第三方服务在访问资源的时候，只需要带上 AccessToken 就可以了。当 AccessToken 过期，访问资源返回 401 错误，这个时候，第三方服务再把 RefreshToken 发送给系统，换取一个新的 AccessToken，然后就可以继续访问需要的资源了。
这么做的优势，RefreshToken 只有在 AccessToken 过期的时候才会被使用，降低了泄漏的可能性。
此外，还有一些策略可以用在 AccessToken 和 RefreshToken 的生成和校验上，用来优化系统性能。
比如 AccessToken 中可以存一些用户相关的非敏感信息，比如 UserId，这样 API 在设计的时候就不需要每次都需要 UserId 这个参数了。
比如 AccessToken 可以采取数字签名的方式，服务器每次验证 Token 是否有效的时候，可以直接在本地验证签名是否有效，而不用去访问 Redis 之类的存储服务，提高响应速度的同时，也可以避免单点故障，允许服务横向扩容。
比如 RefreshToken 换取 AccessToken 的服务可以独立部署，集中精力把这个服务的安全性做好，就能事半功倍。
当用户撤销授权，或者 RefreshToken 泄漏时，只需要把之前签发的 RefreshToken 撤销，等 AccessToken 过期，使用 RefreshToken 换取 AccessToken 还会失败，这个时候再弹出授权页面让用户重新授权，整个流程就又跑通了。

![AboutCookies](AboutCookies.025.jpeg)
![AboutCookies](AboutCookies.026.jpeg)
