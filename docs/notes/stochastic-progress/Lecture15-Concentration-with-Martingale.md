---
title: Concentration with Martingale
tags:
  - stochastic-progress
math: true
---

## Azuma-Hoeffding’s Inequality

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202408271436454.png)

## Doob Martingale

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202408271439871.png)

- 易验证 $\{ Z_n\}_{n \ge 0}$ 是一个鞅：$E\left[Z_i | \overline{X}_{1, i-1}\right] = E\left[E\left[f(\overline{X}_{1,n}) \mid \overline{X}_{1,i} \right] \mid \overline{X}_{1, i-1} \right] = E[f(\overline{X}_{1,n}) \mid \overline{X}_{1, i-1}] = Z_{i-1}$
- $Z_n = f(\overline{X}_{1,n})$
- $Z_0 = E[f(\overline{X}_{1,n})]$

## McDiarmids Inequality

可以将 Azuma-Hoeffding’s Inequality 和 Doob Martingale 联合起来使用，用于有 $n$ 个（不一定独立的）随机变量 $X_1, \ldots, X_n$，以及一个待估计的函数 $f(X_1, \ldots, X_n)$ 的情况：

$$
\begin{align*}
    & \quad \mathrm{Pr}\left[\left|f(\overline{X}_{1,n}) - E[f(\overline{X}_{1,n})]\right| \ge t\right] \\
    &= \mathrm{Pr}\left[|Z_n - Z_0| \ge t\right]\\
    & \le 2 \exp \left(-\frac{2t^2}{\sum_{i=1}^{n}c_i^2}\right)
\end{align*} 
$$

这里 $c_i$ 与 $\left|Z_i - Z_{i-1}\right|$ 有关

如果 $f$ 和 $X_i$ 再满足一些特殊性质，就更好了。例如如果 $f$ 为 c-Lipschitz function，即

$$
\forall i \in \{ 1,2,\ldots,n \}, x_1, \ldots, x_n, \forall y_i: \left| f(x_1, \ldots, x_i, \ldots, x_n) - f(x_1, \ldots, y_i, \ldots, x_n) \right| \le c
$$

且 $X_1, \ldots, X_n$ 相互独立，就会有 McDiarmids Inequality：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202408271509653.png)