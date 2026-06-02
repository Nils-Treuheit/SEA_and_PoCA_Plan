---
title: "Vision-Language Model (VLM) for Robotics"
type: concept
tags: [vlm, llm, robotics, reasoning, multimodal]
sources: [Project_Gemmas_General_Thoughts.md]
created: 2026-06-02
updated: 2026-06-02
status: stable
related: [[semantic-environment-analysis]], [[action-affordance]], [[3d-semantic-scene-graph]]
---

# Vision-Language Model (VLM) for Robotics

## Summary

In the Semantic Environment Analysis architecture, a **VLM serves as the top-level reasoning engine**. The system builds a structured representation (3DSSG) and then queries a VLM with a text summary of that representation to produce actionable, contextual advice — essentially asking the robot to "think out loud" about what to do next.

## Key Points

- VLMs bridge the gap between structured perception and open-ended reasoning.
- The scene graph is serialized into text, queried via natural language prompt.
- **GPT-4o** and **LLaVA** are the two candidate approaches (cloud API vs. local).
- This approach lets the robot handle novel situations not covered by rule-based planning.

## How the VLM Query Works

1. Scene graph → text summary: *"Detected: cup at (x,y,z), table at (x,y,z), tray at (x,y,z). Relationships: cup on table, tray on table."*
2. Query VLM: *"Based on detected objects and their positions, what is the most logical next step for a cleaning robot?"*
3. VLM returns: *"The robot should move to the table to clear the tray."*

## VLM Options

| Option | Pros | Cons |
|--------|------|------|
| **GPT-4o** (API) | State-of-the-art reasoning, handles novel situations well | Latency, cost, privacy, internet dependency |
| **LLaVA** (local) | No internet, full control, no cost | Lower reasoning quality, needs GPU, still maturing |

## Why VLM Over Traditional Planning?

Traditional planners (PDDL, RL) require domain-specific encoding and struggle with novel situations. VLMs can reason about novel object arrangements and produce contextually appropriate advice without retraining — trading compute and latency for flexibility.

## Sources

- [Project_Gemmas_General_Thoughts.md](../../raw/notes/Project_Gemmas_General_Thoughts.md)

## Questions

1. What latency does the VLM query add to the overall pipeline?
2. Can VLM output be cached/structured for downstream consumption?
3. Is there a hybrid approach: VLM for novel situations, PDDL/RL for routine ones?
