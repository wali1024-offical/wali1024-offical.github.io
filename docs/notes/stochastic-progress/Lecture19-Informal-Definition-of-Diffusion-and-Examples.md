---
title: Informal Definition of Diffusion and Examples
tags:
  - diffusion-model
  - stochastic-progress
math: true
---

## Informal Definition

A **continuous** stochastic process with **Markov property** is called a diffusion. In other words, a diffusion can be viewed as a **Markov process** in **continuous time** with **continuous sample paths**.

注意，**Markov Process** 和 **Markov chain** 是不同的概念：前者范围更广、包含后者，状态空间和时间步长都可以是连续的；后者只能是离散的。

举例：

- Standard Brownian Motion $\{ W(t) \}_{t \ge 0}$
- $(\mu, \sigma^2)$-Brownian Motion $X_t = \mu t + \sigma W(t)$
- 连续函数 $X_t = e^t$

但泊松过程不是 diffusion，因为它的 sample path 是离散的（jump process）

## 描述一个 Diffusion

最简单的情况：对连续函数 $X_t = f(t)$，可以用 ODE 来描述

$$
\begin{cases}
    \mathrm{d}X_t = f' \mathrm{dt} \\
    X_0 = 1\\
\end{cases} 
$$

复杂一点：对标准布朗运动（SBM）$\{ W_t\}_{t \ge 0}$，可以用随机微分方程（SDE）来描述。由于 $\mathrm{d}W_t = W_{t+\mathrm{dt}} - W_t \sim \mathcal{N}(0, \mathrm{dt})$，故可从 $\mathcal{N}(0, \mathrm{dt})$ 中随机取样 $\mathrm{d}B_t$

$$
\begin{cases}
    \mathrm{d}W_t = \mathrm{d}B_t\\
    W_0 = 0\\
\end{cases} 
$$

而对 $(\mu, \sigma ^2)$-Brownian motion $\{ X_t\}_{t \ge 0}$，对应的 SDE 为

$$
\begin{cases}
    \mathrm{d}X_t = \mu \mathrm{d}t + \sigma \mathrm{d}B_t \\
    X_0 = 0 \\
\end{cases} 
$$

总的来说，可以用如下形式的 SDE 来描述一个 diffusion:

$$
\mathrm{d}X_t = \mu(t, X_t)\mathrm{d}t + \sigma(t, X_t)\mathrm{d}B_t
$$

也就是说，在极短的时间 $\mathrm{d}t$ 中，这个 diffusion 过程按照 $\mathcal{N}(\mu(t, X_t), \sigma ^2(t, X_t))$ 的进行了移动。这个 SDE 被称为 **Itô diffusion**。

## 一些重要的 Diffusion

### Ornstein-Uhlenbeck Process

对应的 SDE:

$$
\mathrm{d}X_t = -X_t \mathrm{d}t + \mathrm{d}B_t
$$

最终会收敛到 Gaussian.

### Wright-Fisher Process

$\mu(x) = 0, \sigma^2(x)=x(1-x), X(0) = \frac{1}{2}$

### Langevin Dynamics

在梯度下降中，有

$$
X_{t+1} - X_t = -\eta \nabla f(X_t)
$$

也就是下述 ODE 的离散化（gradient flow）

$$
\mathrm{d}X_t = - \eta \nabla f(X_t)\mathrm{d}t
$$

可以加上一些白噪声：

$$
\mathrm{d}X_t = - \eta \nabla f(X_t)\mathrm{d}t + \mathrm{d}B_t
$$

可以证明，这个 Markov process 的稳态为 $p(x) \propto e^{-f(x)}$，这也正是 [Energy-Based-Model-(EBM)](../CS236/Energy-Based-Model-(EBM).md) 中的形式，联系着 [Energy-Based-Model-(EBM)](../CS236/Energy-Based-Model-(EBM).md)、[Score-based-Model](../CS236/Score-based-Model.md) 和 [Diffusion-Model](../CS236/Diffusion-Model.md)

基于此，可以证明，如果想要从 $p(x)$ 中取样，只需要知道 $f$ 的导数，然后迭代：

$$
X_{t+1} - X_t = - \eta \nabla f(X_t) + \sqrt{2\eta}B_t
$$

次数足够多之后，就可以用 $X_n$ 来近似 $p(x)$ 了，也就是求得了 energy function / score function。
