---
layout: page
sidebar: "right"
comments: true
show_meta: true
authors: ["阳哥"]
title: "Manim社区版教程"
header:
   image_fullwidth: "image-head.jpg"
permalink: "/tutorials-2-manimce/"
---



## Manim社区版教程



**本系列已发布文章如下：**

<div style="background-color:#E0EAF2">

<!-- {% for entry in site.data.mycategories.entries %}
{% for mytag in entry.tags %}
{% if mytag == "tutorials-manimce" %}
<strong>{% include list-posts-link.html tag=mytag %}</strong>
{% endif %}
{% endfor %}
{% endfor %} -->

<ul>
    {% for post in site.tags.Numpy %}
    <li><a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>


</div>
