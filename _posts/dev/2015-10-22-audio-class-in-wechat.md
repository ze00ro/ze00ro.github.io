---
layout: dev
title: "微信音频课程的实现"
categories: [article, dev, share]
tags: 

---

![h5 截图]({{ site.localimg }}img/wk-capture.jpeg)

上图是我们微信内 <客栈大学> 的详情页面, 我们有老师在微信群内讲课, 然后希望将微信内讲的内容做成网页回放. 展现模式还是模仿群内的交互.

经过一些弯路, 后来发现了很多要学的东西.

- 聊天记录可以通过同步助手导出
- 音频需要转成 hls 格式, 这样初次加载才没延迟感
- 低版本安卓不支持 hls, 还要有不同的版本和判断
- 所有音频都存储到了七牛, 做异步转码

参考

- [音视频切片](https://developer.qiniu.com/dora/manual/1485/audio-and-video-slice)
- [音视频转码](https://developer.qiniu.com/dora/manual/1248/audio-and-video-transcoding-avthumb)






