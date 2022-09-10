---
title: Hexo指南
date: 2022-04-07 22:32:19
tags: 技术
---


### 写作流程

1. 创建新文章

```shell
hexo new [layout] <title>

- post
- page
- draft

hexo new post -p cs-theory/xxx "title name"
```


2. 部署

```bash
hexo d -g
```

### 主题的修改

主题是该hexo博客的一个子项目

1. 子项目（主题）的修改

可以直接修改self-hexo-theme目录中的内容，修改完成之后，通过正常的三部曲提交到主题仓库

2. 子项目（主题）的同步

通过`git submodule update`就可以同步主题的修改。

### 一些说明

github上的两个分支，master和hexo分支。默认clone和展示的分支是hexo分支。hexo分支就是保存我们本地文件的分支。而master分支是hexo进行generate和deploy的分支，当我们写完文章准备同步到远程的时候。我们需要同时更新这两个分支。

1. 更新hexo分支

- git add .
- git commit
- git push

2. 更新master分支

- hexo deploy -g

master分支可以看作是存放public内容的分支，public目录是通过命令`hexo g`生成的。该命令会将所有文章和主题等文件和资源进行处理然后拷贝到public中。

`hexo deploy`Hexo 会将 public 目录中的文件和目录推送至 `_config.yml` 中指定的远端仓库和分支中，并且完全覆盖该分支下的已有内容。
