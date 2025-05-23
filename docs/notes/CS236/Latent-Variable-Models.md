---
tags: [generative-model]
title: Latent-Variable-Models
date created: 星期六, 九月 21日 2024, 4:05:11 下午
date modified: 星期三, 九月 25日 2024, 4:42:00 下午
---

> 和 [Autoregressive-Models](Autoregressive-Models.md) 一样，是一种 [Max Likelihood](Maximum-Likelihood-Learning.md) based learning

## Motivation

使用一个向量 $z$ 来隐式的代表输入 $x$ 的 high-level 的 features，这个 $z$ 应该也会服从某种概率分布。

例如，假设 $z \sim \mathcal{N}(0, I)$，且 $x$ 满足 $p(x|z)=\mathcal{N}(\mu_{\theta}(z), \Sigma_{\theta}(z))$，可以用 neural network 来拟合 $\mu_{\theta}$ 和 $\Sigma_{\theta}$ 两个映射

## Mixture of Gaussians

Bayes net $z \to x$

- $z \sim \text{Categorical}(1, \cdots, K)$
- $p(x|z=k) = \mathcal{N}(\mu_{k}, \Sigma_{k})$

如下图：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/Screenshot%20from%202024-09-21%2016-18-29.png)

当 Gaussian 的个数很多，例如无穷个的时候，可以将 $z$ 的分布进行修改，例如：$\text{Categorical} \to \mathcal{N}(0, I)$，此时 $p(x|z)=\mathcal{N}(\mu_{\theta}(z), \Sigma_{\theta}(z))$，这里两个参数都用神经网络来拟合。这就是 VAE 的雏形。

## Variational Autoencoders

用 [Maximum-Likelihood-Learning](Maximum-Likelihood-Learning.md) 中的**最小化 KL divergence 相当于最大化 log-likelihood**的结论，目标函数：

$$
\arg \max_{\theta}\log \prod_{x \in \mathcal{D}} P_{\theta}(x)
$$

而

$$
\log \prod_{x \in \mathcal{D}} P_{\theta}(x) = \sum_{x \in \mathcal{D}} \log P_{\theta}(x) = \sum_{x \in \mathcal{D}}\log \left( \sum_{z} P_{\theta}(x, z) \right)
$$

但是

$$
\sum_{z}P_{\theta}(x,z)
$$

基本上不可算，可做如下尝试：

### Naive Monte Carlo

用 MC 定理，可以用

$$
\sum_{z}P_{\theta}(x,z) = |\mathcal{Z}| \sum_{z \in \mathcal{Z}} \frac{1}{|\mathcal{Z}|} P_{\theta}(x,z)=|\mathcal{Z}|\mathbf{E}_{z \sim \text{Uniform}}P_{\theta}(x,z) \approx |\mathcal{Z}| \frac{1}{k} \sum_{i=1}^k P_{\theta}(x, z^{(j)})
$$

近似。理论上可行，但是实际上不可行，效果较差。

### Importance Sampling

$$
P_{\theta}(x) = \sum_{z} P_{\theta}(x,z) = \sum_{z \in \mathcal{Z}} \frac{q(z)}{q(z)} P_{\theta}(x,z) = \mathbf{E}_{z \sim q(z)} \left[ \frac{P_{\theta}(x,z)}{q(z)} \right] \approx \frac{1}{k} \sum_{j=1}^k \frac{P_{\theta}(x,z^{(j)})}{q(z^{(j)})}
$$

这里 $z^{(j)},j=1,\dots,k$ 是从 $z \sim q(z)$ 中采样得到的，$q(z)$ 是任意一个分布。由于需要显式地制定 $q(z)$，因此一般也不容易达到较好的效果。

### Evidence Lower Bound

由于 $\log$ 是 concave 的，所以有

$$
\begin{align*}
    \log \sum_{z \in \mathcal{Z}} P_{\theta}(x,z) &= \log \left( \mathbf{E}_{z \sim q(z)} \left[ \frac{P_{\theta}(x,z)}{q(z)} \right] \right) \\
    &\ge \mathbf{E}_{z \sim q(z)}\left[ \log \left( \frac{P_{\theta}(x,z)}{q(z)} \right) \right] \\
    &=\mathbf{E}_{z \sim q(z)}[\log P_{\theta}(x,z)] - \mathbf{E}_{z \sim q(z)}[\log q(z)] \\
    &= \sum_{z}q(z)\log P_{\theta}(x,z) - \sum_{z}^{}  q(z)\log q(z) \\
    &= \sum_{z}q(z)\log P_{\theta}(x,z)+H(q)
\end{align*}
$$

当 $q(z)=P_{\theta }(z|x)$ 时取等（*EM 算法中的 E-step*）

也可以用 KL divergence 证明 [^1]：

$$
\begin{align*}
D_{KL}(q(z)||p(z|x;\theta )) &= \sum_{z} q(z)\log \frac{q(z)}{p(z|x;\theta )} \\
&=\sum_{z}q(z)\log \frac{q(z)p(x; \theta )}{p(x,z;\theta)} \\
&=\sum_{z}q(z)\log q(z) + \sum_{z}q(z)\log p(x;\theta) - \sum_{z}q(z)\log p(x,z;\theta) \\
&=-H(q) + \log p(x;\theta) - \sum_{z}q(z)\log p(x,z;\theta) \geq 0
\end{align*}
$$

故有

$$
\log p(x;\theta) \ge H(q) + \sum_{z}q(z)\log p(x,z;\theta )
$$

这也符合 $q(z)$ 的近似 $p(z|x;\theta)$ 的用途。也就是说，$q(z)$ 和 $p(z|x;\theta)$ 越接近，ELBO 的估计就越准确。自然期望直接令 $q(z)=p(z|x;\theta)$，不过当 $p(z|x;\theta)$ 不可计算时，可以引入参数 $\phi$，用 $q(z; \phi)$ 来代替 $q(z)$，期望 $q(z;\phi)$ 能够近似 $p(z|x;\theta)$（即使后者未知）。其中 $\phi$ 和 $x$ 有对应关系，如下图：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/Screenshot%20from%202024-09-21%2021-40-58.png)

The ELBO applied to the entire dataset:

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/Screenshot%20from%202024-09-21%2021-15-30.png)

Learning via stochastic variational inference (SVI):

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/Screenshot%20from%202024-09-21%2021-16-02.png)

关于 $\nabla_{\theta}\mathcal{L}(x;\theta,\phi)$ 和 $\nabla_{\phi}\mathcal{L}(x;\theta,\phi)$（这里省略了 $x^i$ 和 $\phi^i$ 的上标），前者

$$
\nabla_{\theta}\mathcal{L}(x;\theta,\phi)=\mathbf{E}_{z \sim q(z;\phi)}[\nabla_{\theta}\log p(z,x;\theta)] \approx \frac{1}{K} \sum_{i=1}^{K} \nabla_{\theta}\log P(z^i,x;\theta )
$$

较容易计算，而后者，由于 $\phi$ 的位置更特殊，可以使用更一般的 [REINFORCE 算法](../RL/Policy-Function-Approximation.md)。也有此处效果更好，但是不那么 general 的技术（**reparameterization**），如下：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/Screenshot%20from%202024-09-21%2022-16-50.png)

应用到此处即为

$$
\begin{align}
\mathbf{E}_{z \sim q(z;\phi)}[r(z,\phi)]&=E_{\epsilon \sim \mathcal{N}(0,I)}[r(g(\epsilon;\phi), \phi )] \\
&\approx \frac{1}{K}\sum_{i=1}^{K}r(g(\epsilon^i,\phi), \phi ) 
\end{align}
$$

其中 $\epsilon^i$ 为服从 $\mathcal{N}(0,I)$ 随机采出的样本。倘若 $q_{\phi}(z)$ 不支持进行 reparameterization，那么可以使用

### Amortized Inference

在之前，对每一个样本 $x^i$，都给定了一个参数 $\phi^i$ 来计算 $q(z;\phi^i)$ 以及 $\mathcal{L}(x^i; \theta, \phi^i)$。可以尝试通过构建一个从样本到 $\phi$ 的参数空间的映射 $f_{\lambda}$，这样就大大减少参数量，且避免了昂贵的计算。此时 $q(z;\phi)=q(z;f_{\lambda}(x))$ 记为 $q_{\lambda}(z|x)$，或者 $q_{\phi}(z|x)$。就有

$$
\begin{align}
\mathcal{L}(x;\theta,\phi) & =\sum_{z}q_{\phi}(z|x)\log p(z,x;\theta)+H(q_{\phi}(z|x)) \\
 & =\mathbf{E}_{{z \sim q_{\phi}(z|x)}}[\log p(z,x;\theta)-\log q_{\phi}(z|x)]  \\
 & =\mathbf{E}_{z \sim q_{\phi}(z|x)}[\log p(z,x;\theta) - \log p(z) + \log p(z) - \log q_{\phi}(z|x)] \\
 & =\mathbf{E}_{z \sim q_{\phi}(z|x)}[\log p(x|z;\theta)] - D_{KL}(q_{\phi}(z|x)||p(z))
\end{align}
$$

这里 $p(z)$ 是 $z$ 的先验分布，此处未给定，为了方便可以取 Gaussian 分布等简单的假设。这和 EECS 498-007 中做的笔记结果相同，可以顺利的引出 encoder-decoder 结构了（而不是像之前那样突然地提出来，虽然这里引入 $\log p(z)$ 也有点突然），如下：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/Screenshot%20from%202024-09-21%2023-04-03.png)

## Summary

### Pros

- Easy to be flexible models
- suitable for unsupervised learning

### Cons

- Hard to evaluate likelihoods
- Hard to train via maximum-likelihood
- Fundamentally, the challenge is that posterior inference $p(z | x)$ is hard. Typically requires variational approximations

### 和 [Autoregressive-Models](Autoregressive-Models.md) 的对比

|     Features      | Autoregressive models                                  | Variational Autoencoders                   |
|:---------------: | ------------------------------------------------------ | ------------------------------------------ |
|    probability    | $p_{\theta}(x)=\prod_{i=1}^np_{\theta}(x_{i}\|x_{<i})$ | $p_{\theta}(x)=\int p_{\theta}(x,z) \, dz$ |
|    likelihoods    | tractable                                              | intractable                                |
| learning features | no direct mechanism                                    | has learn feature representation           |

[^1]: 为什么会想到用这个散度呢？因为 VAE 想干的事情就是求隐变量 $z$ 的后验分布 $p(z|x)$，而一般来讲，后验分布都是很难求的，可以通过泛函分析里面的**变分法**来间接的逼近，也就是引入参数 $\theta$，此时变分就是 $\delta q= \nabla_{\theta}q(z;\theta)$
