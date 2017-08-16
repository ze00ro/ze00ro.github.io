---
layout: post
title: "使用Ruby时候遇到的问题"
date: 2012-08-10 00:00:00
categories: program
tags:

---

非专业Ruby, 记录一下使用时候遇到的问题

### gems安装非常的慢, 甚至无响应

这个是装jekyll时候遇到的问题, 经查原来是G-F-W搞怪, 这里要感谢 [淘宝gems镜像][taobao],

```bash
# 查看现有列表
gem sources

# 如果现有列表是rubygems.org就删了它
gem sources --remove https://rubygems.org/

# 增加淘宝的
gem sources -a http://ruby.taobao.org/

# 在确认一下
gem sources

# 显示正确的话就可以用gem install xxx了, 速度挺快的
gem install jekyll

```


  [taobao]: http://ruby.taobao.org/
