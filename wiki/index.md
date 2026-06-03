---
title: "Wiki Index"
type: index
---

# Wiki Index

> Last updated: 2026-06-02 15:30
> Total pages: 52 | Total sources: 9

## Entities
| Name | Summary | Updated |
|------|---------|---------|
| [kimera](entities/kimera.md) | Semantic SLAM framework (Kimera, Kimera-Semantics, Hydra) | 2026-06-02 |
| [segment-anything-model](entities/segment-anything-model.md) | Meta's foundation segmentation model | 2026-06-02 |
| [mask2former](entities/mask2former.md) | Real-time instance segmentation model | 2026-06-02 |
| [yolov8-seg](entities/yolov8-seg.md) | Lightest real-time segmentation option | 2026-06-02 |
| [ros2](entities/ros2.md) | Robot middleware / nervous system | 2026-06-02 |
| [intel-realsense-d435i](entities/intel-realsense-d435i.md) | Primary RGB-D camera (0.2–3.5m) | 2026-06-02 |
| [azure-kinect](entities/azure-kinect.md) | Alternative RGB-D camera (0.5–4.5m, wider FOV) | 2026-06-02 |
| [scene-graph-transformer](entities/scene-graph-transformer.md) | Transformer-based scene graph construction | 2026-06-02 |
| [few-shot-object-detection](entities/few-shot-object-detection.md) | Novel category detection with few examples | 2026-06-02 |
| [interaction-transformer](entities/interaction-transformer.md) | Transformer for HRI analysis | 2026-06-02 |
| [yolov8-3d](entities/yolov8-3d.md) | 3D bounding box detection from RGB-D | 2026-06-02 |
| [coco-dataset](entities/coco-dataset.md) | COCO dataset (330K images, 80 classes) | 2026-06-02 |
| [yolov8-seg-3d](entities/yolov8-seg-3d.md) | YOLOv8 instance segmentation extended for RGB-D | 2026-06-02 |
| [rt-detr](entities/rt-detr.md) | Real-time Detection Transformer | 2026-06-02 |
| [ntu-rgbd](entities/ntu-rgbd.md) | NTU RGB+D dataset (120K videos, 60 classes) | 2026-06-02 |

## Concepts
| Name | Summary | Updated |
|------|---------|---------|
| [3d-semantic-scene-graph](concepts/3d-semantic-scene-graph.md) | Graph representation with spatial + functional edges | 2026-06-02 |
| [semantic-slam](concepts/semantic-slam.md) | SLAM with semantic labels per voxel/vertex | 2026-06-02 |
| [action-affordance](concepts/action-affordance.md) | What actions objects allow/invite | 2026-06-02 |
| [3d-semantic-mesh](concepts/3d-semantic-mesh.md) | 3D mesh with semantic labels per vertex | 2026-06-02 |
| [vision-language-model](concepts/vision-language-model.md) | VLM as top-level reasoning engine | 2026-06-02 |
| [graph-neural-network](concepts/graph-neural-network.md) | GNNs for learning scene graph edges | 2026-06-02 |
| [object-permanence](concepts/object-permanence.md) | Maintaining occluded objects in memory | 2026-06-02 |
| [tsdf](concepts/tsdf.md) | Volumetric 3D surface representation | 2026-06-02 |

## Work Packages
| Name | Summary | Updated |
|------|---------|---------|
| [wp1: Human Detection and Pose Understanding](wp/wp1-human-detection-and-pose-understanding.md) | Detect humans + estimate poses/gestures in real-time | 2026-06-02 |
| [wp2: Human Action and Trajectory Prediction](wp/wp2-human-action-and-trajectory-prediction.md) | Predict where a person is going and what they'll do next | 2026-06-02 |
| [wp3: Semantic Environment Analysis](wp/wp3-environment-analysis.md) | Detect/understand objects, materials, spatial relationships | 2026-06-02 |
| [wp4: Continuous Multi-Object Tracking](wp/wp4-continuous-multi-object-tracking.md) | Maintain persistent IDs across cameras/time | 2026-06-02 |
| [wp5: Initial Scene Interpretation](wp/wp5-initial-scene-interpretation.md) | Build structured scene representation (entities, relationships, goals) | 2026-06-02 |
| [wp6: Human-Robot Interaction](wp/wp6-human-robot-interaction.md) | Analyze interaction quality, trust, and collaboration | 2026-06-02 |
| [wp7: Robot Control and Motion Planning](wp/wp7-robot-control-and-motion-planning.md) | Plan safe, efficient, socially-aware robot paths | 2026-06-02 |
| [wp8: Robot Motion and Trajectory Planning](wp/wp8-robot-motion-and-trajectory-planning.md) | Generate socially-compliant motion trajectories | 2026-06-02 |
| [wp9: Robot Control and Motion Generation](wp/wp9-robot-control-and-motion-generation.md) | Generate low-level control commands for planned motions | 2026-06-02 |
| [wp10: Human-Aware Task Planning](wp/wp10-human-aware-task-planning.md) | Plan robot tasks respecting human presence/intentions | 2026-06-02 |
| [wp11: Action Recommendation](wp/wp11-action-recommendation.md) | Recommend optimal robot actions based on context | 2026-06-02 |

## Syntheses
| Name | Summary | Updated |
|------|---------|---------|
| [semantic-environment-analysis](syntheses/semantic-environment-analysis.md) | Full pipeline architecture: Perception → Mapping → Semantic → Reasoning | 2026-06-02 |

## Roadmaps
| Name | Summary | Updated |
|------|---------|---------|
| [roadmap](../roadmap.html) | Interactive Gantt chart of project timeline: M1–M6 with key decisions and risks | 2026-06-02 |

## Milestones
| Name | Summary | Updated |
|------|---------|---------|
| [m1: Detection and Pose Estimation Pipeline](milestone/m1-detection-pipeline.md) | Deploy YOLOv11/MediaPipe baseline on edge hardware | 2026-06-02 |
| [m2: Action Prediction and Tracking](milestone/m2-action-prediction.md) | Integrate SlowFast + Social-LSTM baseline | 2026-06-02 |
| [m3: Semantic Map of Workspace](milestone/m3-semantic-map.md) | Build real-time semantic map with detected objects/materials | 2026-06-02 |
| [m4: Interaction Module](milestone/m4-interaction-module.md) | Deploy Interaction Transformer + trust evaluation pipeline | 2026-06-02 |
| [m5: Integrated Demo in Real Factory](milestone/m5-integrated-demo.md) | Deploy full SEA system in Test Case 1 (factory) | 2026-06-02 |
| [m6: Optimized Deployment and Publication](milestone/m6-optimized-deployment.md) | Optimize all modules for edge deployment + publish results | 2026-06-02 |

## Validation Scenarios
| Name | Summary | Updated |
|------|---------|---------|
| [scenario: Human Detection and Pose Estimation](validation/scenario-human-detection-pose.md) | Validate WP1 on workspace footage with ground truth | 2026-06-02 |
| [scenario: Object Detection](validation/scenario-object-detection.md) | Validate WP3 methods for 3D detection from LiDAR/RGB-D | 2026-06-02 |
| [scenario: Trajectory Prediction and Tracking](validation/scenario-traj-prediction.md) | Validate WP2/WP4 methods for prediction and tracking | 2026-06-02 |

## Imported Resources from ENABLING Project
| Name | Summary | Updated |
|------|---------|---------|
| [Automating 3D Dataset Generation with Neural Radiance Fields](../raw/articles/2025-04-29-Automating3DdetectionRadianceFields.md) | Automated pipeline using Radiance Fields for 3D dataset generation | 2026-06-02 |
| [Real-Time Digital Twin of TIAGo: A Framework for Synchronized Human-Robot Interaction](../raw/articles/2025-10-05-real-time-digital-twin-tiago.md) | Framework for synchronized human-robot interaction through digital twin | 2026-06-02 |
| [Synthetic Data Generation for Robotics: A Comprehensive Review](../raw/articles/2025-10-19-synthetic-dataset-generation-review.md) | Review of synthetic data generation techniques for robotics applications | 2026-06-02 |
| [Intent Recognition in Speech-to-Text Processing in Cognitive Assistive Systems](../raw/articles/2026-05-11-intent-recognition-speech-to-text-cognitive-assistive-systems.md) | Speech-to-intent recognition for human-robot interaction in elderly-care | 2026-06-02 |
| [Think Like a Team: Graph-Based Representation of Shared Mental Models](../raw/articles/2026-03-04-think-like-a-team-graph-based-shared-mental-models.md) | Temporal Shared Mental Model using TGNNs for human-agent collaboration | 2026-06-02 |
| [Eye Contact Based Engagement Prediction for dyadic Human-Robot Interaction](../raw/articles/2025-04-28-EyeContactBasedEngagementForHRI.md) | Engagement prediction using eye contact detection in HRI | 2026-06-02 |
| [Toward Truly Intelligent Autonomous Systems – A Taxonomy of Integrating LLMs into MBSE](../raw/articles/2025-04-28-TowardTrulyIntelligentAutonomousSystems.md) | Taxonomy for integrating LLMs into model-based systems engineering | 2026-06-02 |

## Drafts (temp/)
| Name | Summary | Updated |
|------|---------|---------|
| *(none)* | | |
