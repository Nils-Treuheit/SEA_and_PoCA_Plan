---
title: "TSDF (Truncated Signed Distance Function)"
type: concept
tags: [3d-representation, surface-reconstruction, point-cloud, volumetric-mapping]
sources: [Project_Gemmas_General_Thoughts.md]
created: 2026-06-02
updated: 2026-06-02
status: draft
related: [[3d-semantic-mesh]], [[semantic-slam]]
---

# TSDF (Truncated Signed Distance Function)

## Summary

A **TSDF** is a volumetric 3D representation where each voxel stores the *signed distance* to the nearest surface, truncated to a fixed range. Voxels near a surface have distance values; voxels far from any surface are clamped. It's the foundational 3D representation for RGB-D fusion, where multiple depth frames are integrated into a coherent volumetric surface.

## Key Points

- **Signed distance**: + on one side of surface, - on the other
- **Truncated**: values clamped to ±threshold — focuses precision near surface
- **Fusion**: multiple depth frames are integrated by averaging voxel distances
- **Use in pipeline**: intermediate step between depth maps and 3D point cloud/mesh

## How It Works

1. Each RGB-D frame defines a local TSDF volume centered on the camera
2. Voxels are transformed into a global coordinate frame
3. Voxels are fused across frames by weighted averaging
4. Final surface extracted via marching cubes

## Why It's Mentioned

The Semantic Environment Analysis blueprint references TSDF as an option for the "Mapping Layer." It's one of two main approaches for aggregating 2D depth into 3D:

- **TSDF** — volumetric, great for smooth surfaces
- **Point Cloud** — explicit, great for sparse/irregular geometry

## Sources

- [Project_Gemmas_General_Thoughts.md](../../raw/notes/Project_Gemmas_General_Thoughts.md)

## Questions

1. How does TSDF integrate with semantic labels? (Can each voxel carry a class?)
2. What's the memory cost of a room-scale TSDF?
3. Does Kimera use TSDF under the hood?
