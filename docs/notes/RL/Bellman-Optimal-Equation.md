---
tags:
  - RL
title: Bellman Optimal Equation
math: true
---

## Bellman Optimal Equation

比较两个 policy 的好坏：如果对任意 state $s$，都有

$$
v_{\pi_1}(s) \ge v_{\pi_2}(s)
$$

那么称 $\pi_1$ 比 $\pi_2$ 更好。然后定义 **optimal policy**：如果 policy $\pi^*$ 满足：

$$
\forall \pi, s \in \mathcal{S}, \pi^*(s) \ge \pi(s)
$$

那么称 $\pi^*$ 为最优的（optimal）

**Bellman optimal equation (BOE)**：

单元素形式：

$$
\begin{align*}
    v(s) &= \max_{\pi}\sum_{a \in A}^{} \pi(a | s)\left( \sum_{r}^{}p(r|a,s)r + \gamma \sum_{s' \in \mathcal{S}}^{}p(s'|s,a)v(s') \right), \forall s \in \mathcal{S} \\
    &:= \max_{\pi} \sum_{a \in A}^{} \pi(a|s) q(s,a)
\end{align*} 
$$

在求解 value function $v(s)$ 时，$p(r|s,a), p(s'|s,a)$ 都是已知的。

矩阵形式：

$$
v = \max_{\pi}(r_\pi + \gamma P_\pi v)
$$

这里

$$
\begin{align*}
    [r_\pi]_s &= \sum_{a \in A}^{} \pi(a|s)\sum_r p(r|a,s)r  \\
    [P_\pi]_{s,s'} &= p(s'|s) :=\sum_{a \in A}^{} \pi(a|s) p(s'|s, a)
\end{align*} 
$$

且 $\max _\pi$ 是 element-wise 的

求解 BOE 实际上是一个受约束的线性优化问题：先固定 $v$，这样 $v(s), q(s,a)$ 就都已知了，那么权重 $\pi(a|s)$ 的和为 1 且非负，当右式取最大值是，必然所有权重都压在最大的一个 $q(s,a)$ 上，即 $\pi (a^{*}|s) = 1, a^{*} = \operatorname{argmax} _{a} q(s,a)$，这样就可以解出 $\pi$，再代入 $\pi$，**只剩下一个向量 $v$ 需要被解出**。（实际上这时已经转化为 Bellman equation 的形式了，可以用解析解、动态规划、蒙特卡洛方法等来求解）

## Contraction Mapping Theorem

定义 **contractive function**: 如果 $f$ 满足存在常量 $\gamma \in (0,1)$，使得

$$
\Vert f(x_1) - f(x_2) \Vert \le \gamma \Vert x_1 - x_2 \Vert
$$

对任意 $f$ 定义域中的 $x_1$ 和 $x_2$ 成立，这里 $\Vert\cdot \Vert$ 可以为任意向量范数

**Contraction Mapping Theorem**：对 contractive function $f$，$f$ 有唯一不动点 $x^*$，且对任意序列 $\{x_k \}$ 满足 $x_{k+1} = f(x_k), x_k \rightarrow x^*$，该序列以**指数**速度收敛。

## Solution of BOE

由于可以令 $f(v) = \max _\pi(r_\pi + \gamma P_\pi v)$，BOE 转化为 $v = f(v)$

可先证明：$\Vert f(v_1) - f(v_2)\Vert \le \gamma \Vert v_1 - v_2 \Vert$，这里 $\gamma $ 是 discounting rate.

那么就可以用迭代 $v_{k+1} = f(v_k)$ 的方式来收敛到 BOE 的解 value function $v^*$ 了。

每次更新，迭代方程为：

$$
v_{k+1}(s) = \max_a q_k(s,a)
$$

最后，由 $v^*$ 得出对应的 policy $\pi^*$: 

$$
\pi^*(a|s) = \begin{cases}
    1, & a = \operatorname{argmax}a q^*(a,s) \\
    0, & \text{otherwise}
\end{cases}
$$

则可知 $\pi^*$ deterministic and greedy

## Analyzing Optimal Policies

- 当 $\gamma$ 更大时，agent 更“远视”；而更小时，agent 更“近视”
- 对已知的奖励 $r$，$\pi^*$ 只与奖励之间的“差距”有关，而和绝对值无关。也就是，如果对每一个奖励 $r$ 都进行一个相同的仿射变换 $ar+b$，那么新的 optimal state value $v'^*$ 也是原有 $v^*$ 的仿射：

$$
v'^* = a v^* + \frac{b}{1-\gamma}\mathbf{1}
$$
