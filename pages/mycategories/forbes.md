---
layout: page
show_meta: false
title: "福布斯项目实战-Python数据分析"
subheadline: "博客文章分类"
header:
   image_fullwidth: "image-head.jpg"
permalink: "/forbes/"
---
<ul>
    {% for post in site.categories.forbes %}
    <li><a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
