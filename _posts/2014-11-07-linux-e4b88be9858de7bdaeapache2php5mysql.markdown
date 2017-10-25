---
author: zhangman1123
comments: true
date: 2014-11-07 02:36:16+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=26
slug: linux-%e4%b8%8b%e9%85%8d%e7%bd%aeapache2php5mysql
title: Linux 下配置apache2+php5+mysql
wordpress_id: 26
categories:
- Linux
---

>LAMP是Linux web服务器组合套装的缩写，分别是Apache+MySQL+PHP。

>此教程教大家如何在Ubuntu 12.04 LTS server 上安装Apache2服务器,包括PHP5(mod_php)+MySQL。

>我们使用**root**用户或有 **sudo** 权限的帐号来安装软件和配置

## 1\. 安装 Apache2

输入命令:
{% highlight shell %}
sudo apt-get install apache2
{% endhighlight %}

在浏览器输入你服务器地址列入 http://youip查看Apache2是否工作，如果显示(It works!)，说明已经工作。

>Apache 在 Ubuntu 中默认文档根目录为 `/var/www`，配置文件 `etc/apache2/apache2.conf`，额外配置存储子目录 `/etc/apache2` 例如 `/etc/apache2/mods-enabled` (为 Apache 模块), `/etc/apache2/sites-enabled` (为虚拟主机 virtual hosts), 和 `/etc/apache2/conf.d`

## 2\. 安装 PHP5

>安装 PHP5 和 Apache PHP5 模块:
{% highlight shell %}
sudo apt-get install php5 libapache2-mod-php5
{% endhighlight %}
然后重启apache:
{% highlight shell %}
sudo /etc/init.d/apache2 restart
{% endhighlight %}

测试 PHP5 / 可以建立一个探针页面
{% highlight shell %}
vi /var/www/info.php
{% endhighlight %}
输入下面的内容：

{% highlight php %}
<?php
phpinfo();
?>
{% endhighlight %}

然后打开浏览器访问 (http://your_Ip/info.php):

你可以看到一些已经支持的模块 
## 3\. 安装 MySQL 5
输入命令：

{% highlight shell %}
sudo apt-get install mysql-server mysql-client
{% endhighlight %}

安装过程中需要设置root账户密码，系统会作以下提示：
{% highlight shell %}
New password for the MySQL "root" user:Repeat password for the MySQL "root" user:
{% endhighlight %}

## 4\. 为PHP5取得 MySQL 支持

我们需要安装 php5-mysql，先查看一下php5的模块

{% highlight shell %}
sudo apt-cache search php5
{% endhighlight %}

然后安装所需模块，例如下面的命令：

{% highlight shell %}
sudo apt-get install php5-mysql php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl
{% endhighlight %}

>重启 Apache2:

{% highlight shell %}
sudo /etc/init.d/apache2 restart
{% endhighlight %}

## 5\.安装phpmyadmin
{% highlight shell %}
sudo apt-get install phpmyadmin
{% endhighlight %}

然后就可以通过youidp/phpmyadmin
来管理mysql了
