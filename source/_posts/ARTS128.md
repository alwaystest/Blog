---
title: ARTS128
tags: ARTS
date: 2024-06-14 16:25:29
---

忻州古城是个好地方！

<!-- more -->

# Algorithm

none

# Review

none

# Tips

Obsidian 启用了 Slash Commands 之后，使用起来非常不顺手。有些命令是英文的，有些是中文的，比如插入模板。
为了保持使用的一致性，在 Obsidian 的设置中切换语言为英文，这下用起来舒服多了。

又遇到了 RoomDatabase 编译不过的问题，看起来还和设备环境相关，有的设备可以编译过，换一台设备就不行。
升级 room-compiler 的版本大概率可以解决这个问题。
所有机器都编译不过的话，应该就是代码编写的问题了，数据类型确实不是 Room 支持的。
时间不是很充足，先记录一下，就不扒 Room 的源码深究了。

# Share

Rime 输入法永久了之后，用户词典中会有一些误触产生的自造词，洁癖用户看着四万条自造词，实在难以忍受。
但是 squirrel 仓库的 [Issue](https://github.com/rime/squirrel/issues/212) 中 lotem 回应了不支持手动编辑词库。
总得想个办法清理一下。

1. 先在最常用的电脑上点击同步词库，备份当前最完整的自造词记录。
2. 备份 Rime 词库同步文件夹，防止操作出现问题无法回退。
3. 在 Rime 词库同步文件夹下运行清理脚本
4. 切换输入法到非 Rime
5. 到 Rime 配置文件夹，删除 userdb 文件夹
6. 重新部署
7. sync userdata

到别的设备上，确认最新词库已经同步好。
1. 备份词库同步文件夹以防手抖搞坏
2. 切换输入法到非 Rime
3. 到 Rime 配置文件夹，删除 userdb 文件夹
4. 重新部署
5. sync userdata
6. 确认无误，删除备份

清理脚本：
```bash
#!/bin/bash

# 使用 find 命令查找当前目录及其子目录下的所有以 "userdb.txt" 结尾的文件
for file in $(find . -type f -name "*userdb.txt")
do
    # 使用 awk 命令处理文件，删除第三个字段以 "c=1 " 或 "c=0 " 开头的行
    awk -F'\t' '!($3 ~ /^c=1 / || $3 ~ /^c=0 /)' $file > temp && mv temp $file
done
```
