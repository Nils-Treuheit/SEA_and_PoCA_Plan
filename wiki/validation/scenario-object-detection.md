---
title: "Scenario: Object Detection"
type: validation_scenario
tags: [validation, object-detection, 3d-detection]
sources: [notes/SEA_and_PoAC(AP1)_Plan.md]
created: 2026-06-02
status: active
related: [[wp3-environment-analysis]]
---

# Scenario: Object Detection

## Objective
Validate WP3 methods for 3D object detection from LiDAR and RGB-D sensors.

## Setup
- **Dataset**: KITTI/nuScenes + custom factory annotations
- **Metrics**: mAP, mATE, mASE, IoU@0.5
- **Hardware**: Jetson AGX Orin

## Results Template
| Method | mAP | mATE | mASE | FPS |
|------|---|---|---|--|
| VoteNet | | | | |
| GroupFree3D | | | | |
| PointPillars | | | | |
| CenterPoint | | | | |

## Pass Criteria
- mAP > 0.8 on factory objects
- mATE < 0.5m
- Real-time on edge hardware

## Sources
- [notes/SEA_and_PoAC(AP1)_Plan.md](../../raw/notes/SEA_and_PoAC(AP1)_Plan.md)
