---
layout: post
title:  "Ubuntu简易防火墙UFW的使用"
date:   2017-05-17 21:30:00
categories: Others  
tags: [整理]
---

# Ubuntu简易防火墙UFW的使用

最近遇到一个问题，正式服务器担心被攻击，要求关闭所有非使用端口的连接，并开启防火墙。    
这个问题之前一直没有尝试过，不过进行配置后发现其实并不难。   

经过查询可以发现，Ubuntu系统的防火墙主要使用的是强大的iptables，但是配置比较复杂。相对应的，还有一个相对简单的工具ufw（uncomplicated firewall），我们主要使用这个进行端口的限制

### ufw的基本使用命令

sudo ufw enable 启动（慎用）
sudo ufw disable 停止

需要注意的是，如果是使用远程ssh连接到服务器上，在开启防火墙enable命令前，一定要事先将允许22号端口的规则设置好再打开，否则开了防火墙就连不上了。   

允许22端口
```
$ sudo ufw allow 22
```
允许 80 端口
```
$ sudo ufw allow 80/tcp
```
禁用 80 端口
```
$ sudo ufw delete allow 80/tcp
```

查看状态
```
$ sudo ufw status
```



基本用法就是这样。
之后有时间再来学校强大的iptables配置。