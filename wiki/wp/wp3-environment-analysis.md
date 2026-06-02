---
title: "WP3: Semantic Environment Analysis"
type: wp
tags: [wp3, semantic-analysis, object-detection, scene-understanding]
sources: [notes/SEA_and_PoAC(AP1)_Plan.md]
created: 2026-06-02
status: active
related: [[semantic-environment-analysis]], [[votenet]], [[pointpillars]]
---

# WP3: Semantic Environment Analysis

## Goal
Detect and understand objects, materials, and spatial relationships in the workspace to build a semantic map.

## Methods
| Sub-task | Methods |
|------ ----|-- ----- --|
| **WP3.1 Object Detection (RGB)** | YOLOv11, YOLOv8, RT-DETR, GroundingDINO (open-world) |
| **WP3.2 Object Detection (RGB-D)** | VoteNet, GroupFree3D, FCAF3D, PointPillars, CenterPoint, YOLOv8-Seg |
| **WP3.3 Scene Graph Generation** | Scene Graph Transformer, Few-Shot Object Detection |

## Integration
Semantic map feeds into WP5 (scene interpretation) and WP10 (task planning).

## Validation
Compare on mAP, segmentation IoU, scene graph quality (prec@k), and spatial reasoning accuracy.

## Sources
- [notes/SEA_and_PoAC(AP1)_Plan.md](../../raw/notes/SEA_and_PoAC(AP1)_Plan.md)
