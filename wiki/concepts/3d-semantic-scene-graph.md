---
title: "3D Semantic Scene Graph (3DSSG)"
type: concept
tags: [scene-graph, 3d, robotics, spatial-representation]
sources: [Project_Gemmas_General_Thoughts.md]
created: 2026-06-02
updated: 2026-06-02
status: stable
related: [[semantic-environment-analysis]], [[semantic-slam]], [[graph-neural-networks]]
---

# 3D Semantic Scene Graph (3DSSG)

## Summary

A 3DSSG is a graph-based representation of a robot's environment where **nodes** represent objects, rooms, or the robot itself, and **edges** encode both spatial and functional relationships between them. It goes beyond geometry by attaching semantic labels and affordance information to each node and edge — making it the bridge between perception and action.

## Key Points

- Nodes (V): objects (cup), rooms (kitchen), the robot itself.
- Edges (E): two types — spatial (topology) and functional/action-oriented.
- GNNs can learn edge relationships from geometric proximity + semantic labels.
- Output format: JSON Scene Graph consumable by planners (PDDL, RL).

## Spatial vs Functional Edges

- **Spatial:** `Inside(cup, cupboard)`, `On(cup, table)`, `Adjacent(cup, spoon)`
- **Functional/Action:** `Graspable(cup)`, `Obstacle(table)`, `Supportable(tray)`

## Why It Matters

A 3DSSG transforms a raw point cloud into an *understandable* world model. Instead of seeing "a set of points near the table", the robot reads "a cup is on a table, and the cup is graspable." This is the minimal representation needed for any autonomous reasoning or planning system.

## Sources

- [Project_Gemmas_General_Thoughts.md](../../raw/notes/Project_Gemmas_General_Thoughts.md)

## Questions

1. What existing datasets support 3DSSG training/evaluation?
2. Are there open-source 3DSSG visualization tools?
3. What's the trade-off between GNN-based edge inference vs. rule-based edge inference?
