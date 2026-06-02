---
title: "WP2: Human Action and Trajectory Prediction"
type: wp
tags: [wp2, action-recognition, trajectory-prediction]
sources: [notes/SEA_and_PoAC(AP1)_Plan.md]
created: 2026-06-02
status: active
related: [[semantic-environment-analysis]], [[social-lstm]], [[slowfast]]
---

# WP2: Human Action and Trajectory Prediction

## Goal
Predict where a person is going and what they will do next in the workspace.

## Methods
| Sub-task | Methods |
|------ ----|-- ----- --|
| **WP2.1 Trajectory Prediction** | Social-LSTM, Trajectron++, Motion Transformer |
| **WP2.2 Action Recognition** | SlowFast, Video Swin Transformer, TimeSformer |

## Integration
Predicted actions feed into WP5 (scene interpretation). Predicted trajectories feed into WP4 (tracking).

## Validation
Compare on ADE/FDE (trajectory) and accuracy (action recognition) across real-world datasets.

## Sources
- [notes/SEA_and_PoAC(AP1)_Plan.md](../../raw/notes/SEA_and_PoAC(AP1)_Plan.md)
