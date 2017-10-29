---
layout: posts
title: "福布斯系列(9)-数据清洗(e)"
teaser:
date: 2017-09-18
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



**福布斯系列之数据清洗（5） - Python数据分析项目实战**

## 1 前言



本文作为 **数据清洗的第五篇**，内容包含：

（1） **2016-2017年** 数据的初步处理过程。

（2）将2007-2017年数据合并，并统一国家名称



**本项目运行环境：**

* windows7
* jupyter notebook
* Python 3.5


## 2 2016-2017年数据处理

2016年数据的清洗，前文已做详细描述。

2017年的数据信息，跟2016年基本一致。具体过程就不描述了。 代码如下：

```python
df_2017 = pd.read_csv('./data/data_forbes_2017.csv', encoding='gbk')

# 更新列名
df_2017.columns = ['Year', 'Rank', 'Company_cn','Company_en',
                   'Country_en', 'Sales', 'Profits', 'Assets', 'Market_value']
print('the shape of DataFrame: ', df_2017.shape)
df_2017.head()

cols = ['Sales', 'Profits', 'Assets', 'Market_value']
for col in cols:
    pro_col(df_2017, col)
print('the shape of DataFrame: ', df_2017.shape)
print(df_2017.dtypes)
df_2017.head()

# 添加空白列
df_2017['Company_cn_en'],df_2017['Country_cn_en'], df_2017['Country_cn'], df_2017['Industry_cn'], df_2017['Industry_en'] = ['','','','','']

# 按指定list重新将columns进行排序
columns_sort = ['Year', 'Rank', 'Company_cn_en','Company_en',
                'Company_cn', 'Country_cn_en', 'Country_cn',
                'Country_en', 'Industry_cn', 'Industry_en',
                'Sales', 'Profits', 'Assets', 'Market_value']
df_2017 = df_2017.reindex(columns=columns_sort)
print(df_2017.shape)
df_2017.head()
```

其中，函数`pro_col()`来自2016年处理过程的函数。









## 3 2007-2017年数据的合并

用pandas的`concat()`方法，将2007年至2017年的数据进行合并，统一到一个Dataframe中。

当然，也可以保存到csv文件中，以便后续分析时使用。

```python
df_concat = pd.concat([df_2007, df_2008, df_2009,
                       df_2010, df_2011, df_2012,
                       df_2013_all, df_2014,
                       df_2015, df_2016, df_2017], ignore_index=True)
# 保存数据                       
# df_concat.to_csv('data_forbes_concat.csv')
df_concat.head()
```

## 4 国家名称的统一

福布斯全球上市企业排行榜数据，我们获得的数据相对比较杂乱。

我们希望将国家名称进行规范，这里以国家英文名称的列"Country\_en"作为清洗的目标。

国家英文名称中，不规范的情况包括：
1. 有些样本中国家的英文名称为空白
1. 有些是中文名称
1. 有些一个国家对应多个英文名称
1. 有些一个缩写对应多个不同的国家
1. 一家企业可能在多个国家有登记


### 4.1 国家英文名称为空值的处理

若 "Country\_en"列有空值，则用"Conutry\_cn"列的值替换


```python
df_concat.loc[df_concat['Country_en']=='', 'Country_en'] = df_concat.loc[df_concat['Country_en']=='', 'Country_cn']
```

### 4.2 将国家名称统一为英文

将国家名称统一成英文名称，这里主要用的是`replace()`方法

```python
list_origin = [']','AR', 'AS','AU', 'US', 'BE', 'BR', 'BS', 'BU', 'CA', 'CH', 'CI',
              'CN-HK', 'CN-TA', 'CO', 'CZ', 'DE', 'EG', 'FI', 'FR', 'GE',
              'GR', 'Hong Kong', 'Hong Kong/China', 'HU', 'IC', 'ID', 'IN', 'IR',
              'IS','IT', 'JA', 'JO', 'KO','LI', 'LU', 'MX', 'NL', 'NO', 'NZ',
               'PA', 'PE', 'PH', 'PK', 'PL', 'PO', 'RU', 'SI', 'SP', 'SU',
              'SW', 'SZ', 'Taiwan', 'TH', 'TU', 'UK', 'VE', '阿根廷', '阿联酋',
              '阿曼', '埃及', '爱尔兰', '奥地利', '澳大利亚', '巴基斯坦', '巴林',
              '巴拿马', '巴西', '百慕大', '比利时', '波多黎各', '波兰', '丹麦',
              '德国', '多哥', '俄罗斯', '法国', '菲律宾', '芬兰', '哥伦比亚',
              '哈萨克斯坦', '海峡群岛', '韩国', '荷兰', '加拿大', '捷克共和国',
              '卡塔尔', '开曼群岛', '科威特', '克罗地亚', '黎巴嫩', '利比里亚',
              '列支敦士登', '列支敦斯登', '卢森堡', '马来西亚', '毛里求斯',
              '美国', '秘鲁', '摩洛哥', '墨西哥', '南非', '尼日利亚', '挪威',
              '葡萄牙', '日本', '瑞典', '瑞士', '塞浦路斯', '沙特$',
              '沙特阿拉伯', '斯洛伐克', '泰国', '土耳其', '委内瑞拉', '西班牙',
              '希腊', '新加坡', '新西兰', '匈牙利', '以色列', '意大利',
              '印度$', '印度尼西亚', '印尼', '英国', '约旦', '越南', '智利',
              '中国大陆', '中国台湾', '中国香港', 'CN$', '中国$', 'Netherlands']

list_replace = ['','Argentina', 'Austria','Australia', 'United States', 'Belgium',
                'Brazil', 'Bahamas', 'Bermuda', 'Canada', 'Chile', 'Cayman Islands',
               'China-HongKong', 'China-Taiwan', 'Colombia', 'Czech Republic',
               'Denmark', 'Egypt', 'Finland', 'France', 'Germany', 'Greece', 'China-HongKong',
               'China-HongKong', 'Hungary', 'Iceland', 'Indonesia', 'India', 'Ireland', 'Israel',
               'Italy', 'Japan', 'Jordan', 'South Korea','Liberia', 'Luxembourg', 'Mexico',
               'Netherland', 'Norway', 'New Zealand', 'Panama', 'Peru', 'Philippines',
               'Pakistan', 'Poland', 'Portugal', 'Russia', 'Singapore', 'Spain',
               'Saudi Arabia', 'Sweden', 'Switzerland', 'China-Taiwan',
               'Thailand', 'Turkey', 'United Kingdom', 'Venezuela', 'Argentina',
               'United Arab Emirates', 'Oman', 'Egypt', 'Ireland', 'Austria', 'Australia',
               'Pakistan', 'Bahrain', 'Panama', 'Brazil', 'Bermuda', 'Belgium',
               'Puerto Rico', 'Poland', 'Denmark', 'Germany', 'Togo', 'Russia',
               'France', 'Philippines', 'Finland', 'Colombia', 'Kazakhstan',
               'Channel Islands', 'South Korea', 'Netherland', 'Canada',
               'Czech Republic', 'Qatar', 'Cayman Islands', 'Kuwait', 'Croatia', 'Lebanon',
               'Liberia', 'Liechtenstein', 'Liechtenstein', 'Luxembourg', 'Malaysia',
               'Mauritius', 'United States', 'Peru', 'Morocco', 'Mexico', 'South Africa',
               'Nigeria', 'Norway', 'Portugal', 'Japan', 'Sweden', 'Switzerland', 'Cyprus',
               'Saudi Arabia', 'Saudi Arabia', 'Slovakia', 'Thailand', 'Turkey', 'Venezuela',
               'Spain', 'Greece', 'Singapore', 'New Zealand', 'Hungary', 'Israel', 'Italy',
               'India', 'Indonesia', 'Indonesia', 'United Kingdom', 'Jordan', 'Vietnam',
               'Chile',  'China', 'China-Taiwan', 'China-HongKong', 'China', 'China',
               'Netherland']

df_concat['Country_en'] = df_concat['Country_en'].replace(list_origin,list_replace, regex=True)
```

**两个list数据的录入，没有捷径，自己录入的。**

### 4.3 一家公司属于多个国家的情况

进一步替换，包括一家公司属于多个国家的情况。


```python
# 进一步替换，包括一家公司属于多个国家的情况
# 注意这里没有 regex=True
df_concat['Country_en'] = df_concat['Country_en'].replace(['China-China-Taiwan',r'China-HongKong/China', r'Australia)/United Kingdom(United Kingdom',
                                                          r'Netherland)/United Kingdom(United Kingdom', r'Panama)/United Kingdom(United Kingdom',
                                                          r'SA)/United Kingdom(United Kingdom', r'United Kingdom)/Australia(SA', r'United Kingdom)/Netherland(Netherland',
                                                          r'United Kingdom)/South Africa(SA' , 'Netherland/United Kingdom', 'Panama/United Kingdom',
                                                          'Australia/United Kingdom'],
                                                          ['China-Taiwan', 'China-HongKong', 'United Kingdom/Australia', 'United Kingdom/Netherland',
                                                          'United Kingdom/Panama', 'United Kingdom/Australia', 'United Kingdom/Australia',
                                                          'United Kingdom/Netherland', 'United Kingdom/South Africa', 'United Kingdom/Netherland',
                                                          'United Kingdom/Panama', 'United Kingdom/Australia'])
```




### 4.4 同一个英文缩写代表不同国家的情况

通过观察数据，发现有一类特殊情况，同样的英文缩写，代表的国家可能不同，需要进一步处理。

如 **SA可以代表Australia 或 South Africa**，MA可以代表 Malaysia 或 Morocco

处理这类特殊情况的思路与步骤为：

（a）将此类英文缩写用空白值替换

（b）将空白值替换为当前样本的中文国家名称

（c）将中文国家名称替换为各自的英文国家名称


```python
# 注意需要以 "$"结尾
df_concat['Country_en'] = df_concat['Country_en'].replace(['MA$','SA$'],['', ''], regex=True)

df_concat.loc[df_concat['Country_en']=='', 'Country_en'] = df_concat.loc[df_concat['Country_en']=='', 'Country_cn']

df_concat['Country_en'] = df_concat['Country_en'].replace(['澳大利亚','马来西亚', '摩洛哥', '南非'],
                                                          ['Australia', 'Malaysia', 'Morocco', 'South Africa'])

```



最后，保存数据


```python
df_concat.to_csv('data_forbes_concat.csv')
```



## 5 总结

2016年至2017年的数据清洗，前文已作详细描述，本文主要是描述国家名称的统一处理。

至此，福布斯全球上市企业排行榜数据（2007年至2017年）的清洗告一段落。

结合前文2007年-2015年的数据清洗，将2007-2017年的数据清洗的代码合并到一起，感兴趣的同学可以回复关键字获取相关代码。

>关注微信公众号公众号"Python数据之道"，后台回复"**PyDataRoad**"，获取本文的源代码及原始数据文件。

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
