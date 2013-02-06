---
layout: post
title: "long varchar in DB2"
date: 2013-02-06 00:55
comments: true
categories: DB2
---
DB2 中LONG VARCHAR 与VARCHAR 数据类型都用来存储长文本，但是它们之间的用法有很大不同。VARCHAR 与普通数据类型一样，要使用到bufferpool，在创建表时受制于最大的bufferpool page size，而LONG VARCHAR 则与LOB数据一样，有单独的存储区域，不需要使用bufferpool，所以在创建表时也不需要有大的bufferpool存在，在访问这些数据时，直接操作磁盘IO进行存取，所以速度更快。但LONG VARCHAR 数据类型的使用也相应受到限制，不能用在以下语句中：

- DISTINCT
- GROUP BY 
- ORDER BY 
- BETWEEN/IN 
- LIKE 
- 子查询内部 
- 列函数中

LONG VARCHAR 允许的数据最大长度为32700字节，VARCHAR 最大允许32672字节。在CLP与CE中操作LONG VARCHAR 会有一些不期盼的事情发生，比如对于长度大于8192字节的LONG VARCHAR列使用以下语句，会导致截断，并且不给出任何warning。 

`SELECT longvarchar FROM table` 
使用以下语句也是不安全的，因为一旦列长度超出VARCHAR 允许的最大长度32672，语句将会失败。 

`SELECT VARCHAR(longvarchar) FROM table`
安全的写法是使用表达式CAST. 

`SELECT CAST(langvarchar AS VARCHAR(32672)) FROM table` 
以上内容适用于DB2 版本8以及版本9。 

longvarchar 字段在where条件中可以用like，不能用＝。