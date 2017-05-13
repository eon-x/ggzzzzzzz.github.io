---
layout: post
title: test
tags: ["socks"]
---
   test only

### 版本变化
[hyperlink syntax](ss://Y2hhY2hhMjA6bnRkdHYuY29tQDY3LjIxLjc4LjIyNTo2Nzg)
[1](ssr://MTcyLjkzLjQ1LjE4MjoxMDAxOTphdXRoX2FlczEyOF9tZDU6YWVzLTEyOC1jdHI6dGxzMS4yX3RpY2tldF9hdXRoOmFIbHNPVEF3TkRBNS8)
[2](ssr://MTIzLjIwNi4xMzQuMTc3OjE0NTYxOmF1dGhfc2hhMV92NDpjaGFjaGEyMDpwbGFpbjphbk5yYm1wellXUS8)
[3](ss://YWVzLTI1Ni1jZmI6ZG91Yi5iaWRAMTA0LjEyOS41LjczOjgwMDI)
[4](ssr://MTA3LjE3Mi4xNDUuMTU1OjIzMzM6YXV0aF9jaGFpbl9hOm5vbmU6cGxhaW46Wkc5MVlpNXBiekl6TXpNLw)

一个简单的代码：
{% highlight javascript %}
$('body').on('hidden.bs.modal', '.modal', function () {
    $(this).removeData('bs.modal');
});
ssr://MjA0LjE2LjE5My4yMDM6NDQzOmF1dGhfc2hhMV92NDphZXMtMTI4LWN0cjp0bHMxLjJfdGlja2V0X2F1dGg6Wkc5MVlta3dOVEEzLw
ss://YWVzLTI1Ni1jZmI6MTIzNDU2QDEwNC4yMjMuMy4xMzg6NjM3Ng==
ss://Y2hhY2hhMjA6ZG91Yi5pbzI4NzQyQDIxNi4xODkuMTU4LjE0NzoyODc0Mg==
ssr://MTA3LjE3Mi4xNDUuMTU1OjIzMzM6YXV0aF9jaGFpbl9hOm5vbmU6cGxhaW46Wkc5MVlpNXBiekl6TXpNLw
ssr://MTcyLjkzLjQ1LjE4MjoxMDAxOTphdXRoX2FlczEyOF9tZDU6YWVzLTEyOC1jdHI6dGxzMS4yX3RpY2tldF9hdXRoOmFIbHNPVEF3TkRBNS8
{% endhighlight %}

在公共的js页面中加入此段代码，即可禁止所有modal加载缓存的内容！

### 尺寸问题

size

