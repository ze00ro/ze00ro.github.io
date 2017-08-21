---
layout: dev
title: "一次线上事故"
categories: dev
tags: 

---

大概去年12月底, 接到了乌云的邮件, 说被爆了漏洞. 一个线上系统的库被拖了.

```
back-end DBMS: MySQL 5.0
Database: weikezhan
+----------+---------+
| Table    | Entries |
+----------+---------+
| qy_users | 158     |
+----------+---------+

sqlmap resumed the following injection point(s) from stored session:
---
Parameter: innid (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: from=timeline&innid=21898' AND 6804=6804 AND 'lpaQ'='lpaQ&isappinstalled=0

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: from=timeline&innid=21898' AND (SELECT 2256 FROM(SELECT COUNT(*),CONCAT(0x716b767a71,(SELECT (ELT(2256=2256,1))),0x7170767871,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.CHARACTER_SETS GROUP BY x)a) AND 'cdXh'='cdXh&isappinstalled=0

    Type: AND/OR time-based blind
    Title: MySQL >= 5.0.12 OR time-based blind (SELECT)
    Payload: from=timeline&innid=21898' OR (SELECT * FROM (SELECT(SLEEP(5)))yzKF) AND 'cQQF'='cQQF&isappinstalled=0
```

这真的是非常尴尬, 开始排查问题. 按乌云的描述, 应该是 sql 注入到了详情页面, 迅速排查看哪里没有做校验. 发现可能的地方还是蛮多的, 一方面 url 传入的参数有些没有校验, 一方面 mysql 用的也是很老的方式, 并没有用到 pdo prepare, 事件紧急, 我们打算所有的输入都统一进行一次校验, 因为没有过多的用户输入, 所以不会有什么误杀.

之后寻找了一个

