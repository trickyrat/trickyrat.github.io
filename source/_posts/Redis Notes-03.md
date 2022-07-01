---
title: Redis Notes-03
date: 2021-12-01 15:58:48
categories: [Notes, Redis]
tags: Notes
---

## Redis 事务

`Redis` 的命令是原子性的，而 `Redis` 的事务是非原子性的

### MULTI 命令

开启事务命令，`Redis`将操作命令逐个放到队列中，根据`EXEC`命令来原子化执行命令

### EXEC 命令

执行事务中的所有操作命令

### DISCARD 命令

取消事务命令，放弃执行事务模块中的所有命令

### WATCH 命令

监视一个 `key` 或者多个 `key` 如果在事务执行前，这些 `key` 被其他命令修改，则事务被中断，不会执行事务中的任何命令

### UNWATCH 命令

取消 `WATCH` 命令对所有 `key` 的监控

### 事务流程

#### 正常事务流程

使用`MULTI` 命令开启一个事务，并且使用 `set` 命令新建两个字符串键 `key1` 和 `key2`，初始值分别为 `val1` 和 `val2`

```redis
127.0.0.1:6379> set key1 val1
OK
127.0.0.1:6379> set key2 val2
OK
```

然后我们使用一个事务来修改两个 key 的值，命令为

```redis
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> set key1 new_val1
QUEUED
127.0.0.1:6379> set key2 new_val2
QUEUED
127.0.0.1:6379> EXEC
1) OK
2) OK
```

最后我们使用`get`命令来查询我们修改的键

```redis
127.0.0.1:6379> get key1
"new_val1"
127.0.0.1:6379> get key2
"new_val2"
127.0.0.1:6379>
```

可以看到外面已经修改了两个键的值

#### 语法错误导致事务失败流程

在事务开启后，其中某一条命令因为 redis 语法错误导致整个事务提交失败，key 的值不会发生改变

```redis
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379(TX)> set key1 new_val1
QUEUED
127.0.0.1:6379(TX)> sets key2 new_val2
(error) ERR unknown command `sets`, with args beginning with: `key2`, `new_val2`,
127.0.0.1:6379(TX)> EXEC
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get key1
"val1"
127.0.0.1:6379> get key2
"val2"
```

#### 数据类型错误导致事务失败流程

在事务开启后，其中某一条命令因为 redis 数据类型错误导致整个事务提交失败，发生类型错误的命令的 key 的值不会发生改变，但是其他 key 会发生变化

```redis
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379(TX)> set key1 val1
QUEUED
127.0.0.1:6379(TX)> lpush key2 val2
QUEUED
127.0.0.1:6379(TX)> EXEC
1) OK
2) (error) WRONGTYPE Operation against a key holding the wrong kind of value
127.0.0.1:6379> get key1
"val1"
127.0.0.1:6379> get key2
"new_val2"
127.0.0.1:6379>
```
