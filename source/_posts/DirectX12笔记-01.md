---
title: DirectX12笔记-01 基础知识
date: 2020-03-27 21:31:07
categories: [笔记, DirectX]
tags: CG
math: true
---

## 系列介绍

最近在阅读[《DirectX 12 3D 游戏开发实战》](https://book.douban.com/subject/30426701/)这本书，也就是[《Introduction to 3D Game Programming with DirectX 12》](https://book.douban.com/subject/26628210/)的国内翻译版本，然后决定写一个 DirectX 知识点系列。该系列主要讲基于 DirectX12 制作 3D 游戏，内容大概是从基础的 3D 数学讲起,然后递进直到做出一个游戏 demo，并运用上书籍上所有知识点。

## 正文

该书前三章主要介绍了 3d 数学的基础，向量、矩阵和变换三个要点以及在 3d 运算中扮演的角色。

### 向量

向量（[vector](https://en.wikipedia.org/wiki/Euclidean_vector)）就是一种带有大小（模，[magnitude](<https://en.wikipedia.org/wiki/Magnitude_(mathematics)>)）和方向（direction）的量。如，物理上常见的力。

### 矩阵

矩阵（[matrix](<https://en.wikipedia.org/wiki/Matrix_(mathematics)>)），一个 m×n 的矩阵是由 m 行 n 列的实数所构建的矩形阵列。右图便是一个矩阵： $\left[
    \begin{matrix}
    1 & 2 & 3 \\
    4 & 5 & 6 \\
    7 & 8 & 9
    \end{matrix}
\right]$  
矩阵的几何意义就是将某个数据从 A 坐标系转换到 B 坐标系，在 3d 中应用便是将向量在不同的空间转换。

### 线性变换

线性变换（linear transformation）：数学上如果函数$\tau(\nu) = \tau(x, y, z) = \tau(x', y', z')$，此函数的输入和输出都是 3D 向量，当且仅当此函数满足

$$
\begin{cases}
\tau(u + v) = \tau(u) + \tau(v) \\
\tau(ku) = k\tau(u)
\end{cases}
$$

则称$\tau$为 v 的线性变换。其中$u = (u_x, u_y, u_z)$和$v = (v_x, v_y, v_z)$是任意的 3D 向量，k 为标量。

线性变换主要靠矩阵来实现，如`ObjectToWorldMatrix`便是将对象从对象空间转换到世界空间。DirectX12 中矩阵`XMMATRIX`使用齐次坐标，即 4x4。[为什么不使用 3x3 的矩阵呢？](https://trickyrat.github.io/2020/04/06/DirectX12笔记-03//)

## 参考文献

- [Introduction to 3D Game Programming with DirectX 12, Frank D.Luna](https://book.douban.com/subject/26628210/)
