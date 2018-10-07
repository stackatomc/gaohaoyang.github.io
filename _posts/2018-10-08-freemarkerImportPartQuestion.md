---
layout: post
title:  "FreeMarker import标签路径问题"
categories: FreeMarker
tags: FreeMarker
author: MayerFang
---

* content
{:toc}

>FreeMarker import标签路径问题





**Outline**
- [FreeMarker import标签路径问题](#FreeMarker import标签路径问题)



---

# FreeMarker import标签路径问题

标签：FreeMarker

---

- 问题描述：当需要提供路径时，容易出现无法确定路径描述的问题（绝对路径、相对路径）。freemarker中import标签可导入其他模板并为其设置别名使用
- 解决问题：freemarker中.ftl 中常使用<#import ""> 标签中路径位置是基于当前位置进行查找，环回路径，类似使用markdown导入文件路径
- 举例：
	```
	有2个文件 
	file1: templates/commom/commom.ftl 公共模板
	file2: templates/home/commandList/command.ftl
	
	需要在command.ftl中导入公共模板，输入：
	<#import "../../commom/commom.ftl" as commom>
	
	就可以使用以以下简易格式：
	（如使用宏：）
	<@commom.header>
	```