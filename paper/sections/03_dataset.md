# 3. Baihu Dataset

## 3.1 Dataset version

This paper focuses on **Baihu v2.0**, the first completed large-scale version of the Baihu dataset designed for embodied foundation model pretraining. Baihu v2.0 was constructed from robot data available up to April 1, 2026, and was released as a completed dataset version on April 25, 2026. It contains **9521.75 hours** of robot manipulation data, **2989 tasks**, **513575 episodes**, and **1028349814 frames**.

Although later versions such as Baihu v3 and Baihu v4 are under development, they are not used as the primary dataset in this paper because they remain in progress. Baihu v2.0 is therefore selected as the main experimental and analytical version due to its completed status and stable data scale.

## 3.2 Dataset scale

Baihu v2.0 provides a large-scale robot data foundation for embodied model pretraining. Its overall statistics are summarized below.

| Dataset Version | Status | Hours | Tasks | Episodes | Frames |
|---|---:|---:|---:|---:|---:|
| Baihu v1.0 | Completed | 2693.34 | 2120 | 144141 | 290880109 |
| Baihu v1.1 / v1.2 | Completed | 2451.51 | 1287 | 96474 | 264763101 |
| **Baihu v2.0** | **Completed** | **9521.75** | **2989** | **513575** | **1028349814** |

Compared with Baihu v1, Baihu v2.0 significantly increases the dataset scale in terms of total duration, task number, episode number, and frame count. This expansion makes Baihu v2.0 suitable for large-scale pretraining of robot policies, rather than only task-specific imitation learning.

Based on the total frame count and total duration, Baihu v2.0 has an average recording rate of approximately 30 frames per second. Each episode contains approximately 2002 frames on average, corresponding to about 66.7 seconds per episode.

## 3.3 Data sources

Baihu v2.0 integrates multi-source robot manipulation data curated for model training. Source trajectories are first stored as HDF5 records and are then standardized into the LeRobot v2.1 format. This design separates source data ingestion from the final model-training format, allowing heterogeneous robot data to be processed under a unified dataset pipeline.

## 3.4 Embodiment distribution

Baihu v2.0 contains data from 14 embodiment tags, covering a diverse set of robot platforms and data sources. The dataset is not uniformly distributed across embodiments. Instead, it follows a long-tailed distribution in which a few high-resource embodiments contribute the majority of the data, while several low-resource embodiments provide additional embodiment diversity.

The largest embodiment subset is **genie1**, which contains 4253.51 hours of data and accounts for 44.67% of the total dataset duration. The next largest subsets are **gr2** with 959.40 hours, **xinghaitu_r1** with 896.88 hours, **qinlongros1** with 747.41 hours, and **zhiyuana2** with 646.74 hours. Together, the top five embodiment tags contribute approximately 78.81% of the total dataset duration. This distribution suggests that Baihu v2.0 provides both large-scale high-resource training data and a long tail of lower-resource embodiments for studying cross-embodiment generalization.

Because the dataset distribution is imbalanced, benchmark evaluation should report not only aggregate performance but also per-embodiment performance. In particular, evaluating models separately on high-resource and low-resource embodiments can reveal whether a policy benefits from the full multi-embodiment dataset or primarily learns from the dominant embodiment subsets.

## 3.5 Scenario and task coverage

Beyond scale and embodiment diversity, Baihu v2.0 is designed to cover a broad range of manipulation scenarios and task structures. The dataset is intended to support robot learning in both daily-life and production-oriented environments. Representative scenario families include industrial manufacturing, household service, catering or food handling, retail or pharmacy-like environments, and other generalization scenarios. These scenario categories provide a practical organization for analyzing how robot policies transfer across environments and object distributions.

At the task level, Baihu v2.0 contains 2989 tasks. These tasks include both short-horizon primitive behaviors and longer-horizon task chains. Many tasks can be viewed as compositions of lower-level manipulation skills, such as grasping, placing, pushing, pulling, handing over, inserting, pressing, opening, closing, sorting, and arranging. This composition makes the dataset useful not only for direct action prediction, but also for studying how low-level manipulation skills combine into longer task sequences.

## 3.6 Object and interaction diversity

Baihu v2.0 contains robot interactions with diverse real-world objects. The objects span household items, kitchen utensils, retail goods, packages, industrial parts, tools, flexible materials, and irregularly shaped objects. These objects differ in size, shape, material, rigidity, surface texture, and manipulation affordance. Such object diversity is important for learning policies that do not overfit to a small set of familiar objects.

The dataset also includes different interaction patterns between robots and objects. Some trajectories involve single-object pick-and-place behaviors, while others involve object sorting, tool use, assembly-like manipulation, container interaction, drawer or door operation, and multi-object organization. This diversity supports the study of generalization across objects, tasks, and physical interaction types.

## 3.7 Temporal horizon and skill composition

Robot manipulation tasks in Baihu v2.0 span multiple temporal horizons. Short-horizon trajectories typically correspond to local operations such as grasping, pressing, scanning, or simple placement. Medium-horizon trajectories may include object transfer, opening and closing, sorting, or simple multi-step rearrangement. Long-horizon trajectories involve more extended task chains, such as cleaning, assembly, loading or unloading, and multi-stage object organization.

This temporal diversity is important for embodied foundation model training. Short trajectories provide dense examples of low-level visuomotor control, while longer trajectories expose models to task phases, intermediate goals, action rhythm, and sequential dependencies. As a result, Baihu can support both low-level action prediction and higher-level behavior modeling.

## 3.8 Data format

Baihu v2.0 is converted from HDF5 source records into the LeRobot v2.1 format. Each episode is represented as a trajectory consisting of temporally ordered observation-action pairs. Depending on the original robot platform and task setup, each trajectory may include visual observations, robot state information, action commands, task labels, and metadata. The standardized format is intended to support direct loading by robot learning pipelines and large-scale model training systems.

A typical Baihu trajectory can be represented as:

- task instruction or task label;
- synchronized visual observations;
- robot proprioceptive states;
- robot actions;
- episode metadata;
- frame-level timestamps or ordering information.
