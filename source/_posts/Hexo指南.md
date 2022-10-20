---
title: Hexo指南
date: 2022-04-07 22:32:19
categories:
- 静态网站
tags: 
- 技术
- hexo
- ejs
- node.js
- jquery
- 前端
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

2. git提交修改

```shell
git add xxx
git commit -m ""
git push
```
git push之后，内容就会被推送到hexo分支上

### 部署流程

1. clean and generate public floder
```bash
hexo clean
hexo g
```

2. 推送并部署
该处推送，会将master分支（部署分支）推送到远程master分支上。
```bash
hexo d
```

### 主题的定制

1. 主题的设置
修改博客配置\_config.yml中的theme为theme文件夹下的目录

2. [主题目录结构](https://hexo.io/zh-cn/docs/themes)
一个主题的目录结构如下：
- layout/    网站页面的布局文件,可以当成是html，不过使用到了ejs模板引擎调整过后的html
    - \_partial/    不同布局页面会有一些公共的组件，布局文件通过partial函数可以使用这些公共组件
- source/    网站的css/js等文件
- \_config.yml    主题级别的配置文件

3. 新添加layout
一种layout代表了一种页面的显示形式，比如网站首页，类别页面，tags页面等这些页面的内容显示方式是不同的。自定义一种新的页面，就是在layout目录下新建一个<new layout>.ejs 模板文件。然后在source目录下新建一个实际显示内容的页面，并将其Front-matter中的layout修改为你的自定义布局<new layout>，然后即可将模板内容显示到新建的页面中了。
```bash
1. 新建layout页面
hexo new page <new layout>
2. 修改layout头部信息
```

4. ejs简单说明

ejs是一种模板引擎，类似于php。用于动态生成html。在ejs标记内部使用的是js来作为编程语言的。通过模板引擎，让我们的html可以在前端就动态生成了。

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

3. ejs中插入bing每日一图

> [bing每日一图url](https://api.muvip.cn/doc/bing)






## 再次明确

做笔记的目的在于构建某一个方面的`知识体系`和记录在实践中碰到的问题的解决方案的。而不是用来记录一些零散的自己认为很重要的东西