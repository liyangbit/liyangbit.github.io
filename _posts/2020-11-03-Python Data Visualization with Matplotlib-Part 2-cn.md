---
layout: posts
title: "100个案例，7万字Matplotlib 实操干货，带你从入门到进阶！"
teaser:
date: 2020-11-03
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PythonVisualization
tags:    
   - Matplotlib
comments: true
show_meta: true
sidebar: right
authors: ["阳哥"]
---

{% include alert info='100个案例，7万字Matplotlib 实操干货，带你从入门到进阶！' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# 100个案例，7万字Matplotlib 实操干货，带你从入门到进阶！

<!-- Matplotlib 入门到进阶，38个案例，3万字长文

30000字 Matplotlib 实操干货，38个案例带你从入门到进阶！

100个案例，7万字Matplotlib 实操干货，带你从入门到进阶！

太赞了！100个案例，Matplotlib 从入门到大神！ -->

译文出品：Python数据之道

原文作者：Rizky Maulana Nurhidayat

翻译：阳哥


本文是 Matplotlib 可视化的第二篇。在第一篇文章中，我们学习了如何生成和自定义散点图、折线图、直方图和条形图。 本文将继续使用 Matplotlib 在 Python 绘图中进行研究，涉及生成和自定义箱形图、小提琴图、饼图、极坐标图、地理投影、3D图和等高线图。

[part1:30000字 Matplotlib 实操干货，38个案例带你从入门到进阶！](http://liyangbit.com/pythonvisualization/Python-Data-Visualization-with-Matplotlib-Part-1/)

与第一篇文章中一样，我已经在 Matplotlib 中自定义了默认参数。这是我的绘画风格:

```python
import numpy as np
import matplotlib.pyplot as plt

plt.rcParams['text.usetex'] = True
plt.rcParams['font.size'] = 15
plt.rcParams['font.family'] = "serif"

tdir = 'in'
major = 5.0
minor = 3.0
plt.rcParams['xtick.direction'] = tdir
plt.rcParams['ytick.direction'] = tdir

plt.rcParams['xtick.major.size'] = major
plt.rcParams['xtick.minor.size'] = minor
plt.rcParams['ytick.major.size'] = major
plt.rcParams['ytick.minor.size'] = minor
```

## 01. 箱形图(Box plot)

你知道箱形图吗？ 维基百科将箱形图定义为一种通过四分位数以图形方式描绘数字数据组的方法。 它用于描述性统计。 您可以在 图1 中看到箱形图的示例。

![图1. 箱形图](https://tva1.sinaimg.cn/large/0081Kckwly1gkak6fj183j30k509ijrj.jpg)

通常，箱形图表示分布图。 它是由一个框体、晶须和离群值构成的。 在 图1 中，没有异常值。 在框体元素中，可以显示分布的中位数或均值。 我们可以在 图1 中看到中位数。该框体受 Q1（第一个四分位数）和 Q3（第三个四分位数）值的限制。 Q1 和 Q3 的差值称为四分位数（IQR）。 默认情况下，晶须显示分布的边界，最小值和最大值。

![图2. 箱形图](https://tva1.sinaimg.cn/large/0081Kckwly1gkak6t4377j30k208idg0.jpg)

在 图2 中，可以看到一个异常值。 箱形图如何检测离群值？

当其值小于 Q1-1.5 x IQR 或大于 Q3 + 1.5 x IQR 时，将在箱形图中检测到异常值。

在使用 Matplotlib 创建箱形图之前，将使用下面的代码生成模拟数据：

```python
N = 50
np.random.seed(100)
box1 = np.random.randn(N) * 2 + 1
```

要以箱形图的形式显示变量 box1 ，可以使用以下代码:

```python
plt.figure()
plt.boxplot(box1)
```

运行上面的代码时，可以在 图3 中看到结果:

![图3. 创建箱形图](https://tva1.sinaimg.cn/large/0081Kckwly1gkak6z6pe5j31700srt8q.jpg)

### 水平箱形图 (Horizontal box plot)

如果要水平更改箱形图的方向，则需要在 `plt.boxplot（）` 代码中应用以下参数。

```python
vert = False
```

水平箱形图如 图4 所示：

![图4. 水平箱形图](https://tva1.sinaimg.cn/large/0081Kckwly1gkak810wq4j315o0srt8p.jpg)

在下一个示例中，将通过更改随机数据来创建一个具有异常值的分布，如以下代码所示:

```python
N = 50
np.random.seed(140)
box1 = np.random.randn(N) * 2 + 1

plt.boxplot(box1, vert=False)
```

运行代码后，如 图5 所示：

![图5. 含异常值的水平箱形图](https://tva1.sinaimg.cn/large/0081Kckwly1gkak885qiqj31p10pm0st.jpg)

下面将展示 Matplotlib 如何通过计算 Q1–1.5 x IQR，Q3 + 1.5 x IQR 的值并使用以下代码对分布进行排序来检测离群值：

```python
q1 = np.quantile(box1, .25)
q3 = np.quantile(box1, .75)
iqr = q3-q1
lower_boundary = q1 - 1.5 * iqr
upper_boundary = q3 + 1.5 * iqr
sort = np.sort(box1)
```

为了可视化需要的值，可以运行以下代码：

```python
N = 50
np.random.seed(140)
box1 = np.random.randn(N) * 2 + 1

q1 = np.quantile(box1, .25)
q3 = np.quantile(box1, .75)
iqr = q3-q1
lower_boundary = q1 - 1.5 * iqr
upper_boundary = q3 + 1.5 * iqr
sort = np.sort(box1)

colors = ['#6b2983', '#5f82cb', '#00d6ff', '#e77ca3', '#93003a']

plt.figure(figsize=(9, 3.5))
plt.boxplot(box1, vert=False)
plt.scatter(box1, np.linspace(1.3, 1.3, box1.shape[0]), color = 'r', alpha = .4, label = 'random numbers')

plt.vlines(lower_boundary, ymin = 0.2, ymax=.65, alpha = .7, ls = '-.', color = 'k', label = 'Q1 - 1.5 x IQR')
plt.vlines(sort[0], ymin = 0.2, ymax = .65, ls = '-', lw = 1.25, color = colors[2], label = 'minimum')
plt.vlines(sort[-2], ymin = 0.2, ymax = .65, ls = '-', lw = 1.25, color = colors[3], label = 'maximum')
plt.vlines(sort[-1], ymin = 0.2, ymax = .65, ls = '-', lw = 1.25, color = colors[4], label = 'outliers')
plt.vlines(upper_boundary, ymin = 0.2, ymax=.65, alpha = .7, ls = '--', color = 'k', label = 'Q3 + 1.5 x IQR')

plt.annotate(np.round(lower_boundary, 2), xy = (lower_boundary, 0.7), ha = 'center', color = 'k')
plt.annotate(np.round(sort[0], 2), xy = (sort[0], 0.7), ha = 'center', color = colors[2])
plt.annotate(np.round(sort[-2], 2), xy = (sort[-2], 0.7), ha = 'center', color = colors[3])
plt.annotate(np.round(sort[-1], 2), xy = (sort[-1], 0.7), ha = 'center', color = colors[4])
plt.annotate(np.round(upper_boundary, 2), xy = (upper_boundary, 0.8), ha = 'center', color = 'k')

plt.legend(bbox_to_anchor = (1.0, 1.05))

plt.savefig('box4.png', dpi = 300, bbox_inches = 'tight', facecolor='w')
```

上述代码运行的结果如 图6 所示：

![图6. 箱形图中异常值阐述](https://tva1.sinaimg.cn/large/0081Kckwly1gkak8mjqoxj328m0pv0u5.jpg)

在 图6 中，可以看到在红点中生成的随机数。 下边界（Q1-1.5 x IQR）等于 -3.69，分布的最小值是 -2.41 。 因此，图6 中的左晶须极限为最小值 -2.41。 上限（Q3 + 1.5 x IQR）在 5.98 上，因此值 7.0 的数据被定义为离群值。 晶须右边的极限值将在 5.47 处结束，处在上限之前的最大值。

在下一个示例中，将显示四个分布的平均值。 需要使用以下代码为四个分布创建模拟数据：

```python
np.random.seed(1214)
data = [np.random.normal(0, std, 100) for std in range(10, 14)]
```

### 显示均值

数据将生成四个正态分布，其中 mu 的值为0，并且每个分布的 sigma 值不同（10、11、12和13）。 要在箱形图中显示均值，需要使用以下代码:

```python
plt.boxplot(data, showmeans=True)
```

如果运行上面的代码，将生成一个箱形图，如 图7-1 所示。绿色三角形代表每个箱形图的均值:

![图7-1. 箱形图的均值](https://tva1.sinaimg.cn/large/0081Kckwly1gkak8z1k9tj317u0sraad.jpg)

同样的，可以使用下面代码在水平方箱形图中显示均值：

```python
plt.boxplot(data, showmeans=True, vert = False)
```

运行代码，如 图7-2 所示：

![图7-2. 水平箱形图中显示均值](https://tva1.sinaimg.cn/large/0081Kckwly1gkak97t0vqj318b0u03yy.jpg)

可以通过添加以下参数来更改均值显示的符号：

```python
meanline=True
```

或使用以下参数隐藏框体形状：

```python
showbox=False
```

箱形图中的一些自定义设置如 图8 所示:

![图8. 箱形图中自定义设置](https://tva1.sinaimg.cn/large/0081Kckwly1gkak9ilfnkj31b20u0acw.jpg)

要更改框体的颜色可以使用以下设置进行操作。 首先，需要设置下面的参数，代码所示如下：

```python
patch_artist=True
```

然后，准备颜色，并使用以下代码将其应用到箱形图中：

```python
colors = ['royalblue', 'lightblue', 'lightgreen', 'pink']
for patch, color in zip(box['boxes'], colors):
    patch.set_facecolor(color)
```

以下是完整的代码：

```python
np.random.seed(123)
all_data = [np.random.normal(0, std, 100) for std in range(10, 14)]

box = plt.boxplot(all_data, notch=False, patch_artist=True)

colors = ['royalblue', 'lightblue', 'lightgreen', 'pink']
for patch, color in zip(box['boxes'], colors):
    patch.set_facecolor(color)

plt.ylim(-50, 50)
```

结果如 图9 所示：

![图9. 箱形图中自定义框体颜色](https://tva1.sinaimg.cn/large/0081Kckwly1gkak9zws2rj317u0srjrq.jpg)

### 激活缺口(Activate notch)

可以使用以下参数在箱形图中显示缺口，如 图10 所示：

```python
notch=True
```

![图10. 箱形图中显示缺口](https://tva1.sinaimg.cn/large/0081Kckwly1gkaka81u38j317u0srt9c.jpg)

生成 图10 的完整代码如下：

```python
np.random.seed(123)
all_data = [np.random.normal(0, std, 100) for std in range(10, 14)]

box = plt.boxplot(all_data, notch=True, patch_artist=True)

colors = ['royalblue', 'lightblue', 'lightgreen', 'pink']
for patch, color in zip(box['boxes'], colors):
    patch.set_facecolor(color)

plt.ylim(-50, 50)
```

## 02. 小提琴图(Violin plot) 

小提琴图与箱形图的可视化几乎相似，但未定义离群值。 要创建小提琴图，可以使用以下代码：

```python
N = 50
np.random.seed(140)
viol = np.random.randn(N) * 2 + 1

plt.figure()
plt.violinplot(viol)
```

以下代码将显示小提琴图，如 图11 所示:

![图11. 小提琴图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakal2d3cj31700srq3l.jpg)

水平的小提琴图，设置如下：

```python
vert=False
```

默认情况下，小提琴图未显示中位数和均值。 如果要显示它，则需要插入这些参数:

```python
showmeans=True, showmedians=True
```

如果我组合上面的参数并运行它，将显示如 图12 所示的小提琴图:

![图11. 小提琴图-显示均值和中位数](https://tva1.sinaimg.cn/large/0081Kckwly1gkakasywovj316z0f1dg7.jpg)

在 图12 中，估计不能区别中位数和平均值，因为它们具有相似的颜色。 要更改小提琴图中每个元素的颜色，例如条形的颜色，中位数，平均值，最小值和最大值，需要进行以下设置：

```
violin_parts = plt.violinplot(data, showmedians=True, 
                              showmeans=True)
```

下面生成四个不同的正态分布，并将其定义为数据变量。

要更改中位数、均值、条形、最小值和最大值的颜色，可以使用以下代码：

```python
vmedian = violin_parts['cmedians']
vmedian.set_edgecolor('r')

vmean = violin_parts['cmeans']
vmean.set_edgecolor('k')

vbars = violin_parts['cbars']
vbars.set_edgecolor('k')

vmax = violin_parts['cmaxes']
vmax.set_edgecolor('darkorange')

vmin = violin_parts['cmins']
vmin.set_edgecolor('darkorange')
```

自定义后的小提琴图如 图13 所示：

![图13. 自定义设置的小提琴图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakb0k5v2j31ek0u0q4o.jpg)

生成 图13 的完整代码如下：

```python
np.random.seed(1214)
data = [np.random.normal(0, std, 100) for std in range(10, 14)]

plt.figure(figsize = (10, 6))
violin_parts = plt.violinplot(data, showmedians=True, 
                              showmeans=True)

vbars = violin_parts['cbars']
vbars.set_edgecolor('k')

vmedian = violin_parts['cmedians']
vmedian.set_edgecolor('r')

vmean = violin_parts['cmeans']
vmean.set_edgecolor('k')

vmax = violin_parts['cmaxes']
vmax.set_edgecolor('darkorange')

vmin = violin_parts['cmins']
vmin.set_edgecolor('darkorange')
```

要更改轮廓颜色，需要使用以下代码：

```python
for vp in violin_parts['bodies']:
    vp.set_facecolor('orange')
    vp.set_edgecolor('k')
    vp.set_linewidth(3)
```

上面的代码会将主体颜色更改为橙色，边缘为黑色，并将线宽调整为 3 ，如 图14 所示:

![图14. 小提琴图-自定义设置轮廓](https://tva1.sinaimg.cn/large/0081Kckwly1gkakb90opcj31ek0u0n1m.jpg)

在 图14 中，仅显示每个分布的中值，可以使用以下代码生成 图14 ：

```python
np.random.seed(1214)
data = [np.random.normal(0, std, 100) for std in range(10, 14)]

plt.figure(figsize = (10, 6))
violin_parts = plt.violinplot(data, widths=0.9, showmedians=True)

for vp in violin_parts['bodies']:
    vp.set_facecolor('orange')
    vp.set_edgecolor('k')
    vp.set_linewidth(3)
```

高度自定义的小提琴图如 图15 所示：

![图15. 自定义小提琴图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakbiv3n6j31ek0u0n17.jpg)

生成 图15 的代码如下：

```python
np.random.seed(1214)
data = [np.random.normal(0, std, 100) for std in range(10, 14)]

# Create violin plot objects:
plt.figure(figsize=(10, 6))
violin_parts=plt.violinplot(data, showmedians=True, showmeans=True)

# Make the violin body blue with a red border:
for vp in violin_parts['bodies']:
    vp.set_facecolor('royalblue')
    vp.set_edgecolor('k')
    vp.set_linewidth(2)
    vp.set_alpha(.8)

colors = ['k', 'red', 'red', 'w', 'k']
part = ['cbars','cmins','cmaxes','cmeans','cmedians']

for i in range(len(part)):
    vp = violin_parts[part[i]]
    vp.set_edgecolor(colors[i])
    vp.set_linewidth(2)
```

## 03. 饼图（Pie chart）

在生成饼图之前，将使用以下代码创建一些数据：

```python
labels = 'Frogs', 'Hogs', 'Dogs', 'Logs'
numbers = [15, 30, 45, 10]
```

要在饼图中可视化，可以使用以下代码：

```python
plt.pie(numbers)
```

结果如 图16 所示：

![图16. 饼图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakbtf82hj30qt0qugm2.jpg)

图16 显示了一个简单的饼图, 可以使用以下参数为每个数据提供标签：

```python
labels = labels
```

如果要显示每个动物的数量，则需要插入以下参数：

```python
autopct='%1.1f%%'
```

在 图17 中添加标签和数字后，可以看到饼图的结果:

![图17. 饼图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakc0z9ndj30s70qu3zh.jpg)

以下是生成 图17 的完整代码：

```python
labels = 'Frogs', 'Hogs', 'Dogs', 'Logs'
sizes = [15, 30, 45, 10]

plt.figure()
plt.pie(sizes, labels = labels, autopct='%1.1f%%')
```

可以使用以下参数修改饼图的起始角度：

```python
startangle=90
```

默认起始角度为 0 ，应用于第一个数据（青蛙）。 下面以两种不同的角度自定义起始角度，如 图18 所示：

![图18. 饼图-自定义起始角度](https://tva1.sinaimg.cn/large/0081Kckwly1gkakc8q839j31p30u0416.jpg)

以下是生成 图18 的代码：

```python
labels = 'Frogs', 'Hogs', 'Dogs', 'Logs'
sizes = [15, 30, 45, 10]
explode1 = (0, 0.1, 0, 0)
explode2 = (0, 0, 0.1, 0)

plt.figure(figsize=(10, 10))
plt.subplot(121)
plt.pie(sizes, labels=labels, autopct='%1.1f%%', startangle=90)
plt.title('Startangle = 90$^\circ$')plt.subplot(122)
plt.pie(sizes, labels=labels, autopct='%1.1f%%', startangle=180)
plt.title('Startangle = 180$^\circ$')
```

还可以为特定的饼图设置边距，如 图19 所示:

![图18. 饼图-设置间距](https://tva1.sinaimg.cn/large/0081Kckwly1gkakcfrkx6j31ud0u0774.jpg)

以下是生成 图19 的代码：

```python
labels = 'Frogs', 'Hogs', 'Dogs', 'Logs'
sizes = [15, 30, 45, 10]
explode1 = (0, 0.1, 0, 0)
explode2 = (0, 0, 0.1, 0)

plt.figure(figsize=(10, 10))plt.subplot(121)
plt.pie(sizes, explode=explode1, labels=labels, 
        autopct='%1.1f%%', startangle=90)plt.subplot(122)
plt.pie(sizes, explode=explode2, labels=labels, 
        autopct='%1.1f%%', startangle=180)
```

您可以通过调整变量 `explode` 中的值来更改边距。

### 在饼图中应用不同的样式

您是否要以不同的样式在饼图中可视化数据？ 是的，您可以看到 图20 :

![图20. 饼图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakcmslvwj311t0u076f.jpg)

与之前的饼图的不同之处在于，这里将标签放置在图例中。 为此，需要使用以下参数：

```python
textprops={'color':"w"}
```

以及：

```python
plt.legend(labels, bbox_to_anchor = (1., .95),  title="Labels Name")
```

生成 图20 的代码如下：

```python
labels = 'Frogs', 'Hogs', 'Dogs', 'Logs'
sizes = [15, 30, 45, 10]
explode = (0, 0.05, 0, 0)

plt.figure(figsize=(7, 7))

plt.pie(sizes, autopct='%1.1f%%', textprops={'color':"w"}, 
        explode = explode, startangle = 90)
```

### 饼图中设置阴影

可以使用以下参数在饼图中添加阴影：

```python
shadow=True
```

结果如 图21 所示：

![图21. 饼图-添加阴影](https://tva1.sinaimg.cn/large/0081Kckwly1gkakcwky7fj311t0u0gon.jpg)

我想我知道你在想什么。 阴影很难看，对不对？ 为此我尝试了许多替代方法，如下：

1. 如果您使用的是 Jupyter，则可以通过右键单击该图来手动保存饼图。 然后，单击“将图像另存为”，如 图22 所示:

![图22. 饼图保存](https://tva1.sinaimg.cn/large/0081Kckwly1gkakda3jg2j30nk0h1q4w.jpg)

保存后的图片如 图23 所示：

![图23. 饼图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakdgn1bnj30dt0aymxh.jpg)

2. 另一种选择是将白色描边添加为前景。 要应用它，您需要使用以下代码从 Matplotlib 导入 `path_effects` 。

```python
import matplotlib.patheffects as path_effects
```

然后，为饼图定义变量名，如以下代码所示:

```python
patches, texts, autotexts = plt.pie(sizes, autopct='%1.1f%%', 
                                    textprops={'color':"w"}, 
                                    explode = explode, 
                                    startangle = 90, shadow=True)
```

接下来，自定义设置 `path_effects` ，如下：

```python
for patch in patches:
    patch.set_path_effects([path_effects.Stroke(linewidth=2.5, 
                                                foreground = 'w')])
```

以下是完整的代码：

```python
labels = 'Frogs', 'Hogs', 'Dogs', 'Logs'
sizes = [15, 30, 45, 10]
explode = (0, 0.05, 0, 0)

plt.figure(figsize=(7, 7), facecolor = 'w')

patches, texts, autotexts = plt.pie(sizes, autopct='%1.1f%%', 
                                    textprops={'color':"w"}, 
                                    explode = explode, 
                                    startangle = 90, shadow=True)

for patch in patches:
    patch.set_path_effects([path_effects.Stroke(linewidth=2.5, 
                                                foreground = 'w')])

plt.legend(labels, bbox_to_anchor = (1., .95),  title="Labels Name")
```

运行代码后，结果如 图24 所示：

![图24. 饼图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakdx7immj311t0u0gpg.jpg)

效果并不完美，还可以组合第一和第二种选择，并获得如 图25 所示的饼图:

![图25. 饼图](https://tva1.sinaimg.cn/large/0081Kckwly1gkake7n2xjj30dt0ayt95.jpg)

### 环形图（Doughnut chart）

在 Matplotlib 中，也可以绘制环形图，如 图26 所示：

![图26. 环形图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakeegczyj30u00u0q54.jpg)

生成 图26 的数据如下：

```python
size = 0.3
vals = np.array([[60.], [37.], [29.]])cmap = plt.get_cmap("tab20c")
outer_colors = cmap(np.arange(3)*4)
inner_colors = cmap(np.array([1, 2, 5, 6, 9, 10]))
```

环形图与饼图相似，但没有中心的完整半径。 在 图26 中，我将半径设置为 0.3，这是生成 图26 的代码。

```python
plt.figure(figsize=(10,10))

plt.pie(vals.sum(axis=1), radius=1, colors=outer_colors,
       wedgeprops=dict(width=size, edgecolor='w'))
```

我还创建了两个不同的半径大小，如 图27 所示：

![图27. 环形图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakem2otxj31ny0u0dhp.jpg)

生成 图27 的代码如下：

```python
vals = np.array([[60.], [37.], [29.]])

plt.figure(figsize=(10,10))plt.subplot(121)
plt.pie(vals.sum(axis=1), radius=1, colors=outer_colors,
       wedgeprops=dict(width=.3, edgecolor='w'))
plt.title('Size = 0.3')plt.subplot(122)
plt.pie(vals.sum(axis=1), radius=1, colors=outer_colors,
       wedgeprops=dict(width=.5, edgecolor='w'))
plt.title('Size = 0.5')
```

也可以生成双层的环形图，如 图28 所示：

![图28. 双层环形图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakeu3ogjj30u00u0tcg.jpg)

以下是生成 图28 的代码：

```python
plt.figure(figsize=(10, 10))

size = 0.3
vals = np.array([[60., 32.], [37., 40.], [29., 10.]])

cmap = plt.get_cmap("tab20c")
outer_colors = cmap(np.arange(3)*4)
inner_colors = cmap(np.array([1, 2, 5, 6, 9, 10]))

plt.pie(vals.sum(axis=1), radius=1, colors=outer_colors,
       wedgeprops=dict(width=size, edgecolor='w'))

plt.pie(vals.flatten(), radius=1-size, colors=inner_colors,
       wedgeprops=dict(width=size, edgecolor='w'))
```

如果您在 Matplotlib 官方文档上阅读了环形图的内容，您将看到一个不错的环形图，如 图29 所示：

![图29. 环形图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakf2lguoj31cs0u0di1.jpg)

生成 图29 的代码如下：

```python
fig, ax = plt.subplots(figsize=(10, 4.5), subplot_kw=dict(aspect="equal"))

recipe = ["225 g flour",
          "90 g sugar",
          "1 egg",
          "60 g butter",
          "100 ml milk",
          "1/2 package of yeast"]

data = [225, 90, 50, 60, 100, 5]

wedges, texts = ax.pie(data, wedgeprops=dict(width=0.5), startangle=-40)

bbox_props = dict(boxstyle="square,pad=0.3", fc="w", ec="k", lw=0.72)
kw = dict(arrowprops=dict(arrowstyle="-"),
          bbox=bbox_props, zorder=0, va="center")

for i, p in enumerate(wedges):
    ang = (p.theta2 - p.theta1)/2. + p.theta1
    y = np.sin(np.deg2rad(ang))
    x = np.cos(np.deg2rad(ang))
    horizontalalignment = {-1: "right", 1: "left"}[int(np.sign(x))]
    connectionstyle = "angle,angleA=0,angleB={}".format(ang)
    kw["arrowprops"].update({"connectionstyle": connectionstyle})
    ax.annotate(recipe[i], xy=(x, y), xytext=(1.35*np.sign(x), 1.4*y),
                horizontalalignment=horizontalalignment, **kw)

ax.set_title("Matplotlib bakery: A donut")
```

## 04. 极坐标图（Polar chart）

在平面投影中，将具有x轴和y轴。 在极坐标投影中，需要以半径和角度的形式定义它，如 图30 所示：

![图30. 极坐标图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakfd9og2j31rh0u0dl9.jpg)

在极坐标投影中，半径轴以圆半径的大小显示，并且以每个角度为 0 度的圆的角度作为起点投影角度。 要生成极坐标投影，需要将投影类型定义为极坐标，如以下参数所示：

```python
projection='polar'
```

以下是生成 图30 的代码：

```python
r = np.linspace(0, 2, 100)
theta = 2 * np.pi * r

fig = plt.figure(figsize=(13, 4))

ax1 = plt.subplot(121, projection='polar')
ax1.scatter(theta, r, label = 'Polar Projection', s = 10)
ax1.legend(bbox_to_anchor = (.85, 1.35))

ax2 = plt.subplot(122)
ax2.scatter(theta, r, label = 'Planar Projection', s = 10)
ax2.legend(bbox_to_anchor = (0.85, 1.35))
ax2.set_xlabel('R')
ax2.set_ylabel(r'$\theta$')
```

还可以在极坐标投影中以绘图线样式显示数据，如图31所示：

![图31. 极坐标图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakfn12omj31uu0u0dld.jpg)

以下是生成 图31 的代码：

```python
r = np.linspace(0, 2, 100)
theta = 2 * np.pi * r

fig = plt.figure(figsize=(13, 4))

ax1 = plt.subplot(121, projection='polar')
ax1.plot(theta, r, label = 'Polar Projection', ls = '--', color = 'r')
ax1.legend(bbox_to_anchor = (.85, 1.35))

ax2 = plt.subplot(122)
ax2.plot(theta, r, label = 'Planar Projection')
ax2.legend(bbox_to_anchor = (.85, 1.35))
```

接下来是在极坐标图中创建条形图，如 图32 所示：

![图32. 在极坐标图中创建条形图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakfw3nwqj30ue0u010p.jpg)

以下是生成 图32 的数据：

```python
np.random.seed(10130)

N = 20
theta = np.linspace(0.0, 2 * np.pi, N, endpoint=False)
radii = 10 * np.random.rand(N)
width = np.random.rand(N) * .8 - .1
colors = plt.cm.Spectral(radii / 10)
```

用以下代码来进行可视化：

```python
plt.figure(figsize=(7, 7))

ax = plt.subplot(111, projection='polar')
ax.bar(theta, radii, width=width, bottom=0.0,
       color=colors, alpha=0.5)
```

如果您对使用极坐标图生成 Matplotlib 徽标感兴趣，则可以阅读以下文档。

[https://matplotlib.org/3.1.0/gallery/misc/logos2.html](https://matplotlib.org/3.1.0/gallery/misc/logos2.html)

## 05. 地理投影（Geographic projection）

要可视化地理数据，可以使用 Matplotlib 提供的地理投影。 有四种类型的投影：`Aitoff`，`Hammer`，`Mollweide` 和 `Lambert` 投影。 为了理解它，我将使用以下代码创建一个模拟数据：

```python
N = 100

np.random.seed(157)
long = np.random.random(N) * 360 - 180
lat = np.random.random(N) * 180 - 90
```

To visualize it in the Aitoff projection, you can use this code

`Aitoff` 投影的代码如下：

```python
plt.figure(figsize=(12, 7))
plt.subplot(111, projection="aitoff")
plt.scatter(long, lat, marker = '*', color = 'red', s = 40)
plt.title("Aitoff")
plt.grid(True)
```

运行代码，结果如 图33 所示：

![图33. 地理投影-Aitoff](https://tva1.sinaimg.cn/large/0081Kckwly1gkakgh6smxj31ih0u0n73.jpg)

在`Aitoff` 投影中，需要确保数据以度为单位。

`Hammer` 投影的代码如下：

```python
plt.figure(figsize=(12, 7))
plt.subplot(111, projection="hammer")
plt.scatter(long, lat, marker = '*', color = 'red', s = 40)
plt.title("Hammer")
plt.grid(True)
```

`Hammer` 投影中所示的 long 和 lat 变量的单位是度。 上面的代码将生成一个图形，如 图34 所示:

![图34. 地理投影-Hammer](https://tva1.sinaimg.cn/large/0081Kckwly1gkakgr0i5sj31ih0u012d.jpg)

我不确定 `Aitoff` 和 `Hammer` 投影之间有什么区别。 如果需要一些说明，可以阅读这些链接:

`Aitoff` 投影

[https://en.wikipedia.org/wiki/Aitoff_projection](https://en.wikipedia.org/wiki/Aitoff_projection)

`Hammer` 投影

[https://en.wikipedia.org/wiki/Hammer_projection](https://en.wikipedia.org/wiki/Hammer_projection)

在 `Mollweide` 投影中，数据单位必须以弧度转换, 这是生成弧度数据的代码。

```python
N = 100

np.random.seed(157)
long = np.random.random(N) * 2 * np.pi  - np.pi
lat = np.random.random(N) * np.pi - (np.pi / 2)
```

`Hammer` 投影的代码如下：

```python
plt.figure(figsize=(12, 7))
plt.subplot(111, projection="mollweide")
plt.scatter(long, lat, marker = '*', color = 'red', s = 40)
plt.title("Mollweide")
plt.grid(True)
```

代码运行后，如 图35 所示：

![图35. 地理投影-Mollweide](https://tva1.sinaimg.cn/large/0081Kckwly1gkakh2ko2mj31kf0u07cr.jpg)

`Lambert` 投影的图示如 图36 所示：

![图35. 地理投影-Lambert](https://tva1.sinaimg.cn/large/0081Kckwly1gkakhgp457j30u00u6tj9.jpg)

生成 图36 的代码如下：

```python
N = 100

np.random.seed(157)
long = np.random.random(N) * 2 * np.pi  - np.pi
lat = np.random.random(N) * np.pi - (np.pi / 2)

plt.figure(figsize=(12, 12))
plt.subplot(111, projection="lambert")
plt.scatter(long, lat, marker = '*', color = 'red', s = 40)
plt.title("Lambert")
plt.grid(True)
```

## 06. 3D图（3D plot）

要创建 3D 图，需要将投影类型定义为 3d ，如以下参数所示:

```python
projection = '3d'
```

3D 投影将为您提供如 图37 所示的结果:

![图37. 3D图](https://tva1.sinaimg.cn/large/0081Kckwly1gkb2mxt7xtj30ui0u0gts.jpg)



上图的数据如下：

```python
N = 250

np.random.seed(124)
x = 15 * np.random.random(N)
y = np.sin(x) + 0.25 * np.random.random(N)
z = np.cos(x) + 0.25 * np.random.random(N)
```

生成 3D 图，可以用如下代码：

```python
plt.figure(figsize=(9, 6))
ax = plt.axes(projection = '3d')
ax.scatter3D(x, y, z, color = 'r')

ax.set_xlabel('x', fontsize = 20, labelpad = 20)
ax.set_ylabel('y', fontsize = 20, labelpad = 20)
ax.set_zlabel('z', fontsize = 20, labelpad = 20)
```

3D 图的结果如 图38 所示：

![图38. 3D图](https://tva1.sinaimg.cn/large/0081Kckwly1gkb2ng1ba5j30x30u0wou.jpg)

在最新的 Matplotlib 版本中，每个轴的长宽比始终相等。 要更改它，可以使用以下代码：

```python
ax.set_box_aspect((2., 1.5, 1.2))
```

将x轴，y轴和z轴的长宽比更改为 2：1.5：1.2 。 应用上面的代码后，将得到一个图，如 图39 所示:

![图39. 3D图](https://tva1.sinaimg.cn/large/0081Kckwly1gkaki5twmaj30zl0u0n6n.jpg)

### 3D线形图（3D plotline）

3D线形图如 图40 所示：

![图40. 3D图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakie572pj30xa0u0n3k.jpg)

生成 图40 的代码如下：

```python
N = 100

np.random.seed(124)
xline = np.linspace(0, 15, N)
yline = np.sin(xline) 
zline = np.cos(xline)

fig = plt.figure(figsize=(9, 6))
ax = plt.axes(projection = '3d')
ax.plot3D(xline, yline, zline)

ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')

ax.set_box_aspect((2, 1.5, 1.2))
```

可以使用以下参数来修改角度：

```python
ax.view_init(10, 180)
```

`view_init` 中的第一个参数是仰角，第二个参数是方位角，可以看到不同角度的不同表示形式，如 图41 所示：

![图41. 3D图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakiq4tuyj30us0u0wxp.jpg)

### 三角3D曲面（Triangular 3D surfaces）

要在 Matplotlib 中生成三角形3D曲面，可以使用以下代码:

```python
ax.plot_trisurf()
```

我使用以下代码生成要在三角形3D曲面中可视化的数据：

```python
N = 2000

np.random.seed(124)
r = 2 * np.pi * np.random.random(N)
theta = 20 * np.pi * np.random.random(N)

xdata = np.ravel(r * np.sin(theta))
ydata = np.ravel(r * np.cos(theta))
zdata = np.sin(xdata) + np.cos(ydata)
```

可以从上面的数据中看到三角形3D曲面图，见 图42:

![图42. 3D图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakj300azj31s30u0e1q.jpg)

生成 图42 的代码如下：

```python
N = 2000

np.random.seed(124)
r = 2 * np.pi * np.random.random(N)
theta = 20 * np.pi * np.random.random(N)

xdata = np.ravel(r * np.sin(theta))
ydata = np.ravel(r * np.cos(theta))
zdata = np.sin(xdata) + np.cos(ydata)

fig = plt.figure(figsize=(15, 6))
plt.subplots_adjust(wspace=0)

ax1 = plt.subplot(121, projection = '3d')
ax1.plot_trisurf(xdata, ydata, zdata, cmap = 'inferno')

ax1.set_xlabel('x')
ax1.set_ylabel('y')
ax1.set_zlabel('z')

ax1.view_init(40, 100)
ax1.set_box_aspect((4.5, 4.5, 1.5))
ax1.set_title('Elevation = 40$^\circ$, Azimuth = 100$^\circ$')

ax2 = plt.subplot(122, projection = '3d')
ax2.plot_trisurf(xdata, ydata, zdata, cmap = 'inferno')

ax2.set_xlabel('x')
ax2.set_ylabel('y')
ax2.set_zlabel('z')

ax2.view_init(20, 100)
ax2.set_box_aspect((4.5, 4.5, 1.5))
ax2.set_title('Elevation = 20$^\circ$, Azimuth = 100$^\circ$')
```

### 3D等高线图（3D contour plot） 

Matplotlib 提供 3D等高线图，可以使用下面的代码创建：

```python
ax.contour3D()
```

使用的数据如下：

```python
N = 100
np.random.seed(3124)
x = np.linspace(-2, 2, N) + np.random.random(N)
y = np.linspace(-2, 2, N) + np.random.random(N)

X, Y = np.meshgrid(x, y)
Z = np.sin(X) + np.cos(Y)
```

运行以下代码来进行可视化：

```python
fig = plt.figure(figsize=(9, 6))

ax = plt.axes(projection = '3d')
ax.contour3D(X, Y, Z, cmap = 'Spectral')
```

进行自定义后，3D等高线图如 图43 所示：

![图43. 3D等高线图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakjbv6zij30zu0u0q91.jpg)

生成 图43 的代码如下：

```python
N = 100
np.random.seed(3124)
x = np.linspace(-2, 2, N) + np.random.random(N)
y = np.linspace(-2, 2, N) + np.random.random(N)

X, Y = np.meshgrid(x, y)
Z = np.sin(X) + np.cos(Y)

fig = plt.figure(figsize=(9, 6))

ax = plt.axes(projection = '3d')
ax.contour3D(X, Y, Z, cmap = 'Spectral')

ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')

ax.set_box_aspect((2, 2, 1))
ax.view_init(60, 100)

ax.set_title('Contour counts = Default, elevation = 60, azimuth = 100')
```

默认情况下，Matplotlib 以 7 个计数的轮廓生成 3D轮廓图。 可以通过在 `ax.contour3D（）` 的第四个参数中写下所需的计数来更改它，如下面的代码所示：

```python
ax.contour3D(X, Y, Z, 256, cmap = 'Spectral')
```

图44 显示的是不一样的 3D等高线图：

![图44. 3D等高线图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakjmat4pj314j0u0qby.jpg)

图44的完整代码如下：

```python
N = 100
np.random.seed(3124)
x = np.linspace(-2, 2, N) + np.random.random(N)
y = np.linspace(-2, 2, N) + np.random.random(N)

X, Y = np.meshgrid(x, y)
Z = np.sin(X) + np.cos(Y)

fig = plt.figure(figsize=(20, 6))
ax = plt.axes(projection = '3d')
p = ax.contour3D(X, Y, Z, 256, cmap = 'Spectral')

ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')

ax.set_box_aspect((3, 3, 1))
ax.view_init(60, 100)

plt.colorbar(p)

ax.set_title('Contour counts = 256, elevation = 60, azimuth = 100')
```

如果要显示 `colorbar`，可以进行下面的设置：

```python
plt.colorbar()
```

所需的参数是您的3D等高线图。 因此，最好将绘图定义为变量。 在我的代码中，它被定义为变量 p 。

可以使用前面图中使用的相同代码来更改视角，如 图45 所示:

![图45. 3D等高线图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakjx26zbj313m0u0woo.jpg)

### 3D等高线图的一些 bugs

我认为 Matplotlib 3D等高线图中存在一个 bug ，如图46所示。

![图46. 3D等高线图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakk66uk1j313m0u0dk4.jpg)

要创建 图46，请在上一个代码（创建图45的代码）中更改仰角和方位角的值。如果仔细分析图46，将获得一个奇怪的功能，可以在 图47 中看到它。

![图47. 3D等高线图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakkfyy4lj313m0u00xj.jpg)

蓝线覆盖的区域不在正确的位置。 我们不应该看到它，因为它放在后面。 我不确定这是否是bug，但我认为需要 Matplotlib 对其进行修复。

为了更详细地了解它，我在 Matplotlib 中引入了两种不同的轮廓表示形式，`ax.contour（）` 和 `ax.contourf（）` ，如 图48 所示:

![图48. 3D等高线图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakkpgwxwj31u20u0wit.jpg)

`ax.contourf` 中的 contourf 表示填充轮廓。 因此，可以在 图48 中看到 `ax.contour（）` 和 `ax.contourf（）` 之间的区别。`ax.contour` 具有与 `ax.contour3D（）` 类似的默认轮廓计数，为 7 个计数。 但是，`ax.contourf（）` 的计数不同，为8。您可以使用此代码重现 图48 :

```python
N = 100
np.random.seed(3124)
x = np.linspace(-2, 2, N) + np.random.random(N)
y = np.linspace(-2, 2, N) + np.random.random(N)

X, Y = np.meshgrid(x, y)
Z = np.sin(X) + np.cos(Y)

plt.figure(figsize=(14, 6))

ax1 = plt.subplot(121, projection = '3d')
ax1.contour(X, Y, Z, cmap = 'Spectral')
ax1.set_xlabel('x')
ax1.set_ylabel('y')
ax1.set_zlabel('z')

ax1.set_box_aspect((3, 3, 1))
ax1.view_init(10, 100)
ax1.set_title('Contour Default, elevation = 10, azimuth = 100')ax2 = plt.subplot(122, projection = '3d')
ax2.contourf(X, Y, Z, cmap = 'Spectral')
ax2.set_xlabel('x')
ax2.set_ylabel('y')
ax2.set_zlabel('z')

ax2.set_box_aspect((3, 3, 1))
ax2.view_init(10, 100)
ax2.set_title('Contourf Default, elevation = 10, azimuth = 100')
```

可以在右侧面板中仔细观察 图48（轮廓图）。 在3D等高线图中提到的bug未检测到。 可以在 图49 中看到更多详细信息。

![图49. 3D等高线图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakl13czrj31xi0u0wke.jpg)

您可以看到 `ax.contour（）` 是否会给您同样的错误，但 `ax.contourf（）` 不会。 因此，如果要在3D投影中可视化等高线图，建议使用 `ax.contourf（）` 。

### 线框图 （Wireframe plot）

使用以下设置生成线框图：

```python
ax.plot_wireframe()
```

以下是生成 线框图 （Wireframe plot）的数据：

```python
N = 100
np.random.seed(3124)
x = np.linspace(-2, 2, N) + np.random.random(N)
y = np.linspace(-2, 2, N) + np.random.random(N)

X, Y = np.meshgrid(x, y)
Z = np.sin(X) + np.cos(Y)
```

默认的线框图 （Wireframe plot）如 图50 所示：

![图50. 线框图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakndizroj30u50u04ks.jpg)

生成 图50 的代码如下：

```python
N = 100
np.random.seed(3124)
x = np.linspace(-2, 2, N) + np.random.random(N)
y = np.linspace(-2, 2, N) + np.random.random(N)

X, Y = np.meshgrid(x, y)
Z = np.sin(X) + np.cos(Y)

fig = plt.figure(figsize=(10, 10))

ax = plt.axes(projection = '3d')
ax.plot_wireframe(X, Y, Z, color = 'k', alpha = .2)
```

线框图 （Wireframe plot）的自定义设置，如 图51 所示：

![图51. 线框图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakp1zc6aj30xi0u07fm.jpg)

我将线框图层的数量更改为 5 ，将仰角设置为 60 度，将方位角设置为 100 度。 您可以使用以下代码生成 图51 :

```python
N = 100
np.random.seed(3124)
x = np.linspace(-2, 2, N) + np.random.random(N)
y = np.linspace(-2, 2, N) + np.random.random(N)

X, Y = np.meshgrid(x, y)
Z = np.sin(X) + np.cos(Y)

fig = plt.figure(figsize=(9, 6))
ax = plt.axes(projection = '3d')

# 3d contour plot
ax.plot_wireframe(X, Y, Z, 5, color = 'k', alpha = .2)
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')

ax.set_box_aspect((2, 2, 1))
ax.view_init(60, 100)

ax.set_title('Wireframe counts = 5, elevation = 60, azimuth = 100')
```

如果要检查在 `ax.contour（）` 和 `ax.contour3D（）` 中是否遇到类似的错误，可以将仰角更改为 10 度，将方位角更改为 100 度，如图 52 所示:

![图52. 线框图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakpf54d7j30xq0u0gsx.jpg)

在线框图中，没有遇到 bugs 。

### 3D表面图 （3D surface plot） 

3D表面图 （3D surface plot）的代码设置如下：

```python
ax.plot_surface()
```

3D表面图 （3D surface plot）的自定义设置如 图53：

![图53. 3D表面图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakpprt5cj30u00v8qac.jpg)

生成 图53 的代码如下：

```python
N = 100
np.random.seed(3124)
x = np.linspace(-2, 2, N) + np.random.random(N)
y = np.linspace(-2, 2, N) + np.random.random(N)

X, Y = np.meshgrid(x, y)
Z = np.sin(X) + np.cos(Y)

fig = plt.figure(figsize=(8, 8))ax = plt.axes(projection = '3d')

# 3d contour plot
ax.plot_surface(X, Y, Z, )
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')

ax.set_box_aspect((2, 2, 1))
ax.view_init(10, 100)

ax.set_title('Plot surface Default, elevation = 10, azimuth = 100')
```

可以使用以下参数指定 cstride 和 rstride 的值：

```python
rstride = 1, cstride = 1
```

可以在图54中看到默认的 cstride 和 rstride 值与自定义值之间的差异。

![图54. 3D表面图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakq3vco3j31p60u07m2.jpg)

生成 图54 的代码如下：


```python
N = 200
np.random.seed(3124)
x = np.linspace(-2, 2, N) + np.random.random(N)
y = np.linspace(-2, 2, N) + np.random.random(N)

X, Y = np.meshgrid(x, y)
Z = np.sin(X) + np.cos(Y)

fig = plt.figure(figsize=(14, 6))

ax1 = plt.subplot(121, projection = '3d')# 3d contour plot
ax1.plot_surface(X, Y, Z,  cmap = 'Spectral')
ax1.set_xlabel('x')
ax1.set_ylabel('y')
ax1.set_zlabel('z')

ax1.set_box_aspect((2, 2, 1))
ax1.view_init(60, 100)

ax1.set_title('Plot surface rstride = cstride = default, \n elevation = 60, azimuth = 100')

ax2 = plt.subplot(122, projection = '3d')# 3d contour plot
ax2.plot_surface(X, Y, Z,  cmap = 'Spectral', rstride = 1, cstride = 1)
ax2.set_xlabel('x')
ax2.set_ylabel('y')
ax2.set_zlabel('z')

ax2.set_box_aspect((2, 2, 1))
ax2.view_init(60, 100)

ax2.set_title('Plot surface rstride = cstride = 1, \n elevation = 60, azimuth = 100')
```

您不会遇到等高线图中遇到的错误，如 图55 所示:

![图55. 3D表面图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakqebkznj312y0u0n2y.jpg)

### 在Matplotlib中创建球体

在Matplotlib中创建球体，如 图56 所示：

![图56. 创建球体](https://tva1.sinaimg.cn/large/0081Kckwly1gkakqt9zjqj30ud0u0h65.jpg)

图 56 的代码如下：

```python
u = np.linspace(0, 2 * np.pi, 100)
v = np.linspace(0, np.pi, 100)
x = 10 * np.outer(np.cos(u), np.sin(v))
y = 10 * np.outer(np.sin(u), np.sin(v))
z = 10 * np.outer(np.ones(np.size(u)), np.cos(v))

plt.figure(figsize=(10, 10))
ax = plt.subplot(projection = '3d')
ax.plot_surface(x, y, z, cmap = 'inferno')
```

可以自定义视角，如 图57 所示:

![图57. 创建球体](https://tva1.sinaimg.cn/large/0081Kckwly1gkakr7sabfj30u00u11di.jpg)

生成 图57 的代码如下：

```python
u = np.linspace(0, 2 * np.pi, 100)
v = np.linspace(0, np.pi, 100)
x = 10 * np.outer(np.cos(u), np.sin(v))
y = 10 * np.outer(np.sin(u), np.sin(v))
z = 10 * np.outer(np.ones(np.size(u)), np.cos(v))

rows = 2
columns = 2

grid = plt.GridSpec(rows, columns, wspace = .2, hspace = .2)

elev = np.arange(0, 40, 10)
azim = np.arange(0, 200, 50)

plt.figure(figsize=(12, 12))
for i in range(rows * columns):
    ax = plt.subplot(grid[i], projection = '3d')
    ax.plot_surface(x, y, z, cmap = 'inferno')
    ax.view_init(elev[i], azim[i])
    ax.set_title('Elevation = ' + str(elev[i]) + ', Azimuth = ' + str(azim[i]))
```

## 07. 2D等高线图（2D Contour plot）

绘制 2D等高线图，使用以下设置：

```python
plt.contour()
```

虽然名称是2D等高线图，但它显示了3D数据。 使用以下代码创建模拟数据：

```python
N = 100
np.random.seed(100)

x = np.linspace(-2, 2, N) + np.random.random(N)
y = np.linspace(-2, 2, N) + np.random.random(N)

X, Y = np.meshgrid(x, y)
Z = np.sin(X) + np.cos(Y)
```

将模拟数据呈现为等高线图，如 图58 所示:

![图58. 2D等高线图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakrv7yp5j314h0u0q8k.jpg)

生成 图58 的代码如下：

```python
N = 100
np.random.seed(100)

x = np.linspace(-2, 2, N) + np.random.random(N)
y = np.linspace(-2, 2, N) + np.random.random(N)

X, Y = np.meshgrid(x, y)
Z = np.sin(X) + np.cos(Y)

plt.figure(figsize=(7, 5))
plt.contour(X, Y, Z)
plt.title('Contour 2D Default', pad = 10)
```

我生成了两个自定义的等高线图，如 图59 所示:

![图59. 2D等高线图](https://tva1.sinaimg.cn/large/0081Kckwly1gkaksardcdj328s0u07o6.jpg)

生成 图59 的代码如下：

```python
N = 100
np.random.seed(100)

x = np.linspace(-2, 2, N) + np.random.random(N)
y = np.linspace(-2, 2, N) + np.random.random(N)

X, Y = np.meshgrid(x, y)
Z = np.sin(X) + np.cos(Y)

plt.figure(figsize=(15, 5))

plt.subplot(121)
plt.contour(X, Y, Z, 256)
plt.title('Contour 2D counts = 256, cmap = viridis', pad = 10)
plt.colorbar()

plt.subplot(122)
plt.contour(X, Y, Z, 256, cmap = 'Spectral')
plt.colorbar()
plt.title('Contour 2D counts = 256, cmap = Spectral', pad = 10)
```

另外，可以生成填充到2D投影中的等高线图，如 图60 所示：

![图60. 2D等高线图](https://tva1.sinaimg.cn/large/0081Kckwly1gkakskeqi1j313g0u0tar.jpg)

生成 图60 的代码如下：

```python
N = 100
np.random.seed(100)x = np.linspace(-2, 2, N) + np.random.random(N)
y = np.linspace(-2, 2, N) + np.random.random(N)

X, Y = np.meshgrid(x, y)
Z = np.sin(X) + np.cos(Y)**3

plt.figure(figsize=(7, 5))
plt.contourf(X, Y, Z, cmap = 'Spectral')
plt.colorbar()

plt.title('Contourf 2D Default', pad = 10)
```

在这里，生成了两个不同的 contourf 图，如 图61 所示:

![图61. 2D等高线图](https://tva1.sinaimg.cn/large/0081Kckwly1gkaksx5izdj328s0u0toj.jpg)

生成 图61 的代码如下：

```python
N = 100
np.random.seed(100)

x = np.linspace(-2, 2, N) + np.random.random(N)
y = np.linspace(-2, 2, N) + np.random.random(N)

X, Y = np.meshgrid(x, y)
Z = np.sin(X) + np.cos(Y)**3

plt.figure(figsize=(15, 5))

plt.subplot(121)
plt.contourf(X, Y, Z, 50, cmap = 'inferno')
plt.colorbar()

plt.title('Contourf 2D counts = 50', pad = 10)plt.subplot(122)
plt.contourf(X, Y, Z, 200, cmap = 'inferno')
plt.colorbar()

plt.title('Contourf 2D counts = 200', pad = 10)
```

## Conclusion 总结

与第一篇文章一起，这两篇文章展示了用 Matplotlib 进行可视化的 100 余个图形示例，这些示例分布在 11 种不同的样式图中：散点图、折线图、一维直方图、二维直方图、边际图、条形图、箱形图、小提琴图，饼图，极坐标图，地理投影，3D图和等高线图。大家可以通过这些案例，快速的掌握 Matplotlib 的绘图技巧，对于熟练使用 Matplotlib 有诸多益处。


原文链接：

https://towardsdatascience.com/python-data-visualization-with-matplotlib-part-2-66f1307d42fb

[part1:30000字 Matplotlib 实操干货，38个案例带你从入门到进阶！](http://liyangbit.com/pythonvisualization/Python-Data-Visualization-with-Matplotlib-Part-1/)

---

对我的文章感兴趣的朋友，可以关注我的微信公众号「**Python数据之道**」（ID：PyDataLab），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
