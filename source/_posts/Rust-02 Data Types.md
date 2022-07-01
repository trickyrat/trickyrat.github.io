---
title: Rust-02 Data Types
date: 2020-10-23 13:59:34
categories: [Tutorial, Rust]
tags: Programming Language
---

## Integer Number

| 长度    | 有符号 | 无符号 |
| :------ | :----- | :----- |
| 8-bit   | i8     | u8     |
| 16-bit  | i16    | u16    |
| 32-bit  | i32    | u32    |
| 64-bit  | i64    | u64    |
| 128-bit | i128   | u128   |
| arch    | isize  | usize  |

最后的 isize 和 usize 会自动基于当前运行平台如果是 64 位系统就是 i64 和 u64，32 位系统则是 i32 和 u32。Rust 默认整型为 i32。

```rust
fn main() {
    let x = 2; // i32
    let y: i64 = 3; // i64
}
```

## Integer Literal

| 字面值        | 例子        |
| :------------ | :---------- |
| Decimal       | 100_000     |
| Hex           | 0xff        |
| Octal         | 0o77        |
| Binary        | 0b1111_0000 |
| Byte(u8 only) | b'A'        |

## Float Point Number

| 长度   | 类型名 |
| :----- | :----- |
| 32-bit | f32    |
| 64-bit | f64    |

Rust 默认浮点数类型为 f64。

```rust
fn main() {
    let x = 2.0; // f64
    let y: f32 = 3.0; // f32
    // 支持的操作
    let sum = 5.3 + 10.7;
    let difference = 95.5 - 5.5;
    let product = 5.13 * 2;
    let quotient = 152.4 / 22.2;
    let remainder = 25 % 4;
}
```

## Boolean

Rust 的 Boolean 类型大小为 1 byte

| 名称 | 类型名 |
| :--- | :----- |
| 真   | true   |
| 假   | false  |

```rust
fn main() {
    let t = true;
    let f: bool = false; // 使用类型名显示声明
}
```

## Character

Rust 的字符是一个 4 bytes 的 Unicode Scalar Value

```rust
fn main() {
    let c = 'z';
    let dog = '🐶';
}
```

## Tuple

Rust 的元组长度固定，一旦声明后大小不能增长和收缩。

```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
    let (x, y, z) = tup;
    println!("The value of y is {}", y);
    let five_hundred = tup.0;
    let six_point_four = tup.1;
    let one = tup.2;
}
```

## Array

固定长度

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
    let b: [i32; 5] = [1, 2, 3, 4, 5];

    let array = [1, 2, 3, 4, 5];
    let arr: [i32; 5] = [1, 2, 3, 4, 5];
    let elememt = array[1];
    let index = 3;
    let element2 = arr[index];   // 访问数组元素 数组索引是0开头
}
```

Rust 中声明变量的方式是`let [mut] variablename : typename = initialvalue;`
