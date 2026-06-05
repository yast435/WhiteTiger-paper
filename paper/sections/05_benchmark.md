# 5. Benchmark Design

To demonstrate the value of Baihu for embodied foundation model training, we propose the following benchmark protocol.

## 5.1 Open-loop action prediction

Open-loop evaluation measures whether a trained policy can predict ground-truth robot actions from recorded observations and language/task inputs. This evaluation can be performed on held-out Baihu validation episodes. Metrics may include mean absolute error, mean squared error, per-dimension action error, gripper prediction accuracy, and action smoothness.

Because Baihu v2.0 is a multi-embodiment dataset with a long-tailed distribution, open-loop metrics should be reported both globally and per embodiment. A global average alone may be dominated by high-resource embodiments such as genie1 and may hide poor performance on low-resource embodiments.

## 5.2 Data scaling

A key question for large-scale robot datasets is how model performance changes with data scale. Baihu enables data scaling experiments by training the same model on different fractions of the dataset, such as 10%, 25%, 50%, and 100%. This analysis can reveal whether additional robot data continues to improve action prediction and policy generalization.

## 5.3 Task generalization

Task generalization evaluates whether a model trained on Baihu can perform well on held-out tasks, held-out objects, or held-out scenarios. Depending on the task annotation granularity, the dataset can be split by task category, object type, data source, or robot embodiment.

Potential evaluation settings include:

- in-distribution validation episodes;
- held-out tasks;
- held-out embodiments;
- high-resource-to-low-resource transfer;
- source-specific evaluation.

## 5.4 Downstream policy evaluation

For robot foundation models, offline metrics alone are insufficient. Therefore, Baihu should also support downstream policy evaluation through real-world rollout or simulated evaluation. A policy pretrained on Baihu can be fine-tuned on a target robot and evaluated by task success rate, completion time, failure mode, and robustness to object pose variation.

## 5.5 Suggested baseline models

The following models can be considered as baselines depending on implementation availability:

- GR00T / GR00T N1.7;
- ACT;
- Diffusion Policy;
- OpenVLA-style policies;
- other internal VLA or imitation-learning policies.

## 5.6 Results placeholder

The final paper should include at least one benchmark result table.

| Model | Training data | Evaluation split | Metric 1 | Metric 2 | Notes |
|---|---|---|---:|---:|---|
| [TBD] | Baihu v2 subset/full | validation | [TBD] | [TBD] | [TBD] |

Recommended result tables:

1. overall open-loop evaluation;
2. per-embodiment evaluation;
3. data scale ablation;
4. high-resource vs low-resource embodiment comparison;
5. downstream rollout success rate, if available.
