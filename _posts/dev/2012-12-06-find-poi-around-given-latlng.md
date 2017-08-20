---
layout: dev
title: "LBS 大数据量查附近的点"
categories: article
tags: 

---

问题是: 我们有很多的油站地址, 用户开车查询 x 公里内的油站. 最初数据量很小, 用的是球面距离公式算出与当前点的距离, 然后取 x 公里内的. 

这样在初期是没有问题的, 但是随着数据量的增加, 速度越来越慢, 我们需要考虑索引的效果.

### 近似矩形

刚开始也是想到经纬度旁边画个方块, 用四个点的大小来检索. [球面矩形](http://charlee.li/location-search.html)

### mysql 空间方法

mysql 新版本也对空间几何运算提供了很多支持. [stDistance](https://dev.mysql.com/doc/refman/5.6/en/spatial-relation-functions-object-shapes.html)

### geohash

因为 mysql 版本原因, 另外 geohash 性能非常棒, 最后选择了这种方式. geohash 是将二维的经纬度转换为一个字符串, 字符串越相近表示越靠近; 字符串越长代表的区域越小(详细).

参考 [geohash](http://www.cnblogs.com/LBSer/p/3310455.html)




