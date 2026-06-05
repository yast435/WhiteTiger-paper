# 2. Related Work

## 2.1 Large-scale robot manipulation datasets

Large-scale robot manipulation datasets have become a central resource for training generalist robot policies. Recent datasets differ in their collection setting, robot coverage, task diversity, and degree of standardization. RoboMIND focuses on multi-embodiment manipulation data and benchmark evaluation, providing 107k demonstration trajectories across 479 tasks and 96 object classes. DROID emphasizes in-the-wild real-world robot data, collecting 76k trajectories, 350 hours of data, and manipulation demonstrations across many scenes. BridgeData V2 provides 60,096 trajectories across 24 environments and supports goal-image and language-conditioned manipulation learning. Open X-Embodiment aggregates robot data across many institutions and robots to study cross-embodiment learning at scale.

Baihu follows the same broad direction of building large-scale robot data for embodied intelligence, but differs in its focus on pretraining-scale dataset construction, versioned data management, and LeRobot-compatible organization. Baihu v2.0 contains 9521.75 hours of robot data, 2989 tasks, 513575 episodes, and 1028349814 frames. This makes Baihu not only a task collection, but a large-scale data foundation for embodied foundation model pretraining.

A dataset comparison table is maintained in:

```text
paper/tables/dataset_comparison.md
```

## 2.2 Multi-embodiment robot learning

Multi-embodiment learning aims to train policies that can benefit from data collected on different robot platforms. This is challenging because different robots often have different action spaces, joint configurations, camera viewpoints, gripper designs, control frequencies, and task distributions. Open X-Embodiment and RoboMIND both show that cross-robot data can be useful for training more general policies, but they also highlight the need for careful data standardization and evaluation.

Baihu v2.0 is explicitly multi-embodiment. It contains 14 embodiment tags and a long-tailed distribution across robot platforms. The largest embodiment subset contributes 44.67% of the total duration, while lower-resource embodiments provide additional platform diversity. This distribution makes Baihu suitable for studying both high-resource pretraining and cross-embodiment generalization.

## 2.3 Vision-language-action models

Vision-language-action models learn to map visual observations, language instructions, robot states, and other context into robot actions. Such models require synchronized observation-action trajectories and task-level annotations. The quality, scale, and diversity of the training data directly affect whether a policy can generalize beyond narrow task-specific demonstrations.

Baihu is designed to support this training paradigm by organizing robot trajectories into standardized observation-action sequences. Its LeRobot-compatible format enables loading by modern imitation-learning and VLA training pipelines. In this paper, we evaluate this data foundation through offline open-loop zero-shot evaluation using GR00T N1.6 checkpoints.

## 2.4 Data standardization for robot learning

Robot data is inherently heterogeneous. Different data sources may use different state definitions, action dimensions, camera streams, gripper conventions, control rates, and metadata schemas. Without standardization, it is difficult to train and evaluate a unified robot policy across multiple robots and tasks.

Baihu addresses this challenge through versioned dataset management and LeRobot-compatible standardization. Versioned releases make it possible to track data additions, data cleaning, normalization changes, and embodiment-specific fixes. This is especially important for large-scale robot pretraining datasets, where small inconsistencies in state/action mapping can affect many downstream training runs.

## 2.5 Positioning of Baihu

Compared with prior datasets, Baihu v2.0 is positioned as a billion-frame, multi-embodiment robot manipulation dataset for embodied foundation model pretraining and evaluation. Its key distinction is not only its scale, but the combination of four properties: multi-source data integration, multi-embodiment coverage, LeRobot-compatible standardization, and direct offline evaluation with a foundation-model checkpoint. This positioning makes Baihu complementary to existing robot datasets and benchmarks, while targeting the specific need for large-scale embodied model pretraining infrastructure.
