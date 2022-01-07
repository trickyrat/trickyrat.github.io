---
title: Rust笔记-03 函数
date: 2020-10-26 10:06:51
categories: [笔记, Rust]
tags: 编程语言
---

## Function

Rust 和 C/C++一样必须有一个名称为 main 的函数，作为程序的入口，Rust 的函数使用关键字`fn`声明。Rust 约定使用`snake case`的代码风格命名 function 和 variable 的名称。

例子

```rust
fn main() {
    another_function();
}
fn another_function() {
    println!("Another function.");
}
```
