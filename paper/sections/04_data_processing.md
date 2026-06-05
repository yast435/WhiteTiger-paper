# 4. Data Processing Pipeline

Baihu is constructed through a multi-stage data processing pipeline. The goal of this pipeline is to convert heterogeneous raw robot data into a consistent and trainable format while preserving embodiment-specific information that is important for policy learning. Because Baihu v2.0 integrates data from multiple robot platforms, the processing pipeline must handle differences in sensor configurations, robot states, action spaces, gripper or hand structures, task metadata, and source-specific recording formats.

At a high level, raw robot trajectories are first collected or integrated from multiple sources. Each trajectory is associated with task information, robot operation records, and embodiment-specific metadata. The data then passes through collection-time monitoring, cloud-side validation, data cleaning, manual review, schema standardization, and LeRobot-compatible conversion. The resulting Baihu v2.0 release provides a versioned dataset that can be used for large-scale imitation learning and vision-language-action model training.

## 4.1 Multi-source raw data ingestion

Raw data is integrated from multiple robot data sources. Each source may have its own directory structure, robot metadata, control frequency, sensor configuration, and action representation. During ingestion, Baihu keeps track of the data source, embodiment tag, task name, episode identity, and available modalities.

The ingestion stage is designed for heterogeneous robot data. Different embodiments may include full-size humanoid robots, humanoid-like wheeled platforms, dual-arm robots, single-arm manipulators, or portable data collection devices. Rather than assuming a single robot morphology, Baihu registers each source under an embodiment tag and preserves the platform-specific state and action schema for later processing.

The final paper should further document the following source-level details for Baihu v2.0:

- raw data formats;
- source directory structure;
- robot-specific metadata;
- original control frequencies;
- camera streams and resolutions;
- language or task annotation source.

## 4.2 Episode validation and filtering

Episode validation is used to remove incomplete, corrupted, or invalid trajectories before model training. A valid episode should contain temporally ordered observations and actions, consistent task metadata, and usable modality fields. Validation is especially important for large-scale robot datasets because a small percentage of invalid episodes can still correspond to many hours of data.

Baihu uses a multi-stage quality control workflow. During data collection, real-time monitoring is used to detect obvious acquisition problems as early as possible. After collection, data is uploaded for more detailed validation and cleaning. The validation stage checks whether the trajectory, modality fields, and metadata are usable for downstream training. After automated or semi-automated checks, professional review is used to further ensure that delivered data satisfies the required collection standard.

The final paper should explicitly describe the validation rules used for Baihu v2.0, including:

- minimum episode length;
- missing frame handling;
- corrupted image handling;
- invalid action or state detection;
- task label validation;
- duplicate episode removal;
- synchronization checks between observations and actions;
- consistency checks between metadata and trajectory files.

## 4.3 Task annotation and metadata standardization

Baihu v2.0 contains thousands of tasks across multiple robot embodiments. To make the dataset usable for policy training and evaluation, task information is standardized into a consistent metadata representation. Each episode is associated with a task label or language instruction, an embodiment tag, an episode identifier, and source-specific metadata when available.

Task metadata standardization makes it possible to analyze the dataset distribution by task, embodiment, and data source. It also supports downstream evaluation settings such as held-out task evaluation, per-platform evaluation, and task-level error analysis. In addition to task names, Baihu can organize trajectories according to higher-level scenario categories, task families, temporal horizons, and atomic skill composition when such annotations are available.

The final paper should clarify the Baihu v2.0 task annotation protocol, including whether task labels are manually written, automatically imported from collection plans, normalized across sources, or mapped to task identifiers.

## 4.4 State and action schema alignment

Because Baihu integrates multiple robot embodiments, state and action representations must be aligned before training. Different platforms may have different numbers of joints, different gripper or hand structures, and different action semantics. Baihu therefore uses embodiment-specific schemas to describe which dimensions correspond to arm joints, grippers, hands, or other controllable components.

This schema alignment is necessary for two reasons. First, it allows each embodiment to preserve its native control structure rather than being forced into an overly simplified universal action vector. Second, it enables evaluation metrics such as Joint MSE and ALL MSE to select the appropriate action dimensions for each platform.

The final paper should further specify:

- action space definitions by embodiment;
- state space definitions by embodiment;
- dimension ordering for joints, grippers, hands, and other effectors;
- treatment of missing fields;
- clipping or validity checks applied during conversion.

## 4.5 Gripper and hand dimension handling

For each robot embodiment, gripper and hand data are stored according to the actual physical travel range of that platform. In other words, Baihu does not assume a single universal gripper or hand range across all robots. A parallel-jaw gripper, a dexterous hand, and a robot-specific end-effector may have different mechanical limits and different action semantics, so their values are preserved according to the corresponding embodiment's real motion range.

This design avoids forcing heterogeneous end-effectors into an artificial shared scale before dataset construction. It also makes the dataset more faithful to the original robot control space. During evaluation, Joint MSE can be computed using only joint-related dimensions, while ALL MSE includes the full action vector, including gripper or hand dimensions when they are present. This distinction is important because gripper and hand dimensions may have different numerical ranges, sparsity patterns, and task relevance across embodiments.

The final paper should add an embodiment-level table describing the gripper or hand representation for each platform, including:

- whether the platform uses a parallel gripper, dexterous hand, or other end-effector;
- the number of gripper or hand action dimensions;
- the physical travel range used in the stored data;
- whether the value represents position, width, joint angle, or another control variable;
- how the dimension is included or excluded in Joint MSE and ALL MSE.

## 4.6 LeRobot v2.1 conversion and toolchain

After validation and schema alignment, data is converted into the LeRobot v2.1-compatible format. Each episode is represented as a temporally ordered trajectory of observation-action pairs, with associated task and metadata fields. This format allows the dataset to be loaded by robot learning pipelines and reused across different model training experiments.

The Baihu toolchain also supports data inspection and format conversion. Dataset visualization tools are used to inspect numerical values, joint trajectories, image streams, and video content. A Python-based conversion toolchain supports conversion between the Baihu data representation and the LeRobot-compatible representation used for model training. This toolchain helps reduce the gap between dataset construction and downstream training.

The final paper should describe the generated files, metadata schema, and loading process, including:

- LeRobot version;
- episode file layout;
- task metadata schema;
- dataset information schema;
- dataset statistics schema;
- video storage format;
- train/validation/test split files.

## 4.7 Version management and quality control

Baihu is maintained as a versioned dataset. Version management is important because large-scale robot datasets often require iterative fixes to action mappings, gripper or hand representations, task annotations, and metadata. By tracking dataset versions, Baihu makes it possible to reproduce training runs and understand which data corrections or additions are included in each release.

The quality control process can be summarized as a collection-to-delivery chain: data acquisition, real-time quality monitoring, cloud upload, data validation, data cleaning, manual review, and final dataset delivery. This process is designed to prevent low-quality trajectories from entering the training corpus and to maintain consistent quality across heterogeneous sources.

Earlier Baihu releases corrected issues such as effector mapping range inconsistencies and gripper value reconstruction. These examples show why quality control is essential for robot foundation model pretraining: small errors in action dimensions or end-effector values can affect many downstream model updates. Baihu v2.0 is therefore treated as a completed and stable dataset version for the experiments in this paper.
