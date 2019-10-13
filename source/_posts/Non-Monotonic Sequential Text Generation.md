---

title: Non-Monotonic Sequential Text Generation
date: 2019-09-05 21:43:51
tags: 2019-9-7-论文总结
---

## 题目

### Non-Monotonic Sequential Text Generation

## 启发

强化学习也是解决NLP问题的一个重要的手段

本文提出的方法， 既然是二叉树，可不可以考虑通过并发生成左右子树在不降低性能的前提下，提高效率？

<!-- more -->

------

## 总结

本论文是关于**强化学习**的，生成文本是采用树的形式：首先随机生成一个word，这样会将Y（ground-truth）随机分成左右两个（左右子树），在左右子树递归生成。

![1567691305607](/image/1567691305607.png)

state：已经生成的序列

action：下一步（t）要生成的word

policy：$\pi$，是state到action的映射，在给定s下，a的概率是$\pi(a|s)$

生成二叉树的过程是**层序（level-order）遍历**，生成的正确顺序是**中序遍历（in-order）**，上图中，$s_1=<b>, s_2=<b, a>,...$终止的状态是每个子树都是<end>，如果有T个非叶子节点，则有2*T+1个节点。

oracle: 可以认为是reference的答案。在每一个预测的过程中，oracle都是不确定的，因为树的结构是不确定的。

该模型的目标是学习$\pi$，引入$\pi^{in}$和$\pi^{out}$，$\pi^{in}$是状态分布，$\pi_{out}$是给定状态s和a的情况下，通过仿真等一系列手段来计算概率，所以模型的目标是优化：

![1567691886484](/image/1567691886484.png)

使用相对熵（KL-散度）来计算（RNN中的通用做法），来衡量$\pi^{out}$和$\pi$的分布差异，相对熵的优化目标是使得两个分布越来越接近，$\pi^{out}$是真实分布，$\pi$是非真实分布（预测分布）

![1567692005547](/image/1567692005547.png)

#### oracle policy（$\pi^{*}$）的计算

文中的$\pi^{in}$使用的$\pi^{*}$（the oracle policy），使用the ground truth output Y 和  the current state s 计算

![1567692189133](/image/1567692189133.png)

$Y_t$见上图，表示  合法的action，本文提出四种关于$p_a$的计算

- Uniform Oracle： 无偏好，对$Y_t$中的每个action都一样$p_a=1/n$

- Coaching Oracle: 保持当前policy的偏好

  ![1567692421432](/image/1567692421432.png)

- Annealed（ 退火） Coaching Oracle： 解决在训练初期，多样性的问题，$\beta$从1慢慢降为0

  ![1567692531961](/image/1567692531961.png)

- Deterministic Left-to-Right Oracle： 从左到右生成（只有右子树，没有左子树

#### learned policy（$\pi$）的计算

其实是训练一个RNN模型用于计算learned policy

+ 使用LSTM的分类分布（categorical distribution ）

![1567692772957](/image/1567692772957.png)

+ 使用 transform

+ 在transform中，将end的预测加入