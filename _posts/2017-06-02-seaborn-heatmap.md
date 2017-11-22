---
layout: posts
title: "Python可视化：Seaborn库热力图使用进阶"
teaser:
date: 2017-06-02
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PythonVisualization
tags:
   - Seaborn
   - Pandas
   - Matplotlib
   - Numpy
   - recommend
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

在日常工作中，经常可以见到各种各种精美的热力图，热力图的应用非常广泛，下面一起来学习下Python的Seaborn库中热力图（heatmap）如何来进行使用。

本次运行的环境为：

* windows 64位系统

* python 3.5

* jupyter notebook

  ​


## 1 构造数据


```python
import seaborn as sns
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
% matplotlib inline
```


```python
region = ['Albania', 'Algeria', 'Angola', 'Argentina', 'Armenia', 'Azerbaijan',
       'Bahamas', 'Bangladesh', 'Belize', 'Bhutan', 'Bolivia',
       'Bosnia and Herzegovina', 'Brazil', 'Burkina Faso', 'Burundi',
       'Cambodia', 'Cameroon', 'Cape Verde', 'Chile', 'China', 'Colombia',
       'Costa Rica', 'Cote d Ivoire', 'Cuba', 'Cyprus',
       "Democratic People's Republic of Korea",
       'Democratic Republic of the Congo', 'Dominican Republic', 'Ecuador',
       'Egypt', 'El Salvador', 'Equatorial Guinea', 'Ethiopia', 'Fiji',
       'Gambia', 'Georgia', 'Ghana', 'Guatemala', 'Guyana', 'Honduras']


kind = ['Afforestation & reforestation', 'Biofuels', 'Biogas',
        'Biomass', 'Cement', 'Energy efficiency', 'Fuel switch',
       'HFC reduction/avoidance', 'Hydro power',
        'Leak reduction', 'Material use', 'Methane avoidance',             
       'N2O decomposition', 'Other renewable energies',
       'PFC reduction and substitution','PV',
       'SF6 replacement', 'Transportation', 'Waste gas/heat utilization',
      'Wind power']
```


```python
print(len(region))
print(len(kind))
```

    40
    20



```python
np.random.seed(100)
arr_region = np.random.choice(region, size=(10000,))
list_region = list(arr_region)

arr_kind = np.random.choice(kind, size=(10000,))
list_kind = list(arr_kind)

values = np.random.randint(50, 1000, 10000)
list_values = list(values)

df = pd.DataFrame({'region':list_region,
                  'kind': list_kind,
                  'values':list_values})
df.head()
```

![](http://upload-images.jianshu.io/upload_images/5462537-d066a840f891f38a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



```python
pt = df.pivot_table(index='kind', columns='region', values='values', aggfunc=np.sum)
pt.head()
```

![](http://upload-images.jianshu.io/upload_images/5462537-82289d002053c987.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




```python
f, ax = plt.subplots(figsize = (10, 4))
cmap = sns.cubehelix_palette(start = 1, rot = 3, gamma=0.8, as_cmap = True)
sns.heatmap(pt, cmap = cmap, linewidths = 0.05, ax = ax)
ax.set_title('Amounts per kind and region')
ax.set_xlabel('region')
ax.set_ylabel('kind')

f.savefig('sns_heatmap_normal.jpg', bbox_inches='tight')
# ax.set_xticklabels(ax.get_xticklabels(), rotation=-90)
```



![](http://upload-images.jianshu.io/upload_images/5462537-898bb3f6daff71cf.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




## 2 Seaborn的heatmap各个参数介绍

**seaborn.heatmap**

seaborn.heatmap(data, vmin=None, vmax=None, cmap=None, center=None, robust=False, annot=None, fmt='.2g', annot_kws=None, linewidths=0, linecolor='white', cbar=True, cbar_kws=None, cbar_ax=None, square=False, ax=None, xticklabels=True, yticklabels=True, mask=None, \*\*kwargs)

* data：矩阵数据集，可以使numpy的数组（array），如果是pandas的dataframe，则df的index/column信息会分别对应到heatmap的columns和rows
* linewidths,热力图矩阵之间的间隔大小
* vmax,vmin, 图例中最大值和最小值的显示值，没有该参数时默认不显示

### 2.1 cmap

* cmap：matplotlib的colormap名称或颜色对象；如果没有提供，默认为cubehelix map (数据集为连续数据集时) 或 RdBu_r (数据集为离散数据集时)


```python
f, (ax1,ax2) = plt.subplots(figsize = (10, 8),nrows=2)

# cubehelix map颜色
cmap = sns.cubehelix_palette(start = 1.5, rot = 3, gamma=0.8, as_cmap = True)
sns.heatmap(pt, linewidths = 0.05, ax = ax1, vmax=15000, vmin=0, cmap=cmap)
ax1.set_title('cubehelix map')
ax1.set_xlabel('')
ax1.set_xticklabels([]) #设置x轴图例为空值
ax1.set_ylabel('kind')

# matplotlib colormap
sns.heatmap(pt, linewidths = 0.05, ax = ax2, vmax=15000, vmin=0, cmap='rainbow')
# rainbow为 matplotlib 的colormap名称
ax2.set_title('matplotlib colormap')
ax2.set_xlabel('region')
ax2.set_ylabel('kind')

f.savefig('sns_heatmap_cmap.jpg', bbox_inches='tight')
```

![](http://upload-images.jianshu.io/upload_images/5462537-3334083181ad8022.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




### 2.2 center

* center:将数据设置为图例中的均值数据，即图例中心的数据值；通过设置center值，可以调整生成的图像颜色的整体深浅；设置center数据时，如果有数据溢出，则手动设置的vmax、vmin会自动改变


```python
f, (ax1,ax2) = plt.subplots(figsize = (10, 8),nrows=2)

cmap = sns.cubehelix_palette(start = 1.5, rot = 3, gamma=0.8, as_cmap = True)

sns.heatmap(pt, linewidths = 0.05, ax = ax1, vmax=15000, vmin=0, cmap=cmap, center=None )
# center为None时，由于最小值为0，最大值为15000，相当于center值为vamx和vmin的均值，即7500
ax1.set_title('center=None')
ax1.set_xlabel('')
ax1.set_xticklabels([]) #设置x轴图例为空值
ax1.set_ylabel('kind')

sns.heatmap(pt, linewidths = 0.05, ax = ax2, vmax=15000, vmin=0, cmap=cmap, center=3000 )
# 由于均值为2000，当center设置为3000时，大部分数据会比7500大，所以center=3000时，生成的图片颜色要深
# 设置center数据时，如果有数据溢出，则手动设置的vmax或vmin会自动改变
ax2.set_title('center=3000')
ax2.set_xlabel('region')
ax2.set_ylabel('kind')

f.savefig('sns_heatmap_center.jpg', bbox_inches='tight')
```

![](http://upload-images.jianshu.io/upload_images/5462537-b6777227b82abe74.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




### 2.3 robust


```python
f, (ax1,ax2) = plt.subplots(figsize = (10, 8),nrows=2)

cmap = sns.cubehelix_palette(start = 1.5, rot = 3, gamma=0.8, as_cmap = True)

sns.heatmap(pt, linewidths = 0.05, ax = ax1, cmap=cmap, center=None, robust=False )
# robust默认为False
ax1.set_title('robust=False')
ax1.set_xlabel('')
ax1.set_xticklabels([]) #设置x轴图例为空值
ax1.set_ylabel('kind')

sns.heatmap(pt, linewidths = 0.05, ax = ax2, cmap=cmap, center=None, robust=True )
# If True and vmin or vmax are absent, the colormap range is computed with robust quantiles instead of the extreme values.
ax2.set_title('robust=True')
ax2.set_xlabel('region')
ax2.set_ylabel('kind')

f.savefig('sns_heatmap_robust.jpg', bbox_inches='tight')
```


![](http://upload-images.jianshu.io/upload_images/5462537-e74ba2405fc39077.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### 2.4 mask


```python
f, (ax1,ax2) = plt.subplots(figsize = (10, 8),nrows=2)

cmap = sns.cubehelix_palette(start = 1.5, rot = 3, gamma=0.8, as_cmap = True)

p1 = sns.heatmap(pt, linewidths = 0.05,ax=ax1, vmax=15000, vmin=0, cmap=cmap, center=None, robust=False, mask=None )
# robust默认为False
ax1.set_title('mask=None')
ax1.set_xlabel('')
ax1.set_xticklabels([]) #设置x轴图例为空值
ax1.set_ylabel('kind')

p2 = sns.heatmap(pt, linewidths = 0.05, ax=ax2, vmax=15000, vmin=0, cmap=cmap, center=None, robust=False, annot=False,mask=pt<10000 )
# mask: boolean array or DataFrame

ax2.set_title('mask: boolean DataFrame')
ax2.set_xlabel('region')
ax2.set_ylabel('kind')

f.savefig('sns_heatmap_mask.jpg', bbox_inches='tight')
```


![](http://upload-images.jianshu.io/upload_images/5462537-3ba48f526794a9a3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 2.5 xticklabels, yticklabels

* xticklabels: 如果是True，则绘制dataframe的列名。如果是False，则不绘制列名。如果是列表，则绘制列表中的内容作为xticklabels。 如果是整数n，则绘制列名，但每个n绘制一个label。  默认为True。
* yticklabels: 如果是True，则绘制dataframe的行名。如果是False，则不绘制行名。如果是列表，则绘制列表中的内容作为yticklabels。 如果是整数n，则绘制列名，但每个n绘制一个label。  默认为True。默认为True。


```python
f, (ax1,ax2) = plt.subplots(figsize = (10, 8),nrows=2)

cmap = sns.cubehelix_palette(start = 1.5, rot = 3, gamma=0.8, as_cmap = True)

p1 = sns.heatmap(pt, linewidths = 0.05,ax=ax1, vmax=15000, vmin=0, cmap=cmap, center=None, robust=False, mask=None, xticklabels=False )
# robust默认为False
ax1.set_title('xticklabels=None')
ax1.set_xlabel('')
# ax1.set_xticklabels([]) #设置x轴图例为空值
ax1.set_ylabel('kind')

p2 = sns.heatmap(pt, linewidths = 0.05, ax=ax2, vmax=15000, vmin=0, cmap=cmap, center=None, robust=False, annot=False,mask=None,xticklabels=3, yticklabels=list(range(20)) )
# mask: boolean array or DataFrame

ax2.set_title('xticklabels=3, yticklabels is a list')
ax2.set_xlabel('region')
ax2.set_ylabel('kind')

f.savefig('sns_heatmap_xyticklabels.jpg', bbox_inches='tight')
```


![](http://upload-images.jianshu.io/upload_images/5462537-ae65b945535a225f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





### 2.6 annot

* annotate的缩写，annot默认为False，当annot为True时，在heatmap中每个方格写入数据
* annot_kws，当annot为True时，可设置各个参数，包括大小，颜色，加粗，斜体字等


```python
np.random.seed(0)
x = np.random.randn(10, 10)
f, (ax1, ax2) = plt.subplots(figsize=(8,8),nrows=2)

sns.heatmap(x, annot=True, ax=ax1)
sns.heatmap(x, annot=True, ax=ax2, annot_kws={'size':9,'weight':'bold', 'color':'blue'})
# Keyword arguments for ax.text when annot is True.
# http://stackoverflow.com/questions/35024475/seaborn-heatmap-key-words

f.savefig('sns_heatmap_annot.jpg')
```


![](http://upload-images.jianshu.io/upload_images/5462537-a44b5c11cb2e7346.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**关于annot_kws的设置，还有很多值得研究的地方，ax.text有很多属性，有兴趣的可以去研究下**；

ax.text可参考官方文档：http://matplotlib.org/api/text_api.html#matplotlib.text.Text

### 2.7 fmt

* fmt，格式设置


```python
np.random.seed(0)
x = np.random.randn(10, 10)
f, (ax1, ax2) = plt.subplots(figsize=(8,8),nrows=2)

sns.heatmap(x, annot=True, ax=ax1)
sns.heatmap(x, annot=True, fmt='.1f', ax=ax2)

f.savefig('sns_heatmap_fmt.jpg')
```


![](http://upload-images.jianshu.io/upload_images/5462537-dffe62c06957d9e4.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 3 案例应用：突出显示某些数据

### 3.1 method 1：利用mask来实现


```python
f,ax=plt.subplots(figsize=(10,5))

x = np.random.randn(10, 10)
sns.heatmap(x, annot=True, ax=ax)
sns.heatmap(x, mask=x < 1, cbar=False, ax=ax,
            annot=True, annot_kws={"weight": "bold"})

f.savefig('sns_heatmap_eg1.jpg')
```


![](http://upload-images.jianshu.io/upload_images/5462537-181f1c2888bc6dd9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 3.2 method 2：利用ax.texts来实现


```python
f,ax=plt.subplots(figsize=(10,5))

flights = sns.load_dataset("flights")
flights = flights.pivot("month", "year", "passengers")
pic = sns.heatmap(flights, annot=True, fmt="d", ax=ax)

for text in pic.texts:
    text.set_size(8)
    if text.get_text() == '118':
        text.set_size(12)
        text.set_weight('bold')
        text.set_style('italic')

f.savefig('sns_heatmap_eg2.jpg')
```


![](http://upload-images.jianshu.io/upload_images/5462537-2d29268d3ce92f93.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



> 你可能会发现本文中seaborn的heatmap中还有些参数没有进行介绍，介于篇幅，这里就不在啰嗦了，建议各位小伙伴自己可以研究下其他参数如何使用。












<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="200">
</div>
