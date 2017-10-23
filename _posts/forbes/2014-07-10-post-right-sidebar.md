---
layout: page
subheadline: Templates
title:  "Page/Post Right Sidebar"
teaser: "This is an example of page/post with a sidebar on the right."
breadcrumb: true
tags:
  - post format
  - Matplotlib
  - forbes
categories:
  - forbes
comments: false
header:
  image_fullwidth: "image-head.jpg"
sidebar: right
---
*Feeling Responsive* shows metadata by default. The default behaviour can be changed via `config.yml`. To show metadata at the end of a page/post just add the following to front matter:
<!--more-->

~~~
show_meta: true
~~~

If you don't want to show metadata, it's simple again:

~~~
show_meta: false
~~~


## Other Post Formats
<!-- {: .t60 } -->
{% include list-posts.html tag='Matplotlib' %}