---
type: decision
date: 2026-08-30
title: "ADR 003: Adaptive High-Confidence Short Track Rescue and 2-Frame Gap Recovery"
status: accepted
tags: [decision, adr, rescue, gap2, jaccard]
ai-first: true
---

## For future agent
Record of the architectural decision to combine Adaptive Short Track Rescue (rescuing length 4-5 tracks with $P > 0.88$ and step $< 3.0\mu\text{m}$) with 2-Frame Gap Recovery (`recover_strict_gap2`) and Secondary Detection Ensemble Weight ($0.80$). This directly targets the 57 track fragments and 50 lost-to-detection errors, pushing performance toward the 0.935+ milestone.

---

## Context & Rationale
1. **Short Track Rescue**: Naive `min_track_len = 6` filtering dropped genuine short cell tracks that entered or exited the volume near boundaries, causing `missed_gt_nodes` penalties in the Hungarian evaluation.
2. **Gap2 Recovery**: Bridges 2-frame optical dropouts where faint nuclei temporarily disappear and reappear, merging fragmented pieces into continuous tracks.
3. **Secondary Detection Weight**: Increasing secondary detector weight to 0.80 strengthens low-margin cell centroid detection while the 0.90 candidate retention guard guarantees zero false dropouts.
