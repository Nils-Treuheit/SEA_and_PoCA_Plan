---
title: "Physical Human-Robot Interaction for Grasping in Augmented Reality via Rigid-Soft Robot Synergy"
source: "https://ieeexplore.ieee.org/abstract/document/11522919"
author:
published:
created: 2026-06-03
description: "Hybrid rigid-soft robots combine the precision of rigid manipulators with the compliance and adaptability of soft arms, offering a promising approach for versat"
tags:
  - "clippings"
---
## Abstract:

Hybrid rigid-soft robots combine the precision of rigid manipulators with the compliance and adaptability of soft arms, offering a promising approach for versatile graspi...

---

Teleoperation has long been a standard paradigm for robot control, particularly in industrial and medical applications. Conventional articulated robots are typically teleoperated through teaching pendants, joysticks, master-slave systems, or graphical user interfaces \[1\], \[2\]. Beyond these standard interfaces, recent advances have sought to enhance intuitiveness and immersion by multi-modality feedback, such as haptic feedback on wearable devices \[3\], \[4\], augmented reality (AR)-based visual overlays on robot arms \[5\] or aerial robots \[6\], and more intuitive body-machine interfaces \[7\].

However, soft continuum robots continue to pose significant challenges for teleoperation, and achieving reliable control remains an open research problem. Their high degrees of freedom (DoFs) and lack of analytical models complicate the mapping between human input and robot motion. Most existing efforts require labor-intensive manual tuning and rely on prototype-specific actuation strategies that limit generalizability. More advanced paradigms such as shared autonomy, user intention inference, or immersive extended reality-based teleoperation remain underexplored for soft robots due to difficulties in real-time modeling, sensing, and control.

**Fig. 1.**

Schematic diagram of the proposed AR interaction method.

Teleoperation of continuum arms can be generally categorized into end-effector control and shape control. Endeffector control approaches allow users to specify a desired trajectory for the robot tip, for instance, through a digital pen interface \[8\]. Gesture-based methods have also been developed to map human motions to robot kinematics, enabling operations such as growth, retraction, steering, and rotation in growing robots \[9\], or to execute constrained motions in two-segment continuum robots \[10\]. Shape control strategies are less common: some studies use shape correspondence to align the robot’s deformation with user-drawn curves for obstacle avoidance \[8\], while others employ physical twin demonstrators to transfer desired configurations \[11\].

Despite progress, relatively few studies have attempted to bridge hybrid rigid-soft structures with human-in-the-loop teleoperation. Prior work has typically treated the rigid and soft subsystems separately, controlling the rigid arm through kinesthetic teaching while actuating the soft arm using predefined motion patterns \[12\], \[13\]. As such, the potential of hybrid teleoperation, which combines the precision of rigid mechanisms with the adaptability of soft structures, remains underexplored.

In this paper, we present a novel physical human-robot interaction (HRI) method to teleoperate a hybrid rigid-soft robotic system via AR (Fig. 1). Our hybrid system comprises a tendon-driven soft continuum arm mounted on a rigid robot arm, enabling high-DoF whole-arm grasping capabilities. We refer readers to our previous work in \[12\] for more details about the mechanical structure of our robotic system. Our HRI platform leverages the strengths of both subsystems, provides an immersive and straightforward teleoperation interface, and incorporates a virtual preview mechanism that allows users to evaluate and refine commands before execution on the physical robot. Through this approach, we demonstrate how AR can enable effective human-in-the-loop control of hybrid robots, thereby enabling safer and more versatile grasping in real-world environments.

To accurately superimpose the virtual robot onto the real one, a high-fidelity simulation is required to capture the robot’s static and dynamic behavior as well as its control system response. A common method to establish this reality-to-simulation (real-to-sim) transfer is via model formulation and parameter identification. Parameter identification is a well-established topic in rigid robotics \[14\], \[15\], frequently employed to determine quantities that are difficult to measure directly. However, parameter identification for soft robots remains underexplored due to several inherent challenges:

1. Soft robots possess a significantly larger parameter space than rigid robots.
2. Soft robots lack inherent proprioception, requiring a more complex sensing mechanism to capture their deformation accurately.
3. Soft robot modeling is computationally intensive, limiting real-time usability.

Analytical Lagrangian dynamics are built for relatively simple soft robot designs such as soft fingers \[16\] or soft grippers \[17\]. More complicated hydrodynamics have also been taken into account for an aquatic soft robot \[18\] for locomotion. Other studies adopt data-driven methods to identify both the dynamics and the controller parameters \[19\], \[20\].

Instead of deriving analytical models, we employ the physics-based simulation engine MuJoCo \[21\] to model the robot and identify simulation-oriented parameters using motion capture data and a numerical optimization algorithm. Our main contributions are summarized as follows:

1. We develop a novel physical human-robot interaction framework for a hybrid rigid-soft robot, enabling the user to specify end-effector positions for both the rigid and soft components within the shared workspace via an AR headset and controllers.
2. We establish a simulation-centered parameter identification framework for a cable-driven soft continuum arm, enabling the estimation of dynamic and control parameters within a general-purpose physics engine.
3. We validate the proposed interaction through a series of experiments demonstrating the system’s effectiveness for grasping.

In this section, we identify the selected methods to develop the HRI framework. Section II-A presents the inverse kinematics approach and the reachability maps for the hybrid system. Section II-B describes the modeling of our soft arm in MuJoCo. Section II-C describes the parameter identification procedures for our soft robot. Section II-D presents the methodology and practical applications of our HRI framework.

### A. Inverse Kinematics and Reachability Maps

#### 1) Rigid Robot:

We compute the inverse kinematics (IK) of the rigid robot numerically using an iterative solver to handle the redundancy of its seven degrees of freedom \[22\].

Next, we generate a reachability map as a user input indicator for the following HRI interface. A dense sampling of feasible end-effector poses is performed via forward kinematics (FK), with the end-effector orientation constrained to point downward. While this restricted sampling does not capture the robot’s full orientation dexterity, it aligns with practical AR teleoperation scenarios in which the endeffector predominantly points downward. The corresponding point cloud is enclosed to form a map as shown in Fig. 2(a).

#### 2) Soft Robot:

The spiral-shaped soft continuum arm poses a greater challenge for IK computation, which is essential for mapping the user’s desired target input to the actuation value. To achieve a balance between accuracy and real-time deployability, we employ a lightweight neural network that directly learns the mapping from end-effector poses to tendon actuation lengths. The model is applied directly to both the simulated and physical robots.

Our model is a multilayer perceptron. The input to our model is an eight-dimensional vector comprising the gravity direction $\theta_{G}$, end-effector position $(x, y, z)$, and orientation represented as a unit quaternion $\left(q_{w}, q_{x}, q_{y}, q_{z}\right)$. Our model outputs the corresponding tendon lengths $\left(l_{1}, l_{2}, l_{3}\right)$ with three hidden layers, ReLU activations, and the Adam optimizer.

We generate the training dataset from analytical FK simulations across multiple gravity directions using the model of \[12\], which consists of approximately 120,000 samples uniformly distributed throughout the reachable workspace. We split the data into training and validation sets at a 80/20 ratio. We export the trained network in ONNX format for integration into the AR-based control framework.

This architecture is sufficiently expressive to approximate the nonlinear relationship between end-effector pose and actuation while remaining compact enough for real-time inference on embedded hardware, enabling a low-latency IK controller that generalizes across the entire operating workspace.

We construct the reachability map for our soft arm by sampling forward kinematics (FK) outputs and enclosing the resulting point cloud within a 3D reachable surface, as shown in Fig. 2(b). Since the effective reachability of our soft arm varies with gravity direction, which depends on the orientation of the rigid end-effector, we sample the FK outputs across multiple gravity directions-as shown in Fig. 2(c) -to capture this dependence. We then update the reachability map in real time according to the current gravity direction. Fig. 2(d) shows the restricted reachability map of our hybrid robotic system.

[$Fig. 2. - 
                (a) Restricted workspace of the rigid robot. (b) Reachability map of the spiral-shaped soft arm with its backbone aligned with the gravity direction. (c) Reachability maps of our soft arm at $0^{\circ}, 60^{\circ}$, and 120° relative to the gravity direction. (d) Visualization of the restricted reachability map overlaid on the AR robot model.$](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11522815/11522816/11522919/huang2-robosoft26-298-large.gif)

**Fig. 2.**

(a) Restricted workspace of the rigid robot. (b) Reachability map of the spiral-shaped soft arm with its backbone aligned with the gravity direction. (c) Reachability maps of our soft arm at $0^{\circ}, 60^{\circ}$, and 120° relative to the gravity direction. (d) Visualization of the restricted reachability map overlaid on the AR robot model.

### B. Modeling as Pseudo-Rigid-Body

We develop a simulation model of our cable-driven continuum arm within the general-purpose physics engine MuJoCo. MuJoCo employs a pseudo-rigid-body model to represent our soft arm by discretizing it into multiple concatenated rigid segments, where each inter-segment connection is modeled as a torsional spring-damper system with two rotational degrees of freedom.

To understand which parameters need to be identified, we analyze the passive constitutive law for the pseudo-rigidbody model as follows. The total torque acting on each joint consists of the active driving torque (from the tendons) and the passive internal torque. [Equation 1](#deqn1) specifically models the latter one, the viscoelastic resistance of the elastic joint, as a function of joint deformation and velocity. This internal torque acts as a counteracting force during cable-driven maneuvers. The mass and inertia of the joint are negligible compared to the link.

$$
\begin{equation*}\tau=-K_{\text {bending }} \cdot\left(\theta-\theta_{0}\right)-D \cdot \dot{\theta} \tag{1}\end{equation*}
$$
 View Source

Here, the torque $\tau$ applied to each joint, characterized by bending stiffness $K_{\text {bending }}$ and damping coefficient *D*, drives the initial joint rotation $\theta_{0}$ toward the current rotation $\theta$ with angular velocity $\dot{\theta}$. The spring term generates a restoring torque that returns the joint to its equilibrium position, while the damping term suppresses rapid oscillations.

According to Euler-Bernoulli beam theory, the bending stiffness $K_{\text {bending }}$ can be expressed as [Equation 2](#deqn2):

$$
\begin{equation*}K_{\text {bending }}=\frac{E I}{L} \tag{2}\end{equation*}
$$
 View Source

For a circular cross section with radius *r*, the area moment of inertia scales as $I \propto r^{4}$. The Young’s modulus *E* is constant.

Since the robot geometry follows a logarithmic spiral constraint \[23\], the segment dimensions exhibit a fixed scaling ratio $\alpha$ along the backbone. This property allows the stiffness distribution to be initialized, where the stiffness $K_{\text {bend }, i}$ in segment *i* is scaled along the arm according to [Equation 3](#deqn3):

$$
\begin{equation*}K_{\text {bend }, i}=K_{0} \cdot \alpha^{(3 *(i-1))} \tag{3}\end{equation*}
$$
 View Source

where $K_{0}$ is the stiffness of the base segment. Consequently, the bending stiffness scales as $K_{\text {bending }} \propto \alpha^{3}$.

The damping ratio $\zeta$ is a dimensionless parameter that characterizes the rate of energy dissipation in an oscillating system, as given by [Equation 4](#deqn4):

$$
\begin{equation*}\zeta=\frac{D}{2 \sqrt{K_{\text {bending }} \cdot m}} \tag{4}\end{equation*}
$$
 View Source

Here, *m* denotes the mass of the segment. To initialize the damping parameters, each joint is approximated as a secondorder vibrational system. When modeling our soft arm, we aim to maintain consistent vibration response behavior across all sections, which requires keeping the damping ratio $\zeta$ constant. Consequently, the damping coefficient scales as $D \propto \zeta \cdot 2 \sqrt{K_{\text {bending }} \cdot m}=\alpha^{3}$.

### C. Parameter Identification

To accurately model the dynamic behavior and control system response of the soft continuum robot, we implement a staged parameter identification pipeline. The identification process is divided into three stages: (1) stiffness, (2) damping and friction, and (3) control gains and actuator force range. Each stage focuses on a specific subset of parameters, optimized using experimental data obtained from motioncapture trajectories recorded under varying conditions. The stages are performed sequentially, with each step building upon the previous one to ensure consistent dynamic behavior and control system response across the complete parameter space. We design a series of experiments for each stage (summarized in Table I), targeting static properties, dynamic behaviors, and the controller parameters. Our experiment setup is shown in Fig. 3.

#### 1) Initialization:

Geometric parameters such as mass and inertia are initialized based on the material properties and robot geometry, while dynamic and control system parameters are estimated empirically.

Although the mass and inertia matrices can be directly derived from the robot’s CAD model, real-world imperfections, such as non-uniform tendon routing, asymmetric 3D printing, and assembly inconsistencies, lead to discrepancies between theoretical and actual values. Since the primary objective is to capture the qualitative dynamic behavior of the continuum arm, mass and inertia are not explicitly reidentified. Instead, their effects are implicitly accounted for during the identification of bending stiffness and damping parameters.

[![Fig. 3. - 
                (a). Experimental setup of our soft arm and the motion capture system with six infrared cameras. (b). Static tilting experiment to identify bending stiffness for each section, corresponds to Table I Exp.1. (c) Curling and uncurling in different actuation statuses to identify damping properties for the robot and the actuator properties for the control system, corresponds to Table I Exp. 2 and 3.
              ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11522815/11522816/11522919/huang3-robosoft26-298-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11522815/11522816/11522919/huang3-robosoft26-298-large.gif)

**Fig. 3.**

(a). Experimental setup of our soft arm and the motion capture system with six infrared cameras. (b). Static tilting experiment to identify bending stiffness for each section, corresponds to Table I Exp.1. (c) Curling and uncurling in different actuation statuses to identify damping properties for the robot and the actuator properties for the control system, corresponds to Table I Exp. 2 and 3.

**Table I** Parameter Identification Experiments For Our Soft Arm

#### 2) Stiffness Identification:

The first optimization stage focuses on the identification of joint stiffness coefficients $K_{\text {bending }}$. Simulations are conducted by tilting the robot to a fixed displacement and allowing it to relax under gravity, while recording the pose of body centroids using a motioncapture system. A Butterworth low-pass filter is subsequently applied to the recorded data to remove measurement noise. We minimize the following position-based loss function:

$$
\begin{equation*}\mathscr{L}=\frac{1}{N-1} \sum_{i=2}^{N} \operatorname{Huber}\left(\frac{d_{i}^{\text {sim }}-d_{i}^{\text {real }}}{\left|d_{i}^{\text {real }}\right|+\varepsilon}, \delta_{\text {pos }}\right) \tag{5}\end{equation*}
$$
 View Source

Here, $d_{i}^{\text {sim }}$ and $d_{i}^{\text {real }}$ denote the Euclidean distances between the base segment and segment *i* in the simulated and measured configurations, respectively. The loss compares the pairwise distances across all actuation sequences, focusing on the internal shape consistency of the robot rather than absolute coordinates. *N* is the number of robot segments, $\varepsilon$ is a small constant for numerical stability, and $\delta_{\text {pos }}$ is the Huber threshold controlling the transition between quadratic and linear penalization. The Huber loss ensures robustness to outliers, penalizing small residuals quadratically and large residuals linearly, thereby emphasizing accurate reproduction of the robot’s physical deformation.

The stiffness parameters are optimized in two stages: an initial coarse global identification optimized the stiffness of the first joint and scaled down for the following joints towards the tip, followed by a fine local adjustment of individual joint parameters within $\pm 10 {\%}$ based on the coarse solution.

The optimization is performed using Differential Evolution (DE), a population-based, derivative-free global optimization algorithm. In DE, new candidate solutions are generated by combining scaled differences of randomly selected population vectors, followed by crossover and selection based on the objective value. For each candidate parameter set, a forward simulation is executed in MuJoCo and evaluated using the defined loss function.

#### 3) Damping:

Damping coefficients $D_{i}$ are estimated using free-uncurling experiments, in which the robot is released from an actuated configuration and allowed to return to its neutral shape. To ensure that oscillatory behavior is accurately captured, a weighted loss $\mathscr{L}_{\text {sum }, \mathrm{t}}$ is employed, combining the positional loss $\mathscr{L}_{\text {pos, } \mathrm{t}}$ in [Equation 6](#deqn6) and the velocity loss $\mathscr{L}_{\text {vel,t }}$ in [Equation 7](#deqn7). The second term penalizes discrepancies in the dominant oscillations and decay behavior, thereby improving the identification of viscoelastic effects that are only weakly observable through a purely geometric objective.

$$
\begin{equation*}\mathscr{L}_{\mathrm{pos}, t}=\frac{1}{N-1} \sum_{i=2}^{N} \operatorname{Huber}\left(\frac{d_{i}^{\mathrm{sim}}(t)-d_{i}^{\mathrm{real}}(t)}{\left|d_{i}^{\mathrm{real}}(t)\right|+\varepsilon}, \delta_{\mathrm{pos}}\right), \tag{6}\end{equation*}
$$
 View Source

$$
\begin{equation*}\mathscr{L}_{\mathrm{vel}, t}=\frac{1}{N-1} \sum_{i=2}^{N} \operatorname{Huber}\left(\frac{\Delta d_{i}^{\mathrm{sim}}(t)-\Delta d_{i}^{\mathrm{real}}(t)}{\left|\Delta d_{i}^{\mathrm{real}}(t)\right|+\varepsilon}, \delta_{\mathrm{vel}}\right) \tag{7}\end{equation*}
$$
 View Source

$$
\begin{equation*}\mathscr{L}_{\mathrm{sum}, t}=w_{\mathrm{pos}} \mathscr{L}_{\mathrm{pos}, t}+w_{\mathrm{vel}} \mathscr{L}_{\mathrm{vel}, t} \tag{8}\end{equation*}
$$
 View Source

Here, $d_{i}^{\text {sim }}(t)$ and $d_{i}^{\text {real }}(t)$ denote the Euclidean distances between the base segment and segment *i* at time step *t* in the simulated and measured trajectories, respectively. $\Delta d_{i}(t)$ represents the temporal difference of these distances, capturing segment-wise velocity behavior. The time index *t* spans all frames within a sequence, $t \in\left[1, T_{s}\right]$, with $\mathscr{L}_{\text {vel }, 1}=0$. $\varepsilon$ is a small constant for numerical stability, while $\delta_{\text {pos }}$ and $\delta_{\text {vel }}$ are Huber thresholds. At each time step, the position and velocity terms are weighted by $w_{\text {pos }}$ and $w_{\text {vel }}$ to form $\mathscr{L}_{\text {sum }, t}$ in [Equation 8](#deqn8). These timestep losses are then averaged over each sequence and subsequently across all actuation trials to obtain the final loss $\mathscr{L}_{\text {sum }}$.

Coarse damping factors are first optimized jointly across all segments, followed by fine-tuning of individual segment parameters within $\pm 10 {\%}$ of the coarse solution.

#### 4) Control System Parameters:

The control behavior of the system is simplified and implicitly modeled as a position actuator governed by a proportional-derivative (PD) controller. After the stiffness and damping parameters are determined, the control-related parameters, namely the proportional gain $k_{p}$, velocity gain $k_{v}$, force range $F_{\text {range }}$, and actuator time constant $\tau_{m}$, are optimized. Assuming critical damping, the relationship between the proportional and derivative gains is initialized as:

$$
\begin{equation*}k_{v}=2 \sqrt{k_{p} m} \tag{9}\end{equation*}
$$
 View Source

The same trajectory-based loss function, $\mathscr{L}_{\text {sum }, \mathrm{t}}$, is applied across all time frames to evaluate the overall shape convergence throughout each motion.

### D. Physical Human Robot Interaction

In the proposed framework, the user operates two ARtracked joysticks equipped with virtual laser pointers. As illustrated in Fig. 1, the endpoint of one pointer specifies the desired position of the rigid arm end-effector, while the other defines the target location of the soft arm tip. The laser pointers are retractable, enabling intuitive 3D spatial control.

Before executing the motion on the physical robot, a virtual replica of the robot performs the commanded action in simulation, including the grasping attempt. This preview mechanism allows the user to assess whether the command aligns with the intended goal and to make adjustments through iterative trials in the virtual environment. Once the user confirms the command, the motion is executed on the physical robot, reducing trial-and-error during real hardware interaction and enhancing efficiency, safety, and user confidence.

The trunk-like robot is constrained to bending motions only. This enables a unique shape mapping under single-DoF actuation, which is sufficient for fundamental interaction and grasping tasks. Specifically, planar bending can be achieved through either single-cable dorsal actuation or symmetric dual-cable ventral actuation, resulting in a smooth spiral configuration that has been demonstrated to successfully grasp hanging objects \[12\]. When the user specifies a position outside these planes, the rigid arm end-effector automatically rotates to reorient the soft arm so that its bending plane aligns with the user-defined direction in the virtual space.

[![Fig. 4. - 
              Additional details are provided in the supplementary video. (a) Adjustment of the ray length projected from the joystick; (b) Tilting the joystick to change the orientation of the rigid robot for alignment with the joystick. (c) Control of the soft robot’s curling and uncurling motions by specifying the end-effector position using the ray. (d) Control of the rigid robot’s end-effector position using the ray. (e) Overview of the joystick functions.
            ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11522815/11522816/11522919/huang4-robosoft26-298-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11522815/11522816/11522919/huang4-robosoft26-298-large.gif)

**Fig. 4.**

Additional details are provided in the supplementary video. (a) Adjustment of the ray length projected from the joystick; (b) Tilting the joystick to change the orientation of the rigid robot for alignment with the joystick. (c) Control of the soft robot’s curling and uncurling motions by specifying the end-effector position using the ray. (d) Control of the rigid robot’s end-effector position using the ray. (e) Overview of the joystick functions.

We first present the hardware and experimental setup (Section III-A), then demonstrate the performance of the real-to-sim parameter identification (Section III-B), and finally describe the grasping experiments (Section III-C).

### A. Hardware and Experimental Setup

The soft robot used in this experiment consists of a three-cable-driven, spiral-based soft arm connected to a tiltable base mount at one end and an end-effector gripper at the other. The physical prototype is fabricated via 3D printing using thermoplastic polyurethane (TPU-95A) with a 100% infill rate to minimize structural inconsistencies. During data collection and user experiments, cable lengths are actuated by three independent motors (DJI GM6020 brushless DC motors). An OptiTrack motion capture system with six cameras (Fig. 3) tracks the real-time poses of 18 key segments at 120 Hz, enabling reliable reconstruction of the arm’s 3D backbone trajectory. This setup is used to conduct the parameter identification experiments summarized in Table I.

We use the optimized parameters obtained in Section IIC to calibrate the soft robot model in MuJoCo 3.3.1. The calibrated MuJoCo XML model is subsequently imported into Unity 6 using the official MuJoCo-for-Unity plugin, which enables the MuJoCo physics engine to run natively within Unity. In this setup, MuJoCo remains responsible for all physics simulation, while Unity is used exclusively for rendering, AR visualization, and user interaction. The system is deployed on a Meta Quest 3 operating at 60 Hz.

### B. Parameter Identification

The qualitative results of our experiments in Table I are shown in Fig. 5 (a)-(c). The quantitative results are presented in Fig. 5 (d) and (e), where a Butterworth low-pass filter is applied for improved interpretability. We define the internal shape error $E_{\text {internal }}$ as:

$$
\begin{align*}E_{\text {internal }}=\frac{1}{(N-1) T} \sum_{t=1}^{T} \sum_{i=2}^{N} & \left\|\mathbf{P}_{1}^{\text {sim }}(t)-\mathbf{P}_{i}^{\text {sim }}(t)\right\| \\ & -\left\|\mathbf{P}_{1}^{\text {real }}(t)-\mathbf{P}_{i}^{\text {real }}(t)\right\| \tag{10}\end{align*}
$$
 View Source

where *N* is the number of segments in the robot, *T* is number of frames in the dynamic process. $p_{i}^{\text {sim }}(t)$ is the 3D position of segment *i* in simulation at frame $t. p_{i}^{\text {real }}(t)$ refers to 3D position of segment *i* in real-world data at frame *t*.

The initial model before optimization exhibits an internal error of 8.98 cm over 437 frames, whereas the optimized model after parameter identification achieves an error of $1.58 c m$ over the same number of frames. This corresponds to a reduction from 17% to 3% when normalized by the total length of the soft robot (50.7 cm). The fine-tuning process improves model performance by less than 2%, thereby validating the assumptions derived from the robot’s geometric scaling.

### C. Grasping via Augmented Reality Platform

We conduct a series of experiments to demonstrate the teleoperation capabilities of our AR framework. A detailed explanation of each experiment is summarized in Table II. We also show a series of keyframes of our experiments in Fig. 6. We refer readers to our supplementary video to view each experiment.

### A. Parameter Identification

The proposed simulation-centered parameter identification framework effectively aligns simulated and real robot behavior by jointly estimating static, dynamic, and controller parameters. It significantly reduces internal shape error and enables stable teleoperation within the AR framework, providing a practical and reproducible real-to-sim calibration pipeline without requiring analytical continuum models.

However, the simulated controller is a simplified abstraction of the physical system, and unmodeled effects may be implicitly absorbed into the optimized parameters. As a result, the identified parameters are behaviorally consistent but For applications demanding higher sim-to-real fidelity, such as reinforcement learning, trajectory optimization, or contact-rich manipulation, further sim-to-real adaptation and model refinement beyond the initial real-to-sim identification will be necessary.

[![Fig. 5. - 
              (a) Uncurling motion of the physical robot with motion-capture markers illustrating the trajectory over time. (b) Simulated uncurling motion after parameter identification. (c) Simulated uncurling motion using initial, unoptimized parameters. (d) Positional error between the optimised and unoptimized models. (e) Velocity error between the optimized and unoptimized models. not strictly physically interpretable, limiting their direct use in analytical model-based control approaches such as model predictive control or observer-based methods. Furthermore, the current evaluation focuses primarily on internal error metrics and qualitative demonstrations, and future work should incorporate broader quantitative benchmarks and higherfidelity physics simulations to improve generalizability.
            ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11522815/11522816/11522919/huang5-robosoft26-298-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11522815/11522816/11522919/huang5-robosoft26-298-large.gif)

**Fig. 5.**

(a) Uncurling motion of the physical robot with motion-capture markers illustrating the trajectory over time. (b) Simulated uncurling motion after parameter identification. (c) Simulated uncurling motion using initial, unoptimized parameters. (d) Positional error between the optimised and unoptimized models. (e) Velocity error between the optimized and unoptimized models. not strictly physically interpretable, limiting their direct use in analytical model-based control approaches such as model predictive control or observer-based methods. Furthermore, the current evaluation focuses primarily on internal error metrics and qualitative demonstrations, and future work should incorporate broader quantitative benchmarks and higherfidelity physics simulations to improve generalizability.

### B. AR-based Human Robot Interaction

The proposed AR-based HRI framework introduces a novel target specification and a real-time teleoperation concept for hybrid rigid-soft robots, demonstrating feasibility across trajectory-following, reaching, and grasping tasks. The integration of simulation, AR visualization, and physical execution establishes a coherent interaction pipeline suitable for exploratory manipulation scenarios.

However, a comprehensive quantitative assessment incorporating additional task scenarios, a larger user cohort, and standardized performance metrics is required to rigorously evaluate usability, intuitiveness, and cognitive workload.

In this work, we presented a simulation-centered parameter identification framework for a cable-driven soft continuum arm, enabling the estimation of static, dynamic, and controller parameters in MuJoCo. Building on this calibrated simulation, we developed a novel augmented-reality-based human-robot interaction framework for a hybrid rigid-soft robot that enables users to define end-effector targets via a head-mounted display. Our qualitative evaluation demonstrates the feasibility and practical functionality of the integrated real-to-sim and AR teleoperation pipeline. Future work will focus on conducting comprehensive quantitative assessments to systematically evaluate usability, intuitiveness, and cognitive workload.

[![Fig. 6. - 
            Supplementary videos provide additional details of the demonstration. (a) The user moves the virtual robot freely in 3D space, while the physical robot follows with a designed delay. (b) The user teleoperates the robot to reach a hanging target point. (c) The user teleoperates the robot to grasp an object being handed over in mid-air. (d) The user teleoperates the robot to grasp a toy placed on a chair.
          ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11522815/11522816/11522919/huang6-robosoft26-298-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11522815/11522816/11522919/huang6-robosoft26-298-large.gif)

**Fig. 6.**

Supplementary videos provide additional details of the demonstration. (a) The user moves the virtual robot freely in 3D space, while the physical robot follows with a designed delay. (b) The user teleoperates the robot to reach a hanging target point. (c) The user teleoperates the robot to grasp an object being handed over in mid-air. (d) The user teleoperates the robot to grasp a toy placed on a chair.

**Table II** Human-Robot Interaction Experiments

### ACKNOWLEDGMENTS

This research is supported by the National Research Foundation, Prime Ministers Office, Singapore under its Campus for Research Excellence and Technological Enterprise (CREATE) programme. The Mens, Manus, and Machina (M3S) is an interdisciplinary research group of the Singapore MIT Alliance for Research and Technology (SMART) centre. This research is also supported by A\*STAR, Singapore, through the Italy-Singapore collaborative project “DESTRO - Dextrous, strong yet soft robots” (C.L.).