---
title: git-skill
date: 2022-04-07 22:31:08
categories:
- 工具
tags:
- git
---



## 基本概念

[git reference](https://gitee.com/all-about-git)

- diff & patch usage
[Git 打补丁-- patch 和 diff 的使用](https://www.jianshu.com/p/ec04de3f95cc)
[git 补丁 - diff 和 patch 使用详解](https://cloud.tencent.com/developer/article/1423939)

[git am 解决冲突](https://www.cnblogs.com/joker1937/p/15731049.html) 

## 基本操作


1. git clone
```bash

# clone私有仓库
git clone https://username:password@github.com/username/repo_name.git

# 本地仓库clone
git clone --depth=1 -b dev file:///home/like/code <dst>

# 本地仓库clone多个分支到一个仓库
# 1. 克隆第一个分支 dev-2.0
git clone --depth=1 -b dev-2.0 file:///home/like/code/ $dst
# 2. 进入目录
cd $dst
# 3. 添加第二个分支的追踪信息
git remote set-branches origin dev-2.0 dev-2.1
# 4. 拉取第二个分支的深度为 1 的数据
git fetch --depth=1 origin dev-2.1
# 5. 创建本地分支追踪远程
git checkout -b dev-2.1 origin/dev-2.1
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

# 删除untracked files
# 查看待清理文件
git clean -n 
# 执行清理动作
git clean -fd
```

6. 推送本地新建分支到远程新建分支（创建远程分支），并在两者之间建立联系。
```bash
git push <remote repo name> <remote new branch>:<local new branch>
git branch --set-upstream <branch> <remote-branch>
```


## rebase

对本地提交A进行补充修复或提交

```bash
git commit --fixup <hash_A>
git rebase -i <Commit_Before_A_Hash>

pick <Hash_A> Commit A message
fixup <Hash_Fixup_A> Fixup! Commit A message  <-- 移动到 A 后面，操作改为 fixup
pick <Hash_B> Commit B message
pick <Hash_C> Commit C message
```

## 子模块

```bash
git submodule add <git path>
```


## bundle


在 `git bundle` 命令中，`<git-rev-list args>` 是最关键的部分，它决定了你的“离线包”（bundle）里**包含哪些提交记录（History）**。

简单来说，它的语法和 `git log` 后面跟的参数完全一样，用于**划定一个范围**。

---

### 1. 为什么需要它？

`git bundle` 的本质是将一部分仓库数据打包成一个单一文件。为了让这个文件在另一台电脑上能被顺利“解压”（fetch/pull），你需要告诉 Git：

- 从哪个点开始打包？
    
- 打包到哪个点结束？
    

### 2. 常见的参数写法

#### A. 打包全量历史（整个分支）

如果你想把某个分支从诞生到现在的全部内容都打进去：
```
git bundle create my-repo.bundle main
```

_这里的 `main` 就是一个 rev-list 参数，代表“从 main 分支的最新点往回追溯到最初”。_

#### B. 打包增量更新（范围选择）

这是最常用的场景。假设你上次已经给对方传过数据了，这次只想把 `v1.0` 之后的新提交打包：
```
git bundle create incremental.bundle v1.0..main
```

- `v1.0..main` 意思就是：**包含所有在 main 分支上、但不在 v1.0 标签中的提交。**
    
- 这能极大地减小打包文件的体积。
    

#### C. 打包所有分支和标签

如果你想备份整个仓库：
```
git bundle create full-backup.bundle --all
```

- `--all` 会告诉 `git-rev-list` 遍历所有的 refs。
    

---

### 3. 本质：它是如何工作的？

当你输入这些参数时，Git 内部会调用 `git rev-list` 命令。这个命令的作用是**列出所有符合条件的提交哈希值**。

- 如果你提供 `master`，它会列出从 master 开始往回数的所有 commit。
    
- 如果你提供 `origin/master..master`，它只会列出你本地领先于远程的那几个提交。
    

### 4. 特别注意：必须包含引用（Ref）

在 `git bundle` 中，你**必须**在参数中包含一个分支名或标签名，否则别人拿到这个包后，不知道该把这些提交挂在哪个“分支指针”下。

> **错误示范：** `git bundle create test.bundle HEAD~3..HEAD`
> 
> 虽然这划定了 3 个提交，但包里没有分支名，对方可能无法直接通过分支方式拉取。
> 
> **正确示范：** `git bundle create test.bundle main -3` 或 `git bundle create test.bundle main ^v1.0`

### 总结

你可以把 `<git-rev-list args>` 理解为**“提交范围过滤器”**。

- 想打包全部？写**分支名**。
    
- 想打包部分？写 **`旧版本..新版本`**。
    
- 想打多个分支？写 **`branch1 branch2`**。



收到这个 `.bundle` 文件后，对方的操作逻辑其实和从 GitHub/GitLab 上 `git fetch` 或 `git pull` 几乎一模一样，只不过把 **URL** 换成了 **文件路径**。

根据你提供的信息，这个包是一个**增量包**（它明确标注了 `requires` 某个特定提交），所以同步步骤如下：

---

### 1. 验证兼容性（可选但推荐）

在同步之前，对方可以先检查自己的本地仓库是否拥有该 bundle 所需的基础（即那个 `3a2f8b...`）。
```
git bundle verify /路径/to/.dev.bundle
```

如果输出 `is okay`，说明对方的本地仓库里有那个“前置提交”，可以顺利合并。

---

### 2. 从 Bundle 中提取数据

对方不需要解压这个文件，直接在他们的 Git 仓库目录下运行以下命令：

#### 方案 A：直接合并到当前分支（类似 git pull）

如果对方正处于 `dev` 分支，想直接把补丁打上去：

```
git pull /路径/to/.dev.bundle dev
```

#### 方案 B：先获取更新，再手动处理（类似 git fetch）

这是更稳妥的做法，先将内容取回，存放在一个临时分支或远程追踪分支里：

```
# 将 bundle 里的 dev 分支取回并重命名为本地的 bundle-res 分支
git fetch /路径/to/.dev.bundle dev:bundle-res
```

之后，他们可以查看差异并合并：

```
git log main..bundle-res --oneline  # 查看补丁里多了什么
git merge bundle-res                # 合并到当前分支
```

---

### 3. 将其视为一个“离线远程仓库” (进阶用法)

如果对方需要频繁通过这个 bundle 同步，可以把它像远程仓库一样“挂载”起来：

```
git remote add offline-src /路径/to/.dev.bundle
git fetch offline-src
```

这样，以后只要你发了新的同名 bundle 覆盖旧文件，他只需要运行 `git fetch offline-src` 就能看到更新。

---

### ⚠️ 关键注意事项

1. **基础要求 (The Requires)**：
    
    对方的仓库**必须**包含那行 `3a2f8bb9...` 的提交记录。如果对方的进度比这个号还旧（或者完全是另一个仓库），同步会报错，提示 `error: Repository lacks these prerequisite commits`。
    
2. **分支名称**：
    
    你在打包时指定了 `refs/heads/dev`，所以对方在 `fetch` 或 `pull` 时，必须指定 `dev` 这个名字。
    
3. **路径问题**：
    
    命令中的路径可以是相对路径，也可以是绝对路径。如果在 Windows 上，路径看起来像 `C:\Users\Name\Desktop\dev.bundle`。
    

**总结一句话给对方：**

> “运行 `git pull /文件路径/dev.bundle dev` 即可同步。”



## 统计脚本

统计仓库提交
```bash
#!/bin/bash

# 替换 "username" 为实际的用户名
user_commits=$(git log --author="jerry" --since="1 year ago" --pretty=format:"%h" | wc -l)
total_commits=$(git log --since="1 year ago" --pretty=format:"%h" | wc -l)
percentage=$(echo "scale=2; $user_commits / $total_commits * 100" | bc)

echo "User commits in the last year: $user_commits"
echo "Total commits in the last year: $total_commits"
echo "User's commit percentage: $percentage%"


# 替换 "username" 为实际的用户名
user_commits=$(git log --author="jerry" --since="1 year ago" --no-merges --pretty=format:"%h" | wc -l)
total_commits=$(git log --since="1 year ago" --no-merges --pretty=format:"%h" | wc -l)
percentage=$(echo "scale=2; $user_commits / $total_commits * 100" | bc)

echo "User commits in the last year (excluding merges): $user_commits"
echo "Total commits in the last year (excluding merges): $total_commits"
echo "User's commit percentage (excluding merges): $percentage%"
```
