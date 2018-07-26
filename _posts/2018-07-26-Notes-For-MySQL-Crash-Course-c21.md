---
layout: post
<<<<<<< HEAD
title:  "MySQL-Crash-Course-21"
=======
title:  "chapter21创建和操纵表"
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
>该笔记用于简记常见MySQL常见DDL操作




**Outline**

- [第19章 插入数据](#第19章 插入数据)
	- [主键](#主键)
		- [主键定义](#主键定义)
		- [AUTO_INCREMENT](#AUTO_INCREMENT)
	- [清空表](#清空表)
	- [指定默认值](#指定默认值)
	- [引擎类型](#引擎类型)
	- [更新表(DDL)](#更新表DDL)
	- [更改复杂的表结构](#更改复杂的表结构)
	- [删除表](#删除表)
	- [重命名表](#重命名表)



---

# 第21章 创建和操纵表

标签：MySQL

---

## 主键

### 主键定义

- 语句: 
```
CREATE TABLE t_a(
	id int primary key AUTO_INCREMENT,
);
或
CREATE TABLE t_b(
	id int,
	...,
	,PRIMARY KEY(id);
);
```
- 注意: 
	- 主键定义PRIMARY KEY=UNIQUE KEY + NOT NULL ,可无需再次声明唯一和非空,(声明也不会报错). 
	- 一个表中主键可以定义多个,通常使用AUTO_INCREMENT自动增量
	- 主键经常作为另一张表的外键使用
	

### AUTO_INCREMENT 
	
- 主键经常使用AUTO_INCREMENT,且每个表只允许一个AUTO_INCREMENT,而且它必须被索引(如通过使他称为主键)
- 注意: AUTO_INCREMENT,默认第一行从1开始依次递增. 也可通过INSERT数值人为改变,下一次将从插入数值的下一个数值继续递增.
- 使用具体参考第19章插入数据,概括为不声明字段插入可为NULL,则AUTO_INCREMENT列默认增量插入,若声明表字段插入可直接忽略该列,可无需对该列进行字段声明和赋值,AUTO_INCREMENT列默认增量插入.
- 常用命令: `SELECT last_insert_id()`

---

## 清空表

- 语句: `TRUNCATE TABLE t_a;`

---

## 指定默认值

- 语法: 
```
CREATE TABLE t_a(
	id INT PRIMARY KEY AUTO_INCREMENT,
	name VARCHAR(20),
	age INT NOT NULL DEFAULT 0
);
```
- 实用场景: 由于我工作方向是后端,主要在项目中也会应用到项目与数据库交互的部分.另外,单纯对数据库操作默认DEFAULT与NOT NULL的搭配确实看上去十分实用!避免被动性和忽略性等导致的数据库问题.

> Mind 1.从DEFAULT引发的一些思考
我这段时间一直在认真的补基础知识,我过去很大程度是在基础知识上的漏洞导致了我的无法前进.也是我过去的认识浅和开始醒悟晚.
虽说项目的框架是根本,但我最近一直告诉自己要把每一点自己决定要做的事,基础知识或者是看上去很'高大上'的框架认真学为首要,而同时把握进度.默认DEFAULT语句可以避免大部分由于NOT NULL与被动输入/忽略性出错等造成的数据库问题,是十分实用的.
我思考,像一些工具的产生部分是为技术的进步,为项目的优化,也有甚至是专门为解决使用者的粗心或者解放人类存在.比如朴素的扫地机器人,扫地机器人的出现节约了某些人的时间成分,也可以认为一定程度上针对了一部分人存在的懒.正视人类的弱点,才能更好的找到发展方向,显然一大部分工具为人而生.
另外，其二是由DEFAULT对细节的重新认识.试想,测试的时候不注意,程序员自己写的程序可能发现不了什么,数据也是批量导入的.甚至也不注意'NULL'和NULL的区别，到功能上线的时候就会造成时间人力等多方面的巨大损失.对于公司里的员工,每个人都是负责一个小部分,只有将自己任务的小部分做好,才是做到真正为公司负责.初入职场容易年轻气盛,加上现在大城市许多年轻人都是家境宽裕,父辈的积累让有些年轻人虽有远见却略自视清高,忽略自己的身份.我知道我一无所有,我只有一种对自己的负责,对自己的负责就是对我工作的负责,作为公司员工就将是对产品、对部门、对领导同事、对公司、对行业、对消费者的负责。我相信应该整体先得到发展。我知道自己的每一阶段的身份，我每日投入的是我这阶段该做的事。谈不上有什么远见，但我知道我每日投入在我现有的工作上，就是对未来自己发展的一种负责。公司的利益正是由无数个小项目带动,而公司的风险或每一点损失也将从每一个项目的问题上产生.1年后我也许会称为一名程序员,我明白我需要明确自己负责的工作.看过许多优秀同学的博文,同为应届生,我们都需要避免好高骛远.

---

## 引擎类型

- 指定引擎语句:

```
CREATE TABLE t_a(
id INT PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(20)
)ENGINE=MyISAM
```

- 查看系统当前引擎: `show ENGINES;`  Support中DEFAULT为默认引擎
- 查看指定表的引擎(引擎作为创建表的信息显示): `SHOW CREATE TABLE t_a;`
```
+-----------+-------------------------+
| t_teacher | CREATE TABLE `t_teacher` (
  `sid` int(11) NOT NULL AUTO_INCREMENT,
  `sname` varchar(20) NOT NULL,
  `sage` int(11) DEFAULT NULL,
  PRIMARY KEY (`sid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
+-----------+-------------------------+
```
- 常见的三种引擎简记:
	- InnoDB:唯一支持事务操作，不支持全文本搜索
	- MyISAM:支持全文本搜索,数据存储在磁盘上,但不支持事务操作.性能及高
	- MEMORY:功能同MyISAM,而数据存储在内存中,速度快(特别适合于临时表)
- 注意:外键不能跨引擎,即实用一个引擎的表不能引用具有实用不同引擎的表的外键

---

## 更新表DDL

- 语法:
	`ALTER TABLE t_a ADD i INT NOT NULL;` 增加列
	`ALTER TABLE t_a MODIFY c CHAR(10);` 修改列
	`ALTER TABLE t_a DROP d;`???
	`ALTER TABLE t_a RENAME TO c_b;` //修改表名
	`ALTER TALBE t_a ENGINE=MyISAM;` //修改表引擎

	`ALTER TABLE t_a ALTER i SET DEFAULT 1000;` //新增或修改列默认值，删除置为SET DEFAULT NULL即可
	`ALTER TABLE t_a ADD CONSTRAINT fk_customers_products FOREIGN KEY(cust_id) REFERENCES t_b(prod_id);` //增加外键,constraint约束
	`ALTER TATBLE t_a DROP FOREIGN KEY fk_customers_products;`	//删除外键

- 注意: 1 该书未涉及过多外键知识,此处`DESC t_a;`不会显示出外键信息,用`SHOW TABLE t_a;`可以显示
  2 使用ALTER TABLE 要注意无法撤销,建议在进行改动前做一个完整的备份(模式和数据的备份)

> Mind 1.明确注意对表的UPDATE\DELETE\ALTER\DROP等操作,均不能撤销.涉及重要内容一旦删除将导致无法想像的后果,无论有多麻烦,对待这些行为请一定谨慎!谨慎!再谨慎!将公司文件删了,怎么办?刚入职没什么事,小事嫌麻烦就做不好?务必谨慎对待!求稳为一、时间花在该花的地方,是能力局限对工作处理的一种缓冲适应对待方式!切记!做了就是做了,公司的事就是分秒的事,就是利益的事,不想为自己的忽视后悔,就要真的要工作认真对待!!!工作前我一定会好好学习相当的安全做法的.

---

## 更改复杂的表结构

- 拷贝创建表: `CREATE TABLE t_c SELECT * FROM t_a;` 或 `create table t_e (SELECT sid,sage,sname FROM t_a);` 指定列名创建新表.并且在拷贝中包在SELECT外部的()有和无效果一样,可能是默认执行顺序的问题
- 拷贝插入现有表,可指定字段 `INSERT INTO t_d (SELECT * FROM t_a);` 或 插入现有表 `INSERT INTO t_e(id,age,name) SELECT * FROM t_a;`
- 步骤: 检验包含所需函数的新表;重命名旧表(如果需要可以删除它);用旧表原来的名字重命名新表;根据需要,重新创建触发器、存储过程、索引和外键.

---

## 删除表

- 语句: DROP TABLE t_a;

---

## 重命名表

- 语句: `RENAME TABLE t_a TO t_b;` 效果同 `ALTER TABLE t_a RENAME TO t_b;`
