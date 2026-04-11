<div align="center">

# A Survey on Hallucination in Video Understanding: Taxonomy, Causes, and Mitigation Techniques

**Jiayi Sheng**$^{1,2}$, **Wei Luo**$^{1}$, **Wotao Yin**$^{1}$

$^1$ Alibaba Group US DAMO Academy &nbsp;&nbsp; $^2$ Carnegie Mellon University

[[Paper](paper.pdf)]

</div>

# Abstract

Video Large Language Models (Vid-LLMs) have recently achieved strong performance across a wide range of video understanding tasks, including question answering, captioning, and multimodal reasoning. However, these models frequently produce outputs that are not faithfully grounded in the underlying video content, a phenomenon commonly referred to as hallucination. Compared with hallucination in text-only or image-based models, hallucination in video understanding is further complicated by temporal dynamics, motion interpretation, long-context dependencies, and event-level reasoning. In this survey, we present a comprehensive review of hallucination in Vid-LLMs. We begin with a unified taxonomy, which categorizes different hallucination phenomena, and then organize existing mitigation strategies according to the failure mechanisms they address. We also discuss key open challenges and outline some promising research directions.


# Overview

Video understanding has emerged as a core task for modern AI systems, which are now deployed in high-stakes settings including autonomous driving, surveillance, human-robot interaction, and content moderation. In these settings, hallucination — generating outputs not faithfully grounded in the video content — poses direct safety, legal, and ethical risks. Rather than a modeling imperfection, hallucination in video understanding is a system-level reliability problem.

Prior work has established hallucination as a well-defined problem for text-only LLMs and image-based VLMs. Extending these frameworks to video is non-trivial. Vid-LLMs must reason over temporal order, motion patterns, long-range dependencies, and cross-modal signals — requirements that introduce failure modes with no counterpart in static settings. A model may correctly recognize objects in individual frames yet hallucinate how they move, interact, or change over time. It may invent actions, misorder events, attribute wrong causality, or lose track of earlier evidence when reasoning over long videos. These failures are further complicated by the entanglement of perceptual, temporal, and reasoning errors.

Despite rapid progress in Vid-LLMs, their hallucinations have not been systematically reviewed. This survey fills that gap with three contributions:

- **Comprehensive taxonomy of video understanding hallucination.** Six categories of hallucination, each grounded in characteristic error patterns specific to video.

- **Systematic categorization of mitigation techniques.** Mitigation methods organized by the hallucination category they address, connecting each failure type to targeted remedies.

- **Forward-looking research directions.** Open challenges and promising directions toward more reliable Vid-LLMs.


# Hallucination Taxonomy

To systematically analyze failure modes in Vid-LLMs, we organize hallucination phenomena into a unified six-category taxonomy. Each category captures a distinct class of reliability failures that can arise even when other aspects of video perception remain accurate.

We begin with two concrete examples that ground each category in observable model behavior before presenting the full taxonomy.

## Example 1: Tea-Making Scenario

The figure below uses a simplified tea-making scenario with a timeline of six key moments (T1–T6) to illustrate all six hallucination categories in a controlled setting. 

![Tea Hallucination](figure/hallucination_illustration_tea.jpg)


## Example 2: Real-World Scenario

The same six hallucination categories reappear in the real-world grocery self-checkout scenario below. 
![Grocery Hallucination](figure/hallucination_illustration_grocery.jpg)


## Unified Hallucination Taxonomy

With these examples in mind, we now present the unified hallucination taxonomy. As illustrated below, we identify six major categories of hallucination in video understanding, organized by the type of failure each represents.

![Hallucination Taxonomy](figure/hallucination_taxonomy_overview.png)

**Temporal Understanding Hallucination** arises when a model misrepresents the chronological order, timing, or duration of events, manifesting as sequencing errors where events are placed in the wrong order, or temporal grounding errors where incorrect timestamps or boundaries are assigned.

**Motion and Action Dynamics Hallucination** occurs when a model fails to capture how movements unfold over time, through fine-grained action errors that confuse visually similar actions, or physics attribute errors that misrepresent direction, speed, or trajectory.

**Long-Context Video Understanding Hallucination** emerges from the constraints of processing extended video, through catastrophic forgetting of earlier events, attribute merging that conflates properties across time, or redundancy failure where salient events are suppressed by low-information frames.

**Object Hallucination** describes perception-level failures across dynamic video, including existence errors that hallucinate absent objects, attribute errors that misidentify color, shape, or size, and spatial relation errors that incorrectly describe relative positions.

**Causal and Reasoning Hallucination** arises when a model produces logically unsupported conclusions, through false causality that constructs spurious cause-effect links, intent and emotion misattribution that over-interprets observable behavior, or counterfactual reasoning errors that contradict the causal structure indicated by the video.

**Cross-Modal Hallucination** occurs when models incorrectly integrate evidence across vision, audio, and text, through modality neglect, audio-visual asynchrony, audio-video grounding failure, or false premise text-video hallucination.


# Mitigation Techniques

Mitigating hallucination requires interventions at specific stages of the Vid-LLM pipeline, targeting the failure mechanisms identified in the taxonomy above. Before reviewing techniques by hallucination category, we first describe the landscape of methodological paradigms and how they map onto the pipeline.

## Overview of Mitigation Strategies

The figure below illustrates five methodological paradigms and identifies where each intervenes in the Vid-LLM pipeline: reinforcement learning and preference optimization reshape training objectives; supervision augmentation and synthetic data address gaps in the training distribution; structured representations make entities, relations, and temporal indices explicit during reasoning; long-context information management governs what the model retains, compresses, or retrieves across time; and inference-time interventions steer generation toward grounded evidence without modifying model weights.

![Mitigation Overview](figure/mitigation_pipeline_overview.jpg)

## Mitigation Techniques Taxonomy

With the pipeline paradigms in mind, we now present the mitigation techniques taxonomy. As illustrated below, we organize all reviewed mitigation methods by the hallucination category they address, making explicit the connection between each failure type and the techniques designed to remedy it.

![Mitigation Taxonomy](figure/mitigation_taxonomy_overview.png)


### Temporal Understanding Hallucination Mitigation

Existing approaches fall into three paradigms, as illustrated below. Preference optimization and reinforcement learning directly penalize temporal errors through verifiable reward signals such as timestamp-aware IoU. Specialized architectures encode temporal structure via dedicated time tokens, dual-stream designs, or continuous distributional decoders. Synthetic data augmentation exposes models to reversed or shuffled event sequences, improving robustness against temporal shortcut learning.

![Temporal Mitigation](figure/temporal_hallucination_mitigation.jpg)


### Motion and Action Dynamics Hallucination Mitigation

Mitigation methods target different aspects of motion perception, as illustrated below, spanning six paradigms: object-centric tracking for explicit per-entity trajectory representations; specialized motion architectures for high-frequency inter-frame dynamics; graph-based reasoning over object interactions; dynamic token manipulation and visual prompting for inference-time attention steering; reinforcement learning with motion-sensitive reward signals; and physics-informed modeling that embeds geometric priors to reject implausible hypotheses.

![Motion Mitigation](figure/motion_hallucination_mitigation.jpg)


### Long-Context Video Understanding Hallucination Mitigation

Long-context hallucination arises when models fail to retain, compress, or retrieve information across extended video. As illustrated below, mitigation strategies are organized into five categories by where they intervene: memory optimization, token compression and merging, dynamic frame sampling, retrieval-augmented generation, and agentic workflows for iterative evidence gathering and verification.

![Long Context Mitigation](figure/long_context_hallucination_mitigation.jpg)


### Object Hallucination Mitigation

Object hallucination is addressed through four paradigms, as illustrated below: object-centric tracking and fine-grained grounding for explicit entity localization; reinforcement learning and preference optimization that penalize unsupported object predictions; neuro-symbolic and graph-based reasoning that enforces verifiable inference over entities and relations; and inference-time intervention and attention modulation for training-free evidence steering.

![Object Mitigation](figure/object_hallucination_mitigation.jpg)


### Causal and Reasoning Hallucination Mitigation

The figure below covers mitigation for causal and reasoning hallucination alongside cross-modal hallucination. For causal and reasoning failures, four paradigms are employed: reinforcement learning that supervises the faithfulness of intermediate reasoning steps; neuro-symbolic and graph-based reasoning for auditable causal inference; chain-of-thought reasoning that anchors each inferential step to visual evidence; and multi-agent frameworks where specialized agents debate and verify conclusions.

![Reasoning Mitigation](figure/reasoning_and_cross_modal_hallucination_mitigation.jpg)


### Cross-Modal Hallucination Mitigation

Cross-modal hallucination mitigation, also depicted in the figure above, follows two paradigms. Temporal synchronization and interleaved encoding aligns audio and visual signals at fine temporal granularity to prevent source misattribution. Cross-modal alignment through contrastive learning and causal deconfounding enforces representational consistency across vision, audio, and text feature spaces.


# Future Directions

The hallucination taxonomy and mitigation techniques review above surface three open challenges that current methods have not fully resolved, pointing toward promising directions for future research.

### Benchmarking and Evaluation for Video Hallucination

Current benchmarks emphasize task accuracy without disentangling whether strong performance reflects faithful grounding or fluent confabulation. A model may achieve high scores while fabricating objects, inventing temporal orderings, or hallucinating causal relationships not present in the video. Future evaluation protocols must explicitly annotate what is absent alongside what is present, measure grounding accuracy rather than answer correctness alone, and cover diverse temporal scales and hallucination types.

### Out-of-Schema Strategies

Most mitigation methods assume that encountered entities, actions, and event structures fall within the training distribution. Real-world videos routinely present novel entities and unseen configurations, causing Vid-LLMs to project unfamiliar percepts onto the nearest learned prototype and generate fluent but fabricated descriptions. Promising directions include explicit uncertainty estimation over visual concepts, open-vocabulary grounding that composes outputs from primitive visual attributes, large-scale open-world data expansion, and enabling models to recognize the boundaries of their own knowledge.

### Temporal Causality and Action Dynamics via World-Modeling

Video-specific hallucinations stem not only from weak temporal modeling but also from insufficient physical-world understanding. Current Vid-LLMs lack explicit representations of object permanence, motion continuity, and cause-effect transitions, relying instead on appearance cues and language priors. Future Vid-LLMs or Video World Models that explicitly represent how the physical world evolves could substantially reduce hallucinations related to action dynamics and false causal narratives.


# Citation

```bibtex
@article{sheng2026videohallucination,
  title={A Survey on Hallucination in Video Understanding: Taxonomy, Causes, and Mitigation Techniques},
  author={Sheng, Jiayi and Luo, Wei and Yin, Wotao},
  year={2026}
}
```
