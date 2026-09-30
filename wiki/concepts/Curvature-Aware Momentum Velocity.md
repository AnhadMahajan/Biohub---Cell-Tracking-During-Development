---
type: concept
date: 2026-08-29
title: "Curvature-Aware Momentum Velocity"
tags:
  - concept
  - physics
  - velocity
  - tracking
ai-first: true
---

## For future agent
Curvature-aware momentum velocity extrapolation replaces raw 1-step discrete difference vectors with a 2-step history buffer. This accounts for curved morphogenetic tissue migration during zebrafish embryo development, reducing linear assignment matching errors.

---

## Formulation
$$\vec{v}_{eff}(t) = 0.65 \cdot (\mathbf{x}(t) - \mathbf{x}(t-1)) + 0.35 \cdot (\mathbf{x}(t-1) - \mathbf{x}(t-2))$$
$$\hat{\mathbf{x}}(t+1) = \mathbf{x}(t) + w_{vel} \cdot \vec{v}_{eff}(t)$$
