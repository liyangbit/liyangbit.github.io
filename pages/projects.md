---
layout: page
sidebar: "right"
comments: true
show_meta: true
authors: ["Lemonbit"]
title: "项目实战"
header:
   image_fullwidth: "image-head.jpg"
permalink: "/projects/"
---
{% include alert info='通过项目实战来将灵活运用学到的技能，以此提升自己的水平。' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>





## 项目实战：世界杯数据分析实战系列

本项目属于Python数据分析项目实战案例，数据来源于 Kaggle，“2018世界杯：用Python分析热门夺冠球队”这篇文章，经40+公号转载，总阅读量10W+，虽然预测结果差异很大，但过程不妨学习下。


**本系列文章如下：**

<div style="background-color:#E0EAF2">

{% for entry in site.data.mycategories.entries %}

{% for mytag in entry.tags %}
{% if mytag == "projects-world-cup" %}
<strong>{% include list-posts-link.html tag=mytag %}</strong>
{% endif %}
{% endfor %}
{% endfor %}

</div>


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


## 项目实战：求职系列

本项目属于Python数据分析项目实战案例，首先从智联招聘爬取相关信息，然后对数据进行分析，获取有用的信息。

本项目以Python求职为例，分析全国python招聘数量Top10的城市列表以及其他相关信息，同时进行可视化显示。


**本系列文章如下：**

<div style="background-color:#E0EAF2">

{% for entry in site.data.mycategories.entries %}

{% for mytag in entry.tags %}
{% if mytag == "projects-zhilian" %}
<strong>{% include list-posts-link.html tag=mytag %}</strong>
{% endif %}
{% endfor %}
{% endfor %}

</div>
