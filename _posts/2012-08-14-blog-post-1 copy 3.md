---
title: 'Expressivity and Generalisation of Graph Transformer'
date: 2024-06-22
permalink: /posts/2024/06/blog-post-3/
tags:
  - graph learning
  - expressivity
---

During my master study I was working with Luis Müller at Learning on Graphs Group at RWTH Aachen in the intersection of graph theory (expressivity) and machine learning (generalisation). An expressive Graph Neural Network (GNN) or Graph Transformers means it can differentiate many similar graphs that are different.
In other words a model with a higher expressivity can be useful for example in chemistry for predicting molecular properties or generating new molecules depends heavily on identifying and counting specific substructures (like functional groups or rings), or in general useful for anomaly prediction. However, this does not guarantee that the model generalizes on other domains or distribution.
Thus, studying its generalization power is also important. In one of the project we studied the expressivity and generalisation of a variant of Transformers called Graph Transformers and applied it on graph data (NeurIPS 2024).

![alt text](https://ksmdnl.github.io/images/triangular_att_3.png "Triangular attention")
*[1]*

We implemented a tokenization that reconstructs the graph data (nodes and the node pairs, or edges) and theoreticaly proved that it is more expressive than GNNs and many Graph Transformers.
Through experiments across datasets we showed that it generalises better than many higher-order Graph Transformers and GNNs
and show that it is useful for out of domain or distribution downstream tasks (a.k.a. for real life problems).
One caveat is, though, that it performs by doing a triangular attention that can be expensive for big-sized graphs.

[1] Müller, L., Kusuma, D., Bonet, B., & Morris, C. (2024). Towards Principled Graph Transformers. NeurIPs.