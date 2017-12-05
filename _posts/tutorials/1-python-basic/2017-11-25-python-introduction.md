---
layout: posts
title: "01-Python语言的简单介绍"
subheadline: "Python基础"
teaser:
date: 2017-11-26
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

**Python基础（1）-Python语言的简单介绍**

本文来源于：
<a href="http://liyangbit.com" target="blank">liyangbit.com</a>

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>



## 1 起源

Python 的创立者，Guido van Rossum，荷兰人。1982 年，Guido 从阿姆斯特丹大学获得了数学和计算机硕士学位。然而，尽管他算得上是一位数学家，但他更加享受计算机带来的乐趣。用他的话说，尽管拥有数学和计算机双料资质，他总趋向于做计算机相关的工作，并热衷于做任何和编程相关的活儿。

Python的创始人吉多·范罗苏姆（Guido van Rossum），在1989年12月的圣诞节期间，为了打发时间，决定开发一种新的脚本解释程序，作为ABC语言的继承。1991年2月，第一个Python编译器诞生。现在，python以其优美，清晰，简单的特性在全世界广泛流行，成为最主流的编程语言之一。

Guido van Rossum 有一句著名的话就是 “**Life is short, you need Python**”，译为：“人生苦短，我用Python”。

截止至2017年11月，python在TIOBE流行编程语言排行榜，已经上升到第四名。并且从趋势图中可以看出其还在呈增长趋势。

 <div align="center">
     <img src="/images/tutorials/1-python-basic/py-introduction-1.png">
 </div>

 <div align="center">
     <img src="/images/tutorials/1-python-basic/py-introduction-2.png">
 </div>


## 2 为什么名字叫 “Python”

我们知道英文单词 “Python” 的中文翻译是“蟒蛇”或者“巨蛇”的意思。但作为一名编程语言的名称，Python的初始意思并不是这样的。

Python这个名字，来自Guido所挚爱的电视剧 "Monty Python’s Flying Circus" 。他希望这个新的叫做Python的语言，能符合他的理想：创造一种C和shell之间，功能全面，易学易用，可拓展的语言。

## 3 Python的发展历史


* Python 3.6 - 2016/12/23
* Python 3.5 - 2015/09/13
* Python 3.4 - 2014/03/16
* Python 3.3 - 2012/09/29
* Python 3.2 - 2011/02/20
* Python 3.1 - 2009/06/27
* Python 3.0 - 2008/12/03
* **Python 2.7 - 2010/07/03**
* Python 2.6 - 2008/10/1
* Python 2.5 - 2006/09/19
* Python 2.4 – 2004/11/30, 同年目前最流行的WEB框架Django 诞生
* Python 2.0 - 2000/10/16，加入了内存回收机制，构成了现在Python语言框架的基础
* 1999年 Python的web框架之祖——Zope 1发布
* Python 1.0 - January 1994 增加了 lambda, map, filter and reduce.
* 1991年，第一个Python编译器诞生。它是用C语言实现的，并能够调用C语言的库文件。从一出生，Python已经具有了：类，函数，异常处理，包含表和词典在内的核心数据类型，以及模块为基础的拓展系统。
* 1989年的圣诞节，Guido开始编写Python语言的编译器。

2014年11月，**Python2.7 将在 2020 年停止支持的消息被发布，并且不会再发布 2.8版本，建议用户尽可能的迁移到3.4+**,   Python 最初发布时，在设计上有一些缺陷，比如 Unicode 标准晚于 Python 出现，所以一直以来对 Unicode 的支持并不完全，而 ASCII 编码支持的字符有限。例如： 对中文支持不好 Python3 相对 Python 早期的版本是一个较大的升级，Py3 在设计的时候没有考虑向下兼容，所以很多早期版本的 Python 的程序无法再 Py3 上运行。


## 4 Python语言的特点

**1） 简单易学**

Python语言简洁、优雅，相比其他程序语言（比如：Java，C++等），Python语言更容易阅读和编写。Python语言使用起来很有趣，且该语言更多的是专注于解决方案而不是语法本身。

初学者学Python，不但入门容易，而且将来深入下去，可以编写那些非常非常复杂的程序。

**2） 开源且免费**

Python是一门开源的编程语言，可以免费使用，甚至可以用于商业用途。当然，你也可以提交相关贡献代码。 Python有一个庞大的社区来维护并保持快速发展。

**3） 可移植性**

Python是跨平台的编程语言，它可以运行在 windows、Mac 和 各种 Linux/Unix 系统上。也就是说，假设在 windows 系统下编写的 Python 程序，在 Mac 或 Linux 系统下也是可以运行的。

**4） 开发效率非常高**

Python有非常强大的第三方库，基本上你想通过计算机实现任何功能，Python官方库里都有相应的模块进行支持，直接下载调用后，在基础库的基础上再进行开发，大大降低开发周期，避免重复造轮子。


**5） 高级语言**

当你用Python语言编写程序的时候，你无需考虑诸如如何管理你的程序使用的内存以及垃圾回收一类的底层细节。

**6） 可扩展性**

如果你需要你的一段关键代码运行得更快或者希望某些算法不公开，你可以把你的部分程序用C或C++编写，然后在你的Python程序中使用它们。

**7） 可嵌入性**

你可以把Python代码嵌入你的C/C++程序，从而向你的程序用户提供脚本功能。

**8） 面向对象**

Python中的一切都是对象。面向对象编程（OOP）可以帮助您直观地解决复杂的问题。有了OOP，你可以通过创建对象把这些复杂的问题划分成更小的集合。




## 5 Python语言的应用

### 5.1 Python语言的应用领域

Python被广泛的应用于诸多领域，比如：

* **数据科学：** 数据科学涉及领域很广，涉及到当前火热的人工智能领域。Python的数据科学包括数据分析、数据可视化、数据挖掘、自然语言处理、机器学习、深度学习等。典型的Python库有：Numpy，Scipy，Pandas，Matplotlib，Seaborn，Scikit-learn，tensorflow等。

* **云计算：** 典型应用——Python开发的OpenStack


* **WEB开发：** 众多优秀的WEB框架，比如：Django、flask、 tornado

* **网络爬虫：** 使用Python可以便捷的编写网络爬虫，从网页上爬取相关信息，常用的Python库包括：Requests，BeautifulSoup，Scrapy等。

* **系统运维：** 运维人员必备，slatstack（系统自动化配置和管理工具），Ansible（自动化运维工具）

* **图形界面开发：** wxPython、PyQT、TKinter


### 5.2 使用Python的部分知名公司

越来越多的公司选在python作为其主要开发语言，例如：

* Google - Google Groups、Gmail、Google Maps、AlphaGo等，Google App Engine支持python作为开发语言
* NASA - 美国宇航局，从1994年起把python作为主要开发语言
* Dropbox - 美国最大的在线云存储网站，全部用Python实现，每天网站处理10亿个文件的上传和下载
* 豆瓣网 - 图书、唱片、电影等文化产品的资料数据库网站
* BitTorrent - bt下载软件客户端
* gedit - Linux平台的文本编辑器
* GIMP - Linux平台的图像处理软件(Linux下的PS)
* 知乎（国外的Quora） - 社交问答网站，国内最大的问答社区，通过Python开发
* Autodesk Maya - 3D建模软件，支持python作为脚本语言
* YouTube:世界上最大的视频网站YouTube就是用Python开发的
* Facebook:大量的基础库均通过Python实现的
* Redhat: 世界上最流行的Linux发行版本中的yum包管理工具就是用python开发的

除上面之外，还有搜狐、金山、腾讯、盛大、网易、百度、阿里、淘宝 、土豆、新浪、果壳等公司都在使用Python完成各种各样的任务。

更多案例：
<a href="https://www.python.org/about/success/" target="blank">https://www.python.org/about/success/</a>


## 6 Python语言的一些缺点

任何编程语言都有缺点，Python也不例外。比如：

1. 速度慢，Python 的运行速度相比C语言确实慢很多，跟JAVA相比也要慢一些，因此这也是很多所谓的大牛不屑于使用Python的主要原因，但其实这里所指的运行速度慢在大多数情况下用户是无法直接感知到的，必须借助测试工具才能体现出来，比如你用C运一个程序花了0.1s,用Python是0.01s,这样C语言直接比Python快了10s,算是非常夸张了，但是你是无法直接通过肉眼感知的，因为一个正常人所能感知的时间最小单位是0.15-0.4s左右。其实在大多数情况下Python已经完全可以满足你对程序速度的要求，除非你要写对速度要求极高的搜索引擎等，这种情况下，当然还是建议你用C去实现的。
随着硬件设备及计算力的增强，运行速度慢的这个缺点相对来说有所改善。在编写代码所花时间与程序运行所用时间进行平衡后，很多企业都开始选择使用Python。

1. 代码不能加密，因为PYTHON是解释性语言，它的源码都是以名文形式存放的，不过我不认为这算是一个缺点，如果你的项目要求源代码必须是加密的，那你一开始就不应该用Python来去实现。



## 7 Python2还是Python3？

py2.7是2.x系列的最后一个版本，已经停止开发，不再增加新功能。2020年终止支持。所有的最新的标准库的更新改进，只会在3.x的版本里出现。Guido决定清理Python2.x ，并且不再兼容旧版本。

最大的一个改变就是使用Unicode作为默认编码。Pyhton2.x中直接写中文会报错，Python3中可以直接写中文了。

py3比py2更规范统一、去掉了没必要的关键字。Python3.x还在持续改进。所以还是 **推荐大家使用Python3.x**。


## 参考文献：

1. <a href="https://www.tiobe.com/tiobe-index/" target="blank">TIOBE Index for November 2017</a>
1. <a href="https://www.cnblogs.com/hujq1029/archive/2016/08/19/5789268.html" target="blank">Python的历史</a>
1. <a href="https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431608990315a01b575e2ab041168ff0df194698afac000" target="blank">Python简介</a>
1. <a href="http://www.15yan.com/story/1JKTBQvVk5e/" target="blank">一门编程语言的发展简史</a>
1. <a href="https://www.programiz.com/python-programming" target="blank">Learn Python Programming-The Definitive Guide</a>


<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="200">
</div>
