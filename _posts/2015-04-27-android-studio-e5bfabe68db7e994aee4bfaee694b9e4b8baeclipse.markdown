---
author: zhangman1123
comments: true
date: 2015-04-27 08:50:54+00:00
layout: post
#link: http://www.zhangman523.com/wordpress/?p=120
slug: android-studio-%e5%bf%ab%e6%8d%b7%e9%94%ae%e4%bf%ae%e6%94%b9%e4%b8%baeclipse
title: Android studio 快捷键修改为eclipse
tags:
- Android
categories:
- Android 笔记
---

安装android studio 后最不习惯的就是快捷键了。那么按照下列步骤修改

1.Android Studio ->Preferences->keymap 在右侧上面就可以选择eclipse ok！

2.修改后发现部分快捷键没有效果怎么办，别担心继续看下去

(1).发现ctrl+鼠标左键没效果 在keymap界面搜索Declaration 发现没有设置快捷键，选择右键 **Add Mouse ShortCut**

然后直接按ctrl+鼠标左键就ok了

(2).文件修改后ctrl＋z 可以，但ctrl+y没有。。 在keymap界面搜索Redo 发现没有设置快捷键 选择右键**Add Keyboard ShortCut 然后按ctrl+y 就OK**

(3).ctrl+f  没有效果，一样的，在keymap节目搜索Find 选择右键**Add Keyboard ShortCut 然后按ctrl+f 就OK**

(4).android studio 的代码提示还时很及时的，但是有的时候电脑卡的话提示久很慢，怎么办？

设置eclipse 代码提示快捷键alt+/

在keymap节目搜索Class Name Completion 选择右键**Add Keyboard ShortCut 然后按alt+/就OK**

然后Apply 在 OK！

基本是快捷键都一样了！以后有什么不同继续发现!




