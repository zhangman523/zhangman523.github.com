---
author: zhangman1123
comments: true
date: 2014-12-15 09:31:45+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=86
slug: android-listview-%e8%a7%a3%e5%86%b3%e6%bb%91%e5%8a%a8%e5%8f%98%e9%bb%91
title: android listView 解决滑动变黑
tags:
- Android
categories:
- Android 笔记
---

在listview 添加下面属性 即可
{% highlight xml %}
android:cacheColorHint="#00000000"
android:divider="#19000000"
{% endhighlight %}