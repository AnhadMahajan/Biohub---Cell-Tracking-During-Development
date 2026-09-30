---
type: concept
date: 2026-08-29
title: "Anisotropic Voxel Coordinate Scaling"
tags:
  - concept
  - microscopy
  - geometry
ai-first: true
---

## For future agent
The light-sheet fluorescence microscope recordings have non-isotropic voxel spacing: axial resolution ($Z$) is lower than lateral resolution ($X, Y$). All physical Euclidean distance metrics and velocity relinking must scale voxel indices to physical microns ($\mu\text{m}$).

---

## Physical Scaling Factors
- $\Delta Z = 1.625\,\mu\text{m}$
- $\Delta Y = 0.40625\,\mu\text{m}$
- $\Delta X = 0.40625\,\mu\text{m}$
