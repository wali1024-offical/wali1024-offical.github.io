---
title: Mixing Time of Markov Chains
tags:
  - stochastic-progress
math: true
---

Mixing Time（混合时间） 定义：$\forall \epsilon > 0$，Markov Chain $P$ 的 mixing time 为：

$$
\begin{equation*}
    \tau_{\mathrm{mix}}(\epsilon):=\min_{t}\{\max_{\mu_0} D_{\mathrm{TV}}(\mu_t,\pi)\le \epsilon\}.
\end{equation*}
$$

也就是说，$\tau_{\mathrm{mix}}(\epsilon)$ 是使得无论任何初始分布，$t$ 步之后 $\mu_t$ 和 $\pi$ 的差距一定会小于 $\epsilon$ 的最小的 $t$。当 $\epsilon$ 未给出时，默认为 $\frac{1}{4}$。

由 [coupling](Lecture5-Coupling-Fundamental-Theorem-of-Markov-Chains.md) 可知：

$$
D_{\mathrm{TV}}(\mu_t,\pi) \leq \Pr_{(X,Y)\sim\omega_t}[X \neq Y].
$$

那么只需构造出一个 coupling $\omega_t$，使得 $\Pr_{(X,Y)\sim\omega_t}[X \neq Y]\le \epsilon$，就会有 $\tau_{\mathrm{mix}}(\epsilon) \le t$

## Example: Random walk on hypercubes

在 $n$ 维单位超立方体上随机游走，每次一步，且有 $1/2$ 概率原地不动，可以抽象为：

- 初始位置为 $X_0$
- 每个步 $t$，从 $\{1,2,\ldots,n\}, \{0,1\}$ 中分别均匀取样 $i$ 和 $b$
- 改变 $X_t$ 的第 $i$ 维为 $b$

构造一个 coupling $(X_t, Y_t)$，满足每一步两者共用 $i$ 和 $b$。显然有当 $i$ 在某一步被选中后，$X_t(i) = Y_t(i)$ 将在之后永远成立。这样原问题被转化为了奖券收集问题。那么，对于时刻 $t > n \log n + cn$，某一个（指定的）维度没有被选择的概率 $P$

$$
\begin{equation*}
    P \le\left(1-\frac{1}{n}\right)^{n\log n + cn}\leq \frac{e^{-c}}{n}.
\end{equation*}
$$

希望通过这个来为 $\tau_{\mathrm{mix}}$ 推出一个上界，自然希望

$$
\frac{e^{-c}}{n} \le \epsilon
$$

也就是 $c>\log \frac{1}{\epsilon}$。立即有 $\tau_{\mathrm{mix}}(\epsilon) \le n\log n + cn \rightarrow n \log \frac{n}{\epsilon}$