---
layout: posts
title: "Pandas实用技能，将列（column）排序的几种方法"
teaser:
date: 2021-08-07
header:
   image_fullwidth: "image-head.jpg"
categories:
   - DataAnalysis
tags:    
   - Pandas
comments: true
show_meta: true
sidebar: right
authors: ["阳哥"]
---

{% include alert info='Pandas实用技能，将列（column）排序的几种方法' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# Pandas实用技能，将列（column）排序的几种方法


大家好，我是阳哥。

Pandas 可以说是 在Python数据科学领域应用最为广泛的工具之一。

Pandas是一种高效的数据处理库，它以 `dataframe` 和 `series` 为基本数据类型，呈现出类似excel的二维数据。

在数据处理过程中，咱们经常需要将列按照一定的要求进行排序，以方便展示。

这里，阳哥来给大家分享下 在 Pandas 中将列排序的几种常用方法。

### 数据准备

文中主要使用了 `pandas` 和 `akshare` ，首先导入 Python 库，如下：

```python
import pandas as pd
import akshare as ak

print(f'pandas version: {pd.__version__}')
```

本次使用的数据如下：

```python
data = {
    'brand':['Python数据之道','价值前瞻','菜鸟数据之道','Python','Java'],
    'B':[4,6,8,12,10],
    'A':[10,2,5,20,16],
    'D':[6,18,14,6,12],
    'years':[4,1,1,30,30],
    'C':[8,12,18,8,2],
}

df = pd.DataFrame(data=data)
df
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gt5xt1k39ij30fg0a4mxl.jpg)

现将现有的 `columns` 输出，方便后面 copy 使用。

```python
df.columns

# Index(['brand', 'B', 'A', 'D', 'years', 'C'], dtype='object')
```

### Method 1

第一种方法，也是我自己常用的方法，就是自己将列的名称按需要进行手动排序，然后运行代码如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gt5xwwgr4pj30nw0eeaaw.jpg)

### Method 2

第二种方法，是使用 `.iloc` 方法，通过列的位置来进行排序，如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gt5xwyvp33j30i20e6750.jpg)

### Method 3

第三种方法，是使用 `.loc` 方法，通过列的名称来进行排序，如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gt5xx16rjwj30p40dwq3t.jpg)

这种方法跟第一种方法类似，个人觉得第一种方法更简洁些。

### Method 4

第四种是 逆序 排序，算是排序中一种特定的排序方式。

```python
# Method 4 ,逆序

cols = list(df.columns)

cols.reverse()

df[cols]
```

上述代码中，`cols.reverse()` 是将列表（list）进行逆序排序。

此外，列表（list）的逆序排序，还可以用 `cols[::-1]` 来实现。因此，下面的方法也可以实现逆序排序。

```python
# Method 4 ,逆序

cols = list(df.columns)

df[cols[::-1]]
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gt5y3ytcwqj30fe0awaah.jpg)

关于 Python数据类型 list 的介绍，详细内容可以前往下面的链接：

- [Python数据类型-List介绍(上)](https://mp.weixin.qq.com/s/QALgGF3VQ-_kB_P2rSF7sA)

- [Python数据类型-List介绍(下)-列表推导式](https://mp.weixin.qq.com/s/93LQCdGe2ILQKcQmqsnBdA)


### 实战案例：自由排序

有时候，当存在变量、列的数量较多，或者不同的dataframe中列的名称不完全一致等情况出现时，咱们不一定会通过列名称来实现排序。

这里分享一个实战案例，是关于制作基金的十大持仓数据表的，具体过程我就不在这里描述了，下面给出实现的函数，有兴趣的同学可以研究下。

自定义函数如下：

```python
# 需要安装 akshare
# pip install akshare

years = ['2019','2020','2021']

def fund_stock_holding(years,code):
    data = pd.DataFrame()
    for yr in years:
        df_tmp = ak.fund_em_portfolio_hold(code=code,year=yr)
        data = data.append(df_tmp)

    data['季度']=data['季度'].apply(lambda x:x[:8])
    data['占净值比例'] = pd.to_numeric(data['占净值比例'])
    data = data.sort_values(['季度','持仓市值'],ascending=[True,False])
    df = data.set_index(['序号','季度']).stack().unstack([1,2]).head(10)
    # df1.loc[:,(slice(None), ['股票名称','占净值比例'])]
    df = df.loc[:,(slice(None), '股票名称')]
    df = df.droplevel(None,axis=1)
    df.columns.name=None
    df = df.reset_index()
#     df.index.name = None
    df['基金代码'] = code
    return df
    
df = fund_stock_holding(years,'005669')
df
```

得到的数据表格如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gt5yd8vtv8j31jq0na7bb.jpg)

上面的表格中，我需要将 `基金代码` 这一列移动到 `序号` 这列的后面，由于 `years = ['2019','2020','2021']` 这是一个变量，当具体的值不同时，会导致列名称不一样，因此，在这种情况下我们不能直接使用列的具体名称，但咱们可以通过 列的位置组合来实现，列的调整具体如下：

```python
    cols = df.columns.tolist()
    cols = cols[:1] + cols[-1:] + cols[1:-1]  # 将基金代码列名放前面
    df = df[cols]
```

将上面的调整过程整合到自定义函数中，完整的代码如下：

```python
# 需要安装 akshare
# pip install akshare

years = ['2019','2020','2021']

def fund_stock_holding_update(years,code):
    data = pd.DataFrame()
    for yr in years:
        df_tmp = ak.fund_em_portfolio_hold(code=code,year=yr)
        data = data.append(df_tmp)

    data['季度']=data['季度'].apply(lambda x:x[:8])
    data['占净值比例'] = pd.to_numeric(data['占净值比例'])
    data = data.sort_values(['季度','持仓市值'],ascending=[True,False])
    df = data.set_index(['序号','季度']).stack().unstack([1,2]).head(10)
    # df1.loc[:,(slice(None), ['股票名称','占净值比例'])]
    df = df.loc[:,(slice(None), '股票名称')]
    df = df.droplevel(None,axis=1)
    df.columns.name=None
    df = df.reset_index()
#     df.index.name = None
    df['基金代码'] = code
    cols = df.columns.tolist()
    cols = cols[:1] + cols[-1:] + cols[1:-1]  # 将基金代码列名放前面
    df = df[cols]
    return df
    
df = fund_stock_holding_update(years,'005669')
df
```

效果如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gt5ydco6yij31jk0mydm6.jpg)

当然，我最后实现的效果是将基金代码换成基金名称，这个可以想办法实现，效果如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gt5pypti9tj31la0b0agn.jpg)


### 小结

以上就是关于 Pandas 中 列名称排序的介绍，看似很简单的内容，在最后的实践中，也还是有些小技巧的。

欢迎大家来畅聊，Pandas 中有哪些实用的小技巧~~

---

对我的文章感兴趣的朋友，可以关注我的微信公众号「**Python数据之道**」（ID：PyDataLab），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
