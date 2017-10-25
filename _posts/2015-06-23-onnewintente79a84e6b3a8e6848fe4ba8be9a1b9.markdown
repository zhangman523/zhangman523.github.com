---
author: zhangman1123
comments: true
date: 2015-06-23 05:18:31+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=135
slug: onnewintent%e7%9a%84%e6%b3%a8%e6%84%8f%e4%ba%8b%e9%a1%b9
title: onNewIntent的注意事项
wordpress_id: 135
categories:
- Android 笔记
---

Activity 中 重写onNewIntent方法无效，所有在网上找了下资料

才发现这个方法与activity的启动方式有关系

于是在Manifest中添加activity的启动方法sinleTask

具体事项请参照

[http://blog.csdn.net/findsafety/article/details/9664061](http://blog.csdn.net/findsafety/article/details/9664061)
