---
layout: page
sidebar: "right"
comments: true
show_meta: true
authors: ["阳哥"]
title: "图解Pandas教程"
header:
   image_fullwidth: "image-pandas.jpg"
permalink: "/tutorials-3-graphical-pandas/"
---

## 图解Pandas教程

<!-- 大家好，我是阳哥。

有不少同学跟我提过，看看能不能出一系列Pandas数据处理的教程，之前一直也没来得及弄。为了大家能够生动、形象的学习 Pandas，阳哥打算制作一系列 **《图解Pandas》** 的内容，跟大家以动态图片、视频等方式来讲解 Pandas 的基础知识，方便大家快速的掌握这些知识，相信《图解Pandas》会给大家带来一些不一样的视角。

《图解Pandas》的视频内容可以在微信视频号「Python数据之道」观看：

<div align="center">
    <img src="../../images/QR-shipin-PyDataLab.jpeg" width="40%">
</div>

**《图解Pandas》系列已发布的图文汇总如下：**

- [《图解Pandas》内容框架介绍](https://mp.weixin.qq.com/s/gh063BUAM90vFhy6ZLaznw)
- [图文01-数据结构介绍](https://mp.weixin.qq.com/s/H9kJf9zJU7ys6esr0DBhHg)
- [图文02-创建数据对象](https://mp.weixin.qq.com/s/tH8bc20DvhA7i8HDlWv0uQ)
- [图文03-读取和存储Excel文件](https://mp.weixin.qq.com/s/fmhnE20HOBCG-AGh6_lrxA)
- [图文04-常见的数据访问](https://mp.weixin.qq.com/s/yyT9okzlbb-f7P9yGeQPQQ)
 -->





大家好，我是阳哥。

有不少同学跟我提过，看看能不能出一系列Pandas数据处理的教程，之前一直也没来得及弄。

为了大家能够生动、形象的学习 Pandas，阳哥打算制作一系列 **《图解Pandas》** 的内容，跟大家以动态图片、视频等方式来讲解 Pandas 的基础知识，方便大家快速的掌握这些知识，相信《图解Pandas》会给大家带来一些不一样的视角。

《图解Pandas》的视频内容一般如下：

<!-- ![](https://tva1.sinaimg.cn/large/e6c9d24egy1h1akk6t223j20u00zd422.jpg) -->

<div align="center">
    <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h1akk6t223j20u00zd422.jpg" width="40%">
</div>

扫描下面的二维码，关注视频号，可以观看《图解Pandas》已发布的所有视频以及及时推送最新的视频：

<div align="center">
    <img src="../../images/QR-shipin-PyDataLab.jpeg" width="40%">
</div>

<!-- ![](https://tva1.sinaimg.cn/large/e6c9d24egy1h1ajrzuq7gj20u0130q4w.jpg) -->

<!-- ![](https://tva1.sinaimg.cn/large/e6c9d24egy1h1ajvrpcjxj20ry0s1q54.jpg) -->

**《图解Pandas》** 系列，是一个不小的工程，预计将会有超过100个视频，现在来说算是一个期货吧，搞不好就是烂尾工程啦，大家的支持就是我持续分享的动力，嘿嘿~~

## 01 已发布的内容

《图解Pandas》系列包括在微信视频号「Python数据之道」上发布的视频，以及在公众号「Python数据之道」上发布的图文（图文中配套源代码以及在PC端观看的视频）。

《图解Pandas》系列已发布的图文链接以及对应的视频编号，汇总如下：

图文链接|对应的<br>视频编号
:-------|:---------:
[《图解Pandas》内容框架介绍](https://mp.weixin.qq.com/s/gh063BUAM90vFhy6ZLaznw)|-
[图文01-数据结构介绍](https://mp.weixin.qq.com/s/H9kJf9zJU7ys6esr0DBhHg)|001
[图文02-创建数据对象](https://mp.weixin.qq.com/s/tH8bc20DvhA7i8HDlWv0uQ)| 002
[图文03-读取和存储Excel文件](https://mp.weixin.qq.com/s/tH8bc20DvhA7i8HDlWv0uQ)| 003<br>004
[图文04-常见的数据访问](https://mp.weixin.qq.com/s/yyT9okzlbb-f7P9yGeQPQQ)| 005
[图文05-常见的数据运算](https://mp.weixin.qq.com/s/-9aZN6VW8x9Q_SYzz__dSA)|006<br>007<br>008
[图文06-常见的数学计算](https://mp.weixin.qq.com/s/fN4gc9PyzwN3Y4nyzSgqdw)|009<br>010



## 02 部分内容介绍

### 数据结构介绍

DataFrame （中文翻译“数据框”）介绍：

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h03dhtmew3j21fm0sgjv9.jpg)

图解数据框轴方向：

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h03di1i5l6j21700mwq4x.jpg)

DataFrame 与 Series 之间的关联：

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h1aibw4dezg20nq0dc4q5.gif)

### 创建数据对象

**通过列表创建数据框**


![](https://tva1.sinaimg.cn/large/e6c9d24egy1h08djb0j5zj21wk0lk0uz.jpg)

### 读取Excel文件

从Excel读取数据时，有时需要跳过数据文件末尾部分数据行：

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h0ouhp1ho2g20nq0dc1l0.gif)

### 数据框行列转置

通过 `df.T` 可以实现数据框的行列转置：

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h0xrow0dokg20nq0dcu10.gif)

### 索引位置以及索引值

如何获取 Series 中最小值或最大值对应的索引位置以及索引值。

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h16ydrcjc6j20zo0kewh8.jpg)

通过 `argmin()` 函数来获取最小值对应的索引位置，如红色标注所示，结果为数字 `1` ，如下：

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h16yduvw55j21c00io76y.jpg)

通过 `idxmin()` 函数来获取最小值对应的索引值，如红色标注所示，结果为字母 `e` ，如下：

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h16ydyg350j21c00hkmzz.jpg)



## 03 专题汇总

考虑到《图解Pandas》系列内容在不断更新过程中，大家可以通过下面的专题来找到最新发布的内容。

[![](https://tva1.sinaimg.cn/large/e6c9d24egy1h01f6wflmkj20go05kjrh.jpg)](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzI2NjY5NzI0NA==&action=getalbum&album_id=2293754972943122444#wechat_redirect)

同时考虑到，以后如果文章数量较多（比如超过50篇文章），可能在专题中也不好快速的找到所需要的内容，我会以文章汇总的形式，将《图解Pandas》系列的文章进行手动汇总，并形成 `图解Pandas汇总` 的专题，最新的汇总文章，可以点击下面专题，找到最新的文章即可。

[![](https://tva1.sinaimg.cn/large/e6c9d24egy1h01f6yjzssj20go05kweo.jpg)](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzI2NjY5NzI0NA==&action=getalbum&album_id=2293756873331933190#wechat_redirect)

---

学习 Pandas，最难的还在于坚持，希望《图解Pandas》能给大家带来一些乐趣，一起加油吧！
