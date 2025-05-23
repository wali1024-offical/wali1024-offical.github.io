---
tags: [generative-model]
title: Score Matching
date created: 星期一, 九月 23日 2024, 9:24:33 上午
date modified: 星期一, 九月 23日 2024, 9:39:05 上午
---

> 采用 **Fish Divergence** 来定义目标分布和模型分布的差距，直观上看，就是想要先让梯度近似相等，那么两个分布就应该很接近了，也是一种 [Maximum-Likelihood-Learning](Maximum-Likelihood-Learning.md) 算法

$$
D_{F}(p,q):= \frac{1}{2} \mathbf{E_{x \sim p}}[\lVert \nabla_{x}\log p(x) - \nabla_{x}\log q(x) \rVert^2 ]
$$

这里被称为 **score matching**：

$$
\begin{align}
D_{F}(p_{data},p_{\theta}) & =\frac{1}{2} \mathbf{E}_{x \sim p_{data}}[\lVert \nabla_{x}\log p_{data}(x) - \nabla_{x} \log p_{\theta}(x)\rVert^2] \\
 & =\frac{1}{2}\mathbf{E}_{x \sim p_{data}}[\lVert \nabla_{x}\log p_{data}(x)-s_{\theta}(x)\rVert^2] \\
 & =\frac{1}{2}\mathbf{E}_{x \sim p_{data}}[\lVert\nabla_{x}\log p_{data}(x)-\nabla_{x}f_{\theta}(x)\rVert^2]
\end{align}
$$

由于 $\log p_{data}(x)$ 不易求得，可以变形：

$$
\begin{align}
D_{F}(p_{data},p_{\theta})  & = \frac{1}{2} \int _{x} p_{data}(x) \lVert \nabla_{x}\log p_{data}(x)-\nabla_{x}\log p_{\theta}(x) \rVert ^2 \, dx  \\
 & =\frac{1}{2}\int _{x} p_{data}(x)(\nabla_{x}\log p_{data}(x))^2 \, dx +\frac{1}{2}\int _{x}p_{data}(x)(\nabla_{x}\log p_{\theta}(x))^2 \, dx  \\
 & \quad-\int _{x}p_{data}(x)\nabla_{x}\log p_{data}(x) \nabla_{x}\log p_{\theta}(x)  \, dx 
\end{align}
$$

而

$$
\begin{align}
 & \quad -\int _{x}p_{data}(x)\nabla_{x}\log p_{data}(x) \nabla_{x}\log p_{\theta}(x)  \, dx   \\
 & =-\int _{x}p_{data}(x) \frac{\nabla_{x}p_{data}(x)}{p_{data}(x)} \nabla_{x}\log p_{\theta}(x) \, dx  \\
 & =-\int _{x}\nabla_{x}p_{data}(x)\nabla_{x}\log p_{\theta}(x) \, dx  \\
 & =-p_{data}(x)\nabla_{x}\log p_{\theta}(x)|_{-\infty}^{\infty} + \int _{x} p_{data}(x)\nabla_{x}^2\log p_{\theta}(x) \, d x \\
 & =\int _{x}p_{data}(x)\nabla_{x}^2 \log p_{\theta}(x) \, dx 
\end{align}
$$

这里假设了 $p_{data}(x)$ 会在无穷大时充分的小，故

$$
\begin{align}
D_{F}(p_{data},p_{\theta}) & =\frac{1}{2}\int _{x} p_{data}(x)(\nabla_{x}\log p_{data}(x))^2 \, dx +\frac{1}{2}\int _{x}p_{data}(x)(\nabla_{x}\log p_{\theta}(x))^2 \, dx  \\
 & \quad-\int _{x}p_{data}(x)\nabla_{x}\log p_{data}(x) \nabla_{x}\log p_{\theta}(x)  \, dx  \\
 & = \frac{1}{2}\int _{x} p_{data}(x)(\nabla_{x}\log p_{data}(x))^2 \, dx +\frac{1}{2}\int _{x}p_{data}(x)(\nabla_{x}\log p_{\theta}(x))^2 \, dx \\
 & \quad + \int _{x}p_{data}(x)\nabla_{x}^2 \, \log p_{\theta}(x)  \\
 & = \frac{1}{2}\int _{x}p_{data}(x)(\nabla_{x}\log p_{\theta}(x))^2 \, dx + \int _{x}p_{data}(x)\nabla_{x}^2 \, \log p_{\theta}(x) +\text{const} \\
 & = \mathbf{E}_{x \sim p_{data}(x)}\left[ \frac{1}{2}\left(\nabla_{x}\log p_{\theta}(x) \right)^2 + \nabla_{x}^2 \log p_{\theta}(x) \right] + \text{const}
\end{align}
$$

这是一维情形，高维情形下：

$$
D_{F}(p_{data},p_{\theta})=\mathbf{E}_{x \sim p_{data}(x)}\left[\frac{1}{2}\left\lVert \nabla_{x}\log p_{\theta}(x)  \right\rVert ^2 + \mathrm{tr}(\nabla_{x}^2\log p_{\theta}(x))\right] + \text{const}
$$

这样就消掉 $p_{data}(x)$ 了，只需要从真实分布中取样，就可以进行计算。
