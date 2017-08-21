---
layout: dev
title: "大数据量分页的优化"
categories: article
tags: 

---

数据量越来越大, 分页是越来越慢, 怎么搞呢? 一般都是这样的

```sql
SELECT * FROM table ORDER BY id LIMIT  9000000,10;
```

充分利用 id 索引的方法, 前提是知道 id 有哪些

```sql
SELECT * FROM table WHERE id in (1000000, 1000001);
```

如果不知道 id, 可以找到当前页最大的 id, 只 select id

```sql
SELECT * FROM table WHERE id >= (SELECT id FROM table LIMIT 9000000, 1) LIMIT 10;
```

但是如果 分页 用其他的条件检索和排序呢?

```sql
SELECT id FROM table WHERE cond=1 order by id limit 9000000, 1
```

这就需要建立一个符合索引, 然后再取出 id, 在 查询出 指定的 区间




