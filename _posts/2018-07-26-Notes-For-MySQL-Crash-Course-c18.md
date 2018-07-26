---
layout: post
<<<<<<< HEAD
title:  "MySQL-Crash-Course-18"
=======
title:  "chapter18全文本搜索"
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
>该笔记用于简记常见MySQL全文本搜索




**Outline**

- [第18章 全文本搜索](#第18章 全文本搜索)
	- [两种引擎在全文本搜索上的区别](#两种引擎在全文本搜索上的区别)
	- [基础使用与查询扩展](#基础使用与查询扩展)
	- [布尔文本查询IN BOOLEAN MODE](#布尔文本查询IN BOOLEAN MODE)
	- [其他说明](#其他说明)



---


# 第18章 全文本搜索

标签：MySQL

---

## 两种引擎在全文本搜索上的区别

- 常见的两种引擎时MyISAM和InnoDB.
- MyISAM支持全文本搜索，而不支持事务；InnoDB支持事务，而不支持全文本搜索.
- 常见的其他搜索方式: LIKE和通配符, 正则表达式, 我体会与全文本搜索差异最大的是前两种不支持提供最优匹配，而全文本搜索提供这种更智能化的结果，可以按照更好的匹配来排列它们，如根据文本出现的位置或改行出现的次数。 

---

## 基础使用与查询扩展
- 全文本搜索使用方法: 前提使用MyISAM引擎，在建表中加入`FULLTEXT('该表字段')`，即相当于对该列建立索引，可在后续直接对该列进行搜索。SELECT使用MATCH()，Against()实际执行搜索.

- 实例
	- 建表
```
CREATE TABLE productnotes(
note_id int NOT NULL AUTO_INCREMENTT,
prod_od char(10) NOT NULL,
note_date datetime,
note_text text,
PRIMARY KEY(note_id),
FULLTEXT(note_text)
)ENGINE=myISAM;```

	- 检索时:
	`SELECT note_text FROM productnotes WHERE Match(note_text) Against('rabbit');`
	- 语句解释:该处使用了全文本搜索，Match()指定了仅对note_text进行检索，Against()制定了检索的文本内容.由于两行包含词rabbit,所以这两行被返回，且包含词rabbit在第3个词的等级比作为第20个词的行高.这正体现全文本搜索具有一个重要部分就是对结果排序，具有较高等级的行先返回。


- 扩展查询
	- 语句:`SELECT note_text FROM productnotes WHERE Match(note_text) Against('rabbit' WITH QUERY EXPANSION);`
	- 效果: 除了含有rabbit的行被返回，还返回了如某不含rabbit行中含有2个在另外某含rabbit行中出现过的2个文本，也均被返回，并且还根据这2个非rabbit词在行中的前后位置排序了结果集。


- 导入新表: 对导入数据到一个新表时不应该启用FULLTEXT索引，应先导入所有数据然后再修改表，定义FULLTEXT，这样有助于更快的导入数据。（而且使索引数据的总时间小于再导入每行时分别进行索引所需的总时间）

---

## 布尔文本查询IN BOOLEAN MODE

- 特点: 是另一种支持全文本搜索的方式。可以提供细节化: 定义要匹配的词、要排斥的词、排列提示(如某些词比某些词的等级更高等)
- 实例查询1:`SELECT note_text FROM productnotes WHERE Match(note_text) Against('rabbit -repo' IN BOOLEAN MODE)`//注意`-repo`可直接在含rabbit行查询结果集中过滤掉含有repo行，只返回剩下的行
- 实例查询2:`SELECT note_text FROM productnotes WHERE Match(note_text) Against('rabbit bait' IN BOOLEAN MODE)`//注意无指定布尔操作符，则搜索匹配包含rabbit和bait中的至少一个词的行
- 实例查询3:`SELECT note_text FROM productnotes WHERE Match(note_text) Against('+rabbit +bait' IN BOOLEAN MODE)`//注意该搜索匹配包含rabbit和bait的行
- 其他: BOOLEAN MODE 还提供+ - > <(包含，且减少等级值?这个没有详细了解了) 等全文本布尔操作符进行细节化匹配，但注意的是若不进行使用则与没有指定布尔方式的结果相同

---

## 其他说明

- 非用词忽略. 含义对于全文本搜索，若某些词在该文本中出现在50%以上的行中，则将它作为一个非用词忽略，因为许多词过于频繁出现搜索无意。而50%规则不适用IN BOOLEAN MODE.
- 仍是强调，如前所述，仅在MyISAM数据库引擎中支持全文本搜索.