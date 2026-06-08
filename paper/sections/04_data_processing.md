# 4. Data Processing Pipeline

Baihu v2.0 converts heterogeneous robot operation records into a consistent dataset for large-scale policy training. The source trajectories are stored in HDF5 format and are standardized into LeRobot v2.1 for downstream imitation-learning and vision-language-action training. The processing pipeline emphasizes three requirements: preserving embodiment-specific control information, filtering invalid trajectories, and producing a unified representation that can be loaded by modern robot learning systems.

## 4.1 HDF5 ingestion and trajectory validation

Raw Baihu v2.0 trajectories are first organized as HDF5 records. Each record is associated with an embodiment tag, task identity, episode identity, and available modality fields. Depending on the source platform and task setup, a trajectory may include visual observations, robot proprioceptive states, robot actions, task annotations, timestamps, and episode-level metadata.

Because Baihu integrates data from multiple robot platforms, validation is required before data enters the released training corpus. The pipeline checks whether each episode contains temporally ordered observations and actions, usable modality fields, consistent task metadata, valid embodiment information, and aligned observation-action sequences. This validation step removes incomplete, corrupted, or unusable trajectories that would otherwise provide incorrect supervision during model training.

## 4.2 Quality control and metadata standardization

Baihu uses a multi-stage quality control workflow covering data acquisition, monitoring, upload, validation, cleaning, review, and final delivery. This workflow is important at the scale of Baihu v2.0 because even a small fraction of invalid trajectories can correspond to a large amount of unusable data. Quality control therefore aims to maintain consistency across heterogeneous sources while preserving the diversity needed for embodied model pretraining.

Task and episode metadata are standardized to support dataset analysis and model conditioning. Each episode is associated with task information, embodiment metadata, and source-specific fields when available. These annotations enable analysis by task, embodiment, and data source, and they also provide the language or semantic conditioning signals used by vision-language-action policies.

## 4.3 State and action schema alignment

State and action representations differ across robot embodiments. Platforms may have different joint layouts, gripper or hand structures, control frequencies, and action semantics. Baihu therefore preserves embodiment-specific schemas that define which dimensions correspond to arm joints, grippers, hands, or other controllable components.

This schema alignment is important for both training and evaluation. It allows each platform to retain its native control representation while enabling unified dataset loading. It also supports metric definitions such as Joint MSE and ALL MSE: Joint MSE uses only joint-related action dimensions, while ALL MSE uses the full action vector for each embodiment.

For grippers and hands, values are stored according to the actual physical travel range of each platform. Baihu does not impose a single universal gripper or hand scale across robots. This design keeps the stored action values faithful to the original robot control space and avoids forcing heterogeneous end-effectors into an artificial shared range.

## 4.4 HDF5-to-LeRobot v2.1 conversion and versioning

After validation and schema alignment, Baihu v2.0 is converted from the source HDF5 format into LeRobot v2.1. This conversion separates the source storage format from the model-training format: HDF5 provides the raw trajectory container, while LeRobot v2.1 provides the standardized dataset interface used by downstream training and evaluation pipelines.

Baihu is maintained as a versioned dataset. Versioning makes it possible to track data additions, data cleaning, action mapping updates, and embodiment-specific fixes over time. This paper focuses on Baihu v2.0 because it is a completed and stable dataset version with fixed statistics and an associated offline benchmark.
