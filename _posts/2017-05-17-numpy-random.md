---
layout: posts
title: "为什么你用不好Numpy的random函数？"
teaser:
date: 2017-05-17
header:
   image_fullwidth: "image-head.jpg"
categories:
   - Numpy
tags:
   - Numpy
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---

**为什么你用不好Numpy的random函数？**


<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>




在python数据分析的学习和应用过程中，经常需要用到numpy的随机函数，由于随机函数random的功能比较多，经常会混淆或记不住，下面我们一起来汇总学习下。


```python
import numpy as np
```

## 1 numpy.random.rand()


numpy.random.rand(d0,d1,...,dn)
* rand函数根据给定维度生成[0,1)之间的数据，包含0，不包含1
* dn表格每个维度
* 返回值为指定维度的array


```python
np.random.rand(4,2)
```




    array([[ 0.02173903,  0.44376568],
           [ 0.25309942,  0.85259262],
           [ 0.56465709,  0.95135013],
           [ 0.14145746,  0.55389458]])




```python
np.random.rand(4,3,2) # shape: 4*3*2
```




    array([[[ 0.08256277,  0.11408276],
            [ 0.11182496,  0.51452019],
            [ 0.09731856,  0.18279204]],

           [[ 0.74637005,  0.76065562],
            [ 0.32060311,  0.69410458],
            [ 0.28890543,  0.68532579]],

           [[ 0.72110169,  0.52517524],
            [ 0.32876607,  0.66632414],
            [ 0.45762399,  0.49176764]],

           [[ 0.73886671,  0.81877121],
            [ 0.03984658,  0.99454548],
            [ 0.18205926,  0.99637823]]])



## 2 numpy.random.randn()

numpy.random.randn(d0,d1,...,dn)
* randn函数返回一个或一组样本，具有标准正态分布。
* dn表格每个维度
* 返回值为指定维度的array


```python
np.random.randn() # 当没有参数时，返回单个数据
```




    -1.1241580894939212




```python
np.random.randn(2,4)
```




    array([[ 0.27795239, -2.57882503,  0.3817649 ,  1.42367345],
           [-1.16724625, -0.22408299,  0.63006614, -0.41714538]])




```python
np.random.randn(4,3,2)
```




    array([[[ 1.27820764,  0.92479163],
            [-0.15151257,  1.3428253 ],
            [-1.30948998,  0.15493686]],

           [[-1.49645411, -0.27724089],
            [ 0.71590275,  0.81377671],
            [-0.71833341,  1.61637676]],

           [[ 0.52486563, -1.7345101 ],
            [ 1.24456943, -0.10902915],
            [ 1.27292735, -0.00926068]],

           [[ 0.88303   ,  0.46116413],
            [ 0.13305507,  2.44968809],
            [-0.73132153, -0.88586716]]])



标准正态分布介绍
* 标准正态分布---standard normal distribution
* 标准正态分布又称为u分布，是以0为均值、以1为标准差的正态分布，记为N（0，1）。


## 3 numpy.random.randint()

### 3.1 numpy.random.randint()
numpy.random.randint(low, high=None, size=None, dtype='l')
* 返回随机整数，范围区间为[low,high），包含low，不包含high
* 参数：low为最小值，high为最大值，size为数组维度大小，dtype为数据类型，默认的数据类型是np.int
* high没有填写时，默认生成随机数的范围是[0，low)


```python
np.random.randint(1,size=5) # 返回[0,1)之间的整数，所以只有0
```




    array([0, 0, 0, 0, 0])




```python
np.random.randint(1,5) # 返回1个[1,5)时间的随机整数
```




    4




```python
np.random.randint(-5,5,size=(2,2))
```




    array([[ 2, -1],
           [ 2,  0]])



### 3.2 numpy.random.random_integers

numpy.random.random_integers(low, high=None, size=None)
* 返回随机整数，范围区间为[low,high]，包含low和high
* 参数：low为最小值，high为最大值，size为数组维度大小
* high没有填写时，默认生成随机数的范围是[1，low]

该函数在最新的numpy版本中已被替代，建议使用randint函数


```python
np.random.random_integers(1,size=5)
```

    array([1, 1, 1, 1, 1])



## 4 生成[0,1)之间的浮点数

* numpy.random.random_sample(size=None)
* numpy.random.random(size=None)
* numpy.random.ranf(size=None)
* numpy.random.sample(size=None)


```python
print('-----------random_sample--------------')
print(np.random.random_sample(size=(2,2)))
print('-----------random--------------')
print(np.random.random(size=(2,2)))
print('-----------ranf--------------')
print(np.random.ranf(size=(2,2)))
print('-----------sample--------------')
print(np.random.sample(size=(2,2)))
```

    -----------random_sample--------------
    [[ 0.34966859  0.85655008]
     [ 0.16045328  0.87908218]]
    -----------random--------------
    [[ 0.25303772  0.45417512]
     [ 0.76053763  0.12454433]]
    -----------ranf--------------
    [[ 0.0379055   0.51288667]
     [ 0.71819639  0.97292903]]
    -----------sample--------------
    [[ 0.59942807  0.80211491]
     [ 0.36233939  0.12607092]]


## 5 numpy.random.choice()

numpy.random.choice(a, size=None, replace=True, p=None)
* 从给定的一维数组中生成随机数
* 参数： a为一维数组类似数据或整数；size为数组维度；p为数组中的数据出现的概率
* a为整数时，对应的一维数组为np.arange(a)


```python
np.random.choice(5,3)
```

    array([4, 1, 4])




```python
np.random.choice(5, 3, replace=False)
# 当replace为False时，生成的随机数不能有重复的数值
```


    array([0, 3, 1])




```python
np.random.choice(5,size=(3,2))
```


    array([[1, 0],
           [4, 2],
           [3, 3]])




```python
demo_list = ['lenovo', 'sansumg','moto','xiaomi', 'iphone']
np.random.choice(demo_list,size=(3,3))
```




    array([['moto', 'iphone', 'xiaomi'],
           ['lenovo', 'xiaomi', 'xiaomi'],
           ['xiaomi', 'lenovo', 'iphone']],
          dtype='<U7')



* 参数p的长度与参数a的长度需要一致；
* 参数p为概率，p里的数据之和应为1


```python
demo_list = ['lenovo', 'sansumg','moto','xiaomi', 'iphone']
np.random.choice(demo_list,size=(3,3), p=[0.1,0.6,0.1,0.1,0.1])
```




    array([['sansumg', 'sansumg', 'sansumg'],
           ['sansumg', 'sansumg', 'sansumg'],
           ['sansumg', 'xiaomi', 'iphone']],
          dtype='<U7')



## 6 numpy.random.seed()

* np.random.seed()的作用：使得随机数据可预测。
* 当我们设置相同的seed，每次生成的随机数相同。如果不设置seed，则每次会生成不同的随机数


```python
np.random.seed(0)
np.random.rand(5)
```




    array([ 0.5488135 ,  0.71518937,  0.60276338,  0.54488318,  0.4236548 ])




```python
np.random.seed(1676)
np.random.rand(5)
```




    array([ 0.39983389,  0.29426895,  0.89541728,  0.71807369,  0.3531823 ])




```python
np.random.seed(1676)
np.random.rand(5)
```

    array([ 0.39983389,  0.29426895,  0.89541728,  0.71807369,  0.3531823 ])    






{% include alert text='对我的文章感兴趣的朋友，可以关注我的微信公众号"Python数据之道"，接收我的更新通知。' %}

<div align="center"><img src="/images/qrcode.jpg" width="200"/></div>
