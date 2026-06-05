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
