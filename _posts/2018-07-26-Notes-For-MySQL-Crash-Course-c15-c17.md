---
layout: post
title:  "MySQL-Crash-Course-15-17"
categories: Notes-For-MySQL-Crash-Course
tags: MySQL
author: stackc
---

* content
{:toc}

>该笔记用于简记常见MySQL联结表




**Outline**

- [第15章联结表 & 第16章创建高级联结](#第15章联结表 & 第16章创建高级联结)
	- [内连接/等值连接（内部联结）](#内连接/等值连接（内部联结）)
	- [自然连接](#自然连接)
	- [左外连接(外部联结)](#左外连接(外部联结))
	- [右外连接](#右外连接)
	- [笛卡尔积（交叉连接）](#笛卡尔积（交叉连接）)
	- [多表连接](#多表连接)
- [第17章组合查询](#第17章组合查询)
	- [UNION](#UNION)



---

# 第15章联结表 & 第16章创建高级联结 & 第17章组合查询

标签：MySQL

---

# 第15章联结表 & 第16章创建高级联结

## 内连接/等值连接（内部联结）

- t_a INNER JOIN t_b ON t_a.a = t_b.b;
- 也可用WHERE 表达，FROM t_a ,t_b WHERE t_a.a = t_b.b;
- 效果: 只保留两张表中共同有的，且默认JOIN等于INNER JOIN
- 语法: `SELECT t_a.c,t_b.d FROM t_a INNER JOIN t_b ON t_a.a=t_b.b;`
- `SELECT t_a.c,t_b.d FROM t_a JOIN t_b ON t_a.a=t_b.b;` INNNER可省略
- 这个也可以用where来实现, `SELECT t_a.c,t_b.d FROM t_a,t_b WHERE t_a.a=t_b.b;` 

---


## 自然连接

- t_a NATURAL JOIN t_b ON t_a.a = t_b.b;
- 效果: 只保留两张表中共同有的,且相同列只出现一次
- 语法: `SELECT t_a.c,t_b.d FROM t_a INNER NATURAL JOIN t_b ON t_a.a=t_b.b;`

---

## 左外连接(外部联结)

- t_a LEFT OUTER JOIN t_b ON t_a.a = t_b.b;
- 效果: 保留左边有的。具体是右表对应无即置NULL，有则保留。
- 语法: `SELECT t_a.c,t_b.d FROM t_a LEFT JOIN t_b ON t_a.a=t_b.b; `

---

## 右外连接

- t_a RIGHT OUTER JOIN t_b ON t_a.a = t_b.b;
- 效果: 保留右边有的。具体是左表对应无即置NULL，有则保留。
- 语法: `SELECT t_a.c,t_b.d FROM t_a RIGHT OUTER JOIN t_b ON t_a.a=t_b.b; `

---

## 笛卡尔积（交叉连接）

- t_a CROSS JOIN t_b ON t_a.a = t_b.b;
- 效果: 笛卡尔积的形式
- 语法: `SELECT t_a.c,t_b.d FROM t_a CROSS JOIN t_b ON t_a.a=t_b.b; `

---

## 多表连接

- 使用from和where，按照普通方式进行。注意对各列明要指定清楚表.列明区分，并且复杂查询时应对表使用别名
- 语法: `SELECT t_a.a AS a, t_b.c AS b, t_c.c AS c FROM t_a a, t_b b, t_c c WHERE a.a=b.a AND b.b=c.c;`

---

# 第17章 组合查询

## UNION

- 直接将两个查询连接在一起，默认为UNION，去除重复的列。可强制不去重，命令中使用UNION ALL代替UNION即可。
- 实例
`SELECT vend_id,prod_id,prod_price FROM products WHERE prod_price<=5;`
`SELECT vend_id,prod_price FROM products WHERE vend_id IN(1001,1002);`
使用UNIONh后修改为`SELECT vend_id,prod_id,prod_price FROM products WHERE prod_price<=5 UNION SELECT vend_id,prod_price FROM products WHERE vend_id IN(1001,1002);`
- 效果: 可看两表的SELECT字段相同，可以UNION，合并后效果为将两者的查询结果直接组合成单个查询结果集。
- UNION规则: UNION中每个查询必须包含相同的列、表达式或聚集函数(不过各个列不需要以相同的次序列出)
- 注意: 注意使用UNION组合后并不是重新合并两表，而是按顺序依次输出。若要重新对量表中的每个字段进行排序，需要使用ORDER BY关键字写于最后(无需对前面使用括号，直接写在最后即可，顺序执行)。