---
layout: post
title: Git常用操作汇总
subtitle:
tags: [Git]
---

## 初始化

`git init`：创建一个空的Git仓库或重新初始化一个已存在的Git仓库。

## 记录更新到仓库

`git add [-A] [<pathspec>]`

`git commit [--amend] [-m <message>]`

## 恢复暂存区的文件

`git restore -staged <file>`

## 查看信息

`git status`

`git diff [(commit ID [commit ID 2]|--cached [commit ID])]`

- `git diff` 默认比较暂存区和工作区的差异。

- `git diff <commit ID> [commit ID 2]`: 比较指定提交之间的差异. 如果没有指定 commit ID 2, 则比较指定提交与工作区的差异。

- `git diff --cached [commit ID]`: 比较暂存区和目标提交的差异. 如果没有指定 commit ID, 则默认和最近一次提交比较。

`git ls-files`: 查看暂存区追踪的文件。

`git reflog`: 管理reflog信息。

## 分支管理

`git branch [(new-branch|-d <old-branch>)]`

`git checkout [-b] <branch>`

`git merge <branch>`

## 与远程仓库的交互

`git remote [(add <name> <url>|rename <old> <new>|remove <name>)]`：为本地仓库添加远程仓库。

`git push [-u] [<remote> [<branch>]]`

- `git push -u <remote>`：当第一次为本地分支推送时, 需要为本地分支设置跟踪的远程仓库，此时需要运行该命令。此后进行推送时, 将不再需要指定 `-u` 参数, 只需运行 `git push` 即可。

`git pull [<remote> [<branch>]]`

`git clone <url> [path]`
