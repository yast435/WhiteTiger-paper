# 6. Discussion and Limitations

Baihu v2.0 provides a large-scale and standardized data foundation for embodied intelligence research. Its main strength lies in its scale, versioned organization, multi-embodiment coverage, and compatibility with modern robot learning pipelines. With over 1 billion frames and nearly 10 thousand hours of robot data, Baihu can support pretraining regimes that are difficult to study using smaller task-specific datasets.

At the same time, Baihu v2.0 has several limitations that should be addressed in future work.

First, the current dataset source definition needs to be clarified and documented consistently. The internal version document contains slightly different descriptions of the v2.0 data sources, and the final paper should use one verified description.

Second, the distribution of tasks, embodiments, sensors, and action spaces should be analyzed in greater detail. Baihu v2.0 is clearly imbalanced across embodiments, with several high-resource embodiments dominating the dataset duration. This imbalance is not necessarily a weakness, but it must be considered when designing training and evaluation protocols.

Third, benchmark results with representative VLA and imitation-learning models are needed to demonstrate the training value of the dataset. Dataset scale alone is not sufficient; the paper should show that Baihu improves policy learning, open-loop prediction, downstream adaptation, or cross-embodiment generalization.

Fourth, future versions should include clearer train/validation/test splits and standardized evaluation protocols. This is especially important if Baihu is intended to serve not only as an internal training corpus but also as a benchmark for comparing robot foundation models.

Finally, because Baihu integrates multi-source robot operation data, licensing, privacy, and release constraints should be described clearly if any part of the dataset or benchmark is released publicly.
