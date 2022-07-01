---
title: C-05 Control Flow
date: 2020-10-12 14:45:03
categories: [Tutorial, C]
tags: Programming Language
---

## 二分搜索（bsearch）

C 标准库实现了二分搜索的函数 bsearch

```c
void *bsearch(const void *key, const void *ptr,
              size_t count, size_t size,
              int (*comp)(const void *, const void *));
```

### 参数

| 参数名 |                                                  说明                                                  |
| :----: | :----------------------------------------------------------------------------------------------------: |
|  key   |                                         指向要查找的元素的指针                                         |
|  ptr   |                                         指向要检验的数组的指针                                         |
| count  |                                             数组的元素数目                                             |
|  size  |                                          数组每个元素的字节数                                          |
|  comp  | 比较函数，升序返回负数，降序返回正数，相等返回零，函数签名形式 `int cmp(const void *a, const void *b)` |

### 返回值

|           返回值           |             说明             |
| :------------------------: | :--------------------------: |
|        null pointer        | 在给定的数组中没有找到该元素 |
| 指向与\*key 比较相等的指针 |   在给定的数组中找到该元素   |

### 二分搜索示例

```c
#include <stdio.h>
#include <stdlib.h>

// 定义带有可以比较元素的结构体
typedef struct data
{
    int nr;
    char const *value;
} Data;
// 定义比较函数
int data_cmp(void const *lhs, void const *rhs)
{
    Data const *const l = lhs;
    Data const *const r = rhs;
    return (l->nr > r->nr) - (l->nr < r->nr);
}

int main(void)
{
    // 准备数据
    Data pairs[] = {
        {1, "Foo"},
        {2, "Bar"},
        {3, "Hello"},
        {4, "World"},
        {5, "Jacky"},
        {6, "John"}};

    Data key = {.nr = 3};
    // 调用<stdlib.h>中的bsearch函数，进行二分搜索查找元素
    Data const *res = bsearch(&key, pairs, sizeof pairs / sizeof pairs[0],
                              sizeof pairs[0], data_cmp);
    if (res)
    {
        printf("NO %d: %s\n", res->nr, res->value);
    }
    else
    {
        printf("NO %d not found\n", key.nr);
    }

    return EXIT_SUCCESS;
}
```

编译后执行结果：

> clang main.c -std=c11 -Wall -o main.out && ./main.out  
> NO 3: Hello

## 快速排序（qsort）

C 标准库实现了快速排序的函数 qsort

```c
void qsort(void *ptr, size_t count, size_t size,
           int (*comp)(const void *, const void *));
```

### 参数

| 参数名 |                                                  说明                                                  |
| :----: | :----------------------------------------------------------------------------------------------------: |
|  ptr   |                                         指向待排序的数组的指针                                         |
| count  |                                             数组的元素数目                                             |
|  size  |                                          数组每个元素的字节数                                          |
|  comp  | 比较函数，升序返回负数，降序返回正数，相等返回零，函数签名形式 `int cmp(const void *a, const void *b)` |

### 快速排序示例

```c
#include <stdio.h>
#include <stdlib.h>
#include <limits.h>

int compare_ints(const void *a, const void *b)
{
    int arg1 = *(const int *)a;
    int arg2 = *(const int *)b;
    return (arg1 > arg2) - (arg1 < arg2);
}

int main(void)
{
    // 准备数据
    int ints[] = { -2,
                   99,
                   INT_MAX,
                   0,
                   -743,
                   10,
                   2,
                   44,
                   INT_MIN,
                   2 };

    int size = sizeof ints / sizeof *ints;
    printf("Unordered: ");
    for (int i = 0; i < size; i++)
    {
        if(i == size - 1)
        {
            printf("%d\n", ints[i]);
        }
        else
        {
            printf("%d ", ints[i]);
        }
    }

    // 调用<stdlib.h>的快排算法，对数组进行排序
    qsort(ints, size, sizeof(int), compare_ints);
    printf("Ordered: ");
    for (int i = 0; i < size; i++)
    {
        if(i == size - 1)
        {
            printf("%d\n", ints[i]);
        }
        else
        {
            printf("%d ", ints[i]);
        }
    }
    return EXIT_SUCCESS;
}
```

编译后执行结果：

> clang main.c -std=c11 -Wall -o main.out && ./main.out  
> Unordered: -2 99 2147483647 0 -743 10 2 44 -2147483648 2  
> Ordered: -2147483648 -743 -2 0 2 2 10 44 99 2147483647
