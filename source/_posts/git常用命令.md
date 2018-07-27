---
title: git命令（一）
date: 2018-07-19 16:51:31
tags:
- git
categories:
- 编程工具
description: 关于git的一些知识，以及常用的git命令
---

# 常用git命令

## 一、git状态
### 工作区域
![git-area](/images/git-command/git-command-1.png)
工作区：日常编辑代码的地方
本地仓库：保存本地提交记录
暂存区：相当于工作区和本地仓库中简的缓存，代表要提交代码的一个工作状态，维护的是一个虚拟的树形结构

### 文件变化周期
![git-file-live](/images/git-command/git-command-2.png)
添加一个新文件A，A处于 `Untracked` 状态，
通过add将其加至暂存区，A变成 `Staged` 状态，
通过commit提交，A变成 `Unmodified` 状态，
对 `Unmodified` 状态的文件进行修改，就变成 `Unmodified` 状态，
删除文件，将会使其变成 `Untracked` 状态


## 二、基本
### config
```
查看git配置
git config --list

编辑git配置
git config -e [--global]

设置用户信息
git config [--global] user.name "[name]"
git config [--global] user.email "[emali]"
```

### status
```
显示工作目录和暂存区的状态
git status
```

### log
```
查看提交历史
git log

--oneline 将每个提交放在一行显示
git log --oneline

--stat，仅显示简要的增改行数统计
git log --stat

--grep，搜索提交说明中的关键字
git log --grep keywords

-p 选项展开显示每次提交的内容差异
git log -p -- file/path

显示所有提交过的用户，按提交次数排序
git shortlog -sn
```

### diff
```
比较暂存区和工作区的差异
git diff

比较暂存区和上一个commit的差异
git diff --cached

比较工作区与指定commit-id的差异
git diff <commit> [path]

显示今天你写了多少行代码
git diff --shortstat "@{0 day ago}"
```

## 三、本地
### add
```
添加文件到暂存区
git add [file...]

添加指定路径的文件到暂存区
git add [path]

将所有文件从工作区添加到暂存区，包括修改的、新建的，但不包括删除的
git add .

将所有文件从工作区添加到暂存区，包括修改的、删除的，但不包括新建的
git add -u

将所有文件从工作区添加到暂存区，包括修改的、删除的、新建的
git add -A
```

### commit
```
将文件从暂存区提交至本地仓库
git commit -m "[your message]"

撤销上一次提交
git commit --amend

将工作区自从上次提交之后的变化 提交至本地仓库
git commit -a

提交时显示所有diff信息
git commit -v
```

### branch
```
显示所有本地分支
git branch

显示所有远程分支
git branch -r

显示所有分支，包括本地和远程
git branch -a

新建一个分支，以branch-name命名
git branch <branch-name>

将old branch分支重命名为new branch
git branch -m <old branch> <new branch>

将branch分支强制移动至new place提交位置
git branch -f <branch> [new place]

新建一个分支branch，并与远程分支remote-branch建立追踪关系
git branch --track <branch> <remote-branch>

将本地分支branch与远程分支remote-branch建立追踪关系
git branch --set-upstream <branch> <remote-branch>

删除远程分支
git branch -dr [remote/branch]
```

### checkout
```
切换至分支branch
git checkout <branch>

新建并切换至分支new branch
git checkout -b [new branch]

切换至上一个进行操作的分支
git checkout -
```

### cherry-pick
```
将指定提交选定，并合进当前分支
git cherry-pick <commit 1> ... <commit n>
```

### rebase
```
以线性关系合并branch
git rebase <upstream> <branch>

修改提交历史。
startpoint, endpoint 表示一个编辑区间。endpoint如果不指定则默认当前HEAD所指向的commit
git rebase -i <start point> <end point>
```


## 四、远程
### clone
```
克隆远程仓库
git clone
```

### remote
```
显示所有远程仓库
git remote -v

显示指定远程仓库
git remote show <remote>

添加一个远程仓库，并命名为 new name
git remote add <new name> <url>

修改远程仓库的地址
git remote set-url origin <url>
```

### fetch
```
下载远程仓库所有变动
git fetch

从remote仓库的source获取提交记录，放到本地的destination上，
如果没有source，将以destination创建一个新分支在本地
git fetch <remote> <source>:<destination>
```

### pull
```
下载远程仓库的变动并更新至本地
git pull

从remote仓库的source获取提交记录，放到本地的destination上，最后将destination合并到当前分支上
如果没有source，将以destination创建一个新分支在本地，再进行合并
git pull <remote> <source>:<destination>

以rebase的方式合并提交，相当于执行git fetch; git rebase
git pull --rebase
```

### push
```
将本地仓库的变动提交至远程仓库
git push

将本地仓库的source的提交上传至remote远程仓库的destination上
如果没有source，将创建一个destination分支
git push <remote> <source>:<destination>

强行推送当前分支到远程仓库，即使有冲突
git push [remote] --force

推送所有分支到远程仓库
git push [remote] --all
```

## 五、撤销
### checkout
```
将所有暂存区的文件恢复到工作区
git checkout .

将指定暂存区的文件恢复到工作区
git checkout <file>
```

### commit
```
重写上一次commit
git commit --amend
```

### merge
```
抛弃合并
git merge --abort
```

### reset
```
重置暂存区的指定文件
git reset <file>

重置暂存区和工作区
git reset --hard

重置当前分支的指针为指定提交，重置暂存区，工作区不变
git reset <commit>

重置当前分支的指针为指定提交，重置暂存区和工作区
git reset --hard [commit]

重置当前分支的指针为指定提交，暂存区，工作区不变
git reset --keep [commit]
```

### revert
```
新建一个提交，来撤销指定的提交
git revert [commit]
```

### stash
```
暂时将未提交的变化保存起来
git stash

恢复之前未提交的变化
git stash pop
```

| 作者：[Dk](https://github.com/Darkindom)