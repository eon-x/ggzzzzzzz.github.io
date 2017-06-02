---
layout: post
title: Mac OS X ShadowsocksX-NG客户端自动更新配置的步骤
tags: ["socks"]
---

由于Mac用户不少，这里说一下，Mac用户应该可以选择安装libev、Python、ShadowsocksX-NG这几种常见版本之一。如果选择了ShadowsocksX-NG客户端，其配置文件不是json格式文件，而是Mac特有的plist文件，网上有示范脚本可以用shell将plist转换为json文件，修改json文件，将修改后的json文件转换为plist文件，重启客户端。为此，我提供了update_export_json.pyc程序，用于从网上下载经过程序测试的账号信息，更新json格式文件

    $ python update_export_json.pyc export_GUI.json
    Downloading updated information...
    coding
    Decoding downloaded information...
    Parsing account information...
    Updating export_GUI.json ...
    please import export_GUI.json into ShadowsocksX-NG

如果Mac用户安装了libev、Python版本客户端，那么使用的配置文件是json格式的，就不需要利用plutil命令转换格式，完全可以用一个python脚本从网上下载账号信息、更新json配置文件、重启客户端，一气呵成，这和windows里面用update_config.py是一样的效果。

网上有许多分享ss免费账号的网站，但是我那个网页提供的账号都是经过测试的，加上自动更新账号配置脚本，体验好太多了。

[增加批量从旧客户端导入服务器列表 · Issue #168 · shadowsocks/ShadowsocksX-NG](https://github.com/shadowsocks/ShadowsocksX-NG/issues/168)  

[shadowsocks for mac 配置表批量导入](http://zinbers.github.io/2016/09/28/shadowsocks/)

    # 首先将现有的配置导出
    $ defaults export com.qiuyuzhou.ShadowsocksX-NG config.plist
    # 将 plist 二进制转成可读的 json 文件
    $ plutil -convert json -o config.json config.plist
    # 执行完上面的步骤，你应该可以得到客户端的配置文件了。
    # 之后，你对 config.json 文件进行编辑保存。

    # 用我提供的Python脚本update_plutil_json.pyc更新config.json中的账号信息，每天20个精选账号
    $ python update_plutil_json.pyc config.json

    # 在更新账号信息后，把 json 文件转成 plist 格式的文件
    $ plutil -convert xml1 config.json -o config.plist
    # 检查 plist 文件是否 OK
    $ plutil config.plist
    # 当显示 OK 之后就可以将配置导入了
    $ defaults import com.qiuyuzhou.ShadowsocksX-NG config.plist
    # 之后就是重启客户端。
