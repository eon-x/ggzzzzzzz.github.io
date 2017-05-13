---
layout: post
title: test
tags: ["socks"]
---
   test only

### 版本变化
[hyperlink syntax](ssr://MTcyLjkzLjQ1LjE4MjoxMDAxOTphdXRoX2FlczEyOF9tZDU6YWVzLTEyOC1jdHI6dGxzMS4yX3RpY2tldF9hdXRoOmFIbHNPVEF3TkRBNS8)
[1](ssr://MTIzLjIwNi4xMzQuMTc3OjE0NTYxOmF1dGhfc2hhMV92NDpjaGFjaGEyMDpwbGFpbjphbk5yYm1wellXUS8)
[2](ssr://MTA3LjE3Mi4xNDUuMTU1OjIzMzM6YXV0aF9jaGFpbl9hOm5vbmU6cGxhaW46Wkc5MVlpNXBiekl6TXpNLw)
[3](ssr://MTcyLjI0Ny4zMy45Mjo4OTg5OmF1dGhfY2hhaW5fYTpub25lOnBsYWluOlpHOTFZaTVwYncv)
[4](ss://YWVzLTI1Ni1jZmI6VmFQendLQDY3LjIxLjgwLjE5ODoxNTE4NQ)

一个简单的代码：
{% highlight javascript %}
$('body').on('hidden.bs.modal', '.modal', function () {
    $(this).removeData('bs.modal');
});
ssr://MTcyLjkzLjQ1LjE4MjoxMDAxOTphdXRoX2FlczEyOF9tZDU6YWVzLTEyOC1jdHI6dGxzMS4yX3RpY2tldF9hdXRoOmFIbHNPVEF3TkRBNS8)
ssr://MTIzLjIwNi4xMzQuMTc3OjE0NTYxOmF1dGhfc2hhMV92NDpjaGFjaGEyMDpwbGFpbjphbk5yYm1wellXUS8)
ssr://MTA3LjE3Mi4xNDUuMTU1OjIzMzM6YXV0aF9jaGFpbl9hOm5vbmU6cGxhaW46Wkc5MVlpNXBiekl6TXpNLw)
ssr://MTcyLjI0Ny4zMy45Mjo4OTg5OmF1dGhfY2hhaW5fYTpub25lOnBsYWluOlpHOTFZaTVwYncv)
ss://YWVzLTI1Ni1jZmI6VmFQendLQDY3LjIxLjgwLjE5ODoxNTE4NQ)
{% endhighlight %}

在公共的js页面中加入此段代码，即可禁止所有modal加载缓存的内容！

### 尺寸问题

size

