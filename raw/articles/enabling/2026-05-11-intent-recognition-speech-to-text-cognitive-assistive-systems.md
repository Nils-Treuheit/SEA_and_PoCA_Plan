# Original

---
title: "Intent Recognition in Speech-to-Text Processing in the Context of Natural Interaction with Cognitive Assistive Systems"
description: |
  This study investigates efficient speech-to-intent recognition for human–robot interaction in elderly-care environments in German, targeting deployment on resource-constrained platforms such as the Jetson AGX Orin. Two pipelines are compared: a modular ASR+LLM system and an end-to-end Large Audio-Language Model (LALM) approach.
background: /assets/theme/images/lrec2026.png
image: /assets/theme/images/lrec2026.png
author: "Behnam Ensan, Magnus Jung, Matthias Busch, Andreas Wendemuth"
categories: [Publication, Conference Paper, Speech Recognition, Human-Robot Interaction]
conference: "**LREC 2026**"
tags: [Conference, LREC, Publication, NLP, Speech, HRI]
---

## Intent Recognition in Speech-to-Text Processing in the Context of Natural Interaction with Cognitive Assistive Systems

This study investigates efficient speech-to-intent recognition for human–robot interaction in elderly-care environments in German, targeting deployment on resource-constrained platforms such as the Jetson AGX Orin. To benchmark performance, we created a domain-specific German dataset with two sub-datasets (PaSID and PaSynTex) that simulate specific nursing home communication scenarios.

Two alternative speech-to-intent pipelines were developed and evaluated: a two-stage system combining automatic speech recognition (ASR) with a large language model (LLM), and an end-to-end large audio–language model (LALM) architecture. The performance of Whisper-based ASR systems was evaluated across a wide variety of LLMs and several LALMs, comparing intent-classification accuracy, latency, and resource efficiency.

### Datasets

Two custom datasets were created for evaluation:

- **PaSID** (Patient Speech for Intent Detection): a dataset comprising one hour of German speech and 348 samples based on predefined scenarios simulating interactions in a nursing-home setting. Each sample is annotated with the corresponding intent and slot labels.
- **PaSynTex** (Patient Synthetic Text Expressions): a dataset of 300 synthetic German sentences, ranging from simple to implicit formulations, for intent classification and slot extraction.

Both datasets cover four intent categories: *Assistance Request*, *Medication Reminder*, *Information Query*, and *Emergency Alert*.

### Key Findings

The results indicate that optimized ASR + LLM configurations, particularly **Whisper Turbo** coupled with **Phi-3.5-mini** or **Qwen2.5-7B**, outperform unified LALM approaches while maintaining substantially lower memory and inference costs. Although unified LALM models outperform the two-step ASR+LLM integration in the same configuration, this comes at the cost of approximately 60% higher GPU memory usage, likely due to limited optimization for edge deployment.

Overall, the findings provide initial evidence that modular ASR+LLM pipelines provide a more practical solution for real-time, on-device intent recognition in assistive robotics in German, offering an effective trade-off between performance and deployability on resource-constrained platforms.

---

# AI Interpretation

---
title: "Intent Recognition in Speech-to-Text Processing in the Context of Natural Interaction with Cognitive Assistive Systems"
description: |
  This study investigates efficient speech-to-intent recognition for human–robot interaction in elderly-care environments in German, targeting deployment on resource-constrained platforms such as the Jetson AGX Orin. Two pipelines are compared: a modular ASR+LLM system and an end-to-end Large Audio-Language Model (LALM) approach.
background: /assets/theme/images/lrec2026.png
image: /assets/theme/images/lrec2026.png
author: "Behnam Ensan, Magnus Jung, Matthias Busch, Andreas Wendemuth"
categories: [Publication, Conference Paper, Speech Recognition, Human-Robot Interaction]
conference: "**LREC 2026**"
tags: [Conference, LREC, Publication, NLP, Speech, HRI]
---

## Intent Recognition in Speech-to-Text Processing in the Context of Natural Interaction with Cognitive Assistive Systems

This study investigates efficient speech-to-intent recognition for human–robot interaction in elderly-care environments in German, targeting deployment on resource-constrained platforms such as the Jetson AGX Orin. To benchmark performance, we created a domain-specific German dataset with two sub-datasets (PaSID and PaSynTex) that simulate specific nursing home communication scenarios.

Two alternative speech-to-intent pipelines were developed and evaluated: a two-stage system combining automatic speech recognition (ASR) with a large language model (LLM), and an end-to-end large audio–language model (LALM) architecture. The performance of Whisper-based ASR systems was evaluated across a wide variety of LLMs and several LALMs, comparing intent-classification accuracy, latency, and resource efficiency.

### Datasets

Two custom datasets were created for evaluation:

- **PaSID** (Patient Speech for Intent Detection): a dataset comprising one hour of German speech and 348 samples based on predefined scenarios simulating interactions in a nursing-home setting. Each sample is annotated with the corresponding intent and slot labels.
- **PaSynTex** (Patient Synthetic Text Expressions): a dataset of 300 synthetic German sentences, ranging from simple to implicit formulations, for intent classification and slot extraction.

Both datasets cover four intent categories: *Assistance Request*, *Medication Reminder*, *Information Query*, and *Emergency Alert*.

### Key Findings

The results indicate that optimized ASR + LLM configurations, particularly **Whisper Turbo** coupled with **Phi-3.5-mini** or **Qwen2.5-7B**, outperform unified LALM approaches while maintaining substantially lower memory and inference costs. Although unified LALM models outperform the two-step ASR+LLM integration in the same configuration, this comes at the cost of approximately 60% higher GPU memory usage, likely due to limited optimization for edge deployment.

Overall, the findings provide initial evidence that modular ASR+LLM pipelines provide a more practical solution for real-time, on-device intent recognition in assistive robotics in German, offering an effective trade-off between performance and deployability on resource-constrained platforms.

---

## Fulltext Access

[http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.793.pdf](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.793.pdf)


## Citation (BibTeX)

```bibtex
@inproceedings{ensan2026intent,
  title     = {Intent Recognition in Speech-to-Text Processing in the Context of Natural Interaction with Cognitive Assistive Systems},
  author    = {Ensan, Behnam and Jung, Magnus and Busch, Matthias and Wendemuth, Andreas},
  booktitle = {Proceedings of the Fifteenth Language Resources and Evaluation Conference (LREC 2026)},
  pages     = {10102--10113},
  year      = {2026},
  month     = may,
  address   = {Torino, Italy},
  publisher = {ELRA Language Resources Association (ELRA)}
}
```
```