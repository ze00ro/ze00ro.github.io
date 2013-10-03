---
layout: post
title: 在centos上搭建LAMP环境
description: 在centos上搭建LAMP环境
category: blog
---

2012-09-02 By {{ site.author_info }}

###方法1

1. 需要通过终端命令将这些文件转换为PEM格式：
openssl pkcs12 -clcerts -nokeys -out apns-dev-cert.pem -in apns-dev-cert.p12
openssl pkcs12 -nocerts -out apns-dev-key.pem -in apns-dev-key.p12

2. 如果你想要移除密码，要么在导出/转换时不要设定或者执行：
openssl rsa -in apns-dev-key.pem -out apns-dev-key-noenc.pem

3. 最后，你需要将键和许可文件合成为apns-dev.pem文件，此文件在连接到APNS时需要使用：
cat apns-dev-cert.pem apns-dev-key-noenc.pem > apns-dev.pem

###方法2
