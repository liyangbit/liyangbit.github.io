---
layout: page
title: "Contact Us"
meta_title: "Contact us"
show_meta: false
teaser: "Get in touch"
header:
   image_fullwidth: "wood_plank.jpg"
permalink: "/contact/"
---

## 关于我

Hi, 我是 Lemon，大家可以通过下面途径联系我。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}


## 赞助支持


| <center>遇见，是缘</center> | <center>欢迎关注</center> |
| ---------------------------------------- | ---------------------------------------- |
| <img src="/images/wechat-pay.png" width="300"/> | <img src="/images/foot.jpg" width="300"/> |
