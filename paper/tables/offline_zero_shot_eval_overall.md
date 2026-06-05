# Table 3. Overall offline zero-shot open-loop evaluation results

The table reports the unweighted mean over 42 paired task-dataset records from 13 evaluated platforms. Lower MSE is better.

| Model checkpoint | Joint MSE ↓ | ALL MSE ↓ |
|---|---:|---:|
| checkpoint-1 (no training) | 0.116746 | 0.157283 |
| checkpoint-390000 (Baihu v2.0, 1 epoch) | 0.000680 | 0.005051 |
| Relative improvement | 99.42% | 96.79% |

After training on BAIHU_v2.0 for one epoch, GR00T N1.6/checkpoint-390000 substantially reduces the offline open-loop prediction error compared with the no-training checkpoint-1 baseline. Using the unweighted task-level mean, Joint MSE decreases from 0.116746 to 0.000680, and ALL MSE decreases from 0.157283 to 0.005051. This corresponds to a 99.42% improvement in Joint MSE and a 96.79% improvement in ALL MSE.
