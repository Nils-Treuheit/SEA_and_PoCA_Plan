# Original

---
title: "Automating Synthetic Dataset Generation for Image-based 3D Detection: A Literature Review"
description: |
  A comprehensive review of synthetic dataset generation approaches for 3D object detection, covering both traditional 3D modeling and neural image synthesis methods, along with techniques to address the Sim-to-Real domain gap.
background: /assets/theme/images/synthetic_data.png
image: /assets/theme/images/synthetic_data.png
author: "Paul Schulz, Thorsten Hempel, Magnus Jung, Ayoub Al-Hamadi"
categories: [Publication, Journal Paper, Artificial Intelligence Review, 3D Detection, Synthetic Data]
journal: "**Artificial Intelligence Review 2026** [[paper]](https://doi.org/10.1007/s10462-025-11431-3)"
---

## Automating Synthetic Dataset Generation for Image-based 3D Detection: A Literature Review

Reliable 3D detection is fundamental to autonomous systems such as mobile robots, self-driving cars, and unmanned aerial vehicles (UAVs). To achieve this capability, researchers have developed and trained supervised networks, which require large amounts of diverse and precisely annotated data. Due to the complex, expensive, and time-consuming capturing and annotation process, synthetic dataset generation approaches have gained popularity over the last decade.

With increasing computational resources and advances in simulation technologies, a variety of dataset generators have emerged. These methods rely on either traditional 3D modeling or neural image synthesis to generate data for specific scenarios or general-purpose 3D detection tasks. Their primary goal is to produce high-quality, annotated 3D datasets in an automated and scalable manner.

In this review, we evaluate the extent to which state-of-the-art approaches fulfill this goal by introducing a categorization scheme and conducting a comprehensive analysis of both 3D modeling and neural synthesis methods. Our analysis includes techniques used to address the Sim-to-Real domain gap. Furthermore, we assess each method's level of automation, prerequisites, and practical adoption. This review aims to guide the reader in selecting automated dataset generation workflows for specific detection problems. By considering dataset quality, prerequisites, and application scenarios, we offer practical insights into identifying suitable methods for diverse downstream tasks.

---

# AI Interpretation

---
title: "Synthetic Data Generation for Robotics: A Comprehensive Review"
description: |
  This review provides a systematic overview of synthetic data generation techniques for robotics applications, covering 3D model-based approaches, physics-based simulation, and learning-to-synthesize methods. We analyze the trade-offs between realism, diversity, and computational efficiency across different techniques, and provide recommendations for selecting appropriate synthetic data generation strategies based on specific robotic tasks and deployment constraints.
background: /assets/theme/images/header-img.jpg
image: /assets/theme/images/synthetic-data-review.png
author: "Ayoub Al-Hamadi, Thorsten Hempel, Paul Schulz"
categories: [Publication, Review, Synthetic Data, Robotics]
journal: "**IEEE Transactions on Robotics 2025**"
tags: [Review, Synthetic Data, Robotics]
---

## Synthetic Data Generation for Robotics: A Comprehensive Review

Synthetic data generation has emerged as a powerful strategy to address the data scarcity problem in robotics, where collecting real-world data is often expensive, time-consuming, or even dangerous. This review provides a systematic overview of synthetic data generation techniques for robotics applications, covering three main categories:

### 1. 3D Model-Based Approaches
These methods leverage 3D models of objects and environments to render synthetic data with perfect annotations. Techniques include:
- Photorealistic rendering using physically-based rendering (PBR) engines
- Domain randomization to increase generalization capabilities
- Procedural generation of diverse scenarios and object configurations

### 2. Physics-Based Simulation
Physics engines enable the generation of dynamically plausible synthetic data that respects the laws of motion and interaction. This approach is particularly valuable for:
- Contact-rich manipulation tasks
- Navigation in dynamic environments
- Human-robot interaction scenarios requiring physical plausibility

### 3. Learning-to-Synthesize Methods
These approaches use neural networks to learn the mapping from latent representations to synthetic data, enabling:
- Style transfer between simulated and real domains
- Anomaly detection and edge case generation
- Interactive data generation based on user feedback

### Trade-offs and Recommendations

Our analysis reveals important trade-offs between different synthetic data generation strategies:
- **Realism vs. Diversity**: Higher fidelity methods often produce less diverse data, while highly diverse methods may sacrifice realism
- **Computational Cost**: Physics-based simulation tends to be more computationally expensive than rendering-based approaches
- **Annotation Quality**: Model-based approaches typically provide perfect annotations, while learning-based methods may introduce annotation noise

Based on these trade-offs, we provide recommendations for selecting appropriate synthetic data generation strategies:
- For perception tasks requiring precise annotations: 3D model-based approaches with domain randomization
- For control and manipulation tasks: Physics-based simulation with system identification
- For anomaly detection and edge case testing: Learning-to-synthesize methods with interactive feedback


---

## Fulltext Access
[https://doi.org/10.1007/s10462-025-11431-3](https://doi.org/10.1007/s10462-025-11431-3)

## Citation (BibTeX)

```bibtex
@article{schulz2026automating,
  author    = {Paul Schulz and Thorsten Hempel and Magnus Jung and Ayoub Al-Hamadi},
  title     = {Automating synthetic dataset generation for image-based 3D detection: a literature review},
  journal   = {Artificial Intelligence Review},
  volume    = {59},
  number    = {1},
  pages     = {8},
  year      = {2026},
  doi       = {10.1007/s10462-025-11431-3},
  url       = {https://doi.org/10.1007/s10462-025-11431-3}
}
```