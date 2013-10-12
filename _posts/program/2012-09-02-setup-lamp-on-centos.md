---
layout: post
title: 在centos上软件源搭建LAMP环境
tagline:
date: 2012-09-02 00:00:01
categories: program
tags: [centos, lamp, beginner, tutorial]

---

## Yum安装
升级内核 yum -y update
安装Apahce, PHP, Mysql, 以及php连接mysql库组件

```bash
yum -y install httpd php mysql mysql-server php-mysql
```

安装mysql扩展

```bash
yum -y install mysql-connector-odbc mysql-devel libdbi-dbd-mysql
yum -y install httpd php mysql mysql-server php-mysql httpd-manual mod_ssl mod_perl mod_auth_mysql php-mcrypt php-gd php-xml php-mbstring php-ldap php-pear php-xmlrpc mysql-connector-odbc mysql-devel libdbi-dbd-mysql
```

设置mysql密码

```bash
mysqladmin -u root password 'newpassword' [引号内填密码]
```

mysql的一些简单设置

```bash
mysql -u root -p 此时会要求你输入刚刚设置的密码，输入后回车即可
mysql> DROP DATABASE test; [删除test数据库]
mysql> DELETE FROM mysql.user WHERE user = ''; [删除匿名帐户]
mysql> FLUSH PRIVILEGES; [重载权限]
```