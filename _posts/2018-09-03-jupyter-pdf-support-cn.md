---
layout: posts
title: "Jupyter Notebook 输出pdf并支持中文显示"
teaser:
date: 2018-09-03
header:
   image_fullwidth: "image-head.jpg"
categories:
   - Jupyter
tags:
   - Jupyter
   - recommend
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---



{% include alert info='Jupyter Notebook 输出pdf并支持中文显示' %}


<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>


Jupyter Notebook 作为用 Python 进行数据分析的重要工具之一，其最大的特色是可以将代码和结果同步显示在源文件里。

Jupyter Notebook 可以用来 演示，以及输出各种形式的文件，比如 ipynb，html， md， py， pdf 等，本文主要来阐述如何将写好的内容输出为 pdf 格式的文件。

在 Jupyter Notebook 输出 pdf 过程中，相信不少同学因为遇到难以解决的坑而退却，这里给大家分享下我的历程，希望对大家有所帮助。

系统环境：
* windows 7 / windows 10
* Anancoda （基于 Python 3.6 版）

## 安装顺序

我是按照下面的顺序安装的，缺少支持的话，pip install xxxxxx

1 安装pandoc

https://github.com/jgm/pandoc/releases

2 安装MiKTex

https://miktex.org/download


3 中文支持

（1）直接修改tex模版文件。

首先找到 article.tplx 文件，我的 Anaconda 中的路径是

 D:\ProgramData\Anaconda3\pkgs\nbconvert-5.3.1-py36h8dc0fde_0\Lib\site-packages\nbconvert\templates\latex\article.tplx

你可以根据自己的安装情况，找到该文件，然后用文本编辑器打开article.tplx ，将 “\documentclass[11pt]{article}” 修改为 “\documentclass{ctexart}”，对比图示如下：

修改前：

<div align="center">
    <img src="/images/posts/jupyter-pdf-support-cn/1.png">
</div>


修改后：

<div align="center">
    <img src="/images/posts/jupyter-pdf-support-cn/2.png">
</div>


（2）打开一个含有中文内容的 jupyter notebook 文件 （.ipynb 文件），在浏览器中打开，选择输出为 pdf 文件 （我这里是新建的一个空白的 ipynb 文档），如下：

<div align="center">
    <img src="/images/posts/jupyter-pdf-support-cn/3.jpg">
</div>


这里有可能 可以正常的输出 含有中文的 pdf 文档， 如果已输出，那么恭喜你，已经成功啦。

但我在输出 含有中文内容的 pdf 文档过程中，还遇到了一些问题，主要是 显示 “ XXX.sty”
文件缺失，这时候，需要安装缺失的文件，由于默认安装情况下，经常会失败。

此时，我们需要选择安装源，步骤图示如下：

首先，要通过点击 “Change”来选择
<div align="center">
    <img src="/images/posts/jupyter-pdf-support-cn/4.jpg">
</div>


按下面图示选项，点击 “next”
<div align="center">
    <img src="/images/posts/jupyter-pdf-support-cn/5.png">
</div>

在选择安装源的时候，如果有中国的安装来源，请优先选择国内的，这样安装速度较快，成功率相对高些。 如果没有，可以多尝试几次其他国家和地区的， 比如 日本等地。
<div align="center">
    <img src="/images/posts/jupyter-pdf-support-cn/6.png">
</div>

选择好后，点击安装即可。
<div align="center">
    <img src="/images/posts/jupyter-pdf-support-cn/7.png">
</div>


一般情况下，将上述缺失的多个 .sty 文件安装好后，是可以输出 含中文内容的 pdf 文档的。

至此，我们本次的目的已基本完成。

但在后面的使用过程中，我还发现了一个小 bug， 就是： 如果 .ipynb 文件的命名中含有中文，则文件名输出是不含中文的，不知道这个问题有没有同学可以提供下解决方案，欢迎献计献策~



本文参考以下内容：

* https://www.cnblogs.com/SC-CS/p/Jupyter-PDF_Chinese_Support.html

对我的文章感兴趣的朋友，可以关注我的微信公众号『Python数据之道』（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
