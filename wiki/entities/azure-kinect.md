---
title: "Azure Kinect"
type: entity
tags: [hardware, depth-camera, rgb-d, microsoft]
sources: [Project_Gemmas_General_Thoughts.md]
created: 2026-06-02
updated: 2026-06-02
status: draft
related: [[intel-realsense-d435i]]
---

# Azure Kinect

## Summary

**Azure Kinect** is Microsoft's depth camera, offered as an alternative to the Intel RealSense D435i for the Semantic Environment Analysis tool. It provides a larger depth field of view and longer range, at the cost of higher compute and power requirements.

## Key Points

- **Active stereo + structured light**: dual approach for robust depth
- **Longer range** than D435i (~0.5–4.5m)
- **Higher FOV**: captures more of the scene per frame
- **Heavier**: more compute/power requirements
- **ROS2 support**: `azure_kinect_ros_driver` available

## Comparison to D435i

| Property | D435i | Azure Kinect |
|------|---------|-------------|
| Range | 0.2–3.5m | 0.5–4.5m |
| FOV | Narrower | Wider |
| Compute | Lower | Higher |
| IMU | Built-in | Built-in |
| USB vs. USB-C | USB 3.0 | USB-C (USB 3.1) |

## Sources

- [Project_Gemmas_General_Thoughts.md](../../raw/notes/Project_Gemmas_General_Thoughts.md)

## Questions

1. Does Azure Kinect work better for larger rooms?
2. Is there a successor (Azure Kinect DK 2)?
