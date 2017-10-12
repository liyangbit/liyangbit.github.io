---
layout: page
title: About
description: Python数据之道
keywords: Lemon, Lemonbit, Liyangbit, PyDataRoad
comments: true
menu: 关于
permalink: /about/
---

我是Lemon



## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}


## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

* 微信公众号

<div><img src="../images/foot.jpg" width="350"/></div>