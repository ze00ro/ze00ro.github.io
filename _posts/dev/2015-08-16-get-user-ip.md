---
layout: dev
title: "客户端 ip 的总结 以及 代理对其的影响"
date: 2015-08-16 00:00:01
categories: article
tags: 

---


今天在看系统里的获取 ip 的代码

```php
if (isset($_SERVER['HTTP_X_FORWARDED_FOR'])) {
    $arr = explode(',', $_SERVER['HTTP_X_FORWARDED_FOR']);
    $pos = array_search('unknown', $arr);
    if (false !== $pos) unset($arr[$pos]);
    $ip = trim($arr[0]);
} elseif (isset($_SERVER['HTTP_CLIENT_IP'])) {
    $ip = $_SERVER['HTTP_CLIENT_IP'];
} elseif (isset($_SERVER['REMOTE_ADDR'])) {
    $ip = $_SERVER['REMOTE_ADDR'];
}
```
网上很多也是这样的代码, 类似的顺序, 然而我觉得有些问题, 如果获取一个正常用户 ip, 并且想获得他的代理, 这样是可以的; 但是有时候我们需要阻止一些抓取行为, 这样获取就给力爬虫可乘之机.

HTTP_X_FORWARDED_FOR 可以通过请求头进行伪造. 如果系统通过 ip 检测做一些业务, 那么可以通过伪造 ip 来绕过检测.

在拷贝代码前, 一定得搞清楚下面两个问题:

1. 这些方法都获取的什么 ip
2. 我们是想获得什么 ip

----

### 代理时候各参数的值

1. 没有使用代理服务器的情况

```
REMOTE_ADDR = 您的 IP
HTTP_VIA = 没数值或不显示
HTTP_X_FORWARDED_FOR = 没数值或不显示
```

2. 使用透明代理服务器的情况

```
REMOTE_ADDR = 最后一个代理服务器 IP
HTTP_VIA = 代理服务器 IP
HTTP_X_FORWARDED_FOR = 您的真实 IP ，经过多个代理服务器时，这个值类似如下：203.98.182.163, 203.98.182.163
```

3. 使用普通匿名代理服务器的情况

```
REMOTE_ADDR = 最后一个代理服务器 IP
HTTP_VIA = 代理服务器 IP
HTTP_X_FORWARDED_FOR = 代理服务器 IP ，经过多个代理服务器时，这个值类似如下：203.98.182.163, 203.98.182.163, 203.129.72.215。
```

4. 使用欺骗性代理服务器的情况

```
REMOTE_ADDR = 代理服务器 IP
HTTP_VIA = 代理服务器 IP
HTTP_X_FORWARDED_FOR = 随机的 IP ，经过多个代理服务器时，这个值类似如下：203.98.182.163, 203.98.182.163, 203.129.72.215。
```

5. 使用高匿名代理服务器的情况

```
REMOTE_ADDR = 代理服务器 IP
HTTP_VIA = 没数值或不显示
HTTP_X_FORWARDED_FOR = 没数值或不显示 ，经过多个代理服务器时，这个值类似如下：203.98.182.163, 203.98.182.163, 203.129.72.215。
```

----

需要注意的是

```
HTTP_X_FORWARDED_FOR 可以伪造
HTTP_CLIENT_IP 没见过, 况且可以伪造
HTTP_VIA 可以伪造
REMOTE_ADDR 和服务器握手的 ip, 难以伪造
```

知道了他们是什么之后, 我们就要清楚我们到底想要什么?

**在需要 ip 做一些处理的时候, 我倾向于只用 REMOTE_ADDR 获取, 因为即使是用代理, 这个也都是真实的, 对方的更换难度相对高一些.** 在只是记录的时候, 我倾向于用原来的顺序. 然而现实中很多时候都是混合的情况, 就需要权衡一下功用, 另外, 我们也可以用其他的标识来混合校验. 比如手机的 imei, idfa.

