---
title: "People Tracking System Using DeepSORT"
source: "https://ieeexplore.ieee.org/abstract/document/9204956"
author:
published:
created: 2026-06-03
description: "The rapid development of image detection algorithm has led to its widespread application in security, such as facial recognition and crowd surveillance. However"
tags:
  - "clippings"
---
## Abstract:

The rapid development of image detection algorithm has led to its widespread application in security, such as facial recognition and crowd surveillance. However, real-tim...

---

There are many applications of people tracking systems. It could range from tracking movements of burglars to monitoring the movement of your children at home. Using Deep SORT framework, it can either detect the trajectory of people in real-time or even retrospectively based on an existing recording. This framework will train itself using a large dataset and be able to track the movement of individuals based on velocity, distance, and the physical appearance of that individual. Deep SORT algorithm is incredibly powerful and fast, making it the leading algorithm in object detection and tracking.

We have identified several problems with regards to people tracking in the security system. Firstly, occlusion of people is the biggest issue when it comes to tracking people. This problem arises commonly in a crowded area with different people moving in and out of the frame very rapidly (i.e. a bustling Zebra crossing). When people occlude within a frame, it may result in incorrect and inaccurate identification of an individual’s trajectory. Secondly, anointing a useful dataset can also be a hurdle when it comes to building a deep learning system. The efficiency of a system is directly contingent on the system being able to learn from a dataset that is diverse and large. In contrast to Image detection, it requires a variety of video recordings that have different movements of different individuals in various areas.

The purpose of this work is to design a system of people tracking using deep learning, Deep SORT framework to monitor the movement of people within a video recording or real-time video streaming. We want to be able to extract information from the monitoring it has done and be able to display it to the users. This work also focuses on tracking movement of individuals for crowd security and surveillance. Especially in crowded areas with congested traffic, this system should still be able to rapidly detect the trajectory of movement of individuals within the frame. From there, the system should also be able to extract specific information from the movements detected. The application of Deep SORT into crowd security can create a far more efficient system of surveillance and ultimately, a safer society for us all.

The rest of this paper is arranged as follows: Section 2 outlines some related works while section 3 elaborate details of methodology used in this work. Section 4 analyses and discusses the results obtained from the experiments conducted. Subsequently in Section 5, the paper is concluded.

Neural networks are an excellent tool for rapid detection as well as image classification of the desired objects. The use of deep learning or CNNs has become extremely extensive \[1\], and there are many applications of it, such as facial recognition, people detection and tracking systems \[2\] \[3\]. It could range from tracking movements of burglars to monitoring the movement of children at home \[4\].

Fundamentally, People-Tracking Systems capable of locating individuals within a camera frame, ranging from the identification to tracking the movement of persons \[5\]. Based on previous research, it suggested that neural networks would have improved performance if the database they use to train themselves is broad and diverse. For example, having images with more than one person in the training database would mean that the system would be more efficient. The steps for creating an accurate neural network are as follows: image acquisition, image pre-processing, image segmentation, feature extraction and classification. This project will follow the same steps, adopt certain parts of similar algorithms, and apply them to the surveillance of crowd safety \[6\].

You Only Look Once (YOLO) algorithm differ from other object detection as it is a single convolutional network predicts the bounding boxes and the class probabilities for these boxes. YOLO is a single network detection \[7\]; thus, its performance can be optimized end-to-end directly and subsequently compute the input very quickly. Deep learning has been playing a vital role in the metabolism of object detection. Faster speed, higher accuracy and greater robustness make object detection technology more practical \[8\]. Pairing with Deep SORT framework, the system can either detect the trajectory of people in real-time or even retrospectively based on an existing recording. This framework will train itself using a large dataset and be able to track the movement of individuals based on velocity, distance, and the physical appearance of that individual. Mostly, sort uses a Kalman filter to track objects without the use of ego-motion information \[9\] \[10\]. Correcting information comes from affiliating objects in two adjacent frames. Deep SORT uses a pre-trained neural net to create features for subjects so that a connection can be made based on the similarity of the features in addition to overlapping \[11\]. Deep SORT algorithm is incredibly powerful and fast, making it the leading algorithm in object detection and tracking \[2\].

This section will discuss in detail the methodology used within this project and the flow of work to complete the system. It explains the mechanism used to implement the image detection system, hardware design and flowchart of the project, as shown in Fig. 1.

**Fig. 1.**

Methodology flowchart

**Fig. 2.**

Block Diagram

Fig. 2 shows the block diagram of the system flow. The block diagram separated into three phases, which categorize the system steps into its functionality. Each phase step deals with different processes that are crucial to obtain the output. This project is a software-based system that uses YOLOv3 and Deep SORT algorithm as the main building block

### A. Phase 1

This stage deals with user input to the system. This system requires the user to input the details instruction command for the system to start the analysis. Using the command prompt in Windows OS, the user input specified the type of input video, type of YOLOv3 weight used, and output file.

**Fig. 3.**

Type of input command for different weight file

**Fig. 4.**

Example of a user input command

Using get() function in python, all those input will be stored in their relative variable and will be used throughout the tracking process. The next step is video collection which in this part, the system accepts the type of video input from user command. Video collection input can either be a prerecorded or real-time because the system is able to process both types. If user enter command for real-time video input, then the system will access and start the available webcam. Before the phase 2 begin, the system will check the necessary software for it to run. It will check for tensorflow version, CUDA version, and YOLO weight file. Only after passing those, the system can progress to the next phase.

**Fig. 5.**

YOLOv3 weight loaded into the system

### B. Phase 2

The next phase focus on processing the video input. The system will convert the input video into the image sequence firsthand, frame-by-frame for extracting color information and object detection. YOLO is been used as it only need the images to pass one time through its network. YOLO uses a unique feature of the images to predict multiples, each containing specific object. Images is divided into S’ x S’ region. If the center of the object is in one of the Region of Interest (ROI), these ROI will be responsible for the detecting the object. There will be confidence score for each detected object to classify the class name of the object. However, only class name person will be processed. Fig. 6 show the code that we use to filter only person class.

**Fig. 6.**

Code to filter person class only

### C. Phase 3

Phase 3 carries out the final process of the system which is the visual tracking and data storage process. Deep SORT is used altogether with Kalman filter algorithm. Kalman filter is used for state prediction of the target, as take the state vector of the target. It used series of observing measurement over time. The possible movement path can be predicted using this algorithm to accurately track target during occlusion. The tracking part processed the detected person, assign it with a unique id, and start to track its movement trajectory. In this stage, the system tracks the movement of each person, while maintaining their own id, respectively. After tracking part done, the system will then save the output as a video file with Audio Video Interleave (AVI) format.

### A. Dataset

This work uses three different types of datasets, varying in weight file size and object class. The difference datasets affect the systems capability to efficiently and accurately, detect objects, hence affecting its overall performance to track these objects. The dataset used is YOLOv3, YOLOv3 tiny, and YOLOv3 custom. Each dataset is different in weight size. Table 1 show the comparison between each dataset used.

**TABLE 1** Frame Rates and Accuracy Using Different Dataset

Mean Average Precision (mAP) for object detection is a popular metric in measuring the accuracy of object detector.

It computes the average precision value for recall value over 0 to 1. The results of the system effectively identifying, and tracking objects based on the 3 difference datasets (normal, tiny, and custom) can be seen in Fig. 7, Fig. 8, and Fig. 9, respectively. The dataset is acquired with permission from [https://pjreddie.com](https://pjreddie.com/) website \[12\].

**Fig. 7.**

Testing on normal YOLOv3 dataset

**Fig. 8.**

Testing on tiny YOLOv3 dataset

**Fig. 9.**

Testing on normal custom YOLOv3 dataset

The reason why the system could detect and track the most number of people in Figure 7 is due to it uses an accurate dataset. YOLOv3 normal dataset has 80 object classes and big in size. Furthermore, it has the highest accuracy among those three datasets. However, because of the large number of object classes and the size of the weight file, the system’s performance is quite slow, as it only managed to process the recording at 2.36 frames per second (FPS). YOLOv3 tiny manage to get the highest FPS among the three tested datasets, which can be seen in Figure 8. On the other hand, YOLOv3 custom manages to get a higher frame rate to compare to YOLOv3 normal at 4.4 FPS, which can be seen in Figure 9. But it is the least accurate dataset as it only has a single object class, and only being trained once. YOLOv3 and YOLOv3 tiny manage to get better tracking results because of better performance in the object detection part.

### B. Tracking

The tracking function is one that is both simple and unique. Once the system detects an individual, it will assign that individual with their specific id number. This id number now becomes the primary source of tracking that the system uses for that specific individual. This both allows the system and makes it easier for the system to track the same individual throughout different sequence frames. The assigning of the specific id numbers to the individual can be seen in Figure 10, while the ability for the system to track the same id number throughout sequence frames continuously can be seen in Figure 11, 12, and 13. The use of the Deep SORT algorithm can be seen to be in use here as well as the system can maintain the tracking of the individual from frame 10 to frame 100 due to the algorithm being able to predict its path trajectory.

**Fig. 10.**

Tracking on frame 10

**Fig. 11.**

Tracking on frame 50

**Fig. 12.**

Tracking on frame 80

**Fig. 13.**

Tracking on frame 100

### C. Occlusion

In the midst of tracking objects within different frames, there are times when a said individual is occluded by another which could potentially make the object disappear, from view entirely. This does not prove as an inherent problem to the system as it is able to detect occlusions and overcome them by applying the Kalman filter to the object. The occlusion occurrence and the ability for the system to overcome the occurrence and continuously track the individual can be seen in Fig. 14.

While the higher frame rate means the system’s processing speed will be faster, the type of dataset used will determine the accuracy of the system. For a system with higher processing speed does not necessarily mean that it is accurate as can also be seen in Table 2.

**Fig. 14.**

Tracking during occlusion in sequence

**TABLE 2** Frame Rates and Accuracy Using Different Dataset

As can be seen from the Table 2, the dataset YOLOv3 has the highest accuracy. Meanwhile, custom YOLOv3 is the least accurate. This is due to differences in the size and object class of these datasets. The sizes of these datasets directly affect the accuracy of the system. This is because, during the detection process, the type of dataset used plays a vital role in detecting the individual. The accuracy of tiny YOLOv3 and custom YOLOv3 dataset is reduced due to the detection part re-detecting the same subject and assign it with another id. Hence, providing an accurate dataset to the tracker will be beneficial for the tracking process performance.

Given the huge improvement in machine learning technology, its application should be implemented in crowd security surveillance. This paper proposes a people tracking system in real-time using YOLO and Deep SORT algorithm. This system aims was to be able to keep tracking the individual even occlusion occur. This system can be beneficial especially for maintaining security in places with high rates of individuals going in and out the place. The outcome show that the tracking could be improve by providing a reliable and accurate dataset.

For later improvement, there are few suggestions could be made. First, system should be deploy using more powerful GPU. Hence, the processing part could progress faster and significantly improved tracking frame rates. Moreover, using dataset with high accuracy could make the tracking process much better when keeping in track of same person. When we come to the industrial revolution 4.0, we make the use of artificial intelligence in various sectors of industry. This project is also crucial for spreading the use of deep learning in the security system and others.

### ACKNOWLEDGMENT

We would like to extend our acknowledgment to Universiti Teknologi Mara (UiTM) and to those who have directly or indirectly contribute to our research. This research is funded by the Trans-Disciplinary Research Grant Scheme (600IRMI/TRGS 5/3 (001/2019)-1), Ministry of Higher Education Malaysia.