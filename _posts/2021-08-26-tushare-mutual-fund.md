---
layout: posts
title: "用 Python 快速追踪基金的收益情况"
teaser:
date: 2021-08-26
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PyFinance
tags:    
   - Tushare
   - Pandas    
comments: true
show_meta: true
sidebar: right
authors: ["阳哥"]
---

{% include alert info='用 Python 快速追踪基金的收益情况' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>


# 用 Python 快速追踪基金的收益情况 | Python财经实践


大家好，我是阳哥。

在文章 [《股票亏惨了，用Python来做一个投资计划》](https://mp.weixin.qq.com/s/WYuMwCJBrWaBiDs8xp2KMA) 发布后，有同学留言，希望能够有更多的关于 Python 在财经领域应用的案例分享。

其实，在财经领域，我之前也是有过部分内容分享的：

- [视频：Plotly 和 Dash 在投资领域的应用](https://mp.weixin.qq.com/s/VopPrpe_64j5za4AYkmUTg)

- [视频：Plotly中绘制股票交易图表](https://mp.weixin.qq.com/s/1TUB2G-xavNm796uXUnRfQ)

- [Plotly中绘制三种经典的股票交易图表](https://mp.weixin.qq.com/s/1v3h5-5fNVk7cBdaRKzJkg)

- [神器Tushare，财经数据必备工具！](https://mp.weixin.qq.com/s/c1ukemeK12flCgA-lo69fA)

- [用 Python 读取巴菲特近期持仓数据](https://mp.weixin.qq.com/s/6h9Xq2lfrZSlRNw_W8aWcg)

大家知道，近几年，不少同学都是经由基金进入到股市中的。去年就很流行“买股不如买基”的说话，至于股票和基金到底谁更好，这个仁者见仁智者见智，恐怕一时半会儿也说不清楚。

今天，阳哥给大家分享的主题是 用 Python 来追踪和更新基金的收益情况，涉及到的Python库主要是 pandas 和 tushare 。

最终实现的效果如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gtt3x6riggj60xi0u0wn802.jpg)

上面表格中的信息，主要涉及四个方面：

1. 基金基础信息，包括基金名称、基金费率、基金公司、基金成立时间等；
2. 基金经理的信息，包括姓名、性别、任职时间等；
3. 基金规模，体现出来的是基金金额的规模，是通过基金份额以及基金净值计算出来的；
4. 基金年度收益情况，一般是计算基金近几年的收益情况。

上面的这些信息，在财经工具 tushare 中，目前是都已经提供了的。因此，咱们有必要稍微介绍下 tushare 。


## 01 tushare 介绍

关于财经数据，有多个Python库可以供咱们选择，其中 tushare 是国内较早开始发布财经数据的社区，其内容比较完善，今天我们使用的就是 tushare 。

Tushare 是一个金融大数据平台，数据内容包含股票、指数、基金、期货、债券、外汇、行业大数据等，同时包括了数字货币行情等区块链数据，为各类金融投资和研究人员提供适用的数据和工具，概览如下：

![tushare概览](https://tva1.sinaimg.cn/large/008eGmZEgy1gn3j7d8wnwj30is0ya45g.jpg)

全部内容很丰富，为了有助于大家有个整体的了解，阳哥绘制了一张完整的思维导图，截图如下：

![tushare思维导图](https://tva1.sinaimg.cn/large/008eGmZEgy1gn3r6vz09nj30u016r7mw.jpg)


### 使用 Tushare

Tushare 平台的数据，已全面升级到 tushare pro 了，通常情况下，还是称之为 tushare。

想使用 tushare 中的数据和功能，首先需要进行注册，获得一份 token （一串字母和数字组成的文本），然后才可以获取到数据，大家可以通过以下链接来注册（也可以点击文末的“**阅读原文**”）：

[https://tushare.pro/register?reg=129033](https://tushare.pro/register?reg=129033)

在 `tushare` 中注册后，通过 “个人主页”——“接口TOKEN” 可以找到自己的 token 值，界面如下：

<!-- ![tushare](images/posts/2020-04-27-plotly-txt/2.png) -->

![tushare接口TOKEN](https://tva1.sinaimg.cn/large/008eGmZEgy1gmwqwnfvdsj30l60b8aar.jpg)

复制 token 值，然后在代码中进行如下设置：

```python
# 设置 token
# tushare 注册地址： https://tushare.pro/register?reg=129033
# 以上方法只需要在第一次或者token失效后调用，完成调取tushare数据凭证的设置，正常情况下不需要重复设置。
ts.set_token('你的token值')

pro = ts.pro_api()
```

在设置好 token 值后，我们就可以开始获取数据。


关于 tushare 的详细介绍，请点击下面的链接前往：

- [神器Tushare，财经数据必备工具！](https://mp.weixin.qq.com/s/c1ukemeK12flCgA-lo69fA)

## 02 基金信息获取

首先，导入本次需要用到的python库，并设置好tushare，如下：

```python
import pandas as pd
import datetime
import numpy as np
import tushare as ts

# token='XXXXXXX'  
# ts.set_token(token)

pro = ts.pro_api()
```

>需要说明的是，tushare 中不同的数据获取需要的权限是不一样的，如果权限不够，可能获取不到数据哦。

### 获取基金基础信息

首先，咱们来获取基金基础信息，包括基金名称、基金费率、基金公司、基金成立时间等。在 tushare 中，提供了 `fund_basic()` 接口来获取这些信息。

![](https://tva1.sinaimg.cn/large/008i3skNgy1gtt6yv58qzj613k09a75302.jpg)

该接口有两个参数，咱们主要关注的是第一个参数 `market`，由于基金有场内基金（ETF和LOF）和场外基金，因此，全部的基金是这两者之和。

所有基金的基础信息获取代码如下：

```python
# 获取基金中文名称信息
df_e = pro.fund_basic(market='E')
df_o = pro.fund_basic(market='O')
df_fund_info = df_e.append(df_o)
df_fund_info = df_fund_info[['ts_code','name', 'management','m_fee', 'c_fee','found_date']]
df_fund_info.columns = ['fund_code','fund_name', 'management','m_fee', 'c_fee','found_date']
df_fund_info = df_fund_info.reset_index(drop=True)
df_fund_info
```

结果如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gtt72png96j60vw0m2ad202.jpg)

目前整个市场上有 1.5万多支基金，是不是超出你的想象了啊~~~

当然了，这是包括了混合型、股票型、债券型、货币基金类等全部的基金。


### 获取基金经理信息

对于主动型基金而言，挑选基金时，选择一个好的基金经理是很重要的因素，因此，在追踪基金的收益情况时，是有必要将基金经理的信息一并获取的。

在tushare 中，可通过接口 `fund_manager` 来获取某只基金过往基金经理的信息。

在获取基金经理信息后，可以跟之前获取基金基础信息进行合并。

这个过程，阳哥写了一个自定义函数，如下：

```python
# 获取单个基金的信息

def get_fund(fund_code,df_fund_info):    
    # 获取基金经理信息，以及开始管理该基金的日期
    df_manager = pro.fund_manager(ts_code=fund_code)
    df_manager = df_manager[df_manager['end_date'].isna()]
    df_manager = df_manager.sort_values('begin_date',ascending=True).head(1)
    begin_date = df_manager['begin_date'].values[0] # 开始管理该基金的日期
    df_manager = df_manager[['ts_code', 'name', 'gender','begin_date']]
    df_manager.columns = ['fund_code', 'manager_name', 'gender','begin_date']
    
    # 合并
    df_fund = pd.merge(df_manager,df_fund_info,how='left',on='fund_code')

    return df_fund
```

假如你想获取易方达中小盘基金的信息，可以基于上面的自定义函数 `get_fund` ，使用如下的代码：


```python
# 110011.OF,易方达中小盘

df_fund = get_fund('110011.OF',df_fund_info) 
df_fund
```

获取的信息如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gttb998eixj611a05oaan02.jpg)

### 计算基金规模和收益情况

接下来，咱们还需要获取两个信息，基金规模和基金近几年的收益情况。

基金规模，可以从一个角度来观察该基金在市场上的受欢迎度，一般来说，规模较大的基金，说明该基金过往的表现应该还是可以的。不过这里也有两点需要注意：

1. 基金过往业绩并不代表未来依然会如此；
1. 基金规模小，并不一定说明该基金未来表现会不好。

除了基金规模，另一个咱们经常关注的，也是基金营销机构经常拿出来宣传的，就是基金的过往收益情况。

基金规模的计算公式如下：

$$基金规模(亿元) = 基金份额数量 \times 基金单位净值 $$

在 tushare 中 基金份额数量由函数 `fund_share` 来获取，基金单位净值（unit_nav）数据由函数 `fund_nav` 来获取。

此外，在计算基金的过往年度收益时，也是基于其累计净值（accum_nav）来实现的。

因此，阳哥将基金规模获取过程和基金近几年收益情况计算合并在一个自定义函数里，代码如下：

```python
# 获取单支基金的年度收益,基金规模等信息

def get_returns(fund_code,start_year):
    df_fund_nav = pro.fund_nav(ts_code=fund_code)
    df_fund_nav['date'] = pd.to_datetime(df_fund_nav['end_date'])
    df_fund_nav = df_fund_nav.set_index('date').sort_index()
    
    years = df_fund_nav['accum_nav'].resample('AS').sum().to_period('A')
    # 获取年度数据，类型为 pandas 的 period
    years = years.index.tolist()
    
    # 有些基金的开始日期晚于2018年，需要对齐列，补空白
    year_list = [yr.year for yr in years if yr.year>=start_year]
    
    # 将 period 时间数据转为 string 的年度数据
    years = [str(yr.year) for yr in years]

    df_yrs_index = pd.DataFrame()
    for yr in years:
        df_yr_index = df_fund_nav.loc[yr].tail(1)
        df_yrs_index = df_yrs_index.append(df_yr_index)
    df_yrs = df_fund_nav.head(1).append(df_yrs_index)

    # 计算每年的收益率，即涨跌幅度
    df_yrs['returns'] = df_yrs['accum_nav'].pct_change()

    # 删除收益率为 NA 的行 （第1天有数据记录的日期）
    df_yrs = df_yrs.dropna(subset=['returns'])

    # 筛选自开始年份以来的数据
    df_yrs = df_yrs.loc[str(start_year):]

    df_yrs = df_yrs.reset_index()
    df_yrs['year'] = df_yrs['date'].dt.year

    # 透视表
    df_yr_returns = pd.pivot_table(
        df_yrs, index=['ts_code'],
        values=['returns'], columns=['year'], fill_value=""
    )

    # 将多层索引转变为单层索引
    df_yr_returns = df_yr_returns['returns']
    df_yr_returns = df_yr_returns.reset_index()
    df_yr_returns.columns.name = None
    
    df_yr_returns = df_yr_returns.rename(columns={'ts_code':'fund_code'})
    
    # 基金份额
    df_fund_share = pro.fund_share(ts_code=fund_code).head(1)
    df_fund_share.columns = ['fund_code', 'fd_share_date', 'fd_share', 'fund_type', 'market']
    # fd_share,单位是 万份
    fd_share_date = df_fund_share['fd_share_date'].values[0] # 份额对应的日期
    
    # 份额日期的净值数据
    df_ann_nav = df_fund_nav.loc[fd_share_date:fd_share_date].sort_values('update_flag',ascending=False).head(1)
    df_ann_nav = df_ann_nav[['ts_code', 'accum_nav','unit_nav']]
    df_ann_nav.columns = ['fund_code', 'accum_nav','unit_nav']
    
    # 计算基金规模，amount，单位：亿元
    df_fund_amount = pd.merge(df_fund_share,df_ann_nav,how='left',on='fund_code')    
    df_fund_amount['amount'] = df_fund_amount['fd_share'] * df_fund_amount['unit_nav']/10000
    df_fund_amount = df_fund_amount[['fund_code','amount','fd_share_date']]
    
    # 合并数据    
    df_yr_returns = pd.merge(df_fund_amount,df_yr_returns,how='left',on='fund_code')

    for yr in year_list:
        if yr not in df_yr_returns.columns.tolist():
            df_yr_returns[yr]=np.nan
    
    return df_yr_returns
```

假如你想获取易方达中小盘基金的2018年以来的收益情况信息，可以基于上面的自定义函数 `get_returns` ，使用如下的代码：

```python
df_return = get_returns('110011.OF',2018)
df_return
```

获取的信息如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gttbmrnngbj60t404emxj02.jpg)

### 同时获取多只基金的信息

上面已经实现了获取单只基金所需要的信息，接下来，咱们需要拼接之前获取的信息。

同时，我们一般会同时关注多只基金，因此同时获取多只基金的信息，也是基本必备的需求。

实现的代码如下：

```python
# 获取多只基金的信息

def get_data_fund(df_fund_info,fund_code_short,code_exception,start_year):
    df_filter_info = pd.DataFrame()
    for code in fund_code_short:
        df_tmp = df_fund_info[df_fund_info['fund_code'].str.contains(code)].head(1)
        df_filter_info = df_filter_info.append(df_tmp)
    df_filter_info = df_filter_info.reset_index(drop=True)   
    fund_codes = df_filter_info['fund_code'].tolist()
    # fund_codes

    df = pd.DataFrame()
    for code in fund_codes:
        if code in code_exception:
            code_update = code[:-2]+'OF'
            code_short = code[:-3]
            df_fund = get_fund(code_update,df_fund_info)
            df_fund['fund_code'] = code
            df_fund = df_fund[['fund_code','manager_name','gender','begin_date']]
            df_fund = df_fund.dropna(axis=1) # 删除含 NaN 的列
            test_m = df_fund_info[df_fund_info['fund_code'].str.contains(code_short)]
            df_fund = pd.merge(df_fund,test_m,how='left',on='fund_code')
        else:
            df_fund = get_fund(code,df_fund_info) 
        df_return = get_returns(code,start_year)
        df_fund_merge = pd.merge(df_fund,df_return,how='left',on='fund_code')
        df = df.append(df_fund_merge)
    df = df.reset_index(drop=True)
    
    df = df.rename(columns={
        'fund_code':'基金代码','manager_name':'基金经理',
        'gender':'性别','begin_date':'上任日期',
        'fund_name':'基金名称','management':'基金公司',
        'm_fee':'管理费','c_fee':'托管费',
        'found_date':'成立时间','amount':'基金规模(亿元)',
        'fd_share_date':'规模对应日期'
    })
    
    # 调整列的排序
    cols = df.columns.tolist()
    col_1 = cols[:4]
    col_2 = cols[4:5]
    col_3 = cols[5:]
    cols = col_2 + col_1 + col_3
    df = df[cols]

    return df
```

上面的自定义函数 `get_data_fund` 中，存在一些特列，我也进行了调整。

这个特列，主要是有一类基金，叫做LOF基金，场内和场外的数字代码是一样的，比如谢治宇的兴全合润基金，在tushare中，场内的完整代码是163406.SZ，场外的完整代码是163406.OF，对于LOF基金，我们只需要获取一个的信息即可。

同时，由于LOF基金获取信息是基金基础信息和基金经理信息用的代码有些区别，需要分开应用。因此，需要在上面的函数 `get_data_fund` 中进行区别，并且引入一个 `code_exception` 参数。

在上面的函数 `get_data_fund` 中，还对最后显示的列的顺序也进行了调整，该过程的实现的详细内容，可以在下面的链接中来了解：

- [Pandas实用技能，将列（column）排序的几种方法](https://mp.weixin.qq.com/s/utZY96yPQy9uR1o1V7K0xQ)

基于自定义函数 `get_data_fund`，设置好函数所需要的参数后，就可以获取多只基金的信息了，具体参数如下：

```python
# 需要获取的基金代码简称列表
fund_code_short = ['000772', '003095','166005','320007',
                   '163406','260101','161005','162605',
                   '163402','005827','110022','110011',
                   '164908','000800','590008',
                   '360007','118001',
                   '519736','007119','002190','005911',
                   '001938','166002','377240'
                  ]

# ['200011','690003','200010',]

# 特列：LOF 基金，基金经理和管理费等费用所用的代码两处不一致，需要调整
# 输入的是 LOF 基金在场内的完整代码
code_exception = ['163406.SZ','160632.SZ','160222.SZ','159843.SZ','515920.SH'] 

# 设置基金收益开始的年份
start_year = 2018
# 设置基金收益截止的年份
end_year = 2021
year_list = list(range(start_year,end_year+1))

df_fund_final = get_data_fund(df_fund_info,fund_code_short,code_exception,start_year)
df_fund_final
```

得到的结果如下：


![](https://tva1.sinaimg.cn/large/008i3skNgy1gttcz84zq8j61ig0je43v02.jpg)

Pandas 中，可以通过 Style 对表格样式进行设置，对收益情况进行红涨绿跌的设置。

代码如下：

```python
def color_returns(val):
    if val >=0:
        color = '#EE7621' # light red 
    elif val <0:
        color = '#99ff66' # light green 
    else:
        color = '#FFFAFA' # ligth gray 
    return f'background-color: {color}'

format_dict = {'基金规模(亿元)': '￥{0:.1f}', 
               '管理费': '{0:.1f}', 
               '托管费': '{0:.2f}', 
               2017: '{0:.1%}', 
               2018: '{0:.1%}', 
               2019: '{0:.1%}', 
               2020: '{0:.1%}', 
               2021: '{0:.1%}', 
                }
df_fund_final = df_fund_final.sort_values('基金经理',ascending=True)

df_fund_final.style.hide_index()\
                    .hide_columns(['规模对应日期','成立时间'])\
                    .format(format_dict)\
                    .applymap(color_returns,subset=year_list)\
                    .background_gradient(subset=['基金规模(亿元)'],cmap='Blues')
```

得到的结果如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gtt3x6riggj60xi0u0wn802.jpg)

关于 Pandas 中表格样式设置，阳哥之前分享了一份详细的内容，有兴趣的同学可以前往查看：

- [Pandas表格样式配置指南，看这一篇就够了](https://mp.weixin.qq.com/s/uaYAQj69BCx5EqcxL4uqoA)

## 03 小结

至此，咱们就获取了本次想要得到的信息，这个表格可以定期更新，用于给其他人员分享基金的基本信息以及年度收益情况。

在今天的这个财经案例里，需求其实是很常见的。在这个案例里，主要用到的是 Pandas 和 Tushare 工具。

对于 Pandas 而言，用到的知识点还是挺多的，包括数据筛选、数据合并、数据排序、数据透视表、样式设置等，虽然都是基础知识点，但要做到熟练运用，也是需要一定功底的。

对于 tushare ，主要是关注该工具中财经信息的完整性、准确性以及及时性。

如果其他财经工具有这样的功能，也是可以了。


在代码编写好后，复用性还是挺好的。

我自己也是经常性在另一个公号「价值前瞻」里分享读书和投资的一些经验与教训。

本文的代码，大家可以点击下面的公众号，回复 “**pyfin**” 来获取代码。


<div align="center">
    <img src="/images/qrcode_valuelab.jpg" width="20%">
</div>

---

**风险提示：** 文章主要目的是给大家分享 Python 在财经领域的应用，文中提到的品种或标的，仅作为文中技术实现之用。投资有风险，入市需谨慎，文中内容不构成投资建议，抄作业请理性分析市场。
