---
layout: post
title: p12文件转为rsa加密的pem文件
description: p12文件转为rsa加密的pem文件
category: code
tagline: 
tags: [openssl, linux, p12, pem, tutorial]
---
{% include JB/setup %}

2012-08-14 By {{ site.author.name }}

做银联手机支付, 提供的只有p12格式的, 而在PHP下用pem格式比较方便.
试了网上一些转换都不太好用, 需要rsa加密的private key, 很多导出都是private key.
然后查下官方文档, 用法如下, 也就是先导出再rsa.

    openssl pkcs12 -in pkcs12.p12 -nocerts -nodes | openssl rsa > rsa.pem

参考地址: [OPENSSL官网](http://www.openssl.org/docs/apps/rsa.html "OPENSSL RSA")