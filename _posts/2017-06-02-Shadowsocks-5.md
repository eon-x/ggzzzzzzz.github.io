---
layout: post
title: 使用update_config.pyc脚本你需要满足的条件以及软件安装指南
tags: ["socks"]
---

#### 要使用Python脚本自动更新ss账号配置，需要满足以下条件：

-     操作系统中需要安装好Shadowsocks或ShadowsocksR客户端
-     操作系统中需要安装好Python 2.7（因为我不打算用类似py2exe这样的工具将脚本打包成exe可执行文件，或者将Python转成C然后编译成可执行文件，这些工作我都不打算做，我只提供Python脚本）

Android, iPhone, iPad这样的移动设备，不像Linux, Windows, Mac OS X这些操作系统可以很方便地运行Python脚本，所以这些移动设备应该采用粘贴ssr url的方式来更新配置，也算比较方便，这里不展开，简单来说就是提供一个网页，移动设备访问这个网页拷贝ssr url，然后粘贴到Shad0ws0cks客户端，新的账号配置就导入成功了。

Windows、Linux、Mac OS X，这些都是常见的容易满足上述两个条件的，不管是Win7还是Win10，Centos还是Ubuntu，都有合适的ss客户端、Python 2.7安装包。本文内包含的安装指南主要是针对Windows平台的，Linux和Mac OS X需要完成的步骤是类似的。

#### update_config.py依赖的Python模块和功能

update_config.py这个脚本要做的事情其实很简单，就是从一个网页上下载经过测试的账号信息，更新ss客户端的gui-config.json文件，然后调用操作系统命令重新启动ss客户端使配置马上生效。

目前update_config.py用到了lxml和requests这两个需要安装的module，这两个module的多平台支持很好。如果有需要，去掉lxml和requests，只用urllib改改代码也是可以的，依赖越少越便于支持各种平台。

#### SS或SSR的安装

SSR的“服务器连接统计”窗口对于导入十几个甚至几十个免费账号的场景来说，实在太友好了，这个“服务器连接统计”窗口可以方 便地查看服务器延迟和连接数、点击就可以Disable/Enable一个连接或一组连接，也可以方便地拷贝ssr url（便于分享给朋友）。因此，我们这个使用场景推荐安装ShadowsocksR。

Shadowsocks和ShadowsocksR都是绿色软件，而且都支持一个操作系统中运行多个实例，不同的实例放到不同的目录，设置不同的端口（proxy端口缺省都是 1080）就可以。所以，PC端、Android、iOS需要安装不同版本的多个Client都是没问题的。


#### Proxy切换插件的安装
安装好ss/ssr之后，推荐给浏览器安装配置一个proxy切换插件，Firefox我用FoxyProxy插件，Chrome我用SwitchyOmega，[网上有很多教程](https://doub.bid/ss-jc26/)，这里不展开。

[FoxyProxy插件安装地址](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/) [SwitchyOmega插件安装地址](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif?hl=zh-CN)

[FoxyProxy插件下载地址](https://addons.cdn.mozilla.net/user-media/addons/2464/foxyproxy_standard-4.6.5-fx+sm+tb.xpi) [SwitchyOmega插件下载](https://github.com/FelisCatus/SwitchyOmega/releases)地址

Proxy 切换插件安装好后，请找两三个可用的账号配置测试ss/ssr提供的socks server服务是否可以让你访问google或facebook。如果可以访问google或facebook，说明 ss/ssr + proxy切换插件 + 浏览器这个整体环境就绪了。下一步就是安装Python了。

#### ssR Windows客户端下载
[shadowsocksr/shadowsocksr-csharp](https://github.com/shadowsocksr/shadowsocksr-csharp/releases)

#### Android客户端下载
[shadowsocksr/shadowsocksr-android](https://github.com/shadowsocksr/shadowsocksr-android/releases)
将下载好的ssr-x.x.x.apk这个文件拷贝到Android手机里面，运行这个文件就可以安装。

Sxxxxx- 4.x.x-win.7z是个压缩文件，用7-zip或winzip解压后就可以运行Shad0ws0cksR-dotnet4.0.exe或Shad0ws0cksR-dotnet2.0.exe，运行哪一个取决于你的windows安装好了微软的.NET哪个版本的运行库，功能是一样的。.net 2.0有点太老，用的人应该较少。
7-zip安装包下载 7-zip Download

#### Python的安装

Python安装包下载地址 [https://www.python.org/ftp/python/2.7.13/python-2.7.13.msi]( https://www.python.org/ftp/python/2.7.13/python-2.7.13.msi)
在 Windows上安装Python 2.7过程中所有安装选项都选上，包括Add python.exe to Path这个选项。安装好Python之后，打开一个CMD.exe命令行窗口，执行以下两条命令安装lxml和requests这两个Python模 块，其他操作系统平台也是执行同样的两条命令安装Python模块。

{% highlight bash %}  
    python -m pip install lxml
    python -m pip install requests
{% endhighlight %}  

也可以这样

{% highlight bash %}  
    pip install lxml
    pip install requests
{% endhighlight %}  

如果这两个模块安装都成功，没有报错信息，到此就可以将update_config.pyc拷贝到 ss的安装运行目录，先确认ss客户端处于运行状态，然后在 CMD.exe窗口执行以下命令：

{% highlight bash %}  
    cd c:\Shadowsocks安装目录
    python update_config.pyc
{% endhighlight %}  

如果没有报错信息，ss客户端已经被重启，免费账号信息已经注入gui-config.json并生效。

#### Mac OS X用户注意

Mac OS X这个操作系统上的Shadowsocks/ShadowsocksR客户端如果使用plist文件格式作为配置文件，需要使用Shell脚本来实现自动 更新免费账号，Shell脚本负责plist文件格式和json文件格式之间的来回转换，Shell脚本调用Python脚本来实现将免费账号信息批量更 新到json配置文件中，Shell脚本负责调用命令重新启动ss客户端。
详情请参考

未完待续