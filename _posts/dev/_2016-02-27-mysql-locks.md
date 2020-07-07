---
layout: dev
title: "mysql 锁的一些总结"
categories: dev
tags: 

---


行级锁的优点如下：
1）、当很多连接分别进行不同的查询时减小LOCK状态。  2）、如果出现异常，可以减少数据的丢失。因为一次可以只回滚一行或者几行少量的数据。  行级锁的缺点如下：  1）、比页级锁和表级锁要占用更多的内存。  2）、进行查询时比页级锁和表级锁需要的I/O要多，所以我们经常把行级锁用在写操作而不是读操作。  3）、容易出现死锁。  对于写锁定如下：  1）、如果表没有加锁，那么对其加写锁定。  2）、否则，那么把请求放入写锁队列中。  对于读锁定如下：  1）、如果表没有加写锁，那么加一个读锁。  2）、否则，那么把请求放到读锁队列中。  
