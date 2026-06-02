---
title: "3D Semantic Mesh"
type: concept
tags: [3d-representation, mesh, semantic-labeling, robotics]
sources: [Project_Gemmas_General_Thoughts.md]
created: 2026-06-02
updated: 2026-06-02
status: draft
related: [[semantic-slam]], [[semantic-environment-analysis]], [[tsdf]]
---

# 3D Semantic Mesh

## Summary

A 3D mesh where each **vertex carries a semantic label** (object class, affordance, attributes). Unlike a plain geometric mesh, a semantic mesh is a *knowledge-bearing* representation — every point in the surface knows what it belongs to, what it can do, and how it relates to other parts of the scene.

## Key Points

- Produced by Semantic SLAM frameworks (Kimera-Semantics, Hydra).
- Each vertex is labeled with object class + attributes (is_graspable, is_heavy, etc.).
- Enables "ghosting-free" mapping: an object seen from multiple viewpoints stays one labeled entity.
- The semantic mesh is the intermediate representation between raw sensor data and the scene graph.

## Why It Matters

The semantic mesh is the "persistent memory" of the system. It's the data structure that holds everything together between frames. Without it, each camera frame is a disconnected snapshot. With it, there's a continuous world model that the robot can query at any time.

## Sources

- [Project_Gemmas_General_Thoughts.md](../../raw/notes/Project_Gemmas_General_Thoughts.md)

## Questions

1. What's the memory footprint of a semantic mesh for a typical room-scale environment?
2. Can semantic meshes be streamed/updated in real-time without rebuilding from scratch?
3. How does this relate to TSDF volumes? Is a semantic mesh built on top of a TSDF?
