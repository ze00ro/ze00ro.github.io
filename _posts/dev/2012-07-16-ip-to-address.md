---
layout: dev
title: "ip 转 省市地址 的方式总结"
categories: article
tags: 

---


### 通过IP数据库

#### a. 纯真的数据库

这个是相对来说最丰富的, 信息很好, 就是数据不统一, 比方说有些地方是"宁夏", 而有些是"宁夏自治区"这样的,

而且很多省市县不是分离好的, 有些带"省", 有些不带. 不过如果是想显示给用户看的话, 也挺好的.

discuz 用的这个.

#### b. 叫做 tinyipdata 的数据库

如其名, 就是tiny, 速度更快, 但是一般只到市级别.

ecshop 用的这个.

#### c. 国外 maxmind 提供的 geolite

国外做的, 格式做的好, 省, 市, 县是分离的, 比较统一. 但是国内支持不够多, 查出来不是很准确. 可能跟免费版也有关系?


### 通过IP使用第三方解析服务

[百度出了个 api](http://developer.baidu.com/map/wiki/index.php?title=webapi/ip-api)

### 通过html5 的 geolocation 方法

这就必须是支持 html 5 的浏览器才可以, 还需要用户允许才可以, 直接调用 浏览器 接口即可

```javascript
function getLocation()
{
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(showPosition);
  } else {
    // todo not support
  }
}
```




