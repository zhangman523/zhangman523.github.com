---
author: zhangman1123
comments: true
date: 2015-10-19 05:51:14+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=141
slug: android-textview-marquee
title: Android TextView marquee
wordpress_id: 141
categories:
- Android 笔记
---

TextView 设置跑马灯效果

TextView 添加以下属性 长度不够不会滚动 可以考虑加上空格
{% highlight java %}
android:ellipsize="marquee"
android:focusable="true"
android:focusableInTouchMode="true"
android:marqueeRepeatLimit="marquee_forever"
{% endhighlight %}
