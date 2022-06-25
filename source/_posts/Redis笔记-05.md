---
title: Redis笔记-05
date: 2022-01-18 01:47:14
categories: [笔记, Redis]
---

## Redis中的布隆过滤器（BloomFilter）

`Redis`官方在`4.0`版本中提供了插件功能，此时可以将`rebloom`作为插件加载到Redis Server中，给Redis提供布隆去重功能。

```docker
docker pull redislab/rebloom
docker run -p 6739:6739 redislab/rebloom
redis-cli
```

```
# 插入
bf.add key value
# 多个插入
bt.madd key value1 value2 value3 ...
# 查询单个key是否存在
bf.exists key value
# 查询多个key是否存在
bf.mexists key value1 value2 ...

# 自定义参数过滤器
bf.reserve key error_rate initial_size

# error_rate 容错率 默认值为0.01 越低需要的存储空间越大 
# initial_size 预计放入元素数量 默认值为100 当放入的实际元素数量超过该数量，会出现误判，超过数量越大，误判越严重 
```