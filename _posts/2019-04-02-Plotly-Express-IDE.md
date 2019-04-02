---
layout: posts
title: "在 Pycharm 等编辑器下使用 Python 可视化神器 Plotly Express"
teaser:
date: 2019-04-02
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PythonVisualization
tags:
   - Plotly
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---

{% include alert info='在 Pycharm 等编辑器下使用 Python 可视化神器 Plotly Express' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

前些天公众号发布了一篇很受欢迎的文章

内容是关于 Plotly Express 的使用介绍。

在上面这篇文章里，展示了 Plotly Express 在交互式可视化方面的强大功能，很多童鞋表示很赞~~

但同时也遇到一些小问题，比如，上述代码是在 Jupyter Notebook 中运行的，有不少童鞋问到在 Pycharm 中如何运行？

刚开始遇到这个问题时，我基本回复的都是 “建议参考 Plotly 来进行修改”。

但估计很多童鞋还是不清楚怎么运行，这里，我提供一种方法，大家可以在 Pycharm 、 VScode 等 IDE中运行代码， 由于 Pycharm、VSCode 我用的相对较多，只测试了这两种。 其他的 IDE，个人觉得也是差不多的，大家可以自己试试。

## 第一个示例

我们以 iris 数据集为例来演示，代码如下（.py 文件）：

```python
# Code based on Python 3.x
# _*_ coding: utf-8 _*_
# Author: "LEMON"
# Website: http://liyangbit.com
# 公众号: Python数据之道
# ID: PyDataRoad

import plotly_express as px
import plotly
import plotly.graph_objs as go
plotly.offline.init_notebook_mode(connected=True)

iris = px.data.iris()

iris_plot = px.scatter(iris, x='sepal_width', y='sepal_length',
           color='species', marginal_y='histogram',
          marginal_x='box', trendline='ols')

plotly.offline.plot(iris_plot)
```

代码运行后，结果会出现在浏览器端，效果如下：

![image](https://wx4.sinaimg.cn/large/007EIIJlgy1g1nfx2opsug30rm0gptdp.gif)

## 第二个示例

以 wind 数据集为例来演示，代码如下（.py 文件）：

```python
# Code based on Python 3.x
# _*_ coding: utf-8 _*_
# Author: "LEMON"
# Website: http://liyangbit.com
# 公众号: Python数据之道
# ID: PyDataRoad

import plotly_express as px
import plotly
plotly.offline.init_notebook_mode(connected=True)

wind = px.data.wind()
wind_plot = px.bar_polar(wind, r="value", theta="direction", color="strength", template="plotly_dark",
            color_discrete_sequence= px.colors.sequential.Plotly[-2::-1])

plotly.offline.plot(wind_plot)
```

代码运行后，结果会出现在浏览器端，图示如下：

![image](https://wx4.sinaimg.cn/large/007EIIJlgy1g1cnut0791j30p00an0wm.jpg)

看完上面的代码，是不是觉得 so easy， 赶紧动手试试吧。