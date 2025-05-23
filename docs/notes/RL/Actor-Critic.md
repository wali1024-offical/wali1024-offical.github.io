---
title: Actor Critic
tags:
  - RL
math: true
---

## The Simplest Actor-critic (QAC)

**Actor (policy update)**：在 [Policy-Function-Approximation](Policy-Function-Approximation.md) 中的算法，用来更新 policy 参数 $\theta$、实施 policy 与环境交互

$$
\theta _{t+1} =\theta _t + \alpha \nabla _\theta \ln \pi (a_t|s_t, \theta _t) q_t (s_t,a_t) \\
$$

**Critic (value update)**：用于估计 $q_t \approx q_\pi$ 的算法，即评判当前策略到底好不好，和参数 $w$ 相关

伪代码：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409120910852.png)

Critic 中使用了 sarsa + value function approximation

该算法是 on-policy 的。

## Advantage Actor-critic (A2C)

通过引入一个 bias 来减少方差，同时保持期望：

$$
\begin{align*}
    \nabla _\theta J(\theta) &= \mathbb{E} _{S \sim \eta, A \sim \pi} \left[ \nabla _\theta \ln \pi (A|S, \theta_t)q_\pi (S, A) \right] \\
    &= \mathbb{E} _{S \sim \eta, A \sim \pi} \left[ \nabla _\theta \ln \pi (A|S, \theta_t)(q_\pi (S, A) - b(S)) \right]
\end{align*}
$$

这样采样的时候，误差也会更小。

可以推导出最好的 $b(S)$ 为：

$$
b(S) = \frac{\mathbb{E}_{A \sim \pi}\left[ \Vert \nabla _\theta \ln \pi (A | s, \theta_t) \Vert ^2 q(s,A) \right]}{\mathbb{E}_{A \sim \pi}[\Vert \nabla _\theta \ln \pi (A | s, \theta_t)\Vert ^2]}
$$

太复杂，在实际中可以去除权重，简化为

$$
b(s) = \mathbb{E}_{A \sim \pi}[q(s, A)] = v_\pi (s)
$$

此时

$$
\nabla _\theta J(\theta) = \mathbb{E} _{S \sim \eta, A \sim \pi} \left[ \nabla _\theta \ln \pi (A|S, \theta_t)(q_\pi (S, A) - v_\pi (S)) \right]
$$

记 $\delta _\pi (S, A) = q_\pi (S,A) - v_\pi (S)$，称为 advantage function。直观理解为，在某一个状态下，状态所带来的价值只有可能在一个很小的范围内，但是可以通过选择不同的 action 来相对提升 reward（比方说打牌的时候拿到一手烂牌，技术好的话，就可以用好的出牌策略减小损失/增大收益）

stochastic version:

$$
\begin{align*}
    \theta_{t+1} &= \theta_t + \alpha \nabla _\theta \ln \pi (a_t | s_t, \theta_t) [ q_t(s_t, a_t) - v_t (s_t)] \\
    &= \theta_t + \alpha \nabla _\theta \ln \pi (a_t | s_t, \theta_t) \delta _t (s_t, a_t) \\
    &= \theta_t + \alpha \left( \frac{\delta _t (s_t, a_t)}{\pi (a_t | s_t, \theta_t)}  \right) \nabla _\theta \pi (a_t| s_t, \theta_t)
\end{align*}
$$

显然 $\delta_t (s_t, a_t)$ 比 $q_t (s_t, a_t)$ 更好，因为在乎的是相对大小。

进一步，对 $\delta _t$ 可以进行近似：

$$
\delta _t = q_t(s_t, a_t) - v_t (s_t) \rightarrow r_{t+1} + \gamma v_t (s_{t+1}) - v_t (s_t)
$$

从而减少参数量。

伪代码：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409120942789.png)

## Off-policy Actor-critic

使用 importance sampling trick 可以将 actor-critic 算法改为 off-policy 的，

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409121010269.png)

**off-policy gradient theorem**

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409121011694.png)

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409121015398.png)

伪代码：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409121016067.png)

### Importance Sampling

目标：通过 $X \sim p_1$ 求 $\mathbb{E}_{X \sim p_0}[X]$

$$
\mathbb{E}_{X \sim p_0}[X] = \sum_{x} xp_0(x) = \sum_{x} p_1(x) \frac{p_0(x)}{p_1(x)} x := \sum_{x} p_1(x) f(x) = \mathbb{E}_{X \sim p_1}[f(X)]
$$

而 $\mathbb{E}_{X \sim p_1}[f(X)]$ 可以用经验期望来估计，所以

$$
\mathbb{E}_{X \sim p_0}[X] \approx \frac{1}{n} \sum_{i=1}^{n}\frac{p_0(x_i)}{p_1(x_i)}x_i
$$
