---
author: zhangman1123
comments: true
date: 2015-04-27 01:06:50+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=110
slug: actionbar-pulltorefresh-%e7%9a%84%e4%bd%bf%e7%94%a8
title: actionbar-pulltorefresh 的使用
tags:
- Android
categories:
- Android 笔记
---

## 1\. 在自己工程app的build.gradle 中加入依赖

{% highlight java %}   
  dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar']) 
     // compile 'com.android.support:appcompat-v7:22.0.0'
     // compile project(':actionbarsherlock')
     // compile project(':pullToRefreshLibrary') 
    repositories { 
      mavenCentral()
     } 
    compile 'com.github.chrisbanes.actionbarpulltorefresh:extra-abs:+'
  }
{% endhighlight %}

## 2\. 布局文件中使用

{% highlight xml %}  
    <uk.co.senab.actionbarpulltorefresh.extras.actionbarsherlock.PullToRefreshLayout  
       xmlns:android="http://schemas.android.com/apk/res/android"  
      android:id="@+id/ptr_layout" 
      android:layout_width="match_parent"  
      android:layout_height="match_parent">  
      <!-- Your content, here we're using a ScrollView -->   
    <ScrollView  
        android:layout_width="match_parent"                
        android:layout_height="match_parent">  
    </ScrollView>    
    </uk.co.senab.actionbarpulltorefresh.extras.actionbarsherlock.PullToRefreshLayout>
{% endhighlight %}  


## 3\. 代码中

{% highlight java %}
    mPullToRefreshLayout = (PullToRefreshLayout) view.findViewById(R.id.ptr_layout); 
    mPullToRefreshLayout.setBackgroundResource(R.color.title_bg); 
    ActionBarPullToRefresh.from(getActivity()) 
    .allChildrenArePullable().listener(this) 
    .setup(mPullToRefreshLayout);
{% endhighlight %} 

然后实现
OnRefreshListener //接口

{% highlight java %}
@Override 
public void onRefreshStarted(View view) {
  mPullToRefreshLayout.setRefreshComplete();
}
{% endhighlight %}

ok!搞定
