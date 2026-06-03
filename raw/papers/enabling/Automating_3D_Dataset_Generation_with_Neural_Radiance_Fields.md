---
title: "Automating 3D Dataset Generation with Neural Radiance Fields"
source: "https://arxiv.org/html/2503.15997v1"
author: "Paul Schulz, Thorsten Hempel, Ayoub Al-Hamadi"
conference: "**ROBOVIS 2025** [[paper]](https://arxiv.org/abs/2503.15997) [[code]]( https://github.com/PaulSK98/Nerf2Dataset)"
description: We present a fully automated pipeline that leverages Radiance Field–based universal 3D representations to rapidly generate high-quality models of arbitrary objects and synthesize large, diverse 3D datasets—enabling state-of-the-art detection and pose-estimation performance despite the scarcity of public 3D data.
tags: [ROBOVIS 2025, Conference]
---
<sup>1</sup>

Paul Schulz 11 [0009-0006-3444-5800](https://orcid.org/0009-0006-3444-5800 "ORCID identifier")    Thorsten Hempel 11 [0000-0002-3621-7194](https://orcid.org/0000-0002-3621-7194 "ORCID identifier")    Ayoub Al-Hamadi 11 [0000-0002-3632-2402](https://orcid.org/0000-0002-3632-2402 "ORCID identifier")

###### Abstract

3D detection is a critical task to understand spatial characteristics of the environment and is used in a variety of applications including robotics, augmented reality, and image retrieval. Training performant detection models require diverse, precisely annotated, and large scale datasets that involve complex and expensive creation processes. Hence, there are only few public 3D datasets that are additionally limited in their range of classes. In this work, we propose a pipeline for automatic generation of 3D datasets for arbitrary objects. By utilizing the universal 3D representation and rendering capabilities of Radiance Fields, our pipeline generates high quality 3D models for arbitrary objects. These 3D models serve as input for a synthetic dataset generator. Our pipeline is fast, easy to use and has a high degree of automation. Our experiments demonstrate, that 3D pose estimation networks, trained with our generated datasets, archive strong performance in typical application scenarios.

###### Keywords:

Dataset Generation Radiance Fields 3D Pose Estimation.

THIS WORK HAS BEEN SUBMITTED TO SPRINGER FOR PUBLICATION. COPYRIGHT MAY BE TRANSFERRED WITHOUT NOTICE, AFTER WHICH THIS VERSION WILL NO LONGER BE ACCESSIBLE.

![Refer to caption](https://arxiv.org/html/2503.15997v1/extracted/6290814/figures/Teaser.png)

Figure 1: Automated dataset generation with our pipeline. The process start with capturing images of an object, continues with 3D model creation and finishes with 3D dataset generation.

## 1 Introduction

3D scene perception and manipulation is a fundamental task in various fields where autonomous agents interact with their environment. Applications in autonomous driving, mobile robotics, medicine or manufacturing [^1] [^2] [^3] [^4] and intuitive human-robot interaction systems [^5] [^6] rely on this capability.

To effectively perform scene manipulation, an agent has to estimate the pose of relevant objects within its environment. State-of-the-art pose estimation methods utilize neural networks that require large, diverse and precisely annotated 3D datasets for training. However, generating this data is a significant challenge, because collecting and annotating 3D datasets is labor-intensive and expensive, particularly when tailored datasets are required for specific applications.

Recent works proposed approaches to automate 3D dataset generation, by incorporating 3D models of the objects of interest [^7] [^8] [^9] [^3] [^10] [^11] [^12]. These methods either generate real or synthetic datasets. Automating real world dataset generation requires dedicated equipment and is limited in its field of application [^10] [^11] [^12]. Synthetic dataset generation does not have these limitations, because it only requires 3D modeling software and the workflow can be easily adapted [^7] [^8] [^9] [^3].

However, real-world and synthetic approaches assume that 3D models of the target objects are available. Creating these models with traditional methods, like 3D scanning or 3D modeling, is expensive and requires expert knowledge. Generating even a single high quality model for an individual target object may require, a textureless CAD model 3D scanning equipment and model refinement [^13]

We address these limitations by proposing a pipeline, which incorporates 3D model creation into the dataset generation process. To archive this, our pipeline creates 3D models from 2D images of target objects by utilizing Radiance Fields. Afterward, our pipeline incorporates these models in a state-of-the-art dataset generation workflow.

In contrast to previous works, our pipeline generates datasets for the pose estimation of arbitrary objects without requiring 3D models. To verify our approach, we applied our pipeline to six objects of varying complexity and trained a 3D pose estimation model on our generated datasets. Furthermore, we tested the trained networks for two tasks: in-hand pose estimation and tabletop pose estimation. In summary, our contribution is an end-to-end dataset generation pipeline, which begins with arbitrary objects and ends with diverse, precisely annotated 3D datasets for these objects.

## 2 Related Works

To the best of our knowledge, our method is the first to automate dataset generation by utilizing Radiance Fields for 3D model creation. Most similarly, Wong et al. [^14] introduced a synthetic dataset generation pipeline, but in contrast to us, they use photogrammetry to create 3D models from 2D images. In the following, we provide an overview of synthetic dataset generation methods and Radiance Field frameworks that could potentially be integrated into our pipeline.

### 2.1 Automated Generation of Synthetic 3D Datasets

#### 2.1.1 Dataset Generation Methods

Synthetic dataset generation takes place in virtual 3D environments, like game engines [^8] or general 3D modeling software [^7]. The dataset generation workflow, proposed by state-of-the-art publications, can be divided in two phases: scene composition and rendering + annotation. Scene composition constructs 3D scenes by placing objects in a virtual environment. These objects include target objects and additional content like distractors. The subsequent rendering + annotation process generates RGB images from the scenes, by applying 3D rendering software. Since all parameters of the 3D scene are known, the step also performs automated annotation [^8] [^14] [^3].

Recent works can be divided in general and specific generation methods. General methods can be applied to arbitrary domains, specific method incorporate domain restrictions and are therefore applied in specific domains.

Hodan et al. [^7] created a general generator for the BOP challenge. Tremblay et al. [^8] developed another general method for the pose estimation of arbitrary household objects. Both methods are suitable for arbitrary target objects, because they do not incorporate domain restrictions. In contrast to that, Sun et al. [^15] developed a specific method, which creates datasets for the autonomous driving domain. Their framework incorporates special domain shifts for autonomous driving, like changing weather conditions. Other specific methods [^9] [^3] tackle industrial tasks, like bin packing. These methods apply domain restrictions in the form of special industrial environment. Furthermore, McCormac et al. [^16] applied a specific dataset generator to create a benchmark dataset for household scenes.

#### 2.1.2 Dataset Generation Frameworks

Recent works also introduced dedicated frameworks to develop customized dataset generators [^17] [^18] [^19]. These frameworks incorporate measurements to approach a central limitation of synthetic dataset generation, which is known as the simulation to reality gap (Sim2Real gap). The Sim2Real gap causes a performance drop, when networks trained on synthetic data are applied in the real world. The gap can be divided into the appearance gap and the content gap. The appearance gap is caused by differences in detailed, pixel-level scene appearances, resulting from a lack of realism [^20] [^21]. The content gap is related to the context of simulated scenes. It appears when the scene content does not reflect the variety of objects and environments found in the real world [^20] [^21] [^22]. Dataset generation frameworks incorporate photorealistic rendering and domain randomization to reduce the Sim2Real gap. Photorealistic rendering reduces the appearance gap by making objects and environments appear more realistic. Domain randomization reduces the content gap by randomizing the texture of objects of interest and/or the 3D environment [^17] [^19].

### 2.2 Radiance Fields

In 2020, Mildenhall et al. [^23] introduced Neural Radiance Fields (NeRF) for novel view synthesis. NeRFs combine the function approximation capability of multi layer perceptrons (MLP) with principles of volume rendering. NeRFs learn an unrestricted volumetric 3D representation and the view dependent appearance of an object or a scene from 2D images. Furthermore, they utilize volume rendering principles like ray marching to render photorealistic views from this representation [^23] [^24].

Dataset generators require 3D models of the object in the form of meshes or pointclouds. Extracting such representations is straight forward: In the first step one applies a volume sampling algorithm like Marching Cubes to transfer the volume in a mesh, afterward, one projects the color of the images onto the mesh surface. However, applying this approach, leads to a significant fidelity drop. Despite a tendency to learn smooth volumes [^25], NeRFs learn unrestricted volumes. Therefore, the surfaces extracted from the volume may not resemble the actual surface of the scene. [^26] [^27] [^28] We will refer to this problem as unconstrained surface issue. Recent works approach this problem by regularizing NeRF to resemble surfaces [^29] [^30] [^27]. Wang et al. [^27] and Yariv et al. [^30] approximate the zero level set of the SDF with volume rendering. Oechsle et al. [^29] unify volume and surface rendering by learning an occupancy field. 3D models exported from these representations have a more accurate surface than 3D model exported from standard NeRF models.

NeRFs model view dependent effects as part of the appearance. With the goal of high quality mesh export in mind, this feature can lead to difficulties, since view dependency might be baked into the mesh texture. A way to migrate this problem, is to decompose the appearance in reflectance and illumination [^31].

Tang et al. [^26] present a pipeline for textured mesh generation, which addresses this weakness. They optimize a NeRF and utilize two MLPs for color rendering to separate view-dependent colors into diffuse and specular components. Afterward, they apply a mesh refinement strategy, which adjusts mesh vertices and faces. The diffuse color is baked in a diffuse texture. Combined with the refined mesh, it can be used in traditional 3D modeling software.

## 3 Methodical Approach

Our goal was to create a reliable, fast and easily adaptable synthetic dataset generation pipeline for 3D Pose Estimation. The pipeline should work for arbitrary objects, without requiring 3D models of the target objects. We propose a pipeline that expands a state-of-the-art synthetic dataset generator with a neural rendering framework, specialized on 3D model creation. <sup>1</sup> The design of this pipeline is illustrated in Fig. 2. In the following, we will introduce each component of the pipeline in detail.

![Refer to caption](https://arxiv.org/html/2503.15997v1/x1.png)

Figure 2: Automated dataset generation pipeline, each block is depicted with its corresponding phases.

![Refer to caption](https://arxiv.org/html/2503.15997v1/x2.png)

Figure 3: Design of our Object Capturing block, each capturing phase is displayed with its in- and output. The process begins with image capturing. Structure from Motion and Foreground Extraction perform post-processing on the captured images.

### 3.1 Object Capturing

Object Capturing is the first block of our pipeline. It captures 2D images of a target object and prepares them to be used to train a Radiance Field. The design of our Object Capturing process is depicted in Fig. 3.

The process begins with capturing images of the object of interest. Our setup to perform this task is shown in Fig. 4. Radiance Fields work well when maintaining a constant camera-to-object distance, varying distances may lead to rendering artifacts [^32]. Additionally, Object Capturing should include many perspectives to prevent overfitting to specific views. To ensure both aspects, we captured the object in a 360-degree circular trajectory, with the object placed on a rotating plate and the camera kept static. We masked the static scene parts of the background during this process. We chose this setup because it is easily reproducible by others, as it only requires a fixed camera and a plate to rotate the object. Radiance Fields assume a static object of interest, a static light source and a moving camera. To reduce additional view-depended effects which might be introduced by hurting this assumption, we use diffuse background light during capturing.

Applying Structure-from-Motion (SfM) is the first of two post-processing steps after image capturing. Neural rendering frameworks utilize SfM pipelines like COLMAP [^33] to estimate camera poses for real-world images. Since COL-MAP is the de-facto standard for this step, we applied COLMAP as well. COLMAP relies on a sufficient number of 2D features and 2D-to-3D correspondences to estimate camera positions. To support this process for objects with fewer features, we used a plane with tags as the background during object capturing. Foreground extraction is the second post-processing step, applied during Object Capturing. It ensures that the Radiance Field learns the target object rather than the background. We applied an off-the-shelf foreground extractor to perform this task.

![Refer to caption](https://arxiv.org/html/2503.15997v1/x3.png)

Figure 4: Object capturing set up, we placed the object on a rotating plate and captured it with a static camera.

![Refer to caption](https://arxiv.org/html/2503.15997v1/x4.png)

Figure 5: Design of our Model Generation block, it receives the output of the previous block as input and generates a textured 3D model.

### 3.2 Model Generation

Model Generation is the second stage of our pipeline. It takes the output from the previous block (virtual cameras and masked images) as input and creates high-quality textured meshes. To achieve this, we integrated a NeuS implementation <sup>2</sup> of the Nerf2Mesh framework by Hoang et al. [^26] into the design of the Model Generation Block. The block design is displayed in Fig. 5 As mentioned in the previous section, Radiance Fields struggle with poor texture mapping. Volumetric representations additionally suffer from the unconstrained surface issue. The NeuS-based Nerf2Mesh implementation migrates both of these issues by incorporating a neural surface and by separating the texture into a diffuse and a specular branch. Neural Rendering is the first step in the Model Generation process. It trains a NeuS-based Radiance Field using the virtual cameras and masked images, provided by the Object Capturing process. Furthermore, Neural Rendering applies a Multiresolution Hash Encoding [^34] to speed up the training process. Afterward, the Marching Cubes algorithm extracts a coarse mesh from the Radiance Field. In the last step of Model Generation, Mesh Refinement optimizes the mesh geometry and appearance through differentiable rendering and non-differentiable face adjustment. In the end, a textured mesh is exported, by mapping the texture on the refined mesh.

### 3.3 Dataset Generation

Dataset Generation is the third and final step of our pipeline. It creates a synthetic 3D dataset for 6D Pose Estimation of the target object. Our goal is to design a dataset generation pipeline for arbitrary objects. Therefore, we implemented a general dataset generator. Because dataset generation frameworks integrate measures, which approach the Sim2Real gap, we implemented the block via BlenderProc [^17].

![Refer to caption](https://arxiv.org/html/2503.15997v1/x5.png)

Figure 6: Design of our Dataset Generation block, based on the textured meshes of the subsequent block, it generates a 3D Dataset in automated fashion.

Fig. 6 illustrates our dataset generation workflow. Model Alignment is the first phase of this process. It aligns the textured meshes generated in the previous block with the coordinate axes of the world coordinate system. This step is required, to determine the 3D position of the target objects after Scene Composition. In the next step, Scene Composition creates domain randomized 3D scenes which contain light sources, virtual cameras, the objects of interest and distractors. Lastly, a Rendering + Annotation phase renders images from the virtual cameras and annotates the target objects present in the image. It utilizes BlenderProc’s photorealistic rendering capabilities to approach the appearance gap.

## 4 Experiments

![[Uncaptioned image]](https://arxiv.org/html/2503.15997v1/extracted/6290814/figures/Experiments/OOI/baseball.png)

\[Uncaptioned image\]

We tested Object Capturing and Model Generation for all these objects. Dataset Generation was tested for one object per category. All experiments were conducted with an NVIDIA Quadro RTX 8000 graphics card.

![[Uncaptioned image]](https://arxiv.org/html/2503.15997v1/extracted/6290814/figures/Experiments/Object_Capturing_Block/Object_Capturing/Elephant.jpg)

\[Uncaptioned image\]

### 4.1 Object Capturing

Table 2 displays qualitative results for three of six objects. During Object Capturing, we took 105 images per object, similar to the perspectives shown in the first column of Table 2. Structure from Motion derived camera poses for all six objects of interest. The trajectories for these objects are similar to the trajectories displayed in the second row.

### 4.2 Model Generation

Object Capturing successfully processed all six objects, enabling Model Generation to create textured meshes for each object. Table 3 summarizes the qualitative performance of this process. In the beginning, Neural Rendering trained a NeuS-based Radiance Field for all objects. The first column of Table 3 shows views rendered from three of the six Radiance Fields. The remaining Radiance Fields produce similar results. All Radiance Fields generated novel views of high quality. Minor artifacts are present in challenging areas, such as the central wheel of the multimeter. This qualitative results indicate that the Radiance Fields learned accurate 3D representations, enabling photorealistic view synthesis.

![[Uncaptioned image]](https://arxiv.org/html/2503.15997v1/extracted/6290814/figures/Experiments/Model_Generation_Block/Radiance_Field_Generation/Elephant.png)

\[Uncaptioned image\]

|  | PSNR \[dB\] | LPIPS | SSIM |
| --- | --- | --- | --- |
| Elephant | 35.09 | 0.0105 | 0.994 |
| Cup | 32.52 | 0.0349 | 0.983 |
| Baseball | 38.61 | 0.0103 | 0.996 |
| Remote | 32.72 | 0.0191 | 0.990 |
| Controller | 32.70 | 0.0317 | 0.982 |
| Multimeter | 34.58 | 0.0370 | 0.982 |
| Average | 32.88 | 0.0239 | 0.9878 |

Table 4: Peak Signal to Noise Ratio (PSNR), Learned Perceptual Image Patch Similarity (LPIPS) and Structural Similarity Index Measure (SSIM) for the Radiance Fields, generated during the Neural Rendering Phase.

Table 4 presents quantitative performance metrics of our Radiance Fields on real world test images. Other Radiance Field frameworks [^23] [^34] [^26] achieve similar results on synthetic and real-world datasets of comparable complexity. Therefore, the quantitative results indicate successful 3D representation learning as well. Mesh Export extracted triangular meshes for all six target objects. Due to the NeuS-based Radiance Fields avoiding the unconstrained surface issue, the meshes exhibit relatively high surface quality. However, stronger rendering errors persist in the coarse mesh, for example in the central area of the multimeter. During Mesh Refinement, the coarse meshes were optimized, and the diffuse texture of the appearance model was exported for each object.

Mesh refinement improved vertex positions and mesh faces, correcting minor flaws in the coarse meshes, such as the holes in the elephant’s ear. The textured meshes are displayed in the third row of Table 3. The diffuse mesh textures appear darker than the rendered meshes from the previous phase because they do not account for the lighting conditions during capture. Overall, the meshes demonstrate high fidelity, comparable to the rendered views of the Radiance Fields.

![Refer to caption](https://arxiv.org/html/2503.15997v1/extracted/6290814/figures/Experiments/Dataset_Generation_Block/Ele_1.png)

(a) No occlusion

### 4.3 Dataset Generation

Our Dataset Generation process created a dataset for the elephant, the remote, and the multimeter. It aligned each mesh with Blender’s internal coordinate system in the first step. Afterward, it applied Scene Composition and Rendering + Annotation to create a synthetic dataset. Fig. 7 displays annotated and unannotated images of the elephant dataset, which were generated with this workflow. The first row of Fig. 7 demonstrates measurements to approach the content gap. The first row shows how domain randomization creates diverse backgrounds. Additionally, the second row demonstrates the photorealistic rendering capabilities of BlenderProc.

### 4.4 Pipeline Validation

We trained and tested a pose estimation network on three generated datasets to verify our pipeline. The pose estimation model is based on the DOPE (Deep Object Pose Estimation) [^35] approach that predicts keypoint belief maps and their corresponding affinity maps. DOPE has to be trained per object, therefore we trained 3 different models. Each DOPE network was trained for 100 epochs, with a learning rate of 0.0001 and a batch size of 64.

To verify our pipeline in practical scenarios, we tested the DOPE models for two scenarios: tabletop pose estimation and in-hand pose estimation. Since we generated datasets for custom objects, no public datasets are available to apply these scenarios. Furthermore, a real world test set is not attainable, without significant effort, due to the complex annotation process. To still assess the model’s performance, we recorded test sequences for both tasks and performed a qualitative evaluation.

![Refer to caption](https://arxiv.org/html/2503.15997v1/extracted/6290814/figures/Experiments/Testing/table_top/ele/good.png)

Figure 8: Tabletop pose estimation with elephant (first column), remote (second column), and multimeter (third column) as target objects.

#### 4.4.1 Tabletop Pose Estimation

To test our networks on tabletop pose estimation, we created a scene which contains the target objects and some distractors. All three networks were tested on this sequence, qualitative results of this process are displayed in Fig. 8. The results for the multimeter and the remote indicate successful pose estimation with correct bounding box locations. However, the performance of the DOPE network trained on the elephant dataset was notably worse. Unlike the other two objects, the elephant was evaluated against a complex background. This complexity may have contributed to the detection challenges. To address this issue, we created a test sequence, which solely focuses on the elephant was Fig. 9 shows four frames of this sequence. The DOPE model performed significantly better, showing strong performance.

![Refer to caption](https://arxiv.org/html/2503.15997v1/extracted/6290814/figures/Experiments/Testing/table_top/ele_alone/1.png)

Figure 9: Table-top pose estimation for the elephant in a simpler scenario.

![Refer to caption](https://arxiv.org/html/2503.15997v1/extracted/6290814/figures/Experiments/Testing/In_hand/ele/1.jpeg)

Figure 10: Qualitative results during in-hand detection for the three target objects.

#### 4.4.2 In-Hand Pose Estimation

We recorded a human, holding the target object in different angles, in front of a camera, to derive the test sequences for in-hand pose estimation. The six frames, depicted in Fig. 10 showcases the performance of our the three models. The multimeter and remote networks achieved a performance similar to that of the tabletop scenario. The elephant network produced more accurate bounding boxes compared to the previous scenario. However, the performance of the elephant network was still worse than the performance of the remaining two networks.

### 4.5 Limitations

Our pipeline has some limitations related to the general concept and specific pipeline blocks. The pipeline is limited to the front side of the target objects, it cannot reconstruct the back side of objects, since these parts are not present during Radiance Field training. Subsequently, the back side can neither be meshed nor detected. Furthermore, the generated meshes show high fidelity but contain small errors in ambiguous regions, like the central regions of the multimeter. These errors are introduced during Neural Rendering and inherited in the subsequent phases. This limitation might restrict our pipeline to objects of lower complexity.

## 5 Conclusion and Future Work

In this work, we proposed a pipeline that utilizes Radiance Fields to automatically generate datasets for the 3D pose estimation of arbitrary objects. In contrast to previous works, our pipeline does not require 3D models of the object of interest to generate datasets. Our experiments demonstrated that our pipeline can generate high-quality textured meshes for objects of varying complexity. We also showed that the pipeline is sufficient to train 3D pose estimation models that work in practical scenarios. Our pipeline only involves two manual steps: placing the target objects on the turntable and aligning the model with Blender’s internal coordinate system. Based on these results, we conclude that Radiance Fields can provide 3D models which are sufficient to generate high-quality synthetic datasets in an automated fashion.

Future works will address the conceptual weakness of our pipeline by expanding the model generation to full object reconstruction. Generating two models per object and fusing them is an approach we might pursue in upcoming publications to archive this. Expanding the pipeline to stereo 3D datasets is another promising research direction, we might approach in future works. This could be realized by creating and leveraging stereo images to improve object detection accuracy and enable depth perception. Furthermore, we will expand our qualitative evaluation with a quantitative evaluation. We will apply our pipeline to a benchmark dataset, which contains 3D models and a real world test set, is a way to approach this goal.

#### 5.0.1 Acknowledgements

This work is funded and supported by the Federal Ministry of Education and Research of Germany (BMBF) (AutoKoWAT-3DMAt under grant No. 13N16336), German Research Foundation (DFG) (SEMIAC under grant number No. 502483052) and by the European Regional Development Fund (ERDF) (ENABLING under grant No. ZS/2023/12/182056) and (RoboLab under grant No. ZS/2023/12/182065).

#### 5.0.2

The authors have no competing interests to declare that are relevant to the content of this article.

[^1]: Alexey Dosovitskiy, Germán Ros, Felipe Codevilla, Antonio M. López, and Vladlen Koltun. Carla: An open urban driving simulator. In Conference on Robot Learning, 2017.

[^2]: Thorsten Hempel and Ayoub Al-Hamadi. An online semantic mapping system for extending and enhancing visual slam. Eng. Appl. Artif. Intell., 111:104830, 2022.

[^3]: Kilian Kleeberger, Christian Landgraf, and Marco F. Huber. Large-scale 6d object pose estimation dataset for industrial bin-picking. 2019 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), pages 2573–2578, 2019.

[^4]: Juan Antonio Barragan, Jintan Zhang, Haoying Zhou, Adnan Munawar, and Peter Kazanzides. Realistic data generation for 6d pose estimation of surgical instruments. 2024 IEEE International Conference on Robotics and Automation (ICRA), pages 13347–13353, 2024.

[^5]: Dominykas Strazdas, Jan Hintz, Aly Khalifa, Ahmed A. Abdelrahman, Thorsten Hempel, and Ayoub Al-Hamadi. Robot system assistant (rosa): Towards intuitive multi-modal and multi-device human-robot interaction. Sensors (Basel, Switzerland), 22, 2022.

[^6]: Dominykas Strazdas, Jan Hintz, Anna-Maria Felßberg, and Ayoub Al-Hamadi. Robots and wizards: An investigation into natural human–robot interaction. IEEE Access, 8:207635–207642, 2020.

[^7]: Tomás Hodan, Martin Sundermeyer, Bertram Drost, Yann Labbé, Eric Brachmann, Frank Michel, Carsten Rother, and Jiri Matas. Bop challenge 2020 on 6d object localization. ArXiv, abs/2009.07378, 2020.

[^8]: Jonathan Tremblay, Thang To, and Stan Birchfield. Falling things: A synthetic dataset for 3d object detection and pose estimation. 2018 IEEE/CVF Conference on Computer Vision and Pattern Recognition Workshops (CVPRW), pages 2119–21193, 2018.

[^9]: Xianzhi Li, Rui Cao, Yidan Feng, Kai Chen, Biqi Yang, Chi-Wing Fu, Yichuan Li, Qi Dou, Yunhui Liu, and Pheng-Ann Heng. A sim-to-real object recognition and localization framework for industrial robotic bin picking. IEEE Robotics and Automation Letters, 7:3961–3968, 2022.

[^10]: Roman Kaskman, Sergey Zakharov, Ivan S. Shugurov, and Slobodan Ilic. Homebreweddb: Rgb-d dataset for 6d pose estimation of 3d objects. 2019 IEEE/CVF International Conference on Computer Vision Workshop (ICCVW), pages 2767–2776, 2019.

[^11]: Yu Xiang, Tanner Schmidt, Venkatraman Narayanan, and Dieter Fox. Posecnn: A convolutional neural network for 6d object pose estimation in cluttered scenes. ArXiv, abs/1711.00199, 2017.

[^12]: Pengyuan Wang, Hyunjun Jung, Yitong Li, Siyuan Shen, Rahul Parthasarathy Srikanth, Lorenzo Garattoni, Sven Meier, Nassir Navab, and Benjamin Busam. Phocal: A multi-modal dataset for category-level object pose estimation with photometrically challenging objects. 2022 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), pages 21190–21199, 2022.

[^13]: Quang Hieu. Tran, Jeffrey A. Choate, C. N. Taylor, Scott L Nykl, and David H. Curtis. Monocular vision and machine learning for pose estimation. 2023 IEEE/ION Position, Location and Navigation Symposium (PLANS), pages 128–136, 2023.

[^14]: Matthew Z. Wong, Kiyohito Kunii, Max Baylis, Wai Hong Ong, Pavel Kroupa, and Swen Koller. Synthetic dataset generation for object-to-model deep learning in industrial applications. PeerJ Computer Science, 5, 2019.

[^15]: Tao Sun, Mattia Segu, Janis Postels, Yuxuan Wang, Luc Van Gool, Bernt Schiele, Federico Tombari, and Fisher Yu. Shift: A synthetic driving dataset for continuous multi-task domain adaptation. 2022 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), pages 21339–21350, 2022.

[^16]: John McCormac, Ankur Handa, Stefan Leutenegger, and Andrew J. Davison. Scenenet rgb-d: 5m photorealistic images of synthetic indoor trajectories with ground truth. ArXiv, abs/1612.05079, 2016.

[^17]: Maximilian Denninger, Dominik Winkelbauer, Martin Sundermeyer, Wout Boerdijk, Markus Knauer, Klaus H. Strobl, Matthias Humt, and Rudolph Triebel. Blenderproc2: A procedural pipeline for photorealistic rendering. J. Open Source Softw., 8:4901, 2023.

[^18]: Klaus Greff, Francois Belletti, Lucas Beyer, Carl Doersch, Yilun Du, Daniel Duckworth, David J. Fleet, Dan Gnanapragasam, Florian Golemo, Charles Herrmann, Thomas Kipf, Abhijit Kundu, Dmitry Lagun, Issam Hadj Laradji, Hsueh-Ti Liu, Henning Meyer, Yishu Miao, Derek Nowrouzezahrai, Cengiz Oztireli, Etienne Pot, Noha Radwan, Daniel Rebain, Sara Sabour, Mehdi S. M. Sajjadi, Matan Sela, Vincent Sitzmann, Austin Stone, Deqing Sun, Suhani Vora, Ziyu Wang, Tianhao Wu, Kwang Moo Yi, Fangcheng Zhong, and Andrea Tagliasacchi. Kubric: A scalable dataset generator. 2022 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), pages 3739–3751, 2022.

[^19]: NVIDIA. Replicator. [https://docs.omniverse.nvidia.com](https://docs.omniverse.nvidia.com/), 2022. Accessed: 11, 09, 2024.

[^20]: Nyla Worker Kshitiz Gupta. Closing the sim2real gap with nvidia isaac sim and nvidia isaac replicator. [https://developer.nvidia.com/blog/closing-the-sim2real-gap-with-nvidia-isaac-sim-and-nvidia-isaac-replicator/](https://developer.nvidia.com/blog/closing-the-sim2real-gap-with-nvidia-isaac-sim-and-nvidia-isaac-replicator/), 2022. Accessed: 05, 03, 2024.

[^21]: Amlan Kar, Aayush Prakash, Ming-Yu Liu, Eric Cameracci, Justin Yuan, Matt Rusiniak, David Acuna, Antonio Torralba, and Sanja Fidler. Meta-sim: Learning to generate synthetic datasets. 2019 IEEE/CVF International Conference on Computer Vision (ICCV), pages 4550–4559, 2019.

[^22]: Aayush Prakash, Shaad Boochoon, Mark Brophy, David Acuna, Eric Cameracci, Gavriel State, Omer Shapira, and Stan Birchfield. Structured domain randomization: Bridging the reality gap by context-aware synthetic data. 2019 International Conference on Robotics and Automation (ICRA), pages 7249–7255, 2018.

[^23]: Ben Mildenhall, Pratul P. Srinivasan, Matthew Tancik, Jonathan T. Barron, Ravi Ramamoorthi, and Ren Ng. Nerf: Representing scenes as neural radiance fields for view synthesis. In Andrea Vedaldi, Horst Bischof, Thomas Brox, and Jan-Michael Frahm, editors, Computer Vision – ECCV 2020, pages 405–421, Cham, 2020. Springer International Publishing.

[^24]: Anju Tewari, Otto Fried, Justus Thies, Vincent Sitzmann, S. Lombardi, Z. Xu, Tanaba Simon, Matthias Nießner, Edgar Tretschk, L. Liu, Ben Mildenhall, Pranatharthi Srinivasan, R. Pandey, Sergio Orts-Escolano, S. Fanello, M. Guang Guo, Gordon Wetzstein, J y Zhu, Christian Theobalt, Manju Agrawala, Donald B. Goldman, and Michael Zollhöfer. Advances in neural rendering. Computer Graphics Forum, 41, 2021.

[^25]: Kai Zhang, Gernot Riegler, Noah Snavely, and Vladlen Koltun. Nerf++: Analyzing and improving neural radiance fields. ArXiv, abs/2010.07492, 2020.

[^26]: Jiaxiang Tang, Hang Zhou, Xiaokang Chen, Tianshu Hu, Errui Ding, Jingdong Wang, and Gang Zeng. Delicate textured mesh recovery from nerf via adaptive surface refinement. 2023 IEEE/CVF International Conference on Computer Vision (ICCV), pages 17693–17703, 2023.

[^27]: Peng Wang, Lingjie Liu, Yuan Liu, Christian Theobalt, Taku Komura, and Wenping Wang. Neus: Learning neural implicit surfaces by volume rendering for multi-view reconstruction. ArXiv, abs/2106.10689, 2021.

[^28]: Lior Yariv, Peter Hedman, Christian Reiser, Dor Verbin, Pratul P. Srinivasan, Richard Szeliski, Jonathan T. Barron, and Ben Mildenhall. Bakedsdf: Meshing neural sdfs for real-time view synthesis. ACM SIGGRAPH 2023 Conference Proceedings, 2023.

[^29]: Michael Oechsle, Songyou Peng, and Andreas Geiger. Unisurf: Unifying neural implicit surfaces and radiance fields for multi-view reconstruction. 2021 IEEE/CVF International Conference on Computer Vision (ICCV), pages 5569–5579, 2021.

[^30]: Lior Yariv, Jiatao Gu, Yoni Kasten, and Yaron Lipman. Volume rendering of neural implicit surfaces. ArXiv, abs/2106.12052, 2021.

[^31]: Mark Boss, Raphael Braun, V. Jampani, Jonathan T. Barron, Ce Liu, and Hendrik P. A. Lensch. Nerd: Neural reflectance decomposition from image collections. 2021 IEEE/CVF International Conference on Computer Vision (ICCV), pages 12664–12674, 2020.

[^32]: Jonathan T. Barron, Ben Mildenhall, Matthew Tancik, Peter Hedman, Ricardo Martin-Brualla, and Pratul P. Srinivasan. Mip-nerf: A multiscale representation for anti-aliasing neural radiance fields. 2021 IEEE/CVF International Conference on Computer Vision (ICCV), pages 5835–5844, 2021.

[^33]: Johannes L. Schönberger and Jan-Michael Frahm. Structure-from-motion revisited. 2016 IEEE Conference on Computer Vision and Pattern Recognition (CVPR), pages 4104–4113, 2016.

[^34]: Thomas Müller, Alex Evans, Christoph Schied, and Alexander Keller. Instant neural graphics primitives with a multiresolution hash encoding. ACM Transactions on Graphics (TOG), 41:1 – 15, 2022.

[^35]: Jonathan Tremblay, Thang To, Balakumar Sundaralingam, Yu Xiang, Dieter Fox, and Stan Birchfield. Deep object pose estimation for semantic robotic grasping of household objects. ArXiv, abs/1809.10790, 2018.