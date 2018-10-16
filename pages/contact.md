---
layout: page
title: "Contact Us"
meta_title: "Contact us"
show_meta: true
authors: ["Lemon"]
comments: true
teaser:
sidebar: right
header:
   image_fullwidth: "image-head.jpg"
permalink: "/contact/"
---

## 关于我

Hi, 我是 Lemon，对数据科学有浓厚的兴趣，跨界玩 Python。喜欢鼓捣些东西，平时也喜欢看看书、跑跑步啥的，大家可以通过以下途径联系我，也欢迎在本站留言。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}
