---
tags:
  - RL
title: Policy Function Approximation
math: true
---

和 [value function approximation](Value-Function-Approximation.md) 类似，都是引入参数 $\theta$，$\pi (a | s) \rightarrow \pi (a | s, \theta)$

## Metrics to Define Optimal Policies

**Average state value**：

$$
\overline{v}_\pi  = \sum_{s \in S}^{} d(s) v_\pi (s) = \mathbb{E}[v_\pi (s)] = d^T v_\pi = \mathbb{E}\left[ \sum_{t=0}^{\infty} \gamma ^t R_{t+1} \right]
$$

选取概率向量 $d$：可以简单假设为 uniform 的，或者服从 stationary distribution。

**Average reward**：

$$
\overline{r}_\pi = \sum_{s \in S}^{} d_\pi (s) r_\pi (s) = \mathbb{E}[r_\pi (S)], S \sim d_\pi
$$

这里

$$
\begin{align*}
    r_\pi (s) &= \sum_{a \in A}^{} \pi (a | s) r(s,a) \\
    r(s, a) &= \mathbb{E}[R|s,a] =\sum_{r}^{} rp(r | s,a)
\end{align*} 
$$

容易得到

$$
\overline{r}_\pi = \lim_{n \to \infty} \frac{1}{n}\mathbb{E}\left[ \sum_{k=1}^{n}R_{t+k}|S_t = s_0 \right] = \lim_{n \to \infty} \frac{1}{n} \mathbb{E}\left[ \sum_{k=1}^{n} R_{t+k} \right]
$$

上述两个 metric 都为策略 $\pi$ 的函数，也就是参数 $\theta$ 的函数。自然的想法是，每一步的平均价值/奖励要尽可能的大。所以可以训练 $\theta$ 使得 $\overline{v}_\pi$ 和 $\overline{r}_\pi$ 最大（梯度上升）。

这两个 metric 是等价的：$\overline{r}_\pi = (1 - \gamma)\overline{v}_\pi$

## Gradients of the Metrics

$$
\begin{align*}
\nabla _\theta J(\theta) &=\mathbb{E}_{s \sim \eta (s)}\left[\mathbb{E}_{a \sim \pi_{\theta}(s)}[q_{\pi}(s,a)]\right] \\
&= \mathbb{E}_{s \sim \eta (s)}\left[ \sum_{a \in A} \pi (a|s, \theta) q_{\pi} (s,a) \right] \\ &
=\sum_{s \in S}^{} \eta (s) \sum_{a \in A}^{} \pi (a | s, \theta) q_\pi (s,a)
\end{align*}
$$

这里 $q_{\pi } (s,a)$ 也可以取很多其他的值

推导：

$$
\nabla _\theta \overline{r}_\pi = \sum_{s}^{} d_\pi (s) \sum_{a}^{} \nabla _\theta \pi (a | s, \theta) q_\pi (s, a)
$$

其余 metric 同理。

上述 gradient 的一个变型为：

$$
\nabla _\theta J(\theta) = \mathbb{E}\left[ \nabla _\theta \ln \pi (A|S, \theta) q_\pi (S,A) \right]
$$

这里 $S \sim \eta, A \sim \pi (A|S, \theta)$，使用了梯度 $\ln$ 转期望的 trick

## Gradient-ascent Algorithm (REINFORCE)

$$
\begin{align*}
    \theta _{t+1} &= \theta _t + \alpha \nabla _\theta J(\theta) \\
    &= \theta _t + \alpha \mathbb{E}\left[ \nabla _\theta \ln \pi (A|S, \theta _t) q_\pi (S,A) \right] \\
    &\rightarrow \theta _t + \alpha \nabla _\theta \ln \pi (a|s, \theta _t) q_\pi (s,a)
\end{align*} 
$$

为了进行 SGD，需要采样 $s$ 和 $a$，而 $S \sim d$ (long-run behavior under $\pi$)，进而 $a_t \sim \pi (s, \theta_t)$

由于 $q_\pi$ 未知，可使用 MC (此时称为 REINFORCE 算法) 或 TD learning (此时称为 actor-critic 算法) 用 $q_t$ 近似 $q_\pi$

$$
\begin{align*}
    \theta _{t+1} &\rightarrow \theta _t + \alpha \nabla _\theta \ln \pi (a_t|s_t, \theta _t) q_t (s_t,a_t) \\
    &= \theta _t + \alpha \left( \frac{q_t(s_t, a_t)}{\pi (a_t | s_t, \theta _t)} \right) \nabla _\theta \pi (a_t | s_t, \theta _t) \\
    &:= \theta _t + \alpha \beta _t \nabla _\theta \pi (a_t | s_t, \theta _t) \\
\end{align*} 
$$

还可以得到：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409120843359.png)

$\beta_t$ 可以用于平衡 exploration 和 exploitation：

- $q_t(s_t, a_t)$ 越大，$\beta _t$ 越大，也就是 value 越大，学习的步长越大，exploitation 就越强
- $\pi (a_t | s_t, \theta_t)$ 越小，$\beta _t$ 越大，也就是在概率小的 action 上进行 exploration

REINFORCE 伪代码：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409120850359.png)
