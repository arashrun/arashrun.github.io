---
title: lambda
date: 2023-03-06 00:31:48
categories:
tags:
- c++
---



## lambda表达式

### [C++ 中的 Lambda 表达式](https://docs.microsoft.com/zh-cn/cpp/cpp/lambda-expressions-in-cpp?redirectedfrom=MSDN&view=vs-2019)



![](/images/lambuda组成.png)

>   [lambda 表达式的概念和基本用法](http://c.biancheng.net/view/3741.html)

**概念和形式：**

lambda 表达式定义了一个匿名函数，并且可以捕获一定范围内的变量。lambda 表达式的语法形式可简单归纳如下：

```c++
[ capture ] ( params ) opt -> ret { body; };
```

其中 capture 是捕获列表，params 是参数表，opt 是函数选项（一般就是mutable），ret 是返回值类型，body是函数体。



**捕获列表：**

lambda 表达式可以通过捕获列表捕获一定范围内的变量：

-   [] 不捕获任何变量。
-   [&] 捕获外部作用域中所有变量，并作为引用在函数体中使用（按引用捕获）。
-   [=] 捕获外部作用域中所有变量，并作为副本在函数体中使用（按值捕获）。
-   [=，&foo] 按值捕获外部作用域中所有变量，并按引用捕获 foo 变量。
-   [bar] 按值捕获 bar 变量，同时不捕获其他变量。
-   [this] 捕获当前类中的 this [指针](http://c.biancheng.net/c/80/)，让 lambda 表达式拥有和当前类成员函数同样的访问权限。如果已经使用了 & 或者 =，就默认添加此选项。捕获 this 的目的是可以在 lamda 中使用当前类的成员函数和成员变量。

```c++
// captures_lambda_expression.cpp
// compile with: /W4 /EHsc
#include <iostream>
using namespace std;

int main()
{
   int m = 0;
   int n = 0;
   [&, n] (int a) mutable { m = ++n + a; }(4); //m使用引用传递，n按值传递。mutable说明按值传递的n可以修改n值，等价于引用方式
   cout << m << endl << n << endl;
}
```

