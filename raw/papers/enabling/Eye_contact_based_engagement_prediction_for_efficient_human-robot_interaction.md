---
title: "Eye contact based engagement prediction for efficient human–robot interaction - Complex & Intelligent Systems"
source: "https://link.springer.com/article/10.1007/s40747-025-01902-z"
author:
  - "[[Magnus Jung]]"
  - "[[Ahmed Abdelrahman]]"
  - "[[Thorsten Hempel]]"
  - "[[Basheer Al-Tawil]]"
  - "[[Qiaoyue Yang]]"
  - "[[Sven Wachsmuth]]"
  - "[[Ayoub Al-Hamadi]]"
published: 2025-05-12
description: "This paper introduces a new approach to predict human engagement in human–robot interactions (HRI), focusing on eye contact and distance information."
tags: [Journal, Springer, Publication]
---
## Abstract

This paper introduces a new approach to predict human engagement in human–robot interactions (HRI), focusing on eye contact and distance information. Recognising engagement, particularly its decline, is essential for successful and natural interactions. This requires early, real-time user behavior detection. Previous HRI engagement classification approaches use various audiovisual features or adopt end-to-end methods. However, both approaches face challenges: the former risks error accumulation, while the latter suffer from small datasets. The proposed class-sensitive model for capturing engagement in HRI is based on eye contact detection. By analyzing eye contact intensity over time, the model provides a more robust and reliable measure of engagement levels, effectively capturing both temporal dynamics and subtle behavioral changes. Direct eye contact detection, a crucial social signal in human interactions that has not yet been explored as a standalone indicator in HRI, offers a significant advantage in robustness over gaze detection and incorporates additional facial features into the assessment. This approach reduces the number of features from up to over 100 to just two, enabling real-time processing and surpassing state-of-the-art results with 80.73% accuracy and 80.68% F1-Score on the UE-HRI dataset, the primary resource in current engagement detection research. Additionally, cross-dataset testing on a newly recorded dataset with the Tiago robot from Pal Robotics achieved an accuracy of 86.8% and an F1-score of 87.9%. The model employs a sliding window approach and consists of just three fully connected layers for feature fusion and classification, offering a minimalistic yet effective architecture. The study reveals that engagement, traditionally relying on extensive feature sets, can be inferred reliably from temporal eye contact dynamics. The results include a detailed analysis of established engagement levels on the UE-HRI dataset using the proposed model. Additionally, models for more nuanced engagement classification are introduced, showcasing the effectiveness of this minimalistic feature set. These models provide a robust foundation for future research, advancing robotic systems and deepening understanding of HRI, for example by improving real-time social cue detection and creating adaptive engagement strategies in HRI.

## Introduction

In Human–Robot Interaction (HRI), accurate user state estimation and secure data processing are essential \[[^12], [^14]\]. There is often a trade-off between computational complexity and real-time capability, as a slightly inaccurate but fast estimate is usually preferable to a delayed computation—unlike in sensitive areas such as medical diagnostics \[[^15]\]. In addition, decision making in HRI requires dealing with uncertainties in environmental and user data that classical state machines often cannot handle adequately. Here, fuzzy methods \[[^7], [^13]\] or LLMs \[[^43]\] offer more flexible and robust solutions. However, the progressive integration of humanoid robots into complex scenarios of HRI requires accurate observation and understanding of how users are feeling and responding. Achieving humanoid robot behavior that is similar to human behavior, and thus intuitive, depends crucially on recognizing and imitating social interactions among humans \[[^31], [^70]\]. This paves the way for establishing a social connection between humans and robots. Continuous feedback between human behavior and the robot’s behavior plays a key role in this process. Human engagement prediction is a fundamental aspect in designing intuitive, natural interactions and cooperation between social robots and humans \[[^21], [^23], [^49], [^63]\].

In human–robot interaction (HRI), engagement is the sustained level of interest, attention, and emotional involvement that a participant exhibits during an interaction. Engagement includes both attentional and emotional components \[[^59]\]. Attentional involvement refers to how individuals initiate, maintain, and terminate their connection with the robot \[[^67]\]. Emotional involvement reflects the intensity of emotions expressed during the interaction \[[^60]\]. High levels of emotional expression are typically associated with higher levels of engagement \[[^26], [^32], [^60]\]. Consequently, declining engagement refers to all observable signs that a participant’s engagement is decreasing, e.g. decreasing concentration, less involvement or emotional withdrawal.

Automated engagement detection involves monitoring various social cues during interactions, such as gaze, head pose, body posture, and distance (see Table [1](https://link.springer.com/article/10.1007/s40747-025-01902-z#Tab1)). These features are crucial for understanding and measuring engagement levels in interactive settings, highlighting the complexities involved in tracking and interpreting these cues effectively. However, reliance on extensive and diverse sensor inputs can complicate the design and operation of socially aware robots \[[^29], [^56]\]. Furthermore, incorporating these signals into engagement prediction algorithms presents challenges, as it often requires pre-existing systems for estimating these cues, which may not be accurate and require high computational costs \[[^10], [^40], [^41]\]. In addressing these challenges, two main approaches emerge: feature-based methods and end-to-end approaches. Feature-based methods leverage extracted features from the input data, such as features derived from signal processing or computer vision techniques \[[^9],[^10],[^11], [^52]\]. These methods offer advantages, particularly for small datasets and models intended for deployment in unfamiliar scenarios. Algorithms for feature extraction improve the generalization capability with less complex models. On the other hand, end-to-end approaches learn representations directly from raw data, bypassing the need for handcrafted features in the training data \[[^3], [^23], [^64]\]. While these methods can potentially capture complex patterns in the data, they may require larger datasets and computational resources for training. Additionally, end-to-end approaches may lack interpretability compared to feature-based methods.

Eye contact is a fundamental element of non-verbal communication that encompasses more than just the orientation of one’s eyes (gaze) \[[^42]\]. It includes subtle elements such as facial expressions and the focus of the counterpart, signaling the beginning or end of an interaction, and fading attention \[[^22], [^37]\]. Indeed, eye contact involves a range of non-verbal cues including facial expressions and attention, which signal various communication intents such as initiating or ending an interaction, or indicating shifting attention levels \[[^19], [^37], [^42], [^66], [^76]\]. Building on this, we argue that, for engagement in the context of this study, eye contact plays a crucial role and can serve solely as a comprehensive indicator to predict engagement levels, replacing all other visual features. This approach would allow us to streamline the process by reducing the need to predict various cues such as gaze, head pose, and emotions. As a result, the system’s complexity is simplified. In addition, focusing on eye contact could reduce computational costs and enhance the feasibility of real-time applications.

In this paper, we propose a novel end-to-end framework for human engagement prediction that primarily utilizes eye contact as the key indicator, streamlining the integration of nonverbal communication cues into engagement detection methods. Eye contact, in combination with proxemic information (distance to each other), is identified as the most decisive feature in predicting engagement. Our framework aims to overcome the challenges associated with traditional engagement prediction models by minimizing the dependency on extensive sensor inputs and complex feature sets. Our model leverages the nuanced aspects of eye contact to predict engagement levels effectively. By focusing on eye contact, we reduce the need of analyzing multiple separate cues like gaze direction, head pose, or emotional expressions, thus significantly simplifying the computational requirements.

Initially, our framework involves detecting faces frame-by-frame from RGB image sequences using a sliding window technique. Subsequently, for each face crop, we compute an eye contact value, which quantifies the directness and intensity of gaze indicative of engagement. These computed values are then fed into a neural network that classifies the level of engagement based on the eye contact values only. We evaluate our framework against state of the art methods on UE-HRI dataset \[[^8]\]. The results reveal that our framework outperforms the state of the art approaches not only in accuracy but also in efficiency.

After a preliminary discussion on the correlation between eye contact and engagement in “Correlation between eye contact and engagement” section, our framework is introduced in “EyeConNet framework” section, followed by an explanation and discussion of the experiments in “Experiments and results” section. Finally, “Discussion and conclusion” section presents a comprehensive discussion of the results and outlines directions for future research.

## Related works

Understanding engagement in HRI is crucial as robots become more sophisticated. Researchers employ diverse methods, from rule-based to machine learning, to predict engagement stages and breakdowns. However, accurately detecting engagement decrease, especially in real-time, remains challenging, necessitating innovative techniques and robust datasets for improved interactions.

### Engagement in HRI

Engagement detection \[[^6], [^35], [^39], [^47], [^48], [^51]\] in HRI becomes increasingly significant as robotic capabilities advance. It shows how effectively a robot demonstrates socially acceptable behaviors and actions which influence the quality of HRI \[[^20]\]. Engagement in HRI consists of two main stages: engagement initiation and engagement decrease. Engagement occurs when humans initiate and maintain perceived interactions with robots, generally starting when an intention to engage with the robot is detected from the human \[[^52]\]. On the other hand, engagement decrease happens when humans are about to terminate their interactions with the robot. The goal of detecting and predicting engagement decreases is to achieve interaction goals and avoiding disinterest which is challenging in spontaneous social interactions and public environments. This difficulty arises due to the high level of individual social behavior, potential distractions, and the freedom of interaction partners to spontaneously leave the interaction \[[^9], [^23], [^39]\].

At the outset, early research \[[^4], [^51], [^57]\] focused on understanding interaction issues and predicting of user engagement stages such as point of engagement, sustained engagement, disengagement, and re-engagement. Recent studies delve into predicting engagement breakdowns and conversation termination risks \[[^9], [^52]\]. Current research focuses on classifying engagement changes, with a particular emphasis on predicting decreases to dynamically adapt robot behavior \[[^8], [^10], [^64]\].

Predicting engagement is continuous and crucial within an interaction, as the engagement level continually changes and shifts through the interaction. To address this continuously, a feedback loop must exist between the robot’s actions and the user’s engagement level \[[^49]\]. Therefore, engagement detection, especially recognizing its decline, forms the basis for successful and natural interaction between humans and robots, requiring early and timely recognition of the user’s engagement level \[[^10]\]. Engagement detection relies on observing a period of time which requires the analysis of a sequence of data to achieve good classification results \[[^9], [^10], [^16], [^52], [^62], [^64]\].

Early approaches utilize rule-based methods \[[^28], [^62]\], statistical methods \[[^16]\] and probabilistic models \[[^16], [^17]\] to predict engagement. However, they face the challenge of overly frequent changes in their estimates and they tend to be too conservative, making them task-specific. Machine learning methods have marked a pivotal shift toward predicting accurate engagement\[[^64]\] including logistic regression \[[^9]\], boosted decision trees \[[^17]\], Support Vector Machines (SVM) \[[^20]\]. For instance, Bohus et al. \[[^17]\] developed self-supervised forecasting models without manual annotations, demonstrating their effectiveness in predicting disengagement with a low false-positive rate. Leite et al. \[[^51]\] proposed SVM-based models for disengagement prediction during HRI, highlighting that single-user models may lack generalizability to group interactions. However, their combined model integrating single-user and group interaction models outperformed both individual models. However, it was shown that the rule-based classifier performed comparably to the group of supervised classifiers trained on a limited labeled dataset \[[^28]\].

Advances in deep learning have significantly improved the accuracy of engagement prediction. They replaced earlier methods with advanced models such as feature-based multilayer perceptrons (MLP) \[[^9]\], recurrent neural networks (RNNs) \[[^52]\], deep neural networks (DNNs) \[[^52]\] and long-short-term memory networks (LSTMs) \[[^10], [^11]\]. For instance, Liu et al. \[[^52]\] proposed an echo state network (ESN), a variant of RNNs, to learn the relationship between facial expressions and engagement breakdown behavior. The model achieved promising accuracy results for single-user interactions. Ben-Youssef et al. \[[^10]\] adopt a binary classification approach for real-time detection through prediction of user engagement decrease during interactions with a humanoid robot. They Leverage LSTM network to analyze multimodal user behavior features extracted from observation windows. However, these methods require access to large and diverse datasets, which poses significant challenges and involves high costs. Additionally, real-time capability cannot be achieved with these methods, as the computational cost for processing numerous features is too high.

End-to-end CNNs and hybrid neural networks, such as combinations of end-to-end residual network (ResNet) and temporal convolutional network (TCN), are designed to estimate the engagement of interaction partners by directly processing sequential image data, without the need for prior feature extraction \[[^3], [^64]\]. This method circumvents the need for feature calculation and directly processes image data, significantly reducing computation time compared to feature-based methods, thus enabling real-time capabilities. Additionally, the end-to-end approach often results in enhanced accuracy compared to feature-based methods that depend solely on visual data \[[^64]\]. For instance, Saleh et al. \[[^64]\] propose End-to-End Spatio-Temporal Convolutional Neural Network for engagement prediction decrease. These methods require complex networks to extract crucial engagement information from small regions of images and primarily rely on visual data. Another challenge is the brevity of a few seconds of some social interactions and the limited availability of training data, which impacts their generalizability.

**Table 1 Used streams for engagement classification in HRI**

**Table 2 Number of features used in current studies for classifying engagement states in HRI**

Existing research underscores the complexity of predicting engagement decrease and breakdowns even for humans \[[^38]\]. Engagement is recognized as a multimodal issue that is not directly measurable, typically addressed through combinations of manually created, observable features spanning seven key groups: gaze, head pose, facial expressions, posture, speech, distance, and others (e.g. semantic output, attention inference output, manual annotation, semantics, attention, emotions and affects, Game state, displaying an affective reaction) as shown in Table [1](https://link.springer.com/article/10.1007/s40747-025-01902-z#Tab1). These can be categorized into three groups: Visual features, including gaze, head pose, facial expressions and posture; Auditory features, including speech; and proxemic features. Current feature-based approaches enable accurate predictions of engagement changes based on a large number of feature sets (up to 127 features \[[^9]\], as shown in Table [2](https://link.springer.com/article/10.1007/s40747-025-01902-z#Tab2)). However, many of these features (e.g., emotion, pose, speech) require extensive feature extraction processes. This not only leads to increased computational overhead and error accumulation but also poses challenges in creating usable databases for engagement detection. The selection and extraction of features can be challenging as they may be tailored to specific tasks, and some features may not scale well for a wide range of HRI scenarios \[[^64]\]. In addition, the dimensionality of the feature vector in current feature-based approaches increases the requirements in terms of the type and quality of the sensors.

### Datasets

Engagement detectors and classificators are crucial not only in HRI but also in diverse settings such as monitoring attention during online meetings and large events \[[^25], [^65]\]. Engagement detection and classification heavily rely on the creation and annotation of new datasets to define engagement states, a process that is not only costly but also requires expertise from trained annotators.

The process of dataset creation often involves the complex and time-consuming task of manually annotating communication cues and readiness for interaction, which is further complicated by the subjective nature of assessing emotional cues and can lead to redundancies \[[^8], [^10], [^17], [^28], [^51]\]. In HRI, data collection poses unique challenges as it requires an egocentric perspective from the robot, not a stationary camera, and often additional sensors to handle the dynamic nature of interactions \[[^8], [^11]\]. These challenges significantly limit the availability of usable datasets for engagement detection in HRI. Among the available datasets (Bohus et al. \[[^17]\], Leite et al. \[[^51]\], Vaufreydaz et al. \[[^72]\], Vernissage \[[^73]\]), the UE-HRI dataset is notable for meeting the highlighted criteria, its large number of publicly available recorded interactions, broad range of features, and diverse scenarios in the wild, making it a primary resource for current research in this field \[[^8], [^73]\]. Figure [1](https://link.springer.com/article/10.1007/s40747-025-01902-z#Fig1) presents several images from the dataset, illustrating various situations and challenges within the UE-HRI dataset.

### Eye contact

Gaze is the most used feature for engagement detection in HRI in recent work as seen in Table [1](https://link.springer.com/article/10.1007/s40747-025-01902-z#Tab1). Following is head pose, this feature is highly correlated with gaze \[[^2], [^36]\]. It reflects that gaze is one of the most important features in engagement prediction, but the analysis of direct eye contact is still missing. Human–human interactions serve as a model for human–robot interaction (HRI), and the effects of eye contact can be similarly transferred \[[^76]\]. Kendon \[[^45]\] observed that speakers tend to look at each other during fluent speech and at the end of sentences, and the mutual eye contact diminishes when focus wanes. Kompatsiari et al. \[[^48]\] also investigated the influence of the robot’s eye contact with the interaction partner. The results of subjective reports demonstrated that participants were sensitive to eye contact and established a stronger connection with the robot. These social signals are relevant information for evaluating engagement during interaction. Liu et al. \[[^52]\] exclusively used facial expression features to predict engagement breakdowns, emphasizing the importance of facial features, which are also crucial when considering eye contact. In other works until now (e.g., \[[^9], [^10]\], etc.) that utilize gaze as a feature, it is assumed that this information provides a sufficient understanding of eye contact. As outlined in \[[^37]\], models for determining gaze direction are not robust and accurate enough to reliably determine eye contact. Additionally, deriving eye contact from gaze direction always requires manually setting a threshold that defines the boundary between eye contact and no eye contact. However, this can be situation-dependent and may either lead to unintended errors or be meaningful only for very specific cases, if the determination of gaze direction are precise enough.

**Fig. 1**

![Fig. 1](https://media.springernature.com/lw685/springer-static/image/art%3A10.1007%2Fs40747-025-01902-z/MediaObjects/40747_2025_1902_Fig1_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1007/s40747-025-01902-z/figures/1)

Exemplary images from the UE-HRI dataset \[[^8]\] depicting various engagement states and challenges

Accurate measurement of eye contact using additional devices is not feasible for HRI with autonomous robots. This requires a specialised AI model based on available sensor data that can not only detect the gaze direction of the interaction partner from an egocentric perspective, but also take into account facial expressions and social cues. Currently, there are only three publicly available datasets that address this indirectly through manual annotation: NITEC \[[^37]\], DEEPEC \[[^54]\], and OFDIW \[[^77]\]. Among these, the NITEC dataset is the largest dataset explicitly labelled with the premise of focusing not only on gaze direction but also on intentional eye contact \[[^37]\]. The baseline models trained on this dataset demonstrate the high quality of the dataset and its applicability to HRI.

## Correlation between eye contact and engagement

Before introducing our method, we first explore the existing correlation between eye contact and engagement. We address the critical role of eye contact as a key feature in detecting engagement levels, highlighting its importance, which extends beyond simple gaze direction. We demonstrate the influence of facial expressions on the recognition of eye contact in real-world situations and discuss how this insight leads to more efficient engagement detection through a streamlined architecture that reduces the number of necessary features.

### Eye contact detection

Eye contact is a main aspect of social human–human interactions between partners \[[^42], [^48]\]. It serves as a fundamental element of nonverbal communication, encompassing more than just the orientation of one’s eyes \[[^42]\]. Eye contact includes subtle elements such as facial expressions and the focus of the counterpart, signaling the beginning or end of an interaction and indicating fading attention \[[^22], [^37]\]. This study harnesses the power of direct eye contact recognition in social contexts, specifically excluding verbal utterances. This approach is particularly relevant in scenarios like shared work environments, where eye contact remains uninterrupted by other activities \[[^39], [^55]\]. However, the focus on engagement detection within HRI has predominantly been on gaze direction. Binary features like “Looking at the robot” have been employed. However, nuanced aspects of eye contact, varying in intensity, have not been addressed in previous studies.

Therefore, we have adapted a state-of-the-art eye contact detection model based on ResNet18 \[[^34]\], trained on the publicly available NITEC dataset \[[^37]\], which is currently the largest freely accessible eye contact dataset, created and published by us. This model outputs a probability value between 0 and 1 for eye contact, delivering excellent results with reasonable computational effort and speed, making it ideal for mobile robots. It has been trained on diverse and well-aligned in-the-wild data from an egocentric perspective, ensuring a situation-independent and robust assessment of eye contact across various challenging conditions such as unusual head poses, individuals wearing glasses, dense groups, and different lighting conditions.

Furthermore, the model accounts for social components like a social smile, which increases the probability of eye contact detection. These social cues are integrated into the prediction process due to the hand-annotated nature of our NITEC dataset. Eye contact is annotated only when the annotator perceives a sense of eye contact with the camera or the annotator. Consequently, gazes that drift into empty space or pass nearby without establishing eye contact are not annotated as eye contact. Moreover, the training data cover diverse real-world scenarios, incorporating images where gaze direction alone may not suffice to determine eye contact, as potential gaze targets are densely clustered. In such cases, additional cues need to be learned to to discern whether eye contact is established or if attention is directed elsewhere. This enhances the model’s ability to distinguish between genuine eye contact associated with communication interest and an unfocused gaze into space, which is not considered eye contact. These capabilities are qualitatively illustrated in Fig. [2](https://link.springer.com/article/10.1007/s40747-025-01902-z#Fig2) and underscore the model’s sophisticated approach to detecting eye contact by considering additional facial features beyond just gaze direction. The inclusion of non-binary resolution in eye contact probability further incorporates crucial social cues, thus increasing detection reliability.

**Fig. 2**

![Fig. 2](https://media.springernature.com/lw685/springer-static/image/art%3A10.1007%2Fs40747-025-01902-z/MediaObjects/40747_2025_1902_Fig2_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1007/s40747-025-01902-z/figures/2)

The detection of eye contact using the ResNet18 eye contact model, trained on our NITEC dataset \[[^37]\], is significantly influenced by facial expressions. For instance, smiling increases the probability of eye contact, as qualitatively observed in the comparison between (**a, b**), as well as (**c, d**). The impact of facial expressions is also evident in the visual comparison of Gradient Class Activation Maps \[[^30]\]. By covering the eyes in (**c, d**), the focus is further emphasized on this area

**Table 3 Definitions of different engagement classes in human–robot interaction as described in the UE-HRI dataset \[[^8]\] and used in this paper. The table presents different levels of engagement and disengagement, including full engagement (ENG), signs of engagement decrease (SED), early signs of a future engagement breakdown (EBD), actual engagement breakdown (BD), and temporary disengagement (TD), highlighting the behavioural indicators associated with each category**

### Correlation between eye contact and engagement in the UE-HRI dataset

The UE-HRI dataset \[[^8]\] consists of egocentric recordings of interactions between humans and the social robot Pepper from SoftBank Robotics. These interactions were captured in a public space (university corridor), allowing participants to engage and leave freely. The dataset includes 195 spontaneous, unscripted interactions, 54 are publicly available. Each interaction starts when a person enters the robot’s engagement zone (1.5 ms around the robot) and ends when they exit this zone. If the participant confirms the agreement displayed on the robot’s screen, the data is saved and transmitted to the server \[[^8]\].

Various factors influence data acquisition, such as distance and information transfer via the robot’s screen, significantly affecting interactive behavior. Distance as a proxemic information, a crucial social cue \[[^10]\], is actually not rigidly defined in the wild.

Annotations encompass manually annotated signs of engagement decrease (SED) and categories like early signs of future engagement breakdown (EBD), engagement breakdown (BD), and temporary engagement decrease (TD). The definitions are provided in Table [3](https://link.springer.com/article/10.1007/s40747-025-01902-z#Tab3). Detection features for negative emotions and information on single-user or multi-user situations were also considered.

In the 54 publicly available interactions (32 single-user and 22 multi-user interactions), the RGB camera recorded at variable frequencies between 4.9 and 10.4 Hz (between 4.9 and 10.4 fps), while the front sonar (for the proxemic information) recorded frequencies from 1.5 to 3.4 Hz.

In the present dataset, analyses of the interaction partner’s gaze behavior are already available, specifically the moments of initiating and terminating gaze towards the robot. However, this is a binary value that does not provide information about the certainty or distance of the gaze towards the robot’s head. Additionally, the quality of this gaze data cannot be verified with this dataset.

In this work we are following Lalmas et al. \[[^50]\] definition of engagement, that engagement involves an emotional, cognitive and behavioral connection that can be built and maintained through interpersonal or human–robot signals but can also be (unilaterally) dissolved. Various degrees of attention and interaction readiness are possible in this context. In order to be able to use the pre-annotated data and remain comparable with other works, we follow the definition provided by Ben-Youssef et al. \[[^8]\] for SED, EBD, BD and TD.

A particular challenge in engagement classification within the UE-HRI dataset, especially when using eye contact, is that during interaction, information is presented on the display, and attention may be directed towards it, potentially causing certain humanoid features of the robot to be overlooked.

Another challenge, commonly encountered in reality and present in this dataset, is missing (eye contact) values due to occlusions and self-movements of the robot, leading to individuals moving out of the field of view. Furthermore, the low image quality, attributed to the chosen sensors with a resolution of 640x480 pixels and limited contrast and sharpness, poses additional obstacles for analysis. Moreover, accurate annotation of engagement proves to be challenging as the initiation and termination points of notable sequences are not clearly defined.

**Fig. 3**

![Fig. 3](https://media.springernature.com/lw685/springer-static/image/art%3A10.1007%2Fs40747-025-01902-z/MediaObjects/40747_2025_1902_Fig3_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1007/s40747-025-01902-z/figures/3)

Boxplots of corresponding eye contact values to engagement labels for individual RGB frames. The eye contact values are predicted with the NITEC-18 Model, a ResNet-18 \[[^34]\] model trained on our NITEC eye contact dataset \[[^37]\] (see “Eye contact detection” section). Firstly, considering frames without faces (for example, due to the robot’s own movement or the interaction partner turning away) and without considering them (**a, b**), as well as considering all class-related frames and frames derived from sequences consisting only of frames of the same class (**c, d**)

A notable advantage of eye contact as the sole feature is demonstrated in the SED cues recorded in the UE-HRI dataset. These social behaviors exhibit strong redundancy, with eye contact and its temporal dynamics being one of the most distinctive cues.

**Table 4 Results of multiple comparisons of means with Tukey HSD (FWER = 0.01). In both cases (with and without exclusion of images where no face could be detected) all classes show significant differences from each other. Therefore, a correlation between data within the same class across different experiments (with similar conditions) could be expected**

The expected results are promising, based on the statistical distribution of eye contact values in the engagement level sequences. An attenuation of eye contact intensity for decreasing engagement can already be observed from the distribution in Fig. [3](https://link.springer.com/article/10.1007/s40747-025-01902-z#Fig3). For this statistical analysis, the eye contact values are predicted with the state-of-the-art NITEC-18 model, a ResNet-18 \[[^34]\] model trained on our NITEC eye contact dataset \[[^37]\] (see “Eye contact detection” section). An analysis of variance (ANOVA) shows \[[^69]\] that in each of the cases shown in [3](https://link.springer.com/article/10.1007/s40747-025-01902-z#Fig3) at least one class is significantly different from the others ($p=0.000$ and $F>10000$). A Tukey-HSD (honestly significant difference) test \[[^71]\] was performed for a more detailed examination of the statistical distinguishability between the classes. Table [4](https://link.springer.com/article/10.1007/s40747-025-01902-z#Tab4) shows the results for [3](https://link.springer.com/article/10.1007/s40747-025-01902-z#Fig3) (c) and (d). All classes show significant differences from each other (significance level: $\alpha =0.01$). The mean differences indicate that discriminating between TBD and BD is more difficult when frames with unrecognised faces are not excluded, whereas discriminating between BD and EBD becomes more difficult when these frames are excluded. A qualitative differentiation between TBD and BD is also evident by more intense eye contact aversion during TBD compared to BD, since momentary distractions by other people or multimedia devices occur during TBD (cf. Fig. [1](https://link.springer.com/article/10.1007/s40747-025-01902-z#Fig1)), temporarily claiming complete attention, but only briefly interrupting.

## EyeConNet framework

We propose EyeConNet framework, an innovative framework to predict engagement in real time. Figure [4](https://link.springer.com/article/10.1007/s40747-025-01902-z#Fig4) shows the whole pipeline of the proposed EyeConNet framework, which consists of four components: (1) Input sequence, (2) face detection, (3) eye contact detection and (4) engagement classification. Initially, face detection is performed frame-wise on the RGB images of a sequence, and the faces are cropped. Subsequently, an eye contact value is determined for each face crop. These values serve as the starting point for engagement classification using a fully connected neural network.

### Input sequence

The determination of engagement is based on observing sequences over time using a sliding window approach. These sequences not only include frames with ground truth for a specific class but also allow for changes within the sequence. Essential for labeling the sequence is the ground-truth label for the frame determined by the buffer length, as depicted in Fig. [5](https://link.springer.com/article/10.1007/s40747-025-01902-z#Fig5). This methodology is consistent with the approach outlined in Ben-Youssef et al.’s paper \[[^10]\], where sequences and buffer play integral roles in analyzing engagement dynamics.

In our experimental setup, meticulous attention is given to determining the precise sequence and buffer lengths. These parameters are vital for capturing meaningful temporal patterns. Figure [5](https://link.springer.com/article/10.1007/s40747-025-01902-z#Fig5) illustrates the temporal evolution of the eye contact value, alongside the trend of the distance value of the front sonar and segmented representations of various engagement classes. As each new frame is added, the analyzed sequence shifts. Simultaneously, the EyeConNet framework endeavors to estimate the engagement class at the buffer point where the ground truth label is determined. This dynamic process facilitates a real-time assessment of engagement levels, adapting to evolving interactions and behavioral patterns.

The assessment of engagement must be done in real-time practice. This is ensured by relatively short sequence lengths, simple and therefore fast models, and the considered classes such as SED as the detection of early signs of specific behavioral patterns rather than just the detection of behavioral patterns, allowing for early intervention and guidance or prevention.

**Fig. 4**

![Fig. 4](https://media.springernature.com/lw685/springer-static/image/art%3A10.1007%2Fs40747-025-01902-z/MediaObjects/40747_2025_1902_Fig4_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1007/s40747-025-01902-z/figures/4)

Proposed EyeConNet framework for engagement classification using eye contact. Initially, face detection is performed frame-wise on the RGB images of a sequence, and the faces are cropped. Subsequently, an eye contact value is determined for each face crop. These values serve as the starting point for engagement classification using a fully connected neural network. For subsequent sequences, only the newly added RGB image needs to have the face cropped, enabling improved performance

**Fig. 5**

![Fig. 5](https://media.springernature.com/lw685/springer-static/image/art%3A10.1007%2Fs40747-025-01902-z/MediaObjects/40747_2025_1902_Fig5_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1007/s40747-025-01902-z/figures/5)

Exemplary representation of signals for eye contact and distance across frames. In this case, a sequence length of 7 is depicted with a buffer length of 4 (number of frames until the ground truth label). The ground truth of the sequence is the label of the frame at the buffer length

The data is processed directly frame by frame, eliminating the need for pre-processing before feeding it into the pipeline. This approach simplifies the analysis process as the first steps of the pipeline only need to be performed for the next image in the series. This efficiency is made possible by storing the processed images so that the previously calculated data can continue to be used in the ongoing analysis.

### Face detection

To generate the eye contact feature, faces must first be detected on the RGB frames. For this purpose, a face detector based on RetinaFace is employed \[[^24]\]. RetinaFaceMobileNetV1 is a variant of the RetinaFace face detection model based on the MobileNetV1 architecture. It is specifically designed to accurately locate faces in images while maintaining high speed and efficiency (up to 485 fps \[[^68]\]). This detector initially locates all faces in the RGB image (cf. Fig. [4](https://link.springer.com/article/10.1007/s40747-025-01902-z#Fig4)) and determines a facial crop. To ensure high-quality detections, only those with a confidence score exceeding 0.5 and a resolution greater than $10\times 10$ pixels are selected.

### Eye contact detection

The facial crops extracted from the face detection model undergo a series of transformations to prepare them for processing in the eye contact detection module. Initially, the selected facial crops undergo a 20additional contextual information, such as head pose. Following this expansion, the crops are resized to dimensions of $224\times 224$ pixels. Finally, they are normalized, with mean values of \[0.485, 0.456, 0.406\] and standard deviations of $[0.229, 0.224, 0.225]$ across each color channel.

To accurately compute the eye contact feature across all detected faces, we train the NITEC-18 model, which is based on the ResNet-18 architecture \[[^34]\]. This model has been specifically designed with two output neurons to predict eye contact in binary representation from an egocentric perspective. The training is conducted on our NITEC dataset \[[^37]\], employing a binary cross-entropy loss function, which is well-suited for our binary output. The binary cross-entropy loss $L_B$ is computed based on the ground truth labels $y_i$, the predicted probabilities $p_i$, and the number of samples *N*:

$$
\begin{aligned} L_B=- \frac{1}{N} \sum _{i=1}^N \left( y_i log(p_i)+(1-y_i)log(1-p_i)\right) \end{aligned}
$$

(1)

The training data consists of normalized facial crops, augmented with random cropping and horizontal flipping. For optimization, the Adam optimizer \[[^46]\] is employed, chosen for its effectiveness in handling sparse gradients on noisy problems. We set the learning rate to 0.0001, optimizing the balance between training speed and accuracy. The model is trained for 20 epochs, with a batch size of 80. This model outperformed state of the art models for eye contact detection, such as Chong et al..’s \[[^22]\], and gaze estimation methods with gaze direction boundaries for eye contact estimation, like Gaze360 \[[^44]\] or L2SC-Net \[[^2]\], on the freely available datasets for eye contact detection (NITEC \[[^37]\], OFDIW \[[^77]\] and DEEPEC \[[^53]\]).

We utilize the trained NITEC-18 to determine the degree of eye contact for each face crop, with the output value ranging between 0 and 1. The feature for each frame is the eye contact value in that image. In the case of a multi-user scene, the maximum eye contact value is selected under the assumption that the corresponding individual is the primary user. In cases where no face is detected or none meets the specified criteria, the eye contact feature for that frame is denoted as $-1$.

The NITEC-18 model enables reliable detection while considering other facial expressions and head poses without redundancy. Neglecting redundant information leads to more stability and robustness in the classifications of deep learning algorithms, reducing model complexity \[[^61]\]. This results in improved runtime and reduced resource requirements due to fewer features. This is particularly advantageous for small datasets, enhancing results’ solidity and generalization capability.

**Table 5 Engagement detection model architecture for classifying engagement values from sequence of eye contact features. The architecture is adaptable to different numbers of engagement classes and can be extended by further input features**

**Table 6 Distribution of subjects from teh UE-HRI dataset across 3 folds, ensuring separate test and train sets with different individuals in each fold to avoid overfitting and demonstrate the generalizability of the results. The table also shows the number of interactions excluded due to missing annotations**

### Engagement detection

The eye detection values for each frame are input into the final classification network, which consists of three fully connected layers paired with Rectified Linear Unit (ReLU) activations, as outlined in Table [5](https://link.springer.com/article/10.1007/s40747-025-01902-z#Tab5).

The input size of the network is determined by the product of the number of features and the sequence length, while the output size corresponds to the number of classes to be differentiated. The model employs a cross-entropy loss function ($L_C$), where $\hat{y}$ represents the predicted scores for each class for each input, *y* represents the ground truth class labels, *N* is the number of samples in the batch, and *C* is the number of classes. The loss function is defined as follows:

$$
\begin{aligned} L_C(\hat{y}, y) = -\frac{1}{N} \sum _{i=1}^{N} \log \left( \frac{exp(\hat{y}_{i, y_i})}{\sum _{j=1}^{C} exp(\hat{y}_{i, j})} \right) \end{aligned}
$$

(2)

Training is conducted using the Adam optimizer \[[^46]\], which is initialized with a learning rate of 0.001. The training duration was adjusted and optimized throughout the course of the experiment.

Built on these principles, the proposed network is trained using minimal features extracted from a simple video stream captured by an RGB camera positioned at the head area of a humanoid robot. This setup is then used with real-time capability for engagement prediction in HRI scenarios. The method is designed to be flexible, allowing for the inclusion of additional features such as distance information, which can provide added value in certain scenarios with specific constraints. With eye contact as the primary feature, the approach supports the creation of fundamentally more naturalistic HRI scenarios.

## Experiments and results

To validate the effectiveness of the EyeConNet framework in predicting engagement, extensive experiments have been conducted using the UE-HRI dataset. The initial focus was on classifying SED and ENG, which is then expanded to include other categories such as EBD, TBD, BD, and various practical combinations of these engagement levels (cf. Table [10](https://link.springer.com/article/10.1007/s40747-025-01902-z#Tab10)). We evaluate the results using both binary and multi-class classifiers and compare them with state of the art approaches. To demonstrate the performance advantages of our proposed EyeConNet model, we present the classification results obtained through 3-fold cross-validation. These results are compared with the state-of-the-art approaches.

**Table 7 This table illustrates the uneven distribution of annotated classes among participants in the UE-HRI dataset**

### Experiments setup

The experiments utilized the UE-HRI dataset in its current form and avoids additional adjustments to the data or annotations (e.g. no sequence merging).

We conduct two main experiments one solely considering eye contact as a feature, and additionally, considering the distance as a proxemic information determined by the front sonar, thereby we have two models EyeConNet and EyeConNet-D, respectively. This was done because the setup artificially introduces distance as a determining parameter influencing the interaction.

The information was associated with individual frames or the eye contact data derived from the frames. The annotation assignment is based on the timestamp of the frames, matched with the time intervals of the listed intervals.

### Evaluation criteria

To evaluate the results, participants were divided into 3 folds (cf. Table [6](https://link.springer.com/article/10.1007/s40747-025-01902-z#Tab6)), and the experiments were conducted using a 3-fold cross-validation. This ensures that no participant’s data appears in both the test and training sets, ensuring independence between them to avoid overfitting and to allow demonstration of generalizability. A challenge arises from the significant discrepancy in the number of annotated sequences and their length between participants (cf. Table [7](https://link.springer.com/article/10.1007/s40747-025-01902-z#Tab7)), resulting in a nearly identical number of participants in the folds (14, 14, 13). However, the number of labeled data varies considerably. The number of sequences per class depends on the sequence and buffer length (sliding window). A solid data foundation is available for sequence lengths under 20 frames (with overlapping sequences and sequences consisting of frames from not only one class). To ensure an even distribution of classes in the training and test sets, sequences were randomly removed from the sets, achieving a balanced distribution of classes through downsampling.

After training the models on sequences with mixed classes, testing is conducted on both cross-class and pure class sequences. The influence of the distance of the participants on the results is also considered (cf. Table [8](https://link.springer.com/article/10.1007/s40747-025-01902-z#Tab8)).

### Performance metric

To provide a comprehensive analysis, the accuracy and F1 scores are evaluated separately. Accuracy and F1 scores offer different perspectives on classification performance. Accuracy gauges the overall correctness of predictions across all classes, whereas F1 score provides a balanced measure by considering both precision and recall:

$$
\begin{aligned} \begin{aligned} Accuracy&= \frac{TP + TN}{TP + TN + FP + FN}\\ Precision&= \frac{TP}{TP + FP}\\ Recall&= \frac{TP}{TP + FN}\\ F1&= \frac{2 \times Precision \times Recall}{Precision + Recall}\\ \end{aligned} \end{aligned}
$$

(3)

Although accuracy is susceptible to biases in imbalanced datasets, the F1 score offers a more nuanced evaluation, particularly beneficial for scenarios with uneven class distributions. This dual assessment ensures a comprehensive evaluation of the efficacy of the model across various classification challenges.

To further improve model evaluation, we introduce sensitivity and specificity, focusing on the detection of the Sign of Engagement Decrease (SED):

$$
\begin{aligned} \begin{aligned} Sensitivity&=\frac{TP}{TP+FN}\\ Specificity&=\frac{TN}{TN+FP}\\ \end{aligned} \end{aligned}
$$

(4)

Sensitivity, or recall, measures the ability of the model to correctly identify SED cases. High sensitivity indicates effective detection of SED cases among all actual SED occurrences. Specificity assesses the accuracy of the model in identifying non-SED cases, highlighting its ability to avoid false positives. High specificity means that the model effectively discriminates between SED and non-SED cases, minimising misclassification of non-SED as SED.

### Quantitative results

The evaluation of EyeConNet against state-of-the-art engagement estimation methods demonstrates its competitive accuracy in predicting engagement stages. By comparing with end-to-end and feature-based techniques, EyeConNet achieves promising results, especially when considering the real-time capability.

**Table 8 Results of the classification of SED and ENG sequences for class-specific and class-crossing sequences using only eye contact, as well as eye contact and distance, compared to state-of-the-art approaches \[[^9], [^10], [^64]\] (the early detection method utilizes the implementation from \[[^9]\] for SED detection, with LR standing for Logistic Regression. All results are based on a 3-fold cross-validation)**

#### Comparison with SOTA

The accuracy of EyeConNet is evaluated against several state-of-the-art gaze estimation methods. The comparison includes a variety of approaches that utilize end-to-end approach and feature based techniques. For instance, 3D Light-Inception” \[[^64]\] provides the best results for SED detection based on visual data from the UE-HRI dataset, with limited access to the published part of the UE-HRI data. As a second end-to-end approach for comparison with the results, the ConvNet-LSTM model \[[^23]\] is employed. This model also works directly with RGB sequences and has been replicated with a sequence length of 30 frames. To align with our classification task, the last layer was replaced with a softmax activation function, transforming it from a regression to a classification task \[[^64]\]. We also consider the results of Ben-Youssef et al. from “On-the-fly Detection of User Engagement Decrease in Spontaneous Human–Robot Interaction” \[[^10]\], which includes a variety of features, including non-visual ones, and could use the entire dataset. It delivered the best results in the detection of SED sequences compared to ENG sequences. For an additional comparison, including considerations of real-time capability, the results of this model are also utilized in a modified form \[[^10]\], focusing solely on RGB features. We refer to this approach as “OpenFace-LSTM”. A direct comparison with early engagement detection, as used in \[[^9]\], can only be indirect, as this work considers only the annotated data and their classification, and only an indirect conclusion about the classes EBD and BD of these annotations can be drawn.

**Fig. 6**

![Fig. 6](https://media.springernature.com/lw685/springer-static/image/art%3A10.1007%2Fs40747-025-01902-z/MediaObjects/40747_2025_1902_Fig6_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1007/s40747-025-01902-z/figures/6)

Confusion matrix and precision for the classification between SED and ENG for class-pure sequences with a sequence length of 15 and a buffer length of 13

When examining the binary classification between SED and ENG, considering the used data, it becomes evident that even with existing results, visual data (in a end-to-end approach) alone achieves almost as good results \[[^64]\] (Accuracy: 72.21) as using a broader feature basis (Accuracy: 78.56 \[[^10]\]). However, the RGB feature-based model OpenFace-LSTM yields significantly poorer results, clearly indicating the absence of crucial RGB information in the features of OpenFace-LSTM. By reducing visual data in experiment 1 to the feature of eye contact, which already considers social features, significant improvement and increased generalization of the EyeConNet model are observed (Accuracy: 78.3). The NITEC-18 eye contact model is based on a large amount of data, allowing for environment-independent feature extraction. The trained model is therefore less complex, leading to better generalization ability due to a less situation-dependent data situation than the end-to-end approach of Saleh et al. \[[^64]\]. The results in Table [8](https://link.springer.com/article/10.1007/s40747-025-01902-z#Tab8) quantify the advantages of this approach. Our NITEC dataset contributes to incorporating additional information relevant to engagement into the model. A more in-depth analysis for the direct classification of engagement levels based on face crops, using a model trained on datasets such as NITEC, could still provide promising results.

The improvements achieved by experiment 2 (EyeConNet-D) by integrating distance as a proxemic feature (Accuracy: 80.73) surpass previous results and are mostly attributable to the structure of the database, although they only marginally manifest in the results. The confusion matrix for this classification, along with the precision for each class, is shown in Fig. [6](https://link.springer.com/article/10.1007/s40747-025-01902-z#Fig6). It should be noted that the distance information could possibly be estimated through a purely visual approach from the image data.

The results show that the model has a sensitivity of 83.9% for detecting SED, indicating a strong ability to accurately identify cases of SED. This high sensitivity reflects the effectiveness of the model in capturing the majority of actual SED occurrences, minimising the number of missed cases. Conversely, the specificity of the model is 77.5%, demonstrating its ability to correctly identify instances of ENG. Overall, the model performs well in detecting declines in engagement, with strong class sensitivity.

The model comparison between the On-the-Fly model \[[^10]\] and our EyeConNet-D is made using the Wilson confidence interval \[[^74]\]. A direct comparison of the models by significance testing is not possible due to the different databases (different sliding windows). The significance test is performed by calculating and comparing the Wilson score significance intervals. The On-the-Fly model has an confidence interval of \[0.7746, 0.7966\] with a confidence level of $85\%$ (significance level $\alpha =0.15$), while EyeConNet-D has an confidence interval of \[0.7967, 0.8179\]. As these intervals do not overlap, the results are significantly different at $\alpha =0.15$, indicating that the model based on eye contact and distance offers a significant improvement in the detection of engagement.

#### Cross-dataset evaluation

In the absence of other suitable publicly available datasets for cross-dataset evaluation, we collected a new dataset using the TIAGo robot from PAL Robotics \[[^58]\] in a Wizard of Oz study conducted in our robotics lab with nine participants. A video of up to 4 min was recorded for each participant. The task consisted of the participant interacting with the robot and asking it to retrieve and hand over a cube. This resulted in several key interaction scenarios, including the initial engagement, the robot turning away from the conversation to retrieve the object, the re-engagement for the handover, the farewell, as well as delays in the robot’s responses that caused the participant to become restless and look around for help with several declines in engagement.

Recordings were taken using the TIAGo robot’s internal RGBD camera at a resolution of 640x480 at 31.8 Hz. Engagement was manually annotated by a single annotator using the same criteria as the UE-HRI dataset. Importantly, the data was reserved for testing only, ensuring that the context and environment were unknown to the model, thus eliminating any risk of overfitting.

With the best model (our EyeConNet-D with sequence length of 15 and buffer length of 13) trained on the UE-HRI dataset, the results on the new dataset showed an accuracy of 86.8% and an F1 score of 87.9%, surpassing the performance on the original dataset. This improvement is attributed to the more consistent and reduced motion of the robot, the constant frame rate of the recordings, and fewer external variables as the study was conducted in a controlled environment rather than in a more unpredictable real-world setting. This dataset serves as a critical resource for evaluating the model’s ability to generalize.

#### Computational complexity

The consideration of real-time capability in the classification also reveals advantages of the proposed models EyeConNet and EyeConNet-D (up to 45 fps) over other RGB feature-based models (16 fps \[[^64]\]) and achieves better frame rate as the 3D light-inception end-to-end model (25 fps \[[^64]\]) (cf. Table [9](https://link.springer.com/article/10.1007/s40747-025-01902-z#Tab9)). The models results were obtained on a PC with Intel Core i7-12700K and Nvidia GeForce GTX 1080 Ti GPU (due to the face detection approach, the computation time scales with the number of faces in the frame.).

**Table 9 The runtime evaluation on the Nvidia GeForce GTX 1080 Ti GPU demonstrates that our feature-based EyeConNet model has a better runtime as the end-to-end model by Saleh et al. \[[^64]\] and significantly better runtime characteristics than other feature-based models like the models by Ben-Youssef et al. \[[^9], [^10]\], even when these models exclusively use RGB-features**

### Comprehensive analysis

The optimization of the sliding window approach for the EyeConNet model involved varying sequence lengths and buffer sizes, revealing the optimal configuration for engagement prediction. Through qualitative analysis, it was observed that shorter sequence lengths did not significantly impact binary classifications, while longer lengths improved multiclass classification accuracy. Additionally, the model demonstrated promising capabilities in distinguishing between multiple further engagement stages, providing valuable insights for real-time interaction systems.

**Fig. 7**

![Fig. 7](https://media.springernature.com/lw685/springer-static/image/art%3A10.1007%2Fs40747-025-01902-z/MediaObjects/40747_2025_1902_Fig7_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1007/s40747-025-01902-z/figures/7)

Results of the classification between SED and ENG for different sequence lengths and buffer lengths for cross-class sequences

**Table 10 Results (accuracy and F1) of different classification combinations for engagement estimation of humans in HRI. It is visible that classes, assuming an ordinal distribution, which are further apart are better distinguishable. For real-world applications, the change in engagement is relevant, and thus a class sensitive perception is provided here. These multi-class classification models represent a baseline for further work**

#### Sliding window

To optimize the underlying sliding window approach (cf. Fig. [5](https://link.springer.com/article/10.1007/s40747-025-01902-z#Fig5)), our EyeConNet model was trained with various sequence lengths and buffer sizes. Therefore, feature vectors from eye contact (and distance) sequences between 1 considered frame to 20 frames were coupled and the buffer length can be between frame 1 and the first frame after the sequence. Overlapping sequences are permissible and increase the number of available sequences to generate a larger database for the models.

Sequence length and buffer length were varied in the experiment following the example of \[[^10]\] also to make the influence on the prediction evident. A qualitative examination of the results, as shown in Fig. [7](https://link.springer.com/article/10.1007/s40747-025-01902-z#Fig7), reveals a decline in classification results for sequence lengths beyond 16 frames ($\sim 1.5s$). Predominantly, the best results are obtained with sequence lengths of 15 frames or slightly less. Shorter sequences do not significantly impact the accuracy of binary classifications. However, more complex multiclass classifiers require a longer sequence length to achieve good results. The buffer length, on the other hand, has a smaller influence, especially for testing on class-pure sequences, with the difference being minimal. It can still be generalized, particularly concerning non-class-pure sequences in the test set, that a longer lead time for the sequence tends to lead to better classification results. The required time, i.e., sequence length and thus delayed classification, does not compromise real-time capability, which is essential for engagement detection. Especially SED and EBD are early signs of engagement termination, allowing the model to analyze the situation in advance and predict TBD and BD. The classification of TBD and BD is also feasible with very short sequences, as the results suggest.

Distinguishing between SED and EBD is challenging, as the results show, but it is also less relevant in real scenarios. However, when EBD and SED are considered together and compared with ENG sequences, distinguishing between these class combinations is possible with an accuracy of 81.34. On the other hand, distinguishing between BD and TBD can be important in practice. The classification between these two is challenging and error-prone. However, using eye contact features, the distinction between these classes could achieve an accuracy of 74.48.

#### Multi class

Examining multi-class problems also leads to interesting and valuable insights as seen in Table [10](https://link.springer.com/article/10.1007/s40747-025-01902-z#Tab10). When engagement is divided into 3 classes—ENG, SED and EBD, and TBD and BD—these classes can be well-differentiated (Accuracy: 72.54). This division seems to be the most fundamental for intuitive interactions. Examining the results of binary classifiers reveals that the separation between SED and EBD, as well as between TBD and BD, is particularly challenging. The maximum possible division of engagement levels on the UE-HRI dataset is into 5 classes. Here, an accuracy of 52.03 percent is still possible using eye contact features with a sequence length of 13 and a buffer length of 3 (a confusion matrix could be found in Fig. [8](https://link.springer.com/article/10.1007/s40747-025-01902-z#Fig8)). Accuracy losses in this case also result from the similarity between SED and EBD, as well as TBD and BD, as evident in the challenges of classifying sequences in the confusion matrix. Misclassifications primarily occur between the similar classes of SED and EBD, as well as between BD and TBD, rather than between classes that are further apart. This indicates that predicting behavior with this model for interaction-relevant mechanisms is possible, enabling real-time responses. By continuously assessing the engagement level of the interaction partner, conversation trajectories can be predicted and manipulated through feedback loops between the behavior of the interaction partner and the humanoid robot. Additional class combinations can also be found in Table [10](https://link.springer.com/article/10.1007/s40747-025-01902-z#Tab10). These multi-class classification models and their results represent a competitive baseline for further work.

**Fig. 8**

![Fig. 8](https://media.springernature.com/lw685/springer-static/image/art%3A10.1007%2Fs40747-025-01902-z/MediaObjects/40747_2025_1902_Fig8_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1007/s40747-025-01902-z/figures/8)

Confusion matrix for the classification of all 5 engagement classes for a sequence length of 13 and a buffer length of 3 frames

## Discussion and conclusion

Two class sensitive models have been introduced: EyeConNet and EyeConNet-D. EyeConNet, based solely on eye contact as a single feature derived from RGB images, provides state-of-the-art results in engagement estimation. EyeConNet-D, based on both eye contact as a single visual feature and distance as a proxemic information, surpasses state-of-the-art results in engagement estimation. Both models are real-time capable and outperform state-of-the-art models in computational efficiency.

Eye contact proves to be a crucial feature for engagement, particularly through the robust and versatile model based on our NITEC dataset, which allows reliable differentiated classification of eye contact. Thus, it can be demonstrated that in similar contexts of the UE-HRI scenario, engagement can be inferred from the temporal dynamics of eye contact. This is due, in part, to eye contact containing more than just the direction of the gaze, as facial expressions and demeanor play relevant roles. The challenge of eye-contact detection is very similar to determining engagement, and the issues overlap significantly.

In addition to outstanding results for already investigated engagement levels, additional classifiers for more challenging class combinations have been presented, establishing a baseline for further research. This serves as a foundation for advancing toward more intuitive human–robot interaction.

For future research on engagement, it is promising to explore the use of eye contact-based engagement detection models in different contexts, such as online education, zoom calls, and other HRI scenarios. The social context is crucial and requires model adaptation to these specific conditions. Further studies are needed to thoroughly evaluate the adaptability and effectiveness of such models in these diverse scenarios, with long-term insights expected from future longitudinal studies.

Given the importance of eye contact as a visual feature, our future studies will also investigate the impact of additional features in different scenarios. In HRI contexts where interaction involves not only direct communication but also collaborative work without direct eye contact, non-visual features may become important. Engagement could be influenced by aspects such as linguistic elements.

In summary, our EyeConNet-D model represents a significant advance in engagement detection due to its exceptional quality and efficiency. It introduces direct eye contact estimation as a key feature in HRI and shows that the recognition and interpretation of eye contact is not only essential in human interactions, but can and should also be applied to HRI. Combining the integration of eye contact with proxemic (distance) information enables a more accurate and comprehensive assessment of engagement. The real-time capability and enhanced efficiency of our EyeConNet-D provide critical advantages for practical implementation in real-world engagement detection scenarios, marking a remarkable advance in the optimization of human–robot interactions.

Our future research will focus on extending sensitive engagement detection as a fundamental element of HRI. We aim to refine real-time adaptation mechanisms that allow robots to dynamically adjust their behavior based on deeper contextual understanding. In addition, we will explore self-learning systems that continuously improve engagement prediction through long-term human interaction data. By pushing the boundaries of socially intelligent robotics, we aim to create robots that are not only more responsive and intuitive, but also capable of forming meaningful and lasting social relationships.

## Data Availability

The UE-HRI dataset \[[^8]\] is only partially publicly available. This part was utilized for this research and can be accessed with registration via the following link: [https://adasp.telecom-paris.fr/resources/2017-05-18-ue-hri](https://adasp.telecom-paris.fr/resources/2017-05-18-ue-hri). The NITEC dataset \[[^37]\] is fully publicly available and can be accessed via the following link: [https://github.com/thohemp/nitec](https://github.com/thohemp/nitec).

## References

## Acknowledgements

This work is funded and supported by the German Research Foundation (DFG) (SEMIAC under Grant Number No. 502483052), by the Federal Ministry of Education and Research of Germany (BMBF) (AutoKoWAT-3DMAt under Grant No. 13N16336) and by the European Regional Development Fund (ERDF) (ENABLING under Grant No. ZS/2023/12/182056).

## Funding

Open Access funding enabled and organized by Projekt DEAL.

## Ethics declarations

### Conflict of interest

The authors declare that they have no Conflict of interest or Conflict of interest.

### Ethics approval, consent to participate and consent for publication

Not applicable, as existing and publicly available data sets were used. This study does not contain any studies with human or animal subjects performed by any of the authors.

## Additional information

### Publisher's Note

Springer Nature remains neutral with regard to jurisdictional claims in published maps and institutional affiliations.

## Rights and permissions

**Open Access** This article is licensed under a Creative Commons Attribution 4.0 International License, which permits use, sharing, adaptation, distribution and reproduction in any medium or format, as long as you give appropriate credit to the original author(s) and the source, provide a link to the Creative Commons licence, and indicate if changes were made. The images or other third party material in this article are included in the article’s Creative Commons licence, unless indicated otherwise in a credit line to the material. If material is not included in the article’s Creative Commons licence and your intended use is not permitted by statutory regulation or exceeds the permitted use, you will need to obtain permission directly from the copyright holder. To view a copy of this licence, visit [http://creativecommons.org/licenses/by/4.0/](http://creativecommons.org/licenses/by/4.0/).

[^1]: Abdelrahman AA, Strazdas D, Khalifa A, Hintz J, Hempel T, Al-Hamadi A (2022) Multimodal engagement prediction in multiperson human-robot interaction. IEEE Access 10:61980–61991

[Article](https://doi.org/10.1109%2FACCESS.2022.3182469) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Multimodal%20engagement%20prediction%20in%20multiperson%20human-robot%20interaction&journal=IEEE%20Access&doi=10.1109%2FACCESS.2022.3182469&volume=10&pages=61980-61991&publication_year=2022&author=Abdelrahman%2CAA&author=Strazdas%2CD&author=Khalifa%2CA&author=Hintz%2CJ&author=Hempel%2CT&author=Al-Hamadi%2CA)

[^2]: Abdelrahman AA, Hempel T, Khalifa A, Al-Hamadi A, Dinges L (2023) L2CS-NET: fine-grained gaze estimation in unconstrained environments. In: 2023 8th international conference on frontiers of signal processing (ICFSP). IEEE, pp 98–102

[^3]: Abedi A, Khan SS (2021) Improving state-of-the-art in detecting student engagement with RESNET and TCN hybrid network. In: 2021 18th conference on robots and vision (CRV). IEEE, pp 151–157

[^4]: Andrist S, Bohus D, Kamar E, Horvitz E (2017) What went wrong and why? Diagnosing situated interaction failures in the wild. In: Social robotics: 9th international conference, ICSR 2017, Tsukuba, Japan, November 22–24, 2017, Proceedings 9. Springer, pp 293–303

[^5]: Anzalone SM, Boucenna S, Ivaldi S, Chetouani M (2015) Evaluating the engagement with social robots. Int J Soc Robot 7:465–478

[Article](https://link.springer.com/doi/10.1007/s12369-015-0298-7) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Evaluating%20the%20engagement%20with%20social%20robots&journal=Int%20J%20Soc%20Robot&doi=10.1007%2Fs12369-015-0298-7&volume=7&pages=465-478&publication_year=2015&author=Anzalone%2CSM&author=Boucenna%2CS&author=Ivaldi%2CS&author=Chetouani%2CM)

[^6]: Anzalone SM, Varni G, Zibetti E, Ivaldi S, Chetouani M (2015) Automated prediction of extraversion during human-robot interaction. In: AIRO@ AI\* IA, pp 29–39

[^7]: Aslam MS, Bilal H, Chang W, Yahya A, Badruddin IA, Kamangar S, Hussien M (2024) Indirect adaptive observer control (I-AOC) design for truck-trailer model based on T-S fuzzy system with unknown nonlinear function. Complex Intell Syst 10(5):7311–7331

[Article](https://link.springer.com/doi/10.1007/s40747-024-01544-7) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Indirect%20adaptive%20observer%20control%20%28I-AOC%29%20design%20for%20truck-trailer%20model%20based%20on%20T-S%20fuzzy%20system%20with%20unknown%20nonlinear%20function&journal=Complex%20Intell%20Syst&doi=10.1007%2Fs40747-024-01544-7&volume=10&issue=5&pages=7311-7331&publication_year=2024&author=Aslam%2CMS&author=Bilal%2CH&author=Chang%2CW&author=Yahya%2CA&author=Badruddin%2CIA&author=Kamangar%2CS&author=Hussien%2CM)

[^8]: Ben-Youssef A, Clavel C, Essid S, Bilac M, Chamoux M, Lim A (2017) UE-HRI: a new dataset for the study of user engagement in spontaneous human–robot interactions. In: Proceedings of the 19th ACM international conference on multimodal interaction, pp 464–472

[^9]: Ben-Youssef A, Clavel C, Essid S (2019) Early detection of user engagement breakdown in spontaneous human–humanoid interaction. IEEE Trans Affect Comput 12(3):776–787

[Article](https://doi.org/10.1109%2FTAFFC.2019.2898399) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Early%20detection%20of%20user%20engagement%20breakdown%20in%20spontaneous%20human%E2%80%93humanoid%20interaction&journal=IEEE%20Trans%20Affect%20Comput&doi=10.1109%2FTAFFC.2019.2898399&volume=12&issue=3&pages=776-787&publication_year=2019&author=Ben-Youssef%2CA&author=Clavel%2CC&author=Essid%2CS)

[^10]: Ben-Youssef A, Varni G, Essid S, Clavel C (2019) On-the-fly detection of user engagement decrease in spontaneous human–robot interaction using recurrent and deep neural networks. Int J Soc Robot 11:815–828

[Article](https://link.springer.com/doi/10.1007/s12369-019-00591-2) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=On-the-fly%20detection%20of%20user%20engagement%20decrease%20in%20spontaneous%20human%E2%80%93robot%20interaction%20using%20recurrent%20and%20deep%20neural%20networks&journal=Int%20J%20Soc%20Robot&doi=10.1007%2Fs12369-019-00591-2&volume=11&pages=815-828&publication_year=2019&author=Ben-Youssef%2CA&author=Varni%2CG&author=Essid%2CS&author=Clavel%2CC)

[^11]: Bi J, Hu F, Wang Y, Luo M, He M (2023) Human engagement intention intensity recognition method based on two states fusion fuzzy inference system. Intell Serv Robot 1–16

[^12]: Bilal H, Ahmed F, Aslam M, Li Q, Hou J, Yin B (2024) A blockchain-enabled approach for privacy-protected data sharing in internet of robotic things networks. Hum-Cent Comput Inf Sci 1–21:05. [https://doi.org/10.22967/HCIS.2024.14.071](https://doi.org/10.22967/HCIS.2024.14.071)

[Article](https://doi.org/10.22967%2FHCIS.2024.14.071) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=A%20blockchain-enabled%20approach%20for%20privacy-protected%20data%20sharing%20in%20internet%20of%20robotic%20things%20networks&journal=Hum-Cent%20Comput%20Inf%20Sci&doi=10.22967%2FHCIS.2024.14.071&volume=1%E2%80%9321&publication_year=2024&author=Bilal%2CH&author=Ahmed%2CF&author=Aslam%2CM&author=Li%2CQ&author=Hou%2CJ&author=Yin%2CB)

[^13]: Bilal H, Aslam MS, Tian Y, Yahya A, Izneide BA (2024) Enhancing trajectory tracking and vibration control of flexible robots with hybrid fuzzy ADRC and input shaping. IEEE Access

[^14]: Bilal H, Obaidat MS, Aslam MS, Zhang J, Yin B, Mahmood K (2024) Online fault diagnosis of industrial robot using iort and hybrid deep learning techniques: an experimental approach. IEEE Internet Things J

[^15]: Bilal H, Tian Y, Ali A, Muhammad Y, Yahya A, Izneid BA, Ullah I (2024) An intelligent approach for early and accurate predication of cardiac disease using hybrid artificial intelligence techniques. Bioengineering 11(12):1290

[Article](https://doi.org/10.3390%2Fbioengineering11121290) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=An%20intelligent%20approach%20for%20early%20and%20accurate%20predication%20of%20cardiac%20disease%20using%20hybrid%20artificial%20intelligence%20techniques&journal=Bioengineering&doi=10.3390%2Fbioengineering11121290&volume=11&issue=12&publication_year=2024&author=Bilal%2CH&author=Tian%2CY&author=Ali%2CA&author=Muhammad%2CY&author=Yahya%2CA&author=Izneid%2CBA&author=Ullah%2CI)

[^16]: Bohus D, Horvitz E (2009) Dialog in the open world: platform and applications. In: Proceedings of the 2009 international conference on Multimodal interfaces, pp 31–38

[^17]: Bohus D, Horvitz E (2014) Managing human-robot engagement with forecasts and... um... hesitations. In: Proceedings of the 16th international conference on multimodal interaction, pp 2–9

[^18]: Bosch N, D’Mello S (2017) The affective experience of novice computer programmers. Int J Artif Intell Educ 27:181–206

[Article](https://link.springer.com/doi/10.1007/s40593-015-0069-5) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=The%20affective%20experience%20of%20novice%20computer%20programmers&journal=Int%20J%20Artif%20Intell%20Educ&doi=10.1007%2Fs40593-015-0069-5&volume=27&pages=181-206&publication_year=2017&author=Bosch%2CN&author=D%E2%80%99Mello%2CS)

[^19]: Burgoon JK, Buller DB, Hale JL, de Turck MA (1984) Relational messages associated with nonverbal behaviors. Hum Commun Res 10(3):351–378

[Article](https://doi.org/10.1111%2Fj.1468-2958.1984.tb00023.x) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Relational%20messages%20associated%20with%20nonverbal%20behaviors&journal=Hum%20Commun%20Res&doi=10.1111%2Fj.1468-2958.1984.tb00023.x&volume=10&issue=3&pages=351-378&publication_year=1984&author=Burgoon%2CJK&author=Buller%2CDB&author=Hale%2CJL&author=Turck%2CMA)

[^20]: Castellano G, Pereira A, Leite I, Paiva A, McOwan PW (2009) Detecting user engagement with a robot companion using task and social interaction-based features. In: Proceedings of the 2009 international conference on Multimodal interfaces, pp 119–126

[^21]: Castellano G, Leite I, Pereira A, Martinho C, Paiva A, McOwan PW (2012) Detecting engagement in HRI: an exploration of social and task-based context. In: 2012 international conference on privacy, security, risk and trust and 2012 international conference on social computing. IEEE, pp 421–428

[^22]: Chong E, Clark-Whitney E, Southerland A, Stubbs E, Miller C, Ajodan EL, Silverman MR, Lord C, Rozga A, Jones RM, Rehg JM (2020) Detection of eye contact with deep neural networks is as accurate as human experts. Nat Commun 11

[^23]: Del Duchetto F, Baxter P, Hanheide M (2020) Are you still with me? Continuous engagement assessment from a robot’s point of view. Front Robot AI 7:116

[Article](https://doi.org/10.3389%2Ffrobt.2020.00116) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Are%20you%20still%20with%20me%3F%20Continuous%20engagement%20assessment%20from%20a%20robot%E2%80%99s%20point%20of%20view&journal=Front%20Robot%20AI&doi=10.3389%2Ffrobt.2020.00116&volume=7&publication_year=2020&author=Duchetto%2CF&author=Baxter%2CP&author=Hanheide%2CM)

[^24]: Deng J, Guo J, Ververas E, Kotsia I, Zafeiriou S (2020) Retinaface: single-shot multi-level face localisation in the wild. In: Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, pp 5203–5212

[^25]: Dewan M, Murshed M, Lin F (2019) Engagement detection in online learning: a review. Smart Learn Environ 6(1):1–20

[Article](https://link.springer.com/doi/10.1186/s40561-018-0080-z) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Engagement%20detection%20in%20online%20learning%3A%20a%20review&journal=Smart%20Learn%20Environ&doi=10.1186%2Fs40561-018-0080-z&volume=6&issue=1&pages=1-20&publication_year=2019&author=Dewan%2CM&author=Murshed%2CM&author=Lin%2CF)

[^26]: Dybala P, Ptaszynski M, Rzepka R, Araki K (2009) Activating humans with humor-a dialogue system that users want to interact with. IEICE Trans Inf Syst 92(12):2394–2401

[Article](https://doi.org/10.1587%2Ftransinf.E92.D.2394) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Activating%20humans%20with%20humor-a%20dialogue%20system%20that%20users%20want%20to%20interact%20with&journal=IEICE%20Trans%20Inf%20Syst&doi=10.1587%2Ftransinf.E92.D.2394&volume=92&issue=12&pages=2394-2401&publication_year=2009&author=Dybala%2CP&author=Ptaszynski%2CM&author=Rzepka%2CR&author=Araki%2CK)

[^27]: D’Mello S, Graesser A (2012) Dynamics of affective states during complex learning. Learn Instruct 22(2):145–157

[Article](https://doi.org/10.1016%2Fj.learninstruc.2011.10.001) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Dynamics%20of%20affective%20states%20during%20complex%20learning&journal=Learn%20Instruct&doi=10.1016%2Fj.learninstruc.2011.10.001&volume=22&issue=2&pages=145-157&publication_year=2012&author=D%E2%80%99Mello%2CS&author=Graesser%2CA)

[^28]: Foster ME, Gaschler A, Giuliani M (2017) Automatically classifying user engagement for dynamic multi-party human-robot interaction. Int J Soc Robot 9(5):659–674

[Article](https://link.springer.com/doi/10.1007/s12369-017-0414-y) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Automatically%20classifying%20user%20engagement%20for%20dynamic%20multi-party%20human-robot%20interaction&journal=Int%20J%20Soc%20Robot&doi=10.1007%2Fs12369-017-0414-y&volume=9&issue=5&pages=659-674&publication_year=2017&author=Foster%2CME&author=Gaschler%2CA&author=Giuliani%2CM)

[^29]: Gao Y, Huang C-M (2022) Evaluation of socially-aware robot navigation. Front Robot AI 8:721317

[Article](https://doi.org/10.3389%2Ffrobt.2021.721317) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Evaluation%20of%20socially-aware%20robot%20navigation&journal=Front%20Robot%20AI&doi=10.3389%2Ffrobt.2021.721317&volume=8&publication_year=2022&author=Gao%2CY&author=Huang%2CC-M)

[^30]: Gildenblat J (2021) Contributors. Pytorch library for cam methods. [https://github.com/jacobgil/pytorch-grad-cam](https://github.com/jacobgil/pytorch-grad-cam)

[^31]: Goodrich MA, Schultz AC et al (2008) Human-robot interaction: a survey. Found Trends Hum Comput Interact 1(3):203–275

[Article](https://doi.org/10.1561%2F1100000005) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Human-robot%20interaction%3A%20a%20survey&journal=Found%20Trends%20Hum%20Comput%20Interact&doi=10.1561%2F1100000005&volume=1&issue=3&pages=203-275&publication_year=2008&author=Goodrich%2CMA&author=Schultz%2CAC)

[^32]: Hall L, Woods S, Aylett R, Newall L, Paiva A (2005) Achieving empathic engagement through affective interaction with synthetic characters. In: Affective computing and intelligent interaction: first international conference, ACII 2005, Beijing, China, October 22–24, 2005. Proceedings 1. Springer, pp 731–738

[^33]: Hastie T, Tibshirani R, Friedman JH, Friedman JH (2009) The elements of statistical learning: data mining, inference, and prediction, vol 2. Springer

[Book](https://link.springer.com/doi/10.1007/978-0-387-84858-7) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=The%20elements%20of%20statistical%20learning%3A%20data%20mining%2C%20inference%2C%20and%20prediction&doi=10.1007%2F978-0-387-84858-7&publication_year=2009&author=Hastie%2CT&author=Tibshirani%2CR&author=Friedman%2CJH&author=Friedman%2CJH)

[^34]: He K, Zhang X, Ren S, Sun J (2016) Deep residual learning for image recognition. In: Proceedings of the IEEE conference on computer vision and pattern recognition (CVPR)

[^35]: Hempel T, Dinges L, Al-Hamadi A (2023) Sentiment-based engagement strategies for intuitive human-robot interaction. arXiv preprint [arXiv:2301.03867](http://arxiv.org/abs/2301.03867)

[^36]: Hempel T, Abdelrahman AA, Al-Hamadi A (2024) Toward robust and unconstrained full range of rotation head pose estimation. IEEE Trans Image Process 33:2377–2387

[Article](https://doi.org/10.1109%2FTIP.2024.3378180) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Toward%20robust%20and%20unconstrained%20full%20range%20of%20rotation%20head%20pose%20estimation&journal=IEEE%20Trans%20Image%20Process&doi=10.1109%2FTIP.2024.3378180&volume=33&pages=2377-2387&publication_year=2024&author=Hempel%2CT&author=Abdelrahman%2CAA&author=Al-Hamadi%2CA)

[^37]: Hempel T, Jung M, Abdelrahman AA, Al-Hamadi A (2024) NITEC: versatile hand-annotated eye contact dataset for ego-vision interaction. In: Proceedings of the IEEE/CVF winter conference on applications of computer vision, pp 4437–4446. arXiv preprint [arXiv:2311.04505](http://arxiv.org/abs/2311.04505)

[^38]: Higashinaka R, Funakoshi K, Kobayashi Y, Inaba M (2016) The dialogue breakdown detection challenge: task description, datasets, and evaluation metrics. In: Proceedings of the tenth international conference on language resources and evaluation (LREC’16), pp 3146–3150

[^39]: Ivaldi S, Lefort S, Peters J, Chetouani M, Provasi J, Zibetti E (2017) Towards engagement models that consider individual factors in HRI: on the relation of extroversion and negative attitude towards robots to gaze and speech during a human-robot assembly task: experiments with the ICUB humanoid. Int J Soc Robot 9:63–86

[Article](https://link.springer.com/doi/10.1007/s12369-016-0357-8) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Towards%20engagement%20models%20that%20consider%20individual%20factors%20in%20HRI%3A%20on%20the%20relation%20of%20extroversion%20and%20negative%20attitude%20towards%20robots%20to%20gaze%20and%20speech%20during%20a%20human-robot%20assembly%20task%3A%20experiments%20with%20the%20ICUB%20humanoid&journal=Int%20J%20Soc%20Robot&doi=10.1007%2Fs12369-016-0357-8&volume=9&pages=63-86&publication_year=2017&author=Ivaldi%2CS&author=Lefort%2CS&author=Peters%2CJ&author=Chetouani%2CM&author=Provasi%2CJ&author=Zibetti%2CE)

[^40]: Jahanmahin R, Masoud S, Rickli J, Djuric A (2022) Human-robot interactions in manufacturing: a survey of human behavior modeling. Robot Comput Integr Manuf 78:102404

[Article](https://doi.org/10.1016%2Fj.rcim.2022.102404) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Human-robot%20interactions%20in%20manufacturing%3A%20a%20survey%20of%20human%20behavior%20modeling&journal=Robot%20Comput%20Integr%20Manuf&doi=10.1016%2Fj.rcim.2022.102404&volume=78&publication_year=2022&author=Jahanmahin%2CR&author=Masoud%2CS&author=Rickli%2CJ&author=Djuric%2CA)

[^41]: Ji Y, Yang Y, Shen F, Shen HT, Li X (2019) A survey of human action analysis in HRI applications. IEEE Trans Circuits Syst Video Technol 30(7):2114–2128

[Article](https://doi.org/10.1109%2FTCSVT.2019.2912988) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=A%20survey%20of%20human%20action%20analysis%20in%20HRI%20applications&journal=IEEE%20Trans%20Circuits%20Syst%20Video%20Technol&doi=10.1109%2FTCSVT.2019.2912988&volume=30&issue=7&pages=2114-2128&publication_year=2019&author=Ji%2CY&author=Yang%2CY&author=Shen%2CF&author=Shen%2CHT&author=Li%2CX)

[^42]: Jongerius C, Hessels RS, Romijn JA, Smets EMA, Hillen MA (2020) The measurement of eye contact in human interactions: a scoping review. J Nonverb Behav 44:363–389

[Article](https://link.springer.com/doi/10.1007/s10919-020-00333-3) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=The%20measurement%20of%20eye%20contact%20in%20human%20interactions%3A%20a%20scoping%20review&journal=J%20Nonverb%20Behav&doi=10.1007%2Fs10919-020-00333-3&volume=44&pages=363-389&publication_year=2020&author=Jongerius%2CC&author=Hessels%2CRS&author=Romijn%2CJA&author=Smets%2CEMA&author=Hillen%2CMA)

[^43]: Jung M, Hempel T, Al-Tawil B, Yang Q, Wachsmuth S, Al-Hamadi A (2025) Toward truly intelligent autonomous systems: a taxonomy of LLM integration for everyday automation. In: International conference on robotics, computer vision and intelligent systems. Springer

[^44]: Kellnhofer P, Recasens A, Stent S, Matusik W, Torralba A (2019) GAZE360: physically unconstrained gaze estimation in the wild. In: Proceedings of the IEEE/CVF international conference on computer vision, pp 6912–6921

[^45]: Kendon A (1967) Some functions of gaze-direction in social interaction. Acta Psychol 26:22–63

[Article](https://doi.org/10.1016%2F0001-6918%2867%2990005-4) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Some%20functions%20of%20gaze-direction%20in%20social%20interaction&journal=Acta%20Psychol&doi=10.1016%2F0001-6918%2867%2990005-4&volume=26&pages=22-63&publication_year=1967&author=Kendon%2CA)

[^46]: Kingma DP, Ba J (2014) ADAM: a method for stochastic optimization. arXiv preprint [arXiv:1412.6980](http://arxiv.org/abs/1412.6980)

[^47]: Kompatsiari K, Ciardo F, De Tommaso D, Wykowska A (2019) Measuring engagement elicited by eye contact in human–robot interaction. In: 2019 IEEE/RSJ international conference on intelligent robots and systems (IROS). IEEE, pp 6979–6985

[^48]: Kompatsiari K, Ciardo F, Tikhanoff V, Metta G, Wykowska A (2021) It’s in the eyes: the engaging role of eye contact in HRI. Int J Soc Robot 13:525–535

[Article](https://link.springer.com/doi/10.1007/s12369-019-00565-4) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=It%E2%80%99s%20in%20the%20eyes%3A%20the%20engaging%20role%20of%20eye%20contact%20in%20HRI&journal=Int%20J%20Soc%20Robot&doi=10.1007%2Fs12369-019-00565-4&volume=13&pages=525-535&publication_year=2021&author=Kompatsiari%2CK&author=Ciardo%2CF&author=Tikhanoff%2CV&author=Metta%2CG&author=Wykowska%2CA)

[^49]: Lala D, Inoue K, Milhorat P, Kawahara T (2017) Detection of social signals for recognizing engagement in human–robot interaction. arXiv preprint [arXiv:1709.10257](http://arxiv.org/abs/1709.10257)

[^50]: Lalmas M, O’Brien H, Yom-Tov E (2022) Measuring user engagement. Springer, New York

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Measuring%20user%20engagement&publication_year=2022&author=Lalmas%2CM&author=O%E2%80%99Brien%2CH&author=Yom-Tov%2CE)

[^51]: Leite I, McCoy M, Ullman D, Salomons N, Scassellati B (2015) Comparing models of disengagement in individual and group interactions. In: Proceedings of the tenth annual ACM/IEEE international conference on human–robot interaction, pp 99–105

[^52]: Liu T, Kappas A (2018) Predicting engagement breakdown in HRI using thin-slices of facial expressions. In: Workshops at the thirty-second AAAI conference on artificial intelligence

[^53]: Mitsuzumi Y, Nakazawa A, Nishida T (2017) Deep eye contact detector: robust eye contact bid detection using convolutional neural network. In: British machine vision conference

[^54]: Mitsuzumi Y, Nakazawa A, Nishida T (2017) Deep eye contact detector: robust eye contact bid detection using convolutional neural network. In: British machine vision conference 2017, BMVC 2017. BMVA Press

[^55]: Nakano YI, Ishii R (2010) Estimating user’s engagement from eye-gaze behaviors in human-agent conversations. In: Proceedings of the 15th international conference on intelligent user interfaces, pp 139–148

[^56]: Natarajan M, Seraj E, Altundas B, Paleja R, Ye S, Chen L, Jensen R, Chang KC, Gombolay M (2023) Human–robot teaming: grand challenges. Curr Robot Rep 4(3):81–100

[Article](https://link.springer.com/doi/10.1007/s43154-023-00103-1) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Human%E2%80%93robot%20teaming%3A%20grand%20challenges&journal=Curr%20Robot%20Rep&doi=10.1007%2Fs43154-023-00103-1&volume=4&issue=3&pages=81-100&publication_year=2023&author=Natarajan%2CM&author=Seraj%2CE&author=Altundas%2CB&author=Paleja%2CR&author=Ye%2CS&author=Chen%2CL&author=Jensen%2CR&author=Chang%2CKC&author=Gombolay%2CM)

[^57]: O’Brien HL, Toms EG (2008) What is user engagement? A conceptual framework for defining user engagement with technology. J Am Soc Inf Sci Technol 59(6):938–955

[Article](https://doi.org/10.1002%2Fasi.20801) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=What%20is%20user%20engagement%3F%20A%20conceptual%20framework%20for%20defining%20user%20engagement%20with%20technology&journal=J%20Am%20Soc%20Inf%20Sci%20Technol&doi=10.1002%2Fasi.20801&volume=59&issue=6&pages=938-955&publication_year=2008&author=O%E2%80%99Brien%2CHL&author=Toms%2CEG)

[^58]: Pages J, Marchionni L, Ferro F (2016) Tiago: the modular robot that adapts to different research needs. In: International workshop on robot modularity, IROS, vol 290

[^59]: Peters C, Castellano G, De Freitas S (2009) An exploration of user engagement in HCI. In: Proceedings of the international workshop on affective-aware virtual agents and social robots, pp 1–3

[^60]: Poggi I (2007) Mind, hands, face and body. A goal and belief view of multimodal communication. Weidler, Berlin

[^61]: Pudjihartono N, Fadason T, Kempa-Liehr AW, O’Sullivan JM (2022) A review of feature selection methods for machine learning-based disease risk prediction. Front Bioinform 2:927312

[Article](https://doi.org/10.3389%2Ffbinf.2022.927312) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=A%20review%20of%20feature%20selection%20methods%20for%20machine%20learning-based%20disease%20risk%20prediction&journal=Front%20Bioinform&doi=10.3389%2Ffbinf.2022.927312&volume=2&publication_year=2022&author=Pudjihartono%2CN&author=Fadason%2CT&author=Kempa-Liehr%2CAW&author=O%E2%80%99Sullivan%2CJM)

[^62]: Rich C, Ponsler B, Holroyd A, Sidner CL (2010) Recognizing engagement in human–robot interaction. In: 2010 5th ACM/IEEE international conference on human–robot interaction (HRI). IEEE, pp 375–382

[^63]: Salam H (2021) Distinguishing engagement facets: an essential component for AI-based interactive healthcare. arXiv preprint [arXiv:2111.11138](http://arxiv.org/abs/2111.11138)

[^64]: Saleh K, Yu K, Chen F (2021) Improving users engagement detection using end-to-end spatio-temporal convolutional neural networks. In: Companion of the 2021 ACM/IEEE international conference on human–robot interaction, pp 190–194

[^65]: Savchenko AV, Savchenko LV, Makarov I (2022) Classifying emotions and engagement in online learning based on a single facial expression recognition neural network. IEEE Trans Affect Comput 13(4):2132–2143

[Article](https://doi.org/10.1109%2FTAFFC.2022.3188390) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Classifying%20emotions%20and%20engagement%20in%20online%20learning%20based%20on%20a%20single%20facial%20expression%20recognition%20neural%20network&journal=IEEE%20Trans%20Affect%20Comput&doi=10.1109%2FTAFFC.2022.3188390&volume=13&issue=4&pages=2132-2143&publication_year=2022&author=Savchenko%2CAV&author=Savchenko%2CLV&author=Makarov%2CI)

[^66]: Schellen E, Bossi F, Wykowska A (2021) Robot gaze behavior affects honesty in human–robot interaction. Front Artif Intell 4:663190

[Article](https://doi.org/10.3389%2Ffrai.2021.663190) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Robot%20gaze%20behavior%20affects%20honesty%20in%20human%E2%80%93robot%20interaction&journal=Front%20Artif%20Intell&doi=10.3389%2Ffrai.2021.663190&volume=4&publication_year=2021&author=Schellen%2CE&author=Bossi%2CF&author=Wykowska%2CA)

[^67]: Sidner CL, Lee C, Kidd CD, Lesh N, Rich C (2005) Explorations in engagement for humans and robots. Artif Intell 166(1–2):140–164

[Article](https://doi.org/10.1016%2Fj.artint.2005.03.005) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Explorations%20in%20engagement%20for%20humans%20and%20robots&journal=Artif%20Intell&doi=10.1016%2Fj.artint.2005.03.005&volume=166&issue=1%E2%80%932&pages=140-164&publication_year=2005&author=Sidner%2CCL&author=Lee%2CC&author=Kidd%2CCD&author=Lesh%2CN&author=Rich%2CC)

[^68]: Srinivasan S, Singh RR, Biradar RR, Revathi SA (2021) COVID-19 monitoring system using social distancing and face mask detection on surveillance video datasets. In: 2021 International conference on emerging smart computing and informatics (ESCI). IEEE, pp 449–455

[^69]: Stahle L, Wold S (1989) Analysis of variance (ANOVA). Chemometr Intell Lab Syst 6(4):259–272. [https://doi.org/10.1016/0169-7439(89)80095-4](https://doi.org/10.1016/0169-7439\(89\)80095-4)

[^70]: Strazdas D, Jung M, Marquenie J, Siegert I, Al-Hamadi A (2025) IM here: interaction model for human effort based robot engagement. In: 2025 IEEE conference on cognitive and computational aspects of situation management (CogSIMA). IEEE

[^71]: Tukey JW (1949) Comparing individual means in the analysis of variance. Biometrics 56:99–114

[Article](https://doi.org/10.2307%2F3001913) [MathSciNet](http://www.ams.org/mathscinet-getitem?mr=30734) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Comparing%20individual%20means%20in%20the%20analysis%20of%20variance&journal=Biometrics&doi=10.2307%2F3001913&volume=56&pages=99-114&publication_year=1949&author=Tukey%2CJW)

[^72]: Vaufreydaz D, Johal W, Combe C (2016) Starting engagement detection towards a companion robot using multimodal features. Robot Autonom Syst 75:4–16

[Article](https://doi.org/10.1016%2Fj.robot.2015.01.004) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Starting%20engagement%20detection%20towards%20a%20companion%20robot%20using%20multimodal%20features&journal=Robot%20Autonom%20Syst&doi=10.1016%2Fj.robot.2015.01.004&volume=75&pages=4-16&publication_year=2016&author=Vaufreydaz%2CD&author=Johal%2CW&author=Combe%2CC)

[^73]: Wienke J, Klotz D, Wrede S (2012) A framework for the acquisition of multimodal human–robot interaction data sets with a whole-system perspective. In: LREC 2012 workshop on multimodal corpora for machine learning. Citeseer

[^74]: Wilson EB (1927) Probable inference, the law of succession, and statistical inference. J Am Stat Assoc 22(158):209–212

[Article](https://doi.org/10.1080%2F01621459.1927.10502953) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Probable%20inference%2C%20the%20law%20of%20succession%2C%20and%20statistical%20inference&journal=J%20Am%20Stat%20Assoc&doi=10.1080%2F01621459.1927.10502953&volume=22&issue=158&pages=209-212&publication_year=1927&author=Wilson%2CEB)

[^75]: Xu Q, Liyuan L, Gang W (2013) Designing engagement-aware agents for multiparty conversations. In: Proceedings of the SIGCHI conference on human factors in computing systems, pp 2233–2242

[^76]: Xu T, Zhang H, Yu C (2016) See you see me: the role of eye contact in multimodal human-robot interaction. ACM Trans Interact Intell Syst TIIS 6(1):1–22

[Article](https://doi.org/10.1145%2F2882970) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=See%20you%20see%20me%3A%20the%20role%20of%20eye%20contact%20in%20multimodal%20human-robot%20interaction&journal=ACM%20Trans%20Interact%20Intell%20Syst%20TIIS&doi=10.1145%2F2882970&volume=6&issue=1&pages=1-22&publication_year=2016&author=Xu%2CT&author=Zhang%2CH&author=Yu%2CC)

[^77]: Zhang D, Wang B, Wang G, Zhang Q, Zhang J, Han J, You Z (2022) Onfocus detection: identifying individual-camera eye contact from unconstrained images. Sci China Inf Sci 65(6):160101

[Article](https://link.springer.com/doi/10.1007/s11432-020-3181-9) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Onfocus%20detection%3A%20identifying%20individual-camera%20eye%20contact%20from%20unconstrained%20images&journal=Sci%20China%20Inf%20Sci&doi=10.1007%2Fs11432-020-3181-9&volume=65&issue=6&publication_year=2022&author=Zhang%2CD&author=Wang%2CB&author=Wang%2CG&author=Zhang%2CQ&author=Zhang%2CJ&author=Han%2CJ&author=You%2CZ)