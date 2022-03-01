---
layout: posts
title: "一个高逼格的数学视频制作神器：Manim"
teaser:
date: 2021-12-22
header:
   image_fullwidth: "image-head.jpg"
categories:
   - VideoCreation
tags:    
   - Manim
   - ManimCE    
comments: true
show_meta: true
sidebar: right
authors: ["阳哥"]
---

{% include alert info='一个高逼格的数学视频制作神器' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# 一个高逼格的数学视频制作神器



大家好，我是阳哥。

## 写在前面

最近几个月，我在微信视频号「价值前瞻」和「Python数据之道」发布了一些视频，有不少同学问到这些视频是怎么做的，用什么工具制作的。

今天来给大家揭秘。

## 视频制作神器：Manim

最近，我在视频号上的视频，主要是用视频制作工具 `Manim` 来制作的。

`Manim` 真的是一款高逼格的视频制作工具，尤其是数学动画领域。

`Manim` 是斯坦福大学数学系小哥 `Grant Sanderson` 开源的python库，并用于 油管频道 `3Blue1Brown`，来解说数学等相关内容。

可能大家没有听说过 `3Blue1Brown` 频道, 该频道在油管上拥有 400万+ 的粉丝，其发布的视频浏览量大部分都在百万级以上，有不少在千万级别，作为一个专业领域的博主，不得不令人佩服。

![](https://tva1.sinaimg.cn/large/008i3skNgy1gxgoel10z4j31600u0dlb.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1gxgofslbsxj30zw0esjuk.jpg)

### Manim 的魅力

`Manim` 是一个非常优秀的数学动画制作引擎，可以制作视频格式(`.mp4`)或动图格式的动画(`.gif`)，先来几个动画感受一下 `Manim` 的魅力：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gxgoq44ajpg30nq0dch0e.gif)

<!-- ![](https://tva1.sinaimg.cn/large/008i3skNgy1gxgotj49c8g30nq0dce81.gif) -->

![](https://tva1.sinaimg.cn/large/008i3skNgy1gxgrcwck3tg30nq0dcqv5.gif)



### 版本说明

对于 `Manim` 的初学者，使用 `Manim` 时，对于其版本的选择还是有些需要注意的地方。

不同版本的代码、教程等，还是有稍许差异的。

目前 `Manim` 主要有三个版本：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gxgpx4ecblj30p905wwf3.jpg)

- 3b1b 旧版：3blue1brown 自己维护的版本，使用 Cairo 作为后端；

- 3b1b 新版：3blue1brown 自己维护的版本，使用 OpenGL 和 moderngl 来进行 GPU 渲染，优点是速度快；

- `Manim` 社区版：社区版是2020年下半年才出来的版本，目前主要是 `Manim` 旧版的衍生，更新更活跃，有很好的文档和社区支持。当然，随着社区版的迭代更新，目前 3b1b 新版 的某些特性也在逐步容纳进来。

关于这几个版本如何选择的建议：

- 如果你是新接触这个工具的，建议可以从社区版开始，主要是社区版的文档教程比较齐全，其迭代的频率更快。
- 如果你是 Python 或者其他编程领域的资深人士，想体自己动手深入理解并优化这个工具，可以使用 3b1b 新版
- 对于 3b1b 旧版，我暂时没有更多的想法

### Manim 的安装

在 `Manim` 3b1b 版本 和 社区版均提供了如何安装的文档：

- 3b1b 新版的安装文档： 
  https://github.com/3b1b/manim
- 社区版的安装文档： 
  https://docs.manim.community/en/stable/installation.html

有同学说安装是比较容易的，不过，我自己在安装过程中还是遇到了一些麻烦。

最终，上述两个版本我都安装了，大家可以自行尝试安装下。由于不同的电脑环境，有时候总会出现一些意想不到的bug，在这里我就不介绍如何安装了。

一些有意思的经历，在2019年的时候，我曾经尝试安装 3b1b 旧版 的 `Manim`，几次安装失败后，成功的被劝退了。

要不是开始弄视频，估计也不会再次去尝试安装 `Manim`，毕竟之前的阴影还在。

## 我的 Manim 之路

3b1b 新版 的 `Manim` 和 社区版的 `Manim` 我都有安装并使用。

在最初的时候，我只安装了 3b1b 新版，在制作了一些视频后，觉得支持文档这方面对我而言还是有不少难度，不然就需要自己花大量时间去熟悉这个工具的源代码。

总的来说，刚开始使用 3b1b 新版，在效率方面会有一些折扣。

后面折腾了一阵，将社区版也安装好了，所以我主要是以使用社区版 `Manim` 为主。

社区版的文档、教程，可以在下面这个网站查询：

https://docs.manim.community/en/stable/tutorials/quickstart.html

我制作的视频主要在微信视频号「价值前瞻」和「Python数据之道」上发布：

- **价值前瞻**，主要是读书、财经、思维等相关内容
- **Python数据之道**，主要是Python、算法、数学、IT等相关内容

### 价值前瞻

#### 思维类

对于读书笔记、思维模型等内容，其实也是可以用 `Manim` 这个工具的，尤其是某些思维模型如果涉及某些数学常识相关的内容。

当然，这类视频，并不一定要用 `Manim` 这个稍显生硬的工具，也还是有其他很多更好的工具的。

我制作了一个 “复利效应” 相关的视频，如下：

[![复利效应](https://tva1.sinaimg.cn/large/e6c9d24egy1gzu62vtd2ej20os0dwgm0.jpg)](https://mp.weixin.qq.com/s/Y_3CB6bxSqhn2UPdEY5vLw)

>注：视频需要在手机端观看

#### 财经类

时序数据的曲线绘制，在 `Manim` 上并没有提供现成的方法，但经过研究，其实也是可以实现的，如下：

[![收益率对比：沪深300 vs 标普500](https://tva1.sinaimg.cn/large/e6c9d24egy1gzu601wqnjj20oi0doaaz.jpg)](https://mp.weixin.qq.com/s/hPw-qIpae5trKI8xf_NktQ)

对于企业的财务数据，分阶段的可视化，也可以想办法来实现，如下：

[![企业观察：恒大](https://tva1.sinaimg.cn/large/e6c9d24egy1gzu604f0nfj20oq0dsmy4.jpg)](https://mp.weixin.qq.com/s/XQK7YdzI8xnZ27MGpYtd1g)

上面的效果，个人觉得，后续还可以继续优化，使切换过程更加平滑。

### Python数据之道

在视频号「Python数据之道」上，目前主要有 算法系列、数学系列的视频。

算法方面，目前已经跟大家分享了10大经典排序算法的视频。

[![](https://tva1.sinaimg.cn/large/008i3skNgy1gxgpx7zxpnj30p00aodi6.jpg)](https://mp.weixin.qq.com/s/ufViNQMGwx8KIX23x4rmOw)

**排序算法：** [汇总](https://mp.weixin.qq.com/s/ufViNQMGwx8KIX23x4rmOw) | [冒泡排序](https://mp.weixin.qq.com/s/qFuF6w00P6zPOkzDEoE4pA) | [选择排序](https://mp.weixin.qq.com/s/uJnamO-Pf_0smeIXbst0Qw) | [快速排序](https://mp.weixin.qq.com/s/cy5-M0aan8XO5K2CpEee0w) | [归并排序](https://mp.weixin.qq.com/s/o_-S9jHSBt_za3XK5kBW0A) | [堆排序](https://mp.weixin.qq.com/s/2dtKn4uNlhXsdhM1tGrD4w) | [插入排序](https://mp.weixin.qq.com/s/llgaW7HGJ4DRTRMkVX1N-A) | [希尔排序](https://mp.weixin.qq.com/s/14t7U61vW6xJDvhdoQbY_A) | [计数排序](https://mp.weixin.qq.com/s/cEVWRJ-xldYC9ztmBSpdhA) | [桶排序](https://mp.weixin.qq.com/s/mC7OobFV6GVNIkOA6PRKRg) | [基数排序](https://mp.weixin.qq.com/s/LFUUL7TlIr0csGwbl6fgOQ)

数学方面，主要是一些经典图形，以及一些基础内容的介绍，要做好数学类的视频，最最最主要的，还是要对数学原理有比较深入的理解，这个是最难滴~

[![有趣的数学：阶乘求和](https://tva1.sinaimg.cn/large/e6c9d24egy1gzu5gnyzczj20ns0d83za.jpg)](https://mp.weixin.qq.com/s/SaDoXDWyVqZdGE1HhrJfMA)

[![有趣的数学：正多边形](https://tva1.sinaimg.cn/large/e6c9d24egy1gzu5gs1uj3j20nw0d6dgl.jpg)](https://mp.weixin.qq.com/s/_icRJfCxipVMTj_zjcOMhQ)

此外，我还想弄一些跟Python相关的视频，暂时称之为 “好玩的Python”，最近的就是做了一个如何绘制圣诞树的视频。

类似的视频，有不少同学是用 Python 的 `Turtle` 包来绘制的，在这里我用 `Manim` 进行了绘制，效果如下：

[![好玩的Python：绘制圣诞树](https://tva1.sinaimg.cn/large/e6c9d24egy1gzu5gou6kej20ny0de753.jpg)](https://mp.weixin.qq.com/s/5zuQiY92eURu4H5Yt7grIw)


## 小结

不得不说，用 `Manim` 制作出来的视频，效果确实挺不错的。在使用过程中，也有一些小小的经验，想用 `Manim` 制作出高逼格的视频，需要注意的是：

- 每一个视频花费的时间，都会不少；
- 对 `Manim` 这个工具的源代码有足够的理解；
- 对 数学或算法知识有一定的理解；
- 所谓的高逼格，其实主要是在内容领域和视觉领域，内容领域体现的是内容的专业性，而视觉领域，则更多的是艺术效果，比如布局、配色等。

所有的这些因素，每个人在刚开始的时候，并不是都熟悉，但随着知识与经验的积累，会越来越好的。

为了更好的使用 `Manim`，我建立一个 github 仓库，来整理相关的资源，目前还是一个起始阶段，欢迎大家提供建议！

地址如下：

https://github.com/liyangbit/ManimLab

---

对我的文章感兴趣的朋友，可以关注我的微信公众号「**Python数据之道**」（ID：PyDataLab），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
