---
layout: posts
title: "Plotly中4种文本类型设置详解"
teaser:
date: 2020-04-27
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PythonVisualization
tags:    
   - Plotly  
comments: true
show_meta: true
sidebar: right
authors: ["阳哥"]
---

{% include alert info='Plotly中4种文本类型设置详解' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# Plotly中4种文本类型设置详解

大家好，我是阳哥 。

在过去的一段时间里，我写了几篇用 Python 的交互式可视化工具 Plotly 来演示全球疫情情况的文章，如下：

有不少同学都问到图表中文本如何设置，今天阳哥来跟大家分享下。

Plotly 中文本设置大概可以分为四种类型，效果如下：

![img1](/images/posts/2020-04-27-plotly-txt/1.jpg)

下面将以 Plotly Express 中的设置为基础，来介绍这 4种类型的详细情况。

## 项目环境背景

本文的数据来自开源项目 tushare， 从 tushare 中获取数据，首先要进行注册获取 token（一串字母和数字组成的文本），然后才可以获取到数据，大家可以通过以下链接来注册（也可以点击文末的“阅读原文”）：

[https://tushare.pro/register?reg=129033](https://tushare.pro/register?reg=129033)

本文的代码是在 Jupyter notebook 环境中运行，Python 环境版本及其他工具如下：

- Python 3.7
- pandas version: 1.0.3
- tushare version: 1.2.51
- plotly version: 4.5.0

首先导入相关工具：

```python
import tushare as ts
import plotly
from plotly.offline import iplot,plot, init_notebook_mode
import plotly_express as px
import plotly.graph_objs as go
import pandas as pd
import plotly.io as pio

pio.templates.default = 'plotly_white'
init_notebook_mode()
```

## 数据获取

在 tushare 中注册后，通过 “个人主页”——“接口TOKEN” 可以找到自己的 token 值，界面如下：

![img2](/images/posts/2020-04-27-plotly-txt/2.png)

复制 token 值，然后在代码中进行如下设置：

```python
# 设置 token
# tushare 注册地址： https://tushare.pro/register?reg=129033
# 以上方法只需要在第一次或者token失效后调用，完成调取tushare数据凭证的设置，正常情况下不需要重复设置。
ts.set_token('你的token值')

pro = ts.pro_api()
```

在设置好 token 值后，我们就可以开始获取数据，这里以获取股票代码是 “000001.SZ” 的企业为例，来描述 Plotly 中文本设置。

数据获取及初步整理如下：

```python
df = pro.daily(ts_code='000001.SZ',
               start_date='19900101',
               end_date = '20200425'
              )
# 设置时间格式
df['trade_date'] = pd.to_datetime(df['trade_date'])
df
```

## Plotly 中文本设置

这次，我们以 Plotly express 为例，来阐述文本的设置，在原生的 plotly 中，文本设置的方式也是一致的。

文本的设置分为 4中类型，分别是 `title`，`text`，`label`，以及 `annotations` 。

`title` 是图表的标题； `text` 是显示x轴或y轴数据的值；`label` 是设置数据列在图表中的显示名称；`annotations` 是设置图表中的注释文本。

这4种类型的文本，前3种相对而言比较好设置， `annotations` 要稍微复杂些。

下面以柱状图为例来进行说明，代码如下：

```python
df_bar = df[:10]
# df_bar

fig_bar = px.bar(df_bar,
                 x='vol',
                 y='trade_date',
                 orientation='h',
                 text='vol',     # 设置柱状图柱子上的显示文本
                 title='volumn', # 设置图表标题
                 width=600,
                 height=800,
#                  template='plotly_white',
                 labels={'vol':'交易量',      # 设置标签显示名称
                         'trade_date':'日期',
                        }
                )

# 设置文字注释内容
annotation = [dict(x=0.8,
                   xref='paper',  #使用相对坐标
                   y=0.98,
                   yref='paper',
                   text='Python数据之道',
                   showarrow=False,  # 不显示箭头
                   
                   ),
             ]

fig_bar.update_layout(annotations= annotation)

fig_bar.show()
```

效果如下：

![img3](/images/posts/2020-04-27-plotly-txt/3.png)

上图中，对 4种类型的文本进行了初步设置，当然你可能觉得有点丑，想更个性化的设置，比如 标题（title）居中、设置标题文本颜色，等等， 类似效果如下：

![img4](/images/posts/2020-04-27-plotly-txt/4.png)

上面这些效果在 Plotly 中也是可以实现的，大家可以自己研究下，或者在公众号 **「Python数据之道」** 后台回复数字 「662」获取完整的代码来查看。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
