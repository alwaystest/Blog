---
title: ARTS090
date: 2023-03-27 18:19:25
tags: ARTS
---

Notion 的 AI 也挺好用！

<!--more-->

# Algorithm

none

# Review

none

# Tips

AI 给我解释了负向零宽断言，还是挺容易理解的：

Q: 怎么理解正则表达式中的负向零宽断言？举例子说明。

A: 正则表达式中的负向零宽断言（Negative Lookahead Assertion）指的是，在匹配某个模式时，通过在该模式前添加`(?!)`来指定一个负向断言，即匹配该模式的前提是其后面不跟着指定模式。

例如，假设我们想要匹配所有不以某个特定字符串结尾的邮箱地址，我们可以使用下面的正则表达式：

```
w+@w+.(?!invalid|spam)[a-zA-Z]{2,3}
```

在上面的表达式中，`(?!invalid|spam)`表示不允许字符串以"invalid"或者"spam"结尾。

举个例子，如果有一个邮箱地址列表如下：

```
user1@example.com, user2@spam.com, user3@example.invalid, user4@example.net
```

通过使用上述正则表达式进行匹配，能够匹配到的邮箱地址有：

```
user1@example.com  # 不以"spam"或者"invalid"结尾
user4@example.net  # 不以"spam"或者"invalid"结尾
```

而不能匹配的邮箱地址则有：

```
user2@spam.com     # 以"spam"结尾
user3@example.invalid  # 以"invalid"结尾
```

因此，通过使用负向零宽断言，我们能够更加灵活地匹配字符串，避免过度依赖复杂的正则表达式。

# Share

none
