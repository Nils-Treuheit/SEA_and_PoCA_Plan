---
title: "V2XCrafter: Learning to Generate Driving Scene Across Agents"
source: "https://arxiv.org/html/2605.29471v1"
author:
published:
created: 2026-06-03
description:
tags:
  - "clippings"
---
Yihang Tao <sup>1,2,∗</sup> Yu Guo <sup>1,2,∗</sup> Senkang Hu <sup>1,2</sup> Yanan Ma <sup>1,2</sup> Zihan Fang <sup>1,2</sup> Sam Tak Wu Kwong <sup>3</sup> Yuguang Fang <sup>1,2</sup>  
<sup>1</sup> Hong Kong JC STEM Lab of Smart City, <sup>2</sup> City University of Hong Kong, <sup>3</sup> Lingnan University

###### Abstract

Collaborative driving systems leverage vehicle-to-everything (V2X) communication for multi-agent collaborative perception to enhance driving safety, yet they remain constrained by scarce annotated real-world V2X driving datasets and limited generalization across diverse driving conditions. While image generation technology offers a feasible solution for data augmentation, existing methods tailored for single-vehicle multi-view scenarios face two fundamental challenges in multi-agent driving settings: (1) the expansion of the learning objective degrades generation quality, and (2) the highly dynamic variations across agents hinder the modeling of consistency for physical attributes (e.g., color, category) in jointly observed objects. To bridge this gap, we propose V2XCrafter, the first framework for generating controllable and realistic collaborative driving scene across agents’ camera views. For effective learning, we develop a progressive multi-agent diffusion model based on a single-agent backbone, using neighboring agents’ latent states as reference signals to progressively guide the single-to-multi diffusion. To address cross-vehicle inconsistency, we propose a cross-agent attention module that leverages a collaboration view graph and learnable jointly observed object representation to model the dynamic cross-agent camera view relationships. Experiments have shown that V2XCrafter can generate high-fidelity and controllable street views with consistency across agents, thereby effectively enhancing the downstream collaborative 3D object detection tasks.

<sup>1</sup>

## 1 Introduction

Collaborative driving systems enhance driving safety by leveraging Vehicle-to-Everything (V2X) communication for Collaborative Perception (CP) among multiple vehicles (agents) \[10979246, fangPACPPriorityAwareCollaborative2024, wei2024editable, tao2025gcpguardedcollaborativeperception, hu2024cpguardmaliciousagentdetection, ma2025sense4flvehicularcrowdsensingenhanced, tao2026learningmutualviewinformation, tao2024directcpdirectedcollaborativeperception\]. While deep learning-based CP models have advanced rapidly, their generalization ability is still heavily constrained by the scale and diversity of available annotated training data. Specifically, the acquisition and annotation process is costly and labor-intensive. Different from single-vehicle scenarios \[Sun\_2020\_CVPR\], data collection from multiple connected vehicles requires more significant operational resources for sensor calibration, data synchronization, and precise spatiotemporal alignment, resulting in datasets that are orders of magnitude scarcer \[v2x-real\]. This inherent scarcity is further exacerbated by the substantial labor required for annotating 3D scenes from multiple viewpoints. Furthermore, existing datasets exhibit large disparities in training difficulty across multiple attributes, including different traffic scenarios, weather conditions, and object distributions. Stemming from skewed sample distributions and systematic data collection biases, these imbalances compromise the model’s ability to generalize to rare and complex collaborative driving conditions.

To address these challenges, current efforts in connected and autonomous driving domain have primarily relied on data augmentation with simulation platforms \[10161384, 10979246, Pan\_2025\_CVPR, fang2026agent\]. These methods programmatically generate new training scenarios by altering parameters such as agent poses and communication conditions within the simulator like CARLA \[Dosovitskiy17\]. However, these approaches are constrained to virtual environments and thus suffer from a persistent sim-to-real domain gap. Building upon recent advancements in diffusion models for autonomous driving street-view generation \[gao2023magicdrive, wang2024drive\], another feasible solution is image generation-driven data augmentation. Such methods typically condition the generation process on camera poses and their adjacency relationships, as well as layout information such as bounding boxes and road maps \[li2023gligen, Zheng\_2023\_CVPR\]. By applying a joint initial noise across multiple views, they learn to denoise all perspectives simultaneously to generate a coherent, multi-view scene.

Despite this impressive progress, the transfer of these single-agent methods to multi-agent settings poses challenges due to inherent differences in system complexity. The primary obstacles for such an extension are twofold. ❶ First, the expansion of the learning objective. The increased number of views from multiple agents makes it more difficult to converge (see Appendix A), as the shared-noise, one-shot denoising process becomes difficult to optimize when conditioned on a large and variable number of distinct multi-agent viewpoints, leading to degraded generation quality. ❷ Second, the highly dynamic variations across vehicles (agents). Existing models are designed for a fixed intra-agent camera view pairs and are unable to model the dynamic cross-agent camera view relationships that change as vehicles (agents) move. This inability to enforce consistency across a dynamic connection of agents makes it impossible to generate a coherent global scene, which is critical for collaborative driving systems.

In response to the challenges outlined, we introduce V2XCrafter, the first framework for realistic and controllable collaborative driving scene generation across multiple vehicles (agents). Instead of learning to denoise all views from multiple agents at once, we propose a progressive multi-agent diffusion model built upon a shared single-agent diffusion backbone. This approach overcomes the challenges on the expansion of the learning objective by reformulating the generation as a two-stage progressive process: it first masters single-agent scene generation and then uses the latent states of neighboring collaborative agents as reference signals to progressively guide the diffusion from a single-agent to a multi-agent context. To further enhance cross-agent consistency, we design a novel cross-agent attention module. This module effectively models the dynamic spatial relationships between agents by leveraging a collaboration view graph and learnable jointly observed object representation, ensuring that the generated views are coherent across agents. Representative results are shown in Fig. LABEL:fig:comparison.

The main contributions of this work are threefold:

- We propose V2XCrafter, the first framework dedicated to realistic and controllable multi-agent driving scene generation with consistent cross-agent camera views.
- We introduce a novel multi-agent driving diffusion model for effective single-to-multi-agent expansion, along with a cross-agent attention module that ensures consistency across dynamic cross-agent camera views.
- Experiments have shown that our method generates high-fidelity and coherent collaborative driving scenes, and effectively boosts the performance of downstream collaborative 3D object detection tasks.

## 2 Related Work

Diffusion Models for Conditional Generation. Diffusion models have demonstrated strong cross-modal generation capabilities, particularly in Text-to-Image (T2I) synthesis \[NEURIPS2019\_1d72310e, Zhang\_2023\_ICCV, NEURIPS2022\_ec795aea\]. Pioneering works like DALL-E 2 \[ramesh2022hierarchicaltextconditionalimagegeneration\] and Imagen \[NEURIPS2022\_ec795aea\] leveraged text encoders, while Stable Diffusion \[rombach2021highresolution\] improved efficiency via latent space compression. However, text alone is insufficient for specifying precise geometric relationships (e.g., object positions and orientations). To address this, Layout-to-Image (L2I) methods integrate explicit structural constraints. Techniques such as LayoutDiff \[Zheng\_2023\_CVPR\], GLIGEN \[li2023gligen\], and Neptune-X \[guo2025neptune-x\] incorporated bounding boxes or masks for precise, instance-level control. However, these approaches treat observers independently, overlooking complex multi-observer relationships. Beyond synthesis, diffusion models demonstrate that synthetically generated data from geometric annotations can benefit downstream perception tasks like 2D object detection \[chen2023geodiffusion, guo2025neptune-x\]. Building on this, our work focuses on L2I diffusion for multi-agent collaborative driving scenes to enhance downstream 3D collaborative object detection.

Data Augmentation for V2X Collaborative Driving. To enhance collaborative driving robustness under diverse environments, existing works have explored approaches to augment multi-agent collaborative driving datasets. V2XP-ASG \[10161384\] introduced the first framework to automatically generate challenging scenarios for collaborative driving. The approach perturbs multiple agents’ LiDAR poses in adversarial yet physically plausible manners, with the perturbed configurations serving as inputs to the CARLA simulator for generating synthetic datasets. Similarly, V2Xverse \[10979246\] enabled comprehensive robustness evaluation by supporting dynamic editing of communication parameters within CARLA-based simulations, facilitating systematic assessment against practical challenges such as communication latency and pose estimation errors. Most recently, TYP \[Pan\_2025\_CVPR\] explored generative models for collaborative perception. However, it targets sparse LiDAR point clouds and relies on simulation datasets (created by CARLA) to learn perspective transfer from single-vehicle data. Despite these advances, current techniques rely exclusively on simulation-based environments or focus on sparse geometric data, largely ignoring the challenge of augmenting real-world collaborative driving datasets with dense, photorealistic multi-agent camera images.

Multi-Camera Street View Generation. For outdoor driving scenes, several methods have explored the generation of realistic and consistent surround-view image. For example, BEVGen \[swerdlow2024streetviewimagegenerationbirdseye\] utilized an auto-regressive transformer with cross-view attention to ensure visual consistency. Building on this, BEVControl \[yang2023bevcontrolaccuratelycontrollingstreetview\] introduced cross-view and cross-object attention to further augment multi-view generation consistency. More recently, MagicDrive \[gao2023magicdrive\] and MagicDrive-v2 \[gao2025magicdrive-v2\] enhanced performance by injecting cross-view attention directly into UNet block or DiT blocks. Yet, these designs are fundamentally for single-agent contexts, enforcing consistency across camera pairs with fixed and static overlaps, which is ill-suited for multi-agent scenarios characterized by dynamic and unstructured cross-agent camera view relationships. Furthermore, a challenge in achieving training convergence arises from jointly denoising a large and variable number of views when considering multi-agent driving scene generation problem.

## 3 Problem Formulation

Collaborative Driving Scene Representation. We model a collaborative driving scene with $N$ agents as $\mathcal{A}=\{A_{1},\dots,A_{N}\}$. Each agent $A_{i}$ has a global position $L_{i}$ and is equipped with a set of cameras $\mathcal{C}_{i}=\{C_{i,1},\dots,C_{i,M_{i}}\}$. Each camera $C_{i,j}$ has a pose $\mathbf{P}_{i,j}=[\mathbf{K}_{i,j},\mathbf{R}_{i,j},\mathbf{t}_{i,j}]$, where $\mathbf{K}_{i,j}$ is the intrinsic matrix, and $(\mathbf{R}_{i,j},\mathbf{t}_{i,j})$ are the extrinsic parameters (rotation and translation). The shared environment is described by $\mathcal{S}=\{\mathcal{O},\mathcal{L}\}$, where $\mathcal{O}$ represents the set of 3D objects (each with bounding box and semantic class) visible in the scene, and $\mathcal{L}$ contains global attributes like weather and street background. To manage the complex spatial relationships among cameras, we define a collaboration view graph $\mathcal{G}=(\mathcal{V},\mathcal{E})$. The vertices $\mathcal{V}=\bigcup_{i=1}^{N}\mathcal{C}_{i}$ are the set of all cameras, and the edges $\mathcal{E}$ connect any two cameras with overlapping fields of view. We perform an edge-disjoint 2-decomposition <sup>1</sup> of the graph $\mathcal{G}$ into an intra-agent view graph $\mathcal{G}_{\text{in}}$ and a cross-agent view graph $\mathcal{G}_{\text{cr}}$. The overall goal is to train a generative model $G$ that synthesizes a set of realistic and consistent images $\{\mathbf{I}_{i,j}\}$ for all cameras, conditioned on this comprehensive scene representation.

Progressive Single-to-multi-agent Training. Instead of directly learning multi-agent multi-camera image denoising, we decompose the problem into a two-stage progressive process. This involves first learning single-agent multi-camera denoising, and then learning to guide this process with cross-agent context. This decomposition is realized through a two-stage progressive training process with a unified objective representation:

$$
\mathcal{L}=\mathbb{E}_{z_{i,0},\epsilon,t}\left[||\epsilon-\epsilon_{\theta}(z_{i,t},t,\underbrace{\{\mathcal{S},\mathbf{P}_{i},\mathcal{G}_{\text{in}}(i)\}}_{\text{Intra-Agent Info.}},\underbrace{\kappa_{\text{ref}}}_{\text{C.A. Ref.}})||^{2}\right],
$$

where $z_{i,t}$ is the noisy latent for agent $A_{i}$ ’s views. The training has two stages:

Stage I: Intra-agent Multi-conditional Diffusion. The model is first trained using a simplified version of the objective where the cross-agent reference (C.A. Ref.) term $\kappa_{\text{ref}}$ is set to $\varnothing$. In this stage, the network learns single-agent multi-camera view generation based solely on its own information: camera poses ($\mathbf{P}_{i}$), geometric conditions (including text descriptions, 3D bounding boxes, road map, etc.), and the intra-agent view graph ($\mathcal{G}_{\text{in}}(i)$). This ensures generating coherent multi-view images based on basic conditions for an individual agent.

Stage II: Cross-agent Context-guided Diffusion. Building upon the pre-trained model, this stage uses the full objective shown above, introducing the cross-agent reference $\kappa_{\text{ref}}=\{\mathcal{H}_{\text{ref}}(i),\mathcal{G}_{\text{cr}}(i)\}$. This term consists of the latent states of collaborative agents ($\mathcal{H}_{\text{ref}}(i)$) and the cross-agent view graph ($\mathcal{G}_{\text{cr}}(i)$). Crucially, the latent states are obtained by encoding the neighbors’ data using the model pre-trained in Stage I. This stage adapts the model to generate consistent views across multiple agents by conditioning on both the neighbors’ latent states and their spatial connections. A detailed analysis of why this progressive decomposition improves trainability is provided in Appendix A.

![Refer to caption](https://arxiv.org/html/2605.29471v1/x1.png)

Figure 1: Architecture of our V2XCrafter. (a) Multi-agent geometric conditions encoding with hierarchical FPV-BEV fusion and learnable modifier 𝐕 ∗ \\mathbf{V}^{\*} for jointly observed objects. (b) Two-stage progressive training pipeline. (c) Ego-view attention for intra-agent consistency. (d) Cross-agent attention with collaboration view graph and ROI masking for cross-agent consistency.

## 4 V2XCrafter

The architecture of our proposed V2XCrafter is illustrated in Fig. 1. It features a two-stage progressive learning framework and a cross-agent attention module for realistic and controllable multi-agent driving scene generation. This section details the key components of our approach.

### 4.1 Multi-agent Geometric Conditions Encoding

As shown in Fig. 1a, the multi-agent geometric condition encoding strategy is used to help the model better understand the complexities of collaborative driving. For scene-level encoding, we retain strategies similar to those in single-agent autonomous driving. Specifically, we use the CLIP encoder for text descriptions and represent camera poses ($\mathbf{P}$) using Fourier features to provide global context. However, to respond to the unique challenges of the multi-agent setting, we introduce two additional designs.

Shared Object Encoding with Learnable Modifier. For each agent $i$, we represent the object condition as a sequence $\mathcal{O}_{i}=\{(\mathbf{b}_{k},l_{k})\}_{k=1}^{K_{i}}$, where $\mathbf{b}_{k}\in\mathbb{R}^{8\times 3}$ encodes the 3D bounding box corners and $l_{k}$ is the semantic class. To explicitly distinguish jointly observed objects (the same 3D object observed by multiple agents at the same time), we introduce a learnable modifier embedding $\mathbf{V}^{*}\in\mathbb{R}^{d}$ that is additively combined with class embeddings of jointly observed objects. Let $\mathcal{A}_{k}$ denote the set of agents observing object $k$, the enriched class embedding becomes:

$$
\tilde{\mathbf{l}}_{k}=\mathbf{l}_{k}+\mathbbm{1}_{|\mathcal{A}_{k}|>1}\cdot\mathbf{V}^{*},
$$

where $\mathbf{l}_{k}$ is the CLIP-initialized class embedding and $\mathbbm{1}_{|\mathcal{A}_{k}|>1}$ is an indicator function for jointly observed objects. The final object token is computed as $\mathbf{e}_{k}=\text{MLP}([\mathcal{F}(\mathbf{b}_{k});\tilde{\mathbf{l}}_{k}])$, where $\mathcal{F}$ denotes Fourier positional encoding \[hua2025fourier\]. We embed $\mathbf{V}^{*}$ as a learnable parameter within the trainable bounding box encoder of the ControlNet module \[Zhang\_2023\_ICCV\]. These object tokens are then concatenated with text and camera embeddings to form the encoder hidden states, which are injected into the frozen UNet via its cross-attention layers, as shown in Fig. 1b. Unlike textual inversion \[kumari2022customdiffusion\] requiring trainable cross-attention parameters, our approach enables $\mathbf{V}^{*}$ to be optimized through backpropagation as gradients flow from the generation loss through the frozen cross-attention layers back to the trainable bounding box encoder. This design allows $\mathbf{V}^{*}$ to modulate jointly observed object representations while maintaining full compatibility with the frozen Stable Diffusion backbone, providing semantic-level control that complements the spatial-level guidance introduced below.

Hierarchical FPV-BEV Joint Encoder. Implicit 3D box encoding with Bird’s-Eye-View (BEV) maps \[gao2023magicdrive\] suffers from limited precision in object localization and poor convergence under data-constrained settings, making it less suitable for our multi-agent driving scenarios. To address this, we introduce explicit per-camera First-Person-View (FPV) semantic masks that provide direct spatial supervision and work jointly with global BEV maps in a hierarchical encoding scheme. For each camera $C_{i,j}$, we generate FPV masks by projecting object bounding boxes onto the image plane. Given a 3D box $\mathbf{b}=(\mathbf{p},\mathbf{s},\theta)$ with center $\mathbf{p}\in\mathbb{R}^{3}$, size $\mathbf{s}\in\mathbb{R}^{3}$, and yaw $\theta$, its corner points $\{\mathbf{v}_{k}\}_{k=1}^{8}$ are projected to 2D coordinates $\mathbf{u}_{k}=[x_{1}/x_{3},x_{2}/x_{3}]^{\top}$ using camera intrinsics $\mathbf{K}_{i,j}$ and extrinsics $[\mathbf{R}_{i,j}|\mathbf{t}_{i,j}]$, where the homogeneous coordinates are calculated as:

$$
[x_{1},x_{2},x_{3}]^{\top}=\mathbf{K}_{i,j}(\mathbf{R}_{i,j}\mathbf{v}_{k}+\mathbf{t}_{i,j}).
$$

The resulting vector $[x_{1},x_{2},x_{3}]^{\top}$ represents the point in homogeneous image coordinates, where $x_{1},x_{2}$ are the pixel coordinates and $x_{3}$ is the depth. The projected polygon is rasterized using convex hull operations <sup>2</sup> to form a per-camera mask $\mathbf{M}_{i,j}\in\mathbb{R}^{H\times W}$, where each pixel encodes the object semantic class.

To integrate complementary information from global road structure and local object placement, we employ dual encoders $\mathcal{E}_{\text{BEV}}$ and $\mathcal{E}_{\text{FPV}}$ that extract features at different semantic levels. Let $\mathbf{B}\in\mathbb{R}^{C_{b}\times H_{b}\times W_{b}}$ denote the BEV map and $\mathbf{M}=\{\mathbf{M}_{i,j}\}$ denote the set of all per-camera masks:

$$
\mathbf{F}_{\text{BEV}}=\mathcal{E}_{\text{BEV}}(\mathbf{B}),\quad\mathbf{F}_{\text{FPV}}=\mathcal{E}_{\text{FPV}}(\mathbf{M}).
$$

The encoded features are then fused via a weighted residual connection:

$$
\mathbf{F}_{\text{cond}}=\mathbf{F}_{\text{FPV}}+\eta\cdot\mathbf{F}_{\text{BEV}},
$$

where $\eta$ is a weight parameter that balances the importance of FPV and BEV features. Rather than directly summing raw signals, the two encoders first map FPV masks and BEV layouts into a shared latent space, where FPV mainly governs precise object placement and BEV supplements global road topology. We provide an ablation on $\eta$ in Appendix C, showing that a small residual weight yields the best trade-off. This hierarchical fusion is then injected into the UNet decoder blocks <sup>3</sup>.

### 4.2 Ego-view Attention

In the Stage I training, we establish a robust single-agent multi-camera synthesis model, as shown in Fig. 1c. This model leverages the ego-agent’s camera arrangement, which defines a static intra-agent view graph $\mathcal{G}_{\text{in}}(i)=(\mathcal{V}_{i},\mathcal{E}_{\text{in}})$. Here, $\mathcal{V}_{i}=\mathcal{C}_{i}$ are the camera vertices, and the edges $\mathcal{E}_{\text{in}}\subseteq\mathcal{V}_{i}\times\mathcal{V}_{i}$ encode the fixed spatial adjacency based on field-of-view overlap. For each target camera $c\in\mathcal{V}_{i}$, its neighborhood is defined by the graph structure: $\mathcal{N}_{\text{in}}(c)=\{c^{\prime}\in\mathcal{V}_{i}\mid(c,c^{\prime})\in\mathcal{E}_{\text{in}}\}$. Given the latent feature representation $\mathbf{f}_{c}\in\mathbb{R}^{L\times D}$ for camera $c$, we perform a static message passing process via a neighbor attention mechanism to facilitate information flow. The feature aggregation for camera $c$ is formulated as:

$$
\mathbf{f}^{\prime}_{c}=\mathbf{f}_{c}+\sum_{j\in\mathcal{N}_{\text{in}}(c)}\mathbf{A}(c,j)\odot\phi_{V}(\mathbf{f}_{j}),
$$

where $\phi_{V}:\mathbb{R}^{L\times D}\to\mathbb{R}^{L\times D}$ is a learnable value projection, and the attention coefficient $\mathbf{A}(c,j)\in\mathbb{R}^{L\times L}$ is computed via:

$$
\mathbf{A}(c,j)=\text{Softmax}\left(\frac{\phi_{Q}(\mathbf{f}_{c})\cdot\phi_{K}(\mathbf{f}_{j})^{\top}}{\sqrt{d_{k}}}\right),
$$

with $\phi_{Q},\phi_{K}:\mathbb{R}^{L\times D}\to\mathbb{R}^{L\times d_{k}}$ being query and key projections, respectively. This design enables a static information flow along the fixed edges of $\mathcal{G}_{\text{in}}$, enforcing spatial coherence across neighboring views. The pre-defined sparse connectivity ensures computation efficiency while capturing the essential geometric constraints of the single-agent setup. The updated features $\{\mathbf{f}^{\prime}_{c}\}_{c\in\mathcal{V}_{i}}$ are then processed by the UNet decoder to generate multi-view images for agent $i$.

### 4.3 Cross-agent Attention

Dynamic Collaboration View Graph Construction. The complete collaboration view graph $\mathcal{G}=(\mathcal{V},\mathcal{E})$ comprises two disjoint subgraphs: the static intra-agent view graph $\mathcal{G}_{\text{in}}$ (used in the ego-view attention) and the dynamic cross-agent view graph $\mathcal{G}_{\text{cr}}$, where $\mathcal{V}=\bigcup_{i=1}^{N}\mathcal{C}_{i}$ and $\mathcal{E}=\mathcal{E}_{\text{in}}\cup\mathcal{E}_{\text{cr}}$. To enable our cross-agent attention, we first construct $\mathcal{G}_{\text{cr}}=(\mathcal{V},\mathcal{E}_{\text{cr}})$ where $\mathcal{E}_{\text{cr}}\subseteq\mathcal{V}\times\mathcal{V}$ captures inter-agent spatial relationships. Let $\mathcal{A}(c)$ denote the agent owning camera $c$, and $\mathcal{O}_{c}$ the set of visible object IDs in $c$. We define auxiliary predicates: $\Delta(c_{i},c_{j}):=\mathbbm{1}_{\mathcal{A}(c_{i})\neq\mathcal{A}(c_{j})}$ for cross-agent constraint, and $\Omega(c_{i},c_{j}):=\mathbbm{1}_{|\mathcal{F}_{c_{i}}\cap\mathcal{F}_{c_{j}}|>0}$ for geometric overlap where $\mathcal{F}_{c}$ denotes camera $c$ ’s frustum footprint on the BEV plane. We construct $\mathcal{E}_{\text{cr}}$ through two steps:

Step I: Object-based Connectivity Analysis. We first establish connections between cameras that share at least one jointly observed object, as these views explicitly exhibit strong semantic correlation and spatial overlap. This stage leverages object ID annotations commonly available in collaborative driving datasets to identify camera pairs that observe the same physical entities:

$$
E_{\text{obj}}=\{(c_{i},c_{j})\mid\Delta(c_{i},c_{j})\cdot\mathbbm{1}_{\mathcal{O}_{c_{i}}\cap\mathcal{O}_{c_{j}}\neq\emptyset}=1\}.
$$

Step II: Geometric Overlap Reasoning. For camera pairs not connected in Step I, we perform geometric reasoning by projecting their viewing frustums onto the BEV plane. This stage captures spatial relationships between cameras that may not observe common objects but still have overlapping fields of view, which is crucial for maintaining global spatial coherence:

$$
E_{\text{geo}}=\{(c_{i},c_{j})\notin E_{\text{obj}}\mid\Delta(c_{i},c_{j})\cdot\Omega(c_{i},c_{j})=1\}.
$$

The final edge set is $\mathcal{E}_{\text{cr}}=E_{\text{obj}}\cup E_{\text{geo}}$, yielding adjacency matrix $\mathbf{W}\in\{0,1\}^{|\mathcal{V}|\times|\mathcal{V}|}$ with $W_{ij}=\mathbbm{1}_{(c_{i},c_{j})\in\mathcal{E}_{\text{cr}}}$.

Shared-visibility ROI Masking. To enforce object-level consistency for jointly observed entities, we introduce a structured attention bias that guides cross-agent feature fusion. For each connected camera pair $(c_{i},c_{j})\in\mathcal{E}_{\text{cr}}$, we partition the token space into three categories based on their spatial correspondence to 3D bounding boxes and object visibility: (1) $\mathbf{V}^{*}$ object tokens corresponding to the shared objects marked by the learnable modifier $\mathbf{V}^{*}$, which require strict cross-agent appearance consistency; (2) non- $\mathbf{V}^{*}$ object tokens representing vehicle-specific objects visible only to one agent; and (3) background tokens covering road, buildings, and sky regions. We construct a dual-component attention bias matrix $\mathbf{M}_{ji}\in\mathbb{R}^{L\times L}$ that differentially regulates attention flow across token types:

$$
\mathbf{M}_{ji}=\alpha\cdot\mathbf{S}_{ji}+\beta\cdot\mathbf{B}_{ji},
$$

where $\mathbf{S}_{ji}$ and $\mathbf{B}_{ji}$ serve different roles, respectively: $\mathbf{S}_{ji}$ uses strong suppression ($-\tau_{o}$) to isolate $\mathbf{V}^{*}$ objects, ensuring each shared object only attends to its corresponding instance in other vehicles while blocking attention to unrelated objects; $\mathbf{B}_{ji}$ applies weaker suppression ($-\tau_{b}$) to regularize background and non- $\mathbf{V}^{*}$ tokens.

We define the following indicator functions: $\mathbbm{1}_{s}(k,l)$ indicates whether tokens $k$ and $l$ represent the same shared object instance; $\mathbbm{1}_{v}(k)$ indicates $\mathbf{V}^{*}$ membership; $\mathbbm{1}_{d}(k,l)$ indicates whether tokens $k$ and $l$ correspond to different objects or spatial regions (i.e., they should not strongly attend to each other); and $\mathbbm{1}_{v\oplus}(k,l)=\mathbbm{1}_{v}(k)\oplus\mathbbm{1}_{v}(l)$ (XOR operation) captures the mixed $\mathbf{V}^{*}$ /non- $\mathbf{V}^{*}$ pairs. The attention bias components are:

$$
\displaystyle S_{ji}^{(k,l)}
$$
 
$$
\displaystyle=-\tau_{o}\cdot[\mathbbm{1}_{v\oplus}(k,l)+\mathbbm{1}_{v}(k)\mathbbm{1}_{v}(l)(1-\mathbbm{1}_{s}(k,l))],
$$
$$
\displaystyle B_{ji}^{(k,l)}
$$
 
$$
\displaystyle=-\tau_{b}\cdot(1-\mathbbm{1}_{v}(k))(1-\mathbbm{1}_{v}(l))\mathbbm{1}_{d}(k,l),
$$

where $\tau_{o}\gg\tau_{b}>0$ ensures $\mathbf{V}^{*}$ objects receive stronger attention control than background regions. The term $\mathbbm{1}_{v\oplus}(k,l)$ creates a bidirectional barrier preventing information leakage between $\mathbf{V}^{*}$ and non- $\mathbf{V}^{*}$ tokens, while the term $\mathbbm{1}_{v}(k)\mathbbm{1}_{v}(l)(1-\mathbbm{1}_{s}(k,l))$ blocks attention between different $\mathbf{V}^{*}$ objects. The learnable scalars $\alpha$, and $\beta$ balance the object-level consistency and the scene-level coherence.

Cross-agent Masked Attention. To account for the evolving cross-agent camera view relationships, we perform dynamic message passing on the graph $\mathcal{G}_{\text{cr}}$. For an ego agent’s camera $c_{j}$ with features $\mathbf{f}_{j}\in\mathbb{R}^{L\times D}$, we aggregate information from its dynamically-determined cross-agent neighbors $\mathcal{N}_{\text{cr}}(c_{j})=\{c_{i}\mid(c_{j},c_{i})\in\mathcal{E}_{\text{cr}}\}$ using a masked attention mechanism:

$$
\mathbf{f}^{\prime\prime}_{j}=\sum_{i\in\mathcal{N}_{\text{cr}}(c_{j})}\text{Softmax}\left(\frac{\phi_{Q}(\mathbf{f}_{j})\cdot\phi_{K}(\mathbf{f}_{i})^{\top}}{\sqrt{d_{k}}}+\mathbf{M}_{ji}\right)\phi_{V}(\mathbf{f}_{i}).
$$

The attention bias $\mathbf{M}_{ji}$ modulates the attention distribution to prioritize tokens corresponding to shared objects (associated with $\mathbf{V}^{*}$ tokens at the semantic level), thereby establishing a multi-level consistency enforcement mechanism: the $\mathbf{V}^{*}$ token modulates object semantics through cross-attention in the UNet, while the ROI mask guides spatial feature alignment in the graph attention layer. The enhanced features $\mathbf{f}^{\prime\prime}_{j}$ are then refined through a feed-forward network:

$$
\tilde{\mathbf{f}}_{j}=\text{FFN}(\text{LayerNorm}(\mathbf{f}_{j}+\mathbf{f}^{\prime\prime}_{j})),
$$

enabling the model to synthesize views that are geometrically and semantically consistent across agents. The overall process of this attention is shown in Fig. 1d.

### 4.4 Model Training and Data Selection

Our training strategy progressively operationalizes the unified objective function defined in Section 3. Specifically, we first train the denoising model $\epsilon_{\theta}$ with only the Ego-view Attention module activated. After convergence, we freeze the pre-trained ego-centric layers and finetune the model with the Cross-agent Attention Module, guided by the added cross-agent reference condition $\kappa_{\text{ref}}$. To further enhance generation quality, we employ a high-quality data selection strategy to prioritize challenging training samples and a selective classifier-free guidance mechanism to balance the appearance flexibility with layout fidelity. See Appendix B for details.

![Refer to caption](https://arxiv.org/html/2605.29471v1/x2.png)

Figure 2: Cross-agent controllability of V2XCrafter. We show the generation results under different jointly observed object conditions.

## 5 Experiments

### 5.1 Experimental Setup

Dataset. Our model is trained and evaluated on V2X-Real dataset \[v2x-real\]. We filtered the scene to include at least two connected and autonomous vehicles (CAVs), resulting in 11,808 training and 2,624 validation samples. We additionally generated BEV maps using OpenCOOD \[xuOPV2VOpenBenchmark2022\] and text descriptions via Qwen2.5-VL-7B-Instruct \[bai2025qwen25vltechnicalreport\]. For details on data preprocessing, see Appendix F.

Implementation Details and Baselines. Our model builds upon Stable Diffusion v1.5 and is trained on 8 NVIDIA A800 GPUs for 150 epochs. We use a learning rate of 1e-4, a batch size of 4 per GPU, and a CFG scale of 1.5. For multi-agent camera image generation, we adapt MagicDrive-V2 \[gao2025magicdrive-v2\], MagicDrive \[gao2023magicdrive\], and BEVControl \[yang2023bevcontrolaccuratelycontrollingstreetview\] by extending them for one-shot multi-agent view generation with static intra-agent cross-view attention, denoted as MagicDrive-V2 (V2X), MagicDrive (V2X), and BEVControl (V2X). For the downstream collaborative 3D object detection task, we employ BEVFusion \[liu2022bevfusion\] as the base detector and AttnFuse \[xuOPV2VOpenBenchmark2022\] for multi-agent feature fusion. For BEV detection, we follow the dataset’s default Camera-LiDAR fusion setting, ensuring robust detection performance by leveraging multi-modal cues. For data augmentation experiments, we additionally evaluate V2VNet \[v2vnet\] and Late Fusion \[11020620\]. More details are in Appendix B.

![Refer to caption](https://arxiv.org/html/2605.29471v1/x3.png)

Figure 3: Qualitative comparison on V2X-Real dataset. Baselines fail to keep appearance consistency for jointly observed objects (highlighted regions). In contrast, our V2XCrafter ensures coherent appearance and geometry for jointly observed objects.

Evaluation Metrics. We use Fréchet Inception Distance (FID) to measure generation realism, and mean Average Precision ($\text{mAP}_{30}$ and $\text{mAP}_{50}$) to assess both layout controllability and data augmentation utility. To quantify cross-agent consistency of jointly observed objects, we report three retrieval-based metrics on CLIP (ViT-B/32) crops \[pmlr-v139-radford21a\]: CLIP Similarity measures pairwise appearance agreement, Mean Reciprocal Rank (MRR) evaluates how highly the correct cross-agent match is ranked among all shared instances, and Top-1 Accuracy measures whether the correct match is retrieved at rank 1. For more details of the metrics, see Appendix D.

![Refer to caption](https://arxiv.org/html/2605.29471v1/x4.png)

Figure 4: Qualitative ablation study on key components. Green circles/boxes denote correctly generated objects/backgrounds, red or yellow circles indicate wrong object generation, and boxes indicate inconsistent backgrounds.

### 5.2 Image Generation Results

Cross-agent Shared Foreground Control. Beyond basic scene-level controls shown in Fig. LABEL:fig:comparison, V2XCrafter provides fine-grained control over shared foreground objects with cross-agent consistency. As shown in Fig. 2, we demonstrate four capabilities: (a) consistent generation of jointly observed objects; (b) removal of jointly observed objects while maintaining background coherence; (c) repositioning jointly observed objects by modifying their 3D bounding boxes; and (d) inserting new jointly observed objects at specified locations. These capabilities are enabled by our shared-visibility ROI masking mechanism, which enforces object-level correspondence during cross-agent attention. This controllability further facilitates targeted data augmentation for collaborative perception. For instance, by strategically increasing the proportion of jointly observed objects during augmentation, we observe performance gains in downstream collaborative object detection tasks.

Diversity under Random Seeds and Text-Free Conditions. By changing random seeds or removing text prompts, our V2XCrafter can generate more diverse collaborative driving scenes while preserving cross-agent consistency on shared objects. Figure 5 shows that varying seeds changes foreground attributes and background appearance across CAV 1 and CAV 2, yet jointly observed objects marked by $\mathbf{V}^{*}$ still maintain identical appearance across agent perspectives. Figure 6 further demonstrates text-free conditional generation: even without text prompts, the model still synthesizes diverse object appearances and scene backgrounds under fixed geometric conditions. More supplementary comparisons and video generation results are provided in Appendix D and Appendix E.

![Refer to caption](https://arxiv.org/html/2605.29471v1/x5.png)

Figure 5: Visualization of generated images with different random seeds. With fixed text prompts and 3D bounding box conditions, varying random seeds produces diverse foreground object attributes and background appearance across CAV 1 and CAV 2. However, jointly observed objects marked by 𝐕 ∗ \\mathbf{V}^{\*} ( yellow and green boxes) consistently maintain identical visual appearance across agent perspectives.

![Refer to caption](https://arxiv.org/html/2605.29471v1/x6.png)

Figure 6: Text-free conditional generation results. By removing text prompts while keeping geometric conditions (3D boxes, BEV maps, camera poses), the model generates diverse appearances for both foreground objects ( yellow boxes highlight the jointly observed 𝐕 ∗ \\mathbf{V}^{\*} bus) and background scenes across two vehicles’ multi-view observations, demonstrating the model’s capability to synthesize varied physical attributes under identical layout constraints.

Baseline Comparison. To assess image quality and control accuracy, we replace real validation images with synthesized ones aligned with annotations and evaluate a collaborative perception system utilizing BEVFusion as the detector and AttnFuse for feature fusion, pre-trained on real data. Qualitatively (Fig. 3), baselines exhibit inconsistent appearances for jointly observed objects. Quantitatively (Table 1), V2XCrafter achieves the best consistency across all three retrieval-based metrics (0.836/0.586/0.406 for Sim./MRR/Top-1) and outperforms the best baseline by 58.0% in jointly observed mAP <sub>50</sub>, evidencing superior cross-agent consistency. Meanwhile, the lowest FID (17.46) and a 7.5% gain in overall mAP <sub>50</sub> demonstrate its high-fidelity image generation quality.

The substantial performance gap reveals two fundamental issues in baseline approaches, which are particularly detrimental to multi-modal collaborative perception. First, geometric mismatch induces modality conflict. Lacking precise spatial control, baseline-generated objects frequently exhibit positional drift. This visual misalignment contradicts the precise geometric signals from depth sensors (e.g., LiDAR), causing the fusion encoder to reject the mismatched image features as noise, thus degrading detection performance. Second, semantic inconsistency hinders collaborative fusion. Collaborative algorithms aggregate features based on semantic correspondence. When baselines render the same object with inconsistent appearances (e.g., varying textures or types) across agents, the resulting feature conflict prevents effective information aggregation. V2XCrafter addresses these root causes by enforcing strict geometric alignment and semantic coherence, producing data that is not only visually realistic but also “perceptually consistent” for downstream fusion models. Additional quantitative results and video generation demonstrations are provided in Appendix D and Appendix E, respectively.

Table 1: Quantitative comparison of generation quality and cross-agent consistency. Jointly Obs. denotes detection performance on jointly observed objects. Consistency reports CLIP Similarity / MRR / Top-1 Accuracy on shared object crops across agents. The best is in bold, and the second-best is underlined. Relative gains compared to the best baseline are marked in green. Gray rows indicate results from real data (GT) for reference.

<table><thead><tr><th></th><th rowspan="2">Method</th><th rowspan="2">FID <math><semantics><mo>↓</mo> <annotation>\downarrow</annotation></semantics></math></th><th rowspan="2">Consistency (Sim./MRR/Top-1) <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th><th colspan="2">CoDet. (Overall)</th><th colspan="2">CoDet. (Jointly Obs.)</th></tr><tr><th></th><th>mAP <sub>30</sub> <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th><th>mAP <sub>50</sub> <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th><th>mAP <sub>30</sub> <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th><th>mAP <sub>50</sub> <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th></tr><tr><th>Real</th><th>GT Ref.</th><th>–</th><th>0.813 / 0.656 / 0.512</th><th>38.60</th><th>32.18</th><th>47.85</th><th>40.12</th></tr></thead><tbody><tr><th rowspan="4">Gen.</th><th>BEVControl (V2X)</th><td>22.06</td><td>0.621 / 0.265 / 0.062</td><td>34.61</td><td>28.53</td><td>28.48</td><td>22.84</td></tr><tr><th>MagicDrive (V2X)</th><td>18.41</td><td>0.649 / 0.348 / 0.131</td><td>35.82</td><td>29.81</td><td>30.22</td><td>24.53</td></tr><tr><th>MagicDrive-V2 (V2X)</th><td>20.91</td><td>0.654 / 0.370 / 0.161</td><td>35.21</td><td>29.12</td><td>31.53</td><td>25.21</td></tr><tr><th>V2XCrafter (Ours)</th><td>17.46</td><td>0.836 / 0.586 / 0.406</td><td>38.45 <sup>+7.3%</sup></td><td>32.05 <sup>+7.5%</sup></td><td>47.53 <sup>+50.7%</sup></td><td>39.82 <sup>+58.0%</sup></td></tr></tbody></table>

### 5.3 Data Augmentation Results

To evaluate V2XCrafter’s utility for downstream tasks, we augment the V2X-Real training set with synthetic scenes (details in Appendix B) and measure the impact on collaborative perception.

Table 2: Impact of data augmentation with V2XCrafter on collaborative perception. The best results are in bold, and relative improvements over baselines (without generated data) are marked in green.

<table><thead><tr><th rowspan="2">Method</th><th colspan="2">Overall</th><th colspan="2">0-30m</th><th colspan="2">30-50m</th><th colspan="2">50-100m</th></tr><tr><th><math><semantics><msub><mtext>mAP</mtext> <mn>30</mn></msub> <annotation>\text{mAP}_{30}</annotation></semantics></math>   <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th><th><math><semantics><msub><mtext>mAP</mtext> <mn>50</mn></msub> <annotation>\text{mAP}_{50}</annotation></semantics></math>   <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th><th><math><semantics><msub><mtext>mAP</mtext> <mn>30</mn></msub> <annotation>\text{mAP}_{30}</annotation></semantics></math>   <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th><th><math><semantics><msub><mtext>mAP</mtext> <mn>50</mn></msub> <annotation>\text{mAP}_{50}</annotation></semantics></math>   <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th><th><math><semantics><msub><mtext>mAP</mtext> <mn>30</mn></msub> <annotation>\text{mAP}_{30}</annotation></semantics></math>   <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th><th><math><semantics><msub><mtext>mAP</mtext> <mn>50</mn></msub> <annotation>\text{mAP}_{50}</annotation></semantics></math>   <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th><th><math><semantics><msub><mtext>mAP</mtext> <mn>30</mn></msub> <annotation>\text{mAP}_{30}</annotation></semantics></math>   <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th><th><math><semantics><msub><mtext>mAP</mtext> <mn>50</mn></msub> <annotation>\text{mAP}_{50}</annotation></semantics></math>   <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th></tr></thead><tbody><tr><th>AttnFuse</th><td>38.60</td><td>32.18</td><td>47.85</td><td>40.25</td><td>30.92</td><td>25.18</td><td>17.64</td><td>15.82</td></tr><tr><th>+Gen Data</th><td>41.71 <sup>+8.1%</sup></td><td>34.27 <sup>+6.5%</sup></td><td>50.14 <sup>+4.8%</sup></td><td>42.08 <sup>+4.5%</sup></td><td>34.86 <sup>+12.7%</sup></td><td>27.53 <sup>+9.3%</sup></td><td>21.75 <sup>+23.3%</sup></td><td>18.29 <sup>+15.6%</sup></td></tr><tr><th>V2VNet</th><td>44.15</td><td>38.45</td><td>54.82</td><td>46.53</td><td>35.41</td><td>29.54</td><td>20.23</td><td>19.52</td></tr><tr><th>+Gen Data</th><td>46.02 <sup>+4.2%</sup></td><td>39.21 <sup>+2.0%</sup></td><td>56.19 <sup>+2.5%</sup></td><td>47.08 <sup>+1.2%</sup></td><td>37.72 <sup>+6.5%</sup></td><td>30.56 <sup>+3.5%</sup></td><td>22.65 <sup>+12.0%</sup></td><td>20.69 <sup>+6.0%</sup></td></tr><tr><th>Late Fusion</th><td>29.84</td><td>20.75</td><td>37.12</td><td>25.53</td><td>23.94</td><td>16.52</td><td>13.53</td><td>10.51</td></tr><tr><th>+Gen Data</th><td>33.48 <sup>+12.2%</sup></td><td>22.77 <sup>+9.7%</sup></td><td>39.91 <sup>+7.5%</sup></td><td>27.19 <sup>+6.5%</sup></td><td>28.37 <sup>+18.5%</sup></td><td>18.84 <sup>+14.0%</sup></td><td>17.86 <sup>+32.0%</sup></td><td>13.14 <sup>+25.0%</sup></td></tr></tbody></table>

As shown in Table 2, augmenting with synthetic data consistently improves Camera-LiDAR fusion performance across all methods (+12.2%). Specifically, the significant improvement in the 50-100m range (+23.3%) highlights the efficacy of our approach in addressing LiDAR sparsity. For distant objects where LiDAR point clouds are sparse, the detector relies heavily on camera features. Our generated images provide diverse and high-quality semantic cues (e.g., clear object boundaries under varying lighting) that align perfectly with the sparse LiDAR points, boosting long-range detection. Additionally, for collaborative perception feature fusion methods like AttnFuse, our cross-agent consistent generation ensures that features of jointly observed objects from different agents are semantically compatible, enabling more effective feature aggregation than inconsistent baselines. This indicates that while LiDAR provides precise geometric information, our generated images contribute complementary semantic richness (e.g., diverse object appearances and lighting conditions), which helps the model generalize better to complex scenarios.

Table 3: Ablation study on key components and training strategy. The best is in bold, and the second-best is underlined. Relative gains compared to the “w/o CA-Attn.” variant are marked in green. Gray rows indicate results from real data (GT) for reference. For the additional “w/o $\mathbf{V}^{*}$ ” variant, FID is unavailable and thus marked as “–”.

<table><thead><tr><th rowspan="2">Method</th><th rowspan="2">Prog. Training</th><th rowspan="2">FPV Mask</th><th rowspan="2"><math><semantics><msup><mi>𝐕</mi> <mo>∗</mo></msup> <annotation>\mathbf{V}^{*}</annotation></semantics></math></th><th rowspan="2">CA-Attn.</th><th rowspan="2">FID <math><semantics><mo>↓</mo> <annotation>\downarrow</annotation></semantics></math></th><th rowspan="2">CLIP Sim <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th><th colspan="2">CoDet. (Overall)</th><th colspan="2">CoDet. (Jointly Obs.)</th></tr><tr><th>mAP <sub>30</sub> <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th><th>mAP <sub>50</sub> <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th><th>mAP <sub>30</sub> <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th><th>mAP <sub>50</sub> <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th></tr><tr><th>GT Ref. (Real)</th><th>–</th><th>–</th><th>–</th><th>–</th><th>–</th><th>0.8132</th><th>38.60</th><th>32.18</th><th>47.85</th><th>40.12</th></tr></thead><tbody><tr><th rowspan="5">V2XCrafter (Gen.)</th><td>✓</td><td>✓</td><td>✓</td><td>×</td><td>18.01</td><td>0.6239</td><td>35.48</td><td>29.78</td><td>31.53</td><td>25.21</td></tr><tr><td>✓</td><td>✓</td><td>×</td><td>✓</td><td>17.88</td><td>0.8154</td><td>37.25</td><td>31.92</td><td>43.25</td><td>33.68</td></tr><tr><td>✓</td><td>×</td><td>✓</td><td>✓</td><td>28.26</td><td>0.5823</td><td>26.53</td><td>21.24</td><td>30.51</td><td>24.52</td></tr><tr><td>×</td><td>✓</td><td>✓</td><td>✓</td><td>20.33</td><td>0.6589</td><td>35.12</td><td>29.54</td><td>44.18</td><td>37.54</td></tr><tr><td>✓</td><td>✓</td><td>✓</td><td>✓</td><td>17.46</td><td>0.8369</td><td>38.45 <sup>+8.4%</sup></td><td>32.05 <sup>+7.6%</sup></td><td>47.53 <sup>+50.7%</sup></td><td>39.82 <sup>+58.0%</sup></td></tr></tbody></table>

### 5.4 Ablation Study

Effectiveness of Key Modules. We first analyze the impact of our two core modules. As shown in Fig. 4 and Table 3, without the FPV mask, the model must rely on implicit 3D box encoding for object placement control, resulting in poor object generation and potential positional drift. This leads to a high FID of 28.26 and a significant drop in collaborative BEV detection performance (21.24% Overall mAP <sub>50</sub>). This is because severely degraded or hallucinated images (Fig. 4) generate features that conflict with accurate LiDAR returns, confusing the fusion module and suppressing true positives. Fig. 4 visually confirms that objects are either distorted or misplaced. Removing the Cross-agent Attention (CA-Attn.) is equivalent to independent single-agent generation. While performing reasonably well in terms of FID (18.01), it fails to enforce consistency for jointly observed objects, resulting in a low CLIP similarity (0.6239) and poor detection performance on jointly observed objects (25.21% mAP <sub>50</sub>). As seen in Fig. 4, this leads to incoherent appearances for the same object viewed by different agents. When CA-Attn. remains enabled but the learnable shared-object modifier $\mathbf{V}^{*}$ is removed, the model still preserves basic alignment, yet consistency and jointly observed detection both decline (CLIP Sim. 0.8154, jointly observed mAP <sub>50</sub> 33.68). This indicates that CA-Attn. serves as the primary spatial alignment mechanism, while $\mathbf{V}^{*}$ acts as an auxiliary semantic tag that further helps the model identify and align shared instances across agents.

Effectiveness of Progressive Training. We then evaluate our core training strategy. The setting “w/o Prog. Training” means the model is trained one-shot to generate all 8 views for two agents simultaneously. As shown in Table 3, this approach yields suboptimal performance (FID of 20.33) and lower detection accuracy (29.54% Overall mAP <sub>50</sub>). This demonstrates that directly scaling up the generation task is difficult to optimize, validating the necessity of our progressive approach. More ablation studies on key hyperparameters including different CFG scales and data augmentation scales, are provided in Appendix C.

## 6 Conclusion

In this paper, we have introduced V2XCrafter, the first framework for collaborative driving scene generation across agents’ camera views. Our proposed two-stage progressive training strategy effectively addresses the expansion challenge of learning objective. Furthermore, the novel cross-agent attention mechanism ensures perceptual consistency for shared visibility across agent viewpoints. Experiments have confirmed that V2XCrafter generates high-fidelity, coherent scenes that boost downstream collaborative perception tasks.

Limitations and Future Work. Despite these advances, our work still has two main limitations. First, generation is confined to weather conditions seen during training. Second, Vehicle-to-Infrastructure (V2I) generation is not yet supported, primarily due to the limited view diversity of fixed RoadSide Units (RSUs) in current datasets. Future work will focus on extending the framework to these out-of-distribution conditions and incorporating V2I generation.

## References

V2XCrafter: Learning to Generate Driving Scene Across Agents

Supplementary Material

Contents

- A Analysis of Training Strategy........................................................................................................................................................................A
	- A.1 Challenges of Joint Multi-Agent Denoising........................................................................................................................................................................A.1
	- A.2 Progressive Two-Stage Decomposition........................................................................................................................................................................A.2
- B More Implementation Details........................................................................................................................................................................B
	- B.1 Implementation Details........................................................................................................................................................................B.1
	- B.2 Baseline Implementation Details........................................................................................................................................................................B.2
	- B.3 Data Selection Strategy........................................................................................................................................................................B.3
	- B.4 Classifier-Free Guidance (CFG)........................................................................................................................................................................B.4
	- B.5 Data Augmentation Details........................................................................................................................................................................B.5
- C More Ablation Studies........................................................................................................................................................................C
	- C.1 Ablation on CFG Scale........................................................................................................................................................................C.1
	- C.2 Ablation on Data Augmentation Scale........................................................................................................................................................................C.2
	- C.3 Ablation on FPV-BEV Fusion Weight........................................................................................................................................................................C.3
	- C.4 Competitor Utility Analysis........................................................................................................................................................................C.4
- D Experimental Results........................................................................................................................................................................D
	- D.1 Evaluation Metrics........................................................................................................................................................................D.1
	- D.2 More Generation Results........................................................................................................................................................................D.2
	- D.3 More Results on Collaborative Detection........................................................................................................................................................................D.3
- E Extension to Video Generation........................................................................................................................................................................E
- F More Details of Data Preprocessing........................................................................................................................................................................F
	- F.1 Scene Filtering........................................................................................................................................................................F.1
	- F.2 FPV Mask Generation........................................................................................................................................................................F.2
	- F.3 BEV Map Generation........................................................................................................................................................................F.3
	- F.4 Driving Scene Description Generation........................................................................................................................................................................F.4
- G More Discussion........................................................................................................................................................................G

## Appendix A Analysis of Training Strategy

We analyze why our progressive two-stage training strategy improves trainability compared to directly learning multi-agent multi-view generation. We first provide empirical evidence demonstrating the training convergence failure of naive multi-agent extension, then present a theoretical analysis showing how decomposing the joint denoising problem into sequential conditional optimizations significantly reduces training difficulty in the DDPM framework.

### A.1 Challenges of Joint Multi-Agent Denoising

Empirical Evidence. We train MagicDrive \[gao2023magicdrive\] with identical hyperparameters on two V2X-Real variants: (i) single-agent dataset with $N=1$ (1 $\times$ 4 views), derived by masking CAV2 data, and (ii) dual-agent dataset with $N=2$ (2 $\times$ 4 views), using complete V2X-Real. As shown in Figure 7, single-agent training converges by epoch 150, producing high-quality street-view images with clear object boundaries and consistent multi-view geometry. In contrast, dual-agent training exhibits severe convergence failure: generated images remain blurry with fragmented objects and inconsistent cross-agent appearance even after 150 epochs. This empirical evidence confirms that naive joint multi-agent multi-view generation faces fundamental optimization difficulties.

![Refer to caption](https://arxiv.org/html/2605.29471v1/x7.png)

Figure 7: Empirical evidence of training convergence failure in naive multi-agent extension. While MagicDrive trained on single-agent data (left) converges well, the same model trained on dual-agent data (right) fails to produce coherent results, demonstrating the necessity of a progressive training strategy.

Theoretical Analysis. Joint training optimizes over all $N$ agents simultaneously. For $N$ agents with $M$ cameras each, the DDPM objective is:

$$
\mathcal{L}_{\text{joint}}=\mathbb{E}_{z_{1:N,0},\epsilon,t}\left[\sum_{i=1}^{N}||\epsilon_{i}-\epsilon_{\theta}(z_{1:N,t},t,\mathcal{S},\mathbf{P}_{1:N},\mathcal{G})||^{2}\right],
$$

where $z_{1:N,t}=\{z_{1,t},\ldots,z_{N,t}\}$ are noisy latents for all $N\times M$ views at timestep $t$.

Decomposition of View Relationships. Multi-agent scenes contain two types of view relationships: (1) intra-agent relationships $\mathcal{R}_{\text{in}}$ between cameras on the same vehicle (static, frame-invariant), and (2) cross-agent relationships $\mathcal{R}_{\text{cr}}$ between cameras on different vehicles (dynamic, frame-varying). Following the principle of additive noise decomposition in denoising diffusion models \[ho2020denoising\], the predicted noise can be decomposed as:

$$
\epsilon_{\theta}(z,\mathcal{R}_{\text{in}},\mathcal{R}_{\text{cr}})=\epsilon_{\theta}^{\mathcal{R}_{\text{in}}}(z,\mathcal{R}_{\text{in}})+\epsilon_{\theta}^{\mathcal{R}_{\text{cr}}}(z,\mathcal{R}_{\text{cr}})+\epsilon_{\text{res}},
$$

where $\epsilon_{\theta}^{\mathcal{R}_{\text{in}}}$ captures noise conditioned on static intra-agent consistency, $\epsilon_{\theta}^{\mathcal{R}_{\text{cr}}}$ captures noise conditioned on dynamic cross-agent correlations, and $\epsilon_{\text{res}}$ represents residual interactions. In joint training, the network implicitly learns all three terms simultaneously, where $\epsilon_{\text{res}}$ encodes the coupling between static and dynamic relationships.

Challenge 1: Entangled Relationship Learning. We quantify learning complexity using the Rademacher complexity framework \[rademacher2024exponential\]. For a view relationship graph $\mathcal{R}$ with edge set $\mathcal{E}$, define complexity as:

$$
\mathcal{C}(\mathcal{R})=|\mathcal{E}|\cdot\mathbb{E}_{\text{frames}}[\text{Var}(\mathcal{E})],
$$

where $|\mathcal{E}|$ measures model capacity requirements and $\text{Var}(\mathcal{E})$ measures the variance of edge patterns across frames. For intra-agent relationships, $\text{Var}(\mathcal{R}_{\text{in}})\approx 0$ (fixed camera configurations), while for cross-agent relationships, $\text{Var}(\mathcal{R}_{\text{cr}})\gg 0$ (varying agent positions). By the law of total variance, joint learning complexity is:

$$
\mathcal{C}(\mathcal{L}_{\text{joint}})=\mathcal{C}(\mathcal{R}_{\text{in}})+\mathcal{C}(\mathcal{R}_{\text{cr}})+\underbrace{2\sqrt{\mathcal{C}(\mathcal{R}_{\text{in}})\cdot\mathcal{C}(\mathcal{R}_{\text{cr}})}}_{\mathcal{C}_{\text{couple}}>0},
$$

where the coupling term $\mathcal{C}_{\text{couple}}$ arises from learning interactions between static and dynamic patterns. Since $\mathcal{C}_{\text{couple}}$ scales with $\sqrt{N}$ (number of agents), joint training becomes increasingly difficult as the fleet size grows.

Challenge 2: Gradient Interference. The gradient aggregates signals from all agents:

$$
\nabla_{\theta}\mathcal{L}_{\text{joint}}=\sum_{i=1}^{N}\mathbb{E}\left[\nabla_{\theta}||\epsilon_{i}-\epsilon_{\theta}(z_{1:N,t},\cdots)||^{2}\right].
$$

To analyze gradient conflicts, we define idealized gradient components: let $\bar{g}^{\text{in}}=\frac{1}{N}\sum_{i=1}^{N}\nabla_{\theta}\mathcal{L}_{i}^{\mathcal{R}_{\text{in}}}$ represent the average gradient if training focused solely on intra-agent relationships, and $\bar{g}^{\text{cr}}=\frac{1}{N}\sum_{i=1}^{N}\nabla_{\theta}\mathcal{L}_{i}^{\mathcal{R}_{\text{cr}}}$ for cross-agent relationships. Following multi-task learning theory \[NEURIPS2021\_9d27fdf2\], gradients are considered conflicting if their cosine similarity is low or negative:

$$
\cos(\bar{g}^{\text{in}},\bar{g}^{\text{cr}})=\frac{\langle\bar{g}^{\text{in}},\bar{g}^{\text{cr}}\rangle}{\|\bar{g}^{\text{in}}\|\cdot\|\bar{g}^{\text{cr}}\|}\approx 0\text{ or }<0.
$$

When $\cos(\bar{g}^{\text{in}},\bar{g}^{\text{cr}})\leq 0$, the two objectives push parameters in orthogonal or opposite directions. In this case, the effective gradient magnitude becomes:

$$
\|\bar{g}^{\text{in}}+\bar{g}^{\text{cr}}\|^{2}=\|\bar{g}^{\text{in}}\|^{2}+\|\bar{g}^{\text{cr}}\|^{2}+2\langle\bar{g}^{\text{in}},\bar{g}^{\text{cr}}\rangle\leq\|\bar{g}^{\text{in}}\|^{2}+\|\bar{g}^{\text{cr}}\|^{2},
$$

which is significantly smaller than the case where gradients align ($\langle\bar{g}^{\text{in}},\bar{g}^{\text{cr}}\rangle>0$). For multi-agent driving scenes, gradient conflict arises because: (1) static intra-agent patterns (fixed camera overlap) dominate early training, producing large $\|\bar{g}^{\text{in}}\|$, while (2) dynamic cross-agent patterns (frame-varying visibility) require different parameter updates, producing $\bar{g}^{\text{cr}}$ that conflicts with $\bar{g}^{\text{in}}$. This results in oscillating updates where progress on one objective degrades performance on the other, as empirically observed in Figure 7.

### A.2 Progressive Two-Stage Decomposition

We decompose $\mathcal{L}_{\text{joint}}$ into sequential optimizations that learn $\mathcal{R}_{\text{in}}$ and $\mathcal{R}_{\text{cr}}$ separately.

Stage 1: Learning Static Intra-Agent View Relationships. Train on single agents:

$$
\mathcal{L}_{\text{stage1}}=\mathbb{E}_{z_{i,0},\epsilon,t}\left[||\epsilon-\epsilon_{\theta_{1}}(z_{i,t},t,\mathcal{S},\mathbf{P}_{i},\mathcal{G}_{\text{in}}(i))||^{2}\right].
$$

The gradient simplifies to:

$$
\nabla_{\theta_{1}}\mathcal{L}_{\text{stage1}}=\mathbb{E}\left[\nabla_{\theta_{1}}||\epsilon-\epsilon_{\theta_{1}}(z_{i,t},\cdots)||^{2}\right],
$$

eliminating inter-agent gradient interference ($g^{\text{cr}}=0$ in Stage 1). By Hoeffding’s inequality \[wang2022confidencedimensiondeeplearning\], since $\mathcal{G}_{\text{in}}(i)$ is frame-invariant (bounded variance), the gradient variance is bounded:

$$
\text{Var}(\nabla_{\theta_{1}}\mathcal{L}_{\text{stage1}})\leq\frac{\sigma^{2}}{B},\quad\text{where}\quad\sigma^{2}=\mathbb{E}[\|\epsilon-\epsilon_{\theta_{1}}\|^{2}],
$$

and $B$ is batch size. This is significantly lower than joint training: $\text{Var}(\nabla_{\theta_{1}}\mathcal{L}_{\text{stage1}})\ll\text{Var}(\nabla_{\theta}\mathcal{L}_{\text{joint}})$, ensuring stable convergence.

Stage 2: Learning Dynamic Cross-Agent View Relationships. Freeze $\theta_{1}$ and train cross-agent attention $\phi$:

$$
\mathcal{L}_{\text{stage2}}=\mathbb{E}_{z_{i,0},\epsilon,t}\left[||\epsilon-\epsilon_{\theta_{1},\phi}(z_{i,t},\cdots,h_{-i},\mathcal{G}_{\text{cr}}(i))||^{2}\right],
$$

where $h_{-i}=\text{Enc}_{\theta_{1}}(z_{-i,0})$ with frozen $\theta_{1}$. Following residual learning theory \[he2016deep\], this models a refinement:

$$
\epsilon_{\theta_{1},\phi}(z_{i,t},\cdots,h_{-i})=\underbrace{\epsilon_{\theta_{1}}(z_{i,t},\cdots)}_{\text{fixed intra-agent}}+\underbrace{\Delta\epsilon_{\phi}(h_{-i},\mathcal{G}_{\text{cr}}(i))}_{\text{learnable cross-agent}},
$$

where $\Delta\epsilon_{\phi}$ captures only cross-agent correlations without re-learning $\mathcal{R}_{\text{in}}$. Since $\theta_{1}$ is frozen, $\frac{\partial\epsilon_{\theta_{1}}}{\partial\phi}=0$, ensuring gradient orthogonality:

$$
\nabla_{\phi}\mathcal{L}_{\text{stage2}}=\mathbb{E}\left[\nabla_{\phi}\|\Delta\epsilon_{\phi}(h_{-i},\mathcal{G}_{\text{cr}})\|^{2}\right],\quad\text{Cov}(g_{\theta_{1}},g_{\phi})=0.
$$

This eliminates gradient interference between static and dynamic relationship learning.

Complexity Reduction. The two-stage approach achieves complexity reduction through decoupling. From Eq. (18), joint training has:

$$
\mathcal{C}(\mathcal{L}_{\text{joint}})=\mathcal{C}(\mathcal{R}_{\text{in}})+\mathcal{C}(\mathcal{R}_{\text{cr}})+2\sqrt{\mathcal{C}(\mathcal{R}_{\text{in}})\cdot\mathcal{C}(\mathcal{R}_{\text{cr}})}.
$$

In contrast, sequential training satisfies:

$$
\mathcal{C}(\mathcal{L}_{\text{stage1}})+\mathcal{C}(\mathcal{L}_{\text{stage2}})=\mathcal{C}(\mathcal{R}_{\text{in}})+\mathcal{C}(\mathcal{R}_{\text{cr}}),
$$

since the coupling term vanishes: $\mathcal{C}_{\text{couple}}=0$ (orthogonal parameter spaces). Define the relative complexity reduction ratio as:

$$
\rho_{\text{reduce}}:=\frac{\mathcal{C}(\mathcal{L}_{\text{joint}})-[\mathcal{C}(\mathcal{L}_{\text{stage1}})+\mathcal{C}(\mathcal{L}_{\text{stage2}})]}{\mathcal{C}(\mathcal{L}_{\text{joint}})}=\frac{2\sqrt{\mathcal{C}(\mathcal{R}_{\text{in}})\cdot\mathcal{C}(\mathcal{R}_{\text{cr}})}}{\mathcal{C}(\mathcal{R}_{\text{in}})+\mathcal{C}(\mathcal{R}_{\text{cr}})+2\sqrt{\mathcal{C}(\mathcal{R}_{\text{in}})\cdot\mathcal{C}(\mathcal{R}_{\text{cr}})}},
$$

where $\rho_{\text{reduce}}$ measures the fraction of learning complexity eliminated by decoupling. This ratio approaches 50% when $\mathcal{C}(\mathcal{R}_{\text{in}})\approx\mathcal{C}(\mathcal{R}_{\text{cr}})$. Additionally, from Eq. (27), gradient orthogonality ensures:

$$
\text{Tr}(\text{Cov}(g_{\theta_{1}},g_{\phi}))=0,
$$

eliminating gradient interference ($\beta_{\text{interf}}=0$). This transforms implicit joint learning into explicit conditional generation with clear supervision from frozen neighbor features $h_{-i}$, as validated by Figure 7.

## Appendix B More Implementation Details

### B.1 Implementation Details

Our model initializes its weights from the pre-trained Stable Diffusion v1.5 \[rombach2021highresolution\] and only trains the newly added parameters. V2XCrafter was trained on a server equipped with 2 Intel(R) Xeon(R) Platinum 8200C CPUs and 8 NVIDIA A800-SXM4-80GB GPUs. We set the learning rate to 1e-4 and used a batch size of 4 per GPU. The model was trained for 150 epochs (100 for stage I and 50 for stage II), which took approximately 38 hours to complete. The output image resolution for each of the 8 camera views (from 2 CAVs) is 272x480. For inference, we use a CFG guidance scale of 1.5 and 50 diffusion steps. For the downstream collaborative 3D object detection task, we employ BEVFusion \[liu2022bevfusion\] as the base detector for each vehicle and use AttnFuse \[xuOPV2VOpenBenchmark2022\] for multi-agent feature fusion. For data augmentation experiments, we additionally evaluate V2VNet \[v2vnet\] and Late Fusion \[11020620\] baselines, testing both camera-only (C) and camera-LiDAR (C+L) configurations.

### B.2 Baseline Implementation Details

To the best of our knowledge, no prior work has addressed the task of multi-agent collaborative driving scene generation. Therefore, we establish baselines by adapting state-of-the-art single-agent driving scene generation models. We select three representative methods, MagicDrive \[gao2023magicdrive\], MagicDrive-V2 \[gao2025magicdrive-v2\], and BEVControl \[yang2023bevcontrolaccuratelycontrollingstreetview\], which are originally designed for single-vehicle six-view generation on Nuscenes dataset \[Caesar\_2020\_CVPR\]. We retain their original single-vehicle cross-view attention with fixed neighbor camera view pairs and extend them to generate eight views from two vehicles simultaneously. We denote these adapted versions as MagicDrive (V2X), MagicDrive-V2 (V2X), and BEVControl (V2X), respectively, and retrain them on our processed V2X-Real dataset. Note that MagicDrive and MagicDrive-V2 originally do not use FPV mask encoding as an explicit control signal. However, we found that without FPV masks, these models completely fail to converge on the V2X-Real dataset. For fair comparison, we augment them with explicit FPV mask conditioning using the same masks generated in our preprocessing pipeline (see Appendix F.2).

### B.3 Data Selection Strategy

To ensure training data quality, we adopt a filtering and selection pipeline inspired by \[guo2025neptune-x\]. We evaluate generated samples from two perspectives: semantic consistency by computing CLIP similarity between generated images and text descriptions, and layout accuracy by using a pre-trained ResNet classifier to verify object categories within bounding boxes. We then employ an active sampling strategy to prioritize challenging samples with higher training difficulty across multiple attributes including viewpoint, location, and environment. For multi-agent multi-view generation, we aggregate difficulty scores across all cameras from all vehicles to select top-k instances that most effectively improve cross-agent consistency.

### B.4 Classifier-Free Guidance (CFG)

We employ a selective CFG strategy where scene-level conditions such as camera poses and text descriptions are randomly dropped during training at rate $\gamma$, while object-level conditions including 3D bounding boxes, BEV maps, and FPV masks are always retained. This ensures flexible control over scene appearance while maintaining strict spatial layout accuracy.

### B.5 Data Augmentation Details

Camera-Only Augmentation. For camera-only (C) data augmentation, we generate synthetic samples by randomly recombining and modifying control conditions from the original training set. Specifically, we randomly sample text prompts, BEV maps, camera poses, and 3D bounding boxes from different frames and combine them to create diverse driving scenarios. We also introduce controlled variations to these conditions, such as adjusting object positions, modifying lighting and weather descriptions in text prompts, and changing scene layouts. This strategy significantly increases scene diversity and is particularly effective for camera-only perception, which benefits from expanded visual coverage and varied environmental conditions.

Camera-LiDAR Augmentation. For camera-LiDAR (C+L) data augmentation, downstream collaborative detection models require strict geometric alignment between generated RGB images and the original LiDAR point clouds. To maintain this alignment, we fix the 3D bounding box conditions to match the original LiDAR data, ensuring that object positions and sizes remain consistent. However, we still modify other control signals, including text prompts (to change weather, lighting), to introduce visual variation while preserving geometric layouts. This approach generates appearance-diverse training data that maintains geometric compatibility with the original LiDAR measurements, enabling effective multi-modal training without breaking sensor fusion constraints.

## Appendix C More Ablation Studies

### C.1 Ablation on CFG Scale

![Refer to caption](https://arxiv.org/html/2605.29471v1/x8.png)

Figure 8: Ablation studies on key hyperparameters.

We conduct ablation studies to investigate two key hyperparameters: CFG scale and data augmentation scale. Results are shown in Figure 8. For CFG scale (Figure 8(a)), we evaluate FID for image quality and mAP for layout controllability. As observed, a CFG scale of 1.5 achieves the highest mAP scores (11.11 for mAP@30 and 9.15 for mAP@50), indicating optimal adherence to input layout conditions. While a scale of 2.0 yields slightly better FID (17.31 vs. 17.46), its mAP is lower. Increasing the scale beyond 2.0 degrades both metrics, suggesting overly strong guidance harms diversity and quality. Therefore, we select CFG scale of 1.5 for our main experiments.

### C.2 Ablation on Data Augmentation Scale

For data augmentation scale (Figure 8(b)), we vary the ratio of generated synthetic data size relative to the original training set size. The default ratio is 1.0, meaning the synthetic data equals the original training data in size. Results show that increasing augmentation scale consistently improves collaborative perception performance. At scale 1.5, mAP@30 reaches 14.62% (+29.8% relative improvement) and mAP@50 reaches 12.92%, demonstrating the value of large-scale synthetic data. Beyond scale 1.5, performance continues to improve but with diminishing returns. This saturation phenomenon suggests that the model has already learned relevant features from existing samples, and further increasing data volume alone provides limited additional information. To achieve further improvements, more effective sample selection strategies that prioritize diverse and challenging scenarios would be needed, which presents an interesting direction for future work.

### C.3 Ablation on FPV-BEV Fusion Weight

As discussed in Section 4.1, FPV masks and BEV maps provide complementary geometric cues: FPV primarily governs precise object placement, while BEV supplies global road topology. We therefore ablate the residual fusion weight $\eta$ in Eq. (5) to study how these two signals should be balanced. As shown in Table 4, $\eta=0.1$ achieves the best trade-off between image realism and cross-agent consistency. When $\eta=0$, the model underuses global road context; when $\eta$ becomes too large, coarse BEV layouts begin to overwhelm the more precise local guidance from FPV masks, degrading both FID and CLIP similarity.

Table 4: Ablation on the FPV-BEV fusion weight $\eta$ in Eq. (5).

| $\eta$ Value | $\mathbf{0.0}$ | $\mathbf{0.05}$ | $\mathbf{0.1}$ (Ours) | $\mathbf{0.5}$ | $\mathbf{1.0}$ |
| --- | --- | --- | --- | --- | --- |
| FID $\downarrow$ / CLIP Sim. $\uparrow$ | 17.94 / 0.835 | 17.62 / 0.821 | 17.46 / 0.836 | 18.52 / 0.798 | 19.35 / 0.764 |

### C.4 Competitor Utility Analysis

Generation quality can strongly affect downstream augmentation utility. To examine this effect, we augment the AttnFuse detector with synthetic data produced by three adapted baselines and a degraded internal variant (“w/o CA&V\*”), then compare them against our full model. As shown in Table 5, only the full model yields clear positive gains, while lower-quality synthetic data provides negligible benefit or even degrades detection performance. This suggests that cross-agent appearance inconsistency and geometric misalignment can act as semantic noise during collaborative fusion training.

Table 5: Impact of different generated data sources on downstream collaborative perception utility.

<table><thead><tr><th rowspan="2">Augmentation Data</th><th rowspan="2">None</th><th colspan="4">Inferior Competitors / Variants</th><th>Full Model</th></tr><tr><th>+ BEVControl (V2X)</th><th>+ MagicDrive (V2X)</th><th>+ MagicDrive-V2 (V2X)</th><th>+ w/o CA&V*</th><th>(Ours)</th></tr></thead><tbody><tr><td>Overall <math><semantics><msub><mtext>mAP</mtext> <mrow><mn>30</mn> <mo>/</mo> <mn>50</mn></mrow></msub> <annotation>\text{mAP}_{30/50}</annotation></semantics></math> <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></td><td>38.60 / 32.18</td><td>36.82 / 30.43</td><td>38.15 / 31.74</td><td>38.31 / 31.85</td><td>38.84 / 31.92</td><td>41.71 / 34.27</td></tr><tr><td>Relative <math><semantics><mi>Δ</mi> <annotation>\Delta</annotation></semantics></math></td><td>–</td><td>-1.78 / -1.75</td><td>-0.45 / -0.44</td><td>-0.29 / -0.33</td><td>+0.24 / -0.26</td><td>+3.11 / +2.09</td></tr></tbody></table>

## Appendix D Experimental Results

### D.1 Evaluation Metrics

This section describes the metrics employed to evaluate our generation quality, including mean Average Precision (mAP) for object detection evaluation, Fréchet Inception Distance (FID) for image realism assessment, and three retrieval-based consistency metrics for jointly observed objects across agents.

Mean Average Precision (mAP): mAP is a standard metric for evaluating object detection performance. The computation follows these steps: First, predicted bounding boxes are classified as True Positives (TP) or False Positives (FP) by comparing their Intersection-over-Union (IoU) with ground truth boxes against a predefined threshold. Second, predictions are ranked by confidence scores to construct the Precision-Recall (P-R) curve. Third, the Average Precision (AP) for each object class is computed as the area under its P-R curve. Finally, mAP is obtained by averaging AP values across all classes. In our experiments with V2X-Real dataset, we report two variants: $\text{mAP}_{50}$ uses a single IoU threshold of 0.5, following the PASCAL VOC convention, while $\text{mAP}_{30}$ uses a threshold of 0.3, which is more suitable for collaborative perception scenarios where localization requirements may be relaxed due to communication constraints. These metrics reflect both the detection accuracy across object categories and the localization precision of the generated scenes.

Fréchet Inception Distance (FID): FID \[heusel2017gans\] is a widely adopted metric for evaluating generative models by measuring the statistical distribution discrepancy between generated and real images in feature space. The computation involves two stages: feature extraction and distribution distance calculation. First, FID extracts deep feature representations $f_{\text{gen}}$, $f_{\text{gt}}$ from generated and ground truth images using a pre-trained Inception-v3 network. Then, it computes the mean $\{\mu_{\text{gen}},\mu_{\text{gt}}\}$ and covariance matrices $\{\Sigma_{\text{gen}},\Sigma_{\text{gt}}\}$ of these features. The FID score is calculated as:

$$
\text{FID}=\|\mu_{\text{gen}}-\mu_{\text{gt}}\|^{2}+\text{Tr}\left(\Sigma_{\text{gen}}+\Sigma_{\text{gt}}-2(\Sigma_{\text{gen}}\Sigma_{\text{gt}})^{1/2}\right),
$$

where $\|\cdot\|^{2}$ denotes the squared Euclidean norm and $\text{Tr}(\cdot)$ is the matrix trace operator. Lower FID values indicate better generation quality with closer distribution alignment to real data.

CLIP Similarity: To evaluate the semantic consistency of jointly observed objects across different agents, we first extract image crops of the shared objects using projected 2D bounding boxes, then encode these crops with the pre-trained CLIP ViT-B/32 model \[pmlr-v139-radford21a\]. CLIP Similarity is computed as the cosine similarity between the averaged embeddings of the same object from two agents. Higher values indicate better cross-agent appearance consistency.

Mean Reciprocal Rank (MRR): CLIP Similarity only measures pairwise agreement for matched object pairs. To further evaluate whether the correct cross-agent correspondence can be distinguished from all other shared objects in the same scene, we formulate a retrieval task. For each shared object crop from Agent A, we rank all shared object crops from Agent B by CLIP cosine similarity, and record the reciprocal rank of the true match. MRR averages this reciprocal rank over all shared objects. A higher MRR means the correct cross-agent match is ranked closer to the top more consistently.

Top-1 Accuracy: Based on the same retrieval protocol as MRR, Top-1 Accuracy measures the fraction of queries for which the correct cross-agent object is ranked first among all candidate shared objects from the other agent. Compared with CLIP Similarity and MRR, Top-1 is a stricter metric, since it requires exact rank-1 retrieval rather than just high similarity or near-top ranking. Higher Top-1 Accuracy indicates more reliable instance-level consistency across agents.

### D.2 More Generation Results

More Baseline Comparison. We supplement the main paper (Figure 3) with qualitative comparisons against MagicDrive-V2 (V2X) in Figure 9. Note that MagicDrive-V2 is originally designed for video generation. We adapt it for single-frame synthesis by removing temporal layers while preserving its spatial control capabilities, ensuring the same generation resolution as our method. Although MagicDrive-V2 (V2X) generates plausible scenes, it fails to maintain cross-agent consistency for jointly observed objects, leading to conflicting semantic features. This visual discrepancy aligns with its lower CLIP similarity (0.6541) in Table 1, further validating the necessity of our cross-agent attention for coherent multi-agent generation.

![Refer to caption](https://arxiv.org/html/2605.29471v1/figs/MagicDrive-v2.png)

Figure 9: Qualitative results of MagicDrive-V2 (V2X). The model fails to ensure appearance consistency for jointly observed objects across agents.

### D.3 More Results on Collaborative Detection

Camera-Only Results. As discussed in the main paper, we primarily target Camera+LiDAR (C+L) mode for BEV object Detection and collaborative perception, adhering to the V2X-Real dataset’s default configuration where minimal overlap of 4 surrounding cameras each vehicle makes multi-modal fusion essential. To ensure experimental completeness, we provide additional results using the Camera-only (C) setting here. Table 6 presents the generation quality with pretrained camera-only BEVFusion model, while Table 7 details the ablation study. Moreover, Table 8 reports fine-grained camera-only augmentation results on Easy / Moderate / Hard subsets. Specifically, we reorganize the V2X-Real validation set into Easy (28.9%), Moderate (24.4%), and Hard (46.7%) subsets according to 3D pose estimation difficulty, mainly considering factors such as object distance, occlusion level, and road slope. The results show that V2XCrafter-generated data consistently benefits camera-only collaborative perception across different difficulty levels. These results follow similar trends to the C+L setting, further validating the effectiveness of our method.

Table 6: Quantitative comparison of generation quality in C mode. The best is in bold, and the second-best is underlined. Relative gains compared to the best baseline are marked in green. Gray rows indicate results from real data (GT) for reference.

<table><thead><tr><th></th><th rowspan="2">Method</th><th rowspan="2">Views</th><th rowspan="2">FID <math><semantics><mo>↓</mo> <annotation>\downarrow</annotation></semantics></math></th><th colspan="2">CoDetection (Camera-only)</th></tr><tr><th></th><th><math><semantics><msub><mtext>mAP</mtext> <mn>30</mn></msub> <annotation>\text{mAP}_{30}</annotation></semantics></math> <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th><th><math><semantics><msub><mtext>mAP</mtext> <mn>50</mn></msub> <annotation>\text{mAP}_{50}</annotation></semantics></math> <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></th></tr><tr><th>Real</th><th>GT Ref.</th><th>–</th><th>–</th><th>11.48</th><th>9.77</th></tr></thead><tbody><tr><th rowspan="4">Gen.</th><th>BEVControl (V2X)</th><td>1 <math><semantics><mo>×</mo> <annotation>\times</annotation></semantics></math> 6 <math><semantics><mo>→</mo> <annotation>\rightarrow</annotation></semantics></math> 2 <math><semantics><mo>×</mo> <annotation>\times</annotation></semantics></math> 4</td><td>22.06</td><td>8.45</td><td>7.45</td></tr><tr><th>MagicDrive (V2X)</th><td>1 <math><semantics><mo>×</mo> <annotation>\times</annotation></semantics></math> 6 <math><semantics><mo>→</mo> <annotation>\rightarrow</annotation></semantics></math> 2 <math><semantics><mo>×</mo> <annotation>\times</annotation></semantics></math> 4</td><td>18.41</td><td>9.78</td><td>8.52</td></tr><tr><th>MagicDrive-V2 (V2X)</th><td>1 <math><semantics><mo>×</mo> <annotation>\times</annotation></semantics></math> 6 <math><semantics><mo>→</mo> <annotation>\rightarrow</annotation></semantics></math> 2 <math><semantics><mo>×</mo> <annotation>\times</annotation></semantics></math> 4</td><td>20.91</td><td>9.02</td><td>7.92</td></tr><tr><th>V2XCrafter (Ours)</th><td>2 <math><semantics><mo>×</mo> <annotation>\times</annotation></semantics></math> 4</td><td>17.46</td><td>11.11 <sup>+13.6%</sup></td><td>9.15 <sup>+7.4%</sup></td></tr></tbody></table>

Table 7: Ablation study on key components and training strategy in camera-only mode. The best is in bold, and the second-best is underlined. Relative gains compared to the best variant are marked in green. Gray rows indicate results from real data (GT) for reference.

<table><tbody><tr><th rowspan="2">Method</th><td rowspan="2">Prog. Training</td><td rowspan="2">FPV Mask</td><td rowspan="2"><math><semantics><msup><mi>𝐕</mi> <mo>∗</mo></msup> <annotation>\mathbf{V}^{*}</annotation></semantics></math></td><td rowspan="2">CA-Attn.</td><td rowspan="2">FID <math><semantics><mo>↓</mo> <annotation>\downarrow</annotation></semantics></math></td><td colspan="2">CoDetection (Camera-only)</td></tr><tr><td><math><semantics><msub><mtext>mAP</mtext> <mn>30</mn></msub> <annotation>\text{mAP}_{30}</annotation></semantics></math> <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></td><td><math><semantics><msub><mtext>mAP</mtext> <mn>50</mn></msub> <annotation>\text{mAP}_{50}</annotation></semantics></math> <math><semantics><mo>↑</mo> <annotation>\uparrow</annotation></semantics></math></td></tr><tr><th>GT Ref. (Real)</th><td>–</td><td>–</td><td>–</td><td>–</td><td>–</td><td>11.48</td><td>9.77</td></tr><tr><th rowspan="5">V2XCrafter (Gen.)</th><td>✓</td><td>✓</td><td>✓</td><td>×</td><td>18.01</td><td>9.92</td><td>8.53</td></tr><tr><td>✓</td><td>✓</td><td>×</td><td>✓</td><td>17.88</td><td>10.83</td><td>9.11</td></tr><tr><td>✓</td><td>×</td><td>✓</td><td>✓</td><td>28.26</td><td>7.02</td><td>6.53</td></tr><tr><td>×</td><td>✓</td><td>✓</td><td>✓</td><td>20.33</td><td>8.15</td><td>7.24</td></tr><tr><td>✓</td><td>✓</td><td>✓</td><td>✓</td><td>17.46</td><td>11.11 <sup>+12.0%</sup></td><td>9.15 <sup>+7.3%</sup></td></tr></tbody></table>

Detailed Analysis on Collaborative Detection. To understand how our V2XCrafter-generated data impacts collaborative perception across object categories, we present per-class performance analysis using AttFuse on V2X-Real. Tables 10 and 11 compare training with real data only versus augmenting with synthetic data for camera-only and camera-LiDAR configurations. Comparing baseline performance across sensor configurations, we observe that LiDAR fusion benefits categories asymmetrically. Car achieves 3.87 $\times$ improvement when adding LiDAR, increasing from 15.28% to 59.18% at IoU 0.3, while Truck only sees 1.75 $\times$ gain, from 15.28% to 26.74%. This disparity arises because large vehicles achieve reasonable performance with cameras alone due to their distinctive visual features, whereas smaller cars critically depend on LiDAR’s precise depth information for accurate localization.

Table 8: Fine-grained camera-only collaborative perception results on Easy / Moderate / Hard subsets. The V2X-Real validation set is reorganized according to 3D pose estimation difficulty following our rebuttal protocol. Green numbers indicate improvements, while red numbers indicate degradations.

<table><tbody><tr><th rowspan="2">Method</th><td colspan="3">Vehicle (AP@30/50)</td><td colspan="3">Overall (mAP@30/50)</td></tr><tr><td>Easy</td><td>Moderate</td><td>Hard</td><td>Easy</td><td>Moderate</td><td>Hard</td></tr><tr><th>AttnFuse</th><td>34.21/16.48</td><td>16.89/14.33</td><td>14.67/12.44</td><td>29.18/13.57</td><td>12.54/10.04</td><td>9.58/7.21</td></tr><tr><th>+ Ours</th><td>38.68/19.84 (+4.47/+3.36)</td><td>19.83/17.59 (+2.94/+3.26)</td><td>16.21/11.66 (+1.54)/(-0.78)</td><td>32.74/16.12 (+3.56/+2.55)</td><td>14.97/12.18 (+2.43/+2.14)</td><td>10.11/8.93 (+0.53/+1.72)</td></tr><tr><th>V2VNet</th><td>37.86/18.23</td><td>18.21/15.16</td><td>16.43/13.88</td><td>31.64/15.22</td><td>13.42/10.61</td><td>10.71/8.04</td></tr><tr><th>+ Ours</th><td>42.17/22.49 (+4.31/+4.26)</td><td>21.82/18.67 (+3.61/+3.51)</td><td>18.39/14.51 (+1.96/+0.63)</td><td>35.27/17.94 (+3.63/+2.72)</td><td>16.98/13.74 (+3.56/+3.13)</td><td>9.98/8.71 (-0.73)/(+0.67)</td></tr><tr><th>Late Fusion</th><td>25.19/14.56</td><td>13.53/11.89</td><td>11.27/10.13</td><td>20.63/10.47</td><td>9.88/8.31</td><td>7.61/6.28</td></tr><tr><th>+ Ours</th><td>29.81/18.63 (+4.62/+4.07)</td><td>15.86/13.61 (+2.33/+1.72)</td><td>10.86/10.69 (-0.41)/(+0.56)</td><td>24.36/13.58 (+3.73/+3.11)</td><td>12.11/10.05 (+2.23/+1.74)</td><td>8.19/6.11 (+0.58)/(-0.17)</td></tr></tbody></table>

Table 9: Impact of jointly observed object ratio on collaborative perception performance in C mode.

| Training Data | mAP <sub>30</sub> $\uparrow$ | mAP <sub>50</sub> $\uparrow$ |
| --- | --- | --- |
| Real Only | 11.48 | 9.77 |
| +Gen Data (2.1%) | 14.38 | 12.61 |
| +Gen Data (4.2%) | 14.59 | 12.82 |
| +Gen Data (6.3%) | 15.02 | 13.10 |

The results reveal an interesting pattern regarding data augmentation effects. Underrepresented categories such as Van, Bus, and Truck benefit more significantly from synthetic data, with improvements ranging from +3.3% to +5.8%, while well-represented categories like Car and Pedestrian show modest gains between +1.5% and +3.1%. This is because baseline models have already learned rich features for Car and Pedestrian from abundant training samples, reaching saturated performance where additional data provides diminishing returns. For underrepresented classes with limited samples, baseline models suffer from insufficient learning, making them more receptive to synthetic data diversity. Overall, V2XCrafter consistently improves detection performance across all major categories, demonstrating its effectiveness for multi-agent collaborative perception data augmentation.

Table 10: Per-class performance comparison with AttFuse (Camera-only) for 3D object detection on V2X-Real validation set. Results show AP (%) at IoU thresholds 0.3 and 0.5 for major categories.

<table><thead><tr><th rowspan="2">Data</th><th colspan="2">Overall</th><th colspan="2">Car</th><th colspan="2">Van</th><th colspan="2">Bus</th><th colspan="2">Truck</th><th colspan="2">Pedestrian</th></tr><tr><th>mAP <sub>30</sub></th><th>mAP <sub>50</sub></th><th>AP <sub>30</sub></th><th>AP <sub>50</sub></th><th>AP <sub>30</sub></th><th>AP <sub>50</sub></th><th>AP <sub>30</sub></th><th>AP <sub>50</sub></th><th>AP <sub>30</sub></th><th>AP <sub>50</sub></th><th>AP <sub>30</sub></th><th>AP <sub>50</sub></th></tr></thead><tbody><tr><th>Real Only</th><td>11.48</td><td>9.77</td><td>15.28</td><td>13.52</td><td>6.82</td><td>5.45</td><td>8.73</td><td>7.28</td><td>15.28</td><td>13.12</td><td>10.24</td><td>8.74</td></tr><tr><th>+V2XCrafter</th><td>14.59</td><td>12.82</td><td>18.42</td><td>16.48</td><td>10.95</td><td>9.18</td><td>13.18</td><td>11.52</td><td>21.04</td><td>18.47</td><td>13.18</td><td>11.64</td></tr><tr><th>Difference</th><td>+3.11</td><td>+3.05</td><td>+3.14</td><td>+2.96</td><td>+4.13</td><td>+3.73</td><td>+4.45</td><td>+4.24</td><td>+5.76</td><td>+5.35</td><td>+2.94</td><td>+2.90</td></tr></tbody></table>

Table 11: Per-class performance comparison with AttFuse (Camera+LiDAR) for 3D object detection on V2X-Real validation set. Results show AP (%) at IoU thresholds 0.3 and 0.5 for major categories.

<table><thead><tr><th rowspan="2">Data</th><th colspan="2">Overall</th><th colspan="2">Car</th><th colspan="2">Van</th><th colspan="2">Bus</th><th colspan="2">Truck</th><th colspan="2">Pedestrian</th></tr><tr><th>mAP <sub>30</sub></th><th>mAP <sub>50</sub></th><th>AP <sub>30</sub></th><th>AP <sub>50</sub></th><th>AP <sub>30</sub></th><th>AP <sub>50</sub></th><th>AP <sub>30</sub></th><th>AP <sub>50</sub></th><th>AP <sub>30</sub></th><th>AP <sub>50</sub></th><th>AP <sub>30</sub></th><th>AP <sub>50</sub></th></tr></thead><tbody><tr><th>Real Only</th><td>38.60</td><td>32.18</td><td>59.18</td><td>55.82</td><td>19.35</td><td>16.48</td><td>24.52</td><td>21.17</td><td>26.74</td><td>23.18</td><td>32.64</td><td>18.35</td></tr><tr><th>+V2XCrafter</th><td>41.71</td><td>34.27</td><td>61.24</td><td>57.74</td><td>23.18</td><td>19.74</td><td>28.15</td><td>24.52</td><td>30.12</td><td>26.48</td><td>34.18</td><td>19.82</td></tr><tr><th>Difference</th><td>+3.11</td><td>+2.09</td><td>+2.06</td><td>+1.92</td><td>+3.83</td><td>+3.26</td><td>+3.63</td><td>+3.35</td><td>+3.38</td><td>+3.30</td><td>+1.54</td><td>+1.47</td></tr></tbody></table>

Jointly Observed Object Augmentation. In the original V2X-Real training set, only 4.2% of objects are jointly observed by multiple agents, with 19,815 jointly observed objects out of 472,555 total objects. To investigate whether increasing the proportion of jointly observed objects could further improve collaborative perception, we conduct additional experiments where we manipulate this ratio during data augmentation by randomly inserting or removing jointly observed boxes in each frame, while keeping the total number of training samples constant. Table 9 presents the results using AttFuse (C), compared against the real-only baseline. Increasing the jointly observed ratio from 2.1% to 6.3% yields consistent improvements, with the 6.3% ratio achieving 15.02% mAP@30, representing a +30.9% relative gain over the real-only baseline.

![Refer to caption](https://arxiv.org/html/2605.29471v1/x9.png)

Figure 10: Distribution of jointly observed objects in V2X-Real training set.

This finding is particularly noteworthy when compared with the data augmentation scale ablation in Section C.2. While simply increasing data scale from $1.0\times$ to $1.5\times$ shows diminishing returns with only +0.03% absolute gain (14.59% to 14.62%), fine-grained augmentation that increases the jointly observed ratio from 4.2% to 6.3% achieves +0.43% improvement without adding more training samples. This demonstrates that strategically increasing jointly observed objects provides more valuable learning signals for collaborative perception models than naively scaling up data volume. The underlying reason is that collaborative perception relies on cross-agent feature fusion modules (e.g., spatial attention or graph neural networks) to aggregate complementary information from multiple agents. Jointly observed objects serve as natural correspondence anchors in the feature spaces of different agents: they provide explicit supervision for the fusion module to learn how to align and aggregate features representing the same physical entity observed from different viewpoints. When such objects are scarce in training data, the fusion module receives insufficient guidance to establish robust cross-agent feature correspondences, limiting its ability to effectively leverage multi-agent information. By increasing the proportion of jointly observed objects, we provide richer training signals that explicitly teach the fusion module to recognize and combine features of the same objects across agents, thereby improving collaborative detection performance. Overall, these results highlight V2XCrafter’s ability to generate controllable collaborative driving scenarios with targeted characteristics, making it particularly effective for addressing specific training requirements in multi-agent perception systems.

![Refer to caption](https://arxiv.org/html/2605.29471v1/x10.png)

Figure 11: The video generation results of V2XCrafter. We show the generation results of on the V2X-Real dataset. The first and last frames are provided with 3D bounding box annotations, and the intermediate frames are generated by the model. The cross-agent co-visible objects are highlighted with the same color.

## Appendix E Extension to Video Generation

Inspired by recent works on extending street view generation to driving videos \[gao2023magicdrive, gao2025magicdrive-v2\], V2XCrafter can be extended to multi-agent driving scene video generation as well. The key challenge lies in maintaining cross-agent consistency not only spatially within each frame, but also temporally across the video sequence. We address this by incorporating temporal modeling while preserving our core cross-agent consistency mechanisms.

Temporal Attention with Cross-Agent Awareness. To enable video generation, we augment the architecture with temporal attention layers. We replace the standard self-attention in ego-view attention (Section 4.2) with spatio-temporal attention (ST-Attn) \[wu2023tune\], allowing each camera to aggregate information from its own historical frames. Meanwhile, the cross-agent attention module (Section 4) operates at keyframes where collaboration view graphs $\mathcal{G}_{\text{cr}}$ and shared-visibility ROI masks are provided, ensuring that jointly observed objects maintain consistent appearance across vehicles.

Keyframe-Guided Temporal Interpolation. Unlike single-agent scenarios where only temporal coherence is required, multi-agent video generation must preserve cross-vehicle consistency throughout the temporal sequence. We achieve this by injecting our multi-agent geometric conditions (3D boxes, collaboration graphs, and ROI masks) at keyframes, while the temporal attention mechanism learns to interpolate intermediate frames. The learnable modifier $\mathbf{V}^{*}$ (Section 4.1) marks co-visible objects at keyframes, creating semantic anchors that guide the temporal interpolation to maintain object consistency across both agents and time.

Video Training on Top of Two-Stage Framework. Building upon our two-stage progressive training framework (Section 4), we add a third video fine-tuning stage. We fine-tune the model on 5-frame clips (chosen due to the training data size limitation) from V2X-Real with geometric conditions provided only at the first and last frames. During training, we sample initial noise independently for each frame and perform 50-step denoising using the UniPC sampler \[zhao2023unipc\]. As shown in Figure 11, this enables V2XCrafter to generate temporally coherent multi-agent videos while leveraging the spatial consistency mechanisms learned in the previous stages.

## Appendix F More Details of Data Preprocessing

The V2X-Real dataset \[v2x-real\] contains multi-view images from both connected vehicles and roadside units. To prepare the data for training V2XCrafter, we perform four main preprocessing steps: (1) scene filtering to select high-quality multi-vehicle scenarios, (2) FPV mask generation for fine-grained object-level control, (3) BEV map generation from LiDAR point clouds using OpenCOOD \[xuOPV2VOpenBenchmark2022\], and (4) text description generation via Qwen2.5-VL-7B-Instruct \[bai2025qwen25vltechnicalreport\]. This section provides detailed technical descriptions of each preprocessing component.

### F.1 Scene Filtering

We focus exclusively on Vehicle-to-Vehicle (V2V) data and exclude views from stationary RoadSide Units (RSUs), as RSU perspectives exhibit limited diversity and do not align with our multi-agent collaborative driving scenario. To ensure meaningful cross-agent generation, we filter the dataset to retain only frames where at least two connected autonomous vehicles (CAVs) are present and actively communicating.

Our filtering pipeline operates at two levels. First, at the scene level, we identify scenes containing multiple vehicles by checking for valid vehicle directories (folders named “1” and “2”) in the dataset structure. Second, at the frame level, we verify that each frame contains valid sensor data (cameras and LiDAR) from both vehicles, and that the annotation files (YAML format) are complete and correctly formatted. We also apply temporal consistency checks using a load interval parameter to ensure continuous sequences, discarding isolated frames that lack sufficient context.

After filtering, our dataset comprises 14,432 samples collected from 68 unique street scenes. We partition this data into training and validation sets using a scene-based 4.5:1 split ratio to prevent data leakage, resulting in 11,808 training samples (from 56 scenes) and 2,624 validation samples (from 12 scenes).

### F.2 FPV Mask Generation

To enable fine-grained control over jointly observed objects, we generate camera-specific object masks for each vehicle’s four cameras through precise 3D-to-2D projection.

3D Bounding Box Projection. For each annotated object, we generate precise camera-specific masks through a multi-step projection pipeline. Starting from the CARLA coordinate system annotations (location, center offset, extent, and rotation angles), we first construct the eight corner points of each 3D bounding box in world coordinates. Following CARLA conventions, we apply rotation matrices in the order of yaw-pitch-roll. We then transform these world coordinates to each camera’s view space through a composition of transformations: world $\rightarrow$ LiDAR $\rightarrow$ camera. The LiDAR-to-camera transformation is provided by the dataset’s extrinsic matrix, while the world-to-LiDAR transformation is computed from the vehicle’s 6-DOF pose. Finally, we project the transformed 3D points to 2D image coordinates using the camera’s intrinsic matrix, applying appropriate scaling to match our target resolution of 480 $\times$ 272 pixels.

Mask Rasterization. After obtaining 2D corner coordinates, we use OpenCV’s convexHull and fillPoly functions to create pixel-accurate masks. Each object is assigned a unique class ID, and we rasterize its convex hull onto a single-channel mask image. For each vehicle, we generate a 4-channel mask array where each channel corresponds to one camera view. This multi-channel representation allows the model to efficiently process all camera masks simultaneously during training.

### F.3 BEV Map Generation

The original V2X-Real dataset does not provide BEV semantic maps required for spatial layout control. We generate these maps by rendering LiDAR point clouds and 3D bounding box annotations into bird’s-eye view representations using tools from OpenCOOD \[xuOPV2VOpenBenchmark2022\].

Coordinate System Transformation. The generation process begins with transforming 3D point clouds from the vehicle’s local coordinate frame to a standardized BEV coordinate system. Given a LiDAR point cloud $\mathcal{P}=\{p_{i}\in\mathbb{R}^{3}\}$ and the vehicle’s 6-DOF pose $(x,y,z,\text{roll},\text{yaw},\text{pitch})$, we first construct the LiDAR-to-world transformation matrix $\mathbf{T}_{\text{L2W}}\in SE(3)$ by composing rotation matrices for yaw, pitch, and roll with the translation vector. The world-to-BEV transformation then projects points onto the ground plane while centering the coordinate system at the ego vehicle’s position.

Spatial Discretization. We adopt a 200 $\times$ 200 pixel BEV grid covering a spatial range of \[-50m, 50m\] in both X and Y directions, yielding a resolution of 0.5m per pixel. For each LiDAR point $p_{i}=(x_{i},y_{i},z_{i})$, we compute its grid coordinates as:

$$
u_{i}=\lfloor(x_{i}-x_{\min})/\Delta\rfloor,\quad v_{i}=\lfloor(y_{i}-y_{\min})/\Delta\rfloor,
$$

where $x_{\min}=y_{\min}=-50$ m and $\Delta=0.5$ m is the spatial resolution.

BEV Rendering. We render the BEV map using the OpenCOOD visualization utilities with offline rendering to ensure consistent quality. Specifically, we use the visualize\_single\_sample\_dataloader function to process LiDAR points and 3D bounding boxes into Open3D geometries. The rendering pipeline includes: (1) colorizing point clouds based on intensity values or height information via the color\_mode parameter, (2) projecting 3D bounding boxes onto the ground plane as wireframe line sets, and (3) rendering the scene from a top-down view using either Open3D’s OffscreenRenderer (with camera positioned at \[0, 0, 50\] looking down at \[0, 0, 0\]) or matplotlib’s scatter plot backend for headless server environments. The final output is a 200 $\times$ 200 RGB image where point clouds appear as colored dots and vehicle bounding boxes as red wireframes, providing clear spatial layout information for the diffusion model.

### F.4 Driving Scene Description Generation

To provide rich textual context for our generative model, we developed a sophisticated pipeline to create hierarchical driving scene descriptions for the V2X-Real dataset. We employ the powerful Qwen2.5-VL-7B-Instruct model to generate two distinct types of descriptions: fine-grained frame-level descriptions and holistic scene-level summaries. This hierarchical approach allows us to capture both dynamic and static elements of a driving scene. Frame-level descriptions provide detailed, real-time information about objects and their interactions, which can change rapidly. However, since many stylistic aspects like weather, time of day, and road environment are consistent across a scene, we generate a single, overarching scene-level description to capture this overall style. During training, we combine both descriptions to create a comprehensive text prompt.

Frame-Level Description Generation. To capture the dynamic elements of the scene, we generate a description for each frame that focuses on the specific objects present and their immediate context. The model is provided with images from all available cameras for a given frame, along with a list of detected objects. The prompt, shown in Figure 12 (top), directs the model to produce a fine-grained description of these objects and their immediate traffic situation, strictly adhering to a 20-25 word limit. An example of a frame-level description is: “A white SUV is turning left at an intersection while a red sedan waits at the traffic light and several pedestrians are crossing the street.” This ensures that the model has precise, time-sensitive information about the entities within the scene.

Scene-Level Description Generation. Since overarching characteristics such as weather, time of day, and the general road environment remain largely constant throughout a single driving scene, we generate a single scene-level description to capture this stylistic context. This avoids redundancy and provides a stable, high-level narrative. The model receives a set of sampled images from across the scene to form a holistic impression. The prompt, shown in Figure 12 (bottom), guides the model to summarize these static, stylistic elements within a strict 25-30 word limit. An example of a scene-level description is: “A daytime scene on a multi-lane urban road under clear, sunny skies, surrounded by modern high-rise buildings.” This description serves as the foundational layer of context.

To provide a comprehensive understanding, we create the final text prompt by concatenating scene-level with frame-level description using the template: `"{scene_description} {frame_description}"`. For instance, a complete prompt might be: “A daytime scene on a multi-lane urban road under clear, sunny skies, surrounded by modern high-rise buildings. In this scene, a white SUV is turning left at an intersection while a red sedan waits at the traffic light and several pedestrians are crossing the street.” This combined approach ensures the final prompt has a consistent structure and length (typically 45-55 words), providing both high-level context and specific, time-sensitive details.

## Appendix G More Discussion

### G.1 Future Extensions

Our current study mainly focuses on the V2V setting, where existing datasets provide richer multi-agent interaction patterns and more diverse viewpoints. Looking forward, extending the framework to V2I is a natural next step, since our core designs are fundamentally agent-agnostic and can be readily adapted once suitable datasets with sufficient RSU view diversity become available.

Meanwhile, the generation quality of V2XCrafter can be further improved by integrating stronger restoration and obstruction-removal modules, such as \[guo2024onerestore, li2025instruct2see\], although the current generation quality is already sufficient to provide effective augmentation gains for downstream collaborative perception.

### G.2 Potential Applications

Beyond data augmentation, V2XCrafter opens up several broader application directions.

Realistic V2X Driving Simulator: V2XCrafter serves as a high-fidelity neural rendering engine bridging lightweight simulators (e.g., CARLA) and reality. It synthesizes photo-realistic, spatially consistent multi-vehicle observations, enabling controllable and cost-effective simulation for complex collaborative driving scenarios involving interactions between multiple connected vehicles.

Training Generalizable Feedforward 3DGS for V2X. V2XCrafter’s ability to generate cross-agent consistent observations is uniquely suited for V2X-based 3D reconstruction. By synthesizing synchronized multi-view images from spatially distributed vehicles, it provides dense scene coverage that effectively resolves occlusions common in single-agent views. This consistent multi-agent data serves as high-quality supervision for training generalizable feedforward 3D Gaussian Splatting (3DGS) models, facilitating the reconstruction of large-scale, holistic collaborative driving environments.

V2X World Models for Collaborative Planning. V2XCrafter can be extended to predictive digital twin and world models \[10107755, 10437455, 10614333, Min\_2024\_CVPR, Zheng\_2025\_ICCV\] for collaborative driving. By generating future multi-agent observations conditioned on joint planned trajectories, it supports anticipatory planning and decision-making, allowing vehicles to simulate and evaluate cooperative maneuvers in complex traffic environments.

<svg id="A7.F12.pic1" height="2698.72" overflow="visible" version="1.1" viewBox="0 0 600 2698.72" width="600"><g style="--ltx-stroke-color:#000000;--ltx-fill-color:#000000;" transform="translate(0,2698.72) matrix(1 0 0 -1 0 0)" fill="#000000" stroke="#000000" stroke-width="0.4pt"><g style="--ltx-fill-color:#9F9F9F;" fill="#9F9F9F" fill-opacity="1.0"><path style="stroke:none" d="M 0 5.91 L 0 2692.81 C 0 2696.07 2.64 2698.72 5.91 2698.72 L 594.09 2698.72 C 597.36 2698.72 600 2696.07 600 2692.81 L 600 5.91 C 600 2.64 597.36 0 594.09 0 L 5.91 0 C 2.64 0 0 2.64 0 5.91 Z"></path></g><g style="--ltx-fill-color:#F2F2F2;" fill="#F2F2F2" fill-opacity="1.0"><path style="stroke:none" d="M 1.97 5.91 L 1.97 2674.61 L 598.03 2674.61 L 598.03 5.91 C 598.03 3.73 596.27 1.97 594.09 1.97 L 5.91 1.97 C 3.73 1.97 1.97 3.73 1.97 5.91 Z"></path></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 2683.2)"><foreignObject style="--ltx-fg-color:#FFFFFF;--ltx-fo-width:40.23em;--ltx-fo-height:0.69em;--ltx-fo-depth:0.19em;" width="556.69" height="12.3" transform="matrix(1 0 0 -1 0 9.61)" overflow="visible" color="#FFFFFF"><span id="A7.F12.pic1.1.1.1.1.1" style="width:34.98em;"><span id="A7.F12.pic1.1.1.1.1.1.1"><span id="A7.F12.pic1.1.1.1.1.1.1.1">Frame-Level Description Prompt</span></span> </span></foreignObject></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 2653.88)"><foreignObject style="--ltx-fg-color:#000000;--ltx-fo-width:40.23em;--ltx-fo-height:0.64em;--ltx-fo-depth:190.8em;" width="556.69" height="2649.02" transform="matrix(1 0 0 -1 0 8.92)" overflow="visible" color="#000000"><span id="A7.F12.pic1.2.2.2.1.1" style="width:40.23em;"><span id="A7.F12.pic1.2.2.2.1.1.1"><a href="data:text/plain;base64,RGVzY3JpYmUgdGhlIHZpc2libGUgb2JqZWN0cyBhbmQgdGhlaXIgaW1tZWRpYXRlIHRyYWZmaWMgc2l0dWF0aW9uIGluIHRoaXMgZnJhbWUgZnJvbSB2ZWhpY2xlIHt2ZWhpY2xlX2lkfS4gRm9jdXMgb24gdGhlIGFjdGlvbnMgYW5kIHBvc2l0aW9ucyBvZiBjYXJzLCB0cnVja3MsIGFuZCBwZWRlc3RyaWFucy4gR2VuZXJhdGUgYSBkZXNjcmlwdGlvbiBvZiBleGFjdGx5IDIwLTI1IEVuZ2xpc2ggd29yZHMuCgpAXHRleHRiZntPYmplY3RzIGRldGVjdGVkfUA6IHtjb250ZXh0X2luZm99CgpXcml0ZSBvbmUgc2VudGVuY2UgZGVzY3JpYmluZyB0aGUgQFx0ZXh0YmZ7a2V5IG9iamVjdHN9QCBhbmQgdGhlaXIgQFx0ZXh0YmZ7Y3VycmVudCBhY3Rpb25zfUAuCgpAXHRleHRiZntFeGFtcGxlfUA6ICJBIHdoaXRlIFNVViBpcyB0dXJuaW5nIGxlZnQgYXQgYW4gaW50ZXJzZWN0aW9uIHdoaWxlIGEgcmVkIHNlZGFuIHdhaXRzIGF0IHRoZSB0cmFmZmljIGxpZ2h0IGFuZCBzZXZlcmFsIHBlZGVzdHJpYW5zIGFyZSBjcm9zc2luZyB0aGUgc3RyZWV0LiIKCkBcdGV4dGJme1lvdXIgZGVzY3JpcHRpb259QDo=" download="">⬇</a> <span id="lstnumberx1"><span id="lstnumberx1.1">Describe</span> <span id="lstnumberx1.3">the</span> <span id="lstnumberx1.5">visible</span> <span id="lstnumberx1.7">objects</span> <span id="lstnumberx1.9">and</span> <span id="lstnumberx1.11">their</span> <span id="lstnumberx1.13">immediate</span> <span id="lstnumberx1.15">traffic</span> <span id="lstnumberx1.17">situation</span> <span id="lstnumberx1.19">in</span> <span id="lstnumberx1.21">this</span> <span id="lstnumberx1.23">frame</span> <span id="lstnumberx1.25">from</span> <span id="lstnumberx1.27">vehicle</span> { <span id="lstnumberx1.29">vehicle_id</span> }.<span id="lstnumberx1.31">Focus</span> <span id="lstnumberx1.33">on</span> <span id="lstnumberx1.35">the</span> <span id="lstnumberx1.37">actions</span> <span id="lstnumberx1.39">and</span> <span id="lstnumberx1.41">positions</span> <span id="lstnumberx1.43">of</span> <span id="lstnumberx1.45">cars</span>,<span id="lstnumberx1.47">trucks</span>,<span id="lstnumberx1.49">and</span> <span id="lstnumberx1.51">pedestrians</span>.<span id="lstnumberx1.53">Generate</span> <span id="lstnumberx1.55">a</span> <span id="lstnumberx1.57">description</span> <span id="lstnumberx1.59">of</span> <span id="lstnumberx1.61">exactly</span> 20-25 <span id="lstnumberx1.64">English</span> <span id="lstnumberx1.66">words</span>.</span> <span id="lstnumberx3"><span id="lstnumberx3.1">Objects detected</span>:{ <span id="lstnumberx3.3">context_info</span> }</span> <span id="lstnumberx5"><span id="lstnumberx5.1">Write</span> <span id="lstnumberx5.3">one</span> <span id="lstnumberx5.5">sentence</span> <span id="lstnumberx5.7">describing</span> <span id="lstnumberx5.9">the</span> <span id="lstnumberx5.11">key objects</span> <span id="lstnumberx5.13">and</span> <span id="lstnumberx5.15">their</span> <span id="lstnumberx5.17">current actions</span>.</span> <span id="lstnumberx7"><span id="lstnumberx7.1">Example</span>:" <span id="lstnumberx7.3">A</span> <span id="lstnumberx7.5">white</span> <span id="lstnumberx7.7">SUV</span> <span id="lstnumberx7.9">is</span> <span id="lstnumberx7.11">turning</span> <span id="lstnumberx7.13">left</span> <span id="lstnumberx7.15">at</span> <span id="lstnumberx7.17">an</span> <span id="lstnumberx7.19">intersection</span> <span id="lstnumberx7.21">while</span> <span id="lstnumberx7.23">a</span> <span id="lstnumberx7.25">red</span> <span id="lstnumberx7.27">sedan</span> <span id="lstnumberx7.29">waits</span> <span id="lstnumberx7.31">at</span> <span id="lstnumberx7.33">the</span> <span id="lstnumberx7.35">traffic</span> <span id="lstnumberx7.37">light</span> <span id="lstnumberx7.39">and</span> <span id="lstnumberx7.41">several</span> <span id="lstnumberx7.43">pedestrians</span> <span id="lstnumberx7.45">are</span> <span id="lstnumberx7.47">crossing</span> <span id="lstnumberx7.49">the</span> <span id="lstnumberx7.51">street</span>."</span> <span id="lstnumberx9"><span id="lstnumberx9.1">Your description</span>:</span></span></span></foreignObject></g></g></svg>

<svg id="A7.F12.pic2" height="1934.92" overflow="visible" version="1.1" viewBox="0 0 600 1934.92" width="600"><g style="--ltx-stroke-color:#000000;--ltx-fill-color:#000000;" transform="translate(0,1934.92) matrix(1 0 0 -1 0 0)" fill="#000000" stroke="#000000" stroke-width="0.4pt"><g style="--ltx-fill-color:#9F9F9F;" fill="#9F9F9F" fill-opacity="1.0"><path style="stroke:none" d="M 0 5.91 L 0 1929.01 C 0 1932.27 2.64 1934.92 5.91 1934.92 L 594.09 1934.92 C 597.36 1934.92 600 1932.27 600 1929.01 L 600 5.91 C 600 2.64 597.36 0 594.09 0 L 5.91 0 C 2.64 0 0 2.64 0 5.91 Z"></path></g><g style="--ltx-fill-color:#F2F2F2;" fill="#F2F2F2" fill-opacity="1.0"><path style="stroke:none" d="M 1.97 5.91 L 1.97 1910.8 L 598.03 1910.8 L 598.03 5.91 C 598.03 3.73 596.27 1.97 594.09 1.97 L 5.91 1.97 C 3.73 1.97 1.97 3.73 1.97 5.91 Z"></path></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 1919.4)"><foreignObject style="--ltx-fg-color:#FFFFFF;--ltx-fo-width:40.23em;--ltx-fo-height:0.69em;--ltx-fo-depth:0.19em;" width="556.69" height="12.3" transform="matrix(1 0 0 -1 0 9.61)" overflow="visible" color="#FFFFFF"><span id="A7.F12.pic2.1.1.1.1.1" style="width:34.98em;"><span id="A7.F12.pic2.1.1.1.1.1.1"><span id="A7.F12.pic2.1.1.1.1.1.1.1">Scene-Level Description Prompt</span></span> </span></foreignObject></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 1890.08)"><foreignObject style="--ltx-fg-color:#000000;--ltx-fo-width:40.23em;--ltx-fo-height:0.64em;--ltx-fo-depth:135.6em;" width="556.69" height="1885.21" transform="matrix(1 0 0 -1 0 8.92)" overflow="visible" color="#000000"><span id="A7.F12.pic2.2.2.2.1.1" style="width:40.23em;"><span id="A7.F12.pic2.2.2.2.1.1.1"><a href="data:text/plain;base64,RGVzY3JpYmUgdGhlIG92ZXJhbGwgc3R5bGUgb2YgdGhpcyB0cmFmZmljIHNjZW5lIGluIGV4YWN0bHkgMjUtMzAgRW5nbGlzaCB3b3JkcywgYmFzZWQgb24gdGhlIHByb3ZpZGVkIGltYWdlcyBmcm9tIGRpZmZlcmVudCB0aW1lcy4KClN1bW1hcml6ZSB0aGUgZ2VuZXJhbCBAXHRleHRiZntyb2FkIHR5cGV9QCwgQFx0ZXh0YmZ7d2VhdGhlciBjb25kaXRpb25zfUAsIEBcdGV4dGJme3RpbWUgb2YgZGF5fUAsIGFuZCBAXHRleHRiZntzdXJyb3VuZGluZyBlbnZpcm9ubWVudH1ALgoKQFx0ZXh0YmZ7RXhhbXBsZX1AOiAiQSBkYXl0aW1lIHNjZW5lIG9uIGEgbXVsdGktbGFuZSB1cmJhbiByb2FkIHVuZGVyIGNsZWFyLCBzdW5ueSBza2llcywgc3Vycm91bmRlZCBieSBtb2Rlcm4gaGlnaC1yaXNlIGJ1aWxkaW5ncy4iCgpAXHRleHRiZntZb3VyIGRlc2NyaXB0aW9ufUA6" download="">⬇</a> <span id="lstnumberx10"><span id="lstnumberx10.1">Describe</span> <span id="lstnumberx10.3">the</span> <span id="lstnumberx10.5">overall</span> <span id="lstnumberx10.7">style</span> <span id="lstnumberx10.9">of</span> <span id="lstnumberx10.11">this</span> <span id="lstnumberx10.13">traffic</span> <span id="lstnumberx10.15">scene</span> <span id="lstnumberx10.17">in</span> <span id="lstnumberx10.19">exactly</span> 25-30 <span id="lstnumberx10.22">English</span> <span id="lstnumberx10.24">words</span>,<span id="lstnumberx10.26">based</span> <span id="lstnumberx10.28">on</span> <span id="lstnumberx10.30">the</span> <span id="lstnumberx10.32">provided</span> <span id="lstnumberx10.34">images</span> <span id="lstnumberx10.36">from</span> <span id="lstnumberx10.38">different</span> <span id="lstnumberx10.40">times</span>.</span> <span id="lstnumberx12"><span id="lstnumberx12.1">Summarize</span> <span id="lstnumberx12.3">the</span> <span id="lstnumberx12.5">general</span> <span id="lstnumberx12.7">road type</span>,<span id="lstnumberx12.9">weather conditions</span>,<span id="lstnumberx12.11">time of day</span>,<span id="lstnumberx12.13">and</span> <span id="lstnumberx12.15">surrounding environment</span>.</span> <span id="lstnumberx14"><span id="lstnumberx14.1">Example</span>:" <span id="lstnumberx14.3">A</span> <span id="lstnumberx14.5">daytime</span> <span id="lstnumberx14.7">scene</span> <span id="lstnumberx14.9">on</span> <span id="lstnumberx14.11">a</span> <span id="lstnumberx14.13">multi</span> - <span id="lstnumberx14.14">lane</span> <span id="lstnumberx14.16">urban</span> <span id="lstnumberx14.18">road</span> <span id="lstnumberx14.20">under</span> <span id="lstnumberx14.22">clear</span>,<span id="lstnumberx14.24">sunny</span> <span id="lstnumberx14.26">skies</span>,<span id="lstnumberx14.28">surrounded</span> <span id="lstnumberx14.30">by</span> <span id="lstnumberx14.32">modern</span> <span id="lstnumberx14.34">high</span> - <span id="lstnumberx14.35">rise</span> <span id="lstnumberx14.37">buildings</span>."</span> <span id="lstnumberx16"><span id="lstnumberx16.1">Your description</span>:</span></span></span></foreignObject></g></g></svg>

Figure 12: The structured prompts used for generating frame-level (top) and scene-level (bottom) descriptions. These prompts guide the Qwen2.5-VL-7B-Instruct model to produce consistent and relevant textual data.