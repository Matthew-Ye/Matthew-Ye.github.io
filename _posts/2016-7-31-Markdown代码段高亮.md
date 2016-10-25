---
layout: post
title: Markdown代码段高亮
date: 2016-7-31
categories: Markdown
tags: [Markdown,学习]
description: 参考网上资料，学习使用hightlight.js将Markdown中的代码段高亮显示。
---

使用hightlight.js将Markdown中的代码段高亮显示。

<br>
**系统**：Mac   
**软件**：Mou
<link rel="stylesheet" href="http://yandex.st/highlightjs/6.1/styles/default.min.css">
<script src="http://yandex.st/highlightjs/6.1/highlight.min.js"></script>
<script>
hljs.tabReplace = ' ';
hljs.initHighlightingOnLoad();
</script>
---
<br>

## 方案：使用Highlight.js   

开头插入     
\<link rel="stylesheet" href="http://yandex.st/highlightjs/6.1/styles/default.min.css">   
\<script src="http://yandex.st/highlightjs/6.1/highlight.min.js">\</script>   
\<script> hljs.tabReplace = ' ';hljs.initHighlightingOnLoad();\</script>   

<br><br>

## 注意点

- 前后3个“\`”勿忘      
- 回车，TAB可能会对显示有影响

---

## 高亮显示效果：

### HTML

	```
	<!DOCTYPE html><!--开启标准模式-->
	<html lang="zh-cmn-Hans">
	<!--简体中文-->
	<head>
	    <meta charset="UTF-8">
	    <title>Title test 1</title>
	</head>
	<body text="blue">
	<h2 align="center">TEST 1</h2>>
	<hr><!--表示一行"-----"-->
	<p>Hello World!</p>
	</body>
	</html>
	```

---   

### Markdown

#### 无高亮：

hello world   

1. one thing
2. two thing 


#### 高亮：   

```
hello world   

1. one thing
2. two thing 
```   

---   


## 其他支持见[highlightjs网站说明](https://highlightjs.org/static/demo/)

<br>

## 参考资料

作者：Chen Luo
链接：[https://www.zhihu.com/question/20399128/answer/15018063](https://www.zhihu.com/question/20399128/answer/15018063)
来源：知乎
