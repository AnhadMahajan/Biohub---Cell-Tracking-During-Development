# ADR 005: Calibrated 0.933 Champion Architecture and Anti-Overfitting Safeguards

**Date**: 2026-08-31  
**Status**: Accepted / Verified (Public LB: **0.933** 🏆, Local Proxy: **0.9360**)  
**Context**: Competition Leaderboard Optimization for Biohub Cell Tracking During Development

## Context and Problem Statement
When attempting aggressive detection pooling (pointwise max-pooling) and relaxed boundary-aware daughter track thresholds (reducing required track length to 1), the leaderboard score regressed sharply from 0.932 down to 0.922. Detailed error analysis revealed:
1. Division false-positive count surged from 286 to 379 (+32.5% FP surge).
2. Spurious node additions inflated submission rows to 245,635 (+7,145 rows vs baseline).
3. Edge Jaccard and Division Jaccard precision dropped due to background noise nodes and conflicting long-range Gap-3 bridges.

## Decision
We systematically restore, benchmark, and deploy the calibrated **0.933 Champion Architecture** with strict safeguards:
1. **Calibrated Detector Confidence Threshold**: Fixed at `0.965` (eliminates low-confidence background candidates).
2. **Native ILP Division & Disappearance Penalties**: `ilp_division_weight = 1.2` and `ilp_disappearance_weight = 2.0` to guide the integer linear program toward clean, high-confidence linear tracks with biological forks.
3. **Harmonic Mutual-Support Association Fusion**: $P_{\text{harmonic}} = \frac{2 P_{\text{fwd}} P_{\text{bwd}}}{P_{\text{fwd}} + P_{\text{bwd}} + 10^{-6}}$ with reverse-time weight $w_{\text{bwd}} = 0.30$, producing **0 wrong associations** on validation.
4. **Conservative Mitosis Geometry Constraints**: `SAFE_DIV_MAX_UM = 7.0um`, `SAFE_DIV_SISTER_MAX_UM = 12.0um` with mutual nearest-neighbor pairing and strict spatial divergence ($2.25\mu\text{m}$).
5. **Strict Gap-2 Recovery & Adaptive Short-Track Rescue**: Restricting synthetic gap interpolation to $\Delta t = 2$ to avoid ambiguous multi-frame trajectory warping.

## Validation Results
- **Kaggle Public Leaderboard**: **0.933**
- **Local Ground-Truth Validation Proxy**: **0.9360**
- **Submission Volume**: 234,226 rows (119,150 nodes, 115,076 edges)
- **Mitosis Precision**: 307 division parents across test set (81, 65, 8, 153).
- **Association Accuracy**: 0 wrong association errors across 4 validation movies.
