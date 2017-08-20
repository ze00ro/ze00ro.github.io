---
layout: dev
title: "把图片存储和处理单独出去"
date: 2016-11-19 00:00:01
categories: article
tags: 

---

刚开始都是单台存储, 到后来 NFS 挂载, 现在基本都使用云服务. 开始的时候都是只是上传, 生成一张缩略图, 没有多平台的处理, 现在随着业务的拓展, 图片要生成多种尺寸的, 而且以后可能还会扩展, 这样在上传的时候就处理就有点不合适了.

看到了很多云平台的处理方式, 都是在获取图片的时候传递一些参数, 根据参数生成图片, 并缓存. 之前还有很多的图片没有放到云上, 但是又有这样的需求. 就想自己写个试试.

只实现了根据尺寸生成图片的功能, 用的 [Intervention](http://image.intervention.io/) 处理图片, 非常强大方便

```php
<?php
...
// size check
if (!$this->ifSizeAllowed($parsed['width'], $parsed['height'])) {
    return $response->withStatus(404);
}

// 检查是否要请求的原图存在? 如果不存在返回404, 如果存在进行处理

if ($parsed['width'] == null or $parsed['height'] == null) {
    // 比例缩放
    $image = $manager->make($parsed['source'])
        ->resize($parsed['width'], $parsed['height'], function (Constraint $constraint) {
            $constraint->aspectRatio();
            $constraint->upsize();
        });
} else {
    // 裁剪
    $image = $manager->make($parsed['source'])
        ->fit($parsed['width'], $parsed['height']);
}

$cacheDir = pathinfo($parsed['cachep'], PATHINFO_DIRNAME);
if (!is_dir($cacheDir)) {
    mkdir($cacheDir, 0777, true);
}

$image->save($parsed['cachep'], 90);

return $response
    ->withHeader("Content-type", $image->mime())
    ->withBody($image->stream());
```

----

todo 以后遇到需要的再完善

- 头部对于图片的缓存?
- 水印?
- 指定裁切类型

---- 

- [Intervention 图片处理](http://image.intervention.io/)







