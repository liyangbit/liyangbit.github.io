---
layout: page
show_meta: false
title: "����ѧϰ"
subheadline: "�������·��ࣺ����ѧϰ"
header:
   image_fullwidth: "header_unsplash_5.jpg"
permalink: "/MachineLearning/"
---
<ul>
    {% for post in site.categories.MachineLearning %}
    <li><a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>