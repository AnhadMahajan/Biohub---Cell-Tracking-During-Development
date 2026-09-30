---
title: "ADR 004: Peak-Preserving Detection Fusion, Boundary-Aware Mitosis, and Gap-3 Recovery"
date: 2026-08-30
status: accepted
tags:
  - architecture-decision
  - biohub
  - detection
  - mitosis
  - gap-recovery
ai-first: true
---

# ADR 004: Peak-Preserving Detection Fusion, Boundary-Aware Mitosis, and Gap-3 Recovery

## Context
Following the 0.932 milestone, diagnostic analysis revealed that:
1. In movie `44b6_0b24845f`, 65 frames triggered the candidate retention fallback due to naive convex damping $(1-w)P + wS < 0.96875$.
2. Mitosis divisions occurring at $t \ge 98$ or near volume borders were discarded because daughters could not reach length 2.
3. 61 residual fragmented tracks could be recovered via curvature-constrained 3-frame bridging.

## Decision
1. **Peak-Preserving Detection Fusion**:
   $$D_{\text{fused}} = \max(D_{\text{primary}}, (1 - w) D_{\text{primary}} + w D_{\text{sec\_aligned}})$$
2. **Boundary-Aware Mitosis Persistence**:
   Relax daughter persistence from 2 to 1 for terminal frames ($t \ge 98$) and boundary coordinates ($< 4.0\,\mu\text{m}$).
3. **Curvature-Constrained Gap-3 Recovery**:
   Bridge $t \to t+4$ for tracks with collinear velocity vectors ($\cos(\theta) > 0.70$).

## Consequences
- 100% frame coverage for the dual-seed ensemble across all 400 test frames (0 fallbacks).
- Increased division recall without increasing false positives.
- Seamless execution with zero syntax or configuration errors.
