---
title: "Mask2Former"
type: entity
tags: [segmentation, computer-vision, deep-learning]
sources: [Project_Gemmas_General_Thoughts.md]
created: 2026-06-02
updated: 2026-06-02
status: stable
related: [[segment-anything-model]], [[yolov8-seg]]
---

# Mask2Former

## Summary

**Mask2Former** is a real-time instance segmentation model that extends Mask R-CNN with a mask classification formulation. Unlike SAM (which is prompt-based and slower), Mask2Former processes an entire image in one forward pass, producing instance masks for all objects in a predefined set of classes — making it suitable for the real-time streaming requirements of the Semantic Environment Analysis pipeline.

## Key Points

- **Real-time**: processes full image in one pass (not pixel-by-pixel like SAM)
- **Instance segmentation**: produces per-object masks + class labels
- **Class-aware**: knows specific object categories (cup, table, etc.)
- **Used in pipeline**: Step A — 2D semantic segmentation & instance detection

## Comparison to SAM

| Property | Mask2Former | SAM |
|------|---------|---|
| Speed | Fast (real-time) | Slower (per-prompt) |
| Classes | Predefined set | Any object (zero-shot) |
| Precision | Good | High |
| Latency | Low | Higher |
| Use case | Streaming pipeline | Post-hoc refinement |

## Sources

- [Project_Gemmas_General_Thoughts.md](../../raw/notes/Project_Gemmas_General_Thoughts.md)

## Questions

1. What classes does Mask2Former support out of the box?
2. Can it be fine-tuned for custom object categories?
3. Is there a real-time-optimized version (e.g., for Jetson hardware)?
