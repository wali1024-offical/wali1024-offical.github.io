---
tags:
  - RL
title: Monte Carlo Learning
math: true
---

主要思想：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409082143645.png)

也就是用**大量重复的取样**来代替 model（model free）

可以将蒙特卡洛方法引入 [policy iteration](Value-or-Policy-Iteration.md)，代替计算 $q_{\pi_k}$ 的步骤：

$$
q_{\pi_k}(s,a) = \mathbb{E}\left[G_t | S_t = s, A_t = a\right]
$$

从 $(s,a)$ 出发，大量采样 $G_t$，从而进行估计。

## MC Basic

伪代码：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409090827477.png)

- MC basic 是 policy iteration algorithm 的一个变体
- model-free algorithm 是在 model-based algorithm 基础上构建的
- MC basic 效率很低，无法实用
- 当 $G_t$ 的 episode length 较小时，只有靠近 target 的 state 会学习到较好的策略；随着 length 增大，较远的 state 也能找到最优策略。

## MC Exploring Starts

main idea: 重复利用每一条 episode。

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409090852712.png)

伪代码：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409090900060.png)

## MC without Exploring Starts

由于实际中不一定每一个 start 都能被设置（或者因为太麻烦），所以可以采用 *soft policy*：因为采取任意一个 action 的概率都是 positive 的。这样只需要在几个 start 开始取样足够长的 episode 就可以覆盖到其它的 state。

可以使用 $\epsilon$-greedy 算法：

$$
\pi(a|s) = \begin{cases}
    1 - \frac{\epsilon}{|\mathcal{A}(s)}(|\mathcal{A}(s) -1|), & \text{for greedy action} \\
    \frac{\epsilon}{|\mathcal{A}(s)|}, & \text{for the orhter}\space |A(s)| - 1 \space \text{actions}
\end{cases} 
$$

这里 $\epsilon \in [0,1]$。这样就可以 **balance exploitation and exploration**。

- $\epsilon = 0$：完全 greedy
- $\epsilon = 1$：成为 uniform distribution, exploration 最强

在之前的 policy iteration 中，policy improvement:

$$
\pi_{k+1}(s) = \operatorname{argmax}_{\pi \in \Pi} \sum_{a \in \mathcal{A}}^{} \pi(a|s)q_{\pi_k}(s,a)
$$

这里 $\Pi$ 是所有可能的策略集合。可以将 $\Pi$ 限制为所有 $\epsilon$-greedy 策略的集合 $\Pi_\epsilon$，从而引入 $\epsilon$-greedy 算法：

$$
\pi_{k+1}(s) = \operatorname{argmax}_{\pi \in \Pi_\epsilon} \sum_{a \in \mathcal{A}}^{} \pi(a|s)q_{\pi_k}(s,a)
$$

可以得到，这样的更新结果为：

$$
\pi_{k+1}(a|s) = \begin{cases}
    1 - \frac{|\mathcal{A}(s)| - 1}{|\mathcal{A}(s)|}\epsilon , & a = a^*_k \\
    \frac{1}{|\mathcal{A}(s)|} \epsilon, & a \neq a^*_k
\end{cases} 
$$

伪代码：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409090928525.png)

优势/劣势：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409090938124.png)
