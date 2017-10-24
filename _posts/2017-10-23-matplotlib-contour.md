---
layout: posts
title: "Matplotlib 中等高线图（contour）的绘制"
teaser:
date: 2017-10-23
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PythonVisualization
tags:
   - Matplotlib
sidebar: right
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


## 前言

在此前的文章里，我们介绍了 Numpy 中 meshgrid 函数的应用，并提到了等高线图的使用。

今天，我们来介绍一下 matplotlib 中等高线图的绘制。

先上图来看一下等高线图的绘制效果，是不是很炫啊。

<div align="center">
    <img src="/images/posts/matplotlib-contour/matplotlib-contour01.png">
</div>


下面我们来分步骤介绍 matplotlib 中等高线图（contour）的绘制。

**数据初始化**

```python
import matplotlib
import numpy as np
import matplotlib.cm as cm
import matplotlib.mlab as mlab
import matplotlib.pyplot as plt

%matplotlib inline

# 定义等高线高度函数
def f(x, y):
    return (1 - x / 2 + x ** 5 + y ** 3) * np.exp(- x ** 2 - y ** 2)

# 数据数目
n = 256
# 定义x, y
x = np.linspace(-3, 3, n)
y = np.linspace(-3, 3, n)

# 生成网格数据
X, Y = np.meshgrid(x, y)
```
注：数据初始化过程中用到了 Numpy 的 meshgrid 函数，有兴趣的同学可以了解下这个函数的使用，链接如下：

* [Numpy中Meshgrid函数介绍及2种应用场景](http://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247484206&idx=1&sn=f10690c085b504123e9c526947e7e4c1&chksm=ea8b6c5dddfce54b7cd3ade5d1076572da560b8d47dcdeae190c1c9b9794623cf6a9b6e292b4&scene=21#wechat_redirect)

## 仅绘制等高线

```python
# 设置图像大小尺寸
plt.figure(figsize=(10,6))

# 填充等高线的颜色, 8是等高线分为几部分
plt.contour(X, Y,f(X, Y), 8,alpha = 0.75, cmap = plt.cm.hot)
```

结果如下：

<div align="center">
    <img src="/images/posts/matplotlib-contour/matplotlib-contour02.png">
</div>


## 仅填充等高区域颜色：

```python
# 设置图像大小尺寸
plt.figure(figsize=(10,6))

# 填充等高区域的颜色, 8是等高线分为几部分
plt.contourf(X, Y,f(X, Y), 8, alpha = 0.75, cmap = plt.cm.hot)
```

结果如下：

<div align="center">
    <img src="/images/posts/matplotlib-contour/matplotlib-contour03.png">
</div>

## 绘制完整的等高线图

```python
# 设置图像大小尺寸
plt.figure(figsize=(10,6))

# 填充等高线的颜色, 8是等高线分为几部分
plt.contourf(X, Y, f(X, Y), 8, alpha = 0.75, cmap = plt.cm.hot)

C = plt.contour(X,Y, f(X,Y), 8, colors='black', linewidth=8)

# 绘制等高线数据
plt.clabel(C, inline = True, fontsize = 10)

# 去除坐标轴
plt.xticks(())
plt.yticks(())

plt.show()
```

结果如下：

<div align="center">
    <img src="/images/posts/matplotlib-contour/matplotlib-contour04.png">
</div>

是不是很容易实现啊，赶快动手来试试吧。

<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="192">
</div>
