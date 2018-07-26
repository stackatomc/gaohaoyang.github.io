---
layout: post
title:  "chapter27全球化和本地化&chapter28安全管理&chapter29数据库维护&chapter30改善性能"
categories: Notes-For-MySQL-Crash-Course
tags: MySQL
author: stackc
---

>该笔记用于简记常见MySQL其他问题




**Outline**

- [第27章全球化和本地化](#第27章全球化和本地化)
	- [基本概念](#基本概念)
- [第28章安全管理](#第28章安全管理)
	- [安全管理基础入门](#安全管理基础入门)
- [第29章数据库维护](#第29章数据库维护)
	- [基本了解](#基本了解)
	- [日志文件简介](#日志文件简介)
- [第30章 改善性能](#第30章 改善性能)
	- [UNION](#UNION)
	- [一些建议](#一些建议)



---

# 第27章全球化和本地化 & 第28章安全管理 & 第29章数据库维护 & 第30章改善性能

标签：MySQL

---

# 第27章全球化和本地化

## 基本概念

-	需求: 数据库用来存储和检索数据，需要存放不同的字符集和语言.即使用不同的方式进行存储,因此MySQL需要适应不同的字符集.


- 几个概念
	- 字符集: 字母和符号的集合,如unicode,gbk,gb2312,latin1...相当于所有可读或者可显示字符的数据库，字库表决定了整个字符集能够展现表示的所有字符的范围。
	- 编码: 某个字符集成员的内部表示如utf8和utf16都是unicode的编码方式,字符编码是定义在字符集上的映射规则。（字符-------->计算机中的实际存储值）
	- 校对: 规定字符如何比较的指令,如是否支持大小写会影响排序方式
<br/>
- 相关语法
	- 查看支持的数据库字符集: `SHOW CHARACTER SET;`
	- 查看支持的校对: `SHOW COLLATIONS;` COLLATIONS排序规则
	- 查看系统默认的数据库的字符集命令 `mysql> SHOW VARIABLES LIKE 'character%';//常见utf8,binary一般放图片等,latin1易与utf8发生乱码问题`
	- 查看系统默认的校对方式`SHOW VARIABLES LIKE 'collation%'` variables 变量
<br/>
- 创建时指定相关语法:
	- 为表指定 DEFAULT CHARACTER SET x COLLATE x_x_x 
	```
CREATE TABLE t_a(
	id INT PRIMARY KEY,
	name VARCHAR(20)
)DEFAULT CHARACTER SET hebrew
COLLATE hebrew_general_cli;
	```
	- 为字段指定 CHARACER SET x COLLATE x_x_x
	```
CREATE TABLE t_a(
	id INT PRIMARY KEY,
	name VARCHAR(20) CHARACTER SET latin1 COLLATE latin1_general_ci 
)DEFAULT CHARACTER SET hebrew
COLLATE hebrew_general_ci;
	```
	- 为查询语句指定 COLLATE x_x_X;
	` SELECT * FROM t_a ORDER BY lastname COLLATE latin1_general_ci;`
<br/>
- 关于大小写
	- Windows默认对数据库、数据表、列、变量等都设置未不区分大小写
Linux则不同
	- 修改大小写方法: 数据库管理员可在数据库的目录中修改my.ini文件,在[mysqld]的下面设置lower_case_table_names = 0(无则手动添加)
	- 创建表时指定大小写: 字段后添加 BINARY 表示修改为区分大小写,如 `CREATE TABLE NAME(name VARCHAR(10) BINARY );`
	- 简单判断方法: 常见的如utf8_general_ci不区分大小写,utf8_bin区分大小写
	- 另外，虽然大小写默认不区分，但可以看到在查询时还是按照输入的大小写格式进行存储

---

# 第28章 安全管理

## 安全管理基础入门

- 使用需求和常见场景
	- 多数用户只需要对表进行读和写,但少数用户甚至需要能创建和删除表
	- 某些用户需要读表，而不需要更新表
	- 你可能想允许用户添加数据，但不允许他们删除数据
	- 某些用户(管理员)可能需要处理用户账户的权限，但多数用户不需要
	- 你可能想让用户通过存储过程访问数据，但不允许他们直接访问数据
	- 你可能想根据根据用户登陆的地点限制对某些功能的访问
	- 日常工作中决不能使用root,应该创建一系列的账号,有的用于管理,有的供用户使用,有的供开发人员使用.数据梦魇更为常见的时无意识错误的结果,如错打MySQL语句,在不合适的数据中操作或者其他一些用户错误,通过保证用户不能执行他们不应该执行的语句,访问控制有助于避免这些情况的发生.

- 创建用户
	- 语句: `CREATE USER ben INDENTIFIED BY 'password';` 	- SET PASSWORD FOR newben = Password('newpwd'); SET PASSWORD  = Password('newpwd'); 
	- `RENAME USER ben TO newben;` 回顾`RENAME TABLE t_a TO t_b;` 好像一般DDL语句都需要指明类型TABLE/VIEW等,而DML一般不用,待总结
	- `DROP USER newbean;` 
- 赋予权限
	- `SHOW GRANTS FOR newben;` 
	- `GRANT SELECT ON crashcourse.* TO newben;` 权限授予语句思路是授予操作对于什么数据库的什么表给某用户
	- `REVOKE SELECT ON crashcourse.* FROM newben;` 撤回授予思路类似REVOKE撤回取消
	- GRANT 和REVOKE 在下列层次上限制: 整个服务器GRANT ALL 所有表或特定表 ON database.* 特定的列 特定的存储过程
	- 备注:授予权限包括存储过程之类的没有提供和做太多实例

---

# 第29章 数据库维护

## 基本了解
- 数据库由于安全问题,需要经常对数据库做备份.可能涉及启动问题处理和日志文件的查看.
- 书上内容似乎有些残缺和介绍也不是很详细.主要记录几个常见命令把
	- ANALYZE TABLE t_a; 
	- CHECK TABLE t_a; 

## 日志文件简介

- 错误日志,包含启动和古娜彼问题记忆任意关键错误的细节,通常名为hostname.err,位于data目录
- 查询日志.记录所有MySQL活动.该日志文件可能会很快变得非常大,因此不应该长期使用它,通常名为hostname.log,位于data目录
- 二进制日志.记录更新过数据(或者可能更新过数据)的所有语句.通常名为hostname-bin,替代MySQL5之前的更新日志
- 缓慢查询日志.记录执行缓慢的查询,有助于确认数据库优化位置.名为hostname-slog.log

不得不说这一章我没有感觉学到什么...需要多看一些书进行学习,单是寥寥几句感觉除了日志看到几个介绍,其他都是一知半解,就不记下了,数据库维护非常实习,待学习.

---

# 第30章 改善性能

## 一些建议

- 遵循MySQL特定的硬件建议
- 关键的生产DBMS应该运行在自己的专用服务器上
- MySQL起初按照预先配置，但一段时间后你可能需要调整内存分配，缓冲区大小等
- 一般来说,存储过程执行的比一条一条的执行其中的各条MySQL语句快
- LIKE很慢,一般最好使用FULLTEXT而不是LIKE
- 其他内容我现在缺乏实践和更多的深入了解,不适合一点点查找,先记下这里把
[另外还有书中附录CD命令的总结、数据类型和该处性能优化的建议均截图附在resources中，请手动转移]()