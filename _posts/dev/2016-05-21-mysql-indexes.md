---
layout: dev
title: "mysql 索引的一些"
categories: article
tags: 

---

索引（index）是在存储引擎（storage engine）层面实现的，而不是 server 层面。不是所有的存储引擎都支持所有的索引类型。即使多个存储引擎支持某一索引类型，它们的实现和行为也可能有所差别。

### 从数据结构角度

1. B-Tree
最常见的索引类型，基于B-Tree数据结构。B-Tree的基本思想是，所有值（被索引的列）都是排过序的，每个叶节点到跟节点距离相等。所以B-Tree适合用来查找某一范围内的数据，而且可以直接支持数据排序（ORDER BY）。但是当索引多列时，列的顺序特别重要，需要格外注意。InnoDB和MyISAM都支持B-Tree索引。InnoDB用的是一个变种B+Tree，而MyISAM为了节省空间对索引进行了压缩，从而牺牲了性能。

2. Hash索引
基于hash表。所以这种索引只支持精确查找，不支持范围查找，不支持排序。这意味着范围查找或ORDER BY都要依赖server层的额外工作。目前只有Memory引擎支持显式的hash索引（但是它的hash是nonunique的，冲突太多时也会影响查找性能）。Memory引擎默认的索引类型即是Hash索引，虽然它也支持B-Tree索引。

3. Spatial (R-Tree)（空间）索引
// todo

4. Full-text索引
主要用来查找文本中的关键字，而不是直接与索引中的值相比较。Full-text索引跟其它索引大不相同，它更像是一个搜索引擎，而不是简单的WHERE语句的参数匹配。你可以对某列分别进行full-text索引和B-Tree索引，两者互不冲突。Full-text索引配合MATCH AGAINST操作使用，而不是一般的WHERE语句加LIKE。

### 从逻辑角度

1. 主键索引：主键索引是一种特殊的唯一索引，不允许有空值
2. 普通索引或者单列索引
3. 多列索引（复合索引）：复合索引指多个字段上创建的索引，只有在查询条件中使用了创建索引时的第一个字段，索引才会被使用。使用复合索引时遵循最左前缀集合
4. 唯一索引或者非唯一索引
5. 空间索引：空间索引是对空间数据类型的字段建立的索引，MYSQL中的空间数据类型有4种，分别是GEOMETRY、POINT、LINESTRING、POLYGON。 MYSQL使用SPATIAL关键字进行扩展，使得能够用于创建正规索引类型的语法创建空间索引。创建空间索引的列，必须将其声明为NOT NULL，空间索引只能在存储引擎为MYISAM的表中创建

---

```sql
CREATE TABLE table_name[col_name data type]
[unique|fulltext|spatial][index|key][index_name](col_name[length])[asc|desc]
```

1. unique,fulltext,spatial 为可选参数，分别表示唯一索引、全文索引和空间索引；
2. index和key为同义词，两者作用相同，用来指定创建索引
3. col_name为需要创建索引的字段列，该列必须从数据表中该定义的多个列中选择；
4. index_name指定索引的名称，为可选参数，如果不指定，MYSQL默认col_name为索引值；
5. length为可选参数，表示索引的长度，只有字符串类型的字段才能指定索引长度；
6. asc或desc指定升序或降序的索引值存储



