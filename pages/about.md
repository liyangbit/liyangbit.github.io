---
layout: page
title: About
description: Python数据之道
keywords: Lemon, Lemonbit, Liyangbit, PyDataRoad
comments: true
menu: 关于
permalink: /about/
---

## 关于我

Hi, 我是 Lemon，大家可以通过下面途径联系我。



<!-- ## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %} -->


## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}


## 赞助支持


| <center>遇见，是缘</center> | <center>欢迎关注</center> |
| ---------------------------------------- | ---------------------------------------- |
| <img src="/images/wechat-pay.png" width="300"/> | <img src="/images/foot.jpg" width="300"/> |
