---
layout: posts
title: "在jupyter notebook中同时安装python2和python3"
teaser:
date: 2017-06-30
header:
   image_fullwidth: "image-head.jpg"
categories:
   - Jupyter
tags:
   - Anaconda
   - Jupyter
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---

**在jupyter notebook中同时安装python2和python3**


<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>





之前讨论过在anaconda下安装多个python版本，本期来讨论下，jupyter notebook中怎样同时安装python2.7 和python3.x。

由于我之前使用的jupyter notebook是基于python3.5版本的，所以只要在此基础上安装基于python2.7的内核即可。

我的环境如下：
* windows 7， 64位系统
* 已安装基于python3.5版本的anaconda
* 已在anaconda中安装py27和py36的虚拟环境
* 已有的jupyter notebook的kernel是基于python3.5版本的

## 在py35的kernel基础上安装py27的kernel
在jupyter notebook中，选择不同的python版本，叫做kernel（内核）。在进行安装时，安装 ipykernel即可。

在之前的文章中，我是将anaconda的虚拟环境py27安装在指定路径（D:\pyenv\py27），因此在安装ipykernel时，也需要指定安装。
安装命令为 “conda install --prefix=D:\pyenv\py27 ipykernel”，如下：

![](http://oqb5ftrdh.bkt.clouddn.com/17-6-29/17825040.jpg)

然后激活py27的虚拟环境，输入命令：

```python
python -m ipykernel install --user
```

![](http://oqb5ftrdh.bkt.clouddn.com/17-6-29/11590887.jpg)

启动jupyter notebook去查看，这时可以看到，python2的kernel已经安装好了。

![](http://oqb5ftrdh.bkt.clouddn.com/17-6-29/37253866.jpg)


## 快速启动jupyter notebook的一个小技巧

启动jupyter notebook时，每次都要切换文件路径，需要输入相关的命令，相对来说比较麻烦。
有一个可以快速启动的小技巧，我个人一般是这么用的。

在你想要打开的文件夹路径下，创建一个后缀为“.bat”的文件（比如命名为 ipy.bat），用记事本打开这个文件，输入如下内容并保存。

```
rem -- start_ipython_notebook_here.bat ---
dir
jupyter notebook
pause

```

以后双击这个 ipy.bat文件，就可以快速启动jupyter notebook。我一般会把这个 ipy.bat文件以快捷方式发送到桌面（纯懒人的做法~~）。




{% include alert text='对我的文章感兴趣的朋友，可以关注我的微信公众号"Python数据之道"，接收我的更新通知。' %}

<div align="center"><img src="/images/qrcode.jpg" width="200"/></div>
