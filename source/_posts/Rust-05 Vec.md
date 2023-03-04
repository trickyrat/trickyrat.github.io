---
title: Rust-05 Vec
categories:
  - Tutorial
  - Rust
tags: Programming Language
date: 2022-08-07 21:48:10
---

## Vec

Vec is a contiguous growable array type.

```rust
pub struct Vec<T, A = Global>
where
  A: Allocator,
  { ... }
```

## Examples

We can declare a mutable/immutable Vec by `Vec::new()`
```rust
let mut mut_vec = Vec::new();
let vec = Vec::new();
```

We can also declare a vec by `vec!` macro 
```rust
let mut mut_vec = vec![1,2,3];
let vec = vec![1,2,3];
```

## Macro vec!

```rust
macro_rules! vec {
    () => { ... };
    ($elem:expr; $n:expr) => { ... };
    ($($x:expr),+ $(,)?) => { ... };
}
```

There are two forms of this macro `vec!`

- Create a Vec containing a given list of elements:

```rust
let v = vec![1, 2, 3];
assert_eq!(v[0], 1);
assert_eq!(v[1], 2);
assert_eq!(v[2], 3);
```

- Create a Vec from a given element and size:

```rust
let v = vec![1; 3];
assert_eq!(v, [1, 1, 1]);
```