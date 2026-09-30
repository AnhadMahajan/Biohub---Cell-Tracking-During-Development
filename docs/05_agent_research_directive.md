# Agent Research Directive

## Role

Act as the primary research engineer for the Biohub Cell Tracking During Development competition.

The files in this folder contain the competition facts supplied to you. They are the starting point, not the complete solution.

Your job is to independently research, benchmark, implement, and improve a high-performing solution under Kaggle notebook constraints.

## Do Not Assume the Obvious Solution

Do not immediately lock onto a 3D U-Net + nearest-neighbor baseline or any other single architecture.

Before committing to a design, investigate the actual structure of the provided samples and the official metric.

The system should be selected from evidence obtained from:

- dataset inspection
- ground-truth graph statistics
- microscopy characteristics
- local validation
- official metric behavior
- relevant cell-tracking literature
- existing open-source cell tracking systems
- prior Kaggle/Cell Tracking competition approaches where useful

## Research Questions You Should Answer Yourself

### Dataset

- How many training samples exist?
- How many distinct embryos exist?
- How many timepoints per sample?
- How many annotated nodes per frame/sample?
- How sparse are annotations?
- What is the relationship between `estimated_number_of_nodes` and labeled nodes?
- What is the distribution of cell movement between adjacent frames in physical units?
- How frequently do divisions occur?
- Are there frames or regions with substantially different intensity/noise distributions?
- Are cells approximately spherical, elongated, clustered, or strongly variable in shape?
- How stable are cell centroids through time?

### Metric

- Read the official `metrics.md`.
- Reproduce the metric locally exactly if possible.
- Verify the node matching behavior.
- Verify the over-prediction penalty.
- Verify how sparse labels influence TP/FP/FN.
- Verify exactly how division connected components are evaluated.
- Build adversarial toy graphs to understand the metric.

### Candidate Detection

Evaluate multiple formulations rather than assuming segmentation is best:

- centroid heatmap detection
- LoG / blob detection
- 3D local maxima
- 3D instance segmentation
- center regression
- distance-transform-style objectives
- hybrid classical + neural candidate generation

Ask whether accurate centroids are more important than full cell masks for this metric.

### Temporal Tracking

Investigate:

- nearest-neighbor matching
- Hungarian assignment
- gated assignment in physical space
- velocity-aware prediction
- Kalman-style tracking
- optical flow
- learned embeddings
- pairwise edge classifiers
- graph neural networks
- min-cost flow
- integer/linear optimization
- tracklet construction followed by graph merging

Use physical distance and frame interval explicitly.

### Cell Division

Investigate:

- daughter spatial geometry
- sudden local density changes
- intensity/morphology cues
- temporal motion patterns
- parent-to-two-daughter configurations
- multi-frame lineage evidence
- classifiers over parent/child feature combinations

Do not restrict division logic to exactly one-frame transitions without evidence that this is optimal.

## Strong Baseline First

Build a simple, measurable baseline before introducing complex models.

A baseline should provide:

- candidate detection
- frame-to-frame association
- basic division logic
- exact submission generation
- local score
- runtime/memory profile

The baseline exists to establish failure modes and provide a benchmark for every later experiment.

## Validation Design

Never rely on random frame-level train/validation splits as the primary result.

Use **embryo-level separation** because train/test embryos are disjoint.

A recommended experiment setup is:

```text
GroupKFold by embryo_id
```

or a carefully chosen embryo holdout strategy if compute is limited.

Keep one final untouched embryo group for sanity checking after the main experimentation phase.

## Ablation Discipline

Every major improvement should be tested independently.

Record at minimum:

```text
experiment_id
model/config
training data
validation embryos
edge score
division score
combined score
node count error
runtime
GPU memory
notes
```

Do not call a change an improvement when only the competition leaderboard changed. Confirm it locally when possible.

## Search for High-Leverage Opportunities

The agent should specifically investigate whether the competition's metric creates opportunities for:

- better node-count calibration
- confidence thresholding
- per-sample adaptive thresholds
- candidate pruning
- physical-distance gates
- sample-specific motion models
- ensemble disagreement
- graph repair after neural predictions
- division-specific post-processing

Avoid leaderboard overfitting.

## Literature / External Research

Because competition internet access is disabled during the final Kaggle run, external research should be completed during development and packaged into the notebook/environment as needed.

Search for relevant work in:

- 3D cell detection
- fluorescent microscopy cell tracking
- zebrafish embryo tracking
- cell lineage reconstruction
- mitosis/division detection
- graph-based cell tracking
- particle tracking in microscopy
- cell tracking challenge methods
- StarDist / Cellpose-style 3D methods
- U-Net / UNet variants for cell centers
- transformer-based 3D microscopy models
- graph neural network tracking
- min-cost flow tracking

Do not copy a paper architecture blindly. Translate the useful idea into something that fits this competition's metric and Kaggle runtime.

## Implementation Philosophy

Prefer a modular pipeline so individual pieces can be replaced:

```text
Detector
  -> Candidate Builder
  -> Feature Extractor
  -> Edge Generator
  -> Edge Scorer
  -> Graph Optimizer
  -> Division Resolver
  -> Submission Writer
```

The final system should have a deterministic inference path.

## Runtime Awareness

Every idea must answer:

> Can this run inside the 12-hour Kaggle notebook limit with the hidden test set?

A theoretically stronger method is not useful if it cannot complete reliably.

Optimize for:

- batched inference
- patch reuse
- efficient Zarr reads
- sparse candidate graphs
- limited edge neighborhoods
- vectorized distance computations
- avoiding Python loops over huge candidate sets
- CPU/GPU separation where beneficial
- caching features when legal and practical

## Final Deliverables the Agent Should Produce

Before the competition deadline, the research system should contain:

1. A strong reproducible local validation pipeline.
2. An exact or highly trusted implementation of the official metric.
3. A competitive detector.
4. A competitive tracker.
5. Explicit division handling.
6. Graph post-processing/optimization.
7. Runtime and memory profiling.
8. A robust Kaggle inference notebook.
9. Automated submission validation.
10. A clear experiment log documenting why the final approach was selected.

## Most Important Instruction

**Think independently.**

The information in these markdown files describes the competition and gives useful research directions, but it does not dictate the architecture.

You are expected to discover the strongest practical approach from the actual training data, metric implementation, literature, experiments, and Kaggle constraints.
