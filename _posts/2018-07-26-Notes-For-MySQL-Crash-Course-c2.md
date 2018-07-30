---
layout: post
title:  "MySQL-Crash-Course-02"
categories: MySQL-beginner
tags: Notes-For-MySQL-Crash-Course
author: MayerFang
---

* content
{:toc}

>该笔记用于简记MySQL工具 




**Outline**

- [第2章 MySQL简介](#第2章 MySQL简介)
  - [数据库的基本概念](#数据库的基本概念)
  - [MySQL 工具](#MySQL 工具)



---

# 第2章 MySQL简介

标签：MYSQL

---

## MySQL 基础了解

- MySQL 是开源的
- MySQL 一种基于客户机-服务器的DBMS. 另一种是共享文件系统的DBMS.(如Microsoft Access和FileMaker)

---

## MySQL 工具

- mysql命令行实用工具
    - 进入mysql>后,每句命令用;结束
    - exit 或 quit回车退出mysql
    - 操作过程

```
1.打开mysql服务
1) 方法一 右键"此电脑" -> "管理" -> "服务和应用程序" -> 在"服务"中选择MySQL选择启动
2)比较简单 方法二 "任务管理器" -> 在"服务"中选择MySQL选择启动
备注: 我基本是用时才开
2.通过控制台进入MySQL
>mysql -P 3306 -h localhost -u root -p
Enter password: 在这里输入密码,上面-p表示申请需要输入密码,在下一行系统会给出提示输入
-P 端口号(默认3306) -h 主机号 记忆便于访问远程数据库
在mysql.user表中存储host与user的记录 mysql> select host,user from mysql.user;
3.使用mysql命令行实用工具
工具: MySQL 5.5 Command Line Client(实际位置:MYSQL安装目录/bin/mysql.exe)
操作: 点开后 提示Enter password:(默认root登陆) 后面同
远程: 在系统控制台cmd中输入: ...绝对位置/mysql.exe -P 端口 -h 主机名 -u 用户 -p回车 ,即可指定
```

- MySQL Administrator(MySQL管理器)
    - MySQL Administrator是一个图形交互客户机，用于简化MySQL服务器的管理
    - 不作为MySQL的核心组成部分安装，因此需要自行下载
        - Server Information：显示客户机和被连接的服务器的状态和版本信息
        - Service Control：允许停止和启动MySQL以及指定服务器的特性，简单来说，mysqld.exe是服务器服务也是"服务"中实际启动的MySQL的exe可执行文件，mysql.exe则是客户机
        - Startup Variables：一些参数的配置，如端口号、最大连接数等
        - User Administration 管理MySQL用户，权限等
        - Server Logs：统一管理查看日志信息
        - Catalogs：当前可用的数据库并允许创建数据库和表，可以修改表中数据.注意权限. Create New Schema 指创建新的数据库
- MySQL Query Browser
    - 也是一个图形交互客户机，用来编写和执行MySQL命令
    - 同样需要自行安装    
        -  简单来说,提供功能有：Execute窗口执行命令，查看数据库和表，列出MySQL语法、函数的语句帮助等(右下角窗口位置)

> **>Points**
> Point 1.一般在校都使用MySQL命令行或者Navicat for MySQL进行操作数据库。附上另一种常见网页服务器可管理MySQL数据库
- PhpMyAdmin - MySQL
- 另外，oracle企业管理em也在这里一提

> Mind 1.以上三种所提及的MySQL常见工具均已在本节中进行下载安装，并实际尝试。勿以小而不为之，不因为可能用不上而怕麻烦跳过实际操作.
