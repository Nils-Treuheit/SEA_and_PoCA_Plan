---
title: "An improved PointNet++ point cloud segmentation model applied to automatic measurement method of pig body size"
source: "https://www.sciencedirect.com/science/article/pii/S0168169922008687"
author:
  - "[[•An automatic pig body size measurement algorithm based on point cloud segmentation is proposed.•We have improved the PointNet++ model]]"
  - "[[which can accurately segment the pig point clouds in different poses.•Our measurement method has good robustness and scalability in intelligent agriculture.]]"
  - "[[non-contact and automatic measurement of livestock body size is the research orientation of computer-aided livestock breeding and intelligent farming industry. However]]"
  - "[[directly locating the body size measurement key points on the dense overall point clouds of livestock often results in positioning deviation and affects the measurement accuracy. Additionally]]"
  - "[[various postures of livestock and the interference of different parts of livestock body are also prone to key points positioning deviations. For more accurate measurement of pig body size]]"
  - "[[we propose in our paper an improved PointNet++ point cloud segmentation model to subdivide the overall pig point clouds into various parts]]"
  - "[[such as the pig’s head]]"
  - "[[ears]]"
  - "[[trunk]]"
  - "[[limbs]]"
published:
created: 2026-06-03
description: "Currently, non-contact and automatic measurement of livestock body size is the research orientation of computer-aided livestock breeding and intellige…"
tags:
  - "clippings"
---
## Published by: Elsevier

### Published by

[![Elsevier](https://www.sciencedirect.com/eu-west-1/prod/dc6e12e0fa077cb242a1170a0f83362834988af9/image/elsevier-non-solus.svg)](https://www.sciencedirect.com/journal/computers-and-electronics-in-agriculture "Go to Computers and Electronics in Agriculture on ScienceDirect")

## An improved PointNet++ point cloud segmentation model applied to automatic measurement method of pig body size

,,,,,

[View **PDF**](https://www.sciencedirect.com/science/article/pii/S0168169922008687/pdfft?md5=4b8a7b67fd9670c87b3e42bbd0958062&pid=1-s2.0-S0168169922008687-main.pdf)

[10.1016/j.compag.2022.107560](https://doi.org/10.1016/j.compag.2022.107560)

## Highlights

## Keywords

PointNet++

;

Point Cloud Segmentation

;

Automatic measurement

- [Previous article in this issue](https://www.sciencedirect.com/science/article/pii/S0168169922008894)
- [Next article in this issue](https://www.sciencedirect.com/science/article/pii/S0168169922008961)

## 1\. Background

The body size parameter is an important indicator of evaluating the growth status, which becomes an essential factor in breeding stock traits selection. Traditionally, the phenotype of [livestock](https://www.sciencedirect.com/topics/agricultural-and-biological-sciences/farm-animal) is measured by hand (, ). The staff use a measuring tape and other tools to manually measure the data of body height, body length, body width, and abdominal circumference and so on. However, manual measurement methods easily cause the stress response of livestock and have other problems such as poor accuracy, difficulty in measuring complex traits, and heavy workload.

In recent years, [computer vision](https://www.sciencedirect.com/topics/engineering/computervision) technology has become a major research hotspot, promoting the development of animal husbandry and aquaculture in the trend of refinement, automation and intelligence. There are many successful cases (,,, ).

In terms of bovine body measurement parameters, () used Morpho3D scanner to capture the data of 30 [Holstein cows](https://www.sciencedirect.com/topics/biochemistry-genetics-and-molecular-biology/holstein-cattle), and they adopted Poisson surface reconstruction algorithm to build a complete triangular mesh model for the purpose of measuring their body size parameters in 3D imaging space. () established an image capture system in a dairy farm, using [linear transformation](https://www.sciencedirect.com/topics/earth-and-planetary-sciences/linear-transformation) to convert the pixels of 2D images into 3D coordinates, and measured rump height (RH), rump width (RW), body length (BL) and other dimensions scale data by means of regression analysis to determine the weight of [Holstein](https://www.sciencedirect.com/topics/agricultural-and-biological-sciences/holstein) cows. () utilized three Kinect V2 cameras to capture the point cloud data of live cows, and they used the ICP algorithm to perform non-rigid 3D reconstruction of the multi-view point clouds. On this basis, nine body measurement parameters such as shoulder height, rump height, and oblique body length were calculated. () employed a single 3D sensor to collect data on the back of [heifers](https://www.sciencedirect.com/topics/agricultural-and-biological-sciences/heifer), using ellipse fitting and curvature analysis to locate the body measurement points, so as to measure rump height and shoulder height. () used the YOLOv5 target [detection algorithm](https://www.sciencedirect.com/topics/engineering/detection-algorithm) to extract the characteristic part information of the bovine body size, converting the measurement point information in two-dimensional images to the three-dimensional coordinate system for measurement by using depth information.

As far as sheep body measurement parameters are concerned, () proposed a sheep body measurement method based on [visual image](https://www.sciencedirect.com/topics/biochemistry-genetics-and-molecular-biology/retina-image) analysis. The automatic foreground region extraction algorithm of the class extracts the measurement points, and calculates body height, rump height, body length, thorax depth, thorax width, and rump width. Given the complex scene background and the change of sheep body postures, () proposed a sheep body contour acquisition method, which was combined with color and depth image to extract contour. With the U-chord curvature used to locate the rump point, together with the depth information used to establish the sheep body space [contour line](https://www.sciencedirect.com/topics/engineering/contour-line) fitting plane, the sheep body size data were calculated. By adopting the grayscale-based background difference method combined with the principle of chromatic invariance, () detected the sheep body in a complex environment, using the grid method to extract the contour envelope of the sheep body. Meanwhile, the D-P algorithm and the Helen-Qin Jiushao formula are used to analyze the contour sequence. By obtaining the maximum curvature point of the envelope, body size calculation of the sheep rumps can be conducted.

In terms of horse body measurement parameters, () proposed a dual network cameras used a [stereo vision](https://www.sciencedirect.com/topics/engineering/stereovision) system to automatically extract the required information to verify the difference between manual measurement and stereo image measurement accuracy. () used LiDAR sensor to collect data, carrying out 3D reconstruction of the body side of a horse, and the horse body size can be measured by zooming and measuring.

Regarding the measurement of pig body size parameters, (,, ) collected pig body point cloud data from multiple perspectives with three Kinect cameras, positioned the measurement points according to the whole point cloud density distribution and [geometric features](https://www.sciencedirect.com/topics/engineering/geometric-feature) to calculate body size parameters. () integrated multiple methods such as the minimum circumscribed rectangle method, the projection method, the background difference method, and the [skeletonization algorithm](https://www.sciencedirect.com/topics/engineering/skeletonization-algorithm) combined with the Hough transform algorithm to perform ideal posture detection and body measurement. In order to solve the problem concerning automatic positioning of the measurement points in point clouds of the pig body, () used the cross-sectional features of the point clouds to detect the measurement position of heart girth, obtained the heart girth point clouds slicing, and fitted the curve to calculate body length. (, ) proposed the extraction of pig body measurement points under complex background based on envelope analysis and the contour extraction algorithm. (,, ) proposed a pose normalization method for 3D point clouds of pigs with similar morphology based on bilateral symmetry and geometric features, which can adjust the input point clouds of livestock to a target angle, facilitating the calculation of body size. In order to enhance the portability of the measurement equipment, () developed a pig body measurement system based on Xtion, which uses posture normalization and calculation of geometric features to locate measurement points, and measures the thorax width, rump width and body height. Measurements were made in a fairly accurate way, and the errors were 10.30 %, 5.87 %, and 7.01 %, respectively. () used upper and side cameras to collect pig [body image](https://www.sciencedirect.com/topics/biochemistry-genetics-and-molecular-biology/body-image) data, with extracted image feature parameters such as area, convex area, long axis length, etc. inputted in the [neural network](https://www.sciencedirect.com/topics/agricultural-and-biological-sciences/neural-network) for weight prediction.

Automatic measurement methods of 2D image requires complicated operation to separate the measurement target from the background. And the image with only two-dimensional information is incapable of measuring three-dimensional body measurement parameters such as thoracic circumference, abdominal circumference, and rump circumference. The above problems can be effectively avoided by using 3D point cloud data for measurement, however, there are still some difficulties in the existing automatic livestock body size measurement method based on 3D point cloud. First of all, in practice, a 200 lb weight pig’s point cloud data acquired by multi-view Kinect depth camera has a high data density with more than 70,000 points, and these points are disorder. The existing automatic measurement methods extracted the geometric characteristics, located the measurement key points, and calculated of body size measurement all based on the whole point cloud (). The whole point cloud would cause a huge amount of computation, on the other hand, due to the difference in posture of each part, it might locate the incorrect key positioning and the body size measurement were inaccurate. Second, most of the existing automatic measurement methods are based on the standard posture (). However, in authentic farm environment, when livestock walk freely through the data collection area, the posture of obtained point cloud would be significantly different, resulting in deviations in the location of the main measurement points and leading to large errors in the final body measurement results ().

In this paper, we propose an improved [deep learning](https://www.sciencedirect.com/topics/engineering/deep-learning) model based on PointNet++, which segments each part of the pig body point cloud in different postures, and then locates the pig body measurement key points according to the geometric characteristics. This model can significantly improve positioning accuracy, producing more reliable results in body size measurements of pigs.

## 2\. Materials and methods

In our experiment, all procedures were approved by the laboratory Animal Ethics Committee of South China Agricultural University(reference number 2020G008). We propose an improved PointNet++ 3D point cloud segmentation network to partition various parts of the pig body in different postures, then perform accurate measurement key point positioning according to the results of the part division, and finally calculate the body size parameters. The specific process is shown in.

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr1.jpg)

Download: Download high-res image (210KB)

### 2.1. Point cloud preprocessing

Our data collection work was completed in the farm affiliated to Guangdong Wens Foodstuff Co., ltd. in Heyuan City, Guangdong Province. Specifically, we set up a circular walkway in the pig farm to facilitate multiple data collection. Three KinectV2 cameras are installed in the best shooting area (including top view, left view and right view), and the distance between the camera and the target pig is 1.2 m. Iron railings are used to separate data acquisition equipment from target pigs. When the target pig walks through the optimal collection area, the device automatically detects and synchronously collects point cloud data, and sends the data to the main controller, as shown in the. The 3D point cloud data under different perspectives coordinate system are converted to the global point cloud by the [linear transformation](https://www.sciencedirect.com/topics/earth-and-planetary-sciences/linear-transformation) of rotation and translation for registration, then the background and noise are removed and obtain the complete pig point cloud data, as shown in the. The specific process is described in detail in our previous works (, ).

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr2.jpg)

Download: Download high-res image (987KB)

### 2.2. Data marking

In a method of [deep learning](https://www.sciencedirect.com/topics/engineering/deep-learning) point cloud segmentation, certain number of manually labeled samples are needed for a training set. Manually labeled data is affected by subjective factors and different pig postures, and there may be subtle differences in the positioning and segmentation boundaries of various parts of the pig body. In order to reduce these differences, the manually labeled [experimental data set](https://www.sciencedirect.com/topics/engineering/experimental-data-set) is marked by the same person. The specific process is shown in.

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr3.jpg)

Download: Download high-res image (315KB)

According to the positioning of the key points, CloudCompare software is used to mark the pig’s legs, ears, head, [tail](https://www.sciencedirect.com/topics/agricultural-and-biological-sciences/tail), and body in proper order. The pig’s legs are subdivided into left fore leg, right fore leg, left hind leg, and right hind leg. With a total of 10 parts, the left fore leg is marked “0″, the right fore leg marked ”1″, the left hind leg marked “2″, the right hind leg marked ”3″, the ear marked “4″, the head marked ”5″, the tail marked “6″, the body (front) marked ”7″, the body (middle) marked “8″, and the body (back) marked ”9″.

### 2.3. Improved PointNet++ segmentation model of pig body point clouds

PointNet () mainly solves the problems concerning disorder and rotation invariance of point cloud data, directly processing 3D point cloud data without converting the point clouds to other [data types](https://www.sciencedirect.com/topics/engineering/data-type). PointNet++ () adds a layer-by-layer abstract structure on the basis of PointNet, completes the [target acquisition](https://www.sciencedirect.com/topics/earth-and-planetary-sciences/target-acquisition) ability at different scales, and achieves better segmentation effects on the public dataset ShapeNet instead of the PointNet practice of directly abstracting the overall point clouds (). However, PointNet++, which employs the farthest point sampling (FPS) method, cannot well capture the non-rigid deformation caused by the change of pig body postures. Therefore, the Octree sampling method was adopted in this study, enabling PointNet++ to better extract the local structural features of the pig point clouds in different postures, so as to complete the automatic part segmentation of pig point clouds more accurately.

The specific architecture of the pig body point cloud segmentation network improved by PointNet++ is shown in: (1) The input is a single complete pig body point cloud data after preprocessing $\left[\right. N \times n p o i n t \times 3 \left]\right.$, among which $N$ is the batch input pig body point cloud number; the number of single pig body point clouds ($\mathit{npoint}$) is greater than 70,000; 3 is the point cloud coordinate $\left(X , Y , Z\right)$. (2) Sampling pig body point clouds and grouping are conducted based on the feature points extracted by Octree, and the number of Octree layers (Layers) is 5; $n_{1}$ is the number of sampling center points, and the number of points in the neighborhood of the center point (nsample) is 64. (3) Encode the sampled and grouped local point clouds through PointNet to obtain feature vectors $\left[\right. N \times 1024 \times 256 \left]\right.$. (4) Repeat step 2 and step 3. Here, a 4-layer Octree architecture is used. $n_{2}$ is the number of points in the neighborhood of the center point, and the feature vector $\left[\right. N \times 256 \times 512 \left]\right.$ is obtained. (5) Use PointNet to encode the feature vector. (6) Since the input feature points are sub-sampled after the feature learning of the hierarchical point set, it is necessary to use the inverse distance weighted (IDW) interpolation algorithm to transfer the features of the sampling points back to the feature point set, obtaining data $\left(N \times 4096 \times 256\right)$. Among them, the formula of IDW interpolation is as follows:(1) $f^{\left(j\right)} \left(x\right) = \frac{\sum_{i = 1}^{k} w_{i} \left(\right. x \left.\right) f_{i}^{\left(\right. j \left.\right)}}{\sum_{i = 1}^{k} w_{i} \left(\right. x \left.\right)} w h e r e w_{i} \left(x\right) = \frac{1}{\left(d \left(\right. x , x_{i} \left.\right)\right)^{p}} , j = 1 , \hdots , C$

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr4.jpg)

Download: Download high-res image (501KB)

In formula, $f$ is the eigenvalue of interpolation; $d \left(\right. x , x_{i} \left.\right)$ is the distance for $x , x_{i}$; $p$ is the index of $d \left(\right. x , x_{i} \left.\right)$; $k$ is the number of neighbor points (by default $p = 2$, $k = 3$). (7) After convolution, batch normalization, [activation function](https://www.sciencedirect.com/topics/engineering/activation-function), and back-propagation processing, the feature points are obtained in 10 categories (left fore leg, right fore leg, left hind leg, right hind leg, ears, head, tail, front part of the body, middle part of the body, back part of the body); take the maximum value as the output result, and complete the segmentation of the point clouds of a target pig body. Point cloud segmentation is a multi-classification problem for each point, so the loss function we use to train the deep network model is the cross-entropy loss function. The formula is as follows(2):(2) $\mathit{Loss} = - \frac{1}{N} \sum_{i = 0}^{N - 1} \sum_{k = 0}^{K - 1} y_{i , k} l n p_{i , k}$

In formula, $y_{i , k}$ indicates that the true label of the sample $i$ is $k$, there are $K$ tagged values and $N$ samples, $p_{i , k}$ denotes the probability that the sample $i$ is predicted to be the $k$ labeled value.

### 2.4. Point cloud segmentation- based pig body size calculation

Automated body measurements usually utilize a whole point cloud to locate key points for body size measurement. However, different body parts can interfere with each other, thus affecting the accuracy of key point location in body size measurements. For example, differences in ear point cloud traits and head distortion can make it difficult to locate the ear root point; the situation where the pig walks forward on its legs can easily lead to incorrect positioning of body height, thoracic circumference and rump circumference. To solve these problems, the present study segments the overall point clouds into different parts, and then locates and calculates the measurement key points according to the point clouds of different body parts, so as to exclude the interference caused by other parts of the point clouds. According to the segmentation results of the pig body point clouds, body length, body height(front and back), body width(front, middle and back), thoracic circumference, abdominal circumference, and rump circumference are measured. The body size parameters of each part are defined, as shown in.

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr5.jpg)

Download: Download high-res image (1MB)

#### 2.4.1. Body length measurement

Body length is measured by first determining the midpoint of ear root and the point of the tail root and then using these as endpoints to calculate the body length along the chine bone of the pig.

The location of midpoint of ear root is shown in. The segmented ear point cloud needed to be further segmented from the left and right ear, and K-means algorithm was used for subdivided. The yellow parts point cloud in were the local extremum of each ear point cloud. The two black points were centre points of the yellow parts point cloud and the ear root point $p_{e a r}$ is the midpoint(in red) of the two centre points (in black). The tail root point is positioned by selecting the point clouds at the front end of the tail point clouds (yellow part of the figure). The tail root point $p_{t a i l}$ is then the centre point of the point clouds in this part, as shown in.

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr6.jpg)

Download: Download high-res image (472KB)

The body point clouds were divided into three parts, as shown in: the front point clouds $S_{f r o n t}$, the middle point clouds $S_{m i d d l e}$, and the back point clouds $S_{b a c k}$. Firstly, the three parts of the body are denoised by radius filtering for better trunk profile extraction. Then we used the point clouds in these three parts to determine five positioning points: $p_{1}$, $p_{2}$, $p_{3}$, $p_{4}$, and $p_{5}$. $p_{1}$, $p_{3}$ and $p_{5}$ are the midpoints of the points with the largest and smallest z-coordinate values in $S_{f r o n t}$, $S_{m i d d l e}$ and $S_{b a c k}$ respectively. $p_{2}$ is the midpoint of the connection between $S_{f r o n t}$ and $S_{m i d d l e}$, and $p_{4}$ is the midpoint of the connection between $S_{m i d d l e}$ and $S_{b a c k}$.

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr7.jpg)

Download: Download high-res image (175KB)

A curve was then fitted quartic polynomials to the seven points $p_{e a r}$, $p_{t a i l}$, $p_{1}$, $p_{2}$, $p_{3}$, $p_{4}$, and $p_{5}$. The curve was sampled to obtain the point set of dorsal ridge line $S_{d o r s a l}$. $S_{d o r s a l} = \left{\right. p_{1} , p_{2} , . . . , p_{n} \left.\right} , \left(\right. p_{i} = \left(\right. x_{i} , y_{i} , z_{i} \left.\right) , i = 1 , 2 , . . . , n_{d o r s a l} \left.\right)$,$n_{d o r s a l}$ is the number of the sample points. The body length is calculated using the point in the set, and let the body length be $L_{l e n g t h}$:(3) $L_{\mathit{length}} = \sum_{i = 1}^{n_{\mathit{dorsal}} - 1} \left(\parallel p_{i} - p_{i + 1} \parallel\right)_{2}$

#### 2.4.2. Body height measurements (front part and back part)

Body height is generally measured as the vertical distance from the apex of the [scapula](https://www.sciencedirect.com/topics/agricultural-and-biological-sciences/scapula) to the ground.

In our work, the front part height and the back part height of a pig are obtained separately. The height of the front part of a pig is determined using the body point clouds and the fore leg point clouds, while the height of the back part is determined using the body point clouds and the hind leg point clouds. Take the calculation of the front part height as an example. First, $A$ and $B$ were the local extremum selected from the upper part of the point clouds for both left and right fore legs, as shown in the yellow part in. Next, calculate the midpoint of the point clouds $A$ and $B$, and finally make a plane parallel to the y-axis across the two midpoints. This plane extends along the x-axis to obtain a body point cloud slice $C , a n d u s e t h e p o i n t w i t h t h e l a r g e s t y$ value in $C$ as the body height measurement point $p_{B H}$. The vertical distance from this point to the ground point cloud is the body height of the front part of a pig.

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr8.jpg)

Download: Download high-res image (433KB)

#### 2.4.3. Body width measurement

Body widths, including front, middle and back body widths, are measured by confining the pig between two plates and calculating the distance between the plates to obtain the body width.

Take the front body width calculation as an example, the front part of the denoised body point cloud $S_{\mathit{front}}$ was clustered using the k-means algorithm. The clustered point cloud was divided into two parts as shown in, where $p_{11}$ was the midpoint of the maximum z-coordinate value point $p_{\mathit{right}}$, and the minimum z-coordinate value point $p_{\mathit{left}}$. $p_{10}$,$p_{12}$ were the midpoint of the front part and the midpoint of the back part, respectively. A [quadratic curve](https://www.sciencedirect.com/topics/engineering/quadratic-curve) was fitted using the three points.

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr9.jpg)

Download: Download high-res image (107KB)

Then make a tangent $f \left(\right. x \left.\right) = a x + b$ through $p_{11}$ to the quadratic curve $, a$,$b$ being constants and $a \neq 0$. And find the distances $l_{1}$,$l_{2}$ from $p_{\mathit{right}} \left(\right. x_{\mathit{right}} , y_{\mathit{right}} , z_{\mathit{right}} \left.\right)$, $p_{\mathit{left}} \left(\right. x_{\mathit{left}} , y_{\mathit{left}} , z_{\mathit{left}} \left.\right)$ to $f \left(\right. x \left.\right)$ respectively:(4) $\begin{bmatrix} l_{1} = \sqrt{\frac{\left(z_{\mathit{right}} - \left(a x_{\mathit{right}} + b\right)\right)^{2}}{1 + a^{2}}} \\ l_{2} = \sqrt{\frac{\left(z_{\mathit{lefft}} - \left(a x_{\mathit{left}} + b\right)\right)^{2}}{1 + a^{2}}} \end{bmatrix}$

Finally the front body width $W_{f r o n t}$ is obtained, and the middle and back body widths are found in the same way as the front body width.(5) $W_{\mathit{front}} = l_{1} + l_{2}$

#### 2.4.4 Thoracic circumference measurement

Thoracic circumference is measured from a loop around the pig body at the point where the back of the fore legs [joint](https://www.sciencedirect.com/topics/engineering/joints-structural-components) with the abdomen, as shown in. Therefore, our work uses the point cloud of two fore legs and the point cloud of the body for the measurement of the thoracic circumference, which is illustrated in.

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr10.jpg)

Download: Download high-res image (258KB)

The upper part of the left fore leg and right fore leg are selected as the point cloud $A_{\mathit{front}}$ and the point cloud $B_{\mathit{front}}$ (yellow part in ), and the points, $p_{8}$ and $p_{9}$, with the largest x-coordinate values in $A_{\mathit{front}}$ and $B_{\mathit{front}}$ are used as the thoracic circumference measurement key points.

Then a plane parallel to the y-axis is determined by $p_{8}$, $p_{9}$. This plane extends along the x-axis to obtain a body point cloud slice, and the slice forms the contour of thoracic circumference. Let the contour point cloud be $E = \left{\right. e_{1} , e_{2} , . . . , e_{k} \left.\right} , \left(\right. e_{i} = \left(\right. x e_{i} , y e_{i} , z e_{i} \left.\right) , i = 1 , 2 , . . . , n_{\mathit{thorax}} \left.\right)$, and through $E$ the thoracic circumference $B_{\mathit{thorax}}$ can be obtained.(6) $B_{\mathit{thorax}} = \left(\parallel e_{1} - e_{n_{\mathit{thorax}}} \parallel\right)_{2} + \sum_{i = 1}^{n_{\mathit{thora}} - 1} \left(\parallel e_{i} - e_{i + 1} \parallel\right)_{2}$

#### 2.4.5. Rump circumference measurement

Rump circumference is measured from the front of the hind legs and the belly joint in a circle around the pig's body, as shown in. Therefore, the key points $p_{6}$ and $p_{7}$ are the connecting points between the left, right hind legs and the body are need to be positioned, as illustrated in. Therefore, the key points $p_{6}$ and $p_{7}$ are the connecting points between the left, right hind legs and the body to be positioned, as illustrated in.

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr11.jpg)

Download: Download high-res image (231KB)

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr12.jpg)

Download: Download high-res image (166KB)

Firstly, the point cloud $A_{\mathit{back}}$ is the upper part of left hind leg, and the point cloud $B_{\mathit{back}}$ is the upper part of right hind leg (shown as yellow area in ), which were selected. The points, $p_{6}$ and $p_{7}$, with the smallest x-coordinate value in $A_{b a c k}$,$B_{b a c k}$ are located as the rump circumference measurement key point. Then the plane parallel to the y-axis is obtained by the [straight line](https://www.sciencedirect.com/topics/engineering/straight-line) passing through $p_{6}$, $p_{7}$. Also this plane extends along the x-axis to obtain a body point cloud slice and gets the contour of rump circumference. The contour point cloud can be defined as $D = \left{\right. d_{1} , d_{2} , . . . , d_{n} \left.\right} , \left(\right. d_{i} = \left(\right. x d_{i} , y d_{i} , z d_{i} \left.\right) , i = 1 , 2 , . . . , n_{\mathit{rump}} \left.\right)$, and through $D$ the rump circumference $B_{\mathit{rump}}$ can be found.(7) $B_{\mathit{rump}} = \left(\parallel d_{1} - d_{n}_{\mathit{rump}} \parallel\right)_{2} + \sum_{i = 1}^{n_{\mathit{rump}} - 1} \left(\parallel d_{i} - d_{i + 1} \parallel\right)_{2}$

#### 2.4.6. Abdominal circumference measurement

Abdominal circumference is measured around the largest part of the abdomen. The first step is to determine the lowest point $p_{13}$ of the abdominal point cloud. In our study, the minimum value of the y-axis of the denoised middle body point cloud is taken as $p_{13}$.

Then get the fitted quadratic curve of the middle body according to the measurement method of the body width. $p_{14}$ is the nearest point on the quadratic curve to $p_{13}$. The line $\varphi \left(\right. x \left.\right)$ is drawn through $p_{13}$,$p_{14}$ as shown in. From $\varphi \left(\right. x \left.\right)$ the abdominal circumference is obtained by calculating the slice point cloud $F = \left{\right. f_{1} , f_{2} , . . . , f_{n} \left.\right} , \left(\right. f_{i} = \left(\right. x f_{i} , y f_{i} , z f_{i} \left.\right) , i = 1 , 2 , . . . , n \left.\right)$, and $F$ satisfies.(8) $\varphi \left(x f_{i}\right) - \tau \leq z f_{i} \leq \varphi \left(x f_{i}\right) + \tau$

The contour point cloud of the abdominal circumference measurement is $= \left{\right. g_{1} , g_{2} , . . . , g_{m} \left.\right} , \left(\right. g_{i} = \left{\right. x g_{i} , y g_{i} , z g _ i \left.\right} , i = 1 , 2 , . . . , n_{\mathit{abdomen}} \left.\right)$, and the abdominal circumference is $B_{\mathit{abdomen}}$:(9) $B_{\mathit{abdomen}} = \left(\parallel g_{1} - g_{m_{\mathit{abdomen}}} \parallel\right)_{2} + \sum_{i = 1}^{m_{\mathit{abdomen}} - 1} \left(\parallel g_{i} - g_{i + 1} \parallel\right)_{2}$

## 3\. Experimental results and analyses

The pig body point cloud data used for the experiment in this paper was collected from June 3 to June 5, 2020 in the farm affiliated to Guangdong Wens Foodstuff Co., ltd. in Heyuan City, Guangdong Province. The point cloud data were acquired in this farm: 25 individual live pigs; 207 groups of relatively complete pig body point clouds screened for registration, denoising, and labeling. Based on expert's empirical [judgement](https://www.sciencedirect.com/topics/biochemistry-genetics-and-molecular-biology/decision-making), the different postures of pigs can be identified by detecting point clouds from top and side views. The top view point cloud detects mainly the degree of body tilt or twist; the side view point cloud determines whether the pig is standing and the pig's head looks twisting. The top view point cloud for a standard posture shows a straight head, while the side view shows a flat stance with its head straight forward and no head dropping(). There are multiple sets of point cloud data in different postures for each target pig, as shown in. We divided 207 sets of data with [segmentation labels](https://www.sciencedirect.com/topics/engineering/segmentation-label) into three different sets: 127 for training set, 40 for validation set, and 40 for testing set. Train sets of data were sent to the [neural network](https://www.sciencedirect.com/topics/agricultural-and-biological-sciences/neural-network) for deep learning, and parameter adjustment was conducted in the validation set. The model parameters with the optimal segmentation effect, which are saved in the validation set, will be used to segment all data parts and finally calculate the body size parameters according to the results of the model segmentation.

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr13.jpg)

Download: Download high-res image (777KB)

### 3.1. Evaluation indicators of segmentation results

To evaluate the performance of deep learning-based 3D semantic segmentation network models, objective performance indicators, overall accuracy (OA) and mean intersection over union (mIoU) are usually used. Among them, OA is the simplest metric to calculate the probability that the semantic annotation result of each random sample is consistent with the real data annotation type. The formula for calculating OA is as follows:(10) $\mathit{OA} = \frac{\sum_{i = 0}^{k} p_{\mathit{ii}}}{\sum_{i = 0}^{k} \sum_{j = 0}^{k} p_{\mathit{ij}}}$

where mIoU represents the overlap ratio between the ground truth region and the predicted region after segmentation, which is used to measure the segmentation accuracy. The calculation formula of mIoU is as follows:(11) $\mathit{mIoU} = \frac{1}{k + 1} \sum_{i = 0}^{k} \frac{p_{\mathit{ii}}}{\sum_{j = 0}^{k} p_{\mathit{ij}} + \sum_{j = 0}^{k} p_{\mathit{ji}} - p_{\mathit{ii}}}$

In equations,, $p_{x y}$ represents the number of real values $X$ that are predicted to be $Y$,$a n d k + 1$ is the number of classes (including empty classes). $p_{i i}$ indicates the number of correct forecasts; $p_{i j}$ and $p_{j i}$ represent the numbers of [false negative](https://www.sciencedirect.com/topics/engineering/false-negative) and false positive, respectively. Generally, mIoU is calculated based on the class; after each class of IoU is calculated, it is accumulated and the average value is obtained to get the global evaluation.

### 3.2. Pig body point cloud segmentation results

The execution system environment of the point cloud segmentation experiment is based on: Intel i5-11400 CPU, RTX2060 [GPU](https://www.sciencedirect.com/topics/engineering/graphics-processing-unit), 6 GB video memory, deep learning framework Pytorch1.4, Cuda10.0. Set the initial learning rate of the model (Learning Rate) to 0.001, the decay rate (Learning Rate Decay) to 0.5, decay once every 20 iterations, and the batch size of the input point cloud each time the model is trained.

During our experiment, the pig body point cloud data in the training set was sent to the segmentation model for 200 epochs of training, and the segmentation results were tested in the validation set for each round of training. The visualization results of the training process are shown in. It can be seen from that in the early training (epoch ≤ 30), the network did not segment the tail from the trunk point clouds, and some of the trunk point clouds were marked as fore legs; in the mid-term training (30 < epoch ≤ 100), the tail can be marked, but partial trunk point clouds were marked as pig legs; in the later training (100 < epoch ≤ 200), the network was able to accurately mark each part of the pig body, and the segmentation boundary was processed in a distinct way. It is very clear that the mIoU was stable (over 80 %), and the mIoU value was improved, and there was no obvious difference in the visualization effect.

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr14.jpg)

Download: Download high-res image (550KB)

After 200 rounds of iterative training of the network model, the best segmentation model was used to segment the data of the testing set. The segmentation results are shown in. The data of both the validation set and the testing set contain the same number of pig body point clouds in standard and non-standard postures. The results showed that the model had a desirable segmentation effect for both the validation set and the [testing set data](https://www.sciencedirect.com/topics/engineering/testing-dataset) highlighted by its generalization ability, which could better extract the three-dimensional space information of the pig body point clouds in different postures. The data of standard postures had more similar [morphological characteristics](https://www.sciencedirect.com/topics/biochemistry-genetics-and-molecular-biology/morphological-trait), so the segmentation effect of the network was very accurate. For the data of non-standard postures, the correlation between different data was relatively insignificant due to the complexity of these postures, so the data segmentation effect was inferior to that of standard postures. The comparison of visualization effects after data segmentation between network models and manually labeled data are shown in. The segmentation results show that the network model has clearly segmented various parts of the pig body point clouds in different postures, which can well meet the requirements of principal points positioning for body size measurement. However, some segmentation boundaries appear jagged and unsmooth, which is a deficiency and will affect the visualization effect. Fortunately, this imperfection will not affect the accuracy of body size measurement.

Table 1. Network segmentation results.

<table><thead><tr><th rowspan="2">Evaluation indicator</th><th colspan="2">Validation set</th><th colspan="2">Testing set</th></tr><tr><th>Standard<br>posture</th><th>Non-standard<br>posture</th><th>Standard<br>posture</th><th>Non-standard<br>posture</th></tr></thead><tbody><tr><td>mIoU</td><td>88.36 %</td><td>86.56 %</td><td>88.24 %</td><td>86.41 %</td></tr><tr><td>OA</td><td>96.82 %</td><td>95.09 %</td><td>96.41 %</td><td>94.98 %</td></tr></tbody></table>

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr15.jpg)

Download: Download high-res image (1MB)

### 3.3. Body measurement results

We select the pig body point clouds in standard postures for automatic calculation of body size. The manual measurement of pig body size is completed in a measuring cage. As is shown in, the target pigs are constrained in the measuring cage and they basically present a standard posture. The manual measurement parameters are used as the standard for evaluating the accuracy of the automatic measurement method.

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr16.jpg)

Download: Download high-res image (279KB)

Based on the point cloud segmentation results, 9 body size parameters such as body height, body width, body length, and abdominal circumference of the 25 live pigs in the experiment were calculated. The calculation results are shown in, and the measurement errors are shown in. We proposed in this study an improved measurement method that had more accuracy in positioning key points of various body sizes. The body length measurement results showed that the average [relative error](https://www.sciencedirect.com/topics/engineering/relative-error) of the manual measurement was 2.57 %, which indicated that the deviation between the automatic measurement results and the manual measurement values was about 3.0 cm. The main reason for the deviation of the measurement results is not the positioning of the starting point and the end point, but the interference of the back noise points that will cause deviation in the dorsal ridge line fitting.

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr17a.jpg)

Download: Download high-res image (496KB)

Table 2. Error comparison between our method and traditional measurement methods (the bold font indicates the optimal value).

<table><thead><tr><th rowspan="2">Trait</th><th>Manual measurement</th><th colspan="4">Traditional algorithm measurement</th><th colspan="4">Our method</th></tr><tr><th>Mean (cm)</th><th>Mean standard deviation</th><th>Mean variance</th><th>Mean absolute error (cm)</th><th>Mean relative error</th><th>Mean standard deviation</th><th>Mean variance</th><th>Mean absolute error (cm)</th><th>Mean relative error</th></tr></thead><tbody><tr><td>Body<br>length</td><td>115</td><td>3.00 %</td><td>1.96 %</td><td>4.2</td><td>3.66 %</td><td><strong>2.24 %</strong></td><td><strong>1.15 %</strong></td><td><strong>3.0</strong></td><td><strong>2.57 %</strong></td></tr><tr><td>Body height(front)</td><td>62</td><td>1.01 %</td><td>0.20 %</td><td>1.8</td><td>2.84 %</td><td><strong>0.93 %</strong></td><td><strong>0.12 %</strong></td><td><strong>1.3</strong></td><td><strong>2.18 %</strong></td></tr><tr><td>Body height(back)</td><td>67</td><td>1.06 %</td><td>0.73 %</td><td>2.0</td><td>2.97 %</td><td><strong>0.87 %</strong></td><td><strong>0.53 %</strong></td><td><strong>1.5</strong></td><td><strong>2.28 %</strong></td></tr><tr><td>Body width(front)</td><td>31</td><td>0.93 %</td><td>0.21 %</td><td>1.6</td><td>5.26 %</td><td><strong>0.82 %</strong></td><td><strong>0.15 %</strong></td><td><strong>1.4</strong></td><td><strong>4.56 %</strong></td></tr><tr><td>Body width(middle)</td><td>30</td><td>1.14 %</td><td>0.5 %</td><td>1.9</td><td>1.86 %</td><td><strong>1.13 %</strong></td><td><strong>0.23 %</strong></td><td><strong>1.6</strong></td><td><strong>5.19 %</strong></td></tr><tr><td>Body width(back)</td><td>32</td><td>1.84 %</td><td>0.15 %</td><td>1.9</td><td>5.9 %</td><td><strong>1.63 %</strong></td><td><strong>0.12 %</strong></td><td><strong>1.7</strong></td><td><strong>5.26 %</strong></td></tr><tr><td>Thoracic circumference</td><td>104</td><td>3.03 %</td><td>0.16 %</td><td>3.3</td><td>3.16 %</td><td><strong>2.23 %</strong></td><td><strong>0.15 %</strong></td><td><strong>3.0</strong></td><td><strong>2.85 %</strong></td></tr><tr><td>Rump circumference</td><td>106</td><td><strong>4.11 %</strong></td><td>0.18 %</td><td>4.77</td><td>4.48 %</td><td>4.21 %</td><td><strong>0.12 %</strong></td><td><strong>2.7</strong></td><td><strong>2.50 %</strong></td></tr><tr><td>Abdominal circumference</td><td>115</td><td>3.19 %</td><td><strong>0.14 %</strong></td><td><strong>3.1</strong></td><td><strong>2.71 %</strong></td><td><strong>3.08 %</strong></td><td>0.17 %</td><td>3.6</td><td>3.14 %</td></tr></tbody></table>

The automatic measurement results of body width and body height are more stable than those of body length, because it is easier to calculate the straight-line distance, and the method of positioning the measurement key points is more accurate in body width and body height. The segmented point cloud can locate the key points more accurately, and better measurement results have been achieved. The accuracy of the three body measurement parameters(rump circumference, abdominal circumference and thoracic circumference) has been slightly improved. The positioning of abdominal circumference and others are more accurate, but the point cloud holes caused by railing occlusion and view restriction of a shooting angle will bring about calculation errors. In this study, the point cloud repair is not considered in our calculation method.

On the whole, the mean variance and mean standard deviation of our method in the measurement of most body size parameters are smaller than those directly using the overall point cloud measurement algorithm, which indicates that the measurement results made by our method are more stable than those made by the overall point cloud measurement. After body part segmentation, the positioning of measurement key points will be more accurate and robust.

## 4\. Discussion

The visualization of body measurement key points and fitted measurement lines can be used to evaluate the advantages of our present proposed measurement method. Specifically, for the point cloud data of turning head, the segmented point clouds of pig body parts can be least-squares fitted to each small part separately, with the dorsal ridge line perfectly fitting the changes of the pig's dorsal posture; for the starting point of body length, it is coincidentally on the dorsal ridge line, positioned as the midpoint of the line between two pig ears, as shown in (a). In contrast, deviations in the dorsal ridge line will occur when directly conducting least-squares fitting on the overall point clouds in non-standard postures of pigs. In this case, the leg point clouds or part of the head point clouds will affect the fitting result as well as the positioning of the measurement points. The overall point cloud measurement method utilizes the positioning of key points based on the density and body symmetry features of pig point cloud, nevertheless, different postures will change the distribution of morphological features in point cloud data. As is shown in (b), in body length measurement, if the starting point using overall point cloud positioning is located in the [neck](https://www.sciencedirect.com/topics/agricultural-and-biological-sciences/neck), which is quite different from the actual positioning point, the measurement result is bound to be small.

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr18.jpg)

Download: Download high-res image (268KB)

In addition, partitioning the pig body point clouds can accurately differentiate one body part from another; only the point clouds of relevant parts are selected during our calculation. Therefore, using our measurement method can avoid some positioning errors due to the interference of other body parts of the pig. As is shown in (b), because the pig's leg extends to one side as it passes through the data acquisition walkway, positioning of the body width measurement points (back part) through the overall point clouds may mistakenly position the measurement points on the pig's leg. The segmented data, on the other hand, can easily locate the body width measurement points in the front, middle and back parts of the trunk, as shown in (a).

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr19.jpg)

Download: Download high-res image (307KB)

The accuracy of measurement point selection for abdominal circumference has also been greatly improved thanks to the point cloud segmentation of a pig body. If the pig’s hind leg drastically moves forward, making the hind leg point clouds too close to the abdomen point clouds, the method of overall point cloud positioning may include part of the hind leg point clouds when abdomen point cloud slice is performed, resulting in a severely skewed measurement result, as shown in (b). In contrast, the segmented data is available for fitting, only requiring the abdomen point cloud slice, without taking the leg point clouds into consideration, as shown in (a).

![](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-gr20.jpg)

Download: Download high-res image (430KB)

## 5\. Conclusion

To resolve the existing problems concerning inaccurate positioning of measurement key points for automated pig body size measurements based on the overall point cloud data, we propose a measurement method based on segmentation results of the pig body point clouds. On the basis of PointNet++, the sampling and grouping methods have been improved, so that it can adapt to the non-rigid deformation caused by the changes of pig postures, accurately completing point cloud segmentation in ten body parts of a pig: head, tail, fore legs, hind legs, and trunk etc. This upgraded method makes the large-scale automatic segmentation of pig body point clouds practical and feasible. With the assistance of body part segmentation, the automatic measurement of multiple body parts is realized by designing a powerful algorithm, which is more accurate and more robust than the method of overall point clouds positioned by density and features.

In this paper, the body measurement method proposed by us is more refined. Specifically, when measuring body length, the fitting of the dorsal ridge line uses the trunk point cloud segmentation to achieve fitting, reducing the interference created by other parts of the point clouds and by the noise points. In measuring body width, the body width calculation adopts the two-stage method. First, the center plane is fitted to the front, middle and back parts of the trunk point clouds, and then the vertical distances between the left and right measurement points of the body width and the center plane are calculated and accumulated. The experimental results of body size measurement show that our method based on point cloud segmentation is more in line with the actual measurement results compared with the overall point cloud measurement method. Accordingly, this improved PointNet++ segmentation model can better meet the requirements of automatic body size measurements of pigs.

In order to train an accurate segmentation model for each part of the pig body point clouds, in this experiment, we manually marked 127 sets of pig body point cloud data as the training set of the deep network model. However, in practical applications, the number of training samples can be reduced by using data enhancement, which can also achieve automatic measurement of body size. Meanwhile, the research findings of point cloud segmentation can not only be applied to body size measurements, but also can be used to compute the volume and [surface area](https://www.sciencedirect.com/topics/agricultural-and-biological-sciences/surface-area) of an individual part of an animal, therefore, more refined body size information such as volume ratio and [surface area](https://www.sciencedirect.com/topics/biochemistry-genetics-and-molecular-biology/surface-area) of each part of the animal can be calculated. Regarding its potential application in intelligent farming industry, our method can be readily transferred to the body size parameter measurement of similar [tetrapods](https://www.sciencedirect.com/topics/earth-and-planetary-sciences/tetrapod) such as cattle, sheep, horses and other [livestock](https://www.sciencedirect.com/topics/agricultural-and-biological-sciences/farm-animal), and we will make further research on [transfer learning](https://www.sciencedirect.com/topics/engineering/transfer-learning) in the agricultural sector in the future.

## Declaration of Competing Interest

The authors declare that they have no known competing financial interests or personal relationships that could have appeared to influence the work reported in this paper.

## Acknowledgements

The authors thank Guangdong Wens Foodstuff Co., Ltd. for providing us materials to conduct the experiment. We thank everyone who has helped us to obtain data and manually measure pigs. This project is sponsored by the National Natural Science Foundation of China \], Key Area R&D Plan of Guangdong Province\] and by National Engineering Research Center for Breeding Swine Industry and by the Key Laboratory of Guangzhou for Intelligent Agriculture\]. and Key Laboratory of Smart Agricultural Technology in Tropical South China, Ministry of Agriculture and Rural Affairs, P.R. China.

## Appendix A. Supplementary material

The following are the Supplementary data to this article:

Supplementary figure 1.

[Download: Download Word document (252KB)](https://ars.els-cdn.com/content/image/1-s2.0-S0168169922008687-mmc1.docx "Download Word document (252KB)")

Supplementary data 1.

## Data availability

## References

[View Abstract](https://www.sciencedirect.com/science/article/abs/pii/S0168169922008687)