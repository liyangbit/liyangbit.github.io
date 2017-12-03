---
layout: posts
title: "Python中字典合并的实现方法"
teaser:
date: 2017-11-05
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PythonBasic
tags:
   - dictionary
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---




{% include alert info='Python中字典合并的实现方法' %}


<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>




## 背景


Python 中有时候需要将字典进行合并，今天我们来探讨下这个主题。

先来看看下面的运行结果


```python
x = {'a':1, 'b':2}
y = {'b':4, 'c':5}
z = x.update(y)
```

来看看 z 的返回结果：


```python
print(z)
```

    None


发现 z 返回的是None值，并不是我们想要的结果。

再来看一下此时 x 的返回结果，发现 x 已经是合并的结果。


```python
x
```




    {'a': 1, 'b': 4, 'c': 5}



那么有没有比较简单的实现方法呢。下面我们来探索一下：

## Methon1：适用于Python3.5及以上版本呢

在 Python3.5 以上版本（含3.5），可以通过下列方法实现


```python
z1 = {**x, **y}
z1
```




    {'a': 1, 'b': 4, 'c': 5}



## Method2：自定义函数

如果在 Python2 或 Python3.4 以及低于3.4的版本，要实现两个 dict 的合并，该怎样做呢？

先定义一个函数，然后用自定义函数来运行


```python
def merge(x,y):
    z = x.copy()
    z.update(y)
    return z
```


```python
z2 = merge(x,y)
z2
```




    {'a': 1, 'b': 4, 'c': 5}



Method2 在 Python3.5 以上版本也是可以运行的。

此外，请注意 x 中 'b'=2， y 中 'b'=4, 而运算结果中 'b'=4，是用 y 中 'b'的值来对字典进行更新。

## 多个 dict 进行合并

如果是多个 dictionary 需要进行合并呢？

同样可以通过自定义的形式来实现。


```python
def merge_dicts(*dict_args):
    result = {}
    for item in dict_args:
        result.update(item)
    return result
```


```python
x1 = {'a':1, 'b':2}
y1 = {'b':4, 'c':5}
x2 = {'d':8, 'e':10}
```


```python
z3 = merge_dicts(x1,y1,x2)
z3
```




    {'a': 1, 'b': 4, 'c': 5, 'd': 8, 'e': 10}



此方法在 Python2 和 Python3 下都是可以运行的


<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="200">
</div>
