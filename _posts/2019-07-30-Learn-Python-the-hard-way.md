---
layout: posts
title: "笨办法学Python，其实一点都不笨"
teaser:
date: 2019-07-30
header:
   image_fullwidth: "image-head.jpg"
categories:
   - ITBooks
tags:    
   - PythonBooks      
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---

{% include alert info='笨办法学Python，其实一点都不笨' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# 笨办法学Python，其实一点都不笨

经常有读者问我，想让我推荐一些关于 Python 学习的书籍，鉴于每个人的知识背景以及跟我交流的深浅程度不一样，可能每次推荐的书籍都会有些差异。

最近有些想法，打算陆陆续续介绍一些书籍，希望能给大家带来一些益处。

需要注意的是，我本身不会刻意对书籍的内容进行点评，尽量做到相对客观的对书籍进行介绍。

至于你是否需要去学习文章中介绍的书籍，这个还是需要结合自己的实际情况来做筛选。毕竟每个人的时间和精力都有限，市面上优秀的书籍数不胜数，我们挑选适合自己的就好。

今天我们来介绍的书籍是 《笨办法学 Python 3》，这本书的作者是 泽德 A. 肖（Zed A.Shaw）。

本书作者是《“笨办法”学Python》《“笨办法”学Ruby》《“笨办法”学C语言》等几本备受欢迎的图书的作者，他的书在全世界有数百万读者。他还是诸多开源项目的创建者，拥有近20年的编程和写作经验。

《笨办法学 Python 3》基于 Python 3.6，是一本从零基础入门开始介绍 Python 3 的书籍。本书介绍了一个非常有用的学习窍门，就是把编程书籍习题化，全书共 52 道习题。

习题的安排遵循了循序渐进的原则，每道题 2~5 页不等，内容相对通俗易懂，对于初学者而言，可以减少不少枯燥之处。

下面将就书中的部分内容进行描述，希望大家对本书有所了解。

## 习题 1：第一个程序

作者建议先编写一个完整的 Python 程序，即 `.py` 文件，然后在终端运行代码。

例如，文件 `ex1.py` 的代码如下

```python
print('Hello World!')
print('Hello again')
print('I like typing this.')
print('This is fun.')
print('Yay! Printing.')
print("I'd much rather you 'not'.")
print('I "said" do not touch this.')
```

在终端运行下面代码

```python
python ex1.py
```

我运行的效果如下：

![img01](/images/posts/013-learn-python-the-hard-way/1.png)

## 习题 4： 变量和命名

在这个习题中，提到了一些我们经常遇到的小问题。

比如：

> = （单等号）和 ==（双等号）有什么不同？

这个问题的答案，请自行查找（本书也进行了阐述）。

比如：

>书写 `x=100` 和 `x = 100` 有什么差异？

上面两种写法，本质上都是可以的，但操作符两边加上空格，会让代码可阅读性更好。

## 习题 6：字符串和文本

在 Python 3 中，字符串有几种常见的输入表达方法，本书着重介绍了一种，称为 "f-string"，其语法格式如下：

```python
f"some stuff here {avariable}"
```

在 ipython 中实际运行下面代码：

```python
num = 10

y = f"there are {num} apples."

print(y)
```

效果如下：

![img02](/images/posts/013-learn-python-the-hard-way/2.png)

同时，关于 字符串中 `.format()` 语法格式的使用，书中亦有描述。

## 总结

总体而言，本书 52 道习题，内容逐步深入，在最后几道习题，作者通过使用 flask 来建立一个微小型的网页，使本书内容延伸到 web 编程的范畴。

在本书的结尾部分，作者也给出了一些建议：

>你已经完成了这本书并且打算继续编程。也许这会成为你的职业，也许你只是作为业余爱好玩玩而已。无论如何，你都需要一些建议以确保你在正确的道路上继续前行，并且让这项新的爱好最大程序为你带来享受。
>…………
>**编程语言这东西并不重要，重要的是你用这些编程语言做的事情。**

更多关于本书的详情，可以点击下面的链接进行了解：

可以说，本书以一种较为新颖的方式，帮助广大读者完成了 Python 学习 从 0 到 0.5 的过程，如果需要继续深入了解 Python，可以配合其他书籍进一步学习。

最后，**「Python数据之道」** 将本书的内容概览进行了整理，以便更加清晰的了解本书的内容结构。

[点击获取本书高清 pdf 版的思维导图](http://liyangbit.com/pdf/)

![img03](/images/posts/013-learn-python-the-hard-way/3.png)

---

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataLab），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
