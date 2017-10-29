---
layout: page
title: "博客文章分类 (Categories)"
meta_title: "Categories"
show_meta: true
sidebar: right
authors: ["Lemon"]
teaser:
header:
   image_fullwidth: "image-head.jpg"
permalink: "/categories/"
---



<section class="container posts-content">
{% assign sorted_categories = site.categories | sort %}
{% for category in sorted_categories %}
<h3>{{ category | first }}</h3>
<ol class="posts-list" id="{{ category[0] }}">
{% for post in category.last %}
<li class="posts-list-item">
<span class="posts-list-meta">{{ post.date | date:"%Y-%m-%d" }}</span>
<a class="posts-list-name" href="{{ post.url }}">{{ post.title }}</a>
</li>
{% endfor %}
</ol>
{% endfor %}
</section>
