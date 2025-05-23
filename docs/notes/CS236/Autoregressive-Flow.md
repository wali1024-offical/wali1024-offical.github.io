---
tags:
  - generative-model
  - flow-model
title: Autoregressive Flow
math: true
---

> [Autoregressive-Models](Autoregressive-Models.md) 和 [Flow-Models](Flow-Models.md) 的结合

## Masked Autoregressive Flow (MAF)

> 使用 data 来计算均值、方差

Forward mapping:

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/Screenshot%20from%202024-09-22%2011-28-19.png)

Inverse mapping:

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/Screenshot%20from%202024-09-22%2011-29-09.png)

## Inverse Autoregressive Flow (IAF)

> 使用 latent variable 计算均值、方差，是 MAF 的反向

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/Screenshot%20from%202024-09-22%2011-37-06.png)

## IAF 和 MAF 的对比

| Feature               | MAF                                       | IAF                  |
| --------------------- | ----------------------------------------- | -------------------- |
| likelihood evaluation | fast                                      | slow                 |
| sampling              | slow                                      | fast                 |
| suitable cases        | training based on MLE, density estimation | real-time generation |

可以考虑结合两者的优点，从而改进出新的模型 [Parallel-Wavenet](Parallel-Wavenet.md)，使用 teacher-student 的架构
