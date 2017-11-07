---
layout: posts
title: "Pandas: 如何将一列中的文本拆分为多行？"
teaser:
date: 2017-07-31
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



{% include alert info='本文首发于我的微信公众号（ID：PyDataRoad）。' %}

<br>

**Pandas: 如何将一列中的文本拆分为多行？**

**Pandas: split text in a column into multiple rows**

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>


## 背景

在数据处理过程中，经常会遇到以下类型的数据：

<div align="center">
    <img src="/images/posts/split-text.jpg">
</div>



在同一列中，本该分别填入多行中的数据，被填在一行里了，然而在分析的时候，需要拆分成为多行。

在上图中，列名为"Country" ，index为4和5的单元格内，值为"UK/Australia"和"UK/Netherland"。

今天，我们来介绍将含有多值的内容分拆成多行的几种方法。

加载数据

**PS：可以通过左右滑动来查看代码**

```python
import pandas as pd

df = pd.DataFrame({'Country':['China','US','Japan','EU','UK/Australia', 'UK/Netherland'],
               'Number':[100, 150, 120, 90, 30, 2],
               'Value': [1, 2, 3, 4, 5, 6],
               'label': list('abcdef')})
df

Out[2]:
         Country  Number  Value label
0          China     100      1     a
1             US     150      2     b
2          Japan     120      3     c
3             EU      90      4     d
4   UK/Australia      30      5     e
5  UK/Netherland       2      6     f
```

## 1 Method-1

分为如下几步：
1. 将含有多值的列进行拆分，然后通过`stack()`方法进行变换，并通过index的设置来完成
1. 用`drop()`方法从DataFrame中删除含有多值的列
1. 然后用`join()`方法来合并

```python
df.drop('Country', axis=1).join(df['Country'].str.split('/', expand=True).stack().reset_index(level=1, drop=True).rename('Country'))
Out[3]:
   Number  Value label     Country
0     100      1     a       China
1     150      2     b          US
2     120      3     c       Japan
3      90      4     d          EU
4      30      5     e          UK
4      30      5     e   Australia
5       2      6     f          UK
5       2      6     f  Netherland
```

过程分步介绍

```python
df['Country'].str.split('/', expand=True).stack()
Out[4]:
0  0         China
1  0            US
2  0         Japan
3  0            EU
4  0            UK
   1     Australia
5  0            UK
   1    Netherland
dtype: object

df['Country'].str.split('/', expand=True).stack().reset_index(level=1, drop=True)
Out[5]:
0         China
1            US
2         Japan
3            EU
4            UK
4     Australia
5            UK
5    Netherland
dtype: object

df['Country'].str.split('/', expand=True).stack().reset_index(level=1, drop=True).rename('Country')
Out[6]:
0         China
1            US
2         Japan
3            EU
4            UK
4     Australia
5            UK
5    Netherland
Name: Country, dtype: object

df.drop('Country', axis=1)
Out[7]:
   Number  Value label
0     100      1     a
1     150      2     b
2     120      3     c
3      90      4     d
4      30      5     e
5       2      6     f
```



## 2 Method-2

该方法的思路跟Method-1基本是一样的，只是在具体的细节方面有些差异。代码如下：


```python

df['Country'].str.split('/', expand=True).stack().reset_index(level=0).set_index('level_0').rename(columns={0:'Country'}).join(df.drop('Country', axis=1))
Out[8]:
      Country  Number  Value label
0       China     100      1     a
1          US     150      2     b
2       Japan     120      3     c
3          EU      90      4     d
4          UK      30      5     e
4   Australia      30      5     e
5          UK       2      6     f
5  Netherland       2      6     f
```

过程分步介绍如下：

```python
df['Country'].str.split('/', expand=True).stack().reset_index(level=0)
Out[9]:
   level_0           0
0        0       China
0        1          US
0        2       Japan
0        3          EU
0        4          UK
1        4   Australia
0        5          UK
1        5  Netherland

df['Country'].str.split('/', expand=True).stack().reset_index(level=0).set_index('level_0')
Out[10]:
                  0
level_0            
0             China
1                US
2             Japan
3                EU
4                UK
4         Australia
5                UK
5        Netherland

df['Country'].str.split('/', expand=True).stack().reset_index(level=0).set_index('level_0').rename(columns={0:'Country'})
Out[11]:
            Country
level_0            
0             China
1                US
2             Japan
3                EU
4                UK
4         Australia
5                UK
5        Netherland

df.drop('Country', axis=1)
Out[12]:
   Number  Value label
0     100      1     a
1     150      2     b
2     120      3     c
3      90      4     d
4      30      5     e
5       2      6     f
```


## 3 闲谈

当然，将某列中含有多值的单元拆分成多行，还有其他方法，各位小伙伴们可以研究下~~


<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="190">
</div>
