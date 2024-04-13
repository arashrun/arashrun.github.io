---
title: vim-manual
date: 2022-10-19 15:17:32
categories:
- 工具
tags:
- 学习术语
- vim
---




## vim手册
[Vim: help.txt (vimhelp.org)](https://vimhelp.org/)
![vim manual](/images/Pasted%20image%2020221018235635.png)
<br>
vim文档分为基础文档和高阶文档，分别面向vim基本使用者和想要详细了解vim方方面面的高阶用户。

### 面向基本用户的文档

文档分为四个部分：
1. [快速开始](#快速开始)：对vim的一个简单上手教程
2. [高效编辑](#高效编辑)：一些好用的vim建议
3. 调整vim：一些自定义的方式
4. 写自己的vimscrpit脚本：

<center>快速开始</center>


#### 关于手册文档

1. 对两种手册文档的介绍（user manual/reference manual），并介绍如何在文档之间跳转
2. 让用户确认vi-compatibility选项`set compatible?`选项是否关闭
3. 教你如何使用vim tutor一个vim内置的沙盒，用于学习vim基本操作
4. 版权信息

#### 使用vim的第一步

#### 到处移动

**做小的修改**

三种基本的修改文本的方式：
- operation-motion（操作+光标运动）  

	operation操作是对文本的具体操作，motion代表光标运动的方式。这种方式意思就是，在光标运动的范围内进行某项具体操作。比如：`dw`, `dl`, `cw`
	vim中包含的[==operator==]([Vim: motion.txt (vimhelp.org)](https://vimhelp.org/motion.txt.html#operator))如下：
![operator](/images/Pasted%20image%2020221019212831.png)
	vim中支持的光标运动方式==motion==，可以分为如下几类。【重点】
	1. left-right motion（光标左右运动）
	2. up-down motion（光标上下运动）
	3. word motions（光标单词范围运动）
	单词包括
	- ==word==【字母+数字+下划线/非空字符组成的字符串】
	- ==WORD==【非空字符组成的字符串】
		print("hello")
		对上面的句子来说
		word 有四个 print, (", hello, ")
		WORD 有一个 print("hello")
	4. text object motion（光标文本对象范围运动）
	文本对象包含
	- ==sentence==【句子】
	- ==paragraphs==【段落】
	- ==sections==【节】
	5. marks motions（光标标记之间运动）
	6. jump motions（光标的跳转运动）
	7. various motions（其他的光标运动）



<br>

- visual mode（可视模式）
	按`v`进入可是模式，然后移动光标选择想要操作的范围，最后使用operation对选中的部分进行操作。
	1. 多行选择（V进入多行选择模式）
	2. 块选择（c-v进入块选择，操作一个矩形区域）
	3. 光标跳转到另一边（进入可视模式后，按`o`光标跳转到选择区域的另一边
<br>

- text objects（文本对象）
	当光标处于一个单词的中间，但是你想删除这整个单词，普通操作是将光标移动到这个单词开始位置，然后再`dw`，vim提供了一种更简单的方式：文本对象。可以用
	`operation-textobject`方式来总结这种文本修改方式。这种模式是第一种方式的一种特殊情形，属于text-object motion类别
	因此`daw`就可以直接删除光标的单词(delete a word)。


<br>

#### 修改配置

1. vimrc文件
2. example vimrc解释
3. default.vim文件解释
4. [简单映射](https://vimhelp.org/usr_05.txt.html#05.4)
> mapping:=让你将一系列vim commands绑定到一个key上
> :map :=map命令，显示当前的mapping


5. [添加==package==](https://vimhelp.org/usr_05.txt.html#05.5)

> package:= 包是一个目录，用于包含一个或多个plugin。
> optional package
> automatically load package
> packadd!

```
~/.vim/pack/fancy/start/fancytext/plugin/fancy.vim
上面的fancy目录是自己起的任意名称，用于对插件进行分类存储。其中fancytext才是package的名称
```


6. [添加==plugin==](https://vimhelp.org/usr_05.txt.html#05.6)
> global plugin:=全局插件
> filetype plugin:=文件类型插件

- 全局插件
- 创建一个全局插件
- 使用一个全局插件
- 文件类型插件
- 创建一个文件类型插件
- 使用一个文件类型插件

7. [添加帮助文档](https://vimhelp.org/usr_05.txt.html#05.7)
> runtimepath:=
> :helptags := 生成本地tags文件[helptags](https://vimhelp.org/helphelp.txt.html#%3Ahelptags)

- 写本地的帮助文件

8. options窗口
> options:=vim中内部的变量或者开关，设置用于实现一些特殊功能。options的值分为三类：boolen，number，string
> :options:=用于查看vim中的内置选项，会打开一个选项窗口

- [设置options](https://vimhelp.org/options.txt.html#set-option)
- [自动设置options](https://vimhelp.org/options.txt.html#auto-setting)
- [可用options列表](https://vimhelp.org/options.txt.html#option-summary)

9. 经常使用的options
- 不要自动换行(set nowrap)
- 前进，后退自动换行(set whichwrap=b,s)
- 显示tabs（set list）
- 定义word的构成字符（set iskeyword+=）
- 设置底部命令行的空间大小(set cmdheight=3)
<br>

**使用语法高亮**

1. 开关语法高亮功能
2. 颜色错误，没有颜色的可能原因
3. 不同颜色
4. 开启颜色，或关闭颜色
5. 带颜色打印文本
6. 跟多相关内容
- [自定义语法高亮](https://vimhelp.org/usr_44.txt.html#usr_44.txt)

<br>

**编辑多个文件**

<br>

**窗口拆分**

<br>

**使用GUI界面**

<br>

**做大修改**

在visual模式下可以做大范围修改

1. 录制和回放一系列命令

<br>

**从crash中恢复**

<br>

**一些技巧**

****


<center>高效编辑</center>

**快速输入命令行命令**

**离开和回来**

**查找文件去编辑**

**编辑其他文件**

**快速插入**

**编辑格式化文本**

**重复**

**搜索命令和模板**

**折叠**

**在程序文件之间移动**

**编辑代码**

**使用gui**

**undo树**



<center>调整vim</center>

<center>vimscript入门</center>


### 面向高阶用户的文档

这部分文档介绍vim的方方面面的特性和机制，文档分为11个部分
1. 通用主题
2. 基本编辑
3. 进阶编辑
4. 特殊问题
5. 编程相关特性支持
6. 文本语言支持
7. gui界面相关
8. 编程接口相关
9. vim版本变迁（changelog）
10. 特殊系统需要注意的问题
11. vim标准插件

虽然有11个部分，但是我们需要特别学习的其实就是前5个部分。第8个部分对写vim插件的开发者有帮助


