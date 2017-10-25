---
layout: page
sidebar: "right"
show_meta: false
title: "智联求职实战"
header:
   image_fullwidth: "wood_plank.jpg"
permalink: "/zhaopin/"
folder: casestudy
---

## 项目实战：求职系列

本Python数据分析项目实战案例，首先从智联招聘爬取相关信息，然后对数据进行分析，获取有用的信息。

本项目以Python求职为例，分析全国python招聘数量Top10的城市列表以及其他相关信息，同时进行可视化显示。

“python求职Top10城市”，这篇文章，经几个大号转发，**累计阅读量过万**，推荐阅读。

### 本系列文章如下：
* python求职Top10城市，来看看是否有你所在的城市
* 5分钟掌握智联招聘网站爬取并保存到MongoDB数据库


```python
f, ax= plt.subplots(figsize = (14, 10))

sns.heatmap(corr,cmap='RdBu', linewidths = 0.05, ax = ax)

# 设置Axes的标题
ax.set_title('Correlation between features')

f.savefig('sns_style_origin.jpg', dpi=100, bbox_inches='tight')
```
