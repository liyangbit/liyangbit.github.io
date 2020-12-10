---
layout: posts
title: "Python中必须知道的5对魔术方法"
teaser:
date: 2020-09-30
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PythonBasics
tags:    
   - Python基础 
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---


{% include alert info='Python中必须知道的5对魔术方法' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# Python中必须知道的5对魔术方法

>原文作者：Yong Cui
>
>翻译：Lemon
>
>译文出品：Python数据之道


## 简介

在使用Python命名函数时，我们可以使用下划线以及字母和数字。 在单词之间使用下划线时，它们没有多大意义，它们只是通过在单词之间创建空格来提高可读性。 这就是蛇形命名方式。 例如，`calculate_mean_score` 比 `calculatemeanscore` 更易于阅读。 您可能已经知道，除了这种使用下划线的常用方式之外，我们还为函数名称加上一个或两个下划线（例如，`_func`，`__func`），以表示这些函数供类或模块内的私有使用。 没有下划线前缀的名称被认为是公共API。

下划线在函数命名中的另一种用法是魔术方法（magic methods），也称为特殊方法。 具体来说，我们在函数名称之前放置两个下划线，在函数名称之后放置两个下划线-类似于`__func__`。 由于使用了双下划线，因此某些人将特殊方法称为 “dunder方法” 或简称为 “dunders” 。 在本文中，我想回顾五对紧密相关的常用魔术方法，每对方法代表一个 Python 概念。

> 「Python数据之道」注：dunder 是 double  underscore 的缩写，即双下划线。

## 1. 实例化: `__new__` 和 `__init__`

在学习了 Python 数据结构的基础知识（例如字典，列表）之后，您应该已经看到了一些定义自定义类的示例，在这些示例中，您第一次接触到了魔术方法 `__init__`。 此方法用于定义实例对象的初始化行为。 具体来说，在 `__init__` 方法中，您想要为创建的实例对象设置初始属性。 这是一个简单的示例：

```python
class Product:
    def __init__(self, name, price):
        self.name = name
        self.price = price
```

当我们使用`__init__`方法时，我们不会直接调用它。 取而代之的是，`__init__`方法成为该类的构造函数方法的构建基础，该类的构造函数与`__init__`方法具有相同的功能签名。 例如，要创建一个新的`Product` 实例，请使用以下代码：

```python
product = Product("Vacuum", 150.0)
```

与`__init__`方法最接近的是 `__new__` 方法，我们通常不会在自定义类中实现该方法。 本质上，`__new__` 方法实际上创建了实例对象，该实例对象被传递给 `__init__` 方法以完成初始化过程。

换句话说，构造新的实例对象（称为实例化的过程）涉及依次调用 `__new__` 和 `__init__`方法。

以下代码展示了这样的一系列反应：

```python
>>> class Product:
...     def __new__(cls, *args):
...         new_product = object.__new__(cls)
...         print("Product __new__ gets called")
...         return new_product
... 
...     def __init__(self, name, price):
...         self.name = name
...         self.price = price
...         print("Product __init__ gets called")
... 
>>> product = Product("Vacuum", 150.0)
Product __new__ gets called
Product __init__ gets called
```

## 2. 字符串（String）的表现形式: `__repr__` 和 `__str__`

这两种方法对于为自定义类设置字符串表现形式都很重要。 在解释它们之前，让我们快速看一下下面的实现过程：

```python
class Product:
    def __init__(self, name, price):
        self.name = name
        self.price = price

    def __repr__(self):
        return f"Product({self.name!r}, {self.price!r})"

    def __str__(self):
        return f"Product: {self.name}, ${self.price:.2f}"
```

`__repr__` 方法应该返回一个字符串，该字符串显示如何创建实例对象。具体来说，可以将该字符串传递给`eval（）`来重新构造实例对象。 以下代码段向您展示了这样的操作：

```python
>>> product = Product("Vacuum", 150.0)
>>> repr(product)
"Product('Vacuum', 150.0)"
>>> evaluated = eval(repr(product))
>>> type(evaluated)
<class '__main__.Product'>
```

`__str__` 方法可以返回有关实例对象的更多描述。应该注意的是，`print()` 函数使用`__str__`方法来显示与实例相关的信息，如下所示：

```python
>>> print(product)
Product: Vacuum, $150.00
```

尽管这两种方法都应返回字符串，但是 `__repr__` 方法通常是供开发人员使用的，因此我们希望显示实例化信息，而 `__str__` 方法是针对常规用户的，因此我们希望显示更多的信息。

## 3. 迭代: `__iter__` 和 `__next__`

我们可以使代码自动化的一个关键操作是为我们重复执行一项工作，该工作的实现涉及到 for 循环作为逻辑流程。 就相关对象而言，它可以在 for 循环中使用。 for 循环的最基本形式如下所示：

```python
for item in iterable:
    # Operations go here
```

在底层，可迭代对象转换为迭代器，该迭代器为每个循环显示可迭代对象。 一般来说，迭代器是 Python 对象，可用于渲染要迭代的变量。转换是通过实现 `__iter__` 特殊方法来完成的。 另外，检索迭代器的下一项涉及 `__next__` 特殊方法的实现。 让我们继续前面的示例，并使我们的 Product 类作为 for 循环中的迭代器工作：

```python
>>> class Product:
...     def __init__(self, name, price):
...         self.name = name
...         self.price = price
... 
...     def __str__(self):
...         return f"Product: {self.name}, ${self.price:.2f}"
... 
...     def __iter__(self):
...         self._free_samples = [Product(self.name, 0) for _ in range(3)]
...         print("Iterator of the product is created.")
...         return self
... 
...     def __next__(self):
...         if self._free_samples:
...             return self._free_samples.pop()
...         else:
...             raise StopIteration("All free samples have been dispensed.")
... 
>>> product = Product("Perfume", 5.0)
>>> for i, sample in enumerate(product, 1):
...     print(f"Dispense the next sample #{i}: {sample}")
... 
Iterator of the product is created.
Dispense the next sample #1: Product: Perfume, $0.00
Dispense the next sample #2: Product: Perfume, $0.00
Dispense the next sample #3: Product: Perfume, $0.00
```

如上所示，我们创建了一个对象列表，该对象列表在 `__iter__` 方法中保存了免费样本 （free samples），这些样本为自定义类实例提供了迭代器。 为了实现迭代行为，我们通过提供免费样本列表中的对象来实现 `__next__` 方法。 当我们用完免费样本时，迭代结束。

## 4. 上下文管理器: `__enter__` 和  `__exit__`

当我们使用 Python 处理文件对象时，你遇到的最常见的语法可能是这样的：

```python
with open('filename.txt') as file:
    # Your file operations go here
```


 `with` 语句的使用被称为上下文管理器技术。 具体来说，在上面的文件操作示例中， `with` 语句将为文件对象创建一个上下文管理器，并且在文件操作之后，上下文管理器将帮助我们关闭文件对象，以便共享资源（即文件） 可以用于其他进程。

因此，通常来说，上下文管理器是 Python 对象，它们为我们管理共享资源，例如打开和关闭。 没有它们，我们必须手动管理它们，这很容易出错。

为了通过自定义类实现这种行为，我们的类需要实现`__enter__` 和 `__exit__` 方法。 `__enter__` 方法设置了上下文管理器，该上下文管理器为我们进行操作准备了所需的资源，而 `__exit__` 方法则是清理应释放的所有已使用资源，以使其可用。 让我们考虑下面的简单示例，其中包含先前的 “ Product” 类：

```python
>>> class Product:
...     def __init__(self, name, price):
...         self.name = name
...         self.price = price
... 
...     def __str__(self):
...         return f"Product: {self.name}, ${self.price:.2f}"
... 
...     def _move_to_center(self):
...         print(f"The product ({self}) occupies the center exhibit spot.")
... 
...     def _move_to_side(self):
...         print(f"Move {self} back.")
... 
...     def __enter__(self):
...         print("__enter__ is called")
...         self._move_to_center()
... 
...     def __exit__(self, exc_type, exc_val, exc_tb):
...         print("__exit__ is called")
...         self._move_to_side()
... 
>>> product = Product("BMW Car", 50000)
>>> with product:
...     print("It's a very good car.")
... 
__enter__ is called
The product (Product: BMW Car, $50000.00) occupies the center exhibit spot.
It's a very good car.
__exit__ is called
Move Product: BMW Car, $50000.00 back.
```

如你所见，当实例对象嵌入在 with 语句中时，将调用 `__enter__` 方法。 当在 with 语句中完成该操作时，将调用 `__exit__` 方法。

但是，应该注意，我们可以实现`__enter__`和`__exit__`方法来创建上下文管理器。 使用上下文管理器“装饰器”函数，可以更轻松地完成此操作。

## 5. 更精细的属性访问控制: `__getattr__` 和 `__setattr__`

如果您有其他语言的编程经验，则可能已习惯于为实例属性设置显式的 getter 和 setter 。 在 Python 中，我们不需要为每个单独的属性使用这些访问控制技术。 但是，有可能通过实现`__getattr__`和`__setattr__`方法来进行控制。 具体来说，访问实例对象的属性时将调用 `__getattr__` 方法，而当我们设置实例对象的属性时将调用 `__setattr__` 方法。

```python
>>> class Product:
...     def __init__(self, name):
...         self.name = name
... 
...     def __getattr__(self, item):
...         if item == "formatted_name":
...             print(f"__getattr__ is called for {item}")
...             formatted = self.name.capitalize()
...             setattr(self, "formatted_name", formatted)
...             return formatted
...         else:
...             raise AttributeError(f"no attribute of {item}")
... 
...     def __setattr__(self, key, value):
...         print(f"__setattr__ is called for {key!r}: {value!r}")
...         super().__setattr__(key, value)
... 
>>> product = Product("taBLe")
__setattr__ is called for 'name': 'taBLe'
>>> product.name
'taBLe'
>>> product.formatted_name
__getattr__ is called for formatted_name
__setattr__ is called for 'formatted_name': 'Table'
'Table'
>>> product.formatted_name
'Table'
```

每当我们尝试设置对象的属性时，都会调用 `__setattr__` 方法。 若要正确使用它，必须通过使用 `super()` 使用超类方法。 否则，它将陷入无限递归。

设置好 formatted_name 属性后，该属性将成为 `__dict__` 对象的一部分，因此不会调用`__getattr__`。

附带说明一下，还有另一种与访问控制紧密相关的特殊方法称为`__getattribute__`，它类似于`__getattr__`，但是每次访问属性时都会被调用。 在这方面，它类似于`__setattr__ `，同样，你应使用super（）实现 `__getattribute__` 方法，以避免无限递归错误。

## 小结

在本文中，我们回顾了五对重要的特殊方法，通过它们我们学习了有关的五个 Python 概念。 我希望您对这些概念以及如何在自己的 Python 项目中使用特殊方法有更好的理解。

---

原文作者：Yong Cui

来源：

https://medium.com/better-programming/5-pairs-of-magic-methods-in-python-you-should-know-f98f0e5356d6

---

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataLab），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
