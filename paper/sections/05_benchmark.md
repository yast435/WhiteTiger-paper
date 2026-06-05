# 5. Benchmark Experiments

To demonstrate the value of Baihu for embodied foundation model training, we evaluate whether a model trained on Baihu v2.0 improves offline action prediction on unseen tasks across multiple robot embodiments. The benchmark focuses on open-loop zero-shot evaluation, where the model predicts actions from recorded observations and the predictions are compared against ground-truth actions in held-out trajectories.

## 5.1 Offline open-loop zero-shot evaluation

Open-loop evaluation provides an offline assessment by comparing the model's predicted actions against ground-truth actions from robot demonstration data. In this benchmark, we compare two GR00T N1.6 checkpoints:

| Checkpoint | Description |
|---|---|
| GR00T N1.6 / checkpoint-1 | No-training checkpoint, used as the baseline |
| GR00T N1.6 / checkpoint-390000 | Complete 1-epoch checkpoint trained on BAIHU_v2.0 |

The evaluation covers **13 embodiments / platforms** and **42 paired task-dataset records**. This setting is designed to measure whether Baihu v2.0 pretraining improves cross-task offline prediction performance without additional task-specific finetuning on the evaluation tasks.

## 5.2 Evaluation metrics

The benchmark reports two mean squared error metrics. For each trajectory, the policy predicts an action sequence from the recorded observations. The predicted action values are compared with the corresponding demonstration actions from the dataset. For a selected set of action dimensions and evaluated timesteps, MSE is the average squared difference between the predicted action and the dataset action.

**ALL MSE** computes this error using all action dimensions available in the dataset. This metric reflects the model's full action prediction error, including arm, gripper, hand, or other action dimensions when they are present.

**Joint MSE** computes this error using only the joint-related action dimensions. Gripper or hand dimensions are excluded from this metric. This metric is useful because hand and gripper dimensions may have different scales, semantics, or sparsity patterns across embodiments, and may otherwise dominate or distort the action prediction error.

For each task-dataset record, MSE is averaged over the evaluated trajectory steps and selected action dimensions. Aggregate results are then reported as the unweighted mean across the 42 paired task-dataset records unless otherwise specified.

## 5.3 Overall results

The benchmark results show that one epoch of training on BAIHU_v2.0 substantially reduces offline open-loop prediction error across the 13 evaluated platforms and 42 paired task-dataset records.

| Model checkpoint | Joint MSE ↓ | ALL MSE ↓ |
|---|---:|---:|
| checkpoint-1 (no training) | 0.116746 | 0.157283 |
| checkpoint-390000 (Baihu v2.0, 1 epoch) | 0.000680 | 0.005051 |
| Relative improvement | 99.42% | 96.79% |

Compared with the no-training checkpoint-1 baseline, the Baihu-trained checkpoint reduces Joint MSE by **99.42%** and ALL MSE by **96.79%**. This indicates that Baihu v2.0 provides effective supervision for improving action prediction across a broad set of unseen task-dataset records.

The complete overall result table is maintained in:

```text
paper/tables/offline_zero_shot_eval_overall.md
```

## 5.4 Per-platform results

Because Baihu v2.0 is a multi-embodiment dataset, aggregate metrics alone are insufficient. We therefore also report per-platform results. Across all 13 evaluated platforms, the Baihu-trained checkpoint achieves lower Joint MSE and ALL MSE than the no-training baseline.

The complete per-platform result table is maintained in:

```text
paper/tables/offline_zero_shot_eval_by_machine.md
```

## 5.5 Task-level analysis

Task-level metrics are maintained in:

```text
paper/tables/offline_zero_shot_eval_by_task.md
```

These records can be used to identify tasks with higher residual error after Baihu training and tasks with the largest relative improvement. Task-level reporting is especially useful because different embodiments may have different action spaces, control frequencies, and gripper or hand dimensions.

## 5.6 Figures

Editable HTML/SVG figures for the benchmark are maintained in:

```text
paper/figures/offline_zero_shot_eval_editable.html
```

The recommended paper figures are:

1. Bar chart of overall ALL MSE and Joint MSE for checkpoint-1 vs checkpoint-390000.
2. Per-platform relative improvement plot.
3. Scatter plot comparing ALL MSE and Joint MSE across tasks.
4. Sorted task-level improvement plot for the 42 paired task-dataset records.

## 5.7 Additional benchmark extensions

The current benchmark already provides a useful offline zero-shot comparison. Future versions can extend the benchmark with:

- data-scaling evaluation using different fractions of Baihu v2.0;
- high-resource vs low-resource embodiment evaluation;
- held-out embodiment evaluation;
- closed-loop simulation evaluation;
- real-world rollout success rate.
