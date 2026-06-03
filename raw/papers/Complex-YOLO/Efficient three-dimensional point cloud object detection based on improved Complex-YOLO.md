---
title: "Efficient three-dimensional point cloud object detection based on improved Complex-YOLO"
source: "https://www.frontiersin.org/journals/neurorobotics/articles/10.3389/fnbot.2023.1092564/full"
author:
  - "[[Yongxin Shao]]"
  - "[[Zhetao Sun]]"
  - "[[Aihong Tan]]"
  - "[[Tianhong Yan]]"
published: 2023-02-16
created: 2026-06-03
description: "Lidar-based 3D object detection and classification is a critical task for autonomous driving. However, inferencing from exceedingly sparse 3D data in real-ti..."
tags:
  - "clippings"
---
- College of Mechanical and Electrical Engineering, China Jiliang University, Hangzhou, China

Article metrics

[View details](#metrics)

19

Citations

11,5k

Views

2,1k

Downloads

## Abstract

Lidar-based 3D object detection and classification is a critical task for autonomous driving. However, inferencing from exceedingly sparse 3D data in real-time is a formidable challenge. Complex-YOLO solves the problem of point cloud disorder and sparsity by projecting it onto the bird’s-eye view and realizes real-time 3D object detection based on LiDAR. However, Complex-YOLO has no object height detection, a shallow network depth, and poor small-size object detection accuracy. To address these issues, this paper has made the following improvements: (1) adds a multi-scale feature fusion network to improve the algorithm’s capability to detect small-size objects; (2) uses a more advanced RepVGG as the backbone network to improve network depth and overall detection performance; and (3) adds an effective height detector to the network to improve the height detection. Through experiments, we found that our algorithm’s accuracy achieved good performance on the KITTI dataset, while the detection speed and memory usage were very superior, 48FPS on RTX3070Ti and 20FPS on GTX1060, with a memory usage of 841Mib.

## 1\. Introduction

Deep learning-related technologies are increasingly integrated into people’s daily life, and object detection algorithms (;,; ), as a crucial component of the autonomous driving perception layer, can create a solid foundation for behavioral judgments during autonomous driving. Although object detection algorithms based on 2D images (;;;;;;; ) have had a lot of success at this stage, single-view images cannot completely reflect the position pose, and motion orientation of objects in 3D space due to the lack of depth information in 2D images. Consequently, in the field of autonomous driving, the focus of object detection research has increasingly switched from 2D image detection to 3D image detection and point cloud detection. To compensate for the lack of depth information in 2D images, researchers primarily use multi-view (; ) and depth cameras () when studying image-based 3D object detection algorithms. However, because both depth cameras and ordinary 2D cameras are affected by light, image-based object detection algorithms frequently perform poorly in complex environmental conditions. Compared with images, point cloud data collected by LIDAR not only provides accurate geometric features but also has better robustness to light. However, point cloud-based 3D object detection algorithms face significant challenges. For instance, (1) Sparsity: Compared with the dense pixel arrangement in images, point cloud data are sparse and unevenly distributed; (2) Disorder: The nature of the object is unaffected by the ordering of the point cloud data; and (3) Rotation invariance: The coordinates of all points in a point cloud collection change when rotated, yet they still represent the same object (). To resolve these issues proposed Complex-YOLO to complete 3D object detection by projecting 3D point cloud data onto the 2D bird’s eye view, transforming the disordered point cloud into the 2D pseudo-image after meshing it and using the image-based 2D object detection algorithm. Complex-YOLO uses YOLOV2 () for feature extraction, classification, and regression, and extends the E-RPN (Euler Region Proposal Network) structure for object orientation angle regression. However, because Complex-YOLO lacks a neck network, it must complete object detection at a lower resolution, resulting in poor performance for small objects. It performs poorly in 3D bounding box regression because it does not complete the detection of object height and instead provides a fixed height value for different classes of objects. In this paper, the proposed Efficient Complex-YOLO (from now on, we will call our proposal as Efficient Complex-YOLO) improves Complex-YOLO comprehensively, chooses a more advanced network structure, makes up for the original lack of Complex-YOLO for height detection, and substantially improves the detection accuracy while ensuring real-time detection, the main work is as follows:

- **(1)**
	A more advanced 2D object detection technique with a more efficient feature extraction network \[RepVGG-A2 ()\] is used.
- **(2)**
	Building an improved FPN \[Feature Pyramid Network ()\] structure to accomplish the detection task at different resolutions, which improves the detection accuracy for small objects.
- **(3)**
	A more efficient head detection network is designed to complete the detection of object height, cancel the E-RPN structure, and complete the detection of object orientation angle directly in the Cartesian coordinate system to achieve true 3D object detection.

The rest of this paper is as follows.

- **(1)**
	Section 2 presents the work related to the point cloud-based 3D object detection algorithm.
- **(2)**
	Section 3 presents the algorithmic structure of Efficient Complex-YOLO.
- **(3)**
	Section 4 will experimentally verify the effectiveness of the improvements made and compares them with other point cloud-based 3D object detection algorithms.
- **(4)**
	Section 5 concludes the whole paper.

## 2\. Related work

The point cloud-based 3D object detection algorithms can be divided into three categories based on the pre-processing techniques used on point cloud data. (1) 3D object detection with Point-based methods; (2) 3D object detection with Voxel-based methods; and (3) 3D object detection with Grid-based methods.

### 2.1. 3D object detection with point-based methods

The PointNet proposed by processes point cloud data directly. PointNet solves the point cloud rotation invariance problem using a spatial transformation network, solves the point cloud disorder problem using the Max-pooling method, and builds a unified system framework for object classification and semantic segmentation, and many subsequent algorithms will use this network to extract features. However, PointNet can only characterize each point and cannot effectively integrate the point cloud information of a region of the point cloud. The subsequently proposed PointNet++ () effectively solves this problem by drawing on the idea of hierarchical feature extraction; it consists of three segments: sampling, grouping, and feature extraction, using the farthest point sampling method to extract key points from the original point cloud for sampling, then using PointNet to extract features from the grouped point set to select key point features for subsequent semantic segmentation or 3D object detection. The PointRCNN algorithm proposed by is the first 3D object detection method that is entirely based on point clouds; it is a two-stage object detection algorithm that uses PointNet as the feature extraction network. The first stage performs binary classification of point clouds, classifying each point into the foreground or background, and generating a bounding box for each foreground point; the second stage optimizes the bounding box generated in the first stage, and the features of each bounding box are obtained through point cloud pooling and other operations; finally, the bounding box is optimized and scored with confidence by combining the features obtained in the first stage, to obtain the final bounding box. However, unlike object detection on 2D images, point cloud data do not have a regular grid, making it difficult to apply the idea of 2D detection to complete the regression of centroids and bounding boxes for objects whose centroids are outside the point cloud. The VoteNet algorithm proposed by uses the Hoff voting mechanism to continuously generate some virtual points close to the object’s centroid, and then completes the classification and regression tasks using a clustering algorithm and some classifiers and regressors. Many point-based 3D object detection algorithms currently use hand-crafted grouping schemes to group point clouds and extract local features using feature extraction networks like PointNet. However, the hand-crafted grouping scheme leads to inaccurate point assignment and degrades the performance of 3D object detection. The Group-Free 3D algorithm proposed by uses Transformer () to adaptively determine which points are effective for the current object detection; it uses Transformer’s attention mechanism to compute the object features of all points in the point cloud and automatically learns the contribution of each point during network training. An improved attention stacking scheme is then used to fuse object features from different stages to produce more accurate object detection results.

### 2.2. 3D object detection with voxel-based methods

A voxel is similar to a pixel in an image. The smallest unit in an RGB image is the pixel, while the smallest unit in a three-dimensional spatial representation is the voxel. If the whole space is divided into small cubes into three dimensions, each small cube is a voxel. The voxelization process is to use voxels to express the appearance and shape of an object at the corresponding position in the three-dimensional space, and by doing so, the disordered point cloud can be sorted and the internal information of the object can be well described. VoxelNet proposed by divides the 3D point cloud into a certain number of voxels, and then, after random sampling of points and normalization, for each non-empty voxel use VFE (Voxel Feature Encoding) layers for local feature extraction to obtain Voxel-wise feature. Then 3D Convolution Middle Layers are used to further abstract the features (increase the perceptual field and learn the geometric space representation), and RPN (Region Proposal Network) is used for object classification and regression. However, 3D convolution tends to consume a lot of computing power. The subsequent SECOND proposed by makes a series of improvements based on VoxelNet and proposes an improved sparse convolution method that increases the detection speed, reduces the memory usage, and improves the orientation angle detection performance by introducing an angle loss method. subsequently proposed PointPillars, which retains the idea of VoxelNet and Second, but unlike the former, divides the point cloud into individual pillars for feature extraction and replaces the original 3D convolution with 2D convolution. proposed a new 3D detection network called PV-RCNN, which improves the detection accuracy by combining two different data forms, point clouds, and voxels, and extracting features from them. The algorithm combines the efficient feature extraction capability of 3D convolution with the advantage of point clouds’ variable perceptual field.

### 2.3. 3D object detection with grid-based methods

These algorithms usually project point cloud data into the 2D grid, then use 2D convolution for feature extraction. proposed MV3D to achieve 3D object detection by multi-view fusion, firstly computing candidate regions on the bird’s-eye view of the point cloud, then integrating the candidate regions with point cloud bird’s-eye view features, point cloud front view features, and image features into one dimension, respectively by Roi (Region of Interest) pooling, and finally using convolution neural networks to complete classification and regression. Subsequently, proposed AVOD based on MV3D by eliminating the input of the point cloud main view and adding intensity features in the point cloud bird’s-eye view, followed by modifying the feature extraction network to improve small object detection accuracy.

## 3\. Materials and methods

### 3.1. Detection process

The overall detection algorithm is shown in above, which has the following four steps:

FIGURE 1

- **Step 1**
	Data pre-processing. Rasterized projection of point cloud data in the bird’s-eye view to generate the 2D pseudo-image (from now on, we will call it as BEV-map);
- **Step 2**
	BEV-map detection. The Bev-map is fed into the improved Complex YOLO to complete the extraction of feature maps and the detection of the 2D bounding box in the bird’s eye view and the detection of object orientation angle;
- **Step 3**
	Height detection. The feature maps obtained in step 2 are fed into the height detector to output the object’s height;
- **Step 4**
	Finally. The information obtained from step 2 is fused with step 3 to complete the 3D object detection task;

### 3.2. Data preprocessing

In this paper, we generate BEV-map using the same data pre-processing method as Complex-YOLO by rasterizing and projecting the single-frame point cloud data covering the 50 m × 50 m area in front onto the bird’s-eye view map, and the grid size of BEV-map is *m* = 608 and *n* = 608, corresponding to the size of each grid is about *g* = 0.8. Then the maximum height, maximum intensity, and density of the point cloud data in the bird’s-eye view are encoded and filled into the RGB triple channel. Considering that the height of LIDAR is about 1.73 m during data acquisition, while most targets are between 0 and 4 m high, to avoid the influence of obscurants such as trees, we selected the point cloud data in the range of *z* ∈ \[2.73, 1.27\]. All point clouds were defined *P* <sub>Ω</sub> as:

**(1)** PΩ={P=\[x,y,z\]T|x∈\[0,50m\],y∈\[25,25m\],z∈\[2.73,1.27m\]} $PΩ={P=[x,y,z]T|x∈[0,50m],y∈[25,25m],z∈[2.73,1.27m]}$

(1)

The three channels of the transformed image are encoded by the following equations (2), (3), and (4) for point cloud height information, point cloud intensity information, and point cloud density information, respectively.

**(2)** zg=max(PΩi→j⋅\[0,0,1\]T) $zg=max(PΩi→j⋅[0,0,1]T)$

(2)

**(3)** zb=max(I(PΩi→j)) $zb=max(I(PΩi→j))$

(3)

**(4)** zr=min(1.0,log(N+1)/log(64)) $zr=min(1.0,log(N+1)/log(64))$

(4)

In above equations (2), (3), and (4) above, **I** (*P* <sub>Ω</sub>) represents Point cloud intensity; *N* represents the number of point clouds in each grid; **z <sub>g</sub>** represents the maximum height; **z <sub>b</sub>** represents the maximum intensity; **z <sub>r</sub>** the normalized density within the grid; **z <sub>b</sub>**, **z** <sub><b>g</b>,</sub> and **z <sub>r</sub>** the final output RGB-map pixel values for each channel.

### 3.3. Complex-YOLO and its improved parts

shows the network structure of Complex-YOLO. The yellow block represents the backbone network, the green block represents the head network, and the red dashed box is the part to be improved in this paper.

FIGURE 2

In this paper, a comprehensive improvement is made to Complex-YOLO, and the Efficient Complex-YOLO network structure is shown in, as follows.

FIGURE 3

- **(1)**
	Improvement of the backbone network part: using RepVGG as the backbone network to extract feature maps.
- **(2)**
	Adding the neck network structure: adding FPN+CBAM (Convolutional Block Attention Module) () as the neck network to achieve special multi-scale fusion.
- **(3)**
	Improvements in the head network: selection of the Anchor-free approach, in addition to removing the E-RPN structure in the head network and adding an efficient height detector to regress height.

#### 3.3.1. Backbone network improvements

To meet the demand for real-time detection while maintaining detection accuracy, we used RepVGG-A2 in the feature extraction network section, which performs better in terms of detection accuracy, detection speed, and memory usage, and the specific structure is shown in. RepVGG is a feature extraction network proposed by. Compared with other multi-branch structures \[e.g., ResNet (), DenseNet ()\], RepVGG converts the multi-branch structure into a single-branch structure consisting entirely of 3 × 3 convolutions in the inference stage, which can improve the detection speed of the algorithm and reduce the memory usage while maintaining a high detection accuracy. shows the RepVGG training phase structure. In the training phase, two branching structures are used: one is a residual structure similar to ResNet with *y* = *x* + *F* (*x*), and the other is a branching structure with *y* = *G* (*x*) + *F* (*x*), to which a 1 × 1 convolution is added for feature extraction, and the final performance is *y* = *x* + *F* (*x*) + *G* (*x*). This branching structure provides the network with numerous gradient update pathways, making the trained model more adaptable. Training such a network is similar to the concept of model integration, in which many models are fused to improve model robustness and reduce gradient disappearance, which is more favorable to loss function convergence. shows the network structure of RepVGG during the inference phase. In the inference phase, RepVGG reconstructs the model weights of the identity branch and 1 × 1 branch into 3 × 3 convolution, and then fuses the reconstructed parameters with the original 3 × 3 convolution weights of the main branch, resulting in a network consisting solely of 3 × 3 convolution. This method can avoid the need to save the calculation results of all branches before completing the feature fusion operation of the residual structure during the actual program operation and effectively reduce memory usage while ensuring accuracy and greatly improving detection speed. In addition, most algorithmic frameworks nowadays accelerate 3 × 3 convolution, and with GPU acceleration, the FLOPS (computational density, number of floating points operations per second) of 3 × 3 convolution can reach four times that of 1 × 1 convolution and 5 × 5 convolution, which greatly accelerates RepVGG’s detection speed ().

FIGURE 4

The model used in this paper is the RepVGG-A2 model. The specific parameters are shown in.

TABLE 1

| Stage | Output size | Number of blocks |
| --- | --- | --- |
| 1 | 304 × 304 × 64 | 1 |
| 2 | 152 × 152 × 69 | 1 |
| 3 | 76 × 76 × 192 | 2 |
| 4 | 38 × 38 × 384 | 14 |
| 5 | 19 × 19 × 1,408 | 1 |

RepVGG-A2 specific parameters.

#### 3.3.2. Neck network improvements

There is no neck network in the original Complex-YOLO. However, when the convolution neural network is used for feature extraction, the size of the extracted feature maps decreases steadily as the network level increases, and the perceptual field corresponding to the regions in the original image will gradually get larger, resulting in the loss of feature information for small objects and making small object detection difficult. Based on this, this paper adds a neck network to the original Complex-YOLO and uses the structure of FPN+CBAM to improve the detection of small-sized objects.

We add the CBAM module before constructing the FPN structure. CBAM is a convolutional block attention module proposed by that combines the channel attention mechanism and the spatial attention mechanism, and the specific structure is shown in. The channel attention mechanism generates a channel attention map by exploiting the inter-channel relationships of features. As each channel of a feature map is considered as a feature detector, channel attention focuses on “what” is meaningful given an input image. The spatial attention mechanism uses the spatial relationships of features to build a spatial attention map. It focuses on “where” is an informative part, which is complementary to the channel attention. Adding the CBAM module before building the FPN structure enables the model to learn which features are more beneficial to the detection task, eliminates redundant features, and facilitates the integration of different levels of semantic information in the FPN structure ().

FIGURE 5

The upper part of shows the channel attention mechanism, and its implementation can be divided into two parts that will execute global average pooling and global max pooling for each of the individual feature layers coming in. Following that, the results of the average pooling and max pooling are processed using a shared fully connected layer, and the two processed results are summed, followed by a Sigmoid operation to produce the weights (between 0 and 1) of each channel of the input feature layer. After obtaining this weight, the spatial attention map is generated by multiplying this weight by the original input feature layer. The bottom half of shows the spatial attention mechanism, which takes the maximum value and average value on each channel for each feature point, for the input to be passed into the feature layer. Following that, a stacking of these two results is performed, and the number of channels is adjusted using a convolution with one channel at a time, and then the same Sigmoid operation is conducted to acquire the weight (between 0 and 1) of each feature point of the input feature layer, and finally, this weight is multiplied by the original input feature layer to generate the channel attention map.

FIGURE 6

The input features are defined as *F* ∈ R <sup><i>C</i> × <i>W</i> × <i>H</i></sup>, *M* <sub><i>C</i></sub> ∈ R <sup><i>C</i> × 1 × 1</sup>,and *M* <sub><i>S</i></sub> ∈ *R* <sup>1 × <i>W</i> × <i>H</i></sup>, which are CBAM-derived one-dimensional channel attention weights and two-dimensional spatial attention weights. Then *M <sub>C</sub>* and *M <sub>S</sub>* are described by equations (5) and (6).

**(5)** MC=σ(MLP(AvgPool(F))+MLP(MaxPool(F)))=σ(W1(W0(FCavg))+W1(W0(FCmax))) $MC=σ(MLP(AvgPool(F))+MLP(MaxPool(F)))=σ(W1(W0(FavgC))+W1(W0(FmaxC)))$

(5)

**(6)** MS=σ(f7×7(\[AvgPool(F);;MaxPool(F)\]))=σ(f7×7(\[FSavg;;FSmax\])) $MS=σ(f7×7([AvgPool(F);;MaxPool(F)]))=σ(f7×7([FavgS;;FmaxS]))$

(6)

Where σ denotes the Sigmoid activation function; *f* <sup>7 × 7</sup> denotes the 7 × 7 convolution operation; *W* <sub>0</sub> ∈ R <sup><i>C</i> / <i>r</i> × <i>C</i></sup>, *W* <sub>1</sub> ∈ R <sup><i>C</i> × <i>C</i> / <i>r</i></sup> denote the weight parameters of the MLP (Multi-Layer Perception), where *r* is the reduction ratio; FCavg $FavgC$, FCmax $FmaxC$ denote the average pooled and maximum pooled features; FSavg $FavgS$, FSmax $FmaxS$ denote the average pooled and max pooled features across channels.

Equations (7) and (8) define the computational process of CBAM by defining *F* as the output of the channel attention mechanism and *F* as the output of the spatial attention mechanism. ⊗ denotes element-wise multiplication.

**(7)** F=MC(F)⊗F $F=MC(F)⊗F$

(7)

**(8)** F=MC(F)⊗F $F=MC(F)⊗F$

(8)

As shown in, we select the feature maps output from Stage2, Stage3, Stage4, and Stage5 of RepVGG-A2 (corresponding resolutions 152 × 152, 76 × 76, 38 × 38, and 19 × 19, in that order) as input, and then build the FPN structure after passing through the CBAM module. The outputs through CBAM are defined as F2, F3, F4, and F5, and we will do 2× up-sampling of the lower resolution feature map in the adjacent feature maps, then stitch it with the adjacent feature maps of the higher resolution by channel dimension (for example, the resolution of F5 is 19 × 19, and the resolution of F4 is 38 × 38, so we do 2× up-sampling of F5, and then stitch it with F4 by channel dimension), and finally, adjust the number of channels by 1 × 1 convolution. This completes the feature fusion of adjacent feature maps. The fused results are defined as P2, P3, P4, and P5. In the course of subsequent experiments, we found that when predicted at a resolution of 152 × 152, each grid corresponds to a size of about 0.32 m × 0.32 m in the original 50 m × 50 m space, which is not conducive to pedestrian detection in dense scenes, so we subjected the obtained P2, P3, and P4 to another 2× up-sampling operation and completed a feature of adjacent resolution fusion, and the final set of feature maps participating in the prediction is defined as (P2, P3, P4, corresponding resolutions 304 × 304, 152 × 152, and 76 × 76). In general, the lower-resolution feature map contains more semantic information, and each grid has a wider perceptual range, whereas the higher-resolution feature map contains rich spatial information, and each grid has a narrower perceptual range. As a result, by constructing an FPN structure, we can fuse different feature information of different resolutions on the one hand, and predict objects of different sizes in different perceptual ranges on the other. We use P4 to predict some larger objects and P3 and P2 to predict some smaller objects during the prediction stage.

FIGURE 7

#### 3.3.3. Head network improvements

Complex-YOLO adds the E-RPN network based on YOLOv2 to achieve the regression of object orientation angles and in the Efficient Complex-YOLO, we choose to regress the angle of the object directly. In addition, Complex-YOLO does not detect the height of the object but instead gives a fixed height value for different classes of objects (Car:1.5 m; Cyclist:1.4 m; Pedestrian:1.8 m), which is still essentially 2D object detection and is not conducive to the return of the 3D bounding box. Efficient Complex-YOLO adds the height detector based on Complex-YOLO to directly predict the height of objects, which greatly improves the detection accuracy of the 3D Bounding box. shows the specific head prediction network structure.

FIGURE 8

For the prediction of 2D bounding box information, we choose the Anchor-free based prediction method. The anchor proposed by Faster R-CNN () tries to predetermine the shape of objects for different shapes and sizes, it can handle different shapes of objects well and also reduces the difficulty of training. However, the foreground points (objects we want to detect, such as cars, pedestrians, etc.) in a point cloud data frame are few, resulting in a large number of anchor boxes matching the background points and being judged as negative samples, and only a few anchor boxes matching the foreground points and being judged as positive samples, resulting in serious positive and negative sample mismatch, which is detrimental to model training.

In the prediction stage, we predict five results for each output of the FPN structure (both P2, P3, and P4 mentioned above), corresponding to the predicted class confidence of the object, the 2D bounding box coordinate offset, the orientation angle of the object, the z-axis coordinate offset under the 3D bounding box, and the length, width, and height of the 3D bounding box. Each prediction result is completed by two convolutions. For the first time, 3 × 3 convolution is used to adjust the number of channels. For example, for category probability prediction of objects, the feature map size is integrated from W × H × C to W × H × 3 by a 3 × 3 convolution operation (we selected three categories of Car, Pedestrian and Cyclist for testing. Therefore, the channel number of the integrated feature map is 3). Finally, we use a 1 × 1 convolution operation to complete the prediction.

In Complex-YOLO, the E-RPN network decomposes the regression of the object orientation angle into the regression of the parameters *t* <sub><i>im</i></sub> and *t* <sub><i>re</i></sub>, which correspond to the phase of a complex number. *artan* (*t* <sub><i>im</i></sub>, *t* <sub><i>re</i></sub>) is used to calculate the orientation angle (). We attempt to predict the orientation angle of the object directly, and subsequent experiments show that direct regression of the angle can improve the algorithm’s detection accuracy even further.

As previously stated, the BEV-map constructed during the data pre-processing stage contains the maximum height information of the point cloud data in each grid, and we chose point cloud data in the range of *z* ∈ \[2.73, 1.27\], which can filter out the majority of the occlusion cases. As a result, we add the height detector using the same procedure as the category prediction described above (3 × 3 convolution is used to integrate the feature map size from W × H × C to W × H × 1, and then 1 × 1 convolution is used to complete the final height detection).

Let’s define *b <sub>x</sub>, b <sub>y</sub>, b <sub>z</sub>* as the center point coordinates of the 3D bounding box, *b <sub>w</sub>, b <sub>h</sub>, b <sub>l</sub>* as the length, width, and height of the 3D bounding box, *b* <sub>ϕ</sub> as the orientation angle, *t <sub>x</sub>, t <sub>y</sub>, t <sub>z</sub>, t <sub>w</sub>, t <sub>h</sub>, t <sub>l</sub>, t* <sub>ϕ</sub> as the relative offset obtained from the network prediction, *c <sub>x</sub>, c <sub>y</sub>* as the grid center point coordinates in the bird’s eye view, and σ(*x*) as the Sigmoid function. Equations (9) to (15) indicate the regression modes of 3D bounding boxes of objects.

**(9)** bx=σ(tx)+cx $bx=σ(tx)+cx$

(9)

**(10)** by=σ(ty)+cy $by=σ(ty)+cy$

(10)

**(11)** bz=tz $bz=tz$

(11)

**(12)** bw=tw $bw=tw$

(12)

**(13)** bh=th $bh=th$

(13)

**(14)** bl=tl $bl=tl$

(14)

**(15)** bϕ=tϕ $bϕ=tϕ$

(15)

In the training phase, inspired by CenterNet (), we choose to generate a heatmap to calculate the object category loss and Focal Loss () is chosen to calculate the category loss of objects to avoid the problem of positive and negative sample mismatch. The specific calculation formula is shown in equation (16).

**(16)** Lcls=−1NC∑c=1W∑w=1H∑h=1⎧⎪⎨⎪⎩(1−^pclscij)αlog(^pclscij)ifpclscij=1(1−pclscij)βlog(1−^pclscij)otherwise $Lcls=-1N∑c=1C∑w=1W∑h=1H{(1-p^cijcls)αlog⁡(p^cijcls)ifpcijcls=1(1-pcijcls)βlog⁡(1-p^cijcls)otherwise$

(16)

*C* denotes the number of channels of the heatmap (in this paper, we selected three categories of Car, Pedestrian and Cyclist for testing, so *C* = 3 here); *H* and *W* denotes the length and width of heatmap; pclscij $pcijcls$ can be obtained from the length and width of the ground truth as well as the Gaussian kernel calculated; ^pclscij $p^cijcls$ is the predicted value of the network; we set α to 2 and β to 4.

We used L1 Loss to calculate the loss values of the x, y coordinate offset and the loss values of the orientation angle of the object. The specific calculation formula is shown in equations (17) (18).

**(17)** Lyaw=1NW∑w=1H∑h=11objij|^pyawij−pyawij| $Lyaw=1N∑w=1W∑h=1H1ijobj|p^ijyaw-pijyaw|$

(17)

**(18)** Loff=1NW∑w=1H∑h=11objij|^poffij−poffij| $Loff=1N∑w=1W∑h=1H1ijobj|p^ijoff-pijoff|$

(18)

pyawij $pijyaw$, ^pyawij $p^ijyaw$ denote the ground truth and predicted values of orientation angle; poffij $pijoff$, ^poffij $p^ijoff$ denote the ground truth and predicted values of x, y offsets, respectively; 1objij $1ijobj$ denotes if an object appears in position i, j.

We use Balanced L1 Loss () to calculate the loss values of the z-coordinate and length, width and height, the specific formula as shown in equations (19) (20).

**(19)** Ldim=1NW∑w=1H∑h=11objij⎧⎨⎩αb(b|ˆpdimij−pdimij|)ln(b|ˆpdimij−pdimij|)if|ˆpdimij−pdimij|=1γ|x|+Cotherwise $Ldim=1N∑w=1W∑h=1H1ijobj{αb(b|p^ijdim−pijdim|)ln(b|p^ijdim−pijdim|)if|p^ijdim−pijdim|=1γ|x|+Cotherwise$

(19)

**(20)** Lz=1NW∑w=1H∑h=11objij⎧⎨⎩αb(b|ˆpzij−pzij|)ln(b|ˆpzij−pzij|)if|ˆpzij−pzij|=1γ|x|+Cotherwise $Lz=1N∑w=1W∑h=1H1ijobj{αb(b|p^ijz−pijz|)ln(b|p^ijz−pijz|)if|p^ijz−pijz|=1γ|x|+Cotherwise$

(20)

pdimij $pijdim$, ^pdimij $p^ijdim$ denote the ground truth and predicted values of the object’s length, width and height, respectively; pzij $pijz$, ^pzij $p^ijz$ denote the ground truth and predicted values of the object’s z-coordinate; α and γ are 0.5 and 1.5, respectively; α, γ and *b* satisfy the relation *b* = *e* <sup>γ/α</sup> − 1;

The final loss function is calculated as shown in equation (21).

**(21)** Ltotal=λclsLcls+λyawLyaw+λoffLoff+λzLz+λdimLdim $Ltotal=λclsLcls+λyawLyaw+λoffLoff+λzLz+λdimLdim$

(21)

λ <sub><i>cls</i></sub>, λ <sub><i>yaw</i></sub>, λ <sub><i>off</i></sub>, λ <sub><i>z</i></sub> and λ <sub><i>dim</i></sub> denote the weight of each part of the loss function, respectively.

## 4\. Result

### 4.1. Experiment environment

The experimental platform in this paper is Ubuntu 18.04; the GPU used is Nvidia RTX3070Ti with 8G memory; the deep learning framework is PyTorch 1.10; the KITTI () dataset is selected for model training, 6,000 data are randomly selected as the training set, and the rest are used as the validation set.

### 4.2. Ablation experiment design and analysis

The experiments in this paper used the same KITTI dataset used by Complex-YOLO to evaluate detection accuracy for three categories of objects: Car, Pedestrian, and Cyclist. AP (Average Precision) and mAP (Mean Average Precision) are used as accuracy evaluation metrics, while FPS (Frames Per Second) is used as speed evaluation metrics. The IoU (Intersection over Union) threshold is 0.7 for Car and 0.5 for Pedestrian and Cyclist (for example, if the IOU threshold is 0.7, when the IOU between the detected bounding box and the ground truth bounding box is greater than 0.7, the detected bounding box is considered to be a positive sample), and detection results are classified as Easy, Mod, and Hard depending on the object being obscured., [^1] show the specific experimental results, where shows the accuracy of 2D object detection in the bird’s eye view perspective and shows the accuracy of 3D object detection.

TABLE 2

<table><thead><tr><td align="left" colspan="2">Improved part</td><td align="center">Method A</td><td align="center">Method B</td><td align="center">Method C</td><td align="center">Method D</td><td align="center">Method E</td><td align="center">Method F</td><td align="center">Method G</td></tr></thead><tbody><tr><td align="left" colspan="2">E-RPN</td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td></td><td></td><td></td></tr><tr><td align="left" colspan="2">RepVGG-A2</td><td></td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td></tr><tr><td align="left" colspan="2">FPN</td><td></td><td></td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td></tr><tr><td align="left" colspan="2">Height detector</td><td></td><td></td><td></td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td></tr><tr><td align="left" colspan="2">CBAM</td><td></td><td></td><td></td><td></td><td></td><td align="center">√</td><td align="center">√</td></tr><tr><td align="left" colspan="2">Higher resolution</td><td></td><td></td><td></td><td></td><td></td><td></td><td align="center">√</td></tr><tr><td align="left">Car AP</td><td align="center">Easy</td><td align="center">85.89</td><td align="center">89.46</td><td align="center">84.79</td><td align="center">92.73</td><td align="center">87.96</td><td align="center"><b>97.34</b></td><td align="center">96.52</td></tr><tr><td></td><td align="center">Mod</td><td align="center">77.40</td><td align="center">89.36</td><td align="center">87.18</td><td align="center">87.20</td><td align="center">88.24</td><td align="center">89.74</td><td align="center"><b>89.80</b></td></tr><tr><td align="left">(IoU = 0.7)</td><td align="center">Hard</td><td align="center">77.33</td><td align="center">80.80</td><td align="center">87.81</td><td align="center">87.83</td><td align="center">88.61</td><td align="center">89.90</td><td align="center"><b>89.92</b></td></tr><tr><td align="left">Pedestrian AP</td><td align="center">Easy</td><td align="center">46.08</td><td align="center">36.96</td><td align="center">66.91</td><td align="center">66.76</td><td align="center">67.94</td><td align="center">71.88</td><td align="center"><b>73.26</b></td></tr><tr><td></td><td align="center">Mod</td><td align="center">45.90</td><td align="center">32.88</td><td align="center">69.27</td><td align="center">69.27</td><td align="center">70.51</td><td align="center">73.44</td><td align="center"><b>73.56</b></td></tr><tr><td align="left">(IoU = 0.5)</td><td align="center">Hard</td><td align="center">44.20</td><td align="center">33.84</td><td align="center">70.63</td><td align="center">70.63</td><td align="center">71.35</td><td align="center">74.42</td><td align="center"><b>75.42</b></td></tr><tr><td align="left">Cyclist AP</td><td align="center">Easy</td><td align="center">72.37</td><td align="center">59.12</td><td align="center">76.26</td><td align="center">85.15</td><td align="center"><b>85.92</b></td><td align="center">85.87</td><td align="center">85.61</td></tr><tr><td></td><td align="center">Mod</td><td align="center">63.36</td><td align="center">68.00</td><td align="center">76.71</td><td align="center">84.70</td><td align="center">85.64</td><td align="center">86.10</td><td align="center"><b>86.52</b></td></tr><tr><td align="left">(IoU = 0.5)</td><td align="center">Hard</td><td align="center">60.27</td><td align="center">60.13</td><td align="center">77.00</td><td align="center">84.94</td><td align="center">78.02</td><td align="center">86.21</td><td align="center"><b>86.21</b></td></tr><tr><td></td><td align="center">Easy</td><td align="center">68.11</td><td align="center">61.84</td><td align="center">75.99</td><td align="center">81.54</td><td align="center">80.61</td><td align="center">85.03</td><td align="center"><b>85.13</b></td></tr><tr><td align="left">mAP</td><td align="center">Mod</td><td align="center">62.22</td><td align="center">63.41</td><td align="center">77.72</td><td align="center">80.39</td><td align="center">81.46</td><td align="center">83.09</td><td align="center"><b>83.29</b></td></tr><tr><td></td><td align="center">Hard</td><td align="center">60.60</td><td align="center">58.26</td><td align="center">78.48</td><td align="center">81.13</td><td align="center">79.33</td><td align="center">83.51</td><td align="center"><b>83.85</b></td></tr><tr><td align="center" colspan="2">FPS</td><td align="center">108</td><td align="center">102</td><td align="center">78</td><td align="center">72</td><td align="center">71</td><td align="center">68</td><td align="center">48</td></tr></tbody></table>

Experimental results of the ablation experiments were used to verify the contribution of the improvements in this paper.

2D object detection results in the bird’s eye view perspective on the KITTI dataset. The evaluation metric is AP (Average Precision). The Intersection over Union (IoU) threshold is 0.7 for Car and 0.5 for Pedestrian/Cyclist. The bolded data are the highest scores for this item.

TABLE 3

<table><thead><tr><td align="left" colspan="2">Improved part</td><td align="center">Method A</td><td align="center">Method B</td><td align="center">Method C</td><td align="center">Method D</td><td align="center">Method E</td><td align="center">Method F</td><td align="center">Method G</td></tr></thead><tbody><tr><td align="left" colspan="2">E-RPN</td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td></td><td></td><td></td></tr><tr><td align="left" colspan="2">RepVGG-A2</td><td></td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td></tr><tr><td align="left" colspan="2">FPN</td><td></td><td></td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td></tr><tr><td align="left" colspan="2">Height detector</td><td></td><td></td><td></td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td></tr><tr><td align="left" colspan="2">CBAM</td><td></td><td></td><td></td><td></td><td></td><td align="center">√</td><td align="center">√</td></tr><tr><td align="left" colspan="2">Higher resolution</td><td></td><td></td><td></td><td></td><td></td><td></td><td align="center">√</td></tr><tr><td align="left">Car AP</td><td align="center">Easy</td><td align="center">67.72</td><td align="center">43.88</td><td align="center">49.68</td><td align="center">92.50</td><td align="center">87.37</td><td align="center"><b>97.01</b></td><td align="center">96.56</td></tr><tr><td></td><td align="center">Mod</td><td align="center">64.00</td><td align="center">41.19</td><td align="center">45.06</td><td align="center">86.89</td><td align="center">87.44</td><td align="center">88.98</td><td align="center"><b>89.10</b></td></tr><tr><td align="left">(IoU = 0.7)</td><td align="center">Hard</td><td align="center">63.01</td><td align="center">37.68</td><td align="center">43.24</td><td align="center">87.51</td><td align="center">87.90</td><td align="center">89.26</td><td align="center"><b>89.32</b></td></tr><tr><td align="left">Pedestrian AP</td><td align="center">Easy</td><td align="center">41.79</td><td align="center">27.06</td><td align="center">53.78</td><td align="center">65.67</td><td align="center">67.31</td><td align="center">70.53</td><td align="center"><b>71.33</b></td></tr><tr><td></td><td align="center">Mod</td><td align="center">39.70</td><td align="center">28.62</td><td align="center">55.99</td><td align="center">68.11</td><td align="center">69.92</td><td align="center">72.53</td><td align="center"><b>73.26</b></td></tr><tr><td align="left">(IoU = 0.5)</td><td align="center">Hard</td><td align="center">35.92</td><td align="center">29.70</td><td align="center">57.58</td><td align="center">69.52</td><td align="center">70.88</td><td align="center">73.42</td><td align="center"><b>74.11</b></td></tr><tr><td align="left">Cyclist AP</td><td align="center">Easy</td><td align="center">68.17</td><td align="center">62.92</td><td align="center">52.58</td><td align="center">85.15</td><td align="center">84.77</td><td align="center"><b>85.81</b></td><td align="center">84.76</td></tr><tr><td></td><td align="center">Mod</td><td align="center">58.32</td><td align="center">69.83</td><td align="center">48.34</td><td align="center">84.70</td><td align="center">76.79</td><td align="center"><b>85.92</b></td><td align="center">85.60</td></tr><tr><td align="left">(IoU = 0.5)</td><td align="center">Hard</td><td align="center">54.30</td><td align="center">69.60</td><td align="center">49.06</td><td align="center">84.94</td><td align="center">76.99</td><td align="center">86.13</td><td align="center"><b>86.72</b></td></tr><tr><td></td><td align="center">Easy</td><td align="center">59.22</td><td align="center">44.62</td><td align="center">52.01</td><td align="center">81.11</td><td align="center">79.82</td><td align="center"><b>84.45</b></td><td align="center">84.21</td></tr><tr><td align="left">mAP</td><td align="center">Mod</td><td align="center">54.00</td><td align="center">46.55</td><td align="center">49.80</td><td align="center">79.90</td><td align="center">78.05</td><td align="center">82.48</td><td align="center"><b>82.65</b></td></tr><tr><td></td><td align="center">Hard</td><td align="center">51.07</td><td align="center">45.66</td><td align="center">49.96</td><td align="center">80.66</td><td align="center">78.59</td><td align="center">82.94</td><td align="center"><b>83.38</b></td></tr><tr><td align="center" colspan="2">FPS</td><td align="center">108</td><td align="center">102</td><td align="center">78</td><td align="center">72</td><td align="center">71</td><td align="center">68</td><td align="center">48</td></tr></tbody></table>

Experimental results of the ablation experiments were used to verify the contribution of the improvements in this paper.

3D object detection results on the KITTI dataset. The evaluation metric is AP (Average Precision). The Intersection over Union (IoU) threshold is 0.7 for Car and 0.5 for Pedestrian/Cyclist. The bolded data are the highest scores for this item.

Method A is the original Complex-YOLO. It can be seen that the running speed is excellent and can reach 108 FPS on RTX3070Ti, but the detection accuracy is low.

Method B is a modified Complex-YOLO based RepVGG-A2 feature extraction network. Method B has higher detection accuracy for large objects like Car but lower detection accuracy for Pedestrian in 2D object detection in the bird’s-eye view perspective. The analysis found that the output feature map of RepVGG-A2 has a down-sampling multiplicity of 32, i.e., if the BEV-map with an input resolution of 608 × 608, then the output feature map size is 19 × 19. Such a smaller resolution means a larger perceptual field, which is not conducive to the prediction of small-sized objects. Method B has low accuracy in 3D object detection. In terms of detection speed, RepVGG still maintains a good detection speed, which can reach 102 FPS, because it consists of all 3 × 3 convolution in the inference stage and has no multi-branch structure.

Method C builds the FPN structure based on method B. Compared with method B, method C builds the FPN structure to accomplish the object detection of different sizes (38 × 38, 76 × 76, 152 × 152) at three different resolutions, greatly improving the 2D object detection accuracy of Pedestrian and Cyclist in the bird’s-eye view perspective, as well as the 3D object detection accuracy of Car and Pedestrian. In terms of detection speed, due to the construction of the FPN structure, which makes the network more complex, the detection speed is reduced to 78FPS, but the performance requirement of real-time detection is met.

Method D is based on method C, which adds the height detector. The addition of height detection in method D improves both 2D object detection accuracy in the bird’s-eye view perspective and 3D object detection accuracy.

Method E is based on method D, which removes the E-RPN structure and completes the detection of the orientation angle directly in the Cartesian coordinate system. The detection accuracy is also improved compared with method D.

Method F is an improved FPN structure built based on method E, the CBAM attention mechanism part is added to further improve the attention of the network for small objects.

Method G adds one more feature fusion operation of 2× up-sampling with adjacent resolution to the FPN structure based on method F, and complete the 3D object detection task at a higher resolution.

To express the improvements in this paper more intuitively, the improved algorithm and the detection results of the original Complex-YOLO are converted into images for presentation. shows the detection results of the two algorithms. From, it can be seen that the algorithm in this paper has a great improvement in 3D detection results compared to the original algorithm. Although the detection speed is slower than the original Complex-YOLO algorithm, it meets the performance requirements of real-time detection perfectly.

FIGURE 9

### 4.3. Comparative experiments and analysis with other methods

To further validate the advantages of the algorithms in this paper, we use the KITTI dataset as test data and compare it to other 3D object detection algorithms based on point cloud involved in this paper in three aspects: detection accuracy, detection speed, and memory usage. In addition, this paper also compares some subsequently improved algorithms of Complex-YOLO (Complex-YOLOv3 and Complex-YOLOv4; these algorithms all improve on Complex-YOLO according to the evolution of the subsequent YOLO series algorithms, but all maintain the original YOLO series network + E-RPN structure; Complex YOLOv3 with YOLOv3 + E-RPN and Complex YOLOv4 with YOLOv4 + E-RPN). We used the same data set division (6,000 data for training and 1,481 data for validation) for testing on GTX1060. shows the 2D object detection results in the bird’s eye view. shows the 3D object detection results. shows the detection speed and memory usage for the same test conditions.

TABLE 4

<table><thead><tr><td align="left">Methods</td><td align="center" colspan="3">Car AP (IoU = 0.7)</td><td align="center" colspan="3">Pedestrian AP (IoU = 0.5)</td><td align="center" colspan="3">Cyclist AP (IoU = 0.5)</td></tr><tr><td align="left"></td><td align="center">Easy</td><td align="center">Mod</td><td align="center">Hard</td><td align="center">Easy</td><td align="center">Mod</td><td align="center">Hard</td><td align="center">Easy</td><td align="center">Mod</td><td align="center">Hard</td></tr></thead><tbody><tr><td align="left">PointRCNN ()</td><td align="center">90.64</td><td align="center">89.54</td><td align="center">88.68</td><td align="center">86.94</td><td align="center"><b>78.69</b></td><td align="center">74.70</td><td align="center"><b>95.43</b></td><td align="center"><b>87.14</b></td><td align="center"><b>86.24</b></td></tr><tr><td align="left">PV-RCNN ()</td><td align="center">99.14</td><td align="center">89.91</td><td align="center">89.57</td><td align="center">73.85</td><td align="center">71.56</td><td align="center">69.60</td><td align="center">91.70</td><td align="center">82.88</td><td align="center">81.73</td></tr><tr><td align="left">SECOND ()</td><td align="center">98.01</td><td align="center">88.98</td><td align="center">87.72</td><td align="center">67.49</td><td align="center">65.59</td><td align="center">62.75</td><td align="center">90.89</td><td align="center">84.26</td><td align="center">83.52</td></tr><tr><td align="left">PartA <sup>2</sup> ()</td><td align="center">98.81</td><td align="center">89.52</td><td align="center">89.09</td><td align="center">76.71</td><td align="center">70.14</td><td align="center">68.65</td><td align="center">94.03</td><td align="center">83.94</td><td align="center">83.33</td></tr><tr><td align="left">PointPillars ()</td><td align="center">97.33</td><td align="center"><b>90.04</b></td><td align="center">89.40</td><td align="center">66.74</td><td align="center">61.28</td><td align="center">58.83</td><td align="center">91.12</td><td align="center">83.82</td><td align="center">81.72</td></tr><tr><td align="left">VoxelRCNN ()</td><td align="center"><b>99.21</b></td><td align="center">89.63</td><td align="center">89.16</td><td align="center"><b>75.01</b></td><td align="center">70.01</td><td align="center">67.49</td><td align="center">94.15</td><td align="center">83.80</td><td align="center">82.40</td></tr><tr><td align="left">Complex-YOLOv3</td><td align="center">71.73</td><td align="center">74.87</td><td align="center">78.73</td><td align="center">54.74</td><td align="center">59.44</td><td align="center">62.39</td><td align="center">64.06</td><td align="center">74.23</td><td align="center">67.71</td></tr><tr><td align="left">Complex-YOLOv4</td><td align="center">71.26</td><td align="center">74.35</td><td align="center">78.26</td><td align="center">49.98</td><td align="center">54.34</td><td align="center">57.54</td><td align="center">52.11</td><td align="center">60.72</td><td align="center">56.01</td></tr><tr><td align="left">Our method</td><td align="center">96.52</td><td align="center">89.80</td><td align="center"><b>89.92</b></td><td align="center">73.26</td><td align="center">73.56</td><td align="center"><b>75.42</b></td><td align="center">85.61</td><td align="center">86.52</td><td align="center">86.21</td></tr></tbody></table>

Experimental results of comparison experiments with other algorithms.

2D object detection results in the bird’s eye view perspective on the KITTI dataset. The evaluation metric is AP (Average Precision). The Intersection over Union (IoU) threshold is 0.7 for Car and 0.5 for Pedestrian/Cyclist. The bolded data are the highest scores for this item.

TABLE 5

<table><thead><tr><td align="left">Methods</td><td align="center" colspan="3">Car AP (IoU = 0.7)</td><td align="center" colspan="3">Pedestrian AP (IoU = 0.5)</td><td align="center" colspan="3">Cyclist AP (IoU = 0.5)</td></tr><tr><td align="left"></td><td align="center">Easy</td><td align="center">Mod</td><td align="center">Hard</td><td align="center">Easy</td><td align="center">Mod</td><td align="center">Hard</td><td align="center">Easy</td><td align="center">Mod</td><td align="center">Hard</td></tr></thead><tbody><tr><td align="left">PointRCNN</td><td align="center">90.31</td><td align="center">86.05</td><td align="center">79.86</td><td align="center"><b>86.26</b></td><td align="center"><b>77.97</b></td><td align="center">69.88</td><td align="center"><b>95.44</b></td><td align="center"><b>87.15</b></td><td align="center">86.11</td></tr><tr><td align="left">PV-RCNN</td><td align="center"><b>98.69</b></td><td align="center"><b>89.23</b></td><td align="center">88.52</td><td align="center">72.10</td><td align="center">68.78</td><td align="center">65.21</td><td align="center">89.65</td><td align="center">81.39</td><td align="center">80.49</td></tr><tr><td align="left">SECOND</td><td align="center">98.01</td><td align="center">88.97</td><td align="center">87.72</td><td align="center">65.80</td><td align="center">63.49</td><td align="center">60.05</td><td align="center">90.89</td><td align="center">84.26</td><td align="center">83.52</td></tr><tr><td align="left">PartA <sup>2</sup></td><td align="center">90.51</td><td align="center">88.64</td><td align="center">87.44</td><td align="center">72.46</td><td align="center">69.40</td><td align="center">67.01</td><td align="center">93.56</td><td align="center">83.57</td><td align="center">83.16</td></tr><tr><td align="left">PointPillars</td><td align="center">90.04</td><td align="center">88.19</td><td align="center">84.63</td><td align="center">63.09</td><td align="center">58.07</td><td align="center">55.77</td><td align="center">89.91</td><td align="center">82.81</td><td align="center">80.54</td></tr><tr><td align="left">VoxelRCNN</td><td align="center">90.22</td><td align="center">88.41</td><td align="center">87.06</td><td align="center">74.95</td><td align="center">67.11</td><td align="center">65.40</td><td align="center">94.02</td><td align="center">83.68</td><td align="center">82.30</td></tr><tr><td align="left">Complex-YOLOv3</td><td align="center">61.16</td><td align="center">55.69</td><td align="center">61.13</td><td align="center">51.63</td><td align="center">56.39</td><td align="center">59.51</td><td align="center">62.94</td><td align="center">64.55</td><td align="center">65.60</td></tr><tr><td align="left">Complex-YOLOv4</td><td align="center">62.39</td><td align="center">56.61</td><td align="center">61.81</td><td align="center">47.94</td><td align="center">51.82</td><td align="center">55.17</td><td align="center">51.78</td><td align="center">52.90</td><td align="center">54.35</td></tr><tr><td align="left">Our method</td><td align="center">96.56</td><td align="center">89.10</td><td align="center"><b>89.32</b></td><td align="center">71.33</td><td align="center">73.26</td><td align="center"><b>74.11</b></td><td align="center">84.76</td><td align="center">85.60</td><td align="center"><b>86.72</b></td></tr></tbody></table>

Experimental results of comparison experiments with other algorithms.

3D object detection results on the KITTI dataset. Comparison of the results of 3D object detection on the KITTI dataset with other algorithms. The evaluation metric is AP (Average Precision). The Intersection over Union (IoU) threshold is 0.7 for Car and 0.5 for Pedestrian/Cyclist. The bolded data are the highest scores for this item.

TABLE 6

| Methods | FPS | Memory |
| --- | --- | --- |
| PointRCNN | 3 | 1093Mib |
| PV-RCNN | 3.8 | 1439Mib |
| SECOND | 11 | 889Mib |
| PartA <sup>2</sup> | 3 | 1021Mib |
| PointPillars | 18 | 931Mib |
| VoxelRCNN | 8 | 991Mib |
| Complex-YOLOv3 | 15 | 1195Mib |
| Complex-YOLOv4 | 12 | 1401Mib |
| Our method | 20 | 841Mib |

Experimental results of comparison experiments with other algorithms.

The detection speed and memory usage of each algorithm was compared on the GTX1060.

, [^2] show that Efficient Complex-YOLO has a high detection accuracy for tiny objects and objects with a high detection difficulty. shows that Efficient Complex-YOLO performs well in terms of detection speed and memory usage in the same test environment.

## 5\. Conclusion

In light of Complex-YOLO’s weaknesses in detection accuracy, this paper proposes a more efficient 3D object detection algorithm based on a series of improvements to Complex-YOLO. The RepVGG is used to increase the network’s depth and improve model fitting capabilities. To address the issue of poor detection accuracy of small objects in the original algorithm, improved FPN structure greatly improves the network’s accuracy for small objects. The Anchor-free + height detector method is used in the head network, and the original E-RPN structure is removed, which improves the network’s detection accuracy and compensates for the lack of height detection in the original algorithm. Although these improvements reduce the detection speed to some extent, the Efficient Complex-YOLO algorithm proposed in this paper can fully meet the performance requirements of real-time detection, reaching 48FPS on RTX3070Ti (it can guarantee a detection speed of 20FPS even on GTX1060). It has been experimentally proved that the method proposed in this paper has more advantages than Complex-YOLO. Compared with the subsequently improved algorithms of Complex-YOLO (Complex-YOLOv3 and Complex-YOLOv4), they all continue the original framework of Complex YOLO and do not achieve object height detection, so the proposed algorithms in this paper are more advantageous. The mAP of 2D object detection in bird’s eye view perspective is 85.13, 83.29, and 83.85 under Easy, Mod, and Hard conditions, respectively, while the mAP of 3D object detection is 84.21, 82.65, and 83.38 under Easy, Mod, and Hard conditions, respectively. Although Efficient Complex-YOLO falls short of top 3D object detection algorithms (e.g., PV-RCNN), it has a smaller memory usage and faster detection speed. In some applications with limited cost, it is still a method worth considering.

Point-based or Voxel-based 3D object detection algorithms, after the data pre-processing stage, often send a large amount of data into the network, which also tends to take a lot of arithmetic power consuming 3D convolution or fully connected layer for feature extraction, which causes the problem of slow detection speed and high memory occupation. The data processing method used in the Grid-based 3D object detection algorithm can significantly reduce the number of parameters input into the network and adopt the same structural form as the 2D object detection algorithm in the network, which can significantly improve the detection speed and reduce the memory occupation, and this paper also achieves good results in terms of detection speed and detection accuracy by improving Complex-YOLO. However, the data processing method like the one used in this paper will cause data loss in the encoding stage of the BEV-map, especially in the height information of the Z-axis direction, which largely limits the accuracy improvement of the algorithm. We suggest that the reader improve the data preprocessing or try other data processing methods to encode the point cloud to avoid the loss of information as much as possible.

## Statements

### Data availability statement

The original contributions presented in this study are included in the article/supplementary material, further inquiries can be directed to the corresponding author.

### Author contributions

AT and TY: overall work direction. YS: algorithm design and essay writing. YS and ZS: code writing and software implementation. All authors have read and agreed to the published version of the manuscript.

### Funding

This research was funded by the Provincial Natural Science Foundation of Zhejiang, grant number Y21F010057.

### Acknowledgments

The authors thank the referees whose comments helped to improve the manuscript.

### Conflict of interest

The authors declare that the research was conducted in the absence of any commercial or financial relationships that could be construed as a potential conflict of interest.

### Publisher’s note

All claims expressed in this article are solely those of the authors and do not necessarily represent those of their affiliated organizations, or those of the publisher, the editors and the reviewers. Any product that may be evaluated in this article, or claim that may be made by its manufacturer, is not guaranteed or endorsed by the publisher.

## References

- 1
	BaiJ.DaiJ.WangZ.YangS. (2022). A detection method of the rescue targets in the marine casualty based on improved YOLOv5s.**Front. Neurorobot.**16:1053124. 10.3389/fnbot.2022.1053124
	- [Pubmed Abstract](https://pubmed.ncbi.nlm.nih.gov/36467570)
	- [CrossRef](https://doi.org/10.3389/fnbot.2022.1053124)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=J..%2BBai&author=J..%2BDai&author=Z..%2BWang&author=S..%2BYang&publication_year=2022&title=A%2Bdetection%2Bmethod%2Bof%2Bthe%2Brescue%2Btargets%2Bin%2Bthe%2Bmarine%2Bcasualty%2Bbased%2Bon%2Bimproved%2BYOLOv5s.&journal=Front.+Neurorobot.&volume=16)
- 2
	BochkovskiyA.WangC.-Y.LiaoH.-Y. M. (2020). Yolov4: Optimal speed and accuracy of object detection.**arXiv** \[Preprint\].
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=A..%2BBochkovskiy&author=C.-Y..%2BWang&author=H.-Y.%2BM..%2BLiao&publication_year=2020&title=Yolov4%3A%2BOptimal%2Bspeed%2Band%2Baccuracy%2Bof%2Bobject%2Bdetection.&journal=arXiv)
- 3
	ChenX.MaH.WanJ.LiB.XiaT. (2017). “Multi-view 3d object detection network for autonomous driving,” in **Proceedings of the IEEE conference on computer vision and pattern recognition**, Beijing, 1907–1915. 10.1109/CVPR.2017.691
	- [CrossRef](https://doi.org/10.1109/CVPR.2017.691)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=X..%2BChen&author=H..%2BMa&author=J..%2BWan&author=B..%2BLi&author=T..%2BXia&publication_year=2017&title=Multi-view%2B3d%2Bobject%2Bdetection%2Bnetwork%2Bfor%2Bautonomous%2Bdriving&journal=Proceedings+of+the+IEEE+conference+on+computer+vision+and+pattern+recognition&pages=1907-1915)
- 4
	CheonJ.BaekS.PaikS.-B. (2022). Invariance of object detection in untrained deep neural networks.**Front. Comput. Neurosci.**16:1030707. 10.3389/fncom.2022.1030707
	- [Pubmed Abstract](https://pubmed.ncbi.nlm.nih.gov/36405785)
	- [CrossRef](https://doi.org/10.3389/fncom.2022.1030707)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=J..%2BCheon&author=S..%2BBaek&author=S.-B..%2BPaik&publication_year=2022&title=Invariance%2Bof%2Bobject%2Bdetection%2Bin%2Buntrained%2Bdeep%2Bneural%2Bnetworks.&journal=Front.+Comput.+Neurosci.&volume=16)
- 5
	DengJ.ShiS.LiP.ZhouW.ZhangY.LiH. (2021). Voxel r-cnn: Towards high performance voxel-based 3d object detection.**Proc. AAAI Conf. Artif. Intell.**351201–1209. 10.1609/aaai.v35i2.16207
	- [CrossRef](https://doi.org/10.1609/aaai.v35i2.16207)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=J..%2BDeng&author=S..%2BShi&author=P..%2BLi&author=W..%2BZhou&author=Y..%2BZhang&author=H..%2BLi&publication_year=2021&title=Voxel%2Br-cnn%3A%2BTowards%2Bhigh%2Bperformance%2Bvoxel-based%2B3d%2Bobject%2Bdetection.&journal=Proc.+AAAI+Conf.+Artif.+Intell.&volume=35&pages=1201-1209)
- 6
	DingX.ZhangX.MaN.HanJ.DingG.SunJ. (2021). “Repvgg: Making vgg-style convnets great again,” in **Proceedings of the IEEE/CVF conference on computer vision and pattern recognition**, Nashville, 13733–13742. 10.1109/CVPR46437.2021.01352
	- [CrossRef](https://doi.org/10.1109/CVPR46437.2021.01352)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=X..%2BDing&author=X..%2BZhang&author=N..%2BMa&author=J..%2BHan&author=G..%2BDing&author=J..%2BSun&publication_year=2021&title=Repvgg%3A%2BMaking%2Bvgg-style%2Bconvnets%2Bgreat%2Bagain&journal=Proceedings+of+the+IEEE%2FCVF+conference+on+computer+vision+and+pattern+recognition&pages=13733-13742)
- 7
	DuanK.BaiS.XieL.QiH.HuangQ.TianQ. (2019). “Centernet: Keypoint triplets for object detection,” in **Proceedings of the IEEE/CVF international conference on computer vision**, Seoul, 6569–6578. 10.1109/ICCV.2019.00667
	- [CrossRef](https://doi.org/10.1109/ICCV.2019.00667)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=K..%2BDuan&author=S..%2BBai&author=L..%2BXie&author=H..%2BQi&author=Q..%2BHuang&author=Q..%2BTian&publication_year=2019&title=Centernet%3A%2BKeypoint%2Btriplets%2Bfor%2Bobject%2Bdetection&journal=Proceedings+of+the+IEEE%2FCVF+international+conference+on+computer+vision&pages=6569-6578)
- 8
	GeigerA.LenzP.UrtasunR. (2012). “Are we ready for autonomous driving? the kitti vision benchmark suite,” in **Proceedings of the 2012 IEEE conference on computer vision and pattern recognition**, (Piscataway, NJ: IEEE), 3354–3361. 10.1109/CVPR.2012.6248074
	- [CrossRef](https://doi.org/10.1109/CVPR.2012.6248074)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=A..%2BGeiger&author=P..%2BLenz&author=R..%2BUrtasun&publication_year=2012&title=Are%2Bwe%2Bready%2Bfor%2Bautonomous%2Bdriving%3F%2Bthe%2Bkitti%2Bvision%2Bbenchmark%2Bsuite&journal=Proceedings+of+the+2012+IEEE+conference+on+computer+vision+and+pattern+recognition&pages=3354-3361)
- 9
	GromadaK.Siemia̧tkowskaB.SteczW.PłochockiK.WoźniakK. (2022). Real-time object detection and classification by UAV equipped with SAR.**Sensors (Basel)** 22:2068. 10.3390/s22052068
	- [Pubmed Abstract](https://pubmed.ncbi.nlm.nih.gov/35271213)
	- [CrossRef](https://doi.org/10.3390/s22052068)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=K..%2BGromada&author=B..%2BSiemia%CC%A7tkowska&author=W..%2BStecz&author=K..%2BP%C5%82ochocki&author=K..%2BWo%C5%BAniak&publication_year=2022&title=Real-time%2Bobject%2Bdetection%2Band%2Bclassification%2Bby%2BUAV%2Bequipped%2Bwith%2BSAR.&journal=Sensors+%28Basel%29&volume=22)
- 10
	HeK.ZhangX.RenS.SunJ. (2016). “Deep residual learning for image recognition,” in **Proceedings of the IEEE conference on computer vision and pattern recognition**, Las Vegas, 770–778. 10.1109/CVPR.2016.90
	- [CrossRef](https://doi.org/10.1109/CVPR.2016.90)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=K..%2BHe&author=X..%2BZhang&author=S..%2BRen&author=J..%2BSun&publication_year=2016&title=Deep%2Bresidual%2Blearning%2Bfor%2Bimage%2Brecognition&journal=Proceedings+of+the+IEEE+conference+on+computer+vision+and+pattern+recognition&pages=770-778)
- 11
	HuangG.LiuZ.van Der MaatenL.WeinbergerK. Q. (2017). “Densely connected convolutional networks,” in **Proceedings of the IEEE conference on computer vision and pattern recognition**, Honolulu, HI, 4700–4708. 10.1109/CVPR.2017.243
	- [CrossRef](https://doi.org/10.1109/CVPR.2017.243)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=G..%2BHuang&author=Z..%2BLiu&author=L..%2Bvan%2BDer%2BMaaten&author=K.%2BQ..%2BWeinberger&publication_year=2017&title=Densely%2Bconnected%2Bconvolutional%2Bnetworks&journal=Proceedings+of+the+IEEE+conference+on+computer+vision+and+pattern+recognition&pages=4700-4708)
- 12
	KhatabE.OnsyA.AbouelfaragA. (2022). Evaluation of 3D vulnerable objects’ detection using a multi-sensors system for autonomous vehicles.**Sensors (Basel)** 22:1663. 10.3390/s22041663
	- [Pubmed Abstract](https://pubmed.ncbi.nlm.nih.gov/35214569)
	- [CrossRef](https://doi.org/10.3390/s22041663)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=E..%2BKhatab&author=A..%2BOnsy&author=A..%2BAbouelfarag&publication_year=2022&title=Evaluation%2Bof%2B3D%2Bvulnerable%2Bobjects%E2%80%99%2Bdetection%2Busing%2Ba%2Bmulti-sensors%2Bsystem%2Bfor%2Bautonomous%2Bvehicles.&journal=Sensors+%28Basel%29&volume=22)
- 13
	KuJ.MozifianM.LeeJ.HarakehA.WaslanderS. L. (2018). “Joint 3d proposal generation and object detection from view aggregation,” in **Proceedings of the 2018 IEEE/RSJ international conference on intelligent robots and systems (IROS)**, (Piscataway, NJ: IEEE), 1–8. 10.1109/IROS.2018.8594049
	- [CrossRef](https://doi.org/10.1109/IROS.2018.8594049)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=J..%2BKu&author=M..%2BMozifian&author=J..%2BLee&author=A..%2BHarakeh&author=S.%2BL..%2BWaslander&publication_year=2018&title=Joint%2B3d%2Bproposal%2Bgeneration%2Band%2Bobject%2Bdetection%2Bfrom%2Bview%2Baggregation&journal=Proceedings+of+the+2018+IEEE%2FRSJ+international+conference+on+intelligent+robots+and+systems+%28IROS%29&pages=1-8)
- 14
	LangA. H.VoraS.CaesarH.ZhouL.YangJ.BeijbomO. (2019). “Pointpillars: Fast encoders for object detection from point clouds,” in **Proceedings of the IEEE/CVF conference on computer vision and pattern recognition**, Long Beach, CA, 12697–12705. 10.1109/CVPR.2019.01298
	- [CrossRef](https://doi.org/10.1109/CVPR.2019.01298)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=A.%2BH..%2BLang&author=S..%2BVora&author=H..%2BCaesar&author=L..%2BZhou&author=J..%2BYang&author=O..%2BBeijbom&publication_year=2019&title=Pointpillars%3A%2BFast%2Bencoders%2Bfor%2Bobject%2Bdetection%2Bfrom%2Bpoint%2Bclouds&journal=Proceedings+of+the+IEEE%2FCVF+conference+on+computer+vision+and+pattern+recognition&pages=12697-12705)
- 15
	LinT.-Y.DollárP.GirshickR.HeK.HariharanB.BelongieS. (2017a). “Feature pyramid networks for object detection,” in **Proceedings of the IEEE conference on computer vision and pattern recognition**, Honolulu, HI, 2117–2125. 10.1109/CVPR.2017.106
	- [CrossRef](https://doi.org/10.1109/CVPR.2017.106)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=T.-Y..%2BLin&author=P..%2BDoll%C3%A1r&author=R..%2BGirshick&author=K..%2BHe&author=B..%2BHariharan&author=S..%2BBelongie&publication_year=2017a&title=Feature%2Bpyramid%2Bnetworks%2Bfor%2Bobject%2Bdetection&journal=Proceedings+of+the+IEEE+conference+on+computer+vision+and+pattern+recognition&pages=2117-2125)
- 16
	LinT.-Y.GoyalP.GirshickR.HeK.DollárP. (2017b). “Focal loss for dense object detection,” in **Proceedings of the IEEE international conference on computer vision**, (Piscataway, NJ: IEEE), 2980–2988. 10.1109/ICCV.2017.324
	- [CrossRef](https://doi.org/10.1109/ICCV.2017.324)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=T.-Y..%2BLin&author=P..%2BGoyal&author=R..%2BGirshick&author=K..%2BHe&author=P..%2BDoll%C3%A1r&publication_year=2017b&title=Focal%2Bloss%2Bfor%2Bdense%2Bobject%2Bdetection&journal=Proceedings+of+the+IEEE+international+conference+on+computer+vision&pages=2980-2988)
- 17
	LiuX.JiangW.SuH.QiW.GeS. S. (2022a). **A control strategy of robot eye-head coordinated gaze behavior achieved for minimized neural transmission noise.**Piscataway, NJ: IEEE. 10.1109/TMECH.2022.3210592
	- [CrossRef](https://doi.org/10.1109/TMECH.2022.3210592)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=X..%2BLiu&author=W..%2BJiang&author=H..%2BSu&author=W..%2BQi&author=S.%2BS..%2BGe&publication_year=2022a&journal=A+control+strategy+of+robot+eye-head+coordinated+gaze+behavior+achieved+for+minimized+neural+transmission+noise.)
- 18
	LiuX.LiX.SuH.ZhaoY.GeS. S. (2022b). The opening workspace control strategy of a novel manipulator-driven emission source microscopy system.**ISA Trans.**10.1016/j.isatra.2022.09.002
	- [Pubmed Abstract](https://pubmed.ncbi.nlm.nih.gov/36163198)
	- [CrossRef](https://doi.org/10.1016/j.isatra.2022.09.002)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=X..%2BLiu&author=X..%2BLi&author=H..%2BSu&author=Y..%2BZhao&author=S.%2BS..%2BGe&publication_year=2022b&title=The%2Bopening%2Bworkspace%2Bcontrol%2Bstrategy%2Bof%2Ba%2Bnovel%2Bmanipulator-driven%2Bemission%2Bsource%2Bmicroscopy%2Bsystem.&journal=ISA+Trans.)
- 19
	LiuZ.ZhangZ.CaoY.HuH.TongX. (2021). “Group-free 3d object detection via transformers,” in **Proceedings of the IEEE/CVF international conference on computer vision**, Montreal, 2949–2958. 10.1109/ICCV48922.2021.00294
	- [CrossRef](https://doi.org/10.1109/ICCV48922.2021.00294)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=Z..%2BLiu&author=Z..%2BZhang&author=Y..%2BCao&author=H..%2BHu&author=X..%2BTong&publication_year=2021&title=Group-free%2B3d%2Bobject%2Bdetection%2Bvia%2Btransformers&journal=Proceedings+of+the+IEEE%2FCVF+international+conference+on+computer+vision&pages=2949-2958)
- 20
	LongN.YanH.WangL.LiH.YangQ. (2022). Unifying obstacle detection, recognition, and fusion based on the polarization color stereo camera and LiDAR for the ADAS.**Sensors (Basel)** 22:2453. 10.3390/s22072453
	- [Pubmed Abstract](https://pubmed.ncbi.nlm.nih.gov/35408068)
	- [CrossRef](https://doi.org/10.3390/s22072453)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=N..%2BLong&author=H..%2BYan&author=L..%2BWang&author=H..%2BLi&author=Q..%2BYang&publication_year=2022&title=Unifying%2Bobstacle%2Bdetection%2C%2Brecognition%2C%2Band%2Bfusion%2Bbased%2Bon%2Bthe%2Bpolarization%2Bcolor%2Bstereo%2Bcamera%2Band%2BLiDAR%2Bfor%2Bthe%2BADAS.&journal=Sensors+%28Basel%29&volume=22)
- 21
	OtgonboldM.-E.GochooM.AlnajjarF.AliL.TanT.-H.HsiehJ.-W.et al (2022). SHEL5K: An extended dataset and benchmarking for safety helmet detection.**Sensors (Basel)** 22:2315. 10.3390/s22062315
	- [Pubmed Abstract](https://pubmed.ncbi.nlm.nih.gov/35336491)
	- [CrossRef](https://doi.org/10.3390/s22062315)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=M.-E..%2BOtgonbold&author=M..%2BGochoo&author=F..%2BAlnajjar&author=L..%2BAli&author=T.-H..%2BTan&author=J.-W..%2BHsieh&publication_year=2022&title=SHEL5K%3A%2BAn%2Bextended%2Bdataset%2Band%2Bbenchmarking%2Bfor%2Bsafety%2Bhelmet%2Bdetection.&journal=Sensors+%28Basel%29&volume=22)
- 22
	PangJ.ChenK.ShiJ.FengH.OuyangW.LinD. (2019). “Libra r-cnn: Towards balanced learning for object detection,” in **Proceedings of the IEEE/CVF conference on computer vision and pattern recognition**, Long Beach, CA, 821–830. 10.1109/CVPR.2019.00091
	- [CrossRef](https://doi.org/10.1109/CVPR.2019.00091)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=J..%2BPang&author=K..%2BChen&author=J..%2BShi&author=H..%2BFeng&author=W..%2BOuyang&author=D..%2BLin&publication_year=2019&title=Libra%2Br-cnn%3A%2BTowards%2Bbalanced%2Blearning%2Bfor%2Bobject%2Bdetection&journal=Proceedings+of+the+IEEE%2FCVF+conference+on+computer+vision+and+pattern+recognition&pages=821-830)
- 23
	PerekP.MielczarekA.MakowskiD. (2022). High-performance image acquisition and processing for stereoscopic diagnostic systems with the application of graphical processing units.**Sensors (Basel)** 22:471. 10.3390/s22020471
	- [Pubmed Abstract](https://pubmed.ncbi.nlm.nih.gov/35062431)
	- [CrossRef](https://doi.org/10.3390/s22020471)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=P..%2BPerek&author=A..%2BMielczarek&author=D..%2BMakowski&publication_year=2022&title=High-performance%2Bimage%2Bacquisition%2Band%2Bprocessing%2Bfor%2Bstereoscopic%2Bdiagnostic%2Bsystems%2Bwith%2Bthe%2Bapplication%2Bof%2Bgraphical%2Bprocessing%2Bunits.&journal=Sensors+%28Basel%29&volume=22)
- 24
	QiC. R.LitanyO.HeK.GuibasL. J. (2019). “Deep hough voting for 3d object detection in point clouds,” in **Proceedings of the IEEE/CVF international conference on computer vision**, Seoul, 9277–9286. 10.1109/ICCV.2019.00937
	- [CrossRef](https://doi.org/10.1109/ICCV.2019.00937)
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=C.%2BR..%2BQi&author=O..%2BLitany&author=K..%2BHe&author=L.%2BJ..%2BGuibas&publication_year=2019&title=Deep%2Bhough%2Bvoting%2Bfor%2B3d%2Bobject%2Bdetection%2Bin%2Bpoint%2Bclouds&journal=Proceedings+of+the+IEEE%2FCVF+international+conference+on+computer+vision&pages=9277-9286)
- 25
	QiC. R.SuH.MoK.GuibasL. J. (2017a). “Pointnet: Deep learning on point sets for 3d classification and segmentation,” in **Proceedings of the IEEE conference on computer vision and pattern recognition**, Stanford, CA, 652–660.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=C.%2BR..%2BQi&author=H..%2BSu&author=K..%2BMo&author=L.%2BJ..%2BGuibas&publication_year=2017a&title=Pointnet%3A%2BDeep%2Blearning%2Bon%2Bpoint%2Bsets%2Bfor%2B3d%2Bclassification%2Band%2Bsegmentation&journal=Proceedings+of+the+IEEE+conference+on+computer+vision+and+pattern+recognition&pages=652-660)
- 26
	QiC. R.YiL.SuH.GuibasL. J. (2017b). “Pointnet++: Deep hierarchical feature learning on point sets in a metric space,” in **Proceedings of the 31st international conference on neural information processing systems**, Stanford, CA, 30.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=C.%2BR..%2BQi&author=L..%2BYi&author=H..%2BSu&author=L.%2BJ..%2BGuibas&publication_year=2017b&title=Pointnet%2B%2B%3A%2BDeep%2Bhierarchical%2Bfeature%2Blearning%2Bon%2Bpoint%2Bsets%2Bin%2Ba%2Bmetric%2Bspace&journal=Proceedings+of+the+31st+international+conference+on+neural+information+processing+systems)
- 27
	QiW.OvurS. E.LiZ.MarzulloA.SongR. J. I. R.LettersA. (2021). Multi-sensor guided hand gesture recognition for a teleoperated robot using a recurrent neural network.**IEEE Robot. Autom. Lett.**66039–6045.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=W..%2BQi&author=S.%2BE..%2BOvur&author=Z..%2BLi&author=A..%2BMarzullo&author=R.%2BJ.%2BI.%2BR..%2BSong&author=A..%2BLetters&publication_year=2021&title=Multi-sensor%2Bguided%2Bhand%2Bgesture%2Brecognition%2Bfor%2Ba%2Bteleoperated%2Brobot%2Busing%2Ba%2Brecurrent%2Bneural%2Bnetwork.&journal=IEEE+Robot.+Autom.+Lett.&volume=6&pages=6039-6045)
- 28
	RedmonJ.FarhadiA. (2017). “YOLO9000: Better, faster, stronger,” in **Proceedings of the IEEE conference on computer vision and pattern recognition**, Honolulu, HI, 7263–7271.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=J..%2BRedmon&author=A..%2BFarhadi&publication_year=2017&title=YOLO9000%3A%2BBetter%2C%2Bfaster%2C%2Bstronger&journal=Proceedings+of+the+IEEE+conference+on+computer+vision+and+pattern+recognition&pages=7263-7271)
- 29
	RenS.HeK.GirshickR.SunJ. (2015). Faster r-cnn: Towards real-time object detection with region proposal networks.**Adv. Neural Inf. Process. Syst.**2891–99.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=S..%2BRen&author=K..%2BHe&author=R..%2BGirshick&author=J..%2BSun&publication_year=2015&title=Faster%2Br-cnn%3A%2BTowards%2Breal-time%2Bobject%2Bdetection%2Bwith%2Bregion%2Bproposal%2Bnetworks.&journal=Adv.+Neural+Inf.+Process.+Syst.&volume=28&pages=91-99)
- 30
	ShiS.GuoC.JiangL.WangZ.ShiJ.WangX.et al (2020a). “Pv-rcnn: Point-voxel feature set abstraction for 3d object detection,” in **Proceedings of the IEEE/CVF conference on computer vision and pattern recognition**, Seattle, WA, 10529–10538.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=S..%2BShi&author=C..%2BGuo&author=L..%2BJiang&author=Z..%2BWang&author=J..%2BShi&author=X..%2BWang&publication_year=2020a&title=Pv-rcnn%3A%2BPoint-voxel%2Bfeature%2Bset%2Babstraction%2Bfor%2B3d%2Bobject%2Bdetection&journal=Proceedings+of+the+IEEE%2FCVF+conference+on+computer+vision+and+pattern+recognition&pages=10529-10538)
- 31
	ShiS.WangZ.ShiJ.WangX.LiH. (2020b). From points to parts: 3d object detection from point cloud with part-aware and part-aggregation network.**IEEE Trans. Pattern Anal. Mach. Intell.**432647–2664.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=S..%2BShi&author=Z..%2BWang&author=J..%2BShi&author=X..%2BWang&author=H..%2BLi&publication_year=2020b&title=From%2Bpoints%2Bto%2Bparts%3A%2B3d%2Bobject%2Bdetection%2Bfrom%2Bpoint%2Bcloud%2Bwith%2Bpart-aware%2Band%2Bpart-aggregation%2Bnetwork.&journal=IEEE+Trans.+Pattern+Anal.+Mach.+Intell.&volume=43&pages=2647-2664)
- 32
	ShiS.WangX.LiH. (2019). “Pointrcnn: 3d object proposal generation and detection from point cloud,” in **Proceedings of the IEEE/CVF conference on computer vision and pattern recognition**, Long Beach, CA, 770–779.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=S..%2BShi&author=X..%2BWang&author=H..%2BLi&publication_year=2019&title=Pointrcnn%3A%2B3d%2Bobject%2Bproposal%2Bgeneration%2Band%2Bdetection%2Bfrom%2Bpoint%2Bcloud&journal=Proceedings+of+the+IEEE%2FCVF+conference+on+computer+vision+and+pattern+recognition&pages=770-779)
- 33
	SiddiqueA.LeeS. (2022). Sym3DNet: Symmetric 3D prior network for single-view 3D reconstruction.**Sensors (Basel)** 22:518.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=A..%2BSiddique&author=S..%2BLee&publication_year=2022&title=Sym3DNet%3A%2BSymmetric%2B3D%2Bprior%2Bnetwork%2Bfor%2Bsingle-view%2B3D%2Breconstruction.&journal=Sensors+%28Basel%29&volume=22)
- 34
	SimonyM.MilzyS.AmendeyK.GrossH.-M. (2018). “Complex-yolo: An euler-region-proposal for real-time 3d object detection on point clouds,” in **Proceedings of the European conference on computer vision (ECCV) workshops**, Munich, 197–209.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=M..%2BSimony&author=S..%2BMilzy&author=K..%2BAmendey&author=H.-M..%2BGross&publication_year=2018&title=Complex-yolo%3A%2BAn%2Beuler-region-proposal%2Bfor%2Breal-time%2B3d%2Bobject%2Bdetection%2Bon%2Bpoint%2Bclouds&journal=Proceedings+of+the+European+conference+on+computer+vision+%28ECCV%29+workshops&pages=197-209)
- 35
	VaswaniA.ShazeerN.ParmarN.UszkoreitJ.JonesL.GomezA. N.et al (2017). Attention is all you need.**Adv. Neural Inf. Process. Syst.**305998–6008.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=A..%2BVaswani&author=N..%2BShazeer&author=N..%2BParmar&author=J..%2BUszkoreit&author=L..%2BJones&author=A.%2BN..%2BGomez&publication_year=2017&title=Attention%2Bis%2Ball%2Byou%2Bneed.&journal=Adv.+Neural+Inf.+Process.+Syst.&volume=30&pages=5998-6008)
- 36
	WahabF.UllahI.ShahA.KhanR. A.ChoiA.AnwarM. S. (2022). Design and implementation of real-time object detection system based on single-shoot detector and OpenCV.**Front. Psychol.**13:1039645.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=F..%2BWahab&author=I..%2BUllah&author=A..%2BShah&author=R.%2BA..%2BKhan&author=A..%2BChoi&author=M.%2BS..%2BAnwar&publication_year=2022&title=Design%2Band%2Bimplementation%2Bof%2Breal-time%2Bobject%2Bdetection%2Bsystem%2Bbased%2Bon%2Bsingle-shoot%2Bdetector%2Band%2BOpenCV.&journal=Front.+Psychol.&volume=13)
- 37
	WangD.MaG.LiuX. (2022). An intelligent recognition framework of access control system with anti-spoofing function.**AIMS Maths.**710495–10512.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=D..%2BWang&author=G..%2BMa&author=X..%2BLiu&publication_year=2022&title=An%2Bintelligent%2Brecognition%2Bframework%2Bof%2Baccess%2Bcontrol%2Bsystem%2Bwith%2Banti-spoofing%2Bfunction.&journal=AIMS+Maths.&volume=7&pages=10495-10512)
- 38
	WooS.ParkJ.LeeJ.-Y.KweonI. S. (2018). “Cbam: Convolutional block attention module,” in **Proceedings of the European conference on computer vision (ECCV)**, Munich, 3–19.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=S..%2BWoo&author=J..%2BPark&author=J.-Y..%2BLee&author=I.%2BS..%2BKweon&publication_year=2018&title=Cbam%3A%2BConvolutional%2Bblock%2Battention%2Bmodule&journal=Proceedings+of+the+European+conference+on+computer+vision+%28ECCV%29&pages=3-19)
- 39
	XuY.SuH.MaG.LiuX. J. C. (2022). A novel dual-modal emotion recognition algorithm with fusing hybrid features of audio signal and speech context.**Complex Intell. Syst.**1–13.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=Y..%2BXu&author=H..%2BSu&author=G..%2BMa&author=X.%2BJ.%2BC..%2BLiu&publication_year=2022&title=A%2Bnovel%2Bdual-modal%2Bemotion%2Brecognition%2Balgorithm%2Bwith%2Bfusing%2Bhybrid%2Bfeatures%2Bof%2Baudio%2Bsignal%2Band%2Bspeech%2Bcontext.&journal=Complex+Intell.+Syst.&pages=1-13)
- 40
	YanY.MaoY.LiB. (2018). Second: Sparsely embedded convolutional detection.**Sensors (Basel)** 18:3337.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=Y..%2BYan&author=Y..%2BMao&author=B..%2BLi&publication_year=2018&title=Second%3A%2BSparsely%2Bembedded%2Bconvolutional%2Bdetection.&journal=Sensors+%28Basel%29&volume=18)
- 41
	ZhouY.TuzelO. (2018). “Voxelnet: End-to-end learning for point cloud based 3d object detection,” in **Proceedings of the IEEE conference on computer vision and pattern recognition**, Salt Lake City, 4490– 4499.
	- [Google Scholar](http://scholar.google.com/scholar_lookup?author=Y..%2BZhou&author=O..%2BTuzel&publication_year=2018&title=Voxelnet%3A%2BEnd-to-end%2Blearning%2Bfor%2Bpoint%2Bcloud%2Bbased%2B3d%2Bobject%2Bdetection&journal=Proceedings+of+the+IEEE+conference+on+computer+vision+and+pattern+recognition&pages=4490-+4499)

## Summary

Keywords

deep learning, object detection, point cloud, 3D object detection, neural network

Citation

Shao Y, Sun Z, Tan A and Yan T (2023) Efficient three-dimensional point cloud object detection based on improved Complex-YOLO. *Front. Neurorobot.* 17:1092564. doi: [10.3389/fnbot.2023.1092564](http://dx.doi.org/10.3389/fnbot.2023.1092564)

Received

08 November 2022

Accepted

03 February 2023

Published

16 February 2023

Volume

17 - 2023

Edited by

Hang Su, Fondazione Politecnico di Milano, Italy

Reviewed by

Wen Qi, Politecnico di Milano, Italy; Xiaorui Liu, Qingdao University, China

Updates

Copyright

[^1]: TABLE 3 

<table><thead><tr><td align="left" colspan="2">Improved part</td><td align="center">Method A</td><td align="center">Method B</td><td align="center">Method C</td><td align="center">Method D</td><td align="center">Method E</td><td align="center">Method F</td><td align="center">Method G</td></tr></thead><tbody><tr><td align="left" colspan="2">E-RPN</td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td></td><td></td><td></td></tr><tr><td align="left" colspan="2">RepVGG-A2</td><td></td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td></tr><tr><td align="left" colspan="2">FPN</td><td></td><td></td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td></tr><tr><td align="left" colspan="2">Height detector</td><td></td><td></td><td></td><td align="center">√</td><td align="center">√</td><td align="center">√</td><td align="center">√</td></tr><tr><td align="left" colspan="2">CBAM</td><td></td><td></td><td></td><td></td><td></td><td align="center">√</td><td align="center">√</td></tr><tr><td align="left" colspan="2">Higher resolution</td><td></td><td></td><td></td><td></td><td></td><td></td><td align="center">√</td></tr><tr><td align="left">Car AP</td><td align="center">Easy</td><td align="center">67.72</td><td align="center">43.88</td><td align="center">49.68</td><td align="center">92.50</td><td align="center">87.37</td><td align="center"><b>97.01</b></td><td align="center">96.56</td></tr><tr><td></td><td align="center">Mod</td><td align="center">64.00</td><td align="center">41.19</td><td align="center">45.06</td><td align="center">86.89</td><td align="center">87.44</td><td align="center">88.98</td><td align="center"><b>89.10</b></td></tr><tr><td align="left">(IoU = 0.7)</td><td align="center">Hard</td><td align="center">63.01</td><td align="center">37.68</td><td align="center">43.24</td><td align="center">87.51</td><td align="center">87.90</td><td align="center">89.26</td><td align="center"><b>89.32</b></td></tr><tr><td align="left">Pedestrian AP</td><td align="center">Easy</td><td align="center">41.79</td><td align="center">27.06</td><td align="center">53.78</td><td align="center">65.67</td><td align="center">67.31</td><td align="center">70.53</td><td align="center"><b>71.33</b></td></tr><tr><td></td><td align="center">Mod</td><td align="center">39.70</td><td align="center">28.62</td><td align="center">55.99</td><td align="center">68.11</td><td align="center">69.92</td><td align="center">72.53</td><td align="center"><b>73.26</b></td></tr><tr><td align="left">(IoU = 0.5)</td><td align="center">Hard</td><td align="center">35.92</td><td align="center">29.70</td><td align="center">57.58</td><td align="center">69.52</td><td align="center">70.88</td><td align="center">73.42</td><td align="center"><b>74.11</b></td></tr><tr><td align="left">Cyclist AP</td><td align="center">Easy</td><td align="center">68.17</td><td align="center">62.92</td><td align="center">52.58</td><td align="center">85.15</td><td align="center">84.77</td><td align="center"><b>85.81</b></td><td align="center">84.76</td></tr><tr><td></td><td align="center">Mod</td><td align="center">58.32</td><td align="center">69.83</td><td align="center">48.34</td><td align="center">84.70</td><td align="center">76.79</td><td align="center"><b>85.92</b></td><td align="center">85.60</td></tr><tr><td align="left">(IoU = 0.5)</td><td align="center">Hard</td><td align="center">54.30</td><td align="center">69.60</td><td align="center">49.06</td><td align="center">84.94</td><td align="center">76.99</td><td align="center">86.13</td><td align="center"><b>86.72</b></td></tr><tr><td></td><td align="center">Easy</td><td align="center">59.22</td><td align="center">44.62</td><td align="center">52.01</td><td align="center">81.11</td><td align="center">79.82</td><td align="center"><b>84.45</b></td><td align="center">84.21</td></tr><tr><td align="left">mAP</td><td align="center">Mod</td><td align="center">54.00</td><td align="center">46.55</td><td align="center">49.80</td><td align="center">79.90</td><td align="center">78.05</td><td align="center">82.48</td><td align="center"><b>82.65</b></td></tr><tr><td></td><td align="center">Hard</td><td align="center">51.07</td><td align="center">45.66</td><td align="center">49.96</td><td align="center">80.66</td><td align="center">78.59</td><td align="center">82.94</td><td align="center"><b>83.38</b></td></tr><tr><td align="center" colspan="2">FPS</td><td align="center">108</td><td align="center">102</td><td align="center">78</td><td align="center">72</td><td align="center">71</td><td align="center">68</td><td align="center">48</td></tr></tbody></table>

Experimental results of the ablation experiments were used to verify the contribution of the improvements in this paper.

3D object detection results on the KITTI dataset. The evaluation metric is AP (Average Precision). The Intersection over Union (IoU) threshold is 0.7 for Car and 0.5 for Pedestrian/Cyclist. The bolded data are the highest scores for this item.

Method A is the original Complex-YOLO. It can be seen that the running speed is excellent and can reach 108 FPS on RTX3070Ti, but the detection accuracy is low.

Method B is a modified Complex-YOLO based RepVGG-A2 feature extraction network. Method B has higher detection accuracy for large objects like Car but lower detection accuracy for Pedestrian in 2D object detection in the bird’s-eye view perspective. The analysis found that the output feature map of RepVGG-A2 has a down-sampling multiplicity of 32, i.e., if the BEV-map with an input resolution of 608 × 608, then the output feature map size is 19 × 19. Such a smaller resolution means a larger perceptual field, which is not conducive to the prediction of small-sized objects. Method B has low accuracy in 3D object detection. In terms of detection speed, RepVGG still maintains a good detection speed, which can reach 102 FPS, because it consists of all 3 × 3 convolution in the inference stage and has no multi-branch structure.

Method C builds the FPN structure based on method B. Compared with method B, method C builds the FPN structure to accomplish the object detection of different sizes (38 × 38, 76 × 76, 152 × 152) at three different resolutions, greatly improving the 2D object detection accuracy of Pedestrian and Cyclist in the bird’s-eye view perspective, as well as the 3D object detection accuracy of Car and Pedestrian. In terms of detection speed, due to the construction of the FPN structure, which makes the network more complex, the detection speed is reduced to 78FPS, but the performance requirement of real-time detection is met.

Method D is based on method C, which adds the height detector. The addition of height detection in method D improves both 2D object detection accuracy in the bird’s-eye view perspective and 3D object detection accuracy.

Method E is based on method D, which removes the E-RPN structure and completes the detection of the orientation angle directly in the Cartesian coordinate system. The detection accuracy is also improved compared with method D.

Method F is an improved FPN structure built based on method E, the CBAM attention mechanism part is added to further improve the attention of the network for small objects.

Method G adds one more feature fusion operation of 2× up-sampling with adjacent resolution to the FPN structure based on method F, and complete the 3D object detection task at a higher resolution.

To express the improvements in this paper more intuitively, the improved algorithm and the detection results of the original Complex-YOLO are converted into images for presentation. shows the detection results of the two algorithms. From, it can be seen that the algorithm in this paper has a great improvement in 3D detection results compared to the original algorithm. Although the detection speed is slower than the original Complex-YOLO algorithm, it meets the performance requirements of real-time detection perfectly.

[^2]: TABLE 5 

<table><thead><tr><td align="left">Methods</td><td align="center" colspan="3">Car AP (IoU = 0.7)</td><td align="center" colspan="3">Pedestrian AP (IoU = 0.5)</td><td align="center" colspan="3">Cyclist AP (IoU = 0.5)</td></tr><tr><td align="left"></td><td align="center">Easy</td><td align="center">Mod</td><td align="center">Hard</td><td align="center">Easy</td><td align="center">Mod</td><td align="center">Hard</td><td align="center">Easy</td><td align="center">Mod</td><td align="center">Hard</td></tr></thead><tbody><tr><td align="left">PointRCNN</td><td align="center">90.31</td><td align="center">86.05</td><td align="center">79.86</td><td align="center"><b>86.26</b></td><td align="center"><b>77.97</b></td><td align="center">69.88</td><td align="center"><b>95.44</b></td><td align="center"><b>87.15</b></td><td align="center">86.11</td></tr><tr><td align="left">PV-RCNN</td><td align="center"><b>98.69</b></td><td align="center"><b>89.23</b></td><td align="center">88.52</td><td align="center">72.10</td><td align="center">68.78</td><td align="center">65.21</td><td align="center">89.65</td><td align="center">81.39</td><td align="center">80.49</td></tr><tr><td align="left">SECOND</td><td align="center">98.01</td><td align="center">88.97</td><td align="center">87.72</td><td align="center">65.80</td><td align="center">63.49</td><td align="center">60.05</td><td align="center">90.89</td><td align="center">84.26</td><td align="center">83.52</td></tr><tr><td align="left">PartA</td><td align="center">90.51</td><td align="center">88.64</td><td align="center">87.44</td><td align="center">72.46</td><td align="center">69.40</td><td align="center">67.01</td><td align="center">93.56</td><td align="center">83.57</td><td align="center">83.16</td></tr><tr><td align="left">PointPillars</td><td align="center">90.04</td><td align="center">88.19</td><td align="center">84.63</td><td align="center">63.09</td><td align="center">58.07</td><td align="center">55.77</td><td align="center">89.91</td><td align="center">82.81</td><td align="center">80.54</td></tr><tr><td align="left">VoxelRCNN</td><td align="center">90.22</td><td align="center">88.41</td><td align="center">87.06</td><td align="center">74.95</td><td align="center">67.11</td><td align="center">65.40</td><td align="center">94.02</td><td align="center">83.68</td><td align="center">82.30</td></tr><tr><td align="left">Complex-YOLOv3</td><td align="center">61.16</td><td align="center">55.69</td><td align="center">61.13</td><td align="center">51.63</td><td align="center">56.39</td><td align="center">59.51</td><td align="center">62.94</td><td align="center">64.55</td><td align="center">65.60</td></tr><tr><td align="left">Complex-YOLOv4</td><td align="center">62.39</td><td align="center">56.61</td><td align="center">61.81</td><td align="center">47.94</td><td align="center">51.82</td><td align="center">55.17</td><td align="center">51.78</td><td align="center">52.90</td><td align="center">54.35</td></tr><tr><td align="left">Our method</td><td align="center">96.56</td><td align="center">89.10</td><td align="center"><b>89.32</b></td><td align="center">71.33</td><td align="center">73.26</td><td align="center"><b>74.11</b></td><td align="center">84.76</td><td align="center">85.60</td><td align="center"><b>86.72</b></td></tr></tbody></table>

Experimental results of comparison experiments with other algorithms.

3D object detection results on the KITTI dataset. Comparison of the results of 3D object detection on the KITTI dataset with other algorithms. The evaluation metric is AP (Average Precision). The Intersection over Union (IoU) threshold is 0.7 for Car and 0.5 for Pedestrian/Cyclist. The bolded data are the highest scores for this item.