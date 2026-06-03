---
title: "A Real-Time Digital Twin Framework for the TIAGo Service Robot"
source: "https://ieeexplore.ieee.org/document/11343553"
author:
  - "[[Malte Herrmann]]"
  - "[[Thorsten Hempel]]"
  - "[[Ayoub Al-Hamadi]]"
conference: "**IEEE SMC 2025** [[paper]](https://doi.org/10.1109/SMC58881.2025.11343553)"
description: "The rapid development of humanoid robots to perform human-like tasks has introduced new possibilities in automation. In the context of advanced autonomous robot"
tags: [IEEE SMC 2025, Conference, Digital Twin]
---
## Abstract:

The rapid development of humanoid robots to perform human-like tasks has introduced new possibilities in automation. In the context of advanced autonomous robotic operati...

---

Service robots are attracting significant attention across various industries due to their potential to enhance efficiency, autonomy, and reliability in task execution. However, developing and testing robotic operation methods can be costly, requiring significant energy, physical space, continuous monitoring, and ongoing maintenance \[1\]. In addition, path or trajectory planning can fail, and the robot has a risk of damaging itself or the environment. To compensate for these drawbacks and to address these planning challenges, it is crucial to have a digital representation of the robot to simulate such motions in advance. In addition to motion monitoring, a virtual copy offers many other possibilities, such as an ecosystem for reinforcement learning (RL) or providing a platform for students to safely learn the robot context. For this reason, this paper introduces a Digital Twin Instance (DTI) \[2\] of the TIAGo service robot by PAL Robotics \[3\]. A side-by-side comparison between the real robot and the digital twin is shown in Fig. 1.

A Digital Twin Instances (DTI) facilitate the safe and cost-effective simulation of robotic behavior in dynamic or challenging environments, such as factories or hospitals, where modifications are difficult or impractical. Rather than undergoing physical adaptation to real-world infrastructure, which is often a time-consuming and expensive process, robotic programming and AI training can be transferred to virtual environments. For RL, training conditions can be set up quickly by randomizing the placement of objects. Even dynamic settings can be reconstructed. This facilitates development because, in a real-world scenario, the robot’s unpredictable behavior can disrupt the human workflow. Another advantage is scalability, which allows multiple instances of the simulation to run in parallel. The training process, including collecting data, applying rewards to the algorithms, and resetting the environment to its default settings, can be easily configured. By integrating the remaining sensors, such as LiDAR and sonar, the sim-to-real gap can be reduced and later overcome by transfer learning or domain randomization methods \[4\].

**Fig. 1.**

**Top left**: TIAGo and his digital Twin performing a heart emote; **Bottom right**: Both performing the *wave* motion simultaneously

This paper introduces a digital twin operating on three different levels. The initial level is designated as **Real2Sim**, wherein the DTI replicates movement and displays data for the purpose of supervision. Subsequently, the level referred to as **Sim2Real** is employed for the purpose of robot control through the utilization of isomorphic teleoperation and the formulation of paths and motions. The final level is entitled **Sim Only**, in which all sensors and actors operate exclusively within the simulation. This feature makes it ideal for training neural networks using RL methods.

The concept of digital twins (DT) encloses at least two definitions. As presented in \[2\], a DT prototype (DTP) is defined as an entity encompassing all the requisite information to fabricate a specific product, including its 3D model and a comprehensive bill of materials. The DTI (digital twin instance) is a comprehensive representation of a specific physical product throughout its entire life cycle, including detailed condition and process data. This paper focuses on the utilization of the DTI for such purposes.

In recent years, the use of digital twins has increased significantly, not only in robot context but also in simulation of automated vehicles for autonomous driving \[5\] or the utilization of RL with a robot arm \[6\]. This development is primarily driven by the inherent advantages of cost-effectiveness, accessibility and resilience \[7\]. The Robot Operating System (ROS) \[8\] is a common tool in robotics that allows communication between individual hardware and software components. ROS uses a server-client architecture. The central component, called the *ROS core*, manages the communication infrastructure by handling ROS topics and routing messages between publishing and subscribing nodes.

Many solutions are available to assist integrating robots into the digital world. The two most frequently utilized tools are Unity \[9\] and Gazebo \[10\], which are compared in \[11\]. Initially developed as a gaming platform, Unity has evolved to offer a wide range of features that are suitable to the robotic domain. These include plug-ins for ROS integration and GameObjects that behave like real robot joints. Conversely, Gazebo is an open-source software that was developed specifically for simulating robots. The system’s intuitive design facilitates rapid skill acquisition, making it an optimal choice for novice users.

In \[12\], the TIAGo service robot was integrated into the digital world, with Unity serving as the simulation engine. It is imperative to acknowledge that the DH parameter of the 7 DOF robot arm were used to calculate the forward kinematics. Furthermore, due to the redundancy of the joints, an analysis of the inverse kinematics was performed using a particle swarm algorithm to identify an optimal solution. It is important to note that this implementation may result in discrepancies between the two instances due to the utilization of distinct algorithms. Hence, this paper addresses the aforementioned flaw by utilizing and enhancing the original software (further referred as TIAGo-OS) for both the real robot and the DT.

To facilitate the transition of components related to the robot into the digital space, a software counterpart is introduced for each hardware component. The real-world elements, such as the robot hardware, additional PCs, and software implementation (e.g., Unity and a virtual machine), are illustrated in Fig. 2, where matching colors represent the relationship between real and simulated components.

**Fig. 2.**

Real-world components (left) and the corresponding simulation components (right) of the TIAGo system.

The real-world scenario, shown on the left, involves the actual robot hardware. The original TIAGo-OS runs on the integrated PC alongside the ROS-Core. For the context of the simulation, these components are transferred to a virtual machine (VM) on the Host-PC. Due to the absence of essential components in the VM that ensure uninterrupted, error-free functionality, certain ROS nodes were disabled or satisfied by code running within the simulation engine.

The physical joints and sensors are implemented in Unity due to its advantages in high-fidelity graphics (to minimize the sim-to-real gap), high accuracy, low latency, and scalability (to accelerate RL training) \[11\]. Additionally, Unity facilitates the development of intuitive graphical user interfaces (GUIs). Unity Technologies offers useful tools that simplify the implementation process. The *URDF-Importer* <sup>1</sup> transfers the robot model, which contains the robot’s links, joints, and materials, to Unity, while the *ROS-TCP-Connector* <sup>2</sup> establishes a ROS connection from within Unity. This network interface allows Unity to participate in the communication process.

The NVIDIA Jetson \[13\], which serves as an energy-efficient platform for AI-supported calculations in embedded systems, is installed for the training and execution of AI applications (referred to as ML ROS Packages) that have been developed independently. Since these applications also run on the Host-PC and require only the Python programming language and ROS, the Jetson is no longer needed in simulation mode. The development PC, referenced in Fig. 2, can be omitted, as it is exclusively utilized for tutorials and robot configuration.

At the **Real2Sim** level, Unity subscribes to certain ROS nodes to receive information on joint angles and apply them to the model, mirroring real movement. Motion planning will be possible with the **Sim2Real** mode. The model can be manipulated inside the simulation to define a robot configuration, which is then published to the TIAGo-OS via ROS. The calculated joint angles are also mirrored afterwards. Since this mode utilizes the existing robot’s functionalities, no implementation is necessary. In **Sim Only** mode, the original software is used for calculations and runs inside the VM. Communication occurs with a different ROS core located there.

**TABLE I** Current development state of actor and sensor integration, as well as manufacturer and in-house ROS modules

While the implementation involves numerous and different components, the most essential ones, such as joints and a camera sensor, have already been successfully completed, laying a strong foundation for what follows. Moreover, Tab. I provides a summary of the by now implemented elements, those already in progress and those planned for the future.

### A. Actuators

Following the importation of the robot model, which contains information regarding the physical motors, a subscription for the corresponding ROS topic has been established. Afterwards, a code snippet extracts relevant data from the message. The joint angles are then applied to the articulation bodies, as they are designated in Unity, which move to their defined positions autonomously. As illustrated in Fig. 1 (bottom right) the robot and the DT, both performing the *wave* motion simultaneously as the movement is only calculated in the real robot.

**Fig. 3.**

Virtual test environment of the digital twin procedural generated with *Infinigen Indoors* \[18\] and the graphical user interface. **Top left**: Activation of ML nodes; **Top side**: Switching between real world and simulation; **Top right**: Status of the nodes; **Right side**: Buttons for predefined motions; **Bottom right**: Actual recorded camera image; **Bottom left**: Processed and returned image of the active node.

### B. Sensors

The RGB camera is implemented to record the scene in Unity and subsequently publish the data to the local ROS network. This allows the ML ROS packages to process the images and return images that can be displayed in the GUI (see bottom of Fig. 3). Conversely, the ability to show the perspective of a real robot is already possible. By subscribing to the corresponding image ROS topic, the image can be displayed analogous to the virtual one. Currently, the images captured by the depth camera and the data obtained from the LiDAR sensor are only displayed in Real2Sim mode due to the ongoing development stage.

### C. Functionality

Teleoperating the robot via a gamepad has already been implemented. The inputs are processed by Unity and assembled into a ROS topic, which is then published to the network. Additionally, a motion library containing predefined robot configurations has been integrated. It can be accessed by pressing a button on the Unity GUI, which publishes the corresponding action to the TIAGo-OS.

The GUI is designed to be both modular and intuitive, as illustrated in Fig. 3. The Layout enables the user to transition between operating modes, activate and deactivate specific ROS nodes, oversee the status of these nodes, observe the sensor data, and monitor incoming ROS topics. The TIAGo-OS and the ROS core are both booted via a startup procedure. In parallel, the ROS-TCP-Endpoint is started. Next, the missing nodes of the original software’s ROS topics are imitated from within Unity. These components can continue to run when switching to Real2Sim mode because the ROS-TCP-Connector connects to a different ROS core. This enables seamless transition between modes.

As a proof of concept, the simulation includes an indoor environment procedurally generated by the *Infinigen Indoors* \[18\] generation framework. Fig. 3 illustrates parts of the interior design and functionality of the ML ROS Packages. This implementation will validate the development of the remaining sensors and test the SLAM algorithm running in the TIAGo-OS.

This paper proposes a digital twin of the TIAGo service robot in Unity, including authentic kinematic calculations and an intuitive GUI to facilitate the application of reinforcement learning. Primary functionalities, such as joint mirroring and camera recording, have already been successfully implemented. A crucial element of the DT is the virtual machine operating the original software. This ensures that the path and trajectory planning is identical to the calculations that would have been made directly on the robot. Moreover, the integration of the ML ROS packages and associated functionalities enables the simulation to behave like a real robot. This facilitates the integration process and subsequent algorithm application. The node status clearly monitors the system’s state. Furthermore, it has been demonstrated that external settings can be integrated into Unity. The generation framework will be used to create different procedural training environments. The further development of a fully functional digital twin will maximize the potential of our laboratory.

The next phase of development will focus on integrating the remaining sensor systems, such as LiDAR, sonar, and the inertial measurement unit (IMU). The depth camera will use Unity shaders to read the engine’s z-buffer and create a depth image.

A ghost model of the robot will be employed to facilitate motion planning and isomorphic teleoperation. This model can be maneuvered within the simulation environment. Then, the motion planning algorithm calculates the desired trajectory to reach the specified destination.

In order to use reinforcement learning, components specific to Unity, such as colliders, rigid bodies, and lighting, must be integrated into the environment. This integration is essential for detecting and rewarding collisions with objects and leaving the workspace.

To complete this implementation and ascertain the laboratory’s current status, the remaining ML packages of the institute will first be implemented. Secondly, the integration of additional robots in the laboratory is under consideration. Due to the system’s modular configuration, achieving this goal will be highly efficient and easy. Finally, the physical structure, including desks, chairs, and other furnishings, will be digitized. This process is facilitated by \[19\].