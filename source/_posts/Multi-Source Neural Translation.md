---

title: Multi-Source Neural Translation
date: 2019-09-08 13:56:17
tags: 2019-9-7-论文总结
---

## 题目

### Multi-Source Neural Translation

## 启发

本论文简单介绍了多源注意力机制，通过输入多个源语言，来提高翻译的性能。阅读本论文是为了增大知识面，了解一下多源的NMT如何实现的

<!-- more -->

------

## 总结

本文是两个输入语言，一个输出语言的frame

![1567922411634](/image/1567922411634.png)

对于每个输入语言，对应一个encoder，然后将两个encoder的输出（对于LSTM来说，是h和c），输入到combiner（黑色的模块），combiner将两个h和两个c整合成一个h和一个c，相当于一个注意力机制。combiner的三种做法

+ Basic Combination Method: 简单的拼接经过非线性层

![1567922422876](/image/1567922422876.png)

![1567922429459](/image/1567922429459.png)

+  Child-Sum Method： 借鉴child-Sum tree-Lstm的思想，将两个encoder当做两个子节点

![1567922495607](/image/1567922495607.png)

+ Multi-Source Attention： 使用local-p的注意力机制

  ![1567922530759](/image/1567922530759.png)