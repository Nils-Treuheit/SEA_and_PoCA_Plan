---
title: "Kimera (Semantic SLAM Framework)"
type: entity
tags: [slam, semantic-segmentation, robotics, visual-localization]
sources: [Project_Gemmas_General_Thoughts.md]
created: 2026-06-02
updated: 2026-06-02
status: stable
related: [[semantic-slam]], [[3d-semantic-mesh]], [[hydra-slam]]
---

# Kimera

## Summary

**Kimera** is a metric-semantic simultaneous localization and mapping framework that produces real-time 3D semantic meshes from monocular or stereo camera input. Each vertex in the output mesh carries a semantic label, making it the ideal mapping backbone for the Semantic Environment Analysis pipeline.

## Key Points

- **Kimera** (original): metric-semantic VIO + mapping. Real-time, robust to motion blur.
- **Kimera-Semantics**: specifically for building 3D semantic meshes from RGB-D input.
- **Hydra**: hierarchical extension — multi-level semantic mapping (fine-grained objects → coarse rooms).

## Strengths for This Project

- Ghosting-free: objects maintain identity across viewpoints
- Real-time capable
- Produces semantic meshes (not just point clouds)
- Open source, well-maintained

## Alternatives

- **Hydra** — hierarchical approach, better for multi-scale scenes
- **ORB-SLAM3 + semantic extension** — lighter weight but less semantic depth

## Sources

- [Project_Gemmas_General_Thoughts.md](../../raw/notes/Project_Gemmas_General_Thoughts.md)

## Questions

1. What hardware requirements does Kimera-Semantics have?
2. Does it support stereo + RGB-D simultaneously?
3. How does it handle dynamic objects (people moving in the scene)?
