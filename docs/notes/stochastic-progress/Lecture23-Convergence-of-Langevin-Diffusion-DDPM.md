---
title: Convergence of Langevin Diffusion, DDPM
tags:
  - stochastic-progress
math: true
---
## Convergence of Langevin diffusion

- Langevim diffusion 见 [lecture19](Lecture19-Informal-Definition-of-Diffusion-and-Examples.md)
- $f: \mathbb{R}^d \rightarrow \mathbb{R}$, a **$\mu$-strongly convex function**

记 $\{ X_t\}_{t\ge0}$ 和 $\{ Y_t\}_{t\ge0}$ 为两个过程，由

$$
\begin{cases}
    \mathrm{d}X_t &= - \nabla f(X_t) \mathrm{d}t + \mathrm{d}B_t \\
    \mathrm{d}Y_t &= - \nabla f(Y_t) \mathrm{d}t + \mathrm{d}B_t \\
\end{cases}
$$

生成，且 $Y_0$ 从稳态分布 $\pi \propto e^{-f}$ 中取样，$X_0$ 则是随机取样。

使用同样的 SBM 耦合 $X_t$ 和 $Y_t$，则

$$
\frac{\mathrm{d}(X_t - Y_t)}{\mathrm{d}t} = \nabla f(Y_t) - \nabla f(X_t) \\
$$

进而

$$
\begin{align*}
    \frac{\mathrm{d}\mathbf{E}\left[\left|\left|X_t - Y_t\right|\right|^2\right]}{\mathrm{d}t} &= 2 \mathbf{E}[\langle \frac{\mathrm{d}(X_t - Y_t)}{\mathrm{d}t}, X_t - Y_t \rangle] \\
    &= 2 \mathbf{E}[\nabla f(Y_t) - \nabla f(X_t), X_t - Y_t] \\
    & \le -2 \mu \mathbf{E}\left[||X_T - Y_t||^2\right]
\end{align*} 
$$

最终

$$
\mathbf{E}[||X_t - Y_t||^2] \le e^{-2\mu t}\mathbf{E}[||X_0 - Y_0||^2]
$$

即收敛速度挺快。

## DDPM (Denoising Diffusion Probabilistic Modeling)

**Main Idea**: 对最初抽样出的 $X_0 \sim \pi$ 增加 Gaussian 噪声，直到分布接近 Gaussian。然后再从得到的 Gaussian 分布中抽一个样，运行反向过程得到一个从目标分布中取出的样本。

**具体**：

- 从 $X_0 \sim \pi$ 开始进行一个 Ornstein-Uhlenbeck process (OU process)，得到 $\{ \overline{X}_t\}$，满足 $\overline{X}_t = e^{-t}X_0 + \sqrt{1 - e^{-2t}}Z_t, Z_t \sim \mathcal{N}(0, 1)$ 为一个标准高斯函数。共进行 $T$ 次，得到一列随机变量 $\{ \overline{X}_t\}_{t \le T}$
- 记 $q_t$ 为 $\overline{X}_t$ 的分布。在反向过程中
  - 首先取样出 $\tilde{X}_T$
  - 接着方向计算 $\{ \tilde{X}_t\}$：$\mathrm{d}\tilde{X}_t = \left( \tilde{X}_t + 2 \nabla \log q_{T-t}\left( \tilde{X}_t \right) \right) \mathrm{d}t + \sqrt{2} \mathrm{d}B_t$。这个 OC 反向过程推导较麻烦，请参看课程 note

加噪过程采用数学规律，而去噪过程使用神经网络学习评分函数 $\nabla \log q_{T-t}(\tilde{X}_t)$（因为 $q$ 这些分布是未知的，需要被学习）。

实际上不用每个 $X_t$ 和 $\overline{X}_t$ 都需要被算出，因为公式中它们没有依赖关系（除 $X_0$ 外），况且 $t$ 连续时也算不完。

### Score Matching

可以用神经网络来训练得出 score function $\nabla \log q_{T-t}(\tilde{X}_t)$，记 $F$ 为可选函数空间，需要找到：

$$
\arg\min_{S_t \in F} \mathbf{E}_{x \sim q_t}\left[ ||S_t(x) - \nabla \log q_{t}(\tilde{X}_t) ||^2 \right]
$$

由于 $\nabla \log q_{t}(\tilde{X}_t)$ 和 $S_t(x)$ 无关，有

$$
\\arg\min_{S_t \in F} \mathbf{E}_{x \sim q_t}\left[ ||S_t(x) - \nabla \log q_{T-t}(\tilde{X}_t) ||^2 \right] = \arg\min_{S_t \in F}\mathbf{E}_{x\sim q_t}\left[ ||S_t(x)||^2 - 2\langle S_t(x), \nabla \log q_{t}(\tilde{X}_t) \rangle \right]
$$

可以证明，上式等价于

$$
\arg\min_{S_t \in F}\mathbf{E}_{x \sim q_t}\left[ \left\lVert S_t(x) - \frac{Z_t}{1 - e^{-2t}} \right\rVert^2 \right]
$$