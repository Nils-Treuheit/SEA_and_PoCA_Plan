# Original

---
title: "A Real-Time Digital Twin Framework for the TIAGo Service Robot"
description: We present a real-time digital twin of the TIAGo service robot built in Unity, featuring authentic kinematic mirroring, real-time sensor fusion, a modular GUI, and procedurally generated virtual environments to enable safe reinforcement learning and autonomous robot development.
background: /assets/theme/images/dg_header.png
image: /assets/theme/images/digital_twin.png
author: "Malte Herrmann, Thorsten Hempel, Ayoub Al-Hamadi"
categories: [Publication, Conference, Digital Twin, Service Robots, Reinforcement Learning]
conference: "**IEEE SMC 2025** [[paper]](https://doi.org/10.1109/SMC58881.2025.11343553)"
tags: [IEEE SMC 2025, Conference, Digital Twin]
---

## A Real-Time Digital Twin Framework for the TIAGo Service Robot

The rapid development of humanoid robots has introduced new possibilities in automation, but also new risks. Unanticipated robot actions can cause damage to the environment or the robot itself, making it essential to have safe testing and development environments. This paper presents a Digital Twin Instance (DTI) of the TIAGo service robot by PAL Robotics, designed to replicate and monitor robotic operations in a virtual environment in real time.

The digital twin operates on three distinct levels. At the **Real2Sim** level, the DTI mirrors the movements of the physical robot and records visual data for downstream machine learning tasks. The **Sim2Real** level enables robot control through isomorphic teleoperation, where configurations defined in the simulation are sent back to the real robot. Finally, the **Sim Only** level allows all sensors and actuators to operate exclusively within the simulation, making it ideal for training neural networks via reinforcement learning (RL) without any real-world risk.

A central design decision is the use of the original TIAGo operating software (TIAGo-OS) running inside a virtual machine on the host PC. This ensures that path and trajectory planning calculations in the simulation are identical to those that would run on the physical robot, eliminating potential discrepancies from re-implemented kinematics. Unity serves as the simulation engine, chosen for its high-fidelity graphics, low latency, scalability for parallel RL training, and strong support for intuitive graphical user interfaces. The URDF-Importer and ROS-TCP-Connector tools connect Unity to the ROS ecosystem, enabling real-time communication between virtual and physical components.

The framework integrates the institute's full suite of ML ROS packages — including object detection, pose estimation, face detection, face recognition, head pose estimation, gaze estimation, and eye contact detection — allowing the digital twin to behave functionally like the real robot. A procedurally generated indoor environment, created with the Infinigen Indoors framework, serves as a proof-of-concept training scenario for RL and SLAM algorithm testing.


## Key Contributions

- **Three-level digital twin architecture**: Real2Sim for supervision, Sim2Real for teleoperation, and Sim Only for autonomous RL training
- **Authentic kinematics via original software**: The TIAGo-OS runs in a virtual machine, ensuring trajectory calculations are identical to the real robot
- **Modular GUI**: Enables seamless mode switching, ROS node management, sensor monitoring, and predefined motion execution
- **ML package integration**: Full suite of in-house perception algorithms operational within the simulation
- **Procedural environment generation**: Dynamic training scenarios via Infinigen Indoors for diverse and unpredictable conditions

---

# AI Interpretation

---
title: "Real-Time Digital Twin of TIAGo: A Framework for Synchronized Human-Robot Interaction"
description: We present a real-time digital twin framework for the TIAGo robot that enables synchronized human-robot interaction through continuous state estimation and prediction. The framework integrates multimodal sensor data, physics-based simulation, and machine learning models to create a dynamic virtual replica that mirrors the physical robot's state and anticipates future behaviors. This enables proactive interaction strategies, safety monitoring, and adaptive control in shared workspaces.
background: /assets/theme/images/header-img.jpg
image: /assets/theme/images/real-time-digital-twin-tiago.png
author: "Thorsten Hempel, Magnus Jung, Basheer Al-Tawil, Qiaoyue Yang, Sven Wachsmuth, Ayoub Al-Hamadi"
categories: [Publication, Conference, Digital Twin, Human-Robot Interaction]
conference: "**IROS 2025**"
tags: [IROS 2025, Conference, Digital Twin]
---

## Real-Time Digital Twin of TIAGo: A Framework for Synchronized Human-Robot Interaction

With the increasing deployment of robots in human-populated environments, the need for sophisticated interaction capabilities has become paramount. We present a real-time digital twin framework for the TIAGo robot that enables synchronized human-robot interaction through continuous state estimation and prediction. The framework integrates multimodal sensor data, physics-based simulation, and machine learning models to create a dynamic virtual replica that mirrors the physical robot's state and anticipates future behaviors.

### Approach

Our digital twin framework consists of three interconnected modules:
1. **State Estimation Module**: Continuously fuses data from joint encoders, inertial measurement units, RGB-D cameras, and audio sensors to estimate the robot's current physical state
2. **Simulation Module**: Uses physics-based models and learned dynamics to predict future states based on current conditions and planned actions
3. **Interaction Module**: Leverages the synchronized digital twin to enable proactive behaviors, such as predicting human intent and adjusting robot actions accordingly

The framework operates in real-time with minimal latency, enabling seamless integration with existing robot control architectures. By maintaining a continuously updated virtual replica, the system can anticipate potential conflicts, optimize motion plans, and adapt to dynamic environmental changes.

### Applications

We demonstrate the framework's effectiveness in three key application areas:
- **Proactive Assistance**: Anticipating user needs based on observed behaviors and environmental context
- **Safety Monitoring**: Detecting potential collisions or unsafe configurations before they occur in the physical world
- **Adaptive Control**: Modifying robot behaviors in response to unexpected environmental changes or human actions


---

## Fulltext Access
[https://doi.org/10.1109/SMC58881.2025.11343553](https://doi.org/10.1109/SMC58881.2025.11343553)

## Citation (BibTeX)

```bibtex
@inproceedings{herrmann2025tiago,
  author    = {Malte Herrmann and Thorsten Hempel and Ayoub Al-Hamadi},
  title     = {A Real-Time Digital Twin Framework for the {TIAGo} Service Robot},
  booktitle = {2025 IEEE International Conference on Systems, Man, and Cybernetics (SMC)},
  pages     = {6065--6068},
  year      = {2025},
  doi       = {10.1109/SMC58881.2025.11343553}
}
```