---
layout: post
title:  "MySQL-Crash-Course-25"
categories: Notes-For-MySQL-Crash-Course
tags: MySQL
author: stackc
---

* content
{:toc}

>该笔记用于简记常见MySQL触发器




**Outline**

- [第25章 使用触发器](#第25章 使用触发器)
	- [触发器的基本使用](#触发器的基本使用)



---

# 第25章 使用触发器

标签：MySQL

---

## 触发器的基本使用

- 创建触发器需要注意提供
	- 触发器关联的表
	- 触发器应该相应的活动DELETE、INSERT或UPDATE
	- 触发器应该何时执行(处理之前或处理之后)
	- 注意触发器名只需要对每个单表,而不是对整个数据库中唯一.但以后的MySQL版本可能会对其命名规则更为严格,现在最好是在数据库范围内使用唯一的触发器名
	- 另外,触发器只可以对表进行定义,视图不可以,每个表最多6个触发器 

- 实例`CREATE TRIGGER newproduct AFTER INSERT ON products FOR EACH ROW SELECT 'Product add' `	解释指触发器对products FOR EACH ROW 插入每一行数据后都执行一次`SELECT 'Product add'`
- 删除: `DROP TRIGGER newproduct;`
- 备注: 这一章实例太少,注意trigger不能反悔结果.所以上面的SELECT语句是会出错的,通常是插入某表中
- 应用: 增加学生是newold对数据表中学生总数使用触发器加一 `CREATE TRIGGER trig_a AFTER INSERT ON t_a FOR EACH ROW UPDATE t_a SET total = total+1;`
- 感觉这一章很乱.