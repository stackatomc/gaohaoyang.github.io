---
layout: post
<<<<<<< HEAD
title:  "MySQL-Crash-Course-13-14"
=======
title:  "chapter13分组数据&chapter14使用子查询"
>>>>>>> 1c5fe1ad4b3e9ac8775f5c4456f095f36f72b77a
categories: Notes-For-MySQL-Crash-Course
tags: MySQL
author: stackc
---

<<<<<<< HEAD
* content
{:toc}

=======
>>>>>>> 1c5fe1ad4b3e9ac8775f5c4456f095f36f72b77a

>该笔记用于简记常见MySQL分组数据关键字




**Outline**

- [第13章 分组数据](#第13章 分组数据)
	- [数据分组 GROUP BY](#数据分组 GROUP BY)
	- [HAVING 和 WHERE 区分](#HAVING 和 WHERE 区分)
	- [SELECT子句顺序](#SELECT子句顺序)
- [第14章 使用子查询](#第14章 使用子查询)
	- [两子句查询结合](#两子句查询结合)
	- [使用子句检索结果作为另一子句字段](#使用子句检索结果作为另一子句字段)



---

# 第13章分组数据 & 第14章使用子查询

标签：MySQL

---

# 第13章 分组数据

## 数据分组 GROUP BY

- 主要是GROUP BY 的使用. 用法不难，使用上我思考了一下，应该可以**理解为就某一共性进行分组，以该分组原则对具有同共性的其他数据列进行组合聚集函数等处理**
- 具体语句体现为: select COUNT(column)|SUM(column)|AVG(column)等进行函数处理，[where 过滤需要的数据 ]，而group by age|price|MOUNTH(create_time)等共性.
- 如果不适用分组聚集方法，则默认应该是输出分组后各组内的第一行数据，其他行不显示。

```
mysql> select sname,COUNT(*) AS RECORD from t_student where sname='2018-07-16 13:16:26' group by sname;
+---------------------+----------+
| sname               |  RECORD  |
+---------------------+----------+
| 2018-07-16 13:16:26 |        2 |
+---------------------+----------+
1 row in set (0.00 sec)

```

---

## HAVING 和 WHERE 区分

- HAVING 是分组中第二个关键的语句，重点在HAVING是先分组后筛选；WHERE是先筛选后分组。数据筛选的区别在于WHERE先筛选后再分组的数据可能已丢失，而HAVING能能体现出分组后的不同需求。

一个不是很恰当的例子。
`select sex,AVG(age)from t_student group by sex having AVG(age)>25;`
`select sex,AVG(age) from t_student where AVG(age)>25 group by sname;`
`mysql> select name,COUNT(age) from b group by name having age>2;`
`mysql> select name,age,COUNT(age) from b group by name having age>2;`
`mysql> select name,age,COUNT(age) from b where age>2 group by name;`

分析:
第一二句逻辑上表示了where和having作用位置不同的含义，where发生在分组前对所有age求平均，而第一个having作用与分组后对各分组的age作平均。另外，该例子不合理在where后不能使用函数，切记。
第三句是错误的，需要注意，having的内容只能参考select中提取出的字段。因此修改为第四局。
第四句和第五句是可执行的，虽然含义略为诡异。若所有数据中age存在小于2时，第五句中where会将其排出在外，则分组后COUNT(age)会发生变化；而having则仍是对分组后的数据进行筛选。含义略为诡异是因为此时age为该分组中第一个数据显示的age，没有特别具体实际作用。


- 综上，where的作用是对查询结果进行分组前，且不能使用函数。
- 而having的作用是发生再对查询结果进行分组后，可以使用函数。

---

## SELECT子句顺序

- SELECT
- FROM 
- WHERE
- GROUP BY
- HAVING
- ORDER BY
- LIMIT

---

# 第14章 使用子查询

## 两子句查询结合

- 实例
`SELECT order_num FROM orderitems WHERE prod_id='TNT2';`
`SELECT cust_id FROM orders WHERE order_num IN( 2005,2007);`
`SELECT cust_id FROM orders WHERE order_num IN (SELECT order_num FROM orderitems WHERE prod_id='TNT2');`


- 注意:如上合并查询重点在于关联的部分前后均有同列名，如上例子中的order_num

---

## 使用子句检索结果作为另一子句字段

- 实例
`SELECT cust_name,cust_state,(SELECT COUNT(*) FROM orders WHERE orders.cust_id=customers.cust_id) AS orders FROM customers ORDER BY cust_name;`

- 注意:如上也称为相关子查询，事对每一个customers.cust_id都执行一次