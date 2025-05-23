---
tags:
  - RL
title: Value Function Approximation
math: true
---

之前的 state value / action value 都是表格化的（离散的），当 state / action 种类不多时效率较高。如果是连续的情况（或者离散但状态太多），就需要通过采样，利用函数来进行拟合：$\hat{v}(s,w) \approx v _\pi (s)$. 这里 $w$ 为参数向量。

## Algorithm

目标：找到最优的 $w$ 使得 value function 近似最好 $\hat{v}(S, w) \approx v_{\pi}(S)$

### Objective Function

$$
J(w) = \mathbb{E}\left[(v_\pi (S) - \hat{v}(S, w))^2\right]
$$

是关于随机变量 $S$ 的期望。那么需要知道 $S$ 的概率分布，即 states 的概率分布。

当假设所有状态都一样重要（uniform distribution）：

$$
J(w) = \frac{1}{|S|}\sum_{s \in S}^{} (v_\pi (s) - \hat{v}(s, w))^2
$$

但状态不太可能地位相等，使用 stationary distribution（即长期状态）：

$$
J(w) = \sum_{s \in S}^{} d_\pi (s) (v_\pi (s) - \hat{v}(s,w)) ^2
$$

### Optimization Algorithms

使用梯度下降方法：

$$
\begin{align*}
    w_{k+1} &= w_k - \alpha _k \nabla _w J(w_k) \\
    \nabla _w J(w) &= \nabla _w \mathbb{E} \left[ (v_\pi (S) - \hat{v} (S, w))^2 \right] \\
    &= \mathbb{E} \left[\nabla _w (v_\pi (S) - \hat{v} (S, w))^2 \right] \\
    &= 2 \mathbb{E}\left[ (v_\pi (S) - \hat{v} (S, w)) ( -\nabla _w \hat{v}(S, w) ) \right] \\
    &= -2 \mathbb{E}\left[ (v_\pi (S) - \hat{v} (S, w)) \nabla _w \hat{v}(S, w) \right]
\end{align*} 
$$

使用 SGD 消去期望：

$$
w_{t+1} = w_t + \alpha _t (v_\pi (s_t) - \hat{v}(s_t, w_{t}))\nabla _w \hat{v}(s_t, w_t)
$$

由于 $v_\pi (s_t)$ 未知，可以使用 MC 或者 [TD learning](Temporal-Difference-Learning.md) 来估计。其中 TD learning:

$$
w_{t+1} = w_t + \alpha _t [r_{t+1} + \gamma \hat{v}(s_t, w_t) - \hat{v}(s_t, w_t)]\nabla _w \hat{v}(s_t, w_t)
$$

伪代码：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409110928914.png)

- 现在 $\hat{v}(s,w)$ 可以用神经网络来计算。
- $\phi(s)$ 是 feature vector，类似于 SVM 中的。当选择 $\phi(s) = e^s \in \{0,1\}^{|S|}$ 时，就能将表格形式的 TD learning 和上述公式联系了（即上述公式的离散化）
- 严格来说，这个算法并不是在拟合最初的 objective function (Bellman error)，而是 projected Bellman error

## Sarsa with Function Approximation

$$
w_{t+1} = w_t + \alpha _t [r_{t+1} + \gamma \hat{q}(s_{t+1}, a_{t+1}, w_t) - \hat{q}(s_t, a_t, w_t)] \nabla _w \hat{q}(s_t, a_t, w_t)
$$

伪代码：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409110958943.png)

## Q-learning with Function Approximation

$$
w_{t+1} = w_t + \alpha _t \left[ r_{t+1} + \gamma \max _{a \in A(s_{t+1})} \hat{q} (s_{t+1}, a, w_t) - \hat{q}(s_t, a_t, w_t) \right] \nabla _w \hat{q}(s_t, a_t, w_t)
$$

伪代码：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409111003384.png)

## Deep Q-learning (DQN)

> 神经网络用于 nonlinear function approximation，DQN 一般用于状态无限（连续），动作有限（离散）的情况；是 offline 的算法

### Objective Function

$$
J(w) = \mathbb{E}\left[ \left( R + \gamma \max _{a \in A(S')} \hat{q}(S', a, w) - \hat{q}(S,A,w) \right)^2 \right]
$$

### Optimization

使用梯度下降，记 $y = R + \gamma \max _{a \in A(S')} \hat{q}(S', a, w)$，暂时假设 $y$ 中的 $w$ 是固定的。

引入两个网络 **main network** $\hat{q}(s,a,w)$ 和 **target network** $\hat{q}(s,a,w_T)$

$$
J(w) = \mathbb{E}\left[ \left( R + \gamma \max _{a \in A(S')} \hat{q}(S', a, w_T) - \hat{q}(S,A,w) \right)^2 \right]
$$

基本想法：使用 $w_T$ 进行对 $w$ 的一段时间优化后，再将 $w$ 的值赋给 $w_T$，反复进行。

$$
\nabla _w J(w) = \mathbb{E}\left[ \left( R + \gamma \max _{a \in A(S')} \hat{q}(S', a, w_T) - \hat{q}(S, A, w) \right) \nabla _w \hat{q}(s,a,w) \right]
$$

### Experience Replay

先进行一系列采样，然后从其中均匀随机选取样本进行训练，可以打破不同样本之间的 correlation。

因为实际训练时这些样本其实应该是没有关系的，但是根据一定的 policy 采集样本时不可避免的在时间上会有前后的 correlation，也就是 not uniformly collected。也即

1. 让样本满足独立同分布假设
2. 提高样本利用效率

### 伪代码

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409111033384.png)

- 这里由于是 off-policy 的，所以不用 policy update，只需要最后得出 value function 后计算一遍 policy 就行了
