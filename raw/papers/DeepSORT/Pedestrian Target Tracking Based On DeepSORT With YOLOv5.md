---
title: "Pedestrian Target Tracking Based On DeepSORT With YOLOv5"
source: "https://ieeexplore.ieee.org/abstract/document/9692002"
author:
  - "[[Yuqiao Gai]]"
  - "[[Weiyang He]]"
  - "[[Zilong Zhou]]"
published:
created: 2026-06-03
description: "Pedestrian target tracking is an important problem in the field of computer vision. To address low tracking accuracy and tracking errors in pedestrian target tr"
tags:
  - "clippings"
---
## Abstract:

Pedestrian target tracking is an important problem in the field of computer vision. To address low tracking accuracy and tracking errors in pedestrian target tracking. Th...

---

Target tracking is one of the hotspots in computer vision research. Over the past decades, target tracking research has achieved sufficient development, and pedestrian target tracking has been widely used in surveillance and other industries. In the current period of rapid development of deep learning technology, deep learning technology is also successfully applied in various application areas of computer vision, and tracking is no exception. In the context of big data, deep learning is used to train the network model, and the output of the convolutional features obtained is more expressive.

Currently, target tracking algorithms are mainly divided into traditional and deep learning algorithms, and the traditional methods mainly include the optical flow method <sup>[1]</sup>, Meanshift and Camshif <sup>[2]</sup>. Since these classical methods cannot handle and adapt to complex tracking variations, the model building will be greatly affected by lighting changes, motion blur, low resolution, and target rotation deformation, affecting tracking accuracy. After the emergence of deep learning tracking methods, the classical tracking methods are discarded, and deep learning algorithms surpass their robustness and accuracy.

Compared with traditional algorithms, deep learning algorithms provide faster tracking and higher accuracy.

Target tracking is divided into single-objective tracking and multi-objective tracking according to the number of targets to be tracked. The development of single-objective tracking algorithms has been relatively mature, but the tracking algorithm in practical scenarios is a multi-objective problem. Multi-target tracking is not simply multiple single-target tracking. It involves the continuous tracking of each target and the identification between different targets, the processing of self-obscuring and mutual obscuring, and the data correlation of tracking and detection results. For the multi-target tracking problem, Bewley et al <sup>[3]</sup> proposed the SORT algorithm, which uses Kalman filter to predict and update the targets, and uses the Hungarian algorithm to match the prediction frame and detection frame, which can achieve better multi-target tracking when there is no occlusion, but the tracking effect is poor in the case of occlusion; in order to improve the performance of the multi-target tracking algorithm in the case of occlusion, Wojke <sup>[4]</sup> et al. proposed DeepSORT tracking algorithm, which introduced cascade matching on the basis of SORT, and matched the data association between the prediction frame and detection frame of the target trajectory by the Hungarian algorithm in cascade matching, which improved the target occlusion problem of SORT algorithm, but in the case of low light intensity and occlusion, there still existed the target miss detection; Chen Yong et al <sup>[5]</sup> based on DeepSORT algorithm was improved by introducing acceleration parameter components and global trajectory generation mechanism, which further improved the accuracy of the algorithm; Chen Jiaqian et al <sup>[6]</sup> selected YOLOv3 for inspection, which improved the performance and robustnessof the algorithm; Chen Ziqiang et al.<sup>[7]</sup> selected YOLOv4 for inspection and proposed an improved Deep Sort target tracking based on the YOLOv4 algorithm, which reduces the phenomenon of missed detection. However, the detectors used in the above methods still have low accuracy and slow speed problems, and the tracking accuracy needs to be improved.

In order to meet the demand for real-time tracking, this paper proposes an improved DeepSORT algorithm based on the shortcomings of the above methods and uses YOLOv5 as a detector instead of the original FasterR-CNN algorithm to further improve the accuracy and operation speed of the algorithm and achieves better results after experimental verification.

The improved tracking model is designed to detect leakage in the case of weak illumination and occlusion. The YOLOv5 algorithm is used as a detector to extract feature information, and the network structure of the DeepSORT algorithm is improved.

### A. YOLOv5 target detection algorithm

Target detection is the basis of target tracking, through which the position of the pedestrian target in each frame of the video can be determined, and on this basis, the tracking of the pedestrian target can be realized. The accuracy of the target detection algorithm also affects the tracking accuracy.

Utralytics proposed YOLOv5 in 2020, which is improved based on YOLOv3, a lightweight detection network. The YOLOv5 network structure consists of four parts: the input side, Backbone, Neck and Prediction. As shown in Figure 1, the input side adopts Mosaic data enhancement, and it also obtains adaptive anchor frame calculation and adaptive image scaling for different data sets. Then, the backbone network adopts a focused structure, while two CSP structures are designed, one is applied in the Backbone backbone network, and the other is applied in Neck; the Neck part is FPN and PAN structure, which is different from YOLOv4, is different from YOLOv5 in that the Neck structure adopts the CSP2 structure borrowed from the CSPnet design, which enhances the ability of network feature fusion; the output side adopts the Bounding box loss function to achieve a faster and better convergence effect.

**Figure 1**

YOLOv5 network structure

### B. The DeepSORT target-tracking algorithm

DeepSORT tracking algorithm is a detection-based multi-target tracking algorithm proposed in 2017, which has an excellent performance in multi-target tracking and becomes the main detection-based multi-target tracking algorithm with better robustness in multi-target tracking. DeepSORT uses Faster R-CNN as a detector for target detection in each frame, but because DeepSORT uses Faster R-CNN as the detector to detect the target in each frame. However, since Faster R-CNN is a two-step target detection algorithm, it needs to extract the region where the target is located using region extraction technique first, and then detect the target for the specific region, which is a tedious step that leads to slow detection speed and thus the whole tracking process is slow. The specific improvements over the SORT algorithm include two aspects: combining motion information with the apparent features of the target as the matching criteria for the same target; using a combination of cascade matching and IOU matching to reduce tracking errors due to occlusion, and the overall process is shown in Figure 2.

**Figure 2**

DeepSORT algorithm overall process

#### 1) Kalman filtering

Kalman filtering is one of the core algorithms in the DeepSORT target tracking algorithm, and its role is mainly to accurately predict the tracking target position based on the target's motion state. Kalman filtering is an algorithm that establishes the equations of motion describing the target state based on the input data to achieve an optimal estimation of the overall system state.

The target model refers to the motion model that transfers the identity information of the target to the next frame. DeepSORT uses a linear isokinetic model that approximates the displacement of each object from the current frame to the next frame independently of other targets and camera movements and uses an 8-dimensional space vector to inscribe the target model as follows.

$$
\begin{equation*}x = {\left[ {u,v,r,h,{x^\prime },{y^\prime },{r^\prime },{h^\prime }} \right]^T}\tag{1}\end{equation*}
$$
 View Source

In the above equation: *u*, *v* denote the horizontal and vertical coordinates of the centre of the target bounding box, respectively; *r* is the aspect ratio of the target bounding box; *h* is the height of the target bounding box; (*u, v, r, h*) is known as the observed quantity; $(\dot u,\dot v,\dot r,\dot h)$ are the velocity information of the observed quantities, respectively.

A standard Kalman filter with isokinetic motion and linear observation model characteristics predict and update the target trajectory state. Prediction is that the system uses parameters such as the detection frame and motion speed of the last moment, which can predict the trajectory position of the target at the current moment through the time update equation. The update is to linearly weight the predicted and observed values to obtain parameters such as the error covariance matrix and enter the next iteration process to get a predicted value closer to the current system's actual state. The time update equation of the Kalman filter algorithm is:

$$
\begin{gather*} {X_{(k\mid k - 1)}} = A{X_{(k - 1\mid k - 1)}} + B{U_{(k)}}\tag{2} \\ {P_{(k\mid k - 1)}} = A{P_{(k - 1\mid k - 1)}}{A^T} + Q\tag{3}\end{gather*}
$$
 View Source

The state update equation of the Kalman filter algorithm is:

$$
\begin{gather*} K{g_{(k)}} = {P_{(k\mid k - 1)}}{H^T}/\left( {H{P_{(k\mid k - 1)}}{H^T} + R} \right)\tag{4} \\ {X_{(k\mid k - 1)}} = {X_{(k\mid k - 1)}} + K{g_{(k)}}\left( {{Z_{(k)}} - H{X_{(k\mid k - 1)}}} \right)\tag{5} \\ {P_{(k\mid k)}} = \left( {I - K{g_{(k)}}H} \right){P_{(k\mid k - 1)}}\tag{6}\end{gather*}
$$
 View Source

#### 2) Hungarian algorithm

The DeepSORT algorithm uses the Hungarian algorithm for matching the prediction target and detection target. The Hungarian algorithm, also called the KM algorithm, was proposed by Kuhn in 1955, and it is an algorithm used to solve the problem of optimal task assignment. The Kalman filter algorithm obtains the optimal estimate of the target state, and the predicted bounding box of the target is generated, and the detection target obtained by the detection algorithm is matched with the predicted bounding box of the target obtained by the Kalman filter algorithm. The matching process is divided into two steps.

- First, the cost matrix is calculated, i.e., the weighted values of IOU distance and appearance similarity distance between the predicted and detected targets are calculated.
- The Munkres version of the Hungarian algorithm is used to minimize the total matching cost and return a matching matrix containing the flags of the predicted and detected targets that have been matched and the flags of the predicted or detected targets that have failed to match.

The algorithm models the optimal matching problem by finding the optimal solution to an assignment bipartite graph. The principle is that if any of the equal subgraphs has a perfect match, then this perfect match is the maximum weight perfect match of the corresponding assignment bipartite graph, even if the matching cost is the smallest set of matching results so that the optimal match between the predicted and detected targets can be derived.

### A. Experimental Environment

The experimental environment used for pedestrian target tracking in this paper is Windows 10 operating system, Pytorch 1.8.1 framework, Python 3.7, 8G RAM, Intel(R)Core (TM) i5- 8250U processor, and NVIDIA GeForce GTX 2080 (8G) GPU.

### B. Experimental Data

To ensure the practicality and accuracy of this experiment, the tracking video used for the experiment in this paper is a real video taken by street surveillance, and the scene contains several moving pedestrian targets. Dark label labelling software is also used to label the video and obtain the real label of each target for accuracy verification.

### C. Experimental Results and Analysis

To test the performance of the improved DeepSORT algorithm, this paper conducts a comparison experiment, using YOLOv3 and YOLOv5 algorithms as detectors, respectively, to detect pedestrian targets in each frame of the video, calculate the target appearance feature values, and then perform Kalman filtering to predict the position of the target in the next frame, and use the Hungarian algorithm based on the target appearance features to match the target in the current frame with the predicted The cascade matching and IOU matching are performed based on the target appearance features. Finally, the accuracy evaluation is performed using the labelled video. The experimental results are shown in Table 1.

**TABLE I.** Performance Comparison of Two Algorithms

As shown in Table 1, the YOLOv5-DeepSORT tracking accuracy MOTA is 60.1% in 500 frames of video, which is 9.9% better than YOLOv3-DeepSORT. Moreover, the ratio of correctly identified detections to the average number of true and calculated detections IDF1 is 76%, which is 6.5% better; the total number of false positives FP is 156, the total number of missed targets total number of FN was 227, and the total number of ID switching IDs was 6, all of which were significantly reduced compared to YOLOv3-DeepSORT.

Figure 3 shows the comparison between the detection effect of different detectors and the effect of manually labelled pedestrians in the same frame. The rectangular boxes with different colours in the figure indicate the different pedestrian targets detected. Among them, Fig. 3 (a) shows the pedestrian detection results of the YOLOv3 detector, Fig. 3 (b) shows the pedestrian detection results of the YOLOv5 detector, and Fig. 3 (c) shows the manually labelled pedestrian targets. For most of the targets, both YOLOv3-DeepSORT and YOLOv5- DeepSORT can detect them. For the targets located at the edges, as indicated by the black arrows in Fig. 3, the YOLOv3- DeepSORT algorithm has a missed detection, while the YOLOv5-DeepSORT algorithm can successfully detect the pedestrians located at the edges. Therefore, YOLOv5 is better at detecting targets with fewer details at the edges.

**Figure 3**

Comparison of target detection experiments

**Figure 4**

YOLOv3-DeepSORT Pedestrian Tracking Experiment

**Figure 5**

YOLOv5-DeepSORT Pedestrian Tracking Experiment

Figures 4 and 5 show the pedestrian tracking results of YOLOv3-DeepSORT and YOLOv5-DeepSORT algorithms, respectively. Figures (a) to (c) indicate the pedestrian tracking states before, during and after occlusion, respectively. Different colour pedestrian target boxes in Figs. 4 and 5 are assigned with different IDs to represent different targets. As shown in Fig. 4, when the YOLOv3-DeepSORT algorithm runs, the pedestrian occlusion process produces the ID switch phenomenon, and the IDs of two targets are swapped. As shown in Figure 5, the YOLOv5-DeepSORT algorithm also runs with the same occlusion phenomenon, but the target still maintains its original ID after being occluded, which makes the pedestrian tracking more accurate and the target trajectory more reliable and robust.

To address low tracking accuracy and tracking errors in pedestrian target tracking., this paper develops a YOLOv5- based DeepSORT pedestrian target tracking algorithm and uses this algorithm to track pedestrians in real videos captured by street surveillance. To check the performance of YOLOv5- DeepSORT, this paper compares it with YOLOv3-DeepSORT. The results show that compared with YOLOv3-DeepSORT, YOLOv5-DeepSORT can identify edge targets well and overcome identity transformation and tracking errors due to occlusion, and the phenomenon of missed and false detection is significantly reduced. The detection effect and tracking effect are improved.

However, in the street surveillance video used for the experiments in this paper, the YOLOv5-DeepSORT algorithm still suffers from detection errors. YOLOv5 cannot recognize real people and dummies and will recognize and track stationary dummies as real people. Due to the high similarity between the dummy and the real in this scene, the difference of dummy is stationary. Therefore, an algorithm for still dummy determination can be introduced when using the YOLOv5- DeepSORT algorithm in future street surveillance videos.