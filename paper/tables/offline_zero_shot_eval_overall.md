# Offline zero-shot evaluation: overall results

Source CSV: `materials/预训练离线实验记录表_测试记录数据总表_0430-同任务机型性能对比.csv`

## Data integrity check

| Item | Value |
|---|---:|
| CSV rows | 84 |
| Checkpoints | 2 |
| Paired task-dataset records | 42 |
| Machines / platforms | 13 |
| Missing checkpoint pairs | 0 |

Note: the user description says 13 machines and 39 unseen tasks, while the CSV currently contains **42 paired task-dataset records** across 13 machines. Please verify whether 3 rows are duplicated, auxiliary tests, or should be excluded from the final paper statistics.

## Overall MSE comparison

The table below reports the unweighted mean across the paired task-dataset records in the CSV.

| Model checkpoint | Joint MSE ↓ | ALL MSE ↓ |
|---|---:|---:|
| checkpoint-1 (no training) | 0.113532 | 0.153642 |
| checkpoint-390000 (Baihu v2.0, 1 epoch) | 0.003893 | 0.008692 |
| Relative improvement | 96.57% | 94.34% |

## Interpretation

After training on BAIHU_v2.0 for one epoch, GR00T N1.6/checkpoint-390000 substantially reduces the offline open-loop prediction error compared with the no-training checkpoint-1 baseline. Using the unweighted task-level mean, Joint MSE decreases from 0.113532 to 0.003893, and ALL MSE decreases from 0.153642 to 0.008692. This corresponds to a 96.57% improvement in Joint MSE and a 94.34% improvement in ALL MSE.
