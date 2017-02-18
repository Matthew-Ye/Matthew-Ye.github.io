---
layout: post
title: github中的html以浏览模式打开
date: 2017-2-18
categories: 前端
tags: [前端]
description: 简单介绍了github中的html以浏览模式打开的方法。
---

# github中的html以浏览模式打开

如果我们直接打开存放在github中的网页文件，会看到源代码。如   
[点我显示源代码](https://github.com/Matthew-Ye/d3/blob/master/positioningForce.html)

当我们想展示网页效果的时候就会带来不便。因此可以以浏览模式打开。笔者收集的方法如下

## 1. htmlpreview.github.io

在url之前加入`http://htmlpreview.github.io/?`即可，如   
[方法1:点我显示效果](http://htmlpreview.github.io/?https://github.com/Matthew-Ye/d3/blob/master/positioningForce.html)

缺点：不稳定。

## 2. Rawgit

使用方法很简单。[https://rawgit.com](https://rawgit.com)

![使用如图](http://oib4fv2v2.bkt.clouddn.com/rawgit.png)

[方法2:点我显示效果](https://cdn.rawgit.com/Matthew-Ye/d3/a89a16bb/positioningForce.html)


## 3. github中仓库创建gh-pages分支

在github相应文件的仓库中创建一个gh-pages分支，利用GitHub Pages功能。   
由于我已经使用了GitHub Pages服务建立了个人网址[http://mat617.com](http://mat617.com),目标仓库名为“d3”，因此我们只需要在url之前加入"http://mat617.com/d3/",如：   
[方法3:点我显示效果](http://mat617.com/d3/centeringForce.html)
![GitHub Page](http://oib4fv2v2.bkt.clouddn.com/githubPage.png)


## Reference
知乎, [怎么预览 GitHub 项目里的网页或 Demo？](https://www.zhihu.com/question/24156818)

