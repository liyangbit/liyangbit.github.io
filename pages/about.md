---
layout: page
sidebar: "right"
comments: true
show_meta: true
title: "About Us"
authors: ["Lemon"]
header:
   image_fullwidth: "image-head.jpg"
permalink: "/about/"
---

## 关于我

我是 Lemon，『Python数据之道』创始人，对数据科学有浓厚的兴趣。喜欢鼓捣些东西，平时也喜欢看看书、跑跑步，大家可以通过以下途径联系我。

如有需要提问的，欢迎在本文评论区留言 或者 加入我的知识星球进行交流。

提问请附上必要代码、输出等截屏。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## 知识星球

<div align="center">
    <img src="/images/xingqiu-1.png" width="500"/>
</div>