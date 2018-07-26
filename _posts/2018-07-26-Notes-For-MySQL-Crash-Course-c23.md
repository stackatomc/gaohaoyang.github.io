---
layout: post
<<<<<<< HEAD
title:  "MySQL-Crash-Course-23"
=======
title:  "chapter23使用存储过程"
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
>该笔记用于简记常见MySQL存储过程




**Outline**

- [第23章 使用存储过程](#第23章 使用存储过程)
	- [创建和执行](#创建和执行)
	- [插入语句](#插入语句)
	- [查看存储过程状态](#查看存储过程状态)



---

# 第23章 使用存储过程

标签：MySQL

---

## 存储过程基本概念

- 存储过程主要针对: 一个包含多条语句的完整操作,是一个完整的流程,希望得到定式处理
- 使用存储过程的原因: 简单、安全、高性能
	- 通过把处理封装在容易使用的单元中，简化复杂的操作
	- 由于不要求反复建立一系列处理步骤，这保证了数据的完整性。如果所有开发人院都使用同一(试验和测试)存储过程，则所使用的代码都是相同的.这一点的延伸就是为了防止错误，需要执行的步骤越多，出错的可能性就越大。防止错误保证了数据的一致性。
	- 简化对变化的管理。如果表名、列名或业务逻辑(或别的内容)有变化，只需要更改存储过程的代码。使用它的人员甚至不需要知道这些变化。
	- 安全性.通过存储过程限制对基础数据的访问减少了数据讹误的机会(无意识的或别的原因所导致的数据讹误)
	- 提高性能。因为使用存储过程比使用单独的SQL语句要快
	- 存储一些只能在单个请求中的MySQL元素和特性，存储过程可以使用它们来编写功能更强更灵活的代码.包括使用触发器等结合
- 存储过程的缺陷: 存储过程的执行和创建权限是分离的,特定用户可能没办法创建存储过程,只能通过数据库管理员授权后才可以执行和创建均允许.(比如说偷偷自建用户进了别人数据库吗)

---

## 创建和执行

- 简要执行语句说明: 注意前后修改DEMILITER语句分隔符 -> CREATE PROCEDURE 可无参、带参数OUT或IN均可-> CALL proc_name(@outcname,invalue) -> SELECT @outcname
- 语句实例三类:

```
DELIMITER // /*这句用于修改MySQL对;分号语句分隔符的修改,修改后为//,保证存储过程内部;分号出现语句错误 */

/* 无参存储过程 */
CREATE PROCEDURE proc_a()
BEGIN
	SELECT AVG(prod_price) AS priceaverage from PRODUCTS;
END //

DILIMITER ; // 注意存储过程操作完毕后恢复分号作为语句分隔符

//调用
CALL proc_a();
/*Output:
+-----------+
| AVG(sage) |
+-----------+
|    1.5000 |
+-----------+
*/

```
```
DELIMITER //

/* 只带OUT参数 */
CREATE PROCEDURE proc_b(
OUT p1 DECIMAL(8,2) 意思是小数点左边右边保留精度共8位,右边保留2位.即左边保留8-2=6位.若超过精度返回会提示ERROR: Out of range value for column...
OUT p1 DECIMAL(8,2)
)
BEGIN
	SELECT Min(prod_price) INTO p1 FROM products;
	SELECT Max(prod_price) INTO p2 FROM products;
END //

DILIMITER ;

//同样调用
CALL proc_b(@pricelow,@pricehigh);

//查看存储过程返回数据
SELECT @pricelow,@pricehigh;

```
```
DELIMITER //

CREATE PROCEDURE proc_c(
IN number INT,
out total DECIMAL(8,2)
)
BEGIN
	SELECT SUM(price*number) FROM t_a INTO total ; 这个语句中INTO和FROM的顺序没有差别,好像一般是只有OUT先INTO再FROM,有IN就先FROM、WHERE再INTO
END;

DELIMITER ;

CALL proc_b(10,@pay);
SELECT @pay;
```
- 一个智能存储过程的建立实例

```
// 说明
1.增加了注释(前面放置-- ),对于复杂存储过程注解很重要
2.另外设置一个布尔值taxable和IF判断语句
3.COMMENT语句可在SHOW CREATE PROCEDURE proc_name中查看到,推荐使用.

DELIMITER //
-- Name:ordertotal
-- Parameters:onumber = order number
-- 			  taxable = 0 if not taxable, 1 if taxable
-- 			  ototal = order total variable

CREATE PROCEDURE ordertotal(
	IN onumber INT,
	IN taxable BOOLEAN,
	OUT ototal DECIMAL(8,2)
)COMMENT 'Obtain order total,optionally adding tax'
BEGIN
	-- Declare variable for total
	DECLARE total DECIMAL(8,2);
	-- Declare tax percentage
	DECLARE taxrate INT DEFAULT 6;
	
	-- Get the order total
	SELECT Sum(item_price*quantity)
	FROM orderitems
	WHERE order_num = onumber
	INTO total;
	
	-- Is this taxable?
	IF taxable THEN
		-- Yes, so add taxrate to the total
		SELECT total+(total/100*taxrate) INTO total;
	END IF;
	-- And finally. save to out variable
	SELECT total INTO ototal;
END;
DEMILITER ;

```

---

## 查看存储过程状态

- 检查存储过程创建: `SHOW CREATE PROCEDURE proc_a;` 
- 查看所有存储过程状态,对表的查看语句类似,包括何时、由谁创建等详细信息的存储过程列表: `SHOW PROCEDURE STATUS;` 表`SHOW TABLE STATUS;`

```MySQL
mysql> show create procedure proc_a;
+-----------+----------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+--------------------+
| Procedure | sql_mode                                                       | Create Procedure                                                                                                                                    | character_set_client | collation_connection | Database Collation |
+-----------+----------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+--------------------+
| proc_a    | STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION | CREATE DEFINER=`root`@`localhost` PROCEDURE `proc_a`(
IN number INT,
OUT total Decimal(8,2))
BEGIN
SELECT SUM(sage*number) INTO total FROM t_a;
END | utf8                 | utf8_general_ci      | utf8_general_ci    |
+-----------+----------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+--------------------+
1 row in set (0.00 sec)

mysql> SHOW PRODUCEDURE STATUS;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'PRODUCEDURE STATUS' at line 1
mysql> show procedure status;
+------------+--------+-----------+----------------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
| Db         | Name   | Type      | Definer        | Modified            | Created             | Security_type | Comment | character_set_client | collation_connection | Database Collation |
+------------+--------+-----------+----------------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
| springtest | proc_a | PROCEDURE | root@localhost | 2018-07-19 11:13:56 | 2018-07-19 11:13:56 | DEFINER       |         | utf8                 | utf8_general_ci      | utf8_general_ci    |
| springtest | proc_b | PROCEDURE | root@localhost | 2018-07-19 10:56:51 | 2018-07-19 10:56:51 | DEFINER       |         | utf8                 | utf8_general_ci      | utf8_general_ci    |
+------------+--------+-----------+----------------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
2 rows in set (0.01 sec)```

