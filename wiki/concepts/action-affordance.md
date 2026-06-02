---
title: "Action Affordance in Robotics"
type: concept
tags: [affordance, robotics, perception, action-recognition]
sources: [Project_Gemmas_General_Thoughts.md]
created: 2026-06-02
updated: 2026-06-02
status: stable
related: [[semantic-environment-analysis]], [[3d-semantic-scene-graph]], [[vision-language-model]]
---

# Action Affordance in Robotics

## Summary

**Affordance** describes what actions an object *allows or invites*. A cup affords grasping and pouring. A table affords placing things on it. A chair affords sitting. In the context of the Semantic Environment Analysis tool, affordance mapping converts geometric properties of a 3D object into quantitative "affordance scores" — enabling the robot to understand not just what things *are* but what it can *do* with them.

## Key Points

- Affordance is geometric: a flat surface on top of a cylinder = high "support" affordance.
- Affordance scores can be computed per-object from 3D geometry.
- They become the functional edges in a Scene Graph: `Graspable(cup)`, `Supportable(table)`, `Obstacle(table)`.
- VLMs can augment geometric affordance with contextual reasoning.

## Affordance Types

| Type | Description | Example |
|------|-------------|---------|
| **Graspable** | Object can be picked up/manipulated | cup, wrench, tray |
| **Supportable** | Object can bear weight | table, shelf, countertop |
| **Containable** | Object can hold others | cupboard, bowl, cup |
| **Sittable** | Object supports human weight | chair, stool, bench |
| **Obstacle** | Object blocks a path or target | box in walkway, closed door |
| **Approachable** | Object is accessible to robot gripper | mug within arm's reach |

## Computation Approaches

1. **Geometric heuristics:** surface normals, curvature, extent analysis on 3D mesh.
2. **GNN-based:** learn affordances from labeled 3D object datasets.
3. **VLM-based:** pass 3D description to GPT-4o/LLaVA for contextual affordance reasoning.
4. **Hybrid:** geometric scoring + VLM refinement for contextual awareness.

## Sources

- [Project_Gemmas_General_Thoughts.md](../../raw/notes/Project_Gemmas_General_Thoughts.md)

## Questions

1. What datasets exist with pre-labeled affordance annotations for 3D objects?
2. Can affordance scoring be done purely from geometry, or does texture/color matter?
3. How do affordances change contextually? (Same cup, different task = different affordances?)
