---
title: "Intel RealSense D435i"
type: entity
tags: [hardware, depth-camera, rgb-d, intel]
sources: [Project_Gemmas_General_Thoughts.md]
created: 2026-06-02
updated: 2026-06-02
status: draft
related: [[azure-kinect]]
---

# Intel RealSense D435i

## Summary

**Intel RealSense D435i** is a depth camera recommended as the primary hardware sensor for the Semantic Environment Analysis tool. It provides synchronized RGB + depth + IMU data, which is essential for the 2.5D-to-3D back-projection pipeline and for inertial-assisted SLAM.

## Key Points

- **RGB-D + IMU**: provides synchronized visual and inertial data
- **Stereo depth**: active stereo depth sensing (no infrared projector)
- **Working range**: ~0.2–3.5 meters
- **ROS2 support**: native driver available via `realsense2_camera`
- **Recommended**: primary sensor for this project

## Key Specs (approximate)

- Resolution: 1280x720 RGB, 848x480 depth
- FPS: 30 RGB, 90 depth
- Depth range: 0.2–3.5m
- IMU: 200 Hz
- Connectivity: USB 3.0

## Sources

- [Project_Gemmas_General_Thoughts.md](../../raw/notes/Project_Gemmas_General_Thoughts.md)

## Questions

1. Does D435i work well in low-light or high-contrast scenes?
2. How does its depth accuracy compare to Azure Kinect at range?
3. Is there a D435i successor available?
