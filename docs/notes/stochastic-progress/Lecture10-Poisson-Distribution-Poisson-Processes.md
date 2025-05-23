---
title: Poisson Distribution, Poisson Processes
tags:
  - stochastic-progress
math: true
---

## 定义

**Poissen Distribtion**:

对 $k \in \mathbb{Z}$，定义泊松分布 $X \sim \mathrm{Pois}(\lambda)$

$$
\mathrm{Pr}[X=k] = \begin{cases}
    e^{-\lambda}\frac{\lambda^k}{k!}, &k \ge 0 \\
    0, & k < 0
\end{cases}
$$

容易得到，

$$
E[X] = Var[X] = \lambda
$$

**Poissen Process**:

定义 Poissen Process $\{N(s): s \ge 0\}$ with rate $\lambda$：

1. $N(0) = 0$
2. $\forall t,s \ge 0, N(t+s) - N(s) \sim \mathrm{Pois}(\lambda t)$
3. $\forall t_0 \le t_1 \le \cdots \le t_n,\space N(t_1) - N(t_0), N(t_2) - N(t_1), \ldots, N(t_n) - N(t_{n-1})$ 互相独立

**Exponential distribution with rate $\lambda$**:

$$
f(x) = \begin{cases}
    \lambda e^{-\lambda x} &x \ge 0\\
    0 & x < 0\\
\end{cases} 
$$

- CDF: $F(x) = 1 - e^{-\lambda x}$
- $E[X] = \frac{1}{\lambda}, Var[X] = \frac{1}{\lambda ^2}$

## 命题

### 命题1

对 $X_1 \sim \mathrm{Pois}(\lambda_1), \space X_2 \sim \mathrm{Pois}(\lambda_2)$，有 $X_1 + X_2 \sim \mathrm{Pois}(\lambda_1 + \lambda_2)$

### 命题2

Suppose that $\tau_1, \tau_2, \ldots, \tau_n$ is a sequence of independent random variable that $\tau _i \sim \mathrm{Exp}(\lambda)$. Let $T_n = \sum_{i=1}^{n}\tau _i$. For $s \ge 0$, let $N(s) = max\{ n\mid T_n \le s\}$, then $N(s)$ is a Poisson process with rate $\lambda$

### 命题3

对 $X \sim \mathrm{Exp}(\lambda)$，则对任意 $t,s \ge 0$

$$
\mathrm{Pr}[X > t + s | X > s] = \mathrm{Pr}[X > t]
$$

### 命题4

对 $X_1 \sim \mathrm{Exp}(\lambda_1), \space X_2 \sim \mathrm{Exp}(\lambda_2)$，有 $\min(X_1, X_2) \sim \mathrm{Exp}(\lambda_1 + \lambda_2)$

## Thinning

对一段时间内“到达”的泊松随机过程，可以做分割（举例：排队、零件分级等），分为若干子泊松随机过程，则这些过程相互独立。