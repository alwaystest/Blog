---
title: ARTS062
date: 2022-05-03 16:35:26
tags: ARTS
---

折腾一把输入法。

<!--more-->

# A.R.T.S 062

# Algorithm

ignore

# Review

[Rime_collections/Rime_description.md at master · LEOYoon-Tsaw/Rime_collections](https://github.com/LEOYoon-Tsaw/Rime_collections/blob/master/Rime_description.md)

Rime 的 Scheme 方案字段介绍，Cheat Sheet 🙂️

[](https://scomper.me/gtd/-shu-xu-guan-de-diao-jiao-bi-ji)

[我的 Rime 配置 2022](https://dvel.me/posts/my-rime-setting-2022/)

看了一下别人的配置方案，打算自己搞自动化

[Understanding and Implementing FastCGI Proxying in Nginx | DigitalOcean](https://www.digitalocean.com/community/tutorials/understanding-and-implementing-fastcgi-proxying-in-nginx)

Nginx 配置 FastCGI 访问 PHP-FPM 的说明，很详细，各种字段都解释的很通透，适合小白上手。

# Tips

好久没有折腾输入法了，近期想使用 Lua 给 Rime 输入法增加网络词库功能，粗略的又翻了一下 Rime 的配置说明。

```yaml
"含列表的設定項/@next": 在列表最後一個元素之後插入新的設定值（不建議在補靪中使用）
```

发现很久之前配置过的 translator patch 不生效了，经过试验，发现是这个语法定义的插入设定值只有最后一个能生效，看到文档说明也不建议在补丁中使用，干脆换了另一个推荐的写法

```yaml
"engine/translators/+":
    - table_translator@english
    - lua_translator@date_translator
    - lua_translator@time_translator
```

# Share

[基于统计语言模型的拼音输入法](https://byvoid.com/zhs/blog/slm_based_pinyin_ime/)

Rime 提供了一个基于统计语言模型来优化输入的方案，在 Github 上提供的使用说明比较简略，上手还是挺有难度的，趁着刚配置成功，来记录一下。统计语言模型的概念参见上文。

部署之后的优势大概就是可以在输入长文字的时候可以给出更加准确的结果，提升输入效率。

鼠须管在 0.12.0 版本加入了 https://github.com/lotem/librime-octagram 插件的支持，所以关于插件的部分，我们不需要再进行改动了。

[【鼠鬚管】更新日誌](https://rime.im/release/squirrel/)

我目前安装的是鼠须管 0.15.2 版本，所以 Plugin 的支持就已经是自带。

1. 下载配置
    
    直接把 https://github.com/lotem/rime-octagram-data 的 grammary.yaml 下载到 Rime 的配置文件夹下，然后到 https://github.com/lotem/rime-octagram-data 的 hant 分支，下载语言模型，同样放到 Rime 的配置文件夹下。
    
2. 修改输入法配置
    
    修改自己使用的输入方案的配置文件，比如我使用小鹤双拼方案，就修改 `double_pinyin_flypy.custom.yaml` 文件，在 Patch 下加入配置，具体格式见 [修改配置文件](https://github.com/lotem/rime-octagram-data#%E4%BF%AE%E6%94%B9%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
    
    如果之前有别的 Patch，直接加入 __include 那一行即可
    
3. 重新部署 Rime
4. 测试是否生效
    
    输入 `各個國家有各個國家的國歌` 的拼音，我这边测试在应用统计语言模型之前，输出结果是 `各个国家有各个国家的郭哥` ，如果部署成功的话，就可以直接输出正确的结果啦。
    

值得注意的地方：

> 若詞典是簡化字的，採用補丁 `grammar:/hans`；
若輸入方案以打單字爲主，即依通常習慣會將詞組分成單字輸入，則使用補丁 `grammar:/hant_char` 或 `grammar:/hans_char`。
> 

grammary.yaml 里面提供了几个不同的方案，和词典类型以及输入习惯有关，

还看到另一个语言模型 https://github.com/lotem/rime-octagram-data-s1 ，根据文件的命名方式，比较适合字典是繁体，习惯单字输入的用户。

比如 https://github.com/tumuyan/rime-melt 就提供了一个简体词库，就应该使用 hans 的语言模型。
