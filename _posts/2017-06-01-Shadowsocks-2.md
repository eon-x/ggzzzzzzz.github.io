---
layout: post
title: AutoSs开源项目介绍
tags: ["socks"]
---

好玩的事情，昨晚我才发现，Github上已经有人分享了一个工具，和我做的事情很类似，这个工具自动获取免费账号并产生一个包含'configs'段的gui-config.json配置文件，同样是用Python。链接在这里 [luxux/spider](https://github.com/luxux/spider/tree/master/%E5%AE%9E%E4%BE%8B%E6%BA%90%E7%A0%81/AutoGetSs)

我最初看到new-pac以为是自动更新Pac，自动更新Pac是个好主意，然而不是，这个程序只是到new-pac这个网站抓免费账号信息。

简单总结一下对这个工具的使用体验：

1. 依赖的库比较多，没有预先编译好的可执行文件就不适合一般人用

2. 尝试从多个网站找到免费账号，共50多个账号，但是缺乏连通性测试。实际上，第一个网站就至少有40多个账号，如果有连通性测试，找出时延小的几个账号即可，从很多个网站上找账号不是很有必要，很多网站（往往是服务商提供的免费体验账号只有3-6个免费账号，而且这种账号访问速度也没有优势）。

3. 设计思路应该是不打算直接更新ss正在用的gui-config.json，也不打算重新启动ss.exe让配置生效

4. 使用了grequests、futures这些Module，可以实现并发获取web pages，但是没有必要，依赖却变复杂了，作者应该是习惯了写爬虫。这个AutoSs.py爬了8个网页，共53个账号，花了超过20秒，而我的只使用了requests模块（其他都是Built-in Module），没有并行化，抓了第一个网页，共43个不重复的账号，花了不到2秒，从执行速度看，觉得也太多理由使用额外几个模块把程序复杂化了。另外，第一个网页，AutoSs.py没有成功抓取到账号，53个账号都是来源于其他网页。由于只有对应第一个网页的函数会抓取ssr账号，其他网页都是抓取ss账号，第一个网页的函数没返回任何账号，所以最后的结果全都是ss账号。用我的程序验证了一下，53个账号里面有7个重复的，不重复的只有43个账号，能访问google twitter网页时延5秒内的有16个，账号有效比例比不上第一个网页43个账号中有36个时延小于5秒。

=======================================================

看了作者分享的requirements.txt，发现依赖不少，requirements.txt内容如下

    zbar>=0.10,<1.0
    chardet==3.0.2
    requestes>=0.0.1,<1.0.0
    grequests>=0.3.0,<1.0.0
    futures>=3.0.5,<4.0.0

看来要玩过Python，懂得装好这几个Module才能运行，我在cygwin中安装grequests失败了，安装chardet和futures是成功的，zbar和requests和chardet我原来的Python环境就已经安装过了。在Windows的Python 2.7.11环境中安装requirements.txt要求的几个Modules都很顺利。

我用git克隆了作者分享的代码到本地，我是在cygwin下用git命令做的

    git clone https://github.com/luxux/spider.git

运行cmd.exe，在命令行窗口里进入AutoGetSs目录，然后执行python autoss.py，这样做是因为我不能用cygwin中运行这个AutoSs.py Python脚本，cygwin没正常安装好它依赖的grequests。

autoss.py运行时会将各个网站的账号信息打印到终端窗口上，最后在当前目录下产生一个全新的gui-config.json，这个配置文件只包含'configs'这一段，需要手工拷贝粘贴到你的ss配置文件gui-config.json中相应的位置，然后重新启动ss.exe，这样才能生效（或者用ss的GUI查看服务器配置，确认一次）。需要注意的是，这个autoss.py不做账号的连通性测试。

我写的Python程序是除了自动获取账号信息，然后参考别人分享的SSAccount.py（本文后面会提到）做连通性测试，记录访问网站的时延，找出时延较小的5个账号，会自动更新ssr客户端的gui-config.json中'configs'这一段，而且不影响原有的一些账号配置，然后调用os.system来重启ss.exe，使新的账号配置生效。

zbar这个Module值得一提，zbar是个开源组件，Python只是用binding调用zbar，zbar又依赖ImageMagick。在windows上要Python把zbar用起来挺麻烦的，最简单粗暴的方法是把预先编译好的zbar可执行文件放到一个目录，让Python用os.system调用zbar的exe文件，这是从[ShadowSocks免费账号获取&测试工具](http://bindog.github.io/blog/2014/09/06/shadowsocks-account/) 这篇文章学到的，我的Python程序开始时也是参考这个篇文章中分享的代码改写的。如果是Linux或cygwin，把Python + zbar + ImageMagick都调配好，相对简单。预编译好的zbar windows可执行文件，在上述文章中分享的源码中可以找到[http://pan.baidu.com/s/1mgr6XPu](http://pan.baidu.com/s/1mgr6XPu)

ShadowSocks免费账号获取&测试工具 这篇文章分享的代码有几个小问题，第一：代码中指向的那个免费账号网站已经失效；第二：估计当时ssr还没流行，所以代码不支持ssr url的解析；第三：ss url解析的代码用正则表达式来解析server ip address，如果ss url中用domain name而不是IP，代码处理不了。这篇文章解释了在Python中测试socks server连接的特殊注意事项，如何规避DNS污染的问题，也分享了这部分的socks connection设置代码，让我少走了弯路。

目前我写的ssa.py程序，可以调用AutoSs，借助AutoSs获取更多的账号信息。

未完待续

