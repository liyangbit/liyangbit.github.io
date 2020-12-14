---
layout: posts
title: "Matplotlib 动态图还可以这样玩，收藏了！"
teaser:
date: 2020-12-15
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PythonVisualization
tags:    
   - Matplotlib
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---

{% include alert info='利用 Matplotlib 创建gif动态图' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# Matplotlib 动态图还可以这样玩，收藏了！

大家好，我是 Lemon。

Matplotlib 是 Python 数据可视化中不可或缺的一个可视化库，「Python数据之道」之前也给大家分享了一系列 Matplotlib 从基础到进阶的内容，包括：

今天给大家分享的是使用 Matplotlib 来创建动态 `gif` 图片，这个内容是以前没有分享过的。 但 Lemon 觉得今天的内容也是特别的实用。

## 4种类型的动态 gif 图

今天给大家分享的包括4种类型的动态 gif 图，分别是单曲线动态图、分段分不同背景颜色的动态图、双曲线动态图、多图多曲线动态图。

核心思路是通过 Matplotlib 绘制动态图的动画帧，然后通过 python 的 'gif' 库来绘制动态图片。

### 单曲线动态图

先来看看单曲线动态图，效果如下：

![](/images/posts/202012-matplotlib-gif/mat-1.gif)

<!-- /Users/lemon/Documents/Github/liyangbit.github.io/images/posts/202012-matplotlib-gif/mat-1.gif -->

效果是不是很赞啊，另外 3 种动态图的代码实现过程也是类似的，其效果如下：

### 分段分不同背景颜色的动态图

![](/images/posts/202012-matplotlib-gif/mat-2.gif)

### 双曲线动态图

![](/images/posts/202012-matplotlib-gif/mat-3.gif)

### 多图多曲线动态图

![](/images/posts/202012-matplotlib-gif/mat-4.gif)
