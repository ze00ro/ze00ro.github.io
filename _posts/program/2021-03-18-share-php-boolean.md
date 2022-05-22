---
layout: dev
title: "问题研讨会：迁 java 过程中接口参数问题"
categories: [article, program]
tags: 

---

公司后来招的新人都是 Java 程序员，可能简单的 php 还是能写，但是遇到一些偏门的还是很成问题。

这次就遇到了一个这样的问题，因为 java 里大多 http 请求都是 feign 处理的，拼好的直接发，这样习惯了。

然后请求一个 php GET 接口的时候也这样传了布尔值，php 那边接到直接用 if 判断的，如下：

``` php
if ($_GET['is_done']) {
    echo "success";
}
```

那么不管传来的是 true 还是 false，因为在 url 里是没有类型的。php 就成了判断字符串，这个字符串有值，都会是true。

看似简单的问题，在测试的时候也只测试了正例，刚好漏掉 `false` 字符串的情况。

附上 php 里类型比较表

![php-compare]({{ site.uploads }}php-lose-compare.png)


