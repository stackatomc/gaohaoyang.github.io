---
layout: post
title:  "MySQL-Crash-Course-04-05"
categories: Notes-For-MySQL-Crash-Course
tags: MySQL
author: stackc
---

* content
{:toc}

>该笔记用于简记MySQL基本使用




**Outline**

- [第4章 检索数据](#第4章 检索数据)
  - [SELECT 语句](#SELECT 语句)
- [第5章 排序检索数据](#第5章 排序检索数据)
  - [ORDER命令排序](#ORDER命令排序)
  - [WHERE 自居操作符](#WHERE 自居操作符)


---

# 第4章检索数据 & 第5章排序检索数据

标签：MySQL

---

## SELECT 语句
- 检索单个列
	- 若无指定ORDER 输出排序顺序，则返回的数据顺序没有特殊意义，不一定是按添加到表中的顺序
	- 每一条语句用;分号结束，也可作为好习惯记住. 若不用;分号，则该命令未输入结束.
	- SQL默认不区分大小写，一般对SQL关键字使用大写，对所有列和表名使用小写.
- 使用*通配符
	- 一般情况，建议减少使用*通配符，尽可能明确列出所需列，但检索不需要的列通常会降低检索和应用程序的性能
- 使用DISTINCT值不重复
	- 命令: `SELECT DISTINCT column FROM table;`
	- 注意: DISTINCT关键字放在列名前
- 使用LIMIT限制结果
	- 命令: SELECT column FROM table LIMIT 5；//意思是从第一条数据显示起5条输出，即0,5
	- SELECT column FROM table LIMIT 1,5；//从行1开始输出起5条，一定注意对比上面. 数据表的第一条记录在行0的位置.
	- 易与开始位置和结束为止混淆，必须注意！所以MySQL5后增加新LIMIT语法.如下
	`SELECT column FROM table LIMIT 3,54`
	`SELECT column FROM table LIMIT 3 OFFSET 4;` 均表示从行3开始取4行

---

## ORDER命令排序

- 命令格式 默认ASC升序 先符号后数字最后字母
`SELECT column FROM table ORDER BY column ASC;`
`SELECT column FROM table ORDER BY DESC;`
- 仍需要注意若无特殊配置，Windows下默认不区分大小写

---

## WHERE 子句操作符

- 记一个不熟的 `<>` 和 `!=` 都是不等于的意思


