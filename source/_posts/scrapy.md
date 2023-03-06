---
title: scrapy
date: 2023-03-06 14:25:07
categories:
- 爬虫
tags:
- 三方库
- 框架
---

### 参考资料

- [简单视频入门](https://www.bilibili.com/video/BV1p84y1z7kp/?spm_id_from=333.999.0.0)



### scrapy项目的一般步骤





### scrapy爬虫常见问题

1. 为了排查爬虫运行时出现的异常问题，需要调试手段。常用的将log输出保存到文件中，便于查找问题
	[ 爬虫scrapy框架--log日志输出配置及使用scrapy 日志](https://blog.csdn.net/weixin_41666747/article/details/82716688)

2. 当数据中有gbk无法写入的字符时，文件在保存的时候记得设置encoding为utf-8,如果时windows上，需要设置为utf-8-sig（utf-8 with bom）

```python
open('bilibili-top100-{}.csv'.format(self.today), 'w', encoding='utf-8')
```

3. 当出现需要代理才能连接上的网站时，需要设置代理。有两种方式设置代理，一种是直接在构造request请求时添加meta参数。第二种是自定义download middleware。
	[Scrapy 设置代理终极宝典 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/79067223)

	若出现如下错误，就代表需要通过代理才能访问该网站
```shell
[<twisted.python.failure.Failure OpenSSL.SSL.Error: [('SSL routines', '', 'unexpected eof while reading')]>]
```
