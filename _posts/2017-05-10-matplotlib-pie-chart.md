---
layout: posts
title: "关于matplotlib，你要的饼图在这里"
teaser:
date: 2017-05-10
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PythonVisualization
tags:
   - Matplotlib
   - Pandas
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


 >matplotlib， 官方提供的饼图Demo，功能比较比较简单，在实际应用过程中，往往会有许多个性化的绘制需求，在这里跟大家一起了解下饼图（pie chart）的一些特色的功能的实现。

```python
from matplotlib import font_manager as fm
import matplotlib as mpl
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
% matplotlib inline
plt.style.use('ggplot')
```

## 1. 官方Demo


```python
import matplotlib.pyplot as plt

# Pie chart, where the slices will be ordered and plotted counter-clockwise:
labels = 'Frogs', 'Hogs', 'Dogs', 'Logs'
sizes = [15, 30, 45, 10]
explode = (0, 0.1, 0, 0)  # only "explode" the 2nd slice (i.e. 'Hogs')

fig1, ax1 = plt.subplots()
ax1.pie(sizes, explode=explode, labels=labels, autopct='%1.1f%%',
        shadow=True, startangle=90)
ax1.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.

plt.savefig('Demo_official.jpg')
plt.show()
```

![Demo_official.jpg](http://upload-images.jianshu.io/upload_images/5462537-e944a162e16ad080.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 2. 将实际数据应用于官方Demo


```python
# 原始数据
shapes = ['Cross', 'Cone', 'Egg', 'Teardrop', 'Chevron', 'Diamond', 'Cylinder',
       'Rectangle', 'Flash', 'Cigar', 'Changing', 'Formation', 'Oval', 'Disk',
       'Sphere', 'Fireball', 'Triangle', 'Circle', 'Light']
values = [  287,   383,   842,   866,  1187,  1405,  1495,  1620,  1717,
        2313,  2378,  3070,  4332,  5841,  6482,  7785,  9358,  9818, 20254]

s = pd.Series(values, index=shapes)
s
```


```python
from matplotlib import font_manager as fm
import matplotlib as mpl

# Pie chart, where the slices will be ordered and plotted counter-clockwise:
labels = s.index
sizes = s.values
explode = (0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0)  # only "explode" the 1st slice

fig1, ax1 = plt.subplots()
patches, texts, autotexts = ax1.pie(sizes, explode=explode, labels=labels, autopct='%1.0f%%',
        shadow=False, startangle=170)
ax1.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.

plt.savefig('Demo_project.jpg')
plt.show()
```

![Demo_project.jpg](http://upload-images.jianshu.io/upload_images/5462537-eead622b9bfac772.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**上图的一些问题：**
1. 颜色比较生硬
2. 部分文字拥挤在一起，绘图显示不齐整


# 3. 一些改善措施

* 重新设置字体大小
* 设置自选颜色
* 设置图例
* 将某些类别突出显示

### 3.1 重新设置字体大小


```python
from matplotlib import font_manager as fm
import matplotlib as mpl

labels = s.index
sizes = s.values
explode = (0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0)  # only "explode" the 1st slice

fig1, ax1 = plt.subplots()

patches, texts, autotexts = ax1.pie(sizes, explode=explode, labels=labels, autopct='%1.0f%%',
        shadow=False, startangle=170)
ax1.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.

# 重新设置字体大小
proptease = fm.FontProperties()
proptease.set_size('xx-small')
# font size include: ‘xx-small’,x-small’,'small’,'medium’,‘large’,‘x-large’,‘xx-large’ or number, e.g. '12'
plt.setp(autotexts, fontproperties=proptease)
plt.setp(texts, fontproperties=proptease)

plt.savefig('Demo_project_set_font.jpg')
plt.show()
```

![Demo_project_set_font.jpg](http://upload-images.jianshu.io/upload_images/5462537-a9a94ff64faa5e63.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3.2 设置显示颜色，Method 1：


```python
from matplotlib import font_manager as fm
import matplotlib as mpl

# Pie chart, where the slices will be ordered and plotted counter-clockwise:
labels = s.index
sizes = s.values
explode = (0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0)  # only "explode" the 1st slice

fig1, ax1 = plt.subplots(figsize=(6,6)) # 设置绘图区域大小

a = np.random.rand(1,19)
color_vals = list(a[0])
my_norm = mpl.colors.Normalize(-1, 1) # 将颜色数据的范围设置为 [0, 1]
my_cmap = mpl.cm.get_cmap('rainbow', len(color_vals)) # 可选择合适的colormap，如：'rainbow'

patches, texts, autotexts = ax1.pie(sizes, explode=explode, labels=labels, autopct='%1.0f%%',
        shadow=False, startangle=170, colors=my_cmap(my_norm(color_vals)))

ax1.axis('equal')  

# 重新设置字体大小
proptease = fm.FontProperties()
proptease.set_size('xx-small')
# font size include: ‘xx-small’,x-small’,'small’,'medium’,‘large’,‘x-large’,‘xx-large’ or number, e.g. '12'
plt.setp(autotexts, fontproperties=proptease)
plt.setp(texts, fontproperties=proptease)

plt.savefig('Demo_project_set_color_1.jpg')
plt.show()
```

![Demo_project_set_color_1.jpg](http://upload-images.jianshu.io/upload_images/5462537-632e62953e4edfdd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面这种方法设置颜色时，但类别比较多时，部分颜色的填充会重复。

有时候，我们可能想设置成连续的颜色，可以有另外一种方法来实现。

### 3.3 设置显示颜色， Method 2：


```python
from matplotlib import font_manager as fm
from  matplotlib import cm

labels = s.index
sizes = s.values
# explode = (0.2,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0)  # only "explode" the 1st slice

fig, ax = plt.subplots(figsize=(6,6)) # 设置绘图区域大小


colors = cm.rainbow(np.arange(len(sizes))/len(sizes)) # colormaps: Paired, autumn, rainbow, gray,spring,Darks
patches, texts, autotexts = ax.pie(sizes, labels=labels, autopct='%1.0f%%',
        shadow=False, startangle=170, colors=colors)

ax.axis('equal')  
ax.set_title('Shapes -------------------', loc='left')

# 重新设置字体大小
proptease = fm.FontProperties()
proptease.set_size('xx-small')
# font size include: ‘xx-small’,x-small’,'small’,'medium’,‘large’,‘x-large’,‘xx-large’ or number, e.g. '12'
plt.setp(autotexts, fontproperties=proptease)
plt.setp(texts, fontproperties=proptease)

plt.savefig('Demo_project_set_color_2.jpg')
plt.show()
```

![Demo_project_set_color_2.jpg](http://upload-images.jianshu.io/upload_images/5462537-c97bd3d41eddd5a4.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从上图可以看出，颜色显示是连续的，实现了我们想要的效果

### 3.4 设置图例（legend）


```python
from matplotlib import font_manager as fm
from  matplotlib import cm

labels = s.index
sizes = s.values
# explode = (0.2,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0)  # only "explode" the 1st slice

fig, ax = plt.subplots(figsize=(6,6)) # 设置绘图区域大小


colors = cm.rainbow(np.arange(len(sizes))/len(sizes)) # colormaps: Paired, autumn, rainbow, gray,spring,Darks
patches, texts, autotexts = ax.pie(sizes, labels=labels, autopct='%1.0f%%',
        shadow=False, startangle=170, colors=colors)

ax.axis('equal')  

# 重新设置字体大小
proptease = fm.FontProperties()
proptease.set_size('xx-small')
# font size include: ‘xx-small’,x-small’,'small’,'medium’,‘large’,‘x-large’,‘xx-large’ or number, e.g. '12'
plt.setp(autotexts, fontproperties=proptease)
plt.setp(texts, fontproperties=proptease)


ax.legend(labels, loc=2)

plt.savefig('Demo_project_set_legend_error.jpg')
plt.show()
```

![Demo_project_set_legend_error.jpg](http://upload-images.jianshu.io/upload_images/5462537-9e56dac15e3a1c2a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从上面可看出，当类别较多时，图例（legend）的位置摆放显示有重叠，显示有些问题，需要进行调整。

### 3.5 重新设置图例（legend）


```python
from matplotlib import font_manager as fm
from  matplotlib import cm

labels = s.index
sizes = s.values
# explode = (0.2,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0)  # only "explode" the 1st slice

fig, axes = plt.subplots(figsize=(10,5),ncols=2) # 设置绘图区域大小
ax1, ax2 = axes.ravel()

colors = cm.rainbow(np.arange(len(sizes))/len(sizes)) # colormaps: Paired, autumn, rainbow, gray,spring,Darks
patches, texts, autotexts = ax1.pie(sizes, labels=labels, autopct='%1.0f%%',
        shadow=False, startangle=170, colors=colors)

ax1.axis('equal')  

# 重新设置字体大小
proptease = fm.FontProperties()
proptease.set_size('xx-small')
# font size include: ‘xx-small’,x-small’,'small’,'medium’,‘large’,‘x-large’,‘xx-large’ or number, e.g. '12'
plt.setp(autotexts, fontproperties=proptease)
plt.setp(texts, fontproperties=proptease)

ax1.set_title('Shapes', loc='center')

# ax2 只显示图例（legend）
ax2.axis('off')
ax2.legend(patches, labels, loc='center left')

plt.tight_layout()
plt.savefig('Demo_project_set_legend_good.jpg')
plt.show()
```

![Demo_project_set_legend_good.jpg](http://upload-images.jianshu.io/upload_images/5462537-d67fb5eb511c5755.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3.6 将某些类别突出显示
* 将某些类别突出显示
* 控制label的显示位置
* 控制百分比的显示位置
* 控制突出位置的大小


```python
from matplotlib import font_manager as fm
from  matplotlib import cm

labels = s.index
sizes = s.values
explode = (0.1,0,0,0,0,0,0,0,0,0,0,0,0,0.2,0,0,0,0.1,0)  # "explode" ， show the selected slice

fig, axes = plt.subplots(figsize=(8,5),ncols=2) # 设置绘图区域大小
ax1, ax2 = axes.ravel()

colors = cm.rainbow(np.arange(len(sizes))/len(sizes)) # colormaps: Paired, autumn, rainbow, gray,spring,Darks
patches, texts, autotexts = ax1.pie(sizes, labels=labels, autopct='%1.0f%%',explode=explode,
        shadow=False, startangle=170, colors=colors, labeldistance=1.2,pctdistance=1.03, radius=0.4)
# labeldistance: 控制labels显示的位置
# pctdistance: 控制百分比显示的位置
# radius: 控制切片突出的距离

ax1.axis('equal')  

# 重新设置字体大小
proptease = fm.FontProperties()
proptease.set_size('xx-small')
# font size include: ‘xx-small’,x-small’,'small’,'medium’,‘large’,‘x-large’,‘xx-large’ or number, e.g. '12'
plt.setp(autotexts, fontproperties=proptease)
plt.setp(texts, fontproperties=proptease)

ax1.set_title('Shapes', loc='center')

# ax2 只显示图例（legend）
ax2.axis('off')
ax2.legend(patches, labels, loc='center left')

plt.tight_layout()
# plt.savefig("pie_shape_ufo.png", bbox_inches='tight')
plt.savefig('Demo_project_final.jpg')
plt.show()
```

![Demo_project_final.jpg](http://upload-images.jianshu.io/upload_images/5462537-d6cfab586e78d139.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="200">
</div>
