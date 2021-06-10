---
layout: page
sidebar: "right"
comments: true
show_meta: true
title: "About Us"
authors: ["阳哥"]
header:
   image_fullwidth: "image-head.jpg"
permalink: "/about/"
---

## 关于我

我是 阳哥，微信公众号「Python数据之道」的号主，对数据科学有浓厚的兴趣。喜欢鼓捣些东西，平时也喜欢看看书、跑跑步，大家可以通过以下途径联系我。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}
