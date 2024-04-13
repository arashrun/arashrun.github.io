---
title: python导入系统
date: 2023-04-03 15:36:25
categories:
- python
tags:
---



1. python中有多种导入机制，import语句是最常用的，但不是唯一的，`importlib.import_module()` 函数和内建的 `__import__()` 函数也能用于模块导入。
2. import语句会执行两部操作来实现导入机制：首先查找命名的模块，然后将找到的结果绑定到本地空间的一个名称上
3. 查找操作实际调用的是 `__import__()` 函数，如果找到了则会执行模块创建操作，但是会有一些副作用，比如会导入parent package，更新各种缓存，只用 `import` 语句会执行名称绑定操作
4. 当import语句执行的时候，标准的 `__import__()` 函数会被调用到，其他的一些导入方式如 `importlib.import_module()` 可以选择不通过 `__import__()` 而使用自己定义的方式来实现导入功能
5. 当一个模块第一次被导入，python会搜索这个模块，如果找到了就会创建一个模块对象并初始化它，如果没有找到，`ModuleNotFoundError` 就会被抛出。python有各种搜索命名模块的策略，这些策略可以通过使用各种hook技术来进行修改和扩展
6. `importlib` 模块提供了丰富的接口用于修改python的导入系统


### packages

1. python中只有一种模块对象————packages
2. 可以拿文件系统中的目录和文件来打比方，packages相当于目录，modules相当于文件，但是又与这个比喻(analogy)有差异。因为packages和modules不是通过文件系统来组成的，packages可以递归包含，这个和 `regular modules` 是一样的
3. ==所有的packages都是modules，但不是所有的modules都是packages。一个module如果包含一个 `__path__` 属性，那么它就被看作一个package==
4. 所有的modules都有一个名字，子包名称与其父包名称之间用一个点分隔，类似于 Python 的标准属性访问语法。例如：父包 `email` ，包含有一个子包 `email.mime` ，子包中又包含一个module叫做 `email.mime.text` 

### Regular Packages

1. python中两种package类型
	1. regular packages（以下简称RP）
	2. namespace packages（简称NP）
2. regular packages属于传统的包类型存在于python3.2之前。典型的特征是一个目录中包含一个 `__init__.py` 文件。当一个RP被导入，这个init文件就会被隐式被执行，其中的对象就会被绑定到这个包的namespace中

### Namespace Packages

1. NP是由多个部分(portion)组合而成的一个集合体（composite），每个部分看作父包的一个子包。每个部分可能存在于文件系统中的不同位置。可能存在于zip等压缩文件中，在网络上，在任何python导入时可以搜索到的地方
2. NP不使用一般的list作为 `__path__` 属性，而是使用一个自定义的可迭代类型，该迭代类型可用于执行自动进行下一步搜索
3. NP不存在 `__init__.py` 文件，因为NP中的各部分都是由散落各地的package组成。

### Searching

1. 开始搜索模块之前，python需要获取模块的完全限定的名称，该名称会用于搜索的不同阶段。例如 `foo.bar.baz` 这个名称，python搜索分为3个阶段，首先尝试导入 `foo` ， 然后尝试导入 `foo.bar` ，最后尝试导入 `foo.bar.baz` ，这中间的(intermediate)任何一个阶段导入失败，都会抛出模块未找到的异常
2. 搜索策略如下：
	1. 首先搜索模块缓存 ==`sys.modules`== 这个字典对象，key为模块名，value为module object。
	2. 如果在模块缓存中搜索不到指定的模块，python下一步搜索 ==`sys.meta_path`== 这个list中存储的所有 ==`meta path finder`== 对象（简称：MPF），这些MPF按照顺序进行查找相关模块，MPF对象必须实现 `find_spec()` 方法，用于实现搜索策略
	3. import机制是可扩展的，因此可以添加新的finder，从而扩展模块的搜索范围。finder在找到模块之后不会实际地加载模块，而是返回一个 ==`module spec`== 对象，该对象中包含了模块导入相关的信息，然后import机制之后根据这个spec对象加载模块
3. import hooks：为了实现导入机制的可扩展，基本的方式就是使用 `import hooks` 技术，该技术分为两种： `meta hooks` 和 `import path hooks` 。
4. `meta.hooks` 在所有导入步骤之前起作用(当然 `sys.modules` 缓存查找不算在内)，因此这就允许该类型hook可以复写 `sys.path` 过程，frozen modules, 甚至是内建模块。通过在 `sys.meta_path` 中新增一个finder对象（对应于 `meta path finder` ），我们就实现了一个meta hooks的注册
5. `import path hooks` 被认作是 `sys.path` 处理过程中的一部分，该hooks可以通过往 ==`sys.path_hooks`== 中添加新的可调用对象来实现。这种hooks方式对应于 `path entries finder` 。





### 参考文档

[5. The import system — Python 3.11.2 documentation](https://docs.python.org/3/reference/import.html#package-relative-imports)