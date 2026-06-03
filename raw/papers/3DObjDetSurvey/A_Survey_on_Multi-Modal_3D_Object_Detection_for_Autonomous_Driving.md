---
title: "A Survey on Multi-Modal 3D Object Detection for Autonomous Driving"
source: "https://ieeexplore.ieee.org/abstract/document/11366738"
author:
published:
created: 2026-06-03
description: "This survey systematically reviews multi-modal 3D object detection in autonomous driving and introduces a novel three-dimensional analytical framework. The fram"
tags:
  - "clippings"
---
## Abstract:

This survey systematically reviews multi-modal 3D object detection in autonomous driving and introduces a novel three-dimensional analytical framework. The framework orga...

---

3D object detection is crucial for autonomous driving perception and safety. Early single-modal methods rely on a single sensor, which offers limited information and poor robustness. To overcome this, multi-modal fusion significantly improves detection accuracy and robustness by integrating data from different sensors. Although existing surveys \[1\] review related techniques from various perspectives, they often remain at the level of stage or method classification, lacking a systematic, multi-dimensional analysis of the entire fusion process. To fill this gap, we propose a three-dimensional analytical framework(as shown in Fig. 1) that provides a comprehensive overview of multi-modal 3D object detection from three dimensions: fusion stage, granularity, and method. Compared to existing work, our paper covers the entire multi-modal fusion process, revealing the interconnections among “when to fuse,” “what to fuse,” and “how to fuse.” This offers a more comprehensive and insightful systemic reference for both beginners and researchers.

[![Fig. 1. - Overview and category of multi-modal 3d object detection.](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11366221/11366691/11366738/11366738-fig-1-source-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11366221/11366691/11366738/11366738-fig-1-source-large.gif)

**Fig. 1.**

Overview and category of multi-modal 3d object detection.

In this section, we introduce the definition of 3D object detection and commonly used datasets and their evaluation metrics.

### A. 3D Object Detection Definition

3D object detection predicts the attributes of objects in a scene from single-sensor or multi-sensor inputs, aiming to generate accurate 3D bounding boxes. Its general formulation \[2\] can be expressed as:

$$
\begin{equation*}
B=F_{d e t}\left(T_{{sensor }}\right) \tag{1}
\end{equation*}
$$
 View Source

where $B=\{B 1, B 2, \ldots, B N\}$ is used to describe the set of $N$ 3D objects in the scene, $F_{\text {det }}$ represents the 3D detector, and $T_{\text {sensor }}$ denotes the input data from one or more sensors.

### B. Datasets and Evaluation Metrics

This section analyzes mainstream datasets(as illustrated in TABLE I) and evaluation metrics for multi-modal 3D object detection, highlighting their impact on research methods and performance evaluation.

**Table I** Datasets for 3d object detection in autonomous driving

**Datasets Comparisons**. As a pioneering benchmark, KITTI \[3\] provided a foundation for validating early algorithm. However, its limited scale and single-sensor configuration constrained model generalization in complex scenarios. In contrast, large-scale datasets such as nuScenes \[4\] and Waymo Open \[5\] have facilitated efficient multi-sensor fusion. For example, the sparse LiDAR in nuScenes has driven the development of image-semantic fusion methods, which compensate for point cloud sparsity by integrating image and LiDAR features, whereas the dense LiDAR in Waymo supports voxel-level fusion, enabling more accurate object detection through voxelization.

**Evaluation Metrics Analysis**. Evaluation metrics have evolved with practical demands. While traditional mAP(mean Average Precision) measures 3D bounding box localization, it is insufficient for dynamic scenarios. Accordingly, nuScenes introduced the NDS (nuScenes Detection Score), which integrates errors in orientation, velocity, and other factors to align with autonomous driving safety requirements. Similarly, Waymo's APH (Average Precision Heading) emphasizes heading accuracy, which is critical for path planning. These metrics shift the research focus from pure localization to comprehensive perception.

**Academic Benchmarks and Real-World Gaps**. Despite their strong performance on benchmarks, models often lack robustness in real-world scenarios. Discrepancies in dataset characteristics such as geography, weather, and traffic lead to poor generalization across datasets. For example, some models that excel on Waymo may experience a performance drop when transferred to the dense urban environments of nuScenes due to factors like sparse point clouds. Future research should focus on improving the universality and robustness of models to handle unforeseen complexities in autonomous driving.

Fusion stage concerns when heterogeneous multi-modal data are fused. Based on recent advances, we analyze it from early, late, and hybrid fusion to provide a systematic review for future research.

### A. Early Fusion

Early fusion, which usually occurs in the input or feature extraction phase before each branch reaches its prediction, is the most commonly used strategy in multi-modal 3D object detection. It integrates information from multiple sources at the raw data layer, generates new representations containing rich modal features, and makes full use of the intermediate features of each modality. However, the method has a high computational overhead, especially when dealing with high-dimensional heterogeneous data. Fig. 2 illustrates a typical early fusion architecture for LiDAR and camera fusion.

### B. Late Fusion

Late fusion, also known as decision-level fusion, integrates the predictions from different modalities at the final stage of the model (as illustrated in Fig. 3). This approach preserves each modality's unique features, reducing information loss, but it limits deep cross-modal interaction.

The CLOCs \[10\] adopts a late fusion strategy by utilizing separate 2D and 3D detectors to generate candidate boxes from the image and point cloud branches respectively. Then,these candidates are fused through geometric constraint-based matching, effectively avoiding the information redundancy commonly associated with early fusion. Building upon this, Fast-CLOCs \[11\] introduces a significant improvement with the novel 3D-Q-2D mechanism, which leverages 3D detection results to guide 2D detection. This design efficiently addresses the low integration efficiency of traditional multi-modal fusion while introducing negligible inference latency, thereby substantially enhancing overall fusion performance.

### C. Hybrid Fusion

Hybrid fusion establishes cascaded inter-modal relationships by fusing one branch's predictions with another's input (as shown in Fig. 4), leveraging one modality's prior to reduce another's search space and boost detection accuracy, typically with a dominant branch and auxiliary modalities-though it increases model complexity and training difficulty. F-PointNet \[12\] innovatively applies this to 2D detection, projecting image results into point clouds to generate 3D concave regions and fuse semantic-geometric info; its improvements like F-ConvNet \[13\]'s mesh processing use 2D efficiency to reduce search space while preserving point cloud geometric accuracy, yet reliance on 2D detection accuracy remains a key issue.

**Fig. 2.**

Multi-modal early fusion process \[1\].

**Fig. 3.**

Multi-modal late fusion process \[1\].

**Fig. 4.**

Multi-modal hybrid fusion process \[1\].

**Analysis**. Each of early, late, and hybrid fusion has its own benefits, but is also subject to intrinsic drawbacks. Early fusion fully exploits raw data but incurs high computational overhead and heavily relies on precise sensor calibration, making it difficult to meet the real-time and robustness requirements of autonomous driving systems. Late fusion is more efficient but lacks deep cross-modal interaction, which restricts performance. Hybrid fusion balances the two, yet it is prone to error propagation and increases system design and training complexity. Future research should move beyond single-stage paradigms and explore scenario-adaptive fusion strategies to optimize both accuracy and efficiency.

The granularity of multi-modal fusion reflects the fineness of sensor data integration and influences the trade-off between detection performance and computational efficiency. This chapter analyzes three levels of fusion granularity: ROI-wise, voxel-wise, and point-wise.

### A. ROI Wise

ROI-wise fusion (as shown in Fig. 5) employs ROI pooling or geometric transformation to unify feature representations, effectively leveraging the strengths of different modalities for accurate detection while maintaining a simple and end-to-end optimizable structure. However, its late-stage fusion mechanism struggles to capture early cross-modal interactions and fine-grained features, often leading to loss of geometric and depth information as well as incorporation of background noise, resulting in coarse predictions. Consequently, this approach is more suitable for scenarios with prominent objects and often requires integration with other fusion strategies in high-precision tasks to achieve robust detection performance.

### B. Voxel Wise

Voxel-wise fusion divides LiDAR point clouds into small voxels and fuses cross-modal features at the voxel level(as illustrated in Fig. 6), participating in the object detection process earlier than ROI-wise fusion. This method uses the camera projection matrix to associate voxel features with image features. By projecting the centroid of each voxel and attaching corresponding image features, it effectively integrates multi-source data, enabling the capture of subtle object structures and significantly improving detection accuracy in complex scenarios. However, voxelization inevitably leads to the loss of spatial information, and there are inherent errors in the correspondence between camera images and voxel features, which requires interpolation projection techniques to optimize the matching.

**Fig. 5.**

Roi wise fusion process \[1\].

### C. Point Wise

Point-wise fusion maps 3D point cloud points to image pixels via a calibration matrix (as illustrated in Fig. 7). As an early fusion method, it embeds image features into point clouds using transformation matrices for efficient early multi-modal integration, boosting detail capture-especially in complex scenes and fine-grained detection, alleviating resolution-induced feature blurring to reliably fuse sparse point clouds with dense images. Yet, it has flaws: image occlusion invalidates point mapping, causing errors; storing image features per point increases memory and computation far more than voxel-wise fusion. Thus, practical use demands balancing performance and efficiency via rational fusion strategy selection.

**Analysis**. Fusion granularity affects information integrity, efficiency, and complexity. ROI-wise fusion improves efficiency but loses geometric details; voxel-wise fusion balances efficiency and completeness yet suffers from voxelization errors; point-wise fusion retains full information but is computationally expensive and calibration-sensitive. Recently, projecting multi-modal features into the BEV(bird's eye view) space has shown a better trade-off between information integrity and efficiency, providing a promising direction for future research on efficient and accurate multi-modal fusion.

In multi-modal systems, data representation aims to leverage the complementarity of heterogeneous data and reduce inter-modal discrepancies to enable more effective fusion. However, since data from different sensors typically reside in distinct heterogeneous coordinate systems, establishing correspondences across modalities is highly challenging. To address this, data alignment techniques have been proposed to coordinate spatial or feature-level relationships between modalities. Once aligned, the heterogeneous data can be fused to further enhance detection performance. Therefore, dat representation, alignment, and fusion are critical to multimodal 3D object detection, and this chapter provides a detailed analysis of these aspects.

**Fig. 6.**

Voxel wise fusion process \[1\].

**Fig. 7.**

Point wise fusion process \[1\].

### A. Data Representation

Data representation is crucial for multi-modal fusion. This chapter categorizes existing methods into two groups based on whether feature fusion is performed in a unified space: unified representation and raw representation.

1. *Unified Representation*: Unified representation optimizes fusion by projecting multi-modal data into the same space, mainly including three types of methods: The data dimension elevation method expands two-dimensional data into three dimensions. For example, UVTR \[14\] embeds 2D image features into 3D voxels. Representatively, the data dimension reduction method, such as MV3D \[15\], reduces 3D point clouds to 2D views and fuses them with images, and its idea has been widely adopted. The method based on the BEV is a research hotspot. BEVFusion \[16\] encodes multi-modal data into the BEV space for fusion, and methods like BEVDet \[17\] project 2D features of multi-view images to achieve feature integration in the BEV space. These methods promote the development of data representation technology for multi-modal 3D object detection through dimension conversion and unified space strategies.
2. Raw Representation: The original representation directly fuses the multi-modal raw data without any transformation or encoding, maximizing the retention of the original information from the sensors. PointFusion \[18\] takes the cropped RGB images and the original point cloud data as inputs, avoiding the quantization loss of the point cloud and fully integrating the heterogeneous features of the images and the point clouds, providing a practical example of the fusion strategy that preserves the original characteristics of the data.

### B. Data Alignment

Feature alignment for heterogeneous multi-modal data is crucial to effective fusion. This section reviews alignment methods through hard and soft associations, providing insights for the development of fusion technologies.

1. *Hard Correlation*: Hard association-based feature alignment projects image semantics onto point clouds using calibration matrices, enabling explicit spatial alignment and enhancing semantic representation. F-PointNet \[12\] constructs 3D frustums from 2D boxes, while methods like PointPainting \[19\] and MVX-Net \[20\] enrich point clouds by “painting” them with segmentation results. These methods offer high alignment accuracy but are sensitive to occlusion and projection errors. Below is the process representation of projecting lidar point cloud features onto image features:
	$$
	\begin{equation*}
	z_{c}\begin{bmatrix}
	u \\
	v \\
	1
	\end{bmatrix}=h K\left[\begin{array}{ll}
	R & T
	\end{array}\right]\begin{bmatrix}
	P_{x} \\
	P_{y} \\
	P_{z} \\
	1
	\end{bmatrix} \tag{2}
	\end{equation*}
	$$
	 View Source
	Where $P_{x}, P_{y}, P_{z}$ are LiDAR coordinates, ($u, v$) are image coordinates, $Z_{c}$ is depth, $K$ is the camera intrinsics, $R$ and $T$ are the LiDAR-to-camera transformation, and $h$ is the downsampling scale.
2. *Soft Correlation*: Soft association-based feature alignment outperforms hard association methods by leveraging attention mechanisms, which more effectively capture cross-modal correspondences. This approach not only offers greater flexibility but also reduces misalignment and projection errors that often arise from calibration matrices. Transformer-based fusion methods, such as TransFusion \[21\], effectively utilize self-attention to capture long-range dependencies and cross-attention to align and fuse features, leading to notable improvements in detection accuracy. The attention mechanism in Transformer can be expressed as:
	$$
	\begin{equation*}
	\operatorname{Attention}(Q, K, V)=\operatorname{softmax}\left(\frac{Q K^{T}}{\sqrt{d_{k}}}\right) V \tag{3}
	\end{equation*}
	$$
	 View Source
	where $Q, K$, and $V$ represent the query, key, and value respectively, while $d k$ is the dimension of the key vector.

### C. Data Fusion

Data fusion improves detection accuracy by integrating aligned data. Traditional methods (e.g., algebraic, Kalman filtering) underperform due to poor modal correlation mining. Deep learning and attention-based methods, leveraging efficient fusion, significantly optimize detection. The existing multi-modal fusion methods are detailed in TABLE II.

1. *Traditional Methods*: Traditional multi-modal fusion methods mostly rely on arithmetic operations (such as addition and averaging) and feature concatenation to achieve data integration. Arithmetic operations generate composite vectors through operations on features of the same dimension, which are simple to calculate. For example, MV3D \[15\] deeply fuses multi-view features using element-wise means, and ContFuse \[23\] combines features through element-wise summation. Although the concatenation operation has higher computational costs, it is more favored because it can retain more information. However, traditional methods exhibit poor robustness in complex scenarios and are highly sensitive to data misalignment and noise interference. Consequently, researchers have increasingly turned to attention-based fusion approaches, which enhance fusion effectiveness and model performance by adaptively focusing on key features.
2. *Attention-based methods*: DETR \[30\] innovatively combines the strengths of CNN and Transformer for end-to-end object detection, streamlining traditional processes, with its derivative DETR3D \[31\] extending into the 3D domain. Attention-based fusion methods, by intelligently assigning weights, integrate multi-modal data more precisely and efficiently than traditional approaches, notably enhancing model accuracy and robustness. FUTR3D \[26\] leverages multi-head self-attention and relative position bias to deepen multi-modal understanding; CMT \[27\] achieves accurate prediction via coordinate encoding. While these methods effectively harness the potential of the attention mechanism to optimize 3D object detection, challenges like high data demands and substantial computing resource consumption remain critical areas for future research.

**Table II** Multi-modal fusion methods

**Analysis**. Fusion methods for multi-modal 3D object detection typically involve three core aspects: data representation, alignment, and fusion. While significant progress has been made, challenges remain. At the data representation level, inter-modal transformations inevitably lead to information loss and computational overhead. For alignment, rigid association methods depend on precise calibration and are susceptible to environmental factors, whereas attention-based soft association, although better at modeling cross-modal relationships, is computationally expensive. At the fusion level, traditional methods lack adaptability, and attention mechanisms, despite significant performance gains, require substantial computational power. Future research should take a systemic, co-design approach, jointly optimizing representation and alignment while incorporating self-supervised learning and uncertainty modeling to enhance overall efficiency and robustness.

Multi-modal 3D object detection enhances autonomous driving perception through sensor fusion, yet practical deployment still faces challenges. This chapter discusses these issues and future directions.

**Data Augmentation**: Data augmentation is crucial for multi-modal detection, but the heterogeneous nature of different modalities makes traditional single-modal augmentation methods less applicable. For instance, applying random geometric transformations independently to images and point clouds can disrupt their cross-modal correspondence. Such inconsistency degrades the performance of methods \[19\] that rely on precise alignment. Future work should leverage deep generative models and self-supervised learning to generate annotation-free yet modality-consistent augmented data, thereby improving model generalization and robustness while preserving cross-modal correspondence.

**Information Loss**: The fusion of heterogeneous data inevitably leads to information loss. Late fusion(such as CLOCS \[10\] and Fast-CLOCs \[11\]) lacks fine-grained interaction, while point cloud voxelization \[29\] or coarse-grained fusion strategies can result in the loss of geometric details. This significantly impacts the detection accuracy of small objects. Future research should explore adaptive attention mechanisms, learnable calibration, and flexible fusion strategies to achieve a better balance between efficiency and accuracy, thereby minimizing information loss.

**Robustness**: Robustness remains a core challenge for multi-modal 3D detection in dynamic environments. Sensor data are prone to degradation under challenging conditions: inclement weather reduces LiDAR point cloud integrity and camera visibility, while urban occlusion leads to incomplete perceptual data. Although methods like uncertainty modeling and adversarial training improve adaptability, their computational complexity often hinders real-time deployment on embedded platforms. Developing efficient and robust fusion algorithms that maintain reliable performance under noise and extreme conditions is therefore essential for dependable autonomous driving systems.

Unlike surveys that focus solely on fusion strategies or a single dimension, our three-dimensional framework provides a more holistic and interconnected analysis of the entire fusion pipeline. In this framework, fusion stage determines the timing of information interaction, fusion granularity affects information integrity and accuracy, and fusion method shapes the effectiveness of multi-modal feature integration. Together, these factors jointly influence detection accuracy, real-time performance, and robustness. Despite recent advances, multi-modal 3D object detection still faces challenges such as information loss and limited robustness. Future research should prioritize lightweight and adaptive fusion architectures, self-supervised learning, and cross-modal data augmentation to achieve reliable deployment and enhance the safety and efficiency of autonomous driving perception.