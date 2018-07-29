---
layout: post
title:  "MySQL-Crash-Course-19-20"
categories: Notes-For-MySQL-Crash-Course
tags: MySQL
author: MayerFang
---

* content
{:toc}

>该笔记用于简记常见MySQL常见DML操作




**Outline**

- [第19章 插入数据](#第19章 插入数据)
	- [自增列的插入方法](#自增列的插入方法)
	- [插入语句](#插入语句)
- [第20章 更新和删除数据](#第20章 更新和删除数据)
	- [更新](#更新)
	- [删除](#删除)
	- [更新与删除原则](#更新与删除原则)


---

# 第19章插入数据 & 第20章更新和删除数据

标签：MySQL

---

# 第19章 插入数据

## 自增列的插入方法

- 场景: 一般默认将主键设置为自增列，如自增id。便于插入和查询，设置自增也不需要每一次先查询上一次获取id再插入。
- 自增列的插入方法: `INSERT INTO t_a VALUES(NULL,'a',23);` 或者 `INSERT INTO t_a(name,age) VALUES('b',35);`
解释: 指定字段可以忽略id，若不指定字段必须匹配插入每个字段，所以将自增列置为NULL即可。默认会自增，第一行id从1开始。 

## 插入语句

- 单列插入语句: `INSERT INTO t_a VALUES(NULL,'a',23);`为不指定字段，将自增列置为NULL
`INSERT INTO t_b(name,age) VALUES('b',35);`指定字段，可无需插入自增列,插入一列也要用VALUES而不是VALUE
- 多列插入语句: `INSERT INTO t_a VALUES(NULL,'a',23),(NULL,'b',35),(NULL,'c',26);`并在其后用逗号,分割
`INSERT INTO t_b(name,age) VALUES('b',35),('a',23),('d',25);`

注意: 明确给出列明的方式更安全，即使表的结构改变，此INSERT语句仍能正常工作，所以一般不要使用没有明确给出列的列表的INSERT语句，使用列的列表能使SQL代码继续发挥作用，即使表的结构发生了改变。
两个易忘点: 插入自增列不指定列名需对应插入NULL，指定列明可省略;单列插入也同多列插入，使用关键字VALUES，每个插入的行中所有变量放在VALUES后，多列用,逗号分隔.

---

# 第20章 更新和删除数据

## 更新

- 更新对象:
	- 更新表中特定行
	- 更新表中所有行 (这里一定注意如果不加上WHERE条件，则是对所有行进行更新)
- 语法: `UPDATE t_a SET a=1(,b=2) WHERE id=5;` 或 `UPDATE t_a SET a=1(,b=2);`
- 特殊: 只需要将某一列置为空 `UPDATE t_a SET a=NULL WHERE id=1;`

> **>Points**
> Point 1. 'NULL'、NULL和''
- 对NOT NULL的列可赋'NULL'和''，而不可以赋NULL
- 对可空的列,int类型不可赋值为'NULL'和'',但可赋值为NULL
- 而可空的列,VARCHAR类型可赋值为'NULL'、NULL和''
- 分析: 一方面是对上三的体会,'NULL'类似字面值,匹配IS NOT NULL;''也匹配IS NOT NULL;只有NULL才是NULL,就是会被where检索时/COUNT(*)记数时忽略掉的行,NULL当作0行，而''和'NULL'都当作1行记数.
- 拓展: 看到一篇关于mysql空值和NULL的博文,关于占空间和索引花费的时间上未具体了解，但对该点上注意区别字带''面值为NULL或空与''实际NULL的区别,可能会运用在后续Spring等项目与数据库交互时数据的处理上以及前端jsp的处理、表单提交与获取form交互上的注意上.[流風餘韻的专栏CSND](https://blog.csdn.net/u014743697/article/details/54136092)

---

## 删除

- 删除对象:
	- 从表中删除特定的行
	- 从表中删除所有行 (同更新,注意加上WHERE,谨慎操作)
- 语法: `delete from t_a where id=1; ` 或 `delete from t_a;`
- 注意: delete是针对行进行处理，不存在delete 列 from t_table的语句. 我理解为delete指的是物理上改变这个列的存在，delete之后该行的值可大致想象为在物理上表中消失(正在看基础语法,日后再深入学习更原理的内容).这样理解的可以的,所以正解释了对表中的每一行都共有相同的列,无法delete**将某一行的单列**从物理结构上消失,则该行与其余行列数不匹配.这种物理结构上的需求不属于DML操作,delete是DML操作.若需要修改全表的物理结构,应使用DDL操作中如alter的关键字.

---

## 更新与删除原则

- 谨慎使用不带WHERE子句的UDPATE和DELETE语句
- 保证每个表都有主键，尽可能WHERE根据主键指定
- 在对UPDATE或DELETE语句操作前，尽可能先用SELECT进行测试，保证它过滤的是正确的记录，以防编写的WHERE自居不正确
- MySQL没有撤销按钮，谨慎操作UPDATE和DELETE