---
title: Hexo指南
date: 2022-04-07 22:32:19
categories:
  - web
tags:
  - 技术
  - hexo
  - ejs
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

### 主题定制

1. 主题的设置

	修改博客配置\_config.yml中的theme为theme文件夹下的目录

2. [主题目录结构](https://hexo.io/zh-cn/docs/themes)

3. 添加新模板

	主题模板(区别于hexo根目录下的scaffolds目录中的模板，scaffolds目录中的模板用于创建不同布局文件时的format-header自动填充)，指的是页面的显示模式，不同功能的页面需要不同的模板以适应不同内容的显示。theme下的layout目录中的各种ejs就代表不同的模板，如：index.ejs是首页模板，用于渲染首页；tags.ejs是tag页面模板，用于渲染tags页面等。这些不同的模板当然可以相互之间毫无关联，均开始于 `html` 标签，但由于复用的缘故，模板默认使用一个共同的布局——layout.ejs。该布局文件layout.ejs中需要提供 `<%- body %>` ，其他ejs模板就可以公用layout中的公共部分了


```bash
1. 新建一个新模板
	hexo new page "about"
2. 修改模板头部信息,添加layout头部信息
	cd _post/about
	vim index.md
	layout: about

3. 在theme/xxx/layout目录下创建模板ejs文件 `about.ejs`
	cd layout
	vim about.ejs
```

4. ejs简单说明

	ejs是一种模板引擎，类似于php。用于动态生成html。在ejs标记内部使用的是js来作为编程语言的。通过模板引擎，让我们的html可以在前端就动态生成了。

### 一些说明

github上的两个分支，master和hexo分支。默认clone和展示的分支是hexo分支。hexo分支就是保存我们本地文件的分支。而master分支是hexo进行generate和deploy的分支，当我们写完文章准备同步到远程的时候。我们需要同时更新这两个分支。

> master分支：hexo博客部署和展示的分支，html
> hexo分支：博客修改和创作的分支,md
> 

1. 更新hexo分支

- git add .
- git commit
- git push

2. 更新master分支

- hexo deploy -g


> [!NOTE]
> 
> 1. hexo部署流程已通过Github Actions的功能进行了优化，文章写完之后推送到远程即可实现自动化部署到Github Pages界面
> 2. 更好的笔记部署方式是通过obsidian插件`webpage html export` 插件将笔记转换成html之后，部署到vercel这个 `静态界面托管平台` [^1][^2] 进行管理，这个平台只需要一个GitHub网址，即可在你更新仓库之后自动部署到vercel平台，不需要编写Github Actions文件。并且该插件能够保留你obsidian中当前的布局和主题，真正做到所见即所得的网址效果



### 站点

- [Github Pages](https://github.com/arashrun.github.io)
- [Vercel](https://arashrun.vercel.app)



[^1]: 推荐的托管平台
	- [几个免费的静态网站托管平台对比 | xxyopen](https://www.xxyopen.com/2022/07/19/tools/pages_host.html)
	- [静态托管平台收集 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/180152636)


[^2]: vercel的站点需要一个 `index.html` 文件才能正常访问，使用webpage插件导出的笔记目录中是没有该index.html的，导致访问vercel站点会出现404错误，可以手写一个入口界面，指向指定的内部界面即可