---
title: Insertion transformer Flexible sequence generation via insertion operations
date: 2019-09-05 22:33:37
tags: 2019-9-7-论文总结
---

## 题目

### Insertion transformer Flexible sequence generation via insertion operations

## 启发

本文给我的启发是提出了同时插入多个词的方法，提高效率。

计算(c,l)的联合分布也是一个很新颖的做法

<!-- more -->

------

## 总结

本文提出通过插入来生成序列的方法，

![1567752832247](/image/1567752832247.png)

该模型预测插入的位置l和插入的词语c的联合分布（joint distribution）

本文提出一种新颖的表示方法，插槽表示（Slot Representations）：如果一个句子包含n个词，那么需要n+1个插槽表示，每个插槽表示 = 左边单词和右边的单词的概括表示。

### 计算 p(c,l)

1.  Content-Location Distribution

   1. 共同分布：

      ![1567753291262](/image/1567753291262.png)

   2. 因式分解

      ![1567753333418](/image/1567753333418.png)
   
      ![1567753340363](/image/1567753340363.png)
   
2.  Contextualized Vocabulary Bias：词汇级别的，给每个词加一个偏差

   ![1567754368347](/image/1567754368347.png)

### 损失函数

1.  left-to-right：随机采样k，选取$y_0,,,y_k$

   ![1567754442493](/image/1567754442493.png)

2.  平衡二叉树：通过给中间的词赋予较高的权重来保持平衡，随机采样k，然后打乱y，取前k个。对于第k+1个插槽，假设插槽的target seqs为$(y_{i_l},y_{i_l+1},...,y_{j_l})$，定义距离如下：

   ![1567866010629](/image/1567866010629.png)

   使用-d作为奖赏函数(reward function)

   ![1567866222383](/image/1567866222383.png)

   定义每个插槽的损失函数：(位于中间的w大，损失函数小)

   ![1567866238574](/image/1567866238574.png)

   定义总的损失函数：

   ![1567866271539](/image/1567866271539.png)

3.  Uniform ：和平衡二叉树相似

   ![1567866349220](/image/1567866349220.png)

### 终结

+ slot完成：如果一个插槽对应的target seqs为空，则该插槽对应的为end-of-slot token，所有的插槽对应end-of-slot token，则认为结束
+ sequence 完成：如果对应的target seqs为空，不对应任何东西，当所有的对应为空时，则停止。

### 推理

1.  贪婪解码：总而言之，选取最大的位置和word

   ![1567866687989](/image/1567866687989.png)

2.  并行解码：对于每一个位置，计算

![1567867099225](/image/1567867099225.png)

