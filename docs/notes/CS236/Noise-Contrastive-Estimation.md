---
tags:
  - generative-model
title: Noise Contrastive Estimation
date created: 星期一, 九月 23日 2024, 9:11:01 上午
date modified: 星期一, 九月 23日 2024, 9:39:25 上午
---

> 采用类似于 GAN 中的对抗算法训练，是一种 likelihood-free 的算法

取一个易于计算的 noise distribution $p_{noise}(x)$，训练一个 discriminator $D_{\theta}(x) \in [0,1]$ 来将它和真实数据分布 $p_{data}(x)$ 进行区分

目标函数：

$$
\max_{\theta} \mathbf{E}_{x \sim p_{data}}[\log D_{\theta}(x)] + \mathbf{E}_{x \sim p_{noise}}[1-\log D_{\theta}(x)]
$$

最终得到的最优 $D_{\theta^*}(x)$ 满足

$$
D_{\theta^*}(x)=\frac{p_{data}(x)}{p_{data}(x) + p_{noise}(x)}
$$

如果事先令

$$
D_{\theta}(x)=\frac{p_{\theta}(x)}{p_{\theta}(x) + p_{noise}(x)}
$$

则训练 $D_{\theta}(x)$ 相当于间接的训练了 $p_{\theta}(x)$，现实中只需要反解出 $p_{\theta}(x)$ 就行了

$$
p_{\theta}(x)= \frac{p_{noise}(x)D_{\theta}(x)}{1-D_{\theta}(x)}
$$
