---
title: A Simple Introduction to Maximum Entropy Models for Natural Language Processing
date: 2018/2/22
tags: [maximum entropy]
pubtime: 1997
pubhouse: [Ircs Technical Reports]
---

原文引用：Ratnaparkhi A. A Simple Introduction to Maximum Entropy Models for Natural Language Processing[J]. Ircs Technical Reports, 1997.

<!--more-->

## Abstract
Many problems in natural language processing can be viewed as linguistic classification problems, in which linguistic contexts are used to predict linguistic classes. Maximum entropy models offer a clean way to combine diverse pieces of contextual evidence in order to estimate the probability of a certain linguistic class occurring with a certain linguistic context. This report demonstrates the use of a particular maximum entropy model on an example problem, and then proves some relevant mathematical facts about the model in a simple and accessible manner. This report also describes an existing procedure called Generalized Iterative Scaling, which estimates the parameters of this particular model. The goal of this report is to provide enough detail to re-implement the maximum entropy models described in [Ratnaparkhi, 1996, Reynar and Ratnaparkhi, 1997, Ratnaparkhi, 1997] and also to provide a simple explanation of the maximum entropy formalism.

【译文】自然语言处理中的许多问题都可以被看作是通过语境预测语言类别的分类问题。最大熵模型提供了一种清晰的方法，它通过结合各种背景证据来估计某个语言类别在特定语言背景下发生的概率。本报告演示了在一个示例问题上使用特定的最大熵模型，然后以简单易理解的方式证明了有关该模型的一些相关数学事实。本报告还介绍了一种称为 Generalized Iterative Scaling(GIS 算法) 的过程，用于估计此特定模型的参数。本报告的目标是为重现 [Ratnaparkhi, 1996, Reynar and Ratnaparkhi, 1997, Ratnaparkhi, 1997] 中描述的最大熵模型提供充足的细节，并提供最大熵形式的简单解释。

## 1 Introduction
Many problems in natural language processing (NLP) can be re-formulated as statistical classification problems, in which the task is to estimate the probability of "class" $a$ occurring with "context" $b$ or $p(a, b)$. Contexts in NLP tasks usually include words, and the exact context depends on the nature of the task; for some tasks, the context $b$ may consist of just a single word, while for others, $b$ may consist of several words and their assoiated syntactic labels. Large text corpora usually contain some information about the cooccurrence of $a$'s and $b$'s, but never enough to completely specify $p(a, b)$ for all possible $(a, b)$ pairs, since the words in $b$ are typically sparse. The problem is then to find a method for using the sparse evidence about the $a$'s and $b$'s to reliably estimate a robability
model $p(a, b)$.

【译文】自然语言处理（NLP）中的许多问题可以重新形式化为统计分类问题，其任务是估计 “类” $a$ 在 “上下文” $b$ 出现的概率，即 $p(a, b)$。 NLP 任务中的上下文通常由许多单词构成，但确切的上下文还是取决于任务的性质; 对于某些任务，上下文 $b$ 可能只包含一个单词，而对于其他任务，$b$ 可能由几个单词及其相关的语法标签组成。大的文本语料库通常会包含一些 $a$ 和 $b$ 共同出现的信息，但是也没有足够完整到能为所有可能的 $(a,b)$ 计算 $p(a,b)$，因为 $b$ 中的单词通常是稀疏的。那么当务之急是找到一种仅使用 $a$ 和 $b$ 的稀疏数据便能可靠地估计出一个可用性模型 $p(a,b)$ 的方法。

Consider the Principle of Maximum Entropy [Jaynes, 1957, Good, 1963], which states that the correct distribution $p(a,b)$ is that which maximizes entropy, or "uncertainty", subject to the constraints, which represent "evidence", i.e., the facts known to the experimenter. [Jaynes, 1957] discusses its advantages:

【译文】考虑最大熵原理 [Jaynes, 1957, Good, 1963] 中指出熵（即 “不确定性”）的最大化是 $p(a,b)$ 正确分布的约束条件，它也在表征着 “数据” ，这是研究人员众所周知的事实。[Jaynes, 1957] 讨论了它的优点：

> ...in making inferences on the basis of partial information we must use that probability distribution which has maximum entropy subject to whatever is known. This is the only unbiased assignment we can make; to use any other would amount to arbitrary assumption of information which by hyp othesis we do not have.

> 【译文】...在根据部分信息进行推理时，我们必须使用具有最大熵的概率分布，而这些分布受任何已知的情况影响。 这是我们可以做的唯一没有偏见的任务; 使用任何其他方式都等于通过假设我们没有的信息的任意假设


More explicitly? if A denotes the set of p ossible classes? and B denotes the set
of p ossible contexts? p should maximize the entropy

$$
    H(p)=-\sum_{x\in\varepsilon}{p(x)log{p(x)}}
$$

where x ? ?a? b?? a ? A? b ? B ? and E ? A ? B ? and should remain consistent
with the evidence? or ?partial information?? The representation of the evidence?
discussed below? then determines the form of p?

## 2 Representing Evidence