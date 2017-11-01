---
author: zhangman1123
comments: true
date: 2014-11-07 02:47:45+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=45
slug: linux-%e4%b8%8b-tomcat-%e7%ab%af%e5%8f%a3%e4%bf%ae%e6%94%b9
title: Linux 下 tomcat 端口修改
tags:
- Linux
- tomcat
categories:
- Linux
---

修改 /etc/tomcat7/server.xml 文件
{% highlight shell %}
<Connector port="8080" protocol="HTTP/1.1"
connectionTimeout="20000"
URIEncoding="UTF-8"
redirectPort="8443" />
{% endhighlight %}

将port="8080"端口修改即可

如果修改端口小于1024 则将还要做一下配置

修改 /etc/default/tomcat7 文件
找到AUTHBIND=no 这行，改为yes

重启tomcat服务器即可
