# Original

---
title: "Think Like a Team: Graph-Based Representation of Shared Mental Models in Human-Agent Collaboration"
description: |
  We introduce a conceptual temporal Shared Mental Model (SMM) that maps changing human-human and human-agent team dynamics using temporal graph neural networks (TGNNs), allowing Human-Agent Teaming Systems (HATs) to achieve shared teaming goals.
background: /assets/theme/images/essv2026.png   
image: /assets/theme/images/essv2026.png
author: "Moinam Chatterjee, Behnam Ensan, Andreas Wendemuth, Ayoub Al-Hamadi"
categories: [Publication, Conference Paper, Human-Agent Teaming, Graph Neural Networks]
conference: "**ESSV 2026**"
tags: [Conference, ESSV, Publication, HAT, SMM, Graph Neural Networks]
---

## Think Like a Team: Graph-Based Representation of Shared Mental Models in Human-Agent Collaboration

We introduce a conceptual temporal Shared Mental Model (SMM) that maps changing human-human and human-agent team dynamics, allowing Human-Agent Teaming Systems (HATs) to achieve shared teaming goals. We use temporal graph neural networks (TGNNs) to capture the evolving roles and tasks within the team, learnt from time-stamped team interactions.

Current HATs struggle to adapt dynamically to evolving goals and perceived user roles. When goals change rapidly, humans and AI agents fail to maintain their shared mental models. This work addresses this gap by proposing a framework that enables AI agents to maintain shared cognition in real time, enabling them to continuously update and synchronise their mental models.

### Approach

The proposed temporal SMM represents each team member's cognitive state via four parameters:

- **Team Role**: a predefined, formal role fixed throughout the meeting (e.g., Program Manager, UI Designer)
- **Social Role**: an informal role that evolves with team dynamics (Gatekeeper, Protagonist, Supporter, Neutral, Attacker)
- **Intention**: the underlying dialogue act (Inform, Assess, Suggest, Offer)
- **Alignment State**: whether the member is aligned or misaligned with the rest of the team

These mental states are modelled as nodes in a temporal graph, where edges carry an *alignment valence* (AV) score in the range [−1, 1], computed as a weighted combination of normalised social role and alignment state values. The graph structure evolves over time via TGNNs, capturing both synchronisation (alignment) and divergence (misalignment) among team members.

### User Study

A proof-of-concept exploratory user study was conducted using audio clips from the AMI Meeting Corpus. Participants rated speaker alignment states on a 5-point scale. The simulation demonstrated the evolution of team dynamics and interpersonal relationships across eight timestamps, showing how alignment valence and social roles shift throughout a collaborative meeting scenario.

Graph-level outputs at each timestamp can be used to compute a team cohesion index, generate trust calibration signals, or trigger adaptive teaming strategies — laying the foundation for affect-aware, adaptive teaming beyond static role assignment.

---

# AI Interpretation

---
title: "Think Like a Team: Graph-Based Representation of Shared Mental Models in Human-Agent Collaboration"
description: |
  We introduce a conceptual temporal Shared Mental Model (SMM) that maps changing human-human and human-agent team dynamics using temporal graph neural networks (TGNNs), allowing Human-Agent Teaming Systems (HATs) to achieve shared teaming goals.
background: /assets/theme/images/essv2026.png   
image: /assets/theme/images/essv2026.png
author: "Moinam Chatterjee, Behnam Ensan, Andreas Wendemuth, Ayoub Al-Hamadi"
categories: [Publication, Conference Paper, Human-Agent Teaming, Graph Neural Networks]
conference: "**ESSV 2026**"
tags: [Conference, ESSV, Publication, HAT, SMM, Graph Neural Networks]
---

## Think Like a Team: Graph-Based Representation of Shared Mental Models in Human-Agent Collaboration

We introduce a conceptual temporal Shared Mental Model (SMM) that maps changing human-human and human-agent team dynamics, allowing Human-Agent Teaming Systems (HATs) to achieve shared teaming goals. We use temporal graph neural networks (TGNNs) to capture the evolving roles and tasks within the team, learnt from time-stamped team interactions.

Current HATs struggle to adapt dynamically to evolving goals and perceived user roles. When goals change rapidly, humans and AI agents fail to maintain their shared mental models. This work addresses this gap by proposing a framework that enables AI agents to maintain shared cognition in real time, enabling them to continuously update and synchronise their mental models.

### Approach

The proposed temporal SMM represents each team member's cognitive state via four parameters:

- **Team Role**: a predefined, formal role fixed throughout the meeting (e.g., Program Manager, UI Designer)
- **Social Role**: an informal role that evolves with team dynamics (Gatekeeper, Protagonist, Supporter, Neutral, Attacker)
- **Intention**: the underlying dialogue act (Inform, Assess, Suggest, Offer)
- **Alignment State**: whether the member is aligned or misaligned with the rest of the team

These mental states are modelled as nodes in a temporal graph, where edges carry an *alignment valence* (AV) score in the range [−1, 1], computed as a weighted combination of normalised social role and alignment state values. The graph structure evolves over time via TGNNs, capturing both synchronisation (alignment) and divergence (misalignment) among team members.

### User Study

A proof-of-concept exploratory user study was conducted using audio clips from the AMI Meeting Corpus. Participants rated speaker alignment states on a 5-point scale. The simulation demonstrated the evolution of team dynamics and interpersonal relationships across eight timestamps, showing how alignment valence and social roles shift throughout a collaborative meeting scenario.

Graph-level outputs at each timestamp can be used to compute a team cohesion index, generate trust calibration signals, or trigger adaptive teaming strategies — laying the foundation for affect-aware, adaptive teaming beyond static role assignment.

---

## Fulltext Access

[https://www.essv.de/pdf/2026_151_158.pdf](https://www.essv.de/pdf/2026_151_158.pdf)

## Citation (BibTeX)

```bibtex
@inproceedings{chatterjee2026think,
  title     = {Think Like a Team: Graph-Based Representation of Shared Mental Models in Human-Agent Collaboration},
  author    = {Chatterjee, Moinam and Ensan, Behnam and Wendemuth, Andreas and Al-Hamadi, Ayoub},
  booktitle = {Elektronische Sprachsignalverarbeitung (ESSV 2026)},
  pages     = {151--158},
  year      = {2026},
  month     = mar,
  address   = {Eichst{\"a}tt, Germany}
}
```