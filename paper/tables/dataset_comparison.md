# Table 6. Comparison with representative robot manipulation datasets

This table compares WhiteTiger v2.0 with representative large-scale robot manipulation datasets and benchmarks. Public dataset numbers are listed according to the cited papers.

| Dataset | Multi-embodiment | Scale | Tasks / Skills |
|---|---|---:|---:|
| RoboMIND \cite{wu2024robomind} | Yes | 4 embodiments; hours not reported in the abstract | 479 tasks, 96 object classes |
| DROID \cite{khazatsky2024droid} | Limited / same robot setup across many sites | 350 hours, 564 scenes | 84 tasks |
| Open X-Embodiment / RT-X \cite{oneill2023openx} | Yes | 22 robots from 21 institutions | 527 skills / 160,266 tasks |
| BridgeData V2 \cite{walke2023bridgedatav2} | Limited / one low-cost robot platform | 24 environments | Multi-task manipulation behaviors |
| **WhiteTiger v2.0** | **Yes** | **9521.75 hours, 1.03B frames, 14 embodiment tags** | **2989 tasks** |

Table 6 highlights WhiteTiger's distinguishing emphasis on billion-frame scale, multi-embodiment coverage, versioned data management, HDF5-to-LeRobot v2.1 standardization, and direct offline evaluation with GR00T N1.6 checkpoints.
