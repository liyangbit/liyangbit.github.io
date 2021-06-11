---
layout: posts
title: "手把手教你用 Python 进行机器学习"
teaser:
date: 2019-06-28
header:
   image_fullwidth: "image-head.jpg"
categories:
   - MachineLearning
tags:    
   - Scikit-learn
   - MachineLearning      
comments: true
show_meta: true
sidebar: right
authors: ["阳哥"]
---

{% include alert info='手把手教你用 Python 进行机器学习' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# 手把手教你用 Python 进行机器学习

原文作者：Oleksii Kharkovyna

翻译：李洁

编辑：阳哥

译文出品：Python数据之道

![img01](/images/posts/009-machine-learning-guideline/01.jpg)

机器学习是人工智能领域的一个重要研究课题，近年来一直倍受关注。这一领域可能会提供有吸引力的机会，而且在这一领域开始职业生涯并不像乍看上去那么困难。即使你在数学或编程方面没有任何经验，也不是问题。学会的最重要的因素纯粹是你自己的兴趣和学习所有这些东西的动机。

如果你是一个新手，不知道从哪里开始学习、为什么需要机器学习、以及为什么它最近越来越受欢迎，看本文就对了！我已经收集了所有必要信息和有用资源来帮助你学到新的知识和完成你的第一个项目。

## 为什么选择Python？

如果你的目标是成为一名成功的程序员，你需要知道很多事情。但是，对于机器学习和数据科学来说，掌握至少一种编程语言并能够自信地运用它就足够了。所以，冷静点，你不必成为编程天才。

成功的机器学习之旅需要从一开始就选择适当的编程语言，因为你的选择将决定你的未来。在这一步，你必须有计划地思考，正确安排优先事项，不要把时间花在不必要的事情上。

我的观点是：Python 对于初学者来说是一个完美的选择，可以让你专注于机器学习和数据科学领域。它是一种极简且直观的语言，具有全功能的工具库线（也称为框架），大大减少了获得第一个结果所需的时间。

你也可以考虑使用 R 语言，但就个人而言，我更倾向于使用 Python。你可以在以下链接读到我对这个问题的详细解释：

Python vs R. Choosing the Best Tool for AI, ML & Data Science.

https://medium.com/datadriveninvestor/python-vs-r-choosing-the-best-tool-for-ai-ml-data-science-7e0c2295e243

## 第0步  你需要了解的ML流程的简要概述

![img02](/images/posts/009-machine-learning-guideline/02.jpg)

机器学习是基于经验的学习。举个例子，它就像一个人通过观察别人下棋来学习下棋。通过这种方式，计算机可以通过提供经过训练的信息来编程，从而获得高概率识别个体或其特征的能力。

首先，你需要知道机器学习分为这些不同的阶段:

- 数据收集
- 数据排序
- 数据分析
- 算法开发
- 检查生成的算法
- 使用算法进一步得出结论

关于提取模式，不同的算法被使用，这些算法被分为两类:

- 无监督学习
- 有监督学习

在无监督学习的情况下，机器只接收一组输入数据。此后，机器将决定输入数据与任何其他假定数据之间的关系。与监督学习不同的是，当机器被提供一些用于学习的验证数据时，独立的无监督学习意味着计算机本身将发现不同数据集之间的模式和关系。无监督学习可以进一步分为聚类和关联学习。

有监督学习意味着计算机能够根据提供的样本识别元素。计算机对其进行研究，并开发基于这些数据识别新数据的能力。例如，可以训练你的计算机根据以前收到的信息过滤垃圾邮件。

一些监督学习算法包括：

- 决策树
- 支持向量机
- 朴素贝叶斯分类器
- K近邻
- 线性回归

## 第1步  复习 Python 数学库所需要的数学技能

> 一个在人工智能和机器学习领域工作的不懂数学的人就像一个不懂说服技巧的政客。

如果没有起码的数学知识基础，你就无法处理机器学习（**Machine Learning**，ML）和数据科学项目。然而，并不是说需要有数学学位。根据我的个人经验，每天至少花 30-45 分钟学习就会很有收获，你会更快地理解和学习 Python 的应用数学和统计主题。

你需要不断阅读或更新基础理论。不需要阅读整个教程，只需要关注关键的概念。

以下是数据分析和机器学习所需数学的3个步骤:

1. 用于数据分析的线性代数：标量、向量、矩阵和张量

   例如，对于主成分法，你需要了解特征向量，而回归方法需要知道矩阵乘法。此外，机器学习通常使用高维数据（包含许多变量的数据）。这种数据类型最好由矩阵表示。

2. 数学分析：导数和梯度

   数学分析是许多机器学习算法的基础。优化问题需要理解导数和梯度。例如，最常见的优化方法之一就是梯度下降。
   为了快速学习线性代数和数学分析，我推荐以下课程：

   - [Khan Academy](https://www.khanacademy.org/) 提供线性代数和数学分析的短期实战课程，基本涵盖了最重要的内容。

   - [MIT OpenCourseWare](https://ocw.mit.edu/index.htm) 为机器学习提供了很棒的数学学习课程，提供了所有视频讲座和学习材料。

3. 梯度下降：从头开始构建一个简单的神经网络

   ![img03](/images/posts/009-machine-learning-guideline/03.gif)

   数据分析和机器学习领域学习数学的最好方法之一是从零开始构建一个简单的神经网络。你将会使用线性代数来表示网络，并使用数学分析来优化它。特别是，你将从零开始创建一个梯度下降过程。不需要太担心神经网络间的细微差别。只按照说明编写代码就可以了。

   以下是一些很好的练习：

   《Python中的神经网络（**Neural Network in Python** ）》
   https://www.amazon.com/Neural-Network-Projects-Python-ultimate/dp/1789138906
   这是一个很棒的教程，会教会你从头构建一个简单的神经网络。你也能够找到有用的插图来理解梯度下降是如何工作的。

   有些更短的教程也能够帮助你逐步掌握神经网络:

   《如何在Python中从零开始构建自己的神经网络（[**How to build your own Neural Network from scratch in Python**](https://towardsdatascience.com/how-to-build-your-own-neural-network-from-scratch-in-python-68998a08e4f6)）》
   https://towardsdatascience.com/how-to-build-your-own-neural-network-from-scratch-in-python-68998a08e4f6

   《在Python中从零实现一个神经网络——导论（[**Implementing a Neural Network from Scratch in Python — An Introduction**](http://www.wildml.com/2015/09/implementing-a-neural-network-from-scratch/)）》
   http://www.wildml.com/2015/09/implementing-a-neural-network-from-scratch/

   《面向初学者的机器学习：神经网络导论（[**Machine Learning for Beginners: An Introduction to Neural Networks**](https://victorzhou.com/blog/intro-to-neural-networks/) ）》
   https://victorzhou.com/blog/intro-to-neural-networks/
   关于神经网络如何工作以及如何在Python中从零开始实现的一个很棒的简明解释。

## 第2步  学习Python语法的基础知识

好消息是：你不需要完整的学习课程，因为 Python 和数据分析并不是同义词。

![img04](/images/posts/009-machine-learning-guideline/04.jpg)

在开始深入研究语法之前，我想分享一个可以将你失败可能性降到最低的建议。

> 通过阅读有关游泳技巧的书籍来学习游泳是不可能的，但是在泳池训练的同时阅读这些书籍可以更有效地学会技能。

编程的学习也是类似的。只关注语法是不值当的，你很可能会因此而失去兴趣。

你不需要记住所有的东西。尝试迈出一小步，把理论知识和实践结合起来。专注于直观的理解，例如，在特定的情况下哪个函数是合适的，以及条件运算符是如何工作的。在编写代码的过程中，你会通过阅读文档逐步记住语法。很快你就不再需要谷歌这些东西了。

如果你对编程没有任何了解，建议阅读《用Python自动处理那些无聊的事情（[**Automate the Boring Stuff With Python**](https://automatetheboringstuff.com/)）》。https://automatetheboringstuff.com/
这本书为初学者提供了实用编程的解释，并从零开始教学。阅读第6章“字符串操作”，完成该课的实际任务，就足够了。

下面是其他一些值得探索的好资源:

[**Codecademy**](https://www.codecademy.com/)——https://www.codecademy.com/, 教授良好的通用语法。

[**Learn Python the Hard Way**](https://learnpythonthehardway.org/) —— https://learnpythonthehardway.org/, 一本出色的手册式书籍，解释了基础知识和更复杂的应用程序。

[**Dataquest**](https://www.dataquest.io/) —— https://www.dataquest.io/, 这个资源在教授数据科学的同时也教授语法。

Python教程（[**The Python Tutorial**](https://docs.python.org/3/tutorial/)）——https://docs.python.org/3/tutorial/, 官方文档。

记住，你越早开始做真正的项目，你就会越早学会它。无论如何，如果需要的话，你总能够回头继续学习语法。

## 第3步  探索主要的数据分析库

<!-- ![img](file:///D:/Work/%E7%BF%BB%E8%AF%91/Beginner%E2%80%99s%20Guide%20to%20Machine%20Learning%20with%20Python/Beginner%E2%80%99s%20Guide%20to%20Machine%20Learning%20with%20Python%20%E2%80%93%20Towards%20Data%20Science_files/0_dAiGm5JDmM83PZTf(1)) -->

![img05](/images/posts/009-machine-learning-guideline/05.jpg)

下一个阶段是进一步学习适用于数据科学的 Python 库或框架。如前所述，   Python 拥有大量的库。库是现成函数和对象的集合，你可以直接将它们导入到脚本中，从而节省时间。

如何使用库？以下是我的建议：

1. 打开Jupyter Notebook(见下文)。
2. 大约用半小时把库文件看一遍。
3. 将该库导入到您的 Jupyter Notebook 中。
4. 按照分步的指南查看正在运行的库。
5. 检查文档，了解它的其他功能。

我不建议立即投入到学习库中，因为当你开始在项目中使用它们时，你可能会忘记所学的大部分内容。相反，你应该试着弄明白用到的每个库都能做些什么。

### Jupyter Notebook

Jupyter Notebook 是一款轻量级的 IDE，是分析师们的最爱。在大多数情况下，Python 的安装包里已经包含了 Jupyter Notebook。你可以通过Anaconda Navigator 打开一个新项目，它包含在 Anaconda 包中。

## 你可能会需要的Python库

### Numpy

[安装教程](https://docs.scipy.org/doc/numpy/user/)
https://docs.scipy.org/doc/numpy/user/

[快速上手教程](https://docs.scipy.org/doc/numpy/user/quickstart.html)
https://docs.scipy.org/doc/numpy/user/quickstart.html

NumPy 由 Numerical Python 缩写而来，它是专业人士和初学者最通用和使用最广泛的库。使用这个工具，你可以轻松舒适地操作多维数组和矩阵。像线性代数运算和数值转换这样的函数也是可用的。

### Pandas

[安装教程](https://pandas.pydata.org/pandas-docs/stable/)
https://pandas.pydata.org/pandas-docs/stable/

[快速上手教程](https://pandas.pydata.org/pandas-docs/stable/getting_started/10min.html)
https://pandas.pydata.org/pandas-docs/stable/getting_started/10min.html

Pandas 是一个众所周知的高性能数据框表示工具。使用它，你几乎可以从任何数据源加载数据，计算各种函数并创建新的参数，使用类似于 SQL 的聚合函数构建对数据的查询。此外，还有各种矩阵变换函数、滑动窗口法等从数据中获取信息的方法。所以这个库对于一个优秀的专家来说是必不可少的。

### Matplotlib

[安装教程](https://matplotlib.org/contents.html)
https://matplotlib.org/contents.html

[快速上手教程](https://matplotlib.org/users/pyplot_tutorial.html)
https://matplotlib.org/users/pyplot_tutorial.html

Matplotlib 是一个用于创建图形和可视化的灵活库。它很强大，但有点笨重。你可以跳过 Matplotlib 并从使用 Seaborn 开始（请参阅下面的 Seaborn）。

### Scikit-Learn

[安装教程](https://scikit-learn.org/stable/documentation.html)
https://scikit-learn.org/stable/documentation.html

[快速上手教程](https://elitedatascience.com/python-machine-learning-tutorial-scikit-learn)
https://elitedatascience.com/python-machine-learning-tutorial-scikit-learn

我可以说这是迄今为止我见过的设计得最完善的 ML 包。它实现了广泛的机器学习算法，并使其易于嵌入到实际应用程序中。你可以使用库中大量的函数，比如回归、聚类、模型选择、预处理、分类等等。所以它值得全面学习和使用。它最大的优点是工作速度快，因此 Spotify、Booking.com、J.P.Morgan 等主流平台都在使用 Scikit-Learn 是理所当然的。

## 第4步  开发结构化的项目

一旦掌握了基本语法和库的基本知识，就可以开始自己完成项目了。多亏了这些项目，你将能够学习新的东西，并为长远的工作做投资。

已有足够多的资源为结构化项目提供主题：

[Dataquest](https://www.dataquest.io/)——交互式地教授 Python 和数据科学。分析从中央情报局的文件到 NBA 比赛的统计数据等的一系列有趣的数据集，并开发包括神经网络和决策树在内的策略算法。
https://www.dataquest.io/

[Python for Data Analysis](http://bedford-computing.co.uk/learning/wp-content/uploads/2015/10/Python-for-Data-Analysis.pdf)——作者写了很多关于 Python数据分析的论文。
http://bedford-computing.co.uk/learning/wp-content/uploads/2015/10/Python-for-Data-Analysis.pdf

[Scikit 文档](https://scikit-learn.org/stable/documentation.html)——Python 的主要计算机训练库。
https://scikit-learn.org/stable/documentation.html

[CS109](https://cs109.github.io/2015/)——哈佛大学数据科学的课程。
https://cs109.github.io/2015/

## 第5步  做出你自己的项目

你可以发现许多新东西，但重要的是找到那些你真正感兴趣的项目。然而，在找到理想工作的快乐时刻到来之前，你应该学会如何出色地处理程序中的错误。以下是在这方面最受欢迎的有用资源中，比较突出的一些:

StackOverflow —— 一个人们可以在上面讨论所有可能的问题的多功能网站，提供了一系列问题和答案。此外，它受众广泛，所以你可以询问你的错误并从大量用户那里得到答案

Python 文档 —— 另一个搜索参考资料的好地方

不言而喻，你也不应该忽视任何机会或合作的邀请。尽可能参与所有与 Python 相关的活动，并找到从事有趣项目的人员。顺便说一下， Github 是实现这一目标的绝佳场所。学习新知识并在同类中不断调整 —— 所有这些都将有助于提高你的水平！

## 写在最后以及一点鼓励

![img06](/images/posts/009-machine-learning-guideline/06.jpg)

你可能会问：“为什么我要投身于机器学习领域；也许已经有很多其他优秀的专家了。”

知道吗？我也曾落入这个陷阱，但现在我可以大胆地说——这类想法不会给你带来任何好处而是你成功的巨大障碍。

> 根据摩尔定律，集成电路上的晶体管数量每24个月翻一番。这意味着，我们的计算机性能每年都在进步，也意味着以前无法触及的知识边界再次“向右移动”——大数据和机器学习算法的研究还有空间！

谁知道将来会发生什么。也许这些数字会增加得更多，机器学习也会变得更重要？很可能是的！

最可怕的就是你误认为你的位置已经被其他专家取代了。

我希望你有足够的意愿和力量去学习，然后拥有那些能点燃你内心之光的项目！在评论中分享你的经验吧！

---

本文来源：

https://towardsdatascience.com/beginners-guide-to-machine-learning-with-python-b9ff35bc9c51

---
译者简介：

李洁，北京师范大学香港浸会大学联合学院 数据科学系助教，香港科技大学电信学硕士。

---

对我的文章感兴趣的朋友，可以关注我的微信公众号「Python数据之道」（ID：PyDataLab），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
