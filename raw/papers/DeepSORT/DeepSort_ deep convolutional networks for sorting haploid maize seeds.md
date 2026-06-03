---
title: "DeepSort: deep convolutional networks for sorting haploid maize seeds"
source: "https://link.springer.com/article/10.1186/s12859-018-2267-2"
author:
  - "[[Balaji Veeramani]]"
  - "[[John W. Raymond]]"
  - "[[Pritam Chanda]]"
published: 2018-08-13
created: 2026-06-03
description: "Maize is a leading crop in the modern agricultural industry that accounts for more than 40% grain production worldwide. THe double haploid technique that u"
tags:
  - "clippings"
---
## Abstract

### Background

Maize is a leading crop in the modern agricultural industry that accounts for more than 40% grain production worldwide. THe double haploid technique that uses fewer breeding generations for generating a maize line has accelerated the pace of development of superior commercial seed varieties and has been transforming the agricultural industry. In this technique the chromosomes of the haploid seeds are doubled and taken forward in the process while the diploids marked for elimination. Traditionally, selective visual expression of a molecular marker within the embryo region of a maize seed has been used to manually discriminate diploids from haploids. Large scale production of inbred maize lines within the agricultural industry would benefit from the development of computer vision methods for this discriminatory task. However the variability in the phenotypic expression of the molecular marker system and the heterogeneity arising out of the maize genotypes and image acquisition have been an enduring challenge towards such efforts.

### Results

In this work, we propose a novel application of a deep convolutional network (DeepSort) for the sorting of haploid seeds in these realistic settings. Our proposed approach outperforms existing state-of-the-art machine learning classifiers that uses features based on color, texture and morphology. We demonstrate the network derives features that can discriminate the embryo regions using the activations of the neurons in the convolutional layers. Our experiments with different architectures show that the performance decreases with the decrease in the depth of the layers.

### Conclusion

Our proposed method DeepSort based on the convolutional network is robust to the variation in the phenotypic expression, shape of the corn seeds, and the embryo pose with respect to the camera. In the era of modern digital agriculture, deep learning and convolutional networks will continue to play an important role in advancing research and product development within the agricultural industry.

## Background

Feeding the growing population amidst shrinking farm lands across the world requires increases in innovation and efficiencies in agricultural output through high yielding crops. Maize (aka corn) is a major crop that accounted for 28.3% harvested acres within US in 2015, with 38,105 million bushels produced across the world in 2015-2016 \[[^1]\]. Maize breeding programs generate better yielding and more predictable hybrid seed varieties by crossing two distinct inbred lines. Traditional development of inbred lines, whose copies of genomes are 99% identical, takes about 6-7 generations of recurrent selfing or crossing. Double haploid (DH) based induction technology enables large commercial breeding programs in Europe, North America and China to efficiently generate homozygous lines within 2-3 generations of breeding \[[^2]\]. For example, using DH process, Dupont-Pioneer has reported that they have developed a greater number of inbred lines since 2012 than they had produced in the first 80 years of their breeding program. \[[^3]\]. In addition to the shortening of the generation time, DH process provides other benefits such as simplified logistics, efficiency and precision of selection, accelerated product development, and fulfillment of DUS (distinctness, uniformity, and stability) requirements for plant variety protection \[[^2]\].

Sorting the diploids from the haploids seeds is a critical step in a DH induction process. Double haploid based induction uses a dominant anthocyanin color marker or gene, referred to as R1-Navajo (R1- *nj*), to distinguish putative haploid seeds from diploids. This R1- *nj* marker when expressed in a particular tissue leads to its purple coloration. R1- *nj* marker is expressed in both the outermost layer of the maize endosperm (aleurone) as well as the embryo (scutellum) in diploid maize seeds, whereas in haploid seeds it is expressed only in the endosperm layer. Using these visual differences, the most common method for sorting haploid seed is by a manual inspection process \[[^4], [^5]\]. In an agricultural industry setting, hundreds of thousands of seeds are sorted to separate haploids from the diploids during every breeding cycle. Manually sorting these high volumes of haploid/diploid seeds by visual discrimination is both labor intensive and error prone, and developing automated methods to classify haploids from diploids is critical.

To our knowledge, within the agricultural industry there have been two automation efforts using the R1- *nj* molecular marker system \[[^6], [^7]\]. The primary advancement of the disclosure \[[^6]\] can be attributed to developments in the mechanical handling and image acquisition; however the image analysis methods are preliminary (uses thresholding with erosion and fill morphological operations) and its performance demonstrated in only 24 seeds. In the patent disclosure \[[^7]\], a different mechanical system along with a PC Eyebot system (Sightech Vision Systems, Inc.) was implemented for image classification that recovered 38-53% haploids and 92-98% diploids correctly. However robust approaches in recovering haploids at industrial scale are lacking.

Automated sorting of haploid seeds from the diploid seeds robustly is challenging due to the differences in the expression of the R1- *nj* marker in maize seeds from different genetic backgrounds used in DH induction \[[^8], [^9]\], morphological variations due to incomplete pollination of corn cobs, and several environmental factors \[[^10], [^11]\]. In addition, variations that naturally arise during real-time image acquisition in field settings pose a challenge to the image analysis methods. Researchers have resorted to developing alternative phenotyping methods based on oil content \[[^9]\] and NMR \[[^5]\]. However, the throughput of these methods are constrained by multiple image acquisitions \[[^9]\], and requires expensive equipment as compared with visible range RGB cameras. In this context, development of robust computer vision methods to enable automated seed sorting becomes imperative.

Recent developments in deep learning algorithms and availability of modern powerful GPUs have spurred a revolution in the areas of image classification, speech recognition, and genomics \[[^12]\]. State-of-the-art performance was demonstrated in the ImageNet challenge where the images were classified into a thousand categories \[[^13]\] using deep convolutional networks. Convolutional neural networks (CNN) were initially developed to efficiently represent images using neural nets with far fewer parameters (using local connectivity and weight sharing) and train them using backpropagation algorithm \[[^14]\]. CNN has been widely used for several applications such as predicting sequence specificities for DNA and RNA binding proteins \[[^15]\], chromatin effects of sequence alternations \[[^16]\], self driving vehicles, automated phenotyping of developing *C. elegans* embryos and connectomics \[[^12]\].

Applications of convolutional networks are starting to emerge in agriculture/plant sciences recently. Convolutional networks have been used to recognize paddy field pests localized using saliency maps \[[^17]\], identify 44 different plant species using leaf images collected at the Royal Botanic Gardens, Kew, England \[[^18]\] and classification of forest and agricultural regions in Indian Pines hyperspectral image dataset \[[^19]\].

In this work, we propose an application of a convolutional network (DeepSort) to discriminate maize haploids from diploids using several thousand corn seed images based on our earlier preliminary work \[[^20]\]. We demonstrate that performance of the convolutional network closely matches the visual classification of the seeds by the human experts. We also show this performance remains robust under diverse lighting conditions, seed shapes, embryo orientation relative to the camera field of view, and heterogeneous genetic backgrounds; something that has remained challenging in practice. Using visualizations, we show that the convolutional network derives information that are discriminative of haploids and extracts features from the embryo regions. Our experiments using multiple network architectures indicate that the presence of more layers (i.e. deeper network) contributes to improved classification accuracy.

## Methods

### Diploid and haploid images

Corn seeds expressing the R1- *nj* marker are shown in the Fig. [1](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig1). Haploid seed embryos that receive only the maternal genetic material do not show the purple coloration from dominant R1- *nj* marker expression (see Fig. [1](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig1) [b](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig1) in the region marked embryo), but diploid seed embryos that also receive the genetic material from the inducer carrying the R1- *nj* marker exhibits purple coloration in the embryo (shown next to the arrow in Fig. [1](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig1) [a](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig1)). R1- *nj* marker expressed in the endosperm of both diploid and haploid seeds leads to a dark coloration as seen at the bottom of the seed images shown in Figs. [1](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig1) [a](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig1) & [1](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig1) [b](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig1) (below the embryo). We acquired images of 4021 seeds for training, and 710 seeds for testing the performance of the automatic seed classification system. A substantial heterogeneity in the level of expression of the R1-nj marker, seed morphology, color and embryo region texture is observed in our dataset. In addition, image acquisition introduces further variations such as lighting inconsistencies and embryo positioning relative to the camera field of view. Sample images from our dataset that demonstrate this heterogeneity in both diploids and haploids are shown in Figs. [1](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig1) [c](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig1) & [1](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig1) [d](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig1) respectively.

**Fig. 1**

![Fig. 1](https://media.springernature.com/lw685/springer-static/image/art%3A10.1186%2Fs12859-018-2267-2/MediaObjects/12859_2018_2267_Fig1_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1186/s12859-018-2267-2/figures/1)

Corn seeds expressing the R1- *nj* marker in (**a**) Diploid and (**b**) Haploid seeds. The marker is only expressed in the diploid embryo as a vertical dark purple patch indicated by the arrow in (**a**). Variability in visual indications of marker expression, seed morphology, color and texture, embryo positioning with respect to camera, and lighting conditions across multiple (**c**) diploid and (**d**) haploid seeds

Our dataset consists of 4731 RGB images of corn seeds (3779 diploids, 952 haploids) obtained from multiple proprietary inbred lines. Seeds were manually classified into haploid and diploid categories using high resolution images by a human expert. The images were acquired under realistic settings across several days and different genetic populations using uEye high-speed cameras. The image acquisition apparatus returns two images; approximately half the images do not depict the embryo side and were discarded. The raw 640 × 480 pixel images were pre-processed using basic techniques (cropping, centering and resizing). Each seed was repositioned so that it’s pixel-based center of mass was located in the middle of 64 × 64 cropped image and having its tip oriented upward. These 64 × 64 RGB images were used for training the convolutional network and testing its performance.

### Convolutional network

We adopt an exemplar CNN architecture for the classification of haploid and diploid seeds shown in Fig. [2](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig2) (termed DeepSort). It comprises of the following layers: two convolutional layers each followed by max-pooling and local-response-normalization layers, two densely connected layers and an output layer. We first describe each of these components briefly.

**Fig. 2**

![Fig. 2](https://media.springernature.com/lw685/springer-static/image/art%3A10.1186%2Fs12859-018-2267-2/MediaObjects/12859_2018_2267_Fig2_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1186/s12859-018-2267-2/figures/2)

Schematic architecture of DeepSort Convolution network “Arch-1” used for classifying maize seeds. Input maize images are convolved with 16 filter kernels in the first convolutional layer followed by pooling and normalization layers. Outputs of these operations are again convolved with 16 kernels in the second convolutional followed by pooling, normalization and two fully connected layers

We start with description of the convolutional layer. Formally, let the input to a convolutional layer be represented by the tensor $\mathbf {X} \in \mathbb {R}^{N \times M \times C}$. Let a convolutional layer be comprised of *D* kernels. Each kernel (or weight) is a tensor, $\mathbf {W}^{(l)} \in \mathbb {R}^{d \times d \times C}, \{l=1\dots D\}$ that applies a 2-dimensional convolution operation on *d* × *d* patches in each of the *X* <sub>·· <i>k</i></sub> plane (of dimension *N* × *M*) of the input tensor **X**, and with stride *s*. For example, if **X** represent the input image tensor, $X_{\cdot \cdot k} \in \mathbb {R}^{N \times M}, \{k=1\dots C\}$ represents a N × M pixel image across a color channel *k*, with *C* =3 such color channels (RGB). The convolution is followed by a non-linear activation function *f* (*z*)= *m* *a* *x* (0,*z*) (Rectifier Linear Unit or ReLU). This results in neuronal activations of the form $f\left ({\sum \nolimits }_{k=1}^{C} \left (W^{(l)} \circ X_{i:i+d-1,j:j+d-1,k}\right) + b\right) \in \mathbb {R}$. Here “ ∘” denotes the Hadamard product, $b \in \mathbb {R}$ is a bias term; *X* <sub><i>i</i>:<i>i</i> + <i>d</i> −1,<i>j</i>:<i>j</i> + <i>d</i> −1,<i>k</i></sub> represents the *d* × *d* subregion on 2-dimensional matrix *X* <sub>·· <i>k</i></sub>; and $W^{(l)} = \mathbf {W}^{(l)}_{\cdot \cdot k}$, a *d* × *d* slice of the tensor. Convolutions over the entire input tensor **X** using stride size *s* and padding *p* produces activation matrix $A_{l} \in \mathbb {R}^{\left (\frac {N-d+2\cdot p}{s}+1\right) \times \left (\frac {M-d+2\cdot p}{s}+1\right)}$, which represents the activations of all the neurons in a slice *l*,{ *l* =1… *D* } that share the weight tensor **W** <sup>(<i>l</i>)</sup>. Each neuron in a convolutional layer is connected only to a local region (defined by the convolution window) of the input spatial volume, but to the full depth *C* of the input tensor. Repeating this pattern for *l* =1… *D*, all the neurons located on the similar region of a plane but along the depth *D* look at the same region of the input tensor through different kernels **W** <sup>(<i>l</i>)</sup>. A convolutional layer is followed by max-pooling and local-response-normalization layers. The max-pooling layer downsamples the input tensor by partitioning it into a set of non-overlapping sub-regions and outputs the maximum value of each such sub-region. The normalization layer mimics lateral inhibition in real neurons and performs damping of neuronal responses that are uniformly large in a local neighborhood, while boosting neuron responses that are moderately strong in a local neighborhood of weaker responses \[[^13]\].

In our primary design (called “Arch-1”, see Fig. [2](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig2)), we have *D* =16 kernels in the first convolutional layer looking at RGB images (i.e tensor $\mathbf {X} \in \mathbb {R}^{N \times M \times 3}$). This is followed by max pooling and normalization layers. The first set of layers is followed by the second convolution, normalization and max pooling layers, in that order. The second convolutional layer also has *D* =16 kernels and processes as input, a tensor that is the output from the preceding normalization layer. For both our convolutional layers, *d* =5, *s* =1 and *p* =2, other kernel receptive field patch sizes (*d* =3,7) provided similar results. These layers are followed by 2 fully connected layers (with 192 and 96 neurons respectively) that are connected to all the neurons from the preceding layer, which is followed by an output layer having 2 neurons that represent the haploid and diploid classes. Our initial design is motivated by the network used for classifying cifar10 data of natural images ([https://code.google.com/p/cuda-convnet/](https://code.google.com/p/cuda-convnet/)). The choice of the number of kernels for classifying haploids from diploids were four times smaller than the cifar10 network that classifies images into 10 categories (roughly 5 times more classes than our problem).

We further experimented with different architectures where the number of kernels in the convolutional layers and neurons in fully connected layers were reduced by half for every subsequent architecture. We considered two such architectures, followed by an architecture that reduced the number the layers. Briefly the architectures are: (1) “Arch-2”: 8 kernels in each of the two convolutional layers, 96 and 48 neurons in first and second fully connected layers respectively; (2) “Arch-3”: 4 kernels in each of the convolutional layers, 48 and 24 neurons in first and second fully connected layers respectively; and (3) “Arch-4”: a single convolutional layer with 4 kernels and a fully connected layer with 24 neurons. In all architectures, each convolutional layer is followed by max-pooling and local-response-normalization layers similar to “Arch-1”.

Convolutional networks were implemented with the library tensorflow version 0.8.0 ([https://www.tensorflow.org/](https://www.tensorflow.org/)) and K80 Nvidia GPUs. During training, parameter values (such initial learning rate (0.1), learning rate decay factor (0.1), number of epochs per decay(350), moving average decay (0.999)), images transformations (approximate whitening), and data augmentation distortions (random flip, image brightness and contrast distortions) were used. Furthermore, we used the strategies that have been reported to reduce model overfitting during training and improve performances of convolutional networks \[[^13]\], such as data augmentation by introducing transformations to the training data, and moving average weight decay. We used a batch size of 64 samples, and training was carried out for 400,000 iterations ensuring the algorithm was not trapped in local optima with unfavorable classification accuracy.

## Results and discussion

### Classification performance of deep convolutional network

We use 4021 randomly chosen images of corn seeds (809 haploids and 3212 diploids) for training and 710 images (143 haploids and 567 diploids) for testing. The training and test datasets contained 20% haploids. The training dataset was further split in 5-folds to assess the performance of different network architectures (discussed in “ [Architectures of deep convolutional networks](https://link.springer.com/article/10.1186/s12859-018-2267-2#Sec8) ” section).

We also compared the performance of the convolutional network with an image analysis pipeline that uses feature extraction followed by classification, an approach similar to one used by \[[^21]\] to classify pepper seed images. We extracted Haralick texture features \[[^22]\], local binary patterns, zernike moments, and shape features using MATLAB ‘regionprops’ (total 84 features).

Using these features we experimented with several classifiers: Support Vector Machine (SVM), Random Forest (RF), and Logistic Regression (LR) \[[^23]\]. In our experiments, Haralick texture features were found to better discriminate haploids from diploids as compared to morphology, color and shape features. Therefore we also report the performances of the classifiers using only 13 Haralick features in addition to using all the features (see Table [1](https://link.springer.com/article/10.1186/s12859-018-2267-2#Tab1)). Image handling and feature extraction were performed using the Mahotas 1.4 library package \[[^24]\] and MATLAB, and the classifiers was implemented using scikit-learn library \[[^25]\]. For each classifier, the respective parameters in scikit-learn terminology (*gamma* and *C* for SVM with a radial basis kernel; *n* \_ *estimators, min* \_ *samples* \_ *split, min* \_ *samples* \_ *leaf* for RF; regularization parameter *C* for LR), were chosen using a grid search with 5-fold cross-validation (CV).

**Table 1 Comparison of classification accuracies of DeepSort and other classifiers. Other classifiers were tested with all features described in text (values within brackets), and using only Haralick texture features (values outside brackets). CV indicates 5-fold cross-validation**

The performance results are summarized in Table [1](https://link.springer.com/article/10.1186/s12859-018-2267-2#Tab1). We observe DeepSort is able to classify the training images perfectly which shows enough flexibility in its architecture to learn the heterogeneity of the samples in training dataset. In our experiments using the test dataset, we observe DeepSort outperforms all other machine learning classifiers, and attained a classification accuracy of 0.968. Among the other methods compared, SVM achieved the highest classification accuracy (0.876) using Haralick features; 9.2 *%* lesser than DeepSort. We discuss only this comparitive method in the next sections. We observe the test and cross-validation accuracies of compared methods to be lacking even though their training accuracies were high (RF Haralick, all features; SVM all features), demonstrating their inability to generalize. Further, we also observed the SVM classifier to have a large number of support vectors (1390 with full training dataset and Haralick features).

We examined the confusion matrix of DeepSort and the best performing machine learning classifier (SVM with Haralick features) to understand the performance of these methods on each category individually using the test data set (See Table [2](https://link.springer.com/article/10.1186/s12859-018-2267-2#Tab2)). Diagonal values of the confusion matrix represent the correct classification of haploids and diploids into their appropriate categories, and off-diagonals represent mis-classifications. DeepSort misclassified 11 diploid images as haploids, and 12 haploid images as diploids, while SVM misclassified 22 diploid images as haploids and 66 haploid images as diploids. We observe SVM is biased towards classifying haploid images as diploids, possibly representing the distribution of the diploid images in the dataset. Our attempts to compensate for this class bias in the SVM (using ‘balanced’ setting for class\_weight parameter in Scikit-learn) decreased the training error but led to a larger cross-validation error (results not shown).

**Table 2 Confusion Matrix for DeepSort and SVM**

### Visualizing the convolutional network neurons

We conducted visualizations of the kernels and neuronal activations of the two convolutional layers in order to understand the features learnt by the network in achieving superior classification performance. Understanding the features learned by deep neural networks, and how these features are effectively combined towards superior classification is complex (as these networks often have several hundreds of thousands of parameters), and is an active field of ongoing research \[[^26], [^27]\]. Visualization of the neuronal activations in a convolution layer, weight tensors and bias parameters; image regions/features that lead to maximal neuronal activations; and receptive fields of individual neurons are some of the current techniques employed towards this goal \[[^27]\]. In our work, we chose to focus on visualizations of the neuronal activations of the first two convolutional layers, as the structure of the input images are somewhat preserved in these two layers. We show that these visualizations help to gain key insights into the functions performed by the convolutional layers in extracting discriminatory seed features.

Figure [3](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3) shows the activations of the neurons in the first two convolutional layers to an input of 30 randomly chosen haploid and diploid seeds from our test dataset (i.e. 15 of each category, and denoted by columns numbered 1-15). The activations of first convolutional layer neurons to diploid and haploid seeds are shown in Figs. [3](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3) [a](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3) and [3](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3) [b](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3), respectively. An image(*i*,*j*) in the grid of each subfigure a-d of Fig. [3](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3) denotes activations of all neurons that share a kernel *i* on an input from the seed image in column *j* shown at the top. Visually comparing the images in haploid and diploid categories across a row of Fig. [3](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3) (a & b or c & d) allows one to identify kernels that are discriminatory and the operations performed by them. The 16 kernels in the first convolutional layer each looking at 5×5×3 segment of an input RGB image are shown in Fig. [3](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3) [e](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3). Kernels in the second layer, looking at an input tensor 5×5×16 with a depth larger than three, are not shown.

**Fig. 3**

![Fig. 3](https://media.springernature.com/lw685/springer-static/image/art%3A10.1186%2Fs12859-018-2267-2/MediaObjects/12859_2018_2267_Fig3_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1186/s12859-018-2267-2/figures/3)

Figure (**a**, **b**) shows the activations of all neurons in the convolutional layer 1 (each row corresponds to the activations that share a kernel) across images of 15 random diploid (**a**) and haploid (**b**) seeds (each column for a seed shown at the top row) from the test data set. Similar to figures (**a**, **b**), figures (**c**, **d**) shows the activations of neurons in the convolutional layer 2 across the same set of seeds. Kernels in the convolutional layers 1 and 2 perform various feature extractions and their complex compositions. For example, kernels 3 of first layer segments the seed from the background, and kernel 5 of the second layer provides discriminatory features (for other examples see text). Figure (**e**) shows visualizations of 16 kernels from the convolutional layer 1

We observe several interesting patterns. The neurons in the first layer perform image pre-processing operations, while those in the second layer synthesize higher level features. Looking at the activations of neurons in the first convolution layer in Figs. [3](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3) [a](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3) and [3](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3) [b](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3), we observe kernels 2, 3, 8, 11, 12, 14, 15 to broadly segment the seed from the background, however with differences near the seed boundaries and output intensity. We also looked at the corresponding kernel tensors of these neurons, and observe them to be different in terms of their magnitudes, patterns within a given channel, and across the different RGB channels (see Fig. [3](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3) [e](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3)), possibly contributing to the robustness of performance across the heterogeneous seeds. Although some kernels are seen as dark patches in Fig. [3](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3) [e](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3), they are not uniformly zero as seen from the raw values. Several other kernels in the first convolution layer (kernels 4-7,9-10) extract features related to seed texture, while kernel 1 extracts texture in endosperm region but not in the embryo. Kernel 13 serves to perform an intensity based segmentation, while kernel 10 accentuates embryo regions is diploid seeds (and in some haploids).

The second convolutional layer kernels extract complex features that do not reflect the exact shape of the seeds or embryo features. Rather these features encode more abstract input concepts combining features extracted from the max-pooled and normalized output of the first convolutional layer. We notice that fewer neurons in the second convolutional layer are active as observed by fewer white pixels in Figs. [3](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3) [c](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3) and [3](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3) [d](https://link.springer.com/article/10.1186/s12859-018-2267-2#Fig3). Strikingly, activations from the kernels 5 and 9 are already discriminatory of most diploid and haploid seeds, roughly highlighting the purple dark regions in diploid seeds. Kernels 12 and to some extent 15 marks the brighter regions of the embryo in the diploid seeds with two vertical patches. Information from these activations are further nonlinearly combined by the fully connected layers to achieve robust performance.

### Architectures of deep convolutional networks

We explored the effect of the architecture on the performance of the convolutional network in classifying haploid seeds from diploids by changing the number of kernels in the convolutional layer, neurons in the fully connected layer, and depth (i.e, number of layers) of the network (see Table [3](https://link.springer.com/article/10.1186/s12859-018-2267-2#Tab3)). We considered two different architectures (“Arch-2” and “Arch-3”) with the same number of layers as “Arch-1”, but with reduced number of kernels and neurons in the fully connected layer. “Arch-2” had eight kernels in the first and second convolutional layers, and “Arch-3” had four kernels in the convolutional layers (see “ [Convolutional network](https://link.springer.com/article/10.1186/s12859-018-2267-2#Sec4) ” section for the number of kernels/neurons, and Table [3](https://link.springer.com/article/10.1186/s12859-018-2267-2#Tab3) for the number of parameters in each layer). Going from “Arch-1” to “Arch-3”, the number of parameters decreases roughly four fold for each step. We also considered a shallower architecture (“Arch-4”) with a single convolutional and fully connected layer.

**Table 3 Effect of CNN architecture on classification accuracy (cols. 2,3) and number of parameters per layer (cols. 4-9) in each architecture**

We observe that the performance of the “Arch-2” (0.968 on test set) is similar to that of “Arch-1”, whereas in “Arch-3” the accuracy is reduced by 0.027 (or roughly 15 more test images were misclassified). We also observe that the training accuracy goes down by 0.008 (roughly 32 more training images were wrongly classified). We further reduced a convolutional and a fully connected layer and obtained “Arch-4” to assess the impact of having a shallower network. The performance on test data dropped to 0.935 in “Arch-4” (a slight reduction as compared to “Arch-3”, but lesser than “Arch-2” by 0.033), even though the total number of parameters is more than “Arch-3” (since “Conv1” of “Arch-4” has more neurons than “Conv2” of “Arch-3”). Although “Arch-1” displayed similar classification accuracy as “Arch-2” (with four times more parameters than “Arch-2”), future experiments have to be performed to study if more trainable parameters enable “Arch-1” to be more robust to unseen variations and more heterogeneity.

## Conclusion

Our experiments provide evidence to the usefulness of DeepSort in discriminating haploids from diploid seeds in the double haploid induction process, and to its robustness amidst variations arising from biological factors and image acquisition. We establish such robustness using thousands of seed images obtained in an industrial scenario from different genetic backgrounds. Our visualizations indicate that embryo’s features are being extracted by the network, which may be used further to classify the seeds, as are carried out manually by agricultural field workers. We further observe that deeper architectures provide better classification accuracies as compared to shallower architectures. In the future, we intend to develop more general deep networks, which can classify haploids from diploids using two un-identified images with one showing the embryo and other not showing it, and with minimal pre-processing.

Convolutional networks and other deep learning methods, though popular in several commercial applications (e-commerce, social networking, retail, automotive, etc.), have only began to find applications within agriculture recently. The approach we use to classify corn seed images into haploid or diploid categories, could be extended to other agricultural applications. A few of these include sorting seed images of crops such as soybean, canola, etc. into various categories (e.g. high oil vs low oil, high-moisture vs low-moisture, deformed vs non-deformed, etc.), detecting pest infestations using remote sensing images, estimating plant vigor using field images from drones, assigning diseased status to plants from leaf images, insect mortality rate estimation from bioassay images, etc. The combination of different deep network architectures with a variety of sensors (such as multispectral, infra-red, MRI, etc.) offers enormous possibilities, and will contribute to next generation agricultural phenotyping. In addition, modern high-throughput technologies has enabled agricultural industries to collect large scale molecular datasets. Deep networks can be applied to several such agricultural biotech predictive applications using biological sequences (DNA, RNA, proteins etc.), genetic (SNPs other genetic variations etc.), chemical, environmental and phenotypic data.

## References

## Acknowledgements

Authors acknowledge help/feedback by W. Edsall, S. Cryer, B. John, K. Koehler, G. Tragesser, G. Temnykh, E. Frederickson, and P. Setlur. The abridged abstract of this work was previously published in the Proceedings of the 13th International Symposium on Bioinformatics Research and Applications (ISBRA 2017), Lecture Notes in Computer Science: Bioinformatics Research and Applications \[[^20]\].

### Funding

This study and publication charges was supported by Dow AgroSciences LLC.

### Availability of data and materials

The data that support the findings of this study are available from Dow AgroSciences LLC but restrictions apply to the availability of these data. Upon reasonable request data may be provided by Dow AgroSciences LLC under a licensing agreement.

### About this supplement

This article has been published as part of BMC Bioinformatics Volume 19 Supplement 9, 2018: Selected articles from the 13th International Symposium on Bioinformatics Research and Applications (ISBRA 2017): bioinformatics. The full contents of the supplement are available online at [https://bmcbioinformatics.biomedcentral.com/articles/supplements/volume-19-supplement-9](https://bmcbioinformatics.biomedcentral.com/articles/supplements/volume-19-supplement-9).

## Ethics declarations

### Ethics approval and consent to participate

Not applicable.

### Consent for publication

Not applicable.

### Competing interests

The authors declare that they have no competing interests.

### Publisher’s Note

Springer Nature remains neutral with regard to jurisdictional claims in published maps and institutional affiliations.

## Rights and permissions

**Open Access** This article is distributed under the terms of the Creative Commons Attribution 4.0 International License ([http://creativecommons.org/licenses/by/4.0/](http://creativecommons.org/licenses/by/4.0/)), which permits unrestricted use, distribution, and reproduction in any medium, provided you give appropriate credit to the original author(s) and the source, provide a link to the Creative Commons license, and indicate if changes were made. The Creative Commons Public Domain Dedication waiver([http://creativecommons.org/publicdomain/zero/1.0/](http://creativecommons.org/publicdomain/zero/1.0/)) applies to the data made available in this article, unless otherwise stated.

[^1]: Association NCG. [http://www.worldofcorn.com/](http://www.worldofcorn.com/). Accessed 5 Oct 2016.

[^2]: Prasanna B, Chaikam V, Mahuku G. Doubled Haploid Technology in Maize Breeding: Theory and Practice. Mexico: CIMMYT; 2012.

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Doubled%20Haploid%20Technology%20in%20Maize%20Breeding%3A%20Theory%20and%20Practice&publication_year=2012&author=Prasanna%2CB&author=Chaikam%2CV&author=Mahuku%2CG)

[^3]: Dupont-Pioneer. Our Unique Approach: World-Class Science. [https://www.pioneer.com/home/site/about/research/our-unique-approach/](https://www.pioneer.com/home/site/about/research/our-unique-approach/). Accessed 15 Sept 2016.

[^4]: Janick J. Plant Breeding Reviews. Plant Breeding Reviews, vol. 39. New Jersey: Wiley; 2015.

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Plant%20Breeding%20Reviews.%20Plant%20Breeding%20Reviews%2C%20vol.%2039&publication_year=2015&author=Janick%2CJ)

[^5]: Wang H, Liu J, Xu X, Huang Q, Chen S, Yang P, Chen S, Song Y. Fully-automated high-throughput nmr system for screening of haploid kernels of maize (corn) by measurement of oil content. PLoS ONE. 2016; 11(7):0159444.

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Fully-automated%20high-throughput%20nmr%20system%20for%20screening%20of%20haploid%20kernels%20of%20maize%20%28corn%29%20by%20measurement%20of%20oil%20content&journal=PLoS%20ONE&volume=11&issue=7&publication_year=2016&author=Wang%2CH&author=Liu%2CJ&author=Xu%2CX&author=Huang%2CQ&author=Chen%2CS&author=Yang%2CP&author=Chen%2CS&author=Song%2CY)

[^6]: Modiano SH, Deppermann KL, Crain J, Eathington SR, Graham M. Seed sorter. Google Patents. US Patent 8,189,901. 2012.

[^7]: Koehler KL, Tragesser G, Swanson M. Apparatus and method for sorting plant material. Google Patents. US Patent 9,156,064. 2015.

[^8]: Chaikam V, Nair SK, Babu R, Martinez L, Tejomurtula J, Boddupalli PM. Analysis of effectiveness of r1-nj anthocyanin marker for in vivo haploid identification in maize and molecular markers for predicting the inhibition of r1-nj expression. Theor Appl Genet. 2015; 128(1):159–71.

[Article](https://link.springer.com/doi/10.1007/s00122-014-2419-3) [CAS](https://link.springer.com/articles/cas-redirect/1:CAS:528:DC%2BC2cXhvFWgsrvP) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Analysis%20of%20effectiveness%20of%20r1-nj%20anthocyanin%20marker%20for%20in%20vivo%20haploid%20identification%20in%20maize%20and%20molecular%20markers%20for%20predicting%20the%20inhibition%20of%20r1-nj%20expression&journal=Theor%20Appl%20Genet&doi=10.1007%2Fs00122-014-2419-3&volume=128&issue=1&pages=159-71&publication_year=2015&author=Chaikam%2CV&author=Nair%2CSK&author=Babu%2CR&author=Martinez%2CL&author=Tejomurtula%2CJ&author=Boddupalli%2CPM)

[^9]: Melchinger AE, Schipprack W, Würschum T, Chen S, Technow F. Rapid and accurate identification of in vivo-induced haploid seeds based on oil content in maize. Sci Rep. 2013; 3:1–5.

[Article](https://doi.org/10.1038%2Fsrep02129) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Rapid%20and%20accurate%20identification%20of%20in%20vivo-induced%20haploid%20seeds%20based%20on%20oil%20content%20in%20maize&journal=Sci%20Rep&doi=10.1038%2Fsrep02129&volume=3&pages=1-5&publication_year=2013&author=Melchinger%2CAE&author=Schipprack%2CW&author=W%C3%BCrschum%2CT&author=Chen%2CS&author=Technow%2CF)

[^10]: Prigge V, Melchinger AE. Production of haploids and doubled haploids in maize. Plant Cell Cult Protocol. 2012; 877:161–72.

[Article](https://link.springer.com/doi/10.1007/978-1-61779-818-4_13) [CAS](https://link.springer.com/articles/cas-redirect/1:CAS:528:DC%2BC38XhslSnt7zP) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Production%20of%20haploids%20and%20doubled%20haploids%20in%20maize&journal=Plant%20Cell%20Cult%20Protocol&doi=10.1007%2F978-1-61779-818-4_13&volume=877&pages=161-72&publication_year=2012&author=Prigge%2CV&author=Melchinger%2CAE)

[^11]: Kebede AZ, Dhillon BS, Schipprack W, Araus JL, Bänziger M, Semagn K, Alvarado G, Melchinger AE. Effect of source germplasm and season on the in vivo haploid induction rate in tropical maize. Euphytica. 2011; 180(2):219–26.

[Article](https://link.springer.com/doi/10.1007/s10681-011-0376-3) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Effect%20of%20source%20germplasm%20and%20season%20on%20the%20in%20vivo%20haploid%20induction%20rate%20in%20tropical%20maize&journal=Euphytica&doi=10.1007%2Fs10681-011-0376-3&volume=180&issue=2&pages=219-26&publication_year=2011&author=Kebede%2CAZ&author=Dhillon%2CBS&author=Schipprack%2CW&author=Araus%2CJL&author=B%C3%A4nziger%2CM&author=Semagn%2CK&author=Alvarado%2CG&author=Melchinger%2CAE)

[^12]: LeCun Y, Bengio Y, Hinton G. Deep learning. Nature. 2015; 521(7553):436–44.

[Article](https://doi.org/10.1038%2Fnature14539) [CAS](https://link.springer.com/articles/cas-redirect/1:CAS:528:DC%2BC2MXht1WlurzP) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Deep%20learning&journal=Nature&doi=10.1038%2Fnature14539&volume=521&issue=7553&pages=436-44&publication_year=2015&author=LeCun%2CY&author=Bengio%2CY&author=Hinton%2CG)

[^13]: Krizhevsky A, Sutskever I, Hinton GE. Imagenet classification with deep convolutional neural networks. In: Advances in Neural Information Processing Systems. NY: Curran Associates, Inc.: 2012. p. 1097–1105.

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Imagenet%20classification%20with%20deep%20convolutional%20neural%20networks&publication_year=2012&author=Krizhevsky%2CA&author=Sutskever%2CI&author=Hinton%2CGE)

[^14]: Le Cun BB, Denker JS, Henderson D, Howard RE, Hubbard W, Jackel LD. Handwritten digit recognition with a back-propagation network. In: Advances in Neural Information Processing Systems. Denver: Morgan-Kaufmann: 1990.

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Handwritten%20digit%20recognition%20with%20a%20back-propagation%20network&publication_year=1990&author=Le%20Cun%2CBB&author=Denker%2CJS&author=Henderson%2CD&author=Howard%2CRE&author=Hubbard%2CW&author=Jackel%2CLD)

[^15]: Alipanahi B, Delong A, Weirauch MT, Frey BJ. Predicting the sequence specificities of dna-and rna-binding proteins by deep learning. Nat Biotechnol. 2015.

[^16]: Zhou J, Troyanskaya OG. Predicting effects of noncoding variants with deep learning-based sequence model. Nat Methods. 2015; 12(10):931–4.

[Article](https://doi.org/10.1038%2Fnmeth.3547) [CAS](https://link.springer.com/articles/cas-redirect/1:CAS:528:DC%2BC2MXhtlynsL%2FL) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Predicting%20effects%20of%20noncoding%20variants%20with%20deep%20learning-based%20sequence%20model&journal=Nat%20Methods&doi=10.1038%2Fnmeth.3547&volume=12&issue=10&pages=931-4&publication_year=2015&author=Zhou%2CJ&author=Troyanskaya%2COG)

[^17]: Liu Z, Gao J, Yang G, Zhang H, He Y. Localization and classification of paddy field pests using a saliency map and deep convolutional neural network. Sci Rep. 2016; 6:1–12.

[Article](https://doi.org/10.1038%2Fs41598-016-0001-8) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Localization%20and%20classification%20of%20paddy%20field%20pests%20using%20a%20saliency%20map%20and%20deep%20convolutional%20neural%20network&journal=Sci%20Rep&doi=10.1038%2Fs41598-016-0001-8&volume=6&pages=1-12&publication_year=2016&author=Liu%2CZ&author=Gao%2CJ&author=Yang%2CG&author=Zhang%2CH&author=He%2CY)

[^18]: Lee SH, Chan CS, Wilkin P, Remagnino P. Deep-plant: Plant identification with convolutional neural networks. In: Image Processing (ICIP), 2015 IEEE International Conference On. Quebec City: IEEE: 2015. p. 452–456.

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Deep-plant%3A%20Plant%20identification%20with%20convolutional%20neural%20networks&publication_year=2015&author=Lee%2CSH&author=Chan%2CCS&author=Wilkin%2CP&author=Remagnino%2CP)

[^19]: Slavkovikj V, Verstockt S, De Neve W, Van Hoecke S, Van de Walle R. Hyperspectral image classification with convolutional neural networks. In: Proc. of the 23rd ACM Intl. Conference on Multimedia. ACM: 2015. p. 1159–1162.

[^20]: Veeramani B, Raymond J, Chanda P. Deep learning for classifying maize seeds in double haploid induction process In: Cai Z, Daescu O, Li M, editors. Bioinformatics Research and Applications, 13th International Symposium, ISBRA 2017, Honolulu, HI, USA, May 29 – June 2, 2017, Proceedings. Lecture Notes in Bioinformatics. Honolulu: Springer: 2017.

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Deep%20learning%20for%20classifying%20maize%20seeds%20in%20double%20haploid%20induction%20process&publication_year=2017&author=Veeramani%2CB&author=Raymond%2CJ&author=Chanda%2CP)

[^21]: Kurtulmus F, Alibas I, Kavdir I. Classification of pepper seeds using machine vision based on neural network. Int J Agric Biol Eng. 2016; 9(1):51.

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Classification%20of%20pepper%20seeds%20using%20machine%20vision%20based%20on%20neural%20network&journal=Int%20J%20Agric%20Biol%20Eng&volume=9&issue=1&publication_year=2016&author=Kurtulmus%2CF&author=Alibas%2CI&author=Kavdir%2CI)

[^22]: Haralick RM, Shanmugam K, et al.Textural features for image classification. IEEE Trans Syst Man Cybern. 1973; SMC-3(6):610–21.

[Article](https://doi.org/10.1109%2FTSMC.1973.4309314) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Textural%20features%20for%20image%20classification&journal=IEEE%20Trans%20Syst%20Man%20Cybern&doi=10.1109%2FTSMC.1973.4309314&volume=SMC-3&issue=6&pages=610-21&publication_year=1973&author=Haralick%2CRM&author=Shanmugam%2CK)

[^23]: Friedman J, Hastie T, Tibshirani R. The Elements of Statistical Learning vol. 1. New York: Springer; 2001.

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=The%20Elements%20of%20Statistical%20Learning%20vol.%201&publication_year=2001&author=Friedman%2CJ&author=Hastie%2CT&author=Tibshirani%2CR)

[^24]: Coelho LP. Mahotas: Open source software for scriptable computer vision. J Open Res Softw. 2013; 1(1):e3. [https://doi.org/10.5334/jors.ac](https://doi.org/10.5334/jors.ac)

[Article](https://doi.org/10.5334%2Fjors.ac) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Mahotas%3A%20Open%20source%20software%20for%20scriptable%20computer%20vision&journal=J%20Open%20Res%20Softw&doi=10.5334%2Fjors.ac&volume=1&issue=1&publication_year=2013&author=Coelho%2CLP)

[^25]: Pedregosa F, Varoquaux G, Gramfort A, Michel V, Thirion B, Grisel O, Blondel M, Prettenhofer P, Weiss R, Dubourg V, et al.Scikit-learn: Machine learning in python. J Mach Learn Res. 2011; 12(Oct):2825–30.

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Scikit-learn%3A%20Machine%20learning%20in%20python&journal=J%20Mach%20Learn%20Res&volume=12&issue=Oct&pages=2825-30&publication_year=2011&author=Pedregosa%2CF&author=Varoquaux%2CG&author=Gramfort%2CA&author=Michel%2CV&author=Thirion%2CB&author=Grisel%2CO&author=Blondel%2CM&author=Prettenhofer%2CP&author=Weiss%2CR&author=Dubourg%2CV)

[^26]: Zeiler MD, Fergus R. Visualizing and understanding convolutional networks. In: European Conference on Computer Vision. Switzerland: Springer: 2014. p. 818–833.

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Visualizing%20and%20understanding%20convolutional%20networks&publication_year=2014&author=Zeiler%2CMD&author=Fergus%2CR)

[^27]: Yosinski J, Clune J, Nguyen A, Fuchs T, Lipson H. Understanding neural networks through deep visualization. In: Deep Learning Workshop, 31 St International Conference on Machine Learning. Lille: 2015.