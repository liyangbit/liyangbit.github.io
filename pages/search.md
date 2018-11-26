---
layout: page
title: "站内搜索"
show_meta: true
authors: ["Lemon"]
comments: true
teaser:
sidebar: right
header:
   image_fullwidth: "image-head.jpg"
permalink: "/search/"
---


{% include alert info='搜索' %}

{% include sidebar-search.html %}

**tips:请按关键词搜索，包括：**

『站名（Python数据之道）』，『文章标题的关键词』，『分类（Categories）』，『标签（Tags）』，『其他……』


{% include alert info='推荐文章' %}
<ul>
    {% for post in site.tags.recommend %}
    <li><a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
