---
layout: dev
title: "mysql 事务"
categories: [article, dev]
tags: 

---

当处理一个用户的时候, 我们可能需要处理他的相关信息, 比如发表的文章, 发布的评论. 可能要删除, 可能要归档. 无论如何, 我们都需要执行一系列的 sql:

```sql
delete from user_posts where user_id=1
delete from user_comments where user_id=1
delete from users where id=1
...
```

那如果由于一些原因, 在删除某一个表的时候出错了, 那么就出现了不一致的情况, 之后的查询可能会发生意想不到的结果. 那么事务就出现了.

### 事务

一般来说，事务是必须满足4个条件（ACID）： Atomicity（原子性）、Consistency（稳定性）、Isolation（隔离性）、Durability（可靠性）

- 事务的原子性：一组事务，要么成功；要么撤回。
- 稳定性 ：有非法数据（外键约束之类），事务撤回。
- 隔离性：事务独立运行。一个事务处理后的结果，影响了其他事务，那么其他事务会撤回。事务的100%隔离，需要牺牲速度。
- 可靠性：软、硬件崩溃后，InnoDB数据表驱动会利用日志文件重构修改。可靠性和高速度不可兼得， innodb_flush_log_at_trx_commit 选项 决定什么时候吧事务保存到日志里。

### 使用

MySQL 的 innodb 和 bdb 支持事务. 
		
- BEGIN或START TRANSACTION；显式地开启一个事务；
- COMMIT；也可以使用COMMIT WORK，不过二者是等价的。COMMIT会提交事务，并使已对数据库进行的所有修改称为永久性的；
- ROLLBACK；有可以使用ROLLBACK WORK，不过二者是等价的。回滚会结束用户的事务，并撤销正在进行的所有未提交的修改；

### php demo

```php
<?php
$dbhost = 'localhost:3306';  // mysql服务器主机地址
$dbuser = 'root';            // mysql用户名
$dbpass = '123456';          // mysql用户名密码
$conn = mysqli_connect($dbhost, $dbuser, $dbpass);
if(! $conn )
{
    die('连接失败: ' . mysqli_error($conn));
}
// 设置编码，防止中文乱码
mysqli_query($conn, "set names utf8");
mysqli_select_db($conn, 'test');
mysqli_query($conn, "SET AUTOCOMMIT=0"); // 设置为不自动提交，因为MYSQL默认立即执行
mysqli_begin_transaction($conn);            // 开始事务定义
 
if(!mysqli_query($conn, "insert into test (id) values(8)"))
{
    mysqli_query($conn, "ROLLBACK");     // 判断当执行失败时回滚
}
 
if(!mysqli_query($conn, "insert into test (id) values(9)"))
{
    mysqli_query($conn, "ROLLBACK");      // 判断执行失败时回滚
}
mysqli_commit($conn);            // 执行事务
mysqli_close($conn);
```


