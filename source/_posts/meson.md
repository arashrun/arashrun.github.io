---
title: meson
date: 2022-04-07 22:31:08
categories:
- 工具
tags:
- 语法
---

## yml文件

> yml文件和json文件类似，是一种配置文件。但由于其语法更加简洁明了，因此也常出没在github中（.travis.yml）
>
> 

### 语法

**规则**：

- 大小写敏感
- 用缩进表示层级关系
- #表示注释

**数据结构**

1. 对象

   ```yaml
   # conf.yml
   animal: pets
   hash: { name: Steve, foo: bar }
   ```

   等价于

   ```json
   {
       { "animal": "pets" },
       { "hash": { "name": "Steve", "foo": "bar" } }
   }
   ```

   

2. 数组

   ```yaml
   # conf.yml
   Animal:
    - Cat
    - Dog
    - Goldfish
   ```

   ```json
   { "Animal": [ "Cat", "Dog", "Goldfish" ] }
   ```

   

3. 字符串

   ```yaml
   # conf.yml
   # 正常情况下字符串不用写引号
   str: 这是一行字符串
   # 字符串内有空格或者特殊字符时需要加引号
   str: '内容： 字符串'
   ```

   

4. null

   ```yaml
   # conf.yml
   parent: ~
   ```

   ```json
   { "parent": null }
   ```

   





