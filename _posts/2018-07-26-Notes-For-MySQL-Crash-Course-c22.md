---
layout: post
title:  "chapter22使用视图"
categories: Notes-For-MySQL-Crash-Course
tags: MySQL
author: stackc
---

>该笔记用于简记常见MySQL视图操作




**Outline**

- [第22章 使用视图](#第22章 使用视图)
	- [视图](#视图)
	- [更新视图的原则](#更新视图的原则)




---

# 第22章 使用视图

标签：MySQL

---

## 视图

- 版本支持: MySQLS5开始.
- 视图概念: 视图是虚拟的表,与包含数据的表不一样,视图只包含使用时动态检索数据的查询不包含数据.可以大致理解为将SELECT语句查询出的结果存储起来生成视图,供二次使用且在权限方面发挥了重要作用.
- 使用场景
	- 重用SQL语句
	- 简化复杂的SQL操作,在编写查询后,可以方便地重用它而不必知道它的基本查询细节
	- 使用表的组成部分而不是整个表
	- 保护数据.可以给用户授予表的特定部分的访问权限而不是整个表的访问权限
	- 更改数据格式和表示.视图可返回与底层表的表示和格式不同的数据
- 注意
	- 为了创建视图，必须具有足够的访问权限。这些权限通常由数据库管理人员授予
	- 视图可以嵌套，可以利用从其他视图中检索数据的查询来构造一个视图 
	- 视图不能索引，也不能有关联的触发器或默认值
	- 视图可以和表一起使用。例如，编写一条联结表和视图的SELECT语句
	- 创建视图建议使用先DROP `DROP VIEW IF EXISTS v_ta`再用CREATE,也可以直接用CREATE OR REPLACE VIEW(感觉这样操作安全性不高)

- 语句: 
```
> CREATE VIEW v_ta AS SELECT t_a.sname,t_c.sage FROM t_a,t_c WHERE t_a.sid=t_c.id; //应明确指明表.字段,避免模糊不清ambiguous
> CREATE VIEW v_ta2 AS SELECT a.sname,c.sage FROM t_a a,t_c c WHERE a.sid=c.sid;//可使用表的别名;
> CREATE VIEW v_ta3 AS SELECT CONCAT(a.sname,'(',c.sage,')') FROM t_a a,t_c c WHERE a.sid=c.sid; //也可以使用文本处理函数等对视图重新格式化检索出来的数据
```

---

## 更新视图的原则

- 大部分视图在授予权限后，可直接对源表数据进行操作.
- 而某些视图无法对源表更新,判断为能否正确的确定被更新的基数据.一下为几种常见易导致无法更新的操作.
	- 分组(使用GROUP BY 和HAVING)
	- 联结
	- 子查询
	- 并
	- 聚集函数(Min()、Count()、Sum()等)