# Offline zero-shot evaluation: per-machine results

The table reports the unweighted mean across paired task-dataset records for each machine/platform. The `智元G1 / 电源组件安装` row has been corrected after confirming that checkpoint-1 and checkpoint-390000 MSE values were originally reversed.

| Machine | N tasks | Joint MSE checkpoint-1 ↓ | Joint MSE checkpoint-390000 ↓ | Joint improvement | ALL MSE checkpoint-1 ↓ | ALL MSE checkpoint-390000 ↓ | ALL improvement |
|---|---:|---:|---:|---:|---:|---:|---:|
| 青龙 | 6 | 0.157132 | 0.000572 | 99.64% | 0.180542 | 0.002733 | 98.49% |
| 智元A2 | 3 | 0.132757 | 0.000896 | 99.33% | 0.225461 | 0.007562 | 96.65% |
| 傅利叶GR-2 | 3 | 0.137993 | 0.001805 | 98.69% | 0.233863 | 0.003824 | 98.36% |
| 星海图R1 | 3 | 0.075369 | 0.000197 | 99.74% | 0.117581 | 0.004864 | 95.86% |
| 乐聚KUAVO | 3 | 0.133456 | 0.000743 | 99.44% | 0.157261 | 0.004335 | 97.24% |
| 松灵Aloha | 3 | 0.133968 | 0.000949 | 99.29% | 0.156535 | 0.002307 | 98.53% |
| Franka FR3 | 3 | 0.041619 | 0.000711 | 98.29% | 0.077804 | 0.004525 | 94.18% |
| 星尘智能S1 | 3 | 0.135078 | 0.000822 | 99.39% | 0.158673 | 0.005819 | 96.33% |
| UR5e | 3 | 0.062389 | 0.000068 | 99.89% | 0.083854 | 0.007897 | 90.58% |
| 方舟无限arx-acone | 3 | 0.138172 | 0.001076 | 99.22% | 0.161330 | 0.001803 | 98.88% |
| 天机 | 3 | 0.017828 | 0.000612 | 96.57% | 0.096915 | 0.002092 | 97.84% |
| Dwheel | 3 | 0.176382 | 0.000208 | 99.88% | 0.203833 | 0.005157 | 97.47% |
| 智元G1 | 3 | 0.135167 | 0.000282 | 99.79% | 0.167769 | 0.015057 | 91.02% |

## Notes

- Improvement is computed as `(checkpoint-1 MSE - checkpoint-390000 MSE) / checkpoint-1 MSE`.
- After correcting the reversed `智元G1 / 电源组件安装` row, all machines show positive improvement in both Joint MSE and ALL MSE.
