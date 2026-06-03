---
title: "Real-Time Posture Tracking and Visual Feedback for Improving Exercise Performance Using Deep Learning"
source: "https://ieeexplore.ieee.org/abstract/document/11490277"
author:
published:
created: 2026-06-03
description: "In today's busy era, many individuals have limited access to gyms due to time or financial conditions, which leads to self-training through online resources. Ho"
tags:
  - "clippings"
---
## Abstract:

In today's busy era, many individuals have limited access to gyms due to time or financial conditions, which leads to self-training through online resources. However, inc...

---

We know that “Health is Wealth,” and taking care of our bodies requires regular exercise. Yet in today's fast-paced, work-heavy lifestyle, many people struggle to maintain their fitness. Financial limitations and lack of proper supervision are major obstacles. Exercising without guidance can lead to issues such as muscle pain, soreness, or shoulder, spine, and neck injuries-sometimes even requiring hospitalization \[1\]. Although many products offer supervised training, they tend to be expensive and mostly rely on pre-recorded or animated videos \[2\] \[3\]. Hiring a trainer or maintaining a gym membership is also costly, making proper guidance inaccessible for many.

To make self-exercising safer and more affordable, this research proposes a deep learning-based visual feedback system that works with any basic camera-equipped device. It supervises exercises by analyzing posture and showing corrective visual cues directly on the user's body. This eliminates the need for costly equipment, trainers, or subscriptions and saves valuable time.

The system captures live footage and processes it through a classification model (TCN) that identifies the workout using MediaPipe pose keypoints. Once the posture is detected, a regression model (MLP) compares the user's joint angles with pre-trained datasets. The system then draws a ghost line alongside the user's pose, helping them adjust their form to the correct angles. A key advantage of this approach is its ability to normalize posture detection for different body types and varying distances from the camera by using 3D coordinates. We also addressed occlusion challenges by applying a particle filter, resulting in smoother and more reliable tracking.

The authors of this research proposed a three-phase model combining CNN transfer learning, HPO, and image augmentation to address the challenge of training CNNs with limited data. Using this approach, they optimized AlexNet, VGG16, CNN, and MLP, achieving accuracies of 91.2% for AlexNet, 90.2% for VGG16, 87.5% for CNN, and 89.9 % for MLP using the MPII dataset \[4\]. Another study introduced an exercise-tracking system using Mediapipe with 11 selected keypoints and an MLP model for exercise recognition, providing user feedback and achieving a 16 % higher accuracy than the baseline \[5\]. A related work explored posture detection and exercise comparison using MediaPipe and OpenPose to improve user performance through real-time feedback supported by AI-based posture detection using live images and video sensors \[6\].

To address occlusion issues in detection, another study introduced a particle filter-based algorithm that estimates camera pose for vision-based tasks. It overcame traditional limitations like uncertainty and noise by integrating particle filtering for improved accuracy and robustness in real-time environments \[8\]. Similarly, another system proposed a smartphone-compatible sign-language recognition model using Mediapipe for hand landmark detection and particle filtering for smooth, frame-to-frame tracking, enabling accurate gesture recognition \[7\].

For posture correction, one study developed a webcambased system using OpenCV and Mediapipe to calculate joint angles and detect body landmarks. They verified accuracy in push-up repetitions, achieving success rates of $100 \%, 100 \%, 67 \%, 75 \%$, and 80 %, and provided real-time visual and audio feedback \[9\]. Another system, “Fitness Done Right,” used two two-branch multi-stage CNNs to detect movements, recognize exercises, and provide corrective feedback, reducing the error rate to 1.2 % \[10\]. For exercise recognition, another study proposed an efficient HAR system combining TCN and Inception modules, capturing complex temporal patterns and achieving high accuracy across several datasets. The model was later compared with other deep learning methods and showed strong performance \[11\]. Finally, MotionMixer, an MLP-based model, was introduced for forecasting 3D human body motion. Using spatial MLPs to capture joint correlations and temporal MLPs for motion dynamics, it achieved slightly better results than baseline models when tested on Human3.6M, AMASS, and 3DPW datasets \[12\].

### A. Dataset Preparation

This research is based on video data of workouts, so we needed to collect a video dataset and then annotate the videos of the dataset so that we can have the right landmark positions for use as ground truth.

#### 1) Dataset Collection

For this research, we collected publicly available videos from Kaggle datasets, YouTube, and official sites. For videos sourced from YouTube and official sites such as DareBee, Opex, and NASM. For videos that we obtained from YouTube and official sources, we contacted the respective content creators and secured permission for research use. To further enhance the dataset, we included our own videos under controlled conditions.

#### 2) Dataset Annotation

After collecting the dataset, the next step was annotation. To do so, MediaPipe combined with a particle filter pipeline was used to annotate the dataset.

#### 3) Dataset Normalization

To make our models robust across different body types and camera distances, we applied hip-centering normalization. Since torso proportions vary significantly between individuals, directly using raw coordinates can make posture detection and correction challenging. We calculate the hip center and store the relative $\mathrm{x}, \mathrm{y}, \mathrm{z}$ coordinates of 12 key landmarks (LEFT SHOULDER, RIGHT SHOULDER, LEFT ELBOW, RIGHT ELBOW, LEFT WRIST, RIGHT WRIST, LEFT HIP, RIGHT HIP, LEFT KNEE, RIGHT KNEE, LEFT ANKLE, RIGHT ANKLE).

Given the 3D landmarks from MediaPipe

$$
\begin{equation*}
L_{t}^{(k)}=\left[x_{t}^{(k)}, \quad y_{t}^{(k)}, \quad z_{t}^{(k)}\right]
\tag{1}\end{equation*}
$$
 View Source

where $k$ indexes the landmark at frame $t$, the hip center is computed as

$$
\begin{equation*}
h_{t}=\frac{1}{2}\left(l_{t}^{(\text{left}_{-}\text{hip})}+l_{t}^{(\text{right}_{-}\text{hip})}\right).
\tag{2}\end{equation*}
$$
 View Source

Each landmark is then translated into a hip-centered coordinate system:

$$
\begin{equation*}
\tilde{l}_{t}^{(k)}=l_{t}^{(k)}-h_{t}.
\tag{3}\end{equation*}
$$
 View Source

#### 4) Dataset Augmentation

To ensure the pose correction model performs effectively on real-world data, a data augmentation strategy is used during training. The model was trained not on the raw annotated landmarks data alone, but on a synthetically generated dataset designed to teach it how to handle noisy inputs. For each frame, add a small amount of Gaussian noise, and sometimes it hides the landmarks. The technique effectively trains the model as a denoising autoencoder.

#### 5) Feature Engineering

The raw mediapipe detection was used, and then the angle was calculated to feed the model. For this, 8 major angles were selected (LEFT ELBOW ANgle, RIGHT ELBOW ANGLE, LEFT SHOULDER ANGLE, RIGHT SHOULDER ANGLE, LEFT HIP ANGLE, RIGHT HIP ANGLE, LEFT KNEE ANGLE, RIGHT KNEE ANGLE). This helps the model to understand the posture more accurately.

### B. Pose Landmark Estimation using MediaPipe

MediaPipe Pose is a framework developed by Google that can detect 33 key landmarks across the entire human body. In this project, it was used to detect joint points with a visibility confidence threshold of 0.5. Each video frame, captured using OpenCV, was processed through the MediaPipe model to extract pose landmarks. Lines were drawn between detected points to visualize the skeletal structure in real time.

### C. Adding a Filter for Smooth Tracking

To improve the temporal consistency of the detected landmarks and mitigate issues like jitter or temporary occlusions, this paper used Particle Filtering. It only activates when the detection confidence of MediaPipe is less than 0.6, and the rest of the time, detection is continued using MediaPipe's detection.

#### 1) Adding a Filter for Smooth Tracking

To improve the temporal consistency of the detected landmarks and mitigate issues like jitter or temporary occlusions, this paper used Particle Filtering. It only activates when the detection confidence of Mediapipe is less than 0.6, and the rest of the time, detection is continued using Mediapipe's detection.

#### 2) Particle Filtering

A Particle Filter was implemented for each landmark to estimate its position.

- Particles: A set of NUM PARTICLES (1000) was used to represent the probability distribution of the landmark's state.
- State: Each particle represented the state as a 3D vector \[px, py, pz\]. Let the state of a landmark at time $t$ be defined as
	$$
	\begin{equation*}
	\mathbf{x}_{t}=\left[\begin{array}{l}
	p_{x} \\
	p_{y} \\
	p_{z}
	\end{array}\right], \quad \mathbf{z}_{t}=\left[\begin{array}{l}
	\hat{p}_{x} \\
	\hat{p}_{y} \\
	\hat{p}_{z}
	\end{array}\right]
	\tag{4}\end{equation*}
	$$
	 View Source
	where $\mathbf{z}_{t}$ is the observation from MediaPipe. The filter maintains $N$ particles Process:
	$$
	\begin{equation*}
	\left\{\mathbf{x}_{t}^{(i)}, w_{t}^{(i)}\right\}_{i=1}^{N}
	\tag{5}\end{equation*}
	$$
	 View Source
	where $w_{t}^{(i)}$ denotes the normalized weight.
- Initialization: For each landmark, the particles were initialized around the first high-confidence MediaPipe measurement, and weights were set uniformly.
	$$
	\begin{equation*}
	\mathbf{x}_{0}^{(i)} \sim \mathcal{N}\left(\mathbf{z}_{0}, \boldsymbol{\Sigma}_{0}\right), \quad w_{0}^{(i)}=\frac{1}{N}
	\tag{6}\end{equation*}
	$$
	 View Source

The particles are sampled around the first high-confidence measurement z0.

- Prediction Step: All particles were propagated according to the defined process model.
	$$
	\begin{equation*}
	\mathbf{x}_{t-1}^{(i)}=f\left(\mathbf{x}_{t-1}^{(i)}\right)+\boldsymbol{\eta}_{t}^{(i)}, \quad \boldsymbol{\eta}_{t}^{(i)} \sim \mathcal{N}\left(0, \boldsymbol{\Sigma}_{p}\right)
	\tag{7}\end{equation*}
	$$
	 View Source
	where $f(\cdot)$ denotes the process model (which can be the identity function if no explicit dynamics are assumed).
- Update Step: Particle weights were updated using the latest MediaPipe measurements and their associated confidence scores.
	$$
	\begin{equation*}
	w_{t}^{(i)} \propto w_{t-1}^{(i)} \cdot p\left(\mathbf{z}_{t} \vert \mathbf{x}_{t}^{(i)}\right)
	\tag{8}\end{equation*}
	$$
	 View Source
	with the Gaussian likelihood function:
	$$
	\begin{equation*}
	p\left(\mathbf{z}_{t} \vert \mathbf{x}_{t}^{(i)}\right)=c_{t} \cdot \exp \left(-\frac{1}{2}\left(\mathbf{z}_{t}-\mathbf{x}_{t}^{(i)}\right)^{T} \mathbf{\Sigma}_{m}^{-1}\left(\mathbf{z}_{t}-\mathbf{x}_{t}^{(i)}\right)\right)
	\tag{9}\end{equation*}
	$$
	 View Source
	where $c_{t}$ is the MediaPipe's visibility confidence.
- Normalization: After calculating the weight, it normalizes the value.
	$$
	\begin{equation*}
	w_{t}^{(i)}=\frac{w_{t}^{(i)}}{\sum_{j=1}^{N} w_{t}^{(j)}}
	\tag{10}\end{equation*}
	$$
	 View Source
- Estimation: The state of each landmark was estimated as the weighted average of all particles.
	$$
	\begin{equation*}
	\hat{\mathbf{x}}_{t}=\sum_{i=1}^{N} w_{t}^{(i)} \mathbf{x}_{t}^{(i)}
	\tag{11}\end{equation*}
	$$
	 View Source

### D. Classification Using TCN

This research used TCN (Temporal Convolutional Network) to classify exercises like push-ups and squats. The model analyzes short sequences (20 frames) of joints angles of 3D landmarks derived from Mediapipe, instead of using raw video.

$$
\begin{equation*}
h_{t}^{(l)}=\sigma\left(\sum_{k=0}^{K-1} W_{k}^{(l)} \cdot h_{t-d \cdot k}^{(l-1)}+b^{(l)}\right)
\tag{12}\end{equation*}
$$
 View Source

where

- $h_{t}^{(l)}$ is the hidden representation at layer $l$,
- $W_{k}^{(l)}$ are convolutional filters,
- $d$ is the dilation factor,
- $K$ is the kernel size,
- $\sigma(\cdot)$ is a non-linear activation function (ReLU).

We also applied data augmentation and handled class imbalance with class weights to make the model more reliable.

The final network includes a TCN layer, dropout to reduce overfitting, and a softmax layer for class prediction.

The model was trained with the Adam optimizer and early stopping, and it performed well in distinguishing between push-ups, squats, and no activity. Finally, we converted the model to TensorFlowLite, so it can run efficiently on lowpower devices.

### E. Correction Using MLP

To complement our classifier, we developed pose correction models for push-ups and squats using an MLP (Multi-Layer Perceptron). 2 MLP models issued to use push-ups and squats. These models learn to correct imperfect form by training on ideal 3D body landmark data that have been slightly distorted with random noise. By learning to “denoise” these data, the models can take the user's real-time movements and predict the ideal posture, providing live feedback and guidance. The model was trained in correctly performed exercises of different lengths, which allows it to learn the expected posture at each stage of the movement. Based on this, the model can predict the ideal biomechanical posture for each frame and shows it as a “Ghost Line” overlay. Whether the user is doing exercise correctly or in the wrong form, it will help the user to track whether the user is on the correct form or not.

The denoising network is an MLP (Multi-Layer Perceptron) defined as:

$$
\begin{equation*}
\mathbf{h}_{1}=\phi\left(W_{1} \tilde{\mathbf{x}}_{t}+b_{1}\right), \quad \mathbf{h}_{2}=\phi\left(W_{2} \mathbf{h}_{1}+b_{2}\right), \quad \mathbf{h}_{3}=\phi\left(W_{3} \mathbf{h}_{2}+b_{3}\right)
\tag{13}\end{equation*}
$$
 View Source

with ReLU activation $\phi(\cdot)$, batch normalization, and dropout applied after intermediate layers to improve generalization.

The output layer uses a linear activation to predict corrected landmark coordinates:

$$
\begin{equation*}
\hat{\mathbf{y}}_{t}=W_{0} \mathbf{h}_{3}+b_{0}
\tag{14}\end{equation*}
$$
 View Source

These models are built using a Multi-Layer Perceptron (MLP) with several dense layers, batch normalization, and dropout to prevent overfitting.

### F. Proposed Workflow

The workflow starts at data collection, and we divide the data into training (70 %), validation (15 %) and test (15 %). We pre-processed that data by annotating them using MediaPipe and a particle filter pipeline and used hip centering for normalizing and augmentation by adding noise to ensure generalization of the model. After that joint angles are calculated from the detected landmarks, which serve as features for training the Temporal Convolutional Network (TCN) to recognize exercises. Once preprocessing is complete, both models are trained and later evaluated on the test set. Finally, the workflow outputs exercise classification and correction, ensuring consistency across training, validation, and testing phases. The workflow of the system is shown in Fig 1.

**Fig. 1.**

WorkFlow of the system

### A. Experimental Setup

All experiments were carried out on a desktop with an Intel Pentium Core 2 Duo processor, 16 GB of RAM, and an AMD Radeon RX 580 GPU. The environment was set up using Visual Studio Code on Windows 10 (64-bit), with Python 3.10.11. MediaPipe (v0.10.9) was used for the detection model, and TensorFlow (v2.19.1) was used to implement the particle filtering, MLP, and TCN. Our experimental setup prioritized performance efficiency while balancing computational constraints. The architecture is to operate stage by stage; first it detects landmarks using MediaPipe and if occlusion occurs, it uses the Particle filter; otherwise, the MediaPipe output is used. By processing the landmark vectors (extracted via BlazePose) instead of raw video, the computational cost is reduced. TCN processes the landmarks input across 20 frames to classify, and MLP is triggered to do the forecast. Furthermore, TFLite allows the system to execute on low-spec devices also.

### B. Results

#### 1) Filter Evaluation

The performance of the raw MediaPipe detections and the filtering approach was quantitatively evaluated against the ground truth data using MPJPE, PCK, and AUC for PCK. Table I demonstrates that the Particle filtering method outperforms raw MediaPipe across all metrics, achieving better MPJPE, PCK, and AUC.

**Table I** Comparison of landmark estimation methods across MPJPE, PCK, and AUC metrics.

#### 2) Classifier

##### a) Classification Report

The classification report summarizes the model's performance on the test set. Overall accuracy is 98 %, with per-class F1-scores of 0.99 (PUSHUP), 0.98 (SQUAT), and 0.95 (NONE). Table II demonstrates that the model can correctly identify each exercise while effectively distinguishing it from other movements.

**Table II** Classification report showing precision, recall, f1-score, and support for each class.

##### b) Validation vs Test Accuracy

The model accuracy over epochs is shown in Fig 2.

**Fig. 2.**

Validation vs test accuracy

##### c) Validation vs Test Loss

The model loss over epochs is shown in Fig 3.

**Fig. 3.**

Validation vs test loss

##### d) Precision Recall Curve

The precision-recall curves further confirm the model's reliability, especially for imbalanced data. All curves are near the top-right corner, indicating high precision and recall. The Average Precision (AP) scores are 1.00 for “PUSHUP” and “SQUAT” and 0.99 for “NONE,” highlighting consistently excellent performance. The multiclass precision-recall curve is shown in Fig 4.

**Fig. 4.**

Precision recall curve

##### e) F1 Score Curve

This analysis shows the F1-score for each class across decision thresholds from 0.0 to 1.0. “PUSHUP” and “SQUAT” maintain near-perfect F1-scores (0.99) at high thresholds (0.72 and 0.82). This confirms that setting a high confidence threshold in the real-time system ensures reliable predictions. The F1 score vs. decision threshold is shown in Fig 5.

**Fig. 5.**

F1 score curve

##### f) Confusion Matrix

The confusion matrix gives a clear picture of how well the classifier performed on the test set. The strong diagonal values (486,741, and 341) show that the model correctly identified most samples of “PUSHUP,” “SQUAT,” and “NONE.” Off-diagonal values are low, meaning there was very little confusion between classes. The main misclassifications occurred when some “NONE” samples were mistaken for “SQUAT” (17 instances). Overall, this matrix highlights the model's strong accuracy and ability to distinguish between different movements. The confusion matrix is shown in Fig 6.

**Fig. 6.**

Confusion matrix

##### g) ROC Curve

The ROC curves for “PUSHUP,” “SQUAT,” and “NONE” are all positioned near the top-left corner, reflecting high true positive rates and low false positive rates. Each class achieved an AUC of 1.00, demonstrating near-perfect classification performance across all thresholds. The multi class ROC curve is shown in Fig 7.

**Fig. 7.**

ROC curve

##### h) Classfier Test Accuracy

The Classifier Test accuracy is: 97.63 %.

3) *Regression*:

1. *Mean Squared Error*:
	- For Pushup: 0.072°
		- For Squat: 0.09°
2. *Mean Absolute Error*:
	- For Pushup: 2.68°
		- For Squat: 2.86°

### C. Output

Qualitative testing showed significant benefits for beginners. For example, during push-ups, the system effectively corrected common mistakes such as keeping the arms too wide by projecting a ghost line that guides the elbows backward. This way the system shows its potential to prevent injuries caused by repeatedly performing exercises with improper form.

The outputs are visually shown in Fig 8. where the first image displays a squat, and the system successfully classifies that with a confidence of 1. The second image shows a push-up, and the system successfully classifies that with a confidence of 1. Both figures illustrate the live output of exercise detection and the result of the system.

**Fig. 8.**

Squat and pushup live

In recent years, many smart fitness products-like workout mirrors, treadmills, and weightlifting devices-have appeared on the market. However, these technologies are often too expensive for most people. While gym memberships can be affordable, hiring a personal trainer to ensure correct exercise form adds significant cost. Although some devices collect daily fitness data, they rarely provide guidance on proper exercises. In contrast, most people already own a mobile phone or laptop equipped with a camera and display. Our research proposes a cost-effective system that uses these accessible devices to detect posture through deep learning and provide real-time visual feedback. This helps users avoid injuries and exercise safely anytime, anywhere, without needing costly equipment or a professional trainer.

In its current stage, the system recognizes only two exercises-push-ups and squats-which limits its overall coverage. It also depends on environmental conditions; low light or multiple people in the frame can reduce accuracy since the model relies on 3D body coordinates. Additionally, because feedback is visual, users must keep checking the screen during their workout, which can be distracting.

Despite these constraints, the research shows strong potential. Future development could include training the model with larger datasets to support more exercises, enabling fullbody workout monitoring. A major improvement would be integrating audio feedback, allowing the system to give realtime verbal cues like “Straighten your back” or “Nice squat!” This would make the experience more intuitive, hands-free, and user-friendly, even when the screen isn't visible.