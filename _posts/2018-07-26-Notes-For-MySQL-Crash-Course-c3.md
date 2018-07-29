---
layout: post
title:  "MySQL-Crash-Course-03"
categories: Notes-For-MySQL-Crash-Course
tags: MySQL
author: MayerFang
---

* content
{:toc}

>该笔记用于简记MySQL基本使用




**Outline**

- [第3章 使用MySQL](#第3章 使用MySQL)
  - [查看数据库基本信息](#查看数据库基本信息)



---

# 第3章 使用MySQL

标签：MySQL

---

## 查看数据库基本信息

- 这章主要是如何连接数据库，切换数据库
- 查看所有表：`SHOW TABLES`
- 查看表描述信息：`DESC table` 简写自 `DESCRIBE table `或`SHOW COLUMNS FROM table`
    - 比较常用第一种简写表达，而`SHOW COLUMNS FROM table`输出同样信息，需了解
- 查看某数据库或某表的创建语句`SHOW CREATE DATABASE database` `SHOW CREATE TABLE table`
- `SHOW GRANTS;` 用来显示授予用户(所有用户或特定用户)的安全权限(待学)
- `SHOW STATUS` 用于显示广泛的服务器状态信息(待学，可用于数据库优化)

