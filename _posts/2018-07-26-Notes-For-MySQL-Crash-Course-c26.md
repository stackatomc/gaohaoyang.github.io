---
layout: post
title:  "MySQL-Crash-Course-26"
categories: Notes-For-MySQL-Crash-Course
tags: MySQL
author: stackc
---

* content
{:toc}

>该笔记用于简记常见MySQL事务




**Outline**

- [第26章 管理事务处理](#第26章 管理事务处理)
	- [基本介绍](#基本介绍)



---

# 第26章 管理事务处理

标签：MySQL

---

## 基本介绍

- 引擎: 使用InnoDB,MyISAM不支持事务
- 目的: 保证一组操作不会中途停止,而是作为整体执行.要么整体成功执行,要么完全不执行,或回退到某个已知且安全的状态
- 可回退内容: INSERT UPDATE DELETE, 事务不用于处理SELECT(没什么意义)
- 术语
	- 事务transaction: 指一组SQL语句
	- 回退rollback: 指撤销指定SQL语句的过程
	- 提交commit: 将指为存储的SQL语句结果写入数据库表中
	- 保留点savepoint: 指事务处理中设置的临时占位符,可以对它发布回退(与回退整个事务处理不同) 
- 三个实例
```
//ROLLBACK使用
SELECT * FROM ordertotals；
START TRANSACTION;
DELETE FROM ordertotals;
SELECT * FROM ordertotals；
ROLLBACK;
SELECT *  FROM ordertatols;

//明确提交点COMMIT使用
START TRANSACTON;
DELETE FROM orderitems WHERE order_name=2000;
delete from orders where order_num=20010;
COMMIT; //当使用COMMIT或ROLLBACK语句执行后，事务会自动关闭(将来的更改会隐含提交，也就是MySQL默认的数据表执行方式)

//使用保留点
SAVEPOINT delete1;
ROLLBACK TO deletel;
```
