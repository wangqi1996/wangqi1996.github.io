---
title: Modeling Past and Future for Neural Machine Translation
date: 2019-10-13 20:25:45
tags: 2019-10-7 论文阅读
---

## 题目

### Modeling Past and Future for Neural Machine Translation

## 启发

本文指出当前decoder扮演了多个角色，通过将不同的共功能分离出去，可以提高模型的性能，可以考虑一下decoder具体还有哪些功能，可以将这些功能单独建模实现嘛？

<!-- more -->

------

## 总结

该论文解决**under-translation**和**over-translation**的问题，通过对已经翻译（**past**）的内容和还未翻译（**future**）的内容单独建模，分别为Past Layer 和 Future Layer，然后将他们分为集成到attention model和decoder state的计算中。作者提出该方法不仅可以提供model哪些内容翻译了，哪些内容没翻译的信息，还会给模型一个全局的视角。

论文指出，decoder承担了三个责任：

1. LM，保证流畅性
2. 建模最相关的source words（present）
3. 建模已经翻译过的source（past） 和 未翻译过的source（future）

attention的context vector记为c，future layer的更新通过减去c，past layer层的更新通过加上c。

![1570973926890](/image/1570973926890.png)

#### modeling future

![1570973903700](/image/1570973903700.png)

我们提出了三种计算F的model，期望能更好的建模减去的操作。

![1570974045642](/image/1570974045642.png)

**GRU**

使用标准的GRU做为F

![1570974102536](/image/1570974102536.png)

**GRU with Outside Minus (GRU-o）**

![1570974175168](/image/1570974175168.png)

直接建模减去操作

![1570974211751](/image/1570974211751.png)

#### model past

![1570974266468](/image/1570974266468.png)

注意这里是直接使用了GRU

#### modeling PAST and FUTURE（集成到模型中）

![1570974323615](/image/1570974323615.png)

首先，本文直接使用了context vector 而不是context weight，其次，本文不仅仅更新了attention model，该更新了decoder state的计算。

#### training

根据以下公式推导:

![1570979844448](/image/1570979844448.png)

![1570979863911](/image/1570979863911.png)

![1570982351602](/image/1570982351602.png)

最终的损失函数计算公式为

![1570982407367](/image/1570982407367.png)