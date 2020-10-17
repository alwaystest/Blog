---
title: ARTS017
date: 2020-10-17 11:45:58
tags: ARTS
---
节后第一周，脱离了常规 Routine，感觉时间都变慢了呢。

自由泳继续换气喝水中，崇礼已经开始下雪了。
<!-- more -->

# Algorithm

[Maximum Subarray - LeetCode](https://leetcode.com/problems/maximum-subarray/)

DP 专项练习。

Easy。但是为啥我想出来的 DP 思路需要用二维数组，时间复杂度还是 $O(n^2)$ 呢。

看到题干中有 $O(n)$ 的解法，才想到一个看起来不像是 DP 的解法。如果不是按照 DP 筛选出来这道题，不把思路限制到 DP 上，没准还真的是很快就做出来了呢。

# Review

[LeetCode 按照怎样的顺序来刷题比较好？-五分钟学算法](https://www.cxyxiaowu.com/10830.html)

算是记录和分享吧，看了一篇中文文章，介绍 LeetCode 刷题的正确姿势。看完之后醍醐灌顶。看来我从之前的 Random 刷题转到 DP 专项刷题是正确的，只有针对一类问题多加练习，才能更加熟练，快速的寻找到解题的思路。

算法怎么刷？无它，惟手熟尔。

[Deep dive into how pyenv actually works by leveraging the shim design pattern - MungingData](https://mungingdata.com/python/how-pyenv-works-shims/)

对 Python 开发环境不熟悉，使用 pyenv 来做版本隔离，这个前提下用 `pip install flask` 后发现直接调用 flask 提示 command not found。于是了解了一下 [`setup.py`](http://setup.py) 和 pyenv 的工作原理。

最后发现 `pip install --user flask` 之后不会将 flask command 添加到 `~/.pyenv/shims` 中，而 `pip install flask` 可以。瞅了一眼源码，发现了奥妙，原来 pyenv 根本就不检查安装给 user 的 command ：

```bash
# List basenames of executables for every Python version
list_executable_names() {
  local version file
  pyenv-versions --bare --skip-aliases | \
  while read -r version; do
    for file in "${PYENV_ROOT}/versions/${version}/bin/"*; do
      echo "${file##*/}"
    done
  done
}
```

# Tips

[How to Install an Older Brew Package](https://itnext.io/how-to-install-an-older-brew-package-add141e58d32)

用 Brew 升级了一下 pyenv，一不小心把 VIM 也升级了，最新版的 VIM 依赖了最新版的 Python 3.9，VIM 运行需要的一些 Python Module 没有装上，Brew 上的 Python3 指向的是 Python 3.8，升级 Python 也比较麻烦，就先想办法把 VIM 装回旧版吧，问题是 Brew 已经升级了 Formula，现在并没有指向旧版 VIM 的配置了，查了一下资料，发现可以用执行 Formula 的 URL 的方式来安装，但是实际运行的时候却提示不允许直接通过 Github 的 Commit URL 来安装，需要用 `brew extract` 命令将某个历史稳定版本的 rb 配置导出到一个自定义的 Tap，然后执行安装。

这个方法的缺陷是无法复用已有的 bottles，需要从本地重新编译，如果图快的话，直接进 brew-core Tap 中，checkout 出旧版的 VIM Formula 文件重新安装即可。

棒！

# Share

暂时还没想到什么可以分享的。🕊️
