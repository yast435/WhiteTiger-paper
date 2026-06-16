# Offline zero-shot evaluation summary

Source material: `materials/预训练离线实验记录表_测试记录数据总表_0430-同任务机型性能对比.xlsx`

This table records the current benchmark setting described by the source material. Numerical results should be extracted from the Excel file and filled into follow-up tables.

| Item | Description |
|---|---|
| Training dataset | WhiteTiger_v2.0 |
| Base model / no-training checkpoint | GR00T N1.6 / checkpoint-1 |
| WhiteTiger-trained checkpoint | GR00T N1.6 / checkpoint-390000 |
| Training status | checkpoint-390000 is the complete 1-epoch checkpoint |
| Evaluation type | Offline open-loop zero-shot evaluation |
| Evaluation scope | 13 embodiments / platforms, 39 unseen tasks |
| Comparison setting | Same-task, same-platform performance comparison between checkpoint-1 and checkpoint-390000 |
| Metric 1 | ALL MSE: computed using all action dimensions in the dataset |
| Metric 2 | Joint MSE: computed using only joint dimensions, excluding gripper / hand dimensions |
| Evaluation implementation | `gr00t/batch_open_loop_eval.py` one-click batch evaluation in the GR00T N1.6 open-loop evaluation repository |
| Result file | Source Excel table uploaded under `materials/` |

## Result tables to extract next

Recommended paper tables:

1. Overall comparison between checkpoint-1 and checkpoint-390000.
2. Per-embodiment comparison across 13 platforms.
3. Per-task comparison across 39 unseen tasks.
4. ALL MSE vs joint-only MSE comparison.
5. Relative improvement table: `(checkpoint-1 MSE - checkpoint-390000 MSE) / checkpoint-1 MSE`.
