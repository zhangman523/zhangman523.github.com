---
author: zhangman1123
comments: true
date: 2014-11-07 02:37:37+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=30
slug: linux-%e4%b8%8b%e9%85%8d%e7%bd%aemysql%e8%bf%9c%e7%a8%8b%e7%99%bb%e5%bd%95
title: Linux 下配置mysql远程登录
tags:
- Linux
- Mysql
categories:
- Linux
---

Ubuntu下安装mysql 默认的root用户无法远程登录mysql(反正我是弄了半天都没弄好)

所以我就另辟新径

创建另一个mysql 用户

>1\. 修改 /etc/mysql/my.cnf

找到 

{% highlight shell %}
bind-address = 127.0.0.1
{% endhighlight %}
将其注释

>2\. 通过root用户登入mysql
{% highlight shell %}
mysql -u root -p
{% endhighlight %}
然后输入密码...
{% highlight shell %}
mysql>GRANT ALL PRIVILEGES ON *.* TO admin@localhost IDENTIFIED BY 'something' WITH GRANT OPTION;
mysql>GRANT ALL PRIVILEGES ON *.* TO admin@"%" IDENTIFIED BY 'something' WITH GRANT OPTION;
{% endhighlight %}

开始创建admin用户 并允许本地访问数据库

然后允许远程访问

admin@"%"就是允许远程访问

然后重启mysql服务就ok  !

搞定！

**ps**:admin用户的密码还没设置呢，登不进去\>\_\<\!

改密码

用root用户登录mysql
{% highlight shell %}
mysql -u root -p
{% endhighlight %}
输入密码
{% highlight shell %}
use mysql;
{% endhighlight %}
然后
{% highlight shell %}
UPDATE user SET password=PASSWORD('123456') WHERE user='admin';
FLUSH PRIVILEGES;
{% endhighlight %}
搞定。
