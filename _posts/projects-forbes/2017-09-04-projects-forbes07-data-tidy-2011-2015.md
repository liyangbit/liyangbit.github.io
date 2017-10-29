---
layout: posts
title: "福布斯系列(7)-数据清洗(c)"
teaser:
date: 2017-09-04
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



**福布斯系列之数据清洗（3） - Python数据分析项目实战**

## 1 前言


本文作为 **数据清洗的第三篇**，内容包含福布斯全球上市企业2000强排行榜数据中 **2011-2015年** 数据的初步处理过程。

福布斯全球上市企业2000强排行榜数据，从2007年到2017年，各个年份的数据都存在一些不规范的地方。

本文以2013年为例，描述2011年至2015年的数据清洗过程。


**本项目运行环境：**

* windows7
* jupyter notebook
* Python 3.5


## 2 数据清洗的目的

将不规范的数据进行处理，包括：
1. 检查数据完整性
1. 去除重复内容
1. 替换NaN值
2. 将字符串型数字转换为数字类型
3. 将数字后面多余的字母等文字剔除
4. 将公司和国家进行拆分
5. 按列名将DataFrame重新排序


最终达到的效果如下：

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
dtype: object
```

图1：



<div align="center"><img src="/images/projects/forbes/data_tidy_2013/1.png"></div>

## 3 2013年数据清洗的详细过程

导入相关python库

```python
import pandas as pd
import numpy as np

```

2013年的数据，由于初始爬取的数据不完整，后续又查找了另外2个数据源，所以共有3个数据源，下面分别介绍。

### 3.1 数据源1


从csv文件中读取数据

```python
df_2013 = pd.read_csv('./data/data_forbes_2013.csv', encoding='gbk',header=None)
print('the shape of DataFrame: ', df_2013.shape)
print(df_2013.dtypes)
df_2013.head()

out:
the shape of DataFrame:  (1991, 9)
```

数据源1中，2013年的数据只有1991条记录，数据可能有缺失，待进一步核实。


### 3.2 数据源2

2013年的数据在网上继续寻找，发现Economy Watch网站有相关数据，于是进行数据爬取。前文《》已描述了从该网站获取数据的过程。


从csv文件中读取数据

```python
df_2013_economy = pd.read_csv('./data/data_forbes_2013_economywatch.csv', encoding='gbk')
print('the shape of DataFrame: ', df_2013_economy.shape)
print(df_2013_economy.dtypes)
df_2013_economy.head()

out:
the shape of DataFrame:  (1984, 7)
```

发现数据只有1984条记录，也缺少相关记录，需要继续寻找其他记录。

### 3.3 数据源3

后来，找到一个excel文件，发现其数据记录是完整的，于是用该文件。

首先，从csv文件中读取数据


```python
df_2013_all = pd.read_excel('./data/data_forbes_2013_all.xlsx')
print('the shape of DataFrame: ', df_2013_all.shape)
print(df_2013_all.dtypes)
df_2013_all.head()

out:
the shape of DataFrame:  (2000, 7)
排名           int64
公司名         object
国家（地区）      object
销售额（亿美元）     int64
利润（亿美元）      int64
资产（亿美元）      int64
市值（亿美元）      int64
dtype: object
```

这里在前面两篇数据清洗文章的基础上，将数据清洗的过程整合到一起了。处理过程如下：


```python
# 更新列名
df_2013_all.columns = ['Rank', 'Company_cn_en',
                   'Country_cn', 'Sales', 'Profits', 'Assets', 'Market_value']

# 拆分"Company_cn_en"列，新生成两列，分别为公司英文名称和中文名称
df_2013_all['Company_cn'],df_2013_all['Company_en'] = df_2013_all['Company_cn_en'].str.split('/', 1).str
# print(df_2013_all['Company_cn'][:5])
# print(df_2013_all['Company_en'] [-5:])

# 将数据单位转换成十亿美元
df_2013_all[['Sales','Profits','Assets','Market_value']] =df_2013_all[['Sales','Profits','Assets','Market_value']].apply(lambda x: x/10)

# 添加年份2013
df_2013_all['Year'] = 2013

# 添加空白列
df_2013_all['Country_en'],df_2013_all['Country_cn_en'], df_2013_all['Industry_cn'], df_2013_all['Industry_en'] = ['','','','']

df_2013_all['Rank'] = pd.to_numeric(df_2013_all['Rank'])

# 按指定list重新将columns进行排序
df_2013_all = df_2013_all.reindex(columns=columns_sort)
print(df_2013_all.shape)
print(df_2013_all.dtypes)
df_2013_all.head()

```

请注意，2013年的数据，sales、Profits、Assets及Market_value列的本身就是为数字类型，不需要进一步处理。

## 4 2015年的个别情况

在前文《》提到，2015年的企业数量有重复的，因此，在数据处理过程中，需要剔除重复的内容。

去重的代码如下：

```python
# 数据有2020行，有重复行，需要去除重复行
# inplace=True，使去重生效
df_2015.drop_duplicates('Company_cn_en', inplace=True)
```

查看是否还有重复的内容：

```python
# 查看'Company_cn_en'是否还有重复行
df_2015[df_2015['Company_cn_en'].duplicated()]
```

通过查看结果可知，已无重复内容。


2015年的其他处理过程与2013年类似，就不做进一步描述了。



## 5 总结

2011年至2015年的数据，相互之间的相似度较高，数据清洗与处理过程和2013年的基本类似，具体过程就不再详细描述了（**提供源代码供参考**）。

2011年-2015年的数据， **Sales、Profits、Assets及Market_value列的本身就是数字类型**，可以在后续分析时计算使用，所以不需要像2008-2010年的数据那样处理。

结合前文2007年-2010年的数据清洗，将2007-2015年的数据清洗的代码合并到一起，感兴趣的同学可以回复关键字获取相关代码。

细心的同学可能会发现，本次依然没有提到针对国家数据的规整，因为国家数据的规整将统一到最后进行处理，敬请后续关注。

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
