---
title: "Few-Shot Learning in Video and 3D Object Detection: A Survey"
source: "https://arxiv.org/html/2507.17079v1"
author:
published:
created: 2026-06-03
description:
tags:
  - "clippings"
---
Md Meftahul Ferdaus Kendall N. Niles Joe Tom Mahdi Abdelguerfi Elias Ioup

###### Abstract

Few-shot learning (FSL) enables object detection models to recognize novel classes given only a few annotated examples, thereby reducing expensive manual data labeling. This survey examines recent FSL advances for video and 3D object detection. For video, FSL is especially valuable since annotating objects across frames is more laborious than for static images. By propagating information across frames, techniques like tube proposals and temporal matching networks can detect new classes from a couple examples, efficiently leveraging spatiotemporal structure. FSL for 3D detection from LiDAR or depth data faces challenges like sparsity and lack of texture. Solutions integrate FSL with specialized point cloud networks and losses tailored for class imbalance. Few-shot 3D detection enables practical autonomous driving deployment by minimizing costly 3D annotation needs. Core issues in both domains include balancing generalization and overfitting, integrating prototype matching, and handling data modality properties. In summary, FSL shows promise for reducing annotation requirements and enabling real-world video, 3D, and other applications by efficiently leveraging information across feature, temporal, and data modalities. By comprehensively surveying recent advancements, this paper illuminates FSL’s potential to minimize supervision needs and enable deployment across video, 3D, and other real-world applications.

###### keywords:

Data Scarcity, Few-Shot Learning, Modern Tracking Systems, Video, 3D Object Detection

\[inst1\]organization=Canizaro Livingston Gulf States Center for Environmental Informatics,addressline=University of New Orleans, city=New Orleans, postcode=70148, state=Louisiana, country=USA

\[inst2\]organization=US Army Corps of Engineers,city=Vicksburg, postcode=39183, state=Mississippi, country=USA

\[inst3\]organization=Center for Geospatial Sciences, Naval Research Laboratory,addressline=Stennis Space Center, city=Hancock County, postcode=39529, state=Mississippi, country=USA

## 1 Introduction

Object detection is a fundamental task in computer vision that involves locating and classifying objects belonging to predefined categories in images or video frames [^1]. Over the years, deep convolutional neural networks (CNNs) have revolutionized object detection with remarkable accuracy [^2]. However, the success of these models heavily relies on large annotated datasets for training, which are often costly and time-consuming to acquire. The data scarcity problem poses a significant challenge to the development of robust object detectors that can generalize well to new, unseen objects and domains [^3].

To address the limitations of data scarcity, considerable research has been devoted to exploring few-shot and zero-shot learning techniques in the field of object detection [^4]. Few-shot learning (FSL), in particular, seeks to recognize novel object categories with only a few training examples per class, typically ranging from 1 to 5 [^5]. The aim is to minimize the prohibitive annotation effort and enable the scalable deployment of object detectors in real-world applications [^6]. By leveraging knowledge transfer and efficient adaptation, FSL methods strive to extract transferable knowledge from a set of base classes with abundant labeled data, enabling generalization to novel classes with limited available examples [^7] [^8] [^9].

Effective FSL algorithms introduce strong inductive biases into models, allowing for rapid adaptation using the limited annotations associated with novel classes. Meta-learning algorithms [^10], which train models to quickly adapt to new tasks and metrics with few examples, have shown promise in this regard [^11]. Transfer learning from related domains and data augmentation techniques are also commonly employed to enhance FSL performance [^12] [^13]. Additionally, distance metric learning is utilized to learn embeddings that reflect semantic class relationships, aiding in effective few-shot object detection [^14].

While few-shot classification has been extensively explored, few-shot object detection presents unique challenges [^15]. In addition to recognizing object classes with limited data, few-shot object detection requires accurate object localization. This localization task becomes particularly challenging when only a small number of examples are available [^16]. By overcoming these challenges, FSL techniques have the potential to revolutionize the field of object detection [^17]. They can enable accurate and efficient detection of novel objects with minimal annotated data, enhancing the scalability and real-world applicability of object detectors. In this survey, we comprehensively investigate recent advancements in FSL techniques applied to video and 3D object detection, examining their strengths, limitations, and potential for future development.

### 1.1 Motivation

The field of object detection has witnessed significant advancements with the rise of deep learning and convolutional neural networks (CNNs). However, these advancements primarily focus on 2D image-based object detection, which poses limitations in real-world scenarios where objects exist in three-dimensional space and exhibit temporal dynamics [^18] [^19]. Hence, there is a pressing need to explore and understand the progress made in video and 3D object detection. However, existing surveys on FSL have not focused specifically on video or 3D object detection [^8] [^20] [^7] [^21] [^22].

Video object detection is of paramount importance in various domains such as surveillance, autonomous driving, and action recognition. However, the task of detecting objects in videos presents unique challenges compared to static image-based detection. These challenges arise from the need to cope with motion blur, occlusions, and object interactions across frames [^23] [^19] [^18] [^24]. By conducting a survey specifically dedicated to video object detection, we aim to provide a comprehensive overview of the latest methodologies, techniques, and benchmarks, thus shedding light on the progress made in this critical area and identifying potential future research directions.

On the other hand, 3D object detection, especially in the context of autonomous driving, is crucial for enabling safe and reliable perception systems. Traditional object detection methods primarily rely on 2D sensors such as cameras, which may not provide accurate depth information and struggle with challenging lighting and weather conditions. Integrating LiDAR (Light Detection and Ranging) sensors with cameras can significantly enhance the detection accuracy by providing precise depth information. However, 3D object detection remains a challenging task due to the sparsity of LiDAR point clouds, object occlusions, and the need to handle large-scale 3D data [^25] [^26]. Our survey on 3D object detection aims to provide an in-depth analysis of the state-of-the-art techniques, highlighting their strengths, limitations, and novel approaches that address these challenges.

By conducting a survey on both video and 3D object detection, we aim to bridge the gap and provide a comprehensive understanding of the advancements and challenges in these emerging areas. By exploring the latest techniques, model architectures, and evaluation benchmarks, we can assess the progress made, identify gaps in current approaches, and propose potential research directions for future work. This survey serves as a valuable resource for researchers, practitioners, and developers working on video and 3D object detection, paving the way for further advancements in these domains.

### 1.2 Organization of the Paper

This paper is organized into seven sections as follows: Section 1 provides an introduction that motivates the need for a comprehensive survey on FSL techniques for video and 3D object detection. It highlights the unique challenges posed by these domains and outlines the structure of the paper. Section 2 establishes the theoretical foundations of few-shot learning by reviewing key concepts, problem formulations, and common strategies. It focuses on principles like the support set, episodic training, meta-learning, metric-based approaches, data augmentation, and regularization. Section 3 explores the fundamentals of object detection, including two-stage and one-stage detector paradigms. It analyzes influential architectures like Faster R-CNN, YOLO, and SSD, and examines video and 3D detection approaches. Section 4 provides an in-depth analysis of state-of-the-art few-shot techniques tailored for video object detection. It discusses specialized model architectures, losses, and training methodologies to overcome video-specific challenges. Section 5 investigates few-shot learning strategies for 3D object detection using modalities like LiDAR. It reviews model designs, losses, and training procedures enabling effective few-shot detection on sparse 3D data. Section 6 identifies open challenges and promising research directions to advance few-shot video and 3D object detection. It proposes solutions to limitations in existing approaches. Section 7 presents concluding remarks and summarizes the key insights. Additional architectural diagrams, detailed comparisons, and secondary discussions are provided in the supplementary material. To provide an overview of the paper structure, a visual taxonomy outlining the relationships between the key sections and topics is presented in Figure S1. This diagram aims to enhance comprehension of the survey scope and content flow for the reader.

![Refer to caption](https://arxiv.org/html/2507.17079v1/extracted/6645249/images/taxonomy_vdo_3d_whole3.png)

Figure S1: Visual taxonomy illustrating comprehensive structural organization of survey content

## 2 Foundations of Few-Shot Learning

Few-shot learning (FSL) has emerged as a critical research area in deep learning to address the pressing need for vast labeled data, which is often expensive, time-consuming, or infeasible to obtain in real-world scenarios. As deep learning model complexity grows with millions or billions of parameters, substantial data is required to avoid overfitting and ensure generalizability [^27]. FSL counters this limitation by recognizing new visual concepts from only a few labeled examples, typically 1-5 shots. FSL problems are commonly formulated as classification tasks, where models are provided with scarce labeled examples of new classes called support sets, and must predict labels of unseen query samples from those classes [^28]. Meta-learning algorithms are widely utilized to train FSL models by learning to swiftly adapt to new tasks through experience gained from prior tasks [^29]. Metric-based approaches have also proven effective by learning distance metrics to measure support and query sample similarities [^30]. Additionally, transfer learning by pre-training on labeled data can enhance FSL performance [^31]. This section summarizes the core principles and techniques underpinning FSL, with further details in the supplementary document.

Central to FSL is the sparse support set representing each new class that models must generalize from [^32] [^7] [^33]. Key training strategies include episodic training on simulated few-shot tasks [^34] [^35] and transfer learning to utilize knowledge from data-rich base classes when adapting to novel classes [^36]. Fine-tuning and in-context learning show promise, but require careful experimental design and tuning [^37] [^38] [^39] [^40] [^41] [^42]. Feature extraction directly applies pre-trained models to novel classes [^43] [^44] [^45] [^45]. Classifier retraining uses base model features to train new classifiers from scratch [^46] [^47]. Weight imprinting provides informed initialization of novel class weights [^48] [^49]. Overall, transfer learning enables utilizing prior base class knowledge when adapting to limited novel data. Vital techniques involve meta-learning algorithms that optimize for rapid adaptation [^29] [^50], metric-based approaches for classification by learned sample similarities [^28] [^51] [^5], data augmentation for regularization [^52] [^53] [^54], and explicit regularization to prevent overfitting [^55]. Together, these mechanisms provide models with essential generalization capabilities and inductive biases tailored for effective learning under limited supervision, enabling rapid adaptation and knowledge transfer when data is scarce [^56] [^57] [^58]. The supplementary document provides additional details on the support set, problem formulations, training strategies, and inductive biases discussed in this foundations of few-shot learning section.

## 3 Foundations of Object Detection

This section provides an overview of fundamental concepts in object detection, before discussing techniques for video and 3D detection. Further architectural and methodological details are in the supplementary document.

Object detection integrates classification and localization to identify object categories within images and enclose them in bounding boxes [^59]. Given variability in object quantities, initial detection strategies leveraged sliding windows [^60]. However, convolutional neural networks (CNNs) now dominate [^61] [^62]. Object detection involves training from supervised datasets containing images $X$ and corresponding annotations $y$ to extract feature maps $F$ that enable bounding box regression and classification. There are two paradigms in object detection: the two-stage detector and the single-stage detector.

The Faster R-CNN architecture stands out in the two-stage detector category. It integrates ntegrates a Region Proposal Network (RPN) and a Fast R-CNN detection network. The RPN uses a convolutional network to generate object proposals with scores based on a set of anchor boxes. Proposals are reshaped via RoI pooling into fixed features for the detection network. The Fast R-CNN extracts features from the proposals to classify objects and refine bounding boxes. It uses a multi-task loss for classification and regression and separate bounding box regressors per class.

One-stage detectors directly output object locations and classes in one pass, allowing faster inference but reduced accuracy. YOLO is a seminal one-stage object detector using a single CNN to concurrently predict class probabilities and bounding boxes [^63]. YOLO divides the input image into an $S\times S$ grid. Each grid cell predicts $B$ boxes with objectness confidence scores. Predictions comprise box center coordinates, dimensions, and class. While exploiting contextual information, YOLO’s grid approach can miss small objects. YOLOv2 introduced anchor boxes and multi-scale training to address this [^64]. YOLOv3 incorporated a deeper architecture and multi-scale predictions to boost accuracy while maintaining real-time performance [^65]. YOLOv4 optimized speed and accuracy via techniques like weighted residual connections, cross-stage connections, normalization, and self-adversarial training [^66]. Open-source YOLOv5 refined efficiency and usability [^67]. YOLOv6 adopted an anchor-free design optimized for industrial use cases [^68]. YOLOv7 pushed accuracy further, surpassing prior detectors across FPS targets without pre-trained backbones, via innovations in self-supervised learning, model design, and enhancements [^69]. Most recently, YOLOv8 introduced anchor-free prediction with fewer boxes and faster NMS, achieving state-of-the-art accuracy by disabling aggressive augmentation during late training [^70].

SSD enhances YOLO by utilizing anchor boxes tailored to diverse object shapes and performing detection across multiple network layers to achieve robustness across varying scales [^71]. Smaller feature maps in earlier layers focus on detecting larger objects, while higher resolution layers target smaller objects. This multi-scale design contrasts with YOLO’s single output scale, enabling SSD to capture a wide range of object sizes. Predictions from all layers are aggregated and refined to produce unified detections across scales. SSD’s multi-feature map architecture has influenced other single-stage detectors for handling scale variation through its effectiveness at detecting objects across a spectrum of sizes.

### 3.1 Video and 3D Object Detection

Video object detection involves identifying and localizing objects across consecutive frames in video sequences, presenting distinct challenges compared to static image detection, including motion blur, defocus, complex object motions, and viewpoint variations over time [^72]. Effective techniques require specialized modeling of temporal information propagation and consistent detection across frames to cope with video-specific complexities [^73]. Key techniques for video object detection harness temporal context to enhance per-frame detection accuracy. One approach is temporal aggregation which propagates detections across frames using optical flow [^74] [^75] or aligns and averages neighboring frame features [^76] [^77]. This provides useful contextual clues to help resolve detection ambiguities [^76] [^78]. Spatial aggregation is another strategy which applies larger receptive fields or coarse pooling to frames farther from the reference frame, organizing multi-scale features and improving inter-frame complementarity. Flow-guided aggregation employs optical flow correspondence [^79] to enable flexible multi-frame fusion at earlier layers before final detection [^80] [^81], although computational costs and handling large motions remain challenges [^79] [^82]. Recently, transformer-based architectures like TransVOD [^83] and DETR [^84] have shown promising results by enabling effective modeling of long-range dependencies through self-attention, achieving state-of-the-art accuracy [^85] [^86] [^87] [^88]. Combining transformers and multi-frame feature aggregation is also being explored to jointly leverage temporal context, inter-frame correlations, and self-attention [^89] [^80]. However, balancing efficiency remains an open issue.

Specialized techniques have emerged to address the unique complexities of 3D object detection. LiDAR-based 3D detection operates on raw point clouds using networks like PointNet [^90], extended by works like PointRCNN [^91], Part-A2 Net [^92], and PV-RCNN [^93] for proposal generation and refinement. Other approaches aggregate points into efficient pillars, like PointPillars [^94] and PIXOR [^95], but lose details. Advanced pillar variants like SpindleNet [^96] and CenterPoint [^97] improve representations by encoding local context more effectively. Camera-based 3D object detection such as 3DOP [^98] lifts 2D detections into 3D or estimates depth to apply LiDAR techniques, enhanced by stereo fusion as observed in Mono3D [^99], Mono3D++ [^100], and Pseudo-LiDAR [^101]. Earlier works rely heavily on priors and ground plane assumptions [^102] [^103]. Multi-sensor fusion combines LiDAR geometry and camera semantics, with robust recent approaches exploring transformer soft-attention. Early fusion integrates modalities in the network while late fusion generates proposals in one modality using the other. PointPainting [^104] and PointFusion [^105] feature learned feature fusion throughout. Key challenges include computational efficiency, maintaining geometric details, handling noise and occlusion, optimizing proposal generation, and effectively fusing multi-modal cues. Specialized techniques aim to address the unique complexities of sparse 3D data for robust detection vital for applications like autonomous driving. Further architectural and methodological details are provided in the supplementary document.

### 3.2 Few-Shot Object Detection

Few-shot object detection poses unique challenges compared to classification, demanding accurate localization from extremely scarce bounding box annotations. Popular techniques like incremental learning in two-stage detectors [^106], label smoothing in one-stage detectors [^107] [^108], specialized augmentation [^109] [^110], and transformer architectures [^111] help address these difficulties. However, core issues persist, including unreliable localization [^112] [^15], imbalance between base and novel classes [^113] [^114], complex domain shifts [^21] [^115] [^116], lack of context from limited examples [^110] [^117] [^118], and overfitting tendencies [^113] [^110]. Advanced data augmentation shows promise but faces information-theoretic constraints on synthesizing new signals from scarce data. Further innovations in areas like meta-learning, metric-based learning, context modeling, and transfer learning hold promise for advancing few-shot detection by overcoming limitations like scarce annotations, class imbalance, and domain shifts. The supplementary document provides additional details on few-shot object detection challenges, and state-of-the-art techniques.

Building on these foundations, the following sections dive deeper into applying FSL to address the unique complexities posed by video and 3D object detection.

## 4 Few-Shot Video Object Detection

In the context of video, FSL becomes especially valuable given the additional difficulty of annotating objects across multiple frames. Manually labeling objects across numerous video frames is much more laborious than for static images. Few-shot video detection techniques can help significantly reduce annotation requirements by propagating information across frames.

### 4.1 Key Strategies in Few-Shot Video Object Detection

Few-shot video object detection (FSVOD) task presents unique challenges compared to few-shot image detection, as it requires effectively modeling complex spatiotemporal variations in object appearance, scale, motion, and viewpoint across frames. To address these challenges, several key strategies have been developed in the field of FSVOD.

One common approach in FSVOD is to use a pretrained CNN backbone to extract rich spatiotemporal features from input video clips. This backbone network captures both spatial information and temporal dynamics, enabling the model to analyze object appearance and motion across frames. Additionally, modeling local object cues and global context throughout the video is important. Specialized components, such as memory modules, region proposal networks, and temporal propagation mechanisms, have been developed to enhance coherence across frames by reasoning about object trajectories and identities. Metric-based learning approaches are also commonly used in FSVOD. These approaches compare query and support embeddings to measure the similarity between objects in the video. By leveraging metric learning, FSVOD models can effectively match objects and adapt to novel classes from limited support examples. Furthermore, meta-learning focused fine-tuning strategies have been developed to enable rapid adaptation to novel classes while retaining knowledge from the base classes. This allows the FSV to quickly learn and generalize from few-shot examples. Techniques like multi-scale feature learning, relational reasoning, metric-based matching, and meta-adaptability enable FSVOD models to recognize novel object classes from scarce video examples by effectively capturing spatiotemporal information and adapting to novel classes with limited support examples.

### 4.2 Architectures for FSVOD

Recent few-shot video object detection architectures follow a two-stage approach. In the first stage, a proposal generation module creates spatiotemporal tube proposals representing object trajectories across frames. The second stage classifies these aggregated tube features by matching against the few-shot support examples to produce detection predictions. Two representative two-stage architectures are the Tube Proposal Network (TPN) [^119] and the MEGA model-based Thaw [^24], which effectively incorporate this proposal-classification framework tailored for few-shot video scenarios. The TPN architecture generates tube proposals connecting objects across frames to utilize temporal consistency. Thaw’s two-stage design aggregates both local object features and global video-level features to classify tube proposals based on comparison with the few-shot supports. These concrete implementations showcase how the two-stage approach of proposal generation followed by temporal feature aggregation and matching enables state-of-the-art few-shot detection performance on videos.

#### 4.2.1 TPN and TMN + Hybrid

Fan et al. [^119] proposed the tube proposal network (TPN) as a representative architecture for few-shot video object detection. As illustrated in Figure S1 of the supplementary document, their system comprises various components for both training and inference, including the Tube Proposal Network (TPN), Temporal Alignment Branch (TAB), and Query and Support branches. During training, the weight-shared convolutional neural network (CNN) backbone extracts spatiotemporal features from the input query video frames and support images. The backbone enables subsequent analysis and detection by learning discriminative representations.

Specifically, the query branch processes two query frames $\{I_{1},I_{2}\}$ using RoIAlign to extract query features $\{Q_{i1},Q_{i2}\}$ corresponding to each proposal $p_{i}$ of instance $i$. These instance-specific query features capture relevant visual cues about the object. Concurrently, the support branch extracts support features $S$ from the ground-truth boxes in the support images to serve as references during matching. Crucially, the Temporal Alignment Branch (TAB) aligns the query features temporally to ensure synchronization across frames before comparison with the support features. Matching occurs in the tube matching network (TMN), which utilizes tube-level features aggregated over time by the TPN via inter-frame regression and identification scoring. This establishes temporal consistency in detections. The matching network, called the Multi-Relation Network, compares the aggregated query features $Q=\frac{Q_{i1}+Q_{i2}}{2}$ and support features $S$ by computing their distance, measuring similarity between them. Additionally, a multi-relation head and contrastive training, inspired by FSOD, improve the discriminability of the matching. This allows effective classification of the query features based on their affinity to the supports. By integrating and jointly optimizing the TPN and TMN end-to-end, the model can handle the challenges of high dynamism and diversity in video object detection.

#### 4.2.2 Thaw

A different architecture was created by [^24] for few-shot video object detection, as shown in Figure S2 of the supplementary document. This framework is called Thaw and consists of several essential steps:

1. Pretraining Phase: The first stage of the proposed method involves pretraining a video object detector on a base dataset containing an abundant number of videos per class $V_{\text{base}}=\{V^{i}_{\text{base}}|i=1,...,N\}$, where $N$ is the number of videos. The MEGA model [^120] is utilized as the video object detector because it can efficiently aggregate both local and global spatiotemporal information across frames in a video.
	Specifically, for each key frame $I_{k}$ in a given video, MEGA generates multiple feature representations. First, a local feature pool $L$ is extracted from region proposals in $I_{k}$. Next, a global feature pool $G$ is obtained by applying a convolutional network backbone on the entire frame $I_{k}$. An aggregated local feature pool $L_{g}$ is then formed which condenses information from $L$ across multiple neighboring key frames. Additionally, an enhanced local feature pool $L_{m}$ is generated by integrating $L$ and $G$. Finally, a memory module $M$ aligns features from region proposals using ROI alignment.
	These various features are concatenated into an enhanced feature representation $f_{e}(I_{k})$ for each key frame $I_{k}$:
	$$
	f_{e}(I_{k})={f_{e}(I_{k})_{1},f_{e}(I_{k})_{2},...,f_{e}(I_{k})_{Q}}
	$$
	where $Q$ is the dimensionality of the concatenated feature vector. This enhanced feature representation $f_{e}(I_{k})$ is then utilized in MEGA’s region proposal network for object classification and localization in the video. The multi-level feature extraction provides both local object details and global spatiotemporal video context to enable effective few-shot detection.
2. Adaptation Phase: Subsequent to the pretraining, the model is adapted to novel classes using a few-shot dataset with limited videos per class $V_{\text{novel}}$. A cosine classifier is introduced in the detection head:
	$$
	S(W,x)=\left[\cos(\theta(w_{1},x)),\dots,\cos(\theta(w_{N+M},x))\right]^{\prime}
	$$
	where $W$ contains class weight vectors $w_{i}$ and $S(W,x)$ measures similarity between features $x:=f_{e}(I_{t})_{l}$ (where $f_{e}$ is the feature extractor and $I_{t}$ is the input frame at time $t$) and classes. The probability for the $i^{\text{th}}$ class can then be calculated as:
	$$
	P_{i}=\frac{\exp(S(W,x)_{i})}{\sum_{c}\exp(\alpha S(W,x)_{c})}
	$$
	where $\alpha$ is a scaling factor to reduce the discrepancy between one-hot and real distributions \[1\], \[20\], \[30\].
3. Fine-tuning Phase: In the final phase, fine-tuning uses Joint (all weights updated), Freeze (only classifier updated), and Thaw (gradual unfreezing) methods:
	$$
	\text{Freeze }f_{e}(\cdot)\rightarrow\text{Unfreeze }f_{e}(\cdot)
	$$
	The Joint method fine-tunes all weights, but often leads to overfitting on small datasets. Freeze only updates the classifier while keeping the feature extractor fixed, making it suitable for FSL. Thaw gradually unfreezes the feature extractor for improved adaptation. Recent work shows Freeze attains the highest few-shot detection performance to date by preventing overfitting to the limited novel class examples during fine-tuning. While fully fine-tuning tends to overfit on scarce data, Freeze provides a simple yet effective alternative that concentrates model updates only on the task-specific classifier head during few-shot adaptation [^121].

Additionally, a balanced sampling strategy is proposed to overcome the class imbalance between novel and base classes during fine-tuning. Classes are uniformly sampled during each iteration to provide an evenly distributed gradient update. This prevents the model from overfitting to base classes and forgetting novel classes. Experiments show balanced sampling is crucial for good few-shot detection performance.

### 4.3 Losses and Training Strategies for Few-Shot Video Object Detection

Achieving effective FSL on videos requires a specialized training methodology that accounts for the unique spatiotemporal dynamics of these data. The following section discusses key training phases, regularization techniques, and video-specific training strategies that contribute to accurate and generalizable few-shot detection with limited novel class training data.

#### 4.3.1 Training Phases

A multi-phased training approach is critical to prevent overfitting generalization in few-shot video object detection. The training phases typically include:

- Pretraining Phase: In this phase, the feature extractors are pretrained exclusively on base classes to learn transferable representations. By leveraging abundant labeled data from the base classes, the models can extract high-level features that capture relevant visual patterns and semantics. This base class knowledge transfers well when adapting to novel classes, allowing the models to generalize effectively with limited labeled data.
- Adapter Fine-Tuning Phase: The task-specific components, such as matching networks or tube proposal modules, are fine-tuned on the novel classes while keeping the base class weights fixed. This approach prevents interference between the base and novel classes, as the models continue to rely on the prelearned feature representations from the pretrained feature extractors. Gradual unfreezing of later layers in the feature extractors can strike a balance between retaining generalization and increasing model capacity for the novel classes.

The multi-phased training approach allows the models to effectively utilize the knowledge acquired from the base classes while adapting to the few-shot novel classes. It helps prevent overfitting and ensures that the models can generalize well to unseen classes in the video data.

#### 4.3.2 Regularization Techniques

To further reduce overfitting during the training of few-shot video object detection models, various regularization techniques can be employed.

##### Label Smoothing

Label smoothing is a highly effective regularization technique that can significantly enhance few-shot video object detection models. By introducing small amounts of target noise, label smoothing serves to prevent models from making overconfident predictions solely based on limited video training examples, thereby improving calibration and generalizability [^122]. Recent research has actively explored the use of label smoothing in the context of few-shot video detection tasks. In the case of FSL, where only scarce labeled examples are available for novel classes, models often encounter challenges with overfitting and struggling to robustly detect new classes [^123] [^109]. Label smoothing plays a crucial role in mitigating these issues by redistributing some target probability to non-ground truth classes. This reduces the model’s reliance on memorization and fosters a more comprehensive and adaptable understanding of the data. The importance of label smoothing further amplifies in the context of few-shot video detection, where each class possesses a limited number of annotated frames, and objects may exhibit significant appearance variations across frames and viewing angles. By discouraging overconfidence, label smoothing compels models to place more emphasis on invariant class-specific features instead of relying on superficial cues. Additionally, label smoothing aids in addressing imbalanced classes [^123] commonly observed in few-shot video detection. Given that novel classes typically have far fewer examples than base classes, smoothing techniques effectively limit the model’s reliance on individual samples, which prevents biases and enables more balanced and generalizable recognition across both base and novel classes.

##### Episodic Training

Episodic training is a essential technique for improving few-shot performance in video object detection. By constructing varied few-shot task distributions, episodic training exposes the model to diverse training scenarios, enabling better generalization. This training approach organizes the model training into a series of learning problems or episodes, with each episode mimicking the FSL setting encountered during evaluation. Each episode consists of a small training set and a validation set. The model is trained on these small but varied episodes, allowing it to improve its ability to generalize to new tasks with only a few examples during testing. In the context of few-shot video object detection, episodic training has shown promise by constructing episodes that contain only a few labeled frames per video. The model is trained to detect objects in these sparse labeled videos, effectively leveraging information across frames and learning to generalize from limited annotation. Compared to fully supervised pre-training, episodic training better simulates the intended few-shot test scenario. Although originally proposed for image classification, episodic training has proven effective in improving generalization for few-shot video recognition [^24]. By exposing the model to varied few-shot episodes during training, episodic learning encourages the development of inductive biases tailored for rapid adaptation from scarce video data. Overall, constructing representative episodes is a vital technique for enhancing few-shot performance in video understanding tasks.

##### Data Augmentation

Data augmentation plays a vital role in enabling few-shot video object detection models to generalize effectively from limited labeled data. While basic augmentations such as random cropping, padding, flipping, and color transforms are commonly used [^124] [^125], more advanced techniques like mixup offer the opportunity to combine samples from different classes, thereby exposing models to a more diverse range of augmented samples during training. Dynamic Video Mixup [^126], for instance, fuses videos from different domains to enhance cross-domain generalization, while Manifold Mixup [^127] creates mixes that are robust to small shifts in the data distribution. Additionally, Hard Mixup [^128] utilizes uncertainty measures to generate challenging class combinations. These mixup approaches contribute to increased diversity and improved generalization capabilities. Furthermore, beyond mixup, additional advanced augmentation techniques have proven to be effective. For instance, CutMix [^129] blends object patches between videos to introduce variations in context, while CutBlur [^130] incorporates Gaussian blurring to simulate motion and occlusion. Temporal crop and paste [^131] perturbs object motion and timing by cropping object tubes and inserting them at different temporal locations in the video. Temporal jittering alters frame rates, improving robustness to variable frame rates during inference. Spatial jittering applies transformations such as translation, flipping, rotation, and scaling to individual video frames, bolstering robustness to spatial variations. Video mixup combines full clips from different domains, which is especially useful for cross-domain few-shot detection [^132]. Finally, context augmentation involves pasting detected objects from the same classes into new background scenes and contexts, enhancing context invariance for the model [^133].

#### 4.3.3 Video-Specific Training Techniques

To enable more effective FSL in video object detection, several strategies leverage the unique spatiotemporal characteristics of video data. These techniques aim to improve detection consistency, reduce noise, and exploit temporal context.

##### Inter-Frame Propagation

In the context of few-shot video object detection, inter-frame propagation is a technique that enhances detection consistency and incorporates valuable temporal context by propagating object detections or features across frames. Recent works have proposed several advancements in inter-frame propagation techniques. Chakravarthy et al. [^134] proposed a method that utilizes inter-frame attentions for temporally stable video instance segmentation. By refocusing on missing objects using box predictions from neighboring frames, their method overcomes missing detections and improves temporal stability. In another work, Xu et al. [^135] proposed a method called Temporal Consistency learning Network (TCNet) for video super-resolution that employs fine-tuned flow estimation and temporal self-alignment modules for motion compensation, demonstrating the effectiveness of inter-frame propagation. Wang et al. [^136] introduced the Dynamic Warping Network (DWNet) that adaptively warps inter-frame features to improve semantic video segmentation performance, further evidencing the utility of propagation. Zhang et al. [^137] combined weighted optical flow prediction with an attention model for object tracking, showing inter-frame propagation’s usefulness in tracking. Finally, Lin et al. [^138] proposed an unsupervised flow-aligned sequence-to-sequence learning approach for video restoration using optical flow for motion compensation. Together, these advancements demonstrate inter-frame propagation’s effectiveness for various video tasks like few-shot detection, instance segmentation, super-resolution, semantic segmentation, tracking, and restoration. By propagating information between frames, consistency, context, and performance can be enhanced despite limited supervision.

##### Temporal Feature Aggregation

Aggregating features over tubes or temporal segments allows models to capture rich contextual information and exploit temporal dynamics of objects. Strategies like temporal average/max pooling [^139], LSTMs [^140], and attention mechanisms [^141] [^142] [^143] provide aggregation across clips or tubelets. These enable models to learn robust spatiotemporal representations [^144] [^145] [^146], facilitating few-shot detection without requiring extensive annotation [^141]. Architectures can be designed to enable aggregation at multiple levels [^140] [^147], from early convolutional features to late detection features [^148] [^149]. Average pooling reduces the effect of noisy features but can lose prominent features like edges. In contrast, max pooling extracts pronounced features but may overfit more easily [^148]. To balance these tradeoffs, mixed pooling combines max and average pooling. More advanced pooling explores higher order statistics like skewness and kurtosis [^150]. Tree pooling and stochastic pooling add randomness to avoid overfitting [^151]. Spatial pyramid pooling adapts pooling to spatial structure. Another notable approach is the use of spatiotemporal graph networks, which make use of graph convolutions and recurrent neural networks (RNNs) to incorporate both spatial and temporal information [^146].

##### Meta-learning

Recent advancements in few-shot video detection have focused on leveraging meta-learning to enable models to quickly adapt to novel classes with only a few examples. Specifically, meta-learning can take advantage of the additional spatial-temporal information present in videos compared to static images [^152] [^24]. One approach is to pretrain a video object detector on a base dataset by aggregating local and global information across frames using techniques like MEGA [^24], and then fine-tune it on the novel classes. The model learns to effectively extract spatial-temporal features from the base classes that transfer well to novel classes. Another promising direction is to integrate spatial reasoning into the few-shot video detection framework [^153]. For example, STEm-Seg [^153] encodes relative spatial contexts between tubelet proposals in a graph neural network. This allows the model to understand object interactions and scene layout to generalize better. In addition, recent work has explored going beyond individual frames to use information from surrounding frames when adapting to novel classes [^152] [^24]. For example, TPN [^24] aggregates RoI features from a local temporal window centered on each query frame during few-shot matching. This provides useful cues from motion and temporal consistency to recognize novel objects with scarce examples. However, there remain significant challenges in scaling up to longer videos and more complex scenes. Further advancements in meta-learning will help enable few-shot video detection for real-world applications.

Various specialized loss functions and training strategies have been developed to enable effective few-shot learning on videos. To provide readers with an overview of these techniques, we include the following comparison table 1 summarizing the key methods discussed in this survey. This table highlights how contemporary approaches tailor their optimization methodology to account for challenges like class imbalance and limited supervision.

Table 1: Comparison of loss functions, training strategies, and class imbalance techniques for few-shot video object detection (including generic video object detection techniques)

| Method | Loss Function | Aux. Losses | Training Strategy | Class Imbalance Tech. |
| --- | --- | --- | --- | --- |
| TPN \[30\] | Cross-entropy | Segmentation loss | Episodic training | \- |
| Thaw \[178\] | Cosine similarity | \- | Balanced sampling | Balanced sampling |
| FSCE \[141\] | Online hard mining | \- | Adam optimizer | Focuses on hard examples |
| TCL \[27\] | Consistency loss | \- | Information propagation | Inter-frame propagation |
| DSLA \[140\] | Smooth L1 | \- | SGD optimizer | Label smoothing |
| FSOD \[156\] | Online hard mining | Attribute prediction loss | Class re-weighting | Feature re-weighting |
| MetaYOLO \[149\] | MSE | \- | Meta-learning | Over-sampling |

Table 1 compares several major few-shot video object detection methods in terms of their loss functions, auxiliary losses, training strategies, and techniques to handle class imbalance. The cross-entropy, cosine similarity, online hard example mining, consistency, and smooth L1 losses are common choices adapted to the few-shot setting. Auxiliary losses like segmentation help improve feature learning. Strategies like episodic training, balanced sampling, label smoothing, and information propagation aim to prevent overfitting and make use of the spatiotemporal structure of videos. Re-weighting and over-sampling help mitigate issues with class imbalance. As shown in Table 1, specialized loss formulations and training methodologies are instrumental for achieving effective few-shot learning for video object detection. The multi-faceted approach of combining tailored losses, auxiliary tasks, regularization techniques, and class re-balancing enables models to generalize from scarce training data across imbalanced classes. Advancing these optimization and learning strategies remains an active research area for improving few-shot video object detection.

## 5 Few-Shot 3D Object Detection

Few-shot 3D object detection (FS3DOD) stands at the intersection of 3D computer vision and FSL, aiming to detect objects in 3D space with minimal labeled examples. The challenge is intensified due to the inherent complexities of 3D data, such as point clouds from LiDAR or depth sensors, which are inherently sparse, unordered, and lack the rich texture information available in 2D images.

### 5.1 Key Themes in FS3DOD

One recurring theme across these algorithms is the emphasis on leveraging both geometric and semantic information. Many FS3DOD approaches build upon PointNet-based architectures, which are adept at handling raw point clouds, extracting hierarchical features and preserving the spatial structure of the data. These architectures often employ attention mechanisms, prototype matching, and other techniques to enhance the discriminative power of the learned embeddings.

Furthermore, there’s a trend towards hybrid models that synergize both metric-based and optimization-based FSL strategies. For instance, some methods use prototype-based approaches where class representations are computed as the mean of feature embeddings. These prototypes are then used to classify query points based on their similarity, often measured through cosine distances or other distance metrics.

Another significant insight is the challenge of data imbalance in the few-shot setting. Several methods introduce novel loss functions or sampling strategies to handle the disparity between base classes with abundant data and novel classes with limited examples. These strategies aim to prevent the model from being overwhelmingly biased towards the base classes.

Additionally, the role of auxiliary tasks, such as segmentation or attribute prediction, is evident in many FS3DOD algorithms. By training on these auxiliary tasks alongside the primary detection task, models can learn richer and more generalized feature representations.

The fusion of 2D and 3D information is also a promising direction. Some algorithms project 3D point clouds into 2D space, extract features using 2D CNNs, and then lift these features back into 3D space for detection. This multi-modal approach aims to capitalize the strengths of both 2D images and 3D point clouds.

In summary, FS3DOD represents a confluence of techniques designed to address the unique challenges posed by 3D data and the scarcity of labeled examples. As the demand for 3D object detection in applications like autonomous driving, robotics, and augmented reality continues to grow, the innovations in FS3DOD provide a promising pathway to achieve robust performance with minimal annotations.

### 5.2 Architectures

Most FS3DOD build on top of standard 3D convolutional backbones like VoxelNet [^25] or PointNet++ [^26] to extract features from raw point clouds or voxel grids. The extracted features are then fed into metric learning modules for comparison against few-shot prototype features to produce classifications. Some prominent FS3DOD architectures that follow this overall pipeline are described below for better understanding.

#### 5.2.1 Prototypical VoteNet for FS3DOD

Prototypical VoteNet is a novel methodology introduced to address the challenges inherent in 3D point cloud object detection [^154]. Traditional approaches in this domain heavily depend on a vast amount of labeled training data. However, acquiring these labels is both expensive and time-intensive. This is particularly challenging when considering the detection of objects from novel categories, for which only a limited number of labeled examples might be available. To circumvent these challenges, researchers proposed the Prototypical VoteNet [^154], which aims to efficiently detect and localize instances even with minimal training data.

The core innovation of Prototypical VoteNet lies in its introduction of two distinct modules namely Prototypical Vote Module (PVM) and Prototypical Head Module (PHM) as shown in Figure S3 of the supplementary document.

Prototypical Vote Module (PVM) The PVM is designed to take advantage of shared 3D basic geometric structures among object categories. Recognizing that these structures can be class-agnostic, the PVM focuses on refining the local features of novel categories based on these commonalities. It consists of the following key components:

- Memory Bank Construction: A class-agnostic memory bank $G=\{g_{k}\}_{k=1}^{K}$ is constructed, containing geometric prototypes. These prototypes are learned from the rich base categories.
- Prototype Update Mechanism: Initialized randomly, the prototypes undergo iterative updates during the training process. The formula for this is given by:
	$$
	g_{k}\leftarrow\gamma\cdot g_{k}+(1-\gamma)f_{k}
	$$
	where $f_{k}$ is the average of point features $\{f_{m}\}_{k}$ assigned to prototype $k$ and $\gamma$ is a momentum term.
- Feature Refinement: PVM employs a multi-head cross-attention module to enhance the input point features using the established prototypes. The refinement formula is:
	$$
	f_{j}\leftarrow\text{Cross\_Att}(f_{j},\{g_{k}\})=\sum_{h=1}^{H}W_{h}\left(%
	\sum_{k=1}^{K}A_{h,j,k}\cdot V_{h}g_{k}\right)
	$$
	Where $f_{j}$ represents the point feature, $g_{k}$ signifies the prototype, and $A_{h,j,k}$ is the attention weight that measures the similarity between the query $f_{j}$ and key $g_{k}$.
- Vote Layer: The refined features are subsequently used by the Vote Layer, which predicts point offsets and features.

Prototypical Head Module (PHM) The Prototypical Head Module (PHM) plays a crucial role in few-shot 3D detection by utilizing class-specific prototypes to refine object features. These prototypes, denoted as $E=\{e_{r}\}_{r=1}^{R}$, are extracted from a support set, where $R$ indicates the total number of class prototypes available.

The primary purpose of the PHM is to enhance object features by leveraging class-specific prototypes. To achieve this, the PHM employs a two-step process. First, it extracts the prototype for a specific class $e_{r}$ by averaging the instance features from support samples of that class. This class prototype captures the representative characteristics of the objects belonging to that class. Next, the PHM utilizes a multi-head cross-attention module, similar to the Prototypical VoteNet’s (PVM) approach, to refine the object features. The refinement is accomplished by applying the cross-attention mechanism as follows:

$$
f_{o,t}\leftarrow\text{Cross\_Att}(f_{o,t},e_{r})
$$

In this equation, $f_{o,t}$ represents the object feature, and $e_{r}$ denotes the class prototype. By combining the object feature with the class-specific prototype, the PHM enhances the discriminative power of the object representation.

After feature refinement, the enhanced features are passed to the prediction layer, which is responsible for the actual detection process. The prediction layer utilizes the refined features to make accurate predictions for object presence and location. To train the PHM module, an episodic training approach is employed. This training strategy is designed to learn a distribution of few-shot tasks. By exposing the PHM to various few-shot scenarios during training, it can effectively generalize and adapt to new objects with limited annotated examples.

In summary, the Prototypical Head Module (PHM) in Prototypical VoteNet takes a dual-pronged approach to few-shot 3D detection. While the PVM refines local features through geometric prototypes, the PHM focuses on enhancing global features by utilizing class-specific prototypes. This combination enables the model to effectively handle the challenges of few-shot 3D object detection, such as sparsity and lack of texture, by leveraging both local and global information.

#### 5.2.2 Generalized Few-Shot 3D Object Detection

Figure S4 shows the overall framework for generalized few-shot 3D object detection [^155]. The input 3D point cloud first goes through a 3D feature extractor based on VoxelNet to generate feature embeddings. These features are then processed by a region proposal network (RPN) for further feature encoding. The features then pass through a shared convolutional layer, whose outputs are fed into multiple prediction heads for final detection.

The framework adopts a two-stage training approach. In the base training stage, it trains on the base classes with abundant data. In the few-shot fine-tuning stage, it freezes the base network and adds incremental branches for novel classes, each with a small training set. Specifically, each incremental branch for a novel class contains a convolution layer, a batch normalization layer, and a ReLU activation layer. These branches share the feature embeddings from the earlier layers, but make separate predictions for their respective classes.

During fine-tuning, the loss function is a weighted combination of a sample adaptive balance (SAB) loss $L_{\text{SAB}}$ for classification and an $L_{1}$ loss for regression:

$$
L=L_{\text{SAB}}+\lambda L_{\text{regression}}
$$

where $\lambda$ balances the two loss terms.

The SAB loss handles the imbalance between foreground objects and background regions, and focuses on hard negative samples that have high confidence scores. It dynamically adjusts weights $w_{\text{pos}},w_{\text{neg}},w_{\text{hn}}$ for positive, negative, and hard negative samples respectively based on the number of samples.

Figure S5 shows the incremental branches added for novel classes. Each novel class gets its own branch that shares an earlier convolutional layer with the base class branches. This avoids interference between base and novel classes during fine-tuning. Only the novel class branches are updated during the second training stage.

#### 5.2.3 MetaDet3D

MetaDet3D is a meta-learning based framework for few-shot 3D object detection introduced by [^156]. It takes a novel approach of using meta-learning to derive class-specific knowledge from the few-shot support examples, which is then used to guide the downstream 3D object detector. Specifically, MetaDet3D comprises two essential components that operate in collaboration, as shown in Figure S6 and described below:

- 3D Meta-Detector: The first is a lightweight 3D Meta-Detector implemented as a class-specific reweighting module $M$. It takes as input the few support examples available for each novel class. A PointNet++ backbone extracts features from these support points. The meta-detector then condenses these features into a compact class-specific reweighting vector $z_{n}$ for each novel class $n$. This reweighting vector encapsulates class-specific knowledge learned from the scarce support examples:
	$$
	z_{n}=M(\text{support samples})
	$$
- 3D Object Detector: The second component is the primary 3D Object Detector, which uses the reweighting vectors to guide its prediction process. This component consists of three sub-components: point feature extraction, guided voting and clustering, and guided object proposal.
	- Point Feature Extraction: The PointNet++ backbone $F$ is used to extract point features $[x,f]$ from the query point cloud:
		$$
		[x,f]=F(\text{query point cloud})
		$$
	- Guided Voting and Clustering: Channel-wise multiplication is applied between the extracted features $f$ and the class-specific reweighting vector $z_{n}$ to obtain the modified features $f^{\prime}$. These modified features $f^{\prime}$ are then used in a voting module $V$ to generate object candidates $[y,g]$:
		$$
		\displaystyle f^{\prime}
		$$
		 
		$$
		\displaystyle=f\odot z_{n}\ [y,g]
		$$
		 
		$$
		\displaystyle=V(f^{\prime})
		$$
	- Guided Object Proposal: The PointNet $H$ is applied to each cluster $C$ to extract features. These features are then reweighted with the class-specific reweighting vector $z_{n}$ and passed through an MLP to predict bounding boxes and class scores:
		$$
		\text{predictions}=P(H(C\odot z_{n}))
		$$

By learning to generate class-specific reweighting vectors from the few-shot examples, MetaDet3D provides an elegant way to transfer knowledge from scarce support data to guide the downstream object detector. The model is trained end-to-end, first on base classes and then base+novel classes. Experiments demonstrate MetaDet3D outperforming prior state-of-the-art techniques for few-shot 3D detection by effectively utilizing the reweighting vectors for guidance.

#### 5.2.4 Neural Graph Matching (NGM) Networks

Introduced by Michelle Guo et al. in their ECCV 2018 paper [^157], the Neural Graph Matching (NGM) Networks present an innovative approach for addressing the FSL challenges in 3D action recognition. The fundamental idea behind NGM is to encode videos into graph structures, where individual nodes represent video frames and edges capture the temporal relations between them.

For a given video $V$ with $T$ frames, the graph $G(V)$ is constructed in the following manner:

- Each frame $f_{t}$ is embedded using a neural network $f$, yielding $f(f_{t})$, which subsequently serves as a node in the graph.
- Edges are formed based on pairwise relations between nodes.

To determine the similarity between a support set $S$ and a query $Q$, a graph matching score $M(G(S),G(Q))$ is computed. This score is derived by comparing nodes ($v$) and edges ($e$) of the two graphs. Specifically:

- Node matching is given by $m_{v}(v_{i}^{S},v_{j}^{Q})=\text{cosine}(v_{i}^{S},v_{j}^{Q})$.
- Edge matching is defined as $m_{e}(e_{ij}^{S},e_{kl}^{Q})=\text{cosine}(e_{ij}^{S},e_{kl}^{Q})$.
- The overall graph matching score is expressed as
	$$
	M(G(S),G(Q))=\sum_{i,j}m_{v}(v_{i}^{S},v_{j}^{Q})+\lambda\sum_{i,j,k,l}m_{e}(e%
	_{ij}^{S},e_{kl}^{Q})
	$$
	where $\lambda$ is a weighting parameter.

A soft assignment mechanism is employed to map nodes of $G(Q)$ to $G(S)$. This is described by:

$$
a_{ij}=\frac{\exp(m_{v}(v_{i}^{S},v_{j}^{Q}))}{\sum_{k}\exp(m_{v}(v_{i}^{S},v_%
{k}^{Q}))}
$$

Here, $a_{ij}$ represents the assignment score of node $v_{j}^{Q}$ to node $v_{i}^{S}$.

The goal is to optimize the graph matching score across all support-query pairs, aggregated over all classes, utilizing the softmax function.

The process is visually encapsulated in Figure S7, which depicts the sequence from inputting a video and deriving embeddings for each frame using a CNN, to constructing the graph representation, and finally obtaining a matching score to determine video similarity based on their graph structures.

In summary, NGM Networks employ graph representations and graph matching to enable effective FSL for 3D action recognition. Matching the structural similarity between graph representations of videos is the key idea.

#### 5.2.5 Few-shot Action Recognition

The paper by Wang et al. [^158] introduces a framework for few-shot 3D action recognition on skeletal sequences. The framework has two main components: 1) An Encoding Network (EN) to model temporal dynamics, and 2) Joint tEmporal and cAmera viewpoiNt alIgnmEnt (JEANIE) to handle varying viewpoints. Together, these components enable robust few-shot action recognition by accounting for the complexity of human actions over time and across different camera angles. The proposed approach aims to overcome challenges in understanding and classifying skeletal actions with limited training examples.

Encoding Network (EN): The EN takes as input the query and support skeleton sequences for few-shot action recognition. As a preprocessing step, it generates multiple rotated or simulated viewpoints of the query skeleton sequences. This is done by applying Euler angle rotations to generate $K\times K^{\prime}$ different views spanning a range of azimuth and altitude angles. Alternatively, simulated camera positions can be used to render the skeletons from different viewpoints, based on the geometry of stereo projections.

Each skeleton sequence, whether query or support, is divided into short temporal blocks containing $M$ frames each. This is meant to capture local short-term motion patterns. Each temporal block is passed through a simple 3-layer multilayer perceptron (MLP), consisting of fully connected layers interleaved with ReLU nonlinearities. The MLP encodes each block into a feature map of size $d\times J$, where $d$ is the feature dimension and $J$ is the number of joints in each skeleton.

The feature maps for all the temporal blocks of a sequence are then passed into a Graph Neural Network (GNN) like GCN. The GNN can model the inherent graph structure of the skeleton in each block. An optional Transformer can also be added after the GNN to further process the graph features. Finally, a fully connected layer converts the block features into a sequence feature representation, denoted as $\Psi$ for queries and $\Psi^{\prime}$ for supports. These graph-based features capture information about both short-term motions in the blocks and long-term dynamics across the sequence. They serve as input to the next key component, JEANIE, for joint temporal and viewpoint alignment between queries and supports as shown in Figure S8.

Joint tEmporal and cAmera viewpoiNt alIgnmEnt (JEANIE): JEANIE performs a joint alignment of query and support skeleton sequences in both the temporal and viewpoint dimensions. This approach is built upon soft-DTW, a differentiable counterpart of Dynamic Time Warping (DTW). However, JEANIE’s distinctiveness lies in its ability to simultaneously align simulated viewpoints.

The optimal alignment between a query sequence feature map $\Psi$ and its support $\Psi^{\prime}$ is conceptualized through a transportation plan $A$. This plan outlines the most efficient path aligning the sequences within the 4D space composed of time steps and viewpoints.

The distance between the aligned query and support sequences can be mathematically represented as:

$$
d_{\text{JEANIE}}(\Psi,\Psi^{\prime})=\text{SoftMin}_{\gamma}\langle A,D(\Psi,%
\Psi^{\prime})\rangle
$$

Here:

- $D\in\mathbb{R}^{K\times K^{\prime}\times\tau\times\tau^{\prime}}$ is the distance matrix containing distances $d_{\text{base}}(\psi_{m,k,k^{\prime}},\psi^{\prime}_{n})$ between all query blocks $\psi$ across all $K\times K^{\prime}$ viewpoints and support blocks $\psi^{\prime}$.
- $A$ signifies the optimal transportation plan derived by JEANIE that aligns the query and support in the combined temporal-viewpoint space.
- SoftMin denotes the soft minimum operation.

Consequently, JEANIE finds an optimal smooth path aligning the sequences in time and viewpoint, without sudden jumps between distant viewpoints or temporal locations.

The model is trained end-to-end by minimizing $d_{\text{JEANIE}}$ between query and support sequences belonging to the same class, and maximizing the distance between sequences from different classes. This aligns same-class sequences while pushing apart sequences from different classes in the joint temporal-viewpoint space.

### 5.3 Losses and Training Strategies

Most few-shot 3D detectors build on top of standard 3D convolutional backbones like VoxelNet [^25] or PointNet++ [^26] to extract features from raw point clouds or voxel grids. The extracted features are then fed into metric learning modules for comparison against few-shot prototype features to produce classifications. However, specialized losses and training strategies are required to enable effective FSL on top of these standard backbones.

For example, Liu et al. (2023) [^155] proposed adding incremental classifier branches tailored for each novel class alongside the base class branches. This avoids interference between the highly imbalanced base and novel classes within a single classifier. An adaptive loss function called Sample Adaptive Balance (SAB) loss helps balance the base and novel classes during fine-tuning of the novel branches. The SAB loss dynamically adjusts weights for positive, negative and hard negative samples based on their relative proportions to handle the foreground-background class imbalance.

Zhao and Qi (2022) introduced the Prototypical VoteNet [^154], which trains the Prototypical Head Module (PHM) through episodic training. Episodic training constructs varied few-shot task distributions during each iteration to improve generalization. The loss function is based on optimizing distance metrics between embeddings of query samples and class-specific prototypes derived from the few-shot support examples.

MetaDet3D by Liu et al. (2022) [^156] introduces class-specific reweighting vectors that are learned from the few-shot support examples using a meta-learning module. These reweighting vectors act as conditioning inputs to guide the downstream 3D object detector. The model is trained end-to-end using base class samples first, followed by base+novel class samples.

For the task of few-shot action recognition on 3D skeletal sequences, Wang et al. [^159] proposed a framework consisting of an EN and a JEANIE module. The model is trained end-to-end to optimize the JEANIE transportation plan which aligns sequences in both time and viewpoint space. The loss function aims to minimize the aligned distance between query and support sequences from the same class, while maximizing the distance between sequences from different classes.

Guo et al.’s Neural Graph Matching (NGM) Networks [^157] also adopt an end-to-end training approach based on graph matching for few-shot action recognition. The overall training loss optimizes the graph matching scores across all support-query pairs from the same class using a softmax function. This loss aims to improve structural similarity between same-class graph pairs while pushing apart different classes.

In summary, key training strategies and losses for few-shot 3D detection and action recognition include: 1) Episodic training for better generalization; 2) Adaptive losses to handle class imbalance; 3) Separate prediction heads for base and novel classes; 4) Meta-learning modules to learn conditioning vectors; 5) Two-stage training process of base then base+novel classes; 6) Graph matching losses for sequence alignment; 7) Distance metrics between prototypes and embeddings. By tailoring the methodology and losses for few-shot scenarios, performance can be enhanced for 3D tasks despite scarce novel class data. However, developing universal principles remains an open challenge.

To provide an accessible overview of the key techniques discussed in this section, Table 2 and 3 summarizes and compares prominent few-shot 3D detection approaches in terms of their architectures, loss functions, and training strategies. The use of this concise tabular format enhances the readability of this section by distilling the core information into a visually structured guide. The table enables easier comparison between different methods at a glance. Along with the in-depth qualitative descriptions provided earlier, this summary table aims to equip readers with a comprehensive understanding of the state-of-the-art and promising future directions in few-shot 3D object detection research.

Table 2: Comparison of few-shot learning strategies for 3D object detection and action recognition

| Method | Modality | Backbone | Loss Function | Aux. Task | Training Strategy |
| --- | --- | --- | --- | --- | --- |
| Frustum VoxNet \[10\] | RGB-D | VoxelNet | Smooth L1 | Depth estimation | Two-stage fine-tuning |
| PV-RCNN \[136\] | Point cloud | PointNet++ | Softmax | Point segmentation | Episodic training |
| Part-A2 Net \[135\] | Point cloud | PointNet++ | Sample Adaptive Balance | Part segmentation | Incremental branches |
| STEM-Seg \[66\] | RGB-D | STEM | Cross-entropy | Segmentation | Episodic training |
| FSOD \[156\] | RGB | VGG-16 | Online hard example mining | Attribute prediction | Class re-weighting |
| NGM Networks \[40\] | RGB | 3D-CNN | Graph matching | \- | Graph matching |

Table 3: Training strategies and loss functions for few-shot 3D learning

| Category | Strategy/Loss | Methods |
| --- | --- | --- |
| Training Strategy | Episodic training | PV-RCNN, STEM-Seg |
|  | Two-stage fine-tuning | Frustum VoxNet, FS3DOD |
|  | Incremental branches | Part-A2 Net |
|  | Graph matching | NGM Networks |
| Loss Function | Smooth L1 | Frustum VoxNet |
|  | Softmax | PV-RCNN |
|  | Sample Adaptive Balance | Part-A2 Net, FS3DOD |
|  | Online hard example mining | FSOD |
|  | Graph matching | NGM Networks |

## 6 Challenges and future scope

Despite notable progress in the field of few-shot video and 3D object detection, there are several open challenges that impede the widespread and robust deployment of these methods in real-world scenarios. Addressing these challenges is crucial for advancing the state-of-the-art and fully realizing the potential of FSL in practical applications. Below, we highlight some key problem areas that require further exploration and development.

### 6.1 Base Class Generalization

Base class generalization remains an open challenge in few-shot video and 3D object detection. The base classes provide the initial examples for models to learn feature representations that can generalize to novel classes. However, curating optimal base class data is difficult.

#### 6.1.1 Challenges

For video detection, bases lacking diversity of scenes, motion patterns, and viewing angles hinder generalization [^160]. Insufficient variability in appearance, scale, and occlusion makes robust learning infeasible. For 3D detection, bases need diverse object shapes, sizes, poses, and spatial arrangements to enable generalization. Limited sensor viewpoints, occlusion patterns, and point densities also constrain learning. Appropriate granularity of annotations is required to distinguish between fine-grained classes without incurring excessive labeling effort. Furthermore, video and 3D data have unique attributes requiring specialized inductive biases. Models struggle to generalize well if bases lack diversity, balance, domain-specific considerations, and efficient labeling [^161].

#### 6.1.2 Future scopes

Several promising directions can be pursued to enhance base class generalization. One approach is to explore advanced augmentation techniques, such as class mixing, which expose models to richer variations and improve their ability to generalize [^162]. Additionally, incorporating contrastive losses and self-supervision methods can facilitate the learning of robust representations. By encouraging models to identify and differentiate between similar and dissimilar examples, these techniques promote better generalization to unseen objects. Furthermore, the utilization of semi-supervised learning can provide additional data diversity, leveraging both labeled and unlabeled examples to improve the model’s ability to generalize. Another avenue worth exploring is the design of specialized model architectures and the application of transfer learning, which can effectively transfer knowledge from pre-trained models to boost generalization performance. By leveraging prior knowledge and adapting it to new tasks, these strategies contribute to improved base class generalization

### 6.2 Class Imbalance

#### 6.2.1 Challenges

Class imbalance poses significant challenges in few-shot video and 3D detection. Real-world data exhibits long-tail distributions, with many examples for common ”head” classes but limited data for rare ”tail” classes [^115] [^117]. This imbalance between frequent base classes and scarce novel classes impedes few-shot detection performance [^163]. For video detection, tail classes lack sufficient labeled examples to model appearance variations over time. Spatial context also becomes ambiguous with few examples. In 3D detection, rare classes have insufficient point annotations to learn robust shape representations from sparse views. Extensive occlusion and partial observations further compound the problem. Without strategies to address imbalance, few-shot models struggle to detect tail classes, instead focusing on more frequent heads. Overall, class imbalance remains an open challenge requiring further research.

#### 6.2.2 Future scopes

Several promising research directions have the potential to address the pressing challenge of class imbalance in few-shot video and 3D object detection. Advanced sampling techniques could be explored that emphasize selection of rare classes during training to prevent model bias towards frequent classes [^24]. Multi-scale refinement approaches focusing on hard examples from tail classes may reduce the neglect of scarce classes [^164]. The design of balanced loss functions that weight classes inversely proportional to their frequency merits investigation, in order to avoid overfitting to dominant head classes [^165]. Imaginative data augmentation techniques such as mixing tail class examples could prove useful for synthetically increasing the volume of limited tail class data [^166]. Transfer learning from datasets exhibiting more balanced class distributions could provide richer examples of tail classes to compensate for their scarcity in the target datasets [^110]. In conclusion, this combination of targeted sampling strategies, loss formulations, augmentation approaches, and transfer learning techniques appears promising to address the key challenge of class imbalance. They may empower few-shot video and 3D object detection models to improve recognition of under-represented tail classes within real-world long-tail visual distributions. However, extensive research is still required to develop robust and universal solutions.

### 6.3 Training Regularization

#### 6.3.1 Challenges

Regularization techniques such as weight decay, dropout, and augmentation are crucial for preventing overfitting and ensuring the success of few-shot video and 3D detection due to the scarcity of data. However, finding the right balance between underfitting and overfitting can be challenging when working with limited examples [^24]. Complex neural networks tend to easily overfit small training sets (1, 11). In the case of video detection, overfitting leads to difficulties in adapting to varying viewpoints, occlusion, and motion patterns across frames when there are few examples available [^119]. Similarly, in 3D detection, models tend to overfit to specific partial observations, sparse points, and occlusion configurations [^24]. Standard regularization techniques designed for fully supervised settings often prove inadequate for few-shot scenarios, necessitating the development of more principled, task-specific methods to mitigate overfitting given the extremely limited training data [^110]. Advanced approaches such as meta-regularization show promise for few-shot tasks but require further research [^119].

#### 6.3.2 Future scopes

Several promising research avenues may help advance regularization for few-shot video and 3D detection. While techniques like weight decay, dropout, and augmentation are widely used, determining optimal hyperparameters and balances for few-shot settings remains an open question. Adaptively tuning regularization via meta-learning is a promising direction [^167]. Advancing meta-regularization schemes like meta-augmentation [^168] specially tailored for few-shot video and 3D tasks could reduce overfitting. Semi-supervised and self-supervised techniques need further adaptation to maximize the utilization of unlabeled video and 3D data more effectively [^119] [^169]. Developing simplified model architectures optimized for few-shot fine-tuning may prevent overfitting [^121]. Designing augmentations and regularizers to address video and 3D specific challenges like viewpoint and occlusion variations is another area warranting focus [^170]. Standardized benchmarks and protocols for few-shot video and 3D detection are needed to accurately evaluate progress [^171]. Analysis into optimal regularization schedules and hyperparameters spaces using generalization bounds and uniform stability can provide insights [^172].

### 6.4 Cross-Domain Transfer and Handling Domain Shift

#### 6.4.1 Challenges

Domain shift presents significant challenges for few-shot video and 3D object detection models. Enabling these models to effectively adapt across domains is crucial for real-world deployment in diverse environments. However, objects in video and 3D data can exhibit significant variations in appearance and shape due to factors like lighting, occlusions, and pose changes. Few-shot models need to handle these intra-domain variances robustly alongside inter-domain shifts [^173]. One major challenge is adapting models trained on real image datasets to novel simulated environments, which often differ substantially in appearance and distribution. The domain gap from real to synthesized data persists as a problematic issue hindering few-shot model generalization [^174]. Another key challenge is enabling few-shot video and 3D object detection models to generalize robustly when tested on distributions different from the training data. While techniques like domain adaptation and transfer learning have shown promise, more research is needed into specialized approaches tailored for few-shot video and 3D contexts [^173] [^175] [^176].

#### 6.4.2 Future Scopes

One approach to address the domain gap is to use data synthesis methods, such as the Cross-Domain CutMix method, which pastes parts of the target image onto the source image and aligns the pasted region using the object bounding box information [^174]. This method has been shown to achieve higher accuracy in cases where the target domain differs significantly from the source domain, such as RGB images as the source and thermal infrared images as the target [^174]. Another promising strategy is to utilize FSL for better domain adaptation. Fine-tuning a 3D CNN feature extractor based on a few-shot approach can improve adaptation across domains [^175]. Incorporating spatiotemporal features also helps describe subtle feature deformations and discriminate ambiguous classes across domains [^176]. Unsupervised domain adaptation (UDA) has been explored in 3D cross-domain tasks, such as the Bi3D approach, which combines active learning and UDA to solve the cross-domain 3D object detection task [^175]. This approach aims to achieve a good trade-off between high performance and low annotation cost [^177]. Data augmentation is another important aspect of FSL, as it helps expand the training samples for the novel classes [^175]. Techniques such as pseudo-labeling have emerged as crucial approaches for 3D object detection in adverse weather conditions [^178]. Further research into domain adaptation, transfer learning, data augmentation, and other techniques tailored for few-shot video and 3D problems is important to enhance cross-domain generalization and handle appearance variations with scarce training data.

### 6.5 Temporal Reasoning

#### 6.5.1 Challenges

For few-shot 3D object detection, a key challenge is effectively incorporating temporal information from consecutive point cloud frames captured in autonomous driving datasets [^156]. Naively aggregating features across frames can introduce noise [^179]. Explicitly modeling object motions and trajectories across sparse point cloud frames also remains difficult [^156]. In few-shot video object detection, a major difficulty is establishing reliable associations between sparse object detections across frames to generate consistent tubes [^119]. Matching object features over time is challenging with limited examples [^110]. Propagating detections via optical flow can be unreliable [^119]. Complex optimization is required for tracking-by-detection frameworks during inference [^119]. Overall, for both few-shot video and 3D detection, integrating long-range temporal contexts across frames and modeling complex motion dynamics with scarce examples is still a largely unsolved problem [^110]. More research is needed into sophisticated temporal reasoning techniques for few-shot detection [^8].

#### 6.5.2 Future scopes

For few-shot 3D detection, advanced temporal feature aggregation methods could help effectively summarize cross-frame contexts while reducing noise [^179]. Explicitly modeling object motions and trajectories by establishing cross-frame correspondences is another promising direction [^156]. Recurrent architectures may also be able to implicitly learn temporal dynamics from sequences of point cloud frames. For few-shot video detection, techniques like learned association metrics could help match object features for consistent detections across frames [^119]. Object tracking and flow propagation may also aid in linking sparse detections over time. Exploring recurrent network architectures tailored for few-shot video detection could be impactful for capturing long-range temporal dependencies in videos. In general, directions like end-to-end learned associations, object tracking, and recurrent modeling of temporal dynamics deserve deeper exploration to advance temporal reasoning for few-shot video and 3D object detection. By effectively harnessing long-range temporal contexts, significant performance gains may be achieved with minimal supervision [^8].

### 6.6 Multimodal Fusion

#### 6.6.1 Challenges

Integrating complementary cues from different modalities, such as RGB, depth, and semantics, has the potential to enhance few-shot 3D and video object detection performance. However, achieving optimal multimodal fusion schemes and developing principled methods for reconciling heterogeneous modalities remains a challenging task. For example, recent works have proposed techniques like multi-scale feature fusion \[5\], unsupervised contrastive feature learning \[1\], and sensor fusion \[3\] for fusing visual, point cloud, and other modalities to enhance few-shot detection. However, heterogeneity, lack of annotated data, and interference between modalities make optimal fusion and adaptation difficult. Encouraging interaction between multimodal features while reducing disturbance is also essential for effective fusion. Incorporating attention mechanisms to focus models on relevant features can further improve few-shot detection accuracy. However, developing end-to-end learning frameworks that can fuse multimodal information and adapt to few-shot scenarios remains a key challenge.

#### 6.6.2 Future scopes

Promising future directions to address these multimodal fusion challenges include exploring principled fusion methods to reconcile heterogeneous modalities and using techniques like contrastive learning to enable better feature interaction. Incorporating channel attention mechanisms to focus on critical features and developing end-to-end learning frameworks to jointly optimize fusion and few-shot detection also hold promise. Additionally, leveraging unsupervised or self-supervised pretraining to extract robust multimodal representations merits investigation. By advancing fusion techniques tailored for few-shot settings, multimodal detection performance could be enhanced despite scarce annotated data across modalities. Sophisticated multimodal fusion schemes are crucial for fully realizing the potential of integrating complementary cues to improve few-shot video and 3D object detection.

### 6.7 Similarity Metrics

#### 6.7.1 Challenges

Few-shot 3D and video object detection often struggle in crowded, cluttered scenes with occlusion. Developing robust techniques to handle such complex real-world conditions remains a key challenge. Additionally, there is a need for advanced similarity metrics beyond naive distance measures to enhance generalization and discrimination capabilities. More research is required into specialized similarity functions tailored for few-shot 3D and video detection that can effectively measure visual relationships between proposals despite truncated, incomplete views and cluttered backgrounds. Designing metrics that can match objects based on limited examples while handling occlusion and complex scenery will be critical for improving few-shot detection performance in real-world video and 3D environments.

#### 6.7.2 Future Scopes

##### Specialized metric learning

Further research into learning tailored metrics for few-shot detection could improve matching with limited data. Exploring locality-sensitive hashing techniques can enable efficient similarity search. Learning task-specific functions trained jointly with detection models can provide specialized metrics based on appearance and shape. Comparing metric families like cosine or Euclidean distance may reveal optimal choices for few-shot tasks.

##### Multi-cue fusion

Fusing different visual cues like appearance, shape, motion and context into unified metrics can leverage complementary information to enhance few-shot matching. Graph-based methods are also promising for modeling relationships between support examples and queries in a shared embedding space.

##### Improving robustness

A key challenge is developing similarity functions robust to real-world conditions like occlusion, truncation, and sensor noise using scarce training data. Advancing metrics to reliably match objects under complex conditions will be vital for few-shot detection.

Overall, progress in tailored similarity metrics is crucial for advancing few-shot 3D and video object detection in cluttered, occluded environments. A structured approach exploring specialized learning, multi-cue fusion, and improving robustness holds promise.

### 6.8 Scalability and Deployment Efficiency

#### 6.8.1 Challenges

One major challenge in the field of few-shot video and 3D object detection models is ensuring scalability and efficiency during deployment. While significant advancements have been made in terms of accuracy, there is a need to optimize these methods to handle large-scale datasets and real-time applications [^110] [^155]. Specifically, some key challenges include handling both common and rare classes that are often present in real-world data like autonomous driving scenarios [^180]. Generalized few-shot detection methods need to utilize abundant data for frequent classes while adapting to rare classes with only a few examples [^155]. Reducing computational costs and latency during training and inference is another key challenge, as the complex deep learning models used in few-shot detection can be resource intensive [^181]. Managing efficient feature fusion across support and query branches for effective FSL is also difficult [^182]. Enabling multi-scale feature learning without compromising efficiency poses problems [^183]. Finally, deploying sophisticated few-shot detection models on resource-constrained edge devices remains an open challenge.

#### 6.8.2 Future Scopes

To address these challenges, some promising research directions include developing efficient model compression techniques to reduce redundancies and minimize the computational footprint of few-shot detection models without significantly impacting performance. Exploring methods like feature fusion to improve information sharing between support and query branches in an efficient manner also holds promise. Designing multi-scale attention mechanisms to selectively aggregate useful information across scales without introducing excessive costs could be beneficial [^184]. Leveraging knowledge distillation and model pruning strategies to create lightweight few-shot detection models suitable for edge devices is another potential avenue [^185]. Adapting generalized few-shot detection formulations that can handle both common and rare classes in a scalable way merits investigation [^155]. Applying automated machine learning to find optimal architectures and hyperparameters for efficient few-shot detection is also worth exploring [^186].

### 6.9 Interpretable and Explainable Few-Shot Learning

#### 6.9.1 Challenges

While few-shot video and 3D object detection models have achieved impressive results, interpreting and explaining their predictions remains challenging, especially given the limited data available. Key issues include understanding how models generalize to novel classes from scarce examples [^123], since blackbox models offer limited insight into their reasoning processes. Diagnosing failure modes and biases learned from small datasets is difficult, as models may latch onto dataset quirks. Explaining model predictions to establish trustworthiness is also critical for real-world deployment but lacks justification. Analyzing knowledge transfer across domains is not well understood, especially regarding cross-domain shifts [^187]. Handling complex spatiotemporal dynamics in video and 3D data with interpretability methods that lag behind model advancement poses difficulties [^188]. Finally, the lack of interpretability impedes human intervention in model learning.

#### 6.9.2 Future scopes

To enhance interpretability and explainability of few-shot video and 3D detection models, some promising directions are developing attention mechanisms to highlight spatiotemporal regions critical for few-shot generalization in videos and 3D data [^189] [^190]. Leveraging prototype analysis to provide visual and geometric summaries of model knowledge for each class also holds promise [^155]. Generating saliency maps tailored for spatiotemporal data to reveal model focus areas could be beneficial [^191]. Designing interfaces for interactive visualization, debugging, and annotation guided by model explanations may be impactful. Studying latent representations and decision boundaries to diagnose model biases and overfitting is also important. Performing extensive ablation studies to identify influential components supporting few-shot generalization can provide insights. Quantifying model confidence and uncertainty to identify unreliable predictions requiring intervention is another potential direction. Finally, building modular and transparent model architectures amenable to analysis will aid in increasing interpretability [^192].

### 6.10 Benchmark Datasets and Evaluation Metrics

#### 6.10.1 Challenges

The availability of diverse, challenging benchmark datasets is crucial for effectively evaluating few-shot video and 3D detection methods. However, curating optimal benchmarks poses several difficulties. These include balancing class diversity, environment variability, annotation complexity, and data sizes. Incorporating modalities like images, videos, point clouds, and multiple sensor data is also challenging. Emulating real-world conditions such as rare classes and domain shifts poses problems. Capturing spatiotemporal dynamics and geometric intricacies is difficult. Finally, designing data sets that allow reproducible comparisons remains an open issue [^193]. Additionally, identifying evaluation metrics that can effectively measure few-shot generalization remains an open challenge, as metrics optimized for fully-supervised scenarios may not highlight few-shot capabilities.

#### 6.10.2 Future scopes

Some potential ways to advance few-shot video and 3D detection benchmarks and metrics include collaboratively constructing large-scale datasets spanning diverse environments, modalities and annotation types. Establishing standardized train-test splits designed specifically for few-shot evaluation is important [^194]. Introducing cross-domain settings to assess generalization across distributions would be beneficial. Incorporating synthetically generated data to expand variability also holds promise. Developing metrics that quantify capability to detect novel classes given limited examples is critical. Designing metrics focused on spatiotemporal and geometric reasoning under restricted supervision is also needed. Reporting performance across metrics to enable multi-faceted evaluation can provide more insights. In summary, purpose-built datasets and metrics are imperative to rigorously measure progress in few-shot video and 3D detection. Community efforts for collaborative benchmarking, coupled with principled metric design, will strengthen evaluation.

### 6.11 Combining Few-Shot Learning with Other Techniques

#### 6.11.1 Challenges

While FSL is a powerful paradigm, it faces limitations in real-world applications due to lack of data and supervision. Combining FSL with complementary techniques like transfer learning, active learning, and self-supervision can help address these challenges [^8]. However, seamlessly integrating these approaches poses difficulties. Transferring knowledge without negative transfer or catastrophic forgetting is difficult. Selecting optimal samples for labeling to maximize utility is challenging [^195]. Designing pretext tasks that extract useful features for few-shot tasks can be problematic. Developing unified frameworks to synergize different techniques in an end-to-end manner remains an open issue. Adaptively combining techniques dynamically based on few-shot problem characteristics poses problems. Finally, generalizing to diverse unseen tasks beyond lab settings is difficult.

#### 6.11.2 Future scopes

Some promising directions to advance hybrid FSL include developing principled frameworks to integrate FSL with transfer learning, active learning [^196] and self-supervision [^197]. Designing adaptive methods to dynamically adjust combinations tailored to each few-shot problem holds promise. Exploring conditional self-supervision guided by few-shot task structure is impactful. Leveraging meta-learning to learn optimal combinations of techniques also has potential. Building diverse benchmarks requiring hybrid techniques, such as cross-domain few-shot tasks, can drive progress. Studying theoretical connections between FSL and other paradigms can provide fundamental insights.

Overall, addressing these open challenges and exploring the future scope of few-shot video and 3D object detection will pave the way for more scalable, efficient, and accurate models. Additionally, developing interpretable and explainable methods and expanding FSL to unseen tasks will enable the widespread deployment and practical usage of few-shot detection methods. By advancing the field in these directions, we can unlock the full potential of FSL and further push the boundaries of video and 3D object detection applications.

### 6.12 Relating Recent Methods to Open Challenges

With rapid advancements in few-shot video and 3D object detection, it is crucial to analyze how current algorithms and innovations relate to open challenges that remain for future progress. To aid researchers in understanding the state-of-the-art capabilities and where to focus future efforts, we provide a summary table mapping key algorithms discussed in this survey to the 11 core challenges identified in Section 7.

This table serves as an informative resource for comprehending how modern techniques address or fail to address pressing research gaps. Bridging current innovations to open problems is vital for accelerating progress in few-shot detection. The table aims to highlight promising capabilities that can be built upon while revealing limitations that require novel solutions.

Table 4: Summary of how different few-shot video and 3D object detection algorithms address the 11 challenges from Section 7. The challenges are: 1) Base class generalization, 2) Class imbalance, 3) Training regularization, 4) Cross-domain transfer, 5) Temporal reasoning, 6) Multimodal fusion, 7) Similarity metrics, 8) Scalability & efficiency, 9) Interpretability & explainability, 10) Benchmark datasets & metrics, 11) Combining with other techniques.

<table><tbody><tr><td>Algorithm</td><td colspan="11">Challenges</td></tr><tr><td>TPN & TMN+ hybrid <sup><a href="#fn:119">119</a></sup></td><td>✗</td><td>✗</td><td>✗</td><td>✓</td><td>✓</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td></tr><tr><td>Thaw <sup><a href="#fn:24">24</a></sup></td><td>✗</td><td>✓</td><td>✗</td><td>✓</td><td>✓</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td></tr><tr><td>Prototypical VoteNet <sup><a href="#fn:154">154</a></sup></td><td>✓</td><td>✗</td><td>✓</td><td>✗</td><td>✗</td><td>✓</td><td>✓</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td></tr><tr><td>Generalized FS 3D OD <sup><a href="#fn:155">155</a></sup></td><td>✓</td><td>✓</td><td>✓</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td></tr><tr><td>MetaDet3D <sup><a href="#fn:156">156</a></sup></td><td>✓</td><td>✗</td><td>✓</td><td>✗</td><td>✓</td><td>✓</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td></tr><tr><td>NGM Networks <sup><a href="#fn:157">157</a></sup></td><td>✓</td><td>✗</td><td>✓</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td></tr><tr><td>FS Action Recognition <sup><a href="#fn:158">158</a></sup></td><td>✓</td><td>✗</td><td>✓</td><td>✗</td><td>✓</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td><td>✗</td></tr><tr><td>Challenges</td><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td><td>6</td><td>7</td><td>8</td><td>9</td><td>10</td><td>11</td></tr></tbody></table>

By relating algorithms to challenges, we enable informed analysis of current strengths versus areas needing improvement. Researchers can identify open problems aligned with their interests and expertise while benefiting from latest innovations.

For instance, Prototypical VoteNet demonstrates promise for base class generalization and training regularization but does not address domain shift or efficiency. MetaDet3D, however, introduces techniques for temporal reasoning and multimodal fusion.

By summarizing capabilities and limitations, the table guides investigation into impactful research directions. It encourages building upon advances made while tackling persistent challenges through novel solutions. Overall, relating algorithms to open problems provides crucial perspective into the state of few-shot detection and future pathways for exploration.

## 7 Conclusions

FSL holds immense promise in minimizing the need for extensive data annotations in video and 3D object detection. Throughout this survey, we have examined the progress made in various aspects of few-shot detection, including formulations, prototypical networks, transfer learning strategies, and domain-specific architectures, losses, and techniques.

However, despite significant advancements, several challenges remain to be addressed. Generalization across diverse datasets, handling class imbalance, incorporating effective regularization techniques, and reasoning across complex data modalities and scenes are among the key areas that demand further investigation.

Looking ahead, the field of few-shot detection can benefit from research into semi-supervised and self-supervised learning. By leveraging unlabeled data in combination with limited labeled examples, these approaches have the potential to enhance FSL performance. Additionally, the development of more flexible feature representations and stronger inductive biases can further improve the adaptability and generalization capabilities of few-shot detection models.

As computer vision systems continue to advance, the ability to accurately detect novel objects from just a few examples becomes increasingly critical for their ubiquitous deployment. By mitigating the reliance on large-scale annotations, FSL offers a pathway towards more efficient and scalable object detection systems. With continued innovation and exploration in the aforementioned areas, FSL can pave the way for remarkable advancements in the field and contribute to the realization of robust and adaptable computer vision systems.

Thus, we encourage researchers to further explore the potential of FSL in video and 3D object detection, striving to develop novel methodologies and techniques that push the boundaries of detection accuracy and efficiency. However, it is important to recognize the information theoretical limits on the amount of augmented ’data’ that can be manufactured from limited examples. Augmentation techniques inevitably reach a point of diminishing returns where they can no longer reliably synthesize useful training signals without exceeding the true information content of the scarce data. Beyond these information theoretical limits, augmentation methods will fail and produce spurious results. Therefore, researchers should be cognizant of these inherent bounds when developing specialized augmentation techniques tailored for few-shot learning. By doing so, we can unlock the full potential of FSL within realistic constraints and foster the widespread deployment of computer vision systems in diverse real-world applications.

## Acknowledgments

This research was supported in part by the U.S. Department of the Army – U.S. Army Corps of Engineers (USACE) under contract W912HZ-23-2-0004 and the U.S. Department. of the Navy, Naval Research Laboratory (NRL) under contract N00173-20-2-C007. The views expressed in this paper are solely those of the authors and do not necessarily reflect the views of the funding agencies.

## References

The supplementary materials provide additional details and visual overviews to support the survey paper “Few-Shot Learning in Video and 3D Object Detection: A Survey”.

The sections covered are:

1. Foundations of Few-Shot Learning: Discusses key concepts like episodic training, problem formulations, meta-learning algorithms, metric-based approaches, data augmentation, and regularization techniques for few-shot learning.
2. Foundations of Object Detection: Provides an overview of object detection methods, including two-stage and one-stage detectors, as well as video and 3D object detection approaches.
3. Few-Shot Video Object Detection: Presents example frameworks and architectures tailored for few-shot video object detection, highlighting techniques like metric learning, temporal feature aggregation, and episodic training.
4. Few-Shot 3D Object Detection: Covers specialized few-shot detection methods for 3D data such as LiDAR point clouds, using techniques like geometric prototypes, support set guidance, and incremental learning.

The supplementary materials expand on the key concepts, architectures, and methodologies discussed in the main survey paper, providing visual overviews and additional technical details to enhance understanding of few-shot learning for video and 3D object detection.

## 2 Foundations of Few-Shot Learning

Few-shot learning (FSL) has emerged as a critical area of study within the deep learning framework, addressing one of the most pressing challenges in machine learning: the need for vast amounts of labeled data. In many real-world scenarios, obtaining labeled data can be expensive, time-consuming, or even impossible. As deep learning models grow in complexity, with millions or even billions of parameters, they often require a substantial amount of data to avoid overfitting and ensure generalizability. FSL attempts to counter this limitation by recognizing new visual concepts from only a few labeled examples [^27]. FSL problems are typically formulated as a classification task, where the model is given a few labeled examples of new classes (support set) and asked to predict the labels of unseen examples from the same classes (query set) [^28]. Meta-learning algorithms are typically used to train FSL models, which learn to learn new tasks quickly by leveraging their knowledge from previous tasks [^29]. Metric-based approaches have also been shown to be effective for FSL, where a distance metric is learned to measure the similarity between examples [^30]. Transfer learning strategies can also be used to improve the performance of FSL models by pre-training them on large datasets of labeled data [^31]. This section provides an in-depth analysis of the fundamental principles of FSL. It discusses the crucial role of the support set and explores the different problem formulations in this domain. Additionally, it highlights the importance of meta-learning algorithms, the potential of metric-based approaches, and the transformative capabilities of transfer learning strategies within the context of FSL.

### 2.1 Support Set

The support set is a crucial component of the FSL paradigm which guides the model’s learning process [^32]. It is a carefully selected subset of labeled examples that represent the new visual concepts that the model aims to recognize [^7]. Given its sparse nature, the support set challenges models to extrapolate knowledge, identify patterns, and make informed decisions. In the typical N-way K-shot problem configuration, the model encounters N unique classes, each represented by K labeled examples. This scenario presents a complex challenge where models must accurately classify query samples while maintaining robustness and adaptability in the face of limited data [^33].

### 2.2 Problem Formulations in Few-Shot Learning

FSL aims to bridge the gap between data-hungry deep learning models and the reality of limited labeled data in many domains [^28]. This subsection explores the two primary problem formulations commonly encountered in FSL.

#### 2.2.1 Episodic Training

In this formulation, the model is trained on episodes that are sampled from a set of base classes [^34]. Each training episode imitates an N-way K-shot problem by sampling N classes from the base classes and selecting K examples per class to construct the support set. The model learns from these episodic simulations of few-shot tasks. Through this episodic training strategy on base classes, the model is able to learn generalizable knowledge and inductive biases that allow it to effectively adapt when presented with novel classes at test time [^35].

#### 2.2.2 Transfer Learning

Transfer learning uses knowledge learned from base classes to enable quick adaptation given only a small number of examples for novel classes unseen during training [^36]. Several effective transfer learning strategies are commonly used in FSL:

- Fine-tuning and in-context learning: Recent work has shown that fine-tuning and in-context learning are effective transfer learning techniques for few-shot learning. Fine-tuning deep neural networks can achieve competitive generalization comparable to in-context learning [^37], when controlling for model size and training data. Techniques like learning rate schedules and early stopping enable effective fine-tuning even with limited examples [^38]. However, fine-tuning often requires more examples per class than in-context learning to reach optimal performance on complex tasks [^37]. Adapter modules can facilitate highly parameter-efficient fine-tuning with a minimal number of eight examples per class. They achieve this by isolating task-specific parameters [^39]. Ensemble approaches combining fine-tuned and in-context models provide improved robustness across diverse tasks [^40]. Transfer learning from related datasets also improves out-of-domain generalization for fine-tuning by pre-training on data with similar characteristics [^41]. Careful experimental design is critical for fair comparison between techniques, controlling for factors like model scale, optimization strategy, and similarity between pre-training and novel classes. Analysis has shown in-context learning relies more on superficial biases in the examples, while fine-tuning better captures underlying concepts. However, in-context learning allows rapid adaptation without parameter updates. Combining the complementary strengths of both approaches remains an open challenge. The optimal technique likely depends on factors like amount of in-domain vs. out-of-domain data, task complexity, and cross-domain similarities [^42]. Further work is needed to develop universal principles for effectively applying fine-tuning and in-context learning under varying real-world conditions.
- Feature Extraction: In feature extraction-based transfer learning for few-shot learning, a model pre-trained on base classes is directly applied to novel class examples as a fixed feature extractor without any fine-tuning [^43]. This allows leveraging the pre-trained feature hierarchy to extract transferable representations for the novel classes using just the limited examples available. For few-shot fault diagnosis, the pre-trained model consists of attention mechanisms and convolutional neural networks to learn discriminative fault features through a multi-stage process [^44]. First, the model is pre-trained on known faults to learn base feature representations. Next, meta-transfer learning adapts the model to new faults by transferring knowledge from known faults. Finally, a lightweight classifier is meta-trained from scratch on the novel fault features extracted by the pre-trained model. Complementary base and meta transfer features can be extracted to enhance representation capabilities [^45]. The pre-trained model is adapted during meta-transfer using parameter modulation guided by known fault features to instill relevant knowledge [^44]. Prototype representations of novel faults are iteratively corrected in an unsupervised manner by aggregating information from all query samples of the same task, thereby refining the prototypes [^45].
- Classifier Re-training: Instead of fine-tuning the base model weights or training a linear classifier, a new non-linear classifier can be trained from scratch on the novel class features extracted by passing the examples through the base model [^46]. SVM classifiers are commonly used in this context [^47].
- Weight Imprinting: This strategy involves initializing the model weights corresponding to the novel classes using the mean activations of the model when processing the few-shot examples [^48]. Weight imprinting provides an informed initialization for the novel classes before further training [^49].

In general, transfer learning provides an effective approach for few-shot learning by allowing prior knowledge gained on data-rich base classes to be utilized when adapting to data-scarce novel classes. However, the specific transfer strategy must be carefully designed to minimize overfitting to the limited novel data while avoiding catastrophic forgetting of the base classes.

### 2.3 Inductive Biases for Effective Few-Shot Learning

Effective FSL depends on the ability of models to quickly adapt to novel concepts and tasks with only a handful of data points available to them [^198]. Such rapid adaptation demands the incorporation of strong inductive biases, guiding the model’s learning trajectory in alignment with the overarching objective of few-shot generalization. Several key techniques have emerged as fundamental approaches to instilling these guiding principles, such as meta-learning algorithms, distance metric learning, data augmentation, and regularization.

- Meta-learning Algorithms: Meta-learning, also known as “learning to learn”, is a foundational paradigm in the field of FSL. Algorithms such as Model-Agnostic Meta-Learning (MAML) by Finn et al. [^29] exemplify this approach by optimizing models to discover optimal initialization parameters. These parameters then enable rapid adaptation to novel tasks and concepts during the FSL phase. Meta-learning is based on episodic training, where models are trained over a wide range of adaptation episodes. Each episode replicates a distinct FSL task. This episodic exposure creates an inductive bias in the model, preparing it for efficient generalization even when confronted with entirely unfamiliar tasks and concepts in the future. As explained by Hospedales et al. [^50], meta-learning algorithms aim to acquire fundamental learning skills that go beyond specific tasks, thus developing robust models that can swiftly adapt from limited data. The learned inductive biases capture underlying structure of task distributions, enabling rapid learning of new tasks from sparse data. Hence, meta-learning has become a crucial strategy for FSL, equipping models with the ability to generalize rather than solely relying on memorization.
- Metric-based Approaches: Metric-based approaches have emerged as a powerful paradigm for FSL, leveraging learned distance metrics and embedding spaces to enable effective knowledge transfer from limited data. As previously discussed by [^28], the core concept is to learn an embedding function $f(x)$ that projects inputs into a feature space where distances reflect semantic similarities. By transforming inputs into an informed embedding space, models can intelligently relate new examples to available prior knowledge, even under data scarcity [^51]. The strength of metric-based FSL arises from the ability to create embeddings that encapsulate semantic nuances, thereby enabling meaningful extrapolation from only a few examples [^5]. Prototypical networks, introduced by [^28], present one effective metric-based approach for FSL problems in computer vision. This method computes class prototypes by averaging the embedded support examples belonging to each class. Query points are then classified based on distance to these learned prototypes in the embedding space. By condensing classes into prototypical representations, models can rapidly assimilate new concepts from few examples during the testing phase. In summary, metric-based approaches, such as prototypical networks, utilize informed embedding spaces to enable intelligent matching and rapid adaptation for FSL. By encapsulating semantic relationships within learned distances, models can transfer knowledge and make inferences about novel concepts from just a few examples.
- Data Augmentation: Data augmentation has emerged as a vital technique to mitigate the challenges of limited training data for FSL. As discussed by [^52], data augmentation artificially expands the limited dataset by generating diverse variations of the existing examples. This simulates the variability that would be present in larger datasets, while reducing the risk of models simply memorizing the constrained training examples. Through data augmentation, models are exposed to a rich tapestry of information despite limited data, empowering the extraction of meaningful patterns and relationships. Augmented data provides crucial regularization during few-shot model training, enabling robust generalization instead of overfitting to a small number of examples [^53].
	Techniques such as random cropping, rotations, and color jittering can produce augmented variants that capture essential invariant characteristics in the data [^54]. This facilitates the learning of more universal features that transfer to novel concepts in few-shot scenarios. In summary, data augmentation stands as a vital strategy in FSL to artificially expand limited training data, prevent memorization, extract meaningful features, and improve generalizability. The diversity generated from existing examples provides a regularization effect that primes models for effective adaptation even when data is scarce
- Regularization Techniques: In FSL, where limited data is inherent, overfitting poses a significant threat to model performance. As discussed by [^55], regularization techniques like weight decay and dropout are critical to mitigate overfitting in data-scarce regimes.
	By intentionally restricting model flexibility, regularization forces the model to identify more robust, generalizable patterns instead of latching onto spurious correlations [^56]. As described by [^57], this selectivity prevents models from relying on superficial cues, compelling focus on fundamental relationships that better transfer across varied concepts.
	In few-shot contexts, regularization is vital to prevent models from simply memorizing sparse training examples and failing to generalize [^58]. Techniques like dropout improve generalization by limiting co-adaptation between neurons [^56]. Overall, by intentionally limiting model capacity, regularization promotes extraction of core invariant features, enhancing FSL performance despite limited data.

Together, these inductive biases empower models with core capabilities like generalization, knowledge transfer, and rapid adaptation that are vital for excelling in FSL.

## 3 Foundations of Object Detection

This section begins with an overview of object detection, including common techniques and applications. It then discusses various techniques used in video and 3D object detection.

### 3.1 Object Detection

Object detection (OD) is a cornerstone of computer vision (CV) that seamlessly integrates the tasks of classification and localization. Its aim is twofold: assigning class labels to images and enclosing each detected object within a bounding box. These bounding boxes are typically delineated by a starting point coupled with the dimensions (height and width) of the box. Given the inherent variability in the number of objects within different images, initial OD strategies were crafted around sliding window classification problems. However, the evolution of deep learning has ushered in the dominance of convolutional neural network (CNN) based methodologies.

The quintessential data structure for OD encompasses a dataset of $N_{s}$ supervised samples

$$
D=\{X\}_{i=1}^{N_{s}},\{y\}_{i=1}^{N_{s}}
$$

where each image $X_{i}$ possesses dimensions $W\times H\times 3$ and is paired with a set of object annotations $y_{i}$. Feature maps, denoted by $F$, are extracted from input images. These maps encapsulate sub-regions termed as “Regions of Interest” (RoIs). The detection process then undergoes two pivotal stages: bounding box regression and object classification.

Object detection strategies are largely divided into two main paradigms: two-stage detectors and one-stage detectors. Two-stage detectors initially generate region proposals and subsequently classify and refine the proposed regions in a second stage. This allows for more accurate localization and classification of objects but at the cost of slower inference speed. One-stage detectors directly predict object classes and locations in one pass through the network, allowing for faster inference but typically with reduced accuracy compared to two-stage methods. Popular two-stage detectors include R-CNN [^59], Fast R-CNN [^61], and Faster R-CNN [^62], which utilize region proposal networks to generate candidate object regions. Prominent one-stage detectors include SSD [^71] and YOLO [^63], which apply convolutional filters across an image in a single shot to directly output object locations and classes. The tradeoff between accuracy and speed makes two-stage detectors preferable for applications where accuracy is critical, while one-stage detectors are better suited for real-time applications requiring very fast inference speeds. Two-stage detectors are generally more accurate because they use region proposal networks to narrow down potential object locations before making final classifications. However, their multi-step approach comes at the cost of slower inference. One-stage detectors make predictions in a single pass, allowing much faster inference, but they sacrifice some accuracy due to making localization and classification predictions simultaneously across full images. The choice between one-stage and two-stage detectors depends on the specific requirements of the application. Tasks demanding high accuracy like medical imaging would benefit more from two-stage detectors, while self-driving vehicles and real-time surveillance may need the faster inference of one-stage detectors even if some accuracy is compromised. In order to gain a better comprehension of them, some well-known single-stage and two-stage structures are outlined below.

#### 3.1.1 Two-Stage Detectors: The Case of Faster R-CNN

The Faster R-CNN architecture stands out in the two-stage detector category. It seamlessly integrates two networks:

##### Region Proposal Network (RPN)

The RPN is essentially a fully convolutional network that simultaneously predicts object bounds and objectness scores at each position. The RPN operates on several scales due to the pyramidal form of its architecture, allowing it to detect objects of various sizes. The anchor boxes play a significant role in the operation of the RPN. The anchor boxes are essentially bounding boxes of different scales and aspect ratios that act as references for object proposals. For each anchor box, the RPN predicts two things: the presence or absence of an object (foreground or background classification) and the refinements needed to better fit the potential object (bounding box regression). The RPN makes these predictions using a sliding window approach. It slides a small network over the convolutional feature map output by the previous layer, which is used to predict both the objectness and the bounding box coordinates for the anchor boxes. The output of the RPN is a set of object proposals, each with an objectness score. These object proposals are currently in an early stage, requiring refinement as they may not perfectly align with the intended target object. Therefore, the RPN also proposes refinements to the bounding boxes that are designed to improve the fit of the box to the object. These proposed regions are then reshaped to extract a fixed-length feature for each, using a process known as Region of Interest (RoI) pooling. This ensures that the subsequent fully connected layers receive inputs of a fixed size, regardless of the size of the proposed regions. The features extracted from the proposed regions are then channeled into the detection network as described below.

##### Detection Network (Fast R-CNN)

Once the Region Proposal Network (RPN) has been trained and the regions of interest have been identified, the Fast R-CNN architecture comes into play. It extracts features from these regions using a Region of Interest (RoI) pooling layer, which performs max pooling on inputs of non-uniform sizes to obtain fixed-size feature maps. These are then used to predict the class of the object and the bounding box regressors. The Fast R-CNN also uses a multi-task loss function that combines the losses for classification and bounding box regression. The classification loss is computed using log loss, while the bounding box regression loss is computed using a smooth $L_{1}$ loss function, similar to the RPN. This combination of loss functions allows the network to simultaneously learn to classify and localize objects, improving its overall performance. The softmax classifier in the Fast R-CNN architecture is responsible for assigning class probabilities to the proposed regions. It utilizes softmax loss, which is a type of cross entropy loss, to compute the probability distribution over all possible classes. The bounding box regressors are responsible for refining the proposed regions to more accurately encapsulate the objects. For each class, there is a separate bounding box regressor, which adjusts the coordinates of the proposed region to minimize the difference between the predicted and ground-truth bounding boxes.

#### 3.1.2 One-Stage Detectors: Spotlight on YOLO and SSD

##### YOLO (You Only Look Once)

The YOLO framework, first introduced by [^63], is a seminal one-stage object detector based on a single convolutional neural network that jointly predicts class probabilities and bounding boxes. As analyzed by [^63], YOLO divides the input image into an $S\times S$ grid and each grid cell predicts $B$ bounding boxes along with confidence scores reflecting objectness. The bounding box predictions consist of the box center coordinates, dimensions, and class. While YOLO employs contextual information for high recall, its grid-based approach can miss small objects. To address this, YOLOv2 [^64] introduced anchor boxes and multi-scale training. Further refinements in YOLOv3 [^65] incorporated a deeper network architecture with multi-scale predictions, improving accuracy while maintaining real-time performance. YOLOv4 [^66] built on YOLOv3 by introducing techniques like weighted residual connections, cross-stage partial connections, cross mini-batch normalization, and self-adversarial training to optimize speed and accuracy. The open-source YOLOv5 [^67] further refined the model for efficiency and ease of use. Recently, YOLOv6 [^68] adopted an anchor-free design optimized for industrial use cases, achieving 52.5% AP on MS COCO. YOLOv7 [^69] pushed accuracy and speed even further, surpassing all prior detectors across a range of FPS targets without pre-trained backbones. Key innovations in YOLOv7 include efficient self-supervised learning, scalable model design, and accuracy-boosting enhancements. Most recently, YOLOv8 [^70] introduced an anchor-free approach with fewer predicted boxes and faster NMS. By disabling aggressive augmentation late in training, YOLOv8 achieved 53.9% AP on MS COCO at 640px input size, surpassing prior versions.

##### SSD (Single Shot Detection)

Single Shot MultiBox Detector (SSD) improves upon YOLO by employing anchor boxes tailored to diverse object shapes and performing detection across multiple feature maps to achieve robustness across varying object scales. As analyzed by Liu et al. [^71], SSD utilizes feature maps from different layers in a convolutional network, with smaller feature maps focusing on larger objects and layers with higher resolution detecting smaller objects. This multi-scale design stands in contrast to YOLO’s single output scale and enables SSD to capture objects across a wide range of sizes. Specifically, SSD attaches convolutional predictors for detection to multiple feature layers. Shallow layers with smaller receptive fields focus on small instance detection, while deeper layers learn coarser semantics useful for detecting larger objects. The predictions from all layers are aggregated and refined via non-maximum suppression to produce the final detections across scales. By harnessing features attuned to different object scales, the multi-feature map architecture of SSD achieves strong performance across objects of varied sizes. This design has influenced subsequent single-stage detectors focused on handling scale variation, such as RetinaNet and EfficientDet, enabling robust one-stage detection across a spectrum of object scales.

### 3.2 Video and 3D Object Detection

Video object detection refers to the task of detecting and localizing objects across frames in a video stream, as opposed to static images. This introduces additional challenges compared to image-based object detection, including motion blur, video defocus, complex object motions, and viewpoint variations across frames. Effective video object detection requires modeling temporal information and propagating detections across frames.

3D object detection involves identifying and localizing objects within 3D sensor data such as point clouds, voxel grids, or mesh representations generated from stereo cameras, LIDAR, or other 3D sensing modalities. Compared to 2D images, 3D data lacks reliable texture and color cues while presenting difficulties like sparsity and occlusion patterns. Successful 3D detection relies more heavily on modeling geometric shapes and leveraging structural cues. Both video and 3D object detection have become crucial technologies enabling various applications including autonomous vehicles, augmented/virtual reality, robotics, surveillance, and environmental mapping. While image-based object detection only requires reasoning about a 2D scene, video and 3D detection demand more complex spatiotemporal and geometric reasoning to perceive objects in dynamic or 3D environments. This has motivated research into specialized techniques for these modalities, including spatiotemporal feature learning for video and view-invariant shape recognition for 3D. With growing prevalence of video and 3D sensing, advancing object detection in these domains remains an important challenge.

#### 3.2.1 Video Object Detection Approaches

Multi-frame feature aggregation is a key technique for harnessing temporal context to improve video object detection accuracy [^72]. By processing multiple frames, inter-frame correlations can be used to enhance per-frame detections [^73]. There are several aggregation methods:

- Temporal aggregation propagates detections using optical flow [^74] [^75] or aligns and averages neighboring frame features [^73], providing context to resolve ambiguities.
- Spatial aggregation applies larger receptive fields or coarse pooling to frames farther from the reference, organizing multi-scale features [^76]. Measuring pixel-level context similarity also enhances features [^77].
- Coarse-to-fine aggregation combines features from neighboring frames in a coarse-to-fine manner, with farther frames having larger receptive fields [^76]. Coarse pooling support frames boosts inter-frame complementarity [^78].

Some key benefits include improving per-frame features via temporal/spatial correlations, enabling multi-scale representations, resolving per-frame detection ambiguities, and enhancing cross-frame feature complementarity.

Flow-guided aggregation employs optical flow to establish inter-frame feature correspondence [^79]. Flow warps adjacent frame features to align with the current frame before aggregation [^80] [^81]. Compared to box-level aggregation, it enables flexible multi-frame fusion at earlier layers before final detection [^79]. End-to-end learning can jointly optimize flow, features, and aggregation. Despite accuracy gains over single-frame detection, challenges include computational cost and handling large motions [^82].

Recent transformer-based architectures have shown promising results for advancing video object detection by enabling effective spatial-temporal reasoning. Notable approaches include TransVOD [^83], the first end-to-end transformer model for video detection without post-processing, and DETR [^84], which eliminates hand-designed components in detectors via a transformer encoder-decoder architecture. Although originally for images, DETR has been adapted for video tasks [^86]. Other examples are: TT-SRN [^85], which aggregates spatial-temporal information for joint detection, segmentation, and tracking; and Swin Transformer [^87], a hierarchical model used for various vision tasks including video detection. TOD-Net [^88] is another transformer-based framework improving query representations by feature aggregation.

Transformers and multi-frame feature aggregation offer complementary techniques for modeling temporal context. Transformer-based methods like TransVOD [^83] and DETR [^86] have achieved state-of-the-art accuracy by capturing long-range dependencies [^89]. Multi-frame aggregation also boosts accuracy but is generally outperformed by transformers [^72]. Multi-frame approaches have lower computational cost by operating on earlier features, while transformers are more expensive due to self-attention. However, efficient transformer designs are being explored [^89]. Transformers inherently excel at temporal modeling through their architecture, whereas aggregation relies more on fixed schemes. Transformers also better handle varying video characteristics thanks to self-attention.

Recent works have combined both approaches, such as the FAQ method [^89], which aggregates inter-frame features to enhance transformer queries, and Attention-Guided Disentangled Feature Aggregation [^80], a method that combines features from multiple frames to improve object detection by leveraging inter-frame correlations. These works show that combining transformers with multi-frame aggregation can improve accuracy by jointly modeling temporal context and leveraging inter-frame correlations. The integration of transformer self-attention and multi-frame feature aggregation remains an active area for advancing video understanding by utilizing their complementary strengths.

#### 3.2.2 3D Object Detection Approaches

This section provides an overview of deep learning based 3D detection using different modalities and input representations, focusing on LiDAR and camera-based approaches as they are most prevalent.

##### LiDAR-Based 3D Object Detection

LiDAR directly provides sparse 3D point clouds encoding precise geometric scene information. Earlier methods discretize the point cloud into 3D voxels and apply 3D convolutions. However, these are computationally expensive. More recent methods operate on raw point clouds by designing permutation invariant networks. PointNet [^90] is a pioneering work enabling direct point cloud processing. Subsequent works like PointRCNN [^91], Part-A2 Net [^92], and PV-RCNN [^93] extend it for 3D detection by first generating proposals which are then refined using point features. Another line of work aggregates points into compact representations like pillars which encode vertical point columns, before applying efficient 2D convolutions on pseudo images. Pillar-based methods like PointPillars [^94] and PIXOR [^95] are efficient but lose fine details. Recent pillar variants like SpindleNet [^96] and CenterPoint [^97] improve representations by encoding local context more effectively. Range view methods like LaserNet utilize established image processing techniques by projecting point clouds into 2D range images and applying 2D convolutions.

##### Camera-Based 3D Object Detection

Camera-based 3D object detection has been enhanced by the rich texture information provided by cameras. Earlier works, such as 3DOP [^98], have lifted 2D detections into 3D using ground plane assumptions. More recent methods, including Mono3D [^99], Mono3D++ [^100], and Pseudo-LiDAR [^101], have improved performance by first estimating depth from images and then applying LiDAR-based detectors. Other approaches, such as Deep3DBox [^199] and ROI-10D [^200], have directly regressed 3D boxes from images without depth estimation, but have relied heavily on priors. Stereo cameras have provided better geometric constraints compared to monocular images, enabling techniques like Pseudo-LiDAR++ [^101] to further improve performance by combining depth and imagery.

##### Multi-Sensor Fusion

LiDAR and camera provide complementary geometric and semantic information that can be fused to improve 3D detection accuracy, especially for small or distant objects [^102]. Early fusion methods like AVOD [^103] fuse LiDAR and RGB early in the network, while late fusion approaches like Frustum PointNets [^201] use RGB to generate frustum proposals for LiDAR. PointPainting [^104] paints LiDAR points with semantic image features. PointFusion [^105] dynamically fuses multimodal features throughout the network. Recent techniques explore more robust fusion using attention mechanisms. The proposed TransFusion [^202] method fuses LiDAR and images using a novel transformer architecture with soft-attention. It first generates initial boxes from LiDAR, then fuses image features in the second decoder layer using soft-attention. This provides robustness to misalignment and degraded image quality. Evaluated on KITTI and Waymo datasets, TransFusion outperforms prior fusion methods by 2-4% in various metrics. It sets a new state-of-the-art for LiDAR-camera fusion in 3D detection by leveraging transformer attention to achieve soft-association between the modalities. The results validate that soft-attention based sensor fusion using transformers is an effective approach for handling misalignment and image degradations.

### 3.3 Few-Shot Object Detection

Few-shot object detection (FSOD) poses unique challenges compared to few-shot classification, as models need to accurately localize objects using extremely limited bounding box annotations. Several key techniques have been tailored specifically for few-shot object detection, including:

- Two-stage detectors with incremental learning: In this approach, two-stage detectors like Faster R-CNN are modified for few-shot detection by incorporating separate classifier branches for novel classes. These branches are trained on the limited data, while keeping the weights of the base classes frozen during novel class training. This incremental learning strategy helps prevent interference [^106].
- One-stage detectors with label smoothing: One-stage detectors such as YOLO are optimized for few-shot detection through the use of label smoothing techniques during training. By redistributing a portion of the target probability mass to non-ground truth classes, label smoothing improves model calibration and mitigates overfitting to the scarce training examples [^107] [^108].
- Transformer-based detectors: Transformer architectures, such as DETR, are particularly well-suited for few-shot detection. These architectures employ the self-attention mechanism to effectively model relationships between sparsely annotated examples. The absence of hand-designed components in transformers provides flexibility in adapting to few-shot detection scenarios [^111].
- Advanced data augmentation: Specialized augmentation techniques, such as CutMix, play a crucial role in enhancing few-shot detection. CutMix involves blending object patches from different images to create new training examples, thereby improving the model’s ability to handle few-shot scenarios. Additionally, techniques like class mixup and contextual augmentation help prevent overfitting in these challenging settings [^109] [^110].

These techniques address the inherent difficulties faced in few-shot object detection, enabling models to overcome the limitations imposed by scarce annotations and perform object localization effectively. The main challenges in few-shot object detection include:

- Localization from scarce bounding box annotations: Accurate localization becomes extremely difficult when there are only a few bounding box annotations available per novel class. The regression task becomes highly unstable, leading to poor generalizability. To address this researchers have explored techniques such as meta-learning [^112] and instance weighting [^15] to improve localization performance with limited annotations.
- Imbalance between base and novel classes: Few-shot detection inevitably creates an imbalance between base classes with abundant training data and novel classes with only a few examples [^113]. Focusing on frequent base classes often causes models to neglect rare new classes. This imbalance can be addressed using techniques like class balancing [^114], where training samples from novel classes are augmented or weighted to alleviate the class imbalance problem.
- Domain shift between base and novel classes: Complex domain shifts often exist between the distributions of the base dataset and the novel classes that the model must adapt to [^21]. Few-shot models struggle to transfer knowledge effectively across domains, leading to reduced performance on novel classes. To mitigate this challenge, researchers have proposed domain adaptation methods, such as domain alignment [^115] or domain generalization [^116], to align the feature distributions between the base and novel classes.
- Context modeling from limited examples: With only a few examples available, modeling contextual relationships between objects in a scene becomes highly ambiguous and uncertain [^110]. This lack of context information makes few-shot detection unreliable. To overcome this challenge, researchers have explored techniques such as attention mechanisms [^117] and graph-based reasoning [^118] to incorporate context information and improve the detection performance of novel classes.
- Prevention of overfitting: Modern deep detection models with high capacity easily overfit to the scarce few-shot data, memorizing training examples without generalizing well to new instances [^113]. This demands principled regularization techniques tailored for few-shot scenarios. Regularization techniques such as dropout, weight decay, and early stopping are commonly applied to prevent overfitting and improve the generalization ability of few-shot detection models [^110].

These challenges highlight the complexity of few-shot object detection and the need for innovative techniques to address them. Researchers are actively exploring novel algorithms and approaches to overcome these challenges and improve the performance of few-shot object detection models. For example, advanced data augmentation techniques have shown promise for synthesizing useful training signals from limited data. However, as helpfully pointed out in [^203], there are information-theoretic limits on the additional ‘information’ that can be fabricated from scarce examples. Recent studies such as [^204] have analyzed these limits for few-shot learning, finding diminishing returns on augmentation beyond a certain point. Other works like [^205] have proposed more principled augmentation approaches that consider information-theoretic measures to maximize diversity within feasible bounds. But further research is still needed into specialized augmentation techniques that work within information-theoretic constraints to provide useful signals without overstepping the true information content of limited training data. In addition to data augmentation, algorithms like [^51] [^28] aim to improve few-shot detection through other techniques such as meta-learning, metric-based learning, context modeling, and transfer learning. There are still many possibilities to enhance few-shot object detection through innovations in modeling, training strategies, evaluation protocols, and datasets. By overcoming challenges like scarce annotations, class imbalance, and domain shifts, researchers can achieve significant progress in few-shot detection and minimize dependency on extensive supervised data.

## 4 Few-Shot Video Object Detection

![Refer to caption](https://arxiv.org/html/2507.17079v1/extracted/6645249/images/FS_Video2.png)

Figure S2: Overview of the two-stage architecture for few-shot video object detection proposed by 119. It consists of: 1) A Tube Proposal Network (TPN) that generates spatiotemporal proposals representing object trajectories across frames. 2) A Tube Matching Network (TMN) that classifies tube proposals by matching against few-shot support examples using multi-relation modules. The Temporal Alignment Branch (TAB) aligns query features across frames before matching. This design applies various techniques such as metric learning, temporal feature aggregation, and episodic training to enable effective few-shot detection in videos.

![Refer to caption](https://arxiv.org/html/2507.17079v1/extracted/6645249/images/When_FSL_Meets_Video2.png)

Figure S3: Overview of the Thaw architecture for few-shot video object detection proposed by 24. It consists of three key phases: 1) Pretraining on base classes using the MEGA model to extract multi-level local and global spatiotemporal features from input videos. 2) Adaptation on novel classes by adding a cosine classifier tuned on the scarce novel training data. 3) Fine-tuning using techniques like model freezing and gradual unfreezing to balance knowledge retention and adaptation. The tailored two-stage training process and integration of spatial-temporal information from video enables effective few-shot detection.

![Refer to caption](https://arxiv.org/html/2507.17079v1/extracted/6645249/images/3D_proto_votenet11.png)

Figure S4: Overview of the Prototypical VoteNet architecture for few-shot 3D object detection 154. It contains two key components - the Prototypical Vote Module (PVM) which refines local features using geometric prototypes, and the Prototypical Head Module (PHM) which enhances object features using class-specific prototypes derived from the few-shot support examples. The class-agnostic PVM and class-specific PHM work together to enable effective FSL.

## 5 Few-Shot 3D Object Detection

![Refer to caption](https://arxiv.org/html/2507.17079v1/extracted/6645249/images/3D_LiDAR_Point_Cloud22.png)

Figure S5: Overview of the generalized few-shot 3D object detection framework proposed by Liu et al. 155. It consists of a 3D feature extractor based on VoxelNet, followed by a region proposal network (RPN). A shared convolution layer feeds into separate prediction heads for base and novel classes, with incremental branches added for each novel class.

![Refer to caption](https://arxiv.org/html/2507.17079v1/extracted/6645249/images/3D_LiDAR_Point_Cloud11.png)

Figure S6: The incremental classifier branches tailored for each novel class in the few-shot 3D detection framework 155. The branches share an earlier convolutional layer with the base classes to avoid interference. Only the novel class branches are updated during the fine-tuning stage.

![Refer to caption](https://arxiv.org/html/2507.17079v1/extracted/6645249/images/metadet3D1.png)

Figure S7: Overview of the MetaDet3D framework for few-shot 3D object detection 156. It consists of a 3D Meta-Detector module that generates class-specific reweighting vectors zn from the few-shot support points. These reweighting vectors guide the 3D Object Detector module, which contains point feature extraction, guided voting and clustering, and guided object proposal components. The reweighting vectors transfer knowledge from the scarce supports to enhance few-shot detection.

![Refer to caption](https://arxiv.org/html/2507.17079v1/extracted/6645249/images/ngm1.png)

Figure S8: Overview of the Neural Graph Matching (NGM) Networks approach for few-shot 3D action recognition 157. Videos are encoded into graph representations, with nodes as frame features and edges capturing temporal relationships. Graph matching is performed between support and query graphs by comparing node and edge features using cosine similarity. This structural matching enables effective FSL.

![Refer to caption](https://arxiv.org/html/2507.17079v1/extracted/6645249/images/janie2.png)

Figure S9: Overview of the few-shot action recognition framework on 3D skeletal sequences proposed by Wang et al. 158. It consists of two key components - the Encoding Network (EN) which models temporal dynamics from skeletal blocks, and the Joint tEmporal and cAmera viewpoiNt alIgnmEnt (JEANIE) module which aligns sequences in both time and viewpoint space for robust matching.

[^1]: T.-Y. Lin, P. Goyal, R. Girshick, K. He, P. Dollár, Focal loss for dense object detection, in: Proceedings of the IEEE international conference on computer vision, 2017, pp. 2980–2988.

[^2]: A. Krizhevsky, I. Sutskever, G. E. Hinton, Imagenet classification with deep convolutional neural networks, Advances in neural information processing systems 25 (2012).

[^3]: Z. Tian, C. Shen, H. Chen, T. He, Fcos: Fully convolutional one-stage object detection, in: Proceedings of the IEEE/CVF international conference on computer vision, 2019, pp. 9627–9636.

[^4]: J.-B. Alayrac, J. Donahue, P. Luc, A. Miech, I. Barr, Y. Hasson, K. Lenc, A. Mensch, K. Millican, M. Reynolds, et al., Flamingo: a visual language model for few-shot learning, Advances in Neural Information Processing Systems 35 (2022) 23716–23736.

[^5]: F. Sung, Y. Yang, L. Zhang, T. Xiang, P. H. Torr, T. M. Hospedales, Learning to compare: Relation network for few-shot learning, in: Proceedings of the IEEE conference on computer vision and pattern recognition, 2018, pp. 1199–1208.

[^6]: S. Ravi, H. Larochelle, Optimization as a model for few-shot learning, in: International conference on learning representations, 2016.

[^7]: Y. Wang, Q. Yao, J. T. Kwok, L. M. Ni, Generalizing from a few examples: A survey on few-shot learning, ACM computing surveys (csur) 53 (3) (2020) 1–34.

[^8]: Y. Song, T. Wang, P. Cai, S. K. Mondal, J. P. Sahoo, A comprehensive survey of few-shot learning: Evolution, applications, challenges, and opportunities, ACM Computing Surveys (2023).

[^9]: Y. Xian, C. H. Lampert, B. Schiele, Z. Akata, Zero-shot learning—a comprehensive evaluation of the good, the bad and the ugly, IEEE transactions on pattern analysis and machine intelligence 41 (9) (2018) 2251–2265.

[^10]: A. Nichol, J. Achiam, J. Schulman, On first-order meta-learning algorithms, arXiv preprint arXiv:1803.02999 (2018).

[^11]: Z. Li, F. Zhou, F. Chen, H. Li, Meta-sgd: Learning to learn quickly for few-shot learning, arXiv preprint arXiv:1707.09835 (2017).

[^12]: Q. Sun, Y. Liu, T.-S. Chua, B. Schiele, Meta-transfer learning for few-shot learning, in: Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, 2019, pp. 403–412.

[^13]: Z. Yu, L. Chen, Z. Cheng, J. Luo, Transmatch: A transfer-learning scheme for semi-supervised few-shot learning, in: Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, 2020, pp. 12856–12864.

[^14]: W. Jiang, K. Huang, J. Geng, X. Deng, Multi-scale metric learning for few-shot learning, IEEE Transactions on Circuits and Systems for Video Technology 31 (3) (2020) 1091–1102.

[^15]: L. Qiao, Y. Zhao, Z. Li, X. Qiu, J. Wu, C. Zhang, Defrcn: Decoupled faster r-cnn for few-shot object detection, in: Proceedings of the IEEE/CVF International Conference on Computer Vision, 2021, pp. 8681–8690.

[^16]: B. Sun, B. Li, S. Cai, Y. Yuan, C. Zhang, Fsce: Few-shot object detection via contrastive proposal encoding, in: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2021, pp. 7352–7362.

[^17]: G. Han, J. Ma, S. Huang, L. Chen, S.-F. Chang, Few-shot object detection with fully cross-transformer, in: Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, 2022, pp. 5321–5330.

[^18]: M. Anderson, Video object recognition and detection, [https://www.itransition.com/blog/video-object-recognition-detection](https://www.itransition.com/blog/video-object-recognition-detection), accessed: 2023-08-31.

[^19]: I. Kolesnikova, Detecting objects in video: a comprehensive guide 2022, [https://mindtitan.com/resources/blog/detecting-objects-in-video/](https://mindtitan.com/resources/blog/detecting-objects-in-video/), accessed: 2023-08-31.

[^20]: S. Antonelli, D. Avola, L. Cinque, D. Crisostomi, G. L. Foresti, F. Galasso, M. R. Marini, A. Mecca, D. Pannone, Few-shot object detection: A survey, ACM Computing Surveys (CSUR) 54 (11s) (2022) 1–37.

[^21]: L. Jiaxu, C. Taiyue, G. Xinbo, Y. Yongtao, W. Ye, G. Feng, W. Yue, A comparative review of recent few-shot object detection algorithms, arXiv preprint arXiv:2111.00201 (2021).

[^22]: X. Li, Z. Sun, J.-H. Xue, Z. Ma, A concise review of recent few-shot meta-learning methods, Neurocomputing 456 (2021) 463–468.

[^23]: E. d’Archimbaud, Video Object Detection: AI’s New Challenge, [https://kili-technology.com/data-labeling/computer-vision/video-annotation/video-object-detection](https://kili-technology.com/data-labeling/computer-vision/video-annotation/video-object-detection), accessed: 2023-08-31.

[^24]: Z. Yu, G. Wang, L. Chen, S. Raschka, J. Luo, When few-shot learning meets video object detection, in: 2022 26th International Conference on Pattern Recognition (ICPR), IEEE, 2022, pp. 2986–2992.

[^25]: Y. Zhou, O. Tuzel, Voxelnet: End-to-end learning for point cloud based 3d object detection, in: Proceedings of the IEEE conference on computer vision and pattern recognition, 2018, pp. 4490–4499.

[^26]: C. R. Qi, L. Yi, H. Su, L. J. Guibas, Pointnet++: Deep hierarchical feature learning on point sets in a metric space, Advances in neural information processing systems 30 (2017).

[^27]: Z. Jiang, X. Chen, X. Huang, X. Du, D. Zhou, Z. Wang, Back razor: Memory-efficient transfer learning by self-sparsified backpropagation, Advances in Neural Information Processing Systems 35 (2022) 29248–29261.

[^28]: J. Snell, K. Swersky, R. Zemel, Prototypical networks for few-shot learning, Advances in neural information processing systems 30 (2017).

[^29]: C. Finn, P. Abbeel, S. Levine, Model-agnostic meta-learning for fast adaptation of deep networks, in: International conference on machine learning, PMLR, 2017, pp. 1126–1135.

[^30]: G. Koch, R. Zemel, R. Salakhutdinov, et al., Siamese neural networks for one-shot image recognition, in: ICML deep learning workshop, Vol. 2, Lille, 2015.

[^31]: S. J. Pan, Q. Yang, A survey on transfer learning, IEEE Transactions on knowledge and data engineering 22 (10) (2009) 1345–1359.

[^32]: M. Boudiaf, I. Ziko, J. Rony, J. Dolz, P. Piantanida, I. Ben Ayed, Information maximization for few-shot learning, Advances in Neural Information Processing Systems 33 (2020) 2445–2457.

[^33]: J. Zhang, C. Zhao, B. Ni, M. Xu, X. Yang, Variational few-shot learning, in: Proceedings of the IEEE/CVF International Conference on Computer Vision, 2019, pp. 1685–1694.

[^34]: L. Qiao, Y. Shi, J. Li, Y. Wang, T. Huang, Y. Tian, Transductive episodic-wise adaptive metric for few-shot learning, in: Proceedings of the IEEE/CVF international conference on computer vision, 2019, pp. 3603–3612.

[^35]: S. Hajimiri, M. Boudiaf, I. Ben Ayed, J. Dolz, A strong baseline for generalized few-shot semantic segmentation, in: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2023, pp. 11269–11278.

[^36]: E. Gavves, T. Mensink, T. Tommasi, C. G. Snoek, T. Tuytelaars, Active transfer learning with zero-shot priors: Reusing past datasets for future tasks, in: Proceedings of the IEEE International Conference on Computer Vision, 2015, pp. 2731–2739.

[^37]: M. Mosbach, T. Pimentel, S. Ravfogel, D. Klakow, Y. Elazar, Few-shot fine-tuning vs. in-context learning: A fair comparison and evaluation, arXiv preprint arXiv:2305.16938 (2023).

[^38]: P. Eustratiadis, Ł. Dudziak, D. Li, T. Hospedales, Neural fine-tuning search for few-shot learning, arXiv preprint arXiv:2306.09295 (2023).

[^39]: P. Peng, J. Wang, How to fine-tune deep neural networks in few-shot learning?, arXiv preprint arXiv:2012.00204 (2020).

[^40]: H. Liu, D. Tam, M. Muqeeth, J. Mohta, T. Huang, M. Bansal, C. A. Raffel, Few-shot parameter-efficient fine-tuning is better and cheaper than in-context learning, Advances in Neural Information Processing Systems 35 (2022) 1950–1965.

[^41]: Z. Shen, Z. Liu, J. Qin, M. Savvides, K.-T. Cheng, Partial is better than all: revisiting fine-tuning strategy for few-shot learning, in: Proceedings of the AAAI Conference on Artificial Intelligence, Vol. 35, 2021, pp. 9594–9602.

[^42]: S. X. Hu, D. Li, J. Stühmer, M. Kim, T. M. Hospedales, Pushing the limits of simple pipelines for few-shot learning: External data and fine-tuning make a difference, in: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2022, pp. 9068–9077.

[^43]: Y. Guo, H. Shi, A. Kumar, K. Grauman, T. Rosing, R. Feris, Spottune: transfer learning through adaptive fine-tuning, in: Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, 2019, pp. 4805–4814.

[^44]: C. Li, S. Li, H. Wang, F. Gu, A. D. Ball, Attention-based deep meta-transfer learning for few-shot fine-grained fault diagnosis, Knowledge-Based Systems 264 (2023) 110345.

[^45]: X. Xu, Z. Wang, Z. Chi, H. Yang, W. Du, Complementary features based prototype self-updating for few-shot learning, Expert Systems with Applications 214 (2023) 119067.

[^46]: A. Shafahi, P. Saadatpanah, C. Zhu, A. Ghiasi, C. Studer, D. Jacobs, T. Goldstein, Adversarially robust transfer learning, arXiv preprint arXiv:1905.08232 (2019).

[^47]: E. Real, C. Liang, D. So, Q. Le, Automl-zero: Evolving machine learning algorithms from scratch, in: International conference on machine learning, PMLR, 2020, pp. 8007–8019.

[^48]: A. Abuduweili, X. Li, H. Shi, C.-Z. Xu, D. Dou, Adaptive consistency regularization for semi-supervised transfer learning, in: Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, 2021, pp. 6923–6932.

[^49]: D. Yan, J. Huang, H. Sun, F. Ding, Few-shot object detection with weight imprinting, Cognitive Computation (2023) 1–11.

[^50]: T. Hospedales, A. Antoniou, P. Micaelli, A. Storkey, Meta-learning in neural networks: A survey, IEEE transactions on pattern analysis and machine intelligence 44 (9) (2021) 5149–5169.

[^51]: O. Vinyals, C. Blundell, T. Lillicrap, D. Wierstra, et al., Matching networks for one shot learning, Advances in neural information processing systems 29 (2016).

[^52]: A. Antoniou, A. Storkey, H. Edwards, Data augmentation generative adversarial networks, arXiv preprint arXiv:1711.04340 (2017).

[^53]: C. Shorten, T. M. Khoshgoftaar, A survey on image data augmentation for deep learning, Journal of Big Data 6 (1) (2019) 69.

[^54]: J. Lemley, S. Bazrafkan, P. Corcoran, Smart augmentation learning an optimal data augmentation strategy, Ieee Access 5 (2017) 5858–5869.

[^55]: I. Goodfellow, Y. Bengio, A. Courville, Regularization for deep learning, Deep learning (2016) 216–261.

[^56]: N. Srivastava, G. Hinton, A. Krizhevsky, I. Sutskever, R. Salakhutdinov, Dropout: a simple way to prevent neural networks from overfitting, The journal of machine learning research 15 (1) (2014) 1929–1958.

[^57]: J. Kukačka, V. Golkov, D. Cremers, Regularization for deep learning: A taxonomy, arXiv preprint arXiv:1710.10686 (2017).

[^58]: Y. Li, P. Zhang, X. Xu, Y. Lai, F. Shen, L. Chen, P. Gao, Few-shot prototype alignment regularization network for document image layout segementation, Pattern Recognition 115 (2021) 107882.

[^59]: R. Girshick, J. Donahue, T. Darrell, J. Malik, Rich feature hierarchies for accurate object detection and semantic segmentation, in: Proceedings of the IEEE conference on computer vision and pattern recognition, 2014, pp. 580–587.

[^60]: G. Cheng, X. Yuan, X. Yao, K. Yan, Q. Zeng, X. Xie, J. Han, Towards large-scale small object detection: Survey and benchmarks, IEEE Transactions on Pattern Analysis and Machine Intelligence (2023).

[^61]: R. Girshick, Fast r-cnn, in: Proceedings of the IEEE international conference on computer vision, 2015, pp. 1440–1448.

[^62]: S. Ren, K. He, R. Girshick, J. Sun, Faster r-cnn: Towards real-time object detection with region proposal networks, Advances in neural information processing systems 28 (2015).

[^63]: J. Redmon, S. Divvala, R. Girshick, A. Farhadi, You only look once: Unified, real-time object detection, in: Proceedings of the IEEE conference on computer vision and pattern recognition, 2016, pp. 779–788.

[^64]: J. Redmon, A. Farhadi, Yolo9000: better, faster, stronger, in: Proceedings of the IEEE conference on computer vision and pattern recognition, 2017, pp. 7263–7271.

[^65]: J. Redmon, A. Farhadi, Yolov3: An incremental improvement, arXiv preprint arXiv:1804.02767 (2018).

[^66]: A. Bochkovskiy, C.-Y. Wang, H.-Y. M. Liao, Yolov4: Optimal speed and accuracy of object detection, arXiv preprint arXiv:2004.10934 (2020).

[^67]: YOLOv5 by Ultralytics, [https://github.com/ultralytics/yolov5](https://github.com/ultralytics/yolov5), accessed: 2023-08-31.

[^68]: C. Li, L. Li, H. Jiang, K. Weng, Y. Geng, L. Li, Z. Ke, Q. Li, M. Cheng, W. Nie, et al., Yolov6: A single-stage object detection framework for industrial applications, arXiv preprint arXiv:2209.02976 (2022).

[^69]: C.-Y. Wang, A. Bochkovskiy, H.-Y. M. Liao, Yolov7: Trainable bag-of-freebies sets new state-of-the-art for real-time object detectors, in: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2023, pp. 7464–7475.

[^70]: YOLOv8 by Ultralytics, [https://github.com/ultralytics/ultralytics](https://github.com/ultralytics/ultralytics), accessed: 2023-08-31.

[^71]: W. Liu, D. Anguelov, D. Erhan, C. Szegedy, S. Reed, C.-Y. Fu, A. C. Berg, Ssd: Single shot multibox detector, in: Computer Vision–ECCV 2016: 14th European Conference, Amsterdam, The Netherlands, October 11–14, 2016, Proceedings, Part I 14, Springer, 2016, pp. 21–37.

[^72]: F. He, N. Gao, Q. Li, S. Du, X. Zhao, K. Huang, Temporal context enhanced feature aggregation for video object detection, in: Proceedings of the AAAI Conference on Artificial Intelligence, Vol. 34, 2020, pp. 10941–10948.

[^73]: S. Lin, F. Qin, H. Peng, R. A. Bly, K. S. Moe, B. Hannaford, Multi-frame feature aggregation for real-time instrument segmentation in endoscopic video, IEEE Robotics and Automation Letters 6 (4) (2021) 6773–6780.

[^74]: D. Cores, V. M. Brea, M. Mucientes, Spatiotemporal tubelet feature aggregation and object linking for small object detection in videos, Applied Intelligence 53 (1) (2023) 1205–1217.

[^75]: X. Zhu, Y. Wang, J. Dai, L. Yuan, Y. Wei, Flow-guided feature aggregation for video object detection, in: Proceedings of the IEEE international conference on computer vision, 2017, pp. 408–417.

[^76]: G. Sun, Y. Liu, H. Ding, T. Probst, L. Van Gool, Coarse-to-fine feature mining for video semantic segmentation, in: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2022, pp. 3126–3137.

[^77]: C. Xu, J. Zhang, M. Wang, G. Tian, Y. Liu, Multilevel spatial-temporal feature aggregation for video object detection, IEEE Transactions on Circuits and Systems for Video Technology 32 (11) (2022) 7809–7820.

[^78]: S. Honari, J. Yosinski, P. Vincent, C. Pal, Recombinator networks: Learning coarse-to-fine feature aggregation, in: Proceedings of the IEEE conference on computer vision and pattern recognition, 2016, pp. 5743–5752.

[^79]: J. Guo, W. Liu, S. Xin, Z. Zhao, B. Zhang, A frame level feature aggregation method for video target detection, in: 2021 33rd Chinese Control and Decision Conference (CCDC), IEEE, 2021, pp. 1368–1373.

[^80]: S. Muralidhara, K. A. Hashmi, A. Pagani, M. Liwicki, D. Stricker, M. Z. Afzal, Attention-guided disentangled feature aggregation for video object detection, Sensors 22 (21) (2022) 8583.

[^81]: L. Han, P. Wang, Z. Yin, F. Wang, H. Li, Exploiting better feature aggregation for video object detection, in: Proceedings of the 28th ACM International Conference on Multimedia, 2020, pp. 1469–1477.

[^82]: Y. Qian, L. Yu, W. Liu, G. Kang, A. G. Hauptmann, Adaptive feature aggregation for video object detection, in: Proceedings of the IEEE/CVF Winter Conference on Applications of Computer Vision Workshops, 2020, pp. 143–147.

[^83]: Q. Zhou, X. Li, L. He, Y. Yang, G. Cheng, Y. Tong, L. Ma, D. Tao, Transvod: end-to-end video object detection with spatial-temporal transformers, IEEE Transactions on Pattern Analysis and Machine Intelligence (2022).

[^84]: N. Carion, F. Massa, G. Synnaeve, N. Usunier, A. Kirillov, S. Zagoruyko, End-to-end object detection with transformers, in: European conference on computer vision, Springer, 2020, pp. 213–229.

[^85]: Y. Wang, Z. Xu, X. Wang, C. Shen, B. Cheng, H. Shen, H. Xia, End-to-end video instance segmentation with transformers, in: Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, 2021, pp. 8741–8750.

[^86]: L. He, Q. Zhou, X. Li, L. Niu, G. Cheng, X. Li, W. Liu, Y. Tong, L. Ma, L. Zhang, End-to-end video object detection with spatial-temporal transformers, in: Proceedings of the 29th ACM International Conference on Multimedia, 2021, pp. 1507–1516.

[^87]: Z. Liu, Y. Lin, Y. Cao, H. Hu, Y. Wei, Z. Zhang, S. Lin, B. Guo, Swin transformer: Hierarchical vision transformer using shifted windows, in: Proceedings of the IEEE/CVF international conference on computer vision, 2021, pp. 10012–10022.

[^88]: Y. Cui, Feature aggregated queries for transformer-based video object detectors, in: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2023, pp. 6365–6376.

[^89]: Y. Cui, Faq: Feature aggregated queries for transformer-based video object detectors, arXiv preprint arXiv:2303.08319 (2023).

[^90]: Z. Gao, Q. Wang, Z. Pan, Z. Zhai, H. Long, Pointpainting: 3d object detection aided by semantic image information, Sensors 23 (5) (2023) 2868.

[^91]: S. Shi, X. Wang, H. Li, Pointrcnn: 3d object proposal generation and detection from point cloud, in: Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, 2019, pp. 770–779.

[^92]: S. Shi, Z. Wang, X. Wang, H. Li, Part-aˆ 2 net: 3d part-aware and aggregation neural network for object detection from point cloud, arXiv preprint arXiv:1907.03670 2 (3) (2019).

[^93]: E. Shreyas, M. H. Sheth, et al., 3d object detection and tracking methods using deep learning for computer vision applications, in: 2021 International Conference on Recent Trends on Electronics, Information, Communication & Technology (RTEICT), IEEE, 2021, pp. 735–738.

[^94]: A. H. Lang, S. Vora, H. Caesar, L. Zhou, J. Yang, O. Beijbom, Pointpillars: Fast encoders for object detection from point clouds, in: Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, 2019, pp. 12697–12705.

[^95]: Z. Tian, X. Chu, X. Wang, X. Wei, C. Shen, Fully convolutional one-stage 3d object detection on lidar range images, Advances in Neural Information Processing Systems 35 (2022) 34899–34911.

[^96]: H. Zhao, M. Tian, S. Sun, J. Shao, J. Yan, S. Yi, X. Wang, X. Tang, Spindle net: Person re-identification with human body region guided feature decomposition and fusion, in: Proceedings of the IEEE conference on computer vision and pattern recognition, 2017, pp. 1077–1085.

[^97]: T. Yin, X. Zhou, P. Krahenbuhl, Center-based 3d object detection and tracking, in: Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, 2021, pp. 11784–11793.

[^98]: H.-S. Kim, M. Won Lee, 3d object recognition using x3d and deep learning, in: The 25th International Conference on 3D Web Technology, 2020, pp. 1–8.

[^99]: X. Chen, K. Kundu, Z. Zhang, H. Ma, S. Fidler, R. Urtasun, Monocular 3d object detection for autonomous driving, in: Proceedings of the IEEE conference on computer vision and pattern recognition, 2016, pp. 2147–2156.

[^100]: T. He, S. Soatto, Mono3d++: Monocular 3d vehicle detection with two-scale 3d hypotheses and task priors, in: Proceedings of the AAAI Conference on Artificial Intelligence, Vol. 33, 2019, pp. 8409–8416.

[^101]: Y. You, Y. Wang, W.-L. Chao, D. Garg, G. Pleiss, B. Hariharan, M. Campbell, K. Q. Weinberger, Pseudo-lidar++: Accurate depth for 3d object detection in autonomous driving, arXiv preprint arXiv:1906.06310 (2019).

[^102]: H. Liu, C. Wu, H. Wang, Real time object detection using lidar and camera fusion for autonomous driving, Scientific Reports 13 (1) (2023) 8056.

[^103]: J. Ku, M. Mozifian, J. Lee, A. Harakeh, S. L. Waslander, Joint 3d proposal generation and object detection from view aggregation, in: 2018 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), IEEE, 2018, pp. 1–8.

[^104]: S. Vora, A. H. Lang, B. Helou, O. Beijbom, Pointpainting: Sequential fusion for 3d object detection, in: Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, 2020, pp. 4604–4612.

[^105]: D. Xu, D. Anguelov, A. Jain, Pointfusion: Deep sensor fusion for 3d bounding box estimation, in: Proceedings of the IEEE conference on computer vision and pattern recognition, 2018, pp. 244–253.

[^106]: E. Belouadah, A. Dapogny, K. Bailly, Multiod: Rehearsal-free multihead incremental object detector, arXiv preprint arXiv:2309.05334 (2023).

[^107]: U. Krothapalli, L. Abbott, One size doesn’t fit all: Adaptive label smoothing (2020).

[^108]: W. Lv, S. Xu, Y. Zhao, G. Wang, J. Wei, C. Cui, Y. Du, Q. Dang, Y. Liu, Detrs beat yolos on real-time object detection, arXiv preprint arXiv:2304.08069 (2023).

[^109]: H. Su, Y. He, R. Jiang, J. Zhang, W. Zou, B. Fan, Dsla: Dynamic smooth label assignment for efficient anchor-free object detection, Pattern Recognition 131 (2022) 108868.

[^110]: T. Liu, L. Zhang, Y. Wang, J. Guan, Y. Fu, J. Zhao, S. Zhou, Recent few-shot object detection algorithms: A survey with performance comparison, ACM Transactions on Intelligent Systems and Technology 14 (4) (2023) 1–36.

[^111]: W. Jin, F. Guo, L. Zhu, Incremental self-supervised learning based on transformer for anomaly detection and localization, arXiv preprint arXiv:2303.17354 (2023).

[^112]: X. Jiang, Z. Li, M. Tian, J. Liu, S. Yi, D. Miao, Few-shot object detection via improved classification features, in: Proceedings of the IEEE/CVF Winter Conference on Applications of Computer Vision, 2023, pp. 5386–5395.

[^113]: M. Köhler, M. Eisenbach, H.-M. Gross, Few-shot object detection: a comprehensive survey, IEEE Transactions on Neural Networks and Learning Systems (2023).

[^114]: A. Wu, S. Zhao, C. Deng, W. Liu, Generalized and discriminative few-shot object detection via svd-dictionary enhancement, Advances in Neural Information Processing Systems 34 (2021) 6353–6364.

[^115]: B. Kang, Z. Liu, X. Wang, F. Yu, J. Feng, T. Darrell, Few-shot object detection via feature reweighting, in: Proceedings of the IEEE/CVF International Conference on Computer Vision, 2019, pp. 8420–8429.

[^116]: Z. Shangguan, M. Rostami, Identification of novel classes for improving few-shot object detection, arXiv preprint arXiv:2303.10422 (2023).

[^117]: X. Wang, T. E. Huang, T. Darrell, J. E. Gonzalez, F. Yu, Frustratingly simple few-shot object detection, arXiv preprint arXiv:2003.06957 (2020).

[^118]: J. Wang, D. Chen, Few-shot object detection method based on knowledge reasoning, Electronics 11 (9) (2022) 1327.

[^119]: Q. Fan, C.-K. Tang, Y.-W. Tai, Few-shot video object detection, in: European Conference on Computer Vision, Springer, 2022, pp. 76–98.

[^120]: Y. Chen, Y. Cao, H. Hu, L. Wang, Memory enhanced global-local aggregation for video object detection, in: Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, 2020, pp. 10337–10346.

[^121]: K. Lee, H. Yang, S. Chakraborty, Z. Cai, G. Swaminathan, A. Ravichandran, O. Dabeer, Rethinking few-shot object detection on a multi-domain benchmark, in: European Conference on Computer Vision, Springer, 2022, pp. 366–382.

[^122]: R. Müller, S. Kornblith, G. E. Hinton, When does label smoothing help?, Advances in neural information processing systems 32 (2019).

[^123]: G. Han, Y. He, S. Huang, J. Ma, S.-F. Chang, Query adaptive few-shot object detection with heterogeneous graph convolutional networks, in: Proceedings of the IEEE/CVF International Conference on Computer Vision, 2021, pp. 3263–3272.

[^124]: S. Niklaus, L. Mai, F. Liu, Video frame interpolation via adaptive separable convolution, in: Proceedings of the IEEE international conference on computer vision, 2017, pp. 261–270.

[^125]: M. Xu, S. Yoon, A. Fuentes, D. S. Park, A comprehensive survey of image augmentation techniques for deep learning, Pattern Recognition (2023) 109347.

[^126]: H. Wu, C. Song, S. Yue, Z. Wang, J. Xiao, Y. Liu, Dynamic video mix-up for cross-domain action recognition, Neurocomputing 471 (2022) 358–368.

[^127]: P. Mangla, N. Kumari, A. Sinha, M. Singh, B. Krishnamurthy, V. N. Balasubramanian, Charting the right manifold: Manifold mixup for few-shot learning, in: Proceedings of the IEEE/CVF winter conference on applications of computer vision, 2020, pp. 2218–2227.

[^128]: A. Roy, A. Shah, K. Shah, P. Dhar, A. Cherian, R. Chellappa, Felmi: few shot learning with hard mixup, Advances in Neural Information Processing Systems 35 (2022) 24474–24486.

[^129]: Y. Nakamura, Y. Ishii, Y. Maruyama, T. Yamashita, Few-shot adaptive object detection with cross-domain cutmix, in: Proceedings of the Asian Conference on Computer Vision, 2022, pp. 1350–1367.

[^130]: J. Yoo, N. Ahn, K.-A. Sohn, Rethinking data augmentation for image super-resolution: A comprehensive analysis and a new strategy, in: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2020, pp. 8375–8384.

[^131]: C. Zhang, T. Yang, J. Weng, M. Cao, J. Wang, Y. Zou, Unsupervised pre-training for temporal action localization tasks, in: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2022, pp. 14031–14041.

[^132]: A. Aich, K.-C. Peng, A. K. Roy-Chowdhury, Cross-domain video anomaly detection without target domain adaptation, in: Proceedings of the IEEE/CVF Winter Conference on Applications of Computer Vision, 2023, pp. 2579–2591.

[^133]: V. Olsson, W. Tranheden, J. Pinto, L. Svensson, Classmix: Segmentation-based data augmentation for semi-supervised learning, in: Proceedings of the IEEE/CVF Winter Conference on Applications of Computer Vision, 2021, pp. 1369–1378.

[^134]: A. S. Chakravarthy, W.-D. Jang, Z. Lin, D. Wei, S. Bai, H. Pfister, Object propagation via inter-frame attentions for temporally stable video instance segmentation, arXiv preprint arXiv:2111.07529 (2021).

[^135]: M. Liu, S. Jin, C. Yao, C. Lin, Y. Zhao, Temporal consistency learning of inter-frames for video super-resolution, IEEE Transactions on Circuits and Systems for Video Technology 33 (4) (2022) 1507–1520.

[^136]: C. Deng, D. Chen, Q. Wu, Identity-consistent aggregation for video object detection, arXiv preprint arXiv:2308.07737 (2023).

[^137]: Y. Zhang, H. Wang, H. Zhu, Z. Chen, Optical flow reusing for high-efficiency space-time video super resolution, IEEE Transactions on Circuits and Systems for Video Technology (2022).

[^138]: J. Lin, X. Hu, Y. Cai, H. Wang, Y. Yan, X. Zou, Y. Zhang, L. Van Gool, Unsupervised flow-aligned sequence-to-sequence learning for video restoration, in: International Conference on Machine Learning, PMLR, 2022, pp. 13394–13404.

[^139]: X. Du, Y. Li, Y. Cui, R. Qian, J. Li, I. Bello, Revisiting 3d resnets for video recognition, arXiv preprint arXiv:2109.01696 (2021).

[^140]: Z. Ma, H. Zhang, J. Liu, Ms-lstm: Exploring spatiotemporal multiscale representations in video prediction domain, arXiv preprint arXiv:2304.07724 (2023).

[^141]: P. L. Jeune, A. Mokraoui, A unified framework for attention-based few-shot object detection, arXiv preprint arXiv:2201.02052 (2022).

[^142]: P. Pal, P. Chattopadhyay, M. Swarnkar, Temporal feature aggregation with attention for insider threat detection from activity logs, Expert Systems with Applications 224 (2023) 119925.

[^143]: J. Gordevičius, J. Gamper, M. Böhlen, Parsimonious temporal aggregation, in: Proceedings of the 12th International Conference on Extending Database Technology: Advances in Database Technology, 2009, pp. 1006–1017.

[^144]: Y. Fu, S. Sen, J. Reimann, C. Theurer, Spatiotemporal representation learning with gan trained lstm-lstm networks, in: 2020 IEEE International Conference on Robotics and Automation (ICRA), IEEE, 2020, pp. 10548–10555.

[^145]: S. Dai, Y. Yu, H. Fan, J. Dong, Spatio-temporal representation learning with social tie for personalized poi recommendation, Data Science and Engineering 7 (1) (2022) 44–56.

[^146]: M. Jin, Y.-F. Li, Y. Zheng, B. Yang, S. Pan, Spatiotemporal representation learning on time series with dynamic graph odes (2021).

[^147]: F. He, Q. Li, X. Zhao, K. Huang, Temporal-adaptive sparse feature aggregation for video object detection, Pattern Recognition 127 (2022) 108587.

[^148]: R. Nirthika, S. Manivannan, A. Ramanan, R. Wang, Pooling in convolutional neural networks for medical image analysis: a survey and an empirical study, Neural Computing and Applications 34 (7) (2022) 5321–5347.

[^149]: X. Luo, X. Tu, Y. Ding, G. Gao, M. Deng, Expectation pooling: an effective and interpretable pooling method for predicting dna–protein binding, Bioinformatics 36 (5) (2020) 1405–1412.

[^150]: M. Rouvier, P.-M. Bousquet, J. Duret, Study on the temporal pooling used in deep neural networks for speaker verification, in: 2021 29th European Signal Processing Conference (EUSIPCO), IEEE, 2021, pp. 501–505.

[^151]: A. Zafar, M. Aamir, N. Mohd Nawi, A. Arshad, S. Riaz, A. Alruban, A. K. Dutta, S. Almotairi, A comparison of pooling methods for convolutional neural networks, Applied Sciences 12 (17) (2022) 8643.

[^152]: A.-K. N. Vu, N.-D. Nguyen, K.-D. Nguyen, V.-T. Nguyen, T. D. Ngo, T.-T. Do, T. V. Nguyen, Few-shot object detection via baby learning, Image and Vision Computing 120 (2022) 104398.

[^153]: G. Kim, H.-G. Jung, S.-W. Lee, Spatial reasoning for few-shot object detection, Pattern Recognition 120 (2021) 108118.

[^154]: S. Zhao, X. Qi, Prototypical votenet for few-shot 3d point cloud object detection, Advances in Neural Information Processing Systems 35 (2022) 13838–13851.

[^155]: J. Liu, X. Dong, S. Zhao, J. Shen, Generalized few-shot 3d object detection of lidar point cloud for autonomous driving, arXiv preprint arXiv:2302.03914 (2023).

[^156]: S. Yuan, X. Li, H. Huang, Y. Fang, Meta-det3d: Learn to learn few-shot 3d object detection, in: Proceedings of the Asian Conference on Computer Vision, 2022, pp. 1761–1776.

[^157]: M. Guo, E. Chou, D.-A. Huang, S. Song, S. Yeung, L. Fei-Fei, Neural graph matching networks for fewshot 3d action recognition, in: Proceedings of the European conference on computer vision (ECCV), 2018, pp. 653–669.

[^158]: L. Wang, P. Koniusz, Temporal-viewpoint transportation plan for skeletal few-shot action recognition, in: Proceedings of the Asian Conference on Computer Vision, 2022, pp. 4176–4193.

[^159]: L. Wang, J. Liu, P. Koniusz, 3d skeleton-based few-shot action recognition with jeanie is not so na $\backslash$ ” ive, arXiv preprint arXiv:2112.12668 (2021).

[^160]: F. Liu, S. Yang, D. Chen, H. Huang, J. Zhou, Few-shot classification guided by generalization error bound, Pattern Recognition (2023) 109904.

[^161]: J. He, Y. Chen, N. Wang, Z. Zhang, 3d video object detection with learnable object-centric global optimization, in: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2023, pp. 5106–5115.

[^162]: G. Brazil, G. Pons-Moll, X. Liu, B. Schiele, Kinematic 3d object detection in monocular video, in: Computer Vision–ECCV 2020: 16th European Conference, Glasgow, UK, August 23–28, 2020, Proceedings, Part XXIII 16, Springer, 2020, pp. 135–152.

[^163]: J. Wu, L. Song, T. Wang, Q. Zhang, J. Yuan, Forest r-cnn: Large-vocabulary long-tailed object detection and instance segmentation, in: Proceedings of the 28th ACM international conference on multimedia, 2020, pp. 1570–1578.

[^164]: J. Wu, S. Liu, D. Huang, Y. Wang, Multi-scale positive sample refinement for few-shot object detection, in: Computer Vision–ECCV 2020: 16th European Conference, Glasgow, UK, August 23–28, 2020, Proceedings, Part XVI 16, Springer, 2020, pp. 456–472.

[^165]: D. Lee, J. Kim, Resolving class imbalance for lidar-based object detector by dynamic weight average and contextual ground truth sampling, in: Proceedings of the IEEE/CVF Winter Conference on Applications of Computer Vision, 2023, pp. 682–691.

[^166]: Z. Yu, G. Wang, L. Chen, S. Raschka, J. Luo, Few-shot learning for video object detection in a transfer-learning scheme, arXiv preprint arXiv:2103.14724 (2021).

[^167]: S. Baik, M. Choi, J. Choi, H. Kim, K. M. Lee, Meta-learning with adaptive hyperparameters, Advances in neural information processing systems 33 (2020) 20755–20765.

[^168]: J. Rajendran, A. Irpan, E. Jang, Meta-learning requires meta-augmentation, Advances in Neural Information Processing Systems 33 (2020) 5705–5715.

[^169]: C. Si, X. Nie, W. Wang, L. Wang, T. Tan, J. Feng, Adversarial self-supervised learning for semi-supervised 3d action recognition, in: Computer Vision–ECCV 2020: 16th European Conference, Glasgow, UK, August 23–28, 2020, Proceedings, Part VII 16, Springer, 2020, pp. 35–51.

[^170]: G. Yang, D. Sun, V. Jampani, D. Vlasic, F. Cole, C. Liu, D. Ramanan, Viser: Video-specific surface embeddings for articulated 3d shape reconstruction, Advances in Neural Information Processing Systems 34 (2021) 19326–19338.

[^171]: G. Huang, I. Laradji, D. Vazquez, S. Lacoste-Julien, P. Rodriguez, A survey of self-supervised and few-shot object detection, IEEE Transactions on Pattern Analysis and Machine Intelligence 45 (4) (2022) 4071–4089.

[^172]: F. Bao, G. Wu, C. Li, J. Zhu, B. Zhang, Stability and generalization of bilevel programming in hyperparameter optimization, Advances in neural information processing systems 34 (2021) 4529–4541.

[^173]: X. Luo, H. Wu, J. Zhang, L. Gao, J. Xu, J. Song, A closer look at few-shot classification again, arXiv preprint arXiv:2301.12246 (2023).

[^174]: W. Zimmer, M. Grabler, A. Knoll, Real-time and robust 3d object detection within road-side lidars using domain adaptation, arXiv preprint arXiv:2204.00132 (2022).

[^175]: Y. Wang, J. Yin, W. Li, P. Frossard, R. Yang, J. Shen, Ssda3d: Semi-supervised domain adaptation for 3d object detection from point cloud, in: Proceedings of the AAAI Conference on Artificial Intelligence, Vol. 37, 2023, pp. 2707–2715.

[^176]: J. Yang, S. Shi, Z. Wang, H. Li, X. Qi, St3d: Self-training for unsupervised domain adaptation on 3d object detection, in: Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, 2021, pp. 10368–10378.

[^177]: D. Hegde, V. Kilic, V. Sindagi, A. B. Cooper, M. Foster, V. M. Patel, Source-free unsupervised domain adaptation for 3d object detection in adverse weather, in: 2023 IEEE International Conference on Robotics and Automation (ICRA), IEEE, 2023, pp. 6973–6980.

[^178]: Q. Xu, Y. Zhou, W. Wang, C. R. Qi, D. Anguelov, Spg: Unsupervised domain adaptation for 3d object detection via semantic point generation, in: Proceedings of the IEEE/CVF International Conference on Computer Vision, 2021, pp. 15446–15456.

[^179]: J. Han, Y. Ren, J. Ding, K. Yan, G.-S. Xia, Few-shot object detection via variational feature aggregation, arXiv preprint arXiv:2301.13411 (2023).

[^180]: I. H. Sarker, Machine learning: Algorithms, real-world applications and research directions, SN computer science 2 (3) (2021) 160.

[^181]: S. S. A. Zaidi, M. S. Ansari, A. Aslam, N. Kanwal, M. Asghar, B. Lee, A survey of modern deep learning based object detection models, Digital Signal Processing 126 (2022) 103514.

[^182]: H. Wang, X. Zhang, Y. Hu, Y. Yang, X. Cao, X. Zhen, Few-shot semantic segmentation with democratic attention networks, in: Computer Vision–ECCV 2020: 16th European Conference, Glasgow, UK, August 23–28, 2020, Proceedings, Part XIII 16, Springer, 2020, pp. 730–746.

[^183]: C. Guo, B. Fan, Q. Zhang, S. Xiang, C. Pan, Augfpn: Improving multi-scale feature learning for object detection, in: Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, 2020, pp. 12595–12604.

[^184]: R. Cao, K. Zhang, Y. Chen, X. Yang, C. Jin, Point cloud completion via multi-scale edge convolution and attention, in: Proceedings of the 30th ACM International Conference on Multimedia, 2022, pp. 6183–6192.

[^185]: Z. Li, P. Xu, X. Chang, L. Yang, Y. Zhang, L. Yao, X. Chen, When object detection meets knowledge distillation: A survey, IEEE Transactions on Pattern Analysis and Machine Intelligence (2023).

[^186]: X. He, K. Zhao, X. Chu, Automl: A survey of the state-of-the-art, Knowledge-Based Systems 212 (2021) 106622.

[^187]: C. Chen, J. Wang, J. Pan, C. Bian, Z. Zhang, Graphskt: graph-guided structured knowledge transfer for domain adaptive lesion detection, IEEE Transactions on Medical Imaging 42 (2) (2022) 507–518.

[^188]: K. Liu, S. Lyu, P. Shivakumara, Y. Lu, Few-shot object segmentation with a new feature aggregation module, Displays 78 (2023) 102459.

[^189]: L. Zhao, G. Liu, D. Guo, W. Li, X. Fang, Boosting few-shot visual recognition via saliency-guided complementary attention, Neurocomputing 507 (2022) 412–427.

[^190]: B. Munjal, A. Flaborea, S. Amin, F. Tombari, F. Galasso, Query-guided networks for few-shot fine-grained classification and person search, Pattern Recognition 133 (2023) 109049.

[^191]: D. Mahapatra, Interpretable saliency maps and self-supervised learning for generalized zero shot medical image classification, arXiv preprint arXiv:2204.01728 (2022).

[^192]: W. Wang, L. Duan, Y. Wang, J. Fan, Z. Gong, Z. Zhang, A survey of deep visual cross-domain few-shot learning, arXiv preprint arXiv:2303.09253 (2023).

[^193]: J. Zhu, J. Liu, S. Yang, Q. Zhang, X. He, Open benchmarking for click-through rate prediction, in: Proceedings of the 30th ACM International Conference on Information & Knowledge Management, 2021, pp. 2759–2769.

[^194]: S. Sun, Y. Lu, S. Yu, X. Li, Z. Li, Z. Cao, Z. Liu, D. Ye, J. Bao, Rethinking dense retrieval’s few-shot ability, arXiv preprint arXiv:2304.05845 (2023).

[^195]: X. Wang, L. Lian, S. X. Yu, Unsupervised selective labeling for more effective semi-supervised learning, in: European Conference on Computer Vision, Springer, 2022, pp. 427–445.

[^196]: C. McClurg, A. Ayub, H. Tyagi, S. M. Rajtmajer, A. R. Wagner, Active class selection for few-shot class-incremental learning, arXiv preprint arXiv:2307.02641 (2023).

[^197]: Z. Chen, J. Ge, H. Zhan, S. Huang, D. Wang, Pareto self-supervised training for few-shot learning, in: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2021, pp. 13663–13672.

[^198]: J.-M. Perez-Rua, X. Zhu, T. M. Hospedales, T. Xiang, Incremental few-shot object detection, in: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2020, pp. 13846–13855.

[^199]: A. Mousavian, D. Anguelov, J. Flynn, J. Kosecka, 3d bounding box estimation using deep learning and geometry, in: Proceedings of the IEEE conference on Computer Vision and Pattern Recognition, 2017, pp. 7074–7082.

[^200]: F. Manhardt, W. Kehl, A. Gaidon, Roi-10d: Monocular lifting of 2d detection to 6d pose and metric shape, in: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2019, pp. 2069–2078.

[^201]: C. R. Qi, W. Liu, C. Wu, H. Su, L. J. Guibas, Frustum pointnets for 3d object detection from rgb-d data, in: Proceedings of the IEEE conference on computer vision and pattern recognition, 2018, pp. 918–927.

[^202]: X. Bai, Z. Hu, X. Zhu, Q. Huang, Y. Chen, H. Fu, C.-L. Tai, Transfusion: Robust lidar-camera fusion for 3d object detection with transformers, in: Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, 2022, pp. 1090–1099.

[^203]: Y. Wang, et al., Data augmentation for meta-learning, arXiv preprint arXiv:2002.08973 (2020).

[^204]: J.-Y. Franceschi, et al., A theoretical analysis of the number of samples needed to estimate information-theoretic quantities, arXiv preprint arXiv:1708.01974 (2017).

[^205]: E. D. Cubuk, et al., Randaugment: Practical data augmentation with no separate search, arXiv preprint arXiv:1909.13719 (2019).