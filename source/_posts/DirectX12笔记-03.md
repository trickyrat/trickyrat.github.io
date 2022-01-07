---
title: DirectX12笔记-03
date: 2020-04-06 16:56:04
categories: [笔记, DirectX]
tags: CG
math: true
---

# 为什么不使用 3x3 矩阵来作为变换矩阵

## 缩放（Scale）

假设有一个向量$u(x,y,z)^T$（一般使用列向量来表示一个向量）。我们将缩放操作定义为

$$
S(x,y,z)=S(s_xx,s_yy,s_zz)
$$

现在需要证明一下缩放操作是一种线性变换。

$$
\begin{aligned}
S(u+v)
& = (s_x(u_x+v_x),s_y(u_y+v_y),s_z(u_z+v_z))        \\
& = (s_xu_x+s_xv_x,s_yu_y+s_yv_y,s_zu_z+s_zv_z)     \\
& = (s_xu_x,s_yu_y,s_zu_z) + (s_xv_x,s_yv_y,s_zv_z) \\
& = S(u)+S(v)
\end{aligned}
$$

$$
\begin{aligned}
S(ku)
& = (s_xku_x,s_yku_y,s_zku_z) \\
& = kS(u)
\end{aligned}
$$

有以上推导可知，缩放变换是一种线性变换。
我们可以将$u$拆分成$u_x(x,0,0)^T$、$u_y(0,y,0)^T$和$u_z(0,0,z)^T$。那么$u=u_x+u_y+u_z$，我们对三个基坐标进行缩放也就是三个分量分别进行放大或缩小，即$u_xs_x$、$u_ys_y$和$u_zs_z$，便可以得到$u=s_xu_x+s_yu_y+s_zu_z$，将三个分量的向量组合成一个矩阵，便可以得到

$$
\left[
    \begin{array}{ccc}
    s_x & 0   & 0 \\
    0   & s_y & 0 \\
    0   & 0   & s_z
    \end{array}
\right] \left[
    \begin{array}{c}
    x \\
    y \\
    z
    \end{array}
\right] = \left[
    \begin{array}{c}
    s_xx \\
    s_yy \\
    s_zz
    \end{array}
\right]
$$

所以缩放变换对应的矩阵是

$$
M_{scale} = \left[
    \begin{array}{ccc}
    s_x & 0   & 0 \\
    0   & s_y & 0 \\
    0   & 0   & s_z
    \end{array}
\right]
$$

其中$s_x$、$s_y$和$s_z$分别对应三个基方向的缩放比例。

## 旋转（Rotation）

同样的我们需要证明旋转是一种线性变换。

## 平移（Translate）

平移操作并不是一种线性变化。这时我们可以引入齐次坐标

$$
M = \left[
    \begin{array}{ccc}
    s_x & 0   & 0   & 0  \\
    0   & s_y & 0   & 0  \\
    0   & 0   & s_z & 0 \\
    t_x & t_y & t_z & 1
    \end{array}
\right]
$$

第四行的参数代表在三个基础轴上的平移分量
