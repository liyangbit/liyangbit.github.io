---
layout: page
sidebar: "right"
show_meta: true
authors: ["Lemonbit"]
title: "世界杯系列-Pyton数据分析项目实战"
comments: true
header:
   image_fullwidth: "image-head.jpg"
permalink: "/projects-world-cup-page/"
---


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
