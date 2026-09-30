# Biohub Evaluation and Metric

## Metric Overview

The competition uses a combined tracking metric based on:

1. **Edge Jaccard**
2. **Division Jaccard**

These are the primary objectives of the solution.

The agent should read the official `metrics.md` implementation/code before optimizing anything. Do not infer edge/division scoring only from the competition description.

Official metric reference:

https://github.com/royerlab/kaggle-cell-tracking-competition/blob/main/metrics.md

## Edge Jaccard

Predicted nodes are matched to ground-truth nodes independently at each timepoint using optimal bipartite assignment on scaled centroid distance.

The maximum matching distance is:

```text
7.0 micrometers
```

The coordinate scaling is based on the physical voxel size:

```text
z = 1.625 um/voxel
y = 0.40625 um/voxel
x = 0.40625 um/voxel
```

A predicted edge is counted as a true positive when both predicted endpoints match ground-truth nodes and those corresponding ground-truth nodes are connected by a ground-truth edge.

The metric combines:

```text
TP / (TP + FP + FN)
```

with an adjustment/penalty related to over-predicting the total number of nodes.

### Practical implication

False-positive nodes and unnecessary edges can actively hurt the score. Increasing the number of detections is not automatically beneficial.

## Division Jaccard

A predicted division is represented by a node with two or more outgoing edges.

For each ground-truth division, the predicted graph is checked for a connected component that:

- covers the pre-split stage, and
- reaches both daughter lineages.

Division TP/FP/FN are then calculated and combined into a micro-averaged Jaccard.

### Practical implication

Division handling cannot be reduced to simply checking whether a parent has exactly two children in the next frame. The metric evaluates lineage connectivity around the split.

## Score Anomaly

Because of the details of the metric and its node-count adjustment, the competition states that scores can exceed 1.0.

The agent should use the **official metric implementation as the source of truth**, not a re-created approximation, for local validation.

## What to Optimize

The solution should explicitly evaluate these failure modes separately:

```text
1. missed detections
2. duplicate detections
3. centroid localization error
4. incorrect temporal links
5. missing temporal links
6. identity switches
7. false divisions
8. missed divisions
9. broken daughter lineage connectivity
10. excessive node counts
```

A useful experiment matrix should measure how each component changes both edge and division metrics.
