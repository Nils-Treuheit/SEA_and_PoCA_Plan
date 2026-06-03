---
title: "TGS-SLAM: Tri-Plane Gaussian Splatting for Semantic SLAM"
source: "https://ieeexplore.ieee.org/abstract/document/11513902"
author:
published:
created: 2026-06-03
description: "Semantic SLAM aims to build 3D maps that are both geometrically accurate and semantically consistent for downstream tasks such as navigation, manipulation, and"
tags:
  - "clippings"
---
## Abstract:

Semantic SLAM aims to build 3D maps that are both geometrically accurate and semantically consistent for downstream tasks such as navigation, manipulation, and AR/VR. Exi...

---

Semantic SLAM aims to build 3D maps that are both geometrically accurate and semantically structured, enabling navigation, manipulation, AR/VR, and human–robot interaction in indoor environments. Compared with purely geometric SLAM, semantic SLAM provides object-level scene understanding and editable priors that facilitate high-level planning, robust relocalization, and long-term map maintenance. Early systems typically store semantics in explicit voxel grids, point clouds, or signed distance fields (SDFs) \[1\], \[2\], \[3\], but their memory footprint and optimization cost grow rapidly with scene size and complexity, limiting scalability for large-scale dense mapping.

With the advent of differentiable rendering and learned scene representations, neural SLAM methods have largely followed two directions. Implicit-field-based approaches \[4\], \[5\], \[6\] represent a scene as continuous neural fields or multi-scale feature planes and use NeRF-style volumetric rendering to jointly optimize appearance and geometry. Several works extend this paradigm to semantics by embedding semantic information into multi-scale feature grids or Tri-plane fields \[7\], \[8\], \[9\], \[10\], promoting cross-view consistency and sharper boundaries. However, per-ray volumetric integration makes the runtime scale with image resolution and sampling density, which becomes expensive for high-resolution, dense semantic mapping.

In parallel, 3D Gaussian Splatting (3DGS) \[11\] has been widely adopted in SLAM, where recent methods represent the map as explicit 3D Gaussians rendered via differentiable splatting \[12\], \[13\], \[14\], \[15\], \[16\], \[17\], \[18\], \[19\]. These approaches are efficient and naturally support incremental updates and editing, and they have shown strong performance in dense RGB-D SLAM. Most existing semantic Gaussian SLAM systems, however, attach semantic parameters directly to each Gaussian primitive. Whether using low-dimensional “semantic colors” \[16\], high-dimensional embeddings \[17\], or hierarchical codes \[18\], \[19\], the semantic parameter count grows linearly with the number of Gaussians and the label dimension. This introduces substantial memory and bandwidth overhead in large or cluttered scenes, and it encourages semantics to be learned as local, primitive-level attributes, often leading to fragmented labels and inconsistent cross-view behavior. Moreover, many systems rely primarily on constant-velocity motion models for tracking \[12\], \[16\], \[17\], \[18\], which can be brittle under sharp turns, long baselines, or degraded visual conditions.

We argue that a more scalable and robust design is to decouple semantic representation from Gaussian primitives while still leveraging Gaussians as efficient anchors for geometry and appearance. To this end, we propose **TGS-SLAM**, a *Tri-plane Gaussian Splatting for Semantic SLAM* system, together with **TriDS**, a *Tri-plane Decoupled Semantic Representation*. In TGS-SLAM, 3D Gaussians store only geometry and appearance, while semantics are encoded in a shared coarse-to-fine Tri-plane field. Gaussian centers query TriDS to obtain semantic features, which are decoded into logits and splatted jointly with color and depth in a single rendering pass. We further adopt a geometry-first, two-stage optimization schedule to stabilize learning, and a hybrid tracking module that fuses constant-velocity prediction with ORB (Oriented FAST and Rotated BRIEF) + PnP pose proposals selected by render-based scoring to improve robustness in challenging motions.

Our contributions are threefold:

1. We propose TGS-SLAM, a semantic SLAM system with a hybrid Tri-plane–Gaussian representation that uses explicit 3D Gaussians for geometry/appearance and TriDS for semantics. By moving semantics from per-Gaussian label vectors to a shared continuous tri-plane field, the trainable semantic storage (incl. optimizer states) is decoupled from the number of Gaussians, substantially reducing semantic storage while alleviating semantic fragmentation and improving cross-view consistency.
2. We design a geometry-first, two-stage optimization strategy together with a single-pass multi-modal renderer that stabilizes training and reduces computational cost. We first optimize the geometry and appearance of 3D Gaussians under RGB-D supervision and only then enable semantics, avoiding training the semantic field on unstable geometry and keeping early iterations lightweight.
3. We develop a hybrid camera tracking scheme that combines constant-velocity prediction with an ORB+PnP proposal selected by render-based scoring. This improves pose initialization under challenging motions while keeping the tracking thread lightweight. Across Replica, ScanNet, and ScanNet++, TGS-SLAM achieves strong 3D semantic mapping performance and competitive pose/reconstruction accuracy, while reducing semantic parameters by $\sim 100\times$ compared with per-Gaussian one-hot encodings.

### A. Implicit Neural SLAM

Implicit neural representations have become a prominent paradigm for dense SLAM by modeling scenes as continuous functions optimized with differentiable rendering. iMAP \[4\] uses a single MLP for joint pose and scene optimization, NICE-SLAM \[5\] improves robustness with hierarchical voxel-grid features, and ESLAM \[6\] further adopts multi-scale Tri-plane features for scalable reconstruction.

Several works extend implicit SLAM to semantic mapping. SNI-SLAM, COS-SLAM, DNS-SLAM, and NIDS-SLAM \[7\], \[8\], \[9\], \[10\] incorporate semantic features into shared continuous fields, such as multi-scale grids or Tri-planes, and optimize them with image-space supervision.

Most of these methods rely on NeRF-style volumetric rendering, whose cost scales with the number of samples along each ray. In contrast, our method combines rasterization-based Gaussian splatting for geometry and appearance with a shared Tri-plane semantic field, providing a more efficient alternative for semantic SLAM.

### B. Gaussian SLAM and Semantic Representations

On the explicit mapping side, 3D Gaussian Splatting (3DGS) \[11\] has recently been adapted to SLAM. SplaTAM \[12\] represents scenes as 3D Gaussians and uses differentiable splatting for RGB-D reconstruction and tracking. Follow-up works further improve robustness, scalability, and semantic integration \[13\], \[14\], \[15\].

Most existing semantic Gaussian SLAM methods encode semantics as per-Gaussian attributes. SGS-SLAM \[16\] uses low-dimensional semantic channels, SemGauss-SLAM \[17\] adopts higher-dimensional semantic embeddings, and Hier-SLAM and Hier-SLAM++ \[18\], \[19\] organize categories hierarchically with compact per-Gaussian codes. In these methods, semantic storage and update cost generally grow with the number of Gaussians.

In contrast, our method models semantics as a shared Tri-plane field, while Gaussians remain responsible for geometry and appearance. This decouples semantic capacity from the number of primitives while preserving efficient Gaussian splatting for dense SLAM.

We now present TGS-SLAM. As shown in Fig. 1, the mapping thread runs in two stages. Stage 1 incrementally builds a 3D Gaussian map that models only geometry and appearance from RGB-D supervision. Stage 2 introduces TriDS, a coarse-to-fine semantic field: semantic features sampled at Gaussian centers are decoded into logits and splatted together with color and depth to jointly render RGB, depth, and semantics. In parallel, a separate tracking thread keeps all Gaussian parameters fixed and optimizes only the current camera pose by aligning rendered views with incoming RGB-D frames. In the following, we detail TriDS, the two-stage optimization schedule, and the Gaussian splatting and camera tracking modules.

**Fig. 1.**

TGS-SLAM pipeline. Mapping runs in two stages: **Stage 1** builds a 3D Gaussian map and optimizes geometry and appearance from RGB-D with photometric and depth losses; **Stage 2** enables **TriDS** (Tri-plane Decoupled Semantic Representation), where Gaussian centers query coarse-to-fine Tri-plane features to decode semantic logits, which are splatted together with color and depth in a single rendering pass under a pixel-wise segmentation loss. Tracking keeps the map fixed and optimizes only the current pose by aligning rendered and observed RGB-D.

### A. Tri-Plane Decoupled Semantic Representation

To avoid semantic parameters growing linearly with the number of Gaussians, we propose TriDS, a Tri-plane Decoupled Semantic Representation, which decouples semantics from individual Gaussian primitives by parameterizing a shared semantic field with tri-planes. TriDS encodes semantics on three orthogonal feature planes using a coarse-to-fine design, where each plane location stores a $d$ -dimensional semantic feature. Given a 3D query point, we project it onto the three planes, bilinearly interpolate the corresponding features, aggregate the tri-plane responses at each scale, and concatenate multi-scale features to form the semantic embedding queried by Gaussians.

#### 1) Advantages of TriDS for Gaussian SLAM

In prior semantic Gaussian SLAM systems, each Gaussian stores a $C$ -dimensional semantic vector \[14\], \[16\], \[17\], \[18\], \[19\], so the semantic parameter count scales as $\mathcal {O}(N_{\text{gauss}}\!\cdot C)$, where $N_{\text{gauss}}$ is the number of Gaussians and $C$ the number of semantic channels or classes. This tight coupling between semantics and primitives not only incurs large memory and bandwidth costs in cluttered scenes, but also makes semantics highly local: each Gaussian carries its own label vector with limited sharing across neighboring primitives.

In TriDS, semantics live only on shared 2D feature planes with $d$ channels, yielding $\mathcal {O}(\text{res}^{2}\!\cdot d)$ parameters (up to constant factors from plane aspect ratios), effectively independent of $N_{\text{gauss}}$. In our Replica \[20\] experiments, $N_{\text{gauss}}$ typically reaches on the order of $10^{5}$, whereas TriDS contains only on the order of $10^{4}$ grid cells per plane, so many Gaussians reuse the same 2D descriptors, leading to a large reduction in semantic parameters. This comparison concerns trainable semantic parameter storage (incl. optimizer states); we report end-to-end runtime and peak GPU memory in Section IV-C.

Moreover, because the semantic field is shared, multiple Gaussians in the same spatial region query overlapping TriDS features and update the same parameters. This turns semantics from purely per-primitive attributes into a shared, spatially structured field: Gaussians belonging to the same object naturally interact through TriDS, jointly refining the same descriptors across views. Empirically, this design encourages object-level, cross-view-consistent semantics and stabilizes optimization compared with learning independent label vectors for each Gaussian.

#### 2) Multi-Scale Planes, Resolution, and Initialization

To capture both global layout and fine details, we use a two-level TriDS structure: a coarse set of planes for global semantic structure and a fine set for local boundaries. Instead of fixing the number of pixels per plane, we specify target grid spacings $r_{\text{coarse}}$ and $r_{\text{fine}}$ in world units. Given the scene AABB, we derive the plane resolutions by dividing the scene extent along each axis by the corresponding spacing, so larger scenes automatically receive more pixels at the same physical resolution. In all experiments, we use the same world-unit spacing settings across scenes of different sizes. We found the method not overly sensitive to these spacing values within a reasonable range in our experiments. For each level, we construct three feature planes $F_{xy}$, $F_{xz}$, and $F_{yz}$ with channel dimension $d$ and spatial sizes compatible with the corresponding axes. Plane features are initialized with independent Gaussian noise of small variance (zero mean, standard deviation 0.01) \[6\].

#### 3) Semantic Feature Query and Decoding

When entering the semantic optimization stage, each Gaussian center $\mathbf {p}=(x,y,z)$ is first normalized to the canonical cube $[-1,1]^{3}$ using the scene AABB:

$$
\begin{align*}
 \tilde{\mathbf {p}} = 2\,\frac{\mathbf {p}-\mathbf {p}_{\min }}{\mathbf {p}_{\max }-\mathbf {p}_{\min }} - 1, \qquad \tilde{\mathbf {p}}=(\tilde{x},\tilde{y},\tilde{z}). \tag{1} 
\end{align*}
$$
 View Source

For both the coarse and fine semantic fields, we project $\tilde{\mathbf {p}}$ onto the three orthogonal planes and bilinearly interpolate the corresponding features. For level $l \in \lbrace \mathrm{c},\mathrm{f}\rbrace$ (coarse and fine), the aggregated Tri-plane feature is

$$
\begin{align*}
 \mathbf {f}_{s}^{\,l}(\tilde{\mathbf {p}}) = F_{xy}^{\,l}(\tilde{x},\tilde{y}) + F_{xz}^{\,l}(\tilde{x},\tilde{z}) + F_{yz}^{\,l}(\tilde{y},\tilde{z}). \tag{2} 
\end{align*}
$$
 View Source

We then concatenate the coarse and fine features to obtain the joint semantic feature:

$$
\begin{align*}
 \mathbf {f}_{\text{sem}}(\tilde{\mathbf {p}}) = \left[\, \mathbf {f}_{s}^{\,\mathrm{c}}(\tilde{\mathbf {p}}),\, \mathbf {f}_{s}^{\,\mathrm{f}}(\tilde{\mathbf {p}}) \,\right] \in \mathbb {R}^{2d}. \tag{3} 
\end{align*}
$$
 View Source

The concatenated feature is then fed into a shallow MLP to produce the semantic logits:

$$
\begin{align*}
 \mathbf {z}(\tilde{\mathbf {p}}) = \text{MLP}\!\left(\mathbf {f}_{\text{sem}}(\tilde{\mathbf {p}})\right). \tag{4} 
\end{align*}
$$
 View Source

The decoded logits at Gaussian centers are splatted to the image plane to produce per-pixel semantic predictions. Gaussians provide geometric support for rasterization, while semantic information is encoded in TriDS and decoded by the MLP.

#### 4) Discussion

TriDS is an efficiency-oriented factorized semantic representation. Parameterizing semantics with axis-aligned tri-planes and querying them at Gaussian centers enables high parameter efficiency and shared cross-view semantics, but may be less flexible than heavier dense 3D representations in highly cluttered regions, for thin structures, or for highly non-axis-aligned layouts. In addition, since TriDS is queried on Gaussian geometry, semantic predictions remain coupled to geometric accuracy. Our design targets indoor RGB-D semantic SLAM, where we prioritize parameter efficiency, shared semantics, and stable online optimization.

### B. Two-Stage Optimization Strategy

Jointly optimizing Gaussian geometry and TriDS from scratch is inefficient and unstable in the early part of a sequence. The semantic branch adds high-dimensional logits and extra channels to the renderer, and at the beginning the Gaussian centers and scales still change rapidly, causing the sampled locations on the TriDS planes to drift. We therefore adopt a *geometry-first, semantics-later* two-stage optimization strategy.

#### 1) Geometric Mapping Stage

In the initial stage, we optimize only the geometric and appearance parameters of the Gaussian map. Following SplaTAM \[12\], when a new keyframe is selected, we first identify regions in the current view that are not yet covered by the existing map based on depth and visibility, and initialize new 3D Gaussians from points sampled in these uncovered regions. With the associated camera poses fixed, we then jointly optimize the parameters of Gaussians visible in a set of $k$ optimization frames, i.e., $\lbrace \bm{\mu }_{i}, r_{i}, \mathbf {c}_{i}, \alpha _{i}\rbrace$ (see Section III-C), so that the rendered outputs match the RGB-D observations across multiple views.

We adopt the same $k$ -frame optimization window as SplaTAM \[12\], which always includes the current frame and the most recent keyframe. For the remaining $k\!-\!2$ frames, we use a mixed replay strategy: we select $\lfloor (k\!-\!2)/2 \rfloor$ highest-overlap keyframes with respect to the current view and sample the rest uniformly at random from the global keyframe set. In contrast, SplaTAM selects all $k\!-\!2$ replay frames purely by highest overlap. Here, overlap is measured by the number of 3D points back-projected from the current depth map that fall inside a keyframe frustum. Injecting globally sampled keyframes into each batch improves long-term coverage and mitigates forgetting that can arise from overly local, overlap-only optimization.

The total mapping loss in the first stage is

$$
\begin{align*}
 \mathcal {L}_{\text{map}}^{(1)} = \lambda _{\text{rgb}} \mathcal {L}_{\text{rgb}} + \lambda _{\text{depth}} \mathcal {L}_{\text{depth}}, \tag{5} 
\end{align*}
$$
 View Source

where $\mathcal {L}_{\text{rgb}}$ is a photometric reconstruction loss between the rendered and observed RGB images, implemented as a weighted sum of an $L_{1}$ color difference term and an SSIM-based structural term, and $\mathcal {L}_{\text{depth}}$ measures the discrepancy between rendered and observed depths. The coefficients $\lambda _{\text{rgb}}$ and $\lambda _{\text{depth}}$ control the relative weights of the color and depth losses. Since the semantic branch is inactive in this stage, the renderer outputs only color and depth, resulting in a lighter computation graph and faster convergence to a stable Gaussian map.

#### 2) Semantic–Geometric Joint Optimization Stage

Once the number of geometric iterations reaches $r_{\text{warm}} \times N_{\text{total}}$, where $N_{\text{total}}$ is the total number of mapping iterations and $r_{\text{warm}}\in (0,1)$ denotes the geometry-only warmup ratio, we unfreeze the TriDS semantic field and the semantic MLP, and start decoding semantic logits for the Gaussians involved in optimization. In this stage, we retain the geometric constraints $\mathcal {L}_{\text{rgb}}$ and $\mathcal {L}_{\text{depth}}$, and additionally render a semantic logit map $\mathbf {Z}_{\text{render}}$ for each optimization batch. Together with the corresponding pixel-wise semantic labels $\mathbf {Y}_{\text{gt}}$, we construct a segmentation loss $\mathcal {L}_{\text{seg}}$ using cross-entropy over valid pixels, providing per-pixel supervision between the rendered semantic predictions and the ground-truth segmentation. The total mapping loss in this stage is

$$
\begin{align*}
 \mathcal {L}_{\text{map}}^{(2)} = \lambda _{\text{rgb}} \mathcal {L}_{\text{rgb}} + \lambda _{\text{depth}} \mathcal {L}_{\text{depth}} + \lambda _{\text{seg}} \mathcal {L}_{\text{seg}}, \tag{6} 
\end{align*}
$$
 View Source

where $\lambda _{\text{seg}}$ controls the weight of the semantic term. End-to-end optimization of $\mathcal {L}_{\text{map}}^{(2)}$ propagates gradients jointly to the Gaussian geometry and appearance, the TriDS semantic field, and the semantic MLP.

Overall, this two-stage schedule keeps early iterations lightweight, avoids training TriDS while geometry is still changing rapidly, and anchors semantic learning on a more reliable geometric prior. This makes semantic gradients more stable and reduces the risk of inconsistent or noisy semantic maps. We note that delaying semantic supervision may temporarily under-utilize semantic cues in the earliest iterations, especially near object boundaries or semantically ambiguous regions. However, the geometry-first stage serves only as a short warmup rather than a hard commitment to a geometry-only solution. Once semantics are enabled, the segmentation loss is jointly optimized with the RGB and depth losses, and its gradients are back-propagated not only to TriDS and the semantic MLP, but also to the Gaussian geometry. This allows later semantic supervision to refine earlier geometric estimates and mitigates potential early-stage bias.

### C. Gaussian Splatting and Camera Tracking

#### 1) Differentiable Gaussian Splatting

We adopt the standard gsplat -based differentiable Gaussian splatting renderer \[12\], \[21\] to render color and depth from the Gaussian map. Following prior work, per-pixel compositing weights $w_{i}(\mathbf {u})$ are obtained by alpha compositing projected Gaussians along each ray. During the semantic–geometric joint optimization stage, we additionally query a semantic logit vector $\bm{\ell }_{i}$ at each Gaussian center $\bm{\mu }_{i}$ from TriDS and splat it using the same weights:

$$
\begin{align*}
 \mathbf {Z}(\mathbf {u}) = \sum _{i=1}^{n} \bm{\ell }_{i}\, w_{i}(\mathbf {u}), \tag{7} 
\end{align*}
$$
 View Source

where $\text{softmax}(\mathbf {Z}(\mathbf {u}))$ gives the semantic probability distribution at pixel $\mathbf {u}$. Thus, color, depth, and semantics are rendered within a unified splatting framework in a single pass.

#### 2) Camera Tracking

Given the current 3D Gaussian map, the tracking thread optimizes only the pose of the newly arriving RGB-D frame. A natural baseline is a constant-velocity motion model. Denoting the poses of frames $t-1$ and $t$ by $E_{t-1}$ and $E_{t}$, respectively, we extrapolate the pose of frame $t+1$ as

$$
\begin{align*}
 E_{t+1}^{\text{cv}} = E_{t} + (E_{t} - E_{t-1}), \tag{8} 
\end{align*}
$$
 View Source

where we use an additive notation for simplicity; in practice, the extrapolation is implemented in $\text{SE}(3)$ by composing relative pose increments. This assumes that the motion increment between successive frames remains approximately constant. It works well for smooth motion, but can yield poor initializations under sharp turns or large-baseline motions, making gradient-based refinement prone to wrong local optima and even causing pose divergence.

To improve robustness, we introduce an additional ORB+PnP-based geometric proposal. Between frames $t$ and $t+1$, we extract ORB features, match them, and obtain a set of 3D–2D correspondences $\lbrace (\mathbf {X}_{i}^{t}, \mathbf {u}_{i}^{\,t+1})\rbrace$ by back-projecting pixels in frame $t$ using its depth map and camera intrinsics. We then estimate the relative pose $T^{\text{orb}}_{t\rightarrow t+1}$ by solving a PnP problem under RANSAC, minimizing the reprojection error

$$
\begin{align*}
 \min _{T} \sum _{i} \bigl \Vert \pi \!\bigl (T \mathbf {X}_{i}^{t} \bigr) - \mathbf {u}_{i}^{\,t+1} \bigr \Vert _{2}^{2}, \tag{9} 
\end{align*}
$$
 View Source

where $\pi (\cdot)$ denotes the perspective projection. Composing this relative pose with the previous global pose yields a second candidate initialization:

$$
\begin{align*}
 E_{t+1}^{\text{orb}} = T^{\text{orb}}_{t\to t+1}\, E_{t}. \tag{10} 
\end{align*}
$$
 View Source

Instead of always trusting either constant velocity or ORB+PnP, we combine their strengths via render-based selection. For any candidate pose $E$, we render the color and depth $(\hat{I}(E), \hat{D}(E))$ from the current Gaussian map and compare them with the observation $(I_{t+1}, D_{t+1})$ using a weighted appearance-and-geometry loss:

$$
\begin{align*}
 \mathcal {L}(E) = \lambda _{\text{im}} \bigl \Vert \hat{I}(E) - I_{t+1} \bigr \Vert _{1} + \lambda _{\text{depth}} \bigl \Vert \hat{D}(E) - D_{t+1} \bigr \Vert _{1}. \tag{11} 
\end{align*}
$$
 View Source

For tracking efficiency, we do not render or supervise semantic logits here, so the tracking loss always involves only color and depth. We evaluate $\mathcal {L}(E)$ for both candidates, obtaining $\mathcal {L}_{\text{cv}}$ and $\mathcal {L}_{\text{orb}}$, and choose the better initialization as

$$
\begin{align*}
 E_{t+1}^{\text{init}} = {\begin{cases}E_{t+1}^{\text{orb}}, & \text{if } \mathcal {L}_{\text{orb}} < \mathcal {L}_{\text{cv}}, \\
 E_{t+1}^{\text{cv}}, & \text{otherwise}. \end{cases}} \tag{12} 
\end{align*}
$$
 View Source

This procedure only adds two forward renderings and a lightweight ORB+PnP computation, which is negligible compared to subsequent iterative refinement.

Starting from $E_{t+1}^{\text{init}}$, we fix all 3D Gaussian parameters and optimize the current-frame pose via gradient descent on the same color and depth losses, yielding the final estimate $E_{t+1}$. On challenging sequences such as ScanNet++, which exhibit large baselines and complex motion, this hybrid initialization substantially improves tracking robustness and helps prevent pose divergence.

### A. Experimental Setup

We evaluate TGS-SLAM on three standard benchmarks: the synthetic Replica dataset \[20\], the real-world ScanNet dataset \[22\], and ScanNet++ \[23\]. For ScanNet++, we use the DSLR RGB-D sequences from two scenes, 8b5caf3398 (S1) and b20a261fdf (S2), which feature larger baselines and more aggressive camera motion. SGS-SLAM \[16\] is used as the primary Gaussian-based semantic SLAM baseline.

#### 1) Metrics

Following prior work \[9\], \[16\], \[17\], \[18\], \[19\], we report semantic quality using mIoU on 2D semantic maps rendered from the reconstructed 3D Gaussian map (as a proxy for 3D semantic mapping quality). Trajectory accuracy is measured by ATE RMSE (cm). Geometric reconstruction is evaluated by the Depth L1 error (cm) between rendered and observed depth maps. We additionally report PSNR (dB), SSIM, and LPIPS for rendering quality. Efficiency is quantified by total parameter size, semantic-related parameter size, and runtime. All experiments are conducted on a single NVIDIA A40 GPU.

#### 2) Implementation Details

We set the coarse and fine semantic grid spacings to $r_{\text{coarse}}=24$ cm and $r_{\text{fine}}=3$ cm, respectively. Both coarse and fine Tri-planes use 16 channels per plane. The semantic decoder is a two-layer MLP with hidden dimension 128. During mapping, we use $\lambda _{\text{rgb}}=0.5$, $\lambda _{\text{depth}}=1$, and $\lambda _{\text{seg}}=0.5$. In the two-stage optimization, we set the geometry-only warmup ratio to $r_{\text{warm}}=0.2$. Unless otherwise specified, all other hyperparameters follow SGS-SLAM \[16\]. Our implementation assumes bounded indoor RGB-D scenes with a predefined scene AABB; online scene expansion and truly unbounded environments are not considered in this work.

### B. Experimental Results

#### 1) Evaluation on Synthetic Dataset

We first evaluate TGS-SLAM on the Replica \[20\] dataset. For a fair comparison with prior baselines, Table I reports mIoU on the first four Replica scenes, which are the scenes commonly reported in previous works. In addition to Gaussian-based semantic SLAM methods, we also include recent implicit neural field baselines for a broader comparison across representation families. TGS-SLAM achieves the best performance on all reported scenes and reaches an average mIoU of 97.02%, outperforming both Gaussian-based semantic SLAM systems and recent implicit-field baselines. Table II summarizes tracking and reconstruction metrics on Replica. TGS-SLAM matches the best average ATE (0.33 cm), attains the lowest depth error (0.47 cm), and achieves the highest image quality (PSNR 35.82 dB, SSIM 0.983) with a competitive LPIPS score. Overall, these results indicate that the proposed TriDS and two-stage optimization improve semantic mapping without degrading, and in practice slightly improving, overall SLAM accuracy.

**TABLE I** Semantic Segmentation mIoU (%) on Selected Replica Scenes. Methods are Grouped by Representation Family.

**TABLE II** Quantitative Results on Replica. We Report Averages Over All Scenes. “–” Means the Metric is Not Reported for That Method.

Fig. 2 provides qualitative semantic segmentation results on several representative views, comparing Hier-SLAM, SemGauss-SLAM, SGS-SLAM, our method, and the ground truth. TGS-SLAM yields sharper boundaries and more consistent semantics, especially near object interfaces and thin structures, closely matching the ground-truth labels.

**Fig. 2.**

Qualitative semantic segmentation comparison on Replica. Rows correspond to different scenes. Red boxes highlight regions where our method yields sharper boundaries and more consistent semantics than competing approaches.

To assess robustness under noisy supervision, we replace ground-truth labels on four Replica scenes with pseudo labels predicted by a fixed, off-the-shelf segmentation network built from a pretrained DINOv2 universal feature extractor \[24\] with an attached segmentation head. These pseudo labels are used as fixed supervision for both SGS-SLAM and our method, while evaluation is still performed against the ground-truth annotations.

Table III reports the resulting mIoU. The first row (*Supervision mIoU*) corresponds to the DINOv2 model itself. Trained only on its noisy predictions, TGS-SLAM attains an average mIoU of 87.50%, about 4 percentage points higher than SGS-SLAM and roughly 5.5 percentage points above the supervising network. This indicates that the TriDS, together with the stabilized 3D Gaussian geometry, effectively aggregates multi-view evidence and improves robustness to imperfect labels.

**TABLE III** Semantic Robustness to Noisy Supervision on the Replica Dataset. Supervision mIoU Reports the Performance of the Frozen DINOv2 Segmentation Network Used to Generate Pseudo Labels.

#### 2) Evaluation on Real-World Datasets

We next evaluate on ScanNet \[22\], a challenging indoor dataset with noisy depth and motion blur. Per-scene ATE RMSE is reported in Table IV. All methods suffer a drop in accuracy compared with Replica, but TGS-SLAM achieves an average ATE comparable to state-of-the-art Gaussian-based methods and clear improvements over early baselines on several sequences, showing that our representation and tracking remain robust in real-world conditions.

**TABLE IV** Per-Scene ATE RMSE on Selected ScanNet Sequences. Lower is Better

We further test on ScanNet++ \[23\], which provides higher-quality RGB-D sequences with larger inter-frame baselines and more aggressive camera motion. Table V compares SGS-SLAM \[16\], our primary Gaussian-based baseline, with TGS-SLAM on both novel-view and training-view reconstruction. In the top block, camera poses are estimated online by each SLAM system. Under this challenging setting, SGS-SLAM suffers severe degradation on camera S2, indicating unstable tracking. Benefiting from our hybrid constant-velocity + ORB+PnP initialization, TGS-SLAM maintains more stable tracking and substantially improves both reconstruction and semantics, particularly on S2, while remaining comparable on S1.

**TABLE V** Novel-View and Training-View Reconstruction Quality on a ScanNet++ Sequence, Where Ground-Truth Semantic Labels are Used for Supervision

To decouple mapping quality from tracking, the bottom block evaluates both methods using the provided reference poses. Even with accurate poses, TGS-SLAM achieves consistently higher mIoU for both novel and training views and comparable PSNR overall, indicating that TriDS improves mapping quality beyond pose robustness. Qualitative results in Fig. 3 further confirm cleaner geometry and sharper, more consistent semantics.

**Fig. 3.**

Qualitative novel-view rendering and semantic segmentation on ScanNet++ \[23\]. From top to bottom: SGS-SLAM, TGS-SLAM (ours), and ground truth. For each method, we show the RGB rendering and the corresponding semantic map for two representative viewpoints. TGS-SLAM produces cleaner geometry and sharper, more consistent semantics, especially along walls, furniture, and thin structures.

### C. Runtime Analysis

Table VI summarizes parameter storage and runtime on Replica. With TriDS, TGS-SLAM uses only 10 MB of semantic parameters, which is $\sim 100\times$ smaller than the one-hot variant of Hier-SLAM (1059 MB) and yields a 6– $30\times$ reduction compared with other Gaussian-based semantic SLAM baselines.

**TABLE VI** Runtime, Parameter Storage, and Peak GPU Memory Comparison on Replica

For runtime, TGS-SLAM achieves the fastest tracking among semantic Gaussian SLAM methods in Table VI, with 44.76 ms per tracking iteration and 1.79 s per frame under the default iteration settings, corresponding to approximately $2.5\times$ speedup over SGS-SLAM and over $3\times$ over Hier-SLAM and SemGauss-SLAM. Mapping is slower than SGS-SLAM but comparable to other semantic baselines. Note that SGS-SLAM reports the lowest peak memory partly because it encodes semantics as a 3D “semantic color” vector (RGB-like), rather than high-dimensional class logits/embeddings. Our reduction targets trainable semantic storage (incl. optimizer states), while peak GPU memory is additionally affected by rendering buffers and intermediate activations; since TriDS and the shallow decoder are lightweight, the peak-memory overhead remains modest in practice.

### D. Ablation Study Analysis

#### 1) TriDS vs. One-Hot and Low-Rank Per-Gaussian Representations

Table VII compares TriDS with two per-Gaussian semantic representations under the same setup: one-hot semantics and a low-rank per-Gaussian latent baseline. For the latter, each Gaussian is assigned a learnable 32-D latent vector and decoded by the same MLP as TriDS, matching the 32-D input formed by concatenating the 16-D coarse and 16-D fine tri-plane features.

**TABLE VII** Ablation on Semantic Representation on Replica. “one-Hot” Follows the Per-Gaussian One-Hot Encoding Used in Hier-SLAM. “per-Gaussian Latent” Denotes a 32-D Latent Vector Per Gaussian Decoded by the Same MLP as TriDS.

The low-rank per-Gaussian latent already improves over one-hot semantics, showing that replacing direct per-Gaussian semantic attributes with low-dimensional latent embeddings decoded by an MLP is beneficial. However, TriDS still achieves the best results, improving ATE, PSNR, and mIoU while reducing semantic parameters from 644 MB to 10 MB. This shows that the gain of TriDS comes not only from lower-dimensional semantic attributes, but also from the shared tri-plane representation, which provides spatially structured feature sharing across nearby Gaussians.

#### 2) Effect of the Two-Stage Optimization Schedule

Fig. 4 studies the warmup ratio, i.e., the fraction of mapping iterations devoted to geometry-only optimization before enabling semantics. A larger warmup ratio reduces average mapping time, since fewer iterations execute the full semantic branch, while mIoU remains high over a broad range. We set $r_{\text{warm}}=0.2$ by default, which stabilizes the Gaussian map and achieves near-maximal mIoU with reduced mapping time compared with a one-stage scheme.

**Fig. 4.**

Ablation on the two-stage optimization schedule on Replica. We vary the warmup ratio (fraction of iterations used for geometry-only training) from 0.0 to 0.9. Left: semantic mIoU. Right: average mapping iteration time (ms).

#### 3) Effect of the Hybrid Tracking Module

Table VIII ablates the ORB+PnP proposal in the hybrid tracker on the ScanNet++ S1 sequence. Over 20 independent runs, removing ORB+PnP reduces the success rate from 100% to 20%, while the ATE and mIoU of successful runs remain nearly unchanged. This indicates that ORB+PnP mainly improves tracking robustness and sequence completion under challenging motions.

**TABLE VIII** Ablation on the Hybrid Tracking Module on the ScanNet++ S1 Sequence. Success Rate is Measured Over 20 Independent Runs. ATE and mIoU are Averaged Over Successful Runs Only.

We have presented **TGS-SLAM**, a semantic SLAM system that combines 3D Gaussians for geometry and appearance with **TriDS**, a shared coarse-to-fine Tri-plane Decoupled Semantic Representation. By decoupling semantics from Gaussian primitives, TGS-SLAM breaks the linear dependence between semantic parameters and the number of Gaussians, and yields object-level, cross-view-consistent semantics. Together with a geometry-first two-stage optimization, single-pass RGB-D semantics rendering, and a hybrid constant-velocity + ORB+PnP tracking scheme, our system achieves strong 3D semantic mapping performance on Replica, ScanNet, and ScanNet++ with competitive pose and reconstruction accuracy, while reducing semantic parameters by about $100\times$ and remaining robust to noisy pseudo labels. TriDS currently assumes a bounded support volume (AABB). Future work will explore unbounded extensions via tiled/chunked tri-planes or submap-based semantic fields.