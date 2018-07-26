---
layout: post
<<<<<<< HEAD
title:  "MySQL-Crash-Course-10-11"
=======
title:  "chapter10创建计算字段&chapter11使用数据处理函数"
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
>该笔记用于简记常见MySQL字段、数值、日期处理函数




**Outline**

- [第10章 创建计算字段](#第10章 创建计算字段)
	- [<> !=  和 IS NULL 对比使用MySQL处理字段的优点](#使用MySQL处理字段的优点)
	- [CONCAT 拼接字段](#CONCAT 拼接字段)
- [第11章 使用数据处理函数](#第11章 使用数据处理函数)
	- [常见函数分类](#常见函数分类)
	- [文本处理函数](#文本处理函数)
	- [日期和时间处理函数](#日期和时间处理函数)
	- [数值处理函数](#数值处理函数)



---

# 第10章创建计算字段 & 第11章使用数据处理函数

标签：MySQL

---

# 第10章 创建计算字段

## 使用MySQL处理字段的优点

- 在数据库服务器上完成这些操作比在客户机中完成要快得多，因为iDBMS是设计来快速有效地完成这种处理的。个人理解为在底层操作上先进行处理，性能会优于在上层进行处理的情况；不类似，但仍提一点，Linux命令行比图形化界面优秀的原因，并且linux支持非常多基础软件，用同一个图形化界面使用不同的上层软件，效果和性能明显不如命令行通用了。

---

## CONCAT 拼接字段

- 语法: CONCAT(,,,) 用逗号进行字符和变量的连接，若插入字符用单引号包含''
	- 如 `mysql> SELECT CONCAT (sname,'(',sage,')') AS 'SNAME WITH SAGE'from t_student;` 
- 注意: 一般进行拼接字段、函数处理等字段/值 处理时，要使用别名，SELECT 后面的 内容为列名内容，可用AS指定别名。若不指定别名，将会直接显示CONCAT (sname'('sage')')在列名处


---

# 第11章 使用数据处理函数

## 常见函数分类

- 处理文本串(大小写转换、去空格等)
- 数值数据上算术操作(取绝对值等)
- 日期和时间处理
- 返回DBMS正使用的特殊信息(如返回用户登录信息，检查版本细节)的系统函数

---

## 文本处理函数

- 常见: Left() 返回字符串左边内容;Right();Upper() 大写;Lower()小写;Trim()去左右空格;LTrim()去左边空格;RTrim();Length();Soundex()返回串的SOUNDEX值;Substring(str,start,length)从start位置输出直到末尾截取str,第一个字符为start=1而不是0,若start为负值则反向寻找start起点.
- 注意: Trim()等均不对中间空格做处理

```MySQL
mysql> select substring('hello',2);
+----------------------+
| substring('hello',2) |
+----------------------+
| ello                 |
+----------------------+
mysql> select substring('hello',-5);
+-----------------------+
| substring('hello',-5) |
+-----------------------+
| hello                 |
+-----------------------+
1 row in set (0.00 sec)

mysql> select substring('hello',0);//0不可以作为start位置，第三个参数是否输入都无结果返回
+----------------------+
| substring('hello',0) |
+----------------------+
|                      |
+----------------------+
```

---

## 日期和时间处理函数

- 常见: CurDate()返回当前日期(天、周等);CurTime()返回当前时间(时、分等);Now()返回当前日期时间;
- DATE(date)返回一个日期时间的日期部分;Time(date)返回一个日期时间的时间部分;Year(date)返回一个日期的年份部分；Month(date);Day(date)返回一个日期的天数部分;DayOfWeek(date)对于一个日期返回对应的星期几(周一显示为2);Hour(date);Minute(date);Second(date);
- DATEDIFF(date1,date2)计算两个日期之差;DATE_FORMAT(date,format)返回一个格式化的日期或时间串;DATE_ADD(date,INTERVAL expr type)高度灵活的时间运算函数,INTERVAL间隔;
- 第三部分的参数用法以及其他常见时间函数，具体可参考文档
[MySQL官方文档](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_date-format)
[w3schoolMYSQL教程](http://www.w3school.com.cn/sql/func_date_format.asp)


** 实例 **
```MySQL
· 第一类不带参数查询日期时间
mysql> SELECT CurDate();
+------------+
| CurDate()  |
+------------+
| 2018-07-15 |
+------------+
mysql> SELECT CurTime();
+-----------+
| CurTime() |
+-----------+
| 11:47:08  |
+-----------+

· 第二类待参数查询日期时间
mysql> select Day(Now());//Now()为2018-07-16 09:47:08
+------------+
| Day(Now()) |
+------------+
|         16 |
+------------+

· 第三类不确定参数做日期运算
mysql> select DATEDIFF('2018-06-06','2018-06-09') AS DATEDIFF;
+----------+
| DATEDIFF |
+----------+
|       -3 |
+----------+
mysql> select DATE_FORMAT(Now(),'%b-%d'); 
+----------------------------+
| DATE_FORMAT(Now(),'%b-%d') |
+----------------------------+
| Jul-16                     |
+----------------------------+
mysql> select DATE_ADD(NOW(),INTERVAL 2 DAY) AS NEWDATE;
+---------------------+
| NEWDATE             |
+---------------------+
| 2018-07-18 09:38:10 |
+---------------------+
```

> **>Points**
> Point 1.注意日期默认格式为yyyy-mm-dd
> 注意无论是插入还是使用日期数据，默认为yyyy-mm-dd,(我自己感觉使用DATE_FORMAT()日期函数十分不便从某种程序上来说.) 注意这个细节点，在后续过滤、插入等操作中需按照默认格式匹配为最佳，因为这个排除了多义性如07/06/18可能是年/月/日或日/月/年.简单来说，就是按照这个格式去赋日期值，如DateDiff('2018-06-06','20118-07-09')不会有错，也没什么好争议格式的。
> Point 2.匹配时注意尽可能原子化匹配日期
> 举例为`2018-08-08`无法直接匹配出当日所有时间如`2018-08-08 11:13:25`,只能匹配出`2018-08-08` ，所以查询时要指定具体字段的`Date(createtime)`类似这样。这是一个小技巧点！
> 
```MySQL
mysql> insert into t_student values(null,"2018-07-16",4);
mysql> insert into t_student values(null,"2018-07-16 13:16:26",4);
mysql> select * from t_student where createtime = Now();
Empty set, 9 warnings (0.00 sec)
mysql> select * from t_student where createtime = Date(Now());
+------------+------------+------+
| sid        | createtime | sage |
+------------+------------+------+
| 0000000012 | 2018-07-16 |    4 |
+------------+------------+------+
1 row in set, 9 warnings (0.00 sec)
mysql> select sid,createtime,sage from t_student where Date(createtime) = '2018-07-16';
+------------+---------------------+------+
| sid        | createtime          | sage |
+------------+---------------------+------+
| 0000000012 | 2018-07-16          |    4 |
| 0000000013 | 2018-07-16 13:16:26 |    4 |
+------------+---------------------+------+
2 rows in set, 9 warnings (0.00 sec)
```
> Point 3.还有一种匹配当月的两种思路
```MySQL
一、只提取出对应列的月（提取列值关键部分）进行等值的 = 数值比较
mysql> select sid,sname from t_student where Month(sname) = '7';
+------------+---------------------+
| sid        | sname               |
+------------+---------------------+
| 0000000012 | 2018-07-16          |
| 0000000013 | 2018-07-17 13:16:26 |
+------------+---------------------+
2 rows in set, 9 warnings (0.00 sec)

二、对所有数据使用like等做相近通配符处理检索
mysql> select * from t_student where sname like '2018-07%';
+------------+---------------------+------+
| sid        | sname               | sage |
+------------+---------------------+------+
| 0000000012 | 2018-07-16          |    4 |
| 0000000013 | 2018-07-16 13:16:26 |    4 |
| 0000000016 | 2018-07-16 13:16:26 | NULL |
+------------+---------------------+------+
3 rows in set (0.00 sec)


三、使用BETWEEN ... AND ... 匹配出中间日期（使用范围检索日期记录）
mysql> select sid,sname from t_Student where sname between '2018-07-01' and '2018-07-31';
+------------+---------------------+
| sid        | sname               |
+------------+---------------------+
| 0000000012 | 2018-07-16          |
| 0000000013 | 2018-07-17 13:16:26 |
+------------+---------------------+
2 rows in set (0.00 sec)


```

> Mind 1.说说我的感受
> 看了一周左右的《MySQL必知必会》，我心里感到有些空洞。对这些语法像是一种十分肤浅的东西，是学生的心态就是记熟会考试会基本操作。到了应聘工作，才觉得所学甚浅，也几乎不知道有何实际用途。就是有些虚无缥缈的感受，但像数据库基础的知识是一定要牢固的。说是学得浅很容易说服自己，但是想学深用深却不知意义何在，可能是求职心切，虽爱钻研各种技术。但在此关头还是希望自己能不偏离正确方向，抓住重心。
> 且当"读书十年，不如解决问题一个"说服自己罢。

---

## 数值处理函数

- 常见数值处理函数 Abs(x)绝对值;Pi();Sqrt(x)平方根如Sqrt(4)=2,Square root;Round(x)四舍五入;floor(x)返回比x小的最大整数；ceil(x)返回比x大的最小整数;truncate(x,y)将x保留到小数点后y的位置**不四舍五入**select truncate(1.234567,3)=1.234;Mod(x,y)x除以y的余数;Rand(x)返回0~1之间的随机数;exp(x),返回e的x次方.
- Cos()余弦;Sin()正弦;
- 该节均以select 函数(值) 进行测试