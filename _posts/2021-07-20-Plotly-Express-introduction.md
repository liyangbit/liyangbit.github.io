---
layout: posts
title: "Plotly Express 可视化使用指南"
teaser:
date: 2021-07-22
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PythonVisualization
tags:    
   - Plotly
   - PlotlyExpress
comments: true
show_meta: true
sidebar: right
authors: ["阳哥"]
---

{% include alert info='Plotly Express 可视化使用指南' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# Plotly Express 可视化使用指南



>作者 | 阳哥
>
>出品 | Python数据之道 （ID：PyDataLab）

Plotly Express 是 Python 交互式可视化库 Plotly 的高级组件，受 Seaborn 和 ggplot2 的启发，它专门设计为具有简洁，一致且易于学习的 API ：只需一次导入，您就可以在一个函数调用中创建丰富的交互式绘图，包括分面绘图（faceting）、地图、动画和趋势线等。

使用 Plotly Express 可以轻松地进行数据可视化，一旦导入Plotly Express（通常简称 `px` ），大多数绘图只需要一个函数调用，接受一个整洁的 Pandas dataframe，并简单描述你想要制作的图。 如果你想要一个基本的散点图，它只是`px.scatter（dataframe，x =“column_name”，y =“column_name”）`。

Plotly Express 语法简洁，同时功能强大，可以绘制咱们遇到的大部分图表类型，比如线形图、散点图、柱状图、面积图、树形图、旭日图、甘特图等，本文将从如下20个方面，详细介绍 Plotly Express 的使用，看完内容介绍后，相应你也会喜欢上这个工具的。

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsma6qvupgj30ey0otaca.jpg)

<!-- ![](images/posts/202107-Plotly-Express/00-content.png) -->

部分动态图如下：

![](https://tva1.sinaimg.cn/large/008i3skNly1gsobv36kbtg30r30e5tt8.gif)

![](https://tva1.sinaimg.cn/large/008i3skNly1gsobv84eacg60r30e51iy02.gif)

Plotly 以及 Plotly Express 是交互式可视化工具，阳哥录制了一段简短的视频，来展示下其强大与优雅之处。

- [Plotly Express 可视化使用指南](https://mp.weixin.qq.com/s/6_7N6Wdu7mGWAIrmVIrU_Q)

## 00 环境与数据准备

### 环境准备

现在 Plotly Express 不需要单独安装，它是包含在 Plotly 中的，用 `pip install plotly` 命令可以安装 Plotly，如果你用的是 Anaconda，也可以用 `conda install plotly` 命令来安装，然后导入如下：

```python
import plotly.express as px
```

本文中，还用到 Pandas 等工具，完整的导入如下：

```python
import plotly.express as px
import plotly.io as pio
import plotly.graph_objects as go
import pandas as pd
```

本文代码测试的时候，使用的环境如下：

- Mac系统
- Jupyter Notebook
- Python版本为 v3.8
- Plotly version: 5.1.0
- Pandas version: 1.3.0rc1

### 数据准备

本文涉及的图形种类比较多，原本阳哥想用一份数据来覆盖这些图形的，后来发现不太现实，因此使用了多份数据。

对于大部分图形，阳哥在文中使用的是 `covid-19` 的数据，此外，还使用了跟股票投资相关的一些数据。

这里，先介绍下 `covid-19` 相关的数据。

我们需要读取并整理出需要的DataFrame，以便在后续可视化时使用。

分为三个小的步骤

(a) 读取数据及调整格式


```python
data = pd.read_csv('./data/covid-19.csv',parse_dates=['date'],index_col=0)

# 将数据复制一份
df_all = data.copy()

# 将时间格式转为字符串格式的日期，以 YYYY-mm-dd 的形式保存
df_all['dates'] = df_all['date'].apply(lambda x:x.strftime('%Y-%m-%d'))

# 添加现存确诊列
df_all['current'] = df_all['confirmed'] - df_all['cured'] - df_all['dead']

df_all.fillna('', inplace=True)

print(df_all.info())

df_all
```

(b) 获取全球的数据

```python
# 国内总计数量
df_cn = df_all.query("country=='中国' and province==''") 
df_cn = df_cn.sort_values('date',ascending=False)

# 国外，按国家统计
df_oversea = df_all.query("country!='中国'") 
df_oversea = df_oversea.fillna(value="")
df_oversea = df_oversea.sort_values('date',ascending=False)

df_global =  df_cn.append(df_oversea)
df_global = df_global.sort_values(['country','date'])
df_global
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmb8omihmj31de0luwhu.jpg)

<!-- ![](images/posts/202107-Plotly-Express/00-1.png) -->



(c) 更新全球的数据

```python
countries_all = list(df_global['country'].unique())

df_global_new = pd.DataFrame()

for item in countries_all:
    df_item = df_global.query('country==@item')
    df_i = df_item.copy(deep=False)
    df_i['new-confirmed'] = df_i['confirmed'].diff()
    df_i['new-cured'] = df_i['cured'].diff()
    df_global_new = df_global_new.append(df_i)
    
df_global_new = df_global_new.dropna()

df_global_new
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmb8ray0fj31jm0ss42l.jpg)

<!-- ![](images/posts/202107-Plotly-Express/00-2.png) -->

这两个 DataFrame： `df_global` 和 `df_global_new`我们会在后续可视化时多次用到。

> **说明：**
>
>文中使用的数据文件，在文末提供获取方式。


## 01 线形图（Line）

线形图是所有可视化工具中最常见的图表之一，在 px 中，用 `px.line()` 来进行可视化。

### 单条曲线

```python
def plot_line():
    df = df_global.groupby('date')[['confirmed','current','cured','dead']].sum()
    df['new-confirmed'] = df['confirmed'].diff()
    df = df.dropna()
    df = df.sort_index().loc['20200122':,:]
    fig = px.line(df,x=df.index,y='confirmed')
    return fig

fig = plot_line()
# fig.write_image('./images/px/01-line-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmbh22dqbj30jg0dw3yy.jpg)

**代码函数说明：**

需要说明的是，在最简洁的情况下，绘制线形图只需要用 `px.line(df,x =“column_name”，y =“column_name”)` 来绘制即可，比如上面示例中的 `px.line(df,x=df.index,y='confirmed')`

由于本文涉及可视化图表比较多，需要调整的DataFrame也比较多，因此，阳哥一般以自定义函数的形式进行封装，这样显得逻辑要清楚些。

实际上， **Plotly Express 默认的可视化，一般就是一行代码，这点咱们需要在脑海里有个印象。**

同时，在本文后续的图表可视化中，也是以这种形式进行的。

**图片保存**

在使用 Plotly 绘制图片时，是可以将静态图片保存下来的，具体方法请前往下面的链接查看：

- [Plotly中如何保存jpeg等图片？](https://mp.weixin.qq.com/s/jKuvxP9NjHcNdZv4VrUjAg)

### 多条曲线绘制

在 Plotly v4.8 版本以后，支持同时绘制多条曲线，其语法格式为 `px.line(df,x =“column_name”，y =[“column_name_1”,“column_name_2”,“column_name_3”,……])`

示例如下：

```python
def plot_line_multi():
    df = df_global.groupby('date')[['confirmed','current','cured','dead']].sum()
    df['new-confirmed'] = df['confirmed'].diff()
    df = df.dropna()
    df = df.sort_index().loc['20200122':,:]
    fig = px.line(df,x=df.index,y=['confirmed','current','dead'])
    return fig

fig = plot_line_multi()
# fig.write_image('./images/px/01-line-2.png')
fig.show()

```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmbh4sghkj30jg0dwwf2.jpg)


### 分列绘制曲线

Plotly Express 支持绘制分列或行的曲线图，通过设置参数 `facet_row` 或 `facet_col` 来实现，示例如下：

```python
# 绘制分列或行的曲线图
def plot_line_facet():  
    df = df_global_new.set_index('date').loc['20201204':'20201208',:]
    countries_list = ['美国','印度','俄罗斯','巴西','法国']
    df = df[df['country'].isin(countries_list)]
    fig = px.line(df,
              x=df.index,
              y='new-confirmed',
              color='country',
              line_group='country',
              facet_col='country',
#               log_y=True,
             )
    fig.update_traces(mode='markers+lines')
    return fig

fig = plot_line_facet()
# fig.write_image('./images/px/01-line-3.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmbh7j9u9j30jg0dw0tq.jpg)

在 Plotly Express 中，不止 `px.line()` 支持参数 `facet_row` 或 `facet_col` ，还有其他一些图形也是支持的，比如散点图、柱状图等，大家可以在 Jupyter Notebook 中查看其是否有这两个参数：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmcat86hpj30ko0ta0vn.jpg)

<!-- ![](images/posts/202107-Plotly-Express/01-line-4.png) -->

## 02 面积图（Area）

面积图，跟线形图有点类似，曲线下带阴影面积，因此称之为面积图。

Plotly Express 中通过 `px.area()` 来实现，示例如下：

```python
# 绘制填充的面积曲线图
def plot_area():
    df = df_global_new.set_index('date').loc['20200904':'20201208',:]
    countries_list = ['美国','印度','俄罗斯','巴西','法国']
    df = df[df['country'].isin(countries_list)]

    fig = px.area(df,
                  x=df.index,
                  y='confirmed',
                  color='country',
                  line_group='country'
                 )
    return fig

fig = plot_area()
# fig.write_image('./images/px/02-area-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmcg0adlgj30jg0dwt9g.jpg)



## 03 散点图（Scatter）

散点图，也是咱们常用的图形之一，Plotly Express 中通过 `px.scatter()` 来实现，示例如下：

```python
def plot_scatter_line():
    df = df_global.groupby('date')[['confirmed','current','cured','dead']].sum()
    df['new-confirmed'] = df['confirmed'].diff()
    df = df.dropna()
    df = df.sort_index().loc['20200122':,:]
    fig = px.scatter(df,x='confirmed',y='dead')
    return fig

fig = plot_scatter_line()
# fig.write_image('./images/px/03-sactter-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmcim18y2j30jg0dwq3g.jpg)

上面这张图，初看起来，跟线形图有点类似，实际它是散点图，只是点比较密集。

再来看一个示例，对全球多个国家的数据进行可视化后，散点分布就明显了：

```python
def plot_scatter():
    df = df_global_new.set_index('date').loc['20201208':'20201208']
    df = df.sort_values('confirmed',ascending=False).head(30)
    fig = px.scatter(df,x='confirmed',y='dead',
                     color='country',size='confirmed'
                    )
    return fig

fig = plot_scatter()
# fig.write_image('./images/px/03-sactter-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmcipe71sj30jg0dwjrx.jpg)


### 散点矩阵图

在 Plotly Express 中，针对散点矩阵图，专门有一个API来实现 `px.scatter_matrix()`，示例如下：


```python
def plot_scatter_matrix():
    df = df_global_new.set_index('date').loc['20201208':'20201208']
    df = df.sort_values('confirmed',ascending=False).head(30)
    fig = px.scatter_matrix(df,dimensions=['confirmed','cured','dead'],
                        color='country',
#                         size='confirmed'
                )
    return fig

fig = plot_scatter_matrix()
# fig.write_image('./images/px/03-sactter-3.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmcit00l4j30jg0dwq3v.jpg)



## 04 饼图（Pie）

绘制饼图时，用的是跟股票投资相关的数据，数据如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmn97607uj30pw0iy406.jpg)

>数据文件可在文末获取

上面这份数据，可以更方便的以饼图的形式来展示，在 Plotly Express 中，通过 `px.pie()` 来进行饼图的可视化，示例如下：


```python
fig = px.pie(df_invest,values='持有市值',names='指数名称')
fig.update_traces(textposition='inside', 
                  textinfo='percent+label'
                 )
# fig.write_image('./images/px/04-pie-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmn8zltr6j30jg0dw3z7.jpg)

通过设置参数 `hole`，还可以将饼图变为环形图，如下：

```python
fig = px.pie(df_invest,values='持有市值',
             names='指数名称',
             hole=0.6
            )
fig.update_traces(textposition='inside', 
                  textinfo='percent+label'
                 )
# fig.write_image('./images/px/04-pie-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmn92gq3pj30jg0dwgmb.jpg)


## 05 柱状图（Bar）

柱状图，是数据可视化中必不可少的基础图形，在 Plotly Express 中，通过 `px.bar()` 来实现。

### 垂直柱状图

默认情况下，`px.bar()` 绘制的是垂直柱状图，示例如下：

```python
def plot_bar():
    df = df_global_new.set_index('date').loc['20201208':'20201208']
    df = df.sort_values('confirmed',ascending=False).head(30)
    fig = px.bar(df,x='country',y='confirmed',color='country')
    return fig

fig = plot_bar()
# fig.write_image('./images/px/05-bar-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmnhb658wj30jg0dwgml.jpg)

### 水平柱状图

通过设置参数 `orientation` 的值，可以绘制水平柱状图，示例如下：

```python
def plot_bar_h():
    df = df_global_new.set_index('date').loc['20201208':'20201208']
    df = df.sort_values('confirmed',ascending=False).head(30)
    fig = px.bar(df,x='confirmed',y='country',
                 color='country',orientation='h'
                )
    return fig

fig = plot_bar_h()
# fig.write_image('./images/px/05-bar-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmnhdxzvoj30jg0dwmxx.jpg)

### 对数坐标

上面的水平柱状图，由于最大的数值和最小的数值差异比较大，用数据绝对值进行可视化时，图表的展示效果可能不是太好，这个时候，可以考虑用对数坐标来展示，通过设置参数 `log_x` 或 `log_y` 来实现， 如下：

```python
def plot_bar_log():
    df = df_global_new.set_index('date').loc['20201208':'20201208']
    df = df.sort_values('confirmed',ascending=False).head(30)
    fig = px.bar(df,x='confirmed',y='country',
                 color='country',orientation='h',
                 log_x=True  # log_x 对数坐标
                )
    return fig

fig = plot_bar_log()
# fig.write_image('./images/px/05-bar-3.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmnhgfnmhj30jg0dw75c.jpg)

### 柱状图的展现方式

当同一系列中有不同的类型时，绘制柱状图的时候，通常有堆积和分组两种展示形式，默认情况下，是堆积柱状图，如下：

```python
def plot_bar_stack():
    df = df_global_new.set_index('date').loc['20201201':'20201208',:]
    countries_list = ['美国','印度','俄罗斯','巴西','法国']
    df = df[df['country'].isin(countries_list)]
    fig = px.bar(df,x=df.index,
                 y='confirmed',
                 color='country'
                )
    return fig

fig = plot_bar_stack()
# fig.write_image('./images/px/05-bar-4.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmnqq5bdvj30jg0dwq3u.jpg)

通过设置参数 `barmode="group"` 可以转变为分组的柱状图，如下：

```python
def plot_bar_group():
    df = df_global_new.set_index('date').loc['20201201':'20201208',:]
    countries_list = ['美国','印度','俄罗斯','巴西','法国']
    df = df[df['country'].isin(countries_list)]
    fig = px.bar(df,x=df.index,
                 y='confirmed',
                 color='country',
                 barmode='group'
                )
    return fig

fig = plot_bar_group()
# fig.write_image('./images/px/05-bar-6.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmnqw29jkj30jg0dwdh0.jpg)

此外，在 Plotly 5.1版本中，还支持设置样式形状，有两个相关的参数，`pattern_shape` 和 `patter_shape_sequence`，如下：

```python
def plot_bar_shape():
    df = df_global_new.set_index('date').loc['20201201':'20201208',:]
    countries_list = ['美国','印度','俄罗斯','巴西','法国']
    df = df[df['country'].isin(countries_list)]
    fig = px.bar(df,x=df.index,
                 y='confirmed',
                 color='country',
                 pattern_shape="country", 
                 pattern_shape_sequence=[".", "x", "+",'/','\\'],
                )
    return fig

fig = plot_bar_shape()
# fig.write_image('./images/px/05-bar-5.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmnqtadjrj30jg0dwtb3.jpg)

## 06 箱形图（Box）

又称为盒须图、盒式图或箱线图，是一种用作显示一组数据分散情况资料的统计图，因形状如箱子而得名。它能显示出一组数据的最大值、最小值、中位数、及上下四分位数。

在股票以及指数基金投资时，我个人喜欢用箱形图来对标的的估值分布情况进行可视化，效果如下：

[![](https://tva1.sinaimg.cn/large/008i3skNgy1gsn506v4qbj30zi0jiq57.jpg)](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzU3OTA1MTUwOA==&action=getalbum&album_id=1537248357746835457#wechat_redirect)

因此，针对箱形图，我准备了一份类似的数据，是ETF的净值数据，数据结构如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmo0sdvxcj30wk0oe773.jpg)

针对ETF的累计净值数据绘制箱形图，通过 `px.box()` 来实现，如下：

```python
fig = px.box(df3,x='code',y='累计净值',color='code')
# fig.write_image('./images/px/06-box-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmo0nai5lj30jg0dwq3e.jpg)

通过设置参数 `orientation` 的值，可以绘制水平箱形图，示例如下：

```python
fig = px.box(df3,x='累计净值',y='code',color='code',orientation='h')
# fig.write_image('./images/px/06-box-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmo0pmvmxj30jg0dw74p.jpg)

## 07 小提琴图（Violin）

小提琴图 (Violin Plot)是用来展示多组数据的分布状态以及概率密度。这种图表结合了箱形图和密度图的特征，主要用来显示数据的分布形状。跟箱形图类似，但是在密度层面展示更好。在数据量非常大不方便一个一个展示的时候小提琴图特别适用。

Plotly Express 中通过 `px.violin()` 来实现，如下：

```python
fig = px.violin(df3,x='code',y='累计净值',color='code')
# fig.write_image('./images/px/07-violin-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmo76jq7bj30jg0dwaah.jpg)

上面的图只展示了数据密度分布情况，没有显示箱体及具体的分位数值情况，可以通过设置参数 `box=True` 来显示箱体，如下：


```python
fig = px.violin(df3,x='code',y='累计净值',
                color='code',
                box=True)
# fig.write_image('./images/px/07-violin-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmo79aya9j30jg0dwdga.jpg)

在显示箱体后，可以更好的体现出数据的密度分布以及数值分位情况。

此外，通过设置参数 `points='all'` ，可以在小提琴图旁边展示数据的密度分布情况，如下：


```python
fig = px.violin(df3,x='code',y='累计净值',
                color='code',
                box=True,
                points='all')
# fig.write_image('./images/px/07-violin-3.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmo7d0wo4j30jg0dw74z.jpg)

同样地，我们也可以以水平的方式来展示小提琴图，通过设置参数 `orientation='h'` 即可，如下：

```python
fig = px.violin(df3,x='累计净值',y='code',
                color='code',box=True,
                points='all',
                orientation='h'
               )
# fig.write_image('./images/px/07-violin-4.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmogn3wc8j30jg0dwq3o.jpg)

在小提琴图中，数据密度分布是通过设置参数 `point` 来实现的，在 Plotly Express 中，还有一个专门的函数 `px.strip()`，来绘制数据密度分布，如下：

```python
fig = px.strip(df3,y='code',x='累计净值')
# fig.write_image('./images/px/07-violin-5.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsmogphmhbj30jg0dw0tb.jpg)

## 08 联合分布图（Marginal）

还可以创建联合分布图（marginal rugs），使用直方图，箱形图（box）或小提琴来显示双变量分布，也可以添加趋势线。 Plotly Express 甚至可以帮助你在悬停框中添加线条公式和 R² 值！ 它使用 statsmodels 进行普通最小二乘（OLS）回归或局部加权散点图平滑（LOWESS）。

**使用小提琴图和箱形图**

```python
def plot_marginal():
    df = df_global_new.query('country=="美国" or country=="印度"')
    fig = px.scatter(df,x='new-confirmed',y='new-cured',
                     color='country',
    #                  size='confirmed',
                     trendline='ols',
                     marginal_x='violin',
                     marginal_y= 'box'
                    )
    return fig

fig = plot_marginal()
# fig.write_image('./images/px/08-marginal-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsn6l0rs9kj30jg0dwmy1.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNly1gsoc1i6ojfg30r30e5e2i.gif)


**使用小提琴图和直方图**

```python
def plot_marginal_hist():
    df = df_global_new.query('country=="美国" or country=="印度"')
    fig = px.scatter(df,x='new-confirmed',y='new-cured',
                     color='country',
    #                  size='confirmed',
                     trendline='ols',
                     marginal_x='violin',
                     marginal_y= 'histogram'
                    )
    return fig

fig = plot_marginal_hist()
# fig.write_image('./images/px/08-marginal-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsn6l3hj68j30jg0dw3zc.jpg)



## 09 直方图（Histogram）

直方图(Histogram)，又称质量分布图，是一种统计报告图，由一系列高度不等的纵向条纹或线段表示数据分布的情况。 一般用横轴表示数据类型，纵轴表示分布情况。

在 Plotly Express 中，通过 `px.histogram()` 来实现。

默认情况下，纵轴以计数形式（count）表示数据分布情况。

```python
df_hist = df3[df3['code']=='515180']
fig = px.histogram(df_hist,x='return')
# fig.write_image('./images/px/09-hist-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsn6qomtjsj30jg0dwq39.jpg)

通过参数 `histnorm` 可以设置纵轴数据分布的展现方式，其值可以是  `'percent'`, `'probability'`, `'density'`, 或 `'probability
    density'` 

纵轴为百分比的示例如下：

```python
fig = px.histogram(df_hist,x='return',histnorm='percent')
# fig.write_image('./images/px/09-hist-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsn6qqyaboj30jg0dwjrn.jpg)

通过设置参数 `color` ，可以同时对多个对象进行可视化，示例如下：

```python
fig = px.histogram(df3,x='return',color='code')
# fig.write_image('./images/px/09-hist-3.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsn6qtfqc3j30jg0dw74t.jpg)



## 10 漏斗图（Funnel）

漏斗图是一种形如漏斗状的明晰展示事件和项目环节的图形。漏斗图由横行或竖形条状形状一层层拼接而成 ，分别组成按一定顺序排列的阶段层级。每一层都用于表示不同的阶段，从而呈现出这些阶段之间的某项要素/指标递减的趋势。

漏斗图在常常应用于各行业的管理。

>网站转化率是指用户通过网站进行了相应目标行动的访问次数与总访问次数的比率，相应的行动可以是用户注册、用户参与、用户购买等一系列用户行为，而漏斗图是最能表示网站转化率的图表。

示例如下：

```python
data = dict(
    number=[10000, 7000, 4000, 2000, 1000],
    stage=["浏览次数", "关注数量", "下载数量", "咨询数量", "成交数量"])
fig = px.funnel(data, x='number', y='stage')
# fig.write_image('./images/px/10-funnel-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsn7bri70fj30jg0dwt94.jpg)

还有一种是面积漏斗图，通过 `px.funnel_area()` 来实现。

<!-- 此外，漏斗图在逻辑上一般是层层递减，也有一些时候，分析不同阶段的所占比例时，不一定逻辑是层层递减的，比如下面的面积漏斗图，展示每一个阶段的工作量的大小。 -->

```python
fig = px.funnel_area(names=["第一阶段", "第二阶段", "第三阶段", "第四阶段", "第五阶段"],
                    values=[10000, 7000, 4000, 2000, 1000])
# fig.write_image('./images/px/10-funnel-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsn7iio0ywj30jg0dwglz.jpg)



## 11 平行坐标图（Parallel）

平行坐标图 (parallel coordinate plot) 是可视化高维多元数据的一种常用方法，为了显示多维空间中的一组对象，绘制由多条平行且等距分布的轴，并将多维空间中的对象表示为在平行轴上具有顶点的折线。顶点在每一个轴上的位置就对应了该对象在该维度上的中的变量数值。

平行坐标图可以方便的展示数据标签分类或者数据流向。

数据标签分类的示例如下：


```python
def plot_para():
    df = df_global_new.set_index('date').loc['20201125':'20201208',:]
    countries_list = ['美国','印度','俄罗斯','巴西','法国']
    n = len(countries_list)
    countries_dict = {countries_list[x]:n-x for x in range(n)}
#     countries_dict = {'美国': 5, '印度': 4, '俄罗斯': 3, '巴西': 2, '法国': 1}
    df = df[df['country'].isin(countries_list)]
    df = df.reset_index()
    df = df[['country','confirmed','current','cured','dead']]
    df['country_id'] = df['country'].apply(lambda x: countries_dict[x])
#     print(df)
    fig = px.parallel_coordinates(df,color='country_id', labels={"country_id": "Country",
                      "confirmed": "confirmed", "current": "current",
                      "cured": "cured", "dead": "dead", },
                        color_continuous_scale=px.colors.diverging.Tealrose, 
#                         color_continuous_midpoint=3
                                 )
    return fig

fig = plot_para()
# fig.write_image('./images/px/11-para-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsn7oapf6nj30jg0dw3zs.jpg)

上图展示的是5个国家的疫情数据特征分布，展示不同国家（国家以 1、2、3、4、5代表）在累计确诊人数、现存确诊人数、累计治愈人数、累计死亡人数的分布情况。

这种类型的平行坐标图适合对数据分类进行可视化。

还有一类平行坐标图，适合对数据流向进行可视化，有点类似桑基图，不过展现形式稍微有点区别。在 Plotly Express 中， 通过 `px.parallel_categories()` 来实现。

下面以投资数据为例，来进行演示：

```python
def plot_para_cat():
    df = df_invest.reindex(columns=['市场','指数名称','标的名称','持有市值'])
    fig = px.parallel_categories(df, 
                             color="持有市值", 
                             color_continuous_scale=px.colors.sequential.PuBu
                            )
    return fig

fig = plot_para_cat()
# fig.write_image('./images/px/11-para-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsn7oddw0ij30jg0dwab3.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNly1gsobv84eacg60r30e51iy02.gif)

从上面这个数据流向的平行坐标图，可以从市场、指数名称、标的名称等维度来对投资数据的情况进行分析。


## 12 密度图（Density）

密度图表现与数据值对应的边界或域对象的一种理论图形表示方法，一般用于呈现连续变量。

在 Plotly Express 中， 通过 `px.density_contour()` 来实现。

示例如下：

```python
iris = px.data.iris()
fig = px.density_contour(iris, x="sepal_width", y="sepal_length")
# fig.write_image('./images/px/12-density-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsn7vo064cj30jg0dwgmi.jpg)

可以通过设置参数 `color` 来显示不同类别的颜色，如下“

```python
fig = px.density_contour(iris, x="sepal_width", y="sepal_length",color='species')
# fig.write_image('./images/px/12-density-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsn7vr1lptj30jg0dwwfj.jpg)


## 13 极坐标图（Polar）

极坐标图可用来显示一段时间内的数据变化，或显示各项之间的比较情况。适用于枚举的数据，比如不同地域之间的数据比较。

### 极坐标柱状图

在极坐标下，以柱状图的形式对数据进行可视化，在 Plotly Express 中， 通过 `px.bar_polar()` 来实现。

以疫情数据为例，对不同国家的数据演示如下：

```python
# 将累计确诊人数划分为不同的范围等级
def confirm_class(x):
    if x<=50000:
        y='1'
    elif 50000<x<=100000:
        y='2'
    elif 100000<x<=500000:
        y='3'
    elif 500000<x<=900000:
        y='4'
    elif 900000<x<=1200000:
        y='5'
    elif 1200000<x<=1700000:
        y='6'
    elif x>1700000:
        y='7'
    return y

countries_list = ['法国','意大利','英国','西班牙','阿根廷',
                  '哥伦比亚','德国','墨西哥','波兰','伊朗',
                  '秘鲁','土耳其','乌克兰','南非'
                 ]

df_global_top10_tmp = df_global_new[df_global_new['country'].isin(countries_list)]
# SettingWithCopyWarning
df_global_top10 = df_global_top10_tmp.copy(deep=False)
df_global_top10['gradient'] = df_global_top10['confirmed'].apply(confirm_class)
# df_global_top10
fig = px.bar_polar(df_global_top10, r="confirmed", 
                   theta="country", color="gradient", 
                   color_discrete_sequence= px.colors.sequential.Blugrn
                  )
# fig.write_image('./images/px/13-polar-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsn82vcz01j30jg0dwgme.jpg)

### 极坐标散点图

此外，在极坐标下，也可以使用散点图来对数据进行可视化，在 Plotly Express 中， 通过 `px.scatter_polar()` 来实现，示例如下：

```python
fig = px.scatter_polar(df_global_top10, r="confirmed", 
                       theta="country", color="gradient", 
                       symbol="gradient",
                       color_discrete_sequence=px.colors.sequential.Blugrn)

# fig.write_image('./images/px/13-polar-3.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsn82xwbqgj30jg0dwmxr.jpg)

### 雷达图

在 Plotly Express 中，还支持极坐标线形图（实际上是雷达图），通过 `px.line_polar()` 来实现，继续以上面的数据为例，如下：

```python
fig = px.line_polar(df_global_top10, r="confirmed", 
                    theta="country", color="gradient", 
                    line_close=True,
                    color_discrete_sequence=px.colors.sequential.Blugrn)

# fig.write_image('./images/px/13-polar-4.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsna9w6pzjj30jg0dw753.jpg)

总觉得上面的雷达图，用于这个疫情数据时，没有太多实际意义。

不妨来看看官方的示例，如下：

```python
df = px.data.wind()
fig = px.line_polar(df, r="frequency", theta="direction", 
                    color="strength", line_close=True,
                    color_discrete_sequence=px.colors.sequential.Plasma_r)

# fig.write_image('./images/px/13-polar-5.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsna9z4ewgj30jg0dw0to.jpg)

对于风力强度，以及方位分布的可视化，显得更加形象和符合实际情况。


## 14 图片显示（Imshow）

在 Plotly Express 中，还可通过 `px.imshow()` 来显示图片以及绘制热力图。

### 图片显示

读取一张图片，并在jupyter notebook 中显示出来，如下：

```python
from skimage import io
img = io.imread('./data/QR-PyDataLab.jpg')
fig = px.imshow(img)
# fig.write_image('./images/px/14-imshow-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnae9s560j30jg0dwwfe.jpg)

如果不想图片中显示坐标轴中的数字，可以调整轴的参数设置来实现，如下：

```python
fig = px.imshow(img)

fig.update_yaxes(
#     title=None, # 不显示轴标题
    visible = False,
#     showticklabels=True
)

fig.update_xaxes(visible = False)
# fig.write_image('./images/px/14-imshow-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnaed0360j30jg0dwdgo.jpg)

### 热力图

此外，`px.imshow()` 还可以用来绘制热力图。利用热力图可以看数据表里多个特征两两的相似度，在投资中，经常会考察不同投资产品之间的相关性强弱情况，就可以用热力图来表示，效果如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnarlx4iwj31fc0lotdr.jpg)

这里，阳哥也以前文的数据，对三支不同的ETF之间的相关性用热力图来绘制，示例如下：

```python
df_im = pd.pivot_table(df3,index=['净值日期'],
                       columns=['code'],values=['累计净值']
                      )
# 删除外层索引
df_im.columns = df_im.columns.droplevel()

# 隐藏列名称
df_im.columns.name = None

# 计算指数点位数值每日变化
returns = df_im.pct_change().dropna()

# 计算相关性
corr = returns.corr()
# corr

fig = px.imshow(corr,color_continuous_scale='PuBu')
# fig.write_image('./images/px/14-imshow-3.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnaefrfqdj30jg0dwmxf.jpg)

Plotly Express 中，还支持绘制热力密度图，通过 `px.density_heatmap()` 来实现，示例如下：

```python
df_hp = df_global_new.set_index('date').loc['20201208':'20201208']

countries_list = ['法国','意大利','英国','西班牙','阿根廷',
                  '哥伦比亚','德国','墨西哥','波兰','伊朗',
                  '秘鲁','土耳其','乌克兰','南非','比利时',
                  '印度尼西亚','伊拉克','荷兰','智智利','捷克','罗马尼亚'
                 ]

df_hp = df_hp[df_hp['country'].isin(countries_list)]
fig = px.density_heatmap(df_hp, 
                         x="confirmed", 
                         y="cured", 
                         nbinsx=20, 
                         nbinsy=20, 
                         color_continuous_scale="Blues"
                        )
fig.write_image('./images/px/14-imshow-4.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnau7ese8j30jg0dw3yt.jpg)



## 15 旭日图（Sunburst）

旭日图（Sunburst Chart）是一种现代饼图，它超越传统的饼图和环图，能表达清晰的层级和归属关系，以父子层次结构来显示数据构成情况。旭日图中，离远点越近表示级别越高，相邻两层中，是内层包含外层的关系。

在实际项目中使用旭日图，可以更细分溯源分析数据，真正了解数据的具体构成。而且，旭日图不仅数据直观，而且图表用起来特别炫酷，分分钟拉高数据汇报的颜值！

在下面这个视频中，针对投资组合的分布，阳哥就使用了旭日图，还是蛮直观实用的。

- [视频：Plotly 和 Dash 在投资领域的应用](https://mp.weixin.qq.com/s/VopPrpe_64j5za4AYkmUTg)


Plotly Express 中，，通过 `px.sunburst()` 来实现，示例如下：


```python
fig = px.sunburst(df_invest,path=['市场','指数名称','标的名称'],values='持有市值')
# fig.write_image('./images/px/15-sunburst-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnb0i4q6bj30jg0dw0tj.jpg)

可以通过设置参数 `textinfo` 的值来控制不同的交互显示方式。

```python
fig = px.sunburst(df_invest,path=['市场','指数名称','标的名称'],values='持有市值')

fig.update_traces(
        textinfo='label+percent entry'
        # 'label+percent root',    都是按照根节点来计算百分比，根节点为100%
        # 'label+percent entry',   根据当前位置来计算百分比，当前的最大节点为100%
        # 'label+percent parent'  当前最大节点占它的上一级节点的百分比不变，其他节点根据当前最大节点来计算百分比
    )
# fig.write_image('./images/px/15-sunburst-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnb0jwc3mj30jg0dw759.jpg)


![](https://tva1.sinaimg.cn/large/008i3skNly1gsobv36kbtg30r30e5tt8.gif)

## 16 甘特图（Timeline）

在进行项目进度管理时，经常会用到甘特图，在 Plotly Express 中，，通过 `px.timeline()` 来实现进度可视化，示例如下：

```python
def plot_timeline():

    df = pd.DataFrame([
        dict(Task="项目1", Start='2021-02-01', Finish='2021-03-25',Manager='Lemon',Completion_pct=90),
        dict(Task="项目2", Start='2021-03-05', Finish='2021-04-15',Manager='Lee',Completion_pct=60),
        dict(Task="项目3", Start='2021-02-20', Finish='2021-05-30',Manager='Zhang',Completion_pct=70),
        dict(Task="项目4", Start='2021-04-20', Finish='2021-09-30',Manager='Lemon',Completion_pct=20),
    ])

    fig = px.timeline(df, x_start="Start", x_end="Finish", y="Task")
    fig.update_yaxes(autorange="reversed")
    return fig

fig = plot_timeline()
# fig.write_image('./images/px/16-timeline-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnb4e7e8zj30jg0dwwes.jpg)

针对不同的项目人员或者其他非数值型因素，还可以设置颜色分组，如下：

```python
def plot_timeline_update():

    df = pd.DataFrame([
        dict(Task="项目1", Start='2021-02-01', Finish='2021-03-25',Manager='Lemon',Completion_pct=90),
        dict(Task="项目2", Start='2021-03-05', Finish='2021-04-15',Manager='Lee',Completion_pct=60),
        dict(Task="项目3", Start='2021-02-20', Finish='2021-05-30',Manager='Zhang',Completion_pct=70),
        dict(Task="项目4", Start='2021-04-20', Finish='2021-09-30',Manager='Lemon',Completion_pct=20),
    ])

    fig = px.timeline(df, x_start="Start", x_end="Finish", y="Task",color='Manager')
    fig.update_yaxes(autorange="reversed")
    return fig

fig = plot_timeline_update()
# fig.write_image('./images/px/16-timeline-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnb4gzcdcj30jg0dw74m.jpg)

对于数值型因素（比如完成进度比例），设置颜色分组，如下：


```python
def plot_timeline_color():

    df = pd.DataFrame([
        dict(Task="项目1", Start='2021-02-01', Finish='2021-03-25',Manager='Lemon',Completion_pct=90),
        dict(Task="项目2", Start='2021-03-05', Finish='2021-04-15',Manager='Lee',Completion_pct=60),
        dict(Task="项目3", Start='2021-02-20', Finish='2021-05-30',Manager='Zhang',Completion_pct=70),
        dict(Task="项目4", Start='2021-04-20', Finish='2021-09-30',Manager='Lemon',Completion_pct=20),
    ])

    fig = px.timeline(df, x_start="Start", 
                      x_end="Finish", 
                      y="Task",
                      color='Completion_pct',
                      color_continuous_scale=px.colors.sequential.RdBu
                     )
    fig.update_yaxes(autorange="reversed")
    return fig

fig = plot_timeline_color()
# fig.write_image('./images/px/16-timeline-3.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnbbtpmv7j30jg0dwt93.jpg)

## 17 树形图（Treemap）

树形图（Treemap）适用于显示大量分层结构（树状结构）的数据。

在一个树形图中，图表被分为若干个矩形，这些矩形的大小和顺序取决于制定变量，除此之外还常用颜色不同来表示另一个变量。

在 Plotly Express 中，，通过 `px.treemap()` 来实现树形图的可视化，如下：

**颜色为字符型，即离散型**

```python
fig = px.treemap(df_sunburst, path=[px.Constant('Invest portfolio'), '市场', '指数名称','标的名称'], 
                 values='持有市值',
                 color='市场', 
                 color_discrete_sequence= px.colors.sequential.RdBu_r  # 字符型，离散颜色
                )

# fig.write_image('./images/px/17-treemap-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnbgbkk2vj30jg0dwwf9.jpg)

**颜色为数值型，即连续型**

```python
fig = px.treemap(df_sunburst, path=[px.Constant('Invest portfolio'), '市场', '指数名称','标的名称'], 
                 values='持有市值',
                 color='持有市值', 
                 color_continuous_scale='YlGnBu',  # 数值型，连续颜色
                )
# fig.write_image('./images/px/17-treemap-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnbgecxedj30jg0dw75d.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNly1gsoc37hsttg30r30e51kx.gif)

### 冰柱图(icicle)

还有一种特殊的树状图，称之为冰柱图（icicle），冰柱图形状类似于冬天屋檐上垂下的冰柱，因此得名。

在 Plotly Express 中，，通过 `px.icicle()` 来实现，示例如下：

```python
fig = px.icicle(df_invest, 
                path=[px.Constant("Total Portfolio"), '市场', '指数名称','标的名称'], 
                values='持有市值',
#                 color_continuous_scale='YlGnBu',  # 数值型，连续颜色
                color_discrete_sequence= px.colors.sequential.RdBu_r  # 字符型，离散颜色
               )
fig.update_traces(root_color="lightgrey")
fig.update_layout(margin = dict(t=50, l=25, r=25, b=25))
# fig.write_image('./images/px/17-treemap-3.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnbggnk4fj30jg0dwmxq.jpg)



## 18 三维散点图（Scatter 3D）

对散点图从三个维度进行可视化，在 Plotly Express 有 `px.scatter_3d` 和 `px.scatter_ternary()` 两个函数来实现。

**三维立体空间的可视化：**

```python
df_global_top_latest = df_global_new.set_index('date').loc['20201208':'20201208',:]
countries_list = ['美国','印度','俄罗斯','巴西','法国','意大利','英国','西班牙','阿根廷','哥伦比亚','德国']

df_global_top_latest = df_global_top_latest[df_global_top_latest['country'].isin(countries_list)]

fig = px.scatter_3d(df_global_top_latest, 
                    x="confirmed", y="cured", z="dead", 
                    color="country", size="dead", 
                    hover_name="country",
                   )
# fig.write_image('./images/px/18-3d-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnbqgc952j30jg0dw0t8.jpg)

**同一个平面下，从三个维度进行可视化：**

```python
df = px.data.election()
fig = px.scatter_ternary(df, a="Joly", b="Coderre", c="Bergeron", 
                         color="winner", size="total", 
                         hover_name="district",
                         size_max=15, 
                         color_discrete_map = {"Joly": "blue", "Bergeron": "green", "Coderre":"red"} )
# fig.write_image('./images/px/18-3d-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnbqj6xiwj30jg0dw74u.jpg)


## 19 地图（Map）

Plotly Express 对于地图的可视化提供了许多的功能，由于我自己用的并不多，这里直接给大家介绍了官方的应用案例。

大家有兴趣的话，可以进一步研究下。

**示例（1）：**

```python
df = px.data.gapminder().query("year == 2007")
fig = px.scatter_geo(df, locations="iso_alpha",
                     size="pop", # size of markers, "pop" is one of the columns of gapminder
                     )
fig.update_layout(margin={"r":0,"t":0,"l":0,"b":0})

# fig.write_image('./images/px/19-map-1.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnbvh8gkij30jg0dwdgu.jpg)

**示例（2）：**

```python
df = px.data.gapminder().query("year == 2007")
fig = px.line_geo(df, locations="iso_alpha",
                  color="continent", # "continent" is one of the columns of gapminder
                  projection="orthographic")
# fig.write_image('./images/px/19-map-2.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnbvjon4zj30jg0dwjsg.jpg)

**示例（3）：**

```python
df = px.data.election()
geojson = px.data.election_geojson()

fig = px.choropleth(df, geojson=geojson, color="Bergeron",
                    locations="district", featureidkey="properties.district",
                    projection="mercator"
                   )
fig.update_geos(fitbounds="locations", visible=False)
fig.update_layout(margin={"r":0,"t":0,"l":0,"b":0})

# fig.write_image('./images/px/19-map-3.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnbvnara5j30jg0dwwfj.jpg)

**示例（4）：**

```python
# us_cities.to_csv('./data/us_cities.csv',index=False)
us_cities = pd.read_csv('./data/us_cities.csv')
# us_cities = pd.read_csv("https://raw.githubusercontent.com/plotly/datasets/master/us-cities-top-1k.csv")
fig = px.scatter_mapbox(us_cities, lat="lat", lon="lon", hover_name="City", hover_data=["State", "Population"],
                        color_discrete_sequence=["fuchsia"], zoom=3, height=300)
fig.update_layout(
    mapbox_style="white-bg",
    mapbox_layers=[
        {
            "below": 'traces',
            "sourcetype": "raster",
            "sourceattribution": "United States Geological Survey",
            "source": [
                "https://basemap.nationalmap.gov/arcgis/rest/services/USGSImageryOnly/MapServer/tile/{z}/{y}/{x}"
            ]
        },
        {
            "sourcetype": "raster",
            "sourceattribution": "Government of Canada",
            "source": ["https://geo.weather.gc.ca/geomet/?"
                       "SERVICE=WMS&VERSION=1.3.0&REQUEST=GetMap&BBOX={bbox-epsg-3857}&CRS=EPSG:3857"
                       "&WIDTH=1000&HEIGHT=1000&LAYERS=RADAR_1KM_RDBR&TILED=true&FORMAT=image/png"],
        }
      ])
fig.update_layout(margin={"r":0,"t":0,"l":0,"b":0})
# fig.write_image('./images/px/19-map-4.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnbya4pofj60jg08cdii02.jpg)

**示例（5）：**

```python
us_cities = us_cities.query("State in ['New York', 'Ohio']")

fig = px.line_mapbox(us_cities, lat="lat", lon="lon", color="State", zoom=3, height=300)

fig.update_layout(mapbox_style="stamen-terrain", mapbox_zoom=4, mapbox_center_lat = 41,
    margin={"r":0,"t":0,"l":0,"b":0})
# fig.write_image('./images/px/19-map-5.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnbyc3ztuj30jg08ct9v.jpg)

**示例（6）：**

```python
df = px.data.election()
geojson = px.data.election_geojson()

fig = px.choropleth_mapbox(df, geojson=geojson, color="Bergeron",
                           locations="district", featureidkey="properties.district",
                           center={"lat": 45.5517, "lon": -73.7073},
                           mapbox_style="carto-positron", zoom=9)
fig.update_layout(margin={"r":0,"t":0,"l":0,"b":0})
# fig.write_image('./images/px/19-map-6.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnc0shofbj30rd0eldhy.jpg)

**示例（7）：**

```python
df_eq = pd.read_csv('./data/earthquakes.csv')

fig = px.density_mapbox(df_eq, lat='Latitude', lon='Longitude', z='Magnitude', radius=10,
                        center=dict(lat=0, lon=180), zoom=0,
                        mapbox_style="stamen-terrain")
fig.update_layout(margin={"r":0,"t":0,"l":0,"b":0})
# fig.write_image('./images/px/19-map-7.png')
fig.show()
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnc0v1ljgj30jg0dwmzc.jpg)


## 20 颜色（Colors）


在 Plotly Express 使用过程中，经常会设置多种颜色，Plotly 中内置了多种颜色，咱们可以熟悉下，方便使用。

下面跟大家分享两种我常用的颜色体系：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnc4kvnbzj31cc0u0n13.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsnc4n39xhj31dn0u042w.jpg)

对于颜色搭配，有兴趣的同学可以自行研究。配色是一门高深的艺术，里面门道还是很多的哈~~

## Plotly 生态系统的一部分

Plotly Express 在 2019年初刚发布的时候，是一个单独的 Python 库，在后来的版本中，Plotly 官方将 Plotly Express 合并到 Plotly 中了，因此， Plotly Express 可以使用 Plotly 中大部分接口来对图形进行个性化设置。这个也可以认为是 Plotly Express 相对其他 基于 Plotly 的第三方库（比如 cufflinks）的一些优势之处。

### 能够与 Dash 完美匹配

Dash 是 Plotly 的开源框架，用于构建具有 Plotly.py 图表的分析应用程序和仪表板。

Plotly Express 产生的对象与 Dash 100％兼容。

在下面这个视频中，针对投资组合的分析，阳哥就综合使用了Plotly、Plotly Express 以及Dash，还是蛮实用的。

- [视频：Plotly 和 Dash 在投资领域的应用](https://mp.weixin.qq.com/s/VopPrpe_64j5za4AYkmUTg)

最后，Plotly Express 作为一个简洁实用的 Python 可视化库，在 Plotly 生态系统下，发展迅速，所以不要犹豫，立即开始使用 Plotly Express 吧！

### 数据获取

为方便大家运行文中的内容，阳哥已经整理好数据文件以及完整的 `《Plotly Express 可视化使用指南》` （高清pdf版），大家可以在公众号「**Python数据之道**」回复 **px** 来获取。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
