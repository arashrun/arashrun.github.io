---
title: C++中IO流体系
date: 2023-03-20 08:21:04
categories:
- c++
tags:
- c++
- io操作
---


### stream类型

stream有如下几种类型

1. 标准输入输出流
2. string流
3. file流

### stream类继承体系

![](/images/IO流类的继承体系.png)
![](/images/cppref.png)


### ios

```mermaid
classDiagram
ios_base <|-- basic_ios
basic_ios <|-- ios : char模板类
basic_ios <|-- wios : wchar模板类

fpos <|-- streampos: char模板类
fpos <|-- wstreampos: wchar模板类

%% 定义如下
class basic_ios {
<<类的模板>>
}
class fpos {
<<类模板>>
}
```

- ios_base 和 basic_ios 定义了不区分是输入流还是输出流的基本功能
- ios_base 独立于模板参数（不是模板类），描述了所有类型的stream的最基本部分，独立于stream的具体数据类型
- basic_ios 依赖于模板参数，是一个类模板，通过传入不同的模板参数创建不同模板类
- fpos 代表在stream中的位置


### istream-ostream

```mermaid
classDiagram

basic_istream <|-- istream : char模板类
basic_istream <|-- wistream: wchar模板类

basic_ostream <|-- ostream: char模板类
basic_ostream <|-- wostream: wchar模板类

basic_iostream <|-- iostream: char模板类
basic_iostream <|-- wiostream: wchar模板类

class basic_istream {
<<类模板>>
}

class basic_ostream {
<<类模板>>
}

class basic_iostream {
<<类模板>>

}
```

- basic_istream继承自basic_ios。因此具有basic_ios的功能

