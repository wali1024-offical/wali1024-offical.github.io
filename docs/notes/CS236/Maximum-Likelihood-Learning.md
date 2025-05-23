---
tags: [DL, information-theory, probability]
title: Maximum-Likelihood-Learning
date created: 星期六, 九月 21日 2024, 3:12:37 下午
date modified: 星期三, 十一月 13日 2024, 4:45:18 下午
---

## KL Divergence Perspective

可以用 KL divergence 度量两个分布之间的 " 距离 "：

$$
D_{KL}(p||q) = \mathbf{E}_{x \sim p}\left[\log \frac{p(x)}{q(x)}\right] = \sum_{x} p(x) \log \frac{p(x)}{q(x)}
$$

具有如下性质：

- $D_{KL}(p||q) \ge 0$ for all $p$, $q$，当且仅当 $p=q$ 时取等
- **Asymmetric**: $D_{KL}(p||q) \neq D_{KL}(q||p)$
- Measures the expected number of extra bits required to describe samples from $p(x)$ using a compression code based on $q$ instead of $p$

这样，对于实际分布 $P_{data}(x)$ 和从数据中学到的分布 $P_{\theta}(x)$，就可以计算它们之间的 " 差距 " 了：

$$
\begin{align*}
    D_{KL}(P_{data}||P_{\theta}) &= \mathbf{E}_{x \sim P_{data}}\left[\log \frac{P_{data}(x)}{P_{\theta}(x)}\right] \\
    &= \mathbf{E}_{x \sim P_{data}}[\log_{} P_{data}(x)] - \mathbf{E}_{x \sim P_{data}} [\log_{} P_{\theta}(x)]
\end{align*} 
$$

由于第一项和 $\theta$ 无关，所以**最小化 KL divergence 等价于最大化 log-likelihood**

$$
\arg\min_{\theta} D_{KL}(P_{data} || P_{\theta}) = \arg\min_{{\theta}} -\mathbf{E}_{x \sim P_{data}}[\log_{} P_{\theta}(x)] = \arg\max_{\theta} \mathbf{E}_{x \sim P_{data}} [\log P_{\theta}(x)]
$$

又由于 $P_{data}$ 一般都是未知的，所以采取近似：

$$
\mathbf{E}_{x \sim P_{data}}[\log P_{\theta}(x)] \to \mathbf{E}_\mathcal{D}[\log P_{\theta}(x)] \to \frac{1}{|\mathcal{D}|} \sum_{x \in \mathcal{D}}^{} \log P_{\theta}(x)
$$

第一步是缩小了数据的范围，第二步是使用了 Monte Carlo Estimate

这里 $\mathcal{D}$ 是已知的 dataset

## Shannon/Huffman Code Perspective

对于一个概率分布 $p_{\theta}(x)$，每一个 $x \sim p_{\theta}(x)$ 都可以赋予一个编码长度 $\left\lceil  \log \frac{1}{p_{\theta}(x)}  \right\rceil$，也就是出现频率越高，编码长度越低，从而总的平均编码长度能够尽可能的小，设真实分布为 $p_{data}$，则

$$
\mathbf{E}_{x \sim p_{data}}\left[ \left\lceil  \log \frac{1}{p_{\theta}(x)}  \right\rceil  \right] \approx \mathbf{E}_{x \sim p_{data}}\left[ \log \frac{1}{p_{\theta}(x)} \right] = - \mathbf{E}_{x \sim p_{data}} [\log p_{\theta}(x)]
$$

就代表了在真实分布下的平均编码长度。最大化 likelihood 也就是最小化这个平均编码长度。
