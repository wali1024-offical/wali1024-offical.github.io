---
title: Stochastic Approximation
tags:
  - RL
math: true
---
Main Idea: 使用**增量**代替一次性计算

$$
\begin{align*}
    w_{k+1} &= \frac{1}{k}\sum_{i=1}^{k}x_i \\
    &= w_k - \frac{1}{k}(w_k - x_k)
\end{align*} 
$$

在 [chapter 5](Monte-Carlo-Learning.md) 中计算 $q_{\pi_k}(s,a)$ 时，也可以使用一样的手法。

推广：使用更 general 的 $\alpha_k > 0$ 替代 $\frac{1}{k}$:

$$
w_{k+1}  = w_k - \alpha_k(w_k - x_k)
$$

用 RM 算法，可以证明这样的迭代一样可以收敛到 $\mathbb{E}[x]$ 去

## Robbins-Monro Algorithm

> **Philosophy**: without model, we need data!

$$
w_{k+1} = w_k - a_k \tilde{g}(w_k, \eta _k)
$$

这里 $\tilde{g}(w_k, \eta _k) = g(w_k) + \eta _k$，$\eta _k$ 为噪声

通过初始化一个 $w_0$ 以及反复迭代，最终可以得到 $g(w) = 0$ 的解。

这样，即使没有 $g(w)$ 的，除了 input-output 对的，任何信息，也可以进行零点求值。也就是：没有模型 $g$，就需要数据 $y = g(x)$

**正式描述**：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409091037853.png)

当把 $g(w)$ 看作是一个 loss function 的导数时，即 $g(w) = \nabla _wJ(w)$，第一个条件暗示 $J(w)$ 是一个凸函数。

第二个条件的 $\sum_{k=1}^{\infty}a_k = \infty$，是因为 $w_\infty - w_0 = \sum_{k=1}^{\infty}a_k \tilde{g}(w_k, \eta_k)$，如果收敛，则初始的 $w_0$ 就不能离 $w_\infty$ 过远，但实际中是不知道到底有多远的，所以要求不能收敛；而 $\sum_{k=1}^{\infty}a_k^2 = 0$ 则保证了 $a_k \rightarrow 0$，从而 $w_{k+1} - w_k= - a_k \tilde{g}(w_k, \eta _k) \rightarrow 0$。

$a_k = \frac{1}{k}$ 显然满足定理要求. 不过实际中一般取 $a_k$ 足够小就行了。

SGD 是 RM 的特殊情况，mean estimation 又是 SGD 的特殊情况

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409091122435.png)
