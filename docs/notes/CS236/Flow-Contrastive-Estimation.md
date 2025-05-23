---
tags: [energy-based-model, flow-model, generative-model]
title: Flow Contrastive Estimation
date created: 星期二, 九月 24日 2024, 12:19:21 中午
date modified: 星期三, 十月 2日 2024, 8:47:18 早上
---

在 [Noise-Contrastive-Estimation](Noise-Contrastive-Estimation.md) 中，引入了噪声分布 $p_{noise}$，但是当其过于简单时，可能并不能很好的学习到 discriminator $D_{\theta}$。自然的想法是期望 $p_{noise}$ 和 $p_{data}$ 越接近越好，这样 $D_{\theta}$ 更强大，学到的 $p_{\theta}$ 也会更好。可以用 [Flow-Models](Flow-Models.md) 来实现。

使用 Flow Model 来从 $p_{noise}$ 变换到 $p_{noise, \phi }$，接着使用新的 discriminator $D_{\theta,Z,\phi}$：

$$
D_{\theta,Z,\phi}=\frac{e^{f_{\theta}(x)}}{e^{f_{\theta}(x)} + p_{noise, \phi}(x)Z}
$$

当然期望 flow model 能够使得 $p_{noise,\phi}(x) \approx p_{data}$
