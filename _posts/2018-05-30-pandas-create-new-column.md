---
layout: posts
title: "Pandas小册子：根据条件创建新的列"
teaser:
date: 2018-05-30
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




{% include alert info='Pandas小册子：根据条件创建新的列' %}


<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>




在进行数据分析时，经常会遇到根据已有的数据列，按照一定条件创建新的数据列，然后进行进一步分析。

今天，我们来看一个根据已有数据按照一定条件创建新的数据列的方法。

数据如下：

```python
import pandas as pd

df = pd.DataFrame({'team_A': ['Spain', 'Germany', 'Brazil', 'France'],
                  'team_B': ['USA', 'Argentina', 'Mexico', 'Belgium'],
                  'score_A': [5, 3, 2, 0],
                  'score_B': [4, 0, 3, 0]},
                 columns = ['team_A', 'team_B', 'score_A', 'score_B'])
df

Out[2]:
    team_A     team_B  score_A  score_B
0    Spain        USA        5        4
1  Germany  Argentina        3        0
2   Brazil     Mexico        2        3
3   France    Belgium        0        0
```

**问题：** 从上面数据中创建新的一个数据列，用来存储获胜队伍的名称。

即，根据 "score_A" 与 "score_B" 比较的结果，来获取相应的结果。

例如，第一行， "Spain":"USA" 为 5:4 ， "Spain" 获胜， 新创建的列中存储的结果为 "Spain"。

下面介绍两种方法来实现上述要求。

## 第一种方法

第一种方法是 利用 Pandas 中 DataFrame 的条件选择功能来实现，过程如下：

```python
# 创建新的列 "win_team"，赋值为空白
df['win_team'] = ''

# 创建判断条件 mask
mask = df['score_A'] - df['score_B']
df.loc[mask > 0, 'win_team'] = df.loc[mask > 0, 'team_A']
df.loc[mask < 0, 'win_team'] = df.loc[mask < 0, 'team_B']
df.loc[mask == 0, 'win_team'] = 'Draw'

df

Out[3]:
    team_A     team_B  score_A  score_B win_team
0    Spain        USA        5        4    Spain
1  Germany  Argentina        3        0  Germany
2   Brazil     Mexico        2        3   Mexico
3   France    Belgium        0        0     Draw
```

## 第二种方法

第二种方法是结合 DataFrame.iterrows() 以及 Python 的 list 的功能来实现，过程如下：

```python
# The second method to get the winners

def find_win_team(df):
    winners = []
    for i, row in df.iterrows():
        if row['score_A'] > row['score_B']:
            winners.append(row['team_A'])
        elif row['score_A'] < row['score_B']:
            winners.append(row['team_B'])
        else:
            winners.append('Draw')
    return winners

df['winner'] = find_win_team(df)
df

Out[4]:
    team_A     team_B  score_A  score_B win_team   winner
0    Spain        USA        5        4    Spain    Spain
1  Germany  Argentina        3        0  Germany  Germany
2   Brazil     Mexico        2        3   Mexico   Mexico
3   France    Belgium        0        0     Draw     Draw
```

关于 DataFrame.iterrows(), 我们先来看看其运行结果。

```python
for row_index, row in df.iterrows():
    print('%s\n%s' % (row_index, row))

0
team_A      Spain
team_B        USA
score_A         5
score_B         4
win_team    Spain
winner      Spain
Name: 0, dtype: object
1
team_A        Germany
team_B      Argentina
score_A             3
score_B             0
win_team      Germany
winner        Germany
Name: 1, dtype: object
2
team_A      Brazil
team_B      Mexico
score_A          2
score_B          3
win_team    Mexico
winner      Mexico
Name: 2, dtype: object
3
team_A       France
team_B      Belgium
score_A           0
score_B           0
win_team       Draw
winner         Draw
Name: 3, dtype: object
```

DataFrame.iterrows() 的作用是将 dataframe的每行转换成为一个 Series，可以理解为 针对于每一行，做了行列转置。

图示如下：

<div align="center">
    <img src="/images/posts/20180530-1.jpg">
</div>



<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号『Python数据之道』（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="200">
</div>
