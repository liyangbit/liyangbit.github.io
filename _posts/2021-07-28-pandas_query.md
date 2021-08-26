---
layout: posts
title: "Pandas实用技能，数据筛选query函数详细介绍"
teaser:
date: 2021-07-28
header:
   image_fullwidth: "image-head.jpg"
categories:
   - DataAnalysis
tags:    
   - Pandas
comments: true
show_meta: true
sidebar: right
authors: ["阳哥"]
---

{% include alert info='Pandas实用技能，数据筛选query函数详细介绍' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# Pandas实用技能，数据筛选query函数详细介绍

大家好，我是阳哥。

Pandas 可以说是 在Python数据科学领域应用最为广泛的工具之一。

Pandas是一种高效的数据处理库，它以 `dataframe` 和 `series` 为基本数据类型，呈现出类似excel的二维数据。

在数据处理过程中，咱们经常会用到数据筛选，Pandas 中提供了数据筛选的多种方法，这里，阳哥来给大家分享下 在Pandas中应用 `query` 函数来进行数据筛选。

`query` 函数的一般用法如下：

```python
df.query('expression')
```

文中的代码是在 Jupyter Notebook 中运行的（也可以是其他IDE），本次使用的 Pandas 版本是 1.3.0 版，如下：

```python
import pandas as pd


print(f'pandas version: {pd.__version__}')

# pandas version: 1.3.0rc1
```

在开始之前，先创建一份数据，供后续使用：

```python
data = {
    'brand':['Python数据之道','价值前瞻','菜鸟数据之道','Python','Java'],
    'A':[10,2,5,20,16],
    'B':[4,6,8,12,10],
    'C':[8,12,18,8,2],
    'D':[6,18,14,6,12],
    'till years':[4,1,1,30,30]
}

df = pd.DataFrame(data=data)
df
```

数据如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsuqyatslhj30jq0amq3b.jpg)

### 常用方法

筛选 "brand" 列中值为 "Python数据之道" 的行，如下：

```python
df.query('brand == "Python数据之道"')
```

结果如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsurts83aaj30le05yq37.jpg)

<!-- ![](https://tva1.sinaimg.cn/large/008i3skNgy1gsur55dtmgj30l0060q36.jpg) -->

说明一下，上面代码中的单引号和双引号是可以互换的，下面的写法，其结果也是一样的：

```python
df.query(" brand == 'Python数据之道' ")
```

上面用 query 函数筛选数据，用下面的方法也是可以实现的：

```python
df[df['brand']=="Python数据之道"]
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsurtull5sj30j805qjro.jpg)

<!-- ![](https://tva1.sinaimg.cn/large/008i3skNgy1gsur77tc0vj30jc05ct8x.jpg) -->


上面是筛选字符串的值，也可以是筛选数字，如下：

<!-- ![](https://tva1.sinaimg.cn/large/008i3skNgy1gsur9zj2wnj30ey05mq2z.jpg) -->

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsurtx4uitj30i005qdfy.jpg)

### 通过数学表达式筛选

除了直接通过等于某个值来筛选， query 函数还支持通过数学表达式来进行数据筛选，包括 `>`、 `<`、 `+`、 `-`、 `*`、 `/` 等。

示例如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsurv69x1nj30j40b8q3d.jpg)

<!-- ![](https://tva1.sinaimg.cn/large/008i3skNgy1gsurdvcz8pj30fs0bqjrq.jpg) -->

<!-- ![](https://tva1.sinaimg.cn/large/008i3skNgy1gsurdxpfpuj30ec08swer.jpg) -->

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsurv8h7swj30iy08qgly.jpg)

<!-- ![](https://tva1.sinaimg.cn/large/008i3skNgy1gsure069aoj30dq07et8v.jpg) -->

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsurvar1bvj30gw072aaa.jpg)


### 通过变量筛选

在程序比较长的时候，经常会使用变量来作为筛选条件， query 函数在使用变量作为判断标准时，通过在变量前面添加 `@` 符号来实现，示例如下：

```python
# 通过变量来筛选数据，在变量前使用 @ 符号即可

name = 'Python数据之道'

df.query('brand == @name')
```

<!-- ![](https://tva1.sinaimg.cn/large/008i3skNgy1gsurhn4eahj30lg0awgm6.jpg) -->

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsurwhlwdcj30m60am3z3.jpg)

### 列表数据筛选

当需要在某列中筛选多个符合要求的值的时候，可以通过列表（list）来实现，示例如下：

```python
# 需要注意下 双引号 和 单引号的分开使用

df.query('brand in ["Python数据之道","价值前瞻"]')
# df.query("brand in ['Python数据之道','价值前瞻']")
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsurwjyoskj30r00b0my3.jpg)

### 多条件筛选

有很多情况下，咱们需要通过多个条件来筛选数据，query 函数支持多种条件的组合，

- 两者都需要满足的并列条件使用符号 `& `,或 单词 `and`
- 只需要满足其中之一的条件使用符号 `| `,或 单词 `or`

示例如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsurwm3t37j30tk0b0gmm.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsus0lewpjj30tw05st96.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsus0nhmlwj30ti0963zb.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsus0pyhbrj30t008y3z7.jpg)

### 列名称有空格的情况

当 dataframe 的列名称中有空格或其他特殊符号的时候，需要使用 `反引号（backtick mark）`，即键盘ESC键下面的按键(就是键盘上第二排第一个按键，有‘~’这个符号的按键) 来将列名包裹起来，示例如下：

```python
df.query("`till years` < 5")
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsus5jpafqj310m0e2mya.jpg)

注意，如果使用单引号，将会报错，如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsus5m0mytj30nm0e0q58.jpg)

### 筛选后选取数据列

在数据筛选后，还可以选择需要的数据列，如下：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsus5o704xj30ms096dg7.jpg)

### 小结

以上就是关于 Pandas 中 query 函数的主要内容介绍，应用 query函数进行数据筛选，其语言还是比较简洁的，尤其是当条件比较多的时候，会显得更优雅。

比如下面的对比，假设都是三个筛选条件（假设数据量较多，符合的结果也较多）：

**没有使用query函数时**

```python
df[(df['brand']=="Python数据之道") & (df['A'] >2) & (df['C'] >4)]
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsusvhyxljj30yy06emxn.jpg)

可以看出上面的表达式是比较长的，略显繁琐。

**使用query函数时**

```python
df.query(" brand == 'Python数据之道' & A>2 & C>4 ")
```

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsusvkdr7kj30r805o0t5.jpg)

相对来说，使用query 函数会显得更加简洁，如果觉得这个功能不错，就赶紧用起来吧~~

---

对我的文章感兴趣的朋友，可以关注我的微信公众号「**Python数据之道**」（ID：PyDataLab），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
