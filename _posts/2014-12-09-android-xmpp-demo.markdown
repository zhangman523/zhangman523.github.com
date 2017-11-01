---
author: zhangman1123
comments: true
date: 2014-12-09 09:15:18+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=80
slug: android-xmpp-demo
title: android xmpp demo
tags:
- Android
categories:
- Android 笔记
---

http://git.oschina.net/way/XMPP/




上面是国人一个xmpp聊天软件很多方面值得借鉴。大家可以去看看




在文件传输这一方面还是有欠缺




最近我一直在做一块，有点思路




我的思路是，既然xmpp服务器传输文件有很多bug那我们就不用那个传输，




自己在服务器写个接口，接收文件上传并返回一个文件url




然后在手机端就吧url发出去，另一端接收是更加消息类型判断是文字消息还是文件消息，文件消息就下载展示！



