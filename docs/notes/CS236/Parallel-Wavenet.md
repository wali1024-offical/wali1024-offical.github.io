---
tags:
  - generative-model
  - flow-model
title: Parallel Wavenet
date created: 星期日, 九月 22日 2024, 11:46:24 中午
date modified: 星期日, 九月 22日 2024, 12:47:06 下午
---

> 结合了两种 [Autoregressive-Flow](Autoregressive-Flow.md) model 的优点（MAF 和 IAF），是一个 teacher-student 架构的模型：teacher 用 MAF，利用 training 部分速度快的优点，可以采用 MLE loss；student 采用 IAF, 利用 sampling 快的特点。相较于原始的 wavenet 快了近 1000 倍

先训练 teacher model，再训练 student model，期望是最小化和 teacher model 给出的分布的 KL divergence：

$$
D_{KL}(s||t)=\mathbf{E}_{x \sim s}[\log s(x) - \log t(x)]
$$

需要进行以下操作：

- 从 student model 中取样 $x$
- 通过 student model 计算 $x$ 的 density
- 通过 teacher model 计算 $x$ 的 density

## Training

1. 训练 teacher model (MAF)，使用 MLE
2. 训练 student model (IAF)，最小化和 teacher model 的 KL divergence

## Testing

使用 student model 进行测试
