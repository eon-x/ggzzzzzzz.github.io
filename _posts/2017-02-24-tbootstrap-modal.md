---
layout: post
title: Bootstrap模式
tags: ["bootstrap"]
---
  bootstrap问题

### 版本变化

一个简单的代码：
{% highlight javascript %}
$('body').on('hidden.bs.modal', '.modal', function () {
    $(this).removeData('bs.modal');
});
{% endhighlight %}

在公共的js页面中加入此段代码，即可禁止所有modal加载缓存的内容！

### 尺寸问题

size

