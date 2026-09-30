---
type: decision
date: 2026-08-29
title: "ADR 002: Momentum Velocity Extrapolation and Mitosis Daughter Persistence"
status: accepted
tags:
  - decision
  - adr
  - tracking
ai-first: true
---

## For future agent
Record of the architectural decision to implement 2-step history momentum velocity extrapolation ($0.65 v_t + 0.35 v_{t-1}$) and mandate $\ge 2$ frame persistence on cytokinesis daughter tracks, targeting the 0.930+ score tier.

---

## Context & Rationale
Instantaneous 1-frame velocity vectors had discrete voxel snapping error on sharp embryo tissue turns. Additionally, false-positive mitosis splits frequently branched into 1-frame noise specks. Requiring daughter persistence directly eliminates spurious divisions.
