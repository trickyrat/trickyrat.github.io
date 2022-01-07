---
title: Redis笔记-01
date: 2020-09-19 16:32:19
categories: [笔记, Redis]
tags: 数据库
---

# Redis 命令

## 创建和获取 key

| 命令 |                     说明                      |        例子        |
| :--: | :-------------------------------------------: | :----------------: |
| set  |      创建一个名为 key 值为 value 键值对       |    set views 10    |
| get  | 获取名为 key 的值，存在返回值，不存在返回 nil |     get views      |
| mset |              一次创建多个键值对               | mset key1 1 key2 2 |
| mget |              一次获取多个键的值               |   mget key1 key2   |

```bash
local_host:0>set views 10
"OK"
local_host:0>get views
"10"
local_host:0>mset key1 1 key2 2 key3 3
"OK"
local_host:0>mget key1 key2 key3
 1)  "1"
 2)  "2"
 3)  "3"
```

## 自增自减

| 命令 |       说明        |    例子    |
| :--: | :---------------: | :--------: |
| incr | 自增 key 的 value | incr views |
| decr | 自减 key 的 value | decr views |

```bash
local_host:0>incr views
"11"
local_host:0>decr views
"10"
```

## 带步进自增自减

|  命令  |          说明           |      例子      |
| :----: | :---------------------: | :------------: |
| incrby | 有步进增加 key 的 value | incrby views 2 |
| decrby | 有步进减少 key 的 value | decrby views 2 |

```bash
local_host:0>incrby views 2
"12"
local_host:0>decrby views 2
"10"
```

## 判断一个 key 是否存在

|  命令  |         说明          |     例子     |
| :----: | :-------------------: | :----------: |
| exists | 判断一个 key 是否存在 | exists views |

```bash
local_host:0>exists views
"1"
local_host:0>exists key4
"0"
```

## 设置 key 过期时间（支持秒和毫秒）

|  命令  |       说明        |     例子      |
| :----: | :---------------: | :-----------: |
| expire | 设置 key 过期时间 | expire key2 5 |

```bash
local_host:0>expire key2 5
"1"
local_host:0>get key2
"2"
# after 5 seconds
local_host:0>get key2
null
```

## 删除某个 key

| 命令 |     说明     |   例子   |
| :--: | :----------: | :------: |
| del  | 删除某个 key | del key3 |

```bash
local_host:0>del key3
"1"
```

## 查询 value 的类型

| 命令 |         说明          |    例子    |
| :--: | :-------------------: | :--------: |
| type | 查看 key 的存储的类型 | type views |

```bash
local_host:0>type views
"string"
```

## 模式匹配查询 key

| 命令 |         说明         |  例子   |
| :--: | :------------------: | :-----: |
| keys | 根据模式匹配查询 key | keys \* |

```bash
local_host:0>keys *
 1)  "key3"
 2)  "views"
 3)  "key2"
 4)  "key1"
```
