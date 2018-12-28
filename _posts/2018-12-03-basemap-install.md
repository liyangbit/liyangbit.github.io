---
layout: posts
title: "Windows系统下安装Basemap，以及 PROJ_LIB 错误处理"
teaser:
date: 2018-12-03
header:
   image_fullwidth: "image-article-head.jpg"
categories:
   - PythonVisualization
tags:
   - Matplotlib
   - Basemap
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---



{% include alert info='历尽艰辛，Windows系统下安装Basemap，以及 PROJ_LIB 错误处理' %}




<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>


Basemap是matplolib的一个组件，是地图数据可视化的重要工具。

本人安装的是基于 python.6 版本的 Anaconda,尝试了诸多种安装basemap的方法，其中多次遭遇安装失败，也查找了网上许多tips，发现这个一个普遍的问题。因此决定将该方法分享出来，但不保证一定成功,供大家参考。

首先声明下本人的PC环境:

# 环境一： win10
- 操作系统:Windows10
- 基于 python.6 版本的 Anaconda

在 windows10 下，安装步骤如下：

（1）首先，从下面的网站下载 basemap 和 pyproj 的whl文件

https://www.lfd.uci.edu/~gohlke/pythonlibs/

对应 python.6 版，windows 系统下的文件如下：
* basemap-1.2.0-cp36-cp36m-win_amd64.whl
* pyproj-1.9.5.1-cp36-cp36m-win_amd64.whl


（2）安装 pyproj 的 whl 文件， 如下：

```python
pip install 路径+文件名
```

<div align="center">
    <img src="/images/posts/basemap-install/1.png">
    <font size="3" >图1</font>
</div>

（3）安装basemap 的 whl 文件，如下：

<div align="center">
    <img src="/images/posts/basemap-install/2.png">
    <font size="3" >图2</font>
</div>

（4）测试下 basemap 是否安装成功

```python
from mpl_toolkits.basemap import Basemap
```

<div align="center">
    <img src="/images/posts/basemap-install/3.png">
    <font size="3" >图3</font>
</div>

如上图所示，显示 Matplotlib 缺少文件， Basemap 不能正确运行。

（5）更新 Matplotlib 库

```python
pip install matplotlib --upgrade
```

<div align="center">
    <img src="/images/posts/basemap-install/4.png">
    <font size="3" >图4</font>
</div>

（6）再次测试 Basemap 是否安装成功

<div align="center">
    <img src="/images/posts/basemap-install/5.png">
    <font size="3" >图5</font>
</div>

可以看到，现在 Basemap 是可以正确运行的。

接下来，在 Jupyter Notebook 中运行代码试试。

```python
import matplotlib as mpl
import matplotlib.pyplot as plt

%matplotlib inline
plt.style.use('ggplot')

from mpl_toolkits.basemap import Basemap

plt.figure(figsize=(8,8))

m = Basemap(projection='ortho', resolution=None,
           lat_0=50,lon_0=-100)
m.bluemarble(scale=0.5)
```

运行结果如下：

<div align="center">
    <img src="/images/posts/basemap-install/6.png">    
</div>

<div align="center">   
    <font size="3" >图6</font>
</div>

以上运行结果说明，Basemap 已经成功安装。

# 环境二： win7

- 操作系统:Windows7
- 基于 python.6 版本的 Anaconda

本以为 Basemap 的安装就告一段落了，但本人还有一个 windows 7 系统 的PC。

想着 win7 跟 win10 应该是一样的安装， 但按上述的步骤安装后，出现以下错误：

运行代码：

```python
from mpl_toolkits.basemap import Basemap
```

出现以下问题：

```python
Traceback (most recent call last)
<ipython-input-1-d9467465a3b6> in <module>()
----> 1 from mpl_toolkits.basemap import Basemap

/opt/conda/lib/python.6/site-packages/mpl_toolkits/basemap/__init__.py in <module>()
    144
    145 # create dictionary that maps epsg codes to Basemap kwargs.
--> 146 pyproj_datadir = os.environ['PROJ_LIB']
    147 epsgf = open(os.path.join(pyproj_datadir,'epsg'))
    148 epsg_dict={}

/opt/conda/lib/python.6/os.py in __getitem__(self, key)
    667         except KeyError:
    668             # raise KeyError with the original key value
--> 669             raise KeyError(key) from None
    670         return self.decodevalue(value)
    671

KeyError: 'PROJ_LIB'
```

这个 “PROJ_LIB” 问题，我尝试了很多遍，也查阅了很多网站提供的解决方案，发现大部分都不是太符合，最后在某个英文的解决方案里有提到一种 较为间接的解决方案， 经过尝试后，发现是可以安装 Basemap， 并成功运行的。

在此，给大家分享下。

其中曲折的试错过程就不描述了，直接介绍成功的过程（但不保证在你的电脑上是没有问题的）。

这种方法的核心是 在 Anaconda 下创建一个虚拟环境，然后在 虚拟环境中安装和运行 Basemap。

（1）在 Anaconda 下创建一个虚拟环境

打开 Anaconda Prompt， 在 Anaconda 的默认跟目录下创建一个虚拟环境，名称为 “py36”。

```python
conda create -n py36 python=3.6
```

关于 Anaconda 下虚拟环境的安装，请参考以下内容：
http://liyangbit.com/anaconda/anaconda-install-env-to-custom-path/

安装好后，可以用以下命令来查看虚拟环境的列表：
```python
conda info --envs
```

然后激活虚拟环境
```python
activate py36
```

（2）在 虚拟环境 “py36” 下安装 matplotlib
```python
pip install matplotlib
```

（3）在 虚拟环境 “py36” 下安装 pyproj 的 whl 文件， 如下：

```python
pip install 路径+文件名
```


（4）在 虚拟环境 “py36” 下安装basemap 的 whl 文件，如下：

```python
pip install 路径+文件名
```

（5）在 虚拟环境 “py36” 下测试下 basemap 是否安装成功

```python
from mpl_toolkits.basemap import Basemap
```


<div align="center">
    <img src="/images/posts/basemap-install/7.png">
</div>

<div align="center">   
    <font size="3" >图7</font>
</div>

可以看到，在虚拟环境下，Basemap 是可以正确运行的。

（6）测试下在 Jupyter Notebook 中 Basemap 是否可以运行

程序文件目录下输入

```python
jupyter notebook
```
发现在默认情况下没有找到之前创建的 虚拟环境。

在 Jupyter Notebook 中， 创建的虚拟环境需要安装一个插件才能正确的显示出来。

打开 Anaconda Prompt，插件的安装命令如下：

```python
conda install nb_conda
```

再重新开启Jupyter Notebook，就可以找到安装好的虚拟环境，如下：

<div align="center">
    <img src="/images/posts/basemap-install/8.png">
</div>

<div align="center">   
    <font size="3" >图8</font>
</div>


在虚拟环境下运行下面的代码：


```python
import matplotlib as mpl
import matplotlib.pyplot as plt

%matplotlib inline
plt.style.use('ggplot')

from mpl_toolkits.basemap import Basemap

plt.figure(figsize=(8,8))

m = Basemap(projection='ortho', resolution=None,
           lat_0=50,lon_0=-100)
m.bluemarble(scale=0.5)
```

运行结果如下：

<div align="center">
    <img src="/images/posts/basemap-install/6.png">    
</div>

<div align="center">   
    <font size="3" >图6</font>
</div>

以上运行结果说明，Basemap 已经成功安装。

至此，在 win10 和 win7 下都可以安装和运行 Basemap。

如果你对 Basemap 感兴趣，也可以进行安装和使用。

如有问题，不妨一起交流下。


---

对我的文章感兴趣的朋友，可以关注我的微信公众号『Python数据之道』（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>

