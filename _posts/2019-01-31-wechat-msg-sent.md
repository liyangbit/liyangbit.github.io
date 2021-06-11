---
layout: posts
title: "轻松玩转Python发送新春祝福给指定好友"
teaser:
date: 2019-01-31
header:
   image_fullwidth: "image-head.jpg"
categories:
   - Comprehensive
tags:
   - Comprehensive
comments: true
show_meta: true
sidebar: right
authors: ["阳哥"]
---



{% include alert info='轻松玩转Python发送新春祝福给指定好友' %}


<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>



“阳哥，别玩手机了，赶紧过来打牌，三缺一，十万火急！！！”

“别急，马上就来，还有几个重要的朋友要发送新年祝福……”

每逢重大节假日，您是否遇到类似略显尴尬的情景。

为了解放我们的双手，不再一个一个去发送祝福内容，这次，我们决定使用 Python 来发送新春祝福给指定好友。

这里，我们主要是用 itchat 来发送消息。 首先，安装 itchat，如下：

```python
pip install itchat
```

如果是想给所有的微信好友发送祝福语，相对来说代码要简单一些。

但有时候，我们不想给所有好友都发信息，只想给一部分好友发送信息。 尤其是有些领导或老板或其他特殊人员，发送消息可能效果适得其反。

所以，本次的代码设计思路分为两个部分。

1. 获取你的所有微信好友
2. 针对你想发送信息的好友发送祝福语

## 步骤一：获取所有微信好友

对于我的微信好友，我有一个习惯，会将好友的备注填上，这样的话，即使好友修改昵称，在我的手机上显示的名称也不会发生改变，有利于自己对好友进行识别。

所以，这里我是获取所有微信好友的备注名称，即 `RemarkName`（当然，如果你的好友大部分是没有进行备注的，你可以试试 `DisplayName` 或者 `NickName`）

获取所有好友的代码如下，运行代码后，会跳出二维码，用你的微信扫描登录即可：

```python
#coding=utf8
import itchat, time
import pandas as pd
 
itchat.auto_login(True)
 
friendList = itchat.get_friends(update=True)[1:]

# --------------------------start------------------------------
# step 1： 获取所有的微信好友列表，在获取完毕后，需要注释掉这段代码
# 从获得的好友列表里筛选你想发送消息的好友，同时可以将好友按照不同的分组进行设置
# 获取所有微信好友列表，这里我获取的是好友的备注名称
count_02 = 0
friends_remark = []
for friend in friendList:
    friends_remark.append(friend['RemarkName'])
    count_02 = count_02 + 1

# df_friends = pd.Series(friends_remark)
# df_friends.to_csv('friends.csv', encoding='utf_8_sig') # utf-8
print(friends_remark)
print("Total {} friends".format(count_02))
# -------------------------end---------------------------------

```

在上面的代码中， `print(friends_remark)` 会输出你的所有好友，你可以从里面挑选你想发送消息的好友，同时可以分为几个不同的组。 后续可以考虑针对不同分组的好友发送不同的祝福内容。

## 步骤二： 将好友分组，并发送消息

在获取了所有好友之后，首先请先把上面的 `step 1` 的代码进行注释，然后需要从你的所有好友里挑选想发送消息的好友的名称，可以分别填入到下面代码中的 `friend_msg_list_01` 或 `friend_msg_list_02` 。

其次，需要设置发送的祝福语的内容。

把以上内容设置好之后，则可以参考下面的代码进行设置，以及运行代码。

以下的代码，默认将好友分为两组，当然，你也可以设置更多的分组，在后面添加 `elif` 条件语句进行发送设置即可。

```python
# --------------------------start------------------------------
# step 2: 给指定好友按不同分组发送不同的消息
# 在运行 step 1 的代码时，这里的代码需要注释

# 定义要发送的好友的范围，防止自动乱发消息
# 好友列表里，我用的是 备注名称
friend_msg_list_01 = ["lemon-zs", "lemon-zs-01"]  # 根据自己的实际情况设置好友分组
friend_msg_list_02 = ["lemon-zs-02", "lemon-zs-02" ] # 根据自己的实际情况设置好友分组


#设置不同好友分组需要发送的内容
SINCERE_WISH = u'test, 祝%s新年快乐！'  # 根据自己的实际情况设置祝福内容
msg_01 = 'test01, 祝新年快乐！！！'     # 根据自己的实际情况设置祝福内容
msg_02 = 'test02, 祝新年快乐！！！'     # 根据自己的实际情况设置祝福内容


count = 0
for friend in friendList:
    # 通过 if 条件设置，可以只针对 指定的好友发送消息
    # 也可以设置为几个分组，不同分组发送不同的消息
    # 由于我的好友一般都设置了备注名称， 即 "RemarkName"，所以我是用 备注名称来作为条件判断
    # 主要是为了防止别错发给一些不能随便发送信息的老板或领导或其他人士
    if friend['RemarkName'] in friend_msg_list_01:
        # itchat.send( SINCERE_WISH % (friend['DisplayName'] or friend['NickName']), friend['UserName'])
        itchat.send( SINCERE_WISH % (friend['DisplayName'] or friend['NickName']), friend['UserName'])
        time.sleep(5)
        count = count + 1
    elif friend['RemarkName'] in friend_msg_list_02:
        itchat.send( msg_01, friend['UserName'])
        time.sleep(5)
        count = count + 1


print("Total {} messages have been sent.".format(count))
print("----end----")
# -------------------------end---------------------------------
```

---
友情提醒下：

itchat 在使用过程中，如果频繁调取与发送消息，可能会导致微信被封，请谨慎使用。当发送消息的人员很多时，请适当设置每发送一个之后的休息时间间隔，即 `time.sleep(5)`。

完整的代码文件，可以在公众号『Python数据之道』后台回复 "code" 来获取。

<div align="center">
    <img src="/images/qrcode.jpg" width="20%">
</div>
