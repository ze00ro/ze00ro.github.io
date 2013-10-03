---
layout: post
title: apache配置文件的配置
description: apache配置文件的配置
category: blog
tagline: 
tags: [centos, lamp, beginner, tutorial, apache]
---
{% include JB/setup %}

2012-09-14 By {{ site.author.name }}

## about redirect

各种不同的域名地址对于搜索引擎的除重(deduplication)来说是一个负担，有没有"/"和首页的文件连接，一个域名首页就可以有6个地址：

    www.domain.com/
    domain.com/
    www.domain.com
    domain.com
    www.domain.com/index.php
    domain.com/index.php

所以搜索引擎鼓励发布者把URL标准化（归一化）。首先就是域名的归一化，`peter-lab.com` 301转向到`www.peter-lab.com`

    <VirtualHost *:80>
    ServerName peter-lab.com
    RewriteEngine on
    RewriteRule ^(.*)$ http://www.peter-lab.com$1 [R=301,L]
    </VirtualHost>

如果没有mod_rewrite也可以设置mod_alias：

    RedirectMatch 301 ^(.*)$ http://www.peter-lab.com

在域名和目录的规划方面： 豆瓣和FlickR都是非常干净的（完全隐藏了程序文件名的后缀，而统一规划了目录路径），而且在RSS方面豆瓣比FlickR做的更彻底。当然Wikipedia更彻底，目录结构非常扁平，根本没有目录。从而避免了很多潜在的连接混淆，而相互的引用，也因此非常集中而少歧义。

参考：[Apache和IIS的服务器301转向配置](http://www.mcanerin.com/EN/articles/301-redirect-apache.asp "Apache和IIS的服务器301转向配置")