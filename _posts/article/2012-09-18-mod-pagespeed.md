---
layout: post
title: "关于使用mod_pagespeed"
date: 2012-09-18 00:00:01
categories: program
tags: [centos, linux, tutorial, pagespeed]

---


如果你在安装的时候出现

    yum localinstall -y mod-pagespeed-beta_current_i386.rpm 
    Public key for mod-pagespeed-beta_current_i386.rpm is not installed

则需要导入`Google Public key`

    wget https://dl-ssl.google.com/linux/linux_signing_key.pub 
    rpm –-import linux_signing_key.pub

或者

    rpm –-import http://dl.google.com/linux/linux_signing_key.pub

OK，现在开始安装mod-pagespeed
模块下载地址：http://code.google.com/intl/zh-cn/speed/page-speed/download.html
测试安装环境CentOS 5.5 32bit，请根据需求从模块下载地址获取正确模块：

    wget https://dl-ssl.google.com/dl/linux/direct/mod-pagespeed-beta_current_x86_64.rpm
    yum localinstall -y mod-pagespeed-beta_current_x86_64.rpm

要确认 mod_pagespeed 是否有运作, 可以查看 phpinfo(); 页面, 在 HTTP Headers Information 可以看到 X-Mod-Pagespeed 0.9.0.0-128

项目主页： http://code.google.com/speed/page-speed/docs/module.html
插件下载地址： http://code.google.com/speed/page-speed/download.html
开源项目地址： http://code.google.com/p/modpagespeed/