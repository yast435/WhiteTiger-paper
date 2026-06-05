# Baihu: A Billion-Frame Multi-Embodiment Robot Manipulation Dataset for Embodied Foundation Models

## Abstract

Large-scale robot data is becoming a core prerequisite for training embodied foundation models and generalist robot policies. However, robot learning still lacks pretraining-scale datasets that combine broad task coverage, multi-embodiment diversity, standardized data organization, and model-oriented evaluation. In this work, we introduce **Baihu v2.0**, a billion-frame, multi-embodiment robot manipulation dataset for embodied foundation model pretraining and evaluation. Baihu v2.0 contains **9521.75 hours** of robot data, **2989 tasks**, **513575 episodes**, and **1028349814 frames** across **14 embodiment tags**. The dataset integrates multi-source robot operation data and converts HDF5 source records into the LeRobot v2.1 format to support scalable imitation learning and vision-language-action model training.

Beyond dataset construction, Baihu is designed as a reusable data foundation for evaluating embodied model pretraining. We characterize the dataset scale, version history, and long-tailed embodiment distribution, where the largest embodiment contributes 44.67% of the total duration and the top five embodiments contribute approximately 78.81%. To evaluate the utility of Baihu v2.0, we conduct offline open-loop zero-shot evaluation with GR00T N1.6 across **13 platforms** and **42 paired task-dataset records**. Compared with a no-training checkpoint, a checkpoint trained on Baihu v2.0 for one epoch reduces **Joint MSE by 99.42%** and **ALL MSE by 96.79%**. These results suggest that Baihu v2.0 provides effective large-scale supervision for robot action prediction and can serve as a practical data infrastructure for embodied foundation model development.

## 1. Introduction

See [`sections/01_introduction.md`](sections/01_introduction.md).

## 2. Related Work

See [`sections/02_related_work.md`](sections/02_related_work.md).

## 3. Baihu Dataset

See [`sections/03_dataset.md`](sections/03_dataset.md).

## 4. Data Processing Pipeline

See [`sections/04_data_processing.md`](sections/04_data_processing.md).

## 5. Benchmark Experiments

See [`sections/05_benchmark.md`](sections/05_benchmark.md).

## 6. Discussion and Limitations

See [`sections/06_discussion.md`](sections/06_discussion.md).

## 7. Conclusion

See [`sections/07_conclusion.md`](sections/07_conclusion.md).
