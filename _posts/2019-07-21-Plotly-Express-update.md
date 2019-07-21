---
layout: posts
title: "可视化神器 Plotly Express 合并到 Plotly，安装过程有点小尴尬"
teaser:
date: 2019-07-21
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PythonVisualization
tags:    
   - Plotly      
comments: true
show_meta: true
sidebar: right
authors: ["Lemonbit"]
---

{% include alert info='可视化神器 Plotly Express 合并到 Plotly，安装过程有点小尴尬' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# 可视化神器 Plotly Express 合并到 Plotly，安装过程有点小尴尬

前些时候，「Python数据之道」发布了关于 Plotly Express 的两篇文章，发现大家都很喜欢这个新的可视化神器。

7月17日， Plotly 官方宣布将 Plotly 更新到 version 4，并将 plotly_express 包含到其中，所以，今后可以直接在 Plotly 中在运行 Plotly Express 了。

>Plotly Express is now part of Plotly.py version 4 and so the plotly_express module now just re-exports the contents of plotly.express

今天，我们紧跟时代步伐，来畅览新的版本， let's go!!!

## 如何安装？

**Method-1：直接更新 Plotly**

在终端输入代码如下：

```python
pip install plotly --upgrade
```

**Method-2: 更新 plotly_express**

在终端输入代码如下：

```python
pip install plotly_express==0.4.0
```

安装完成后，后续使用 plotly express 就可以用下面的方式来运行

```python
# 在 plotly v4.0 版本中
import plotly.express as px

# 在 plotly_express v0.1.1 版本中
# import plotly_express as px
```

更多说明可以在 Plotly Express 的 github 项目仓库中来查找

[https://github.com/plotly/plotly_express](https://github.com/plotly/plotly_express)

## 小小插曲

然而，在安装过程中，发现了一些小小的尴尬。

在执行以下命令的时候：

```python
pip install plotly --upgrade
```

会提示 cufflinks 的安装环境与 Plotly 的更新有冲突，图示如下：

![img01](/images/posts/012-plotly-express-update/plotly-1-revise.jpg)

cufflinks （version 0.16） 安装时要求 plotly 的版本小于 4.0 版本，所以当更新 plotly 到 v4 版本的时候， cufflinks v0.16 就不能运行了。

为了让 cufflinks 能顺利运行，我们更新安装这个库试试

```python
pip install cufflinks --upgrade
```

过程图示如下：

![img02](/images/posts/012-plotly-express-update/plotly-2.png)

![img03](/images/posts/012-plotly-express-update/plotly-3.png)

在更新完成后，会提示 Plotly 版本由 v4.0 降级到了 v3.1 版本。

在这种情况下，如果还想运行 Plotly Exprss，需要安装以前的单独的版本，在终端执行以下代码

```python
pip install plotly_express==0.1.1
```

如果想在不同的环境中使用 plotly_express 的最新版本（即 Plotly v4）或者使用 cufflinks ，可以考虑安装两个虚拟环境，分别安装 plotly v4 版本 和 cufflinks，来暂时解决这个问题。

当然， 考虑到 plotly 4.0 刚刚发布没多久 （7月17日），cufflinks 后续应该会修改这个安装的要求，以适应 plotly 的最新版。

## 最后

如果你还不知道 cufflinks 是什么，建议先自行了解下，跟 plotly express 一样， cufflinks 也是一个神器哦。Cufflinks 是基于 Plotly 来运行的，后续会介绍下，这里先上一张用 cufflinks 绘制的图，大家可以先过过眼瘾。

![img04](/images/posts/012-plotly-express-update/cufflinks1.gif)

---

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PythonDataLab），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
