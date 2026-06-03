---
title: "Multi-Object Tracking Algorithm Based on Improved ByteTrack"
source: "https://ieeexplore.ieee.org/abstract/document/10704485"
author:
published:
created: 2026-06-03
description: "With the rapid advancements of computer vision technology, the application of multi-object tracking in complex scenes is receiving increasing attention. For exa"
tags:
  - "clippings"
---
## Abstract:

With the rapid advancements of computer vision technology, the application of multi-object tracking in complex scenes is receiving increasing attention. For example, occl...

---

Multi-object tracking is a crucial research focus within the field of computer vision. At present, detection based tracking strategies have received widespread attention and practice in research and application. This strategy requires first determining the location box of the target, and then conducting subsequent tracking based on these location boxes\[1\]. Detection based multi object tracking can be seen as an extension of the object detection problem, or it can be described as a matching problem of target position boxes in the front and back frames\[2\]. Compared to single object tracking, multi-object tracking requires handling more complex target interaction problems, such as target occlusion, misalignment, etc. This article improves the ByteTrack algorithm based on yolov8 by adding a ReID re identification branch on the basis of ByteTrack. OSNet is used to train the re identification model, and it is added to the first association matching stage of ByteTrack. By integrating the informative cues from both the motion model and the appearance model, the similarity between the detected target and the tracking trajectory can be more accurately evaluated, leading to improved tracking performance. In order to enhance the matching effect, this article uses DIoU instead of its original IoU matching in the second matching of ByteTrack. Introducing Global Motion Compensation technology to address the issue of motion camera jitter. After experimental verification on the public dataset MOT17, the approach introduced in this work has shown superior performance and more effective implementation for multi-object tracking tasks when compared to the original ByteTrack method.

The ByteTrack algorithm \[3\] is a multi-object tracking approach that builds upon object detection techniques. This tracking framework employs Kalman filtering to forecast bounding box locations, and subsequently utilizes the Hungarian algorithm to associate detected targets with their corresponding trajectories. In order to improve the problem of directly discarding low score detection boxes that have the chance to match, ByteTrack uses the data association method BYTE to filter out the background from the low score detection boxes, find the real target, and maintain the continuity of the trajectory\[4\].

The specific process of the BYTE method is as follows: The first step is to obtain the confidence level of the detection box by the object detection algorithm, set it to high confidence level 0.45 and low confidence level 0.1, and divide the detection box into three parts. The detection box with a confidence level less than 0.1 is directly discarded, the box with a confidence level greater than 0.45 is a high confidence box, and the box with a confidence level less than 0.45 and greater than 0.1 is a low confidence box. The second step is to match the high confidence detection box with the previous tracking trajectory for the first time, and use Kalman filtering to obtain the predicted target trajectory. The third step is to use the tracking trajectory of low confidence boxes and high confidence boxes that were not matched in the first match for the second match. Step 4: For detection boxes that have not matched the tracking trajectory twice, match them with the new trajectory that appears in the middle of the target tracking pool, that is, match them with the inactive trajectory. Step 5: The tracking trajectory of the detection box that has not yet been matched will be retained for 30 frames. When it reappears, it will be matched. If it still does not reappear after 30 frames, it will be discarded.

### A. Fusion of Appearance Features and Sports Features

In the target matching stage, the algorithm will use the Hungarian algorithm to associate and pair the detected target in the current frame with the previously tracked target based on the target's motion model, appearance features, etc. The core objective of this process is to quantify the similarity between the detected bounding box and the existing tracking trajectory, in order to ascertain whether they correspond to the same target object. Next is target state estimation. This step will use methods such as Kalman filtering to predict the current position and motion characteristics of the target based on its historical motion state, thereby updating the target's state parameters such as position, velocity, etc. This helps to more accurately associate targets in subsequent frames and reduce tracking errors. However, ByteTrack only uses motion models for target matching. In order to better adapt to occlusion, ID switching, and other situations, an appearance model is further introduced on the basis of ByteTrack, and a re recognition branch is added in the target matching stage.

#### 1) Motion Model Metrics

In multi-object tracking, motion models play an important role. It models and predicts the motion patterns of targets between consecutive frames, helping to better analyze the attributes of targets such as posture, velocity, and position. Taking ByteTrack algorithm as an example, it uses Gaussian distribution to represent trajectory and detection results, and uses 8-dimensional state vectors $(u,v,\gamma,h,\dot{x},\dot{y},\gamma/,\dot{h})$ to describe the motion state of the target. In order to measure the similarity between feature vectors of the same target in different frames, ByteTrack chose Mahalanobis distance, focusing on the spatial distance reflected by the first 4 dimensions $(u,v,\gamma,h)$. Equation [(1)](#deqn1) is the calculation formula for Mahalanobis distance.

$$
\begin{equation*}
d_{M}(i,j)=(d_{j}-y_{i})^{\mathrm{T}}S_{i}^{-1}(d_{j}-y_{i})\tag{1}\end{equation*}
$$
 View Source

Among them, $d_{M}(i,j)$ is the Mahalanobis distance, $d_{j}$ is the orientation of the j-th detection box, $y_{\ddagger}$ is the orientation of the i-th trajectory, and $S_{l}$ is the covariance matrix between the tracking trajectory and the detection box. Equation [(2)](#deqn2) is a distance metric. When the Mahalanobis distance is less than the threshold $\xi_{1}=9.4877$, it indicates successful tracking. The corresponding detection box is updated with the corresponding trajectory through Kalman filtering.

$$
\begin{equation*}
b_{i,j}^{(1)}=1[d_{M}(i,j)\leq\xi_{1}]\tag{2}\end{equation*}
$$
 View Source

#### 2) Appearance Model Metrics

During the movement of objects, occlusion issues can lead to poor tracking performance based solely on spatial distance, resulting in label switching. Therefore, appearance features have also been introduced as effective tracking criteria.

Specifically, the algorithm will extract the appearance feature vectors of trajectories and detection boxes through the Re-ID branch and save them. The backbone network used here is OSNet\[5\]. OSnet network structure is shown in Figure 1.

**Figure 1.**

OSNet Network Structure.

The output of the network is a high-dimensional feature vector. The calculation of cosine similarity is shown in equation(3).

$$
\begin{equation*}\cos(\theta)=\frac{A\cdot B}{\Vert A\Vert \Vert B\Vert }=\frac{\sum_{i=1}^{n}A_{i}\times B_{i}}{\sqrt{\sum\limits_{i=1}^{n}(A_{i})^{2}}\times\sqrt{\sum\limits_{i=1}^{n}(B_{i})^{2}}}\tag{3}\end{equation*}
$$
 View Source

The cosine distance can be obtained from cosine similarity, which is used in appearance model measurement to measure similarity. The calculation formula is shown in equation [(4)](#deqn4):

$$
\begin{equation*}
d_{\cos}(i,j)=\min\{1-r_{j}^{T}r_{k}^{(i)}\vert r_{k}^{(i)}\in R_{i}\}\tag{4}\end{equation*}
$$
 View Source

Among them, $d_{\cos}(i,j)$ represents the cosine distance, $r_{j}$ represents the feature vector extracted by detection box $d_{j}, r_{k}^{(i)}$ represents the k-th feature vector in the i-th trajectory feature set $R_{i}$, and $r_{j}^{T}r_{k}^{(i)}$ represents the cosine similarity obtained by equation [(3)](#deqn3). Similarly, set the distance metric for equation [(5)](#deqn5) for cosine distance. Where $\xi_{2}=0.25$, if the cosine distance $d^{(2)}(i,j)$ is less than $\xi_{2}$, **it** is considered that the content of the detection box and the trajectory match, and the tracking is successful.

$$
\begin{equation*}
b_{i,j}^{(2)}=1[d_{\cos}(i,j)\leq\xi_{2}]\tag{5}\end{equation*}
$$
 View Source

In order to comprehensively consider the motion model and appearance model of the target object, it is also necessary to weight the Mahalanobis distance and cosine distance, and obtain the weighted cost function as shown in equation [(6)](#deqn6), $\lambda=0.98$.

$$
\begin{equation*}
C=\lambda d_{\cos}(i,j)+(1-\lambda)d_{M}(i,j)\tag{6}\end{equation*}
$$
 View Source

### B. Improving Cascade Matching Strategy

DIoU provides a richer measure of similarity by considering the distance between the overlapping area and the center point of two bounding boxes. Compared to IoU, DIoU can better capture the relative position information of targets, rather than being limited to overlapping areas. Even if the overlap area between the two targets is small, but the center point is close, DIoU can still give a high similarity score. This is helpful for partially occluded or predicted deviations, as it can provide a more reasonable similarity measure and improve tracking performance. By introducing the additional term of center point distance, DIoU is able to capture richer target spatial relationship features, and performs more robustly and accurately in complex scenes compared to IoU. This makes DIoU a more effective similarity calculation method in the field of multi-object tracking. $\rho(b,b^{gt})$ represents the Euclidean distance between two center points, while $c$ represents the diagonal distance of the smallest convex shapes.

$$
\begin{equation*}
DIoU=IoU-\frac{\rho^{2}(b,b^{gt})}{c^{2}}\tag{7}\end{equation*}
$$
 View Source

The second matching of ByteTrack is low score detection box matching, which is often caused by occlusion or rapid movement. In order to enhance the matching effect, DIoU is used instead of its original IoU matching in the second matching to improve the matching effect.

### C. Motion Camera Compensation

In scenarios involving dynamic camera setups, the positions of bounding boxes in the image plane can undergo significant changes, potentially leading to drift in the bounding boxes predicted by Kalman filtering-based multi-object tracking algorithms. To address this issue, this article applies Global Motion Compensation\[6\] technology to handle motion camera jitter. The implementation steps are as follows: first, extract the key points in the image as reference points for motion tracking. Then, sparse optical flow method is used to track these key points and suppress local outliers. Next, use the RANSAC algorithm to calculate the radiative transformation matrix A that describes the image transformation. Lastly, the target bounding box estimated through Kalman filtering is transformed from the coordinate system of the previous frame to that of the current frame.

$$
\begin{equation*}
A_{k}^{k-1}=[U_{2\times 2}\vert V_{2\times 1}]=\left[\begin{array}{lll}
a_{11} & a_{12} & a_{13} \\
a_{21} & a_{22} & a_{23}\end{array}\right]\tag{8}\end{equation*}
$$
 View Source

In the formula, $U\in R^{2\times 2}$ is the rotation scaling matrix, and $V\in R^{2\times 1}$ is the translation transformation matrix. When the predicted value $\hat{\mathrm{X}}_{\mathrm{t}\vert \mathrm{t}-1}=[\mathrm{x},\mathrm{y},\mathrm{w},\mathrm{h},\dot{\mathrm{x}},\dot{\mathrm{y}},\dot{\mathrm{w}},\dot{\mathrm{h}}]^{\mathrm{T}}$ is obtained through the Kalman filter prediction stage, it is necessary to perform a radial transformation on the center point coordinates and width height of the prior state estimation value. Therefore, the reconstructed rotation scaling matrix and translation change matrix are represented as:

$$
\begin{equation*}
U_{t\vert t-1}^{\prime}=\begin{bmatrix}
U & 0 & 0 & 0\\
0 & U & 0 & 0\\
0 & 0 & U & 0\\
0 & 0 & 0 & U\end{bmatrix},V_{t\vert t-1}^{\prime}=\begin{bmatrix}
V\\
0\\
0\\
0\\
0\\
0\\
0\end{bmatrix}\tag{9}\end{equation*}
$$
 View Source

Then use the obtained rotation scaling matrix $U_{t\vert t-1}\in \mathbb{R}^{8\times 8}$ and translation change matrix $V_{t\vert t-1}\in \mathbb{R}^{8\times 1}$ to correct the predicted values $\hat{\mathrm{X}}_{\mathrm{t}\vert \mathrm{t}-1}$ and covariance matrix $\mathrm{P}_{\mathrm{t}\vert \mathrm{t}-1}$ in the Kalman filter prediction stage.

$$
\begin{align*}
& \hat{X}_{\mathrm{t} \mid t-1}^{-}=\mathrm{U}_{\mathrm{t} t-1}^{\prime} \hat{X}_{\mathrm{t} t-1}^{-}+\mathrm{V}_{\mathrm{t} \mid t-1}^{\prime}\tag{10}\\
& \hat{P}_{t \mid t-1}=U_{t \mid t-1}^{\prime} P_{t \mid t-1}+V_{t \mid t-1}^{\prime}\tag{11}\end{align*}
$$
 View Source

Finally, the corrected predicted value $\hat{X}_{\mathrm{t}\vert \mathrm{t}-\mathrm{l}}$ and covariance matrix $\hat{\mathrm{P}}_{\mathrm{t}\vert \mathrm{t}-1}$ are included in the update stage of the Kalman filter. The obtained Kalman gain value, state estimation value at time during the Kalman filter update stage, and covariance matrix are represented as follows:

$$
\begin{gather*}\mathrm{K}_{\mathrm{t}}=\hat{\mathrm{P}}_{\mathrm{t} \mid t-1} \mathrm{H}_{\mathrm{t}}^{\mathrm{T}}\left(\mathrm{H}_{\mathrm{t}} \hat{\mathrm{P}}_{\mathrm{t} t-1} \mathrm{H}_{\mathrm{t}}^{\mathrm{T}}+\mathrm{R}_{\mathrm{t}}\right)^{-1}\tag{12}\\\hat{x}_{t \mid t}^{++}=\hat{X}_{t \mid t-1}^{-}+K_t\left(\hat{Z}_t-H_t \hat{X}_{t \mid t-1}^{-}\right)\tag{13}\\\mathrm{P}_{\mathrm{t} \mid \mathrm{t}}=\left(\mathrm{I}-\mathrm{K}_{\mathrm{t}} \mathrm{H}_{\mathrm{t}}\right) \mathrm{P}_{\mathrm{t} \mid \mathrm{t}-1}\tag{14}\end{gather*}
$$
 View Source

In the formula, $\mathrm{K}_{\mathrm{t}}$ is the corrected Kalman gain value, $\hat{x}_{t}^{++}$ is the corrected state estimation value, and $\mathrm{P}_{\mathrm{t}\vert \mathrm{t}}$ is the corrected covariance matrix. Figure 2 shows the effect of adding motion camera compensation.

**Figure 2.**

The effect of motion camera compensation.

### A. MOT17 Dataset

The MOT17 dataset has become a widely adopted benchmark for evaluating multi-object tracking algorithms. The video scene involves moving targets such as pedestrians and vehicles, and there are significant changes in lighting, occlusion, and congestion levels, which pose high requirements for the robustness of tracking algorithms.

### B. Evaluating Indicator

This article evaluates the algorithm using commonly used evaluation criteria in the field of multi-object tracking. As shown in Table 1.

**Table I.** Evaluation indicators

### C. Experimental Environment

The experimental work presented in this study was conducted within a Windows computing environment. The deep learning framework is Pytorch 2.1.2, and CUDA 11.8 and cuDNN 8.6 are used to accelerate training; The running hardware is as follows: Intel Core i7 12700KF, NVIDIA RTX 4070 Ti graphics card.

### D. Comparative Experimental Results

The performance comparison between our method and other multi-object tracking methods on the MOT17 dataset is shown in Table 2.

**Table II.** Comparative experiment

### E. Ablation Experiment

This article has made three improvements to ByteTrack, including adding Re-id to introduce appearance features, using DIoU matching, and introducing Motion camera compensation. The ablation experiment is shown in Table 3.

**Table III.** Results of ablation experiment

This paper improves the ByteTrack algorithm based on YOLOv8. In order to better adapt to issues such as occlusion and ID switching, a Re-Id re identification branch is added to ByteTrack. OSNet is used to train the re-identification model, and it is added to the first association matching stage of ByteTrack. By fusing the informative cues from the motion model and the appearance model, the method is able to more accurately evaluate the similarity between the detected target and the tracking trajectory, leading to enhanced tracking performance. The second matching of ByteTrack is a low score detection box matching, which is often caused by occlusion or rapid motion. In order to enhance the matching effect, this article uses DIoU instead of its original IoU matching in the second matching. However, in scenarios involving dynamic camera setups, the positions of bounding boxes within the image plane can undergo substantial changes, potentially leading to drift in the bounding box predictions generated by Kalman filtering-based multi-object tracking algorithms. Therefore, we introduce Global Motion Compensation technology to address the problem of motion camera jitter. Through experimental evaluation on the widely-used MOT17 public dataset, the method proposed in this work has demonstrated superior performance in comparison to the original ByteTrack algorithm.