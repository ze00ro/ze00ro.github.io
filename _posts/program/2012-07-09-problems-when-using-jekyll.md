---
layout: post
title: "使用Jekyll时候遇到的问题"
date: 2012-07-09 00:00:00
categories: program
tags:

---

### 很多可以参考

刚开始可以跟着BeiYuu做 [使用Github Pages建独立博客][beiyuu] 写的很明了,

更加深入的可以参考 [Jekyll][jekyll] 的官方文档,

[Github-pages页面][pages] 也有详细的说明, 404页面, 绑定域名的方法都在这里可以找到.


### 我是怎么搭建环境的

我习惯用vagrant做部署, 而在windows下做开发, 所以jekyll搭建到了vagrant里的ubuntu里.

```bash
# 我初始化了一个precise64的box
vagrant init {boxname}

# 配置端口映射4000后启动虚拟机
vagrant up

# 用个ssh工具连接进去, 安装ruby gems 和 git
apt-get install ruby rubygems git

# 然后你可以更改下gems源为淘宝的源, 然后开始安装
gem install jekyll

# 就可以了
jekyll new my-awesome-site
cd my-awesome-site
jekyll serve

```
然后就可以从映射的端口在host机器里访问了, 如 http://127.0.0.1:4000/.

这样, 就可以在本机调调页面结构, 写写看看后再push到服务器了.


### 记录一些问题

a. 可以用 `jekyll serve --watch --detach` 启动服务器, watch是指监控文件变化, detach是后台运行服务.

b. 编码问题, 首先文件肯定得utf8无bom, 换行符也得是unix, 在用户目录下.bash_profile里面加上

```bash
set LC_ALL=en_US.UTF-8
set LANG=en_US.UTF-8
```

c.



  [pages]: http://pages.github.com/
  [jekyll]: http://jekyllrb.com/
  [beiyuu]: http://beiyuu.com/github-pages/
