---
layout: post
title:  "Notes-For-Git(PART1)"
categories: Git
tags: Git
author: stackc
---

>该笔记用于简记Git个人操作要点




**Outline**

- [Git Notes（PART 1）](#Git Notes（PART 1）)
	- [版本控制概念入门](#版本控制概念入门)
	- [安装过程简记](#安装过程简记)
	- [配置Git仓库全局用户名和邮箱](#配置Git仓库全局用户名和邮箱)
	- [创建本地版本库](#创建本地版本库)
	- [添加文件](#添加文件)
	- [版本回退](#版本回退)
	- [工作区和暂存区清理和撤回](#工作区和暂存区清理和撤回)
	- [保存工作现场](#保存工作现场)
	- [Git代码托管平台](#Git代码托管平台)
		- [使用Github](#使用Github)
		- [码云——国内的git代码托管平台](#码云——国内的git代码托管平台)
	-[自定义配置](#自定义配置) 
		- [忽略特殊文件](#忽略特殊文件)
		- [对命令使用别名](#对命令使用别名)
		


---

# Git Notes（PART 1）

标签：Git

---

## 版本控制概念入门

- 版本控制: 自动记录每次文件的改动，还可让同事或其他人协同编辑
- 对比不同版本控制工具
	- 集中式版本控制系统：如CVS、SVN，慢，必须联网或付费
	- 分布式版本控制系统：Git等工具

---

## 安装过程简记

- 使用版本：Git Git-2.18.0 64位
- 操作系统：Windows 10
- 过程备注: 基本是全程按照Git的安装程序一步步指示完成，几点备注:
	- 过程中一并安装了Notepad++ v7.5.7文本编辑器
	- 每一步都简单做了了解，选项基本按照默认配置
	- 注意在Choosing HTTPS transport backend中选择了native Windows Secure Channel library. 该选项可以和企业环境更好的集成，便于和企业域中的认证方式一起工作
	- Configuring the terminal emulator to use with Git Bash 中选择 Use MinTTY（the default terminal of MSYS2），意思是use mintty是一种仿真样式，比cmd窗口好在可以调节大小、字体样式等，而另一选项use windows defaultconsole window则是使用windows系统自带的cmd窗口打开Git Bash命令行工具

---

## 配置Git仓库全局用户名和邮箱

- 安装结束，在任意位置点击右键可进入 Git Bash
- 配置Git本地全局仓库用户名和Email地址
`$git config --global user.name "stackc"`
`$git config --global user.email "fangsiqichn@163.com"`
- 查看已有配置: `$git config --list`
- 修改用户名(win10): `$git config --global user.name "New Name"`

---

## 创建本地版本库

- 创建本地仓库位置，目录名注意勿含中文,此处为H:/learngit
- 进入learngit，点击右键进行Git Bash命令行
- 显示当前目录: `$pwd`-> /h/learngit
- 变为Git可以管理的仓库: `$git init` -> h:/learngit/.git/，生成.git目录，若隐藏用`$ls -ah`可查看

---

## 添加文件

- 文件格式要求: 支持文本内容TXT文件,网页,程序代码等,不支持格式如图片,视频,Word文档等二进制文件,Git无法跟踪具体变化,每次提交只能显示大小变化
- Windows系统工具使用: **勿用Windows自带记事本**编辑任何文本文件,由于Microsoft开发者为保护UTF-8编码的文件实际加入了某些字符,因此我们使用自带Windows记事本编译程序会报错.推荐使用Notepad++代替,注意修改编码为UTF-8 without BOM.
	- 此处下载为Notepad++ v7.5.7版本，在'编码'下拉框中没有UTF-8 without BOM选项,只有UTF-8 和UTF-8 含BOM.设置UTF-8即可.
	- 若不放心,可进入'设置'-'首选项'-'新建'-'UTF-8(无BOM)',该处有完整显示,确认勾上即可.
- 该目录下建立3个文件,分别用Notepad++创建了1个readme.txt文本文件和一个readme.md的markdown格式文件,因为较常使用另外也用MarkdownPad2创建了readme2.md文件.(以下以readme.md为例)
- **添加到暂存区**: `$git add readme.md`
- **提交到版本库**: `$git commit -m "readme.md by Notepad++"` (-m 参数表示写版本说明)

- **删除工作区文件**也需要将改变提交到暂存区再到版本库
`$rm readme2.md` ->  `$git rm readme2.md`  ->  `$git commit -m "delete readme2.md "`
- 分别使用Notepad++,MarkdownPad2,vi对readme.md进行修改测试(任以单次为例)
- **查看工作区当前状态**: `$git status`
	- 建议习惯性使用`$git status`,以免文件丢失或覆盖 

```
// 此时对readme.md修改但未加入暂存区,readme.txt修改只加入暂存区无提交到版本库
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   readme.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.md
```

- **查看暂存区与工作区的变化**:`$git diff` , 也可以直接指定具体文件 `$git diff readme.md`
	- 说明: --- 表示源文件,+++ 表示目标文件,@@ 之间是差异的定位,- 开头的行是只出现在源文件中的行,+开头的行是只出现在目标文件中的行,空格开头的行是源文件和目标文件都出现的行.
	- **三个常见的 `git diff` 命令**：（比如出差回来忘记了之前修改过的内容,非常实用)
  `$git diff` 比较的是工作区和暂存区的差别
  `$git diff --cached` 比较的是暂存区和版本库的差别
  `$git diff HEAD` 可以查看工作区和版本库的差别

```
Administrator@PC-2017 MINGW64 /h/learngit (master)
$ git diff
diff --git a/readme.md b/readme.md
index 621fe90..a5fd976 100644
--- a/readme.md
+++ b/readme.md
@@ -1,3 +1,4 @@
 # Git 学习
 Notepad++ .md文件测试
-修改readme.md第一次
\ No newline at end of file
+第一次 Notepad++修改readme.md
+第二次 vi修改readme.md
```

---

## 版本回退

- 显示历史记录 `$git log`,单行简略显示 `$ git log --pretty=oneline`
	- 参数: commit 后是该版本id
- 也可使用Git GUI可视化工具查看Git对版本管理的时间线

- 概念: HEAD像一个指针，可以在版本间移动.
- 回退版本指定HEAD命令：`$git reset --hard HEAD^` 
(HEAD即当前版本，指仓库此前最新commit的版本,HEAD^即上一个commit的版本,可直接使用HEAD~100)
- 根据commit id回退: `$git reset --hard 09ea4` 
  (至少5位可以,确保足以唯一确定版本;该命令适合未关闭命令行直接向上查找commit id回退到上次回退之前的版本4-3-2-1HEAD=>4-3-2HEAD-1,`git log`无法再显示1，可用该思路回退回当前1)
- 若已关闭命令行, 先查`$git reflog` 记录有之前每一次命令，寻找所需commit id

> **>Q&K**
> Q1. Git GUI 出现乱码
Key1. 在乱码处除点击右键设置Encoding为Unicode(UTF-8)即可,另外修改.gitconfig全局配置文件,增加
```git
[gui]
       encoding = utf-8
```
此时gitk也可以正常显示中文了
另外,查看.gitconfig目录命令为:  `$git config –list –show-origin`

---

## 工作区和暂存区清理和撤回

- Git版本库基本结构
 `[文件区]-- add --> [暂存区stage]-- commit -m --> [版本区HEAD]`
- 相关命令说明
- `$git diff HEAD -- readme.txt` 查看工作区和版本库里面最新版本的区别
- 丢弃当前某区域的修改命令,用于发现错误提修改
	- 若还未add进暂存区,发现错误后,直接输入该命令可**将工作区当前恢复最新版库一样**`$git checkout -- readme.txt` 
	- 若已add进暂存区而未commit提交到版本区,该命令可**将暂存区的撤回覆盖当前工作区**`$git checkout -- readme.txt` 
	- 若已add进暂存区,希望**删除当前暂存区中内容**,则使用`$git reset HEAD readme.txt`,工作区则不变

---

## 保存工作现场

- 场景: 突然需要解决一个bug需要将当前工作区的内容先隔离开,即'保存工作现场'储藏.并允许当bug解决后,重新恢复刚才保存的工作现场,继续开发.
- 默认情况下，git stash会缓存下列文件：
	- 添加到暂存区的修改（staged changes）
	- Git跟踪的但并未添加到暂存区的修改（unstaged changes）
- 但不会缓存一下文件：
	- 在工作目录中新的文件（untracked files）
	- 被忽略的文件（ignored files） [Tocy博客](https://www.cnblogs.com/tocy/p/git-stash-reference.html)(参考来源)
- 命令:
	- git stash 将当前工作区所有内容放入储藏区.
	- git status  再次查看git status工作现场为空.本地文件仍在,这是git中的一种策略
	- git stash list 查看当前储藏区列表
	- bug修改完成后,可以继续原来的开发进行stash储藏区的工作现场恢复
	- git stash apply stash@{0} 可指定序号恢复该工作区域.逐次从0增加.
	- git stash pop 弹出0最新,且不会在stash中保存.上句仍会保存在储藏区中
	- git stash drop stash@{0}  

---

## Git代码托管平台

### 使用Github

- 使用步骤
	- 访问对方项目主页
	- `fork` 将别人的工程一键复制到自己账号下
	- `clone` 克隆至本地(`download .zip`则与.git无关联)
	- 此时使用`$git push`只会对自己远程仓库中的进行操作,申请原项目的修改,需要执行`$git pull request`,等待对方同意方可合并到对方项目

---

### 码云——国内的git代码托管平台

- 可以一个目录下与多个仓库关联,注意区分不同的仓库命即可
- 具体操作
`git remote -v`	
`git remote rm origin` 
`$git remote add gitee git@ggitee...`
`$git remote add github git@github...`
`git upsh github dev`
`git push gitee dev`

---

## 自定义配置

### 忽略特殊文件

- `.gitignore`中设置忽略的特殊文件,在Github开源了针对对不同计算机语言的配置文件,所有配置文件可以直接在线浏览：[https://github.com/github/gitignore](https://github.com/github/gitignore)  可下载使用并加入自己的特殊配置
- 一般java需要配置
	- 自动生成的(缩略图)
	- 中间文件、可执行文件(.class)
	- 带有敏感信息的配置文件(口令等,需要自己配置)
- 若程序被.gitignore文件限制,则可强制-发执行添加 `$git add -f App.class `
- 或对比查看出错规则位置 `$git check-ignore -v App.class`

### 对命令使用别名

- 在.git/config中配置或命令行 `$git config --global alias.st status`
- 常见命令别名	
	- `co` -> `chekcout`
	- `ci` -> `commit`
	- `br` -> `branch`