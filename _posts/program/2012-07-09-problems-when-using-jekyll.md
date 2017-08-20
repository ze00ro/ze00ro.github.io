---
layout: post
title: "使用 Jekyll 时候遇到的问题"
date: 2012-07-09 00:00:00
categories: [article, program]
tags:

---

### 可以参考

刚开始可以跟着BeiYuu做 [使用Github Pages建独立博客][beiyuu] 写的很明了,

更加深入的可以参考 [Jekyll][jekyll] 的官方文档,

[Github-pages页面][pages] 有介绍是什么, 怎么用, 绑定域名都在这里.

### 我是怎么搭建环境的

我习惯用 vagrant 做部署, 而在 windows 下做开发, 所以 jekyll 搭建到了 vagrant 里的 ubuntu 里.

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

### gems安装非常的慢, 甚至无响应

这个是装jekyll时候遇到的问题, 经查原来是被墙了... 这里要感谢 [淘宝gems镜像][taobao],

```bash
# 查看现有列表
gem sources

# 如果现有列表是rubygems.org就删了它
gem sources --remove https://rubygems.org/

# 增加淘宝的
gem sources -a http://ruby.taobao.org/

# 在确认一下
gem sources

# 可以迅速的安装啦
gem install jekyll

```

### 记录一些问题

a. 可以用 `jekyll serve --watch --detach` 启动服务器, watch是指监控文件变化, detach是后台运行服务.

b. 编码问题, 首先文件肯定得utf8无bom, 换行符也得是unix, 在用户目录下.bash_profile里面加上

```bash
set LC_ALL=en_US.UTF-8
set LANG=en_US.UTF-8
```

  [pages]: http://pages.github.com/
  [jekyll]: http://jekyllrb.com/
  [beiyuu]: http://beiyuu.com/github-pages/
  [taobao]: http://ruby.taobao.org/


