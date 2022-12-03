---
title: ARTS077
tags: ARTS
date: 2022-12-03 18:53:04
---

click to see more!
<!--more-->

# Algorithm

# Review

[Field data types | Elasticsearch Guide [8.4] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html)

[Keyword type family | Elasticsearch Guide [8.4] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/keyword.html)

ElasticSearch 的字段类型。以及 Keywords 类型和 Text 类型的取舍。主要取决于将来打算怎么使用这个字段。

[Term query | Elasticsearch Guide [8.4] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-term-query.html)

初步观察 Kibana 生成的 Elastic 搜索参数，其中有个 Term 不太好理解。

对于 Text 类型的字段，使用 Term 去做检索会比较困难，推荐使用 match。

Term query 的翻译结果是词条查询，要求结果中包含可以精确匹配查询条件的词条。

如果不看一下 Elastic 的文档，直接上手使用，感觉会踩坑。所幸客户端开发用到这个工具的场景还是比较少的，偶尔来这么一两次性能比较差的查询应该问题不大。不过能对 Elastic 有更多了解的话，这把屠龙刀用起来会更顺手吧。

RuntimeFields 和 ScriptFields 的主要区别

RuntimeFields 应该是打算替代 ScriptFields，检索的结果可以参与聚合，而 ScriptFields 不行。

[Elasticsearch Runtime Fields- How to Use Them, Examples & More](https://opster.com/guides/elasticsearch/how-tos/runtime-fields/)

这篇差不多算是正儿八经的 Review，一篇介绍 RuntimeFields 如何使用的文章。

[An Introduction to Elasticsearch Mapping](https://www.elastic.co/blog/found-elasticsearch-mapping-introduction)

看 Elastic 相关的介绍的时候，经常看到两个名词 Schema on Read 和 Schema on Write。

这里说明了什么是 Schema。

> A *schema* is a description of one or more fields that describes the document type and how to handle the different fields of a document.
> 

Schema 翻译为 图式 概要。我的理解是这个东西有点像说明书，告诉 Elastic 数据应该怎么处理。

[前言](https://elkguide.elasticsearch.cn/)

中文版本的 ELK 技术指南。先简单了解一下，方便快速上手。

# Tips

用 reduce 深度遍历树还真是好用。

# Share
