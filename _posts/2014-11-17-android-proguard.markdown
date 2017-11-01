---
author: zhangman1123
comments: true
date: 2014-11-17 13:45:25+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=65
slug: android-proguard
title: Android proguard
tags:
- Android
categories:
- Android 笔记
---

新更新的sdk中没有proguard.cfg文件

可在proje.properties 中做出如下修改

将proguard.config=${sdk.dir}/tools/proguard/proguard-android.txt:proguard-project.txt取消注释

然后在proguard-project.txt 中添加混淆配置

如下

{% highlight java %} 
-libraryjars libs/android-support-v4.jar
-libraryjars libs/achartengine-1.2.0.jar
-libraryjars libs/armeabi/libbdpush_V2_0.so
-libraryjars libs/armeabi/libbspatch.so
-libraryjars libs/armeabi-v7a/libcyberplayer.so
-libraryjars libs/armeabi-v7a/libcyberplayer-core.so
-libraryjars libs/asmack.jar
-libraryjars libs/Baidu_CyberPlayer_SDK_1.7s.jar
-libraryjars libs/pushservice-4.0.0.jar
-dontwarn com.baidu.**

-keep class com.baidu.** { *; } 
-keep class org.apache.**{ *; }
{% endhighlight %}
