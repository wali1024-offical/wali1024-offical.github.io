---
title: Fundamental Theorem of Markov Chains (General Version)
tags:
  - stochastic-progress
math: true
---

**内容**：Positive Recurrence + Aperiodic + Irreducible -> Stationary distribution + Unique + Convergence

## 大数定律

**强大数定律** (Strong law of large numbers (SLLN) or Kolmogorov’s law)：对于独立同分布的随机变量 $X_1, X_2, \ldots$，满足 $E[X_1] = E[X_2] = \cdots = \mu < \infty$，记 $\bar{X}_n = \frac{1}{n}\sum_{i=1}^n X_i$，则有 $\bar{X}_n$ 几乎或概率1的逼近 $\mu$：

$$
\bar{X}_n\stackrel{\mathrm{a.s.}}{\longrightarrow}\mu \space \mathrm{when\ } n\to \infty.
$$

即

$$
\bar{X}_n\stackrel{\mathrm{a.s.}}{\longrightarrow}\mu
$$

对于概率空间 $(\Omega, \mathcal{F}, P)$， $\bar{X}_n\stackrel{\mathrm{a.s.}}{\longrightarrow}\mu$ 表示存在 $M \in \mathcal{F}$，满足

- $P(M)=1$
- $\forall \omega \in M, \bar{X}_n(\omega)\stackrel{n \rightarrow \infty}{\longrightarrow}\mu$

## Strong law of large numbers for Markov chains

## 内容

如果状态 $i,j$ 之间存在一条有限长的路径，那么

$$
P_i \left[ \lim_{n\to\infty} \frac{1}{n}\sum_{t=1}^n 1[X_t=j]=\frac{1}{E_j[T_j]}\right]=1.
$$

这里的 $P_i$ 定义在 Lecture7 中

## 证明

## 推论

对于任意一个 irreducible 的 Markov chain，转移矩阵 $P$，则对任何两个状态 $i,j$

$$
\lim_{n\to\infty}\frac{1}{n}\sum_{t=1}^n P^t(i,j) =\frac{1}{E_j[T_j]}.
$$