---
title: "Semantic SLAM"
type: concept
tags: [slam, semantic-segmentation, robotics, 3d-mapping]
sources: [Project_Gemmas_General_Thoughts.md]
created: 2026-06-02
updated: 2026-06-02
status: stable
related: [[semantic-environment-analysis]], [[3d-semantic-mesh]], [[kimera]]
---

# Semantic SLAM

## Summary

Semantic SLAM extends traditional Simultaneous Localization and Mapping by adding *semantic labels to spatial structures*. Instead of producing a purely geometric map (points, voxels, mesh), it produces a map where every element carries a class label — turning a "where am I?" map into a "what is where" map.

## Key Points

- Traditional SLAM: geometric map + robot pose estimation
- Semantic SLAM: geometric map + semantic voxel/mesh with class labels per vertex/voxel
- Solves the "ghosting" problem: objects don't appear in multiple locations as the robot moves
- Prevents object identity loss across frames and viewpoints

## Key Implementations

- **Kimera** — metric-semantic visual localization and mapping. Produces real-time semantic meshes.
- **Hydra** — hierarchical semantic mapping.
- **Kimera-Semantics** — specifically designed for building 3D semantic meshes from RGB-D input.

## How It Fits the Pipeline

Semantic SLAM is the "Mapping + Semantic" bridge. It takes the 2D segmentation masks (from SAM/Mask2Former), back-projects them into 3D, and maintains consistent identity as the camera/robot moves. Without semantic SLAM, each frame is a disconnected 2D snapshot — with it, there's a persistent world model.

## Sources

- [Project_Gemmas_General_Thoughts.md](../../raw/notes/Project_Gemmas_General_Thoughts.md)

## Questions

1. What's the compute budget for running Kimera in real-time?
2. Does Kimera handle occlusion well (object permanence problem)?
3. Can Kimera be extended to produce affordance labels, or does that need a separate pass?
