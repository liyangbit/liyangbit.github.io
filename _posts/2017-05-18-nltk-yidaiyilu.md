---
layout: posts
title: "Python分析：用词云图分析一带一路峰会哪3个词说的最多"
teaser:
date: 2017-05-18
header:
   image_fullwidth: "image-head.jpg"
categories:
   - NLTK
tags:
   - Jieba
   - Wordclound
   - Matplotlib
   - Numpy
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---




{% include alert info='本文首发于我的微信公众号（ID：PyDataRoad）。' %}


<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>



## 前言
最近几日关注度最高的新闻莫过于一带一路峰会相关的消息，会议结束后，一带一路峰会联合公告已经公布出来了。
本文通过词云分析，了解本次公告以及习大大在峰会开幕式上主要都讲了哪些关键内容。

## 1 一带一路峰会联合公告词云图
5月17日公布的一带一路峰会联合公告的词云分析结果图，如下：

![ydyl_gb_colors_cloud.png](http://upload-images.jianshu.io/upload_images/5462537-c5e59189d25a5957.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

词云图上，字体越大表示该词语在文件中出现的次数越多。

从上图可以看出，出现次数最多的3个词语为 **“合作”、“我们”以及“加强”** ，基本可以看出，本次峰会是一个新的起点，今后需要做的事情还很多，需要各方务实合作，哈。

## 2 一带一路峰会开幕式上习大大的讲话词云图
分析开幕式上习大大的讲话内容，得到词云图如下：

![ydyl_orange_cloud.png](http://upload-images.jianshu.io/upload_images/5462537-0d6937f67f47bd5c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


从上图可看出，出现次数最多的几个词语为“一带一路”、“合作”、“我们”以及“发展”等。


## 3 用python制作词云图
下面主要来介绍下用python制作这个词云图的过程，主要分为如下几个步骤：
* 从txt文件读取需要分析的文本内容
* 使用jieba分词工具进行中文分词
* 选取合适的显示图形
* 用wordcloud进行词云图显示
* 保存图片

一带一路峰会联合公告词云图实现的代码如下：
```python
# -*- coding: utf-8 -*-
"""
Created on Wed May 17 2017

@author: lemon
"""

import jieba
from wordcloud import WordCloud, ImageColorGenerator
import matplotlib.pyplot as plt
import os
import PIL.Image as Image
import numpy as np


with open('ydyl_gb.txt', 'rb') as f:
# 读取一带一路峰会联合公报的内容
# with open('yidaiyilu.txt', 'rb') as f:
    text = f.read()
    f.close()


# 首先使用 jieba 中文分词工具进行分词
wordlist = jieba.cut(text, cut_all=False)      
# cut_all, True为全模式，False为精确模式

wordlist_space_split = ' '.join(wordlist)

d = os.path.dirname(__file__)
alice_coloring = np.array(Image.open(os.path.join(d,'colors.png')))
my_wordcloud = WordCloud(background_color='#F0F8FF', max_words=100, mask=alice_coloring,
                         max_font_size=300, random_state=42).generate(wordlist_space_split)

image_colors = ImageColorGenerator(alice_coloring)

plt.show(my_wordcloud.recolor(color_func=image_colors))
plt.imshow(my_wordcloud)            # 以图片的形式显示词云
plt.axis('off')                     # 关闭坐标轴
plt.show()

my_wordcloud.to_file(os.path.join(d, 'ydyl_gb_colors_cloud.png'))
```





<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="200">
</div>
