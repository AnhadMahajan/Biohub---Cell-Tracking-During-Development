---
type: concept
date: 2026-08-29
title: "Mitosis Cytokinesis Recovery"
tags:
  - concept
  - biology
  - mitosis
  - division
ai-first: true
---

## For future agent
Mitosis recovery identifies binary cell division events (1 parent cell splitting into 2 daughter cells at $t+1$). It uses physical search gates ($8.0\mu\text{m} / 11.0\mu\text{m} / 10.0\mu\text{m}$), mutual nearest-neighbor consensus, daughter trajectory persistence ($\ge 2$ consecutive frames), and 3D Euclidean divergence checking ($\Delta d \ge 1.75\mu\text{m}$).

---

## Key Constraints
1. Parent cell must be mid-track (in-degree = 1).
2. Daughters must be mutual nearest unassigned nodes.
3. Daughters must physically move apart in frame $t+2$.
4. Both daughter branches must persist for at least 2 consecutive frames.
