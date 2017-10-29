---
layout: page
title: "Contact Us"
meta_title: "Contact us"
show_meta: false
comments: true
teaser:
header:
   image_fullwidth: "image-head.jpg"
permalink: "/contact/"
---

## 关于我

Hi, 我是 Lemon，对数据科学有浓厚的兴趣，主要使用 Python语言来实现。对 ACCESS、MongDB、MySQL 等数据库知识以及 HTML、CSS 有过一定的了解，以前学过 Java语言、C语言。喜欢鼓捣些东西，平时也喜欢看看书、跑跑步啥的，大家可以通过以下途径联系我，也欢迎在本站留言。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}
