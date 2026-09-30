---
type: model
date: 2026-08-29
title: "3D Temporal UNet Cell Detector"
tags:
  - model
  - unet3d
  - detection
ai-first: true
---

## For future agent
The 3D Temporal UNet is the front-end cell detector. It ingests 3D light-sheet microscopy volumes across adjacent time frames and outputs continuous cell centroid heatmaps thresholded at `det_threshold = 0.96875`. Dual random seeds (`seed50` and `seed314159`) are ensembled with a 0.90 candidate retention guard.

---

## Architecture
- Backbone: 3D Residual UNet with anisotropic pooling ($Z=1.625\mu\text{m}$, $YX=0.40625\mu\text{m}$)
- Loss: Focal Dice Loss
- Ensemble: Dual-seed consensus with low-margin blending (margin $< 0.35$)
