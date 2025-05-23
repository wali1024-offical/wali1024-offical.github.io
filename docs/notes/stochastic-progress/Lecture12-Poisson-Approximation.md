---
title: Poisson Approximation
tags: [stochastic-progress]
math: true
modified: 星期日, 三月 16日 2025, 9:25:09 晚上
---

## 问题

### 奖券收集问题的推广

$n$ 种奖券，对应概率 $p_1, \ldots, p_n$, 且 $\sum_{i=1}^{n}p_i = 1$，求收集到所有种类的奖券的时间 $N$ 的期望 $E[N]$。

可以利用泊松过程的分割来转化这个问题：

假设有一个 rate 为 1 的泊松过程，每到达一个，有 $p_j$ 的概率是第 $j$ 种奖券。记 $N_j$ 为第 $j$ 种奖券第一次到达的最少次数，那么 $N = \max _{1 \le j \le n} N_j$。

又记 $X_j(t)$ 为 $[0,t]$ 时间内到达的第 $j$ 种奖券的个数，那么 $\{X_j(t)\}$ 互为独立的泊松过程。再记 $T_j = \min\left\{ t \mid X_j(t) = 1 \right\}$，$T = \max_{j \in [n]}T_j$。

## 定理

### 定理 1

- 若 $X$ 离散且 $X \in \mathbb{N}$，那么 $E[X]=\sum_{t=1}^{\infty}\mathrm{Pr}[X \ge t]$
- 若 $X$ 连续，则 $E[X] = \int_{0}^{\infty}\mathrm{Pr}[X \ge t]\mathrm{dt}$

通过交换求和/积分顺序易证。

### Wald's Equation

记 $X_1, X_2, \ldots$ 为 $n$ 个独立同分布的随机变量，满足 $E[|X_1|] < \infty$，$T$ 为 stopping time，满足 $E[T] < \infty$，那么有：

$$
E\left[\sum_{t=1}^{T}X_t\right] = E[T]E[X_1]
$$

## 结果

由定理 1 可以得到，

$$
\begin{align*}
    E[T] &= \int_{0}^{\infty}\mathrm{Pr}[T \ge t]\mathrm{dt} = \int_{0}^{\infty}1 - \mathrm{Pr}[T < t] \mathrm{dt} \\
    &= \int_{0}^{\infty}1 - \Pi_{j=1}^{n}\mathrm{Pr}[T_j < t] \mathrm{dt} \\
    &= \int_{0}^{\infty}1 - \Pi_{j=1}^{n}\left(1 - e^{-p_jt}\right) \mathrm{dt}
\end{align*} 
$$

记 $\tau _i$ 为第 $i$ 和 $i-1$ **张**奖券到达的时间差，则

$$
E[T] = E\left[\sum_{j=1}^{N}\tau _j\right] = E[N]E[\tau _1]
$$

最终可以得到

$$
E[N] = \int_{0}^{\infty}1 - \Pi_{j=1}^n(1 - e^{-p_jt})\mathrm{dt}
$$

## 总结

对于“投球入盒”的问题，可以尝试使用泊松过程来转换思路。
