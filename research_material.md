# 📚 Research Roadmap: Semantic Robot Interaction

This reading list is designed to support the development of the **SEA (Semantic Environment Awareness)** project, moving from raw sensor perception to high-level task reasoning.

---

## 🟢 Phase 1: Robust Perception & Detection
**Goal:** *Achieve reliable detection of humans, objects, and environmental boundaries in dynamic settings.*

### 1. Human & Object Detection
*Focus: Reaching the high precision/recall required for safe operation.*
*   **Foundations:**
    *   *YOLO Series (v8/v11):* Understanding the evolution of real-scale, high-speed detectors.
    *   *DeepSORT / ByteTrack:* The state-of-the-art in multi-object tracking (MOT) for low-latency environments.
*   **Robustness in Dynamic Scenes:**
    *   *Papers on "occlusion-aware" detection:* How to maintain tracks when humans pass behind objects.

### 2. 3D Scene Reconstruction & Segmentation
*Focus: Building the geometric foundation of the environment.*
*   **3D Object Detection:**
    *   *VoteNet:* Learning 3D object detection from point clouds.
    *   *PointNet / PointNet++:* The fundamental architectures for processing unstructured 3D data.
*   **Semantic Segmentation:**
    *   *Mask R-CNN:* The foundation for instance segmentation in 2D and its projection into 3D.

---

## 🟡 Phase 2: Semantic Scene Understanding
**Goal:** *Transform geometric data into structured, relational knowledge (3D Scene Graphs).*

### 1. 3D Scene Graphs (3DSGs)
*Focus: Modeling the "meaning" of the environment through relationships.*
*   **Relational Modeling:**
    *   *3D Scene Graph Generation:* Research on building hierarchical graphs (Object $\rightarrow$ Part $\rightarrow$ Room) from sensor data.
    *   *Semantic SLAM:* Integrating semantic labels into Simultaneous Localization and Mapping.
*   **Relationship Extraction:**
    *   *Affordance Learning:* Identifying object functionalities (e.g., "graspable", "pushable") based on geometry.

### 2. Affordance & Interaction
*   **Affrondance Mapping:** Understanding interaction possibilities in the scene.

---

## 🔴 Phase 3: Reasoning & Action
### 2. Reasoning & Action
*   **Reasoning:** Using LLMs (like GPT-4) to interpret scene descriptions and plan actions.
*   **Execution:** Translating high-level plans into robot motion primitives.
