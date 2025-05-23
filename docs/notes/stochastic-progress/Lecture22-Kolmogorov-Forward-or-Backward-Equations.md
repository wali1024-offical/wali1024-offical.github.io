---
title: Lecture22 Kolmogorov Forward or Backward Equations
tags:
  - stochastic-progress
math: true
---
## Kolmogorov Backward Equation

对马尔可夫半群 $\{ Q_t\}_{t\ge0}$ 和生成元 $\mathcal{L}$，有

$$
\frac{\mathrm{d}}{\mathrm{d}t} Q_t f = \mathcal{L}Q_t f
$$

定义 $f_t = Q_t f$，则有 $\frac{\mathrm{d}}{\mathrm{d}t} f_t = \mathcal{L}f_t$

## Kolmogorov Forward Equation

算子的共轭（conjugate）：对算子 $P$，其共轭 $p^*$ 满足 $\langle Px, y\rangle = \langle x, P^*y\rangle$

对于连续马尔可夫链，$\mathbf{E}[f(X_t)]$ 有两种计算方式：站在时间 $t$ 的角度，或者站在时间 $t=0$ 的角度，因而有 $\mathbf{E}[f(X_t)] = \langle f, p_t \rangle = \langle Q_tf, p_0 \rangle = \langle f, Q_t^*p_0 \rangle$，进而 $p_t = Q_t^*p_0$。所以 $Q_t$ 的共轭和离散情况下 $P^t$ 作用相同。

同理可得，$\frac{\mathrm{d}}{\mathrm{d}t}\mathbf{E}[f(X_t)] = \langle \mathcal{L}f, p_t \rangle = \frac{\mathrm{d}}{\mathrm{d}t}\langle f, p_t \rangle$，即 $\langle f, \mathcal{L}^*p_t \rangle = \langle f,\frac{\mathrm{d}}{\mathrm{d}t}p_t \rangle$，所以有

$$
\mathcal{L}^*p_t = \frac{\mathrm{d}}{\mathrm{d}t}p_t
$$

这就是 **Kolmogorov Forward Equation (KFE)**。对应离散情况下的等式为：$\pi_t - \pi_{t-1} = (P - I) \pi_{t-1}$，$\mathcal{L}^*$ 对应 $P - I$

## Distribution Evolution of a Diffusion

对于 diffusion $\mathrm{d}X_t = \mu(X_t)\mathrm{d}t + \sigma (X_t)\mathrm{d}B_t$，可以用 KFE 来计算概率密度分布 $p_t(x)$，也就是找到算子 $\mathcal{L}^*$

由于

$$
\mathcal{L}f(x) = f'(x)\mu(x) + \frac{1}{2}f''(x)\sigma^2 (x)
$$

可以通过计算 $\frac{\mathrm{d}}{\mathrm{d}t}\mathbf{E}[f(X_t)]$ 得到，

$$
\mathcal{L}^*(g) = - \frac{\partial}{\partial x}\mu g + \frac{1}{2} \frac{\partial ^2}{\partial x^2}(\sigma ^2 g)
$$

那么就有

$$
\frac{\partial}{\partial t}p_t(x) = - \frac{\partial}{\partial x}\mu(x) p_t(x) + \frac{1}{2} \frac{\partial ^2}{\partial x^2}(\sigma ^2(x)p_t(x))
$$

## Computing Stationary Distribution

设 $\pi(x)$ 为 stationary distribution，代入上节所得式子，得

$$
\frac{\partial}{\partial x}\mu(x) p_t(x) = \frac{1}{2} \frac{\partial ^2}{\partial x^2}(\sigma ^2(x)p_t(x))
$$

## Evolution of High Dimension Diffusion

对高维情况

$$
\mathrm{d}X_t = \mu(X_t)\mathrm{d}t + \sigma (X_t)\mathrm{d}B_t
$$

这里 $X_t, \mu(X_t), \mathrm{d}B_t \in \mathbb{R}^n, \sigma (X_t) \in \mathbb{R}^{n \times n}$

有：

- **Itô formula**

$$
\mathrm{d}f(X_t) = \langle \nabla f(X_t), \mu(X_t) \rangle\mathrm{d}t + \langle \nabla f(X_t), \sigma (X_t)\mathrm{d}B_t \rangle + \frac{1}{2} \operatorname{tr}(\sigma (X_t)^T \nabla ^2f(X_t) \sigma (X_t)) \mathrm{d}t
$$

- **Multi-dimensional integration by parts formula**

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202408311417544.png)

通过计算可以得到：

$$
\langle \mathcal{L}^*p_t, f \rangle = -\langle \nabla \cdot (\mu \cdot p_t), f \rangle + \langle f, \frac{\partial ^2}{\partial x_i \partial x_j } ([\sigma \sigma^T]_{i,j} \cdot p_t)\rangle
$$

进而得到：

$$
\mathcal{L}^*f(x) = - \sum_{i \in [n]} \frac{\partial }{\partial x_i} (\mu(x) \cdot f(x)) + \frac{1}{2} \sum_{i, j \in [n]} \frac{\partial ^2}{\partial x_i \partial x_j}([\sigma (x)\sigma (x)^T]_{i, j} \cdot f(x))
$$
