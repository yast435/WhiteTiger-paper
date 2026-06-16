# Figure and table captions draft

This file tracks the current planned figure and table numbering for the WhiteTiger paper draft.

## Figures

**Figure 1. Overview of the WhiteTiger v2.0 construction and evaluation pipeline.** Multi-source robot demonstrations are stored as HDF5 trajectory records, pass through quality control and schema alignment, are converted into LeRobot v2.1 format, and are then used for GR00T N1.6 pretraining and offline zero-shot open-loop evaluation.

**Figure 2. Embodiment duration distribution of WhiteTiger v2.0.** The figure shows the long-tailed duration distribution across 14 embodiment tags. The largest embodiment subset contributes 44.67% of the total duration, while the top five embodiment tags contribute approximately 78.81%.

**Figure 3. Offline zero-shot open-loop evaluation results.** The figure compares GR00T N1.6/checkpoint-1 and GR00T N1.6/checkpoint-390000 on Joint MSE and ALL MSE across 42 paired task-dataset records from 13 evaluated platforms.

## Tables

**Table 1. WhiteTiger dataset version statistics.** The table compares completed WhiteTiger dataset versions by duration, task count, episode count, and frame count.

**Table 2. Embodiment-level distribution of WhiteTiger v2.0.** The table reports tasks, episodes, frames, hours, and duration percentage for each of the 14 embodiment tags.

**Table 3. Overall offline zero-shot open-loop evaluation results.** The table reports the unweighted mean over 42 paired task-dataset records. Lower MSE is better.

**Table 4. Per-platform offline zero-shot evaluation results.** The table reports the unweighted mean over paired task-dataset records for each evaluated platform.

**Table 5. Task-level offline zero-shot evaluation results.** The table reports Joint MSE, ALL MSE, and relative improvement for each of the 42 paired task-dataset records.

**Table 6. Comparison with representative robot manipulation datasets.** The table compares WhiteTiger v2.0 with RoboMIND, DROID, Open X-Embodiment / RT-X, and BridgeData V2 in terms of real-robot data, embodiment diversity, scale, task coverage, trajectories or episodes, data format, and main focus.
