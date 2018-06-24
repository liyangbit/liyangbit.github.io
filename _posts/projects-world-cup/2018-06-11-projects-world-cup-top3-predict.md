---
layout: posts
title: "2018世界杯：用Python分析热门夺冠球队-附源代码"
teaser:
date: 2018-06-11
header:
   image_fullwidth: "image-head.jpg"
categories:
  - projects
tags:
  - projects-world-cup
  - Pandas
  - recommend
comments: true
show_meta: true
sidebar: right
authors: ["Lemonbit"]
---


**Table of Contents**

<div class="panel radius" markdown="1">
{: #toc }
*  TOC
{:toc}
</div>


{% include alert info='文中有获取本文源代码的方式' %}

2018年，火热的世界杯即将拉开序幕。在比赛开始之前，我们不妨用 Python 来对参赛队伍的实力情况进行分析，并大胆的预测下本届世界杯的夺冠热门球队。


通过数据分析，可以发现很多有趣的结果，比如：
* 找出哪些队伍是首次进入世界杯的黑马队伍
* 找出2018年32强中之前已经进入过世界杯，但在世界杯上没有赢得过一场比赛的队伍

当然，我们本次的主要任务是要通过数据分析来预测2018年世界杯的夺冠热门队伍。

本次分析的数据来源于 Kaggle， 包含从 1872 年到今年的数据，包括世界杯比赛、世界杯预选赛、亚洲杯、欧洲杯、国家之间的友谊赛等比赛，一共大约 40000 场比赛的情况。

本次的环境为
* window 7 系统
* python 3.6
* Jupyter Notebook
* pandas version 0.22.0


先来看看数据的情况：

```python
import pandas as pd
import matplotlib.pyplot as plt

%matplotlib inline
plt.style.use('ggplot')

df = pd.read_csv('results.csv')
df.head()
```

该数据集包含的数据列的信息如下：
* 日期
* 主队名称
* 客队名称
* 主队进球数 （不含点球）
* 客队进球数 （不含点球）
* 比赛的类型
* 比赛所在城市
* 比赛所在国家
* 是否中立

结果如下：


<div align="center">
    <img src="/images/projects/world-cup/1.png">
</div>


## 1、 获取所有世界杯比赛的数据（不含预选赛）

```Python
df_FIFA_all = df[df['tournament'].str.contains('FIFA', regex=True)]
df_FIFA = df_FIFA_all[df_FIFA_all['tournament']=='FIFA World Cup']
df_FIFA.head()
```

结果如下：

<div align="center">
    <img src="/images/projects/world-cup/2.png">
</div>

数据做一个初步整理

```Python
df_FIFA.loc[:,'date'] = pd.to_datetime(df_FIFA.loc[:,'date'])
df_FIFA['year'] = df_FIFA['date'].dt.year
df_FIFA['diff_score'] = df_FIFA['home_score']-df_FIFA['away_score']
df_FIFA['win_team'] = ''
df_FIFA['diff_score'] = pd.to_numeric(df_FIFA['diff_score'])
```

创建一个新的列数据，包含获胜队伍的信息

```Python
# The first method to get the winners

df_FIFA.loc[df_FIFA['diff_score']> 0, 'win_team'] = df_FIFA.loc[df_FIFA['diff_score']> 0, 'home_team']
df_FIFA.loc[df_FIFA['diff_score']< 0, 'win_team'] = df_FIFA.loc[df_FIFA['diff_score']< 0, 'away_team']
df_FIFA.loc[df_FIFA['diff_score']== 0, 'win_team'] = 'Draw'

df_FIFA.head()

# The second method to get the winners

def find_win_team(df):
    winners = []
    for i, row in df.iterrows():
        if row['home_score'] > row['away_score']:
            winners.append(row['home_team'])
        elif row['home_score'] < row['away_score']:
            winners.append(row['away_team'])
        else:
            winners.append('Draw')
    return winners

df_FIFA['winner'] = find_win_team(df_FIFA)
df_FIFA.head()
```
结果如下：

<div align="center">
    <img src="/images/projects/world-cup/3.png">
</div>

## 2、 获取世界杯所有比赛的前20强数据情况

### 2.1 获取世界杯所有比赛获胜场数最多的前20强数据

```Python
s = df_FIFA.groupby('win_team')['win_team'].count()
s.sort_values(ascending=False, inplace=True)
s.drop(labels=['Draw'], inplace=True)
```

用pandas可视化如下：

**柱状图**

```Python
s.head(20).plot(kind='bar', figsize=(10,6), title='Top 20 Winners of World Cup')
```

<div align="center">
    <img src="/images/projects/world-cup/4.png">
</div>

**水平柱状图**

```Python
s.sort_values(ascending=True,inplace=True)
s.tail(20).plot(kind='barh', figsize=(10,6), title='Top 20 Winners of World Cup')
```

<div align="center">
    <img src="/images/projects/world-cup/5.jpg">
</div>


**饼图**


```Python
s_percentage = s/s.sum()
s_percentage
s_percentage.tail(20).plot(kind='pie', figsize=(10,10), autopct='%.1f%%',
                           startangle=173, title='Top 20 Winners of World Cup', label='')
```

<div align="center">
    <img src="/images/projects/world-cup/6.png">
</div>

**分析结论1：** 从赢球场数来看，巴西、德国、意大利、阿根廷四支球队实力最强。

通过上面的分析，我们还可以来查看部分国家的获胜情况

```Python
s.get('China', default = 'NA')
s.get('Japan', default = 'NA')
s.get('Korea DPR', default = 'NA')
s.get('Korea Republic', default = 'NA')
s.get('Egypt', default = 'NA')
```

运行结果分别是 ‘NA’，4，1，5，‘NA’。

从结果来看，中国队，在世界杯比赛上（不含预选赛）还没有赢过。当然，**本次世界杯的黑马-埃及队，之前两度进入世界杯上，但也没有赢过~~**



上面分析的是赢球场数的情况，下面我们来看下进球总数情况。

### 2.2 各个国家队进球总数量情况

```Python
df_score_home = df_FIFA[['home_team', 'home_score']]
column_update = ['team', 'score']
df_score_home.columns = column_update
df_score_away = df_FIFA[['away_team', 'away_score']]
df_score_away.columns = column_update
df_score = pd.concat([df_score_home,df_score_away], ignore_index=True)
s_score = df_score.groupby('team')['score'].sum()
s_score.sort_values(ascending=False, inplace=True)
s_score.sort_values(ascending=True, inplace=True)
s_score.tail(20).plot(kind='barh', figsize=(10,6), title='Top 20 in Total Scores of World Cup')
```

<div align="center">
    <img src="/images/projects/world-cup/7.png">
</div>

**分析结论2：** 从进球总数量来看，德国、巴西、阿根廷、意大利四支球实力最强。


上面分析的是自1872年以来的所有球队的数据情况，下面，我们重点来分析下2018年世界杯32强的数据情况。


## 3、2018年世界杯32强分析

2018年世界杯的分组情况如下：

第一组：俄罗斯、德国、巴西、葡萄牙、阿根廷、比利时、波兰、法国

第二组：西班牙、秘鲁、瑞士、英格兰、哥伦比亚、墨西哥、乌拉圭、克罗地亚

第三组：丹麦、冰岛、哥斯达黎加、瑞典、突尼斯、埃及、塞内加尔、伊朗

第四组：塞尔维亚、尼日利亚、澳大利亚、日本、摩洛哥、巴拿马、韩国、沙特阿拉伯


**获取32强的所有数据**


首先，判断是否有队伍首次打入世界杯。

```python
team_list = ['Russia', 'Germany', 'Brazil', 'Portugal', 'Argentina', 'Belgium', 'Poland', 'France',
             'Spain', 'Peru', 'Switzerland', 'England', 'Colombia', 'Mexico', 'Uruguay', 'Croatia',
            'Denmark', 'Iceland', 'Costa Rica', 'Sweden', 'Tunisia', 'Egypt', 'Senegal', 'Iran',
            'Serbia', 'Nigeria', 'Australia', 'Japan', 'Morocco', 'Panama', 'Korea Republic', 'Saudi Arabia']
for item in team_list:
    if item not in s_score.index:
        print(item)

out:
Iceland
Panama
```

通过上述分析可知，**冰岛队和巴拿马队是首次打入世界杯的。**

由于冰岛队和巴拿马队是首次进入世界杯，所以这里的32强数据，事实上是没有这两支队伍的历史数据的。

```Python
df_top32 = df_FIFA[(df_FIFA['home_team'].isin(team_list))&(df_FIFA['away_team'].isin(team_list))]
```

### 3.1 自1872年以来，32强数据情况

**赢球场数情况**

```Python
s_32 = df_top32.groupby('win_team')['win_team'].count()
s_32.sort_values(ascending=False, inplace=True)
s_32.drop(labels=['Draw'], inplace=True)
s_32.sort_values(ascending=True,inplace=True)
s_32.plot(kind='barh', figsize=(8,12), title='Top 32 of World Cup since year 1872')
```

<div align="center">
    <img src="/images/projects/world-cup/8.png">
</div>

**进球数据情况**

```Python
df_score_home_32 = df_top32[['home_team', 'home_score']]
column_update = ['team', 'score']
df_score_home_32.columns = column_update
df_score_away_32 = df_top32[['away_team', 'away_score']]
df_score_away_32.columns = column_update
df_score_32 = pd.concat([df_score_home_32,df_score_away_32], ignore_index=True)
s_score_32 = df_score_32.groupby('team')['score'].sum()
s_score_32.sort_values(ascending=False, inplace=True)
s_score_32.sort_values(ascending=True, inplace=True)
s_score_32.plot(kind='barh', figsize=(8,12), title='Top 32 in Total Scores of World Cup since year 1872')
```

<div align="center">
    <img src="/images/projects/world-cup/9.png">
</div>

**分析结论3：** 自1872年以来，32强之间的世界杯比赛，从赢球场数和进球数量来看，德国、巴西、阿根廷三支球队实力最强。


自1872年到现在，已经有100多年，时间跨度较大，有些国家已发生重大变化，后续分别分析自1978年（近10届）以及2002年（近4届）以来的比赛情况。

程序代码是类似的，这里只显示可视化的结果。

### 3.2 自1978年以来，32强数据情况

**赢球场数情况**

<div align="center">
    <img src="/images/projects/world-cup/10.png">
</div>


**进球数据情况**

<div align="center">
    <img src="/images/projects/world-cup/11.jpg">
</div>

**分析结论4：** 自1978年以来，32强之间的世界杯比赛，从赢球场数来看，阿根廷、德国、巴西三支球队实力最强。从进球数量来看，前3强也是这三支球队，但德国队的数据优势更明显。

### 3.3 自2002年以来，32强数据情况

**赢球场数情况**

<div align="center">
    <img src="/images/projects/world-cup/12.png">
</div>


**进球数据情况**

<div align="center">
    <img src="/images/projects/world-cup/13.png">
</div>


**分析结论5：** 自2002年以来，32强之间的世界杯比赛，从赢球场数和进球数量来看，德国、阿根廷、巴西三支球队实力最强。其中，德国队的数据优势更明显。



## 4、综合结论
2018年世界杯的32支队伍，根据以往的世界杯比赛数据来看，预测前三强为 德国、阿根廷和巴西，其中德国队应该是夺冠的最大热门。

**特别说明：** 以上数据分析，纯属个人学习用，预测结果与实际情况可能偏差很大，不能用于其他用途。


本文是一次比较综合的项目实战，希望可以给大家带来一些启发。

 >赞赏、点赞、转发、点点微信文章广告支持是一种认可，如需获取本文源代码，请在公众号后台回复 “PyDataRoad” ，谢谢大家支持。

<div align="center"><img src="/images/qrcode.jpg" width="200"/></div>
