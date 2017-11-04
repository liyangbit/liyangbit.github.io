---
layout: posts
title: "特朗普退出《巴黎协定》：python词云图舆情分析"
teaser:
date: 2017-06-03
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



## 1 前言
2017年6月1日，美国特朗普总统正式宣布美国退出《巴黎协定》。宣布退出《巴黎协定》后，特朗普似乎成了“全球公敌”。
* 特斯拉总裁马斯克宣布退出总统顾问团队
* 迪士尼董事长离开总统委员会
* 谷歌等25家大公司联名刊发整版广告：美国不应退出巴黎协定
* 法国总统马克龙：特朗普宣布退出《巴黎协定》是“错误”之举
* ......

在此背景下，本文以python词云图分析的方式来了解下相关情况。

## 2 特朗普演讲内容分析
首先来看下特朗普宣布退出《巴黎协定》时都说了啥。

特朗普宣布退出《巴黎协定》的演讲内容（英文）来自美国政府的官方网站，将其演讲内容存入文本文件中（statement.txt）。

对其演讲内容进行词云图分析后，得到如下结果：

![](http://upload-images.jianshu.io/upload_images/5462537-e6c8385a3a17f999.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从上图可看出，整个讲话中，"美国"（United states, America）出现的次数最多，其他的词语除巴黎协定外包括就业、工作等。
可见其核心关注点还是在“美国”，跟其就职演讲的口号“Make America Great Again”的基本思路是一致的，还是一切以美国为重。

**具体分析的代码如下：**
```python
# -*- coding: utf-8 -*-
"""
@author: lemon
欢迎关注微信公众号：“Python数据之道”
"""

import jieba
from wordcloud import WordCloud, ImageColorGenerator
import matplotlib.pyplot as plt
import os
import PIL.Image as Image
import numpy as np

# 读取文本内容
with open('statement.txt') as f:
    text = f.read()
    f.close()

# 首先使用 jieba 中文分词工具进行分词
wordlist = jieba.cut(text, cut_all=False)
# cut_all, True为全模式，False为精确模式

wordlist_space_split = ' '.join(wordlist)

d = os.path.dirname(__file__)
alice_coloring = np.array(Image.open(os.path.join(d,'colors.png')))
# my_wordcloud = WordCloud(background_color='#F0F8FF', max_words=50, mask=alice_coloring,
#                          max_font_size=300, random_state=42).generate(wordlist_space_split)

stop_words_list = ['applause', 'Applause']

# 对分词后的文本生成词云
my_wordcloud = WordCloud(background_color='#F0F8FF',
                         max_words=100,
                         font_step=1,
                         mask=alice_coloring,
                         random_state= 30, # 设置有多少种随机生成状态，即有多少种配色方案
                         max_font_size=300,
                         )
# Generate word cloud
my_wordcloud.generate(wordlist_space_split)

image_colors = ImageColorGenerator(alice_coloring)

plt.show(my_wordcloud.recolor(color_func=image_colors))
plt.imshow(my_wordcloud)            # 以图片的形式显示词云
plt.axis('off')                     # 关闭坐标轴
plt.show()

my_wordcloud.to_file(os.path.join(d, 'Pairs_accord_colors_cloud.jpg'))
```

## 3 新闻媒体舆论分析

特朗普宣布退出《巴黎协定》后，各方讨论纷纷，新闻媒体上更是炸开了锅，总的来看，似乎是批评的声音占多少。

例如世界资源所发布声明：
>特朗普总统退出《巴黎协定》的决定是巨大的错误

>退出《巴黎协定》将造成美国外交孤立，使之与近200个国家对立。现在有194个国家展现出团结一致应对气候变化的决心，而只有叙利亚、尼加拉瓜和美国这三个国家背道而驰。

下面通过收集部分新闻媒体发布的关于美国退出《巴黎协定》的评论内容，然后通过python词云图的方式来进行分析。

收集的新闻评论内容包括（只列出标题）：
* 特朗普宣布退出巴黎协议 欧洲称协议不能被重新谈判
* 特朗普宣布美国将退出《巴黎协定》 美各界反对
* 特朗普宣布美国将退出《巴黎协定》 美各界反对
* 特朗普宣布退出《巴黎协定》 众科技大佬纷纷发文反对
* 特朗普正式宣布退巴黎协定 中欧将联手反击
* 退出巴黎协定是特朗普一步错棋
* 《巴黎协定》究竟哪里得罪了特朗普？
* 世界资源研究所声明：特朗普总统退出《巴黎协定》的决定是巨大的错误

词云图以特朗普的一张图片为背景轮廓来进行分析，采用的图片如下（川普的造型还是很拽滴！）：

![](http://upload-images.jianshu.io/upload_images/5462537-120e589ad4a26be3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过词云图分析后，得到的结果如下：

![](http://upload-images.jianshu.io/upload_images/5462537-12c0cdf2eba5899a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

具体代码跟上述分析过程类似，这里就不贴出来了。

通过上图可以看出，这个事件最出名的当然是特朗普了。从词频来看，还是“美国”出现次数最多，特朗普以美国为重的执政理念通过这些舆论也能从某些程度上体现一些出来。

## 4 闲谈
当然，客观的来说，特朗普宣布美国退出《巴黎协定》，其影响实际上应该是没有当年小布什政府宣布退出《京都议定书》那时的影响大，其主要原因包括两点：
* 《巴黎协定》目前已于2016年生效，全球已有190多个国家参与，美国退出的影响力有限。
* 美国退出《京都议定书》后，导致《京都议定书》推迟了4年才生效。







<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="200">
</div>
