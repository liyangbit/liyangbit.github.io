---
layout: page
title: "博客文章标签 (Tags)"
meta_title: "Tag"
show_meta: true
sidebar: right
authors: ["Lemon"]
teaser:
header:
   image_fullwidth: "image-head.jpg"
permalink: "/tags/"
---

<section class="container posts-content">
{% assign sorted_tags = site.tags | sort %}
{% for tag in sorted_tags %}
<h3>{{ tag | first }}</h3>
<ol class="posts-list" id="{{ tag[0] }}">
{% for post in tag.last %}
<li class="posts-list-item">
<span class="posts-list-meta">{{ post.date | date:"%Y-%m-%d" }}</span>
<a class="posts-list-name" href="{{ post.url }}">{{ post.title }}</a>
</li>
{% endfor %}
</ol>
{% endfor %}
</section>
