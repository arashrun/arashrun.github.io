---
title: hexo插件
date: 2023-03-07 22:13:04
categories:
- web
tags:
- hexo
- 插件
---


[Plugins · hexojs/hexo Wiki (github.com)](https://github.com/hexojs/hexo/wiki/Plugins)

hexo插件用于在不修改hexo源码的情况下扩展hexo博客的功能，hexo插件分为两种
- script类型：用于单文件实现的简单插件，放在根目录script目录下即可自动加载
- plugin类型：多文件的复杂插件需要在`node_module`目录下创建以`hexo-`打头的文件夹下，以符合node模块的方式编写即可，完成之后需要在根目录下的 `package.json` 中添加依赖

hexo提供了如下一些插件用于加速你对插件开发

![](/images/hexo-tools.png)


目前已有的插件基本上按功能分，不限如下几类：
- Server：hexo-admin在线修改文章
- Generator：生成器类型插件，用于生成不同类型的静态文件,rss,html,css
- Renderer：渲染器类型，用于处理不同类型文件如，ejs，less，stylus，markdown等
- Migrator：用于从不同博客类型迁移到hexo，rss，wordpress，
- MultiMedia：多媒体插件，提供插入多媒体文件功能，各种远程图片，video等



### Render插件

#### Hexo-renderer-markdown-it-plus

- 基于[Hexo-renderer-markdown-it](#hexo-renderer-markdown-it)插件，做了markdown-it插件集成和拓展

该插件默认集成并开启了如下这些markdown-it插件

![](/images/hexo-it-plus.png)

#### Hexo-renderer-markdown-it

- 基于 [markdown-it](#markdown-it) 库的markdown渲染器
- 比hexo的默认自带的markdown渲染器[Hexo-renderer-marked]更快
- 支持如下特性，注脚，上下标，下划线

#### markdown-it

- 一个js版本的markdown语法解析器，支持CommonMark规范，支持扩展，语法插件，且高速


#### Hexo-renderer-marked

- hexo初始化默认的markdown渲染器，npm删除会导致无法执行md->html的转换（若没有其他的渲染器的话）
- 基于 [marked](#marked) 库，做成hexo拓展
- 可扩展性好，可以自定义标签的渲染


#### marked

- 一个markdown解释器js库