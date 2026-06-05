# Offline zero-shot evaluation: task-level improvement extremes

The task-level tables below are generated after correcting the reversed `智元G1 / 电源组件安装` row. All paired task-dataset records now show positive improvement in both Joint MSE and ALL MSE.

## Lowest Joint MSE improvement

| Machine | Task | Joint improvement | Joint MSE ckpt-1 | Joint MSE ckpt-390000 | ALL improvement |
|---|---|---:|---:|---:|---:|
| 天机 | 插花 | 95.50% | 0.017314 | 0.000778 | 97.44% |
| 天机 | 叠杯子 | 96.26% | 0.019495 | 0.000730 | 97.37% |
| Franka FR3 | 物品放置_玩具鸭子 | 97.41% | 0.034437 | 0.000892 | 93.30% |
| 天机 | 电池分拣 | 98.03% | 0.016675 | 0.000329 | 98.60% |
| Franka FR3 | 物品整理 | 98.11% | 0.038663 | 0.000731 | 92.08% |

## Highest Joint MSE improvement

| Machine | Task | Joint improvement | Joint MSE ckpt-1 | Joint MSE ckpt-390000 | ALL improvement |
|---|---|---:|---:|---:|---:|
| UR5e | 硬币放入存钱罐_补充 | 99.95% | 0.062034 | 0.000031 | 87.97% |
| UR5e | 语义测试 | 99.94% | 0.062930 | 0.000041 | 94.55% |
| Dwheel | 预-章鱼玩具分类_桌面背景泛化_小鱼方巾 | 99.91% | 0.176082 | 0.000165 | 97.35% |
| 智元G1 | 触摸板组装 | 99.89% | 0.134856 | 0.000146 | 88.62% |
| 青龙 | 用盖子盖住锅 | 99.89% | 0.180345 | 0.000200 | 98.96% |

## Notes

- The lowest-improvement records are not failures; after correction, even the lowest Joint MSE improvement remains above 95%.
- The difference between Joint MSE improvement and ALL MSE improvement suggests that non-joint dimensions such as gripper / hand dimensions still contribute to residual error on some tasks.
