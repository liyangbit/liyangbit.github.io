---
layout: posts
title: "Python：Pandas的DataFrame如何按指定list排序"
teaser:
date: 2017-06-13
header:
   image_fullwidth: "image-head.jpg"
categories:
   - Pandas
tags:
   - Pandas
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

写这篇文章的起由是有一天微信上一位朋友问到一个问题，问题大体意思概述如下：

*现在有一个pandas的Series和一个python的list，想让Series按指定的list进行排序，如何实现？*

这个问题的需求用流程图描述如下：

![](http://upload-images.jianshu.io/upload_images/5462537-5e95a6a12fe295d4.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我思考了一下，这个问题解决的核心是引入pandas的数据类型“category”，从而进行排序。

在具体的分析过程中，先将pandas的Series转换成为DataFrame，然后设置数据类型，再进行排序。思路用流程图表示如下：

![](http://upload-images.jianshu.io/upload_images/5462537-6a7f5a455c50617b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 分析过程

* 引入pandas库


```python
import pandas as pd
```

* 构造Series数据


```python
s = pd.Series({'a':1,'b':2,'c':3})
s
```




    a    1
    b    2
    c    3
    dtype: int64




```python
s.index
```




    Index(['a', 'b', 'c'], dtype='object')



* 指定的list，后续按指定list的元素顺序进行排序


```python
list_custom = ['b', 'a', 'c']
list_custom
```




    ['b', 'a', 'c']



* 将Series转换成DataFrame
```python
df = pd.DataFrame(s)
df = df.reset_index()
df.columns = ['words', 'number']
df
```

![1.jpg](http://upload-images.jianshu.io/upload_images/5462537-5a2970f02ecc7577.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)







**设置成“category”数据类型**


```python
# 设置成“category”数据类型
df['words'] = df['words'].astype('category')
```


```python
# inplace = True，使 recorder_categories生效
df['words'].cat.reorder_categories(list_custom, inplace=True)

# inplace = True，使 df生效
df.sort_values('words', inplace=True)
df
```





![2.jpg](http://upload-images.jianshu.io/upload_images/5462537-f3484be33b6304f8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



## 指定list元素多的情况：

若指定的list所包含元素比Dataframe中需要排序的列的元素**多**，怎么办？

* reorder_catgories（）方法不能继续使用，因为该方法使用时要求新的categories和dataframe中的categories的元素个数和内容必须一致，只是顺序不同。
* 这种情况下，可以使用 set_categories()方法来实现。新的list可以比dataframe中元素多。


```python
list_custom_new = ['d', 'c', 'b','a','e']
dict_new = {'e':1, 'b':2, 'c':3}
df_new = pd.DataFrame(list(dict_new.items()), columns=['words', 'value'])
print(list_custom_new)
df_new.sort_values('words', inplace=True)
df_new
```

    ['d', 'c', 'b', 'a', 'e']






![3.jpg](http://upload-images.jianshu.io/upload_images/5462537-b833ac0395ebe313.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





```python
df_new['words'] = df_new['words'].astype('category')

# inplace = True，使 set_categories生效
df_new['words'].cat.set_categories(list_custom_new, inplace=True)

df_new.sort_values('words', ascending=True)
```





![4.jpg](http://upload-images.jianshu.io/upload_images/5462537-87752f4c7709a43a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



## 指定list元素少的情况：

若指定的list所包含元素比Dataframe中需要排序的列的元素**少**，怎么办？
* 这种情况下，set_categories()方法还是可以使用的，只是没有的元素会以NaN表示

注意下面的list中没有元素“b”


```python
list_custom_new = ['d', 'c','a','e']
dict_new = {'e':1, 'b':2, 'c':3}
df_new = pd.DataFrame(list(dict_new.items()), columns=['words', 'value'])
print(list_custom_new)
df_new.sort_values('words', inplace=True)
df_new
```

    ['d', 'c', 'a', 'e']






![5.jpg](http://upload-images.jianshu.io/upload_images/5462537-babac9470c82cec4.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





```python
df_new['words'] = df_new['words'].astype('category')

# inplace = True，使 set_categories生效
df_new['words'].cat.set_categories(list_custom_new, inplace=True)

df_new.sort_values('words', ascending=True)
```





![6.jpg](http://upload-images.jianshu.io/upload_images/5462537-ccbd273321362d96.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 总结

根据指定的list所包含元素比Dataframe中需要排序的列的元素的多或少，可以分为三种情况：

* 相等的情况下，可以使用 reorder_categories和 set_categories方法；
* list的元素比较多的情况下， 可以使用set_categories方法；
* list的元素比较少的情况下， 也可以使用set_categories方法，但list中没有的元素会在DataFrame中以NaN表示。

**源代码**

需要的童鞋可在微信公众号“Python数据之道”（ID：PyDataRoad）后台回复关键字获取，关键字如下：

“**2017-025**”（不含引号）


<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
