> 记录原生vim中的一些概念，不涉及外部插件等用法和配置
> 基本都是从vim的官方帮助文档中总结出来的有用的特性和概念(https://vimhelp.org/)


## 安装

1.  下载最新vim源码包。
2. ./configure --with-features=huge --enable-python3interp --enable-multibyte
3.  执行make && make install



<br>

## 基本概念：

<b>1. 文件，缓冲区，窗口，标签页</b>

文件：磁盘中的数据块

缓冲区：加载到内存中的文件

窗口：用于承载缓存区的容器，通过sp，vs等操作可以创建新窗口(对应于tmux中的panel的概念)

标签页：承载多个窗口的容器，通过tabnew创建




<br>

## vim脚本语法

### 变量类型：

-   全局变量	g:name
-   脚本范围局部变量    s:name 
-   vim预定义变量    v:name 
-   函数内局部变量：l:name 
-   函数参数：a:name 
-   缓冲区局部变量：b:name 
-   窗口局部变量：w:name 
-   标签页局部变量：t:name 

>   脚本范围的局部变量，相当于c语言中static关键字，定义之后，只在当前声明的文件范围有效。

<br>

### 变量定义：

```shell
let g:a = 123
let s:b = "adf"
```



### 取消变量定义：

```shell
unlet s:a
```

### 表达式
1 基本表达式
    数值
    字符串常量
    $name 环境变量
    &name 选项
    @reg  寄存器
2 算法表达式
    + - * / %
    "hello" . "world" =="helloworld" 字符串拼接
### 函数
call function_name(para1,para2) 	调用函数
let line = getline('.') 			调用函数作为表达式
:functions 							查看vim中内置的完整函数列表

1 定义函数
    function {name}({var1,{var2},...})
        {body}
    endfunction
    - 函数名必须以大写字母开始
    - a:var1 代表函数参数变量
    - 函数内部没有特殊标识的变量(g: a: s:)都是局部变量
    - 函数内部访问全局变量要加 g:
2 重定义函数
    function! name(var1,var2)
3 范围使用
    :10,30call funcname()
        function funcname() range 	定义
4 可变参数
    function Show(start,...)
    a:0 	可变参数个数
    a:1 	第一个可变参数
    a:2 	第二个可变参数
    a:000 	可变参数列表
5 删除函数
    delfunction xxx




<br>
<h2>常见问题</h2>

<b>1. win上dos的文件到unix上就会多出一个^M符号</b>
<br>
<b>reason:</b> win上是用两个字符代表换行(\r\n)，而linux采用简洁原则换行符采用(\n)。因此导致win上的文件导入到unix等类平台时会多出一个(\r)字符，而该字符在unxi上就是显示为^M(ctrl-v)
<b>solution:</b> :%s/\r//g


## TODOLIST
1. v:在当前文件查找选择的可视化部分
2. i:如何创建代码片段
