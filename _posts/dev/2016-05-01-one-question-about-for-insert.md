---
layout: dev
title: "一道批量注册用户的面试题"
date: 2016-05-01 00:00:01
categories: article
tags: 

---

朋友面试遇到了个题, 看了大牛的解读还是比我要好很多的. 真的是一个简单的东西, 可以做的很精妙, 如果考虑到性能, 安全性, 会有很多很多的点. 而用框架多了, 很多基础的东西, 又难以发现.

转载自 [寸谋](http://www.cunmou.com/php/a-bishiti.md)

### 问题

请找出下面代码中的问题，修复并优化。

```php
<?php
// 批量注册用户，每次>100个。
// 注册新用户，要求用户名与email不能与以前的重复。
$mysqli = new Mysqli($host, $user, $pass);
for ($i=0; $i<count($_POST['user_info']); $i++) {
    $info = $_POST['user_info'][$i];

    $re_1 = $mysqli->query("SELECT * FROM `demo` WHERE `uname`=$info['uname']");
    $re_2 = $mysqli->query("SELECT * FROM `demo` WHERE `email`=$info['email']");
    
    if (!$re_1 || !$re_2) {
        $mysqli->query("INSERT INTO `demo` (`uname`, `email`) VALUES('$info['email']', '$info['uname']')");
    }
}
```

### 答案

- 基础：应该把count提到循环外。
- 基础：在字符串中拼装数组时候应该用 { 与 } 括起来。
- 基础：判断是否存在的逻辑语句错误 and
- 基础：insert语句的values部分两个字段顺序错了。
- 性能：uname与email两个语句应该拼装成一个OR语句。
- 性能：应该把所有SELECT拼装一个Sql，然后去除冲突的，再把剩余的通过批量插入的方式通过一条Sql插入。
- 性能：for应该该用foreach。
- 安全：参数没有过滤，但回答 htmlspecialchars\addslashes 而非 mysqli->real_escape_string 的减分。
- 其它：query前没有USE database之类的操作，没有SET NAMES，能回答上来的比较细心。
- 其它：没有错误处理。


