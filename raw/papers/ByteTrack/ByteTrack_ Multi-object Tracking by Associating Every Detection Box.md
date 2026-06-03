---
title: "ByteTrack: Multi-object Tracking by Associating Every Detection Box"
source: "https://link.springer.com/chapter/10.1007/978-3-031-20047-2_1"
author:
  - "[[Yifu Zhang]]"
  - "[[Peize Sun]]"
  - "[[Yi Jiang]]"
  - "[[Dongdong Yu]]"
  - "[[Fucheng Weng]]"
  - "[[Zehuan Yuan]]"
  - "[[Ping Luo]]"
  - "[[Wenyu Liu]]"
  - "[[Xinggang Wang]]"
published: 2022-01-01
created: 2026-06-03
description: "Multi-object tracking (MOT) aims at estimating bounding boxes and identities of objects in videos. Most methods obtain identities by associating detection boxes whose scores are higher than a threshold. The objects with low detection scores, e.g. occluded objects,..."
tags:
  - "clippings"
---
## Abstract

Multi-object tracking (MOT) aims at estimating bounding boxes and identities of objects in videos. Most methods obtain identities by associating detection boxes whose scores are higher than a threshold. The objects with low detection scores, *e*.*g*. occluded objects, are simply thrown away, which brings non-negligible true object missing and fragmented trajectories. To solve this problem, we present a simple, effective and generic association method, tracking by associating almost every detection box instead of only the high score ones. For the low score detection boxes, we utilize their similarities with tracklets to recover true objects and filter out the background detections. When applied to 9 different state-of-the-art trackers, our method achieves consistent improvement on IDF1 score ranging from 1 to 10 points. To put forwards the state-of-the-art performance of MOT, we design a simple and strong tracker, named ByteTrack. For the first time, we achieve 80.3 MOTA, 77.3 IDF1 and 63.1 HOTA on the test set of MOT17 with 30 FPS running speed on a single V100 GPU. ByteTrack also achieves state-of-the-art performance on MOT20, HiEve and BDD100K tracking benchmarks. The source code, pre-trained models with deploy versions and tutorials of applying to other trackers are released at [https://github.com/ifzhang/ByteTrack](https://github.com/ifzhang/ByteTrack).

## 1 Introduction

*Was vernünftig ist, das ist wirklich; und was wirklich ist, das ist vernünftig.*

— *G. W. F. Hegel*

Tracking-by-detection is the most effective paradigm for multi-object tracking (MOT) in current. Due to the complex scenarios in videos, detectors are prone to make imperfect predictions. State-of-the-art MOT methods \[[^1],[^2],[^3], [^6], [^13], [^18], [^46], [^58], [^69], [^71], [^84]\] need to deal with true positive / false positive trade-off in detection boxes to eliminate low confidence detection boxes \[[^4], [^41]\]. However, is it the right way to eliminate all low confidence detection boxes? Our answer is NO: as Hegel said “What is reasonable is real; that which is real is reasonable.” Low confidence detection boxes sometimes indicate the existence of objects, *e*.*g*. the occluded objects. Filtering out these objects causes irreversible errors for MOT and brings non-negligible missing detection and fragmented trajectories.

**Fig. 1.**

[![figure 1](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-3-031-20047-2_1/MediaObjects/539987_1_En_1_Fig1_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-3-031-20047-2_1/figures/1)

Examples of our method which associates almost every detection box. (a) shows all the detection boxes with their scores. (b) shows the tracklets obtained by previous methods which associates detection boxes whose scores are higher than a threshold, *i*.*e*. 0.5. The same box color represents the same identity. (c) shows the tracklets obtained by our method. The dashed boxes represent the predicted box of the previous tracklets using Kalman filter. The two low score detection boxes are correctly matched to the previous tracklets based on the large IoU.

Figure [1](#Fig1) (a) and (b) show this problem. In frame $t_1$, we initialize three different tracklets as their scores are all higher than 0.5. However, in frame $t_2$ and frame $t_3$ when occlusion happens, red tracklet’s corresponding detection score becomes lower *i*.*e*. 0.8 to 0.4 and then 0.4 to 0.1. These detection boxes are eliminated by the thresholding mechanism and the red tracklet disappears accordingly. Nevertheless, if we take every detection box into consideration, more false positives will be introduced immediately, *e*.*g*., the most right box in frame $t_3$ of Fig. [1](#Fig1) (a). To the best of our knowledge, very few methods \[[^31], [^61]\] in MOT are able to handle this detection dilemma.

In this paper, we identify that the similarity with tracklets provides a strong cue to distinguish the objects and background in low score detection boxes. As shown in Fig. [1](#Fig1) (c), two low score detection boxes are matched to the tracklets by the motion model’s predicted boxes, and thus the objects are correctly recovered. At the same time, the background box is removed since it has no matched tracklet.

For making full use of detection boxes from high scores to low ones in the matching process, we present a simple and effective association method BYTE, named for each detection box is a basic unit of the tracklet, as byte in computer program, and our tracking method values every detailed detection box. We first match the high score detection boxes to the tracklets based on motion similarity or appearance similarity. Similar to \[[^6]\], we adopt Kalman filter \[[^30]\] to predict the location of the tracklets in the new frame. The similarity can be computed by the IoU or Re-ID feature distance of the predicted box and the detection box. Figure [1](#Fig1) (b) is exactly the results after the first matching. Then, we perform the second matching between the unmatched tracklets, *i*.*e*. the tracklet in red box, and the low score detection boxes using the same motion similarity. Figure [1](#Fig1) (c) shows the results after the second matching. The occluded person with low detection scores is matched correctly to the previous tracklet and the background (in the right part of the image) is removed.

**Fig. 2.**

[![figure 2](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-3-031-20047-2_1/MediaObjects/539987_1_En_1_Fig2_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-3-031-20047-2_1/figures/2)

MOTA-IDF1-FPS comparisons of different trackers on the test set of MOT17. The horizontal axis is FPS (running speed), the vertical axis is MOTA, and the radius of circle is IDF1. Our ByteTrack achieves 80.3 MOTA, 77.3 IDF1 on MOT17 test set with 30 FPS running speed, outperforming all previous trackers. Details are given in Table [4](#Tab4).

As the integrating topic of object detection and association, a desirable solution to MOT is never a detector and the following association; besides, well-designed of their junction area is also important. The innovation of BYTE lies in the junction area of detection and association, where low score detection boxes are bridges to boost both of them. Benefiting from this integration innovation, when BYTE is applied to 9 different state-of-the-art trackers, including the Re-ID-based ones \[[^34], [^48], [^68], [^84]\], motion-based ones \[[^70], [^87]\], chain-based one \[[^49]\] and attention-based ones \[[^58], [^79]\], notable improvements are achieved on almost all the metrics including MOTA, IDF1 score and ID switches. For example, we increase the MOTA of CenterTrack \[[^87]\] from 66.1 to 67.4, IDF1 from 64.2 to 74.0 and decrease the IDs from 528 to 144 on the half validation set of MOT17 (Fig. [2](#Fig2)).

Towards pushing forwards the state-of-the-art performance of MOT, we propose a simple and strong tracker, named ByteTrack. We adopt a recent high-performance detector YOLOX \[[^25]\] to obtain the detection boxes and associate them with our proposed BYTE. On the MOT challenges, ByteTrack ranks 1st on both MOT17 \[[^45]\] and MOT20 \[[^17]\], achieving 80.3 MOTA, 77.3 IDF1 and 63.1 HOTA with 30 FPS running speed on V100 GPU on MOT17 and 77.8 MOTA, 75.2 IDF1 and 61.3 HOTA on much more crowded MOT20. ByteTrack also achieves state-of-the-art performance on HiEve \[[^38]\] and BDD100K \[[^78]\] tracking benchmarks. We hope the efficiency and simplicity of ByteTrack could make it attractive in real applications such as social computing.

## 2 Related Work

### 2.1 Object Detection in MOT

Object detection is one of the most active topics in computer vision and it is the basis of multi-object tracking. The MOT17 dataset \[[^45]\] provides detection results obtained by popular detectors such as DPM \[[^23]\], Faster R-CNN \[[^51]\] and SDP \[[^76]\]. A large number of methods \[[^3], [^9], [^13], [^15], [^29], [^72], [^90]\] focus on improving the tracking performance based on these given detection results.

**Tracking by Detection.** With the rapid development of object detection \[[^11], [^21], [^22], [^24], [^27], [^36], [^50], [^51], [^57], [^59], [^63]\], more and more methods begin to utilize more powerful detectors to obtain higher tracking performance. The one-stage object detector RetinaNet \[[^36]\] begin to be adopted by several methods such as \[[^40], [^49]\]. CenterNet \[[^88]\] is the most popular detector adopted by most methods \[[^61], [^64], [^66], [^70], [^84], [^86], [^87]\] for its simplicity and efficiency. The YOLO series detectors \[[^8], [^50], [^63]\] are also adopted by a large number of methods \[[^16], [^34], [^35], [^68], [^74]\] for its excellent balance of accuracy and speed. Most of these methods directly use the detection boxes on a single image for tracking.

However, the number of missing detections and very low scoring detections begin to increase when occlusion or motion blur happens in the video sequence, as is pointed out by video object detection methods \[[^42], [^60]\]. Therefore, the information of the previous frames are usually leveraged to enhance the video detection performance.

**Detection by Tracking.** Tracking can also adopted to help obtain more accurate detection boxes. Some methods \[[^13],[^14],[^15],[^16], [^53], [^90]\] utilize single object tracking (SOT) \[[^5]\] or Kalman filter \[[^30]\] to predict the location of the tracklets in the following frame and fuse the predicted boxes with the detection boxes to enhance the detection results. Other methods \[[^35], [^85]\] leverage tracked boxes in the previous frames to enhance feature representation of the following frame. Recently, Transformer-based \[[^19], [^39], [^62], [^65]\] detectors \[[^12], [^91]\] are adopted by several methods \[[^10], [^43], [^58], [^79]\] for its strong ability to propagate boxes between frames. Our method also utilize the similarity with tracklets to strength the reliability of detection boxes.

After obtaining the detection boxes by various detectors, most MOT methods \[[^34], [^40], [^48], [^58], [^68], [^70], [^84]\] only keep the high score detection boxes by a threshold, *i*.*e*. 0.5, and use those boxes as the input of data association. This is because the low score detection boxes contain many backgrounds which harm the tracking performance. However, we observe that many occluded objects can be correctly detected but have low scores. To reduce missing detections and keep the persistence of trajectories, we keep all the detection boxes and associate across every of them.

### 2.2 Data Association

Data association is the core of multi-object tracking, which first computes the similarity between tracklets and detection boxes and leverage different strategies to match them according to the similarity.

**Similarity Metrics.** Location, motion and appearance are useful cues for association. SORT \[[^6]\] combines location and motion cues in a very simple way. It first adopts Kalman filter \[[^30]\] to predict the location of the tracklets in the new frame and then computes the IoU between the detection boxes and the predicted boxes as the similarity. Some recent methods \[[^56], [^58], [^70], [^87]\] design networks to learn object motions and achieve more robust results in cases of large camera motion or low frame rate. Location and motion similarity are accurate in the short-range matching. Appearance similarity are helpful in the long-range matching. An object can be re-identified using appearance similarity after being occluded for a long period of time. Appearance similarity can be measured by the cosine similarity of the Re-ID features. DeepSORT \[[^69]\] adopts a stand-alone Re-ID model to extract appearance features from the detection boxes. Recently, joint detection and Re-ID models \[[^34], [^40], [^48], [^68], [^83], [^84], [^89]\] becomes more and more popular because of their simplicity and efficiency.

**Matching Strategy.** After similarity computation, matching strategy assigns identities to the objects. This can be done by Hungarian Algorithm \[[^32]\] or greedy assignment \[[^87]\]. SORT \[[^6]\] matches the detection boxes to the tracklets by once matching. DeepSORT \[[^69]\] proposes a cascaded matching strategy which first matches the detection boxes to the most recent tracklets and then to the lost ones. MOTDT \[[^13]\] first utilizes appearance similarity to match and then utilize the IoU similarity to match the unmatched tracklets. QDTrack \[[^48]\] turns the appearance similarity into probability by a bi-directional softmax operation and adopts a nearest neighbor search to accomplish matching. Attention mechanism \[[^62]\] can directly propagate boxes between frames and perform association implicitly. Recent methods such as \[[^43], [^79]\] propose track queries to find the location of the tracked objects in the following frames. The matching is implicitly performed in the attention interaction process without using Hungarian Algorithm.

All these methods focus on how to design better association methods. However, we argue that the way detection boxes are utilized determines the upper bound of data association and we focus on how to make full use of detection boxes from high scores to low ones in the matching process.

## 3 BYTE

We propose a simple, effective and generic data association method, BYTE. Different from previous methods \[[^34], [^48], [^68], [^84]\] which only keep the high score detection boxes, we keep almost every detection box and separate them into high score ones and low score ones. We first associate the high score detection boxes to the tracklets. Some tracklets get unmatched because they do not match to an appropriate high score detection box, which usually happens when occlusion, motion blur or size changing occurs. We then associate the low score detection boxes and these unmatched tracklets to recover the objects in low score detection boxes and filter out background, simultaneously. The pseudo-code of BYTE is shown in Algorithm 1.

![figure a](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-3-031-20047-2_1/MediaObjects/539987_1_En_1_Figa_HTML.png)

The input of BYTE is a video sequence $\texttt {V}$, along with an object detector $\texttt {Det}$. We also set a detection score threshold $\tau$. The output of BYTE is the tracks $\mathcal {T}$ of the video and each track contains the bounding box and identity of the object in each frame.

For each frame in the video, we predict the detection boxes and scores using the detector $\texttt {Det}$. We separate all the detection boxes into two parts $\mathcal {D}_{high}$ and $\mathcal {D}_{low}$ according to the detection score threshold $\tau$. For the detection boxes whose scores are higher than $\tau$, we put them into the high score detection boxes $\mathcal {D}_{high}$. For the detection boxes whose scores are lower than $\tau$, we put them into the low score detection boxes $\mathcal {D}_{low}$ (line 3 to 13 in Algorithm 1).

After separating the low score detection boxes and the high score detection boxes, we adopt Kalman filter to predict the new locations in the current frame of each track in $\mathcal {T}$ (line 14 to 16 in Algorithm 1).

The first association is performed between the high score detection boxes $\mathcal {D}_{high}$ and all the tracks $\mathcal {T}$ (including the lost tracks $\mathcal {T}_{lost}$). Similarity#1 can be computed by either by the IoU or the Re-ID feature distances between the detection boxes $\mathcal {D}_{high}$ and the predicted box of tracks $\mathcal {T}$. Then, we adopt Hungarian Algorithm \[[^32]\] to finish the matching based on the similarity. We keep the unmatched detections in $\mathcal {D}_{remain}$ and the unmatched tracks in $\mathcal {T}_{remain}$ (line 17 to 19 in Algorithm 1).

BYTE is highly flexible and can be compatible to other different association methods. For example, when BYTE is combined with FairMOT \[[^84]\], Re-ID feature is added into \* first association \* in Algorithm 1, others are the same. In the experiments, we apply BYTE to 9 different state-of-the-art trackers and achieve notable improvements on almost all the metrics.

The second association is performed between the low score detection boxes $\mathcal {D}_{low}$ and the remaining tracks $\mathcal {T}_{remain}$ after the first association. We keep the unmatched tracks in $\mathcal {T}_{re-remain}$ and just delete all the unmatched low score detection boxes, since we view them as background. (line 20 to 21 in Algorithm 1). We find it important to use IoU alone as the Similarity#2 in the second association because the low score detection boxes usually contains severe occlusion or motion blur and appearance features are not reliable. Thus, when apply BYTE to other Re-ID based trackers \[[^48], [^68], [^84]\], we do not adopt appearance similarity in the second association.

After the association, the unmatched tracks will be deleted from the tracklets. We do not list the procedure of track rebirth \[[^13], [^69], [^87]\] in Algorithm 1 for simplicity. Actually, it is necessary for the long-range association to preserve the identity of the tracks. For the unmatched tracks $\mathcal {T}_{re-remain}$ after the second association, we put them into $\mathcal {T}_{lost}$. For each track in $\mathcal {T}_{lost}$, only when it exists for more than a certain number of frames, *i*.*e*. 30, we delete it from the tracks $\mathcal {T}$. Otherwise, we remain the lost tracks $\mathcal {T}_{lost}$ in $\mathcal {T}$ (line 22 in Algorithm 1). Finally, we initialize new tracks from the unmatched high score detection boxes $\mathcal {D}_{remain}$ after the first association. (line 23 to 27 in Algorithm 1). The output of each individual frame is the bounding boxes and identities of the tracks $\mathcal {T}$ in the current frame. Note that we do not output the boxes and identities of $\mathcal {T}_{lost}$.

To put forwards the state-of-the-art performance of MOT, we design a simple and strong tracker, named ByteTrack, by equipping the high-performance detector YOLOX \[[^25]\] with our association method BYTE.

## 4 Experiments

### 4.1 Setting

**Datasets.** We evaluate BYTE and ByteTrack on MOT17 \[[^45]\] and MOT20 \[[^17]\] datasets under the “private detection” protocol. Both datasets contain training sets and test sets, without validation sets. For ablation studies, we use the first half of each video in the training set of MOT17 for training and the last half for validation following \[[^87]\]. We train on the combination of CrowdHuman dataset \[[^55]\] and MOT17 half training set following \[[^58], [^70], [^79], [^87]\]. We add Cityperson \[[^81]\] and ETHZ \[[^20]\] for training following \[[^34], [^68], [^84]\] when testing on the test set of MOT17. We also test ByteTrack on HiEve \[[^38]\] and BDD100K \[[^78]\] datasets. HiEve is a large scale human-centric dataset focusing on crowded and complex events. BDD100K is the largest driving video dataset and the dataset splits of the MOT task are 1400 videos for training, 200 videos for validation and 400 videos for testing. It needs to track objects of 8 classes and contains cases of large camera motion.

**Metrics.** We use the CLEAR metrics \[[^4]\], including MOTA, FP, FN, IDs, *etc.*, IDF1 \[[^52]\] and HOTA \[[^41]\] to evaluate different aspects of the tracking performance. MOTA is computed based on FP, FN and IDs. Considering the amount of FP and FN are larger than IDs, MOTA focuses more on the detection performance. IDF1 evaluates the identity preservation ability and focus more on the association performance. HOTA is a very recently proposed metric which explicitly balances the effect of performing accurate detection, association and localization. For BDD100K dataset, there are some multi-class metrics such as mMOTA and mIDF1. mMOTA/mIDF1 is computed by averaging the MOTA/IDF1 of all the classes.

**Implementation Details.** For BYTE, the default detection score threshold $\tau$ is 0.6, unless otherwise specified. For the benchmark evaluation of MOT17, MOT20 and HiEve, we only use IoU as the similarity metrics. In the linear assignment step, if the IoU between the detection box and the tracklet box is smaller than 0.2, the matching will be rejected. For the lost tracklets, we keep it for 30 frames in case it appears again. For BDD100K, we use UniTrack \[[^67]\] as the Re-ID model. In ablation study, we use FastReID \[[^28]\] to extract Re-ID features for MOT17.

For ByteTrack, the detector is YOLOX \[[^25]\] with YOLOX-X as the backbone and COCO-pretrained model \[[^37]\] as the initialized weights. For MOT17, the training schedule is 80 epochs on the combination of MOT17, CrowdHuman, Cityperson and ETHZ. For MOT20 and HiEve, we only add CrowdHuman as additional training data. For BDD100K, we do not use additional training data and only train 50 epochs. The input image size is 1440 $\times$ 800 and the shortest side ranges from 576 to 1024 during multi-scale training. The data augmentation includes Mosaic \[[^8]\] and Mixup \[[^80]\]. The model is trained on 8 NVIDIA Tesla V100 GPU with batch size of 48. The optimizer is SGD with weight decay of $5\times 10^{-4}$ and momentum of 0.9. The initial learning rate is $10^{-3}$ with 1 epoch warm-up and cosine annealing schedule. The total training time is about 12 h. Following \[[^25]\], FPS is measured with FP16-precision \[[^44]\] and batch size of 1 on a single GPU.

### 4.2 Ablation Studies on BYTE

**Similarity Analysis.** We choose different types of similarity for the first association and the second association of BYTE. The results are shown in Table [1](#Tab1). We can see that either IoU or Re-ID can be a good choice for Similarity#1 on MOT17. IoU achieves better MOTA and IDs while Re-ID achieves higher IDF1. On BDD100K, Re-ID achieves much better results than IoU in the first association. This is because BDD100K contains large camera motion and the annotations are in low frame rate, which causes failure of motion cues. It is important to utilize IoU as Similarity#2 in the second association on both datasets because the low score detection boxes usually contains severe occlusion or motion blur and thus Re-ID features are not reliable. From Table [1](#Tab1) we can find that using IoU as Similarity#2 increases about 1.0 MOTA compared to Re-ID, which indicates that Re-ID features of the low score detection boxes are not reliable.

**Comparisons with Other Association Methods.** We compare BYTE with other popular association methods including SORT \[[^6]\], DeepSORT \[[^69]\] and MOTDT \[[^13]\] on the validation set of MOT17 and BDD100K. The results are shown in Table [2](#Tab2).

SORT can be seen as our baseline method because both methods only adopt Kalman filter to predict the object motion. We can find that BYTE improves the MOTA metric of SORT from 74.6 to 76.6, IDF1 from 76.9 to 79.3 and decreases IDs from 291 to 159. This highlights the importance of the low score detection boxes and proves the ability of BYTE to recover object boxes from low score one.

DeepSORT utilizes additional Re-ID models to enhance the long-range association. We surprisingly find BYTE also has additional gains compared with DeepSORT. This suggests a simple Kalman filter can perform long-range association and achieve better IDF1 and IDs when the detection boxes are accurate enough. We note that in severe occlusion cases, Re-ID features are vulnerable and may lead to identity switches, instead, motion model behaves more reliably.

**Table 1. Comparison of different type of similarity metrics used in the first association and the second association of BYTE on the MOT17 and BDD100K validation set.**

**Table 2. Comparison of different data association methods on the MOT17 and BDD100K validation set. The best results are shown in **bold**.**

MOTDT integrates motion-guided box propagation results along with detection results to associate unreliable detection results with tracklets. Although sharing the similar motivation, MOTDT is behind BYTE by a large margin. We explain that MOTDT uses propagated boxes as tracklet boxes, which may lead to locating drifts in tracking. Instead, BYTE uses low-score detection boxes to re-associate those unmatched tracklets, therefore, tracklet boxes are more accurate.

Table [2](#Tab2) also shows the results on BDD100K dataset. BYTE also outperforms other association methods by a large margin. Kalman filter fails in autonomous driving scenes and it is the main reason for the low performance of SORT, DeepSORT and MOTDT. Thus, we do not use Kalman filter on BDD100K. Additional off-the-shelf Re-ID models greatly improve the performance of BYTE on BDD100K.

**Fig. 3.**

[![figure 3](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-3-031-20047-2_1/MediaObjects/539987_1_En_1_Fig3_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-3-031-20047-2_1/figures/3)

Comparison of the performances of BYTE and SORT under different detection score thresholds. The results are from the validation set of MOT17.

**Fig. 4.**

[![figure 4](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-3-031-20047-2_1/MediaObjects/539987_1_En_1_Fig4_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-3-031-20047-2_1/figures/4)

Comparison of the number of TPs and FPs in all low score detection boxes and the low score tracked boxes obtained by BYTE. The results are from the validation set of MOT17.

**Robustness to Detection Score Threshold.** The detection score threshold $\tau _{high}$ is a sensitive hyper-parameter and needs to be carefully tuned in the task of multi-object tracking. We change it from 0.2 to 0.8 and compare the MOTA and IDF1 score of BYTE and SORT. The results are shown in Fig. [3](#Fig3). From the results we can see that BYTE is more robust to the detection score threshold than SORT. This is because the second association in BYTE recovers the objects whose scores are lower than $\tau _{high}$, and thus considers almost every detection box regardless of the change of $\tau _{high}$.

**Table 3. Results of applying BYTE to 9 different state-of-the-art trackers on the MOT17 validation set. “K” is short for Kalman filter. In green are the improvements of at least point.**

**Analysis on Low Score Detection Boxes.** To prove the effectiveness of BYTE, we collect the number of TPs and FPs in the low score boxes obtained by BYTE. We use the half training set of MOT17 and CrowdHuman for training and evaluate on the half validation set of MOT17. First, we keep all the low score detection boxes whose scores range from $\tau _{low}$ to $\tau _{high}$ and classify the TPs and FPs using ground truth annotations. Then, we select the tracking results obtained by BYTE from low score detection boxes. The results of each sequence are shown in Fig. [4](#Fig4). We can see that BYTE obtains notably more TPs than FPs from the low score detection boxes even though some sequences (*i*.*e*. MOT17-02) have much more FPs in all the detection boxes. The obtained TPs notably increases MOTA from 74.6 to 76.6 as is shown in Table [2](#Tab2).

**Table 4. Comparison of the state-of-the-art methods under the “private detector” protocol on MOT17 test set. The best results are shown in **bold**. MOT17 contains rich scenes and half of the sequences are captured with camera motion. **ByteTrack** ranks 1st among all the trackers on the leaderboard of MOT17 and outperforms the second one ReMOT by a large margin on almost all the metrics. It also has the highest running speed among all trackers.**

**Table 5. Comparison of the state-of-the-art methods under the “private detector” protocol on MOT20 test set. The best results are shown in **bold**. The scenes in MOT20 are much more crowded than those in MOT17. **ByteTrack** ranks 1st among all the trackers on the leaderboard of MOT20 and outperforms the second one SOTMOT by a large margin on all the metrics. It also has the highest running speed among all trackers.**

**Table 6. Comparison of the state-of-the-art methods under the “private detector” protocol on HiEve test set. The best results are shown in **bold**. HiEve has more complex events than MOT17 and MOT20. **ByteTrack** ranks 1st among all the trackers on the leaderboard of HiEve and outperforms the second one CenterTrack by a large margin on all the metrics.**

**Applications on Other Trackers.** We apply BYTE on 9 different state-of-the-arts trackers, including JDE \[[^68]\], CSTrack \[[^34]\], FairMOT \[[^84]\], TraDes \[[^70]\], QDTrack \[[^48]\], CenterTrack \[[^87]\], Chained-Tracker \[[^49]\], TransTrack \[[^58]\] and MOTR \[[^79]\]. Among these trackers, JDE, CSTrack, FairMOT, TraDes adopt a combination of motion and Re-ID similarity. QDTrack adopts Re-ID similarity alone. CenterTrack and TraDes predict the motion similarity by the learned networks. Chained-Tracker adopts the chain structure and outputs the results of two consecutive frames simultaneously and associate in the same frame by IoU. TransTrack and MOTR adopt the attention mechanism to propagate boxes among frames. Their results are shown in the first line of each tracker in Table [3](#Tab3). To evaluate the effectiveness of BYTE, we design two different modes to apply BYTE to these trackers.

- The first mode is to insert BYTE into the original association methods of different trackers, as is shown in the second line of the results of each tracker in Table [3](#Tab3). Take FairMOT \[[^84]\] for example, after the original association is done, we select all the unmatched tracklets and associate them with the low score detection boxes following the \* second association \* in Algorithm 1. Note that for the low score objects, the Re-ID features are not reliable so we only adopt the IoU between the detection boxes and the tracklet boxes after motion prediction as the similarity. We do not apply the first mode of BYTE to Chained-Tracker because we find it is difficult to implement in the chain structure.
- The second mode is to directly use the detection boxes of these trackers and associate using the whole procedure in Algorithm 1, as is shown in the third line of the results of each tracker in Table [3](#Tab3).

We can see that in both modes, BYTE can bring stable improvements over almost all the metrics including MOTA, IDF1 and IDs. For example, BYTE increases CenterTrack by 1.3 MOTA and 9.8 IDF1, Chained-Tracker by 1.9 MOTA and 5.8 IDF1, TransTrack by 1.2 MOTA and 4.1 IDF1. The results in Table [3](#Tab3) indicate that BYTE has strong generalization ability and can be easily applied to existing trackers.

### 4.3 Benchmark Evaluation

We compare ByteTrack with the state-of-the-art trackers on the test set of MOT17, MOT20 and HiEve under the private detection protocol in Table [4](#Tab4), Table [5](#Tab5) and Table [6](#Tab6), respectively. All the results are directly obtained from the official MOT Challenge evaluation server <sup><a href="#Fn1">1</a></sup> and the Human in Events server <sup><a href="#Fn2">2</a></sup>.

**MOT17.** ByteTrack ranks 1st among all the trackers on the leaderboard of MOT17. Not only does it achieve the best accuracy (*i*.*e*. 80.3 MOTA, 77.3 IDF1 and 63.1 HOTA), but also runs with highest running speed (30 FPS). It outperforms the second-performance tracker \[[^75]\] by a large margin (*i*.*e*. +3.3 MOTA, +5.3 IDF1 and +3.4 HOTA). Also, we use less training data than many high performance methods such as \[[^34], [^35], [^54], [^64], [^84]\] (29K images vs. 73K images). It is worth noting that we only leverage the simplest similarity computation method Kalman filter in the association step compared to other methods \[[^34], [^48], [^58], [^66], [^79], [^84]\] which additionally adopt Re-ID similarity or attention mechanisms. All these indicate that ByteTrack is a simple and strong tracker.

**MOT20.** Compared with MOT17, MOT20 has much more crowded scenarios and occlusion cases. The average number of pedestrians in an image is 170 in the test set of MOT20. ByteTrack also ranks 1st among all the trackers on the leaderboard of MOT20 and outperforms other trackers by a large margin on almost all the metrics. For example, it increases MOTA from 68.6 to 77.8, IDF1 from 71.4 to 75.2 and decreases IDs by 71% from 4209 to 1223. It is worth noting that ByteTrack achieves extremely low identity switches, which further indicates that associating every detection boxes is very effective under occlusion cases.

**Human in Events.** Compared with MOT17 and MOT20, HiEve contains more complex events and more diverse camera views. We train ByteTrack on CrowdHuman dataset and the training set of HiEve. ByteTrack also ranks 1st among all the trackers on the leaderboard of HiEve and outperforms other state-of-the-art trackers by a large margin. For example, it increases MOTA from 40.9 to 61.3 and IDF1 from 45.1 to 62.9. The superior results indicate that ByteTrack is robust to complex scenes (Table [7](#Tab7)).

**Table 7. Comparison of the state-of-the-art methods on BDD100K test set. The best results are shown in **bold**. **ByteTrack** ranks 1st among all the trackers on the leaderboard of BDD100K and outperforms the second one QDTrack by a large margin on most metrics. The methods denoted by \* are the ones reported on the leaderboard of BDD100K.**

**BDD100K.** BDD100K is multiple categories tracking dataset in autonomous driving scenes. The challenges include low frame rate and large camera motion. We utilize a simple ResNet-50 ImageNet classification model from UniTrack \[[^67]\] to compute appearance similarity. ByteTrack ranks first on the leaderboard of BDD100K. It increases mMOTA from 36.6 to 45.5 on the validation set and 35.5 to 40.1 on the test set, which indicates that ByteTrack can also handle the challenges in autonomous driving scenes.

## 5 Conclusion

We present a simple yet effective data association method BYTE for multi-object tracking. BYTE can be easily applied to existing trackers and achieve consistent improvements. We also propose a strong tracker ByteTrack, which achieves 80.3 MOTA, 77.3 IDF1 and 63.1 HOTA on MOT17 test set with 30 FPS. ByteTrack is very robust to occlusion for its accurate detection performance and the help of associating low score detection boxes. It also sheds light on how to make the best use of detection results to enhance multi-object tracking. We hope the high accuracy, fast speed and simplicity of ByteTrack can make it attractive in real applications.

## Notes

1. 1.
	[https://motchallenge.net](https://motchallenge.net/).
2. 2.
	[http://humaninevents.org](http://humaninevents.org/).

## References

## Acknowledgement

This work was in part supported by NSFC (No. 61733007 and No. 61876212). Ping Luo is supported by the General Research Fund of HK No.27208720, No. 17212120, and No. 17200622.

## Editor information

### Editors and Affiliations

## 1 Electronic supplementary material

Below is the link to the electronic supplementary material.

### Supplementary material 1 (pdf 2281 KB) (download PDF )

## Rights and permissions

## About this paper

### Cite this paper

Zhang, Y. *et al.* (2022). ByteTrack: Multi-object Tracking by Associating Every Detection Box. In: Avidan, S., Brostow, G., Cissé, M., Farinella, G.M., Hassner, T. (eds) Computer Vision – ECCV 2022. ECCV 2022. Lecture Notes in Computer Science, vol 13682. Springer, Cham. https://doi.org/10.1007/978-3-031-20047-2\_1

- [.RIS](https://citation-needed.springer.com/v2/references/10.1007/978-3-031-20047-2_1?format=refman&flavour=citation "Download this article's citation as a .RIS file")
- [.ENW](https://citation-needed.springer.com/v2/references/10.1007/978-3-031-20047-2_1?format=endnote&flavour=citation "Download this article's citation as a .ENW file")
- [.BIB](https://citation-needed.springer.com/v2/references/10.1007/978-3-031-20047-2_1?format=bibtex&flavour=citation "Download this article's citation as a .BIB file")
- DOI https://doi.org/10.1007/978-3-031-20047-2\_1
- Published 23 October 2022
- Publisher NameSpringer, Cham
- Print ISBN978-3-031-20046-5
- Online ISBN978-3-031-20047-2
- eBook Packages [Computer Science](https://link.springer.com/search?facet-content-type=%22Book%22&package=11645&facet-start-year=2022&facet-end-year=2022) [Computer Science (R0)](https://link.springer.com/search?facet-content-type=%22Book%22&package=43710&facet-start-year=2022&facet-end-year=2022) [Springer Nature Proceedings Computer Science](https://link.springer.com/search?facet-content-type=%22Book%22&package=85344&facet-start-year=2022&facet-end-year=2022)

### Keywords

## Publish with us

[Policies and ethics](https://www.springernature.com/gp/policies/book-publishing-policies)

[^1]: Bae, S.H., Yoon, K.J.: Robust online multi-object tracking based on tracklet confidence and online discriminative appearance learning. In: Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition, pp. 1218–1225 (2014)

[Google Scholar](https://scholar.google.com/scholar?&q=Bae%2C%20S.H.%2C%20Yoon%2C%20K.J.%3A%20Robust%20online%20multi-object%20tracking%20based%20on%20tracklet%20confidence%20and%20online%20discriminative%20appearance%20learning.%20In%3A%20Proceedings%20of%20the%20IEEE%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%201218%E2%80%931225%20%282014%29)

[^2]: Berclaz, J., Fleuret, F., Turetken, E., Fua, P.: Multiple object tracking using k-shortest paths optimization. IEEE Trans. Pattern Anal. Mach. Intell. **33** (9), 1806–1819 (2011)

[Article](https://doi.org/10.1109%2FTPAMI.2011.21) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Multiple%20object%20tracking%20using%20k-shortest%20paths%20optimization&journal=IEEE%20Trans.%20Pattern%20Anal.%20Mach.%20Intell.&volume=33&issue=9&pages=1806-1819&publication_year=2011&author=Berclaz%2CJ&author=Fleuret%2CF&author=Turetken%2CE&author=Fua%2CP)

[^3]: Bergmann, P., Meinhardt, T., Leal-Taixe, L.: Tracking without bells and whistles. In: ICCV, pp. 941–951 (2019)

[Google Scholar](https://scholar.google.com/scholar?&q=Bergmann%2C%20P.%2C%20Meinhardt%2C%20T.%2C%20Leal-Taixe%2C%20L.%3A%20Tracking%20without%20bells%20and%20whistles.%20In%3A%20ICCV%2C%20pp.%20941%E2%80%93951%20%282019%29)

[^4]: Bernardin, K., Stiefelhagen, R.: Evaluating multiple object tracking performance: the clear mot metrics. EURASIP J. Image Video Process. **2008**, 1–10 (2008)

[Article](https://doi.org/10.1155%2F2008%2F246309) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Evaluating%20multiple%20object%20tracking%20performance%3A%20the%20clear%20mot%20metrics&journal=EURASIP%20J.%20Image%20Video%20Process.&volume=2008&pages=1-10&publication_year=2008&author=Bernardin%2CK&author=Stiefelhagen%2CR)

[^5]: Bertinetto, L., Valmadre, J., Henriques, J.F., Vedaldi, A., Torr, P.H.S.: Fully-convolutional Siamese networks for object tracking. In: Hua, G., Jégou, H. (eds.) ECCV 2016. LNCS, vol. 9914, pp. 850–865. Springer, Cham (2016). [https://doi.org/10.1007/978-3-319-48881-3\_56](https://doi.org/10.1007/978-3-319-48881-3_56)

[Chapter](https://link.springer.com/doi/10.1007/978-3-319-48881-3_56) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Fully-convolutional%20Siamese%20networks%20for%20object%20tracking&pages=850-865&publication_year=2016%202016%202016&author=Bertinetto%2CL&author=Valmadre%2CJ&author=Henriques%2CJF&author=Vedaldi%2CA&author=Torr%2CPHS)

[^6]: Bewley, A., Ge, Z., Ott, L., Ramos, F., Upcroft, B.: Simple online and realtime tracking. In: ICIP, pp. 3464–3468. IEEE (2016)

[Google Scholar](https://scholar.google.com/scholar?&q=Bewley%2C%20A.%2C%20Ge%2C%20Z.%2C%20Ott%2C%20L.%2C%20Ramos%2C%20F.%2C%20Upcroft%2C%20B.%3A%20Simple%20online%20and%20realtime%20tracking.%20In%3A%20ICIP%2C%20pp.%203464%E2%80%933468.%20IEEE%20%282016%29)

[^7]: Bochinski, E., Eiselein, V., Sikora, T.: High-speed tracking-by-detection without using image information. In: 2017 14th IEEE International Conference on Advanced Video and Signal Based Surveillance (AVSS), pp. 1–6. IEEE (2017)

[Google Scholar](https://scholar.google.com/scholar?&q=Bochinski%2C%20E.%2C%20Eiselein%2C%20V.%2C%20Sikora%2C%20T.%3A%20High-speed%20tracking-by-detection%20without%20using%20image%20information.%20In%3A%202017%2014th%20IEEE%20International%20Conference%20on%20Advanced%20Video%20and%20Signal%20Based%20Surveillance%20%28AVSS%29%2C%20pp.%201%E2%80%936.%20IEEE%20%282017%29)

[^8]: Bochkovskiy, A., Wang, C.Y., Liao, H.Y.M.: YOLOv4: optimal speed and accuracy of object detection. arXiv preprint [arXiv:2004.10934](http://arxiv.org/abs/2004.10934) (2020)

[^9]: Brasó, G., Leal-Taixé, L.: Learning a neural solver for multiple object tracking. In: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pp. 6247–6257 (2020)

[Google Scholar](https://scholar.google.com/scholar?&q=Bras%C3%B3%2C%20G.%2C%20Leal-Taix%C3%A9%2C%20L.%3A%20Learning%20a%20neural%20solver%20for%20multiple%20object%20tracking.%20In%3A%20Proceedings%20of%20the%20IEEE%2FCVF%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%206247%E2%80%936257%20%282020%29)

[^10]: Cai, J., Xu, M., Li, W., Xiong, Y., Xia, W., Tu, Z., Soatto, S.: MeMOT: multi-object tracking with memory. In: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pp. 8090–8100 (2022)

[Google Scholar](https://scholar.google.com/scholar?&q=Cai%2C%20J.%2C%20Xu%2C%20M.%2C%20Li%2C%20W.%2C%20Xiong%2C%20Y.%2C%20Xia%2C%20W.%2C%20Tu%2C%20Z.%2C%20Soatto%2C%20S.%3A%20MeMOT%3A%20multi-object%20tracking%20with%20memory.%20In%3A%20Proceedings%20of%20the%20IEEE%2FCVF%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%208090%E2%80%938100%20%282022%29)

[^11]: Cai, Z., Vasconcelos, N.: Cascade R-CNN: delving into high quality object detection. In: CVPR, pp. 6154–6162 (2018)

[Google Scholar](https://scholar.google.com/scholar?&q=Cai%2C%20Z.%2C%20Vasconcelos%2C%20N.%3A%20Cascade%20R-CNN%3A%20delving%20into%20high%20quality%20object%20detection.%20In%3A%20CVPR%2C%20pp.%206154%E2%80%936162%20%282018%29)

[^12]: Carion, N., Massa, F., Synnaeve, G., Usunier, N., Kirillov, A., Zagoruyko, S.: End-to-end object detection with transformers. In: Vedaldi, A., Bischof, H., Brox, T., Frahm, J.-M. (eds.) ECCV 2020. LNCS, vol. 12346, pp. 213–229. Springer, Cham (2020). [https://doi.org/10.1007/978-3-030-58452-8\_13](https://doi.org/10.1007/978-3-030-58452-8_13)

[Chapter](https://link.springer.com/doi/10.1007/978-3-030-58452-8_13) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=End-to-end%20object%20detection%20with%20transformers&pages=213-229&publication_year=2020%202020%202020&author=Carion%2CN&author=Massa%2CF&author=Synnaeve%2CG&author=Usunier%2CN&author=Kirillov%2CA&author=Zagoruyko%2CS)

[^13]: Chen, L., Ai, H., Zhuang, Z., Shang, C.: Real-time multiple people tracking with deeply learned candidate selection and person re-identification. In: 2018 IEEE International Conference on Multimedia and Expo (ICME), pp. 1–6. IEEE (2018)

[Google Scholar](https://scholar.google.com/scholar?&q=Chen%2C%20L.%2C%20Ai%2C%20H.%2C%20Zhuang%2C%20Z.%2C%20Shang%2C%20C.%3A%20Real-time%20multiple%20people%20tracking%20with%20deeply%20learned%20candidate%20selection%20and%20person%20re-identification.%20In%3A%202018%20IEEE%20International%20Conference%20on%20Multimedia%20and%20Expo%20%28ICME%29%2C%20pp.%201%E2%80%936.%20IEEE%20%282018%29)

[^14]: Chu, P., Fan, H., Tan, C.C., Ling, H.: Online multi-object tracking with instance-aware tracker and dynamic model refreshment. In: 2019 IEEE Winter Conference on Applications of Computer Vision (WACV), pp. 161–170. IEEE (2019)

[Google Scholar](https://scholar.google.com/scholar?&q=Chu%2C%20P.%2C%20Fan%2C%20H.%2C%20Tan%2C%20C.C.%2C%20Ling%2C%20H.%3A%20Online%20multi-object%20tracking%20with%20instance-aware%20tracker%20and%20dynamic%20model%20refreshment.%20In%3A%202019%20IEEE%20Winter%20Conference%20on%20Applications%20of%20Computer%20Vision%20%28WACV%29%2C%20pp.%20161%E2%80%93170.%20IEEE%20%282019%29)

[^15]: Chu, P., Ling, H.: FAMNet: joint learning of feature, affinity and multi-dimensional assignment for online multiple object tracking. In: ICCV, pp. 6172–6181 (2019)

[Google Scholar](https://scholar.google.com/scholar?&q=Chu%2C%20P.%2C%20Ling%2C%20H.%3A%20FAMNet%3A%20joint%20learning%20of%20feature%2C%20affinity%20and%20multi-dimensional%20assignment%20for%20online%20multiple%20object%20tracking.%20In%3A%20ICCV%2C%20pp.%206172%E2%80%936181%20%282019%29)

[^16]: Chu, P., Wang, J., You, Q., Ling, H., Liu, Z.: TransMOT: spatial-temporal graph transformer for multiple object tracking. arXiv preprint [arXiv:2104.00194](http://arxiv.org/abs/2104.00194) (2021)

[^17]: Dendorfer, P., et al.: MOT20: a benchmark for multi object tracking in crowded scenes. arXiv preprint [arXiv:2003.09003](http://arxiv.org/abs/2003.09003) (2020)

[^18]: Dicle, C., Camps, O.I., Sznaier, M.: The way they move: tracking multiple targets with similar appearance. In: Proceedings of the IEEE International Conference on Computer Vision, pp. 2304–2311 (2013)

[Google Scholar](https://scholar.google.com/scholar?&q=Dicle%2C%20C.%2C%20Camps%2C%20O.I.%2C%20Sznaier%2C%20M.%3A%20The%20way%20they%20move%3A%20tracking%20multiple%20targets%20with%20similar%20appearance.%20In%3A%20Proceedings%20of%20the%20IEEE%20International%20Conference%20on%20Computer%20Vision%2C%20pp.%202304%E2%80%932311%20%282013%29)

[^19]: Dosovitskiy, A., et al.: An image is worth 16x16 words: transformers for image recognition at scale. arXiv preprint [arXiv:2010.11929](http://arxiv.org/abs/2010.11929) (2020)

[^20]: Ess, A., Leibe, B., Schindler, K., Van Gool, L.: A mobile vision system for robust multi-person tracking. In: CVPR, pp. 1–8. IEEE (2008)

[Google Scholar](https://scholar.google.com/scholar?&q=Ess%2C%20A.%2C%20Leibe%2C%20B.%2C%20Schindler%2C%20K.%2C%20Van%20Gool%2C%20L.%3A%20A%20mobile%20vision%20system%20for%20robust%20multi-person%20tracking.%20In%3A%20CVPR%2C%20pp.%201%E2%80%938.%20IEEE%20%282008%29)

[^21]: Fang, Y., et al.: You only look at one sequence: rethinking transformer in vision through object detection. Adv. Neural. Inf. Process. Syst. **34**, 26183–26197 (2021)

[Google Scholar](https://scholar.google.com/scholar_lookup?&title=You%20only%20look%20at%20one%20sequence%3A%20rethinking%20transformer%20in%20vision%20through%20object%20detection&journal=Adv.%20Neural.%20Inf.%20Process.%20Syst.&volume=34&pages=26183-26197&publication_year=2021&author=Fang%2CY)

[^22]: Fang, Y., Yang, S., Wang, S., Ge, Y., Shan, Y., Wang, X.: Unleashing vanilla vision transformer with masked image modeling for object detection. arXiv preprint [arXiv:2204.02964](http://arxiv.org/abs/2204.02964) (2022)

[^23]: Felzenszwalb, P., McAllester, D., Ramanan, D.: A discriminatively trained, multiscale, deformable part model. In: CVPR, pp. 1–8. IEEE (2008)

[Google Scholar](https://scholar.google.com/scholar?&q=Felzenszwalb%2C%20P.%2C%20McAllester%2C%20D.%2C%20Ramanan%2C%20D.%3A%20A%20discriminatively%20trained%2C%20multiscale%2C%20deformable%20part%20model.%20In%3A%20CVPR%2C%20pp.%201%E2%80%938.%20IEEE%20%282008%29)

[^24]: Fu, J., Zong, L., Li, Y., Li, K., Yang, B., Liu, X.: Model adaption object detection system for robot. In: 2020 39th Chinese Control Conference (CCC), pp. 3659–3664. IEEE (2020)

[Google Scholar](https://scholar.google.com/scholar?&q=Fu%2C%20J.%2C%20Zong%2C%20L.%2C%20Li%2C%20Y.%2C%20Li%2C%20K.%2C%20Yang%2C%20B.%2C%20Liu%2C%20X.%3A%20Model%20adaption%20object%20detection%20system%20for%20robot.%20In%3A%202020%2039th%20Chinese%20Control%20Conference%20%28CCC%29%2C%20pp.%203659%E2%80%933664.%20IEEE%20%282020%29)

[^25]: Ge, Z., Liu, S., Wang, F., Li, Z., Sun, J.: YOLOX: exceeding YOLO series in 2021. arXiv preprint [arXiv:2107.08430](http://arxiv.org/abs/2107.08430) (2021)

[^26]: Han, S., et al.: MAT: motion-aware multi-object tracking. arXiv preprint [arXiv:2009.04794](http://arxiv.org/abs/2009.04794) (2020)

[^27]: He, K., Gkioxari, G., Dollár, P., Girshick, R.: Mask R-CNN. In: ICCV, pp. 2961–2969 (2017)

[Google Scholar](https://scholar.google.com/scholar?&q=He%2C%20K.%2C%20Gkioxari%2C%20G.%2C%20Doll%C3%A1r%2C%20P.%2C%20Girshick%2C%20R.%3A%20Mask%20R-CNN.%20In%3A%20ICCV%2C%20pp.%202961%E2%80%932969%20%282017%29)

[^28]: He, L., Liao, X., Liu, W., Liu, X., Cheng, P., Mei, T.: FastReID: a PyTorch toolbox for general instance re-identification. arXiv preprint [arXiv:2006.02631](http://arxiv.org/abs/2006.02631) (2020)

[^29]: Hornakova, A., Henschel, R., Rosenhahn, B., Swoboda, P.: Lifted disjoint paths with application in multiple object tracking. In: International Conference on Machine Learning, pp. 4364–4375. PMLR (2020)

[Google Scholar](https://scholar.google.com/scholar?&q=Hornakova%2C%20A.%2C%20Henschel%2C%20R.%2C%20Rosenhahn%2C%20B.%2C%20Swoboda%2C%20P.%3A%20Lifted%20disjoint%20paths%20with%20application%20in%20multiple%20object%20tracking.%20In%3A%20International%20Conference%20on%20Machine%20Learning%2C%20pp.%204364%E2%80%934375.%20PMLR%20%282020%29)

[^30]: Kalman, R.E.: A new approach to linear filtering and prediction problems. J. Fluids Eng. **82** (1), 35–45 (1960)

[MathSciNet](http://www.ams.org/mathscinet-getitem?mr=3931993) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=A%20new%20approach%20to%20linear%20filtering%20and%20prediction%20problems&journal=J.%20Fluids%20Eng.&volume=82&issue=1&pages=35-45&publication_year=1960&author=Kalman%2CRE)

[^31]: Khurana, T., Dave, A., Ramanan, D.: Detecting invisible people. arXiv preprint [arXiv:2012.08419](http://arxiv.org/abs/2012.08419) (2020)

[^32]: Kuhn, H.W.: The Hungarian method for the assignment problem. Naval Res. Logistics Q. **2** (1–2), 83–97 (1955)

[Article](https://doi.org/10.1002%2Fnav.3800020109) [MathSciNet](http://www.ams.org/mathscinet-getitem?mr=75510) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=The%20Hungarian%20method%20for%20the%20assignment%20problem&journal=Naval%20Res.%20Logistics%20Q.&volume=2&issue=1%E2%80%932&pages=83-97&publication_year=1955&author=Kuhn%2CHW)

[^33]: Li, W., Xiong, Y., Yang, S., Xu, M., Wang, Y., Xia, W.: Semi-TCL: semi-supervised track contrastive representation learning. arXiv preprint [arXiv:2107.02396](http://arxiv.org/abs/2107.02396) (2021)

[^34]: Liang, C., et al.: Rethinking the competition between detection and ReID in multi-object tracking. arXiv preprint [arXiv:2010.12138](http://arxiv.org/abs/2010.12138) (2020)

[^35]: Liang, C., Zhang, Z., Zhou, X., Li, B., Lu, Y., Hu, W.: One more check: making “fake background” be tracked again. arXiv preprint [arXiv:2104.09441](http://arxiv.org/abs/2104.09441) (2021)

[^36]: Lin, T.Y., Goyal, P., Girshick, R., He, K., Dollár, P.: Focal loss for dense object detection. In: ICCV, pp. 2980–2988 (2017)

[Google Scholar](https://scholar.google.com/scholar?&q=Lin%2C%20T.Y.%2C%20Goyal%2C%20P.%2C%20Girshick%2C%20R.%2C%20He%2C%20K.%2C%20Doll%C3%A1r%2C%20P.%3A%20Focal%20loss%20for%20dense%20object%20detection.%20In%3A%20ICCV%2C%20pp.%202980%E2%80%932988%20%282017%29)

[^37]: Lin, T.-Y., et al.: Microsoft COCO: common objects in context. In: Fleet, D., Pajdla, T., Schiele, B., Tuytelaars, T. (eds.) ECCV 2014. LNCS, vol. 8693, pp. 740–755. Springer, Cham (2014). [https://doi.org/10.1007/978-3-319-10602-1\_48](https://doi.org/10.1007/978-3-319-10602-1_48)

[Chapter](https://link.springer.com/doi/10.1007/978-3-319-10602-1_48) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Microsoft%20COCO%3A%20common%20objects%20in%20context&pages=740-755&publication_year=2014%202014%202014&author=Lin%2CT-Y)

[^38]: Lin, W., et al.: Human in events: a large-scale benchmark for human-centric video analysis in complex events. arXiv preprint [arXiv:2005.04490](http://arxiv.org/abs/2005.04490) (2020)

[^39]: Liu, Z., et al.: Swin transformer: hierarchical vision transformer using shifted windows. arXiv preprint [arXiv:2103.14030](http://arxiv.org/abs/2103.14030) (2021)

[^40]: Lu, Z., Rathod, V., Votel, R., Huang, J.: RetinaTrack: online single stage joint detection and tracking. In: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pp. 14668–14678 (2020)

[Google Scholar](https://scholar.google.com/scholar?&q=Lu%2C%20Z.%2C%20Rathod%2C%20V.%2C%20Votel%2C%20R.%2C%20Huang%2C%20J.%3A%20RetinaTrack%3A%20online%20single%20stage%20joint%20detection%20and%20tracking.%20In%3A%20Proceedings%20of%20the%20IEEE%2FCVF%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%2014668%E2%80%9314678%20%282020%29)

[^41]: Luiten, J., et al.: HOTA: a higher order metric for evaluating multi-object tracking. Int. J. Comput. Vision **129** (2), 548–578 (2021)

[Article](https://link.springer.com/doi/10.1007/s11263-020-01375-2) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=HOTA%3A%20a%20higher%20order%20metric%20for%20evaluating%20multi-object%20tracking&journal=Int.%20J.%20Comput.%20Vision&volume=129&issue=2&pages=548-578&publication_year=2021&author=Luiten%2CJ)

[^42]: Luo, H., Xie, W., Wang, X., Zeng, W.: Detect or track: towards cost-effective video object detection/tracking. In: Proceedings of the AAAI Conference on Artificial Intelligence, vol. 33, pp. 8803–8810 (2019)

[Google Scholar](https://scholar.google.com/scholar?&q=Luo%2C%20H.%2C%20Xie%2C%20W.%2C%20Wang%2C%20X.%2C%20Zeng%2C%20W.%3A%20Detect%20or%20track%3A%20towards%20cost-effective%20video%20object%20detection%2Ftracking.%20In%3A%20Proceedings%20of%20the%20AAAI%20Conference%20on%20Artificial%20Intelligence%2C%20vol.%2033%2C%20pp.%208803%E2%80%938810%20%282019%29)

[^43]: Meinhardt, T., Kirillov, A., Leal-Taixe, L., Feichtenhofer, C.: TrackFormer: multi-object tracking with transformers. arXiv preprint [arXiv:2101.02702](http://arxiv.org/abs/2101.02702) (2021)

[^44]: Micikevicius, P., et al.: Mixed precision training. arXiv preprint [arXiv:1710.03740](http://arxiv.org/abs/1710.03740) (2017)

[^45]: Milan, A., Leal-Taixé, L., Reid, I., Roth, S., Schindler, K.: MOT16: a benchmark for multi-object tracking. arXiv preprint [arXiv:1603.00831](http://arxiv.org/abs/1603.00831) (2016)

[^46]: Milan, A., Roth, S., Schindler, K.: Continuous energy minimization for multitarget tracking. IEEE Trans. Pattern Anal. Mach. Intell. **36** (1), 58–72 (2013)

[Article](https://doi.org/10.1109%2FTPAMI.2013.103) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Continuous%20energy%20minimization%20for%20multitarget%20tracking&journal=IEEE%20Trans.%20Pattern%20Anal.%20Mach.%20Intell.&volume=36&issue=1&pages=58-72&publication_year=2013&author=Milan%2CA&author=Roth%2CS&author=Schindler%2CK)

[^47]: Pang, B., Li, Y., Zhang, Y., Li, M., Lu, C.: TubeTK: adopting tubes to track multi-object in a one-step training model. In: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pp. 6308–6318 (2020)

[Google Scholar](https://scholar.google.com/scholar?&q=Pang%2C%20B.%2C%20Li%2C%20Y.%2C%20Zhang%2C%20Y.%2C%20Li%2C%20M.%2C%20Lu%2C%20C.%3A%20TubeTK%3A%20adopting%20tubes%20to%20track%20multi-object%20in%20a%20one-step%20training%20model.%20In%3A%20Proceedings%20of%20the%20IEEE%2FCVF%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%206308%E2%80%936318%20%282020%29)

[^48]: Pang, J., et al.: Quasi-dense similarity learning for multiple object tracking. In: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pp. 164–173 (2021)

[Google Scholar](https://scholar.google.com/scholar?&q=Pang%2C%20J.%2C%20et%20al.%3A%20Quasi-dense%20similarity%20learning%20for%20multiple%20object%20tracking.%20In%3A%20Proceedings%20of%20the%20IEEE%2FCVF%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%20164%E2%80%93173%20%282021%29)

[^49]: Peng, J., et al.: Chained-tracker: chaining paired attentive regression results for end-to-end joint multiple-object detection and tracking. In: Vedaldi, A., Bischof, H., Brox, T., Frahm, J.-M. (eds.) ECCV 2020. LNCS, vol. 12349, pp. 145–161. Springer, Cham (2020). [https://doi.org/10.1007/978-3-030-58548-8\_9](https://doi.org/10.1007/978-3-030-58548-8_9)

[Chapter](https://link.springer.com/doi/10.1007/978-3-030-58548-8_9) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Chained-tracker%3A%20chaining%20paired%20attentive%20regression%20results%20for%20end-to-end%20joint%20multiple-object%20detection%20and%20tracking&pages=145-161&publication_year=2020%202020%202020&author=Peng%2CJ)

[^50]: Redmon, J., Farhadi, A.: YOLOv3: an incremental improvement. arXiv preprint [arXiv:1804.02767](http://arxiv.org/abs/1804.02767) (2018)

[^51]: Ren, S., He, K., Girshick, R., Sun, J.: Faster R-CNN: towards real-time object detection with region proposal networks. In: Advances in Neural Information Processing Systems, pp. 91–99 (2015)

[Google Scholar](https://scholar.google.com/scholar?&q=Ren%2C%20S.%2C%20He%2C%20K.%2C%20Girshick%2C%20R.%2C%20Sun%2C%20J.%3A%20Faster%20R-CNN%3A%20towards%20real-time%20object%20detection%20with%20region%20proposal%20networks.%20In%3A%20Advances%20in%20Neural%20Information%20Processing%20Systems%2C%20pp.%2091%E2%80%9399%20%282015%29)

[^52]: Ristani, E., Solera, F., Zou, R., Cucchiara, R., Tomasi, C.: Performance measures and a data set for multi-target, multi-camera tracking. In: Hua, G., Jégou, H. (eds.) ECCV 2016. LNCS, vol. 9914, pp. 17–35. Springer, Cham (2016). [https://doi.org/10.1007/978-3-319-48881-3\_2](https://doi.org/10.1007/978-3-319-48881-3_2)

[Chapter](https://link.springer.com/doi/10.1007/978-3-319-48881-3_2) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Performance%20measures%20and%20a%20data%20set%20for%C2%A0multi-target%2C%20multi-camera%20tracking&pages=17-35&publication_year=2016%202016%202016&author=Ristani%2CE&author=Solera%2CF&author=Zou%2CR&author=Cucchiara%2CR&author=Tomasi%2CC)

[^53]: Sanchez-Matilla, R., Poiesi, F., Cavallaro, A.: Online multi-target tracking with strong and weak detections. In: Hua, G., Jégou, H. (eds.) ECCV 2016. LNCS, vol. 9914, pp. 84–99. Springer, Cham (2016). [https://doi.org/10.1007/978-3-319-48881-3\_7](https://doi.org/10.1007/978-3-319-48881-3_7)

[Chapter](https://link.springer.com/doi/10.1007/978-3-319-48881-3_7) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Online%20multi-target%20tracking%20with%20strong%20and%20weak%20detections&pages=84-99&publication_year=2016%202016%202016&author=Sanchez-Matilla%2CR&author=Poiesi%2CF&author=Cavallaro%2CA)

[^54]: Shan, C., et al.: Tracklets predicting based adaptive graph tracking. arXiv preprint [arXiv:2010.09015](http://arxiv.org/abs/2010.09015) (2020)

[^55]: Shao, S., et al.: CrowdHuman: a benchmark for detecting human in a crowd. arXiv preprint [arXiv:1805.00123](http://arxiv.org/abs/1805.00123) (2018)

[^56]: Shuai, B., Berneshawi, A., Li, X., Modolo, D., Tighe, J.: SiamMOT: Siamese multi-object tracking. In: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pp. 12372–12382 (2021)

[Google Scholar](https://scholar.google.com/scholar?&q=Shuai%2C%20B.%2C%20Berneshawi%2C%20A.%2C%20Li%2C%20X.%2C%20Modolo%2C%20D.%2C%20Tighe%2C%20J.%3A%20SiamMOT%3A%20Siamese%20multi-object%20tracking.%20In%3A%20Proceedings%20of%20the%20IEEE%2FCVF%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%2012372%E2%80%9312382%20%282021%29)

[^57]: Sun, P., et al.: What makes for end-to-end object detection? In: Proceedings of the 38th International Conference on Machine Learning. Proceedings of Machine Learning Research, vol. 139, pp. 9934–9944. PMLR (2021)

[Google Scholar](https://scholar.google.com/scholar?&q=Sun%2C%20P.%2C%20et%20al.%3A%20What%20makes%20for%20end-to-end%20object%20detection%3F%20In%3A%20Proceedings%20of%20the%2038th%20International%20Conference%20on%20Machine%20Learning.%20Proceedings%20of%20Machine%20Learning%20Research%2C%20vol.%20139%2C%20pp.%209934%E2%80%939944.%20PMLR%20%282021%29)

[^58]: Sun, P., et al.: TransTrack: multiple-object tracking with transformer. arXiv preprint [arXiv:2012.15460](http://arxiv.org/abs/2012.15460) (2020)

[^59]: Sun, P., et al.: Sparse R-CNN: end-to-end object detection with learnable proposals. In: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pp. 14454–14463 (2021)

[Google Scholar](https://scholar.google.com/scholar?&q=Sun%2C%20P.%2C%20et%20al.%3A%20Sparse%20R-CNN%3A%20end-to-end%20object%20detection%20with%20learnable%20proposals.%20In%3A%20Proceedings%20of%20the%20IEEE%2FCVF%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%2014454%E2%80%9314463%20%282021%29)

[^60]: Tang, P., Wang, C., Wang, X., Liu, W., Zeng, W., Wang, J.: Object detection in videos by high quality object linking. IEEE Trans. Pattern Anal. Mach. Intell. **42** (5), 1272–1278 (2019)

[Article](https://doi.org/10.1109%2FTPAMI.2019.2910529) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Object%20detection%20in%20videos%20by%20high%20quality%20object%20linking&journal=IEEE%20Trans.%20Pattern%20Anal.%20Mach.%20Intell.&volume=42&issue=5&pages=1272-1278&publication_year=2019&author=Tang%2CP&author=Wang%2CC&author=Wang%2CX&author=Liu%2CW&author=Zeng%2CW&author=Wang%2CJ)

[^61]: Tokmakov, P., Li, J., Burgard, W., Gaidon, A.: Learning to track with object permanence. arXiv preprint [arXiv:2103.14258](http://arxiv.org/abs/2103.14258) (2021)

[^62]: Vaswani, A., et al.: Attention is all you need. In: Advances in Neural Information Processing Systems, pp. 5998–6008 (2017)

[Google Scholar](https://scholar.google.com/scholar?&q=Vaswani%2C%20A.%2C%20et%20al.%3A%20Attention%20is%20all%20you%20need.%20In%3A%20Advances%20in%20Neural%20Information%20Processing%20Systems%2C%20pp.%205998%E2%80%936008%20%282017%29)

[^63]: Wang, C.Y., Bochkovskiy, A., Liao, H.Y.M.: YOLOv7: trainable bag-of-freebies sets new state-of-the-art for real-time object detectors. arXiv preprint [arXiv:2207.02696](http://arxiv.org/abs/2207.02696) (2022)

[^64]: Wang, Q., Zheng, Y., Pan, P., Xu, Y.: Multiple object tracking with correlation learning. In: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pp. 3876–3886 (2021)

[Google Scholar](https://scholar.google.com/scholar?&q=Wang%2C%20Q.%2C%20Zheng%2C%20Y.%2C%20Pan%2C%20P.%2C%20Xu%2C%20Y.%3A%20Multiple%20object%20tracking%20with%20correlation%20learning.%20In%3A%20Proceedings%20of%20the%20IEEE%2FCVF%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%203876%E2%80%933886%20%282021%29)

[^65]: Wang, W., et al.: Pyramid vision transformer: a versatile backbone for dense prediction without convolutions. arXiv preprint [arXiv:2102.12122](http://arxiv.org/abs/2102.12122) (2021)

[^66]: Wang, Y., Kitani, K., Weng, X.: Joint object detection and multi-object tracking with graph neural networks. arXiv preprint [arXiv:2006.13164](http://arxiv.org/abs/2006.13164) (2020)

[^67]: Wang, Z., Zhao, H., Li, Y.L., Wang, S., Torr, P.H., Bertinetto, L.: Do different tracking tasks require different appearance models? arXiv preprint [arXiv:2107.02156](http://arxiv.org/abs/2107.02156) (2021)

[^68]: Wang, Z., Zheng, L., Liu, Y., Li, Y., Wang, S.: Towards real-time multi-object tracking. In: Vedaldi, A., Bischof, H., Brox, T., Frahm, J.-M. (eds.) ECCV 2020. LNCS, vol. 12356, pp. 107–122. Springer, Cham (2020). [https://doi.org/10.1007/978-3-030-58621-8\_7](https://doi.org/10.1007/978-3-030-58621-8_7)

[Chapter](https://link.springer.com/doi/10.1007/978-3-030-58621-8_7) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Towards%20real-time%20multi-object%20tracking&pages=107-122&publication_year=2020%202020%202020&author=Wang%2CZ&author=Zheng%2CL&author=Liu%2CY&author=Li%2CY&author=Wang%2CS)

[^69]: Wojke, N., Bewley, A., Paulus, D.: Simple online and realtime tracking with a deep association metric. In: 2017 IEEE International Conference on Image Processing (ICIP), pp. 3645–3649. IEEE (2017)

[Google Scholar](https://scholar.google.com/scholar?&q=Wojke%2C%20N.%2C%20Bewley%2C%20A.%2C%20Paulus%2C%20D.%3A%20Simple%20online%20and%20realtime%20tracking%20with%20a%20deep%20association%20metric.%20In%3A%202017%20IEEE%20International%20Conference%20on%20Image%20Processing%20%28ICIP%29%2C%20pp.%203645%E2%80%933649.%20IEEE%20%282017%29)

[^70]: Wu, J., Cao, J., Song, L., Wang, Y., Yang, M., Yuan, J.: Track to detect and segment: an online multi-object tracker. In: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pp. 12352–12361 (2021)

[Google Scholar](https://scholar.google.com/scholar?&q=Wu%2C%20J.%2C%20Cao%2C%20J.%2C%20Song%2C%20L.%2C%20Wang%2C%20Y.%2C%20Yang%2C%20M.%2C%20Yuan%2C%20J.%3A%20Track%20to%20detect%20and%20segment%3A%20an%20online%20multi-object%20tracker.%20In%3A%20Proceedings%20of%20the%20IEEE%2FCVF%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%2012352%E2%80%9312361%20%282021%29)

[^71]: Xiang, Y., Alahi, A., Savarese, S.: Learning to track: online multi-object tracking by decision making. In: ICCV, pp. 4705–4713 (2015)

[Google Scholar](https://scholar.google.com/scholar?&q=Xiang%2C%20Y.%2C%20Alahi%2C%20A.%2C%20Savarese%2C%20S.%3A%20Learning%20to%20track%3A%20online%20multi-object%20tracking%20by%20decision%20making.%20In%3A%20ICCV%2C%20pp.%204705%E2%80%934713%20%282015%29)

[^72]: Xu, J., Cao, Y., Zhang, Z., Hu, H.: Spatial-temporal relation networks for multi-object tracking. In: Proceedings of the IEEE/CVF International Conference on Computer Vision, pp. 3988–3998 (2019)

[Google Scholar](https://scholar.google.com/scholar?&q=Xu%2C%20J.%2C%20Cao%2C%20Y.%2C%20Zhang%2C%20Z.%2C%20Hu%2C%20H.%3A%20Spatial-temporal%20relation%20networks%20for%20multi-object%20tracking.%20In%3A%20Proceedings%20of%20the%20IEEE%2FCVF%20International%20Conference%20on%20Computer%20Vision%2C%20pp.%203988%E2%80%933998%20%282019%29)

[^73]: Xu, Y., Ban, Y., Delorme, G., Gan, C., Rus, D., Alameda-Pineda, X.: TransCenter: transformers with dense queries for multiple-object tracking. arXiv preprint [arXiv:2103.15145](http://arxiv.org/abs/2103.15145) (2021)

[^74]: Yan, B., et al.: Towards grand unification of object tracking. arXiv preprint [arXiv:2207.07078](http://arxiv.org/abs/2207.07078) (2022)

[^75]: Yang, F., Chang, X., Sakti, S., Wu, Y., Nakamura, S.: ReMOT: a model-agnostic refinement for multiple object tracking. Image Vis. Comput. **106**, 104091 (2021)

[Article](https://doi.org/10.1016%2Fj.imavis.2020.104091) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=ReMOT%3A%20a%20model-agnostic%20refinement%20for%20multiple%20object%20tracking&journal=Image%20Vis.%20Comput.&volume=106&publication_year=2021&author=Yang%2CF&author=Chang%2CX&author=Sakti%2CS&author=Wu%2CY&author=Nakamura%2CS)

[^76]: Yang, F., Choi, W., Lin, Y.: Exploit all the layers: fast and accurate CNN object detector with scale dependent pooling and cascaded rejection classifiers. In: Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition, pp. 2129–2137 (2016)

[Google Scholar](https://scholar.google.com/scholar?&q=Yang%2C%20F.%2C%20Choi%2C%20W.%2C%20Lin%2C%20Y.%3A%20Exploit%20all%20the%20layers%3A%20fast%20and%20accurate%20CNN%20object%20detector%20with%20scale%20dependent%20pooling%20and%20cascaded%20rejection%20classifiers.%20In%3A%20Proceedings%20of%20the%20IEEE%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%202129%E2%80%932137%20%282016%29)

[^77]: Yu, E., Li, Z., Han, S., Wang, H.: RelationTrack: relation-aware multiple object tracking with decoupled representation. arXiv preprint [arXiv:2105.04322](http://arxiv.org/abs/2105.04322) (2021)

[^78]: Yu, F., et al.: BDD100K: a diverse driving dataset for heterogeneous multitask learning. In: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pp. 2636–2645 (2020)

[Google Scholar](https://scholar.google.com/scholar?&q=Yu%2C%20F.%2C%20et%20al.%3A%20BDD100K%3A%20a%20diverse%20driving%20dataset%20for%20heterogeneous%20multitask%20learning.%20In%3A%20Proceedings%20of%20the%20IEEE%2FCVF%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%202636%E2%80%932645%20%282020%29)

[^79]: Zeng, F., Dong, B., Wang, T., Chen, C., Zhang, X., Wei, Y.: MOTR: end-to-end multiple-object tracking with transformer. arXiv preprint [arXiv:2105.03247](http://arxiv.org/abs/2105.03247) (2021)

[^80]: Zhang, H., Cisse, M., Dauphin, Y.N., Lopez-Paz, D.: mixup: beyond empirical risk minimization. arXiv preprint [arXiv:1710.09412](http://arxiv.org/abs/1710.09412) (2017)

[^81]: Zhang, S., Benenson, R., Schiele, B.: CityPersons: a diverse dataset for pedestrian detection. In: CVPR, pp. 3213–3221 (2017)

[Google Scholar](https://scholar.google.com/scholar?&q=Zhang%2C%20S.%2C%20Benenson%2C%20R.%2C%20Schiele%2C%20B.%3A%20CityPersons%3A%20a%20diverse%20dataset%20for%20pedestrian%20detection.%20In%3A%20CVPR%2C%20pp.%203213%E2%80%933221%20%282017%29)

[^82]: Zhang, Y., Sheng, H., Wu, Y., Wang, S., Ke, W., Xiong, Z.: Multiplex labeling graph for near-online tracking in crowded scenes. IEEE Internet Things J. **7** (9), 7892–7902 (2020)

[Article](https://doi.org/10.1109%2FJIOT.2020.2996609) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Multiplex%20labeling%20graph%20for%20near-online%20tracking%20in%20crowded%20scenes&journal=IEEE%20Internet%20Things%20J.&volume=7&issue=9&pages=7892-7902&publication_year=2020&author=Zhang%2CY&author=Sheng%2CH&author=Wu%2CY&author=Wang%2CS&author=Ke%2CW&author=Xiong%2CZ)

[^83]: Zhang, Y., Wang, C., Wang, X., Liu, W., Zeng, W.: VoxelTrack: multi-person 3D human pose estimation and tracking in the wild. IEEE Trans. Pattern Anal. Mach. Intell. (2022)

[Google Scholar](https://scholar.google.com/scholar?&q=Zhang%2C%20Y.%2C%20Wang%2C%20C.%2C%20Wang%2C%20X.%2C%20Liu%2C%20W.%2C%20Zeng%2C%20W.%3A%20VoxelTrack%3A%20multi-person%203D%20human%20pose%20estimation%20and%20tracking%20in%20the%20wild.%20IEEE%20Trans.%20Pattern%20Anal.%20Mach.%20Intell.%20%282022%29)

[^84]: Zhang, Y., Wang, C., Wang, X., Zeng, W., Liu, W.: FairMOT: on the fairness of detection and re-identification in multiple object tracking. Int. J. Comput. Vision **129** (11), 3069–3087 (2021)

[Article](https://link.springer.com/doi/10.1007/s11263-021-01513-4) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=FairMOT%3A%20on%20the%20fairness%20of%20detection%20and%20re-identification%20in%20multiple%20object%20tracking&journal=Int.%20J.%20Comput.%20Vision&volume=129&issue=11&pages=3069-3087&publication_year=2021&author=Zhang%2CY&author=Wang%2CC&author=Wang%2CX&author=Zeng%2CW&author=Liu%2CW)

[^85]: Zhang, Z., Cheng, D., Zhu, X., Lin, S., Dai, J.: Integrated object detection and tracking with tracklet-conditioned detection. arXiv preprint [arXiv:1811.11167](http://arxiv.org/abs/1811.11167) (2018)

[^86]: Zheng, L., Tang, M., Chen, Y., Zhu, G., Wang, J., Lu, H.: Improving multiple object tracking with single object tracking. In: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pp. 2453–2462 (2021)

[Google Scholar](https://scholar.google.com/scholar?&q=Zheng%2C%20L.%2C%20Tang%2C%20M.%2C%20Chen%2C%20Y.%2C%20Zhu%2C%20G.%2C%20Wang%2C%20J.%2C%20Lu%2C%20H.%3A%20Improving%20multiple%20object%20tracking%20with%20single%20object%20tracking.%20In%3A%20Proceedings%20of%20the%20IEEE%2FCVF%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%202453%E2%80%932462%20%282021%29)

[^87]: Zhou, X., Koltun, V., Krähenbühl, P.: Tracking objects as points. In: Vedaldi, A., Bischof, H., Brox, T., Frahm, J.-M. (eds.) ECCV 2020. LNCS, vol. 12349, pp. 474–490. Springer, Cham (2020). [https://doi.org/10.1007/978-3-030-58548-8\_28](https://doi.org/10.1007/978-3-030-58548-8_28)

[Chapter](https://link.springer.com/doi/10.1007/978-3-030-58548-8_28) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Tracking%20objects%20as%20points&pages=474-490&publication_year=2020%202020%202020&author=Zhou%2CX&author=Koltun%2CV&author=Kr%C3%A4henb%C3%BChl%2CP)

[^88]: Zhou, X., Wang, D., Krähenbühl, P.: Objects as points. arXiv preprint [arXiv:1904.07850](http://arxiv.org/abs/1904.07850) (2019)

[^89]: Zhou, X., Yin, T., Koltun, V., Krähenbühl, P.: Global tracking transformers. In: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pp. 8771–8780 (2022)

[Google Scholar](https://scholar.google.com/scholar?&q=Zhou%2C%20X.%2C%20Yin%2C%20T.%2C%20Koltun%2C%20V.%2C%20Kr%C3%A4henb%C3%BChl%2C%20P.%3A%20Global%20tracking%20transformers.%20In%3A%20Proceedings%20of%20the%20IEEE%2FCVF%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%208771%E2%80%938780%20%282022%29)

[^90]: Zhu, J., Yang, H., Liu, N., Kim, M., Zhang, W., Yang, M.-H.: Online multi-object tracking with dual matching attention networks. In: Ferrari, V., Hebert, M., Sminchisescu, C., Weiss, Y. (eds.) ECCV 2018. LNCS, vol. 11209, pp. 379–396. Springer, Cham (2018). [https://doi.org/10.1007/978-3-030-01228-1\_23](https://doi.org/10.1007/978-3-030-01228-1_23)

[Chapter](https://link.springer.com/doi/10.1007/978-3-030-01228-1_23) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Online%20multi-object%20tracking%20with%20dual%20matching%20attention%20networks&pages=379-396&publication_year=2018%202018%202018&author=Zhu%2CJ&author=Yang%2CH&author=Liu%2CN&author=Kim%2CM&author=Zhang%2CW&author=Yang%2CM-H)

[^91]: Zhu, X., Su, W., Lu, L., Li, B., Wang, X., Dai, J.: Deformable DETR: deformable transformers for end-to-end object detection. arXiv preprint [arXiv:2010.04159](http://arxiv.org/abs/2010.04159) (2020)