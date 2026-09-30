---
type: project
date: 2026-08-29
title: "Biohub Cell Tracking During Development"
status: active
tags:
  - project
  - deep-learning
  - cell-tracking
  - kaggle
score: 0.927
target_score: 0.935
ai-first: true
---

## For future agent
This note is the central project record for the Biohub 3D Cell Tracking competition. The system tracks zebrafish embryo cell nuclei across 4D light-sheet microscopy volumes using a dual-seed 3D UNet, Node Transformer edge predictor, bidirectional harmonic probability association, DeepCenter spatial centroid verification, and physics-informed motion relinking. Current public leaderboard score is 0.927 (verified on 2026-08-28).

---

## Key Artifacts & Entry Points
- Production Notebook: [[biohub_cell_tracking_0901_plus_pipeline.ipynb]]
- Experiment History: [[HISTORY.md]]
- Knowledge Graph Report: [[graphify-out/GRAPH_REPORT.md]]
- Interactive Visualization: [[graphify-out/graph.html]]

## Key Models
- [[3D Temporal UNet Cell Detector]] — Centroid heatmap detection
- [[Node Transformer Edge Predictor]] — Candidate pair temporal association
- [[DeepCenter UNet3D Center Prior]] — Auxiliary spatial center prior (`best.pt` Epoch 2)

## Key Concepts & Algorithms
- [[Bidirectional Harmonic Association Fusion]] — Reverse-time consensus ($w=0.30$)
- [[Mitosis Cytokinesis Recovery]] — Divergence & mutual-NN division recovery
- [[Curvature-Aware Momentum Velocity]] — 2-step history motion relinking
- [[Anisotropic Voxel Coordinate Scaling]] — Physical $\mu\text{m}$ space conversion
- [[Hungarian Bipartite Metric Evaluation]] — $7.0\,\mu\text{m}$ local evaluation engine

## Benchmark Progression
- 0.882: Baseline UNet + ILP
- 0.901: Motion relinking + linefit smoothing
- 0.915: Dual-seed ensemble + reverse-time harmonic fusion
- 0.916: Velocity relinking + sister radius expansion (321 divisions)
- 0.927 🏆: DeepCenter Epoch 2 + harmonic fusion (0.30) + divergence division recovery
- 0.930+ 🎯: Momentum velocity + daughter persistence guard
