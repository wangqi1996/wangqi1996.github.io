---
title: PRIMT A Pick-Revise Framework for Interactive Machine Translation
date: 2019-09-02 20:10:02
tags: 2019-9-7-论文总结
---

## 题目

### PRIMT : A Pick-Revise Framework for Interactive Machine Translation

## 启发

借鉴该论文PSM模型的思想，我们也可以训练一个分类器来学习机器翻译的中心词，然后使用middle-out论文的方法来训练模型观察结果。分类器的输入为源句子，输出为该target/source的中心词（如果是source的中心词，则可以通过一个对齐模型来确定对应的target）。

<!-- more -->

问题转变成获取训练集合。对于source/target sentence的数据集，我们可以从source/target的每一个词开始翻译，观察通过哪一个词开始像两边翻译能够取得最好的结果，从而确定中心词。***这个方法有待考察***



## 总结

本文是一篇关于IMT（Interactive machine translation）的论文，传统的IMT的方法是从左到右（left-most）进行修改的，本文提出一种pickrevise的方法，pick： 选出**最错误**的词语，revise：从translation table中选择正确的候选翻译或者手动输入合适的候选翻译。pick0revise允许在任何位置进行修改，使用一个受限的decoder进行解码。改论文是基于SMT的。

本文提出一个suggestion model，旨在帮助人类进行pick或者revise步骤。The Picking Suggestion Model(PSM)，该模型的目标在于识别可能翻译错误的短语，通过遍历每个短语，对该短语进行revise，如果BLEU得分得到提高，则认为是错误的翻译；否则是正确的翻译。The Revising Suggestion Model(RSM)，该模型的目的在于从候选翻译中选择一个正确的短语，如果一个短语(1). 在ref中出现，(2)可以通过预训练的词对齐模型对齐，则认为该短语是正确的，否则是错误的。





