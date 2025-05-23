---
title: Martingale
tags:
  - stochastic-progress
math: true
---

对于 Chernoff Bound 和 Hoeffding's Inequality，可以使用鞅（maringale）来去除随机变量互相独立的条件

## 定义

### Maringale

记 $\{ X_n\}_{n \ge 0}$ 和 $\{ Z_n\}_{n \ge 0}$ 为两列随机变量，且 $Z_n$ 只与 $X_1, X_2, \ldots, X_n$ 有关，则称 $\{ Z_n\}_{n \ge 0}$ 为关于 $\{ X_n\}_{n \ge 0}$ 的**鞅**，如果有

$$
E[Z_{n+1} | X_0, X_1, \ldots, X_n] = Z_n
$$

即

$$
\forall (x_0, x_1, \ldots, x_n), E[Z_{n+1}| X_0 = x_0, \ldots, X_n = x_n] = Z_n[X_0 = x_0, \ldots, X_n = x_n]
$$

对于单独一列随机变量 $\{ X_n\}_{n \ge 0}$，如果有

$$
E[X_{n+1}| X_0, X_1, \ldots, X_n] = X_n
$$

那么也称 $\{ X_n\}_{n \ge 0}$ 为一个关于自身的鞅。

### Maringale with filtration

更广义的，通过 **过滤（filtration）** 定义的鞅：

记 $\{ \mathcal{F}_n\}_{n \ge 0}$ 为一列 $\sigma$ 代数，称其为一个 filtration，如果

$$
\mathcal{F}_0 \subseteq \mathcal{F}_1 \subseteq \cdots \subseteq \mathcal{F}_n \subseteq \cdots
$$

给定一个 filtration $\{ \mathcal{F}_n\}_{n \ge 0}$，如果随机变量序列 $\{ Z_n\}_{n \ge 0}$ 满足 $Z_n$ 是 $\mathcal{F}_n$ measurable 的，且

$$
E[Z_{n+1} \mid \mathcal{F}_n] = Z_n
$$

那么称 $\{ Z_n\}_{ n \ge 0}$ 为一个关于 $\{ \mathcal{F}_n\}_{n \ge 0}$ 的鞅。

## 上鞅、下鞅

如果

$$
E[Z_{n+1} | X_0, X_1, \ldots, X_n] < Z_n
$$

则称 $\{ Z_n\}_{n \ge 0}$ 为上鞅

如果

$$
E[Z_{n+1} | X_0, X_1, \ldots, X_n] > Z_n
$$

则称 $\{ Z_n\}_{n \ge 0}$ 为下鞅
