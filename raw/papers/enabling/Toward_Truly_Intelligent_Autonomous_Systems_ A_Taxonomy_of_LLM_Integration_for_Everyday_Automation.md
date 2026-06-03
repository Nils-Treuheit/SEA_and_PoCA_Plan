---
title: "Toward Truly Intelligent Autonomous Systems: A Taxonomy of LLM Integration for Everyday Automation"
source: "https://link.springer.com/chapter/10.1007/978-3-032-00986-9_6"
author:
  - "[[Magnus Jung]]"
  - "[[Thorsten Hempel]]"
  - "[[Basheer Al-Tawil]]"
  - "[[Qiaoyue Yang]]"
  - "[[Sven Wachsmuth]]"
  - "[[Ayoub Al-Hamadi]]"
published: 2026-01-01
description: "With the rapid development of large language models (LLMs), their integration into autonomous systems has become essential. This integration significantly increases the flexibility and adaptability of the system. In this paper, we propose a categorization of LLM..."
tags: [ROBOVIS 2025, Conference]
---
## Abstract

With the rapid development of large language models (LLMs), their integration into autonomous systems has become essential. This integration significantly increases the flexibility and adaptability of the system. In this paper, we propose a categorization of LLM integration into three levels: open-loop, closed-loop, and autonomous systems fully driven by robotic LLMs. They are analyzed through existing literature, real experiments with the humanoid robot TIAGo, and simulations with models such as ChatGPT-4 and Vicuna 13b-v1.5-16k. We demonstrate the potential of LLMs to enhance the flexibility and adaptability of autonomous systems, particularly in dynamic environments where conventional finite state machines may prove inadequate. Closed-loop systems effectively handle unexpected situations with human-like problem solving. Integrating LLMs with autonomous systems enables new real-world applications by enhancing their ability to adapt, reason, and respond intelligently in dynamic environments.

## 1 Introduction

**Fig. 1.**

[![Flow chart illustrating a process with three main stages: "Case Loss," "Closed Loop," and "Case End." The "Case Loss" stage includes a blue circle labeled "Start" leading to an orange arrow labeled "Loss" and a green rectangle labeled "Loss Outcome." The "Closed Loop" stage features a blue circle labeled "Start," an orange arrow labeled "Link," and multiple green rectangles labeled "Link Outcome." The "Case End" stage shows a blue circle labeled "End," an orange oval labeled "Final Outcome," and a green rectangle labeled "Audit Outcome." Arrows indicate the flow between stages.](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-3-032-00986-9_6/MediaObjects/637836_1_En_6_Fig1_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-3-032-00986-9_6/figures/1)

Schematic representation of the three levels of LLM integration for autonomous systems (open-loop, closed-loop and robotic LLM systems).

Intelligent systems (e.g., intelligent robots or assistive systems) are central to many futuristic applications, yet society’s expectations often exceed the current state of the art \[[^32]\]. While research continues to advance individual capabilities and AI-powered modules for autonomous systems such as robots, assistive systems, software systems, smart home systems, autonomous vehicles, etc., these systems still fall short of being intuitive or truly intelligent \[[^43]\]. For example, humanoid robots equipped with modules such as emotion recognition, gaze direction estimation, and engagement detection may struggle to exhibit appropriate social behavior. For instance, while they may be able to recognize a smiling user, they often rely on predefined scripts to respond and lack the ability to adapt their interactions based on context \[[^40]\]. Another example is state driven workflows for adaptive control systems. SQL queries and Bash commands often rely on rigid, predefined rules, making it difficult to detect errors and resolve problems dynamically \[[^66]\]. This lack of adaptability leads to lower success rates and efficiency compared to systems that could seamlessly connect the various modules dynamically through contextual knowledge.

Open problems in autonomous systems are often addressed by breaking them down into sub-goals; this approach increases the challenge of bringing the solutions together. Despite the increasing use of AI to tackle specific tasks, the overall integration and adaptability of these modules remains limited, preventing intelligent systems from meeting human-level expectations \[[^31]\]. The challenge for intelligent action is not in the individual modules, but in how they interact with the environment and align with the system’s goals. It is the thoughtful and adaptive interaction between these modules that builds competence and ultimately enables natural and intelligent behavior in autonomous systems.

The rapid development of large language models (LLMs) in recent years has made natural language processing possible. By incorporating LLMs and their knowledge processing capabilities, natural interaction becomes possible, and task planning can be automated at a low- or no-code level.

This paper explores three levels of LLM integration - open-loop, closed-loop, and robotic LLM systems - into autonomous systems, using the TIAGo robot from Pal Robotics as a demonstrator (see Fig. [2](#Fig2)) to evaluate the advantages and disadvantages of each approach. In the open-loop configuration, a state machine with predefined states and transitions is generated and then executed on the autonomous system. In the closed-loop configuration, an LLM dynamically selects the next state and its parameters in real time, adapting based on the previous states of the system and the current task requirements. At the third level of integration, predefined states are completely removed and the MLLM (a multimodal LLM) directly controls the autonomous system by processing multiple input modalities and generating corresponding actions without relying on a predefined state structure. In the following sections, these levels are explained, analyzed, simulated, and implemented (see Fig. [1](#Fig1)). There are significant differences between these systems in terms of flexibility, security, and hardware requirements, as shown in Fig. [3](#Fig3) and detailed in Table [1](#Tab1). It is shown that the closed-loop system shows a significant increase in performance compared to previous systems in dynamic environments with disturbances and that it is (currently) superior to other LLM integration depths in terms of feasibility, safety and precision.

**Fig. 2.**

[![A robotic device labeled "TIAGO" is shown in a room, holding a can with its mechanical arm. The robot is white with a black jointed arm and a head featuring sensors or cameras. In the background, there are posters and a screen displaying diagrams and text. The setting appears to be a laboratory or research environment.](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-3-032-00986-9_6/MediaObjects/637836_1_En_6_Fig2_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-3-032-00986-9_6/figures/2)

Humanoid robot TIAGo from Pal Robotics performing a pick-and-place task autonomously using closed-loop LLM integration.

## 2 Related Works

The creation of a variety of discrete actions for tackling independent problems leads to a variety of atomic actions that can be stored in a library as functions with input variables and parameters \[[^44]\]. These actions can be combined to perform previously unknown tasks \[[^44]\] that can be achieved through the use of finite state machines. Most current systems operate in an open-loop configuration, meaning that once the task and motion plan have been generated, they cannot troubleshoot or adapt to unexpected changes in the environment \[[^12], [^22]\].

### 2.1 State Machines

Since the 2000s, there has been minimal change in how individual capabilities are combined to enable machines to autonomously solve tasks and pursue goals \[[^10], [^42]\]. Rule-based systems, typically implemented as finite state machines, remain the dominant approach \[[^19]\]. In these systems, the precise sequence and order of actions are critical to success \[[^22]\].

State machines model systems with finite states and transitions, where actions occur within states and transitions move between them \[[^20]\]. This paper focuses on actions performed within states and logically arranged by transition conditions. State machines are often used to model and control the behavior of systems in robotics, software engineering, and automation \[[^19], [^34]\]. The states and their transition conditions are (manually) predefined, leading to deterministic behavior in static environments (without random transition selection) \[[^49]\]. They have evolved through fuzzy logic (1960s) \[[^67]\], behavior-based robotics (1980s) \[[^9]\], and probabilistic systems (1990s) \[[^26]\]. In the 2000s, these systems were further enhanced with machine learning and artificial intelligence \[[^8]\], although these approaches offer limited flexibility and only predefined possible outcomes.

Today, autonomous systems include a wide range of scientifically validated skills (modules) \[[^43]\], spanning actuation (e.g., movement \[[^53]\], speech output \[[^5]\]) and input processing (e.g. image processing for pose estimation \[[^15]\], gaze recognition \[[^1], [^21]\], social information \[[^2]\], SLAM \[[^3]\], speech recognition \[[^4]\], etc.).

However, the interaction between actuation and sensor processing is very inflexible due to a lack of common sense \[[^12]\]. Specifically, the lack of independent assessment of situations using logical reasoning, experiential knowledge, practical judgment and social understanding. This gap can now be filled by LLMs.

### 2.2 Large Language Models

Large Language Models (LLMs) have attracted considerable attention in recent years. They are transformer-based models that acquire strong language processing and generation capabilities by learning statistical relationships from large amounts of text during a self-supervised and semi-supervised training process \[[^7]\]. Based on these large amounts of text during training, they can reflect the common sense of society in inference \[[^12]\]. Further, they are able to process additional knowledge from supplementary information during inference \[[^50]\].

In conjunction with processed sensor inputs such as object recognition \[[^46]\], face recognition \[[^29]\], emotion recognition \[[^13]\], body pose estimation \[[^15]\], gaze recognition \[[^1], [^21]\], speech processing \[[^4]\], VLMs (vision-language models) \[[^72]\], etc., these systems can flexibly respond to environmental changes and unexpected situations \[[^12]\]. In this context, the output of the LLM is not predefined, non-deterministic, but determined by the specific interplay of task and environmental influences.

Training custom LLMs is not feasible for most institutions due to hardware requirements \[[^37], [^50]\]. However, pre-trained LLMs exhibit human-like common sense influenced by their training data, and vary in speed, size, comprehension, and memory \[[^45]\]. In addition, there are various methods for adapting existing LLMs to specific requirements, including hidden or system prompts \[[^30]\] and Retrieval-Augmented Generation (RAG) \[[^18]\].

**System Prompts.** Using system prompts, some set before interaction with the LLM begins and some appended to each prompt (sometimes before and sometimes after the prompt), the output of the LLM can be directed \[[^30]\]. This includes positioning the LLM in a specific role with defined behavioral instructions \[[^61]\]. The format of the response can also be specified, allowing further processing of the free text response \[[^6]\]. By imposing strict rules within the system prompts, the LLM is prepared for use within a more complex system, and disruptive properties of the LLM are suppressed.

**Retrieval Augmented Generation.** RAG in the context of LLMs is a technique that aims to improve the generative performance of these models by integrating retrieved information from external knowledge sources \[[^73]\]. LLMs have impressive text generation capabilities; however, they are limited to the knowledge available during their training \[[^68]\]. This knowledge is static and may become outdated or incomplete. In addition, it is biased due to the distribution of the training data \[[^17]\], which limits its effectiveness in applications for autonomous systems. Through RAG, user prompts are enhanced by dynamically adding relevant retrieved data in context \[[^18]\]. This includes conveying the characteristics and capabilities of the system, as well as providing example applications of the system, or recording past applications to gain knowledge and insight for future tasks.

This paper examines the integrability of LLMs in autonomous systems and classifies their depth of integration using a three-level taxonomy and compares it with the current state. It provides an outlook on the possibilities of future applications, as well as their validation and risks. Simulations of simple situations are used to demonstrate the strengths of these approaches.

## 3 Taxonomy of LLM Integration

The integration of the LLM into autonomous systems takes place on several levels. Here the LLM assesses problems and supports task planning while maintaining compatibility with existing solutions.

A brief overview of the strengths and weaknesses of the three integration levels is given in the Table [1](#Tab1). Depending on the architecture of the system, combinatorial applications of the integration levels are also possible. As LLMs are easily distracted by unnecessary information \[[^55]\], the decision about necessary sensor inputs (information acquisition) is left to the LLMs. This ensures that only necessary information is processed and that the LLM remains focused. All three levels use system prompts and RAG. System prompts are used to define the output format and interpret the task from the perspective of an autonomous system. RAG provides the LLM with system information, state definitions, background information, hints, example tasks, and their results from previous applications. This enables the generation of new solutions based on specific experience and prior knowledge.

**Fig. 3.**

[![Comparison chart showing three categories: Flexibility, Security, and Computing Power. Each category is represented by a triangle with a gradient from green to red. The rows are labeled Open-Loop, Closed-Loop, and Robot-LLM, indicating different systems. The chart visually compares these systems across the three categories.](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-3-032-00986-9_6/MediaObjects/637836_1_En_6_Fig3_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-3-032-00986-9_6/figures/3)

Overview of the main comparison criteria for the three levels of integration of LLM in autonomous systems.

**Table 1. Comparison of the three LLM integration levels, using ‘xx’ (very applicable), ‘x’ (applicable), and ‘(x)’ (partially applicable).**

### 3.1 Level 1: Open-Loop Systems

The development of autonomous systems often involves the manual creation of state machines to define tasks and ensure safety \[[^47]\]. However, this can cause the state machine to stop in dynamic environments, making full task execution unreliable \[[^12]\]. The manual creation process is time-consuming and complex, as even simple tasks can result in large, unwieldy machines \[[^22]\]. While prebuilt substate machines improve clarity, they limit flexibility and cannot respond to unexpected events due to their predetermined, open-loop structure.

A promising alternative to manually created state machines is the use of LLMs that can generate a concrete state machine from a free-form task description (see Fig. [4](#Fig4)). Guided by system prompts and RAG, these models produce high-quality state machines comparable to manually generated ones \[[^28]\]. The advantage is that the user doesn’t need any specific prior knowledge, and the generated state machine is deterministic, verifiable, and adjustable via graphical interfaces without programming skills \[[^24]\].

Regarding the three criteria security, flexibility, and computational power, it is evident that this LLM-based state machine development (open-loop) offers high security, since the end user does not have direct access to the LLM, thus preventing problems like prompt injection \[[^41]\]. Task verifiability is ensured by graphically examining the generated state machine, with individual states designed to remain safe regardless of combinations. In addition, state machine generation can be offloaded as it does not require real-time execution on the device, allowing the use of powerful LLMs. The flexibility of the approach lies in the natural task descriptions and their automatic interpretation by the LLM. However, the flexibility of the task execution is limited, as no adaptation to unexpected events is possible.

**Fig. 4.**

[![Flow chart illustrating a process. A blue speech bubble labeled "Text" points to a red rectangle labeled "LLM." Above, a gray box labeled "Data" points to the LLM. Below, a purple box labeled "System Analysis" also points to the LLM. The LLM then points to a green rectangle labeled "State Machine," which is part of an "Autonomous system." The chart is titled "Open-Loop."](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-3-032-00986-9_6/MediaObjects/637836_1_En_6_Fig4_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-3-032-00986-9_6/figures/4)

Schematic of the first level of LLM integration for autonomous systems: the open-loop. In this configuration, a state machine is first generated and then executed on the autonomous system.

Several approaches have been proposed to use LLMs in state machine generation. One innovative method uses LLMs as task planners, decomposing natural language descriptions into sub-goals, which are then translated into robot actions \[[^22], [^23]\]. Another approach uses the code-writing capabilities of LLMs to generate robot programs, with “code-as-policies” defining robot-centric programs in Python that invoke parameterized primitives \[[^22], [^38]\]. However, these methods have mainly addressed smaller subtasks, leaving full state machine generation for autonomous system control unrealized. The Alchemist system allows end-users to create and execute robotic applications using natural language input, bypassing the need for specialized programming knowledge \[[^11], [^51]\]. Despite its utility, Alchemist faces challenges in debugging the generated code, highlighting the difficulties of open-loop programming \[[^28], [^59]\]. The PROGPROMPT project uses LLMs to build executable programs based on specifications of available actions and objects \[[^58]\]. While traditional high-level planning in robotics involves searching a predefined space \[[^16], [^25]\], PROGPROMPT directly generates a complete program with predefined action primitives \[[^58]\]. However, its linear execution avoids loops and does not adapt to errors, limiting flexibility for new tasks. Another open-loop concept is CODEBOTLER \[[^22]\], an open-source tool designed to simplify service robot programming using LLMs and a user-friendly web interface. It generates LMPs that can be executed via ROS Actionlib on different robot platforms. However, CODEBOTLER lacks support for low-level behaviors, such as the instruction “follow Alice to her office” \[[^22]\], and linear execution prevents it from responding to dynamic changes or unforeseen events \[[^22]\]. Another model, the Foundation Model-based Open Vocabulary Task Planning and Executive System for General Service Robots \[[^47]\], focuses on task execution in the RoboCup@Home competition. It uses ROS with SMACH, but is limited to linear processes with sub-state machines, which hinders flexibility and responsiveness to dynamic environmental changes or task uncertainties \[[^47]\].

An important area where LLMs are used as planning aids in the open-loop method is navigation. For example, a cobot navigation approach in human-centered smart manufacturing can interpret natural language commands and generate Python code for navigation actions \[[^63]\]. The Pathfinder algorithm performs path planning, enabling the Autonomous Transport System (ATS) to interpret complex voice commands for task completion \[[^63]\]. For effective navigation, the LLM is given scene semantics, allowing it to generate simple instructions such as “Go to object XY”. From this point on, the path planner takes over the execution of the instruction \[[^63]\].

The SMART-LLM study \[[^27]\] shows that LLMs can control multi-agent systems by breaking down high-level tasks into subtasks for robot coordination, demonstrating their role in both planning and multi-robot management.

These open-loop systems can be enhanced by improving the knowledge base of LLMs. Appropriate collaboration between humans and autonomous machines can improve the system through hints and enhancements provided by humans \[[^39]\]. The improved functions/processes are stored for reuse in subsequent processes.

The lack of error detection in open-loop systems is a major limitation, especially in complex environments. Initial attempts use uncertainties from multimodal LLMs (MLLMs), but these only restart the open loop without dynamically responding to changes \[[^70]\]. Hybrid systems with step-by-step instructions offer more granular planning, but still face the same problem of restarting the process instead of adapting to errors or changes \[[^57]\]. In addition, generating long executable code or large state machines with LLMs is challenging \[[^62]\]. To address these issues, a shift to a closed-loop system is needed, where the LLM incrementally solves problems at runtime.

### 3.2 Level 2: Closed-Loop Systems

A closed-loop approach requires the ad hoc execution of the LLM (on the autonomous device) to determine the next action module (state) during task execution. This makes the real-time capability of the LLM, and therefore its size, relevant. This level of integration also uses existing and pre-defined states and transitions, leveraging the accumulated experience to solve specific tasks. The LLM acts as the master of the system, deciding after each state based on the information from the transition and the parameters collected during the execution of the previous actions (see Fig. [5](#Fig5)). To increase speed, pre-defined routines or sub-state machines can also be considered as states.

The system has access to a wide range of reliable functions, enabling it to respond directly to dynamic environments and unexpected situations. The LLM determines the next required state, including not only action states but also states for gathering additional information. Using RAG, the LLM learns from examples and its own experience by storing state sequences.

The closed-loop system balances safety, flexibility, and computational power. It is safe as actions occur only through predefined states, but the combination of these states increases the demands on their integrity. To avoid errors, system prompts and RAG must be optimized to prevent the LLM’s output from exceeding the system’s framework. In terms of flexibility, the system is limited only by the number of available states; more states allow for greater creativity and tailored solutions, making actions more human-like. The system can be trained and optimized by RAG using past trajectories to improve its task performance. In terms of computational power, running the LLM on the autonomous device requires careful selection to avoid performance degradation during decision pauses.

**Fig. 5.**

[![Flow chart illustrating the CaseA-Loop process. It begins with a blue circle labeled "Task," leading to a red arrow marked "LM." Above this arrow is a gray box labeled "RAS." Below is a purple box with "System message." The red arrow points to a green stack labeled "State," which is part of the "Autonomous System" section. Black arrows indicate the flow direction between elements.](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-3-032-00986-9_6/MediaObjects/637836_1_En_6_Fig5_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-3-032-00986-9_6/figures/5)

Schematic representation of the second level of LLM integration for autonomous systems: the closed-loop. In this configuration an LLM runs continuously on the system and selects the next state and its parameters in real time on the autonomous system based on previous states and transitions and the task at hand.

Especially in path planning, simple closed-loop systems are already in use \[[^14], [^33], [^54], [^56]\]. One example is the 3P-LLM \[[^33]\], which allows probabilistic path planning using LLMs. It demonstrates that models such as GPT-3.5-turbo can provide real-time feedback for path planning and outperform their more powerful counterparts. The system receives user instructions in natural language describing the destination within the environment and dynamically updates the path when an obstacle is detected \[[^33]\].

In addition, studies on object-based navigation have shown that LLMs can not only support navigation, but also improve object recognition. A study on zero-shot object navigation \[[^14]\] emphasizes that humans do not adhere to predefined class labels, but describe objects in free, natural language. The system synthesizes prompts for the LLM using extracted object labels. Pre-trained models such as CLIP \[[^52]\] and GLIP \[[^36]\] are used to perform zero-shot target anchoring of open vocabulary in the wild \[[^14]\]. The LLM controls the navigation process by generating an output from the object list that tells the agent in which direction to move. When the target object is located, the decision process is complete and the agent navigates directly to it \[[^14]\]. This iterative approach highlights the flexibility of LLMs to respond to dynamic environments. The LFENav system \[[^56]\] also uses LLMs for semantic navigation. The emergence of LLMs provides new avenues for semantic understanding, allowing them to analyze user instructions, select sub-goals, and generate action instructions.

As an alternative to state machines, the concept of LLM-BT \[[^71]\], which combines LLMs with behavior trees, represents a promising solution. This method allows the addition of new actions based on changes in the environment and assigns appropriate priorities for the execution of these actions, thereby increasing the adaptability of the system \[[^71]\].

Finally, cooperative agents, as seen in CAMON \[[^64]\], have shown that LLMs have remarkable capabilities for planning and communication in a closed-loop context. These systems enable multiple agents to communicate efficiently, share tasks and collaborate through a dynamic leadership organisation, further enhancing navigation efficiency in complex environments \[[^64]\].

Collectively, these approaches illustrate how LLMs in closed-loop systems not only extend existing capabilities, but also create new opportunities for autonomous navigation and interaction with dynamic environments. They not only promote flexibility and creativity in problem solving, but also enhance contextual understanding and common sense, which are critical to the success of autonomous systems. These advances in LLM technology and their application to autonomous systems can be seen as a fundamental step towards smarter and more adaptive robots capable of successfully tackling complex tasks in dynamic environments.

### 3.3 Level 3: Robot-LLM

In a fully integrated LLM within the autonomous system, no states are used. There is minimal pre-processing of sensor data (e.g., image data by VLMs) and hardware-level outputs are used for system control. It is possible to have a specific MLLM optimized for the specific input and output channels \[[^65]\], thus forming a robot LLM (see Fig. [6](#Fig6)). This allows the system to achieve maximum flexibility, but also requires it to manage its capabilities autonomously. There is no prior knowledge of its capabilities. As a result, the reliability of the execution of specific tasks is reduced \[[^35]\]. In addition to the LLM, powerful VLMs are needed to process the input and convert it into linguistically understandable information. It is critical that the information conveyed is relevant at the time. This can be achieved by MLLMs.

**Fig. 6.**

[![Flow chart illustrating an autonomous system. On the left, a blue circle labeled "Task" connects with a large orange oval labeled "Multimodal LLM" on the right. Arrows indicate the flow of information between these elements. A gray rectangle labeled "RAG" points towards the "Multimodal LLM," while a purple rectangle labeled "System Prompts" points towards the "Task." The entire diagram is enclosed within a dashed green line labeled "Autonomous System."](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-3-032-00986-9_6/MediaObjects/637836_1_En_6_Fig6_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-3-032-00986-9_6/figures/6)

Schematic representation of the third level of LLM integration for autonomous systems: the robot LLM. In this configuration, the use of predefined states is eliminated and the MLLM takes direct control of the autonomous system, receiving all inputs and controlling all outputs.

Considering the three criteria of security, flexibility and computational power, robot-LLM represents a very promising scenario for flexibility, provided that the LLM can develop a complete understanding of the system and gain full control over all components. This is challenging and makes existing models and methods obsolete. It raises the capabilities of the system to a level where natural and intuitive behavior of the autonomous system becomes possible. However, there is a concern that accuracy may decrease, making this variant attractive for Human-Robot Interaction (HRI), but still requiring separate modules for tasks that require precision. In terms of computing power, Level 3 relies heavily on high computational power to process complex multimodal inputs in real time and generate solutions. In terms of security, this system is also much more vulnerable to LLM hacking. The different modalities can influence the LLM and cause undesired behavior. Furthermore, the high level of flexibility increases the risk of unintended and erroneous actions, as there is no safety net provided by inherently secure and verifiable states. This system has not yet been implemented.

### 3.4 Risks of LLM Integration

The use of LLMs presents both significant opportunities and certain risks. On the one hand, the advantage of incorporating common sense also brings a disadvantage, as it can reduce the precision and accuracy of executions. This can be mitigated by using states with specific actions, such as dedicated AI models for specific tasks. On the other hand, LLMs are prone to hallucination, resulting in responses that do not match the desired output format for further processing. This problem is exacerbated by the fact that LLMs, originally designed as open dialog systems, are now being repurposed to control autonomous systems.

There is also the risk of LLM hacking. In open-loop systems, only authorized individuals have access to the inputs provided to the LLM. In closed-loop systems, however, the LLM can be manipulated during speech processing and parameter passing. This problem is even more pronounced in robotic LLMs, where any external input can be passed directly to the multimodal LLM, potentially leading to system manipulation.

## 4 Simulations and Real-World Experiments

In this study, we investigated the integration of LLMs into open-loop and closed-loop autonomous systems. The experiments were partly performed with the humanoid robot TIAGo from Pal Robotics (see Fig. [2](#Fig2)) and partly simulated with different LLMs (ChatGPT-4 \[[^48]\] and Vicuna 13b-v1.5-16k \[[^69]\], a fine-tuned LLaMA model \[[^60]\]). The state machines were implemented using ROS SMACH, and a graphical user interface (GUI) was provided to manually create state machines from the predefined states and transitions. A significant improvement in the functionality of both open-loop and closed-loop systems was achieved by using intuitive naming conventions for individual states. When the function was clearly identifiable from the state name, even the smaller LLM could work more effectively with these states.

The states, their descriptions, and example state machines were made available to the LLMs through the RAG. The LLMs either acted as generators of state machines (open-loop) or as masters determining the next state in the sequence (closed-loop). At level 1, the system was characterized as a generator of autonomous systems, with a predefined output format that included the states and their transitions. At level 2, the system was optimized to select the next state based on the transitions and parameters of the previous state, without parallelization (only one subsequent state). The output format at level 1 consisted of two lists - states with parameters and transitions - while at level 2 only the next state with parameters was generated (see Fig. [2](#Tab2)). If the LLM produced an invalid format, the task was repeated with more explicit instructions about the desired format.

A variety of example tasks from the field of autonomous mobile assistance systems were used to evaluate the possibilities and challenges of open and closed-loop systems. These experiments included fine-grained states as well as experiments combining single states into larger sub-state machines. The tasks were always presented to the LLM in free-form language.

**Table 2. Comparison of output formats for open-loop and closed-loop systems. The output generated by the LLM in the open-loop system is significantly longer and more complex. This increased length and complexity correlates with a higher failure rate.**

### 4.1 Open-Loop

Creating state machines manually, even with the help of a GUI, became increasingly time-consuming as their complexity increased. In contrast, LLMs significantly streamlined the process in an open-loop system. For simpler tasks, where larger sub-state machines could be used as modular building blocks, all of the LLMs tested produced results identical to those created manually. However, for more complex tasks requiring specialized knowledge, such as working with AprilTags, smaller models such as Vicuna 13b-v1.5-16k struggled due to their limited understanding and began to hallucinate. While RAG improved the performance of these smaller models, the reliability of their results remained inconsistent. ChatGPT-4, on the other hand, consistently completed tasks effectively. However, as task complexity increased, error rates increased for both small and large LLMs. The main sources of error were incorrect output formats and hallucinations. The error rate also depended on the task and task formulation, but could be described qualitatively as an exponential increase.

The complexity of state machines is further limited by the token limits of LLMs. This also affects the ability to process additional information via RAG, where iterative approaches helped but posed challenges in maintaining coherence over multiple iterations. Real-world applications typically require more complex state machines with a large number of states and transitions. Without decomposing tasks into smaller sub-state machines, practical applicability remains uncertain.

### 4.2 Closed-Loop

Closed-loop systems offer a significant advantage in their ability to respond flexibly to disturbances because the LLM can dynamically respond to unexpected events. This can exceed the performance of predefined open-loop state machines.In addition, the closed-loop system only needs to generate very short outputs consisting of the next state and its parameters, unlike open-loop methods where complete state machines are generated in advance. This simplified output facilitates automatic validation of the LLM’s response and reduces the risk of hallucination, allowing immediate correction. In addition, the token limit has much less impact on the complexity of the resulting state machine.

As with the open-loop system, performance varied according to the technical knowledge of the LLM. ChatGPT-4 consistently provided better state suggestions due to its broader understanding of technical concepts. In environments with minimal perturbations - where only perturbations accounted for in manually created state machines occurred - both large and small LLMs were able to mimic complex state machines and produce sequences identical to those created manually. However, smaller models such as Vicuna 13b-v1.5-16k were more likely to encounter problems with states that required specific technical knowledge (these states were often skipped, resulting in certain tasks becoming unexecutable). Improving the RAG integration reduced the error rate, suggesting that optimizing the RAG process could address these issues in the future. ChatGPT-4, on the other hand, did not encounter these problems because it already had the necessary specific technical knowledge without relying on RAG.

In addition, especially with the smaller LLM Vicuna 13b-v1.5-16k, it became apparent that the focus on the task and previously performed actions would drift away after a certain period of time. While repeated task delivery and the history of previously traversed states led to improvements, the problem could not be completely resolved.

When tested in more dynamic scenarios, the closed-loop system showed creative but reasonable responses to unforeseen events. For example, if an object was not found in the expected location, the system searched other areas of the room. If the object was still not found, it interacted with people to inquire about the object. If a particular person could not be found to hand over the object, the system tried to locate the person by calling out. If this failed, the system either gave the object to another person, specifying who it was for, or kept the object and waited for it to be picked up. These experiments highlight the problem-solving approach of a closed-loop system, where state machines become more complex as perturbations and task complexity increase. However, since the LLM only needs to determine the next step, the error rate - especially with ChatGPT-4 - remained close to zero after optimizing the RAG and well-chosen system prompts and demonstrating human-like problem solving strategies.

The behavior of the system varied depending on the contextual information included in the free-form task description. For example, when an object was described as “critical,” the LLM refused to release it to unauthorized persons. This demonstrates that level-2 closed-loop systems can mirror human decision-making behavior and respond flexibly to perturbations.

In addition, several simulations were run to analyze the extent to which the prior knowledge provided by the RAG led to better results. In addition to the available states and their definitions, the LLM was provided with state sequences and associated transitions from previous tasks. This was illustrated by the following simulation: A level 2 system was initially tasked with navigating to a specific room using only basic motion commands (“move forward” (to the next door/into the room), “turn 90 $^\circ$ left”, “turn 90 $^\circ$ right”, and “where am I?”). The floor plan is shown in Fig. [7](#Fig7). As prior knowledge, the system was given information about the number of doors on the left and right. Without any other prior knowledge, the system tried to search the rooms one by one until it found the corresponding room. In contrast, the system that had this sequence as prior knowledge went directly to the desired room and also found the shortest routes to the previously discovered rooms. This demonstrates the ability of LLMs to optimize task performance through experience, which can be transferred via RAG.

**Fig. 7.**

[![Diagram of a circuit with labeled sections: "Area 1," "Site 1," and "Zone 2." The circuit includes blue and green lines representing different pathways, with a circle labeled "10 kV" at the end of the green line. A legend indicates blue lines as "Current flow in normal mode" and green lines as "Current flow during backup."](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-3-032-00986-9_6/MediaObjects/637836_1_En_6_Fig7_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-3-032-00986-9_6/figures/7)

Illustration of the route planning of a simulated closed-loop system. The goal is to navigate to the laboratory. Without prior knowledge (blue path) the system needs 13 steps. With the knowledge gained from the first run without prior knowledge (via RAG), it was able to find the shortest path (green) and took only two steps. (Color figure online)

It can be observed that LLMs in closed-loop systems are significantly more robust in proposing meaningful next states, with fewer hallucinations and output format errors. These systems are capable of solving more complex tasks and are more flexible in responding to and communicating with their environment.

## 5 Conclusion

This paper presents a taxonomy for autonomous systems with integrated LLMs. First tests already show the capabilities as well as the advantages and disadvantages of such systems. It is shown that with existing resources, systems can be realized that use LLMs to control state machines in closed-loop operation. This leads to a significant increase in intuitiveness of use and interaction, as well as improved adaptation to unexpected disturbances and creative solutions.

A key advantage of these systems is their natural and intuitive operation. The use of LLMs significantly improves situational understanding and adaptability, enabling the successful completion of more complex tasks that require greater flexibility and are subject to more disturbances. In addition, LLMs have been shown to be easier to integrate into closed-loop systems and significantly less prone to error, particularly due to shorter outputs and better situational understanding. This also makes it easier to create more complex, less linear processes.

Another advantage of these systems is their ability to learn from previous situations. By providing information about the system, previous processes, and background information via RAG, the system can be “briefed” in a targeted manner. This enables the system not only to repeat past processes, but also to draw conclusions from them, optimize them, and find parallels to new tasks.

The vast repertoire of potential skills for autonomous systems can be extended and used for everyday, intuitive and practical support. By assembling these capabilities into states and their sequences, a code-free approach to autonomous systems can be implemented, allowing productive use even without specific knowledge. The field of application is not limited to robotics, but can be applied to any system that can be described by state machines, where basic flexibility increases utility. In particular, the use of closed-loop systems significantly increases the capabilities of systems and enables human-centered applications.

Future work will test these systems in complex scenarios, including multi-robot collaboration, and explore applications in domains such as smart homes, manufacturing, and healthcare to demonstrate their versatility. A more comprehensive comparison with other state-of-the-art automation techniques will also be conducted to deeply evaluate their respective strengths and limitations in practical use cases.

## References

## Acknowledgments

This work is funded and supported by the German Research Foundation (DFG) (SEMIAC under grant number No. 502483052), by the Federal Ministry of Education and Research of Germany (BMBF) (AutoKoWAT-3DMAt under grant No. 13N16336) and by the European Regional Development Fund (ERDF) (ENABLING under grant No. ZS/2023/12/182056 and RoboLab under grant No. ZS/2023/12/182065).

## Editor information

### Editors and Affiliations

## Ethics declarations

### Disclosure of Interests

The authors have no competing interests to declare that are relevant to the content of this article.

## Rights and permissions

## About this paper

### Cite this paper

Jung, M., Hempel, T., Al-Tawil, B., Yang, Q., Wachsmuth, S., Al-Hamadi, A. (2026). Toward Truly Intelligent Autonomous Systems: A Taxonomy of LLM Integration for Everyday Automation. In: Röning, J., Filipe, J. (eds) Robotics, Computer Vision and Intelligent Systems. ROBOVIS 2025. Communications in Computer and Information Science, vol 2629. Springer, Cham. https://doi.org/10.1007/978-3-032-00986-9\_6

- [.RIS](https://citation-needed.springer.com/v2/references/10.1007/978-3-032-00986-9_6?format=refman&flavour=citation "Download this article's citation as a .RIS file")
- [.ENW](https://citation-needed.springer.com/v2/references/10.1007/978-3-032-00986-9_6?format=endnote&flavour=citation "Download this article's citation as a .ENW file")
- [.BIB](https://citation-needed.springer.com/v2/references/10.1007/978-3-032-00986-9_6?format=bibtex&flavour=citation "Download this article's citation as a .BIB file")
- DOI https://doi.org/10.1007/978-3-032-00986-9\_6
- Published 01 October 2025
- Publisher NameSpringer, Cham
- Print ISBN978-3-032-00985-2
- Online ISBN978-3-032-00986-9
- eBook Packages [Computer Science](https://link.springer.com/search?facet-content-type=%22Book%22&package=11645&facet-start-year=2026&facet-end-year=2026) [Computer Science (R0)](https://link.springer.com/search?facet-content-type=%22Book%22&package=43710&facet-start-year=2026&facet-end-year=2026) [Springer Nature Proceedings Computer Science](https://link.springer.com/search?facet-content-type=%22Book%22&package=85344&facet-start-year=2026&facet-end-year=2026)

### Keywords

## Publish with us

[Policies and ethics](https://www.springernature.com/gp/policies/book-publishing-policies)

[^1]: Abdelrahman, A.A., Hempel, T., Khalifa, A., Al-Hamadi, A., Dinges, L.: L2CS-Net: fine-grained gaze estimation in unconstrained environments. In: 2023 8th International Conference on Frontiers of Signal Processing (ICFSP), pp. 98–102. IEEE (2023)

[Google Scholar](https://scholar.google.com/scholar?&q=Abdelrahman%2C%20A.A.%2C%20Hempel%2C%20T.%2C%20Khalifa%2C%20A.%2C%20Al-Hamadi%2C%20A.%2C%20Dinges%2C%20L.%3A%20L2CS-Net%3A%20fine-grained%20gaze%20estimation%20in%20unconstrained%20environments.%20In%3A%202023%208th%20International%20Conference%20on%20Frontiers%20of%20Signal%20Processing%20%28ICFSP%29%2C%20pp.%2098%E2%80%93102.%20IEEE%20%282023%29)

[^2]: Abdelrahman, A.A., Strazdas, D., Khalifa, A., Hintz, J., Hempel, T., Al-Hamadi, A.: Multimodal engagement prediction in multiperson human-robot interaction. IEEE Access **10**, 61980–61991 (2022)

[Article](https://doi.org/10.1109%2FACCESS.2022.3182469) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Multimodal%20engagement%20prediction%20in%20multiperson%20human-robot%20interaction&journal=IEEE%20Access&volume=10&pages=61980-61991&publication_year=2022&author=Abdelrahman%2CAA&author=Strazdas%2CD&author=Khalifa%2CA&author=Hintz%2CJ&author=Hempel%2CT&author=Al-Hamadi%2CA)

[^3]: Al-Tawil, B., Hempel, T., Abdelrahman, A., Al-Hamadi, A.: A review of visual slam for robotics: evolution, properties, and future applications. Front. Robot. AI **11**, 1347985 (2024)

[Article](https://doi.org/10.3389%2Ffrobt.2024.1347985) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=A%20review%20of%20visual%20slam%20for%20robotics%3A%20evolution%2C%20properties%2C%20and%20future%20applications&journal=Front.%20Robot.%20AI&volume=11&publication_year=2024&author=Al-Tawil%2CB&author=Hempel%2CT&author=Abdelrahman%2CA&author=Al-Hamadi%2CA)

[^4]: Alharbi, S., et al.: Automatic speech recognition: systematic literature review. IEEE Access **9**, 131858–131876 (2021)

[Article](https://doi.org/10.1109%2FACCESS.2021.3112535) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Automatic%20speech%20recognition%3A%20systematic%20literature%20review&journal=IEEE%20Access&volume=9&pages=131858-131876&publication_year=2021&author=Alharbi%2CS)

[^5]: Alonso Martin, F., Malfaz, M., Castro-González, Á., Castillo, J.C., Salichs, M.Á.: Four-features evaluation of text to speech systems for three social robots. Electronics **9** (2), 267 (2020)

[Article](https://doi.org/10.3390%2Felectronics9020267) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Four-features%20evaluation%20of%20text%20to%20speech%20systems%20for%20three%20social%20robots&journal=Electronics&volume=9&issue=2&publication_year=2020&author=Alonso%20Martin%2CF&author=Malfaz%2CM&author=Castro-Gonz%C3%A1lez%2C%C3%81&author=Castillo%2CJC&author=Salichs%2CM%C3%81)

[^6]: Beurer-Kellner, L., Fischer, M., Vechev, M.: Guiding LLMS the right way: fast, non-invasive constrained generation. arXiv preprint: [arXiv:2403.06988](http://arxiv.org/abs/2403.06988) (2024)

[^7]: Bharathi Mohan, G., et al.: An analysis of large language models: their impact and potential applications. Knowl. Inf. Syst., 1–24 (2024)

[Google Scholar](https://scholar.google.com/scholar?&q=Bharathi%C2%A0Mohan%2C%20G.%2C%20et%20al.%3A%20An%20analysis%20of%20large%20language%20models%3A%20their%20impact%20and%20potential%20applications.%20Knowl.%20Inf.%20Syst.%2C%201%E2%80%9324%20%282024%29)

[^8]: Bishop, C.M., Nasrabadi, N.M.: Pattern Recognition and Machine Learning, vol. 4. Springer (2006)

[Google Scholar](https://scholar.google.com/scholar?&q=Bishop%2C%20C.M.%2C%20Nasrabadi%2C%20N.M.%3A%20Pattern%20Recognition%20and%20Machine%20Learning%2C%20vol.%C2%A04.%20Springer%20%282006%29)

[^9]: Brooks, R.: A robust layered control system for a mobile robot. IEEE J. Robot. Autom. **2** (1), 14–23 (1986)

[Article](https://doi.org/10.1109%2FJRA.1986.1087032) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=A%20robust%20layered%20control%20system%20for%20a%20mobile%20robot&journal=IEEE%20J.%20Robot.%20Autom.&volume=2&issue=1&pages=14-23&publication_year=1986&author=Brooks%2CR)

[^10]: Chen, J., Abbod, M., Shieh, J.S.: Integrations between autonomous systems and modern computing techniques: a mini review. Sensors **19** (18), 3897 (2019)

[Article](https://doi.org/10.3390%2Fs19183897) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Integrations%20between%20autonomous%20systems%20and%20modern%20computing%20techniques%3A%20a%20mini%20review&journal=Sensors&volume=19&issue=18&publication_year=2019&author=Chen%2CJ&author=Abbod%2CM&author=Shieh%2CJS)

[^11]: Cobbe, K., et al.: Training verifiers to solve math word problems. arXiv preprint: [arXiv:2110.14168](http://arxiv.org/abs/2110.14168) (2021)

[^12]: Ding, Y., Zhang, X., Paxton, C., Zhang, S.: Task and motion planning with large language models for object rearrangement. In: 2023 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), pp. 2086–2092. IEEE (2023)

[Google Scholar](https://scholar.google.com/scholar?&q=Ding%2C%20Y.%2C%20Zhang%2C%20X.%2C%20Paxton%2C%20C.%2C%20Zhang%2C%20S.%3A%20Task%20and%20motion%20planning%20with%20large%20language%20models%20for%20object%20rearrangement.%20In%3A%202023%20IEEE%2FRSJ%20International%20Conference%20on%20Intelligent%20Robots%20and%20Systems%20%28IROS%29%2C%20pp.%202086%E2%80%932092.%20IEEE%20%282023%29)

[^13]: Dinges, L., Al-Hamadi, A., Hempel, T., Al Aghbari, Z.: Using facial action recognition to evaluate user perception in aggravated HRC scenarios. In: 2021 12th International Symposium on Image and Signal Processing and Analysis (ISPA), pp. 195–199. IEEE (2021)

[Google Scholar](https://scholar.google.com/scholar?&q=Dinges%2C%20L.%2C%20Al-Hamadi%2C%20A.%2C%20Hempel%2C%20T.%2C%20Al%C2%A0Aghbari%2C%20Z.%3A%20Using%20facial%20action%20recognition%20to%20evaluate%20user%20perception%20in%20aggravated%20HRC%20scenarios.%20In%3A%202021%2012th%20International%20Symposium%20on%20Image%20and%20Signal%20Processing%20and%20Analysis%20%28ISPA%29%2C%20pp.%20195%E2%80%93199.%20IEEE%20%282021%29)

[^14]: Dorbala, V.S., Mullen Jr, J.F., Manocha, D.: Can an embodied agent find your “cat-shaped mug”? LLM-based zero-shot object navigation. IEEE Robot. Autom. Lett. (2023)

[Google Scholar](https://scholar.google.com/scholar?&q=Dorbala%2C%20V.S.%2C%20Mullen%C2%A0Jr%2C%20J.F.%2C%20Manocha%2C%20D.%3A%20Can%20an%20embodied%20agent%20find%20your%20%E2%80%9Ccat-shaped%20mug%E2%80%9D%3F%20LLM-based%20zero-shot%20object%20navigation.%20IEEE%20Robot.%20Autom.%20Lett.%20%282023%29)

[^15]: Fang, H.S., et al.: AlphaPose: whole-body regional multi-person pose estimation and tracking in real-time. IEEE Trans. Pattern Anal. Mach. Intell. **45** (6), 7157–7173 (2022)

[Article](https://doi.org/10.1109%2FTPAMI.2022.3222784) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=AlphaPose%3A%20whole-body%20regional%20multi-person%20pose%20estimation%20and%20tracking%20in%20real-time&journal=IEEE%20Trans.%20Pattern%20Anal.%20Mach.%20Intell.&volume=45&issue=6&pages=7157-7173&publication_year=2022&author=Fang%2CHS)

[^16]: Fikes, R.E., Nilsson, N.J.: STRIPS: a new approach to the application of theorem proving to problem solving. Artif. Intell. **2** (3–4), 189–208 (1971)

[Article](https://doi.org/10.1016%2F0004-3702%2871%2990010-5) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=STRIPS%3A%20a%20new%20approach%20to%20the%20application%20of%20theorem%20proving%20to%20problem%20solving&journal=Artif.%20Intell.&volume=2&issue=3%E2%80%934&pages=189-208&publication_year=1971&author=Fikes%2CRE&author=Nilsson%2CNJ)

[^17]: Gallegos, I.O., et al.: Bias and fairness in large language models: a survey. Comput. Linguist., 1–79 (2024)

[Google Scholar](https://scholar.google.com/scholar?&q=Gallegos%2C%20I.O.%2C%20et%20al.%3A%20Bias%20and%20fairness%20in%20large%20language%20models%3A%20a%20survey.%20Comput.%20Linguist.%2C%201%E2%80%9379%20%282024%29)

[^18]: Gao, Y., et al.: Retrieval-augmented generation for large language models: a survey. arXiv preprint: [arXiv:2312.10997](http://arxiv.org/abs/2312.10997) (2023)

[^19]: Ghzouli, R., Berger, T., Johnsen, E.B., Wasowski, A., Dragule, S.: Behavior trees and state machines in robotics applications. IEEE Trans. Software Eng. **49** (9), 4243–4267 (2023)

[Article](https://doi.org/10.1109%2FTSE.2023.3269081) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Behavior%20trees%20and%20state%20machines%20in%20robotics%20applications&journal=IEEE%20Trans.%20Software%20Eng.&volume=49&issue=9&pages=4243-4267&publication_year=2023&author=Ghzouli%2CR&author=Berger%2CT&author=Johnsen%2CEB&author=Wasowski%2CA&author=Dragule%2CS)

[^20]: Grieskamp, W., Gurevich, Y., Schulte, W., Veanes, M.: Generating finite state machines from abstract state machines. In: Proceedings of the 2002 ACM SIGSOFT international symposium on Software Testing and Analysis, pp. 112–122 (2002)

[Google Scholar](https://scholar.google.com/scholar?&q=Grieskamp%2C%20W.%2C%20Gurevich%2C%20Y.%2C%20Schulte%2C%20W.%2C%20Veanes%2C%20M.%3A%20Generating%20finite%20state%20machines%20from%20abstract%20state%20machines.%20In%3A%20Proceedings%20of%20the%202002%20ACM%20SIGSOFT%20international%20symposium%20on%20Software%20Testing%20and%20Analysis%2C%20pp.%20112%E2%80%93122%20%282002%29)

[^21]: Hempel, T., Jung, M., Abdelrahman, A.A., Al-Hamadi, A.: NITEC: versatile hand-annotated eye contact dataset for ego-vision interaction. In: Proceedings of the IEEE/CVF Winter Conference on Applications of Computer Vision, pp. 4437–4446 (2024)

[Google Scholar](https://scholar.google.com/scholar?&q=Hempel%2C%20T.%2C%20Jung%2C%20M.%2C%20Abdelrahman%2C%20A.A.%2C%20Al-Hamadi%2C%20A.%3A%20NITEC%3A%20versatile%20hand-annotated%20eye%20contact%20dataset%20for%20ego-vision%20interaction.%20In%3A%20Proceedings%20of%20the%20IEEE%2FCVF%20Winter%20Conference%20on%20Applications%20of%20Computer%20Vision%2C%20pp.%204437%E2%80%934446%20%282024%29)

[^22]: Hu, Z., et al.: Deploying and evaluating LLMS to program service mobile robots. IEEE Robot. Autom. Lett. (2024)

[Google Scholar](https://scholar.google.com/scholar?&q=Hu%2C%20Z.%2C%20et%20al.%3A%20Deploying%20and%20evaluating%20LLMS%20to%20program%20service%20mobile%20robots.%20IEEE%20Robot.%20Autom.%20Lett.%20%282024%29)

[^23]: Huang, W., Abbeel, P., Pathak, D., Mordatch, I.: Language models as zero-shot planners: extracting actionable knowledge for embodied agents. In: International Conference on Machine Learning, pp. 9118–9147. PMLR (2022)

[Google Scholar](https://scholar.google.com/scholar?&q=Huang%2C%20W.%2C%20Abbeel%2C%20P.%2C%20Pathak%2C%20D.%2C%20Mordatch%2C%20I.%3A%20Language%20models%20as%20zero-shot%20planners%3A%20extracting%20actionable%20knowledge%20for%20embodied%20agents.%20In%3A%20International%20Conference%20on%20Machine%20Learning%2C%20pp.%209118%E2%80%939147.%20PMLR%20%282022%29)

[^24]: Hugel, V., Amouroux, G., Costis, T., Bonnin, P., Blazevic, P.: Specifications and design of graphical interface for hierarchical finite state machines. In: RoboCup 2005: Robot Soccer World Cup IX 9, pp. 648–655. Springer (2006)

[Google Scholar](https://scholar.google.com/scholar?&q=Hugel%2C%20V.%2C%20Amouroux%2C%20G.%2C%20Costis%2C%20T.%2C%20Bonnin%2C%20P.%2C%20Blazevic%2C%20P.%3A%20Specifications%20and%20design%20of%20graphical%20interface%20for%20hierarchical%20finite%20state%20machines.%20In%3A%20RoboCup%202005%3A%20Robot%20Soccer%20World%20Cup%20IX%209%2C%20pp.%20648%E2%80%93655.%20Springer%20%282006%29)

[^25]: Jiang, Y.Q., Zhang, S.Q., Khandelwal, P., Stone, P.: Task planning in robotics: an empirical comparison of PDDL-and asp-based systems. Front. Inf. Technol. Electr. Eng. **20**, 363–373 (2019)

[Google Scholar](https://scholar.google.com/scholar?&q=Jiang%2C%20Y.Q.%2C%20Zhang%2C%20S.Q.%2C%20Khandelwal%2C%20P.%2C%20Stone%2C%20P.%3A%20Task%20planning%20in%20robotics%3A%20an%20empirical%20comparison%20of%20PDDL-and%20asp-based%20systems.%20Front.%20Inf.%20Technol.%20Electr.%20Eng.%2020%2C%20363%E2%80%93373%20%282019%29)

[^26]: Kaelbling, L.P., Littman, M.L., Cassandra, A.R.: Planning and acting in partially observable stochastic domains. Artif. Intell. **101** (1–2), 99–134 (1998)

[Article](https://doi.org/10.1016%2FS0004-3702%2898%2900023-X) [MathSciNet](http://www.ams.org/mathscinet-getitem?mr=1641530) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Planning%20and%20acting%20in%20partially%20observable%20stochastic%20domains&journal=Artif.%20Intell.&volume=101&issue=1%E2%80%932&pages=99-134&publication_year=1998&author=Kaelbling%2CLP&author=Littman%2CML&author=Cassandra%2CAR)

[^27]: Kannan, S.S., Venkatesh, V.L., Min, B.C.: Smart-LLM: smart multi-agent robot task planning using large language models. arXiv preprint: [arXiv:2309.10062](http://arxiv.org/abs/2309.10062) (2023)

[^28]: Karli, U.B., Chen, J.T., Antony, V.N., Huang, C.M.: Alchemist: LLM-aided end-user development of robot applications. In: Proceedings of the 2024 ACM/IEEE International Conference on Human-Robot Interaction, pp. 361–370 (2024)

[Google Scholar](https://scholar.google.com/scholar?&q=Karli%2C%20U.B.%2C%20Chen%2C%20J.T.%2C%20Antony%2C%20V.N.%2C%20Huang%2C%20C.M.%3A%20Alchemist%3A%20LLM-aided%20end-user%20development%20of%20robot%20applications.%20In%3A%20Proceedings%20of%20the%202024%20ACM%2FIEEE%20International%20Conference%20on%20Human-Robot%20Interaction%2C%20pp.%20361%E2%80%93370%20%282024%29)

[^29]: Khalifa, A., Abdelrahman, A.A., Strazdas, D., Hintz, J., Hempel, T., Al-Hamadi, A.: Face recognition and tracking framework for human-robot interaction. Appl. Sci. **12** (11), 5568 (2022)

[Article](https://doi.org/10.3390%2Fapp12115568) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Face%20recognition%20and%20tracking%20framework%20for%20human-robot%20interaction&journal=Appl.%20Sci.&volume=12&issue=11&publication_year=2022&author=Khalifa%2CA&author=Abdelrahman%2CAA&author=Strazdas%2CD&author=Hintz%2CJ&author=Hempel%2CT&author=Al-Hamadi%2CA)

[^30]: Kong, X., Zhang, W., Hong, J., Braunl, T.: Embodied AI in mobile robots: coverage path planning with large language models. arXiv preprint: [arXiv:2407.02220](http://arxiv.org/abs/2407.02220) (2024)

[^31]: Korteling, J.H., van de Boer-Visschedijk, G.C., Blankendaal, R.A., Boonekamp, R.C., Eikelboom, A.R.: Human-versus artificial intelligence. Front. Artif. Intell. **4**, 622364 (2021)

[Article](https://doi.org/10.3389%2Ffrai.2021.622364) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Human-versus%20artificial%20intelligence&journal=Front.%20Artif.%20Intell.&volume=4&publication_year=2021&author=Korteling%2CJH&author=Boer-Visschedijk%2CGC&author=Blankendaal%2CRA&author=Boonekamp%2CRC&author=Eikelboom%2CAR)

[^32]: Kwon, M., Jung, M.F., Knepper, R.A.: Human expectations of social robots. In: 2016 11th ACM/IEEE International Conference on Human-Robot Interaction (HRI), pp. 463–464. IEEE (2016)

[Google Scholar](https://scholar.google.com/scholar?&q=Kwon%2C%20M.%2C%20Jung%2C%20M.F.%2C%20Knepper%2C%20R.A.%3A%20Human%20expectations%20of%20social%20robots.%20In%3A%202016%2011th%20ACM%2FIEEE%20International%20Conference%20on%20Human-Robot%20Interaction%20%28HRI%29%2C%20pp.%20463%E2%80%93464.%20IEEE%20%282016%29)

[^33]: Latif, E.: 3P-LLM: probabilistic path planning using large language model for autonomous robot navigation. arXiv preprint: [arXiv:2403.18778](http://arxiv.org/abs/2403.18778) (2024)

[^34]: Lee, D., Yannakakis, M.: Principles and methods of testing finite state machines-a survey. Proc. IEEE **84** (8), 1090–1123 (1996)

[Article](https://doi.org/10.1109%2F5.533956) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Principles%20and%20methods%20of%20testing%20finite%20state%20machines-a%20survey&journal=Proc.%20IEEE&volume=84&issue=8&pages=1090-1123&publication_year=1996&author=Lee%2CD&author=Yannakakis%2CM)

[^35]: Lewis, P., et al.: Retrieval-augmented generation for knowledge-intensive NLP tasks. In: Advances in Neural Information Processing Systems, vol. 33, 9459–9474 (2020)

[Google Scholar](https://scholar.google.com/scholar?&q=Lewis%2C%20P.%2C%20et%20al.%3A%20Retrieval-augmented%20generation%20for%20knowledge-intensive%20NLP%20tasks.%20In%3A%20Advances%20in%20Neural%20Information%20Processing%20Systems%2C%20vol.%2033%2C%209459%E2%80%939474%20%282020%29)

[^36]: Li, L.H., et al.: Grounded language-image pre-training. In: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pp. 10965–10975 (2022)

[Google Scholar](https://scholar.google.com/scholar?&q=Li%2C%20L.H.%2C%20et%C2%A0al.%3A%20Grounded%20language-image%20pre-training.%20In%3A%20Proceedings%20of%20the%20IEEE%2FCVF%20Conference%20on%20Computer%20Vision%20and%20Pattern%20Recognition%2C%20pp.%2010965%E2%80%9310975%20%282022%29)

[^37]: Li, X., et al.: FLM-101B: An open LLM and how to train it with \\$100 K budget. arXiv preprint: [arXiv:2309.03852](http://arxiv.org/abs/2309.03852) (2023)

[^38]: Liang, J., et al.: Code as policies: language model programs for embodied control. In: 2023 IEEE International Conference on Robotics and Automation (ICRA), pp. 9493–9500. IEEE (2023)

[Google Scholar](https://scholar.google.com/scholar?&q=Liang%2C%20J.%2C%20et%20al.%3A%20Code%20as%20policies%3A%20language%20model%20programs%20for%20embodied%20control.%20In%3A%202023%20IEEE%20International%20Conference%20on%20Robotics%20and%20Automation%20%28ICRA%29%2C%20pp.%209493%E2%80%939500.%20IEEE%20%282023%29)

[^39]: Liu, H., et al.: Enhancing the LLM-based robot manipulation through human-robot collaboration. arXiv preprint: [arXiv:2406.14097](http://arxiv.org/abs/2406.14097) (2024)

[^40]: Liu, P., Orru, Y., Paxton, C., Shafiullah, N.M.M., Pinto, L.: Ok-robot: What really matters in integrating open-knowledge models for robotics. arXiv preprint: [arXiv:2401.12202](http://arxiv.org/abs/2401.12202) (2024)

[^41]: Liu, Y., et al.: Prompt injection attack against LLM-integrated applications. arXiv preprint: [arXiv:2306.05499](http://arxiv.org/abs/2306.05499) (2023)

[^42]: Madani, K., Rinaldi, A.M., Russo, C., Tommasino, C.: A combined approach for improving humanoid robots autonomous cognitive capabilities. Knowl. Inf. Syst. **65** (8), 3197–3221 (2023)

[Article](https://link.springer.com/doi/10.1007/s10115-023-01844-3) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=A%20combined%20approach%20for%20improving%20humanoid%20robots%20autonomous%20cognitive%20capabilities&journal=Knowl.%20Inf.%20Syst.&volume=65&issue=8&pages=3197-3221&publication_year=2023&author=Madani%2CK&author=Rinaldi%2CAM&author=Russo%2CC&author=Tommasino%2CC)

[^43]: Maslej, N., et al.: Artificial intelligence index report 2023. arXiv preprint: [arXiv:2310.03715](http://arxiv.org/abs/2310.03715) (2023)

[^44]: Mower, C.E., et al.: ROS-LLM: A ROS framework for embodied AI with task feedback and structured reasoning. arXiv preprint: [arXiv:2406.19741](http://arxiv.org/abs/2406.19741) (2024)

[^45]: Naveed, H., et al.: A comprehensive overview of large language models. arXiv preprint: [arXiv:2307.06435](http://arxiv.org/abs/2307.06435) (2023)

[^46]: Nazir, A., Wani, M.A.: You only look once-object detection models: a review. In: 2023 10th International Conference on Computing for Sustainable Global Development (INDIACom), pp. 1088–1095. IEEE (2023)

[Google Scholar](https://scholar.google.com/scholar?&q=Nazir%2C%20A.%2C%20Wani%2C%20M.A.%3A%20You%20only%20look%20once-object%20detection%20models%3A%20a%20review.%20In%3A%202023%2010th%20International%20Conference%20on%20Computing%20for%20Sustainable%20Global%20Development%20%28INDIACom%29%2C%20pp.%201088%E2%80%931095.%20IEEE%20%282023%29)

[^47]: Obinata, Y., et al.: Foundation model based open vocabulary task planning and executive system for general purpose service robots. arXiv preprint: [arXiv:2308.03357](http://arxiv.org/abs/2308.03357) (2023)

[^48]: OpenAI, Achiam, J., et al.: GPT-4 technical report. arXiv preprint: [arXiv:2303.08774](http://arxiv.org/abs/2303.08774) (2024)

[^49]: O’Regan, G., O’Regan, G.: Automata theory. guide to discrete mathematics: an accessible introduction to the history, theory, logic and applications, pp. 121–131 (2021)

[Google Scholar](https://scholar.google.com/scholar?&q=O%E2%80%99Regan%2C%20G.%2C%20O%E2%80%99Regan%2C%20G.%3A%20Automata%20theory.%20guide%20to%20discrete%20mathematics%3A%20an%20accessible%20introduction%20to%20the%20history%2C%20theory%2C%20logic%20and%20applications%2C%20pp.%20121%E2%80%93131%20%282021%29)

[^50]: Ouyang, A.: Understanding the performance of transformer inference. Ph.D. thesis, Massachusetts Institute of Technology (2023)

[Google Scholar](https://scholar.google.com/scholar?&q=Ouyang%2C%20A.%3A%20Understanding%20the%20performance%20of%20transformer%20inference.%20Ph.D.%20thesis%2C%20Massachusetts%20Institute%20of%20Technology%20%282023%29)

[^51]: Ouyang, L., et al.: Training language models to follow instructions with human feedback. In: Advances in Neural Information Processing Systems, vol. 35, pp. 27730–27744 (2022)

[Google Scholar](https://scholar.google.com/scholar?&q=Ouyang%2C%20L.%2C%20et%20al.%3A%20Training%20language%20models%20to%20follow%20instructions%20with%20human%20feedback.%20In%3A%20Advances%20in%20Neural%20Information%20Processing%20Systems%2C%20vol.%2035%2C%20pp.%2027730%E2%80%9327744%20%282022%29)

[^52]: Radford, A., et al.: Learning transferable visual models from natural language supervision. In: International Conference on Machine Learning, pp. 8748–8763. PMLR (2021)

[Google Scholar](https://scholar.google.com/scholar?&q=Radford%2C%20A.%2C%20et%C2%A0al.%3A%20Learning%20transferable%20visual%20models%20from%20natural%20language%20supervision.%20In%3A%20International%20Conference%20on%20Machine%20Learning%2C%20pp.%208748%E2%80%938763.%20PMLR%20%282021%29)

[^53]: Saveriano, M., Abu-Dakka, F.J., Kramberger, A., Peternel, L.: Dynamic movement primitives in robotics: a tutorial survey. Int. J. Robot. Res. **42** (13), 1133–1184 (2023)

[Article](https://doi.org/10.1177%2F02783649231201196) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Dynamic%20movement%20primitives%20in%20robotics%3A%20a%20tutorial%20survey&journal=Int.%20J.%20Robot.%20Res.&volume=42&issue=13&pages=1133-1184&publication_year=2023&author=Saveriano%2CM&author=Abu-Dakka%2CFJ&author=Kramberger%2CA&author=Peternel%2CL)

[^54]: Schumann, R., Zhu, W., Feng, W., Fu, T.J., Riezler, S., Wang, W.Y.: VELMA: verbalization embodiment of LLM agents for vision and language navigation in street view. In: Proceedings of the AAAI Conference on Artificial Intelligence, vol. 38, pp. 18924–18933 (2024)

[Google Scholar](https://scholar.google.com/scholar?&q=Schumann%2C%20R.%2C%20Zhu%2C%20W.%2C%20Feng%2C%20W.%2C%20Fu%2C%20T.J.%2C%20Riezler%2C%20S.%2C%20Wang%2C%20W.Y.%3A%20VELMA%3A%20verbalization%20embodiment%20of%20LLM%20agents%20for%20vision%20and%20language%20navigation%20in%20street%20view.%20In%3A%20Proceedings%20of%20the%20AAAI%20Conference%20on%20Artificial%20Intelligence%2C%20vol.%C2%A038%2C%20pp.%2018924%E2%80%9318933%20%282024%29)

[^55]: Shi, F., et al.: Large language models can be easily distracted by irrelevant context. In: International Conference on Machine Learning, pp. 31210–31227. PMLR (2023)

[Google Scholar](https://scholar.google.com/scholar?&q=Shi%2C%20F.%2C%20et%20al.%3A%20Large%20language%20models%20can%20be%20easily%20distracted%20by%20irrelevant%20context.%20In%3A%20International%20Conference%20on%20Machine%20Learning%2C%20pp.%2031210%E2%80%9331227.%20PMLR%20%282023%29)

[^56]: Shi, Y., Liu, J., Zheng, X.: LFENav: LLM-based frontiers exploration for visual semantic navigation. In: IFIP International Conference on Artificial Intelligence Applications and Innovations, pp. 375–388. Springer (2024)

[Google Scholar](https://scholar.google.com/scholar?&q=Shi%2C%20Y.%2C%20Liu%2C%20J.%2C%20Zheng%2C%20X.%3A%20LFENav%3A%20LLM-based%20frontiers%20exploration%20for%20visual%20semantic%20navigation.%20In%3A%20IFIP%20International%20Conference%20on%20Artificial%20Intelligence%20Applications%20and%20Innovations%2C%20pp.%20375%E2%80%93388.%20Springer%20%282024%29)

[^57]: Sikorski, P., et al.: Deployment of NLP and LLM techniques to control mobile robots at the edge: a case study using GPT-4-turbo and LLaMA 2. arXiv preprint: [arXiv:2405.17670](http://arxiv.org/abs/2405.17670) (2024)

[^58]: Singh, I., et al.: ProgPrompt: generating situated robot task plans using large language models. In: 2023 IEEE International Conference on Robotics and Automation (ICRA), pp. 11523–11530. IEEE (2023)

[Google Scholar](https://scholar.google.com/scholar?&q=Singh%2C%20I.%2C%20et%20al.%3A%20ProgPrompt%3A%20generating%20situated%20robot%20task%20plans%20using%20large%20language%20models.%20In%3A%202023%20IEEE%20International%20Conference%20on%20Robotics%20and%20Automation%20%28ICRA%29%2C%20pp.%2011523%E2%80%9311530.%20IEEE%20%282023%29)

[^59]: Tian, L., Ellis, K., Kryven, M., Tenenbaum, J.: Learning abstract structure for drawing by efficient motor program induction. In: Advances in Neural Information Processing Systems, vol. 33, 2686–2697 (2020)

[Google Scholar](https://scholar.google.com/scholar?&q=Tian%2C%20L.%2C%20Ellis%2C%20K.%2C%20Kryven%2C%20M.%2C%20Tenenbaum%2C%20J.%3A%20Learning%20abstract%20structure%20for%20drawing%20by%20efficient%20motor%20program%20induction.%20In%3A%20Advances%20in%20Neural%20Information%20Processing%20Systems%2C%20vol.%2033%2C%202686%E2%80%932697%20%282020%29)

[^60]: Touvron, H., et al.: LLaMA: open and efficient foundation language models. arXiv preprint: [arXiv:2302.13971](http://arxiv.org/abs/2302.13971) (2023)

[^61]: Tseng, Y.M., et al.: Two tales of persona in LLMS: a survey of role-playing and personalization. arXiv preprint: [arXiv:2406.01171](http://arxiv.org/abs/2406.01171) (2024)

[^62]: Wang, R., Yang, Z., Zhao, Z., Tong, X., Hong, Z., Qian, K.: LLM-based robot task planning with exceptional handling for general purpose service robots. arXiv preprint: [arXiv:2405.15646](http://arxiv.org/abs/2405.15646) (2024)

[^63]: Wang, T., Fan, J., Zheng, P.: An LLM-based vision and language cobot navigation approach for human-centric smart manufacturing. J. Manuf. Syst. (2024)

[Google Scholar](https://scholar.google.com/scholar?&q=Wang%2C%20T.%2C%20Fan%2C%20J.%2C%20Zheng%2C%20P.%3A%20An%20LLM-based%20vision%20and%20language%20cobot%20navigation%20approach%20for%20human-centric%20smart%20manufacturing.%20J.%20Manuf.%20Syst.%20%282024%29)

[^64]: Wu, P., Mu, Y., Zhou, K., Ma, J., Chen, J., Liu, C.: CAMON: cooperative agents for multi-object navigation with LLM-based conversations. arXiv preprint: [arXiv:2407.00632](http://arxiv.org/abs/2407.00632) (2024)

[^65]: Wu, S., Fei, H., Qu, L., Ji, W., Chua, T.S.: Next-GPT: any-to-any multimodal LLM. arXiv preprint: [arXiv:2309.05519](http://arxiv.org/abs/2309.05519) (2023)

[^66]: Wu, Y., Yue, T., Zhang, S., Wang, C., Wu, Q.: StateFlow: enhancing LLM task-solving through state-driven workflows. arXiv preprint: [arXiv:2403.11322](http://arxiv.org/abs/2403.11322) (2024)

[^67]: Zadeh, L.A.: Fuzzy sets. Inf. Control **8** (3), 338–353 (1965)

[Article](https://doi.org/10.1016%2FS0019-9958%2865%2990241-X) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Fuzzy%20sets&journal=Inf.%20Control&volume=8&issue=3&pages=338-353&publication_year=1965&author=Zadeh%2CLA)

[^68]: Zhang, Z., Fang, M., Chen, L., Namazi-Rad, M.R., Wang, J.: How do large language models capture the ever-changing world knowledge? a review of recent advances. arXiv preprint: [arXiv:2310.07343](http://arxiv.org/abs/2310.07343) (2023)

[^69]: Zheng, L., et al.: Judging LLM-as-a-judge with MT-bench and chatbot arena. In: Advances in Neural Information Processing Systems, vol. 36, pp. 46595–46623 (2023)

[Google Scholar](https://scholar.google.com/scholar?&q=Zheng%2C%20L.%2C%20et%20al.%3A%20Judging%20LLM-as-a-judge%20with%20MT-bench%20and%20chatbot%20arena.%20In%3A%20Advances%20in%20Neural%20Information%20Processing%20Systems%2C%20vol.%2036%2C%20pp.%2046595%E2%80%9346623%20%282023%29)

[^70]: Zheng, Z., Feng, Q., Li, H., Knoll, A., Feng, J.: Evaluating uncertainty-based failure detection for closed-loop LLM planners. arXiv preprint: [arXiv:2406.00430](http://arxiv.org/abs/2406.00430) (2024)

[^71]: Zhou, H., Lin, Y., Yan, L., Zhu, J., Min, H.: LLM-BT: performing robotic adaptive tasks based on large language models and behavior trees. arXiv preprint: [arxiv:2404.05134](http://arxiv.org/abs/2404.05134) (2024)

[^72]: Zhou, P., et al.: A survey on generative AI and LLM for video generation, understanding, and streaming. arXiv preprint: [arXiv:2404.16038](http://arxiv.org/abs/2404.16038) (2024)

[^73]: Zhu, Y., et al.: Large language models for information retrieval: a survey. arXiv preprint: [arXiv:2308.07107](http://arxiv.org/abs/2308.07107) (2023)