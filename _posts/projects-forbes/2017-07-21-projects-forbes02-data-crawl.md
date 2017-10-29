---
layout: posts
title: "福布斯系列(2)-数据采集"
teaser:
date: 2017-07-21
header:
   image_fullwidth: "image-head.jpg"
categories:
  - projects
tags:
  - Requests
  - BeautifulSoup
  - projects-forbes
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



## 1 数据采集概述

开始一个数据分析项目，首先需要做的就是get到原始数据，获得原始数据的方法有多种途径。比如：

1. 获取数据集（dataset）文件
1. 使用爬虫采集数据
1. 直接获得excel、csv及其他数据文件
1. 其他途径...

本次福布斯系列数据分析项目实战，数据采集方面，主要数据来源于使用爬虫进行数据采集，同时也辅助其他数据进行对比。

本文主要是介绍使用爬虫进行数据采集的思路和步骤。

本次采集的福布斯全球上市企业2000强排行榜数据，涉及年份从2007年到2017年，跨越10多年。

本次采集的目标网站，是多个网页，但多个网页的分布结构都有所不同，虽然思路和步骤都差不多，但需要分开来编写，分别采集。

## 2 数据采集步骤

数据采集大体分为几步：

1. 目标主网页内容的Download
1. 主网页上数据的采集
1. 主网页上其他分发页面网站链接的采集
1. 各分发网页数据的download与采集
1. 将采集的数据保存

涉及到的python库包括，requests、BeautifulSoup以及csv。 下面以采集某年的数据为案例，来描述下数据采集的步骤。

```python
import requests
from bs4 import BeautifulSoup
import csv
```

### 2.1 数据Download模块
主要是基于 requests，代码如下：

```python
def download(url):
    headers = {'User-Agent':'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36'}
    response = requests.get(url,headers=headers)
    # print(response.status_code)
    return response.text
```
这个模块会在主网页数据下载，以及各个分页面数据下载时使用，是一个比较通用的模块。


### 2.2 主网页上数据的采集

主网页的页面结构，主要分为两个部分，一类是包含其他页面数据的网页链接，一类是主网页上的公司数据列表，以表格形式在网页上显示。

用BeautifulSoup可以把这些数据解析出来。 代码模块如下：

* 解析主网页上的公司数据列表信息

```python
def get_content_first_page(html, year):
    '''
    获取排名在1-100的公司列表，且包含表头
    '''
    soup = BeautifulSoup(html, 'lxml')
    body = soup.body
    body_content = body.find('div', {'id': 'bodyContent'})
    tables = body_content.find_all('table', {'class': 'XXXXtable'})

    # tables一共有3个，最后一个才是我们想要的
    trs = tables[-1].find_all('tr')

    # 获取表头名称
    # trs[1], 这里跟其他年份不一样
    row_title = [item.text.strip() for item in trs[1].find_all('th')]
    row_title.insert(0, '年份')

    rank_list = []
    rank_list.append(row_title)
    for i, tr in enumerate(trs):
        if i == 0 or i == 1:
            continue
        tds = tr.find_all('td')

        # 获取公司排名及列表
        row = [ item.text.strip() for item in tds]
        row.insert(0, year)
        rank_list.append(row)
    return rank_list
```

* 解析主网页上其他页面的网页链接

```python
def get_page_urls(html):
    '''
    获取排名在101-2000的公司的网页链接
    '''
    soup = BeautifulSoup(html, 'lxml')
    body = soup.body
    body_content = body.find('div', {'id': 'bodyContent'})
    label_div = body_content.find('div', {'align':'center'})
    label_a = label_div.find('p').find('b').find_all('a')

    page_urls = ['basic_url' + item.get('href') for item in label_a]
    return page_urls
```
### 2.3 各个分发页面上的数据采集

步骤也是 网页页面下载 和表格类数据爬取。 代码内容跟主网页页面类似，只是细节上有些差异，这里就不作赘述了。

### 2.4 数据存储

采集的数据，最后保存到csv文件中。模块代码如下：

```python
def save_data_to_csv_file(data, file_name):
    '''
    保存数据到csv文件中
    '''
    with open(file_name, 'a', errors='ignore', newline='') as f:
        f_csv = csv.writer(f)
        f_csv.writerows(data)
```

### 2.5 数据采集主函数

```python
def get_forbes_global_year_2007(year=2007):
    url = 'url'
    html = download(url)
    # print(html)

    data_first_page = get_content_first_page(html, year)
    # print(data_first_page)
    save_data_to_csv_file(data_first_page, 'forbes_'+str(year)+'.csv')

    page_urls = get_page_urls(html)
    # print(page_urls)

    for url in page_urls:
        html = download(url)
        data_other_page = get_content_other_page(html, year)
        # print(data_other_page)
        print('saving data ...', url)
        save_data_to_csv_file(data_other_page, 'forbes_'+str(year)+'.csv')


if __name__ == '__main__':

    # get data from Forbes Global 2000 in Year 2009
    get_forbes_global_year_2007()
```

## 3 总结

本文只介绍了数据采集的思路与各个模块，并没有提供目标网页的链接， 一方面由于原始网页的数据信息比较杂乱，采集的时候需要写多个采集程序，另外一方面，由于我们的重点在于后续的数据分析部分，希望不要着重于数据爬取。

在后续的分析过程中，我们会来查看数据的结构、数据完整性及相关信息，欢迎继续关注。


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
