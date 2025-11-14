---
title: git-skill
date: 2022-04-07 22:31:08
categories:
- 工具
tags:
- git
---



## git基本概念

[git reference](https://gitee.com/all-about-git)

- diff & patch usage
[Git 打补丁-- patch 和 diff 的使用](https://www.jianshu.com/p/ec04de3f95cc)
[git 补丁 - diff 和 patch 使用详解](https://cloud.tencent.com/developer/article/1423939)

### git 基本操作


1. git clone私有仓库
```shell
git clone https://username:password@github.com/username/repo_name.git
```
2. add, commit

3. git撤销修改
```shell
git的撤销是基于修改的，比如修改工作空间的文件内容是一种修改操作，git add 到缓存区也是一种修改操作，git commit也是。
因此基于这三种修改操作就会有对应的三种撤销操作。分别是：

- 修改了工作空间的文件，想完全删除修改的内容。
    git checkout xxx
- 使用git add操作，想撤销add操作。
    git reset HEAD xxx
- 使用git commit操作，想撤销commit的操作。
    git reset --hard HEAD^
```
4. 修改git远程厂库地址
```shell
$ git remote -v
origin  root@192.168.145.128:~/opt/git/tools.git (fetch)
origin  root@192.168.145.128:~/opt/git/tools.git (push)
$ git remote set-url origin root@192.168.147.130:~/opt/git/tools.git
```
5. 取消文件跟踪(取消git add)
```bash
git rm -r --cached xxx/         -- 不删除本地文件，只是取消git跟踪
git rm -r --f xxx/              -- 删除本地文件，取消跟踪
```

6. 推送本地新建分支到远程新建分支（创建远程分支），并在两者之间建立联系。
```bash
git push <remote repo name> <remote new branch>:<local new branch>
git branch --set-upstream <branch> <remote-branch>
```


### rebase

对本地提交A进行补充修复或提交

```bash
git commit --fixup <hash_A>
git rebase -i <Commit_Before_A_Hash>

pick <Hash_A> Commit A message
fixup <Hash_Fixup_A> Fixup! Commit A message  <-- 移动到 A 后面，操作改为 fixup
pick <Hash_B> Commit B message
pick <Hash_C> Commit C message
```

### git 子模块

1. git submodule add <git path>
