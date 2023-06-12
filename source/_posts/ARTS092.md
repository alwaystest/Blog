---
title: ARTS092
tags: ARTS
date: 2023-04-18 22:44:19
---

像是拿到了一个新锤子，看什么问题都像是钉子。

<!--more-->

# Algorithm

none

# Review

none

# Tips

给 vim 配置了 github copilot 之后，设置 git 的 editor 为 vim，并且配置强制给 `gitcommit` 类型开启 copilot，就可以使用 copilot 帮忙生成 commit message 啦

当然，如果网络不是特别通畅的话，也可以配置 proxy 提高响应速度

```bash
" github_copilot {
    let g:copilot_proxy = 'localhost:7890'
    let g:copilot_filetypes = {
        \ 'gitcommit': v:true,
        \ }
" }
```

另一个方案是给 vs code 添加 copilot plugin，然后把 core editor 设置为 `code -w` ，前提是需要安装 VS Code 的 CLI。

我还没有用这种方式，感觉 VSCode 启动速度还是慢了一些。

vim 有段时间无法触发 copilot，我把 vim 的 config 全部清空之后单独重新配置了一遍 copilot 又好了，restore 之前的 vim 配置也没有问题，真是奇怪，果然卸载重装大法还是好用。

对了，我当前在使用 nvim 来代替 vim，配置了 alias vim=nvim

git 默认使用的 editor 可能是 vi，可以找 New Bing 问一下 git 默认的 editor 是什么。需要检查确认一下使用的 editor 确实是安装并启用了 github copilot 的。

Glide 设置图片加载 CrossFade 效果的时候有一个 `com.bumptech.glide.request.transition.DrawableCrossFadeFactory.Builder#setCrossFadeEnabled` 的选项，会影响 PreviousDrawable 的 Alpha 是否会改变。当 PlaceHolder 和最终加载的 Resource 形状一样的时候，这个选项无关痛痒。但是在一些特殊场景下，这个选项就比较关键了。比如加载图片的时候设置了 Placeholder 是方形的 Drawable，最终的 Target 使用了 CircleCrop 把资源裁剪成圆形，就会出现 PlaceHolder 和 Resouece 重叠展示的情况。

理论上 PlaceHolder 的形状应该和 Resource 保持一致对吧？

让 Copilot 解释 AOP 插桩的操作灵的一匹。需要 VS Code 安装 Copilot Labs Plugin。

# Share

none
