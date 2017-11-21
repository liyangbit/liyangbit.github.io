---
layout: posts
title: "程序员如何优雅的排版微信公众号的内容"
teaser:
date: 2017-11-21
header:
   image_fullwidth: "image-head.jpg"
categories:
   - Comprehensive
tags:
   - Comprehensive
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---




{% include alert info='程序员如何优雅的排版微信公众号的内容' %}


<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>




经常有朋友问到关于公众号内容的排版问题，针对程序员的文章，这里提供两种解决方案。

公众号的排版，对于代码高亮，微信公众号默认的界面并不友好。

经常有朋友问到关于公众号内容的排版问题，针对包含代码的文章，这里提供两种解决方案。

## 首先

两种方案中，微信文章的编写都是以Markdown格式来编写的。如果不懂Markdown格式的相关语法，可以自行在网上查找相关内容。

## 方案一：

通过以下网站来将将编写好的Markdown文章内容进行格式转换。

<a href="http://md.phodal.com/" target="blank">http://md.phodal.com/</a>

这个是 IT大佬 Phodal提供的在线编辑器。

当然，如果你想个性化设置你自己的样式，你可以在他的 github 上 fork 编辑器的源文件，进行修改。
github 地址：
<a href="https://github.com/phodal/mdpub" target="blank">https://github.com/phodal/mdpub</a>


我的微信公众号文章，就是在修改了这个编辑器的一些样式之后，一般情况下就是用的这个编辑器来转换Markdown文章。


## 方案二：

在 Chrome 浏览器中 使用 Markdown Here 插件来转换 编写好的 Markdown 文章。

遗憾的是，原生的 Markdown Here 插件在微信公众号的后台编辑器界面中，代码不能换行，会显示代码错乱。

偶然看到，公众号 “Python之禅” 的作者已提供了一种解决方案，可以完好的转换 Markdown 文章。感兴趣的童鞋可以查看以下内容：

<a href="http://mp.weixin.qq.com/s?__biz=MjM5MzgyODQxMQ==&mid=2650367120&idx=1&sn=36dc4785e897394ca3321556c9094b46&scene=21#wechat_redirect" target="blank">程序员如何优雅地写公众号</a>




## 需要说明的是：

这两种方案下，对于图片，都需要在微信公众号的后台进行图片插入，在Markdown转换时，并不能将本地的图片在编辑器中显示出来。

当然，你可以找一个图床网站，将图片上传到网站，然后在文章中使用图片的网址链接。这样的话，图片是可以显示出来的。

如果您有更好的方法，欢迎给大家一起分享下。


<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="200">
</div>
