---
title: Redis Notes-02
date: 2020-09-22 10:41:38
categories: [Notes, Redis]
tags: Notes
---

# Redis 类型

## Redis List

|  命令  |                    说明                    |        例子        |
| :----: | :----------------------------------------: | :----------------: |
| LPush  |        在 List 头插入一个或多个元素        | LPush mylist hello |
| RPush  |        在 List 尾插入一个或多个元素        | RPush mylist world |
|  LPop  | 获取 List 头的一个元素，并从 List 中一移除 |    LPop mylist     |
|  RPop  | 获取 List 尾的一个元素，并从 List 中一移除 |    RPop mylist     |
| LRange |           获取指定索引范围的元素           | LRange mylist 0 -1 |

```bash
localhost:0>lpush mylist hello
"1"
localhost:0>lpush mylist world my
"3"
localhost:0>rpush mylist honey
"4"
localhost:0>lrange mylist 0 -1
 1)  "my"
 2)  "world"
 3)  "hello"
 4)  "honey"
localhost:0>lpop mylist
"my"
localhost:0>lrange mylist  0 -1
 1)  "world"
 2)  "hello"
 3)  "honey"
localhost:0>rpop mylist
"honey"
localhost:0>lrange mylist 0 -1
 1)  "world"
 2)  "hello"
```
