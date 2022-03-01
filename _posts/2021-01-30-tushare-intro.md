---
layout: posts
title: "财经数据神器 Tushare，股票数据全搞定"
teaser:
date: 2021-01-30
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PyFinance
tags:    
   - Tushare
   - recommend 
comments: true
show_meta: true
sidebar: right
authors: ["阳哥"]
---

{% include alert info='财经数据神器 Tushare，股票数据全搞定' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# 财经数据神器 Tushare，股票数据全搞定

大家好，我是 阳哥 。

## 01 背景

前一段时间， 阳哥发了一期视频，分享了 Plotly 和 Dash 在投资领域的应用案例。

- [传送门：Plotly 和 Dash 在投资领域的应用](https://mp.weixin.qq.com/s/VopPrpe_64j5za4AYkmUTg)

看完视频后，有同学提了如下的一个问题：

![问题](https://tva1.sinaimg.cn/large/008eGmZEgy1gn3j3yld83j30u00cmabf.jpg)

关于财经数据，有多个Python库可以供咱们选择，其中 tushare 是国内较早开始发布财经数据的社区，其内容比较完善，今天咱们来重点介绍下 Tushare 。

Tushare 是一个金融大数据平台，数据内容包含股票、指数、基金、期货、债券、外汇、行业大数据等，同时包括了数字货币行情等区块链数据，为各类金融投资和研究人员提供适用的数据和工具，概览如下：

![tushare概览](https://tva1.sinaimg.cn/large/008eGmZEgy1gn3j7d8wnwj30is0ya45g.jpg)

阳哥绘制了一张完整的思维导图，截图如下：

![tushare思维导图](https://tva1.sinaimg.cn/large/008eGmZEgy1gn3r6vz09nj30u016r7mw.jpg)

>注：文末提供了上图的高清pdf版的获取方式。

下面咱们一起来了解下这个高大上的财经数据神器 Tushare 。

## 02 使用 Tushare

Tushare 平台的数据，已全面升级到 tushare pro 了，通常情况下，还是称之为 tushare。

想使用 tushare 中的数据和功能，首先需要进行注册，获得一份 token （一串字母和数字组成的文本），然后才可以获取到数据，大家可以通过以下链接来注册：

[tushare注册地址](https://tushare.pro/register?reg=129033)

### 数据获取

在 `tushare` 中注册后，通过 “个人主页”——“接口TOKEN” 可以找到自己的 token 值，界面如下：

<!-- ![tushare](images/posts/2020-04-27-plotly-txt/2.png) -->

![tushare接口TOKEN](https://tva1.sinaimg.cn/large/008eGmZEgy1gmwqwnfvdsj30l60b8aar.jpg)

复制 token 值，然后在代码中进行如下设置：

```python
# 设置 token
# tushare 注册地址： https://tushare.pro/register?reg=129033
# 以上方法只需要在第一次或者token失效后调用，完成调取tushare数据凭证的设置，正常情况下不需要重复设置。
ts.set_token('你的token值')

pro = ts.pro_api()
```

在设置好 token 值后，我们就可以开始获取数据。

## 03 沪深股票

作为国内的投资，首先要接触的当然就是 A股，也就是沪深两市的股票。Tushare 在沪深股票方面，提供了大量的功能和接口，可以供咱们来使用，主要包括 基础数据、行情数据、财务数据和市场参考数据四个子板块。

![沪深股票](https://tva1.sinaimg.cn/large/008eGmZEgy1gn51pv2rmdj30q01f8qeq.jpg)

每个细分板块都提供一些特定的接口来获取数据，下面分享部分用的较多的功能。

### 获取交易日历信息

通过 `trade_cal` 可以便捷的获取所有的交易日的日期信息，代码如下：

```python
df = pro.trade_cal(exchange_id='', start_date='20180101', 
                   end_date='', fields='pretrade_date',
                  is_open='0')

df
```

这个信息获取来有什么用呢？

在下面的文章中，曾提到过在绘制股票交易图表时，希望能够隐藏周六日和节假日，只显示交易日的交易信息，有了 tushare 的这个功能，就可以快速的对沪深股市进行设置。即从全部日期中剔除交易日，就是想隐藏的周六日和节假日的范畴了。

- [Plotly中绘制三种经典的股票交易图表](https://mp.weixin.qq.com/s/1v3h5-5fNVk7cBdaRKzJkg)


### 股票列表

获取股票信息列表也是我们经常需要获取的数据之一，在 tushare 中可以有几种方法来获取，包括 `pro.stock_basic` 和 `pro.query`。

![股票列表](https://tva1.sinaimg.cn/large/008eGmZEgy1gn3p1zxmmtj30xq0r8tdo.jpg)

通过这个功能，可以快速的了解到，当前沪深两市，有4000多只股票在正常交易。

也可以通过下面的方法来获取这些信息：

```python
#查询当前所有正常上市交易的股票列表
data = pro.query(
    'stock_basic', exchange='', list_status='L', 
    fields='ts_code,symbol,name,area,industry,list_date,list_status')
data
```

### 股票的日线行情数据

获取日线行情数据，这恐怕是平时使用最多的功能了。在 tushare 中，日线行情数据的接口是 `daily` ，目前在 tushare 中可以通过三种方式来获取股票的日线行情数据。

1. `pro.daily()` 方式

可以使用下面的代码来获取茅台股份的日线行情数据，如下：

![日线行情](https://tva1.sinaimg.cn/large/008eGmZEgy1gn3p6epkqcj31cm0sen3e.jpg)

此外，还可以同时获取多只股票的日线行情数据，代码如下：

```python
# 获取多只股票日线行情数据
# 茅台，600519.SH
# 300433 蓝思科技
data = pro.daily(
    ts_code='600519.SH,300433.SZ',
    start_date='20180101',
    end_date='20210120')
data
```

`pro.daily()` 方法还可以获取某个交易日，整个沪深市场所有股票的日线行情数据，如下：

```python
# 获取历史某天全部股票的交易数据
df = pro.daily(trade_date='20210120')
df
```

2. `pro.query()` 方法

第二种获取日线行情数据的方式是使用 `pro.query()` ，示例如下：

```python
# 获取单只股票日线行情数据
# 300433 蓝思科技
data = pro.query('daily', ts_code='300433.SZ', 
                 start_date='20180101', end_date='20210120')

data
```

3. `ts.pro_bar()` 方法

此外，tushare 还提供了通用行情接口 `pro_bar` ，目前整合了股票（未复权、前复权、后复权）、指数、数字货币、ETF基金、期货、期权的行情数据。可以通过这个方法实现多种标的数据的获取。

通用行情接口，适合多种数据的获取。

```python
# 获取多个股票的数据，有 bug？
data = ts.pro_bar(
    ts_code='600519.SH',
    start_date='20180101',
    end_date='20210120',
    adj='qfq',
)
data
```

上面的参数 `adj` ，复权因子，只适用于股票交易数据， `qfq` 表示的是 前复权。

## 04 指数数据

除了股票数据的获取，通常情况下，还会经常关注指数的行情，比如上证综合指数、沪深300指数等。Tushare 提供的指数分类信息如下：

![指数](https://tva1.sinaimg.cn/large/008eGmZEgy1gn51t5gf7dj30s60xan43.jpg)

### 指数基本信息

通过接口 `index_basic` 可以获取不同市场指数的基本信息，比如可以设置参数 `market='CSI` 来获取中证指数的基础信息。

![指数基本信息](https://tva1.sinaimg.cn/large/008eGmZEgy1gn3pdsurn7j317m0p8tdx.jpg)

目前，通过 tushare ，可以快速的获取以下市场的基本信息：

![市场分类](https://tva1.sinaimg.cn/large/008eGmZEgy1gn3pfajqsaj31ai0isdhf.jpg)

### 指数日线行情数据

指数日线行情数据可以通过 `index_daily` 接口 或 通过行情接口 来获取数据。

1. `index_daily` 接口

![指数日线行情](https://tva1.sinaimg.cn/large/008eGmZEgy1gn3plh00fhj31hs0o4wkl.jpg)

`index_daily` 方法可以设置获取数据的开始日期和结束日期，如下：

```python
# 获取沪深300指数的日线行情
# 设置开始和结束日期
df = pro.index_daily(ts_code='000300.SH',
                     start_date='20180101', 
                     end_date='20210120')
df
```

2. 通用行情接口

同样的，可以通过通用行情接口来获取指数的数据，设置参数 `asset=I` ，表示获取的是指数数据（I：index），代码如下：

```python
df = ts.pro_bar(ts_code='000300.SH',
                asset='I',start_date='20180101', end_date='20210120')
df
```

## 05 积分

上面提到的这些接口和功能还只是很小一部分。

Tushare 提供的功能确实很多，而且基础功能都是免费提供的。在本文刚开始的时候，跟大家提到了使用 tushare 需要进行注册。

为啥需要注册呢，因为 tushare 进行了不同积分权限的设置，不同的积分，获取数据的权限是不一样的。

上面跟大家介绍的常用的接口和功能，大部分是注册之后就可以使用的。如果想更高频率的获取数据，或者想获取公募基金数据、港股、美股数据等，就需要提高积分权限了。

目前， tushare 官方给的一些提高积分的方法，有那么几种，一种是金额赞助，可以去换积分。另一种是官方给了一些推广的途径，去帮它推广也可以获取积分。

比如大家通过本文的 “**阅读原文**” 来注册 tushare 的话，我可以获得一丢丢的积分，哈。

其实，也是需要理解 tushare官方 的这种做法的，毕竟天下没有免费的午餐。这么好用的工具，如果完全免费，tushare官方估计也很难维护这样一个社区。

阳哥 也给大家提供了一份高清晰的pdf版本，想要获取的同学，可以在公众号 **「Python数据之道」** 后台回复 `code` 来获取。

### 拓展阅读

阳哥 使用 Tushare 作为工具之一进行应用，有兴趣的同学可以点击链接前往：

- [视频：Plotly 和 Dash 在投资领域的应用](https://mp.weixin.qq.com/s/VopPrpe_64j5za4AYkmUTg)

- [视频：Plotly中绘制股票交易图表](https://mp.weixin.qq.com/s/1TUB2G-xavNm796uXUnRfQ)

- [Plotly中绘制三种经典的股票交易图表](https://mp.weixin.qq.com/s/1v3h5-5fNVk7cBdaRKzJkg)


---

对我的文章感兴趣的朋友，可以关注我的微信公众号「Python数据之道」（ID：PyDataLab），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
