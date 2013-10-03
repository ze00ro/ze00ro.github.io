---
layout: post
title: PHP获取内容, 解析内容总结
description: PHP获取内容, 解析内容总结
category: code
tagline: 
tags: [regex, phpquery, htmlsql, simplehtmldom]
---

##获取内容##

curl, filegetcontent, fopen的差异
snoopy类库

##解析内容##

```php
/**
 * Passing by reference
 *
 * Outputs
 *
 * 10 - before add() call
 * 10 - after add() call
 * 15 - after addref() call
 */
$a = 10;
echo $a;
add($a);
echo $a;
addref($a);
echo $a;

function addref(&$a)
{
    $a += 5;
}

function add($a)
{
    $a += 5;
}
```

1. 正则
2. phpquery
3. simplehtmldom
4. TextPatternParser

##共享内容##

ci的获取内容整合类

参考地址: [ci类库总结](http://codeigniter.org.cn/forums/thread-14634-1-1.html "参考内容")