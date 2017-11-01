---
author: zhangman1123
comments: true
date: 2014-11-07 02:37:00+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=28
slug: ubuntu-%e5%ae%89%e8%a3%85%e5%92%8c%e5%8d%b8%e8%bd%bd%e8%bd%af%e4%bb%b6
title: ubuntu 安装和卸载软件
wordpress_id: 28
tags:
- Linux
- Ubuntu
categories:
- Linux
---
>安装和卸载

`sudo apt-get autoremove`  要移除的软件包名

`sudo apt-get remove`  要移除的软件包名
    
`sudo apt-get install`  要安装的软件包名

>要安装 .deb 套件包时

`sudo dpkg -i package_file.deb`
    
`sudo dpkg -r package_name`
