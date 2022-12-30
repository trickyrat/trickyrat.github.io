---
title: Rust-06 HashMap
categories:
  - Tutorial
  - Rust
tags: Programming Language
date: 2022-12-30 17:32:00
---


## HashMap

### std::collections::hash_map::Entry

访问HashMap的元素，提供有`or_default`方法来获取没有存在的key，并对其设置默认值。也可以使用`or_insert`对没有存在的key进行值的设置

```rust
use std::collections::HashMap;

let mut map: HashMap<&str, u32> = HashMap::new();

map.entry("poneyland").or_insert(3);
assert_eq!(map["poneyland"], 3);

*map.entry("poneyland").or_insert(10) *= 2;
assert_eq!(map["poneyland"], 6);
```