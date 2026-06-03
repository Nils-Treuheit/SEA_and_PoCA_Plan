---
title: "O2O-Afford"
source: "https://cs.stanford.edu/~kaichun/o2oafford/"
author:
published:
created: 2026-06-03
description:
tags:
  - "clippings"
---
O2O-Afford: Annotation-Free Large-Scale  
Object-Object Affordance Learning  
  
[Kaichun Mo](http://cs.stanford.edu/~kaichun) <sup>1</sup> [Yuzhe Qin](https://yzqin.github.io/) <sup>2</sup> [Fanbo Xiang](https://www.fbxiang.com/) <sup>2</sup> [Hao Su](http://cseweb.ucsd.edu/~haosu/) <sup>2</sup> [Leonidas J. Guibas](http://geometry.stanford.edu/member/guibas/index.html) <sup>1</sup>  
  
<sup>1</sup> [Stanford University](http://www.stanford.edu/) <sup>2</sup> [UCSD](https://www.ucsd.edu/)  
  
[*Conference on Robot Learning (CoRL) 2021*](https://www.robot-learning.org/home)

Abstract

Contrary to the vast literature in modeling, perceiving, and understanding agent-object (e.g., human-object, hand-object, robot-object) interaction in computer vision and robotics, very few past works have studied the task of object-object interaction, which also plays an important role in robotic manipulation and planning tasks. There is a rich space of object-object interaction scenarios in our daily life, such as placing an object on a messy tabletop, fitting an object inside a drawer, pushing an object using a tool, etc. In this paper, we propose a unified affordance learning framework to learn object-object interaction for various tasks. By constructing four object-object interaction task environments using physical simulation (SAPIEN) and thousands of ShapeNet models with rich geometric diversity, we are able to conduct large-scale object-object affordance learning without the need for human annotations or demonstrations. At the core of technical contribution, we propose an object-kernel point convolution network to reason about detailed interaction between two objects. Experiments on large-scale synthetic data and real-world data prove the effectiveness of the proposed approach.

Video Presentation \[5-min\]

  

![](https://www.youtube.com/watch?v=cbDSalrMhlo)

Framework and Tasks

![](https://cs.stanford.edu/~kaichun/o2oafford/images/teaser.png)  

Figure 1. For a given object-object interaction task (i.e. placement, fitting, pushing, stacking), our method takes as inputs a 3D acting object point cloud (a) and a partial 3D scan of the scene/object (b), and outputs an affordance prediction heatmap (c) that estimates the likelihood of the acting object successfully accomplishing the task at every pixel. At the test time, one may easily sample a position from the heatmap to perform the action (d).

Network Architecture

![](https://cs.stanford.edu/~kaichun/o2oafford/images/network.png)  

Figure 2. Taking as inputs a partial 3D scan of the scene S (dark blue) and a complete 3D point cloud of acting object O (dark red), our network learns to extract per-point features on both inputs, correlate the two point cloud feature maps using an object-kernel point convolution, and finally predict a point-wise affordance heatmap over the scene point cloud.

Qualitative Results

![](https://cs.stanford.edu/~kaichun/o2oafford/images/results.png)  

Figure 3. For each of the tasks, we show examples of our network predictions. Left two examples show test shapes from the training categories, while two right ones are shapes from the test categories. In each pair of result figures, we draw the scene geometry together with the acting object (marked with red dashed boundary) on the left, and show our predicted per-point affordance heatmaps on the right. We observe that our framework successfully learns meaningful O2O priors, and also demonstrates the capability of generalizing to out-of-distribution test categories.

Real-world Data

![](https://cs.stanford.edu/~kaichun/o2oafford/images/realdata.png)  

Figure 4. We show qualitative results testing our learned models directly over real-world data. For the input 3D scene geometry scans, we use the Replica dataset (the placement example on the left), the RBO dataset (the fitting example on the left), our own collected 3D scans with iPad Pro (the placement and fitting examples on the right), and Google Scanned Objects (the scene geometry for the other two tasks). We use shapes from Google Scanned Objects and YCB Dataset for the acting objects in the four tasks. For each tuple of results, from left to right, we respectively show the scene geometry, the acting object, the interaction snapshot (to illustrate the poses and sizes of the objects), and our point-wise affordance predictions.

Result Analysis

![](https://cs.stanford.edu/~kaichun/o2oafford/images/analysis.png)  

Figure 5. Our network predictions are sensitive to the acting object size (top-row) and orientation (bottom-row) changes. In the top-row fitting example, increasing the size of the mug reduces the chance of putting it inside the drawer, as the drawer cannot be further closed containing a big mug. For the bottom-row placement example, we observe some detailed affordance heatmap changes while we rotate the cuboid-shaped acting object.

Acknowledgements

This research was supported by NSF grant IIS-1763268, NSF grant RI-1763268, a grant from the Toyota Research Institute University 2.0 program, a Vannevar Bush faculty fellowship, and gift money from Qualcomm. This work was also supported by AWS Machine Learning Awards Program.

---

source: [https://cs.stanford.edu/~kaichun/o2oafford/](https://cs.stanford.edu/~kaichun/o2oafford/)
[video](https://cs.stanford.edu/~kaichun/o2oafford/#video)
[code](https://github.com/daerduoCarey/o2oafford)

