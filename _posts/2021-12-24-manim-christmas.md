---
layout: posts
title: "手把手教你用Python绘制圣诞树 - Manim实践"
teaser:
date: 2021-12-24
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

{% include alert info='手把手教你用Python绘制圣诞树' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# 手把手教你用Python绘制圣诞树 - Manim实践




## 写在前面

大家好，我是阳哥。

前几天，我跟大家介绍了视频制作工具 `Manim`，不少同学都觉得不错，想进一步学习。

- [用 python 制作高逼格的数学动画](https://mp.weixin.qq.com/s/Endbg9M1XRg2JFx2XpYr6A)


刚好圣诞节快要到了，我之前在视频号「Python数据之道」上发了圣诞树制作的视频。

今天，我来介绍下视频中的效果是怎么制作的。

## Manim 基础知识

### 如何运行 Manim

用 `Manim` 绘制图形，首先需要引入 `Manim` 库，然后将需要绘制的内容封装到一个 类(class) 里面。

```python
from manim import *
```

对于 编辑好的程序文件（例如`christmas-intro.py` 文件），需要在同一个文件夹下运行命令来运行程序，命令格式如下：

```python
manim -pql christmas-intro.py DemoSquare
```

上面的命令行中：

- `manim` 是运行程序的主要标志用语；
- `p` 表示程序运行后会进行预览（图片或视频）；
- `ql` 表示低质量（quality low）, 其他的选项有 `-ql`, `-qm`, `-qh`, `-qk`, 分别表示 低质量、正常质量、高质量、4K质量；
- christmas-intro.py 是py代码文件；
- DemoSquare 是 py代码文件中的一个类；

命令行中，还有其他许多参数可以设置，可以通过社区版的支持文档来进一步了解：

https://docs.manim.community/en/stable/tutorials/configuration.html#command-line-arguments

### 圣诞树中的元素

绘制圣诞树，有不少同学是用 Python 的 `Turtle` 包来绘制的，在这里我用 `Manim` 进行了绘制，效果如下：

[![好玩的Python：绘制圣诞树](https://tva1.sinaimg.cn/large/e6c9d24egy1gzu5gou6kej20ny0de753.jpg)](https://mp.weixin.qq.com/s/5zuQiY92eURu4H5Yt7grIw)

在视频中，一共绘制了 3 颗不同的圣诞树，这里以下面的这棵树为例，来介绍 `Manim` 的一些用法。

![](https://tva1.sinaimg.cn/large/008i3skNgy1gxo446i0mtg30nq0dchdv.gif)


对于圣诞树中的细分元素，通过观察，只涉及到正方形和圆形，以及填充不同的颜色。


### 绘制正方形

用 `Manim` 绘制正方形，`Manim` 提供了类对象 `Square` 来绘制，如下：

```python
class DemoSquare(Scene):
    def construct(self):
        # WaterMark.construct(self)
        r = 1
        sq = Square(
            side_length=2*r,
            color=GREEN_LEMON,
            )
        self.add(sq)
```

通过下面的命令来运行：

```python
manim -pql christmas-intro.py DemoSquare
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gxo693ud50g30s60kdtbe.gif)

绘制效果如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gxo5lh33ioj30nq0dcmx7.jpg)

如果需要对正方形进行填充，则可以设置参数 `fill_color` 和 `fill_opacity` ，代码和效果如下：

```python
class DemoSquare(Scene):
    def construct(self):
        # WaterMark.construct(self)
        r = 1
        sq = Square(
            side_length=2*r,
            color=GREEN_LEMON,
            fill_color=GREEN_LEMON, 
            fill_opacity=1,
            )
        self.add(sq)
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gxo5nz076kj30nq0dc74a.jpg)

### 绘制圆形

对于绘制圆形，与绘制正方形类似，`Manim` 提供了类对象 `Circle` 来绘制，如下：

```python
class DemoCircle(Scene):
    def construct(self):
        # WaterMark.construct(self)
        r = 1
        circle = Circle(
            radius=r,
            color=GREEN_LEMON,
            fill_color=GREEN_LEMON,
            fill_opacity=1,
        )
        self.play(Create(circle))
        self.wait()
```

运行的命令行如下：

```python
manim -pql --disable_caching christmas-intro.py DemoCircle --format=gif
```

其中：
- `--format=gif` 表示保存为 `.gif` ；
- `--disable_caching` 一般是视频帧数很多的时候，加上这个参数，可以加快渲染的速度，一般情况下，可以不加这个参数。

效果如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gxo6h42p9fg30nq0dctab.gif)

在代码中添加了 `play` 、`Create` 因此会有动态的效果了。

## 绘制圣诞树

对于这个圣诞树，在用 `Manim` 制作时，对于形状部分可以分为三个部分，如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gxo4zvckw7j31dz0u0n0o.jpg)

第 1 部分和 第 3 部分，相对而言要简单些，第2部分，看起来复杂些，但其实也是由三个类似的单元组合起来的。

![](https://tva1.sinaimg.cn/large/008i3skNgy1gxo6v3vfnzj31er0u041e.jpg)

### 绘制第2部分

对于第2部分中的每一个细分单元，可以用通过一个自定义函数来实现：

```python
def draw_tree(n, buff=1):
  shapes = Group()
  for i in range(4):
      sq_lst = [
          Square(
              side_length=2 * r,
              color=GREEN_LEMON,
              fill_color=GREEN_LEMON,
              fill_opacity=1,
          )
          for j in range(2 * (i + n) + 1)
      ]
      sq_group = Group(*sq_lst).arrange(RIGHT)
      sq_group.to_edge(UP, buff=buff)
      if i == 2:
          sq_side_lst = [
              Square(
                  side_length=2 * r,
                  color=YELLOW,
                  fill_color=YELLOW,
                  fill_opacity=1,
              )
              for j in range(2)
          ]
          sq_side_group = Group(*sq_side_lst)
          sq_side_group[0].next_to(sq_group, LEFT)
          sq_side_group[1].next_to(sq_group, RIGHT)
          self.add(sq_side_group[0])
          self.wait(time_wait)
          for sq in sq_group:
              self.add(sq)
              self.wait(time_wait)
          self.add(sq_side_group[1])
          self.wait(time_wait)
          shapes.add(sq_side_group[0])
          shapes.add(sq_group)
          shapes.add(sq_side_group[1])
      elif i == 3:
          circle_side_lst = [
              Circle(
                  radius=r,
                  color=RED_LEMON,
                  fill_color=RED_LEMON,
                  fill_opacity=1,
              )
              for j in range(2)
          ]
          circle_side_group = Group(*circle_side_lst)
          circle_side_group[0].next_to(sq_group, LEFT)
          circle_side_group[1].next_to(sq_group, RIGHT)
          self.add(circle_side_group[0])
          self.wait(time_wait)
          for sq in sq_group:
              self.add(sq)
              self.wait(time_wait)
          self.add(circle_side_group[1])
          self.wait(time_wait)
          shapes.add(circle_side_group[0])
          shapes.add(sq_group)
          shapes.add(circle_side_group[1])

      else:
          shapes.add(sq_group)
          for sq in sq_group:
              self.add(sq)
              self.wait(time_wait)

      buff += buff_gap
  return shapes
```

上面这个代码片段，看起来很长，其实逻辑还是不复杂的。主要是根据图形之间的布局关系来编写代码的。有兴趣的同学可以先自行研究下。

通过上面的这个自定义函数，咱们可以将第二部分绘制出来，关键代码如下：

```python
tree_caps = Group()
        for n in range(3):
            tree_cap = draw_tree(n, buff=buff_gap * 2 + buff_gap * n * 4)
            tree_caps.add(tree_cap)
```

### 绘制第3部分

第3部分，在代码中，我称之为 `tree_root` ，主要是绘制正方形组成的矩形方阵，然后控制视频演示的顺序。

代码如下：

```python
tree_root = Group()
  for i in range(3):
      sq_lst = [
          Square(
              side_length=2 * r,
              color=RED_LEMON,
              fill_color=RED_LEMON,
              fill_opacity=1,
          )
          for j in range(5)
      ]
      sq_group = Group(*sq_lst).arrange(RIGHT)
      tree_root.add(sq_group)
  for i in range(1, len(tree_root)):
      tree_root[i].next_to(tree_root[i - 1], DOWN, buff=buff_gap / 2)

  tree_root.next_to(tree_caps, DOWN, buff=buff_gap / 2)

  for i in range(len(tree_root)):
      for j in range(len(tree_root[i])):
          self.add(tree_root[i][j])
          self.wait(time_wait)
  self.wait()
```

### 缩小放大效果

对于图形的所辖和放大效果，在  `Manim` 中，可以通过 `animate` 以及 `scale` 的设置来实现：

```python
tree = Group(circle, tree_caps, tree_root)
self.play(tree.animate.move_to(ORIGIN).scale(0.5), run_time=4)
self.play(tree.animate.scale(2), run_time=4)
```

### 完整的代码

这颗树的完整代码，我已经封装在 `christmas-intro.py` 文件的 类 `Tree02` 中，大家可以运行下面的命令来运行代码：

```python
manim -pql christmas-intro.py Tree02
```

完整的代码文件 `christmas-intro.py`，大家可以在公众号「Python数据之道」回复 **manim** 来获取。

代码文件 `christmas-intro.py` 中，还包括 `Tree01` 和 `Tree03`，大家也可以运行看看。

对于代码运行过程中的一些问题，欢迎一起交流。

#### 相关阅读

- [用 python 制作高逼格的数学动画](https://mp.weixin.qq.com/s/Endbg9M1XRg2JFx2XpYr6A)

- **排序算法：** [汇总](https://mp.weixin.qq.com/s/ufViNQMGwx8KIX23x4rmOw) | [冒泡排序](https://mp.weixin.qq.com/s/qFuF6w00P6zPOkzDEoE4pA) | [选择排序](https://mp.weixin.qq.com/s/uJnamO-Pf_0smeIXbst0Qw) | [快速排序](https://mp.weixin.qq.com/s/cy5-M0aan8XO5K2CpEee0w) | [归并排序](https://mp.weixin.qq.com/s/o_-S9jHSBt_za3XK5kBW0A) | [堆排序](https://mp.weixin.qq.com/s/2dtKn4uNlhXsdhM1tGrD4w) | [插入排序](https://mp.weixin.qq.com/s/llgaW7HGJ4DRTRMkVX1N-A) | [希尔排序](https://mp.weixin.qq.com/s/14t7U61vW6xJDvhdoQbY_A) | [计数排序](https://mp.weixin.qq.com/s/cEVWRJ-xldYC9ztmBSpdhA) | [桶排序](https://mp.weixin.qq.com/s/mC7OobFV6GVNIkOA6PRKRg) | [基数排序](https://mp.weixin.qq.com/s/LFUUL7TlIr0csGwbl6fgOQ)

---

对我的文章感兴趣的朋友，可以关注我的微信公众号「**Python数据之道**」（ID：PyDataLab），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
