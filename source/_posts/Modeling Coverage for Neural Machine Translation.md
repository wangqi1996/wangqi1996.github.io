---


title: Modeling Coverage for Neural Machine Translation
date: 2019-10-10 15:38:52
tags: 2019-10-7 论文阅读
typora-copy-images-to: ..\image
---

## 题目

###  Modeling Coverage for Neural Machine Translation

## 启发

这篇论文通过将注意力保存下来作为coverage，来解决under-translation和over-translation的问题。

<!-- more -->

------

## 总结

attention-base的NMT模型没有保存历史的注意力信息，这会导致model不知道句子的哪些部分已经翻译了，哪些部分还没有被翻译，产生**over-translation**和**under-translation**。本文提出使用**coverage vector**来保存注意力的历史信息，然后将coverage vector应用到attention model的计算中。我们希望通过这种方式，使model可以更加关注未翻译的部分。在SMT模型中是有使用coverage vector的概念的，也相当于部分借鉴了这种思路。在SMT中，会记录每个短语是否翻译，所有的短语翻译结束代表整个句子翻译结束，在NMT中，翻译到EOS代表翻译结束。

<img src="/image/1570721876118.png" alt="1570721876118"  />

![1570724221708](/image/1570724221708.png)

通用的**coverage model**如上图所示。

本文介绍了两种coverage model，一种利用语言学的知识（Linguistic Coverage Model），一种使用了NN。

![1570724198328](/image/1570724198328.png)

#### Linguistic Coverage Model

借助于语言学的知识？

和SMT很相似，区别在于SMT是fully translated，然而该模型是partially translated，代表每个词翻译的百分比，其中， $ a_{i,j} $ 代表第j个sourc word 对第i个target word的贡献。

![1570724262465](/image/1570724262465.png)

其中，代表一个source word被翻译成几个target word，起到一个归一化的作用。C初始为0。每一个源端单词，都有一个C向量。

引入了**fertility**的概念来评估该变量。

![1570724282698](/image/1570724282698.png)

其中，N是预先定义的常数，

####  Neural Network Based Coverage Model 

借助于NN模型

![1570724306252](/image/1570724306252.png)

#### 将coverger model集成到NMT的attention model中：

![1570724315333](/image/1570724315333.png)

#### training时，loss function：

![1570724327584](/image/1570724327584.png)

作者对比了将coverage model的目标也同时作为优化目标的方法，结果不理想。

#### 结论： 提高了BLEU得分，降低了对齐的错误率

