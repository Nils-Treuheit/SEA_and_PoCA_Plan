---
title: "WP5: Initial Scene Interpretation"
type: wp
tags: [wp5, scene-interpretation, intent-inference, scene-graph]
sources: [notes/SEA_and_PoAC(AP1)_Plan.md]
created: 2026-06-02
status: active
related: [[semantic-environment-analysis]], [[scene-graph-transformer]], [[few-shot-object-detection]]
---

# WP5: Initial Scene Interpretation

## Goal
Build a structured representation of the scene (entities, relationships, goals, plans) from detected data.

## Methods
| Sub-task | Methods |
|------ ----|-- ----- --|
| **WP5.1 Scene Graph Construction** | Scene Graph Transformer, Few-Shot Object Detection |
| **WP5.2 Intent Inference** | Rule-based + learned models |

## Integration
Interpretation feeds into WP10 (task planning) and WP11 (action recommendation).

## Validation
Compare on scene graph quality (prec@k), intent inference accuracy, and novelty detection (new objects/actions).

## Sources
- [notes/SEA_and_PoAC(AP1)_Plan.md](../../raw/notes/SEA_and_PoAC(AP1)_Plan.md)
