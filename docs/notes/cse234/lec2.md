---
title: " Basics: Modern DL, computational graph, frameworks"
tags: [AI, LLM]
math:
modified: 星期三, 三月 5日 2025, 4:03:30 下午
---

## DL Overview

一个模型可以如下表示：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@images/20250305145821809.png)

三个重要概念：

- **Data**: image, text, audio, table…
- **Model**: CNN, RNN, Transformer, MoE
- **Compute**: CPU, GPU, TPU, LPU

> Matmul (plus softmax) is all you need 

| Model        | Matmul | Softmax | Others                |
| ------------ | ------ | ------- | --------------------- |
| CNN          | +      | +       | Conv, ReLU, BatchNorm |
| RNN          | +      | -       | Sigmoid, Tanh         |
| Transformers | +      | +       | GeLU, LayerNorm       |
| MoE          | +      | +       | -                     |

## Dataflow Graph

例如

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@images/20250305151813172.png)

有两种：symbolic 和 imperative。前者如 tensorflow，是 define-then-run 的；后者如 Pytorch，是 define-and-run 的

- Symbolic：易于优化，效率更高；但编程反直觉、难以 debug，更不灵活
- Imperative：更灵活，易于编程和 debug；但不易优化，效率更低

可以使用 `@torch.compile` 来提前编译 torch 代码得到 **JIT**，以提高效率，但更难 debug

还可以分为 static 和 dynamic 的（如 CNN 和 RNN）

- Static：只需定义一次、优化一次
- Dynamic：难以表示复杂的控制逻辑，难实现计算图，难 debug
    - 解决方案：忽略表现；引入 control flow ops；分片段编译

Control flow 原型：

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@images/20250305155008501.png)

例如

![](https://cdn.jsdelivr.net/gh/KinnariyaMamaTanha/Images@images/20250305155211942.png)
