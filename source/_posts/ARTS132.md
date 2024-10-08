---
title: ARTS132
tags: ARTS
date: 2024-08-30 11:42:03
---

做减法，关注的事情太多就会失焦。
取消订阅了一波邮件通知，清爽多了。
我确实不需要同时关注那么多事情。

<!-- more -->

# Algorithm

none

# Review

none

# Tips

代码仓库从 Gerrit 迁移到 Gitlab，Review 过程中最不习惯的一点就是 Merge 的时候，如果出现代码冲突，MR 中很难独立区分出来这个改动进行重点 Review。
搜了半天都没有找到 Gitlab 或者 Github 提供了什么对应的 Review 策略。
Fork 也没有看到对应的功能。
最后还是 Google 在 Stack Overflow 上帮我找到了一个 Git 的命令参数 ` --remerge-diff` 可以满足我的需求。
虽然 Fork 支持自定义菜单来扩展 App 的能力，但是在最终的使用体验上还是差强人意，因为没有代码高亮。
CLI YYDS！

# Share

学杂了。被术语困扰的我向 AI 提了一个问题：

Q: 召回率和准确率分别是什么指标？

A: 召回率和准确率是评估分类模型性能的两个重要指标。它们通常用于机器学习、信息检索和模式识别等领域。让我们分别来看这两个指标：

召回率（Recall）：
召回率也被称为灵敏度（Sensitivity）或真阳性率（True Positive Rate）。它衡量的是模型正确识别出的正例占所有实际正例的比例。

计算公式： 召回率 = 真阳性 / (真阳性 + 假阴性)

含义：

召回率高，表示模型能够找到大部分的正例。
在某些应用中非常重要，比如疾病筛查，我们希望尽可能不漏掉任何患病的人。

准确率（Precision）：
准确率衡量的是模型预测为正例的样本中实际为正例的比例。

计算公式： 准确率 = 真阳性 / (真阳性 + 假阳性)

含义：

准确率高，表示模型预测为正例的结果中，确实为正例的比例高。
在某些应用中很重要，比如推荐系统，我们希望推荐的内容尽可能准确。
这两个指标通常存在权衡关系：

提高召回率可能会降低准确率，因为模型可能会将更多的样本预测为正例，包括一些实际为负例的样本。
提高准确率可能会降低召回率，因为模型可能会变得更加保守，只在非常确定的情况下才预测为正例。
在实际应用中，我们经常需要根据具体问题和需求来平衡这两个指标。

Q: 召回率这个名词是怎么来的呢？对于消费行业来讲，产品召回应该是一个比较负面的概念，看起来这里的召回率是一个偏正向的概念。

A: "召回率"这个术语乍一听可能会让人联想到消费行业中的产品召回，但在机器学习和信息检索领域，它的含义和来源是完全不同的。

术语的起源： "召回率"（Recall）这个术语最初来源于信息检索领域。在英语中，"recall"有"回忆"、"召回"的意思。在信息检索的背景下，它指的是系统能够"召回"（即检索出）多少相关信息。

信息检索中的应用： 假设您在图书馆的数据库中搜索某个主题的书籍。召回率高意味着系统能够找到（"召回"）大部分与该主题相关的书籍，即使可能会包含一些不太相关的书籍。

机器学习中的延伸： 这个概念后来被引入到机器学习和模式识别领域。在这里，它表示模型能够正确"召回"（识别）多少个实际的正例。

正面含义： 在机器学习中，高召回率通常是一个正面的指标，因为它表示模型能够捕获到大部分我们感兴趣的案例。这与消费品行业中的产品召回确实有很大的不同。

术语的选择： 选择"召回率"这个术语可能是因为它在英语中的"recall"既简洁又能准确传达"找回"或"识别"的意思。虽然在中文语境下可能会引起一些混淆，但它已经成为了该领域的标准术语。

其他可能的翻译： 有些场合，"召回率"也被翻译为"查全率"或"检出率"，这些翻译可能更直观地反映了该指标的含义，但使用频率不如"召回率"高。

总的来说，尽管"召回率"这个术语在消费行业中可能有负面含义，但在机器学习和信息检索领域，它是一个重要的正面指标，反映了模型或系统找回相关信息的能力。
