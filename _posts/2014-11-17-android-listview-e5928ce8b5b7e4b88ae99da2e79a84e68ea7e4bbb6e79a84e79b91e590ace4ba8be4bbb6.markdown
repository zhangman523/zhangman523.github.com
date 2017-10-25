---
author: zhangman1123
comments: true
date: 2014-11-17 13:46:30+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=67
slug: android-listview-%e5%92%8c%e8%b5%b7%e4%b8%8a%e9%9d%a2%e7%9a%84%e6%8e%a7%e4%bb%b6%e7%9a%84%e7%9b%91%e5%90%ac%e4%ba%8b%e4%bb%b6
title: android ListView 和起上面的控件的监听事件
wordpress_id: 67
categories:
- Android 笔记
---

当listView 上面有控件获取焦点是，listItem无法获取到焦点

添加了 Item事件lv.setOnItemClickListener（......）  不起作用

解决方案：

在list的配置xml的根节点添加属性
{% highlight xml %}
android:descendantFocusability="blocksDescendants"
{% endhighlight %}

还有就是在要添加事件的控件上添加

{% highlight xml %}
android:focusable="false"
{% endhighlight %}