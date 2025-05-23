---
tags: [flow-model, generative-model]
title: Flow-Models
date created: 星期六, 九月 21日 2024, 11:30:07 晚上
date modified: 星期三, 九月 25日 2024, 10:21:04 晚上
---

## Key Idea

> [!note] Key Idea
> Map **simple** distributions (easy to sample and evaluate densities) to **complex** distributions through an **invertible transformation**

和 Variational Autoencoders 相似：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@images/Screenshot%20from%202024-09-22%2008-47-38.png)

## Change of Variables Formula

> [!note] Change of Variables formula
> (1D case) If $X=f(Z)$ and $f(\cdot)$ is monotone with inverse $Z=f^{-1}(X)=h(X)$, then:
> 
> $$
> p_{X}(x)=p_{Z}(h(X))|h'(X)|=\frac{p_{Z}(z)}{f'(z)}
> $$

（证明：

$$
P(X \le x)=P(f(Z) \leq x) = P(Z \leq f^{-1}(x))=P(Z \leq h(x))
$$

对 $x$ 求导：

$$
p_{X}(x)=p_{Z}(h(x))|h'(x)|
$$

证毕）

> [!note] Change of Variables formula
> (General case) The mapping between $Z$ and $X$, given by $f:\mathbb{R}^n \to \mathbb{R}^n$, is invertible such that $X=f(Z)$ and $Z=f^{-1}(X)$. Then
> 
> $$
> p_{X}(x)=p_{Z}(f^{-1}(x))\left| \det \left( \frac{\partial f^{-1}(x)}{\partial x} \right) \right|
> $$

- 和 VAE 不同，$x$ 和 $z$ 都需要是连续的，并且维度相同
- 如果 $f$ 是可逆的线性变换，则

$$
p_{X}(x)=p_{Z}(f^{-1}(x))\left| \det \left( \frac{\partial f(z)}{\partial z} \right) \right|^{-1}
$$

## Normalizing Flow Model

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@main/Screenshot%20from%202024-09-22%2009-12-11.png)

### Flow of Transformations

应用一系列如上图的变换，使得从最初的简单分布到最后的复杂分布的过程更容易:

$$
z_{m}=f_{\theta}^m \circ \cdots \circ f_{\theta}^1(z_{0}) := f_{\theta}(z_{0})
$$

此时 change of variable 公式变为

$$
p_{X}(x;\theta)=p_{Z}(f_{\theta}^{-1}(x))\prod_{m=1}^{M}\left| \det \left( \frac{\partial (f_{\theta}^m)^{-1}(z_{m})}{\partial z_{m}}\right) \right|
$$

### Learning and Inference

仍然使用 [Maximum-Likelihood-Learning](Maximum-Likelihood-Learning.md):

$$
\max_{\theta}\log p_{X}(\mathcal{D};\theta)=\max_{\theta }\sum_{x \in \mathcal{D}} \left( \log p_{Z}(f_{\theta}^{-1}(x)) + \log \left| \det \left(\frac{ \partial f_{\theta}^{-1}(x) }{ \partial x } \right) \right| \right)
$$

$$
\begin{align}
\nabla_{\theta}P_{X}(x;\theta) &=\nabla_{\theta}\left( \log p_{Z} \left(f_{\theta}^{-1}(x)\right) + \log \left| \det \left( \frac{ \partial f_{\theta}^{-1}(x) }{ \partial x }  \right)  \right|  \right) \\
 & = \frac{1}{p_{Z}(f_{\theta}^{-1}(x))} \nabla_{\theta}p_{Z}\left(f_{\theta}^{-1}(x)\right) + \frac{1}{\left| \det\left(\frac{ \partial f_{\theta}^{-1}(x) }{ \partial x } \right) \right| } \nabla_{\theta} \left| \det \left( \frac{ \partial f_{\theta}^{-1}(x) }{ \partial x }  \right)  \right| 
\end{align}
$$

使用正向的 $f$ 和 逆向的 $f^{-1}$ 就可以完成类似于 VAE 中 encoder-decoder 的任务

### Desiderata

- latent variable 和 data 都要是连续的，且维数相同
- 简单的先验分布 $p_{Z}(z)$，例如各向同性的高斯分布
- 可逆、高效的变换 $f$
- Determinant 结构特殊，计算更高效（否则计算 determinant 的复杂度为 $O(n^3)$），例如三角阵：设 $x=(x_{1}, x_{2}, \dots,x_{n})=f(z)=(f_{1}(z),\dots,f_{n}(z)), z=(z_{1},\dots,z_{n})$ 如果 $x_{i}$ 仅依赖于 $z_{<i}$，则为下三角阵

$$
J=\frac{ \partial f }{ \partial z } = \begin{pmatrix}
\frac{ \partial f_{1} }{ \partial z_{1} } & \cdots & \frac{ \partial f_{1} }{ \partial z_{n} }  \\
\dots  & \dots & \dots \\
\frac{ \partial f_{n} }{ \partial z_{1} } & \dots & \frac{ \partial f_{n} }{ \partial z_{n} }  
\end{pmatrix} = \begin{pmatrix}
\frac{ \partial f_{1} }{ \partial z_{1} } & \cdots & 0  \\
\dots  & \dots & \dots \\
\frac{ \partial f_{n} }{ \partial z_{1} } & \dots & \frac{ \partial f_{n} }{ \partial z_{n} }  
\end{pmatrix}
$$
