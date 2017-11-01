---
layout: posts
title: "Anaconda安装虚拟环境到指定路径"
teaser:
date: 2017-06-23
header:
   image_fullwidth: "image-head.jpg"
categories:
   - Anaconda
tags:
   - Anaconda
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---

**Python：Anaconda安装虚拟环境到指定路径**



<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>





## 1 曾经的困扰

有段时间，想使用基于不同python版本的anaconda，就直接从官网下载了两个不同的anaconda版本进行安装。

刚开始的时候，还觉得也没啥问题。用了一小段时间，在安装其他的第三方库时，经常发现安装失败，并且经常出现下面的问题：

![](http://oqb5ftrdh.bkt.clouddn.com/17-6-22/71956562.jpg)

这个问题，我google、百度等查了好久，也没有解决好。后来，我把两个版本的anaconda都卸载了，重新安装了其中一个版本，发现再安装其他第三方库时，上述问题就不存在了。

很有可能上述问题就是同时安装两个版本的anaconda引起的（不过我也不能完全肯定）。

## 2 安装虚拟环境

虽然只安装一个版本的anaconda，能顺利的运行，但有时候，还是需要运行基于不同python版本的anaconda的。后来了解到，anaconda是可以在虚拟环境下运行不同python版本的。

下面的步骤演示了我的安装过程，也希望大家能避免一些坑。

先说下我的安装环境：

* windows7,64位系统
* 目前安装了anaconda4.2.0版本（基于python3.5）

由于经常看到有说python3.6版本运行效率比3.5版本高，就有一种想安装3.6的冲动，但是由于部分python库还不支持3.6，所以主要版本还是以3.5为主。

anaconda安装python3.6版本的虚拟环境的步骤如下：

输入安装命令：
```
conda create -n py36 python=3.6
```
结果如下：

![](http://oqb5ftrdh.bkt.clouddn.com/17-6-22/80922784.jpg)

“-n”命令，会将虚拟环境安装在anaconda已安装路径默认的envs目录下。

可以看出，命令行中提示有一个警告，由于我的anaconda默认安装路径中有空格，提示可能会引起一些问题。由于有这个警告，我就没有继续往下安装。

另外，我的路径在C盘，考虑到可能空间也不多，还是不要继续往C盘装东西了。于是想，要是能安装在其他指定路径且路径中没有空格，不就解决这个问题了么。

通过查阅anaconda的文档，发现是可以进行指定路径安装的。可以输入如下命令进行查看：

```
conda create --help
```

**安装虚拟环境到指定路径的命令如下：**

```
conda create --prefix=D:\python36\py36 python=3.6
```
![](http://oqb5ftrdh.bkt.clouddn.com/17-6-22/10330601.jpg)

上面的命令中， 路径D:\python36是先建好的文件夹，py36是需要安装的虚拟环境名称。请注意，安装完成后，虚拟环境的全称包含整个路径，为D:\python36\py36。激活指定路径下的虚拟环境的命令如下：
```
activate D:\python36\py36
```

退出虚拟环境的命令如下：
```
deactivate
```
![](http://oqb5ftrdh.bkt.clouddn.com/17-6-22/20853572.jpg)


想要删除指定路径下的虚拟环境，使用如下的命令：
```
conda remove --prefix=D:\python36\py36 --all
```
![](http://oqb5ftrdh.bkt.clouddn.com/17-6-22/24065329.jpg)

插播一条硬广：技术文章转发太多。文章来自微信公众号“Python数据之道”（ID：PyDataRoad）。

## 3 虚拟环境下安装python库
如果还想继续安装python2.7的虚拟环境，方法跟安装python3.6是一致的。

上述虚拟环境py27安装完成后，激活后虚拟环境后，可以安装其他python库。

比如安装requests库

```
pip install requests
```

是可以安装成功的。

**如果遇到pip安是失败**，可尝试用下述方法：（指定路径下安装）：

```
conda install -prefix=D:\pyenv\py27 package
```

请注意，默认路径下的命令是 "conda install -n py27 package"

当然了，**有一些库不论conda和pip都无法直接安装，只能下载.whl进行安装**。
安装的方法可以参考下面文章的内容：


最后，来查看已安装好的虚拟环境下都安装了那些python库，输入命令：

```
conda list
```

发现已安装好的虚拟环境py36下只安装了少量的基本库，如果也想用3.6版来进行科学计算，需要安装许多其他的库，如果一个一个安装，则会费时费力，而且容易出错，这时anaconda提供了一个命令，可以把基于python3.6版本的anaconda中的其他库一次安装好，命令如下：

```
conda install -prefix=D:\pyenv\py36 anaconda
```
![](http://oqb5ftrdh.bkt.clouddn.com/17-6-22/10330601.jpg)

**请特别注意，全部安装时，安装包会很多，安装时间比较长，同时占用安装空间也会比较大，请根据自己的需求选择是否安装**

我安装py36全部的库花了大概1个多小时。



{% include alert text='对我的文章感兴趣的朋友，可以关注我的微信公众号"Python数据之道"，接收我的更新通知。' %}

<div align="center"><img src="/images/qrcode.jpg" width="200"/></div>
