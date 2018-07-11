---
layout: post
title:  "MYSQL存储引擎"
date:   2018-07-11 14:22 +0200
categories: [web]
tags: [mysql]
---

数据库存储引擎是数据库底层软件组织，数据库管理系统（DBMS）使用数据引擎进行创建、查询、更新和删除数据。不同的存储引擎提供不同的存储机制、索引技巧、锁定水平等功能，使用不同的存储引擎，还可以 获得特定的功能。现在许多不同的数据库管理系统都支持多种不同的数据引擎。MySQL的核心就是存储引擎。

## 存储引擎的选择

不同的存储引擎都有各自的特点，以适应不同的需求，如下表所示：

|  功能  |  MYISAM  |  Memory  |  InnoDB  |  Archive  |
|:------:|:------:|:------:|:------:|:------:|
| 存储限制  | 256TB | RAM | 64TB | None |
| 支持事物  | No | No | Yes | No |
| 支持全文索引 | Yes | No | No | No |
| 支持数索引 | Yes | Yes | Yes | No |
| 支持哈希索引 | No | Yes | No | No |
| 支持数据缓存 | No | N/A | Yes | No |
| 支持外键 | No | No | Yes | No |


- 如果要提供提交、回滚、崩溃恢复能力的事物安全（ACID兼容）能力，并要求实现并发控制，选择InnoDB；
- 如果数据表主要用来插入和查询记录，则MyISAM引擎能提供较高的处理效率；
- 如果只是临时存放数据，数据量不大，并且不需要较高的数据安全性，可以选择将数据保存在内存中的Memory引擎，MySQL中使用该引擎作为临时表，存放查询的中间结果；
- 如果只有INSERT和SELECT操作，可以选择Archive，Archive支持高并发的插入操作，但是本身不是事务安全的。Archive非常适合存储归档数据，如记录日志信息可以使用Archive
- 使用哪一种引擎需要灵活选择，一个数据库中多个表可以使用不同引擎以满足各种性能和实际需求，使用合适的存储引擎，将会提高整个数据库的性能

## 常用命令

存储引擎查看

> SHOW ENGINES

查看数据库默认引擎

> SHOW VARIABLES LIKE 'database_name'

