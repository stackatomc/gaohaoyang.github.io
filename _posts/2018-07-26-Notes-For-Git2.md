---
layout: post
title:  "Notes For Git(PART2)"
categories: Git
tags: Git
author: stackc
---

* content
{:toc}

>该笔记用于简记Git建立与使用远程仓库




**Outline**

- [Git Notes（PART 2）](#Git Notes（PART 2）)
	- [创建密钥SSH](#创建密钥SSH)
	- [先有本地库,再添加远程库的操作](#先有本地库,再添加远程库的操作)
	- [先有远程库,再克隆到本地的操作](#先有远程库,再克隆到本地的操作)
	- [远程信息](#远程信息)
	- [标签管理](#标签管理)



---

# Git Notes（PART 2）

标签：Git

---

## 创建密钥SSH

- 创建密钥
	- 原理: Git本地仓库与Github仓库之间通过ssh加密传输,github知道我的公钥id_rsa.pub,只有拥有私钥id_rsa才允许push提交上去.
	- 首先查看用户主目录是否已有.ssh目录(用户主目录HOME:C:\Users\Administrator)
	- 若无,打开Git Bash使用命令: `$ssh-keygen -t rsa -C "fangsiqichn@e163.com"`
	- 场景:GitHub允许添加多个Key,将公司和家里的Key都添加到GitHub,才能在需要的电脑上网GitHub推送.
	- 本地生成.ssh命令后,在GitHub中Settings创建SSH Keys.此处我先以日期命名区分Title,将id_rsa.pub内容粘贴进去.

---

## 先有本地库,再添加远程库的操作

- 先在GitHub中创建一个Repository仓库,这里使用'learngit'命名,在创建时可对应语言选择.gitgrouse??和生成README,均不勾选,保持默认
- **将本地项目与远程仓库关联**.进入本地仓库/learngit中:
 `$git remote add origin git@github.com:stackwayc/learngit.git`
说明: origin是对Github远程仓库默认的命名
- 将**本地项目推送到GitHub远程仓库**: `$git push -u origin master`
(只有第一次提交需要-u参数,以后只需用`$git push origin master`即可)
- 注意：需要从本地提交整个项目到远程，在建立项目时建议勿先建立README.
- 备注:Learn how to use Git, as a beginner. 

---

## 先有远程库,再克隆到本地的操作

- 先删除刚才的库,项目Settings中直接删除即可.效果:完全删除,连小绿块也一起消失.
- 一样先在GitHub上创建一个新的Repository仓库,这次勾选上初始化README.md文件.进入仓库后,README.md显示在项目首页.
- 这次我在本地新建了一个空文件夹命名为/newlearngit,进入该文件执行克隆操作
- 原理: Git除支持SSH协议还支持HTTPS协议,这次使用HTTPS,使用https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https。
- 这次测试通过HTTPS进行克隆，SSH同理.克隆命令为:
`$git clone https://github.com/stackwayc/learngit.git`
效果:/newlearngit/learngit/.git README.md 注意是将整个learngit目录克隆下来.

---

## 远程信息

- 显示更详细的远程信息: `$git remote -v`

```
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)```

---

## 标签管理

- 形式与用途: 格式如v1.2版本,可通过打标签tag对log直接定位
- 打标签常用命令: 
	- `$git checkout -b dev`
	- `$git tag v1.0 //对当前`
	- `$git tag v1.1 fe20358` //可指定
	- `$git tag` 查看当前分支内所有标签(按照字母数字排序)
	- `$git tag -a 标签名 -m "comment" 1091234`
	- `$git show v0.1` 查说明等详细信息
 	- `$git tag -d v0.1`
	- `$git push origin v0.1`
	- `$git push origin --tags`一次性推送全部尚未推送到远程的本地标签
	- `$git push origin :refs/tags/v0.9` 从远程删除。删除命令也是push
