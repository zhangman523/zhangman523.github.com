---
author: zhangman1123
comments: true
date: 2015-05-05 03:35:36+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=126
slug: apache-%e6%8d%a2nginx-%e7%bb%8f%e9%aa%8c
title: apache 换nginx 经验
tags:
- Linux
- nginx
categories:
- Linux
---

以前都是用apache 做web服务器，apache对php支持挺好的。但是听说nginx 比apache 性能要好，你懂的啊！租的云服务器配置较低，既然可以节省资源的当然不能浪费，ok就开始动手了！

## 1\. 首先安装nginx

{% highlight shell %}
sudo apt-get install nginx
{% endhighlight %}

等待下载安装，貌似安装过程中要重设mysql密码。

## 2\. 安装完成后然后把apache 服务停掉

{% highlight shell %}
sudo /etc/init.d/apache2 stop
{% endhighlight %}

## 3\. 启动nginx

{% highlight shell %}
sudo /etc/init.d/nginx start
{% endhighlight %}

然后看看有没有效果

输入http://yourip 出现welcome nginx 挺简洁的，初步安装就已经完成了

## 4\. 安装完成后我们要配置nginx

以前apache的网站根目录在/var/www/

配置nginx

{% highlight shell %}
vim /etc/nginx/sites-available/default
{% endhighlight %}

找到下面几行，取消注释后如下就可以了

{% highlight shell %}
61     location ~ \.php$ {
62         include fastcgi_params;
63         fastcgi_param SCRIPT_FILENAME /var/www$fastcgi_script_name;
64         fastcgi_split_path_info ^(.+\.php)(/.+)$;
65         # NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
66
67         # With php5-cgi alone:
68         fastcgi_pass 127.0.0.1:9000;
69         # With php5-fpm:
70         #fastcgi_pass unix:/var/run/php5-fpm.sock;
71         fastcgi_index index.php;
72     }
{% endhighlight %}

修改index的一行，添加

{% highlight shell %}
index.php index index.php index.html index.htm;
{% endhighlight %}

## 5\. 安装FastCgi

{% highlight shell %}
sudo apt-get install spawn-fcgi
{% endhighlight %}

6.重启nginx

{% highlight shell %}
sudo /etc/init.d/nginx restart
{% endhighlight %}

启动fastcgi php

{% highlight shell %}
spawn-fcgi -a 127.0.0.1 -p 9000 -C 10 -u www-data -f /usr/bin/php-cgi
{% endhighlight %}

## 7\. 访问以前的网站看看有没有效果。

如果出现502 错误

安装php5-fpm

然后重启nginx 就ok！
