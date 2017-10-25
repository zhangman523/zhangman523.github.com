---
author: zhangman1123
comments: true
date: 2015-04-27 01:12:48+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=118
slug: '%e8%87%aa%e5%ae%9a%e4%b9%89actionbar-pulltorefresh-%e7%9a%84%e6%a0%b7%e5%bc%8f'
title: 自定义actionbar-pulltorefresh 的样式
wordpress_id: 118
categories:
- Android 笔记
---

{% highlight java %}
//定义actionbar 背景颜色和进度条颜色 文字颜色   
DefaultHeaderTransformer transformer = (DefaultHeaderTransformer)  
mPullToRefreshLayout.getHeaderTransformer();   
transformer.getHeaderView().findViewById(R.id.ptr_text).setBackgroundColor(getResources().getColor(R.color.title_bg));  
TextView title = (TextView) transformer.getHeaderView().findViewById(R.id.ptr_text);  
title.setTextColor(getResources().getColor(android.R.color.white));   
transformer.setProgressBarColor(getResources().getColor(R.color.title_bg)); 
//transformer.setPullText("下拉加载");
{% endhighlight %}
