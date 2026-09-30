# Biohub Kaggle Execution Constraints

## Submission Mode

The competition requires submissions through **Kaggle Notebooks**.

The notebook must produce:

```text
submission.csv
```

## Runtime Limits

Current competition requirements stated in the prompt:

- CPU notebook runtime: **<= 12 hours**
- GPU notebook runtime: **<= 12 hours**
- Internet access: **disabled during the competition submission run**

External data are allowed when they are freely and publicly available, including pretrained models, but the notebook must be able to run without live internet access.

## Engineering Priorities

The solution must be designed around the actual Kaggle submission environment rather than a workstation-only experiment.

The agent should account for:

- GPU memory
- RAM usage
- Zarr I/O speed
- preprocessing overhead
- model loading time
- inference throughput
- graph construction complexity
- submission generation time
- repeatability

## Data Loading

Do not assume the full 4D dataset can or should be loaded into memory.

The Zarr data are chunked per timepoint, so the default design should support timepoint-wise or patch-wise access.

The agent should profile:

```text
Zarr read time
preprocessing time
model inference time
candidate detection time
pair generation time
graph optimization time
CSV generation time
```

## Kaggle Notebook Structure

A production notebook should be organized into clear stages:

```text
1. environment/imports
2. configuration
3. data discovery
4. Zarr/GEFF inspection
5. local validation split
6. preprocessing utilities
7. detection model / detector
8. candidate extraction
9. temporal candidate generation
10. edge scoring / tracking
11. division inference
12. graph post-processing
13. submission writer
14. validation checks
15. final submission generation
```

Avoid an architecture that requires a slow training phase inside the final submission notebook unless it is proven to fit the runtime budget.

A practical competition system may train offline in development and ship only the inference pipeline, weights, and necessary deterministic preprocessing to the submission notebook where permitted.
