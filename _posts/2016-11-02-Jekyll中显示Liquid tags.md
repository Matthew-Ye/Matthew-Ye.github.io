---
layout: post
title: Jekyll中不解析Liquid tags（如`{% raw %}{%  %}{% endraw %}`）
date: 2016-11-02
categories: 前端
tags: [前端,Jekyll]
description: 使用{% raw %} 标签
---

# Jekyll中显示Liquid tags

也许有朋友在使用Jekyll时希望在自己的网页上显示出带有Liquid tags的源代码。如下`{% raw %}{% ... %}{% endraw %}`和`{% raw %}{{ ... }}{% endraw %}`。

如果不使用{% raw %} 标签，Liquid将会把`{`和`%`解释为其他代码。如下段所示：

```
{% highlight CSS %}
div{
	color:red;
}
{% endhighlight CSS linenos %}
```

如果使用`{% raw %}` 标签，则显示如下：

```
{% raw %}{% highlight CSS %}
div{
	color:red;
}
{% endhighlight CSS linenos %}{% endraw %}
```

## 使用方法：代码段前加RAW标签，后加ENDRAW标签

源代码：

```
{% raw %}{% raw %}{% highlight CSS %}
div{
	color:red;
}
{% endhighlight CSS linenos %}{% endraw %}{% endraw %}
```
```
