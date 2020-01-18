---
layout: posts
title: "干货推荐 | 掌握这几点，轻松玩转 Bokeh 可视化 （项目实战经验分享）"
teaser:
date: 2019-03-05
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PythonVisualization
tags:
   - Bokeh
   - recommend
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---

{% include alert info='摘要：

本文通过一个项目案例，详细的介绍了如何从 Bokeh 基础到构建 Bokeh 交互式应用程序的过程，内容循序渐进且具有很高的实用性。本文共有两万字左右，属于纯干货分享，强烈推荐大家阅读后续内容。

如果觉得内容不错，欢迎关注『Python数据之道』并将内容分享到您的朋友圈。' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

{% include alert success='

作者 : Will Koehrsen

翻译 : Lemon

译文出品 : Python数据之道 （公众号ID：PyDataLab）

' %}

作者 : Will Koehrsen

翻译 : Lemon

译文出品 : Python数据之道 （公众号ID：PyDataLab）

本文通过一个项目案例，详细的介绍了如何从 Bokeh 基础到构建 Bokeh 交互式应用程序的过程，内容循序渐进且具有很高的实用性。本文共有两万字左右，属于纯干货分享，强烈推荐大家阅读后续内容。

如果觉得内容不错，欢迎关注『Python数据之道』并将内容分享到您的朋友圈。

本文由以下几个大的部分组成：

1. Bokeh 基础介绍
2. 在 Bokeh 中添加主动交互功能
3. 在 Bokeh 中创建交互式可视化应用程序

Tips：

本文源代码地址，可以在公众号『Python数据之道』后台回复 “code” 来获取。

关于 Bokeh 基础的详细介绍，可以参考以下内容：
  
* [Bokeh入门](http://liyangbit.com/pythonvisualization/bokeh-start-circle/)
* [figure 详细解读](http://liyangbit.com/pythonvisualization/bokeh-figure/)
* [Bokeh基础可视化图形](http://liyangbit.com/pythonvisualization/bokeh-glyphs-1st/)
* [数据类型简介： ColumnDataSource](http://liyangbit.com/pythonvisualization/bokeh-data-01/)
* [数据的添加、修改和筛选](http://liyangbit.com/pythonvisualization/bokeh-data-02/)
* [图形与组件的布局简介](http://liyangbit.com/pythonvisualization/Bokeh-Laying-out-Plots-and-Widgets/)


可用于数据科学的资源正在迅速发展，这在可视化领域尤其明显，似乎每周都有另一种选择。 随着所有这些进步，有一个共同的趋势：增加交互性。 人们喜欢在静态图中查看数据，但他们更喜欢的是使用数据来查看更改参数如何影响结果。 关于我的研究，一份报告告诉建筑物所有者他们可以通过改变他们的空调（AC）使用计划表节省多少电力是很好的，但是给他们一个交互式图表更有效，他们可以选择不同的使用计划表，看看他们的选择如何影响用电量。 最近，受到互动图的趋势和不断学习新工具的渴望的启发，我一直在使用 Bokeh，一个 Python 库。 我为我的研究项目构建的仪表板中显示了 Bokeh 交互功能的一个示例，如下：

![image1-能耗项目示例](/images/posts/003-bokeh-data-visualization/1.gif)

虽然我不能分享这个项目背后的代码，但我可以通过一个使用公开数据构建完全交互式 Bokeh 应用程序的例子。 本文将介绍使用 Bokeh 创建应用程序的整个过程。 首先，我们将介绍 Bokeh 的基础内容， 我们将使用 `nycflights13` 数据集，该数据集记录了 2013年超过 300,000 个航班。首先，我们将专注于可视化单个变量，在这种情况下，航班的到达延迟时间为几分钟，我们将从构造基本直方图开始。

## Bokeh 基础

Bokeh 的主要概念是图形一次构建一层。 我们首先创建一个图形(figure)，然后在图形中添加称为 [图形符号(glyphs)](http://liyangbit.com/pythonvisualization/bokeh-glyphs-1st/) 的元素。 glyphs 可以根据所需的用途呈现多种形状：圆形(circles)，线条(lines) ，补丁(patches)，条形(bars)，弧形(arcs)等。 让我们通过制作带有正方形和圆形的基本图表来说明 glyphs 的概念。 首先，我们使用 `figure` 方法创建一个图，然后通过调用适当的方法并传入数据将我们的 glyphs 附加到 figure 中。 最后，我们展示了所做的图表。

```python
# bokeh basics
from bokeh.plotting import figure
from bokeh.io import show, output_notebook

# Create a blank figure with labels
p = figure(plot_width = 600, plot_height = 600,
           title = 'Example Glyphs',
           x_axis_label = 'X', y_axis_label = 'Y')

# Example data
squares_x = [1, 3, 4, 5, 8]
squares_y = [8, 7, 3, 1, 10]
circles_x = [9, 12, 4, 3, 15]
circles_y = [8, 4, 11, 6, 10]

# Add squares glyph
p.square(squares_x, squares_y, size = 12, color = 'navy', alpha = 0.6)
# Add circle glyph
p.circle(circles_x, circles_y, size = 12, color = 'red')

# Set to output the plot in the notebook
output_notebook()
# Show the plot
show(p)
```

图示如下：

![image2-Bokeh基础图](/images/posts/003-bokeh-data-visualization/2.png)

现在让我们开始展示航班延误数据，在进入图表之前，应该加载数据并对其进行简要检查：

```python
# Read the data from a csv into a dataframe
flights = pd.read_csv('../data/flights.csv', index_col=0)
# Summary stats for the column of interest
flights['arr_delay'].describe()

out[]:
count    327346.000000
mean          6.895377
std          44.633292
min         -86.000000
25%         -17.000000
50%          -5.000000
75%          14.000000
max        1272.000000

```

上述统计数据提供了可以用来决策的信息：共有 327,346 次航班，最短延误时间为-86 分钟（意味着航班提前 86 分钟），最长延迟时间为 1272 分钟，惊人的 21 小时！ 75％ 的分位数仅在 14 分钟，因此我们可以假设超过 1000 分钟的数字可能是异常值（这并不意味着它们是非法的，只是极端的）。 下面将重点关注直方图的 -60 分钟到 +120 分钟之间的延迟。

直方图是单个变量的初始可视化的常见选择，因为它显示了数据的分布。 x 位置是被称为区间（bins）的变量的值，并且每个柱子的高度表示每个区间中的数据点的计数（数量）。 在我们的例子中，x 位置将代表以分钟为单位的到达延迟，高度是相应 bin 中的航班数量。 Bokeh 没有内置的直方图，但是我们可以使用 `quad` 来制作我们自己的直方图。

为条形图（bars）创建数据，我们将使用 Numpy 的 `histogram` 函数来计算每个指定 bin 中的数据点数。 我们将使用 5 分钟长度的时间间隔（bins），这意味着该功能将计算每五分钟延迟间隔的航班数量。 生成数据后，我们将其放在 Pandas 的 dataframe 中，以将所有数据保存在一个对象中。

```python
"""Bins will be five minutes in width, so the number of bins
is (length of interval / 5). Limit delays to [-60, +120] minutes using the range."""
arr_hist, edges = np.histogram(flights['arr_delay'],
                               bins = int(180/5),
                               range = [-60, 120])
# Put the information in a dataframe
delays = pd.DataFrame({'arr_delay': arr_hist,
                       'left': edges[:-1],
                       'right': edges[1:]})

```

数据如下：

![image3-flights数据](/images/posts/003-bokeh-data-visualization/3.png)

`flights` 列是从 `left` 到 `right` 的每个延迟间隔内的航班数量。 从这里开始，我们可以创建一个新的 Bokeh 图形，并添加一个指定适当参数的 `quad`：

```python
# Create the blank plot
p = figure(plot_height = 600, plot_width = 600,
           title = 'Histogram of Arrival Delays',
           x_axis_label = 'Delay (min)]',
           y_axis_label = 'Number of Flights')

# Add a quad glyph
p.quad(bottom=0, top=delays['flights'],
       left=delays['left'], right=delays['right'],
       fill_color='red', line_color='black')

# Show the plot
show(p)
```

![image4-Bokeh绘制直方图](/images/posts/003-bokeh-data-visualization/4.png)

从上述图表来看，我们看到到达延迟几乎正态分布，右侧有轻微的正偏斜或重尾。

当然，其实有更简单的方法可以在 Python 中创建基本直方图，比如可以使用几行 matplotlib 代码完成相同的结果。 但是，我们想在 Bokeh 图中添加直方图并进行交互演示。

### 增加交互性

本文介绍的第一种交互方式是被动交互。 这些是读者可以采取的不会改变所显示数据的动作。 这些被称为检查员（inspectors），因为它们允许读者更详细地 “查看” 数据。 一个有用的检查器是当用户将鼠标悬停在数据点上时出现的提示工具，在 Bokeh 中称为 HoverTool 。

![image5-HoverTool](/images/posts/003-bokeh-data-visualization/5.png)

为了添加提示工具(tooltips)，我们需要将数据源从 dataframe 更改为 `ColumnDataSource` （CDS），这是 Bokeh 中的一个关键概念。 CDS 是一个专门用于绘图的对象，包括数据以及多个方法和属性。 CDS 允许我们为图形添加注释和交互性，并且可以从pandas 的 dataframe 构建。 实际数据本身保存在可通过 CDS 的 data 属性访问的字典中。 在这里，我们从 dataframe 创建源代码，并查看数据字典中与 dataframe 列对应的键。

```python
# Import the ColumnDataSource class
from bokeh.models import ColumnDataSource
# Convert dataframe to column data source
src = ColumnDataSource(delays)
src.data.keys()

out:
dict_keys(['flights', 'left', 'right', 'index'])

```

当我们使用 CDS 添加 glyphs 时，我们传入 CDS 作为 source 参数并使用字符串引用列名：

```python
# Add a quad glyph with source this time
p.quad(source = src, bottom=0, top='flights',
       left='left', right='right',
       fill_color='red', line_color='black')

```

注意代码如何通过单个字符串而不是之前的 `df ['column']` 格式引用特定数据列，例如'flights'，'left' 和 'right'。

### Bokeh 中的 HoverTool

HoverTool 的语法起初可能看起来有些复杂，但通过练习它们很容易创建。 我们将 `HoverTool` 实例作为 Python 元组的 “tooltips” 列表传递，其中第一个元素是数据的标签，第二个元素引用我们想要突出显示的特定数据。 我们可以使用 `$` 引用图表的任一属性，例如 x 或 y 位置，或使用 `@` 引用我们数据源中的特定字段。 这可能听起来有点令人困惑，所以这里有一个 HoverTool 的例子：

```python
# Hover tool referring to our own data field using @ and
# a position on the graph using $
h = HoverTool(tooltips = [('Delay Interval Left ', '@left'),
                          ('(x,y)', '($x, $y)')])
```

在这里，我们使用 `@` 引用 ColumnDataSource 中的 `left` 数据字段（对应于原始 dataframe 的 'left' 列），并使用 `$` 引用光标的（x，y）位置。 结果如下：

![image06-HoverTool](/images/posts/003-bokeh-data-visualization/6.png)

（x，y）位置是图表上鼠标的位置，对我们的直方图不是很有帮助，因为我们要找到给定条形中对应于条形顶部的航班数量。 为了解决这个问题，我们将改变我们的 tooltip 实例以引用正确的列。 格式化提示工具中显示的数据可能令人沮丧，因此我通常在 dataframe 中使用正确的格式创建另一列。 例如，如果我希望我的提示工具显示给定栏的整个间隔，我在 dataframe 中创建一个格式化的列：

```python
# Add a column showing the extent of each interval
delays['f_interval'] = ['%d to %d minutes' % (left, right) for left, right in zip(delays['left'], delays['right'])]

```

然后，我将此 dataframe 转换为 [ColumnDataSource](http://liyangbit.com/pythonvisualization/bokeh-data-01/) 并在我的 HoverTool 调用中访问此列。 下面的代码使用悬停工具创建绘图，引用两个格式化的列并将工具添加到绘图中：

```python
# Create the blank plot
p = figure(plot_height = 600, plot_width = 600,
           title = 'Histogram of Arrival Delays',
           x_axis_label = 'Delay (min)]',
           y_axis_label = 'Number of Flights')

# Add a quad glyph with source this time
p.quad(bottom=0, top='flights', left='left', right='right', source=src,
       fill_color='red', line_color='black', fill_alpha = 0.75,
       hover_fill_alpha = 1.0, hover_fill_color = 'navy')

# Add a hover tool referring to the formatted columns
hover = HoverTool(tooltips = [('Delay', '@f_interval'),
                             ('Num of Flights', '@f_flights')])

# Style the plot
p = style(p)

# Add the hover tool to the graph
p.add_tools(hover)

# Show the plot
show(p)
```

在 Bokeh 样式中，通过将元素添加到原始图形中来包含元素。 注意在 `p.quad` 调用中，还有一些额外的参数，`hover_fill_alpha` 和 `hover_fill_color`，当将鼠标悬停在条形图上时会改变 glyph 的外观。 我还使用 `style` 函数添加了样式。 当使用样式时，我会保持简单并专注于标签的可读性。 图的主要观点是显示数据，添加不必要的元素只会减少图形的用处！ 最终的图形如下：

![image07-带HoverTool的直方图](/images/posts/003-bokeh-data-visualization/7.png)

当将鼠标悬停在不同的栏上时，会得到该栏的精确统计数据，显示该区间内的间隔和航班数。 如果我们为图形感到自豪，可以将其保存到html文件中进行分享：

```python
# Import savings function
from bokeh.io import output_file
# Specify the output file and save
output_file('hist.html')
show(p)

```

上面这张图完成了工作，但它不是很吸引人！ 读者可以看到航班延误的分布接近正态分布（略有正偏斜），但他们没有理由再花费更多的时间来分析该图。

如果想要创建更具吸引力的可视化图表，我们可以允许用户通过交互自己来探索数据。 例如，在直方图中，一个有价值的特征是能够选择特定航空公司进行比较，或者选择更改 bins 的宽度以更精细地检查数据。 幸运的是，这些都是可以使用 Bokeh 在现有绘图之上添加的功能。 直方图的初始开发可能似乎涉及一个简单的绘图，但现在我们看到使用像 Bokeh 这样强大的库的回报！

## 在 Bokeh 中添加主动交互

Bokeh中有两类交互：被动交互和主动交互。 前面介绍的被动交互也称为检查器（inspectors），因为它们允许用户更详细地查阅图表中的信息，但不会更改显示的信息。 一个示例是当用户将鼠标悬停在数据点上时显示的提示信息，如下：

![image10-被动交互](/images/posts/003-bokeh-data-visualization/10.png)

第二类交互称为主动交互，因为它会更改绘图上显示的实际数据。 这可以是从选择数据子集（例如特定航空公司）到改变多项式回归拟合自由度的任何事情。 Bokeh 中有多种类型的主动交互，但在这里我们将重点关注所谓的“小部件”（“widgets”），可以点击的元素，并让用户控制图形的某些方面。

![image11-主动交互](/images/posts/003-bokeh-data-visualization/11.png)

当查看图表时，我喜欢使用主动交互，因为它们允许我自己探索数据。 我发现从我自己的数据（来自设计师的某个方向）而不是从完全静态的图表中发现数据的结论更具洞察力。 此外，为用户提供一定的自由度使他们能够略微不同的解释，从而产生有关数据集的有益讨论。

### 主动互动的实现方法

一旦我们开始添加主动交互，我们需要超越单行代码并进入封装特定操作的函数。 对于 Bokeh 小部件（widgets）交互，有三个主要功能要实现：

* make_dataset(): 按特定格式整理要显示的特定数据
* make_plot(): 使用指定的数据绘图
* update(): 根据用户选择更新绘图

#### 整理数据

在制作绘图之前，需要设计将要显示的数据。 对于交互式直方图，将为用户提供三个可控参数：

1. 航空公司 （在代码中称为 carriers）
2. 延迟的时间范围，比如： -60 至 +120 分钟
3. 直方图的宽度（即 bin 大小），默认值为 5 分钟

对于为绘图创建数据集的函数，我们需要允许指定每个参数。 为了告知我们如何在`make_dataset` 函数中转换数据，我们可以加载所有相关数据并进行检查。

![image12-加载数据](/images/posts/003-bokeh-data-visualization/12.png)

在此数据集中，每行是一个单独的航班。 `arr_delay` 列是以分钟为单位的航班到达延迟（负数表示航班早到）。 从前面的描述中我们知道有 327,236 个航班，最小延迟为 -86 分钟，最大延迟为 +1272 分钟。 在 `make_dataset` 函数中，我们希望根据 dataframe 中的 `name` 列选择航空公司，并通过 `arr_delay` 列限制航班数量。

为了生成直方图的数据，我们使用 numpy 中的 `histogram` 函数来计算每个bin中的数据点数。在示例中，这是每个指定延迟间隔内的航班数量。 在前面内容中，为所有航班制作了直方图，但现在我们将针对每个航空公司进行。 由于每个航空公司的航班数量差异很大，我们可以按比例显示延迟，而不是原始计数。 也就是说，图上的高度表示的是，在相应的 bin 区间，特定航空公司中该航班相对应于所有航班的延迟比例。 为了从计数到比例，我们将计数除以该航空公司的航班总数。

下面是制作数据集的完整代码，该函数接收我们想要包括的航空公司列表，要绘制的最小和最大延迟，以及以分钟为单位的指定 bin 宽度。

```python
def make_dataset(carrier_list, range_start = -60, range_end = 120, bin_width = 5):

    # Check to make sure the start is less than the end!
    assert range_start < range_end, "Start must be less than end!"
    by_carrier = pd.DataFrame(columns=['proportion', 'left', 'right',
                                       'f_proportion', 'f_interval',
                                       'name', 'color'])
    range_extent = range_end - range_start
    # Iterate through all the carriers
    for i, carrier_name in enumerate(carrier_list):

        # Subset to the carrier
        subset = flights[flights['name'] == carrier_name]

        # Create a histogram with specified bins and range
        arr_hist, edges = np.histogram(subset['arr_delay'],
                                       bins = int(range_extent / bin_width),
                                       range = [range_start, range_end])

        # Divide the counts by the total to get a proportion and create df
        arr_df = pd.DataFrame({'proportion': arr_hist / np.sum(arr_hist),
                               'left': edges[:-1], 'right': edges[1:] })

        # Format the proportion
        arr_df['f_proportion'] = ['%0.5f' % proportion for proportion in arr_df['proportion']]

        # Format the interval
        arr_df['f_interval'] = ['%d to %d minutes' % (left, right) for left,
                                right in zip(arr_df['left'], arr_df['right'])]

        # Assign the carrier for labels
        arr_df['name'] = carrier_name

        # Color each carrier differently
        arr_df['color'] = Category20_16[i]

        # Add to the overall dataframe
        by_carrier = by_carrier.append(arr_df)

    # Overall dataframe
    by_carrier = by_carrier.sort_values(['name', 'left'])  
    # Convert dataframe to column data source
    return ColumnDataSource(by_carrier)

```

上述运行结果如下：

![image13-整理好的数据](/images/posts/003-bokeh-data-visualization/13.png)

提醒一下，我们使用 Bokeh 中 `quad` 函数来制作直方图，因此我们需要提供该图形符号的左、右和顶部（底部将固定为0）参数。 它们分别位于 “left”，“right” 和 “proportion” 列中。 color 列为每个显示的航空公司提供了唯一的颜色，`f_` 列为 tooltips 提供了格式化文本。

下一个要实现的功能是 `make_plot` 。 该函数应该采用 ColumnDataSource（Bokeh中用于绘图的特定类型的对象）并返回绘图对象：

```python
def make_plot(src):
        # Blank plot with correct labels
        p = figure(plot_width = 700, plot_height = 700,
                  title = 'Histogram of Arrival Delays by Carrier',
                  x_axis_label = 'Delay (min)', y_axis_label = 'Proportion')

        # Quad glyphs to create a histogram
        p.quad(source = src, bottom = 0, top = 'proportion', left = 'left', right = 'right',
               color = 'color', fill_alpha = 0.7, hover_fill_color = 'color', legend = 'name',
               hover_fill_alpha = 1.0, line_color = 'black')

        # Hover tool with vline mode
        hover = HoverTool(tooltips=[('Carrier', '@name'),
                                    ('Delay', '@f_interval'),
                                    ('Proportion', '@f_proportion')],
                          mode='vline')

        p.add_tools(hover)

        # Styling
        p = style(p)

        return p

```

如果我们导入所有航空公司的数据，绘制的图形如下：

![image14-所有航线的延迟图](/images/posts/003-bokeh-data-visualization/14.png)

这个直方图非常混乱，因为有 16 家航空公司在同一图表上绘制！ 如果想比较航空公司，由于信息重叠，这几乎是不可能的。 幸运的是，我们可以添加小部件（widgets）以使绘图更清晰并实现快速比较。

#### 创建交互的小部件

一旦我们在 Bokeh 中创建基本图形，通过窗口小部件添加交互相对简单。 我们想要的第一个小部件是一个选择框，允许读者选择要显示的航空公司。 该控件将是一个复选框，允许根据需要进行尽可能多的选择，并在 Bokeh 中称为 “CheckboxGroup” 。 为了制作选择工具，我们导入 `CheckboxGroup` 类并使用两个参数来创建一个实例：`labels` 是想要在每个框旁边显示的值和 `active`：初始选择的值。 以下是包括所有航空公司的 `CheckboxGroup` 的代码。

```python
from bokeh.models.widgets import CheckboxGroup
# Create the checkbox selection element, available carriers is a  
# list of all airlines in the data
carrier_selection = CheckboxGroup(labels=available_carriers,
                                  active = [0, 1])
```

![image15-CheckboxGroup](/images/posts/003-bokeh-data-visualization/15.png)

Bokeh 复选框中的标签必须是字符串，而活动值是整数。 这意味着在图形中 'AirTran Airways Corporation' 对应数字 0 ，'Alaska Airlines Inc.' 对应数值 1。 当想要将所选复选框与航空公司匹配时，需要确保查找与所选整数活动值关联的字符串名称。 我们可以使用小部件的 `.labels` 和 `.active` 属性来做到这一点：

```python
# Select the airlines names from the selection values
[carrier_selection.labels[i] for i in carrier_selection.active]


out:
['AirTran Airways Corporation', 'Alaska Airlines Inc.']

```

制作复选的小部件后，需要将选定的航空公司复选框链接到图表上显示的信息。 这是使用 CheckboxGroup 的 `.on_change` 方法和我们定义的 `update` 函数完成的。 update 函数总是有三个参数：`attr` ，`old`，`new` 并根据选择控件更新绘图。 我们更改图表上显示的数据的方法是改变我们传递给 `make_plot` 函数中的 glyph(s) 的数据源。 这可能听起来有点抽象，所以这里是有一个 `update` 函数的例子，它改变了直方图以显示所选的航空公司：

```python
# Update function takes three default parameters
def update(attr, old, new):
    # Get the list of carriers for the graph
    carriers_to_plot = [carrier_selection.labels[i] for i in carrier_selection.active]
    # Make a new dataset based on the selected carriers and the
    # make_dataset function defined earlier
    new_src = make_dataset(carriers_to_plot,
                           range_start = -60,
                           range_end = 120,
                           bin_width = 5)
    # Update the source used in the quad glpyhs
    src.data.update(new_src.data)
```

在这里，我们将检查基于 CheckboxGroup 中所选航空公司显示的航空公司列表。 此列表将传递给 `make_dataset` 函数，该函数返回一个新的列数据源。 我们通过调用 `src.data.update`  并从新数据源传入数据来更新 glyphs 中使用的源的数据。 最后，为了将 `carrier_selection` 小部件中的更改链接到 `update` 函数，我们必须使用 `.on_change` 方法（称为事件处理程序）。

```python
# Link a change in selected buttons to the update function
carrier_selection.on_change('active', update)

```

只要选择或取消选择不同的航空公司，就会调用更新功能。 最终结果是在直方图上仅绘制了与所选航空公司相对应的图形 ，如下所示：

![image16-交互图](/images/posts/003-bokeh-data-visualization/16.gif)

### 更多的交互式控制

现在我们知道了创建控件的基本工作流程，可以添加更多元素。 每次，我们创建窗口小部件，编写更新函数以更改绘图上显示的数据，并使用事件处理程序将更新功能链接到窗口小部件。 我们甚至可以通过重写函数来从多个元素中使用相同的更新函数，以从小部件中提取需要的值。 为了练习，我们将添加两个额外的控件：一个 Slider，用于选择直方图的 bin 宽度;一个 RangeSlider，用于设置要显示的最小和最大延迟。 以下是制作这些小部件和新的 `update` 函数的代码：

```python
# Slider to select the binwidth, value is selected number
binwidth_select = Slider(start = 1, end = 30,
                     step = 1, value = 5,
                     title = 'Delay Width (min)')
# Update the plot when the value is changed
binwidth_select.on_change('value', update)

# RangeSlider to change the maximum and minimum values on histogram
range_select = RangeSlider(start = -60, end = 180, value = (-60, 120),
                           step = 5, title = 'Delay Range (min)')

# Update the plot when the value is changed
range_select.on_change('value', update)


# Update function that accounts for all 3 controls
def update(attr, old, new):

    # Find the selected carriers
    carriers_to_plot = [carrier_selection.labels[i] for i in carrier_selection.active]

    # Change binwidth to selected value
    bin_width = binwidth_select.value

    # Value for the range slider is a tuple (start, end)
    range_start = range_select.value[0]
    range_end = range_select.value[1]

    # Create new ColumnDataSource
    new_src = make_dataset(carriers_to_plot,
                           range_start = range_start,
                           range_end = range_end,
                           bin_width = bin_width)

    # Update the data on the plot
    src.data.update(new_src.data)

```

标准的 slider 和 range slider 如下所示：

![image17-滑动块](/images/posts/003-bokeh-data-visualization/17.png)

除了使用更新功能显示的数据之外，还可以更改绘图的其他方面。例如，要更改标题文本以匹配 bin 宽度，可以执行以下操作：

```python
# Change plot title to match selection
bin_width = binwidth_select.value
p.title.text = 'Delays with %d Minute Bin Width' % bin_width

```

在 Bokeh 中还有许多其他类型的交互，但是现在，我们的三个控件允许用户在图表上“玩”很多！

### 把它们放在一起

我们的互动图表的所有元素都已到位。 我们有三个必要的函数：`make_dataset`，`make_plot`和 `update` 来根据控件和小部件本身改变绘图。 我们通过定义布局将所有这些元素连接到一个页面上。

```python
from bokeh.layouts import column, row, WidgetBox
from bokeh.models import Panel
from bokeh.models.widgets import Tabs
# Put controls in a single element
controls = WidgetBox(carrier_selection, binwidth_select, range_select)

# Create a row layout
layout = row(controls, p)

# Make a tab with the layout
tab = Panel(child=layout, title = 'Delay Histogram')
tabs = Tabs(tabs=[tab])

```

我将整个布局放在一个选项卡上，当我们完成一个完整的应用程序时，我们可以将每个绘图放在一个单独的选项卡上。 所有这些工作的最终结果如下：

![image18-带选项卡的交互图](/images/posts/003-bokeh-data-visualization/18.gif)

## 在 Bokeh 中创建交互式可视化应用程序

接下来将重点介绍 Bokeh 应用程序的结构，而不是绘图细节，但后续会提供所有内容的完整代码。我们将继续使用 NYCFlights13 数据集，这是 2013年 纽约 3 个机场的航班的真实航班信息集合。

要自己运行完整的应用程序，首先请确保安装了Bokeh（使用`pip install bokeh`）。

其次，请在公众号『Python数据之道』后台回复 “code”，获取本项目的源代码地址，然后从该地址中下载 `bokeh_app.zip` 文件夹，解压缩，打开目录中的命令窗口，然后键入 `bokeh serve --show bokeh_app` 。 这将设置一个本地 Bokeh 服务器并在浏览器中打开该应用程序。

### 最终的产品

在进入细节之前，让我们来看看我们的目标是什么，这样可以看到这些产品是如何组合在一起的。 以下是一个简短的剪辑，展示了我们如何与整个仪表板进行交互：

<iframe id='myiframe' src="//player.bilibili.com/player.html?aid=45277942&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="696" height="470"> </iframe>

在这里，我在浏览器中使用 Bokeh 应用程序（在 Chrome 的全屏模式下），该应用程序在本地服务器上运行。 在顶部，我们看到许多选项卡，每个选项卡包含应用程序的不同部分。 仪表板的初衷是，虽然每个选项卡可以独立存在，但我们可以将它们中的许多连接在一起，以便能够完整地探索数据。 该视频显示了我们可以使用 Bokeh 制作的图表范围，从直方图和密度图，到我们可以按列排序的数据表，再到完全交互式地图。 除了我们可以在 Bokeh 中创建的图形范围之外，使用 Bokeh 库的另一个好处是交互。 每个选项卡都有一个交互元素，使用户可以访问数据并进行自己的发现。 根据经验，在探索数据集时，人们喜欢自己探索，我们可以允许他们通过各种控制选择和筛选数据。

现在我们已经了解了我们的目标，让我们来看看如何创建一个 Bokeh 应用程序。 强烈建议您自己下载代码来运行（在公众号『Python数据之道』后台回复 “code”，获取本项目的源代码地址）！

### Bokeh 应用程序的文件结构

在编写任何代码之前，为我们的应用程序建立一个框架很重要。 在任何项目中，很容易被代码带走，很快就会丢失在一堆半完成的脚本和不合适的数据文件中，因此我们希望事先为我们所有的代码和数据创建一个结构。 该结构将帮助我们跟踪应用程序中的所有元素，并在出现不可避免的错误时协助调试。 此外，我们可以将此框架重新用于未来的项目，因此我们在规划阶段的初始投资将获得回报。

要设置 Bokeh 应用程序，我创建一个父目录来保存名为 `bokeh_app` 的所有内容。 在这个目录中，我们将有一个数据子目录（称为 `data`），我们脚本的子目录（`scripts`）和一个 `main.py` 脚本将所有内容整合到一起。 通常，为了管理所有代码，我发现最好将每个选项卡的代码保存在单独的 Python 脚本中，并从单个主脚本中调用它们。 以下是我用于 Bokeh 应用程序的文件结构，该文件结构改编自官方文档。

```python
bokeh_app
|
+--- data
|   +--- info.csv
|   +--- info2.csv
|
+--- scripts
|   +--- plot.py
|   +--- plot2.py
|
+--- main.py

```

对于这次我们分析的航班程序项目，文件结构遵循一般大纲，如下：

![image20-航班程序项目结构](/images/posts/003-bokeh-data-visualization/20.png)

在一个 `bokeh_app` 目录下有三个主要部分：`data`，`scripts` 和 `main.py`。 当运行服务器时，我们告诉 Bokeh 服务于 `bokeh_app` 目录，它将自动搜索并运行 `main.py` 脚本。 有了一般的结构，让我们来看看 `main.py` ，这就是我喜欢称之为 Bokeh 应用程序的执行者！

### 主程序文件 （main.py）

`main.py` 脚本就像一个 Bokeh 应用程序的执行程序。 它加载数据，将其传递给其他脚本，返回结果图，并将它们组织到一个显示中。 这将是我完整展示的唯一脚本，因为它对应用程序尤其重要。

```python
# Pandas for data management
import pandas as pd

# os methods for manipulating paths
from os.path import dirname, join

# Bokeh basics
from bokeh.io import curdoc
from bokeh.models.widgets import Tabs


# Each tab is drawn by one script
from scripts.histogram import histogram_tab
from scripts.density import density_tab
from scripts.table import table_tab
from scripts.draw_map import map_tab
from scripts.routes import route_tab

# Using included state data from Bokeh for map
from bokeh.sampledata.us_states import data as states

# Read data into dataframes
flights = pd.read_csv(join(dirname(__file__), 'data', 'flights.csv'),          index_col=0).dropna()

# Formatted Flight Delay Data for map
map_data = pd.read_csv(join(dirname(__file__), 'data', 'flights_map.csv'),
                            header=[0,1], index_col=0)

# Create each of the tabs
tab1 = histogram_tab(flights)
tab2 = density_tab(flights)
tab3 = table_tab(flights)
tab4 = map_tab(map_data, states)
tab5 = route_tb(flights)

# Put all the tabs into one application
tabs = Tabs(tabs = [tab1, tab2, tab3, tab4, tab5])

# Put the tabs in the current document for display
curdoc().add_root(tabs)
```

我们从必要的导入开始，包括制作选项卡的函数，每个函数都存储在 `scripts` 目录中的单独脚本中。 如果查看文件结构，请注意 `scripts` 目录中有一个 `__init __.py` 文件。 这是一个完全空白的文件，需要放在目录中，以便我们使用相对语句导入相应的函数（例如 `from scripts.histogram import histogram_tab`）。 我不太确定为什么需要它，但是它有效。

在 Python 库和脚本导入之后，我们在Python `__file__` 属性的帮助下读取必要的数据。 在这种情况下，我们使用两个 pandas dataframe（ `flights` 和 `map_data`）以及 Bokeh 中包含的美国各州的数据。 一旦读入数据，脚本就会进行委托：它将适当的数据传递给每个函数，每个函数都绘制并返回一个选项卡，主脚本将所有这些选项卡组织在一个名为 `tabs` 的布局中。 作为每个单独的选项卡函数的功能示例，让我们看一下绘制 `map_tab` 的函数。

此函数包含 `map_data`（航班数据的格式化版本）和美国各州的数据，并为选定的航空公司生成航班路线图：

![image21-航班图](/images/posts/003-bokeh-data-visualization/21.png)

```python
def map_tab(map_data, states):
    ...
    def make_dataset(airline_list):
    ...
       return new_src
    def make_plot(src):
    ...
       return p

   def update(attr, old, new):
   ...
      new_src = make_dataset(airline_list)
      src.data.update(new_src.data)

   controls = ...
   tab = Panel(child = layout, title = 'Flight Map')
   return tab

```

我们看到熟悉的 `make_dataset` ，`make_plot` 和 `update` 函数用于绘制带有交互式控件的图。 一旦我们设置了绘图，最后一行将整个绘图返回到主脚本。 每个单独的脚本（5个选项卡中有5个）遵循相同的模式。

接下来返回主脚本，最后一步是收集选项卡并将它们添加到单个文档中。

```python
# Put all the tabs into one application
tabs = Tabs(tabs = [tab1, tab2, tab3, tab4, tab5])
# Put the tabs in the current document for display
curdoc().add_root(tabs)
```

选项卡显示在应用程序的顶部，就像任何浏览器中的选项卡一样，我们可以轻松地在它们之间切换以探索数据。

![image22-带选项卡的交互图](/images/posts/003-bokeh-data-visualization/22.png)

### 运行 Bokeh 服务器

在制作绘图所需的所有设置和代码编写完成之后，在本地运行 Bokeh 服务器非常简单。 我们打开一个命令行界面（我更喜欢 Git Bash， 但任何一个都可以工作），切换到包含 `bokeh_app` 的目录并运行 `bokeh serve --show bokeh_app` 。 假设一切都正确，应用程序将在我们的浏览器中自动打开地址 `http：// localhost：5006 / bokeh_app` 。 然后我们可以访问该应用程序并浏览我们的仪表板，效果如下：

![image23-程序运行后的动态图](/images/posts/003-bokeh-data-visualization/23.gif)

### 在 Jupyter Notebook 中进行调试

如果出现问题（因为毫无疑问，我们最初几次编写仪表板），必须停止服务器，更改文件，然后重新启动服务器以查看我们的更改是否具有所需效果，这可能会令人沮丧。 为了快速迭代和解决问题，我通常在 Jupyter Notebook 中开发。 Jupyter Notebook 是 Bokeh 开发的理想环境，因为您可以在 notebook 中创建和测试完全交互式的图形。 语法略有不同，但是一旦你有一个完整的绘图，代码只需要稍加修改，然后可以复制并粘贴到一个独立的 `.py` 脚本中。

要了解这一点，请查看用于开发应用程序的 Jupyter Notebook （请在公号『Python数据之道』后台回复 “code”，找到本项目的源代码地址，获取相应的 Jupyter Notebook 代码文件）。

### 总结

完全交互式的 Bokeh 仪表板使任何数据科学项目都脱颖而出。 通常情况下，我看到我的同事做了很多很棒的统计工作，但却未能清楚地传达结果，这意味着所有工作都没有得到应有的认可。 从个人经验来看，我也看到了 Bokeh 应用程序在传达结果方面的有效性。 虽然制作完整的仪表板需要做很多工作，但结果是值得的。 此外，一旦我们有了一个应用程序，可以将该框架重新用于其他项目。

从这个项目中，我们可以总结出几个关键点，以适用于许多类似的数据科学项目：

1. 在开始数据科学任务（Bokeh 或其他任何东西）之前，拥有适当的框架/结构至关重要。 这样，你就不会发现自己迷失在试图查找错误的代码的泥潭中。 此外，一旦我们开发出一个有效的框架，它可以用最少的努力重复使用。
2. 找到一个允许您快速迭代思路的调试工具至关重要。 编写代码 - 查看结果 - 修复错误，这种循环在 Jupyter Notebook 可以实现高效的开发（尤其是对于小规模项目）。
3. Bokeh 中的交互式应用程序将提升您的项目并鼓励用户参与。 仪表板可以是一个独立的探索项目，或突出您已经完成的所有艰难的分析工作！
4. 估计你永远不知道在哪里可以找到你将在工作或辅助项目中使用的下一个工具。 所以，不要害怕尝试新的软件和技术！

以上是本文的全部内容，通过像 Bokeh 和 plot.ly 这样的 Python 库，制作交互式图表变得更加容易，并且能够以引人注目的方式呈现数据科学成果。

本文的源代码，请在公号『Python数据之道』后台回复 “code” 来获取。

关于 Bokeh 基础介绍的更多内容，可以查看一下文章内容：

* [Bokeh入门](http://liyangbit.com/pythonvisualization/bokeh-start-circle/)
* [figure 详细解读](http://liyangbit.com/pythonvisualization/bokeh-figure/)
* [Bokeh基础可视化图形](http://liyangbit.com/pythonvisualization/bokeh-glyphs-1st/)
* [数据类型简介： ColumnDataSource](http://liyangbit.com/pythonvisualization/bokeh-data-01/)
* [数据的添加、修改和筛选](http://liyangbit.com/pythonvisualization/bokeh-data-02/)
* [图形与组件的布局简介](http://liyangbit.com/pythonvisualization/Bokeh-Laying-out-Plots-and-Widgets/)

---

本文来源

作者：Will Koehrsen

* [Data Visualization with Bokeh in Python, Part I: Getting Started](https://towardsdatascience.com/data-visualization-with-bokeh-in-python-part-one-getting-started-a11655a467d4)

* [Data Visualization with Bokeh in Python, Part II: Interactions](https://towardsdatascience.com/data-visualization-with-bokeh-in-python-part-ii-interactions-a4cf994e2512)

* [Data Visualization with Bokeh in Python, Part III: Making a Complete Dashboard](https://towardsdatascience.com/data-visualization-with-bokeh-in-python-part-iii-a-complete-dashboard-dc6a86aa6e23)

---

<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>