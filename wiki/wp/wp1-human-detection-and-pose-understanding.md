---
title: "WP1: Human Detection and Pose Understanding"
type: wp
tags: [wp1, human-detection, pose, gesture]
sources: [notes/SEA_and_PoAC(AP1)_Plan.md]
created: 2026-06-02
status: active
related: [[semantic-environment-analysis]], [[yolov11]], [[openpose]], [[st-gcn]]
---

# WP1: Human Detection and Pose Understanding

## Goal
Detect humans in workspace and estimate their body poses, gestures, and actions in real-time.

## Methods
| Sub-task | Methods |
|----------|---------|
| **WP1.1 Human Detection** | YOLOv11, YOLOv10, RT-DETR, GroundingDINO |
| **WP1.2 Pose Estimation** | OpenPose, MMPose, ViTPose, MediaPipe Pose, MotionBERT |
| **WP1.3 Gesture Recognition** | ST-GCN, CTR-GCN |

## Integration
Pose data feeds into WP2 (action prediction). Gesture data feeds into WP5 (scene interpretation).

## Validation
Compare methods on detection accuracy, pose estimation precision, gesture recognition accuracy, and inference latency.

## Sources
- [notes/SEA_and_PoAC(AP1)_Plan.md](../../raw/notes/SEA_and_PoAC(AP1)_Plan.md)
