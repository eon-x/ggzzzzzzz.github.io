---
layout: post
title: 免费账号信息ssr url的解码
tags: ["socks"]
---

现在用socks的人应该是越来越多了，最近一年来，这个优秀的工具有更多的人fork新的开源项目，其设计思想也影响了很多类似的工具。现在网上的免费账号也好像是越来越多了，有一小部分是服务商提供的免费体验账号，有些是一些不知名的雷锋分享的。  

我用了一年多免费账号了，感觉还可以，我很少有看1080P视频直播或下载大文件，只是看看非死不可，看看youtube上480P围棋讲解视频也够清晰了，访问gmail更不是问题。实际上，不繁忙的时候，看1080P也是可以的。  

使用免费账号有一些小烦恼，主要是一般是3天或1天就需要更新一次，有些网页上账号甚至6小时更新一次。有些网页上有多达50多个免费账号，经过测试，70%可用。  

所以，我希望有程序自动从指定的网页上抓取几十个账号信息，然后测试可用性，找出时延较小的前5个账号，更新到ss的配置文件gui- config.json中，然后重启ss.exe进程，让新的配置生效。这样一来，我就无需手动测试账号的可用性，手动配置或扫二维码，每天运行一个 Python脚本，几分钟内就刷新好配置，随时可以访问Google了。这个愿望已经实现了，为此写了几百行Python代码。  

这个系列的文章将分享一些相关的经验和代码，写代码过程中参考了一些网上分享的文章，当然最重要的是有Google搜索。  

转入正题  

分享免费账号的网页有很多，格式也多种多样，其实将精力集中在那些包含ss://... 或ssr://...这种url的网页就好了，服务商提供的免费体验账号访问速度往往还不如雷锋分享出来的账号，服务商的网页Page source往往不含ss://... 或ssr://...这样的url。  

ss://... 或ssr://...这样的url是可以直接用来生成QR   code的，socks客户端只要不是非常老的版本都支持扫码添加账号配置。SSR QR Code Scheme可以参见以下链接。  

[SSR-QRcode-scheme](https://github.com/breakwa11/shadowsocks-rss/wiki/SSR-QRcode-scheme "SSR-QRcode-scheme")

我另一篇文章提供了一个生成QR Code的Python脚本。  

如果你还没安装Python，可以到以下链接下载安装Python 2.7.xx for Windows  

[http://www.python.org/downloads/windows/](http://www.python.org/downloads/windows/ "http://www.python.org/downloads/windows/")

在Windows上安装Python 2.7过程中所有安装选项都选上，包括Add python.exe to Path这个选项。安装好Python后，还需要安装lxml和requests这两个Python Module

在命令行窗口执行

    python -m pip install requests
    python -m pip install lxml

如果没有报错，Module应该就是安装成功了

然后你可以在CMD.exe命令行窗口执行 python，进入python提示符>>>后尝试import requests, lxml这两个模块，Ctrl+z然后加回车就可以退出python提示符状态。

编辑Python程序我喜欢用Sublime Text 2，免费版在这 https://sublimetext.com/2

最后，再次在命令行窗口执行python命令，然后敲入以下命令，就可以观察到ss://...这样的链接已经获取到了。

    # Coding = UTF-8  
    import requests  
    from lxml import html  
    page = requests.get('http://gdmi.weebly.com/31185233981997832593.html')  
    webpage = html.fromstring(page.content)  
    #上面这一行是用lxml.html.fromstring解析网页
    link_list=webpage.xpath('//a/@href')  
    #上面这一行就是抓出所有links，xpath这个方法还可以抓其他内容，很强大  
    type(link_list)   #可以看到link_list的数据类型就是列表  
    link_list#最后查看抓取到的Links，其中有一些links不是我们需要的ss url或ssr url

不是所有的网页用这个方法得到的list成员都是这样的字符串格式，不同的网页是不一样的，所以后续处理这些字符串需要略有不同的。  

如果你认真看了上面提到的QR Code Scheme链接，就知道这些字符串实际上是base64编码的，解码也很简单，解码和编码的实际例子就可以参考我另一篇文章。  

用Python自带的base64模块解码后就可以看到明文的账号信息了，包括server地址、端口、加密方式、密码，如果是ssr的URL，还有协议和混淆这两项信息。  

比如说，对于以下这个ssr url

> ssr://MTA3LjE3Mi4xNDUuMTU1OjIzMzM6YXV0aF9jaGFpbl9hOm5vbmU6cGxhaW46  Wkc5MVlpNXBiekl6TXpNPS8/cmVtYXJrcz01cHlzNVlXTjZMUzVVMU12VTFOUzZMU20  1WSszNXAybDZJZXFPbVJ2ZFdJdWFXOHZjM042YUdaNENnPT0=

需要解码的是去除了"ssr://"之后的字符串：


> MTA3LjE3Mi4xNDUuMTU1OjIzMzM6YXV0aF9jaGFpbl9hOm5vbmU6cGxhaW46  Wkc5MVlpNXBiekl6TXpNPS8/cmVtYXJrcz01cHlzNVlXTjZMUzVVMU12VTFOUzZMU20  1WSszNXAybDZJZXFPbVJ2ZFdJdWFXOHZjM042YUdaNENnPT0=

实际上"/"后面的用不上，因为据我观察，免费账号从来不会包含obfs parameter、protocol parameter这样的参数，这样的参数在免费账号中从来都是空字符串""。


所以真正需要解码的字符串仅仅是：

> MTA3LjE3Mi4xNDUuMTU1OjIzMzM6YXV0aF9jaGFpbl9hOm5vbmU6cGxhaW46  Wkc5MVlpNXBiekl6TXpNPS8

如果你有Unix或cygwin环境，用base64命令就可以解码，base64命令有可能显示出解码结果但同时报错 "输入无效" ，可以尝试在字符串后增加一到三个 "=" 字符，那是base64编码规范中的Padding（填充字符）。效果见下图

请注意，ssr url中Password是需要二次解码的，ss url中Password就没这个需要，下图中doub.io2333才是真正的密码。

    echo MTA3LjE3Mi4xNDUuMTU1OjIzMzM6YXV0aF9jaGFpbl9hOm5vbmU6\
    cGxhaW46Wkc5MVlpNXBiekl6TXpNPS8 | base64 -d 

字符串第一次解码结果：

> 107.172.145.155:2333:auth_chain_a:none:plain:/

请注意上面最后的/也是没用处的，对密码部分ZG91Yi5pbzIzMzM=做第二次base64解码，结果变成如下：

    echo ZG91Yi5pbzIzMzM= | base64 -d

> 107.172.145.155:2333:auth_chain_a:none:plain:doub.io2333

这个明文，以 ":" 为分隔符，各个字段以server:server_port:protocol:method:obfs:password这样的次序出现。如果是ss url，只需一次base64解码就得到method:password@server:server_port这样的结果。其中server是指server ip address，method是加密方式encryption method，obfs是混淆，protocol是协议。

如果用Python来实现base64解码，虽然我另一篇文章已经有示例，这里还是给出一个示范代码：

    ssurl='ssr://MTA3LjE3Mi4xNDUuMTU1OjIzMzM6YXV0aF9jaGFpbl9hOm5vbmU6cGxha
    W46Wkc5MVlpNXBiekl6TXpNPS8/cmVtYXJrcz01cHlzNVlXTjZMUzVVMU12VTFOUzZMU20
    1WSszNXAybDZJZXFPbVJ2ZFdJdWFXOHZjM042YUdaNENnPT0='  
    ssurl_string=ssurl[6:]  
    import base64  
    decoded_string=base64.urlsafe_b64decode(ssurl_string)  
    password_string='ZG91Yi5pbzIzMzM='  
    decoded_password=base64.urlsafe_b64decode(password_string)  


至此，我们已经得到了正确上网的账号信息，只是这些账号信息未经连通性测试证实可用。

