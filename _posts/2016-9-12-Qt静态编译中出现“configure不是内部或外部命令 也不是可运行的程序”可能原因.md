---
layout: post
title: Qt静态编译中出现“configure不是内部或外部命令 也不是可运行的程序”可能原因
date: 2016-9-12
categories: Qt
tags: [Qt,静态编译]
description: 对静态编译时出现的意外原因纠错。
---

安装Qt时未勾选source components组件。

**系统**：Win 7 64 bits   
**软件**：Qt Creator 5.6.0(VS2013)




笔者依照网上教程在windows 7平台进行Qt5.6.0的静态编译时，出现不同于教程中的现象

![pic1.png](http://pic.ffsky.net/images/2016/09/12/5dfb25d4425507743827df9e69c2897d.png)  

后百度多次，于Jason188080501的csdn博文[http://blog.csdn.net/wsj18808050/article/details/42301561](http://blog.csdn.net/wsj18808050/article/details/42301561) 中发现自己所犯问题：   

	安装Qt时未勾选source components组件，因此在cmd中没有进入到正确的源码目录(现为C:\Qt\Qt5.6.0\5.6\Src)

![pic2.png](http://pic.ffsky.net/images/2016/09/12/3b0891f505ecdb7f9fc13f5cb9d9242b.png)
   
虽然其他博主也提到了这点，但是由于参考教程时笔者已经以默认安装完毕了Qt Creator，自动忽略了这一点(汗颜emoji)；而博主Jason的博文中特意提到了这点——“注意：源码那里也要勾上，默认是不勾选的”。
   
说到底还是自己不细心啊。如果有同样问题的朋友们可以参考一下。



