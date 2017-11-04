---
layout: posts
title: "Python分析：50年高考作文题，记录时代变迁"
teaser:
date: 2017-06-05
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
今天在办公室里笑着问同事，过几天要高考了，你紧张么？
同事淡定的说，你又不高考，紧张啥。

是啊，高考已经过去好多年了。但当年的情况还仿佛历历在目，每年高考，多少莘莘学子憧憬着美好的未来。

每年的高考作文题，都是考试后社会或者新闻媒体追踪的热点，尤其是满分作文或者是零分作文。

## 50年高考作文题目
从1952年到2016年，已经历了50多年的高考（1966-1976年中断了11年），通过观察每年的高考作文题目，可以从某些方面看出时代变迁的一些情况。

本文以10年为一个阶段（1966年前为一个阶段），通过词云图分析的方式来展现不同时期的主题方向，记录哪些曾经令人兴奋或激动的时刻。

**1952-1965**

在这个时代，革命、锻炼等词出现较多。
![](http://upload-images.jianshu.io/upload_images/5462537-67baec5a9235ff2f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



**1977-1986**

78年后，逐步开始改革，开始迈向开放。
![](http://upload-images.jianshu.io/upload_images/5462537-c1e0e9843637e6bf.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**1987-1997**

这个时期的高考作文题，逐步开始接近生活，出现了漫画等词。
![](http://upload-images.jianshu.io/upload_images/5462537-47d77597d86e9d57.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**1997-2006**

这个时期，文体较丰富，自由度也比较好。
![](http://upload-images.jianshu.io/upload_images/5462537-a97cba898a1ff390.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**2007-2016**

内容趋向丰富多彩，更接近日常生活。
![](http://upload-images.jianshu.io/upload_images/5462537-6bf46fb03fa7b361.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**历年高考作文题汇总：1952-2016**

![](http://upload-images.jianshu.io/upload_images/5462537-ef46e501ac96acc8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


总的来看，通过观察不同时期高考作文题目的词云图，还是可以从某些方面看出时代变迁的痕迹。

当然，要是能够获取数量足够多的不同时期的高考作文，那样的分析结果应该更具有代表性。

**源代码**
>关于本文分析所有的高考作文题目文本内容以及分析的源代码，请关注我的公众号“Python数据之道”，回复数字“1”来获取。





<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="200">
</div>
