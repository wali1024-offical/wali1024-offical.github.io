---
title: " Itô Integral, Itô Formula"
tags:
  - stochastic-progress
math: true
---
## Itô Integral

可以将 diffusion 公式化为

$$
\begin{equation}
\mathrm{d}X_t = \mu(t, X_t)\mathrm{d}t + \sigma(t, X_t)\mathrm{d}B_t
\end{equation}
$$

这里 $\{ B_t\}_{t \ge 0}$ 为 SBM

对式(1)积分得

$$
\begin{equation}
\forall T, X_T = X_0 + \int_{0}^{T}\mu(t, X_t)\mathrm{d}t + \int_{0}^{T}\sigma (t, X_t)\mathrm{d}B_t
\end{equation}
$$

$$
\begin{equation*}
\forall T, \forall \omega \in \Omega,  X_T(\omega) = X_0(\omega) + \int_{0}^{T}\mu(t, X_t(\omega))\mathrm{d}t + \int_{0}^{T}\sigma (t, X_t(\omega))\mathrm{d}B_t
\end{equation*}
$$

- $\int_{0}^{T}\mu(t, X_t)\mathrm{d}t$ 为一个常见的黎曼积分
- $\int_{0}^{T}\sigma (t, X_t(\omega))\mathrm{d}B_t$ 为一个 Itô Integral

### Riemann-Stieltjes Integral

记 $F: [0, T] \rightarrow \mathbb{R}$ 是一个导函数有界的函数，区间 $[0, T]$ 的一个划分为：$0 = t_0 \le t_1 \le \cdots \le t_n = T$，定义

$$
\begin{align*}
    \int_{0}^{T}X\mathrm{d}F &= \lim_{n \rightarrow \infty} \sum_{i=1}^n X(t_i^*)(F(t_i) - F(t_{i-1})) \\ 
    &= \lim_{n \rightarrow \infty} \sum_{i=1}^{n} X(t_i^*)F'(t_{i-1})(t_i - t_{i-1}) \\
    &= \int_{0}^{T}X(t)F'(X(t))\mathrm{d}t
\end{align*} 
$$

常用于计算期望：$E[X] = \int_{\Omega}X\mathrm{d}F$

关于 $\int_{0}^{T}F(x)\mathrm{d}F(x)$ 容易得到：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202408281712732.png)

### Itô Integral

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202408281717561.png)

这里 $\int_{0}^{T}B_t\mathrm{d}B_t = \frac{1}{2}B_T^2 - \frac{1}{2}T$ 是因为 $\lim_{\Delta \rightarrow 0}\frac{1}{2}\sum_{i=1}^{n}(B(t_i) - B(t_{i-1}))^2$ 不一定收敛了，因此需要引入 mean square convergence 的概念来处理，记 $Q_n = \sum_{i=1}^{n}(B(t_i) - B(t_{i-1}))^2$，可以算出 $E[Q_n] = T, \operatorname{Var}[Q_n] \rightarrow 0$，故最终出现了 $-\frac{1}{2}T$ 这一项。

## Itô Formula

由于对任意 $T$

$$
\lim_{n \rightarrow \infty}Q_n = \int_{0}^{T}(\mathrm{d}B_t)^2 = T = \int_{0}^{T}\mathrm{d}t
$$

可推断出 $(\mathrm{d}B_t)^2 \approx \mathrm{d}t$

故对于式(1)，以及函数 $f$，有

$$
\begin{align*}
    \mathrm{d}f(X_t) &= f(X_t + \mathrm{d}X_t) - f(X_t) \\
    &= f'(X_t)\mathrm{d}X_t + \frac{1}{2}f''(X_t)(\mathrm{d}X_t)^2 + o((\mathrm{d}X_t)^2) \\
    &= f'(X_t)(\mu_t \mathrm{d}t + \sigma_t\mathrm{d}B_t) + \frac{1}{2}f''(X_t)(\mu_t \mathrm{d}t + \sigma_t\mathrm{d}B_t)^2 + o((\mathrm{d}X_t)^2) \\
    & \stackrel{\mathrm{d}t \rightarrow 0}{\longrightarrow} \left(f'(X_t)\mu + \frac{1}{2}f''(X_t) \sigma _t^2\right)\mathrm{d}t + f'(X_t)\sigma _t\mathrm{d}B_t
\end{align*} 
$$

这就是 Itô Formula。

可以用这个公式计算：

- $\mathrm{d}(B_t^2) = 2 B_t \mathrm{d}B_t + \mathrm{d}t$
- **Geometric Brownian Motion**: 对 $Y_t = e^{B_t}$，有 $\mathrm{d}Y_t = Y_t \mathrm{d}B_t + \frac{1}{2}Y_t \mathrm{d}t$
- **Ornstein-Uhlenbeck Process**: 对 $f(t, X_t) = e^t X_t$，$\{ X_t\}$ 为 Ornstein-Uhlenbeck process 且 $X_0 = 0$，有 $\mathrm{d}f(t, X_t) = 2e^t\mathrm{d}B_t$