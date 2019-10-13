---
title: Insertion-based decoding with automatically inferred generation order
date: 2019-09-03 21:53:19
tags: 2019-9-7-论文总结
---

## 题目

### Insertion-based decoding with automatically inferred generation order

## 启发

在计算插入位置的时候，相对位置是优于绝对位置的。

**小小的不同**：本文先计算生成的词，后计算生成的词的相对位置，如果更换这两个操作的顺序会怎样？

<!-- more -->

***

## 总结

本文提出 **InDIGO**模型，通过插入操作来实现任意顺序生成文本。具体看论文

![1567521909211](/image/1567521909211.png)

##### 解码过程

1. 在每一步，同时预测word和position

   ![1567521575735](/image/1567521575735.png)

2. 使用绝对位置会造成隐层状态的不可复用，故采用相对位置(-1, 0, 1)，构造相对位置矩阵以及相应的更新方法

   ![1567521713764](/image/1567521713764.png)

3. 生成预测的单词$y_{t+1}$，选择插入的位置所在的单词，并确认是插在left or right， 更新R

   ![1567521776174](/image/1567521776174.png)

##### learning

在整个排列空间中计算P太复杂了，提出两种方法来解决：

Pre-deﬁned Order：使用预先定义好的顺序

Searched Adaptive Order (SAO) ：使用beam-search的结果来替代整个排列空间。