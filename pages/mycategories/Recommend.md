---
layout: page
show_meta: true
sidebar: right
authors: ["Lemon"]
title: "推荐文章"
subheadline: "博客文章分类"
header:
   image_fullwidth: "image-article-head.jpg"
permalink: "/Recommend/"
---


<ul>
    {% for post in site.tags.recommend %}
    <li>{{ post.date | date: '%Y-%m-%d' }} - <a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}"><strong>{{ post.title }}</strong></a></li>
    {% endfor %}
</ul>


