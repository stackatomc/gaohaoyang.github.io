---
layout: post
<<<<<<< HEAD
title:  "MySQL-Crash-Course-06-09"
=======
title:  "chapter06过滤数据&chapter07数据过滤&chapter08用通配符进行过滤&chapter09用正则表达式进行搜索"
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
>该笔记用于简记MySQL数据过滤几种方法




**Outline**

- [第6章 过滤数据](#第6章 过滤数据)
	- [<> !=  和 IS NULL 对比](# <> !=  和 IS NULL 对比)
- [第7章 数据过滤](#第7章 数据过滤)
	- [括号( ) 改变执行语义](#括号 改变执行语义)
	- [OR 和 IN ()](#OR 和 IN)
	- [NOT](#NOT)
- [第8章 用通配符进行过滤](#第8章 用通配符进行过滤)
	- [LIKE 操作符](#LIKE 操作符)
- [第9章 用正则表达式进行搜索](#第9章 用正则表达式进行搜索)
	- [正则表达式简介](#正则表达式简介)
	- [MySQL正则表达式](#MySQL正则表达式)



---

# 第6章过滤数据 & 第7章数据过滤 & 第8章用通配符进行过滤 & 第9章用正则表达式进行搜索

标签：MySQL

---

# 第6章 过滤数据

## <> !=  和 IS NULL 对比

- <> 和 != 注意一个点，当希望查询出结果不匹配的行时，使用左边符号为NULL的行会默认被排除在外不属于不匹配而是未知行不返回在结果中
```MySQL
mysql> SELECT userid,username FROM t_user where username <> 'xiaowang'; 
// 默认不返回该列为NULL的行，只返回其他不匹配的非空行
```
- IS NULL的用法(注意认真记住第二点 = NULL 是错误的, IS NULL 是正确用法)
```MySQL
mysql> SELECT userid,username FROM t_user where username IS NULL;
mysql> SELECT userid,username FROM t_user where username = NULL; //这种查询是错误的
```

> **>Points**
> Point 1. 在控制台直接启动mysqld的方法
由于设置了开机不自动启动mysql的服务，所以每次都要打开'管理'或'任务管理器',才能在控制台实用mysql,相当麻烦. 
在控制台启动方法: 以管理员身份进入控制台, 启动命令为 `> net start MySQL` 即可,MySQL注意大小写,mysql将不被系统识别出对应服务.暂停MySQL服务未`> net stop mysql`，注意没有分号.

> Point 2. MySQL中使用单引号还是双引号
```双引号里面的字段会经过编译器解释然后再当作HTML代码输出，但是单引号里面的不需要解释，直接输出。例如：
$abc='I love u';
echo $abc //结果是:I love u
echo '$abc' //结果是:$abc
echo "$abc" //结果是:I love u
所以在对数据库里面的SQL语句赋值的时候也要用在双引号里面SQL="select a,b,c from ..."
但是SQL语句中会有单引号把字段名引出来
例如:select * from table where user='abc';
这里的SQL语句可以直接写成SQL="select * from table where user='abc'"
但是如果象下面：
$user='abc';
SQL1="select * from table where user=' ".$user." ' ";对比一下
SQL2="select * from table where user=' abc ' "
我把单引号和双引号之间多加了点空格，希望你能看的清楚一点。
也就是把'abc' 替换为 '".$user."'都是在一个单引号里面的。只是把整个SQL字符串分割了。
SQL1可以分解为以下3个部分
1："select * from table where user=' "
2：$user
3：" ' "    字符串之间用 . 来连接，这样能明白了吧。```
[yfm081616的博客](https://blog.csdn.net/yfm081616/article/details/59499150)(参考来源)

> 自己总结:
如果是通过赋值给变量，再使用**变量**作为MySQL需要先解析的内容进行检索，则需要在SQL语句中搭配使用双引号和单引号，我个人理解是主要关于在程序代码中SQL的使用；而简单在命令行/无需解析时(**直接赋值做检索**)，使用单引号为最佳，而双引号也是可以解析出内容的。

---

# 第7章 数据过滤

## 括号 改变执行语义

- 当条件过多时，OR等操作需要使用()括号，避免查询语义模糊
```MySQL
mysql> SELECT prod_name,prod_price FROM products WHERE prod_price >= 10 AND vend_id=1002 OR vend_id=1003;// SQL1中AND等级优先于OR，因此prod_price >= 10 AND vend_id=1002被结合看作一个新过滤条件. 判断为满足prod_price >= 10 AND vend_id=1002或只满足vend_id=1003均可作为返回结果.
mysql> SELECT prod_name,prod_price FROM products WHERE (prod_price >= 10 AND vend_id=1002) OR vend_id=1003;//SQL2同SQL1.
mysql> SELECT prod_name,prod_price FROM products WHERE prod_price >= 10 AND (vend_id=1002 OR vend_id=1003);//SQL3.语义改变，由于()括号优先级高，所以两个独立条件为prod_price >= 10或(vend_id=1002 OR vend_id=1003)均可返回结果
mysql> SELECT prod_name,prod_price FROM products WHERE (vend_id=1002 OR vend_id=1003) AND prod_price>=10;//SQL4同SQL3.
mysql> SELECT prod_name,prod_price FROM products WHERE prod_price >=10 AND vend_id IN (1002,1003);//SQL5同SQL3.
```


对比上面五句SQL语句，第一句执行结果与后面不同.虽然有时候觉得()使用的有些突兀，但是结果是正确的.

---

## OR 和 IN

- OR 和 IN() 类似,如 上标题中SQL3和SQL5.
- 特别注意IN()不是BETWEEN...AND...，而是OR
- 另外，IN(a,b,c,d,e,f,g...) 条件可并排写入，均为OR

---

## NOT 

- MySQL支持使用NOT对IN、BETWEEN和EXISTS自句取反，这与多数其他DBMS允许使用NOT对各种条件取反有很大差别
- 并且 NOT IN () 效果同<> ... 均不会返回条件为NULL的行记录


---

# 第8章 用通配符进行过滤

## LIKE 操作符

> ** > Q & k **
> Q1. 若LIKE不搭配使用%等通配符，是否与=等号同义
> Key1. 准确来说，不完全一样，看情况. 如DBMS系统对=操作上大小写的区分，或不同字符集等差异。但mysql上普通操作的结果使用是基本一致的.[SegmentFault问答](https://segmentfault.com/q/1010000000353239)(参考来源);

---

## % 通配符 和 _ 下划线通配符

- % 可匹配任意个字符，具体指可匹配0，1或多个字符
- _ 下划线只匹配指定位置的一个字符.
- 使用通配符的技巧: 除非绝对有必要，否则尽量不把其用在搜索模式的开始.把通配符至于搜索模式的开始处，搜索起来时最慢的。本来想对比一下使用%和正则表达式$的性能差异，如 '%jet5'和'jet5$',但是暂时找不到答案.

```MySQL
//LIKE与通配符 语法
mysql> SELECT age from t_user where username LIKE '_ jet 15';//注意空格，_在该命令中指空格前任意一个字符，若空格前有两个则无果.此外jet在window默认情况下不区分大小写将输出所有结果
//表中
a jet 15//将被查询返回
 jet 15//无法返回，因为_匹配含义为该位置上必**占有一个字符位置**，可该位置为空，空也占有一个字符位置
  jet 15//将被查询返回，理由如上
```

---

# 第9章 用正则表达式进行搜索

## 正则表达式简介

- 使用场景: 从一个文本文件中提取电话号码，查找名字中间有数字的所有文件等情况，还有爬虫技术，都经常使用到正则表达式

---

## MySQL正则表达式

- 基本字符匹配: `mysql> SELECT age from t_ user where sname REGEXP '1000';// 表示返回含有1000的所有行记录`
- . 的使用:  `mysql> SELECT age from t_ user where sname REGEXP '.000';// 表示返回含有_000的所有行记录，.与通配符_类似`
- | 的使用: `mysql> SELECT age from t_ user where sname REGEXP '1000|2000';// 与OR同理`
- 匹配几个字符之一: `mysql> SELECT age from t_user where sname REGEXP '[123]000';//'[]中括号内任选择一个,空或不占位都不算入条件返回内容`
- 否定 ^:  `mysql> SELECT age from t_user where sname REGEXP '[^123]000';//可匹配出' 000',(可理解为'.000'但.不能为1|2|3中任一，)列值整体为NULL也不返回，只能用之前的WHERE column IS NULL'`
- 匹配返回[0-3]，相当于[0123],在返回中取一个,可结合[]{}这种使用: `mysql> select * from t_student where sage REGEXP '[123]{2}';//可匹配出21、31等`
- 匹配特殊字符(如转义字符、元字符)前面写用两个反斜杠\\: \\.匹配点而非表示匹配一个字符位
	- 类似有 \\f 换页 \\n换行 \\r回车 \\t制表 \\v纵向制表 .匹配反斜杠本身 \\\
- 任意字母和数字[a-zA-Z0-9] 任意字符[a-zA-Z]其他同理
- 匹配多个实例:（匹配数目，而非占位符；占位符见上可为null但比占位，不占位时不匹配在内，通配符%相当于.*,_相当于.）
	- `*` 星号前的字符符合0或多个匹配，`+` 为1或多个匹配，`？`前面一个0或1个匹配，`{n}`指定数目匹配，`{n，}`指定不少于数目匹配，`{n，m}`指定匹配数目范围(跟解析和html中的差不多，**这里? 如'^1?D$'可匹配出1D或D.1存在则为1D，1若不存在则为D**)
- 定位符: ^ 文本开始 $文本结束 `'^1D$' /* 只能匹配出1D */`
- 只用SELECT用带文字串的REGEXP检错正则表达式. `SELECT 'hello' REGEXP '^hel' //结果无匹配返回0，有匹配返回1`

> ** > Points **
> Point 1. 在MySQL中，常用SELECT直接加上字符串或搭配函数进行检测.使用方法不需要使用到数据库，只需SELECT 字段 对字段的处理;或 SELECT 函数(数值) 直接操作,按正常方式使用直接字符串代替数据库数据进行检测查询即可。？？？举个例子把
> `SELECT 'hello' REGEXP '^hel'; //结果无匹配返回0，有匹配返回1`
> `SELECT TRIM(' 123'); //结果返回去空格后123 `