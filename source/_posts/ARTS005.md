---
title: ARTS005
date: 2020-07-06 01:06:44
tags: ARTS
---
# A.R.T.S 005

# Algorithm

TODO:
> lannister always pays his debts

# Review

[Time to upgrade your monitor](https://tonsky.me/blog/monitors/?nsukey=QLUi2Zk9GuSXx5M26%2FfeaQlEU1J3zCdWTm8INzA%2BA%2BlNiyUeFePHAaBJqg%2BeDjxeEa3BX3A3IPBrw%2B8dz2QJcm4tLQ04y%2FcdgexnmP9YwMbqALrImTZ221VlzUOAvtV1JgPLmz400RhjXxkAcc91VEDlW74jK%2FQfDsjhxh6KiWdbHTHbgzZpg0kS5c6f8kbLYd2zv4WqksKFmZkoNjh7XQ%3D%3D)
少数派上看到了翻译的文章，刚好有点好奇为什么 Mac 在 2K 的显示器上展示的效果不尽如人意，看完了原文，关闭了 Mac 的 Font Smooth，嗯，世界又美好了。

# Tips

VIM 执行替换命令的时候可以使用函数表达式来书写替换内容
比如我把 Keynote 导出为 jpeg 后，想在 markdown 中批量插入所有图片的链接，比如

```
![NewBull2020](NewBull2020.001.jpeg)
![NewBull2020](NewBull2020.002.jpeg)
![NewBull2020](NewBull2020.003.jpeg)
```
就用到了这个命令
我先把 001 复制了 50+ 次，然后分别替换其中的数字，让其递增。匆忙中没找到合适的方法，只能用行号强行关联。
`:%s/01/\=line(".")-5`
这里我第 7 行应该展示图片 002，所以出现了 -5 的操作。

---

# Share

[NewBull2020 -- View/Layout](/2020/07/06/NewBull2020)
