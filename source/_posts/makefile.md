---
title: makefile
date: 2022-04-07 22:31:08
categories:
- 工具
tags:
- make
---


## 预定义变量

```makefile
$* 　　不包含扩展名的目标文件名称。

$+ 　　所有的依赖文件，以空格分开，并以出现的先后为序，可能包含重复的依赖文件。

$< 　　第一个依赖文件的名称。

$? 　　所有的依赖文件，以空格分开，这些依赖文件的修改日期比目标的创建日期晚。

$@ 　 目标的完整名称。

$^ 　　所有的依赖文件，以空格分开，不包含重复的依赖文件。

$% 如果目标是归档成员，则该变量表示目标的归档成员名称。
```




## 函数
```makefile
$(subst FROM,TO,TEXT)
$(subst ee,EE,feet on the stree) //替换“feet on the street“中的ee为EE。结果得到字符串”fEEt on the strEEt”

$(patsubst PATTERN,REPLACEMENT,TEXT)
$(patsubst %.c,%.o,x.c.c bar.c) //替换以.o结尾的字符,函数的返回结果就为”x.c.o bar.o”

$(strip STRING)
去掉字符串STRING开头和结尾的空格，并将其中多个连续空字符合并为一个空字符

$(findstring FIND,IN)
$(findstring a,a b c)返回 a //如果在IN中找到FIND子字符串，则返回FIND，否则返回空

$(filter PATTERN…,TEXT) //返回text中匹配符合pattern的字符串
cc $(filter %.c %s,foo.c bar.c baz.s ugh.h) -o foo  

$(eval string) 	//

$(filter-out PATTERN…,TEXT)
和filter相反，剔除掉TEXT中所有符合模式PATTERN的单词

$(sort LIST)
给字符串LIST中的单词以首字母为主进行排序，并去掉重复的单词

$(word N,TEXT)
$(word 2,foo bar baz) 返回bar  //取字符串TEXT中第N个单词(N的值从1开始)

$(wordlist S,E,TEXT)
$(wordlist 2,3,foo bar baz)   返回”bar baz”  //返回TEXT中从第S到E的单词串

$(words TEXT)
统计TEXT字符串的单词个数，返回值即为单词个数

$(firstword NAMES…)
//返回NAMES的第一个单词
```





### 编译注意事项：

```makefile
windows上涉及到**多线程**和**网络**编译时需要加上（-lwsock32）

```


<h2>Autotool</h2>

>Let's start from the gnu-build-system

