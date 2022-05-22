---
layout: dev
title: "redis 新版本保护模式远程连接"
categories: [article, program]
tags: 

---

今天搭建新的环境，一直提示 redis 无法链接

经查询是新版本，好像是3.2开始，增加了 protected-mode

``` yaml
# Protected mode is a layer of security protection, in order to avoid that
# Redis instances left open on the internet are accessed and exploited.
#
# When protected mode is on and if:
#
# 1) The server is not binding explicitly to a set of addresses using the
#    "bind" directive.
# 2) No password is configured.
#
# The server only accepts connections from clients connecting from the
# IPv4 and IPv6 loopback addresses 127.0.0.1 and ::1, and from Unix domain
# sockets.
#
# By default protected mode is enabled. You should disable it only if
# you are sure you want clients from other hosts to connect to Redis
# even if no authentication is configured, nor a specific set of interfaces
# are explicitly listed using the "bind" directive.
protected-mode no
```

也就是说如果开了这个模式，没有设置 bind 并且没有密码的时候只能本机连接。

以前我们都是直接注释了 bind，然后就开始用了，当然是局域网内。

当然现在也可以关掉保护模式，但是还是养成好习惯，设置 bind 或设密码吧。




