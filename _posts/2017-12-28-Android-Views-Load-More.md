---
author: zhangman1123
comments: true
date: 2017-12-28 05:51:14+00:00
layout: post
slug: Android-Views-Load-More
title: Android-Views-Load-More
tags:
- Android
categories:
- Android 笔记
---

## [LoadMoreViewLayout](https://github.com/zhangman523/LoadMoreViewLayou)

这个项目是作为 [Ultra Pull To Refresh](https://github.com/liaohuqiu/android-Ultra-Pull-To-Refresh) 的`上拉加载`补充功能。

[APK下载](https://github.com/zhangman523/LoadMoreViewLayout/blob/master/load_more_view.apk)

- 支持所有的View：

    `ListView`、`GridView`、`RecyclerView`、`ScrollView`、`WebView`、`FrameLayout`、`RelativeLayout` 等


  ![]({{ site.url }}/assets/images/list_view.gif)
  ![]({{ site.url }}/assets/images/recycler_view.gif)

  - 点击加载更多

  ![]({{ site.url }}/assets/images/grid_view.gif)

  - 加载失败 点击重试

  ![]({{ site.url }}/assets/images/scroll_view.gif)

## 使用方法

### jcenter() 依赖

  Maven 依赖

{% highlight xml %}
<dependency>
  <groupId>zhangman.github.loadmore</groupId>
    <artifactId>loadmore</artifactId>
    <version>1.0.1</version>
      <type>pom</type>
    </dependency>
{% endhighlight %}

gradle 配置

{% highlight xml %}
compile 'zhangman.github.loadmore:loadmore:1.0.1'
{% endhighlight %}

#### 配置

有3个参数可配置：

* 阻尼系数

    默认： `1.7f`, 越大，感觉上拉越吃力

* 回弹延时

    默认： `300` ,回弹到底部所用的时间

* 上拉加载／点击加载

    默认为上拉加载

##### xml 中配置示例

{% highlight xml %}
<zhangman.github.loadmore.LoadMoreViewContainer
        android:id="@+id/load_more_container"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:load_more_auto_load_more="false"
        app:load_more_resistance="1.7"
        app:load_more_duration_to_close="300"
        >
      <FrameLayout
          android:id="@+id/frameLayout"
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:clickable="true"
          android:focusable="true"
          >
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:text="FameLayout"
            />
      </FrameLayout>
</zhangman.github.loadmore.LoadMoreViewContainer>
{% endhighlight %}

### 也可以在java中配置

{% highlight java %}
    mLoadMoreViewContainer.setDurationToClose(300);
      mLoadMoreViewContainer.setAutoLoadMore(true);
        mLoadMoreViewContainer.setResistance(1.7f);
{% endhighlight %}

## 处理加载更多

通过 `LoadMoreHandler` 触发加载更多

{% highlight java %}
public interface LoadMoreHandler {
   /**
        * 需要加载数据时触发
         *
          * @param loadMoreContainer
           */
    void onLoadMore(LoadMoreContainer loadMoreContainer);
}
{% endhighlight %}


例子：

{% highlight java %}
    mLoadMoreViewContainer.useDefaultFooter();
    //加载数据成功后调用
    mLoadMoreViewContainer.loadMoreFinish(false, true);
    mLoadMoreViewContainer.setLoadMoreHandler(new LoadMoreHandler() {
      @Override
      public void onLoadMore(final LoadMoreContainer loadMoreContainer) {
        mLoadMoreViewContainer.postDelayed(new Runnable() {
          @Override
          public void run() {
            loadMoreContainer.loadMoreFinish(false, true);
          }
        }, 1000);
      }
    });
{% endhighlight %}

## 自定义加载View

自定义View 实现`LoadMoreUIHandler`  然后调用
{% highlight java %}
    mLoadMoreViewContainer.setLoadMoreView(footerView);
      mLoadMoreViewContainer.setLoadMoreView(footerView);
        mLoadMoreViewContainer.setLoadMoreUIHandler(footerView);
{% endhighlight %}

参考 `LoadMoreDefaultFooterView`

## Thanks

* [android-Ultra-Pull-To-Refresh](https://github.com/liaohuqiu/android-Ultra-Pull-To-Refresh)
* [android-cube-app](https://github.com/liaohuqiu/android-cube-app)