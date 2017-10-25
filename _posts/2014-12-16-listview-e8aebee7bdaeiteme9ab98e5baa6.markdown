---
author: zhangman1123
comments: true
date: 2014-12-16 08:29:14+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=91
slug: listview-%e8%ae%be%e7%bd%aeitem%e9%ab%98%e5%ba%a6
title: listView 设置item高度
wordpress_id: 91
categories:
- Android 笔记
---

listView 直接在item布局中设置高度，显示出来是没有效果！

**解决方法**

在item布局中加入属性

{% highlight xml %}
android:minHeight="?android:attr/listPreferredItemHeight"
{% endhighlight %}

在设置布局文件的height就可以了
