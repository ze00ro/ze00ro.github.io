---
layout: dev
title: PHP 获取内容, 解析内容总结
categories: [article, program]
tagline: 

---

### 获取内容

php 获取 http 的内容主要有三种方法 `curl`, `file_get_contents`, `fsockopen`. 有很多的类库比如 snoopy, guzzle, requests 都是基于他们的封装.

- file_get_contents 每次请求都会重新做DNS查询，并不对DNS信息进行缓存。curl 可以.
- file_get_contents 也可以做 post 等其他类型发送, 但是可定制参数不多, 不够强大.
- fsockopen 很强大, curl 能做的, 它都能做, 反之则不行. 但是它太偏底层, 输入输出都需要自己处理.

所以 curl 方便, 易读, 强大.

### 解析内容

有一些库可以像 js 操作 dom 一样获取 html 内容, 特别方便

- phpquery
- simplehtmldom
- domcrawler

但是他们占用的内存极高, 要及时销毁, 有时候遇到一些简单的, 或者特别复杂的, 或者 html 无法解析的, 还是倾向于直接用正则去取.

