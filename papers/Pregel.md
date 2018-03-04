---
title: Pregel: A System for Large-Scale Graph Processing
date: 2018/2/21
tags: [graph process, Pregel]
pubtime: 2010
pubhouse: [ACM, SIGMOD]
---

原文引用：Malewicz G, Austern M H, Bik A J C, et al. Pregel: a system for large-scale graph processing[C]// ACM SIGMOD International Conference on Management of Data. ACM, 2010:135-146.

<!--more-->

## ABSTRACT
Many practical computing problems concern large graphs. Standard examples include the Web graph and various social networks. The scale of these graphs—in some cases billions of vertices, trillions of edges—poses challenges to their efficient processing. In this paper we present a computational model suitable for this task. Programs are expressed as a sequence of iterations, in each of which a vertex can receive messages sent in the previous iteration, send messages to other vertices, and modify its own state and that of its outgoing edges or mutate graph topology. This vertex-centric approach is flexible enough to express a broad set of algorithms. The model has been designed for efficient, scalable and fault-tolerant implementation on clusters of thousands of commodity computers, and its implied synchronicity makes reasoning about programs easier. Distribution-related details are hidden behind an abstract API. The result is a framework for processing large graphs that is expressive and easy to program.

【译文】当今 Web 图和各种社交网络等许多实际问题都涉及到了大规模图计算。在某些情况下，图的规模达到数十亿顶点、数万亿边，这对图数据的高效处理提出了挑战。在本文中，我们提出了一个适用于此任务的计算模型。程序使用一系列迭代表示，且在迭代时顶点可以接收前一次迭代中发送的消息、向其他顶点发送消息、修改自身及其出边状态、还可以修改图的拓扑结构。这种以顶点为中心的方法足够灵活，可以表达一系列通用的算法。该模型的设计目的是在数千台商用机器集群上实现高效率、可扩展和高容错。其隐含的同步性使得程序更易理解分析。由于抽象 API 隐藏了与分布式相关的细节，使得这个框架在处理大规模图时，富有表现力且易于编程。

### Categories and Subject Descriptors
D.1.3 [Programming Techniques]: Concurrent Programming—Distributed programming; D.2.13 [Software Engineering]: Reusable Software—Reusable libraries

### General Terms
Design, Algorithms

### Keywords
Distributed computing, graph algorithms

## 1. INTRODUCTION
The Internet made the Web graph a popular object of analysis and research. Web 2.0 fueled interest in social networks. Other large graphs—for example induced by transportation routes, similarity of newspaper articles, paths of disease outbreaks, or citation relationships among published scientific work—have been processed for decades. Frequently applied algorithms include shortest paths computations, different flavors of clustering, and variations on the page rank theme. There are many other graph computing problems of practical value, e.g., minimum cut and connected components.

【译文】互联网使网络图成为分析和研究的热门对象，Web 2.0 更助长了研究者对社交网络的兴趣。其他大规模图——例如运输路线、报纸文章的相似网、疾病传播路径或已发表的科学着作之间的引文关系——已经处理了数十年。常用的算法包括最短路径计算、不同风格的聚类以及页面等级主题的变化。还有许多其他具有实际价值的图计算问题，例如最小切割和连通分量。

