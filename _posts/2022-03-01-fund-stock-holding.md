---
layout: posts
title: "用 Python 快速获取基金的股票持仓增减情况"
teaser:
date: 2022-03-01
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PyFinance
tags:    
   - Akshare
   - Pandas    
comments: true
show_meta: true
sidebar: right
authors: ["阳哥"]
---

{% include alert info='用 Python 快速获取基金的股票持仓增减情况' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# 用 Python 快速获取基金的股票持仓增减情况



## 01 写在前面

大家好，我是阳哥。

大家知道，在财经领域，Python是有着广泛的用途的。2021年，量化基金开始成为弄潮儿。有不少消息称，量化基金贡献了相当比例的成交量，量化基金对成交量的贡献到底有多大，咱也不知道。

我们所知道的是，在金融量化领域，Python 是主力编程语言之一。

在2021年9月和2022年1月，阳哥曾分享过 「用 Python 快速获取基金持仓增值与减持情况」 这个内容，在1月份的内容上，发现有部分内容存在一些bug，本次将更新下文章内容，以及代码文件。

在财经领域，之前阳哥分享过部分Python实践应用的主题，包括：

1. [用Python来做一个投资计划](https://mp.weixin.qq.com/s/WYuMwCJBrWaBiDs8xp2KMA)
2. [用Python快速追踪基金的收益情况](https://mp.weixin.qq.com/s/7w3Ned9M5FqRQd6inxmeRw)
3. [视频：Plotly 和 Dash 在投资领域的应用](https://mp.weixin.qq.com/s/VopPrpe_64j5za4AYkmUTg)
4. [视频：Plotly中绘制股票交易图表](https://mp.weixin.qq.com/s/1TUB2G-xavNm796uXUnRfQ)
5. [Plotly中绘制三种经典的股票交易图表](https://mp.weixin.qq.com/s/1v3h5-5fNVk7cBdaRKzJkg)
6. [神器Tushare，财经数据必备工具！](https://mp.weixin.qq.com/s/c1ukemeK12flCgA-lo69fA)
7. [用 Python 读取巴菲特近期持仓数据](https://mp.weixin.qq.com/s/6h9Xq2lfrZSlRNw_W8aWcg)

大家知道，近几年，不少同学都是经由基金进入到股市中的。2020年就很流行“买股不如买基”的说话，至于股票和基金到底谁更好，这个仁者见仁智者见智，恐怕一时半会儿也说不清楚。

本文中，阳哥给大家分享的主题是 用 Python 来追踪和更新基金的持仓结构以及基金的股票增持和减持情况，涉及到的Python库主要是 `pandas` 和 `akshare` 。

最终实现的效果包括两个方面：

- 单支基金的不同季度持仓变化情况
- 多支基金的十大持仓的历史信息

效果如下：

![单支基金](https://tva1.sinaimg.cn/large/008i3skNgy1gu83t22r3yj616q0u0q7z02.jpg)

![多支基金](https://tva1.sinaimg.cn/large/008i3skNgy1gu83t2qwfdj61e40lydmm02.jpg)


## 02 akshare 介绍

与 [Tushare](https://mp.weixin.qq.com/s/c1ukemeK12flCgA-lo69fA) 类似， akshare 是国内比较的优秀的财经数据工具。

AKShare 是基于 Python 的财经数据接口库, 目的是实现对股票、期货、期权、基金、外汇、债券、指数、加密货币等金融产品的基本面数据、实时和历史行情数据、衍生数据从数据采集、数据清洗到数据落地的一套工具。

在使用 akshare 之前，需要进行安装，使用命令如下：

`pip install akshare`

在安装好 akshare 后，导入相关 Python库，如下：

```python
import akshare as ak
import pandas as pd
import numpy as np

print(f'pandas version: {pd.__version__}')
print(f'akshare version: {ak.__version__}')

# pandas version: 1.3.2
# akshare version: 1.3.41
```

>**2022.1.10 更新内容：**
>
>本文中， akshare 使用的版本为 1.3.41


## 03 单支基金

对于单支基金的持仓情况变化，主要是通过对比临近的两个季度，来观察基金持仓的股票增持与减持情况，从而来跟踪基金的趋势变化。

由于基金季度报告中，1季度和3季度只披露前十大股票的持仓情况，而半年度（Q2）和年度（Q4）报告会披露全部持仓的情况，因此，也可以通过对更详细信息的对比来挖掘有些趋势信息。

下面，将详细的介绍获取并对比两个季度的持仓数据。

### 分步骤介绍

首先，咱们通过 akshare 来获取基金的季度持仓数据，以 易方达蓝筹精选基金（基金代码：005827）为例，获取2019-2021年的持仓数据情况，代码如下：

```python
# 易方达蓝筹精选，005827
# 前海开源公共事业股票，005669
years = ['2019','2020','2021']

data = pd.DataFrame()
for yr in years:
    # df_tmp = ak.fund_em_portfolio_hold(code="005827",year=yr) # akshare version: 1.0.91
    df_tmp = ak.fund_portfolio_hold_em(code="005827",year=yr) # akshare version: 1.3.41
    data = data.append(df_tmp)

data['季度']=data['季度'].apply(lambda x:x[:6])
data['季度'] = data['季度'].str.replace('年','Q')
data['占净值比例'] = pd.to_numeric(data['占净值比例'])
data
```

>**2022.1.10 更新内容：**
>
>基金持仓数据获取的接口，在 akshare 1.3.41 版本中，更新为 `fund_portfolio_hold_em`


结果如下：

![基金持仓历史数据](https://tva1.sinaimg.cn/large/008i3skNgy1gu84n6rdddj60qm0lk41e02.jpg)

假设咱们需要对比2021年1季度和2021年2季度的持仓变化情况，则只需要在上面的数据上选择 `2021Q1` 和 `2021Q2` 即可。

数据选取如下：


```python
s1 = '2021Q1'
s2 = '2021Q2'
s1_share = s1+'持股数'
s2_share = s2+'持股数'
s1_value = s1+'持仓市值'
s2_value = s2+'持仓市值'
s1_ratio = s1+'持仓比例'
s2_ratio = s2+'持仓比例'

df1 =data[data['季度']==s1]
df1 = df1[['股票代码', '股票名称','持股数','持仓市值','占净值比例']]
df1 = df1.rename(columns={'持股数':s1_share,'持仓市值':s1_value,'占净值比例':s1_ratio})
df2 =data[data['季度']==s2]
df2 = df2[['股票代码', '股票名称','持股数','持仓市值','占净值比例']]
df2 = df2.rename(columns={'持股数':s2_share,'持仓市值':s2_value,'占净值比例':s2_ratio})
df1
```

2021年1季度数据结果如下：

![2021Q1](https://tva1.sinaimg.cn/large/008i3skNgy1gu84rm96bjj60q00qmn0n02.jpg)


在获取了两个季度的数据后，将数据按 "股票代码" 进行拼接，在得到的 dataframe 中，可以对持股的增减情况进行判断。

需要注意的是，由于Q2 和 Q4，即半年度和年度报告，是需要披露全部持仓的，因此在拼接后的dataframe 中，对应列的 NaN 数据应为 0，这一点可以细细品味下。

```python
df_merge = pd.merge(df1,df2,on='股票代码',how='outer')

# Q2 和 Q4，即半年度和年度报告，是需要披露全部持仓的
# 合并后，在dataframe 中 NaN 的数据应为 0 
if s1.endswith('Q2') or s1.endswith('Q4'):
    df_merge[s1_share] = df_merge[s1_share].fillna(0)
    df_merge[s1_value] = df_merge[s1_value].fillna(0)
    df_merge[s1_ratio] = df_merge[s1_ratio].fillna(0)

if s2.endswith('Q2') or s2.endswith('Q4'):
    df_merge[s2_share] = df_merge[s2_share].fillna(0)
    df_merge[s2_value] = df_merge[s2_value].fillna(0)
    df_merge[s2_ratio] = df_merge[s2_ratio].fillna(0)


df_merge['持股数变化'] = df_merge[s2_share] - df_merge[s1_share]
df_merge = df_merge.sort_values(s2_value,ascending=False)


df_merge['股票名称'] = df_merge['股票名称_y']
# df_merge['股票名称'] = df_merge['股票名称'].fillna('0')
# df_merge.loc[df_merge['股票名称']=='0','股票名称'] = df_merge.loc[df_merge['股票名称']=='0','股票名称_x']
df_merge.loc[df_merge['股票名称'].isna(),'股票名称'] = df_merge.loc[df_merge['股票名称'].isna(),'股票名称_x']
df_merge = df_merge[['股票代码', '股票名称', s1_share, 
                     s1_value, s1_ratio, 
                     s2_share,s2_value, 
                     s2_ratio, '持股数变化']]

df_merge
```

结果如下：

![持仓对比](https://tva1.sinaimg.cn/large/008i3skNgy1gu850wevapj61aa0m2dk202.jpg)

上面的结果中，如果没有对 Q2 和 Q4 季度进行判断，则上图中红框内的数据会是 `NaN`，同时也计算不出减持的结果（结果会显示 `NaN`）。

因此，在进行调整后，可以真实的看到基金减持的情况。在2021年2季度，易方达蓝筹精选基金清仓了中炬高新等股票。

针对上述表格，咱们也可以通过 pandas 的[样式设置](https://mp.weixin.qq.com/s/uaYAQj69BCx5EqcxL4uqoA)，来较为清晰的查看基金的增减持情况，代码如下：


```python
format_dict = {s1_share: '{0:.2f}', s2_share: '{0:.2f}', 
               s1_value: '{0:.0f}', s2_value: '{0:.0f}',
               s1_ratio: '{0:.2f}', s2_ratio: '{0:.2f}', 
               '持股数变化': '{0:.2f}'}

df_merge.style.hide_index()\
                .hide_columns(['股票代码'])\
                .format(format_dict)\
                .bar(subset=['持股数变化'],color=['#99ff66','#ee7621'],align='mid')
```

由于数据行数较多，只截取了部分，效果如下：

![持仓对比](https://tva1.sinaimg.cn/large/008i3skNgy1gu858o7nuyj619c0lq0x102.jpg)


在上面的表格中，`nan` 数据结果，是由于数据披露的情况，有些数据咱们是获取不到的，因此也计算不了持仓增减情况，这些数据，在实际判断中，意义是不大的。

因此，咱们在显示数据结果的时候，也可以选择隐藏这些数据，如下：

```python
df_merge_update = df_merge[~(df_merge['持股数变化'].isna())]
df_merge_update.style.hide_index()\
                .hide_columns(['股票代码'])\
                .format(format_dict)\
                .bar(subset=['持股数变化'],color=['#99ff66','#ee7621'],align='mid')
```

结果如下：

![隐藏nan值](https://tva1.sinaimg.cn/large/008i3skNgy1gu858qi25vj618a0qyn2d02.jpg)

这样，咱们就可以得到，从两个季度信息对比，可以确定的增减持股票情况了（注意，不是全部的增减持，因为有些季度，有些持仓数据是不披露的）。

关于 Pandas表格样式设置的详细内容，请前往下面的链接：

- [Pandas表格样式配置指南](https://mp.weixin.qq.com/s/uaYAQj69BCx5EqcxL4uqoA)



### 函数封装

将上面的详细过程封装成一个自定义函数，这样咱们就可以在后续重复使用该函数来获取不同基金的持仓变化信息了。

函数封装如下：

```python
def compare(code,years,s1,s2):
    """
    code,str,基金代码;
    years,list,年份列表,['yr1','yr2','……'];
    s1,str,靠前的季度, 格式为 'YYYYQ1',例如: '2021Q2';
    s2,str,靠后的季度, 格式为 'YYYYQ1',例如: '2021Q2';
    注意，s1和s2的年份应在 years 里
    """
    
    s1_share = s1+'持股数'
    s2_share = s2+'持股数'
    s1_value = s1+'持仓市值'
    s2_value = s2+'持仓市值'
    s1_ratio = s1+'持仓比例'
    s2_ratio = s2+'持仓比例'

    data = pd.DataFrame()
    for yr in years:
        # df_tmp = ak.fund_em_portfolio_hold(code=code,year=yr) # akshare version: 1.0.91
        df_tmp = ak.fund_portfolio_hold_em(code=code,year=yr) # akshare version: 1.3.41
        data = data.append(df_tmp)

    data['季度']=data['季度'].apply(lambda x:x[:6])
    data['季度'] = data['季度'].str.replace('年','Q')
    data['占净值比例'] = pd.to_numeric(data['占净值比例'])

    df1 =data[data['季度']==s1]
    df1 = df1[['股票代码', '股票名称','持股数','持仓市值','占净值比例']]
    df1 = df1.rename(columns={'持股数':s1_share,'持仓市值':s1_value,'占净值比例':s1_ratio})
    df2 =data[data['季度']==s2]
    df2 = df2[['股票代码', '股票名称','持股数','持仓市值','占净值比例']]
    df2 = df2.rename(columns={'持股数':s2_share,'持仓市值':s2_value,'占净值比例':s2_ratio})
    
    df_merge = pd.merge(df1,df2,on='股票代码',how='outer')

    # Q2 和 Q4，即半年度和年度报告，是需要披露全部持仓的
    # 合并后，在dataframe 中 NaN 的数据应为 0 

    if s1.endswith('Q2') or s1.endswith('Q4'):
        df_merge[s1_share] = df_merge[s1_share].fillna(0)
        df_merge[s1_value] = df_merge[s1_value].fillna(0)
        df_merge[s1_ratio] = df_merge[s1_ratio].fillna(0)

    if s2.endswith('Q2') or s2.endswith('Q4'):
        df_merge[s2_share] = df_merge[s2_share].fillna(0)
        df_merge[s2_value] = df_merge[s2_value].fillna(0)
        df_merge[s2_ratio] = df_merge[s2_ratio].fillna(0)


    df_merge['持股数变化'] = df_merge[s2_share] - df_merge[s1_share]
    df_merge = df_merge.sort_values(s2_value,ascending=False)


    df_merge['股票名称'] = df_merge['股票名称_y']
    # df_merge['股票名称'] = df_merge['股票名称'].fillna('0')
    # df_merge.loc[df_merge['股票名称']=='0','股票名称'] = df_merge.loc[df_merge['股票名称']=='0','股票名称_x']
    df_merge.loc[df_merge['股票名称'].isna(),'股票名称'] = df_merge.loc[df_merge['股票名称'].isna(),'股票名称_x']
    df_merge = df_merge[['股票代码', '股票名称', s1_share, 
                         s1_value, s1_ratio, 
                         s2_share,s2_value, 
                         s2_ratio, '持股数变化']]
    return df_merge
```

>**2022.1.10 更新内容：**
>
>基金持仓数据获取的接口，在 akshare 1.3.41 版本中，更新为 `fund_portfolio_hold_em`

自定义函数 `compare()` 涉及4个参数，参数的说明如下：

- code,str,基金代码;
- years,list,年份列表,['yr1','yr2','……'];
- s1,str,靠前的季度, 格式为 'YYYYQ1',例如: '2021Q2';
- s2,str,靠后的季度, 格式为 'YYYYQ1',例如: '2021Q2';
- 注意，s1和s2的年份应在 years 里

使用该自定义函数之前，先要设置好参数的具体数值，还是以 易方达蓝筹精选基金（基金代码：005827）为例，获取 2020Q3 和 2021Q1 的持仓数据情况，代码如下：

```python
code = "005827"
years = ['2020','2021']
s1 =  '2020Q3' 
s2 = '2021Q1'
s1_share = s1+'持股数'
s2_share = s2+'持股数'
s1_value = s1+'持仓市值'
s2_value = s2+'持仓市值'
s1_ratio = s1+'持仓比例'
s2_ratio = s2+'持仓比例'

df_merge = compare(code,years,s1,s2)

format_dict = {s1_share: '{0:.2f}', s2_share: '{0:.2f}', 
               s1_value: '{0:.0f}', s2_value: '{0:.0f}',
               s1_ratio: '{0:.2f}', s2_ratio: '{0:.2f}', 
               '持股数变化': '{0:.2f}'}

df_merge.style.hide_index()\
                .hide_columns(['股票代码'])\
                .format(format_dict)\
                .bar(subset=['持股数变化'],color=['#99ff66','#ee7621'],align='mid')

```

![demo](https://tva1.sinaimg.cn/large/008i3skNgy1gu85o785rgj616v0u079y02.jpg)

从数据计算结果来看，有 `nan` 的数据行，是不能直接判断出结果的。

但如果是在最新的季度出现，并且是在十大持仓里靠前的位置，大概率是增值了，或者是完全新增的股票持仓。

比如下面的招行：

![nan值](https://tva1.sinaimg.cn/large/008i3skNgy1gu85qfnerzj616x0u0wjw02.jpg)

>注：一般是直接比较两个临近的季度，这里的 2020Q3 和 2021Q1 之间，还隔个一个 2020Q4 。



## 04 多支基金

同时获取多支基金的十大股票持仓信息时，在这里只获取每个季度股票名称的信息。

首先，来获取单支基金的十大持仓股票名称信息，自定义函数如下：

```python
# 获取单只基金的十大股票名称信息
def fund_stock_holding(years,code):
    data = pd.DataFrame()
    for yr in years:
        # df_tmp = ak.fund_em_portfolio_hold(code=code,year=yr) # akshare version: 1.0.91
        df_tmp = ak.fund_portfolio_hold_em(code=code,year=yr) # akshare version: 1.3.41
        data = data.append(df_tmp)

#     data['季度']=data['季度'].apply(lambda x:x[:8])
    data['季度']=data['季度'].apply(lambda x:x[:6])
    data['季度'] = data['季度'].str.replace('年','Q')
    data['占净值比例'] = pd.to_numeric(data['占净值比例'])    
    data = data.sort_values(['季度','持仓市值'],ascending=[True,False])
    
    # 将序号按持仓市值排序，从大到小    
    data1 = pd.DataFrame()
    quarter = data['季度'].unique().tolist()
    for q in quarter:
        df_tmp = data.query('季度==@q')
        df_tmp['序号'] = list(range(1,len(df_tmp)+1)) 
        data1 = data1.append(df_tmp)
        
    df = data1.set_index(['序号','季度']).stack().unstack([1,2]).head(10)
    # df1.loc[:,(slice(None), ['股票名称','占净值比例'])]
    df = df.loc[:,(slice(None), '股票名称')]  # 只选取 股票名称
    df = df.droplevel(None,axis=1)
    df.columns.name=None
    df = df.reset_index()
#     df.index.name = None
    df['基金代码'] = code
    cols = df.columns.tolist()
    cols = cols[:1] + cols[-1:] + cols[1:-1]  # 将基金代码列名放前面
    df = df[cols]
    return df
```

>2022.1.10 更新内容：
>
>基金持仓数据获取的接口，在 akshare 1.3.41 版本中，更新为 `fund_portfolio_hold_em`

>**2022年3月 更新内容：**
>
>自定义函数 `fund_stock_holding` 关于股票持仓市值的排序，之前的版本存在一些问题，此次更新计算持仓市值的序号。

以 易方达蓝筹精选基金（基金代码：005827）为例，该基金2019-2021的十大持仓信息如下：

```python
years = ['2020','2021']
df3 = fund_stock_holding(years,'005827')
df3
```

>**2022年3月 更新内容：**
>
>年份只选取 2020 和 2021年，后面的图片内容同步更新

结果如下：


![单支基金持仓信息](https://tva1.sinaimg.cn/large/e6c9d24egy1gzprj9916hj215o0ien1m.jpg)

通过自定义函数 `fund_stock_holding(years,code)` 可以同时获取多支基金的十大持仓信息，代码如下：

```python
codes = ['005669','006281','003956','001532',
         '001714','001054','001809','003853',
         '004890','001125','001808']
df = pd.DataFrame()
for code in codes:
    df_tmp = fund_stock_holding(years,code)
    df = df.append(df_tmp)
df
```

结果如下：


![多支基金持仓信息](https://tva1.sinaimg.cn/large/e6c9d24egy1gzprjbjx0gj21440lkaeo.jpg)

上面数据中，显示的是基金代码，一般情况下，咱们还是习惯看基金名称，因此可以把基金代码换成基金名称。

基金名称信息获取如下：

```python
# 获取基金名称信息
df_fund_info = ak.fund_em_fund_name()
df_fund_info = df_fund_info.query('基金代码 in @codes')
df_fund_info = df_fund_info[['基金代码','基金简称']]
df_fund_info = df_fund_info.reset_index(drop=True)
df_fund_info
```

效果如下：

![基金名称信息](https://tva1.sinaimg.cn/large/008i3skNgy1gu8fnwecpaj60ee0k6jsx02.jpg)

有了基金名称信息后，可以用 pandas 通过拼接的方式，将基金名称信息显示出来，代码如下：

```python
df_fund = pd.merge(df,df_fund_info,on='基金代码')
cols = df_fund.columns.tolist()
cols = cols[:1] + cols[-1:] + cols[2:-1]  # 将基金简称放前面
df_fund =df_fund[cols]
df_fund
```

结果如下：


![多支基金持仓信息](https://tva1.sinaimg.cn/large/e6c9d24egy1gzprjdyz45j21ay0m4q8o.jpg)



## 05 小结

至此，咱们就获取了本次想要得到的信息。针对单支基金或者多支基金，咱们可以定期来获取信息，可以方便快捷的给其他人员分享基金的持仓信息，以及增减持情况。

在今天的这个财经案例里，需求其实是很常见的。在这个案例里，主要用到的是 Pandas 和 akshare 工具。

对于 Pandas 而言，用到的知识点还是挺多的，包括数据筛选、数据合并、数据排序、字符换和数值转换、样式设置等，虽然都是基础知识点，但要做到熟练运用，也是需要一定功底的。

财经工具，本次主要使用的是 akshare，使用 [tushare](https://mp.weixin.qq.com/s/c1ukemeK12flCgA-lo69fA) 或者其他的财经工具应该也是可以的。

在代码编写好后，复用性还是挺好的。

我自己也是经常性在另一个公号「价值前瞻」里分享读书和投资的一些经验与教训。

本文的代码，大家可以点击下面的公众号卡片，回复 “**pyfin**” 来获取代码。

<div align="center">
    <img src="/images/qrcode_valuelab.jpg" width="20%">
</div>


---

**风险提示：** 文章主要目的是给大家分享 Python 在财经领域的应用，文中提到的品种或标的，仅作为文中技术实现之用。投资有风险，入市需谨慎，文中内容不构成投资建议，抄作业请理性分析市场。


