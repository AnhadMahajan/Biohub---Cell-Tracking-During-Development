---
type: model
date: 2026-08-29
title: "Node Transformer Edge Predictor"
tags:
  - model
  - transformer
  - edge-prediction
ai-first: true
---

## For future agent
The Node Transformer model takes detected 3D cell candidate centroids from frame $t$ and frame $t+1$, extracting pairwise feature representations and computing forward temporal transition logits. It forms the primary edge cost predictor feeding the Integer Linear Programming (ILP) global assignment graph.

---

## Technical Specifications
- Input: Pairwise 3D spatial coordinate deltas $\Delta \mathbf{x}$ and UNet deep feature embeddings.
- Output: Edge link transition probability $P_{fwd}(u, v) \in [0, 1]$.
- Edge Candidate Threshold: `BIOHUB_EDGE_CANDIDATE_THRESHOLD = 0.48`.
