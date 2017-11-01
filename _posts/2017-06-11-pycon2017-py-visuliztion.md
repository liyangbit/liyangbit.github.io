---
layout: posts
title: "Pycon 2017： Python可视化库大全"
teaser:
date: 2017-06-11
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PythonVisualization
tags:
   - Matplotlib
   - Seaborn
   - Pandas
   - Bokeh
   - Plotly
   - Holoviews
   - Altair
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---

**"Pycon 2017： Python可视化库大全**


<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>


Pycon 2017： Python可视化库大全

## 前言

本文主要摘录自 pycon 2017大会的一个演讲，同时结合自己的一些理解。

pycon 2017的相关演讲主题是“The Python Visualization Landscape”。

先来一张全景图镇楼~~

![](http://oqb5ftrdh.bkt.clouddn.com/17-6-10/32911806.jpg)

看完这张图是不是有点懵？

别着急，我们一起来看看后面的阐述。

**python可视化库可以大致分为几类：**

* 基于matplotlib的可视化库
* 基于JS的可视化库
* 基于上述两者或其他组合功能的库


## 基于matplotlib的可视化库

**matplotlib是python可视化库的基础**。matplotlib库的设计参考了matlab，甚至连名称也是以“mat”开头。

**matplotlib库的一些优势：**(翻译比较别扭，英文原文也附后)

* 设计很像Matlab，容易进行转换（Designed like Matlab: switching was easy）
* 有很多渲染后端（Many rendering backends）
* 差不多可以绘制任何图（当然需要花费些努力）(Can reproduce just about any plot with a bit of effort)
* 有长时间良好运行的历史 (Well-tested, standard tool for over a decade)

**matplotlib库的一些缺点：**

* 必须要掌握API，且描述较为冗长（API is imperative & often overly berbose)
* 有时候默认的样式设计比较弱（Sometimes poor stylistic defaults）
* 对网页以及交互式绘图的支持较弱(Poor support for web/interactive graphs)
* 数据量大时经常运行较慢（Often slow for large & complicated data）

Matplotlib自2003年发布以来，使用情况还是呈现了良好的趋势：
![](http://oqb5ftrdh.bkt.clouddn.com/17-6-10/75428056.jpg)

Matplotlib部分绘图实例如下：

![](http://oqb5ftrdh.bkt.clouddn.com/17-6-10/36501140.jpg)

>关于matplotlib，微信公众号之前写过一篇关于**饼图**的介绍，有兴趣的可阅览下。



也因此，后续开发者都吸取了matplotlib库的优点，经过对matplotlib库的缺点进行改进，从而衍生出一系列的可视化库。

基于matplotlib的库概览如下：
![](http://oqb5ftrdh.bkt.clouddn.com/17-6-10/20394078.jpg)

下面介绍两个重要的基于matplotlib的第三方库：pandas以及seaborn

**Pandas**

可能我们平时使用pandas时更多的是用来进行数据分析和处理，其实pandas也提供了较为简单的API来进行图形绘制。

我个人一般是用pandas来处理数据，然后结合其他可视化库（如matplotlib，seaborn，bokeh等）来绘图图形。

Pandas部分绘图实例如下：

![](http://oqb5ftrdh.bkt.clouddn.com/17-6-10/27243863.jpg)

>微信公众号中部分文章的案例可供参考。


**seaborn**

Seaborn是基于matplotlib的Python可视化库。它提供了一个高级别接口用于图形绘制。

Seaborn在matplotlib的基础上进行了更高级的API封装，从而使得作图更加容易，在大多数情况下使用seaborn就能做出很具有吸引力的图。

>关于Searbon，微信公众号之前写过一篇关于热力图的介绍文章，有兴趣的可阅览下。



Seaborn部分绘图实例如下：
![](http://oqb5ftrdh.bkt.clouddn.com/17-6-10/33619236.jpg)

![](http://oqb5ftrdh.bkt.clouddn.com/17-6-10/70980238.jpg)


详细信息可在其官方网站查阅：

 http://seaborn.pydata.org/

## 基于JavaScript的可视化库

两个受欢迎度较高的可视化库 **bokeh** 和 **plotly**

**bokeh**

Bokeh (Bokeh.js) 是一个 Python 交互式可视化库，支持现代化Web浏览器，提供非常完美的展示功能。Bokeh 的目标是使用 D3.js样式提供优雅，简洁新颖的图形化风格，同时提供大型数据集的高性能交互功能。Boken 可以快速的创建交互式的绘图，仪表盘和数据应用。

>关于bokeh，微信公众号之前写过的文章用应用过**timeseries**图，有兴趣的可阅览下。


bokeh部分绘图实例如下：
![](http://oqb5ftrdh.bkt.clouddn.com/17-6-10/16694917.jpg)

![](http://oqb5ftrdh.bkt.clouddn.com/17-6-10/31499319.jpg)

详细信息可在其官方网站查阅：

http://bokeh.pydata.org


**plotly**

Plotly是另一个免费进行数据分析和绘制图表的APP，建立在d3.js上。 Plotly图可下载为SVG，EPS或PNG格式，并简单地导入到Illustrator或者Photoshop中。

跟bokeh类似，也可以制作一些交互式的web图。

plotly部分绘图实例如下：

![](http://oqb5ftrdh.bkt.clouddn.com/17-6-10/601051.jpg)


详细内容可以去官网参考：

https://plot.ly/python/


## 其他综合类型库

**Holoviews**

Holoviews能够让用户构建有助于可视化的数据结构，而且它包含丰富的可组合元素库，可以覆盖、嵌套和轻松定位。同时，它还支持快速数据探索。

Holoviews可以和Seaborn、pandas或者bokeh组合使用。

由于笔者还没具体了解过Holoviews的使用功能，这里将英文的描述直接放在下面，以免个人理解不准确而产生误导。


关于Holoviews的效果图，可参考下面动态图，建议观看原视频，效果会好点。

![](http://oqb5ftrdh.bkt.clouddn.com/17-6-10/25242968.jpg)

**Altair**

Altairs起源于D3，但代码编写量要比D3简单。这里有两幅对比的图如下：

* 用D3绘制的柱状图

![](http://oqb5ftrdh.bkt.clouddn.com/17-6-10/91261829.jpg)

* 用Altairs绘制的柱状图

![](http://oqb5ftrdh.bkt.clouddn.com/17-6-10/38550555.jpg)



Altair 是 Python 高级声明式可视化库。Altair 提供一个 Python API 在声明式 manner 中构建静态统计可视化库。

什么是声明式可视化库呢，不管是翻译成中文的名称还是看英文的名称，我都一脸懵圈。幸好，有英文的进一步描述，如下：
![](http://oqb5ftrdh.bkt.clouddn.com/17-6-10/28829091.jpg)


总的来说，Altairs让开发者们更专注于数据及他们之间的关系，而不是一些不重要的细节。

**最后**，再放上这张Python可视化的全景图，对于个人而言，不一定能全部熟悉使用，但是能熟练用好其中的一部分，实际应用于工作及项目中，也就OK了。

![](http://oqb5ftrdh.bkt.clouddn.com/17-6-10/32911806.jpg)

**关于视频**
pycon 2017的相关演讲主题的视频在youtube上可观看，由于墙的缘故，部分童鞋可能不能观看视频。

python可视化这个视频，我已经下载下来，需要的童鞋可在微信公众号“Python数据之道”后台回复关键字获取视频，关键字如下：

“link01”（不含引号）

{% include alert text='对我的文章感兴趣的朋友，可以关注我的微信公众号"Python数据之道"，接收我的更新通知。' %}


<div align="center"><img src="/images/qrcode.jpg" width="200"/></div>
