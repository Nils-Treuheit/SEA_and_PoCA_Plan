---
title: "Object Permanence in Robotics"
type: concept
tags: [robotics, perception, tracking, scene-graph]
sources: [Project_Gemmas_General_Thoughts.md]
created: 2026-06-02
updated: 2026-06-02
status: stable
related: [[semantic-environment-analysis]], [[semantic-slam]], [[3d-semantic-mesh]]
---

# Object Permanence in Robotics

## Summary

**Object Permanence** is the ability to maintain a representation of an object *even when it's no longer visible* to the camera. When a cup is moved behind a table (leaving the camera's field of view), the robot "remembers" that the cup still exists at its last-known 3D position. This is the **"Killer App" feature** of the Semantic Environment Analysis tool.

## Key Points

- Without object permanence, every occlusion = "object lost" for the planner
- With object permanence, the robot navigates *around* occluded objects it remembers
- Objects that go out of view are marked `occluded` rather than deleted
- Last-known 3D position and semantic state persist in the scene graph

## How It Works

1. Object is tracked and visible → normal scene graph entry
2. Object moves out of view (occluded or left room) → entry marked `occluded`
3. Entry persists with last-known pose, class, and affordance scores
4. If object reappears → entry reactivated with updated pose
5. If object not seen for extended period → entry decayed/removed

## Why It's a "Killer App"

Most systems treat occluded objects as lost. Object permanence enables:
- Navigation through cluttered spaces (remembering what's behind other objects)
- Task continuity across viewpoint changes
- More intelligent planning ("the cup is on the counter behind the microwave — I can't see it but I know it's there")
- True understanding of physical scenes, not just visual snapshots

## Implementation Notes

- Requires persistence logic in the mapping layer (keep entries alive after last observation)
- Needs a decay/removal strategy (don't keep forever)
- Works best with SLAM (consistent coordinate frames)
- Scene graph queries must filter `visible` vs `occluded` entries

## Sources

- [Project_Gemmas_General_Thoughts.md](../../raw/notes/Project_Gemmas_General_Thoughts.md)

## Questions

1. What's the appropriate timeout before "forgetting" an occluded object?
2. Should we maintain a probabilistic pose (with uncertainty) rather than a deterministic one?
3. How does object permanence interact with dynamic objects (people, pets)?
