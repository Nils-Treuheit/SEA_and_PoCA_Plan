---
title: "Semantic Environment Analysis & Perception of Action Context"
type: synthesis
tags: [robotics, computer-vision, scene-graph, affordance, 3d-ssg]
sources: [Project_Gemmas_General_Thoughts.md]
created: 2026-06-02
updated: 2026-06-02
status: stable
related: [[3d-semantic-scene-graph]], [[semantic-slam]], [[action-affordance]], [[3d-semantic-mesh]]
---

# Semantic Environment Analysis & Perception of Action Context

## Summary

A multi-disciplinary system that transforms 2D RGB-D video streams into **3D Semantic Scene Graphs (3DSSG)** — enabling a robot to perceive not just *what* objects exist and *where* they are, but *what actions are possible* with respect to each object. This is the conceptual foundation for "Gemmas" (Semantic Environment Analysis tool).

## Key Points

- A 2D object detector is insufficient; the target is a persistent 3DSSG with spatial, functional, and action affordance relationships.
- The system is organized in 4 hierarchical layers: Perception, Mapping, Semantic, Context/Reasoning.
- The pipeline moves from 2D segmentation → 3D projection → semantic SLAM → scene graph → action reasoning.
- VLMs (GPT-4o, LLaVA) can serve as the top-level reasoning engine, querying the semantic scene description.
- The "killer app" feature is **Object Permanence** — maintaining 3D semantic position for objects outside the camera's field of view.

## Architecture: Four Layers

### 1. Perception Layer (The "Senses")
Processes raw RGB-D frames. Extracts geometry (from depth) and 2D semantics (from segmentation).

### 2. Mapping Layer (The "Memory")
Aggregates 2D data into a persistent 3D volumetric or point-cloud representation. Converts back-projected pixel points into world coordinates using camera intrinsic matrix K.

### 3. Semantic Layer (The "Knowledge")
Links 3D geometry to labels and attributes (e.g., "is graspable", "is heavy", "is supportable").

### 4. Context/Reasoning Layer (The "Intelligence")
Evaluates spatial relationships and action affordances. Produces actionable scene graphs that feeding into planners.

## Technical Pipeline

### A. 2D Semantic Segmentation & Instance Detection
- **SAM (Segment Anything Model)** — high-precision masking
- **Mask2Former** — real-time instance segmentation
- Output: mask M where each pixel p is assigned class C

### B. 2.5D to 3D Projection
- Uses camera intrinsic matrix K
- Back-projection: pixel coordinates (u, v) + depth d → 3D points (X, Y, Z)
- Aggregates into TSDF (Truncated Signed Distance Function) or Point Cloud
- **Tools:** Open3D, PCL (Point Cloud Library)

### C. Semantic SLAM
- Maps "semantic voxels" instead of just geometric points
- Prevents ghosting (object appearing in two places as the robot moves)
- **Kimera** or **Hydra** — frameworks for 3D mesh with semantic labels on each vertex

### D. 3D Scene Graph Construction
- Graph G = (V, E): nodes = objects/rooms/robot, edges = spatial + functional relationships
- **Spatial edges:** `Inside(cup, cupboard)`, `On(cup, table)`
- **Functional edges:** `Graspable(cup)`, `Obstacle(table)`
- **GNNs** learn relationships from geometric proximity + semantic labels
- Output: JSON Scene Graph for downstream planners (PDDL, RL agents)

### E. Action Affordance & VLM Reasoning
- **Affordance mapping:** geometric analysis per object (e.g., flat surface on cylinder top → high "support" affordance)
- **VLM integration:** pass semantic scene description as text to GPT-4o or LLaVA
- Query example: *"Based on detected objects (cup, table, tray) and their positions, what is the most logical next step?"*
- Result: executable action recommendation

## Recommended Tech Stack

| Component | Technology |
|-----------|-----------|
| Hardware | Intel RealSense D435i, Azure Kinect |
| Middleware | ROS2 |
| 3D Processing | Open3D (Python/C++) |
| Deep Learning | PyTorch |
| Segmentation | SAM, YOLOv8-seg |
| Mapping/SLAM | Kimera-Semantics |
| Reasoning | GPT-4o, LLaVA |

## Summary Workflow

1. **Input:** RGB-D frames → ROS2
2. **Feature Extraction:** Mask2Former on RGB → 2D boundaries
3. **Spatial Mapping:** Depth map projects boundaries into 3D point cloud
4. **Object Tracking:** SLAM ensures consistent identity across movement
5. **Relational Analysis:** Overlap checking creates spatial edges
6. **Action Output:** JSON Scene Graph for planner

## Killer App Feature: Object Permanence

When an object leaves the camera's field of view, the tool maintains its 3D semantic position in the Scene Graph. The robot can "remember" that a tool is behind another object without needing to see it. This requires tracking persistence logic — objects aren't deleted when occluded, just marked as occluded with their last-known 3D position and semantic state.

## Sources

- [Project_Gemmas_General_Thoughts.md](../../raw/notes/Project_Gemmas_General_Thoughts.md) — 2026-06-02, high confidence

## Questions

1. What are the computational latency requirements? Real-time at ~30 FPS or batch processing is acceptable?
2. How many objects/scene complexity does this need to handle simultaneously?
3. Is the system static (single room) or fully dynamic (robot moving through multiple spaces)?
4. What's the required accuracy for spatial relationships? Centimeter-level or coarse (bin-level)?
5. Should affordance scores be pre-computed or computed on-demand via VLM?
6. How does this integrate with the downstream action planner — what's the interface contract?
7. Is "Object Permanence" a Phase 2 feature or must it work from day one?
