---
layout: post
<<<<<<< HEAD
title:  "MySQL-Crash-Course-24"
=======
title:  "chapter24使用游标"
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
>该笔记用于简记常见MySQL游标使用




**Outline**

- [第24章 使用游标](#第24章 使用游标)
	- [游标使用](#游标使用)



---

# 第24章 使用游标

标签：MySQL

---

## 游标使用

- 游标使用场景: 在查询中的结果集中(都是与SQL语句相匹配的行,零行或多行),SELECT语句没有办法得到第一行、下一行或前10行.因此,需要在检索出来的行中前进或后退一行或多行,就是使用游标的原因,经常搭配存储过程使用.
- 使用游标的步骤
	- 使用游标前,先声明(定义)它.声明时并没有检索数据,只是定义要使用的SELECT语句.
	- 声明后,先打开游标.再用前面定义的SELECT语句把数据实际检索出来. 
	- 对于填有数据的游标,根据是需要取出(检索)各行
	- 结束游标使用时,必须关闭游标
- 注意:
	- MySQL中游标只用于存储过程和函数.
	- 游标可根据需求频繁打开和关闭,游标打开后,可根据需要频繁执行取操作
	- 存储过程中内部的注解方式是 -- 开头

- 实例(实例给出太少,学的非常浅,日后需加强)
```
CREATE PROCEDURE proc_a()
BEGIN
	-- Declare local variables
	DECLARE o INT; 
	
	-- Declare the cursor
	DECLARE curs_order CURSOR
	FOR
	SELECT order_name from orders;

	OPEN curs_order;
	FETCH curs_order INTO o;
	CLOSE curs_order;

END;

CREATE PROCEDURE proc_b()
BEGIN
	DECLARE o INT;
	DECLARE done BOOLEAN DEFAULT 0;
	DECLARE curs_order CURSOR
	FOR
	SELECT order_name from orders;

	-- Declare continue handler continue handler是条件发生时会被执行的代码.这句话是对SQLSTATE '02000'即未找到条件,当REPEAT重新读取所有行后无法提供更多行循环时而将donw置为1;从而便于退出这个循环体.
	DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET done=1;

	OPEN curs_order;
	--Loop through all rows	
	REPEAT
		--Get order name
		FETCH curs_order INTO o;
	
	--End of loop
	UNTIL donw END REPEAT；

	--Close the cursor
	CLOSE cur_order;
			
END;

CREATE PROCEDURE processorders()
BEGIN
	DECLARE done BOOLEAN DEFAULT 0;
	DECLARE o INT;
	DECLARE t DECIMAL(8,2);

	DECLARE ordernumbers CURSOR
	FOR
	SELECT order_num FROM orders;
	
	DECLARE CONTINUS HANDLER FOR SQLSTATE '02000' SET donw=1;
	
	CREATE TABLE IF NOT EXISTS ordertotals
		(order_num INT,total DECIMAL(8,2));

	OPEN ordernumbers;
	REPEAT
		FETCH ordernumbers INTO o;
		CALL ordertotal(o,1,t); //这里是上一节的,有些郁闷t为什么不加@t;
		INSERT INTO ordertotals(order_num,total)VALUES(o,t);
	UNTIL done END REPEAT;
	CLOSE ordernumbers;
END;
```

>Mind 1.
有时我希望自己强迫自己投入或者努力把.我以前总会觉得是做给别人看的,现在我慢慢发现可能这些投入和努力是我不知不觉中形成的也不知道好不好的习惯,其实可能只是给自己看的.没有人会在乎我其实做事有多认真,重要的是到底做出了什么,公司那么大,认真的人可能不会被看到,可能产品好的人并不是这样认真,可能认真并不一定有好的产品出啦.而,只有有好的产出(也许对我现在来说是对知识的掌握和运用)这些才是对公司利益重要的.人只有有本事,才会真正属于这里;关系、...这些都是虚无的,无实际能力也是苟且度日罢.这些其实是做给自己看的,也许有共同爱好的人会喜欢我的认真也可以做朋友,但这不是公司需要的,而对于朋友来说,也许共同兴趣是首而倘若在关系中只有这种自己装出来的刻苦又缺乏实际,也是低人一等.暂且不说这种想法.试问,如果说是表现给自己看的,是希望我可以这样对待我的工作,你还会这样热爱时间吗.也许会把.
有时会想,如果没有人看到,会不会还喜欢这样下去.我想其实我觉得我会的,我喜欢沉浸在这些技术中的时间,因为好奇,所以接触.我想真正有一些我自己能明白能拥有的东西,我喜欢这些神奇的符号,有意思的逻辑.如果我能养活自己,离开这里.可能40岁的时候,我就会只拿着我够用的一些钱,活在世界上的某个地方,可能只有一两个朋友在身边,安安静静的研究技术...然后到结束.