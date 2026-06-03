---
title: "Blitz-SLAM: A semantic SLAM in dynamic environments"
source: "https://www.sciencedirect.com/science/article/pii/S0031320321004064"
author:
  - "[[•A semantic SLAM system in indoor dynamic environments is proposed.•The depth masks of objects are obtained.•The noise blocks formed by moving objects are removed.]]"
  - "[[dynamic matching points will directly interrupt the camera localization]]"
  - "[[and the noise blocks formed by moving objects will contaminate the constructed map. In this paper]]"
  - "[[a semantic SLAM system working in indoor dynamic environments named Blitz-SLAM is proposed. The noise blocks in the local point cloud are removed by combining the advantages of semantic and geometric information of mask]]"
  - "[[RGB and depth images. The global point cloud map can be obtained by merging the local point clouds. We evaluate Blitz-SLAM on the TUM RGB-D dataset and in the real-world environment. The experimental results demonstrate that Blitz-SLAM can work robustly in dynamic environments and generate a clean and accurate global point cloud map simultaneously.]]"
  - "[[Static environment is a prerequisite for most of visual simultaneous localization and mapping systems. Such a strong assumption limits the practical application of most existing SLAM systems. When moving objects enter the camera’s view field]]"
published:
created: 2026-06-03
description: "Static environment is a prerequisite for most of visual simultaneous localization and mapping systems. Such a strong assumption limits the practical a…"
tags:
  - "clippings"
---
## Published by: Elsevier

### Published by

[![Elsevier](https://www.sciencedirect.com/eu-west-1/prod/dc6e12e0fa077cb242a1170a0f83362834988af9/image/elsevier-non-solus.svg)](https://www.sciencedirect.com/journal/pattern-recognition "Go to Pattern Recognition on ScienceDirect")

,,,,

[View **PDF**](https://www.sciencedirect.com/science/article/pii/S0031320321004064/pdfft?md5=6d724d7addaa358ba0bb9954d83ff964&pid=1-s2.0-S0031320321004064-main.pdf)

[10.1016/j.patcog.2021.108225](https://doi.org/10.1016/j.patcog.2021.108225)

## Highlights

## Keywords

Semantic SLAM

;

Dynamic environments

;

Noise block

;

Local point cloud

;

Global point cloud map

- [Previous article in this issue](https://www.sciencedirect.com/science/article/pii/S0031320321004040)
- [Next article in this issue](https://www.sciencedirect.com/science/article/pii/S0031320321004076)

## 1\. Introduction

Simultaneous localization and mapping (SLAM) plays an important role in the navigation and human-computer interaction of unmanned systems. Most of the existing visual SLAM systems are based on the static environment assumption, which greatly limits the application of these systems in the real world, such as autonomous driving on a busy road. When moving objects enter the camera’s view field, the matching points on these objects will directly interfere with the camera pose estimation, and the information of these objects would be remained in the constructed map. The main idea of SLAM systems working properly in dynamic scenes is to locate stable static features in the environment for camera localization. RANSAC (Random Sample Consensus) method is the most representative way adopted by SLAM systems to remove mismatches in static or lowly dynamic scenes. However, RANSAC cannot guarantee to remove those mismatches in highly dynamic environments.

Great breakthroughs have been achieved in object detection and segmentation in recent years. Many researchers have tried to combine the SLAM with [deep learning](https://www.sciencedirect.com/topics/engineering/deep-learning) to construct semantic SLAM systems that adapt to dynamic environments. The negative effects of moving objects can be removed by combining the semantic information of pre-defined moving objects with spatial geometric information. The pre-defined moving objects are often conventional, such as people, cars, animals, etc. In fact, the object orginal masks provided by the current semantic [segmentation methods](https://www.sciencedirect.com/topics/engineering/segmentation-method) are imperfect and cannot cover the moving objects completely, especially the object boundaries. The boundary information would be leaked into the point cloud map to form lots of noise blocks.

In this paper, we propose Blitz-SLAM, which is a novel semantic SLAM system working in indoor dynamic environments. Blitz-SLAM adopts ORB-SLAM2, one of the most complete and easiest SLAM systems based on feature points, as the global SLAM solution. Our moving objects removal approach is intergrated with the front end of ORB-SLAM2. In Blitz-SLAM, BlitzNet is utilized to obtain the semantic information of objects in the environment. BlitzNet is a real-time [deep neural network](https://www.sciencedirect.com/topics/chemical-engineering/deep-neural-network), which can obtain the [bounding boxes](https://www.sciencedirect.com/topics/engineering/bounding-box) and masks of objects simultaneously in one-time forward propagation. The bounding boxes of the moving objects are used to divide the image into environment regions and potential dynamic regions quickly. The static matching points in the potential dynamic regions can be located by the [epipolar constraints](https://www.sciencedirect.com/topics/engineering/epipolar-constraint) constructed by the matching points in the environment regions. The original masks are revised using the geometric information of depth images to cover the moving objects more completely. The noise blocks in the local point clouds are removed with the revised masks. And a clean global point cloud map can be obtained by merging these local point clouds. The main contributions of this paper are as follows:
- 1.
	A method to obtain the revised masks of objects is proposed by combining the original masks and depth information of the objects.
- 2.
	We propose an approach to eliminate the noise blocks formed by moving objects in the construction of the point cloud map.
- 3.
	A solution is proposed to solve the problem that movable objects appear multiple times in the global point cloud map.

The rest of the paper is organized as follows: presents the related work. gives the whole pipeline and details of Blitz-SLAM. shows the experiment results., finally, concludes the work.

## 2\. Related work

### 2.1. Dynamic SLAM without prior-known moving-object information

Seeking reliable static matching features is the fundamental task for dynamic SLAM systems without prior-known moving-object information. Sun *et al.* proposed a RGB-D data-based motion removal approach as a pre-processing module to filter out moving objects. This work is one of the pioneering works in the field of dynamic SLAM. The follow-up work of consists of two on-line parallel processes: the learning process for building and updating the foreground model; the inference process for foreground pixel-wisely segmentation. DMS-SLAM uses the sliding window model to perform feature matching between two discontinuous frames, and adopts GMS (Grid-based Motion Statistics) to filter out outliers. The RGB-D SLAM system proposed by Li *et al.* adopts a static weighting method to calculate the likelihood of each [keyframe](https://www.sciencedirect.com/topics/engineering/keyframes) point being part of the static environment. LPM (Locality Preserving Matching) can maintain the local neighborhood structures of the true matches. LMR (Learning for Mismatching Removal) transforms the problem of mismatch removal into a two-class classification problem, and learns a general classifier to identify inliers and outliers.

To overcome the shortcomings that the natural landmarks are not robust to rapid motion, large change of viewpoint and significant change of scene, an offline SLAM system based on the squared planar markers detection is proposed by Rafael *et al.* SPM-SLAM and UcoSLAM are the continuation of this system. SPM-SLAM is a real-time system and solves a number of problems left over from its predecessor, such as map initialization, marker pose estimation from ambiguous observation, and loop closure detection with markers. UcoSLAM fuses [keypoints](https://www.sciencedirect.com/topics/engineering/keypoints) and squared [fiducial](https://www.sciencedirect.com/topics/engineering/fiducials) markers to achieve long-term robust tracking in repetitive environment.

### 2.2. Semantic SLAM in dynamic environments

The masks obtained by the semantic segmentation networks can mark the regions of moving objects in the images, and provide prior information for the construction of dynamic SLAM. Dyna-SLAM removed the pre-defined moving objects by combining multi-view geometry and the masks, and a background inpainting method is proposed to inpaint the occluded background. DynaSLAM II can estimate the camera poses, the sparse static 3D map and the trajectories of multiple moving objects simultaneously. And a novel bundle adjustment is proposed to jointly optimize the structures of static scene and the poses of dynamic objects. MaskFusion adopts a geometric segmentation to produce the accurate object boundaries to overcome the drawback of the imperfect boundaries provide by the semantic segmentation. MID-Fusion can provide the geometric, semantic and motion properties of objects in the environment. DS-SLAM excludes the person regions in the image, and utilizes moving consistency to eliminate the dynamic matching points. DP-SLAM tracks the dynamic matching points in a Bayesian [probability estimation](https://www.sciencedirect.com/topics/engineering/probability-estimation) framework to overcome deviation of geometry constraints and sematic segmentation.

Some researchers utilized the [bounding boxes](https://www.sciencedirect.com/topics/engineering/bounding-box) of potential moving objects to remove these objects instead of the masks. Detect-SLAM adopts the strategy of propagating the moving probability of four categories of keypoints to overcome the delay of semantic information. Dynamic-SLAM employs a missed detection compensation algorithm to address the problem of the low recall rate of the existing SSD network. CubeSLAM generates 3D bounding boxes from 2D bounding boxes based on vanishing points, and improves the camera pose estimation by object representation and motion model constraints.

### 2.3. Object detection and semantic segmentation networks

Mask-RCNN is one of the commonly used instance [segmentation methods](https://www.sciencedirect.com/topics/engineering/segmentation-method) in current semantic SLAM systems, such as Dyna-SLAM, MaskFusion, MID-Fusion, DP-SLAM, etc. While Mask-RCNN is impressive and can provide good object masks, the algorithm cannot run in real-time. Some semantic SLAM systems, such as DS-SLAM, utilize SegNet to obtain the masks of objects, which can provide pixel-wise masks in real-time. Based on the assumption that many man-made objects can fit a 3D bounding box, DynaSLAM II adopts the CNN 2D bounding box to optimize the calculated 3D bounding box, and the literature mentioned that YOLACT can be used to provide high-quality instance segmentation in real time.

YOLO (You Only Look Once) and SSD (Single Shot MultiBox Detector) are two popular object detection networks in the construction of semantic SLAM systems, because the two detectors have achieved a good balance between speed and accuracy. CubeSLAM uses YOLO detector for indoor scenarios, Detect-SLAM and Dynamic-SLAM adopt different strategies to improve the existing SSD detector to meet specific task requirements.

In fact, many excellent semantic segmentation networks can be combined with SLAM. Unet proposes the encoder-decoder architecture with skip connections from the contracting path to the expanding path. BiSeNet decouples the function of spatial information preservation and receptive field offering into a spatial path and a context path. BiSeNet V2 proposes a two-pathway architecture to treat low-level details and high-level semantics separately, and designs a guided aggregation layer to merge the two features. DeepLab v3 employs atrous convolution with upsampled filters to extract dense feature maps and capture long range context. DeepLab v3+ extends DeepLab v3 by adding a simple yet effective decoder module to recover the object boundaries.

With the development of SLAM sensors, it is a trend to construct a semantic segmentation network using multi-modal images. FuseNet incorporates depth information into a semantic segmentation framework as complement of appearance information. Depth-aware CNN handles geometric information by introducing depth-aware convolution and depth-aware average pooling operations. MFNet addresses the semantic segmentation of urban scenes using multi-spectral images, and a RGB-Thermal dataset was published to benchmark the method. RTFNet takes advantage that the quality of thermal images would not be degraded under unsatisfied lighting conditions, and integrates both the RGB and thermal information into a [deep neural network](https://www.sciencedirect.com/topics/chemical-engineering/deep-neural-network).

### 2.4. Differences from related works

Unlike most existing dynamic SLAM systems, which focus on improving the accuracy of camera localization, our work concentrates on building a SLAM system that can obtain a clean and accurate point cloud map in dynamic environments. The depth images used in the experiments have some depth missing regions and are not suitable for performing semantic segmentation on RGB-D images using multi-modal networks. Like most semantic SLAM systems mentioned above, we only perform semantic segmentation on [RGB images](https://www.sciencedirect.com/topics/engineering/rgb-image), and BlitzNet is our choice after balancing performance and real-time. The original masks obtained by other semantic segmentation networks can also be combined with our method to construct point cloud map without noise blocks.

The most similar SLAM system to Blitz-SLAM is the work presented in. To our knowledge, this is the first dynamic SLAM system designed to eliminate the noise blocks formed by moving objects. However, this system did not solve the problem of environmental points being polluted by object semantic information, so the system could not construct a suitable semantic map. Another drawback of it is that the extension of the masks is restricted in the bounding boxes of moving objects, which directly causes the extended masks not to cover the regions of moving objects effectively. Different from, by making full use of the semantic and geometric information of the original masks, RGB and depth images, Blitz-SLAM obtains two kinds of masks, namely depth masks and revised masks. The semantic map can be generated by mapping the semantic information of the depth masks to the point cloud map. The revised masks can cover the regions of moving objects completely.

## 3\. System description

### 3.1. Overview of blitz-SLAM

The overview of our proposed semantic SLAM system is shown in. In this system, the [RGB images](https://www.sciencedirect.com/topics/engineering/rgb-image) are firstly fed into BlitzNet from which the bounding boxes and original masks of the objects in the environment are obtained simultaneously. BlitzNet uses ResNet-50 as the backbone network, because of its high performance and good trade-off for speed. BlitzNet has been trained on PASCAL VOC (VOC2007, VOC2012) dataset, and a total of 20 classes can be segmented and detected. BlitzNet can detect something common in ordinary indoor scenarios, such as people, furniture, etc.

![Fig. 1](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr1.jpg)

Download: Download high-res image (717KB)

According to the state of objects in the environment, objects can be roughly divided into three categories:
- 1.
	Moving objects: such as walking people. These objects not only interfere with the camera pose estimation directly, but also leave a large number of smears in the constructed environment map.
- 2.
	Static objects: such as tables, monitors, etc. These objects are often at the same positions in the environment and would not be moved frequently.
- 3.
	Movable objects: such as chairs, books, etc. These objects can be either moving or static.

Since the original object mask obtained by BlitzNet is imperfect, the depth information of the object region is utilized to improve the original mask to obtain the revised mask that can completely cover the object region. The clear local point cloud can be obtained after removing the noise blocks caused by moving objects. The flow is marked with orange arrows in.

In the process of camera localization, images can be divided into environment regions and potential dynamic regions quickly by the bounding boxes of the moving objects. We construct [epipolar constraints](https://www.sciencedirect.com/topics/engineering/epipolar-constraint) by the matching points in the environment regions to classify the static and dynamic matching points in the potential dynamic regions. After that, static matching points are used for camera localization. The flow is marked with blue arrows in.

The cyan arrows in reprsent the process of constructing global point cloud map. The essence of constructing global point cloud map is to merge the local point clouds corresponding to each group of key frames, as follows:(1) $P_{E} = \sum_{i = 1}^{n} \left(\right. R_{i} P_{i} + t_{i} \left.\right)$ where $P_{i}$ represents the local point cloud, $P_{E}$ indicates the global point cloud map, the [rotation matrix](https://www.sciencedirect.com/topics/engineering/rotation-matrix) $R_{i}$ and the translation matrix $t_{i}$ are determined by the pose of the camera relative to the reference coordinate system. Usually, the camera coordinate system of the first group of key frames is selected as the reference coordinate system.

The quality of the global point cloud map is related to three factors: the camera [localization accuracy](https://www.sciencedirect.com/topics/engineering/localization-accuracy), the removal of noise blocks in the local point clouds, and the completeness of the tracked camera trajectory.

### 3.2. Depth region segmentation

Due to the limitation of the depth camera, some regions in the depth image lose their depth values. And when the surface of an object is very smooth, the depth values of the object would also be seriously missing.

Based on the assumption that objects in the scene, especially the artificial ones are convex to a large extent, edge points can be placed at discontinuities in the depth image. Our depth image segmentation method as follows:

A slider of size $2 \times 2$ is used to traverse the depth image and the depth values in the slider are recorded, as follows:(2) $D_{b l o c k} = d \left(\right. u : u + 1 , v : v + 1 \left.\right)$ where $\left(\right. u , v \left.\right)$ represent the image coordinates corresponding to the pixel in the upper left corner of the slider. The edge points can be obtained by:(3) $\left{\right. max \left(\right. D_{b l o c k} \left.\right) - min \left(\right. D_{b l o c k} \left.\right) > \tau_{1} , & \left(\right. u , v \left.\right) \in E d g e \\ e l s e , & \left(\right. u , v \left.\right) \notin E d g e$ where $\tau_{1}$ is a preset threshold value, in this paper $\tau_{1} = 500$ (0.1m), and $E d g e$ represents the set of edge points.

### 3.3. Removal of depth unstable regions

The accuracy of the depth camera decreases as the distance between the object and the camera increases. To observe the influence of the depth unstable regions on the point cloud, we utilize a set of RGB and depth images selected form TUM dataset to obtain the local point cloud, as shown in. From the front view, the point cloud of the distant plank occupies a larger region compared to the point cloud of the desk plank. From the top view, the point cloud of the distant plank has a serious layering phenomenon, while the point cloud of the desk plank is more compact. When multiple local point clouds are merged, the layering phenomenon of the distant plank would be more serious, which directly affect the visual effect of the global point cloud map.

![Fig. 2](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr2.jpg)

Download: Download high-res image (178KB)

To ensure the quality of the obtained point cloud map, we need to remove the depth unstable regions. According to the noise model of the depth camera and empirical value obtained in our experiments, we set the depth value of a point in the depth image to 0 when the depth value is greater than 30,000 (6m).

### 3.4. Depth mask

The original mask of object obtained by BlitzNet has two problems: first, the original mask cannot completely cover the object; second, the region covered by the original mask also contains some information of the environment and other objects. So the semantic point cloud map constructed directly by the original masks has the following two features:

1\. The information of the moving objects would be leaked into the environment, forming a lot of noise blocks;

2\. Part of the semantic information would be mapped to other environmental points if the masks exceeds the region of the static objects.

As shown in, the original mask of the swivel chair covers part of the body of the sitting person, while the original mask of the ordinary chair covers only part of the chair itself. We mapped the semantic information provided by the original masks of the two chairs to the current point cloud and it could be found that some environmental points are also dyed red.

![Fig. 3](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr3.jpg)

Download: Download high-res image (340KB)

We use the depth information of the region covered by the original mask $M^{o}$ to improve the mask, and obtain the depth mask $M^{d}$, as follows:

Firstly, the original masks of the objects are used to label the objects in the image to obtain the labels of the objects, such as $\left{\right. O b j \left(\right. 1 \left.\right) , \ldots , O b j \left(\right. k \left.\right) \left.\right}$.

Then, the depth values in the original mask $M_{O b j \left(\right. i \left.\right)}^{o}$ are counted, and the 0 values and outliers are removed to obtain the depth value set $d_{O b j \left(\right. i \left.\right)}^{o}$. Find the pixels in the depth image with the same depth range as $O b j \left(\right. i \left.\right)$, as follows:(4) $\left{\right. L_{d} - \tau_{2} \leq d \left(\right. u , v \left.\right) \leq U_{d} + \tau_{2} , & \left(\right. u , v \left.\right) \in M_{O b j \left(\right. i \left.\right)}^{s} \\ e l s e , & \left(\right. u , v \left.\right) \notin M_{O b j \left(\right. i \left.\right)}^{s}$ where $U_{d}$ and $L_{d}$ are the maximum and minimum values of $d_{O b j \left(\right. i \left.\right)}^{o}$, respectively. $d \left(\right. u , v \left.\right)$ represents the depth value of $\left(\right. u , v \left.\right)$ and $M_{O b j \left(\right. i \left.\right)}^{s}$ indicates the regions with the same depth range as $O b j \left(\right. i \left.\right)$. In order to increase the robustness of the system, the depth value range of $O b j \left(\right. i \left.\right)$ is extended for $2 \star \tau_{2}$, in this paper $\tau_{2} = 500 \left(\right. 0.1 m \left.\right)$.

$M_{O b j \left(\right. i \left.\right)}^{s}$ obtained by Equ may be composed of multiple image blocks $M_{i}$ with the same depth range, namely:(5) $M_{O b j \left(\right. i \left.\right)}^{s} = \cup_{i = 1}^{m} M_{i}$

Finally, constructing semantic constraint to save the image blocks belong to the depth mask $M_{O b j \left(\right. i \left.\right)}^{d}$, as follows:(6) $\left{\right. \frac{P_{M_{i}}^{S}}{P_{M_{i}}} \geq \tau_{3} & M_{i} \in M_{O b j \left(\right. i \left.\right)}^{d} \\ e l s e , & M_{i} \notin M_{O b j \left(\right. i \left.\right)}^{d}$ where $P_{M_{i}}^{S}$ is the number of pixels with semantic information of $O b j \left(\right. i \left.\right)$ in $M_{i}$, $P_{M_{i}}$ is the number of pixels in $M_{i}$, $\tau_{3}$ is a preset threshold value, in this paper $\tau_{3} = 0.25$. The depth mask acquisition algorithm is shown in.

![Algorithm 1](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr17.jpg)

Download: Download high-res image (135KB)

The revised masks $M_{O b j \left(\right. i \left.\right)}^{r}$ is the union of the original mask $M_{O b j \left(\right. i \left.\right)}^{o}$ and the depth mask $M_{O b j \left(\right. i \left.\right)}^{d}$, namely:(7) $M_{O b j \left(\right. i \left.\right)}^{r} = M_{O b j \left(\right. i \left.\right)}^{o} \cup M_{O b j \left(\right. i \left.\right)}^{d}$

If $O b j \left(\right. i \left.\right)$ is determined as a moving object, what need to be removed is the region covered by $M_{O b j \left(\right. i \left.\right)}^{r}$ when eliminating the information of $O b j \left(\right. i \left.\right)$. If $O b j \left(\right. i \left.\right)$ is a static object, the semantic information of the region covered by $M_{O b j \left(\right. i \left.\right)}^{d}$ should be mapped when constructing a semantic point cloud map.

shows the revision process of the human masks. The input are the original masks of two people and the corresponding depth image displayed in the orange box. The label of the sitting person is $P \left(\right. 1 \left.\right)$, and the label of the walking person is $P \left(\right. 2 \left.\right)$. The obtained regions $M_{P \left(\right. 1 \left.\right)}^{s}$ and $M_{P \left(\right. 2 \left.\right)}^{s}$ are shown in the blue box, where $M_{P \left(\right. 1 \left.\right)}^{s}$ is composed of multiple image blocks. After excluding redundant blocks by Equ, the depth mask $M_{P \left(\right. 1 \left.\right)}^{d}$ can be obtained. Finally, the depth masks and the original masks are fused to obtain the revised masks.

![Fig. 4](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr4.jpg)

Download: Download high-res image (229KB)

### 3.5. Judgment of interaction between moving people and movable object

When the revised mask of a person intersects with the revised mask of a movable object, it is considered that they two interact with each other, namely:(8) $\left{\right. M_{P \left(\right. i \left.\right)}^{r} \cap M_{C_{O b j} \left(\right. j \left.\right)}^{r} \neq \emptyset , & C_{O b j} \left(\right. j \left.\right) \in D_{O b j} \\ e l s e , & C_{O b j} \left(\right. j \left.\right) \in S_{O b j}$ where $i = 1 , \ldots , n$, $n$ is the number of people in the image, $j = 1 , \ldots , m$, $m$ is the number of the pre-defined movable objects in the image, $D_{O b j}$ is the set of moving objects, $S_{O b j}$ is the set of static objects. The human and movable object interaction judgment algorithm is shown in.

![Algorithm 2](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr18.jpg)

Download: Download high-res image (181KB)

shows the diagram of judging whether the person interacts with the chairs. The revised masks of the two people and two chairs are displayed in the red box. The label of the swivel chair is $C \left(\right. 1 \left.\right)$, while the label of the ordinary chair is $C \left(\right. 2 \left.\right)$, and the labels of the two people are consistent with that in. After judgment, $P \left(\right. 1 \left.\right)$ interacts with $C \left(\right. 1 \left.\right)$, that is, $C \left(\right. 1 \left.\right)$ is a moving object.

![Fig. 5](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr5.jpg)

Download: Download high-res image (143KB)

We set the depth values of the regions covered by the revised masks of the two people and the swivel chair in the depth image to 0 and map the semantic information of the ordinary chair to the current point cloud. As shown in, the moving objects is effectively removed, and the semantic information of the ordinary chair does not pollute other environmental points.

![Fig. 6](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr6.jpg)

Download: Download high-res image (126KB)

### 3.6. Removal of the residual information of moving objects

Part of the boundary information of the moving objects may be leaked into the environment occasionally after removing the regions covered by the revised masks. The shape of these noise blocks formed by the residual boundaries is long and narrow. The method flow of removing the boundary information we adopted is shown in. The input is the RGB images and the corresponding depth images after removing the regions covered by the revised masks of the moving objects, and the output is the depth images without the noise blocks.

![Fig. 7](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr7.jpg)

Download: Download high-res image (480KB)

Firstly, we match the feature points extracted in the environment regions of the previous frame $I_{P}$ and the current frame $I_{C}$. The [homography](https://www.sciencedirect.com/topics/engineering/homography) matrix $H$ between $I_{P}$ and $I_{C}$ can be estimated by RANSAC and LM algorithms.

Suppose $p_{P} = \left(\left[\right. u_{P} , v_{P} , 1 \left]\right.\right)^{T}$ is the coordinates of a point on $I_{P}$, the coordinates $p_{C} = \left(\left[\right. u_{C} , v_{C} , 1 \left]\right.\right)^{T}$ corresponding to the point on $I_{C}$ can be obtained by the following formula:(9) $p_{C} = H p_{P}$

The depth values of the residual boundaries in the adjacent frames often differ greatly. The noise blocks can be removed by the following formula:(10) $\left{\right. \left|\right. d_{C} \left(\right. u_{C} , v_{C} \left.\right) - d_{P} \left(\right. u_{P} , v_{P} \left.\right) \left|\right. > \tau_{4} , & d_{C} \left(\right. u_{C} , v_{C} \left.\right) = 0 \\ e l s e , & d_{C} \left(\right. u_{C} , v_{C} \left.\right) = d_{C} \left(\right. u_{C} , v_{C} \left.\right)$ where $d_{P}$ and $d_{C}$ are the depth images corresponding to $I_{P}$ and $I_{C}$, respectively. $\tau_{4}$ is a preset threshold value, in this paper $\tau_{4} = 5 , 000 \left(\right. 1 m \left.\right)$.

Finally, we remove the isolated image blocks containing less than 1,000 pixels in the depth images through [morphological operations](https://www.sciencedirect.com/topics/engineering/morphological-operations) to ensure that the information of the moving objects is completely removed. The moving objects residual information removal algorithm is shown in. shows two obtained local point clouds before and after removing the noise blocks, and the noise blocks are marked with red boxes.

![Algorithm 3](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr19.jpg)

Download: Download high-res image (198KB)

![Fig. 8](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr8.jpg)

Download: Download high-res image (236KB)

Every time the person move the movable objects, the position of them would be different from that before the movement. If this situation can not be handled, the timely redundant information of these movable objects would be appeared in the global point cloud map multiple times. The approach we have taken is to keep only the most recent information of these objects, and they have no interaction with people during this period.

shows the four stages of the state change of the two chairs, information of the two chairs that need to be retained is in the green frame. In the initial stage, the chairs are static objects, and the information is kept in the global map. In the second stage, two people enter the camera’s view field and interact with the chairs. The chairs are dynamic objects and do not participate in the map construction. At this stage, the chair information in the map are the chairs in the first red dashed box. In the third stage, the two people leave the camera’s view field and the chairs became stationary. Because the positions of the two chairs have changed, the chair information in the first stage is deleted from the global map, and the current chair information is retained. In the final stage, the two people interact with the chairs again, as in the second stage, the chair information saved in the global map is the chairs in the green solid box.

![Fig. 9](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr9.jpg)

Download: Download high-res image (299KB)

### 3.7. Location of the static matching points

After obtaining the two sets of static matching points $P_{p} = \left{\right. P_{p 1} , P_{p 2} , \ldots , P_{p n} \left.\right}$ and $P_{c} = \left{\right. P_{c 1} , P_{c 2} , \ldots , P_{c n} \left.\right}$ in the previous frame $I_{P}$ and the current frame $I_{C}$, the camera pose between $I_{P}$ and $I_{C}$ can be estimated by solving the following [least squares problem](https://www.sciencedirect.com/topics/engineering/least-square-problem):(11) $\underset{R , t}{min} \sum_{i = 1}^{N} \parallel P_{p i} - \left(\right. R P_{c i} + t \left.\right) \parallel^{2}$

Firstly, we extract matching points from the environment regions of $I_{P}$ and $I_{C}$. The [fundamental matrix](https://www.sciencedirect.com/topics/engineering/fundamental-matrix) $F$ between $I_{P}$ and $I_{C}$ can be calculated by RANSAC algorithm.

Then, we extract the matching point sets $P_{D P} = \left[\right. u_{D P} , v_{D P} , 1 \left]\right.$, $P_{D C} = \left[\right. u_{D C} , v_{D C} , 1 \left]\right.$ in the potential dynamic regions of $I_{P}$ and $I_{C}$.

After that, we calculate the distance from the potential dynamic matching points to the corresponding [epipolar line](https://www.sciencedirect.com/topics/engineering/epipolar-line) by(12) $d = \frac{\left|\right. P_{D C} F P_{D P}^{T} \left|\right.}{\sqrt{l_{x}^{2} + l_{y}^{2}}}$ where, the value of $l_{x}$ and $l_{y}$ can be obtained by(13) $\left(\left[\right. l_{x} , l_{y} , l_{z} \left]\right.\right)^{T} = F P_{D P}^{T}$

The state of the $i t h$ matching points in the potential dynamic regions can be determined by(14) $\left{\right. d_{i} > \tau_{5} & i \in D \\ e l s e & i \in S$ where, $D$ represents the set of dynamic matching points, $S$ indicates the set of static matching points. $\tau_{5}$ is a preset threshold value, in this paper, $\tau_{5} = 0.5$. The static matching points location algorithm is shown in.

![Algorithm 4](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr20.jpg)

Download: Download high-res image (235KB)

## 4\. Experimental results

In this section, we will compare the performance of Blitz-SLAM with ORB-SLAM2, Dyna-SLAM and DS-SLAM on the TUM RGB-D dataset. Both Dyna-SLAM and DS-SLAM are built on ORB-SLAM2, which are the two best solutions for vision-based semantic SLAM in dynamic environments. TUM datasets provide several dynamic scenarios with accurate camera trajectories and parameters. In the walking sequences, two persons walk back and forth, make gestures, and move chairs, etc. In the sitting sequences, the two people just sit on the chairs chatting and occasionally making gestures. In the desk sequences, a person enters the scene and sits on a chair, moving objects on the desk during the recording. Among these sequences, walking sequences represent the highly dynamic environments, while sitting and desk sequences depict the lowly dynamic environments. We choose four sets of walking sequences, three set of lowly dynamic sequences and a set of static sequences as the experimental materials. The scene of the static sequences is an office, without moving objects. The purpose of evaluations in the pure static sequences is to validate whether the proposed front end has a negative impact on ORB-SLAM2.

The camera has four motion modes: halfsphere, which means that the camera moved on a hemispherical trajectory; rpy, which indicates that the camera rotated around the principal axes; static, which represents that the camera was kept in place manually; and xyz, which means that the camera moved along the x, y, and z axes. The desk sequences were recorded by a walking person with a handheld camera. For the convenience of expression, we use fr3/w/half, fr3/w/rpy, fr3/w/static, fr3/w/xyz, fr3/s/half, fr3/s/xyz, fr2/desk/p and fr2/xyz to represent the eight sets of image sequences, where fr, s, w, p represent freiburg, sitting, walking, person, respectively. The computer we used is the Ubuntu 16.04 system with 8G [RAM](https://www.sciencedirect.com/topics/engineering/reliability-availability-and-maintainability-reliability-engineering) and a Core i5-6500 processor.

### 4.1. Evaluation of camera localization

In the experiments, Metrics Absolute Trajectory Error (ATE) and Relative Pose Error (RPE) are used for [quantitative evaluation](https://www.sciencedirect.com/topics/engineering/quantitative-evaluation). Let $P_{1} , \ldots , P_{n} \in S E \left(\right. 3 \left.\right)$ denote the sequence of the estimated camera poses, and $Q_{1} , \ldots , Q_{n} \in S E \left(\right. 3 \left.\right)$ denote the ground truth. The ATE measures the global consistency of the estimated trajectory, and ATE $F_{i}$ at time step $i$ can be computed by:(15) $F_{i} = Q_{i}^{- 1} S P_{i}$ where $S$ denotes the rigid-body transformation which aligns the estimated trajectory with the real one. The RPE measures the local accuracy of the estimated trajectory over a fixed time $\Delta$, and RPE $E_{i}$ at time step $i$ is defined as:(16) $E_{i} = \left(\left(\right. Q_{i}^{- 1} Q_{i + \Delta} \left.\right)\right)^{- 1} \left(\right. P_{i}^{- 1} P_{i + \Delta} \left.\right)$

First, we perform a quantitative analysis of the ATE and RPE of these four systems. The values of [Root Mean Square Error](https://www.sciencedirect.com/topics/earth-and-planetary-sciences/root-mean-square-error) (RMSE) and Standard Deviation (S.D.) of ATE and RPE of the four SLAM systems are presented in -. RMSE measures the deviation between the observed value and the true value, which reflects the robustness of the system. S.D. measures the extent of deviation for a group as a whole, which reflects the stability of the system. The improvement values in the tables are calculated as follows:(17) $\kappa = \left(\right. 1 - \frac{\beta}{\alpha} \left.\right) \times 100 \%$ where $\kappa$ denotes the improvement value, $\alpha$ is the value obtained by ORB-SLAM2, $\beta$ represents the value of Blitz-SLAM.

Table 1. Results of absolute trajectory error (ATE).

<table><thead><tr><th>Sequences</th><th colspan="2">ORB-SLAM2</th><th colspan="2">Dyna-SLAM</th><th colspan="2">DS-SLAM</th><th colspan="2">Blitz-SLAM</th><th colspan="2">Improvements</th></tr><tr><td>Empty Cell</td><th>RMSE</th><th>S.D.</th><th>RMSE</th><th>S.D.</th><th>RMSE</th><th>S.D.</th><th>RMSE</th><th>S.D.</th><th>RMSE</th><th>S.D.</th></tr></thead><tbody><tr><td>fr3/w/half</td><td>0.4543</td><td>0.2524</td><td>0.0296</td><td>0.0157</td><td>0.0303</td><td>0.0159</td><td><strong>0.0256</strong></td><td><strong>0.0126</strong></td><td><strong>94.36%</strong></td><td><strong>95.01%</strong></td></tr><tr><td>fr3/w/rpy</td><td>0.5391</td><td>0.2283</td><td><strong>0.0354</strong></td><td><strong>0.0190</strong></td><td>0.4442</td><td>0.2350</td><td>0.0356</td><td>0.0220</td><td><strong>93.40%</strong></td><td><strong>90.36%</strong></td></tr><tr><td>fr3/w/static</td><td>0.3194</td><td>0.1819</td><td><strong>0.0068</strong></td><td><strong>0.0032</strong></td><td>0.0081</td><td>0.0033</td><td>0.0102</td><td>0.0052</td><td><strong>96.81%</strong></td><td><strong>97.14%</strong></td></tr><tr><td>fr3/w/xyz</td><td>0.7521</td><td>0.4712</td><td>0.0164</td><td>0.0086</td><td>0.0247</td><td>0.0161</td><td><strong>0.0153</strong></td><td><strong>0.0078</strong></td><td><strong>97.97%</strong></td><td><strong>98.34%</strong></td></tr><tr><td>fr3/s/half</td><td>0.0182</td><td>0.0110</td><td>0.0186</td><td>0.0086</td><td>0.0166</td><td>0.0077</td><td><strong>0.0160</strong></td><td><strong>0.0076</strong></td><td><strong>12.09%</strong></td><td><strong>30.91%</strong></td></tr><tr><td>fr3/s/xyz</td><td><strong>0.0092</strong></td><td><strong>0.0044</strong></td><td>0.0127</td><td>0.0060</td><td>0.0115</td><td>0.0056</td><td>0.0148</td><td>0.0069</td><td><strong>-60.87%</strong></td><td><strong>-56.82%</strong></td></tr><tr><td>fr2/desk/p</td><td><strong>0.0060</strong></td><td>0.0027</td><td>0.0074</td><td>0.0038</td><td>0.0062</td><td>0.0027</td><td>0.0063</td><td><strong>0.0026</strong></td><td><strong>-5.00%</strong></td><td><strong>3.70%</strong></td></tr><tr><td>fr2/xyz</td><td>0.0039</td><td>0.0018</td><td>0.0042</td><td>0.0021</td><td>0.0036</td><td><strong>0.0016</strong></td><td><strong>0.0035</strong></td><td><strong>0.0016</strong></td><td><strong>10.26%</strong></td><td><strong>11.11%</strong></td></tr></tbody></table>

Table 2. Results of translational relative pose error (RPE).

<table><thead><tr><th>Sequences</th><th colspan="2">ORB-SLAM2</th><th colspan="2">Dyna-SLAM</th><th colspan="2">DS-SLAM</th><th colspan="2">Blitz-SLAM</th><th colspan="2">Improvements</th></tr><tr><td>Empty Cell</td><th>RMSE</th><th>S.D.</th><th>RMSE</th><th>S.D.</th><th>RMSE</th><th>S.D.</th><th>RMSE</th><th>S.D.</th><th>RMSE</th><th>S.D.</th></tr></thead><tbody><tr><td>fr3/w/half</td><td>0.3216</td><td>0.2629</td><td>0.0284</td><td>0.0149</td><td>0.0297</td><td>0.0152</td><td><strong>0.0253</strong></td><td><strong>0.0123</strong></td><td><strong>92.13%</strong></td><td><strong>95.32%</strong></td></tr><tr><td>fr3/w/rpy</td><td>0.3880</td><td>0.2823</td><td><strong>0.0448</strong></td><td><strong>0.0262</strong></td><td>0.1503</td><td>0.1168</td><td>0.0473</td><td>0.0283</td><td><strong>87.81%</strong></td><td><strong>89.98%</strong></td></tr><tr><td>fr3/w/static</td><td>0.1928</td><td>0.1773</td><td><strong>0.0089</strong></td><td>0.0044</td><td>0.0102</td><td><strong>0.0038</strong></td><td>0.0129</td><td>0.0069</td><td><strong>93.31%</strong></td><td><strong>96.11%</strong></td></tr><tr><td>fr3/w/xyz</td><td>0.4834</td><td>0.3663</td><td>0.0217</td><td>0.0119</td><td>0.0333</td><td>0.0229</td><td><strong>0.0197</strong></td><td><strong>0.0096</strong></td><td><strong>95.92%</strong></td><td><strong>97.38%</strong></td></tr><tr><td>fr3/s/half</td><td>0.2113</td><td>0.0142</td><td>0.0239</td><td>0.0120</td><td>0.0177</td><td>0.0084</td><td><strong>0.0165</strong></td><td><strong>0.0073</strong></td><td><strong>92.19%</strong></td><td><strong>48.59%</strong></td></tr><tr><td>fr3/s/xyz</td><td><strong>0.0117</strong></td><td><strong>0.0057</strong></td><td>0.0142</td><td>0.0073</td><td>0.0133</td><td>0.0069</td><td>0.0144</td><td>0.0071</td><td><strong>-23.08%</strong></td><td><strong>-24.56%</strong></td></tr><tr><td>fr2/desk/p</td><td><strong>0.0065</strong></td><td><strong>0.0034</strong></td><td>0.0075</td><td>0.0039</td><td>0.0066</td><td>0.0035</td><td>0.0067</td><td>0.0035</td><td><strong>-3.08%</strong></td><td><strong>-2.94%</strong></td></tr><tr><td>fr2/xyz</td><td>0.0032</td><td>0.0015</td><td>0.0035</td><td>0.0017</td><td><strong>0.0031</strong></td><td>0.0015</td><td><strong>0.0031</strong></td><td><strong>0.0014</strong></td><td><strong>3.13%</strong></td><td><strong>6.67%</strong></td></tr></tbody></table>

Table 3. Results of rotational relative pose error (RPE).

<table><thead><tr><th>Sequences</th><th colspan="2">ORB-SLAM2</th><th colspan="2">Dyna-SLAM</th><th colspan="2">DS-SLAM</th><th colspan="2">Blitz-SLAM</th><th colspan="2">Improvements</th></tr><tr><td>Empty Cell</td><th>RMSE</th><th>S.D.</th><th>RMSE</th><th>S.D.</th><th>RMSE</th><th>S.D.</th><th>RMSE</th><th>S.D.</th><th>RMSE</th><th>S.D.</th></tr></thead><tbody><tr><td>fr3/w/half</td><td>6.6515</td><td>5.3990</td><td><strong>0.7842</strong></td><td>0.4012</td><td>0.8142</td><td>0.4101</td><td>0.7879</td><td><strong>0.3751</strong></td><td><strong>88.15%</strong></td><td><strong>93.05%</strong></td></tr><tr><td>fr3/w/rpy</td><td>7.5906</td><td>5.4768</td><td><strong>0.9894</strong></td><td><strong>0.5701</strong></td><td>3.0042</td><td>2.3065</td><td>1.0841</td><td>0.6668</td><td><strong>85.72%</strong></td><td><strong>87.83%</strong></td></tr><tr><td>fr3/w/static</td><td>3.5991</td><td>3.2457</td><td><strong>0.2612</strong></td><td>0.1259</td><td>0.2690</td><td><strong>0.1215</strong></td><td>0.3038</td><td>0.1437</td><td><strong>91.56%</strong></td><td><strong>95.57%</strong></td></tr><tr><td>fr3/w/xyz</td><td>8.8419</td><td>6.6762</td><td>0.6284</td><td>0.3848</td><td>0.8266</td><td><strong>0.2826</strong></td><td><strong>0.6132</strong></td><td>0.3348</td><td><strong>93.06%</strong></td><td><strong>94.99%</strong></td></tr><tr><td>fr3/s/half</td><td><strong>0.5769</strong></td><td><strong>0.2704</strong></td><td>0.7045</td><td>0.3488</td><td>0.6098</td><td>0.2850</td><td>0.5981</td><td>0.2739</td><td><strong>-3.67%</strong></td><td><strong>-1.29%</strong></td></tr><tr><td>fr3/s/xyz</td><td><strong>0.4874</strong></td><td><strong>0.2532</strong></td><td>0.5042</td><td>0.2651</td><td>0.5044</td><td>0.2623</td><td>0.5024</td><td>0.2634</td><td><strong>-3.08%</strong></td><td><strong>-4.03%</strong></td></tr><tr><td>fr2/desk/p</td><td>0.4285</td><td><strong>0.2294</strong></td><td>0.4427</td><td>0.2378</td><td><strong>0.4275</strong></td><td>0.2303</td><td>0.4325</td><td>0.2302</td><td><strong>-0.93%</strong></td><td><strong>-0.35%</strong></td></tr><tr><td>fr2/xyz</td><td>0.3103</td><td><strong>0.1565</strong></td><td>0.3172</td><td>0.1589</td><td><strong>0.3062</strong></td><td>0.1567</td><td>0.3065</td><td>0.1576</td><td><strong>1.22%</strong></td><td><strong>-0.70%</strong></td></tr></tbody></table>

and present the results of ATE and translational RPE, respectively. Blitz-SLAM achieved the best results in fr3/w/half, fr3/w/xyz, fr3/s/half and fr2/xyz. In fr3/w/rpy, the results of Blitz-SLAM are second to Dyna-SLAM. In fact, the results of these four systems in the lowly dynamic and static sequences are very close.

shows the results of rotational RPE. In the highly dynamic sequences, overall, Dyna-SLAM performs best. Blitz-SLAM achieved the best result on the RMSE indicator in fr3/w/xyz and the best value on the S.D. indicator in fr3/w/half. And it should be noticed that in fr3/w/rpy, the results of Blitz-SLAM are second to Dyna-SLAM. In the lowly dynamic sequences, ORB-SLAM2 performs better than these three dynamic SLAM systems. In fr3/s/half, the results of Blitz-SLAM are second to ORB-SLAM2.

According to the results obtained by -, we can inform that in the highly dynamic environments the camera pose estimation would be greatly improved after eliminating the moving objects. Dyna-SLAM, DS-SLAM and Blitz-SLAM have achieved good pose estimation results in the four sets of walking sequences. For Blitz-SLAM, the average RMSE improvement values for the ATE, translational RPE and rotational RPE in the walking sequences are 95.64%, 92.29% and 89.62%, respectively. The average S.D. improvement values for the ATE, translational RPE and rotational RPE in the walking sequences are 95.21%, 94.70% and 92.86%, respectively. This demonstrates that the proposed dynamic objects removal front end can effectively improve the performance of ORB-SLAM2 in the highly dynamic environments. Meanwhile, we find that our approach has no obvious or negative effect on the improvement of ORB-SLAM2 in the lowly dynamic environments. As mentioned above, outliers can be removed effectively by RANSAC method in the lowly dynamic environments, which makes ORB-SLAM2 work well, so the space for improvement of camera localization is very limited. In the pure static sequences, the performance of the four SLAM systems is similar.

shows the estimated trajectories of the four SLAM systems in the four highly dynamic sequences. Black line represents the ground truth, blue line indicates the estimated trajectory and red line represents the difference between the ground truth and the estimated trajectory. We can intuitively see from first row that ORB-SLAM2 cannot deal with highly dynamic environments effectively. Compared with ORB-SLAM2, the errors of the camera trajectories estimated by the other three SLAM systems are reduced significantly.

![Fig. 10](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr10.jpg)

Download: Download high-res image (2MB)

It can be seen from that all these four SLAM systems have lost part of the trajectory points. In order to quantitatively compare the trajectory points tracked by the four systems, we give the results of their successfully tracked trajectory points, as shown in. The trajectory points tracked by DS-SLAM and our proposed system are the most complete compared with ORB-SLAM2 and Dyna-SLAM in the highly dynamic sequences. ORB-SLAM2, DS-SLAM and Blitz-SLAM tracked the same number of trajectory points in the pure static sequences. Although Dyna-SLAM has achieved the best tracking results in fr3/s/xyz and fr2/desk/p, it tracked fewer trajectory points compared with the other three SLAM systems in the other six sequences, especially in the walking sequences. In practice, especially long-term navigation, better coverage of trajectory points is more important than a slight improvement in accuracy. In the highly dynamic sequences, the average ratio of successfully tracked trajectory points of Blitz-SLAM is 99.74%. In the lowly dynamic sequences, the average ratio of successfully tracked trajectory points of Blitz-SLAM is 96.77%.

Table 4. Results of successfully tracked trajectory points.

<table><thead><tr><th>Sequences</th><th>Total</th><th colspan="2">ORB-SLAM2</th><th colspan="2">Dyna-SLAM</th><th colspan="2">DS-SLAM</th><th colspan="2">Ours</th></tr><tr><td>Empty Cell</td><td>Empty Cell</td><th>Tracked</th><th>Ratio</th><th>Tracked</th><th>Ratio</th><th>Tracked</th><th>Ratio</th><th>Tracked</th><th>Ratio</th></tr></thead><tbody><tr><td>fr3/w/half</td><td>1021</td><td>942</td><td>92.26%</td><td>1011</td><td>99.02%</td><td><strong>1018</strong></td><td><strong>99.71%</strong></td><td><strong>1018</strong></td><td><strong>99.71%</strong></td></tr><tr><td>fr3/w/rpy</td><td>866</td><td>825</td><td>95.27%</td><td>711</td><td>82.10%</td><td><strong>864</strong></td><td><strong>99.77%</strong></td><td><strong>864</strong></td><td><strong>99.77%</strong></td></tr><tr><td>fr3/w/static</td><td>717</td><td><strong>714</strong></td><td><strong>99.58%</strong></td><td>696</td><td>97.07%</td><td><strong>714</strong></td><td><strong>99.58%</strong></td><td><strong>714</strong></td><td><strong>99.58%</strong></td></tr><tr><td>fr3/w/xyz</td><td>827</td><td>809</td><td>97.82%</td><td>757</td><td>91.54%</td><td><strong>826</strong></td><td><strong>99.88%</strong></td><td><strong>826</strong></td><td><strong>99.88%</strong></td></tr><tr><td>fr3/s/half</td><td>1074</td><td><strong>1069</strong></td><td><strong>99.53%</strong></td><td>1050</td><td>97.77%</td><td><strong>1069</strong></td><td><strong>99.53%</strong></td><td>1068</td><td>99.44%</td></tr><tr><td>fr3/s/xyz</td><td>1218</td><td>1215</td><td>99.75%</td><td><strong>1218</strong></td><td><strong>100%</strong></td><td>1215</td><td>99.75%</td><td>1215</td><td>99.75%</td></tr><tr><td>fr2/desk/p</td><td>3983</td><td><strong>3631</strong></td><td><strong>91.16%</strong></td><td><strong>3631</strong></td><td><strong>91.16%</strong></td><td>3629</td><td>91.11%</td><td>3629</td><td>91.11%</td></tr><tr><td>fr2/xyz</td><td>3669</td><td><strong>3615</strong></td><td><strong>98.53%</strong></td><td>3362</td><td>91.63%</td><td><strong>3615</strong></td><td><strong>98.53%</strong></td><td><strong>3615</strong></td><td><strong>98.53%</strong></td></tr></tbody></table>

### 4.2. Evaluation of the point cloud map

qualitatively displays the results of motion removal in the TUM walking sequences. The first row shows the RGB images selected from the four sets of walking sequences. The second and third rows show the original masks obtained by BlitzNet and the revised masks generated by our approach, respectively. It can be seen that the boundaries of the original masks are smooth, while the boundaries of the revised masks are sharper. The original masks cannot cover some parts of the moving objects, such as the legs of the pedestrian in the first column, the feet and back of the sitting person in the third column, and the arm of the person on the left in the fourth column. Compared with the original masks, the revised masks can cover the regions of moving objects more completely. The fourth row of images are the local point clouds obtained after removing the regions covered by the revised masks of the moving objects, in which the smears formed by the moving objects have been removed.

![Fig. 11](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr11.jpg)

Download: Download high-res image (565KB)

As shown in, we present the three views of the global point cloud maps obtained by ORB-SLAM2, Dyna-SLAM, DS-SLAM, and Blitz-SLAM in the highly dynamic sequences fr3/w/xyz. Most of the information of the two persons is retained in the point cloud map constructed by ORB-SLAM2, which looked messy. You cannot even point out what objects exist in the point cloud map and the plank of the desk is severely twisted.

![Fig. 12](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr12.jpg)

Download: Download high-res image (1MB)

Dyna-SLAM and DS-SLAM removed the interference of the two persons, but due to the imperfect semantic segmentation, the boundary information of the two persons was leaked into the point cloud maps. Compared with ORB-SLAM2, the noise blocks in the point cloud maps of Dyna-SLAM and DS-SLAM are relatively sparse, so you can see what objects there are in the environment clearly. However, in the navigation of unmanned systems, this point cloud map with a large number of noise blocks cannot be applied directly. The unmanned system must first determine whether the space occupied by these noise blocks can pass through or not, which is very difficult.

Because the boundary information leaked by the moving people in the environment has been removed, there is almost no residual information of persons in the point cloud map obtained by Blitz-SLAM. It can be found that two chairs appear many times in the point cloud maps obtained by Dyna-SLAM and DS-SLAM. This is because the positions of these two chairs are often changed by the two persons, and the information of the chairs at each different position is retained in the point cloud map. This phenomenon does not appear on the two computer monitors that have fixed positions in the environment. In our point cloud map, we only retain the most recent information of the chairs before the two persons interact with the two chairs. So it can be seen that the two chairs only appear once in our point cloud map.

The three views of the point cloud maps obtained by the four SLAM systems in the lowly dynamic sequences fr3/s/xyz are shown in. In the first column of images, we can easily identify the objects in the point cloud map and the desk plank is not distorted. This is because ORB-SLAM2 can locate the camera accurately in the lowly dynamic environments and the points are mapped to the correct position. However, the information of the two persons is completely retained in the point cloud map.

![Fig. 13](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr13.jpg)

Download: Download high-res image (897KB)

From the images in the second and third columns, it can be seen that Dyna-SLAM and DS-SLAM have removed most of the information of the two persons. But we can point out the contours of the two persons which are composed of the noise blocks. As can be seen from the images in the fourth column, the noise blocks formed by the two persons are removed. But it can be noticed that part of information of the legs of the person on the left still exists in the map. It is believed that there are two reasons for this phenomenon: first, in the semantic segmentation stage, the human body region covered by the original mask is too small; second, the person’s legs are very close to the table, and his left leg is under the table, which caused the failure of the subsequent geometric methods. Overall, compared with the other three SLAM systems, the global point cloud map obtained by Blitz-SLAM in the lowly dynamic sequences is the best.

After obtaining the global point cloud map without noise blocks, we can map the semantic information of the static objects to the global map, and generate the semantic map. Taking the point cloud map obtained by Blitz-SLAM in, as an example, we map the semantic information of the two chairs and the two monitors to the point cloud map, in which the chairs are dyed red and the monitors are dyed blue, as shown in. Since we only map the semantic information corresponding to the depth mask region of the object to the point cloud map, it can be obviously seen that the semantic information of the object has not been leaked into the environment and polluted other points.

![Fig. 14](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr14.jpg)

Download: Download high-res image (331KB)

### 4.3. Evaluation of processing time

For practical applications, real-time performance is an important indicator for evaluating the SLAM systems. Because it takes a lot of computing resources to build a global point cloud map, we use an offline map building method. We tested the time required for the sub-modules of the camera localization to process a frame; the results are shown in. It can be seen that in the three sub-modules of camera localization, the segmentation and detection of moving objects takes the longest time. As a whole, our camera localization module meets the requirements of real-time.

Table 5. Time evaluation.

| Module | ORB extraction and matching | Outliers rejection | BlitzNet | Total |
| --- | --- | --- | --- | --- |
| Time (s) | 0.022 | 0.005 | 0.054 | 0.081 |

### 4.4. Evaluation in the real-world environment

To demonstrate the effectiveness of Blitz-SLAM in the real world, we conduct extensive experiments in a laboratory environment with Xtion Pro-camera. The camera has been calibrated accurately. The size of the captured RGB images and that of depth images are both $640 \times 480$. In our experiments, the camera was moved by hand, and the camera would shake and rotate while moving. In the process of shooting, two people entered the camera field of view, so they interfered with the camera localization and the point cloud map construction.

shows the results of dynamic feature points rejection. In the first row, the environmental regions and potential dynamic regions are divided quickly by the bounding boxes of the two people. The second row shows the classification results of dynamic and static feature points. Most of the feature points on the moving people are judged as dynamic points and marked with red dots.

![Fig. 15](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr15.jpg)

Download: Download high-res image (568KB)

shows the constructed point cloud maps before and after removing the information of the two people, respectively. In the first row, the pictures on the wall are blocked by noise blocks formed by the moving people, and the wall is also severely distorted due to incorrect camera localization. In the second row, the interference of the people is completely removed, the static objects are clearly visible and the geometric structure of the room is well preserved.

![Fig. 16](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-gr16.jpg)

Download: Download high-res image (381KB)

## 5\. Conclusion

In this paper, we propose a semantic SLAM system working in dynamic environments, called Blitz-SLAM. The main idea of Blitz-SLAM is to use the semantic information to help the SLAM system to remove the interference caused by moving objects. The original masks and the depth information of the moving objects are combined to get the depth masks. The revised masks obtained by integrating the depth masks with the original masks can cover the regions of the moving objects effectively. Meanwhile, we propose a method to determine whether a movable object has been contacted with a person, which eliminates the phenomenon that the movable objects appear at different places in the global map. With the bounding boxes of moving objects, the images can be divided into environment regions and potential dynamic regions. The matching points in the environment regions are utilized to construct epipolar constraints to remove outliers in the potential dynamic regions. The experimental results show that Blitz-SLAM can obtain accurate camera poses and clean global point cloud maps in the dynamic environments.

However, some issues require further discussion: Firstly, the number and the distribution of matching points in the images will directly affect the results of the camera pose estimation. The optimal selection of matching points is the key to further improving the camera [localization accuracy](https://www.sciencedirect.com/topics/engineering/localization-accuracy). Secondly, at present, the moving objects in semantic SLAM systems are mainly pre-defined, which means that when an unknown moving object enters the camera’s view field, the camera pose estimation would be disturbed, and the map would be polluted by smears formed by the object. In this case, the camera pose can be estimated by multi-source sensor information fusion, such as the combination of IMU (Inertial Measurement Unit) and camera, and then process smears formed by the unknown moving objects in the map. Thirdly, our system is currently designed to handle indoor dynamic scenes, and has not been extended to outdoor scenes. Extending the application scenarios of the system is the next work, which would be meaningful for practical outdoor applications such as autonomous driving. Finally, considering that unmanned systems may interact with moving objects in practical applications, object-level SLAM would be one of our future research focuses. Tracing moving objects and accurately estimating their pose is beneficial for improving the robustness of SLAM systems in complex environments.

## Declaration of Competing Interest

All the authors do not have any possible conflicts of interest.

## Appendix A. Supplementary materials

<video controls=""><source src="https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-mmc1.mp4" type="video/mp4"></video> [Download: Download video (8MB)](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-mmc1.mp4 "Download video (8MB)")

Supplementary Data S1. Supplementary Raw Research Data. This is open data under the CC BY license [http://creativecommons.org/licenses/by/4.0/](http://creativecommons.org/licenses/by/4.0/)

## References

![image of the author](https://ars.els-cdn.com/content/image/1-s2.0-S0031320321004064-fx1.jpg)

**Yingchun Fan** received the B.S. degree in intelligence science and technology from the School of Electronic Engineering, Xidian University, Xi’an, China, in 2014 and the M.S. degree in optical engineering from the School of Physics and Information Technology, Shaanxi Normal University, Xi’an, China, in 2017. He is currently pursuing the Ph.D. degree in pattern recognition and intelligent system from the School of Artificial Intelligence, Xidian University, Xi’an, China.

**Qichi Zhang** received the B.S. degree in electronics and information engineering from Shenzhen University, China, in 2017. He is currently pursuing the M.S. degree in electronics and communication engineering from the School of Artificial Intelligence, Xidian University, Xi’an, China. His research interests include computer vision, simultaneously localization and mapping, and machine learning, with a focus on dynamic slam.

**Yuliang Tang** was born in 1994. He received his bachelor’s degree in Xi’an University of Science and Technology, Xi’an, China, in 2017. He is a graduate student in School of Artificial Intelligence, Xidian University. His research interests include SLAM, computer vision, and semantic segmentation.

**Shaofeng Liu** received his bachelor’s degree in NorthWestern Polytechnical University, Xi’an, China, in 2017. He is a graduate student in School of Artificial Intelligence, Xidian University. His reaserch interests include computer vision, image processing, and object detection.

**Hong Han** was born in 1974. She received the Ph.D. degree in electrical engineering from Xi’an Jiaotong University, Xi’an, China, in 2003. She is a Senior Researcher with the School of Artificial Intelligence, Xidian University, Xi’an. Her research interests include computer vision, information fusion, and machine learning.

Student with the School of Artificial Intelligence, Xidian University, Xi’an.

Senior Researcher with the School of Artificial Intelligence, Xidian University, Xi’an.

[View Abstract](https://www.sciencedirect.com/science/article/abs/pii/S0031320321004064)