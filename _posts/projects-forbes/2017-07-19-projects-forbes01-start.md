---
layout: posts
title: "福布斯系列(1)-数据分析思路篇"
teaser:
date: 2017-07-19
header:
   image_fullwidth: "image-head.jpg"
categories:
  - projects
tags:
  - Pandas
  - Requests
  - BeautifulSoup
  - projects-forbes
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---

福布斯每年都会发布福布斯全球上市企业2000强排行榜（Forbes Global 2000），这个排行榜每年发布的时候，国内外总有新闻会热闹的讨论一番，但很少见到比较全面的分析。

因此才有了这样一个想法，搜集近些年每年发布的排行榜，做一个进一步的分析。

在准备做这个小小的项目前，先理了一下整个思路，大概可以分为下面这几个步骤：
1. 数据采集
1. 原始数据完整性检查
1. 数据清洗、整理
1. 从不同角度对数据进行分析
1. 数据可视化
1. 总结


整个分析过程会涉及多篇文章，主要使用Python来进行分析。

**数据采集** 主要涉及的python库包括 requests，BeautifulSoup，csv，以及一些其他常用工具。

**数据完整性检查**，包括不同数据来源的对比，以及其他一些常识性的知识。需要对比数据量的多少是否完整，以及有些数据是否缺失。

当然，在拿到数据的初期，其实只能做一个初步的判断，有些内容是在整个分析过程中发现的。

**数据清洗与整理**，主要用到Pandas、Numpy以及其他常用库和函数。由于数据比较杂乱，数据清洗与整理涉及的内容比较多，可以说是整个福布斯系列的重点之一。

前文的初步整理2016年数据，也是整个数据清理与整理内容的一部分。

同时，这个也印证了通常我们所说的数据清洗与整理可能占整个分析的50~80%。

**数据分析与可视化**，经常是伴随在一起的。主要根据不同分析目的进行分析与可视化。用到的工具包括Pandas、Numpy、Matplotlib、Seaborn以及其他一些相关库。

希望能通过福布斯系列的实战来对数据分析的知识点与工具作一个简单的示例整理与分享。

对我的文章感兴趣的朋友，可以关注我的微信公众号"Python数据之道"，接收我的更新通知.......

<div align="center"><img src="/images/qrcode.jpg" width="200"/></div>

## 相关文章


{% for entry in site.data.mycategories.entries %}
{% for mytag in entry.tags %}

{% for tag in page.tags %}
{% if mytag == tag %}
{% include list-posts.html tag=mytag %}
{% endif %}
{% endfor %}

{% endfor %}
{% endfor %}
