# Baihu Paper

This repository tracks the LaTeX draft and supporting materials for the Baihu dataset paper.

## Working title

**Baihu: A Billion-Frame Multi-Embodiment Robot Manipulation Dataset for Embodied Foundation Models**

## Current paper direction

The current paper positions Baihu as a large-scale, fully human-teleoperated, LeRobot v2.1-standardized, multi-platform robot manipulation dataset for robot foundation-model adaptation, offline zero-shot evaluation, and event-aware robot learning. The draft emphasizes two complementary dataset strengths: (1) large-scale dense trajectories for adapting pretrained robot foundation models and improving offline zero-shot generalization, and (2) event/keyframe-level temporal annotations for keyframe-aware and goal-oriented learning.

The draft emphasizes:

- billion-frame robot data for foundation-model adaptation;
- fully human-teleoperated trajectories collected on physical robot platforms;
- multi-platform robot manipulation data coverage;
- task, scenario, skill, robot-category, end-effector, and camera-configuration characterization;
- event- and keyframe-level temporal annotations that capture task progress and semantic milestones beyond gripper or dexterous-hand state changes;
- HDF5-to-LeRobot v2.1 data standardization;
- offline open-loop zero-shot benchmark evaluation with GR00T N1.6;
- reserved real-world evaluation protocol for event-guided value learning.

## Repository structure

```text
paper/
  main.tex                         # Main LaTeX entry point
  body_main.tex                    # Abstract, introduction, related work, and dataset scale
  body_dataset_rest.tex            # Data collection protocol, skill taxonomy, platform composition, robot configurations, scenarios, objects, data format, and event/keyframe annotations
  body_processing_benchmark.tex    # Data processing pipeline and benchmark experiments
  body_discussion.tex              # Discussion and limitations
  body_conclusion.tex              # Conclusion
  references.bib                   # Bibliography entries
  tables/                          # Dataset, skill taxonomy, robot configuration, platform, and benchmark tables

materials/                         # Source materials and experiment records
notes/                             # Notes and remaining tasks
```

## Main LaTeX build entry

The current paper draft should be built from:

```text
paper/main.tex
```

`main.tex` imports the paper body from the modular `body_*.tex` files and includes the task-level benchmark table in the appendix.

## Baihu facts used in the current draft

- Primary dataset version: `BAIHU_v2.0`
- Status: completed
- Collection protocol: fully human teleoperation on physical robot platforms
- Duration: 9521.75 hours
- Tasks: 2989
- Episodes: 513575
- Frames: 1028349814
- Source format: HDF5
- Training format: LeRobot v2.1
- Robot platforms: 13
- Manipulation skills: 64
- Event/keyframe annotations: semantic temporal milestones including gripper events, contact events, motion events, alignment events, boundary-crossing events, object-state events, handover events, and deformation-related events

## Benchmark facts used in the current draft

- Model family: GR00T N1.6
- Baseline checkpoint: `checkpoint-1` no-training checkpoint
- Baihu-trained checkpoint: `checkpoint-390000` complete 1-epoch checkpoint fully fine-tuned on `BAIHU_v2.0`
- Evaluation type: offline open-loop zero-shot evaluation
- Evaluation scope: 13 platforms and 42 paired task-dataset records
- Overall Normalized ALL MSE reduction: 96.79%
- Overall Normalized Joint MSE reduction: 99.42%
- Planned event-guided evaluation: real-world comparison between dense-trajectory-only training and event-derived value supervision

## Important open issues

The final paper should still verify or refine:

1. final platform naming and hardware-description consistency;
2. dataset split definitions and release/access policy;
3. data quality control rules and filtering statistics;
4. formal citations and BibTeX entries for related datasets and model/tooling dependencies;
5. final figure/table captions and target submission formatting;
6. completed closed-loop, held-out-platform, data-scaling, or event-guided real-world rollout experiments.
