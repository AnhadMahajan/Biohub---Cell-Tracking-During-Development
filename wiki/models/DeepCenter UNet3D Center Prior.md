---
type: model
date: 2026-08-29
title: "DeepCenter UNet3D Center Prior"
tags:
  - model
  - deepcenter
  - postprocessing
ai-first: true
---

## For future agent
DeepCenter is an auxiliary 3D UNet model trained independently for spatial cell center likelihood estimation. Crucially, the Epoch 2 checkpoint (`best.pt`) generalizes smoothly without the extreme over-confidence of late epochs, enabling active division veto gating (`BIOHUB_DEEPCENTER_SAFE_DIV_VETO = 1`) and synthetic gap midpoint confirmation without false rejections.

---

## Role in Pipeline
1. **Synthetic Gap Confirmation**: Confirms cell existence at interpolated 1-frame and 2-frame gap midpoints ($p > 0.25$).
2. **Mitosis Division Veto**: Evaluates candidate daughter coordinates ($p > 0.12$) to reject background speckle bifurcations.
