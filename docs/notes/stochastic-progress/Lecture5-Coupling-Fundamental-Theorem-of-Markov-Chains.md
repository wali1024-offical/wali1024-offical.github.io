---
title: Coupling, Fundamental Theorem of Markov Chains
tags:
  - stochastic-progress
math: true
---

## Total Variation Distance (TVD)

对于两个定义在可数状态集合 $\Omega$ 上的概率分布 $\mu$ 和 $\nu$，定义

$$
D_{VT}(\mu, \nu) = \frac{1}{2}\sum_{x \in \Omega}|\mu(x) - \nu(x)|
$$

如图：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/202408201806361.png)

还可以证明

$$
D_{VT}(\mu(x), \nu(x)) = \max_{A \subseteq \Omega}|\mu(A) - \nu(A)|
$$

这里

$$
\mu(A) = \sum_{x \in A}\mu(x)
$$

## Coupling (耦合)

**定义**：对于在同一个样本空间 $\Omega$ 上的分布 $\mu$ 和 $\nu$，如果在样本空间 $\Omega \times \Omega$ 上的分布 $\omega$ 满足，$\mu$ 和 $\nu$ 分别为 $w$ 的边缘概率分布，那么称 $\omega$ 为 $\mu$ 和 $\nu$ 的耦合（coupling）

一个简单的例子：当 $\Omega$ 为有限集时，取非负概率矩阵 $\omega$ 满足，每一行的和与 $\mu$ 相同，每一列的和与 $\nu$ 相同。例如：

$$
\begin{align*}
    \mu &= [1/2, 1/2]^T \\
    \nu &= [1/3, 2/3]^T \\
    \omega &= \begin{pmatrix}
        1/3 & 1/6 \\
        0 & 1/2
    \end{pmatrix}
\end{align*}
$$

就有 $\omega$ 是 $\mu$ 和 $\nu$ 的 coupling

## Coupling Lemma

### 内容

$\mu$ 和 $\nu$ 是同一个样本空间 $\Omega$ 上的分布，则对于它们的任意一个耦合 $\omega$ 来说，有

$$
\mathrm{Pr}_{(X,Y)\sim \omega}[X \ne Y] \ge D_{TV}(\mu, \nu)
$$

并且存在一个 $\omega^*$ 使得上式取等。

### 一种理解

在 $\Omega$ 有限的情况下，左式表示 $\omega$ 的非对角线元素之和，右式为两分布的差异。

### 证明

由于

$$
\begin{align*}
    \mathrm{Pr}[X = Y] &= \sum_{t \in \Omega}\mathrm{Pr}[X = Y = t] \\
    &\le \sum_{t \in \Omega} \min(\mu(t), \nu(t))
\end{align*}
$$

故

$$
\begin{align*}
    \mathrm{Pr}[X \ne Y] &= 1 - \mathrm{Pr}[X = Y] \\
    &\ge 1 - \sum_{t \in \Omega} \min(\mu(t), \nu(t)) \\
    & = \sum_{t \in \Omega} (\mu(t) - \min(\mu(t), \nu(t))) \\
    &= \max_{A \subseteq \Omega}\{\mu(A) - \nu(A)\} \\
    &= D_{TV}(\mu, \nu)
\end{align*}
$$

构造 $w^*$：对于任意一个 $t \in \Omega$, 定义 $\mathrm{Pr}_{(X,Y)\sim \omega}[X=Y=t] = \min\{\mu(t), \nu(t)\}$

得证。

## Coupling of Markov Chains

有两个 Markov Chain $\mu_0, \mu_1, \ldots$ 和 $\nu_0, \nu_1, \ldots$。对每一个 $t$，记 $\omega_t$ 为 $\mu_t$ 和 $\nu_t$ 的 coupling，$(X_t, Y_t) \sim \omega_t$。此外，当从 Markov Chain $\omega = (\omega_1, \omega_2, \ldots)$ 中取样出两个序列 $(X_1, X_2, \ldots)$ 和 $(Y_1, Y_2, \ldots)$ 时满足，它们分别初始分布为 $\mu$ 和 $\nu$ 的 Markov chain.

则称 $w$ 为 $\mu$ 和 $\nu$ 的耦合。因此，Markov chain 的耦合比两个随机变量的耦合的条件要强一些。

## Foundamental Theorem of Markov Chains (Finite)

### 内容

如果一个 finite Markov Chain 既是 *irreducable* 的又是 *aperiodic* 的，那么三个问题的答案都是都是成立的（即存在、唯一，且一定收敛）。

### 证明

**引理**：正整数 $c_1, c_2, \ldots, c_s$ 满足 $\gcd(c_1, c_2, \ldots, c_s) = 1$，则对于充分大的整数 $b$，方程

$$
c_1y_1 + c_2 y_2 + \cdots c_s y_s = b.
$$

存在正整数解 $y_1, y_2, \ldots, y_s$

利用这个引理，可以证明：对于任意一个 irreducible 且 aperiodic 的 Markov chain $P \in \mathbb{R} \times \mathbb{R}$，有

$$
\begin{equation*}
    \exists\, t^*: \forall\, i, j\in [n]:   \quad P^{t^*}(i, j) > 0\,.
\end{equation*}
$$

最后，就可以证明原定理：设 $(X_t \sim \mu_0)$ 和 $(Y_t\sim \pi)$ 是两个服从相同转移方程的 markov chain，这里 $\pi$ 为 stationary distribution：

$$
\begin{align*}
\begin{array}{c}
    \mu_0 & & \mu_1 & &  & & & & \mu_t & & & & \cr
    \wr &    &  \wr & &  & & & & \wr & &  & & \cr
    X_0 & \to &  X_1 & \to & X_2 & \to & \cdots &  \to & X_t & \to & X_{t + 1} & \to & \cdots\cr
    & & & & & & & & & & & & \cr
    Y_0 & \to &  Y_1 & \to & Y_2 & \to & \cdots &  \to & Y_t & \to & Y_{t + 1} & \to & \cdots\cr
    \wr &    &  \wr & &  & &  & & \wr & & & & \cr
    \pi & & \pi & &  & & & & \pi & & & &
\end{array}
\end{align*}
$$

为了证明原定理，只需构造出一个服从 $(\mu_t)_t$ 和 $(\pi)_t$ 的耦合 $(\omega_t)_t$ 的序列 $(X_t, Y_t)_t$，再计算 $\Pr_{(X_t,Y_t)\sim \omega_t}[X_t \neq Y_t]$

归纳构造 Markov chain：任意取 $X_0 \sim \mu_0$ 和 $Y_0 \sim \pi$。当已取出 $(X_t, Y_t)$ 时，如果

- $X_t = Y_t$：对任意 $t' > t$，取 $X_{t'} = Y_{t'}$ （因为这意味着两个链相遇了，在之后的过程中必然一同前进）
- $X_t \ne Y_t$：依照 $X_t$ 正常取出 $X_{t+1}$ 即可，$Y_{t+1}$ 同理。

对于之前得出的 $t^*$，存在 $\delta$ 满足 $P^{t^*}(i,j) \ge \delta \gt 0$。可以证明：$\Pr[X_{t^*}\neq Y_{t^*}]\leq 1-\delta^2$（考虑到 $\Pr[X_{t^*} = c] \ge \delta$ 和 $\Pr[Y_{t^*} = c] \ge \delta$ 易证，这里 $c \in \Omega$ 是任取的一个常状态）。紧接着可以得到：

$$
\begin{align*}
    \Pr[X_{2t^*}\neq Y_{2t^*}]&= \Pr[X_{2t^*}\neq Y_{2t^*}| X_{t^*}=Y_{t^*}]\cdot \Pr[X_{t^*}=Y_{t^*}] \\
    &\quad \quad+ \Pr[X_{2t^*}\neq Y_{2t^*}| X_{t^*}\neq Y_{t^*}]\cdot \Pr[X_{t^*}\neq Y_{t^*}] \\
    &= \Pr[X_{2t^*}\neq Y_{2t^*}| X_{t^*}\neq Y_{t^*}]\cdot \Pr[X_{t^*}\neq Y_{t^*}] \\
    &\leq (1-\delta^2)^2.
\end{align*}
$$

以此类推，可以得到 $\mathrm{Pr}[X_{kt^*} \ne Y_{kt^*}]$ 会收敛于 $0$。

又可以证明引理：对任意 $t \ge 0$，$D_{TV}(\mu_{t+1},\pi)\le D_{TV}(\mu_t,\pi)$（考虑构造 coupling 使得 $D_{TV}(\mu_{t+1}, \pi) \le \mathrm{Pr}_{(X, Y) \sim w_{t+1}}[X \ne Y] \le \mathrm{Pr}_{(X, Y)\sim w_{t}^*}[X \ne Y] = D_{TV}(\mu_t, \pi)$）。进而原定理得证。
