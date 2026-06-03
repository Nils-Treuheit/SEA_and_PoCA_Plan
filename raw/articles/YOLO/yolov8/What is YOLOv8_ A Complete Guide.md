---
title: "What is YOLOv8? A Complete Guide"
source: "https://blog.roboflow.com/what-is-yolov8/"
author:
  - "[[Jacob Solawetz]]"
published: 2024-10-23
created: 2026-06-03
description: "YOLOv8 is a computer vision model architecture that you can use for object detection, segmentation, keypoint detection, and more."
tags:
  - "clippings"
---
![What is YOLOv8? A Complete Guide](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/size/w1200/format/webp/2024/04/image-1791.webp)

The field of computer vision advances with the release of [YOLOv8](https://github.com/ultralytics/ultralytics?ref=blog.roboflow.com), a model that defines a new state of the art for [object detection](https://blog.roboflow.com/object-detection/), [instance segmentation](https://blog.roboflow.com/instance-segmentation/), and [classification](https://blog.roboflow.com/image-classification/).

Along with improvements to the model architecture itself, [YOLOv8](https://roboflow.com/model/yolov8?ref=blog.roboflow.com) introduces developers to a new friendly interface via a PIP package for using the YOLO model.

In this blog post, we will dive into the significance of [YOLOv8](https://roboflow.com/model/yolov8?ref=blog.roboflow.com) in the computer vision world, compare it to similar models in terms of accuracy, and discuss the recent changes in the [YOLOv8](https://roboflow.com/model/yolov8?ref=blog.roboflow.com) GitHub repository.

![](https://www.youtube.com/watch?v=D-D6ZmadzPE)

If you are looking to learn how to train or work with a specific [YOLOv8 model](https://yolov8.com/?ref=blog.roboflow.com), we have dedicated posts to help:

- [Train a YOLOv8 object detection model](https://blog.roboflow.com/how-to-train-yolov8-on-a-custom-dataset/)
- [Train a YOLOv8 pose estimation (keypoint detection) model](https://blog.roboflow.com/train-a-custom-yolov8-pose-estimation-model/)
- [Train a YOLOv8 Oriented Bounding Box (OBB) model](https://blog.roboflow.com/train-yolov8-obb-model/)
- [Train a YOLOv8 classification model](https://blog.roboflow.com/how-to-train-a-yolov8-classification-model/)
- [Train a YOLOv8 segmentation model](https://blog.roboflow.com/how-to-train-yolov8-instance-segmentation/)

You can try a [YOLOv8](https://roboflow.com/model/yolov8?ref=blog.roboflow.com) model with the following Workflow:

<iframe src="https://rf-model-embed.vercel.app/what-is-yolov8" title="Roboflow embedded workflow widget for YOLOv8"></iframe>

In the Workflow above, you can drag and drop an image of an animal and the system will detect the presence of animals using a YOLOv8 model. You can also see how the model compares to [YOLO-World](https://playground.roboflow.com/models/tencent%20ai%20lab/yolo-world?ref=blog.roboflow.com), a zero-shot model.

⚡

RF-DETR Neural Architecture Search (NAS) is faster and more accurate than YOLOv8. Read the [blog post here](https://blog.roboflow.com/train-with-neural-architecture-search/).

## What is YOLOv8?

YOLOv8 is the newest state-of-the-art YOLO model that can be used for object detection, image classification, and instance segmentation tasks. YOLOv8 was developed by the same team who created the [YOLOv5](https://blog.roboflow.com/how-to-train-yolov5-on-a-custom-dataset/) model. YOLOv8 includes numerous architectural and developer experience changes and improvements over YOLOv5.

## How YOLO Grew Into YOLOv8

The [YOLO (You Only Look Once)](https://blog.roboflow.com/guide-to-yolo-models/) series of models has become famous in the computer vision world. YOLO's fame is attributable to its considerable accuracy while maintaining a small model size. YOLO models can be trained on a single GPU, which makes it accessible to a wide range of developers. Machine learning practitioners can deploy it for low cost on edge hardware or in the cloud.

YOLO has been nurtured by the computer vision community since its first launch in 2015 by Joseph Redmond. In the early days (versions 1-4), YOLO was maintained in C code in a custom deep learning framework written by Redmond called [Darknet](https://blog.roboflow.com/training-yolov4-on-a-custom-dataset/).

<video src="https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/media/2023/01/ezgif.com-gif-maker--23-.mp4" width="480" height="238" controls=""></video>

YOLOv5 inferring on a bicycle

The YOLOv8 author shadowed the [YOLOv3 repo in PyTorch](https://blog.roboflow.com/training-a-yolov3-object-detection-model-with-a-custom-dataset/) (a deep learning framework from Facebook). As the training in the shadow repo got better, they eventually launched [YOLOv5](https://blog.roboflow.com/how-to-train-yolov5-on-a-custom-dataset/).

[YOLOv5](https://roboflow.com/model/yolov5?ref=blog.roboflow.com) quickly became the world's SOTA repo given its flexible Pythonic structure. This structure allowed the community to invent new modeling improvements and quickly share them across repository with similar PyTorch methods.

Along with strong model fundamentals, the [YOLOv5](https://roboflow.com/model/yolov5?ref=blog.roboflow.com) maintainers have been committed to supporting a healthy software ecosystem around the model. They actively fix issues and push the capabilities of the repository as the community demands.

In the last two years, various models branched off of the YOLOv5 PyTorch repository, including [Scaled-YOLOv4](https://roboflow.com/model/scaled-yolov4?ref=blog.roboflow.com), [YOLOR](https://blog.roboflow.com/train-yolor-on-a-custom-dataset/), and [YOLOv7](https://blog.roboflow.com/yolov7-breakdown/). Other models emerged around the world out of their own PyTorch based implementations, such as [YOLOX](https://blog.roboflow.com/how-to-train-yolox-on-a-custom-dataset/) and [YOLOv6](https://blog.roboflow.com/how-to-train-yolov6-on-a-custom-dataset/). Along the way, each YOLO model has brought new SOTA techniques that continue to push the model's accuracy and efficiency.

Over the last six months, the newest SOTA version of YOLO, YOLOv8, was launched on January 10th, 2023.

## Why Should I Use YOLOv8?

Here are a few main reasons why you should consider using YOLOv8 for your next computer vision project:

1. YOLOv8 has a high rate of accuracy measured by [Microsoft COCO](https://universe.roboflow.com/microsoft/coco?ref=blog.roboflow.com) and [Roboflow 100](https://www.rf100.org/?ref=blog.roboflow.com).
2. YOLOv8 comes with a lot of developer-convenience features, from an easy-to-use CLI to a well-structured Python package.
3. There is a large community around YOLO and a growing community around the YOLOv8 model, meaning there are many people in computer vision circles who may be able to assist you when you need guidance.

YOLOv8 achieves strong accuracy on COCO. For example, the YOLOv8m model -- the medium model -- achieves a 50.2% [mAP](https://blog.roboflow.com/mean-average-precision/) when measured on COCO. When evaluated against Roboflow 100, a dataset that specifically evaluates model performance on various task-specific domains, YOLOv8 scored substantially better than YOLOv5. More information on this is provided in our performance analysis later in the article.

Furthermore, the developer-convenience features in YOLOv8 are significant. As opposed to other models where tasks are split across many different Python files that you can execute, YOLOv8 comes with a CLI that makes training a model more intuitive. This is in addition to a Python package that provides a more seamless coding experience than prior models.

The community around YOLO is notable when you are considering a model to use. Many computer vision experts know about YOLO and how it works, and there is plenty of guidance online about using YOLO in practice. Although YOLOv8 is new as of writing this piece, there are many guides online that can help.

Let's do a deep dive into the architecture and what makes YOLOv8 different from prior [YOLO models](https://blog.roboflow.com/guide-to-yolo-models/).

## YOLOv8 Architecture: A Deep Dive

YOLOv8 does not yet have a published paper, so we lack direct insight into the direct research methodology and ablation studies done during its creation. With that said, we analyzed the repository and information available about the model to start documenting what's new in YOLOv8.

If you want to peer into the code yourself, check out [the YOLOv8 repository](https://github.com/ultralytics/ultralytics?ref=blog.roboflow.com) and you view [this code differential](https://github.com/ultralytics/yolov5/compare/master...exp13?ref=blog.roboflow.com) to see how some of the research was done.

Here we provide a quick summary of impactful modeling updates and then we will look at the model's evaluation, which speaks for itself.

The following image made by GitHub user RangeKing shows a detailed vizualisation of the network's architecture.

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2024/04/image-1799.webp)

YOLOv8 Architecture, visualisation made by GitHub user RangeKing

### Anchor Free Detection

YOLOv8 is an anchor-free model. This means it predicts directly the center of an object instead of the offset from a known [anchor box](https://blog.roboflow.com/what-is-an-anchor-box/).

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2024/04/image-1806.webp)

Visualization of an anchor box in YOLO

[Anchor boxes](https://blog.roboflow.com/what-is-an-anchor-box/) were a notoriously tricky part of earlier YOLO models, since they may represent the distribution of the target benchmark's boxes but not the distribution of the custom dataset.

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2024/04/image-1812.webp)

The detection head of YOLOv5, visualized in netron.app

Anchor free detection reduces the number of box predictions, which speeds up [Non-Maximum Suppression](https://blog.roboflow.com/how-to-code-non-maximum-suppression-nms-in-plain-numpy/) (NMS), a complicated post processing step that sifts through candidate detections after inference.

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2024/04/image-1816.webp)

The detection head for YOLOv8, visualized in netron.app

### New Convolutions

The stem's first `6x6` conv is replaced by a `3x3`, the main building block was changed, and [C2f](https://github.com/ultralytics/ultralytics/blob/dba3f178849692a13f3c43e81572255b1ece7da9/ultralytics/nn/modules.py?ref=blog.roboflow.com#L196) replaced [C3](https://github.com/ultralytics/yolov5/blob/cdd804d39ff84b413bde36a84006f51769b6043b/models/common.py?ref=blog.roboflow.com#L157). The module is summarized in the picture below, where "f" is the number of features, "e" is the expansion rate and CBS is a block composed of a `Conv`, a `BatchNorm` and a `SiLU` later.

In `C2f`, all the outputs from the `Bottleneck` (fancy name for two 3x3 `convs` with residual connections) are concatenated. While in `C3` only the output of the last `Bottleneck` was used.

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2024/04/image-1822.webp)

```
C2f
```

The `Bottleneck` is the same as in YOLOv5 but the first conv's kernel size was changed from `1x1` to `3x3`. From this information, we can see that YOLOv8 is starting to revert to the ResNet block defined in 2015.

In the neck, features are concatenated directly without forcing the same channel dimensions. This reduces the parameters count and the overall size of the tensors.

### Closing the Mosaic Augmentation

Deep learning research tends to focus on model architecture, but the training routine in YOLOv5 and YOLOv8 is an essential part of their success.

YOLOv8 augments images during training online. At each epoch, the model sees a slightly different variation of the images it has been provided.

One of those augmentations is called [mosaic augmentation](https://blog.roboflow.com/advanced-augmentations/). This involves stitching four images together, forcing the model to learn objects in new locations, in partial occlusion, and against different surrounding pixels.

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2024/04/image-1831.webp)

Mosaic augmentation of chess board photos

However, this augmentation is empirically shown to degrade performance if performed through the whole training routine. It is advantageous to turn it off for the last ten training epochs.

This sort of change is exemplary of the careful attention YOLO modeling has been given in overtime in the YOLOv5 repo and in the YOLOv8 research.

## YOLOv8 Accuracy Improvements

YOLOv8 research was primarily motivated by empirical evaluation on [the COCO benchmark](https://blog.roboflow.com/coco-dataset/). As each piece of the network and training routine are tweaked, new experiments are run to validate the changes effect on COCO modeling.

### YOLOv8 COCO Accuracy

COCO (Common Objects in Context) is the industry standard benchmark for evaluating object detection models. When comparing models on COCO, we look at the [mAP](https://blog.roboflow.com/mean-average-precision/) value and FPS measurement for inference speed. Models should be compared at similar inference speeds.

The image below shows the accuracy of YOLOv8 on COCO, using data collected by the team and published in their [YOLOv8 README](https://github.com/ultralytics/ultralytics?ref=blog.roboflow.com):

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2024/04/image-1838.webp)

YOLOv8 COCO evaluation

YOLOv8 COCO accuracy is state of the art for models at comparable inference latencies as of writing this post.

### RF100 Accuracy

At Roboflow, we have drawn 100 sample datasets from [Roboflow Universe](https://universe.roboflow.com/?ref=blog.roboflow.com), a repository of over 100,000 datasets, to evaluate how well models generalize to new domains. Our benchmark, developed with support from Intel, is a benchmark for computer vision practitioners designed to provide a better answer to the question: "how well will this model work on my custom dataset?"

We evaluated YOLOv8 on our [RF100](https://www.rf100.org/?ref=blog.roboflow.com) benchmark alongside YOLOv5 and YOLOv7, the following box plots show each model's *mAP@.50.*

*We run the small version of each model for 100 epochs, we ran once with a single seed so due to gradient lottery take this result with a **grain of salt.***

The box plot below tells us YOLOv8 had fewer outliers and an overall better [mAP](https://blog.roboflow.com/mean-average-precision/) when measured against the Roboflow 100 benchmark.

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2024/04/image-1849.webp)

YOLOs mAP@.50 against RF100

The following bar plot shows the average *mAP@.50* for each RF100 category. Again, YOLOv8 outperforms all previous models.

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2024/04/image-1859.webp)

YOLOs average mAP@.50 against RF100 categories

Relative to the YOLOv5 evaluation, the YOLOv8 model produces a similar result on each dataset, or improves the result significantly.

## YOLOv8 Repository and PIP Package

The [YOLOv8 code repository](https://github.com/ultralytics/ultralytics?ref=blog.roboflow.com) is designed to be a place for the community to use and iterate on the model. Since we know this model will be continually improved, we can take the initial YOLOv8 model results as a baseline, and expect future improvements as new mini versions are released.

The best outcome that we can hope for is that researchers begin to develop their networks on top of the repository. Research has been happening in forks of YOLOv5, but it would be better if models were made in one location and eventually merged into the main line.

### YOLOv8 Repository Layout

The YOLOv8 models utilize similar code to YOLOv5 with new structure where classification, instance segmentation, and object detection task types are supported with the same code routines.

Models are still initialized with the same [YOLOv5 YAML format](https://roboflow.com/formats/yolov8-pytorch-txt?ref=blog.roboflow.com) and the dataset format remains the same as well.

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2024/04/image-1867.webp)

### YOLOv8 CLI

The `ultralytics` package is distributed with a CLI. This will be familiar to many YOLOv5 users where the core training, detection, and export interactions were also accomplished via CLI.

```
yolo task=detect mode=val model={HOME}/runs/detect/train/weights/best.pt data={dataset.location}/data.yaml
```

You can pass a `task` in `[detect, classify, segment]`, a `mode` in `[train, predict, val, export]`, a `model` as a uninitialized `.yaml` or as a previously trained `.pt` file.

### YOLOv8 Python Package

In addition to the CLI tool available, YOLOv8 is now distributed as a PIP package. This makes local development a little harder, but unlocks all of the possibilities of weaving YOLOv8 into your Python code.

```
from ultralytics import YOLO

# Load a model
model = YOLO("yolov8n.yaml")  # build a new model from scratch
model = YOLO("yolov8n.pt")  # load a pretrained model (recommended for training)

# Use the model
results = model.train(data="coco128.yaml", epochs=3)  # train the model
results = model.val()  # evaluate model performance on the validation set
results = model("https://ultralytics.com/images/bus.jpg")  # predict on an image
success = YOLO("yolov8n.pt").export(format="onnx")  # export a model to ONNX format
```

## The YOLOv8 Annotation Format

YOLOv8 uses the YOLOv5 PyTorch TXT annotation format, a modified version of the Darknet annotation format. If you need to convert data to YOLOv5 PyTorch TXT for use in your YOLOv8 model, we have you covered. Check out our [Roboflow Convert](https://roboflow.com/formats/yolov8-pytorch-txt?ref=blog.roboflow.com) tool to learn how to convert data for use in your new YOLOv8 model.

## YOLOv8 Labeling Tool

The creator and maintainer of YOLOv8 has partnered with Roboflow to be a recommended annotation and export tool for use in your YOLOv8 projects. Using Roboflow, you can annotate data for all the tasks YOLOv8 supports – object detection, classification, and segmentation – and export data so that you can use it with the YOLOv8 CLI or Python package.

## Getting Started with YOLOv8

To get started applying YOLOv8 to your own use case, check out our guide on [how to train YOLOv8 on custom dataset](https://blog.roboflow.com/how-to-train-yolov8-on-a-custom-dataset/).

To see what others are doing with YOLOv8, [browse Roboflow Universe for other YOLOv8 models](https://blog.roboflow.com/yolov8-models-apis-datasets/), datasets, and inspiration.

For practitioners who are putting their model into production and are using active learning strategies to continually update their model - we have added a pathway where you can [deploy your YOLOv8 model](https://blog.roboflow.com/upload-model-weights-yolov8/), using it in our inference engines and for label assist on your dataset. Alternatively, you can deploy YOLOv8 on device using [Roboflow Inference](https://inference.roboflow.com/?ref=blog.roboflow.com), an open source inference server.

## Deploy YOLOv8 Models to Roboflow

Once you have finished training a YOLOv8 model, you will have a set of trained weights ready for use with a [hosted API endpoint](https://docs.roboflow.com/inference/hosted-api?ref=blog.roboflow.com). You can upload your model weights to [Roboflow Deploy](https://roboflow.com/deploy?ref=blog.roboflow.com) with the deploy() function in the [Roboflow pip package](https://docs.roboflow.com/python?ref=blog.roboflow.com) to use your trained weights in the cloud.

To upload model weights, first create a new project on Roboflow, upload your dataset, and create a project version. Check out our complete guide on how to [create and set up a project in Roboflow](https://blog.roboflow.com/getting-started-with-roboflow/#full-tutorial-20-minutes-). Then, write a Python script with the following code:

```python
import roboflow

roboflow.login()

rf = roboflow.Roboflow()

project = rf.workspace().project(PROJECT_ID)

project.version(DATASET_VERSION).deploy(model_type="yolov8", model_path=f"{HOME}/runs/detect/train/")
```

Replace PROJECT\_ID with the ID of your project and DATASET\_VERSION with the version number associated with your project. Learn how to [find your project ID and dataset version number.](https://docs.roboflow.com/api-reference/workspace-and-project-ids?ref=blog.roboflow.com)

Shortly after running the above code, your model will be available for use in the Deploy page on your Roboflow project dashboard.

## Deploy YOLOv8 Models to the Edge

In addition to using the Roboflow hosted API for deployment, you can use [Roboflow Inference](https://github.com/roboflow/inference?ref=blog.roboflow.com), an open source inference solution that has powered millions of API calls in production environments. Inference works with CPU and GPU, giving you immediate access to a range of devices, from the NVIDIA Jetson to TRT-compatible devices to ARM CPU devices.

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2024/04/image-1874.webp)

With [Roboflow Inference](https://inference.roboflow.com/?ref=blog.roboflow.com), you can self-host and deploy your model on-device.

You can deploy applications using the Inference Docker containers or the [pip package](https://inference.roboflow.com/?ref=blog.roboflow.com). Let's use the pip package. First run:

```bash
pip install inference
```

Then, create a new Python file and add the following code:

```python
from inference import get_model
import supervision as sv
import cv2

# define the image url to use for inference
image_file = "image.jpeg"
image = cv2.imread(image_file)

# load a pre-trained yolov8n model
model = get_model(model_id="yolov8n-640")

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

![](https://storage.ghost.io/c/2c/8d/2c8d8c0d-1c15-4b6d-825e-02b78d61d40a/content/images/2024/09/Screenshot-2024-09-05-at-11.53.22.png)

The model successfully detected a person in the image, indicated by the purple bounding box on the image.

To use your YOLOv8 model commercially with Inference, you will need a Roboflow Enterprise license, through which you gain a pass-through license for using YOLOv8. An enterprise license also grants you access to features like advanced device management, multi-model containers, auto-batch inference, and more.

To learn more about deploying commercial applications with Roboflow Inference, [contact the Roboflow sales team](https://roboflow.com/sales?ref=blog.roboflow.com).

## YOLOv8 FAQs

### What are the versions of YOLOv8?

YOLOv8 has five versions as of its release on January 10th, 2023, ranging from YOLOv8n (the smallest model, with a 37.3 mAP score on COCO) to YOLOv8x (the largest model, scoring a 53.9 mAP score on COCO).

### What tasks can YOLOv8 be used for?

YOLOv8 has support for object detection, instance segmentation, and image classification out of the box.

  
<iframe src="https://rf-model-embed.vercel.app/agent?query=Build%20a%20YOLO%20application" title="Roboflow Agent"></iframe>

### Cite this Post

Use the following entry to cite this post in your research:

*, . (Oct 23, 2024). What is YOLOv8? A Complete Guide. Roboflow Blog: https://blog.roboflow.com/what-is-yolov8/*

Stay Connected

Get the Latest in Computer Vision First

### Written by

Jacob Solawetz

Founding Engineer @ Roboflow - ascending the 1/loss

Francesco

Making Neural Networks go brum brum in the day, fighting Siths in the night

### Topics

- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/ "Computer Vision")