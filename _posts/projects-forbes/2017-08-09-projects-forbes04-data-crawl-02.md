---
layout: posts
title: "福布斯系列(4)-补充数据收集"
teaser:
date: 2017-08-09
header:
   image_fullwidth: "image-head.jpg"
categories:
  - projects
tags:
  - projects-forbes
  - Requests
  - BeautifulSoup
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



**福布斯系列之补充数据收集 - Python数据分析项目实战**

在上篇文章：《福布斯系列之数据完整性检查 - Python数据分析项目实战》，提到2013年的企业数量只有1991家，少于2000家。数据可能存在缺失与不完整。

于是，继续来寻找其他可用的数据源。

## 1 使用爬虫获取数据

通过网站搜索，发现Economy Watch网站有相关数据，且数据结构比较规范，质量较好，于是进行数据爬取。

网站地址如下：

* [Economy Watch：Forbes Global 2000: China's Largest Companies](http://www.economywatch.com/companies/forbes-list/china.html)


先打开网站，来查看数据情况，在这个页面上，我们关心的内容主要有两个部分。

1. 某个国家的数据列表，即数据表格。
1. 获得所有国家的信息列表的网页链接，方便后续从这些网页中爬取相关国家的数据。

这两部分在网站上对应的情况如下图所示：

<div align="center"><img src="/images/projects/forbes/complementary_data_collection/complementary_data_collection-1.jpg"></div>

<br>

<div align="center"><img src="/images/projects/forbes/complementary_data_collection/complementary_data_collection-2.jpg"></div>



**本次数据爬取大体分为几步：**

1. 起始页面内容的爬取
1. 起始页面上其他国家的网站链接的采集
1. 其他国家的网页数据的download与企业列表数据信息采集
1. 将采集的数据保存

涉及到的python库主要包括requests、BeautifulSoup以及csv。

下面来描述下数据采集的步骤。

先引入相关python库：

```python
# _*_ coding: utf-8 _*_

'''
# Code based on Python 3.x
# Author: "LEMON"
# 微信公众号ID：PyDataRoad
'''

import requests
from bs4 import BeautifulSoup
import csv
```


**模块：页面Download**

网站页面download页面是一个通用的模块，在所有国家的数据获取时都要用到。代码如下：

```python
def download(url):
    headers = {'User-Agent':'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36'}
    response = requests.get(url,headers=headers)
    # print(response.status_code)
    return response.text
```

**模块：企业表格信息**

在分析完网页结构之后，首先我们来获取起始页面上的信息，分别包括国家的企业信息数据和其他国家的网页链接信息。

可以分为两个模块来实现这些信息的获取。

国家的企业列表表格上的信息获取的模块代码如下：

```python
def get_content_first_page(html, country='China'):
    '''
    获取China的公司列表，且包含表头
    '''
    soup = BeautifulSoup(html, 'lxml')
    body = soup.body
    label_div = body.find('div', {'class': 'content post'})
    tables = label_div.find_all('table')

    # tables一共有2个，第一个才是我们想要的数据
    trs = tables[0].find_all('tr')

    # 获取标题行信息
    row_title = [td.text for td in trs[0].find_all('td')]
    row_title.insert(2, 'Country')

    data_list = []
    data_list.append(row_title)
    for i, tr in enumerate(trs):
        if i == 0:
            continue
        tds = tr.find_all('td')

        # 获取公司排名及列表
        row = [ item.text.strip() for item in tds]
        row.insert(2, country)
        data_list.append(row)
    return data_list
```

**模块：其他国家的网页链接**

从起始页面上获取其他国家的网页链接信息的模块如下：

```python
def get_country_info(html):
    '''
    获取除China外其他国家的网页链接及国家名称
    '''
    soup = BeautifulSoup(html, 'lxml')
    body = soup.body
    label_div = body.find('div', {'class': 'content post'})
    tables = label_div.find_all('table')

    label_a = tables[1].find_all('a')
    country_names = [item.text for item in label_a]
    page_urls = [ item.get('href') for item in label_a]
    country_info = list(zip(country_names, page_urls))
    return country_info
```
**模块：其他国家的企业表格信息**

对于其他国家而言，只需要在其对应的页面上获取企业列表信息即可，代码内容跟起始页面上的企业信息获取代码基本是一样的。

有一点差异，就是不需要再获取表格的标题信息了。

代码就不作赘述了。

**模块：信息保存**

由于信息量不大，且结构相对比较简单，保存到csv文件即可。

```python
def save_data_to_csv_file(data, file_name):
    '''
    保存数据到csv文件中
    '''
    with open(file_name, 'a', errors='ignore', newline='') as f:
        f_csv = csv.writer(f)
        f_csv.writerows(data)
```

**编写网页爬取主函数文件**

在各个模块编写好后，就可以在主函数里进行调用，从而爬取数据。如下：

```python
def get_forbes_global_year_2013():
    url = 'http://www.economywatch.com/companies/forbes-list/china.html'
    html = download(url)
    # print(html)

    data_first_page = get_content_first_page(html)
    # print(data_first_page)
    print('saving data ...', 'China')
    save_data_to_csv_file(data_first_page, 'forbes_economywatch_2013.csv')

    country_info = get_country_info(html)
    # print(country_info)

    for item in country_info:
        country_name = item[0]
        country_url = item[1]

        if country_name == 'China':
            continue

        html = download(country_url)
        data_other_country = get_content_other_country(html, country_name)
        # print(data_other_country)
        print('saving data ...', country_name)
        save_data_to_csv_file(data_other_country, 'forbes_economywatch_2013.csv')


if __name__ == '__main__':

    # get data from Forbes Global 2000 in Year 2013
    get_forbes_global_year_2013()
```

将数据爬取下来后，查看数据是否完整，通过pandas查看，发现Economy Watch上，2013年的企业数量只有 **1984家**，数据也不完整。

需要继续查找其他可靠的数据。

## 2 其他数据源收集

后来，在网站找到一个excel文件，发现其企业数量是2000家，初步认为数据是完整的。

在后续分析过程中，将会使用这个excel文件的企业数据信息。

所以说，数据收集的途径很多，关键是数据的质量要好，数据要完整，尽量少的出现异常情况。

至此，福布斯系列文章已经发布了多篇文章，**接下来要进入数据清洗的深水区**，欢迎大家持续关注~~



对我的文章感兴趣的朋友，可以关注我的微信公众号"Python数据之道"，接收我的更新通知.......

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
