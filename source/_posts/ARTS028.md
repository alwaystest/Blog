---
title: ARTS028
date: 2021-04-17 17:05:53
tags: ARTS
---

乔迁
<!--more-->

# Algorithm

🕊️

# Review

[Google's OKR playbook: Learn more about goal setting and OKRs.](https://www.whatmatters.com/resources/google-okr-playbook/)

看过了 Measure What Matters，但是实际操作依然问题重重。

Google 的 OKR Playbook 给出了他们的操作说明，列举了一些常见的问题，值得参考一下。

另外据说头条的 OKR 执行的不错，有机会拜访一下头条的朋友，看看他们怎么做对齐和同步。

---

[Enable the roadmap | Jira Software Cloud | Atlassian Support](https://support.atlassian.com/jira-software-cloud/docs/enable-the-roadmap/)

Jira 怎么玩 Roadmap，并且和 Confluence 联动起来。

上个季度规划 OKR 的时候文档里面用 Confluence 自己的 Roadmap 宏绘制了排期计划，但是在执行的时候还需要在 Jira 建立对应的 Epics，感觉有一些重复劳动，看看能不能避免一份文档多处维护的窘境。

# Tips

发现 VPS 上的时间偏移后会出现一些问题，打算用 Crontab 来和 NTP 对时。

```bash
0 5 * * * ntpdate asia.pool.ntp.org >> /var/log/ntpdate.log
```

配置了任务后发现依然没有生效，也没有 Log 出现。

临时把任务调整到 1次/min 并且将错误日志也输出

```bash
*/1 * * * * ntpdate asia.pool.ntp.org >> /var/log/ntpdate.log 2>&1
```

才发现问题，执行的时候找不到 ntpdate 这个命令。

最简单的解决办法就是把执行的命令使用绝对路径来调用。

之后这种定时任务也一定要记得将错误日志输出到 Log，方便后面排查

最终命令

```bash
0 5 * * * ntpdate asia.pool.ntp.org >> /var/log/ntpdate.log 2>&1
```

# Share

暂无

