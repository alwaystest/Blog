---
title: ARTS133
tags: ARTS
date: 2024-08-30 14:29:57
---

be 坦诚。
忙死了忙死了忙死了，Review 别人的代码就花一整天，自己的任务只能加班搞。
梅小姐还天天抱怨我加班没生活。

烦死了！
<!-- more -->

# Algorithm

none
# Review

[198.18.0.2 是什么？ - V2EX](https://v2ex.com/t/974350)
困惑了好久的一个问题，总算是大致搞明白了。

ClashX 的策略是直接配置 DNS 地址为 TUN 所在网段。
Clash Verge Rev 的策略是配置 223.5.5.5，通过 128.0/1 的规则来进行路由优先控制。

netstat -rn 返回的规则，并不是按照顺序进行优先级匹配的，大致是按照越具体越优先的策略。
总之，开启 TUN 模式之后，配置合适的 DNS 地址还是蛮重要的。不过 Linux 似乎可以通过 iptables 直接重定向。Mac 上还是需要别的办法把 DNS 请求路由到 TUN 设备来做劫持的。

被 TUN 模式搞懵好几次了，实在是有点过于复杂了。

[mp.weixin.qq.com/s/6k0MyNApYL8yU6ZnBibIGw](https://mp.weixin.qq.com/s/6k0MyNApYL8yU6ZnBibIGw)
生产环境确实也见过这个崩溃，量级比较小就没有深挖，既然看到了就顺便排期研究一下吧。
# Tips

none
# Share

none
