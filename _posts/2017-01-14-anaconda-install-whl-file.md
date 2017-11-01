---
layout: posts
title: "Anaconda安装第三方包（whl文件）"
teaser:
date: 2017-01-14
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
**Anaconda安装第三方包（whl文件）**


<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>



**先说下环境**

Anaconda 对应Python3.5的版本

win7,64位系统。



## step1：下载whl文件

## step2：打开‘Anaconda Command Prompt‘，

如下图：

<div align="center">
    <img src="/images/posts/anaconda-whl/1.png">
</div>



## step3：命令行窗口pip安装，代码如下：

pip install 路径+whl文件名

具体如下：


<div align="center">
    <img src="/images/posts/anaconda-whl/2.png">
</div>



## Step4：检查是否安装成功

命令行窗口输入： pip list

查看是否有刚才安装的package。


{% include alert text='对我的文章感兴趣的朋友，可以关注我的微信公众号"Python数据之道"，接收我的更新通知。' %}

<div align="center"><img src="/images/qrcode.jpg" width="200"/></div>
