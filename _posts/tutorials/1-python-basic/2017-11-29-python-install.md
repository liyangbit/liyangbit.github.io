---
layout: posts
title: "02-Python安装"
subheadline: "Python基础"
teaser:
date: 2017-11-29
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

**Python基础（2）-Python安装**

本文来源于：
<!-- [liyangbit.com](liyangbit.com) -->
<a href="http://liyangbit.com" target="blank">liyangbit.com</a>

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>


Python是跨平台的编程语言，它可以运行在 windows、Mac 和 各种 Linux/Unix 系统上。也就是说，假设在 windows 系统下编写的 Python 程序，在 Mac 或 Linux 系统下也是可以运行的。

目前，Python的版本主要分为两大类，即 Python3.X版 和 Python2.x版，这两个版本是不兼容的。由于 Python3.x版本越来越普及，我们接下来的教程使用 Python3.x版 （Python3.5版 或 Python3.6版及以上版本均可）。

由于个人windows系统用的多，所以这里的 Python 安装以 windows 系统为例来演示。

其他系统下的安装，建议大家可以在网上查找相关安装方法。

下面介绍在 Windows 系统下两种常用的安装方法，第一种最基础，第二种会在以后带来一些便利，个人推荐第二种安装方法。

## 1 安装方法一：

### 1.1 安装官方版Python3.6

下面以在 windows 系统下安装 Python3.6版为例：

首先，根据你的 Windows版本（64位还是32位）从Python的官方网站下载 Python3.6 对应的 <a href="https://www.python.org/ftp/python/3.6.3/python-3.6.3-amd64.exe" target="blank">64位安装程序</a>  或 <a href="https://www.python.org/ftp/python/3.6.3/python-3.6.3.exe" target="blank">32位安装程序</a> ，然后，运行下载的EXE安装包：


<div align="center">
    <img src="/images/tutorials/1-python-basic/1-py-install-1.png">
</div>

特别要注意勾上 `Add Python 3.6 to PATH` ，然后点 “Install Now” 即可完成安装，如下：

<div align="center">
    <img src="/images/tutorials/1-python-basic/1-py-install-2.png">
</div>

<div align="center">
    <img src="/images/tutorials/1-python-basic/1-py-install-3.png">
</div>

<div align="center">
    <img src="/images/tutorials/1-python-basic/1-py-install-4.png">
</div>

### 1.2 运行Python3.6

安装完成后，我们可以运行下 python 的解释器里，来查看是否安装成功。

首先，在windows开始菜单里找到Python3.6，如下：

<div align="center">
    <img src="/images/tutorials/1-python-basic/1-py-install-5.png">
</div>


<div align="center">
    <img src="/images/tutorials/1-python-basic/1-py-install-6.png">
</div>

在这里，Python3.6程序下，有两个可以运行 python 的解释器，我们可以分别打开来看看。

* IDLE （Python 3.6 64-bit）：

<div align="center">
    <img src="/images/tutorials/1-python-basic/1-py-install-7.png">
</div>

* Python 3.6 （64-bit）

<div align="center">
    <img src="/images/tutorials/1-python-basic/1-py-install-8.png">
</div>

这两个界面下，都是可以运行 python 程序的。这里以 IDLE 界面位例，来测试下 python3.6 是否安装成功。

<div align="center">
    <img src="/images/tutorials/1-python-basic/1-py-install-9.png">
</div>

从上述界面来看，我们的安装程序能够运行 python 语句，已成功的安装了 Python3.6 。


## 2 安装方法二：（推荐）

第二种安装方法是推荐直接安装 Anaconda，它已经包含了Python的对应版本。

为什么要推荐安装 Anaconda 呢，针对想用 Python 来进行数据挖掘、机器学习等的童鞋，安装 Anaconda 会带来一些优势，比如：

1. 不需要配置PYTHON环境变量；
1. 集成很多packages，尤其是科学计算相关的库，省去安装这些库带来的烦扰；
1. packages的安装很简单，conda一键解决；

### 2.1 安装 Anaconda

安装最新版的 Anaconda 可以去官网下载，地址为：

[Anaconda官网下载地址](https://www.anaconda.com/download/)

windows系统下，如下：

<div align="center">
    <img src="/images/tutorials/1-python-basic/1-py-install-10.png">
</div>

根据系统，下载对应的版本，然后根据提示进行安装，选默认设置就可以，一路 "next"，即可安装完毕。

安装完毕后，打开 "cmd"命令行，查看 Anaconda 是否安装好。

<div align="center">
    <img src="/images/tutorials/1-python-basic/1-py-install-11.png">
</div>

在命令行中输入 `conda info` ，查看 Anaconda 安装的版本信息，以及对应的Python版本信息。

<div align="center">
    <img src="/images/tutorials/1-python-basic/1-py-install-12.png">
</div>

如果你得到了和上图类似的界面，恭喜你已经成功安装了 Anaconda 。

从上图来看，可知我安装的是 Anaconda 4.3.30版，对应的Python版本是 3.5.2版，**并不是目前的最新版**。

大家可以安装最新的 Anaconda版本，也可以选择安装跟我一样的版本。

如果想安装的版本不是最新版，可以在下述网址查找相应的版本。

[https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)

<div align="center">
    <img src="/images/tutorials/1-python-basic/1-py-install-13.png">
</div>

### 2.2 运行Python程序

Anaconda 安装完成后，可以在几个地方运行 Python程序，最常用的是 “IPython”。

在windows开始菜单的所有程序中找到 anaconda文件夹，点开文件夹，下面可以找到 “IPython”。

点击 “IPython”，得到如下界面:

<div align="center">
    <img src="/images/tutorials/1-python-basic/1-py-install-14.png">
</div>

从上述界面来看，我们的安装程序能够运行 python 语句，已成功的安装了 Anaconda 及 Python 。


## 3 总结

对于上述两种安装 Python 的方法，个人推荐第二种方法。尤其是想用Python来进行数序分析、数据挖掘、机器学习等，第二种方法会带来很多便捷性。

<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="200">
</div>
