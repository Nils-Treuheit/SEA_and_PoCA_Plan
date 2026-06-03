---
title: "Automating synthetic dataset generation for image-based 3D detection: a literature review - Artificial Intelligence Review"
source: "https://link.springer.com/article/10.1007/s10462-025-11431-3"
author:
  - "[[Paul Schulz]]"
  - "[[Thorsten Hempel]]"
  - "[[Magnus Jung]]"
  - "[[Ayoub Al-Hamadi]]"
published: 2025-11-13
journal: "**Artificial Intelligence Review 2026** [[paper]](https://doi.org/10.1007/s10462-025-11431-3)"
description: "A comprehensive review of synthetic dataset generation approaches for 3D object detection, covering both traditional 3D modeling and neural image synthesis methods, along with techniques to address the Sim-to-Real domain gap."
tags: [Publication, Journal Paper, Artificial Intelligence Review, 3D Detection, Synthetic Data]
---
## Abstract

Reliable 3D detection is fundamental to autonomous systems such as mobile robots, self-driving cars, and unmanned aerial vehicles (UAVs). To achieve this capability, researchers have developed and trained supervised networks, which require large amounts of diverse and precisely annotated data. Due to the complex, expensive, and time-consuming capturing and annotation process, synthetic dataset generation approaches have gained popularity over the last decade. With increasing computational resources and advances in simulation technologies, a variety of dataset generators have emerged. These methods rely on either traditional 3D modeling or neural image synthesis to generate data for specific scenarios or general-purpose 3D detection tasks. Their primary goal is to produce high-quality, annotated 3D datasets in an automated and scalable manner. In this review, we evaluate the extent to which state-of-the-art approaches fulfill this goal by introducing a categorization scheme and conducting a comprehensive analysis of both 3D modeling and neural synthesis methods. Our analysis includes techniques used to address the Sim-to-Real domain gap. Furthermore, we assess each method’s level of automation, prerequisites, and practical adoption. This review aims to guide the reader in selecting automated dataset generation workflows for specific detection problems. By considering dataset quality, prerequisites, and application scenarios, we offer practical insights into identifying suitable methods for diverse downstream tasks.

## 1 Introduction

Reliably detecting 3D objects is a fundamental skill for autonomous systems to perform low-level tasks, such as object grasping, collision free movement or object placing. Beyond that, these abilities form the basis for long-standing milestones, such as natural human-robot collaboration (Strazdas et al. [^99], [^98]), fully self-driving cars (SAE [^92]) or embodied artificial intelligence (Liu et al. [^59]).

State-of-the-art detection approaches facilitate deep neural networks, based on RGB, RGB-D or point cloud input (Drobnitzky et al. [^22]). Due to their supervised learning paradigm, most of these solutions share a common disadvantage: they require large amounts of precisely annotated 3D data. Established benchmark datasets, such as KITTI (Geiger et al. [^30]), SUN RBD-D (Song et al. [^97]), Objectron (Ahmadyan et al. [^1]) and ScanNet (Dai et al. [^17]) demonstrated the large amount of time, money and workforce, required to perform these tasks for large-scale real-world data. Training performant detection models becomes especially challenging in domains where none of these large-scale datasets are available. In such cases, practitioners must either create their own datasets or rely on publicly available data that may not adequately represent the edge cases and conditions specific to their application scenarios. For these reasons, research developed approaches to automate aspects of the data capturing and annotation process (Çalli et al. [^13]; Kaskman et al. [^43]; Wang et al. [^121]; Blomqvist et al. [^8], [^9]; Guo et al. [^34]). However, these solutions require complex equipment (Çalli et al. [^13]; Kaskman et al. [^43]; Wang et al. [^121]) or suffer from scalability limitations (Çalli et al. [^13]; Kaskman et al. [^43]; Wang et al. [^121]; Blomqvist et al. [^8], [^9]; Guo et al. [^34]). Furthermore, real-world datasets may suffer from biases, lack of diversity, or insufficient coverage of rare edge cases.

Fueled by increasing computational power and progress in the fields of photorealistic rendering, game development, physics simulation and generative AI, various publications proposed methods to create image-based dataset with synthetic approaches. These dataset generators offer a scalable and flexible alternative by enabling automated annotation and the ability to systematically control scene parameters, such as object placement, lighting, and environmental conditions. They offer an efficient solution to create datasets even for specialized scenarios, for example industrial or medical applications. In general, synthetic dataset generation can be distinguished in 3D modeling and Neural Image Synthesis (NIS) approaches. 3D-modeling approaches utilize 3D content creation frameworks such as Blender (Blender Institute [^7]) as baseline, while NIS approaches incorporate generative models, such as controllable diffusion networks (Zhang et al. [^134]).

In the following, we introduce key concepts for creating synthetic datasets with both methods and present strategies to bridge the Simulation-to-Reality (Sim2Real) gap, which remains a primary challenge of synthetic dataset generation. Subsequently, we outline the scope and methodological structure of our review.

### 1.1 3D modeling approaches

3D Modeling approaches rely on 3D content creation and rendering software to generate synthetic datasets. They usually apply a two-phase workflow, which is displayed in Fig. [1](https://link.springer.com/article/10.1007/s10462-025-11431-3#Fig1). The first step for generating synthetic datasets is scene composition, which creates virtual 3D scenes by applying 3D content creation software, including Game Engines (Unity Technologies [^116]; Epic Games [^23]) and 3D modeling software (Blender Institute [^7]). To create these scenes, established approaches utilize 3D assets, arranged in a virtual environment.

This environment includes the target object which should be detected in the first place. Furthermore, it usually includes light sources and virtual cameras. After scene composition, the workflow finishes with rendering and automated annotation. During this process, images are captured by the previously defined virtual cameras. Furthermore, the target objects are annotated in an automated manner. This step is possible, since all extrinsic and intrinsic parameters are precisely defined. By applying world-to-image transformations, one can project 3D positions into the image domain, and therefore annotate images without human effort (McCormac et al. [^68]).

**Fig. 1**

![Fig. 1](https://media.springernature.com/lw685/springer-static/image/art%3A10.1007%2Fs10462-025-11431-3/MediaObjects/10462_2025_11431_Fig1_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1007/s10462-025-11431-3/figures/1)

Workflow for generating synthetic datasets with 3D modeling: The first step is scene composition, which creates a virtual 3D scene, including target objects, additional 3D assets, virtual cameras and light sources. After scene composition, the workflow finishes with rendering and automated annotation. During this process, images are captured by the previously defined virtual cameras

### 1.2 Neural image synthesis approaches

NIS approaches utilize neural networks to generate photorealistic images alongside 3D annotations. Most methods in this category rely on real-world imagery as input to generate synthetic data that preserves the statistical and geometric properties of the source domain while providing 3D annotations. A prominent application scenario for this paradigm is autonomous driving, where recent studies aim to expand real-world datasets by learning how to generate similar synthetic data. The generated data typically includes controlled variations of the inference input or training images, such as out-of-distribution examples, changes in lighting and texture, or novel object arrangements (Lin et al. [^55]; Gao et al. [^27], [^28]).

While the specific techniques vary, recent works follow a common two-phase workflow, illustrated in Fig. [2](https://link.springer.com/article/10.1007/s10462-025-11431-3#Fig2). The first phase involves model construction and training, followed by a second phase where the trained model is used for inference-based dataset generation.

To implement such pipelines, the literature predominantly adopts two paradigms: radiance field methods and diffusion methods. Radiance field methods—such as Neural Radiance Fields (NeRF) (Mildenhall et al. [^71]) and Gaussian Splats (Kerbl et al. [^45])—learn to reconstruct a 3D scene from 2D images by jointly optimizing a volumetric model and its view-dependent appearance. These radiance fields can be modified and composed, for example, by merging separate fields representing background environments and target objects within a specific domain, such as autonomous driving. Diffusion methods, on the other hand, have seen significant advances with the development of controllable image and video generation models, such as ControlNet (Zhang et al. [^134]). These approaches leverage latent diffusion models (Rombach et al. [^89]) instead of traditional 3D modeling and rendering pipelines. Typically, these models are fine-tuned on specific domains and conditioned on control signals that define the spatial layout of the scene—for example, the 3D positions of objects. The control signal acts as a structural blueprint, which the generative model fills in with realistic content, producing synthetic data accompanied by suitable annotations (Gao et al. [^27], [^28]).

**Fig. 2**

![Fig. 2](https://media.springernature.com/lw685/springer-static/image/art%3A10.1007%2Fs10462-025-11431-3/MediaObjects/10462_2025_11431_Fig2_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1007/s10462-025-11431-3/figures/2)

Workflow for generating synthetic datasets with NIS methods: The first step is model construction and training. During this process, a NIS model is created—usually based on radiance fields or generative models. Once this process is complete, the trained model can be utilized with control signals during inference to create monocular data with corresponding annotations

### 1.3 Simulation-to-reality gap

Automating the annotation process is a key advantage of synthetic dataset generation. This features significantly reduces the labor intensity and complexity of dataset generation compared to real-world datasets. Furthermore, synthetic dataset generation enables the composition of virtual scenes without the need to craft or capture complex real-world scenarios, thus further simplifying the dataset generation process.

Despite the strength of synthetic dataset generation, there is one major weakness, when applying this approach: Object detectors trained on synthetic data often experience a performance gap, when applied in the real world. This performance gap is caused by the simulation to reality gap (Sim2Real gap), which can be divided into two categories: the appearance gap and the content gap (Kar et al. [^42]; NVIDIA [^75]). The appearance gap refers to differences in detailed, pixel-level scene appearances. Eventually, it is caused by a lack of realism regarding light conditions, textures, and colors. Furthermore, synthetic data does not include real-world noise, such as motion blur (Kar et al. [^42]; NVIDIA [^75]). The second source of the Sim2Real gap is often referred to as the content gap. The content gap is related to the elements and arrangement of simulated scenes. It arises because synthetic data might not match the variety of objects and constellations found in the real world (Kar et al. [^42]; Prakash et al. [^78]; NVIDIA [^75]).

Since the Sim2Real gap is the primary weakness of synthetic datasets, recent publications emphasize approaches to reduce its impact. Many of these methods generate datasets by incorporating plausible object positioning, realistic scene elements, and appropriate scene complexity (Roberts and Paczan [^88]; Garcia-Garcia et al. [^29]; Gaidon et al. [^26]; Richter et al. [^87]; Sun et al. [^100]). Additionally, there are techniques to further reduce the Sim2Gap, which will be introduced in the following.

**Photorealistic Rendering:** A straightforward approach to reduce the domain gap is synthesizing photorealistic scene images. This technique minimizes the appearance gap by enhancing realism, particularly through accurate modeling of light, textures, and their complex, view-dependent interactions (Bai et al. [^4]; Denninger et al. [^18]; Akar et al. [^2]; Hodan et al. [^37]). Note that providing this photorealism is a computationally intensive task, which can lead to increased rendering times.

**Domain Randomization:** Domain randomization reduces the content gap by alternating the texture of target objects and by randomizing the 3D environment. Texture randomization forces the object detector to learn the shape of a target instead of its appearance. Furthermore, environment randomization enables the model to generalize across a wide range of environments. Alternating the environment includes varying light conditions, adding additional objects (distractors), randomizing object placement, and varying the number of objects (Prakash et al. [^78]). However, domain randomization should be applied with caution, as overly randomized scenes might eliminate essential scene semantics, which could otherwise help the model learn the detection task effectively (Akar et al. [^2]; Prakash et al. [^78]).

To address this issue, Prakash et al. ([^78]) introduced structured domain randomization, which preserves essential scene semantics while randomizing the scene in realistic and plausible ways.

**Domain Adaptation:** Domain adaptation addresses the Sim2Real gap by modifying synthetic data to align its distribution more closely with real-world data. Approaches include discrepancy-based methods, which minimize measurable distances in feature space, adversarial learning-based methods, which use a generator and discriminator to align distributions, and self-supervised methods (Wang et al. [^119]), which introduce auxiliary tasks to enhance cross-domain learning. For a detailed exploration of domain adaptation methodologies, we refer readers to Wang and Deng ([^117]), who provide a comprehensive review of this field.

### 1.4 Review objective and methodology

The primary motivation behind synthetic dataset generation is to create annotated datasets "for free" by automating the annotation process. However, we argue that only a dataset generation pipeline, which automates all aspects of 3D-modeling or NIS approaches, truly meets this objective.

Building on this paradigm, various publications have proposed methods to address automated dataset generation for both branches. These solutions focus on various application scenarios and apply different levels of automation. Furthermore, they come with varying prerequisites and measurements to approach the Sim2Real gap. From a practitioner’s perspective, aiming to create datasets with minimal effort, there is currently no comprehensive review of these synthetic automated dataset generation methods. Furthermore, no existing review highlights the latest developments in this rapidly evolving field. In this publication, we address this gap by categorizing and analyzing relevant approaches. First, we introduce an application scenario-related categorization scheme. Subsequently, we provide an in-depth analysis of these approaches by evaluating their automation levels, measurements to approach the Sim2Real gap, and acceptance within the branches of our scheme. In the end, we discuss and summarize our results and outline key development trends observed in recent approaches.

As stated previously, this review focuses on synthetic dataset generation approaches for supervised, image-based 3D detection. However, it is important to further clarify the scope of our work, as there are multiple synonyms, related research directions and restrictions to consider. Foremost, image-based 3D detection includes techniques often referred to as 6D pose estimation (Rad and Lepetit [^82]; Tremblay et al. [^113]) or monocular 3D object detection (Liu et al. [^57]; Huang et al. [^40]) These approaches operate on instance-level and class-level. Instance-level approaches estimate the pose of a single target object within the input domain, while class-level approaches can detect objects of one or more classes (Lee et al. [^51]; Manhardt et al. [^63]). We include all publications that address automated dataset generation within these areas and will refer to them collectively as 3D detection.

In addition to the image-based aspect, the supervised learning paradigm is the second focus of our review. Therefore, we only consider dataset generation methods, which incorporate suitable annotations, including 3D bounding boxes and 6D poses.

This limitation excludes related methods, which provide unsuitable annotations, for example semantic masks (Mata et al. [^67]; Pugh et al. [^80]) or 2D Bounding Boxes (Akar et al. [^2]).

Last of all, 3D detection methods typically address either rigid objects or deformable instances. Detecting deformable instances, such as human heads (Hempel et al. [^35]) or full bodies (Yang et al. [^130]), introduces additional challenges due to non-rigid shape changes and complex articulation, which in turn require specialized detection architectures and annotation schemes (e.g., joint keypoints or mesh parameters). While we recognize that many recent synthetic-data generators can handle deformable entities and that including these would broaden the applicability of our review, we have chosen to focus here on rigid object detection pipelines. This narrower scope allows us to provide a more in-depth analysis of automated synthetic-data workflows for rigid targets, while leaving the integration of deformable object generators as an important direction for future work.

In summary, we consider all synthetic dataset automation approaches that can be utilized for supervised 3D detection tasks of rigid objects.

## 2 Related works

Over the last years, multiple reviews have categorized and analyzed 3D detection methods. Most of these works focus primarily on the detection process itself. They categorize approaches according to different domains (Mao et al. [^64]; Doruk et al. [^20]) or input modalities (Wang et al. [^120]; Kim and Hwang [^46]). However, while these works discuss dataset generation, they do not consider it a central focus. For instance, Mao et al. ([^64]) briefly discuss dataset generation in the context of autonomous driving, describing simulation frameworks and methods to reduce the domain gap. Doruk et al. ([^20]), on the other hand, evaluate different pose estimation approaches and synthetic datasets for textureless industrial objects. In contrast, Wang et al. ([^120]) focus on the RGB-D domain, and briefly discuss popular RGB-D datasets. Lastly, Kim and Hwang ([^46]) categorize methods for 3D detection, but also summarize the most popular benchmark datasets.

Multiple reviews specifically address dataset generation for 3D detection. For example, Teufel et al. ([^106]) review prominent datasets in the autonomous driving domain, covering both synthetic and real-world generation techniques. Paulin and Ivasic-Kos ([^77]) propose a categorization framework for synthetic dataset generation across various computer vision tasks and domains. Furthermore, the authors provide best practices for practitioners, aiming to create high-quality synthetic datasets for custom tasks. Drobnitzky et al. ([^22]) categorize real-world datasets for autonomous driving and indoor environments, but do not address synthetic dataset generation. Similarly, Wang et al. ([^120]) provide an overview of real-world RGB-D datasets, detailing their acquisition setups and annotation pipelines. Firman ([^25]) presented a roadmap for RGB-D dataset creation in 2016, reviewing past developments, summarizing the current state, and outlining future directions across domains such as indoor perception and autonomous driving. Recently, Schieber et al. ([^93]) provided a systematic literature review of synthetic dataset generation for indoor 3D detection.

NIS approaches for synthetic dataset generation are a recently emerging family of methods. As a result, there are a few reviews addressing aspects of these approaches. For instance, Xiang et al. ([^127]) focus on 3D-aware diffusion models for image generation, which are essential for NIS pipelines that rely on control signals to structure the scene. Finally, Yang et al. ([^131]) categorize applications of AI-generated imagery, including synthetic dataset generation for 3D detection in autonomous driving and robotics.

In summary, previous reviews have mainly concentrated on specific domains (Mao et al. [^64]; Teufel et al. [^106]), input modalities (Drobnitzky et al. [^22]; Wang et al. [^120]; Firman [^25]), or application areas (Doruk et al. [^20]). To the best of our knowledge, this work is the first to explicitly examine the automation progress in synthetic 3D dataset generation.

While Schieber et al. ([^93]) also investigate synthetic dataset generation for indoor detection, their focus lies in method categorization. In contrast, our review emphasizes automation and extends the scope to include outdoor scenarios. Among all related reviews, the work of Paulin and Ivasic-Kos ([^77]) aligns most closely with our study. The authors address synthetic dataset generation, including monocular detection, and empathize the level of automation during their analysis. However, they do not propose a task-related categorization scheme, nor do they treat automation as an explicit focus. Moreover, our review covers the latest developments in NIS approaches, including radiance fields and diffusion models (Table [1](https://link.springer.com/article/10.1007/s10462-025-11431-3#Tab1)).

**Table 1 Comparison of existing reviews and their focus on Synthetic Dataset Generation (SDG), respective domain, and key notices**

## 3 Categorization of 3D modeling methods

Various publications proposed methods which follow the 3D-model-based workflow, depicted in Fig. [1](https://link.springer.com/article/10.1007/s10462-025-11431-3#Fig1) to perform synthetic dataset generation. The annotation and rendering process of these works is automated by default. Therefore, most methods that automate synthetic dataset generation focus on scene composition. There are two primary approaches to implement this concept:

1. *Domain Specific Modeling*: Automatically creating constrained scenes that resemble the real world.
2. *Domain Agnostic Modeling*: Automatically generating unconstrained scenes that represent arbitrarily complex detection scenarios.

To resemble the real world, methods from the first approach model an environment with an appearance and rule set similar to the real world. These methods reduce the domain gap, by approaching the content gap, but do not necessarily incorporate additional techniques to further reduce the domain gap. In contrast to the first approach, automatically generating unconstrained scenes follows a similar intuition as domain randomization. Publications in this field assume that generating a sufficient number of randomized scenes will cover the application scenario.

In the first section, we introduce a typical workflow for creating these generators in both application scenarios. In the subsequent sections, we focus on specific scenarios by introducing and categorizing dataset generators for multiple specific applications. Afterward, we introduce agnostic dataset generators and categorize them as well.

### 3.1 Dataset generators

Over the last decade, researchers developed dataset generators for both specific and agnostic scene composition. Figure [3](https://link.springer.com/article/10.1007/s10462-025-11431-3#Fig3) displays the general workflow behind this process. 3D graphics pipelines, like the game engines UnrealEngine (Epic Games [^23]) and Unity (Unity Technologies [^116]) or the 3D modeling pipeline Blender (Blender Institute [^7]) are the foundation to develop generators for both approaches. Furthermore, multiple framework extensions were developed, such as BlenderProc (Denninger et al. [^18], [^19]), UnrealCV (Qiu and Yuille [^81]), Sim4CV (Müller et al. [^73]) and Unity perception (Borkman et al. [^10]) which directly provide functionalities for dataset generation. Other methods, utilize features, provided by simulation platforms, such as the autonomous driving simulator CARLA (Dosovitskiy et al. [^21]). By utilizing the 3D modeling-, physics-simulation-, and rendering-capabilities, provided by these pipelines, a generator creates domain agnostic and specific 3D scenes.

A specific generator, such as the CARLA-based simulator SHIFT (Sun et al. [^100]), incorporates environmental constraints that limit the composition process to its domain. For instance, in autonomous driving, scenes feature physically plausible driving scenarios within a realistic virtual environment, such as a 3D-modeled city. In contrast, agnostic generators, such as the NVIDIA Deep Learning Dataset Synthesizer (NDDS) (To et al. [^110]), utilize graphics pipelines to randomly sample 3D models of target objects, distractors, light sources, and textures to produce randomized 3D scenes. In the final step, generators use the rendering engine of their graphics pipeline to create images for specific datasets, such as SHIFT (Sun et al. [^100]), or for generic datasets, like Falling Things (Tremblay et al. [^112]).

**Fig. 3**

![Fig. 3](https://media.springernature.com/lw685/springer-static/image/art%3A10.1007%2Fs10462-025-11431-3/MediaObjects/10462_2025_11431_Fig3_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1007/s10462-025-11431-3/figures/3)

Domain specific and agnostic dataset generators: 3D graphics pipelines such as Unreal Engine (Epic Games [^23]) and Blender (Blender Institute [^7]) serve as the foundation for both scene composition approaches. The SHIFT dataset generator, for example, leverages CARLA—which is based on Unreal Engine—to create domain-specific driving scenes incorporating domain shifts. In contrast, the domain-agnostic approach of the BOP Pose Estimation challenge employs the Blender extension BlenderProc (Denninger et al. [^18]) to synthesize scenes for pose estimation of arbitrary objects without domain constraints. By rendering these scenes, generators produce specific datasets like SHIFT (Sun et al. [^100]), or domain-agnostic datasets such as the BOP benchmark (Hodan et al. [^38])

### 3.2 Domain-specific dataset generation

Researchers have developed specific dataset generation methods using world-based and procedural approaches. World-based approaches define plausible camera positions in predefined virtual worlds and render images from these positions. Depending on the detection scenario, these images often incorporate the perspective of an autonomous agent, like a self-driving car or a mobile robot.

Procedural scene composition, on the other hand, generates 3D scenes by applying a set of predefined rules. These rules define the configuration of the scene, enabling the creation of various scenarios, for example medical operation rooms (Barragan et al. [^5]), or industrial bin-picking setups (Li et al. [^52]; Kleeberger et al. [^48]). The 3D scenes, created with this procedural paradigm, are usually on a smaller scale, than the virtual worlds, utilized for world-based approaches.

#### 3.2.1 World-based approaches for outdoor environments

Applying world-based approaches is a popular method for generating synthetic data for 3D detection in outdoor environments, with autonomous driving being one of the most prominent application scenarios. All identified methods in this domain utilize driving simulations to generate synthetic data. These simulations feature interactive 3D environments populated by agents that navigate through and interact with these spaces. Typically, these agents are road-bound ground vehicles equipped with camera and LiDAR sensors.

Early approaches created customized solutions by directly programming game engines. Researchers provided virtual 3D worlds, such as 3D city models, and defined virtual vehicles along with their interactions within these worlds using game engines. The creators of SYNTHIA (Ros et al. [^91]) followed this workflow, by crafting virtual cities and applying the game engine Unity for vehicle definition and scene interaction. Instead of crafting a game-like simulation, Richter et al. ([^87]) directly utilized the open world game GTA V for scene composition in VIPER. Their dataset generator relied on the 3D assets and physics of GTA. While game content can be used for non-commercial usage, the internal process of the game engines are inaccessible. Therefore, the authors extracted rendering commands with the help of detouring middleware (Richter et al. [^87]).

While SYNTHIA and VIPER relied on custom-built simulators, most modern approaches utilize general-purpose driving simulators such as CARLA (Dosovitskiy et al. [^21]). In addition to supporting tasks like autonomous agent training, hardware simulation, and traffic modeling, these frameworks are also widely used for synthetic data generation. To enable this functionality, dataset generation methods must incorporate a dedicated controller that manages the data pipeline. This controller is responsible for capturing, annotating, and storing data generated by simulated sensors during runtime. This paradigm has been adopted by several CARLA-based approaches to create dataset generators for roadside detection (Barra et al. [^6]; Carta et al. [^14]), adverse environmental conditions (Sural et al. [^101]), driving safety (Muller et al. [^74]), multi-camera fusion (Mehr and Eskandarian [^69]), and aerial/drone detection (Meier et al. [^70]).

In addition to CARLA, AirSim (Shah et al. [^94]) is another general-purpose simulator used to develop dataset generators for outdoor detection. Similar to CARLA, AirSim provides environment and agent models, while dataset generators are responsible for implementing a controller that operates the agents and captures synthetic data. Following this workflow, Hu et al. ([^39]) introduced a generator for creating an outdoor dataset for 3D detection from a drone’s perspective.

In 2024, Zhou et al. ([^135]) and Türkcan et al. ([^114]) introduced autonomous driving simulators that directly incorporate controller functionalities into their software stacks. As a result, these solutions can be used for dataset generation out of the box, without requiring separate controller implementation.

#### 3.2.2 World-based approaches for indoor environments

Creating generators for reliable indoor detection is the second application area for world-based approaches. Detecting objects in indoor 3D worlds such as domestic-, industrial- or retail-environments is especially relevant for mobile robotics. High-level tasks in this domain, such as pick-and-place operations or obstacle navigation, require reliable 3D detection. Due to this reason, most dataset generators are designed for indoor detection from mobile robot perspectives. Recent publications utilized two approaches to create these methods, which will be introduced in the following.

##### 3.2.2.1 Position-based capturing

Providing suitable indoor worlds and rendering images from plausible positions is the first approach to creating dataset for indoor detection. We refer to this technique as position-based rendering.

The generator of the Hypersim dataset (Roberts and Paczan [^88]) applied position-based capturing to high quality 3D scenes, obtained from an online marked place. The authors ensured plausible perspectives, by restraining the camera position to non-occupied points in 3D with a minimum distance to the closest 3D asset. (Li et al. [^56]) applied a similar concept to create the Structured3D dataset. Both approaches sample images, in predefined 3D assets, limiting the scene diversity to fixed environments. Recently, Behavior Vision Suite (BVS) (Ge et al. [^31]) overcame this limitation, by providing unlimited variations of baseline indoor environments.

##### 3.2.2.2 Robotics simulation

The second approach for creating synthetic indoor data is utilizing robotics simulators. Similar to autonomous driving, methods of this category apply simulations that offer comprehensive 3D environments inhabited by robotic agents. However, mobile robotics involves a high diversity of actors compared to autonomous driving, due to the wide variety of robot types, including humanoid robots and logistics transport robots (Martinez-Gonzalez et al. [^65]).

In UnrealROX (Martinez-Gonzalez et al. [^65]), and UnrealROX++(Martinez-Gonzalez et al. [^66]), Martinez-Gonzalez et al. introduced and expanded a dataset generator designed for humanoid robots operating in domestic environments. The generator utilizes open-source 3D scenes provided by the Unreal Engine community. The key contribution of their work lies in the control system for humanoid robots. Using virtual reality (VR) equipment, a human operator directly controls the humanoid robot. Barragan et al. ([^5]) applied a similar approach by, recording surgical operations with a teleoperation device in a virtual operation room. They utilize the recorded trajectories to create datasets for the 3D detection of surgical instruments.

To create the THUD dataset (Tang et al. [^105]), developed a dataset generator for mobile Robots in human-centered environments, based on Unity. Their dataset includes various indoor scenes, such as offices, supermarkets, and domestic homes.

Developing and testing robotics applications in simulation environments such as iGibson(Xia et al. [^126]; Li et al. [^60]) and AI2-THOR(Kolve et al. [^50]) has been a standard practice for several years. However, due to the real-time requirements necessary for testing robotic applications, these simulators often compromise on photorealism. Fueled by advancements in rendering- and physics simulation-capabilities, simulation platforms like NVIDIA’s Omniverse ([^76]) have emerged.

These platforms support large-scale physics simulation 3D and photorealistic rendering via ray tracing. Based on Omniverse, researchers proposed an expansion of iGibson for computer vision tasks, called OmniGibson (Li et al. [^61]), which can be utilized as a dataset generator for indoor environments.

#### 3.2.3 World-based approaches for flexible environments

Previously, we introduced dataset generators tailored for either indoor or outdoor environments, with indoor solutions further classified into position- and simulation-based approaches. However, certain dataset generators merge these functionalities to create flexible pipelines. We already introduced Omniverse as a framework for physics simulation and rendering. In IsaacSim (NVIDIA [^75]), it is utilized to create a highly flexible dataset generator, which involves all branches of our categorization scheme. The world-based branch, (IsaacSim-world) supports, position-based rendering and robotics simulation in industrial environments. Therefore, scenes can be rendered from fixed points within the environment and from the robot’s point of view.

With Infinte Worlds (Raistrick et al. [^84], [^83]), Raistrick et al. developed a dataset generator, capable of creating 3D environments from scratch. The generators achieve this by applying predefined rules to a large collection of 3D assets. In addition, the system uses position-based rendering to generate images from plausible viewpoints. The generated 3D worlds include both indoor and outdoor scenes, classifying this dataset generator as the second flexible approach suitable for object detection tasks in both environments.

#### 3.2.4 Procedural approaches

We introduced procedural approaches as the second method for creating domain-specific datasets, alongside world-based solutions. In contrast to world-based generators, procedural approaches, do not involve large indoor and outdoor worlds. These methods compose isolated 3D scenes, by applying predefined rules, making them particularly valuable for edge-case scenarios where no suitable 3D worlds are available. Procedural approaches can be further categorized into virtual and hybrid methods.

##### 3.2.4.1 Virtual methods

Anagnostopoulou et al. ([^3]) applied procedural scene composition to generate datasets for mushroom detection. Using Blender, they created deformed ground models and added mushrooms with randomized numbers, sizes, shapes, and positions. Other procedural approaches have been applied to generate datasets for industrial bin picking (Li et al. [^52]; Kleeberger et al. [^48]). These methods utilized Blender’s physics engine to simulate objects falling into virtual bin-picking scenes. Furthermore, Filipovic et al. ([^24]) developed a procedural dataset generator by crafting a CAD model of a supermarket shelf and simulating the placement of tea boxes to estimate their 6D positions. Lindermayr et al. ([^54]) utilized a similar approach for multiple classes of retail objects. In the context of autonomous driving, Synscapes (Wrenninge and Unger [^123]) and VALERIE22 (Grau and Hagn [^32]) employed Blender to procedurally compose driving scenes with varied layouts and environmental conditions.

##### 3.2.4.2 Hybrid methods

Creating realistic 3D environments is a complex task that often requires detailed modeling of the background. To simplify this, hybrid approaches model only the target object and project it onto real-world background images. These methods use 3D modeling software to render the target under varying environmental conditions, such as different positions, orientations, and lighting setups.

Hybrid approaches have been widely used in spacecraft detection. To perform this task, Proença and Gao ([^79]) developed the dataset generator URSO, based on UnrealCV. The authors combined a 3D earth-model with different 3D spacecraft models. Furthermore, they reproduce lighting conditions, in space, including sunlight and Earth albedo. Multiple publications applied a render and paste approach, by rendering spacecrafts, rockets and satellites under different illuminations in Blender and projecting them on celestial background images (Sharma et al. [^95]; Tran et al. [^111]; Tong et al. [^109]; Liu et al. [^58]). Tang et al. ([^104]) implemented a similar method for submarine detection.

### 3.3 Domain-agnostic dataset generation

We introduced Domain-Agnostic Dataset Generators as pipelines that utilize domain-randomized scene composition. By rendering images with varying backgrounds, lighting conditions, and additional randomized scene elements, these pipelines can create datasets for arbitrary detection tasks. In order to apply this concept, all agnostic solutions require 3D models of target objects. Since many real-world datasets (Tyree et al. [^115]; Hodan et al. [^36]; Kaskman et al. [^43]) have similar requirements, various high-quality 3D models are available. For example, the T-LESS dataset features textureless 3D models of industrial objects (Hodan et al. [^36]), while the HomebrewedDB dataset includes 3D models of arbitrary objects such as toys and household items (Kaskman et al. [^43]). Similarly, the HOPE dataset provides 3D models of toy objects resembling household items (Tyree et al. [^115]). These 3D models often serve as target objects for domain-agnostic dataset generators.

#### 3.3.1 Rasterization-based methods

Early approaches prioritized speed and scalability, often at the expense of photorealism (Denninger et al. [^18]). As a result, these methods frequently utilized rasterization techniques. For instance, Kehl et al. ([^44]) employed an OpenGL-based rendering approach, leveraging rasterization and a "render-and-paste" paradigm to create 3D object detection datasets. Based on UnrealEngine 4, NDDS (To et al. [^110]) also relies on rasterization but utilizes more sophisticated scene composition. In this generator, 3D models of target objects and distractors were subjected to gravity simulation to create realistic spatial arrangements. Virtual cameras were placed within these scenes to capture images from various perspectives.

#### 3.3.2 Ray tracing-based methods

Recent publications have further developed the concept of physics-based scene composition, introduced in NDDS, by incorporating path/ray-tracing techniques instead of rasterization (Hodan et al. [^38]; Morrical et al. [^72]; Greff et al. [^33]).

Applying Blender’s cycles ray tracing engine, combined with physics-based scene composition, is a popular approach to implement this concept. For instance, Hodan et al. ([^38]) created a BlenderProc-based dataset generator, which incorporates both aspects, to produce datasets for the Benchmark for 6D Object Pose Estimation Challenge (BOP-Challenge). Furthermore, Remus et al. ([^85]) proposed a similar BlenderProc approach to train their class-level 3D detector.

Kubric (Greff et al. [^33]) is another dataset generator based on Blender and cycles. It is very similar to solutions, based on BlenderProc. In contrast to the Blender-based approaches, NViSII has adopted NVIDIA’s OptiX ray-tracing engine to implement the rendering process (Morrical et al. [^72]).

## 4 Categorization of neural image synthesis methods

We introduced NIS approaches as methods which can be utilized to extend datasets with synthetic images or to create new synthetic datasets. Furthermore, we presented two approaches for creating datasets. These approaches typically utilize radiance fields or generative models and follow the workflow illustrated in Fig. [2](https://link.springer.com/article/10.1007/s10462-025-11431-3#Fig2). In contrast to the 3D modeling methods discussed in the previous section, this workflow does not involve explicit scene composition. Instead, these methods build and train an NIS model, which is applied during inference to generate images along with suitable annotations. As a result, the categorization scheme used for model-based methods is not directly applicable to NIS approaches.

### 4.1 Diffusion approaches

We introduced diffusion approaches as methods that employ generative diffusion models, to produce photorealistic imagery based on an input prompt. This paradigm has been successfully applied to various 2D computer vision tasks, including object detection (Chen et al. [^16]), semantic segmentation (Xu et al. [^129]; Simoni and Pelosin [^96]; Wu et al. [^125]), and depth estimation (Xu et al. [^129]; Simoni and Pelosin [^96]; Wu et al. [^125]). In the 2D domain, these models do not require an understanding of 3D scene arrangements. However, this changes when the detection task is extended to the 3D domain. As a result, all approaches in this area incorporate some form of 3D control signal, even though they may differ in how they train and apply the diffusion model before and during dataset generation.

To the best of our knowledge, diffusion-based approaches for dataset expansion have been explored exclusively within the autonomous driving domain. These methods typically augment existing driving datasets by generating rare or edge-case scenarios, thereby enhancing the scale and diversity of the original data. In gereral, such approaches either fine-tune a diffusion model on real-world driving data or incorporate this data during inference to produce controlled variations of driving scenes.

MagicDrive (Gao et al. [^27]) proposes a latent-diffusion generator that fuses explicit 3-D geometry controls (road maps, 3-D object bounding boxes, and camera poses) with text prompts. The NIS model is trained on a subset of NuScenes street-view images paired with corresponding control signals. During inference, MagicDRive produces realistic driving images that respect in the style of NuScenes, while following the input control signal and text prompts. Following MagicDrive, MagicDrive-V2 (Gao et al. [^28]) and DriveDreamer (Wang et al. [^122]) expand this capability to video generation, by leveraging transformer architectures. Furthermore, DrivingDiffusion (Li et al. [^62]) proposes a dataset generator for multi-view video generation. The method combines a multi-view module that synthesizes aligned key-frames from a detailed 3D scene layout, and a temporal module that expands these frames to coherent driving videos. In contrast to previous the methods, DriveGEN (Lin et al. [^55]) introduces a training-free, controllable text-to-image pipeline. By extracting self-prototypes from an input image, this method generarates to create domain-shifted variants, such as different weather or lighting conditions.

### 4.2 Radiance field approaches

We introduced Radiance Fields as the second approach to construct NIS models for 3D dataset generation. Methods of this category utilize neural rendering (NeRF or Gaussian Splats) to reconstruct neural scene representations from 2D imagery, usually by learning separate foreground and background models. These models are combined and modified to synthesize alternative 3D scene layouts. Although several works have adopted this workflow to build simulation environments for autonomous driving (Wu et al. [^124]; Tonderski et al. [^107]) and mobile robotics (Xie et al. [^128]), we restrict our review to approaches that deliver the annotations required for monocular 3D detection. This criterion limits the Radiance Field approaches to two methods (Tong et al. [^108]; Zanjani et al. [^133]), we will introduce in the following.

Tong et al. ([^108]) and Zanjani et al. ([^133]) both focus the autonomous driving domain. Furthermore, they leverage annotated datasets that provide driving images, calibrated camera poses, and 3D bounding boxes. Consequently, both approaches are more suitable for augmenting existing datasets, rather than generating new data.

Tong et al. ([^108]) generate synthetic driving data by training and augmenting NeRF models of real driving scenes from Waymo and NuScenes. To archive this, the authors decompose driving scenes in background and foreground via segmentation and train NeRF’s for both. In the next step, they create new 3D representations by alternating object locations, backgrounds and light conditions. Finally, they render images from camera positions in the composed scene to generate synthetic data.

By utilizing an augmentable, instance aware Gaussian Splatting model, trained on the nuScenes dataset, Zanjani et al. ([^133]), applied a similar approach for generating Radiance Fields. However, Gaussian Splatting offers a more computationally efficient alternative to NeRFs, enabling faster rendering and training.

## 5 Analysis of 3D modeling approaches

In the following, we analyze the dataset generators, based on 3D modeling we introduced in the sect. [3](https://link.springer.com/article/10.1007/s10462-025-11431-3#Sec7). To provide a structured overview, we summarize our categorization scheme in Fig. [5](https://link.springer.com/article/10.1007/s10462-025-11431-3#Fig5). Furthermore, we provide an overview of example output for prominent dataset generators in Fig. [4](https://link.springer.com/article/10.1007/s10462-025-11431-3#Fig4). The analysis is divided into three parts: first, we evaluate the level of automation within the branches of our categorization scheme. Next, we examine the measurements, applied to address the Sim2Real gap. Finally, we assess the relevance of all open-source dataset generators, regarding their field of application (Table [2](https://link.springer.com/article/10.1007/s10462-025-11431-3#Tab2)).

**Fig. 4**

![Fig. 4](https://media.springernature.com/lw685/springer-static/image/art%3A10.1007%2Fs10462-025-11431-3/MediaObjects/10462_2025_11431_Fig4_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1007/s10462-025-11431-3/figures/4)

Images generated with prominent 3D modeling dataset generators, introduced in the previous section

**Fig. 5**

![Fig. 5](https://media.springernature.com/lw685/springer-static/image/art%3A10.1007%2Fs10462-025-11431-3/MediaObjects/10462_2025_11431_Fig5_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1007/s10462-025-11431-3/figures/5)

Summarized categorization scheme for the introduced dataset generators. specific approaches: red, agnostic approaches: green

**Table 2 Overview of specific (outdoor, indoor, flexible, procedural) and agnostic dataset generators, categorized by approach, prerequisites (including semantics and 2D), and availability**

### 5.1 Automation progress

We considered all methods, that automate the annotation and rendering processes of Fig. [1](https://link.springer.com/article/10.1007/s10462-025-11431-3#Fig1) in our categorization scheme. Therefore, all methods exhibit a basic level of automation. To further analyze automation progress, it is necessary to evaluate the scene composition process. We aim to perform this evaluation using consistent criteria. However, a key challenge arises: scene composition techniques differ across our scheme because both branches utilize varying methods and have distinct prerequisites, complicating direct comparisons. To address this problem, we define the prerequisites for each dataset generator. Subsequently, we evaluate the dataset generators under the assumption that these prerequisites are met. This allows us to apply a high-level evaluation, which distinguishes between semi- and fully automated approaches. Semi-automated approaches require a human controller, which has to perform different actions, such as controlling robots or vehicles. These actions are incorporated in the scene composition process. Fully automated approaches, on the other hand, do not need such a controller. Table [3](https://link.springer.com/article/10.1007/s10462-025-11431-3#Tab3) summarizes the result of this evaluation and the corresponding prerequisites to apply the dataset generators. Furthermore, it clarifies whether a dataset generator is publicly available. In the following, we evaluate each branch of our scheme in detail.

**Table 3 Overview of specific (outdoor, indoor, flexible, hybrid, virtual) and agnostic dataset generators, categorized by approach, automation level, and prerequisites (including controller)**

#### 5.1.1 World-based approaches

##### 5.1.1.1 Outdoor environments

All world-based approaches for outdoor environments require a complex 3D world model to generate datasets. These 3D worlds are incorporated in game engines to create video game-like simulations with dynamic environmental features, such as traffic flow, seasonal changes, and weather conditions. SYNTHIA (Ros et al. [^91]) provides a Unity-powered city simulation, which incorporates all these elements. In VIPER (Richter et al. [^87]), on the other hand, the 3D world of GTA V is utilized. The other referenced generators create these simulations by combining 3D worlds with build in functionalities (traffic-, pedestrian-simulation etc.). With a 3D world provided, simulators may support both automated and fully automated dataset generation. VIPER (Richter et al. [^87]) is considered semi-automated, as human operators are required to navigate vehicles through the GTA V environment, effectively "playing" the game. Similarly, SYNTHIA (Ros et al. [^91]), requires vehicles to be manually driven through their virtual environments.

We stated previously, that all CARLA-based approaches (Barra et al. [^6]; Carta et al. [^14]; Sural et al. [^101]; Muller et al. [^74]; Mehr and Eskandarian [^69]; Meier et al. [^70]) use dedicated controllers to capture and annotate datasets. The dataset creators implemented these control mechanisms, tailoring them to the specific purpose of each dataset. In addition to setting simulation parameters, such as simulation time, number of iterations, and frame rate—these controllers manage data capture from selected sensors. For example, the controller in (Barra et al. [^6]) samples images from multiple car-mounted cameras to capture roadside scenarios, while the controller in (Muller et al. [^74]) collects images from perspectives that resemble those used in the nuScenes dataset (Caesar et al. [^12]). However, these controllers usually offer a self-driving mode, allowing vehicles to navigate autonomously without human intervention. This capability qualifies CARLA for fully automated scene composition. The remaining simulators, GarchingSim (Zhou et al. [^135]) and Boundless (Türkcan et al. [^114]), were proposed recently and do not provide sufficient information to determine if self-driving modes are available. However, they do not require controller mechanisms, due to their inherent focus on image-based dataset generation.

##### 5.1.1.2 Indoor environments

World-based approaches for indoor environments also rely on 3D worlds; however, the complexity varies between position-based and simulation-based methods. Position-based approaches (Roberts and Paczan [^88]; Li et al. [^56]; Ge et al. [^31]) usually require predefined high-quality 3D assets, such as offices or domestic indoor environments. BVS (Ge et al. [^31]) is particularly noteworthy in context of these prerequisites, as it supports procedural world generation. By applying large-scale procedural scene composition, it can create entirely new 3D worlds, such as houses or offices. In addition to complex 3D environments, simulation-based methods (Martinez-Gonzalez et al. [^65], [^66]; Tang et al. [^105]) demand a physically accurate robot model. Specifically, UnrealRox (Martinez-Gonzalez et al. [^65]) and UnrealRox+ (Martinez-Gonzalez et al. [^66]) utilize humanoid robots, while THUD (Tang et al. [^105]) requires a mobile transport platform.

When their prerequisites are met, indoor approaches can support either semi-automated or fully automated scene composition. As with outdoor environments, semi-automated methods involve human operators, while fully-automated solutions do not require human operation.

Position-based methods (Roberts and Paczan [^88]; Li et al. [^56]; Ge et al. [^31]), achieve fully automated scene composition by automatically generating suitable camera positions within their environment. Simulation-based approaches, on the other hand, facilitate semi-automated scene composition. For instance, UnrealROX (Martinez-Gonzalez et al. [^65]) and UnrealROX+ (Martinez-Gonzalez et al. [^66]) require a human operator to directly control a humanoid robot to generate datasets, making them semi-automated. Similarly, Barragan et al. (Barragan et al. [^5]) require teleoperation equipment to model the trajectory of the surgeon.

##### 5.1.1.3 Flexible environments

Similar to outdoor approaches, the world-based branch of ISAAC (NVIDIA [^75]) creates flexible, game-like simulations to model dynamic environments, therefore requiring realistic 3D worlds as input. As ISAAC is specialized on industrial scenarios, it requires industrial 3D worlds, such as digital twins of warehouses. In contrast to ISAAC, Infinite Worlds (Raistrick et al. [^84], [^83]) applies a procedural paradigm to create indoor and outdoor 3D worlds. By utilizing a precisely defined rule set and a comprehensive database of 3D models, Infinite Worlds generates 3D environments from scratch. Unlike other world-based approaches, Infinite Worlds does not require a pre-existing 3D world as input. When the prerequisites are met, ISAAC (world) and Infinite Worlds enable fully automated scene composition, with camera perspectives, sampled from agent-POV’s or plausible positions within the 3D environment.

#### 5.1.2 Procedural approaches

Due to their procedural paradigm, which creates 3D scenes from predefined assets, most virtual and hybrid approaches do not require a complete 3D world as a prerequisite. The only exception is the virtual methods URSO (Proença and Gao [^79]), which relies on an Earth orbit model to generate datasets. All other procedural approaches require 3D models, including both the target objects and background assets, to compose 3D scenes. Additionally hybrid methods require realistic background imagery, such as celestial images (Sharma et al. [^95]; Tran et al. [^111]; Tong et al. [^109]) or marine environments (Tang et al. [^104]), as they project rendered 3D models on real images. If the corresponding 3D models are provided, all approaches sample 3D assets and virtual cameras in automated fashion without requiring human intervention. Therefore, all procedural methods are capable of fully automated scene composition,

#### 5.1.3 Domain agnostic generators

All agnostic generators rely on 3D models of the target objects as input. These methods also adopt a procedural paradigm, similar to procedural approaches. However, unlike these methods, the scene composition in agnostic generators is unconstrained, focusing on creating domain-randomized datasets. To achieve this, all methods include arbitrary distractor models to enhance domain randomization. Once these requirements are met, all methods perform fully automated scene composition, eliminating the need for human supervision.

### 5.2 Simulation-reality gap

Domain agnostic and specific dataset generators already incorporate certain measures to bridge the Sim-to-Real gap within their respective paradigms. While agnostic generators rely on domain-randomized scene composition, specific generators focus on creating realistic synthetic scenes. To further address the Sim-to-Real gap, dataset generators typically incorporate either photorealistic rendering or domain randomization. Domain adaptation, is less common for 3D modeling approaches, because it is often applied after a dataset has already been generated. For these reasons, we limit our evaluation to photorealistic rendering and domain randomization. Table [4](https://link.springer.com/article/10.1007/s10462-025-11431-3#Tab4) summarizes the results of our evaluation regarding both aspects. In the following sections, we provide a detailed analysis of the main branches of our categorization scheme.

**Table 4 Measurements to bridge the Sim2Real gap, including Photorealism, Domain Randomization (DR) and Structured Domain Randomization (SDR). Furthermore, the applied rendering software is displayed**

**Fig. 6**

![Fig. 6](https://media.springernature.com/lw685/springer-static/image/art%3A10.1007%2Fs10462-025-11431-3/MediaObjects/10462_2025_11431_Fig6_HTML.png?as=webp)

[Full size image](https://link.springer.com/article/10.1007/s10462-025-11431-3/figures/6)

Different degrees of photorealism in specific and agnostic dataset generators. Dataset generators with low photorealism incorporate simple view-dependent effects and basic textures. At the medium level, both aspects are more refined. For a high degree of photorealism, advanced view-dependent effects, such as specular reflections, are included. The images were derived from: GarchingSIM (Zhou et al. [^135]) (**a**), VIPER (Richter et al. [^87]) (**b**), Boundless (Türkcan et al. [^114]) (**c**), SSD-6D (Kehl et al. [^44]) (**d**), NDDS (To et al. [^110]) (**e**) and BP4BOP (Hodan et al. [^38])(**f**)

#### 5.2.1 Photorealistic rendering

Over the last decade, many dataset generators have claimed to utilize photorealistic rendering. However, with advancements in rendering capabilities and hardware resources, the standards for achieving photorealism have significantly increased. For older dataset generators like SYNTHIA, photorealism was characterized by features such as varying weather conditions and realistic shadows. In contrast, recent publications define photorealism with more complex effects, including specular reflections and the accurate modeling of transparent objects.

To account this development, we define photorealism from a modern perspective. Overall, we classify it into three levels: low, medium, and high. Figure [6](https://link.springer.com/article/10.1007/s10462-025-11431-3#Fig6) provides an overview of these levels by showcasing example images from agnostic dataset generators and specific generators for world-based approaches.

##### 5.2.1.1 Specific generators

Specific generators cover all three levels of photorealism, with noticeable clustering across different methods. Most outdoor and robotics approaches are classified as low photorealism, while position-based and procedural methods generally reach high photorealism.

The low and medium photorealism in many outdoor (Rong et al. [^90]; Zhou et al. [^135]; Carta et al. [^14]; Barra et al. [^6]), robotics (Tang et al. [^105]; Martinez-Gonzalez et al. [^65], [^66]) and flexible (Shah et al. [^94]) generators can be attributed to two primary reasons. First, outdoor autonomous driving and robotics simulators often prioritize speed over visual fidelity, to run the 3D environment and its corresponding functionalities with reasonable computational overhead. Additionally, some approaches (Ros et al. [^91]; Martinez-Gonzalez et al. [^65], [^66]) rely on older rendering engines, limiting visual fidelity, even if they prioritize photorealism. In contrast, most recent outdoor and flexible methods (Raistrick et al. [^84], [^83]; NVIDIA [^75]) achieve high photorealism by integrating modern engines and a greater emphasis on fidelity. Note that these methods require considerable computational resources, due to large indoor environments and high definition rendering. Similar to these solutions, most position-based and procedural approaches also prioritize high fidelity and are therefore capable of high photorealism. Similar to modern flexible approaches, position-based methods require huge amounts of computational power.

##### 5.2.1.2 Agnostic generators

Similar to specific solutions, agnostic generators also offer all three levels of photorealism. While older rasterization-based methods achieve low to medium fidelity, newer ray tracing-based solutions deliver high photorealism. This trend directly correlates with the improved rendering capabilities.

#### 5.2.2 Domain randomization

We previously introduced that domain randomization is inherently integrated into the agnostic paradigm, making it a default feature of agnostic generators. However, some specific generators also provide domain randomization capabilities, which will be discussed in the following

Outdoor autonomous driving simulators inherently support structured domain randomization by simulating dynamic yet physically plausible changes, such as weather variations, traffic flow, lighting conditions, and texture updates. Similarly, BVS (Ge et al. [^31]) incorporates structured domain randomization for indoor scenes by utilizing a restricted procedural paradigm for environment generation, combined with Omniverse’s advanced physics simulation. These restrictions ensure that related objects spawn together realistically. Furthermore, Ominverse enhances physical plausibility, particularly for non-rigid objects. Most recent flexible approaches (NVIDIA [^75]) adopt similar functionalities, enabling structured domain randomization for both indoor and outdoor environments. Procedural approaches, on the other hand, achieve structured domain randomization by only applying restricted scene composition. However, scene composition in these methods remains relatively simple compared to large-scale procedural environment generation and high-fidelity physics simulation.

### 5.3 Acceptance

To evaluate the acceptance of the introduced dataset generators, we review two criteria. First, we assess whether the generator has been used to create publicly available datasets. Second, we determine if these datasets were applied to train 3D detection networks. However, we only consider these aspects if the dataset generator itself is publicly accessible. We distinguish between high, medium and low acceptance. High acceptance requires multiple public available datasets and an evaluation on 3D detection tasks. In contrast, medium acceptance requires one public dataset and a corresponding evaluation, while low acceptance indicates that only one or neither criterion has been met.

#### 5.3.1 Domain specific generators

For outdoor world-based approaches, CARLA has been utilized extensively to develop dataset generators. These dataset generators, cover roadside detection (Barra et al. [^6]; Carta et al. [^14]), adverse environmental conditions (Sural et al. [^101]), and driving security (Muller et al. [^74]). More recently, CARLA was also applied to create a dataset generator for drone perspectives (Meier et al. [^70]). However, it is important to note that this widespread acceptance primarily concerns the general concept of using CARLA, rather than the individual dataset generators themselves. This is because most of these generators have not been reused or applied to new dataset generation tasks beyond their original, corresponding datasets. To the best of our knowledge, Boundless (Türkcan et al. [^114]) and GarchingSim (Zhou et al. [^135]) have not been used to produce publicly available datasets, despite offering suitable annotations. Consequently, both approaches exhibit low acceptance. The SYNTHIA and VIPER Dataset generators are not available for the public, therefore, we did not evaluate their acceptance.

Among publicly available indoor approaches, Hypersim (Roberts and Paczan [^88]) and UnrealROX (Martinez-Gonzalez et al. [^65]) exhibit medium acceptance, as both were employed to generate one public dataset. Additionally, both solutions were evaluated for 3D detection tasks. Hypersim was incorporated into the Omni3D dataset (Brazil et al. [^11]), which was comprehensively evaluated for 3D detection. In contrast to that, the creators of UnrealROX only presented a proof of concept for 3D detection. In the domain of flexible approaches, both solutions neither provided public datasets nor underwent evaluation for 3D detection.

For virtual and hybrid procedural approaches, most generators are not publicly accessible, despite frequent evaluation on 3D detection tasks. Therefore, we excluded them from our acceptance assessment. The two publicly available procedural approaches, NOCS (Wang et al. [^118]) and IPA-Bin Picking (Kleeberger et al. [^48]) both provided a public dataset with a corresponding evaluation, indicating medium acceptance.

#### 5.3.2 Domain agnostic generators

Among agnostic approaches, BlenderProc4BOP (Hodan et al. [^38]) and NDDS (To et al. [^110])incorporate high acceptance, as both have been used to generate multiple datasets that were evaluated on 3D detection tasks. Furthermore, NDDS has been applied to a wide range of tasks, including household object detection (Jalal et al. [^41]; Tremblay et al. [^112]; Chen et al. [^15]) and pallet detection (Knitt et al. [^49]) in the following years.

BlenderProc4BOP, in particular, has achieved notable recognition due to its extensive use in the BOP Challenge, a state-of-the-art benchmark for evaluating pose estimation algorithms over several years. The remaining generators demonstrated either medium (SSD-6D, NVISII) or low (Kubric, ISAAC (object)) acceptance (Table [5](https://link.springer.com/article/10.1007/s10462-025-11431-3#Tab5)).

**Table 5 Overview of the prevalence and evaluation status of the introduced dataset generators**

**Table 6 Overview of NIS Automation, categorized by automation level, and prerequisites**

## 6 Analysis of neural image synthesis approaches

To compare NIS methods with the 3D modeling pipelines analyzed in the previous section, we apply the same evaluation criteria. Therefore, we examine automation, measures for narrowing the Sim2Real gap, and overall acceptance. Table [6](https://link.springer.com/article/10.1007/s10462-025-11431-3#Tab6), [7](https://link.springer.com/article/10.1007/s10462-025-11431-3#Tab7), and [8](https://link.springer.com/article/10.1007/s10462-025-11431-3#Tab8) summarize the results of our analysis.

**Table 7 Overview of NIS measurements to address the Sim2Real gap, categorized by Photorealism, SDR and Domain Adaptation (DA)**

**Table 8 Overview of NIS acceptance and evaluation**

### 6.1 Automation progress

Similar to the analysis of 3D model-based methods, we considered all approaches that automate annotation and rendering. Approaches, to construct NIS models differ between radiance field and diffusion methods. Even methods within the same category can vary significantly in their design. To address this, we apply the same technique as previously: we assume all prerequisites are met for the generator to function, and we conduct a high-level evaluation distinguishing between semi- and fully automated approaches.

In the context of NIS models, semi-automated approaches require a human operator to guide the diffusion or radiance field rendering process during inference. Fully automated approaches, on the other hand, do not require human intervention and can generate instances without supervision.

#### 6.1.1 Diffusion-based methods

All diffusion-based dataset generation methods require an annotated dataset to train their models. Additionally, most methods require structured data as input during inference. During inference, all methods also require control signals in the form of 3D scene layouts and text prompts.

When all requirements are fulfilled, all methods support semi-automated dataset generation. They are considered semi-automated because a human operator must provide control signals, such as a scene layout and a text prompt. While text prompt generation can often be automated, creating a suitable scene layout—such as road maps, bounding boxes, and camera poses, as required by methods like DrivingDiffusion (Li et al. [^62]), DriveDreamer (Wang et al. [^122]), and MagicDrive (Gao et al. [^27], [^28]) remains a complex task. In contrast, DriveGEN (Lin et al. [^55]) only requires input images and 3D bounding boxes, making it comparatively easier to automate.

#### 6.1.2 Radiance field approaches

The Radiance Field methods we considered in our categorization (Tong et al. [^108]; Zanjani et al. [^133]) have similar requirements to the diffusion-based methods. To utilize NeRFs or Gaussian Splatting models with awareness of 3D target positions, one must provide a set of images, camera poses, and 3D bounding boxes. All considered methods rely on real-world datasets such as nuScenes to obtain this data. During inference, radiance field methods require control signals to combine and modify the trained fields. Additionally, they require camera trajectories for the synthesized scene in order to render and annotate new images. Because these inputs are provided by a human operator, all radiance field-based methods are classified as semi-automated.

### 6.2 Simulation-reality gap

In the previous section, we analyzed photorealism and domain randomization to assess how 3D modeling methods approach the sim-to-real gap. We did not include domain adaptation, as it is often applied after the dataset has been generated, making it a separate pipeline. However, for NIS approaches, we consider all three aspects, because domain adaptation is inherently part of these methods. Many NIS models are trained or conditioned on real-world imagery. During training, these methods are directly supervised with real images, which is essentially a core mechanism of domain adaptation.

#### 6.2.1 Photorealism

All NIS approaches are capable of medium to high photorealism. This is because they rely exclusively on NeRF (Tong et al. [^108]), Gaussian Splatting (Zanjani et al. [^133]), or diffusion models (Li et al. [^62]; Lin et al. [^55]; Wang et al. [^122]; Gao et al. [^27], [^28]), which are methods developed for high quality images synthesis. While (Tong et al. [^108]) only achieves medium photorealism due to the restricted composition of target and background NeRFs, the other methods are capable of high photorealism.

#### 6.2.2 Domain randomization

Similar to autonomous driving methods in the 3D model-based paradigm, all NIS approaches support structured domain randomization. They allow alterations in weather conditions, object placement, lighting, and camera perspectives. Moreover, some approaches such as 3D Data Augmentation (Tong et al. [^108]) and DriveDreamer (Wang et al. [^122]) are explicitly designed to enable such capabilities.

#### 6.2.3 Domain adaptation

We introduced domain adaptation as a set of techniques that bridge the Sim2Real gap by aligning synthetic data distributions with real-world data. These methods include discrepancy-based approaches, adversarial learning, and self-supervised strategies. Following the structure used for photorealism and domain adaptation, we classify the level of domain adaptation as low, medium, or high. We define no domain adaptation when no strategy is applied, medium when one strategy is used, and high when multiple strategies are combined.

We emphasize that domain adaptation plays an inherent role in NIS models. NIS approaches rely on discrepancy-based methods because they condition on rendered images to match the training data. As a result, most models adapt to the nuScenes data distribution. Based on this, we consider all NIS models to include medium domain adaptation.

### 6.3 Acceptance

Similar to the previous analysis, we only consider publicly available dataset generators. As DriveGEN(Lin et al. [^55]),Tong et al. ([^108]), and Zanjani et al. ([^133]) are not yet publicly available, we did not assess their level of acceptance. In general, we distinguish between low, medium, and high acceptance, based on whether the methods were evaluated on 3D detection tasks and whether they were used to create a dataset. All remaining approaches were applied to create datasets, which all have been evaluated on 3D detection tasks. Similar to some indoor methods evaluated in the previous section, these data were used to augment the nuScenes dataset and were evaluated in combination with it. However, like other autonomous driving methods, they were only applied to a single dataset. For this reason, they showcase medium acceptance.

## 7 Conclusion

The objective of this review was to deliver a comprehensive analysis of the automation level in synthetic dataset generation for monocular 3D detection. To achieve this, we distinguished between 3D-modeling and NIS approaches. For each of these categories, we proposed a branch-based categorization scheme, which classifies relevant approaches based on their scene composition and NIS methods. Subsequently, we evaluated the automation level of these methods, within the branches of our categorization scheme. Additionally, we outlined strategies to bridge the Sim2Real gap and determined the acceptance of the introduced methods within the scientific community. In the following we summarize the primary insights of our review, furthermore, we outline challenges and development trends.

### 7.1 Summary

To summarize our findings, we revisit the proposed categorization scheme.Furthmore, we summarize our results for the three categories applied during analyzing both model families.

#### 7.1.1 3D modeling approaches

##### 7.1.1.1 Categorization scheme

In this review, we proposed a categorization scheme that distinguishes 3D modeling solutions into domain specific and domain agnostic methods. Domain specific methods construct 3D scenes with spatial arrangements and appearances that closely resemble the real world, while domain agnostic methods generate scenes with randomized compositions. We further divided domain specific approaches into world-based and procedural methods, depending on whether they utilize large scale environments or smaller, constructed scenes. Domain agnostic generators were categorized based on their rendering techniques, either rasterization or ray tracing.

##### 7.1.1.2 Automation level

3D Model based approaches for monocular 3D detection involve semi-automated and fully-automated dataset generation for both domain specific and agnostic approaches. All virtual and hybrid procedural methods for specific dataset generation, all agnostic methods, and some world-based solutions are capable of fully automated dataset generation. To archive this, agnostic and procedural approaches create 3D scenes with all required elements (3D assets, virtual cameras, etc.) from scratch and render/annotate images these scenes. World-based approaches, on the other hand, automatically determine suitable camera positions in their corresponding environment, by applying rule-based camera positioning or by utilizing self-driving in simulation environments. In contrast, semi-automated methods require human intervention to guide the scene composition process. This is often the case in world-based approaches for indoor and outdoor environments, where a human operator is needed to control a robot or drive a vehicle through the simulated world.

##### 7.1.1.3 Sim2Real gap

Our review highlights that photorealism plays a central role in narrowing the Sim2Real gap. However, the standards for what qualifies as photorealistic have evolved considerably. Therefore, we introduced three levels of photorealism: low, medium, and high. Older generators relied on simple effects such as shadow rendering. In contrast, modern generators increasingly incorporate complex phenomena such as specular highlights, accurate reflections, and transparent materials. Furthermore, they utilize ray tracing to render images. This advancement enables high photorealism, particularly in procedural and position-based generators. However, many simulation-based outdoor and indoor generators still achieve only low to medium realism due to the computational complexity of the simulations.

Domain randomization offers another strategy for addressing the Sim2Real gap, particularly for improving generalization. It is a built-in feature of agnostic generators and is increasingly being adopted by specific generators as well. In particular, structured domain randomization has emerged in specific generators based on both indoor and outdoor simulations. Examples include variations in weather, traffic, lighting, and materials in outdoor simulators, as well as object placement constraints in indoor scenes.

##### 7.1.1.4 Acceptance

Our acceptance evaluation considered only methods that are publicly available. We introduced three levels of acceptance: low, medium, and high. High acceptance requires that the dataset generator has been used for multiple datasets and has been evaluated on 3D detection tasks. Medium acceptance requires the availability of one public dataset and evaluation. Low acceptance indicates that only one or neither of these criteria has been met.

Among all methods, domain-agnostic generators received the highest level of acceptance. Most specific and approaches were only applied to generate a single dataset. Moreover, many of these methods lack publicly available implementations or 3D assets, which prevented their inclusion in our analysis. In particular, hybrid procedural approaches are generally not publicly available. For outdoor world-based approaches, CARLA has been widely used to develop dataset generators. However, these generators were typically applied to create only a single dataset. As a result, they demonstrate only medium acceptance

#### 7.1.2 Neural image synthesis approaches

##### 7.1.2.1 Categorization scheme

Our categorization Scheme for NIS-based approaches involved two sub categories: Diffision based asnr Radiacne Field based methodsd. Diffisuin based methods were further distinguished in methods for dataset expension and new dataset creation.

##### 7.1.2.2 Automation level

All seven NIS-approach in Radiance Field and Diffusion model category are semi-automarewd solutions, which only autolate annotation process. Reason for this, that scene layouts have or images have to be provided during inference, by human operator.

##### 7.1.2.3 Sim2Real gap

In contrast to 3D modeling methods, we considered photorealism, domain randomization, and domain adaptation for NIS-based approaches. All methods exhibit medium to high photorealism due to their high-fidelity rendering techniques. Furthermore, they incorporate SDR, as they are capable of altering driving scenes with physically plausible variations. Finally, they demonstrate medium domain adaptation capabilities, as discrepancy-based approaches are integrated into their paradigm

##### 7.1.2.4 Acceptance

Four out of seven methods are currently available to the public. All of these methods were used to create public datasets, which were evaluated on monocular 3D detection tasks. As is typical for autonomous driving methods, they were not applied to other tasks. Therefore, all four methods are considered to have medium acceptance.

### 7.2 Discussion

In the following, we discuss the main findings of our review. For this purpose, we describe the state of the art with respect to each of our three evaluation criteria. Based on this analysis, we outline key challenges and development trends in the field.

#### 7.2.1 Automation level

Domain-agnostic generators and position-based solutions for indoor approaches exhibit the highest degree of automation. This is primarily due to their simplicity compared to world-based simulation approaches. They often require only 3D models of the target objects and do not rely on additional components such as full 3D environments, simulation platforms, driving controllers, or complex control signals, as seen in NIS-based approaches. Moreover, the involvement of a human operator to run complex simulations in indoor and outdoor 3D modeling approaches, along with the intricate inference processes in most NIS solutions, limits their potential for higher automation. However, it is important to note that domain-agnostic dataset generation is significantly less challenging, as the 3D scene does not need to be structurally coherent. Additionally, these solutions are explicitly designed for dataset generation, whereas in most world-based approaches, dataset generation is just one among many capabilities. In the case of NIS-based approaches, the implicit control mechanisms of diffusion models, along with complex radiance field composition, manipulation, and inference trajectory generation, further hinder a higher degree of automation.

##### 7.2.1.1 Challenges

In order to perform semi- or fully automated dataset generation with 3D modeling approaches, world-based approaches usually require realistic 3D worlds. This reliance often limits synthetic dataset generation to domains where such worlds are available. In specialized domains, ready-to-use 3D worlds are often scarce. For example, while autonomous driving frequently leverages 3D worlds provided by the community (Türkcan et al. [^114]), industrial mobile robot applications often require environments to be created by 3D experts (Akar et al. [^2]). To overcome this issue, simulators-platforms such as CARLA, ISAAC and Air-Sim provide utility functions. These utility functions allow users to create new worlds, by combining and customizing predefined 3D assets. However, this process is still time-consuming and requires expert knowledge about the simulation platform. Due to this reason, most recent dataset generators utilize procedural environment generation (Ge et al. [^31]; Raistrick et al. [^84], [^83]). However, to create new environments in a procedural manner, a dataset generation still requires 3D assets as baseline. Generating these models is a key challenge, which is also relevant for agnostic methods, since both categories rely on high-quality 3D models. Even for simpler objects like pallets, the creation of suitable 3D models remains a complex and time-intensive process (Knitt et al. [^49]). Another example is provided by Tran et al. ([^111]), who demonstrate the complexity of creating a high-quality 3D model for a customized detection task, such as spacecraft detection (Tran et al. [^111]).

##### 7.2.1.2 Development trends and research direction

Another major trend related to improved automation is increasing compatibility, which highlights the potential for dataset generators to be integrated into unified pipelines. Recent world-based methods, such as BVS (Ge et al. [^31]) and Infinite Worlds (Raistrick et al. [^84], [^83]), can generate new 3D worlds through large-scale procedural composition. These procedurally generated worlds can be seamlessly integrated with robotic simulation frameworks like OmniGibson (Li et al. [^61]) and ISAAC (NVIDIA [^75]), providing the necessary 3D environments for effective dataset generation.

Even NIS-based approaches might be integrated into such pipelines—for example, to provide digital twins of real 3D worlds, as demonstrated in Vid2Sim (Xie et al. [^128]). Additionally, they could be used to apply domain adaptation after a world-based dataset has been generated. Corporate research from Intel and NVIDIA shows promising results in this area. Notably, this paradigm is already being applied successfully in the 2D domain (Zhou et al. [^136]).

Another possibility for further automation is generating 3D models, as these assets form the foundation for 3D model-based approaches. Diffusion-based 3D model generation methods, such as Gaussian Dreamer (Yi et al. [^132]) or DreamGaussian (Tang et al. [^102]), could be integrated into existing pipelines to enable text-to-3D model generation.

Additionally, radiance field-based reconstruction of 3D models from images could also be utilized (Tang et al. [^103]). A similar approach might be applied directly to NIS methods, based on radiance fields. For example, multi-view images generation from text prompts could be used to train object-centric radiance fields, which could be utilized to compose larger radiance fields.

#### 7.2.2 Bridging the Sim2Real gap

To bridge the Sim2Real gap, modern 3D modeling approaches place growing emphasis on photorealism, a development closely tied to advances in rendering technologies and increased computational power. As a result, the appearance gap between synthetic and real-world data has been significantly reduced. A similar trend can be observed in NIS-based approaches, which also demonstrate high photorealism.

However, high photorealism comes at the cost of increased computational demands. Techniques such as ray tracing, NIS inference using radiance fields, and diffusion models are computationally intensive. Due to real-time performance requirements, photorealism in many world-based simulation approaches is limited, often resulting in medium to low photorealism. Nonetheless, recent advancements, such as the upgrade of CARLA (Dosovitskiy et al. [^21]) to Unreal Engine 5 and the high rendering capabilities of flexible approaches, may lead to improved photorealism in the near future.

We considered domain randomization as second measurement to approach the Sim2Real gap. Domain Randomization and SDR are present across various branches of 3D modeling and NIS-based approaches. While domain-agnostic methods tend to demonstrate a high degree of domain randomization, world-based and NIS-based approaches often exhibit strong SDR capabilities.

Domain Adaptation is considered the third key strategy for addressing the Sim2Real gap. In the context of 3D modeling, we assume that domain adaptation is typically applied after dataset generation and is not inherently part of the generation framework itself. However, integration is possible, particularly as new methods emerge that apply domain adaptation directly to simulated images. For example, structure-preserving generative models may enable style transfer from synthetic to real-world domains.

One promising approach is NVIDIA’s COSMOS model for domain transfer, which could potentially be integrated into frameworks like ISAAC to further narrow the Sim2Real gap.

##### 7.2.2.1 Challenges

Challenges related to photorealism and domain randomization arise when addressing the Sim2Real gap. First, improved rendering capabilities in 3D modeling approaches are of limited value if high-quality models are not available. Many rendering parameters, such as reflections, depend heavily on model-specific attributes like texture and material properties. For example, the authors of NViSII (Morrical et al. [^72]) had to redesign a prominent 3D model because it was not sufficiently detailed for accurate rendering. Therefore, the availability and quality of 3D models may become a major challenge in future work. Another challenge regarding photorealism is that diffusion-based NIS approaches may be difficult to apply in domains with specialized environments or uncommon target objects. This is because such methods might not be capable of generating sufficiently accurate representations.

Alongside photorealism, domain randomization also poses a challenge: SDR is often limited to basic variations, such as object placement or surface texture. However, many detection tasks require handling objects in varying states, such as empty and full cups or open and closed doors. Capturing these dynamic object states in model-based dataset generation remains a significant challenge. Extending this capability to NIS-based approaches is even more complex, as it requires precise diffusion control along with a functional understanding of object behavior or support for deformable radiance fields.

##### 7.2.2.2 Development trends and research direction

We already referenced possible approaches to address the challenge of model generation when discussing acceptance. However, to provide suitable model textures or high-quality surfaces, one could apply neural surface modeling techniques that are specialized in capturing surface properties.

Regarding SDR, a recent extension of Infinigen, proposed by Lian et al. ([^53]), enables the creation of objects in different states, which may enhance the capabilities of this generator. In addition, modern world-based robotics frameworks and flexible simulation platforms such as BVS (Ge et al. [^31]) and ISAAC (world) (NVIDIA [^75]) also address this challenge by supporting dynamic object states during simulation, making them a promising solution as well.

#### 7.2.3 Acceptance

Domain-agnostic approaches based on 3D models represent the most widely accepted category, while NIS-based and domain-specific approaches are comparatively less popular. The primary reason for the popularity of agnostic methods is their simplicity combined with strong performance. Unlike domain-specific or NIS-based methods, agnostic approaches do not require a full 3D world, which makes them more accessible and easier to use.

Another contributing factor is the availability of dataset generation frameworks such as BlenderProc, which offer a wide range of dataset generation functionalities. These frameworks are easy to modify and adapt to different application scenarios, further increasing their utility.

In contrast, world-based approaches face lower acceptance for several reasons. Many real-world tasks require custom object detection, and integrating specific objects into simulated 3D worlds is significantly more complex than using task-agnostic dataset generators. Additionally, many world-based dataset generators are either not publicly available or do not include the 3D worlds required to use them effectively.

##### 7.2.3.1 Challenges

Due to their scenario-specific scene composition and SDR functionalities, NIS-based and world-based solutions offer promising capabilities for narrowing the Sim2Real gap. However, the primary challenge for both approaches is improving accessibility.

Procedural world generation and the world-building capabilities of flexible approaches provide a path toward making world-based methods more accessible. Additionally, simplifying the NIS inference process and reducing reliance on annotated real-world data could make NIS-based approaches more practical and widely adoptable.

##### 7.2.3.2 Development trends and research direction

Relying on real-world data for both training and inference remains a major challenge, limiting wider adoption and practical use of NIS approaches. To address this, future work might focus on extending 2D scene understanding and annotation tools like Grounded-SAM(Ren et al. [^86]), which is based on Meta’s Segment Anything Model (SAM)(Kirillov et al. [^47]), to support 3D annotation.

Due to their scenario-specific scene composition and SDR functionalities, NIS-based and world-based solutions offer promising capabilities for narrowing the Sim2Real gap. The main challenge for both approaches is improving accessibility. Procedural world generation and world-building functions in flexible approaches provide a way to make world generation more accessible. In addition, simplifying the NIS inference process could further lower the barrier to adoption.

## References

- Ahmadyan A, Zhang L, Wei J, Ablavatski A, Grundmann M (2020) Objectron: a large scale dataset of object-centric videos in the wild with pose annotations. In: 2021 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 7818–7827
- Akar CA, Tekli J, Khalil J, Yaghi A, Haddad Y, Makhoul A, Kamradt M (2024) Sordi.ai: large-scale synthetic object recognition dataset generation for industries. Multimed Tools Appl 84(17):18263–18304
- Anagnostopoulou D, Retsinas G, Efthymiou N, Filntisis PP, Maragos P (2023) A realistic synthetic mushroom scenes dataset. In: 2023 IEEE/CVF Conference on computer vision and pattern recognition workshops (CVPRW), pp 6282–6289
- Bai K, Zhang L, Chen Z, Wan F, Zhang J (2024) Close the sim2real gap via physically-based structured light synthetic data simulation. In: 2024 IEEE international conference on robotics and automation (ICRA), pp 17035–17041
- Barragan JA, Zhang J, Zhou H, Munawar A, Kazanzides P (2024) Realistic data generation for 6d pose estimation of surgical instruments. In: 2024 IEEE international conference on robotics and automation (ICRA), pp 13347–13353
- Barra S, Marras M, Mohamed S, Podda AS, Saia R (2023) Can existing 3d monocular object detection methods work in roadside contexts? a reproducibility study. In: International conference of the italian association for artificial intelligence. [https://api.semanticscholar.org/CorpusID:265051747](https://api.semanticscholar.org/CorpusID:265051747)
- Blender Institute (2024): Blender. [https://www.blender.org/](https://www.blender.org/)
- Blomqvist K, Chung JJ, Ott L, Siegwart RY (2022) Semi-automatic 3d object keypoint annotation and detection for the masses. In: 2022 26th International conference on pattern recognition (ICPR), pp 3908–3914
- Blomqvist K, Chung JJ, Ott L, Siegwart RY (2023) Nerfing it: offline object segmentation through implicit modeling. In: 2023 IEEE International conference on robotics and automation (ICRA), pp 9407–9413
- Borkman S, Crespi A, Dhakad S, Ganguly S, Hogins J, Jhang YC, Kamalzadeh M, Li B, Leal S, Parisi P, Romero C, Smith W, Thaman A, Warren S, Yadav N (2021) Unity perception: generate synthetic data for computer vision. [arXiv:abs/2107.04259](http://arxiv.org/2107.04259)
- Brazil G, Kumar A, Straub J, Ravi N, Johnson J, Gkioxari G (2022) Omni3d: a large benchmark and model for 3d object detection in the wild. In: 2023 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 13154–13164
- Caesar H, Bankiti V, Lang AH, Vora S, Liong VE, Xu Q, Krishnan A, Pan Y, Baldan G, Beijbom O (2019) Nuscenes: a multimodal dataset for autonomous driving. In: 2020 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 11618–11628
- Çalli B, Singh A, Walsman A, Srinivasa SS, Abbeel P, Dollar AM (2015) The ycb object and model set: towards common benchmarks for manipulation research. In: 2015 international conference on advanced robotics (ICAR), pp 510–517
- Carta S, Castrillón-Santana M, Marras M, Mohamed S, Podda AS, Saia R, Sau M, Zimmer W (2024) Roadsense3d: a framework for roadside monocular 3d object detection. In: Adjunct Proceedings of the 32nd ACM conference on user modeling, adaptation and personalization
- Chen T, Ren T, Niu J, Li Q (2021) A novel shape-based robotic sorting approach based on computer vision. In: 2021 IEEE Intl Conf on Parallel & Distributed Processing with Applications, Big Data & Cloud Computing, Sustainable Computing & Communications, Social Computing & Networking (ISPA/BDCloud/SocialCom/SustainCom), pp 660–667
- Chen K, Xie E, Chen Z, Hong L, Li Z, Yeung D-Y (2023) Geodiffusion: text-prompted geometric control for object detection data generation. In: International Conference on Learning Representations. [https://api.semanticscholar.org/CorpusID:259096176](https://api.semanticscholar.org/CorpusID:259096176)
- Costanzo M, Simone MD, Federico S, Natale C, Pirozzi S (2023) Enhanced 6d pose estimation for robotic fruit picking. In: 2023 9th International conference on control, decision and information technologies (CoDIT), pp 901–906
- Dai A, Chang AX, Savva M, Halber M, Funkhouser TA, Nießner M (2017) Scannet: richly-annotated 3d reconstructions of indoor scenes. In: 2017 IEEE conference on computer vision and pattern recognition (CVPR), pp 2432–2443
- Denninger M, Sundermeyer M, Winkelbauer D, Olefir D, Hodan T, Zidan Y, Elbadrawy M, Knauer MW, Katam H, Lodhi A (2020) Blenderproc: reducing the reality gap with photorealistic rendering
- Denninger M, Winkelbauer D, Sundermeyer M, Boerdijk W, Knauer M, Strobl KH, Humt M, Triebel R (2023) Blenderproc2: a procedural pipeline for photorealistic rendering. J Open Source Softw 8:4901
- Doruk AE, Ozkaya TE, Gülmez F, Uslu F (2023) A comparative study for 6d pose estimation of textureless and symmetric objects used in automotive manufacturing industry. In: 2023 5th International Congress on Human-Computer Interaction, Optimization and Robotic Applications (HORA), pp 1–7
- Dosovitskiy A, Ros G, Codevilla F, López AM, Koltun V (2017) Carla: an open urban driving simulator. In: Conference on Robot Learning. [https://api.semanticscholar.org/CorpusID:5550767](https://api.semanticscholar.org/CorpusID:5550767)
- Drobnitzky M, Friederich J, Egger B, Zschech P (2022) Survey and systematization of 3d object detection models and methods. [arXiv:abs/2201.09354](http://arxiv.org/2201.09354)
- Epic Games (2024): Unreal Engine. [https://www.unrealengine.com](https://www.unrealengine.com/)
- Filipovic B, Sikic F, Kalafatic Z, Loncaric S, Subavsic M (2023) Detection of tea box orientations in retail shelves images. In: 2023 International symposium on image and signal processing and analysis (ISPA), pp 1–5
- Firman M (2016) Rgbd datasets: past, present and future. In: 2016 IEEE conference on computer vision and pattern recognition workshops (CVPRW), pp 661–673
- Gaidon A, Wang Q, Cabon Y, Vig E (2016) Virtualworlds as proxy for multi-object tracking analysis. In: 2016 IEEE conference on computer vision and pattern recognition (CVPR), pp 4340–4349
- Gao R, Chen K, Xie E, Hong L, Li Z, Yeung D-Y, Xu Q (2023) Magicdrive: street view generation with diverse 3d geometry control. [arXiv:abs/2310.02601](http://arxiv.org/2310.02601)
- Gao R, Chen K, Xiao B, Hong L, Li Z, Xu Q (2024) Magicdrive-v2: high-resolution long video generation for autonomous driving with adaptive control. [https://api.semanticscholar.org/CorpusID:274165928](https://api.semanticscholar.org/CorpusID:274165928)
- Garcia-Garcia A, Martinez-Gonzalez P, Oprea S, Castro-Vargas JA, Orts S, Rodríguez JG, Jover-Alvarez A (2018) The robotrix: an extremely photorealistic and very-large-scale indoor dataset of sequences with robot trajectories and interactions. In: 2018 IEEE/RSJ International conference on intelligent robots and systems (IROS), pp 6790–6797 (2018)
- Geiger A, Lenz P, Urtasun R (2012) Are we ready for autonomous driving? the kitti vision benchmark suite. In: 2012 IEEE conference on computer vision and pattern recognition, pp 3354–3361
- Ge Y, Tang Y, Xu J, Gokmen C, Li C, Ai W, Martinez BJ, Aydin A, Anvari M, Chakravarthy AK, Yu H-X, Wong J, Srivastava S, Lee S, Zha SC, Itti L, Li Y, Mart’in-Mart’in R, Liu M, Zhang P, Zhang R, Li F-F, Wu J (2024) Behavior vision suite: customizable dataset generation via simulation. In: 2024 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), pp 22401–22412
- Grau O, Hagn K (2023) Valerie22 - a photorealistic, richly metadata annotated dataset of urban environments. Proceedings of the 7th ACM computer science in cars symposium
- Greff K, Belletti F, Beyer L, Doersch C, Du Y, Duckworth D, Fleet DJ, Gnanapragasam D, Golemo F, Herrmann C, Kipf T, Kundu A, Lagun D, Laradji IH, Liu H-T, Meyer H, Miao Y, Nowrouzezahrai D, Oztireli C, Pot E, Radwan N, Rebain D, Sabour S, Sajjadi MSM, Sela M, Sitzmann V, Stone A, Sun D, Vora S, Wang Z, Wu T, Yi KM, Zhong F, Tagliasacchi A (2022) Kubric: a scalable dataset generator. In: 2022 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 3739–3751
- Guo A, Wen B, Yuan J, Tremblay J, Tyree S, Smith J, Birchfield S (2023) Handal: a dataset of real-world manipulable object categories with pose annotations, affordances, and reconstructions. In: 2023 IEEE/RSJ international conference on intelligent robots and systems (IROS), pp 11428–11435
- Hempel T, Abdelrahman AA, Al-Hamadi A (2023) Toward robust and unconstrained full range of rotation head pose estimation. IEEE Trans Image Process 33:2377–2387
- Hodan T, Haluza P, Obdrzálek S, Matas J, Lourakis MIA, Zabulis X (2017) T-less: an rgb-d dataset for 6d pose estimation of texture-less objects. In: 2017 IEEE winter conference on applications of computer vision (WACV), pp 880–888
- Hodan T, Vineet V, Gal R, Shalev E, Hanzelka J, Connell T, Urbina P, Sinha SN, Guenter BK (2019) Photorealistic image synthesis for object instance detection. In: 2019 IEEE international conference on image processing (ICIP), pp 66–70
- Hodan T, Sundermeyer M, Drost B, Labbé Y, Brachmann E, Michel F, Rother C, Matas J (2020) Bop challenge 2020 on 6d object localization. [arXiv:abs/2009.07378](http://arxiv.org/2009.07378)
- Hu Y, Fang S, Xie W, Chen S (2022) Aerial monocular 3d object detection. IEEE Robot Autom Lett 8:1959–1966
- Huang R, Zheng H, Wang Y, Xia Z, Pavone M, Huang G (2024) Training an open-vocabulary monocular 3d detection model without 3d data. In: The Thirty-eighth annual conference on neural information processing systems
- Jalal M, Spjut JB, Boudaoud B, Betke M (2019) Sidod: a synthetic image dataset for 3d object pose recognition with distractors. In: 2019 IEEE/CVF conference on computer vision and pattern recognition workshops (CVPRW), pp 475–477
- Kar A, Prakash A, Liu M-Y, Cameracci E, Yuan J, Rusiniak M, Acuna D, Torralba A, Fidler S (2019) Meta-sim: learning to generate synthetic datasets. In: 2019 IEEE/CVF international conference on computer vision (ICCV), pp 4550–4559
- Kaskman R, Zakharov S, Shugurov IS, Ilic S (2019) Homebreweddb: rgb-d dataset for 6d pose estimation of 3d objects. In: 2019 IEEE/CVF international conference on computer vision workshop (ICCVW), pp 2767–2776
- Kehl W, Manhardt F, Tombari F, Ilic S, Navab N (2017) Ssd-6d: making rgb-based 3d detection and 6d pose estimation great again. In: 2017 IEEE international conference on computer vision (ICCV), pp 1530–1538
- Kerbl B, Kopanas G, Leimkuehler T, Drettakis G (2023) 3d gaussian splatting for real-time radiance field rendering. ACM Trans Gr (TOG) 42:1–14
- Kim S-H, Hwang Y (2021) A survey on deep learning based methods and datasets for monocular 3d object detection. Electronics 10:517
- Kirillov A, Mintun E, Ravi N, Mao H, Rolland C, Gustafson L, Xiao T, Whitehead S, Berg AC, Lo W-Y, Dollár P, Girshick RB (2023) Segment anything. In: 2023 IEEE/CVF international conference on computer vision (ICCV), pp 3992–4003
- Kleeberger K, Landgraf C, Huber MF (2019) Large-scale 6d object pose estimation dataset for industrial bin-picking. In: 2019 IEEE/RSJ international conference on intelligent robots and systems (IROS), pp 2573–2578
- Knitt M, Schyga J, Adamanov A, Hinckeldeyn J, Kreutzfeldt J (2022) Estimating the pose of a euro pallet with an rgb camera based on synthetic training data. [arXiv:abs/2210.06001](http://arxiv.org/2210.06001)
- Kolve E, Mottaghi R, Han W, VanderBilt E, Weihs L, Herrasti A, Deitke M, Ehsani K, Gordon D, Zhu Y, Kembhavi A, Gupta AK, Farhadi A (2017) Ai2-thor: an interactive 3d environment for visual AI. [arXiv:abs/1712.05474](http://arxiv.org/1712.05474)
- Lee T, Lee B-U, Shin I, Choe J, Shin U, Kweon I-S, Yoon K-J (2021) Uda-cope: unsupervised domain adaptation for category-level object pose estimation. In: 2022 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 14871–14880
- Li X, Cao R, Feng Y, Chen K, Yang B, Fu C-W, Li Y, Dou Q, Liu Y, Heng P-A (2022) A sim-to-real object recognition and localization framework for industrial robotic bin picking. IEEE Robot Autom Lett 7:3961–3968
- Lian X, Yu Z, Liang R, Wang Y, Luo LR, Chen K, Zhou Y, Tang Q, Xu X, Lyu Z, Dai B, Pang J (2025) Infinite mobility: scalable high-fidelity synthesis of articulated objects via procedural generation. [arXiv:abs/2503.13424](http://arxiv.org/2503.13424)
- Lindermayr J, Odabaşi C, Jordan F, Graf F, Knak L, Kraus W, Bormann R, Huber MF (2023) Ipa-3d1k: a large retail 3d model dataset for robot picking. In: 2023 IEEE/RSJ International conference on intelligent robots and systems (IROS), pp 11404–11411
- Lin H, Guo Z, Zhang Y, Niu S, Li Y, Zhang R, Cui S, Li Z (2025) Drivegen: generalized and robust 3d detection in driving via controllable text-to-image diffusion generation. [arXiv:abs/2503.11122](http://arxiv.org/2503.11122)
- Li W, Saeedi S, McCormac J, Clark R, Tzoumanikas D, Ye Q, Huang Y, Tang R, Leutenegger S (2018) Interiornet: mega-scale multi-sensor photo-realistic indoor scenes dataset. [arXiv:abs/1809.00716](http://arxiv.org/1809.00716)
- Liu Z, Wu Z, T’oth R (2020) Smoke: single-stage monocular 3d object detection via keypoint estimation. In: 2020 IEEE/CVF conference on computer vision and pattern recognition workshops (CVPRW), pp 4289–4298
- Liu X, Hao S, Xu K (2023) Pose estimation of space targets based on geometry structure features. In: Proceedings of the 2023 2nd Asia conference on algorithms, computing and machine learning
- Liu Y, Chen W, Bai Y, Luo J-H, Song X, Jiang K, Li Z, Zhao G, Lin J, Li G, Gao W, Lin L (2024) Aligning cyber space with physical world: a comprehensive survey on embodied AI. [arXiv:abs/2407.06886](http://arxiv.org/2407.06886)
- Li C, Xia F, Mart’in-Mart’in R, Lingelbach M, Srivastava S, Shen B, Vainio K, Gokmen C, Dharan G, Jain T, Kurenkov A, Liu K, Gweon H, Wu J, Fei-Fei L, Savarese S (2021) igibson 2.0: object-centric simulation for robot learning of everyday household tasks. [arXiv:abs/2108.03272](http://arxiv.org/2108.03272)
- Li C, Zhang R, Wong J, Gokmen C, Srivastava S, Martín-Martín R, Wang C, Levine G, Lingelbach M, Sun J, Anvari M, Hwang M, Sharma M, Aydin A, Bansal D, Hunter S, Kim K-Y, Lou A, Matthews CR, Villa-Renteria I, Tang JH, Tang C, Xia F, Savarese S, Gweon H, Liu CK, Wu J, Fei-Fei L (2022) Behavior-1k: a benchmark for embodied ai with 1, 000 everyday activities and realistic simulation. In: Conference on Robot Learning. [https://api.semanticscholar.org/CorpusID:255198985](https://api.semanticscholar.org/CorpusID:255198985)
- Li X, Zhang Y, Ye X (2023) Drivingdiffusion: layout-guided multi-view driving scene video generation with latent diffusion model. [arXiv:abs/2310.07771](http://arxiv.org/2310.07771)
- Manhardt F, Wang G, Busam B, Nickel M, Meier S, Minciullo L, Ji X, Navab N (2020) Cps++: Improving class-level 6d pose and shape estimation from monocular images with self-supervised learning. Computer Vision and Pattern Recognition
- Mao J, Shi S, Wang X, Li H (2022) 3d object detection for autonomous driving: a comprehensive survey. Int J Comput Vision 131:1909–1963
- Martinez-Gonzalez P, Oprea S, Garcia-Garcia A, Jover-Alvarez A, Orts-Escolano S, Garcia-Rodriguez J (2018) Unrealrox an extremely photorealistic virtual reality environment for robotics simulations and synthetic data generation. [https://api.semanticscholar.org/CorpusID:53111294](https://api.semanticscholar.org/CorpusID:53111294)
- Martinez-Gonzalez P, Oprea S, Castro-Vargas JA, Garcia-Garcia A, Orts-Escolano S, Garcia-Rodriguez J, Vincze M (2021) Unrealrox+: an improved tool for acquiring synthetic data from virtual 3d environments. In: 2021 International joint conference on neural networks (IJCNN), pp 1–8
- Mata C, Locascio N, Sheikh MA, Kihara K, Fischetti DL (2022) Standardsim: a synthetic dataset for retail environments. In: International conference on image analysis and processing. [https://api.semanticscholar.org/CorpusID:246634940](https://api.semanticscholar.org/CorpusID:246634940)
- McCormac J, Handa A, Leutenegger S, Davison AJ (2016) Scenenet rgb-d: 5m photorealistic images of synthetic indoor trajectories with ground truth. [arXiv:abs/1612.05079](http://arxiv.org/1612.05079)
- Mehr G, Eskandarian A (2025) Simbev: a synthetic multi-task multi-sensor driving data generation tool and dataset. [arXiv:abs/2502.01894](http://arxiv.org/2502.01894)
- Meier J, Scalerandi L, Dhaouadi O, Kaiser J, Araslanov N, Cremers D (2024) Carla drone: monocular 3d object detection from a different perspective. [arXiv:abs/2408.11958](http://arxiv.org/2408.11958)
- Mildenhall B, Srinivasan PP, Tancik M, Barron JT, Ramamoorthi R, Ng R (2020) Nerf: representing scenes as neural radiance fields for view synthesis. In: Vedaldi A, Bischof H, Brox T, Frahm J-M (eds) Computer vision - ECCV 2020. Springer, Cham, pp 405–421
- Morrical N, Tremblay J, Lin Y, Tyree S, Birchfield S, Pascucci V, Wald I (2021) Nvisii: a scriptable tool for photorealistic image generation. [arXiv:abs/2105.13962](http://arxiv.org/2105.13962)
- Müller M, Casser V, Lahoud J, Smith NG, Ghanem B (2017) Sim4cv: a photo-realistic simulator for computer vision applications. Int J Comput Vision 126:902–919
- Muller R, Man Y, Celik ZB, Li MH, Gerdes RM (2022) Drivetruth: automated autonomous driving dataset generation for security applications. In: Proceedings fourth international workshop on automotive and autonomous vehicle security
- NVIDIA (2022) NVIDIA Isaac Sim. [https://developer.nvidia.com/isaac/sim](https://developer.nvidia.com/isaac/sim). Accessed: 05, 03, 2024
- NVIDIA (2024): NVIDIA Omniverse. [https://www.nvidia.com/en-us/omniverse/](https://www.nvidia.com/en-us/omniverse/)
- Paulin G, Ivasic-Kos M (2023) Review and analysis of synthetic dataset generation methods and techniques for application in computer vision. Artif Intell Rev 56(9):1–45
- Prakash A, Boochoon S, Brophy M, Acuna D, Cameracci E, State G, Shapira O, Birchfield S (2018) Structured domain randomization: bridging the reality gap by context-aware synthetic data. In: 2019 International conference on robotics and automation (ICRA), pp 7249–7255
- Proença PF, Gao Y (2019) Deep learning for spacecraft pose estimation from photorealistic rendering. In: 2020 IEEE international conference on robotics and automation (ICRA), pp 6007–6013
- Pugh B, Chernak D, Jiddi S (2023) Geosynth: a photorealistic synthetic indoor dataset for scene understanding. IEEE Trans Visual Comput Gr 29:2586–2595
- Qiu W, Yuille AL (2016) Unrealcv: connecting computer vision to unreal engine. [arXiv:abs/1609.01326](http://arxiv.org/1609.01326)
- Rad M, Lepetit V (2017) Bb8: a scalable, accurate, robust to partial occlusion method for predicting the 3d poses of challenging objects without using depth. In: 2017 IEEE international conference on computer vision (ICCV), pp 3848–3856
- Raistrick ARE, Lipson L, Ma Z, Mei L, Wang M, Zuo Y, Kayan K, Wen H, Han B, Wang Y, Newell A, Law H, Goyal A, Yang K, Deng J (2023) Infinite photorealistic worlds using procedural generation. In: 2023 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), pp 12630–12641
- Raistrick ARE, Mei L, Kayan K, Yan D, Zuo Y, Han B, Wen H, Parakh M, Alexandropoulos S, Lipson L, Ma Z, Deng J (2024) Infinigen indoors: photorealistic indoor scenes using procedural generation. In: 2024 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 21783–21794
- Remus A, D’Avella S, Felice FD, Tripicchio P, Avizzano CA (2023) i2c-net: using instance-level neural networks for monocular category-level 6d pose estimation. IEEE Robot Autom Lett 8:1515–1522
- Ren T, Liu S, Zeng A, Lin J, Li K, Cao H, Chen J, Huang X, Chen Y, Yan F, Zeng Z, Zhang H, Li F, Yang J, Li H, Jiang Q, Zhang L (2024) Grounded sam: assembling open-world models for diverse visual tasks. [arXiv:abs/2401.14159](http://arxiv.org/2401.14159)
- Richter SR, Hayder Z, Koltun V (2017) Playing for benchmarks. In: 2017 IEEE International conference on computer vision (ICCV), pp 2232–2241
- Roberts M, Paczan N (2020) Hypersim: a photorealistic synthetic dataset for holistic indoor scene understanding. In: 2021 IEEE/CVF International conference on computer vision (ICCV), pp 10892–10902
- Rombach R, Blattmann A, Lorenz D, Esser P, Ommer B (2021) High-resolution image synthesis with latent diffusion models. In: 2022 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 10674–10685
- Rong G, Shin BH, Tabatabaee H, Lu Q, Lemke S, Mozeiko M, Boise E, Uhm G, Gerow M, Mehta S, Agafonov E, Kim TH, Sterner E, Ushiroda K, Reyes M, Zelenkovsky D, Kim S (2020) Lgsvl simulator: a high fidelity simulator for autonomous driving. In: 2020 IEEE 23rd international conference on intelligent transportation systems (ITSC), pp 1–6
- Ros G, Sellart L, Materzynska J, Vázquez D, López AM (2016) The synthia dataset: a large collection of synthetic images for semantic segmentation of urban scenes. In: 2016 IEEE conference on computer vision and pattern recognition (CVPR), pp 3234–3243
- SAE (2016) Taxonomy and definitions for terms related to driving automation systems for on-road motor vehicles
- Schieber H, Demir KC, Kleinbeck C, Yang SH, Roth D (2024) Indoor synthetic data generation: a systematic review. Comput Vis Image Underst 240:103907
- Shah S, Dey D, Lovett C, Kapoor A (2017) Airsim: high-fidelity visual and physical simulation for autonomous vehicles. In: International Symposium on Field and Service Robotics. [https://api.semanticscholar.org/CorpusID:20999239](https://api.semanticscholar.org/CorpusID:20999239)
- Sharma S, Beierle C, D’Amico S (2018) Pose estimation for non-cooperative spacecraft rendezvous using convolutional neural networks. In: 2018 IEEE Aerospace Conference, pp 1–12
- Simoni A, Pelosin F (2025) Bounding box-guided diffusion for synthesizing industrial images and segmentation map. [https://api.semanticscholar.org/CorpusID:278339348](https://api.semanticscholar.org/CorpusID:278339348)
- Song S, Lichtenberg SP, Xiao J (2015) Sun rgb-d: a rgb-d scene understanding benchmark suite. In: 2015 IEEE conference on computer vision and pattern recognition (CVPR), pp 567–576
- Strazdas D, Hintz J, Felßberg A-M, Al-Hamadi A (2020) Robots and wizards: an investigation into natural human-robot interaction. IEEE Access 8:207635–207642
- Strazdas D, Hintz J, Khalifa A, Abdelrahman AA, Hempel T, Al-Hamadi A (2022) Robot system assistant (Rosa): towards intuitive multi-modal and multi-device human-robot interaction. Sens 22(3):923
- Sun T, Segu M, Postels J, Wang Y, Gool LV, Schiele B, Tombari F, Yu F (2022) Shift: a synthetic driving dataset for continuous multi-task domain adaptation. In: 2022 IEEE/CVF Conference on computer vision and pattern recognition (CVPR), pp 21339–21350
- Sural S, Sahu N, Rajkumar R (2024) Contextualfusion: context-based multi-sensor fusion for 3d object detection in adverse operating conditions. In: 2024 IEEE intelligent vehicles symposium (IV), pp 1534–1541
- Tang J, Ren J, Zhou H, Liu Z, Zeng G (2023a) Dreamgaussian: generative gaussian splatting for efficient 3d content creation. [arXiv:abs/2309.16653](http://arxiv.org/2309.16653)
- Tang J, Zhou H, Chen X, Hu T, Ding E, Wang J, Zeng G (2023b) Delicate textured mesh recovery from nerf via adaptive surface refinement. In: 2023 IEEE/CVF international conference on computer vision (ICCV), pp 17693–17703
- Tang J, Chen Z, Fu B, Lu W, Li S, Li X, Ji X (2024a) Rov6d: 6d pose estimation benchmark dataset for underwater remotely operated vehicles. IEEE Robot Autom Lett 9:65–72
- Tang Y, Tai C, Chen F-X, Zhang W, Zhang T, Liu X-P, Liu Y-J, Zeng L (2024b) Mobile robot oriented large-scale indoor dataset for dynamic scene understanding. In: 2024 IEEE international conference on robotics and automation (ICRA), pp 613–620
- Teufel S, Gamerdinger J, Kirchner J-P, Volk G, Bringmann O (2024) Collective perception datasets for autonomous driving: a comprehensive review. In: 2024 IEEE intelligent vehicles symposium (IV), pp 1548–1555
- Tonderski A, Lindström C, Hess G, Ljungbergh W, Svensson L, Petersson C (2023) Neurad: neural rendering for autonomous driving. In: 2024 IEEE/CVF Conference on computer vision and pattern recognition (CVPR), pp 14895–14904
- Tong W, Xie J, Li T, Deng H, Geng X, Zhou R, Yang D, Dai B, Lu L, Li H (2023) 3d data augmentation for driving scenes on camera. [arXiv:abs/2303.10340](http://arxiv.org/2303.10340)
- Tong S, Ouyang Z, Hu Q, Li D (2024) 3d spacecraft position and orientation estimation model based on monocular image. In: 2024 IEEE international conference on cybernetics and intelligent systems (CIS) and IEEE international conference on robotics, automation and mechatronics (RAM), pp 508–513
- To T, Tremblay J, McKay D, Yamaguchi Y, Leung K, Balanon A, Cheng J, Hodge W, Birchfield S (2018) NDDS: NVIDIA deep learning dataset synthesizer. [https://github.com/NVIDIA/Dataset\_Synthesizer](https://github.com/NVIDIA/Dataset_Synthesizer)
- Tran QH, Choate JA, Taylor CN, Nykl SL, Curtis DH (2023) Monocular vision and machine learning for pose estimation. In: 2023 IEEE/ION Position, Location and Navigation Symposium (PLANS), pp 128–136
- Tremblay J, To T, Birchfield S (2018a) Falling things: a synthetic dataset for 3d object detection and pose estimation. In: 2018 IEEE/CVF conference on computer vision and pattern recognition workshops (CVPRW), pp 2119–21193
- Tremblay J, To T, Sundaralingam B, Xiang Y, Fox D, Birchfield S (2018b) Deep object pose estimation for semantic robotic grasping of household objects. [arXiv:abs/1809.10790](http://arxiv.org/1809.10790)
- Türkcan MK, Li Y, Zang C, Ghaderi J, Zussman G, Kostic Z (2024) Boundless: generating photorealistic synthetic data for object detection in urban streetscapes. [arXiv:abs/2409.03022](http://arxiv.org/2409.03022)
- Tyree S, Tremblay J, To T, Cheng J, Mosier T, Smith J, Birchfield S (2022) 6-dof pose estimation of household objects for robotic manipulation: an accessible dataset and benchmark. In: 2022 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), pp 13081–13088
- Unity Technologies (2024): Unity. Game development platform. [https://unity.com/](https://unity.com/)
- Wang M, Deng W (2018) Deep visual domain adaptation: a survey. [arXiv:abs/1802.03601](http://arxiv.org/1802.03601)
- Wang H, Sridhar S, Huang J, Valentin JPC, Song S, Guibas LJ (2019) Normalized object coordinate space for category-level 6d object pose and size estimation. In: 2019 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 2637–2646
- Wang G, Manhardt F, Shao J, Ji X, Navab N, Tombari F (2020) Self6d: self-supervised monocular 6d object pose estimation. [arXiv:abs/2004.06468](http://arxiv.org/2004.06468)
- Wang Y, Wang C, Long P, Gu Y, Li W (2021) Recent advances in 3d object detection based on rgb-d: a survey. Displays 70:102077
- Wang P, Jung H, Li Y, Shen S, Srikanth RP, Garattoni L, Meier S, Navab N, Busam B (2022) Phocal: a multi-modal dataset for category-level object pose estimation with photometrically challenging objects. In: 2022 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 21190–21199
- Wang X, Zhu Z, Huang G, Chen X, Zhu J, Lu J (2024) Drivedreamer: towards real-world-drive world models for autonomous driving. In: European conference on computer vision. [https://api.semanticscholar.org/CorpusID:274611109](https://api.semanticscholar.org/CorpusID:274611109)
- Wrenninge M, Unger J (2018) Synscapes: a photorealistic synthetic dataset for street scene parsing. [arXiv:abs/1810.08705](http://arxiv.org/1810.08705)
- Wu Z, Liu T, Luo L, Zhong Z, Chen J, Xiao H, Hou C, Lou H, Chen Y-H, Yang R, Huang Y, Ye X, Yan Z, Shi Y, Liao Y, Zhao H (2023a) Mars: an instance-aware, modular and realistic simulator for autonomous driving. [arXiv:abs/2307.15058](http://arxiv.org/2307.15058)
- Wu W, Zhao Y, Chen H, Gu Y, Zhao R, He Y, Zhou H, Shou MZ, Shen C (2023b) Datasetdm: synthesizing data with perception annotations using diffusion models. [arXiv:abs/2308.06160](http://arxiv.org/2308.06160)
- Xia F, Shen BW, Li C, Kasimbeg P, Tchapmi ME, Toshev A, Martín-Martín R, Savarese S (2019) Interactive Gibson benchmark: a benchmark for interactive navigation in cluttered environments. IEEE Robot Autom Lett 5:713–720
- Xiang J, Yang J, Huang B, Tong X (2023) 3d-aware image generation using 2d diffusion models. In: 2023 IEEE/CVF international conference on computer vision (ICCV), pp 2383–2393
- Xie Z, Liu Z, Peng Z, Wu W, Zhou B (2025) Vid2sim: realistic and interactive simulation from video for urban navigation. [arXiv:abs/2501.06693](http://arxiv.org/2501.06693)
- Xu A, Vasileva MI, Dave A, Seshadri A (2022) Handsoff: labeled dataset generation with no additional human annotations. In: 2023 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 7991–8000
- Yang Z, Zeng A, Yuan C, Li Y (2023a) Effective whole-body pose estimation with two-stages distillation. In: 2023 IEEE/CVF international conference on computer vision workshops (ICCVW), pp 4212–4222
- Yang Z, Zhan F, Liu K, Xu M, Lu S (2023b) Ai-generated images as data source: the dawn of synthetic era. [arXiv:abs/2310.01830](http://arxiv.org/2310.01830)
- Yi T, Fang J, Zhou Z, Wang J, Wu G, Xie L, Zhang X, Liu W, Wang X, Tian Q (2024) Gaussiandreamerpro: text to manipulable 3D Gaussians with highly enhanced quality. [arXiv:abs/2406.18462](http://arxiv.org/2406.18462)
- Zanjani FG, Abati D, Wiggers AJ, Kalatzis D, Petersen J, Cai H, Habibian A (2025) Gaussian splatting is an effective data generator for 3d object detection. [arXiv:abs/2504.16740](http://arxiv.org/2504.16740)
- Zhang L, Rao A, Agrawala M (2023) Adding conditional control to text-to-image diffusion models. In: 2023 IEEE/CVF international conference on computer vision (ICCV), pp 3813–3824
- Zhou L, Song Y, Gao Y, Yu Z, Sodamin M, Liu H, Ma L, Liu L, Liu H, Liu Y, Li H, Chen G, Knoll A (2023) Garchingsim: an autonomous driving simulator with photorealistic scenes and minimalist workflow. In: 2023 IEEE 26th international conference on intelligent transportation systems (ITSC), pp 4227–4232
- Zhou Y, Simon M, Peng Z, Mo S, Zhu H, Guo M, Zhou B (2024) Simgen: simulator-conditioned driving scene generation. [arXiv:abs/2406.09386](http://arxiv.org/2406.09386)

## Funding

Open Access funding enabled and organized by Projekt DEAL. This work was supported in part by the Federal Ministry of Education and Research of Germany (BMBF) through AutoKoWAT-3DMAt under Grant 13N16336, in part by German Research Foundation (DFG) through SEMIAC under Grant 502,483,052, and in part by European Regional Development Fund (ERDF) through ENABLING under Grant ZS/2023/12/182,056) and through RoboLab under Grant ZS/2023/12/182065.

## Additional information

### Publisher's Note

Springer Nature remains neutral with regard to jurisdictional claims in published maps and institutional affiliations.

## Rights and permissions

**Open Access** This article is licensed under a Creative Commons Attribution 4.0 International License, which permits use, sharing, adaptation, distribution and reproduction in any medium or format, as long as you give appropriate credit to the original author(s) and the source, provide a link to the Creative Commons licence, and indicate if changes were made. The images or other third party material in this article are included in the article’s Creative Commons licence, unless indicated otherwise in a credit line to the material. If material is not included in the article’s Creative Commons licence and your intended use is not permitted by statutory regulation or exceeds the permitted use, you will need to obtain permission directly from the copyright holder. To view a copy of this licence, visit [http://creativecommons.org/licenses/by/4.0/](http://creativecommons.org/licenses/by/4.0/).

[^1]: Ahmadyan A, Zhang L, Wei J, Ablavatski A, Grundmann M (2020) Objectron: a large scale dataset of object-centric videos in the wild with pose annotations. In: 2021 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 7818–7827

[^2]: Akar CA, Tekli J, Khalil J, Yaghi A, Haddad Y, Makhoul A, Kamradt M (2024) Sordi.ai: large-scale synthetic object recognition dataset generation for industries. Multimed Tools Appl 84(17):18263–18304

[Article](https://link.springer.com/doi/10.1007/s11042-024-19731-6) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Sordi.ai%3A%20large-scale%20synthetic%20object%20recognition%20dataset%20generation%20for%20industries&journal=Multimed%20Tools%20Appl&doi=10.1007%2Fs11042-024-19731-6&volume=84&issue=17&pages=18263-18304&publication_year=2024&author=Akar%2CCA&author=Tekli%2CJ&author=Khalil%2CJ&author=Yaghi%2CA&author=Haddad%2CY&author=Makhoul%2CA&author=Kamradt%2CM)

[^3]: Anagnostopoulou D, Retsinas G, Efthymiou N, Filntisis PP, Maragos P (2023) A realistic synthetic mushroom scenes dataset. In: 2023 IEEE/CVF Conference on computer vision and pattern recognition workshops (CVPRW), pp 6282–6289

[^4]: Bai K, Zhang L, Chen Z, Wan F, Zhang J (2024) Close the sim2real gap via physically-based structured light synthetic data simulation. In: 2024 IEEE international conference on robotics and automation (ICRA), pp 17035–17041

[^5]: Barragan JA, Zhang J, Zhou H, Munawar A, Kazanzides P (2024) Realistic data generation for 6d pose estimation of surgical instruments. In: 2024 IEEE international conference on robotics and automation (ICRA), pp 13347–13353

[^6]: Barra S, Marras M, Mohamed S, Podda AS, Saia R (2023) Can existing 3d monocular object detection methods work in roadside contexts? a reproducibility study. In: International conference of the italian association for artificial intelligence. [https://api.semanticscholar.org/CorpusID:265051747](https://api.semanticscholar.org/CorpusID:265051747)

[^7]: Blender Institute (2024): Blender. [https://www.blender.org/](https://www.blender.org/)

[^8]: Blomqvist K, Chung JJ, Ott L, Siegwart RY (2022) Semi-automatic 3d object keypoint annotation and detection for the masses. In: 2022 26th International conference on pattern recognition (ICPR), pp 3908–3914

[^9]: Blomqvist K, Chung JJ, Ott L, Siegwart RY (2023) Nerfing it: offline object segmentation through implicit modeling. In: 2023 IEEE International conference on robotics and automation (ICRA), pp 9407–9413

[^10]: Borkman S, Crespi A, Dhakad S, Ganguly S, Hogins J, Jhang YC, Kamalzadeh M, Li B, Leal S, Parisi P, Romero C, Smith W, Thaman A, Warren S, Yadav N (2021) Unity perception: generate synthetic data for computer vision. [arXiv:abs/2107.04259](http://arxiv.org/2107.04259)

[^11]: Brazil G, Kumar A, Straub J, Ravi N, Johnson J, Gkioxari G (2022) Omni3d: a large benchmark and model for 3d object detection in the wild. In: 2023 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 13154–13164

[^12]: Caesar H, Bankiti V, Lang AH, Vora S, Liong VE, Xu Q, Krishnan A, Pan Y, Baldan G, Beijbom O (2019) Nuscenes: a multimodal dataset for autonomous driving. In: 2020 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 11618–11628

[^13]: Çalli B, Singh A, Walsman A, Srinivasa SS, Abbeel P, Dollar AM (2015) The ycb object and model set: towards common benchmarks for manipulation research. In: 2015 international conference on advanced robotics (ICAR), pp 510–517

[^14]: Carta S, Castrillón-Santana M, Marras M, Mohamed S, Podda AS, Saia R, Sau M, Zimmer W (2024) Roadsense3d: a framework for roadside monocular 3d object detection. In: Adjunct Proceedings of the 32nd ACM conference on user modeling, adaptation and personalization

[^15]: Chen T, Ren T, Niu J, Li Q (2021) A novel shape-based robotic sorting approach based on computer vision. In: 2021 IEEE Intl Conf on Parallel & Distributed Processing with Applications, Big Data & Cloud Computing, Sustainable Computing & Communications, Social Computing & Networking (ISPA/BDCloud/SocialCom/SustainCom), pp 660–667

[^16]: Chen K, Xie E, Chen Z, Hong L, Li Z, Yeung D-Y (2023) Geodiffusion: text-prompted geometric control for object detection data generation. In: International Conference on Learning Representations. [https://api.semanticscholar.org/CorpusID:259096176](https://api.semanticscholar.org/CorpusID:259096176)

[^17]: Dai A, Chang AX, Savva M, Halber M, Funkhouser TA, Nießner M (2017) Scannet: richly-annotated 3d reconstructions of indoor scenes. In: 2017 IEEE conference on computer vision and pattern recognition (CVPR), pp 2432–2443

[^18]: Denninger M, Sundermeyer M, Winkelbauer D, Olefir D, Hodan T, Zidan Y, Elbadrawy M, Knauer MW, Katam H, Lodhi A (2020) Blenderproc: reducing the reality gap with photorealistic rendering

[^19]: Denninger M, Winkelbauer D, Sundermeyer M, Boerdijk W, Knauer M, Strobl KH, Humt M, Triebel R (2023) Blenderproc2: a procedural pipeline for photorealistic rendering. J Open Source Softw 8:4901

[Article](https://doi.org/10.21105%2Fjoss.04901) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Blenderproc2%3A%20a%20procedural%20pipeline%20for%20photorealistic%20rendering&journal=J%20Open%20Source%20Softw&doi=10.21105%2Fjoss.04901&volume=8&publication_year=2023&author=Denninger%2CM&author=Winkelbauer%2CD&author=Sundermeyer%2CM&author=Boerdijk%2CW&author=Knauer%2CM&author=Strobl%2CKH&author=Humt%2CM&author=Triebel%2CR)

[^20]: Doruk AE, Ozkaya TE, Gülmez F, Uslu F (2023) A comparative study for 6d pose estimation of textureless and symmetric objects used in automotive manufacturing industry. In: 2023 5th International Congress on Human-Computer Interaction, Optimization and Robotic Applications (HORA), pp 1–7

[^21]: Dosovitskiy A, Ros G, Codevilla F, López AM, Koltun V (2017) Carla: an open urban driving simulator. In: Conference on Robot Learning. [https://api.semanticscholar.org/CorpusID:5550767](https://api.semanticscholar.org/CorpusID:5550767)

[^22]: Drobnitzky M, Friederich J, Egger B, Zschech P (2022) Survey and systematization of 3d object detection models and methods. [arXiv:abs/2201.09354](http://arxiv.org/2201.09354)

[^23]: Epic Games (2024): Unreal Engine. [https://www.unrealengine.com](https://www.unrealengine.com/)

[^24]: Filipovic B, Sikic F, Kalafatic Z, Loncaric S, Subavsic M (2023) Detection of tea box orientations in retail shelves images. In: 2023 International symposium on image and signal processing and analysis (ISPA), pp 1–5

[^25]: Firman M (2016) Rgbd datasets: past, present and future. In: 2016 IEEE conference on computer vision and pattern recognition workshops (CVPRW), pp 661–673

[^26]: Gaidon A, Wang Q, Cabon Y, Vig E (2016) Virtualworlds as proxy for multi-object tracking analysis. In: 2016 IEEE conference on computer vision and pattern recognition (CVPR), pp 4340–4349

[^27]: Gao R, Chen K, Xie E, Hong L, Li Z, Yeung D-Y, Xu Q (2023) Magicdrive: street view generation with diverse 3d geometry control. [arXiv:abs/2310.02601](http://arxiv.org/2310.02601)

[^28]: Gao R, Chen K, Xiao B, Hong L, Li Z, Xu Q (2024) Magicdrive-v2: high-resolution long video generation for autonomous driving with adaptive control. [https://api.semanticscholar.org/CorpusID:274165928](https://api.semanticscholar.org/CorpusID:274165928)

[^29]: Garcia-Garcia A, Martinez-Gonzalez P, Oprea S, Castro-Vargas JA, Orts S, Rodríguez JG, Jover-Alvarez A (2018) The robotrix: an extremely photorealistic and very-large-scale indoor dataset of sequences with robot trajectories and interactions. In: 2018 IEEE/RSJ International conference on intelligent robots and systems (IROS), pp 6790–6797 (2018)

[^30]: Geiger A, Lenz P, Urtasun R (2012) Are we ready for autonomous driving? the kitti vision benchmark suite. In: 2012 IEEE conference on computer vision and pattern recognition, pp 3354–3361

[^31]: Ge Y, Tang Y, Xu J, Gokmen C, Li C, Ai W, Martinez BJ, Aydin A, Anvari M, Chakravarthy AK, Yu H-X, Wong J, Srivastava S, Lee S, Zha SC, Itti L, Li Y, Mart’in-Mart’in R, Liu M, Zhang P, Zhang R, Li F-F, Wu J (2024) Behavior vision suite: customizable dataset generation via simulation. In: 2024 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), pp 22401–22412

[^32]: Grau O, Hagn K (2023) Valerie22 - a photorealistic, richly metadata annotated dataset of urban environments. Proceedings of the 7th ACM computer science in cars symposium

[^33]: Greff K, Belletti F, Beyer L, Doersch C, Du Y, Duckworth D, Fleet DJ, Gnanapragasam D, Golemo F, Herrmann C, Kipf T, Kundu A, Lagun D, Laradji IH, Liu H-T, Meyer H, Miao Y, Nowrouzezahrai D, Oztireli C, Pot E, Radwan N, Rebain D, Sabour S, Sajjadi MSM, Sela M, Sitzmann V, Stone A, Sun D, Vora S, Wang Z, Wu T, Yi KM, Zhong F, Tagliasacchi A (2022) Kubric: a scalable dataset generator. In: 2022 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 3739–3751

[^34]: Guo A, Wen B, Yuan J, Tremblay J, Tyree S, Smith J, Birchfield S (2023) Handal: a dataset of real-world manipulable object categories with pose annotations, affordances, and reconstructions. In: 2023 IEEE/RSJ international conference on intelligent robots and systems (IROS), pp 11428–11435

[^35]: Hempel T, Abdelrahman AA, Al-Hamadi A (2023) Toward robust and unconstrained full range of rotation head pose estimation. IEEE Trans Image Process 33:2377–2387

[Article](https://doi.org/10.1109%2FTIP.2024.3378180) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Toward%20robust%20and%20unconstrained%20full%20range%20of%20rotation%20head%20pose%20estimation&journal=IEEE%20Trans%20Image%20Process&doi=10.1109%2FTIP.2024.3378180&volume=33&pages=2377-2387&publication_year=2023&author=Hempel%2CT&author=Abdelrahman%2CAA&author=Al-Hamadi%2CA)

[^36]: Hodan T, Haluza P, Obdrzálek S, Matas J, Lourakis MIA, Zabulis X (2017) T-less: an rgb-d dataset for 6d pose estimation of texture-less objects. In: 2017 IEEE winter conference on applications of computer vision (WACV), pp 880–888

[^37]: Hodan T, Vineet V, Gal R, Shalev E, Hanzelka J, Connell T, Urbina P, Sinha SN, Guenter BK (2019) Photorealistic image synthesis for object instance detection. In: 2019 IEEE international conference on image processing (ICIP), pp 66–70

[^38]: Hodan T, Sundermeyer M, Drost B, Labbé Y, Brachmann E, Michel F, Rother C, Matas J (2020) Bop challenge 2020 on 6d object localization. [arXiv:abs/2009.07378](http://arxiv.org/2009.07378)

[^39]: Hu Y, Fang S, Xie W, Chen S (2022) Aerial monocular 3d object detection. IEEE Robot Autom Lett 8:1959–1966

[Article](https://doi.org/10.1109%2FLRA.2023.3245421) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Aerial%20monocular%203d%20object%20detection&journal=IEEE%20Robot%20Autom%20Lett&doi=10.1109%2FLRA.2023.3245421&volume=8&pages=1959-1966&publication_year=2022&author=Hu%2CY&author=Fang%2CS&author=Xie%2CW&author=Chen%2CS)

[^40]: Huang R, Zheng H, Wang Y, Xia Z, Pavone M, Huang G (2024) Training an open-vocabulary monocular 3d detection model without 3d data. In: The Thirty-eighth annual conference on neural information processing systems

[^41]: Jalal M, Spjut JB, Boudaoud B, Betke M (2019) Sidod: a synthetic image dataset for 3d object pose recognition with distractors. In: 2019 IEEE/CVF conference on computer vision and pattern recognition workshops (CVPRW), pp 475–477

[^42]: Kar A, Prakash A, Liu M-Y, Cameracci E, Yuan J, Rusiniak M, Acuna D, Torralba A, Fidler S (2019) Meta-sim: learning to generate synthetic datasets. In: 2019 IEEE/CVF international conference on computer vision (ICCV), pp 4550–4559

[^43]: Kaskman R, Zakharov S, Shugurov IS, Ilic S (2019) Homebreweddb: rgb-d dataset for 6d pose estimation of 3d objects. In: 2019 IEEE/CVF international conference on computer vision workshop (ICCVW), pp 2767–2776

[^44]: Kehl W, Manhardt F, Tombari F, Ilic S, Navab N (2017) Ssd-6d: making rgb-based 3d detection and 6d pose estimation great again. In: 2017 IEEE international conference on computer vision (ICCV), pp 1530–1538

[^45]: Kerbl B, Kopanas G, Leimkuehler T, Drettakis G (2023) 3d gaussian splatting for real-time radiance field rendering. ACM Trans Gr (TOG) 42:1–14

[Article](https://doi.org/10.1145%2F3592433) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=3d%20gaussian%20splatting%20for%20real-time%20radiance%20field%20rendering&journal=ACM%20Trans%20Gr%20%28TOG%29&doi=10.1145%2F3592433&volume=42&pages=1-14&publication_year=2023&author=Kerbl%2CB&author=Kopanas%2CG&author=Leimkuehler%2CT&author=Drettakis%2CG)

[^46]: Kim S-H, Hwang Y (2021) A survey on deep learning based methods and datasets for monocular 3d object detection. Electronics 10:517

[Article](https://doi.org/10.3390%2Felectronics10040517) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=A%20survey%20on%20deep%20learning%20based%20methods%20and%20datasets%20for%20monocular%203d%20object%20detection&journal=Electronics&doi=10.3390%2Felectronics10040517&volume=10&publication_year=2021&author=Kim%2CS-H&author=Hwang%2CY)

[^47]: Kirillov A, Mintun E, Ravi N, Mao H, Rolland C, Gustafson L, Xiao T, Whitehead S, Berg AC, Lo W-Y, Dollár P, Girshick RB (2023) Segment anything. In: 2023 IEEE/CVF international conference on computer vision (ICCV), pp 3992–4003

[^48]: Kleeberger K, Landgraf C, Huber MF (2019) Large-scale 6d object pose estimation dataset for industrial bin-picking. In: 2019 IEEE/RSJ international conference on intelligent robots and systems (IROS), pp 2573–2578

[^49]: Knitt M, Schyga J, Adamanov A, Hinckeldeyn J, Kreutzfeldt J (2022) Estimating the pose of a euro pallet with an rgb camera based on synthetic training data. [arXiv:abs/2210.06001](http://arxiv.org/2210.06001)

[^50]: Kolve E, Mottaghi R, Han W, VanderBilt E, Weihs L, Herrasti A, Deitke M, Ehsani K, Gordon D, Zhu Y, Kembhavi A, Gupta AK, Farhadi A (2017) Ai2-thor: an interactive 3d environment for visual AI. [arXiv:abs/1712.05474](http://arxiv.org/1712.05474)

[^51]: Lee T, Lee B-U, Shin I, Choe J, Shin U, Kweon I-S, Yoon K-J (2021) Uda-cope: unsupervised domain adaptation for category-level object pose estimation. In: 2022 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 14871–14880

[^52]: Li X, Cao R, Feng Y, Chen K, Yang B, Fu C-W, Li Y, Dou Q, Liu Y, Heng P-A (2022) A sim-to-real object recognition and localization framework for industrial robotic bin picking. IEEE Robot Autom Lett 7:3961–3968

[Article](https://doi.org/10.1109%2FLRA.2022.3149026) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=A%20sim-to-real%20object%20recognition%20and%20localization%20framework%20for%20industrial%20robotic%20bin%20picking&journal=IEEE%20Robot%20Autom%20Lett&doi=10.1109%2FLRA.2022.3149026&volume=7&pages=3961-3968&publication_year=2022&author=Li%2CX&author=Cao%2CR&author=Feng%2CY&author=Chen%2CK&author=Yang%2CB&author=Fu%2CC-W&author=Li%2CY&author=Dou%2CQ&author=Liu%2CY&author=Heng%2CP-A)

[^53]: Lian X, Yu Z, Liang R, Wang Y, Luo LR, Chen K, Zhou Y, Tang Q, Xu X, Lyu Z, Dai B, Pang J (2025) Infinite mobility: scalable high-fidelity synthesis of articulated objects via procedural generation. [arXiv:abs/2503.13424](http://arxiv.org/2503.13424)

[^54]: Lindermayr J, Odabaşi C, Jordan F, Graf F, Knak L, Kraus W, Bormann R, Huber MF (2023) Ipa-3d1k: a large retail 3d model dataset for robot picking. In: 2023 IEEE/RSJ International conference on intelligent robots and systems (IROS), pp 11404–11411

[^55]: Lin H, Guo Z, Zhang Y, Niu S, Li Y, Zhang R, Cui S, Li Z (2025) Drivegen: generalized and robust 3d detection in driving via controllable text-to-image diffusion generation. [arXiv:abs/2503.11122](http://arxiv.org/2503.11122)

[^56]: Li W, Saeedi S, McCormac J, Clark R, Tzoumanikas D, Ye Q, Huang Y, Tang R, Leutenegger S (2018) Interiornet: mega-scale multi-sensor photo-realistic indoor scenes dataset. [arXiv:abs/1809.00716](http://arxiv.org/1809.00716)

[^57]: Liu Z, Wu Z, T’oth R (2020) Smoke: single-stage monocular 3d object detection via keypoint estimation. In: 2020 IEEE/CVF conference on computer vision and pattern recognition workshops (CVPRW), pp 4289–4298

[^58]: Liu X, Hao S, Xu K (2023) Pose estimation of space targets based on geometry structure features. In: Proceedings of the 2023 2nd Asia conference on algorithms, computing and machine learning

[^59]: Liu Y, Chen W, Bai Y, Luo J-H, Song X, Jiang K, Li Z, Zhao G, Lin J, Li G, Gao W, Lin L (2024) Aligning cyber space with physical world: a comprehensive survey on embodied AI. [arXiv:abs/2407.06886](http://arxiv.org/2407.06886)

[^60]: Li C, Xia F, Mart’in-Mart’in R, Lingelbach M, Srivastava S, Shen B, Vainio K, Gokmen C, Dharan G, Jain T, Kurenkov A, Liu K, Gweon H, Wu J, Fei-Fei L, Savarese S (2021) igibson 2.0: object-centric simulation for robot learning of everyday household tasks. [arXiv:abs/2108.03272](http://arxiv.org/2108.03272)

[^61]: Li C, Zhang R, Wong J, Gokmen C, Srivastava S, Martín-Martín R, Wang C, Levine G, Lingelbach M, Sun J, Anvari M, Hwang M, Sharma M, Aydin A, Bansal D, Hunter S, Kim K-Y, Lou A, Matthews CR, Villa-Renteria I, Tang JH, Tang C, Xia F, Savarese S, Gweon H, Liu CK, Wu J, Fei-Fei L (2022) Behavior-1k: a benchmark for embodied ai with 1, 000 everyday activities and realistic simulation. In: Conference on Robot Learning. [https://api.semanticscholar.org/CorpusID:255198985](https://api.semanticscholar.org/CorpusID:255198985)

[^62]: Li X, Zhang Y, Ye X (2023) Drivingdiffusion: layout-guided multi-view driving scene video generation with latent diffusion model. [arXiv:abs/2310.07771](http://arxiv.org/2310.07771)

[^63]: Manhardt F, Wang G, Busam B, Nickel M, Meier S, Minciullo L, Ji X, Navab N (2020) Cps++: Improving class-level 6d pose and shape estimation from monocular images with self-supervised learning. Computer Vision and Pattern Recognition

[^64]: Mao J, Shi S, Wang X, Li H (2022) 3d object detection for autonomous driving: a comprehensive survey. Int J Comput Vision 131:1909–1963

[Article](https://link.springer.com/doi/10.1007/s11263-023-01790-1) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=3d%20object%20detection%20for%20autonomous%20driving%3A%20a%20comprehensive%20survey&journal=Int%20J%20Comput%20Vision&doi=10.1007%2Fs11263-023-01790-1&volume=131&pages=1909-1963&publication_year=2022&author=Mao%2CJ&author=Shi%2CS&author=Wang%2CX&author=Li%2CH)

[^65]: Martinez-Gonzalez P, Oprea S, Garcia-Garcia A, Jover-Alvarez A, Orts-Escolano S, Garcia-Rodriguez J (2018) Unrealrox an extremely photorealistic virtual reality environment for robotics simulations and synthetic data generation. [https://api.semanticscholar.org/CorpusID:53111294](https://api.semanticscholar.org/CorpusID:53111294)

[^66]: Martinez-Gonzalez P, Oprea S, Castro-Vargas JA, Garcia-Garcia A, Orts-Escolano S, Garcia-Rodriguez J, Vincze M (2021) Unrealrox+: an improved tool for acquiring synthetic data from virtual 3d environments. In: 2021 International joint conference on neural networks (IJCNN), pp 1–8

[^67]: Mata C, Locascio N, Sheikh MA, Kihara K, Fischetti DL (2022) Standardsim: a synthetic dataset for retail environments. In: International conference on image analysis and processing. [https://api.semanticscholar.org/CorpusID:246634940](https://api.semanticscholar.org/CorpusID:246634940)

[^68]: McCormac J, Handa A, Leutenegger S, Davison AJ (2016) Scenenet rgb-d: 5m photorealistic images of synthetic indoor trajectories with ground truth. [arXiv:abs/1612.05079](http://arxiv.org/1612.05079)

[^69]: Mehr G, Eskandarian A (2025) Simbev: a synthetic multi-task multi-sensor driving data generation tool and dataset. [arXiv:abs/2502.01894](http://arxiv.org/2502.01894)

[^70]: Meier J, Scalerandi L, Dhaouadi O, Kaiser J, Araslanov N, Cremers D (2024) Carla drone: monocular 3d object detection from a different perspective. [arXiv:abs/2408.11958](http://arxiv.org/2408.11958)

[^71]: Mildenhall B, Srinivasan PP, Tancik M, Barron JT, Ramamoorthi R, Ng R (2020) Nerf: representing scenes as neural radiance fields for view synthesis. In: Vedaldi A, Bischof H, Brox T, Frahm J-M (eds) Computer vision - ECCV 2020. Springer, Cham, pp 405–421

[Chapter](https://link.springer.com/doi/10.1007/978-3-030-58452-8_24) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Nerf%3A%20representing%20scenes%20as%20neural%20radiance%20fields%20for%20view%20synthesis&doi=10.1007%2F978-3-030-58452-8_24&pages=405-421&publication_year=2020&author=Mildenhall%2CB&author=Srinivasan%2CPP&author=Tancik%2CM&author=Barron%2CJT&author=Ramamoorthi%2CR&author=Ng%2CR)

[^72]: Morrical N, Tremblay J, Lin Y, Tyree S, Birchfield S, Pascucci V, Wald I (2021) Nvisii: a scriptable tool for photorealistic image generation. [arXiv:abs/2105.13962](http://arxiv.org/2105.13962)

[^73]: Müller M, Casser V, Lahoud J, Smith NG, Ghanem B (2017) Sim4cv: a photo-realistic simulator for computer vision applications. Int J Comput Vision 126:902–919

[Article](https://link.springer.com/doi/10.1007/s11263-018-1073-7) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Sim4cv%3A%20a%20photo-realistic%20simulator%20for%20computer%20vision%20applications&journal=Int%20J%20Comput%20Vision&doi=10.1007%2Fs11263-018-1073-7&volume=126&pages=902-919&publication_year=2017&author=M%C3%BCller%2CM&author=Casser%2CV&author=Lahoud%2CJ&author=Smith%2CNG&author=Ghanem%2CB)

[^74]: Muller R, Man Y, Celik ZB, Li MH, Gerdes RM (2022) Drivetruth: automated autonomous driving dataset generation for security applications. In: Proceedings fourth international workshop on automotive and autonomous vehicle security

[^75]: NVIDIA (2022) NVIDIA Isaac Sim. [https://developer.nvidia.com/isaac/sim](https://developer.nvidia.com/isaac/sim). Accessed: 05, 03, 2024

[^76]: NVIDIA (2024): NVIDIA Omniverse. [https://www.nvidia.com/en-us/omniverse/](https://www.nvidia.com/en-us/omniverse/)

[^77]: Paulin G, Ivasic-Kos M (2023) Review and analysis of synthetic dataset generation methods and techniques for application in computer vision. Artif Intell Rev 56(9):1–45

[Article](https://link.springer.com/doi/10.1007/s10462-022-10358-3) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Review%20and%20analysis%20of%20synthetic%20dataset%20generation%20methods%20and%20techniques%20for%20application%20in%20computer%20vision&journal=Artif%20Intell%20Rev&doi=10.1007%2Fs10462-022-10358-3&volume=56&issue=9&pages=1-45&publication_year=2023&author=Paulin%2CG&author=Ivasic-Kos%2CM)

[^78]: Prakash A, Boochoon S, Brophy M, Acuna D, Cameracci E, State G, Shapira O, Birchfield S (2018) Structured domain randomization: bridging the reality gap by context-aware synthetic data. In: 2019 International conference on robotics and automation (ICRA), pp 7249–7255

[^79]: Proença PF, Gao Y (2019) Deep learning for spacecraft pose estimation from photorealistic rendering. In: 2020 IEEE international conference on robotics and automation (ICRA), pp 6007–6013

[^80]: Pugh B, Chernak D, Jiddi S (2023) Geosynth: a photorealistic synthetic indoor dataset for scene understanding. IEEE Trans Visual Comput Gr 29:2586–2595

[Article](https://doi.org/10.1109%2FTVCG.2023.3247087) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Geosynth%3A%20a%20photorealistic%20synthetic%20indoor%20dataset%20for%20scene%20understanding&journal=IEEE%20Trans%20Visual%20Comput%20Gr&doi=10.1109%2FTVCG.2023.3247087&volume=29&pages=2586-2595&publication_year=2023&author=Pugh%2CB&author=Chernak%2CD&author=Jiddi%2CS)

[^81]: Qiu W, Yuille AL (2016) Unrealcv: connecting computer vision to unreal engine. [arXiv:abs/1609.01326](http://arxiv.org/1609.01326)

[^82]: Rad M, Lepetit V (2017) Bb8: a scalable, accurate, robust to partial occlusion method for predicting the 3d poses of challenging objects without using depth. In: 2017 IEEE international conference on computer vision (ICCV), pp 3848–3856

[^83]: Raistrick ARE, Lipson L, Ma Z, Mei L, Wang M, Zuo Y, Kayan K, Wen H, Han B, Wang Y, Newell A, Law H, Goyal A, Yang K, Deng J (2023) Infinite photorealistic worlds using procedural generation. In: 2023 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), pp 12630–12641

[^84]: Raistrick ARE, Mei L, Kayan K, Yan D, Zuo Y, Han B, Wen H, Parakh M, Alexandropoulos S, Lipson L, Ma Z, Deng J (2024) Infinigen indoors: photorealistic indoor scenes using procedural generation. In: 2024 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 21783–21794

[^85]: Remus A, D’Avella S, Felice FD, Tripicchio P, Avizzano CA (2023) i2c-net: using instance-level neural networks for monocular category-level 6d pose estimation. IEEE Robot Autom Lett 8:1515–1522

[Article](https://doi.org/10.1109%2FLRA.2023.3240362) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=i2c-net%3A%20using%20instance-level%20neural%20networks%20for%20monocular%20category-level%206d%20pose%20estimation&journal=IEEE%20Robot%20Autom%20Lett&doi=10.1109%2FLRA.2023.3240362&volume=8&pages=1515-1522&publication_year=2023&author=Remus%2CA&author=D%E2%80%99Avella%2CS&author=Felice%2CFD&author=Tripicchio%2CP&author=Avizzano%2CCA)

[^86]: Ren T, Liu S, Zeng A, Lin J, Li K, Cao H, Chen J, Huang X, Chen Y, Yan F, Zeng Z, Zhang H, Li F, Yang J, Li H, Jiang Q, Zhang L (2024) Grounded sam: assembling open-world models for diverse visual tasks. [arXiv:abs/2401.14159](http://arxiv.org/2401.14159)

[^87]: Richter SR, Hayder Z, Koltun V (2017) Playing for benchmarks. In: 2017 IEEE International conference on computer vision (ICCV), pp 2232–2241

[^88]: Roberts M, Paczan N (2020) Hypersim: a photorealistic synthetic dataset for holistic indoor scene understanding. In: 2021 IEEE/CVF International conference on computer vision (ICCV), pp 10892–10902

[^89]: Rombach R, Blattmann A, Lorenz D, Esser P, Ommer B (2021) High-resolution image synthesis with latent diffusion models. In: 2022 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 10674–10685

[^90]: Rong G, Shin BH, Tabatabaee H, Lu Q, Lemke S, Mozeiko M, Boise E, Uhm G, Gerow M, Mehta S, Agafonov E, Kim TH, Sterner E, Ushiroda K, Reyes M, Zelenkovsky D, Kim S (2020) Lgsvl simulator: a high fidelity simulator for autonomous driving. In: 2020 IEEE 23rd international conference on intelligent transportation systems (ITSC), pp 1–6

[^91]: Ros G, Sellart L, Materzynska J, Vázquez D, López AM (2016) The synthia dataset: a large collection of synthetic images for semantic segmentation of urban scenes. In: 2016 IEEE conference on computer vision and pattern recognition (CVPR), pp 3234–3243

[^92]: SAE (2016) Taxonomy and definitions for terms related to driving automation systems for on-road motor vehicles

[^93]: Schieber H, Demir KC, Kleinbeck C, Yang SH, Roth D (2024) Indoor synthetic data generation: a systematic review. Comput Vis Image Underst 240:103907

[Article](https://doi.org/10.1016%2Fj.cviu.2023.103907) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Indoor%20synthetic%20data%20generation%3A%20a%20systematic%20review&journal=Comput%20Vis%20Image%20Underst&doi=10.1016%2Fj.cviu.2023.103907&volume=240&publication_year=2024&author=Schieber%2CH&author=Demir%2CKC&author=Kleinbeck%2CC&author=Yang%2CSH&author=Roth%2CD)

[^94]: Shah S, Dey D, Lovett C, Kapoor A (2017) Airsim: high-fidelity visual and physical simulation for autonomous vehicles. In: International Symposium on Field and Service Robotics. [https://api.semanticscholar.org/CorpusID:20999239](https://api.semanticscholar.org/CorpusID:20999239)

[^95]: Sharma S, Beierle C, D’Amico S (2018) Pose estimation for non-cooperative spacecraft rendezvous using convolutional neural networks. In: 2018 IEEE Aerospace Conference, pp 1–12

[^96]: Simoni A, Pelosin F (2025) Bounding box-guided diffusion for synthesizing industrial images and segmentation map. [https://api.semanticscholar.org/CorpusID:278339348](https://api.semanticscholar.org/CorpusID:278339348)

[^97]: Song S, Lichtenberg SP, Xiao J (2015) Sun rgb-d: a rgb-d scene understanding benchmark suite. In: 2015 IEEE conference on computer vision and pattern recognition (CVPR), pp 567–576

[^98]: Strazdas D, Hintz J, Felßberg A-M, Al-Hamadi A (2020) Robots and wizards: an investigation into natural human-robot interaction. IEEE Access 8:207635–207642

[Article](https://doi.org/10.1109%2FACCESS.2020.3037724) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Robots%20and%20wizards%3A%20an%20investigation%20into%20natural%20human-robot%20interaction&journal=IEEE%20Access&doi=10.1109%2FACCESS.2020.3037724&volume=8&pages=207635-207642&publication_year=2020&author=Strazdas%2CD&author=Hintz%2CJ&author=Fel%C3%9Fberg%2CA-M&author=Al-Hamadi%2CA)

[^99]: Strazdas D, Hintz J, Khalifa A, Abdelrahman AA, Hempel T, Al-Hamadi A (2022) Robot system assistant (Rosa): towards intuitive multi-modal and multi-device human-robot interaction. Sens 22(3):923

[Article](https://doi.org/10.3390%2Fs22030923) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Robot%20system%20assistant%20%28Rosa%29%3A%20towards%20intuitive%20multi-modal%20and%20multi-device%20human-robot%20interaction&journal=Sens&doi=10.3390%2Fs22030923&volume=22&issue=3&publication_year=2022&author=Strazdas%2CD&author=Hintz%2CJ&author=Khalifa%2CA&author=Abdelrahman%2CAA&author=Hempel%2CT&author=Al-Hamadi%2CA)

[^100]: Sun T, Segu M, Postels J, Wang Y, Gool LV, Schiele B, Tombari F, Yu F (2022) Shift: a synthetic driving dataset for continuous multi-task domain adaptation. In: 2022 IEEE/CVF Conference on computer vision and pattern recognition (CVPR), pp 21339–21350

[^101]: Sural S, Sahu N, Rajkumar R (2024) Contextualfusion: context-based multi-sensor fusion for 3d object detection in adverse operating conditions. In: 2024 IEEE intelligent vehicles symposium (IV), pp 1534–1541

[^102]: Tang J, Ren J, Zhou H, Liu Z, Zeng G (2023a) Dreamgaussian: generative gaussian splatting for efficient 3d content creation. [arXiv:abs/2309.16653](http://arxiv.org/2309.16653)

[^103]: Tang J, Zhou H, Chen X, Hu T, Ding E, Wang J, Zeng G (2023b) Delicate textured mesh recovery from nerf via adaptive surface refinement. In: 2023 IEEE/CVF international conference on computer vision (ICCV), pp 17693–17703

[^104]: Tang J, Chen Z, Fu B, Lu W, Li S, Li X, Ji X (2024a) Rov6d: 6d pose estimation benchmark dataset for underwater remotely operated vehicles. IEEE Robot Autom Lett 9:65–72

[Article](https://doi.org/10.1109%2FLRA.2023.3331624) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Rov6d%3A%206d%20pose%20estimation%20benchmark%20dataset%20for%20underwater%20remotely%20operated%20vehicles&journal=IEEE%20Robot%20Autom%20Lett&doi=10.1109%2FLRA.2023.3331624&volume=9&pages=65-72&publication_year=2024&author=Tang%2CJ&author=Chen%2CZ&author=Fu%2CB&author=Lu%2CW&author=Li%2CS&author=Li%2CX&author=Ji%2CX)

[^105]: Tang Y, Tai C, Chen F-X, Zhang W, Zhang T, Liu X-P, Liu Y-J, Zeng L (2024b) Mobile robot oriented large-scale indoor dataset for dynamic scene understanding. In: 2024 IEEE international conference on robotics and automation (ICRA), pp 613–620

[^106]: Teufel S, Gamerdinger J, Kirchner J-P, Volk G, Bringmann O (2024) Collective perception datasets for autonomous driving: a comprehensive review. In: 2024 IEEE intelligent vehicles symposium (IV), pp 1548–1555

[^107]: Tonderski A, Lindström C, Hess G, Ljungbergh W, Svensson L, Petersson C (2023) Neurad: neural rendering for autonomous driving. In: 2024 IEEE/CVF Conference on computer vision and pattern recognition (CVPR), pp 14895–14904

[^108]: Tong W, Xie J, Li T, Deng H, Geng X, Zhou R, Yang D, Dai B, Lu L, Li H (2023) 3d data augmentation for driving scenes on camera. [arXiv:abs/2303.10340](http://arxiv.org/2303.10340)

[^109]: Tong S, Ouyang Z, Hu Q, Li D (2024) 3d spacecraft position and orientation estimation model based on monocular image. In: 2024 IEEE international conference on cybernetics and intelligent systems (CIS) and IEEE international conference on robotics, automation and mechatronics (RAM), pp 508–513

[^110]: To T, Tremblay J, McKay D, Yamaguchi Y, Leung K, Balanon A, Cheng J, Hodge W, Birchfield S (2018) NDDS: NVIDIA deep learning dataset synthesizer. [https://github.com/NVIDIA/Dataset\_Synthesizer](https://github.com/NVIDIA/Dataset_Synthesizer)

[^111]: Tran QH, Choate JA, Taylor CN, Nykl SL, Curtis DH (2023) Monocular vision and machine learning for pose estimation. In: 2023 IEEE/ION Position, Location and Navigation Symposium (PLANS), pp 128–136

[^112]: Tremblay J, To T, Birchfield S (2018a) Falling things: a synthetic dataset for 3d object detection and pose estimation. In: 2018 IEEE/CVF conference on computer vision and pattern recognition workshops (CVPRW), pp 2119–21193

[^113]: Tremblay J, To T, Sundaralingam B, Xiang Y, Fox D, Birchfield S (2018b) Deep object pose estimation for semantic robotic grasping of household objects. [arXiv:abs/1809.10790](http://arxiv.org/1809.10790)

[^114]: Türkcan MK, Li Y, Zang C, Ghaderi J, Zussman G, Kostic Z (2024) Boundless: generating photorealistic synthetic data for object detection in urban streetscapes. [arXiv:abs/2409.03022](http://arxiv.org/2409.03022)

[^115]: Tyree S, Tremblay J, To T, Cheng J, Mosier T, Smith J, Birchfield S (2022) 6-dof pose estimation of household objects for robotic manipulation: an accessible dataset and benchmark. In: 2022 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), pp 13081–13088

[^116]: Unity Technologies (2024): Unity. Game development platform. [https://unity.com/](https://unity.com/)

[^117]: Wang M, Deng W (2018) Deep visual domain adaptation: a survey. [arXiv:abs/1802.03601](http://arxiv.org/1802.03601)

[^118]: Wang H, Sridhar S, Huang J, Valentin JPC, Song S, Guibas LJ (2019) Normalized object coordinate space for category-level 6d object pose and size estimation. In: 2019 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 2637–2646

[^119]: Wang G, Manhardt F, Shao J, Ji X, Navab N, Tombari F (2020) Self6d: self-supervised monocular 6d object pose estimation. [arXiv:abs/2004.06468](http://arxiv.org/2004.06468)

[^120]: Wang Y, Wang C, Long P, Gu Y, Li W (2021) Recent advances in 3d object detection based on rgb-d: a survey. Displays 70:102077

[Article](https://doi.org/10.1016%2Fj.displa.2021.102077) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Recent%20advances%20in%203d%20object%20detection%20based%20on%20rgb-d%3A%20a%20survey&journal=Displays&doi=10.1016%2Fj.displa.2021.102077&volume=70&publication_year=2021&author=Wang%2CY&author=Wang%2CC&author=Long%2CP&author=Gu%2CY&author=Li%2CW)

[^121]: Wang P, Jung H, Li Y, Shen S, Srikanth RP, Garattoni L, Meier S, Navab N, Busam B (2022) Phocal: a multi-modal dataset for category-level object pose estimation with photometrically challenging objects. In: 2022 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 21190–21199

[^122]: Wang X, Zhu Z, Huang G, Chen X, Zhu J, Lu J (2024) Drivedreamer: towards real-world-drive world models for autonomous driving. In: European conference on computer vision. [https://api.semanticscholar.org/CorpusID:274611109](https://api.semanticscholar.org/CorpusID:274611109)

[^123]: Wrenninge M, Unger J (2018) Synscapes: a photorealistic synthetic dataset for street scene parsing. [arXiv:abs/1810.08705](http://arxiv.org/1810.08705)

[^124]: Wu Z, Liu T, Luo L, Zhong Z, Chen J, Xiao H, Hou C, Lou H, Chen Y-H, Yang R, Huang Y, Ye X, Yan Z, Shi Y, Liao Y, Zhao H (2023a) Mars: an instance-aware, modular and realistic simulator for autonomous driving. [arXiv:abs/2307.15058](http://arxiv.org/2307.15058)

[^125]: Wu W, Zhao Y, Chen H, Gu Y, Zhao R, He Y, Zhou H, Shou MZ, Shen C (2023b) Datasetdm: synthesizing data with perception annotations using diffusion models. [arXiv:abs/2308.06160](http://arxiv.org/2308.06160)

[^126]: Xia F, Shen BW, Li C, Kasimbeg P, Tchapmi ME, Toshev A, Martín-Martín R, Savarese S (2019) Interactive Gibson benchmark: a benchmark for interactive navigation in cluttered environments. IEEE Robot Autom Lett 5:713–720

[Article](https://doi.org/10.1109%2FLRA.2020.2965078) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Interactive%20Gibson%20benchmark%3A%20a%20benchmark%20for%20interactive%20navigation%20in%20cluttered%20environments&journal=IEEE%20Robot%20Autom%20Lett&doi=10.1109%2FLRA.2020.2965078&volume=5&pages=713-720&publication_year=2019&author=Xia%2CF&author=Shen%2CBW&author=Li%2CC&author=Kasimbeg%2CP&author=Tchapmi%2CME&author=Toshev%2CA&author=Mart%C3%ADn-Mart%C3%ADn%2CR&author=Savarese%2CS)

[^127]: Xiang J, Yang J, Huang B, Tong X (2023) 3d-aware image generation using 2d diffusion models. In: 2023 IEEE/CVF international conference on computer vision (ICCV), pp 2383–2393

[^128]: Xie Z, Liu Z, Peng Z, Wu W, Zhou B (2025) Vid2sim: realistic and interactive simulation from video for urban navigation. [arXiv:abs/2501.06693](http://arxiv.org/2501.06693)

[^129]: Xu A, Vasileva MI, Dave A, Seshadri A (2022) Handsoff: labeled dataset generation with no additional human annotations. In: 2023 IEEE/CVF conference on computer vision and pattern recognition (CVPR), pp 7991–8000

[^130]: Yang Z, Zeng A, Yuan C, Li Y (2023a) Effective whole-body pose estimation with two-stages distillation. In: 2023 IEEE/CVF international conference on computer vision workshops (ICCVW), pp 4212–4222

[^131]: Yang Z, Zhan F, Liu K, Xu M, Lu S (2023b) Ai-generated images as data source: the dawn of synthetic era. [arXiv:abs/2310.01830](http://arxiv.org/2310.01830)

[^132]: Yi T, Fang J, Zhou Z, Wang J, Wu G, Xie L, Zhang X, Liu W, Wang X, Tian Q (2024) Gaussiandreamerpro: text to manipulable 3D Gaussians with highly enhanced quality. [arXiv:abs/2406.18462](http://arxiv.org/2406.18462)

[^133]: Zanjani FG, Abati D, Wiggers AJ, Kalatzis D, Petersen J, Cai H, Habibian A (2025) Gaussian splatting is an effective data generator for 3d object detection. [arXiv:abs/2504.16740](http://arxiv.org/2504.16740)

[^134]: Zhang L, Rao A, Agrawala M (2023) Adding conditional control to text-to-image diffusion models. In: 2023 IEEE/CVF international conference on computer vision (ICCV), pp 3813–3824

[^135]: Zhou L, Song Y, Gao Y, Yu Z, Sodamin M, Liu H, Ma L, Liu L, Liu H, Liu Y, Li H, Chen G, Knoll A (2023) Garchingsim: an autonomous driving simulator with photorealistic scenes and minimalist workflow. In: 2023 IEEE 26th international conference on intelligent transportation systems (ITSC), pp 4227–4232

[^136]: Zhou Y, Simon M, Peng Z, Mo S, Zhu H, Guo M, Zhou B (2024) Simgen: simulator-conditioned driving scene generation. [arXiv:abs/2406.09386](http://arxiv.org/2406.09386)

[^137]: Neuro-Information Technology group, Otto von Guericke University, Universitätsplatz 1, Magdeburg, 39104, Saxony Anhalt, Germany

Paul Schulz, Thorsten Hempel, Magnus Jung & Ayoub Al-Hamadi