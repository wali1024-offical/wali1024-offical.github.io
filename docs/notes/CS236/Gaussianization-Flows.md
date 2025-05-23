---
title: Gaussianization Flows
date created: 星期日, 九月 22日 2024, 1:26:42 下午
date modified: 星期日, 九月 22日 2024, 2:05:28 下午
tags: [flow-model, generative-model]
---

> [Flow-Models](Flow-Models.md) 的一个变体

设 latent variable $Z \sim \mathcal{N}(0,I)$ ，根据模型参数 $\theta$ infer 出的数据分布为 $X = f_{\theta}(Z)$，真实数据分布为 $\tilde{x} \sim P_{data}$。仍然使用 [Maximum-Likelihood-Learning](Maximum-Likelihood-Learning.md) 进行训练，以最小化 KL divergence。可以证明：

$$
D_{KL}(P_{data} || P_{\theta}) = D_{KL}(P_{\tilde{X}} || P_{X})=D_{KL}(P_{f_{\theta}^{-1}(\tilde{X})} || P_{f_{\theta}^{-1}(X)}) = D_{KL}(P_{f_{\theta}^{-1}(\tilde{X})} || P_{Z})
$$

## Inverse CDF Trick

（1D case）记真实数据的 CDF 为 $F_{data}(a)=\int _{-\infty}^{a} p_{data}$，当 $F_{data}$ 已知的时候，可以通过 $\tilde{X}=F_{data}^{-1}(U), U \sim \text{Uniform}[0,1]$，来进行取样。可以将 $U$ 变换成 Gaussian 分布：

$$
\Phi^{-1}(U)=\Phi^{-1}(F_{data}(X))
$$

这里 $\Phi$ 为 Gaussian 分布的 CDF，算子 $\Phi^{-1}\circ F_{data}$ 成为 Gaussianize 过程，如下图：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/Screenshot%20from%202024-09-22%2013-44-11.png)

## Model

由两部分组成：Gaussianize 和 rotation。

- **Step 1**: 对每个维度进行 Gaussianization (Jacobian is a diagonal matrix and is tractable)
- **Step 2**: 对第一步得到的数据进行旋转操作 (Jacobian is a diagonal matrix and is tractable)

反复进行以上两步，直到数据分布收敛于 $\mathcal{N}(0,I)$。

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/Screenshot%20from%202024-09-22%2013-54-35.png)
