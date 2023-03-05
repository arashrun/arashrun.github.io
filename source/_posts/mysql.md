---
title: mysql
date: 2023-03-06 00:19:32
categories:
- 数据库
tags:
- mysql
---


## mariadb数据库

mysql的开源版本，mysql被oracle公司收购。mysql之父又写了一个就是mariadb



### mariadb的C库

```c
		//相关数据结构
		MYSQL：代表数据库连接
		MYSQL_SET:代表结果集和元数据
		MYSQL_ROW:一个字符指针数组，指针指向实际数据所在列
		MYSQL_FIELD:表示某一列的元数据
		MYSQL_STMT:表示准备好的语句句柄
		MYSQL_BIND:用于为准备好的语句提供参数，或为接受输出列的值
		MYSQL_TIME:用于日期和时间值

		//建立连接的相关函数
		mysql_init:准备并初始化MYSQL结构，该结构被mysql_real_connect使用
		mysql_real_connect:与要求的数据库进行连接，并返回MYSQL句柄
		mysql_thread_init:用于多线程的程序
		mysql_options:用于设置额外的连接选项，并影响连接行为======mysql_optionsv()
		mysql_close:关闭一个之前打开的连接

		//查询相关函数
		mysql_query:执行一个语句(二进制不安全的：读取字符串时考虑字符转义的问题。二进制安全：不考虑字符转义的问题。)
		mysql_real_query:执行一条语句(二进制安全)
		mysql_hex_string:允许语句中出现16进制
		mysql_store_result:返回一个结果集
		mysql_free_result:释放store分配的动态内存
		mysql_use_result:用于初始化上一次查询结果集的索引值
		mysql_select_db:选择另一个数据库
		mysql_send_query:

		//行列相关的操作
		mysql_num_fields:列数
		mysql_field_count:
		mysql_field_seek:
		mysql_field_tell:
		mysql_fetch_field:
		mysql_fetch_fields:
		mysql_fetch_field_direct:

		mysql_num_rows:行数
		mysql_row_seek:
		mysql_row_tell:

		mysql_affected_rows:被影响的行数


		//工具函数
		mysql_ping:检测和服务器的连接是否在工作c
		mysql_error:
```

