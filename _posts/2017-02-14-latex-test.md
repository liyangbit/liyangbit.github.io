---
layout: posts
title: "latex test"
teaser:
date: 2017-02-14
header:
   image_fullwidth: "image-head.jpg"
categories:
   - Latex
tags:
   - Latex
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---
**latex test**


<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>


## 行内公式：

行前 $ f(x) = \sum_{i=1}^{N} x_{i} $  行后

## 单独显示公式：





```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

% matplotlib inline
```

# 将数据进行初步整理

## Year 2007

* 2007年的数据，原始数据的单位为十亿美元


```python
df_2007 = pd.read_csv('./data/data_forbes_2007.csv', encoding='gbk', thousands=',')
print('the shape of DataFrame: ', df_2007.shape)
print(df_2007.dtypes)
df_2007.head(3)
```

    the shape of DataFrame:  (2000, 9)
    年份                    int64
    排名(Rank)              int64
    公司名称(Company)        object
    所在国家或地区(Country)     object
    所在行业(Industry)       object
    销售收入(Sales)          object
    利润(Profits)          object
    总资产(Assets)          object
    市值(Market Vaue)     float64
    dtype: object





<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>年份</th>
      <th>排名(Rank)</th>
      <th>公司名称(Company)</th>
      <th>所在国家或地区(Country)</th>
      <th>所在行业(Industry)</th>
      <th>销售收入(Sales)</th>
      <th>利润(Profits)</th>
      <th>总资产(Assets)</th>
      <th>市值(Market Vaue)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2007</td>
      <td>1</td>
      <td>Citigroup /花旗集团</td>
      <td>美国(US)</td>
      <td>银行</td>
      <td>146.56</td>
      <td>21.54</td>
      <td>1,884.32</td>
      <td>247.42</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2007</td>
      <td>2</td>
      <td>Bank of America /美国银行</td>
      <td>美国(US)</td>
      <td>银行</td>
      <td>116.57</td>
      <td>21.13</td>
      <td>1,459.74</td>
      <td>226.61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2007</td>
      <td>3</td>
      <td>HSBC Holdings/汇丰集团</td>
      <td>英国(UK)</td>
      <td>银行</td>
      <td>121.51</td>
      <td>16.63</td>
      <td>1,860.76</td>
      <td>202.29</td>
    </tr>
  </tbody>
</table>
</div>



* 更新columns的命名


```python
column_update = ['Year', 'Rank', 'Company_cn_en', 'Country_cn_en',
                 'Industry_cn', 'Sales', 'Profits', 'Assets', 'Market_value']
df_2007.columns = column_update
df_2007.head(3)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Rank</th>
      <th>Company_cn_en</th>
      <th>Country_cn_en</th>
      <th>Industry_cn</th>
      <th>Sales</th>
      <th>Profits</th>
      <th>Assets</th>
      <th>Market_value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2007</td>
      <td>1</td>
      <td>Citigroup /花旗集团</td>
      <td>美国(US)</td>
      <td>银行</td>
      <td>146.56</td>
      <td>21.54</td>
      <td>1,884.32</td>
      <td>247.42</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2007</td>
      <td>2</td>
      <td>Bank of America /美国银行</td>
      <td>美国(US)</td>
      <td>银行</td>
      <td>116.57</td>
      <td>21.13</td>
      <td>1,459.74</td>
      <td>226.61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2007</td>
      <td>3</td>
      <td>HSBC Holdings/汇丰集团</td>
      <td>英国(UK)</td>
      <td>银行</td>
      <td>121.51</td>
      <td>16.63</td>
      <td>1,860.76</td>
      <td>202.29</td>
    </tr>
  </tbody>
</table>
</div>



* **通过前面的分析可看出，只有“Market_value”是数字类型，找出'Sales','Profits'及'Assets'中非数字的内容**


```python
df_2007[df_2007['Sales'].str.contains('.*[A-Za-z]', regex=True)]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Rank</th>
      <th>Company_cn_en</th>
      <th>Country_cn_en</th>
      <th>Industry_cn</th>
      <th>Sales</th>
      <th>Profits</th>
      <th>Assets</th>
      <th>Market_value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>117</th>
      <td>2007</td>
      <td>118</td>
      <td>Repsol-YPF /瑞普索</td>
      <td>西班牙(SP)</td>
      <td>炼油</td>
      <td>64.20 E</td>
      <td>4.12</td>
      <td>58.43</td>
      <td>38.75</td>
    </tr>
    <tr>
      <th>616</th>
      <td>2007</td>
      <td>617</td>
      <td>Inpex Holdings</td>
      <td>日本(JA)</td>
      <td>炼油</td>
      <td>6.49 E</td>
      <td>1.02 E</td>
      <td>10.77 E</td>
      <td>19.65</td>
    </tr>
    <tr>
      <th>880</th>
      <td>2007</td>
      <td>881</td>
      <td>Asahi Breweries/朝日啤酒</td>
      <td>日本(JA)</td>
      <td>食品、饮料和烟草</td>
      <td>7.97 E</td>
      <td>0.38</td>
      <td>10.66</td>
      <td>7.71</td>
    </tr>
  </tbody>
</table>
</div>



* 用replace()方法替换“Sales”列中含有字母的内容


```python
df_2007['Sales'] = df_2007['Sales'].replace('([A-Za-z])', '', regex=True)
```

* 查看替换后的结果


```python
df_2007.loc[[117,616,880], :]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Rank</th>
      <th>Company_cn_en</th>
      <th>Country_cn_en</th>
      <th>Industry_cn</th>
      <th>Sales</th>
      <th>Profits</th>
      <th>Assets</th>
      <th>Market_value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>117</th>
      <td>2007</td>
      <td>118</td>
      <td>Repsol-YPF /瑞普索</td>
      <td>西班牙(SP)</td>
      <td>炼油</td>
      <td>64.20</td>
      <td>4.12</td>
      <td>58.43</td>
      <td>38.75</td>
    </tr>
    <tr>
      <th>616</th>
      <td>2007</td>
      <td>617</td>
      <td>Inpex Holdings</td>
      <td>日本(JA)</td>
      <td>炼油</td>
      <td>6.49</td>
      <td>1.02 E</td>
      <td>10.77 E</td>
      <td>19.65</td>
    </tr>
    <tr>
      <th>880</th>
      <td>2007</td>
      <td>881</td>
      <td>Asahi Breweries/朝日啤酒</td>
      <td>日本(JA)</td>
      <td>食品、饮料和烟草</td>
      <td>7.97</td>
      <td>0.38</td>
      <td>10.66</td>
      <td>7.71</td>
    </tr>
  </tbody>
</table>
</div>



* **查看“Assets”列中非数字的内容**


```python
df_2007[df_2007['Assets'].str.contains('.*[A-Za-z]', regex=True)]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Rank</th>
      <th>Company_cn_en</th>
      <th>Country_cn_en</th>
      <th>Industry_cn</th>
      <th>Sales</th>
      <th>Profits</th>
      <th>Assets</th>
      <th>Market_value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>616</th>
      <td>2007</td>
      <td>617</td>
      <td>Inpex Holdings</td>
      <td>日本(JA)</td>
      <td>炼油</td>
      <td>6.49</td>
      <td>1.02 E</td>
      <td>10.77 E</td>
      <td>19.65</td>
    </tr>
  </tbody>
</table>
</div>



* 替换非数字的内容，以及替换千分位间隔符号


```python
# 将数字后面的字母进行替换
df_2007['Assets'] = df_2007['Assets'].replace('([A-Za-z])', '', regex=True)

# 千分位数字的逗号被识别为string了，需要替换
df_2007['Assets'] = df_2007['Assets'].replace(',', '', regex=True)
df_2007.loc[616, :]
```




    Year                       2007
    Rank                        617
    Company_cn_en    Inpex Holdings
    Country_cn_en            日本(JA)
    Industry_cn                  炼油
    Sales                     6.49
    Profits                  1.02 E
    Assets                   10.77
    Market_value              19.65
    Name: 616, dtype: object



* **发现“Profits”中有NaN值，需要先进行替换**


```python
df_2007[pd.isnull(df_2007['Profits'])]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Rank</th>
      <th>Company_cn_en</th>
      <th>Country_cn_en</th>
      <th>Industry_cn</th>
      <th>Sales</th>
      <th>Profits</th>
      <th>Assets</th>
      <th>Market_value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>958</th>
      <td>2007</td>
      <td>959</td>
      <td>UAL/美国联合航空公司</td>
      <td>美国(US)</td>
      <td>运输</td>
      <td>19.34</td>
      <td>NaN</td>
      <td>25.86</td>
      <td>4.43</td>
    </tr>
    <tr>
      <th>1440</th>
      <td>2007</td>
      <td>1441</td>
      <td>Owens Corning/欧文斯科宁</td>
      <td>美国(US)</td>
      <td>建筑</td>
      <td>6.46</td>
      <td>NaN</td>
      <td>8.47</td>
      <td>4.19</td>
    </tr>
    <tr>
      <th>1544</th>
      <td>2007</td>
      <td>1545</td>
      <td>Parmalat/帕玛拉特公司</td>
      <td>意大利(IT)</td>
      <td>食品、饮料和烟草</td>
      <td>4.83</td>
      <td>NaN</td>
      <td>4.90</td>
      <td>7.02</td>
    </tr>
    <tr>
      <th>1912</th>
      <td>2007</td>
      <td>1912</td>
      <td>Winn-Dixie Stores</td>
      <td>美国(US)</td>
      <td>食品市场</td>
      <td>6.96</td>
      <td>NaN</td>
      <td>1.62</td>
      <td>1.05</td>
    </tr>
  </tbody>
</table>
</div>



* 将NaN值填充为 0


```python
df_2007['Profits'].fillna(0, inplace=True)
df_2007.loc[[958,1440,1544,1912], :]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Rank</th>
      <th>Company_cn_en</th>
      <th>Country_cn_en</th>
      <th>Industry_cn</th>
      <th>Sales</th>
      <th>Profits</th>
      <th>Assets</th>
      <th>Market_value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>958</th>
      <td>2007</td>
      <td>959</td>
      <td>UAL/美国联合航空公司</td>
      <td>美国(US)</td>
      <td>运输</td>
      <td>19.34</td>
      <td>0</td>
      <td>25.86</td>
      <td>4.43</td>
    </tr>
    <tr>
      <th>1440</th>
      <td>2007</td>
      <td>1441</td>
      <td>Owens Corning/欧文斯科宁</td>
      <td>美国(US)</td>
      <td>建筑</td>
      <td>6.46</td>
      <td>0</td>
      <td>8.47</td>
      <td>4.19</td>
    </tr>
    <tr>
      <th>1544</th>
      <td>2007</td>
      <td>1545</td>
      <td>Parmalat/帕玛拉特公司</td>
      <td>意大利(IT)</td>
      <td>食品、饮料和烟草</td>
      <td>4.83</td>
      <td>0</td>
      <td>4.90</td>
      <td>7.02</td>
    </tr>
    <tr>
      <th>1912</th>
      <td>2007</td>
      <td>1912</td>
      <td>Winn-Dixie Stores</td>
      <td>美国(US)</td>
      <td>食品市场</td>
      <td>6.96</td>
      <td>0</td>
      <td>1.62</td>
      <td>1.05</td>
    </tr>
  </tbody>
</table>
</div>



* 将“Profits”列中非数字的内容进行替换，并查看替换后的结果


```python
df_2007['Profits'] = df_2007['Profits'].replace('([A-Za-z])', '', regex=True)
df_2007.loc[[117,616,880], :]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Rank</th>
      <th>Company_cn_en</th>
      <th>Country_cn_en</th>
      <th>Industry_cn</th>
      <th>Sales</th>
      <th>Profits</th>
      <th>Assets</th>
      <th>Market_value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>117</th>
      <td>2007</td>
      <td>118</td>
      <td>Repsol-YPF /瑞普索</td>
      <td>西班牙(SP)</td>
      <td>炼油</td>
      <td>64.20</td>
      <td>4.12</td>
      <td>58.43</td>
      <td>38.75</td>
    </tr>
    <tr>
      <th>616</th>
      <td>2007</td>
      <td>617</td>
      <td>Inpex Holdings</td>
      <td>日本(JA)</td>
      <td>炼油</td>
      <td>6.49</td>
      <td>1.02</td>
      <td>10.77</td>
      <td>19.65</td>
    </tr>
    <tr>
      <th>880</th>
      <td>2007</td>
      <td>881</td>
      <td>Asahi Breweries/朝日啤酒</td>
      <td>日本(JA)</td>
      <td>食品、饮料和烟草</td>
      <td>7.97</td>
      <td>0.38</td>
      <td>10.66</td>
      <td>7.71</td>
    </tr>
  </tbody>
</table>
</div>



* **将sting类型的数字转换为数据类型，这里使用 pd.to_numeric() 方法**


```python
df_2007['Sales'] = pd.to_numeric(df_2007['Sales'])
df_2007['Profits'] = pd.to_numeric(df_2007['Profits'])
df_2007['Assets'] = pd.to_numeric(df_2007['Assets'])
df_2007.dtypes
```




    Year               int64
    Rank               int64
    Company_cn_en     object
    Country_cn_en     object
    Industry_cn       object
    Sales            float64
    Profits          float64
    Assets           float64
    Market_value     float64
    dtype: object



* **拆分"Company_cn_en"列**，新生成两列，分别为公司英文名称和中文名称


```python
df_2007['Company_en'],df_2007['Company_cn'] = df_2007['Company_cn_en'].str.split('/', 1).str
print(df_2007['Company_en'][:5])
print(df_2007['Company_cn'] [-5:])
```

    0           Citigroup
    1     Bank of America
    2        HSBC Holdings
    3    General Electric
    4      JPMorgan Chase
    Name: Company_en, dtype: object
    1995    NaN
    1996    NaN
    1997    NaN
    1998    NaN
    1999    NaN
    Name: Company_cn, dtype: object



```python
df_2007.tail(3)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Rank</th>
      <th>Company_cn_en</th>
      <th>Country_cn_en</th>
      <th>Industry_cn</th>
      <th>Sales</th>
      <th>Profits</th>
      <th>Assets</th>
      <th>Market_value</th>
      <th>Company_en</th>
      <th>Company_cn</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1997</th>
      <td>2007</td>
      <td>1998</td>
      <td>CBOT Holdings</td>
      <td>美国(US)</td>
      <td>综合金融</td>
      <td>0.64</td>
      <td>0.17</td>
      <td>0.81</td>
      <td>8.54</td>
      <td>CBOT Holdings</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1998</th>
      <td>2007</td>
      <td>1998</td>
      <td>Singapore Petroleum</td>
      <td>新加坡(SI)</td>
      <td>炼油</td>
      <td>5.59</td>
      <td>0.19</td>
      <td>2.05</td>
      <td>1.50</td>
      <td>Singapore Petroleum</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1999</th>
      <td>2007</td>
      <td>2000</td>
      <td>DVB Bank</td>
      <td>德国(GE)</td>
      <td>银行</td>
      <td>0.77</td>
      <td>0.06</td>
      <td>12.74</td>
      <td>1.26</td>
      <td>DVB Bank</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



* **拆分"Country_cn_en"列**，新生成两列，分别为国家中文名称和英文名称


```python
df_2007['Country_cn'],df_2007['Country_en'] = df_2007['Country_cn_en'].str.split('(', 1).str
print(df_2007['Country_cn'][:5])
print(df_2007['Country_en'][-5:])
```

    0    美国
    1    美国
    2    英国
    3    美国
    4    美国
    Name: Country_cn, dtype: object
    1995    US)
    1996    US)
    1997    US)
    1998    SI)
    1999    GE)
    Name: Country_en, dtype: object


* 由于国家的英文名称中，最后有半个括号，需要去除，用 Series.str.slice()方法
* 参数表示选取从开始到倒数第二个，即不要括号")"


```python
df_2007['Country_en'] = df_2007['Country_en'].str.slice(0,-1)
df_2007.head(3)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Rank</th>
      <th>Company_cn_en</th>
      <th>Country_cn_en</th>
      <th>Industry_cn</th>
      <th>Sales</th>
      <th>Profits</th>
      <th>Assets</th>
      <th>Market_value</th>
      <th>Company_en</th>
      <th>Company_cn</th>
      <th>Country_cn</th>
      <th>Country_en</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2007</td>
      <td>1</td>
      <td>Citigroup /花旗集团</td>
      <td>美国(US)</td>
      <td>银行</td>
      <td>146.56</td>
      <td>21.54</td>
      <td>1884.32</td>
      <td>247.42</td>
      <td>Citigroup</td>
      <td>花旗集团</td>
      <td>美国</td>
      <td>US</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2007</td>
      <td>2</td>
      <td>Bank of America /美国银行</td>
      <td>美国(US)</td>
      <td>银行</td>
      <td>116.57</td>
      <td>21.13</td>
      <td>1459.74</td>
      <td>226.61</td>
      <td>Bank of America</td>
      <td>美国银行</td>
      <td>美国</td>
      <td>US</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2007</td>
      <td>3</td>
      <td>HSBC Holdings/汇丰集团</td>
      <td>英国(UK)</td>
      <td>银行</td>
      <td>121.51</td>
      <td>16.63</td>
      <td>1860.76</td>
      <td>202.29</td>
      <td>HSBC Holdings</td>
      <td>汇丰集团</td>
      <td>英国</td>
      <td>UK</td>
    </tr>
  </tbody>
</table>
</div>



* 考虑的中国的企业有区分为中国大陆，中国香港，中国台湾
* 对应的国家英文名称也需要修改下
* 中国大陆：CN；中国香港：CN-HK；中国台湾：CN-TA


```python
df_2007[df_2007['Country_cn'].str.contains('中国',regex=True)]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Rank</th>
      <th>Company_cn_en</th>
      <th>Country_cn_en</th>
      <th>Industry_cn</th>
      <th>Sales</th>
      <th>Profits</th>
      <th>Assets</th>
      <th>Market_value</th>
      <th>Company_en</th>
      <th>Company_cn</th>
      <th>Country_cn</th>
      <th>Country_en</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>40</th>
      <td>2007</td>
      <td>41</td>
      <td>PetroChina /中国石油</td>
      <td>中国大陆(CN)</td>
      <td>炼油</td>
      <td>68.43</td>
      <td>16.53</td>
      <td>96.42</td>
      <td>208.76</td>
      <td>PetroChina</td>
      <td>中国石油</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>52</th>
      <td>2007</td>
      <td>53</td>
      <td>ICBC /中国工商银行</td>
      <td>中国大陆(CN)</td>
      <td>银行</td>
      <td>31.98</td>
      <td>4.65</td>
      <td>800.04</td>
      <td>176.03</td>
      <td>ICBC</td>
      <td>中国工商银行</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>68</th>
      <td>2007</td>
      <td>69</td>
      <td>CCB-China Construction Bank /中国建设银行</td>
      <td>中国大陆(CN)</td>
      <td>银行</td>
      <td>23.18</td>
      <td>5.84</td>
      <td>568.21</td>
      <td>126.55</td>
      <td>CCB-China Construction Bank</td>
      <td>中国建设银行</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>70</th>
      <td>2007</td>
      <td>71</td>
      <td>Sinopec-China Petroleum /中石化</td>
      <td>中国大陆(CN)</td>
      <td>炼油</td>
      <td>99.03</td>
      <td>5.07</td>
      <td>65.83</td>
      <td>93.57</td>
      <td>Sinopec-China Petroleum</td>
      <td>中石化</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>81</th>
      <td>2007</td>
      <td>82</td>
      <td>Bank of China /中国银行</td>
      <td>中国大陆(CN)</td>
      <td>银行</td>
      <td>23.10</td>
      <td>3.41</td>
      <td>585.55</td>
      <td>143.80</td>
      <td>Bank of China</td>
      <td>中国银行</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>88</th>
      <td>2007</td>
      <td>89</td>
      <td>China Mobile /中国移动</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>电信运营商</td>
      <td>29.79</td>
      <td>6.56</td>
      <td>51.35</td>
      <td>185.31</td>
      <td>China Mobile</td>
      <td>中国移动</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>175</th>
      <td>2007</td>
      <td>176</td>
      <td>Hutchison Whampoa/和记黄埔</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>多元化</td>
      <td>23.55</td>
      <td>1.85</td>
      <td>74.97</td>
      <td>40.57</td>
      <td>Hutchison Whampoa</td>
      <td>和记黄埔</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>180</th>
      <td>2007</td>
      <td>181</td>
      <td>China Telecom/中国电信</td>
      <td>中国大陆(CN)</td>
      <td>电信运营商</td>
      <td>20.98</td>
      <td>3.46</td>
      <td>50.34</td>
      <td>37.50</td>
      <td>China Telecom</td>
      <td>中国电信</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>242</th>
      <td>2007</td>
      <td>243</td>
      <td>China Life Insurance /中国人寿</td>
      <td>中国大陆(CN)</td>
      <td>保险</td>
      <td>11.18</td>
      <td>1.15</td>
      <td>69.30</td>
      <td>109.96</td>
      <td>China Life Insurance</td>
      <td>中国人寿</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>307</th>
      <td>2007</td>
      <td>308</td>
      <td>Bank of Communications/中国交通银行</td>
      <td>中国大陆(CN)</td>
      <td>银行</td>
      <td>6.64</td>
      <td>1.15</td>
      <td>176.27</td>
      <td>46.14</td>
      <td>Bank of Communications</td>
      <td>中国交通银行</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>309</th>
      <td>2007</td>
      <td>310</td>
      <td>Taiwan Semiconductor/台积电</td>
      <td>中国台湾(TA)</td>
      <td>半导体</td>
      <td>9.74</td>
      <td>3.90</td>
      <td>18.02</td>
      <td>54.32</td>
      <td>Taiwan Semiconductor</td>
      <td>台积电</td>
      <td>中国台湾</td>
      <td>TA</td>
    </tr>
    <tr>
      <th>340</th>
      <td>2007</td>
      <td>341</td>
      <td>Hon Hai Precision Ind /鸿海精密</td>
      <td>中国台湾(TA)</td>
      <td>技术硬件和装备</td>
      <td>27.78</td>
      <td>1.24</td>
      <td>13.99</td>
      <td>34.83</td>
      <td>Hon Hai Precision Ind</td>
      <td>鸿海精密</td>
      <td>中国台湾</td>
      <td>TA</td>
    </tr>
    <tr>
      <th>365</th>
      <td>2007</td>
      <td>366</td>
      <td>Baoshan Iron &amp; Steel /上海宝钢集团</td>
      <td>中国大陆(CN)</td>
      <td>材料</td>
      <td>15.63</td>
      <td>1.57</td>
      <td>17.59</td>
      <td>21.42</td>
      <td>Baoshan Iron &amp; Steel</td>
      <td>上海宝钢集团</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>388</th>
      <td>2007</td>
      <td>389</td>
      <td>Cathay Financial/国泰金融</td>
      <td>中国台湾(TA)</td>
      <td>保险</td>
      <td>10.09</td>
      <td>0.66</td>
      <td>93.29</td>
      <td>19.87</td>
      <td>Cathay Financial</td>
      <td>国泰金融</td>
      <td>中国台湾</td>
      <td>TA</td>
    </tr>
    <tr>
      <th>394</th>
      <td>2007</td>
      <td>395</td>
      <td>Cnooc /中海油</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>炼油</td>
      <td>8.51</td>
      <td>3.10</td>
      <td>14.22</td>
      <td>34.94</td>
      <td>Cnooc</td>
      <td>中海油</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>400</th>
      <td>2007</td>
      <td>401</td>
      <td>China Netcom Group /中国网通</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>电信运营商</td>
      <td>10.69</td>
      <td>1.70</td>
      <td>24.70</td>
      <td>15.70</td>
      <td>China Netcom Group</td>
      <td>中国网通</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>422</th>
      <td>2007</td>
      <td>423</td>
      <td>China Shenhua Energy/中国神华能源股份有限公司</td>
      <td>中国大陆(CN)</td>
      <td>材料</td>
      <td>6.47</td>
      <td>1.94</td>
      <td>17.08</td>
      <td>45.94</td>
      <td>China Shenhua Energy</td>
      <td>中国神华能源股份有限公司</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>429</th>
      <td>2007</td>
      <td>430</td>
      <td>BOC Hong Kong/中银香港</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>银行</td>
      <td>4.13</td>
      <td>1.74</td>
      <td>106.03</td>
      <td>25.58</td>
      <td>BOC Hong Kong</td>
      <td>中银香港</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>436</th>
      <td>2007</td>
      <td>437</td>
      <td>Formosa Petrochemical/台塑石化</td>
      <td>中国台湾(TA)</td>
      <td>炼油</td>
      <td>13.56</td>
      <td>1.74</td>
      <td>12.35</td>
      <td>19.28</td>
      <td>Formosa Petrochemical</td>
      <td>台塑石化</td>
      <td>中国台湾</td>
      <td>TA</td>
    </tr>
    <tr>
      <th>439</th>
      <td>2007</td>
      <td>440</td>
      <td>Ping An Insurance Group/平安保险</td>
      <td>中国大陆(CN)</td>
      <td>保险</td>
      <td>7.95</td>
      <td>0.52</td>
      <td>39.62</td>
      <td>39.60</td>
      <td>Ping An Insurance Group</td>
      <td>平安保险</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>451</th>
      <td>2007</td>
      <td>452</td>
      <td>Jardine Matheson/香港怡和集团</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>食品市场</td>
      <td>11.96</td>
      <td>1.25</td>
      <td>18.34</td>
      <td>13.59</td>
      <td>Jardine Matheson</td>
      <td>香港怡和集团</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>510</th>
      <td>2007</td>
      <td>511</td>
      <td>Sun Hung Kai Properties /新鸿基房地产</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>综合金融</td>
      <td>3.30</td>
      <td>2.56</td>
      <td>29.72</td>
      <td>29.49</td>
      <td>Sun Hung Kai Properties</td>
      <td>新鸿基房地产</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>541</th>
      <td>2007</td>
      <td>542</td>
      <td>China Unicom /中国联通</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>电信运营商</td>
      <td>10.67</td>
      <td>0.60</td>
      <td>17.63</td>
      <td>16.03</td>
      <td>China Unicom</td>
      <td>中国联通</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>551</th>
      <td>2007</td>
      <td>552</td>
      <td>CLP Holdings /中电控股</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>公用事业</td>
      <td>5.87</td>
      <td>1.27</td>
      <td>16.42</td>
      <td>17.65</td>
      <td>CLP Holdings</td>
      <td>中电控股</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>575</th>
      <td>2007</td>
      <td>576</td>
      <td>Chunghwa Telecom/中华电信</td>
      <td>中国台湾(TA)</td>
      <td>电信运营商</td>
      <td>5.59</td>
      <td>1.45</td>
      <td>13.98</td>
      <td>18.22</td>
      <td>Chunghwa Telecom</td>
      <td>中华电信</td>
      <td>中国台湾</td>
      <td>TA</td>
    </tr>
    <tr>
      <th>600</th>
      <td>2007</td>
      <td>601</td>
      <td>China Steel/台湾中钢公司</td>
      <td>中国台湾(TA)</td>
      <td>材料</td>
      <td>8.66</td>
      <td>1.54</td>
      <td>10.35</td>
      <td>12.24</td>
      <td>China Steel</td>
      <td>台湾中钢公司</td>
      <td>中国台湾</td>
      <td>TA</td>
    </tr>
    <tr>
      <th>603</th>
      <td>2007</td>
      <td>604</td>
      <td>China Merchants Bank/招商银行</td>
      <td>中国大陆(CN)</td>
      <td>银行</td>
      <td>3.53</td>
      <td>0.46</td>
      <td>90.76</td>
      <td>33.19</td>
      <td>China Merchants Bank</td>
      <td>招商银行</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>617</th>
      <td>2007</td>
      <td>617</td>
      <td>Nan Ya Plastic/南亚塑胶工业</td>
      <td>中国台湾(TA)</td>
      <td>化学制品</td>
      <td>7.64</td>
      <td>1.22</td>
      <td>11.47</td>
      <td>13.37</td>
      <td>Nan Ya Plastic</td>
      <td>南亚塑胶工业</td>
      <td>中国台湾</td>
      <td>TA</td>
    </tr>
    <tr>
      <th>627</th>
      <td>2007</td>
      <td>628</td>
      <td>Cheung Kong/长江集团</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>综合金融</td>
      <td>0.80</td>
      <td>1.80</td>
      <td>28.01</td>
      <td>28.39</td>
      <td>Cheung Kong</td>
      <td>长江集团</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>736</th>
      <td>2007</td>
      <td>737</td>
      <td>Swire Pacific /太古集团</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>多元化</td>
      <td>2.44</td>
      <td>2.42</td>
      <td>16.05</td>
      <td>17.32</td>
      <td>Swire Pacific</td>
      <td>太古集团</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1636</th>
      <td>2007</td>
      <td>1637</td>
      <td>Champion REIT</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>综合金融</td>
      <td>0.05</td>
      <td>1.16</td>
      <td>2.95</td>
      <td>1.54</td>
      <td>Champion REIT</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>1641</th>
      <td>2007</td>
      <td>1642</td>
      <td>Noble Group</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>运输</td>
      <td>13.75</td>
      <td>0.13</td>
      <td>3.81</td>
      <td>2.14</td>
      <td>Noble Group</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>1661</th>
      <td>2007</td>
      <td>1662</td>
      <td>Taiwan Mobile</td>
      <td>中国台湾(TA)</td>
      <td>电信运营商</td>
      <td>1.81</td>
      <td>0.50</td>
      <td>3.59</td>
      <td>4.84</td>
      <td>Taiwan Mobile</td>
      <td>NaN</td>
      <td>中国台湾</td>
      <td>TA</td>
    </tr>
    <tr>
      <th>1681</th>
      <td>2007</td>
      <td>1682</td>
      <td>Evergreen Marine</td>
      <td>中国台湾(TA)</td>
      <td>运输</td>
      <td>4.29</td>
      <td>0.37</td>
      <td>3.96</td>
      <td>1.90</td>
      <td>Evergreen Marine</td>
      <td>NaN</td>
      <td>中国台湾</td>
      <td>TA</td>
    </tr>
    <tr>
      <th>1692</th>
      <td>2007</td>
      <td>1693</td>
      <td>China Southern Airlines</td>
      <td>中国大陆(CN)</td>
      <td>运输</td>
      <td>4.64</td>
      <td>-0.23</td>
      <td>8.84</td>
      <td>1.97</td>
      <td>China Southern Airlines</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1705</th>
      <td>2007</td>
      <td>1706</td>
      <td>Cosco Pacific</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>运输</td>
      <td>0.30</td>
      <td>0.34</td>
      <td>2.85</td>
      <td>5.94</td>
      <td>Cosco Pacific</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>1710</th>
      <td>2007</td>
      <td>1711</td>
      <td>China Shipping Container</td>
      <td>中国大陆(CN)</td>
      <td>运输</td>
      <td>3.52</td>
      <td>0.44</td>
      <td>3.59</td>
      <td>2.26</td>
      <td>China Shipping Container</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1736</th>
      <td>2007</td>
      <td>1737</td>
      <td>China Resources Power Holdings</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>公用事业</td>
      <td>0.76</td>
      <td>0.37</td>
      <td>3.67</td>
      <td>5.37</td>
      <td>China Resources Power Holdings</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>1739</th>
      <td>2007</td>
      <td>1740</td>
      <td>Citic Securities</td>
      <td>中国大陆(CN)</td>
      <td>综合金融</td>
      <td>0.14</td>
      <td>0.04</td>
      <td>2.52</td>
      <td>14.29</td>
      <td>Citic Securities</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1780</th>
      <td>2007</td>
      <td>1781</td>
      <td>Far EasTone Telecom</td>
      <td>中国台湾(TA)</td>
      <td>电信运营商</td>
      <td>2.19</td>
      <td>0.45</td>
      <td>3.01</td>
      <td>4.45</td>
      <td>Far EasTone Telecom</td>
      <td>NaN</td>
      <td>中国台湾</td>
      <td>TA</td>
    </tr>
    <tr>
      <th>1786</th>
      <td>2007</td>
      <td>1787</td>
      <td>E.Sun Financial</td>
      <td>中国台湾(TA)</td>
      <td>银行</td>
      <td>0.73</td>
      <td>0.14</td>
      <td>19.36</td>
      <td>2.19</td>
      <td>E.Sun Financial</td>
      <td>NaN</td>
      <td>中国台湾</td>
      <td>TA</td>
    </tr>
    <tr>
      <th>1824</th>
      <td>2007</td>
      <td>1825</td>
      <td>Minmetals Development</td>
      <td>中国大陆(CN)</td>
      <td>贸易公司</td>
      <td>8.25</td>
      <td>0.04</td>
      <td>3.46</td>
      <td>1.50</td>
      <td>Minmetals Development</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1840</th>
      <td>2007</td>
      <td>1841</td>
      <td>Shanghai Automotive</td>
      <td>中国大陆(CN)</td>
      <td>耐用消费品</td>
      <td>0.79</td>
      <td>0.14</td>
      <td>1.81</td>
      <td>11.10</td>
      <td>Shanghai Automotive</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1846</th>
      <td>2007</td>
      <td>1847</td>
      <td>HK Exchanges &amp; Clearing</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>综合金融</td>
      <td>0.35</td>
      <td>0.17</td>
      <td>2.96</td>
      <td>10.97</td>
      <td>HK Exchanges &amp; Clearing</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>1852</th>
      <td>2007</td>
      <td>1853</td>
      <td>Link REIT</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>综合金融</td>
      <td>0.43</td>
      <td>0.27</td>
      <td>5.24</td>
      <td>5.00</td>
      <td>Link REIT</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>1860</th>
      <td>2007</td>
      <td>1861</td>
      <td>Kweichow Moutai</td>
      <td>中国大陆(CN)</td>
      <td>食品、饮料和烟草</td>
      <td>0.43</td>
      <td>0.14</td>
      <td>1.00</td>
      <td>10.69</td>
      <td>Kweichow Moutai</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1892</th>
      <td>2007</td>
      <td>1892</td>
      <td>Yanzhou Coal Mining</td>
      <td>中国大陆(CN)</td>
      <td>材料</td>
      <td>1.43</td>
      <td>0.36</td>
      <td>2.63</td>
      <td>4.52</td>
      <td>Yanzhou Coal Mining</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1908</th>
      <td>2007</td>
      <td>1909</td>
      <td>China Shipping Develop</td>
      <td>中国大陆(CN)</td>
      <td>运输</td>
      <td>1.06</td>
      <td>0.33</td>
      <td>1.66</td>
      <td>4.61</td>
      <td>China Shipping Develop</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1920</th>
      <td>2007</td>
      <td>1920</td>
      <td>Wing Lung Bank</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>银行</td>
      <td>0.66</td>
      <td>0.21</td>
      <td>10.92</td>
      <td>2.43</td>
      <td>Wing Lung Bank</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>1922</th>
      <td>2007</td>
      <td>1923</td>
      <td>Delta Electronics</td>
      <td>中国台湾(TA)</td>
      <td>技术硬件和装备</td>
      <td>2.46</td>
      <td>0.23</td>
      <td>2.49</td>
      <td>6.40</td>
      <td>Delta Electronics</td>
      <td>NaN</td>
      <td>中国台湾</td>
      <td>TA</td>
    </tr>
    <tr>
      <th>1945</th>
      <td>2007</td>
      <td>1946</td>
      <td>China Airlines</td>
      <td>中国台湾(TA)</td>
      <td>运输</td>
      <td>3.61</td>
      <td>0.02</td>
      <td>7.63</td>
      <td>1.85</td>
      <td>China Airlines</td>
      <td>NaN</td>
      <td>中国台湾</td>
      <td>TA</td>
    </tr>
    <tr>
      <th>1948</th>
      <td>2007</td>
      <td>1949</td>
      <td>Wing Hang Bank</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>银行</td>
      <td>0.66</td>
      <td>0.17</td>
      <td>13.45</td>
      <td>3.33</td>
      <td>Wing Hang Bank</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>1959</th>
      <td>2007</td>
      <td>1959</td>
      <td>PCCW</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>电信运营商</td>
      <td>2.90</td>
      <td>0.21</td>
      <td>6.87</td>
      <td>3.98</td>
      <td>PCCW</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>1960</th>
      <td>2007</td>
      <td>1961</td>
      <td>Benq</td>
      <td>中国台湾(TA)</td>
      <td>技术硬件和装备</td>
      <td>5.39</td>
      <td>-0.16</td>
      <td>5.04</td>
      <td>1.27</td>
      <td>Benq</td>
      <td>NaN</td>
      <td>中国台湾</td>
      <td>TA</td>
    </tr>
    <tr>
      <th>1963</th>
      <td>2007</td>
      <td>1964</td>
      <td>TCL Corp</td>
      <td>中国大陆(CN)</td>
      <td>技术硬件和装备</td>
      <td>6.40</td>
      <td>-0.04</td>
      <td>3.77</td>
      <td>1.39</td>
      <td>TCL Corp</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1970</th>
      <td>2007</td>
      <td>1971</td>
      <td>Wuliangye Yibin</td>
      <td>中国大陆(CN)</td>
      <td>食品、饮料和烟草</td>
      <td>0.70</td>
      <td>0.10</td>
      <td>1.19</td>
      <td>8.81</td>
      <td>Wuliangye Yibin</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1973</th>
      <td>2007</td>
      <td>1974</td>
      <td>CNPC (Hong Kong)</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>炼油</td>
      <td>0.44</td>
      <td>0.47</td>
      <td>2.07</td>
      <td>2.30</td>
      <td>CNPC (Hong Kong)</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>1975</th>
      <td>2007</td>
      <td>1976</td>
      <td>K Wah International</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>综合金融</td>
      <td>0.04</td>
      <td>0.47</td>
      <td>1.29</td>
      <td>0.98</td>
      <td>K Wah International</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>1986</th>
      <td>2007</td>
      <td>1987</td>
      <td>China Overseas Land &amp; Inv</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>综合金融</td>
      <td>0.90</td>
      <td>0.20</td>
      <td>3.24</td>
      <td>7.05</td>
      <td>China Overseas Land &amp; Inv</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
    <tr>
      <th>1989</th>
      <td>2007</td>
      <td>1989</td>
      <td>Nine Dragons Paper Holdings</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>材料</td>
      <td>0.99</td>
      <td>0.17</td>
      <td>1.86</td>
      <td>8.61</td>
      <td>Nine Dragons Paper Holdings</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>HK)/中国大陆(CN</td>
    </tr>
  </tbody>
</table>
<p>131 rows × 13 columns</p>
</div>




```python
df_2007['Country_en'] = df_2007['Country_en'].replace(['HK.*','TA'],['CN-HK', 'CN-TA'],regex=True)
df_2007[df_2007['Country_en'].str.contains('CN',regex=True)]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Rank</th>
      <th>Company_cn_en</th>
      <th>Country_cn_en</th>
      <th>Industry_cn</th>
      <th>Sales</th>
      <th>Profits</th>
      <th>Assets</th>
      <th>Market_value</th>
      <th>Company_en</th>
      <th>Company_cn</th>
      <th>Country_cn</th>
      <th>Country_en</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>40</th>
      <td>2007</td>
      <td>41</td>
      <td>PetroChina /中国石油</td>
      <td>中国大陆(CN)</td>
      <td>炼油</td>
      <td>68.43</td>
      <td>16.53</td>
      <td>96.42</td>
      <td>208.76</td>
      <td>PetroChina</td>
      <td>中国石油</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>52</th>
      <td>2007</td>
      <td>53</td>
      <td>ICBC /中国工商银行</td>
      <td>中国大陆(CN)</td>
      <td>银行</td>
      <td>31.98</td>
      <td>4.65</td>
      <td>800.04</td>
      <td>176.03</td>
      <td>ICBC</td>
      <td>中国工商银行</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>68</th>
      <td>2007</td>
      <td>69</td>
      <td>CCB-China Construction Bank /中国建设银行</td>
      <td>中国大陆(CN)</td>
      <td>银行</td>
      <td>23.18</td>
      <td>5.84</td>
      <td>568.21</td>
      <td>126.55</td>
      <td>CCB-China Construction Bank</td>
      <td>中国建设银行</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>70</th>
      <td>2007</td>
      <td>71</td>
      <td>Sinopec-China Petroleum /中石化</td>
      <td>中国大陆(CN)</td>
      <td>炼油</td>
      <td>99.03</td>
      <td>5.07</td>
      <td>65.83</td>
      <td>93.57</td>
      <td>Sinopec-China Petroleum</td>
      <td>中石化</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>81</th>
      <td>2007</td>
      <td>82</td>
      <td>Bank of China /中国银行</td>
      <td>中国大陆(CN)</td>
      <td>银行</td>
      <td>23.10</td>
      <td>3.41</td>
      <td>585.55</td>
      <td>143.80</td>
      <td>Bank of China</td>
      <td>中国银行</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>88</th>
      <td>2007</td>
      <td>89</td>
      <td>China Mobile /中国移动</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>电信运营商</td>
      <td>29.79</td>
      <td>6.56</td>
      <td>51.35</td>
      <td>185.31</td>
      <td>China Mobile</td>
      <td>中国移动</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>175</th>
      <td>2007</td>
      <td>176</td>
      <td>Hutchison Whampoa/和记黄埔</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>多元化</td>
      <td>23.55</td>
      <td>1.85</td>
      <td>74.97</td>
      <td>40.57</td>
      <td>Hutchison Whampoa</td>
      <td>和记黄埔</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>180</th>
      <td>2007</td>
      <td>181</td>
      <td>China Telecom/中国电信</td>
      <td>中国大陆(CN)</td>
      <td>电信运营商</td>
      <td>20.98</td>
      <td>3.46</td>
      <td>50.34</td>
      <td>37.50</td>
      <td>China Telecom</td>
      <td>中国电信</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>242</th>
      <td>2007</td>
      <td>243</td>
      <td>China Life Insurance /中国人寿</td>
      <td>中国大陆(CN)</td>
      <td>保险</td>
      <td>11.18</td>
      <td>1.15</td>
      <td>69.30</td>
      <td>109.96</td>
      <td>China Life Insurance</td>
      <td>中国人寿</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>307</th>
      <td>2007</td>
      <td>308</td>
      <td>Bank of Communications/中国交通银行</td>
      <td>中国大陆(CN)</td>
      <td>银行</td>
      <td>6.64</td>
      <td>1.15</td>
      <td>176.27</td>
      <td>46.14</td>
      <td>Bank of Communications</td>
      <td>中国交通银行</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>309</th>
      <td>2007</td>
      <td>310</td>
      <td>Taiwan Semiconductor/台积电</td>
      <td>中国台湾(TA)</td>
      <td>半导体</td>
      <td>9.74</td>
      <td>3.90</td>
      <td>18.02</td>
      <td>54.32</td>
      <td>Taiwan Semiconductor</td>
      <td>台积电</td>
      <td>中国台湾</td>
      <td>CN-TA</td>
    </tr>
    <tr>
      <th>340</th>
      <td>2007</td>
      <td>341</td>
      <td>Hon Hai Precision Ind /鸿海精密</td>
      <td>中国台湾(TA)</td>
      <td>技术硬件和装备</td>
      <td>27.78</td>
      <td>1.24</td>
      <td>13.99</td>
      <td>34.83</td>
      <td>Hon Hai Precision Ind</td>
      <td>鸿海精密</td>
      <td>中国台湾</td>
      <td>CN-TA</td>
    </tr>
    <tr>
      <th>365</th>
      <td>2007</td>
      <td>366</td>
      <td>Baoshan Iron &amp; Steel /上海宝钢集团</td>
      <td>中国大陆(CN)</td>
      <td>材料</td>
      <td>15.63</td>
      <td>1.57</td>
      <td>17.59</td>
      <td>21.42</td>
      <td>Baoshan Iron &amp; Steel</td>
      <td>上海宝钢集团</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>388</th>
      <td>2007</td>
      <td>389</td>
      <td>Cathay Financial/国泰金融</td>
      <td>中国台湾(TA)</td>
      <td>保险</td>
      <td>10.09</td>
      <td>0.66</td>
      <td>93.29</td>
      <td>19.87</td>
      <td>Cathay Financial</td>
      <td>国泰金融</td>
      <td>中国台湾</td>
      <td>CN-TA</td>
    </tr>
    <tr>
      <th>394</th>
      <td>2007</td>
      <td>395</td>
      <td>Cnooc /中海油</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>炼油</td>
      <td>8.51</td>
      <td>3.10</td>
      <td>14.22</td>
      <td>34.94</td>
      <td>Cnooc</td>
      <td>中海油</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>400</th>
      <td>2007</td>
      <td>401</td>
      <td>China Netcom Group /中国网通</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>电信运营商</td>
      <td>10.69</td>
      <td>1.70</td>
      <td>24.70</td>
      <td>15.70</td>
      <td>China Netcom Group</td>
      <td>中国网通</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>422</th>
      <td>2007</td>
      <td>423</td>
      <td>China Shenhua Energy/中国神华能源股份有限公司</td>
      <td>中国大陆(CN)</td>
      <td>材料</td>
      <td>6.47</td>
      <td>1.94</td>
      <td>17.08</td>
      <td>45.94</td>
      <td>China Shenhua Energy</td>
      <td>中国神华能源股份有限公司</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>429</th>
      <td>2007</td>
      <td>430</td>
      <td>BOC Hong Kong/中银香港</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>银行</td>
      <td>4.13</td>
      <td>1.74</td>
      <td>106.03</td>
      <td>25.58</td>
      <td>BOC Hong Kong</td>
      <td>中银香港</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>436</th>
      <td>2007</td>
      <td>437</td>
      <td>Formosa Petrochemical/台塑石化</td>
      <td>中国台湾(TA)</td>
      <td>炼油</td>
      <td>13.56</td>
      <td>1.74</td>
      <td>12.35</td>
      <td>19.28</td>
      <td>Formosa Petrochemical</td>
      <td>台塑石化</td>
      <td>中国台湾</td>
      <td>CN-TA</td>
    </tr>
    <tr>
      <th>439</th>
      <td>2007</td>
      <td>440</td>
      <td>Ping An Insurance Group/平安保险</td>
      <td>中国大陆(CN)</td>
      <td>保险</td>
      <td>7.95</td>
      <td>0.52</td>
      <td>39.62</td>
      <td>39.60</td>
      <td>Ping An Insurance Group</td>
      <td>平安保险</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>451</th>
      <td>2007</td>
      <td>452</td>
      <td>Jardine Matheson/香港怡和集团</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>食品市场</td>
      <td>11.96</td>
      <td>1.25</td>
      <td>18.34</td>
      <td>13.59</td>
      <td>Jardine Matheson</td>
      <td>香港怡和集团</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>510</th>
      <td>2007</td>
      <td>511</td>
      <td>Sun Hung Kai Properties /新鸿基房地产</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>综合金融</td>
      <td>3.30</td>
      <td>2.56</td>
      <td>29.72</td>
      <td>29.49</td>
      <td>Sun Hung Kai Properties</td>
      <td>新鸿基房地产</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>541</th>
      <td>2007</td>
      <td>542</td>
      <td>China Unicom /中国联通</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>电信运营商</td>
      <td>10.67</td>
      <td>0.60</td>
      <td>17.63</td>
      <td>16.03</td>
      <td>China Unicom</td>
      <td>中国联通</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>551</th>
      <td>2007</td>
      <td>552</td>
      <td>CLP Holdings /中电控股</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>公用事业</td>
      <td>5.87</td>
      <td>1.27</td>
      <td>16.42</td>
      <td>17.65</td>
      <td>CLP Holdings</td>
      <td>中电控股</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>575</th>
      <td>2007</td>
      <td>576</td>
      <td>Chunghwa Telecom/中华电信</td>
      <td>中国台湾(TA)</td>
      <td>电信运营商</td>
      <td>5.59</td>
      <td>1.45</td>
      <td>13.98</td>
      <td>18.22</td>
      <td>Chunghwa Telecom</td>
      <td>中华电信</td>
      <td>中国台湾</td>
      <td>CN-TA</td>
    </tr>
    <tr>
      <th>600</th>
      <td>2007</td>
      <td>601</td>
      <td>China Steel/台湾中钢公司</td>
      <td>中国台湾(TA)</td>
      <td>材料</td>
      <td>8.66</td>
      <td>1.54</td>
      <td>10.35</td>
      <td>12.24</td>
      <td>China Steel</td>
      <td>台湾中钢公司</td>
      <td>中国台湾</td>
      <td>CN-TA</td>
    </tr>
    <tr>
      <th>603</th>
      <td>2007</td>
      <td>604</td>
      <td>China Merchants Bank/招商银行</td>
      <td>中国大陆(CN)</td>
      <td>银行</td>
      <td>3.53</td>
      <td>0.46</td>
      <td>90.76</td>
      <td>33.19</td>
      <td>China Merchants Bank</td>
      <td>招商银行</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>617</th>
      <td>2007</td>
      <td>617</td>
      <td>Nan Ya Plastic/南亚塑胶工业</td>
      <td>中国台湾(TA)</td>
      <td>化学制品</td>
      <td>7.64</td>
      <td>1.22</td>
      <td>11.47</td>
      <td>13.37</td>
      <td>Nan Ya Plastic</td>
      <td>南亚塑胶工业</td>
      <td>中国台湾</td>
      <td>CN-TA</td>
    </tr>
    <tr>
      <th>627</th>
      <td>2007</td>
      <td>628</td>
      <td>Cheung Kong/长江集团</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>综合金融</td>
      <td>0.80</td>
      <td>1.80</td>
      <td>28.01</td>
      <td>28.39</td>
      <td>Cheung Kong</td>
      <td>长江集团</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>736</th>
      <td>2007</td>
      <td>737</td>
      <td>Swire Pacific /太古集团</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>多元化</td>
      <td>2.44</td>
      <td>2.42</td>
      <td>16.05</td>
      <td>17.32</td>
      <td>Swire Pacific</td>
      <td>太古集团</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1636</th>
      <td>2007</td>
      <td>1637</td>
      <td>Champion REIT</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>综合金融</td>
      <td>0.05</td>
      <td>1.16</td>
      <td>2.95</td>
      <td>1.54</td>
      <td>Champion REIT</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>1641</th>
      <td>2007</td>
      <td>1642</td>
      <td>Noble Group</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>运输</td>
      <td>13.75</td>
      <td>0.13</td>
      <td>3.81</td>
      <td>2.14</td>
      <td>Noble Group</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>1661</th>
      <td>2007</td>
      <td>1662</td>
      <td>Taiwan Mobile</td>
      <td>中国台湾(TA)</td>
      <td>电信运营商</td>
      <td>1.81</td>
      <td>0.50</td>
      <td>3.59</td>
      <td>4.84</td>
      <td>Taiwan Mobile</td>
      <td>NaN</td>
      <td>中国台湾</td>
      <td>CN-TA</td>
    </tr>
    <tr>
      <th>1681</th>
      <td>2007</td>
      <td>1682</td>
      <td>Evergreen Marine</td>
      <td>中国台湾(TA)</td>
      <td>运输</td>
      <td>4.29</td>
      <td>0.37</td>
      <td>3.96</td>
      <td>1.90</td>
      <td>Evergreen Marine</td>
      <td>NaN</td>
      <td>中国台湾</td>
      <td>CN-TA</td>
    </tr>
    <tr>
      <th>1692</th>
      <td>2007</td>
      <td>1693</td>
      <td>China Southern Airlines</td>
      <td>中国大陆(CN)</td>
      <td>运输</td>
      <td>4.64</td>
      <td>-0.23</td>
      <td>8.84</td>
      <td>1.97</td>
      <td>China Southern Airlines</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1705</th>
      <td>2007</td>
      <td>1706</td>
      <td>Cosco Pacific</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>运输</td>
      <td>0.30</td>
      <td>0.34</td>
      <td>2.85</td>
      <td>5.94</td>
      <td>Cosco Pacific</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>1710</th>
      <td>2007</td>
      <td>1711</td>
      <td>China Shipping Container</td>
      <td>中国大陆(CN)</td>
      <td>运输</td>
      <td>3.52</td>
      <td>0.44</td>
      <td>3.59</td>
      <td>2.26</td>
      <td>China Shipping Container</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1736</th>
      <td>2007</td>
      <td>1737</td>
      <td>China Resources Power Holdings</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>公用事业</td>
      <td>0.76</td>
      <td>0.37</td>
      <td>3.67</td>
      <td>5.37</td>
      <td>China Resources Power Holdings</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>1739</th>
      <td>2007</td>
      <td>1740</td>
      <td>Citic Securities</td>
      <td>中国大陆(CN)</td>
      <td>综合金融</td>
      <td>0.14</td>
      <td>0.04</td>
      <td>2.52</td>
      <td>14.29</td>
      <td>Citic Securities</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1780</th>
      <td>2007</td>
      <td>1781</td>
      <td>Far EasTone Telecom</td>
      <td>中国台湾(TA)</td>
      <td>电信运营商</td>
      <td>2.19</td>
      <td>0.45</td>
      <td>3.01</td>
      <td>4.45</td>
      <td>Far EasTone Telecom</td>
      <td>NaN</td>
      <td>中国台湾</td>
      <td>CN-TA</td>
    </tr>
    <tr>
      <th>1786</th>
      <td>2007</td>
      <td>1787</td>
      <td>E.Sun Financial</td>
      <td>中国台湾(TA)</td>
      <td>银行</td>
      <td>0.73</td>
      <td>0.14</td>
      <td>19.36</td>
      <td>2.19</td>
      <td>E.Sun Financial</td>
      <td>NaN</td>
      <td>中国台湾</td>
      <td>CN-TA</td>
    </tr>
    <tr>
      <th>1824</th>
      <td>2007</td>
      <td>1825</td>
      <td>Minmetals Development</td>
      <td>中国大陆(CN)</td>
      <td>贸易公司</td>
      <td>8.25</td>
      <td>0.04</td>
      <td>3.46</td>
      <td>1.50</td>
      <td>Minmetals Development</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1840</th>
      <td>2007</td>
      <td>1841</td>
      <td>Shanghai Automotive</td>
      <td>中国大陆(CN)</td>
      <td>耐用消费品</td>
      <td>0.79</td>
      <td>0.14</td>
      <td>1.81</td>
      <td>11.10</td>
      <td>Shanghai Automotive</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1846</th>
      <td>2007</td>
      <td>1847</td>
      <td>HK Exchanges &amp; Clearing</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>综合金融</td>
      <td>0.35</td>
      <td>0.17</td>
      <td>2.96</td>
      <td>10.97</td>
      <td>HK Exchanges &amp; Clearing</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>1852</th>
      <td>2007</td>
      <td>1853</td>
      <td>Link REIT</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>综合金融</td>
      <td>0.43</td>
      <td>0.27</td>
      <td>5.24</td>
      <td>5.00</td>
      <td>Link REIT</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>1860</th>
      <td>2007</td>
      <td>1861</td>
      <td>Kweichow Moutai</td>
      <td>中国大陆(CN)</td>
      <td>食品、饮料和烟草</td>
      <td>0.43</td>
      <td>0.14</td>
      <td>1.00</td>
      <td>10.69</td>
      <td>Kweichow Moutai</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1892</th>
      <td>2007</td>
      <td>1892</td>
      <td>Yanzhou Coal Mining</td>
      <td>中国大陆(CN)</td>
      <td>材料</td>
      <td>1.43</td>
      <td>0.36</td>
      <td>2.63</td>
      <td>4.52</td>
      <td>Yanzhou Coal Mining</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1908</th>
      <td>2007</td>
      <td>1909</td>
      <td>China Shipping Develop</td>
      <td>中国大陆(CN)</td>
      <td>运输</td>
      <td>1.06</td>
      <td>0.33</td>
      <td>1.66</td>
      <td>4.61</td>
      <td>China Shipping Develop</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1920</th>
      <td>2007</td>
      <td>1920</td>
      <td>Wing Lung Bank</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>银行</td>
      <td>0.66</td>
      <td>0.21</td>
      <td>10.92</td>
      <td>2.43</td>
      <td>Wing Lung Bank</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>1922</th>
      <td>2007</td>
      <td>1923</td>
      <td>Delta Electronics</td>
      <td>中国台湾(TA)</td>
      <td>技术硬件和装备</td>
      <td>2.46</td>
      <td>0.23</td>
      <td>2.49</td>
      <td>6.40</td>
      <td>Delta Electronics</td>
      <td>NaN</td>
      <td>中国台湾</td>
      <td>CN-TA</td>
    </tr>
    <tr>
      <th>1945</th>
      <td>2007</td>
      <td>1946</td>
      <td>China Airlines</td>
      <td>中国台湾(TA)</td>
      <td>运输</td>
      <td>3.61</td>
      <td>0.02</td>
      <td>7.63</td>
      <td>1.85</td>
      <td>China Airlines</td>
      <td>NaN</td>
      <td>中国台湾</td>
      <td>CN-TA</td>
    </tr>
    <tr>
      <th>1948</th>
      <td>2007</td>
      <td>1949</td>
      <td>Wing Hang Bank</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>银行</td>
      <td>0.66</td>
      <td>0.17</td>
      <td>13.45</td>
      <td>3.33</td>
      <td>Wing Hang Bank</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>1959</th>
      <td>2007</td>
      <td>1959</td>
      <td>PCCW</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>电信运营商</td>
      <td>2.90</td>
      <td>0.21</td>
      <td>6.87</td>
      <td>3.98</td>
      <td>PCCW</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>1960</th>
      <td>2007</td>
      <td>1961</td>
      <td>Benq</td>
      <td>中国台湾(TA)</td>
      <td>技术硬件和装备</td>
      <td>5.39</td>
      <td>-0.16</td>
      <td>5.04</td>
      <td>1.27</td>
      <td>Benq</td>
      <td>NaN</td>
      <td>中国台湾</td>
      <td>CN-TA</td>
    </tr>
    <tr>
      <th>1963</th>
      <td>2007</td>
      <td>1964</td>
      <td>TCL Corp</td>
      <td>中国大陆(CN)</td>
      <td>技术硬件和装备</td>
      <td>6.40</td>
      <td>-0.04</td>
      <td>3.77</td>
      <td>1.39</td>
      <td>TCL Corp</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1970</th>
      <td>2007</td>
      <td>1971</td>
      <td>Wuliangye Yibin</td>
      <td>中国大陆(CN)</td>
      <td>食品、饮料和烟草</td>
      <td>0.70</td>
      <td>0.10</td>
      <td>1.19</td>
      <td>8.81</td>
      <td>Wuliangye Yibin</td>
      <td>NaN</td>
      <td>中国大陆</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>1973</th>
      <td>2007</td>
      <td>1974</td>
      <td>CNPC (Hong Kong)</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>炼油</td>
      <td>0.44</td>
      <td>0.47</td>
      <td>2.07</td>
      <td>2.30</td>
      <td>CNPC (Hong Kong)</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>1975</th>
      <td>2007</td>
      <td>1976</td>
      <td>K Wah International</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>综合金融</td>
      <td>0.04</td>
      <td>0.47</td>
      <td>1.29</td>
      <td>0.98</td>
      <td>K Wah International</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>1986</th>
      <td>2007</td>
      <td>1987</td>
      <td>China Overseas Land &amp; Inv</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>综合金融</td>
      <td>0.90</td>
      <td>0.20</td>
      <td>3.24</td>
      <td>7.05</td>
      <td>China Overseas Land &amp; Inv</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
    <tr>
      <th>1989</th>
      <td>2007</td>
      <td>1989</td>
      <td>Nine Dragons Paper Holdings</td>
      <td>中国香港(HK)/中国大陆(CN)</td>
      <td>材料</td>
      <td>0.99</td>
      <td>0.17</td>
      <td>1.86</td>
      <td>8.61</td>
      <td>Nine Dragons Paper Holdings</td>
      <td>NaN</td>
      <td>中国香港</td>
      <td>CN-HK</td>
    </tr>
  </tbody>
</table>
<p>131 rows × 13 columns</p>
</div>



* 考虑到其他年份，公司所在行业有用英文名称展示的，这里添加一列英文的行业名称，但内容是空白


```python
df_2007['Industry_en'] = ''
df_2007.tail(3)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Rank</th>
      <th>Company_cn_en</th>
      <th>Country_cn_en</th>
      <th>Industry_cn</th>
      <th>Sales</th>
      <th>Profits</th>
      <th>Assets</th>
      <th>Market_value</th>
      <th>Company_en</th>
      <th>Company_cn</th>
      <th>Country_cn</th>
      <th>Country_en</th>
      <th>Industry_en</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1995</th>
      <td>2007</td>
      <td>1995</td>
      <td>Fremont General</td>
      <td>美国(US)</td>
      <td>综合金融</td>
      <td>1.25</td>
      <td>0.17</td>
      <td>12.80</td>
      <td>0.69</td>
      <td>Fremont General</td>
      <td>NaN</td>
      <td>美国</td>
      <td>US</td>
      <td></td>
    </tr>
    <tr>
      <th>1996</th>
      <td>2007</td>
      <td>1997</td>
      <td>United Rentals</td>
      <td>美国(US)</td>
      <td>商业服务和供应</td>
      <td>3.64</td>
      <td>0.22</td>
      <td>5.37</td>
      <td>2.32</td>
      <td>United Rentals</td>
      <td>NaN</td>
      <td>美国</td>
      <td>US</td>
      <td></td>
    </tr>
    <tr>
      <th>1997</th>
      <td>2007</td>
      <td>1998</td>
      <td>CBOT Holdings</td>
      <td>美国(US)</td>
      <td>综合金融</td>
      <td>0.64</td>
      <td>0.17</td>
      <td>0.81</td>
      <td>8.54</td>
      <td>CBOT Holdings</td>
      <td>NaN</td>
      <td>美国</td>
      <td>US</td>
      <td></td>
    </tr>
    <tr>
      <th>1998</th>
      <td>2007</td>
      <td>1998</td>
      <td>Singapore Petroleum</td>
      <td>新加坡(SI)</td>
      <td>炼油</td>
      <td>5.59</td>
      <td>0.19</td>
      <td>2.05</td>
      <td>1.50</td>
      <td>Singapore Petroleum</td>
      <td>NaN</td>
      <td>新加坡</td>
      <td>SI</td>
      <td></td>
    </tr>
    <tr>
      <th>1999</th>
      <td>2007</td>
      <td>2000</td>
      <td>DVB Bank</td>
      <td>德国(GE)</td>
      <td>银行</td>
      <td>0.77</td>
      <td>0.06</td>
      <td>12.74</td>
      <td>1.26</td>
      <td>DVB Bank</td>
      <td>NaN</td>
      <td>德国</td>
      <td>GE</td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>



* **将列名进行重新排序**


```python
columns_sort = ['Year', 'Rank', 'Company_cn_en','Company_en',
                'Company_cn', 'Country_cn_en', 'Country_cn',
                'Country_en', 'Industry_cn', 'Industry_en',
                'Sales', 'Profits', 'Assets', 'Market_value']
```


```python
# 按指定list重新将columns进行排序
df_2007 = df_2007.reindex(columns=columns_sort)
print(df_2007.shape)
print(df_2007.dtypes)
df_2007.head(3)
```

    (2000, 14)
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





<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Rank</th>
      <th>Company_cn_en</th>
      <th>Company_en</th>
      <th>Company_cn</th>
      <th>Country_cn_en</th>
      <th>Country_cn</th>
      <th>Country_en</th>
      <th>Industry_cn</th>
      <th>Industry_en</th>
      <th>Sales</th>
      <th>Profits</th>
      <th>Assets</th>
      <th>Market_value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2007</td>
      <td>1</td>
      <td>Citigroup /花旗集团</td>
      <td>Citigroup</td>
      <td>花旗集团</td>
      <td>美国(US)</td>
      <td>美国</td>
      <td>US</td>
      <td>银行</td>
      <td></td>
      <td>146.56</td>
      <td>21.54</td>
      <td>1884.32</td>
      <td>247.42</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2007</td>
      <td>2</td>
      <td>Bank of America /美国银行</td>
      <td>Bank of America</td>
      <td>美国银行</td>
      <td>美国(US)</td>
      <td>美国</td>
      <td>US</td>
      <td>银行</td>
      <td></td>
      <td>116.57</td>
      <td>21.13</td>
      <td>1459.74</td>
      <td>226.61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2007</td>
      <td>3</td>
      <td>HSBC Holdings/汇丰集团</td>
      <td>HSBC Holdings</td>
      <td>汇丰集团</td>
      <td>英国(UK)</td>
      <td>英国</td>
      <td>UK</td>
      <td>银行</td>
      <td></td>
      <td>121.51</td>
      <td>16.63</td>
      <td>1860.76</td>
      <td>202.29</td>
    </tr>
  </tbody>
</table>
</div>





{% include alert text='对我的文章感兴趣的朋友，可以关注我的微信公众号"Python数据之道"，接收我的更新通知。' %}

<div align="center"><img src="/images/qrcode.jpg" width="200"/></div>
