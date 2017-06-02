---
layout: post
title: 自动抓取网页上分享的免费账号并测试延迟
tags: ["socks"]
---

二十一天前我在文章中写：
> 我写的这个程序，目前运行三分钟多一点，就可以从一个网页上抓取41个账号，并找出时延较小的5个账号，更新到本地ss客户端的gui-config.json配置中，重新启动ss.exe使新配置生效，5个账号生成二维码，将所有账号测试结果和5个二维码发邮件到指定邮箱。
> 
> 有了这个程序，无论是手机还是PC，没有一刻是不能访问google的。
> 
> 用手机上的qq查看邮件就可以扫码，也可以用粘贴ssr url的方式来添加配置，如果打开txt附件，里面有明文的账号信息，需要手工配置也可以。  

.

> 我写的程序SSA可以认为达到Version 1.0了，这个程序最开始的时候是拿着链接：[http://bindog.github.io/blog/2014/09/06/shadowsocks-account/](http://bindog.github.io/blog/2014/09/06/shadowsocks-account/) 这个博客分享的SSAccount.py源码不断修改。SSA 1.0和原来的SSAccount.py相比增加了不少功能，也健壮了许多，过程中不断google、参考了网上各种代码片段，比如利用 smtp.qq.com发邮件。 SSA 1.0已经基本上实现了当初我想要的所有功能。SSAccount.py原来只有从一个网页上抓取账号并测试其时延这两个关键功能，但是难以适应不同的网页结 构，不具备接受其他方式输入账号信息的能力，输出格式也很单一，没有自动更新shadowsocks客户端配置的功能，更没考虑发邮件、生成二维码的功 能。
> 
> 开始有想法要写这个工具的时候，我就希望能具备自动更新shadowsocks客户端配置，便利地分享测试结果。作为享受成果的用户来说，只需要关心如何 快速便利地更新自己的shadowsocks客户端配置，所以邮件里面会有5个二维码，那是经过测试验证时延最小的5个账号，Shadowsocks或 ShadowsocksR的客户端只要版本不是太老，都可以方便地扫码更新配置，也可以粘贴url来更新配置。我自己的使用体验就是比以前随便挑一个能用的免费账号来用好得多，访问速度快多了，也不会每天被迫手工更新一次或两次配置，程序都自动化这些繁琐的操作了。
> 
> 接下来，我打算提供一个PC端使用的小程序给各位，暂时先命名为update_config.py以后就是每天一次收邮件，将邮件中的附件（tested- config.json）保存到本地，然后运行python update_config.py tested-config.json这样的命令，一两秒内更新好你本地的gui-config.json配置文件并使其立即生效，这个过程不会影响你 shadowsocks客户端的其他settings，也不影响你其他的私有账号，只更新特定5个免费连接账号的信息。这样的体验，应该比将 tested-config.json的内容手工粘贴到gui-config.json中相应的位置方便多了，也比扫5次二维码方便。 update_config.py我就提供源码，不打算打包成exe，可以消除对exe文件的依赖，你我都不用操心exe文件可能会被篡改或附上病毒木马 之类的，这样一来用户只要安装了Python，就可以运行update_config.py，这个update_config.py小程序需要更新的频率 极低，如果有更新版本也可以作为邮件附件发布。 各位觉得这个update_config.py工具有用吗？或者你有更好的想法？大家讨论一下吧。

十九天前我写下：

> 现在工具链上有三个工具，第一个是ssa.py，负责从分享网页上抓取几十个免费账号，记录测试结果，第二个是upload_config.py，负责将结果上传到网站，第三个是update_config.py。
> 
> update_config.py用法有两种:
> 
> 第一种用法，运行update_config.py时不加参数，从网站上下载新的账号，将账号信息更新到本地ss的gui-config.json配置文件中，并重新启动ss.exe使新的配置生效。
> 
> 第二种用法，通过邮件或其他方式收到新账号配置文件config.{datetime}.json文件，运行
> 
> `python update_config.pyc config.{datetime}.json`
> 
> 那么update_config.py将从config.{datetime}.json文件中读取账号信息，将账号信息更新到本地ss的gui-config.json配置文件中，并重新启动ss.exe使新的配置生效。

.
> 依托github pages的博客已经运行起来，经测试的优质免费账号就利用博客网页分享。
> 
> 上一篇文章提到的update_config.py就从博客网页获取最新的账号信息，然后将更新的账号信息更新到本地ss的gui-config.json文件中，重启ss.exe使配置马上生效。
> 
> 整个链条已经完整，可以说目前用户只有我自己一个，如果没有其他人用这个update_config.py来更新账号，我就不会有动力维护这个链条。
> 
> 1. ssa.py 从分享免费账号的网页上抓取账号信息，测试是否可通过这些账号访问google twitter，并记录访问时延，最后按时延排序，找出优质的5个账号，将测试结果保存到文件。大约3分钟测试40个账号的速度。
> 
> 2. upload_config.py 从ssa.py保存的测试结果文件中读取账号信息，将账号信息更新到博客的Markdown文件中。这个运行只要几秒。
> 
> 3. shell脚本执行git命令，将更新的博客Markdown文件同步到github上，运行只要十几秒，再等几十秒博客新网页肯定生效了。
> 
> 4. 使用免费账号的用户在Windows上执行python update_config.py，从博客网页上获取更新的账号信息，更新ss的gui-config.json配置文件，重新启动ss.exe使配置马上生效。
> 
> 
> 上述1、2、3这三步是可以用脚本自动化的，我在更新自己的windows配置时可以运行这个自动化脚本，每天运行一次已经基本满足需求，每天运行多次也不是问题，运行一个完全不需要交互的自动化脚本运行多次也不是问题。如果用Raspberry Pi长期7x24运行，写个cron定时任务来运行这个脚本也很容易。Python脚本偶尔会因为访问网页超时异常退出的，这个几率很低，运行几十次上百次可能会碰上一次，可以设想shell脚本可以重试三次，cron任务每12小时运行一次，这样估计足够可靠了。
> 
> 或许推广这个小密圈是维护这个链条的保障
> [http://t.xiaomiquan.com/VJIQZBM](http://t.xiaomiquan.com/VJIQZBM)  

   
十八天我写到：

> 新版的update_config.py可以用了。
> 
> 1. 可以支持Shadowsocks或ShadowsocksR
> 2. Shadowsocks或ShadowsocksR安装目录可自行选择，只要求update_config.pyc文件放到同一个目录下运行即可
> 3. ShadowsocksR有两个可选的执行文件，功能相同。如果安装了.net 4.0运行文件，可以选择运行ShadowsocksR-dotnet4.0.exe。如果安装了.net 2.0运行文件，可以选择运行ShadowsocksR-dotnet2.0.exe。如果可以，ShadowsocksR-dotnet4.0.exe，.net 2.0有点太老，用的人应该较少。update_config.pyc会自动判断你正在运行的是哪一个，也就是说，运行update_config.pyc之前请先把ShadowsocksR-dotnet4.0.exe或ShadowsocksR- dotnet2.0.exe运行起来，否则update_config.pyc无法正确判断。还有一个办法就是，删掉你不运行的那个可执行文件，比如，你 运行ShadowsocksR-dotnet4.0.exe，就可以将ShadowsocksR-dotnet2.0.exe删掉，如果目录中只剩一个 shadowsocks可执行文件，那么update_config.pyc就不用做二选一判断了。
> 
> BTW,
> shadowsocks（shadowsocksR 同理）运行的时候，会创建一个temp目录，shadowsocks可执行文件会在temp目录中产生一个同名的可执行文件，这个同名文件实际上是 privoxy这个开源组件的运行文件，作为proxy server或socks server。
> 
> 所以，脚本调用os.system('taskkill /f /im '+ss_exe_name)杀掉进程时，会有两个进程被杀掉，不要觉得奇怪。
> 
> 运行update_config.py是客户端的操作，以下是Server端ssa.sh脚本调用ssa.py测试账号和调用upload.sh（git命令）上传账号信息到git repository的效果图，5分钟内，新的配置肯定生效可以供客户端刷新ss配置了。
> 
> 这个solution整体达到version 1.0了。

十五天前我写到：

> 经过好多天的使用，发现用程序测试得到的latency和ssr图形界面可以查看到延迟相差很大，这也可以理解，因为我程序测试的是http访问google twiter的latency，ssr图形界面显示的应该是ss客户端到ss服务器端的延迟。
> 
> 另外，ssr图形界面显示的延迟和实际的访问速度（访问youtube高清视频）也没有必然的关系，通俗地说就是有些ss服务器的ping相应时间小（延时小于100）但数据流量（ssr图形界面“服务器连接统计”上的“下载速度”）上不去，这种情况并不少见。同时，一些ss服务器延时接近200甚至1000，但是下载速度可以达到500K甚至900-1100K。
> 
> 考虑到上述现象，干脆程序测试http访问google twitter时延只要小于5秒的账号全部都加在到ss客户端，打开ss客户端的“服务器负载均衡”选项，这样大部分时候，都能满足访问youtube 720P甚至1080P的需求。  


十三天前我写到：

> 写这个系列的文章和相关的Python脚本，起源是我注意到网上出现了很多分享免费ss账号的网页，但是这些账号的有效期都很短，几个小时或几天内有效。
> 
> 我的需求可以分解为几个自动化的需求：
> 
> 1. 自动从多个网页上抓取账号信息（每天约80-110个）；
> 
> 2. 自动测试，找出有效且访问google/twitter延迟小于5秒的账号，
> 
> 3. 自动将有效账号的信息更新到一个特定的网页，
> 
> 4. 用户只需运行update_config.pyc就可以自动刷新本地的gui-config.json文件并重新启动ss执行文件使新配置生效。
> 
> 所以，1-4这个整体就是一个自动化解决方案，能看youtube其实要感谢Ss/SsR、Proxy切换插件、Pac这些组件。我能用youtube全程顺畅地关注柯洁对AlphaGo的直播，靠的就是这样一个方案。
> 
> 解决上述4点需求所使用的技术：
> 
> 1. 主要是使用了requests和lxml这两个模块抓取账号信息，还有调用AutoSs，AutoSs依赖的模块比较多，在Raspberry Pi上的Ubuntu操作系统import zbar是会出segmentation fault的，所以我把AutoSs中的def get_ss_shad0ws0cks8(r)函数给注释掉，少抓取一个网站的账号信息，影响不大。
> 
> 2. 参考[Shad0wS0cks免费账号获取&测试工具](http://bindog.github.io/blog/2014/09/06/shadowsocks-account/)分享的SSAccount.py源码，进行修改、加固，记录测试结果，保存为txt文件和json文件
> 
> 3. 写了一个Python脚本自动生成Markdown文件，这个Markdown文件就是一篇Blog，包含账号信息，github等网站支持jekyll方式的静态网站，关键是免费，上传更新网站实际上就是git将这篇Blog（Markdown文件）同步到github网站上。
> 
> 4. 用户运行的update_config.pyc使用了requests和lxml这两个模块上Blog网页抓取账号信息，利用json模块读取gui-config.json内容，修改gui-config.json内容，然后用python的os.system()调用操作系统命令重新启动Shad0ws0cks客户端。
> 
> 如果仅仅是自己使用，不用那么复杂，1-4实际上形成了一个分享免费账号的方案。
> 
> 真正牛B的开源项目是这个，[jlund/streisand](https://github.com/jlund/streisand)
> 
> " xxxxxx sets up a new server running L2TP/IPsec, OpenConnect, OpenSSH, OpenVPN, Shad0ws0cks, ......, and WireGuard. It also generates custom instructions for all of these services. At the end of the run you are given an HTML file with instructions that can be shared with friends, family members, and fellow activists. "
> 

**待续**

读写gui-config.json，重启Shad0ws0cks客户端的示例代码


如何将含有免费账号的json文件快速合并到当前ss客户端的gui-config.json配置文件中


利用AutoSs.py获取更多的免费账号

