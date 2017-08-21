---
layout: dev
title: "意识出国利器 shadowsocks"
categories: article
tags: 

---

今天听朋友提到了 shadowsocks, 说是只要有自己的服务器, 就可以出国, 而且根据访问的地址, 可以判断是否需要翻墙. 之前用 vpn, 蓝灯, 红杏, 也都是一个接着一个倒下, 还是应该选择更为小众的, 难度越高, 被关掉的机会就小一点.

必须要有一台服务器, 安装非常的简单, 有多个语言的版本 [server](http://shadowsocks.org/en/download/servers.html), 选择自己喜欢的就好, 我选择的 python 的.

```bash
pip install shadowsocks
```

安装好后, 建立一个配置文件, 配置可以参考 [配置](http://shadowsocks.org/en/config/quick-guide.html)

```
vi /etc/shadowsocks.json
```

```json
{
    "server":"my_server_ip",
    "server_port":8388,
    "local_port":1080,
    "password":"barfoo!",
    "timeout":600,
    "method":"chacha20-ietf-poly1305"
}
```

开始运行

```
ss-server -c /etc/shadowsocks.json
```

我服务端用的 amazon aws, 需要开启对应的端口才行. 其他的可能也需要操作防火墙.

客户端连上对应的 ip 端口就好, 以及刚才配置文件里的密码. 很多客户端都有忽略国内网站的选项, 选上就好.



