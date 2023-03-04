---
title: Rust-04 Ownership
categories:
  - Tutorial
  - Rust
tags: Programming Language
date: 2023-01-13 16:25:44
---


|    Description    |    Sample Code    |
| :---------------: | :---------------: |
| mutable reference | &mut varible_name |
|     reference     |   &varible_name   |

Rust中会对变量增加一个所有权的概念，即，变量被声明时，所分配的内存（包括栈（stack）上分配和堆（heap）上分配的变量）归当前变量所有。一旦变量超出其作用域后便会将分配的内存释放掉。

```rust
let a = 5;
let b = a; // a shoule be moved, but it implemented the Copy triat


let s1 = String::from("hello");
let s2 = s1; // s1 will be moved
```