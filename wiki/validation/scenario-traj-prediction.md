---
title: "Scenario: Trajectory Prediction and Tracking"
type: validation_scenario
tags: [validation, trajectory-prediction, tracking]
sources: [notes/SEA_and_PoAC(AP1)_Plan.md]
created: 2026-06-02
status: active
related: [[wp2-human-action-and-trajectory-prediction]], [[wp4-continuous-multi-object-tracking]]
---

# Scenario: Trajectory Prediction and Tracking

## Objective
Validate WP2 and WP4 methods for human trajectory prediction and multi-object tracking.

## Setup
- **Dataset**: ETH/UCY + custom factory tracking footage
- **Metrics**: ADE, FDE, MOTA, ID-switch rate
- **Hardware**: Jetson AGX Orin

## Results Template
| Method | ADE | FDE | MOTA | ID-switch |
|------|---|---|---|--|
| Social-LSTM | | | | |
| Trajectron++ | | | | |
| ByteTrack | | | | |
| DeepSORT | | | | |

## Pass Criteria
- ADE < 0.5m for trajectory prediction
- MOTA > 0.7 for tracking
- ID-switch rate < 5%

## Sources
- [notes/SEA_and_PoAC(AP1)_Plan.md](../../raw/notes/SEA_and_PoAC(AP1)_Plan.md)
