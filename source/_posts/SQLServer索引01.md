---
title: SQLServer索引01
date: 2021-12-08 23:46:28
categories: [数据库, SQLServer]
---

## 不能创建索引的字段类型

无法指定 ntext、 text、 image、 varchar(max) 、 nvarchar(max) 和 varbinary(max) 数据类型的列为索引键列  
如果索引包含多个列，则应考虑列的顺序。 用于等于 (=)、大于 (>)、小于 (<) 或 BETWEEN 搜索条件的 WHERE 子句或者参与联接的列应该放在最前面。 其他列应该基于其非重复级别进行排序，就是说，从最不重复的列到最重复的列
