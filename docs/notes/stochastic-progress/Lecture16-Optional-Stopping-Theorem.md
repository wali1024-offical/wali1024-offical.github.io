---
title: Optional Stopping Theorem (OST)
tags:
  - stochastic-progress
math:
---

## 定理引出

由于已经知道，对鞅 $\{ Z_n\}_{n \ge 0}$ 来说，任意 $t \in \mathbb{N}$，有 $E[Z_t] = E[Z_0]$。那么，如果 $t$ 不再是一个确定的数，而是一个随机变量 $\tau$，是否还有 $E[Z_\tau] = E[Z_0]$？（即将原本单一取值的 $t$ 替换为一个在多个值上有分布的随机变量 $\tau$）

可选时停定理就提供了一些 $E[Z_\tau] = E[Z_0]$ 成立的充分条件。

**Stopping time**:

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202408271538114.png)

也就是说，想要知道 $\tau \le t$ 是否成立，就只需要知道前 $t$ 个时间的信息 $\mathcal{F}_t$ 就可以了，而不需要了解时间 $t$ 之后的信息。

**定理内容**：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202408272145989.png)

## 应用

### Doobs martingale inequality

$\{ X_t \}_{t \ge 0}$ 是一个关于自身的鞅，且对任意 $t \ge 0$ 有 $X_t \ge 0$。则对任意 $n \in \mathbb{N}$：

$$
\mathrm{Pr}\left[\max_{0 \le t\le n} X_t \ge \alpha\right] \le \frac{E[X_0]}{\alpha}
$$

**证明**：

记随机变量 $\tau =\min\{ n, \min_{t \le n}\{ X_t \ge \alpha \} \}$，则 

$$
\mathrm{Pr}\left[\max_{0 \le t\le n} X_t \ge \alpha\right] = \mathrm{Pr}[X_\tau \ge \alpha] \le \frac{E[X_\tau]}{\alpha} = \frac{E[X_0]}{\alpha}
$$

这里 $E[X_\tau] = E[X_0]$ 是因为 $\tau$ 有界，符合 OST 的条件之一。

### Wald’s Equation

如果随机变量 $X_1, X_2, \ldots$ 为非负、i.i.d、有相同分布 $X$ 的，$T$ 是它们的 stopping time，且 $E[X], E[T] < \infty$，那么

$$
E\left[\sum_{t=1}^{T}X_t\right] = E[T]E[X]
$$

**证明**：构造 $Z_t = \sum_{i=1}^{t} (X_i - E[X])$，证明它是一个鞅并且满足 OST 的条件，就有

$$
\begin{align*}
    E[Z_T] &= E\left[\sum_{t=1}^{T} (X_t - E[X])\right] \\
    &= E\left[\sum_{t=1}^{T}X_t\right] - E[T]E[X] \\
    &= E[Z_0] = 0
\end{align*} 
$$

得证。