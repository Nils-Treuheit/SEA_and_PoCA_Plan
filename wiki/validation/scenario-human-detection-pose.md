---
title: "Scenario: Human Detection and Pose Estimation"
type: validation_scenario
tags: [validation, human-detection, pose-estimation]
sources: [notes/SEA_and_PoAC(AP1)_Plan.md]
created: 2026-06-02
status: active
related: [[wp1-human-detection-and-pose-understanding]]
---

# Scenario: Human Detection and Pose Estimation

## Objective
Validate WP1 methods on workspace footage with ground truth annotations.

## Setup
- **Dataset**: 1000 annotated images from factory cameras
- **Metrics**: mAP (detection), PCK (pose), FPS (latency)
- **Hardware**: Jetson AGX Orin

## Results Template
| Method | mAP | PCK@0.5 | FPS |
|------|---|------|---|
| YOLOv11 | | | |
| YOLOv10 | | | |
| RT-DETR | | | |
| GroundingDINO | | | |

## Pass Criteria
- mAP > 0.9 for person detection
- PCK@0.5 > 0.95 for pose
- Latency < 30 FPS on edge hardware

## Sources
- [notes/SEA_and_PoAC(AP1)_Plan.md](../../raw/notes/SEA_and_PoAC(AP1)_Plan.md)
