---
title: "SAM (Segment Anything Model)"
type: entity
tags: [segmentation, computer-vision, sam, meta]
sources: [Project_Gemmas_General_Thoughts.md]
created: 2026-06-02
updated: 2026-06-02
status: stable
related: [[mask2former]], [[yolov8-seg]], [[semantic-segmentation]]
---

# SAM (Segment Anything Model)

## Summary

**SAM** (Segment Anything Model) is Meta's foundation model for image segmentation. Given an image and any prompt (point, box, or text), SAM produces a high-quality segmentation mask. It's designed for *zero-shot generalization* — it can segment objects it was never explicitly trained on.

## Key Points

- **Promptable segmentation**: works with points, boxes, or text prompts
- **Zero-shot**: generalizes to unseen objects/domains without retraining
- **High precision**: produces pixel-accurate masks
- **Trade-off**: computationally expensive — not ideal for real-time without optimization
- **Use in pipeline**: best for post-hoc refinement of object boundaries, not for real-time streaming

## Alternatives

- **Mask2Former** — real-time instance segmentation, slightly less precise but faster
- **YOLOv8-seg** — real-time, lighter weight, less precise than SAM

## When to Use SAM vs Alternatives

| Scenario | Recommendation |
|----------|---------------|
| Real-time streaming | Mask2Former or YOLOv8-seg |
| High-precision masking | SAM |
| Limited compute | YOLOv8-seg |
| Best overall quality | SAM, with Mask2Former as fallback |

## Sources

- [Project_Gemmas_General_Thoughts.md](../../raw/notes/Project_Gemmas_General_Thoughts.md)

## Questions

1. Has Meta released an optimized/deployable SAM variant (SAM2, MobileSAM)?
2. Can SAM handle 3D point clouds (SAM3D)?
3. What's SAM's memory footprint on GPU?
