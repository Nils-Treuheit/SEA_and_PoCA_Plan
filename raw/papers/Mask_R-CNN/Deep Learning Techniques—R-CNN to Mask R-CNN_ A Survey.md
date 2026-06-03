---
title: "Deep Learning Techniques—R-CNN to Mask R-CNN: A Survey"
source: "https://link.springer.com/chapter/10.1007/978-981-13-9042-5_56"
author:
  - "[[Puja Bharati]]"
  - "[[Ankita Pramanik]]"
published: 2020-01-01
created: 2026-06-03
description: "With the advances in the field of machine learning, statistics, and computer vision, the advanced deep learning techniques have attracted increasing research interests over the last decade. This is because of their inherent capabilities of overcoming the drawback of..."
tags:
  - "clippings"
---
## Abstract

With the advances in the field of machine learning, statistics, and computer vision, the advanced deep learning techniques have attracted increasing research interests over the last decade. This is because of their inherent capabilities of overcoming the drawback of traditional techniques. The main contribution of this work is to provide a comprehensive description of region-based convolutional neural network (R-CNN) and its recent improvement like fast R-CNN, faster R-CNN, region-based fully convolutional networks, single shot detector, deconvolutional single shot detector, R-CNN minus R, you only look once (YOLO), mask R-CNN, etc., with brief details. This survey paper presents an overview of the last update in this field and their practical applications and its classification for ease of understanding. The performances and challenges of these techniques in terms of speed, accuracy, or simplicity are also compared. In general, the speed performance of YOLO is approximately 21 ~ 155 fps which is the fastest and the average precision of Mask R-CNN is ~47.3 which outperforms all other techniques.

## 1 Introduction

Convolutional neural network (CNN) is a biologically inspired process. It is an artificial neural network which is feed forward and is most commonly used in computer vision and machine learning to examine visual imagery. The architecture of CNN consists of four layers: convolutional layers which perform convolution, rectified linear unit (ReLU) layers which apply element wise activation function, pooling layers which perform down sampling operation, and fully connected layers (FC) which compute scores of a class. Lecun et al. Network (LeNet) \[[^1]\] is the first CNN model. After this, different CNN architectures are evolved which are deeper such as AlexNet \[[^2]\], Zeiler and Fergus Network (ZF Net) \[[^3]\], Google LeNet (GoogLeNet) \[[^4]\], Visual Geometry Group Network (VGGNet) \[[^5]\], Residual Network (ResNet) \[[^6]\], etc.

Recently, advanced deep learning techniques, especially region-based CNN (R-CNN), have attained remarkable successes in a diversity of tasks in machine learning, statistics, and computer vision, for example, object detection, image categorization, image segmentation, etc. This is because CNN only identifies the object’s class, not the location of object in an image. Especially when multiple objects are in the image then CNN cannot work well due to interference. R-CNN identifies region of interest (RoI) using selective search \[[^7]\] and using a CNN it categories them as foreground and background. Nowadays, R-CNN \[[^8]\] is the best CNN-based detectors as it can identify the location of the object and also work well for an image containing multiple objects.

Due to expensive training and slow object detection, R-CNN is modified and fast R-CNN \[[^9]\], faster R-CNN \[[^10]\], region-based fully convolutional networks (R-FCN) \[[^11]\], single shot detector (SSD) \[[^12]\], you only look once (YOLO) \[[^13]\], grid-CNN (G-CNN) \[[^14]\], spatial pyramid pooling network (SPP-Net) \[[^15]\], R-CNN minus R \[[^16]\], propose network (Pro-Net) \[[^17]\], mask R-CNN \[[^18]\], etc., are evolved one after another. These newer algorithms make the object detection and its classification more accurate and faster. For applying these techniques in real world applications such as face detection \[[^19]\], people detection \[[^20]\], traffic sign detection, and traffic surveillance \[[^21]\], etc., the problem is to choose the suitable model or technique for their application. A trade-off among speed, precision, and simplicity of these techniques is essential to get the desired result. The main and basic applications of all these techniques are object detection and image classification \[[^1], [^7], [^10],[^11],[^12], [^18], [^22], [^23]\]. These models also help in the application of action recognition \[[^24]\], human pose estimation \[[^18]\], instance segmentation \[[^18]\], fault detection, text recognition, etc. These techniques also help in medical diagnosis such as tumor detection, nucleus segmentation \[[^25]\], and much more. For object detection, many datasets such as PASCAL VOC datasets \[[^26]\], Microsoft COCO datasets \[[^27]\], ImageNet datasets, \[[^28]\] etc. are available.

New approach and new techniques in the area of deep learning are evolving each and every day. So, a survey of all the existing techniques is essential for its appropriate application and further development in the field of CNN. The main contributions of this paper are as follows: (1) to give a comprehensive description of R-CNN (2) proposing object detection inspirational tree for ease of understanding (2) to investigate how different techniques or descendants are evolved from one implementation to other and (3) to summarize performance comparisons of the different techniques.

The remaining part of the paper is organized as follows: In the succeeding section, different types of base networks are discussed. The basic principle of R-CNN and the various improvements on R-CNN are presented in Sect. [3](#Sec3). The different techniques are compared in Sect. [4](#Sec10) and this survey paper is finally concluded in Sect. [5](#Sec11).

## 2 Base Network

To understand the working of R-CNN, it is imperative to understand the base network. The performance of the various deep networks is dependent on the base network. The base networks are the base architecture of R-CNN. There are many different types of architecture evolved almost every year such as LeNet \[[^1]\], AlexNet \[[^2]\], ZF Net \[[^3]\], GoogLeNet \[[^4]\], VGG Net \[[^5]\], ResNet \[[^6]\], etc. Some common architecture is described below.

## 3 R-CNN

The R-CNN \[[^8]\] architecture includes three steps as shown in Fig. [1](#Fig1). First step is to take the input image and scan for probable objects using an algorithm called selective search \[[^7]\] which generates ~2000 region proposals. In second step, on the top of each region proposals a CNN is run. Third step is to take the each CNN output and provide this output into (i) a support vector machine (SVM) and (ii) a linear regressor. SVM classifies the region of object in an image and linear regressor tightens the bounding box of the detected object.

**Fig. 1**

[![figure 1](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-981-13-9042-5_56/MediaObjects/469685_1_En_56_Fig1_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-981-13-9042-5_56/figures/1)

R-CNN architecture

R-CNN has some notable drawbacks. (i) To classify ~2000 region proposals per image, it takes huge amount of time to train the network, (ii) Object detection in real time is not possible as it takes approximately 47s for each test image, (iii) R-CNN should train 3 different models separately—CNN that generate image feature, classifier which predict the class, and the regressor model which help to tight the bounding boxes. Hence, the pipeline is really tough to train. Because of these limitations, R-CNN needs improvement.

**Improvement on R-CNN**

Taking the inspiration from R-CNN, different models are evolved year after year. The summary is shown in Fig. [2](#Fig2). The present work proposes this tree for understanding the evolution of the deep networks in a lucid fashion.

**Fig. 2**

[![figure 2](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-981-13-9042-5_56/MediaObjects/469685_1_En_56_Fig2_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-981-13-9042-5_56/figures/2)

Object detection inspirational tree

### 3.1 Fast R-CNN

Fast R-CNN \[[^9]\] approach is similar to that of the R-CNN. The difference is instead of providing the region proposals to the CNN; the input image is directly fed to the CNN for generating convolutional feature map. From this, the region proposals are identified and warped them into squares. To feed into fully connected layer, RoI pooling layer reshape them into a fixed size. To predict the class of the proposed region softmax layer is used instead of SVM as shown in Fig. [3](#Fig3).

**Fig. 3**

[![figure 3](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-981-13-9042-5_56/MediaObjects/469685_1_En_56_Fig3_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-981-13-9042-5_56/figures/3)

Fast R-CNN architecture

This algorithm is faster than R-CNN because ~2 K regions proposals are not required to be fed to the CNN each and every time. It is fed only once to generate the feature map. The training of CNN, classifier and bounding box regressor of fast R-CNN are done simultaneously in a single model.

Instead of having all these advantages, region proposal is the main problem in this technique. These proposals were done using selective search which was a slow process and became bottleneck of the overall process.

### 3.2 Faster R-CNN

Slow selective search as a regional proposal is replaced by region proposal network (RPN) in fast R-CNN and then faster R-CNN \[[^10]\] is evolved. In faster R-CNN as depicted in Fig. [4](#Fig4), input image is fed to the CNN. RPN works on the last layer of the CNN. Here, the feature map of CNN maps to a lower dimension, for example, 256-d by a 3×3 sliding window, called anchor. RPN generates different types of region-based anchor boxes having fixed ratio k for each location of sliding window such as a large box, a wide box, a tall box, etc. For each anchor box, if intersection-over-union (IoU) is more than 0.7 then there is an object or if IoU is less than 0.3 then there is no object, likewise an “objectness” score is calculated and object region is proposed. After this, region proposal is fed into a RoI pooling layer and some fully connected layers. At last, the output of this is fed to a softmax classification layer and bounding box regressor.

**Fig. 4**

[![figure 4](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-981-13-9042-5_56/MediaObjects/469685_1_En_56_Fig4_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-981-13-9042-5_56/figures/4)

Faster R-CNN architecture

Faster R-CNN is able to find exact position of the object in an image and also achieves good accuracy.

### 3.3 R-FCN

Fast R-CNN share a single CNN computation to all region proposals for improving object detection speed. The motivational concept behind R-FCN \[[^11]\] is maximum sharing of the computation to increase the speed. Here, input image is feed to the CNN. A $k \times k \times \left( {C + 1} \right)$ score bank of the “position-sensitive score maps” is created by a fully convolutional layer using the output of CNN, where $k \times k$ represents the number of relative location to partition an object and $\left( {C + 1} \right)$ represents the total number of classes including background. Also to generate RoI, a fully convolutional RPN is used. Each RoI is divided into $k \times k$ subregions. For each subregion, the score bank is checked to know if that subregion corresponds to the same position of some object. For each class, this process is repeated. Once if “object match” value for $k^{2}$ subregions of each class is calculated then average the value of subregions to get single object match score for each class. Then, the RoI is classified with a softmax for the remaining $C + 1$ dimensional vector. R-FCN architecture is approximately twenty times speedier than Faster R-CNN and achieved almost same precision as that of Faster R-CNN.

### 3.4 SSD

In all R-CNN and R-FCN, two different models carry regional proposal by using RPN to generate regions of interest and region classifications either by using fully connected layers (FCL) or by using position sensitive convolutional layers for classifying those regions. SSD perform these two tasks in only one shot. As the image processes, SSD \[[^12]\] predict the bounding box and the class simultaneously.

According to SSD technique, first image is fed to a series of convolutional layers to get different sets of feature maps. Different feature map has different scales like $10 \times 10$ then $5 \times 5$ then $3 \times 3$ etc. For each feature maps, a $3 \times 3$ convolutional filter is used at each location for evaluating a small set of default bounding boxes which is equivalent to anchor boxes of faster R-CNN. The bounding box offset and the class probabilities are predicted for each box simultaneously. For each predicted boxes, IoU is calculated with the help of ground truth box. Among those predicted boxes, one box is marked as “positive” if intersection over union (IoU) exceeds 0.5 and greater than all other predicted boxes are selected and the rest boxes is discarded.

SSD is not entirely different from R-CNN and R-FCN. It draws different bounding boxes having different shapes and scales at every position of the image and classify simultaneously instead of “region proposal”. SSD does this in a single shot; hence it is the fastest of the previous models with comparable accuracy.

### 3.5 YOLO

YOLO is different from region-based algorithm. Region-based algorithm takes the help of regions to know the position of object within an image. YOLO \[[^22]\] looks parts of the image having high probabilities of containing the object. Here, a single CNN decides the bounding boxes with their class probabilities.

YOLO first take an image then split the image into $S \times S$ grid. For each and every grid, it draws M bounding boxes. A class probability and offset values are calculated for each bounding boxes. A bounding box is selected if it has a class probability greater than both the class probability of other bounding boxes and a threshold value of class probabilities; remaining all other bounding boxes is discarded. The selected bounding box corresponds to the object within an image.

YOLO is faster than other algorithm (45 frames per second). Instead of being faster, YOLO cannot recognize a group of small objects or irregularly shaped objects within an image.

### 3.6 Mask R-CNN

Faster R-CNN classifies the objects but it cannot find which pixel is a part of an object in an image. Hence, mask R-CNN \[[^18]\] is evolved which is used for instance segmentation. Instance segmentation has two parts that are object detection and semantic segmentation, also known as pixel level segmentation. In mask R-CNN as shown in Fig. [5](#Fig5), faster R-CNN is used for object detection and fully connected network (FCN) applied on each RoI is used for semantic segmentation.

**Fig. 5**

[![figure 5](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-981-13-9042-5_56/MediaObjects/469685_1_En_56_Fig5_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-981-13-9042-5_56/figures/5)

Framework of mask R-CNN

Mask R-CNN use RoI align (RoIAlign) layer (improved version of RoI pooling layer). This is because pixel level segmentation needs much more fine-grained alignment which is not necessary in bounding boxes. In RoI pooling, misalignment of location occurs due to quantization. For avoiding quantization and adjusting RoI pooling to more precise, bilinear interpolation is used and this method is called RoIAlign.

## 4 Comparative Study

The comparisons among these models are difficult. It cannot be determined easily that which model is best. There should be a trade-off between speed and accuracy for real life applications. Performance of these models or methods are highly dependent on feature extractors (base network), input image resolution, non-max suppression, IoU threshold, positive versus negative anchor ratio, number of proposals, bounding box encoding, data augmentation, training datasets, localization of loss function, training configurations, etc.

The accuracy of the object detection techniques is measured by mean average precision (mAP) which is the average of maximum precisions at different recall values. Precision measures prediction accuracy and recall measures true positives from all true positive and false negative cases. The mAP performance using PASCAL VOC 2012 dataset and AP and bounding box AP $( {\text{AP}}^{\text{bb}} )$ performance using MS COCO datasets comparisons of the works discussed above are given in Tables [1](#Tab1), [2](#Tab2) and [3](#Tab3) respectively.

**Table 1 mAP (mean Average Precision) using VOC 2012 dataset \[[^22]\]**

**Table 2 AP (Average Precision) using MS COCO dataset \[[^22]\]**

**Table 3 Average Precision (AP) using MS COCO dataset \[[^18]\]**

The accuracy comparison of the different base networks is given in Fig. [6](#Fig6). From different tables and Fig. [6](#Fig6) it can be inferred that SSD and R-FCN methods are faster but cannot beat the faster R-CNN in terms of precision or accuracy. In R-FCN and faster R-CNN, detection accuracy is affected by the choice of feature extractors. However, SSD is unaffected by such choices. SSD can outperform R-FCN and faster R-CNN in accuracy for large objects but performs worse for smaller objects. Hence, faster R-CNN balances between accuracy and speed. It matches the speed of R-FCN and SSD at ~32 mAP, for number of proposals = 50. Recently, mask R-CNN outperforms all other methods in accuracy and has an average precision of ~47.3. YOLO is the fastest single stage architecture having speed of approx. 21 ~ 155 frames per second.

**Fig. 6**

[![figure 6](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-981-13-9042-5_56/MediaObjects/469685_1_En_56_Fig6_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-981-13-9042-5_56/figures/6)

Accuracy for different base network \[[^23]\]

## 5 Conclusions

A survey of the various models or techniques evolved year after year from R-CNN to its modified version is presented here. This work also carries out detailed performance comparisons of the existing works. It can be concluded that with the help of a good base network, YOLO performs faster and manage to have 21 ~ 155 fps. In terms of accuracy, mask R-CNN have an average precision ~47.3 with the help of ResNet architecture. So, based on the application area, the deep learning network should be appropriately chosen.

## References

## Editor information

### Editors and Affiliations

## Rights and permissions

## About this paper

### Cite this paper

Bharati, P., Pramanik, A. (2020). Deep Learning Techniques—R-CNN to Mask R-CNN: A Survey. In: Das, A., Nayak, J., Naik, B., Pati, S., Pelusi, D. (eds) Computational Intelligence in Pattern Recognition. Advances in Intelligent Systems and Computing, vol 999. Springer, Singapore. https://doi.org/10.1007/978-981-13-9042-5\_56

- [.RIS](https://citation-needed.springer.com/v2/references/10.1007/978-981-13-9042-5_56?format=refman&flavour=citation "Download this article's citation as a .RIS file")
- [.ENW](https://citation-needed.springer.com/v2/references/10.1007/978-981-13-9042-5_56?format=endnote&flavour=citation "Download this article's citation as a .ENW file")
- [.BIB](https://citation-needed.springer.com/v2/references/10.1007/978-981-13-9042-5_56?format=bibtex&flavour=citation "Download this article's citation as a .BIB file")
- DOI https://doi.org/10.1007/978-981-13-9042-5\_56
- Published 18 August 2019
- Publisher NameSpringer, Singapore
- Print ISBN978-981-13-9041-8
- Online ISBN978-981-13-9042-5
- eBook Packages [Intelligent Technologies and Robotics](https://link.springer.com/search?facet-content-type=%22Book%22&package=42732&facet-start-year=2020&facet-end-year=2020) [Intelligent Technologies and Robotics (R0)](https://link.springer.com/search?facet-content-type=%22Book%22&package=43728&facet-start-year=2020&facet-end-year=2020) [Springer Nature Proceedings excluding Computer Science](https://link.springer.com/search?facet-content-type=%22Book%22&package=85345&facet-start-year=2020&facet-end-year=2020)

### Keywords

## Publish with us

[Policies and ethics](https://www.springernature.com/gp/policies/book-publishing-policies)

[^1]: Lecun, Y., Bottou, L., Bengio, Y., Haffner, P.: Gradient-based learning applied to document recognition. Proc. IEEE **86** (11), 2278–2324 (1998)

[Article](https://doi.org/10.1109%2F5.726791) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Gradient-based%20learning%20applied%20to%20document%20recognition&journal=Proc.%20IEEE&volume=86&issue=11&pages=2278-2324&publication_year=1998&author=Lecun%2CY&author=Bottou%2CL&author=Bengio%2CY&author=Haffner%2CP)

[^2]: Krizhevsky, A., Sutskever, I., Hinton, G.E.: ImageNet classification with deep convolutional neural networks. In: NIPS, pp. 1106–1114 (2012)

[Google Scholar](https://scholar.google.com/scholar?&q=Krizhevsky%2C%20A.%2C%20Sutskever%2C%20I.%2C%20Hinton%2C%20G.E.%3A%20ImageNet%20classification%20with%20deep%20convolutional%20neural%20networks.%20In%3A%20NIPS%2C%20pp.%201106%E2%80%931114%20%282012%29)

[^3]: Zeiler, M.D., Fergus, R.: Visualizing and understanding convolutional neural networks. In: ECCV (2014)

[Google Scholar](https://scholar.google.com/scholar?&q=Zeiler%2C%20M.D.%2C%20Fergus%2C%20R.%3A%20Visualizing%20and%20understanding%20convolutional%20neural%20networks.%20In%3A%20ECCV%20%282014%29)

[^4]: Szegedy, C., Liu, W., Jia, Y., Sermanet, P., Reed, S., Anguelov, D., Erhan, D., Vanhoucke, V., Rabinovich, A.: A going deeper with convolutions. In: Computer Vision and Pattern Recognition, pp. 1–9 (2015)

[Google Scholar](https://scholar.google.com/scholar?&q=Szegedy%2C%20C.%2C%20Liu%2C%20W.%2C%20Jia%2C%20Y.%2C%20Sermanet%2C%20P.%2C%20Reed%2C%20S.%2C%20Anguelov%2C%20D.%2C%20Erhan%2C%20D.%2C%20Vanhoucke%2C%20V.%2C%20Rabinovich%2C%20A.%3A%20A%20going%20deeper%20with%20convolutions.%20In%3A%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%201%E2%80%939%20%282015%29)

[^5]: Simonyan, K., Zisserman, A.: Very deep convolutional networks for large-scale image recognition. [arXiv:1409.1556](http://arxiv.org/abs/1409.1556) (2014)

[^6]: He, K., Zhang, X., Ren, S., Sun, J.: Deep residual learning for image recognition. In: Computer Vision and Pattern Recognition, pp. 770–778 (2015)

[Google Scholar](https://scholar.google.com/scholar?&q=He%2C%20K.%2C%20Zhang%2C%20X.%2C%20Ren%2C%20S.%2C%20Sun%2C%20J.%3A%20Deep%20residual%20learning%20for%20image%20recognition.%20In%3A%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%20770%E2%80%93778%20%282015%29)

[^7]: Uijlings, J., van de Sande, K., Gevers, T., Smeulders, A.: Selective search for object recognition. IJCV (2013)

[Google Scholar](https://scholar.google.com/scholar?&q=Uijlings%2C%20J.%2C%20van%20de%20Sande%2C%20K.%2C%20Gevers%2C%20T.%2C%20Smeulders%2C%20A.%3A%20Selective%20search%20for%20object%20recognition.%20IJCV%20%282013%29)

[^8]: Girshick, R.B., Donahue, J., Darrell, T., Malik, J.: Rich feature hierarchies for accurate object detection and semantic segmentation. In: Proceedings of CVPR (2014)

[Google Scholar](https://scholar.google.com/scholar?&q=Girshick%2C%20R.B.%2C%20Donahue%2C%20J.%2C%20Darrell%2C%20T.%2C%20Malik%2C%20J.%3A%20Rich%20feature%20hierarchies%20for%20accurate%20object%20detection%20and%20semantic%20segmentation.%20In%3A%20Proceedings%20of%20CVPR%20%282014%29)

[^9]: Girshick, R.: Fast R-CNN. In: Proceedings of the IEEE International Conference on Computer Vision 2015, pp. 1440–1448 (2015)

[Google Scholar](https://scholar.google.com/scholar?&q=Girshick%2C%20R.%3A%20Fast%20R-CNN.%20In%3A%20Proceedings%20of%20the%20IEEE%20International%20Conference%20on%20Computer%20Vision%202015%2C%20pp.%201440%E2%80%931448%20%282015%29)

[^10]: Ren, S., et al.: Faster R-CNN: towards real-time object detection with region proposal networks. In: Advances in Neural Information Processing Systems, pp. 91–99 (2015)

[Google Scholar](https://scholar.google.com/scholar?&q=Ren%2C%20S.%2C%20et%20al.%3A%20Faster%20R-CNN%3A%20towards%20real-time%20object%20detection%20with%20region%20proposal%20networks.%20In%3A%20Advances%20in%20Neural%20Information%20Processing%20Systems%2C%20pp.%2091%E2%80%9399%20%282015%29)

[^11]: Dai, J., Li, Y., He, K., Sun, J.: R-FCN: object detection via region-based fully convolutional networks. In: Advances in Neural Information Processing Systems 2016, pp. 379–387 (2016)

[Google Scholar](https://scholar.google.com/scholar?&q=Dai%2C%20J.%2C%20Li%2C%20Y.%2C%20He%2C%20K.%2C%20Sun%2C%20J.%3A%20R-FCN%3A%20object%20detection%20via%20region-based%20fully%20convolutional%20networks.%20In%3A%20Advances%20in%20Neural%20Information%20Processing%20Systems%202016%2C%20pp.%20379%E2%80%93387%20%282016%29)

[^12]: Fu, C.Y., Liu, W., Ranga, A., Tyagi, A., Berg, A.C.: DSSD: deconvolutional single shot detector. [arXiv:1701.06659](http://arxiv.org/abs/1701.06659), 23 Jan 2017

[^13]: Redmon, J., Divvala, S., Girshick, R., Farhadi, A.: You only look once: unified, real-time object detection. In: Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition 2016, pp. 779–788 (2016)

[Google Scholar](https://scholar.google.com/scholar?&q=Redmon%2C%20J.%2C%20Divvala%2C%20S.%2C%20Girshick%2C%20R.%2C%20Farhadi%2C%20A.%3A%20You%20only%20look%20once%3A%20unified%2C%20real-time%20object%20detection.%20In%3A%20Proceedings%20of%20the%20IEEE%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%202016%2C%20pp.%20779%E2%80%93788%20%282016%29)

[^14]: Najibi, M., Rastegari, M., Davis, L.S.: G-CNN: an iterative grid based object detector. In: Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition 2016, pp. 2369–2377 (2016)

[Google Scholar](https://scholar.google.com/scholar?&q=Najibi%2C%20M.%2C%20Rastegari%2C%20M.%2C%20Davis%2C%20L.S.%3A%20G-CNN%3A%20an%20iterative%20grid%20based%20object%20detector.%20In%3A%20Proceedings%20of%20the%20IEEE%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%202016%2C%20pp.%202369%E2%80%932377%20%282016%29)

[^15]: He, K., Zhang, X., Ren, S., Sun, J.: Spatial pyramid pooling in deep convolutional networks for visual recognition. In: European Conference on Computer Vision, 6 September 2014, pp. 346–361. Springer, Cham (2014)

[Chapter](https://link.springer.com/doi/10.1007/978-3-319-10578-9_23) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Spatial%20Pyramid%20Pooling%20in%20Deep%20Convolutional%20Networks%20for%20Visual%20Recognition&pages=346-361&publication_year=2014&author=He%2CKaiming&author=Zhang%2CXiangyu&author=Ren%2CShaoqing&author=Sun%2CJian)

[^16]: Lenc, K., Vedaldi, A.: R-CNN minus R. In: British Machine Vision Conference (BMVC) (2015)

[Google Scholar](https://scholar.google.com/scholar?&q=Lenc%2C%20K.%2C%20Vedaldi%2C%20A.%3A%20R-CNN%20minus%20R.%20In%3A%20British%20Machine%20Vision%20Conference%20%28BMVC%29%20%282015%29)

[^17]: Sun, C., Paluri, M., Collobert, R., Nevatia, R., Bourdev, L.: ProNet: learning to propose object specific boxes for cascaded neural networks. In: Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition 2016, pp. 3485–3493 (2016)

[Google Scholar](https://scholar.google.com/scholar?&q=Sun%2C%20C.%2C%20Paluri%2C%20M.%2C%20Collobert%2C%20R.%2C%20Nevatia%2C%20R.%2C%20Bourdev%2C%20L.%3A%20ProNet%3A%20learning%20to%20propose%20object%20specific%20boxes%20for%20cascaded%20neural%20networks.%20In%3A%20Proceedings%20of%20the%20IEEE%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%202016%2C%20pp.%203485%E2%80%933493%20%282016%29)

[^18]: He, K., Gkioxari, G., Dollár, P., Girshick, R.: Mask R-CNN. In: 2017 IEEE International Conference on Computer Vision (ICCV), pp. 2980–2988. IEEE (2017)

[Google Scholar](https://scholar.google.com/scholar?&q=He%2C%20K.%2C%20Gkioxari%2C%20G.%2C%20Doll%C3%A1r%2C%20P.%2C%20Girshick%2C%20R.%3A%20Mask%20R-CNN.%20In%3A%202017%20IEEE%20International%20Conference%20on%20Computer%20Vision%20%28ICCV%29%2C%20pp.%202980%E2%80%932988.%20IEEE%20%282017%29)

[^19]: Jiang, H., Learned-Miller, E.: Face detection with the faster R-CNN. In: 2017 12th IEEE International Conference on Automatic Face & Gesture Recognition (FG 2017), 30 May 2017, pp. 650–657. IEEE (2017)

[Google Scholar](https://scholar.google.com/scholar?&q=Jiang%2C%20H.%2C%20Learned-Miller%2C%20E.%3A%20Face%20detection%20with%20the%20faster%20R-CNN.%20In%3A%202017%2012th%20IEEE%20International%20Conference%20on%20Automatic%20Face%20%26%20Gesture%20Recognition%20%28FG%202017%29%2C%2030%20May%202017%2C%20pp.%20650%E2%80%93657.%20IEEE%20%282017%29)

[^20]: Stewart, R., Andriluka, M., Ng, A.Y.: End-to-end people detection in crowded scenes. In: Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition 2016, pp. 2325–2333 (2016)

[Google Scholar](https://scholar.google.com/scholar?&q=Stewart%2C%20R.%2C%20Andriluka%2C%20M.%2C%20Ng%2C%20A.Y.%3A%20End-to-end%20people%20detection%20in%20crowded%20scenes.%20In%3A%20Proceedings%20of%20the%20IEEE%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%202016%2C%20pp.%202325%E2%80%932333%20%282016%29)

[^21]: Mao, T., Zhang, W., He, H., Lin, Y., Kale, V., Stein, A., Kostic, Z.: AIC2018 report: traffic surveillance research. In: CVPR Workshop (CVPRW) on the AI City Challenge (2018)

[Google Scholar](https://scholar.google.com/scholar?&q=Mao%2C%20T.%2C%20Zhang%2C%20W.%2C%20He%2C%20H.%2C%20Lin%2C%20Y.%2C%20Kale%2C%20V.%2C%20Stein%2C%20A.%2C%20Kostic%2C%20Z.%3A%20AIC2018%20report%3A%20traffic%20surveillance%20research.%20In%3A%20CVPR%20Workshop%20%28CVPRW%29%20on%20the%20AI%20City%20Challenge%20%282018%29)

[^22]: Redmon, J., Farhadi, A.: YOLO9000: better, faster, stronger. arXiv preprint, 17 Jul 2017

[Google Scholar](https://scholar.google.com/scholar?&q=Redmon%2C%20J.%2C%20Farhadi%2C%20A.%3A%20YOLO9000%3A%20better%2C%20faster%2C%20stronger.%20arXiv%20preprint%2C%2017%20Jul%202017)

[^23]: Huang, J., Rathod, V., Sun, C., Zhu, M., Korattikara, A., Fathi, A., Fischer, I., Wojna, Z., Song, Y., Guadarrama, S., Murphy, K.: Speed/accuracy trade-offs for modern convolutional object detectors. In: IEEE CVPR 2017, 1 July 2017, vol. 4 (2017)

[Google Scholar](https://scholar.google.com/scholar?&q=Huang%2C%20J.%2C%20Rathod%2C%20V.%2C%20Sun%2C%20C.%2C%20Zhu%2C%20M.%2C%20Korattikara%2C%20A.%2C%20Fathi%2C%20A.%2C%20Fischer%2C%20I.%2C%20Wojna%2C%20Z.%2C%20Song%2C%20Y.%2C%20Guadarrama%2C%20S.%2C%20Murphy%2C%20K.%3A%20Speed%2Faccuracy%20trade-offs%20for%20modern%20convolutional%20object%20detectors.%20In%3A%20IEEE%20CVPR%202017%2C%201%20July%202017%2C%20vol.%204%20%282017%29)

[^24]: Peng, X., Schmid, C.: Multi-region two-stream R-CNN for action detection. In: European Conference on Computer Vision, 8 October 2016, pp. 744–759. Springer, Cham (2016)

[Chapter](https://link.springer.com/doi/10.1007/978-3-319-46493-0_45) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Multi-region%20Two-Stream%20R-CNN%20for%20Action%20Detection&pages=744-759&publication_year=2016&author=Peng%2CXiaojiang&author=Schmid%2CCordelia)

[^25]: Johnson, J.W.: Adapting mask-RCNN for automatic nucleus segmentation. arXiv preprint [arXiv:1805.00500](http://arxiv.org/abs/1805.00500), 1 May 2018

[^26]: Everingham, M., Van Gool, L., Williams, C.K., Winn, J., Zisserman, A.: The pascal visual object classes (voc) challenge. IJCV **88** (2), 303–338 (2010)

[Article](https://link.springer.com/doi/10.1007/s11263-009-0275-4) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=The%20pascal%20visual%20object%20classes%20%28voc%29%20challenge&journal=IJCV&volume=88&issue=2&pages=303-338&publication_year=2010&author=Everingham%2CM&author=Gool%2CL&author=Williams%2CCK&author=Winn%2CJ&author=Zisserman%2CA)

[^27]: Lin, T.-Y., Maire, M., Belongie, S., Hays, J., Perona, P., Ramanan, D., Dollár, P., Zitnick, C.L.: Microsoft COCO: common objects in context. In: ECCV (2014)

[Google Scholar](https://scholar.google.com/scholar?&q=Lin%2C%20T.-Y.%2C%20Maire%2C%20M.%2C%20Belongie%2C%20S.%2C%20Hays%2C%20J.%2C%20Perona%2C%20P.%2C%20Ramanan%2C%20D.%2C%20Doll%C3%A1r%2C%20P.%2C%20Zitnick%2C%20C.L.%3A%20Microsoft%20COCO%3A%20common%20objects%20in%20context.%20In%3A%20ECCV%20%282014%29)

[^28]: Deng, J., Dong, W., Socher, R., Li, L.-J., Li, K., Fei-Fei, L.: ImageNet: a large-scale hierarchical image database. In: IEEE Computer Vision and Pattern Recognition (CVPR) (2009)

[Google Scholar](https://scholar.google.com/scholar?&q=Deng%2C%20J.%2C%20Dong%2C%20W.%2C%20Socher%2C%20R.%2C%20Li%2C%20L.-J.%2C%20Li%2C%20K.%2C%20Fei-Fei%2C%20L.%3A%20ImageNet%3A%20a%20large-scale%20hierarchical%20image%20database.%20In%3A%20IEEE%20Computer%20Vision%20and%20Pattern%20Recognition%20%28CVPR%29%20%282009%29)

[^29]: Russakovsky, O., Deng, J., Su, H., Krause, J., Satheesh, S., Ma, S., Huang, Z., Karpathy, A., Khosla, A., Bernstein, M., Berg, A.C.: Imagenet large scale visual recognition challenge. Int. J. Comput. Vis. **115** (3), 211–252 (2015)

[Article](https://link.springer.com/doi/10.1007/s11263-015-0816-y) [MathSciNet](http://www.ams.org/mathscinet-getitem?mr=3422482) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Imagenet%20large%20scale%20visual%20recognition%20challenge&journal=Int.%20J.%20Comput.%20Vis.&volume=115&issue=3&pages=211-252&publication_year=2015&author=Russakovsky%2CO&author=Deng%2CJ&author=Su%2CH&author=Krause%2CJ&author=Satheesh%2CS&author=Ma%2CS&author=Huang%2CZ&author=Karpathy%2CA&author=Khosla%2CA&author=Bernstein%2CM&author=Berg%2CAC)