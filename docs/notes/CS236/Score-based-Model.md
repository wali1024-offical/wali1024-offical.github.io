---
tags: [generative-model, score-based-model]
title: Score-based Model
date created: 星期二, 九月 24日 2024, 1:12:27 下午
date modified: 星期三, 九月 25日 2024, 3:31:52 下午
---

> 在 [Energy-Based-Model-(EBM)](Energy-Based-Model-(EBM).md) 中，使用了 score function 和 [Score-Matching](Score-Matching.md) 技术进行训练。可以拓展到更广的模型上，即为 score-based model

## Training

### [Score-Matching](Score-Matching.md)

定义 score function: $s_{\theta}(x)=\nabla_{\theta}\log(p_{\theta}(x))$，score matching: $\mathbf{E}_{x \sim p_{data}}\left[\frac{1}{2}\lVert s_{\theta}(x) \rVert^{2} + \mathrm{tr}(\nabla_{x}s_{\theta}(x)) \right]$

可以直接利用最小化 Fisher divergence 进行训练。缺点：计算 $\mathrm{tr}(\nabla_{x}s_{\theta}(x))$ 非常 expensive（二阶导，结果是 Hessian 矩阵，分别计算对角线元素时间复杂度 $O(d)$），下述方法可以避免计算二阶导数。

### Denoising Score Matching

> 先给原始分布加上噪声得到新的分布，然后尝试拟合新的分布，并在采样的时候进行 denoising

考虑 

$$
\begin{align}
 & q_{\sigma}(\tilde{x}|x)=\mathcal{N}(x, \sigma^{2}I),  \\
 & q_{\sigma}(\tilde{x})=\int p(x)q_{\sigma}(\tilde{x}|x) \, dx
\end{align}
$$

$q_{\sigma}(\tilde{x})$ 是 $p(x)$ 的一个近似，且计算 $\nabla_{\tilde{x}}\log q_{\sigma}(\tilde{x})$ 更容易（相较于真实数据分布的 score function $\nabla_{x}\log p_{data}(x)$）

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@images/20240924145055.png)

当噪声程度较小时，近似得也就越好，即 $q_{\sigma}(\tilde{x})\approx p(\tilde{x})$。此时 Fisher divergence 变为：

$$
\begin{align}
\frac{1}{2}\mathbf{E}_{\tilde{x} \sim q_{\sigma}}\left[\lVert \log q_{\sigma}(\tilde{x})-s_{\theta}(x) \rVert ^{2}_{2}\right]  & =\frac{1}{2} \int q_{\sigma}(x)\lVert \nabla_{\tilde{x}}\log q_{\sigma}(\tilde{x})-s_{\theta}(\tilde{x}) \rVert _{2}^{2} \, d\tilde{x} \\
 & =\frac{1}{2}\mathbf{E}_{\tilde{x} \sim q_{\sigma}(x)}[\lVert s_{\theta}(\tilde{x}) \rVert _{2}^{2}] - \int q_{\sigma}(\tilde{x})\nabla_{\tilde{x}}\log q_{\sigma}(\tilde{x})^{\top}s_{\theta}(\tilde{x}) \, d\tilde{x} + \text{const} \\
 & =\dots \\
 & =\mathbf{E}_{x \sim p_{data}(x)}\mathbf{E}_{\tilde{x} \sim q_{\sigma}(\tilde{x}|x)} [\lVert s_{\theta}(\tilde{x}) - \nabla_{\tilde{x}}\log q_{\sigma}(\tilde{x}|x) \rVert_{2}^{2} ] + \text{const}
\end{align}
$$

直观理解：在给定的一个 $x$ 处，结合 $x$ 这个信息，估计附近的 score function

推导如下：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@images/20240924145958.png)

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@images/Screenshot%20from%202024-09-24%2015-00-27.png)

并且 

$$
\begin{align}
\nabla_{\tilde{x}}\log q_{\sigma}(\tilde{x}|x) & = \nabla_{\tilde{x}}\log \left( \frac{1}{(2\pi)^{d/2}\sigma^{d}}e^{\frac{\lVert \tilde{x} - x \rVert ^{2}}{2 \sigma^{2}}} \right)\\
 & =-\frac{\tilde{x}-x}{\sigma}
\end{align}
$$

便于计算。另外，这表明此时 score function 的方向就是添加的 noise 的**反**方向，沿 score function 走，就能消除 noise

于是 training 变为

- 取样 $\{ x_{1}, \dots,x_{n} \}\sim p_{data}(x)$
- 取样 $\{ \tilde{x}_{1},\dots,\tilde{x}_{n} \}\sim q_{\sigma}(\tilde{x}), \tilde{x}_{i} \sim q_{\sigma}(\tilde{x}_{i}|x_{i})$，需要取 $\sigma$ 足够小
- 最小化 Fish divergence (SGD):

$$
\begin{align}

 & \quad \frac{1}{2n}\sum_{i=1}^{n} [\lVert s_{\theta}(\tilde{x}*{i}) - \nabla*{\tilde{x}}\log q_{\sigma}(\tilde{x}*{i}|x*{i}) \rVert^{2}*{2}] \\

 & = \frac{1}{2n} \sum*{i=1}^{n} \sum_{i=1}^{n} \left[\left\lVert s_{\theta}(\tilde{x}*{i})-\frac{\tilde{x}*{i}-x_{i}}{\sigma^{2}} \right\rVert *{2}^{2} \right] \\

  & \leftrightarrow \frac{1}{2} \mathbb{E}*{x \sim p(x)} \mathbb{E}*{\tilde{x} \sim q*{\sigma}(\tilde{x}| x)} \left[\left\lVert \frac{x - \tilde{x}}{\sigma^{2}} - s_{\theta}(\tilde{x}*{i}) \right\rVert^{2} \right] + \text{const} \\

 & = \frac{1}{2}\mathbb{E}*{x \sim p_{data}, z \sim \mathcal{N}(0, I)}\left[\left\lVert s_{\theta}(x + \sigma z) + \frac{z}{\sigma} \right\rVert _{2}^{2} \right]

\end{align}
$$

这里 $\mathbb{E}_{x \sim p(x)}$ 代表从真实的数据分布中取样，而 $\mathbb{E}_{\tilde{x} \sim q_{\sigma}(\tilde{x} | x)}$ 则代表向 $x$ 中加入噪声；$\frac{\tilde{x}- x}{\sigma^{2}}$ 则是通过加入一点点轻微的扰动来估计真实的梯度（从而替代了二阶导数的计算），从而和模型预测的梯度 $s_{\theta}(\tilde{x})$ 作比较；$s_{\theta}(\tilde{x})$ 试图模拟这种扰动。

缺点：实际上 score function $s_{\theta}(x)$ 是在拟合 $\nabla_{x}\log q_{\sigma}(x)$ 而不是 $\nabla_{x}\log p_{data}(x)$

#### Pitfalls

- 当 $\sigma \to 0$ 时，loss 的方差会非常大

### Slicing Score Matching

考虑将高维问题转化为一维问题：每次都随机将所有 score function 投影到一个方向上，然后在该方向上拟合 score function。Sliced Fisher divergence:

$$
\frac{1}{2} \mathbf{E}_{v \sim p_{v}} \mathbf{E}_{x \sim p_{data}}[(v^{\top}\nabla_{x}\log p_{data}(x) - v^{\top}s_{\theta}(x))^{2}]
$$

$p_{v}$ 可以使用 Gaussian 等分布。进行和之前方法一样的操作，得到转化后的 sliced Fisher divergence:

$$
\mathbf{E}_{v \sim p_{v}} \mathbf{E}_{x \sim p_{data}}\left[ v^{\top}\nabla_{x}s_{\theta}(x)v + \frac{1}{2} (v^{\top}s_{\theta}(x))^{2} \right]
$$

由于可以一次性算出 $v^{\top}\nabla_{x}s_{\theta}(x)v=v^{\top}\nabla_{x}(v^{\top}s_{\theta}(x))$，如下图，一次反向传播就够了

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@images/20240924212651.png)

## Sampling

### Langevin Dynamics Sampling

> 数学理论见 [Langevin Dynamics](../stochastic-progress/Lecture19-Informal-Definition-of-Diffusion-and-Examples.md)

- sample $x^{0} \sim \pi(x)$
- Repeat for $t=1,2,\dots,T:$
    - $z^{t} \sim \mathcal{N}(0,I)$
    - $x^{t} \leftarrow x^{t-1} + \frac{\epsilon}{2} \nabla_{x} \log p_{}(x^{t-1}) + \sqrt{\epsilon }z^{t}$
- If $\epsilon \to 0$ and $T \to \infty$, then we are guaranteed to get $x^{T} \sim p(x)$

这里第四行就是沿着 score function 的方向前进，途中带一点随机性，以期消除掉 noise（即为 denoising 过程）。为什么成立：因为 score function 就是指向了 log-likelihood 增大的方向，自然 likelihood 大的地方更容易被达到，而 likelihood 小的地方就不容易被达到。

### Annealed Langevin Dynamics Sampling

见 [Gaussian-Perturbation](Gaussian-Perturbation.md)

## Pitfalls

- 真实数据的 score function 是人为假设的，实际上存不存在无法保证
- 在 low data density 的区域表现较差
- slow mixing of Langevin dynamics between data modes

可以使用 [Gaussian-Perturbation](Gaussian-Perturbation.md) 来同时解决这三个问题。

## Conditional Generation

可以在生成时加入标签，控制噪声的产生 $\epsilon_{\theta}(x_{t}, t) \to \epsilon _{\theta  }(x_{t}, t;y)$，此时的 score function 

$$
\begin{align}
\nabla_{x}\log p_{\theta}(x|y)&=\nabla_{x}\log \frac{p(y|x)p_{\theta}(x)}{p(y)} \\
 & =\nabla_{x}\log p(y|x) + \nabla_{x}\log p_{\theta}(x) - \nabla_{x}\log p(y) \\
 & =\nabla_{x}\log p(y|x) + s_{\theta}(x)
\end{align}
$$

而 $p_{\theta}(y|x)$ 可以由一个神经网络计算，故新的 score function 是可以直接计算的，意味着对一个已经训练好的 score model，可以“插入”任意一个 conditional model.
