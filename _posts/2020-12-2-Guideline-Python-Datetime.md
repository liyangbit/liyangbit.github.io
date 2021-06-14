---
layout: posts
title: "Python时间模块Datetime使用指南！"
teaser:
date: 2020-12-02
header:
   image_fullwidth: "image-head.jpg"
categories:
   - PythonBasic
tags:    
   - PythonBasic
comments: true
show_meta: true
sidebar: right
authors: ["阳哥"]
---

{% include alert info='Python时间模块Datetime使用指南！' %}

<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

# Python时间模块Datetime使用指南！

## 前言

在用 Python 进行数据处理，尤其是时间序列数据的处理，经常会涉及处理时间或日期的地方，有些看似简单的问题，经常会混淆，甚至被困住。

>本文分享的内容来自 阳哥 整理的《Python时间使用指南》，完整版的可以通过文末来获取。

首先要介绍的，是大家平时用的较多的 datetime 模块,它属于 Python 的内置模块，功能相对较全。

针对 `Datetime` 模块， 《Python时间使用指南》从构建时间对象实例、时间转换、时间对象的运算三个方面进行了梳理，共涉及 13 个知识点，整理的大纲如下：

![datetime](/images/posts/20201018-guideline-time/datetime.png)

## 构建时间对象实例


```python
import datetime
# from datetime import date, datetime,time,timedelta,tzinfo
import time
```

时间实例的构造包括 日期（如 2020年10月12日），时间（如 20点10分01秒），或者是包含 date 和 time 的 `datetime` （如 2020年10月12日20点10分01秒），下面阳哥来跟大家介绍下具体的构造过程。

### 日期（date）实例的构造

date 是一个理想化的简单型日期，属性有 `year` , `month` , `day` 。


```python
# 构造日期 date 实例
d1 = datetime.date(2020,10,12)
d1
```




    datetime.date(2020, 10, 12)




```python
# 构造日期 date 实例
d1 = datetime.date(2020,10,12)
print(d1)
```

    2020-10-12


除了上面的构造方式，在 date 实例化的时候，还可以通过 `date.today()` 来构造当天的日期。


```python
datetime.date.today()
```




    datetime.date(2020, 10, 14)



`date` 类型的日期，可以通过 `.year` , `.month` , `.day` 来获取日期所属的 年份，月份，和具体的日期号，这几个方法在数据分析中经常会用到。


```python
# 获取日期所属的 年份，月份，和具体的日期号

print(f'日期所属的年份为：{d1.year}')
print(f'日期所属的月份为：{d1.month}')
print(f'日期具体的日期号为：{d1.day}')
```

    日期所属的年份为：2020
    日期所属的月份为：10
    日期具体的日期号为：12


### 时间 time 实例的构造

time 是一个独立于任何特定日期的理想化时间，其属性有 `hour`，`minute`， `second` ， `microsecond` 和 `tzinfo` 。


```python
# 构造时间 time 实例
t1 = datetime.time(20,10,1)
t1
```




    datetime.time(20, 10, 1)




```python
# 获取日期所属的 年份，月份，和具体的日期号

print(f'time 所属的小时为：{t1.hour}')
print(f'time 所属的分钟为：{t1.minute}')
print(f'time 所属的秒为：{t1.second}')
```

    time 所属的小时为：20
    time 所属的分钟为：10
    time 所属的秒为：1


### datetime 实例的构造

datetime 是日期和时间的结合，其属性有 `year`，`month`，`day`，`hour`，`minute`， `second` ， `microsecond` 和 `tzinfo` 。


```python
# 构造时间 datetime 实例
dt1 = datetime.datetime(2020,10,11,20,10,1)
dt1
```




    datetime.datetime(2020, 10, 11, 20, 10, 1)



除了上面的构造方式，在 datetime 实例化的时候，还有其他的一些方式，包括使用 `datetime.now()` 和 `datetime.today()`，以及在 date 的基础上使用 `combine` 方法等。


```python
dt2 = datetime.datetime.now()
dt2
```




    datetime.datetime(2020, 10, 14, 15, 12, 20, 303269)




```python
dt3 = datetime.datetime.today()
dt3
```




    datetime.datetime(2020, 10, 14, 15, 12, 20, 308733)




```python
dt4 = datetime.datetime.combine(d1,t1)
dt4
```




    datetime.datetime(2020, 10, 12, 20, 10, 1)



通过 datetime 的实例化，是我们使用时间是经常用到的方法，在日常使用过程中，我们可能只需要具体到某天，或者只需要具体到某天的某个时间点，这时候，也可以通过 datetime 的一些方法来实现。


```python
# 从 datetime 来获取日期
dt4.date()
```




    datetime.date(2020, 10, 12)




```python
# 从 datetime 来获取具体的时间点
dt4.time()
```




    datetime.time(20, 10, 1)



同样的 `datetime` 类型的时间，可以通过 `.year` , `.month` , `.day` 来获取日期所属的 年份，月份，和具体的日期号。


```python
# 获取日期所属的 年份，月份，和具体的日期号

print(f'日期所属的年份为：{dt4.year}')
print(f'日期所属的月份为：{dt4.month}')
print(f'日期具体的日期号为：{dt4.day}')
```

    日期所属的年份为：2020
    日期所属的月份为：10
    日期具体的日期号为：12


还有一个可能涉及到的时间是获取某天属于星期几，可以通过 `weekday()` 和 `isoweekday()` 方法来实现。


```python
# 从 datetime 来获取日期是星期几
# 使用 weekday 方法
# 数字从0开始，0代表星期一，1代表星期二，以此类推
dt4.weekday()
```




    0




```python
# 从 datetime 来获取日期是星期几
# 使用 isoweekday 方法
# 数字从1开始，1代表星期一，2代表星期二，以此类推
dt4.isoweekday()
```




    1



datetime 还有一种方法，在这里也值得介绍下，如果 datetime 的值由于某些原因弄错了，我们也可以通过 `replace()` 方法来进行更正。这个方法在进行数据清洗的时候会有用。

replace 方法的参数如下：

`datetime.replace(year=self.year, month=self.month, day=self.day, hour=self.hour, minute=self.minute, second=self.second, microsecond=self.microsecond, tzinfo=self.tzinfo, * fold=0)`


```python
dt5 = dt4.replace(year=2019)
dt5
```




    datetime.datetime(2019, 10, 12, 20, 10, 1)



### timedelta 对象的构造

timedelta 对象表示两个 date 或者 time 或者 datetime 的时间间隔。

`class datetime.timedelta(days=0, seconds=0, microseconds=0, milliseconds=0, minutes=0, hours=0, weeks=0)`

所有参数都是可选的并且默认为 0。 这些参数可以是整数或者浮点数，也可以是正数或者负数。

只有 days, seconds 和 microseconds 会存储在内部。 参数单位的换算规则如下：

- 1毫秒会转换成1000微秒。

- 1分钟会转换成60秒。

- 1小时会转换成3600秒。

- 1星期会转换成7天。


```python
delta = datetime.timedelta(weeks=3,
                  days=10,
                  hours=6,
                  minutes=50,
                  seconds=30,
                  microseconds=1000,
                  milliseconds=10000,
                 )

delta
```




    datetime.timedelta(days=31, seconds=24640, microseconds=1000)



### tzinfo 介绍

`datetime.tzinfo` 返回 datetime 对象的时区，前提是在创建 datetime 对象时需传入 tzinfo 参数，如果没有传入则返回值为 None 。


```python
# 如果没有 pytz 库，则需要自行安装
import pytz

sh = pytz.timezone('Asia/Shanghai')
d_tz = datetime.datetime(2020,10,12,hour=8,tzinfo=sh)

d_tz.tzinfo
```




    <DstTzInfo 'Asia/Shanghai' LMT+8:06:00 STD>



## 时间转换

时间的三种存在方式：时间对象，时间字符串，时间戳。

时间对象，比如前面介绍的 `date` 、 `datetime` 、 `time` 对象等；时间字符串，如： "2020-10-12"；时间戳，如 time.time() 返回的就是时间戳。

在数据处理过程中，经常会遇到需要将不同形式的时间进行转换。这里给大家介绍下常用的方法：

### 时间对象转字符串

时间对象转换为字符串，可以通过 `isoformat` 或 `strftime` 方法来实现。

`strftime` 的英文全称是 str format time ，根据给定的格式将时间对象转换为字符串


```python
# 将 date 时间对象转换为字符串
d1 = datetime.date(2020,10,12)
d1.isoformat()
```




    '2020-10-12'




```python
# 用 srtftime 来转换
# YYYY-MM-DD 形式
d1.strftime('%Y-%m-%d')
```




    '2020-10-12'




```python
# MM DD，YYYY 形式
d1.strftime('%b %d，%Y')
```




    'Oct 12，2020'




```python
# 将 time 时间对象转换为字符串
t1 = datetime.time(20,10,1)
t1.strftime('%H:%M:%S')
```




    '20:10:01'




```python
# 将 datetime 时间对象转换为字符串
dt2 = datetime.datetime.now()
dt2.strftime('%Y-%m-%d %H:%M:%S')
```




    '2020-10-14 15:12:20'




```python
dt2.isoformat()
```




    '2020-10-14T15:12:20.403113'



python 中常见的时间日期格式化符号：

| 指令 | 意义                                                         | 示例                                                         |
| :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `%a` | 当地工作日的缩写。                                           | Sun, Mon, ..., Sat (en_US);So, Mo, ..., Sa (de_DE)           |
| `%A` | 本地化的星期中每日的完整名称。                               | Sunday, Monday, ..., Saturday (en_US);Sonntag, Montag, ..., Samstag (de_DE) |
| `%w` | 以十进制数显示的工作日，其中0表示星期日，6表示星期六。       | 0, 1, ..., 6                                                 |
| `%d` | 补零后，以十进制数显示的月份中的一天。                       | 01, 02, ..., 31                                              |
| `%b` | 当地月份的缩写。                                             | Jan, Feb, ..., Dec (en_US);Jan, Feb, ..., Dez (de_DE)        |
| `%B` | 本地化的月份全名。                                           | January, February, ..., December (en_US);Januar, Februar, ..., Dezember (de_DE) |
| `%m` | 补零后，以十进制数显示的月份。                               | 01, 02, ..., 12                                              |
| `%y` | 补零后，以十进制数表示的，不带世纪的年份。                   | 00, 01, ..., 99                                              |
| `%Y` | 十进制数表示的带世纪的年份。                                 | 0001, 0002, ..., 2013, 2014, ..., 9998, 9999                 |
| `%H` | 以补零后的十进制数表示的小时（24 小时制）。                  | 00, 01, ..., 23                                              |
| `%I` | 以补零后的十进制数表示的小时（12 小时制）。                  | 01, 02, ..., 12                                              |
| `%p` | 本地化的 AM 或 PM 。                                         | AM, PM (en_US);am, pm (de_DE)                                |
| `%M` | 补零后，以十进制数显示的分钟。                               | 00, 01, ..., 59                                              |
| `%S` | 补零后，以十进制数显示的秒。                                 | 00, 01, ..., 59                                              |
| `%f` | 以十进制数表示的微秒，在左侧补零。                           | 000000, 000001, ..., 999999                                  |
| `%z` | UTC 偏移量，格式为 `±HHMM[SS[.ffffff]]` （如果是简单型对象则为空字符串）。 | (空), +0000, -0400, +1030, +063415, -030712.345216           |
| `%Z` | 时区名称（如果对象为简单型则为空字符串）。                   | (空), UTC, GMT                                               |
| `%j` | 以补零后的十进制数表示的一年中的日序号。                     | 001, 002, ..., 366                                           |
| `%U` | 以补零后的十进制数表示的一年中的周序号（星期日作为每周的第一天）。 在新的一年中第一个星期日之前的所有日子都被视为是在第 0 周。 | 00, 01, ..., 53                                              |
| `%W` | 以十进制数表示的一年中的周序号（星期一作为每周的第一天）。 在新的一年中第一个第期一之前的所有日子都被视为是在第 0 周。 | 00, 01, ..., 53                                              |
| `%c` | 本地化的适当日期和时间表示。                                 | Tue Aug 16 21:30:00 1988 (en_US);Di 16 Aug 21:30:00 1988 (de_DE) |
| `%x` | 本地化的适当日期表示。                                       | 08/16/88 (None);08/16/1988 (en_US);16.08.1988 (de_DE)        |
| `%X` | 本地化的适当时间表示。                                       | 21:30:00 (en_US);21:30:00 (de_DE)                            |
| `%%` | 字面的 `'%'` 字符。                                          | %                                                            |



### 字符串转时间对象

字符串转时间对象，用的是 `strptime` 方法，与 `strftime` 方法刚好相反。

`strptime` 的英文全称是 str parse time ，将字符串解析为给定相应格式的时间对象。


```python
s1 = '2020-10-09'
d = datetime.datetime.strptime(s1,'%Y-%m-%d')
d
```




    datetime.datetime(2020, 10, 9, 0, 0)



下面提供了 `strftime` 方法与 `strptime` 方法的比较：


|          | `strftime`                                                   | `strptime`                                                   |
| :------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 用法     | 根据给定的格式将对象转换为字符串                             | 将字符串解析为给定相应格式的 `datetime`对象 |
| 方法类型 | 实例方法                                                     | 类方法                                                       |
| 方法     | `date`; `datetime`; `time`| `datetime` |
| 签名     | `strftime(format)`                                           | `strptime(date_string, format)`                              |

需要注意的是，`strftime` 方法可供 `date`、`time` 和 `datetime` 对象使用，而 `strptime` 方法仅供 `datetime` 对象使用。

### 时间戳转换为时间对象

时间戳是指格林威治时间1970年01月01日00时00分00秒(北京时间1970年01月01日08时00分00秒)起至现在的总秒数。 


```python
# 获取当前的时间戳
ts_1 = time.time()
ts_1
```




    1602660751.071785




```python
# 获取当天00:00:00的时间戳
ts_2 = int(time.time()/86400)*86400
ts_2
```




    1602633600




```python
# 获取当天23:59:59的时间戳
# 一天有 24*60*60 =  86400 秒
ts_3 = int(time.time()/86400)*86400+86400-1
ts_3
```




    1602719999




```python
# 将时间戳转换为时间对象
datetime.datetime.fromtimestamp(ts_1)
```




    datetime.datetime(2020, 10, 14, 15, 32, 31, 71785)




```python
# 将时间戳转换为时间对象
datetime.date.fromtimestamp(ts_1)
```




    datetime.date(2020, 10, 14)



### 将时间对象转换为时间戳

鉴于，时间戳是指格林威治时间1970年01月01日00时00分00秒(北京时间1970年01月01日08时00分00秒)起至现在的总秒数。

因此，将时间对象转换为时间戳时，直接计算两个时间对象的 timedelta ，并将timedelta 以 “秒” 来表示就可以了。


```python
dt1
```




    datetime.datetime(2020, 10, 11, 20, 10, 1)




```python
# 注意这里要用 北京时间
dt_s = datetime.datetime(1970,1,1,8)
dt_s
```




    datetime.datetime(1970, 1, 1, 8, 0)




```python
timedelta_1 = dt1 -dt_s
# 返回时间间隔包含了多少秒
timedelta_s = timedelta_1.total_seconds()
timedelta_s
```




    1602418201.0



这里我们来反推下，看我们将时间对象转换为时间戳，是否正确。


```python
# 将时间戳转换为时间对象
datetime.datetime.fromtimestamp(timedelta_s)
```




    datetime.datetime(2020, 10, 11, 20, 10, 1)



## 时间对象的运算

### 获取当天最小时间和最大时间


```python
# 获取当天最小时间
datetime.datetime.combine(datetime.date.today(),datetime.time.min)
```




    datetime.datetime(2020, 10, 14, 0, 0)




```python
# 获取当天最大时间
datetime.datetime.combine(datetime.date.today(),datetime.time.max)
```




    datetime.datetime(2020, 10, 14, 23, 59, 59, 999999)



### 获取当前日期的前几天/后几天


```python
# 获取明天
datetime.date.today() + datetime.timedelta(days=1)
```




    datetime.date(2020, 10, 15)




```python
# 获取明天
datetime.date.today() - datetime.timedelta(days=1)
```




    datetime.date(2020, 10, 13)



### 获取本周或本月第一天及最后一天


```python
d_today = datetime.date.today()
d_today
```




    datetime.date(2020, 10, 14)




```python
# 获取本周第一天
d_today - datetime.timedelta(d_today.weekday())
```




    datetime.date(2020, 10, 12)




```python
# 获取本周最后一天
d_today + datetime.timedelta(6-d_today.weekday())
```




    datetime.date(2020, 10, 18)



### 计算两个日期相差多少天


```python
# timedelta 对象的计算
td1 = dt2 - dt1
td1
```




    datetime.timedelta(days=2, seconds=68539, microseconds=403113)




```python
td1.days
```




    2



注意下，如果需要计算两个日期之间总共相差多少秒，应该用 `total_seconds()` 方法。


```python
td1.seconds
```




    68539




```python
td1.total_seconds()
```




    241339.403113



参考文档：

- https://docs.python.org/zh-cn/3/library/datetime.html#strftime-strptime-behavior
- https://segmentfault.com/a/1190000012112097
- https://blog.csdn.net/qq_34493908/article/details/80888052
- https://zhuanlan.zhihu.com/p/96384066


《Python时间使用指南.pdf》可以在公众号 「Python数据之道」后台回复 “**time**” 来获取。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
