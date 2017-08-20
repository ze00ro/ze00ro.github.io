---
layout: dev
title: PHP 下载总结
categories: [article, program]
tags: [php, download, header, sendfile]

---

很多时候都是直接

```html
<a href="http://xxx.com/x.zip">下载</a>
```

这样应该是最简单的实现, 但是有时候我们会有很多的问题, 比如:

- 鉴权后才可以下载
- 不可以获得真实路径
- 文件巨大, 断点续传
- 多点下载

### 鉴权

先需要把下载请求引到到 php 处理里, 比如 `download.php?file=x.zip`, 文件里验证本次请求, 如果通过, 则跳转到下载地址.

```php
<?php
if ($ok) header("Location: http://xxx.com/x.zip");
```

### 隐藏真实地址

在鉴权的基础上, 因为仍然是跳转, 所以可以获得文件的路径, 跳过鉴权. 那么我们就需要通过 php 直接返回下载内容的方式, 强制下载. 这里要用到 fread.

```php
<?php
$file = fopen($file, "r"); 
Header("Content-type: application/octet-stream");
Header("Accept-Ranges: bytes");
Header("Accept-Length: " . filesize($file));
Header("Content-Disposition: attachment; filename=x.zip");
// 输出文件内容
// 读取文件内容并直接输出到浏览器
echo fread($file, filesize($file));
fclose($file);
```

### 断点续传

fopen 之后会占用内存, 如果文件很大, 则不太适合完全读取, 我们可以利用 http 头的 range 和 length 进行部分读取和输出.

```php
<?php
$len = filesize($sourceFile); // 获取文件大小  

header("Cache-Control: public");  
  
// 设置输出浏览器格式  
header("Content-Type: application/octet-stream"); // todo type
header("Content-Disposition: attachment; filename=x.zip"); // todo name  
header("Accept-Ranges: bytes");  
$size = filesize($sourceFile);  
// 如果有$_SERVER['HTTP_RANGE']参数  
if (isset($_SERVER['HTTP_RANGE'])) {  
    /*Range头域 　　Range头域可以请求实体的一个或者多个子范围。  
    例如，  
    表示头500个字节：bytes=0-499  
    表示第二个500字节：bytes=500-999  
    表示最后500个字节：bytes=-500  
    表示500字节以后的范围：bytes=500- 　　  
    第一个和最后一个字节：bytes=0-0,-1 　　  
    同时指定几个范围：bytes=500-600,601-999 　　  
    但是服务器可以忽略此请求头，如果无条件GET包含Range请求头，响应会以状态码206（PartialContent）返回而不是以200 （OK）。  
    */  
    // 断点后再次连接 $_SERVER['HTTP_RANGE'] 的值 bytes=4390912-  
    list ($a, $range) = explode("=", $_SERVER['HTTP_RANGE']);  
    // if yes, download missing part  
    str_replace($range, "-", $range); //这句干什么的呢。。。。  
    $size2 = $size -1; //文件总字节数  
    $new_length = $size2 - $range; //获取下次下载的长度  
    header("HTTP/1.1 206 Partial Content");  
    header("Content-Length: $new_length"); //输入总长  
    header("Content-Range: bytes $range$size2/$size"); //Content-Range: bytes 4908618-4988927/4988928   95%的时候  
} else {  
    //第一次连接  
    $size2 = $size -1;  
    header("Content-Range: bytes 0-$size2/$size"); // Content-Range: bytes 0-4988927/4988928  
    header("Content-Length: " . $size); // 输出总长  
}  
// 打开文件  
$fp = fopen("$sourceFile", "rb");  
// 设置指针位置  
fseek($fp, $range);   
while (!feof($fp)) {  
    // 设置文件最长执行时间  
    set_time_limit(0);  
    print (fread($fp, 1024 * 8)); // 输出文件  
    flush(); // 输出缓冲  
    ob_flush(); 
}  
fclose($fp);  
exit();
```

### 参考 

- [大文件的下载](http://www.joyphper.net/article/201109/134.html)

