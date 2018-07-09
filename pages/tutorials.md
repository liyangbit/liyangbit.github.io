---
layout: page
sidebar: "right"
comments: true
show_meta: true
authors: ["Lemonbit"]
title: "Python教程"
header:
   image_fullwidth: "image-head.jpg"
permalink: "/tutorials/"
---


{% include alert info='The tutorials will be coming soon ...' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>


## 1 Python基础

### Python入门基础

**本系列已发布文章如下：**

<div style="background-color:#E0EAF2">

{% for entry in site.data.mycategories.entries %}
{% for mytag in entry.tags %}
{% if mytag == "tutorials-python-basic" %}
<strong>{% include list-posts-link.html tag=mytag %}</strong>
{% endif %}
{% endfor %}
{% endfor %}

</div>


### 正则表达式

### 函数

### 对象


## 2 数据分析库：Numpy 和 Pandas
* Numpy
* Pandas
* Other

## 3 数据可视化
* Matplotlib
* Seaborn
* Other

## 4 机器学习
* scikit-learn
