---
title: "WP4: Continuous Multi-Object Tracking"
type: wp
tags: [wp4, multi-object-tracking, re-id, occlusion-handling]
sources: [notes/SEA_and_PoAC(AP1)_Plan.md]
created: 2026-06-02
status: active
related: [[semantic-environment-analysis]], [[byte-tracker]], [[deepsort]]
---

# WP4: Continuous Multi-Object Tracking

## Goal
Maintain persistent identities for people and objects in the workspace over time and across cameras.

## Methods
| Sub-task | Methods |
|------ ----|-- ----- --|
| **WP4.1 Person Tracking** | ByteTrack, DeepSORT |
| **WP4.2 Multi-Object Tracking** | TrackR-CNN, DeepSORT, ByteTrack |

## Integration
Tracked trajectories feed into WP2 (trajectory prediction) and WP5 (scene interpretation).

## Validation
Compare on MOTA (tracking accuracy), ID-switch rate, and occlusion handling.

## Sources
- [notes/SEA_and_PoAC(AP1)_Plan.md](../../raw/notes/SEA_and_PoAC(AP1)_Plan.md)
