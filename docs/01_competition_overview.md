# Biohub - Cell Tracking During Development

## Mission

Build a robust algorithm that detects zebrafish embryo cells in 3D microscopy over time, links the same cells across time, identifies cell divisions, and reconstructs a tracking/lineage graph.

This is a **3D + time cell detection and graph tracking problem**, not a conventional 2D object-detection competition.

## Competition

- Platform: Kaggle
- Competition: **Biohub - Cell Tracking During Development**
- Host: Biohub
- Competition type: Research Code Competition / Notebook-only submission
- Prize pool: **$60,000**
- 1st: $18,000
- 2nd: $12,000
- 3rd: $8,000
- 4th: $6,000
- 5th: $6,000
- 6th: $5,000
- 7th: $5,000

## Timeline

- Start: June 29, 2026
- Entry/team-merger deadline: September 22, 2026
- Final submission deadline: September 29, 2026
- Deadlines are 11:59 PM UTC unless Kaggle/organizers change them.

## Main Task

For each sample, produce:

1. Cell detections (nodes) at each timepoint.
2. Temporal links between cells (edges).
3. Correct branching structure for cell divisions.

The final output is a directed tracking graph across time.

## Important Competition Characteristics

- Data are real 3D fluorescence microscopy volumes of zebrafish embryo cells.
- Cell populations can be dense.
- Cells can look very similar.
- Imaging has noise and difficult structures.
- Cells move and change shape.
- Cells divide, making the graph non-bijective across time.
- Ground truth is **sparse**, so the absence of a label does not necessarily mean the absence of a cell.
- Train and test are **embryo-disjoint**. The first segment of the folder name identifies embryo identity.

## Strategic Interpretation

Do not assume that a detector with a strong per-frame score will produce a strong competition score. The official metric emphasizes temporal edges and division structure. Detection quality matters because bad nodes create bad links, but graph construction and division handling are first-class objectives.

The research process should therefore consider the complete pipeline:

```text
raw 4D microscopy
    -> preprocessing / intensity normalization
    -> cell candidate detection
    -> centroid refinement
    -> candidate generation across adjacent frames
    -> edge scoring / temporal association
    -> division detection
    -> graph optimization / biological constraints
    -> submission.csv
```

## Source / Citation

Competition citation:

Thibaut Goldsborough, Jordão Bragantini, Xiang Zhao, Gordon Leary, Teun Huijben, Ilan da Silva Theodoro, Kyle Harrington, Chi-Li Chiu, Walter Reade, María Cruz, and Loïc A. Royer. **Biohub - Cell Tracking During Development**. Kaggle, 2026.
