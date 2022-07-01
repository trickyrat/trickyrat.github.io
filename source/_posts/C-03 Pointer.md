---
title: C-03 Pointer
date: 2020-10-08 13:27:37
categories: [Tutorial, C]
tags: Programming Language
---

# 指针（Pointer）

指针可谓是 C 语言的灵魂也是难点，

指针也是一个内置类型

从右向左，可以清晰分辨指针

```c
int *ptr;  // 声明一个指向int类型的指针变量ptr
int i = 0; // 声明一个int类型的变量i并赋值0
ptr = &i;  // 将i的内存地址（非物理内存地址）赋值给ptr
```

## 指针分类

### 指针的指针（Pointer to pointer）

```c
int **pp; // 声明一个指向int*类型的指针变量pp
```

### 函数指针（Function Pointer）

> return_type (\*pointer_name)(param_type1, param_type2,...);
