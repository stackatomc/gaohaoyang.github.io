---
layout: post
<<<<<<< HEAD
title:  "MySQL-Crash-Course-12"
=======
title:  "chapter12汇总数据"
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
>该笔记用于简记常见MySQL汇总数据




**Outline**

- [第12章 汇总数据](#第12章 汇总数据)
	- [聚集函数](#聚集函数)



---

# 第12章 汇总数据

标签：MySQL

---
 
## 聚集函数

- 常见的聚集函数: AVG(column);COUNT(*|column);MAX(column);MIN(column);SUM(column)
- AVG(column)，保留小数位的方法需结合Round():`SELECT ROUND(AVG(age),2) FROM t_user;`
- COUNT()，区分COUNT(*)和COUNT(column),COUNT(*)是对当前查出的所有记录包括含有NULL值得列，而COUNT(column)仅是对查询出的结果中column列上不为NULL的记录数。一定要注意区分不是计算总数SUM()，这是计算行数、记录数！时常与GROUP BY 搭配使用。
- 支持包含列运算，注意函数同前均因使用别名。一开始很难注意，但注意检查输出，若忘记应尽快更正，增强可读性。`SELECT SUM(price*quantity) AS total_price FROM t_order;`
- 一种特殊用法: DISTINCT搭配聚集函数，如`SELECT AVG(DISTINCT age) from t_user;`或`SELECT COUNT(DISTINCT age) from t_user` 其结果与不除重的结果是不相同的，特别是AVG()原因是发生了数据数目的权重变化。就像计算成绩绩点的问题不能把低分的都当一个来算了，显然低分也存在绩点属于整体成绩的平等部分。DISTINCT原位置默认为ALL表示不除重，可不特别声明。
