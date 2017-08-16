---
layout: post
title: "安装mcrypt总结"
date: 2012-09-17 00:00:01
categories: program
tags:

---


安装phpmyadmin的时候遇到的问题

### Centos6 里安装方法

centos 6官方源已经没有libmcrypt的rpm包，我们这里选择编译安装，当然你也可以导入第三方源安装(centos 5略过此步)。
地址下不了的可以自己去sourceforge上搜一下

```bash
cd /tmp
wget http://superb-dca2.dl.sourceforge.net/project/mcrypt/Libmcrypt/2.5.8/libmcrypt-2.5.8.tar.gz
wget http://superb-dca2.dl.sourceforge.net/project/mhash/mhash/0.9.9.9/mhash-0.9.9.9.tar.gz
wget http://cdnetworks-kr-1.dl.sourceforge.net/project/mcrypt/MCrypt/2.6.8/mcrypt-2.6.8.tar.gz
tar zxvf libmcrypt-2.5.8.tar.gz
tar zxvf mhash-0.9.9.9.tar.gz
tar zxvf mcrypt-2.6.8.tar.gz
//安装libmcrypt
cd /tmp/libmcrypt-2.5.8
./configure --prefix=/usr
make && make install
//安装libmcrypt
cd /tmp/mhash-0.9.9.9
./configure --prefix=/usr
make && make install
//安装mcrypt
/sbin/ldconfig //搜索出可共享的动态链接库
cd /tmp/mcrypt-2.6.8
./configure
make && make install

```

安装libmcrypt组件包时，当我进行 `./configure` 编译时出现报错：`configure: error: C++ compiler cannot create executables`

```bash
yum install gcc gcc-c++ gcc-g77

```

导致这个问题是由于gcc组件安装不完整。

如果按照上面执行操作还是不行的话，请执行：

```bash
yum clear all  //清除yum所有缓存
yum update   //更新yum
yum install gcc gcc-c++ gcc-g77

rpm -ivh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-7.noarch.rpm
yum install php-mcrypt

```

### ubuntu里的安装方法

updated@2013-07-18

ubuntu里面方便很多, 现在也一直在用ubuntu. 直接运行

```bash
apt-get install php5-mcrypt

```
