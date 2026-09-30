# Biohub Dataset and File Format

## Sample Structure

Each sample is a short 3D+time zebrafish microscopy movie stored as a **Zarr v3** directory.

Each image sample contains one array at:

```text
0/
```

with shape:

```text
(T, Z, Y, X)
```

A typical sample is approximately:

```text
T = 100
Z = 64
Y = 256
X = 256
```

and the image dtype is usually:

```text
uint16
```

### Chunking

Chunks are one timepoint at a time:

```text
(1, 64, 256, 256)
```

The chunk for timepoint `t` is located under a path similar to:

```text
0/c/{t}/0/0/0
```

Array metadata is stored in:

```text
0/zarr.json
```

Compression uses Blosc/Zstandard.

## Physical Voxel Scale

Use physical spacing when calculating distances or motion:

```text
z = 1.625 micrometers / voxel
y = 0.40625 micrometers / voxel
x = 0.40625 micrometers / voxel
```

This matters because one voxel in Z is much larger physically than one voxel in X/Y.

Any distance-based tracker should think in **physical coordinates**, not raw voxel coordinates alone.

For a displacement `(dz, dy, dx)` in voxel units, physical displacement is approximately:

```text
dz_um = dz * 1.625
dy_um = dy * 0.40625
dx_um = dx * 0.40625
```

## Ground Truth

Training samples have a paired `.geff` directory.

`.geff` is a graph exchange format built using Zarr v3.

Important arrays include:

```text
nodes/ids
nodes/props/t/values
nodes/props/z/values
nodes/props/y/values
nodes/props/x/values
edges/ids
```

The edge array has shape:

```text
(N, 2)
```

with columns:

```text
source_id, target_id
```

## Sparse Ground Truth

Ground-truth annotations are **sparse**. Not every true cell in every frame necessarily has a label.

The `.geff` metadata contains an:

```text
estimated_number_of_nodes
```

field that provides an estimate of the total true cell count for the sample.

This means the algorithm should not blindly treat annotated nodes as a complete segmentation of all cells.

The agent should investigate exactly how the official metric handles the sparse annotations before choosing detection thresholds or penalties.

## Embryo Identity

Folder names follow a pattern like:

```text
{embryo_id}_{field_of_view}
```

Example:

```text
44b6_0049_0438_1330_1273
```

The first segment identifies the embryo.

### Critical validation rule

Train and test sets are **embryo-disjoint**.

Validation should therefore be split by **embryo identity**, not by random frames, if the goal is to estimate hidden-test generalization.

## Test Data

Training:

```text
train/
```

contains `.zarr` plus paired `.geff` annotations.

Example test data:

```text
test/
```

contains image volumes only.

For notebook submissions, Kaggle swaps in a hidden test set at execution time. The hidden test set is approximately similar in size to the training dataset.

## Submission Format

`submission.csv` must contain these columns:

```text
id,dataset,row_type,node_id,t,z,y,x,source_id,target_id
```

### Node row

Example:

```text
0,44b6,node,1,0,32,128,128,-1,-1
```

Fields:

- `row_type = node`
- `node_id` = unique node identifier within the submitted graph
- `t,z,y,x` = integer centroid coordinates in voxel coordinates
- `source_id = -1`
- `target_id = -1`

### Edge row

Example:

```text
2,6bba,edge,-1,-1,-1,-1,-1,1,2
```

Fields:

- `row_type = edge`
- `source_id` references a source node
- `target_id` references a target node
- `node_id,t,z,y,x = -1`

### Submission Requirements

- `id` is a consecutive integer index.
- Every test dataset must appear.
- `dataset` must exactly match the hidden-test folder/sample identifier without `.zarr`.
- `submission.csv` must be the final file name used for notebook submission.
