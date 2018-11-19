---
layout: posts
title: "Python：一篇文章掌握Numpy的基本用法"
teaser:
date: 2017-06-18
header:
   image_fullwidth: "image-article-head.jpg"
categories:
   - Numpy
tags:
   - Numpy
   - recommend
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---

{% include alert info='Python：一篇文章掌握Numpy的基本用法' %}


<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>


## 前言

Numpy是一个开源的Python科学计算库，它是python科学计算库的基础库，许多其他著名的科学计算库如Pandas，Scikit-learn等都要用到Numpy库的一些功能。

本文主要内容如下：

1. Numpy数组对象
2. 创建ndarray数组
3. Numpy的数值类型
4. ndarray数组的属性
5. ndarray数组的切片和索引
6. 处理数组形状
7. 数组的类型转换
8. numpy常用统计函数
9. 数组的广播


## 1 Numpy数组对象

Numpy中的多维数组称为ndarray，这是Numpy中最常见的数组对象。ndarray对象通常包含两个部分：

* ndarray数据本身
* 描述数据的元数据

Numpy数组的优势

* Numpy数组通常是由相同种类的元素组成的，即数组中的数据项的类型一致。这样有一个好处，由于知道数组元素的类型相同，所以能快速确定存储数据所需空间的大小。
* Numpy数组能够运用向量化运算来处理整个数组，速度较快；而Python的列表则通常需要借助循环语句遍历列表，运行效率相对来说要差。
* Numpy使用了优化过的C API，运算速度较快

**关于向量化和标量化运算**，对比下面的参考例子就可以看出差异

* 使用python的list进行循环遍历运算


```python
def pySum():
    a = list(range(10000))
    b = list(range(10000))
    c = []
    for i in range(len(a)):
        c.append(a[i]**2 + b[i]**2)

    return c

```


```python
%timeit pySum()
```

    10 loops, best of 3: 49.4 ms per loop
* 使用numpy进行向量化运算


```python
import numpy as np
def npSum():
    a = np.arange(10000)
    b = np.arange(10000)
    c = a**2 + b**2
    return c
```


```python
%timeit npSum()
```

    The slowest run took 262.56 times longer than the fastest. This could mean that an intermediate result is being cached.
    1000 loops, best of 3: 128 µs per loop

从上面的运行结果可以看出，**numpy的向量化运算的效率要远远高于python的循环遍历运算（效率相差好几百倍）**。
（1ms=1000µs）



## 2 创建ndarray数组

首先需要导入numpy库，在导入numpy库时通常使用“np”作为简写，这也是Numpy官方倡导的写法。

当然，你也可以选择其他简写的方式或者直接写numpy，但还是建议用“np”，这样你的程序能和大都数人的程序保持一致。


```python
import numpy as np
```

创建ndarray数组的方式有很多种，这里介绍我使用的较多的几种：

**Method 1: 基于list或tuple**


```python
# 一维数组

# 基于list
arr1 = np.array([1,2,3,4])
print(arr1)

# 基于tuple
arr_tuple = np.array((1,2,3,4))
print(arr_tuple)

# 二维数组 (2*3)
arr2 = np.array([[1,2,4], [3,4,5]])
arr2
```

    [1 2 3 4]
    [1 2 3 4]
    array([[1, 2, 4],
           [3, 4, 5]])


请注意：
* 一维数组用print输出的时候为 [1 2 3 4]，跟python的列表是有些差异的，没有“**,**”
* 在创建二维数组时，在每个子list外面还有一个"[]"，形式为“**[**[list1], [list2]**]**”

**Method 2: 基于np.arange**

```python
# 一维数组
arr1 = np.arange(5)
print(arr1)

# 二维数组
arr2 = np.array([np.arange(3), np.arange(3)])
arr2
```

    [0 1 2 3 4]
    array([[0, 1, 2],
           [0, 1, 2]])


**Method 3: 基于arange以及reshape创建多维数组**


```python
# 创建三维数组
arr = np.arange(24).reshape(2,3,4)
arr
```




    array([[[ 0,  1,  2,  3],
            [ 4,  5,  6,  7],
            [ 8,  9, 10, 11]],

           [[12, 13, 14, 15],
            [16, 17, 18, 19],
            [20, 21, 22, 23]]])



* 请注意：arange的长度与ndarray的维度的乘积要相等，即 24 = 2X3X4

* **用numpy.random创建数组的方法，可以参考下面的文章**

  **为什么你用不好Numpy的random函数？**


* 其他创建ndarray的方法，各位小伙伴们自己可以研究下。


## 3 Numpy的数值类型

Numpy的数值类型如下：

<div align="center">
    <img src="/images/posts/numpy-basic-in-one-acticle\2017026-01.png">
</div>

<!--
![](http://oqb5ftrdh.bkt.clouddn.com/17-6-18/99122977.jpg) -->

每一种数据类型都有相应的数据转换函数，参考示例如下：


```python
np.int8(12.334)
```


    12
```python
np.float64(12)
```


    12.0
```python
np.float(True)
```


    1.0
```python
bool(1)
```


    True



在创建ndarray数组时，可以指定数值类型：


```python
a = np.arange(5, dtype=float)
a
```


    array([ 0.,  1.,  2.,  3.,  4.])



* 请注意，复数不能转换成为整数类型或者浮点数，比如下面的代码会运行出错

  <!-- ![](http://oqb5ftrdh.bkt.clouddn.com/17-6-18/71721165.jpg) -->


  <div align="center">
      <img src="/images/posts/numpy-basic-in-one-acticle\2017026-02.png">
  </div>

```python
# float(42 + 1j)
```

## 4 ndarray数组的属性

* **dtype属性**，ndarray数组的数据类型，数据类型的种类，前面已描述。


```python
np.arange(4, dtype=float)
```


    array([ 0.,  1.,  2.,  3.])
```python
# 'D'表示复数类型
np.arange(4, dtype='D')
```


    array([ 0.+0.j,  1.+0.j,  2.+0.j,  3.+0.j])
```python
np.array([1.22,3.45,6.779], dtype='int8')
```


    array([1, 3, 6], dtype=int8)



* **ndim属性**，数组维度的数量


```python
a = np.array([[1,2,3], [7,8,9]])
a.ndim
```


    2



* **shape属性**，数组对象的尺度，对于矩阵，即n行m列,shape是一个元组（tuple）


```python
a.shape
```


    (2, 3)



* **size属性**用来保存元素的数量，相当于shape中nXm的值


```python
a.size
```


    6



* **itemsize**属性返回数组中各个元素所占用的字节数大小。


```python
a.itemsize
```


    4



* **nbytes属性**，如果想知道整个数组所需的字节数量，可以使用nbytes属性。其值等于数组的size属性值乘以itemsize属性值。


```python
a.nbytes
```


    24
```python
a.size*a.itemsize
```


    24



* **T属性**，数组转置


```python
b = np.arange(24).reshape(4,6)
b
```


    array([[ 0,  1,  2,  3,  4,  5],
           [ 6,  7,  8,  9, 10, 11],
           [12, 13, 14, 15, 16, 17],
           [18, 19, 20, 21, 22, 23]])




```python
b.T
```


    array([[ 0,  6, 12, 18],
           [ 1,  7, 13, 19],
           [ 2,  8, 14, 20],
           [ 3,  9, 15, 21],
           [ 4, 10, 16, 22],
           [ 5, 11, 17, 23]])



* **复数的实部和虚部属性，real和imag属性**


```python
d = np.array([1.2+2j, 2+3j])
d
```


    array([ 1.2+2.j,  2.0+3.j])



real属性返回数组的实部


```python
d.real
```


    array([ 1.2,  2. ])



imag属性返回数组的虚部


```python
d.imag
```


    array([ 2.,  3.])



* **flat属性**，返回一个numpy.flatiter对象，即可迭代的对象。


```python
e = np.arange(6).reshape(2,3)
e
```


    array([[0, 1, 2],
           [3, 4, 5]])


```python
f = e.flat
f
```


    <numpy.flatiter at 0x65eaca0>


```python
for item in f:
    print(item)
```

    0
    1
    2
    3
    4
    5


可通过位置进行索引，如下：


```python
f[2]
```


    2
```python
f[[1,4]]
```


    array([1, 4])



也可以进行赋值


```python
e.flat=7
e
```


    array([[7, 7, 7],
           [7, 7, 7]])


```python
e.flat[[1,4]]=1
e
```


    array([[7, 1, 7],
           [7, 1, 7]])



**下图是对ndarray各种属性的一个小结**

<div align="center">
    <img src="/images/posts/numpy-basic-in-one-acticle\2017026-03.jpeg">
</div>

<!-- ![](http://oqb5ftrdh.bkt.clouddn.com/17-6-18/7858951.jpg) -->



## 5 ndarray数组的切片和索引

* **一维数组**

一维数组的切片和索引与python的list索引类似。


```python
a =  np.arange(7)
a
```


    array([0, 1, 2, 3, 4, 5, 6])
```python
a[1:4]
```


    array([1, 2, 3])


```python
# 每间隔2个取一个数
a[ : 6: 2]
```


    array([0, 2, 4])



* **二维数组的切片和索引**，如下所示：

  <!-- ![](http://oqb5ftrdh.bkt.clouddn.com/17-6-18/59493281.jpg) -->

  <div align="center">
      <img src="/images/posts/numpy-basic-in-one-acticle\2017026-04.png">
  </div>


插播一条硬广：技术文章转发太多。本文涉及的代码量比较多，如需要查看源代码，请在微信公众号“Python数据之道”（ID：PyDataRoad）后台回复关键字“2017026”。



## 6 处理数组形状

### 6.1 形状转换

* **reshape()和resize()**


```python
b.reshape(4,3)
```


    array([[ 0,  1,  2],
           [ 3,  4,  5],
           [ 6,  7,  8],
           [ 9, 10, 11]])


```python
b
```


    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])




```python
b.resize(4,3)
b
```


    array([[ 0,  1,  2],
           [ 3,  4,  5],
           [ 6,  7,  8],
           [ 9, 10, 11]])



**函数resize（）的作用跟reshape（）类似，但是会改变所作用的数组，相当于有inplace=True的效果**

* **ravel()和flatten()**，将多维数组转换成一维数组，如下：


```python
b.ravel()
```


    array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11])


```python
b.flatten()
```


    array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11])


```python
b
```


    array([[ 0,  1,  2],
           [ 3,  4,  5],
           [ 6,  7,  8],
           [ 9, 10, 11]])



**两者的区别在于返回拷贝（copy）还是返回视图（view）**，flatten()返回一份拷贝，需要分配新的内存空间，对拷贝所做的修改不会影响原始矩阵，而ravel()返回的是视图（view），会影响原始矩阵。

参考如下代码：

<!-- ![](http://oqb5ftrdh.bkt.clouddn.com/17-6-18/74821824.jpg) -->


<div align="center">
    <img src="/images/posts/numpy-basic-in-one-acticle\2017026-05.png">
</div>




* **用tuple指定数组的形状**，如下：


```python
b.shape=(2,6)
b
```


    array([[ 0,  1, 20,  3,  4,  5],
           [ 6,  7,  8,  9, 10, 11]])



* **转置**

前面描述了数组转置的属性（T），也可以通过transpose()函数来实现


```python
b.transpose()
```


    array([[ 0,  6],
           [ 1,  7],
           [20,  8],
           [ 3,  9],
           [ 4, 10],
           [ 5, 11]])



### 6.2 堆叠数组


```python
b
```


    array([[ 0,  1, 20,  3,  4,  5],
           [ 6,  7,  8,  9, 10, 11]])


```python
c = b*2
c
```


    array([[ 0,  2, 40,  6,  8, 10],
           [12, 14, 16, 18, 20, 22]])



* **水平叠加**

hstack()


```python
np.hstack((b,c))
```


    array([[ 0,  1, 20,  3,  4,  5,  0,  2, 40,  6,  8, 10],
           [ 6,  7,  8,  9, 10, 11, 12, 14, 16, 18, 20, 22]])



column_stack()函数以列方式对数组进行叠加，功能类似hstack（）


```python
np.column_stack((b,c))
```


    array([[ 0,  1, 20,  3,  4,  5,  0,  2, 40,  6,  8, 10],
           [ 6,  7,  8,  9, 10, 11, 12, 14, 16, 18, 20, 22]])



* **垂直叠加**

vstack()


```python
np.vstack((b,c))
```


    array([[ 0,  1, 20,  3,  4,  5],
           [ 6,  7,  8,  9, 10, 11],
           [ 0,  2, 40,  6,  8, 10],
           [12, 14, 16, 18, 20, 22]])



row_stack()函数以行方式对数组进行叠加，功能类似vstack（）


```python
np.row_stack((b,c))
```


    array([[ 0,  1, 20,  3,  4,  5],
           [ 6,  7,  8,  9, 10, 11],
           [ 0,  2, 40,  6,  8, 10],
           [12, 14, 16, 18, 20, 22]])



* **concatenate()方法，通过设置axis的值来设置叠加方向**

axis=1时，沿水平方向叠加

axis=0时，沿垂直方向叠加



```python
np.concatenate((b,c),axis=1)
```


    array([[ 0,  1, 20,  3,  4,  5,  0,  2, 40,  6,  8, 10],
           [ 6,  7,  8,  9, 10, 11, 12, 14, 16, 18, 20, 22]])




```python
np.concatenate((b,c),axis=0)
```


    array([[ 0,  1, 20,  3,  4,  5],
           [ 6,  7,  8,  9, 10, 11],
           [ 0,  2, 40,  6,  8, 10],
           [12, 14, 16, 18, 20, 22]])



由于针对数组的轴为0或1的方向经常会混淆，通过示意图，或许可以更好的理解。

关于数组的轴方向示意图，以及叠加的示意图，如下：

<div align="center">
    <img src="/images/posts/numpy-basic-in-one-acticle\2017026-06.jpg">
    <img src="/images/posts/numpy-basic-in-one-acticle\2017026-07.jpg">
</div>

<!-- ![](http://oqb5ftrdh.bkt.clouddn.com/17-6-18/77741150.jpg)

![](http://oqb5ftrdh.bkt.clouddn.com/17-6-18/63355812.jpg) -->



**深度叠加**

这个有点烧脑，举个例子如下，自己可以体会下：


```python
arr_dstack = np.dstack((b,c))
print(arr_dstack.shape)
arr_dstack
```

    (2, 6, 2)

    array([[[ 0,  0],
            [ 1,  2],
            [20, 40],
            [ 3,  6],
            [ 4,  8],
            [ 5, 10]],

           [[ 6, 12],
            [ 7, 14],
            [ 8, 16],
            [ 9, 18],
            [10, 20],
            [11, 22]]])



叠加前，b和c均是shape为（2,6）的二维数组，叠加后，arr_dstack是shape为（2,6,2）的三维数组。

**深度叠加的示意图如下：**

<!-- ![](http://oqb5ftrdh.bkt.clouddn.com/17-6-18/49051165.jpg) -->


<div align="center">
    <img src="/images/posts/numpy-basic-in-one-acticle/2017026-08.jpg">    
</div>

### 6.3 数组的拆分

跟数组的叠加类似，数组的拆分可以分为横向拆分、纵向拆分以及深度拆分。

涉及的函数为 hsplit()、vsplit()、dsplit() 以及split()


```python
b
```


    array([[ 0,  1, 20,  3,  4,  5],
           [ 6,  7,  8,  9, 10, 11]])



* **沿横向轴拆分（axis=1）**


```python
np.hsplit(b, 2)
```


    [array([[ 0,  1, 20],
            [ 6,  7,  8]]), array([[ 3,  4,  5],
            [ 9, 10, 11]])]




```python
np.split(b,2, axis=1)
```


    [array([[ 0,  1, 20],
            [ 6,  7,  8]]), array([[ 3,  4,  5],
            [ 9, 10, 11]])]




* **沿纵向轴拆分（axis=0）**


```python
np.vsplit(b, 2)
```


    [array([[ 0,  1, 20,  3,  4,  5]]), array([[ 6,  7,  8,  9, 10, 11]])]


```python
np.split(b,2,axis=0)
```


    [array([[ 0,  1, 20,  3,  4,  5]]), array([[ 6,  7,  8,  9, 10, 11]])]



* **深度拆分**


```python
arr_dstack
```


    array([[[ 0,  0],
            [ 1,  2],
            [20, 40],
            [ 3,  6],
            [ 4,  8],
            [ 5, 10]],

           [[ 6, 12],
            [ 7, 14],
            [ 8, 16],
            [ 9, 18],
            [10, 20],
            [11, 22]]])




```python
np.dsplit(arr_dstack,2)
```


    [array([[[ 0],
             [ 1],
             [20],
             [ 3],
             [ 4],
             [ 5]],

            [[ 6],
             [ 7],
             [ 8],
             [ 9],
             [10],
             [11]]]), array([[[ 0],
             [ 2],
             [40],
             [ 6],
             [ 8],
             [10]],

            [[12],
             [14],
             [16],
             [18],
             [20],
             [22]]])]



拆分的结果是原来的三维数组拆分成为两个二维数组。

这个烧脑的拆分过程可以自行分析下~~

## 7 数组的类型转换

* **数组转换成list，使用tolist()**


```python
b
```


    array([[ 0,  1, 20,  3,  4,  5],
           [ 6,  7,  8,  9, 10, 11]])




```python
b.tolist()
```


    [[0, 1, 20, 3, 4, 5], [6, 7, 8, 9, 10, 11]]



* **转换成指定类型，astype()函数**


```python
b.astype(float)
```


    array([[  0.,   1.,  20.,   3.,   4.,   5.],
           [  6.,   7.,   8.,   9.,  10.,  11.]])






## 8 numpy常用统计函数

常用的函数如下：

**请注意函数在使用时需要指定axis轴的方向**，若不指定，默认统计整个数组。

* np.sum()，返回求和
* np.mean()，返回均值
* np.max()，返回最大值
* np.min()，返回最小值
* np.ptp()，数组沿指定轴返回最大值减去最小值，即（max-min）
* np.std()，返回标准偏差（standard deviation）
* np.var()，返回方差（variance）
* np.cumsum()，返回累加值
* np.cumprod()，返回累乘积值


```python
b
```


    array([[ 0,  1, 20,  3,  4,  5],
           [ 6,  7,  8,  9, 10, 11]])


```python
np.max(b)
```


    20


```python
# 沿axis=1轴方向统计
np.max(b,axis=1)
```


    array([20, 11])


```python
# 沿axis=0轴方向统计
np.max(b,axis=0)
```


    array([ 6,  7, 20,  9, 10, 11])


```python
np.min(b)
```


    0



* **np.ptp()**，返回整个数组的最大值减去最小值，如下：


```python
np.ptp(b)
```


    20


```python
# 沿axis=0轴方向
np.ptp(b, axis=0)
```


    array([ 6,  6, 12,  6,  6,  6])


```python
# 沿axis=1轴方向
np.ptp(b, axis=1)
```


    array([20,  5])



* **np.cumsum()**，沿指定轴方向进行累加


```python
b.resize(4,3)
b
```


    array([[ 0,  1, 20],
           [ 3,  4,  5],
           [ 6,  7,  8],
           [ 9, 10, 11]])




```python
np.cumsum(b, axis=1)
```


    array([[ 0,  1, 21],
           [ 3,  7, 12],
           [ 6, 13, 21],
           [ 9, 19, 30]], dtype=int32)




```python
np.cumsum(b, axis=0)
```


    array([[ 0,  1, 20],
           [ 3,  5, 25],
           [ 9, 12, 33],
           [18, 22, 44]], dtype=int32)



* **np.cumprod()**，沿指定轴方向进行累乘积 （Return the cumulative product of the elements along the given axis）


```python
np.cumprod(b,axis=1)
```


    array([[  0,   0,   0],
           [  3,  12,  60],
           [  6,  42, 336],
           [  9,  90, 990]], dtype=int32)




```python
np.cumprod(b,axis=0)
```


    array([[   0,    1,   20],
           [   0,    4,  100],
           [   0,   28,  800],
           [   0,  280, 8800]], dtype=int32)



## 9 数组的广播

当数组跟一个标量进行数学运算时，标量需要根据数组的形状进行扩展，然后执行运算。

这个扩展的过程称为“广播（broadcasting）”


```python
b
```


    array([[ 0,  1, 20],
           [ 3,  4,  5],
           [ 6,  7,  8],
           [ 9, 10, 11]])




```python
d = b + 2
d
```


    array([[ 2,  3, 22],
           [ 5,  6,  7],
           [ 8,  9, 10],
           [11, 12, 13]])



## 写在最后

numpy涵盖的内容其实是非常丰富的，本文仅仅介绍了numpy一些常用的基本功能，算是对numpy的一个入门级的简单的较为全面的描述。



numpy官方的《Numpy Reference》文档，光页面数量就有1500+页，如想要系统的学习numpy，建议仔细阅读官方的参考文档，可在其官方网站进行查阅。当然，资料都是英文版的，可能看起来难度稍微大点，看习惯了就好。



本文涉及的代码量比较多，如需要查看**源代码**，请在微信公众号“Python数据之道”（ID：PyDataRoad）后台回复关键字“2017026”。


<div align="center"><img src="/images/qrcode.jpg" width="200"/></div>
