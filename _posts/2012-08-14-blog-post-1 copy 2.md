---
title: 'Learning Multimodal Data using JEPA'
date: 2026-11-05
permalink: /posts/2026/11/blog-post-2/
tags:
  - representation learning
  - multimodal learning
---
Joint-Embedding Predictive Architecture (JEPA) [1, 2] is a self-supervised non-generative approach to learn about the data efficiently. The learned or trained network or model in general produces "useful" representations or embeddings. "Useful" could mean many things, what people usually mean though is that the representations are useful for downstream tasks. For example, a useful representation is the one that is transferable to multiple distinct tasks simultaneously without retraining the encoder.
![alt text](https://ksmdnl.github.io/images/image_a.png "Encoding")

In JEPA, there is a context and target representations produced by an encoder, and the encoder is trained to produce a similar representations to the targets. So the key idea is doing a prediction in the latent space. For one, it is cheaper than predicting in the input or output space, since usually latent representation has lower dimension. But also, the encoder needs not learn the detail about the data in order to extract higher order features of the data that are usually useful for downstream tasks. One caveat here is, the encoder may learn to produce trivial representations (representation collapse). Thus, usually people introduces regularizations or put more constraint during the prediction process [1, 2, 3]. What constitutes the context or the targets is quite arbitrary in terms of resource or modality [1, 2], though at the time when I was working on my thesis this hadn't been extensively explored apart from vision modality [3, 4].
![alt text](https://ksmdnl.github.io/images/image_b.png "JEPA training")

In the thesis we proposed an approach to make JEPA work for vision-language understanding. Initially, we designed few setups that involve 
1. cross-modal predictions,
2. multimodal alignment through mixture of experts,
and observed that the problem may be underconstrained and thus the encoder produces unuseful representations, especially with 1. and not so with 2., indicated by low scores in vision, language or vision-language tasks such as cross retrieval, multimodal retrieval and caption generation [5]. To mitigate it we replace the disagreement function during the training with a contrastive loss, which may induce more constraint during the prediction. Contrastive loss was used to trained embeddings that produced CLIP encoder. That improves the quality of the embeddings, so why don't we just use CLIP then? Remember, we want to train an encoder that produces useful embeddings. Compared to CLIP our approach, which is called X-JEPA, shows better transferable characteristics in the downstream tasks. Additionally, CLIP produces a so called modality gap, i.e., a gap in the representation space between the image and the text representations, which reduces usefulness in vision-language tasks [7, 8]. While we also observed that with CLIP, we observe minimal gap in our work [5], which supports the comparison on the downstream task's performances. 

The thesis served as a basis for our work that is accepted for AACL-IJCNLP 2026 with the title Latent Prediction Needs Alignment: A Controlled Study of Joint-Embedding Predictive Vision–Language Learning [9]. Github: https://github.com/mohkoh19/x-jepa.

[1] LeCun, Yann. “A Path Towards Autonomous Machine Intelligence.” (2022).

[2] Dawid, Anna and Yann LeCun. “Introduction to latent variable energy-based models: a path toward autonomous machine intelligence.” Journal of Statistical Mechanics: Theory and Experiment 2024 (2023): n. pag.

[3] Assran, Mahmoud et al. “Self-Supervised Learning from Images with a Joint-Embedding Predictive Architecture.” 2023 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR) (2023): 15619-15629.

[4] Bardes, Adrien et al. “Revisiting Feature Prediction for Learning Visual Representations from Video.” ArXiv abs/2404.08471 (2024): n. pag.

[5] Kusuma, Daniel. “Designing and Evaluating a Joint Embedding Predictive Architecture for Vision-Language Understanding.” Master thesis (2026)

[6] Radford, Alec et al. “Learning Transferable Visual Models From Natural Language Supervision.” International Conference on Machine Learning (2021).

[7] Liang, Weixin et al. “Mind the Gap: Understanding the Modality Gap in Multi-modal Contrastive Representation Learning.” ArXiv abs/2203.02053 (2022): n. pag.

[8] Eslami, Sedigheh and Gerard de Melo. “Mitigate the Gap: Investigating Approaches for Improving Cross-Modal Alignment in CLIP.” ArXiv abs/2406.17639 (2024): n. pag.

[9] Kohankhaki, Mohammad and Kusuma, Daniel and Salehi, Shirin and Kamp, Carsten and Brell-Cokcan, Sigrid and Schmeink, Anke. “Latent Prediction Needs Alignment: A Controlled Study of Joint-Embedding Predictive Vision--Language Learning.” AACL-ICJNLP (2026). To appear.