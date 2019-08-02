---
layout: posts
title: "Flask vs Django，Python Web 开发用哪个框架更好?"
teaser:
date: 2019-08-02
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PythonWeb
tags:    
   - Flask
   - Django     
comments: true
show_meta: true
sidebar: right
authors: ["Lemonbit"]
---

{% include alert info='Flask vs Django，Python Web 开发用哪个框架更好?' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# Flask vs Django，Python Web 开发用哪个框架更好?

作者： SeattleDataGuy

翻译： Lemonbit

译文出品： Python数据之道

![img01](/images/posts/Python-Backends-Flask-Versus-Django/1.jpeg)

Photo by Jefferson Santos on Unsplash

您是否正在考虑使用 Python 开发网站？ 然后你可能听说过 Django 和 Flask。 这两个是 Python 最流行的 Web 框架（当然还有更多）。 现在的问题是你使用哪一个更合适？

本文旨在简要介绍两种框架的工作原理，它们的相同点和不同点，以及在选择框架时应考虑的一些要点。

## 一、简要概览

基于 Python 的主要应用程序是使用命令行界面创建的，可以在命令提示符或 shell 脚本上运行。 当然，人们希望使用 Python 做更多事情，而不仅仅是自动执行任务和管理工作流程。 因此，Web 框架顺势而生。

### Django

![img02](/images/posts/Python-Backends-Flask-Versus-Django/2.jpeg)

Django 是一个 Python Web 框架，适合具有开发时间限制要求的完美主义者。 Django 提供功能齐全的模型 - 视图 - 控制器框架。 它的开发基于“内置电池的方法”（译者注：Django 自带全套工具，可以开箱即用），使开发人员无需第三方库和工具即可创建网站。 随着 2005 年 7 月 15 日的第一次发布， Django 由 Django 软件基金会（DSF）开发和管理。 它是免费且开源的，已经发布了许多个版本。

### Flask

![img03](/images/posts/Python-Backends-Flask-Versus-Django/3.png)

Flask是一个轻量级的 Python 框架（微框架），它基于一次完成一件事并做得很好。 随着 2010 年 4 月 1 日的首次发布，Flask 由奥地利开发商 Armin Ronacher 开发。 正如其文档中所描述的那样，微观框架意味着 Flask 旨在保持其轻量级的简单性和可扩展的使用。 Flask 的真正力量在于它具有灵活性。

## 二、 Flask 与 Django 两者之间的对比

下面是 Django 和 Flask 的深入比较，从如下几方面来对比：

- 受欢迎度：使用率和案例
- 入门文档和导航的易用程度
- 模板引擎和系统
- 路由系统
- 用户灵活性
- 管理系统的可用性
- 开发速度

### 受欢迎度：使用率和案例

几个顶级网站使用 Django 和 Flask。 来自 GitHub 和其他网站的统计数据表明，Django 比 Flask 更受欢迎。 这种受欢迎程度仅限于开发人员使用 Django 强大的功能来快速构建和部署复杂的 Web 应用程序。 同样，开发人员使用 Flask 来加速使用固定内容的网站的开发。 由 Django（Bitbucket，Eventbrite，Instagram，Pinterest等）和 Flask（LinkedIn，Netflix，Twilio，Uber）提供支持的一些知名项目积极使用这些框架。

### 用户灵活性

两个框架之间最大的区别之一是灵活性。

Django 的 **内置电池** 概念有助于开发人员在没有第三方库和工具的情况下创建各种 Web 应用程序。 但是，Django 缺乏对提供的模块进行更改的选项。 因此，开发人员使用内置功能创建 Web 应用程序。 这意味着如果开发人员想要为 Django 已经提供的功能使用不同的库，那就不容易了。

相反，Flask 使用其可扩展的 Web 框架，使用各种 Web 开发库和工具实现 Web 应用程序的灵活开发。 这使得更有经验的开发人员可以自由地使用他们熟悉的库和数据库进行即插即用。

你不会被框架强迫你使用的东西所困扰。 相反，您可以转向您熟悉的技术组件。

### 开发文档和导航的易用程度

Flask 提供了大量文档，其中包括部署，安装，快速入门说明和详细教程。 使用 Python的 pip 安装 Flask 很容易。

```python
pip install flask
```

上面的 pip 命令安装 Flask 及其基本软件包。 在 Flask 上运行代码非常简单（例如，显示 “Hi Welcome”）。 它需要的只是创建 Flask 类的实例，然后是显示 “Hi Welcome” 字符串的路由。

```python
from flask import Flask
app = Flask(__name__)@app.route("/")
    def hi_welcome():
        return "Hi Welcome"

if __name__ == "__main__":
    app.run()
```

Django 的文档比 Flask 更广泛，由于它的复杂性，这是预料之中的。  Djang 也可以使用 pip 快速安装。

```python
pip install Django
```

在 Django 中运行代码涉及使用内置命令来创建项目，以及另一个用于创建应用程序的命令。 它的启动命令很方便，因为它使用了一个现成的目录结构。 但是这个视频解释得最好：

### 路由系统

任何 Web 应用程序或网站都必不可少的是路由系统（routing system），它可以创建 URL 并确定加载 URL 时显示的内容。

Flask采用简单、轻量级的Web框架，路由由 `route（）` 装饰器完成。 使用上面的 “Hello World” 示例，第一行是装饰器。 装饰器是将可调用（类）作为参数的函数，然后在返回之前进行修改。 使用的字符串还告诉 Flask 什么 URL 触发其功能。 例如，使用 /（http：//127.0.0.1：5000 / \），它会在使用浏览器访问 Flask 时加载第一页。

对于 Django ，它采用了内置电池的方法，这使开发人员更容易完成基本的 Web 开发任务，如数据库模式迁移，URL 路由和用户身份验证。 自定义 Web 应用程序还体验 Django 中的进程加速，因为它提供了引导工具，内置模板引擎和 ORM 系统。 使用 `urls.py` 文件处理路由，该文件是在运行内置的 `djangoadmin startproject` 命令时创建的。 要定义路由，需要从 Django 导入 URL 方法并创建实例，指定每个参数（关键字参数，现有 URL 模块和正则表达式字符串）。 管理站点的内置 Django URL 默认位于 `urls.py` 文件中。 它告诉 Django 你在哪里加载来自指定模块的 URL 。

### 管理系统的可用性

![img](/images/posts/Python-Backends-Flask-Versus-Django/4.png)

Django 使用遵循对象关系映射器（ORM）目录结构和数据库系统的管理系统。 当使用 Django 进行开发时，它增加了一致性体验，因为多个项目具有相同的目录结构。

Flask 缺少这些功能，可能需要您安装自定义模块才能将 ORM 用作开发人员的可选项。 这些选项包括 MongoDB，SQLAlchemy，SQLite 等的使用。 如您所见，这是灵活性和易用性之间的权衡。 Django 附带了构建网站所需的大部分技术组件。 Flask 更纯粹只是框架，允许最终用户做出更多决定。

### 开发速度

众所周知，Django 框架可以为复杂的 Web 应用程序提供快速的开发速度。 由于它提供了完整的功能，因此它提供了所有必要的实施工具。

Flask 的简单性允许经验丰富的开发人员在短时间内完成较小的应用程序。  Django 框架的一个显着优势是提供了一个活跃的社区，旨在帮助您扩展应用程序的解决方案或使用有用的内容使您的工作更轻松。 Flask 社区目前没那么大，因此找到有用的信息并不容易。

### 模板引擎和系统

所有 Web 应用程序都有一个支持用户交互的前端（用户界面）。 由于 Web 应用程序不是静态的，因此它们采用不同的方法动态生成 HTML。 Django 和 Flask 提供令人兴奋的模板引擎。

Flask 是使用 Jinja2 模板引擎开发的。 它是一个现代且设计友好的模板，允许开发人员在集成的沙盒环境的帮助下模拟动态 Web 应用程序。 Jinja2 模板包含变量和标签。 Flask 模板的另一个关键特性是模板继承。 以下是一些常用的 Jinja2 语法示例：

```
comments: {# … #}

variables: {{ … }}

statements: {% … %} (Similar to normal programming, Jinja2
statements find usage in a variety of cases, like if-else
statements, imports, loops, and macros).
```

Django 使用内置的模板引擎，允许开发人员毫不费力地定义 Web 应用程序的面向用户的层。 此外，开发人员可以使用 Django 模板语言（DTL）编写模板来创建自定义用户界面开发。 Django 中的模板语法包括：

```
single-line comments: {# … #}

multi-line comments: {% comment %} … {% endcomment %}

filters on variables: {{ variable|filter }}

tags: {% … %}

variables: {{ … }}
```

jinja2 模板从 Django 模板语言中获取灵感，因此语法相似。 Django 模板具有模板继承功能，更多信息可以在 Django 模板官方文档中看到。

## 总结

要开始使用 Flask 和 Django 框架，必须更深入地了解基础知识。 每个框架所附带的差异和好处在于您希望实施哪种项目。 主要对比包括：

**Flask** 提供灵活性，简单性和细粒度控制。 Flask 毫无疑问，让您决定如何实现应用程序。

**Django** 为您的 Web 应用程序开发提供管理面板，数据库接口，目录结构和 ORM 的全面体验。

我们希望这有助于您对所选框架做出明确决定。

祝你未来的项目顺利！

>来源：
https://medium.com/better-programming/python-backends-flask-versus-django-5de314fa70ff

---

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PythonDataLab），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>