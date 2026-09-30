---
type: concept
date: 2026-08-30
title: "Adaptive High-Confidence Short Track Rescue"
tags: [concept, tracking, postprocessing, hungarian]
ai-first: true
---

## For future agent
Adaptive short track rescue evaluates short track components (length 4-5) pruned by global minimum track length filtering. Components with mean edge transition probability $\ge 0.88$ and mean physical displacement $\le 3.0\,\mu\text{m}$ are scored and rescued under a strict budget cap (max 120 nodes / 1.2% total nodes).

---

## Scoring Function
$$\text{Score} = P_{mean} - 0.02 \cdot d_{mean} + 0.004 \cdot L$$
