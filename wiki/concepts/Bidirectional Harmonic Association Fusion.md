---
type: concept
date: 2026-08-29
title: "Bidirectional Harmonic Association Fusion"
tags:
  - concept
  - algorithm
  - association
ai-first: true
---

## For future agent
Harmonic association fusion combines forward association probabilities $P_{fwd}(u, v)$ from $t \to t+1$ with reverse association probabilities $P_{bwd}(v, u)$ from $t+1 \to t$. The harmonic mean consensus with primary weight $0.30$ guarantees that only edge pairs with bidirectional support receive high transition scores, eliminating false cross-track jumping.

---

## Formula
$$P_{harmonic}(u, v) = \frac{2 \cdot P_{fwd}(u, v) \cdot P_{bwd}(v, u)}{P_{fwd}(u, v) + P_{bwd}(v, u) + 10^{-6}}$$
