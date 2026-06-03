---
title: "What Is YOLOv11? An Introduction"
source: "https://blog.roboflow.com/what-is-yolo11/"
author:
  - "[[James Gallagher]]"
published: 2024-11-06
created: 2026-06-03
description: "Learn what YOLO11 is, the tasks for which you can use the YOLO11 architecture, and how to start using YOLO11."
tags:
  - "clippings"
---
![use YOLO11](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/size/w1200/format/webp/2024/10/16_9-4--Medium.jpeg)

[YOLOv11](https://yolov11.com/?ref=blog.roboflow.com) is a computer vision model architecture from the creators of the [YOLOv5](https://blog.roboflow.com/yolov5-improvements-and-evaluation/) and [YOLOv8](https://blog.roboflow.com/what-is-yolov8/) models. YOLOv11 supports object detection, segmentation, classification, keypoint detection, and oriented bounding box (OBB) detection.

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2025/12/Screenshot-2025-12-04-at-3.24.20---PM.png)

YOLOv11 paper, Figure 1

In this guide, we are going to discuss what [YOLOv11](https://roboflow.com/model/yolo11?ref=blog.roboflow.com) is, how it performs, and how you can train and deploy [YOLOv11](https://roboflow.com/model/yolo11?ref=blog.roboflow.com) models to your own hardware.

![](https://www.youtube.com/watch?v=etjkjZoG2F0)

You can try out [YOLO11](https://roboflow.com/model/yolo11?ref=blog.roboflow.com) in the interactive workflow below.

<iframe src="https://rf-model-embed.vercel.app/what-is-yolo11" title="Roboflow embedded workflow widget for YOLO11"></iframe>

Without further ado, let’s get started!

⚡

RF-DETR Neural Architecture Search (NAS) is faster and more accurate than YOLOv11. Read the [blog post here](https://blog.roboflow.com/train-with-neural-architecture-search/).

## What is YOLOv11?

[YOLOv11](https://roboflow.com/model/yolo11?ref=blog.roboflow.com) is a series of computer vision models with varying sizes and levels of accuracy.

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2025/12/Screenshot-2025-12-04-at-3.24.40---PM.png)

YOLOv11 paper, Figure 1

The YOLOv11x model, the largest in the series, reportedly achieves a 54.7% mAP score when evaluated against the Microsoft COCO benchmark. The smallest model, YOLOv11n reportedly achieves a 39.5% mAP score when evaluated against the same dataset. See how YOLOv11 compares to other object detection models in the [object detection model leaderboard](https://leaderboard.roboflow.com/?ref=blog.roboflow.com).

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2024/11/Screenshot-2024-11-13-at-10.51.06-AM.png)

YOLOv11 paper, Figure 1

![](https://www.youtube.com/watch?v=8UB-lk4hG2I)

## YOLOv11 Architecture: An Overview

[YOLOv11](https://roboflow.com/model/yolo11?ref=blog.roboflow.com) introduces the C3k2 (Cross Stage Partial with kernel size 2) block, SPPF (Spatial Pyramid Pooling - Fast), and C2PSA (Convolutional block with Parallel Spatial Attention) components. These new techniques advance feature extraction and improve model accuracy which continues the YOLO lineage of better models for real-time object detection use cases.

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2024/11/Screenshot-2024-11-13-at-11.14.35-AM.png)

YOLOv11 paper, Figure 1

The C3k2 block replaces the C2f block in previous [YOLO models](https://blog.roboflow.com/guide-to-yolo-models/) which is more computationally efficient and improves processing speed.

YOLOv11 supports multiple task types including object detection, classification, image segmentation, keypoint detection, and Oriented Bounding Box (OBB).

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2024/11/Screenshot-2024-11-13-at-11.13.38-AM-min.png)

For a deeper dive into the architecture, we recommend reading the [YOLOv11 paper](https://arxiv.org/pdf/2410.17725?ref=blog.roboflow.com).

## YOLOv11 Annotation Format

YOLOv11 uses the [YOLO PyTorch TXT](https://roboflow.com/formats/yolov5-pytorch-txt?ref=blog.roboflow.com) annotation format, a modified version of the [Darknet annotation](https://roboflow.com/formats/yolo-darknet-txt?ref=blog.roboflow.com) format. If you need to convert data to YOLO PyTorch TXT for use in your YOLOv11 model, we have you covered. Check out [this tool](https://roboflow.com/formats/yolov11-pytorch-txt?ref=blog.roboflow.com) to learn how to convert data for use in your new YOLOv11 model.

## YOLOv11 Data Labeling Tool

The creator and maintainer of YOLOv11 has partnered with [Roboflow](https://roboflow.com/?ref=blog.roboflow.com) to be a recommended annotation and export tool for use in your YOLOv11 projects. Using Roboflow, you can annotate data for all the tasks YOLOv11 supports – object detection, classification, and segmentation – and export data so that you can use it with the YOLOv11 CLI or Python package. See the [YOLOv11 data labeling guide](https://roboflow.com/how-to-label/yolo11?ref=blog.roboflow.com).

## Getting Started with YOLOv11

To get started applying YOLOv11 to your own use case, check out our guide on [how to train YOLOv11 on custom dataset](https://blog.roboflow.com/yolov11-how-to-train-custom-data/).

To see what others are doing with YOLO11, [browse Roboflow Universe for other YOLOv11 models](https://universe.roboflow.com/search?q=model%3Ayolov11&ref=blog.roboflow.com), datasets, and inspiration.

## Deploy YOLOv11 Models

In addition to using the Roboflow hosted API for deployment, you can use [Roboflow Inference](https://github.com/roboflow/inference?ref=blog.roboflow.com), an open source inference solution. Inference works with CPU and GPU, giving you immediate access to a range of devices, from the NVIDIA Jetson (i.e. Jetson Nano or Orin) to ARM CPU devices.

With [Roboflow Inference](https://inference.roboflow.com/?ref=blog.roboflow.com), you can self-host and deploy your model on-device.

### Step #1: Upload Model Weights to Roboflow

Once you have finished training a YOLOv11 model, you will have a set of trained weights ready for use with a [hosted API endpoint](https://docs.roboflow.com/inference/hosted-api?ref=blog.roboflow.com). You can upload your model weights to [Roboflow Deploy](https://roboflow.com/deploy?ref=blog.roboflow.com) with the deploy() function in the [Roboflow pip package](https://docs.roboflow.com/python?ref=blog.roboflow.com) to use your trained weights in the cloud.

To upload model weights, first create a new project on Roboflow, upload your dataset, and create a project version. Check out our complete guide on how to [create and set up a project in Roboflow](https://blog.roboflow.com/getting-started-with-roboflow/#full-tutorial-20-minutes-). Then, write a Python script with the following code:

```python
import roboflow
roboflow.login()
rf = roboflow.Roboflow()
project = rf.workspace().project(PROJECT_ID)
project.version(DATASET_VERSION).deploy(model_type="yolo11", model_path=f"{HOME}/runs/detect/train/")
```

Replace PROJECT\_ID with the ID of your project and DATASET\_VERSION with the version number associated with your project. Learn how to [find your project ID and dataset version number.](https://docs.roboflow.com/api-reference/workspace-and-project-ids?ref=blog.roboflow.com)

Shortly after running the above code, your model will be available for use in the Deploy page on your Roboflow project dashboard.

### Step #2: Install Inference

You can deploy applications using the Inference Docker containers or the [pip package](https://inference.roboflow.com/?ref=blog.roboflow.com). Let's use the pip package. First run:

```
pip install inference
```

### Step #3: Run Inference on an Image

Then, create a new Python file and add the following code:

```python
from inference import get_model
import supervision as sv
import cv2

image_file = "image.jpeg"
image = cv2.imread(image_file)
model = get_model(model_id="model-id")

# run inference on our chosen image, image can be a url, a numpy array, a PIL image, etc.
results = model.infer(image)[0]

# load the results into the supervision Detections api
detections = sv.Detections.from_inference(results)

# create supervision annotators
bounding_box_annotator = sv.BoundingBoxAnnotator()
label_annotator = sv.LabelAnnotator()

# annotate the image with our inference results
annotated_image = bounding_box_annotator.annotate(
    scene=image, detections=detections)
annotated_image = label_annotator.annotate(
    scene=annotated_image, detections=detections)

# display the image
sv.plot_image(annotated_image)
```

Above, set your Roboflow workspace ID, model ID, and API key, if you want to use a custom model you have trained in your workspace.

- [Find your workspace and model ID](https://docs.roboflow.com/api-reference/workspace-and-project-ids?ref=blog.roboflow.com)
- [Find your API key](https://docs.roboflow.com/api-reference/authentication?ref=blog.roboflow.com#retrieve-an-api-key)

Also, set the URL of an image on which you want to run inference. This can be a local file.

Here is an example of an image running through the model:

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2024/10/Untitled.png)

The model successfully detected a person in the image, indicated by the purple bounding box on the image.

You can also run inference on a video stream. To learn more about running your model on video streams – from RTSP to webcam feeds – refer to the [Inference video guide](https://inference.roboflow.com/quickstart/run_model_on_rtsp_webcam/?ref=blog.roboflow.com).

## YOLOv11 FAQs

### Does YOLOv11 have a published paper?

Yes, a [YOLOv11 paper](https://arxiv.org/abs/2410.17725?ref=blog.roboflow.com) was published which analyzes YOLOv11 and provides an overview of the architecture. The paper was not published by the developer of YOLOv11.

### Under what license is YOLOv11 covered?

[YOLO11](https://roboflow.com/model/yolo11?ref=blog.roboflow.com) is covered under an AGPL-3.0 license. If you deploy YOLOvv11 models with Roboflow, you automatically get a commercial license to use the model.

### Where is the YOLOv11 source code?

You can find the YOLOv11 source code in the [YOLO11 GitHub repository](https://github.com/ultralytics/ultralytics?ref=blog.roboflow.com).

### What classes can the base YOLOv11 weights identify?

The base YOLOv11 weights that were released with the model were trained on the [Microsoft COCO dataset](https://blog.roboflow.com/coco-dataset/).

## YOLOv11 Conclusion

Announced and launched in September 2024, YOLO11 is the latest series of computer vision models in the YOLO lineage. The model architecture has support for all the same vision tasks as [YOLOv8](https://roboflow.com/model/yolov8?ref=blog.roboflow.com), while offering improved accuracy when evaluated against the COCO dataset benchmark.

In this guide, we walked through the basics of YOLOv11: what the model is, what you can do with it, and how to deploy the model on your own device. To learn more about training YOLOv11 models, refer to our [YOLOv11 training guide](https://blog.roboflow.com/yolov11-how-to-train-custom-data/).

  
<iframe src="https://rf-model-embed.vercel.app/agent?query=Build%20a%20YOLO%20application" title="Roboflow Agent"></iframe>

### Cite this Post

Use the following entry to cite this post in your research:

*. (Nov 6, 2024). What Is YOLOv11? An Introduction. Roboflow Blog: https://blog.roboflow.com/what-is-yolo11/*

Stay Connected

Get the Latest in Computer Vision First

### Written by

James Gallagher

James is a technical writer at Roboflow, with experience writing documentation on how to train and use state-of-the-art computer vision models.

### Topics

- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/ "Computer Vision")