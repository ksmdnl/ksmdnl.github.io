---
title: 'Performance Estimation'
date: 2023-06-22
permalink: /posts/2023/06/blog-post-1/
tags:
  - vision
---

Developing perception systems for autonomous driving has not been a trivial task. Besides of having been one of the central topics in the community, there are just so many scenarios in the streets to consider in order to secure the system's safety. And while sensor devices are getting better and cheaper, a system that robust against noise or corruption needs to be studied further. To locate objects and boundaries (lines, curves, etc.) in street or traffic scenes a typical approach is to build a system that transforms the input into something more practical to analyse. Object detection locates existing objects in a scene and assigns a category to each identified object, while semantic segmentation deals with a pixel-level classification, producing a mask of certain classes.

![alt text](https://ksmdnl.github.io/images/attack_a.png "Cityscape")
*Adversarial attacks on Cityscape data [1]*

During my bachelor study I was working with Andreas Bär in computer vision for autonomous driving. One of the crucial things to care for in AD is how the performance of a vision model, e.g., semantic segmentation, object detection, object tracking, depth estimation, etc., can be tracked or quantised or estimated. Semantic segmentation, for instance, assigns each pixel on a camera a class and generates the user or planner a segmentation mask. A group of pixels can be classified as pedestrian, others as cars and so on.

![alt text](https://ksmdnl.github.io/images/seg_a.png "Cityscape")
*Example of segmentation on Cityscape for scene understanding [3]*
![alt text](https://ksmdnl.github.io/images/seg_b.png "Lung")
*Example of lesion segmentation on MIMIC-CXR data [4]*

Since real life data is most certainly noisy and there are scenarios in which the system could be injected with an adversarial feature (so called adversarial attacks), quantising and estimating the performance of the segmentation model, i.e., how good is the prediction quality in each given time?, would be a good start. In Andreas Bär's work [1], it extends the prediction head of segmentation model with a reconstruction head that reconstructs the input image. It works by training only to reconstruct the image given the representation of the input image that was only trained for segmentation and then to assume a correlation between the segmentation and the reconstruction since both share a representation module or encoder.

![alt text](https://ksmdnl.github.io/images/pp.png "Framework")
*The performance estimation framework by Bär et al [1]*

So a bad representation should mean a bad segmentation, formulated as a regression problem, and since we can compute the quality of the reconstructed image (e.g., PSNR, SSIM), we could estimate the segmentation performance. So the trained model is calibrated on the validation dataset, i.e., the PSNR and the Mean Intersection over Union (mIoU) to get the regression parameters and apply these parameters on unseen data in online mode. Basically, estimating the mIoU using the PSNR.

![alt text](https://ksmdnl.github.io/images/progression.png "Framework")
*mIou (left) correlates with PSNR (right) in any given attack proportion [1]*

![alt text](https://ksmdnl.github.io/images/calibration.png "Framework")
*(Non)-linear relationship PSNR vs mIoU on the validation set, used to get the regression parameters [1].*

What about on object detection? The setup above works because the assumption holds, i.e., high correlation between the two predictions. I extended the framework to object detection model to estimate the Mean Average Precision (mAP) using the PSNR. We observed a low correlation between the two and sure enough the estimation was way off. So we take a step back focus on other things. So, mIoU(PSNR) works given the assumption. Naturally, it makes sense to try to improve on the correlation, because high correlation -> better estimate. Initially, the reconstruction module was initialised randomly. Here we initialised it using the weights of the segmentation head. We can control how much the transfer is so we can see a gradient. Further, we add the segmentation exposure to the reconstruction module by doing feature fusion on different prediction step. Also here we can control how much fusion is done. The work (CVPR Workshop 2023) shows that the estimate gets better the more exposure, either by parameter initialisation or feature fusion or both, the reconstruction module get, which again confirmed our hypothesis and aligned with our assumption.

![alt text](https://ksmdnl.github.io/images/pp_improved_1.png "Framework")
* Parameter initialisation (purple) and information or feature fusion (blue) on top of the estimation framework [2].*

Here is a demo:

<!--
Source - https://stackoverflow.com/a/10530266
Posted by huon, modified by community. See post 'Timeline' for change history
Retrieved 2026-11-24, License - CC BY-SA 3.0
-->


<iframe width="560" height="315" src="https://www.youtube.com/embed/8wde_ACGk5U?si=RcrD8WOPx8TSaLC8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

In the demo we see on the top lef the original image (input), the top right the reconstructed image by the image reconstruction decoder. Middle left is segmentation output of the image. On the middle right is a monitor about the state of the estimated performance, whether the estimate constitutes a degradation etc.

[1] Bär, Andreas & Klingner, Marvin & Löhdefink, Jonas & Hüger, Fabian & Schlicht, Peter & Fingscheidt, Tim. (2022). Performance Prediction for Semantic Segmentation by a Self-Supervised Image Reconstruction Decoder. 4398-4407. 10.1109/CVPRW56347.2022.00486.
[2] Bär, Andreas & Kusuma, Daniel & Fingscheidt, Tim. “Improvements to Image Reconstruction-Based Performance Prediction for Semantic Segmentation in Highly Automated Driving.” 2023 IEEE/CVF Conference on Computer Vision and Pattern Recognition Workshops (CVPRW) (2023): 219-229.
[3] Cordts, Marius et al. “The Cityscapes Dataset for Semantic Urban Scene Understanding.” 2016 IEEE Conference on Computer Vision and Pattern Recognition (CVPR) (2016): 3213-3223.
[4] Choi, Geon & Yoon, Hangyul & Shin, Hyunju & Park, Hyunki & Seo, Sang & Yang, Eunho & Choi, Edward. (2025). Instruction-Guided Lesion Segmentation for Chest X-rays with Automatically Generated Large-Scale Dataset. 10.48550/arXiv.2511.15186. 