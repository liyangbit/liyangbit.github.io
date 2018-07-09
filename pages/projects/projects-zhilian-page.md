---
layout: page
sidebar: "right"
show_meta: true
authors: ["Lemonbit"]
title: "智联求职实战"
comments: true
header:
   image_fullwidth: "image-head.jpg"
permalink: "/projects-zhilian-page/"
---

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
