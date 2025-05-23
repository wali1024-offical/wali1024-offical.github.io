---
title: Markov Chains with Finite States
tags:
  - stochastic-progress
math: true
---


## Discret Finite MavKov Chain

- **Time-Homogeneous**: 转移概率不随时间改变而改变
- **状态转移矩阵**：$P(i,j) = \mathrm{Pr}[X_{t+1}=j \mid X_t = i]$，$\mu_t = P^T\mu_{t-1}=(P^T)^t\mu_0$

### Stationary Distribution

**定义**：一个分布 $\pi$ 满足 $\pi^T = \pi^T P$，即分布不再随时间变化而变化

**问题**：

1. Existence?
2. Uniqueness?
3. Convergence?（如果存在，那么是否一定收敛）

#### Existence

等价于如下问题：

> [!note] Existence
>    对任意非负矩阵 $P$，其每一行的和均为 $1$，那么必存在非负特征向量 $\pi$，对应特征值为 $1$（，且元素和为 1）。

由 Perron-Frobenius Theorem 可以证明，答案为一定存在。

#### Uniqueness

构造反例：设某一个 Markov Chain 共有两个 state $1, 2$，并且转移矩阵为 $P = I$，显然有无穷多种 stationary distribution。故不一定 unique。

然而，如果一个转移图为 *strongly connected (irreducable)* 的，那么可以证明，是 unique 的。（充分条件，不是必要条件）

#### Convergence

反例同上例构造，只是转移矩阵 $P = 1 - I$，易知当初始分布不为 $(\frac{1}{2}， \frac{1}{2})$ 时是不收敛的。

所以答案为不一定。

- 可以注意到此时不收敛的原因可以理解为某种“周期性”的变化。
- 定义一个转移图为 *aperiodic* 的，如果任何一个状态，它所在的所有有向环的长度的最大公因数为 1。
