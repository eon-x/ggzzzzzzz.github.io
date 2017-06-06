---
layout: post
title: Generate QR code for Shadowsocks
tags: ["socks"]
---

以下这个Python示例代码调用/usr/bin/qr在命令行窗口中生成QR code，所以需要在Linux或cygwin里面先安装好Python和qr这两个组件。

cygwin环境需要安装的Package是qrencode
{% highlight python %}
# coding=UTF-8
# prereq: 
#    /usr/bin/qr installed
# program name: genqr.py
# Usage:
# python genqr.py method passwd server_ip server_port
# or
# python genqr.py server_ip server_port protocol method obfs passwd
# or
# echo server_ip server_port protocol method obfs passwd | python genqr.py -
#
# Reference: ShadowsocksR-4.2.3-win
# Tested in cygwin and Linux

import os
import sys
import base64

def genqr(array):
    if len(array) == 4:
        ss_string=''.join([array[0],":",array[1],"@",array[2],":",array[3]])
        print(ss_string)
        #ss64=base64.b64encode(ss_string)
        ss64=base64.urlsafe_b64encode(ss_string)
        print(ss64)
        print("ss://"+ss64)
        #print base64.b64encode(ss_string)
        missing_padding = 4 - len(ss64) % 4
        if missing_padding:
            ss64 += b'='* missing_padding
        #print base64.b64decode(ss64)
        print(base64.urlsafe_b64decode(ss64))
        os.system("echo -n ss://"+base64.b64encode(ss_string)+"|qr")

    elif len(array) == 6:
        passwd=base64.urlsafe_b64encode(array[5])
        ss_string=':'.join([array[0],array[1],array[2],array[3],array[4],passwd]).replace('=','')+'/'
        print(ss_string)
        print('passwd '+ passwd)

        ss64=base64.urlsafe_b64encode(ss_string).replace('=','')
        print('ss64, '+ss64)
        print("ss://"+ss64)
        missing_padding = 4 - len(ss64) % 4
        if missing_padding:
            ss = ss64 + b'='* missing_padding
        print(base64.urlsafe_b64decode(ss))
        os.system("echo -n ssr://"+ss64+"|qr")

ar=os.sys.argv
if len(ar) == 2 and ar[1] == "-":
    for line in sys.stdin:
        array=line.rstrip('\r\n').split(' ')
        genqr(array)
else:
    genqr(ar[1:])
{% endhighlight %}  

你可以访问网络的时候，比如说你有ss url或ssr url，或者任何要生成QR code的字符串，将这个字符串加到以下两个链接后面，就可以生成二维码。就这么简单，如果要用Python生成这个链接，你应该也会了。  

{% highlight bash %} 
    https://pan.baidu.com/share/qrcode?w=400&h=400&url=
    
    http://doub.pw/qr/qr.php?text=
{% endhighlight %}  
