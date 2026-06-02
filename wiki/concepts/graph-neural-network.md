---
title: "Graph Neural Networks (GNN) for Robotics"
type: concept
tags: [gnn, graph-learning, robotics, relationship-learning]
sources: [Project_Gemmas_General_Thoughts.md]
created: 2026-06-02
updated: 2026-06-02
status: draft
related: [[3d-semantic-scene-graph]], [[semantic-environment-analysis]]
---

# Graph Neural Networks (GNN) for Robotics

## Summary

**GNNs** are used in the Semantic Environment Analysis pipeline to learn relationships (edges) between objects in a scene graph. Given the nodes (objects with their 3D positions and semantic labels), a GNN predicts which pairs of objects have meaningful spatial or functional relationships — producing the edges of the 3DSSG.

## Key Points

- **Input**: nodes with features (3D position, class, affordance scores)
- **Output**: predicted edges with relationship types (On, Inside, Graspable, Obstacle)
- **Mechanism**: message-passing between nodes, edges predicted from node feature products
- **Use in pipeline**: Step D — 3D Scene Graph Construction

## How GNN Edges Differ from Rule-Based Edges

| | Rule-based | GNN-based |
|--|--|---|
| Edge types | Fixed rules | Learned relationships |
| Novel relations | No | Yes (if trained on diverse data) |
| Interpretability | High | Medium |
| Data needed | None | Labeled scene graphs |

## Sources

- [Project_Gemmas_General_Thoughts.md](../../raw/notes/Project_Gemmas_General_Thoughts.md)

## Questions

1. What GNN architecture is appropriate for scene graphs (GAT, GCN, GraphSAGE)?
2. Are there pre-trained GNN models for 3D scene understanding?
3. Can GNN edges be combined with rule-based edges (hybrid approach)?
