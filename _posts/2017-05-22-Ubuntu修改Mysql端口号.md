---
layout: post
title:  "Ubuntu修改Mysql数据库连接端口号"
date:   2017-05-22 16:43:00
categories: Others  
tags: [整理]
---
# Mysql数据库修改连接端口号


在实际情况中，为了避免不避要的攻击，最好更换Mysql数据库的默认端口号3306。   

基本的更换步骤如下：   
找到mysql的配置文件，使用`locate my.cnf`命令可以查找my.cnf文件所在位置。    
使用vim进行编辑`vim /etc/mysql/my.cnf`，查找`port = 3306`所在位置。    
一共有两行:
```
[client]
port            = 3306
socket          = /var/run/mysqld/mysqld.sock
```
以及
```
[mysqld]
#
# * Basic Settings
#
user            = mysql
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
port            = 3306
basedir         = /usr
datadir         = /var/lib/mysql
tmpdir          = /tmp
lc-messages-dir = /usr/share/mysql
```
这两处的3306替换为新端口号即可。


然后重启mysql服务：`service mysqld restart`
