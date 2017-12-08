---
layout: posts
title: "03-第一个Python程序"
subheadline: "Python基础"
teaser:
date: 2017-12-03
header:
   image_fullwidth: "image-head.jpg"
categories:
   - tutorials
tags:
   - tutorials-python-basic
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---

本文来源于：
<a href="http://liyangbit.com" target="blank">liyangbit.com</a>

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

**第一个Python程序**

上一节中我们介绍了 Python程序的安装，这里我推荐的是第二种安装方法。详细请阅读下面链接：

<a href="http://liyangbit.com/tutorials/python-install/" target="blank">Python程序安装</a>

在安装好Python程序后，我们在Anaconda中找到 iPython 来编写代码并输出。

在编写代码前，先介绍下运行环境：
* windows 7
* Python 3.5

后续的教程，如果没有特别说明，基本上也是在Python 3.x （当前用的是 3.5版） 下运行。


## 1 输出函数 print()

跟其他很多程序一样，我们第一个程序用Python来输出 “Hello, world”。

Python代码如下：

```python
print("hello, world")
```

输入结果如下：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-first-program-1.png">
</div>

**语法解释：**

python语言中，输出用 `print()` 函数来描述，括号内可以是字符串（比如 "hello, world"），也可以是数据的运算。

当内容是字符串时，可以用双引号，也可以用单引号。代码演示如下：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-first-program-2.png">
</div>

注意到，上图中输出 "1+2" 时，可以用 `print(1+2)` 或者 `1+2` ，并且数据运算时是不需要加引号的。

当然， `print(1+2)` 显示的结果为 "3"，有时候，你可能希望显示这样的结果 [1+2=3 ]，演示如下：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-first-program-3.png">
</div>

从上面可以看出，`print()` 可以接收多个参数，参数之间以逗号隔开，遇到逗号会输出一个空格。

还是以 "hello, world" 为例，可以再看下输出多个字符串的情形。


```python
print("hello,", "world", "welcome!")
```

演示如下：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-first-program-4.png">
</div>

## 2 输入函数 input()

上面提到的是输出函数，经常会遇到的一种情形是，需要用户输入相关信息，然后程序才继续运行，这种情况怎么办呢？

Python 提供了 `input()` 函数来让用户输入信息。

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-first-program-5.png">
</div>

在上面程序中，运行后需要用户输入用户名信息，程序才能继续运行，我们输入 "Lemon"（不含引号）来试试。

输入 “Lemon” 后， 我们再运行代码 `print(name)` ， 可以看到其输出结果， 演示如下：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-first-program-6.png">
</div>

上图中，有2个 "Lemon"（不含引号），其中第一个是用户输入的信息，第二个是输出的结果。

## 3 小练习

在 "iPython" 中输入如下代码，看看运行情况。

```python
import this
```

>Python程序是不是很简洁易懂啊，快来动手试试吧。

<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="200">
</div>
