---
layout: posts
title: "Plotly中绘制三种经典的股票交易图表（含视频讲解）"
teaser:
date: 2021-01-20
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

{% include alert info='Plotly中绘制三种经典的股票交易图表（含视频讲解）' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# Plotly中绘制三种经典的股票交易图表（含视频讲解）

大家好，我是 阳哥 。

## 背景

前一段时间， 阳哥 发了一期视频，分享了 Plotly 和 Dash 在投资领域的应用案例。

[视频：Plotly 和 Dash 在投资领域的应用](https://mp.weixin.qq.com/s/VopPrpe_64j5za4AYkmUTg)

看完视频后，有同学提了一个问题：

股票价格曲线，带可调节的时间条的图怎么绘制？

今天 阳哥 来详细的分享下，这类图如何绘制，一共会讲解 3 类图形，分别是 面积曲线图、蜡烛图、OHLC图。 这三种类型的图在投资中会经常遇到。

阳哥 录制了一个视频，来说明通过本文绘制的图表效果：

[Plotly中绘制三种经典的股票交易图表](https://mp.weixin.qq.com/s/1v3h5-5fNVk7cBdaRKzJkg)


## 数据来源

本文的数据来自开源项目 tushare， 从 tushare 中获取数据，首先要进行注册获取 token（一串字母和数字组成的文本），然后才可以获取到数据，大家可以通过以下链接来注册：

[tushare注册链接](https://tushare.pro/register?reg=129033)

<!-- 本文的代码是在 Jupyter notebook 环境中运行的。

首先导入相关工具：

```python
import pandas as pd
import datetime as dt
import pandas_datareader.data as web
import plotly.express as px
import plotly.graph_objects as go
import tushare as ts
# from plotly.offline import iplot, init_notebook_mode,plot
from plotly.offline import plot
``` -->

### 数据获取

在 `tushare` 中注册后，通过 “个人主页”——“接口TOKEN” 可以找到自己的 token 值，界面如下：

<!-- ![tushare](images/posts/2020-04-27-plotly-txt/2.png) -->

![tushare](https://tva1.sinaimg.cn/large/008eGmZEgy1gmwqwnfvdsj30l60b8aar.jpg)

复制 token 值，然后在代码中进行如下设置：

```python
# 设置 token
# tushare 注册地址： https://tushare.pro/register?reg=129033
# 以上方法只需要在第一次或者token失效后调用，完成调取tushare数据凭证的设置，正常情况下不需要重复设置。
ts.set_token('你的token值')

pro = ts.pro_api()
```

在设置好 token 值后，我们就可以开始获取数据，这里以获取沪深300指数为例，来演示三种类型的图形绘制。

数据获取及初步整理如下：

```python
# plotly-fin-candlestick.ipynb
# 获取沪深300指数行情数据， asset='I', I 表示 index，即指数

# d_today = dt.date.today().strftime('%Y%m%d')

df_300 = ts.pro_bar(
    ts_code='000300.SH', 
    asset='I', 
    start_date='20180101', 
#     end_date=d_today,
    end_date='20210119'
)

# 设置时间格式
df_300['trade_date'] = pd.to_datetime(df_300['trade_date'])
df_300 = df_300.set_index('trade_date')

df_300
```

## 面积曲线图

在常见的股票软件时，查看分时图，大部分是使用的面积曲线图，这类图形是展示股票数据常用的类型之一。

### 默认的面积曲线图

在 Plotly 中，可以使用 plotly express 的 `area` 图来绘制面积曲线图。

代码如下：

```python
# 面积曲线图

area_chart = px.area(df_300['close'], title = '沪深300')

area_chart.update_xaxes(title_text = '日期')
area_chart.update_yaxes(
    title_text = '沪深300收盘价', 
    # tickprefix = '$'
    )
area_chart.update_layout(showlegend = False)

# Pycharm，VS Code， Spider 等模式下
# Jupyter Notebook 下也可以用
# plot(area_chart,filename='tmp/hushen300-area.html')

# 在 Jupyter Notebook 中，直接使用 show()  来查看结果
area_chart.show()
```

绘制效果如下：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gmw9awbf49j31ik0skwi3.jpg)

这里说明下：

- `show()` 的作用是使绘制的图形在 Jupyter notebook 中显示；
- 如果想在浏览器中使用，可以使用代码 `plot(area_chart,filename='tmp/hushen300-area.html')` ，该代码可以在 Jupyter notebook 、PyCharm、VS Code 等编辑器中使用。

### 对面积曲线图进行个性化修改

对于上面的面积曲线图，我们也可以对其进行一些个性化的修改，比如标题居中、添加可以调节的时间栏、设置y轴数值范围等。

具体代码如下：

```python
# 面积曲线图，个性化修改

y_min = df_300['close'].min()  
y_max = df_300['close'].max()

c_area = px.area(df_300['close'], title = '沪深300')

c_area.update_xaxes(
    title_text = '日期',
    rangeslider_visible = True,
    rangeselector = dict(
        buttons = list([
            dict(count = 1, label = '1M', step = 'month', stepmode = 'backward'),
            dict(count = 6, label = '6M', step = 'month', stepmode = 'backward'),
            dict(count = 1, label = 'YTD', step = 'year', stepmode = 'todate'),
            dict(count = 1, label = '1Y', step = 'year', stepmode = 'backward'),
            dict(step = 'all')])))

c_area.update_yaxes(
    title_text = '沪深300收盘价', 
    # tickprefix = '￥'
    )
c_area.update_layout(showlegend = False,
    title=dict(
        text='沪深300',
        font=dict(
            size=28,),
        x=0.5),
    yaxis=dict(range=[y_min*0.9,y_max*1.05]), # 设置 y 轴的范围
    )

# Pycharm，VS Code， Spider 等模式下
# Jupyter Notebook 下也可以用
# plot(c_area,filename='tmp/hushen300_area_update.html')

c_area.show()
```

上述代码的绘制效果如下：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gmw9y8k0ocj31ie0sy78p.jpg)

上图中：

- 可调节的时间栏是通过在 `update_xaxes` 中设置参数 `rangeslider_visible = True` 来实现的；
- 左上角的时间快速选择按钮，是通过在 `update_xaxes` 中设置参数 `rangeselector` 来实现的；
- 标题居中和y轴数值范围设置，是通过在 `update_layout` 中分别设置参数 'title' 和 'yaxis' 来实现的。


## 蜡烛图

蜡烛图又名 k线图、日本线 或者 日本K线图。

这种图表通常用作交易工具，用来显示和分析证券、衍生工具、外汇货币、股票、债券等商品随着时间的价格变动。

蜡烛图通过使用烛台式的符号来显示多种价格信息，例如开盘价、收盘价、最高价和最低价，每个代表单一时间段（每分钟、每小时、每天或每月）的交易活动。每个烛台符号沿着 X 轴上的时间刻度绘制，显示随着时间推移的交易活动。

蜡烛图的示意图如下：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gmwa727iusj30j60iidgo.jpg)

<!-- 来源：
https://datavizcatalogue.com/ZH/%E6%96%B9%E6%B3%95/%E8%9C%A1%E7%83%9B%E5%9B%BE.html -->

### 默认的蜡烛图

在 Plotly 中，可以使用 `candlestick` 图来绘制蜡烛图。

具体代码如下：

```python
# 蜡烛图，又称 K线 或 日本线

candlestick = go.Figure(data = [go.Candlestick(x = df_300.index, 
                                               open = df_300['open'], 
                                               high = df_300['high'], 
                                               low = df_300['low'], 
                                               close = df_300['close'])])

candlestick.update_layout(xaxis_rangeslider_visible = False, title = '沪深300')
candlestick.update_xaxes(title_text = '日期')
candlestick.update_yaxes(title_text = '沪深300价格')

# plot(candlestick,filename='tmp/hushen300_candlestick.html')

candlestick.show()
```

绘制效果如下：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gmwabcejupj31ja0todjf.jpg)

需要说明下，上图中的颜色，对应的是红跌绿涨，跟咱们国家通常的红涨绿跌刚好是相反的。后面将提到的 OHLC 图，在 Plotly 中也是 红跌绿涨，这个习惯跟美国、香港等区域是一致的。

### 对蜡烛图进行个性化修改

同样的，我们可以对蜡烛图进行一些个性化的修改，同样涉及标题、可调节的时间栏、y轴数值范围等。

具体代码如下：

```python
# 蜡烛图，个性化修改

c_candlestick = go.Figure(data = [go.Candlestick(x = df_300.index, 
                                               open = df_300['open'], 
                                               high = df_300['high'], 
                                               low = df_300['low'], 
                                               close = df_300['close'])])

c_candlestick.update_xaxes(
    title_text = '日期',
    rangeslider_visible = True,
    rangeselector = dict(
        buttons = list([
            dict(count = 1, label = '1M', step = 'month', stepmode = 'backward'),
            dict(count = 6, label = '6M', step = 'month', stepmode = 'backward'),
            dict(count = 1, label = 'YTD', step = 'year', stepmode = 'todate'),
            dict(count = 1, label = '1Y', step = 'year', stepmode = 'backward'),
            dict(step = 'all')])))

c_candlestick.update_layout(
    title = {
        'text': '沪深300',
        'y':0.9,
        'x':0.5,
#         'xanchor': 'center',
#         'yanchor': 'top'
    })
c_candlestick.update_yaxes(title_text = '沪深300价格')

# plot(c_candlestick,filename='tmp/hushen300_candlestick_update1.html')

c_candlestick.show()

```

绘制效果如下：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gmwaevm1ztj31ia0sijvh.jpg)

各个设置参数的说明，与前面提到的面积曲线图是类似的，这里不再赘述。

对于上面这个图，有一个地方需要说明下，当我们把时间范围缩小，比如最近1个月，会发现蜡烛图是不连续的，其中有周六日和假期是跳跃的。

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gmwapx7nyfj31i40sin0h.jpg)

在绘制股票曲线时经常会遇到这类问题，我们需要绘制的图形只包含交易日，这样的图表才是符合实际情况的。因此，我们需要在 Plotly 中也实现这个功能。

可以通过在 `update_xaxes` 设置参数 `rangebreaks` 来实现，实现的代码如下：

```python
c_candlestick.update_xaxes(
    rangebreaks=[
        dict(bounds=["sat", "mon"]), # 隐藏周六、周日
        dict(values=["2021-01-01",]+holidays)  # 隐藏特定假期
    ]
)
```

这里需要提醒下，

- 上面的 `bounds` 和 `values` 都是list 类型；
- `values` 中的元素，字符串格式为 `%Y-%m-%d`，比如 "2021-01-01" 。

**结合标题、时间栏、y轴数值范围、隐藏节假日一起的完整代码如下：**

```python
# 蜡烛图，个性化修改，隐藏周六日以及节假日

c_candlestick = go.Figure(data = [go.Candlestick(x = df_300.index, 
                                               open = df_300['open'], 
                                               high = df_300['high'], 
                                               low = df_300['low'], 
                                               close = df_300['close'])])

c_candlestick.update_xaxes(
    title_text = '日期',
    rangeslider_visible = True,
    rangeselector = dict(
        buttons = list([
            dict(count = 1, label = '1M', step = 'month', stepmode = 'backward'),
            dict(count = 6, label = '6M', step = 'month', stepmode = 'backward'),
            dict(count = 1, label = 'YTD', step = 'year', stepmode = 'todate'),
            dict(count = 1, label = '1Y', step = 'year', stepmode = 'backward'),
            dict(step = 'all')])))

c_candlestick.update_layout(
    title = {
        'text': '沪深300',
        'y':0.9,
        'x':0.5,
#         'xanchor': 'center',
#         'yanchor': 'top'
    })
c_candlestick.update_yaxes(title_text = '沪深300价格')

# 2020年国庆假期 2020.10.1-10.8
holidays = [ dt.date(2020,10,x).strftime('%Y-%m-%d') for x in range(1,9)]
print(holidays)

c_candlestick.update_xaxes(
    rangebreaks=[
        dict(bounds=["sat", "mon"]), # 隐藏周六、周日
        dict(values=["2021-01-01",]+holidays)  # 隐藏特定假期
    ]
)

# plot(c_candlestick,filename='tmp/hushen300_candlestick_update2.html')

c_candlestick.show()
```

## OHLC 图


OHLC 图 也称为「美国线」或「价格图」。

美国线 (Open-high-low-close Charts) 通常用作交易工具，显示和分析证券、货币、股票、债券等商品随时间的价格变动。OHLC 图有助于解释市场日常走势，并通过研究所形成的模式预测未来价格变化。

OHLC 图上的 Y 轴用作价格标尺，X 轴是时间刻度。在每个时段内，OHLC 图中会出现一个符号，以代表两个范围：交易的最高价和最低价，以及该时间段（例如一天）中的开盘价和收盘价。在这个范围符号上，最高和最低价的范围由主垂直线的长度所表示；而开盘和收盘价则分别在垂直线左右两边以一小段水平线代表。

示意图如下：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gmwax6bcanj30iy0eq0t4.jpg)

### 默认的OHLC图

在 Plotly 中，可以使用 `ohlc` 图来绘制蜡烛图。

具体代码如下：

```python
# OHLC图，又称 美国线，或 价格图

ohlc = go.Figure(data = [go.Ohlc(x = df_300.index, 
                                               open = df_300['open'], 
                                               high = df_300['high'], 
                                               low = df_300['low'], 
                                               close = df_300['close'])])

ohlc.update_layout(xaxis_rangeslider_visible = False, title = '沪深300')
ohlc.update_xaxes(title_text = '日期')
ohlc.update_yaxes(title_text = '沪深300价格')

# plot(ohlc,filename='tmp/hushen300_ohlc.html')

ohlc.show()
```

绘制效果如下：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gmwazg78n5j31ia0sm77q.jpg)



### 对OHLC图进行个性化设置

与蜡烛图类似，我们也可以进行个性化修改，具体代码如下：

```python
# # OHLC图，个性化修改

c_ohlc = go.Figure(data = [go.Ohlc(x = df_300.index, 
                                               open = df_300['open'], 
                                               high = df_300['high'], 
                                               low = df_300['low'], 
                                               close = df_300['close'])])

c_ohlc.update_xaxes(
    title_text = '日期',
    rangeslider_visible = True,
    rangeselector = dict(
        buttons = list([
            dict(count = 1, label = '1M', step = 'month', stepmode = 'backward'),
            dict(count = 6, label = '6M', step = 'month', stepmode = 'backward'),
            dict(count = 1, label = 'YTD', step = 'year', stepmode = 'todate'),
            dict(count = 1, label = '1Y', step = 'year', stepmode = 'backward'),
            dict(step = 'all')])))

c_ohlc.update_layout(
    title = {
        'text': '沪深300',
        'y':0.9,
        'x':0.5,
#         'xanchor': 'center',
#         'yanchor': 'top'
    })
c_ohlc.update_yaxes(title_text = '沪深300价格')

# 2020年国庆假期 2020.10.1-10.8
holidays = [ dt.date(2020,10,x).strftime('%Y-%m-%d') for x in range(1,9)]
print(holidays)

c_ohlc.update_xaxes(
    rangebreaks=[
        dict(bounds=["sat", "mon"]), # 隐藏周六、周日
        dict(values=["2021-01-01",]+holidays)  # 隐藏特定假期
    ]
)

# plot(c_ohlc,filename='tmp/hushen300_ohlc_update.html')

c_ohlc.show()
```

绘制效果如下：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gmwb2mp53gj31ic0so0xf.jpg)

上面这张大图里看不出美国线的显示效果，我们可以选择今年以来的图来查看下：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gmwb2jqmnyj31i40sgq6f.jpg)


本文完整的代码，请在公众号 **「Python数据之道」** 后台回复 `Plotly` 获取。


### 延伸阅读

- [视频：Plotly 和 Dash 在投资领域的应用](https://mp.weixin.qq.com/s/VopPrpe_64j5za4AYkmUTg)

- [视频：Plotly中绘制股票交易图表](https://mp.weixin.qq.com/s/1TUB2G-xavNm796uXUnRfQ)

- [Plotly中绘制三种经典的股票交易图表](https://mp.weixin.qq.com/s/1v3h5-5fNVk7cBdaRKzJkg)

- [在 Pycharm 等编辑器下使用 Python 可视化神器 Plotly Express](https://mp.weixin.qq.com/s/hBppR3XToI0hr2aKGkJ4sw)

- [推荐：这才是你寻寻觅觅想要的 Python 可视化神器](https://mp.weixin.qq.com/s/DTrtgdprota8bKRaZSprWA)

- [用 Plotly 动态散点图演示全球疫情变化趋势](https://mp.weixin.qq.com/s/mse3u-wXDsYiZbVS4U6m6w)

- [用 Python 动态曲线图来对全球疫情进行演示](https://mp.weixin.qq.com/s/YLp-nYjy23CDHXN3pOPFcQ)

- [升级版，用Python来进行多条曲线动态演示全球疫情变化](https://mp.weixin.qq.com/s/hUkk99SXN0kncQOCj8gHXw)

- [付费文章：用 Plotly 动态柱状图来演示全球疫情变化趋势](https://mp.weixin.qq.com/s/xQVHT9I3wk3kkBDQ3Dyx3A)

- [付费文章：超火动态排序疫情变化图，这次我们用 Plotly来绘制](https://mp.weixin.qq.com/s/LFTpPXy2se0cf_zNDQlf2g)

- [推荐一个牛逼的生物信息 Python 库 - Dash Bio](https://mp.weixin.qq.com/s/QK9VxHtsYcvGUjFiFlIxbA)


---

对我的文章感兴趣的朋友，可以关注我的微信公众号「**Python数据之道**」（ID：PyDataLab），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
