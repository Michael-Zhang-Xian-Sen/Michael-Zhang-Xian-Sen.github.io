---
title:  git常用命令小记 #文章页面上的显示名称，可以任意修改，不会出现在URL中

date: 2020-8-1 21:35:30 #文章生成时间，一般不改，当然也可以任意修改

categories: 工具 #分类

tags: [git, 版本管理工具]  #文章标签，可空，多标签请用格式，注意:后面有个空格

description: git常用命令
---

git的常用操作及相关问题的解决方案。

<!-- more -->

## 常用命令

遇到git命令参数的问题，可通过查看文档解决：
1. 输入`git`后回车可以显示常用的git命令。
2. 输入`git 某个命令 -h` 可以查看该命令具体的参数信息。

### 1. 分支操作
1. 将当前内容全部复制到一个新分支：`git checkout -b 新分支名称`
2. 删除分支：`git branch -d`

## 具体问题的解决方案

### 1. fork得到的仓库的代码如何更新使得其与原仓库的代码一致。
1. 配置原仓库的路径：`git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git`
2. 查看远程仓库的路径，确保正确添加上游仓库：`git remote -v`
3. 抓取原仓库的修改：`git fetch upstream`
4. 其他内容：删除某个远程仓库`git remote remove <name>`

### 2. 修改commit的注释
`git commit --amend`amend为修正的意思。


### 3. 撤销git add
`git reset 文件名`

