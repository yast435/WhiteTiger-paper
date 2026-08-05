# WhiteTiger Paper

This repository tracks the LaTeX draft and supporting materials for the WhiteTiger dataset paper.

## Working title

**WhiteTiger: A Billion-Frame Multi-Embodiment Robot Manipulation Dataset for Embodied Foundation Models**

## Current paper direction

The current paper positions WhiteTiger as a large-scale, fully human-teleoperated, LeRobot v2.1-standardized, multi-platform robot manipulation dataset for robot foundation-model adaptation and event-aware robot learning. The draft emphasizes two complementary dataset strengths: (1) large-scale dense trajectories for adapting pretrained robot foundation models, and (2) event/keyframe-level temporal annotations for keyframe-aware and goal-oriented learning. An offline open-loop zero-shot evaluation is planned to assess transfer to external robot datasets, but the current version does not report results from that evaluation.

The draft emphasizes:

- billion-frame robot data for foundation-model adaptation;
- fully human-teleoperated trajectories collected on physical robot platforms;
- multi-platform robot manipulation data coverage;
- task, scenario, skill, robot-category, end-effector, and camera-configuration characterization;
- event- and keyframe-level temporal annotations that capture task progress and semantic milestones beyond gripper or dexterous-hand state changes;
- HDF5-to-LeRobot v2.1 data standardization;
- a planned offline open-loop zero-shot evaluation on external datasets;
- a planned real-world evaluation protocol for event-guided value learning.

## Repository structure

```text
paper/
  main.tex                         # Main CVPR-style LaTeX entry point
  cvpr.sty                         # Local CVPR-compatible style file
  body_main.tex                    # Abstract, introduction, related work, and dataset scale
  body_dataset_rest.tex            # Data collection protocol, skill taxonomy, platform composition, robot configurations, scenarios, objects, data format, and event/keyframe annotations
  body_processing_benchmark.tex    # Data processing pipeline and planned benchmark protocols
  body_discussion.tex              # Discussion and limitations
  body_conclusion.tex              # Conclusion
  references.bib                   # Bibliography entries
  tables/                          # Dataset, skill taxonomy, robot configuration, platform, and supporting tables

materials/                         # Source materials and experiment records
notes/                             # Notes and remaining tasks
```

## Main LaTeX build entry

The current paper draft should be built from:

```text
paper/main.tex
```

`main.tex` imports the paper body from the modular `body_*.tex` files and uses a CVPR-compatible layout: 10 pt Times-style font, two-column letter-paper formatting, CVPR-like margins, compact section spacing, CVPR-style captions, and numeric compressed citations.

By default, `main.tex` uses review mode:

```tex
\usepackage[review]{cvpr}
```

For an internal non-anonymous or camera-ready-style draft with page numbers, change this line to:

```tex
\usepackage[pagenumbers]{cvpr}
```

## WhiteTiger facts used in the current draft

- Primary dataset version: `WhiteTiger_v2.0`
- Status: completed
- Collection protocol: fully human teleoperation on physical robot platforms
- Duration: 12877.59 hours
- Tasks: 6195
- Episodes: 737575
- Frames: 1390781176
- Source format: HDF5
- Training format: LeRobot v2.1
- Robot platforms: 14
- Manipulation skills: 64
- Event/keyframe annotations: semantic temporal milestones including gripper events, contact events, motion events, alignment events, boundary-crossing events, object-state events, handover events, and deformation-related events

## Planned benchmark direction

- Planned model family: a foundation-model checkpoint such as GR00T N1.6
- Planned comparison: a pretrained baseline and a WhiteTiger-fine-tuned model using the same architecture
- Planned evaluation type: offline open-loop zero-shot action prediction
- Evaluation data: external robot datasets excluded from WhiteTiger training
- Planned metrics: normalized full-action MSE and normalized joint-only MSE, reported at task, platform, and overall levels
- Current status: the offline evaluation has not yet been completed, and the paper reports no quantitative results or improvement claims from it
- Planned event-guided evaluation: real-world comparison between dense-trajectory-only training and event-derived value supervision

## Important open issues

The final paper should still verify or refine:

1. final platform naming and hardware-description consistency;
2. dataset split definitions and release/access policy;
3. data quality control rules and filtering statistics;
4. formal citations and BibTeX entries for related datasets and model/tooling dependencies;
5. final figure/table captions and target submission formatting;
6. completion of the planned offline open-loop zero-shot evaluation;
7. completed closed-loop, held-out-platform, data-scaling, or event-guided real-world rollout experiments.
