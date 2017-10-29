---
layout: posts
title: "福布斯系列(6)-数据清洗(b)"
teaser:
date: 2017-08-25
header:
   image_fullwidth: "image-head.jpg"
categories:
  - projects
tags:
  - projects-forbes
  - Pandas
  - Numpy
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---


**Table of Contents**

<div class="panel radius" markdown="1">
{: #toc }
*  TOC
{:toc}
</div>



**福布斯系列之数据清洗（2） - Python数据分析项目实战**

## 1 前言


本文作为 **数据清洗的第二篇**，内容包含福布斯全球上市企业2000强排行榜数据中 **2008-2010年** 数据的初步处理过程。

福布斯全球上市企业2000强排行榜数据，从2007年到2017年，各个年份的数据都存在一些不规范的地方。

本文以2008年为例，描述2008年至2010年的数据清洗过程。


**本项目运行环境：**

* windows7
* jupyter notebook


## 2 数据清洗的目的

将不规范的数据进行处理，包括：
1. 替换NaN值
2. 将字符串型数字转换为数字类型
3. 将数字后面多余的字母等文字剔除
4. 将公司和国家进行拆分
5. 按列名将DataFrame重新排序


最终达到的效果如下：

**数据清洗前**

数据类型

```python
the shape of DataFrame:  (2000, 10)
年份                         int64
Rank                       int64
公司名称（英文）                  object
公司名称（中文）                  object
Country/area（国家或地区）       object
Industry（行业）              object
Sales （销售额）($bil十亿美元)     object
Profits （利润）($bil)        object
Assets 资产($bil)           object
Market Value 市值($bil)    float64
dtype: object
```

图1：


<div align="center"><img src="/images/projects/forbes/data_tidy_2008/1.png"></div>


**数据清洗后**

数据类型

```python
Year               int64
Rank               int64
Company_cn_en     object
Company_en        object
Company_cn        object
Country_cn_en     object
Country_cn        object
Country_en        object
Industry_cn       object
Industry_en       object
Sales            float64
Profits          float64
Assets           float64
Market_value     float64
```

图2：



<div align="center"><img src="/images/projects/forbes/data_tidy_2008/2.png"></div>


**数据清洗后， sales、Profits、Assets及Market_value列的数据均为数字类型**，方便后续分析时计算使用。


## 3 2008年数据清洗的详细过程

导入相关python库

```python
import pandas as pd
import numpy as np

```


从csv文件中读取数据

```python
df_2008 = pd.read_csv('./data/data_forbes_2008.csv', encoding='gbk', thousands=',')
print('the shape of DataFrame: ', df_2008.shape)
print(df_2008.dtypes)
df_2008.head()
```

更新列名

```python
df_2008.columns = ['Year', 'Rank', 'Company_en', 'Company_cn','Country_en', 'Industry_en', 'Sales', 'Profits', 'Assets', 'Market_value']
df_2008.head()
```

添加空白列，使之与其他年份的格式保持一致

```python
df_2008['Company_cn_en'], df_2008['Country_cn_en'], df_2008['Country_cn'], df_2008['Industry_cn'] = ['','','','']
df_2008.head()
```

**将字符串转换为数字类型**

在前文《 》2007年的数据清洗过程中，单独针对'Sales','Profits'及'Assets'进行处理。

本文中，**对处理过程进行了一些优化**，通过for循环，一次性处理不同的字段，大大减少了代码量，以及程序的繁冗程度，使整个过程变得更简洁。

处理过程如下：

```python
col_digit = ['Sales', 'Profits', 'Assets', 'Market_value']

for col in col_digit:
    # 将数字后面的字母进行替换
    df_2008[col] = df_2008[col].replace('([A-Za-z])', '', regex=True)

    # 千分位数字的逗号被识别为string了，需要替换
    df_2008[col] = df_2008[col].replace(',', '', regex=True)

    #将数字型字符串转换为可进行计算的数据类型
    df_2008[col] = pd.to_numeric(df_2008[col])
```

按指定list重新将columns进行排序

```python
# 按指定list重新将columns进行排序
columns_sort = ['Year', 'Rank', 'Company_cn_en','Company_en',
                'Company_cn', 'Country_cn_en', 'Country_cn',
                'Country_en', 'Industry_cn', 'Industry_en',
                'Sales', 'Profits', 'Assets', 'Market_value']

df_2008 = df_2008.reindex(columns=columns_sort)
print(df_2008.shape)
print(df_2008.dtypes)
df_2008.head()
```

## 4 后续

2009年和2010年的数据清洗过程跟2008年基本一致，处理过程就不在本文中描述了（**提供源代码供参考**）。


结合前文2007年的数据清洗，将2007-2010年的数据清洗的代码合并到一起，感兴趣的同学可以回复关键字获取相关代码。

细心的同学可能会发现，本次没有提到针对国家数据的规整，因为国家数据的规整将统一到最后进行处理，敬请后续关注。


>关注微信公众号公众号"Python数据之道"，后台回复"**2017038**"，获取本文的源代码及原始数据文件。

<div align="center"><img src="/images/qrcode.jpg" width="200"/></div>

## 相关文章


{% for entry in site.data.mycategories.entries %}
{% for mytag in entry.tags %}

{% for tag in page.tags %}
{% if mytag == tag %}
{% include list-posts.html tag=mytag %}
{% endif %}
{% endfor %}

{% endfor %}
{% endfor %}
