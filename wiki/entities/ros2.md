---
title: "ROS2 (Robot Operating System)"
type: entity
tags: [robot-middleware, ros, robotics, sensor-fusion]
sources: [Project_Gemmas_General_Thoughts.md]
created: 2026-06-02
updated: 2026-06-02
status: stable
related: [[semantic-environment-analysis]]
---

# ROS2 (Robot Operating System)

## Summary

**ROS2** is the middleware layer for robotics — the "nervous system" that connects sensors, algorithms, and actuators into a coherent pipeline. For the Semantic Environment Analysis tool, ROS2 is the essential glue that feeds RGB-D frames to the perception pipeline and routes the output scene graph to the action planner.

## Key Points

- **Essential** for the Semantic Environment Analysis architecture — handles sensor fusion and node communication.
- Supports real-time communication, unlike ROS1.
- Node-based: each pipeline stage is a ROS2 node (image subscriber → segmentation node → SLAM node → graph builder).
- Standard message types for image/depth data, transforms, TF2 coordinate frames.

## Role in the Pipeline

1. **Input:** RGB-D camera publishes sensor_msgs (image, depth)
2. **Bridge:** All pipeline stages communicate via ROS2 topics/services
3. **Coordination:** TF2 manages coordinate transforms between camera, robot base, and world frames
4. **Output:** Scene graph published as a ROS2 message or via a REST/gRPC service

## Why ROS2 Over Alternatives

- Industry standard, large ecosystem of packages
- Built-in support for distributed systems (nodes can run on different machines)
- Native support for real-time communication
- Strong hardware driver support (Intel RealSense, Azure Kinect)

## Sources

- [Project_Gemmas_General_Thoughts.md](../../raw/notes/Project_Gemmas_General_Thoughts.md)

## Questions

1. Should we use ROS2 Humble, Iron, or Rolling?
2. What's the overhead of running ROS2 on resource-constrained hardware (Jetson)?
3. Are there lightweight alternatives (e.g., CycloneDDS) if ROS2 is too heavy?
