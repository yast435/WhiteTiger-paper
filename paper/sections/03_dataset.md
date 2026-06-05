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

Baihu v2.0 integrates robot manipulation data from multiple sources, including internally collected model-demand data and externally delivered or filtered datasets. The current version management document records two descriptions of the v2.0 data source: one refers to internally required model data and delivered data, while another refers to internally required model data and Dwheel-filtered data. This source definition should be finalized before publication. In this draft, we refer to the data sources generally as multi-source robot manipulation data curated for model training.

## 3.4 Embodiment distribution

Baihu v2.0 contains data from 14 embodiment tags, covering a diverse set of robot platforms and data sources. The dataset is not uniformly distributed across embodiments. Instead, it follows a long-tailed distribution in which a few high-resource embodiments contribute the majority of the data, while several low-resource embodiments provide additional embodiment diversity.

The largest embodiment subset is **genie1**, which contains 4253.51 hours of data and accounts for 44.67% of the total dataset duration. The next largest subsets are **gr2** with 959.40 hours, **xinghaitu_r1** with 896.88 hours, **qinlongros1** with 747.41 hours, and **zhiyuana2** with 646.74 hours. Together, the top five embodiment tags contribute approximately 78.81% of the total dataset duration. This distribution suggests that Baihu v2.0 provides both large-scale high-resource training data and a long tail of lower-resource embodiments for studying cross-embodiment generalization.

Because the dataset distribution is imbalanced, benchmark evaluation should report not only aggregate performance but also per-embodiment performance. In particular, evaluating models separately on high-resource and low-resource embodiments can reveal whether a policy benefits from the full multi-embodiment dataset or primarily learns from the dominant embodiment subsets.

### Current embodiment statistics

| Embodiment tag | Hours | Hours percent | Notes |
|---|---:|---:|---|
| genie1 | 4253.51 | 44.67% | Platform description needed |
| gr2 | 959.40 | 10.08% | Platform description needed |
| xinghaitu_r1 | 896.88 | 9.42% | Platform description needed |
| qinlongros1 | 747.41 | 7.85% | Platform description needed |
| zhiyuana2 | 646.74 | 6.79% | Platform description needed |
| [remaining tags] | [TBD] | [TBD] | Complete table needed |

The final paper should include the complete 14-row embodiment distribution table with tasks, episodes, frames, hours, and percentage.

## 3.5 Data format

Baihu v2.0 is organized in the LeRobot format. Each episode is represented as a trajectory consisting of temporally ordered observation-action pairs. Depending on the original robot platform and task setup, each trajectory may include visual observations, robot state information, action commands, task labels, and metadata. The standardized format is intended to support direct loading by robot learning pipelines and large-scale model training systems.

A typical Baihu trajectory can be represented as:

- task instruction or task label;
- synchronized visual observations;
- robot proprioceptive states;
- robot actions;
- episode metadata;
- frame-level timestamps or ordering information.

The exact modality fields, action dimensions, and robot embodiment definitions should be specified for each data subset in the final release.
