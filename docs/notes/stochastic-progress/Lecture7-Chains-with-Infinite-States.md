---
title: Chains with Infinite States
tags: [stochastic-progress]
math: true
modified: 星期日, 三月 16日 2025, 8:30:23 晚上
---

当 Markov Chain 有无限个状态时，stationary distribution 就不一定存在了，例如在分布 $\pi$ 中，满足 $\pi(i+1) = \frac{p}{1-p}\pi(i), i=0,1,\ldots$，则当 $p \ge 1/2$ 时，该分布就没有 stationary distribution。

![](https://notes.sjtu.edu.cn/uploads/upload_621b255a241512efb9b6cea8c35dd1f8.png)

## Recurrence（常返）

对于一个 Markov chain $X_0, X_1, \ldots$，定义:

- $T_i = \min\{ t > 0 \mid X_t = i \}$: 第一次到达状态 $i$ 的时间
- $N_i = \sum_{t=1}^\infty1[X_t = i]$: 到达状态 $i$ 的总次数
- $P_i[\cdot] = \mathrm{Pr}[\cdot | X_0=i]$
- $E_i[\cdot] = \mathrm{E}[\cdot | X_0=i]$
- Recurrent: 一个状态 $i$ 如果满足 $P_i[T_i < \infty] = 1$，那么就称为 recurrent 的，否则称为 transient 的。即从该状态出发还能回到该状态。

### 命题 1

如果状态 $i$ 是常返的，并且 $i \rightarrow j$ 有一条有限长的路径，那么：

- $P_i[T_j < \infty] = 1$
- $P_j[T_i < \infty] = 1$
- $j$ 也是常返的

由该命题可知，常返性是一种类层面上的性质：一个状态为常返的，则其有限步内能达到的状态均为常返的。

### 命题 2

以下三个条件等价于 $i$ 是常返的：

- $P_i[T_i < \infty] = 1$
- $P_i[N_i = \infty] = 1$
- $E_i[N_i] = \infty$
