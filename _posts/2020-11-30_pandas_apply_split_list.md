---
layout: posts
title: "Pandas 的这个知识点，估计 80% 的人都得挂！"
teaser:
date: 2020-11-30
header:
   image_fullwidth: "image-head.jpg"
categories:
   - Pandas
tags:    
   - Pandas 
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---

{% include alert info='Pandas 拆分 list' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# Pandas 的这个知识点，估计 80% 的人都得挂！

Pandas 中 apply 函数，应用广泛，今天要跟大家分享一个使用的技巧，使用 apply 将 dataframe 中内容为 list 的列拆分为多列。

拆分前的数据情况，如下图红色标注所示：

![](https://tva1.sinaimg.cn/large/0081Kckwgy1gl2kvncngmj30va07eta5.jpg)

拆分后，如下图所示：

![](https://tva1.sinaimg.cn/large/0081Kckwgy1gl2kvn5x1zj30pc08i0u5.jpg)

这个案例中，Lemon 使用的数据来自 `akshare` ，在开始前，引入相关 package ：

```python
import pandas as pd
import akshare as ak
```

Lemon 使用的两个 Python 库的版本信息如下：

```python
print(ak.__version__)
0.7.22
print(pd.__version__)
1.1.4
```

先从 akshare 获取需要的数据，分为两步，第一步是获取基金代码的列表，如下：

```python
df = ak.fund_em_fund_name().head(20).tail(5)
df = df[['基金代码','基金简称']]
print(df)
```

第二步是获取基金净值数据和净值日期，通过一个自定义函数来获取，自定义函数如下：

```python
# 获取基金单位净值以及净值日期
def get_mutual_fund(code):
    df = ak.fund_em_open_fund_info(fund=code, indicator="单位净值走势")
    df.columns = ['净值日期', '单位净值', 'equityReturn', 'unitMoney']
    df['净值日期'] = pd.to_datetime(df['净值日期'])
    df = df.sort_values('净值日期',ascending=False)
    unit_equity = df.head(1)['单位净值'].values[0]
    date_latest = df.head(1)['净值日期'].values[0]
    return [unit_equity, date_latest]
```

对于这个自定义函数，在 pandas 使用 apply 来应用自定义函数，这是使用 apply 的一种常用的方法，如下：

```python
# 获取基金最新的单位净值和净值日期
df['tmp'] = df['基金代码'].apply(get_mutual_fund)
print(df)
```

获取的数据截图如下：

![](https://tva1.sinaimg.cn/large/0081Kckwgy1gl2kvncngmj30va07eta5.jpg)

上图中的 `tmp` 列，就是我们这次需要进行处理的对象。

处理方法可以有多种，这里 Lemon 使用 pandas 中的 apply 来处理，相对来说，也是比较便捷的方式。

在 apply 函数中，使用 `pd.Series` 就可以达到我们的目的。

```python
# 将单位净值和净值日期单独成列
df[['最新单位净值','净值日期']] = df['tmp'].apply(pd.Series)
df = df.drop('tmp',axis=1)
print(df)
```

结果如下：

![](https://tva1.sinaimg.cn/large/0081Kckwgy1gl2kvn5x1zj30pc08i0u5.jpg)

看起来复杂的问题，解决起来也很便捷，是不是很容易啊，赶紧用起来吧。

---

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataLab），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
