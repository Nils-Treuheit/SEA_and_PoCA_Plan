---
title: "ByteTrack"
type: entity
tags: [tracking, re-id, multi-object]
sources: [notes/SEA_and_PoAC(AP1)_Plan.md]
created: 2026-06-02
updated: 2026-06-02
status: draft
---

# ByteTrack

## Summary

Multi-object tracker that associates all detections (high and low confidence) to tracks via IoU matching and embedding. Proposed for WP4.2.

## Key Points

- **Use in plan**: WP4.2 — Continuous Multi-Object Tracking
- **Strengths**: Excellent occlusion handling, uses low-confidence detections, real-time
- **Limitations**: Requires good detector (YOLO) upstream

## Sources

- [notes/SEA_and_PoAC(AP1)_Plan.md](../../raw/notes/SEA_and_PoAC(AP1)_Plan.md)
