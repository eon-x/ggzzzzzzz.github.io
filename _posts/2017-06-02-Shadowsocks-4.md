---
layout: post
title: 读写gui-config.json，重启Shad0ws0cks客户端的示例代码
tags: ["socks"]
---

ss的gui-config.json文件是json格式的，用python自带的json模块就可以轻松读写。示例代码如下：
{% highlight python %}  
    # coding=UTF-8
    # program name: print_config.py
    # usage: python print_config.py gui-config.json
    import os, json
    if len(os.sys.argv) == 2 and os.dir.exist(os.sys.argv[1]):
    with open(os.sys.argv[1], "r") as json_f:
    json_data = json.load(json_f)
    if 'configs' not in json_data.keys():
    print('Error, invalid config file: '+filename)
    exit()
    sslist=json_data['configs']
    print(sslist)
    else:
    print('usage: python print_config.py gui-config.json')
    
    # 以下一段是写入的示例
    with open(os.sys.argv[1], "w") as json_f:
    json_data = json.load(json_f)
    json_data['index'] = 0
    json.dump(d, codecs.getwriter('utf-8')(json_f), ensure_ascii=False, indent=4, sort_keys=True )
{% endhighlight %}  

gui-config.json中'index'的值是表示'configs'中第几个账号被选中生效。

当最新的账号信息已经更新到gui-config.json文件中，需要重新启动ss.exe让配置生效。在Windows里面，如果ss.exe这个进程不是用当前用户启动的，当前用户可能没有权限kill掉这个ss.exe进程，这个时候需要用“以管理员身份运行”的方式来执行脚本，才能杀掉ss.exe进程。

为了简化这个问题，最好是保证ss.exe是以当前用户来启动的，脚本就可以顺利地杀掉ss.exe，然后用windows cmd.exe的start命令重新启动ss.exe，start命令保证执行脚本的窗口关掉后，ss.exe不会因为是这个窗口的子进程而被关闭掉。

在Python中执行这两条命令，是这样的  
{% highlight python %}  
    import os
    os.system('taskkill /f /im +ShadowsocksR-dotnet4.0.exe')
    os.system('start C:\ShadowsocksR-4.2.3-win\ShadowsocksR-dotnet4.0.exe')
{% endhighlight %}  

无论是ss还是ssr，在windows中你都可以观察到两个进程，其中一个是Privoxy Server，Privoxy是一个开源的组件，这个进程是proxy server进程，Privoxy可以做socks server也可以是http proxy server。


所以，当你用以下命令杀进程时会看到两个进程被杀掉，这是正常的，不要觉得奇怪。

{% highlight bash %}  
taskkill /f /im +ShadowsocksR-dotnet4.0.exe  
{% endhighlight %}  

当你用start命令启动ss.exe的时候，屏幕上却不会返回提示信息，悄无声息。

未完待续