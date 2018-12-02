---
layout: posts
title: "Jupyter Notebook 主题设置、字体修改等"
teaser:
date: 2018-09-07
header:
   image_fullwidth: "image-head.jpg"
categories:
   - Jupyter
tags:
   - Jupyter
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---



{% include alert info='Jupyter Notebook 主题设置、字体修改等' %}


<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>



作为用Python进行数据分析的人员，jupyter notebook 在平时使用的频率很高。

但经常觉得 jupyter notebook 默认的风格不是很爽，总想换一换。

今天，我们来分享下给 jupyter notebook 更换主题等内容的方法。

## 新的风格预览

首先来看看我现在使用的风格，如下：

notebook页面：
<div align="center">
    <img src="/images/posts/jupyter-themes/1.png">
</div>

代码文件页面：
<div align="center">
    <img src="/images/posts/jupyter-themes/2.png">
</div>


## 主题更换过程

如果你也想更换下 jupyter notebook 的默认风格，不妨接着往下看。

Jupyter Notebook 的默认主题是白色背景的，虽然也简洁大方，便于使用。但是在长时间使用默认界面后，我只有一种感觉，就是亮瞎。。。无比怀念类似 Pycharm 或 vs code 中设置的黑色界面。。。

于是，想给 jupyter notebook 也换个黑色主题。

我搜索了Jupyter Notebook的themes，也就是自定义主题。发现 Github 已经有大神早已解决了这个问题。

本次，我们用到的库为 jupyterthemes ，这个第三方库有多个主题可以更换，并且还可以更换字体类型以及大小等，基本满足我们的需求。


jupyterthemes 的 github地址如下：
https://github.com/dunovank/jupyter-themes

默认情况下库的安装如下：

```python
pip install jupyterthemes
```

但是，上述方法我没有安装成功，主要是获取该库的请求失败。 于是，改用豆瓣的python库来源，安装的代码如下：

```python
pip install -i https://pypi.doubanio.com/simple/ jupyterthemes
```

<div align="center">
    <img src="/images/posts/jupyter-themes/3.png">
</div>

改用豆瓣的来源后，成功安装了 jupyterthemes ，接下来，我们可以来设置自己喜欢的主题。

在命令提示符下输入下面的代码来查看有哪些主题可以选择      

```python
jt -l
```

有如下一些主题可以选择
<div align="center">
    <img src="/images/posts/jupyter-themes/4.png">
</div>

有兴趣的同学可以尝试下这几个主题。

设置主题的代码如下：


```python
jt -t oceans16
```

上面设置的是使用 oceans16 这个主题，更换后的效果如下：

<div align="center">
    <img src="/images/posts/jupyter-themes/5.png" alt="5">
    <font size="2">图5</font>
</div>

<br>

<div align="center">
    <img src="/images/posts/jupyter-themes/6.png">
    <font size="2" >图6</font>
</div>



如果不喜欢上面的主题，可以使用下面的代码来恢复到默认主题，如下：

```python
jt -r
```

对比其他几个主题后，相对来说， 我更喜欢 monokai 以及 onedork 主题，最后，我选择使用 monokai 主题， 也就是本文开始的主题风格。

## <font color="red">我的主题设置参数</font>

我的 monikai 主题设置的详细参数如下：

```python
jt -t monokai -f roboto -nf robotosans -tf robotosans -N -T -cellw 70% -dfs 10 -ofs 10
```

乍一看，看到这串长长的代码，可能有点晕，但其实很好理解。

-t 是设置主题， -f 设置代码的字体， -nf 设置notebook的字体，等等。

更多的参数，请查看 jupyterthemes的参数设置说明，如下：


<div align="center">
    <img src="/images/posts/jupyter-themes/7.png">
</div>


怎么样，新的界面不错吧，心动不如行动，赶紧动手尝试下吧。

对我的文章感兴趣的朋友，可以关注我的微信公众号『Python数据之道』（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
