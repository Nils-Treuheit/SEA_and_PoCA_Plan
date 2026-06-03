---
title: "YOLOv8 Explained: Understanding Object Detection from Scratch"
source: "https://medium.com/@melissa.colin/yolov8-explained-understanding-object-detection-from-scratch-763479652312"
author:
  - "[[Mélissa Colin]]"
published: 2024-10-19
created: 2026-06-03
description: "YOLOv8, the eighth iteration of the widely-used You Only Look Once (YOLO) object detection algorithm, is known for its speed, accuracy, and"
tags:
  - "clippings"
---
YOLOv8, the eighth iteration of the widely-used *You Only Look Once* (YOLO) object detection algorithm, is known for its speed, accuracy, and efficiency. However, understanding its architecture can be challenging, especially for beginners. In this article, we’ll break down the key components that power YOLOv8, starting from fundamental concepts like convolutional neural networks and residual blocks, and moving toward advanced structures like Feature Pyramid Networks and CSPDarknet53. By the end, you’ll have a clear understanding of how these elements come together to create one of the most powerful object detection models today.

## Plan

1. **Convolutional Architectures**
2. **Feature Pyramid Networks (FPN)**
3. **Path Aggregation Network (PANet)**
4. **Residual Blocks**
5. **CSPNet (Cross Stage Partial Networks)**
6. **Darknet53 and CSPDarknet53**
7. **How it All Fits Together in YOLOv8**

## 1\. Convolutional Architectures

Convolutional Neural Networks (CNNs) are based on a series of processing layers, with the most fundamental being convolutional layers and pooling layers.

The **convolutional layer** uses the principle of convolution, an image processing technique that involves a multiplication operation between two matrices. One matrix represents the input image, while the other, called a *kernel* (or convolution filter), generates a new matrix, which is the filtered image. This process is computationally efficient and helps highlight image features, such as edges.

![A convolution process showing edge detection using a filter](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*RdWgEO6cEDV3GnJRbbbypw.png)

A convolution process showing edge detection using a filter \[1\]

After each convolution, a **pooling layer** is typically applied to reduce the size of the filtered image by grouping information within smaller windows, allowing for compression while preserving essential features. One common type of pooling is **max-pooling**, which takes the maximum value in the window.

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*gaD6SJ6kQNVOclE_WkwLNQ.png)

Illustration of the max-pooling process. \[2\]

This cycle of convolution and pooling is repeated several times in a CNN architecture, allowing for the gradual extraction of important features from the image. The output data is then flattened and passed through a **fully connected layer** of neurons. Traditionally, the output from this layer is processed using a *softmax* function to produce classification probabilities.

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*1lgnoaKSwxyKlEl6)

A simplified diagram of a CNN architecture \[3\]

## 2\. Feature Pyramid Networks (FPN)

The **Feature Pyramid Network** (FPN) is an architecture designed to enhance object detection and image segmentation performance. It leverages outputs from various convolutional layers to create a pyramid representation of features, allowing for better detection of objects at different scales.

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*geMPKTA65dBfAe5h-YfXQg.png)

Diagram illustrating the architecture of a Feature Pyramid Network (FPN) for multi-scale object detection. \[4\]

As we progress through the layers of a convolutional network, deeper layers tend to capture fine details, such as small objects, while earlier layers focus on patterns, shapes, and edges of larger objects. The goal of FPN is to use not only the output from the final convolutional layer but also outputs from several intermediate layers to detect objects at multiple scales. This multi-scale detection capability is key to FPN’s effectiveness.

## 3\. Path Aggregation Network (PANet)

he **Path Aggregation Network (PANet)** is an improvement over the **Feature Pyramid Network (FPN)** architecture. PANet strengthens the connections between different feature scales and introduces additional mechanisms for better information aggregation.

## Get Mélissa Colin’s stories in your inbox

Join Medium for free to get updates from this writer.

To better understand the difference between FPN and PANet, imagine a building with several floors. Each floor represents a different level of detail in an image: at the bottom, you see fine details (small objects), while at the top, you get a broader view (larger objects).

- **FPN** works like an elevator that starts at the ground floor (small details) and goes up to the top floor (the global view). At each floor, it gathers information, allowing the model to understand the image at multiple scales.
- **PANet** adds another elevator that descends from the top floor back to the ground floor, combining information from each floor on its way down. In short, PANet ensures that all information (from both the top and bottom) is thoroughly aggregated.
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*wCjblwHrb6E7bBdoUrhTKw.png)

Structure of PANet \[5\]

## 4\. Residual Blocks

Residual blocks were introduced by the ResNet (Residual Network) architecture. They are designed to address the challenges of training very deep networks, such as gradient degradation and information loss.

A residual block typically consists of two or three convolutional layers, but its defining feature is the inclusion of a direct connection (also called a *skip connection*) that bypasses these layers and links the input of the block directly to its output. This is illustrated in the image below. The input information is added to the output of the convolutional layers before being passed to the next stage.

In simple terms, if the block’s input is represented by *x* and *F(x)* represents the transformation function of the convolutional layers, the output of the block is *F(x)+x.*

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*6YKGwS5qeqZf3tA2_35p4A.png)

A residual block with a skip connection \[6\]

## 5\. CSPNet (Cross Stage Partial Networks)

**CSPNet (Cross Stage Partial Network)** is a technique used in neural networks to improve the efficiency and performance of computer vision models. It works by splitting the feature map into two parts before processing them through a network block. One part flows through the network as usual, while the other part is added later. This approach reduces the computational load of the network, making it lighter without sacrificing its processing capacity. CSPNet helps balance the trade-off between accuracy and speed, ensuring the model remains powerful yet efficient.

## 6\. Darknet53 and CSPDarknet53

**Darknet53** (see image below, part a) is a Convolutional Neural Network (CNN) primarily used as the backbone (the concept of a backbone will be explained in the following section) in object detection models. It was originally developed for YOLOv3. Darknet53 was designed to be both fast and accurate, enabling the extraction of relevant features from an image.

**CSPDarknet53** (see image below, part b), on the other hand, is an enhanced version of Darknet53 that was developed for YOLOv4. It incorporates the CSPNet concept to optimize feature learning and reduce computational redundancy.

![](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*mgaF_gtQ5ytcO2uJxpLL7A.png)

Architecture of Darknet53 (part a) and CSPDarknet53 (part b). \[7\]

## 7\. How it All Fits Together in YOLOv8

Once we understand each of these methods, we can better grasp how every layer in YOLOv8 functions.

In fact, YOLOv8 doesn’t introduce major technical innovations compared to its predecessors. However, its architecture has been streamlined and simplified into blocks. It consists of 23 primary layers, each of which contains sub-layers, which in turn contain more sub-layers. In essence, YOLOv8 is like a Russian nesting doll of layers.

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*frzqTxCGA8DHEHMeHug7JQ.png)

Detailed illustration of YOLOv8 model architecture. The Backbone, Neck, and Head are the three parts of our model, and C2f, ConvModule, DarknetBottleneck, and SPPF are modules. \[8\]

The available diagrams of YOLOv8 online can sometimes seem overly complex at first glance. To better observe these layers, a breakdown of the model’s layer dictionary can reveal its full architecture and clarify how they are stacked. YOLOv8 is composed of seven “ConvModule” layers, eight “C2f” layers, one “SPPF” layer, two “Unsample” layers, four “Concat” layers, and one final detection layer.

```c
import torch
model_path = "my_model.pt"
model_dict = torch.load(model_path, map_location=torch.device('cpu'))
model = model_dict['model']
print(model)
```

By visualizing the components of each layer, we can see how they fit into the broader architecture. YOLOv8 can be divided into three key parts:

1. **Backbone**: This is responsible for feature extraction from the input image. It uses a modified version of CSPDarknet53 and aims to capture simple patterns like edges and textures in its early layers. As we go deeper into the network, it captures more detailed features of the image.
2. **Neck**: This part is in charge of fusing the features extracted by the backbone. It uses a PANet (Path Aggregation Network) to combine features at different scales. The convolutional layers P3, P4, and P5 are transmitted to various parts of the pyramid (layers 11, 14, and 20) to ensure the model can detect objects of various sizes.
3. **Head**: This consists of three detection heads, which are connected to the three outputs of the PANet. These detection heads generate bounding boxes, assign confidence scores, and classify the boxes according to their categories. They also eliminate redundant detections of the same object, which may appear at different scales.
```c
Backbone:
    ConvModule (Layer 0) - P1
    ConvModule (Layer 1) - P2
    ConvModule (Layer 2)
    C2f (Layer 3)
    ConvModule (Layer 4) - P3
    C2f (Layer 5)
    ConvModule (Layer 6) - P4
    C2f (Layer 7)
    SPPF (Layer 9)

Neck:
    Upsample (Layer 10)
    Concat (Layer 11)
    C2f (Layer 12)
    Upsample (Layer 13)
    Concat (Layer 14)
    C2f (Layer 15)
    ConvModule (Layer 16)
    Concat (Layer 17)
    C2f (Layer 18)

Head:
    ConvModule (Layer 19)
    Concat (Layer 20)
    C2f (Layer 21)
    Detection Layer (Layer 22)
```

Although Ultralytics (the developer of YOLOv8) does not explicitly label these three parts in its official model representation, the division is commonly accepted by the community because it mirrors the structure of previous YOLO versions and helps simplify the model’s understanding.

I hope this explanation has clarified the workings of YOLOv8 for you. Feel free to explore additional resources and materials to deepen your understanding of these concepts further!