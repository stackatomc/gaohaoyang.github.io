---
layout: post
title:  "Notes For Jekyll"
categories: Jekyll
tags: Jekyll
author: MayerFang
---

* content
{:toc}

>该笔记用于简记Jekyll小白入门操作




**Outline**

- [Jekyll Notes](#Jekyll Notes)
	- [关于Jekyll](#关于Jekyll)
	- [开始旅程——安装jekyll所需环境](#开始旅程——安装jekyll所需环境)
	- [基本用法——小白最简单的操作](#基本用法——小白最简单的操作)
	- [目录结构](#目录结构)
	



---

# Jekyll Notes

标签：Jekyll

---

## 关于Jekyll

- Jekyll 是一个简单的博客形态的静态站点生产机器。
- 它有一个模版目录，其中包含原始文本格式的文档，通过 Markdown （或者 Textile） 以及 Liquid 转化成一个完整的可发布的静态网站，你可以发布在任何你喜爱的服务器上。
- 完全免费

--- 

## 开始旅程——安装jekyll所需环境

- Ruby: Ruby2.2.6(x64) 安装时加入PATH
- DevKit: DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe 解压
- 解压DevKit后进入目录,执行命令
`ruby dk.rb review  # 审查（非必须）`
`ruby dk.rb install  # 安装`
`gem -v  # 查看gem是否正常安装`
- 测试reby -v 和gem -v均通过后能正确显示版本
- 安装jekyll `gem install jekyll` (没有修改源,下载了相当长时间)
- 测试jekyll版本: `jkeyll -v` (jekyll3.8.8)
- 安装bundle `gem install bundle` -> `bundle install`
- 创新一个Jekll模板: `jekyll new myblog`
- 进入/myblog后: `$jekyll serve`
- 打开浏览器,默认端口4000: `http://localhost:4000`
- 如果出错，基本报错的方案都可在网上找到解决

---

## 基本用法——小白最简单的操作

安装了 Jekyll 的 Gem 包之后，就可以在命令行中使用 Jekyll 命令了

- **小白最简单的操作：**
	- 安装Jekyll-> fork一个Jekyll模板,修改项目名为自己博客地址 -> 使用git将该远程版本库克隆到本地 -> 根据自己需求修改原Jekyll模板内容 -> 将文章按规则放入_post中 -> jekyll serve 在4000端口测试 -> 确认无误后git push 推送至远程版本库 -> 其他功能如评论等逐步增加拓展，或为原Jekyll模板继续贡献等
- Jekyll 命令有以下这些用法：
	- $ jekyll serve
		- => 一个开发服务器将会运行在 `http://localhost:4000/`
	- $ jekyll serve --detach
		- => 功能和`jekyll serve`命令相同，但是会脱离终端在后台运行。
   		- 如果你想关闭服务器，可以使用`kill -9 1234`命令，"1234" 是进程号（PID）
  		如果你找不到进程号，那么就用`ps aux | grep jekyll`命令来查看，然后关闭服务器
		注意：关闭Ctrl+c或者关闭命令行窗口即可
	- $ jekyll serve --watch
		- => 和`jekyll serve`相同，但是会查看变更并且自动再生成。
还有一些可以配置的配置选项. 很多配置选项既可以在命令行中作为标识(flags)设定，也可以在源文件根目录中的 _config.yml 文件中进行设定。Jekyll 会自动加载这些配置。
		注意：这里如果只是修改普通文件测试，则可直接显示修改效果，_config.yml的修改则不会马上关联

---

## 目录结构

- Jekyll 的核心其实是一个文本转换引擎。它的概念其实就是： 你用你最喜欢的标记语言来写文章，可以是 Markdown，也可以是 Textile,或者就是简单的 HTML, 然后** Jekyll 就会帮你套入一个或一系列的布局中**。在整个过程中**你可以设置URL路径, 你的文本在布局中的显示样式**等等。这些都可以通过**纯文本编辑**来实现，最终生成的**静态页面**就是你的成品了。
- Jekyll网站目录结构
.
├── _config.yml 保存配置文件,可简化命令行命令.
├── _drafts 未发布的文章,无title.MARKUP数据
|   ├── begin-with-the-crazy-ideas.textile 
|   └── on-simplicity-in-technology.markdown
├── _includes 可加载到布局或文件中使用. 加载_includes/file.ext
|   ├── footer.html
|   └── header.html
├── _layouts 文章外部的模板.布局可在YAML头信息中选择. content...
|   ├── default.html
|   └── post.html
├── _posts 文章.文件格式必须符合YEAR-MONTH-DAY-title.MARKUP数据和标记语言都确定自文件名
|   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile
|   └── 2009-04-26-barcamp-boston-4-roundup.textile
├── _data
|   └── members.yml
├── _site Jekyll转换后文件默认放这里,推荐将该目录放进.gitignore文件中
└── index.html 若文件包含YAML头信息,Jekyll会自动将它们进行转换. 其他格式文件在站点跟目下非上述目录也会被转换.

- 注意：
	- _layout 比较重要，在头信息中定义的layout: post则是等于制定了加载_layout中的post模板，可以很容易的切换模板和应用模板，并使用如page.author...对模板进行引用设计
	- _posts 为文章放置位置，注意命名格式符合YEAR-MONTH-DAY-title.MARKUP 如`2018-07-22-Week-Trends.md` 符合命名规范才可以被识别
	- index.html 同基本的网站搭建，为网站根目录或子目录下的的默认文件.（这次fork的模板中也涉及这个文件的修改）
	- 图片可统一存在在博客根目录创建自己的asserts文件夹下，直接定位图片即可，但推荐使用七牛云等云存储空间进行存储
	- _site.xml目录为Jekyll转换后文件默认放这里,推荐将该目录放进.gitignore文件中
	- 另外要注意_config.yml需要配置不少参数，具体参考官方文档和原Jekyll模板作者的说明，注意设置主页显示摘要的格式设计等在_post最后放入的文档中要做相应修改
（其中有些无法显示，暂时用...标注）