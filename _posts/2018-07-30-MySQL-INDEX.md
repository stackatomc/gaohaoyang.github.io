---
layout: post
title:  "MySQL-INDEX"
categories: MySQL-beginner
tags: MySQL
author: MayerFang
---

* content
{:toc}

>该笔记用于简记MySQL索引使用 




**Outline**

- [补充内容 MySQL索引](#补充内容 MySQL索引)
  - [索引概念](#索引概念)
  - [索引的基本操作](#索引的基本操作)
  - [特殊索引](#特殊索引)
  - [查看索引](#查看索引)



---

# 补充内容 MySQL索引

标签：MySQL

---

## 索引概念

- 分单列索引和组合索引
	- 单列索引，即一个索引只包含单个列，一个表可以有多个单列索引
	- 组合索引，即一个索包含多个列
- 索引建立思路
	- 创建索引时，你需要确保该索引是应用在 SQL 查询语句的条件(一般作为 WHERE 子句的条件)；索引建立后，将索引列作为WHERE 条件进行查询，会提高查询效率。
[MYSQL索引问题：索引在查询中如何使用？看了很多资料都只说索引的建立。是否建立了就不用再理会](https://zhidao.baidu.com/question/194888996.html?qbl=relate_question_1&word=mysql%20%CB%F7%D2%FD%C8%E7%BA%CE%CA%B9%D3%C3)
- 索引缺点
	- 上面都在说使用索引的好处，但过多的使用索引将会造成滥用。因此索引也会有它的缺点：虽然索引大大提高了查询速度，同时却会降低更新表的速度，如对表进行INSERT、UPDATE和DELETE。因为更新表时，MySQL不仅要保存数据，还要保存一下索引文件。
	- 建立索引会占用磁盘空间的索引文件。

## 索引的基本操作

- 创建普通索引

```
CREATE INDEX indexName ON mytable(username(length));//如果是CHAR，VARCHAR类型，length可以小于字段实际长度；如果是BLOB和TEXT类型，必须指定 length
```
- 修改表结构

```
ALTER TABLE table_name
ADD [UNIQUE|FULLTEXT|SPATIAL] INDEX index_name (index_col_name,...) [USING index_type];
```
- 创建表的时候直接指定

```
CREATE TABLE mytable(
ID INT NOT NULL,
username VARCHAR(16) NOT NULL,
INDEX[indexName](username(length))
);
```

- 删除索引：`DROP INDEX [indexName] ON mytable;`

---

## 特殊索引

- 唯一索引 UNIQUE
- 主键即为一种索引
- 普通索引
- 全文索引 FULLTEXT

---

## 查看索引

- 命令：`SHOW INDEX FROM table_name；`