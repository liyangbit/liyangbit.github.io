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

{% include alert info='内容在不断更新中……' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

## 1 Python基础

### 1.1 Python入门基础

<ul>
    {% for post in site.tags.tutorials-python-basic %}
    <li>{{ post.date | date: '%Y-%m-%d' }} / <a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>

## 2 数据分析

### 2.1 Numpy

<ul>
    {% for post in site.tags.Numpy %}
    <li>{{ post.date | date: '%Y-%m-%d' }} / <a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>

### 2.2 Pandas

<ul>
    {% for post in site.tags.Pandas %}
    <li>{{ post.date | date: '%Y-%m-%d' }} / <a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>

### 2.3 Jupyter

<ul>
    {% for post in site.tags.Jupyter %}
    <li>{{ post.date | date: '%Y-%m-%d' }} / <a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>


## 3 数据可视化

### 3.1 Matplotlib

<ul>
    {% for post in site.tags.Matplotlib %}
    <li>{{ post.date | date: '%Y-%m-%d' }} / <a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>

### 3.2 Seaborn

<ul>
    {% for post in site.tags.Seaborn %}
    <li>{{ post.date | date: '%Y-%m-%d' }} / <a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>

### 3.3 Bokeh

<ul>
    {% for post in site.tags.Bokeh %}
    <li>{{ post.date | date: '%Y-%m-%d' }} / <a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>

### 3.4 Plotly

<ul>
    {% for post in site.tags.Plotly %}
    <li>{{ post.date | date: '%Y-%m-%d' }} / <a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>

## 4 机器学习

<ul>
    {% for post in site.categories.MachineLearning %}
    <li>{{ post.date | date: '%Y-%m-%d' }} / <a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>

## 5 视频创作

<ul>
    {% for post in site.tags.tutorials-manimce %}
    <li>{{ post.date | date: '%Y-%m-%d' }} / <a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
