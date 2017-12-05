---
layout: posts
title: "04-创建第一个'.py' 和 '.ipynb' 文件"
subheadline: "Python基础"
teaser:
date: 2017-12-05
header:
   image_fullwidth: "image-head.jpg"
categories:
   - tutorials
tags:
   - tutorials-python-basic
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---

本文来源于：
<a href="http://liyangbit.com" target="blank">liyangbit.com</a>

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>


**创建第一个".py" 和 ".ipynb" 文件**

上一节中，我们运行了第一个 Python 程序。在上节中， 我们是在 Anaconda程序文件夹下打开 “iPython” 来运行的 Python 程序。可能你已经注意到，在上节中，写好的 Python 程序运行后，输出了相关结果。但是，Python代码文件并没有保存下来，如果下次还要运行相同的代码，或者已有代码需要进行修改时，在 “iPython” 中不好实现。

这里，我们来介绍下，在通常情况下，我们应该怎样保存和修改Python程序文件。

通常，我们看到的用Python语言编写的源代码文件，其文件后缀是 ".py" 或 ".ipynb"。这里，我列了两种文件，其中 ".py" 文件是标准的Python源代码文件，通常情况下，我们会使用 ".py" 的python源代码文件。

另一种是 ".ipynb" 文件，会在本文后续部分进行描述。

## 1 创建和运行 ".py" 文件


### 1.1 创建 ".py" 文件

在E盘下创建名称为 "tutorials" 的文件夹，打开"tutorials" 文件夹，鼠标右击创建文本文档，修改文件名称为 "hello.py" ，演示如下：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-01.png">
</div>

**注意要修改文件的后缀为 ".py"** （默认是 ".txt"）

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-02.png">
</div>

### 1.2 打开 ".py" 文件

依次点击“windows开始菜单”，“所有程序”，在 Anaconda程序文件夹下打开 “spyder” 程序（对应下图红色数字1、2、3）

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-08.png">
</div>


然后从 "spyder" 中打开 "hello.py" 程序文件（下图红色箭头所指），得到如下界面：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-03.png">
</div>

打开 "hello.py" 文件后，我们可以从左边看到，这是一个空白文件，里面没有任何代码。

在上图界面的右下角，我们可以找到 “iPython console” （iPython控制台），我们可以选择在此输出Python程序的运行结果。

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-04.png">
</div>

### 1.3 运行 ".py" 文件

在 "hello.py" 文件中输入如下代码：

```python
print("hello, world")
```

保存文件，然后点击绿色的“三角符号”来运行程序（spyder中运行python程序的快捷键是 “F5”），如下：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-05.png">
</div>

如果是第一次运行，点击绿色的“三角符号”后，会弹出如下对话界面：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-06.png">
</div>

选择上图所示默认设置，然后点击 “run”，运行python程序，此时，我们会在右下角的 IPython控制台输出结果，如下：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-07.png">
</div>

可以看到，我们输出的结果是 "hello, world" （不含引号），跟上节在 iPython界面下的输出结果是一致的。

我们再来试试在"hello.py" 文件中加入更多代码：

```python
print("hello, world")

print("hello,", "world", "welcome!")

print(1+2)

print('1+2=', 1+2)
```

保存文件，然后运行程序，运行结果如下：

```python
hello, world
hello, world welcome!
3
1+2= 3
```

请注意，不同于 iPython界面，在 "hello.py" 程序文件中，每一次需要打印输出结果的地方，都需要用 `print()` 函数来输出结果。

## 2 文本编辑器

上面介绍的是在 spyder中编辑并运行python程序，有时候，我们可能想在其他文本编辑器中编辑python程序。有很多的文本编辑器可以用来编写python程序，作为刚开始接触的同学，可以使用 <a href="https://notepad-plus-plus.org/" target="blank">notepad++ </a>  或者 <a href="http://www.sublimetext.com/" target="blank"> sublime text </a> 来编写 python程序。 这两个文本编辑器都是免费可以使用的，并且程序不大。

上述 "hello.py" 文件，在 notepad++ 中打开的界面如下：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-09.png">
</div>

关于这两个编辑器的使用，可以自行查找下。



## 3 创建和运行 ".ipynb" 文件

".ipynb" 文件是使用 Jupyter Notebook 来编写 Python 程序时的文件。

Jupyter Notebook（此前被称为 IPython notebook）是一个交互式笔记本，支持运行 40 多种编程语言。它会在浏览器中打开并运行相关程序，在这里，我们主要介绍其在编写和运行 Python 程序方面的应用。

在安装好 Anaconda 后，已经自动安装好了 Jupyter Notebook，如下所示（红色数字4所指）：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-08.png">
</div>

点击 "Jupyter Notebook"，可以得到如下启动的命令行界面，以及在浏览器中打开的 Jupyter Notebook 界面。

命令行界面：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-10.png">
</div>

Jupyter Notebook 界面：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-11.png">
</div>

从上面可看出，默认的启动界面是以 Jupyter notebook 的安装路径为初始路径的（即从C盘启动）。

**如果我们希望在需要运行的程序所在文件夹启动，该如何操作呢？**

以上述 E盘 "tutorials"文件夹 为例，如果我们希望在该文件夹下创建新的 ".ipynb" 文件或者运行该文件夹下的 ".ipynb" 文件，我们可以先打开 "E:\tutorials"文件夹，然后 按下 "Shift"键同时点击右键，得到如下界面：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-12.png">
</div>

点击 "在此处打开命令窗口(W)"，在弹出的命令行窗口中输入 "jupyter notebook"，在浏览器中得到当前路径下的jupyter notebook 文件列表，如下：

命令行界面：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-13.png">
</div>

Jupyter Notebook 界面：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-14.png">
</div>

请注意，**命令行界面不能关闭**，它是 Jyputer notebook 运行必须要有的环境。

点击上面的 "New" ， 可以开始创建新的 ".ipynb" 文件。

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-15.png">
</div>

在上述新打开的界面中：
* 红色数字1处，可以修改文件名称，比如我们修改为 "hello"(不含引号)
* 红色数字2处，点击保存文件
* 红色数字3处，点击添加新的代码行
* 红色数字4处，点击运行代码
* 红色数字5处，选择当前是编写代码还是以其他格式进行编写（比如 Markdown），默认是编写代码模式，即 "code"。

现在，我们输入之前在 "hello.py" 文件中的代码，每次输入一行代码，然后依次运行（点击红色数字4处的符号，或者按快捷键 "Shift"+"Enter"），运行结果如下：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-16.png">
</div>

此时，我们可以在 "E:\tutorials" 下看到有一个名称为 "hello.ipynb" 的文件，或者在 Jupyter notebook 的首页下，也可以看到该文件，如下：

<div align="center">
    <img src="/images/tutorials/1-python-basic/py-file-17.png">
</div>


## 4 总结

前面我们分别介绍了 ".py" 文件 和 ".ipynb" 文件的创建及运行方法，在以后的章节中，我们会大量涉及到这两类文件的使用。

关于 Jyputer notebook，这里也作了简单的介绍。需要说明的是，Jupyter notebook 是一个强大的工具，在Python的数据科学领域会频繁的使用，在后续也会进一步介绍。

<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="200">
</div>
