---
tags:
  - DL
title: Bayesian-networks
date created: 星期六, 九月 21日 2024, 10:46:45 上午
date modified: 星期三, 九月 25日 2024, 9:21:55 晚上
math: true
---

基于 directed acyclic graph (DAG) $G=(V, E)$，满足

- 用节点 $i$ 代表随机变量 $X_i$
- 每个节点有一个 conditional probability distribution (CPD) $p(x_i | x_{\mathrm{Pa}(i)})$，也就是说每个节点处的概率分布取决于父节点的概率

$$
p(x_1, \ldots, x_n) = \prod_{i \in V} p(x_i | x_{\mathrm{Pa}(i)})
$$

例子：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/Screenshot%20from%202024-09-21%2010-53-41.png)

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/Screenshot%20from%202024-09-21%2011-10-08.png)

可以用 Bayesian network 来直观理解 discriminative 和 generation model：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/Screenshot%20from%202024-09-21%2011-01-00.png)
