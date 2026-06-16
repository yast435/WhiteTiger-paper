# WhiteTiger Paper TODO

## Highest priority

- [ ] Verify the final data-source description for WhiteTiger v2.0.
  - Current ambiguity: internal model-demand data + Ant-delivered data vs. internal model-demand data + Dwheel-filtered data.
- [ ] Add platform descriptions for the 14 embodiment tags.
  - `arx_loong`
  - `astribots1`
  - `cobotmagic`
  - `dualur5e`
  - `dwheel`
  - `fr3`
  - `genie1`
  - `gr2`
  - `lejukuaihu`
  - `qinlongros1`
  - `qinlongros2`
  - `tianji`
  - `xinghaitu_r1`
  - `zhiyuana2`
- [ ] Add formal citations and BibTeX entries.
  - RoboMIND
  - DROID
  - Open X-Embodiment / RT-X
  - BridgeData V2
  - RH20T, AgiBot World, or other datasets if included in the comparison table
- [ ] Decide target paper format.
  - arXiv technical report
  - conference-style LaTeX paper
  - internal white paper

## Completed draft components

- [x] Working title
- [x] Abstract with WhiteTiger v2.0 scale and benchmark result
- [x] Introduction
- [x] Related Work draft
- [x] Dataset Overview draft
- [x] Data Processing Pipeline draft
- [x] Benchmark Experiments draft
- [x] Discussion and Limitations draft
- [x] Conclusion draft
- [x] Dataset comparison table
- [x] Embodiment duration distribution table
- [x] Offline zero-shot evaluation tables
- [x] Editable Figure 1 pipeline HTML/SVG
- [x] Editable benchmark HTML/SVG figures

## Dataset section refinements

- [ ] Confirm all WhiteTiger v2.0 numbers:
  - 9521.75 hours;
  - 2989 tasks;
  - 513575 episodes;
  - 1028349814 frames.
- [ ] Decide whether to keep WhiteTiger v1.1 / v1.2 as a combined row or separate rows.
- [ ] Convert embodiment distribution table into final publication format.
- [ ] Add concise platform descriptions for each embodiment tag.
- [ ] Remove or rewrite any sentence that still sounds like an internal note.

## Figures

- [x] Figure 1: WhiteTiger data construction and evaluation pipeline.
- [x] Figure 2: embodiment duration distribution editable figure.
- [x] Figure 3: benchmark overview / result editable figure.
- [ ] Figure: example trajectory or multi-view observation panel, if screenshots are available.
- [ ] Finalize figure captions.

## Tables

- [x] Table: WhiteTiger version comparison.
- [x] Table: embodiment-level statistics.
- [x] Table: dataset comparison with representative datasets.
- [x] Table: overall offline zero-shot benchmark results.
- [x] Table: per-platform benchmark results.
- [x] Table: per-task benchmark results.
- [ ] Finalize table captions and numbering.

## Experiments

- [x] Run GR00T N1.6 offline open-loop zero-shot evaluation.
- [x] Report global metrics and per-platform metrics.
- [x] Report task-level metrics.
- [ ] Decide whether to add additional baseline models.
  - Candidate: ACT.
  - Candidate: Diffusion Policy.
  - Candidate: GR00T N1.7.
- [ ] Decide whether to run data scale ablation: 10%, 25%, 50%, 100%.
- [ ] Decide whether to add high-resource vs low-resource embodiment evaluation.
- [ ] Add real-world rollout results if available.

## Writing and formatting

- [ ] Add citations in text.
- [ ] Create `references.bib`.
- [ ] Add figure/table references into the section text.
- [ ] Check consistency of terminology:
  - WhiteTiger v2.0 / WhiteTiger_v2.0;
  - LeRobot v2.1;
  - HDF5-to-LeRobot conversion;
  - 13 platforms and 42 paired task-dataset records;
  - GR00T N1.6/checkpoint-1 and checkpoint-390000.
- [ ] Convert Markdown draft to LaTeX when structure stabilizes.
