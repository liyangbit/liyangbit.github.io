---
layout: page
sidebar: "right"
show_meta: true
authors: ["Lemonbit"]
title: "福布斯系列-Pyton数据分析项目实战"
comments: true
header:
   image_fullwidth: "image-head.jpg"
permalink: "/projects-forbes-page/"
---

## 项目实战：福布斯系列

本系列以福布斯全球上市企业排行榜为依据，用Python进行数据分析实战。项目数据从2007年至2017年，涉及10多年。

本系列包括一个基本的数据分析项目的主要流程，涉及数据采集、数据查验、数据清洗、数据分析、数据可视化等内容。目前，本系列文章正在连载中，敬请关注。

通过本项目实战，您将熟悉以下技能：
* python进行数据爬取的思路、步骤以及基本模块的使用；
* python数据处理，尤其是Pandas使用的各种技巧；
* 一些可视化的技巧（未发布，待更新）

**本系列已发布文章如下：**

<div style="background-color:#E0EAF2">

{% for entry in site.data.mycategories.entries %}
{% for mytag in entry.tags %}
{% if mytag == "projects-forbes" %}
<strong>{% include list-posts-link.html tag=mytag %}</strong>
{% endif %}
{% endfor %}
{% endfor %}

</div>
