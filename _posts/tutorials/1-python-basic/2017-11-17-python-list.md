---
layout: posts
title: "Python数据类型-List介绍"
subheadline: "Python基础"
subheadline:
teaser:
date: 2017-11-17
header:
   image_fullwidth: "image-head.jpg"
categories:
   - tutorials
tags:
   - tutorials-python-basic
   - List
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---

**Python基础-Python数据类型-List介绍**

本文来源于：
<!-- [liyangbit.com](liyangbit.com) -->
<a href="http://liyangbit.com" target="blank">liyangbit.com</a>

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

<!-- {% include alert warning='This is a warning.' %}
{% include alert info='An info box.' %}
{% include alert success='Yeah, you made it!' %}
{% include alert alert='Danger!' %}
{% include alert terminal='jekyll -serve' %}
{% include alert text='Just a note!' %} -->

**本文的运行环境：**
* windows7系统
* python3.5
* jupyter notebook

## 1 初识list

list 是 Python 内置的一种高级数据类型。 list是一种有序的集合，在python中应用很广泛。



```python
names = ['James', 'Michael', 'Emma', 'Emily']

print("names的数据类型：",type(names))
print(names)
```

    names的数据类型： <class 'list'>
    ['James', 'Michael', 'Emma', 'Emily']


用 `len()` 可获得 list 的长度，即list集合所包含的元素个数，如下：


```python
n = len(names)
print(n)
```

    4


**空的list**

如果list中一个元素也没有，我们可以定义一个空的list，则其长度为0，如下：


```python
empty_list = []
empty_list
```




    []




```python
len(empty_list)
```




    0



## 2 访问列表中的值

用索引来访问 list 中的每个元素，请注意索引是从 `0` 开始， 最后一个的索引编号为 `n-1`， 即所有元素的编号依次为 (0,1, 2, ..., n-1)。

list中单个元素的访问如下：



```python
names[0]
```




    'James'




```python
names[1]
```




    'Michael'



如果要获取最后一个元素，也可以用 `-1` 来做索引，倒数第二个元素可以用 `-2` 来索引，其他以此类推。


```python
names[-1]
```




    'Emily'




```python
names[-2]
```




    'Emma'



可以通过 for 循环来列出所有元素。有两种方法可以实现，如下：

**方法1**


```python
for name in names:
    print(name)
```

    James
    Michael
    Emma
    Emily


**方法2**


```python
for i in range(len(names)):
    print(names[i])
```

    James
    Michael
    Emma
    Emily


## 3 列表的操作、函数及方法

### 3.1 list中元素的增、改、删等操作

list是一个可变的有序列表，可以通过添加、修改、删除等操作来操作list中的元素。

**往list中添加元素**

可以通过 `append()` 和 `insert()` 方法来往 list 中添加元素。

其中， `append()` 方法是在 list 的末尾添加元素； `insert()` 是在指定位置添加元素。

如下：



```python
names.append('Jacob')
names
```




    ['James', 'Michael', 'Emma', 'Emily', 'Jacob']




```python
names.insert(1, 'Ava')
names
```




    ['James', 'Ava', 'Michael', 'Emma', 'Emily', 'Jacob']



请注意，上述 `insert()` 方法中， "1" 表示在第2个位置添加新的元素（索引是从0开始的）

**删除list中的元素**

* **用 `pop()` 方法删除list末尾的元素**


```python
names.pop()
```




    'Jacob'




```python
names
```




    ['James', 'Ava', 'Michael', 'Emma', 'Emily']



* **删除指定位置的元素，用 `pop(i)` 方法**


```python
names.pop(0)
```




    'James'




```python
names
```




    ['Ava', 'Michael', 'Emma', 'Emily']



**修改list中的元素**

如果需要修改 list 中的元素， 可以直接通过 list 的索引进行赋值来实现， 如下：


```python
names[2] = 'Lemon'
names
```




    ['Ava', 'Michael', 'Lemon', 'Emily']



### 3.2 列表操作符

列表可以进行 相加 "+" 和 相乘 "\*" 运算，"+" 相当于拼接列表， "\*" 相当于重复列表。

此外，还可以判断元素是否存在于列表中。


```python
# 列表相加
print("列表相加：",[1,2,3]+['a','b'])

# 列表相乘
print("列表相乘：",['a','b']*3)

# 判断元素是否存在于列表中
print("判断元素是否存在于列表中：", 'a' in ['a', 'b'])
print("判断元素是否存在于列表中：", 'a' not in ['a', 'b'])
```

    列表相加： [1, 2, 3, 'a', 'b']
    列表相乘： ['a', 'b', 'a', 'b', 'a', 'b']
    判断元素是否存在于列表中： True
    判断元素是否存在于列表中： False


### 3.3 列表函数&方法

**列表的函数包括：**

* `len(list)`，列表元素个数，即列表的长度
* `max(list)`，返回列表元素最大值
* `min(list)`，返回列表元素最小值
* `list(sep)`，将元组转为列表

**列表的方法**

列表的方法除了前面提到的增、改、删等方法外，还有其他一些方法，如下：

* `list.count(obj)`，统计某个元素在列表中出现的次数
* `list.extend(seq)`，在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表）
* `list.index(obj)`，从列表中找出某个值第一个匹配项的索引位置
* `list.remove(obj)`，移除列表中某个值的第一个匹配项
* `list.sort()`，对原列表进行排序
* `list.reverse()`，对原列表进行反向排序


```python
list1 = [1,2,3,9,6,3]
list2 = [3, 8, 5, 4, 7,4]
tuple1 = (3,9,6)

# 列表的函数
print("列表最大值：",max(list1))
print("列表最小值：",min(list1))
print("将元组转为列表：",list(tuple1))

# 列表的方法
print("count：",list1.count(3))

list1.extend(list2)
print("extend：",list1)

print("index：",list2.index(4))

list2.remove(4)
print("remove：",list2)

list2.sort()
print("sort：",list2)

list2.reverse()
print("reverse：",list2)
```

    列表最大值： 9
    列表最小值： 1
    将元组转为列表： [3, 9, 6]
    count： 2
    extend： [1, 2, 3, 9, 6, 3, 3, 8, 5, 4, 7, 4]
    index： 3
    remove： [3, 8, 5, 7, 4]
    sort： [3, 4, 5, 7, 8]
    reverse： [8, 7, 5, 4, 3]


## 4 list中元素的类型可以多样

同一个 list 中的元素的类型可以是字符串（str）、整型（int）、布尔型（Boolean）、以及嵌套的list等，举例如下：


```python
a_list = ['Lemon', 100, ['a', 'b', 'c', 'd'], True]
a_list
```




    ['Lemon', 100, ['a', 'b', 'c', 'd'], True]




```python
a_list[0]
```




    'Lemon'




```python
a_list[2]
```




    ['a', 'b', 'c', 'd']



在上述的 a_list 中， 第3个元素 (即 alist[2]) 其实本身也是一个 list。

list中的元素也可以是list，这样的话，可以进行多重list的嵌套。

上述 list，也可以按下述方式来理解。


```python
b_list = ['a', 'b', 'c', 'd']
a_list = ['Lemon', 100, b_list, True]
a_list
```




    ['Lemon', 100, ['a', 'b', 'c', 'd'], True]



针对 a_list ， 如果想获取其中元素 b_list 里面的单个元素 “b”，该如何实现呢？

其实，这个类似二维数组，用二维的索引可以获取，如下：


```python
item_b = a_list[2][1]
item_b
```




    'b'



上述 `[2]` 表示获取 a_list 的第3个元素，即 b_list， `[1]` 表示获取 `b_list` 的第2个元素，即“b”

## 5 list 的切片（slices）

前面描述了 list 中单个元素如何获取，如果想获取其中连续的部分元素，该如何实现呢。

这里可以通过切片(slices)的形式来获取部分连续的元素。


```python
c_list = ['James', 'Ava', 'Michael', 'Emma', 'Emily', 'Jacob']
c_list
```




    ['James', 'Ava', 'Michael', 'Emma', 'Emily', 'Jacob']



list 中以切片形式使用时，其结构可参考   `new_list[start: end : step]`

其中 "start" 和 "end" 表示索引位置的开始和结束，选取的元素包含 "start"，**但不包含 "end"**。

"step" 表示步长，默认情况下， "step"为1，演示如下：

**Example-1:**


```python
c_list[1:3]
```




    ['Ava', 'Michael']



example-1 中，  `1:3` 表示切片选取的是第2个元素和第3个元素，即包含索引为1和索引为2的元素。
相当于获取 c_list[1] 和 c_list[2]

**Example-2:**


```python
c_list[::2]
```




    ['James', 'Michael', 'Emily']



example-2 中 "start" 和 "end" 为空的时候，默认是全选，即 "start" 为 0， "end" 为 `len(c_list)-1`。

所以 `c_list[::2]` 表示的是从索引为 0 开始，以步长为2来选择元素。

接下来看以下结合上述两个例子的演示：


```python
c_list[1:3:2]
```




    ['Ava']



**Example-3:**

前面提到，new_list[-1]表示获取最后一个元素， 在切片的步长 "step" 中，也可以是负数，比如 "-1"：


```python
c_list[::-1]
```




    ['Jacob', 'Emily', 'Emma', 'Michael', 'Ava', 'James']



从上述结果可以看出， 当 "step"为 `-1` 时，我们发现是将列表进行了逆序排序。

再看看步长为 "-2" 时的结果：


```python
c_list[::-2]
```




    ['Jacob', 'Emma', 'Ava']



可以这么理解，当步长为**正数**时，是**从左到右以该步长**来获取列表中的元素；

而当步长为**负数**时，是**从右到左以该步长的绝对值**来获取列表中的元素。

**Example-4:**

如果想获取离散的元素，比如想获得第1、2、4个元素，能不能通过离散的索引值来获取呢？

我们先来试验一下：


```python
# c_list[0, 1, 3]
```

```python
c_list[0, 1, 3]

out:
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-30-68d834a60325> in <module>()
----> 1 c_list[0, 1, 3]

TypeError: list indices must be integers or slices, not tuple

```

从运行结果可以之道，直接取离散的索引号是不行的，它提示 list的索引必须是整数或者 slices。

那么，我们有没有方法来获取这些离散的元素呢？

方法肯定是有的，其中一种方法就是使用 列表推导式。


## 6 列表推导式(List Comprehension)

### 6.1 列表推导式的一般情况


列表推导式的一般语法结构：

`new_list = [x for x in iterable]`

其中的 iterable 表示可迭代的对象，包括字符串（str）、列表（list），元组（tuple）、字典（dict）、集合（set），以及生成器（generator）等。

先来看几个简单的例子：


```python
str_list = [x.lower() for x in "Lemon"]
str_list
```




    ['l', 'e', 'm', 'o', 'n']




```python
list_list = [x**2 for x in [1,2,3,4]]
list_list
```




    [1, 4, 9, 16]




```python
tuple_list = [x+2 for x in (1,2,3,4)]
tuple_list
```




    [3, 4, 5, 6]




```python
ge_list = [x for x in range(8)]
ge_list
```




    [0, 1, 2, 3, 4, 5, 6, 7]



### 6.2 两层for循环的列表推导式

列表推导式中，可以同时包含多个for循环，比如同时两个for循环，如下：


```python
[x**2+y for x in range(5) for y in range(4,7)]
```




    [4, 5, 6, 5, 6, 7, 8, 9, 10, 13, 14, 15, 20, 21, 22]



上述结果跟下面的写法结果是一致的：


```python
two_for_list = []

for x in range(5):
    for y in range(4,7):
        two_for_list.append(x**2+y)
print(two_for_list)
```

    [4, 5, 6, 5, 6, 7, 8, 9, 10, 13, 14, 15, 20, 21, 22]


列表推导式中，三层或者三层以上的for循环一般很少用到。

### 6.3 使用两个变量来生成list

列表推导式也可以使用两个或多个变量来生成list，结合字典的使用，举例如下：


```python
d = {'x': '1', 'y':'2', 'z':'4'}
d_list = [k+'='+v for k,v in d.items()]
d_list
```




    ['z=4', 'y=2', 'x=1']



### 6.4 含if语句的列表推导式

列表推导式中还可以引入 if 条件语句，如下：


```python
if_list = [x**2 for x in range(10) if x%2==0]
if_list
```




    [0, 4, 16, 36, 64]



上述列表推导式，如果用普通的for循环来编写的话，内容如下：


```python
if_list_1 = []
for x in range(10):
    if x%2==0:
        if_list_1.append(x**2)

print(if_list_1)
```

    [0, 4, 16, 36, 64]


通过对比，可以看出列表推导式的方式更显得 Pythonic。

当然，如果仅仅是编写上更简洁，可能不一定能显现出 列表推导式的优势，下面我们来对比下上述两种方式的运行效率。


```python
%%timeit

if_list = [x**2 for x in range(10) if x%2==0]
if_list
```

    100000 loops, best of 3: 6.06 µs per loop



```python
%%timeit

def for_loop():
    if_list_1 = []
    for x in range(10):
        if x%2==0:
            if_list_1.append(x**2)
    return if_list_1

for_loop()
```

    100000 loops, best of 3: 6.76 µs per loop


从上面的运行结果进行对比，可以看出列表推导式的运行效率要高于普通的 for循环编写方式。

因为，建议大家使用列表推导式，不仅写法更 Pythonic，且运行效率更高。

 **包含if-else语句的列表推导式**


```python
[x**2 if x%2==0 else x+2 for x in range(10)]
```




    [0, 3, 4, 5, 16, 7, 36, 9, 64, 11]



**包含两个if语句的列表推导式**


```python
[x**2 for x in range(10) if x%2==0 if x%3==0]
```




    [0, 36]



## 7 小结

如果想获取离散的元素，比如想获得第1、2、4个元素，能不能通过离散的索引值来获取呢？

前面已经实践过，直接通过离散的索引值来获取列表中的元素是不行的。

通过列表推导式的学习，我们可以换一种思路，来实现列表中离散的元素的获取，如下：


```python
[c_list[i] for i in [0,1,3]]
```




    ['James', 'Ava', 'Emma']



列表（list）作为python最基础也是最重要的数据类型之一，在python数据分析以及其他用途中有着重要的作用，希望上述内容能对于熟悉list有所帮助。


<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="200">
</div>
