---
tags:
  - RL
title: Value or Policy Iteration
math: true
---

## Value Iteration

在 [chapter 3](Bellman-Optimal-Equation.md) 中，求解 Bellman optimality equation 的方法就是 value iteration：

1. **Policy Update**：solve $\pi_{k+1} = \operatorname{argmax}_\pi(r_\pi + \gamma P_\pi v_k)$
2. **Value Update**: $v_{k+1} = r_{\pi_{k+1}} + \gamma P_{\pi_{k+1}}v_k$

注意，这里的 $v_k$ 并不是 state value，而是为了逼近 $v^*$ 而构造的向量。

用 element-wise 的写法：

- **Policy Update (PU)**: 

$$
\pi_{k+1}(a|s) = \begin{cases}
    1 & a = a^*(s) \\
    0 & a \neq a^*(s) \\
\end{cases} 
$$

- **Value Update (VU)**:

$$
v_{k+1}(s) = \max _a q_k(a, s)
$$

伪代码：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409082031937.png)

实际上，value iteration 一般直接对 value 进行更新（隐式地维护策略），

$$
V_{k+1}(s) = \max_{a}\sum_{s'}P(s'\mid s, a) \left[ R(s,a,s') + \gamma V_{k}(s') \right] 
$$
到最后接近收敛时才推算出 policy，这样效率更高：

$$
\pi^{*}(s) = \arg\max_{a}\sum_{s'}P(s'\mid s,a)\left[ R(s,a,s') + \gamma V^{*}(s') \right] 
$$

由于直接对值函数进行更新，所以更适合大规模的任务，并且还可以在 [Value-Function-Approximation](Value-Function-Approximation.md) 中进行扩展

## Policy Iteration

给出一个初始 policy $\pi_0$

1. **Policy Evaluaton (PE)**：计算 $\pi_k$ 的 value state $v_{\pi_k} = r_{\pi_k} + \gamma P_{\pi_k} v_{\pi_k}$。需要用到 Bellman equation 的求解
2. **Policy Improvement (PI)**：$\pi_{k+1} = \operatorname{argmax}_\pi\left(r_\pi + \gamma P_\pi v_{\pi_k} \right)$

流程：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409082043181.png)

可以证明，这样反复迭代后，$v_{\pi_k}$ 越来越好，最终会收敛到 $v^*$

并且，这里的 $v_{\pi_k}$ 是 value state，和 value iteration 中不一样.

由于每次迭代都需要对整个 policy 进行评估，所以在较大规模的任务上会效率较低

## Truncated Policy Iteration

> Value/Policy Iteration 是 truncated policy iteration 的两个特殊情况

Value iteration 和 policy iteration 非常类似：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409082117165.png)

其实在 value iteration 中，只是将 Bellman equation 的迭代求解进行了一步，从 $v_0$ 迭代到 $v_1$，而在 policy iteration 中，则是迭代无穷步得到了 value state $v_{\pi_1}$。

这两者都是极端情况，且后者实际中有可能不能实现，往往只会进行有限步，即为 truncated policy iteration。

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409082128474.png)

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409082127002.png)

伪代码：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409082129390.png)

可以证明：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409082131748.png)

因此，迭代过程中，总会越来越好，不用担心变差的情况。

几种算法的对比：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202409082133108.png)
