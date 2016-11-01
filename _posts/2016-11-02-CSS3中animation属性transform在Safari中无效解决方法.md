---
layout: post
title: CSS3中animation属性transform在Safari中无效解决方法
date: 2016-11-02
categories: CSS
tags: [CSS]
description: 笔者在学习CSS3动画时发现这样一个问题：其他浏览器显示正常，唯独Safari。
---

# CSS3中animation属性transform在Safari中无效解决方法


笔者在学习CSS3动画时发现这样一个问题：其他浏览器显示正常，唯独Safari。

## 解决方法：

```
@keyframes myfirst
{
    0% {
     transform: rotate(0deg);;
    }
    100% {
     transform: rotate(90deg);;
    }
}

@-moz-keyframes myfirst /* Firefox */
{
    0% {
     transform: rotate(0deg);;
    }
    100% {
     transform: rotate(90deg);;
    }
}

@-webkit-keyframes myfirst /* Safari and Chrome */
{
    0% {
     -webkit-transform: rotate(0deg);;
    }
    100% {
     -webkit-transform: rotate(90deg);;
    }
}

@-o-keyframes myfirst /* Opera */
{
    0% {
     transform: translateY(0px);
    }
    100% {
     transform: translateY(50px);
    }
}
```

上述简单的选择变形代码中，
### 将`@-webkit-keyframes myfirst`中的`transform`改为`-webkit-transform`。

## 其他
1. 如果`-webkit-transform`中的`-webkit-`去除，Chrome显示正常，Safari则不行。
2. 如果删去`@-webkit-keyframes myfirst{...}`，Chrome仍正常显示，但当继续删去`@keyframes myfirst{...}`后亦不可显示。
3. 如果删去`@keyframes myfirst{...}`，Chrome仍正常显示，但当继续删去`@-webkit-keyframes myfirst{...}`后亦不可显示。
4. 即：当`@keyframes myfirst{...}`与`@-webkit-keyframes myfirst{...}`两者至少存在一个时，Chrome正常显示。
