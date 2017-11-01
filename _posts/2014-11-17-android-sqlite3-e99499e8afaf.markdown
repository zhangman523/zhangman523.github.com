---
author: zhangman1123
comments: true
date: 2014-11-17 13:39:44+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=63
slug: android-sqlite3-%e9%94%99%e8%af%af
title: Android sqlite3 错误
tags:
- Android
- sqlite3
categories:
- Android 笔记
---

在使用游标 Cursor 是获取列值的时候最好确定列名是否与数据库一至

{% highlight java %}   
if(localcursor.moveToFirst()){
    do {
        Item item = new Item();
        //item.id = localcursor.getInt(1);
        item.setDid(localcursor.getInt(2));
        item.setTitle(localcursor.getString(3));
        item.setType(localcursor.getString(4));
        item.setCollection(localcursor.getInt(5));
        item.setAnswer(localcursor.getString(6));
        ilist.add(item);
    } while (localcursor.moveToNext());
}
{% endhighlight %}

这样写，可能获取的值不是与你想象中的一样。可能会错开。
最好是下面这种写法

{% highlight java %}
if(localcursor.moveToFirst()) {
    do {
        Item item = new Item();
        //item.id = localcursor.getInt(1);
        item.setDid(localcursor.getInt(localcursor.getColumnIndex(StaticPerson.I_DID)));
        item.setTitle(localcursor.getString(localcursor.getColumnIndex(StaticPerson.I_TITLE)));
        item.setType(localcursor.getString(localcursor.getColumnIndex(StaticPerson.I_TYPE)));
        item.setCollection(localcursor.getInt(localcursor.getColumnIndex(StaticPerson.I_COLLECTION)));
        item.setAnswer(localcursor.getString(localcursor.getColumnIndex(StaticPerson.I_ANSWER)));
        list.add(item);
        } while (localcursor.moveToNext());
}
{% endhighlight %}

StaticPerson.I_DID 为表的自动名称

这样更加准确的获取每个列的值
