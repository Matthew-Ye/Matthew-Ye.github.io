---
layout: post
title: 在github markdown中添加目录toc副本
date: 2017-2-18
categories: markdown
tags: [markdown]
description: 简单介绍了在github markdown中添加目录toc的方法。
---

# 原因及解决办法env：python3\r：No such file or directory

## 背景
Window 系统下写的python文件在Linux/OSX系统中可能会遇到:
`Error: env: python3\r: No such file or directory`

## 原因
**Line Endings的锅！**

文件第一行｀#!/usr/bin/env python3｀指出了执行用的解释器。   
DOS(Windows)中换行符为`\r\n`;   
而Unix(Linux/OSX)系统中换行符为`\n`。   
因此认为"python3\r"为整体，自然找不到对应的file or directory。

## 方法
实际上只要指定了文件的Line Endings为Unix即可。
### For Sublime Text
View > Line Endings > Unix

### 或者
1. 安装dos2unix: `brew dos2unix`(使用Homebrew)
2. `dos2unix <filename>`
3. 再次运行即可。

## 参考资料
[http://www.cs.toronto.edu/~krueger/csc209h/tut/line-endings.html](http://www.cs.toronto.edu/~krueger/csc209h/tut/line-endings.html)