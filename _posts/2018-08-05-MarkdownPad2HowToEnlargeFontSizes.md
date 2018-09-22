---
layout: post
title:  "MarkdownPad 2如何扩大左右两边字号"
categories: Tools
tags: Tools
author: MayerFang
---

* content
{:toc}

>MarkdownPad 2如何扩大左右两边字号





**Outline**
- [MarkdownPad 2如何扩大左右两边字号](#MarkdownPad 2如何扩大左右两边字号)
	- [基本情况](#基本情况)
	- [解决步骤](#解决步骤)



---

# MarkdownPad 2如何扩大左右两边字号

标签：Tools

---

## 基本情况

- 系统：Win10 64
- MarkdownPad2 默认显示效果
![上图片!]()

---

## 解决步骤

- 左边编辑字体更改大小
	- 在`【Tools】`-`【Options】`
-`【Editor】`-`Font`：此处26为页面显示大小
![上图片!]()

- 右边浏览字体更改大小
	- 在`【Tools】`-`【Options】`
-`【Stylesheets】`-`Font`：此处26为页面显示大小
编辑在body标签下加zoom:2; (zoom元素缩放比例)
```
body {
...;
zoom:2; }
```