---
layout: post
title: Solve mysql install in ubuntu without point input password
date: 2019-10-22 00:00:00 +0800
categories: note
tag: mysql
---
在Ubuntu18.04 安装msyql-server-5.7的时间没有提示输入密码的具体解决方案如下。

mysql安装时分配了默认的用户名，密码：/etc/mysql/debian.cnf
```shell
        cat /etc/mysql/debian.cnf
```
用获取到的用户名与密码登录

然后设置新的密码（'new password'）
```sql
mysql> update mysql.user set authentication_string=password('new password') where user='root' and Host ='localhost';
mysql> update mysql.user set plugin="mysql_native_password"; 
mysql> flush privileges;
mysql> quit;

```