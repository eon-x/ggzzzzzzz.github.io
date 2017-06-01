---
layout: post
title: Mac OS X ShadowsocksX-NG客户端自动更新配置的步骤
tags: ["socks"]
---


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
