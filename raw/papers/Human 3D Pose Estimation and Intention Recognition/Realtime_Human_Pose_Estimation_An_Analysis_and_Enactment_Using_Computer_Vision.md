---
title: "Realtime Human Pose Estimation: An Analysis and Enactment Using Computer Vision"
source: "https://ieeexplore.ieee.org/abstract/document/11447550"
author:
  - "[[Vanita Babanne]]"
  - "[[Pankaj Agarkar]]"
published:
created: 2026-06-03
description: "Computer vision is the technique to used extensive in human pose estimation from real time with RGB images and videos, which had extensive application like heal"
tags:
  - "clippings"
---
## Abstract:

Computer vision is the technique to used extensive in human pose estimation from real time with RGB images and videos, which had extensive application like health monitor...

---

The Human Pose Estimation (HPE) is an essential task in computer vision that involves identifying and tracking precise key points on the human body to interpret posture and movement. These key points as the head, elbows, shoulders, hips, and knees—are crucial for recognizing various physical poses and actions. By accurately detecting these landmarks, systems can semantically understand human posture, enabling a wide range of applications in fields like healthcare, sports analysis, surveillance, and interactive technologies.

In this study, focus is placed on 2D pose estimation, where the coordinates of key points are determined in a two-dimensional space from static images or dynamic video sequences. Each pose is represented by a set of coordinates that describe the spatial arrangement of the body, commonly in the form of (x, y) pairs. However, for enhanced contextual understanding, depth (z) and visibility scores—indicating the confidence level of each landmark detection—are also considered, bringing the representation closer to a 3D format even within a 2D framework.

To achieve reliable and real-time detection of human landmarks, this system influences MediaPipe, a free framework developed by Google. MediaPipe provides an efficient pipeline for extracting 33 distinct anatomical landmarks from human figures using machine learning models. These landmarks form the basis for posture classification and feature extraction in the proposed approach.

The collected landmark data, including the image dimensions (height and width) and the coordinates of each point, are pre-processed into structured feature sets.

For classification, various machine learning algorithms were tested, and among them, the Random Forest classifier generated the greatest results, achieving an impressive accuracy score of 0.998. This highlights the model efficiency in correctly classifying human poses based on the extracted landmark features. The overall system integrates pose detection, feature selection, and classification to deliver a robust and efficient framework for human posture recognition. It demonstrates the potential of combining lightweight pose estimation tools with advanced feature optimization and classification methods for real-world pose analysis applications.

The authors \[1\] uses convolution neural Networks technique for real-time different human activities from video and estimate those. Two approaches uses first, initial preparation and second prepare input data for public data from preparation using Kinect. The proposed system is very different which is alternative to the Kinect and can be used in many activities ranging from game, health, track, behaviour and motion control of human. The paper authors \[2\] recognize many poses of yoga exercises perform by trainee. An 720P HD web camera, data is created from dissimilar location all the way through videos tested which are recorded, it consist of six yoga poses. The system divided into parts one note facts as of the media pipe and feature engineering preprocessing for training and validation of data then classification based on machine-learning approaches. System achieves an accuracy score of 94%. In \[3\] authors implemented a only one & multiple person human pose estimation with the help of MediaPipe and the OpenCV. And achieve accuracy using BlazePose GHUM 3D model for Pose Landmark to assess human pose in 2D, in terms of key points which are visible at 0.969%. In \[4\] study it present the up-and-coming and recent extent of pose of humans in AI. System particularly builds up for bowlers pose estimation. This experiment was novel and dataset of different bowlers collected by recording of events and activities using an HD camera. The proposed system for bowlers' pose to estimate, evaluate, classify, players using technique called deep learning. The recommended deep learning model (BowlingDL) for bowling and MoveNet for the poses of diverse bowlers to estimate them. The planned model gain accuracy score 80% happening training data and on testing data accuracy score 83%. Mobile application was created with BowlingDL and TensorFlow. The author \[5\], emphasize an actual and well-organized physical action like yoga intended for attractive body strength and on the whole health. For generation of database of poses, the system in a job with the help of algorithm called Adaboost and make use of a software (SDK) development kit purposely intended with regard to Kinect sensor. This planned study employs an collaborating scheme that simultaneously notice six poses of yoga execute by six persons, as long as real-time direction from beginning to end over voice, illustration show, and image shown. Specialist yoga coach shown the data gathering process, and the accuracy achieved in detecting yoga poses was 94.78%.

In \[6\], the intend of this study is to fill the hole in information and let somebody know research on 2D human posture assessment., classify them as sole or multiple person assessments is depend on the figure of people next. Gradually talk about the different approach used in human pose estimation and catalog a few applications and shortcoming come across in pose estimation. This work serve as a groundwork for novel introduction and guiding to find innovative models by identifying gaps in existing research architectures and procedures. The authors of \[7\],it utilizes deep convolutional neural networks in two distinct approaches: firstly, for end-to-end training with our RGB image dataset, employing transfer learning with ImageNet weights to classify five postures; secondly, we leverage a pre-trained deep convolutional network (pose estimator) which has demonstrated accuracy. Introducing a novel method for representation learning utilizing randomly combined, smoothly connected sets of three instances, termed blended triplets, which augment the distinguishing characteristics between recognized and unfamiliar activities\[8\],This innovative method employs Skeleton Detection technology to measure changes in skeleton points during consecutive actions. It utilizes a nearest neighbor technique to classify velocity levels and directs the ST-GCN model to infer changes in states \[9\], the methodology \[10\], employs convolutional neural networks for pose recognition, incorporating a model for localizing human joints to identify discrepancies in posture. In \[11\] authors aspire to build up a novel practice for human 3D estimation posture, imaging depth and extraction color by means of RGBD and Kinect camera. A convolution neural network to forecast the 2D human point and 3D joint point in sequence takes out from the color picture; the joint point coordinates then shift the production to the suitable image. Faster-RCNN and Resnet50 residual structure add to hourglass network meet as person goal pull out for 2D pose estimation.

Deep learning has revolutionized pose estimation by enabling the development of highly accurate and efficient models that can handle complex poses and occlusions. It \[12\] tried three experiments. This paper has trained the system which recognizes each pose of human. It is also helpful in the transformation of variations of different actions into consistent patterns. It \[13\] describes about action recognition and image processing. It takes one dataset and examines the performance and accuracy. The dark videos are divided into images. It \[14\] gives training for videopose3D. It requires less amount of data for human activity. Action based and post estimation problems can be worked. One approach \[15\] utilizes a stateless video-length adaptive input data generator, while the other investigates the stateful capability of recurrent neural networks, particularly utilized in HAR. This approach enables the prototypical to gather to extract discriminative patterns from previous frames while preserving memory integrity. The \[16\] author states that hierarchical classification of poses and recommend a extensive pose dataset called Yoga-82. Author \[17\] stated the comparison of OpenPose, PoseNet, and MoveNet and observe the key points detection as a key difference between these models. In\[18\] the author work on real time and prerecorded videos for posture detection on multiple person and achieve good accuracy. A pose estimation model that notices numerous key points in the human body with moveNet model\[19\]. HPE can be leveraged to develop sports analytics, personalized working out, and self-learning systems, enabling players, sportspersons, and trainers to enhance training quality by analyzing human postures noticed from images or videos\[20\]. yoga and kavayat (YK) has challenges that are addressed in real time\[21\]. Critically review the literature survey and identify that prior research focus on adult dataset and in environment controlled with static images and leaving notable real time gap, child specific application. Here the current method relies on CNN lightweight architecture capture accurate movements. This PoseHeatMap \[22\] method computationally efficient and relevant to improve training and assessment and safety in youth physical activity.

This System based on apprehending the video frame and detection the correct human action using machine learning approach. Mock drill(kavayat) proposed system designed with the help of computer vision to simulate real-world states for evaluation and estimation purposes. This system leverages computer vision techniques to monitor and recognize actions during drills, allowing actions for automatic assessment and its performance metrics. For model performance metrics like accuracy evaluated, which offering insights into its ability to accurately classify various activities.

A lightweight and actual pipeline for categorizing poses of children based on 2D skeletal landmarks using a convolutional neural network (CNN). Method influences in fig. 1 Media Pipe key point extraction by 33 anatomical landmarks, converts the extracted joint coordinates into grayscale spatial heatmaps, and practices a CNN with three convolutional layers with 32,64, and filters 128 correspondingly, each come after by Batch Normalization and Max Pooling layers come after by Global Average Pooling (GAP) reduces the overfitting of model by replacing the fully connected layer. In the last dense layer with 64 neurons followed by Dropout (rate $=0.3$). Softmax output layer for multi-class classification. model is accumulated by means of the Adam optimizer with categorical cross-entropy loss. Parameter adjustment directed over 10 epochs through a batch size of 8. The model achieves a high classification accuracy of 99%, representative its efficiency in unique among visually different poses.

### A. System Architecture

Keypoint detection of each keypoint returns normalized coordinates like n belongs to

$$
\begin{equation*}
P_{n}=\left(x_{n}, y_{n}, z_{n}\right) \in\{0,1\}
\tag{1} \end{equation*}
$$
 View Source

In equation [(1)](#deqn1)

- xn: horizontal (width-wise) location
- yn: vertical (height-wise) location
	**Fig. 1.**
	System architecture
- zn: relative depth or distance from the camera
- $\in [0, 1]$: These coordinates are normalized:

Instead of being in pixels scaled between 0 and 1 with respect to the image frame size.

For example:

- xn=0 means far left, xn=1 means far right
- yn=0 means top, yn=1 means bottom
- zn=0 is the closest depth; positive values indicate increasing distance from the camera.

Normalized coordinates mean the device independent works on different images which support real time application. Machine learning algorithm named Logistic regression is which classifies the tasks in binary numbers. Given a set of input features p1, p2, p3,…pn. Output will be given in the combination of 0 and 1. Each keypoint changed into the heatmap centered (Xn, Yn).

**Table I.** Data set used

The Proposed PoseHeatNet based model, utilizing grayscale heatmaps derived from 33 anatomical keypoints via MediaPipe, achieved stable convergence within 10 training epochs. The loss curves and validation metrics suggest that the model effectively learns discriminative features from pose-specific joint patterns. The high classification accuracy (typically above 90% on clean datasets) demonstrates the robustness of using pose landmarks over raw RGB images, especially in controlled settings like (YK) yoga or Kavayat (mockdrill) environments.

MediaPipe extracted 33 keypoints is a structured representation of body pose often required for this method as shown in fig. 2. the keypoints 33 with very good accuracy. background-invariant based solely on human anatomy, not the image scene. Some advanced models employ deep CNN architectures. In variation, this work highlights simplicity and efficiency by focusing solely on spatial information derived from 2D skeletal landmarks, using lightweight CNN layers. While benchmark datasets like MPII and COCO typically involve fewer body landmarks (16 or 17), our model utilizes all 33 key points provided by Media Pipe, letting for a richer and more detailed depiction of human position. A key distinction of this work is its emphasis on identifying poses performed by children. Since children exhibit higher variability in body proportions and movement accuracy, developing a model that accommodates these variations adds practical value, particularly in educational and fitness-related contexts such as digital yoga trainers or physical activity monitors in school.

By combing the Heat map allows CNN to learn the pose structure and spatial pattern which improve accuracy over the vector-based input. Compare with CNN understand the orientation, symmetry, and spatial layout whereas Heatmap adopt the different body proportion gives the highest accuracy. The Proposed PoseHeatNet method capture fine grained spatial and geometric characteristics of Yoga and Kavayat(YK) which integrated the pipeline to achieve the high accuracy, robustness, and generalization on child specific pose data.

**Table II.** Quantitative evaluation of different pose classification methods.

**Fig. 2.**

Output of system

**Table III.** Qualitative comparison of the proposed cnn-heatmap approach with other pose classification techniques

**Fig. 3.**

Model accuracy and loss

The model on Human Action Pattern Recognition in Physical Training for human environments have explored the integration of advanced technologies, including computer vision, deep learning, to recognize and classify diverse human actions within Physical Training settings. Leveraging the study developed a system which is capable of real-time action recognition, providing feedback sound in the form of the predicted exercise name. By successfully developing an intelligent system PoseHeatMap capable of recognizing human actions in real time, PoseHeatMap system has paved the way for transformative applications in the fitness industry. This technology not only enhances the Physical Training experience for individuals but also holds promise for applications in healthcare, sports training. The implemented system is for single user at a time with state of art on training and testing model on eight different classes with 0.98% to 0.99% accuracy.