---
layout: post
title:  "Notes For Git(PART3)"
categories: Git
tags: Git
author: stackc
---

* content
{:toc}

>该笔记用于简记Git团队合作分支和冲突问题




**Outline**

- [Git Notes（PART 3）](#Git Notes（PART 3）)
	- [分支管理](#分支管理)
	- [解决冲突](#解决冲突)
	- [分支管理策略](#分支管理策略)
	- [多人协作](#多人协作)
		- [简要说明](#简要说明)
		- [Rebase命令](#Rebase命令)
	- [搭建Git服务器](#搭建Git服务器)



---

# Git Notes（PART 3）

标签：Git

---

# 分支管理

- 需求: 对已上线的项目增加新功能,为不影响用户的正常使用,创建分支,拉取到本地进行处理.团队项目种一般有一条开发分支,和GitHub上的主分支隔离,每个成员将开发分支上的pull到本地进行开发.

![dev-process](resources/dev-process.JPG)

- 常用命令
	- `$git checkout -b dev` -b 表示创建并切换到该分支,该命令相当于 `$git branch dev`+`$git checkout dev` 创建+切换,切换后即在该分支进行修改,而且原主分支
	- `$git branch` 查看当前分支列表
	- `$git checkout master` 切换回master
	- `$git merge <name>` 合并n某分支到当前分支,默认为Fast-formed快速模式,合并速度快,在主分支上合并其他分支
	- `$git branch -d dev` 删除dev分支,再查看`$git branch` 当前只剩下master主分支了

---

# 解决冲突

- 场景:
	- 创建并切换到新分支 `$ git checkout -b feature1`
	- 进行某些修改 `vi readme.md` -> `$git add readme.md` -> `'$git commit -m 'readme.md by feature1'` 
	- 切换回master `$git checkout master` 
	- 也进行某些修改 `vi readme.md` -> `$git add readme.md` -> `'$git commit -m 'readme.md by master'` 
	- master执行合并分支命令 `$git merge feature1` 由于当前master与feature1分支上数据不一致,需要先解决冲突,查询冲突文件的冲突信息`$git status`???不用pull?
	- 冲突信息解释: <<<<<<<<< <当前分支name> ========= 分割 >>>>>>>>>>> <对方分支name>
```
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```
	- 对冲突部分进行手动进入修改后,再次经过add、commit便完成最后合并了.
	- 用带参数的git log也可以看到分支的合并情况 `$git log --graph --pretty=oneline --abbrev-commit` 参数--graph表示分支合并图

```
*   cf810e4 (HEAD -> master) conflict fixed
|\  
| * 14096d0 (feature1) AND simple
* | 5dc6824 & simple
|/  
* b17d20e branch test
* d46f35e (origin/master) remove test.txt
* b84166e add test.txt
* 519219b git tracks changes
* e43a48b understand how stage works
* 1094adb append GPL
* e475afc add distributed
* eaadf4e wrote a readme file
```

---

## 分支管理策略

	- `$git merge <name>` 默认事Fast forward模式，删除分支后，会丢失分支内容,即分支中的commit -m信息均不合并在主分支中,仅有合并前的一个commit信息会被记录.
	- `$git merge --no-ff -m "merge with no-ff" dev` 可以强制fast forward模式取消改为--no-ff


- 真实团队开发的分支策略
	- master分支稳定,仅用来发布新版本，平时不能再上面干活
	- 干活在dev分支上,dev分支是不稳定的,到1.0版本版本发布时,再把dev分支合并到master上,在master分支发布1.0版本
	- 团队成员都在dev分支上干活,每个人都有自己的分支,向dev分支上合并
![](resources/group-dev.png)
	
- 强制删除未合并的分支
	- 用于任务取消,包含机密资料的分支需要强制销毁
	- `$git branch -D feature1` -D 参数可强制删除未合并的分支,-d 参数只能删除已合并的分支

---

## 多人协作

### 简要说明

- 查看远程信息命令
`$git remote` //查看远程库的名称(包括主分支和分支若有)
`$git remote -v` //显示更详细的信息
- 说明: 只有有ssh的人才可以push上去，否则只能提交pull request
	- 本地新建的分支如果不推送到远程，对其他人就是不可见的
- 多人协作流程概括:[廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013760174128707b935b0be6fc4fc6ace66c4f15618f8d000)(参考来源)
	- 在本地创建分支 `git checkout -b dev`
	- 推送到远程 `git push origin dev`
	- 查看所有分支 `git branch -a` 会看到远程下有 origin/dev分支,说明上面命令不是将dev推送到origin主分支而是origin/dev分支
	- 删除远程分支 `git branch -r -d origin/dev`
	- 在本地创建和远程分支对应的分支与git pull对应，使用`$git checkout -b branch-name origin/branch-name`
	- 从本地推送分支，使用`$git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交试图合并再提交；
	- 没有冲突或者解决掉冲突后，再用`$git push origin <branch-name>`推送就能成功！
	- 如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`$git branch --set-upstream-to=origin/<branch-name> <branch-name>`。

### Rebase命令

- 场景: 当自己多次commit未及时提交,发现远程仓库中的项目已穿插于自己几个commit提交中push上远程库.可执行该命令将远程仓库中已有的作为基准,自己本地的几次commit将会忽略真实的commit时间顺序,默认均在该基准后进行合并.
`$git rebase`
- 建议: 在团队开发中,每次打开程序应先pull最新的版本下来,避免重复劳动,降低工作效率,减少成员间冲突.
- 技巧：删除不要commit id
	- 可以使用`$git rebase -i "last_id..."` -i 参数写入要删除的commit id前一个
	- 修改为pick为drop,:wq保存.强制push上远程. `$git push -f` 

---

## 搭建Git服务器

- 原因:
	- 安全问题和利益等
- 推荐几个工具
	- 管理公钥 Gitosis(适合几百人团队)
	- 管理权限 Gitolite(几人)