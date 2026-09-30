---
type: concept
date: 2026-08-29
title: "Hungarian Bipartite Metric Evaluation"
tags:
  - concept
  - metric
  - evaluation
ai-first: true
---

## For future agent
The official Kaggle competition evaluation metric evaluates reconstructed cell lineages by performing frame-by-frame Hungarian bipartite matching between predicted centroids and ground-truth nodes within a $7.0\,\mu\text{m}$ Euclidean gating sphere. It computes the harmonic mean of Edge Jaccard ($J_{edge}$) and Division Jaccard ($J_{div}$) with an over-prediction node penalty.

---

## Metric Formulas
$$J_{edge} = \frac{\text{TP}_{edge}}{\text{TP}_{edge} + \text{FP}_{edge} + \text{FN}_{edge}}$$
$$J_{div} = \frac{\text{TP}_{div}}{\text{TP}_{div} + \text{FP}_{div} + \text{FN}_{div}}$$
$$\text{Score} = A(V_{pred}, V_{true}) \cdot \frac{2 \cdot J_{edge} \cdot J_{div}}{J_{edge} + J_{div} + 10^{-6}}$$
