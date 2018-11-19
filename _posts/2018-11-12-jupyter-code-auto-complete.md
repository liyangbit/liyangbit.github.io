---
layout: posts
title: "两行代码搞定：Jupyter Notebook中自动补全代码"
teaser:
date: 2018-11-12
header:
   image_fullwidth: "image-article-head.jpg"
categories:
   - Jupyter
tags:
   - Jupyter
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---



{% include alert info='两行代码搞定：Jupyter Notebook中自动补全代码' %}



在之前的文章中，已经介绍了在Jupyter Notebook中设置主题以及输出代码文件到pdf文件中，本文来继续介绍jupyter notebook中一些实际的技巧。

本次要介绍的两个功能是：

（1）针对 jupyter notebook 中的 Markdown 文件自动生成目录

（2）自动补全代码

上述两个功能，都是有 python的一个 jupyter 扩展插件Nbextensions库来实现。

安装该库的命令如下：

```python
python -m pip install jupyter_contrib_nbextensions
```

然后执行：

```python
jupyter contrib nbextension install --user --skip-running-check
```


安装完成后，勾选 “Table of Contents” 以及 “Hinterland”。

其中 Hinterland 是用来自动补全代码的，这个拓展的代码补全功能虽然没有 PyCharm中的那么全面，但比没有是要好多了。

设置如下：

<div align="center">
    <img src="/images/posts/jupyter-code-auto-complete/1.png">
</div>

自动补全代码的效果如下：

<div align="center">
    <img src="/images/posts/jupyter-code-auto-complete/2.gif">
</div>

关于 Jupyter Notebook，如果你有一些好的使用技巧，不妨跟大家一起分享下。







对我的文章感兴趣的朋友，可以关注我的微信公众号『Python数据之道』（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
