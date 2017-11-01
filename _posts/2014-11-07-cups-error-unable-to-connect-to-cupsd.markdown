---
author: zhangman1123
comments: true
date: 2014-11-07 02:46:20+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=41
slug: cups-error-unable-to-connect-to-cupsd
title: 'CUPS error : Unable to connect to cupsd'
tags:
- CUPSD
categories:
- Linux
---

这几天在弄共享网络打印机(ubuntu 服务器上)
弄了一下午cups都起不来
上网各种找问题，最后找到了
通过命令
{% highlight shell %}
sudo cupsd -t 
{% endhighlight %}
找到问题所在了。

