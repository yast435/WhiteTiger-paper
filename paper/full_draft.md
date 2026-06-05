# Baihu: A Billion-Frame Multi-Embodiment Robot Manipulation Dataset for Embodied Foundation Models

## Abstract

Large-scale robot data is becoming a core prerequisite for training embodied foundation models and generalist robot policies. However, robot learning still lacks pretraining-scale datasets that combine broad task coverage, multi-embodiment diversity, standardized data organization, and model-oriented evaluation. In this work, we introduce **Baihu v2.0**, a billion-frame, multi-embodiment robot manipulation dataset for embodied foundation model pretraining and evaluation. Baihu v2.0 contains **9521.75 hours** of robot data, **2989 tasks**, **513575 episodes**, and **1028349814 frames** across **14 embodiment tags**. The dataset integrates multi-source robot operation data and converts HDF5 source records into the LeRobot v2.1 format to support scalable imitation learning and vision-language-action model training.

Beyond dataset construction, Baihu is designed as a reusable data foundation for evaluating embodied model pretraining. We characterize the dataset scale, version history, and long-tailed embodiment distribution, where the largest embodiment contributes 44.67% of the total duration and the top five embodiments contribute approximately 78.81%. To evaluate the utility of Baihu v2.0, we conduct offline open-loop zero-shot evaluation with GR00T N1.6 across **13 platforms** and **42 paired task-dataset records**. Compared with a no-training checkpoint, a checkpoint trained on Baihu v2.0 for one epoch reduces **Joint MSE by 99.42%** and **ALL MSE by 96.79%**. These results suggest that Baihu v2.0 provides effective large-scale supervision for robot action prediction and can serve as a practical data infrastructure for embodied foundation model development.

# 1. Introduction

Recent progress in large language models and vision-language models has shown that model capability can be significantly improved through large-scale pretraining on diverse data. Robotics, however, still lacks the same level of scalable data infrastructure. Robot learning data is expensive to collect, difficult to standardize, and highly dependent on robot embodiment, sensor configuration, task definition, and action representation. As a result, many robot policies are still trained on relatively small task-specific datasets, limiting their generalization across environments, objects, and manipulation skills.

Vision-language-action models and embodied foundation models require large-scale robot interaction data to learn generalizable mappings from visual observations, language instructions, robot states, and actions. Recent efforts such as RoboMIND \cite{wu2024robomind}, DROID \cite{khazatsky2024droid}, BridgeData V2 \cite{walke2023bridgedatav2}, and Open X-Embodiment \cite{oneill2023openx} have demonstrated the importance of large-scale robot manipulation datasets for imitation learning and generalist policy training. These datasets show that diverse real-world robot trajectories, multi-view observations, language annotations, and standardized data protocols can substantially improve robot policy learning. However, there remains a need for larger, standardized, pretraining-oriented robot datasets that can support industrial-scale model development and systematic evaluation.

To address this need, we present **Baihu v2.0**, a large-scale, multi-embodiment robot manipulation dataset for embodied foundation model pretraining and evaluation. Baihu v2.0 contains **9521.75 hours** of robot data, **2989 tasks**, **513575 episodes**, and **1028349814 frames**. Compared with small task-specific datasets, Baihu is designed to support large-scale multi-task policy learning. Compared with unstructured data collections, Baihu emphasizes version management, format standardization, and model-oriented data organization. The dataset is converted from HDF5 source records into the LeRobot v2.1 format \cite{cadene2026lerobot}, making it compatible with modern robot learning pipelines and scalable training workflows.

The goal of Baihu is not only to accumulate robot data, but also to establish a reusable data foundation for embodied intelligence. A large-scale robot dataset should answer three questions: what skills are represented, how the data is standardized, and how the data can be used to train and evaluate models. Therefore, this paper describes the Baihu dataset from three perspectives: dataset construction, data standardization, and benchmark evaluation. **Figure 1** provides an overview of the Baihu v2.0 construction and evaluation pipeline. We first introduce the data sources and version definition of Baihu v2.0. We then describe the data processing pipeline, including trajectory validation, task organization, action/state schema alignment, and HDF5-to-LeRobot v2.1 format conversion. Finally, we evaluate a GR00T N1.6 checkpoint trained on Baihu v2.0 through offline open-loop zero-shot evaluation across 13 platforms and 42 paired task-dataset records. Compared with the no-training checkpoint, Baihu training reduces Joint MSE by **99.42%** and ALL MSE by **96.79%**.

**Figure 1. Overview of the Baihu v2.0 construction and evaluation pipeline.** Multi-source robot demonstrations are stored as HDF5 trajectory records, pass through quality control and schema alignment, are converted into LeRobot v2.1 format, and are then used for GR00T N1.6 pretraining and offline zero-shot open-loop evaluation. Editable source: `paper/figures/baihu_pipeline_editable.html`.

The main contributions of this work are as follows:

1. We introduce **Baihu v2.0**, a billion-frame, multi-embodiment robot manipulation dataset containing 9521.75 hours of data, 2989 tasks, 513575 episodes, and 1028349814 frames for embodied foundation model pretraining.

2. We present a standardized robot data organization pipeline that converts multi-source HDF5 robot operation records into a unified LeRobot v2.1 format, enabling scalable training of robot policies and vision-language-action models.

3. We characterize the embodiment-level distribution of Baihu v2.0 and show that it contains both high-resource embodiments and a long tail of low-resource embodiments, providing a foundation for studying cross-embodiment generalization.

4. We provide an offline zero-shot benchmark across 13 platforms and 42 paired task-dataset records, showing that a GR00T N1.6 checkpoint trained on Baihu v2.0 substantially reduces action prediction error compared with a no-training checkpoint.

5. We provide a versioned dataset management framework that supports continuous dataset expansion from Baihu v1 to Baihu v2 and future large-scale versions, making the dataset suitable for iterative model development.

# 2. Related Work

## 2.1 Large-scale robot manipulation datasets

Large-scale robot manipulation datasets have become a central resource for training generalist robot policies. Recent datasets differ in their collection setting, robot coverage, task diversity, and degree of standardization. RoboMIND focuses on multi-embodiment manipulation data and benchmark evaluation, providing 107k demonstration trajectories across 479 tasks and 96 object classes \cite{wu2024robomind}. DROID emphasizes in-the-wild real-world robot data, collecting 76k trajectories, 350 hours of data, and manipulation demonstrations across many scenes \cite{khazatsky2024droid}. BridgeData V2 provides 60,096 trajectories across 24 environments and supports goal-image and language-conditioned manipulation learning \cite{walke2023bridgedatav2}. Open X-Embodiment aggregates robot data across many institutions and robots to study cross-embodiment learning at scale \cite{oneill2023openx}.

Baihu follows the same broad direction of building large-scale robot data for embodied intelligence, but differs in its focus on pretraining-scale dataset construction, versioned data management, and HDF5-to-LeRobot v2.1 standardization. Baihu v2.0 contains 9521.75 hours of robot data, 2989 tasks, 513575 episodes, and 1028349814 frames. This makes Baihu not only a task collection, but a large-scale data foundation for embodied foundation model pretraining.

**Table 6. Comparison with representative robot manipulation datasets.** This table compares Baihu v2.0 with representative large-scale robot manipulation datasets and benchmarks. Public dataset numbers are listed according to the cited papers.

| Dataset | Real robot | Multi-embodiment | Scale | Tasks / Skills | Episodes / Trajectories | Format / Standardization | Main focus |
|---|---|---|---:|---:|---:|---|---|
| RoboMIND \cite{wu2024robomind} | Yes | Yes | 4 embodiments; hours not reported in the abstract | 479 tasks, 96 object classes | 107k demonstration trajectories | Unified collection platform and standardized protocol | Multi-embodiment manipulation benchmark with VLA evaluation |
| DROID \cite{khazatsky2024droid} | Yes | Limited / same robot setup across many sites | 350 hours, 564 scenes | 84 tasks | 76k demonstration trajectories | DROID dataset format and reproducible hardware setup | In-the-wild real-world manipulation data |
| Open X-Embodiment / RT-X \cite{oneill2023openx} | Yes | Yes | 22 robots from 21 institutions | 527 skills / 160,266 tasks | Large cross-robot data mixture | Standardized cross-embodiment data formats | X-robot policy learning and RT-X model training |
| BridgeData V2 \cite{walke2023bridgedatav2} | Yes | Limited / one low-cost robot platform | 24 environments | Multi-task manipulation behaviors | 60,096 trajectories | Dataset for goal-image and language-conditioned learning | Scalable robot learning across environments and tasks |
| **Baihu v2.0** | **Yes** | **Yes** | **9521.75 hours, 1.03B frames, 14 embodiment tags** | **2989 tasks** | **513,575 episodes** | **HDF5 source records converted to LeRobot v2.1** | **Billion-frame multi-embodiment data foundation for embodied foundation model pretraining and evaluation** |

Baihu uses `episode` terminology, while some prior datasets use `trajectory`. In this table, both refer to temporally ordered robot demonstration sequences. Baihu's distinguishing emphasis is the combination of billion-frame scale, multi-embodiment coverage, versioned data management, HDF5-to-LeRobot v2.1 standardization, and direct offline evaluation with GR00T N1.6 checkpoints.

## 2.2 Multi-embodiment robot learning

Multi-embodiment learning aims to train policies that can benefit from data collected on different robot platforms. This is challenging because different robots often have different action spaces, joint configurations, camera viewpoints, gripper designs, control frequencies, and task distributions. Open X-Embodiment and RoboMIND both show that cross-robot data can be useful for training more general policies, but they also highlight the need for careful data standardization and evaluation \cite{oneill2023openx,wu2024robomind}.

Baihu v2.0 is explicitly multi-embodiment. It contains 14 embodiment tags and a long-tailed distribution across robot platforms. The largest embodiment subset contributes 44.67% of the total duration, while lower-resource embodiments provide additional platform diversity. This distribution makes Baihu suitable for studying both high-resource pretraining and cross-embodiment generalization.

## 2.3 Vision-language-action models

Vision-language-action models learn to map visual observations, language instructions, robot states, and other context into robot actions. Such models require synchronized observation-action trajectories and task-level annotations. The quality, scale, and diversity of the training data directly affect whether a policy can generalize beyond narrow task-specific demonstrations.

Baihu is designed to support this training paradigm by organizing robot trajectories into standardized observation-action sequences. Its LeRobot v2.1 format enables loading by modern imitation-learning and VLA training pipelines \cite{cadene2026lerobot}. In this paper, we evaluate this data foundation through offline open-loop zero-shot evaluation using GR00T N1.6 checkpoints.

## 2.4 Data standardization for robot learning

Robot data is inherently heterogeneous. Different data sources may use different state definitions, action dimensions, camera streams, gripper conventions, control rates, and metadata schemas. Without standardization, it is difficult to train and evaluate a unified robot policy across multiple robots and tasks.

Baihu addresses this challenge through versioned dataset management and HDF5-to-LeRobot v2.1 standardization. Versioned releases make it possible to track data additions, data cleaning, normalization changes, and embodiment-specific fixes. This is especially important for large-scale robot pretraining datasets, where small inconsistencies in state/action mapping can affect many downstream training runs.

## 2.5 Positioning of Baihu

Compared with prior datasets, Baihu v2.0 is positioned as a billion-frame, multi-embodiment robot manipulation dataset for embodied foundation model pretraining and evaluation. Its key distinction is not only its scale, but the combination of four properties: multi-source data integration, multi-embodiment coverage, HDF5-to-LeRobot v2.1 standardization, and direct offline evaluation with a foundation-model checkpoint. This positioning makes Baihu complementary to existing robot datasets and benchmarks, while targeting the specific need for large-scale embodied model pretraining infrastructure.

# 3. Baihu Dataset

## 3.1 Dataset version

This paper focuses on **Baihu v2.0**, the first completed large-scale version of the Baihu dataset designed for embodied foundation model pretraining. Baihu v2.0 was constructed from robot data available up to April 1, 2026, and was released as a completed dataset version on April 25, 2026. It contains **9521.75 hours** of robot manipulation data, **2989 tasks**, **513575 episodes**, and **1028349814 frames**.

Although later versions such as Baihu v3 and Baihu v4 are under development, they are not used as the primary dataset in this paper because they remain in progress. Baihu v2.0 is therefore selected as the main experimental and analytical version due to its completed status and stable data scale.

## 3.2 Dataset scale

Baihu v2.0 provides a large-scale robot data foundation for embodied model pretraining. Its overall statistics are summarized in **Table 1**.

**Table 1. Baihu dataset version statistics.** The table compares completed Baihu dataset versions by duration, task count, episode count, and frame count.

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

Baihu v2.0 contains data from 14 embodiment tags, covering a diverse set of robot platforms and data sources. **Table 2** summarizes the embodiment-level distribution. The dataset is not uniformly distributed across embodiments. Instead, it follows a long-tailed distribution in which a few high-resource embodiments contribute the majority of the data, while several low-resource embodiments provide additional embodiment diversity.

**Figure 2. Embodiment duration distribution of Baihu v2.0.** The figure shows the long-tailed duration distribution across 14 embodiment tags. The largest embodiment subset contributes 44.67% of the total duration, while the top five embodiment tags contribute approximately 78.81%.

**Table 2. Embodiment-level distribution of Baihu v2.0.** The table reports tasks, episodes, frames, hours, and duration percentage for each of the 14 embodiment tags.

| Embodiment tag | Tasks | Episodes | Frames | Hours | Hours percent |
|---|---:|---:|---:|---:|---:|
| arx_loong | 3 | 842 | 2223107 | 20.58 | 0.22% |
| astribots1 | 100 | 29889 | 40932735 | 379.01 | 3.98% |
| cobotmagic | 112 | 33098 | 55115692 | 510.33 | 5.36% |
| dualur5e | 49 | 14805 | 22939047 | 212.40 | 2.23% |
| dwheel | 231 | 13485 | 36172626 | 334.93 | 3.52% |
| fr3 | 9 | 2090 | 4037181 | 37.38 | 0.39% |
| genie1 | 1512 | 140718 | 459379181 | 4253.51 | 44.67% |
| gr2 | 319 | 96686 | 103615634 | 959.40 | 10.08% |
| lejukuaihu | 123 | 29095 | 42586155 | 394.32 | 4.14% |
| qinlongros1 | 145 | 39401 | 80720445 | 747.41 | 7.85% |
| qinlongros2 | 22 | 7105 | 11189034 | 103.60 | 1.09% |
| tianji | 6 | 1575 | 2727875 | 25.26 | 0.27% |
| xinghaitu_r1 | 239 | 70111 | 96863491 | 896.88 | 9.42% |
| zhiyuana2 | 119 | 34675 | 69847611 | 646.74 | 6.79% |
| **Total** | **2989** | **513575** | **1028349814** | **9521.75** | **100.01%** |

The percentage sum is 100.01% due to rounding to two decimal places.

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

# 4. Data Processing Pipeline

Baihu v2.0 is constructed through a multi-stage data processing pipeline that converts heterogeneous robot operation records into a consistent, model-trainable dataset. The source records are stored in HDF5 format and are converted into the LeRobot v2.1 format for downstream training and evaluation. The pipeline is designed around two goals: preserving embodiment-specific control information and producing a standardized dataset representation that can be used by large-scale imitation-learning and vision-language-action training pipelines. The overall construction and evaluation workflow is summarized in **Figure 1**.

Because Baihu v2.0 integrates data from multiple robot platforms, the processing pipeline must handle differences in sensor configurations, robot states, action spaces, gripper or hand structures, task metadata, and source-specific recording formats. The pipeline consists of seven major stages: HDF5 ingestion, quality monitoring, trajectory validation, metadata standardization, state/action schema alignment, LeRobot v2.1 conversion, and versioned dataset release.

## 4.1 HDF5 raw data ingestion

The raw Baihu v2.0 records are first organized in HDF5 files. HDF5 is used as the source container for robot trajectories before conversion. During ingestion, each trajectory is registered with its data source, embodiment tag, task name, episode identity, and available modality fields. This registration step makes it possible to trace each processed episode back to its source platform and task context.

The ingestion stage is designed for heterogeneous robot data. Different embodiments may include full-size humanoid robots, humanoid-like wheeled platforms, dual-arm robots, single-arm manipulators, or portable data collection devices. Rather than assuming a single robot morphology, Baihu preserves platform-specific state and action schemas. This design allows each robot to retain its native control representation while still being organized under a common dataset interface.

For each ingested episode, the pipeline extracts the available modalities from the source HDF5 record. A typical trajectory may contain visual observations, robot proprioceptive states, robot actions, task annotations, timestamps or frame indices, and episode-level metadata. The exact fields depend on the source embodiment and collection setup.

## 4.2 Multi-stage quality control

Baihu uses a multi-stage quality control workflow before data is included in the released training corpus. The workflow can be summarized as a collection-to-delivery chain: data acquisition, real-time quality monitoring, cloud upload, trajectory validation, data cleaning, manual review, and final dataset delivery.

During collection, real-time monitoring is used to detect obvious acquisition failures as early as possible. This helps reduce large deviations at the data source, such as missing streams, failed recording, abnormal robot states, or incomplete task execution. After collection, data is uploaded for more detailed validation and cleaning. The validation stage checks whether the trajectory, modality fields, and metadata are usable for downstream model training. After automated or semi-automated checks, professional review is used to further ensure that delivered data satisfies the required collection standard.

This quality-control design is important because Baihu v2.0 contains 513575 episodes and more than 1.03 billion frames. Even a small percentage of invalid trajectories can correspond to a large amount of unusable data. The multi-stage workflow therefore aims to prevent low-quality trajectories from entering the training corpus and to maintain consistent quality across heterogeneous sources.

## 4.3 Trajectory validation and filtering

Episode validation is used to remove incomplete, corrupted, or invalid trajectories before model training. A valid episode should contain temporally ordered observations and actions, consistent task metadata, and usable modality fields. For robot learning, trajectory validity is not only a file integrity issue; it also affects whether the model receives correct observation-action supervision.

The validation process checks the consistency between observations, actions, and metadata. For example, a processed episode should have aligned observation and action sequences, a valid task label or task identifier, a valid embodiment tag, and complete files for the required modalities. When visual observations are stored as image or video streams, the corresponding frame count should be consistent with the trajectory metadata. When state/action arrays are stored separately from videos, their temporal ordering should match the episode timeline.

## 4.4 Task annotation and metadata standardization

Baihu v2.0 contains 2989 tasks across multiple robot embodiments. To make the dataset usable for policy training and evaluation, task information is standardized into a consistent metadata representation. Each episode is associated with a task label or language instruction, an embodiment tag, an episode identifier, and source-specific metadata when available.

Task metadata standardization enables dataset analysis by task, embodiment, and data source. It also supports downstream evaluation settings such as held-out task evaluation, per-platform evaluation, and task-level error analysis. In addition to task names, Baihu can organize trajectories according to higher-level scenario categories, task families, temporal horizons, and atomic skill composition when such annotations are available.

For model training, task annotations provide the language or semantic conditioning signal used by vision-language-action policies. Therefore, task naming and task normalization are important parts of the dataset construction process.

## 4.5 State and action schema alignment

Because Baihu integrates multiple robot embodiments, state and action representations must be aligned before training. Different platforms may have different numbers of joints, different gripper or hand structures, different control frequencies, and different action semantics. Baihu therefore uses embodiment-specific schemas to describe which dimensions correspond to arm joints, grippers, hands, or other controllable components.

This schema alignment is necessary for two reasons. First, it allows each embodiment to preserve its native control structure rather than being forced into an overly simplified universal action vector. Second, it enables evaluation metrics such as Joint MSE and ALL MSE to select the appropriate action dimensions for each platform. Joint MSE uses only joint-related action dimensions, while ALL MSE uses all available action dimensions for the embodiment.

In practice, each embodiment defines its state dimension ordering, action dimension ordering, joint fields, gripper or hand fields, and any additional controllable components. This schema-level description is also necessary for training reusable policies across multiple robots because the model must know how to interpret each dimension in the action vector.

## 4.6 Gripper and hand dimension handling

For each robot embodiment, gripper and hand data are stored according to the actual physical travel range of that platform. Baihu does not assume a single universal gripper or hand range across all robots. A parallel-jaw gripper, a dexterous hand, and a robot-specific end-effector may have different mechanical limits and different action semantics, so their values are preserved according to the corresponding embodiment's real motion range.

This design avoids forcing heterogeneous end-effectors into an artificial shared scale during dataset construction. It also makes the dataset more faithful to the original robot control space. During evaluation, Joint MSE can be computed using only joint-related dimensions, while ALL MSE includes the full action vector, including gripper or hand dimensions when they are present. This distinction is important because gripper and hand dimensions may have different numerical ranges, sparsity patterns, and task relevance across embodiments.

## 4.7 HDF5-to-LeRobot v2.1 conversion

After validation and schema alignment, Baihu v2.0 is converted from the source HDF5 format into the LeRobot v2.1 format. This conversion standardizes heterogeneous robot trajectories into a common representation for model training and evaluation. The converted dataset stores observation-action trajectories, task information, embodiment metadata, and episode-level indexing in a LeRobot-compatible structure.

This conversion step separates the source storage format from the training format. HDF5 provides the raw trajectory container, while LeRobot v2.1 provides the standardized dataset interface used by downstream imitation-learning and vision-language-action training pipelines.

## 4.8 Version management and reproducibility

Baihu is maintained as a versioned dataset. Version management is important because large-scale robot datasets often require iterative fixes to action mappings, gripper or hand representations, task annotations, and metadata. By tracking dataset versions, Baihu makes it possible to reproduce training runs and understand which data corrections or additions are included in each release.

Earlier Baihu releases corrected issues such as effector mapping range inconsistencies and gripper value reconstruction. These examples show why quality control is essential for robot foundation model pretraining: small errors in action dimensions or end-effector values can affect many downstream model updates. Baihu v2.0 is therefore treated as a completed and stable dataset version for the experiments in this paper.

The versioned release design also separates completed dataset versions from in-progress versions. This paper focuses on Baihu v2.0 because it is a stable completed version with fixed statistics and an associated offline benchmark. Later versions can expand the dataset, but they should be reported separately to avoid mixing training and evaluation records across dataset releases.

# 5. Benchmark Experiments

To demonstrate the value of Baihu for embodied foundation model training, we evaluate whether a model trained on Baihu v2.0 improves offline action prediction on unseen tasks across multiple robot embodiments. The benchmark focuses on open-loop zero-shot evaluation, where the model predicts actions from recorded observations and the predictions are compared against ground-truth actions in held-out trajectories.

## 5.1 Offline open-loop zero-shot evaluation

Open-loop evaluation provides an offline assessment by comparing the model's predicted actions against ground-truth actions from robot demonstration data. In this benchmark, we compare two GR00T N1.6 checkpoints:

| Checkpoint | Description |
|---|---|
| GR00T N1.6 / checkpoint-1 | No-training checkpoint, used as the baseline |
| GR00T N1.6 / checkpoint-390000 | Complete 1-epoch checkpoint trained on BAIHU_v2.0 |

The evaluation covers **13 platforms** and **42 paired task-dataset records**. This setting is designed to measure whether Baihu v2.0 pretraining improves cross-task offline prediction performance without additional task-specific finetuning on the evaluation tasks.

## 5.2 Evaluation metrics

The benchmark reports two mean squared error metrics. For each trajectory, the policy predicts an action sequence from the recorded observations. The predicted action values are compared with the corresponding demonstration actions from the dataset. For a selected set of action dimensions and evaluated timesteps, MSE is the average squared difference between the predicted action and the dataset action.

**ALL MSE** computes this error using all action dimensions available in the dataset. This metric reflects the model's full action prediction error, including arm, gripper, hand, or other action dimensions when they are present.

**Joint MSE** computes this error using only the joint-related action dimensions. Gripper or hand dimensions are excluded from this metric. This metric is useful because hand and gripper dimensions may have different scales, semantics, or sparsity patterns across embodiments, and may otherwise dominate or distort the action prediction error.

For each task-dataset record, MSE is averaged over the evaluated trajectory steps and selected action dimensions. Aggregate results are then reported as the unweighted mean across the 42 paired task-dataset records unless otherwise specified.

## 5.3 Overall results

The benchmark results show that one epoch of training on BAIHU_v2.0 substantially reduces offline open-loop prediction error across the 13 evaluated platforms and 42 paired task-dataset records. The overall result is summarized in **Table 3** and visualized in **Figure 3**.

**Figure 3. Offline zero-shot open-loop evaluation results.** The figure compares GR00T N1.6/checkpoint-1 and GR00T N1.6/checkpoint-390000 on Joint MSE and ALL MSE across 42 paired task-dataset records from 13 evaluated platforms.

**Table 3. Overall offline zero-shot open-loop evaluation results.** The table reports the unweighted mean over 42 paired task-dataset records. Lower MSE is better.

| Model checkpoint | Joint MSE ↓ | ALL MSE ↓ |
|---|---:|---:|
| checkpoint-1 (no training) | 0.116746 | 0.157283 |
| checkpoint-390000 (Baihu v2.0, 1 epoch) | 0.000680 | 0.005051 |
| Relative improvement | 99.42% | 96.79% |

Compared with the no-training checkpoint-1 baseline, the Baihu-trained checkpoint reduces Joint MSE by **99.42%** and ALL MSE by **96.79%**. This indicates that Baihu v2.0 provides effective supervision for improving action prediction across a broad set of unseen task-dataset records.

## 5.4 Per-platform results

Because Baihu v2.0 is a multi-embodiment dataset, aggregate metrics alone are insufficient. We therefore also report per-platform results. Across all 13 evaluated platforms, the Baihu-trained checkpoint achieves lower Joint MSE and ALL MSE than the no-training baseline.

**Table 4. Per-platform offline zero-shot evaluation results.** The table reports the unweighted mean over paired task-dataset records for each evaluated platform.

| Platform | N tasks | Joint MSE checkpoint-1 ↓ | Joint MSE checkpoint-390000 ↓ | Joint improvement | ALL MSE checkpoint-1 ↓ | ALL MSE checkpoint-390000 ↓ | ALL improvement |
|---|---:|---:|---:|---:|---:|---:|---:|
| Qinglong | 6 | 0.157132 | 0.000572 | 99.64% | 0.180542 | 0.002733 | 98.49% |
| Zhiyuan A2 | 3 | 0.132757 | 0.000896 | 99.33% | 0.225461 | 0.007562 | 96.65% |
| Fourier GR-2 | 3 | 0.137993 | 0.001805 | 98.69% | 0.233863 | 0.003824 | 98.36% |
| Xinghaitu R1 | 3 | 0.075369 | 0.000197 | 99.74% | 0.117581 | 0.004864 | 95.86% |
| Leju KUAVO | 3 | 0.133456 | 0.000743 | 99.44% | 0.157261 | 0.004335 | 97.24% |
| Songling Aloha | 3 | 0.133968 | 0.000949 | 99.29% | 0.156535 | 0.002307 | 98.53% |
| Franka FR3 | 3 | 0.041619 | 0.000711 | 98.29% | 0.077804 | 0.004525 | 94.18% |
| Astribot S1 | 3 | 0.135078 | 0.000822 | 99.39% | 0.158673 | 0.005819 | 96.33% |
| UR5e | 3 | 0.062389 | 0.000068 | 99.89% | 0.083854 | 0.007897 | 90.58% |
| ARX | 3 | 0.138172 | 0.001076 | 99.22% | 0.161330 | 0.001803 | 98.88% |
| Tianji | 3 | 0.017828 | 0.000612 | 96.57% | 0.096915 | 0.002092 | 97.84% |
| Dwheel | 3 | 0.176382 | 0.000208 | 99.88% | 0.203833 | 0.005157 | 97.47% |
| Zhiyuan G1 | 3 | 0.135167 | 0.000282 | 99.79% | 0.167769 | 0.015057 | 91.02% |

The per-platform results show consistent improvement across all evaluated robot platforms, suggesting that the pretraining effect is not limited to a single dominant embodiment. This is important for Baihu because the dataset is explicitly designed as a multi-embodiment training corpus.

## 5.5 Task-level analysis

Task-level results further show that Baihu training improves offline prediction across the 42 paired task-dataset records. Task-level reporting is especially useful because different embodiments may have different action spaces, control frequencies, and gripper or hand dimensions.

**Table 5. Task-level offline zero-shot evaluation results.** The table reports Joint MSE, ALL MSE, and relative improvement for each of the 42 paired task-dataset records. The full task-level table is provided in Appendix A.

The strongest improvements are observed on tasks where the no-training checkpoint has relatively large action prediction error. The remaining residual errors after Baihu training are small in absolute value, but they can still vary across platforms and tasks due to differences in action scale, end-effector representation, and task dynamics.

## 5.6 Additional benchmark extensions

The current benchmark already provides a useful offline zero-shot comparison. Future versions can extend the benchmark with:

- data-scaling evaluation using different fractions of Baihu v2.0;
- high-resource vs low-resource embodiment evaluation;
- held-out embodiment evaluation;
- closed-loop simulation evaluation;
- real-world rollout success rate.

# 6. Discussion and Limitations

Baihu v2.0 provides a large-scale and standardized data foundation for embodied intelligence research. Its main strength lies in its scale, versioned organization, multi-embodiment coverage, and compatibility with modern robot learning pipelines. With **9521.75 hours**, **513575 episodes**, and more than **1.03 billion frames** of robot data, Baihu v2.0 supports pretraining regimes that are difficult to study using smaller task-specific datasets.

The offline zero-shot benchmark provides initial evidence that Baihu v2.0 is useful for embodied foundation model pretraining. Across 13 platforms and 42 paired task-dataset records, the Baihu-trained GR00T N1.6 checkpoint substantially reduces both Joint MSE and ALL MSE compared with the no-training checkpoint. This result suggests that Baihu v2.0 provides effective supervision for action prediction across multiple embodiments and unseen task-dataset records.

At the same time, the current benchmark should be interpreted as an offline evaluation rather than a complete measure of real-world robot performance. Open-loop action prediction is useful for measuring whether the model matches demonstration actions, but it does not capture closed-loop recovery, compounding errors, contact-rich interaction, or task success under real robot execution. Future work should therefore extend the benchmark with real-world rollout success rates, closed-loop evaluation, and simulation-based testing when available.

Baihu v2.0 also has a long-tailed embodiment distribution. A few high-resource embodiments contribute a large fraction of the total duration, while lower-resource embodiments contribute smaller but important platform diversity. This imbalance is common in large multi-source robot datasets, but it should be considered when designing training recipes, sampling strategies, and evaluation protocols. Per-platform results are therefore important for understanding whether a model benefits from the full multi-embodiment corpus rather than only from dominant subsets.

Another limitation is that the current paper focuses on a single primary benchmark setting: GR00T N1.6 checkpoint comparison under offline open-loop evaluation. Additional baselines such as ACT, Diffusion Policy, or other VLA policies would make the benchmark more comprehensive. Data scaling experiments, held-out embodiment evaluation, and high-resource versus low-resource analysis would also help clarify which aspects of Baihu v2.0 contribute most to model performance.

Finally, because Baihu integrates multi-source robot operation data, source descriptions, release constraints, and licensing conditions should be documented clearly if any part of the dataset or benchmark is released publicly. These details are important for reproducibility and for downstream users who want to understand how the dataset can be used.

# 7. Conclusion

We introduced **Baihu v2.0**, a billion-frame, multi-embodiment robot manipulation dataset for embodied foundation model pretraining and evaluation. Baihu v2.0 contains **9521.75 hours** of robot data, **2989 tasks**, **513575 episodes**, and **1028349814 frames** across **14 embodiment tags**. The dataset converts multi-source HDF5 robot operation records into the LeRobot v2.1 format, providing a standardized data foundation for training vision-language-action models and generalist robot policies.

Baihu v2.0 is not only large in scale, but also diverse across robot embodiment tags, task types, objects, and temporal horizons. Its long-tailed embodiment distribution makes it suitable for studying both high-resource policy learning and low-resource cross-embodiment generalization.

We further evaluated the utility of Baihu v2.0 through offline open-loop zero-shot evaluation with GR00T N1.6. Across **13 platforms** and **42 paired task-dataset records**, a checkpoint trained on Baihu v2.0 for one epoch reduced **Joint MSE by 99.42%** and **ALL MSE by 96.79%** compared with a no-training checkpoint. These results suggest that Baihu v2.0 provides effective large-scale supervision for robot action prediction and can serve as practical infrastructure for future embodied foundation model development.

Future work will extend Baihu with additional versions, broader benchmark protocols, more baseline models, closed-loop evaluations, and clearer release documentation. Together, these directions can further strengthen Baihu as a reusable data foundation for large-scale embodied intelligence research.

# Appendix A. Task-level benchmark results

**Table 5. Task-level offline zero-shot evaluation results.** The table reports Joint MSE, ALL MSE, and relative improvement for each of the 42 paired task-dataset records. Lower MSE is better.

| Platform | Task | Dataset ID | Task type | Steps | Joint MSE ckpt-1 | Joint MSE ckpt-390000 | Joint improvement | ALL MSE ckpt-1 | ALL MSE ckpt-390000 | ALL improvement |
|---|---|---|---|---:|---:|---:|---:|---:|---:|---:|
| Qinglong | 圆珠笔放入笔筒 | `69e47d6e377a4953960d9c90a90b704d` |  | 3000 | 0.179469 | 0.000248 | 99.86% | 0.201477 | 0.003823 | 98.10% |
| Qinglong | 水果蔬菜分类筐 | `9440958e5b604b24be9008daec947348` |  | 3000 | 0.183410 | 0.000527 | 99.71% | 0.207567 | 0.005064 | 97.56% |
| Qinglong | 把垃圾丢到垃圾罐中 | `79b05db8ac8f4ebebde2c2534e273b39` | 双臂 | 3000 | 0.132497 | 0.000339 | 99.74% | 0.160485 | 0.000669 | 99.58% |
| Qinglong | 用盖子盖住锅 | `5623ecb56a594c799084543f9134c3ba` |  | 3000 | 0.180345 | 0.000200 | 99.89% | 0.202733 | 0.002103 | 98.96% |
| Qinglong | 鸡蛋放置 | `6df85a4d00b249169f5ab91443a084d2` | 双臂 | 3000 | 0.133400 | 0.001388 | 98.96% | 0.162696 | 0.001901 | 98.83% |
| Qinglong | 食品打包 | `6e779b63d6a946c1ae9b89e77d441834` | 双臂 | 3000 | 0.133672 | 0.000732 | 99.45% | 0.148295 | 0.002839 | 98.09% |
| Zhiyuan A2 | 茄子香蕉分类_桌面背景泛化_黄格子 | `d69be408d6514322a4e8f8bf1937afc6` | 双臂 | 6000 | 0.132409 | 0.000713 | 99.46% | 0.222947 | 0.006068 | 97.28% |
| Zhiyuan A2 | 黑白胶带分类_桌面背景泛化_小鱼方巾 | `bf436fb9d6fd4319ad4eef98c351188d` | 双臂 | 6000 | 0.133624 | 0.001356 | 98.99% | 0.227667 | 0.011405 | 94.99% |
| Zhiyuan A2 | 十字体圆柱积木分类_桌面背景泛化_红色 | `1031fa122eb74232a4382656905d2338` | 双臂 | 6000 | 0.132239 | 0.000620 | 99.53% | 0.225768 | 0.005214 | 97.69% |
| Fourier GR-2 | 物品放置_玩具鸭子 | `9f4cd8915a824c429816ca7a1e072eb9` | 双臂 | 3000 | 0.137008 | 0.001172 | 99.14% | 0.244131 | 0.003854 | 98.42% |
| Fourier GR-2 | 玩具分类 | `c542ba09a28e49d098f807a374f9babf` | 双臂 | 3000 | 0.139023 | 0.001794 | 98.71% | 0.231410 | 0.003836 | 98.34% |
| Fourier GR-2 | 绿黄鸭梨分类 | `e620eda4ce88489ebac00c4e24ac6e5a` | 双臂 | 3000 | 0.137948 | 0.002447 | 98.23% | 0.226048 | 0.003783 | 98.33% |
| Xinghaitu R1 | 汤匙入盘 | `66332e1ef1064a9c8b87ee148b38515b` | 单臂_右 | 默认最大 | 0.068287 | 0.000107 | 99.84% | 0.110777 | 0.005057 | 95.44% |
| Xinghaitu R1 | 苹果和梨分类 | `ade47aa3b9fe4e0a92c8b105d051ea88` | 双臂 | 默认最大 | 0.082550 | 0.000211 | 99.74% | 0.124586 | 0.003934 | 96.84% |
| Xinghaitu R1 | 把T恤放到篮子里 | `2110f6dd503b47fb96eef4da2f2fc966` | 单臂_右 | 默认最大 | 0.075271 | 0.000274 | 99.64% | 0.117380 | 0.005601 | 95.23% |
| Leju KUAVO | 热水壶放置 | `8f9da6aeac0b4d5a8b3c6d6ce393a9f9` | 单臂_左 | 默认最大 | 0.133692 | 0.000806 | 99.40% | 0.158527 | 0.004078 | 97.43% |
| Leju KUAVO | 鸡蛋放置 | `473e0adf47c54f7d9e74b2aa83421204` | 单臂_右 | 默认最大 | 0.133315 | 0.000456 | 99.66% | 0.155686 | 0.004150 | 97.33% |
| Leju KUAVO | 打开抽屉放入物体后关闭 | `ce04377a52344e58aae94f674642bc04` | 双臂 | 默认最大 | 0.133361 | 0.000969 | 99.27% | 0.157571 | 0.004777 | 96.97% |
| Songling Aloha | 蔬菜块入碗与容器居中 | `ec37fad6f7364a99916be6ea31597232` |  |  | 0.133895 | 0.000616 | 99.54% | 0.159897 | 0.001871 | 98.83% |
| Songling Aloha | 抽屉筷子取放与餐具对齐 | `1d7fe9a4cacd4f66b689fd931356c0e7` |  |  | 0.134391 | 0.001479 | 98.90% | 0.151264 | 0.002772 | 98.17% |
| Songling Aloha | 草莓入盘并叠杯 | `06911227af864ee4be3665b01ba48de1` |  |  | 0.133619 | 0.000752 | 99.44% | 0.158443 | 0.002277 | 98.56% |
| Franka FR3 | 叠杯子 | `0f25153abb0d4101a25f155b8ce9b25d` | 双臂 | 3000 | 0.051757 | 0.000510 | 99.02% | 0.086843 | 0.002837 | 96.73% |
| Franka FR3 | 物品整理 | `66c3ffc1521342ad9f9e1cd2fb3439ce` | 双臂 | 3000 | 0.038663 | 0.000731 | 98.11% | 0.074911 | 0.005935 | 92.08% |
| Franka FR3 | 物品放置_玩具鸭子 | `9be0f4a5ab99436999e92001c48358fa` | 双臂 | 3000 | 0.034437 | 0.000892 | 97.41% | 0.071658 | 0.004803 | 93.30% |
| Astribot S1 | 碗放盘中_桌面背景泛化_绿色 | `a52b8a1f1f6b4f27aa0df348cee0876f` | 单臂_右 | 3000 | 0.134600 | 0.000700 | 99.48% | 0.159800 | 0.003900 | 97.56% |
| Astribot S1 | 预 - 汤匙入碗_桌面背景泛化_绿格子 | `698f93b7a4754ccbae02f5ab897579bc` | 单臂_右 | 3000 | 0.135232 | 0.000892 | 99.34% | 0.155694 | 0.007153 | 95.41% |
| Astribot S1 | 预-杯碟放置_桌面背景泛化_橙色 | `9b795273e5874d9984e3eb0fdda3bb3d` | 单臂_右 | 3000 | 0.135401 | 0.000875 | 99.35% | 0.160526 | 0.006405 | 96.01% |
| UR5e | 硬币放入存钱罐_补充 | `44cd7ef348ba4e63ab0e56d1ec3152be` | 单臂_右 | 6000 | 0.062034 | 0.000031 | 99.95% | 0.085492 | 0.010285 | 87.97% |
| UR5e | 堆积木 | `0f55fc9678ad4dd3bd1d6ee430bc827a` | 双臂 | 6000 | 0.062204 | 0.000131 | 99.79% | 0.081088 | 0.008773 | 89.18% |
| UR5e | 语义测试 | `5df79740a10d4784b04975f9af3c558c` | 双臂 | 6000 | 0.062930 | 0.000041 | 99.94% | 0.084983 | 0.004632 | 94.55% |
| ARX | 叠衣服 | `d9986c2261804ea791e4989874eee8c6` | 双臂 | 3000 | 0.137219 | 0.000791 | 99.42% | 0.156832 | 0.001914 | 98.78% |
| ARX | 叠衣服_修改流程 | `45b85d75f5154a939e293480af93b6a1` | 双臂 | 3000 | 0.140643 | 0.002073 | 98.53% | 0.166439 | 0.002632 | 98.42% |
| ARX | 叠衣服_强化细节 | `3b305b19f662438088646cd4e9e5e631` | 双臂 | 3000 | 0.136654 | 0.000363 | 99.73% | 0.160718 | 0.000862 | 99.46% |
| Tianji | 插花 | `69d89b55781b487f8bfa2a65ec53cd23` | 双臂 | 3000 | 0.017314 | 0.000778 | 95.50% | 0.089298 | 0.002285 | 97.44% |
| Tianji | 电池分拣 | `0fd9a046f966439fb1e915a5fed94581` | 双臂 | 3000 | 0.016675 | 0.000329 | 98.03% | 0.106140 | 0.001487 | 98.60% |
| Tianji | 叠杯子 | `de4561f3e74d494fac81e7e39d61f58a` | 双臂 | 3000 | 0.019495 | 0.000730 | 96.26% | 0.095306 | 0.002505 | 97.37% |
| Dwheel | 预-章鱼玩具分类_桌面背景泛化_小鱼方巾 | `555d81b30669478fae7072f4c2293197` | 双臂 | 1924 | 0.176082 | 0.000165 | 99.91% | 0.203914 | 0.005410 | 97.35% |
| Dwheel | 预-绿黄鸭梨分类_桌面背景泛化_蓝格子 | `138e0f85edc24dd3a2cd9e4caf03cee2` | 双臂 | 2420 | 0.176593 | 0.000242 | 99.86% | 0.203826 | 0.006220 | 96.95% |
| Dwheel | 预-圆柱立方体积木分类_桌面背景泛化_绿格子 | `d4e90e8481be4440882499681d8eea70` | 双臂 | 1951 | 0.176471 | 0.000217 | 99.88% | 0.203759 | 0.003841 | 98.11% |
| Zhiyuan G1 | 支架安装 | `129dd0dd602145a1aa17f4385c7a63a8` | 单臂_右 | 6000 | 0.135378 | 0.000409 | 99.70% | 0.167892 | 0.013138 | 92.17% |
| Zhiyuan G1 | 电源组件安装 | `f850372cd2b44bc297522657729d9dd2` | 双臂 | 6000 | 0.135268 | 0.000291 | 99.78% | 0.165634 | 0.012706 | 92.33% |
| Zhiyuan G1 | 触摸板组装 | `f6fb6e44374c47fd95dac7661b8143cb` | 双臂 | 6000 | 0.134856 | 0.000146 | 99.89% | 0.169781 | 0.019328 | 88.62% |

# References

```bibtex
@article{wu2024robomind,
  title        = {{RoboMIND}: Benchmark on Multi-embodiment Intelligence Normative Data for Robot Manipulation},
  author       = {Wu, Kun and Hou, Chengkai and Liu, Jiaming and Che, Zhengping and Ju, Xiaozhu and Yang, Zhuqin and Li, Meng and Zhao, Yinuo and Xu, Zhiyuan and Yang, Guang and Fan, Shichao and Wang, Xinhua and Liao, Fei and Zhao, Zhen and Li, Guangyu and Jin, Zhao and Wang, Lecheng and Mao, Jilei and Liu, Ning and Ren, Pei and Zhang, Qiang and Lyu, Yaoxu and Liu, Mengzhen and He, Jingyang and Luo, Yulin and Gao, Zeyu and Li, Chenxuan and Gu, Chenyang and Fu, Yankai and Wu, Di and Wang, Xingyu and Chen, Sixiang and Wang, Zhenyu and An, Pengju and Qian, Siyuan and Zhang, Shanghang and Tang, Jian},
  journal      = {arXiv preprint arXiv:2412.13877},
  year         = {2024}
}

@article{khazatsky2024droid,
  title        = {{DROID}: A Large-Scale In-The-Wild Robot Manipulation Dataset},
  author       = {Khazatsky, Alexander and Pertsch, Karl and Nair, Suraj and Balakrishna, Ashwin and Dasari, Sudeep and Karamcheti, Siddharth and Nasiriany, Soroush and Srirama, Mohan Kumar and Chen, Lawrence Yunliang and Ellis, Kirsty and others},
  journal      = {arXiv preprint arXiv:2403.12945},
  year         = {2024}
}

@article{oneill2023openx,
  title        = {Open X-Embodiment: Robotic Learning Datasets and {RT-X} Models},
  author       = {{Open X-Embodiment Collaboration} and O'Neill, Abby and Rehman, Abdul and Gupta, Abhinav and Maddukuri, Abhiram and Gupta, Abhishek and Padalkar, Abhishek and Lee, Abraham and Pooley, Acorn and Gupta, Agrim and others},
  journal      = {arXiv preprint arXiv:2310.08864},
  year         = {2023}
}

@article{walke2023bridgedatav2,
  title        = {{BridgeData V2}: A Dataset for Robot Learning at Scale},
  author       = {Walke, Homer and Black, Kevin and Lee, Abraham and Kim, Moo Jin and Du, Max and Zheng, Chongyi and Zhao, Tony and Hansen-Estruch, Philippe and Vuong, Quan and He, Andre and Myers, Vivek and Fang, Kuan and Finn, Chelsea and Levine, Sergey},
  journal      = {arXiv preprint arXiv:2308.12952},
  year         = {2023}
}

@article{cadene2026lerobot,
  title        = {{LeRobot}: An Open-Source Library for End-to-End Robot Learning},
  author       = {Cadene, Remi and Aliberts, Simon and Capuano, Francesco and Aractingi, Michel and Zouitine, Adil and Kooijmans, Pepijn and Choghari, Jade and Russi, Martino and Pascal, Caroline and Palma, Steven and Shukor, Mustafa and Moss, Jess and Soare, Alexander and Aubakirova, Dana and Lhoest, Quentin and Gallou{\'e}dec, Quentin and Wolf, Thomas},
  journal      = {arXiv preprint arXiv:2602.22818},
  year         = {2026}
}
```
