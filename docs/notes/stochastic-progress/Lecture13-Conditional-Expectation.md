---
title: Conditional Expectation
tags:
  - stochastic-progress
math: true
---
## Notations

对于 $(\Omega, \mathcal{F}, P)$:

- $\sigma(S)$，其中事件 $S \subseteq 2 ^\Omega$：包含 $S$ 的最小 $\sigma$ 代数
- 对 $A \in \mathcal{F}$，如果 $\forall B \subset A \space (B \neq \emptyset), B \not\in \mathcal{F}$，则称 $A$ 是 minimal 的
    - 任意两个不同的 minimal set 都是不交的
- 对随机变量 $X$ 和 $\mathcal{F}$，若满足对任意 $r \in \mathbb{R}$，有 $X^{-1}((-\infty, r)) := \{ \omega \in \Omega \mid X(w) < r \} \in \mathcal{F}$（即事件 $\{ X < x\} \in \mathcal{F}$），那么称 $X$ is measurable by $\mathcal{F}$
    - $\sigma(X)$：最小的能使 $X$ measurable 的 $\sigma$ 代数
    - 对值域为 $\mathbb{Z} \cup\{ \infty, -\infty \}$ 的随机变量 $X$，$\sigma(X)=\sigma\{\{X^{-1}(\{k\})\mid k\in \mathbb{Z}\}\cup\{X^{-1}(\{-\infty\})\}\cup\{X^{-1}(\{+\infty\})\}.$

## 定义

对**事件 $A \in \mathcal{F}$** 满足 $P(A) > 0$，以及随机变量 $X$，

$$
E[X|A] = \sum_{a}^{}a \cdot P(X|A = a)
$$

对 **$\Omega$ 的 $\sigma$ 代数** $\mathcal{G}$，更进一步定义为 $\mathcal{G} = \sigma (\{ A_k: k \in I \})$，其中 $\{ A_k\}$ 是 $\Omega$ 的一个分划，$P_{A_k} > 0$。记 $A(\omega)$ 为包含 $\omega \in \Omega$ 的 $A_k$，则 $E[X|\mathcal{G}]$ 是一个 $\mathcal{G}$ measurable 随机变量，满足

$$
\forall \omega \in \Omega, \space E[X|\mathcal{G}](\omega) = E[X|A(w)]
$$

对**随机变量 $Y$**,

$$
E[X\mid Y] := E[X \mid \sigma(Y)]
$$

同上有

$$
E[X \mid Y](\omega) = E[X \mid Y = Y(w)]= E[X\mid Y^{-1}(Y(\omega))]
$$

## 性质

对 $\sigma$ 代数 $\mathcal{G}_1$ 和 $\mathcal{G}_2$，若 $\mathcal{G}_1 \subseteq \mathcal{G}_2$，则

$$
E[E[X \mid \mathcal{G}_1] \mid \mathcal{G}_2 ] = E[E[X \mid \mathcal{G}_2] \mid \mathcal{G}_1 ] = E[X \mid \mathcal{G}_1]
$$

可以马上得到，对任意随机变量 $Y$:

$$
E[X] = E[X \mid \{ \emptyset, \Omega\}] = E[E[X \mid \sigma (Y)] \mid \{ \emptyset, \Omega\}] = E[E[X \mid Y]]
$$

- $E[aX+bY\mid \mathcal{G}] = a E[X\mid \mathcal{G}] + b E[Y\mid \mathcal{G}]$
- 如果 $X$ 是 $\mathcal{G}$ measurable 的，那么 $E[X \mid \mathcal{G}] = X$
- 如果 $X$ 是 $\mathcal{G}$ measurable 的，那么 $E[XY \mid \mathcal{G}] = XE[Y \mid \mathcal{G}]$